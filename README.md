# 🎮 Mobile-Optimized Gomoku (Renju Rule AI)

A lightweight, single-file HTML5/JavaScript Gomoku (Five in a Row) game engine optimized for mobile environments. It features a rule engine that complies with international standard **Renju Rules** and a competitive AI driven by **Alpha-Beta Pruning with Zobrist Hashing**.



## ✨ Key Features

- **Standard Renju Rules:** Strictly prohibits Black (First Player) from making 3-3, 4-4, or Overlines (6+ stones) to eliminate the first-player advantage. White faces no restrictions.
- **Zobrist Hashing Engine:** Translates 15x15 board states into 64-bit integer (`BigInt`) hashes using XOR bitwise operations, reducing state comparison complexity to $O(1)$.
- **Alpha-Beta Pruning Search:** Implements a Minimax game-tree with Alpha-Beta Pruning, integrated with a Transposition Table to eliminate redundant evaluations and optimize move-searching efficiency.
- **Dynamic AI Difficulty:** Scalable AI strength ranging from amateur levels to advanced단(Dan) ranks by adjusting lookahead depth dynamically based on selected difficulty.
- **Mobile Responsive Design:** Solves viewport coordinate distortion between physical mobile screen sizes and standard canvas pixel density using dynamic scale mapping.
- **Rich Game Features:** Supports Player vs Player (PVP), Player vs AI (PVAI), real-time move recommendations (Hint), Undo function, and match record tracking (Rank ON/OFF).

---

## 🛠️ Deep Dive: Core Architecture

### 1. Renju Rule Verification Logic
The core engine continuously checks the validity of Black's moves prior to placement. It runs a deep search on 4 directional axes (horizontal, vertical, and both diagonals) to intercept illegal configurations.
- **Double Three (3-3):** Detected via the `isPureOpenThree()` matrix analyzer, which checks if a placement simultaneously creates two or more "Open Threes" that can safely expand into Open Fours.
- **Double Four (4-4):** Monitored by `isFourLine()`, which bans moves creating two or more independent four-stone lines, regardless of whether their ends are blocked.
- **Overline:** Automatically triggers a soft-lock warning for Black if `checkLineCount() > 5`.

### 2. State Optimization via Zobrist Hashing
Instead of traversing the entire 15x15 grid layout array ($O(N^2)$) at every tree node, this engine initializes an array of random 64-bit cryptographic keys representing each coordinate and stone state.
- Upon placement or an Undo operation, it maps the modifications natively using bitwise XOR operations (`boardHash ^= randomTable[r][c][stone]`).
- The computed hash acts as a unique key for the **Transposition Table**, bypassing pre-evaluated game tree branches entirely.

### 3. Static Board Evaluation weights
The static evaluation function (`evaluateBoard`) scores the board configuration at leaf nodes based on strategic value:
- **Defense-First Weighting:** Assigns a higher priority score to blocking an opponent's open 4-line (+30,000 pts) over finalizing its own 4-line (+10,000 pts) to enforce a rock-solid, defensive AI playstyle.
- **Center-Proximity Focus:** Incorporates positional weights that yield extra scores as moves approach the center point (7,7) to emulate authentic opening game (Fuseki) positioning.

---

## 🚀 Quick Start & Deployment

This project is completely serverless and requires no additional dependencies or compilation tools.

1. Download the executable HTML file: **`Gomoku.html`**
2. Open the file in any modern web browser (Chrome, Safari, Edge, Firefox).
3. Tap or click anywhere on the grid to start your match. Works out-of-the-box on iOS and Android devices.

---

## 📁 File Structure

```text
.
└── Gomoku.html        # Single-file Web Application (HTML5 Canvas / CSS3 / Vanilla JS)
---

## 📝 License
Distributed under the MIT License. Feel free to use, modify, and distribute for personal or educational purposes.
