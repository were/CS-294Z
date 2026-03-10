# Towards Fully Hands-off

## Recap SDD: Plan first, code later.
We only interact with the artifact: the plan, the develop**ed** code.
We want to eliminate human intervention in AI-assisted coding.

## Recap Ralph Loop
We want to eliminate further human intervention during the development process.
If not done, just feed the same prompt back again and again util it is done.

## What is missing for full automation?
Permission interuptions:
- Should I do this edit?
- Should I run this command?
- Should I read this file?

## Permission
Recognize what should we make the decisions:
1. How dangerous is the action?
   - `rm -rf /` is absolutely dangerous
   - `git reset --hard` is also dangerous
2. When is it dangerous?
   - `git push -f` can be safe as long as it is a branch rebase
   - `git commit -m` should be carefully monitored
   - `ls`, `cat` are generally safe, but can be dangerous if used in a wrong way (e.g., `passwd/.pem/.key`)

## Permission v1: Rule Based
- Allow all files in `/tmp` and in the current repo
- Allow all read only commands operating on allowed files
- Allow all the edits and writes in the current repo
- Deny all the obviously dangerous commands (e.g., `rm -rf /`)
- Maybe 20-30% of commands can be automatically allowed.

## Permission v2: Intelligent Agent
- Invoke a Haiku small-model agent to evaluate the risk of the command in permission hook.
  
## Permission v3: In Scripted Orchestration (See W5D1)
- Let the agent modify the code base, and dump a file for commit message.
- Commit is done in script orchestration, so it is equivalent to a human action.
- Similarly, we can build an orchtestrated `git push -f` for a rebase workflow.

## Permission v0: Sandbox
- Put the agent in a sandbox environment and give `yolo` permission within this box.