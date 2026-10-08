# Cards

> Card anatomy, types, keywords, rarities and deckbuilding rules. Card list lives in data files (see [technical/03-data-model.md](../technical/03-data-model.md)).
> Status: Draft · Last updated: 2026-10-08

## Card anatomy

```
┌─────────────────────────┐
│ (3)          WOTAN  ◆   │  cost · name · rarity gem
│                         │
│        [ artwork ]      │
│                         │
│ Unit — God      Gods ☉  │  type — subtype · faction
│ Guard.                  │  keywords
│ On play: draw a card.   │  rules text
│ "Ending! Ending!"       │  flavour text (italic)
│ ⚔ 3              ♥ 5    │  attack · health
└─────────────────────────┘
```

| Field | Required | Notes |
|-------|----------|-------|
| Name | ✔ | Unique per card |
| Cost | ✔ | 0–10 Gold |
| Type | ✔ | See below |
| Subtype | – | God, Dwarf, Giant, Hero, Valkyrie, Weapon… (used by synergies) |
| Faction | ✔ | Or Neutral |
| Rarity | ✔ | See below |
| Attack / Health | units only | |
| Keywords | – | See below |
| Rules text | – | Must be generated / validated from the card's effects |
| Flavour text | – | Libretto quote or paraphrase |
| Artwork | ✔ | |

## Card types

| Type | Behaviour |
|------|-----------|
| **Unit** | Stays on the board, can attack and be attacked |
| **Spell** | One-shot effect, then goes to the graveyard |
| **Artifact** | Stays in the artifact zone with an ongoing effect; can't be attacked, can be destroyed by effects |
| **Leader** | Not in the deck; chosen at deck creation; provides Life and Leader ability |

## Keywords (initial list)

| Keyword | Effect |
|---------|--------|
| **Guard** | Enemies must attack this unit first |
| **Charge** | Can attack the turn it is played |
| **Flying** | Can only be blocked/attacked by Flying or Ranged units *(if lanes/blocking are adopted)* |
| **Valhalla** | When destroyed, can be resurrected by Valkyrie effects (marks heroic units) |
| **Forge X** | Spend X extra Gold when playing to upgrade the card |
| **Leitmotif (Y)** | Bonus if you played another card with motif Y this turn |
| **Cursed** | Holder suffers a drawback (Ring and related cards) |
| **Invisible** | Cannot be targeted until it attacks (Tarnhelm) |
| **Fated** | Effect triggers when the Norns' prophecy condition is met |

> **Open question:** Keep the keyword list ≤ 10 for v1.0 so rules text stays short on a phone screen.

## Rarities

| Rarity | Copies per deck | Share of collection |
|--------|-----------------|---------------------|
| Common | 3 | ~50 % |
| Rare | 2 | ~30 % |
| Epic | 2 | ~15 % |
| Legendary | 1 | ~5 % (named characters, the Ring, Nothung…) |

## Deckbuilding rules

- Exactly **30 cards**.
- One **Leader**; cards must be from the Leader's faction or Neutral.
- Copy limits per rarity as above.
- Starter decks provided for each faction (see [06-progression](06-progression.md)).

> **Open question:** Allow dual-faction decks (e.g. Leader + one ally faction)?

## Card set for v1.0 (target)

| Faction | Units | Spells | Artifacts | Total |
|---------|-------|--------|-----------|-------|
| Gods | 14 | 6 | 2 | 22 |
| Nibelungs | 12 | 5 | 5 | 22 |
| Wälsungs & Heroes | 14 | 6 | 2 | 22 |
| Valkyries | 14 | 6 | 2 | 22 |
| Neutral | 20 | 8 | 4 | 32 |
| **Total** | | | | **~120** |

## Example cards (to validate the format)

| Name | Cost | Type | Faction | Stats | Text |
|------|------|------|---------|-------|------|
| Alberich, Lord of the Nibelungs | 4 | Unit — Dwarf | Nibelungs | 3/4 | On play: gain 2 Gold. If you hold the Ring, draw a card. |
| Fafner | 8 | Unit — Giant | Neutral | 8/8 | Guard. Cannot attack the turn after it attacks. |
| Nothung (broken) | 1 | Artifact — Weapon | Heroes | – | Forge 4: becomes *Nothung*: your units get +2 Attack. |
| Ride of the Valkyries | 5 | Spell | Valkyries | – | Return up to 2 Valhalla units from your graveyard to the board. |
| Loge's Fire | 3 | Spell | Gods | – | Deal 2 damage to all enemy units. |
