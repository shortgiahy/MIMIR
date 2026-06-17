# Sushi Sea — Open Threads

Unresolved items from [[GDD v2]], listed in resolution order. **Do not invent resolutions — surface to user and use grill-me.**

---

## 1. The cook verb ⚡ HIGHEST PRIORITY — blocks vertical slice

**What the player physically does when cooking on the boat is undefined.**

Options on the shelf (not yet evaluated):
- Timing bar
- Filleting/portioning minigame
- Slicing swipe
- Simple hold-button

Same question applies to the **serving verb** on the boat (lower stakes, same session).

**Downstream blockers:** nothing in the playable slice can be prototyped until this verb is designed. Also blocks Omakase counter design (#6).

**Next action:** open next design session here using grill-me.

---

## 2. Menu variety / nigiri depth

Single-fish nigiri is the only dish; attribute-mixing is cut; dry-aging is opt-in and slot-limited.

**Core question:** is species-as-variety enough to hold an 18+ audience over *weeks*?

Relief valves already on the shelf:
- Scored-attribute open mixing (post-launch slot)
- Farming line (post-launch slot)

Pulling either forward is a real scope decision — interrogate before committing to nigiri-only at launch.

---

## 3. First-pass economy model

Build the **5-row faucets-minus-sinks table**:

| Row | Avg plate value | Plates/hr | Headcount | Wages/hr | Spoilage/hr | Net income/hr | Time to next tier |
|---|---|---|---|---|---|---|---|
| Tutorial boat | | | | | | | |
| New restaurant | | | | | | | |
| Mid | | | | | | | |
| Late | | | | | | | |
| Whale | | | | | | | |

**Single output to read:** does net income/hr grow faster than next-tier cost?

**Dial to find:** where does the throughput cliff land? (Too early = game's over; healthy = week 6.)

**Dials to tune against it:** spoilage rate + next-tier pricing. Wages are a weak dial.

**Dependency:** gates real tuning. Run as a dedicated numbers session.

---

## 4. Legendary fight phase structure

The fight is locked as "scaled-up multi-phase reel," but unspecified:
- Actual phase count
- Per-level-band window sizes
- Stamina curves
- Dive-phase mechanics

**Dependency:** needs base reel verb numbers first (#1).

---

## 5. Spoilage ↔ offline-coast values

Stance is resolved (freshness-governed; storage upgrades raise capacity and slow spoilage). Unset:
- Actual decay rates
- Storage tier ladder
- Resulting coast lengths per tier

**Critical:** this is the same dial as the 24–48h return target. Must be tuned jointly with economy table (#3).

---

## 6. Smaller undefineds (close before launch)

- **Purchasing tiers:** boats, rods, equipment, storage, restaurant tiers — upgrade ladder, costs, and what each tier unlocks.
- **Yelp prestige formula:** how shifts accumulate toward 5 stars; what counts as a "bad shift."
- **Hidden traffic stat formula:** exact weighting of Yelp + cosmetics + Hospitality into spawn volume.
- **Omakase counter mechanics:** what the player physically does to lift the ceiling; size of the boss-aura speed bonus. Couples to #1.
- **Walkout rules:** how long customers wait, what gold is lost, how it ties into the (shelved) popularity layer's eventual hook.
- **Storm catalog:** which weather types map to which legendaries, durations, zone sizes, broadcast lead time.
