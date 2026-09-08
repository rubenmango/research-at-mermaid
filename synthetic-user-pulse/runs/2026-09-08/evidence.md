# Synthetic User Test — Evidence Log
**Run date:** 2026-09-08
**Target:** https://mermaid.ai (production, logged out)
**Browser:** Claude built-in browser pane, viewport 1440x900, cookie consent = Deny (all non-necessary)
**Method:** cold walk of the real flow; captured trace is then replayed to 6 persona agents

---

## AREA: WEBSITE (marketing site, logged out)

### W-01 — 22 JS chunks return HTTP 401 to anonymous visitors
- Load 1 of `https://mermaid.ai/`: 22 console errors, all `Failed to load resource: 401`.
- Load 2 (hard navigate): 22 more (44 cumulative). **Deterministic, reproduces.**
- All failures are `GET https://mermaid.ai/_web/immutable/chunks/<hash>.js -> 401 [net::ERR_ABORTED]`.
- Sample failing chunks: `BxH4YZfE.js`, `Cl-0LJ7h.js`, `cXJly0UV.js`, `CmD_9AiI.js`, `FjFeGuPG.js`,
  `DnD_mP1D.js`, `m6bXvShp.js`, `CMLSglfb.js`, `B1kBjBl6.js`, `C-lfAdkV.js`, `C15USdGo.js`,
  `uQGBGtIx.js`, `CSzw3Pjw.js`, `njhfVCGB.js`, `Dvky5JfX.js`, `BckqbxTO.js`, `Bfc3Dvb5.js`,
  `DpNeU2OY.js`, `Bah_7IAA.js`
- **Notable:** other chunks in the *same* `/_web/immutable/chunks/` directory return 200 in the same
  page load. Mixed 200/401 from one immutable-asset path argues against a blanket bot-challenge and
  toward broken/partial asset serving.
- Response bodies not retrievable (ERR_ABORTED).
- **Confound to rule out:** Cloudflare Turnstile is present (`cf.turnstile.u` in the cookie
  disclosure). An automated browser *could* in principle be challenged. Counter-evidence: mixed
  200/401 in the same directory, and the HTML document itself served 200. NEEDS one human eyeball in
  a normal browser to close.

### W-02 — Hero prompt box accepts no input (primary conversion surface)
- Hero copy: "Prompt inside the canvas, drop in a doc, or write it in code — and watch the diagram
  build itself."
