# S0 五件事 ↔ 古月居《ROS2 入门 21 讲》对应表

> 2026-11-14 核对。主体文件 §2.1 口径：S0（W1–W2）**只学 5 件事**：节点通信、cv_bridge、tf2、launch、rosbag；ROS 是载体，不是一门并列课程。

## 课程身份（更正此前误记）

- 实际在跟的是 **古月居《ROS2 入门 21 讲》**（B 站 BV16B4y1Q7jQ，古月居 / 胡春旭，2022-06 随 Humble LTS 发布），不是鱼香ROS《动手学 ROS2 / d2lros2》。
- WSL 里的 `ros2_21_tutorials` 就是这门课的配套源码（Gitee `guyuehome/ros2_21_tutorials`），包名 `learning_node` / `learning_topic` / `learning_service` / `learning_launch` / `learning_tf` / `learning_interface` 等。
- 该课随 Humble 发布，与本机 Ubuntu 22.04 + ROS2 Humble **完全对口**。
- 当前进度：**8/21**（1–7 已看完，第 8 讲「话题」刚点开）。

## 五件事对应位置

| S0 五件事 | 21 讲视频 | 图文教程 / 配套源码 | W1–W2 落点 |
|---|---|---|---|
| **节点通信** | 模块二：**6** 工作空间与功能包、**7** 节点、**8** 话题、**9** 服务、**10** 通信接口 | `learning_node` / `learning_topic` / `learning_service` / `learning_interface` | W1 ⑥⑦ 写 Python 发布者 + 订阅者，两个节点互相收发 |
| **launch** | 模块三：**15** Launch：多节点启动与配置脚本（26:12） | `learning_launch`；图文《ROS 2 常用工具》launch.py 一节 | W2 ② `ros2 launch realsense2_camera ... align_depth.enable:=true` |
| **tf2** | 模块三：**16** TF：机器人坐标系管理神器（30:33） | `learning_tf`；图文《常用工具》§3.4.3 tf 数据可视化 | W2 ⑤ `view_frames` 画一次坐标树 |
| **rosbag** | ⚠️ **21 讲没有这一讲** | 图文《ROS 2 常用工具》**§3.5 rosbag：数据记录与回放**（`ros2 bag record / info / play`）；社区《[ros2学习]-CLI Tools - 记录和播放数据》 | W2 ⑥ 录一次包并回放 |
| **cv_bridge** | ⚠️ **21 讲没有独立讲**；但第 10 讲「通信接口」示例已用 `CvBridge()` 订阅 `Image` + OpenCV 做红色物体检测，正是 W2 图像管线的原型 | 图文《ROS 2 视觉应用：让机器人看懂世界》**§7.3 在 ROS 2 中使用 OpenCV**（`learning_cv` 的 `cv_bridge_test`）；社区《ROS2 图像管线实战：cv_bridge、image_transport 与压缩传输》 | W2 ③ cv_bridge + image_transport、④ 订阅 `/image_raw` → OpenCV → 发布结果 |

## S0 跳看顺序（不顺刷 21 讲）

- **W1（现在）**：看完第 8 讲 → 立刻敲 W1 ⑥⑦；第 9 讲看一遍知道 service 是什么即可。**跳过 10–14**（通信接口 / 动作 / 参数 / 分布式 / DDS），第 10 讲的 CvBridge 示例留到 W2 再翻。
- **W2（11.23 起）**：跳看 **15 Launch → 16 TF**；再补图文 §3.5 rosbag、§7.3 OpenCV / cv_bridge。
- 17–21（URDF / Gazebo / Rviz / RQT / 总结）：S1（W3 MuJoCo + rviz2）用到 19 再回来看，其余先放着。
- §7 原则不变：**视频 : 动手 = 1 : 2**；每个节点写自己的 msg，别搭完 demo 就过；卡住 2h 换途径；环境配置 2h 原则。W1 无 🖐 手写要求。

## 版本坑（Humble 专用）

古月居图文教程部分页面已更新成 **Jazzy** 命令，本机是 Humble，遇到 `ros-jazzy-xxx` 一律换成 `ros-humble-xxx`：

```bash
sudo apt install ros-humble-vision-opencv libopencv-dev python3-opencv   # 不是 ros-jazzy-vision-opencv
```

## 口径偏离（如实记录，待用户决定是否修订主体文件）

主体文件 §7 资源表写「**鱼香ROS 主跟，古月居查漏**」，而实际主线跟的是古月居 21 讲。S0 只学 5 件事、ROS 是载体，跟哪套课不改变 W1–W42 时间轴与 v1→v5 项目主线；但资源优先级与主体文件字面不一致，**不擅自改主体文件**，待用户确认后再决定是否把 §7 补注为「古月居 21 讲主跟，鱼香ROS 查漏 / 深入」。
