# Testing

> Test strategy and tooling.
> Status: Draft · Last updated: 2026-10-08

## Framework

> **Open question:** [GUT](https://github.com/bitwes/Gut) or [gdUnit4](https://github.com/MikeSchulze/gdUnit4)? Both support Godot 4 and headless CLI runs. gdUnit4 has richer assertions/mocking and a VS Code extension; GUT is simpler and long-established. Pick one and record it in an ADR.

## Test pyramid

| Level | Scope | Location | Run |
|-------|-------|----------|-----|
| Unit | Effects, keywords, rules validation, save migration | `tests/unit/` | Every commit (CI) |
| Integration | Full scripted matches: seed + action list → expected final state | `tests/integration/` | Every commit (CI) |
| Card data | Validation of every `CardData` ([03-data-model](03-data-model.md#validation)) | `tests/unit/` | Every commit (CI) |
| AI soak | AI vs AI for N matches: no crash, no illegal move, no infinite loop; win-rate stats per deck | `tests/soak/` | Nightly / manual |
| Manual | UI, feel, device testing | Checklist below | Before each release |

## What must be tested

- Every keyword and every effect type has at least one unit test.
- Every rule in [functional/03-game-rules.md](../functional/03-game-rules.md) has a test named after its section.
- Each bug fix adds a regression test (often a recorded action list from the debug log).

## Determinism helpers

- Tests create matches with a fixed seed and stacked decks (`Match.create_for_test(deck_order_a, deck_order_b)`).
- A replay test re-applies a saved action list and compares the final state hash.

## Running headless

```sh
godot --headless --path game/ -s <test-runner-script> ...
```

(Exact command depends on the chosen framework — document it here once picked.)

## Manual test checklist (release)

- [ ] Install on reference device + one low-end device + one tall-aspect (20:9+) device
- [ ] Complete tutorial
- [ ] Play one match per difficulty
- [ ] Kill app mid-match / mid-save → profile intact
- [ ] Rotate / background / resume
- [ ] Back button on every screen
- [ ] Switch language to French → no text overflow
