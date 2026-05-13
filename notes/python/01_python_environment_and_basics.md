# Python 学习笔记（一）20260513
# Python 环境、基础运行与初步理解
目前进度：python入门与实战第二章结束
## 当前阶段
当前主要目标：
- 理解 Python 的基本运行方式
- 熟悉 VSCode + Python 开发环境
- 初步接触工程化开发流程
- 学会使用终端运行 Python 文件
- 理解 Python 生态与常见工具

## 1. Python 解释器（REPL）
在 PowerShell 中输入：

```powershell
python
```

进入 Python 交互式解释器：

```python
>>>
```

REPL 含义：
- Read
- Eval
- Print
- Loop

特点：
- 输入一句代码立即执行
- 适合学习和测试小段代码

示例：

```python
print("Hello")
```

退出方式：

```python
exit()
```

## 2. 运行 Python 文件
创建文件：

```text
hello_world.py
```

内容：

```python
print("Hello Python world!")
```

在终端运行：

```powershell
py hello_world.py
```

输出：

```text
Hello Python world!
```

## 3. PowerShell 与当前目录
直接输入：

```powershell
hello_world.py
```

会报错。

原因：
PowerShell 默认不会执行当前目录下的脚本。

正确方式：

```powershell
.\hello_world.py
```

或者更推荐：

```powershell
py hello_world.py
```

## 4. VSCode 与 Python 环境
当前 VSCode 使用的是：

```text
Python 3.13
```

VSCode 会提示：

```text
是否创建虚拟环境
```

当前阶段：
- 可以先不创建虚拟环境
- 重点先放在 Python 基础学习

## 5. 虚拟环境（Virtual Environment）
作用：
为不同项目创建独立 Python 环境。

避免：
- 包版本冲突
- Python 版本冲突
- 项目依赖互相影响

当前阶段暂不重点学习。

后续会接触：
- conda
- venv
- pip
- requirements.txt

## 6. Anaconda 与 conda
当前电脑安装了：

```text
Anaconda
```

Anaconda 包含：
- Python
- conda
- Jupyter
- NumPy
- pandas
- matplotlib 等科学计算库

其中最重要的是：

```text
conda
```

作用：
- 环境管理
- 包管理

常用命令：

```powershell
conda env list
conda create -n test python=3.10
conda activate test
conda deactivate
```

## 7. Anaconda 与 Miniconda
当前阶段可以先使用 Anaconda。

后续工程开发中，很多人会转向更轻量的：

```text
Miniconda
```

原因：
- 更轻量
- 更适合工程环境
- 更适合 ROS2 / AI 项目

## 8. Python 常见科学计算库

### NumPy
用于：
- 数组
- 矩阵
- 科学计算

### pandas
用于：
- 表格数据处理
- CSV
- parquet
- 数据分析

### matplotlib
用于：
- 绘图
- 曲线
- 数据可视化

### Jupyter Notebook
交互式代码笔记本。

文件格式：

```text
.ipynb
```

### PyTorch
深度学习核心库。

后续具身智能 / VLA / Robotics 会大量使用。

## 9. Python 字符串方法

### removeprefix()
删除字符串前缀。

示例：

```python
filename = "test_file.txt"
print(filename.removeprefix("test_"))
```

输出：

```text
file.txt
```

### removesuffix()
删除字符串后缀。

示例：

```python
filename = "robot.csv"
print(filename.removesuffix(".csv"))
```

输出：

```text
robot
```

## 10. Tab 与缩进
Python 使用缩进表示代码层级。

推荐：

```text
4 个空格缩进
```

不推荐：

```text
Tab 与空格混用
```

否则可能出现：

```text
TabError
```

## 11. 常见转义字符

### 换行

```python
\n
```

### 制表符（Tab）

```python
\t
```

示例：

```python
print("Name\tAge")
```

## 12. The Zen of Python
查看方式：

```python
import this
```

核心思想：
- Beautiful is better than ugly
- Simple is better than complex
- Readability counts

Python 强调：
- 可读性
- 简洁
- 工程实用性

## 13. 当前学习理解
当前阶段最重要的不是：
- 背语法
- 刷大量课程

而是：
- 能运行代码
- 能看懂报错
- 能使用终端
- 能逐渐理解工程环境

后续计划：
- Python 基础语法
- list / dict / function
- 文件读写
- NumPy
- Git
- ROS2 Python 节点
- 数据处理
- PyTorch