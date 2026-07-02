# Capture Format

How to log a single observation. The whole point: capture has to be cheap. If logging takes more than ~20 seconds you won't do it, and you're back to reconstructing evals from memory. So an observation carries the minimum: what happened, which competency it touches, and whether it's a positive or a concern.

**Zone assignment does not happen at capture time.** You tag a competency and a direction, that's it. Placing someone on the Zone 1-5 ladder happens at review, once the evidence has accumulated. Trying to zone every note live is what kills the habit.

## The entry shape

Each observation is one block appended to an apprentice's file, under `## Observation Log`:

```
### YYYY-MM-DD | <competency> | <+ | - | ~>
<one or two sentences: what happened, concrete and specific>
[optional: source, e.g. PR #123, standup, pairing session]
```

- `+` positive signal
- `-` concern
- `~` neutral / context (useful but not clearly good or bad)

**Concrete beats vague.** "Handled the empty-array case in PR #204 without being asked" is worth ten of "good attention to detail." At review time the specific note reconstructs the moment. The vague one is just a vibe you'll second-guess.

## Competency tags (fast reference)

Use these exact labels so entries are greppable. One-liner each so you can tag without reading the full rubric:

| Tag | Covers |
|-----|--------|
| `Technical` | Language/framework depth, building real features, architecture, scoping |
| `Detail` | Code cleanliness, commits, PR hygiene, edge cases, conventions, review quality |
| `Problem-Solving` | Debugging, breaking down tasks, researching docs, tackling the unfamiliar |
| `Initiative` | Learning from feedback, proactive research, not repeating mistakes, leading |
| `Productivity` | Deadlines, estimates, autonomy, punctuality, agile/process, context-switching |
| `Communication` | Async + meetings, giving/taking feedback, docs, stakeholder + cross-team work |

An observation can touch more than one. Pick the primary. If it genuinely splits, log two entries. Don't agonize: a slightly-miscategorized note is still infinitely better than an unlogged one.

## Examples

```
### 2026-06-14 | Detail | +
Caught a null case in Mariya's PR #188 that would've broken on an empty roster. Left a clear comment explaining the failure mode.
Source: PR #188 review
```

```
### 2026-06-18 | Problem-Solving | -
Stuck on the same auth redirect bug for two days before flagging it. Debugging is there, but the "ask for help in a reasonable timeframe" habit isn't yet.
Source: standup
```

```
### 2026-06-20 | Communication | ~
Quiet in the last two retros. Not a problem on its own, worth watching whether it's a pattern.
```

## Direction is not a verdict

`-` doesn't mean "bad apprentice." It means "evidence pointing at a growth area." A good file has a healthy mix of `+` and `-`, because that's what real development looks like. An all-positive file usually means you're only logging the wins, which quietly reintroduces the recency and halo bias this system exists to kill.
