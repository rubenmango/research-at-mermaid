# Synthetic User Test — Evidence Log
**Run:** 02
**Run date:** 2026-09-22
**Target:** https://mermaid.ai (production)
**Browser:** Claude built-in browser pane, viewport 1440x900
**Previous run:** 2026-09-08 (Run 01) — findings below are marked FIXED / STILL OPEN / REGRESSED / NEW against it
**Method:** real trace of the real product, then replayed to 6 persona agents

---

## RUN CONDITIONS — read this before reading the findings

**The browser pane was already signed in** (Google SSO, Ruben's own account, Basic plan) from Run 01.
That session cannot be rebuilt unattended — Turnstile and OAuth both need a human — so it was not
discarded. Two consequences, both of which shape coverage:

1. **The logged-out half could not be re-walked in the browser.** `https://mermaid.ai/` redirects an
   authenticated user straight to `/app/dashboard`. The marketing hero, the intent chips and the
   logged-out nav were therefore not re-testable. What *was* verifiable about them was verified from
   the cloud container over plain HTTP (no cookies, different IP) — see W-01.
2. **The account was left at 3 of 3 diagrams by Run 01 and nothing reset it.** Creating a new diagram
   is blocked. No diagram was created or deleted this run. This blocks the "New diagram" latency
   check and the new-diagram empty state — but it delivered, for free, the returning-user-at-cap
   surface that Run 01 listed as not covered.

**8 of the 48 fixed checks were unverifiable this run and are scored as failures.** Named in the
scoring section. Product health is reported as 11/48; the ceiling, if all 8 unverifiable checks
would have passed, is 19/48. Both numbers are stated everywhere the score appears.

No account was created, no password typed, no CAPTCHA attempted.

---

## AREA: WEBSITE

### W-01 — 401 JS chunks / hydration failure — **FIXED**
Run 01's single root cause (22+ `/_web/immutable/chunks/*.js` returning 401, SvelteKit never
hydrating) is resolved. Verified from the cloud container, plain `curl`, no cookies, no browser:
- `GET https://mermaid.ai/` → **200**, 0.51 s, 298,173 bytes.
- Entry chunks **rotated since Run 01** — `entry/app.CRAvuv7K.js` → **`entry/app.B5fklXar.js`**,
  `entry/start.Boyq2AjW.js` → **`entry/start.DdSOKDsT.js`**. The stuck 13:41 UTC build was redeployed.
- All 40 chunk URLs referenced by the homepage HTML, plus both entry chunks: **42/42 return 200**,
  `content-type: text/javascript`. **0 failures.**
- Browser pane console across homepage, `/pricing`, `/app/sign-up`, dashboard and editor:
  **zero 401s, zero `Failed to load resource`.** Run 01 counted 67 cumulative.

This closes W-01 and, by shared cause, W-02 / W-02b / W-02c / W-03 (see below). The bucket-policy ask
from the Run 01 follow-up (return 404 not 401 for missing keys, allow `s3:ListBucket`) is **not
verified as done** — only that no asset is currently missing. The latent trap remains.

### W-02 / W-02b / W-02c — hero prompt box accepts no input — **UNVERIFIED (root cause fixed)**
Not re-testable: `/` redirects an authed session to the dashboard. The hydration failure that caused
it is gone and the pages built from the same app bundle now hydrate fully (`/app/sign-up` exposes a
live form, editor and dashboard are fully interactive), so the cause is fixed. Whether the hero box
itself now accepts text is **unproven this run**. Scored as a failure. One human, ten seconds, in a
logged-out window, closes it.

### W-03 — "What do you need to figure out?" preview panel — **UNVERIFIED**
Same reason. Section still present in the served HTML.

### W-04 — unlabelled primary navigation — **UNVERIFIED**
Logged-out nav not reachable.

### W-05 — cookie consent banner on the signup page — **UNVERIFIED**
No Cookiebot banner appeared on `/app/sign-up` this run, but the pane's persistent profile already
carries a stored consent decision from Run 01, so this is not evidence of a fix. Needs a clean profile.

