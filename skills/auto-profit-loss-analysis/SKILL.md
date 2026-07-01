---
name: auto-profit-loss-analysis
description: |
  门店损益自动分析工具。店长发送损益报表数据后，自动生成预填充HTML分析页面。
  第一部分：AI自动分析损益数据（损益总览、收入结构、成本费用结构、核心差距）
  第二部分：店长填写利润差距根源、降本增效方案、下月目标
  
  **知识库依据**：参考《认识损益报表》《如何分析损益报表》《认识毛利》等培训资料，
  按照"利润=收入-成本-费用"框架进行根源分类。
  
  触发词："损益分析"、"利润分析"、"损益复盘"、"利润复盘"、"做损益分析"、"自动分析损益"、"损益自动分析"
---

**!RECHECK: 每次触发前必须重新读取本文件**

# 门店损益自动分析工具

## 核心流程（六步法）

```
店长说"损益分析" → 我发指引 → 店长给数据 → 我生成HTML → 店长填写 → 回复"已完成" → 我同步到钉钉AI表格
```

**关键原则：**
- 每次损益分析 = 生成一份新的HTML（新URL，带时间戳）
- 同一次分析过程中修改 = URL自动更新（`#data=`参数保存最新数据）
- 跨设备访问 = 复制完整链接（含`#data=`）到任何设备都能打开
- 店长只需回复"已完成"，无需手动复制报告内容

---

## 知识库参考与根源分析框架

本技能的根源分析体系基于聚满分内部培训资料设计：

### 参考文件
- `knowledge-base/markdown/认识损益报表.md` — 损益报表结构定义
- `knowledge-base/markdown/如何分析损益报表.md` — 损益分析方法与实战案例
- `knowledge-base/markdown/认识毛利.md` — 毛利率概念与计算

### 损益报表结构（聚满分标准）
```
折前营业额
  - 折扣金额
  = 折后实收营业额（实际收入）
  - 营业成本（原料成本）
  = 毛利
    ÷ 折后营业额 = 折后毛利率
    ÷ 折前营业额 = 折前毛利率
  
  - 可控费用
    - 人工成本（工资+奖金+社保）
    - 水电费
    - 低值易耗品
    - 维修费
    - 宣传费
    - 办公费
    - 配送运费
  = 可控利润
  
  - 不可控费用
    - 租金
    - 品牌管理费
    - 折旧摊销
    - 其他固定费用
  = 净利润
    ÷ 折后营业额 = 净利率
```

### 利润差距根源分类框架

按照 **"利润 = 收入 - 成本 - 费用"** 公式，将根源分为四大维度：

**一、收入问题**
- 营业额未达标（来客数、客单价、品类结构）
- 折扣侵蚀利润（折前vs折后毛利率差距）

**二、成本问题（毛利率偏低）**
- 原料成本偏高（应产率、报废率、出品标准）
- 产品结构问题（低毛利品类占比高）

**三、费用问题（各项费用占比超标）**
- 人工费用超标（编制、排班、人效）
- 水电/低耗/维修费超标
- 其他费用异常

**四、利润问题**
- 可控利润未达标
- 净利润率低于预算
- 不可控费用占比过高

### 关键分析要点（来自知识库）

1. **毛利率是核心指标**：折前毛利率反映产品定价和结构，折后毛利率反映实际盈利能力
2. **折前vs折后差距 = 折扣侵蚀**：差距超过5个百分点必须警示
3. **人工费用是最大可控项**：通常占营业额15-20%，超标需优先排查
4. **应产率决定成本**：出品重量/尺寸偏差直接导致原料浪费
5. **报废率是隐形杀手**：订货预估不准、临期处理是主要来源
6. **费用率 = 费用 ÷ 折后营业额**：必须用折后营业额作为分母，更准确

---

## 第一步：触发技能

当店长说以下任一词时触发：
- 损益分析
- 利润分析
- 损益复盘
- 利润复盘
- 做损益分析
- 自动分析损益
- 损益自动分析

**回复指引（纯文本，不要markdown引用块）：**

📊 月度损益分析请提供以下数据：

**【基础信息】**

● 门店：

● 月份：

● 区域：（孙红梅区 / 陈桂莲区 / 陈超平区）

**【损益核心数据】**

● 折前营业额（实收 + 折扣）

