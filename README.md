# toolskill

Skill composition primitives for reusable tool workflows.

## Overview

toolskill defines a declarative model for composing tools into reusable skills:
sequences, branching, and guardrails. It is orchestration-only: **no execution,
no transport, no network I/O**. Execution is delegated to `toolrun`.

## Design Goals

1. Declarative skill definitions
2. Deterministic step ordering
3. Safe composition with policies/guards
4. Integration with `toolset` and `toolobserve`
5. Minimal dependencies

## Position in the Stack

```
toolset + toolrun + toolobserve --> toolskill --> metatools-mcp
```

## Versioning

toolskill follows semantic versioning aligned with the stack. The source of
truth is `ai-tools-stack/go.mod`, and `VERSIONS.md` is synchronized across repos.

## Next Steps

- See `docs/index.md` for usage and design notes.
- PRD and execution plan live in `docs/plans/`.
