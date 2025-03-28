---
tags:
  - "git"
---

在 Git 中配置 GPG 签名可以为你的提交（commit）和标签（tag）添加数字签名，以验证提交者的身份，增强项目的安全性。以下是详细步骤，适用于 Linux、macOS 和 Windows（使用 Git Bash 或 WSL）。

### 步骤 1：安装 GPG
首先确保你的系统已安装 GPG 工具。

- **Linux**（如 Ubuntu）：

  ```bash
  sudo apt update
  sudo apt install gnupg
  ```

- **macOS**（使用 Homebrew）：

  ```bash
  brew install gnupg
  ```

- **Windows**：
  - 下载并安装 [Gpg4win](https://www.gpg4win.org/)，安装后 GPG 会自动添加到 PATH。
  - 在 Git Bash 或终端中运行 `gpg --version` 检查是否可用。

运行以下命令确认安装：

```bash
gpg --version
```

如果输出版本信息，说明安装成功。

### 步骤 2：生成 GPG 密钥对
1. **生成密钥**：

   ```bash
   gpg --full-generate-key
   ```

   - 按提示操作：
     - 选择密钥类型：默认 `(1) RSA and RSA`。
     - 密钥长度：推荐 `4096`（更安全）。
     - 有效期：可选择默认（0 = 永不过期）或设置具体时间。
     - 输入真实姓名、邮箱（建议与 GitHub 绑定的邮箱一致）、可选注释。
     - 设置密码保护密钥（可选但推荐）。

2. **查看生成的密钥**：

   ```bash
   gpg --list-keys
   ```

   输出中会显示类似以下内容：

   ```
   pub   rsa4096 2023-01-01 [SC]
         ABCDEF1234567890ABCDEF1234567890ABCDEF12
   uid           [ultimate] Your Name <your.email@example.com>
   ```

   - 记录密钥 ID（例如 `ABCDEF1234567890ABCDEF1234567890ABCDEF12`），后续需要用到。

3. **获取密钥的短 ID**（可选）：  
   如果需要短格式的密钥 ID（8 或 16 位），运行：

   ```bash
   gpg --list-keys --keyid-format SHORT
   ```

   例如：`ABCDEF12`。

### 步骤 3：配置 Git 使用 GPG 签名
1. **告诉 Git 使用你的 GPG 密钥**：
   - 使用完整密钥 ID（推荐）：

     ```bash
     git config --global user.signingkey ABCDEF1234567890ABCDEF1234567890ABCDEF12
     ```

   - 或使用短 ID：

     ```bash
     git config --global user.signingkey ABCDEF12
     ```

2. **启用提交签名**：
   - 默认对所有提交签名：

     ```bash
     git config --global commit.gpgsign true
     ```

   - 如果只想对特定项目签名，可以去掉 `--global`，在项目目录下运行。

3. **设置 GPG 程序（可选）**：  
   如果 Git 无法自动找到 GPG，确保指定 GPG 可执行文件路径：

   ```bash
   git config --global gpg.program gpg
   ```

4. **验证配置**：  
   检查 Git 配置：

   ```bash
   git config --global --list
   ```

   应包含：

   ```
   user.signingkey=ABCDEF1234567890ABCDEF1234567890ABCDEF12
   commit.gpgsign=true
   ```

---

### 步骤 4：将公钥添加到 GitHub
1. **导出公钥**：

   ```bash
   gpg --armor --export your.email@example.com
   ```

   输出类似以下内容：

   ```
   -----BEGIN PGP PUBLIC KEY BLOCK-----
   ...
   -----END PGP PUBLIC KEY BLOCK-----
   ```

2. **复制公钥**：  
   将输出内容复制到剪贴板。

3. **添加到 GitHub**：
   - 登录 GitHub，进入 **Settings > SSH and GPG keys**。
   - 点击 **New GPG key**。
   - 粘贴公钥并保存。

### 步骤 5：测试 GPG 签名
1. **创建测试提交**：  
   在任意 Git 仓库中：

   ```bash
   echo "Test" >> test.txt
   git add test.txt
   git commit -m "Test GPG signature"
   ```

   - 如果配置正确，提交会自动签名，可能需要输入密钥密码（若设置了）。

2. **验证签名**：

   ```bash
   git log --show-signature
   ```

   输出应包含：

   ```
   gpg: Signature made ...
   gpg: Good signature from "Your Name <your.email@example.com>"
   ```

3. **推送到 GitHub**：

   ```bash
   git push
   ```

   在 GitHub 上查看提交，旁边会显示 “Verified” 标记。

---

### 常见问题解决
1. **“gpg: signing failed: No secret key”**：
   - 确保 `user.signingkey` 设置的密钥 ID 正确。
   - 检查密钥是否存在：

     ```bash
     gpg --list-secret-keys
     ```

2. **Windows 上 GPG 找不到**：
   - 确保 Gpg4win 安装正确，添加环境变量：

     ```bash
     git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"
     ```

3. **每次提交都要输入密码**：
   - 配置 GPG Agent 缓存密码（Linux/macOS）：

     ```bash
     echo "default-cache-ttl 3600" >> ~/.gnupg/gpg-agent.conf
     gpg-agent --daemon
     ```

4. git 签名失败

```bash
-----BEGIN PGP SIGNED MESSAGE----- 
Hash: SHA512 test gpg: 
signing failed: Inappropriate ioctl for device gpg: [stdin]: clear-sign failed: Inappropriate ioctl for device
```

一般是管道出现问题，因为要通过交互式命令输入密码  
 `export GPG_TTY=$(tty)`

### 总结
- **安装 GPG** → **生成密钥** → **配置 Git** → **上传公钥到 GitHub** → **测试签名**。
- 配置完成后，所有签名提交都会在 GitHub 上显示为 “Verified”。

如果你在某个步骤遇到问题，告诉我具体错误，我可以帮你调试！