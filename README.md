# Hybrid Artificial Intelligence Framework for the Ambulance Routing Problem in Emergency Medical Systems

Abstract
The Ambulance Routing Problem (ARP) is a critical challenge in Emergency Medical Services (EMS), where mini-
mizing response time, travel distance, and improving coverage are essential for saving lives. Traditional optimization
methods often fail to capture the dynamic, stochastic, and priority-driven nature of real-world emergency systems.
In this paper, we propose a Hybrid EMS Optimization Framework combining Artificial Intelligence (AI), Multi-
Objective Evolutionary Optimization, and Reinforcement Learning (RL). A predictive AI module estimates spatio-
temporal incident probabilities and assigns dynamic priority levels based on historical EMS data. These predictions are
used to construct risk-aware demand models and simulation scenarios. A Non-Dominated Sorting Genetic Algorithm
II (NSGA-II) is then applied to generate Pareto-optimal ambulance deployment and routing strategies, optimizing
response time, coverage, and operational cost. In parallel, a Deep Q-Network (DQN)-based Reinforcement Learning
agent is introduced to learn adaptive dispatch policies in a dynamic environment, enabling real-time decision-making
under uncertainty. A hybrid interaction loop combines NSGA-II and RL, ensuring both long-term optimality and short-
term responsiveness. Experiments on large-scale real EMS datasets and OpenStreetMap road networks demonstrate
that the proposed framework improves Pareto efficiency, reduces response time, and increases system robustness.
Keywords: Ambulance Routing Problem; Emergency Medical Services; Hybrid Artificial Intelligence; NSGA-II; Reinforcement Learning;
DQN; Multi-Objective Optimization; OpenStreetMap; Intelligent Transportation Systems
