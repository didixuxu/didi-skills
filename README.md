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

## Install

```bash
# Clone to your Claude Code skills directory
git clone https://github.com/didixuxu/didi-skills.git

# Symlink individual skills to ~/.claude/skills/
ln -s "$(pwd)/didi-skills/competitive-landscape" ~/.claude/skills/competitive-landscape
ln -s "$(pwd)/didi-skills/market-sizing-analysis" ~/.claude/skills/market-sizing-analysis
ln -s "$(pwd)/didi-skills/paper-proofread" ~/.claude/skills/paper-proofread
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
