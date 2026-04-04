# Interactive Policy Visualization

This directory contains the interactive visualization for trained policies. View it live at:

**https://jonasneves.github.io/aipi590-challenge-3/**

## How to generate trajectory data

Run the trajectory extractor after training a model:

```python
from scripts.trajectory_extractor import extract_trajectory, save_trajectories
from stable_baselines3 import SAC
from pathlib import Path

# Load trained model
model = SAC.load('results/models/best_model')

# Extract 5 episodes of trajectory data
episodes = extract_trajectory(model, n_episodes=5)

# Save as JSON
save_trajectories(episodes, Path('docs/data/trajectories.json'))
```

Or add this to the end of a Colab notebook eval cell:

```python
# Extract and save trajectories for web viewer
from scripts.trajectory_extractor import extract_trajectory, save_trajectories

episodes = extract_trajectory(model, n_episodes=5, deterministic=True)
save_trajectories(episodes, Path('docs/data/trajectories.json'))
publish_artifacts('add interactive trajectory viewer')
```

## Files

- `index.html` — Interactive viewer (Three.js + visualization controls)
- `data/trajectories.json` — Trajectory data (joint angles over time)
- `README.md` — This file

## Features

- **Episode selection** — Choose which rollout to view
- **Playback controls** — Play, pause, reset, scrub
- **Speed control** — Slow down or speed up playback
- **Stats** — View success/failure status, duration, end effector position
- **Interactive camera** — Rotate and zoom the 3D view

## Data format

Each trajectory contains:

```json
{
  "episode": 0,
  "task": "FetchPickAndPlace-v4",
  "success": true,
  "length": 150,
  "timesteps": [
    {
      "step": 0,
      "joint_positions": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0],
      "gripper_position": [0.04, 0.04],
      "object_position": [1.3, 0.75, 0.42],
      "goal_position": [1.35, 0.75, 0.45]
    }
  ]
}
```

- `joint_positions`: 7-DOF arm (radians)
- `gripper_position`: Left/right finger positions (m)
- `object_position`: End effector / object position (x, y, z in m)
- `goal_position`: Goal position (x, y, z in m)
