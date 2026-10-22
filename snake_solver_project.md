# Snake → Solver in Python
## A coding-skills project

**Goal:** Build a playable Snake game in Python, then build an algorithm that can play/solve it.

**Primary constraint:** Try to complete at least **90% of the project yourself**. AI is a coach, not the programmer.

**Why this project:** You already use MATLAB at work and want to sharpen general programming skills, algorithms, data structures, debugging, and software design while doing something fun.

---

# 0. Project Rules

## The 90% rule

Before asking AI to write code:

1. Write down what you think the code should do.
2. Try an implementation yourself.
3. Run it.
4. Read the error/output.
5. Debug it yourself for a reasonable amount of time.
6. Only then ask AI for help.

When asking AI for help, prefer this order:

**Level 1 — Concept**
> "What concept am I missing here?"

**Level 2 — Hint**
> "Give me one hint, but don't show code."

**Level 3 — Debugging**
> "Here's my code and error. Tell me what I'm misunderstanding, but don't rewrite it."

**Level 4 — Pseudocode**
> "Give me pseudocode for this part, not Python."

**Level 5 — Tiny example**
> "Show me a minimal example of this concept unrelated to my project."

**Level 6 — Code**
Only use this when you've genuinely exhausted the earlier levels.

### Things AI should NOT do for you

Avoid asking AI to:

- write an entire class
- write the game loop
- implement the solver
- fix an entire file
- generate the architecture
- optimize your algorithm before you understand it
- turn your pseudocode directly into Python

If you eventually use AI-generated code, rewrite it yourself and make sure you can explain every line.

---

# 1. What You're Building

The project has two major phases.

## Phase A — Snake

Build a functioning Snake game.

Minimum features:

- grid-based board
- snake with multiple segments
- player-controlled movement
- food
- snake growth
- wall collision
- self collision
- score
- game-over state
- restart/new game

Optional later:

- increasing difficulty
- obstacles
- wrap-around walls
- high score
- pause
- multiple food types
- replay

## Phase B — Solver

Build a program that can control the snake automatically.

Start with increasingly capable algorithms rather than jumping straight to the hardest solution.

Suggested progression:

1. Random movement
2. Greedy food-seeking
3. Food-seeking with collision avoidance
4. Breadth-first search (BFS)
5. A* search
6. More advanced Snake-specific strategy
7. Optional: Hamiltonian-cycle approach
8. Optional: compare algorithms experimentally

The goal isn't just to "make an AI Snake."

The goal is to understand **why each algorithm succeeds or fails**.

---

# 2. Recommended Project Structure

Don't over-engineer this at first.

A possible eventual structure:

```text
snake_solver/
│
├── README.md
├── snake.py
├── game.py
├── board.py
├── solver.py
├── algorithms.py
├── tests/
│   ├── test_board.py
│   ├── test_snake.py
│   └── test_solver.py
│
└── experiments/
    └── results.md
```

You do **not** need to create all of this on day one.

Start with as few files as possible.

Refactor when the code becomes difficult to reason about.

---

# 3. Phase 0 — Design Before Coding

## Exercise 0.1 — Describe the game

Without writing Python, answer:

- What represents the board?
- What represents the snake?
- How do you represent a position?
- How do you represent direction?
- How do you represent food?
- How does the snake grow?
- How do you detect collision?
- When does a game tick happen?
- What information does the solver need?

Write your answers here:

```text
Board:

Snake:

Position:

Direction:

Food:

Growth:

Collision:

Game tick:

Solver inputs:
```

Do not worry about choosing the "perfect" representation.

Make a reasonable choice and move on.

---

# 4. Phase 1 — Representing the World

This is where the real programming practice starts.

## Exercise 1.1 — Coordinates

Pick a coordinate system.

For example:

```text
(0, 0) (1, 0) (2, 0)
(0, 1) (1, 1) (2, 1)
(0, 2) (1, 2) (2, 2)
```

Questions:

- Which direction increases x?
- Which direction increases y?
- What does "up" mean?
- What does "right" mean?

Pick conventions and stick to them.

## Exercise 1.2 — Position operations

You should eventually be able to answer:

> "If the snake is at `(x, y)` and moves right, what is its next position?"

Try implementing this without AI.

Potential concept:

- tuples
- dictionaries
- enums
- functions

Choose what makes sense to you.

---

# 5. Phase 2 — Build Snake Without Graphics

