I want to talk about two things in this class:

1. Make good plans before every implementation task.
  - Why?
  - "Translating" a current state of the codebase to a desired state (development) is nearly same as translating the state to a plan to an LLM.
  - Plan is the center of human intervention in software development. A rule of thumb: do not stop the development until it is done.
  - Plan make the context focus on what to do next, not the code base.
  - Plan should follow the SDD, DDD, TDD principle in `w2d1` class.
  - Make a plan is all about what files to read to know what files to change. Thus, you should have somewhere to specify the project architecture.
  - Make a plan is about what to change. Thus, it is good to have the code snippets of the changes in the plan.
2. Execute a plan.
   - DO NOT expect the plan can be done in one iteration.
   - 
3. Hands on tutorial of writing a Rust Compiler.
   - Specify the class project scope.
   - Walk through a simple example of writing a Rust compiler.
   - How to break down the task of "lexer".
     - Lexer is simple, you can have it several brief sentences.
     - Parser is too complicated, so you need to break it down to several sub-tasks of the grammar rules.
     - It is nice to bottom-up your parsing process, until you get everything connected.

    3.1 What is Lexer, and how to understand Regex?
    3.2 Some basics on CMakeLists.txt --- a build system generator for a standard C++ project.
      -  What are we going to do to deliver code to others?
         - Executables
         - Libraries and Headers
    3.3 Desendent Recursive Parser
      - Each function corresponds to a grammar rule and returns the corresponding AST node.
    3.4 Parser is more complicated than Lexer
      - So it is recommended to have a bottom-up iteration until you get everything connected.
      - Start from the simplest grammar rule, and then gradually add more rules.
    3.5 Assignment 1: Implement the Lexer and Parser for a basic program.
