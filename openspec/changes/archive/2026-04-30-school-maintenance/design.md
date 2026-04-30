## Context

当前 `index.html` 页面中的学校数据通过 fetch 从 JSON 文件读取并展示，但仅有只读能力。用户需要编辑学校信息时，必须手动打开 JSON 文件、找到对应记录、修改后保存，再刷新页面。

现有架构：
```
浏览器 ──fetch GET──▶ JSON 文件 (只读)
                        ▲
                        │
                    (无写入通道)
```

## Goals / Non-Goals

**Goals:**
- 实现学校数据单条记录的编辑功能
- 使用 File System Access API 实现浏览器直接写入 JSON 文件
- 改进网址列显示，直接展示完整 URL 文本

**Non-Goals:**
- 不实现 Timeline 编辑功能
- 不实现批量编辑
- 不实现新增/删除学校记录
- 不支持 Firefox/Safari（File System Access API 限制）

## Decisions

### Decision 1: 编辑方式 — 单条记录弹窗编辑

**选择:** 点击编辑按钮 → 弹窗表单 → 保存

```
┌─────────────────────────────────────────┐
│  编辑学校信息                        [X] │
├─────────────────────────────────────────┤
│  学校名称: [________________]            │
│  区    域: [________▼] (仅区级)         │
│  培养计划: [____]                       │
│  官方网站: [________________]           │
│                                         │
│           [取消]  [保存]                 │
└─────────────────────────────────────────┘
```

**理由:**
- 用户友好，无需理解 JSON 结构
- 避免整文件编辑导致的格式损坏
- 符合"维护"的直觉操作

### Decision 2: 写入机制 — File System Access API

**选择:** 使用 `showSaveFilePicker()` 获取文件句柄，`createWritable()` 写入

```javascript
// 伪代码
const handle = await window.showSaveFilePicker({
  suggestedName: 'city_schools.json',
  types: [{ description: 'JSON', accept: { 'application/json': ['.json'] } }]
});
const writable = await handle.createWritable();
await writable.write(JSON.stringify(data, null, 2));
await writable.close();
```

**理由:**
- 无需后端服务，纯前端实现
- 用户授权一次后可多次写入同一文件
- 已有标准规范，Chrome/Edge 支持良好

### Decision 3: 网址显示 — 纯文本展示

**选择:** 直接显示 URL 文本，不加链接样式

**现状:**
```html
<td>http://qhfzwjxx.bjchyedu.cn/</td>
```

**理由:**
- URL 可直接复制粘贴
- 避免链接样式干扰阅读
- 简化实现复杂度

### Decision 4: 文件选择策略

**选择:** 首次保存时让用户选择文件，之后记住 handle 复用

```javascript
// 首次: 用户选择 city_schools.json
// 之后: 复用同一 handle，无需再次选择
```

**理由:**
- 减少重复选择操作
- 保持原有文件路径
- 用户体验更流畅

## Risks / Trade-offs

| Risk | Mitigation |
|------|------------|
| Chrome/Edge 以外浏览器不支持 | 在页脚添加浏览器兼容性提示 |
| 用户误操作导致 JSON 格式损坏 | 保存前验证 JSON 结构，无效则提示错误 |
| 文件被其他程序占用导致写入失败 | 捕获异常，显示友好错误提示 |
| 用户取消授权 | 不执行写入，保留内存中修改的数据 |

## Open Questions

无
