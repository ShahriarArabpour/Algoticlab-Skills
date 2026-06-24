# Claude Installation

This package contains the Algoticlab Claude version of `creative-ideation`.

## Claude Code Install

Install as a personal skill available across all Claude Code projects:

```bash
mkdir -p ~/.claude/skills
cp -R skills/claude/creative-ideation ~/.claude/skills/
```

On Windows PowerShell:

```powershell
$skillsRoot = Join-Path $env:USERPROFILE ".claude\skills"
New-Item -ItemType Directory -Path $skillsRoot -Force | Out-Null
Copy-Item -Path ".\skills\claude\creative-ideation" -Destination $skillsRoot -Recurse -Force
```

For a project-only Claude Code skill, copy it to the project repository instead:

```text
<project>/.claude/skills/creative-ideation/
```

## Claude Code Use

Invoke directly:

```text
/creative-ideation generate 10 grounded-but-strange product ideas for a local design studio
```

Claude Code can also load it automatically when the prompt matches the skill description.

## Claude.ai Custom Skill Package

Create a ZIP where the skill folder is the root item inside the archive.

PowerShell:

```powershell
New-Item -ItemType Directory -Path ".\dist" -Force | Out-Null
Compress-Archive -Path ".\skills\claude\creative-ideation" -DestinationPath ".\dist\creative-ideation-claude.zip" -Force
```

macOS/Linux:

```bash
mkdir -p dist
(cd skills/claude && zip -r ../../dist/creative-ideation-claude.zip creative-ideation)
```

Upload `dist/creative-ideation-claude.zip` in Claude's Skills settings, then enable the skill.

## Verify

The Claude skill folder should contain:

```text
creative-ideation/
  SKILL.md
  references/
```

The folder name is the Claude Code command name, so the direct command is `/creative-ideation`.
