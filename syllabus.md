## Week 1 Session 1: An Intro to Agentic Programming

- Teaching policies
- The history of ergonomic programming
- How should we use LLMs to help us code?

## Week 1 Session 2: Duty Separation in Effective Agentic Programming

- Recap: This is NOT a class of Vibe Coding!
- Then what is this class for?
  - Giving you a team of you for effective programming
  - "A team of you" means: AI is just as good as you are
  - But you can save the most bandwidth of communication

- Before: Each of the feature requst takes you a whole day to:
  - Implement
  - Test (Sure!)
  - Document (Did you even do this?)
- Now: If you get this planned properly, agents can do it for you maybe within half an hour.

- Follow up the [pain of agentic programming in last week](./slides/w1d1/slides.md)
- What is the root cause of the pain?
  - Human interaction
    - Bugs
    - Misunderstandings
    - Interrupting AI for corrections

- Proposed Solution: SDD, DDD, and TDD
    - Spec/Standard-driven Development (SDD)
    - Doc-driven Development (DDD)
    - Test-driven Development (TDD)

- Spec is the charter of the development
    - Spec of the development flow enforces AI to follow the flow
    - AI follows the sepc of development to read the spec of feature request to make the plan
    - AI follows the spec to conduct the plan

- What is the standard of the development?
    - Coding standard
    - Documentation standard
    - Testing standard
    - Standard: Write documentation first, write tests second, and write code last

- We did not 

- Doc-driven:
    - Update the documentation first
    - Documntation is for further reference, for better understanding
    - Which saves the CoT of understanding the code

- Spec driven:
    - Spec is both the charter of the 


## Week 2 Session 1: Bootstrapping your agentic automation!

- What is bootstrapping?
  - What does bootstrapping mean?
  - Boot the operating system
  - Bootstrap the compilation toolchain

- Good practices of software engineering (no matter LLMs)
  - LLM is just as good as you are
  - Plan first, implement later
  - Write tests first code later
  - Use version control correctly: fork branches, pr to merge back with code reviews

- What does agentic programming bootstrapping look like?
  - Bootstrapping your rules
  - Bootstrapping your skills
  - Bootstrapping your plans
  - Finally bootstrapping your automations

- Prompting to program
- Make good use of LLMs
  - `AGENTS.md` and `CLAUDE.md`
  - `rules/`: These have lower priority than the above two files, and are optionally loaded to the context.
  - `skills/`: These are the [newest standard](https://agentskills.io/) for formal micro flows.
  - `commands/`: These are for fixed flows.
  - `agents/`: These are for complicated flows that require thinking, OR independent context.

## Week 2 Session 2: Even more automatec

- Breaking down a GCC compiler
