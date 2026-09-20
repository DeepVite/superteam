# Superteam — 个人 AI Agent 技能库

[中文](README.md) | [English](README.en.md)

本仓库是个人技能库（superteam）按**白名单模式**精选发布到 GitHub 的子集，收录可在团队/多设备间复用的 AI Agent 技能（skill）。每个 skill 目录以 `SKILL.md` 为入口，可被 Claude Code、Codex、ZCode 等支持 Agent Skills 规范的工具加载。

> 当前版本 **v0.1.0**，精选发布 3 个 skill，更多 skill 视成熟度逐步放出。

## 仓库结构

```
superteam/
├── README.md                          # 本文件（中文）
├── README.en.md                       # 英文版说明
├── LICENSE                            # Apache License 2.0
├── .gitignore                         # 白名单式忽略规则（默认忽略一切，逐级放行）
├── .claude-plugin/marketplace.json    # 插件市场定义（Claude Code）
├── .agents/plugins/marketplace.json   # 插件市场定义（多 Agent）
└── plugins/superteam-pack/
    ├── .claude-plugin/plugin.json     # 插件 manifest（Claude Code）
    ├── .codex-plugin/plugin.json      # 插件 manifest（Codex）
    └── skills/
        ├── dev-kit/
        │   ├── backend/excel-export/    # SaaS Excel 导出开发
        │   └── front/beautiful-tooltip/ # Tooltip 悬停提示设计规范
        └── init-agent-md-rules/         # 项目初始化规则注入（AGENTS.md / CLAUDE.md）
```

## Skill 简介

### dev-kit — 开发

| Skill | 作用 |
|---|---|
| backend/excel-export | 后端工程师分阶段开发 SaaS Excel 导出功能（EasyExcel/POI/JXLS），内置样式模板与决策树、测试指引 |
| front/beautiful-tooltip | 为界面元素设计/实现 Tooltip（悬停提示）：设计规范 + 文案规范 + 可视示例 |

### init-agent-md-rules — 项目初始化

| Skill | 作用 |
|---|---|
| init-agent-md-rules | 项目执行 /init 或初始化时，向 `AGENTS.md` / `CLAUDE.md` 注入用户全局规则（精炼回复风格、删除安全规则） |

## 使用方式

### 方式一：作为插件市场安装

在 Claude Code 中执行：

```
/plugin marketplace add DeepVite/superteam
/plugin install superteam-pack@superteam
```

仓库同时提供 `.codex-plugin/plugin.json`，支持 Codex 插件格式。

### 方式二：直接挂载 skill 目录

把需要的 skill 目录复制（或用目录链接/junction 挂载）到你的 Agent 技能目录即可，例如 Claude Code / ZCode 的 `~/.claude/skills/` 或 `~/.zcode/skills/`：

```bash
# 以 junction 方式挂载（Windows，源目录保持单一真身）
cmd /c mklink /J "%USERPROFILE%\.zcode\skills\beautiful-tooltip" "<本仓库路径>\plugins\superteam-pack\skills\dev-kit\front\beautiful-tooltip"
```

## 说明

- 本仓库采用**白名单发布**：`.gitignore` 默认忽略一切，仅逐级放行上述 skill 目录；仓库其余内容（含完整技能清单）不发布。
- 许可证：[Apache License 2.0](LICENSE)。
