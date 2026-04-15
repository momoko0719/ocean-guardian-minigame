# Ocean Guardian — Game Rules

> This document is the authoritative source for game rules and controls.  
> When rules are updated here, update the code in `ocean_game.html` accordingly.

---

## Overview

**Ocean Guardian** is a local 4-player competitive/cooperative game.  
Players gather seaweed from the ocean, build fish ponds on the island, sell fish for money, and compete to have the most money when time runs out — while trying not to destroy the ecosystem.

- **Win condition:** Most money ($) when the 3-minute timer ends.
- **Moral dimension:** Releasing fish improves your Morality score (+1 per release). Stealing fish reduces it (−1 per steal). Morality is displayed but does not affect the win condition by default (can be made a tiebreaker in future rules).

---

## Map Layout

```
┌─────────────────────────────────────────────────┐
│    🌊 Ocean (upper-left)   🌊 Ocean (upper-right)│
│                                                 │
│     [MARKET]         [KITCHEN]                  │
│  ─────────────── ISLAND TOP ──────────────────  │
│  P1 zone (top-left) │ P2 zone (top-right)       │
│  ─────────── island center divider ──────────── │
│  P3 zone (bot-left) │ P4 zone (bot-right)       │
│  ─────────────── ISLAND BOT ──────────────────  │
│                                                 │
│    🌊 Ocean (lower-left)   🌊 Ocean (lower-right)│
│                                                 │
│  🌿 Seaweed spots ring the shore of the island  │
└─────────────────────────────────────────────────┘
```

- **Island** — central land mass; split into 4 player zones.
- **Market** — top-left of island; sell fish for money.
- **Kitchen** — top-right of island; cook fish to increase their sell value.
- **Ocean** — everything outside the island; seaweed grows here; fish are released here.
- **Seaweed spots** — 15 fixed positions ringing the island shore. New spots only appear when a fish is released.

---

## Resources

| Resource | Description |
|---|---|
| 🌿 Seaweed | Gathered from ocean spots. Used to build/upgrade ponds (3 per action). |
| 🐟 Fish | Produced by ponds. Held in hand (1 at a time). Sell at Market or cook first at Kitchen. |
| 💰 Money | Earned by selling fish. Main win metric. |
| ⚖️ Morality | +1 per release, −1 per steal. Displayed on HUD. |

---

## Core Loop

```
1. Go to ocean → press A to gather seaweed from a nearby spot (hold ~2.4 s).
2. Return to island → press A in your zone to build/upgrade a pond (costs 🌿×3).
3. Wait for pond to fill with fish → press A near your pond to pick up a fish.
4. Choice A: carry fish to Market → press A to sell instantly.
5. Choice B: carry fish to Kitchen → press A to start cooking → carry to Market to sell cooked (higher value).
6. Optional: press B/Guard in the ocean while holding a fish to release it → restores 2 seaweed spots (+5 for SSR), +1 Morality.
```

---

## Seaweed & Ecosystem

- **15 seaweed spots** are fixed around the island shore at game start.
- Seaweed does **not** auto-regenerate over time.
- New seaweed spots only appear when a player **releases a fish** in the ocean:
  - Common / Blue fish → +2 spots.
  - SSR Goldfish → +5 spots.
- The **ECOSYSTEM bar** (top-right HUD) shows how many spots remain (0–15).
- When ecosystem ≤ 3, a red warning border flashes.
- Pond fish production rate slows when ecosystem < 5.

---

## Fish Ponds

Each player can build up to **2 ponds** in their own zone.

| Level | Cost to build/upgrade | Fish capacity | Spawn rate |
|---|---|---|---|
| Lv 1 | 🌿×3 | 5 fish | ~13 s |
| Lv 2 | 🌿×3 (upgrade) | 10 fish | ~10 s |
| Lv 3 | 🌿×3 (upgrade) | 15 fish | ~8 s |

- Ponds can only be built / upgraded while on the **island** in your own zone.
- Each upgrade applies to your first (oldest) pond only until Lv 3; then you can build a second.

---

## Fish & Market Prices

| Fish | Raw sell | Cooked sell | Cook time |
|---|---|---|---|
| Common fish (75% chance) | $2 | $5 | 8 s |
| Blue fish (20% chance) | $6 | $14 | 12 s |
| SSR Goldfish (5% chance) | $30 | $70 | 20 s |

- SSR fish in a pond trigger the **SSR Clash** minigame if a player tries to steal it.

---

## Stealing

- Press **A** near an **opponent's pond** that has fish → steal the top fish.
- If the pond owner is actively **Guarding (B held)** → steal is blocked; thief gets 5 s cooldown.
- Stealing triggers a **10 s cooldown** for the thief.
- Morality −1 on a successful steal.

### SSR Clash (minigame)

If the stolen fish is SSR, a button-mashing clash starts:
- Both players press A as fast as possible for 3 seconds.
- Winner takes the SSR fish. Loser gets nothing.
- Winner still takes −1 Morality and 10 s steal cooldown.

---

## Guarding

- Press **B** (Guard) while **on the island** to toggle a guard shield.
- While guarding, any steal attempt on your ponds is blocked.
- You can still move while guarding.
- Press **B** while **in the ocean** holding a fish → **release the fish** (instead of toggling guard).

---

## Controls

### Keyboard Layout

| Player | Move | Action (A) | Guard / Release (B) |
|---|---|---|---|
| P1 | WASD | E | Q |
| P2 | Arrow keys | / | . |
| P3 | TFGH | Y | R |
| P4 | IJKL | O | U |

### Gamepad (Xbox-style, standard layout)

| Input | Function |
|---|---|
| Left Stick / D-Pad | Move |
| A button (btn 0) | Action |
| B button (btn 1) | Guard / Release |

Gamepad order = USB connection order. See `GAMEPAD.md` for extended notes.

---

## Action Priority (what A does in each context)

The game checks these in order when A is pressed:

1. **Near enemy pond with fish** → Steal (if not on cooldown)
2. **Near own pond with fish** → Pick up fish (into hand)
3. **In Market zone** → Sell cooked fish (if done cooking) → Sell raw fish in hand → Error
4. **In Kitchen zone** → Deliver cooked fish message → Start cooking fish in hand → Error
5. **In own zone on island** → Upgrade pond (if Lv<3, 🌿≥3) → Build new pond (if <2, 🌿≥3) → Error
6. **In ocean, hand empty** → Start gathering nearest seaweed (within 60px, takes 2.4 s)
7. **In ocean, holding fish** → Prompt to use B/Guard to release instead

---

## End of Game

- When the 3-minute timer hits 0, the game ends.
- Players are ranked by **money** (descending).
- Stats shown: total steals, total releases, Eco Hero (most releases), Most Steals.

---

## Future Rule Ideas (not yet implemented)

- Morality as tiebreaker or separate podium ranking.
- Ecosystem collapse: if eco hits 0, all ponds stop producing.
- Alliance / gifting fish between players.
- Time extensions when eco is healthy (above 12).
