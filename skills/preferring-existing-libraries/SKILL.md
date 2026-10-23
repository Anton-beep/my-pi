---
name: preferring-existing-libraries
description: Use before hand-writing a helper, parser, validator, retry or backoff loop, CLI flag handler, config loader, or utility, and when creating a new module, package, script, or repository. Triggers "can this be replaced with an existing tool", "why are we defining our own", "don't reinvent this", "we already have a library for that".
---


# Library-First Implementation

Before writing new utility, helper, parser, formatter, validator, algorithm, data structure, date/time logic, serialization logic, or other generic functionality:

1. Search the repository for an existing implementation.
2. Inspect package/dependency manifests for libraries that may already provide the functionality.
3. If a relevant library exists, inspect its API/source/documentation and use it rather than implementing equivalent functionality.
4. Prefer:
   a. existing project code
   b. existing project dependencies
   c. a well-established third-party library
   d. new implementation

Examples of functionality that should trigger this check:
- date/time manipulation
- parsing
- validation
- retry/backoff
- caching
- HTTP clients
- URL/path manipulation
- UUIDs
- hashing/encoding
- serialization/deserialization
- CLI argument parsing
- logging
- concurrency primitives
- collection utilities
- text processing
- cryptography
- database utilities

If you decide not to reuse an existing implementation or library,
state the reason briefly before proceeding.

Never add a dependency solely to replace a trivial 2-line operation.
Use engineering judgment regarding dependency cost.