**Important:** Consider making the first version completely text-based.

This separates programming/game logic from graphics.

For example:

```text
..........
...ooH....
..........
.....*....
..........
```

Where:

- `H` = head
- `o` = body
- `*` = food

You don't need fancy graphics to prove the game logic works.

## Milestone

Get this working:

```text
start
  ↓
snake moves
  ↓
food appears
  ↓
snake reaches food
  ↓
snake grows
  ↓
snake eventually collides
  ↓
game over
```

---

# 6. Phase 3 — The Game Loop

This is an important programming concept.

Think about:

```text
while game_is_running:

    get_input()

    update_game()

    check_collisions()

    render()
```

Don't copy this structure blindly.

Figure out:

- What happens first?
- What state changes?
- What happens if two things occur on the same tick?
- Does rendering change the game state?
- What should happen after game over?

## Challenge

Explain your game loop to yourself in plain English before implementing it.

If you can't explain the loop without looking at your code, the architecture probably needs more thought.

---

# 7. Phase 4 — Tests Before You Get Fancy

Start writing small tests.

Examples:

### Movement

Given:

```text
head = (4, 5)
direction = RIGHT
```

Expected:

```text
(5, 5)
```

### Wall collision

Given a board width of `10`:

```text
head = (9, 5)
direction = RIGHT
```

Expected:

```text
collision
```

### Food

Given:

```text
head = (5, 5)
food = (5, 5)
```

Expected:

```text
food consumed
```

### Growth

If the snake eats food:

```text
length before = N
length after  = N + 1
```

Create enough tests that you trust the game logic.

---

# 8. Phase 5 — Separate Game Logic From Input

Eventually you want two different controllers:

```text
Human controller
       ↓
      Game

AI controller
       ↓
      Game
```

The game shouldn't care whether the next direction came from:

- keyboard input
- random selection
- BFS
- A*
- another algorithm

This is one of the most valuable architectural decisions in the project.

## Design question

What is the smallest piece of information the game needs from a controller?

Try to answer this before implementing the solver.

---

# 9. Phase 6 — First Solver: Random

This sounds silly.

Do it anyway.

The random solver should:

1. Look at the legal moves.
2. Choose one.
3. Make the move.
4. Repeat.

Measure:

- score
- survival time
- number of moves
- number of food items eaten

This gives you a baseline.

## Experiment

Run the random solver many times.

Ask:

- How variable are the scores?
- Does board size matter?
- Does snake length matter?
- What causes most deaths?

Don't optimize it yet.

---

# 10. Phase 7 — Greedy Solver

Now make the snake actually care about food.

Concept:

> Choose a legal move that appears to move the snake closer to the food.

You will need to define "closer."

Possible idea:

**Manhattan distance**

For positions:

```text
(x1, y1)
(x2, y2)
```

distance:

```text
abs(x1 - x2) + abs(y1 - y2)
```

Questions:

- Does minimizing distance guarantee safety?
- Can the snake trap itself?
- Can the shortest-looking move be fatal?
- What happens when the food is behind the snake's body?

This is your first introduction to the difference between:

**locally good**

and

**globally good**.

---

# 11. Phase 8 — Collision-Aware Greedy Solver

Improve the greedy algorithm.

Before considering a move, determine whether it is legal.

A move might be illegal because:

- it hits a wall
- it hits the snake's body

Then choose among the remaining moves.

## Challenge

Try to construct a board position where:

```text
Move A gets closer to food
Move B gets farther from food
```

but:

```text
A eventually causes death
B eventually survives
```

This is an important algorithmic insight.

---

# 12. Phase 9 — Breadth-First Search

Now you get to build a real pathfinding algorithm.

## Problem

Treat the board as a graph.

Each cell is a node.

A legal move between adjacent cells is an edge.

Then:

> Find the shortest path from the snake's head to the food.

This is a classic **BFS** problem.

## Before coding

Implement BFS on paper first.

Given:

```text
S . . #
# # . #
. . . F
```

Find a path from `S` to `F`.

Think about:

- queue
- visited set
- parent/predecessor
- reconstructing the path

## Key question

Why does BFS find a shortest path when every move has the same cost?

You should be able to explain this before moving on.

---

# 13. BFS Solver — First Version

Build the simplest version:

```text
head → food
```

while treating the snake body as obstacles.

Do not worry about whether the resulting path is strategically safe over many turns yet.

