# Baymax Learning Roadmap

> [!info] About this file
> What Giahy needs to learn to build the Baymax Home Companion Robot — from zero to full embedded system.
> Maintained by MIMIR. Updated as progress is made. Last updated: 2026-05-19
> See also: [[System/Tasks]] · [[System/Loose Ends]]

---

## The Goal

Build a bipedal/quadruped home companion robot modeled after Baymax (Big Hero 6). Aesthetic: Nendoroid/Good Smile hard panel lines.

> [!tip] Two Phases
> **Phase 1 (now → Oct 2026):** Software only — simulation, RL training, policy architecture. **$0 cost.**
> 
> **Phase 2 (post-funding):** Physical hardware — embedded systems, motors, sensors, 3D printing.

---

## Where You Are Now

> [!warning] Starting Point
> | Skill | Status |
> |-------|--------|
> | Java OOP | ✅ In progress (CS 1410) |
> | Calculus I | ✅ Complete |
> | Calculus II | 🔄 In progress (retake) |
> | Physics I | 🔄 Retake in progress |
> | Linear Algebra | ❌ Not yet — formal course Spring 2027 |
> | Python | ❌ Not yet — self-taught this summer |
> | ML / RL | ❌ Not yet |
> | Isaac Sim | ❌ Not yet |
> | Embedded systems | ❌ Not yet |

---

## Phase 1 — Software & Simulation

### Stage 1 — Python Fluency
> *June–July 2026 · ~6 hrs/week*

Most robotics tools (Isaac Sim, PyTorch, NumPy) are Python. Java OOP transfers — same concepts, different syntax.

| Topic                                    | Why It Matters                | Resource                                       |
| ---------------------------------------- | ----------------------------- | ---------------------------------------------- |
| Python syntax + data structures          | Foundation for everything     | Python.org tutorial, Automate the Boring Stuff |
| Functions, classes, modules              | Direct transfer from Java OOP | Same as above                                  |
| File I/O, pip, virtual environments      | Working like a real dev       | Python docs                                    |
| NumPy basics — arrays, indexing, slicing | Prereq for all ML work        | NumPy quickstart                               |

> [!success] Milestone
> Write a Python script that does matrix multiplication using NumPy without looking it up.

---

### Stage 2 — Linear Algebra in Code
> *July–August 2026 · ~6 hrs/week*
> Formal course (MATH 2250) is Spring 2027 — learn the intuition through code first.

You don't need to derive proofs. You need to understand what these operations mean physically for robotics.

| Topic                                         | Why It Matters for Baymax                      |
| --------------------------------------------- | ---------------------------------------------- |
| Vectors — magnitude, direction, dot product   | Represents joint positions, velocities, forces |
| Matrices — multiplication, transpose, inverse | Transforms between coordinate frames           |
| Linear transformations                        | How joint angles map to end-effector position  |
| Eigenvalues/eigenvectors                      | Understanding stability, PCA (less urgent)     |
| Gradients and partial derivatives             | How neural networks learn (backprop)           |

> [!tip] Resource
> 3Blue1Brown "Essence of Linear Algebra" (YouTube) — watch before doing NumPy exercises. Then implement every concept in NumPy.

> [!success] Milestone
> Implement forward kinematics for a 2-joint arm in Python using matrix transforms.

---

### Stage 3 — ML Fundamentals
> *August–September 2026 · ~6 hrs/week*

You don't need to master all of ML — just enough to understand what RL is doing and why.

| Topic | Why It Matters |
|-------|----------------|
| Neural networks (layers, weights, activations) | RL policies are neural nets |
| Loss functions and gradient descent | How the network learns |
| PyTorch basics — tensors, autograd | Isaac Sim RL uses PyTorch |
| Supervised vs reinforcement learning | Context for the project |

> [!tip] Resource
> fast.ai Part 1 (free, practical) or 3Blue1Brown "Neural Networks" series first.

> [!success] Milestone
> Train a simple neural net on a toy dataset in PyTorch (e.g., XOR or MNIST digits).

---

### Stage 4 — Reinforcement Learning Concepts
> *September–October 2026 · ~6 hrs/week*

This is the core of how Baymax will learn to walk.

