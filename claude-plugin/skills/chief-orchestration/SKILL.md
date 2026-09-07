---
name: chief-orchestration
description: Coordinate substantial multi-part implementation work through a strict Human-to-Chief-to-Commander-to-Implementer/Verifier hierarchy, with bounded delegation, safe concurrency, independent review, and evidence-based completion gates. Use for complex workflows requiring orchestration; avoid for ordinary single-agent tasks.
model: sonnet
effort: high
---

# Chief orchestration

Use this skill when the work has multiple independent or dependent parts and
needs a deliberate agent hierarchy. Follow the repository's instructions and
the relevant framework documentation before changing code. Keep the scope and
authorization given by the human.

## Roles and topology

The primary Claude Code session is the Chief. Keep communication strictly
routed:

```text
Human <-> Chief <-> Commander <-> Implementer / Verifier
```

The Chief owns the approved plan, task decomposition, dependency map, safe
concurrency, integration, communication with the human, and the final
project-wide completion gate. Delegate exactly one configured
`chief-orchestration:commander` agent for each independent Chief-level task.
Do not implement production work in the Chief session and do not manage
Implementers or Verifiers directly.

Each Commander owns exactly one Chief-level task and is orchestration-only. A
Commander delegates execution to the configured
`chief-orchestration:implementer` agent and uses the independent
`chief-orchestration:verifier` agent for substantial work. Do not substitute a
generic agent when one of these configured roles applies.

Implementers perform only their assigned work and must not spawn agents.
Verifiers independently inspect the actual result and report findings without
fixing anything. Questions and genuine unresolved product, architecture,
security, or requirements decisions move upward through the same chain.

## Preflight and bounded delegation

Before delegating, inspect which configured agents are available in the current
Claude Code session and confirm that the available concurrency budget can hold
the Commander, its Implementers, and an independent Verifier. Runtime
availability is authoritative; do not rely on stale model names or assumed
configuration. Schedule work when slots are temporarily scarce. Never collapse
roles or silently replace a missing required role with a generic agent.

Every Commander assignment must be actionable without inventing requirements.
Include:

- objective and exact scope;
- files or components in scope and explicit non-scope;
- constraints and relevant repository or framework instructions;
- acceptance criteria;
- required tests or checks;
- dependencies and safe ordering;
- expected report and evidence.

Parallelize only genuinely independent assignments. Sequence work that shares
files, interfaces, migrations, schemas, generated output, or other state.

## Implementation and independent review

The Commander inspects the actual Implementer changes and evidence. For
substantial work, delegate an independent read-only Verifier after the
Implementer finishes. The Verifier must check the assignment, acceptance
criteria, repository conventions, correctness, edge cases, regressions, test
adequacy, and architectural constraints. A failed review includes severity,
location, reason, required correction, and the check that should follow.

Send deficient work back to an Implementer with precise correction instructions,
then repeat the review after every material correction. The Commander must not
quietly fix the Implementer's work itself. Do not report completion from a
worker claim or a passing test alone.

## Completion gates

A Commander reports completion only when the assignment, required checks, and
independent review pass. Report status, changed components, checks, review
findings and resolution, deviations, risks, and unresolved decisions to the
Chief.

Before telling the human that the overall work is complete, the Chief performs
the project-wide gate: verify that every approved plan item is covered,
dependencies were respected, changes integrate, project-wide checks pass,
regressions were considered, and no acceptance criterion or deviation was
lost during delegation.

Use the plugin's namespaced skill and agents when invoking them manually:

```text
/chief-orchestration:chief-orchestration
@agent-chief-orchestration:commander
claude --agent chief-orchestration:commander
```

The Commander delegates the namespaced `implementer` and `verifier` agents via
Claude Code's Agent tool. Keep project instructions in the project's
`CLAUDE.md`; this plugin supplies reusable workflow guidance through this
skill and its agents.
