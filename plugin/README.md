# Mermaid Research Pipeline

The design research pipeline for Mermaid Chart, packaged for Cowork. Every designer on the team gets the same workflow without needing to memorize the tool chain.

## What this plugin does

Wraps the existing pipeline (Great Question for testing, Notion question bank, Vistaly + Jira for output, Slack #research for broadcast) behind five conversational skills. Designers install it once and stop having to think about plumbing.

## Skills

| Skill | What it does | Typical trigger phrases |
|---|---|---|
| `run-design-test` | Walks through the 10-minute test setup — pick the matching pre-baked Great Question template, paste the prototype URL, publish. | "run a design test", "test this prototype", "start a usability test on..." |
| `synthesize-results` | Pulls the latest Great Question responses for a study, summarizes themes, drafts a Vistaly card + Jira ticket + Slack post. | "synthesize the results", "what did we learn from study X", "wrap up the test" |
| `draft-new-flow` | When none of the five pre-baked templates fit, reads a Jira ticket and drafts a research goal + three task statements + screener for a fresh study. | "draft a new test", "create a study for this ticket", "I need a new flow" |
| `sprint-discovery` | Phase 01 of the pipeline. Pulls prior research, drafts an `R-OPS-G-{topic}` Google Doc (or .md fallback) with TL;DR + top unanswered questions, then asks whether to post a heads-up to `#on-going-design-research` and/or open a Jira ticket on the team's board. **v0.2.0 — doc-first, broadcast on consent.** | "sprint discovery for...", "what do we already know about...", "prior research on..." |
| `check-status` | Lists active studies, response counts, and anything completed since last check. | "check test status", "any new responses", "what's in flight" |

## Required connectors

Before the plugin will function end-to-end, connect these via Cowork (Settings → Connectors). The plugin bundles only the Great Question MCP; the others are pulled from whatever the team already has connected.

- **Great Question** — bundled via `.mcp.json`; first install will OAuth to the workspace.
- **Slack** — for posting research summaries to `#research` (synthesis) and `#on-going-design-research` (sprint-discovery announcements).
- **Google Drive** *(optional, recommended)* — for the `R-OPS-G-{topic}` discovery doc. Without it, sprint-discovery falls back to writing a local `.md` file.
- **Atlassian / Jira** — for reading ticket context (sprint discovery, draft-new-flow) and writing research tickets back.
- **Notion** — for the shared question bank and any out-of-Vistaly research storage.
- **GitHub** — used by sprint-discovery to pull this repo's prior research artifacts when relevant.
- **Figma** — for prototype URL validation and frame-level context where needed.

If any of these are missing the plugin will degrade gracefully — the affected skill will tell the user what to connect.

## The five pre-baked Great Question templates

The plugin assumes these GQ study drafts exist in the workspace. They were created during plugin design and live as templates the designer duplicates per test:

| Template | GQ ID | Incentive |
|---|---|---|
| Onboarding — first 5 minutes | [84736](https://greatquestion.co/studies/84736) | $30 |
| First diagram — under 3 minutes | [84737](https://greatquestion.co/studies/84737) | $25 |
| AI chat — help or hurdle | [84738](https://greatquestion.co/studies/84738) | $30 |
| Export and share — flow check | [84739](https://greatquestion.co/studies/84739) | $25 |
| Plan upgrade — pay or walk | [84740](https://greatquestion.co/studies/84740) | $35 |

## How it fits together

See the full pipeline docs in the [research-at-mermaid repo](https://github.com/rubenmango/research-at-mermaid) — the v2 design pipeline doc, the interactive node graph, and the designer runbook (the same content this plugin operationalizes).

## Versioning

Semver. Bump minor for new skills or flow changes, patch for bug fixes, major when the pipeline shape itself changes.

- **v0.2.0** (2026-05-21) — `sprint-discovery` rewritten: doc-first (`R-OPS-G-{topic}`), then asks designer whether to broadcast to `#on-going-design-research` and/or open a Jira ticket on AI / Editor / Design board. Adds Google Drive as an optional connector for the doc.
- **v0.1.0** (2026-05-20) — initial five-skill scaffold, GQ MCP bundled, five pre-baked templates referenced.

## Install

```bash
# Clone the repo
git clone https://github.com/rubenmango/research-at-mermaid.git
cd research-at-mermaid/plugin

# Zip the plugin contents (everything inside this folder, not the folder itself)
zip -r ~/Downloads/mermaid-research-pipeline.plugin . -x "*.DS_Store"

# Then in Cowork: Settings → Plugins → Install from file → pick the .plugin file
```

The first time the plugin loads it will OAuth to your Great Question workspace. The other connectors (Slack, Jira, Notion, Drive, Figma) are pulled from whatever your Cowork already has connected — the plugin doesn't bundle them.

## Known limitations

- Great Question's MCP doesn't expose study-block editing today, so `run-design-test` ends with the designer pasting the prototype URL in the GQ UI manually (~2 min). When GQ ships block-edit endpoints, this becomes fully automatic.
- The Vistaly integration goes through Notion/Slack rather than a direct MCP — Vistaly doesn't expose one. Vistaly card creation is a copy-paste step from the Slack summary.
