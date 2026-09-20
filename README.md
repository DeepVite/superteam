# Superteam

[中文](README.md) | [English](README.en.md)

这是一个进行 AI 变革的软件工程团队可以共用的技能包，涵盖需求管理、产品设计、UI/UX 设计、后端研发工具、前端研发工具、测试工具等。

目前已开源 **3 个 skill**，还有几十个 skill 待开源（当前版本 v0.1.0）。每个 skill 目录以 `SKILL.md` 为入口，可被 Claude Code、Codex、ZCode 等支持 Agent Skills 规范的工具加载。

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

### dev-kit — 后端 / 前端研发工具

| Skill | 作用 |
|---|---|
| backend/excel-export | 后端工程师分阶段开发 SaaS Excel 导出功能（EasyExcel/POI/JXLS），内置样式模板与决策树、测试指引 |
| front/beautiful-tooltip | 为界面元素设计/实现 Tooltip（悬停提示）：设计规范 + 文案规范 + 可视示例 |

### init-agent-md-rules — 工程规约初始化

| Skill | 作用 |
|---|---|
| init-agent-md-rules | 项目执行 /init 或初始化时，向 `AGENTS.md` / `CLAUDE.md` 注入用户全局规则（精炼回复风格、删除安全规则） |

## 安装与使用

### Claude Code（插件市场）

```
/plugin marketplace add DeepVite/superteam
/plugin install superteam-pack@superteam
```

### Codex

Codex CLI 会从 `~/.codex/skills/`（全局）或项目内的 `.codex/skills/`（项目级）加载技能。把需要的 skill 目录整个复制进去即可：

```bash
git clone https://github.com/DeepVite/superteam.git
mkdir -p ~/.codex/skills
cp -r superteam/plugins/superteam-pack/skills/dev-kit/front/beautiful-tooltip ~/.codex/skills/
```

> 注意：Codex 不识别软链接形式的 `SKILL.md`，请直接复制目录，不要用 symlink。

### 其他工具（ZCode 等）

把 skill 目录复制（或用目录链接/junction 挂载）到对应的技能目录即可，例如 `~/.zcode/skills/`：

```bash
# 以 junction 方式挂载（Windows，源目录保持单一真身）
cmd /c mklink /J "%USERPROFILE%\.zcode\skills\beautiful-tooltip" "<本仓库路径>\plugins\superteam-pack\skills\dev-kit\front\beautiful-tooltip"
```

## 说明

- 本仓库采用**白名单发布**：`.gitignore` 默认忽略一切，仅逐级放行上述 skill 目录；仓库其余内容（含完整技能清单）不发布。
- 许可证：[Apache License 2.0](LICENSE)。
