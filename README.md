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

## Installation

```bash
go get github.com/jonwraymond/toolskill
```

## Quick Start

```go
package main

import (
    "context"
    "fmt"

    "github.com/jonwraymond/toolskill"
)

type simpleRunner struct{}

func (simpleRunner) Run(ctx context.Context, step toolskill.Step) (any, error) {
    return step.ID + \"-ok\", nil
}

func main() {
    skill := toolskill.Skill{
        Name: \"summarize\",\n        Steps: []toolskill.Step{\n            {ID: \"search\", ToolID: \"mcp:search\"},\n            {ID: \"summarize\", ToolID: \"mcp:summarize\"},\n        },\n    }\n\n    plan, _ := toolskill.NewPlanner().Plan(skill)\n    results, _ := toolskill.Execute(context.Background(), plan, simpleRunner{})\n\n    for _, r := range results {\n        fmt.Println(r.StepID, r.Value)\n    }\n}\n```

## Versioning

toolskill follows semantic versioning aligned with the stack. The source of
truth is `ai-tools-stack/go.mod`, and `VERSIONS.md` is synchronized across repos.

## Next Steps

- See `docs/index.md` for usage and design notes.
- PRD and execution plan live in `docs/plans/`.
