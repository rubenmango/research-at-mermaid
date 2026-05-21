---
name: run-design-test
description: Walk a Mermaid Chart designer through setting up an unmoderated design test in Great Question using one of the five pre-baked templates. Use when the user says "run a design test", "test this prototype", "start a usability test on...", "I want to test the onboarding flow", "set up a test for this prototype", or anything else about kicking off a usability test against a hosted prototype URL.
---

# Run a design test

You are guiding a Mermaid Chart designer through the standard pipeline for spinning up an unmoderated usability test in Great Question. The end state is a published GQ study with the right template, screener, and prototype URL. Total time: ~10 minutes from the user saying "I want to test X" to the study being live.

## What you need from the user

Gather these before doing anything in GQ. Do not assume defaults — ask if not provided.

1. **What flow they want to test.** Map it to one of the five pre-baked templates if possible (see template list below). If none fit, hand off to `draft-new-flow` instead.
2. **Where the prototype lives.** Public URL — Figma share link, Vercel preview, Netlify drop, GitHub Pages. Claude artifact URLs are session-private and will NOT work — the designer needs to deploy first.
3. **Any prototype context they want surfaced in the study.** Often the default research goal in the template is enough, but a one-line "we're specifically testing the new welcome screen variant" can sharpen it.

Use the AskUserQuestion tool to gather these if the user hasn't supplied them up front. Don't pepper them with one question at a time — ask the cluster in one form.

## The five pre-baked templates

| Flow | GQ study ID | Title | Incentive |
|---|---|---|---|
| Onboarding | 84736 | Onboarding — first 5 minutes | $30 |
| First diagram | 84737 | First diagram — can a new user create one in under 3 minutes? | $25 |
| AI chat | 84738 | AI chat — does it help or get in the way? | $30 |
| Export & share | 84739 | Export and share — does the flow match what users expect? | $25 |
| Plan upgrade | 84740 | Plan upgrade — what makes someone pay or walk? | $35 |

Look the template up by calling `get_unmoderated_study` with the right ID to confirm it still exists and is in `draft` state. If the workspace shape has changed, fall back to `search_unmoderated_studies` with `q: "TEMPLATE"`.

## The workflow

### Step 1 — Validate the prototype URL

Before doing anything in GQ, sanity-check the URL the designer gave you:

- It must include `https://`
- For Figma: looks like `https://www.figma.com/proto/...` (not `/file/`). If it's a `/file/` link, ask the designer to switch to the prototype share link.
- For Vercel/Netlify/GH Pages: any public URL works. If the URL contains `localhost`, `127.0.0.1`, or a private IP, refuse and ask for a public deploy.
- If you have Figma MCP tools available, you can validate the prototype is reachable via `mcp__plugin_design_figma__*` tools — but don't block on this; a 200 from a URL fetch is enough confirmation.

If the URL looks bad, tell the designer what's wrong and stop. Don't try to "fix" the URL for them.

### Step 2 — Confirm the template choice

Tell the designer which template you're going to duplicate and why. Example: "I'll duplicate template 84736 (Onboarding — first 5 minutes) since you're testing the new welcome screen. The default research goal and 5 tasks are already in there."

If the user wants to customize the title, take the input and use it. Default title format: `{Template short name} · {YYYY-MM-DD} · {brief feature name}`. Example: `Onboarding · 2026-05-21 · new welcome screen`.

### Step 3 — Duplicate via the API (where possible)

Use `create_unmoderated_study` to clone the template's shape. Pass:

- `title`: the date-stamped title from Step 2
- `research_goal`: copy from the template (use `get_unmoderated_study` to pull it) and optionally append the designer's context line
- `participation_limit`: 12 by default
- `currency`: `USD`
- `incentive`: matching the template (see table above)
- `incentive_method`: `manual`
- `screener`: if the template has a screener, mirror its questions

After creation you get a new study ID. Note it for Step 4.

**Be honest about a limitation here:** the MCP cannot copy the website-test block (where the prototype URL lives). The designer must do this part manually in the GQ UI. This is documented and known.

### Step 4 — Hand off the manual paste

Give the designer a short checklist for the GQ UI:

1. Open the new study at `https://greatquestion.co/studies/{new-id}`
2. Build tab → add a Website task block (or copy from the template if duplicating in UI)
3. Paste the prototype URL into the website-test block's URL field
4. Set duration to 10 minutes
5. Attach the standard consent form
6. Optionally turn on AI moderation for follow-up probing on weak answers
7. Review tab → confirm everything reads cleanly
8. Recruit tab → pick participant source → Publish

Frame this as a 2-minute task. Don't apologize for it — it's the smallest manual step in the pipeline.

### Step 5 — Confirm and post

Once the designer says they've published, post a confirmation to Slack `#research` so the rest of the team sees the test is in flight:

- Use the Slack MCP `slack_send_message` action (or whichever Slack tool is connected).
- Channel: `#research`
- Message format:
  ```
  :test_tube: New test live: {study title}
  {flow type} · {participation cap} testers · {duration} min · {incentive}
  Owner: {designer name}
  GQ: https://greatquestion.co/studies/{study-id}
  ```

If Slack isn't connected, skip silently and tell the designer to post manually. Don't block on it.

### Step 6 — Set expectations for results

Tell the designer:
- First responses usually within 24 hours
- Run `synthesize-results` once they have at least 5 responses
- They'll get a Slack ping in `#research` when synthesis writes the Vistaly card

## When to NOT use this skill

- **If the flow doesn't match any of the 5 templates.** Hand off to `draft-new-flow` — that skill handles the "build a fresh study" path with Claude drafting the task script.
- **If they want to test the LIVE product (not a prototype).** Use this skill anyway — point the same template at `app.mermaidchart.com` instead of a prototype URL. This is the Phase 03 post-ship test path.
- **If the user just wants to know what tests are currently running.** Hand off to `check-status`.

## Error handling

- If `get_unmoderated_study` fails for one of the template IDs, the template may have been renamed or moved. Search for `TEMPLATE` in the workspace and ask the designer which one to use as the base.
- If `create_unmoderated_study` fails, surface the actual error to the designer (don't paraphrase). The most common failure is missing GQ workspace permission.
- If the designer pastes a URL that returns 404 when you try to validate it, stop and ask them to confirm — sometimes auth-walled previews look broken to anonymous fetches but work for GQ panel testers.

## Tone

Keep this terse. Designers running the third test of the day don't want hand-holding; designers running their first test do. Mirror the user's tone. Never apologize for the manual paste step — it's the smallest part of the pipeline.
