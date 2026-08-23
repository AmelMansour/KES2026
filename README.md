# A Hybrid AI Framework Combining NSGA-II and Deep Q-Network for the Ambulance Routing Problem in Emergency Medical Services

The Ambulance Routing Problem (ARP) is a critical challenge in Emergency Medical Services (EMS), where minimizing response time and improving coverage are essential for saving lives. Traditional optimization methods often fail to capture the dynamic, stochastic, and priority-driven nature of real-world emergency systems. This repository implements a **Hybrid EMS Optimization Framework** combining Multi-Objective Evolutionary Optimization (NSGA-II) and Deep Q-Network (DQN)-based reinforcement learning.

Incident priority is derived directly from historical EMS data using a **rarity-based heuristic**: call types that occur less frequently in the data are assigned a higher priority. This priority feeds into both stages of the pipeline.

A Non-Dominated Sorting Genetic Algorithm II (NSGA-II) is employed **offline** to generate Pareto-optimal ambulance deployment strategies, optimizing total response time and priority-weighted response time. Its best solution then **warm-starts** a Deep Q-Network (DQN) agent, which learns an adaptive dispatch policy — choosing which ambulance to send to each incoming incident in real time. Routing itself always follows the fixed shortest path (Dijkstra) over the road network; it is not learned by the agent.

The two stages are coupled through a single, **one-directional** interaction: NSGA-II runs once, offline, and its solution initializes the DQN's state and replay buffer — there is no feedback loop back into NSGA-II. Experiments on real EMS datasets and OpenStreetMap road networks, across three scenarios of increasing scale, show that the hybrid framework improves Pareto efficiency and solution quality over standalone NSGA-II, at a small additional computational cost.

## ⚙️ Methodology

The proposed framework introduces an end-to-end pipeline for EMS optimization, integrating data preprocessing, multi-objective optimization, and DQN-based dispatch. As illustrated in Figure 1, the system combines offline global planning with online real-time decision-making.

* **Data Preprocessing:**
  Raw EMS incident records are cleaned and reduced to three core fields — incident ID, incident datetime, and initial call type. Priority levels are derived from call-type frequency: rarer call types receive a higher priority score (a rarity heuristic, not a predictive model).

* **Spatial Mapping and Graph Construction:**
  The urban road network is modeled using OpenStreetMap via OSMnx, where nodes represent intersections and edges encode travel distance and time (shortest path via Dijkstra).

* **Scenario Sampling:**
  Three demand scenarios are generated to test scalability: **Small** (20 incidents / 5 ambulances), **Medium** (100 incidents / 10 ambulances), and **Large** (200 incidents / 15 ambulances).

* **Multi-Objective Optimization (NSGA-II):**
  An offline optimization phase searches over full ambulance-to-incident deployment assignments, minimizing total response time and priority-weighted response time, and produces a set of Pareto-optimal solutions. The single best compromise, S\*, is selected via a normalized weighted aggregation.

* **Deep Q-Network (DQN) Environment:**
  A simulation environment models ambulance availability, incident arrivals, and travel times for sequential dispatch decisions. At each step, the DQN's action space is *which free ambulance to dispatch* — routing itself always follows the fixed shortest path and is never learned by the agent.

* **DQN-Based Dispatch Policy:**
  The DQN learns a dispatch policy by maximizing cumulative reward, where the reward penalizes long travel times on high-priority calls (r = −travel_time × priority), using ε-greedy exploration decayed over training (1.0 → 0.05).

* **Hybrid Integration (Warm-Start):**
  NSGA-II's best offline solution (S\*) warm-starts the DQN — it initializes the agent's state and pre-fills its replay buffer, so training does not start from scratch. This interaction is **one-shot and one-directional**: NSGA-II runs once, offline, and there is no feedback loop back into NSGA-II using the DQN's results.

