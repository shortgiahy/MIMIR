# Sushi Sea — Luau Coding Standards

All code written for this project follows these conventions. Autonomous Claude Code agents must read this before writing any Luau.

---

## Naming

| Thing | Convention | Example |
|-------|-----------|---------|
| ModuleScript return table | PascalCase | `FishTable`, `EconomyService` |
| Local variables | camelCase | `catchResult`, `plateValue` |
| Constants | SCREAMING_SNAKE | `MAX_AGING_SLOTS`, `CLAMP_FRESHNESS_MAX` |
| RemoteEvents | `{System}_{Event}` or `Player_{Action}` | `Weather_StormBroadcast`, `Player_CastLine` |
| DataStore keys | `{Name}_v{version}` | `PlayerData_v1`, `DailyRewards_v1` |
| Private module functions | camelCase prefixed with `_` | `_resolveExtraction` |
| Types (Luau) | PascalCase | `type FishEntry = {...}` |

---

## Module Structure

Every ModuleScript returns a single table. No globals. No shared state in free variables (use `module._state` if state is needed, documented clearly).

```lua
-- ServerStorage/Modules/ExampleModule.lua
local ExampleModule = {}

-- Constants at the top
local SOME_CONSTANT = 42

-- Types
type Config = {
    value: number,
    label: string,
}

-- Private helpers prefixed with _
local function _helperFn(x: number): number
    return x * 2
end

-- Public API
function ExampleModule.doThing(config: Config): string
    return config.label .. ": " .. _helperFn(config.value)
end

return ExampleModule
```

---

## Type Annotations

**All public functions must have typed parameters and return types.** Private helpers should too unless they're trivially obvious.

```lua
-- Good
function EconomyService.resolvePlate(fish: FishEntry, cookingLevel: number): number

-- Not acceptable
function EconomyService.resolvePlate(fish, cookingLevel)
```

Use Luau's strict mode where possible:
```lua
--!strict
```

---

## Error Handling

**All DataStore calls are wrapped in pcall.** No exceptions.

```lua
local success, result = pcall(dataStore.GetAsync, dataStore, key)
if not success then
    warn("[PlayerDataService] GetAsync failed for", key, "—", result)
    return nil
end
```

**Retry pattern for DataStore** (max 3 attempts, exponential backoff):
```lua
local function retryAsync(fn, ...)
    local attempts = 0
    local success, result
    repeat
        attempts += 1
        success, result = pcall(fn, ...)
        if not success then task.wait(2 ^ (attempts - 1)) end
    until success or attempts >= 3
    return success, result
end
```

**Do not use `error()` for expected failure paths** (player not found, fish spoiled, etc.). Return `nil, "reason_string"`. Reserve `error()` for truly unexpected states.

---

## Economy Safety

**Never trust client-sent economy values.** The server recomputes everything.

```lua
-- WRONG — never do this
local plateValue = remoteFunction:InvokeServer(fishId)
awardGold(player, plateValue)  -- client computed this

-- RIGHT
Player_ServePlate:FireServer(fishId)
-- server resolves plateValue from FishTable × multipliers, awards gold server-side
```

**Clamps are enforced at the point of formula resolution**, not at the point of display:
```lua
local freshnessPolish = math.clamp(computedFreshness, EconomyConfig.CLAMP_FRESHNESS_MIN, EconomyConfig.CLAMP_FRESHNESS_MAX)
```

---

## DataStore Rules

- **Schema version field in every store.** If the version doesn't match current, run the migration function before use.
- **Never silently overwrite player data.** Print a warning and run migration.
- **Cache ownership checks** (GamePass, etc.) per session — never call `UserOwnsGamePassAsync` more than once per player per pass.
- **`SetAsync` for saves, `UpdateAsync` only when doing read-modify-write atomically.** Don't use `SetAsync` for atomic ops.

---

## RemoteEvent Rules

- **Client → Server:** Server always validates. Ignore any client-sent value that affects economy or inventory without server recomputation.
- **Server → Client:** Fire the minimal payload. Don't send the entire player data table; send only what the client needs to display.
- **Rate limiting:** Debounce any `Player_*` event that can be spammed (cast, serve, etc.) at the server. Reject calls under `MIN_ACTION_INTERVAL` seconds.

---

## Comments

Default to no comments. Write one only when the **why** is non-obvious.

```lua
-- Good: explains a non-obvious invariant
-- Aging fish are NOT on the spoilage track — SpoilageService must skip locker entries
for _, fish in inventory do
    if not fish.inLocker then
        SpoilageService.tick(fish)
    end
end

-- Bad: narrates the code
-- Loop through inventory and tick spoilage
for _, fish in inventory do
    SpoilageService.tick(fish)
end
```

---

## File Header

Every script gets a one-line header stating its role:

```lua
-- WeatherService: server-authoritative weather events and legendary spawn broadcast
```

That's it. No author, no date, no task reference.

---

## What Not to Build

If an implementation requires any of the following, stop and flag it:
- A global variable visible across scripts
- Client-authoritative economy math
- Shared legendary encounter state between two players
- A DataStore write that doesn't use the retry wrapper
- A multiplier chain that can produce values outside the authored clamp range
