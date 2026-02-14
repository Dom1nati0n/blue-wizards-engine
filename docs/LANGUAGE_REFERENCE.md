# Blue Wizards Language Reference

Complete reference for all opcodes, constants, and behaviors in Blue Wizards Engine V29.

## Table of Contents

1. [Opcode Categories](#opcode-categories)
2. [Alchemy (Math)](#alchemy-math)
3. [Runecraft (Logic)](#runecraft-logic)
4. [Traversal (Movement)](#traversal-movement)
5. [Inventory (Stack)](#inventory-stack)
6. [Scribing (Grid I/O)](#scribing-grid-io)
7. [Divination (Sensing)](#divination-sensing)
8. [Rituals (Control Flow)](#rituals-control-flow)
9. [Prophecy (Output)](#prophecy-output)
10. [Conjuration (Lifecycle)](#conjuration-lifecycle)
11. [Harvest & Resources](#harvest--resources)
12. [Memory Crystals (Variables)](#memory-crystals-variables)
13. [Work Orders (Queues)](#work-orders-queues)
14. [Omens (Events)](#omens-events)
15. [Arcane Arrays (Higher-Order)](#arcane-arrays-higher-order)
16. [Roles & Kingdom](#roles--kingdom)
17. [Constants Reference](#constants-reference)

---

## Opcode Categories

### Alchemy (Math) - Opcodes 1-4, 10, 35-38, 44
Basic and advanced arithmetic operations.

### Runecraft (Logic) - Opcodes 11-12, 39-43
Bitwise operations and logical comparisons.

### Traversal (Movement) - Opcodes 5-9, 32-33, 58-60, 80-81
Grid navigation and teleportation.

### Inventory (Stack) - Opcodes 16-19, 26-29
Stack manipulation primitives.

### Scribing (Grid I/O) - Opcodes 20-21, 25, 56-57, 74
Reading and writing to the grid.

### Divination (Sensing) - Opcodes 52, 55, 61, 65, 73, 90-92
Information gathering about the world.

### Rituals (Control Flow) - Opcodes 13-15, 22-23, 34, 45-49, 64, 66-67
Jumps, calls, loops, and conditionals.

### Prophecy (Output) - Opcodes 24, 30-31
Generating output visible to users.

### Conjuration (Lifecycle) - Opcodes 53-54, 62-63, 70-71, 75-76
Creating/destroying wizards and owls.

### Harvest & Resources - Opcodes 82-87, 91-93
Collecting and managing resources.

### Memory Crystals - Opcodes 101-102
Named variable storage.

### Work Orders - Opcodes 103-105
Queue data structure.

### Omens - Opcodes 107-108
Event-driven programming.

### Arcane Arrays - Opcodes 110-112
Functional programming primitives.

---

## Alchemy (Math)

### ALCHEMY_ADD (1)
**Stack:** `a b → (a+b)`  
**Description:** Pops two values, pushes their sum.  
**Example:**
```
5 3 ALCHEMY_ADD  → 8
```

### ALCHEMY_SUB (2)
**Stack:** `a b → (a-b)`  
**Description:** Pops two values, pushes their difference.  
**Example:**
```
10 3 ALCHEMY_SUB  → 7
```

### ALCHEMY_MUL (3)
**Stack:** `a b → (a*b)`  
**Description:** Pops two values, pushes their product.  
**Example:**
```
4 5 ALCHEMY_MUL  → 20
```

### ALCHEMY_DIV (4)
**Stack:** `a b → (a÷b)`  
**Description:** Integer division. Pops two values, pushes quotient.  
**Example:**
```
17 5 ALCHEMY_DIV  → 3
```

### ALCHEMY_MOD (10)
**Stack:** `a b → (a mod b)`  
**Description:** Modulo operation. Pops two values, pushes remainder.  
**Example:**
```
17 5 ALCHEMY_MOD  → 2
```

### ALCHEMY_ABS (35)
**Stack:** `a → |a|`  
**Description:** Absolute value. Pops value, pushes its absolute value.  
**Example:**
```
-5 ALCHEMY_ABS  → 5
```

### ALCHEMY_NEG (36)
**Stack:** `a → -a`  
**Description:** Negation. Pops value, pushes its negative.  
**Example:**
```
5 ALCHEMY_NEG  → -5
```

### ALCHEMY_MIN (37)
**Stack:** `a b → min(a,b)`  
**Description:** Minimum. Pops two values, pushes the smaller.  
**Example:**
```
5 3 ALCHEMY_MIN  → 3
```

### ALCHEMY_MAX (38)
**Stack:** `a b → max(a,b)`  
**Description:** Maximum. Pops two values, pushes the larger.  
**Example:**
```
5 3 ALCHEMY_MAX  → 5
```

### ALCHEMY_POW (44)
**Stack:** `base exp → (base^exp)`  
**Description:** Exponentiation. Pops base and exponent, pushes power.  
**Example:**
```
2 10 ALCHEMY_POW  → 1024
```

---

## Runecraft (Logic)

### RUNE_ISZERO (11)
**Stack:** `a → (a==0 ? 1 : 0)`  
**Description:** Tests if top of stack is zero. Pushes 1 if zero, 0 otherwise.  
**Example:**
```
0 RUNE_ISZERO  → 1
5 RUNE_ISZERO  → 0
```

### RUNE_GT (12)
**Stack:** `a b → (a>b ? 1 : 0)`  
**Description:** Greater than comparison. Pushes 1 if a > b, else 0.  
**Example:**
```
5 3 RUNE_GT  → 1
3 5 RUNE_GT  → 0
```

### RUNE_AND (39)
**Stack:** `a b → (a & b)`  
**Description:** Bitwise AND.  
**Example:**
```
12 10 RUNE_AND  → 8  (1100 & 1010 = 1000)
```

### RUNE_OR (40)
**Stack:** `a b → (a | b)`  
**Description:** Bitwise OR.  
**Example:**
```
12 10 RUNE_OR  → 14  (1100 | 1010 = 1110)
```

### RUNE_XOR (41)
**Stack:** `a b → (a ^ b)`  
**Description:** Bitwise XOR.  
**Example:**
```
12 10 RUNE_XOR  → 6  (1100 ^ 1010 = 0110)
```

### RUNE_SHL (42)
**Stack:** `value shift → (value << shift)`  
**Description:** Bitwise left shift.  
**Example:**
```
5 2 RUNE_SHL  → 20  (101 << 2 = 10100)
```

### RUNE_SHR (43)
**Stack:** `value shift → (value >> shift)`  
**Description:** Bitwise right shift.  
**Example:**
```
20 2 RUNE_SHR  → 5  (10100 >> 2 = 101)
```

---

## Traversal (Movement)

### MOVE_EAST (5)
**Stack:** (unchanged)  
**Description:** Changes direction to +column (East).  
**Effect:** Next step moves wizard one column to the right.

### MOVE_WEST (6)
**Stack:** (unchanged)  
**Description:** Changes direction to -column (West).  
**Effect:** Next step moves wizard one column to the left.

### MOVE_SOUTH (7)
**Stack:** (unchanged)  
**Description:** Changes direction to +row (South).  
**Effect:** Next step moves wizard one row down.

### MOVE_NORTH (8)
**Stack:** (unchanged)  
**Description:** Changes direction to -row (North).  
**Effect:** Next step moves wizard one row up.

### MOVE_ASCEND (32)
**Stack:** (unchanged)  
**Description:** Changes direction to +layer (Up).  
**Effect:** Next step moves wizard up one layer.

### MOVE_DESCEND (33)
**Stack:** (unchanged)  
**Description:** Changes direction to -layer (Down).  
**Effect:** Next step moves wizard down one layer.

### WANDER (9)
**Stack:** (unchanged)  
**Description:** Sets random direction (one of 6 cardinal directions).  
**Use case:** Exploration, randomized behavior.

### MIRROR (58)
**Stack:** (unchanged)  
**Description:** Reverses current direction.  
**Example:** If moving East, now moves West.

### ORIENT (59)
**Stack:** `dl dr dc → (changes direction)`  
**Description:** Pops 3 values and sets direction vector directly.  
**Example:**
```
1 0 0 ORIENT  → moves in +layer direction
```

### STRIDE (60)
**Stack:** `n → (unchanged)`  
**Description:** Pops n; moves n steps in current direction immediately.  
**Example:**
```
5 STRIDE  → jumps 5 cells forward
```

### LEAP (80)
**Stack:** `l r c → (changes position)`  
**Description:** Pops layer, row, column; teleports to absolute position.  
**Example:**
```
0 5 10 LEAP  → teleport to (0,5,10)
```

### PORTAL (81)
**Stack:** `dl dr dc → (changes position)`  
**Description:** Pops relative offsets; teleports by offset from current position.  
**Example:**
```
0 2 3 PORTAL  → jump 2 rows, 3 columns from current position
```

---

## Inventory (Stack)

### INV_CLONE (16)
**Stack:** `a → a a`  
**Description:** Duplicates top of stack.  
**Example:**
```
5 INV_CLONE  → 5 5
```

### INV_SWAP (17)
**Stack:** `a b → b a`  
**Description:** Swaps top two stack values.  
**Example:**
```
3 7 INV_SWAP  → 7 3
```

### INV_DROP (18)
**Stack:** `a → (empty)`  
**Description:** Removes top of stack.  
**Example:**
```
5 INV_DROP  → (stack empty)
```

### INV_NIP (19)
**Stack:** `a b → b`  
**Description:** Removes second item from top.  
**Example:**
```
3 7 INV_NIP  → 7
```

### INV_CLONE2 (26)
**Stack:** `a b → a b a b`  
**Description:** Duplicates top two values.  
**Example:**
```
3 5 INV_CLONE2  → 3 5 3 5
```

### INV_OVER (27)
**Stack:** `a b → a b a`  
**Description:** Copies second item to top.  
**Example:**
```
3 5 INV_OVER  → 3 5 3
```

### INV_ROT (28)
**Stack:** `a b c → b c a`  
**Description:** Rotates top three items.  
**Example:**
```
1 2 3 INV_ROT  → 2 3 1
```

### INV_DEPTH (29)
**Stack:** `→ n`  
**Description:** Pushes current stack depth.  
**Example:**
```
(with 3 items) INV_DEPTH  → 3
```

---

## Scribing (Grid I/O)

### SCRIBE_COPY (20)
**Stack:** (unchanged)  
**Description:** Reads current cell value, pushes to stack.  
**Use:** Non-destructive read from current position.

### SCRIBE_READ (21)
**Stack:** (unchanged)  
**Description:** Reads current cell, pushes to stack, zeros the cell.  
**Use:** Destructive read, consumes grid value.

### SCRIBE_WRITE (25)
**Stack:** `value → (empty)`  
**Description:** Pops value, writes to current cell.  
**Use:** Store data on grid.

### SCRIBE_MARK (56)
**Stack:** `l r c value → (empty)`  
**Description:** Pops position and value, writes value to that position.  
**Use:** Write to arbitrary grid location.

### SCRIBE_ERASE (57)
**Stack:** `l r c → (empty)`  
**Description:** Pops position, zeros that cell.  
**Use:** Clear specific grid location.

### SCRIBE_SIG (74)
**Stack:** `l r c → signature`  
**Description:** Computes hash of grid contents in 5×5×5 region around position.  
**Use:** Detect grid changes, spatial pattern matching.

---

## Divination (Sensing)

### DIVINE_SCAN (55)
**Stack:** `→ value`  
**Description:** Pushes value from next cell in movement direction.  
**Use:** Look ahead before moving.

### DIVINE_SENSE (73)
**Stack:** `dl dr dc → value`  
**Description:** Pops relative offset, pushes value at that offset.  
**Use:** Examine nearby cells.

### DIVINE_OWLS (61)
**Stack:** `→ count`  
**Description:** Pushes number of active owls belonging to this wizard.  
**Use:** Track sent messages.

### DIVINE_PEERS (52)
**Stack:** `radius → wiz_data`  
**Description:** Pops radius, pushes info about wizards within range.  
**Returns:** 4-tuple per wizard: (ip, wid, status, role)  
**Use:** Multi-agent coordination.

### DIVINE_TIME (65)
**Stack:** `→ tick`  
**Description:** Pushes current simulation tick.  
**Use:** Timing, synchronization.

### DIVINE_TOOL (90)
**Stack:** `tool_id → (0 or 1)`  
**Description:** Checks if wizard has tool in inventory.  
**Returns:** 1 if present, 0 if absent.

### DIVINE_TERRAIN (91)
**Stack:** `→ terrain_type`  
**Description:** Pushes terrain type of current cell.  
**Returns:** WATER, STONE_TILE, ORE_TILE, GRASS, or 0.

### DIVINE_ROLE (92)
**Stack:** `→ role_id`  
**Description:** Pushes wizard's current role.  
**Returns:** ROLE_WIZARD, ROLE_FARMER, etc.

---

## Rituals (Control Flow)

### RITUAL_INVOKE (22)
**Stack:** `addr → (pushes return marker)`  
**Description:** Pops address, calls subroutine. Pushes FRAME_MARK and return address.  
**Use:** Function calls.

### RITUAL_RETURN (23)
**Stack:** `→ (pops to frame, returns)`  
**Description:** Returns from subroutine. Pops stack until FRAME_MARK, jumps to return address.  
**Use:** End of function.

### RITUAL_WARP (46)
**Stack:** `addr → (changes IP)`  
**Description:** Unconditional jump. Pops address, sets IP to it.  
**Use:** Goto, infinite loops.

### RITUAL_CYCLE (49)
**Stack:** `addr count → (may jump)`  
**Description:** Loop construct. Pops count and address. Decrements count; if >0, jumps to address and re-pushes count-1 and address.  
**Use:** For loops.

### RITUAL_SKIP1 (15)
**Stack:** (unchanged)  
**Description:** Skip next instruction.  
**Use:** Simple conditionals.

### RITUAL_SKIPN (45)
**Stack:** `n → (empty)`  
**Description:** Pops n, skips n instructions.  
**Use:** Multi-instruction skipping.

### RITUAL_IFZERO (47)
**Stack:** `value addr → (may jump)`  
**Description:** If value==0, jumps to addr.  
**Use:** Conditional branching.

### RITUAL_IFPOS (48)
**Stack:** `value addr → (may jump)`  
**Description:** If value>0, jumps to addr.  
**Use:** Positive number checks.

### RITUAL_IFEW (13)
**Stack:** `→ (may skip)`  
**Description:** If current direction is East or West, skip next instruction.  
**Use:** Direction-based logic.

### RITUAL_IFNS (14)
**Stack:** `→ (may skip)`  
**Description:** If current direction is North or South, skip next instruction.

### RITUAL_IFUD (34)
**Stack:** `→ (may skip)`  
**Description:** If current direction is Up or Down, skip next instruction.

### RITUAL_CHAOS (64)
**Stack:** `→ random_value`  
**Description:** Pushes random number (0-99).  
**Use:** Randomness, probability.

### RITUAL_SIGIL (66)
**Stack:** `→ (marks position)`  
**Description:** Creates a named checkpoint at current position.  
**Use:** Save points for later jumps.

### RITUAL_OATH (67)
**Stack:** `sigil_name → (may jump)`  
**Description:** Jumps to previously marked sigil position.  
**Use:** Long-distance jumps, labeled positions.

---

## Prophecy (Output)

### PROPHECY_STR (24)
**Stack:** `len → (empty)`  
**Description:** Pops length n, pops n values as ASCII codes, appends string to output.  
**Example:**
```
72 101 108 108 111  5 PROPHECY_STR  → outputs "Hello"
```

### PROPHECY_NUM (30)
**Stack:** `value → (empty)`  
**Description:** Pops value, appends its decimal representation to output.  
**Example:**
```
42 PROPHECY_NUM  → outputs "42"
```

### PROPHECY_CHAR (31)
**Stack:** `ascii → (empty)`  
**Description:** Pops ASCII code, appends character to output.  
**Example:**
```
65 PROPHECY_CHAR  → outputs "A"
```

---

## Conjuration (Lifecycle)

### FORK / SUMMON_CLONE (106 / 70)
**Stack:** (unchanged)  
**Description:** Creates exact copy of wizard with new ID at same position.  
**Use:** Parallelism, multi-agent tasks.

### BANISH_SELF (71)
**Stack:** (unchanged)  
**Description:** Wizard dies, drops apples at current position.  
**Use:** Controlled termination.

### MEDITATE (54)
**Stack:** (unchanged)  
**Description:** Wizard does nothing this turn.  
**Use:** Waiting, yielding.

### CATACLYSM (75)
**Stack:** (unchanged)  
**Description:** Halts entire simulation immediately.  
**Use:** Program termination.

### SEND_OWL (53)
**Stack:** `target_wid value radius → (empty)`  
**Description:** Sends value to wizard with target_wid within radius.  
**Use:** Direct messaging.

### SCREAM_OWL (63)
**Stack:** `value radius → (empty)`  
**Description:** Broadcasts value to all wizards within radius.  
**Use:** Area messaging.

### CONJURE_OWL (76)
**Stack:** `lifetime signal radius → (empty)`  
**Description:** Creates owl that travels in current direction. On death, broadcasts signal within radius.  
**Use:** Delayed messages, projectiles (negative signal = damage).

### RECALL_OWLS (62)
**Stack:** `→ (terminates owls)`  
**Description:** Kills all owls belonging to this wizard.  
**Use:** Cleanup, message cancellation.

---

## Harvest & Resources

### APPLE_COLLECT (82)
**Stack:** `→ (gains apple)`  
**Description:** Picks up one apple from current cell if available.  
**Effect:** Current cell value decreases by 1, wizard apples increase by 1.

### APPLE_DROP (83)
**Stack:** `→ (loses apple)`  
**Description:** Drops one apple at current position.  
**Effect:** Wizard apples decrease by 1, cell value increases by 1.

### SEND_APPLE (84)
**Stack:** `target_wid amount radius → (empty)`  
**Description:** Transfers apples to another wizard within radius.  
**Use:** Resource sharing, cooperation.

### HARVEST_TREE (85)
**Stack:** `→ wood_count`  
**Description:** If on TREE cell (value ≥ TREE_VALUE), harvests wood.  
**Effect:** Adds WOOD items to inventory, pushes count.

### CHOP (86)
**Stack:** `→ wood_count`  
**Description:** With AXE, harvests tree and gets wood.  
**Requires:** AXE in inventory.

### MINE (87)
**Stack:** `→ resource`  
**Description:** With PICKAXE, mines ORE_TILE for METAL or STONE.  
**Requires:** PICKAXE in inventory.

### PLANT_TREE (91)
**Stack:** `→ (creates tree)`  
**Description:** Farmers can plant trees on GRASS/FARMLAND.  
**Requires:** ROLE_FARMER.

---

## Memory Crystals (Variables)

### BIND (101)
**Stack:** `name value → (empty)`  
**Description:** Pops name (as number), pops value, stores in wizard's environment.  
**Use:** Named storage.  
**Example:**
```
42 100 BIND      → env[42] = 100
65 500 BIND      → env[65] = 500
```

### FETCH (102)
**Stack:** `name → value`  
**Description:** Pops name, pushes stored value (or 0 if not found).  
**Use:** Variable retrieval.  
**Example:**
```
42 FETCH  → pushes 100 (from above)
99 FETCH  → pushes 0 (not set)
```

---

## Work Orders (Queues)

### ENQUEUE (103)
**Stack:** `value → (empty)`  
**Description:** Pops value, appends to wizard's queue.  
**Use:** Task scheduling, FIFO buffer.

### DEQUEUE (104)
**Stack:** `→ value`  
**Description:** Removes and pushes first queue item (0 if empty).  
**Use:** Process next task.

### QUEUE_DEPTH (105)
**Stack:** `→ size`  
**Description:** Pushes current queue length.  
**Use:** Check queue status.

**Example usage:**
```
10 ENQUEUE
20 ENQUEUE
30 ENQUEUE
QUEUE_DEPTH    → pushes 3
DEQUEUE        → pushes 10
DEQUEUE        → pushes 20
QUEUE_DEPTH    → pushes 1
```

---

## Omens (Events)

### Kingdom Events
The kingdom generates random events at intervals:
- EVENT_GOBLIN_RAID (1)
- EVENT_BOUNTY (2)
- EVENT_PLAGUE (3)
- EVENT_METEOR (4)
- EVENT_PROSPERITY (5)
- EVENT_DAWN (6)
- EVENT_DUSK (7)
- EVENT_FULL_MOON (8)
- EVENT_EARTHQUAKE (9)

### AWAIT_OMEN (107)
**Stack:** `event_id → (suspends)`  
**Description:** Wizard sleeps until specified event occurs. Status becomes negative.  
**Use:** Event-driven waiting.  
**Example:**
```
EVENT_DAWN AWAIT_OMEN  → wizard waits for dawn
```

### REACT (108)
**Stack:** `event_id handler_addr → (empty)`  
**Description:** Registers event handler. When event occurs, IP jumps to handler_addr.  
**Use:** Asynchronous event handling.  
**Example:**
```
EVENT_GOBLIN_RAID 1000 REACT  → when raid occurs, jump to position 1000
```

---

## Arcane Arrays (Higher-Order)

### MAP (110)
**Stack:** `subroutine_addr [a, b, c, ...] → [f(a), f(b), f(c), ...]`  
**Description:** Applies subroutine to each stack element, creates new array.  
**Use:** Transform collections.  
**Example:**
```
NB Subroutine at addr 100: INV_CLONE ALCHEMY_MUL RITUAL_RETURN (square function)
1 2 3 4 5  100 MAP  → 1 4 9 16 25
```

### FILTER (111)
**Stack:** `subroutine_addr [a, b, c, ...] → [elements where f(x) ≠ 0]`  
**Description:** Keeps only elements where subroutine returns non-zero.  
**Use:** Select items matching criteria.  
**Example:**
```
NB Subroutine at 200: 2 ALCHEMY_MOD RITUAL_RETURN (odd check)
1 2 3 4 5 6  200 FILTER  → 1 3 5
```

### REDUCE (112)
**Stack:** `subroutine_addr init [a, b, c, ...] → result`  
**Description:** Accumulates array using binary operation, starting with init.  
**Use:** Aggregate data (sum, product, etc.).  
**Example:**
```
NB Subroutine at 300: ALCHEMY_ADD RITUAL_RETURN (addition)
0 1 2 3 4 5  300 REDUCE  → 15 (sum)
```

---

## Roles & Kingdom

### Roles

| Role | ID | Special Abilities | Apple Cost/Tick |
|------|----|--------------------|----------------|
| WIZARD | 0 | General magic | 1 |
| FARMER | 1 | Plant trees, better harvest | 1 |
| WARRIOR | 2 | Combat bonuses | 2 |
| MERCHANT | 3 | Trade at shops | 1 |
| BUILDER | 4 | Construct structures | 1 |
| SCRIBE | 5 | Advanced grid manipulation | 0 |
| GUARDIAN | 6 | Territory defense | 0 |

### MY_ROLE (94)
**Stack:** `→ role_id`  
**Description:** Pushes current role.

### CHANGE_ROLE (95)
**Stack:** `new_role → (empty)`  
**Description:** Changes wizard's role.

### ROLE_SENSE (96)
**Stack:** `role_id → (data)`  
**Description:** Senses wizards of specific role.

### Kingdom Stats

### KINGDOM_POP (97)
**Stack:** `→ population`  
**Description:** Pushes total wizard count.

### KINGDOM_APPLES (98)
**Stack:** `→ total_apples`  
**Description:** Pushes kingdom's total apples.

### KINGDOM_GOLD (99)
**Stack:** `→ total_gold`  
**Description:** Pushes kingdom's total gold.

### KINGDOM_EVENT (100)
**Stack:** `→ last_event_id`  
**Description:** Pushes ID of most recent event.

---

## Constants Reference

### Items
```
AXE           = 1
SWORD         = 2
SHIELD        = 3
PICKAXE       = 4
BOAT          = 5
IRON_SWORD    = 6
TOWER_SHIELD  = 7
```

### Resources
```
WOOD    = 101
STONE   = 102
METAL   = 103
GOLD    = 104
ESSENCE = 105
```

### Terrain
```
WATER      = 3001
STONE_TILE = 3002
ORE_TILE   = 3003
GRASS      = 3004
TREE_VALUE = 1000  (cell value ≥ 1000 = tree)
```

### Structures
```
WALL     = 4001
DOOR     = 4002
FARMLAND = 4003
SHOP     = 4004
```

### Special Values
```
FRAME_MARK = 999999  (subroutine return marker)
IMMORTAL   = -1      (wizard never loses apples)
SHORT_RAD  = 5       (default sensing radius)
```

---

## Execution Model

### Wizard Turn Cycle
1. Fetch instruction from current IP
2. Execute opcode
3. Consume apples based on role (if not IMMORTAL)
4. Move in current direction
5. Die if apples ≤ 0

### Multi-Agent Execution
- All wizards execute one instruction per round
- Owls move and broadcast after wizards
- Kingdom events processed after all agents
- Dead agents removed at end of round

### Grid Coordinates
- Layer (L): Vertical axis (up/down)
- Row (R): North/South axis
- Column (C): East/West axis
- Position represented as 3-tuple: (L, R, C)

---

## Programming Tips

1. **Stack Management**: Use INV_CLONE liberally to preserve values
2. **Apples**: Monitor apple count, use IMMORTAL for testing
3. **Subroutines**: Always end with RITUAL_RETURN, mark entry with FRAME_MARK
4. **Events**: Combine AWAIT_OMEN and REACT for robust event handling
5. **Parallelism**: FORK creates independent wizards - they don't share stacks
6. **Queues**: Useful for task management in complex wizards
7. **Variables**: Use sequential numbers as variable names for simplicity

---

*For examples and tutorials, see [Tutorial](TUTORIAL.md) and [examples/](../examples/).*
