## 1. JSON 数据格式转换

- [x] 1.1 将 `data/2025/city_schools.json` 从数组格式转换为对象数组格式
- [x] 1.2 将 `data/2025/district_schools.json` 从数组格式转换为对象数组格式
- [x] 1.3 将 `data/2025/timeline.json` 从数组格式转换为对象数组格式

## 2. HTML 页面改造

- [x] 2.1 移除 `北京25年1+3项目.html` 中的硬编码数据变量（`citySchools`、`districtSchools`、`timelineData`）
- [x] 2.2 实现 `loadData()` 函数，使用 `Promise.all` 并行 fetch 三个 JSON 文件
- [x] 2.3 添加 JSON 解析和数据验证逻辑
- [x] 2.4 添加加载失败时的错误提示 UI
- [x] 2.5 将渲染函数调用从 `DOMContentLoaded` 中的同步调用改为 fetch 成功后的异步调用
