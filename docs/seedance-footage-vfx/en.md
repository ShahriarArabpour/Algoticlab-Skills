# Seedance Footage VFX Guide

This skill writes, rewrites, and improves Seedance 2.0 prompts for video-to-video footage transformation. Use it when the user already has a real source clip and wants to preserve that clip while applying a controlled VFX transformation.

Common use cases:

- Add fire, creatures, invisibility, body transformations, or hard-surface effects to real footage.
- Replace the environment around a preserved subject while keeping face, wardrobe, performance, camera motion, and framing intact.
- Write crash-zoom or push-in prompts synchronized to source dialogue or a timecode.
- Generate a transformed first frame before spending video credits.
- Improve prior Seedance prompts for lighting, scale, timing, creature realism, SFX, runtime, or source preservation.

## Format Detection

The provided `higgsfiled-seedance-footage-vfx_.skill` file was a Claude-compatible skill package: a ZIP-style archive containing `seedance-footage-vfx/SKILL.md`. The `SKILL.md` uses the Agent Skills frontmatter format and is suitable for Claude. A separate Codex version was created with Codex UI metadata in `agents/openai.yaml`.

## Repository Paths

```text
skills/codex/seedance-footage-vfx/
  SKILL.md
  agents/openai.yaml
  references/

skills/claude/seedance-footage-vfx/
  SKILL.md
  references/
```

Two referenced support files were added so the skill's internal links are complete:

- `references/dialogue-timing.md`
- `references/first-frame.md`

## Install For Codex

From the repository root:

```powershell
$skillsRoot = Join-Path $env:USERPROFILE ".codex\skills"
New-Item -ItemType Directory -Path $skillsRoot -Force | Out-Null
Copy-Item -Path ".\skills\codex\seedance-footage-vfx" -Destination $skillsRoot -Recurse -Force
```

If you use `CODEX_HOME`, install the skill at:

```text
$CODEX_HOME/skills/seedance-footage-vfx
```

Example Codex invocation:

```text
Use $seedance-footage-vfx to write a Seedance 2.0 video-to-video prompt for this source clip. Preserve the person and camera move, but make the right arm turn invisible with realistic refraction.
```

## Install For Claude Code

macOS/Linux:

```bash
mkdir -p ~/.claude/skills
cp -R skills/claude/seedance-footage-vfx ~/.claude/skills/
```

Windows PowerShell:

```powershell
$skillsRoot = Join-Path $env:USERPROFILE ".claude\skills"
New-Item -ItemType Directory -Path $skillsRoot -Force | Out-Null
Copy-Item -Path ".\skills\claude\seedance-footage-vfx" -Destination $skillsRoot -Recurse -Force
```

Example Claude Code invocation:

```text
/seedance-footage-vfx write a prompt for this video: preserve the speaker and source dialogue, add a giant photoreal creature on the tower, then push in on the spoken line.
```

## Package For Claude.ai

```powershell
New-Item -ItemType Directory -Path ".\dist" -Force | Out-Null
Compress-Archive -Path ".\skills\claude\seedance-footage-vfx" -DestinationPath ".\dist\seedance-footage-vfx-claude.zip" -Force
```

Upload `dist/seedance-footage-vfx-claude.zip` in Claude.ai Skills settings and enable it.

## Usage Notes

- If no source footage is attached or described, the skill should ask what footage the user is starting from.
- Use `SFX and source dialogue only` when original dialogue must survive.
- Use `SFX only` when only added sound effects are needed.
- When prepending an intro, subtract the intro duration from the total runtime and flag any source-performance truncation.
- This skill is not for building a brand-new scene from scratch; it is for transforming existing footage.
