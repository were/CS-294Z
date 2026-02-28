# Ralph Loop Implementation

- Assuming we already have a plan as several classes before I asked you to make it.
- How can we effectively have such plan executed?
  - What if this plan is big AI may:
    - Not be able to finish it in one session,
    - It is just too long to avoid hallucination,
    - Or AI magically find some workaround undesired?
- Solution: Ralph Loop
  - Name? Because the Claude Code Plugin developer who first found this is named Ralph Wiggum

- Pun: This is the nature of Computer Science: When learning something, you often learn how to use it and how it works.
  - Use Ralph Loop to implement something **BIG**.
  - How Ralph Loop is implemented.

- What is Ralph Loop?
  - Input: Your prompt, your current state of the codebase, and the max iterations.
  - (Intermediate) Output: Modifications to the codebase.
  - How it loops: Your same prompt, and the updated codebase, current iteration + 1, until max iterations or task completion.
  - See? This is what something BIG.
  - Citation: https://arxiv.org/pdf/2512.24601 (RLM)
    - Feeding the same prompt again into the model gives better quality of results.
    - Implicitly, you are actually asking model to first figure out what to do next, as the only difference is the codebase state. Then have the plan executed.

- How is it used?
  - You can just use it in built-in KIMI, or you can install Claude Code Plugin

- How is it implemented?
  - We mainly discuss Claude Code Plugin implementation.
  - How many of you know what is Hook in Operating System?
    - When you do something, we hook it up to inject some of our own code to do something before or after (or even fully replace) the "something".
  - [Claude Code Hooking](https://code.claude.com/docs/en/hooks)
    - Stop: After session execution, and it asks you what to do next.
    - You write an embedded prompt to say "If you decided to stop, print a `DONE` on your output, otherwise print `CONTINUE`. Read our plan to decide what to do next.".

- For now, we have **BIG** thing solved by auto-continuation, but we still need to work against hallucination and undesired workaround.
- Insight: LLMs put a higher weight to the latest input.
  - Still, you should understand how a multip-round conversation works.
  - Dude, there is no multi-round, there is only one round you feed in the prior context and append your latest question. Then, it answers the question based on the whole context.
  - To my understandings, it is implying "Latest input has higher weight" to impact the answers.
- Solution: We open another session to read both the plan and the context and embed the created prompt to instruct the model what to do next.
- Hint: However, **DO NOT** trust AI coding ability too much, as they do not have too much built-in knowledge for the latest Claude Code Plugin documentation. Even few-shot does not help that much. Maybe write some of your own code manually first.

- <del>Finally, what if I do not like Claude</del>
- No, what if I want to make it framework-neutral?
  - You can implement your own Ralph Loop with the same idea of auto-continuation and embedded prompt.
- Write a shell script for this!

```
for i in 0..max_iter:
    run_session(plan, codebase)
```