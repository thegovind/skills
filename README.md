# Skills

Personal productivity skills for AI coding agents — Obsidian, PKM, and automation workflows.

## What Are Skills?

Skills are domain-specific knowledge packages that teach AI coding agents how to work with specific tools and workflows. Each skill has a `SKILL.md` with YAML frontmatter (name + description) and a markdown body with instructions, patterns, and examples.

## Install

```bash
npx skills add thegovind/skills
```

## Skill Catalog

> Skills live in `.github/skills/` — each directory contains a `SKILL.md` and optional `references/` folder.

| Skill | Description |
|-------|-------------|
| [obsidian](.github/skills/obsidian/) | Interact with Obsidian vaults — CLI operations, Obsidian Flavored Markdown, Bases databases, and JSON Canvas files. |

## Structure

```
.github/
└── skills/
    └── <skill-name>/
        ├── SKILL.md              # Main skill file (YAML frontmatter + markdown)
        └── references/           # Optional detailed docs
            ├── guide.md
            └── acceptance-criteria.md

tests/
└── scenarios/
    └── <skill-name>/
        └── scenarios.yaml        # Test scenarios
```

## Adding a New Skill

1. Create `.github/skills/<skill-name>/SKILL.md` with frontmatter:
   ```yaml
   ---
   name: skill-name
   description: >
     What the skill does and when to use it.
     Triggers: "keyword1", "keyword2".
   ---
   ```
2. Add instructions in the markdown body
3. Split large content into `references/` files
4. Add test scenarios in `tests/scenarios/<skill-name>/scenarios.yaml`
5. Update this README's skill catalog

## License

MIT
