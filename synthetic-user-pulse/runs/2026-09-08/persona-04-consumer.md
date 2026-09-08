# Persona 04 — The Knowledge Consumer

## Verdict
Blind spot

## Would I start the trial after this session?
Not yet — nothing in this run showed me a diagram someone else made, so I have no idea whether I could find it, trust it, or link to it; the trial pitch is all about making more diagrams, which is not my job.

## Three words I'd use to describe the product after this run
solo, author-first, uncatalogued

## Brand voice read
The website talks to a maker: "Prompt inside the canvas, drop in a doc, or write it in code — and watch the diagram build itself." The editor talks to a maker: "What do you want to diagram?" The paywall talks to a maker: "Unlimited diagrams". It is one consistent voice, but it is consistently not talking to me. The only line that brushes my world is the tip "Share a live link for teammates to view or edit" — and that's addressed to the author, not the teammate. I'm the teammate. Nobody wrote a sentence for me.

## Reactions

### W-02 · Website
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** I wasn't going to type a prompt anyway — my first question is "is there one already?" But a homepage whose main box silently eats 93 characters makes me wonder what else swallows input without telling me.
- **JTBD hit:** none directly
- **What would fix it:** Fix the chunk serving (W-01) and show an error state when hydration fails instead of a dead box.

### W-03 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "What do you need to figure out?" is actually my question — I clicked "Plan my project" hoping to see what a finished diagram of that looks like, and got a blank bordered box. That's the one preview I'd have used to judge whether this tool's diagrams are readable without the author.
- **JTBD hit:** understand what a diagram is saying without reading every box
- **What would fix it:** Make the preview panel render, and let me open the example full-size.

### W-06 / D-03 · Website
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** "3 Diagrams" on the card, "Up to 6" in the table, "3 of 3 personal" in the app. If the product can't keep one number consistent across three surfaces, I'm nervous about whether the diagram I find is the current version of anything.
- **JTBD hit:** "is this the right one?" — trust in canonical truth
- **What would fix it:** Pick 3, fix the table, and treat copy drift as a bug.

### O-01 · Onboarding
- **My read:** Fine
- **Severity for me (1-5):** 2
- **In character:** Google, GitHub, SSO first and "We recommend using your work email" — good, that's how I'd expect to land in my team's workspace. But nothing here asks "are you joining an existing team?" so I assume I'd end up in an empty personal space, which is the wrong room.
- **JTBD hit:** find what already exists via navigation
- **What would fix it:** After work-email signup, detect the domain and offer "Join <org>'s workspace" before dropping me into Personal.

### A-01 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** 62 SSO successes against 6,551 failures in 28 days. I'm the person who gets invited to a team workspace and logs in through the company IdP — if SSO is actually failing 99% of the time, I never even get to the diagrams. If the event is just over-firing, fine, but somebody needs to know which.
- **JTBD hit:** every job — can't find, understand, or share what I can't reach
- **What would fix it:** Have the owning engineer confirm whether `User SSO Login Failed` fires on a domain probe, and if not, treat SSO as down.

### D-02 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** "0 Items" with three diagrams on screen. When I'm browsing someone else's project to see whether the thing I need exists, the count is the first thing I read. A counter that's wrong in every state tells me I can't trust the list either.
- **JTBD hit:** find via navigation, judge by metadata
- **What would fix it:** Make the footer count match the visible documents.

### D-04 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "Collaborate with comments and sharing" as a Plus benefit — but Share worked on Basic in the same session. The one feature I care about is being sold to me as locked when it isn't. Also "Plusfree" and "You hit a limit" with no price; I'd close this dialog.
- **JTBD hit:** share or cite it elsewhere
- **What would fix it:** Say exactly what sharing Plus adds over Basic, fix the typo, show the price.

### O-04 · Onboarding
- **My read:** Not my problem
- **Severity for me (1-5):** 3
- **In character:** "What do you want to diagram?" — nothing. I want to find the one my team already has. The empty editor is clear for an author; for me it's confirmation I'm in the wrong place with no "Browse team diagrams" exit.
- **JTBD hit:** default action is FIND, not CREATE
- **What would fix it:** Add "Look for an existing diagram" as a peer of "Start with a blank canvas".

### AI-01 · AI
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** The output actually reads cold: Browser / API / EmailService / Database, `alt [Token expired]`, a "User checks email" note. If a colleague handed me this I could understand it without asking them. Though the auto-title drifting from "Email Signup Verification" to the less accurate "Email Password Flow" is exactly how I'd end up opening the wrong diagram later.
- **JTBD hit:** understand without the author
- **What would fix it:** Keep the first, more specific title; let titles be the search key they need to be.

### E-01 · Editor
- **My read:** Fine
- **Severity for me (1-5):** 3
- **In character:** Invite link defaults to "Can edit". As the person receiving the link, that scares me — I don't want to accidentally change the canonical diagram while reading it. Where's the view-only / open-in-editor / fork choice on my side?
- **JTBD hit:** reuse without breaking the original
- **What would fix it:** Default invite links to "Can view" and show me an explicit "Make a copy" when I open someone else's diagram.

### O-09 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** `?shouldShowPopup=true&entryPoint=Dashboard` in the URL — that's the link I'd paste into a spec. Now every reader gets a popup and a fake entry point.
- **JTBD hit:** link to it / cite it
- **What would fix it:** Strip control-flow params after use so the address bar is a clean permalink.

## What I never saw but needed to
- Team spaces / shared workspace / "Shared with you" contents — the evidence never opened one. My whole world is marked not covered.
- Search. No query was ever typed into anything that finds diagrams. I don't know if it exists.
- Any diagram authored by someone else: owner, last-updated, description, "canonical" marker.
- Fork / duplicate behaviour and whether a copy keeps a link to its source.
- Templates & Diagram types gallery (the closest thing to "is there one already?").
- Returning-user dashboard — what I see on day two.

## One thing I'd tell the team
You tested a product for people who make diagrams; run the next pass as someone who inherits them, because right now "Shared with you" is a sidebar button with no name and no evidence behind it.
