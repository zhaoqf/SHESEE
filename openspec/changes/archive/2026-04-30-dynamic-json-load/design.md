## Context

当前 `北京25年1+3项目.html` 页面中，学校数据（`citySchools`、`districtSchools`）和时间表数据（`timelineData`）以硬编码 JavaScript 数组形式存在。数据源位于 `data/2025/` 目录下的 JSON 文件，但格式为"数组的数组"（类似 CSV 表格行），与当前 JS 变量结构不完全匹配。

```
现有架构:
  data/2025/*.json  (数组格式)  ←── 不匹配
        ↓ 手动同步
  HTML 内嵌 JS 变量 (对象数组)
```

## Goals / Non-Goals

**Goals:**
- 实现数据与渲染逻辑分离
- JSON 数据从页面中独立出来，便于单独更新
- 页面加载时自动从 JSON 文件获取数据
- 加载失败时显示友好提示

**Non-Goals:**
- 不实现定时轮询（按用户要求）
- 不修改页面 UI 样式和交互逻辑
- 不实现离线缓存

## Decisions

### Decision 1: JSON 格式转换

**选择:** 将 JSON 从"数组的数组"改为"对象数组"

```json
// 之前 (数组格式)
[["序号","学校名称","培养计划","官方网站"], ["1","中央工艺美术学院附属中学","140","未知"], ...]

// 之后 (对象数组)
[
  { "name": "中央工艺美术学院附属中学", "plan": 140, "website": "未知" },
  ...
]
```

**理由:**
- 数据结构与 JS 代码期望一致，无需转换函数
- 可读性更高，便于人工维护
- 扩展字段更方便

### Decision 2: 数据加载时机

**选择:** 页面 `DOMContentLoaded` 时加载

**理由:**
- 符合标准前端模式
- 确保 DOM 元素存在后再渲染
- 用户首次访问即可看到数据

### Decision 3: 错误处理策略

**选择:** 加载失败时显示错误提示，保留 DOM 结构

```javascript
// 伪代码
async function loadData() {
  try {
    const [city, district, timeline] = await Promise.all([
      fetch('data/2025/city_schools.json'),
      fetch('data/2025/district_schools.json'),
      fetch('data/2025/timeline.json')
    ]);
    // 渲染成功
  } catch (error) {
    // 显示加载失败提示
  }
}
```

**理由:**
- 不显示陈旧数据（无缓存）
- 错误隔离，单个文件失败不影响其他
- 提供明确的用户反馈

## Risks / Trade-offs

| Risk | Mitigation |
|------|------------|
| 服务器未启动时页面显示空白 | 提供明确的"加载失败"提示，引导用户启动服务器 |
| JSON 格式错误导致解析失败 | 加载后验证数据格式，无效则提示错误 |
| 大文件加载慢 | JSON 文件体积小（<100KB），影响可忽略 |

## Open Questions

无