● 折后实收营业额

● 折扣金额

● 营业成本（原料成本）

● 折前毛利率 / 折后毛利率

● 人工成本及占营业额比例

● 水电费、低耗品、维修费、宣传费等各项费用

● 可控利润 / 净利润

**【提交方式】**

● 截图 / 拍照 / 文字 / 文件 均可

⚠️ 注意：数据必须有预算对比（实际vs预算），否则无法分析差距。

---

## 第二步：接收数据并生成HTML

店长发送数据后，解析数据，生成预填充HTML文件。

**数据解析规则：**

| 字段 | 解析方式 | 示例 |
|------|----------|------|
| 门店名称 | 提取"门店"后内容 | 万民城店 |
| 区域 | 提取"区域"后内容 | 孙红梅区 |
| 月份 | 提取"月份"后内容 | 2026-05 |
| 折前营业额 | 提取"折前/折前营业额"后数字 | 500000 |
| 折后实收 | 提取"折后/实收"后数字 | 470148 |
| 折扣金额 | 提取"折扣"后数字 | 29852 |
| 营业成本 | 提取"成本/原料成本"后数字 | 250000 |
| 折前毛利率 | 提取"折前毛利率"后数字 | 44.0 |
| 折后毛利率 | 提取"折后毛利率"后数字 | 46.8 |
| 人工成本 | 提取"人工/薪资/工资"后数字 | 85000 |
| 人工占比 | 提取"人工占比"后数字 | 18.1 |
| 水电费 | 提取"水电"后数字 | 12000 |
| 水电占比 | 提取"水电占比"后数字 | 2.6 |
| 低耗品 | 提取"低耗/低值易耗"后数字 | 5000 |
| 维修费 | 提取"维修"后数字 | 3000 |
| 宣传费 | 提取"宣传/广告"后数字 | 4000 |
| 办公费 | 提取"办公"后数字 | 2000 |
| 配送运费 | 提取"运费/配送"后数字 | 3000 |
| 租金 | 提取"租金"后数字 | 15000 |
| 品牌管理费 | 提取"品牌/管理"后数字 | 8000 |
| 可控利润 | 提取"可控利润"后数字 | 85000 |
| 净利润 | 提取"净利润/净利率"后数字 | 60000 |
| 净利率 | 提取"净利率"后数字 | 12.8 |

**预算对比数据（必须）：**
| 字段 | 解析方式 |
|------|----------|
| 预算营业额 | 提取"预算营业额"后数字 |
| 预算毛利率 | 提取"预算毛利率"后数字 |
| 预算人工占比 | 提取"预算人工"后数字 |
| 预算净利润 | 提取"预算利润"后数字 |
| 预算净利率 | 提取"预算净利率"后数字 |

**品类收入结构（可选）：**
```
品类：现烤131342,包装85076,热酥33763,蛋糕86758,甜品86059
```

**生成HTML文件：**

基于模板文件：`/root/.openclaw/workspace/skills/auto-profit-loss-analysis/template.html`

复制模板，将 `window.__AI_DATA__ = null` 替换为解析后的JSON数据。

**文件名格式：** `{拼音}-sunyi-{年月}-{时间戳}.html`

