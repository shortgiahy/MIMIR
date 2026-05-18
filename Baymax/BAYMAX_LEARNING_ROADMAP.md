# Baymax Learning Roadmap

> What Giahy needs to learn to build the Baymax Home Companion Robot — from zero to full embedded system.
> Maintained by MIMIR. Updated as progress is made.
> Last updated: 2026-05-18
> See also: [[MIMIR_TASKS]] · [[LOOSE_ENDS]]

---

## The Goal

Build a bipedal/quadruped home companion robot modeled after Baymax (Big Hero 6). Aesthetic: Nendoroid/Good Smile hard panel lines.

Two phases:
- **Phase 1 (now → Oct 2026):** Software only — simulation, RL training, policy architecture. $0 cost.
- **Phase 2 (post-funding):** Physical hardware — embedded systems, motors, sensors, 3D printing.

---

## Where You Are Now

- Java OOP (CS 1410, in progress)
- Calculus I completed, Calculus II in progress (retake)
- Physics I completed (retake in progress), Physics II in Fall 2026
- Linear Algebra: **not yet** — formal course is Spring 2027 (MATH 2250)
- Python: **not yet** — self-taught starting this summer
- ML/RL: **not yet**
- Isaac Sim: **not yet**
- Embedded systems: **not yet**

---

## Phase 1 Learning Path: Software & Simulation

### Stage 1 — Python Fluency
*Timeline: June–July 2026 | ~6 hrs/week*

Most robotics tools (Isaac Sim, PyTorch, NumPy) are Python. Java OOP transfers — same concepts, different syntax.

| Topic | Why It Matters | Resource |
|-------|---------------|----------|
| Python syntax + data structures | Foundation for everything | Python.org tutorial, Automate the Boring Stuff |
| Functions, classes, modules | Direct transfer from Java OOP | Same as above |
| File I/O, pip, virtual environments | Working like a real dev | Python docs |
| NumPy basics — arrays, indexing, slicing | Prereq for all ML work | NumPy quickstart |

**Milestone:** Write a Python script that does matrix multiplication using NumPy without looking it up.

---

### Stage 2 — Linear Algebra in Code
*Timeline: July–August 2026 | ~6 hrs/week*
*Note: Formal course (MATH 2250) is Spring 2027 — learn the intuition through code first.*

You don't need to derive proofs. You need to understand what these operations mean physically for robotics.

| Topic | Why It Matters for Baymax |
|-------|--------------------------|
| Vectors — magnitude, direction, dot product | Represents joint positions, velocities, forces |
| Matrices — multiplication, transpose, inverse | Transforms between coordinate frames |
| Linear transformations | How joint angles map to end-effector position |
| Eigenvalues/eigenvectors | Understanding stability, PCA (less urgent) |
| Gradients and partial derivatives | How neural networks learn (backprop) |

**Resource:** 3Blue1Brown "Essence of Linear Algebra" (YouTube) — watch before doing NumPy exercises. Then implement every concept in NumPy.

**Milestone:** Implement forward kinematics for a 2-joint arm in Python using matrix transforms.

---

### Stage 3 — ML Fundamentals
*Timeline: August–September 2026 | ~6 hrs/week*

You don't need to master all of ML — just enough to understand what RL is doing and why.

| Topic | Why It Matters |
|-------|---------------|
| What a neural network is (layers, weights, activations) | RL policies are neural nets |
| Loss functions and gradient descent | How the network learns |
| PyTorch basics — tensors, autograd | Isaac Sim RL uses PyTorch |
| What supervised vs reinforcement learning is | Context for the project |

**Resource:** fast.ai Part 1 (free, practical) or 3Blue1Brown "Neural Networks" series first.

**Milestone:** Train a simple neural net on a toy dataset in PyTorch (e.g., classify XOR, or MNIST digits).

---

### Stage 4 — Reinforcement Learning Concepts
*Timeline: September–October 2026 | ~6 hrs/week*

This is the core of how Baymax will learn to walk.

