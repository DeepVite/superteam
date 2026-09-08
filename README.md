# Superteam — 个人 AI Agent 技能库

[中文](README.md) | [English](README.en.md)

本仓库是个人技能库（superteam）按**白名单模式**精选发布到 GitHub 的子集，收录可在团队/多设备间复用的 AI Agent 技能（skill）。每个 skill 目录以 `SKILL.md` 为入口，可被 Claude Code、Codex、ZCode 等支持 Agent Skills 规范的工具加载。

## 仓库结构

```
superteam/
├── README.md                          # 本文件（中文）
├── README.en.md                       # 英文版说明
├── .gitignore                         # 白名单式忽略规则（默认忽略一切，逐级放行）
└── plugins/superteam-pack/skills/
    ├── codex-tools/                   # Codex 工具类
    │   ├── dish-excel-extractor/          # Excel 食谱数据提取
    │   └── fix-codex-reconnecting/        # Codex 网络模式修复
    ├── create-session-memory/         # 会话记忆生成与交接
    ├── dev-kit/
    │   ├── backend/excel-export/      # SaaS Excel 导出开发
    │   └── front/beautiful-tooltip/   # Tooltip 悬停提示设计规范
    ├── git-tools/                     # Git / GitHub 工作流
    │   ├── using-git-worktrees/           # git worktree 隔离工作流
    │   └── github-cli-install/            # GitHub CLI 一键安装
    └── tencent-ecosystem-kit/         # 腾讯生态（TAPD）集成
        ├── tapd-cli/                      # TAPD 官方 CLI 使用
        ├── tapd-cli-install/              # TAPD CLI 一键安装
        ├── tapd-mcp-one-click-deployment/ # TAPD MCP 一键部署
        └── tapd-skill/                    # TAPD MCP 业务操作
```

## Skill 简介

### git-tools — Git / GitHub 工作流

| Skill | 作用 |
|---|---|
| using-git-worktrees | 开始需要与当前工作区隔离的特性开发，或执行实施计划之前：创建隔离的 git worktree，含智能目录选择与安全校验 |
| github-cli-install | 安装、更新 GitHub CLI（gh）并配置登录认证：检测→安装→验证→登录一键全流程 |

### tencent-ecosystem-kit — TAPD 集成

| Skill | 作用 |
|---|---|
| tapd-cli-install | 安装、更新 TAPD CLI 并配置 Token 凭证：官方一键安装、环境变量配置、安装验证全流程 |
| tapd-cli | TAPD 官方 CLI 的「使用手册」：全量命令语法、字段取值规则（如自定义下拉字段必须传候选文案）、TAPD ↔ 企微智能表格双向同步 |
| tapd-skill | 通过 TAPD MCP 查询、修改项目中的需求、缺陷、任务、迭代等信息（改状态、加评论等） |
| tapd-mcp-one-click-deployment | 一键接入腾讯 TAPD MCP Server（uvx mcp-server-tapd）：环境检查、凭据获取、配置写入、skill 安装与验证 |

### codex-tools — Codex 工具

| Skill | 作用 |
|---|---|
| fix-codex-reconnecting | 在 HTTP 与 WebSocket 网络模式间切换 Codex：修复反复重连、强制 HTTP、恢复默认 provider |
| dish-excel-extractor | 从网达系统导出的带量食谱 Excel 中提取菜品数据：清洗、去重、相似性分析 |

### 通用与开发

| Skill | 作用 |
|---|---|
| create-session-memory | 总结当前对话并生成/更新 `session-memory.md`，让新会话能无缝接续当前工作（项目历史、踩坑与经验） |
| init-agent-or-claude-md-rules | 项目执行 /init 或初始化时，向 `AGENTS.md` / `CLAUDE.md` 注入用户全局规则（精炼回复风格、删除安全规则） |
| excel-export（dev-kit/backend） | 后端工程师分阶段开发 SaaS Excel 导出功能（EasyExcel/POI/JXLS），内置样式模板与决策树、测试指引 |
| beautiful-tooltip（dev-kit/front） | 为界面元素设计/实现 Tooltip（悬停提示）：设计规范 + 文案规范 + 可视示例 |

## 使用方式

把需要的 skill 目录复制（或用目录链接/junction 挂载）到你的 Agent 技能目录即可，例如 Claude Code / ZCode 的 `~/.claude/skills/` 或 `~/.zcode/skills/`：

```bash
# 以 junction 方式挂载（Windows，源目录保持单一真身）
cmd /c mklink /J "%USERPROFILE%\.zcode\skills\tapd-cli" "<本仓库路径>\plugins\superteam-pack\skills\tencent-ecosystem-kit\tapd-cli"
```

## 说明

- 本仓库采用**白名单发布**：`.gitignore` 默认忽略一切，仅逐级放行上述 skill 目录；仓库其余内容（含完整技能清单 `skills/index.md`）不发布。
- Skill 文件中的账号名、公司 ID 等轻度标识为个人配置示例，仓库为 private。
