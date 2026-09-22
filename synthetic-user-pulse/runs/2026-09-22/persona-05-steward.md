# Persona 05 — The Knowledge Steward
## Verdict
Blind spot
## Would I start the trial after this session?
Not yet — no org, no roles, no audit log, no enforceable default appeared anywhere, so there's still nothing I can take to my security team.
## Three words I'd use to describe the product after this run
Ungoverned, inconsistent, unaccountable
## Brand voice read
It doesn't sound like one product. The compare table says Basic gets "Up to 6", the card says "3 Diagrams", the app says "3 of 3 personal" — and the wrong one is what I'd paste into a vendor comparison. The paywall survived a full rewrite still reading "Trial Plusfree for 7 days", and one action carries four names: "Try Plus free", "Start your free trial", "Start Free Trial", "Start my trial". "Unlimited*" with no footnote is what my legal team circles.

## Reactions

### A-01 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** 84 successes against 4,333 failures, and the only thing that moved is which week is in the window. If the event over-fires, a real SSO outage at my company is invisible underneath it; if it doesn't, enterprise SSO is broken. Two runs, no owner.
- **JTBD hit:** Access controls match what IT expects
- **What would fix it:** Name one owner for `User SSO Login Failed` and settle probe-vs-failure this fortnight.

### A-03 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** One row — `authentication`, 63,295 events. I can't ask how many of my people signed up by SSO, and that's the first question a renewal opens with.
- **JTBD hit:** Org-wide visibility
- **What would fix it:** Add an auth-method property to `User Sign Up @server`, as login already has.

### E-01 · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** "Use invite link" → "Copy link" defaulting to **"Can edit"** is what I'd fail you on. Public link at "No access" is right and I'll credit it, but a link that grants edit to whoever receives it is an unmanaged external collaborator with no org switch to disable it.
- **JTBD hit:** Risky behaviours controllable or visible
- **What would fix it:** Default invite links to view; let an admin lock that org-wide.

### D-04 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** You rewrote the whole dialog, kept "Trial Plusfree for 7 days", and replaced the labelled decline "I'll keep my limits" with an unnamed ×. Two unnamed buttons and no labelled exit is an accessibility finding, and mine isn't optional.
- **JTBD hit:** Would I sign off for renewal
- **What would fix it:** Restore a named decline, name both buttons, state a price.

### D-06 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Every file card is an unnamed button wrapping an unnamed link — the file list is unusable without sight. Sidebar naming improved and I'll take it, but I roll out to thousands and I answer for the screen-reader users.
- **JTBD hit:** Safe team-wide rollout
- **What would fix it:** Give each file card the diagram title as its accessible name.

### O-01 · Onboarding
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** "By creating an account, you agree to our Terms of Use and Terms & Conditions" — two legal documents at consent, no Privacy Policy link, while the policy sits in the /pricing footer. That's the line my DPO reads first.
- **JTBD hit:** Compliance
- **What would fix it:** Link the Privacy Policy at consent and say which terms document governs.

### W-06 · Website
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** Fourteen days later the table still says "Up to 6" against the app's "3 of 3 personal" — your wrong number is the one that travels into procurement.
- **JTBD hit:** Standardize on a tool I can defend
- **What would fix it:** Make the compare table read the limit the app enforces.

### D-09 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Two of three slots are empty "Untitled diagram" files from a fortnight ago and nothing flags them — that's how I end up fielding "Mermaid says I'm full" from people who created nothing.
- **JTBD hit:** Rollout that doesn't generate support load for me
- **What would fix it:** Don't count empty diagrams against the cap, or surface "2 empty — delete?".

### D-11 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** The returning dashboard has no memory — no recent activity, no last-opened. If one user's own view can't say what happened, org-wide activity is far off.
- **JTBD hit:** Org-wide visibility
- **What would fix it:** Ship per-user recent activity, then roll it up to the org.

### W-01 / O-04 · Website / Onboarding
- **My read:** Fine
- **Severity for me (1-5):** 2
- **In character:** 42/42 chunks at 200, zero 401s, Turnstile rendering "Verify you are human" — credit where due, the platform stopped being on fire. The bucket policy behind it is still unverified, so the trap stays armed.
- **JTBD hit:** none
- **What would fix it:** Confirm the bucket returns 404, not 401, for missing keys.

### AI-03 · AI
- **My read:** Not my problem
- **Severity for me (1-5):** 2
- **In character:** Two credits spent, two correct diagrams, user left on a blank canvas — that's the Sensemaker's fight. It becomes mine when I'm buying 300 credits a seat and can't show what they bought.
- **JTBD hit:** none
- **What would fix it:** Apply generations to the canvas, and give me an org-level credit usage view.

## What I never saw but needed to
- **Team spaces, Create Organization, Shared with you, search — uncovered in both runs.** Two consecutive pulses with zero governance surface; a "Collapse Team spaces" label is all I know.
- No admin console, roles, permissions, domain restriction, SCIM or audit log anywhere in the evidence.
- No org-wide brand or theme defaults — theming appeared only per-diagram ("Redux"), now removed.
- No export of user activity, no view of who shared what.
- D-10: "New presentation" exists in the product and nowhere on /pricing — an artifact type I'd have to govern with no stated terms.
- Turnstile validation, true cold first-run and the whole logged-out walk unverified.

## One thing I'd tell the team
You fixed the infrastructure and I'll credit that, but my entire world — orgs, roles, audit, brand defaults — has gone two runs untested, so enterprise readiness isn't good or bad, it's unknown.
