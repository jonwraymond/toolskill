# PRD-001: toolskill Library Implementation

> **For agents:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a skill composition library for reusable tool workflows with
step planning, guardrails, and integration hooks.

**Architecture:** Declarative skill definitions compiled into an execution plan
that can be run via `toolrun`. Skill execution supports guards and optional
telemetry hooks.

**Tech Stack:** Go 1.24+, integrates with toolrun/toolset/toolobserve

**Priority:** P3 (Phase 5 in the plan-of-record)

---

## Context and Stack Alignment

toolskill provides a higher-level capability layer built on top of existing
execution and composition primitives:
- `toolset` controls which tools are available
- `toolrun` executes tool calls
- `toolobserve` provides telemetry

---

## Scope

### In scope
- Skill definition model (steps, inputs, outputs)
- Planner to resolve deterministic step order
- Guards and policies (max steps, allowed tool IDs)
- Execution adapter that delegates to `toolrun`
- Unit tests for deterministic planning
- Docs and examples

### Out of scope
- Workflow DSL or YAML parsing
- Long-running state persistence
- Retry/backoff orchestration (future)

---

## Design Principles

1. **Declarative skills**: define steps without embedding logic.
2. **Deterministic planning**: stable, reproducible order.
3. **Safe execution**: guardrails before/after steps.
4. **Composable**: skills can call sub-skills.
5. **Minimal dependencies**: rely on toolrun interfaces only.

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

## API Shape (Conceptual)

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
```

---

## Tasks (TDD)

### Task 1 — Skill + Step Models

- Define skill/step structures
- Tests: deterministic serialization

### Task 2 — Planner

- Resolve step order deterministically
- Tests: stable ordering, dependency handling

### Task 3 — Guards

- Max steps, allowed tool IDs
- Tests: guard failures are returned

### Task 4 — Execution Adapter

- Execute steps via `toolrun` interface
- Tests: execution order, error propagation

### Task 5 — Docs + Examples

- Update README and docs/index.md
- Add Mermaid flow diagram
- Add D2 component diagram in ai-tools-stack

---

## Versioning and Propagation

- **Source of truth**: `ai-tools-stack/go.mod`
- **Version matrix**: `ai-tools-stack/VERSIONS.md` (auto-synced)
- **Propagation**: `ai-tools-stack/scripts/update-version-matrix.sh --apply`
- Tags: `vX.Y.Z` and `toolskill-vX.Y.Z`

---

## Integration with metatools-mcp

- Expose skills as higher-level tools via provider layer.
- Optionally map skills to MCP tools with composite schemas.

---

## Definition of Done

- All TDD tasks complete with tests passing
- `go test -race ./...` succeeds
- Docs include quick start + diagrams
- CI green
- Version matrix updated after first release
