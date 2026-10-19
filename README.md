# Cafeteria Queue Simulation

An agent-based simulation of a university cafeteria, built in **NetLogo**, that tests how the cooking speed of each food station affects student waiting time.

> Course project for **Simulation and Modeling**.

---

## Overview

The model represents a cafeteria with three food stations: **Pizza**, **Burger** and **Salad**. Students arrive, choose a station, queue, order, eat and leave. Each station cooks at its own adjustable speed, so a slow station builds a long line while a fast one keeps up.

**Goal:** find cooking speeds that reduce overall waiting time.

**Key question:** how does changing each station's cooking speed affect the cafeteria's performance?

---

## The Model

### Layout

| Element | Description |
|---|---|
| Entrance doors | Three cyan blocks at the bottom. Students appear at one of them. |
| Counters | Three yellow blocks at the top, labelled Pizza, Burger and Salad (left to right). Each has its own vertical lane. |
| Tables | Brown areas on the left and right walls. |
| Exit doors | Two violet blocks on the side walls. |

### A student's journey

| Stage | Status | What happens |
|---|---|---|
| 1 | `entering` | Walks up from an entrance door and picks a station. |
| 2 | `to-lane` | Moves sideways to the lane of the chosen station. |
| 3 | `lining-up` | Walks toward the counter and stops behind the student ahead. Each tick spent blocked counts as waiting time. |
| 4 | ordering | At the counter, the station cooks for a number of ticks (the service time). |
| 5 | `to-table` | Walks to a table. Pizza students go left, salad students go right, burger students pick a side at random. |
| 6 | `eating` | Sits for 40 ticks. |
| 7 | `leaving` | Walks to the nearest exit door and leaves the simulation. |

Colors show the chosen station: **red** for Pizza, **orange** for Burger and **lime** for Salad. White students have already ordered and they are blue before choosing a station.

### Model rules

- **Time:** Each run lasts **5000 ticks** and then stops automatically.
- **Arrivals:** on every tick, a new student arrives with a probability equal to `arrival-rate` (in percent).
- **Service time:** the average time to serve one order is `base time / cooking speed`, with base times of 40 (pizza), 30 (burger) and 15 (salad) ticks.
- **Waiting time:** the number of ticks a student is stuck behind another student in a queue. Walking and eating time are not counted.
- **Station choice (`lane-policy`):**
  - `random`: every station has an equal chance.
  - `shortest-line`: the student joins the station with the shortest queue.

> Note: The model run lasts 5000 ticks but in the experiments ran using the Behaviourspace tool, each run lasts a 1000 ticks.

---

## Variables

### Inputs

| Variable | Description |
|---|---|
| `pizza-cooking-speed` | Pizza station speed: 1 (slow) to 5 (fast) |
| `burger-cooking-speed` | Burger station speed: 1 (slow) to 5 (fast) |
| `salad-cooking-speed` | Salad station speed: 1 (slow) to 5 (fast) |
| `arrival-rate` | Chance (%) that a new student arrives on each tick |
| `lane-policy` | How students choose a station: `random` or `shortest-line` |

### State variables

| Variable | Description |
|---|---|
| `pizza-service-time`, `burger-service-time`, `salad-service-time` | Average ticks to serve one order at each station |
| `total-waiting-time` | Sum of the waiting ticks of all served students |
| `students-served` | Number of students who have finished ordering |
| `students-waiting` | Number of students standing in a queue right now |

### Outputs

| Output | Description |
|---|---|
| `average-wait` | Average waiting time per served student (ticks) |
| `students-served` | Total students served in a run |
| `avg-queue` | Average queue length at each station |
| `max-queue` | Longest queue seen at each station |
| `utilization` | Percentage of ticks in which a station was busy cooking |

---

## Getting Started

### Requirements

