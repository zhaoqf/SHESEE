## ADDED Requirements

### Requirement: ECharts 地图渲染

系统 SHALL 使用 ECharts 在看板中央区域渲染北京市区级地图。

#### Scenario: 地图加载成功
- **WHEN** 页面加载且 GeoJSON 数据就绪
- **THEN** ECharts 地图正确渲染北京各区边界
- **AND** 各区颜色深浅表示该区学校数量（颜色梯度：少→多）

#### Scenario: GeoJSON 加载失败
- **WHEN** GeoJSON 数据获取失败
- **THEN** 地图区域显示加载失败提示
- **AND** 用户可重试

### Requirement: 区级学校数量热力图

系统 SHALL 根据各区学校数量设置地图颜色热力图。

#### Scenario: 计算热力分布
- **WHEN** 地图渲染前
- **THEN** 系统统计各区学校数量
- **AND** 按数量区间映射到颜色梯度

#### Scenario: 无学校区域
- **WHEN** 某区在当前年度没有学校参与
- **THEN** 该区显示为最浅颜色或灰色

### Requirement: 地图交互

系统 SHALL 支持鼠标悬停和点击地图区域。

#### Scenario: 悬停显示提示
- **WHEN** 用户鼠标悬停在某区上
- **THEN** 显示 Tooltip，包含"区名"和"学校数量"

#### Scenario: 点击筛选
- **WHEN** 用户点击某区
- **THEN** 触发该区筛选逻辑（见 school-dashboard 联动需求）

### Requirement: 地图数据兼容

系统 SHALL 处理 GeoJSON 区域名称与 JSON 数据中 district 字段的命名差异。

#### Scenario: 名称映射
- **WHEN** 地图渲染或数据统计时
- **THEN** 系统使用预定义的映射表处理名称差异
- **EXAMPLE** "门头沟区" in JSON ↔ "门头沟" in GeoJSON

### Requirement: 地图响应式

系统 SHALL 在窗口尺寸变化时调整地图大小。

#### Scenario: 窗口 resize
- **WHEN** 浏览器窗口尺寸变化
- **THEN** ECharts 实例调用 resize() 保持适配
