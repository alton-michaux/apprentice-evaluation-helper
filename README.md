# Apprentice Evaluation System

A lightweight system for evaluating backend apprentices: capture observations as they happen, spot patterns over time, and walk into evals with the evidence already organized against the rubric.

## The one thing to get right

There are two kinds of things here, and they live in **different locations** on purpose:

**The skill** goes at personal scope in Claude Code: `~/.claude/skills/apprentice-eval-capture/`. Installed there, it's available in every terminal session regardless of which repo is open. It's the reusable machinery, and it's portable (you could hand it to another CTD lead with none of your data in it).

**Your data** goes in a separate private folder, by convention `~/apprentice-eval/`: `rubric.md`, `capture-format.md`, and everything under `apprentices/`. This is performance evidence on named people, so it stays **outside any git repo**. Never commit it into rsites-api or any project repo where the apprentices themselves could read it.

**Why not in the project repo (like /peer-review):** `/peer-review` is about shared code and belongs in the repo. This is a private mentor-lead tool operating on sensitive data about the very people who work in that repo. Different scope, different home.

**To run it:** `cd ~/apprentice-eval` first so the data folder is your working directory, then invoke the skill. The skill's relative paths (`rubric.md`, `apprentices/`) resolve against wherever you are, so being in the data folder is what makes them line up.

**Heads up on OneDrive:** if you keep the data folder inside a synced OneDrive path, this evidence goes to the cloud. Keep it in a local-only path unless that's what you want.

## What's here

This bundle contains both pieces so you can place them. After setup they live apart:

```
~/.claude/skills/apprentice-eval-capture/     <- the skill (personal scope, all sessions)
├── SKILL.md
├── assets/
│   └── eval-form-template.md                  CTD's official eval-notes form
└── references/
    └── github-pr-gathering.md                 prompt for pulling PR data via the GitHub MCP

~/apprentice-eval/                             <- your data (private, no git repo)
├── README.md                                  this file
├── rubric.md                                  the six-competency Zone 1-5 rubric
├── capture-format.md                          how to log one observation + fast-tag cheat sheet
└── apprentices/
    ├── _template.md                           starting shape for a new apprentice file
    ├── <name>.md                              evidence log + zone read (source of record)
    └── <name>-eval-form.md                    generated CTD form, pre-filled for a live eval
```

## The three files per apprentice

- `<name>.md` is the **source of record**: the running observation log plus the zone-estimate table and review summaries. Never gets overwritten.
- `<name>-eval-form.md` is a **generated working copy** of the CTD form for a specific eval. Pre-filled from the source of record, then you fill the live fields during the conversation. Regenerating it is safe, it never touches `<name>.md`.

## How it runs

**Day to day (capture):** tell the skill what you saw ("Luis pushed to the wrong branch again", "Mariya caught a great edge case in #188"). It tags the competency and direction and appends a formatted entry to the right apprentice file. Fast by design, sub-20-seconds is the whole point.

**Pulling GitHub data:** use the prompt in the skill's `references/github-pr-gathering.md`. If your current session has the GitHub MCP, run it directly; otherwise run it in a session that does and paste the output back. The skill turns it into tagged observations.

**Review time (summarize):** ask the skill to review an apprentice. It reads their log + the rubric, clusters by competency, places them on each zone ladder with the specific observations cited, and drafts a review summary into their file.

**Eval time (generate form):** ask the skill to generate an eval form for an apprentice. It pre-loads `Notes`, `Summary`, `Evaluation Breakdown`, and the completion rate from their log, and leaves the live fields (`Selection`, `In Agreement?`, `Feedback`, `Question`) blank for the conversation.

## A note on evidence

The system is only as good as what's in the logs. A few things it's built to protect against:

- **Recency and halo bias:** log concerns (`-`) as well as wins (`+`). An all-positive file usually means only wins are getting logged.
- **Vibe-based grading:** every zone placement cites specific observations. No evidence, no placement, `?` instead.
- **Source blindness:** GitHub only sees GitHub. Technical and Productivity in particular need your direct read on meetings, estimates, scoping, and how people work day to day. The forms flag where a placement is PR-only and needs backfill.
