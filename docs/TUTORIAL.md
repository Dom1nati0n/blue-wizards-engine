# Blue Wizards Tutorial

A hands-on guide to learning Blue Wizards programming from the ground up.

## Table of Contents

1. [Getting Started](#getting-started)
2. [Your First Wizard](#your-first-wizard)
3. [Stack Basics](#stack-basics)
4. [Movement and Navigation](#movement-and-navigation)
5. [Control Flow](#control-flow)
6. [Subroutines](#subroutines)
7. [Multi-Wizard Programs](#multi-wizard-programs)
8. [Variables and Queues](#variables-and-queues)
9. [Events and Reactivity](#events-and-reactivity)
10. [Advanced Techniques](#advanced-techniques)

---

## Getting Started

### Installation

1. Install [J](https://www.jsoftware.com/) (tested on version 9.0+)
2. Clone this repository
3. Load the engine:

```j
load 'path/to/blue_wizards_v29.ijs'
```

### Understanding the Environment

Blue Wizards programs exist on a 3D grid. Each cell can contain:
- **Code** (opcode values)
- **Data** (numbers)
- **Resources** (apples, terrain markers)

Wizards:
- Move through this grid
- Read opcodes from cells
- Execute them
- Use a stack for computation

---

## Your First Wizard

### Hello World

Let's create a wizard that outputs "Hi":

```j
NB Define the program (H=72, i=105)
program =: , 72 , 105 , 2 , PROPHECY_STR

NB Initialize the grid
init_grid program

NB Spawn wizard: position (0,0,0), direction (0,0,1), 
NB empty stack, 10 apples, parent_id 0, ROLE_WIZARD
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD

NB Run simulation for 10 steps
run_multi program ; 1 ; 10 ; 10

NB Get output
echo get_outputs ''
```

**What happens:**
1. Grid contains: `[72, 105, 2, PROPHECY_STR]`
2. Wizard starts at (0,0,0) moving East (0,0,1)
3. Step 1: Reads 72, pushes to stack → stack: `[72]`
4. Step 2: Reads 105, pushes to stack → stack: `[72, 105]`
5. Step 3: Reads 2, pushes to stack → stack: `[72, 105, 2]`
6. Step 4: Executes PROPHECY_STR (24)
   - Pops 2 (length)
   - Pops 105, 72 (characters)
   - Outputs "Hi"

### Simple Calculator

Add two numbers and output the result:

```j
program =: , 5 , 3 , ALCHEMY_ADD , PROPHECY_NUM

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 10 ; 10
echo get_outputs ''    NB Outputs: 8
```

**Execution trace:**
1. Push 5 → stack: `[5]`
2. Push 3 → stack: `[5, 3]`
3. ALCHEMY_ADD → pops 3 and 5, pushes 8 → stack: `[8]`
4. PROPHECY_NUM → pops 8, outputs "8"

---

## Stack Basics

The stack is your primary workspace. Master these operations:

### Duplication and Swapping

```j
NB Program: Clone a value, multiply it by itself, output
program =: , 7 , INV_CLONE , ALCHEMY_MUL , PROPHECY_NUM

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 10 ; 10
echo get_outputs ''    NB Outputs: 49
```

**Stack trace:**
1. Push 7 → `[7]`
2. INV_CLONE → `[7, 7]`
3. ALCHEMY_MUL → `[49]`
4. Output 49

### Stack Manipulation

```j
NB Calculate (a+b) * c where a=2, b=3, c=4
NB We need: 2 3 + 4 *
program =: , 2 , 3 , ALCHEMY_ADD , 4 , ALCHEMY_MUL , PROPHECY_NUM

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 15 ; 10
echo get_outputs ''    NB Outputs: 20
```

### Common Stack Patterns

**Swap before operation:**
```j
NB Want: 10 - 3 = 7 (not 3 - 10)
program =: , 3 , 10 , INV_SWAP , ALCHEMY_SUB , PROPHECY_NUM
```

**Keep a value for later:**
```j
NB Compute x and x+1 where x=5
program =: , 5 , INV_CLONE , 1 , ALCHEMY_ADD , PROPHECY_NUM , PROPHECY_NUM
NB Stack: [5] → [5,5] → [5,5,1] → [5,6] → outputs 6, then 5
```

---

## Movement and Navigation

### Changing Direction

Wizards read and execute the code they travel through:

```j
NB Create a cross pattern:
NB     N
NB   W + E
NB     S
NB Code laid out in a line, but wizard turns

program =: , 1 , PROPHECY_NUM
program =: program , MOVE_SOUTH    NB Turn south
program =: program , 2 , PROPHECY_NUM
program =: program , MOVE_WEST     NB Turn west  
program =: program , 3 , PROPHECY_NUM
program =: program , MOVE_NORTH    NB Turn north
program =: program , 4 , PROPHECY_NUM

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 20 ; 10
echo get_outputs ''    NB Outputs: 1234
```

### Grid Interaction

**Reading from the grid:**

```j
NB Place some data on grid, read it
program =: , 42 , SCRIBE_WRITE    NB Write 42 to current cell
program =: program , MOVE_EAST , MOVE_WEST   NB Move away and back
program =: program , SCRIBE_COPY , PROPHECY_NUM   NB Read and output

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 20 ; 10
echo get_outputs ''    NB Outputs: 42
```

**Destructive read:**

```j
NB SCRIBE_READ zeros the cell after reading
program =: , 100 , SCRIBE_WRITE
program =: program , MOVE_EAST , MOVE_WEST
program =: program , SCRIBE_READ , PROPHECY_NUM     NB Outputs 100
program =: program , MOVE_EAST , MOVE_WEST
program =: program , SCRIBE_COPY , PROPHECY_NUM     NB Outputs 0

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 30 ; 10
echo get_outputs ''    NB Outputs: 1000
```

---

## Control Flow

### Conditional Execution

**Skip if zero:**

```j
NB If value is 0, skip the output
program =: , 0 , INV_CLONE , RUNE_ISZERO
program =: program , RITUAL_SKIP1    NB Skip next if top is non-zero
program =: program , PROPHECY_NUM
program =: program , 99 , PROPHECY_NUM

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 20 ; 10
echo get_outputs ''    NB Outputs: 99 (skipped the 0)
```

### Loops

**Simple counting loop:**

```j
NB Output numbers 1 through 5
NB Loop structure: [addr, count] on stack, RITUAL_CYCLE jumps if count > 0

program =: i.0
loop_body =: 5    NB Position where loop body starts

NB Setup: push loop parameters
program =: program , loop_body , 5    NB addr=5, count=5

NB Loop body (starts at position 5):
program =: program , INV_OVER , INV_OVER    NB Keep addr and count
program =: program , ALCHEMY_SUB            NB 5-count = current number
program =: program , 1 , ALCHEMY_ADD        NB +1 to get 1,2,3,4,5
program =: program , PROPHECY_NUM           NB Output it
program =: program , RITUAL_CYCLE           NB Loop

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD  
run_multi program ; 1 ; 50 ; 10
echo get_outputs ''    NB Outputs: 54321
```

### Jumps

**Unconditional jump:**

```j
NB Jump over a section of code
program =: , 10 , RITUAL_WARP    NB Jump to position 10
program =: program , 99 , PROPHECY_NUM    NB This gets skipped (positions 2-3)
program =: program , 0 0 0 0 0 0    NB Filler (positions 4-9)
program =: program , 42 , PROPHECY_NUM    NB Jump destination (position 10)

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 20 ; 10
echo get_outputs ''    NB Outputs: 42
```

---

## Subroutines

### Basic Function Call

Functions use RITUAL_INVOKE and RITUAL_RETURN:

```j
NB Main program
program =: , 5 , 20 , RITUAL_INVOKE , PROPHECY_NUM , 0
NB Padding to position 20
program =: program , (15 # 0)
NB Subroutine at position 20: square function
NB Input: value on stack
NB Output: value squared on stack
program =: program , INV_CLONE , ALCHEMY_MUL , RITUAL_RETURN

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 30 ; 10
echo get_outputs ''    NB Outputs: 25
```

**How it works:**
1. Push 5
2. Push 20 (subroutine address)
3. RITUAL_INVOKE:
   - Pushes FRAME_MARK (999999)
   - Pushes return address (3)
   - Jumps to 20
4. At position 20: duplicate and multiply (5*5=25)
5. RITUAL_RETURN:
   - Pops until FRAME_MARK found
   - Pops return address (3)
   - Jumps back
6. PROPHECY_NUM outputs 25

### Function with Multiple Calls

```j
NB Compute 3² + 4²
program =: i.0
sub_addr =: 30

program =: program , 3 , sub_addr , RITUAL_INVOKE    NB Call square(3)
program =: program , 4 , sub_addr , RITUAL_INVOKE    NB Call square(4)
program =: program , ALCHEMY_ADD , PROPHECY_NUM      NB Add and output
program =: program , 0

NB Padding
program =: program , ((sub_addr - #program) # 0)

NB Square subroutine
program =: program , INV_CLONE , ALCHEMY_MUL , RITUAL_RETURN

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 50 ; 10
echo get_outputs ''    NB Outputs: 25 (3²+4²=9+16=25)
```

---

## Multi-Wizard Programs

### Parallel Execution

Use FORK to create wizard copies:

```j
NB Two wizards count independently
program =: , FORK                  NB Create a clone
program =: program , 1 , PROPHECY_NUM    NB Both output 1
program =: program , 2 , PROPHECY_NUM    NB Both output 2

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 20 ; 10
echo get_outputs ''    NB Outputs: 1122 or 1212 (non-deterministic order)
```

### Communication via Owls

```j
NB Wizard 1 sends a message, Wizard 2 receives it

NB Wizard 1 program (starts at 0,0,0)
w1_program =: , 42                    NB Value to send
w1_program =: w1_program , 2          NB Target wizard ID
w1_program =: w1_program , 100        NB Radius
w1_program =: w1_program , SEND_OWL   NB Send
w1_program =: w1_program , MEDITATE , MEDITATE , MEDITATE

NB Wizard 2 program (starts at 0,0,5) - just waits and outputs
w2_program =: , MEDITATE , MEDITATE , MEDITATE
w2_program =: w2_program , PROPHECY_NUM    NB Output received value

NB Combine programs on grid
program =: w1_program , ((5 - #w1_program) # 0) , w2_program

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
spawn_wizard (0 0 5) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 2 ; 20 ; 10
echo get_outputs ''    NB Wizard 2 outputs: 42
```

---

## Variables and Queues

### Memory Crystals (Variables)

Store and retrieve named values:

```j
NB Store values with different names, retrieve them
program =: , 100 , 1 , BIND           NB env[1] = 100
program =: program , 200 , 2 , BIND   NB env[2] = 200
program =: program , 1 , FETCH        NB Retrieve env[1]
program =: program , 2 , FETCH        NB Retrieve env[2]
program =: program , ALCHEMY_ADD      NB Add them
program =: program , PROPHECY_NUM     NB Output sum

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 30 ; 10
echo get_outputs ''    NB Outputs: 300
```

**Use case:** Preserve state across subroutine calls

### Work Order Scrolls (Queues)

Queue operations for task management:

```j
NB Enqueue tasks, process them in order
program =: , 10 , ENQUEUE
program =: program , 20 , ENQUEUE
program =: program , 30 , ENQUEUE
program =: program , QUEUE_DEPTH , PROPHECY_NUM    NB Output: 3
program =: program , DEQUEUE , PROPHECY_NUM         NB Output: 10
program =: program , DEQUEUE , PROPHECY_NUM         NB Output: 20
program =: program , DEQUEUE , PROPHECY_NUM         NB Output: 30

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 40 ; 10
echo get_outputs ''    NB Outputs: 3102030
```

---

## Events and Reactivity

### Awaiting Events

```j
NB Wait for dawn event, then output
program =: , EVENT_DAWN , AWAIT_OMEN
program =: program , 99 , PROPHECY_NUM

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 200 ; 10
echo get_outputs ''    NB Eventually outputs: 99 (after dawn event)
```

### Event Handlers

```j
NB Register a handler for goblin raids
program =: i.0

NB Main code
program =: program , EVENT_GOBLIN_RAID , 20 , REACT   NB Register handler
program =: program , 1 , PROPHECY_NUM
program =: program , MEDITATE , MEDITATE , MEDITATE
program =: program , 2 , PROPHECY_NUM
program =: program , (10 # MEDITATE)

NB Handler at position 20
program =: program , ((20 - #program) # 0)
program =: program , 999 , PROPHECY_NUM    NB Alert: 999!
program =: program , MEDITATE , MEDITATE , MEDITATE

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 300 ; 10
echo get_outputs ''    NB Normal execution + 999 when raid happens
```

---

## Advanced Techniques

### Higher-Order Functions: MAP

Apply a function to each element:

```j
NB Square each number in [2, 3, 4]
program =: i.0
square_sub =: 30

NB Push array onto stack
program =: program , 2 , 3 , 4

NB Call MAP
program =: program , square_sub , MAP

NB Output results
program =: program , PROPHECY_NUM , PROPHECY_NUM , PROPHECY_NUM

NB Padding
program =: program , ((square_sub - #program) # 0)

NB Square subroutine
program =: program , INV_CLONE , ALCHEMY_MUL , RITUAL_RETURN

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 100 ; 10
echo get_outputs ''    NB Outputs: 16 9 4 (in reverse stack order)
```

### FILTER Example

Keep only even numbers:

```j
NB Filter [1,2,3,4,5,6] for even numbers
program =: i.0
is_even =: 30

program =: program , 1 , 2 , 3 , 4 , 5 , 6
program =: program , is_even , FILTER
program =: program , (10 # PROPHECY_NUM)    NB Output all results

NB Padding
program =: program , ((is_even - #program) # 0)

NB is_even subroutine: returns (value % 2 == 0)
program =: program , 2 , ALCHEMY_MOD
program =: program , RUNE_ISZERO
program =: program , RITUAL_RETURN

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 100 ; 10
echo get_outputs ''    NB Outputs even numbers: 642
```

### REDUCE Example

Sum an array:

```j
NB Sum [1,2,3,4,5]
program =: i.0
add_sub =: 30

program =: program , 0              NB Initial value
program =: program , 1 , 2 , 3 , 4 , 5
program =: program , add_sub , REDUCE
program =: program , PROPHECY_NUM

NB Padding
program =: program , ((add_sub - #program) # 0)

NB Add subroutine
program =: program , ALCHEMY_ADD , RITUAL_RETURN

init_grid program
spawn_wizard (0 0 0) ; (0 0 1) ; i.0 ; 10 ; 0 ; ROLE_WIZARD
run_multi program ; 1 ; 100 ; 10
echo get_outputs ''    NB Outputs: 15
```

---

## Debugging Tips

1. **Use PROPHECY_NUM liberally** - Output intermediate values to trace execution
2. **Start with IMMORTAL** - Set apples to -1 to prevent death during testing
3. **Single-step execution** - Run with max_steps=1 repeatedly
4. **Check wizard state** - Use `wizard_summary ''` to inspect all wizards
5. **Grid inspection** - Use `view_grid` to see grid contents
6. **Stack depth** - Insert INV_DEPTH and PROPHECY_NUM to check stack size

### Common Pitfalls

- **Forgetting RITUAL_RETURN** - Subroutines must end with return
- **Stack underflow** - Popping from empty stack causes errors
- **Wrong direction** - Wizards move after executing, plan accordingly
- **Apple starvation** - Wizards die without apples (use IMMORTAL for testing)
- **Opcode confusion** - Double-check opcode constants

---

## Next Steps

1. Study the [example programs](../examples/)
2. Read the [Language Reference](LANGUAGE_REFERENCE.md) for complete opcode details
3. Explore the [Architecture](ARCHITECTURE.md) to understand internals
4. Write your own programs!
5. Share with the community

Happy wizarding! 🧙‍♂️✨
