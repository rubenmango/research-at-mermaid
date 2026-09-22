# Persona 06 — The Knowledge Contributor

## Verdict
Blind spot

## Would I start the trial after this session?
No — I spent two credits, got two correct diagrams, and ended on a blank canvas with no way back to either.

## Three words I'd use to describe the product after this run
Unfinished, unreusable, lossy

## Brand voice read
Half one product. The paywall says "There's more to Mermaid than Basic", then "Trial Plusfree for 7 days" — same missing space as two weeks ago, on the screen asking for money. One action, four names: "Try Plus free", "Start your free trial", "Start Free Trial", "Start my trial". The AI box says "Describe your idea" on canvas and "What would you like to change or add?" in chat. Confident marketing voice, careless product voice.

## Reactions

### O-08 · Onboarding
- **My read:** Delight
- **Severity for me (1-5):** 2
- **In character:** The bare "Redux" dropdown is gone; the bar is now just "+" and a submit arrow. That was the one thing making me think theming was half-wired into the prompt box.
- **JTBD hit:** Make it look on-brand without re-deciding every time
- **What would fix it:** Nothing needed — now give me a named theme picker that saves.

### AI-03 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** The generation only ever appeared as a preview card behind an "Edit" button; the canvas stayed blank and closing the chat left me with no control anywhere to reopen it. If the output never lands, there is nothing to style, theme, or share.
- **JTBD hit:** All of them — there is no artifact to work on
- **What would fix it:** Apply the first generation to the canvas, and put a persistent AI chat control in the editor header.

### AI-01 · AI
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** 25–28 seconds, and mid-wait it shows "Repairing diagram… / The Mermaid code contains a syntax error." I hand-write Mermaid so I know what that means — but this product tells my teammates they never touch syntax.
- **JTBD hit:** Do it fast enough that I don't dread the next one
- **What would fix it:** Repair silently, or say "Tidying up" — never surface "syntax error" to a generating user.

### AI-07 · AI
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Two generations later it is still "Untitled diagram". I publish into Confluence and Slack; an untitled file is unfindable in three weeks.
- **JTBD hit:** Make something my team can consume
- **What would fix it:** Title from the first prompt at generation time, not on apply.

### E-03 · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Export, version history and the code round-trip went untested again. That is my entire evaluation — does the PNG match the editor, can I roll back a wrecked layout, can I paste my style-guide code back in. Two runs, zero answers.
- **JTBD hit:** Export/embed cleanly; keep the look consistent
- **What would fix it:** Make export fidelity and version history mandatory in Run 03, even if it costs a diagram slot.

### E-02 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** The zoom/fit cluster lost its names and moved to the bottom right, while the same functions stay labelled inside the chat preview. Two components, two sets of rules — same disease as everything else here.
- **JTBD hit:** Don't make me re-tidy by hand
- **What would fix it:** Share one labelled control component between canvas and chat preview.

### D-09 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Two of my three slots are "Untitled diagram" with an "Empty diagram" thumbnail from a fortnight ago, and nothing tells me to clear them. So I can't make a second diagram to see whether any style carries over — the one test I came to run.
- **JTBD hit:** Reuse styling on the next diagram
- **What would fix it:** Don't count empty diagrams against the cap, or flag them with one-click delete.

### D-04 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "Collaborate with comments and sharing" is sold as a Plus benefit while Share demonstrably works on my Basic account, and the labelled exit "I'll keep my limits" is now a bare ×. If the pitch is wrong about a feature I already have, I don't trust it about themes.
- **JTBD hit:** Share it with the team
- **What would fix it:** Cut the false benefit line, state a price, restore a labelled decline.

### AI-04 · AI
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** I submitted an edit, my own message didn't echo, nothing moved for forty seconds — but credits had already gone 14 → 13. Charged before shown any work is a bad look on a metered feature.
- **JTBD hit:** Iterate without dreading it
- **What would fix it:** Echo the message and spin immediately; decrement credits on completion.

### O-10 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Opening an existing empty diagram drops me on a dotted canvas with one bar saying "Describe your idea" — no templates, no diagram types, no paste-Mermaid toggle. Templates are where reusable style would live, and this path hides them.
- **JTBD hit:** Start from something that already looks like our diagrams
- **What would fix it:** Show Templates & Diagram types on any empty canvas, new or returning.

## What I never saw but needed to
- **Export in any format** — PNG/SVG fidelity vs the editor, untested across both runs.
- **Version history** — the control sits in the editor header; nobody clicked it.
- **Code panel round-trip** — my real workaround (paste styled Mermaid back in) unverified.
- **Templates gallery / "Templates & Diagram types"** — blocked by the 3/3 cap.
- **Any theme save or reuse affordance** — no evidence item covers whether one exists at all.
- **Whether a second diagram inherits anything from the first** — impossible at cap.
- **Node-level AI, comments, Mermaid Flow, and the undocumented "New presentation" (D-10)** — uncovered.

## One thing I'd tell the team
Fix AI-03 first — until generated output actually lands on the canvas, nothing about style, reuse or export is even testable.
