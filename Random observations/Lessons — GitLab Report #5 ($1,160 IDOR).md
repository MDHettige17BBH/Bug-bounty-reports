> Rule: Impact is measured in real human damage, not technical cleverness.
  
- Including affected versions and tiers in Impact section adds professional context
- Full raw HTTP request in steps = gold standard proof for API/GraphQL bugs
- Mid-step explanations are acceptable when the bug is complex — clarity beats rigid format
- Asking about bounty timing once professionally is acceptable
- Respect 30-day disclosure waiting period — builds long term reputation
- Incremental IDs = higher severity because attacker can enumerate ALL objects, not just one
  
  curl = command line tool for sending HTTP requests directly without a browser. Same as Burp Repeater but in terminal. Including curl commands in steps is excellent proof — any triager can copy-paste and reproduce instantly.

  ## Exceptions Hunters Can Make on Complex Bugs
  Acceptable only when the bug genuinely requires it:

- Mid-step explanations — when a step needs context to be reproducible
- Longer summary — when the vulnerability chain is complex
- Multiple HTTP requests in steps — when chaining is required
- Explaining what to look for in the response — when proof isn't visually obvious
- Noting affected versions/scope

