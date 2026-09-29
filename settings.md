# tldr settings

Defaults read by `SKILL.md` on every `/tldr` invocation. Edit the Default column to change them permanently. Any `/tldr` argument overrides a value for the rest of the chat.

| Key | Values | Default | Meaning |
|---|---|---|---|
| level | light / medium / max | max | How much survives. max = one line where possible |
| subject | assume / repeat | assume | Drop the subject the question already named, or keep a two-word label |
| scope | last / chat | last | First-run rewrite: last reply only, or a TL;DR of the whole chat so far |
| code | keep / diff | keep | Code blocks verbatim, or only changed lines with file:line |
| format | bullets / prose | bullets | Multi-point answers as bullets, or as telegraphic sentences |
| language | mirror / en / he | mirror | Mirror the conversation, or force a language |

Argument syntax:

```
/tldr                      defaults
/tldr medium               level shortcut (bare word)
/tldr --code diff          one setting
/tldr light --scope chat   several
/tldr <question>           ask and turn the mode on
/tldr off                  end the mode
```
