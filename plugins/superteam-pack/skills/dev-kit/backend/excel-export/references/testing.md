# 导出功能自动化测试清单（阶段 4 使用）

导出功能上线后最常见的两类回归：改代码时碰坏了**数据**，或碰坏了**样式**。测试要分别覆盖这两类。轮次 2 确认通过的成品 xlsx 是验收基准——断言的期望值都从它而来。

## 一、数据正确性测试（POI 读回断言）

用 Apache POI 把导出结果读回来，断言结构和值。模板（JUnit 5）：

```java
@Test
void export_containsExpectedData() throws Exception {
    byte[] bytes = exportService.export(query);   // 调真实导出逻辑
    try (Workbook wb = new XSSFWorkbook(new ByteArrayInputStream(bytes))) {
        Sheet sheet = wb.getSheetAt(0);
        assertEquals("订单明细", sheet.getSheetName());
        // 表头
        Row header = sheet.getRow(0);
        assertEquals("订单号", header.getCell(0).getStringCellValue());
        assertEquals("金额", header.getCell(3).getStringCellValue());
        // 数据行数
        assertEquals(4, sheet.getLastRowNum());   // 3 条数据 + 表头
        // 关键值（注意数字按数值断言，不是字符串）
        assertEquals(1234.56, sheet.getRow(1).getCell(3).getNumericCellValue(), 0.001);
        // 合并区域（多级表头/大标题场景）
        assertEquals(1, sheet.getNumMergedRegions());
        assertEquals("A1:F1", sheet.getMergedRegion(0).formatAsString());
    }
}
```

要点：数字断言用 `getNumericCellValue`——这能同时验证"存的是数值不是文本"（样式规范第五条的回归保障）。

## 二、样式回归测试

改动相邻代码时样式最容易被悄悄破坏，而又没人会在发版前逐格打开看。对关键单元格断言样式属性：

```java
@Test
void header_styleMatchesSpec() throws Exception {
    byte[] bytes = exportService.export(query);
    try (Workbook wb = new XSSFWorkbook(new ByteArrayInputStream(bytes))) {
        Cell headerCell = wb.getSheetAt(0).getRow(0).getCell(0);
        CellStyle style = headerCell.getCellStyle();
        Font font = wb.getFontAt(style.getFontIndex());

        assertTrue(font.getBold());
        assertEquals("微软雅黑", font.getFontName());
        assertEquals(10, font.getFontHeightInPoints());
        // 背景色
        assertEquals("F2F2F2",
            ((XSSFColor) style.getFillForegroundColorColor()).getARGBHex().substring(2));
        // 数字格式
        Cell amountCell = wb.getSheetAt(0).getRow(1).getCell(3);
        assertEquals("#,##0.00", amountCell.getCellStyle().getDataFormatString());
        // 冻结
        assertEquals(1, wb.getSheetAt(0).getPaneInformation().getHorizontalSplitTopRow());
    }
}
```

不需要逐格断言——抽**代表性单元格**（表头 1 格、文本列 1 格、数字列 1 格、合计行 1 格）即可覆盖样式工具类的输出。

## 三、视觉验证（可选，有多模态能力时）

样式断言只能验证"属性对"，验证不了"看起来对"。如果环境里有 LibreOffice：

```bash
soffice --headless --convert-to pdf 导出结果.xlsx
# 再转图片
pdftoppm -png -r 100 导出结果.pdf page
```

然后用多模态方式检查截图，重点看：

- 表头是否明显突出、层级清晰
- 有没有文字被截断（`###` 或溢出）
- 数字列是否右对齐、小数点对齐
- 边框是否完整连续、合并区域对不对
- 整体颜色是否克制（≤ 4 种）

## 四、边界用例清单

- **空数据**：导出 0 行不报错，表头正常，合计行不出现或显示 0（按需求定）。
- **null 字段**：单元格显示 `-` 或空，不出现 "null" 文本。
- **超长文本**：换行或截断符合规范，行高不爆炸。
- **特殊字符**：emoji、`"`, `\n`, 制表符不破坏文件；以 `=`/`+`/`-` 开头的文本防公式注入（前导空格或强制文本格式）。
- **大数字**：Long 型 ID（如订单号 19 位）必须按文本导出，否则 Excel 转成科学计数法丢精度——这是高发事故。
- **最大行数**：xlsx 上限 1,048,576 行/Sheet；大数据量场景验证流式写出的内存和耗时。
- **并发导出**：多人同时点导出不串数据、不 OOM。

## 五、测试通过标准

全部通过后输出收尾总结，包含：导出功能清单、技术栈、样式基准文件路径、测试覆盖项、遗留风险（如"超过 50 万行未实测"）。
