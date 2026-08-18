---
program: Omnicell (HackerOne VDP)
platform: HackerOne
category: authentication
cwe: CWE-306 — Missing Authentication for Critical Function
date_found: 2026-08-09
status: submitted — closed as duplicate
report: https://hackerone.com/reports/3928161
---

# Omnicell — Missing Authentication on `/api/Common/Audit`
my first ever program ever to hunt btw,

## Scope and setup

Open-scope VDP (`.omnicell.com`, `.myomnicell.com`, `.enlivenhealth.co`),
brand new program (launched Jul 2026), response efficiency 98%.
Passive-only recon: `subfinder` + `assetfinder` across all three root
domains, deduped, then one `httpx` pass to find live hosts.

Full recon closed out cleanly before this finding — several other leads
were tested and ruled out first: `340b.myomnicell.com` (three separate
auth hypotheses, all failed), `au.preprod`/`au.integration.omnicell.com`
(CMS marketing pages, not the real app — the actual login lives on
`suremed.cloud`, out of scope), a DNS-leaked internal IP (informational
at best, explicitly excluded by the program's own scope rules), and a
CORS misconfiguration on `akhq3.stage` (also explicitly excluded —
"minor CORS misconfiguration without data exposure").

## Finding

Pulled the JS bundle on `340b.myomnicell.com` and read through it for
every internal API endpoint the frontend actually calls. Found
`/api/Common/Audit` — a POST endpoint meant to log user actions for
compliance (`app.audit(coveredEntityId, action, data)` per the JS).

Tested it with zero authentication — no session cookie, no login — and
it returned `200 OK`, accepting the request as if from a logged-in user.

Compared against sibling endpoints in the same namespace (`GetUser`,
`SendSignalRNotificationsForUser`), which correctly rejected
unauthenticated requests with `401`. One endpoint in the group had no
auth check while its neighbors did — that inconsistency is the bug.

## Root cause

The app is ASP.NET MVC / Web API (`X-AspNetMvc-Version: 5.2` in
response headers). Auth enforcement in this framework runs through the
`[Authorize]` attribute above a controller method:

```csharp
[Authorize]
public class CommonController : ApiController
{
    public IHttpActionResult GetUser(string id) { ... }
}
```

If `[Authorize]` is missing on one specific method — dropped during a
refactor, never added when the endpoint was written, excluded at the
action level despite being present at the class level — that one method
runs with zero auth check while its siblings stay protected. Mechanically,
this is almost certainly a single missing line of code on one method,
not a clever bypass.

**Why CWE-306 and not 862 or 284:** the request carried zero
authentication material and still succeeded — no auth check exists at
all on this function. CWE-862 (Missing Authorization) would apply if a
valid low-privilege session reached an admin-only action instead.
CWE-284 (Improper Access Control) is the umbrella both fall under.

## Impact

- **Audit log poisoning / integrity loss** — any unauthenticated party
  can inject arbitrary, attacker-controlled entries
  (`coveredEntityId`, `action`, `data` — all attacker-supplied,
  unvalidated) into a compliance-relevant audit trail. For a regulated
  340B drug-pricing program, audit integrity has compliance significance.
- **Fabricated attribution** — since `coveredEntityId` is
  attacker-controlled, entries could be injected that falsely appear
  tied to a real covered entity ID. Noted as logical, not confirmed —
  couldn't view the actual log to verify.
- Security property violated is **integrity**, not confidentiality —
  nothing was disclosed, so `VC:N` / `VI:L` on the CVSS vector reflects
  that directly, not a guess.

## The pattern to generalize

When one endpoint in a group is protected, test its siblings — don't
assume consistency. Frameworks make uniform protection easy
(`[Authorize]` at the class level covers every method inside), but
developers break that uniformity by hand constantly — endpoints added
later, refactors, copy-pasted controllers where the attribute didn't
carry over. High-yield, low-effort, because it exploits *human
inconsistency in applying a control*, not a flaw in the control itself.

## Outcome

Closed as duplicate — someone found and reported the same endpoint
first. First real submission either way; the methodology held up even
though the finding didn't land. See
[lessons-learned](../../lessons-learned/) for the post-mortem on timing
and what changes going forward.
