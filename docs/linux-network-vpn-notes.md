# VPN / VPS / WireGuard / OpenVPN 学习笔记

## 1. VPS

VPS（Virtual Private Server）是远程 Linux 服务器。

本质上是一台放在机房中的远程电脑，用户通过 SSH 远程连接。

常见用途：

- 远程开发
- 部署服务
- 文件中转
- VPN 服务
- Docker
- GPU训练
- 机器人远程访问

---

## 2. VPN

VPN（Virtual Private Network）是一种虚拟专用网络技术。

作用：

- 建立加密网络隧道
- 让设备加入远程网络
- 安全传输数据

典型流程：

本地电脑
↓
VPN隧道
↓
远程服务器/VPS
↓
互联网

---

## 3. OpenVPN

OpenVPN 是经典 VPN 方案。

特点：

- 成熟稳定
- 配置灵活
- 企业使用广泛

缺点：

- 配置较复杂
- 性能和延迟不如 WireGuard

相关项目：

- tools/openvpn-install

---

## 4. WireGuard

WireGuard 是较新的 VPN 技术。

特点：

- 配置简单
- 性能高
- 延迟低
- 代码量小

目前越来越流行。

机器人、ROS、远程开发领域也经常使用。

相关项目：

- tools/wireguard-install

---

## 5. Git Submodule

本仓库通过 Git Submodule 管理第三方工具。

添加方式：

```bash
git submodule add 仓库地址 本地目录