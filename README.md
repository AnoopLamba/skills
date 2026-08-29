# skills

[![skills.sh](https://skills.sh/b/AnoopLamba/skills)](https://skills.sh/AnoopLamba/skills)

Reusable Agent Skills by Anoop Lamba.

## Available skills

### `problem-handoff`

Create self-contained, evidence-first problem statements for transferring an existing problem to another human, LLM, coding agent, research agent, reviewer, or new session.

The skill is designed to preserve useful investigation context while reducing accidental anchoring. It keeps verified evidence, reproduction details, prior experiments, constraints, current state, provenance, and unknowns separate from unverified diagnoses or preferred solutions.

It supports three common handoff modes:

- **Independent investigation** — prepare a neutral brief so the recipient can form its own diagnosis.
- **Directed investigation** — preserve a specific hypothesis or approach as a question to evaluate, not as fact.
- **Human communication** — package the issue for another developer, maintainer, vendor, reviewer, or support team.

The skill does **not** replace normal debugging, research, implementation planning, code review, or initial problem framing. It is intended specifically for situations where an already-known problem needs to be packaged, transferred, delegated, escalated, or continued elsewhere.

## Install

Install the skill with the Agent Skills CLI:

```bash
npx skills add AnoopLamba/skills --skill problem-handoff
```

Or install from the repository URL:

```bash
npx skills add https://github.com/AnoopLamba/skills --skill problem-handoff
```

## Repository structure

```text
skills/
├── problem-handoff/
│   └── SKILL.md
├── LICENSE
└── README.md
```

Additional reusable skills may be added as separate directories over time.

## License

MIT License. See [LICENSE](LICENSE).
