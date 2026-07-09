---
name: refactor-to-oop
description: 'Refactoring code from functional programming to object-oriented programming (OOP) without changing behaviour. Use only when user wants to rebuild the code to object-oriented programming paradigm without loss of code functionality.'
model: Claude Opus 4.8
tools: [**TODO**]
---

## Overview

Change code structure to follow object-oriented programming (OOP) principles when source code is written using functional programming paradigm. Aiming to evolution, not revolution.

## When to use

Use this skill when:
- user asks for refactoring code from functional programming into OOP framework
- code is easier to maintain in OOP form

## When NOT to use

Do NOT use this skill when:
- the code is structured in OOP framework 

---

## Refactoring principles

### The golden rules

1. **Behavior is preserved** - Refactoring doesn't change what the code does, only how
2. **Small steps** - Make small changes, test after each
3. **Version control is your friend** - Commit before and after each safe state
4. **Tests are essential** - Without tests, you're not refactoring, you're editing. If tests are missing, start with creating test suite to be confirmed with the user
5. **One thing at a time** - Don't mix refactoring with feature changes
6. **Preserve Don't Repeat Yourself principle** - If function or method might be reused - don't create new feature just use existing one

### When NOT to refactor
1. **Don't make changes just to do something** - When there is a function that is reuseble between classes, do not make it a class method, just leave it as a function

---

## Steps to follow when using skills, e.g. Refactoring steps

### Safe refactoring process

1. PREPARE
    - create feature from current branch
    - understand what code does
    - ensure tests exists, if not, create a test suite to be approved by the user
    - ensure current approach passes the tests

2. IDENTIFY
    - check which code fragments need refactoring
    - verify which functions might remain as is
    - plan the refactoring

3. REFACTOR
    - make small change
    - run relevant tests
    - commit if tests pass, otherwise fix the code
    - repeat

4. VERIFY:
    - all tests passses
    - output of the code is preserved

5. CLEAN UP
    - Update comments
    - Update documentation
    - Final commit

---

## Checklist to assess if the work is done

### Code quality

- [] Functions/methods follows Don't Repeat Yourself principle
- [] Functions/methods are small (max. around 50 lines)
- [] Descriptive names (variables, functions, classes)
- [] No magic numbers/strings
- [] Dead code removed

### Code structure

- [ ] Related code is together
- [ ] Clear module boundaries
- [ ] Dependencies flow in one direction
- [ ] No circular dependencies

### Docstrings

- [] Accurate and describe what code does
- [] Have vertical signatures with type annotations
- [] Written in Polish language
- [] Following NumPy-style docs

### Testing

- [ ] Refactored code is tested
- [ ] Tests cover edge cases
- [ ] All tests pass