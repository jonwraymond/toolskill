# toolskill

Skill composition primitives for reusable tool workflows.

## Overview

toolskill defines a declarative model for composing tools into reusable skills:
sequences, branching, and guardrails. It does not execute tools itself; it
integrates with `toolrun` for execution and `toolobserve` for telemetry.

## Design Goals

1. Declarative skill definitions
2. Deterministic step ordering
3. Policy and guard integration
4. Composable, reusable workflows
5. Minimal dependencies

## Position in the Stack

```
toolset + toolrun + toolobserve --> toolskill --> metatools-mcp
```

## Core Types

| Type | Purpose |
|------|---------|
| `Skill` | Declarative workflow definition |
| `Step` | Single tool call or sub-skill |
| `Guard` | Pre/post conditions and policy checks |
| `Planner` | Resolves step order and dependencies |

## Quick Start

```go
skill := toolskill.New("summarize-docs").
    Step("search", "mcp:search").
    Step("summarize", "mcp:summarize")

_ = skill
```

## Versioning

toolskill follows semantic versioning aligned with the stack. The source of
truth is `ai-tools-stack/go.mod`, and `VERSIONS.md` is synchronized across repos.

## Next Steps

- [Design Notes](design-notes.md)
- [User Journey](user-journey.md)
- [Plans](plans/README.md)
