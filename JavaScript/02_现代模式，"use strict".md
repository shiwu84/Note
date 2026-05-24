---
date: 2026-05-24
tags:
  - javascript
  - strict-mode
  - es5
---

# 现代模式

> [!note] 相关笔记
> 本文介绍 JavaScript 的严格模式。前置知识见 [[JavaScript/01_Hello World!|Hello World]]，后续内容见 [[JavaScript/03_变量|变量声明]]。学习任务见 [[任务清单/2026-05-24|2026-05-24 任务清单]]。

## 向后兼容的包袱

JavaScript 的演进遵循严格的**"不破坏网络"原则**——只允许添加新特性，不允许更改或移除旧有行为，以此确保绝对的向后兼容性。

这为历史代码提供了无与伦比的保护，但也让 JavaScript 背上了永远的包袱：从诞生之初就存在的每一个设计缺陷，都已成为语言中**不可修正的永久部分**。

## `"use strict"` 严格模式

这种情况一直持续到 2009 年 ECMAScript 5 (ES5) 的出现。ES5 增加新特性的同时，也为一些存在缺陷的旧特性设计了更严格、更合理的新行为。但为了保证旧代码绝对不被破坏，这些修正后的行为**默认并不生效**——你必须使用特殊的指令 `"use strict"` 来明确激活这套更严格的执行模式。

这个指令看上去像一个字符串 `"use strict"` 或者 `'use strict'`。当它处于脚本文件的顶部时，则整个脚本文件都将以**"现代"模式**进行工作：

```javascript
"use strict";

// 代码以现代模式工作
...
```

`"use strict"` 也可以被放在函数体的开头，这样则可以只在该函数中启用严格模式。但通常人们会在整个脚本中启用严格模式。

> [!notice]
> 没有办法取消 `use strict`——没有类似于 `"no use strict"` 这样的指令可以使程序返回默认模式。一旦进入了严格模式，就没有回头路了。

## 浏览器控制台

当使用开发者控制台时，它默认是**不开启** `use strict` 的。

### 方法一：多行输入（Shift + Enter）

在控制台里，用 ==Shift+Enter== 换行，把 `"use strict";` 和需要测试的代码写成一个整体代码块，然后一次性运行：

```javascript
// 在控制台里，用 Shift + Enter 换行，输入完整的代码块后，再按 Enter 执行
'use strict';
// 下面随便写一些会触发严格模式行为的代码来验证
let x = 10;          // 正常
y = 20;              // ReferenceError: y is not defined（非严格模式会创建全局变量）
```

### 方法二：立即执行函数表达式（IIFE）

把代码包裹在一个匿名函数里，在该函数顶部声明严格模式，然后立即调用它。这是**最可靠、最清晰**的方法：

```javascript
(function() {
    'use strict';
    // 你的测试代码
    const obj = {};
    Object.defineProperty(obj, 'x', { value: 42, writable: false });
    obj.x = 100; // TypeError: Cannot assign to read only property 'x'
})();
```

## 是否应该使用 `use strict`？

> [!tip]
> 现代 JavaScript 支持 **class** 和 **module** —— 高级语言结构，它们会**自动启用 `use strict`**。因此，如果代码中使用了 class 或 module，则无需手动添加 `"use strict"` 指令。

## 相关笔记

- [[JavaScript/01_Hello World!|Hello World]] — `<script>` 标签与 JavaScript 代码引入
- [[JavaScript/03_变量|变量]] — JavaScript 变量声明与命名（严格模式对变量声明有影响）
- [[任务清单/2026-05-24|2026-05-24 任务清单]] — JavaScript 学习任务
