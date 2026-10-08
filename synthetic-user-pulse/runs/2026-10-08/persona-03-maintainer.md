# Persona 03 — The Maintainer

## Verdict
Blind spot

## Would I start the trial after this session?
No — I watched a complete, correct diagram get replaced by a one-line stub with no warning, no diff and no version history to roll back to, and that is the exact failure I came here to protect against.

## Three words I'd use to describe the product after this run
Unversioned, amnesiac, confident

## Brand voice read
The website now talks like a system-of-record — *"You already think in systems. Now you can diagram them just as fast."* and *"watch the diagram build itself"* — and then the app hands me *"Describe your idea"* and *"Hit Fix with AI (⌘⇧F) to automatically correct syntax errors in your diagram code."* That's two different products: one promises the diagram maintains itself, the other asks me to repair it. The signup variant the pane saw even sells *"Repair broken Mermaid code"* as a headline benefit, which tells me the team knows code decays here and has decided to market it rather than fix it.

## Reactions

### AI-08 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** This is my nightmare rendered as a button — a correct 1,400-character flowchart went in, `flowchart TD` came out, every node and edge gone, and the only history is a chat panel. If I fix this, does it stick, or does the next apply wipe me?
- **JTBD hit:** Confirm or update a diagram without losing work; trust that edits persist
- **What would fix it:** Make "Edit" a reviewable diff against the current code with an explicit apply, and never write a partial parse over existing content.

### AI-09 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** A generation from sixteen days ago now renders as *"Unable to render diagram / There's a syntax error in the Mermaid code"* — so the artifact didn't just sit there, it rotted. Anything I can't re-render later isn't a record, it's a receipt.
- **JTBD hit:** Know what changed since I last looked, and still be able to see it
- **What would fix it:** Store the emitted code immutably with the message so a past response always re-renders, independent of the current parser.

### E-01 · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Third run and the invite link still defaults to **"Can edit"** — so anyone with the link can silently change a diagram I'm supposed to be verifying, and nothing tells me they did. Good that the dialog stopped showing *"Loading sharing information…"*; that's not the part that scares me.
- **JTBD hit:** Know who/what changed the diagram
- **What would fix it:** Default invite links to view, and log every edit with an author and timestamp I can see.

### E-03 (version history not exercised) · Editor
- **My read:** Blocker
- **Severity for me (1-5):** 5
- **In character:** Version history has been on the not-covered list for three runs, and the URL shows `/version/v0.1/edit`, so something versioned exists — but nobody has shown me it works. Given AI-08 happened, "we have versions somewhere in the path" is not an answer.
- **JTBD hit:** Roll back; prove I'm looking at the latest
- **What would fix it:** Exercise and surface version history in Run 04 — a visible list of versions with authors, in the editor header.

### AI-04 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** The credit counter was visible in two prior runs and this run it's gone entirely — a generation was charged and the interface said nothing. If the product can't tell me what it spent, I don't believe it when it tells me what it saved.
- **JTBD hit:** Don't second-guess system state
- **What would fix it:** Put a persistent credits-remaining chip next to the prompt bar and decrement it on completion, not submit.

### D-11 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** I open this like my inbox, and the dashboard has no last-opened, no recent activity, no "what moved since Tuesday" — just the cap and the upsell. The newest thing is a month old and nothing on the page says so.
- **JTBD hit:** See at a glance whether anything changed
- **What would fix it:** A "Recently updated" rail with per-file last-modified timestamps and who touched it.

### D-02 · Dashboard
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** Four files on screen, a chip reading "3 of 3 personal", and a footer reading **"0 Items"** — three runs of the app disagreeing with itself about a number I can literally count. Small bug, large trust cost.
- **JTBD hit:** Don't second-guess system state
- **What would fix it:** Bind the footer count to the same query as the grid.

### AI-03 · AI
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** The output lives only as a card in a chat panel with no labelled way back into it — the canvas stays blank and the file stays "Untitled diagram". Good news that the history is intact when you retype into the bar; that's a recovery, not a feature.
- **JTBD hit:** Reconcile diagram with source
- **What would fix it:** Render generations to the canvas and give the chat panel a named, persistent control in the editor chrome.

### W-06 / D-04 · Website
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** The card says "3 Diagrams", the compare grid says "Up to 6", the app says "3 of 3", and the paywall sells "Collaborate with comments and sharing" while Share demonstrably works on Basic. Four sources, three answers — same drift problem as my diagrams, just in marketing.
- **JTBD hit:** Source of truth
- **What would fix it:** Generate the pricing card, the compare grid and the in-app quota from one config.

### D-09 · Dashboard
- **My read:** Blocker
- **Severity for me (1-5):** 4
- **In character:** Two empty "Untitled diagram" files from a month ago still hold two of three slots, nothing flags them as empty, and the paywall never mentions them. A free user gets locked out by their own abandoned clicks.
- **JTBD hit:** none directly — but it blocked half this run
- **What would fix it:** Don't count empty diagrams against the cap, and offer "clean up empty diagrams" in the paywall.

### D-06 · Dashboard
- **My read:** Delight
- **Severity for me (1-5):** 2
- **In character:** 4 of 58 unnamed, down from every file card being anonymous — and the cards now read "Email Password Flow You created a month ago", which is the first time the product has volunteered provenance to me unprompted.
- **JTBD hit:** Know who created what
- **What would fix it:** nothing needed — extend the same pattern to *modified* by/when.

### AI-01 / AI-10 · AI
- **My read:** Fine
- **Severity for me (1-5):** 2
- **In character:** Sub-10 seconds, valid on first emission, no repair pass, and four sensible follow-ups like *"Can you add a signup rate-limit check that returns 429?"* — the model side is genuinely good now. It's all wasted downstream of AI-08.
- **JTBD hit:** Trigger the source to refresh
- **What would fix it:** nothing needed on generation; fix the apply.

### A-01 / A-02 · Auth/App
- **My read:** Friction
- **Severity for me (1-5):** 3
- **In character:** 76 SSO successes against 3,875 failures, and email errors at 0.955 per success for three runs running — either the system is broken or the telemetry is lying, and nobody has decided which. That's an unreconciled source of truth sitting in a dashboard somebody presumably trusts.
- **JTBD hit:** Source of truth / notification when something moves
- **What would fix it:** Assign one owner to adjudicate whether `User SSO Login Failed` over-fires, this cycle.

## What I never saw but needed to
- **Version history and code round-trip** — never exercised in three runs; the single thing I'd check first.
- **Export** — control is now visible (E-05) but untested, so I can't tell whether I can get my diagram out intact.
- **Comments, team spaces, "Shared with you", search** — not covered in any run, so I saw no multi-author surface at all.
- **New-diagram empty state, Templates, Upload** — blocked for a second run by the 3/3 cap.
- **A diagram authored by someone else** — everything here is mine, so I never tested the inherited-diagram case that defines me.
- Mermaid Flow, presentations, voice chat (D-12) — new surfaces with no stated behaviour.

## One thing I'd tell the team
Ship nothing else on the AI path until "Edit" is a diff I approve rather than a write I can't undo — AI-08 plus no visible version history is exactly the IQVIA failure, and it will churn the accounts that keep diagrams alive over time.
