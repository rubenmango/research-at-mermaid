---
name: draft-new-flow
description: When none of the five pre-baked Mermaid Great Question templates fit, this skill drafts a fresh unmoderated study from scratch — reading a Jira ticket for context, writing the research goal, drafting three task statements, and proposing a screener. Use when the user says "draft a new test", "create a study for this ticket", "I need a new flow", "build a new template", or anything about authoring a usability study from scratch.
---

# Draft a new-flow study

You are creating a fresh Great Question unmoderated study from scratch for a flow that doesn't match the five pre-baked templates (onboarding, first diagram, AI chat, export, plan upgrade). This is for one-off tests or for new flows that might eventually become a sixth pre-baked template.

## Inputs you need

1. **Context.** Either a Jira ticket key (e.g. `MERMAID-1234`) or a paragraph describing the feature. If the user gives you just a ticket key, pull the ticket. If they describe the feature, work with that.
2. **The prototype URL.** Same rules as `run-design-test` — must be a public hosted URL.
3. **Target audience.** Default: people who've used diagramming tools in the last 6 months. Ask if the feature targets a specific user segment (developers, PMs, designers, etc.).
4. **Whether this should be saved as a sixth template.** If yes, prefix the study title with `[TEMPLATE]` so it appears alongside the other five in the workspace.

Use AskUserQuestion if anything is missing.

## The workflow

### Step 1 — Pull the Jira ticket (if applicable)

If the user gave a ticket key, call `getJiraIssue` to pull:
- Summary
- Description
- Acceptance criteria (often in the description)
- Linked design files (Figma)
- Comments mentioning user behavior or research questions

If the ticket is sparse, ask the user to fill in the gaps. Don't invent context.

### Step 2 — Draft the research goal

Write a one-paragraph research goal in the participant's voice. Format:

> "You'll [open a link / sign in / use a feature]. We want to see whether [the thing being tested] is [discoverable / usable / understood]. Spend ~10 minutes and think out loud."

Show the draft to the user before proceeding — they may want to tweak the framing.

### Step 3 — Draft three task statements

Three tasks is the sweet spot for unmoderated tests — enough to cover the flow, not so many that participants rush. Format each task as a clear directive in the second person:

```
Task 1: {Notice / first-impression task}
  Open the link. Take 30 seconds. Tell me what you see and what you'd do first.

Task 2: {Core action task}
  Try to {accomplish the main thing this feature does}. Talk through your thinking.

Task 3: {Reflection / improvement task}
  Rate this feature 1-5 and tell me what one specific change would make it a 5.
```

Adapt the structure to fit the feature. For multi-step flows, replace Task 2 with a guided walkthrough. For navigation-heavy features, add an information-architecture task ("If you wanted to find X, where would you look?").

Show the drafted tasks to the user. Get explicit approval before creating the study.

### Step 4 — Propose a screener

Default screener (one question):

```
Have you used diagramming tools (Lucidchart, draw.io, Miro, Mermaid Chart) in the last 6 months?
  Qualify: Yes
  Disqualify: No
```

Add a second question if the feature targets a specific segment:

```
Which best describes your role?
  Single choice: Developer / PM / Designer / Technical writer / Data / Other
  Qualify: depends on feature
```

Add a third question for AI-feature tests:

```
How comfortable are you with AI features in tools you use daily? (1-5)
  Disqualify: 1-2
```

Show the proposed screener to the user.

### Step 5 — Create the study

Once the user approves goal + tasks + screener, call `create_unmoderated_study` with:

- `title`: `{Feature name} · {YYYY-MM-DD}` (or `[TEMPLATE] {feature name}` if saving as a template)
- `research_goal`: the participant-facing copy from Step 2
- `participation_limit`: 12 (or 8 for narrow segments)
- `currency`: `USD`
- `incentive`: 25-35 depending on duration and complexity (default $25 for 10-min tests)
- `incentive_method`: `manual`
- `screener`: the screener questions from Step 4 with qualify/disqualify rules

After creation, hand off to the same manual UI step from `run-design-test`:
- Open the new study in GQ
- Build tab → add the Website task block with the three drafted task statements
- Paste the prototype URL
- Set duration to 10 minutes
- Publish

### Step 6 — Save the prompt/context for reuse

If the user said this should become a sixth pre-baked template, after publishing:
1. Edit the study title in GQ to prefix with `[TEMPLATE]`
2. Reset the prototype URL in the website-test block to `{{PROTOTYPE_URL}}` placeholder so the next person knows to replace it
3. Document the new template in the plugin's README on the GitHub repo (`rubenmango/research-at-mermaid`)

## Edge cases

- **Ticket has no design / prototype linked.** Tell the user this is a "method debate" — they probably want an interview, not an unmoderated test. Suggest creating an interview study instead.
- **Feature is too broad to test in 3 tasks.** Push back — propose splitting into two tests.
- **The Jira ticket says "test the new dashboard."** That's a brief, not a research question. Help the user narrow it: "What specifically about the dashboard are we testing? Findability? Speed? Comprehension?"

## Tone

Drafting is collaborative. Show drafts, get reactions, iterate. Don't just create the study and hand back an ID — the value is in the drafting conversation. The study creation itself is the last step, not the first.
