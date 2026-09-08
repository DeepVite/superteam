# Superteam — Personal AI Agent Skill Library

[中文](README.md) | [English](README.en.md)

This repository is a curated, **whitelist-published** subset of my personal skill library (superteam), containing AI Agent skills that are reusable across teams and devices. Each skill directory has a `SKILL.md` entry file and can be loaded by tools that follow the Agent Skills convention, such as Claude Code, Codex, and ZCode.

## Repository Structure

```
superteam/
├── README.md                          # This file (Chinese, default)
├── README.en.md                       # English readme
├── .gitignore                         # Whitelist-style ignore rules (ignore all by default, allow level by level)
└── plugins/superteam-pack/skills/
    ├── codex-tools/                   # Codex utilities
    │   ├── dish-excel-extractor/          # Excel recipe data extraction
    │   └── fix-codex-reconnecting/        # Codex network mode fix
    ├── create-session-memory/         # Session memory & handoff
    ├── dev-kit/
    │   ├── backend/excel-export/      # SaaS Excel export development
    │   └── front/beautiful-tooltip/   # Tooltip design spec
    ├── git-tools/                     # Git / GitHub workflows
    │   ├── using-git-worktrees/           # Isolated git worktree workflow
    │   └── github-cli-install/            # GitHub CLI one-click install
    └── tencent-ecosystem-kit/         # Tencent ecosystem (TAPD) integration
        ├── tapd-cli/                      # TAPD official CLI usage
        ├── tapd-cli-install/              # TAPD CLI one-click install
        ├── tapd-mcp-one-click-deployment/ # TAPD MCP one-click deployment
        └── tapd-skill/                    # TAPD MCP business operations
```

## Skill Overview

### git-tools — Git / GitHub workflows

| Skill | Purpose |
|---|---|
| using-git-worktrees | When starting feature work that needs isolation from the current workspace, or before executing implementation plans: creates isolated git worktrees with smart directory selection and safety verification |
| github-cli-install | Install/update GitHub CLI (gh) and configure authentication: detect → install → verify → login, all in one go |

### tencent-ecosystem-kit — TAPD integration

| Skill | Purpose |
|---|---|
| tapd-cli-install | Install/update the TAPD CLI and configure token credentials: official one-click install, environment variables, and verification |
| tapd-cli | Usage handbook for the TAPD official CLI: full command syntax, field value rules (e.g. custom dropdown fields must pass candidate labels), and TAPD ↔ WeCom smartsheet bidirectional sync |
| tapd-skill | Query and modify stories, bugs, tasks, and iterations in TAPD projects via TAPD MCP (change status, add comments, etc.) |
| tapd-mcp-one-click-deployment | One-click setup of the TAPD MCP Server (uvx mcp-server-tapd): environment checks, credential guide, config writing, skill installation and verification |

### codex-tools — Codex utilities

| Skill | Purpose |
|---|---|
| fix-codex-reconnecting | Switch Codex between HTTP and WebSocket network modes: fix repeated reconnection retries, force HTTP, or restore the default provider |
| dish-excel-extractor | Extract dish data from recipe Excel files exported by the "Wangda" system: cleaning, deduplication, and similarity analysis |

### General & development

| Skill | Purpose |
|---|---|
| create-session-memory | Summarize the current conversation into/refresh `session-memory.md` so a brand-new session can seamlessly continue the work (project history, pitfalls, lessons) |
| init-agent-or-claude-md-rules | On project /init or initialization, inject global user rules into `AGENTS.md` / `CLAUDE.md` (concise reply style, recycle-bin delete safety) |
| excel-export (dev-kit/backend) | Phase-by-phase development of SaaS Excel export features for backend engineers (EasyExcel/POI/JXLS), with a style template, decision tree, and testing guide |
| beautiful-tooltip (dev-kit/front) | Design/implement tooltips (hover hints) for UI elements: design spec + copywriting spec + visual examples |

## Usage

Copy the skill directory you need (or mount it via a directory link/junction) into your agent's skills directory, e.g. `~/.claude/skills/` or `~/.zcode/skills/` for Claude Code / ZCode:

```bash
# Junction mount (Windows; the source directory stays the single source of truth)
cmd /c mklink /J "%USERPROFILE%\.zcode\skills\tapd-cli" "<repo path>\plugins\superteam-pack\skills\tencent-ecosystem-kit\tapd-cli"
```

## Notes

- This repo is **whitelist-published**: `.gitignore` ignores everything by default and only allows the skill directories listed above; everything else (including the full skill inventory `skills/index.md`) is not published.
- Account names, company IDs and similar light identifiers inside skill files are personal configuration examples; the repository is private.
