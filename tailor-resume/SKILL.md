---
name: tailor-resume
description: Use when user provides a job description (JD) and wants to customize their resume for a specific job application, or when user says "tailor resume", "定制简历", "修改简历", "投简历"
---

# Tailor Resume

根据 JD 定制简历，生成 ATS 友好的 PDF。

## Configuration

```
MASTER_RESUME: ~/Documents/resume.pdf
OUTPUT_DIR: ~/Documents/
TEMPLATE: ~/.claude/skills/tailor-resume/resume-template.tex
```

## Workflow

1. **Read master resume** - 读取 `~/Documents/resume.pdf` 获取用户完整经历
2. **Analyze JD** - 提取关键要求：
   - 必须技能 vs 加分技能
   - 年限要求
   - 行业关键词
   - 职责匹配点
3. **Tailor content**:
   - 用 JD 原词重写匹配的经历（保持真实）
   - 将最相关的经历/bullet 提前
   - 重写 Summary 针对该职位
   - 添加量化指标
4. **Generate LaTeX** - 使用模板生成 .tex 文件
5. **Compile PDF** - `pdflatex` 编译输出

## Output

```
~/Documents/resume_[公司名]_[职位].pdf
```

## Tailoring Rules

| JD 关键词 | 简历处理 |
|----------|---------|
| Required skill X | 确保 Skills 区显眼位置有 X |
| "3+ years Y" | 在经历中明确写 Y 的年限 |
| "Cross-functional" | 加入跨团队协作案例 |
| "Data-driven" | 强调量化成果 |

## Commands

```bash
# 编译 LaTeX (安装路径生效后)
eval "$(/usr/libexec/path_helper)"
pdflatex -output-directory=~/Documents resume.tex
```

## Mistakes to Avoid

- 堆砌关键词（自然融入上下文）
- 改变真实职位头衔（改描述不改头衔）
- 写通用 Summary（每份简历单独写）
