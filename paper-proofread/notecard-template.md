# 笔记卡 React Artifact 模板参考

本文件定义笔记卡的视觉实现规范。生成 .jsx artifact 时遵循此模板。

## 外部字体

在 artifact 的 `<style>` 标签中引入：

```css
@import url('https://fonts.googleapis.com/css2?family=Caveat:wght@400;700&family=Noto+Serif+SC:wght@400;700&family=JetBrains+Mono:wght@400&display=swap');
```

## 色彩系统

```
背景色:     #faf6ee (页面) / #fdf8ef (卡片)
纸张质感:   #f5f0e6 (浅) / #f0ebe0 (深)
主文字:     #4a3d2e
次要文字:   #6b5d4d / #8b7355
辅助文字:   #b8a88a
边框:       #e8dcc8 / #d4c9b4

强调色（用于故事线标签和知识连接）:
  红棕:     #c45d3e  (痛点、警告)
  松绿:     #6b8f71  (灵感、正面)
  靛蓝:     #4a7fb5  (方案、公式)
  薰紫:     #9b7cb8  (证据、连接)
  赭黄:     #d4a03c  (额外连接)
```

## 字体使用

| 用途 | 字体 | 大小 |
|------|------|------|
| 手写标签、标题 | Caveat, cursive | 14-26px, weight 700 |
| 正文、分析 | Noto Serif SC, serif | 12-14px |
| 公式、代码 | JetBrains Mono, monospace | 10-12px |

## 卡片结构

从上到下的信息架构：

```
┌──────────────────────────────────────┐
│  ✦                            ✦      │  ← 角落装饰
│                                      │
│         论文标题 (Caveat 26px)        │
│     作者 · 会议 · 年份 (12px)         │
│         ────────────                  │  ← 分割线
│                                      │
│    ┌─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┐       │
│    │   NAPKIN FORMULA         │       │  ← 虚线框，微倾斜
│    │   餐巾纸公式 (Caveat 24px)│       │
│    └─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┘       │
│                                      │
│  ✎ 故事线          │  ⚙ 核心机制     │  ← 两列布局
│  痛点 → 灵感       │  机制1 + 公式   │
│    ↓                │  机制2 + 公式   │
│  方案 → 证据       │  机制3 + 公式   │
│                     │                 │
│  ┌────────────────────────────┐      │
│  │ 💡 核心洞察                 │      │  ← 红棕左边框
│  │ 2-3句核心insight            │      │
│  └────────────────────────────┘      │
│                                      │
│  🔗 知识连接                          │
│  ● ResNet (2015) — 跳跃连接…         │  ← 每个不同颜色圆点
│  ● BERT (2019) — 双向预训练…          │
│  ● GPT系列 — 自回归…                  │
│                                      │
│  ┌─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┐      │
│  │ ⚠ 注意事项（隐形假设）      │      │  ← 浅红背景
│  │ ? 假设1                     │      │
│  │ ? 假设2                     │      │
│  └─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┘      │
│                                      │
│     "读完后我的认知变化…"              │  ← 斜体居中
│                                      │
│  ✦                            ✦      │
└──────────────────────────────────────┘
```

## 关键样式细节

### 手绘感

- 卡片整体微倾斜: `transform: rotate(-0.3deg)`
- 餐巾纸公式框微倾斜: `transform: rotate(0.5deg)`（方向相反，制造随意感）
- 虚线边框用 `border: 1px dashed #d4c9b4` 而非实线
- 角落用 ✦ 字符装饰

### 卡片容器样式

```css
background: #fdf8ef;
border: 2px solid #d4c9b4;
border-radius: 12px;
padding: 32px 36px;
box-shadow: 4px 6px 20px rgba(74,61,46,0.1), 
            inset 0 0 80px rgba(232,220,200,0.3);
max-width: 680px;
margin: 0 auto;
```

### 餐巾纸公式框

```css
text-align: center;
padding: 16px 20px;
background: #f5f0e6;
border-radius: 8px;
border: 1px dashed #d4c9b4;
max-width: 400px;
margin: 0 auto 24px;
transform: rotate(0.5deg);
```

公式文字: `font-family: 'Caveat', cursive; font-size: 24px; color: #c45d3e; font-weight: 700`

### 故事线箭头

故事线的四步之间用简单文字箭头连接：
```html
<div style={{ color: '#d4c9b4', fontSize: 10, marginLeft: 8 }}>↓</div>
```

四步标签分别用四种强调色：痛点(#c45d3e)、灵感(#6b8f71)、方案(#4a7fb5)、证据(#9b7cb8)

### 核心机制

每个机制用左边框分隔：
```css
padding-left: 8px;
border-left: 2px solid #e8dcc8;
```

公式用 JetBrains Mono，颜色 #4a7fb5

### 知识连接

每个连接前面一个彩色圆点，循环使用五种强调色：
```css
width: 8px; height: 8px; border-radius: 50%;
background: /* 循环使用 #c45d3e, #4a7fb5, #6b8f71, #9b7cb8, #d4a03c */
```

### 隐形假设区块

```css
background: #fef5f0;
border-radius: 6px;
border: 1px dashed #e8c8b8;
```

每条假设前面用 `?` 符号，颜色 #8b6355

### 页面背景

```css
background: linear-gradient(170deg, #faf6ee 0%, #f0ebe0 50%, #e8e0d0 100%);
font-family: 'Noto Serif SC', 'Georgia', serif;
color: #4a3d2e;
```

## 完整 React 组件结构

```jsx
export default function PaperNoteCard() {
  // 数据直接内嵌在组件中（由 SKILL.md 分析流程填充）
  const paper = { title, authors, year, venue };
  const layer1 = { oneSentence, keyQuestion, firstImpression };
  const layer2 = { storyline, keyFigure };
  const layer3 = { mechanisms, hiddenAssumptions };
  const layer4 = { napkinFormula, coreInsight, connections, whatChanged };

  return (
    <div style={pageBackground}>
      {/* 标题区 */}
      <div style={headerStyle}>
        <h1>{paper.title}</h1>
        <p>{paper.authors} · {paper.venue} {paper.year}</p>
      </div>

      {/* 笔记卡 */}
      <div style={cardStyle}>
        {/* 餐巾纸公式 - 视觉焦点 */}
        <NapkinFormula formula={layer4.napkinFormula} />

        {/* 两列：故事线 + 核心机制 */}
        <div style={twoColumnGrid}>
          <Storyline steps={layer2.storyline} />
          <Mechanisms items={layer3.mechanisms} />
        </div>

        {/* 核心洞察 */}
        <InsightBlock text={layer4.coreInsight} />

        {/* 知识连接 */}
        <Connections items={layer4.connections} />

        {/* 隐形假设 */}
        <Assumptions items={layer3.hiddenAssumptions} />

        {/* 认知变化 */}
        <PersonalChange text={layer4.whatChanged} />
      </div>
    </div>
  );
}
```

注意：不要把上面的结构当成需要拆分的组件。实际输出时，写成一个**单文件** React 组件即可，所有内容内联在一个 `export default function` 中。
