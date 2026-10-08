# Project structure

> Folder layout of the repository and the Godot project, plus naming conventions.
> Status: Draft · Last updated: 2026-10-08

## Repository

```
ring_card_game/
├── docs/                  # This documentation
├── game/                  # Godot project root (project.godot lives here)
├── tools/                 # Scripts: card import/export, balance sheets, CI helpers
├── .github/workflows/     # CI
├── README.md
└── LICENSE
```

> **Open question:** Godot project at repo root or in `game/`? A subfolder keeps docs/tools out of the Godot import and export.

## Godot project (`res://`)

```
res://
├── project.godot
├── export_presets.cfg          # No secrets committed (see 10-build-and-release)
├── autoload/                   # CardDatabase, SaveManager, SceneRouter, AudioManager, Settings
├── core/                       # Rules engine — NO Node / scene dependency
│   ├── match/                  # Match, GameState, PlayerState, Zone
│   ├── actions/                # Action base class + PlayCardAction, AttackAction, EndTurnAction…
│   ├── effects/                # Effect base class + DealDamageEffect, DrawEffect…
│   ├── events/                 # GameEvent types
│   └── rules/                  # RulesValidator, keyword handlers
├── ai/                         # AIController, evaluators, difficulty profiles
├── data/
│   ├── cards/                  # One .tres per card: <faction>/<card_id>.tres
│   ├── decks/                  # Starter & campaign decks (.tres)
│   ├── leaders/
│   └── campaign/               # Chapter & duel definitions
├── scenes/
│   ├── screens/                # main_menu.tscn, duel.tscn, deck_builder.tscn…
│   ├── components/             # card_view.tscn, hand_view.tscn, board_view.tscn…
│   └── popups/
├── ui/
│   ├── theme/                  # Godot Theme resources, fonts, styleboxes
│   └── icons/
├── assets/
│   ├── art/cards/
│   ├── art/boards/
│   ├── audio/music/
│   ├── audio/sfx/
│   └── fonts/
├── localization/               # translations .csv → .translation
└── tests/
    ├── unit/                   # core/ and ai/ tests
    └── integration/            # full scripted matches
```

## Naming conventions

Follows the official Godot style guide.

| Element | Convention | Example |
|---------|-----------|---------|
| Folders & files | `snake_case` | `card_view.tscn`, `deal_damage_effect.gd` |
| `class_name` | `PascalCase` | `class_name DealDamageEffect` |
| Card IDs | `snake_case`, stable, never reused | `alberich_lord`, `nothung_broken` |
| Scene root node | Same as file, PascalCase | `CardView` |
| Translation keys | `UPPER_SNAKE` with prefix | `CARD_ALBERICH_LORD_NAME`, `UI_END_TURN` |

One script = one `class_name` = one file. Scene scripts sit next to their `.tscn`.
