(haxe:compiler-usage)=
# compiler-usage

参考：https://haxe.org/manual/compiler-usage.html

## 基本用法

Haxe 编译器通常从命令行调用，有几个参数必须回答两个问题：

- 应该编译什么?
- 输出应该是什么?

要回答第一个问题，通常通过 `-p <path>` 参数提供一个 class 路径，以及通过 `-m <dot_path>` 参数提供要编译的 main class 就足够了。然后，Haxe 编译器解析 main class 文件并开始编译。

第二个问题通常归结为提供指定所需目标的参数。每个 Haxe 目标都有一个专用的命令行开关，比如 `--js <file_name>` 用于 JavaScript，`--php <directory>` 用于 PHP。根据目标的性质，参数值要么是目录路径（对于 `--php`、`--cpp`、`--cs` 和 `--java`），要么是文件名。

### 常见的参数

````{panels}
:container: +full-width text-center
:header: w3-pale-blue w3-wide w3-card-4 
:column: col-lg-6 px-2 py-2
:body: text-justify w3-padding
:card: w3-pale-red w3-card
---
输入
^^^
*   `-p <path>`（或 `--class-path <path>`）添加一个可以找到 `.hx` 源文件或包（子目录）的类路径。
*   `-L <library_name>`（或 `--library <library_name>`）添加一个 [Haxelib](https://haxe.org/manual/haxelib.html) 库。默认情况下，使用本地 Haxelib 存储库中的最新版本。要求使用特定的库版本 `-L library_name:version`。要想从 git 获取一个版本，可以使用 `-L library_name:git:https://github.com/user/project.git#commit`，其中可选的 `#commit` 可以是一个分支、标签或 commit 哈希。
*   `-m <dot_path>`（或 `--main <dot_path>`） 设置 main class。
*   `-D <var[=value]>`（或 `--define <var[=value]>`）定义 [条件编译标志](https://haxe.org/manual/lf-condition-compilation.html)。
---
输出
^^^

*   `--js <file_name.js>` 在指定的文件中生成 [JavaScript](https://haxe.org/manual/target-javascript.html) 源代码。
*   `--swf <file_name.swf>` 生成指定的文件为 [Flash](https://haxe.org/manual/target-flash.html) `.swf`。
*   `--neko <file_name.n>` 生成指定文件的 [Neko](https://haxe.org/manual/target-neko.html) 二进制文件。
*   `--php <directory>` 在指定的目录中生成 [PHP](https://haxe.org/manual/target-php.html) 源代码。php7 源代码使用 `-D php7`。
*   `--cpp <directory>` 在指定的目录中生成 [C++](https://haxe.org/manual/target-cpp.html) 源代码，并使用本机 C++ 编译器进行编译。
*   `--cs <directory>` 在指定的目录中生成 [C#](https://haxe.org/manual/target-cs.html) 源代码，并使用本机 C# 编译器进行编译。
*   `--java <directory>` 在指定目录中生成 [Java](https://haxe.org/manual/target-java.html) 源代码，并使用 Java Compiler 进行编译。添加 `-D jvm`，直接绕过 Java 编译步骤生成 jvm 字节码。
*   `--jvm <file_name.jar>` 以 jar 文件的形式生成 [JVM 字节码](https://haxe.org/manual/target-jvm.html)。
*   `--python <file_name.py>` 在指定的文件中生成 [Python](https://haxe.org/manual/target-python.html) 源代码。
*   `--lua <file_name.lua>` 在指定的文件中生成 [Lua](https://haxe.org/manual/target-lua.html) 源代码。
*   `--hl <file_name.hl>` 在指定的文件中生成 [HashLink](https://haxe.org/manual/target-hl.html) 字节码。
*   `--cppia <file_name.cppia>` 生成指定的脚本作为 [cppia](https://haxe.org/manual/target-cppia.html) 文件。
*   `-x <file>` 编译和执行 Neko 文件的快捷方式。
*   `--no-output` 编译但不生成任何文件。
*   `--interp` 使用内部宏系统解释程序。
````

### 其他全局参数

*   `--run <module> [args...]` Compile and execute a Haxe module with command line arguments.
*   `--xml <file>` Generate XML types description. Useful for API documentation generation tools like [Dox](https://github.com/HaxeFoundation/dox).
*   `-v` (or `--verbose`) Turn on verbose mode.
*   `--dce <std|full|no>` Set the [Dead Code Elimination](https://haxe.org/manual/cr-dce.html) mode (default `std`).
*   `--debug` Add debug information to the compiled code.
*   `-r <file>[@name]` (or `--resource <file>[@name]`) Add a named resource file.
*   `--prompt` Prompt on error.
*   `--cmd <command>` Run the specified shell command after a successful compilation.
*   `--no-traces` Don't compile trace calls in the program.
*   `--display` Display code tips to provide [completion information for IDEs and editors](https://haxe.org/manual/cr-completion-overview.html).
*   `--times` Measure compilation times.
*   `--no-inline` Disable [inlining](https://haxe.org/manual/class-field-inline.html).
*   `--no-opt` Disable code optimizations.
*   `--remap <package:target>` Remap a package to another one.
*   `--macro` Call the given [initialization macro](https://haxe.org/manual/macro-initialization.html) before typing anything else.
*   `--wait <host:port>` Wait on the given port for commands to run (see [Completion server](https://haxe.org/manual/cr-completion-server.html)).
*   `--connect <host:port>` Connect on the given port and run commands there (see [Completion server](https://haxe.org/manual/cr-completion-server.html)).
*   `-C <dir>` (or `--cwd <dir>`) Set current working directory.

### 目标的具体参数

- `--php-front <filename>` Select the name for the php front file.
- `--php-lib <filename>` Select the name for the php lib folder.
- `--php-prefix <name>` Prefix all classes with given name.
- `--swf-version <version>` Change the SWF version.
- `--swf-header <header>` Define SWF header (width:height:fps:color).
- `--swf-lib <file>` Add the SWF library to the compiled SWF.
- `--swf-lib-extern <file>` Use the SWF library for type checking.
- `--flash-strict` More type strict flash API.
- `--java-lib <file>` Add an external JAR or class directory library.
- `--net-lib <file>[@std]` Add an external .NET DLL file.
- `--net-std <file>` Add a root std .NET DLL search path.
- `--c-arg <arg>` Pass option arg to the native Java/C# compiler.

```{admonition} 编译后运行命令
编译成功后使用 `--cmd` 运行指定的命令。它可以用来运行（测试）工具或直接运行构建，例如 `--cmd java -jar bin/Main.jar`（适用于 Java)，`--cmd node main.js`（适用于 Node.js）或 `--cmd neko Main.n`（适用于 Neko）等。
```

### 全局编译器配置宏

为了包含单个模块，它们的路径可以直接在命令行或 `haxe ... ModuleName pack.ModuleName`。对于更具体的包含或不包含，请使用以下[初始化宏](https://haxe.org/manual/macro-initialization.html)：

- `--macro include(pack:String, recursive=true, ?ignore:Array<String>, ?classPaths:Array<String>, strict=false)` 在编译中包含包中的所有模块。如果 `recursive` 为 `true`，编译器递归地排除所有子包。
- `--macro exclude(pack:String, recursive=true` 不生成包中的特定类、枚举或所有类和枚举。被排除的类型变成 `extern`。如果 `recursive` 为 `true`，编译器递归地排除所有子包。
- `--macro excludeFile(fileName:String)` 不生成从给定的外部文件（每行一个）中列出的类和枚举。
- `--macro keep(?path:String, ?paths:Array<String>, recursive=true)` 标记要由 DCE 保存的包、模块或子类型点路径。这也扩展到已解析模块的子类型。如果 `recursive` 为 `true`，编译器递归地保留包路径的所有子包。
- `--macro includeFile(file:String, position)` 在编译时嵌入一个 JavaScript 文件。position 可以是 "top"，"inline" 或 "closure"。

```{seealso}
在 Haxe Code Cookbook 中[编译教程](http://code.haxe.org/category/compilation/)。
```

## HXML

编译器参数可以存储在 `.hxml` 文件中，并且可以用 `haxe <file.hxml>` 执行。在 hxml 中，可以使用换行符和注释，这使得维护 Haxe 构建配置更加容易。可以在 hxml 文件之后提供更多的参数，例如 `haxe build.hxml --debug`。

````{panels}
:container: +full-width text-center
:header: w3-pale-blue w3-wide w3-card-4 
:column: col-lg-12 px-2 py-2
:body: text-justify w3-padding
:card: w3-pale-red w3-card
---
例子
^^^
这个例子有一个配置，它将类文件 `website.HomePage.hx` 编译成 JavaScript 到一个名为 `bin/homepage.js` 的文件中，这个文件位于 `src` 类路径中。并使用完全死代码消除。

```
--class-path src
--dce full
--js bin/homepage.js
--main website.HomePage
```
````

### 多个构建编译

Hxml 配置允许使用以下参数进行多次编译：

- `--next` 分离几个 Haxe 编译。
- `--each` 将前面的参数附加到所有 haxe 编译，由 `--next` 分隔。这减少了重复参数。

````{panels}
:container: +full-width text-center
:header: w3-pale-blue w3-wide w3-card-4 
:column: col-lg-12 px-2 py-2
:body: text-justify w3-padding
:card: w3-pale-red w3-card
---
---
例子
^^^
这个例子有一个配置，它将三个不同的类编译成它们自己的 JavaScript 文件。每次构建都使用 `src` 作为类路径，并使用完全失效代码消除。

```
--class-path src
--dce full

--each

--js bin/homepage.js
--main website.HomePage

--next

--js bin/gallery.js
--main website.GalleryPage

--next

--js bin/contact.js
--main website.ContactPage
```
````

### hxml 注释

在 `.hxml` 文件中使用哈希值（例如 `#`）注释掉该行的其余部分。

在 HXML 中调用构建配置：

可以创建如下的配置：

```
build-server.hxml
--next
build-website.hxml
--next
build-game.hxml
```

## 全局编译器标志

参考：https://haxe.org/manual/compiler-usage-flags.html