---
name: caesar-cpu
description: Use when user invokes /caesar-cpu to activate ultrawork-mode code review, batched commits, and knowledge map building
---

# Caesar-CPU

## Overview

Activates autonomous multi-agent orchestration for code review, batched commits, and knowledge map expansion — using ultrawork parallel execution model. Spawns specialized agents to work in parallel via shared task list, and persists learnings to wiki.

## When to Use

**Trigger:** User types `skill:/caesar-cpu` or `/caesar-cpu`

**Symptoms:**
- Codebase needs systematic review before commit
- Multi-file changes requiring coordinated review
- Knowledge map (wiki) needs updating after architectural changes
- User wants automated pipeline instead of manual agent dispatching

## Core Workflow

```dot
digraph caesar-cpu-flow {
    "Start: /caesar-cpu" [shape=ellipse];
    "Load skills" [shape=box];
    "Create tasks" [shape=box];
    "Spawn: reviewer, committer, wiki-agent in parallel" [shape=box];
    "Monitor via TaskList" [shape=box];
    "Verify results" [shape=box];
    "Report results" [shape=ellipse];

    "Start: /caesar-cpu" -> "Load skills";
    "Load skills" -> "Create tasks";
    "Create tasks" -> "Spawn: reviewer, committer, wiki-agent in parallel";
    "Spawn: reviewer, committer, wiki-agent in parallel" -> "Monitor via TaskList";
    "Monitor via TaskList" -> "Verify results";
    "Verify results" -> "Report results";
}
```

## Implementation

### Step 1: Load Required Skills

```bash
# Load skills for ultrawork orchestration
Skill: superpowers:verification-before-completion
Skill: superpowers:code-review
Skill: oh-my-claudecode:wiki
```

### Step 2: Read Agent Tiers

```bash
# Read agent tiers for correct model routing
Read: docs/shared/agent-tiers.md
```

### Step 3: Create Tasks

```
TaskCreate: "Code Review" - Review changed files, log issues
TaskCreate: "Batch Commit Planning" - Plan commits based on review
TaskCreate: "Knowledge Map Update" - Update wiki with patterns/decisions
```

### Step 4: Spawn Agents in Parallel (Ultrawork)

Spawn 3 parallel agents using `oh-my-claudecode:executor`:

1. **reviewer** (sonnet): Reviews changed files, logs issues
2. **committer** (sonnet): Plans batch commits based on review feedback
3. **wiki-agent** (haiku): Updates knowledge map with new patterns

Use `run_in_background: true` for each agent.

### Step 5: Monitor & Verify

- Monitor task completion via TaskList
- Lightweight verification: build passes, tests pass
- Collect findings from each agent

## Ultrawork vs Team Mode

| Team Model | Ultrawork Model |
|------------|-----------------|
| TeamCreate/TeamDelete | Direct Task + Agent |
| SendMessage coordination | TaskList monitoring |
| Team shutdown protocol | No shutdown needed |
| Shared task list | Independent parallel tasks |

## Quick Reference

| Phase | Action | Tool |
|-------|--------|------|
| Init | Load skills | Skill tool |
| Setup | Create tasks | TaskCreate |
| Execute | Spawn parallel agents | Agent (run_in_background: true) |
| Monitor | Check tasks | TaskList |
| Verify | Lightweight check | Build/test commands |

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Spawning agents without tasks | Create tasks FIRST, then assign via TaskUpdate |
| Wrong model tier | Haiku=simple, Sonnet=standard, Opus=complex |
| Skipping verification | Use verification-before-completion skill |
| Blocking on agent completion | Use run_in_background, monitor via TaskList |

## Integration

Uses these skills:
- `superpowers:verification-before-completion` — verification strategy
- `superpowers:code-review` — review workflow
- `oh-my-claudecode:wiki` — knowledge persistence
- `oh-my-claudecode:executor` — parallel task execution
