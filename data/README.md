# 数据目录说明

本目录按年度存储 1+3 项目的 JSON 数据。

## 目录结构

```
data/
├── README.md           # 本文件
├── 2025/
│   ├── city_schools.json      # 市级实验学校数据
│   ├── district_schools.json   # 区级实验学校数据
│   └── timeline.json          # 工作时间表
├── 2024/
│   └── ...              # 历史数据（逐步添加）
└── <未来年份>/
    └── ...
```

## JSON 文件格式

### city_schools.json (市级学校)
```json
[
  { "name": "学校名称", "plan": 招生名额, "website": "网址或'未知'" }
]
```

### district_schools.json (区级学校)
```json
[
  { "district": "区名", "name": "学校名称", "plan": 招生名额, "website": "网址或'未知'" }
]
```

### timeline.json (时间表)
```json
[
  { "date": "日期", "content": "事项描述" }
]
```

## 添加新年度数据

1. 在本目录下创建新年度文件夹（如 `2024/`）
2. 复制对应 JSON 文件并更新数据
3. 年度选择器将自动识别可用年度
