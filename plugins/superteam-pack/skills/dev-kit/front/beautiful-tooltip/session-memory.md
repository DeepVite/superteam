# session-memory — beautiful-tooltip

> 记录本 skill 的建设过程、与产品经理达成的共识、踩过的坑。新会话只读本文件即可接续工作。
> 与 `superteam-pack\skills\session-memory.md`（skill 仓库工作日志）不是同一份，勿混。

## 当前任务与目标

建设 `beautiful-tooltip` skill：让前端工程师在收到模糊需求后，按统一规范设计 tooltip、自动产出用户友好的文案，并生成一份自包含 HTML 交给产品经理确认，从而免除产品经理自己写 tooltip 文案和排版的工作。

skill 只规定四件事：外观统一、交互统一、文案怎么写、交付什么。**组件化程度（共享组件还是页面内维护）不写进 skill，由前端自行决定。**

## 已完成（2026-09-08）

- 在 `D:\OneDrive\根` 产出两份设计稿：
  - `tooltip-样式10选.html` — 10 种风格 × 8 种内容的矩阵选型稿
  - `tooltip-02-浅色卡片.html` — 选定的 02 号「浅色卡片」外壳 + 22 种文案原型 + 12 种箭头位置 + 深浅色 + 4 色主题切换 + 调试面板
- 创建 skill 目录 `skills/dev-kit/front/beautiful-tooltip`：
  - `SKILL.md`（195 行）— 工作流 + 三部分规范
  - `reference/design-spec.md` — 设计令牌、尺寸、宽度/对齐/箭头完整规则、量化红线、交互时序
  - `reference/copywriting.md` — 文案三步法、措辞规则、22 个内容原型
  - `reference/tooltip-example.html` — 参考实现（由根目录 `tooltip-02-浅色卡片.html` 复制）
- 补上 `docs/10-more-tooltip-style.html`（原为 0 字节空文件），放入矩阵稿
- 修复模板深色模式漏改的蓝调令牌

## 关键决策与共识（产品经理确认）

1. **宽度**：固定 300px 为默认（可 240 / 300 / 360）；仅「单行且 ≤12 字、无结构、无图标、无按钮」用自适应（min 160 / max 300）。可交互内容一律固定——按钮点击区域不能漂移。
2. **对齐**：单行居中；多行居左；带图标/头像/按钮/列表/数据一律居左。
3. **箭头**：默认 tooltip 在锚点**上方**、箭头下边居中（`.a-bottom-center`，等同 `placement=top`）；上方空间不足自动翻转；靠视口边缘 / 窄锚点 / 表格列 / 防遮挡时偏移到起始或末尾。
4. **交互时序**：悬停 400ms 出现 / 移开 150ms 消失 / 键盘 focus 立即 / Esc 关闭 / 触屏点按切换。
5. **量化红线**：短句 ≤12 字（同时是自适应判据和居中判据）、说明段 ≤3 行、标签 ≤3 个、按钮 ≤2 个、列表项 ≤5 条。
6. **深色底**：中性灰（`#101012` / `#1c1c1f`），**不要蓝调**。
7. **主题色**：默认橙色 `#FF8D00`；预设蓝 / 紫 / 绿 / 橙，蓝色在第一位；**不记忆选择**，刷新回默认；橙色按钮用白字（产品经理明确要求，已知对比度仅 2.3:1）。
8. **第三部分 = 交付物规范**（产品经理确认）：自包含单文件 HTML、命名 `tooltip-<功能名>-预览.html`、文案集中放便于直接改、不提交主分支。
9. **本机不装 junction**（产品经理决定）：skill 随 superteam-pack 分发，直接打包发给前端工程师。
10. **箭头默认位置**（2026-09-08 拍板）：tooltip 在锚点上方、箭头下边居中 `.a-bottom-center`（等同 `placement=top`），与实现一致。
11. **用出经验就回写 skill**（2026-09-08 新增，已写入 SKILL.md）：使用中发现更好做法时，先按正确做法把当前任务做完，再把经验沉淀成 skill 改动并提 PR 到本 skill 仓库，同时在 `session-memory.md` 追加一条记录。

## 踩过的坑与经验

- **深色令牌有两份**：媒体查询 `@media (prefers-color-scheme: dark)` 和 `html[data-mode="dark"]` 各一份，改一份会漏。曾导致手动切「深色」仍偏蓝。改深色务必两处同步。
- **批量替换要看缩进**：两份令牌块缩进不同（6 空格 vs 4 空格），按其中一份做全量替换只会命中一处。
- **自适应宽度会压塌空元素**：`fit-content` 下，空 div + `flex:1` 的柱状图、进度条会塌成 0 宽，必须给 `min-width` 兜底。
- **深色下 `--danger` 会自动调亮**成浅鲑红 `#f87171`，若当按钮填充配白字对比度只有 2.2:1。按钮填充要单独用 `--danger-solid`（两种模式都保持 `#dc2626`）。
- **命名陷阱**：`.a-top-*` 指**箭头在 tooltip 的上边**（箭头朝上 → tooltip 在锚点下方），与直觉相反。规范里已加「命名语义」说明，防止前端搞反。
- **无头 Chrome 默认报告系统为深色**，截图验证浅色时必须同时改 `data-mode` 属性和 JS 里的 `applyMode()` 默认值，只改一个会被 JS 覆盖。
- **验收要用取色器**：曾把按钮边缘的抗锯齿像素 `#F12626` 误判为填充色出错，用众数采样才定位到真实值 `#dc2626`。

## 下一步 / 未完成

- ~~待拍板：箭头默认位置的术语冲突~~ 已拍板（2026-09-08）：默认 `.a-bottom-center`（tooltip 在锚点上方）。
- **待办**：拿 2–3 个真实需求试跑 skill，验证产出是否符合预期。
- **待办**：发给前端工程师试用，收集反馈后迭代。
- **待补**：本 skill 仓库的 GitHub 地址——`superteam/.git` 没有配置 remote（`git remote -v` 为空），SKILL.md 里暂用「superteam-pack 的 git 仓库」描述式写法；拿到地址后换成链接。

## 相关文件路径

| 用途 | 路径 |
|---|---|
| skill 主文件 | `superteam-pack\skills\dev-kit\front\beautiful-tooltip\SKILL.md` |
| 设计规范 | `…\beautiful-tooltip\reference\design-spec.md` |
| 文案规范 | `…\beautiful-tooltip\reference\copywriting.md` |
| 参考实现 | `…\beautiful-tooltip\reference\tooltip-example.html` |
| 选型历史（10 风格） | `…\beautiful-tooltip\docs\10-tooltip-style.html` |
| 选型历史（风格 × 内容矩阵） | `…\beautiful-tooltip\docs\10-more-tooltip-style.html` |
| 源稿（22 用例成品） | `D:\OneDrive\根\tooltip-02-浅色卡片.html` |
| 源稿（10 风格矩阵） | `D:\OneDrive\根\tooltip-样式10选.html` |
