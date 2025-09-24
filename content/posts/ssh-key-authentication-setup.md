---
title: "SSH公钥认证配置指南：Windows到Linux服务器"
date: 2025-09-15
draft: false
tags: ["SSH", "Linux", "Windows", "安全", "运维", "教程"]
---

## **技术记录：配置从 Windows 客户端到 Linux 服务器的 SSH 公钥认证**

**目标:**
为 IP 地址为 `192.168.74.131` 的 Linux 服务器配置 SSH 公钥认证，以允许从 Windows 客户端进行无密码的安全登录。

**环境:**
*   **客户端:** Windows 11 (使用内置 OpenSSH 客户端，通过 PowerShell 执行)
*   **服务器:** Debian GNU/Linux (IP: `192.168.74.131`, 登录用户: `root`)

---

### **执行流程**

#### **第一阶段：在 Windows 客户端生成密钥对**

1.  **启动 PowerShell:** 打开 Windows PowerShell 终端。

2.  **执行密钥生成命令:** 运行 `ssh-keygen` 程序以创建公钥/私钥对。选用 `ed25519` 算法以获得更好的安全性与性能。
    ```powershell
    PS C:\> ssh-keygen -t ed25519
    ```

3.  **确认密钥存储路径与密码:**
    程序将提示指定文件保存路径及设置密码 (passphrase)。为实现无密码登录，所有提示均采用默认值（直接按 `Enter` 键）。
    *   **File path:** `C:\Users\YourUsername\.ssh\id_ed25519` (默认)
    *   **Passphrase:** (留空)
    *   **Passphrase confirmation:** (留空)

4.  **验证密钥生成:** 命令执行成功后，会在指定的路径下生成两个文件：
    *   `id_ed25519`: 私钥文件。此文件必须严格保密。
    *   `id_ed25519.pub`: 公钥文件。此文件将被部署到目标服务器。

#### **第二阶段：在 Linux 服务器上部署公钥**

1.  **在客户端读取并复制公钥:**
    使用 PowerShell 读取公-钥文件内容，并将其复制到系统剪贴板。
    ```powershell
    PS C:\> Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard
    ```

2.  **通过密码认证连接到服务器:**
    建立一个临时的、基于密码的 SSH 会话，以便对服务器进行配置。
    ```powershell
    PS C:\> ssh root@192.168.74.131
    ```
    在提示时输入 `root` 用户的当前密码。

3.  **在服务器端安装公钥:**
    登录服务器后，执行以下命令序列。
    
    a. **确保 `.ssh` 目录存在:**
    `mkdir -p ~/.ssh`
    该命令会创建 `~/.ssh` 目录，如果目录已存在则不执行任何操作。

    b. **将公钥追加到认证文件:**
    将剪贴板中的公钥内容追加到 `~/.ssh/authorized_keys` 文件中。
    ```bash
    root@debian:~# echo '在此处粘贴剪贴板中的公钥内容' >> ~/.ssh/authorized_keys
    ```
    **注:** `' '` 单引号内的内容应被替换为上一步复制的、以 `ssh-ed25519` 开头的完整公钥字符串。

4.  **设置严格的文件权限:**
    为满足 SSH 服务的安全要求，必须为 `.ssh` 目录及 `authorized_keys` 文件设置正确的访问权限。权限过于开放会导致公钥认证失败。
    ```bash
    root@debian:~# chmod 700 ~/.ssh
    root@debian:~# chmod 600 ~/.ssh/authorized_keys
    ```
    *   `chmod 700 ~/.ssh`: 仅允许属主用户对该目录进行读、写、执行操作。
    *   `chmod 600 ~/.ssh/authorized_keys`: 仅允许属主用户对该文件进行读、写操作。

5.  **断开连接:**
    完成服务器端配置后，退出当前 SSH 会话。
    ```bash
    root@debian:~# exit
    ```

#### **第三阶段：验证公钥认证**

1.  **从客户端发起新的连接:**
    在 Windows PowerShell 中，再次执行 SSH 连接命令。
    ```powershell
    PS C:\> ssh root@192.168.74.131
    ```

2.  **确认结果:**
    系统应不再提示输入密码，而是直接完成认证并显示服务器的命令行提示符。这表明公钥认证已成功配置。

通过以上步骤，成功实现了从指定的 Windows 客户端到 Linux 服务器的基于公钥的 SSH 无密码登录，提升了操作便捷性与安全性。