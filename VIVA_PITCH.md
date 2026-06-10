# VisualSort — COAL Project Viva Pitch

**Subject:** Computer Organization and Assembly Language (COAL)
**Project:** Assembly Language Sorting Algorithm Visualizer
**Technology:** x86 MASM Assembly, Node.js, HTML/CSS/JavaScript

---

## 📌 What Is This Project?

**VisualSort** is an interactive web-based visualization tool that runs real **x86 MASM Assembly Language sorting algorithms** and displays their execution step-by-step in a modern, animated user interface.

The core idea is simple:

> **Every time a comparison or swap happens inside the assembly code, the user can see it happen in real-time — both as animated bars AND as highlighted lines of assembly code.**

This bridges the gap between low-level machine code and high-level understanding.

---

## 🎯 Project Objective

The project demonstrates:

1. A deep understanding of **x86 MASM assembly** by implementing multiple sorting algorithms entirely in `.asm` files.
2. The ability to build a **full-stack system** — an Assembly `.exe` acting as a backend, connected to a modern web frontend.
3. Practical understanding of how a **CPU processes data** — via registers, memory addresses, stack frames, and instruction execution.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────┐
│         User's Browser          │
│    (visualizer.html + JS)       │
│                                 │
│  ┌─────────┐   ┌─────────────┐  │
│  │  Array  │   │  ASM Code   │  │
│  │  Bars   │   │   Viewer    │  │
│  └─────────┘   └─────────────┘  │
└──────────────┬──────────────────┘
               │ HTTP API calls
               ▼
┌─────────────────────────────────┐
│       Node.js Backend           │
│         (server.js)             │
│                                 │
│  Calls & monitors the .exe      │
└──────────────┬──────────────────┘
               │ Spawns process
               ▼
