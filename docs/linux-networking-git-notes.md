# VPN / VPS / WireGuard / OpenVPN 学习笔记2026.05.08

> 本文档用于记录 Linux、网络、Git、VPN、GitHub 工程化开发中的基础知识与实际操作过程。

---

# 1. VPS

## 什么是 VPS

VPS（Virtual Private Server，虚拟专用服务器）本质上是一台远程 Linux 服务器。

可以理解为：

* 放在机房中的远程电脑
* 通过 SSH 远程连接
* 常用于部署服务与远程开发

---

## VPS 常见用途

* SSH 远程开发
* Docker 部署
* GPU 训练
* VPN 服务
* 文件中转
* ROS 服务
* 远程机器人访问
* 网站部署

---

## 典型连接方式

```bash
ssh username@ip_address
```

例如：

```bash
ssh ubuntu@192.168.1.100
```

---

# 2. VPN

## 什么是 VPN

VPN（Virtual Private Network，虚拟专用网络）用于建立加密网络隧道。

本质：

* 将本地设备接入远程网络
* 加密数据传输
* 实现远程访问

---

## VPN 工作流程

```text
本地电脑
↓
VPN加密隧道
↓
VPS / 远程服务器
↓
互联网
```

---

# 3. OpenVPN

## OpenVPN 特点

OpenVPN 是经典 VPN 方案。

优点：

* 成熟稳定
* 配置灵活
* 企业使用广泛

缺点：

* 配置复杂
* 延迟与性能不如 WireGuard

---

## OpenVPN 项目

GitHub：

