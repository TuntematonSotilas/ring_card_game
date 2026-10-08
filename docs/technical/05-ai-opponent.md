# AI opponent

> How the computer opponent chooses its moves. Supports [functional/05-game-modes.md](../functional/05-game-modes.md).
> Status: Draft · Last updated: 2026-10-08

## Requirements

- Plays only legal moves (uses `Match.get_legal_actions()`).
- **Does not cheat**: no access to the human's hand or deck order (works on a state where hidden information is masked/randomised).
- Decides within **≤ 1 s** per action on a mid-range phone; runs off the main thread or spread across frames so the UI never freezes.
- Difficulty is tunable; campaign bosses can have personalities.

## Interface

```gdscript
class_name AIController
extends RefCounted

func _init(profile: AIProfile) -> void
func choose_action(state: GameState, legal: Array[Action]) -> Action
```

`AIProfile` (Resource): difficulty, evaluation weights, search depth, randomness, scripted overrides.

## Approach (incremental)

1. **v0 — Random legal**: for testing the engine.
2. **v1 — Greedy heuristic**: for each legal action, simulate on a copy of the state, score with an evaluation function, pick the best. Repeat until `EndTurn` is best.
3. **v2 — Turn planning**: search sequences of actions within the turn (beam search / limited depth), still with a heuristic evaluation.
4. *(Optional)* **v3 — Determinised MCTS** for Hard difficulty.

### Evaluation function (draft)

```
score = w_life   * (my_life - enemy_life)
      + w_board  * (sum my unit value - sum enemy unit value)
      + w_hand   * (my hand size - enemy hand size)
      + w_gold   * unused_gold_penalty
      + w_ring   * ring_value(turns_held)
      + lethal bonus / lethal-threat penalty
```

Unit value ≈ `attack + health` + keyword bonuses. Weights live in `AIProfile` so they can be tuned without code changes.

## Difficulty levels

| Level | Strategy | Noise |
|-------|----------|-------|
| Easy | Greedy, shallow, ignores some keywords | High random choice probability |
| Normal | Greedy with full evaluation | Low |
| Hard | Turn planning (+ MCTS if implemented) | None |

## Campaign personalities

`AIProfile` overrides per boss, e.g.:
- *Alberich*: prioritises Gold and the Ring.
- *Fafner*: defensive, keeps Guard units.
- *Hagen*: targets the strongest enemy unit.

## Threading

> **Open question:** Use `WorkerThreadPool` for the search (state must be fully copied — the core has no Nodes, so this is safe) vs. time-sliced search on the main thread. Start with time slicing; move to threads if needed.
