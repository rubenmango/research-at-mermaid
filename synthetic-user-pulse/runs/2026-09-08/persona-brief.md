# Persona reaction brief (shared by all 6 agents)

You are running one synthetic-user pass for Mermaid Chart's biweekly product health check.

## Setup
1. Load the Notion fetch tool: call `ToolSearch` with query `select:mcp__Notion__notion-fetch`.
2. Fetch your persona page (ID given in your task) with `mcp__Notion__notion-fetch`. Copy the
   **System Prompt** block from it and adopt that persona fully. Stay in character for the reactions.
3. Read `/home/claude/evidence-run-2026-09-08.md` completely. This is the ONLY evidence. It is a
   real trace of the real product captured today. Do not invent screens, copy, or behaviour that is
   not in the file. If the file says something was NOT covered, you did not see it — say so rather
   than imagining it.

## What you produce
Write ONE file: `/home/claude/persona-<NN>-<slug>.md` (NN and slug given in your task). Use exactly
this structure so the results can be merged mechanically:

```
# Persona <NN> — <Name>
## Verdict
one of: Served | Mostly served | Partially served | Blind spot | Out of scope
## Would I start the trial after this session?
Yes / No / Not yet — one sentence why, in character.
## Three words I'd use to describe the product after this run
word, word, word
## Brand voice read
2-3 sentences in character: does the copy across website -> signup -> editor -> paywall sound like
one product talking to someone like me? Name specific strings from the evidence.
## Reactions
One block per evidence item you have an opinion on. Skip items that are genuinely irrelevant to you,
but do NOT skip an item just because it is negative for the product. Include at least 8 blocks.
Reference the evidence IDs exactly (W-01, O-04, AI-03, D-04, A-01, etc.).

### <EVIDENCE-ID> · <area: Website|Onboarding|Editor|AI|Dashboard|Auth/App>
- **My read:** Blocker | Friction | Fine | Delight | Not my problem
- **Severity for me (1-5):** n
- **In character:** 1-2 sentences, first person, the way this persona actually talks.
- **JTBD hit:** which of my jobs this touches, or "none"
- **What would fix it:** one concrete sentence, or "nothing needed"

## What I never saw but needed to
Bullets. Things this persona would have looked for that the evidence file marks as not covered.
## One thing I'd tell the team
One sentence.
```

## Rules
- Severity is *for this persona*, not global. A Steward may rate the 401 skew a 2 and the SSO
  telemetry a 5; a Sensemaker the reverse.
- Quote actual product copy from the evidence when you react to copy.
- Keep the whole file under ~900 words. Dense beats long.
- Do not write anything outside the file; your final message should be one line: the file path.
