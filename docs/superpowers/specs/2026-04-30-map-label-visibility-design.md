# 地图标签可读性优化

## 问题

数据看板中，ECharts 地图的 `visualMap` 配色方案使用蓝渐变色（浅蓝 → 深蓝）。当某区域学校数量较多时，颜色变得很深，而标签文字固定为深灰色 `#333`，导致对比度不足、文字看不清楚。

## 解决方案

实现**动态文字颜色**，根据背景颜色深浅自动切换：

- 高数据区域（深蓝背景）→ 白色文字 `#ffffff`
- 低数据区域（浅蓝背景）→ 深色文字 `#333333`

同时添加文字描边增强可读性。

## 实现细节

### 修改位置

`index.html` 第 1610-1620 行，ECharts `series[0].label` 配置

### 最终代码

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

### 设计决策

- 统一使用白色文字 `#ffffff`
- 添加黑色文字阴影增强可读性，适用于所有背景色
- 方案比动态颜色切换更简单可靠

## 风险评估

- **风险等级**：低
- **影响范围**：仅影响地图标签颜色
- **回滚方案**：恢复固定 `color: '#333'` 即可
