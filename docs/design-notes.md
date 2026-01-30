# Design Notes: Skill Composition

## Core Principles

- **Declarative first**: skills are data, not code.
- **Deterministic planning**: step order is stable and reproducible.
- **Guards and policies**: validate preconditions and ensure safety.
- **Composable**: skills can reference other skills.

## Step Model

- Steps reference tool IDs and input bindings.
- Steps can be sequential, parallel, or conditional (future).

## Integration Points

- `toolrun`: execution of steps
- `toolset`: curated tool availability
- `toolobserve`: telemetry around skill execution
- `metatools-mcp`: expose skills as higher-level capabilities

## Interface Contracts

### Runner

- **Concurrency:** implementations are safe for concurrent use.
- **Context:** honors cancellation/deadlines and returns `ctx.Err()` when canceled.
- **Errors:** execution failures return non-nil error; no panic.

### Guard

- **Concurrency:** implementations are safe for concurrent use.
- **Errors:** validation failures return non-nil error; no panic.