┌─────────────────────────────────┐
│    SortVisualizer.exe           │
│  (Compiled from MASM .asm)      │
│                                 │
│  Runs sorting algorithms in     │
│  pure x86 Assembly Language     │
└─────────────────────────────────┘
```

---

## ⚙️ Algorithms Implemented (in Assembly)

All five sorting algorithms are written from scratch in **x86 MASM Assembly** inside `SortVisualizer.asm`:

| Algorithm      | Time Complexity | Space | Assembly Procedure   |
|----------------|-----------------|-------|----------------------|
| Bubble Sort    | O(n²)           | O(1)  | `BubbleSort PROC`    |
| Selection Sort | O(n²)           | O(1)  | `SelectionSort PROC` |
| Insertion Sort | O(n²)           | O(1)  | `InsertionSort PROC` |
| Quick Sort     | O(n log n)      | O(log n) | `QuickSort PROC` + `QuickSortRange` |
| Merge Sort     | O(n log n)      | O(n)  | `MergeSort PROC`     |

---

## 🖥️ How the Visualizer Works — Step by Step

### Step 1 — User Opens the App
The user opens `visualizer.html` in a browser. The frontend connects to the Node.js server running at `localhost:3001`.

### Step 2 — Array Generation
The user can:
- Click **Random** to generate a random shuffled array.
- Click **Sorted** to generate a best-case (already sorted) array.
- Click **Reversed** to generate a worst-case (reverse sorted) array.
- Click **Custom** and manually type in their own numbers (e.g., `5, 10, 0x1A, 30`). Hex input is supported.

### Step 3 — Algorithm Selection
The user clicks any tab at the top: **Bubble Sort**, **Selection Sort**, **Insertion Sort**, **Quick Sort**, or **Merge Sort**.

### Step 4 — The Assembly Executes
The Node.js server spawns the compiled Assembly `.exe`. The executable runs the chosen sorting algorithm on the array, writing memory snapshots to an output file after each pass.

### Step 5 — The Visualization Plays
The frontend reads the execution data and renders:
- **Animated bar graphs** showing the array state at every step
- **Highlighted ASM code** showing exactly which line of assembly code is currently executing
- **CPU register values** updating in real-time (EAX, EBX, ECX, etc.)

---

## 🎨 Visual Color System

### Array Bars
| Color | Meaning |
|-------|---------|
| **Dark Grey** | Idle element — not currently being accessed |
| **Bright Yellow** | Being **compared** — `CMP` instruction running |
| **Bright Red** | Being **swapped** — `MOV` instruction swapping memory |
| **Neon Green** | **Sorted** — locked into its correct final position |

### ASM Code Panel
| Highlight Color | Meaning |
|-----------------|---------|
| **Yellow** | A `CMP` (compare) instruction is executing |
| **Red** | A `MOV` (memory write/swap) instruction is executing |
| **Blue** | A `JMP`, `JGE`, or `CALL` (control flow) instruction |

---

## 🧠 Assembly-Level Features Exposed to the User

### 1. Live ASM Code Viewer
The actual source code from `SortVisualizer.asm` is loaded and displayed. As the algorithm steps, the exact line being executed is highlighted — making it an **instruction-level debugger**.

### 2. CPU Register Map
Simulates the x86 general-purpose registers:
- **EAX / EDX** — Temporarily hold array values being compared or swapped
- **ESI** — Inner loop index `j` (the "scanner" pointer)
- **EDI** — Outer loop bound (e.g., `n-1-i` in Bubble Sort)
- **ECX** — Outer loop counter `i`
- **EBX** — Holds `n-1` (the array size limit)

Registers **flash white** when their value changes, just like a real CPU would update them.

### 3. Recursive Call Stack (Quick Sort / Merge Sort)
For recursive algorithms, a **Stack Trace panel** shows the call depth growing and shrinking as `QuickSortRange` or `MergeRange` pushes and pops frames.

---

## ⚔️ Compare Mode

Clicking **COMPARE** activates a split-screen mode:

- **Select two different algorithms** from the dropdown menus at the top.
- Both algorithms race on the **same input array** simultaneously.
- Two **separate ASM code panels** appear below — one for each algorithm.
- A **live complexity graph** on the right side plots their actual comparison counts as racing lines.
- The algorithm that finishes first is declared: **🏆 WINS**

This is a powerful demonstration of **why O(n log n) algorithms outperform O(n²)** on large data sets.

---

## 📈 Complexity Graph

The COMPLEXITY panel contains a live **HTML Canvas graph** that plots:

| Line | What It Represents |
|------|--------------------|
| 🟠 Orange line | Actual comparisons made by Algorithm A |
| 🔵 Blue line | Actual comparisons made by Algorithm B (in Compare Mode) |
| ⬛ Grey line | Theoretical upper bound (n² or n log n) |

---

## 🎛️ Playback Controls

| Control | Action |
|---------|--------|
| ▶️ Play | Auto-advance through all frames |
| ⏸️ Pause | Freeze the animation |
| ⏭️ Step Forward | Advance exactly one assembly event |
| ⏮️ Step Back | Go back one assembly event |
| ⏪ Rewind | Reset to the initial unsorted state |
| ⏩ Jump to End | Instantly show the fully sorted result |
| 🔊 Sound Toggle | Enable audio — pitches correspond to element heights |
| Speed Slider | Control how fast the animation runs |

---

## 🔊 Sound Mode

When enabled, the **Web Audio API** generates tones in real-time:
- **Comparisons** produce smooth **sine waves**
- **Swaps** produce harsh **square waves**
- Pitch is directly proportional to the **value of the element** being touched

This creates the characteristic "sorting sound" you might recognize from YouTube algorithm visualization videos.

---

## 📤 Export Feature

Clicking the **EXPORT** button downloads a `.txt` log file containing:
- Algorithm name and array size
- Total comparisons and swaps
- A frame-by-frame log of every event (which indices were compared/swapped and in which pass)

---

## 🗂️ Project File Structure

```
Visual-Sort/
├── SortVisualizer.asm        ← Core: all 5 algorithms in x86 MASM
├── SortVisualizer.exe        ← Compiled Assembly executable
├── server.js                 ← Node.js backend that runs the .exe
├── public/
│   ├── index.html            ← Landing / home page
│   └── visualizer.html       ← Main visualization UI (single-file app)
└── package.json
```

---

## 💡 Key Points to Emphasize in the Viva

1. **"The sorting logic is 100% written in MASM x86 Assembly."** The JavaScript on the frontend is purely for visualization rendering — it does not perform the sort itself.

2. **"I implemented all 5 sorting algorithms from scratch"** using x86 registers, memory-mapped arrays, procedure calls (PROC/ENDP), and conditional jumps (JGE, JLE, JNE).

3. **"The project visualizes at the instruction level."** The highlighted ASM line shows the exact CPU instruction being executed, not just a high-level step.

4. **"Compare Mode proves algorithm efficiency visually."** You can watch Bubble Sort lose to Quick Sort in real-time on the same dataset — demonstrating the real-world impact of O(n²) vs O(n log n).

5. **"The Register Map shows exactly how the CPU uses registers"** during sorting — for instance, EAX holds a temp value during a swap, exactly as the MASM code does.

---

*Built for the COAL Final Project — demonstrating mastery of x86 MASM Assembly Language.*
