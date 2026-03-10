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

# Agentic Programming: Worktree and Semantic Analysis
## Parallel Execution and Compiler Correctness

Jian Weng  
CEMSE, KAUST  
Week 7, Session 1

---

# Today's Agenda

1. Why one checkout is not enough for agent sessions
2. `git` internals: repository vs worktree
3. Practical multi-worktree workflow
4. Why parser is not enough for correctness
5. Symbol table stack for scoped type checking
6. Visitor pattern for semantic traversal
7. `for`/`while` with `break`/`continue` checking
8. In-class implementation task

---

# Problem: Many Sessions, One Repo

We want multiple AI sessions to work in parallel:

- One session for parser fix
- One session for semantic checker
- One session for tests/docs updates

Single checkout forces branch switching and context collisions.

---

# Naive Solution: Multiple Clones

You can clone the same repo many times:

- Works technically
- Heavy on disk and network
- Harder to manage and keep synchronized

Better approach: one repository store, many worktrees.

---

# Git Model Refresher

- `.git` directory stores repository metadata and history
- A worktree is one checked-out branch view
- Normal setup: one `.git` + one worktree

With `git worktree`, we create multiple checked-out branches from the same repository.

---

# Bootstrapping Worktrees

```bash
# 1) clone repository data only
git clone --bare <repo-url> compiler.git

# 2) create multiple working directories
git --git-dir=compiler.git worktree add main master
git --git-dir=compiler.git worktree add sema feature/sema
git --git-dir=compiler.git worktree add parser feature/parser
```

One history store, multiple active branches.

---

# Team-of-You Workflow

- Use `master` as planning/integration branch
- Fork task branches for each AI session
- Assign one worktree per session
- Merge back through normal review flow

```text
master -> plan
  |- feature/sema   (session A)
  |- feature/parser (session B)
  |- feature/tests  (session C)
```

---

# Operational Tips

- Name worktrees by task (`sema`, `parser`, `tests`)
- Keep each worktree on one branch only
- Run `git worktree list` frequently
- Remove finished worktrees to reduce clutter

```bash
git worktree list
git worktree remove sema
```

---

# Transition: Build Fast, But Build Correct

Worktree solves parallel development throughput.

Compiler still needs correctness gates:

- Syntax correctness (parser): already handled
- Semantic correctness (type/scope): next layer

---

# Why Semantic Analysis?

Only semantically correct programs should compile.

Parser ensures syntax shape, but cannot guarantee meaning.

Examples of semantic errors:

```rust
"a" + 1;            // mismatched types
let x: int = "a";   // mismatched types
x();                // x is not a function
```

---

# Core Question

At each expression or statement, we must know:

1. What symbol is this?
2. What type does it have?
3. Is this operation legal for that type?

This requires a scoped symbol table.

---

# Symbol Table as Stack

```rust
struct SymbolTable {
    stack: Vec<HashMap<String, Type>>,
}
```

- One hash map per lexical scope
- Top of stack is current scope
- Outer scopes remain searchable

---

# Scope Operations

When entering/exiting blocks:

```text
enter_scope()  -> push new map
exit_scope()   -> pop top map
declare(name)  -> insert into top map
lookup(name)   -> search from top to bottom
```

This naturally supports variable shadowing and nested blocks.

---

# Semantic Check Flow

```text
AST traversal:
  declaration -> record symbol + type
  identifier  -> lookup and validate existence
  call expr   -> ensure callee is function type
  binary expr -> enforce operator/type compatibility
```

Output: typed AST or explicit semantic errors.

---

# Why Visitor Pattern?

Semantic analysis needs to walk many AST node types:

- Expression nodes (`Binary`, `Call`, `Ident`, ...)
- Statement nodes (`If`, `While`, `For`, `Block`, ...)
- Declaration nodes (`Function`, `VarDecl`, ...)

Visitor pattern separates:
- AST data structure
- Per-pass logic (`TypeChecker`, `Linter`, `CodeGen`, ...)

---

# Visitor Skeleton (C++)

```cpp
struct ASTVisitor {
  virtual void visit(BinaryExpr& n) = 0;
  virtual void visit(CallExpr& n) = 0;
  virtual void visit(WhileStmt& n) = 0;
  virtual void visit(BreakStmt& n) = 0;
};

struct TypeChecker : ASTVisitor {
  SymbolTable symbols;
  void visit(BinaryExpr& n) override { /* type rules */ }
  void visit(WhileStmt& n) override { /* loop scope */ }
};
```

One traversal protocol, many analysis passes.

---

# Control-Flow Semantic Rule

`break` and `continue` are only legal inside loops.

```rust
break;        // error: outside loop
continue;     // error: outside loop
while cond {
  if x { continue; }   // ok
  break;               // ok
}
```

Parser can parse these forms, but semantic phase enforces context legality.

---

# Checking `for` / `while` / `break` / `continue`

Use loop-depth state during visitor traversal:

```cpp
int loopDepth = 0;

void visit(WhileStmt& n) {
  loopDepth++;
  visit(*n.body);
  loopDepth--;
}

void visit(BreakStmt&) {
  if (loopDepth == 0) error("break outside loop");
}
```

Same rule for `ContinueStmt`.

---

# Minimal Checker Skeleton

```cpp
Type checkExpr(Expr* e) {
  switch (e->kind) {
    case IDENT: return symbols.lookup(e->name);
    case ADD: {
      Type l = checkExpr(e->lhs);
      Type r = checkExpr(e->rhs);
      expectSame(l, r, "type mismatch in +");
      return l;
    }
    case CALL: return checkCall(e);
  }
}
```

---

# In-Class Practice

Implement a minimal semantic phase:

1. Add `SymbolTable` with scope push/pop
2. Register variable declarations with explicit type
3. Resolve identifiers through scoped lookup
4. Use visitor pattern for traversal
5. Report at least 4 semantic errors:
   - undeclared symbol
   - mismatched assignment type
   - call on non-function symbol
   - `break` / `continue` outside loop

Deliverable: checker pass + 4 failing tests.

---

# Takeaways

- `git worktree` enables parallel agent sessions without duplicate clones
- Plan on `master`, execute in isolated branch worktrees
- Parser checks syntax, semantic pass checks meaning
- Symbol table stack is the core mechanism for scope-aware type checking
