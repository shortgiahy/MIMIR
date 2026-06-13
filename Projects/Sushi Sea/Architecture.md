# Sushi Sea — System Architecture

How the game's systems connect. Claude Code builds to this layout. Any deviation requires explicit approval from the user.

---

## Roblox Service Ownership

```
ServerScriptService/
  Services/
    WeatherService.server.lua       -- weather events, legendary spawn tables, broadcast
    PlayerDataService.server.lua    -- DataStore reads/writes, offline bank snapshot/restore
    EconomyService.server.lua       -- plate value resolution, server-side catch validation
    StaffService.server.lua         -- NPC cook/serve AI at brick-and-mortar tier
    CustomerService.server.lua      -- customer spawn process, 6-stage lifecycle state machine
    SpoilageService.server.lua      -- freshness tick for all inventory; runs while offline too
  Init.server.lua                   -- wires all services, no logic here

ServerStorage/
  Modules/
    FishTable.lua                   -- AUTHORED lookup: {species -> base_price}. No computed values.
    OfflineBankCalculator.lua       -- snapshot-in / snapshot-out math, net of wages and spoilage
    DryAgingLocker.lua              -- aging track: separate from spoilage, slot management
    PassManager.lua                 -- GamePass ownership cache + purchase prompt

ReplicatedStorage/
  Config/
    EconomyConfig.lua               -- clamped multiplier constants (CLAMP_FRESHNESS, etc.)
    SkillConfig.lua                 -- XP curves, level caps per skill
  Modules/
    FishSpecies.lua                 -- shared fish data client needs for UI (species name, rarity tier)
  Events/
    RemoteEvents/                   -- see naming convention below
    RemoteFunctions/

StarterPlayerScripts/
  Controllers/
    FishingController.lua           -- cast/reel input handler; sends to server, receives validation
    BoatCookController.lua          -- manual cook/serve on the boat (TBD — Open Thread #1)
    WeatherClient.lua               -- storm alert display, legendary FOMO UI
  UI/
    RestaurantUI.lua                -- customer display, prestige bar, Yelp app
    FreshnessUI.lua                 -- inventory freshness timers
    ShopUI.lua                      -- Purchasing skill storefront

StarterGui/                         -- UI containers only; logic lives in StarterPlayerScripts
```

---

## RemoteEvent Naming Convention

**Server → Client:** `{System}_{Event}`
- `Weather_StormBroadcast` — payload: `{zone, duration, legendaryType, oddsMultiplier}`
- `Economy_PlateResolved` — payload: `{plateValue, breakdown}`
- `Spoilage_InventoryUpdate` — payload: `{inventory}` (freshness state sync)

**Client → Server:** `Player_{Action}`
- `Player_CastLine` — payload: `{location}`
- `Player_ReelInput` — payload: `{tension}` (per-frame during a fight)
- `Player_ServePlate` — payload: `{fishId}` (boat manual serve — TBD)
- `Player_PullFromLocker` — payload: `{slot}` (cash out dry-aging fish)

**Rule:** No client RemoteFunction that returns economy-affecting values. Economy resolves server-side only.

---

## PlayerData Schema (DataStore key: `PlayerData_v1`)

```lua
{
  skills = {
    fishing     = { level = 1, xp = 0 },
    cooking     = { level = 1, xp = 0 },
    sailing     = { level = 1, xp = 0 },
    hospitality = { level = 1, xp = 0 },
    purchasing  = { level = 1, xp = 0 },
  },

  inventory = {
    -- array of fish in the spoilage track
    { id = "uuid", species = "salmon", caughtAt = 1718000000 },
  },

  agingLocker = {
    -- fish on the aging track (NOT on the spoilage track)
    -- slot count gated by equipment tier
    { slot = 1, species = "tuna", placedAt = 1718000000 },
  },

  restaurant = {
    tier           = 0,   -- 0 = boat only, 1+ = brick-and-mortar tiers
    staffHeadcount = 0,
    prestigePoints = 0,
    trophies       = {},  -- { species, mountedAt } — legendary mounts
  },

  economy = {
    gold                = 0,
    offlineSnapshotAt   = 0,    -- os.time() at last logout
    offlineStockCount   = 0,    -- fish in inventory at snapshot
    tutorialLoanOwed    = 500,  -- zeroed after repayment; never reused
  },

  meta = {
    schemaVersion = 1,          -- increment on breaking changes; migrate, never overwrite
    firstJoinAt   = 0,
    lastJoinAt    = 0,
  }
}
```

**Migration rule:** if `schemaVersion` doesn't match current, run the migration chain before handing data to any system. Never silently overwrite.

---

## Economy Formula (enforce at EconomyService)

```
served_plate_value = species_base × cooking_extraction × freshness_polish × dry_age_mutation
```

| Term | Source | Clamp |
|------|--------|-------|
| `species_base` | `FishTable.lua` authored lookup | N/A — it IS the large term |
| `cooking_extraction` | `CookingSkill.level / MAX_LEVEL` | `[0, 1]` |
| `freshness_polish` | linear from `caughtAt` timestamp | `[0.5, 1.5]` |
| `dry_age_mutation` | rare roll on locker pull | `[1.0, 2.5]` — never orders of magnitude |

**Hard rule:** all four multipliers are applied server-side only. Client receives the final resolved value for display, never the components. This prevents spoofed plate values.

---

## Offline Bank Calculation (OfflineBankCalculator)

On logout: `PlayerDataService` saves `offlineSnapshotAt = os.time()` and `offlineStockCount = #inventory`.

On next login, `OfflineBankCalculator.compute(data)`:
1. `elapsed = os.time() - data.economy.offlineSnapshotAt`
2. Compute spoilage: fish that would have decayed past "stale" in `elapsed` time are marked lost
3. `platesServed = min(throughputCap × elapsed, remainingStockAfterSpoilage)`
4. `grossIncome = platesServed × avgPlateValueAtLogout`
5. `wages = data.restaurant.staffHeadcount × WAGE_RATE × elapsed`
6. `netBank = max(0, grossIncome - wages)` — never negative
7. Add `netBank` to `data.economy.gold`; clear snapshot fields

**Do not replay the restaurant tick-by-tick.** Compute the closed-form result only.

---

## Weather & Legendary System

One system serves both purposes (from GDD v2):

1. `WeatherService` selects a weather event: `{zone, type, duration, legendaryType}`
2. Broadcasts `Weather_StormBroadcast` to all clients (FOMO hook)
3. Modifies each player's server-side roll table for `Player_CastLine` events in the zone
4. On a legendary hook: initiates the multi-phase reel on that player's connection only
5. Other players are unaffected — their rolls remain independent

**No shared legendary state.** No tag-team, no dogpile. Each player fights their own creature independently.

---

## Manual-then-Automate Code Path

The GDD mandates: build the conversion logic once, swap the driver.

```
[ConversionModule] cook(fish) -> plate
        ↑                           ↑
 player input                  staff AI
 (BoatCookController)          (StaffService)
```

`ConversionModule` in `ServerStorage/Modules/` is the canonical implementation. Both the boat verb and the restaurant staff call it. **Do not duplicate the logic.**

---

## What Is NOT in This Game

Do not build:
- A wholesale fish market (no path from fish to gold except the kitchen)
- Crafting of any kind (all durables are Purchased, never assembled)
- Shared legendary encounters (per-player rolls only)
- A total-loss spoilage state (fish goes stale → spoiled → tossed, but nothing explosive)
- A recurring debt system (tutorial loan only, zeroed and done)

If a design question pushes toward any of these, surface it — don't implement.
