# Part 1: From Workflows to CLI

- CLI: Command Line Interface
- Agents prefer CLI over GUI
  - CLI only synthesizes the command token by token, e.g. `ls -l ~` or `git commit -m "message"`
    - Even unknown new CLI can be easily taught to agents using skills
  - While GUI is tricky to parse and understand
    - Screenshot the image and OCR the text
    - Send click to buttons
    - GUI frontend frameworks are highly diversified and platform-specific
      - e.g. Python: Tkinter
      - e.g. JavaScript: React, Vue, Angular
      - e.g. MacOS: SwiftUI, Cocoa
- We do have some frameworks for GUI test automation, e.g. Playwright for Web-based applications
  - But it is specific to Web-based applications
  - You still need to describe your desired behavior carefully to the agent

=> As long as we have CLI + skills for the CLI + a good AI-assistant driver, we can achieve a lot of things without OpenClaw.
=> I mean, OpenClaw is an overkill: I do not know what it can achieve, but I do not it can achieve too much for me.
=> Think about this: how many buttons do you click in Work/PowerPoint/Excel every day? how many % do they cover the total functionalities of the sw? I guess 3-5%. Similar thing for OpenClaw.
=> We need to change our mindset in LLM era: we should ``vibe'' our own tools/workflows instead of using others.

# Part 2: Intermediate Representation

- Thread: What is thread?
  - Email thread.
  - Chat thread.
  - In computer science, a thread of instructions that are executed in a sequence.
  - How is each instruction encoded?
- RISCV: 32-bit instructions (in a flatten format, I need you draw a text-based diagram for me)
  - 7 bits for opcode
  - 5 bits for source register 1 (rs1)
  - 5 bits for source register 2 (rs2)
  - 5 bits for destination register (rd)
  - 12 bits for immediate value (imm)
- Each thread has an program counter (PC) that points to the current instruction being executed.
- PC = PC + 4
- PC = PC + imm when branching

- Now you know what is a thread, and how assembly works.
- Let's talk more about IR, IR are CFG -- Control flow graph.
- Each graph is composed by nodes and edges.
- Node -> basic block, a sequence of instructions starting with a destination of a branch, and ending with a branch instruction.
- Edge -> control flow, the branch instructions.
- If-statement, when it comes to condition true, go to the then block, otherwise go to the else block.
- Loop, when the condition is true, go back to the loop head, otherwise exit the loop.
  - Break: go to the loop exit.
  - Continue: go to the loop condition check.
  - The branch goes back to the loop head is called latch