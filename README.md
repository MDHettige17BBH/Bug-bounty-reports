# Disclosed-reports-analysis

**17-year-old bug bounty hunter from Sri Lanka.** No mentor. No bootcamp.
Just public reports, PortSwigger labs, and 12-hour days.

This repo is how I learn: I take disclosed reports, reverse-engineer the
methodology, and write how *I* would have found each bug. Not summaries.
Not copy-paste. My own recon, my own testing steps, my own mistakes.

This is training. Real triage data is the closest thing to ground truth for pattern recognition.

**Focus areas:** Access Control | Authentication | Business Logic | SSRF

**Progress** Reports analyzed: 18 / 100+ (2026 target)

| Report | Title | Vulnerability |
| --- | --- | --- |
| 318751c | Access to Private Photos of Apps in App section | IDOR |
| 120121c | Delete any group of any organization remotely | Critical IDOR |
| 642886c | Reauthentication for changing password bypass | Authentication bypass |
| 3219944 | Scheduled data leak to other accounts By "projectID" | IDOR |

## Contact

- **HackerOne:** malikdishan17
- **X/Twitter:** @MalikDisha8108
- **Location:** Colombo, Sri Lanka
- **Availability:** 24/7, full-time bug bounty hunter

#### Pre-report **research**

1.  Read the disclosed report (5 min)
2. Open the program site and find features - Google the vulnerable feature name
3. Look at public screenshots
4. Read the program's security page or API docs
5. What does a developer do there? (upload apps, add photos, set pricing)
6. Where in that flow would a photo ID appear in a URL?

Create a free account (only when necessary), click around, look at URLs. 10 minutes of clicking teaches you more than reading about the program site for an hour.

Open to collaboration, mentorship, and program invitations.
