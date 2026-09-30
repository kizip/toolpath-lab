# ToolpathLab - GitHub项目分析

## 一、项目概述

**项目名称**: ToolpathLab  
**项目描述**: 刀路规划基础工具 - 给定刀具和加工区域，生成光栅刀路，3D显示工件、刀路和刀具，并按编程进给速率回放整个过程。

**GitHub地址**: https://github.com/large-su/toolpath-lab

---

## 二、技术架构

### 2.1 技术栈

| 层级 | 技术 | 说明 |
|------|------|------|
| **后端** | Python 3.10+ | 纯Python实现 |
| **依赖** | numpy | 唯一依赖 |
| **前端** | 原生ES模块 | 无构建步骤 |
| **3D渲染** | three.js | vendored版本 |
| **桌面窗口** | Electron | 提供桌面窗口 |

### 2.2 项目结构

```
toolpath-lab/
├── toolpath_lab/
│   ├── core/              # 领域模型
│   │   ├── parameter      # 参数规格
│   │   ├── tool           # 刀具模型
│   │   ├── region         # 加工区域
│   │   └── move/toolpath  # 刀路模型
│   ├── planning/          # 策略模块
│   │   ├── Planner        # 规划器基类
│   │   ├── raster         # 光栅刀路
│   │   └── geometry       # 平面几何
│   ├── simulation/        # 时间参数化
│   ├── export/            # G代码导出
│   ├── server/            # HTTP API服务
│   └── web/               # 前端界面
├── electron/              # 桌面shell
├── examples/              # 使用示例
└── tests/                 # 测试代码
```

---

## 三、核心功能

### 3.1 刀具 (Tool)

- **类型**: 平底铣刀 (Flat End Mill)
- **参数**: 直径 (diameter) 和 长度 (length)
- **特性**: 刀具在加工平面上的投影半径决定刀路偏移量

### 3.2 加工区域 (Region)

| 区域类型 | 参数 | 说明 |
|----------|------|------|
| **正方形** | side | 边长，以原点为中心 |
| **圆形** | diameter | 直径，以原点为中心 |

### 3.3 刀路策略 (Toolpaths)

| 模式 | 说明 |
|------|------|
| **zigzag** | 往复加工 - 每隔一道反向运行，相邻道连接 |
| **one-way** | 单向加工 - 所有道同向，道间抬刀到安全高度 |

### 3.4 参数配置

| 参数 | 说明 |
|------|------|
| stepover | 步距 |
| direction | 加工方向 |
| feed_rate | 进给速率 |
| safe_height | 安全高度 (5mm) |
| rapid_feed | 快速移动速率 (5000mm/min) |

### 3.5 3D可视化

- 工件显示
- 区域轮廓
- 刀路显示（切削/连接/快速 颜色编码）
- 刀具实体
- 实时阴影
- 网格地面

### 3.6 回放功能

- 播放/暂停（空格键）
- 时间轴拖动
- 显示切割长度和预计加工时间

### 3.7 导出功能

- **格式**: G代码 (NC程序)
- **标准**: G21(公制) / G90(绝对坐标) / G17(XY平面)
- **指令**: G0(快速移动) / G1(切削移动) / F(进给)

---

## 四、使用方式

### 4.1 快速开始 (Windows)

```bash
# 双击 start.bat
# 自动安装依赖并启动
```

### 4.2 手动安装

```bash
git clone https://github.com/large-su/toolpath-lab.git
cd toolpath-lab

pip install -r requirements.txt
npm install

# 启动桌面窗口
npm start

# 或仅启动后端 + 浏览器
python -m toolpath_lab
```

### 4.3 作为库使用

```python
from toolpath_lab.core.region import build_region
from toolpath_lab.core.tool import Tool, ToolKind
from toolpath_lab.planning import run_plan

outcome = run_plan(
    planner_id="raster",
    tool=Tool(ToolKind.FLAT, diameter_mm=6.0, length_mm=30.0),
    region=build_region("square", {"side_mm": 80.0}),
    parameters={"mode": "zigzag", "stepover_mm": 6.0, "feed_mm_per_min": 800.0},
)
print(outcome.toolpath.statistics())
```

### 4.4 HTTP API

| 端点 | 功能 |
|------|------|
| `GET /api/health` | 健康检查 |
| `GET /api/catalog` | 获取能力目录 |
| `POST /api/plan` | 规划刀路 |
| `POST /api/export/gcode` | 导出G代码 |

---

## 五、扩展方向

### 5.1 新增刀路策略

1. 继承 `Planner` 基类
2. 声明参数
3. 实现 `plan()` 方法
4. 注册到系统

### 5.2 新增区域形状

1. 实现 `boundary()` 方法
2. 返回逆时针多边形
3. 裁剪和3D视图自动适配

### 5.3 新增导出格式

1. 在 `export/` 下添加纯函数
2. 在HTTP路由中添加分支

---

## 六、配置常量

| 常量 | 值 | 位置 |
|------|-----|------|
| 安全高度 | 5mm | `toolpath_lab/planning/base.py` |
| 快速移动速率 | 5000mm/min | `toolpath_lab/planning/base.py` |
| 边界处理 | 按刀具投影半径内缩轮廓 | `toolpath_lab/planning/raster.py` |
| 道采样 | 两个端点 | `toolpath_lab/planning/raster.py` |

---

## 七、与我的项目的对比

| 功能 | GitHub项目 | 我的项目 |
|------|------------|----------|
| **刀路类型** | 光栅(光栅) | 螺旋/往复/环切 |
| **区域类型** | 正方形/圆形 | 矩形/圆形 |
| **刀具类型** | 平底铣刀 | 平底/球头/圆鼻 |
| **可视化** | 3D (three.js) | 2D/3D (matplotlib) |
| **导出** | G代码 | CSV/G代码/JSON |
| **界面** | Electron桌面 | 命令行 |
| **依赖** | numpy + electron | numpy + matplotlib |

---

## 八、建议改进

基于GitHub项目的特点，我的项目可以借鉴：

1. **添加光栅刀路策略** - 实现zigzag和one-way模式
2. **添加3D可视化** - 使用three.js或pyvista
3. **添加HTTP API** - 提供REST接口
4. **添加参数面板** - 自动生成参数界面
5. **优化边界处理** - 按刀具半径内缩

---

**分析日期**: 2026年9月29日
**分析人**: 阳佳琪