### W-06 — free-tier diagram limit contradicts itself on /pricing — **STILL OPEN**
Unchanged, verbatim, 14 days later:
- Basic plan **card**: "3 Diagrams"
- Compare-plans **table**, "Number of diagrams" row: Basic = **"Up to 6"**
- In-app truth (dashboard quota chip, this run): **"3 of 3 personal"**
The card is right, the table is wrong, and the table is the surface a user consults when comparing.

### W-07 — Basic allowances vague on the card, specific in the table — **STILL OPEN**
Card "Limited AI" vs table "15". Card "Limited diagram size" vs table "Limited (60)". Unchanged.

### W-08 — dangling asterisk on Enterprise AI credits — **STILL OPEN**
Table still shows "Unlimited*". No footnote anywhere in the page's extracted text. Unchanged.

### W-09 — CTA labels across the price ladder — **CHANGED, still inconsistent (confounded)**
Observed **while signed in on Basic**: Basic = "Get started", Plus = **"Manage plan"**,
Premium = **"Manage plan"**, Enterprise = "Contact sales". Run 01 (logged out) saw Plus =
"Get started for free".
The logged-in state is a confound for comparison — but the observation stands on its own: a **Basic**
user is shown **"Manage plan"** on two tiers they do not have. There is nothing to manage. The
ladder offers a Basic user no way to start a Plus trial from the pricing page, while the dashboard,
the banner and the paywall all push exactly that. Needs a logged-out re-check for the label trend.

### W-10 — 401s site-wide — **FIXED** (see W-01)

### W-11 — /pricing renders clean — **NEW (positive)**
`/pricing` loads and hydrates fully: all four plan cards, the complete compare table, the FAQ
accordion and the footer. Zero console errors. In Run 01 this page contributed ~23 of the 401s.

---

## AREA: ONBOARDING

### O-01 — signup page structure — **STILL OPEN (unchanged), one part FIXED**
`/app/sign-up`, title "Sign up | Mermaid | AI and text based diagramming". Order unchanged:
**Google**, **GitHub**, **SSO**, "— OR —", email form. Helper text "We recommend using your work email".
Fields: Work email, Password, Re-enter password, both with show/hide toggles. Terms line unchanged:
"By creating an account, you agree to our **Terms of Use** and **Terms & Conditions**." Footer
"Already have an account? Sign in".
- **No privacy policy at the point of consent** — STILL OPEN. Two legal documents are named; the
  Privacy Policy exists (it is in the /pricing footer) but is not linked where consent is given.
- The page renders the full create-account form **to an already-authenticated user** — no redirect,
  no "you're already signed in". Minor, new.

### O-02 — form fields carry no text labels — **STILL OPEN**
The a11y tree now reports names — `textbox "Work email"`, `textbox "Password"`,
`textbox "Re-enter password"` — but the screenshot confirms these are **placeholder-only**: grey text
*inside* each box, no persistent label above it. The accessible name is being computed from the
placeholder. Label still disappears on input; still fails WCAG 3.3.2; still worst on password
re-entry, where the user cannot see what the second field is once they start typing.

### O-03 — password re-entry required — **STILL OPEN**
Unchanged. Both fields still carry show/hide toggles, which already solve the problem the confirm
field exists to catch.

### O-04 — Turnstile now initialises on the signup form — **FIXED**
The Cloudflare widget **renders**: a bordered "Verify you are human" control with the Cloudflare mark
sits between the password fields and the "Create account" button, which is disabled until it passes.
In Run 01 the widget never initialised (0 `challenges.cloudflare` iframes, hidden response field only)
and the console repeated `[Cloudflare Turnstile] Error: 600010`. **This run the signup page produced
zero console messages of any kind.**
- **Consequence for the cadence blocker:** Run 01's ticket asked engineering for a Turnstile exemption
  or a staging sitekey. The Run 01 follow-up predicted the 600010 failure was downstream of the
  hydration bug. That prediction looks right. The widget now initialises inside the embedded webview.
  **Whether it validates is untested — completing a CAPTCHA is out of scope.** Do not file the
  exemption ticket yet; ask a human to attempt one email login in the pane first.

