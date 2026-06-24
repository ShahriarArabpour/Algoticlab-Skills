# Algoticlab Skills

Private skill library for Algoticlab agent workflows. This repository is organized to hold multiple skills over time, with separate distributions for each supported agent system.

## Repository Layout

```text
skills/
  codex/
    creative-ideation/
      SKILL.md
      agents/openai.yaml
      references/
  claude/
    creative-ideation/
      SKILL.md
      references/
docs/
  codex-installation.md
  claude-installation.md
```

Add future skills under both platform folders when both runtimes are supported:

```text
skills/<platform>/<skill-name>/
```

## Included Skill

### Creative Ideation

`creative-ideation` is a routed library of creative methods for generating, expanding, selecting, subverting, refining, and synthesizing ideas. It routes the user's prompt to one suitable method, loads only the needed reference file, applies anti-slop rules, and produces concrete, non-obvious ideas with mechanisms, tradeoffs, and at least one grounded next step.

Use it for:

- Brainstorming project, product, writing, art, research, software, career, or system ideas.
- Escaping generic AI brainstorming.
- Making an idea stranger while keeping at least one buildable option.
- Picking between options with premortem/inversion style thinking.
- Refining rough concepts into sharper directions.

## Platform Versions

- Codex version: `skills/codex/creative-ideation`
- Claude version: `skills/claude/creative-ideation`

The Codex version includes `agents/openai.yaml` for Codex UI metadata. The Claude version keeps only the portable `SKILL.md` and references so it can be used by Claude Code or packaged as a Claude custom skill.

## Installation

- Codex: see [docs/codex-installation.md](docs/codex-installation.md)
- Claude: see [docs/claude-installation.md](docs/claude-installation.md)

## Attribution

The Creative Ideation skill is adapted for Algoticlab from the MIT-licensed `optional-skills/creative/creative-ideation` skill in `NousResearch/hermes-agent`. See [NOTICE.md](NOTICE.md) for source attribution.
