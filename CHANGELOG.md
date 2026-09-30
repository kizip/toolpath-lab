# 更新日志

本文档记录了Toolpath Lab的所有重要更改。

格式基于[Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/)，
版本号遵循[语义化版本控制](https://semver.org/lang/zh-CN/)。

## [1.0.0] - 2026-09-29

### 新增 (Added)

#### 刀路规划算法
- 螺旋刀路规划算法 (`SpiralToolpath`)
- 阿基米德螺旋刀路算法 (`ArchimedeanSpiralToolpath`)
- 往复刀路规划算法 (`ZigzagToolpath`)
- 自适应往复刀路算法 (`AdaptiveZigzagToolpath`)
- 环切刀路规划算法 (`ContourToolpath`)
- 螺旋环切刀路算法 (`SpiralContourToolpath`)

#### 刀具建模
- 刀具类型枚举 (`ToolType`)
  - 平底铣刀 (`FLAT_END_MILL`)
  - 球头铣刀 (`BALL_END_MILL`)
  - 圆鼻铣刀 (`BULL_NOSE_MILL`)
  - 钻头 (`DRILL`)
  - 倒角铣刀 (`CHAMFER_MILL`)
- 刀具模型类 (`Tool`)
- 刀具构建器 (`ToolBuilder`)
- 常用刀具预设 (`TOOL_PRESETS`)

#### 曲面建模
- 平面 (`PlaneSurface`)
- 圆柱面 (`CylinderSurface`)
- 球面 (`SphereSurface`)
- 曲面工厂 (`SurfaceFactory`)

#### 工具函数
- 几何计算工具 (`geometry.py`)
  - 2D/3D距离计算
  - 中点计算
  - 向量运算
  - 坐标变换
- 文件读写工具 (`io_utils.py`)
  - CSV导出/导入
  - G代码导出
  - JSON导出/导入

#### 可视化
- 2D刀路可视化
- 3D刀路可视化
- 多刀路对比显示

#### 测试
- 螺旋刀路单元测试

### 变更 (Changed)
- 项目结构按照参考项目重新组织
- 所有模块移至 `toolpath_lab` 包下

### 已知问题 (Known Issues)
- 单元测试仅覆盖螺旋刀路算法
- 可视化依赖matplotlib

## [0.1.0] - 2026-09-28

### 新增 (Added)
- 项目初始化
- 基础框架搭建
