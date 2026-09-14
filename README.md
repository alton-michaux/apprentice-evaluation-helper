# Apprentice Evaluation System

A lightweight system for evaluating backend apprentices: capture observations as they happen, spot patterns over time, and walk into evals with the evidence already organized against the rubric.

## The one thing to get right

There are three kinds of things here, and they live in **different places** on purpose:

**The skill** goes at personal scope in Claude Code: `~/.claude/skills/apprentice-eval-capture/`. Installed there, it's available in every terminal session regardless of which repo is open. It's the reusable machinery, and it's portable (you could hand it to another CTD lead with none of your data in it).

**This repo** (`apprentice-evaluation-helper`) is the scaffolding: the rubric, the capture format, and the templates. Nothing in here names an apprentice. It's safe to share.

**Your data** lives in a **separate private repo** checked out at `apprentices/`, right inside this one. That's performance evidence on named people, so it gets its own remote and its own access list. This repo's `.gitignore` has `/apprentices` so the two never cross. Never commit it into rsites-api or any project repo where the apprentices themselves could read it.

**Why not in the project repo (like /peer-review):** `/peer-review` is about shared code and belongs in the repo. This is a private mentor-lead tool operating on sensitive data about the very people who work in that repo. Different scope, different home.

**To run it:** `cd` into this repo first so it's your working directory, then invoke the skill. The skill's relative paths (`rubric.md`, `apprentices/`) resolve against wherever you are, so being at the root is what makes them line up.

**Heads up on OneDrive:** if you keep this checked out inside a synced OneDrive path, the evidence in `apprentices/` goes to the cloud. Keep it in a local-only path unless that's what you want.

## What's here

The skill and the scaffolding ship together so you can place them. After setup they live apart:

```
~/.claude/skills/apprentice-eval-capture/     <- the skill (personal scope, all sessions)
├── SKILL.md
├── assets/
│   └── eval-form-template.md                  CTD's official eval-notes form
└── references/
    └── github-pr-gathering.md                 prompt for pulling PR data via the GitHub MCP

apprentice-evaluation-helper/                  <- this repo (scaffolding, no names in it)
├── README.md                                  this file
├── rubric.md                                  the six-competency Zone 1-5 rubric
├── capture-format.md                          how to log one observation + fast-tag cheat sheet
├── _template.md                               starting shape for a new apprentice file
├── _escalations.md                            starting shape for apprentices/escalations.md
├── _team-norms.md                             starting shape for apprentices/team-norms.md
├── .gitignore                                 keeps /apprentices out of this repo
└── apprentices/                               <- separate private repo, gitignored here
    ├── <name>.md                              evidence log + zone read (source of record)
    ├── <name>-checkins.md                     running log of monthly 1:1s + open follow-ups
    ├── <name>-eval-form.md                    generated CTD form, pre-filled for a live eval
    ├── escalations.md                         org-level items someone else owns the fix for
    └── team-norms.md                          cross-apprentice habits you own the fix for
```

## The three files per apprentice

- `<name>.md` is the **source of record**: the running observation log plus the zone-estimate table and review summaries. Never gets overwritten.
- `<name>-checkins.md` is the running log of monthly 1:1s and their open follow-ups. Separate from the evidence file on purpose: check-ins are informal and conversational, the evidence file is the formal record. Created lazily on the first check-in.
- `<name>-eval-form.md` is a **generated working copy** of the CTD form for a specific eval. Pre-filled from the source of record, then you fill the live fields during the conversation. Regenerating it is safe, it never touches `<name>.md`.

## The two files that aren't about one person

Some things surface through check-ins that aren't any one apprentice's problem. Those don't belong in an apprentice file, because one item hitting three people reads as systemic in one place and as three unrelated coaching notes in three.

- `escalations.md` holds anything whose fix has an owner **outside this desk**: curriculum gaps, eval-form wording, program-side process.
- `team-norms.md` holds cross-apprentice habits **you** own the fix for, by stating a norm or changing a process you control.

The line is who owns the fix, not how many people it touches. A single-person habit is still just coaching, and stays a normal follow-up next to that person.

Both live in `apprentices/` because they cite named people as instances. The `_escalations.md` and `_team-norms.md` files at the root of this repo are name-free placeholders showing the shape. The skill creates the real ones from those on first use.

## How it runs

**Day to day (capture):** tell the skill what you saw ("Luis pushed to the wrong branch again", "Mariya caught a great edge case in #188"). It tags the competency and direction and appends a formatted entry to the right apprentice file. Fast by design, sub-20-seconds is the whole point.

**Pulling GitHub data:** use the prompt in the skill's `references/github-pr-gathering.md`. If your current session has the GitHub MCP, run it directly; otherwise run it in a session that does and paste the output back. The skill turns it into tagged observations.

**Review time (summarize):** ask the skill to review an apprentice. It reads their log + the rubric, clusters by competency, places them on each zone ladder with the specific observations cited, and drafts a review summary into their file.

**Monthly 1:1s (check-in prep and write-up):** ask the skill what to bring up with someone. It reads the last ~30 days of their log, the open follow-ups from their last check-in, and anything in `escalations.md` or `team-norms.md` that names them, then drafts the agenda. Afterward, tell it how the conversation went and it files the write-up.

**Eval time (generate form):** ask the skill to generate an eval form for an apprentice. It pre-loads `Notes`, `Summary`, `Evaluation Breakdown`, and the completion rate from their log, and leaves the live fields (`Selection`, `In Agreement?`, `Feedback`, `Question`) blank for the conversation.

## A note on evidence

The system is only as good as what's in the logs. A few things it's built to protect against:

- **Recency and halo bias:** log concerns (`-`) as well as wins (`+`). An all-positive file usually means only wins are getting logged.
- **Vibe-based grading:** every zone placement cites specific observations. No evidence, no placement, `?` instead.
- **Source blindness:** GitHub only sees GitHub. Technical and Productivity in particular need your direct read on meetings, estimates, scoping, and how people work day to day. The forms flag where a placement is PR-only and needs backfill.
