---
name: word-proofreader
description: Use when user has a Word document that needs typo correction, text polishing, or proofreading. Triggers on requests to fix typos, correct errors, polish writing, proofread, or when user says "校对", "纠错", "润色", "改错别字", "检查错别字".
---

# Word Proofreader

纠正 Word 文档中的错别字、润色文字表达、统一排版格式，并通过 Word 修订模式（Track Changes）记录每处文字修改。

## When to Use

- 用户提供 .docx 文件需要校对
- 请求"校对"、"纠错"、"润色"、"改错别字"、"检查拼写"
- 需要统一文档排版格式
- 需要修订追踪记录

## When NOT to Use

- 需要从零创建文档 → 用 word-formatter
- 需要生成图表和配图 → 用 word-formatter
- 只需读取文档内容不做修改

## Workflow

```
1. 读取文档 → 2. 选择模式 → 3. 逐段分析 → 4. 应用修正(Track Changes) → 5. 输出
```

## Step 1: 读取文档

```bash
python ~/.claude/skills/word-proofreader/proofreader.py extract "/path/to/document.docx"
```

输出 JSON 格式的段落列表，包含段落索引、文本和样式名。

## Step 2: 用户选择处理模式

使用 AskUserQuestion 询问：

| 模式 | 说明 |
|------|------|
| **仅纠错** | 只修正错别字、拼写错误、标点符号问题 |
| **纠错+润色** | 纠错 + 优化用词、调整句式、提升可读性 |
| **全面处理** | 纠错 + 润色 + 统一排版（字体字号、行距段距、标题层级） |

## Step 3: 逐段分析

读取 extract 输出的段落列表，对每个段落进行分析。

**分析要点：**

### 纠错（所有模式都执行）
- 中文错别字（同音字、形近字替换错误）
- 英文拼写错误
- 标点符号（中英文标点混用、多余/缺失标点）
- 数字和单位格式

### 润色（纠错+润色 和 全面处理 模式）
- 冗余表达精简
- 用词不当替换
- 句式优化（过长句子拆分、被动句转主动句等）
- 逻辑连接词补充
- **注意：润色要保持原文风格和语气，不过度改写**

**生成修正 JSON，写入临时文件：**

```json
{
  "paragraphs": [
    {
      "index": 0,
      "original": "这个方案的目地是为了提高工作效率，使的我们能够更加好的完成任务。",
      "corrected": "这个方案的目的是提高工作效率，使我们能够更好地完成任务。",
      "changes": [
        {"type": "typo", "from": "目地", "to": "目的"},
        {"type": "typo", "from": "使的", "to": "使"},
        {"type": "polish", "from": "为了提高", "to": "提高"},
        {"type": "polish", "from": "更加好的", "to": "更好地"}
      ]
    }
  ]
}
```

**重要：** 只输出有修改的段落，无需修改的段落不要包含在 JSON 中。

## Step 4: 应用修正

将修正 JSON 保存为临时文件后执行：

```bash
python ~/.claude/skills/word-proofreader/proofreader.py apply \
  "/path/to/document.docx" \
  "/tmp/corrections.json" \
  --output "/path/to/document_proofread.docx" \
  --mode full
```

`--mode` 参数：
- `typo_only` - 不排版
- `typo_polish` - 不排版
- `full` - 排版

自定义排版参数（可选）：
```bash
--format /tmp/format_config.json
```

format_config.json 示例：
```json
{
  "body_font_cn": "SimSun",
  "body_font_en": "Times New Roman",
  "body_size": 12,
  "heading1_size": 22,
  "heading2_size": 16,
  "heading3_size": 14,
  "line_spacing": 1.5,
  "space_after": 6
}
```

## Step 5: 输出

脚本会输出修改统计。向用户报告：

```
完成！
- 输出文件：/path/to/document_proofread.docx
- 修改段落：X / Y
- 错别字修正：N 处
- 润色优化：M 处

请用 Word 打开文件，在「审阅」→「修订」中查看每处修改，可逐条接受或拒绝。
```

## Dependencies

```bash
pip install python-docx
```

## Error Handling

| 错误 | 处理方式 |
|------|----------|
| 文件不存在 | 提示用户检查路径 |
| 格式不支持 | 仅支持 .docx |
| python-docx 未安装 | 提示 pip install |
| 文档加密/受保护 | 提示用户先解除保护 |
