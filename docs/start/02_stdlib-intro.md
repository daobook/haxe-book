(haxe:stdlib-intro)=
# Haxe 标准库简介

参考：https://haxe.org/documentation/introduction/stdlib-introduction.html

Haxe 标准库提供了通用的工具，而不是试图成为数据结构和算法的详尽集合。Haxe 发行版附带一个包含 Haxe 标准库的 std 目录。它的内容可以这样分类：

- [一般用途](https://haxe.org/documentation/introduction/stdlib-introduction.html#general-purpose-api)：`std` 目录本身包含一些顶级类，如 `Array`、`Map` 或 `String`，它们可以用于所有目标。`haxe` 子目录提供额外的数据结构、输入/输出 api 和更多工具。
- [系统](https://haxe.org/documentation/introduction/stdlib-introduction.html#sys-api)：子目录 `sys` 包含与文件系统和数据库相关的 api。此外，`Sys` 顶级类允许与操作系统进行各种交互。它们只能在编译到 `sys` 类的目标（C++， C#， Java, Neko, PHP）时被访问。
- [特定于目标](https://haxe.org/documentation/introduction/stdlib-introduction.html#target-apis)：每个 Haxe 目标有一个不同的子目录，其中包含特定于目标的 api。只有在编译到给定目标时才能访问它们。

## 一般用途

对所有目标可用。

````{panels}
:container: +full-width text-center
:header: w3-pale-blue w3-wide w3-card-4 
:column: col-lg-12 px-2 py-2
:body: text-justify w3-padding
:card: w3-pale-red w3-card
---
最高阶层的：
^^^
*   [Array](https://api.haxe.org/Array.html)：类型化集合，它定义了几个符合 ECMA 的操作
*   [Date, DateTools](https://api.haxe.org/Date.html)：与日期和时间戳相关的操作
*   [EReg](https://api.haxe.org/EReg.html)：正则表达式
*   [Lambda](https://api.haxe.org/Lambda.html): Operations over iterables
*   [Map](https://api.haxe.org/Map.html): Key-to-value mapping data structure
*   [Math](https://api.haxe.org/Math.html): ECMA-compliant mathematical functions
*   [Reflect](https://api.haxe.org/Reflect.html): Field-related reflection
*   [Std](https://api.haxe.org/Std.html): Runtime type-checking; numerical parsing; conversion to Int and String
*   [String](https://api.haxe.org/String.html): Basic operations on String
*   [StringBuf](https://api.haxe.org/StringBuf.html): Optimized for building Strings
*   [StringTools](https://api.haxe.org/StringTools.html): Various extensions to String
*   [Type](https://api.haxe.org/Type.html): Type-related reflection
*   [Xml](https://api.haxe.org/Xml.html): Cross-platform XML
---
`haxe` 包：
^^^
*   [haxe.Http](https://api.haxe.org/haxe/Http.html): HTTP requests
*   [haxe.Json](https://api.haxe.org/haxe/Json.html): Encoding and decoding JSON
*   [haxe.Resource](https://api.haxe.org/haxe/Resource.html): Work with Haxe resources
*   [haxe.Serializer](https://api.haxe.org/haxe/Serializer.html): Serialize arbitrary objects as String
*   [haxe.Template](https://api.haxe.org/haxe/Template.html): Simple templating system
*   [haxe.Timer](https://api.haxe.org/haxe/Timer.html): Repeated/delayed execution; measuring
*   [haxe.Unserializer](https://api.haxe.org/haxe/Unserializer.html): Complement to haxe.Serializer
*   [haxe.UnicodeString](https://api.haxe.org/UnicodeString.html): Cross-platform unicode strings
*   [haxe.crypto](https://api.haxe.org/haxe/crypto/): Various encryption algorithms
*   [haxe.macro](https://api.haxe.org/haxe/macro/): Types for working with Haxe macros
*   [haxe.MainLoop](https://api.haxe.org/haxe/MainLoop.html): MainLoop for Haxe
*   [haxe.rtti](https://api.haxe.org/haxe/rtti/): Run-time type information
*   [haxe.xml](https://api.haxe.org/haxe/xml/): Complementary XML tools
*   [haxe.zip](https://api.haxe.org/haxe/zip/): Support of the ZIP-format
---
`haxe.ds` 包：
^^^
*   [haxe.ds.ArraySort](https://api.haxe.org/haxe/ds/ArraySort.html): Stable, cross-platform array sorting
*   [haxe.ds.BalancedTree](https://api.haxe.org/haxe/ds/BalancedTree.html): Balanced tree data structure
*   [haxe.ds.EnumValueMap](https://api.haxe.org/haxe/ds/EnumValueMap.html): Map type supporting enum value keys
*   [haxe.ds.GenericStack](https://api.haxe.org/haxe/ds/GenericStack.html): Stack data structure which is optimized on static targets
*   [haxe.ds.IntMap](https://api.haxe.org/haxe/ds/IntMap.html): Map type supporting Int keys
*   [haxe.ds.ObjectMap](https://api.haxe.org/haxe/ds/ObjectMap.html): Map type supporting object keys
*   [haxe.ds.StringMap](https://api.haxe.org/haxe/ds/StringMap.html): Map type supporting string keys
*   [haxe.ds.Vector](https://api.haxe.org/haxe/ds/Vector.html): Fixed-size data structure
---
`haxe.io` 包：
^^^
*   [haxe.io.Bytes](https://api.haxe.org/haxe/io/Bytes.html): Byte operations on native representations
*   [haxe.io.BytesBuffer](https://api.haxe.org/haxe/io/BytesData.html): Optimized for building Bytes
*   [haxe.io.Path](https://api.haxe.org/haxe/io/Path.html): Operations on path strings
````

## 系统 API

可在 C++，C#， Java, Neko 和 PHP。

*   [Sys](https://api.haxe.org/Sys.html): Execute native commands; interact with stdin, stdout and stderr; various other native operations
*   [sys.FileSystem](https://api.haxe.org/sys/FileSystem.html): Read and modify directories; obtain information on files and directories
*   [sys.db](https://api.haxe.org/sys/db/): APIs for working with MySQL and SQLite databases
*   [sys.io.File](https://api.haxe.org/sys/io/File.html): Read and write file content; copy files
*   [sys.io.Process](https://api.haxe.org/sys/io/Process.html): Use native processes
*   [sys.thread](https://api.haxe.org/sys/thread/): API for multi-threaded applications.

## 目标特定 API

*   [cpp](https://api.haxe.org/cpp/):

    *   [cpp.Lib](https://api.haxe.org/cpp/Lib.html): Provides platform-specific functions for the C++ target.
    *   [cpp.vm](https://api.haxe.org/cpp/vm/): Thread API, debugger, profiler etc.
*   [js](https://api.haxe.org/js/):

    *   [js.Lib](https://api.haxe.org/js/Lib.html): Provides some platform-specific functions for the JavaScript target.
    *   [js.Browser](https://api.haxe.org/js/Browser.html): Shortcuts for common browser functions.
    *   [js.html](https://api.haxe.org/js/html/): Externs for interacting with the native JavaScript API's / DOM.
    *   [js.html](https://api.haxe.org/js/Syntax.html): Helper for low-level JavaScript specific code generation.
*   [php](https://api.haxe.org/php/):

    *   [php.Lib](https://api.haxe.org/php/Lib.html): Provides platform-specific functions for the PHP target.
    *   [php.Syntax](https://api.haxe.org/php/Syntax.html): Helper for low-level PHP specific code generation.
    *   [php.Session](https://api.haxe.org/php/Session.html): Work with native PHP sessions
    *   [php.Web](https://api.haxe.org/php/Web.html): Work with HTTP requests and responses
    *   [php.db.PDO](https://api.haxe.org/php/db/PDO.html): Additional PDO driver for database interactions
*   [cs](https://api.haxe.org/cs/): API for C# target

    *   [cs.Lib](https://api.haxe.org/cs/Lib.html): Provides platform-specific functions for the C# target.
    *   [cs.system](https://api.haxe.org/cs/system/): Externs for interacting with native C# classes.
*   [java](https://api.haxe.org/java/): API for Java target

    *   [java.Lib](https://api.haxe.org/java/Lib.html) Provides platform-specific functions for the Java target.
    *   [java.*](https://api.haxe.org/java/) Externs for interacting with native Java classes.
*   [python](https://api.haxe.org/python/):

    *   [python.Lib](https://api.haxe.org/python/Lib.html): Provides platform-specific functions for the Python target.
    *   [python.Syntax](https://api.haxe.org/python/Syntax.html): Helper for low-level Python specific code generation.
    *   [python.Syntax](https://api.haxe.org/python/lib/): Externs for interacting with native Python classes.
*   [flash](https://api.haxe.org/flash/):

    *   [flash.Lib](https://api.haxe.org/flash/Lib.html) Provides platform-specific functions for the Flash target.
    *   [flash.Memory](https://api.haxe.org/flash/Memory.html) Extern for Flash Memory API
    *   [flash.Vector](https://api.haxe.org/flash/Vector.html) Extern for Flash Vectors
*   [neko](https://api.haxe.org/neko/):

    *   [neko.Lib](https://api.haxe.org/neko/Lib.html): Low level interactions with the Neko VM target.
    *   [neko.Web](https://api.haxe.org/neko/Web.html): Work with HTTP requests and responses.
    *   [neko.net](https://api.haxe.org/neko/net/): Tools for interacting with networks and running servers.
    *   [neko.vm](https://api.haxe.org/neko/vm/): API for multi-threaded applications.
*   [hl](https://api.haxe.org/hl/): Low level interactions with the HashLink target

*   [lua](https://api.haxe.org/lua/): Low level interactions with the lua target
