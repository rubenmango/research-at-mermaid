# Persona 03 — The Maintainer
## Verdict
Blind spot
## Would I start the trial after this session?
Not yet — nothing in this run told me when a diagram was last updated, where it came from, or what changed, so I have no reason to believe it would stay current after I close the tab.
## Three words I'd use to describe the product after this run
generative, unversioned, untrustworthy
## Brand voice read
The whole funnel talks to someone making something new: "watch the diagram build itself" (W-02), "What do you want to diagram?" (O-04), "Built something good? Show it off" (O-06). Not one string speaks to someone checking whether an existing diagram is still true. The one place it promises continuity — "watch the diagram update in real time" (O-06) — turned out to be false (AI-03), which for me is worse than saying nothing. It sounds like one product, but it's not talking to me.

## Reactions

### W-02 · Website
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** The hero box ate 93 characters and said nothing. Silent failure on the front door is exactly the failure mode I distrust everywhere else — if the marketing page swallows input without a word, why would the editor tell me when a sync fails?
- **JTBD hit:** none directly; erodes the trust I need for "don't second-guess the version"
- **What would fix it:** Return 404 not 401 for rotated chunks and show an error state when hydration fails.

### W-06 / D-03 · Website
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "3 Diagrams" on the card, "Up to 6" in the table, "3 of 3" in the app. This is drift in your own pricing page. If the source of truth for your own numbers is out of sync, I'm not confident your product keeps *mine* in sync.
- **JTBD hit:** confidence I'm looking at the current version
- **What would fix it:** One owner for tier limits, rendered from a single source into card, table, and app.

### A-01 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** 62 successes against 6,551 failures and nobody can tell from the data whether SSO is broken or the event over-fires. That is exactly the "diagram and source disagree silently" problem, except it's your own telemetry. Either reading means the thing my org would log in with is unverified.
- **JTBD hit:** trust; I can't approve a tool whose own state is unreadable
- **What would fix it:** Split the failure event by cause (probe vs. real attempt) so the success rate means something.

### D-01 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Three seconds of nothing after "New diagram", button still live. I'd click twice, and now I don't know whether I've got one document or two — and there's no counter I can trust to tell me (D-02).
- **JTBD hit:** knowing current state at a glance
- **What would fix it:** Disable the button and show a pending state until the POST returns.

### D-02 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "0 Items" while three diagrams are on screen and the quota chip says "3 of 3". A counter that is wrong in every state is a freshness signal that lies. I'd stop reading anything the dashboard tells me.
- **JTBD hit:** see at a glance whether the view is current
- **What would fix it:** Bind the footer count to the same list the grid renders from.

### D-04 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "You hit a limit" — which one? "Plusfree" — did anyone review this before it shipped? A paywall with a typo and no price reads like a surface nobody owns, which is the last thing I want to hear from a tool I'm meant to rely on over years.
- **JTBD hit:** none; trust
- **What would fix it:** Name the limit ("3 of 3 diagrams"), show the price, fix the space.

### AI-03 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** The chat said "Here's the updated signup sequence" and took a credit, but the canvas didn't change until I found a button labelled "Edit". So which is the source of truth right now — the canvas or the chat preview? If I close this, which one persists? Nothing told me. I'm not approving a change I can't see land.
- **JTBD hit:** if it's wrong, fix it and know it stuck
- **What would fix it:** Label the button "Apply to diagram" and show a diff of the proposal against the canvas before the credit is charged.

### O-07 / O-06 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** The tip promises edits "update in real time"; they land as proposals. A product that misdescribes its own persistence model on the first tip is one I'd assume misdescribes it elsewhere too.
- **JTBD hit:** knowing what changed and whether it's live
- **What would fix it:** Rewrite the tip to say "review the proposed change and apply it".

### AI-01 · AI
- **My read:** Fine
- **Severity for me (1-5):** 2
- **In character:** Credits 15 to 14, visible immediately — that's the one honest counter in the run. But the auto-title changed from "Email Signup Verification" to "Email Password Flow" with no reason given. Something rewrote a field underneath me and didn't say why; that's a small version of my whole problem.
- **JTBD hit:** know what changed and who changed it
- **What would fix it:** Show why the title changed, or let me pin it.

### E-01 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Invite link defaults to "Can edit". So anyone I hand the link to can change the diagram, and I saw no version history to tell me they did. I'd share view-only or not at all.
- **JTBD hit:** confidence my edits won't get blown away
- **What would fix it:** Default invite links to view; surface an edit log in "Who has access".

### O-08 · Onboarding
- **My read:** Fine
- **Severity for me (1-5):** 1
- **In character:** A dropdown that just says "Redux". Where does that come from? Minor, but it's another unexplained thing on screen.
- **JTBD hit:** none
- **What would fix it:** Prefix it "Theme:".

## What I never saw but needed to
- Version history — the evidence marks it not exercised. This is my primary question and it went unanswered.
- Any "last updated" / "last synced" timestamp on a diagram or the dashboard card.
- Returning-user dashboard / "memory delta" between sessions — not reached.
- Any source-link or origin for a diagram (codebase, doc, agent) — none observed anywhere.
- Notifications when a diagram changes — nothing in the trace.
- What happens to a manual edit when the AI proposes again — conflict behaviour untested.
- Team spaces / Organization — where inherited diagrams actually live for me.

## One thing I'd tell the team
You shipped a product that shows me what it made but never when, from where, or what changed — and until a diagram carries a timestamp, a source, and a diff, I have no reason to open it a second time.
