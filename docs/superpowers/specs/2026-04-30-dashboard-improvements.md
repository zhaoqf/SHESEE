# 数据看板模块优化

> 日期：2026-04-30
> 完成状态：✅ 已完成

## 概述

对北京市"1+3"培养实验管理系统的数据看板模块进行了多项 UI 优化，提升用户体验。

---

## 优化清单

### 1. 指标卡片位置调整

**问题：** 原有"全市学校总数"和"总招生计划"指标位于左侧区域。

**解决方案：** 将两个指标卡片移至"选择年度"选择器所在的容器，改为胶囊样式显示。

**文件：** `index.html`

**样式：**
```css
.stat-pills {
    display: flex;
    gap: 16px;
    margin-left: auto;
}

.stat-pill {
    display: flex;
    align-items: center;
    gap: 8px;
    background: white;
    padding: 8px 16px;
    border-radius: 20px;
    box-shadow: 0 2px 8px rgba(74, 144, 217, 0.15);
}
```

---

### 2. 左侧区域 - 学校排名

**问题：** 左侧区域为空，利用率低。

**解决方案：** 在左侧区域添加学校排名卡片，显示按招生人数排序的前10所学校。

**文件：** `index.html`

**实现：**
- 新增 `renderSchoolRanking()` 函数
- 合并市级和区级学校数据，按 `plan` 降序排列
- 截断过长学校名称（超过10字符显示省略号）

```javascript
function renderSchoolRanking() {
    const allSchools = [...citySchools, ...districtSchools]
        .filter(s => s.plan > 0)
        .sort((a, b) => (b.plan || 0) - (a.plan || 0))
        .slice(0, 10);
    // ...
}
```

---

### 3. 右侧区域 - 各区排名限制

**问题：** 各区排名显示全部区域，内容过多。

**解决方案：** 限制显示前10名。

**文件：** `index.html`

**修改：** 在 `renderRanking()` 函数中添加 `.slice(0, 10)`。

---

### 4. 页面默认展示数据看板

**问题：** 页面默认展示"区级实验学校"列表。

**解决方案：** 将默认激活的 tab 和 content 改为"数据看板"。

**文件：** `index.html`

**修改：**
- `button.tab-btn.active` 从 `district` 改为 `dashboard`
- `div.tab-content.active#district` 改为 `dashboard`

---

### 5. 头部区域紧凑化

**问题：** 头部区域占用空间过大。

**解决方案：** 减小 padding 和字体大小。

**文件：** `index.html`

**修改：**
| 属性 | 原值 | 新值 |
|------|------|------|
| padding | 40px 20px | 24px 20px |
| h1 font-size | 2.2em | 1.6em |
| margin-bottom | 30px | 20px |

---

### 6. 地图标签可读性优化

**问题：** 深色背景区域（如学校数量多的区）的标签文字看不清。

**解决方案：** 统一使用白色文字 + 黑色阴影，增强在所有背景色下的可读性。

**文件：** `index.html`

**修改：**
```javascript
label: {
    show: true,
    fontSize: 10,
    color: '#ffffff',
    textShadowColor: 'rgba(0, 0, 0, 0.8)',
    textShadowBlur: 2,
    textShadowOffsetX: 1,
    textShadowOffsetY: 1
}
```

---

## 未完成项

- 地图放大需求（用户取消）

---

## Git 提交

```
Commit: 9507424
Branch: main
Remote: git@github.com:zhaoqf/SHESEE.git
Message: feat: 优化数据看板模块 - 地图标签可读性、默认展示看板、头部紧凑化、学校排名等功能
```
