# 区级与市级实验学校模块合并实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将"区级实验学校"和"市级试验学校"两个独立Tab合并为一个"实验学校"模块

**Architecture:** 纯前端SPA，代码全在 `index.html`。合并后共用同一套渲染/筛选/排序逻辑，通过 `level` 字段区分数据来源，数据仍分离存储在各自的JSON文件中。

**Tech Stack:** 原生HTML/CSS/JavaScript，无框架依赖

---

## 文件变更概览

| 操作 | 文件路径 | 说明 |
|------|---------|------|
| 修改 | `index.html:820-901` | Tab按钮和内容区重组 |
| 修改 | `index.html:1353-1399` | loadData函数调整 |
| 修改 | `index.html:1053-1215` | 筛选/排序/渲染函数合并 |
| 修改 | `index.html:1222-1310` | openEditModal和saveSchool调整 |
| 修改 | `index.html:1690-1720` | 事件监听器调整 |

---

## Task 1: 新增 loadAllSchools() 函数

**Files:**
- Modify: `index.html:1353-1399` (loadData函数内部)

- [ ] **Step 1: 在 loadData 函数中添加 loadAllSchools() 逻辑**

在 `loadData()` 函数中，将 citySchools 和 districtSchools 的赋值改为合并数组：

```javascript
// 替换原有赋值逻辑（约第1379-1380行）
// 原有代码:
// citySchools = cityData;
// districtSchools = districtData;

// 新增代码 - 标记level并合并
citySchools = cityData.map(s => ({ ...s, level: 'city' }));
districtSchools = districtData.map(s => ({ ...s, level: 'district' }));

// 新增：合并后的数据数组（用于统一渲染）
window.allSchools = [...citySchools, ...districtSchools];
```

---

## Task 2: 修改Tab HTML结构

**Files:**
- Modify: `index.html:820-901`

- [ ] **Step 1: 合并两个Tab按钮为一个**

```html
<!-- 替换第820-821行 -->
<!-- 原有:
<button class="tab-btn" data-tab="district">区级实验学校</button>
<button class="tab-btn" data-tab="city">市级试验学校</button>
-->
<button class="tab-btn" data-tab="experimental">实验学校</button>
```

- [ ] **Step 2: 合并两个Tab内容区为一个**

替换第826-901行的两个tab-content为一个：

```html
<!-- 替换第826-901行 -->
<!-- 原有: <div class="tab-content" id="district">...</div> 和 <div class="tab-content" id="city">...</div> -->

<div class="tab-content" id="experimental">
    <h2 class="section-title">实验学校</h2>

    <div class="filter-bar">
        <div class="search-box">
            <input type="text" id="experimentalSearchInput" placeholder="搜索学校名称...">
        </div>
        <select class="level-select" id="levelSelect">
            <option value="">全部级别</option>
            <option value="district">区级</option>
            <option value="city">市级</option>
        </select>
        <select class="district-select" id="experimentalDistrictSelect">
            <option value="">全部区域</option>
            <option value="东城区">东城区</option>
            <option value="西城区">西城区</option>
            <option value="朝阳区">朝阳区</option>
            <option value="海淀区">海淀区</option>
            <option value="丰台区">丰台区</option>
            <option value="石景山区">石景山区</option>
            <option value="门头沟区">门头沟区</option>
            <option value="房山区">房山区</option>
            <option value="通州区">通州区</option>
            <option value="顺义区">顺义区</option>
            <option value="昌平区">昌平区</option>
            <option value="大兴区">大兴区</option>
            <option value="平谷区">平谷区</option>
            <option value="怀柔区">怀柔区</option>
            <option value="密云区">密云区</option>
            <option value="延庆区">延庆区</option>
            <option value="燕山">燕山</option>
            <option value="经开区">经开区</option>
        </select>
    </div>

    <div class="stats" id="experimentalStats"></div>
    <div class="table-wrapper">
        <table>
            <thead>
                <tr>
                    <th>序号</th>
                    <th>级别</th>
                    <th>区域</th>
                    <th>学校名称</th>
                    <th class="sortable" onclick="sortSchools('plan')">培养计划</th>
                    <th>官方网站</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody id="experimentalSchools"></tbody>
        </table>
    </div>
    <div class="no-results" id="experimentalNoResults" style="display:none;">未找到符合条件的学校</div>
</div>
```

