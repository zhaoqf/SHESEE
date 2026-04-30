## Why

当前 HTML 页面（北京25年1+3项目.html）中的学校数据和时间表数据以硬编码的 JavaScript 变量形式存在，导致数据更新需要手动修改 HTML 文件，容易出现数据不一致的问题。将数据分离到独立的 JSON 文件中，实现数据与渲染逻辑的解耦。

## What Changes

- 将 `citySchools`、`districtSchools`、`timelineData` 三个静态变量替换为动态 fetch 调用
- JSON 数据文件格式从"数组的数组"改为"对象数组"，提升可读性和可维护性
- 页面加载时通过 `Promise.all` 并行获取三个 JSON 文件
- 添加加载失败的用户友好提示

## Capabilities

### New Capabilities

- `json-data-fetch`: 从 `data/2025/` 目录动态加载学校和时间表数据
  - `city_schools.json` → 市级实验学校数据
  - `district_schools.json` → 区级实验学校数据
  - `timeline.json` → 工作时间表数据

## Impact

- 修改文件: `北京25年1+3项目.html`
- 新增/修改文件: `data/2025/*.json`（需转换格式）
