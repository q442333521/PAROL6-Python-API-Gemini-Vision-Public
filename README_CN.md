# PAROL6 Python API - Gemini Vision 智能控制系统

<div align="center">

**基于 Gemini Vision AI 的 PAROL6 机械臂智能控制系统**

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![ROS](https://img.shields.io/badge/ROS-Noetic-brightgreen.svg)](http://wiki.ros.org/noetic)
[![PAROL6](https://img.shields.io/badge/PAROL6-Compatible-orange.svg)](https://github.com/PCrnjak/PAROL-commander-software)

[English](README.md) | 简体中文

</div>

---

## 📖 项目简介

这是一个为 PAROL6 六轴机械臂开发的完整智能控制系统，将传统的 GUI 操作升级为可编程的研究级平台。系统集成了 Google Gemini 2.5 Vision AI、Intel RealSense 深度相机和精确的运动控制，实现了自然语言交互和视觉引导的智能操作。

### ✨ 核心特性

- 🤖 **自然语言控制**
  - 文本和语音输入支持
  - 基于 Gemini 2.5 Flash 的 AI 理解
  - 自动任务分解和执行

- 👁️ **视觉引导操作**
  - Intel RealSense D435I 深度感知
  - 物体检测和 3D 定位
  - 手眼标定系统（毫米级精度）

- 🎯 **精确运动控制**
  - 关节空间和笛卡尔空间控制
  - 平滑轨迹生成（圆形、螺旋、样条曲线）
  - 100Hz 实时控制循环

- 🌐 **远程 API 架构**
  - UDP 客户端-服务器设计
  - 轻量级 Python API（30+ 函数）
  - 跨平台网络控制

- 🛡️ **多层安全保护**
  - 急停监控
  - 关节限位检查
  - 逆运动学验证
  - 速度同步控制

---

## 🎬 快速演示

### 自然语言控制
```
用户: "请抓取红色的方块并放到蓝色盒子里"

AI: 好的，我将执行以下步骤：
    1. 扫描环境寻找红色方块
    2. 规划抓取路径
    3. 执行抓取
    4. 寻找蓝色盒子
    5. 将方块放入盒子

    [自动执行所有操作...]

    ✓ 任务完成！红色方块已放入蓝色盒子。
```

### Python API 控制
```python
from robot_api import *

# 回零
home_robot()

# 移动到目标位置
move_robot_pose([250, 0, 200, 180, 0, 0], speed_percentage=50)

# 执行圆形轨迹
smooth_circle(center=[220, 0, 180], radius=40, duration=8.0)

# 控制夹爪
control_electric_gripper(action='move', position=200)
```

---

## 🏗️ 系统架构

```
┌─────────────────────────────────────────────────────────────────┐
│                      用户交互层                                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ 自然语言输入  │  │  Python API  │  │  Jupyter     │          │
│  │ (文本/语音)   │  │   编程控制   │  │  Notebook    │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└─────────────────────────┬───────────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────────┐
│                      AI 处理层                                    │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │            Gemini 2.5 Flash Vision AI                    │  │
│  │  - 自然语言理解  - 任务规划  - 视觉识别  - 工具调用    │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────┬───────────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────────┐
│                      视觉处理层                                   │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ RealSense    │  │  物体检测    │  │  3D 定位     │          │
│  │ D435I 相机   │  │  Gemini Pro  │  │  坐标转换    │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└─────────────────────────┬───────────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────────┐
│                    运动控制层                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ 逆运动学     │  │  轨迹规划    │  │  速度控制    │          │
│  │ IK Solver    │  │  Path Plan   │  │  Sync        │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└─────────────────────────┬───────────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────────┐
│                    硬件接口层                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ PAROL6       │  │  电动/气动    │  │  串口通信    │          │
│  │ 机械臂       │  │  夹爪        │  │  100Hz       │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📦 快速开始

### 前置要求

**硬件：**
- PAROL6 六轴机械臂
- Intel RealSense D435I 深度相机（可选，视觉功能需要）
- 电动或气动夹爪
- USB 连接线

**软件：**
- Python 3.8+
- Windows 10/11 或 Ubuntu 20.04+
- 8GB+ RAM

### 安装步骤

#### 1. 克隆项目
```bash
git clone https://github.com/your-username/PAROL6-Python-API-Gemini-Vision-Public.git
cd PAROL6-Python-API-Gemini-Vision-Public
```

#### 2. 安装依赖

**核心依赖（必需）：**
```bash
pip3 install roboticstoolbox-python==1.0.3
pip3 install numpy==1.23.4
pip3 install scipy==1.11.4
pip3 install spatialmath
pip3 install pyserial
pip3 install oclock
pip3 install keyboard
```

**视觉依赖（可选）：**
```bash
pip3 install opencv-python
pip3 install pyrealsense2
pip3 install Pillow
```

**AI 控制依赖（可选）：**
```bash
pip3 install google-genai
pip3 install python-dotenv
pip3 install pyaudio
```

#### 3. 配置串口

创建 `Headless/com_port.txt` 文件：
```bash
# Windows
echo COM3 > Headless/com_port.txt

# Linux
echo /dev/ttyUSB0 > Headless/com_port.txt
```

#### 4. 配置 Gemini API（可选）

创建 `.env` 文件：
```bash
GEMINI_API_KEY=your_api_key_here
```

获取 API 密钥：https://makersuite.google.com/app/apikey

---

## 🚀 使用方法

### 方式 1：自然语言控制（推荐）

#### 启动服务器
```bash
# 终端 1 - 启动机器人控制服务器
cd Headless
python headless_commander.py
```

#### 启动 AI 控制器
```bash
# 终端 2 - 启动 Gemini 控制器
cd Headless/Gemini

# 文本模式（逐步控制）
python gemini.py --mode text

# 自主模式（自动分解任务）
python gemini.py --mode autonomous

# 语音输入模式
python gemini.py --mode text --input audio
```

#### 示例命令
```
你: 机器人回零
你: 向上移动 50mm
你: 抓取红色的方块
你: 把它放到左边
你: 清理桌面上所有的红色物体
```

---

### 方式 2：Python API 编程

#### 创建控制脚本
```python
#!/usr/bin/env python3
from Headless.robot_api import *
import time

# 1. 机器人回零
print("机器人回零...")
home_robot(wait_for_ack=True)

# 2. 移动到指定关节角度
print("移动关节...")
move_robot_joints(
    joint_angles=[90, -45, 90, 0, 45, 180],
    duration=5.0,
    wait_for_ack=True
)

# 3. 移动到笛卡尔坐标
print("移动到目标位置...")
move_robot_pose(
    pose=[250, 0, 200, 180, 0, 0],  # [x, y, z, rx, ry, rz]
    speed_percentage=50,
    wait_for_ack=True
)

# 4. 执行圆形轨迹
print("画圆...")
smooth_circle(
    center=[220, 0, 180],
    radius=40,
    plane='XY',
    duration=8.0,
    wait_for_ack=True
)

# 5. 控制夹爪
print("控制夹爪...")
control_electric_gripper(action='open')
time.sleep(1)
control_electric_gripper(action='move', position=200, speed=150)

# 6. 查询状态
pose = get_robot_pose()
print(f"当前位置: {pose}")

angles = get_robot_joint_angles()
print(f"关节角度: {angles}")

print("完成！")
```

运行：
```bash
python your_script.py
```

---

### 方式 3：Jupyter Notebook 交互式测试

```bash
# 安装 Jupyter
pip install jupyter

# 启动 Notebook
jupyter notebook

# 打开文件
# → 01_PAROL6完整功能测试教程.ipynb
```

在 Notebook 中逐个单元格执行测试。

---

## 📚 完整文档

| 文档 | 说明 | 链接 |
|------|------|------|
| **中文使用说明** | 完整的功能说明和 API 文档 | [中文使用说明.md](中文使用说明.md) |
| **分步测试教程** | 从零开始的详细测试步骤 | [分步测试教程.md](分步测试教程.md) |
| **Jupyter Notebook** | 交互式测试教程 | [01_PAROL6完整功能测试教程.ipynb](01_PAROL6完整功能测试教程.ipynb) |
| **API 详细文档** | 所有 API 函数的详细说明 | [API-Specific-README.md](API-Specific-README.md) |
| **英文 README** | 项目概述（英文）| [README.md](README.md) |

---

## 🎯 功能特性详解

### 运动控制

#### 基础运动
- **关节运动** - 控制 6 个关节到指定角度
- **位置运动** - 移动到笛卡尔坐标位置
- **直线运动** - 保证直线路径的运动
- **点动控制** - 单轴微调控制

#### 高级运动
- **圆形轨迹** - 在指定平面画圆
- **圆弧运动** - 精确圆弧路径
- **样条曲线** - 通过多点的平滑曲线
- **螺旋运动** - 螺旋式上升/下降
- **混合运动** - 多段运动平滑衔接

#### 运动特性
- ✅ 速度控制（百分比或时长）
- ✅ 轨迹插值（线性/样条）
- ✅ 速度同步（多关节协调）
- ✅ 碰撞检测（工作空间限制）

### 夹爪控制

#### 电动夹爪
- 位置控制 (0-255)
- 速度控制 (0-255)
- 力控制 (100-1000 mA)
- 物体检测
- 状态反馈

#### 气动夹爪
- 开/关控制
- 数字输出端口控制
- IO 状态监控

### 视觉系统

#### 相机功能
- 彩色图像获取 (640×480 @ 30fps)
- 深度图像获取
- 相机标定
- 手眼标定（ChArUco 板）

#### 视觉处理
- 物体检测（Gemini Vision）
- 3D 位置计算
- 深度滤波和验证
- 可达性分析

#### 应用
- 视觉引导抓取
- 物体识别和分类
- 障碍物检测
- 场景理解

### AI 控制

#### Gemini 集成
- 自然语言理解
- 任务规划和分解
- 视觉识别
- 工具函数调用

#### 控制模式
- **文本模式** - 逐步交互
- **自主模式** - 自动执行复杂任务
- **语音模式** - 语音输入（按住空格键）

#### 功能
- 物体操作（抓取、放置、移动）
- 场景扫描
- 任务执行
- 错误恢复

---

## 🔧 配置说明

### 网络配置

如果客户端和服务器在不同电脑，修改 `Headless/robot_api.py`：

```python
SERVER_IP = "192.168.1.100"  # 服务器 IP
SERVER_PORT = 5001           # 默认端口
```

### 相机配置

修改 `Headless/Vision/vision_controller.py`：

```python
CAMERA_WIDTH = 640
CAMERA_HEIGHT = 480
CAMERA_FPS = 30
```

### 运动参数

修改 `Headless/Gemini/robot_vision_tools.py`：

```python
APPROACH_HEIGHT = 50      # 接近高度 (mm)
GRASP_HEIGHT = 100        # 抓取后抬升高度 (mm)
SPEED_APPROACH = 40       # 接近速度 (%)
SPEED_GRASP = 20          # 抓取速度 (%)
```

---

## 🛡️ 安全注意事项

### 硬件安全
- ⚠️ **急停按钮** - 确保随时可触及
- ⚠️ **工作空间** - 保持无障碍物
- ⚠️ **固定** - 确保机器人底座固定牢固
- ⚠️ **电源** - 使用稳定的电源供应

### 软件安全
- ✅ 自动关节限位检查
- ✅ 运动前验证逆运动学
- ✅ 速度同步防止超速
- ✅ 急停触发立即停止
- ✅ 命令队列自动清空

### 操作规范
1. 首次使用先在低速度测试
2. 确认工作空间范围
3. 观察首次运动路径
4. 保持监控运行状态
5. 异常情况立即按急停

---

## 📊 性能指标

| 指标 | 数值 | 说明 |
|------|------|------|
| **控制频率** | 100 Hz | 实时控制循环 |
| **定位精度** | ±1 mm | 笛卡尔空间 |
| **重复精度** | ±0.5 mm | 相同位置重复 |
| **关节精度** | ±0.1° | 关节空间 |
| **最大负载** | 1.2 kg | 包括夹爪 |
| **工作半径** | 100-400 mm | 从基座中心 |
| **响应延迟** | <50 ms | 局域网环境 |
| **视觉帧率** | 30 fps | RealSense D435I |
| **AI 响应** | 1-3 秒 | Gemini API |

---

## 🐛 故障排除

### 常见问题

#### Q1: 串口连接失败
**现象**: `Serial port not found`

**解决**:
- Windows: 检查设备管理器中的 COM 端口
- Linux: 检查 `/dev/ttyUSB*` 或 `/dev/ttyACM*`
- 更新 `Headless/com_port.txt`
- Linux 权限: `sudo usermod -a -G dialout $USER`

#### Q2: UDP 通信失败
**现象**: 命令无响应

**解决**:
- 确认服务器是否启动
- 检查 IP 地址和端口
- 检查防火墙设置
- 测试: `ping 服务器IP`

#### Q3: 运动学求解失败
**现象**: `IK solution not found`

**解决**:
- 目标位置可能超出工作空间
- 检查姿态角度是否合理
- 分步移动到目标
- 使用 `get_robot_pose()` 查看可达范围

#### Q4: 相机无法启动
**现象**: `RealSense camera not found`

**解决**:
- 使用 USB 3.0 端口（蓝色）
- 更新 RealSense 驱动
- 运行 `realsense-viewer` 测试
- 检查相机权限

#### Q5: Gemini API 错误
**现象**: `API key invalid`

**解决**:
- 检查 `.env` 文件中的密钥
- 确认密钥未过期
- 检查网络连接
- 验证 API 配额

更多问题？查看 [分步测试教程.md](分步测试教程.md) 的故障排除章节。

---

## 📖 API 快速参考

### 运动控制
```python
# 基础运动
home_robot()                                    # 回零
move_robot_joints([...], duration=5)           # 关节运动
move_robot_pose([x,y,z,rx,ry,rz], speed=50)  # 位置运动
move_robot_cartesian([...], duration=4)       # 直线运动

# 高级运动
smooth_circle(center, radius, duration)        # 圆形
smooth_arc_parametric(end, radius, angle)     # 圆弧
smooth_spline(waypoints, duration)            # 样条
smooth_helix(center, radius, pitch, height)   # 螺旋
```

### 夹爪控制
```python
# 电动夹爪
control_electric_gripper(action='open')
control_electric_gripper(action='move', position=200)
get_electric_gripper_status()

# 气动夹爪
control_pneumatic_gripper(action='open', port=1)
control_pneumatic_gripper(action='close', port=1)
```

### 状态查询
```python
get_robot_pose()           # 位置姿态
get_robot_joint_angles()   # 关节角度
get_robot_io()             # IO 状态
is_robot_stopped()         # 运动状态
get_robot_status()         # 完整状态
```

---

## 🤝 贡献指南

我们欢迎所有形式的贡献！

### 如何贡献

1. **Fork 项目**
2. **创建分支** (`git checkout -b feature/AmazingFeature`)
3. **提交更改** (`git commit -m 'Add some AmazingFeature'`)
4. **推送分支** (`git push origin feature/AmazingFeature`)
5. **创建 Pull Request**

### 贡献类型

- 🐛 Bug 修复
- ✨ 新功能
- 📝 文档改进
- 🎨 代码优化
- 🌐 翻译
- 💡 建议和反馈

### 代码规范

- 遵循 PEP 8 风格指南
- 添加清晰的注释
- 编写单元测试
- 更新相关文档

---

## 📜 许可证

本项目采用 MIT 许可证。详见 [LICENSE](LICENSE) 文件。

---

## 🙏 致谢

### 核心依赖
- **PAROL6** - 机械臂硬件和基础软件
- **Google Gemini** - AI 视觉和自然语言处理
- **Intel RealSense** - 深度相机和 SDK
- **Python Robotics Toolbox** - 运动学计算

### 社区
- PAROL6 Discord 社区
- 所有贡献者和测试者
- 开源软件社区

---

## 📞 联系方式

### 获取帮助

1. **查看文档**
   - [中文使用说明](中文使用说明.md)
   - [分步测试教程](分步测试教程.md)
   - [API 文档](API-Specific-README.md)

2. **问题反馈**
   - [GitHub Issues](https://github.com/your-username/PAROL6-Python-API-Gemini-Vision-Public/issues)
   - [PAROL6 Discord](https://discord.com/invite/prjUvjmGpZ)

3. **相关链接**
   - [PAROL6 官方仓库](https://github.com/PCrnjak/PAROL-commander-software)
   - [Gemini API 文档](https://ai.google.dev/gemini-api/docs)
   - [RealSense SDK](https://github.com/IntelRealSense/librealsense)

---

## 🌟 Star History

如果这个项目对你有帮助，请给我们一个 ⭐️！

---

## 📅 更新日志

### v1.0.0 (2024-11-XX)
- ✨ 初始版本发布
- 🤖 自然语言控制
- 👁️ 视觉引导操作
- 📚 完整中文文档
- 📓 Jupyter Notebook 教程

---

<div align="center">

**让机械臂变得更智能、更易用！** 🤖✨

Made with ❤️ by the PAROL6 Community

[⬆ 返回顶部](#parol6-python-api---gemini-vision-智能控制系统)

</div>
