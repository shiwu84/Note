---
title: KVM
date: 2026-05-19
tags:
  - linux
  - kvm
  - virtualization
---

# KVM

> [!note] 相关笔记
> KVM 是 Linux 虚拟化栈的核心组件，与 [[Linux/QEMU|QEMU]]（设备模拟）、[[Linux/Libvirt|Libvirt]]（管理层）紧密配合。实操指南见 [[Linux/在Arch Linux使用虚拟机|在 Arch Linux 使用虚拟机]]。

## 检查 KVM 支持情况

检查处理器是否支持硬件虚拟化：

```shell
LC_ALL=C.UTF-8 lscpu | grep Virtualization
```

或者：

```shell
grep -E --color=auto 'vmx|svm|0xc0f' /proc/cpuinfo
```

如果运行任一命令后没有任何输出，则说明您的处理器不支持硬件虚拟化，您将**无法使用 KVM**。

> [!notice]
> 你可能需要在 BIOS 中启用虚拟化支持。AMD 和 Intel 在过去 10 年中制造的所有 x86_64 处理器都支持虚拟化。如果你的处理器看起来不支持虚拟化，几乎可以肯定是 **BIOS 中未启用该功能**。

## 内核支持

Arch Linux 内核提供了支持 KVM 所需的内核模块。

可以通过以下命令检查内核中是否包含必要的模块（`kvm` 以及 `kvm_amd` 或 `kvm_intel`）：

```shell
zgrep CONFIG_KVM= /proc/config.gz
```

只有当模块设置为 `y` 或 `m` 时才可用。

然后，使用以下命令确保内核模块能够自动加载：

```shell
lsmod | grep kvm
```

如果该命令没有返回任何内容，则需要手动加载模块。

> [!tip]
> 若 `modprobe kvm_intel` 或 `modprobe kvm_amd` 失败，但 `modprobe kvm` 成功，且 `lscpu` 声称硬件加速受支持，请检查 BIOS 设置。部分厂商，尤其是笔记本电脑厂商，默认会禁用这些处理器扩展功能。要判断究竟是缺乏硬件支持，还是扩展功能在 BIOS 中被禁用，可在 `modprobe` 失败后查看 `dmesg` 的输出，其中会有相关提示。

## 使用 Virtio 的半虚拟化

半虚拟化为虚拟机使用宿主机设备提供了一种快速高效的通信方式。KVM 使用 Virtio API 作为虚拟机监控器与虚拟机之间的接口层，为虚拟机提供半虚拟化设备。

所有 Virtio 设备都包含两个部分：主机设备与客户机驱动程序。

在虚拟机内部使用以下命令来检查内核中是否提供了 VIRTIO 模块：

```shell
zgrep VIRTIO /proc/config.gz
```

然后，使用以下命令检查内核模块是否自动加载：

```shell
lsmod | grep virtio
```

如果上述命令没有返回任何内容，你需要手动加载内核模块。

## 嵌套虚拟化

嵌套虚拟化允许现有虚拟机在第三方虚拟机管理程序和其他云平台上运行，而无需对原始虚拟机或其网络配置进行任何修改。

在宿主机上，为 kvm_intel 启用嵌套虚拟化功能：

> [!note]
> 对于 AMD 处理器也可进行相同操作，只需在相应位置将 intel 替换为 amd 即可。

```shell
modprobe -r kvm_intel
modprobe kvm_intel ==nested=1==
```

若要永久生效，编辑 `/etc/modprobe.d/kvm_intel.conf`：

```ini
options kvm_intel ==nested=1==
```

验证该项功能是否已激活：

```shell
cat /sys/module/kvm_intel/parameters/nested
```

> [!note]
> 若输出 ==Y== 则表示嵌套虚拟化已激活。

启用”主机透传”模式，将所有CPU功能转发给客户机系统：

1. 如果使用 [[Linux/QEMU|QEMU]]，请通过以下命令运行客户虚拟机：`qemu-system-x86_64 -enable-kvm -cpu host`。
2. 如果使用 virt-manager，请将 CPU 型号更改为 ==host-passthrough==。
3. 如果使用 virsh，请使用 `virsh edit vm-name`，并将 CPU 配置行更改为 `<cpu mode='host-passthrough' check='partial'/>`。

启动虚拟机并检查是否存在 vmx 标志：

```shell
grep -E --color=auto 'vmx|svm' /proc/cpuinfo
```

## 相关笔记

- [[Linux/QEMU|QEMU]] — 硬件设备模拟器，与 KVM 配合提供完整虚拟化方案
- [[Linux/Libvirt|Libvirt]] — 虚拟化管理中间层，统一管理 KVM/QEMU 等虚拟化后端
- [[Linux/在Arch Linux使用虚拟机|在 Arch Linux 使用虚拟机]] — 从零搭建 KVM/QEMU/Libvirt 虚拟化环境的实操指南