| Topic | Why It Matters |
|-------|---------------|
| States, actions, rewards, episodes | The RL framework |
| Policy vs value function | Two ways to represent what the agent has learned |
| Q-learning (basic) | Foundation of modern RL |
| Policy gradient methods | PPO (used in Isaac Sim) is a policy gradient method |
| PPO — Proximal Policy Optimization | The specific algorithm Isaac Lab uses for locomotion |
| Reward shaping | How you define "good walking" for the robot |

**Resource:** Spinning Up by OpenAI (free) — chapters on RL basics and PPO specifically.

**Milestone:** Run a pre-built OpenAI Gym environment (e.g., CartPole) with a PPO agent from stable-baselines3.

---

### Stage 5 — Isaac Sim / Isaac Lab Setup
*Timeline: October 2026 | ~6 hrs/week*
*Hardware: Use the desktop (RTX 3060 Super, i9 10th gen, 16GB RAM) — laptop cannot run this.*

| Topic | Why It Matters |
|-------|---------------|
| Isaac Sim installation and navigation | Environment setup |
| USD (Universal Scene Description) | How Isaac Sim represents robots and scenes |
| Importing a URDF robot model | Getting a biped/quadruped into the sim |
| Isaac Lab locomotion examples | Pre-built walking policies to study and run |
| Observing policy training in real-time | Understanding what the sim is doing |

**Portfolio milestone by October 2026:** "I set up Isaac Sim, imported a bipedal robot model, ran a pre-built locomotion policy, and understand the policy architecture." — This is sufficient for UC Berkeley + UCSD apps.

---

## Phase 2 Learning Path: Embedded Hardware

*Begins after hardware funding. Not blocking Phase 1. Listed here for planning.*

### Stage 6 — Embedded Systems Fundamentals
| Topic | Why It Matters |
|-------|---------------|
| C/C++ for microcontrollers | Most motor controllers run C/C++ |
| Arduino / STM32 basics | Entry point for embedded dev |
| Digital I/O, PWM, UART, I2C, SPI | How a microcontroller talks to sensors and motors |
| Real-time constraints | Walking requires millisecond-level control loops |

### Stage 7 — Motor Control
| Topic | Why It Matters |
|-------|---------------|
| Servo vs brushless vs quasi-direct drive | Baymax uses quasi-direct drive (like MIT Cheetah) |
| Motor driver boards (e.g., ODrive, VESC) | Interface between controller and motor |
| PID control | Keeping joints at target angles |
| Torque control vs position control | Quasi-direct drive uses torque control |

### Stage 8 — Sensors & Perception
| Topic | Why It Matters |
|-------|---------------|
| IMU (accelerometer + gyroscope) | Balance and orientation |
| Depth camera (e.g., Intel RealSense) | Obstacle detection |
| Sensor fusion | Combining IMU + camera for stable state estimation |

### Stage 9 — ROS2 (Robot Operating System)
| Topic | Why It Matters |
|-------|---------------|
| ROS2 concepts — nodes, topics, services | Standard middleware for robotics |
| Publishing/subscribing to sensor data | Getting data from sensors to the policy |
| Isaac Sim ↔ ROS2 bridge | Deploying sim-trained policy to real hardware |

### Stage 10 — Sim-to-Real Transfer
| Topic | Why It Matters |
|-------|---------------|
| Domain randomization | Making the sim-trained policy robust to real-world variation |
| Edge inference — deploying PyTorch models to embedded hardware | Running the policy on the robot's onboard computer |
| Latency and compute constraints | Real hardware has less compute than your desktop |

---

## What You DON'T Need to Learn (Yet)

- Computer vision / SLAM — not needed for walking
- Full mechanical design — architecture is already sketched
- Manufacturing / tolerances — Phase 2 problem
- ROS1 — skip it, go straight to ROS2

---

## Open Questions / Flags

- [ ] Which bipedal/quadruped robot model to use in Isaac Sim as the Baymax stand-in? (Options: Unitree H1, MIT Humanoid, custom URDF)
- [ ] What onboard computer for Phase 2 hardware? (Options: NVIDIA Jetson Orin, Raspberry Pi 5)
- [ ] Warming massage belt project — separate track or back-burner?
