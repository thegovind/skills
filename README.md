# Skills

Teaching AI agents the things I care about.

```
npx skills add thegovind/skills
```

---

## Installed

| Skill | What it does |
|-------|-------------|
| [obsidian](.github/skills/obsidian/) | CLI, Flavored Markdown, Bases databases, JSON Canvas |

## Roadmap

I work at the intersection of a lot of fields. Each skill distills a domain into something an agent can actually use.

**Interpretability & Alignment** — circuit analysis, sparse autoencoders, activation patching, causal tracing, RLHF, scalable oversight, ablation studies

**Economics** — causal inference, IV estimation, panel data, diff-in-diff, mechanism design, auction theory, contract theory, market design

**Strategy & Operations** — competitive dynamics, real options, value streams, scheduling, supply chain, constraint satisfaction

**Math & Verification** — Lean 4, tactic proofs, mathlib, model checking, type theory, property-based testing

**Complexity & Finance** — computational complexity, emergent systems, agent-based modeling, derivatives pricing, portfolio theory, stochastic calculus

**Optimization** — linear programming, convex optimization, combinatorial optimization, integer programming

---

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
