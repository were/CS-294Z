# Harness-Driven Compiler Interface

In the previous sessions, we kept emphasizing the same engineering lesson:
automation only works when the interface is explicit.

A compiler is not just "some code that works on my machine".
It is a tool in a pipeline:
- humans may run it manually,
- AI agents may drive it in a loop,
- test harnesses may feed it hundreds of cases.

So by `w07d2`, the compiler must expose a stable command-line contract.
We should define this contract now, instead of letting each harness guess how to call the compiler.

## Core Rule

Treat the compiler as a **non-interactive Unix filter**.

That means:
- read source code from `stdin`,
- write machine output to `stdout`,
- write diagnostics to `stderr`,
- report success or failure through the process exit code,
- avoid prompts, banners, REPL-style interaction, and unnecessary colored output.

If a human can drive the compiler with shell redirection, then a harness can drive it too.

## Required Semantic Interface

For semantic checking, require a mode like:

```bash
./build/rcompiler --check < program.rx
```

Expected behavior:
- input: source code from `stdin`,
- output: nothing important on `stdout`,
- diagnostics: semantic or compile errors on `stderr`,
- exit code `0`: program is accepted,
- non-zero exit code: program is rejected.

This is enough to drive `semantic-1` and `semantic-2`, because these harnesses mainly care whether compilation succeeds or fails.

## Required IR Interface

For code generation, require a mode like:

```bash
./build/rcompiler --emit-llvm < program.rx > output.ll
```

Expected behavior:
- input: source code from `stdin`,
- output: LLVM IR only on `stdout`,
- diagnostics: compile errors on `stderr`,
- exit code `0`: IR generated successfully,
- non-zero exit code: compilation failed.

The important constraint is that `stdout` must contain only the generated IR in this mode.
Otherwise a downstream tool such as `clang` or a grading harness cannot consume it reliably.

## Why We Should Not Standardize on `make run`

Some test repositories may use a wrapper like `make run`.
That is fine for a local adapter script, but it should **not** be the compiler contract.

`make run` is a harness-side convenience.
The actual compiler contract should stay at the CLI level:
- one executable,
- explicit mode flags,
- standard streams,
- standard exit codes.

Then we can always write a thin wrapper around the compiler,
but we do not have to redesign the compiler whenever the harness changes.

## Minimal Contract to Tell Students

By `w07d2`, your compiler should support at least:

```bash
./build/rcompiler --check
./build/rcompiler --emit-llvm
```

Both modes must:
- read source from `stdin`,
- use exit codes for success/failure,
- print errors to `stderr`,
- behave non-interactively.

This is the smallest clean interface that supports:
- manual testing,
- agentic automation,
- semantic harnesses,
- later IR harnesses.

This is exactly the kind of spec-first interface design we have been arguing for in the earlier weeks:
do not optimize for one ad hoc script;
define the contract first, then let tools and harnesses build on top of it.


# Multi-pass Semantic Check


Historically, C is a single pass language, as it can only do one pass for the code scanning.
When disks were small and memory was tight, we need to insert in disks of programs one by one.
Users prefer to insert the disks in order, so that the compiler can do a one-pass compilation.
However, this significantly limits the code organization --- where should I declare the function?
Where should I declare the struct?
Where should I declare the global variable?
Where should I implement all of them?

In modern language.
I guess the symbol of being modern is that we no longer worry about memory and we become luxurious on it,
e.g. Java and JS. These managed langauges are memory monsters, but they are also more flexible and resilient
to code organizations.

We no longer have declare first and implement it somewhere else.
Implementation itself is also a declaration.

Then we talk about how to do the multi-pass semantic check.
The first pass gather the function and struct table, and the 2nd pass do the variable declaration and type checking.