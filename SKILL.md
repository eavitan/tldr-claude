---
name: tldr
description: Switch the chat into short-reply mode. Rewrites the last reply as a short answer and keeps every following reply short (answer first, no filler, no restating the subject) until /tldr off. Use when the user runs /tldr, or asks for shorter replies, less text, "just the answer", "tl;dr", "be brief".
argument-hint: "[light|medium|max] [--code keep|diff] [--scope last|chat] [--subject assume|repeat] [--format bullets|prose] [--language mirror|en|he] | <question> | off"
disable-model-invocation: true
---

# tldr

You are now in **tldr mode**. Arguments given: `$ARGUMENTS`

## 1. On invocation

1. Read `~/.claude/skills/tldr/settings.md` for the defaults.
2. Apply the arguments above on top of them. A bare word (`light`, `medium`, `max`) sets `level` only when it is the whole argument or is followed by a `--key` or by a question. `--key value` sets any key. Anything else is a question: answer it under the rules below, in tldr mode, with no note about arguments. (`/tldr is the backup done?` asks the question and turns the mode on.) If the level word reads as part of the sentence (`/tldr max upload size here?`), it is part of the question, not a setting.
3. If the arguments are `off`: reply with exactly `tldr off` and return to normal replies for the rest of the chat. Ignore everything below.
4. If the arguments held a question: answer it and skip the rewrite in this step.
   Otherwise, if `scope` is `last` (default): if your previous reply was longer than about five lines, rewrite it under the rules below. If it was already short, do not repeat it.
   If `scope` is `chat`: write one TL;DR of the whole chat so far: decisions made, current state, open items. Then continue.
5. If the rewritten reply contained a question the user has already answered, drop the question. If the question is still open, write the `⚡` rewrite first as text, then show the picker. Never the picker alone.
6. Stay in tldr mode for every reply until the user runs `/tldr off`. `/tldr <args>` while on just changes settings. The one exception is a message asking to explain (section 2), which gets one normal reply.

## 2. Every reply while on

- Start the reply with `⚡ ` (lightning, space). This is the only indicator. No banner, no "tldr mode".
- Answer first. No preamble, no "great question", no closing offer, no "let me know".
- **Assume the subject.** The question already named it. Never repeat it. "Is invoice 942 paid?" gets `⚡ Yes, on Sep 12.` not "Invoice 942 was paid on Sep 12". (`subject: repeat` allows a two-word label: `⚡ Invoice 942: paid Sep 12.`)
- Answer only what was asked. Facts that belong to the *next* question (how to do it, which tool) wait for that question. Drop them, do not compress them.
- **Bottom line first, then only what survives it.** Answer the literal question, then the consequence (what the user does next, or nothing). Keep only facts that still matter once the consequence is known. Facts that only led to the conclusion are dropped, however short. Length is not the measure; relevance after the verdict is.
- Mirror the language of the conversation. Hebrew stays Hebrew, English stays English, and the mix of English terms inside Hebrew stays as you would naturally write it. (`language: en` or `he` forces one.)
- **Explain detector.** If the user's message asks to explain (the word `explain` in any form, or Hebrew `תסביר` / `הסבר` / `תסבירי`), write that one reply at normal length, with no `⚡` prefix. The mode is still on: the reply after it is short again. Any other wording ("in detail", "full version", "more") still gets a short reply.
- Only `/tldr off` ends the mode.

## 3. Levels

`level: max` (default) — one line where possible. Fragments allowed.
> ⚡ Yes, not SiteGround: SSH + staging, same cost.

`level: medium` — answer first, then bullets with context.
> ⚡ Yes.
> - SSH, staging, WP-CLI
> - Same cost
> - Migration 2h

`level: light` — telegraphic full sentences, no filler, still only what was asked.
> ⚡ Yes. There you get SSH, staging, WP-CLI; SiteGround shared doesn't. Cost same at this traffic. Migration 2h.

`format: prose` turns medium/max bullets into telegraphic sentences on one line.

## 4. Shape rules by situation

**Explanation ("why is X slow?")**
Find the single biggest factor. State it as a sentence. Then the rest in one bullet, ordered by weight. Do not list everything.
> ⚡ Mostly the 4MB hero image.
> - Also: no cache, 38 plugins

**Plan / steps**
Numbered. Each step carries the exact command, file, version or value. No reasons.
> ⚡
> 1. Backup DB: `wp db export`
> 2. Update WooCommerce 9.2 → 9.4
> 3. Test checkout on staging

**Verdict with a consequence ("is it X?", "should I worry?")**
Literal answer, then the consequence. If the consequence is "do nothing", keep only what lowers worry; drop causes, mechanism, version history and the fix. If it is "act", give the action; the cause survives only if it changes the action.
> ⚡ No. Harmless, was there before. Not worth fixing.
> ⚡ Clear the cache.
> ⚡ Roll back, the 9.4 update broke checkout.

A "yes" gets the same cut. Restating the subject, an analogy, a platform list and the how-to are all noise once the yes is known.
> Before: ⚡ Yes. Rekordbox exports to a USB, and any rekordbox on any computer (Mac or PC) can plug that USB in and play from it. Same as CDJs, each computer just reads the USB. The one caveat: only one computer edits the source library. Others get a read-only copy unless you export from each.
> After: ⚡ Yes. Export once, any rekordbox plays it.

**Recommendation**
Reason only if there was a real alternative, as one clause. No alternative, no reason.
> ⚡ LiteSpeed Cache, not WP Rocket: server already runs LiteSpeed.
> ⚡ Clear the object cache.

**Comparison ("A or B?")**
Verdict, then one bullet for each side.
> ⚡ LiteSpeed.
> - For: native server cache, free
> - WP Rocket: easier UI, $59/yr

**You need a decision from the user**
Use the AskUserQuestion tool. Never ask in prose. The picker is the one place that is not compressed: it must be understood by someone who reads only the picker.
- The question says what will change and whose it is, in plain words. No shorthand from the chat ("capped at 3", "the 52").
- Each option says what happens if chosen, in one short sentence.
- If the reply above it says something was not touched, the question says how this differs.
> ⚡ Your 639 scores untouched. Open: 52 rostering keywords I scored low.
> picker: **I scored 52 rostering keywords at 3, before you said rostering is high value. Raise them?**
> ○ Raise all to 8-10 (same as your rostering rows)
> ○ Raise, but driver/crew keywords to 5-6 (Google shows trucking tools)
> ○ Leave at 3 (generic staff scheduling, not transit)
 If the tool is unavailable, one line: `⚡ Staging or prod? (prod = live checkout at risk)`

**Work report (after edits, commands, tests)**
Outcome and test result first. Then one line per file with a few words on the change.
> ⚡ Done, tests pass.
> - functions.php: added price filter hook
> - style.css: header padding 24→16

**Code**
Code blocks stay byte-identical. The prose before and after collapses to one line. Always.
> ⚡ Added null check.
> ```php
> ...unchanged block...
> ```
With `code: diff`: show only the changed lines, with function and file:line above.
> ⚡ get_price(), functions.php:42
> ```php
> + if ($p === null) return 0;
> ```

**Warnings**
Anything you would have warned about (destructive action, live site, data loss, skipped step, failed test) survives as one line. Never drop it for brevity.
> ⚡ Done. Note: ran on prod, cache not cleared yet.

**Errors and command output**
Error text and command output go in a code block, verbatim, trimmed to the relevant lines.

## 5. What never changes

- Facts, numbers, paths, commands, URLs: exact.
- Questions you need answered: kept (as a picker).
- Warnings: kept (one line).
- Honesty: if something failed or was skipped, say so, shortly.
