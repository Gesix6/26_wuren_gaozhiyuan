# CMake任务 - 项目说明文档

## 项目概述

本项目是一个基于 **CMake** 构建系统的 C++ 模块化工程，展示了模块化代码组织、静态库链接和 CMake 依赖管理的典型用法。项目包含卡尔曼滤波器、数学工具函数库以及多个功能模块。

## 项目框架结构

CMake作业/                              # 项目根目录
├── CMakeLists.txt                      # 根目录 CMake 配置（入口）
├── main.cpp                            # 主程序入口文件
├── common/                             # 公共库目录（底层工具）
│   ├── CMakeLists.txt                  # 公共库 CMake 配置
│   ├── kalman/                         # 卡尔曼滤波器模块
│   │   ├── CMakeLists.txt
│   │   └── include/
│   │       └── KalmanFilterX.hpp       # 基于 cv::Matx 的轻量级卡尔曼滤波器
│   └── math/                           # 数学工具函数库
│       ├── include/
│       │   └── Math.h                  # 数学工具头文件
│       └── src/
│           └── Math.cpp                # 数学工具实现
└── modules/                            # 功能模块目录（业务逻辑）
    ├── CMakeLists.txt
    ├── M1/                             # 功能模块 M1
    │   ├── CMakeLists.txt
    │   ├── include/M1.h
    │   └── src/M1.cpp
    ├── M2/                             # 功能模块 M2
    │   ├── include/M2.h
    │   └── src/M2.cpp
    ├── A1/                             # 基础模块 A1
    │   ├── include/A1.h
    │   └── src/
    │       ├── A11.cpp
    │       ├── A12.cpp
    │       └── A13.cpp
    └── A2/                             # 基础模块 A2
        ├── include/A2.h
        └── src/A2.cpp

## 模块依赖关系

### 依赖关系图

main.cpp (主程序入口)
    │
    ├── M1.h ──────────> A1.h
    │
    ├── M2.h ─┬────────> KalmanFilterX.hpp (卡尔曼滤波器)
    │         ├────────> A1.h
    │         └────────> A2.h
    │
    └── Math.h ────────> OpenCV (cv::Matx, cv::Point2f)

### 层级结构说明

| 层级 | 模块名称 | 依赖项 | 职责描述 |
|------|---------|--------|----------|
| 底层 | A1 | 无 | 基础打印模块，提供 print1/print2/print3 接口 |
| 底层 | A2 | 无 | 基础队列模块，基于 std::deque 实现 push/pop |
| 底层 | KalmanFilterX | OpenCV | 轻量级卡尔曼滤波器，使用 cv::Matx |
| 底层 | Math | OpenCV | 数学工具库，含距离计算、PnP姿态解算等 |
| 中间层 | M1 | A1 | 功能模块，封装 A1 打印功能 |
| 中间层 | M2 | KalmanFilterX, A1, A2 | 功能模块，集成滤波和队列 |
| 顶层 | main | M1, M2, Math | 主程序，整合所有模块 |

### CMake 依赖配置

root/CMakeLists.txt
    ├── common/CMakeLists.txt
    │   └── common/kalman/CMakeLists.txt (OpenCV)
    └── modules/CMakeLists.txt
        └── modules/M1/CMakeLists.txt (A1)

## 主要模块功能说明

### 1. 公共库 (common/)

**kalman/KalmanFilterX.hpp**
- 基于 OpenCV cv::Matx 的模板化卡尔曼滤波器
- 支持可控和不可控两种模式（通过模板参数 ControlDim）
- 轻量级实现，避免运行时堆内存分配

**math/Math.h**
- 圆周率 PI、自然对数 e、重力加速度 g 常量定义
- 距离计算函数 getDistances()
- PnP 姿态解算结构体 ResultPnP
- 陀螺仪数据结构体 GyroData

### 2. 功能模块 (modules/)

**A1 模块**
- 基础打印服务，提供三个打印接口
- 被 M1 和 M2 模块依赖

**A2 模块**
- 队列数据结构，基于 std::deque
- 提供 push、pop、size 操作
- 被 M2 模块依赖

**M1 模块**
- 封装 A1 模块的功能
- 提供统一的打印接口

**M2 模块**
- 集成卡尔曼滤波器和基础模块
- 包含滤波器实例和队列实例

## 构建与运行

### 环境要求
- CMake 3.x 或更高版本
- OpenCV 2.x 或更高版本
- 支持 C++11 的编译器（如 GCC、Clang）

### 构建命令
mkdir build && cd build
cmake ..
make

### 运行程序
./<可执行文件名>

## 外部依赖

| 依赖库 | 最低版本 | 用途 |
|--------|----------|------|
| OpenCV | 2.x | 提供 cv::Matx、cv::Point2f 等数据结构 |

## 设计特点

1. 模块化架构：代码按功能划分为独立模块，降低耦合度
2. 依赖清晰：底层模块不依赖上层模块，单向依赖
3. 头文件分离：头文件和实现文件分离，便于接口管理
4. CMake 自动化：自动扫描子目录并构建依赖关系