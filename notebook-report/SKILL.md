---
name: notebook-report
description: Use when user asks for a hand-drawn style report, notebook-style report, or says "手绘板报告", "手绘风", "笔记本风格". Generates warm paper-textured HTML reports with handwriting fonts, sketchy borders, and notebook aesthetics. Also exports PNG via Playwright.
---

# Notebook Report — 手绘板报告风格

## Overview

纸质笔记本风格的 HTML 数据报告。温暖纸张底色 + 手写字体 + 素描边框 + 笔记本装订孔 + 红色页边线。适合竞品分析、数据对比、策略总结等场景。

## Design Tokens

### Colors

```css
:root {
  --paper: #fdf6e3;        /* 纸张底色 */
  --ink: #2c2c2c;          /* 主文字 */
  --ink-light: #666;       /* 辅助文字 */
  --blue: #3b6ea5;         /* 品牌A / 数据色1 */
  --green: #4a8c6f;        /* 品牌B / 数据色2 */
  --red: #c0392b;          /* 强调 / 警告 / 手写批注 */
  --orange: #e67e22;       /* 洞察高亮边框 */
  --highlight: #fff3b0;    /* 荧光笔高亮背景 */
  --grid: #e8dcc8;         /* 分隔线 / 外层背景 */
}
```

- 外层 body 背景：`#e8dcc8`（牛皮纸色）
- 页面卡片背景：`var(--paper)` (#fdf6e3)
- 双品牌对比时：blue = 品牌A，green = 品牌B
- 手写批注一律用 `var(--red)`

### Typography

```css
@import url('https://fonts.googleapis.com/css2?family=Caveat:wght@400;600;700&family=Noto+Sans+SC:wght@300;400;500;700&display=swap');
```

| 用途 | 字体 | 字号 | 字重 |
|------|------|------|------|
| 大标题 h1 | Caveat | 38px | 700 |
| 章节标题 h2 | Caveat | 28px | 600 |
| 数据大字 | Caveat | 42px | 700 |
| 手写批注 | Caveat | 16-18px | 400 |
| 正文 | Noto Sans SC | 13-14px | 400 |
| 辅助/标签 | Noto Sans SC | 11-12px | 500 |

### Notebook Texture

```css
.page {
  max-width: 960px;
  margin: 0 auto;
  background: var(--paper);
  padding: 60px 56px;
  border-radius: 4px;
  box-shadow: 2px 3px 8px rgba(0,0,0,0.12), inset 0 0 60px rgba(0,0,0,0.03);
  position: relative;
  /* 装订孔 — 5个圆点沿左边 */
  background-image:
    radial-gradient(circle at 32px 80px, #ccc 6px, transparent 6px),
    radial-gradient(circle at 32px 280px, #ccc 6px, transparent 6px),
    radial-gradient(circle at 32px 480px, #ccc 6px, transparent 6px),
    radial-gradient(circle at 32px 680px, #ccc 6px, transparent 6px),
    radial-gradient(circle at 32px 880px, #ccc 6px, transparent 6px);
  background-repeat: no-repeat;
}

/* 红色页边线 */
.page::before {
  content: '';
  position: absolute;
  left: 72px; top: 0; bottom: 0;
  width: 2px;
  background: rgba(220, 80, 80, 0.3);
}

.content { margin-left: 24px; }
```

### Sketchy Effects

所有视觉元素都带轻微旋转和手绘感：

```css
/* 素描阴影卡片 */
.card {
  border: 2px solid var(--ink);
  border-radius: 6px;
  background: white;
  box-shadow: 3px 3px 0 var(--ink);
  transform: rotate(-0.3deg);  /* 每张卡片不同角度 */
}

/* 手写批注 */
.annotation {
  font-family: 'Caveat', cursive;
  color: var(--red);
  font-size: 18px;
  transform: rotate(-2deg);
  display: inline-block;
}

/* 赢家标签 */
.winner {
  display: inline-block;
  background: var(--red);
  color: white;
  font-family: 'Caveat', cursive;
  font-size: 14px;
  padding: 1px 8px;
  border-radius: 8px;
  transform: rotate(-3deg);
}

/* 日期/标签贴纸 */
.date-tag {
  display: inline-block;
  background: var(--highlight);
  padding: 2px 10px;
  border-radius: 2px;
  font-size: 13px;
  transform: rotate(1deg);
}

/* 箭头批注 */
.arrow-note {
  font-family: 'Caveat', cursive;
  color: var(--red);
  font-size: 16px;
}
.arrow-note::before { content: '\2191 '; }  /* ↑ */
```

## Component Library

### 1. Summary Cards (总量对比)

两张并排卡片，带角标 badge：

```html
<div class="cards"> <!-- display:flex; gap:20px -->
  <div class="card">
    <div class="card-badge">Brand A</div>
    <div class="card-label">指标名称</div>
    <div class="card-value blue">2,466 件</div>
    <div class="card-sub">补充说明</div>
    <span class="annotation">批注!</span>
  </div>
  <div class="card">
    <div class="card-badge green-bg">Brand B</div>
    <div class="card-value green">1,388 件</div>
  </div>
</div>
```

### 2. Split Bar (对比拉条)

```html
<div class="metric-row"> <!-- flex, white bg, rounded -->
  <div class="metric-name">销量</div>
  <div class="metric-val blue">2,466</div>
  <div class="split-bar"> <!-- flex, rounded, overflow hidden -->
    <div class="left" style="width: 64%"></div>  <!-- blue -->
    <div class="right" style="width: 36%"></div>  <!-- green -->
  </div>
  <div class="metric-val green">1,388</div>
  <span class="winner">A 赢</span>
</div>
```

### 3. Horizontal Bar Chart (横向柱状图)

```html
<div class="chart-section"> <!-- white bg, light border -->
  <div class="chart-legend">
    <div class="legend-item"><div class="legend-dot blue"></div> Brand A</div>
    <div class="legend-item"><div class="legend-dot green"></div> Brand B</div>
  </div>
  <div class="bar-row">
    <div class="bar-label">产品名</div> <!-- 120px, right-aligned -->
    <div class="bar-track"> <!-- flex:1 -->
      <div class="bar blue" style="width: 92%">
        <span class="bar-val">892</span> <!-- absolute, right outside -->
      </div>
    </div>
  </div>
</div>
```

Bar width 按最大值的百分比计算。bar-val 绝对定位在柱子右侧外。

### 4. Detail Table (明细表)

```html
<div class="detail-table"> <!-- full width, sketchy border -->
  <div class="table-header blue-bg">Brand A · N品</div>
  <table>
    <thead><tr><th>产品</th><th>售价</th><th>销量</th><th>销售额</th></tr></thead>
    <tbody>
      <tr><td>产品名</td><td><span class="price-tag">$147</span></td><td><strong>892</strong></td><td>$11.38万</td></tr>
      <tr class="total"><td>合计</td><td>—</td><td>2,466</td><td>$38.35万</td></tr>
    </tbody>
  </table>
</div>
```

### 5. Insight Box (洞察框)

```html
<!-- 黄色高亮版 — 核心发现 -->
<div class="insight-box"> <!-- highlight bg, orange left border -->
  <h3>核心看点</h3>
  <p><strong>红色强调</strong>，普通文字，<em>白底高亮</em></p>
</div>

<!-- 白色版 — 策略建议 -->
<div class="insight-box" style="background: white; border-left-color: var(--blue);">
  <p><strong>1. 标题</strong><br>解释内容</p>
</div>
```

### 6. Section Headers (章节标题)

```html
<h2>默认蓝色边</h2>
<h2 class="green">绿色边</h2>
<h2 class="red">红色边</h2>
<h2 class="orange">橙色边</h2>
```

左边框 4px solid，Caveat 字体，轻微旋转 -0.3deg。

## Report Structure

标准报告结构：

```
1. 标题 + 副标题 + 日期标签
2. 总量对比 — Summary Cards + Split Bars
3. 分项对比 — Bar Charts (销量 + 销售额)
4. 产品明细 — Detail Tables (含价格/日期/佣金)
5. 核心看点 — Insight Box (黄色高亮)
6. 策略启示 — Insight Box (白色)
7. 页脚
```

## PNG Export

使用 Playwright 截取 `.page` 元素，2x 分辨率：

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page(viewport={'width': 1100, 'height': 900}, device_scale_factor=2)
    page.goto(f'file://{html_path}')
    page.wait_for_timeout(3000)  # 等待字体加载
    element = page.query_selector('.page')
    element.screenshot(path=png_path)
    browser.close()
```

## Key Principles

1. **每个元素都要歪** — 卡片 rotate(-0.3deg)，批注 rotate(-2deg)，标签 rotate(1deg)，角度各不同
2. **手写字只用 Caveat** — 标题、数据大字、批注、标签全用 Caveat，正文用 Noto Sans SC
3. **素描阴影 = 2px border + 3px 3px 0 solid shadow** — 不用 blur，保持手绘感
4. **批注用红色** — 所有 annotation / arrow-note / winner 都用 --red
5. **高亮用荧光黄** — --highlight (#fff3b0) 用于贴纸、insight-box、price-tag
6. **虚线分隔** — 用 dashed 而非 solid（更手工感）
7. **HTML 必须自包含** — 内联 CSS + Google Fonts CDN，不依赖外部文件
