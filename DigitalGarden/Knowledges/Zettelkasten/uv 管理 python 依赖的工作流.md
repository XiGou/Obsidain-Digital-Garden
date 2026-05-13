


**uv** 是由 Astral（Ruff 的开发团队）开发的一个极速 Python 包管理器。随着版本的迭代，`uv` 目前提供两种主要的依赖管理工作流：

1. **现代项目管理工作流（推荐）**：类似 Rust 的 Cargo 或 Python 的 Poetry/PDM，基于 `pyproject.toml` 和 `uv.lock`。
2. **Pip 兼容工作流**：作为传统 `pip` 和 `pip-tools` 的极速平替。

下面为您详细介绍这两种工作流的基本操作：

---

### 一、 现代项目管理工作流（官方强烈推荐）

这是目前 `uv` 最核心且推荐的使用方式。它会自动帮你管理虚拟环境、依赖和锁文件（Lockfile），全程不需要你手动激活虚拟环境。

#### 1. 初始化项目
进入一个空目录或新目录，执行初始化：
```bash
uv init my_project
cd my_project
```
这会生成几个关键文件：
* `pyproject.toml`：项目配置文件。
* `.python-version`：记录当前使用的 Python 版本。
* `hello.py`：一个简单的示例代码。

#### 2. 添加依赖
当你需要安装第三方包（如 `requests`）时，直接使用 `add` 命令：
```bash
uv add requests
```
**`uv` 在后台自动做了这几件事：**
1. 发现没有虚拟环境，自动在项目下创建一个 `.venv` 文件夹。
2. 下载并安装 `requests` 及其依赖。
3. 更新 `pyproject.toml`，将 `requests` 写入 `dependencies`。
4. 生成 `uv.lock` 文件，锁定确切的版本号以保证环境可复现。

#### 3. 添加开发依赖
如果是测试工具或代码格式化工具（仅在开发时需要），可以添加为开发依赖：
```bash
uv add --dev pytest ruff
```

#### 4. 运行代码
**无需手动激活虚拟环境**，直接使用 `uv run` 即可在当前项目的隔离环境中执行代码或命令：
```bash
# 运行 Python 脚本
uv run hello.py

# 运行安装的第三方命令行工具
uv run pytest
```

#### 5. 同步与还原依赖（团队协作）
如果你的同事 clone 了这个项目，他们只需要拿到 `pyproject.toml` 和 `uv.lock` 文件，然后运行：
```bash
uv sync
```
`uv` 会自动创建虚拟环境，并根据 `uv.lock` 精确安装所有依赖。

#### 6. 移除依赖
如果你不再需要某个包：
```bash
uv remove requests
```
这会自动卸载包并更新 `pyproject.toml` 和 `uv.lock`。

---

### 二、 Pip 兼容工作流（传统方式的极速平替）

如果你还在使用传统的 `requirements.txt`，或者仅仅想把 `uv` 当作一个“更快的 pip”来用，可以使用这种工作流。

#### 1. 创建虚拟环境
```bash
uv venv
```

#### 2. 激活虚拟环境
与传统方法一样，你需要手动激活：
* **Linux / macOS**: `source .venv/bin/activate`
* **Windows**: `.venv\Scripts\activate`

#### 3. 安装与管理依赖
将平时的 `pip` 替换为 `uv pip`：
```bash
# 单独安装包
uv pip install requests

# 从 requirements.txt 安装
uv pip install -r requirements.txt
```

#### 4. 依赖锁定（类似 pip-tools）
如果你想做依赖锁定，可以编写一个 `requirements.in` 文件，然后编译并同步：
```bash
# 编译生成包含明确版本的 requirements.txt
uv pip compile requirements.in -o requirements.txt

# 将虚拟环境同步到 requirements.txt 的状态（多退少补）
uv pip sync requirements.txt
```

---

### 总结

对于新项目，强烈建议采用**第一种工作流（`uv init` -> `uv add` -> `uv run`）**。它的体验最为丝滑，将 Python 的依赖管理、环境隔离、版本控制完美统一在了一起，且速度是传统工具的几十倍。