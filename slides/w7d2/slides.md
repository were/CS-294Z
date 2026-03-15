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

# Agentic Programming: Harness-Driven Compiler
## End-to-End Interface and Semantic Check

Jian Weng  
CEMSE, KAUST  
Week 7, Session 2

---

# Today's Agenda

1. What is a system?
2. Top-down requirement vs bottom-up implementation
3. What semantic interface should the compiler expose now?
4. `semantic-1` and `semantic-2` are already ready
5. Why semantic checking should be multi-pass
6. What pass 1 and pass 2 each do

---

# What Is a System?

For this lecture, a system is **one end to the other**.

```text
source program
  -> compiler interface
  -> compiler result
  -> test harness decision
```

If this whole path works, then the system works.

If each component works alone but the ends cannot connect,
then the system is still not ready.

---

# Top-Down and Bottom-Up

We need both, but they answer different questions.

**Top-down** asks:
- from the end-to-end system view,
- what interface must the compiler expose to the outside?

**Bottom-up** asks:
- given the parser, AST, and semantic checker,
- how do we connect them until that interface actually works?

So:
- top-down defines the requirement
- bottom-up realizes it incrementally

---

# Top-Down: What Interface Do We Need Now?

For the current semantic stage, the contract can be very small:

```bash
./build/rcompiler --check < program.rx
```

Required behavior:

- read source code from `stdin`
- print diagnostics to `stderr`
- use exit code `0` for accept
- use non-zero exit code for reject

Once this interface is stable, the harness can test it.

---

# Bottom-Up: How Do We Get There?

Implementation does not have to appear all at once.

We connect the pieces step by step:

```text
stdin
  -> lexer / parser
  -> AST
  -> semantic checker
  -> stderr + exit code
```

This is bottom-up implementation toward a top-down contract.

The outside only sees `--check`.
The inside can evolve incrementally until that contract works.

---

# Semantic Test Cases Are Already Ready

We do **not** need to invent new semantic tests right now.

Just use the existing suites:

- `RCompiler-Testcases/semantic-1`
- `RCompiler-Testcases/semantic-2`

At this stage, they mainly care about one thing:

- should this program be accepted?
- should this program be rejected?

That is enough for current end-to-end testing.

---

# Why Semantic Check Needs Another Pass

Parser gives us structure.
Semantic checking needs more global knowledge.

If we force everything into one pass, code organization becomes rigid:

- where should the function be declared?
- where should the struct be declared?
- where should globals be declared?
- where should implementation be placed?

This is exactly why one-pass design tends to constrain the source layout.

---

# Multi-Pass Semantic Check

A cleaner approach is:

```text
Pass 1: gather global declarations
Pass 2: check bodies, locals, and types
```

**Pass 1** can collect:
- function names and signatures
- struct names and layouts
- global declarations

**Pass 2** can then do:
- variable declaration checking
- identifier resolution
- expression type checking

---

# Why This Is REQIURED

With a multi-pass semantic check:

- implementation can also serve as declaration
- forward use becomes easier to support
- code organization is less constrained
- semantic rules become clearer to structure

So the compiler becomes more modern not because the language is magical,
but because the compiler can afford more complete program knowledge.

---

# Simple Example

```rust
fn main() {
  answer();
}

fn answer() -> int {
  return 42;
}
```

In a multi-pass design:

- pass 1 records `answer`
- pass 2 checks the call inside `main`

This is the core idea of the semantic check part.

---

# In-Class Target

Today's target is:

1. expose `./build/rcompiler --check`
2. make the compiler testable as an end-to-end system
3. run it against `semantic-1`
4. run it against `semantic-2`
5. structure semantic checking as multi-pass

IR interface and IR tests are for later.

---

# Takeaways

- A system is the whole end-to-end path
- Top-down defines the interface requirement
- Bottom-up is how we connect code to satisfy it
- `semantic-1` and `semantic-2` are ready now
- Multi-pass semantic check is the right next internal structure
