---
title: Cargo
date: 2026-05-19
tags:
  - rust/cargo
  - rust
---

# Cargo

> [!note] 相关笔记
> Cargo 是 Rust 的包管理器和构建系统。[[Linux/Yazi|Yazi]] 终端文件管理器通过 `cargo install` 安装其 Markdown 预览工具。Rust 编程是 [[任务清单/长期计划|长期计划]] 之一，[[任务清单/2026-05-21|了解 Rust 发展历史]] 是近期的学习任务。

cargo 是 Rust 的**包管理器 + 构建系统**。

## 解决的问题

cargo解决了三个问题：

1. **代码从哪里来?**

你的项目依赖别人写的库。cargo 自动从 ==crates.io== 下载，无需手动管理。

> [!note] crates.io
> crates.io 是 Rust 的**中央包注册表**，类似 npm 之于 Node.js、PyPI 之于 Python。

2. **怎么编译?**

Rust编译器rustc参数繁琐。cargo把规则写在Cargo.toml中，一条命令搞定：

```shell
cargo build # 编译
cargo run # 编译并运行
cargo test # 跑测试
```

3. **依赖版本怎么锁定?**

`Cargo.lock` 记录每个依赖的精确版本，保证团队里所有人编译出一模一样的结果。

## 相关笔记

- [[Linux/Yazi|Yazi]] — 通过 `cargo install` 安装了 mdv（Markdown 预览器）
- [[任务清单/长期计划|长期计划]] — Rust 编程是长期学习目标之一
- [[任务清单/2026-05-21|2026-05-21 任务清单]] — 包含"了解掌握 Rust 发展历史"任务