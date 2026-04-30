# 区级与市级试验学校模块合并设计

## 概述

将"区级实验学校"和"市级试验学校"两个独立Tab合并为一个统一的"试验学校"模块，通过级别字段区分数据来源。

## UI层修改

### Tab合并
- 现有：`区级实验学校` 和 `市级试验学校` 两个Tab
- 合并后：单一Tab `试验学校`，**默认不激活**，需手动点击
- 命名统一使用"试验"（区级试验学校、市级试验学校）

### 表格列调整
| 列名 | 说明 |
|------|------|
| 级别 | 新增，区级/市级 |
| 区域 | 现有，区级数据有效，市级数据留空 |
| 学校名称 | 现有 |
| 招生人数 | 现有 |
| 操作 | 现有，编辑/删除 |

## 筛选器改动

### 统一筛选器
- **级别下拉**：全部 / 区级 / 市级（默认"全部"）
- **区域下拉**：全部 + 各区名称（仅在级别选择"区级"或"全部"时启用）
- **搜索框**：保留，按学校名称模糊搜索

## 数据层改动

### JSON文件
保持分离，不合并：
- `data/2025/district_schools.json` — 区级数据
- `data/2025/city_schools.json` — 市级数据

### 加载逻辑
- `loadAllSchools()` — 同时加载两个JSON，合并成一个数组
- 每条数据标记 `level: "district"` 或 `level: "city"`

### 保存逻辑
- `saveSchool(school)` — 根据 `school.level` 写回对应文件
- 区级 → `district_schools.json`
- 市级 → `city_schools.json`

## 函数合并

| 原函数对 | 合并后 |
|---------|--------|
| `renderDistrictSchools()` + `renderCitySchools()` | `renderSchools(data)` |
| `filterDistrictSchools()` + `filterCitySchools()` | `filterSchools()` |
| `sortDistrictSchools()` + `sortCitySchools()` | `sortSchools()` |
| `applyDistrictSort()` + `applyCitySort()` | `applySort()` |

### 新增函数
- `loadAllSchools()` — 合并加载两个JSON

### 修改函数
- `openEditModal(name, level)` — 保留level参数，支持编辑两个来源的数据
- `saveSchool()` — 根据level写回对应文件

## 实施步骤

1. 新增 `loadAllSchools()` 函数，合并加载逻辑
2. 修改Tab HTML，合并为一个Tab
3. 新增级别列，调整表格渲染 `renderSchools()`
4. 合并筛选函数 `filterSchools()` 和排序函数 `sortSchools()` / `applySort()`
5. 调整编辑弹窗和保存逻辑
6. 清理废弃的分离函数

## 风险与注意事项

- 保持两个JSON文件独立，避免数据相互污染
- 市级数据无区域字段，区域列显示为空
- 命名统一："试验"而非"实验"