That problem comes next.

---

# 14. The Important Snake Problem

Here's where the project gets much more interesting.

A normal pathfinding problem asks:

> "Can I reach the destination?"

Snake asks:

> "Can I reach the destination without eventually trapping myself?"

Those are not the same problem.

Example:

```text
You can reach the food.
```

does NOT necessarily mean:

```text
You can safely reach the food.
```

Your solver now needs to reason about **future state**, not just current geometry.

---

# 15. State Representation

Start thinking about what defines a Snake state.

Is it just:

```text
head position
```

?

Probably not.

Two snakes could have the same head position but completely different bodies.

So perhaps the state is something like:

```text
(head position, body configuration)
```

This is a major algorithmic concept.

## Challenge

Write down exactly what information would be required to reproduce the game from a given state.

---

# 16. A* Search

Once BFS makes sense, implement A*.

You will need to understand:

```text
g(n)
h(n)
f(n) = g(n) + h(n)
```

Where:

- `g(n)` = cost so far
- `h(n)` = estimated remaining cost
- `f(n)` = total estimated cost

A natural first heuristic is Manhattan distance.

## Compare

Run BFS and A* on the same board configurations.

Measure:

- path length
- number of explored cells
- execution time
- success/failure

Don't assume A* is automatically "better."

Understand what changes and why.

---

# 17. The Hard Part — Safety

At this point, deliberately try to break your solver.

Construct situations where:

- food is in a corner
- the snake surrounds itself
- the snake has a long body
- the shortest path enters a dead end
- the snake must temporarily move away from food
- the food is adjacent but unreachable safely
- the snake needs to follow its own tail

Document every failure.

Create a section in `experiments/results.md`:

```text
Failure:
Why it happened:
What the algorithm believed:
What actually happened:
Possible improvement:
```

This is where a lot of the learning happens.

---

# 18. Optional Advanced Route — Tail Following

A useful idea to investigate:

> Instead of always chasing food, can the snake sometimes safely chase/follow its tail?

Questions to explore:

- Is the tail guaranteed to move?
- When does the tail move?
- Can the head safely reach the tail?
- What happens immediately after eating food?
- Can following the tail create a safe cycle?

Don't implement this until the earlier algorithms make sense.

---

# 19. Optional Advanced Route — Hamiltonian Cycle

This is a much more mathematical approach.

Research and understand:

**Hamiltonian cycle**

before implementing anything.

The basic idea is to construct a cycle through every cell of the board.

Then the snake can follow the cycle indefinitely.

Questions:

- What is a Hamiltonian cycle?
- Which board dimensions permit one?
- How could you construct one for a rectangular grid?
- How does following the cycle guarantee safety?
- How much efficiency does this sacrifice?

This is an excellent optional challenge if you enjoy algorithms.

---

# 20. Final Solver Architecture

Eventually you may arrive at something conceptually like:

```text
                 ┌──────────────┐
                 │ Current Game │
                 │    State     │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │   Solver     │
                 └──────┬───────┘
                        │
              chooses next move
                        │
                        ▼
                 ┌──────────────┐
                 │     Game     │
                 │    Update    │
                 └──────┬───────┘
                        │
                        └──────► repeat
```

The solver should not need to know how the screen is drawn.

The game should not need to know which algorithm is controlling it.

---

# 21. Experiments

Once you have multiple solvers, turn the project into a small experiment.

Possible algorithms:

```text
Random
Greedy
Greedy + safety
BFS
A*
Advanced strategy
Hamiltonian cycle
```

For each one, measure:

- average score
- maximum score
- average survival time
- average moves
- computation time
- failure rate

Run enough games to make the comparison meaningful.

---

# 22. Algorithm Questions to Answer

By the end, you should be able to explain:

### Data structures

- Why did I represent the snake this way?
- What are the tradeoffs?
- When do I use a list?
- When do I use a set?
- When do I use a queue?
- What is a priority queue?

### Algorithms

- What makes BFS work?
- Why does BFS find a shortest path?
- What does A* add?
- What makes a heuristic useful?
- What makes a heuristic misleading?
- What is the difference between local and global optimization?

### Complexity

Estimate:

- movement update complexity
- collision detection complexity
- BFS complexity
- A* complexity

You don't need perfect formal proofs initially.

Be able to reason about what happens as the board gets larger.

---

# 23. Refactoring Challenge

After everything works:

