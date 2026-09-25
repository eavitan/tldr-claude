# tldr

**Version 1.1.0**

Short-reply mode for Claude Code. `/tldr` rewrites the last reply short and keeps every reply short until `/tldr off`. Replies start with `⚡`.

## Usage

```
/tldr                      max level
/tldr light|medium|max     level
/tldr --code diff          changed lines only
/tldr --scope chat         first run = TL;DR of whole chat
/tldr off                  normal replies
```

## Rules
- Answer first. No filler. Never repeat the subject.
- Only what was asked. Next-step details wait for the next question.
- Code blocks exact. Prose around them one line.
- Questions via picker. Warnings kept, one line.
- "explain" (or תסביר) gets one normal reply, mode stays on.

## Files
- SKILL.md: the skill (loaded)
- settings.md: defaults, overridable per call
- Spec and dev docs: `~/Projects/tldr-skill/`

## Changelog

### 1.1.0 — 2026-09-25
- Explain detector: "explain" / תסביר gets one normal reply, mode resumes. Reverses 1.0.0 "no escape hatch" after the first live test.
- PRD 0.2: rule 7.11, non-goals and lifecycle updated.

### 1.0.0 — 2026-09-25
- First release. Levels light / medium / max, six settings in settings.md.
- `⚡` prefix as the only indicator. Picker for questions to the user.
- No keyword escape hatch, by decision.