---

## Task 3: 合并渲染函数

**Files:**
- Modify: `index.html:1107-1170` (renderCitySchools + renderDistrictSchools)

- [ ] **Step 1: 创建统一的 renderSchools 函数**

替换 `renderCitySchools` 和 `renderDistrictSchools` 函数为 `renderSchools`：

```javascript
// 替换第1107-1171行的两个函数为一个
function renderSchools(schools) {
    const tbody = document.getElementById('experimentalSchools');
    const noResults = document.getElementById('experimentalNoResults');
    const stats = document.getElementById('experimentalStats');

    if (!schools || schools.length === 0) {
        tbody.innerHTML = '';
        noResults.style.display = 'block';
        stats.textContent = '共 0 所学校';
        return;
    }

    noResults.style.display = 'none';
    stats.textContent = `共 ${schools.length} 所学校`;

    tbody.innerHTML = schools.map((school, index) => {
        const levelText = school.level === 'city' ? '市级' : '区级';
        const levelClass = school.level === 'city' ? 'level-city' : 'level-district';

        return `<tr>
            <td>${index + 1}</td>
            <td><span class="level-tag ${levelClass}">${levelText}</span></td>
            <td>${school.district || '-'}</td>
            <td>${school.name}</td>
            <td>${school.plan || '-'}</td>
            <td>${school.website ? `<a href="${school.website}" target="_blank">访问</a>` : '-'}</td>
            <td><button class="edit-btn" onclick="openEditModal('${school.name}', '${school.level}')">编辑</button></td>
        </tr>`;
    }).join('');
}
```

---

## Task 4: 合并筛选和排序函数

**Files:**
- Modify: `index.html:1053-1210` (filter/sort函数对)

- [ ] **Step 1: 创建统一的 filterSchools 函数**

替换第1173-1192行的两个filter函数为一个：

```javascript
// 替换第1173-1192行的两个filter函数
function filterSchools() {
    const searchText = document.getElementById('experimentalSearchInput').value.toLowerCase();
    const levelFilter = document.getElementById('levelSelect').value;
    const districtFilter = document.getElementById('experimentalDistrictSelect').value;

    let filtered = [...allSchools];

    // 级别筛选
    if (levelFilter) {
        filtered = filtered.filter(s => s.level === levelFilter);
    }

    // 区域筛选（仅对区级有效，市级无district字段）
    if (districtFilter && (levelFilter === '' || levelFilter === 'district')) {
        filtered = filtered.filter(s => s.district === districtFilter);
    }

    // 搜索筛选
    if (searchText) {
        filtered = filtered.filter(s => s.name.toLowerCase().includes(searchText));
    }

    renderSchools(applySort(filtered));
}
```

- [ ] **Step 2: 创建统一的 sortSchools 和 applySort 函数**

替换第1053-1075行和第1195-1210行的四个sort函数为两个：

```javascript
// 替换第1053-1075行
function sortSchools(field) {
    if (currentSortField === field) {
        currentSortDirection = currentSortDirection === 'asc' ? 'desc' : 'asc';
    } else {
        currentSortField = field;
        currentSortDirection = 'asc';
    }
    filterSchools();
}

// 替换第1195-1210行
function applySort(data) {
    if (!currentSortField) return data;

    return [...data].sort((a, b) => {
        let valA = a[currentSortField];
        let valB = b[currentSortField];

        if (typeof valA === 'string') valA = valA.toLowerCase();
        if (typeof valB === 'string') valB = valB.toLowerCase();

        if (valA < valB) return currentSortDirection === 'asc' ? -1 : 1;
        if (valA > valB) return currentSortDirection === 'asc' ? 1 : -1;
        return 0;
    });
}

// 在文件开头（全局变量区）添加
// let currentSortField = null;
// let currentSortDirection = 'asc';
```

---

## Task 5: 调整 openEditModal 和 saveSchool

**Files:**
- Modify: `index.html:1222-1310`

- [ ] **Step 1: 确认 openEditModal 支持 level 参数**

