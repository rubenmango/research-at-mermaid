# Persona 05 — The Knowledge Steward
## Verdict
Blind spot
## Would I start the trial after this session?
Not yet — nothing in this run showed me an org, a role, an audit log, or a default I can enforce; I saw a personal workspace, a broken SSO metric, and a share link that defaults to edit.
## Three words I'd use to describe the product after this run
individual-first, ungoverned, unproven
## Brand voice read
The site talks to a maker, never to the person who has to sign off. "Prompt inside the canvas, drop in a doc" and "Describe it, or paste anything here." are fine for a contributor; nothing anywhere says "your admin controls" or "your org." The paywall's "Your one-time 15% offer... Trial Plusfree for 7 days" reads like a consumer coupon, not a vendor I'd put in front of procurement. It's one voice, but it's not talking to me.
## Reactions

### A-01 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** 62 SSO successes against 6,551 failures in 28 days. Either SSO is broken for enterprise accounts, or you can't tell whether it is — and I can't take "we don't know" past my security team.
- **JTBD hit:** Access controls match what IT/security expects.
- **What would fix it:** Have the owning engineer confirm what `User SSO Login Failed` actually fires on, then publish a real SSO success rate I can be shown.

### W-01 / W-10 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Sixty-seven 401s on public marketing pages is the kind of thing my IT team screenshots and forwards to me. A 401 on a static asset looks like an auth failure to anyone auditing traffic.
- **JTBD hit:** Access controls (perception); otherwise none.
- **What would fix it:** Return 404 for rotated chunk hashes and fix the edge/origin skew so anonymous traffic never sees auth codes.

### E-01 · Editor
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** Public link defaulting to "No access" — good, that's what I'd expect. But "Use invite link — Copy link — Can edit" means any user who pastes that link into a Slack channel has just granted edit rights to whoever forwards it. Can I lock that to view org-wide, or only per diagram?
- **JTBD hit:** Risky behaviors (external sharing) controllable or visible.
- **What would fix it:** Default invite links to "Can view" and give admins an org-level setting to enforce it.

### D-04 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "Collaborate with comments and sharing" is sold as a Plus benefit while Share visibly works on Basic — if the paywall misstates what's gated, I can't trust the pricing table when I'm mapping entitlements for procurement. And "Plusfree" on a revenue surface tells me nobody is reviewing customer-facing copy.
- **JTBD hit:** Standardize — I need accurate entitlement statements.
- **What would fix it:** Correct the benefit list to what Plus actually unlocks, fix the typo, and show the price.

### W-06 / D-03 · Website
- **My read:** Friction
- **Severity for me (1-5):** 2
- **In character:** "3 Diagrams" on the card, "Up to 6" in the table, "3 of 3" in the app. If the free-tier cap is inconsistent, I assume seat counts and AI credits might be too.
- **JTBD hit:** none directly; vendor trust.
- **What would fix it:** One source of truth for plan limits that feeds both the pricing page and the app.

### W-08 / W-09 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "Unlimited*" with no footnote — that asterisk is exactly what legal will ask me about, and I have no answer. And a paid tier labelled "Get started for free" beside a free tier labelled "Get started" is the sort of ambiguity that stalls a purchase order.
- **JTBD hit:** Standardize / what happens when legal asks.
- **What would fix it:** Publish the footnote and align the CTA labels to what each tier actually offers.

### O-01 · Onboarding
- **My read:** Fine
- **Severity for me (1-5):** 2
- **In character:** SSO is offered on signup, and "We recommend using your work email" is the right nudge. But "Terms of Use and Terms & Conditions" with no privacy policy at the point of consent — data residency and processing terms are what I'm checking for.
- **JTBD hit:** Access controls / compliance.
- **What would fix it:** Link the privacy policy and DPA alongside the terms on the signup form.

### O-08 · Onboarding
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** A dropdown that just says "Redux" is a per-user theme pick with no label. That's the exact pattern I'm here to prevent: every person choosing their own colors per diagram, nothing enforceable from the org.
- **JTBD hit:** Org-wide defaults — brand colors, fonts, theme.
- **What would fix it:** Label it "Theme" and add an org-level default that the picker inherits and an admin can lock.

### D-06 / W-04 / AI-06 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Unnamed nav menuitems, 8+ unnamed buttons, unnamed send controls — my accessibility team runs a VPAT check before anything hits the approved list. This fails it on sight.
- **JTBD hit:** Compliance.
- **What would fix it:** Add accessible names to every icon control and nav item before the next enterprise review.

### A-03 · Auth/App
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** If you can't segment your own signup event by auth method, you can't tell me how many of my users came in via SSO versus a personal Google account. That's the first question I'd ask about domain restriction.
- **JTBD hit:** See what's happening across the team.
- **What would fix it:** Add a `method`/`provider` property to `User Sign Up @server`.

### ACCOUNT ACCESS (Turnstile 600010) · Auth/App
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** "Failed to validate. Please check your internet connection" inside an embedded webview — that's my locked-down corporate browser fleet. Email login failing there means a ticket queue on my desk.
- **JTBD hit:** Access controls.
- **What would fix it:** Test and document Turnstile behaviour in managed/embedded browsers, with a fallback path.

### D-08 · Dashboard
- **My read:** Not my problem
- **Severity for me (1-5):** 1
- **In character:** CSP report-only `unsafe-eval` violations get noted by my security reviewer, but report-only means it's being watched. Fine for now.
- **JTBD hit:** none
- **What would fix it:** nothing needed

## What I never saw but needed to
- Team spaces / Create Organization — explicitly not covered; this is my entire world.
- Any admin console, roles, permissions, domain restriction, SCIM.
- Audit log or org-wide activity view — hard block until seen.
- Org-wide theme/brand defaults; only a per-user "Redux" picker was observed.
- Whether an admin can disable public links or set invite-link default to view.
- Data residency, SOC2, DPA, or privacy policy at signup.
- Export and version history — needed for "what happens when legal asks."

## One thing I'd tell the team
Everything I saw was a personal workspace with a personal theme and a personal share link — until there is an org layer with an audit log and enforceable defaults, and an SSO metric you can actually read, I have nothing to show my security team.
