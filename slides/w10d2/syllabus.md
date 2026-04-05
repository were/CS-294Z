# SSA IR - 2

Dominance and Phi $\phi$ node.

````rust
y = 0
if x > 0 {
    y = 1
} else {
    y = 2
}
y = phi([y1, then], [y2, else])
````

- We inevitably need to assign a same value multiple times.
  - When branching, we need to assign y in both branches, and then they converge.
  - $\phi$ node is a special and virtual instruction that selects the value base on the control flow path taken.

`value = $\phi$([<values, blocks>])` if it comes from the corresponding block, then the corresponding value is selected.

# How phi is constructed?

Dominance: If going to a block B always passes through a block A, we say A dominates B.
In this case `y=0` dominates `y=1` and `y=2`.
Dominance frontier: If A dominates B, but A does not dominate all of B's successors, 
then B's successors are A's dominace frontier.

# How is SSA constructed in LLVM?

If you do `O0`, you can see that LLVM allocates a stack slot for each variable.
Then LLVM converts all stack slot load/store to "registers", and insert $\phi$ nodes when necessary.
This is a stack-level alias analysis.

# Build your IR

LLVM IRBuilder. A set of helper API functions for you to build your IR.
I guess LLVM IRBuilder is already in LLM 0-shot, but you can also
add some helper skills to make it more convenient for your IR builder generation.