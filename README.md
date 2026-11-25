# Move to Pose (`move_to_pose`)

A ROS 2 (Jazzy) package implementing a polar-coordinate proportional feedback controller ($\rho, \alpha, \beta$) to drive a mobile robot to a target 2D pose $(x, y, \theta)$ published over `/topic_goal`.

* **Task 1 (`turtlesim_go_to_pose`):** Closed-loop pose control in `turtlesim`.
* **Task 2 (`tb3_go_to_pose`):** Closed-loop pose control for a differential-drive **TurtleBot3 Burger** in Gazebo Harmonic (using `geometry_msgs/msg/TwistStamped` on `/cmd_vel` and odometry feedback from `/odom`).

---

## Prerequisites

* **ROS 2 Jazzy**
* **TurtleSim** (`ros-jazzy-turtlesim`)
* **TurtleBot3 Gazebo Simulation** (`ros-jazzy-turtlebot3-gazebo`):
  ```bash
  sudo apt update
  sudo apt install -y ros-jazzy-turtlebot3-gazebo
  ```

---

## 1. Extract and Build the Package

1. **Extract `move_to_pose.zip` into the `src/` directory of your ROS 2 workspace** (for example, `~/ros2_ws/src/`):
   ```bash
   unzip move_to_pose.zip -d ~/ros2_ws/src/
   ```
   Verify that your directory layout matches:
   ```text
   ~/ros2_ws/
   └── src/
       └── move_to_pose/
           ├── move_to_pose/
           │   ├── __init__.py
           │   ├── turtlesim_go_to_pose.py
           │   └── tb3_go_to_pose.py
           ├── resource/
           ├── test/
           ├── package.xml
           ├── setup.cfg
           └── setup.py
   ```

2. **Build and source the workspace:**
   ```bash
   cd ~/ros2_ws
   colcon build --packages-select move_to_pose
   source install/setup.bash
   ```

---

## 2. Running Task 1: TurtleSim

Open **three separate terminals** (make sure to source `~/ros2_ws/install/setup.bash` in each):

* **Terminal 1 — Start the TurtleSim simulator:**
  ```bash
  ros2 run turtlesim turtlesim_node
  ```

* **Terminal 2 — Start the TurtleSim proportional controller:**
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 run move_to_pose turtlesim_go_to_pose
  ```

* **Terminal 3 — Publish a target goal pose $(x, y, \theta)$ in radians:**
  ```bash
  ros2 topic pub --once /topic_goal geometry_msgs/msg/Pose2D "{x: 1.0, y: 1.0, theta: 1.5}"
  ```

---

## 3. Running Task 2: TurtleBot3 in Gazebo

Open **three separate terminals** (make sure to source `~/ros2_ws/install/setup.bash` in each):

* **Terminal 1 — Launch TurtleBot3 Burger in an empty Gazebo world:**
  ```bash
  export TURTLEBOT3_MODEL=burger
  ros2 launch turtlebot3_gazebo empty_world.launch.py
  ```

* **Terminal 2 — Start the TurtleBot3 proportional controller:**
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 run move_to_pose tb3_go_to_pose
  ```

* **Terminal 3 — Publish a target goal pose $(x, y, \theta)$ in radians:**
  ```bash
  ros2 topic pub --once /topic_goal geometry_msgs/msg/Pose2D "{x: 1.0, y: 1.0, theta: 1.5}"
  ```

> **Note:** You do not need to restart the simulator or controller node between runs—simply publish a new `Pose2D` message to `/topic_goal` with updated `x`, `y`, and `theta` values.
