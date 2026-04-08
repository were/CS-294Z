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

# Agentic Programming: Towards Fully Hands-off
## Permission as the Last Barrier

Jian Weng  
CEMSE, KAUST  
Week 6, Session 1

---

# Today's Agenda

1. Recap: SDD and artifact-only interaction
2. Recap: Ralph Loop for continuous execution
3. Why full automation is still blocked
4. Permission models: v1 / v2 / v3 / v0
5. Practical policy design for safe autonomy

---

# Recap: SDD

SDD principle:
- Plan first, code later
- Follow standards for docs, tests, and code
- Review artifacts, not every keystroke

```text
Human intent -> Plan artifact -> Agent execution -> Reviewed artifact
```

Goal: reduce manual intervention while keeping quality control.

---

# Recap: Ralph Loop

Ralph Loop solves "one task is too big for one session."

- Reuse the same task prompt
- Feed updated codebase state each iteration
- Stop only on `DONE` or max iterations

```text
prompt + state_i -> iterate -> state_{i+1} -> ... -> DONE
```

---

# What Still Blocks Full Automation?

Even with SDD + Ralph Loop, we still get interruptions:

- "Should I do this edit?"
- "Should I run this command?"
- "Should I read this file?"

These permission stops break fully hands-off execution.

---

# Permission: Two Core Questions

When deciding automatically, ask:

1. How dangerous is this action?
2. Under what context is it dangerous?

Examples:
- `rm -rf /` -> always dangerous
- `git reset --hard` -> generally dangerous
- `git push -f` -> maybe safe in controlled rebase flow

---

# Risk Is Action + Context

Same command, different risk:

| Action | Context | Risk |
|---|---|---|
| `git push -f` | Protected main branch | High |
| `git push -f` | Personal rebase branch | Medium/Low |
| `cat` | Source code files | Low |
| `cat` | `.pem` / secret key files | High |

Permission policy must be context-aware, not command-only.

---

# Permission v0: Yolo Mode

- `--dangerously-allow-all-permissions` flag
- Or `--yolo` for short: You Only Live Once; true, you hand over your life to AI.
- A lightweighted work around:
  - No running permission when having access to network.
  - Having `--yolo` permission when offline following a user enforced plan.

---

# Permission v1: Rule-Based

Basic static policy:

- Allow read/write in current repo
- Allow read-only commands on allowed paths
- Deny obviously destructive commands
- Escalate unknown patterns to human

Auto-allow examples (inside repo):
- `ls`, `pwd`, `rg --files`, `cat src/parser.cc`
- `git status`, `git diff -- src/`, `git log --oneline -n 5`

Auto-deny examples:
- `rm -rf /`, `sudo ...`, `curl ... | sh`
- `cat ~/.ssh/id_rsa`, `git reset --hard`, `git push -f origin main`

---

# Permission v2: Intelligent Agent

Add a lightweight risk evaluator in permission hook.

- Input: command, target paths, git state, branch info
- Output: allow / deny / ask-human
- Model size: small model is enough for triage

```text
You are a command permission guard for a coding agent.
Return strict JSON only. No extra text.
Decision must be one of: ALLOW, DENY, ASK_HUMAN.

Below is the command and context:
```

Sometimes it gives me ALLOW, DENY, and ASK_HUMAN perfectly...
But sometimes, it gives me `**ALLOW**`, which is not valid for parsing...

---

# Permission v3: Scripted Orchestration

Move risky actions out of free-form agent behavior.

Pattern:
- Agent edits code and writes structured metadata
- Orchestrator performs guarded `commit`, `rebase`, `push`
- Human-equivalent checks happen in script layer

Key idea: constrain dangerous operations to deterministic pipelines.

---

# Permission v3: Sandbox

Alternative baseline:

- Put agent in an isolated sandbox
- Grant broad permissions inside sandbox
- Limit blast radius by environment boundary

This does not remove risk; it contains risk.

---

# A Practical Layered Architecture

Use all levels together:

0. `v0` --yolo mode
1. `v1` static rules for cheap filtering
2. `v2` intelligent hook for gray-zone decisions
3. `v3` orchestration for high-risk operations
4. `v4` sandbox for hard boundary

```text
Boundary + Rules + Intelligence + Orchestration
```

---

# Decision Flow Sketch

```bash
if in_sandbox_boundary; then
  if rule_engine_allows "$action"; then
    run "$action"
  else
    decision=$(risk_agent "$action" "$context")
    [ "$decision" = "allow" ] && run "$action" || request_human
  fi
else
  deny_and_alert
fi
```

---

# In-Class Practice

Build a minimal permission controller:

1. Define allow/deny rules for repo operations
2. Add one hook to intercept command execution
3. Classify actions by risk level
4. Route high-risk actions to manual approval
5. Log every decision for audit

Deliverable: one policy file + one hook script.

---

# Takeaways

- SDD controls *what* should happen
- Ralph Loop controls *how long* execution continues
- Permission system controls *what is allowed* at each step
- Fully hands-off needs all three layers together
