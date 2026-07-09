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
- **Simplicity First**: Make every change as simple as possible. Impact minimal code.
- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards.
- **Minimal Impact**: Changes should only touch what's necessary. Avoid introducing bugs.

## Commands

Always use `.venv/Scripts/python` instead of `python` to ensure the venv interpreter is used.

```bash
# Run all tests
.venv/Scripts/python -m pytest tests/ -q

# Run a single test file
.venv/Scripts/python -m pytest tests/test_schedule.py -q

# Run a single test by name
.venv/Scripts/python -m pytest tests/test_schedule.py::test_function_name -q
```

## Git

Use Git following instructions in @git.instructions.md

## Working Conventions

- **Notebooks** — clear all cell outputs before committing; `nbstripout` git hook is configured to enforce this.
- **Subagent model selection** — when delegating to a subagent, pick the model deliberately. Use **Haiku** for mechanical work (file search, symbol lookup, cross-package sweeps, mass mechanical edits) — cost win, no cache penalty. Use **Opus** for the `Plan` subagent and other delegated reasoning tasks **when the goal is to keep the parent context clean** (e.g. parent is already heavy, or reasoning would pull in many large files). Plan inline by default — only delegate planning when context isolation is the actual reason. Default to the parent's model otherwise.

## Logging

Library code is configuration-free: every module declares its own logger and installs no handlers. Output is configured at the entry point (notebook, script, or test) by calling `setup_logging()` from the top-level `logging_config.py`, which loads `logging.yaml` via `logging.config.dictConfig`.
### Performance rule

### Channel split: `warnings.warn` vs `logger`

- **`warnings.warn(..., UserWarning)`** — user-facing data-quality signals the caller might want to suppress with `warnings.filterwarnings`. Example: `QuoteHierarchy` discarding a lower-priority quote on a maturity-date collision (`market_structures/rates/bootstrapper.py`).
- **`logger.warning(...)`** — operational/diagnostic concerns the caller does not need to suppress per-call. Example: bisection hit max iterations.

If unsure: would the user want to silence this with `filterwarnings` for a specific test or call? If yes, `warnings`. Otherwise `logger`.