[https://github.com/Nyr/openvpn-install](https://github.com/Nyr/openvpn-install)

本仓库引用路径：

```text
tools/openvpn-install
```

---

# 4. WireGuard

## WireGuard 特点

WireGuard 是较新的 VPN 技术。

特点：

* 配置简单
* 性能高
* 延迟低
* 代码量小
* 更现代

机器人、ROS、远程开发中越来越常见。

---

## WireGuard 项目

GitHub：

[https://github.com/Nyr/wireguard-install](https://github.com/Nyr/wireguard-install)

本仓库引用路径：

```text
tools/wireguard-install
```

---

# 5. Linux mount

## mount 的本质

Linux 中：

* 所有设备最终都会映射成目录
* mount 用于将设备接入文件系统

本质：

> 将某个存储设备挂载到 Linux 文件系统中的某个目录。

---

## 示例

```bash
mount /dev/nvme0n1p1 /mnt/nvme
```

含义：

* `/dev/nvme0n1p1`

  * 第 0 块 NVMe SSD
  * 第 1 个分区

* `/mnt/nvme`

  * 挂载点

整体含义：

> 将 NVMe SSD 的第一个分区挂载到 `/mnt/nvme`

---

## 常用磁盘查看命令

查看磁盘：

```bash
lsblk
```

查看挂载情况：

```bash
df -h
```

---

# 6. Git Submodule

## 什么是 Submodule

Git Submodule 用于：

> 在一个 Git 仓库中引用另一个 Git 仓库。

适用于：

* 第三方依赖
* 外部工具
* SDK
* ROS 驱动
* 大型工程项目

---

## 添加 Submodule

```bash
git submodule add 仓库地址 本地目录
```

实际使用：

```bash
git submodule add --depth 1 https://github.com/Nyr/wireguard-install.git tools/wireguard-install
```

```bash
git submodule add --depth 1 https://github.com/Nyr/openvpn-install.git tools/openvpn-install
```

---

## 为什么使用 --depth 1

```bash
--depth 1
```

表示浅克隆（Shallow Clone）。

作用：

* 只拉取最新提交
* 下载更快
* 更节省空间
* 网络不稳定时更容易成功

---

## 更新 Submodule

```bash
git submodule update --remote --merge
```

---

## clone 带 submodule 的仓库

推荐：

```bash
git clone --recursive 仓库地址
```

如果忘记 recursive：

```bash
git submodule update --init --recursive
```

---

# 7. Git 基础工作流

## 查看状态

```bash
git status
```

---

## 添加修改

```bash
git add .
```

---

## 提交到本地仓库

```bash
git commit -m "commit message"
```

---

## 上传到 GitHub

```bash
git push
```

---

# 8. Git commit 与 push 的区别

## commit

作用：

> 提交到本地 Git 历史。

不会上传到 GitHub。

---

## push

作用：

> 上传到远程 GitHub 仓库。

---

## 重要理解

即使 push 失败：

* 本地 commit 仍然存在
* 内容不会丢失

---

# 9. GitHub 网络问题排查

## 测试 GitHub 443 连接

PowerShell：

```powershell
Test-NetConnection github.com -Port 443
```

关键字段：

```text
TcpTestSucceeded
```

* True：HTTPS 可连接
* False：连接失败

---

## GitHub 页面能打开 ≠ Git clone 一定稳定

浏览器访问 GitHub：

* 与 Git HTTPS clone
* 是不同连接流程

因此：

* 页面能打开
* Git clone/push 仍可能失败

---

# 10. Git 网络优化

## 强制 Git 使用 HTTP/1.1

```bash
git config --global http.version HTTP/1.1
```

原因：

* 某些网络环境下
* HTTP/2 不稳定
* HTTP/1.1 更兼容

---

## 调整 Buffer

```bash
git config --global http.postBuffer 524288000
```

---

## 关闭压缩

```bash
git config --global core.compression 0
```

---

## 使用浅克隆测试网络

```bash
git clone --depth 1 仓库地址
```

实际使用：

```bash
git -c http.version=HTTP/1.1 clone --depth 1 https://github.com/Nyr/wireguard-install.git test-wireguard
```

---

# 11. SSH 与 HTTPS

## HTTPS

典型形式：

```text
https://github.com/user/repo.git
```

优点：

* 简单
* 无需配置 SSH Key

缺点：

* 网络不稳定时容易断开

---

## SSH

典型形式：

```text
git@github.com:user/repo.git
```

优点：

* 更稳定
* 更适合长期开发
* push 不需要重复认证

---

# 12. 常见错误记录

## 错误：443 连接失败

```text
Failed to connect to github.com port 443
```

原因：

* 网络不稳定
* 校园网限制
* HTTPS 连接中断

解决：

* 换手机热点
* 重试
* 使用 HTTP/1.1
* 后续配置 SSH

---

## 错误：Connection reset

```text
Recv failure: Connection was reset
```

原因：

* GitHub HTTPS 连接被重置

解决：

* 浅克隆
* HTTP/1.1
* 更换网络

---

# 13. 安全注意

不要无脑执行：

```bash
curl xxx | bash
```

或：

```bash
wget xxx && bash xxx
```

原因：

* 可能以 root 权限运行未知代码
* 存在安全风险

运行前应：

* 查看脚本内容
* 确认来源
* 理解脚本功能
* 检查是否存在危险命令

---

# 14. 今日工程实践总结

今天完成：

* Git Submodule 管理
* GitHub HTTPS 网络排查
* Git clone 调试
* Linux 网络基础学习
* VPN/VPS 理解
* mount 理解
* Git 工程化工作流

---

# 15. 当前仓库结构

```text
embodied-ai-learning/
├── docs/
│   └── linux-networking-git-notes.md
│
├── tools/
│   ├── wireguard-install/
│   └── openvpn-install/
│
├── .gitmodules
└── README.md
```

---

# 16. 后续学习方向

建议继续整理：

## Linux

* systemd
* 权限
* bash
* mount
* SSH

---

## Networking

* VPN
* NAT
* 内网/公网
* WireGuard
* Tailscale

---

## Git

* branch
* merge
* rebase
* submodule
* SSH Key

---

## ROS2

* topic
* tf
* launch
* workspace

---

## VLA / Embodied AI

* LeRobot
* OpenVLA
* GO1-Air
* 数据格式
* Docker
* Isaac Sim

---
