# Persona 04 — The Knowledge Consumer
## Verdict
Blind spot

## Would I start the trial after this session?
No — nothing in this run showed me a search bar, a team space, or a "shared with you" list, so I have no evidence this product helps me find work I didn't author.

## Three words I'd use to describe the product after this run
Authored-for, unsearchable, anonymous

## Brand voice read
Half of it sounds like one product. "There's more to Mermaid than Basic" is warmer than Run 01's discount framing — but "Trial Plusfree for 7 days" survived a full rewrite of that dialog, so nobody proofed the revenue screen twice running. The bigger tell is the upsell speaking four ways at once: "Try Plus free" · "Start your free trial" · "Start Free Trial" · "Start my trial" — four names for one button, and no owner of the whole surface.

## Reactions

### D-11 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** "Recent files? I don't have any. I'm new." A file grid with no last-opened, no owner, no recent activity, and Favorites as an empty explainer box gives me nothing to ramp on.
- **JTBD hit:** Find what already exists via navigation, not Slack
- **What would fix it:** Put last-opened, owner and updated-date on the dashboard before another upsell pixel.

### D-06 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Every file card being an unnamed button wrapping an unnamed link means the catalogue has no names in it at all. I browse by title; there are no titles to browse.
- **JTBD hit:** Judge candidates by metadata without opening each one
- **What would fix it:** Give each card an accessible name of its title, owner and updated date.

### A-01 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** I'm the person who joins via my company's SSO. 84 successes against 4,333 failures, flat across two runs, no owner — and the "improvement" is just an outlier week rolling out of the window. That's my front door.
- **JTBD hit:** Get into the team workspace at all
- **What would fix it:** Assign an owner this week to prove whether `User SSO Login Failed` is real or over-firing.

### AI-07 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Two of three files are "Untitled diagram". I don't trust a diagram with no metadata, and I'm not asking three people which untitled one is canonical.
- **JTBD hit:** Tell what a diagram is about without reading every box
- **What would fix it:** Auto-title on generation, not on apply.

### D-09 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** Two empty "Untitled diagram" files sitting there for fourteen days is exactly how a team catalogue rots — indistinguishable from real work in the grid.
- **JTBD hit:** "Is this the right one?"
- **What would fix it:** Flag or expire empty diagrams instead of counting them as content.

### E-01 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "Who has access → ruben Mangorrinha" is the only ownership signal anywhere, buried in a modal. And the invite link still defaults to "Can edit" — I want to link to the original, not be handed write access to someone else's canonical diagram.
- **JTBD hit:** Share or cite it forward without breaking the original
- **What would fix it:** Default the copied link to view, and surface the owner on the card.

### D-04 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "Collaborate with comments and sharing" is sold as a Plus benefit while Share demonstrably works on Basic this run. And losing "I'll keep my limits" for a bare × with no name is a regression — a modal with two unnamed buttons is a trap.
- **JTBD hit:** none directly; it damages trust in every other claim
- **What would fix it:** Drop the collaboration bullet, restore the labelled decline, name both buttons.

### AI-03 · AI
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** An author spends two credits, gets two correct diagrams, and ends on a blank canvas called "Untitled diagram" — that empty shell is what lands in my catalogue. Their bug becomes my dead end.
- **JTBD hit:** Understand a diagram someone else made
- **What would fix it:** Apply generations to the canvas by default.

### O-10 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Opening an existing empty diagram and getting a dotted canvas and "Describe your idea" is the opposite of what I want — it asks me to author when I came to read.
- **JTBD hit:** Default action is to FIND, not CREATE
- **What would fix it:** On an empty diagram, offer a browse or template route, not only a prompt bar.

### D-02 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Three files on screen, "3 of 3 personal" in the chip, "0 Items" in the footer. Two runs of the same wrong number makes me distrust every count I'm shown.
- **JTBD hit:** Browse and trust the inventory
- **What would fix it:** Bind the footer counter to the same source as the quota chip.

## What I never saw but needed to
- **Search.** No query was run against diagram titles or content in either run. My first move is always search; it has never been tested.
- **Team spaces / Create Organization / Shared with you.** A "Collapse Team spaces" sidebar control is the entire evidence I have that they exist.
- **Any browse surface** — folders, tags, filters. "All files", "Grid view", "List view" are named buttons; nothing shows what they render.
- **Owner, last-updated or description on a diagram** — never observed on a card or in the editor.
- **Fork / duplicate**, and whether a copy keeps any relationship to the original.
- **Comments as a reading aid** — the rail exists, contents unverified.
- The evidence states plainly that my world is "not covered in either run."

## One thing I'd tell the team
Two consecutive runs have tested the authoring half of a product whose stated Q2 primary persona never authors — seed the pulse its own team account with someone else's diagrams in it, or Run 03 is the third blind spot in a row.