**示例注入数据：**
```javascript
window.__AI_DATA__ = {
    "storeName": "万民城店",
    "region": "孙红梅区",
    "month": "2026-05",
    
    // 收入
    "revenueBeforeDiscount": 500000,
    "revenueAfterDiscount": 470148,
    "discountAmount": 29852,
    
    // 成本与毛利
    "costOfGoods": 250000,
    "grossProfit": 220148,
    "grossMarginBefore": 44.0,
    "grossMarginAfter": 46.8,
    
    // 可控费用
    "laborCost": 85000,
    "laborCostPct": 18.1,
    "utilities": 12000,
    "utilitiesPct": 2.6,
    "lowValueConsumables": 5000,
    "lowValueConsumablesPct": 1.1,
    "repairCost": 3000,
    "repairCostPct": 0.6,
    "advertising": 4000,
    "advertisingPct": 0.9,
    "officeExpense": 2000,
    "officeExpensePct": 0.4,
    "deliveryFee": 3000,
    "deliveryFeePct": 0.6,
    
    // 不可控费用
    "rent": 15000,
    "rentPct": 3.2,
    "brandFee": 8000,
    "brandFeePct": 1.7,
    "otherUncontrollable": 2000,
    "otherUncontrollablePct": 0.4,
    
    // 利润
    "controllableProfit": 85000,
    "controllableProfitPct": 18.1,
    "netProfit": 60000,
    "netProfitMargin": 12.8,
    
    // 预算对比
    "budgetRevenue": 500000,
    "budgetGrossMargin": 48.0,
    "budgetLaborCostPct": 16.0,
    "budgetNetProfit": 80000,
    "budgetNetProfitMargin": 16.0,
    
    // 品类结构
    "categories": [
        {"name": "现烤", "amount": 131342, "pct": 28.0},
        {"name": "包装", "amount": 85076, "pct": 18.1},
        {"name": "热酥", "amount": 33763, "pct": 7.2},
        {"name": "蛋糕", "amount": 86758, "pct": 18.5},
        {"name": "甜品", "amount": 86059, "pct": 18.3},
        {"name": "贸易", "amount": 47150, "pct": 10.0}
    ],
    
    // 上月对比（可选）
    "lastMonthRevenue": 450000,
    "lastMonthGrossMargin": 47.0,
    "lastMonthLaborCostPct": 17.5,
    "lastMonthNetProfit": 65000
};
```

**生成并推送：**
```bash
cd /root/.openclaw/workspace
TIMESTAMP=$(date +%Y%m%d%H%M)
FILENAME="{拼音}-sunyi-{年月}-${TIMESTAMP}.html"
cp /root/.openclaw/workspace/skills/auto-profit-loss-analysis/template.html "./${FILENAME}"
node -e "
const fs = require('fs');
let html = fs.readFileSync('./${FILENAME}', 'utf-8');
const data = { /* 解析后的数据 */ };
const injection = 'window.__AI_DATA__ = ' + JSON.stringify(data) + ';';
html = html.replace(/window\.__AI_DATA__\s*=\s*null;/, injection);
fs.writeFileSync('./${FILENAME}', html);
console.log('✅ 生成: ' + '${FILENAME}');
"
git add "${FILENAME}"
git commit -m "add: {门店名} {年月} 损益分析"
git push -u origin master
```

> **关键：每次生成都是新文件（带时间戳），不会覆盖旧文件**

---

## 第三步：发送链接给店长

📊 **{门店名} · {月份} 损益分析**

**第一步：数据已分析，点击链接继续完成报告。**

点击链接：👉 https://htmlpreview.github.io/?https://raw.githubusercontent.com/Vincent1984juu/wukong/master/{拼音}-sunyi-{年月}-{时间戳}.html

**提示1：**

打开链接后，点右上角**【…】**，选择浏览器打开。

**提示2：**

如遇页面显示错误或"等待数据"，请关闭后稍等30秒重新打开。

---

## 第四步：记录待同步链接

生成HTML后，将链接信息写入临时记录文件。

**记录文件：** `/root/.openclaw/workspace/memory/profit-loss-pending.json`

**写入命令：**
```bash
python3 -c "
import json, os, datetime

pending_file = '/root/.openclaw/workspace/memory/profit-loss-pending.json'
records = []
if os.path.exists(pending_file):
    with open(pending_file, 'r') as f:
        records = json.load(f)

records.append({
    'store_name': '{门店名}',
    'region': '{区域}',
    'month': '{月份}',
    'url': 'https://htmlpreview.github.io/?https://raw.githubusercontent.com/Vincent1984juu/wukong/master/{拼音}-sunyi-{年月}-{时间戳}.html',
    'created_at': datetime.datetime.now().isoformat()
})

with open(pending_file, 'w') as f:
    json.dump(records, f, ensure_ascii=False, indent=2)

print('✅ 已记录待同步链接')
"
```

---

## 第五步：店长填写并粘贴链接

店长点击链接后：
1. 查看第一部分AI分析结果（损益总览、收入结构、成本费用结构、核心差距）
2. 选择诊断类型、勾选利润差距根源、添加降本增效方案、设定目标
3. 点击**【生成完整分析报告】**
4. **报告内容可点击直接修改**
5. **同一次分析，修改后再次点击【生成完整分析报告】，URL会自动更新**
6. 复制浏览器地址栏的完整链接（含 `#data=...`），可跨设备访问
7. 修改满意后，**点【📤 同步到钉钉表格】按钮复制链接，粘贴发送给【满分学院AI助教】**

