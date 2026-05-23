---
title: Yazi
date: 2026-05-19
tags:
  - linux
  - yazi
  - terminal
  - file-manager
---

# Yazi

> [!note] 相关
> Yazi 的 Markdown 预览功能依赖 [[Rust/cargo|Cargo]] 安装的 `mdv` 工具。

在 Arch Linux 中安装 yazi：

```shell
sudo pacman -S yazi ffmpeg 7zip jq poppler fd ripgrep fzf zoxide resvg imagemagick
```

安装Yazi后，通过以下命令启动程序：

```shell
yazi
```

按 ==q== 退出，按 ==~== 打开帮助菜单。

配置Markdown渲染，需要使用[[Rust/cargo]]安装[mdv](https://github.com/WhoSowSee/mdv) ，确保mdv在$PATH中：

```shell
cargo install mdv
ya pkg add WhoSowSee/mdv-previewer
```

将插件添加到 `yazi.toml` 中（如有需要请调整匹配规则）：

```toml
[[plugin.prepend_previewers]]
url = "*.{md,markdown,txt}"
run = "mdv-previewer"

[[plugin.prepend_preloaders]]
url = "*.{md,markdown,txt}"
run = "mdv-previewer"
```

## Shell 包装器

我们建议使用这个 `y` shell 封装函数，它能在退出 Yazi 时保留当前工作目录的更改。

```bash
function y() {
    local tmp="$(mktemp -t "yazi-cwd.XXXXXX")" cwd
    command yazi "$@" --cwd-file="$tmp"
    IFS= read -r -d '' cwd < "$tmp"
    [ "$cwd" != "$PWD" ] && [ -d "$cwd" ] && builtin cd -- "$cwd"
    command rm -f -- "$tmp"
}
```

要使用它，请将该函数复制到您所使用 Shell 的配置文件中。

之后使用 `y` 代替 `yazi` 来启动程序，按下 ==q== 键退出，你会看到当前工作目录已经改变。如果你不希望改变目录，按下 ==Q== 键退出即可。

> [!tip] q vs Q
> `q` 退出并**切换到 Yazi 最后所在的目录**，`Q` 退出并**保持当前 Shell 目录不变**。

## 快捷键绑定

> [!tip]
> 所有按键绑定，请参阅 [默认的 `keymap.toml` 文件](https://github.com/sxyazi/yazi/blob/shipped/yazi-config/preset/keymap-default.toml)。

要在文件和目录之间导航，您可以使用方向键 ← 、 ↓ 、 ↑ 和 → 或类似 Vim 的按键，例如 h 、 j 、 k 、 l ：

| 按键绑定 | 备用键 | 操作 |
| --- | --- | --- |
| k | ↑ | 向上移动光标 |
| j | ↓ | 向下移动光标 |
| l | → | 进入悬停目录 |
| h | ← | 离开当前目录，进入其父目录 |

更多导航操作见下表。

| 按键绑定 | 操作 |
| --- | --- |
| K | 在预览中向上滚动 5 个单位 |
| J | 在预览中向下移动 5 个单位 |
| g ⇒ g | 将光标移动到顶部 |
| G | 将光标移至底部 |
| z | 通过 fzf [进入](https://yazi-rs.github.io/docs/configuration/keymap#mgr.cd) 目录或 [打开](https://yazi-rs.github.io/docs/configuration/keymap#mgr.reveal) 文件 |
| Z | 通过 zoxide [进入](https://yazi-rs.github.io/docs/configuration/keymap#mgr.cd) 目录 |
| g ⇒ 空格 | 通过交互式提示 [切换到](https://yazi-rs.github.io/docs/configuration/keymap#mgr.cd) 某个目录或 [定位](https://yazi-rs.github.io/docs/configuration/keymap#mgr.reveal) 某个文件 |

### 选择

以下操作用于选择文件和目录。

| 按键绑定 | 操作 |
| --- | --- |
| 空格 | 切换选中悬停的文件/目录 |
| v | 进入可视模式 (选择模式) |
| V | 进入可视模式（取消选择模式） |
| Ctrl + a | 选择所有文件 |
| Ctrl + r | 反选所有文件 |
| Esc | 取消选择 |

### 文件操作

使用以下任一操作与选中的文件/目录进行交互。

| 按键绑定 | 操作 |
| --- | --- |
| o | 打开选中的文件 |
| O | 交互式打开选中的文件 |
| 回车 | 打开选中的文件 |
| Shift + Enter | 以交互方式打开选中的文件（部分终端尚不支持） |
| Tab 键 | 显示文件信息 |
| y | 复制选中文件 |
| x | 剪切已选文件 |
| p | 粘贴已剪切文件 |
| P | 粘贴已复制的文件（若目标已存在则覆盖） |
| Y 或 X | 取消复制状态 |
| d | 将选中文件移至回收站 |
| D | 永久删除选中文件 |
| a | 新建文件（以 \`/\` 结尾则创建目录） |
| r | 重命名选中的文件 |
| . | 切换隐藏文件的可见性 |

更多文件操作行为详见下表。

| 按键绑定 | 操作 |
| --- | --- |
| ; | 运行 shell 命令 |
| : | 运行 Shell 命令（阻塞至执行完毕） |
| \- | 为已标记文件创建绝对路径的符号链接 |
| \_ | 为已选文件创建相对路径的符号链接 |
| Ctrl + \- | 为已选文件创建硬链接 |

### 复制路径

要复制路径，请使用以下任一操作。

*注意： c ⇒ d 表示先按 c 键，再按 d 键。*

| 按键绑定 | 操作 |
| --- | --- |
| c ⇒ c | 复制文件路径 |
| c ⇒ d | 复制目录路径 |
| c ⇒ f | 复制文件名 |
| c ⇒ n | 复制文件名（不含扩展名） |

### 文件筛选

| 按键绑定 | 操作 |
| --- | --- |
| f | 过滤文件 |

### 查找文件

| 按键绑定 | 操作 |
| --- | --- |
| / | 查找下一个文件 |
| ? | 查找上一个文件 |
| n | 跳转到下一个查找结果 |
| N | 跳转到上一个查找结果 |

### 搜索文件

| 按键绑定 | 操作 |
| --- | --- |
| s | 使用 [fd](https://github.com/sharkdp/fd) 按文件名搜索文件 |
| S | 使用 [ripgrep](https://github.com/BurntSushi/ripgrep) 按内容搜索文件 |
| Ctrl + s | 取消正在进行的搜索 |

### 排序

使用以下操作对文件/目录进行排序。

*提示：, ⇒ a 表示先按 , 键，再按 a 键。*

| 按键绑定 | 操作 |
| --- | --- |
| , ⇒ m | 按修改时间排序 |
| , ⇒ M | 按修改时间排序（倒序） |
| , ⇒ b | 按创建时间排序 |
| , ⇒ B | 按创建时间排序（倒序） |
| , ⇒ e | 按文件扩展名排序 |
| , ⇒ E | 按文件扩展名排序（倒序） |
| , ⇒ a | 按字母顺序排序 |
| , ⇒ A | 按字母倒序排列 |
| , ⇒ n | 自然顺序排列 |
| , ⇒ N | 自然排序（逆向） |
| , ⇒ s | 按大小排序 |
| , ⇒ S | 按大小排序（倒序） |
| , ⇒ r | 随机排序 |

### 多标签页

| 按键绑定 | 操作 |
| --- | --- |
| t ⇒ t | 在当前工作目录创建新标签页 |
| 1, 2,..., 9 | 切换到第 N 个标签页 |
| \[ | 切换到上一个标签页 |
| \] | 切换到下一个标签页 |
| { | 与上一个标签页交换当前标签页 |
| } | 与下一个标签页交换位置 |
| Ctrl + c | 关闭当前标签页 |

## 主题风格

从我们的 [风格仓库](https://github.com/yazi-rs/flavors) 中挑选一个你喜欢的配色方案，或者 [烹制一款风格](https://yazi-rs.github.io/docs/flavors/overview#cooking)！

## 相关笔记

- [[Rust/cargo|Cargo]] — Rust 包管理器，yazi 的 Markdown 预览工具 `mdv` 通过 `cargo install` 安装
- [[工具|工具]] — 系统工具汇总