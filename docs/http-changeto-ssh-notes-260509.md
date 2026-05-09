# GitHub SSH 配置与使用笔记

## 为什么改用 SSH

之前使用 GitHub HTTPS：

```text
https://github.com/user/repo.git
```

在校园网环境下经常出现：

```text
Failed to connect to github.com port 443
Recv failure: Connection was reset
```

原因：

* GitHub HTTPS 连接不稳定
* 校园网/运营商网络波动
* Git clone 与浏览器访问 GitHub 不是同一链路

因此改为：

# SSH 方式连接 GitHub

---

# 1. HTTPS 与 SSH 的区别

## HTTPS

典型形式：

```text
https://github.com/user/repo.git
```

优点：

* 简单
* 无需配置 SSH key

缺点：

* push / clone 容易断连
* 网络不稳定时经常失败

---

## SSH

典型形式：

```text
git@github.com:user/repo.git
```

优点：

* 更稳定
* 更适合长期开发
* 不需要重复登录
* GitHub 工程开发更常用

---

# 2. 查看 Git 邮箱

用于生成 SSH key：

```powershell
git config --global user.email
```

输出：

```text
mgao88406@gmail.com
```

---

# 3. 生成 SSH Key

命令：

```powershell
ssh-keygen -t ed25519 -C "mgao88406@gmail.com"
```

参数说明：

* `ed25519`

  * 更现代的 SSH 算法
  * 推荐使用

* `-C`

  * comment
  * 一般写 GitHub 邮箱

---

## 生成过程

出现：

```text
Enter file in which to save the key
```

直接回车即可。

---

出现：

```text
Enter passphrase
```

可以：

* 直接回车（方便）
* 或设置密码（更安全）

---

## 生成成功后

会生成：

```text
~/.ssh/id_ed25519
```

私钥（private key）

以及：

```text
~/.ssh/id_ed25519.pub
```

公钥（public key）

---

# 4. Randomart Image

生成 SSH key 后出现：

```text
+--[ED25519 256]--+
...
+----[SHA256]-----+
```

这是：

# SSH key 指纹图

作用：

* 用字符画表示 key 指纹
* 用于辅助验证 key

不是报错。

---

# 5. 查看 SSH 公钥

命令：

```powershell
cat ~/.ssh/id_ed25519.pub
```

输出类似：

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... mgao88406@gmail.com
```

需要：

# 完整复制整行内容

---

# 6. 添加 SSH Key 到 GitHub

进入：

```text
GitHub
→ Settings
→ SSH and GPG keys
→ New SSH key
```

填写：

## Title

例如：

```text
Windows Laptop
```

---

## Key

粘贴：

```text
ssh-ed25519 AAAA...
```

保存即可。

---

# 7. 测试 SSH 是否成功

命令：

```powershell
ssh -T git@github.com
```

第一次连接时：

```text
Are you sure you want to continue connecting?
```

输入：

```text
yes
```

---

## 成功结果

出现：

```text
Hi Mira-Coco! You've successfully authenticated...
```

说明：

# GitHub SSH 配置成功

---

# 8. known_hosts

第一次 SSH 连接 GitHub 后：

Git 会自动记录：

```text
known_hosts
```

用于保存：

* GitHub 主机指纹
* 防止中间人攻击

以后不会再询问。

---

# 9. 修改 Git 仓库远程地址

原来的 HTTPS：

```text
https://github.com/Mira-Coco/embodied-ai-learning.git
```

改为 SSH：

```powershell
git remote set-url origin git@github.com:Mira-Coco/embodied-ai-learning.git
```

---

# 10. 查看当前远程地址

命令：

```powershell
git remote -v
```

输出：

```text
origin  git@github.com:Mira-Coco/embodied-ai-learning.git
```

说明：

# 已切换到 SSH

---

# 11. 之后的 Git 工作流

## 查看状态

```powershell
git status
```

---

## 添加修改

```powershell
git add .
```

---

## 本地提交

```powershell
git commit -m "message"
```

---

## 上传 GitHub

```powershell
git push
```

现在：

# push 已默认走 SSH

不再使用 HTTPS。

---

# 12. SSH 的优点总结

* 更稳定
* 更适合长期开发
* 不容易被校园网 HTTPS 干扰
* 不需要重复登录 GitHub
* Linux / ROS / 服务器开发常用

---

# 13. 当前开发环境

目前开发环境：

```text
Windows + PowerShell
VS Code
Git
GitHub SSH
Git Submodule
Linux / Networking Notes
```

已经具备：

# 基础工程化开发环境

---

# 14. 后续值得继续学习

## Git

* branch
* merge
* rebase
* stash

---

## SSH

* SSH config
* 多服务器管理
* SSH agent

---

## Linux

* 权限
* systemd
* bash
* tmux

---

## Networking

* VPN
* WireGuard
* Tailscale
* NAT
* 内网穿透

---

## ROS2 / Robotics

* topic
* tf
* launch
* workspace
* Docker
* GPU server