| Topic | Why It Matters |
|-------|----------------|
| States, actions, rewards, episodes | The RL framework |
| Policy vs value function | Two ways to represent what the agent has learned |
| Q-learning (basic) | Foundation of modern RL |
| Policy gradient methods | PPO is a policy gradient method |
| PPO — Proximal Policy Optimization | The specific algorithm Isaac Lab uses for locomotion |
| Reward shaping | How you define "good walking" for the robot |

> [!tip] Resource
> Spinning Up by OpenAI (free) — chapters on RL basics and PPO specifically.

> [!success] Milestone
> Run a pre-built OpenAI Gym environment (CartPole) with a PPO agent from stable-baselines3.

---

### Stage 5 — Isaac Sim / Isaac Lab Setup
> *October 2026 · ~6 hrs/week*
> ⚠️ Use the desktop (RTX 3060 Super, i9 10th gen, 16GB RAM) — laptop cannot run this.

| Topic | Why It Matters |
|-------|----------------|
| Isaac Sim installation and navigation | Environment setup |
| USD (Universal Scene Description) | How Isaac Sim represents robots and scenes |
| Importing a URDF robot model | Getting a biped/quadruped into the sim |
| Isaac Lab locomotion examples | Pre-built walking policies to study and run |
| Observing policy training in real-time | Understanding what the sim is doing |

> [!danger] Portfolio Deadline — October 2026
> "I set up Isaac Sim, imported a bipedal robot model, ran a pre-built locomotion policy, and understand the policy architecture."
> This is the deliverable for UC Berkeley + UCSD apps due November 2026.

---

## Phase 2 — Embedded Hardware

> [!note] Not Yet
> Begins after hardware funding. Not blocking Phase 1. Documented here for planning only.

### Stage 6 — Embedded Systems Fundamentals
| Topic | Why It Matters |
|-------|----------------|
| C/C++ for microcontrollers | Most motor controllers run C/C++ |
| Arduino / STM32 basics | Entry point for embedded dev |
| Digital I/O, PWM, UART, I2C, SPI | How a microcontroller talks to sensors and motors |
| Real-time constraints | Walking requires millisecond-level control loops |

### Stage 7 — Motor Control
| Topic                                    | Why It Matters                                    |
| ---------------------------------------- | ------------------------------------------------- |
| Servo vs brushless vs quasi-direct drive | Baymax uses quasi-direct drive (like MIT Cheetah) |
| Motor driver boards (ODrive, VESC)       | Interface between controller and motor            |
| PID control                              | Keeping joints at target angles                   |
| Torque control vs position control       | Quasi-direct drive uses torque control            |

### Stage 8 — Sensors & Perception
| Topic | Why It Matters |
|-------|----------------|
| IMU (accelerometer + gyroscope) | Balance and orientation |
| Depth camera (e.g., Intel RealSense) | Obstacle detection |
| Sensor fusion | Combining IMU + camera for stable state estimation |

### Stage 9 — ROS2
| Topic | Why It Matters |
|-------|----------------|
| ROS2 concepts — nodes, topics, services | Standard middleware for robotics |
| Publishing / subscribing to sensor data | Getting data from sensors to the policy |
| Isaac Sim ↔ ROS2 bridge | Deploying sim-trained policy to real hardware |

### Stage 10 — Sim-to-Real Transfer
| Topic | Why It Matters |
|-------|----------------|
| Domain randomization | Making the policy robust to real-world variation |
| Edge inference — PyTorch on embedded hardware | Running the policy on the robot's onboard computer |
| Latency and compute constraints | Real hardware has less compute than the desktop |

---

## What You Don't Need Yet

> [!abstract] Skip for now
> - Computer vision / SLAM — not needed for walking
> - Full mechanical design — architecture already sketched
> - Manufacturing / tolerances — Phase 2 problem
> - ROS1 — skip entirely, go straight to ROS2

---

## Open Questions

> [!question] Unresolved
> - [ ] Which robot model to use in Isaac Sim as the Baymax stand-in? (Unitree H1, MIT Humanoid, custom URDF)
> - [ ] What onboard computer for Phase 2 hardware? (NVIDIA Jetson Orin vs Raspberry Pi 5)
> - [ ] Warming massage belt — separate track or back-burner?
