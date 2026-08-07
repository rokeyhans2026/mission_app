# 🐢 Mission App — ROS TurtleBot Project

A fully autonomous mobile robot application built on **ROS (Robot Operating System)** and **TurtleBot3**, designed for intelligent navigation, mapping, and mission execution in indoor environments.

## 🎯 Project Overview

Mission App is a ROS-based software stack that transforms a TurtleBot3 into a self-driving mission robot. It handles everything from real-time mapping and localization to autonomous navigation and goal-oriented task execution.

## ✨ Features

- 🗺️ **SLAM-based Mapping** — Real-time map construction using `gmapping`
- 📍 **AMCL Localization** — Precise pose tracking with adaptive Monte Carlo localization
- 🧭 **Autonomous Navigation** — MoveBase navigation with dynamic obstacle avoidance
- 🎯 **Multi-Goal Mission Execution** — Sequential waypoint following for mission tasks
- 📊 **Live Visualization** — RViz-based real-time status monitoring
- 🛰️ **Modular Architecture** — Clean separation of packages for easy extension

## 🏗️ Architecture

```
mission_app/
├── mission_bringup/       # Launch files & configuration
├── mission_nav/           # Navigation stack integration
├── mission_slam/          # Mapping & localization nodes
├── mission_mission/       # Mission/waypoint executor
└── mission_msgs/          # Custom message definitions
```

## 🚀 Quick Start

### Prerequisites

- Ubuntu 20.04 LTS (or later)
- ROS Noetic
- TurtleBot3 (Burger / Waffle)
- Python 3.8+

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/yourname/mission_app.git
cd mission_app

# 2. Build the workspace
catkin_make

# 3. Source the environment
source devel/setup.bash
```

### Running the Robot

```bash
# Terminal 1: Launch the robot (real hardware or Gazebo)
export TURTLEBOT3_MODEL=burger
roslaunch mission_bringup turtlebot3_bringup.launch

# Terminal 2: Start SLAM mapping
roslaunch mission_slam slam.launch

# Terminal 3: Launch RViz for visualization
rosrun rviz rviz -d $(rospack find mission_slam)/rviz/mission.rviz
```

### Executing a Mission

```bash
# Send a mission (e.g., patrol 3 waypoints)
rosrun mission_mission run_mission.py --goals "1,1 2,2 3,3"
```

## 🛠️ Package Details

| Package | Description |
|---------|-------------|
| `mission_bringup` | Robot bring-up, URDF, launch files |
| `mission_nav` | move_base config, costmaps, planners |
| `mission_slam` | SLAM + AMCL localization |
| `mission_mission` | Waypoint executor & mission logic |
| `mission_msgs` | Custom ROS message types |

## 🧪 Testing

```bash
# Run unit tests
catkin_make run_tests
```

## 📌 Roadmap

- [x] Basic navigation stack
- [x] SLAM-based mapping
- [ ] Object detection integration (YOLO)
- [ ] Multi-robot coordination
- [ ] Web-based remote dashboard

## 📄 License

MIT License. See [LICENSE](LICENSE) for details.

---

*Built with ROS, TurtleBot3, and a whole lot of coffee ☕*
