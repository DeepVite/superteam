# 技术选型决策树（轮次 1 技术向讨论时使用）

选型看三个变量：**数据量**、**样式复杂度**、**样式是否频繁变更**。按下面的顺序判断。

## 快速决策

```
样式/结构经常变，且有业务方能维护的 .xlsx 模板？
    └─ 是 → 模板引擎（JXLS / EasyExcel 模板填充）
    └─ 否 ↓

数据量 > 5 万行，或要求低内存？
    ├─ 样式简单（单级表头、标准行列）→ EasyExcel 注解导出
    └─ 样式复杂（多级表头+合并+复杂合计）→ EasyExcel + 自定义 CellWriteHandler
                                          或 POI SXSSF 流式写出
数据量 ≤ 5 万行？
    ├─ 样式简单 → EasyExcel 注解导出（开发最快）
    └─ 样式复杂、布局不规则（交叉报表、合并区域多）→ Apache POI 代码画表格
```

## 三个方案对比

### 1. EasyExcel（阿里，注解式）

- **适合**：标准列表导出（绝大多数 SaaS 导出场景）、大数据量。
- **优点**：API 极简（一个实体类 + 一行 `EasyExcel.write()`）；内存控制好（默认流式读，写可配）；社区活跃。
- **缺点**：样式定制要靠 `WriteHandler`/`CellStyleStrategy`，复杂布局写起来绕。
- **样式做法**：自定义 `AbstractCellStyleStrategy` 或 `CellWriteHandler`，把统一样式规范实现一遍，全系统复用。

### 2. Apache POI（代码画表格）

- **适合**：布局不规则的报表（交叉表、复杂多级表头、大量合并单元格、图表）、样式控制要求最细的场景。
- **优点**：控制力最强，什么都能画。
- **缺点**：代码量大；`XSSFWorkbook` 全量加载内存，大数据量必须换 `SXSSFWorkbook`（流式，但功能有裁剪，如不能读回、部分样式 API 受限）。
- **样式做法**：建一个 `ExcelStyles` 工具类，预建 CellStyle（表头/正文/合计/金额/日期），注意 **CellStyle 有数量上限（约 64000/工作簿）**，必须复用不能每格 new。

### 3. 模板引擎（JXLS2 / EasyExcel 模板填充）

- **适合**：样式由公司统一模板决定、模板样式经常调整、有复杂固定格式（发票、对账单、合同附件）。
- **优点**：样式完全在 .xlsx 模板文件里维护，改样式不用改代码、不用发版；业务方可以参与改模板。
- **缺点**：模板制作维护有成本；动态列（列数不固定）支持弱。
- **注意**：模板文件要纳入版本管理，模板里的占位符语法（JXLS 是 jx:each 等）要写进团队文档。

## 选型建议输出格式

给用户（可转发团队）的建议按这个结构写：

```markdown
【导出功能技术选型建议】
- 需求要点：<数据量 X 行 / 样式复杂度 / 是否有模板>
- 推荐方案：<EasyExcel 注解导出 / POI / JXLS 模板>
- 理由：<2-3 条>
- 代价：<这个方案的主要限制，1-2 条>
- 大数据量预案：<如适用：分页查询 + 流式写出 / 异步生成 + 通知下载>
请确认，或有其他倾向请回复。
```

## 依赖坐标速查（Maven）

```xml
<!-- EasyExcel -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>easyexcel</artifactId>
    <version>3.3.4</version>
</dependency>

<!-- Apache POI -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>5.2.5</version>
</dependency>

<!-- JXLS -->
<dependency>
    <groupId>org.jxls</groupId>
    <artifactId>jxls-poi</artifactId>
    <version>2.14.0</version>
</dependency>
```

版本号以项目实际依赖管理为准，冲突时注意 EasyExcel 内置了 POI，版本要对齐。
