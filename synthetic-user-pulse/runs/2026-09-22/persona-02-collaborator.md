# Persona 02 — The Collaborator
## Verdict
Blind spot
## Would I start the trial after this session?
No — I spent two AI credits, got two correct diagrams, and ended on a blank canvas called "Untitled diagram"; there's nothing here I could send Priya, so there's nothing to pay for.
## Three words I'd use to describe the product after this run
Unshippable, unlabelled, unowned
## Brand voice read
It doesn't sound like one product. The paywall sells "Collaborate with comments and sharing" as a Plus benefit while Share opened fine on Basic in the same session. The trial is "Try Plus free", "Start your free trial", "Start Free Trial" and "Start my trial" depending on which chrome I'm in, and one still reads "Trial Plusfree for 7 days." after a full rewrite. The editor's own voice — "Describe your idea" — is solo and personal, and nothing in the copy acknowledges I have a team.

## Reactions

### AI-03 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** The generation lived in a chat preview behind an "Edit" button, the canvas stayed blank, and closing the panel left no control anywhere to reopen the chat. I can't share a diagram that doesn't exist.
- **JTBD hit:** Get to a shared, agreed diagram and port it into the doc/PR.
- **What would fix it:** Apply the first generation to the canvas by default and keep a persistent AI-chat entry point in the editor header.

### E-01 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "Invite people", "Use invite link", "Public link access: No access" — the Google Doc shape I expect. But the invite link still defaults to **"Can edit"**, so the link I paste in Slack hands edit rights to whoever forwards it.
- **JTBD hit:** Permissions that match a Google Doc; two teammates editing without trampling.
- **What would fix it:** Default the invite link to "Can view" and make edit an explicit upgrade.

### D-04 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** It sells "Collaborate with comments and sharing" as Plus while I just used Share on Basic. If the upsell lies about what I have, I don't trust it on what I'd get. Losing "I'll keep my limits" for an unnamed × — on a modal whose primary button is also unnamed — is a regression I'd have caught in review.
- **JTBD hit:** none directly, but it poisons the buying decision.
- **What would fix it:** Drop the sharing claim, restore a labelled decline, and name both buttons.

### A-01 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** ~1,000 SSO failures a week against about five successes a day, flat across two runs, nobody's ticket. My company logs in by SSO. Broken, or an over-firing event hiding a real outage — either way I can't roll this to my team.
- **JTBD hit:** Loop in two to five other people.
- **What would fix it:** Give it an owner this week to separate over-firing from real failures, and ask what happened on 2026-08-10.

### D-06 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Every file card is an unnamed button wrapping an unnamed link. People on my team use screen readers; this can't be the source of truth when the file list is four anonymous controls. Credit where due — the sidebar did get names.
- **JTBD hit:** Everyone reads the same thing the same way.
- **What would fix it:** Give each file card the diagram's title as its accessible name.

### AI-07 · AI
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "Untitled diagram" is unsendable. Dropped into a PR nobody knows what it is — and the dashboard already shows two of them.
- **JTBD hit:** The diagram becomes the canonical reference, not a snapshot.
- **What would fix it:** Title from the first generation's content, not on apply.

### D-11 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Two weeks away and the dashboard tells me nothing — no last-opened, no recent activity, no "Tom edited this". The only returning-user state is my cap and an upsell. For a team tool, who-changed-what-when *is* the product.
- **JTBD hit:** See what changed and who changed it.
- **What would fix it:** Surface recent activity and last editor on the dashboard and on each file card.

### D-09 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Two of my three slots are empty "Untitled diagram" files from a fortnight ago, and the cap counts them. What's blocking me drafting an RFC for Tom is two accidents, unflagged.
- **JTBD hit:** none — but it's why I couldn't start.
- **What would fix it:** Flag empty diagrams with a one-click delete, or don't count them against the cap.

### AI-01 · AI
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Twenty-five seconds and a visible "Repairing diagram… / The Mermaid code contains a syntax error." is not what I want on screen with four people watching my share. Visible repair beats silent retry, but "syntax error" is the wrong word in a product that promises I never touch syntax.
- **JTBD hit:** Team alignment in a live session.
- **What would fix it:** Say "Tidying up the diagram…" and emit valid Mermaid first time.

## What I never saw but needed to
- **Version history** — the control exists in the editor header but was never exercised (E-03). "Did Tom edit this? When?" is unanswered after two runs.
- **Comments** — the rail exists but was never opened; in-place commenting on a specific node is my top ask and it's unverified.
- **Embedding into Confluence / Notion / GitHub**, and export formats — not covered in either run.
- **Team spaces, Create Organization, Shared with you, search** — not covered in either run. My entire world, twice.
- **Concurrent editing** — no merge or conflict story observed.
- **"New presentation"** (D-10) — undocumented on /pricing; unknown whether it counts against the cap.

## One thing I'd tell the team
The AI now produces correct diagrams that never reach the canvas, never get a name, and therefore can never be shared — fix the apply path before you sell me collaboration.