- Clicked the box, typed 93 chars ("Map how a user signs up, verifies their email, and gets to their
  first diagram in our product"). **Nothing rendered. Box stayed empty.**
- `read_page filter=interactive` returns NO input/textarea element anywhere on the page.
- `find "textbox"` -> "No matches for textbox."
- Interactive elements found on the whole homepage: 12, of which the only hero-area ones are
  `button [ref_9]` (unlabelled submit arrow), `link "Start free"`, `link "Google"`, `link "GitHub"`.
- **Likely root cause:** W-01. The hero input's hydration code is in a 401'd chunk.
- Severity: the page's single largest CTA is inert and silently swallows typed input.

### W-02b — DOM-level proof, and the bot-challenge confound closed
Ran in page context on `https://mermaid.ai/`:
```
inputCount (input,textarea) : 0
[contenteditable] elements   : 0
cf-turnstile-response fields : 0
[data-svelte-h] present      : false   <- page is NOT hydrated
```
- **Zero** inputs, textareas or contenteditable nodes exist in the homepage DOM. The hero "prompt
  box" is a plain container. It cannot receive text by any means.
- **Turnstile is not even loaded on the homepage** (0 turnstile fields), so the 401s are not a bot
  challenge on this page. Turnstile *is* present on `/app/login` (`cf-turnstile-response` hidden
  input) — i.e. it guards the auth forms, not static assets.
- `data-svelte-h` absent = SvelteKit never hydrated, consistent with W-01 killing the chunks.
- **Remaining open question (needs a human, 10 seconds):** the 401s are server responses, so they
  should hit every visitor — but I cannot prove from one client that they aren't specific to this
  browser/IP. If the hero box accepts text in a normal Chrome, W-01/W-02 are client-specific and
  get downgraded. If it doesn't, this is a production bug affecting every anonymous visitor.

### W-02c — Broken on mobile viewport too
- 375x812, reloaded so device gates re-run: still `inputCount 0`, no textbox in the a11y tree.
- The "Paste" affordance visible in the hero at narrow widths is a `generic` node, **not a button**
  — it is not interactive either. At 1440px wide the "Paste" affordance is not rendered at all.

### W-03 — "What do you need to figure out?" preview panel renders empty
- Section offers 12 intent chips + 14 diagram-type chips.
- Clicked chip "Plan my project" -> chip visibly selects (active state works).
- The companion preview panel to its left is a **blank bordered box**. Still blank after a 3s wait.
- So chip selection produces no visible output. Second symptom of the same hydration failure.

### W-04 — Accessibility: unlabelled primary navigation
- Top nav exposes `menuitem [ref_2]` .. `[ref_6]` with **no accessible names**.
  (Visually: Products, Solutions, Pricing, Community, Open Source, Docs.)
- Hero submit control is `button [ref_9]` with no accessible name.
- Intent/diagram chips use `role="radio"` with `type="button"` — radio semantics on a
  non-radiogroup pattern; announced as radios to screen readers.
- Combined with W-02 (no text input in the a11y tree), the hero is unusable by keyboard/AT.

### W-05 — Cookie consent banner fires on the signup page, not the homepage
- Homepage load: no consent banner observed.
- Navigating to `/app/sign-up`: Cookiebot banner appears, overlaying the form.
- All three optional categories (Preferences, Statistics, Marketing) **default to ON** in the
  Consent Selection UI; "Deny" is present. Banner interrupts at peak signup intent.

---

## AREA: ONBOARDING (signup, logged out portion)

### O-01 — Signup page structure
- URL: `/app/sign-up`. Title: "Sign up | Mermaid | AI and text based diagramming".
- Options, in order: **Google**, **GitHub**, **SSO**, then "─ OR ─", then email form.
- Helper text above the email field: "We recommend using your work email".
- Fields: work email, password, re-enter password (both with show/hide toggles).
- Terms: "By creating an account, you agree to our Terms of Use and Terms & Conditions."
  (Two separate legal docs named; no privacy policy linked at the point of consent.)
- Footer: "Already have an account? Sign in".
- Deep-link params work: `/app/sign-up?provider=google`, `?provider=github` from the homepage.

### O-02 — Signup form fields carry no text labels
- Page text extraction returns the helper copy and buttons but **no field labels** — fields appear
  to be placeholder-only ("Work email", "Password", "Re-enter password" seen visually).
- Placeholder-as-label: label disappears on input, fails WCAG 3.3.2, and is a known error-recovery
  problem on password re-entry.

### O-03 — Password re-entry required
- Two password fields with a confirm step, despite show/hide toggles being present on both.
- Adds a field to the highest-drop-off form in the funnel; the show/hide toggle already solves the
  typo problem it exists to catch.

---

## AREA: WEBSITE — /pricing

### W-06 — Free-tier diagram limit contradicts itself on the same page
- Basic plan **card**: "3 Diagrams"
- Compare-plans **table**, "Number of diagrams" row: Basic = **"Up to 6"**
- Verified visually in the table screenshot: `Up to 6 | Unlimited | Unlimited | Unlimited`.
- Two different free-tier caps on one page. This is the exact number that gates the reverse-trial
  conversion lever, so it is not a cosmetic copy bug.

### W-07 — Basic AI allowance is vague on the card, specific in the table
- Basic card: "Limited AI". Table, "AI credits" row: Basic = **15**.
- Same pattern on diagram size: card "Limited diagram size" vs table "Limited (60)".
- The card hides the two numbers a free user most needs to judge the tier.

### W-08 — Dangling asterisk on Enterprise AI credits
- Table shows Enterprise AI credits as **"Unlimited*"**.
- No corresponding footnote appears anywhere in the page's extracted text.

### W-09 — CTA labels inconsistent across the price ladder
- Basic (free): "Get started"
- Plus (€8/user/mo): "Get started for **free**"
- Premium (€16/user/mo): "Get started"
- Enterprise: "Contact sales"
- The paid mid-tier carries a more free-sounding CTA than the actually-free tier. Either Plus has a
  trial the ladder never explains, or the label is wrong.

### W-10 — 401 chunk failures are site-wide, not homepage-only
- `/pricing` load pushed cumulative console errors from 44 to **67** (~23 more 401s).
- So W-01 is a site-wide asset-serving failure, not one broken page.

### VERIFIED NOT A FINDING (recorded so it isn't re-raised)
- The Enterprise column of the compare table is **correctly populated**. Text extraction shows
  dashes only, which reads like an empty column, but the screenshot confirms green check *icons*
  (e.g. "User consolidation" = `- | - | - | ✓`, "Co-editing & external sharing" = `✓ ✓ ✓ ✓`).
  Icons don't extract as text. No bug here.

---

## AREA: APP / AUTH — quantitative (Mixpanel EU, project 2954792)

### A-01 — SSO login succeeds ~1% of the time  [SEVERITY: highest of the run]
| Week (start) | `User SSO Login` | `User SSO Login Failed` | Success rate |
|---|---|---|---|
| 2026-08-10 | 12 | 3,229 | 0.4% |
| 2026-08-17 | 12 | 983 | 1.2% |
| 2026-08-24 | 15 | 1,017 | 1.5% |
| 2026-08-31 | 22 | 1,112 | 1.9% |
| 2026-09-07 (partial) | 1 | 210 | 0.5% |
- **28-day totals: 62 successes / 6,551 failures.**
- SSO is the headline Enterprise-tier differentiator on `/pricing`.
- **Two readings, both serious:** (a) enterprise SSO is broken, or (b) `User SSO Login Failed`
  over-fires — e.g. an "is this email an SSO domain?" probe logging a failure for every ordinary
  user — in which case enterprise auth telemetry is unreadable and (a) would be undetectable.
- Volume (6.5k failures/28d) against a presumably small enterprise SSO population leans toward
  over-firing. NOT resolved from data alone; needs the owning engineer.

### A-02 — Email login error events track 1:1 with successes
`User Email Login` vs `User Email Login ERROR`, daily:
- Aug 18: 343 / 359 · Aug 19: 360 / 417 · Aug 25: 436 / 351 · Sep 1: 405 / 366 · Sep 7: 377 / 415
- Errors *exceed* successes on many days; sustained across the full 21-day window.
- Same ambiguity as A-01: genuine ~50% failure rate, or an over-firing error event.

### A-03 — The official signup event cannot be segmented by auth method
- `User Sign Up @server` (description: "Official event for user sign up") property list contains
  no provider/method/auth dimension. Only meaningful custom property is `area`, whose complete
  value set is a single value: `"authentication"`.
- Login *is* segmented (`User Email Login`, `User Login: google`, `User Login: github`,
  `User SSO Login`). Signup is not.
- Consequence: a failure isolated to one signup method is invisible in signup data by construction.
- Ties directly to the June 2026 report's ranked recommendation #1 (instrument the success event).

### A-04 — Method mix, for prioritization
Logins per weekday (approx, Sep 1-7): **Google ~13,000 · GitHub ~2,300 · Email ~380 · SSO ~3**
- Email is ~2.3% of logins. So a broken email path cannot move aggregate signup/login numbers —
  aggregate volume is not a usable smoke alarm for auth-path regressions.

### A-05 — Signup volume is healthy; no regression visible
- Daily `User Sign Up @server` (30d): weekdays ~7,300-8,900, weekends ~3,400-5,700.
  Sep 7 = 8,849 (window high). Gently rising through late Aug into Sep.
- Hourly for Sep 5-8 shows no cliff; Sep 8 running 287-558/hour through 07:00.
- **This is the evidence that downgrades W-01/W-02** from "global outage" to "affects a subset".

### REVISED READING OF W-01/W-02 (supersedes the initial severity call)
The 401s reproduce from two independent clients, yet email auth is completing for real users every
hour. Most consistent explanation: **deploy skew** — edge-cached HTML referencing chunk hashes that
have since rotated, with the origin answering **401 instead of 404** for unknown asset paths.
Visitors served stale HTML from an affected edge get: dead hero box, blank chip preview panel, and a
Turnstile widget that never initializes (so email signup fails for them). Everyone else is unaffected.
- Real bug, unknown blast radius, silent for the user (no error shown — the box just eats input).
- The 401-instead-of-404 response code is itself worth fixing: it defeats cache-busting and makes
  the failure look like an auth problem.
- Open: what share of visitors hit an affected edge. Not answerable from the data available here.

---

## ACCOUNT ACCESS — how the logged-in half happened
- Account creation / password entry / CAPTCHA completion are not things I perform.
- Ruben created a burner account (`@fidhost.com`) in his own browser — signup **worked** for him
  (3rd independent confirmation the 401 skew is a subset problem). He then tried email login in the
  browser pane: **Cloudflare Turnstile hard-failed** — `[Cloudflare Turnstile] Error: 600010`
  repeated in console, UI showed "Failed to validate. Please check your internet connection and try
  again." Robustness note: Turnstile fails inside embedded Chromium (desktop-app webview). Same
  class of environment as locked-down corporate browsers and in-app webviews.
- Resolution: Ruben signed in with **Google SSO using his own account** in the pane (OAuth path,
  no Turnstile). He deleted his 2 existing diagrams first (two `DELETE /rest-api/documents/...`
  seen in network). So the logged-in half is a **WARM START on an existing account with an empty
  personal space** — not a true first-run. No onboarding wizard fired; user record is old.
- **Blocker for the biweekly cadence:** every future run needs a human past Turnstile unless eng
  provides (a) Turnstile off for a flagged test account / test email domain, or (b) a staging URL
  on Cloudflare's always-pass testing sitekey. Ticket for App team.

---

## AREA: DASHBOARD (App team) — `/app/projects/<id>` "Personal"

### D-01 — "New diagram" gives zero feedback for ~3 seconds
- Primary CTA click -> navigation to editor measured at **3,465 ms** and **2,773 ms** (two trials).
- No spinner, no disabled state, no optimistic UI. Button stays fully interactive the whole time.
- A first-time user will click again. (Both my first two real clicks also appeared to do nothing;
  only a later click produced the `POST /rest-api/projects/<id>/documents`.)

### D-02 — Item counter always reads "0 Items"
- With 1 diagram visible + quota "1 of 3 personal": footer says **"0 Items"**.
- With 3 diagrams visible + quota "3 of 3": still **"0 Items"**. Same at 2 earlier.
- Counter is wrong in every state observed.

### D-03 — Diagram cap is 3 (in-app truth); pricing table's "Up to 6" is the wrong one
- Quota chip: "0 of 3 personal" -> "3 of 3 personal". Resolves W-06 in favour of **3**.

### D-04 — Cap-hit paywall: copy defects on a revenue surface
Triggered by "New diagram" at 3/3 (dialog appears in ~1.5s). Full text:
> "Your one-time 15% offer / You hit a limit. Plus removes all of them – and right now you can lock
> in 15% off with a free trial for 7 days. / Unlimited diagrams / No size restrictions on complex
> work / AI diagram generation (300 credits/yr) / Collaborate with comments and sharing / Trusted by
> over 5M users and 200k companies / Trial Plusfree for 7 days. Cancel anytime / I'll keep my limits
> / Start my free trial"
- **Typo: "Plusfree"** (missing space) on the paywall.
- **No price shown anywhere** — "15% off" with no base or discounted number. User cannot evaluate.
- "You hit a limit" — does not say *which* limit. The user just saw "3 of 3"; say it.
- "Collaborate with comments and sharing" listed as a Plus benefit — but **Share works on Basic**
  (dialog opened on this Basic account: invite people, invite link, public link). Misleading claim.
- Dashboard banner copy does change at cap: "Basic plan with limited editable diagrams" ->
  "**Start your free trial** for unlimited editable diagrams". Good progressive messaging.

### D-05 — Four labels for one upsell action
"Try Plus free" (dashboard button) · "Start my trial" (editor header) · "Start your free trial"
(cap banner) · "Start my free trial" (paywall). Same destination, four strings.

### D-06 — Accessibility: unlabelled controls throughout the dashboard
Sidebar nav items (Dashboard / Personal / Shared with you / Mermaid Flow) expose as `button` with
no accessible name; 8+ unnamed buttons on the page. Two CTAs read "New diagram" and "new diagram".

### D-07 — Stray "Deleting Diagram ..." string present in dashboard DOM text
Observed in extracted body text on first load (during Ruben's deletes). A status string leaking into
the document rather than a transient toast — minor.

### D-08 — Console hygiene (App shell)
`[Statsig] Creating multiple Statsig clients with the same SDK key` · Permissions-Policy
`Unrecognized feature: 'web-share'` · CSP report-only `unsafe-eval` violations · intermittent
**HTTP 429** on an app resource (3 occurrences in one session) · `Error: Not found: /app/diagrams`
(the login `?redirect=` param faithfully preserved a non-existent route and 404'd before recovering).

---

## AREA: ONBOARDING (in-editor first-run, warm account)

### O-04 — Empty-editor first screen is clear
"What do you want to diagram?" · toggle **Generate** / **Paste Mermaid code** · prompt box
"Describe it, or paste anything here." · **Upload** · secondary: "Templates & Diagram types",
"Start with a blank canvas". Header: Export · Share · **15 Credits** · "Start my trial". Good.

### O-05 — Guided-hint anchors fail on the empty editor
Console on first editor load: `Reference element not found for template-toolbar` and
`Reference element not found for ai-chat-btn`. The tour tried to attach to elements that don't
exist in the empty state.

### O-06 — Progressive tips are live and sequenced (positive)
After first generation: "**Keep editing with AI** — Describe your next change in the chat and watch
the diagram update in real time. [Got It]". After first applied edit: "**Built something good? Show
it off** — Share a live link for teammates to view or edit, or export as PNG, SVG, or PDF. [Got it]".
This is the Create -> Edit with AI -> Share checklist from the June report, shipped. It also answers
that report's open question #2: Share = live link (view/edit) or PNG/SVG/PDF export.

### O-07 — The "real time" promise is false (see AI-03)
The tip says edits update the diagram in real time. They don't; they land as proposals in chat.

### O-08 — Theme picker appears as an unexplained word
Once text is entered, a dropdown labelled only "**Redux**" appears beside the submit arrow. No
"Theme:" prefix, no icon that reads as theme. Later the canvas shows "Redux Color theme" — so the
concept exists, but the first encounter is a bare word.

### O-09 — Control-flow flags leak into the URL
Post-generation URL carries `?shouldShowPopup=true&entryPoint=Dashboard`. Harmless, but sloppy
and copy-paste-able.

---

## AREA: AI

### AI-01 — First generation: fast, correct, honest credit accounting (positive)
Prompt: email-signup sequence with verification + expired-link failure path (287 chars).
Submit -> "Loading diagram..." -> full render in **~10 s**. Output: `sequenceDiagram` with Browser /
API / EmailService / Database, `alt [Email already exists]`, `alt [Token expired] / [Token valid]`,
a "User checks email" note. Faithful to the prompt including the failure branch.
Credits **15 -> 14**, visible immediately. Auto-title: "Email Password Flow" (tab briefly read
"Email Signup Verification" then changed — the final title is less accurate than the first).

### AI-02 — Contextual follow-up after first render (positive)
"Would you like to add any additional details, such as password strength validation, retry limits
for expired links, or specific error messages to display to the user?" — shown AFTER output, which
is exactly the ordering the June report recommended (Area 02 tweak). Landed.

### AI-03 — AI edits do NOT apply to the canvas; they are proposals labelled "Edit"
Chat prompt: "Add a rate-limit check in the API before creating the user, and return a 429..."
- Response in ~9 s, credits **14 -> 13**, "Summary: Here's the updated signup sequence with an API
  rate-limit check that returns HTTP 429 before user creation." + 3 suggested next edits.
- **Canvas SVG unchanged** (29 text nodes, no "429"). The updated diagram (34 text nodes, with
  `alt [Rate limit exceeded]`, "Check signup rate limit", "429 Too Many Requests") existed **only
  as a preview inside the chat.**
- Clicking the preview's **"Edit"** button applied it to the canvas (verified: canvas now has 429).
- So: the apply action is called "Edit", the onboarding tip promised "real time", and the credit is
  spent before the user knows whether it took. A Sensemaker would read this as "the edit failed".
- The updated diagram itself was correct and well-structured.

### AI-04 — Three placeholders for one AI input
"Describe it, or paste anything here." (empty state) · "Describe what to add or change" /
"What would you like to change or add?" (chat) · "Describe your idea" (canvas bar after apply).

### AI-05 — Layout jump on first chat use
Sending the first chat message switched the editor from full canvas to a split view ("Your Diagram"
small on the left, chat dominant on the right). Applying the edit closed the panel and restored
full canvas. Disorienting on first encounter; not flagged as a bug.

### AI-06 — Accessibility: the AI chat's send button has no name
Two 32px icon buttons in the chat bar (attach, send) — neither has an `aria-label`. Editor header
also has 6+ unnamed icon buttons; the Generate/Paste toggle buttons are unnamed.

---

## AREA: EDITOR

### E-01 — Share dialog (positive, one governance nit)
"Invite people" (names/emails) · "Use invite link — Copy link — **Can edit**" · Who has access ·
"Public link access — **No access**". Public-off default is right. **Invite-link default is
"Can edit"** — a copied link grants edit by default; view would be the safer default (Steward).
Homepage promises "share with one link (no login required)" — that's the public link, which is off
by default and needs a deliberate toggle. Not wrong, but the promise reads as the default.

### E-02 — Editor canvas: sequence tooling present
Left rail: "Participant", "Auto number". Right rail: Pan / Zoom in / Zoom out / 1:1 / Fit to view —
these DO have aria-labels (the only well-labelled cluster found). "Show code" toggle + "Sequence"
type badge. Voice (mic) icon on the canvas prompt bar.

### E-03 — Not exercised this run
Code panel round-trip (click node -> code), node-level AI, templates gallery, export formats,
version history. Return-session dashboard (`/app/dashboard`) not reached — device disconnected.

---

## NOT COVERED THIS RUN (state plainly in the report)
- True cold first-run after signup (Turnstile blocks automation; account was warm).
- Returning-user dashboard / "memory delta" between sessions.
- Templates & Diagram types entry point. Mermaid Flow (workflows). Export. Version history.
- Team spaces / Create Organization (Steward's world).

---

## Screens captured (stills, in-context)
1. Homepage hero, narrow viewport (800px) — shows a "Paste" affordance in the box.
2. Homepage hero, 1440x900 — "Paste" affordance absent at desktop width; box empty, no placeholder.
3. Homepage hero after typing 93 chars — unchanged, box still empty.
4. "What do you need to figure out?" with chip selected + empty preview panel.
5. `/app/sign-up` full page.
