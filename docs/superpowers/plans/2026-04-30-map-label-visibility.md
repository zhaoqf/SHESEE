# 地图标签可读性优化实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 优化地图标签在不同背景色下的可读性

**Architecture:** 修改 ECharts label 配置，使用函数动态返回文字颜色，高数据区域用白色，低数据区域用深色

**Tech Stack:** 纯前端 HTML + ECharts

---

### Task 1: 修改地图标签配置

**Files:**
- Modify: `index.html:1610-1625`

- [ ] **Step 1: 修改 label 配置**

将第 1610-1614 行：
```javascript
label: {
    show: true,
    fontSize: 10,
    color: '#333'
},
```

改为：
```javascript
label: {
    show: true,
    fontSize: 10,
    color: function(params) {
        const value = params.data?.value || 0;
        const threshold = maxCount * 0.4;
        return value >= threshold ? '#ffffff' : '#333333';
    },
    borderWidth: 1,
    borderColor: 'rgba(255,255,255,0.5)'
},
```

- [ ] **Step 2: 测试验证**

刷新 http://localhost:3000 ，切换到"数据看板"标签，检查地图标签：
- 高学校数量的区域（颜色深的）应显示白色文字
- 低学校数量的区域（颜色浅的）应显示深色文字
- 鼠标悬停高亮时应正常显示

- [ ] **Step 3: 提交**

```bash
git add index.html
git commit -m "feat(dashboard): improve map label readability with dynamic colors"
```

---

**Spec 覆盖检查：**
- ✅ 问题描述：标签可读性差
- ✅ 解决方案：动态文字颜色
- ✅ 实现细节：threshold = maxCount * 0.4
- ✅ 预期效果：表格对照

**无占位符，所有步骤完整。**

---

Plan complete. Two execution options:

**1. Subagent-Driven (recommended)** - I dispatch a fresh subagent per task, review between tasks, fast iteration

**2. Inline Execution** - Execute tasks in this session using executing-plans, batch execution with checkpoints

**Which approach?**
