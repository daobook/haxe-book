(haxe:target-details)=
# target-details

参考：https://haxe.org/manual/target-details.html

## 访问目标特定的 `Syntax`

在为特定目标编写代码时，可能需要访问仅在该目标上可用而在 Haxe 代码中不可访问的某些特性。仍然可以使用特定于目标的 `Syntax` api 来访问这些特性：

*   [`js.Syntax`](https://api.haxe.org/js/Syntax.html) - JavaScript syntax
*   [`php.Syntax`](https://api.haxe.org/php/Syntax.html) - PHP syntax
*   [`python.Syntax`](https://api.haxe.org/python/Syntax.html) - Python syntax

```{important}
在使用这些函数之前，请确保在 Haxe 标准库中没有可用的替代方法。Haxe 编译器无法验证生成的语法，这可能导致输出中出现无效或容易出错的代码。在检查标准库中是否有特定功能时，请参考 [API 文档](https://api.haxe.org/)。目标特定的 api 被放在一个以目标命名的包中，例如 JavaScript 特定的 api 在顶级 `js` 包中。大多数目标包中都有一个 `Lib` 类，它为该目标提供了一些通用方法。
```

[`Syntax.code(expr, args...)`](https://haxe.org/manual/target-syntax.html#syntax.code(expr,-args...))


注入原始代码表达式。该表达式是一个字符串，可以包括 `{0}`，`{1}`，`{2}` 等。这些将被提供给函数的相应参数替换。如果代码需要引用 Haxe 字段，这一点很重要，因为生成的输出中的变量名可能与 Haxe 代码中的变量名不对应。如果提供了字符串，Haxe 编译器将负责添加引号。函数也可以返回值，尽管返回类型总是 `Dynamic`。

`js.Syntax.code` 的一个例子：

```haxe
var myMessage = "Is Haxe great?";
var output:Bool = js.Syntax.code("confirm({0})", myMessage);
```

生成的 JavaScript 输出如下：

```js
var myMessage = "Is Haxe great?";
var output = confirm(myMessage);
```

其他函数在相应的 API 部分进行了说明。这些操作符通常包括在 Haxe 中不可用的本地操作符。

## HashLink

Haxe 支持编译到 HashLink 字节码或 HashLink/C 代码。[HashLink](https://hashlink.haxe.org/) 是为 Haxe 设计的虚拟机。

### Haxe/HashLink

要开始使用 Haxe/HashLink，创建一个新文件夹并将这个类保存为 `Main.hx`。

```haxe
/**
    Multi-line comments for documentation.
**/
class Main {
    static public function main():Void {
        // Single line comment
        trace("Hello World");
    }
}
```

要编译，可以从命令行运行以下命令：

```sh
要编译，可以从命令行运行以下命令:
```

另一种可能是创建并运行（双击）一个名为 `compile.hxml` 的文件。在这个例子中，hxml 文件应该和示例类在同一个目录中。

```
--hl hello.hl
--main Main
```

编译器输出一个名为 `hello.hl` 的文件，可以用 `hl hello.hl` 执行。

### HashLink/C 编译

Haxe 支持 HashLink 的两种编译模式：

- 编译为 HashLink 字节码。这种模式的编译速度非常快，因此适合日常开发。
- 编译为 HashLink/C 代码，用本机编译器编译为常规可执行文件。这种模式可以获得最佳的性能，因此它适合于最终版本。

#### HashLink 字节码

编译为 HashLink 字节码是通过在 `--hl` 参数中指定 `.hl` 输出来实现的：

```sh
haxe --main Main --hl output.hl
```

这将产生单个 `output.hl` 它可以在 HashLink JIT 虚拟机中执行：

```sh
hl output.hl
```

#### HashLink/C 代码
通过在 `--hl` 参数中指定 `.c` 输出，Haxe 代码也可以编译为 HashLink/C 代码：

```sh
haxe --main Main --hl out/main.c
```

这将在 `out` 目录中生成文件，可以使用 `gcc` 等 C 编译器将其编译为本地可执行文件。

```sh
gcc -O3 -o hello -std=c++ -I out out/main.c -lhl [-L/path/to/required/hdll]
```

注意，在本机编译期间，必须链接 HL 库。这个库在 HashLink 的二进制发行版中可用，也可以在编译 [HashLink 的源代码](https://github.com/HaxeFoundation/hashlink) 时获得。

## Python

参考：https://haxe.org/manual/target-python-getting-started.html

要开始使用 Haxe/Python，创建一个新文件夹，并将这个类保存为 `Main.hx`。

```haxe
/**
    Multi-line comments for documentation.
**/
class Main {
    static public function main():Void {
        // Single line comment
        trace("Hello World");
    }
}
```

要编译，可以从命令行运行以下命令：

```sh
haxe --python hello.py --main Main
```

另一种可能是创建并运行（双击）一个名为 `compile.hxml` 的文件。在这个例子中，hxml 文件应该和示例类在同一个目录中。

```
--python hello.py
--main Main
```

编译器在当前文件夹中输出一个名为 `hello.py` 的文件，可以通过命令行使用 `python hello.py` 执行。

## Cppia

参考：https://haxe.org/manual/target-cppia.html

Cppia 是 Haxe 的一个可编写脚本的 cpp 子目标。cppia 脚本是一种指令汇编脚本，可以在 cppia 主机中运行，并在编译时间接近于零的情况下提供运行时速度。它还允许您向主机添加关键性能代码，从而为这些部件提供完整的 cpp 运行时速度。

```{seealso}
*   [Cppia API documentation](https://api.haxe.org/cpp/cppia/index.html)
*   [Cppia tutorial](https://code.haxe.org/category/other/working-with-cppia/index.html)
```

## C++

参考：https://haxe.org/manual/target-cpp.html

### 快速上手 Haxe/C++

C++ 目标使用各种 C++ 编译器（假定已经安装在系统上）来创建本地可执行文件或库。编译分两个阶段进行。首先，Haxe 编译器在输出目录中生成源文件、头文件和构建文件。其次，调用 `hxcpp` [Haxelib 库](https://haxe.org/manual/haxelib.html) 来运行生成最终结果所需的系统编译器和连接器。

#### 准备

在使用 C++ 目标之前，您需要安装：

- `hxcpp`：使用 `haxelib install hxcpp` 安装
- 一种 C++ 编译器，如 `g++`，`clang++`，或 `msvc++`。

#### 系统编译器

系统编译器支持三种主要操作系统——Mac、Linux 和 Windows。

在 Mac 上，建议你从 Mac 应用商店安装最新的 Xcode。

在 Linux 操作系统下，建议使用系统包管理器安装编译器。

在 Windows 环境下，推荐使用 Microsoft Visual Studio。在 Windows 上，您也可以使用基于 `gcc` 的编译器。Haxelib 库中包含一个最小的发行版，可以使用它进行安装 `haxelib install minimingw`。

#### 交叉编译器

如果你安装了一个合适的交叉编译器，`hxcpp` 可以用来编译非主机架构。编译器通常以软件开发工具包（SDK）的形式提供，或者在 iOS 设备中，附带系统编译器（Xcode）。选择使用哪个编译器是通过在 `hxcpp` 构建环境中定义特定变量来实现的。请注意， `hxcpp` 构建工具只负责生成本地可执行文件或本地库（静态或动态），而不是通常需要的资产和元数据的完整绑定和打包。

### Hxcpp 构建环境

`hxcpp` 构建环境可以通过在键值对中设置或定义变量来控制。定义一个没有特定值的键通常足以触发所需的行为，这些键通常被称为“定义”。

有关标准定义的列表，请参阅 [定义](https://haxe.org/manual/target-cpp-defines.html)。

#### 从命令行定义

更改 `hxcpp` 构建环境的最简单方法是使用 `-D` 将定义通过 Haxe 命令行传递。键值对也可以通过这种方式传递，例如：

```sh
haxe --main Main --cpp cpp -D android -D static_link -D PLATFORM=android-9
```

这里：

- `android`：导致 `hxcpp` 交叉编译到 Android。
- `static_link`：导致创建静态（而不是动态）库。
- android 目标平台（`PLATFORM`）被设置为一个特定的值（`android-9`）。这个平台信息被传递给SDK，以便它可以生成适当的代码。

高级用户可以在编译时使用宏向系统添加额外的定义。这些定义也将传递给 `hxcpp` 构建工具。

#### 从系统环境变量进行定义

`hxcpp` 构建工具将导入所有的系统环境变量，所以你可以使用系统配置流程，如：

```sh
export HXCPP_VERBOSE=
```

或者，在 Windows 上：

```sh
set HXCPP_VERBOSE=1
```

如果您通过 IDE 运行 Haxe，那么必须小心处理环境变量，因为这些变量可能是在 IDE 启动时从环境中读取的，而不是在系统上的变量发生变化时进行更改。

#### 从 `.hxcpp_config.xml` 定义

The `hxcpp` build tool parses several "build files". These files are in a basic XML file format and can be used to set, or conditionally set, configuration variables. As part of the build process, the `.hxcpp_config.xml` file, known as the configuration file, will be read (twice). This file is located in the user's home directory (or user's profile directory on Windows) and is the best place to configure variables that are specific to the system that rarely change, such as the location of cross-compiler SDKs. A placeholder file will be generated the first time `hxcpp` is run by a user on the machine. You can see the exact location of this file in the build log if you compile with the `HXCPP_VERBOSE` define.

The configuration file is read twice. The first time the `vars` section is read early in the configuration process, and is used to set up and configure the location of compilers. The second time, the `exes` section is read near the end of the process and allows modification of the compiler or build process based on all the available information. See [`build.xml` file format](https://haxe.org/manual/target-cpp-file-format.html) for details on the file format.

#### [Defining via `@:buildXml` Metadata](https://haxe.org/manual/target-cpp-build-environment.html#defining-via-@buildxml-metadata)

Configuration data can be injected into the `build.xml` file that is created by Haxe. This is done attaching metadata to a Haxe class that is directly or indirectly included in the build. For example:

```xml
@:buildXml("
<target id='haxe'>
  <lib name='${haxelib:nme}/lib/${BINDIR}/libnme${LIBEXTRA}${LIBEXT}'/>
</target>
<include name='${haxelib:nme}/lib/NmeLink.xml'/>
")
@:keep class StaticNme { ... }
```

This metadata is best for adding libraries or include paths to the build. Some notes:

*   The `@:keep` metadata is added to prevent [dead code elimination](https://haxe.org/manual/cr-dce.html) from removing this class.
*   Quoting can be a bit tricky - here the double-quotes are used for Haxe, and the single quotes are added to the `build.xml`.
*   Injecting a single "include" command is a good way to manage the quoting issue.
*   Knowledge of the build system is required to get this right. See [`build.xml` file format](https://haxe.org/manual/target-cpp-file-format.html) for details on the file format.
*   The `build.xml` file is read after the choice of compiler has been made, so it is generally not suitable for configuring the compiler.

### Build.xml

参考：https://haxe.org/manual/target-cpp-file-format.html

### 定义

参考：https://haxe.org/manual/target-cpp-defines.html

### The Hxcpp Cache

参考：https://haxe.org/manual/target-cpp-CompileCache.html

### Threads And Stacks

参考：https://haxe.org/manual/target-cpp-ThreadsAndStacks.html

## JavaScript

参考：https://haxe.org/manual/target-javascript.html