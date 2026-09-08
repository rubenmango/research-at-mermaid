# Persona 06 — The Knowledge Contributor
## Verdict
Partially served
## Would I start the trial after this session?
Not yet — the AI made a correct diagram fast, but nothing in this run showed me a theme I can save, an export I can trust, or a price I can evaluate, and I'm not paying to find out.
## Three words I'd use to describe the product after this run
capable, unstyled, unfinished
## Brand voice read
The homepage sells me a canvas that builds itself ("watch the diagram build itself"); the editor mostly delivers that ("What do you want to diagram?", "Describe it, or paste anything here."). Then the paywall drops to "You hit a limit. Plus removes all of them" and "Trial Plusfree for 7 days" — a typo on the screen that asks for my card. Three placeholders and four trial labels later, it reads like several teams shipping past each other, which is exactly the inconsistency I'm trying to stop in my own diagrams.

## Reactions

### W-02 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** I typed a full prompt into the hero box and it ate it silently. If the front door doesn't work, I assume the editor's "polish" story is marketing too.
- **JTBD hit:** none directly — first impression
- **What would fix it:** Fix the 401 chunk skew (W-01) and show an error state instead of a box that swallows text.

### W-06 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "3 Diagrams" on the card, "Up to 6" in the table. I make diagrams for a living; whether I get 3 or 6 before the wall decides if free is even a real evaluation.
- **JTBD hit:** deciding whether the tool fits my team
- **What would fix it:** Make the table say 3 (D-03 confirms the in-app truth).

### W-07 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "Limited AI" and "Limited diagram size" tell me nothing. 15 credits and a 60-line cap are the real numbers, and my team's diagrams run past 60 lines regularly.
- **JTBD hit:** making diagrams accurate and complete
- **What would fix it:** Put "15 AI credits · 60-line diagrams" on the Basic card.

### O-04 · Onboarding
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** "Generate / Paste Mermaid code" is exactly my split — AI some of the time, syntax the rest. I knew where I was in two seconds.
- **JTBD hit:** make the diagram quickly
- **What would fix it:** nothing needed

### O-08 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** A dropdown that just says "Redux" next to the submit arrow — I had no idea that was the theme picker until the canvas said "Redux Color theme". Why does it look like this by default, and where do I save *my* look?
- **JTBD hit:** on-brand output; save styling for next time
- **What would fix it:** Label it "Theme: Redux" and put a "Save as team theme" option in the same menu.

### AI-01 · AI
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** Ten seconds, a proper `sequenceDiagram` with the failure branch I asked for, credit ticked 15 to 14 honestly. That's the part I'd normally hand-write; happy to let it go.
- **JTBD hit:** accurate diagram, fast
- **What would fix it:** Keep the first auto-title ("Email Signup Verification") — it was more accurate than "Email Password Flow".

### AI-03 · AI
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** I asked for a 429 branch, the credit was spent, and the canvas didn't move. The updated diagram was hiding behind a button called "Edit". Correct output, wrong choreography — and the tip had just promised "real time".
- **JTBD hit:** don't spend 30 minutes fighting the tool
- **What would fix it:** Rename the button "Apply to canvas" and show a diff preview before the credit is charged.

### O-06 / O-07 · Onboarding
- **My read:** Fine
- **Severity for me (1-5):** 2
- **In character:** "Built something good? Show it off — export as PNG, SVG, or PDF" is the right nudge for someone who publishes into Confluence and PRs all day. But "watch the diagram update in real time" is not what happened.
- **JTBD hit:** export/embed cleanly
- **What would fix it:** Change the tip to "review the proposed change in chat and apply it".

### AI-04 · AI
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** "Describe it, or paste anything here." / "Describe what to add or change" / "Describe your idea" — three voices for one box. If the product can't keep its own style consistent, I'm sceptical it'll keep mine.
- **JTBD hit:** consistency (as a signal)
- **What would fix it:** One placeholder string per AI input state, owned by one team.

### D-04 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** "Trial Plusfree for 7 days" and "15% off" with no price on the screen asking me to upgrade. I'm not putting a typo-riddled paywall in front of my manager as the reason we should pay.
- **JTBD hit:** getting the team to adopt
- **What would fix it:** Fix the space, show the actual €/user/mo number, and say "You've used 3 of 3 diagrams".

### D-05 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** "Try Plus free", "Start my trial", "Start your free trial", "Start my free trial" — four labels, one action. I already did this last time. Why am I reading it four ways?
- **JTBD hit:** none
- **What would fix it:** One string, everywhere.

### D-01 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** Clicked "New diagram", nothing for three seconds, clicked again. Small thing, but it's the button I'd hit ten times a week.
- **JTBD hit:** speed
- **What would fix it:** Disabled state plus spinner on click.

### E-01 · Editor
- **My read:** Fine
- **Severity for me (1-5):** 2
- **In character:** Share dialog is clean and "Public link access — No access" by default is sensible. Invite link defaulting to "Can edit" is generous when I mostly want the team to *read* the thing.
- **JTBD hit:** embed/share into team tools
- **What would fix it:** Default invite link to "Can view".

## What I never saw but needed to
- Export (PNG / SVG / PDF) — I never got to check whether the export matches the canvas. My whole "which one is real?" question is unanswered.
- Theme save / reuse across diagrams — only saw "Redux" as a bare dropdown; no save, no team inheritance.
- A second diagram in the same session to test whether style carries over.
- Reusable node types or components — nothing beyond colors observed.
- AI respecting a saved style — couldn't test, since no style was ever saved.
- Templates & Diagram types gallery, version history, code-panel round-trip.

## One thing I'd tell the team
The AI made a correct diagram in ten seconds, then you hid the theme behind one unlabelled word and never let me save it — the generation is solved, the reuse story is the gap.
