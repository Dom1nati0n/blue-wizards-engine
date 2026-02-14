# Blue Wizards Engine Architecture

Technical overview of the Blue Wizards Engine V29 implementation.

## Table of Contents

1. [Overview](#overview)
2. [Data Structures](#data-structures)
3. [Execution Model](#execution-model)
4. [Grid System](#grid-system)
5. [Wizard Lifecycle](#wizard-lifecycle)
6. [Owl System](#owl-system)
7. [Event System](#event-system)
8. [Advanced Features](#advanced-features)
9. [Performance Considerations](#performance-considerations)

---

## Overview

Blue Wizards is implemented in J, a powerful array programming language. The engine consists of:

- **Grid management** - Dynamic 3D array representing the world
- **Wizard execution** - Multi-agent instruction processing
- **Owl messaging** - Inter-wizard communication
- **Event system** - Kingdom-wide event generation and handling
- **Resource management** - Apples, items, terrain interaction

### Core Design Principles

1. **Agent-based execution** - Each wizard is an independent agent
2. **Grid-as-code** - The program and data share the same space
3. **Turn-based simulation** - All agents act once per round
4. **Functional core** - Heavy use of J's array operations
5. **Stateful agents** - Wizards maintain local state (stack, variables, queue)

---

## Data Structures

### Wizard Tuple (13 fields)

```j
wizard =: ip ; dir ; stk ; apples ; wid ; pid ; status ; out ; items ; role ; env ; queue ; react
```

| Field | Type | Description |
|-------|------|-------------|
| `ip` | 3-vector | Instruction pointer (layer, row, column) |
| `dir` | 3-vector | Direction vector for movement |
| `stk` | list | Data stack for computation |
| `apples` | integer | Energy/health points |
| `wid` | integer | Unique wizard ID |
| `pid` | integer | Parent wizard ID (for FORK) |
| `status` | integer | 0=dead, 1=alive, <0=waiting for event |
| `out` | string | Output buffer |
| `items` | list | Inventory (item IDs) |
| `role` | integer | Current role (WIZARD, FARMER, etc.) |
| `env` | dictionary | Variable bindings (name→value) |
| `queue` | list | Work order queue |
| `react` | list of pairs | Event handlers (eventID, address) |

### Owl Tuple (8 fields)

```j
owl =: ip ; dir ; lifetime ; owner ; radius ; signal ; status ; backpack
```

| Field | Type | Description |
|-------|------|-------------|
| `ip` | 3-vector | Current position |
| `dir` | 3-vector | Direction of travel |
| `lifetime` | integer | Steps remaining before expiration |
| `owner` | integer | Wizard ID that created it |
| `radius` | integer | Broadcast radius on death |
| `signal` | integer | Value to broadcast (negative = damage) |
| `status` | integer | 0=dead, 1=alive |
| `backpack` | integer | Apples carried (dropped on death) |

### Grid Structure

```j
grid =: (layers, rows, columns) $ 0
```

- 3D integer array
- Dynamically expands as needed
- Stores opcodes, data, resources, terrain markers

### Global State

```j
wizards =: list of wizard tuples
owls =: list of owl tuples
sim_tick =: current timestep
kingdom_pop =: wizard count
kingdom_apples =: total apples
kingdom_gold =: total gold
kingdom_last_event =: most recent event ID
```

---

## Execution Model

### Round Structure

Each simulation round consists of these phases:

```
1. Wizard Execution
   ├─ For each wizard:
   │  ├─ Fetch opcode at IP
   │  ├─ Execute instruction
   │  ├─ Consume apples (role-dependent)
   │  ├─ Move in current direction
   │  └─ Generate messages (owls, sends)
   
2. Message Processing
   ├─ Process SEND_OWL messages (direct sends)
   └─ Process SEND_APPLE messages (resource transfers)
   
3. Owl Summoning
   └─ Create new owls from CONJURE_OWL calls
   
4. Owl Execution
   ├─ For each owl:
   │  ├─ Move in direction
   │  ├─ Decrement lifetime
   │  └─ Broadcast on death (if signal ≠ 0)
   
5. Owl Broadcasts
   ├─ For each broadcast:
   │  └─ Affect wizards in radius
   │     ├─ Negative signal: damage
   │     └─ Positive signal: push to stack
   
6. Kingdom Update
   ├─ Update population stats
   ├─ Update resource totals
   ├─ Generate random events
   ├─ Wake wizards waiting for events
   └─ Trigger event handlers (REACT)
   
7. Cleanup
   ├─ Remove dead wizards
   └─ Remove dead owls
```

### Instruction Execution

The core execution loop (simplified):

```j
step_wizard =: 3 : 0
  'ip dir stk apples wid pid status out items role env queue react' =. y
  
  NB Fetch opcode
  opcode =. cell_at ip
  
  NB Execute based on opcode
  select. opcode
    case. ALCHEMY_ADD do.
      'a b' =. 2 {. stk
      new_stk =. (2 }. stk) , (a + b)
    case. MOVE_EAST do.
      new_dir =. 0 0 1
    case. PROPHECY_NUM do.
      val =. {. stk
      new_out =. out , ": val
      new_stk =. }. stk
    NB ... (all opcodes)
  end.
  
  NB Move wizard
  new_ip =. ip + dir
  
  NB Consume apples
  new_apples =. apples - cost_for_role
  
  NB Return updated state
  new_ip ; new_dir ; new_stk ; new_apples ; ...
)
```

---

## Grid System

### Dynamic Expansion

The grid grows automatically when wizards access out-of-bounds positions:

```j
ensure_bounds =: 3 : 0
  'l r c' =. y
  
  NB Calculate required bounds
  new_low_l =. low_l <. l
  new_high_l =. high_l >. l
  NB ... similar for rows and columns
  
  NB If bounds changed, create larger grid and copy
  if. changed do.
    new_grid =. (new_size) $ 0
    NB Copy old grid into new grid at offset
    new_grid[offset] =. grid
    grid =: new_grid
  end.
)
```

### Grid Operations

**Reading:** `cell_at position`
- Bounds-checks automatically
- Returns 0 for uninitialized cells

**Writing:** `value cell_put position`
- Expands grid if needed
- Updates cell value

**Sensing:** `cell_at (ip + offset)`
- Used by DIVINE_SENSE, DIVINE_SCAN
- Supports relative positioning

---

## Wizard Lifecycle

### Creation (spawn_wizard)

```j
spawn_wizard =: 3 : 0
  'pos dir stk apples pid role' =. y
  
  wid =. next_wiz_id
  next_wiz_id =: next_wiz_id + 1
  
  wizard =. pos ; dir ; stk ; apples ; wid ; pid ; 1 ; '' ; i.0 ; role ; empty_dict ; i.0 ; i.0
  wizards =: wizards , < wizard
)
```

### Fork (FORK/SUMMON_CLONE)

Creates complete copy of wizard:

```j
case. FORK do.
  new_wid =. next_wiz_id
  next_wiz_id =: next_wiz_id + 1
  
  NB Clone entire state with new ID
  clone =. ip ; dir ; stk ; apples ; new_wid ; wid ; status ; '' ; items ; role ; env ; queue ; react
  
  NB Add to wizard list
  spawn_clone clone
```

### Death

Wizards die when:
1. Apples ≤ 0
2. BANISH_SELF executed
3. Killed by owl damage

On death:
- Status set to 0
- Remaining apples dropped at position
- Half of apples awarded to killer (if applicable)

---

## Owl System

### Creation

Three ways to create owls:

1. **SEND_OWL** - Direct message to specific wizard
2. **SCREAM_OWL** - Broadcast to all nearby wizards
3. **CONJURE_OWL** - Create projectile that travels and broadcasts on death

### Message Delivery

**SEND_OWL:** 
```j
NB Pops: target_wid, value, radius
NB Creates message: ('send', target_wid, value, radius, origin_pos)
NB In processing phase, finds matching wizard within radius
NB If found: pushes value to their stack
```

**SCREAM_OWL:**
```j
NB Pops: value, radius
NB Immediately pushes value to all wizards within radius
```

**CONJURE_OWL:**
```j
NB Pops: lifetime, signal, radius
NB Creates traveling owl that:
NB   - Moves in wizard's current direction
NB   - Lives for 'lifetime' steps
NB   - On death, broadcasts 'signal' within 'radius'
NB   - If signal < 0: damages wizards
NB   - If signal > 0: pushes to their stacks
```

### Combat System

Damage owls (negative signal):

```j
if. signal < 0 do.
  dmg =. -signal
  
  NB Shield reduction
  if. wizard_has_shield do.
    dmg =. <. dmg % 2
  end.
  
  wizard_apples =. wizard_apples - dmg
  
  NB Death and loot
  if. wizard_apples <= 0 do.
    wizard_status =. 0
    drop_apples_at_position
    award_half_apples_to_attacker
  end.
end.
```

---

## Event System

### Event Generation

Random events occur at irregular intervals:

```j
update_kingdom =: 3 : 0
  event_timer =: event_timer + 1
  
  if. event_timer > (80 + ?40) do.  NB 80-120 ticks
    event_timer =: 0
    event_id =. 1 + ?9  NB Random event 1-9
    kingdom_last_event =: event_id
    
    NB Wake waiting wizards
    NB Trigger registered handlers
  end.
)
```

### Event Types

```j
EVENT_GOBLIN_RAID  = 1   NB External threat
EVENT_BOUNTY       = 2   NB Resource bonus
EVENT_PLAGUE       = 3   NB Health reduction
EVENT_METEOR       = 4   NB Random destruction
EVENT_PROSPERITY   = 5   NB Economic boost
EVENT_DAWN         = 6   NB Time of day
EVENT_DUSK         = 7   NB Time of day
EVENT_FULL_MOON    = 8   NB Magical event
EVENT_EARTHQUAKE   = 9   NB Terrain disruption
```

### Event Handling

**AWAIT_OMEN:**
```j
case. AWAIT_OMEN do.
  event_id =. {. stk
  new_status =. - event_id  NB Negative status = waiting
  new_stk =. }. stk
  
  NB Wizard sleeps until event occurs
  NB When event triggers, status → 1 (alive)
```

**REACT:**
```j
case. REACT do.
  'event_id handler_addr' =. 2 {. stk
  new_stk =. 2 }. stk
  new_react =. react , < (event_id ; handler_addr)
  
  NB When event occurs:
  NB   - Set IP to handler_addr
  NB   - Wake wizard (status = 1)
```

---

## Advanced Features

### Memory Crystals (Variables)

Implemented as boxed dictionary in wizard state:

```j
case. BIND do.
  'name value' =. 2 {. stk
  new_stk =. 2 }. stk
  new_env =. env_set(env, name, value)

case. FETCH do.
  name =. {. stk
  value =. env_get(env, name, 0)  NB default: 0
  new_stk =. (}. stk) , value
```

### Work Order Scrolls (Queues)

Simple list append/pop:

```j
case. ENQUEUE do.
  value =. {. stk
  new_stk =. }. stk
  new_queue =. queue , value

case. DEQUEUE do.
  if. #queue > 0 do.
    value =. {. queue
    new_queue =. }. queue
  else.
    value =. 0
  end.
  new_stk =. stk , value
```

### Higher-Order Functions

**MAP:**
```j
case. MAP do.
  sub_addr =. {. stk
  array =. }. stk
  result =. i.0
  
  for_each. array do.
    NB Call subroutine with element
    sub_result =. call_subroutine(sub_addr, element)
    result =. result , sub_result
  end.
  
  new_stk =. result
```

**FILTER:**
```j
case. FILTER do.
  sub_addr =. {. stk
  array =. }. stk
  result =. i.0
  
  for_each. array do.
    keep =. call_subroutine(sub_addr, element)
    if. keep ~: 0 do.
      result =. result , element
    end.
  end.
  
  new_stk =. result
```

**REDUCE:**
```j
case. REDUCE do.
  'sub_addr init' =. 2 {. stk
  array =. 2 }. stk
  accumulator =. init
  
  for_each. array do.
    accumulator =. call_subroutine(sub_addr, accumulator, element)
  end.
  
  new_stk =. , accumulator
```

---

## Performance Considerations

### Optimization Strategies

1. **Boxed arrays** - Wizards and owls stored as boxed lists for efficient updates
2. **Selective grid expansion** - Grid only grows when necessary
3. **Batch processing** - All wizards execute in parallel, then all owls
4. **Lazy evaluation** - J's array operations are highly optimized
5. **Minimal copying** - Most operations modify in-place where possible

### Complexity Analysis

| Operation | Time Complexity | Space Complexity |
|-----------|----------------|------------------|
| Wizard step | O(1) per opcode | O(stack depth) |
| Grid access | O(1) | O(grid volume) |
| Grid expansion | O(new volume) | O(new volume) |
| Owl broadcast | O(wizards) | O(1) |
| Event processing | O(wizards) | O(1) |
| Full round | O(wizards + owls) | O(total state) |

### Scalability Limits

- **Max wizards**: 200 (configurable)
- **Max owls**: 1000 (configurable)
- **Grid size**: Limited by available memory
- **Simulation length**: Unlimited

### Memory Usage

Approximate memory per agent:
- Wizard: ~500 bytes base + stack + variables + queue
- Owl: ~200 bytes
- Grid cell: 8 bytes (J integer)

For 100 wizards, 100 owls, 100×100×10 grid:
- Wizards: ~50 KB
- Owls: ~20 KB
- Grid: ~800 KB
- **Total: ~1 MB**

---

## Implementation Notes

### J Language Features Used

1. **Verb trains** - Function composition
2. **Array operations** - Bulk data processing
3. **Boxing** - Heterogeneous data structures
4. **Explicit definitions** - Imperative code for complex logic
5. **Gerunds** - Lists of verbs for dispatch tables

### Design Trade-offs

**Grid expansion vs. pre-allocation:**
- Chose dynamic expansion for flexibility
- Trade-off: occasional reallocation cost

**Wizard state as tuple vs. object:**
- Chose tuple for J compatibility
- Trade-off: Less type safety, more manual field access

**Turn-based vs. continuous:**
- Chose turn-based for determinism
- Trade-off: Less real-time feel

**Global state vs. pure functional:**
- Chose mutable global state for performance
- Trade-off: Less referential transparency

---

## Extension Points

### Adding New Opcodes

1. Define constant in constants section
2. Add case in `step_wizard` select block
3. Implement stack/state transformation
4. Update documentation

### Custom Terrain Types

1. Add constant (value > TERRAIN_BASE)
2. Implement interaction in movement/sensing code
3. Update terrain query opcodes

### New Event Types

1. Add event constant
2. Extend event generation logic
3. Document event behavior

---

## Debugging and Profiling

### Built-in Tools

```j
wizard_summary ''      NB Show all wizard states
get_outputs ''         NB Collect all wizard outputs
view_grid ''           NB Display grid contents (custom)
```

### J Profiling

```j
6!:2 'run_multi program;1;100;10'  NB Time execution
7!:2 'run_multi program;1;100;10'  NB Memory usage
```

### Common Debugging Patterns

1. **Trace execution:** Insert PROPHECY_NUM to output intermediate values
2. **Inspect state:** Call `wizard_summary` between steps
3. **Single-step:** Run with max_steps=1
4. **Grid dump:** Visualize grid to see code/data layout

---

*For more details, see the source code in `engine/blue_wizards_v29.ijs`*
