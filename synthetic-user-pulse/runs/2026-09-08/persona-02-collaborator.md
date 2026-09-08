# Persona 02 — The Collaborator
## Verdict
Partially served
## Would I start the trial after this session?
Not yet — I got a live share link with real permissions, but I never saw comments, history, or an embed story, and the paywall wouldn't tell me the price for what it was selling.
## Three words I'd use to describe the product after this run
promising, unfinished, unaccountable
## Brand voice read
The homepage talks about my world — "share with one link (no login required)" — but the app quietly walks that back to a public link that's "No access" by default. Then the paywall says "Plusfree for 7 days" and lists "Collaborate with comments and sharing" as a Plus perk while Share already works on my Basic account. It reads like three teams wrote three products; nobody signed off on the whole thing.
## Reactions

### W-02 · Website
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** I typed a 93-char prompt into the hero and it ate it with no error. If I'd sent Priya this link as "try it", she'd have bounced in ten seconds.
- **JTBD hit:** none directly — but it undermines my ability to loop people in via the marketing site.
- **What would fix it:** Fix the 401'd chunks (W-01) and show a visible error when hydration fails instead of a silent box.

### W-06 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "3 Diagrams" on the card, "Up to 6" in the table. If I'm pitching a team plan to my manager I need one number I can put in the RFC — I can't cite a page that disagrees with itself.
- **JTBD hit:** Getting sign-off on tooling.
- **What would fix it:** Make the pricing table say 3, matching the in-app quota chip (D-03).

### A-01 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** 62 SSO successes against 6,551 failures in 28 days. My company is 400 people on Okta — if SSO is broken, or if you can't even tell whether it's broken, I cannot bring the team in. Full stop.
- **JTBD hit:** Two or more teammates can edit — they have to get in first.
- **What would fix it:** Have the owning engineer determine whether `User SSO Login Failed` over-fires, and fix whichever half is true.

### E-01 · Editor
- **My read:** Delight (with a governance nit)
- **Severity for me (1-5):** 2
- **In character:** Okay — "Invite people", "Use invite link — Copy link — Can edit", "Who has access", public link off. That's the Google Doc model I expected. But the copied link defaulting to "Can edit" means I'd paste it into Slack and hand edit to forty people by accident.
- **JTBD hit:** Shared ownership; drop it into Slack.
- **What would fix it:** Default the invite link to view, one click to promote to edit.

### AI-03 · AI
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** The credit was spent, the "Summary" said the diagram was updated, and the canvas didn't change. If Tom and I are both in this and his edit is a proposal sitting in his chat, which version is the source of truth? Apply is labelled "Edit", which tells me nothing.
- **JTBD hit:** See what changed and who changed it.
- **What would fix it:** Rename the apply action to "Apply to diagram" and say in the response that it's a proposal awaiting apply.

### O-07 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "Watch the diagram update in real time" — then it doesn't. I'd assume the edit failed and re-run it, burning another credit.
- **JTBD hit:** Trust in what's on the canvas.
- **What would fix it:** Change the tip to describe the proposal-then-apply flow honestly.

### O-06 · Onboarding
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** "Share a live link for teammates to view or edit, or export as PNG, SVG, or PDF" — that is exactly the sentence I needed, and it arrived after my first edit, not before I'd done anything.
- **JTBD hit:** Port it into the surface where the decision is being made.
- **What would fix it:** nothing needed — though "embed in Confluence/Notion" belongs in that same sentence if it exists.

### D-04 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "Plusfree", "15% off" with no price, and "Collaborate with comments and sharing" as a Plus benefit when Share opened fine on Basic. This is the screen where I'd decide to expense it for five people, and I can't evaluate it.
- **JTBD hit:** Sign-off on tooling.
- **What would fix it:** Show the per-seat price, fix the typo, and only list benefits that are genuinely gated.

### D-05 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** "Try Plus free" / "Start my trial" / "Start your free trial" / "Start my free trial" — four labels for one door. If I tell Sarah "click Start my trial" she may not find it.
- **JTBD hit:** none
- **What would fix it:** One string, everywhere.

### D-01 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** Three seconds of dead button on "New diagram". I clicked twice; on a team account that's two stray documents cluttering the shared space.
- **JTBD hit:** Keeping the shared space clean.
- **What would fix it:** Disable the button and show a spinner on click.

### D-02 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** "0 Items" with three diagrams on screen. Small, but it's the kind of thing that makes me doubt the rest of the counts.
- **JTBD hit:** none
- **What would fix it:** Wire the footer counter to the actual list.

### E-03 · Editor
- **My read:** Blocker (by absence)
- **Severity for me (1-5):** 5
- **In character:** Version history and the code round-trip weren't exercised, and I saw no comment affordance anywhere in this trace. Where's history? Thirty seconds — I'd be back in Slack asking Tom "did you change this?".
- **JTBD hit:** See what changed and who changed it; comment in place.
- **What would fix it:** Cover history, comments, and embed in the next run so I can judge them at all.

## What I never saw but needed to
- Inline comments tied to a node or region — not in the evidence.
- Version history / authorship ("Wait, did Tom edit this? When?") — not exercised (E-03).
- Embed into Confluence / Notion / GitHub PR — never reached; export formats not exercised.
- What happens when two of us edit at once — no co-editing observed.
- Team spaces / Create Organization — explicitly not covered.
- Whether the recipient of an invite link must sign up — not tested.

## One thing I'd tell the team
Sharing is real and permissions look right, but until I can see who changed what, comment on a box, and embed it live in the RFC, the diagram is still a snapshot — and I can't put a snapshot in a PR.
