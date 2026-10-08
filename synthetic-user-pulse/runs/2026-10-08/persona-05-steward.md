# Persona 05 — The Knowledge Steward

## Verdict
Blind spot

## Would I start the trial after this session?
Not yet — I never reached an admin console, an audit log, or one org-wide setting, so there's nothing here I can take to my security team.

## Three words I'd use to describe the product after this run
Ungoverned, improving, unaccountable

## Brand voice read
Marketing now talks like a finished enterprise product — *"You already think in systems. Now you can diagram them just as fast."*, *"Keep your data yours"* — then hands me a signup page linking **Terms of Use** and **Terms & Conditions** but no privacy policy, and a paywall still reading *"Trial Plusfree for 7 days."* three runs on. Worse, the two live signup variants (O-11) word consent differently: *"By creating an account, you agree to…"* vs *"By continuing, you agree to…"*. One product, two legal voices.

## Reactions

### A-01 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** 76 successes against 3,875 failures over four weeks, flat for three runs, no owner — either enterprise SSO is broken or the error over-fires and a real outage hides underneath it, and both end my security review.
- **JTBD hit:** Access controls match what IT expects
- **What would fix it:** Name an owner and instrument `User SSO Login Failed` so an outage is distinguishable from a domain probe.

### A-03 · Auth/App
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Signup breaks down to exactly one row — `authentication`, 49,044 events, no provider, no method — so when legal asks how our people get in, I have nothing.
- **JTBD hit:** See what's happening org-wide
- **What would fix it:** Add an auth-method property to `User Sign Up @server`; it's been recommendation #1 since June.

### AI-04 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** I'm the billing admin: a credit was consumed this run and no counter exists anywhere in the editor, after Runs 01 and 02 both watched it tick 15 → 14 → 13. You don't get to charge my org for something you won't display.
- **JTBD hit:** Controllable, visible spend
- **What would fix it:** Restore the counter and add an org-level consumption view.

### E-01 · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** "Use invite link" → "Copy link" → **"Can edit"**, third run running; "Public link access: No access" is right and I'll credit it, but the default that actually leaks is the one you left on.
- **JTBD hit:** Granular sharing controls
- **What would fix it:** Default invite links to view and let an admin lock the ceiling org-wide.

### D-12 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** "Earn $30", a Mermaid Flow BETA banner and **"Start voice chat"** all arrived with no mention on `/pricing` — undeclared surfaces, one of them a new input modality, are exactly what I get punished for.
- **JTBD hit:** Enforce / lock down
- **What would fix it:** Give admins feature toggles covering beta surfaces, referral and voice before they ship.

### AI-09 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Run 02's output now renders as *"Unable to render diagram / There's a syntax error in the Mermaid code"* — so chat history persists sixteen days, holds billed content, and rots; where does it live, how long is it kept, can I purge it?
- **JTBD hit:** Data residency and retention
- **What would fix it:** Publish AI chat retention and give admins export and deletion.

### O-11 / O-01 · Onboarding
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Two signup pages at one URL with different consent strings and neither linking the privacy policy — which exists, in the `/pricing` footer, just not where consent is given.
- **JTBD hit:** Compliance
- **What would fix it:** Link the policy in both variants and log which variant each user accepted.

### W-06 / D-04 · Website
- **My read:** Friction
- **Severity for me (1-5):** 4
- **In character:** Card says "3 Diagrams", grid says "Up to 6", app says "3 of 3 personal", and the paywall sells "Collaborate with comments and sharing" while sharing demonstrably works on Basic — procurement reads these pages literally.
- **JTBD hit:** Standardize; defend the purchase internally
- **What would fix it:** One source of truth behind `/pricing`, the paywall and the quota chip.

### W-13 / W-12 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Our procurement pack needs a VPAT, and the compare grid gives a screen reader "-" for absent features and **silence** for present ones, while the rebuilt hero's primary textbox shipped unnamed.
- **JTBD hit:** Approved-vendor list
- **What would fix it:** Text alternatives in the grid cells; name the hero controls.

### AI-08 · AI
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** A correct 1,400-character flowchart in under ten seconds, and "Edit" wrote back one line — `flowchart TD` — discarding every node and edge, then blamed the user's syntax; I don't draw, but silent data loss is something I'd answer for.
- **JTBD hit:** none — trust
- **What would fix it:** Fail loudly instead of writing a truncated buffer to the code panel.

### D-06 · Dashboard
- **My read:** Delight
- **Severity for me (1-5):** 1
- **In character:** 4 of 58 controls unnamed, down from every file card anonymous, moves a11y from "no" to "nearly" — and W-02, W-03, W-04, AI-01 and AI-10 landed too; credit where it's due.
- **JTBD hit:** Approved-vendor list
- **What would fix it:** nothing needed

## What I never saw but needed to
- **Team spaces, Create Organization, Shared with you, search** — uncovered in all three runs; no evidence an admin console exists.
- **Any org-wide brand or theme default** — never exercised; I can't say whether brand controls stick past one diagram.
- **Audit log, activity export, SSO/SCIM config, domain restriction, roles and permissions** — absent from the log entirely.
- **Export** (E-05) is visible but unexercised, so no evidence of what leaves the building — same for comments, version history, presentations, Mermaid Flow and voice chat.
- **Turnstile in the new variant** (O-04) and **a clean first-visit EU cookie banner** (W-05) were unverifiable; I did not see them, nor a true cold first-run, nor whether presentations count against the cap (D-10).

## One thing I'd tell the team
Three runs in, the only numbers about my half of the product — 1.9% SSO success, signup with no auth dimension — still have no owner, and I still haven't been shown one setting I could apply to anyone but myself.