### O-05 — guided-hint anchors fail on the empty editor — **UNVERIFIED**
Requires the new-diagram empty state; blocked by the 3/3 cap.

### O-06 — progressive tips (Create → Edit with AI → Share) — **REGRESSED / not observed**
Run 01 recorded both tips firing in sequence, and called this the June report's checklist shipped.
**Neither tip fired this run**, across a first generation and an AI edit in the same diagram. What
appeared instead after the generation was a thumbs-up / thumbs-down feedback pair.
Confounded: the tips may be once-per-account and already consumed on this account in Run 01. Cannot
be separated without a fresh account. Recorded as regressed-or-once-only; do not escalate on this
alone.

### O-07 — the "real time" promise — **STILL OPEN** (see AI-03)

### O-08 — theme picker appears as a bare word ("Redux") — **FIXED**
Typing into the canvas prompt bar no longer surfaces an unexplained "Redux" dropdown. The bar now
shows a "+" attach control on the left and a submit arrow on the right. Nothing else.

### O-09 — control-flow flags in the URL — **UNVERIFIED**
The pane reports a normalised URL for every page, so query strings could not be read this run.

### O-10 — returning user opening an existing empty diagram gets no guidance — **NEW**
Opening a previously-created but empty diagram lands directly on a **blank dotted canvas**. The only
affordance is a one-line bar reading **"Describe your idea"**. There is no "What do you want to
diagram?" heading, no Generate / Paste Mermaid code toggle, no Upload, no "Templates & Diagram types",
no "Start with a blank canvas" — all of which Run 01 recorded on the new-diagram empty state (O-04 of
Run 01).
Scope honestly: this is the *existing-empty-diagram* path, not the *new-diagram* path, and the two
may legitimately differ. But a free user at 3/3 has no new-diagram path left, so this is the only
entry a capped returning user has, and it teaches them nothing.

---

## AREA: AI

### AI-01 — first generation: slower, and invalid on first emission — **REGRESSED**
Prompt (209 chars, canvas prompt bar): *"Map how a user signs up with email, verifies their address,
and reaches their first diagram. Include the failure path where the verification link has expired,
and the branch where the email is already registered."*

| | Run 01 | Run 02 |
|---|---|---|
| Time to rendered output | ~10 s | **~25–28 s** |
| Syntactically valid first emission | yes | **no** |
| Rendered to the canvas | **yes** | **no** (chat preview only) |
| Credits | 15 → 14 | 15 → 14 |

Five seconds after submit the preview showed a spinner over a blurred diagram with the text
**"Repairing diagram… / The Mermaid code contains a syntax error."** and a **Stop** button. The model
emitted Mermaid the renderer could not parse, and a repair pass ran automatically. Repair succeeded;
the final diagram was correct.
- The auto-repair itself is good engineering and it is *visible*, which is better than silent retry.
- But it is on the critical path of the single most important moment in the product, it roughly
  triples time-to-first-diagram, and the word the user reads at that moment is **"syntax error"** —
  in a product whose pitch is that you never have to touch syntax.

### AI-02 — output faithful to the prompt — **STILL GOOD**
Result: a flowchart covering Start → enter email and password → "Email already registered?" →
sign-in prompt / create unverified account → send verification email → user opens link →
"Link valid?" → verify + open first diagram, or expired-link message + resend loop. Every branch
asked for is present. Summary line: *"This flow follows email sign-up through verification to the
user's first diagram. It also handles existing accounts and expired verification links with a resend
loop."* Accurate.