> **数据保存机制：** 点击「生成完整分析报告」后，所有填写内容自动编码保存到URL的 `#data=` 参数中。复制完整链接到任何设备打开，都能看到最新保存的内容。

---

## 第六步：自动同步到钉钉AI表格

当店长**粘贴了含 `#data=` 的完整报告链接**时，触发自动同步。

**重要：优先使用店长粘贴的链接（含 `#data=`），而非待同步记录。**

**执行流程：**
1. 检查店长消息中是否包含 `https://htmlpreview.github.io` 链接
2. 如果包含，提取该链接作为 `report_url`
3. 如果不包含，读取 `memory/profit-loss-pending.json`，取最近记录作为 `report_url`
4. 调用 `sync_profit_loss_to_dingtalk` 同步链接
5. 同步成功后，从记录中删除该条目

**执行命令：**
```bash
cd /root/.openclaw/workspace
python3 -c "
import json, os, re
from dingtalk_ai_table import sync_profit_loss_to_dingtalk

# 从消息中提取URL（如果有）
message = '{店长消息内容}'
url_match = re.search(r'https://vincent1984juu\.github\.io/[^\\s\"\\'>]+', message)
pasted_url = url_match.group(0) if url_match else None

if pasted_url:
    report_url = pasted_url
    print('✅ 使用店长粘贴的链接:', report_url)
else:
    pending_file = 'memory/profit-loss-pending.json'
    if not os.path.exists(pending_file):
        print('❌ 没有待同步的记录')
        exit(1)
    with open(pending_file, 'r') as f:
        records = json.load(f)
    if not records:
        print('❌ 没有待同步的记录')
        exit(1)
    record = records[-1]
    report_url = record['url']
    print('✅ 使用待同步记录:', report_url)

report_data = {
    'store_name': '{门店名}',
    'region': '{区域}',
    'month': '{月份}',
    'report_url': report_url
}

result = sync_profit_loss_to_dingtalk(report_data)
if result:
    if not pasted_url:
        records = [r for r in records if r['url'] != report_url]
        with open(pending_file, 'w') as f:
            json.dump(records, f, ensure_ascii=False, indent=2)
    print('✅ 已同步到钉钉AI表格')
else:
    print('❌ 同步失败')
"
```

**同步成功后会回复店长：**

✅ 报告已同步到钉钉AI表格

表格地址：https://alidocs.dingtalk.com/i/nodes/Y1OQX0akWm6nkvZquvN2ABAjVGlDd3mE?iframeQuery=sheetId%3DtPmsMAL%26viewId%3DZjIwDlt

点击表格中"报告原文"列的链接即可查看完整报告

---

## 部署方式

### 命名规则

- 中文门店名 → 拼音（小写，无空格）
- 格式：`{拼音}-sunyi-{年月}-{时间戳}.html`
- 示例：`wanmincheng-sunyi-202605-202506251030.html`
- **时间戳 = 生成时的年月日时分，确保每次生成都是全新文件，彻底避免缓存问题**

### 缓存问题

GitHub Pages 有 **1-3分钟** 缓存，推送后需要稍等刷新。建议加 `?v=随机数` 参数强制刷新：
```
https://htmlpreview.github.io/?https://raw.githubusercontent.com/Vincent1984juu/wukong/master/wanmincheng-sunyi-202605.html?v=2
```

---

## 模板文件位置

- **主模板：** `/root/.openclaw/workspace/skills/auto-profit-loss-analysis/template.html`

## HTML工具功能说明

### 第一部分：AI自动分析（只读展示）

店长无需填写，数据已预填充：

| 展示项 | 说明 |
|--------|------|
| 基础信息 | 门店、区域、月份 |
| 损益总览 | 收入、成本、毛利、费用、利润（表格，含预算对比、差异、达成率） |
| 收入结构 | 折前/折后营业额、折扣金额及占比 |
| 成本结构 | 营业成本、毛利率（折前/折后）对比 |
| 费用结构 | 人工、水电、低耗、维修、宣传等（占营业额比例，红绿标注超标项） |
| 利润结构 | 可控利润、净利润、净利率 |
| 核心差距 | 黄色提示框，AI自动判断异常指标（毛利率、人工占比、净利率等） |
| 引导行动 | 提示店长开始分析根源 |

