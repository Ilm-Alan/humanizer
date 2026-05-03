# humanizer

A Claude Code skill that rewrites prose to remove signs of LLM authorship and inject specific, varied, opinionated human voice.

Originally adapted from [blader/humanizer](https://github.com/blader/humanizer). This fork tightens the skill around two ideas:

1. **Hard gates over soft suggestions.** The most common AI tells (em dashes, curly quotes, curly apostrophes inside contractions, bolded inline-list headers, decorative emoji) are zero tolerance and enforced by a literal pre-output scan, not a vibe check. A soft "use em dashes less" suggestion buried halfway down a checklist is exactly why em dashes leak through.
2. **One-phase output, two-phase reasoning.** The model still drafts, audits its own draft, and revises. But the deliverable is just the final clean text. No "here is a draft, here is what I noticed, here is the final version" three-section response, which gets skipped in agentic flows anyway and clutters output when it doesn't.

Patterns covered are based on [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), maintained by WikiProject AI Cleanup.

## What's in the skill

The skill enumerates 31 named patterns across content, language, style, communication, and filler. A few that the upstream skill underweights or omits:

- **Em-dash substitutes.** When told "no em dashes," models commonly switch to en dashes, double hyphens, or spaced hyphens that play the same syntactic role. The skill calls these out as also forbidden, with a literal scan step.
- **Curly apostrophes inside contractions** (`don't`, `it's`, `we're`). The most common quote leak and the hardest to catch by eye.
- **Sentence-initial transition pile-up.** Opening sentence after sentence with `Additionally,` / `Moreover,` / `Furthermore,` / `Notably,` / `That said,`.
- **Intra-document register shift.** When one paragraph reads like a person and the next reads like a press release, because the rewrite touched some sections and not others.
- **"Across" as a soft broadener** (`across industries`, `across teams`, `across the board`).
- **Augmented AI vocabulary** including `meticulous`, `bolstered`, `resonate`, `ultimately`, `indeed`, `that said`, `in essence`, `it's worth noting`.

## Install

Drop `SKILL.md` into your Claude Code skills directory:

```
~/.claude/skills/humanizer/SKILL.md
```

Or for OpenAI Codex CLI:

```
~/.codex/skills/humanizer/SKILL.md
```

The skill is invoked when you ask Claude Code to humanize text, or by running `/humanizer` if you have it set up as a slash command.

## Use

Pass text you want humanized. Optionally provide a writing sample for voice matching.

Inline:

```
Humanize this text. Here's a sample of my writing for voice matching:

[paste sample]

[paste text to humanize]
```

From a file:

```
Humanize this text. Use my writing style from ~/Documents/my-writing.md as a reference.

[paste text to humanize]
```

The skill returns only the final humanized text. If you want the audit (what was AI about it, what changed), ask after the fact.

## When to skip the skill

- Code, commits, configs.
- Prose Claude is currently drafting from scratch. The patterns are easier to apply inline as you write than to clean up afterward.

## Credit

Adapted from [blader/humanizer](https://github.com/blader/humanizer). Pattern catalog draws from [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing).

## License

MIT. See [LICENSE](LICENSE).
