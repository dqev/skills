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

## Repository Structure

This repository follows the official `skills.sh` multi-skill layout:

```text
skills/
├── skills.sh.json              # skills.sh catalog manifest and groupings
├── package.json                # Repository package metadata
├── README.md                   # Documentation and usage instructions
└── skills/                     # Skill directory
    └── draw-icons/             # Draw Icons skill
        ├── SKILL.md            # Skill instructions and metadata
        ├── assets/             # Presets and icon configurations
        │   └── presets.json
        ├── references/         # Deep design rules and specifications
        │   ├── design-rules.md
        │   ├── icon-recipes.md
        │   ├── review-checklist.md
        │   ├── setup-guide.md
        │   └── svg-specs.md
        └── scripts/            # CLI utilities and validators
            ├── icons.js
            ├── package.json
            └── lib/
```

---

## Adding More Skills

To add a new skill to this repository:
1. Create a new directory under `skills/<skill-name>/`.
2. Add a `SKILL.md` file with standard YAML frontmatter (`name`, `description`, `license`, `metadata`).
3. Add any supporting folders (`references/`, `scripts/`, `assets/`) as needed.
4. Add the skill name to `skills.sh.json` under `groupings.skills`.
