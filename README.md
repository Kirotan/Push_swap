# push\_swap

**Minimal‑operation sorting of integers with two stacks and a custom instruction set.**

---

## ✨ Overview

`push_swap` is a 42 algorithmic challenge: **sort an input list of integers using only two stacks (`A` and `B`) and a limited set of predefined operations**.
The goal is to output the *shortest possible sequence* of operations while respecting strict time and memory constraints.

---

## 📑 Table of Contents

1. [Rules](#rules)
2. [Operations](#operations)
3. [Algorithm](#algorithm)
4. [Directory Structure](#directory-structure)
5. [Getting Started](#getting-started)
6. [Usage](#usage)
7. [Evaluation & Scoring](#evaluation--scoring)
8. [Project Context](#project-context)

---

<a id="rules"></a>

## 📝 Rules

* Input: **positive or negative 32‑bit ints** provided as CLI arguments.
* Output: **one instruction per line** chosen from the [Operations](#operations) list.
* Constraints:

  * No duplicated numbers.
  * Must detect and handle integer overflow.
  * No memory leaks (checked with Valgrind).
  * Must complete in < 2 seconds on the 42 evaluation machine for 500 numbers.

---

<a id="operations"></a>

## ⚙️ Operations

| Category    | Mnemonic              | Description                                        |
| ----------- | --------------------- | -------------------------------------------------- |
| **Swap**    | `sa` / `sb` / `ss`    | Swap top two elements of *A*, *B*, or both.        |
| **Push**    | `pa` / `pb`           | Push top element from *B* → *A* or *A* → *B*.      |
| **Rotate**  | `ra` / `rb` / `rr`    | Shift all elements up by 1 (top becomes bottom).   |
| **Reverse** | `rra` / `rrb` / `rrr` | Shift all elements down by 1 (bottom becomes top). |

All other operations are **forbidden**.

---

<a id="algorithm"></a>

## 🧠 Algorithm

| Input size | Strategy                                                      |
| ---------- | ------------------------------------------------------------- |
| `≤ 3`      | Hard‑coded optimal sequences (≤ 3 moves).                     |
| `≤ 5`      | Insertion sort with pattern detection.                        |
| `≤ 100`    | **Chunked selection sort** (n / 5 chunks).                    |
| `> 100`    | **Radix sort (binary)** ⇒ $O(n log₂ n)$ moves, $O(n)$ memory. |

Each strategy is chosen dynamically at runtime for best score.

---

<a id="directory-structure"></a>

## 🗂️ Directory Structure

```text
push_swap/
├── inc/            # headers (push_swap.h, dlist.h, etc.)
├── libft/          # embedded libft sub‑project (static lib)       
├── srcs/           # push_swap algorithm & utils
│   ├── functions
├── obj/            # auto‑generated object files
├── Makefile        # builds libft and push_swap
└── README.md
```

---

<a id="getting-started"></a>

## 🚀 Getting Started

### Build

```bash
# Clone repository
git clone https://github.com/Kirotan/push_swap.git
cd push_swap

# Build binary + libft
make                # creates ./push_swap
```

### Clean targets

```bash
make clean          # remove objects
make fclean         # remove objects + binaries + libs
make re             # full rebuild
```

---

<a id="usage"></a>

## 📚 Usage

```bash
./push_swap 2 1 3 6 5 8
# Example output:
sa
ra
pb
...
```

To **measure the number of operations**, pipe the output into `wc -l`:

```bash
./push_swap $(seq 1 100 | sort -R) | tee ops.txt | wc -l
```

<a id="evaluation--scoring"></a>

## 🏆 Evaluation & Scoring

### 42 official thresholds

| Stack size | Result (moves) | Score |
| ---------- | -------------- | ----- |
| 0– 100     | <= 700         | 5 / 5 |
| 101– 500   | <= 5500        | 5 / 5 |

My implementation typically achieves **< 550 moves for 100 numbers** and **< 4500 moves for 500 numbers** on evaluator test sets.

---

<a id="project-context"></a>

## 🏛️ Project Context

*Developed as part of the **École 42** curriculum (Tier 2 algorithm project).*
Focus areas:

* algorithmic optimisation under constraints;
* custom doubly‑linked list for $O(1)$ stack ops;
* robust error handling (overflow, duplicates, invalid args);
* Makefile orchestration (embedded **libft** + optional **ft\_printf**).
