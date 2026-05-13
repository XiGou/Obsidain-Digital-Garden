---
dg-publish: true
link: https://xigou.notion.site/Git-fc6344fe1bfc42be8fe0f616a115f7ac
notionID: fc6344fe-1bfc-42be-8fe0-f616a115f7ac
tags:
  - git
---

### Config email and username:

```bash
git config --global user.name "username"
git config --global user.email useremail@qq.com 
git config --global core.editor vim
git config --global credential.helper store
git config --list 
```

## HTTP Credential

首先打开 `git config --global credential.helper store`

然后生成一个 PTA (Personal access tokens)  https://github.com/settings/tokens

Clone 一个私有仓库即可进入交互式登录命令行，然后输入用户名和 PTA 登录（非密码），git 就会记住 PTA，以后将不再需要登录。

## 基本操作

拉取 Github PR：

```bash
# 获取特定 PR 分支 (假设 PR 编号是 123，本地分支为 pr123)
git fetch origin pull/123/head:pr123
```

## 暂存区操作

在 Git 的日常操作中，这个需求非常高频（通常是误加了配置文件或二进制文件）。核心关键词是 **`--cached`**，它表示只操作索引（Stage），不碰物理文件。

根据你的场景，分为以下三种处理方式：

---

### 1. 从暂存区 (Stage/Index) 移除
**场景：** 你刚执行了 `git add .`，发现把不该加的文件也加进去了，但还没执行 `commit`。

* **现代推荐做法 (Git 2.23+):**
    ```bash
    git restore --staged <文件路径>
    ```
* **传统做法:**
    ```bash
    git reset HEAD <文件路径>
    ```
**结果：** 文件变回 "Untracked" 或 "Modified" 状态，硬盘里的文件完好无损。

---

### 2. 从上一次提交 (Commit) 中移除
**场景：** 你刚刚执行了 `commit`，发现里面包含了一个不该提交的文件，想把它“撤回”到工作区。

1.  **先从暂存区移除（保留物理文件）：**
    ```bash
    git rm --cached <文件路径>
    ```
2.  **覆盖上一次提交：**
    ```bash
    git commit --amend --no-edit
    ```
**结果：** `ZBS-31142` 的最新提交里不再有这个文件，但它依然躺在你的硬盘里。

---

### 3. 从更早的提交中移除 (Interactive Rebase)
**场景：** 那个文件在 3 个 commit 之前就被加进去了，你想在不丢失后续工作的情况下把它删掉。

1.  **开始交互式变基：**
    ```bash
    git rebase -i HEAD~4  # 数字取决于你想回溯多远
    ```
2.  **在编辑器里：** 将目标 commit 前面的 `pick` 改为 **`edit`**，保存退出。
3.  **执行移除：**
    ```bash
    git rm --cached <文件路径>
    ```
4.  **修正 commit 并继续：**
    ```bash
    git commit --amend --no-edit
    git rebase --continue
    ```

---

### 4. 彻底停止追踪（但保留物理文件）
**场景：** 某个文件（如 `config.yaml`）已经被追踪并提交过了，现在你想把它加入 `.gitignore`，让 Git 以后彻底无视它，但不能删掉本地文件。

```bash
git rm --cached <文件路径>
# 然后手动在 .gitignore 中添加该路径
git add .gitignore
git commit -m "Untrack sensitive config file"
```

---

### 💡 极客提示：
作为开发者，建议给这个操作设个别名。如果你经常需要把文件从追踪列表中剔除，可以在 `.gitconfig` 里加一行：

```ini
[alias]
    unstage = restore --staged
    untrack = rm --cached
```

这样你只需要执行 `git untrack secret.key` 就能安全地把它从 Git 的视线中移除了。

你在处理 `zbs-client-go` 的代码时，是不是不小心把测试产生的某些大文件或者日志加进去了？如果你发现仓库体积异常变大，可能还需要用到 `git filter-repo` 这种更重型的历史清理工具。

[[Git GPG Signature]]  
[[Git Proxy Configuration]]
## Refs:
[“Branch early, branch often”](https://medium.com/@huydotnet/branch-early-branch-often-daadaad9468eE)  
[Git 内部原理图解——对象、分支以及如何从零开始建仓库](https://www.freecodecamp.org/chinese/news/git-internals-objects-branches-create-repo/)

