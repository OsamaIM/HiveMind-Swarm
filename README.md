🧠 HiveMind: LLM-Driven Swarm Intelligence Simulation
Role: AI Engineer / Researcher
Tech Stack: Python, PyGame, Vector Mathematics, Local LLMs (Phi-3), Ollama, Multi-Threading.

📌 Overview
HiveMind is a real-time multi-agent physics simulation that bridges classical algorithmic movement with modern generative AI. It simulates a swarm of 100 autonomous agents navigating a 2D environment. While the agents default to classical "Boids" separation algorithms to avoid threats, the system features a localized Large Language Model (LLM) acting as a centralized Overmind.

Instead of hard-coding every possible reaction, the system queries a local Phi-3 neural network in real-time to analyze the battlefield and dictate emergent survival strategies (Scatter, Defend, or Freeze).

🏗️ System Architecture & Engineering Challenges
Building an LLM-driven physics simulation on consumer hardware presents severe latency and compute constraints. Here is how the system was engineered to run seamlessly at 60 FPS:

1. The Physics Engine (PyGame & Vector Math)
Agent Kinematics: Each drone is programmed using 2D Vector mathematics (pygame.math.Vector2). Position, velocity, and acceleration are calculated iteratively every frame.

Spatial Awareness: Agents calculate their Euclidean distance to the "Predator" (user cursor) in real-time, dynamically scaling their escape vectors based on proximity.

2. The Local AI Integration (Ollama API)
Privacy-First Inference: To eliminate API costs and network latency, the simulation runs a 4-bit quantized version of Microsoft's Phi-3 directly on the local GPU via Ollama.

Prompt Engineering: The simulation state is translated into a highly constrained prompt, forcing the LLM to output precise deterministic commands rather than conversational text.

3. Asynchronous Threading (The 60 FPS Solution)
The Problem: Querying an LLM takes ~2-5 seconds. If this ran on the main game loop, the entire physics engine would freeze, ruining the simulation.

The Solution: I implemented Python Threading. When the Swarm triggers the "Ask Hive Mind" event, the REST API call to the LLM is spun off into a background thread. The physics engine continues running at a smooth 60 FPS using the last known strategy until the AI thread returns a new command.

🦠 Emergent Behaviors (The AI Strategies)
Depending on the LLM's real-time reasoning, it injects one of three mathematical states into the swarm:

SCATTER (Classical Evasion): Agents calculate the exact opposite vector of the predator and scale their velocity to sprint away.

DEFEND (Swarm Attack): Agents invert their threat logic, calculating an intersection vector to swarm and engulf the predator.

FREEZE (Camouflage): Velocity is dynamically multiplied by a friction coefficient (0.8), rapidly halting the swarm and altering their rendering color to blend into the environment.

🚀 Research Application: Sports Analytics Integration
Why build this?
This simulation acts as the foundational sandbox for my ongoing research into Optimizing Sports Referee Positioning.

By establishing a working multi-agent vector environment with dynamic obstacle avoidance, the next iteration of this engine will replace the "Predator" with a "Focal Point" (the ball), and the agents with "Players". A specialized Referee Agent will then be trained using these exact vector mechanics to maintain an optimal, unobstructed line-of-sight to the ball while dynamically dodging colliding players.

💻 How to Run Locally
Bash
# 1. Clone the repository
git clone https://github.com/OsamaIM/HiveMind-Swarm.git
cd HiveMind-Swarm

# 2. Install dependencies
pip install pygame requests

# 3. Ensure your local Ollama instance is running with Phi-3
ollama pull phi3

# 4. Launch the simulation
python swarm_ai.py