---
name: check-status
description: Show the current state of all in-flight Mermaid Chart Great Question studies — what's active, response counts, who owns each, and what's new since last check. Use when the user says "check test status", "any new responses", "what's in flight", "any tests running", "what's the state of the research queue", or wants a quick read on the pipeline without doing any work.
---

# Check test status

You are giving the designer (or whoever's asking) a fast read on what tests are running, what's complete, and what needs attention. Output is short — a table or a few bullets. No synthesis, no recommendations, just the state.

## Inputs you may receive

- **None** — default behaviour: show all active studies and recent closed ones.
- **A study ID or name** — drill into one study's response count and timing.
- **A filter like "mine"** — show only studies owned by the current user.
- **A time window** — "since yesterday", "this week", etc.

## The workflow

### Step 1 — Pull active studies

Call `search_unmoderated_studies` with `status: "active"`. For each study, capture:
- Title
- Study ID + URL
- Owner
- Created date
- Maximum slots / participation limit

For each one, call `list_unmoderated_responses` (just to get the count — use `items: 1` to be efficient) and record `meta.count`.

### Step 2 — Pull recently closed studies

Call `search_unmoderated_studies` with `status: "closed"` and `updated_at: "{7 days ago in ISO 8601}"`. Same pattern as Step 1.

### Step 3 — Pull recently completed responses

If the user asked about responses specifically, or if there's an active study with new responses, call `search_unmoderated_responses` with `submitted_at: "{last check time, or 24h ago by default}"`. This catches "any new responses" inquiries fast.

### Step 4 — Render the status

Compact table format:

```
Active studies ({N})

ID     Title                                         Owner   Responses    Progress
84736  Onboarding — first 5 minutes                  Ruben   8 / 12       ████░░ live
84742  Predefined prompts — why zero adoption        Ruben   3 / 12       █░░░░░ early
...

Recently closed (last 7d, {N})

ID     Title                                         Owner   Final count   Closed
84733  Predefined prompts — why zero adoption        Ruben   12 / 12       2026-05-19
...

New since {last check} ({N} new responses)
• 2 new on study 84736 (Onboarding)
• 1 new on study 84742 (Predefined prompts)
```

If there are no active studies, say so directly: "No active GQ studies right now."

If the only "new" thing is zero, also say so: "No new responses in the last 24h."

### Step 5 — Suggest the obvious next action (if applicable)

After the table, offer ONE next action — pick whichever is most relevant:

- If any study has hit `participation_limit`: "Study {X} is full — run `synthesize-results` on it?"
- If there are stale active studies (>14 days old, low responses): "Study {X} has been live 14 days with only {N} responses — consider closing or boosting recruitment."
- If everything looks healthy: "All running smoothly."

Don't suggest more than one action. The point of `check-status` is to be lightweight.

## Edge cases

- **Workspace has 50+ active studies.** Paginate. Show the most recent 10 by default, offer to show more.
- **User asks about a specific study that doesn't exist.** Suggest `search_unmoderated_studies` with the partial name they gave.
- **Permission errors on some studies.** Show what you can, note that some are inaccessible.

## Tone

Information density over prose. Tables and bullets. No "Great question!" preambles. No "Hope this helps." Just the state.
