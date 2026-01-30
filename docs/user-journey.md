# User Journey: Building a Skill

## Scenario

You want to define a reusable "summarize-research" skill that searches for
papers, extracts abstracts, and summarizes the result.

## Step 1: Define the skill

```go
skill := toolskill.Skill{
    Name: "summarize-research",
    Steps: []toolskill.Step{
        {ID: "search", ToolID: "mcp:search_papers"},
        {ID: "extract", ToolID: "mcp:extract_abstracts"},
        {ID: "summarize", ToolID: "mcp:summarize"},
    },
}
```

## Step 2: Add a guard

```go
guard := toolskill.MaxStepsGuard(5)
_ = guard.Validate(skill)
```

## Step 3: Execute via toolrun

```go
plan, _ := toolskill.NewPlanner().Plan(skill)
executor := toolrun.NewExecutor(...)
result, _ := toolskill.Execute(ctx, plan, executor)
```

## Flow Diagram

```mermaid
flowchart LR
  A[Skill Definition] --> B[Planner]
  B --> C[Guard/Policy]
  C --> D[toolrun Execution]
  D --> E[Results]
```
