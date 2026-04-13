# Hybrid Artificial Intelligence Framework for the Ambulance Routing Problem in Emergency Medical Systems

This project proposes a hybrid AI-driven optimization framework for solving the Ambulance Routing Problem (ARP) in dynamic Emergency Medical Services (EMS) environments. The goal is to minimize response time, improve resource allocation, and enhance system robustness under uncertain and time-varying demand.

⚙️ Methodology

The proposed framework integrates three components:

Predictive AI module: estimates spatio-temporal incident demand and patient priority levels from historical EMS data
NSGA-II evolutionary optimization: generates Pareto-optimal ambulance allocation and routing strategies by minimizing response time and priority-weighted cost
Deep Q-Network (DQN) reinforcement learning: enables real-time adaptive dispatching in dynamic environments

A closed-loop hybrid mechanism allows interaction between NSGA-II and DQN, where evolutionary solutions guide the RL state space and RL feedback refines decision-making over time.

<img width="1005" height="1044" alt="Capture d&#39;écran 2026-04-06 112703" src="https://github.com/user-attachments/assets/e30ebe36-be44-4e06-ab42-5a1856a689e2" />


📊 Experimental Results

Experiments were conducted on real EMS datasets and OpenStreetMap road networks across small, medium, and large-scale scenarios.

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

