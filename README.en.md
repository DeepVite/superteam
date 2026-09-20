# Superteam

[中文](README.md) | [English](README.en.md)

A shared skill pack for software engineering teams driving AI transformation — covering requirements management, product design, UI/UX design, backend dev tools, frontend dev tools, testing tools, and more.

**3 skills** are open-sourced so far, with dozens more on the way (current version: v0.1.0). Each skill directory has a `SKILL.md` entry file and can be loaded by tools that follow the Agent Skills convention, such as Claude Code, Codex, and ZCode.

## Repository Structure

```
superteam/
├── README.md                          # This file (Chinese, default)
├── README.en.md                       # English readme
├── LICENSE                            # Apache License 2.0
├── .gitignore                         # Whitelist-style ignore rules (ignore all by default, allow level by level)
├── .claude-plugin/marketplace.json    # Plugin marketplace definition (Claude Code)
├── .agents/plugins/marketplace.json   # Plugin marketplace definition (multi-agent)
└── plugins/superteam-pack/
    ├── .claude-plugin/plugin.json     # Plugin manifest (Claude Code)
    ├── .codex-plugin/plugin.json      # Plugin manifest (Codex)
    └── skills/
        ├── dev-kit/
        │   ├── backend/excel-export/    # SaaS Excel export development
        │   └── front/beautiful-tooltip/ # Tooltip design spec
        └── init-agent-md-rules/         # Project-init rule injection (AGENTS.md / CLAUDE.md)
```

## Skill Overview

### dev-kit — Backend / frontend dev tools

| Skill | Purpose |
|---|---|
| backend/excel-export | Phase-by-phase development of SaaS Excel export features for backend engineers (EasyExcel/POI/JXLS), with a style template, decision tree, and testing guide |
| front/beautiful-tooltip | Design/implement tooltips (hover hints) for UI elements: design spec + copywriting spec + visual examples |

### init-agent-md-rules — Engineering conventions init

| Skill | Purpose |
|---|---|
| init-agent-md-rules | On project /init or initialization, inject global user rules into `AGENTS.md` / `CLAUDE.md` (concise reply style, recycle-bin delete safety) |

## Installation & Usage

### Claude Code (plugin marketplace)

```
/plugin marketplace add DeepVite/superteam
/plugin install superteam-pack@superteam
```

### Codex

Codex CLI loads skills from `~/.codex/skills/` (global) or `.codex/skills/` (per project). Copy the whole skill directory you need into it:

```bash
git clone https://github.com/DeepVite/superteam.git
mkdir -p ~/.codex/skills
cp -r superteam/plugins/superteam-pack/skills/dev-kit/front/beautiful-tooltip ~/.codex/skills/
```

> Note: Codex does not discover skills whose `SKILL.md` is a symlink — copy the directory instead of linking it.

### Other tools (ZCode, etc.)

Copy the skill directory (or mount it via a directory link/junction) into the corresponding skills directory, e.g. `~/.zcode/skills/`:

```bash
# Junction mount (Windows; the source directory stays the single source of truth)
cmd /c mklink /J "%USERPROFILE%\.zcode\skills\beautiful-tooltip" "<repo path>\plugins\superteam-pack\skills\dev-kit\front\beautiful-tooltip"
```

## Notes

- This repo is **whitelist-published**: `.gitignore` ignores everything by default and only allows the skill directories listed above; everything else (including the full skill inventory) is not published.
- License: [Apache License 2.0](LICENSE).
