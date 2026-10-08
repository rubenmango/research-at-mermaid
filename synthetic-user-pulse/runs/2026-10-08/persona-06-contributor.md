# Persona 06 — The Knowledge Contributor
## Verdict
Blind spot
## Would I start the trial after this session?
No — I never got a diagram onto a canvas, let alone styled one, and nothing in this run showed me I could save a look and reuse it next time.
## Three words I'd use to describe the product after this run
Faster, confident, lossy
## Brand voice read
The website now talks like a tool I'd want: *"You already think in systems. Now you can diagram them just as fast."* and *"Stay in your flow"* (W-14) are exactly my pitch to my team. Then the app says *"Describe your idea"* (O-10, AI-05), *"Untitled diagram"* (AI-07), *"Trial Plusfree for 7 days"* (D-04) and *"Hit Fix with AI (⌘⇧F) to automatically correct syntax errors"* (E-04) — about code the product itself truncated. One of the signup variants even sells *"Repair broken Mermaid code"* as a benefit (O-11, container variant: "Create account" / "Work email"). The marketing voice is a product I don't have yet.

## Reactions

### AI-08 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** A correct 1,400-character flowchart, and the Edit button kept `flowchart TD` and threw away every node and edge. That's not a bug I work around, that's me never trusting the apply button again.
- **JTBD hit:** make an accurate diagram fast; stop dreading the next one
- **What would fix it:** Write the AI's full code into the editor buffer atomically and refuse to commit a parse that loses nodes.

### AI-03 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** The output lives in a chat card with an Edit button and the canvas stays blank — so AI isn't authoring for me, it's drafting into a drawer. Good news that the chat history survived 16 days, but there's no labelled way back into it.
- **JTBD hit:** AI does the first draft, I style it
- **What would fix it:** Render generations straight to the canvas with an undo, not behind an apply step.

### AI-09 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Run 02's edit now reads *"Unable to render diagram / There's a syntax error in the Mermaid code"*. I paid a credit for that. Work I generated doesn't just sit there, it rots.
- **JTBD hit:** reuse — I go back to old diagrams constantly
- **What would fix it:** Persist generated code as text, not as a re-rendered card that can go stale.

### AI-04 · AI
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** I burned a generation and the editor showed me no counter anywhere. I budget AI across a week of diagrams; invisible spend means I ration by fear.
- **JTBD hit:** do this quickly enough not to dread the next one
- **What would fix it:** Put remaining credits next to the prompt bar, decrementing after the apply, not before.

### AI-01 / AI-10 · AI
- **My read:** Delight
- **Severity for me (1-5):** 2
- **In character:** Under ten seconds, valid on first emission, no repair pass — that's the speed I want, and the Follow-ups block (*"Can you split user actions and backend actions into swimlanes?"*) is genuinely how I'd iterate. Both are wasted while AI-08 stands.
- **JTBD hit:** fast first draft
- **What would fix it:** nothing needed — fix AI-08 so these pay off.

### E-04 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** Blank dotted grid, a "Flowchart" badge, no error state — and "Fix with AI" is in the DOM at zero pixels. The product knew it was broken and showed me a clean canvas.
- **JTBD hit:** know when my output is shippable
- **What would fix it:** Show the parse error on the canvas with a visible Fix with AI button.

### E-01 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Dialog populating instantly is a real improvement, but the invite link still defaults to **"Can edit"** for a third run — I share diagrams into PRs and Slack and I'm handing out edit rights by default. Also the title renders `Share “Untitled diagram“` with two left quotes, which is exactly the kind of sloppiness my leadership notices.
- **JTBD hit:** publish into my team's tools
- **What would fix it:** Default the invite link to view.

### E-05 / E-03 · Editor
- **My read:** Not my problem (yet)
- **Severity for me (1-5):** 3
- **In character:** Export finally has a visible entry point in the header, and it wasn't exercised — so the one question I care about most, does the PNG/SVG match what I see, is still unanswered after three runs.
- **JTBD hit:** export fidelity
- **What would fix it:** Exercise Export next run and diff the output against the canvas.

### D-09 / D-04 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Two empty "Untitled diagram" files eat 2 of 3 slots forever, and the paywall's answer is *"Get more diagrams for free"* without naming the cap, without a price, and selling me *"Collaborate with comments and sharing"* that already works on Basic (E-01).
- **JTBD hit:** make the next diagram
- **What would fix it:** Don't count empty diagrams against the cap, and name the cap in the paywall.

### AI-07 / D-05 · AI + Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** Everything is "Untitled diagram" and the upsell has five different names now — "Try Plus free", "Start your free trial", "Start Free Trial", "Start my trial", "Upgrade to Plus". Two on screen at once reads as unfinished.
- **JTBD hit:** looks professional enough to share
- **What would fix it:** Auto-title on apply; pick one upsell string.

## What I never saw but needed to
- Any theme save / reuse affordance. O-08 only says the "Redux" dropdown is still labelled — nothing about saving a style or applying it to the next diagram.
- Whether AI generation respects a saved style. Untestable: nothing was applied to a canvas at all.
- Export fidelity. E-05 marks Export visible but **not exercised**; E-03 lists exports, code round-trip, version history and node-level AI as not covered.
- Templates & Diagram types gallery, Upload, Generate/Paste toggle — blocked for a second run by 3/3.
- Reusable node types or workspace-level style inheritance. Team spaces have not been covered in any of three runs.

## One thing I'd tell the team
You made generation fast and the website honest about what it is, then shipped a button whose only job is to put my diagram on the canvas and it deletes it — fix AI-08 before you ship anything else.
