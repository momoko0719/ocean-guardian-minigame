# Tidekeeper — Game Rules

> This document is the authoritative source for game rules and controls.  
> When rules are updated here, update the code in `ocean_game.html` accordingly.

---

## Overview

**Tidekeeper** is a local 4-player competitive game.  
Players gather seaweed from the ocean, build fish ponds, cook and sell fish at the best market price, and compete to have the most money when time runs out.

- **Win condition:** Most money ($) when the 3-minute timer ends.
- **Ranking:** Primary sort by `money + title bonus` (Eco Hero +5, Poacher −5). Tiebreak by Morality.
- **Moral dimension:** Releasing fish gives +1 Morality. Stealing gives −1.

---

## Map Layout

```
┌─────────────────────────────────────────────────┐
│    🌊 Ocean (upper-left)   🌊 Ocean (upper-right)│
│                                                 │
│     [MARKET]         [KITCHEN]                  │
│  ─────────────── ISLAND TOP ──────────────────  │
│            open island ground                   │
│         (players build ponds anywhere)          │
│  ─────────────── ISLAND BOT ──────────────────  │
│                                                 │
│    🌊 Ocean (lower-left)   🌊 Ocean (lower-right)│
│                                                 │
│  🌿 Seaweed spots ring the shore of the island  │
└─────────────────────────────────────────────────┘
```

- **Island** — central land mass; any ground on the island is valid for pond building.
- **Market** — top-left of island; sell fish for money.
- **Kitchen** — top-right of island; cook fish to increase their sell value.
- **Ocean** — everything outside the island; seaweed grows here; fish are released here.
- **Seaweed spots** — 15 fixed positions ringing the island shore. New spots only appear when a fish is released.

---

## Resources

| Resource | Description |
|---|---|
| 🌿 Seaweed | Gathered from ocean spots. Used to build/upgrade ponds (3 per action). |
| 🐟 Fish | Produced by ponds (Salmon or Tuna). Sell raw at Market or cook at Kitchen first. |
| 💰 Money | Earned by selling fish. Main win metric. |
| ⚖️ Morality | +1 per release, −1 per steal. Tiebreaker in ranking. |

---

## Core Loop

```
1. Go to ocean → press A near seaweed to gather (takes ~2.4 s).
2. Return to island → short-press B with 🌿×3 → choose pond type (Salmon or Tuna) → A to confirm.
3. Wait for pond to produce fish → press A near your pond to pick up a fish.
4. Choice A: carry fish to Market → press A to sell at current live market price.
5. Choice B: carry fish to Kitchen → press A to cook (10 s) → fish auto-moves to cooked pile.
           → Sell cooked fish at Market at any time (×2 price).
6. Optional: press B in the ocean while holding a fish to release it → restores seaweed, +1 Morality.
```

---

## Live Fish Prices (top HUD)

- The top bar shows the current market price for **Salmon** and **Tuna**, updated every **30 seconds**.
- Both prices fluctuate between **$3 – $5** per fish.
- When a price is at **$5 (max)**, it pulses and shows ⬆ to signal a sell opportunity.
- Cooked fish sell for **2× the current market price** (rounded).
- SSR Goldfish have a **fixed** price: $15 raw / $35 cooked.

---

## Fish Ponds

Each player can build up to **2 ponds** — one **Salmon** and one **Tuna** — anywhere on the island (including near buildings).

| Level | Cost to build/upgrade | Fish capacity | Spawn rate |
|---|---|---|---|
| Lv 1 | 🌿×3 | 5 fish | ~13 s |
| Lv 2 | 🌿×3 (upgrade) | 10 fish | ~10 s |
| Lv 3 | 🌿×3 (upgrade) | 15 fish | ~8 s |

### Pond Build / Upgrade Menu

When a player has 🌿×3 and is on open island ground, **short-press B** to open the popup menu:

| Situation | Left option | Right option |
|---|---|---|
| No ponds yet | + Create Salmon | + Create Tuna |
| Has Salmon, no Tuna | ⬆ Upgrade Salmon | + Create Tuna |
| Has Tuna, no Salmon | + Create Salmon | ⬆ Upgrade Tuna |
| Has both ponds | ⬆ Upgrade Salmon | ⬆ Upgrade Tuna |
| A pond is at Lv 3 | — (greyed out) | other option |

- **◄ / ►** (stick or D-pad) to switch between the two options.
- **A** to confirm. **B** to cancel.
- The pond is built at the player's current standing position (must not overlap another pond).
- If the selected option is at max level (Lv 3), a warning plays and seaweed is not consumed.

### Pond Label
Each pond shows a small tag above it: **[fish icon] · LvX**. The owner is identified by a coloured triangular flag to the right of the pond.

---

## Fish Types

