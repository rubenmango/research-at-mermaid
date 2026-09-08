# W-01 follow-up — root cause closed

**Written:** 2026-09-08 ~16:00 UTC
**Status of the parent finding:** W-01 / W-02 / W-02b / W-02c / W-03 — **STILL OPEN, now diagnosed**
**Why this file exists:** the scheduled pulse re-fired 16 minutes after Run 01 published. That is
too soon for a real run (no delta, and `runs/2026-09-08/` is Run 01's own folder), so no Run 02 was
published. But the re-check produced new evidence that closes W-01's stated open question, so it is
recorded here rather than lost. Fold this into Run 02 on 2026-09-22.

---

## What Run 01 left open

> "the 401s are server responses, so they should hit every visitor — but I cannot prove from one
> client that they aren't specific to this browser/IP … NEEDS one human eyeball in a normal browser
> to close."

Run 01 also offered *deploy skew* as the most likely cause and recommended "return 404 not 401".

## 1. Not client-specific. Question closed without needing a human.

Re-tested from a second, fully independent client: the cloud container — different IP, plain `curl`,
no browser engine, no cookies, no Turnstile token, no JS.

```
BxH4YZfE.js -> 401      CerFT43y.js -> 200
Cl-0LJ7h.js -> 401      D7HrI6pR.js -> 200
cXJly0UV.js -> 401      mMzmF3y3.js -> 200
T-OLT5OI.js -> 401
Bnvj4OSc.js -> 401
https://mermaid.ai/  -> 200   (document serves fine)
```

Identical pass/fail split to the browser pane. **The 401s are not a bot challenge and not specific
to the test browser.** Every anonymous visitor hitting this build gets them.

## 2. The failing files are missing at origin — headers prove it

Failing chunk (`BxH4YZfE.js`) response headers:
- `x-cache: Error from cloudfront`
- **no** `content-type`, **no** `content-length`, **no** `etag`, **no** `last-modified`, **no** `server: CloudFront`

Passing chunk (`CerFT43y.js`) response headers:
- `x-cache: Miss from cloudfront`, `content-type: text/javascript`, `content-length: 44146`
- `etag: W/"44146-1788874908000"`
- `last-modified: Tue, 08 Sep 2026 13:41:48 GMT`

A served object carries `last-modified`; the 401s carry none of the object metadata. They are not
protected files — **they are absent from the origin bucket.**

This is the well-known S3 behaviour where a missing key returns `AccessDenied` (401/403) instead of
`NoSuchKey` (404) because the bucket policy denies `s3:ListBucket`. So Run 01's "return 404 not 401"
recommendation is correct but is a **bucket-policy change, not an application code change** — worth
retargeting the ticket.

## 3. It is deploy skew — but stuck, not transient

- Every *passing* chunk shares one timestamp: `last-modified: Tue, 08 Sep 2026 13:41:48 GMT`.
  That is the deploy that introduced this, ~2h before Run 01's trace.
- The failing chunk hashes are **the same hashes Run 01 recorded** (`BxH4YZfE`, `Cl-0LJ7h`,
  `cXJly0UV`, `CmD_9AiI`, `FjFeGuPG`, `DnD_mP1D`, `m6bXvShp`, `CMLSglfb`, `B1kBjBl6`, `C-lfAdkV`,
  `C15USdGo`, `uQGBGtIx`, `CSzw3Pjw`, `njhfVCGB`, `Dvky5JfX`, `BckqbxTO`, `Bfc3Dvb5`, `DpNeU2OY`,
  `Bah_7IAA`) plus two not in Run 01's sample list: `T-OLT5OI`, `Bnvj4OSc`.
- No new hashes, no new build. **No redeploy has happened, and it has not self-healed** — broken
  continuously from 13:41 UTC to at least 16:00 UTC (4h20m and counting at time of writing).

The hypothesis is confirmed *and* the "it'll clear itself" reading is ruled out. The asset upload
step of the 13:41 build did not complete and nothing has re-run it.

## 4. It is ONE bug, not five

The entry chain is broken too: `entry/app.CRAvuv7K.js` and `entry/start.Boyq2AjW.js` return **401 to
curl**. (They returned 200 to the browser pane — likely a stale edge object on that POP — but the
browser still threw `TypeError: Failed to fetch dynamically imported module:
.../entry/start.Boyq2AjW.js`, so the module chain fails either way.)

With the entry chain and ~21 chunks unavailable, **SvelteKit never hydrates**, and that single fact
produces every symptom Run 01 filed separately:

| Symptom | Run 01 ID | Re-verified 16:00 UTC |
|---|---|---|
| `[data-svelte-h]` absent — page not hydrated | W-02b | `hydrated: false` |
| Hero prompt box accepts no input | W-02 | `inputCount: 0`, `contentEditable: 0` |
| "What do you need to figure out?" preview blank | W-03 | unchanged |
| Signup Turnstile never initialises | O-0x / blocker | hidden `cf-turnstile-response` field present, **0** `challenges.cloudflare` iframes |

Signup page (`/app/sign-up`) re-check: `inputCount: 4`
(`email`, `password`, `confirmPassword`, hidden `cf-turnstile-response`), `hydrated: false`,
Turnstile iframes `0`. The form is server-rendered so it appears, but the widget that gates
submission never mounts.

**Consequence for the cadence blocker:** the Turnstile failure Run 01 attributed to bot-detection
(`Error 600010`) is at least partly downstream of this same hydration failure. Re-test the
"human past Turnstile" blocker *after* the deploy is fixed — it may not need a Turnstile exemption
at all.

## Recommended ticket (replaces Run 01 escalation items 4 and part of the blocker)

**P0 · Website/infra · one ticket, not five.**
Re-run the asset upload for the 2026-09-08 13:41 UTC build; ~21 `_web/immutable/chunks/*.js` plus
both `_web/immutable/entry/*.js` are missing from the CloudFront origin, so the marketing site has
served a non-hydrating page to every anonymous visitor for 4+ hours. Then (a) add a deploy gate that
fetches the entry chunks and fails the deploy on non-200, and (b) allow `s3:ListBucket` on the
assets bucket so missing objects return 404 instead of 401 and surface correctly in Sentry.

*One-sentence fix:* the build's JS was never uploaded — re-upload it and gate future deploys on the
entry chunks resolving.
