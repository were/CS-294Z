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

# Agentic Programming: CLI and Intermediate Representation
## Text-First Workflows and Control-Flow Thinking

Jian Weng  
CEMSE, KAUST  
Week 8, Session 1

---

# Today's Agenda

1. Why agent workflows gravitate to CLI
2. CLI vs GUI for automation
3. Skills as adapters for new tools
4. Transition from command sequences to program execution
5. Thread, instruction encoding, and program counter
6. IR as CFG: basic blocks, branches, loops

---

# Why Agents Prefer CLI

- Agents operate on tokens naturally
- CLI already exposes actions as text commands
- Commands produce structured signals:
  - `stdout`
  - `stderr`
  - exit code
- CLI actions are easy to log, replay, diff, and script

```text
rg "parse_expr" src/
git diff -- src/parser.cc
./build/rcompiler --check < sample.rx
```

---

# CLI vs GUI

| CLI | GUI |
|---|---|
| explicit command grammar | visual interpretation |
| deterministic text output | screenshots / OCR / DOM |
| easy to script and replay | timing and click sensitivity |
| works well in ssh / CI / containers | often tied to one platform or framework |

For engineering workflows, text is usually the cheaper interface.

---

# GUI Automation Is Possible, But Costly

- GUI agents must understand layout, state, and focus
- Actions become spatial:
  - click
  - drag
  - scroll
  - type into the right widget
- UI stacks are fragmented:
  - Web: React / Vue / Angular
  - Python: Tkinter
  - MacOS: SwiftUI / Cocoa
- Small UI changes can break the flow

---

# CLI Is an Action Language

Think of the shell as a compact language of operations.

- Command name selects the tool
- Flags configure behavior
- Paths and arguments bind the target
- Pipes and redirection compose multiple tools

```bash
git status
rg --files slides
./build/rcompiler --check < tests/ok.rx
```

No pixels, no mouse coordinates, just tokens.

---

# Skills Make New CLI Teachability Cheap

Unknown CLI does not mean the agent is stuck.

Give the agent:
- a small cheatsheet
- common command templates
- path conventions
- safety rules

```text
Intent: run semantic tests
Command: ./scripts/run-semantic.sh semantic-2
Guardrail: never edit files under RCompiler-Testcases/
```

This is often enough to turn a repo-specific workflow into an agent-usable workflow.

---

# Practical Stack and Its Limits

```text
CLI + skill library + agent driver + permission policy
```

- CLI exposes operations
- Skills explain local conventions

GUI still matters when:
- the application has no CLI
- the task is fundamentally visual
- the target is a browser workflow

Example:
- Playwright is excellent for Web automation and testing

But for compilers, build systems, and repositories, start with CLI first.

---

# What Is a Thread?

- External workflow: command sequence
- Internal execution: instruction sequence
- Email thread: ordered messages
- Chat thread: ordered conversation
- CPU thread: ordered instructions being executed

In computer science, a thread is a sequence of instructions with an execution state.

Program counter points to the current instruction, and the CPU executes instructions one by one.

---

# Machine Instruction Intuition

- A machine instruction is a compact encoded record
- It tells the CPU:
  - what operation to perform
  - which operands to read
  - where to write the result
  - sometimes an immediate constant

RISC-V uses several 32-bit layouts, but the recurring idea is the same:

```text
opcode + register fields + optional immediate fields
```

---

# RISC-V Text Diagram

```text
R-type:
31        25 24   20 19   15 14 12 11    7 6      0
+-----------+-------+-------+-----+-------+--------+
| funct7    | rs2   | rs1   |f3   | rd    | opcode |
+-----------+-------+-------+-----+-------+--------+

I-type:
31            20 19   15 14 12 11    7 6      0
+---------------+-------+-----+-------+--------+
| imm[11:0]     | rs1   |f3   | rd    | opcode |
+---------------+-------+-----+-------+--------+
```

- `opcode`: what kind of instruction
- `rs1`, `rs2`: source registers
- `rd`: destination register
- `imm`: immediate constant embedded in the instruction

---

# Program Counter (PC)

```text
PC = address of the current instruction
```

Execution loop:

```text
fetch(PC) -> decode -> execute -> update PC
```

For straight-line execution in 32-bit RISC-V:

```text
PC = PC + 4
```

---

# Branches Create Control Flow

Straight-line code is easy:

```text
PC = PC + 4
```

Branching code is where structure appears:

```text
if condition is true:  PC = branch_target
else:                  PC = PC + 4
```

So the next instruction is no longer determined only by position in memory.

---

# Why We Need IR

- AST is close to source syntax
- Machine code is close to hardware details
- The compiler needs a middle form for analysis and transformation

One useful mental model for this course:

```text
source -> AST -> semantic check -> IR -> codegen
```

Here, IR is where control flow becomes explicit.

---

# IR as Control-Flow Graph

For this class, think of IR as a CFG.

- Graph nodes represent basic blocks
- Graph edges represent possible control transfers
- Splits and merges become explicit
- Loops become visible as back edges

```text
entry -> cond -> then -> merge
             \-> else -^
```

---

# What Is a Basic Block?

A basic block is a straight-line sequence of instructions with:

- one entry
- no internal branch target
- no internal branch out
- a control transfer only at the end

Typical boundaries:
- function entry
- branch target
- jump / branch / return instruction

---

# If-Statement as CFG

```text
        [cond]
        /    \
   true/      \false
     [then]  [else]
        \      /
         [merge]
```

- condition block decides which path to take
- then/else are separate blocks
- both paths rejoin at a merge block

This is much easier to analyze in a graph than in raw source text.

---

# Loop as CFG

```text
          true
      +----------+
      |          v
    [head] -> [body] -> [latch]
      |                 |
      +------false------+ 
              |
             [exit]
```

- `head`: evaluate loop condition
- `body`: loop body instructions
- `latch`: the block that jumps back to the head
- `exit`: leave the loop

---

# Break, Continue, and Latch

Inside a loop:

- `break` jumps to the loop exit
- `continue` jumps to the next loop check path
- the edge from `latch` back to `head` is the back edge

```text
break    -> exit
continue -> head
latch    -> head
```

These are explicit CFG edges, not just syntax sugar.

---

# In-Class Practice

1. Draw the CFG for one `if / else`
2. Draw the CFG for one `while` loop
3. Mark the basic block boundaries
4. Label the true / false / back edges
5. Add where `break` and `continue` should jump

Deliverable:

```text
one handwritten or text-drawn CFG for each construct
```

---

# Takeaways

- Agent-friendly workflows usually start with CLI, not GUI
- Skills make new command-line tools teachable to agents
- Program execution is a thread of encoded instructions
- PC and branches define control flow
- For this course, IR is best understood as a CFG over basic blocks
