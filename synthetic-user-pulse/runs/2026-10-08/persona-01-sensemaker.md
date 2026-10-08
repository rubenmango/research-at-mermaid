# Persona 01 — The Sensemaker

## Verdict
Partially served

## Would I start the trial after this session?
No — the AI wrote me the right diagram in under ten seconds and then the button whose only job is to put it on the canvas deleted it, so I have nothing to pay for yet.

## Three words I'd use to describe the product after this run
Fast, promising, unfinished

## Brand voice read
The website talks like a product that already works: *"You already think in systems. Now you can diagram them just as fast."* and *"Prompt inside the canvas, drop in a doc, or write it in code — and watch the diagram build itself."* Then the app I land in says *"Describe your idea"* and, after it eats my diagram, *"Hit Fix with AI (⌘⇧F) to automatically correct syntax errors in your diagram code."* That's two different companies talking — one selling me a canvas that builds itself, one handing me a keyboard shortcut for a code problem I didn't create. And one signup variant literally advertises *"Repair broken Mermaid code"* as a benefit, which tells me they know.

## Reactions

### AI-08 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** I clicked Edit, the chat closed, and the code panel had one line — `flowchart TD`. Every box, every arrow, gone, and then it tells me *I* have a syntax error. That's my entire session in one click.
- **JTBD hit:** Take an AI diagram from "close but wrong" to "I'd put my name on it"
- **What would fix it:** Apply the full generated code to the canvas, and if apply fails, keep the previous content and say so.

### AI-01 · AI
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** Under ten seconds, valid on the first try, no "Repairing diagram…" theatre. That's the part I'd tell people about. Credit where it's due — last run this was the broken bit.
- **JTBD hit:** Diagram renders on first try
- **What would fix it:** nothing needed

### AI-03 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** The diagram exists as a little card inside a chat panel while the canvas sits blank and the file is still "Untitled diagram". A preview I can't use isn't an output.
- **JTBD hit:** Leave with something I trust enough to share
- **What would fix it:** Render the generation straight to the canvas; make Edit a refinement, not the only door.

### AI-09 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** A diagram I paid a credit for sixteen days ago now reads *"Unable to render diagram / There's a syntax error in the Mermaid code"*. So my work doesn't just sit there, it rots. I'd never build a doc on top of this.
- **JTBD hit:** Trust
- **What would fix it:** Store the generated code, not a re-render that can go stale.

### AI-10 · AI
- **My read:** Delight
- **Severity for me (1-5):** 2
- **In character:** *"Can you add a signup rate-limit check that returns 429?"* — yes, that's exactly how I want to edit, in words, no syntax. Completely wasted while Edit destroys the result.
- **JTBD hit:** Fix the 1–2 wrong things without touching syntax
- **What would fix it:** nothing needed — unblock AI-08 and this becomes the product.

### AI-04 · AI
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Something got charged and nothing on screen told me what or how much is left. I burned a credit to produce a one-line stub.
- **JTBD hit:** Trust
- **What would fix it:** Put the remaining credit count next to the prompt bar.

### E-04 · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Blank dotted grid, a "Flowchart" badge, no error anywhere — and the "Fix with AI" control is in the page but invisible. It knows it's broken, it knows the fix, and it shows me neither.
- **JTBD hit:** One recovery action after a failed render
- **What would fix it:** Show an error state on the canvas with a visible "Fix with AI" button.

### E-01 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Share was easy to find and it opened populated this time, no five-second spinner — good. But "Copy link" defaults to **"Can edit"**, so anyone I drop it to in Slack can rewrite my diagram. Third run, apparently.
- **JTBD hit:** Leave with a link I trust
- **What would fix it:** Default the invite link to view.

### AI-07 · AI
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** Still "Untitled diagram" after all that. I'd have to rename it before sharing or my team sees four identical files.
- **JTBD hit:** Shareable
- **What would fix it:** Title from the AI's own summary line.

### W-02 / W-03 · Website
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** The hero box actually takes typing now, and clicking "Design a system" flipped the preview to UI / API / Service / Database in about three seconds. That's the promise. Shame the app behind it can't keep it.
- **JTBD hit:** See if the thing works before signing up
- **What would fix it:** nothing needed

### O-10 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** I open a diagram and get a blank canvas and a one-line bar saying "Describe your idea". No paste option, no templates, nothing telling me the chat I used earlier still exists.
- **JTBD hit:** Get going without reading anything
- **What would fix it:** Put a labelled "AI chat" control in the editor chrome so history is findable.

### D-09 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Two empty files I never filled are eating two of my three slots and nothing on the paywall mentions them. Clicking "New diagram" while thinking shouldn't lock me out.
- **JTBD hit:** none directly — but it's why I couldn't start clean
- **What would fix it:** Don't count empty diagrams against the cap.

### D-04 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** *"Trial Plusfree for 7 days"* — three runs with a missing space on the page where you ask for money. And it sells me sharing, which I just used on the free plan.
- **JTBD hit:** none
- **What would fix it:** Fix the string, state a price, stop selling a feature I already have.

## What I never saw but needed to
- Export — the control is now in the header but was not exercised, so I don't know if I can get a PNG or embed out of this.
- Code round-trip, version history, node-level AI edits — not covered, so I can't say whether there's a recovery path after AI-08.
- The new-diagram empty state, Templates and the Generate/Paste toggle — blocked at 3/3 for a second run.
- A true cold first run after signup; and whether the progressive "Create → Edit with AI → Share" tips ever fire.
- Voice chat — a new input mode appeared on the prompt bar and nobody tried it.

## One thing I'd tell the team
Nothing else on this list matters until clicking "Edit" puts the diagram the AI just wrote onto the canvas intact.
