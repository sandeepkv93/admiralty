# Assessment Rubrics

Reference material for Phase 2 classification. Load this file during assessment; do not hold it in context otherwise.

## Complexity Rubric

| Level  | Concerns | Files   | Deliverables | Ambiguity   | Architectural Impact |
|--------|----------|---------|--------------|-------------|---------------------|
| Small  | 1        | < 3     | 1            | Low         | None                |
| Medium | 2-4      | 5-15    | 2-4          | Moderate    | Local               |
| Large  | 5+       | 15+     | 5+           | High        | Cross-cutting       |

Notes:
- Count concerns as distinct functional areas (e.g. auth, routing, database, UI).
- Count files as the estimated number of files to create or modify.
- When a task sits between two levels, round UP.

## Threat Tier Decision Tree

Evaluate top-down. Assign the first tier that matches.

```
Station 3 (Trafalgar):
  - Irreversible actions (data deletion, production deploy without rollback)
  - Regulated or safety-sensitive effects
  - Mission failure causes severe incident
  ↓ no match

Station 2 (Action):
  - Security, privacy, compliance, or data integrity implications
  - High customer or financial blast radius
  - Difficult rollback or uncertain side effects
  ↓ no match

Station 1 (Caution):
  - User-visible behavior changes
  - Moderate reliability or cost impact
  - Partial coupling to other tasks
  ↓ no match

Station 0 (Patrol):
  - Low blast radius
  - Easy rollback
  - No sensitive data, security, or compliance impact
```

## Signal Words Table

Keywords in the task description that force a minimum threat tier.

| Signal Word / Phrase | Minimum Tier |
|---------------------|--------------|
| production, prod, live | Station 2 |
| PII, personal data, GDPR, HIPAA | Station 2 |
| delete, drop, remove permanently | Station 2 |
| payment, billing, financial | Station 2 |
| auth, authentication, credentials | Station 1 |
| migration, schema change | Station 1 |
| API contract, breaking change | Station 1 |
| refactor, rename | Station 0 |
| test, lint, format | Station 0 |
| docs, README, comments | Station 0 |

When multiple signal words appear, use the highest minimum tier.

## Execution Mode Heuristics

Choose the first matching condition.

| Condition | Mode | Rationale |
|-----------|------|-----------|
| All work is sequential or same-file | `single-session` | Lowest coordination overhead |
| Tasks are parallel but independent, results merge at admiral | `subagents` | Fast throughput, no peer coordination |
| Parallel tasks with shared state or cross-dependencies | `agent-team` | Supports direct agent-to-agent coordination |
| Station 2+ threat tier | `agent-team` + red-cell navigator | Explicit adversarial review checkpoints |

## Gap Detection Checklist

Verify each item is present or inferable from the task description. Mark missing items as gaps.

1. **Target outcome**: What does "done" look like?
2. **Success metric**: How do we measure success?
3. **Scope boundary**: What is explicitly out of scope?
4. **Environment**: Where does this run (language, framework, platform)?
5. **Existing state**: What already exists that this builds on or modifies?
6. **Dependencies**: External services, libraries, or APIs involved?
7. **Constraints**: Deadlines, budgets, performance requirements?
8. **Rollback path**: Can changes be reversed if something goes wrong?

## Common Defaults

Conservative fallback values for non-critical gaps. Apply these when the gap is not critical enough to warrant asking the user.

| Gap | Default Value | Rationale |
|-----|---------------|-----------|
| Deadline | End of current session | Prevents open-ended scope |
| Token/time budget | No hard limit; flag if approaching 80% of session | Avoids premature stop |
| Reliability floor | All tests pass, no regressions | Safe baseline |
| Compliance constraints | None assumed unless signal words detected | Avoids over-constraining |
| Forbidden actions | No force-push, no production writes, no secret exposure | Safe defaults |
| Rollback path | Git revert or branch reset | Standard for code tasks |
| Success metric | Deliverables complete + validation evidence present | Minimum viable criterion |
| Environment | Infer from project files (package.json, go.mod, etc.) | Context-driven |
