# Migration Guide: toolskill to toolcompose/skill

This document describes how to migrate from `github.com/jonwraymond/toolskill` to `github.com/jonwraymond/toolcompose/skill`.

## Why the Change?

The `toolskill` package has been consolidated into `toolcompose` to provide a unified composition layer for the ApertureStack. This reduces dependency complexity and provides better integration between skill definitions and composition primitives.

## Installation

Remove the old dependency and add the new one:

```bash
go get github.com/jonwraymond/toolcompose
go mod tidy
```

## Import Path Changes

| Old Import | New Import |
|------------|------------|
| `github.com/jonwraymond/toolskill` | `github.com/jonwraymond/toolcompose/skill` |

## Code Changes

### Basic Import Update

```go
// Before
import (
    "github.com/jonwraymond/toolskill"
)

func main() {
    skill := toolskill.Skill{
        Name: "summarize",
        Steps: []toolskill.Step{
            {ID: "search", ToolID: "mcp:search"},
        },
    }
    plan, _ := toolskill.NewPlanner().Plan(skill)
    results, _ := toolskill.Execute(context.Background(), plan, runner)
}
```

```go
// After
import (
    "github.com/jonwraymond/toolcompose/skill"
)

func main() {
    s := skill.Skill{
        Name: "summarize",
        Steps: []skill.Step{
            {ID: "search", ToolID: "mcp:search"},
        },
    }
    plan, _ := skill.NewPlanner().Plan(s)
    results, _ := skill.Execute(context.Background(), plan, runner)
}
```

### Type Reference Updates

All types maintain the same names, just with the new package prefix:

| Old Type | New Type |
|----------|----------|
| `toolskill.Skill` | `skill.Skill` |
| `toolskill.Step` | `skill.Step` |
| `toolskill.Planner` | `skill.Planner` |
| `toolskill.Runner` | `skill.Runner` |
| `toolskill.Result` | `skill.Result` |

### Function Updates

| Old Function | New Function |
|--------------|--------------|
| `toolskill.NewPlanner()` | `skill.NewPlanner()` |
| `toolskill.Execute()` | `skill.Execute()` |

## Automated Migration

You can use `gofmt` to automate the import path change:

```bash
gofmt -w -r '"github.com/jonwraymond/toolskill" -> "github.com/jonwraymond/toolcompose/skill"' .
```

Then update type references manually or with sed:

```bash
find . -name "*.go" -exec sed -i '' 's/toolskill\./skill./g' {} \;
```

## Timeline

- **Now**: Begin migration to `toolcompose/skill`
- **Future**: `toolskill` repository will be archived

## Questions?

If you encounter issues during migration, please open an issue in the [toolcompose repository](https://github.com/jonwraymond/toolcompose/issues).
