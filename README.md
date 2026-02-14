# 🧙‍♂️ Blue Wizards Engine

**Version 29 - "The Blue Grimoire"**

A fantasy-themed esoteric programming language implemented in J, where code becomes spells, variables become memory crystals, and programmers become wizards navigating a mystical 3D grid world.

## ✨ What is Blue Wizards?

Blue Wizards is a stack-based esoteric programming language that reimagines traditional programming concepts through the lens of fantasy and magic:

- **Alchemy** replaces arithmetic operations
- **Runecraft** handles bitwise logic
- **Rituals** control program flow
- **Memory Crystals** store variables
- **Omens & Prophecies** enable reactive programming
- **Work Order Scrolls** implement queues
- **Arcane Arrays** provide higher-order functions

Multiple wizards can exist simultaneously in the world, moving through 3D space, collecting resources, communicating via magical owls, and responding to kingdom-wide events.

## 🎯 Features

### Core Programming Concepts
- **Stack-based execution** - All wizards maintain their own stack
- **3D grid world** - Navigate through layers, rows, and columns
- **Multi-agent system** - Multiple wizards execute concurrently
- **Message passing** - Owls carry values between wizards
- **Resource management** - Apples for energy, items for capabilities
- **Event-driven programming** - React to kingdom events
- **Role-based abilities** - Different wizard types (Farmer, Warrior, Merchant, etc.)

### Advanced Features (V29)
- **Variables** (BIND/FETCH) - Named storage via Memory Crystals
- **Queues** (ENQUEUE/DEQUEUE) - Work order management
- **Parallelism** (FORK) - Clone wizards with full state
- **Reactive Programming** (AWAIT_OMEN/REACT) - Event handlers
- **Higher-order Functions** (MAP/FILTER/REDUCE) - Functional programming
- **Resource Trading** (TRADE) - Economy simulation

## 🚀 Quick Start

### Prerequisites
- [J programming language](https://www.jsoftware.com/) (version 9.0 or higher)

### Installation

1. Clone this repository:
```bash
git clone https://github.com/yourusername/blue-wizards-engine.git
cd blue-wizards-engine
```

2. Load the engine in J:
```j
load 'engine/blue_wizards_v29.ijs'
```

### Your First Spell

Create a simple program that collects an apple and outputs it as a number:

```j
NB Define the spell (program)
program =: , 1 , APPLE_COLLECT , PROPHECY_NUM

NB Initialize grid with the program
init_grid program

NB Spawn a wizard with 10 apples at origin
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD

NB Run for 5 steps
run_multi program ; 1 ; 5 ; 10

NB See the output
get_outputs ''
```

## 📚 Documentation

- **[Language Reference](docs/LANGUAGE_REFERENCE.md)** - Complete opcode documentation
- **[Tutorial](docs/TUTORIAL.md)** - Step-by-step learning guide
- **[Architecture](docs/ARCHITECTURE.md)** - How the engine works
- **[Examples](examples/)** - Sample programs demonstrating features

## 🎮 Example Programs

| Example | Description | Key Features |
|---------|-------------|--------------|
| [hello_world.bw](examples/hello_world.bw) | Classic first program | Output, basic movement |
| [calculator.bw](examples/calculator.bw) | Arithmetic operations | Alchemy spells, stack manipulation |
| [maze_explorer.bw](examples/maze_explorer.bw) | Navigate a maze | Movement, conditionals, sensing |
| [parallel_harvesters.bw](examples/parallel_harvesters.bw) | Multiple wizards harvest resources | FORK, multi-agent coordination |
| [reactive_kingdom.bw](examples/reactive_kingdom.bw) | Event-driven responses | AWAIT_OMEN, REACT, kingdom events |

## 🧩 Language Overview

### The Wizard State (13-tuple)
Each wizard maintains:
1. `ip` - Instruction pointer (3D position)
2. `dir` - Direction vector
3. `stk` - Data stack
4. `apples` - Energy/health
5. `wid` - Wizard ID
6. `pid` - Parent ID
7. `status` - Alive/dead/waiting
8. `out` - Output buffer
9. `items` - Inventory
10. `role` - Profession
11. `env` - Variable bindings (Memory Crystals)
12. `queue` - Work orders
13. `react` - Event handlers

### Thematic Categories

```
🧪 ALCHEMY      - Math operations (ADD, SUB, MUL, DIV, POW, etc.)
🔮 RUNECRAFT    - Logic and bitwise ops (AND, OR, XOR, shifts)
🚶 TRAVERSAL    - Movement (MOVE_EAST, WANDER, LEAP, PORTAL)
🎒 INVENTORY    - Stack manipulation (CLONE, SWAP, DROP, ROT)
📜 SCRIBING     - Grid I/O (READ, WRITE, MARK, ERASE)
🔍 DIVINATION   - Sensing (SCAN, SENSE, DIVINE_OWLS, DIVINE_TIME)
⚡ RITUALS      - Control flow (INVOKE, RETURN, WARP, loops, conditionals)
📢 PROPHECY     - Output (PROPHECY_NUM, PROPHECY_STR, PROPHECY_CHAR)
🌟 CONJURATION  - Lifecycle (FORK, BANISH_SELF, SEND_OWL, MEDITATE)
🍎 HARVEST      - Resources (APPLE_COLLECT, HARVEST_TREE, CHOP, MINE)
💎 MEMORY       - Variables (BIND, FETCH)
📋 QUEUES       - Work orders (ENQUEUE, DEQUEUE, QUEUE_DEPTH)
🌙 OMENS        - Events (AWAIT_OMEN, REACT, kingdom events)
🏺 ARCANE       - Higher-order (MAP, FILTER, REDUCE)
```

## 🎨 Philosophy

Blue Wizards reimagines programming as a mystical practice:

- **Code as Spells** - Programs are magical incantations written on a grid
- **Execution as Exploration** - Wizards move through code, reading and executing it
- **Collaboration as Magic** - Multiple wizards cooperate through owl messengers
- **Resources as Power** - Apples fuel life, items grant abilities
- **Events as Fate** - Kingdom-wide events shape wizard behavior

## 🤝 Contributing

Contributions are welcome! Whether you want to:
- Add new example programs
- Improve documentation
- Extend the language with new opcodes
- Fix bugs or optimize performance
- Create tools or utilities

Please feel free to open an issue or submit a pull request.

## 📜 License

This project is released under the MIT License - see LICENSE file for details.

## 🙏 Acknowledgments

Inspired by:
- Befunge and other 2D esolangs
- Stack-based languages like Forth
- The beauty and expressiveness of J
- The joy of making programming whimsical

## 🔗 Resources

- [J Language](https://www.jsoftware.com/) - The implementation language
- [Esolangs Wiki](https://esolangs.org/) - Esoteric programming languages

---

*"In the realm of code, where logic reigns supreme, we summon wizards to dance upon the grid, weaving spells of computation into the fabric of a mystical world."* ✨
