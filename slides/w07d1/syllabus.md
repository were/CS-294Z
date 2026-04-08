# Worktree

We want to run multiple sessions of the same repo to amortize the cost of waiting the AI assistant respond.

We used to have a branch checkout from master to develop. Then we have a PR to merge it back to master.
It is a common practice to have cooperation across multiple developers.

As I discussed in the very first lecture, AI is giving you a team of you.
So how can we cooperate with all these "you"s?

A trivial solution is to have mutiple clones of the same repository.
But it is not elegant.

Let's take a closer look at `git`. We have a `.git` directory.
Each `.git` directory is the true repository.
Each worktree is a branch of checkout out of the repository.
We normally have one worktree per `.git`.

We can use `git clone --bare` to only clone `.git` without the worktree.
Then we can use `git worktree add` to create multiple worktrees from the same repository.
Then we let each session work in each worktree added.

Plans are made at the `master` branch, and each session works on its own branch forked from `master`.

# Semantic Analysis

Only CORRECT programs can be compiled and executed.
We already enforced the syntactical correctness by using a parser.
Then we need to enforce the semantic correctness.

For example:

```rust
"a" + 1; // error: mismatched types
let x: int = "a"; // error: mismatched types
x(); // error: x is not a function
```

The key is to check the type of each symbol.
We need a symbol table to store the type of each symbol.
Then we can check the type of each symbol when we encounter it.

A symbol table is a stack of hash maps.

```rust
struct SymbolTable {
    stack: Vec<HashMap<String, Type>>,
}
```

When we enter a new scope, we push a new hash map to the stack.
When we exit a scope, we pop the hash map from the stack.
When we declare a new symbol, we insert it into the top hash map of the stack.
When we look up a symbol, we search from the top hash map to the bottom hash map of the stack.