---
name: refactor
description: Refactor code to improve its structure and readability without changing behavior. Use when the user asks to refactor, clean up, simplify, or restructure code.
---


# Refactor

Refactoring changes how code is structured, never what it does. Every step must leave the code working and the behavior identical.

## Rules


- **Preserve behavior.** Same inputs give the same outputs, side effects, errors, and public API. Change a public signature only if the user asks for it.
- **Don't mix in other changes.** No new features, no bug fixes, no dependency upgrades. If you find a bug, leave it as it is and report it at the end.
- **Small steps.** Make one refactoring at a time and verify it before starting the next.
- **Stay in scope.** Touch only the code the user named. If they named none, ask before going codebase-wide.
- **Match the codebase.** Follow the existing style, naming, and patterns. Don't impose new ones.

## Workflow

### 1. Establish a baseline
- Find out how to run the tests, type checker, linter, and build (README, package scripts, Makefile, CI config).
- Run them before changing anything and note what already fails, so you don't blame those failures on your changes later.
- If the target code has no tests, write characterization tests first. These pin down current behavior, including any odd behavior. If you can't write them, tell the user the refactor is unverified and ask whether to continue.

### 2. Find what to improve
Look for common code smells and match each one to a standard refactoring (Martin Fowler's catalog):


| Smell | Refactoring |
|---|---|
| Long function | Extract Function |
| Duplicated code | Extract Function / Pull Up Method |
| Unclear names | Rename Variable / Function |
| Long parameter list | Introduce Parameter Object |
| Deep nesting | Replace Nested Conditional with Guard Clauses |
| Switch/if chains on type | Replace Conditional with Polymorphism |
| Magic numbers or strings | Replace Magic Literal with Named Constant |
| Dead code | Remove Dead Code (confirm it is unused first) |
| Large class or module | Extract Class / Move Function |
| Temp variable used once | Inline Variable |


Rank the findings by value and risk. Do the safe, high-value ones first.


### 3. Apply each change and verify it

For each refactoring:
1. Make the single change.

2. Run the tests (plus the type checker and linter, if the project has them).
3. If anything new fails, undo the change. Don't patch around the failure.

For long jobs, make a local commit after each green step so it's easy to roll back. Don't push and don't open pull requests.

### 4. Report
End with a short summary:
- what you changed and why, one line per refactoring
- test results before and after
- any bugs, risks, or larger refactors you noticed but left alone


