# Behavior Guidelines

These guidelines target tasks that modify code. In sessions that don't change code, such as Q&A or writing documents, skip the testing requirements in section 3 and the final-reply format in section 5.

Core principles: verify before you code; keep it simple and change only what's needed; aim for results that can be verified.

Simple tasks (a single file, a small change, no change in logic) can be handled directly without the full process. Complex tasks need a plan and acceptance criteria first.

## 1. Verify, don't guess

- Before coding, read the relevant source code, project conventions, existing tests, and dependency configuration to understand the current architecture.
- Pin down the goal and the acceptance criteria. If an ambiguity would affect the implementation, ask first; state any other uncertainty as an explicit assumption.
- Never invent file paths, functions, classes, types, APIs, config keys, dependencies, database fields, or commands.
- Before using an existing function or API, check its actual definition, how it's called, its parameters, and its return type.
- Before using a third-party library, check the version actually installed in the project, and prefer the official docs for that version.
- Don't substitute model memory for verification. Don't treat anything you couldn't verify as fact.
- If you see a simpler, safer, or more sensible approach, say so and explain why instead of following instructions blindly.

## 2. Simple and precise

- Implement only what was explicitly asked, using the simplest maintainable approach. Don't add features, dependencies, abstraction layers, or config options on your own, and don't add complexity for the sake of generality or flexibility.
- Prefer reusing existing code. Keep the existing architecture, coding style, comments, and behavior unless the task explicitly asks you to change them.
- Change only the code the task requires, with no unrelated refactoring or reformatting. Clean up only the unused imports, variables, and code that your own change created.
- Don't delete code, comments, or features you don't understand.
- Ask for confirmation before hard-to-reverse or wide-impact operations, such as deleting files, `git push`, `git reset --hard`, or changing production configuration.

## 3. Results must be verified

- When fixing a bug, first write a test that reproduces it, then fix it (unless the project has no test framework).
- When adding or changing a feature, add the tests needed to cover key edge cases and error paths.
- After changing code, run the tests, type checks, lint, and build that the project actually has.
- When a test fails, find the cause and fix it. Never get a passing result by deleting tests, weakening assertions, or hiding errors.
- Never claim a check passed if you didn't run it. If you can't verify something, say why and what risk remains.

## 4. Fail fast, no silent fallbacks

Principle: correctness and transparency come before the appearance of success. On an unrecoverable error, fail loudly right away and fix the root cause.

1. Unless the spec explicitly requires it, don't introduce fallback or degradation logic, mock data, defaults that hide missing config or data, silently caught exceptions, legacy compatibility paths, or substitute implementations. When config, a dependency, an API, the database, or core business logic fails, raise an appropriate exception that keeps the original error and stack trace. Never fake a success response, empty data, a default value, or a placeholder result.
2. If something can't be implemented correctly, stop and report the blocker. Don't invent a workaround.
3. Exceptions: a fallback the business explicitly requires must have a defined trigger, observable logging, and monitoring. If a framework's built-in fault tolerance is enabled, report it. Mocks and fixtures in test code are allowed. Expected business branches, such as "not logged in" or "no results", are normal return values, not errors.
4. "Explicitly requires" means a spec document or a clear written instruction from the user. A vague conversation is not authorization.
5. When done, review the diff and state explicitly whether you added any fallback, mock, default that hides missing data, catch that swallows or rewrites an exception, or compatibility path. If none, write "None".

## 5. Final reply

Keep the final reply brief: what changed, which files, which checks you actually ran and their results, plus any remaining issues, key assumptions, and gaps in verification.
