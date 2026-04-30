## Why

当前页面只能展示学校数据，无法进行维护操作。数据更新需要手动编辑 JSON 文件后再上传，不够便捷。增加学校数据的维护功能，让用户可以直接在页面编辑学校信息并保存到 JSON 文件。

## What Changes

- 增加学校数据单条记录编辑功能：点击编辑按钮 → 弹出编辑表单 → 修改字段 → 保存
- 改进网址列显示：直接显示完整 URL 文本（可复制）
- 利用 File System Access API 实现浏览器直接写入本地 JSON 文件
- 市级学校和区级学校各自独立的编辑能力

## Capabilities

### New Capabilities

- `school-edit`: 学校数据单条记录编辑
  - 点击编辑按钮打开编辑弹窗
  - 可编辑字段：学校名称、培养计划、官方网站
  - 区级学校额外可编辑"区域"字段
  - 保存时直接写入对应 JSON 文件（city_schools.json / district_schools.json）

### Modified Capabilities

- `json-data-fetch`: 现有数据加载能力保持不变，网址显示方式变更
  - 网址从"访问"链接改为直接显示完整 URL 文本

## Impact

- 修改文件: `index.html`（增加编辑 UI、弹窗、写入逻辑）
- JSON 数据文件: `data/2025/city_schools.json`、`data/2025/district_schools.json`（可被写入修改）
- 依赖: File System Access API（Chrome/Edge 支持）
