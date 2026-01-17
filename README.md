# bishop-vs-knight-ruy-lopez-case
Relative positional efficiency comparison of bishop vs knight using a Ruy Lopez opening structure

# Bishop vs Knight: Relative Positional Efficiency (Ruy Lopez Opening Structure)

A small experimental project that evaluates bishop vs knight positional efficiency from a geometric and computational perspective, using a Ruy Lopez opening structure as a concrete test case. Rather than comparing pieces by raw mobility or static piece values, the model measures relative efficiency under central control constraints.

---

## Core Idea

### Central Influence
A central weight matrix assigns higher values to strategically important squares (d4, d5, e4, e5). This allows mobility to be evaluated in a positionally meaningful way rather than simply counting legal moves.

### Knight Mobility
- Non-linear movement
- Obstacle-invariant
- All target squares are always reachable if inside the board.

### Bishop Mobility
- Linear movement along diagonals
- Blocked by pieces
- Mobility depends strongly on pawn structure and piece placement.

Piece effects are evaluated without side affiliation, meaning that color is ignored, even though pieces are explicitly placed separately on the board.

---

## Efficiency Index
Efficiency = Current Positional Mobility / Maximum Theoretical Potential

A small threshold is applied to suppress marginal differences and numerical noise.

---

## Visualization

Each square is labeled by relative efficiency:

- ♝ Bishop advantage
- ♞ Knight advantage
- — Roughly equal
- ★ Occupied squares

---

## Example Code Snippet (Efficiency Calculation)

```python
IDEAL_BOARD = np.zeros((8,8)) # Ideal, unobstructed board for theoretical bishop mobility
center_sq = [(3,3),(3,4),(4,3),(4,4)]
max_k = np.mean([get_knight_score(r,c) for r,c in center_sq])
max_b = np.mean([get_bishop_score(r,c,IDEAL_BOARD) for r,c in center_sq])
top1_map = np.empty((8,8), dtype=object)
threshold = 0.005 # decision margin
for r in range(8):
    for c in range(8):
        # Efficiency Ratio = Current Performance / Theoretical Potential
        k_perf = get_knight_score(r,c) / max_k
        b_perf = get_bishop_score(r,c,board) / max_b
        diff = b_perf - k_perf
        if diff > threshold: top1_map[r,c] = "Bishop"
        elif diff < -threshold: top1_map[r,c] = "Knight"
        else: top1_map[r,c] = "Equal"
```

---

## How to Run

```
pip install -r requirements.txt
jupyter notebook bishop_vs_knight_efficiency.ipynb
```

---

## Project Structure

```
bishop-vs-knight-ruy-lopez-case/
├── bishop-vs-knight-ruy-lopez-case.ipynb
├── result.png
├── README.md
└── requirements.txt
```
