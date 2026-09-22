# Persona 03 — The Maintainer
## Verdict
Blind spot
## Would I start the trial after this session?
No — two correct diagrams evaporated into a blank canvas with no history to recover them from, which is exactly the failure I came here to prevent.
## Three words I'd use to describe the product after this run
amnesiac, lossy, unverifiable
## Brand voice read
Not one product — teams that don't reconcile. Basic diagrams are "3 Diagrams" on the plan card, "Up to 6" in the compare table, "3 of 3 personal" in the app: three numbers, one fact, which is the drift problem I'd buy this tool to catch. The upsell can't agree either — "Try Plus free", "Start your free trial", "Start Free Trial", "Start my trial", and "Trial Plusfree for 7 days", a missing space that survived a full rewrite of the paywall.

## Reactions

### AI-03 · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Two credits, two correct diagrams, and closing the panel leaves a "completely empty canvas" with "no discoverable control in the editor chrome that reopens the AI chat." That is the IQVIA failure — work gone, no history, no backup.
- **JTBD hit:** Confidence my work won't be blown away
- **What would fix it:** Persist every generation as a recoverable revision and keep a permanent labelled control that reopens the chat.

### E-03 · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Version history "exists in the editor header" and was "not exercised this run." Two runs, and the one feature my job rests on has never been opened.
- **JTBD hit:** Tell what changed since I last looked, and who changed it
- **What would fix it:** Make version history, diff and code round-trip mandatory legs of every run.

### D-11 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Back after 14 days to "no last-opened, no recent-activity, no indication that the newest thing is 14 days old." The only returning-user state you computed was my cap and an upsell.
- **JTBD hit:** See at a glance whether a diagram is current
- **What would fix it:** Put last-edited timestamps and a "changed since you were here" marker on every file card.

### AI-04 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Credits went 14 → 13, then forty seconds of nothing — no echoed message, no spinner. Charged before any sign of work; I'd assume it failed and resubmit.
- **JTBD hit:** Not second-guessing what state I'm in
- **What would fix it:** Echo the message instantly and hold the credit decrement until a response lands.

### E-01 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "Use invite link" still defaults to "Can edit" — anyone the link reaches can silently rewrite a diagram I'm accountable for. Public-off is right; edit-by-default isn't, and it's unchanged.
- **JTBD hit:** Confidence content won't change underneath me
- **What would fix it:** Default the invite link to "Can view".

### D-09 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Two of three slots are "Untitled diagram" with an "Empty diagram" thumbnail from two weeks ago, and nothing flags them as junk. The cap converted on an accident, not on value.
- **JTBD hit:** none — but it's the same blindness: you can see they're empty and say nothing
- **What would fix it:** Flag empty diagrams in the grid and offer to clear them before the paywall.

### D-04 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Credit for killing the fake "one-time 15% offer". But trading the labelled decline "I'll keep my limits" for an unnamed × on a modal whose primary button is also unnamed is a regression, and it still sells "Collaborate with comments and sharing" when E-01 proves sharing works on Basic.
- **JTBD hit:** none
- **What would fix it:** Restore a named decline and drop the benefit claim Basic already has.

### A-01 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** ~1,000 SSO failures a week against ~5 successes a day, and after two runs nobody can say whether it's a real outage or an over-firing event. Your own source of truth is drifting, unowned, for a month.
- **JTBD hit:** Where does this number come from? Is it current?
- **What would fix it:** Give one person the job of deciding whether `User SSO Login Failed` over-fires, before Run 03.

### O-04 · Onboarding
- **My read:** Fine
- **Severity for me (1-5):** 1
- **In character:** Turnstile renders now and the signup page threw zero console messages — a real fix, correctly predicted as downstream of hydration.
- **JTBD hit:** none
- **What would fix it:** nothing needed

## What I never saw but needed to
- Version history — control exists, never opened (E-03). Two runs, zero coverage of the feature my job depends on.
- Code panel round-trip: whether visual and code stay in sync. That is literally the IQVIA failure.
- Any diff, "last synced", source link or change notification — nothing in the evidence records such a surface existing.
- Team spaces, Shared with you, comments, search — not covered in either run, so I never saw how someone else's change would reach me.
- Export and Mermaid Flow — not exercised.

## One thing I'd tell the team
The product has no concept of time — not on a file card, not in the AI panel, not in the dashboard — and until it does there is no version of this I can rely on.
