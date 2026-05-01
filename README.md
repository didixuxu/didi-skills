# didi-skills

Claude Code Skills for competitive analysis, market sizing, and academic paper reading.

## Skills

### competitive-landscape

Generate Apple-style minimalist competitive analysis reports in HTML with Chart.js visualizations. Uses Porter's Five Forces, Blue Ocean Strategy, and competitive positioning maps.

**Usage:** Ask Claude to analyze competitors in any industry. It will search for real market data and output a clean HTML report.

### market-sizing-analysis

Calculate TAM/SAM/SOM using three methodologies: top-down (industry reports), bottom-up (customer segments), and value theory (willingness to pay). Includes bilingual docs (EN/CN), example analyses, and data source references.

**Usage:** Ask Claude to size a market opportunity. It walks through the full framework and produces investor-ready analysis.

### paper-proofread

Paper Onion - a four-layer progressive reading skill for academic papers. Peels through: 30-second scan, skeleton analysis, deep mechanism dissection, and knowledge connection. Outputs a hand-drawn style React notecard artifact.

**Usage:** Give Claude a paper PDF or URL. It produces a beautiful visual notecard summarizing the paper's core insights.

### word-formatter

Transform messy Word documents or plain text into professionally formatted .docx/PDF files. Detects content type (meeting notes / report / study notes / proposal), applies matching style (business formal / modern minimal / academic), and can auto-generate cover images and data charts.

**Usage:** Drop a .docx or text file at Claude. It analyzes content type, asks how aggressively you want it restructured, and outputs a polished Word document.

### word-proofreader

Proofread .docx files with Word Track Changes — fix typos, polish wording, optionally unify formatting (font / size / line spacing / heading hierarchy). Every edit is recorded as a tracked change so you can review/accept/reject in Word.

**Usage:** Give Claude a .docx and say "proofread this" or "校对". Choose typo-only / typo+polish / full mode, then open the output file and review changes in Word's Review tab.

## Install

```bash
# Clone to your Claude Code skills directory
git clone https://github.com/didixuxu/didi-skills.git

# Symlink individual skills to ~/.claude/skills/
ln -s "$(pwd)/didi-skills/competitive-landscape" ~/.claude/skills/competitive-landscape
ln -s "$(pwd)/didi-skills/market-sizing-analysis" ~/.claude/skills/market-sizing-analysis
ln -s "$(pwd)/didi-skills/paper-proofread" ~/.claude/skills/paper-proofread
ln -s "$(pwd)/didi-skills/word-formatter" ~/.claude/skills/word-formatter
ln -s "$(pwd)/didi-skills/word-proofreader" ~/.claude/skills/word-proofreader
```

Or copy the skill folders directly into `~/.claude/skills/` or your project's `.claude/skills/` directory.

## Adding a New Skill

```bash
# 1. Copy the skill folder into this repo
cp -r ~/.claude/skills/skill-name "/Users/dixu/Desktop/claude code/didi-skills/"

# 2. Commit and push
cd "/Users/dixu/Desktop/claude code/didi-skills"
git add skill-name/
git commit -m "Add skill-name skill"
git push
```

Or just tell Claude: "把 xxx skill 上传到 GitHub"

## License

MIT