现有 `openEditModal(name, type)` 已支持 type 参数，只需确认编辑弹窗中的保存逻辑能根据 level 正确保存。

- [ ] **Step 2: 调整 saveSchool 保存逻辑**

替换第1262-1300行的 saveSchool 函数：

```javascript
// 替换第1262-1300行
// 注意：原有的 citySchools/districtSchools 数组结构不变（无level字段）
// level标记仅在 allSchools 合并数组中使用
async function saveSchool() {
    const index = parseInt(document.getElementById('editIndex').value);
    const type = document.getElementById('editType').value; // 'city' or 'district'
    const name = document.getElementById('editName').value.trim();
    const plan = parseInt(document.getElementById('editPlan').value);
    const website = document.getElementById('editWebsite').value.trim();

    if (!name || isNaN(plan)) {
        alert('请填写完整信息');
        return;
    }

    const saveBtn = document.getElementById('saveBtn');
    saveBtn.disabled = true;
    saveBtn.textContent = '保存中...';

    try {
        if (type === 'city') {
            citySchools[index] = { name, plan, website };
            await saveToJsonFile(citySchools, 'city_schools.json', 'city');
        } else {
            const district = document.getElementById('editDistrict').value;
            districtSchools[index] = { district, name, plan, website };
            await saveToJsonFile(districtSchools, 'district_schools.json', 'district');
        }

        // 重新合并 allSchools 并刷新
        allSchools = [...citySchools.map(s => ({ ...s, level: 'city' })),
                      ...districtSchools.map(s => ({ ...s, level: 'district' }))];
        filterSchools();
        closeEditModal();

    } catch (error) {
        console.error('Save failed:', error);
        alert('保存失败: ' + error.message);
    } finally {
        saveBtn.disabled = false;
        saveBtn.textContent = '保存';
    }
}
```

---

## Task 6: 调整事件监听器

**Files:**
- Modify: `index.html:1690-1720`

- [ ] **Step 1: 替换事件监听器**

替换原有关键事件监听器：

```javascript
// 替换第1710-1718行
// 原有:
// document.getElementById('searchInput').addEventListener('input', filterDistrictSchools);
// document.getElementById('citySearchInput').addEventListener('input', filterCitySchools);

// 新增:
document.getElementById('experimentalSearchInput').addEventListener('input', filterSchools);
document.getElementById('levelSelect').addEventListener('change', filterSchools);
document.getElementById('experimentalDistrictSelect').addEventListener('change', filterSchools);
```

---

## Task 7: 清理废弃代码

**Files:**
- Modify: `index.html`

- [ ] **Step 1: 删除废弃的分离函数**

删除以下不再使用的函数（约在原位置）：
- `renderCitySchools` (原1107行)
- `renderDistrictSchools` (原1145行)
- `filterCitySchools` (原1173行)
- `filterDistrictSchools` (原1181行)
- `sortCitySchools` (原1053行)
- `sortDistrictSchools` (原1066行)
- `applyCitySort` (原1195行)
- `applyDistrictSort` (原1206行)

---

## Task 8: 添加样式

**Files:**
- Modify: `index.html` (style标签内)

- [ ] **Step 1: 添加级别标签样式**

在 `<style>` 标签中添加：

```css
.level-tag {
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 12px;
}
.level-tag.level-city {
    background: #e3f2fd;
    color: #1565c0;
}
.level-tag.level-district {
    background: #f3e5f5;
    color: #7b1fa2;
}
.level-select {
    padding: 6px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
}
```

---

## Task 9: 验证和测试

- [ ] **Step 1: 验证Tab切换**

点击"实验学校"Tab，验证默认不激活（首次加载时不显示）

- [ ] **Step 2: 验证数据加载**

点击Tab后，验证区级和市级数据都正确显示

- [ ] **Step 3: 验证级别列**

验证表格中"级别"列正确显示"区级"或"市级"

- [ ] **Step 4: 验证筛选功能**

测试级别下拉和区域下拉筛选是否正常工作

- [ ] **Step 5: 验证搜索功能**

测试按学校名称搜索是否正常

- [ ] **Step 6: 验证编辑保存**

测试编辑区级和市级学校后，数据是否正确保存到对应JSON文件