**Do not immediately ask AI to clean it up.**

Instead:

1. Read your own code.
2. Identify duplicated logic.
3. Identify functions doing too many things.
4. Identify confusing variable names.
5. Identify unnecessary state.
6. Identify places where the solver and game are too tightly coupled.

Then refactor it yourself.

Only after that, optionally ask AI:

> "Review this architecture and point out potential design problems without rewriting the code."

---

# 24. AI Usage Log

Keep a tiny log while working.

```text
Date:
Problem:
What I tried:
What I thought was happening:
What I asked AI:
What I learned:
```

This is important because your goal isn't simply:

> "Finish Snake."

Your goal is:

> "Become better at solving programming problems without AI."

---

# 25. Definition of "Done"

The project is complete when:

## Game

- [ ] Snake moves correctly
- [ ] Snake grows
- [ ] Food works
- [ ] Wall collision works
- [ ] Self collision works
- [ ] Game over works
- [ ] Game can restart
- [ ] Tests cover important game logic

## Solver

- [ ] Random solver
- [ ] Greedy solver
- [ ] Collision-aware solver
- [ ] BFS solver
- [ ] A* solver
- [ ] At least one advanced strategy

## Understanding

- [ ] Can explain the game architecture
- [ ] Can explain the state representation
- [ ] Can explain BFS without looking it up
- [ ] Can explain A*
- [ ] Can explain the heuristic
- [ ] Can explain why some strategies fail
- [ ] Can discuss computational complexity

## Experiments

- [ ] Algorithms tested on identical conditions
- [ ] Results recorded
- [ ] Failures documented
- [ ] At least one performance comparison

---

# 26. Stretch Goals

Only do these if you're having fun.

### Visualization

- draw explored BFS/A* cells
- show the planned path
- show the heuristic values
- visualize the snake's predicted future

### Replay

Record game states and replay them later.

### Deterministic testing

Use a fixed random seed so bugs can be reproduced.

### Benchmarking

Run hundreds/thousands of games automatically.

### Genetic algorithm

Try evolving strategies.

### Reinforcement learning

Only after you've built the simpler algorithms.

The point is to understand the baseline first.

---

# 27. Your First Session

Do **not** start by installing a bunch of packages.

Start extremely small.

### Task 1

Create a Python file.

### Task 2

Represent a board.

### Task 3

Represent one snake position.

### Task 4

Represent a direction.

### Task 5

Write code that calculates the next position.

### Task 6

Print the result.

Stop.

That's enough for session one.

If that feels trivial, that's good.

The point is to establish the world before building the game.

---

# 28. Rule for Getting Unstuck

If you're stuck for more than ~20–30 minutes:

**Don't immediately ask AI for the answer.**

Instead write:

```text
I am trying to:
_____

I currently have:
_____

I expected:
_____

I actually got:
_____

I think the problem might be:
_____
```

Then try one of:

- simplify the problem
- print intermediate state
- create a tiny test case
- reproduce the bug with the smallest possible input
- draw the state on paper

If you still can't solve it, ask AI for a **hint**, not a solution.

---

# 29. The Core Principle

The most valuable outcome isn't a sophisticated Snake AI.

It's getting comfortable with this loop:

```text
Understand
    ↓
Design
    ↓
Attempt
    ↓
Fail
    ↓
Inspect
    ↓
Debug
    ↓
Understand
    ↓
Try again
```

That's the skill you're trying to sharpen.

And because you use AI heavily at work, deliberately experiencing the uncomfortable middle steps yourself is part of the exercise.

---

# 30. Project Journal

Use this section as you work.

## Session 1

What I built:

```text

```

What confused me:

```text

```

What I learned:

```text

```

## Session 2

What I built:

```text

```

What confused me:

```text

```

What I learned:

```text

```

## Session 3

What I built:

```text

```

What confused me:

```text

```

What I learned:

```text

```

---

# Final Challenge

When you think you're finished, explain the entire project to another programmer **without showing them the code**.

Explain:

1. How the game state is represented.
2. How movement works.
3. How collisions work.
4. How food and growth work.
5. How the controller interface works.
6. How BFS works.
7. How A* differs from BFS.
8. Why finding food isn't enough to solve Snake.
9. Why your advanced strategy works.
10. What you would change if the board were 100×100 instead of 20×20.

If you can explain those clearly, you've gotten much more out of the project than simply having a working Snake game.
