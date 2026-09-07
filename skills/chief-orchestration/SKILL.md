---
name: chief-orchestration
description: Coordinate substantial multi-part implementation work through a strict Chief-to-Commander-to-Implementer/Verifier hierarchy, with dynamic agent preflight, bounded delegation, and evidence-based completion gates. Use for complex workflows requiring orchestration; avoid for ordinary single-agent tasks.
---

# Chief orchestration

Use this skill when the work has multiple independent or dependent parts and needs the configured agent hierarchy. Follow applicable repository instructions and relevant tool or framework documentation; this skill is cross-project and does not impose project-specific tools or frameworks.

## Roles and topology

The primary session is the Chief. The Chief owns the approved plan, task decomposition, dependency map, safe concurrency, integration, communication with the human, and the final project-wide completion gate. The Chief delegates exactly one configured `commander` selector for each independent Chief-level task. The Chief does not implement production work and does not spawn or manage Implementers or Verifiers directly.

Each Commander owns exactly one Chief-level task and is orchestration-only. A Commander creates bounded assignments for configured `implementer` selectors. Every assignment states the objective, exact scope and files or components, constraints, acceptance criteria, required checks, dependencies, expected output, and explicit non-scope. A Commander must use an independent configured `verifier` selector for substantial work.

Implementers perform only their assigned execution and cannot spawn subagents. An independent Verifier reviews the actual work read-only, makes no fixes, and returns an actionable `PASS` or `FAIL`. A `FAIL` must include severity, location or symbol, reason, required correction, and the verification that should follow.

Keep communication strictly routed:

```text
Human ↔ Chief ↔ Commander ↔ Implementer / Verifier
```

Implementers and Verifiers report to their Commander. Commanders report to the Chief and do not communicate with humans. Questions and genuine unresolved product, architecture, security, or requirements decisions escalate upward through that chain; routine implementation choices should be inferred from the task and repository instructions without pausing.

## Preflight and delegation

Before any delegation, inspect the runtime's currently callable custom-agent selectors and their configured roles. Inspect the current slot budget and concurrency, and reserve enough capacity for descendants and required independent review. Runtime availability is authoritative; do not rely on stale model names, cached configuration, or assumed selectors.

The intended configured role mappings are `commander = gpt-5.6-terra/high`, `implementer = gpt-5.6-luna/high`, and `verifier = gpt-5.6-terra/high`, in the existing personal agent definitions under `%USERPROFILE%\.codex\agents`. Treat these mappings as a verification target and dependency: inspect the current callable selector and configured role or mapping dynamically, never duplicate or assume those definitions, and follow actual runtime truth if it differs. Escalate through the chain when that discrepancy prevents the required hierarchy; do not hardcode behavior around a stale mapping.

Temporary slot scarcity is handled by scheduling work, reusing capacity, or waiting while preserving reserved slots for descendants and required review. Do not immediately escalate for temporary pressure. Escalate only when a necessary configured selector is genuinely unavailable or the required topology is otherwise impossible to honor. Never substitute a generic agent or silently collapse roles.

Delegate only bounded, actionable tasks. Parallelize genuinely independent work when capacity allows. Sequence work with overlapping files, migrations, schemas, shared interfaces, or dependencies. Preserve the user's scope and authorization; do not add product requirements or architectural redesigns.

## Execution and review gates

The Commander inspects the actual changes and test or check evidence from each Implementer. It returns deficient work with precise correction instructions, then reruns verification after every material correction. It must not quietly fix Implementer work, spawn another Commander, broaden scope, or report success from a claim alone.

A Commander may report `COMPLETE` only after its acceptance criteria, required checks, and independent review pass, with corrections rechecked where applicable. Its report to the Chief must be concise and evidence-based: status, changed components, checks, review findings and resolution, deviations, risks, and unresolved decisions. Do not pass raw transcripts upward except when needed to diagnose a blocker.

The Chief must perform the final project-wide gate. Do not declare overall completion merely from Commander reports: verify every approved plan item, dependency ordering, integration, project-wide checks, regressions, and hidden deviations. Distill the result for the human and surface only material risks or decisions.
