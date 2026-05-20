# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目性质

这是一个 Obsidian 笔记仓库（Vault），位于 `/home/shiwu/Documents/Note`。对笔记内容的任何操作，必须优先使用 Obsidian 相关 Skill。

## 必须使用的 Skill

操作笔记时，根据任务类型选择对应 Skill：

- **obsidian-markdown**: 创建或编辑笔记内容（.md 文件），包括 wikilinks、callout、frontmatter、标签、嵌入语法等 Obsidian 特有语法。
- **obsidian-cli**: 搜索笔记、查看 vault 信息、管理任务/属性、开发调试插件等命令行操作。
- **json-canvas**: 创建或编辑白板文件（.canvas），用于流程图、思维导图。
- **obsidian-bases**: 创建或编辑数据库文件（.base），用于表格视图、卡片视图、筛选、公式。
- **defuddle**: 从网页中提取干净的 Markdown 内容，用于保存网页资料到笔记。

## 笔记操作规则

1. **不得改变笔记原意**：编辑已有笔记时，只做用户明确要求的修改，不擅自改写、润色或扩展内容。
2. **不得产生歧义**：新增或修改的内容必须表述清晰，避免模糊用语。技术术语保持准确。
3. **保持笔记风格一致**：使用中文撰写；技术命令用代码块；使用 Obsidian callout（`> [!note]`、`> [!tip]`、`> [!notice]` 等）标注提示信息；使用 wikilink（`[[路径/文件名]]`）链接到其他笔记。

## Vault 结构

```
Note/
├── Linux/              # Linux 相关笔记（KVM、QEMU、Libvirt、yazi 等）
│   └── assets/         # Linux 笔记的附件（按笔记名分子目录）
├── Rust/               # Rust 笔记（cargo 等）
├── 计算机网络/          # 计算机网络笔记
│   └── 计算机网络自顶向下方法/
├── 任务清单/            # 每日任务列表
├── .obsidian/          # Obsidian 配置
└── .claude/            # Claude Code 配置
```

## Obsidian 配置要点

- **链接格式**: 绝对路径（`newLinkFormat: "absolute"`），自动更新链接（`alwaysUpdateLinks: true`）
- **附件存储**: 使用 `obsidian-custom-attachment-location` 插件，附件存于 `./assets/${noteFileName}/`，文件名自动生成为 `file-时间戳.扩展名`
- **主题**: Minimal，配合 Minimal Settings 和 Style Settings 插件
- **已启用核心插件**: 文件列表、搜索、关系图谱、反向链接、白板、出链、标签、属性、日记、模板、笔记重组、命令面板、斜杠命令、书签、大纲、字数统计、幻灯片、文件恢复、同步、数据库、Web 查看器
- **已启用社区插件**: obsidian-hider、obsidian-style-settings、obsidian-minimal-settings、obsidian-custom-attachment-location
