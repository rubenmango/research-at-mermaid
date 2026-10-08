# Persona 04 — The Knowledge Consumer

## Verdict
Blind spot

## Would I start the trial after this session?
No — I never saw search, Shared with you, team spaces, or one diagram authored by someone else, so there's no evidence this product does the job I came for.

## Three words I'd use to describe the product after this run
Untitled, unattributed, unsearched

## Brand voice read
The site talks like a product with its act together — *"You already think in systems. Now you can diagram them just as fast."* — and the pane's signup variant (O-11) promises *"Save and share diagrams"* and *"Real-time collaboration"*. The app then hands me *"Untitled diagram"*, a footer reading *"0 Items"*, and a paywall selling *"Collaborate with comments and sharing"* as the reason to leave the plan where sharing already works. Marketing speaks to a team; the app speaks to one person with three files.

## Reactions

### D-06 · Dashboard
- **My read:** Delight
- **Severity for me (1-5):** 4
- **In character:** First thing in three runs built for someone who didn't make the file — *"Email Password Flow You created a month ago"* vs *"Data to Deployment Pipeline Created a year ago"*; that "You"/no-"You" is exactly the authorship signal I need.
- **JTBD hit:** judge a candidate by metadata without opening it
- **What would fix it:** nothing needed — name the 4 icon-only card buttons too

### D-06 (search control) · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** A control named *"Search diagrams presentations and folders"* sits right there and nobody has typed a query into it in three runs. My default action is FIND, not CREATE, and the only surface that finds is unmeasured.
- **JTBD hit:** find what exists without asking in Slack
- **What would fix it:** Run 04 types two queries — one title, one body-text — and records what comes back

### AI-07 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Three of four files are called "Untitled diagram". Landing cold, I can't tell the real one from the two empties, and I'm not opening every box to find out.
- **JTBD hit:** understand a diagram without the author
- **What would fix it:** title the file from the generation's own summary line, not on apply

### E-01 · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Third run and the invite link still defaults to **"Can edit"**. I want to link to the original, not fork it — here "link" means handing anyone edit rights on someone else's diagram. That's the fragmentation I'm trying to avoid.
- **JTBD hit:** cite or share the original without breaking it
- **What would fix it:** default the copied link to view-only

### D-09 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Two empty files from a month ago, same name, same "Empty diagram" thumbnail. To a newcomer that reads as abandoned work.
- **JTBD hit:** tell whether what I found is canonical
- **What would fix it:** don't create a file until it has content; flag empties on the card

### AI-09 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** A response from sixteen days ago now reads *"Unable to render diagram / There's a syntax error in the Mermaid code"*. If past work rots into error cards, nothing older than a sprint is worth reading.
- **JTBD hit:** rely on work done before I arrived
- **What would fix it:** persist the rendered artifact, not just the code string

### D-11 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** *"Recently shared"* is new and it's the right section name for me — then it holds one year-old file. Recent files? I don't have any. I'm new.
- **JTBD hit:** ramp by seeing what the team touches
- **What would fix it:** make it team-scoped and recency-ordered

### AI-08 · AI
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Not my button — but it's why nothing I'd want to find gets written. "Edit" kept `flowchart TD` and discarded every node and edge of a correct ~1,400-character diagram. The contributors upstream of me produce nothing I can consume.
- **JTBD hit:** none directly — upstream of all of them
- **What would fix it:** write the full generated code to the panel on apply

### D-04 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** The paywall sells *"Collaborate with comments and sharing"* while Share works on Basic and `/pricing`'s grid ticks "Co-editing & external sharing" for Basic. I can't tell if a link I paste into a spec will work for whoever I send it to.
- **JTBD hit:** share it forward
- **What would fix it:** make paywall, grid and app agree on what Basic shares

## What I never saw but needed to
- **Search** — never exercised in three runs, though the control is named in the sidebar.
- **Shared with you**, **Team spaces**, **Create Organization** — named, never opened, three runs running.
- **Any diagram authored by someone else** — every file is the tester's own, so owner metadata is untested.
- Folders, tags, descriptions, hover metadata — no evidence in the log they exist.
- Comments, version history, whether forking keeps a link to the original — not covered.
- A true cold first-run after signup: what a new teammate actually lands on.
- **Export** — the control is now visible (E-05) but was not exercised, so I don't know if it yields an embeddable link.

## One thing I'd tell the team
Run 04 is worthless to me unless someone seeds this account with a diagram I didn't write and types one query into that search box — until then you're testing authoring and calling it a product.
