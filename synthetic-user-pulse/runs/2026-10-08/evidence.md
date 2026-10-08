# Synthetic User Test — Evidence Log
**Run:** 03
**Run date:** 2026-10-08
**Target:** https://mermaid.ai (production)
**Previous run:** 2026-09-22 (Run 02) — findings marked FIXED / STILL OPEN / REGRESSED / NEW against it
**Method:** real trace of the real product, then replayed to 6 persona agents

---

## RUN CONDITIONS — read this before reading the findings

**Two instruments were used this run, and every finding below names which one saw it.**

1. **Browser pane** (Claude built-in browser on Ruben's machine, EU IP, viewport 1440x900).
   It still holds the Google SSO session from Run 01 — that session cannot be rebuilt
   unattended, so it was not discarded, and `https://mermaid.ai/` still redirects to
   `/app/dashboard`. The pane therefore covered the **logged-in** half only.
2. **Headless Chromium in the cloud container** (Playwright, fresh profile, no cookies,
   viewport 1440x900, non-EU IP). This is new in Run 03 and it is how the **logged-out**
   half was recovered after two runs of losing it. It is a real browser executing the real
   app bundle — not `curl` — so hydration, typed input, clicks and console are all genuine.

**What the second instrument cannot tell you.** Its egress proxy blocks
`googletagmanager.com` and `challenges.cloudflare.com`, so Turnstile and GTM failures there
are **instrument artifacts and are not reported as product defects**. Its IP is non-EU, so
the absence of a Cookiebot banner there is **not** evidence the banner was removed — the pane
confirms Cookiebot still loads on the EU IP.

**The account was still at 3 of 3 diagrams.** Run 02's unblock ask — delete the two empty
"Untitled diagram" files — was not done. New-diagram creation, its latency and the
new-diagram empty state are blocked for a **second consecutive run**.

**5 of the 48 fixed checks were unverifiable this run and are scored as failures** (down from
8). Product health is reported as **16/48**; the ceiling if all 5 unverifiable checks would
have passed is **21/48**. Both numbers are stated everywhere the score appears.

**Side effect to declare.** No diagram was created or deleted. One AI generation was run
inside an existing empty "Untitled diagram" (`22a2940c…`), and its "Edit" button was clicked.
That left the diagram's code panel holding the single line `flowchart TD` where it previously
held nothing. The file was already empty and remains empty; no user content was lost.

No account was created, no password typed, no CAPTCHA attempted.

---

## AREA: WEBSITE

### W-01 / W-10 — 401 JS chunks / hydration failure — **FIXED, now confirmed against a real anonymous browser**
Run 02 could only verify this with `curl`. This run a real anonymous Chromium loaded
`/`, `/pricing` and `/app/sign-up` end to end:
- `GET /` → **200**, DOM-content-loaded **1.44 s**, settled **6.4 s**, title "Mermaid".
- `/pricing` → **200**, **3.19 s**.  `/app/sign-up` → **200**, **5.32 s**.
- **Zero 401s on any static asset, on any of the three pages.** The only 401 anywhere is
  `GET /rest-api/users/me` — the expected anonymous auth probe, not an asset.
- The app hydrates: the hero editor mounts, chips respond to clicks, the pricing accordion
  and compare grid render. Run 01 counted 67 cumulative 401s and no hydration at all.

### W-02 — hero prompt box accepts no input — **FIXED** (open since Run 01, unverified in Run 02)
The hero box is a TipTap/ProseMirror `div[contenteditable="true"][role="textbox"]`,
666 × 96 px. It **mounts, focuses and accepts typed text**: typing `Map our checkout flow`
produced `innerText === "Map our checkout flow"` with the element still focused.
Its placeholder is a typewriter animation that completes to *"Show me a system diagram where
a React frontend talks to an API gateway and connects to three microservices on AWS Lambda."*
— sampled mid-animation it looks truncated; it is not a defect.
**This closes the single biggest coverage hole of Runs 01 and 02.**

### W-03 — "What do you need to figure out?" preview panel — **FIXED**
The section renders with **12 intent chips** (Plan my project · Organize my thoughts ·
Explain a complex idea · Design a system · Map out my team · Brainstorm ideas · Show my
startup roadmap · Design a database · Teach a lesson · Show system interactions · Model object
structures · Lay out events in order), then *"Or start with a diagram type:"* and **14 type
chips**, and an **"Open in Mermaid"** CTA.
Clicking **"Design a system"** changed the preview from `Start / Define Goals / Plan Tasks /
Finish` to `UI / API / Service / Database` in **~3 s**. The panel works.

### W-04 — unlabelled primary navigation — **FIXED for the nav itself**
Logged-out nav exposes names: **Products · Solutions · Pricing · Community · Open Source ·
Docs**, then **Sign in** (`/app/login`), **Contact sales**, **Start free** (`/app/sign-up`).
The six left-hand items are `button`s (dropdowns), not links. Residual gaps are split out as
W-12 below so the nav fix is not hidden behind them.

### W-05 — cookie consent banner — **UNVERIFIABLE (both instruments, different reasons)**
Container (non-EU IP): no Cookiebot script in the document at all, `Cookiebot` absent from
the HTML. Pane (EU IP): Cookiebot **is** loaded —
`consent.cookiebot.com/uc.js?cbid=0ef251a9-…` plus `consentcdn.cookiebot.com/sdk/bc-v4.min.html`
— but the pane carries a stored consent decision from Run 01, so no banner appears.
Neither instrument can show a clean first-visit EU banner. Needs a clean EU profile.

### W-06 — free-tier diagram limit contradicts itself on /pricing — **STILL OPEN (3rd run)**
Verbatim, unchanged across 30 days and now confirmed logged out:
- Basic plan **card**: "3 Diagrams"
- Compare-plans **grid**, "Number of diagrams": Basic = **"Up to 6"**
- In-app truth (dashboard quota chip, this run): **"3 of 3 personal"**

### W-07 — Basic allowances vague on the card, specific in the table — **STILL OPEN (3rd run)**
Card "Limited AI" / "Limited diagram size" vs grid "15" / "Limited (60)". Unchanged.

### W-08 — dangling asterisk on Enterprise AI credits — **STILL OPEN (3rd run)**
Grid shows **"Unlimited*"**. The character `*` occurs **exactly once in the entire page text**.
There is no footnote. Unchanged.

### W-09 — CTA labels across the price ladder — **STILL OPEN; Run 02's confound resolved**
Clean logged-out read: Basic **"Get started"** · Plus **"Get started for free"** ·
Premium **"Get started"** · Enterprise **"Contact sales"**. Identical to Run 01 logged out.
Run 02's "Manage plan" reading was a logged-in artifact and should be discarded.
Still inconsistent: Plus is the only paid tier whose CTA promises "free", and nothing on the
page explains why Premium's CTA differs.

### W-11 — /pricing renders clean — **STILL GOOD**
Four plan cards, full compare grid, FAQ accordion, footer. Zero console errors.

### W-12 — the new hero and chips are invisible to a screen reader — **NEW**
On the rebuilt homepage, in an anonymous browser:
- The hero prompt box has `role="textbox"` and **no accessible name** — `aria-label` null,
  `aria-labelledby` null, no associated `<label>`. The placeholder is a `data-placeholder`
  attribute on an inner `<p>`, which is not an accessible name.
- Its two adjacent controls (attach, submit) expose **no names**.
- The 26 chips expose **no `aria-pressed` and no `aria-selected`**, so after clicking
  "Design a system" nothing in the a11y tree says which chip is active — while the visible
  preview panel changed underneath.
- The header logo link has no accessible name.
The marketing page was rebuilt this cycle and the primary interactive element of the new
design shipped unnamed.

### W-13 — compare-grid checkmarks have no accessible text — **NEW**
Positive cells in the compare grid are bare `<svg>` icons with no `aria-label` and no text;
negative cells are the literal string "-". A screen-reader user hears "-" for every feature a
plan lacks and **silence** for every feature it has. Worst case: the "Co-editing & external
sharing" row is four silent checkmarks.

### W-14 — the marketing homepage was rewritten this cycle — **NEW (context, not a defect)**
New H1: *"You already think in systems. Now you can diagram them just as fast."* New subhead:
*"Prompt inside the canvas, drop in a doc, or write it in code — and watch the diagram build
itself."* New section copy throughout ("Shape an idea in seconds", "Get the right diagram, not
just a diagram", "Keep your data yours", "Diagram where you work", "Edit together, in real
time", "Stay in your flow"). Provider deep-links (`/app/sign-up?provider=google|github`) are
present in the hero. Recorded because the W-02/W-03 fixes arrived with this rewrite, and
because the voice is noticeably more confident than the app it leads into.

---

## AREA: ONBOARDING

### O-11 — two different signup pages are live at the same URL — **NEW**
`/app/sign-up` served materially different pages to the two instruments:

| | Container (anonymous, non-EU) | Pane (authenticated, EU) |
|---|---|---|
| Heading | "Create account" | value-prop list, no heading |
| Social buttons | Google · GitHub · SSO | **Continue with** Google / GitHub / SSO |
| Email form | inline, always visible | **behind a "or sign up with email" button** |
| Email placeholder | "Work email" | **"Email"** |
| Helper text | "We recommend using your work email" | **absent** |
| Divider | "─ OR ─" | "or sign up with email" |
| Submit | "Create account" | **"Continue"** |
| Consent wording | "By **creating an account**, you agree to…" | "By **continuing**, you agree to…" |
| Value props | none | Create diagrams with text · Save and share diagrams · AI powered generation · Real-time collaboration · **Repair broken Mermaid code** |

Statsig is running in the app (`[Statsig] Creating multiple Statsig clients…`), so this is
most likely an A/B test rather than a bug. It is recorded because **every copy finding on this
page now has to be scoped to a variant**, and because one of the two variants advertises
*"Repair broken Mermaid code"* as a headline benefit — the same repair pass Run 02 recorded as
a defect on the critical path.

### O-01 — no privacy policy at the point of consent — **STILL OPEN (3rd run), in BOTH variants**
Both variants name exactly two documents: **Terms of Use** (`/terms-of-use`) and
**Terms & Conditions** (`/terms-and-conditions`). The Privacy Policy exists — it is in the
`/pricing` footer — and is still not linked where consent is given. The signup page has no
footer.

### O-02 — form fields carry no real labels — **STILL OPEN, now confirmed at DOM level**
Run 02 read this off the a11y tree. This run, directly from the DOM, in **both variants**:
all three inputs have **`id=""`, no `<label for>`, no wrapping `<label>`, no `aria-label`** —
only a `placeholder`. Still fails WCAG 3.3.2; still worst on the re-entry field.
Also: all three inputs have `required={false}`.

### O-03 — password re-entry required — **STILL OPEN (3rd run), in BOTH variants**
`confirmPassword` is present in both.

### O-04 — Turnstile — **REGRESSION RISK; NOT OBSERVED in the new variant**
In the pane (EU, real browser), after revealing the email form in the new variant:
- hidden `input[name="cf-turnstile-response"]` **present** (`cf-chl-widget-mrxb7_response`)
- `challenges.cloudflare.com` iframes: **0**
- no "Verify you are human" control rendered
- no Turnstile console error of any kind
- "Continue" disabled (expected with empty fields)

**That is the Run 01 failure signature** — hidden response field, no widget — which Run 02
recorded as FIXED when the widget rendered in the old variant. Stated honestly: the widget was
**not observed rendering** in the variant the pane was served. It may initialise lazily on
field focus, which was not tested because typing into a signup form is out of scope. Scored as
a failure; **flagged as a risk, not asserted as a regression.** The container cannot
adjudicate — its proxy blocks `challenges.cloudflare.com`.

### O-05 — guided-hint anchors on the empty editor — **UNVERIFIABLE** (new-diagram state blocked, 3/3)

### O-06 — progressive tips (Create → Edit with AI → Share) — **NOT OBSERVED, 2nd run**
Neither tip fired across a generation and an apply. Still confounded by once-per-account on
this account. Not escalated. What appears instead after a generation is now a **"Follow-ups"**
block (see AI-10).

### O-07 — the "real time" promise — **STILL OPEN** (see AI-03 / AI-08)

### O-08 — bare "Redux" theme dropdown — **STILL FIXED**

### O-09 — control-flow flags in the URL — **FIXED / verifiable again**
The pane reported full URLs this run. Before, during and after the AI generation the editor
URL was unchanged:
`/app/projects/dd7d33c3…/diagrams/22a2940c…/version/v0.1/edit` — no query string, no flags.

### O-10 — returning user opening an existing empty diagram gets no guidance — **STILL OPEN**
Unchanged from Run 02: a blank dotted canvas, a one-line bar reading **"Describe your idea"**,
and nothing else. No "What do you want to diagram?", no Generate/Paste toggle, no Upload, no
Templates entry. A free user at 3/3 has no other entry point.
New this run: the bar is joined by a **"Start voice chat"** control (see D-12).

---

## AREA: AI

### AI-01 — first generation: fast again, and valid on first emission — **FIXED**
Same prompt as Run 02 (212 chars, canvas prompt bar): *"Map how a user signs up with email,
verifies their address, and reaches their first diagram. Include the failure path where the
verification link has expired, and the branch where the email is already registered."*

| | Run 01 | Run 02 | **Run 03** |
|---|---|---|---|
| Time to rendered output | ~10 s | ~25–28 s | **≤ ~9.7 s** |
| Syntactically valid first emission | yes | **no** | **yes** |
| Visible auto-repair pass | no | **yes** | **no** |
| Rendered to the canvas | yes | no | **no** |

The response was already complete at the first poll after submit; the measured upper bound
from end-of-typing to a complete rendered preview is **9.7 s**. No "Repairing diagram…" state,
no "syntax error" string on the new generation. Run 02's regression is reversed.

### AI-02 — output faithful to the prompt — **STILL GOOD**
Flowchart covering Start → enter email and password → "Email already registered?" → sign-in
prompt / create unverified account → send verification email → user opens link → "Link valid?"
→ verify + open first diagram, or expired-link message + resend loop. Every branch asked for is
present. Summary line: *"This maps the email sign-up journey through verification to the first
diagram. It branches for an already registered email and loops through resending an expired
verification link."* Accurate.

### AI-03 — AI output does not reach the canvas — **STILL OPEN**
Unchanged in kind from Run 02: the generation appears **only** as a preview card inside the
chat panel with an **"Edit"** button. The canvas stays blank (`svg text` nodes: 0), the file
stays **"Untitled diagram"**.
One correction to Run 02's reading: the chat **is** recoverable. Typing into the canvas
"Describe your idea" bar reopens the panel with **full history intact** — Run 02's own prompts
and responses from 16 days earlier were still there. There is still **no labelled control** in
the editor chrome that says so.

### AI-08 — clicking "Edit" destroys the generated diagram — **NEW — this run's headline defect**
"Edit" is the one escape hatch from AI-03, and in Run 01 it worked (verified: canvas gained the
429 branch). This run it does not.

After clicking "Edit" on a complete, correct, ~1,400-character flowchart:
- the chat panel closes
- **the code panel contains exactly one line: `flowchart TD`** (one line number rendered, one
  `.view-line` element; the editor's input buffer separately held a 253-char fragment of the
  `classDef`/`class` styling tail)
- **the canvas renders nothing** — 0 `svg text` nodes
- the title is still **"Untitled diagram"**
- the only guidance anywhere is the string *"Hit Fix with AI (⌘⇧F) to automatically correct
  syntax errors in your diagram code."*
- state was re-polled after a further 8 s and was **stable** — this is not a render-in-progress

Net: the model produced the right diagram in under 10 seconds, and the button whose entire job
is to put it on the canvas **deleted every node and edge and kept the header line**. The user
is then told their code has a syntax error, which it does — because the product truncated it.

### AI-09 — a stranded output from a previous run is permanently unrenderable — **NEW**
The reopened chat still holds Run 02's edit (*"Add a rate-limit check before the account is
created, returning a 429 when the signup rate is exceeded."*) and its response. That response
now renders as:
> "Unable to render diagram / There's a syntax error in the Mermaid code / **Retry** **Repair**"

Sixteen days on, a credit that was charged and a diagram that was produced have decayed into an
error card. Whatever AI-03 strands is not merely parked — it rots.

### AI-04 — credit accounting is no longer visible at all — **REGRESSED**
Runs 01 and 02 both observed the credit counter and both recorded it decrementing (15 → 14 →
13). This run **no credit counter was found anywhere in the editor** — not in the header, not
on the canvas, not adjacent to the prompt bar; no element matching `credit`/`credits` exists in
the editor's text. A generation was submitted and consumed, and the interface gave **no
indication of what was charged or what remains**.
Run 02's defect was "charged before any sign of work". Run 03's is worse in kind: charged with
no sign of the charge.

### AI-05 — placeholders for the AI input — **STILL OPEN**
"Describe your idea" (canvas bar) and the chat's own placeholder. Unchanged.

### AI-06 — accessibility of the AI surfaces — **STILL OPEN**
See E-02. 12 of 21 visible interactive controls in the editor expose no accessible name.

### AI-07 — diagram is never auto-titled — **STILL OPEN**
Still "Untitled diagram" after a generation and an apply. Consistent with AI-08: titling
appears to happen on a successful apply, and the apply failed.

### AI-10 — contextual follow-ups are back — **NEW (positive)**
After the generation the response carried a **"Follow-ups"** block with four concrete next
prompts: *"Can you add a signup rate-limit check that returns 429?"* · *"Can you show what
happens when sending the verification email fails?"* · *"Can you add password validation and
its error path?"* · *"Can you split user actions and backend actions into swimlanes?"*
Run 02 lost this. It is good, and it is wasted while AI-08 stands — every follow-up compounds
work that cannot be applied.

---

## AREA: EDITOR

### E-01 — Share dialog — **STILL OPEN (3rd run), one improvement**
Opened on the Basic account. Contents, verbatim:
- Title: `Share “Untitled diagram“`
- "Invite people" → "Enter names or emails"
- "Use invite link" → "Copy link" → permission control reading **"Can edit"**
- "Who has access" → ruben Mangorrinha → "Manage"
- "Public link access" → **"No access"**

- Public-off default still correct.
- **Invite link still defaults to "Can edit"** — third run. A copied link grants edit to
  anyone who receives it.
- **Improved:** no "Loading sharing information…" state this run; the dialog rendered populated
  (Run 02: ~5 s of loading).
- Minor: the title uses a **left** double quotation mark on both sides — `“Untitled diagram“`.
- Confirms again that **sharing works on Basic**, which the paywall contradicts (D-04) and
  which `/pricing`'s own compare grid now also contradicts it on.
- The dialog's accessible name is the generated id **`A-hxeDwb-c`** — no human-readable name.

### E-02 — editor controls have no accessible names — **STILL OPEN**
**12 of 21 visible interactive controls** in the editor expose no accessible name. Named:
Open menu, Personal files, Untitled diagram, Favorite, Export, Share, Start my trial,
"Show code Flowchart", Start voice chat. Everything else — the canvas control cluster and most
header icons — is anonymous. Run 02 counted 14 of 24 on the empty editor; the shape is
unchanged.

### E-04 — the editor knows the diagram is broken and does not say so on the canvas — **NEW**
After AI-08 left a one-line stub, the only signal is the hint string *"Hit Fix with AI (⌘⇧F) to
automatically correct syntax errors in your diagram code."* The **"Fix with AI"** control
itself is present in the DOM but **not visible** (`offsetWidth/offsetHeight` = 0), and the
canvas shows a blank dotted grid with a "Flowchart" type badge and **no error state at all**.
The product has both the diagnosis and the remedy and surfaces neither where the user is
looking.

### E-05 — **Export** now appears in the editor header — **NEW (unmapped surface)**
Not exercised. Recorded because export has been on the not-covered list for three runs and the
entry point is now visible.

### E-03 — not exercised this run
Code panel round-trip, node-level AI, export formats, version history, Templates gallery,
Mermaid Flow, presentations.

---

## AREA: DASHBOARD & APP

### D-01 — "New diagram" latency — **UNVERIFIABLE, 2nd run** (3/3; clicking opens the paywall)

### D-02 — item counter always reads "0 Items" — **STILL OPEN (3rd run)**
Dashboard shows 4 files and a quota chip reading "3 of 3 personal"; the footer reads
**"0 Items"**. Identical in every state observed, across three runs.

### D-03 — in-app cap is 3 — **STILL OPEN** (as a `/pricing` contradiction; see W-06)
Quota chip: "3 of 3 personal".

### D-04 — cap-hit paywall — **STILL OPEN, verbatim identical to Run 02; one a11y improvement**
Triggered by "New diagram" at 3/3. Full text, verbatim:
> "Get more diagrams for free / There's more to Mermaid than Basic. Claim your free Plus trial
> to get: / Unlimited diagrams / No size restrictions on complex work / AI diagram generation
> (300 credits/yr) / Collaborate with comments and sharing / Trusted by over 5M users and 200k
> companies / **Trial Plusfree for 7 days. Cancel anytime** / Start Free Trial"

- **STILL OPEN — "Trial Plusfree for 7 days."** The missing space is now **three runs old** and
  has survived a full rewrite of this dialog. It is on the revenue surface.
- **STILL OPEN** — no price anywhere.
- **STILL OPEN, and now doubly contradicted** — "Collaborate with comments and sharing" is sold
  as the reason to leave Basic, while (a) Share demonstrably works on Basic (E-01, this run)
  and (b) `/pricing`'s own compare grid shows a **checkmark for "Co-editing & external sharing"
  on the Basic column**. The company's pricing page and the company's paywall disagree about
  what Basic includes.
- **STILL OPEN** — the only exit is a bare **×** with no accessible name; Run 01's labelled
  "I'll keep my limits" has not returned.
- **IMPROVED** — the primary button **"Start Free Trial"** now exposes an accessible name
  (Run 02: it did not).
- The dialog's accessible name is the generated id **`b272W8v_0w`**.
- The headline still never names the cap that was hit.

### D-05 — labels for one upsell action — **REGRESSED: four became five**
Observed this run: **"Try Plus free"** (dashboard header) · **"Start your free trial"** (cap
banner) · **"Start Free Trial"** (paywall) · **"Start my trial"** (editor header) ·
**"Upgrade to Plus"** (NEW — on every file card, four instances on the dashboard alone).
Five strings, one destination, two of them on screen simultaneously.

### D-06 — accessibility of dashboard controls — **FIXED**
The single clearest win of the run. On `/app/dashboard`, **4 of 58** interactive controls lack
an accessible name (Run 02: 7 unnamed sidebar buttons **plus every file card**).
- **Every file card now exposes a name** — "Untitled diagram You created a month ago",
  "Email Password Flow You created a month ago", "Data to Deployment Pipeline Created a year
  ago" — each with a named "Select <name>" checkbox and a named "Favorite" control.
- Sidebar and nav are fully named: Go to Dashboard, Search diagrams presentations and folders,
  Collapse Your space, Personal, Actions for Personal, Shared with you, Collapse Workflows,
  Mermaid Flow New, Collapse Favorites, Collapse Team spaces, Create Organization, Earn $30,
  Create a team, Create an organization, Try Mermaid Flow, Dismiss Mermaid Flow banner,
  Start your free trial, New diagram, More create options, All files, Grid view, List view.
- The 4 remaining unnamed are icon-only buttons on the file cards.
Run 02's "the files list is unusable without sight" no longer holds.

### D-07 — stray "Deleting Diagram …" string — **STILL FIXED**

### D-08 — console hygiene — **STILL OPEN, unchanged**
Still present on the app shell: `[Statsig] Creating multiple Statsig clients with the same SDK
key`, Permissions-Policy `Unrecognized feature: 'web-share'` and `'attribution-reporting'`,
CSP report-only `unsafe-eval` violations. No new classes of error.

### D-09 — empty diagrams consume the free cap, permanently — **STILL OPEN, and now the run's blocker**
The two never-filled "Untitled diagram" files from Run 01 are still there, still showing an
"Empty diagram" thumbnail, now labelled **"a month ago"**. They still occupy 2 of the 3 free
slots. Run 02 asked for them to be deleted to unblock this run; they were not.
Consequence beyond the pulse: a free user who clicks "New diagram" three times while deciding
what to draw is **permanently locked out of creating**, and the only routes out are deleting
their own work or paying. Nothing flags the empties; the paywall does not mention them.

### D-10 — "New presentation" exists and is undocumented — **STILL OPEN**
"More create options" still offers **"New diagram"** and **"New presentation"**. The word
"presentation" does not appear anywhere in `/pricing`'s text — not on the cards, not in the
compare grid. Whether presentations count against the 3-diagram cap is still unstated.

### D-11 — returning-user dashboard has no memory of the last session — **STILL OPEN**
Same file grid as a first visit: no "pick up where you left off", no last-opened, no recent
activity, no indication the newest thing is a month old. Favorites is still an empty explainer
box. The returning-user state surfaced is the cap and the upsell.
New since Run 02: a **"Recently shared"** section exists on the dashboard, holding the
year-old "Data to Deployment Pipeline".

### D-12 — three new surfaces appeared, none of them on /pricing — **NEW (unmapped)**
- **"Earn $30"** — a referral entry in the sidebar.
- **Mermaid Flow** — now carries a **"New"** badge plus a dismissible **BETA** banner:
  *"Visualize and run your AI workflows in Mermaid Flow / Try Mermaid Flow"*.
- **"Start voice chat"** — a voice control on the editor's canvas prompt bar.
None of the three appears in `/pricing`'s plan cards or compare grid, so what a Basic user
gets of any of them is unstated. Not defects; unmapped surfaces, and the third is the first
new input modality the pulse has seen.

---

## AREA: APP / AUTH — quantitative (Mixpanel EU, project 2954792)

### A-01 — SSO login still fails ~98% of the time — **STILL OPEN, 3rd run; Run 02's call confirmed**
| Week (start) | `User SSO Login` | `User SSO Login Failed` | Success rate |
|---|---|---|---|
| 2026-09-07 | 19 | 969 | 1.9% |
| 2026-09-14 | 25 | 962 | 2.5% |
| 2026-09-21 | 14 | 1,080 | 1.3% |
| 2026-09-28 | 18 | 864 | 2.0% |
| 2026-10-05 (partial) | 9 | 1,051 | 0.8% |

- **Four full weeks: 76 successes / 3,875 failures — 1.9%.**
- Run 02 predicted the apparent Run 01→02 improvement was the 2026-08-10 outlier week
  (3,229 failures) rolling out of the window, and that the underlying rate was flat.
  **That prediction is confirmed.** Every non-outlier week across all three runs sits at
  **864–1,120 failures**. Run 01 0.9% → Run 02 1.9% → Run 03 1.9%. Flat.
- Daily `User SSO Login` is 0–10. On 7 of the last 21 days it is **exactly 0**.
- Both readings remain live and undistinguishable from the data: enterprise SSO is broken, or
  `User SSO Login Failed` over-fires on a domain probe. At ~1,000/week against ~3 successes/day,
  over-firing is still the likelier — and if it is over-firing, a genuine enterprise SSO outage
  would be invisible underneath it.
- **Three runs. No owner. No movement. Nobody has asked what happened on 2026-08-10.**

### A-02 — email login errors still track 1:1 with successes — **STILL OPEN, 3rd run**
`User Email Login` vs `User Email Login ERROR`, daily, 2026-09-17 → 10-07 (21 full days):
- 21-day totals: **6,322 successes / 6,039 errors — 0.955 errors per success.**
- Run 02: 6,789 / 6,506 — 0.958. **The ratio is identical to three decimal places.**
- Errors exceed successes on **7 of 21 days** (Run 02: 5 of 21). Worst: 2026-10-07,
  414 successes / **447 errors**.
- Email login volume is down ~7% run over run (6,789 → 6,322).
- Same unresolved ambiguity as Runs 01 and 02: a real ~50% failure rate, or an over-firing
  error event. Still nobody's ticket.

### A-03 — signup still cannot be segmented by auth method — **STILL OPEN — three pulses old**
Re-probed directly: `User Sign Up @server` broken down by `area` over the last 7 days returns
**exactly one row — `authentication`, 49,044 events**. No provider, method or auth dimension
exists. Login is segmented four ways; signup is not segmented at all. A signup failure isolated
to one method remains invisible by construction.
This is the June 2026 report's ranked recommendation #1. It has now survived **three** pulses.

### A-04 — method mix — **unchanged in shape**
Logins 2026-09-17 → 10-07 (21 days): **Google 240,655 (84.6%) · GitHub 37,488 (13.2%) ·
Email 6,322 (2.2%) · SSO 52 (0.02%).**
Email is 2.2% of logins (Run 02: ~2.3%). Aggregate login volume still cannot function as a
smoke alarm for an auth-path regression, because the two paths that fail are 2.2% and 0.02% of
the signal.

### A-05 — signup volume healthy, but softening — **CHANGED**
Daily `User Sign Up @server`, 30 days to 2026-10-07: weekdays **8,237–9,478**, weekends
**4,366–4,799**. 30-day total **227,613**. Window high **9,478 on 2026-09-29**.
No cliff, no regression — but the most recent week is the first downward move the pulse has
recorded:
- Signups, last 7 days (Oct 1–7) **48,830** vs prior 7 (Sep 24–30) **52,725** — **−7.4%**.
- Google logins, weekday average: Oct 5–7 **12,545** vs Sep 22–24 **13,728** — **−8.6%**.
Two independent series moving the same way by a similar amount. Three weeks is not a trend and
this is **not escalated**; it is recorded so Run 04 can tell whether it continued.

### A-06 — the softening above, as a watch item — **NEW (watch, not escalate)**
See A-05. Decision rule for Run 04: if weekday signups stay below ~8,500 and Google weekday
logins stay below ~12,800, this stops being noise.

---

## FIXED CHECKLIST SCORING

Checks are frozen so the percentage is a trend. **U** = unverifiable this run; scored as a
failure.

### Website — Run 01: 2/11 → Run 02: 4/11 → Run 03: **6/11**
| # | Check | R01 | R02 | R03 |
|---|---|---|---|---|
| 1 | Static assets serve 200 to anonymous visitors | ✗ | ✓ | ✓ |
| 2 | App hydrates on marketing pages | ✗ | ✓ | ✓ |
| 3 | Hero prompt box accepts typed input | ✗ | ✗ U | **✓** |
| 4 | Intent-chip preview panel renders | ✗ | ✗ U | **✓** |
| 5 | Primary nav + hero controls have accessible names | ✗ | ✗ U | ✗ |
| 6 | Free-tier diagram limit consistent (card vs table) | ✗ | ✗ | ✗ |
| 7 | Basic AI credits + diagram size stated on the card | ✗ | ✗ | ✗ |
| 8 | No dangling footnote markers | ✗ | ✗ | ✗ |
| 9 | CTA labels consistent across the price ladder | ✗ | ✗ | ✗ |
| 10 | Compare table fully populated | ✓ | ✓ | ✓ |
| 11 | Provider deep-link params work | ✓ | ✓ | ✓ |

### Onboarding — Run 01: 2/10 → Run 02: 2/10 → Run 03: **2/10**
| # | Check | R01 | R02 | R03 |
|---|---|---|---|---|
| 1 | Cookie consent does not interrupt the signup form | ✗ | ✗ U | ✗ U |
| 2 | Form fields have real labels, not placeholders | ✗ | ✗ | ✗ |
| 3 | No redundant password re-entry | ✗ | ✗ | ✗ |
| 4 | Privacy policy linked at the point of consent | ✗ | ✗ | ✗ |
| 5 | Turnstile initialises on the signup form | ✗ | ✓ | **✗ U** |
| 6 | Signup page free of console errors | ✗ | ✓ | ✓ |
| 7 | Empty-editor first screen guides the user | ✓ | ✗ U | ✗ U |
| 8 | Guided-hint anchors resolve | ✗ | ✗ U | ✗ U |
| 9 | Progressive tips fire in sequence | ✓ | ✗ | ✗ |
| 10 | Post-generation URL free of control-flow flags | ✗ | ✗ U | **✓** |

### Editor — Run 01: 3/5 → Run 02: 2/5 → Run 03: **2/5**
| # | Check | R01 | R02 | R03 |
|---|---|---|---|---|
| 1 | Public link off by default | ✓ | ✓ | ✓ |
| 2 | Invite link defaults to view, not edit | ✗ | ✗ | ✗ |
| 3 | Canvas tooling present | ✓ | ✓ | ✓ |
| 4 | Zoom/pan controls have accessible names | ✓ | ✗ | ✗ |
| 5 | Code round-trip / version history exercised | ✗ | ✗ | ✗ |

### AI — Run 01: 5/9 → Run 02: 2/9 → Run 03: **3/9**
| # | Check | R01 | R02 | R03 |
|---|---|---|---|---|
| 1 | First generation renders to the canvas | ✓ | ✗ | ✗ |
| 2 | Generation completes <15 s with no repair pass | ✓ | ✗ | **✓** |
| 3 | Output faithful to prompt incl. failure branches | ✓ | ✓ | ✓ |
| 4 | Credit accounting visible and honest | ✓ | ✓ | **✗** |
| 5 | Contextual follow-up after first render | ✓ | ✗ | **✓** |
| 6 | AI edit applies to the canvas | ✗ | ✗ | ✗ |
| 7 | Diagram auto-titled from content | ✗ | ✗ | ✗ |
| 8 | One consistent placeholder for the AI input | ✗ | ✗ | ✗ |
| 9 | AI chat send button has an accessible name | ✗ | ✗ | ✗ |

### Dashboard & App — Run 01: 2/13 → Run 02: 1/13 → Run 03: **3/13**
| # | Check | R01 | R02 | R03 |
|---|---|---|---|---|
| 1 | "New diagram" gives immediate feedback | ✗ | ✗ U | ✗ U |
| 2 | Item counter accurate | ✗ | ✗ | ✗ |
| 3 | In-app cap matches the pricing page | ✗ | ✗ | ✗ |
| 4 | Paywall free of typos | ✗ | ✗ | ✗ |
| 5 | Paywall states a price | ✗ | ✗ | ✗ |
| 6 | Paywall names the limit that was hit | ✗ | ✗ | ✗ |
| 7 | Paywall benefit claims accurate | ✗ | ✗ | ✗ |
| 8 | Paywall offers a labelled decline | ✓ | ✗ | ✗ |
| 9 | Banner messaging changes at cap | ✓ | ✓ | ✓ |
| 10 | One consistent label for the upsell action | ✗ | ✗ | ✗ |
| 11 | Sidebar/nav controls have accessible names | ✗ | ✗ | **✓** |
| 12 | File cards expose accessible names | ✗ | ✗ | **✓** |
| 13 | App shell console clean | ✗ | ✗ | ✗ |

### TOTAL — Run 01: **14/48 (29%)** → Run 02: **11/48 (23%)** → Run 03: **16/48 (33%)**
Movement: Website **+2**, Onboarding **0**, Editor **0**, AI **+1**, App **+2**.
**5 checks were unverifiable and scored as failures** (down from 8). Ceiling if all 5 had
passed: **21/48 (44%)**. The honest statement: the website and the dashboard genuinely
improved, the AI got faster and then broke in a new and worse place, and onboarding has not
moved in six weeks.

---

## NOT COVERED THIS RUN — state plainly in every output
- **New-diagram creation, its latency, and the new-diagram empty state** (Templates & Diagram
  types, Upload, Generate/Paste toggle). Cause: the account is still at 3/3. **Second
  consecutive run.**
- **A clean first-visit EU cookie banner.** The pane has stored consent; the container is
  non-EU and serves no Cookiebot at all.
- **Whether Turnstile validates, or even initialises, in the new signup variant.** Completing a
  CAPTCHA is out of scope and typing into a signup form was not done.
- **True cold first-run after signup.**
- Export (the control is now visible but was not exercised), version history, code round-trip,
  node-level AI, comments, Mermaid Flow, presentations, voice chat.
- **Team spaces / Create Organization / Shared with you / search — not covered in any of the
  three runs.** Two of the six personas have now had no surface to react to, three times.

## HOW TO UNBLOCK RUN 04
1. **Delete the two empty "Untitled diagram" files, or give the pulse its own account.** Asked
   for before Run 03 and not done. It is the single highest-value unblock and it costs ten
   seconds: it restores the new-diagram path, D-01, the empty state, Templates and the
   progressive tips.
2. **One human, one email signup attempt in a clean EU browser:** does the Turnstile widget
   render in the new variant, and does the Cookiebot banner appear before the form? Closes
   O-04 and W-05 together.
3. **Seed the account with a diagram authored by someone else**, or the Knowledge Consumer is a
   blind spot for a fourth run.
4. Keep the headless-Chromium logged-out pass. It recovered five checks this run and it does
   not depend on anyone remembering to sign out of the pane.
