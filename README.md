# Admiralty

**Naval-Themed Planning Skill for Multi-Agent Execution**

Admiralty is a cross-client skill that transforms raw task descriptions into structured mission plans (sailing orders + battle plan) for downstream execution frameworks such as Nelson.

## Overview

Admiralty is the **planning layer** before execution. It analyzes task intent, repository state, constraints, and risk, then produces planning artifacts that are ready to hand off to an execution-oriented workflow.

It produces:

1. **Sailing Orders**: mission outcomes, constraints, scope, stop criteria, handoff artifacts
2. **Battle Plan**: decomposed tasks with ownership, dependencies, risks, validation, and rollback requirements

## Key Features

- **Four-phase workflow**: Reconnoitre -> Assess -> Draft Orders -> Signal
- **Repository-grounded planning**: planning must be based on observed current codebase state
- **Threat-tier classification**: Station 0-3 risk model
- **Execution mode selection**: single-session, subagents, or agent-team
- **Sanity checks before final signal**: measurable criteria, dependency validity, validation coverage
- **Cross-client compatibility**: usable from both Codex and Claude skill loaders

## Relationship to Nelson

Admiralty and Nelson are complementary:

- **Admiralty**: planning and risk framing
- **Nelson**: execution orchestration and coordination

Typical workflow:

1. Run `admiralty` with a task description
2. Review generated orders
3. Execute with your orchestration flow (for example Nelson)

## Installation

### Option A: Clone directly

#### Codex

```bash
git clone https://github.com/sandeepkv93/admiralty.git ~/.codex/skills/admiralty
```

#### Claude

```bash
git clone https://github.com/sandeepkv93/admiralty.git ~/.claude/skills/admiralty
```

### Option B: Install script (recommended)

```bash
git clone https://github.com/sandeepkv93/admiralty.git
cd admiralty
./scripts/install.sh both
```

Supported targets:

- `./scripts/install.sh codex`
- `./scripts/install.sh claude`
- `./scripts/install.sh both`

## Usage

Invoke the skill by name in your client.

Example prompts:

- "Use admiralty to draft orders for migrating auth middleware"
- "Use admiralty to produce a battle plan and threat tier for this refactor"

## Workflow Details

### Phase 1: Reconnoitre

- Identify intent, entities, constraints, and gaps
- Survey repository state thoroughly (layout, manifests, commands, branch/status)
- Capture recon evidence used by planning

### Phase 2: Assess

- Complexity: Small / Medium / Large
- Threat tier: Station 0-3
- Execution mode and team sizing

### Phase 3: Draft Orders

- Generate sailing orders and battle plan
- Include per-task risk, mitigation, validation, and rollback note requirements

### Phase 4: Signal

- Emit final structured output envelope
- Include confidence, unknowns, assumptions, and recon evidence

## File Structure

```text
admiralty/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── assessment-rubrics.md
│   └── admiralty-templates.md
└── scripts/
    └── install.sh
```

## Compatibility Notes

- Primary behavior is defined in `SKILL.md` and is client-agnostic.
- `agents/openai.yaml` provides Codex/OpenAI UI metadata and does not block Claude usage.

## Credits

Author: Sandeep Vishnu ([@sandeepkv93](https://github.com/sandeepkv93))

Companion execution framework: [Nelson](https://github.com/harrymunro/nelson)

## License

MIT License
