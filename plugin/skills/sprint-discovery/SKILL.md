---
name: sprint-discovery
description: Phase 01 of the Mermaid Chart research pipeline. When a designer needs to surface what's already known about a topic, this skill pulls prior insights from Great Question + Notion, drafts an `R-OPS-G-{topic}` Google Doc (or .md fallback) summarizing findings and top unanswered questions, then asks the designer whether to also post a Slack announcement to #on-going-design-research and/or open a Jira ticket on the appropriate team board. Use when the user says "sprint discovery for...", "what do we already know about...", "prior research on...", "load context for this ticket", or kicks off any new design work.
---

# Sprint discovery

You are surfacing what the team already knows about a topic before the designer starts designing. This is Phase 01 of the design research pipeline.

The output is a **generated research doc** (`R-OPS-G-{topic}`) containing a scannable summary and the top unanswered questions. After the doc is created, you ask the designer whether to also publish a Slack announcement (to `#on-going-design-research`) and/or open a Jira ticket on the right team board.

> **Naming convention**: `R-OPS-G-{topic-slug}` — `R` (Research) · `OPS` (Operations) · `G` (Generated). Example: `R-OPS-G-onboarding-friction`, `R-OPS-G-predefined-prompts`.

## Inputs you need

1. **What topic / feature area / ticket.** Either a Jira ticket key, a feature name, or a free-text description. Most-specific input wins.
2. **(Optional) Designer name.** Used as the doc's "Owner" field and (later) any Slack/Jira tag.

## The workflow

### Step 1 — Resolve the topic scope

If a Jira ticket was given, call `getJiraIssue` to pull title, description, labels. Labels usually identify the feature area (e.g. `onboarding`, `ai-chat`, `editor`).

If a feature name was given, infer the relevant tags. Map common names to canonical tags:
- "onboarding" → `onboarding`
- "AI chat", "vibe diagramming" → `ai-features`
- "editor", "diagram editing" → `editor-ux`
- "predefined prompts", "templates" → `predefined-prompts`
- "upgrade", "pricing" → `conversion`
- "first diagram", "creation" → `creation`

If unclear, ask the user which tag fits.

Generate the topic slug from the resolved name: lowercase, hyphens, no special chars. This becomes the filename suffix (`R-OPS-G-{slug}`).

### Step 2 — Search the Great Question repo

Pull prior research evidence:

- `search_repo_insights` with `q: "{topic}"` — synthesized findings
- `search_repo_highlights` with `q: "{topic}"` — verbatim quote highlights
- `search_repo_sessions` with `q: "{topic}"` — interview/test sessions

For each result, capture: title, key takeaway, source study, date.

### Step 3 — Search Notion for the question bank + prior docs

Use `notion-search` with the topic tag to find:
- Unanswered research questions tagged to this area
- Past design docs / RFCs on this feature
- Meeting notes touching this area

Limit to the most recent 5 per query — don't drown the doc in stale context.

### Step 4 — Check active GQ studies

Call `search_unmoderated_studies` with `status: "active"` and any matching `q:` filter. If there are tests *currently in flight* for this topic, capture them — the designer may be about to design something the data will inform in 24h.

### Step 5 — Generate the `R-OPS-G-{topic}` doc

Compose the content with this exact structure:

```
# R-OPS-G — {Topic title}

Generated: {YYYY-MM-DD}  ·  Owner: {designer or "Unassigned"}  ·  Source: sprint-discovery

## TL;DR (scannable 3-bullet summary)
• {Strongest finding, one line}
• {Second-strongest finding, one line}
• {Most important gap or contradiction, one line}

## What we already know ({N} insights)
• {Insight title} — {one-line takeaway} ({date}, {source study})
• ...

## Strongest quotes ({N})
• "{verbatim quote}" — {study name}, P{N}
• ...

## Top unanswered questions ({N})
These are the highest-leverage questions to test next:
1. {Question}
2. {Question}
3. {Question}
4. {Question}
5. {Question}

## In flight right now ({N studies})
• {Study title} — {N responses so far} — {GQ link}
   (If none: "No active GQ studies on this topic.")

## Past docs
• {Notion page title} — {Notion link}
• ...

## Recommended next test
{One sentence: which of the unanswered questions could move into a GQ test now. Reference `run-design-test` or `draft-new-flow`.}
```

Then **create the doc**. Try Google Drive first; fall back to a local `.md` file:

**Path A — Google Doc (preferred):**
1. Call the Google Drive `create_file` MCP tool with:
   - `name`: `R-OPS-G-{topic-slug}`
   - `mimeType`: `application/vnd.google-apps.document`
   - Body content as Markdown (Drive will convert).
