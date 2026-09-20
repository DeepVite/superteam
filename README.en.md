# Superteam — Personal AI Agent Skill Library

[中文](README.md) | [English](README.en.md)

This repository is a curated, **whitelist-published** subset of my personal skill library (superteam), containing AI Agent skills that are reusable across teams and devices. Each skill directory has a `SKILL.md` entry file and can be loaded by tools that follow the Agent Skills convention, such as Claude Code, Codex, and ZCode.

> Current version: **v0.1.0**, with 3 curated skills published; more skills will be released gradually as they mature.

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

### dev-kit — Development

| Skill | Purpose |
|---|---|
| backend/excel-export | Phase-by-phase development of SaaS Excel export features for backend engineers (EasyExcel/POI/JXLS), with a style template, decision tree, and testing guide |
| front/beautiful-tooltip | Design/implement tooltips (hover hints) for UI elements: design spec + copywriting spec + visual examples |

### init-agent-md-rules — Project initialization

| Skill | Purpose |
|---|---|
| init-agent-md-rules | On project /init or initialization, inject global user rules into `AGENTS.md` / `CLAUDE.md` (concise reply style, recycle-bin delete safety) |

## Usage

### Option 1: Install as a plugin marketplace

In Claude Code, run:

```
/plugin marketplace add DeepVite/superteam
/plugin install superteam-pack@superteam
```

The repository also ships `.codex-plugin/plugin.json` for Codex plugin support.

### Option 2: Mount the skill directory directly

Copy the skill directory you need (or mount it via a directory link/junction) into your agent's skills directory, e.g. `~/.claude/skills/` or `~/.zcode/skills/` for Claude Code / ZCode:

```bash
# Junction mount (Windows; the source directory stays the single source of truth)
cmd /c mklink /J "%USERPROFILE%\.zcode\skills\beautiful-tooltip" "<repo path>\plugins\superteam-pack\skills\dev-kit\front\beautiful-tooltip"
```

## Notes

- This repo is **whitelist-published**: `.gitignore` ignores everything by default and only allows the skill directories listed above; everything else (including the full skill inventory) is not published.
- License: [Apache License 2.0](LICENSE).
