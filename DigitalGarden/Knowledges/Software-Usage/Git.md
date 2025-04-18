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

[[Git GPG Signature]]  
[[Git Proxy Configuration]]
## Refs:
[“Branch early, branch often”](https://medium.com/@huydotnet/branch-early-branch-often-daadaad9468eE)  
[Git 内部原理图解——对象、分支以及如何从零开始建仓库](https://www.freecodecamp.org/chinese/news/git-internals-objects-branches-create-repo/)

