

## TL;DR;
- **生成 patch**：`git format-patch <commit-range>`。
- **传输**：手动或用工具（如 `scp`）。
- **应用**：
  - `git apply`：只应用更改。
  - `git am`：应用并创建 commit（推荐）。

这个方法简单高效，适合跨机器共享代码。

---

在 Git 中，你可以将某个 commit 转换为 patch 文件，然后传输到另一台机器上并应用（apply）。这个过程通常用于在无法直接推送（push）或拉取（pull）的情况下共享更改，例如通过电子邮件或手动文件传输。以下是具体的步骤和命令：

---

### 步骤 1：在源机器上生成 Patch 文件

#### 使用 `git format-patch`
`git format-patch` 是专门用来将 commit 转换为 patch 文件的命令。每个 patch 文件会包含 commit 的元数据（作者、日期、消息等）和更改内容。

1. **查看目标 commit**
   - 运行以下命令查看 commit 历史，找到你想转换的 commit 的 SHA 值：

     ```bash
     git log --oneline
     ```

   - 假设目标 commit 是 `abc1234`。

2. **生成 Patch 文件**
   - 生成从某个 commit 到 HEAD 的所有 patch：

     ```bash
     git format-patch abc1234^..abc1234
     ```

     - `abc1234^..abc1234` 表示只包含 `abc1234` 这一个 commit。
     - 输出：生成一个文件，例如 `0001-Your-commit-message.patch`。
   - 如果想生成多个 commit 的 patch，例如从 `abc1234` 到 `def5678`：

     ```bash
     git format-patch abc1234..def5678
     ```

     - 这会生成多个 patch 文件（每个 commit 一个），文件名以数字递增开头。

3. **检查 Patch 文件**
   - 生成的 patch 文件是文本格式，可以用 `cat` 或文本编辑器查看：

     ```bash
     cat 0001-Your-commit-message.patch
     ```

   - 内容包括 commit 信息和 diff。

---

### 步骤 2：传输 Patch 文件
- 将生成的 `.patch` 文件传输到目标机器，可以通过：
  - **U 盘**：手动复制。
  - **SCP**：

    ```bash
    scp 0001-Your-commit-message.patch user@remote-machine:/path/to/destination
    ```

  - **电子邮件**：作为附件发送。
  - 任何其他文件传输方式。

---

### 步骤 3：在目标机器上应用 Patch 文件

#### 使用 `git apply` 或 `git am`
Git 提供了两种主要命令来应用 patch：
- **`git apply`**：只应用 diff，不保留 commit 元数据（如作者、消息）。
- **`git am`**（apply mail）：应用 patch 并创建对应的 commit，保留元数据。

1. **切换到目标仓库**
   - 确保你在目标机器上的 Git 仓库中，且当前分支是你想应用 patch 的分支：

     ```bash
     git checkout target-branch
     ```

2. **应用 Patch**
   - **使用 `git apply`（不创建 commit）**：

     ```bash
     git apply /path/to/0001-Your-commit-message.patch
     ```

     - 检查更改：

       ```bash
       git diff
       ```

     - 手动提交：

       ```bash
       git add .
       git commit -m "Apply patch from abc1234"
       ```

   - **使用 `git am`（推荐，保留 commit 信息）**：

     ```bash
     git am /path/to/0001-Your-commit-message.patch
     ```

     - 这会直接创建一个新 commit，保留原始作者、日期和消息。
     - 如果有多个 patch 文件，按顺序应用：

       ```bash
       git am *.patch
       ```

3. **验证**
   - 检查应用后的历史：

     ```bash
     git log --oneline
     ```

   - 确保更改已正确应用。

### 冲突

使用 git apply 的 --reject 选项（部分应用）  
git apply 支持 --reject 选项，可以让它尝试应用能匹配的部分，并将无法应用的部分保存为 .rej 文件。

### 示例完整流程

#### 源机器

```bash
# 查看 commit
git log --oneline
# 假设目标 commit 是 abc1234
git format-patch abc1234^..abc1234
# 生成 0001-My-commit-message.patch

# 传输到目标机器
scp 0001-My-commit-message.patch user@remote:/home/user/patches/
```

#### 目标机器

```bash
# 进入目标仓库
cd /path/to/repo
git checkout main

# 应用 patch
git am /home/user/patches/0001-My-commit-message.patch

# 检查
git log --oneline
```

---

### 注意事项

1. **冲突处理**
   - 如果 patch 应用时遇到冲突：
     - `git apply` 会报错并停止，你需要手动修改冲突文件，然后提交。
     - `git am` 会暂停并提示冲突：

       ```bash
       git status  # 查看冲突
       # 解决冲突后
       git add <file>
       git am --continue
       ```

   - 或放弃应用：

     ```bash
     git am --abort
     ```

2. **编码问题**
   - 如果 patch 文件包含中文，确保传输过程中编码一致（通常 UTF-8）。可以用 `file` 检查：

     ```bash
     file 0001-My-commit-message.patch
     ```

3. **分支匹配**
   - 确保目标机器的当前分支状态与源机器的基准状态接近，否则 patch 可能无法应用。

4. **文件名自定义**
   - 默认 patch 文件名基于 commit 消息，可以用 `-o` 指定输出目录或手动重命名：

     ```bash
     git format-patch abc1234^..abc1234 -o /path/to/patches
     ```