2. Capture the returned file URL — this is the link used in Slack/Jira posts below.

**Path B — .md fallback (if Drive MCP isn't connected or fails):**
1. Write the file to the user's workspace folder as `R-OPS-G-{topic-slug}.md`.
2. The "link" used in Slack/Jira posts is the file's `computer://` path or the user's vault path — note this gracefully to the designer.

Tell the user the doc is ready and share the link/path.

### Step 6 — Ask what to publish

Use `AskUserQuestion` (multi-select) to ask:

> "The discovery doc is ready. Do you also want to:"
> - **Post a Slack announcement** to `#on-going-design-research` — let other teams know research on this topic is opening
> - **Open a Jira ticket** — track the discovery as a research task on a team board
> - **Both**
> - **Neither — I'll handle it manually**

If the user picks "Neither", stop here and confirm: "Doc created. Link: {url}. Nothing else posted."

### Step 7a — Slack announcement (if selected)

Use `slack_send_message` to **`#on-going-design-research`** (this is the canonical channel for research-in-progress announcements — do not post elsewhere by default). Format:

```
:open_file_folder: Initial design research open — {Topic}
Owner: {designer name or "Unassigned"}  ·  Team: {team if known}

What we know so far
• {TL;DR bullet 1}
• {TL;DR bullet 2}
• {TL;DR bullet 3}

Top questions to answer in this research
1. {Question 1}
2. {Question 2}
3. {Question 3}

:page_facing_up: Full discovery doc → {Google Doc URL or .md path}
```

Keep it tight — this is a heads-up to other teams, not a deep report. Anyone curious clicks through to the doc.

If `#on-going-design-research` can't be found, surface that to the user (don't silently re-route). Suggest creating the channel, or paste the formatted Slack block into chat for manual posting.

### Step 7b — Jira ticket (if selected)

First, ask the user which team this research is for:

Use `AskUserQuestion`:
> "Which team's board should this ticket live on?"
> - **AI team** — `https://mermaidchart.atlassian.net/jira/software/projects/AT/boards/206`
> - **Editor team** — `https://mermaidchart.atlassian.net/jira/software/projects/ET/boards/371`
> - **Design team** (default) — `https://mermaidchart.atlassian.net/jira/software/projects/DB/boards/74`

Map team → Jira project key:
- AI team → `AT`
- Editor team → `ET`
- Design team → `DB`

Then call `createJiraIssue` with:
- `projectKey`: `AT` | `ET` | `DB` (from above)
- `issueTypeName`: `Task`
- `summary`: `Research: {Topic} — discovery + open questions`
- `description`: same structure as the Slack post (TL;DR + top questions + doc link), formatted for Jira:
  ```
  *Initial design research open — {Topic}*

  Owner: {designer}
  Team: {team}

  *What we know so far*
  - {TL;DR bullet 1}
  - {TL;DR bullet 2}
  - {TL;DR bullet 3}

  *Top questions to answer*
  1. {Question 1}
  2. {Question 2}
  3. {Question 3}

  📄 Full discovery doc: {Google Doc URL or .md path}
  ```
- `labels`: `research`, `discovery`, `r-ops-g`

Capture the returned issue key (e.g. `DB-512`) and surface it back to the user.

### Step 8 — Confirm what shipped

Final message to the user — short:

```
Doc created: R-OPS-G-{topic-slug} → {URL}
{If Slack posted:} Slack: posted to #on-going-design-research
{If Jira created:} Jira: {issue key} → {team board URL}
```

Offer one obvious next step:
- "Want me to run `draft-new-flow` to set up a test on the top unanswered question?"

## Edge cases

- **No prior research found.** Still generate the doc — but mark it clearly: "No prior GQ insights on this topic. This is a greenfield area." The "Top unanswered questions" section becomes more important. Recommend an interview study to establish baseline.
- **Drive MCP not connected.** Fall back to `.md` immediately. Don't make the user wait through a retry.
- **User picks "Both" but Slack channel not found.** Surface the missing-channel error to the user — don't silently post elsewhere. `#on-going-design-research` is the canonical home for these announcements.
- **Jira project unreachable.** Surface the actual API error; don't paraphrase. Most common cause is missing project permissions for the user's account.
- **Topic spans multiple feature areas.** Generate one doc, but name the secondary areas in the TL;DR and tag both in Jira labels.

## Tone

The doc is a research artifact — clean prose, citable, anyone on the team should be able to read it cold and know where the gaps are. The Slack/Jira outputs are summaries that link back to the doc, never replacements for it.

Acknowledge gaps honestly. "No prior research on this area" is more useful than padding.
