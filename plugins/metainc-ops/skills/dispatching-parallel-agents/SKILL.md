---
name: dispatching-parallel-agents
description: Use when facing 2+ independent tasks that can be worked on without shared state or sequential dependencies
---

> Vendored from obra/superpowers (skills/dispatching-parallel-agents). Source: https://github.com/obra/superpowers

# Dispatching Parallel Agents

## Overview

You delegate tasks to specialized agents with isolated context. By precisely crafting their instructions and context, you ensure they stay focused and succeed at their task. They should never inherit your session's context or history — you construct exactly what they need. This also preserves your own context for coordination work.

When you have multiple unrelated failures (different test files, different subsystems, different bugs), investigating them sequentially wastes time. Each investigation is independent and can happen in parallel.

**Core principle:** Dispatch one agent per independent problem domain. Let them work concurrently.

## When to Use

**Use when:**
- 3+ test files failing with different root causes
- Multiple subsystems broken independently
- Each problem can be understood without context from others
- No shared state between investigations

**Don't use when:**
- Failures are related (fix one might fix others)
- Need to understand full system state
- Agents would interfere with each other

## The Pattern

### 1. Identify Independent Domains
Group failures by what's broken; each domain is independent.

### 2. Create Focused Agent Tasks
Each agent gets: specific scope, clear goal, constraints, expected output.

### 3. Dispatch in Parallel
Issue all subagent dispatches in the same response — they run in parallel. One per response = sequential.

### 4. Review and Integrate
Read each summary, verify fixes don't conflict, run full test suite, integrate.

## Agent Prompt Structure
Good agent prompts are: focused (one clear domain), self-contained (all context needed), specific about output.

## Common Mistakes
- ❌ Too broad ("Fix all the tests") → ✅ Specific ("Fix agent-tool-abort.test.ts")
- ❌ No context → ✅ Paste error messages and test names
- ❌ No constraints → ✅ "Do NOT change production code"
- ❌ Vague output → ✅ "Return summary of root cause and changes"

## Key Benefits
Parallelization, focus, independence, speed — multiple investigations happen simultaneously without interfering.
