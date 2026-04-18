# From Vibe Coding to Spec Coding

This semester I am teaching CS294Z - Agentic Programming for Compilation.
This class is aimed at teaching students how to write code and manage
thousands of lines of code using agents.

---

What is vibe coding?

I know an average usage of AI programming is that:
1. You pop up an AI agent, and start a session.
2. You tell AI what to do, and wait for AI write code for you.
3. Maybe, AI does something stupid, and you interrupt it, and provide additional feedback to continue.
4. Maybe, you just do not care and do somewhere else, say YouTube, or TikTok, and forget AI when slacking.

---

What is vibe? Vibe is the atmosphere. Vibe is the feeling.
It is just like my kid. I want something, but I do not know what I want.
I just want to give me something, that is exactly what I want.

So, I feel like vibe is an abused word. Let's be more specific:
vibe means you do not exactly know what you need to modify,
but you at least clearly know what you want to achieve.

---

At the beginning of this semester, I was not a fan of vibe coding.
Or at least, I was against the idea of vibe coding, but now I changed
my mind. Vibe coding and spec coding are mutually exclusive.
They are targeting different area of applicability.

specifically if you are just writing some code and not for long-term maintenance and also that is in single file it is good to use the Vibe coding it is good to you know program using that style.

---

But if you are writing code for long term maintenance,
that is across multiple files, then you should do spec coding.
So I'm expecting to tell you how to write a code using AI in a more systematic way.
By spec, I mean specification and standard.
You need to clearly specify how to maintain the codebase in a good way.

---

AI agent is giving you a team of "you".
Before, you just program on your own, so you just program the "kernel"s.
Now, you have a team of "you", so manage the code just like you have a team.
Documentation, testing, and functionality are all important.
- Documentation is important for a long-term maintenance. LLM understands natural language better than code.
- When developing a codebase, you do not need to guarantee a 100% correctness, but test cases allow you to quickly check if newly developed code is not breaking the old code.
- Functionality kernel is of course the core of the codebase, but they only occupy maybe 30% of the codebase.

My experience is telling me that test case is often 1:1 loc compared to the functionality.
Maybe document is also 1:1, so functionality is only 1/3 of the codebase.

---

Question: How can we achieve this spec coding then? How can we inject a mindset of spec coding
into AI agents?

`AGENTS.md`/`CLAUDE.md`: These are readme files for AI to read when entering the codebase
with the highest priority. They are like the consititution/charter of the codebase.
You need to write them in a way that is clear and concise, ask AI to maintain the codebase
in the spec way: documentation, testing, and functionality.

---

Does your manager read your code? If not, how do you know what you are doing and track the progress?
You should also have a similar way. You should have a folder of documentations to track the
architecture of the codebase, the design decisions, the development roadmap, and progress.
I am recommending you to have a `docs` folder at the root of the codebase.
No need to be fancy, just a simple set of `md` files will suffice.
Remember also to have `docs` mentioned in your `AGENTS.md`/`CLAUDE.md`, so that AI can also read the documentation and understand the codebase better.

---

Testing is important. What I am suggesting is to have a TDD (Test Driven Development) style of coding. You write test cases before you write the functionality code.
Have all your interface defined in the document, have empty functions defined in the codebase,
have all the test cases implemented first. Then, what you do is to take advantage of
LLM's persistency to have the goal achieved.

---

When it comes to persistency, do you know what is Ralph loop?
It basically takes advantage of 2 LLM properties:
1. LLM can figure out the current state of the codebase to work towards your final goal.
2. LLM keeps working on it until the goal is achieved.

-> You just keep feeding the same thing in until LLM tells you "DONE".

---

Next, we talked about AGENTS.md/CLAUDE.md, what are the remaining configurations
and what are they for?

- `rules` - it is some charter that is weaker than `AGENTS.md`/`CLAUDE.md`, but it is more specific to the current task. It is like the code of conduct for the current task by reading the `description` brief of the specific rule.
- `prompts` - it works like `functions` in the LLM coding. For most of your frequently used prompts, you can standardize them in prompts, and prompts will be rendered by string substitution, so you can just fill in the blanks and use them. It is like a function call, but for prompts.
- `skills` - before, I would like to say they are `implicit` functions, or macros, but this definition is not that good.

---

Many people say `skills` are specifications. What are specifications?
You bought something from Amazon, but you do not know how to use it, so you read the specification.
Specification is a document of few-shot learning.

For example, everyone knows how to use `git`, `python`, `gcc` commandline tools, because they are widely used, and they are builtin knowledge for the LLM. But if you have some customized tools that are too new. LLMs still know how to use command line commands, but they do not know how to use your customized tools. They need some specifications.

---

What is few shot learning?

- A joke is Rust is the best language for LLM era.
- Why?
- Because they ask to use Rust to rewrite all the existing project.
- By rewriting we mean we already have a reference code base, and we just ask Rust to convert it.
- LLM is good at referencing something to create something. Such references are few shots.

---

Why does that matter? This is my daily workflow:
1. I have a Google Workspace CLI tool installed to manage my KAUST email, google drive, calendar, and TODO list.
2. But Workspace CLI is too new, so LLM does not know how to use it. Luckily, google officially provides skill set for this.
3. What I do nowadays is just I want to have an email drafted to my colleague, I want to have an reimbursement request created, or something. All these chores are handled by LLM.


This is vibe coding. I am asking LLM to do something in short term for me.

---

To conclude, I think vibe coding and spec coding are both important.
They are targeting different area of applicability.