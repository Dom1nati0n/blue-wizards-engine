# Blue Wizards Examples

This directory contains example programs demonstrating various features of the Blue Wizards Engine.

## Running Examples

To run any example:

1. Load the Blue Wizards engine in J:
   ```j
   load 'engine/blue_wizards_v29.ijs'
   ```

2. Load and run an example:
   ```j
   load 'examples/hello_world.bw'
   ```

The output will be displayed in the J console.

## Example Programs

### 🌟 Beginner Examples

#### hello_world.bw
**Difficulty:** Beginner  
**Concepts:** Basic output, ASCII characters, PROPHECY_STR

The classic first program. Outputs "Hello, World!" using character codes and the string output opcode.

**Key Features:**
- ASCII character encoding
- PROPHECY_STR for string output
- Basic program structure

---

#### calculator.bw
**Difficulty:** Beginner  
**Concepts:** Arithmetic, stack manipulation, numeric output

Demonstrates all basic math operations (addition, subtraction, multiplication, division, modulo, power, absolute value, min, max) plus a complex expression.

**Key Features:**
- All ALCHEMY opcodes (math operations)
- Stack-based computation
- PROPHECY_NUM for numeric output
- Sequential operations

---

### 🧭 Intermediate Examples

#### maze_explorer.bw
**Difficulty:** Intermediate  
**Concepts:** Movement, sensing, conditionals, grid interaction

A wizard navigates through a simple maze using sensing to detect walls and the goal.

**Key Features:**
- DIVINE_SCAN for looking ahead
- MOVE_* opcodes for navigation
- RITUAL_IFZERO and RITUAL_IFPOS for conditionals
- RITUAL_WARP for loops
- MIRROR for direction reversal
- Grid interaction and pathfinding logic

**Maze Layout:**
```
S -> -> -> W -> -> -> -> G
(S=Start, W=Wall, G=Goal)
```

---

### 🔮 Advanced Examples

#### parallel_harvesters.bw
**Difficulty:** Advanced  
**Concepts:** Multi-agent, parallelism, resource gathering, teleportation

Multiple wizards are created via FORK and each harvests apples from different locations in the world.

**Key Features:**
- FORK for creating wizard clones
- LEAP for teleportation to specific coordinates
- APPLE_COLLECT for resource gathering
- Independent execution of multiple wizards
- DIVINE_TIME for creating variation
- Role management (ROLE_FARMER)
- BANISH_SELF for controlled termination

---

#### reactive_kingdom.bw
**Difficulty:** Advanced  
**Concepts:** Event-driven programming, asynchronous behavior, handlers

Demonstrates the event system with wizards that respond to kingdom events using both REACT (event handlers) and AWAIT_OMEN (blocking waits).

**Key Features:**
- REACT for registering event handlers
- AWAIT_OMEN for blocking until event occurs
- Multiple event types (GOBLIN_RAID, PROSPERITY, DAWN, FULL_MOON)
- KINGDOM_* opcodes for querying kingdom stats
- Asynchronous event handling
- Multiple concurrent handlers
- DIVINE_TIME for time queries

**Event Types Demonstrated:**
- `EVENT_GOBLIN_RAID` - Triggers raid handler
- `EVENT_PROSPERITY` - Triggers prosperity handler  
- `EVENT_DAWN` - Triggers dawn handler
- `EVENT_FULL_MOON` - Wakes sleeping wizard

---

## Learning Path

**Recommended order for learning:**

1. **hello_world.bw** - Get familiar with basic syntax and output
2. **calculator.bw** - Learn stack manipulation and arithmetic
3. **maze_explorer.bw** - Understand movement, sensing, and control flow
4. **parallel_harvesters.bw** - Explore multi-wizard programs
5. **reactive_kingdom.bw** - Master the event system

## Creating Your Own Examples

See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines on contributing new examples.

**Good example program characteristics:**
- Focused on demonstrating 1-3 related concepts
- Well-commented with clear explanations
- Includes expected output
- Runs without modification
- Teaches something useful

## Example Categories

Examples are organized by complexity and concept:

### By Difficulty
- 🌟 **Beginner**: Basic syntax, simple operations
- 🧭 **Intermediate**: Control flow, grid interaction
- 🔮 **Advanced**: Multi-agent, events, complex logic

### By Concept
- **Output**: hello_world, calculator
- **Movement**: maze_explorer
- **Parallelism**: parallel_harvesters
- **Events**: reactive_kingdom
- **Math**: calculator
- **Sensing**: maze_explorer

## Tips for Running Examples

1. **Use IMMORTAL apples** - Set apples to `-1` to prevent wizard death during testing
2. **Adjust max_steps** - Increase if wizards don't finish executing
3. **Check outputs** - Use `get_outputs ''` to see wizard output
4. **Inspect state** - Use `wizard_summary ''` to debug
5. **Single-step** - Run with `max_steps=1` to step through execution

## Further Resources

- [Language Reference](../docs/LANGUAGE_REFERENCE.md) - Complete opcode documentation
- [Tutorial](../docs/TUTORIAL.md) - Step-by-step learning guide
- [Architecture](../docs/ARCHITECTURE.md) - How the engine works

---

Happy exploring! May your spells compile and your wizards prosper! 🧙‍♂️✨
