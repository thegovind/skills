# Skills

Teaching AI agents the things I care about.

```
npx skills add thegovind/skills
```

## Installed

| Skill | What it does |
|-------|-------------|
| [obsidian](.github/skills/obsidian/) | CLI, Flavored Markdown, Bases databases, JSON Canvas |

## Roadmap

```mermaid
graph LR
    A[Skills] --> B[Tools]
    A --> C[Research]
    A --> D[Math]

    B --> B1[Obsidian]
    B --> B2[PKM workflows]

    C --> C1[Interpretability]
    C --> C2[Alignment]
    C --> C3[Econometrics]
    C --> C4[Microeconomics]
    C --> C5[Corporate Strategy]
    C --> C6[Finance]

    D --> D1[Lean 4]
    D --> D2[Formal Verification]
    D --> D3[Optimization]
    D --> D4[Complexity Theory]
    D --> D5[Operations]

    style B1 fill:#2d333b,stroke:#58a6ff,color:#e6edf3
```

<details>
<summary>Skill structure</summary>

```
.github/skills/<name>/
├── SKILL.md              # YAML frontmatter + markdown body
└── references/           # Detailed docs, split by topic

tests/scenarios/<name>/
└── scenarios.yaml
```
</details>

MIT