| Fish | Raw sell | Cooked sell | Cook time | Spawn chance |
|---|---|---|---|---|
| Salmon | market price ($3–5) | price × 2 | 10 s | 95% in Salmon pond |
| Tuna | market price ($3–5) | price × 2 | 10 s | 95% in Tuna pond |
| SSR Goldfish | $15 (fixed) | $35 (fixed) | 20 s | 5% in any pond |

- SSR fish trigger the **SSR Clash** minigame when stolen.

---

## Cooking System

- Bring a fish to the **Kitchen** and press **A** to start cooking.
- While a fish is cooking, you **cannot start another** until it is done.
- When cooking finishes (timer hits 0), the fish **automatically moves to your cooked pile** — you do not need to go to the Market immediately.
- You can cook multiple fish sequentially, accumulating a cooked pile to sell when prices are high.
- Your badge above your character shows: `🐟N` (raw in hand) · `🍳` (cooking) · `✅N` (cooked, waiting to sell).

---

## Selling

When you press **A** at the **Market**:
- If you have only one sellable item, it sells immediately.
- If you have multiple types (raw salmon, raw tuna, cooked salmon, cooked tuna, etc.), a selection menu appears.
- **◄ / ►** to choose. **A** to sell. **B** to cancel.

---

## Stealing

- Press **A** near an **opponent's pond** that has fish → steal the top fish.
- If the pond owner is **Guarding** → steal is blocked; thief gets 5 s cooldown.
- Stealing triggers a **10 s cooldown** for the thief and **−1 Morality**.

---

## Police NPC

A police officer NPC can appear after any successful steal (including SSR clash victory).

### Spawn conditions
- **50% chance** to spawn each time a steal succeeds (non-SSR or SSR clash winner).
- **Only one police** can be active at a time — if already chasing, no new police spawns.
- Spawn point: **bottom-center of the island** (the dock area gap between seaweed spots).

### Behaviour
- The police immediately **chases the stealing player** for **10 seconds**.
- Police speed is **~150 px/s**, faster than a player (~118 px/s).
- A **red/blue siren capsule** flashes above the police, plus a countdown timer bar.
- A warning announcement appears: 🚔 *Police! Px is being chased!*

### Catch
- If the police closes within 32 px of the target player → **−$10** (money can go negative).
- The police then disappears immediately.
- If the 10-second timer expires without catching the player → police disappears without penalty.

### SSR Clash (minigame)

If the stolen fish is SSR, a button-mashing clash starts:
- Both players press A as fast as possible for 3 seconds.
- Winner takes the SSR fish. Winner still takes −1 Morality and 10 s steal cooldown.

---

## Guarding

- **Hold B** while **on the island** → activates a guard shield after ~0.25 s hold (lasts up to 5 s while held, max 5 s, then 5 s cooldown).
- **Release B** before 0.25 s (short press) on island → opens the **build/upgrade pond menu** instead (requires 🌿×3).
- **Press B** while **in the ocean** holding a fish → **release the fish** (restores seaweed, +1 Morality).
- While guarding, any steal attempt on your ponds is blocked.

---

## End-of-Game Titles & Ranking

- **🌿 Eco Hero +5** — player with the most fish released (must be > 0).
- **🧹 Poacher −5** — player with the most fish stolen (must be > 0).
- Players can hold both titles simultaneously.
- Final rank = `money + title bonus`, tiebreak by Morality.

---

## Controls

### Gamepad (Xbox-style, standard layout)

| Input | Function |
|---|---|
| Left Stick / D-Pad | Move |
| A button (btn 0) | Action (gather / pick fish / sell / steal / confirm menu) |
| B — hold on island | Guard shield (up to 5 s, then 5 s cooldown) |
| B — short press on island | Open build/upgrade pond menu (needs 🌿×3) |
| B — press in ocean with fish | Release fish (+seaweed, +1 Morality) |
| B — any menu open | Cancel menu |
| ◄ / ► during any menu | Switch between options |

### Keyboard Fallback

| Player | Move | Action (A) | Guard/B |
|---|---|---|---|
| P1 | WASD | E | Q |
| P2 | Arrow keys | / | . |
| P3 | TFGH | Y | R |
| P4 | IJKL | O | U |

---

## Action Priority (what A does in each context)

The game checks these in order when A is pressed:

1. **Sell/Cook menu open** → confirm selected option
2. **Build menu open** → confirm selected pond option
3. **Near enemy pond with fish** → Steal (if not on cooldown)
4. **Near own pond with fish** → Pick up fish
5. **In Market zone** → open sell menu (or sell immediately if single option)
6. **In Kitchen zone** → open cook menu (or cook immediately if single type); shows timer if currently cooking
7. **In ocean** → Start gathering nearest seaweed (within 60 px, takes 2.4 s)

---

## Ecosystem (internal)

- The ecosystem value (0–15) tracks how many seaweed spots exist.
- Pond fish production slows when ecosystem < 5.
- When ecosystem ≤ 3, a red warning border flashes around the screen.
- Ecosystem is not displayed on the HUD (replaced by live fish prices), but its effect on production remains.
