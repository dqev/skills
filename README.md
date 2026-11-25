# Skills by Dev Chauhan (@dqev)

Agent skills repository for [skills.sh](https://skills.sh).

## Available Skills

### 1. draw-icons
Design, generate, and maintain production-ready SVG icon sets matching product DNA.

Install command:
```bash
npx skills add dqev/skills --skill draw-icons
```

Or install all skills from this repository:
```bash
npx skills add dqev/skills
```

---

## Adding More Skills

To add a new skill to this repository:
1. Create a new directory under `skills/<skill-name>/`.
2. Add a `SKILL.md` file with standard YAML frontmatter (`name`, `description`, `license`, `metadata`).
3. Add any supporting folders (`references/`, `scripts/`, `assets/`) as needed.
4. Add the skill name to `skills.sh.json` under `groupings.skills`.