### 第二部分：店长填写（交互式）

| 模块 | 交互方式 |
|------|----------|
| 诊断类型 | 单选（毛利率问题 / 费用超标 / 收入不足 / 综合问题） |
| 利润差距根源 | 多选框点击（按收入、成本、费用、利润四大维度分类）、可编辑文字、可新增 |
| 降本增效方案 | 动态添加/删除，每行5个字段（行动、负责人、时间、衡量标准、目标结果） |
| 下月目标 | 毛利率目标、费用控制目标、利润目标等默认输入框 + 可添加自定义目标 |

### 报告生成

点击「生成完整分析报告」后：
- 整合AI分析数据 + 店长填写内容
- 数据表格100%复用第一部分样式
- 核心差距引用第一部分AI生成的文字
- **报告内容可直接点击编辑（虚线边框区域）**
- 编辑后点击"🔄 更新报告内容"保存
- 操作按钮（复制/打印/截图/同步到钉钉表格）在报告下方
- 支持html2canvas一键截图保存
- **无需复制报告内容，回复"已完成"即可同步**

---

## 数据格式规范

### 店长输入格式（灵活解析）

支持以下格式（自动识别）：

```
// 完整格式
门店:万民城店
区域:孙红梅区
月份:2026-05
折前营业额:500000
折后实收:470148
折扣:29852
营业成本:250000
折前毛利率:44.0
折后毛利率:46.8
人工成本:85000(占比18.1%)
水电费:12000(占比2.6%)
低耗品:5000
维修费:3000
宣传费:4000
办公费:2000
运费:3000
租金:15000
品牌管理费:8000
可控利润:85000
净利润:60000
净利率:12.8
预算营业额:500000
预算毛利率:48.0
预算人工占比:16.0
预算净利润:80000
预算净利率:16.0

// 简化格式（也支持）
万民城店,孙红梅区,5月
折前50万,折后47万,折扣3万
成本25万,毛利率折后46.8%
人工85000占18.1%,水电12000
```

### 解析容错规则

- 缺失预算数据 → 尝试从记忆库或上次数据补齐，无法补齐则标记"无预算"
- 缺失品类数据 → categories数组留空
- 缺失区域 → 显示"______"
- 数字识别错误 → 尝试用正则二次提取
- 百分比和绝对值混合 → 智能判断（如"人工85000(18.1%)"同时提取金额和比例）

---

## 技能目录结构

```
skills/auto-profit-loss-analysis/
├── SKILL.md              # 本文件
├── template.html         # HTML模板
└── example/              # 示例文件（可选）
```

## 注意事项

1. **每次分析都是新文件**：不同月份、不同门店的分析生成独立HTML，不会互相覆盖
2. **同一次分析可修改**：店长填写过程中可多次修改，每次点击「生成完整分析报告」URL自动更新
3. **跨设备访问**：复制完整链接（含`#data=`）到任何设备都能打开最新内容
4. **模板更新后需重新生成**：我优化模板后，会批量重新生成所有门店文件
5. **缓存问题**：GitHub Pages有缓存，推送后可能需要 **1-3分钟** 才能看到最新内容，建议加 `?v=2` 强制刷新
6. **钉钉兼容性**：如果钉钉内置浏览器显示异常，建议用系统浏览器(Chrome/Safari)打开
7. **Token安全**：Token已配置，不要泄露给其他人
8. **无需复制内容**：店长只需回复"已完成"，AI自动同步链接到表格
9. **中文文件名不支持**：GitHub Pages 404，必须用拼音命名
10. **折前vs折后毛利率**：必须同时展示，差距超过5个百分点必须突出警示

## 与现有技能的区别

| 技能 | 方式 | 适用场景 | 核心指标 |
|------|------|----------|----------|
| **monthly-profit-loss-analysis** | 对话式教练，一步步问 | 新店店长、需要深度辅导 | 毛利率、费用率、预算达成 |
| **auto-profit-loss-analysis** | 自动解析数据，生成HTML链接 | 老店店长、数据齐全、追求效率 | 同上，但可视化+交互式 |
| **auto-revenue-analysis** | 自动解析数据，生成HTML链接 | 营业额分析 | 营业额、客单价、来客数、品类 |
