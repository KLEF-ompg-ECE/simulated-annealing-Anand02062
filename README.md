# Assignment 1 — Simulated Annealing: Exam Timetable Scheduling
## Observation Report

**Student Name  :** ___C.Ananad___________  
**Student ID    :** ____2310040129________  
**Date Submitted:** _____08/04/2026_______

---

## How to Submit

1. Run each experiment following the instructions below
2. Fill in every answer box — do not leave placeholders
3. Make sure the `plots/` folder contains all required images
4. Commit this README and the `plots/` folder to your GitHub repo

---

## Before You Begin — Read the Code

Open `sa_timetable.py` and read through it. Then answer these questions.

**Q1. What does `count_clashes()` measure? What value means a perfect timetable?**

```
count_clashes() counts the total number of times any student has two or more 
exams assigned to the same time slot across all students. For each student, 
it checks whether any two of their three exams share the same slot. A return 
value of 0 means a perfect timetable — no student has any conflicting exams.
```

**Q2. What does `generate_neighbor()` do? How is the new timetable different from the current one?**

```
generate_neighbor() creates a slightly modified copy of the current timetable 
by picking one exam at random and moving it to a different (randomly chosen) 
time slot. The new timetable is identical to the current one except for exactly 
one exam being reassigned to a new slot, making it a "neighbouring" solution.
```

**Q3. In `run_sa()`, there is this line:**
```python
if delta < 0 or random.random() < math.exp(-delta / T):
```
**What does this line decide? Why does SA sometimes accept a worse solution?**

```
This line decides whether to move to the neighbouring timetable. If the 
neighbour is better (delta < 0), it is always accepted. If the neighbour is 
worse (delta > 0), it is still accepted with a probability of e^(-delta/T). 
SA accepts worse solutions to escape local minima — without this, the algorithm 
could get permanently stuck in a sub-optimal solution and never find the global 
optimum. As temperature T decreases over time, the probability of accepting 
worse solutions drops, so the search gradually becomes more selective.
```

---

## Experiment 1 — Baseline Run

**Instructions:** Run the program without changing anything.
```bash
python sa_timetable.py
```

**Fill in this table:**

| Metric | Your result |
|--------|-------------|
| Number of iterations completed | 1379 |
| Clashes at iteration 1 | 12 |
| Final best clashes | 3 |
| Did SA reach 0 clashes? (Yes / No) | No |

**Copy the printed timetable output here:**
```
  Final Timetable
------------------------------------------
  Slot 1:  Geography
  Slot 2:  Chemistry, English
  Slot 3:  History, Computer Science, Economics
  Slot 4:  Biology, Statistics
  Slot 5:  Mathematics, Physics
------------------------------------------
  Total clashes : 3
```

**Look at `plots/experiment_1.png` and describe what you see (2–3 sentences).**  
*Where does the biggest drop in clashes happen? Does the curve flatten out?*
```
The clashes drop steeply in the first ~100 iterations, falling from 12 down 
toward the lower single digits very quickly while the temperature is still 
high. After around iteration 200, the curve flattens and stays at 3 clashes 
for the remainder of the run, indicating the algorithm has settled into a 
local minimum it cannot escape. The temperature curve shows a smooth 
exponential decay, which corresponds directly to the point at which the 
algorithm stops accepting worse solutions and freezes on its current best.
```

---

## Experiment 2 — Effect of Cooling Rate

**Instructions:** In `sa_timetable.py`, find the `# EXPERIMENT 2` block in `__main__`.  
Copy it three times and run with `cooling_rate` = **0.80**, **0.95**, and **0.995**.  
Save plots as `experiment_2a.png`, `experiment_2b.png`, `experiment_2c.png`.

**Results table:**

| cooling_rate | Final clashes | Iterations completed | Reached 0 clashes? |
|-------------|---------------|----------------------|--------------------|
| 0.80        | 8             | 31                   | No                 |
| 0.95        | 3             | 135                  | No                 |
| 0.995       | 3             | 1379                 | No                 |

**Compare the three plots. What do you notice about how fast vs slow cooling affects the result? (3–4 sentences)**  
*Hint: Fast cooling = temperature drops quickly. Does it have time to explore well?*
```
With cooling_rate=0.80, the temperature drops so rapidly that the algorithm 
exhausts its useful search range in just 31 iterations, leaving it with 8 
clashes — the worst result of the three. The search simply does not have 
enough time at high temperature to explore the solution space broadly before 
the acceptance probability collapses to near zero. With cooling_rate=0.95 
and 0.995, the algorithm runs for 135 and 1379 iterations respectively, both 
reaching 3 clashes, but the slower schedule gives the algorithm far more 
opportunities to escape local minima early on. The plots clearly show that 
faster cooling produces an abrupt, shallow drop, while slower cooling produces 
a longer, more gradual descent with more exploration before freezing.
```

**Which cooling_rate gave the best result? Why do you think that is?**
```
Both 0.95 and 0.995 tied at 3 clashes, but 0.995 is the better choice 
overall. A slower cooling rate keeps the temperature high for more iterations, 
giving SA more time to explore diverse regions of the search space and 
probabilistically escape local minima. With 0.80, the algorithm "freezes" 
too quickly, trapping itself in a poor solution (8 clashes) before it has had 
a chance to find anything better. In general, slower cooling = more thorough 
search = better solution quality, at the cost of more computation time.
```

---

## Summary

**Complete this table with your best result from each experiment:**

| Experiment | Key setting | Final clashes | Main finding in one sentence |
|------------|-------------|---------------|------------------------------|
| 1 — Baseline | cooling_rate = 0.995 | 3 | SA reduces clashes from 12 to 3 in 1379 iterations using the default slow cooling schedule. |
| 2 — Cooling rate | cooling_rate = 0.995 | 3 | Slow cooling (0.995) dramatically outperforms fast cooling (0.80), which freezes after only 31 iterations with 8 clashes remaining. |

**In your own words — what is the most important thing you learned about Simulated Annealing from these experiments? (3–5 sentences)**
```
The most important lesson is that the cooling rate is a critical hyperparameter 
that directly controls the quality of the solution SA finds. A rate too close 
to zero causes the algorithm to cool down so fast that it gets trapped in a bad 
local minimum almost immediately, without ever exploring the wider search space. 
Conversely, a cooling rate close to 1.0 keeps the temperature high long enough 
for the algorithm to probabilistically jump out of local minima and gradually 
converge on a much better solution. The key insight behind SA is that accepting 
worse solutions occasionally — modelled on how physical systems escape energy 
traps during slow cooling — is what makes the difference between getting stuck 
and finding near-optimal answers. Tuning the balance between exploration (high T) 
and exploitation (low T) is the heart of making SA work well in practice.
```

---

## Submission Checklist

- [done] Student name and ID filled in
- [done] Q1, Q2, Q3 answered
- [done] Experiment 1: table filled, timetable pasted, plot observation written
- [done] Experiment 2: results table filled (3 rows), observation and answer written
- [done] Summary table completed and reflection written
- [done] `plots/` contains: `experiment_1.png`, `experiment_2a.png`, `experiment_2b.png`, `experiment_2c.png`
