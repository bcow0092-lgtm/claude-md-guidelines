# CLAUDE.md Behavior Guidelines

[中文](README.md)

Behavior guidelines for AI coding assistants, ready to use as a `CLAUDE.md` for Claude Code. They ask the AI to verify before acting, change only the code a task needs, back claims with checks it actually ran, and fail loudly on errors instead of hiding them behind fallbacks, mocks, or default values.

## Files

| File | Description |
|------|-------------|
| [CLAUDE.md](CLAUDE.md) | Chinese version |
| [CLAUDE.en.md](CLAUDE.en.md) | English version, same content as the Chinese one |

## Usage

Pick a language version and put it in one of these places. If you use the English version, rename it to `CLAUDE.md`.

- **Global rules**, applied to every project: `~/.claude/CLAUDE.md` (on Windows, `%USERPROFILE%\.claude\CLAUDE.md`)
- **Project rules**, applied to one project only: `CLAUDE.md` in the project root

For tools that read `AGENTS.md` (such as Codex), rename the file to `AGENTS.md` and put it in the project root. Nothing in the guidelines is specific to Claude Code.

## Design notes

**Scope.** Global rules load in every session, including plain Q&A. The opening paragraph therefore limits the testing requirements and the final-reply format to tasks that change code, while rules like "never invent" and "don't treat unverified information as fact" apply everywhere.

**1. Verify, don't guess.** The most common AI failure is writing calls to functions, parameters, or config keys that don't exist, based on memory. This section requires reading the source, checking real definitions, and confirming the library version that's actually installed.

**2. Simple and precise.** Limits the scope of changes: no unrequested features or abstractions, no unrelated refactoring, no deleting code the AI doesn't understand, and confirmation before risky operations. The goal is a diff that contains only what the task needs, so it's easy to review.

**3. Results must be verified.** Bug fixes start with a test that reproduces the bug. A failing test gets its root cause fixed, never deleted or weakened. A check that wasn't run can't be reported as passing.

**4. Fail fast, no silent fallbacks.** This is the most distinctive part. To "make the code run", AI assistants often wrap code in try/except that swallows errors, fill in a default when config is missing, or return empty data when an API call fails. The code then looks fine while the real problem stays hidden. This section forbids those patterns and spells out the exceptions: fallbacks the business explicitly requires, mocks in test code, and normal business branches like "not logged in" or "no results". The last rule makes the AI review its diff and report whether it added any of these, so fallback logic can't slip in unnoticed.

**5. Final reply.** A fixed reporting format, so you can see at a glance what changed, what was verified, and what wasn't.

## Customizing

These guidelines are a general-purpose version. Add or remove rules to fit how you work. Common changes:

- **Fix the reply language**: add a section at the top, for example:

  ```markdown
  ## Language

  Always reply to the user in English. Follow the project's existing language for code comments and commit messages.
  ```

- **Using plugins such as superpowers**: the opening line "Simple tasks can be handled directly" takes precedence over mandatory workflows in plugin skills. If you want simple tasks to go through a plugin's workflow too (for example superpowers' brainstorming), reword that line to match.
- **Rules that duplicate a tool's defaults**: some rules in sections 1, 2, and 3 repeat Claude Code's built-in default behavior. If you only use Claude Code and want to save context, you can remove them. If you use several tools, keep them.

## License

[MIT](LICENSE)
