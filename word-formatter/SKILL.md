---
name: word-formatter
description: Use when user has a messy Word document or plain text that needs professional formatting, auto-layout, and intelligent image generation. Triggers on requests to format, beautify, or reorganize documents.
---

# Word Formatter

Transform messy documents into professionally formatted Word/PDF files with intelligent layout and auto-generated images.

## When to Use

- User provides a Word document (.docx) or text file needing cleanup
- Request to "format", "beautify", "reorganize" a document
- Need to add visuals/charts to plain text content
- Converting notes/drafts into professional documents

## Workflow

```
1. Analyze Document → 2. User Chooses Level → 3. Format → 4. Generate Images → 5. Output
```

## Step 1: Analyze Document

Read the input file and detect:

```python
# Content type detection keywords
CONTENT_TYPES = {
    "meeting_notes": ["会议", "议程", "参会", "决议", "行动项", "meeting", "agenda"],
    "report": ["报告", "总结", "分析", "数据", "report", "analysis", "summary"],
    "study_notes": ["笔记", "学习", "知识点", "概念", "notes", "learning"],
    "proposal": ["方案", "计划", "目标", "预算", "proposal", "plan", "budget"]
}

# Style mapping
STYLE_MAP = {
    "meeting_notes": "business_formal",
    "report": "business_formal",
    "study_notes": "modern_minimal",
    "proposal": "academic"
}
```

**Present analysis to user:**
```
检测结果:
- 文档类型: [类型]
- 建议风格: [风格]
- 字数: [X] 字
- 检测到数据: [是/否]
```

## Step 2: User Chooses Processing Level

Ask user to select:

| 级别 | 说明 |
|------|------|
| **仅排版** | 保持原文内容，只调整格式样式 |
| **轻度重组** | 添加小标题、调整段落、补充过渡 |
| **深度重构** | 可重写内容、优化表达、补充逻辑 |

## Step 3: Apply Formatting

### Style Definitions

**Business Formal (商务正式):**
```python
BUSINESS_STYLE = {
    "title_font": "Microsoft YaHei",
    "title_size": 22,
    "heading1_size": 16,
    "heading2_size": 14,
    "body_size": 11,
    "line_spacing": 1.5,
    "colors": {
        "primary": "#1a365d",    # Deep blue
        "accent": "#2b6cb0",     # Medium blue
        "text": "#2d3748"        # Dark gray
    }
}
```

**Modern Minimal (简约现代):**
```python
MODERN_STYLE = {
    "title_font": "PingFang SC",
    "title_size": 24,
    "heading1_size": 18,
    "heading2_size": 14,
    "body_size": 11,
    "line_spacing": 1.8,
    "colors": {
        "primary": "#1a202c",    # Near black
        "accent": "#4a5568",     # Gray
        "text": "#2d3748"
    }
}
```

**Academic (学术风格):**
```python
ACADEMIC_STYLE = {
    "title_font": "SimSun",
    "title_size": 18,
    "heading1_size": 15,
    "heading2_size": 13,
    "body_size": 12,
    "line_spacing": 1.5,
    "colors": {
        "primary": "#1a202c",
        "accent": "#4a5568",
        "text": "#000000"
    }
}
```

### Document Structure

```python
def structure_document(content, level):
    """
    Structure document based on processing level
    """
    if level == "format_only":
        # Keep original structure, apply styles
        return apply_styles(content)

    elif level == "light_restructure":
        # Add headings, reorder paragraphs, add transitions
        sections = detect_sections(content)
        sections = add_subheadings(sections)
        sections = add_transitions(sections)
        return apply_styles(sections)

    elif level == "deep_restructure":
        # Rewrite unclear parts, add logic, improve flow
        sections = detect_sections(content)
        sections = rewrite_unclear(sections)
        sections = add_missing_logic(sections)
        sections = optimize_expression(sections)
        return apply_styles(sections)
```

## Step 4: Generate Images

### Image Decision Logic

```python
def decide_images(content):
    """
    Decide what images to generate based on content
    """
    images = []

    # Check for data → Charts
    if contains_data(content):
        data_sections = extract_data(content)
        for section in data_sections:
            chart_type = suggest_chart_type(section)
            images.append({
                "type": "chart",
                "chart_type": chart_type,
                "data": section
            })

    # Check for concepts → AI illustrations
    concepts = extract_key_concepts(content)
    if concepts:
        images.append({
            "type": "illustration",
            "concept": concepts[0],
            "style": get_document_style()
        })

    # Add cover image
    images.append({
        "type": "cover",
        "title": get_document_title(content),
        "style": get_document_style()
    })

    return images
```

### Chart Generation

