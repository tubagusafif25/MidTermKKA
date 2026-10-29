# Solving Wordle with Search: Greedy vs DFS vs Minimax (α-β)

Midterm project for the **KKA** (Artificial Intelligence) course at ITS, by Tubagus Muhamad Afif.

Wordle is modelled as a search problem and solved with three different strategies, which are then compared on every possible answer and on an adversarial ("Absurdle"-style) version of the game.

## Problem formulation

| Element | Definition |
|---|---|
| **State** | The set of answers still consistent with all feedback so far (starts with all 2,309 answers) |
| **Action** | A guess from the 12,953 allowed words |
| **Transition** | Feedback (🟩 🟨 ⬜) splits the state into buckets; the next state is the matching bucket |
| **Goal** | Feedback 🟩🟩🟩🟩🟩 within 6 guesses |

## Algorithms

1. **Greedy (maximum entropy):** picks the guess with the highest expected information gain for the current turn only.
2. **Depth-limited DFS on the AND-OR tree:** uninformed search in alphabetical order. A plan is accepted only if every feedback bucket can be solved in the guesses left, so it always wins within 6, but it is not optimal.
3. **Minimax with α-β pruning:** treats the feedback as an adversary and minimises the worst-case number of guesses, using depth limits, a leaf heuristic and move ordering.

## Experiments

- Normal Wordle over all 2,309 answers: mean and worst-case guesses, and search effort
- Distribution of guesses per solver
- Adversarial Wordle, where the game keeps the largest bucket after every guess
- Minimax ablation over search depth and branching factor

Each experiment saves one figure to `figures/`.

## Implementation highlights

- Feedback is encoded in base 3, and a full **12,953 × 2,309 pattern matrix** is pre-computed with vectorised NumPy and cached to disk, so every search step becomes an array lookup.
- Duplicate letters are handled the same way as the real game (greens first, then yellows while copies remain).
- Solvers are deterministic, so decisions are cached per state and reused across games.

## Run it

```bash
pip install -r requirements.txt
jupyter notebook wordle_solvers.ipynb
```

Run all cells. The full experiment run takes about 2 minutes. The pattern matrix is generated on the first run and saved as `data/pattern_matrix.npy` (git-ignored).

## Data

`data/answers.txt` and `data/allowed_guesses.txt` are the word lists used in 3Blue1Brown's Wordle analysis ([3b1b/videos](https://github.com/3b1b/videos)).

## Tech

Python · NumPy · pandas · Matplotlib · Jupyter
