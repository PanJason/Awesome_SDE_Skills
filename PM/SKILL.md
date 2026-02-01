---
name: product-manager
description: Create a concise, execution-ready PM plan **PLAN.md** that can be handed over to architect for concrete product design
metadata:
  short-description: PM plan with requirements, scope, metrics, phased delivery, and architect handoff artifacts.
  style: concise
  audience: product + engineering + architecture
---

# Create Product Plan + Architect Handoff

## Goal

Turn a user prompt into a **single, actionable PM plan** that:
- frames the **user problem and desired outcomes**,
- defines **scope, principles, success metrics, and delivery phases**,
- provides an **execution checklist** that engineering can follow,

## Minimal workflow

Throughout the entire workflow, operate in read-only mode. Do not write or update files.

1. **Scan context quickly (product + system)**
   - Read `README.md` and obvious docs (`docs/`, `CONTRIBUTING.md`,
     `ARCHITECTURE.md`).
   - Identify product surface area: UI/UX entry points, API boundaries, data
     stores, integrations, etc. Ask user if uncertain.
   - Identify existing patterns: feature flags, telemetry, authn/authz, config,
     error handling, etc. Ask user if uncertain.
   - Note relevant existing modules/files only as pointers (no edits).

2. **Clarify only the highest-leverage unknowns**
   - Ask **questions** if truly blocking. Questions are given together with
     possible options.
   - Prefer multiple-choice (persona, platform, “must-have” constraints,
     definition of success).
   - If not blocked, state assumptions explicitly and proceed.

3. **Produce an execution-ready plan using the template below**
   - Start with **1 short paragraph** describing the user problem and approach.
   - Clearly call out **scope** (in/out) and **non-goals**.
   - Provide **success metrics** (leading + lagging indicators).
   - Provide a **phased delivery** plan (MVP → V1 → later) when appropriate.
   - Provide a **small checklist** of action items (default 8–12 items),
     ordered: discovery → design → implementation → validation → analytics →
     rollout.
   - Using **Vertical Slice** planning pattern, which focuses on shipping one
     thin end-to-end path (UI → API → data → observable output) first.
   - If unknowns remain, include **Open questions**.

4. **Output only the plan using the template below (no meta commentary).**
5. **Write the plan to the PLAN.md**

## Plan template (follow exactly)

```markdown
# Plan

<1–3 sentences: user problem, intended outcome, and approach (phased if needed).>

## Requirements
- Functional (must-have):
- Functional (nice-to-have):

## Scope
- In:
- Out:
- Non-goals:

## Success metrics
- Leading indicators:
- Lagging indicators:
- Guardrails (quality, performance, safety):

## Delivery approach
- MVP:
- V1:
- Later:

## Architect handoff (to concretize implementation and handoff to the developer team)
- System boundaries & dependencies:
  - Upstream:
  - Downstream:
  - External services/integrations:
- Interfaces & contracts:
  - Public API/CLI/UI contracts (inputs/outputs, error model):
  - Data model changes (entities, fields, migrations):
  - Permissions/authz model:
- Observability:
  - Events to track (name + properties):
  - Logs/traces/metrics (what must exist for debugging and SLOs):
  - Dashboards/alerts (what would indicate success/failure):
- Rollout & operations:
  - Feature flag strategy:
  - Backward compatibility considerations:
  - Rollback plan:
- Test strategy:
  - Acceptance criteria (Given/When/Then bullets):
  - Unit/integration/e2e coverage focus:
  - Edge case matrix (top risks to test):

## Action items
[ ] <Step 1: discovery / alignment>
[ ] <Step 2: UX spec / requirements>
[ ] <Step 3: architect handoff artifacts prepared>
[ ] <Step 4: implementation tasks breakdown>
[ ] <Step 5: testing & verification>
[ ] <Step 6: staged rollout>
[ ] <Step 7: post-launch review & iteration>

## Risks & edge cases (Optional)
- <Risk/edge case 1 + mitigation>
- <Risk/edge case 2 + mitigation>
- <Risk/edge case 3 + mitigation>

## Open questions
- <Question 1>
- <Question 2>
- <Question 3>
```

## Guidance for “execution-ready but implementation-agnostic”

Do:
- Specify contracts (inputs/outputs/errors), not algorithms.
- Specify states and user-visible behaviors.
- Specify data entities/fields at the conceptual level (what exists, not how
  indexed).
- Specify acceptance criteria in Given/When/Then form.
- Specify rollout mechanics (flag, staged release, rollback triggers).
- Point to likely modules/files only as “touchpoints”.

Avoid:
- Picking libraries/frameworks/algorithms unless the user explicitly requests.
- “Just implement X” without defining behaviors, metrics, and contracts.
- Unbounded scope; always define non-goals.