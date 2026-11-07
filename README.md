# Snake Game AI — A* Pathfinding vs Q-Learning

**An interactive full-stack Snake game for exploring and comparing classical pathfinding and reinforcement learning.** Play manually, watch autonomous agents, train a Q-Learning policy, and compare their results in real time.

**Stack:** Python · FastAPI · WebSockets · React 18 · Redux Toolkit · Vite · SQLite · SQLAlchemy · NumPy · pytest · Vitest

## Demo

[Watch the application demonstration](demo/snake-game-demo.mp4) · [Architecture diagrams](#architecture)

> The demo is included in this repository. GitHub may display the video as a file rather than an embedded player.

## Features

- **Three gameplay modes:** keyboard-controlled Snake, autonomous A* agent, and Q-Learning agent.
- **A* vs Q-Learning Battle Arena:** two independent boards with live score, steps, and decision-time comparisons.
- **Training and benchmarking:** run reinforcement-learning episodes, inspect learning curves, and benchmark A*.
- **Analytics dashboard:** compare historical results, scores, and agent performance; export results.
- **Dynamic and static obstacles**, configurable game speed, and a 25 × 25 board.
- **Real-time synchronization:** FastAPI REST endpoints and WebSocket game-state updates.
- **Persistence:** SQLite stores games, events, agent statistics, and training history.

## AI approaches

| Agent | Approach | Implementation |
| --- | --- | --- |
| **A*** | Heuristic graph search toward food | Manhattan-distance heuristic, priority queue, flood-fill safety checks and tail chasing |
| **Q-Learning** | Tabular reinforcement learning | State encoding, epsilon-greedy exploration, Bellman updates and a persisted Q-table |

A* plans paths using the current board state; Q-Learning improves its action policy through repeated episodes. This project compares the approaches experimentally rather than claiming that either always wins.

## Reported performance

Measurements from the project performance report (environment and training affect results):

| Metric | A* | Q-Learning |
| --- | --- | --- |
| Average inference latency per step | < 5 ms | < 0.5 ms |
| Average score, no obstacles / trained RL | 18–25 | 4–8 after 500 episodes |
| Survival rate, same reported scenarios | 95% | 55% |

**Note:** these score scenarios are not identical experimental conditions; they should not be treated as a controlled head-to-head comparison. The report also documents **107 backend pytest tests**, frontend Vitest tests, and a GitHub Actions CI workflow. These are documented project results, not tests rerun for this publication.

## Architecture

```text
React 18 + Redux Toolkit + HTML5 Canvas
             | REST / WebSocket
             v
       FastAPI backend
       /      |       \
  Game engine  AI agents  SQLAlchemy ORM
               |               |
          A* / Q-Learning     SQLite
```

<details><summary>Deployment diagram</summary>

![Deployment architecture](assets/deployment-diagram.png)

</details>

<details><summary>Package diagram</summary>

![Package architecture](assets/package-diagram.png)

</details>

<details><summary>Class diagram</summary>

![Class diagram](assets/class-diagram.png)

</details>

## Run locally

**Requirements:** Python 3.10+, Node.js 18+, npm.

```bash
# Clone your own repository, then enter it
cd Snake-Game-AI

# Linux/macOS: automatic launcher
chmod +x start.sh
./start.sh
```

Windows: run `start.bat`. macOS: `start.command` is also provided.

**Manual startup** (two terminals):

```bash
# Terminal 1 — backend (from repository root)
cd backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python -m uvicorn main:app --reload --port 8000
```

```bash
# Terminal 2 — frontend (from repository root)
cd frontend
npm install
npm run dev
```

Open the frontend at **http://localhost:5174**. API documentation: **http://localhost:8000/docs**. The database is generated on first launch.

## How to use

1. Select **Manual**, **A*** or **Q-Learning** on the welcome screen, then press **Start**.
2. In Manual mode, use **arrow keys** or **Z/Q/S/D** to move.
3. Open **A* vs QL** to watch both agents simultaneously.
4. Open **Training** to run Q-Learning episodes or an A* benchmark.
5. Open **Analysis** to review recorded performance.

## Tests

```bash
cd backend
python -m pytest tests/ -v
```

```bash
cd frontend
npm test
```

The included GitHub Actions workflow runs backend and frontend checks.

## Project layout

```text
backend/
  agents/          # A* and Q-Learning
  game_engine/     # Snake rules, board, obstacles
  models/          # SQLAlchemy models
  routes/          # REST API
  training/        # RL training and benchmarks
  tests/           # Backend tests
frontend/
  src/             # React UI, Redux, services
.github/workflows/ # CI
assets/            # Demo and selected UML diagrams
start.sh / start.bat / start.command
```

## Limitations and next steps

Tabular Q-Learning performance depends on training duration, reward design and obstacle layout. Future experiments could standardize benchmark seeds and compare policies under identical conditions, or explore deep reinforcement learning.

---

*Full-stack AI application project · Python / React · Pathfinding & Reinforcement Learning.*