- [NetLogo](https://ccl.northwestern.edu/netlogo/) 7.0 or later (developed in 7.0.4)

### Run the model

1. Download `cafeteria-simulation.nlogox`.
2. Open it in NetLogo.
3. Set the three cooking-speeds, `arrival-rate` using the sliders and `lane-policy` using the chooser.
4. Click **setup**, then **go**.
5. After 5000 ticks the run stops and a results summary is printed in the Command Center.

### Interface

- **Sliders:** `pizza-cooking-speed`, `burger-cooking-speed`, `salad-cooking-speed`, `arrival-rate`
- **Chooser:** `lane-policy`
- **Monitors:** students served, students waiting, average waiting time
- **Plots:** queue lengths, average wait over time, station utilization

The cooking-speed sliders also work while the model is running, so you can slow a station down and watch its queue grow.

---

## Experiments

Experiments are run with **BehaviorSpace** (*Tools → BehaviorSpace*). Both are saved inside the model file.

| | `cooking-speed-test` | `policy-comparison` |
|---|---|---|
| Question | How does each station's speed affect waiting? | Does the way students choose a station change the result? |
| Varied | Pizza, burger and salad speed: 1, 3, 5 | Same, plus `lane-policy`: random, shortest-line |
| Fixed | `arrival-rate` = 9, random choice | `arrival-rate` = 9 |
| Repetitions | 3 | 5 |
| Total runs | 81 | 270 |
| Run length | 1000 ticks | 1000 ticks |

Each setting is repeated because the model is random (arrivals, order lengths and random station choice), and averaging several runs gives a fairer result than any single run.

**Measured in every run:** students served, students waiting, average wait, average queue length, maximum queue length and utilization for each station.

---

## Results

### Experiment 1: cooking speed (average waiting time in ticks, random choice)

| Station | Slow (1) | Medium (3) | Fast (5) |
|---|---|---|---|
| Pizza | 43.4 | 9.6 | 8.7 |
| Burger | 33.3 | 13.5 | 14.8 |
| Salad | 19.8 | 23.2 | 18.6 |

Station utilization at slow speed: Pizza 78%, Burger 71%, Salad 40%.

### Experiment 2: random vs shortest-line (average waiting time in ticks)

| Setting | Random | Shortest-line |
|---|---|---|
| Overall average | 20.7 | 6.7 |
| Pizza slow (1) | 41.5 | 13.1 |
| Pizza medium (3) | 10.1 | 4.4 |
| Pizza fast (5) | 10.5 | 2.7 |

### Findings

- **Slow pizza or burger stations cause the long waits.** Waits were 33 to 43 ticks at speed 1.
- **Medium speed fixes most of the problem.** Going from slow to medium cut the pizza wait by about 80%. Fast adds very little.
- **Idle stations don't need speed.** Salad is busy only about 40% of the time even when slow, so it never forms a queue.
- **Student behaviour matters.** Choosing the shortest line reduced waiting by about two thirds, and made waits more predictable.
- **Recommended setup:** Pizza 5, Burger 3, Salad 3, with students choosing shorter lines.

The model is random, so exact numbers vary slightly from run to run. The pattern stays the same.

---

## Repository Contents

| File | Description |
|---|---|
| `cafeteria-simulation.nlogox` | The NetLogo model, including both BehaviorSpace experiments |
| `data/cooking_speed_experiment_results.csv` | Experiment 1 results, one row per run (81 rows) |
| `data/policy_lane_experiment_results.csv` | Experiment 2 results, one row per run (270 rows) |

### Reading the data files

**Station numbers:** `1` = Pizza, `2` = Burger, `3` = Salad. Columns named `item 0 max-queue`, `item 1 max-queue` and `item 2 max-queue` are the longest queues at Pizza, Burger and Salad (lists start counting at 0).

---

## Limitations

- Experiment 1 uses 3 repetitions per setting and Experiment 2 uses 5. Large differences are reliable; small ones may be noise.
- Each station receives an equal share of students. Real cafeterias have favourite foods.
- `shortest-line` is an extreme case: it assumes every student will switch food to avoid a queue. Real students behave somewhere between the two policies.
- Cooking times, arrival rate and eating time are assumptions, not measurements from a real cafeteria.
- Waiting time counts only time stuck behind another student, not walking or total time in the cafeteria.
- Students move in straight lines in a fixed layout.

---

## Technologies

- [NetLogo](https://ccl.northwestern.edu/netlogo/) 7.0.4: agent-based modelling and BehaviorSpace
- Google Sheets: pivot-table analysis of the results