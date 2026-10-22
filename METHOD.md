# Snake solver — method

The full plan for the experiments is section 21 of [snake_solver_project.md](snake_solver_project.md); this file
holds the setup as it's actually run.

## Question
Which pathing algorithm finishes Snake in the fewest moves, and what does each one cost (speed, failures)?

## Plan
<Sam fills this in once there are two or more solvers to compare.>

## Setup
<Board size, starting position, food placement and random seeds: anything that must stay the same so every
algorithm gets identical conditions.>

## What's measured
Per algorithm, over enough games to be meaningful: average and maximum score, average survival time, average
moves, computation time, failure rate. Raw results go in `data/`, the scripts that crunch them in `analysis/`,
write-ups (and the paper, if it happens) in `reports/`.
