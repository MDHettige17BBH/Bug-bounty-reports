1. Give clear reproduction steps from the start
2. Quantify impact—explain what *should* be hidden vs what leaked
3. Look for secondary data exposure (photos + domains here)
4. Emphasize attack complexity (how easy to exploit?)
5. Know your scope—don't accept "out of scope" without questioning

**Mistakes:**

1. Weak initial report (forced clarification)
2. Didn't emphasize domain disclosure enough
3. Accepted first severity rating (was upgraded later)
4. Didn't push back on bounty decision firmly

**What to Hunt For:**

1. Sequential/predictable IDs—increment them
2. Secondary data leaks in responses
3. Test resource access in different states (draft, published, deleted)
4. Check permission boundaries across user roles
5. Direct S3/CDN links—can you enumerate them?
