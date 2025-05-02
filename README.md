# QuantumChess
## ♯ Quantum Chess Rulebook (Cirq Implementation)

---

### 🧱 Board and Qubits

* The game is played on a standard **8x8 chessboard**.
* Each square is represented by a **qubit** (e.g., `a1`, `h8` → 64 qubits total).
* Classical chess notation is used for positions.

---

### ♟ Pieces and Mapping

Each piece has:

* **Owner**: `'white'` or `'black'`
* **Type**: `'pawn'`, `'rook'`, `'knight'`, `'bishop'`, `'queen'`, `'king'`
* **Qubit presence**: piece can exist in **superposition** across multiple squares.

---

### ↺ Game Flow

* Players alternate turns (`white` starts).
* On each turn, players can:

  * `move <src> <dst>`: Classical move.
  * `supermove <src> <dst1> <dst2> ...`: Places the piece in **quantum superposition**.
  * `capture <target>`: Measure the target square. If an enemy piece exists (after collapse), it is captured.

---

### ⚛️ Quantum Mechanics Rules

#### 🎲 1. Superposition Move

* A piece may exist in multiple positions via `Hadamard` and `CNOT` gates.
* These positions are **entangled** — any measurement collapses all involved.

#### 🧹 2. Entanglement Tracking

* Superposed squares form an **entangled group**.
* Tracking maps:

  * `entangled_groups`: position → group ID
  * `entangled_sets`: group ID → set of positions

#### 🎯 3. Measurement and Collapse

* **Measuring one qubit in an entangled group collapses all**.
* Result of measurement updates the piece location.
* Non-collapsing positions are **cleared** from the board.

---

### 👑 Check and King Rules

#### ⚠️ Quantum Check

* **Check is not enforced** unless it is **measured**.
* Players may move into potential check.
* A player is only **checkmated** when their **king is measured and captured**.

#### ✅ Checkmate Rule

* If a `capture` operation reveals and removes a `king`, the game ends.

---

### ❓ Quantum Uncertainty on Board

* Squares may have:

  * `P`, `Q`, `K`, etc. → Known piece
  * `?` → Unknown/superposed piece
  * `.` → Empty square

---

### 🎮 Example Commands

```bash
> supermove d1 d3 d5
> move e2 e4
> capture d5
```

---

### ⚒️ Developer Notes

* Circuit simulation is done via `cirq.Simulator()`.
* Entanglement tracking is critical for handling quantum collapse.
* Cleanup of `entangled_groups` and `entangled_sets` is essential after each measurement.

---

### ⚙️ Win Condition

* **Game ends immediately** when a king is successfully measured and captured.
