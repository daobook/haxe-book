# Python & Haxe

参考：[Getting started with Haxe for Python](https://haxe.org/documentation/platforms/python.html)

- 下载并安装 [Haxe 编译器](https://haxe.org/download)。
- 在 {term}`Visual Studio Code` 安装兼容 Haxe 的 IDE：[Haxe Extension Pack](https://marketplace.visualstudio.com/items?itemName=vshaxe.haxe-extension-pack)

## 创建您的项目

为你的 Haxe 项目创建一个新目录，结构如下：

```sh
project/
├── src/
│   └── Main.hx
│
├── bin/
│   └── python/
│
└── build.hxml
```

将以下代码插入 `src/Main.hx` 文件：

```haxe
class Main {
	static function main() {
		Sys.println("Haxe is great!");
	}
}
```

将以下构建配置添加到 `build.hxml` 文件：

```sh
-cp src
-main Main
-python bin/python/main.py
```

上面的配置指定了你的源代码存储在`src/`，你的主 Haxe 文件是 `src/Main.hx`，你希望 Haxe 输出 Python 源代码到 `bin/python/`。

## 安装 Python 运行时

Haxe 需要 Python 3.x。由于安全修复和新特性，建议使用 Python 3.7 或更高版本。

## 开发项目

Haxe API
:   浏览 [Haxe API 网站](https://api.haxe.org/)，查找和使用核心的 Haxe API。除了核心的 Haxe API 之外，您还可以使用[特定于 Python 的 API](https://api.haxe.org/python/)。

Haxe 库
:   浏览 [haxelib](https://lib.haxe.org/t/python/) 网站，找到可以添加到项目中的社区开发的库。可以通过运行 `haxelib install <library>` 来全局安装 haxelib 库。也可以通过在 `.hxml` 文件中添加 `-lib <library>` 来指定你的项目使用 haxelib。

## 编译并运行项目

编译
:   通过在项目目录的控制台运行 `haxe build.hxml` 来构建 Haxe 项目。这将把 `.hx` 源代码编译成 Python 脚本，然后你可以运行和调试这些脚本。

调试和运行
:   用 Haxe 编译项目后，需要在 `bin/python` 目录下打开一个控制台，运行命令 `python main.py`。也可以使用如下所示的成熟的 Python IDE 来运行和调试程序。

* [PyScripter](https://sourceforge.net/projects/pyscripter/)
* [Visual Studio 2017 Community Edition](https://visualstudio.microsoft.com/downloads/)
* [Visual Studio Code](https://visualstudio.microsoft.com/downloads/)


## 问社区

如果您需要任何帮助，可以访问活跃的 [Haxe 社区](https://haxe.org/community/community-support.html)，并简单地请求帮助，了解您项目的细节。我们有一个帮助和活跃的社区，你应该很快得到你的答案!