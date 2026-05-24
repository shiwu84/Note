---
date: 2026-05-24
tags:
  - javascript
  - html
  - script
---

# Hello World

> [!note] 相关笔记
> 本文介绍 JavaScript 的 `<script>` 标签用法。后续内容见 [[JavaScript/02_现代模式，"use strict"|use strict 严格模式]] 和 [[JavaScript/03_变量|变量声明]]。学习任务见 [[任务清单/2026-05-24|2026-05-24 任务清单]]。

## `<script>` 标签

几乎可以使用 `<script>` 标签将 JavaScript 代码插入到 HTML 文档的任何位置。

```html
<!DOCTYPE HTML>
<html>

<body>

  <p>script 标签之前...</p>

  <script>
    alert('Hello, world!');
  </script>

  <p>...script 标签之后</p>

</body>

</html>
```

`<script>` 标签中包裹了 JavaScript 代码，当浏览器遇到 `<script>` 标签时，代码会**自动运行**。

## 过时的标记特性

`<script>` 标签有一些现在很少用到的特性，但我们可以在老代码中找到它们：

| 特性 | 写法 | 现状 |
|------|------|------|
| `type` | `<script type="text/javascript">` | 现代 HTML 标准已改变其含义，不再需要。现用于 JavaScript 模块。 |
| `language` | `<script language="javascript">` | 已无任何意义，语言默认就是 JavaScript。 |
| 注释包裹 | `<!-- ... //-->` | 用于不支持 `<script>` 标签的古老浏览器，最近 15 年内发布的浏览器都不需要。 |

## 外部脚本

当有大量的 JavaScript 代码时，可以将它放入单独的文件。脚本文件通过 `src` 特性添加到 HTML 文件中：

```html
<script src="/path/to/script.js"></script>
```

`src` 支持三种路径形式：

| 路径类型 | 示例 | 说明 |
|----------|------|------|
| 绝对路径 | `src="/path/to/script.js"` | 从网站根目录开始 |
| 相对路径 | `src="script.js"` 或 `src="./script.js"` | 相对于当前页面 |
| 完整 URL | `src="https://cdnjs.cloudflare.com/..."` | 加载外部 CDN 资源 |

要附加多个脚本，使用多个标签：

```html
<script src="/js/script1.js"></script>
<script src="/js/script2.js"></script>
…
```

> [!notice]
> 一般来说，只有最简单的脚本才嵌入到 HTML 中。更复杂的脚本存放在单独的文件中。使用独立文件的好处是浏览器会下载它，然后将它保存到浏览器的[缓存](https://en.wikipedia.org/wiki/Web_cache)中。之后，其他页面想要相同的脚本就会从缓存中获取，而不是下载它。所以文件实际上只会下载一次。这可以节省流量，并使得页面（加载）更快。

> [!warning] `src` 与内嵌代码互斥
> 如果设置了 `src` 特性，`script` 标签内容将会被忽略。一个单独的 `<script>` 标签不能同时有 `src` 特性和内部包裹的代码。
>
> ```html
> <script src="file.js">
>   alert(1); // 此内容会被忽略，因为设定了 src
> </script>
> ```
>
> 正确做法是分开两个标签：
>
> ```html
> <script src="file.js"></script>
> <script>
>   alert(1);
> </script>
> ```

## 相关笔记

- [[JavaScript/02_现代模式，"use strict"|use strict 严格模式]] — JavaScript 的现代执行模式
- [[JavaScript/03_变量|变量]] — JavaScript 变量声明与命名
- [[任务清单/2026-05-24|2026-05-24 任务清单]] — JavaScript 学习任务
