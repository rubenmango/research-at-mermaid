# Persona 01 — The Sensemaker
## Verdict
Partially served
## Would I start the trial after this session?
Not yet — the AI itself got my sequence diagram right in 10 seconds, but the homepage ate my prompt, the "edit" looked like it failed, and the paywall wants me to buy something with no price on it.
## Three words I'd use to describe the product after this run
capable, flaky, half-finished
## Brand voice read
The homepage talks like it knows me: "Prompt inside the canvas, drop in a doc, or write it in code — and watch the diagram build itself." Then the box didn't take a keystroke. The editor keeps that promise better — "What do you want to diagram?" and "Describe it, or paste anything here." are exactly my register. The paywall breaks the spell: "You hit a limit. Plus removes all of them" and "Trial Plusfree for 7 days" read like a different, sloppier company wrote them.

## Reactions

### W-01 · Website
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** I don't see console errors, I see a page that doesn't do anything. If the thing doesn't work before I've signed up, I assume it won't work after.
- **JTBD hit:** "The diagram renders on first try" — I never got to a diagram.
- **What would fix it:** Fix the deploy skew and make unknown asset paths return 404, not 401, so cache-busting actually busts.

### W-02 · Website
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** I typed a full sentence into the hero box and nothing happened. Not an error, not a spinner — nothing. That's my one recovery action spent; I'm pasting the code back into Claude and going to draw.io.
- **JTBD hit:** 90 seconds from arriving to "trusted and shared" — died at second five.
- **What would fix it:** Make the hero box a real input that fails loudly ("Something went wrong, try again") instead of a silent container.

### W-03 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** I clicked "Plan my project", the chip lit up, and the preview next to it stayed a blank box. Two dead things on one page tells me the site is broken, not that I clicked wrong.
- **JTBD hit:** none directly — but it kills my trust before I try.
- **What would fix it:** Same fix as W-01; until then, hide the panel rather than show an empty border.

### W-06 · Website
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** Card says "3 Diagrams", table says "Up to 6". I don't care much on Basic, but if you can't count your own free tier I'm going to double-check everything else you tell me.
- **JTBD hit:** none
- **What would fix it:** Make the card and the table both say 3, since the app says "3 of 3".

### O-03 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** Re-enter password when there's already a show/hide eye on the field? Fine. Annoying. I'd have hit Google anyway.
- **JTBD hit:** none
- **What would fix it:** Drop the confirm field; the toggle already does that job.

### D-01 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** Clicked "New diagram", nothing. Clicked again, nothing. Three seconds later it moved. That's the second time today a button swallowed my click with no feedback.
- **JTBD hit:** speed to first render.
- **What would fix it:** Disable the button and show a spinner the instant it's clicked.

### O-04 · Onboarding
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** "What do you want to diagram?" with Generate / Paste Mermaid code as the toggle — that's the exact question I showed up with. No modal, no tour, just the box.
- **JTBD hit:** "fix things without touching syntax" — the Paste option means my Claude output has a front door.
- **What would fix it:** nothing needed

### AI-01 · AI
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** Ten seconds, sequenceDiagram with the expired-token branch I asked for, and the credit ticked 15 to 14 right there. Okay, the boxes are there. This is the product I came for.
- **JTBD hit:** renders on first try; readable without dragging.
- **What would fix it:** Keep the first auto-title — "Email Signup Verification" was better than "Email Password Flow".

### AI-02 · AI
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** It showed me the diagram and *then* asked about retry limits and error messages. That's the right order — don't interrogate me before I've seen anything.
- **JTBD hit:** proactive guidance instead of blank canvas.
- **What would fix it:** nothing needed

### AI-03 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** I asked for a rate-limit check, it said "Here's the updated signup sequence", spent a credit, and the diagram on my canvas didn't change. I read that as "the edit failed." The button I needed was called "Edit" — I already *did* the edit, why would I click that?
- **JTBD hit:** "describe a change in natural language and see it apply" — the core job.
- **What would fix it:** Apply the change to the canvas by default with an Undo, or at minimum label the button "Apply to diagram".

### O-07 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** The tip said "watch the diagram update in real time." It didn't. Don't promise me a behaviour the very next screen contradicts.
- **JTBD hit:** trust.
- **What would fix it:** Rewrite the tip to match what actually happens, or make AI-03 true.

### O-08 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** A dropdown that just says "Redux" next to the send arrow. I thought it was a framework setting. Insider word, no label.
- **JTBD hit:** none
- **What would fix it:** Prefix it "Theme: Redux" or give it a palette icon.

### D-04 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "Your one-time 15% offer" — off what? There's no price on the dialog. And "Trial Plusfree for 7 days" has a typo on the screen where you ask for my card. I'll keep my limits.
- **JTBD hit:** none — but it decides whether I pay.
- **What would fix it:** Show the actual monthly number, name the limit ("3 of 3 diagrams"), fix the space.

### E-01 · Editor
- **My read:** Fine
- **Severity for me (1-5):** 1
- **In character:** Cool, that worked. Share was in the header, invite link copies, done. "Can edit" by default is a bit loose for a link I'm pasting into Slack, but I'd flip it and move on.
- **JTBD hit:** "leave with a link I trust enough to drop into a PR."
- **What would fix it:** Default the invite link to view; one toggle away from edit.

### D-05 · Dashboard
- **My read:** Fine
- **Severity for me (1-5):** 1
- **In character:** "Try Plus free", "Start my trial", "Start your free trial", "Start my free trial" — four names for one button. Looks fine. Not tidy, but fine.
- **JTBD hit:** none
- **What would fix it:** Pick one string.

## What I never saw but needed to
- Pasting my own AI-generated Mermaid code and seeing it render (or error) — the "Paste Mermaid code" path was never exercised.
- What happens on a syntax error: is there a one-click AI repair, or a wall of red?
- Click a box, rename a label — direct edits were not tried.
- Export as PNG/SVG for a PR or doc — export not covered.
- A true cold first run; this account was warm.

## One thing I'd tell the team
The AI is good enough to win me — so stop making its results look like failures: a silent hero box, a silent "New diagram" click, and an edit that lands in chat instead of on my diagram all read as "it didn't work."
