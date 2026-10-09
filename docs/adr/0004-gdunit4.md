# ADR-0004: gdUnit4 as test framework

- **Status:** Accepted
- **Date:** 2026-10-09
- **Deciders:** Gaël

## Context

The rules engine, card data, AI and save migrations need automated tests that run in the editor and headless in CI ([technical/09-testing.md](../technical/09-testing.md)).

## Decision

Use **gdUnit4**, installed as an addon in `game/addons/gdUnit4/`, with a version compatible with Godot 4.7.2, pinned.

## Alternatives considered

| Option | Pros | Cons |
|--------|------|------|
| **gdUnit4** | Rich assertions, mocking/spying, scene runner, VS Code integration, CLI runner with reports | More features to learn |
| GUT | Simple, long-established | Fewer built-in mocking/assertion features |

## Consequences

- Test suites extend `GdUnitTestSuite`, files named `test_*.gd` under `game/tests/`.
- CI runs the gdUnit4 command-line runner headless and publishes its reports.
- `game/addons/` is excluded from linting.
