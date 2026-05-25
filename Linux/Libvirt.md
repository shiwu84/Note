---
title: Libvirt
date: 2026-05-19
tags:
  - linux/libvirt
  - linux/virtualization
---

# Libvirt

> [!note] 相关笔记
> Libvirt 是虚拟化**管理中间层**，向下管理 [[Linux/KVM|KVM]] 和 [[Linux/QEMU|QEMU]]，向上提供统一 API。实操指南见 [[Linux/在Arch Linux使用虚拟机|在 Arch Linux 使用虚拟机]]。

Libvirt 是一套软件集合，提供了管理虚拟机及其他虚拟化功能（如存储和网络接口管理）的便捷途径。

这些软件包括一个长期稳定的 **C 语言 API**、一个守护进程（**libvirtd**）以及一个命令行工具（**virsh**）。Libvirt 的主要目标之一是提供一种统一的方式来管理多种不同的虚拟化提供程序/虚拟机管理程序，例如 KVM/QEMU、Xen、LXC、OpenVZ 或 VirtualBox 等。

Libvirt 的一些主要功能包括：

- 虚拟机管理：各种域生命周期操作，如启动、停止、暂停、保存、恢复和迁移。支持多种设备类型的热插拔操作，包括磁盘和网络接口、内存以及 CPU。
- 远程主机支持：libvirt 的所有功能均可在任何运行 libvirt 守护进程的主机上访问，包括远程主机。支持多种用于远程连接的网络传输方式，最简单的是 SSH，无需额外的显式配置。
- 存储管理：任何运行 libvirt 守护进程的主机均可用于管理多种类型的存储：创建各种格式的磁盘镜像（如 qcow2、vmdk、raw 等）、挂载 NFS 共享、枚举现有 LVM 卷组、创建新的 LVM 卷组和逻辑卷、对裸磁盘设备进行分区、挂载 iSCSI 共享等等。
- 网络接口管理：任何运行 libvirt 守护进程的主机均可用于管理物理和逻辑网络接口。枚举现有接口，以及配置（和创建）接口、网桥、VLAN 和绑定设备。
- 虚拟 NAT 和基于路由的网络：任何运行 libvirt 守护进程的主机都可以管理和创建虚拟网络。Libvirt 虚拟网络使用防火墙规则充当路由器，为虚拟机提供对主机网络的无缝访问。

## 安装

由于采用守护进程/客户端架构，libvirt 只需安装在将要承载虚拟化系统的机器上。请注意，服务器和客户端可以是同一台物理机。

### 服务器端

安装 `libvirt` 软件包，以及至少一个虚拟机监控程序，这里选择 KVM/QEMU：

```shell
sudo pacman -S libvirt
```

libvirt KVM/QEMU 驱动是主要的 libvirt 驱动，如果启用了 KVM，就可以使用完全虚拟化、硬件加速的客户机。

为保证网络连接，请安装：

```shell
sudo pacman -S dnsmasq openbsd-netcat
```

- `dnsmasq` —— 用于默认的 NAT/DHCP 网络。
- `openbsd-netcat` —— 用于通过 SSH 进行远程管理。

其他可选依赖项可能提供所需或扩展的功能，例如用于 DMI 系统信息支持的 `dmidecode`。在阅读 `pacman` 针对 `libvirt` 的输出后，请将您可能需要的依赖项作为依赖进行安装。

### 客户端

客户端是用于管理和访问虚拟机的用户界面，这里选择 **virt-manager**。virt-manager 是 libvirt 库的图形用户前端，提供虚拟机管理服务。virt-manager 的界面使用户无需通过终端即可轻松创建、删除和操作虚拟机。

> [!notice]
> virt-manager 主要支持 KVM，但也可以与其他虚拟化程序配合使用，如 Xen 和 LXC。

安装 virt-manager：

```shell
sudo pacman -S virt-manager
```

要使用 QEMU 连接，请启用/启动 `libvirtd.socket` 单元：

```shell
sudo systemctl enable --now libvirtd.socket
```

### virt-manager 基本配置

将自己添加到 `libvirt` 用户组：

```shell
sudo usermod -aG libvirt $USER
```

> [!warning]
> 确保 virt-manager 默认存储池之外的任何文件/文件夹属于 `libvirt-qemu` 组，否则在访问默认存储池之外的文件时可能会遇到**权限错误**。

> [!tip]
> 如果您忘记这样做，virt-manager 会请求权限为您更改这些设置。

## 相关笔记

- [[Linux/KVM|KVM]] — 内核虚拟化模块，Libvirt 管理的核心虚拟化后端
- [[Linux/QEMU|QEMU]] — 硬件设备模拟器，与 KVM 配合使用
- [[Linux/在Arch Linux使用虚拟机|在 Arch Linux 使用虚拟机]] — 从零搭建虚拟化环境的实操指南，包含 Libvirt 完整配置流程