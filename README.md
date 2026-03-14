# Skills

Skills for AI coding agents — research, tooling, and personal productivity.

Covers domains across mechanistic interpretability, alignment, econometrics, complexity theory, finance, formal verification, and the tools I use daily.

## Install

```bash
npx skills add thegovind/skills
```

## Skill Catalog

| Area | Skill | Description |
|------|-------|-------------|
| **Tools** | [obsidian](.github/skills/obsidian/) | Obsidian CLI, Flavored Markdown, Bases databases, JSON Canvas |

### Planned Areas

| Area | Topics |
|------|--------|
| **Mechanistic Interpretability** | Circuit analysis, feature visualization, sparse autoencoders, activation patching |
| **Alignment** | RLHF, constitutional AI, scalable oversight, reward modeling |
| **Ablation Studies** | Component ablation, causal tracing, knockout experiments |
| **Econometrics** | Causal inference, IV estimation, panel data, diff-in-diff, synthetic control |
| **Microeconomics** | Mechanism design, auction theory, contract theory, market design |
| **Corporate Strategy** | Competitive dynamics, resource-based view, real options, disruption frameworks |
| **Complexity Theory** | Computational complexity, emergent systems, agent-based modeling |
| **Finance** | Derivatives pricing, portfolio theory, risk modeling, stochastic calculus |
| **Lean** | Value stream mapping, continuous improvement, systems thinking |
| **Formal Verification** | Model checking, theorem proving, type theory, property-based testing |

## Structure

```
.github/skills/<skill-name>/
├── SKILL.md              # Main skill (YAML frontmatter + markdown body)
└── references/           # Detailed docs, split by topic
    └── *.md

tests/scenarios/<skill-name>/
└── scenarios.yaml        # Test scenarios
```

## License

MIT
