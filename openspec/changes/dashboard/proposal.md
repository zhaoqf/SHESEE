## Why

当前页面仅以表格形式展示学校数据，缺乏直观的空间分布可视化。用户无法快速了解各区参与学校数量分布，也无法通过年度维度对比历史趋势。增加可视化看板可提升数据可读性和交互体验。

## What Changes

- 新增**看板视图**（Dashboard），独立于现有学校列表页面
- 顶部增加**年度选择器**，支持下拉切换（2025年起步，逐步扩展历史数据）
- 中间区域使用**ECharts 地图**展示北京市各区参与1+3项目的学校数量分布
- 点击地图区域触发列表筛选，行为与现有"区级实验学校"下拉框一致
- 左侧增加**统计指标面板**：全市学校总数、全市总招生计划
- 保留现有表格视图作为底层数据入口

## Capabilities

### New Capabilities

- `school-dashboard`: 可视化看板，支持年度切换、地图分布展示、左侧统计指标
- `school-map`: 基于ECharts的北京区级地图，渲染各区学校数量，点击筛选功能

### Modified Capabilities

- （无）

## Impact

- 新增页面入口：`dashboard.html`（或作为 index.html 新 Tab）
- 依赖 ECharts 5.x CDN
- 需要北京市区级 GeoJSON 地理数据（公开数据源）
- data 目录结构需支持多年度：`data/<year>/district_schools.json`
