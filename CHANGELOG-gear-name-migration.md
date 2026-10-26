# Changelog — One-time gear name migration for saved student gear

Date: 2026-10-26 (Asia/Hong_Kong)

## Files changed
- `index.html` — migration helpers + load/cloud hooks; demo HTML placeholders

## What
When the game loads (localStorage, BAKED defaults, or after a cloud pull/merge), existing student gear in **equipped** and **inventory** is renamed once to match rarity naming rules via `buildMigratedGearName` (deterministic, seeded by `uid`). Stats / progress are not wiped — only `name` is updated, plus `sellValue` when cheap/safe via `getSellValueForRarity`.

## Rules applied
| Rarity | Migration name |
|--------|----------------|
| Common | seeded material + type |
| Rare | seeded refined/advanced-style adj + type |
| Exotic | keep creative names; else subtype fancy name / flair + type |
| Legendary | always `Legendary ` + type |

## Versioning
`db.gearNameMigrationV1 = true` after the first pass. Non-conforming names (e.g. cloud-pulled old gear) are still fixed on later loads; conforming names are left alone (idempotent).

## Demo HTML
Replaced leftover placeholders (`Shadow Dagger`, `Cloth Scholar Hood`, etc.) with Common-style names matching baked starters (`Iron Dagger`, `Leather Hood`, …).
