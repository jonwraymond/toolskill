# Toolskill Implementation Plan — Professional TDD Execution

Status: Ready for Implementation
Date: 2026-01-29
PRD: docs/plans/2026-01-29-prd-001-toolskill-library.md

## Overview

Implement skill composition primitives and a deterministic planner that
integrates with toolrun for execution.

## TDD Methodology

Each task follows strict TDD:
1. Red — write failing test
2. Red verification — run test, confirm failure
3. Green — minimal implementation
4. Green verification — run test, confirm pass
5. Commit — one commit per task

---

## Task 0 — Module Scaffolding

Commit:
- chore(toolskill): scaffold module and docs

---

## Task 1 — Skill + Step Models

Tests:
- TestSkill_SerializationDeterministic
- TestStep_ValidatesToolID

Commit:
- feat(toolskill): add skill and step models

---

## Task 2 — Planner

Tests:
- TestPlanner_DeterministicOrdering
- TestPlanner_DetectsMissingSteps

Commit:
- feat(toolskill): add deterministic planner

---

## Task 3 — Guards

Tests:
- TestGuard_MaxSteps
- TestGuard_AllowedToolIDs

Commit:
- feat(toolskill): add guard policies

---

## Task 4 — Execution Adapter

Tests:
- TestExecute_OrderPreserved
- TestExecute_ErrorPropagation
- TestExecute_ContextPropagation

Commit:
- feat(toolskill): add execution adapter

---

## Task 5 — Docs + Diagrams

Commit:
- docs(toolskill): finalize documentation

---

## Quality Gates

- go test -v -race ./...
- go test -cover ./...
- go vet ./...
- golangci-lint run (if configured)

---

## Stack Integration

1. Add ai-tools-stack component docs + D2 diagram
2. Add mkdocs multirepo import
3. After first release, update version matrix

---

## Commit Order

1. chore(toolskill): scaffold module and docs
2. feat(toolskill): add skill and step models
3. feat(toolskill): add deterministic planner
4. feat(toolskill): add guard policies
5. feat(toolskill): add execution adapter
6. docs(toolskill): finalize documentation
7. docs(ai-tools-stack): add toolskill component docs
8. chore(ai-tools-stack): add toolskill to version matrix (after release)
