# Algoticlab Skills

Skill library for Algoticlab agent workflows. This repository is organized to hold multiple skills over time, with separate distributions for each supported agent system.

## Repository Layout

```text
skills/
  codex/
    creative-ideation/
      SKILL.md
      agents/openai.yaml
      references/
    seedance-footage-vfx/
      SKILL.md
      agents/openai.yaml
      references/
  claude/
    creative-ideation/
      SKILL.md
      references/
    seedance-footage-vfx/
      SKILL.md
      references/
docs/
  codex-installation.md
  claude-installation.md
  seedance-footage-vfx/
    fa.md
    en.md
```

Add future skills under both platform folders when both runtimes are supported:

```text
skills/<platform>/<skill-name>/
```

## Included Skills

### Creative Ideation

`creative-ideation` is a routed library of creative methods for generating, expanding, selecting, subverting, refining, and synthesizing ideas. It routes the user's prompt to one suitable method, loads only the needed reference file, applies anti-slop rules, and produces concrete, non-obvious ideas with mechanisms, tradeoffs, and at least one grounded next step.

Use it for:

- Brainstorming project, product, writing, art, research, software, career, or system ideas.
- Escaping generic AI brainstorming.
- Making an idea stranger while keeping at least one buildable option.
- Picking between options with premortem/inversion style thinking.
- Refining rough concepts into sharper directions.

### Seedance Footage VFX

`seedance-footage-vfx` writes and refines Seedance 2.0 video-to-video prompts for transforming footage the user already has. It focuses on preserving the source clip's identity, face, wardrobe, performance, framing, lens feel, camera motion, and dialogue while changing only the requested VFX element, environment, timing, lighting, creature, or start frame.

Use it for:

- Adding VFX elements to a real source clip.
- Replacing the environment around a preserved subject.
- Syncing crash zooms or push-ins to dialogue/timecodes.
- Producing a transformed first frame before video generation.
- Improving existing Seedance prompts without re-rolling the whole creative direction.

## Platform Versions

- Creative Ideation Codex version: `skills/codex/creative-ideation`
- Creative Ideation Claude version: `skills/claude/creative-ideation`
- Seedance Footage VFX Codex version: `skills/codex/seedance-footage-vfx`
- Seedance Footage VFX Claude version: `skills/claude/seedance-footage-vfx`

Codex versions include `agents/openai.yaml` for Codex UI metadata. Claude versions keep only the portable `SKILL.md` and references so they can be used by Claude Code or packaged as Claude custom skills.

## Installation

- Codex: see [docs/codex-installation.md](docs/codex-installation.md)
- Claude: see [docs/claude-installation.md](docs/claude-installation.md)
- Seedance Footage VFX Persian guide: see [docs/seedance-footage-vfx/fa.md](docs/seedance-footage-vfx/fa.md)
- Seedance Footage VFX English guide: see [docs/seedance-footage-vfx/en.md](docs/seedance-footage-vfx/en.md)

## Attribution

The Creative Ideation skill is an Algoticlab Codex and Claude adaptation of the MIT-licensed `optional-skills/creative/creative-ideation` skill in `NousResearch/hermes-agent`. The original skill metadata lists `SHL0MS` as author.

The Seedance Footage VFX skill was added from a user-provided Claude-compatible `.skill` package and adapted for Codex and Claude distributions. See [NOTICE.md](NOTICE.md) for source notes.
