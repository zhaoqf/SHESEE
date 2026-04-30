## Context

当前 index.html 已实现：
- 动态加载 JSON 数据（市区级学校、时间表）
- 表格展示 + 筛选（区域下拉、名称搜索）
- 编辑弹窗 + File System Access API 写入
- 培养计划列排序

现有数据仅支持 2025 年度，存储在 `data/2025/` 目录。需扩展为多年度可视化看板。

## Goals / Non-Goals

**Goals:**
- 新增独立看板视图，展示各年度数据分布
- 年度选择器支持多年度切换
- ECharts 地图可视化各区学校数量
- 点击地图区域触发列表筛选（与现有下拉框行为一致）
- 左侧统计指标：全市学校总数、总招生计划

**Non-Goals:**
- 不修改现有 index.html 的表格和编辑功能
- 不实现数据编辑功能的地图端操作
- 右侧板块暂不开发
- 历史数据迁移（仅扩展数据结构）

## Decisions

### 1. 年度数据目录结构

**Decision:** `data/<year>/` 按年度分目录，每年度独立 JSON 文件

**Rationale:**
```
data/
  2025/
    city_schools.json
    district_schools.json
    timeline.json
  2024/
    city_schools.json
    district_schools.json
    timeline.json
```

**Alternative:** 在现有 JSON 中增加 `year` 字段
- **Rejected:** 需修改现有数据结构和所有读取逻辑，工作量大

### 2. 页面入口

**Decision:** 在现有 index.html 增加新 Tab "数据看板"

**Rationale:** 保持单页应用结构，避免多文件维护。用户切换 Tab 即可访问原有列表或看板。

**Alternative:** 新建 dashboard.html
- Rejected: 增加部署复杂度，需保持两套页面同步样式

### 3. 地图技术

**Decision:** ECharts 5.x + 北京区级 GeoJSON

**Rationale:**
- 项目已有静态 HTML + CDN 依赖，无构建工具
- ECharts 地图支持按需加载，CDN 获取方便
- 区级 GeoJSON 可从公开数据源获取（如阿里云 DataV GeoJSON）

**Alternative:** Leaflet + 地理数据
- Rejected: 需额外引入 Leaflet CSS/JS，ECharts 集成更简洁

### 4. 地图数据关联

**Decision:** 通过 district 字段关联学校数据与地图区域

**Rationale:**
- 现有 district_schools.json 中每条记录已有 `district` 字段
- 北京区划名称稳定，ECharts GeoJSON 使用相同命名

### 5. 筛选交互

**Decision:** 点击地图区域时，设置对应下拉筛选值并触发筛选函数

**Rationale:**
- 复用现有 filterDistrictSchools() 函数，保持行为一致
- 无需新增 API 或重新加载数据

## Risks / Trade-offs

| Risk | Mitigation |
|------|-----------|
| GeoJSON 区域名称与 JSON 数据中 district 不一致 | 预处理映射表处理（如 "门头沟区" vs "门头沟"） |
| 年度切换时地图状态残留 | 年度切换时调用 echartsInstance.clear() 后重渲染 |
| ECharts 地图在移动端体验差 | 响应式约束：看板在大屏优先，移动端显示提示 |

## Open Questions

1. 2024 年等历史数据是否有可靠来源？
2. 是否需要显示各区学校数量标签（数字标注）？
3. 地图配色方案：是否需要自定义？
