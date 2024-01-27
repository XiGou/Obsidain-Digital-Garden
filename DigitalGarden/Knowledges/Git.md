---
dg-publish: true
link: https://xigou.notion.site/Git-fc6344fe1bfc42be8fe0f616a115f7ac
notionID: fc6344fe-1bfc-42be-8fe0-f616a115f7ac
---
要将一个 commit 导出成 patch 并将其应用到另一台机器上，你可以使用 `git format-patch` 和 `git apply` 命令来完成。

在源机器上，进入你的 Git 仓库，并使用以下命令将 commit 导出为 patch 文件：

```bash
git format-patch -1 <commit-hash>
```

其中 `<commit-hash>` 是你要导出的 commit 的哈希值或分支名。上述命令将生成一个以 commit 信息命名的 patch 文件，例如 `0001-Commit-Message.patch`。

然后，将生成的 patch 文件复制到目标机器上。在目标机器上，进入你的 Git 仓库，并使用以下命令将 patch 文件应用到代码中：

```bash
git apply <path-to-patch-file>
```
