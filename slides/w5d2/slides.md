---
marp: true
theme: default
paginate: true
size: 16:9
style: |
  section {
    padding: 30px 50px;
    font-size: 34px;
  }
  section::after {
    content: attr(data-marpit-pagination) ' / ' attr(data-marpit-pagination-total);
    font-size: 18px;
    color: #555;
    position: absolute;
    bottom: 30px;
    right: 50px;
  }
  header {
    font-size: 20px;
    color: #888;
  }
  h1 {
    color: #2c3e50;
  }
  h2 {
    color: #34495e;
    border-bottom: 2px solid #3498db;
    padding-bottom: 10px;
  }
  footer {
    display: none;
  }
  .columns { display: flex; gap: 40px; align-items: flex-start; }
  .col { flex: 1; }
  pre { font-size: 20px; }
  code { font-size: 20px; }
---

# Agentic Programming: Ralph Loop
## From Plan to Reliable Execution

Jian Weng  
CEMSE, KAUST  
Week 5, Session 2

---

# Today's Agenda

1. Why big plans fail in one session
2. What Ralph Loop is
3. How Ralph Loop is implemented
4. How to reduce hallucination/workaround risk
5. How to make it framework-neutral

---

# The Execution Gap

Assume we already have a good plan.

Why can execution still fail?

- The plan is too large for one AI session
- Long context increases drift/hallucination risk
- The model may take undesired shortcuts

A plan alone is not enough; we need an execution loop.

---

# Solution: Ralph Loop

- Core idea: auto-continue the same task across iterations
- Name source: Ralph Wiggum (Claude Code Plugin contributor)
- Goal: finish **BIG** tasks with controlled iterative execution

```text
One big task -> many bounded iterations -> one completed outcome
```

---

# What Is Ralph Loop?

Input per iteration:
- User prompt
- Current codebase state
- Max iteration limit

Output per iteration:
- Codebase modifications
- Progress signal for the next iteration

Stop condition:
- Task completed, or max iterations reached

---

# Loop Mechanics

```text
Iteration i:
  prompt + codebase_state_i -> model -> delta_i
  codebase_state_{i+1} = apply(delta_i)
  i = i + 1
  repeat until DONE or i == max_iter
```

Same prompt, updated state, repeated execution.

---

# Why Repeating Works

Citation: https://arxiv.org/pdf/2512.24601 (RLM)

- Re-feeding the task with updated state can improve quality
- The model re-evaluates "what to do next" every iteration
- Changed codebase state becomes implicit progress memory

Interpretation: looped execution helps planning and acting co-evolve.

---

# Where You Can Use It

Two practical paths:

1. Use built-in loop capabilities in tools like Kimi
2. Use Claude Code Plugin with hook-based continuation

Today we focus on hook-based implementation details.

---

# Hooking Basics

What is a hook?

- Inject custom logic before/after a system event
- Optionally replace default behavior

In Claude Code hooks:
- Use a stop-time hook when a session is about to end
- Decide whether to continue automatically

Reference: https://code.claude.com/docs/en/hooks

---

# Stop Hook Contract

Embed a control prompt in the hook:

```text
If the task is completed, print DONE.
Otherwise print CONTINUE.
Read the plan and current context before deciding.
```

Controller behavior:
- `DONE` -> terminate loop
- `CONTINUE` -> start next session iteration

---

# Remaining Risk After Auto-Continuation

Auto-continuation solves "task too big".

But we still need to fight:
- Hallucinated decisions
- Undesired workaround paths
- Misalignment with plan intent

So looping is necessary, but not sufficient.

---

# Why Prompt Refresh Helps

Practical insight:
- Recent input usually has stronger influence on model output
- "Multi-turn" is effectively one long context with newest suffix

Strategy:
- Open a separate control session
- Re-read both plan + latest code context
- Generate an updated embedded prompt for next iteration

---

# Guardrails for Real Projects

- Do not over-trust autonomous coding loops
- Verify against latest tool/plugin documentation
- Add human checkpoints for risky edits
- Start with partial manual scaffolding when needed

Use AI for speed, but keep engineering control.

---

# Framework-Neutral Ralph Loop

I do not like Cursor in 1st class.
Now, I do not like Claude Code additionally.

You can implement the same pattern anywhere.

```bash
for i in $(seq 1 "$MAX_ITER"); do
  run_session "$PLAN" "$CODEBASE"
  status=$(read_status)
  [ "$status" = "DONE" ] && break
done
```

Core is generic: `auto-continuation + control prompt + stop signal`.

---

# In-Class Practice

Implement a minimal Ralph Loop runner:

1. Define iteration limit and stop signal protocol
2. Execute one session and capture output status
3. Continue only on `CONTINUE`
4. Log each iteration decision
5. Stop on `DONE` or max iterations

Deliverable: one shell script + one short README.

---

# Takeaways

- Big tasks need iterative orchestration, not one-shot prompting
- Ralph Loop is a practical pattern for bounded auto-continuation
- Hook + explicit `DONE/CONTINUE` protocol makes control observable
- Reliability comes from looping **plus** guardrails
