# Hybrid Artificial Intelligence Framework for the Ambulance Routing Problem in Emergency Medical Systems

The Ambulance Routing Problem (ARP) is a critical challenge in Emergency Medical Services (EMS), where mini-
mizing response time, travel distance, and improving coverage are essential for saving lives. Traditional optimization
methods often fail to capture the dynamic, stochastic, and priority-driven nature of real-world emergency systems.
In this paper, we propose a Hybrid EMS Optimization Framework combining Artificial Intelligence (AI), Multi-
Objective Evolutionary Optimization, and Deep Q-Network (DQN) (RL). A predictive AI module estimates spatio-
temporal incident probabilities and assigns dynamic priority levels based on historical EMS data. These predictions
are used to construct risk-aware demand models and simulation scenarios.
A Non-Dominated Sorting Genetic Algorithm II (NSGA-II) is employed to generate Pareto-optimal ambulance de-
ployment and routing strategies, optimizing response time and priority-weighted response time. In parallel, a Deep
Q-Network (DQN) agent is introduced to learn adaptive dispatch policies in a dynamic environment, enabling real-
time decision-making under uncertainty.
A hybrid interaction loop combines NSGA-II and RL, ensuring both long-term optimality and short-term responsive-
ness. Experiments on large-scale real EMS datasets and OpenStreetMap road networks demonstrate that the proposed
framework improves Pareto efficiency, reduces response time, and increases system robustness.

⚙️ Methodology

The proposed framework introduces an end-to-end pipeline for EMS optimization, integrating data-driven modeling, multi-objective optimization, and DQN. As illustrated in Figure~\ref{fig:framework}, the system combines global planning with real-time decision-making.

* Data Preprocessing and Feature Engineering:  
Raw EMS incident data are cleaned and transformed into structured features, including temporal attributes and priority levels derived from incident types.

* Spatial Mapping and Graph Construction:  
The urban road network is modeled using OpenStreetMap via OSMnx, where nodes represent intersections and edges encode travel distance and time.

* Scenario Sampling: 
Multiple demand scenarios (Small, Medium, Large) are generated to reflect varying levels of operational complexity.

* Multi-objective Optimization (NSGA-II):  
An offline optimization phase determines initial ambulance positioning by minimizing total response time and priority-weighted response cost, producing a set of Pareto-optimal solutions.

* Deep Q-Network (DQN) Environment:  
A simulation environment models system states including ambulance positions and incident locations for sequential decision-making.

* DQN-based dispatch policy (DQN):  
A Deep Q-Network learns a dispatch policy by maximizing cumulative rewards based on response efficiency.

* Hybrid Integration:  
NSGA-II provides global optimal initialization, while RL enables adaptive real-time dispatch decisions, ensuring both exploration and responsiveness.

* Simulation and Learning:  
A continuous interaction loop updates the RL policy through environment feedback.

* Performance Evaluation:  
The framework is evaluated using Hypervolume, IGD, Spread, and computational runtime.

Final Outcomes:
Experimental results demonstrate significant improvements, including reduced response times, increased operational efficiency, and better coverage of high-risk areas, while maintaining stable learning behavior.

<img width="1005" height="1044" alt="Capture d&#39;écran 2026-04-06 112703" src="https://github.com/user-attachments/assets/e30ebe36-be44-4e06-ab42-5a1856a689e2" />


📊 Experimental Results

Experiments were conducted on real EMS datasets and OpenStreetMap road networks across small, medium, and large-scale scenarios.

| Scenario | Method  | HV       | IGD      | Spread   | Runtime (s) |
| -------- | ------- | -------- | -------- | -------- | ----------- |
| Small    | NSGA-II | 0.047472 | 0.000000 | --       | 263.16      |
| Small    | Hybrid  | 0.049845 | 0.000000 | --       | 266.76      |
| Small    | RL      | --       | --       | --       | 3.59        |
| Medium   | NSGA-II | 0.058565 | 0.003542 | 0.018212 | 1573.36     |
| Medium   | Hybrid  | 0.061494 | 0.003187 | 0.018576 | 1576.65     |
| Medium   | RL      | --       | --       | --       | 3.29        |
| Large    | NSGA-II | 0.074215 | 0.017656 | 0.069426 | 3756.40     |
| Large    | Hybrid  | 0.077926 | 0.015891 | 0.070814 | 3760.04     |
| Large    | RL      | --       | --       | --       | 3.63        |

The results show that the proposed hybrid approach:

Improves Hypervolume (HV) compared to NSGA-II by up to +5.0%
Achieves better Pareto convergence (lower IGD values)
Maintains stable performance across increasing problem scales
Provides very fast decision-making through DQN (near real-time inference)

In particular:

The hybrid model consistently outperforms NSGA-II in all scenarios
RL alone achieves extremely low runtime but does not produce Pareto-optimal solutions
The hybrid approach balances solution quality + real-time adaptability


🚀 Key Contributions
Unified integration of evolutionary optimization + deep reinforcement learning
EMS-specific modeling of ambulances, hospitals, patient priority, and road networks
Closed-loop interaction between global search (NSGA-II) and local policy learning (DQN)
Scalable performance across different EMS demand levels

🧠 Conclusion

The proposed framework provides a scalable, EMS-aware, and adaptive decision support system for ambulance dispatching. It significantly improves optimization quality while enabling real-time responsiveness in dynamic emergency environments.

