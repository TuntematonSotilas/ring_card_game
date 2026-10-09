# Coding standards

> GDScript conventions for the project. Base: the official [GDScript style guide](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_styleguide.html).
> Status: Draft · Last updated: 2026-10-09

## Static typing — mandatory

```gdscript
var life: int = 20
var hand: Array[CardInstance] = []
func deal_damage(target: CardInstance, amount: int) -> void:
```

Enable in Project Settings → `debug/gdscript/warnings`:
- `untyped_declaration` = Error
- `inferred_declaration` = Warning (`:=` allowed only when the type is obvious)
- `unsafe_*` = Warning

## Naming

| Element | Style | Example |
|---------|-------|---------|
| Class / `class_name` | PascalCase | `EffectResolver` |
| Function, variable | snake_case | `get_legal_actions()` |
| Private member | leading `_` | `_queue` |
| Constant, enum value | UPPER_SNAKE | `MAX_HAND_SIZE`, `Type.UNIT` |
| Signal | past tense, snake_case | `card_played`, `turn_ended` |
| Signal handler | `_on_<emitter>_<signal>` | `_on_match_event_emitted` |
| Bool | `is_`/`has_`/`can_` prefix | `is_exhausted` |

## File layout (order inside a script)

1. `class_name`, `extends`, `##` class doc comment
2. signals
3. enums, constants
4. `@export` vars, public vars, private vars, `@onready` vars
5. `_init`, `_ready`, other built-in virtuals
6. public methods
7. private methods

## Rules for `core/`

- `extends RefCounted` or `Resource` only — never `Node`.
- No `get_node`, no autoload access (except a passed-in `CardDatabase`), no `await`, no global `randi()`.
- Functions that mutate state emit the corresponding `GameEvent`.

## Rules for scenes

- "Call down, signal up": parents call children's methods; children emit signals.
- Use `%UniqueName` for node references in a scene instead of long paths.
- No game logic in views.

## Documentation

- `##` doc comments on every public class and method (they show in the Godot editor help).
- Magic numbers belong in constants or in data (`Resource`s).

## Git

- Branches: `feature/<topic>`, `fix/<topic>`; PRs into `main`.
- Commit messages: imperative, short (`Add Guard keyword handler`).
- Commit `.tscn`/`.tres` as text; `.godot/` folder is ignored.

## Linting & formatting

> **Decision (2026-10-09):** `gdtoolkit` (`gdlint` + `gdformat`) runs in CI; a PR fails on lint errors or unformatted files.

- Install: `pip install "gdtoolkit==4.*"` — pin the exact version in CI and check it parses Godot 4.7 syntax.
- Run locally before pushing: `gdformat game/ tools/` then `gdlint game/ tools/`.
- Lint config in `gdlintrc` at the repo root; exclude `game/addons/` (third-party code such as gdUnit4).