```python
import matplotlib.pyplot as plt
import matplotlib
matplotlib.rcParams['font.sans-serif'] = ['PingFang SC', 'Microsoft YaHei']

def generate_chart(data, chart_type, style):
    """
    Generate chart matching document style
    """
    colors = style["colors"]

    fig, ax = plt.subplots(figsize=(10, 6))

    if chart_type == "bar":
        ax.bar(data["labels"], data["values"], color=colors["primary"])
    elif chart_type == "pie":
        ax.pie(data["values"], labels=data["labels"],
               colors=[colors["primary"], colors["accent"], "#e2e8f0"])
    elif chart_type == "line":
        ax.plot(data["x"], data["y"], color=colors["primary"], linewidth=2)

    ax.set_title(data["title"], fontsize=14, color=colors["text"])

    return fig
```

### AI Illustration Generation

Use the `generate-image` skill:

```
Prompt template for business style:
"Professional business illustration of [concept],
flat design, corporate blue color scheme,
minimalist, clean background, vector style"

Prompt template for modern style:
"Modern minimalist illustration of [concept],
geometric shapes, black and white with accent color,
clean lines, abstract, professional"

Prompt template for academic style:
"Educational diagram illustrating [concept],
clean and simple, textbook style,
labeled components, neutral colors"
```

### Image Confirmation

Present image plan to user:
```
配图方案:
1. 封面图: [主题描述]
2. 数据图表: [图表类型] - [数据来源段落]
3. 概念插图: [概念名称]

请选择:
A) 全部生成
B) 部分修改 (请指定)
C) 跳过配图
```

## Step 5: Output Files

### Generate Word Document

```python
from docx import Document
from docx.shared import Pt, Inches, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH

def create_word_document(content, images, style):
    doc = Document()

    # Set page margins
    for section in doc.sections:
        section.top_margin = Inches(1)
        section.bottom_margin = Inches(1)
        section.left_margin = Inches(1.25)
        section.right_margin = Inches(1.25)

    # Add cover image if exists
    if images.get("cover"):
        doc.add_picture(images["cover"], width=Inches(6))
        doc.add_page_break()

    # Add content with styles
    for element in content:
        if element["type"] == "title":
            p = doc.add_heading(element["text"], level=0)
            p.alignment = WD_ALIGN_PARAGRAPH.CENTER
        elif element["type"] == "heading1":
            doc.add_heading(element["text"], level=1)
        elif element["type"] == "heading2":
            doc.add_heading(element["text"], level=2)
        elif element["type"] == "paragraph":
            p = doc.add_paragraph(element["text"])
            p.paragraph_format.line_spacing = style["line_spacing"]
        elif element["type"] == "image":
            doc.add_picture(element["path"], width=Inches(5))

    return doc
```

### Convert to PDF

```bash
# Using LibreOffice (cross-platform)
libreoffice --headless --convert-to pdf --outdir [output_dir] [docx_file]

# Or using docx2pdf (requires MS Word on Windows/Mac)
# pip install docx2pdf
```

```python
from docx2pdf import convert

def export_pdf(docx_path):
    pdf_path = docx_path.replace('.docx', '.pdf')
    convert(docx_path, pdf_path)
    return pdf_path
```

### Output Naming

```python
def get_output_paths(input_path):
    base = os.path.splitext(input_path)[0]
    return {
        "docx": f"{base}_formatted.docx",
        "pdf": f"{base}_formatted.pdf"
    }
```

## Complete Interaction Flow

```
User: /word-formatter /path/to/messy_document.docx

Claude: 正在分析文档...

检测结果:
- 文档类型: 工作报告
- 建议风格: 商务正式
- 字数: 2,350 字
- 检测到数据: 是 (3处数据可视化)

请选择处理程度:
A) 仅排版 - 保持原文，调整格式
B) 轻度重组 - 添加小标题，优化结构
C) 深度重构 - 可重写内容，优化表达

User: B

Claude: 好的，将进行轻度重组。正在处理...

配图方案:
1. 封面图: 商务风格的工作报告封面
2. 柱状图: 季度销售数据对比
3. 饼图: 市场份额分布
4. 概念插图: 增长策略示意图

请选择:
A) 全部生成
B) 部分修改
C) 跳过配图

User: A

Claude: 正在生成配图...
[生成图片中...]

完成！输出文件:
- /path/to/messy_document_formatted.docx
- /path/to/messy_document_formatted.pdf

需要调整吗？
```

## Dependencies

```bash
# Required Python packages
pip install python-docx matplotlib docx2pdf pillow

# For PDF conversion (alternative to docx2pdf)
# macOS: brew install --cask libreoffice
# Ubuntu: sudo apt install libreoffice
```

## Error Handling

| 错误 | 处理方式 |
|------|----------|
| 文件不存在 | 提示用户检查路径 |
| 格式不支持 | 仅支持 .docx, .txt, .md |
| 图片生成失败 | 跳过该图片，继续处理 |
| PDF转换失败 | 仅输出Word，提示安装依赖 |

## Tips

- 输入文档越结构化，排版效果越好
- 深度重构模式下，Claude会尝试改善文档逻辑
- 配图会自动匹配文档风格的配色方案
- 可以多次调用进行迭代优化