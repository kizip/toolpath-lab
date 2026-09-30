# 贡献指南

感谢您对Toolpath Lab的关注！我们欢迎任何形式的贡献。

## 如何贡献

### 报告问题

如果您发现了bug或有功能建议，请在GitHub Issues中创建一个新的issue，并包含以下信息：

1. **问题描述**：清晰简洁地描述问题
2. **复现步骤**：列出复现问题的步骤
3. **期望行为**：描述您期望的行为
4. **实际行为**：描述实际发生的行为
5. **环境信息**：
   - Python版本
   - 操作系统
   - 相关依赖版本

### 提交代码

1. **Fork项目**
   ```bash
   # 在GitHub上fork项目
   git clone https://github.com/your-username/toolpath-lab.git
   cd toolpath-lab
   ```

2. **创建特性分支**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **设置开发环境**
   ```bash
   pip install -r requirements.txt
   pip install pytest pytest-cov
   ```

4. **进行修改**
   - 遵循项目的代码风格
   - 添加必要的注释
   - 更新相关文档

5. **编写测试**
   ```bash
   # 运行测试
   pytest tests/

   # 查看测试覆盖率
   pytest tests/ --cov=toolpath_lab --cov-report=html
   ```

6. **提交更改**
   ```bash
   git add .
   git commit -m "feat: 添加新功能描述"
   git push origin feature/your-feature-name
   ```

7. **创建Pull Request**
   - 在GitHub上创建Pull Request
   - 填写PR描述，说明修改内容
   - 等待代码审查

## 代码规范

### Python代码风格

- 遵循PEP 8规范
- 使用4个空格缩进
- 行长度限制在88个字符（使用Black格式化）
- 使用类型注解
- 编写清晰的docstring

### 提交信息规范

使用[Conventional Commits](https://www.conventionalcommits.org/zh-hans/)规范：

```
<type>(<scope>): <subject>

<body>

<footer>
```

类型（type）：
- `feat`: 新功能
- `fix`: 修复bug
- `docs`: 文档更新
- `style`: 代码格式调整
- `refactor`: 代码重构
- `test`: 测试相关
- `chore`: 构建/工具相关

示例：
```
feat(planning): 添加螺旋刀路算法

- 实现SpiralToolpath类
- 添加参数验证
- 添加单元测试

Closes #123
```

## 开发流程

### 添加新的刀路算法

1. 在 `toolpath_lab/core/` 目录下创建新文件
2. 继承 `ToolpathBase` 基类
3. 实现 `generate_toolpath()` 方法
4. 添加参数验证
5. 编写单元测试
6. 更新文档

示例：
```python
from toolpath_lab.core.base import ToolpathBase, ToolPosition, ToolpathConfig

class MyNewToolpath(ToolpathBase):
    """新的刀路算法"""
    
    def __init__(self, my_param: float, **kwargs):
        super().__init__(**kwargs)
        self.my_param = my_param
        self._validate_params()
    
    def _validate_params(self):
        """参数验证"""
        if self.my_param <= 0:
            raise ValueError("参数必须大于0")
    
    def generate_toolpath(self) -> ToolpathConfig:
        """生成刀路"""
        positions = []
        # 实现刀路生成逻辑
        return ToolpathConfig(positions=positions)
```

### 添加新的刀具类型

1. 在 `toolpath_lab/models/tool.py` 中添加新的枚举值
2. 在 `ToolBuilder` 中添加创建方法
3. 添加预设刀具（可选）
4. 更新文档

### 添加新的曲面类型

1. 在 `toolpath_lab/models/surface.py` 中添加新的曲面类
2. 继承 `Surface` 基类
3. 实现必要的方法
4. 在 `SurfaceFactory` 中注册
5. 更新文档

## 测试要求

- 所有新功能必须包含单元测试
- 测试覆盖率应达到80%以上
- 测试文件放在 `tests/` 目录下
- 测试文件命名：`test_<module>.py`

运行测试：
```bash
# 运行所有测试
pytest tests/

# 运行特定测试
pytest tests/test_spiral.py -v

# 查看覆盖率
pytest tests/ --cov=toolpath_lab --cov-report=term-missing
```

## 文档要求

- 所有公开API必须有docstring
- 使用中文注释（因为目标用户是中文用户）
- 更新README.md（如果添加了新功能）
- 更新CHANGELOG.md

## 问题反馈

如有任何问题，请通过以下方式联系：

- GitHub Issues: https://github.com/large-su/toolpath-lab/issues
- 邮箱: [your-email@example.com]

感谢您的贡献！
