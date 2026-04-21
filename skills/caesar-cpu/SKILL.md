---
name: caesar-cpu
description: Use when user invokes /caesar-cpu to activate team-mode code review, batched commits, and knowledge map building
---

# Caesar-CPU

## Overview

Activates autonomous multi-agent orchestration for code review, batched commits, and knowledge map expansion — mirroring OMC's team mode. Spawns specialized agents to work in parallel, coordinates via shared task list, and persists learnings to wiki.

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
    "Load harness skills" [shape=box];
    "Create team" [shape=box];
    "Analyze codebase" [shape=box];
    "Spawn: reviewer, committer, wiki-agent" [shape=box];
    "Code Review Task" -> "Batch Commit Task" -> "Wiki Update Task" [style=dashed];
    "Parallel: all three agents" [shape=box];
    "Team shutdown" [shape=box];
    "Report results" [shape=ellipse];

    "Start: /caesar-cpu" -> "Load harness skills";
    "Load harness skills" -> "Create team";
    "Create team" -> "Analyze codebase";
    "Analyze codebase" -> "Spawn: reviewer, committer, wiki-agent";
    "Spawn: reviewer, committer, wiki-agent" -> "Parallel: all three agents";
    "Parallel: all three agents" -> "Team shutdown";
    "Team shutdown" -> "Report results";
}
```

## Implementation

### Step 1: Load Required Skills

```bash
# Load harness skills for team orchestration
Skill: oh-my-claudecode:omc-reference
Skill: superpowers:code-review
Skill: superpowers:verification-before-completion
```

### Step 2: Create Team

```javascript
TeamCreate({
  team_name: "caesar-cpu",
  description: "Code review + batch commit + knowledge map team"
})
```

### Step 3: Create Tasks

```
TaskCreate: "Code Review"
TaskCreate: "Batch Commit Planning" 
TaskCreate: "Knowledge Map Update"
```

### Step 4: Spawn Agents

Spawn 3 parallel agents using `executor` subagent_type:

1. **code-reviewer**: Reviews changed files, logs issues
2. **committer**: Plans batch commits based on review feedback
3. **wiki-agent**: Updates knowledge map with new patterns/decisions

### Step 5: Coordinate & Shutdown

- Monitor task completion via TaskList
- Collect findings from each agent
- Graceful shutdown via SendMessage/shutdown_request
- TeamDelete on completion

## Quick Reference

| Phase | Action | Tool |
|-------|--------|------|
| Init | Load skills | Skill tool |
| Setup | Create team | TeamCreate |
| Plan | Create tasks | TaskCreate |
| Execute | Spawn agents | Agent (run_in_background: true) |
| Monitor | Check tasks | TaskList |
| Complete | Shutdown team | SendMessage + TeamDelete |

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Spawning agents without tasks | Create tasks FIRST, then assign via TaskUpdate |
| No skill loading | Always load omc-reference first for team protocol |
| Skipping verification | Use verification-before-completion skill |
| Orphaned agents | Send shutdown_request to each before TeamDelete |

## Integration

Loads these skills dynamically:
- `oh-my-claudecode:team` — team orchestration protocol
- `oh-my-claudecode:verify` — verification strategy
- `superpowers:requesting-code-review` — review workflow
- `oh-my-claudecode:wiki` — knowledge persistence
