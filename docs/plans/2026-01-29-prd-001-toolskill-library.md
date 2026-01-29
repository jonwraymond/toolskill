# PRD-001: toolskill Library Implementation

> **For agents:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a skill composition library for reusable tool workflows with
step planning, guardrails, and execution adapters.

**Architecture:** Declarative skill definitions compiled into an execution plan
that can be run via `toolrun`. Skill execution supports guards and optional
telemetry hooks.

**Tech Stack:** Go 1.24+, integrates with toolrun/toolset/toolobserve.

**Priority:** P3 (Phase 5 in the plan-of-record)

---

## Context and Stack Alignment

toolskill provides a higher-level workflow layer on top of:
- `toolset` for curated tool availability
- `toolrun` for executing steps
- `toolobserve` for telemetry hooks

It should remain declarative and deterministic.

---

## Requirements

### Functional

1. Declarative skill model with steps and bindings.
2. Deterministic planner with stable ordering.
3. Guards and policies to validate steps.
4. Execution adapter delegating to `toolrun`.
5. Structured result model per step.

### Non-functional

- No direct tool execution or network access.
- Deterministic ordering for reproducible results.
- Context propagation in all execution APIs.

---

## API Model (Target)

```go
// Skill represents a declarative workflow.
type Skill struct {
    Name  string
    Steps []Step
}

// Step references a tool and its bindings.
type Step struct {
    ID     string
    ToolID string
    Inputs map[string]any
}

// Guard validates a skill or step.
type Guard interface {
    Validate(skill Skill) error
}
```

---

## Directory Structure

```
toolskill/
├── skill.go
├── skill_test.go
├── step.go
├── step_test.go
├── planner.go
├── planner_test.go
├── guard.go
├── guard_test.go
├── execute.go
├── execute_test.go
├── doc.go
├── README.md
├── go.mod
└── go.sum
```

---

## TDD Task Breakdown (Detailed)

### Task 1 — Skill + Step Models

**Files:** `skill.go`, `step.go`, `*_test.go`

**Tests:**
- `TestSkill_SerializationDeterministic`
- `TestStep_ValidatesToolID`

**Commit:** `feat(toolskill): add skill and step models`

---

### Task 2 — Planner

**Files:** `planner.go`, `planner_test.go`

**Tests:**
- `TestPlanner_DeterministicOrdering`
- `TestPlanner_DetectsMissingSteps`

**Commit:** `feat(toolskill): add deterministic planner`

---

### Task 3 — Guards

**Files:** `guard.go`, `guard_test.go`

**Tests:**
- `TestGuard_MaxSteps`
- `TestGuard_AllowedToolIDs`

**Commit:** `feat(toolskill): add guard policies`

---

### Task 4 — Execution Adapter

**Files:** `execute.go`, `execute_test.go`

**Tests:**
- `TestExecute_OrderPreserved`
- `TestExecute_ErrorPropagation`
- `TestExecute_ContextPropagation`

**Commit:** `feat(toolskill): add execution adapter`

---

### Task 5 — Docs + Examples

**Files:** `README.md`, `docs/index.md`, `docs/user-journey.md`

**Acceptance:** Mermaid diagram and quick start examples included. Add D2
component diagram in ai-tools-stack.

**Commit:** `docs(toolskill): finalize documentation`

---

## PR Process

1. Create branch: `feat/toolskill-<task>`
2. Implement TDD task in isolation
3. Run: `go test -race ./...`
4. Commit with scoped message
5. Open PR against `main`
6. Merge after CI green

---

## Versioning and Propagation

- **Source of truth:** `ai-tools-stack/go.mod`
- **Matrix:** `ai-tools-stack/VERSIONS.md` (auto-synced)
- **Propagation:** `ai-tools-stack/scripts/update-version-matrix.sh --apply`
- Tags: `vX.Y.Z` and `toolskill-vX.Y.Z`

---

## Integration with metatools-mcp

- Expose skills as higher-level tools via provider layer.
- Optionally map skills to MCP tools with composite schemas.

---

## Definition of Done

- All tasks complete with tests passing
- `go test -race ./...` succeeds
- Docs + diagrams updated in ai-tools-stack
- CI green
- Version matrix updated after first release