* **Simulation and Learning:**
  After warm-starting, the DQN continues training through repeated interaction with the simulated environment — 150 episodes of 30 steps each (4,500 steps total) — updating its policy via experience replay and a periodically-synced target network.

* **Performance Evaluation:**
  The framework is evaluated using Hypervolume (HV), Inverted Generational Distance (IGD), Spread, and computational runtime.

**Final Outcomes:**
Experimental results show consistent improvements in Pareto efficiency and solution quality over standalone NSGA-II, with the DQN component adding only a small fraction of extra runtime, and stable behavior across increasing problem scales.

<img width="1500" height="908" alt="ChatGPT Image 23 août 2026, 19_07_33" src="https://github.com/user-attachments/assets/a9923f38-6b4a-4a12-b802-c5f109e60f0b" />


## 📊 Experimental Results

Experiments were conducted on real EMS datasets and OpenStreetMap road networks across small, medium, and large-scale scenarios. NSGA-II and the Hybrid method are evaluated under the same ambulance-availability constraint (an ambulance cannot serve two requests at once), using a shared reference point per scenario.

| Scenario | Method  | HV       | IGD      | Spread   | Runtime (s) |
| -------- | ------- | -------- | -------- | -------- | ----------- |
| Small    | NSGA-II | 0.049526 | 0.525564 | –        | 208.20      |
| Small    | Hybrid  | 0.417754 | 0.087901 | ≈0       | 219.45      |
| Small    | RL      | –        | –        | –        | 11.25       |
| Medium   | NSGA-II | 0.040000 | 0.927663 | –        | 1340.57     |
| Medium   | Hybrid  | 0.829156 | 0.077305 | ≈0       | 1360.68     |
| Medium   | RL      | –        | –        | –        | 20.11       |
| Large    | NSGA-II | 0.055853 | 0.772827 | 0.012915 | 3155.35     |
| Large    | Hybrid  | 0.909406 | 0.259937 | ≈0       | 3205.81     |
| Large    | RL      | –        | –        | –        | 50.46       |

The results show that the proposed hybrid approach:

- Improves Hypervolume (HV) compared to NSGA-II by **8.4×** (Small), **20.7×** (Medium), and **16.3×** (Large)
- Reduces IGD — better Pareto convergence — by **6.0×** (Small), **12.0×** (Medium), and **2.97×** (Large)
- Keeps Spread close to zero across all scenarios, consistent with evaluating a single learned policy over several rollouts rather than a diverse population of solutions
- Adds only **1.5–5%** extra runtime on top of NSGA-II — the DQN stage itself is very fast, so the quality gain does not come at a significant computational cost

In particular:

- The hybrid model consistently outperforms NSGA-II in all three scenarios
- RL (DQN) alone achieves extremely low runtime (11–50 s) but does not produce Pareto-optimal solutions, since it learns a single dispatch policy rather than a set of trade-off solutions — multi-objective metrics don't apply to it directly
- The hybrid approach combines NSGA-II's solution quality with the DQN's fast, adaptive real-time dispatch

## 🚀 Key Contributions

- Unified integration of multi-objective evolutionary optimization (NSGA-II) and deep reinforcement learning (DQN) for ambulance dispatch
- EMS-specific modeling of ambulances, incident priority (derived from historical call-type frequency), and real road networks (OpenStreetMap)
- A one-shot, one-directional warm-start interaction between global offline search (NSGA-II) and online adaptive dispatch policy learning (DQN)
- Evaluation across three EMS demand scales (Small / Medium / Large) using Hypervolume, IGD, Spread, and runtime

## 🧠 Conclusion

The proposed framework provides a scalable, EMS-aware decision support system for ambulance dispatching. It substantially improves Pareto efficiency and solution quality over standalone NSGA-II — most notably on the Medium and Large scenarios, where the ambulance-availability constraint makes the deployment problem most coupled — while adding only a small computational overhead, enabling real-time dispatch responsiveness in dynamic emergency environments.
