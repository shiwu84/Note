---
title: Cargo
date: 2026-05-19
tags:
  - rust
  - cargo
---

# Cargo

cargo是Rust的包管理器+构建系统。

## 解决的问题

cargo解决了三个问题：

1. **代码从哪里来?**

你的项目依赖别人写的库。cargo自动从crates.io下载，无需手动管理。

2. **怎么编译?**

Rust编译器rustc参数繁琐。cargo把规则写在Cargo.toml中，一条命令搞定：

```shell
cargo build # 编译
cargo run # 编译并运行
cargo test # 跑测试
```

3. **依赖版本怎么锁定?**

Cargo.lock记录每个依赖的精确版本，保证团队里所有人编译出一模一样的结果。