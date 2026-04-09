# A Quick Reading

- https://substack.com/home/post/p-193230263
- Story is quick, but the lesson is bitter.
- I just survived the deadline of SOSP last week on the day of last session.
  - This one did not go well.
  - They used OpenAI Prism and it seems ChatGPT did not generate good LaTeX.
  - As a tool and as a compiler, LaTeX is terrible when error happens.
  - Sometimes, it even runs into infinite loop and never finishes. (my guess is that they run into this issue, and at the last minute, they have to manually fix it, but they already did not know how to do it now.)
- Which induces additional points of this class:
  - We still need a sense/a muscle of engineering, even AI is already very well developed. In the prior class, I was too addicted to ``using AI systematically''.
  - When integrating AI to an existing tool, 95% percent maybe works fine, but the remaining 5% may cause a lot of trouble which takes longer even than 95% to fix.

# A Catch Up

Where were we last time?

`SSA` and `PHI` nodes.
- `SSA` is a form of IR, and now it is the de-facto standard for all compiler IRs.
- `PHI` is the key extension to enable `SSA` form when converging control flow.
- When should we inject `PHI` nodes?
  - Dominance frontier: where the power of dominance ends.
- The algorithm on Wikipedia is a little bit too general, but on compiler case:
  - I am not 100% sure, but in the RAW IR you generate, each node strictly has only at most two predecessors, which means a block with two predecessors must be a convergence of control, and thus potentially needs a `PHI` node.

Create a case of c code that accumulates an array of values.
I will respective compile it using `-O0` and `-O3` to see the difference.

# Build-up your IR

- LLVM IR builder.
- What is an insert point?
  - Where IR components are injected.
  - It is a pointer to an instruction.
- Topdown:
- How do we create a function
  - `llvm::Function::Create(type, linkage, name, module)`
  - `type` is a function type, which can be created by `llvm::FunctionType::get(return_type, arg_types, is_var_arg)`
- How to set an insert point?
  - `builder.SetInsertPoint(instruction)`
- How to create a basic block?
  - `llvm::BasicBlock::Create(context, name, function)`
- How do we create a basic block using IR builder?
  - `builder.CreateBlock(name, function)`
- How do we create an instruction without IR builder?
  - `llvm::BinaryOperator::Create(opcode, lhs, rhs, name, insert_point)`
- How do we create an instruction using IR builder?
  - `builder.CreateBinOp(opcode, lhs, rhs, name)`