# Explanation — Coursera MLOps Simulation Labs

This workspace contains **two lab assignments** from Duke University's *MLOps
Foundations / Building Cloud Computing Solutions at Scale* specialization on
Coursera (by Noah Gift). Both labs teach the same underlying skill — **building
robust command-line simulation tools wrapped in professional MLOps/DevOps
scaffolding** — through small, focused example apps.

```
simulation-mlops-coursera/
├── Coursera-MLOPs-Foundations-Lab-2-poker-simulator/   # Lab 2 — CLI simulations
└── Coursera-MLOps-C2-lab4-greedy-optimization/         # C2 Lab 4 — greedy algorithms
```

---

## The shared idea

Despite the "MLOps" name, neither app is machine learning. The point is the
*scaffolding and tooling* that a production Python/ML project uses, demonstrated
around a tiny app:

- **`Makefile`** — standardized verbs every project shares: `make install / test / lint / format`.
- **`requirements.txt`** — pinned dependencies (a big "kitchen sink" template: pytest, black, pylint, fastapi, pandas, mlflow, torch, tensorflow…).
- **`.github/workflows/`** — GitHub Actions CI that runs install → lint → test → format → deploy **on GitHub's servers, after you push** (not locally).
- **`Dockerfile` / `.devcontainer/`** — containerization for reproducible runs (incl. GitHub Codespaces).
- **`test_main.py`** — a pytest unit test.

So the takeaway pattern is: **a tiny app wrapped in a complete, professional
CI/CD + containerized project structure.**

---

## Lab 2 — Poker Hand Simulator

A command-line **poker hand simulator** built with [Click](https://click.palletsprojects.com/).
It builds a 52-card deck and exposes four subcommands:

| Command | What it does |
|---|---|
| `info` | Lists all 10 poker hand types with examples and ranks (`--probability` adds odds) |
| `deal` | Deals random hand(s) and classifies each (`--hands N`) |
| `play` | Deals two hands, you vs. the computer, with a `--bet` |
| `interactive` | Multi-round betting game tracking money, wins/losses, and "expected value" |

The core logic is `evaluate_poker_hand()` — it inspects the 5 cards (counting
distinct suits and ranks) to decide flush, straight, four-of-a-kind, etc.

### The exercise (from the README)

1. **Investigate the tool** — run each command with `--help` (e.g. `./poker.py info --help`) and reflect on how good CLI help/UX is constructed.
2. **Critique the `interactive` simulation** — list **five improvements** to make it more realistic.
3. *(Optional)* Fork it and build your own improved poker simulator.

### Known weaknesses worth citing as improvements

- `evaluate_poker_hand` only compares hand *categories*, so two "One Pair" hands always tie instead of comparing kickers.
- Both hands are dealt from independent full decks, so they can share the same physical card.
- The "expected value" formula doesn't actually reflect real poker odds.

---

## C2 Lab 4 — Greedy Optimization

Same scaffolding, but the topic is **greedy algorithms**. A *greedy algorithm*
always takes the locally best choice at each step, hoping it leads to a good
overall result — fast and simple, but not always optimal. Two CLIs show both
sides of that trade-off.

### 1. `greedy_coin.py` — making change (greedy **is** optimal here)

Given an amount, returns the **minimum number of coins** to make that change by
walking from the biggest coin down and taking as many of each as possible.

```bash
python greedy_coin.py 1.50
```

For US coin denominations this greedy strategy is **provably optimal**. Its
real-world weakness: floating-point money (`0.10`, etc.) causes rounding errors;
you'd normally work in integer cents.

**Tasks:** add `--dollars` and `--cents` flags instead of one float argument;
reflect on whether that makes the tool more robust (yes — integer cents avoids
float drift and allows better validation).

### 2. `tsp.py` — Traveling Salesman Problem (greedy/random is **not** optimal)

Estimates the **shortest route visiting all cities** and returning to start.
Uses [`geopy`](https://geopy.readthedocs.io/) to geocode ~20 US cities into
lat/long, then computes round-trip distance. TSP is **NP-hard**, so this tool
brute-forces by randomization: shuffle the city order, measure total distance,
repeat N times, keep the shortest found.

```bash
python tsp.py simulate --count 10                    # 10 random orderings
python tsp.py cities "New York" "Boston" --count 2   # your own city list
```

**Tasks:** run `python tsp.py simulate`; reflect on the *optimal number of
simulations* — more runs improve your odds of a short route but with
**diminishing returns** and rising cost, with no guarantee of the true optimum
(20 cities = 20! ≈ 2.4×10¹⁸ possible routes).

> Note: `tsp.py` has two `if __name__ == "__main__":` blocks (lines 184 and 189).
> Only the first runs; the second (`main()` with no args) is dead code and would
> error. Lines 187–191 can be safely deleted.

### How the two relate

| | `greedy_coin.py` | `tsp.py` |
|---|---|---|
| Problem | Minimum coins for change | Shortest route through cities |
| Method | Pure greedy (biggest coin first) | Randomized repeated sampling |
| Greedy optimal? | **Yes** (for US coins) | **No** — only approximate |
| Lesson | Greedy *can* be perfect | Hard problems need simulation + "good enough" |

---

## References

- [Coursera-MLOPs-Foundations-Lab-2-poker-simulator](https://github.com/nogibjj/Coursera-MLOPs-Foundations-Lab-2-poker-simulator)
- [Building Cloud Computing Solutions at Scale Specialization](https://www.coursera.org/specializations/building-cloud-computing-solutions-at-scale)
- [Python, Bash and SQL Essentials for Data Engineering Specialization](https://www.coursera.org/learn/web-app-command-line-tools-for-data-engineering-duke)
