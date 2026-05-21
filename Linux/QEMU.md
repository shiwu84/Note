---
title: QEMU
date: 2026-05-19
tags:
  - linux
  - qemu
  - virtualization
---

# QEMU

> [!note] 相关笔记
> QEMU 与 [[Linux/KVM|KVM]] 配合组成 Linux 虚拟化的核心方案 —— QEMU 负责**设备模拟**，KVM 负责 **CPU/内存虚拟化**。上层由 [[Linux/Libvirt|Libvirt]] 统一管理。实操指南见 [[Linux/在Arch Linux使用虚拟机|在 Arch Linux 使用虚拟机]]。

QEMU 是一款**通用的开源机器模拟器和虚拟化器**。当作为机器模拟器使用时，QEMU 可以在不同架构的机器上运行为特定机器（例如 ARM 开发板）制作的操作系统和程序。通过使用**动态翻译**，它能实现非常优异的性能。

QEMU 可以利用 Xen 或 [[Linux/KVM|KVM]] 等其他虚拟化引擎来使用 CPU 扩展指令集（**HVM**）进行虚拟化。当作为虚拟化工具使用时，QEMU 通过在宿主机 CPU 上直接执行客户机代码，从而实现**接近原生水平的性能**。

## 安装

安装 `qemu-full` 软件包（若不需要图形界面则安装 `qemu-base`，若只需默认的 x86_64 仿真则可安装 `qemu-desktop`）：

```shell
sudo pacman -S qemu-full
```

并根据需要安装下列可选包：

- `qemu-block-gluster` —— Glusterfs 块设备支持
- `qemu-block-iscsi` —— iSCSI 块设备支持
- `samba` —— SMB/CIFS 服务器支持

此外，还提供 `qemu-user-static` 作为用户模式静态变体。

## 可信平台模拟（TPM）

QEMU 可以模拟**可信平台模块（TPM）**，这是一些系统（如 Windows 11）所必需的。

> [!important]
> Windows 11 要求 TPM 2.0，没有 TPM 模拟将**无法完成安装**。在 [[Linux/在Arch Linux使用虚拟机|在 Arch Linux 使用虚拟机]] 中有完整的配置步骤。

安装 `swtpm` 软件包，它提供了一个软件 TPM 实现：

```shell
sudo pacman -S swtpm
```

## 相关笔记

- [[Linux/KVM|KVM]] — 内核虚拟化模块，为 QEMU 提供硬件加速
- [[Linux/Libvirt|Libvirt]] — 虚拟化管理层，统一管理 QEMU/KVM
- [[Linux/在Arch Linux使用虚拟机|在 Arch Linux 使用虚拟机]] — 从零搭建虚拟化环境的实操指南，包含 TPM 配置和 Windows 11 安装流程
