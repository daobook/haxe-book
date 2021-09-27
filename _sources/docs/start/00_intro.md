# Haxe 概述

介绍 Haxe 语言，编译器，标准库和其他任何你需要的入门知识。

(haxe:language-intro)=
## Haxe 语言简介

Haxe 编程语言是一种非常现代的高级编程语言。如果您已经熟悉 Java、C++、PHP、AS3 或任何类似的面向对象语言，那么学习它是非常容易的。Haxe 语言的更多特性：

- 通用的
- 严格类型，具有类型推断
- 编译（到多种语言目标/平台，包括 VM 字节码）
- 词法性质的（lexically-scoped）
- 一切都是一个表达式
- 错误处理的异常
- 标准库包括特定于目标平台的模块，以及所有目标通用的模块
- 支持面向对象、泛型和函数式编程
- 熟悉的语法
- Haxe 语言故意保持相当简单、优雅和实用，以适应编译大量不同的目标平台。

由于 Haxe 语言是专门为 Haxe 工具包设计的，所以我们特别注意了它的灵活性。因此，该语言可以轻松地适应开发项目中针对的不同平台的原生行为。这使得非常高效的跨平台开发成为可能，最终节省了时间和资源。

查看 [Haxe 语言功能介绍](haxe:language-features)，了解一些主要的语言功能。

Haxe 由一种高级的开源编程语言和一个编译器组成。它允许将使用面向 ECMAScript 语法编写的程序编译为多种目标语言。使用适当的抽象，可以维护一个可编译为多个目标的代码库。Haxe 是强类型的，但是类型系统可以在需要的地方被颠覆。利用类型信息，Haxe 类型系统可以在编译时检测错误，而这些错误在目标语言的运行时才会被注意到。此外，编译器可以使用类型信息来生成优化的、健壮的代码。

目前，有 9 种目标语言支持不同的用例：

Name|Output type|Main usages
:-:|:-:|:-:
JavaScript|Sourcecode|Browser, Desktop, Mobile, Server
Neko|Bytecode|Desktop, Server, CLI
HashLink|Bytecode|Desktop, Mobile, Game consoles
PHP|Sourcecode|Server
Python|Sourcecode|Desktop, Server
Lua|Sourcecode|Desktop, Scripting
C++|Sourcecode|Desktop, Mobile, Server, Game consoles
Flash|Bytecode|Desktop, Mobile
Java|Sourcecode|Desktop, Mobile, Server
JVM|Bytecode|Desktop, Mobile, Server
C#|Sourcecode|Desktop, Mobile, Server

[类型](haxe:types) 介绍了 Haxe 中的 7 种不同类型以及它们之间的互动方式。[类型系统](haxe:type-system) 中继续讨论类型，其中解释了**unification**、**type parameters**和**type parameters**等特性。

[](haxe:class-field) 是关于 Haxe 类的结构的，其中涉及**属性**、**内联字段**（inline fields）和**泛型函数**（generic functions）。

在 [](haxe:expression) 中，可以看到如何通过使用**表达式**让程序做一些事情。

[](haxe:language-features) 通过详细描述一些特性（如模式匹配、字符串插值和死代码消除（dead code elimination））总结了 Haxe 语言参考。

我们继续学习 Haxe 编译器参考，在[编译器特性](haxe:compiler-features)中进入高级特性之前，它首先处理 [编译器使用](haxe:compiler-usage) 中的基础知识。最后，我们将在宏中冒险进入令人兴奋的 Haxe [](haxe:macro) 领域，看看如何极大地简化一些常见任务。

在 [标准库](haxe:std) 中，我们将探索来自 Haxe 标准库的重要类型和概念。然后我们学习 Haxe 的包管理器在 [](haxe:haxelib)。

Haxe 去掉了许多目标差异，但有时与目标直接交互很重要，这是[目标细节](haxe:target-details)的主题。

## Hello World

Haxe 编程语言的设计既简单又强大。它的语法很大程度上遵循 ECMAScript 标准，但在必要时有所偏离。下面的程序演示了用 Haxe 编写的“Hello World”：

```haxe
/**
	多行注释用于文档。
**/
class Main {
	static public function main():Void {
		// 单行注释
		trace("Hello World");
	}
}
```

安装了 Haxe 后，这个程序保存在一个名为“Hello.hx”的文件中，这可以从命令行编译，例如 JavaScript：

```sh
haxe --main Main --js Main.js
```

转到 JVM 字节码（一个 jar 文件）：

```sh
haxe --main Main --jvm Main.jar
```

或 `Main.hx` 可以直接由 Haxe 的内置解释器运行：

```sh
haxe --main Main --interp
```

大多数 Haxe 代码都是用**类**和**函数**来组织的，这使得 Haxe 成为一种面向对象的语言，让人想起了 Java、ActionScript 3 和 c#。然而，Haxe 在设计时就考虑到了表达能力，通过易于阅读的语法生成了一种强大的语言。

```{note}
- Haxe 程序保存在扩展名为 `.hx` 的文件中。
- Haxe 编译器是一个命令行工具，可以用 `--main Main` 和 `--interp` 等参数调用。
- Haxe 程序有类（`Main`，大写），类有函数(`main`，小写)。
- 包含 Haxe 类的文件的名称与类本身的名称相同（在本例中是 `Main.hx`）。
```
