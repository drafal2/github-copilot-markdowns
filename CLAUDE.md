## Stack
- Python 3.9
- T-SQL
- testing: pytest
- use ipython to run Python scripts

## Conventions
- Always write a code that is in line with community best programming conventions and standards
- Plan code that utilizes design patterns that are best-suited for a task
- Preserve object-oriented programming paradigm following core principles - **encapsulation**, **inheritance**, **polymorphism** and **abstraction**

## Naming
- **English language**
- Always use names for modules, classes and functions that describe what they do
- Use variable names that describe the object that is assigned to them

## Docstrings 
- **NumPy-style** with vertical signatures (one parameter per line for 2+ params beyond `self`) with type annotations present, do not repeat types in the `Parameters` section
- **ALWAYS** in Polish language
- Write in simple, clear, and unambiguous language
- Ensure all information, especially code snippets and technical details, is correct and up-to-date
- Always prioritize the user's goal. Every document must help a specific user achieve a specific task
- Maintain a consistent tone, terminology, and style across all documentation

## In-line comments
- **ALWAYS** in english language
- **DO NOT** comment obvious states
- Comment workarounds
- Explain complex logic
- Explain **WHY**, not what

## Project structure
- [**TODO**]

## Workflow Orchestration

### 1. Plan Mode Default
- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions)
- If something goes sideways, STOP and re-plan immediately - don't keep pushing
- Use plan mode for verification steps, not just building
- Write detailed specs upfront to reduce ambiguity
### 2. Subagent Strategy
- Use subagents liberally to keep main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One task per subagent for focused execution
### 3. Self-Improvement Loop
- After ANY correction from the user: update tasks/lessons.md with the pattern
- Write rules for yourself that prevent the same mistake
- Ruthlessly iterate on these lessons until mistake rate drops
- Review lessons at session start for relevant project
### 4. Verification Before Done
- Never mark a task complete without proving it works
- Diff behavior between main and your changes when relevant
- Ask yourself: "Would a staff engineer approve this?"
- Run tests, check logs, demonstrate correctness
### 5. Demand Elegance (Balanced)
- For non-trivial changes: pause and ask "is there a more elegant way?"
- If a fix feels hacky: "Knowing everything I know now, implement the elegant solution"
- Skip this for simple, obvious fixes - don't over-engineer
- Challenge your own work before presenting it
### 6. Autonomous Bug Fixing
- When given a bug report: just fix it. Don't ask for hand-holding
- Point at logs, errors, failing tests - then resolve them
- Zero context switching required from the user
- Go fix failing CI tests without being told how

### Task Management
- **Plan First**: Write plan to tasks/todo.md with checkable items
- **Verify Plan**: Check in before starting implementation
- **Track Progress**: Mark items complete as you go
- **Explain Changes**: High-level summary at each step
- **Document Results**: Add review section to tasks/todo.md
- **Capture Lessons**: Update tasks/lessons.md after corrections

### Core Principles
- **Simplicity First**: Make every change as simple as possible. Impact minimal code
- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards
- **Minimal Impact**: Changes should only touch what's necessary. Avoid introducing bugs
