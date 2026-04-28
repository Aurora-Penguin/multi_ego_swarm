# Multi EGO Swarm

**Multi EGO Swarm** 是一个基于 EGO-Planner 的多无人机/多无人车集群协同轨迹规划与自主导航系统。它在复杂的、有障碍物的三维环境中，能够实现多智能体的无碰撞、平滑且非保守的轨迹生成与跟踪。

## 1. 核心功能
* **多机协同与避碰**：采用相对位置共享和安全距离约束进行实时集群避碰规划。
* **局部/全局地图感知**：集成了从局部感知（如激光雷达/视觉）到全局网格地图（Grid Map）的建图环境。
* **多智能体框架**：系统内含 `swarm_bridge` 等通信和协同模块。
* **仿真与验证**：项目内部集成了 UAV 仿真器 (`uav_simulator`) 以及多节点管理框架，可直接通过 `rviz` 和 `gazebo` 验证。

## 2. 依赖项与环境
* **操作系统**: Ubuntu 18.04 (ROS Melodic) 或 Ubuntu 20.04 (ROS Noetic)
* **核心依赖**:
  * [Eigen3](http://eigen.tuxfamily.org/index.php) - 用于矩阵与代数计算
  * [PCL (Point Cloud Library)](http://pointclouds.org/) - 用于点云和环境感知
  * [ROS](http://wiki.ros.org/ROS/Installation) (含常用 tf, roscpp, std_msgs, sensor_msgs, msg_generation, visualization_msgs)

## 3. 编译指南
建议在一个标准的 ROS 工作空间下克隆和编译代码（如果你已经完成了该步骤，可略过）：

```bash
# 对于 catkin workspace
cd ~/workspace/ego_planner_ws
catkin_make
# 或者使用 catkin build
```

## 4. 运行示例

在编译完成并 `source devel/setup.bash` 当前工作空间后，你可以通过加载以下 `launch` 文件启动不同模式：

### 4.1 单机 Gazebo 仿真 
```bash
roslaunch ego_planner single_run_in_gazebo.launch
```
或者带激光雷达配置的仿真：
```bash
roslaunch ego_planner single_run_in_gazebo_lidar.launch
```

### 4.2 多机（集群）仿真
```bash
# 启动多机集群规划(如基于 LiDAR)
roslaunch ego_planner multi_ego_planner_lidar.launch
```

> **注意**: 在 Rviz 中，可以使用 `2D Nav Goal` 工具在地图中给无人机设定目标点。如果是多机仿真，通常存在专用的目标点分配(Goal assign) 插件或脚本。

## 5. 目录结构
* `src/planner/`: 包含 `ego_planner` 的核心路径规划节点以及集群通信 `swarm_bridge` 和地图构建 `plan_env` 等。
* `src/uav_simulator/`: 用于在仿真环境（如 Rviz 或 Gazebo）中模拟无人机动力学（如 `so3_quadrotor_simulator`）与点云传感器。
* `src/Utils/`: 提供诸如目标点分配 (`assign_goals`)、自动控制管理等杂项插件。

---
**致谢**: 该项目脱胎于优化的 [EGO-Planner](https://github.com/ZJU-FAST-Lab/ego-planner) 框架，感谢原作者们的优秀工作。
