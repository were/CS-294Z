# Workflows

1. What is a workflow?
   - SDD itself is a workflow: I want to write code, but I need to follow this flow.
     - Design the interface.
     - Design the test cases.
     - Write the code until all test cases pass.
     - Make the steps above a plan to execute.

2. Workflow is something like a pseudo-script, or soft script.
   - What is script? What is program?
   - TV program: a sequence of agenda to be executed and presented.
   - Program script: how each character in the program should act.
   - Computer program/script: a sequence of instructions to be executed by the computer.
     - Program: C/C++/Java/Rust/Go
     - Script: Bash/Python/JS/Perl
   - Key: a sequence of steps to do.
   - Good: a handy routine you do not need to write it every time.
   - Bad: a routine that is too rigid and does not fit the unexpected situation.

3. Agent workflow: write a sequence of steps for an AI agent to follow.
   - Make a plan
   - Step 1: Understand the goal of the task.
   - Step 2: Read thru the codebase to find the relevant code units.
   - Step 3: Document your modifications to the interfaces.
   - Step 4: Design test cases for the modified interfaces.
   - Step 5: Write code to implement the modified interfaces until all test cases pass.

   - Step A: Write the above steps to a plan A.
   - Step B: Make another plan B, but on different perspective, e.g. more aggressive on code refactoring.
   - Step C: Make another plan C, but on different perspective, e.g. more conservative on code refactoring.
   - Step D:Look at all three plans, and combine them to make a better unified plan for final implementation.

4. Findings
   - Step 2 code base understanding requires intelligence.
   - Step 2 output is common across A/B/C
   - Feeding A/B/C with the same step 2 will be good.
   - Feeding A/B/C output to step D requires no intelligence, but step D itself requires intelligence.

5. Claude Code has the capability to execute a natural langauge as a script/workflow, but not all LLMs.
   - I do not know. But maybe I only 10% chance to have this flow style execution successful in Cursor's auto dispatch.
   - Maybe 90% not Claude, or maybe 90% LLMs do not have this capability.

6. We shall seek a separation among human, ai, and code coordination.
   - Human: make the intention.
   - AI: make the plan, and execute the plan.
   - Code: coordinate across AI to schedule invocations. e.g. daemon to monitor the execution, and dispatch the next step when the previous step is done.