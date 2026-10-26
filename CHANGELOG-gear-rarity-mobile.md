# Changelog — Gear rarity naming, sell values, power order & mobile UX

Date: 2026-10-26 (Asia/Hong_Kong)

## Files changed
- `index.html` (main game source; GitHub Pages entry)

## 1) Gear naming by rarity
Implemented `buildGearName(slot, rarity, baseSub, isCosmetic)` plus `typeName` on each `GEAR_SUBTYPES` entry.

| Rarity | Rule | Examples |
|--------|------|----------|
| Common | material + item type only | Steel Longsword, Iron Greaves, Hide Dagger |
| Rare | refined/advanced-style adjective + type | Advanced Longsword, Refined Greaves |
| Exotic | wilder/creative names allowed | Shadowfang Needle, Dragonward Bulwark, Bloodmoon Cap |
| Legendary | **always** starts with `Legendary` + type | Legendary Greatsword, Legendary Amulet |

Starter gear, Royal Armory shop exclusives, and baked default student gear names updated to match.

## 2) Resell value multipliers
`RARITY_SELL_MULT = { Common: 1, Rare: 3, Exotic: 8, Legendary: 20 }`

`getSellValueForRarity(baseSell, rarity, tierScale)` used by loot drops and shop purchases.
- Loot: base ≈ `25 * tierScale * mult` → Common 25 / Rare 75 / Exotic 200 / Legendary 500 at tier 1.
- Shop: ~5% of purchase price (divides out rarity before re-applying mult so Legendary does not refund at full price).

## 3) Power balance (Legendary > Exotic > Rare > Common)
`RARITY_POWER_MULT = { Common: 1.0, Rare: 1.6, Exotic: 2.55, Legendary: 4.1 }`

Subtype stats are blended 70% toward slot average / 30% subtype so intra-slot variance cannot break rarity ordering. Verified with simulation: for every slot, max(Common) < min(Rare) < max(Rare) < min(Exotic) < … < min(Legendary).

Shop exclusives also ordered: max Common < min Rare < min Exotic < min Legendary.

## 4) Mobile UX
- Viewport: `viewport-fit=cover`
- Body: `min-height: 100dvh`, touch momentum scrolling
- `@media (max-width: 640px)`: remove clipping `max-height` on inventory/shop/quest lists; flexible log panel; min 44px tap targets; full-width tooltips/toasts
- Lists use `max-h-none` on mobile / `sm:max-h-[…]` on desktop

## Deliverables / push status
- `gh auth status`: **not logged in** — cannot push or open PR
- Local branch: `feature/gear-rarity-mobile-ux` (committed on box)
- Patch + zip: `/workspace/lsland-deliverables/`

### How to publish
1. Authenticate: `gh auth login` (or push with your GitHub credentials)
2. From `/workspace/lsland`: `git push -u origin feature/gear-rarity-mobile-ux`
3. `gh pr create --title "Gear rarity naming, sell/power balance, mobile UX" --body "$(cat CHANGELOG-gear-rarity-mobile.md)"`
4. Or replace `index.html` on `main` with `/workspace/lsland-deliverables/index.html` / apply the `.patch`