### AI-03 — AI output does not reach the canvas — **REGRESSED (worse than Run 01)**
Run 01: the first generation rendered to the canvas; only subsequent *edits* landed as proposals
behind a button labelled "Edit". **This run neither did.**
- First generation: appeared **only** as a preview card inside the chat panel, with tabs
  Preview / Code and an **"Edit"** button. Canvas remained blank.
- Follow-up edit (*"Add a rate-limit check before the account is created, returning a 429 when the
  signup rate is exceeded."*): credits 14 → 13, response *"Added a rate-limit decision after the
  email-registration check and before account creation. Exceeded requests now return a 429 response."*
  — again a preview card behind **"Edit"**. Canvas still blank.
- Closing the chat panel returns a **completely empty canvas**. Reloading the editor: still an empty
  canvas, title still **"Untitled diagram"**, credits **13**, and **no discoverable control in the
  editor chrome that reopens the AI chat**. The header icons are ⋮, favourite, comments, document and
  version history; the comment icon opens the comments rail, not the chat.
- The dashboard afterwards still shows the file as **"Empty diagram" / "Untitled diagram"**.

Net: **two credits spent, two correct diagrams produced, and the user ends on a blank canvas with no
obvious way back to either.** This is the run's headline defect.

### AI-04 — no pending state while an AI request is in flight — **NEW**
On submitting the chat edit, the user's own message **did not echo into the transcript**, no spinner
appeared, and nothing changed for roughly 40 seconds — while the credit counter had **already**
decremented 14 → 13. The interface says "you have been charged" and shows no sign of work in
progress. (Run 01 recorded a "Loading diagram…" state on the generation path; there is none on the
edit path.) Measured: submit ~08:10:42 UTC, response present by 08:11:22 UTC.

### AI-05 — placeholders for the AI input — **STILL OPEN**
Two observed this run: **"Describe your idea"** (canvas bar) and **"What would you like to change or
add?"** (chat). Run 01 recorded four strings across the same journey.

### AI-06 — accessibility of the AI surfaces — **PARTLY FIXED**
Good: inside the chat, `Preview` / `Code` tabs, `Copy code`, `More actions`, `Fullscreen`, `Edit`,
`Pan`, `Zoom out`, `Zoom in`, `Fit to view`, `Good Response`, `Bad Response`, `New AI Chat` and
`Delete Chat` all expose accessible names. This is a real improvement.
Still unnamed: the chat **send button** (`button [ref_136] type="submit"`, no name), the attach
button, and 5 editor-header icon buttons.

### AI-07 — diagram is never auto-titled — **STILL OPEN**
Run 01: auto-titled "Email Password Flow", though the final title was less accurate than the first.
Run 02: after a generation and an edit the file is still **"Untitled diagram"**. Consistent with
AI-03 — titling appears to happen on apply, and nothing was applied.

---

## AREA: EDITOR

### E-01 — Share dialog — **STILL OPEN (unchanged), works on Basic**
Opened on the Basic account. Dialog title `Share "Untitled diagram"`. Contents, verbatim:
- "Invite people" → "Enter names or emails"
- "Use invite link" → "Copy link" → permission dropdown reading **"Can edit"**
- "Who has access" → ruben Mangorrinha → "Manage"
- "Public link access" → **"No access"**
Public-off default is still right. **Invite link still defaults to "Can edit"** — a copied link grants
edit to anyone who receives it; view is the safer default. Unchanged from Run 01.
The dialog showed **"Loading sharing information…"** for ~5 s before rendering.
Confirms again that **sharing works on Basic**, which the paywall contradicts (see D-04).

### E-02 — canvas control cluster lost its labels — **REGRESSED (moderate confidence)**
On the bare canvas the bottom-right cluster is **6 buttons with no accessible names** (visually:
undo, redo, zoom out, zoom in, fit, fullscreen), plus 5 unnamed header icon buttons — 14 of 24
interactive controls on the empty editor expose no name.
Run 01 recorded the pan/zoom cluster as the one well-labelled group in the product. Confidence is
moderate rather than high because the cluster appears to have **moved** (right rail → bottom-right),
so this may be a new unlabelled control group rather than an existing one losing labels. Note the
same functions *are* correctly labelled inside the chat preview — so the labels exist in one
component and not the other.

### E-03 — not exercised this run
Code panel round-trip, node-level AI, export formats, version history (the control exists in the
editor header), Templates gallery, Mermaid Flow, presentations.

---

## AREA: DASHBOARD & APP

### D-01 — "New diagram" latency — **UNVERIFIED**
Blocked by the 3/3 cap; clicking it opens the paywall instead of creating.

### D-02 — item counter always reads "0 Items" — **STILL OPEN**
Dashboard shows 3 files and a quota chip reading "3 of 3 personal"; the footer reads **"0 Items"**.
Identical to Run 01, in every state observed, across two runs.

### D-03 — in-app cap is 3 — **STILL OPEN** (as a /pricing contradiction; see W-06)
Quota chip: "3 of 3 personal", bar full. The app is the source of truth and says 3.

### D-04 — cap-hit paywall — **REWRITTEN; two defects fixed, three survived, one REGRESSED**
Triggered by "New diagram" at 3/3. Full text, verbatim:
> "Get more diagrams for free / There's more to Mermaid than Basic. Claim your free Plus trial to get:
> / Unlimited diagrams / No size restrictions on complex work / AI diagram generation (300 credits/yr)
> / Collaborate with comments and sharing / Trusted by over 5M users and 200k companies /
> **Trial Plusfree for 7 days. Cancel anytime** / Start Free Trial"
Plus a logo wall: Google, Microsoft, NVIDIA, Atlassian.

- **FIXED** — the "Your one-time 15% offer" framing is gone, and with it the "15% off" with no base
  price and no discounted price. The offer is now simply a free trial, which it always was.
- **FIXED** — the vague "You hit a limit. Plus removes all of them" is gone.
- **STILL OPEN — "Trial Plusfree for 7 days."** The missing space **survived a full rewrite of this
  dialog.** Two runs, same typo, on the revenue surface.
- **STILL OPEN** — no price anywhere. A user cannot learn what Plus costs without leaving the dialog.
- **STILL OPEN** — "Collaborate with comments and sharing" is sold as a Plus benefit while **Share
  works on Basic** (E-01, verified this run on this account).
- **REGRESSED** — Run 01's dialog offered a labelled decline, **"I'll keep my limits"**. That is gone.
  The only way out is a **bare × icon with no accessible name**. The dialog's primary button
  ("Start Free Trial") **also exposes no accessible name**. A screen-reader user meets a modal with
  two unnamed buttons and no labelled exit.
- The new headline, "Get more diagrams for free", no longer tells the user *why* the dialog appeared.
  In context (they just clicked New diagram at 3/3) it is inferable, but the dialog never names the cap.

### D-05 — four labels for one upsell action — **STILL OPEN**
Observed this run: **"Try Plus free"** (dashboard header) · **"Start your free trial"** (cap banner) ·
**"Start Free Trial"** (paywall) · **"Start my trial"** (editor header). Four strings, one destination.
Unchanged in count from Run 01; two of the four strings have changed.

### D-06 — accessibility of dashboard controls — **PARTLY FIXED, one gap widened**
Improved: several sidebar controls now have names — "Collapse Your space", "Actions for Personal",
"Collapse Workflows", "Collapse Favorites", "Collapse Team spaces", "Go to Dashboard", "New diagram",
"More create options", "All files", "Grid view", "List view", "Try Plus free". Run 01's duplicate
"New diagram" / "new diagram" pair is gone.
Still open: 7 unnamed sidebar buttons, and — more seriously — **every file card is an unnamed button
wrapping an unnamed link**. A screen-reader user gets four anonymous controls where the sighted user
sees "Untitled diagram", "Untitled diagram", "Email Password Flow" and "Data to Deployment Pipeline".
The files list is unusable without sight.

### D-07 — stray "Deleting Diagram …" string — **FIXED** (not present in extracted body text)

### D-08 — console hygiene — **PARTLY FIXED**
Gone this run: HTTP 429s, `Error: Not found: /app/diagrams`.
Still present: `[Statsig] Creating multiple Statsig clients with the same SDK key`, Permissions-Policy
`Unrecognized feature: 'web-share'` and `'attribution-reporting'`, CSP report-only `unsafe-eval`
violations.

### D-09 — empty diagrams consume the free cap, permanently — **NEW**
The account sits at **3 of 3** and two of the three files are **"Untitled diagram" with an "Empty
diagram" thumbnail** — created in Run 01, never filled, never expired. They are worth nothing to the
user and they are 2/3 of the free tier.
A free user who clicks "New diagram" three times while deciding what to draw is permanently locked
out of creating, and the only routes out are deleting their own work or paying. Nothing in the
dashboard flags "these two are empty, delete them" and the paywall does not mention it either. This
is the reverse-trial lever converting on an accident rather than on value delivered.

### D-10 — "New presentation" exists and is undocumented — **NEW**
The "More create options" dropdown beside "New diagram" offers **"New diagram"** and
**"New presentation"**. Presentations appear nowhere on `/pricing` — the plan cards and the compare
table describe diagrams only — so whether presentations count against the 3-diagram cap, and what a
Basic user gets, is unstated. Not a defect; an unmapped surface.

### D-11 — returning-user dashboard has no memory of the last session — **NEW**
(Run 01 listed this surface as not covered.) The returning view is the same file grid as a first
visit: no "pick up where you left off", no last-opened, no recent-activity, no indication that the
newest thing is 14 days old. Favorites is an empty explainer box. The one piece of returning-user
state that *is* surfaced is the cap ("3 of 3 personal") and the trial upsell.

---

## AREA: APP / AUTH — quantitative (Mixpanel EU, project 2954792)

### A-01 — SSO login still fails ~98% of the time — **STILL OPEN; the apparent improvement is a windowing artifact**
| Week (start) | `User SSO Login` | `User SSO Login Failed` | Success rate |
|---|---|---|---|
| 2026-08-24 | 15 | 1,017 | 1.5% |
| 2026-08-31 | 22 | 1,112 | 1.9% |
| 2026-09-07 | 19 | 969 | 1.9% |
| 2026-09-14 | 25 | 962 | 2.5% |
| 2026-09-21 (partial) | 3 | 273 | 1.1% |
- **28-day totals: 84 successes / 4,333 failures — 1.9%.** Run 01: 62 / 6,551 — 0.9%.
- Read at face value this looks like a 34% drop in failures. **It is not a fix.** Run 01's window
  included the week of 2026-08-10 with **3,229** failures — roughly triple every other week on
  record. That outlier has now rolled out of the 28-day window. Every non-outlier week in both runs
  sits at **~960–1,120 failures**. The underlying rate is flat.
- The outlier week is itself worth a question nobody has asked: what happened on 2026-08-10.
- Both Run 01 readings remain live and undistinguishable from data: enterprise SSO is broken, or
  `User SSO Login Failed` over-fires (e.g. an "is this an SSO domain?" probe firing a failure for
  every ordinary login attempt). At ~1,000/week against ~5 successes/day, over-firing remains the
  more likely of the two — and if it is over-firing, a genuine enterprise SSO outage would be
  invisible underneath it. **Two runs, no owner, no movement.**

### A-02 — email login errors still track 1:1 with successes — **STILL OPEN, unchanged**
`User Email Login` vs `User Email Login ERROR`, daily, 2026-09-01 → 09-21:
- Sep 1: 405 / 366 · Sep 7: 377 / 415 · Sep 14: 429 / 347 · Sep 15: 463 / 427 · Sep 21: 416 / 428
- 21-day totals: **6,789 successes / 6,506 errors — 0.96 errors per success.**
- Errors exceed successes on 5 of 21 days. Identical shape to Run 01. Same unresolved ambiguity:
  a real ~50% failure rate, or an over-firing error event. Still nobody's ticket.

### A-03 — signup still cannot be segmented by auth method — **STILL OPEN**
Re-probed directly: breaking `User Sign Up @server` down by `area` over 2026-09-14 → 09-21 returns
**exactly one row — `authentication`, 63,295 events.** No provider, method or auth dimension exists.
Login is segmented four ways; signup is not segmented at all. A signup failure isolated to one method
remains invisible by construction. This is the June 2026 report's ranked recommendation #1, now
**two pulses old**.

### A-04 — method mix — **unchanged in shape, up ~12% in volume**
Weekday logins, 2026-09-15 → 09-21: **Google ~15,000 · GitHub ~2,700 · Email ~420 · SSO ~5.**
Run 01 (Sep 1–7): Google ~13,000 · GitHub ~2,300 · Email ~380 · SSO ~3.
Email remains **~2.3%** of logins. Aggregate login volume still cannot function as a smoke alarm for
an auth-path regression.

### A-05 — signup volume healthy and rising — **unchanged**
Daily `User Sign Up @server`, 30 days to 2026-09-21: weekdays **8,000–9,229**, weekends 4,100–5,000.
Window high **9,229 on 2026-09-16**; 2026-09-21 = 9,122. Run 01's weekday band was 7,300–8,900.
**Up roughly 5% run over run, no cliff, no regression.**
Note this cuts against W-01 having ever been a global outage — consistent with the Run 01 revised
reading, and with it now being fixed.

---

## FIXED CHECKLIST SCORING

Checks are frozen so the percentage is a trend. The check names below are recorded explicitly for the
first time this run — Run 01 left them implicit in the evidence, which made re-scoring ambiguous.
**U** = unverifiable this run; scored as a failure.

### Website — Run 01: 2/11 → Run 02: **4/11**
| # | Check | R01 | R02 |
|---|---|---|---|
| 1 | Static assets serve 200 to anonymous visitors | ✗ | **✓** |
| 2 | App hydrates on marketing pages | ✗ | **✓** |
| 3 | Hero prompt box accepts typed input | ✗ | ✗ **U** |
| 4 | Intent-chip preview panel renders | ✗ | ✗ **U** |
| 5 | Primary nav + hero controls have accessible names | ✗ | ✗ **U** |
| 6 | Free-tier diagram limit consistent (card vs table) | ✗ | ✗ |
| 7 | Basic AI credits + diagram size stated on the card | ✗ | ✗ |
| 8 | No dangling footnote markers | ✗ | ✗ |
| 9 | CTA labels consistent across the price ladder | ✗ | ✗ |
| 10 | Compare table fully populated | ✓ | ✓ |
| 11 | Provider deep-link params work | ✓ | ✓ |

### Onboarding — Run 01: 2/10 → Run 02: **2/10**
| # | Check | R01 | R02 |
|---|---|---|---|
| 1 | Cookie consent does not interrupt the signup form | ✗ | ✗ **U** |
| 2 | Form fields have real labels, not placeholders | ✗ | ✗ |
| 3 | No redundant password re-entry | ✗ | ✗ |
| 4 | Privacy policy linked at the point of consent | ✗ | ✗ |
| 5 | Turnstile initialises on the signup form | ✗ | **✓** |
| 6 | Signup page free of console errors | ✗ | **✓** |
| 7 | Empty-editor first screen guides the user | ✓ | ✗ **U** |
| 8 | Guided-hint anchors resolve | ✗ | ✗ **U** |
| 9 | Progressive tips fire in sequence | ✓ | ✗ |
| 10 | Post-generation URL free of control-flow flags | ✗ | ✗ **U** |

### Editor — Run 01: 3/5 → Run 02: **2/5**
| # | Check | R01 | R02 |
|---|---|---|---|
| 1 | Public link off by default | ✓ | ✓ |
| 2 | Invite link defaults to view, not edit | ✗ | ✗ |
| 3 | Canvas tooling present | ✓ | ✓ |
| 4 | Zoom/pan controls have accessible names | ✓ | **✗** |
| 5 | Code round-trip / version history exercised | ✗ | ✗ |

### AI — Run 01: 5/9 → Run 02: **2/9**
| # | Check | R01 | R02 |
|---|---|---|---|
| 1 | First generation renders to the canvas | ✓ | **✗** |
| 2 | Generation completes <15 s with no repair pass | ✓ | **✗** |
| 3 | Output faithful to prompt incl. failure branches | ✓ | ✓ |
| 4 | Credit accounting visible and honest | ✓ | ✓ |
| 5 | Contextual follow-up after first render | ✓ | **✗** |
| 6 | AI edit applies to the canvas | ✗ | ✗ |
| 7 | Diagram auto-titled from content | ✗ | ✗ |
| 8 | One consistent placeholder for the AI input | ✗ | ✗ |
| 9 | AI chat send button has an accessible name | ✗ | ✗ |

### Dashboard & App — Run 01: 2/13 → Run 02: **1/13**
| # | Check | R01 | R02 |
|---|---|---|---|
| 1 | "New diagram" gives immediate feedback | ✗ | ✗ **U** |
| 2 | Item counter accurate | ✗ | ✗ |
| 3 | In-app cap matches the pricing page | ✗ | ✗ |
| 4 | Paywall free of typos | ✗ | ✗ |
| 5 | Paywall states a price | ✗ | ✗ |
| 6 | Paywall names the limit that was hit | ✗ | ✗ |
| 7 | Paywall benefit claims accurate | ✗ | ✗ |
| 8 | Paywall offers a labelled decline | ✓ | **✗** |
| 9 | Banner messaging changes at cap | ✓ | ✓ |
| 10 | One consistent label for the upsell action | ✗ | ✗ |
| 11 | Sidebar/nav controls have accessible names | ✗ | ✗ |
| 12 | File cards expose accessible names | ✗ | ✗ |
| 13 | App shell console clean | ✗ | ✗ |

### TOTAL — Run 01: **14/48 (29%)** → Run 02: **11/48 (23%)**
Movement: Website **+2**, Onboarding **0**, Editor **−1**, AI **−3**, App **−1**.
**8 checks were unverifiable and scored as failures.** If all 8 had passed the score would be
**19/48 (40%)**. The true figure is somewhere in 11–19; the honest statement is that the
infrastructure got better, the AI got worse, and the run could not see a sixth of the checklist.

---

## NOT COVERED THIS RUN — state plainly in every output
- **The entire logged-out walk in a real browser**: hero prompt box, intent chips, logged-out nav,
  cookie banner behaviour. Cause: the pane holds a session that cannot be rebuilt unattended, and `/`
  redirects authenticated users to the dashboard.
- **New-diagram creation, its latency, and the new-diagram empty state** (Templates & Diagram types,
  Upload, Generate/Paste toggle). Cause: the account is at 3/3 and no diagram was deleted.
- True cold first-run after signup. Turnstile validation in the embedded webview.
- Export, version history, code round-trip, node-level AI, comments, Mermaid Flow, presentations.
- Team spaces / Create Organization / Shared with you / search (the Steward's and Consumer's world) —
  **not covered in either run.** Two of the six personas have now had no surface to react to, twice.

## HOW TO UNBLOCK THE NEXT RUN
1. **Delete the two empty "Untitled diagram" files** on Ruben's account before Run 03, or give the
   pulse its own account. That alone restores the new-diagram path, D-01, the empty state, Templates,
   and the progressive tips.
2. **One human, ten seconds, logged out**: does the hero box accept text? Closes W-02/W-03/W-04.
3. **One human, one email login in the pane**: does Turnstile now validate? Decides whether the
   Run 01 exemption ticket is needed at all.
