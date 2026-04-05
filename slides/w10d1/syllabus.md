# Intermediate Representation

IR = CFG + 3-address code.

- Why do we need IR?
  - For NxM: where N is the number of source code languages and M is the number of target machines. We do not want N*M compilers, we write N frontends to IR and M backends from IR to machine code.
  - (We need a mermaid diagram for this above here)
  - For optimization: we can do macine indepedent optimizations on IR, and then generate efficient machine code.

- Why does IR look like this?
  - Recap: What do we have for assembly code?
  - Use w8d1, RISCV encoding example for a recap.
  - Each instruction has three operands: dest, src1, src2/imm --- 3-address code.
  - Each branch has a destination --- control flow graph.

- Functional programming languages, e.g. Haskell, OCaml, F#, etc.
  - No mutable state, no side effects.
  - Only defines, no mutations.

- SSA: Static Single Assignment form.
  - Each variable is assigned exactly once.
  - Each variable is defined before it is used.
  - This makes it easier to do optimizations, e.g. constant propagation, dead code elimination, etc.

```
x = a + b
x = x * 2
```

```
x1 = a + b
x2 = x1 * 2
```