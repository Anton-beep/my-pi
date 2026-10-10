---
name: writing-code-comments
description: Use whenever writing, editing, or refactoring source code in any language, to decide whether a code comment or docstring is needed at all and, if so, how to write it. This skill defines the narrow cases where a comment earns its place.
---

# Writing code comments

Comments are for **future readers of the code**, not for the person you are talking to right now. Most code needs no comments. A comment that restates the code, narrates your reasoning, or explains the obvious is noise: it costs reading time, goes stale, and hides the few comments that matter.

**Default: write no comment.** Add one only when it passes the test below.

## The test

Before writing any comment, ask:

> Would a competent developer, reading this code months from now **without access to this conversation**, be misled, confused, or likely to break something without this comment?

- If **no** → do not write it.
- If **yes**, first ask: can I remove the need for it with a better name, a smaller function, a named constant, or a type? If so, do that instead.
- Only if the answer is still yes → write a short comment.

## When a comment is justified

Comments explain **why**, not **what**. Write one when the code contains something the code itself cannot express:

- **A non-obvious reason**: a business rule, legal/regulatory requirement, or external constraint behind code that looks odd.
- **A workaround**: for a bug or quirk in a library, API, browser, or platform. Include the version or a link to the issue so the next person knows when it can be removed.
- **A trap**: code that looks wrong or simplifiable but isn't ("Order matters: X must run before Y because…"). This is the most valuable kind of comment.
- **An invariant or assumption** the code relies on but doesn't check.
- **A deliberate trade-off**: performance, security, or concurrency choices that a reader might "fix" back.
- **Magic values and complex expressions**: a regex, bitmask, formula, or constant whose meaning isn't readable.
- **Public API documentation**: docstrings on public functions/classes describing the contract (inputs, outputs, errors, side effects) — only where the project already uses them, and only what the signature and name don't already say.
- **An actionable TODO**: with a concrete condition or reference (`TODO: remove after v3 migration`), never a vague "improve later".

## What never goes in a comment

- **Restating the code**: `// increment counter`, `# loop over users`, `/** Gets the name. */` on `getName()`.
- **Your process or reasoning for this task**: "I changed this to use a map because…", "Updated to fix the bug", "As requested, …", "Now we handle the edge case". This belongs in your reply, the commit message, or the PR description.
- **Change history**: "Previously this used X", "Refactored from…", "New implementation". Version control already records history; the comment describes the code as it is now.
- **Messages to the reviewer or user**: "Note: you may want to…", "Feel free to adjust".
- **Long essays**: if explaining needs more than a few lines, the code likely needs restructuring, or the explanation belongs in a design doc/ADR/issue — link to it.
- **Section banners and decorative dividers** (`// ===== HELPERS =====`) unless the project already uses them.
- **Commented-out code**: delete it.
- **Type information** already expressed by types or signatures.

## How to write a comment that passes

- One or two lines. Plain, precise, present tense.
- State the reason or the consequence, not the mechanics: `// Retry once: the payment API returns 503 on cold start` rather than `// Retry the request if it fails`.
- Place it directly above the code it explains.
- Match the project's existing comment style and docstring conventions.

## When editing existing code

- If your change makes an existing comment wrong, **update or remove it**.
- Do not delete existing comments that still pass the test, even if you wouldn't have written them.
- Do not add comments to code you didn't otherwise touch.

## Before finishing

Review every comment you added or changed in this task and delete each one that fails the test. Expect to delete most of them. Anything you wanted to tell the user about *why you made a change* goes in your response to them, not in the code.

## Examples

Bad — restates the code:
```python
# Check if the user is active
if user.is_active:
```

Bad — narrates the agent's work:
```ts
// I refactored this to use reduce instead of a for loop for better readability
const total = items.reduce((sum, i) => sum + i.price, 0);
```

Bad — obvious docstring:
```python
def get_user_by_id(user_id: int) -> User:
    """Get a user by their ID.

    Args:
        user_id: The ID of the user.

    Returns:
        The user.
    """
```

Good — explains a trap:
```python
# Must close the cursor before commit: the driver deadlocks otherwise (psycopg2 < 2.9).
cursor.close()
conn.commit()
```

Good — explains a non-obvious rule:
```ts
// Invoices are rounded per line, not on the total, as required by German tax law (UStG §14).
const lines = items.map(i => round2(i.price * i.qty));
```

Good — no comment needed, the name does the work:
```python
MAX_LOGIN_ATTEMPTS_BEFORE_LOCKOUT = 5
```
