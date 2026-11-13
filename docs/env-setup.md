# 环境搭建记录（S0 W1，2026-11-13）

> 主体文件 §7 使用原则④：环境配置踩坑记进 `docs/`，这些笔记本身就是面试谈资。

## 机器与版本决策

| 项 | 情况 |
|---|---|
| 原系统 | Win11 家庭版 Build 26200，UEFI + GPT，1TB NVMe，16G 内存 |
| 原环境 | WSL2 Ubuntu 22.04 + ROS2 **Humble**（11 月初已跟完 ROS2 21 讲教程、跑过 YOLO） |
| 目标 | 删 WSL，只留双系统（简单环境）；D435i 是 USB 相机，WSL2 访问不了 USB 硬件 |
| 定版 | **Ubuntu 22.04.5 LTS + ROS2 Humble**——延续 WSL 现有 Humble 生态资产；**ROS2 版本仍待与课题组确认**（主体文件口径：版本跟课题组走，未指定选 Jazzy） |

## 磁盘空间（踩坑最多的一段）

1. C 盘 278G 只剩 46G，Ubuntu 从 D 盘切；D 盘 650G 剩 180G
2. 压缩卷只能压出 **35047MB（34G）**——磁盘管理提示“不可移动文件”，`Resize-Partition`（管理员）报 `Size Not Supported`（NTFS 最小卷大小限制），两者都绕不过
3. 真凶：**`D:\pagefile.sys` 20G 页面文件**挡在卷中部；删游戏（地下城与勇士 52.4G + 无畏契约 26.7G）腾出的空闲在它**前部**，尾部连续空闲仍不变
4. 解法：`sysdm.cpl` → 高级 → 性能设置 → 高级 → 虚拟内存更改 → D 盘设“无分页文件”、C 盘自定义 4096/4096 → **重启** → 可用压缩空间 34G → **137915MB**
5. 压缩 102400MB（100G）；D 盘缩到 550G，留 34G 空闲

**教训**：压缩卷看的是“尾部连续空闲”，不是“总可用空间”；页面文件是最常见的不可移动文件。

## 分区与安装

- 未分配 101G → **swap 8G + ext4 92G 挂载 `/`**
- **EFI 用 Windows 现有的 300MB 分区，绝不勾格式化**（否则 Windows 引导丢失）
- 安装类型选“其他”手动分区；不勾“下载更新”，勾“第三方软件”
- 引导器装整块磁盘 `/dev/nvme0n1`；用户名 `ros`（与 WSL 一致，搬资产路径不变）
- Rufus 4.15p：GPT + UEFI（非 CSM）+ FAT32 + “以 ISO 镜像模式写入”
- ⚠️ Rufus 弹“**检测到被吊销的 UEFI 引导加载器**”：22.04.5 ISO 的 shim 被 dbx 标记，非病毒；若 U 盘启动报 Security Violation，进 BIOS 关 **Secure Boot**

## 资产保全

- 删 WSL 前：`wsl -d Ubuntu-22.04 -- bash -lc "cd ~ && tar czf /mnt/d/wsl-ros-assets.tar.gz ..."`
- 产出 `D:\wsl-ros-assets.tar.gz` 691M / 1353 项，`gzip -t` 校验通过
- 新系统恢复：`tar xzf /mnt/d/wsl-ros-assets.tar.gz -C ~`
- 代码同步：GitHub 仓库 `industrial-vision-log`，两边 clone / pull

## 待办

- [ ] W1 验收：`ros2 run demo_nodes_cpp talker`、turtlesim、Python 发布者 + 订阅者互发消息
- [ ] W2：D435i 三组精度数字 → `docs/camera_d435i.md`
- [ ] GRUB 稳定后注销 WSL：`wsl --unregister Ubuntu-22.04`
