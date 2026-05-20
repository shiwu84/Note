---
title: QEMU
date: 2026-05-19
tags:
  - linux
  - qemu
  - virtualization
---

# QEMU

QEMU 是一款通用的开源机器模拟器和虚拟化器。当作为机器模拟器使用时，QEMU 可以在不同架构的机器上运行为特定机器（例如 ARM 开发板）制作的操作系统和程序。通过使用动态翻译，它能实现非常优异的性能。

QEMU 可以利用 Xen 或 KVM 等其他虚拟化引擎来使用 CPU 扩展指令集（HVM）进行虚拟化。当作为虚拟化工具使用时，QEMU 通过在宿主机 CPU 上直接执行客户机代码，从而实现接近原生水平的性能。

## 安装

安装 qemu-full 软件包（若不需要图形界面则安装 qemu-base，若只需默认的 x86_64 仿真则可安装 qemu-desktop），并根据需要安装下列可选包：

- qemu-block-gluster - Glusterfs 块设备支持
- qemu-block-iscsi - iSCSI 块设备支持
- samba - SMB/CIFS 服务器支持

此外，还提供 qemu-user-static 作为用户模式静态变体。

## 可信平台模拟

QEMU 可以模拟可信平台模块，这是一些系统（如 Windows 11，需要 TPM 2.0）所必需的。

安装 swtpm 软件包，它提供了一个软件 TPM 实现。