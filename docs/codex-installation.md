# Codex Installation

This package contains the Algoticlab Codex version of `creative-ideation`.

## Install

From the repository root, copy the skill folder into your Codex skills directory.

Windows PowerShell:

```powershell
$skillsRoot = Join-Path $env:USERPROFILE ".codex\skills"
New-Item -ItemType Directory -Path $skillsRoot -Force | Out-Null
Copy-Item -Path ".\skills\codex\creative-ideation" -Destination $skillsRoot -Recurse -Force
```

macOS/Linux:

```bash
mkdir -p ~/.codex/skills
cp -R skills/codex/creative-ideation ~/.codex/skills/
```

If you use `CODEX_HOME`, install into `$CODEX_HOME/skills/creative-ideation` instead of `~/.codex/skills/creative-ideation`.

## Verify

The installed folder should look like this:

```text
~/.codex/skills/creative-ideation/
  SKILL.md
  agents/openai.yaml
  references/
```

Restart Codex or start a new Codex session after installing if the skill does not appear immediately.

## Use

Invoke the skill explicitly:

```text
Use $creative-ideation to generate non-obvious ideas for a weekend software artifact about personal archives.
```

Codex can also load it implicitly when the user asks for open-ended ideation, brainstorming, stuck-project help, idea selection, subversion, refinement, synthesis, or invention.

## Update

After pulling a new version of this repository, repeat the copy command above. The destination folder will be overwritten with the latest Codex skill files.
