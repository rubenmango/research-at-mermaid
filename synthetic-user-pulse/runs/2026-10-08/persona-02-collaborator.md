# Persona 02 — The Collaborator
## Verdict
Blind spot
## Would I start the trial after this session?
No — I'd be paying to unlock "comments and sharing" that the product already gave me on Basic, while the one button that puts a diagram in front of my team deletes it.
## Three words I'd use to describe the product after this run
Confident, unfinished, unsharable
## Brand voice read
The website talks like a team tool and the app doesn't. *"Edit together, in real time"* and *"Diagram where you work"* (W-14) are exactly my pitch to Priya, and the signup variant that lists *"Real-time collaboration"* (O-11, pane variant) doubles down. Then the paywall sells me *"Collaborate with comments and sharing"* (D-04) for a thing Basic already does, `/pricing` puts a checkmark on *"Co-editing & external sharing"* for Basic, and the editor hands me a share dialog titled `Share “Untitled diagram“` with a mismatched quote mark. One product is marketing; a different, quieter one is shipping.

## Reactions

### E-01 · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Third run and "Copy link" still defaults to **"Can edit"** — I paste that into the RFC thread and now twelve people can rewrite the architecture diagram. Credit where due: it rendered populated instead of five seconds of "Loading sharing information…".
- **JTBD hit:** Permissions that match what I'd expect from a Google Doc
- **What would fix it:** Default the invite link to "Can view" and make the permission control the first thing in the dialog, not the last.

### AI-08 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** A complete, correct ~1,400-character flowchart, and "Edit" leaves `flowchart TD` and nothing else — every node and edge gone, canvas empty, still "Untitled diagram". There is nothing to send Priya. There isn't even a screenshot to send, and I hate screenshots.
- **JTBD hit:** Get to a shared, agreed diagram at all
- **What would fix it:** Apply the full response atomically and refuse to clear the code panel if the write isn't complete.

### AI-09 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Run 02's 429 branch now renders as *"Unable to render diagram / There's a syntax error in the Mermaid code"*. That's the opposite of a source of truth — it's a doc that rots in the drawer. If I'd linked that to the team sixteen days ago, today it's an error card with my name on it.
- **JTBD hit:** The diagram becomes the canonical reference, not a snapshot
- **What would fix it:** Store the generated code, not a re-render, so a past response is always recoverable.

### AI-03 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** The output lives in a chat card and the canvas stays blank. Good news that the history survived sixteen days — but there's **no labelled control** that tells me the conversation exists. My teammate opening this file sees a blank dotted grid and assumes I did nothing.
- **JTBD hit:** Two or more people working on the same artifact
- **What would fix it:** A named "AI history" control in the editor chrome.

### D-04 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** "Collaborate with comments and sharing" is the line aimed at me, and it's false twice over — Share works on Basic right now (E-01) and the compare grid checkmarks "Co-editing & external sharing" for Basic. I don't buy from a company whose pricing page and paywall disagree. Also, three runs of **"Trial Plusfree for 7 days"** on the revenue screen.
- **JTBD hit:** none — this is trust
- **What would fix it:** Make the paywall list what Basic actually lacks, with a price.

### W-13 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** The row I'd point my team at — "Co-editing & external sharing" — is four silent checkmarks to a screen reader. Someone on my team reads these pages with a reader, and the collaboration row is the one that goes unspoken.
- **JTBD hit:** Getting the team to agree on the tool
- **What would fix it:** `aria-label="Included"` / `"Not included"` on every compare cell.

### W-06 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Card says "3 Diagrams", grid says "Up to 6", app says "3 of 3 personal". Third run. I can't tell a VP what a seat costs us if the page can't count.
- **JTBD hit:** Sign-off
- **What would fix it:** One number, sourced from the same config as the quota chip.

### D-09 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Two empty "Untitled diagram" files are eating 2 of 3 slots and nothing flags them. Roll that out to a team trial and half of them lock themselves out before anyone drafts anything real.
- **JTBD hit:** Getting teammates in
- **What would fix it:** Don't count never-saved empties against the cap, and offer to clear them at the paywall.

### D-05 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** Five strings for one trial — "Try Plus free", "Start your free trial", "Start Free Trial", "Start my trial", and now "Upgrade to Plus" on every card, two of them on screen at once. Regressed. Looks like four teams shipping past each other.
- **JTBD hit:** none
- **What would fix it:** One string, one component.

### D-06 · Dashboard
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** 4 of 58 unnamed, down from "every file card". "Email Password Flow You created a month ago" is a real name a real teammate can land on. This is the fix I'd actually mention in the thread.
- **JTBD hit:** Teammates navigating a shared space
- **What would fix it:** nothing needed — finish the 4 icon buttons.

### D-11 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** A "Recently shared" section appeared, which is the right instinct — but there's no "who changed what, when". "Wait, did Tom edit this?" has no answer anywhere in this product.
- **JTBD hit:** See what changed and who changed it
- **What would fix it:** Per-file last-edited-by on the card.

### E-05 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** Export is finally visible in the header and nobody pressed it. That's my handoff path to the PR and I still don't know if it gives me a live link or a dead PNG.
- **JTBD hit:** Port it into the surface where the decision is made
- **What would fix it:** Exercise it next run and say whether there's an embed/live-link option.

## What I never saw but needed to
- **Team spaces, Create Organization, Shared with you, search** — not covered in any of the three runs. My entire persona has had nothing to react to, three times running.
- **Comments** — never exercised. There is still no evidence a comment affordance exists.
- **Version history and code round-trip** (E-03) — not exercised. "Where's the history" has gone unanswered for three runs.
- **Export formats and any embed story** for Confluence / Notion / GitHub — unexercised (E-05).
- **Whether a share-link recipient must sign up** — untested. That's my stated deal-breaker and the pulse can't tell me.
- Mermaid Flow, presentations, voice chat (D-12), and a true cold first-run — all unmapped.

## One thing I'd tell the team
You rewrote the homepage to promise "Edit together, in real time" and you still haven't tested a single collaboration surface in three runs — fix AI-08 so there's something to share, flip the invite link to view-only, then finally point the pulse at team spaces.
