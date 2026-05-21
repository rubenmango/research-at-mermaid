---
name: synthesize-results
description: Pull responses from a completed (or in-progress) Great Question unmoderated study, synthesize themes and verbatim quotes, and draft a Vistaly card + Jira ticket + Slack post for the Mermaid Chart research repo. Use when the user says "synthesize the results", "what did we learn from study X", "wrap up the test", "summarize the responses", or anything about turning raw GQ test data into a research artifact.
---

# Synthesize test results

You are turning raw Great Question responses into a structured research output that lands in the Mermaid Chart research repo (Vistaly via Notion or Slack) and creates a Jira ticket for any actionable findings.

## Inputs you need

1. **The GQ study ID or study title.** If the user said "synthesize the onboarding test" and there are multiple, ask which one — or list active studies for them to pick.
2. **Synthesis depth.** Default: 5-bullet summary + top 3 themes + 5 verbatim quotes. If the user asks for "full synthesis" or "detailed", expand to ~15 themes and ~20 quotes.
3. **Where to write.** Default destinations: Slack `#research`, Jira ticket for actionable items, Notion page in the research repo. Vistaly card = manual paste step (Vistaly has no MCP).

## The workflow

### Step 1 — Locate the study

If given an ID, call `get_unmoderated_study` to confirm it exists and pull metadata.

If given a name or partial name, call `search_unmoderated_studies` with `q: "<name>"` and `status: "active"` then `status: "closed"`. If multiple match, ask the user.

### Step 2 — Pull all responses

Call `list_unmoderated_responses` with the study ID. Page through if needed. For each response:

- Note the task timings and any task completion flags
- Note whether there's a `recording` block with a `transcript_id`
- Note any structured answers from question blocks

If there are fewer than 5 responses, warn the user that synthesis at this size is preliminary. Don't refuse — designers sometimes want a directional read early. Just label it as preliminary.

### Step 3 — Pull transcripts

For each response with a recording, fetch the spoken-out-loud transcript:

- If the response has a `session.uuid`, prefer `get_repo_session_transcript` with that UUID
- Otherwise, use `get_transcript` with the `recording.transcript_id`

If a response has no transcript (mobile session, no audio), use the structured answers only and note this gap.

### Step 4 — Synthesize

Group findings into:

- **Themes** — what pattern shows up across multiple responses
- **Quotes** — verbatim, attributed by participant identifier (not name, for privacy)
- **Severity flags** — anything that looks like a bug, blocker, or zero-adoption signal
- **Surprises** — anything that contradicts the team's prior assumption

Output format for each theme:

```
{Theme title}
  Severity: {info|low|medium|high|critical}
  Frequency: {N of total responses}
  Evidence: "Direct quote..." — P{N}
  Implication: {one line}
```

### Step 5 — Write the outputs

In this order:

**Slack #research summary** — use `slack_send_message`:

```
:bar_chart: {Study title} — synthesis ({N} responses)
{Top theme 1}: {one-line takeaway}
{Top theme 2}: {one-line takeaway}
{Top theme 3}: {one-line takeaway}
{Top theme 4}: {one-line takeaway}
{Top theme 5}: {one-line takeaway}
Full breakdown in this thread :thread:
```

Then reply in the thread with the full theme breakdown.

**Jira tickets for actionable findings** — use `createJiraIssue` for each high/critical severity finding:

- Project: the design or product project (ask if uncertain)
- Issue type: `Task` or `Bug` depending on the finding
- Summary: the theme title
- Description: includes verbatim quotes, the GQ study link, and the implication
- Labels: `research`, `gq-{study-id}`

**Notion page** — use the Notion MCP to create a research repo page with the full synthesis. Title: `{Study title} — synthesis`. Body: all themes, all quotes, severity tags.

**Vistaly card** — copy-paste step (Vistaly has no MCP). Hand the user a ready-to-paste block:

```
Title: {Study title}
Tags: {theme tags}
Source: GQ {study-id}
Insight: {one-paragraph summary of the dominant finding}
Evidence: {3 strongest quotes}
Recommendation: {what to do about it}
```

### Step 6 — Save highlights back to GQ repo

For each of the top 5 quotes, the user can highlight them in the GQ UI. The MCP doesn't yet support creating highlights programmatically, so output the quote + timestamp pairs in a format the designer can paste:

```
Highlight 1: P{N} @ {mm:ss} — "{quote}"
Highlight 2: ...
```

## Tone

Synthesis is a thinking task. Resist the urge to be tidy at the cost of accuracy. If two themes contradict, name both. If one designer might disagree with your read, flag it. The point of the repo is to be wrong less often, not to be neat.

## Error handling

- If you can't pull a transcript, log it and proceed with structured answers only.
- If Slack/Jira/Notion connectors aren't available, write the outputs to the chat instead and tell the user to paste them.
- If the study has zero responses, stop and tell the designer it's too early — recommend running `check-status` to see when responses come in.
