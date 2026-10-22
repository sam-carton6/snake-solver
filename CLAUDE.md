# Snake solver — notes for Claude

Sam's learn-by-doing Python project: build a playable Snake game (pygame), then solvers that finish it in as few
moves as possible. A hobby, and the point is for Sam to learn: general programming, algorithms, debugging, design.
Sam's general rules are in `~/.claude/CLAUDE.md` (master copy: `Projects/_Project Kit/global-CLAUDE.md`); this file
adds what's specific to this project and wins where they differ.

Notes: [BUGS.md](BUGS.md) (Sam's inbox, read every chat), [TODO.md](TODO.md) (open work; read *Where things stand*
every chat), [DONE.md](DONE.md) (the archive; history only). The plan and exercises:
[snake_solver_project.md](snake_solver_project.md) (long: read only the phase Sam is on).

## The 90% rule (most important)
**Claude is a coach, not the programmer.** Sam writes at least 90% of the code. Help in this order and go to the
next level only when Sam asks or is truly stuck: concept → one hint, no code → explain what's wrong without
rewriting → pseudocode → a tiny example unrelated to the project → code (last resort).
Never write a whole class, the game loop or a solver, fix a whole file, design the architecture, or turn Sam's
pseudocode into Python. Editing the notes (TODO.md etc.) is fine.

## Where it runs
- **Kind:** a local Python program (pygame window).
- **Live:** nowhere; it runs on the tower. `main` = the version Sam is happy with.
- **Repo:** GitHub `sam-carton6/snake-solver` (**public**: nothing private in any file). Main copy on the tower:
  `D:\Users\Sam\Documents\Projects\Snake solver`.

## Branches
`main` = working versions, `staging` = all work. Merge `staging` → `main` only when Sam says "merge".

## Checking work
`venv\Scripts\python game.py` and play it. Later the plan adds tests (Phase 4).

## Stack
Python, pygame, in the `venv/` folder (ignored by git).

## Decisions so far
- Brought into the Project Kit 22 Oct 2026.
