(haxe:types)=
# 类型

Haxe 编译器使用了一个丰富的类型系统，它有助于在编译时检测程序中与类型相关的错误。类型错误是对给定类型的无效操作，例如除以 String、试图访问 Integer 的字段或调用参数太少（或太多）的函数。

在某些语言中，这种额外的安全性是有代价的，因为程序员被迫显式地将类型分配给语法结构：

```haxe
var myButton:MySpecialButton = new MySpecialButton(); // As3
```

```C++
MySpecialButton* myButton = new MySpecialButton(); // C++ 
```

在 Haxe 中不需要显式类型注释，因为编译器可以推断出类型：

```haxe
var myButton = new MySpecialButton(); // Haxe
```

我们将在后面的[类型推断](haxe:type-system/type-inference)中详细探讨类型推断。现在，只要知道上面代码中的变量 `myButton` 是类 `MySpecialButton` 的一个实例就足够了。

Haxe 类型系统有 7 种类型组：

- 类实例：给定类或接口（interface）的对象
- 枚举实例：Haxe 枚举的值
- 结构：一个匿名结构，即命名字段的集合
- 函数：由多个参数和一个返回值组成的复合类型
- 动态：通配符类型，与任何其他类型兼容
- 抽象：编译时类型在运行时由不同的类型表示
- Monomorph：一种未知的类型，后来可能变成另一种类型

复合类型
:   复合类型是具有子类型的类型。这包括任何带有 [](haxe:type-system/type-parameters) 的类型和[函数类型](haxe:types/function)。

(haxe:types/basic-types)=
## 基本类型

基本类型是 `Bool`, `Float` 和 `Int`。在语法中可以很容易地通过诸如

- `Bool` 值为 `true` 和 `false`
- `Int` 可以是 `1`、`0`、`-1` 和 `0xFF0000`
- `Float` 可以是 `1.0`、`0.0`、`-1.0` 和 `1e10`

在 Haxe 中基本类型并不是 [](haxe:types/class-instance)。相反，它们被实现为 [](haxe:types/abstract)，并绑定到编译器的内部操作符处理，如下面的部分所述。

### 数值类型

`Float`
:   表示双精度 IEEE 64 位浮点数。

`Int`
:   表示整数。

虽然每个 `Int` 都可以用在需要 `Float` 的地方（也就是说，`Int` 可以赋值给 `Float` 或 `Float` 联合体），反之则不成立：将 `Float` 赋值给 `Int` 可能会导致精度损失，因此是不允许隐式赋值。

### 溢出

出于性能原因，Haxe 编译器不强制执行任何溢出行为。检查溢出的重担落在目标平台上。下面是一些平台特定的溢出行为注意事项：

- C++, Java, C#, Neko, Flash：32 位有符号整数通常可能溢出
- PHP, JS, Flash 8：没有原生 Int 类型，如果数字达到浮点数限制（$2^52$），将会丢失精度。

另外,`haxe.Int32` 和 `haxe.Int64` 类可用于确保正确的溢出行为，但代价是在某些平台上进行额外的计算。

### `Bool`

`Bool`
:   表示一个可以为 `true` 或 `false` 的值。

`Bool` 类型的值经常出现在 [`if`](haxe:if) 和 [`while`](haxe:while) 等条件中。

### `Void`

`Void`
:   表示没有类型。它用来表示某物（通常是一个函数）没有值。

`Void` 是类型系统中的一种特殊情况，因为它实际上不是一种类型。它用于表示没有类型，这主要适用于函数参数和返回类型。比如下面的 `main` 函数的类型是 `Void->Void`，意思是“它没有参数，也不返回任何内容”。：

```haxe
class Main {
    static public function main():Void {
        trace("Hello World");
    }
}
```

Haxe 不允许 `Void` 类型的字段和变量，如果这样声明的话会报错：

```haxe
// 不允许使用 Void 类型的参数和变量
var x:Void;
```

### 为空性

可为空的
:   在 Haxe 中，如果一个类型的有效值为 `null`，那么该类型就被认为是空的（**nullable**）。

对于编程语言来说，对可空性有一个明确的定义是很常见的。然而，由于 Haxe 的目标语言的本质，它不得不在这方面找到一个妥协方案；虽然它们中的一些允许（事实上，默认为任何东西都为 `null`），但其他一些甚至不允许对某些类型为 `null`。这就需要区分两种目标语言：

静态目标
:   静态目标使用它们自己的类型系统，其中 `null` 不是基本类型的有效值。这对于 Flash、c++、Java 和 C# 目标来说是正确的。

动态目标
:   动态目标的类型比较宽松，基本类型允许 `null`。这适用于 JavaScript, PHP, Neko 和 Flash 6-8 目标。

当在动态目标上使用 `null` 时，没有什么需要担心的；然而，静态的可能需要一些思考。对于初学者来说，基本类型被初始化为默认值。

默认值
:   基本类型在静态目标上有以下缺省值：
    - `Int`：`0`
    - `Float`：Flash 是 `NaN`，其他静态目标是 `0.0`
    - `Bool`：`false`

因此，Haxe 编译器 不允许将 `null` 赋值给静态目标的基本类型。为了实现这一点，基本类型必须包装为 `Null<T>`：

```haxe
var a:Int = null; // 静态平台的错误
var b:Null<Int> = null; // 允许
```

类似地，基本类型不能与 `null` 比较，除非包装：

```js
var a : Int = 0;
if( a == null ) { ... } // 静态平台的错误
var b : Null<Int> = 0;
if( b != null ) { ... } // 允许
```

这种限制扩展到所有执行 [](haxe:type-system/unification) 的情况。

`Null<T>`
:   在静态目标上，类型 `Null<Int>`， `Null<Float>` 和 `Null<Bool>` 可以用来允许 `Null` 作为一个值。对于动态目标，这没有影响。`Null<T>` 也可以与其他类型一起使用，以证明 `null` 是一个允许的值。

如果 `null` 值在 `null<T>` 或 `Dynamic` 中被“隐藏”，并分配给一个基本类型，则使用默认值：

```haxe
var n : Null<Int> = null;
var a : Int = n;
trace(a); // 静态平台 0
```

### 可选参数和可空性

当考虑为空时，必须考虑可选参数；非空的**本机**可选参数和需要定义的特定于 haxe 的可选参数之间的分隔。这种区别是通过问号可选参数来实现的：

```haxe
// x 是一个原生 Int(非空)
function foo(x : Int = 0) {}
// y 是 Null<Int> (nullable)
function bar( ?y : Int) {}
// z 也是 Null<Int>
function opt( ?z : Int = -1) {}
```

```{note}
在其他一些编程语言中，参数（argument）和形参（parameter）是可以互换使用的。在 Haxe 中，参数引用方法时使用，参数引用 [](haxe:type-system/type-parameters) 时使用。
```

(haxe:types/class-instance)=
## 类实例

与许多面向对象的语言相似，类是 Haxe 中大多数程序的主要数据结构。每个 Haxe 类都有一个显式的名称、一个隐含的路径和零个或多个类字段。在这里，我们将把重点放在类的一般结构和它们的关系上，而把类字段的细节留给 [](haxe:class-field)。

下面的代码示例是本节其余部分的基础：

```haxe
class Point {
	var x:Int;
	var y:Int;

	public function new(x, y) {
		this.x = x;
		this.y = y;
	}

	public function toString() {
		return "Point(" + x + "," + y + ")";
	}
}
```

从语义上讲，此**类**表示离散的二维空间中的一个点——但在这里这并不重要。让我们来描述一下这个结构：

- 关键字 `class` 表示我们正在声明一个类。
- `Point` 是类的名称，可以是符合[类型标识符规则](https://haxe.org/manual/expression.html#define-identifier)的任何东西。
- 用大括号 `{}` 括起来的是类字段，它由两个 `Int` 类型的变量字段 `x` 和 `y` 组成，
- 后面是一个名为 `new` 的特殊函数字段，这是类的构造函数，
- 以及一个普通的函数 `toString`。

在 Haxe 中有一种特殊的类型是兼容所有类的：

`Class<T>`
:   此类型与所有类类型兼容，这意味着所有类都可以分配给它。但是，不能将类实例分配给此类型。在编译时，`Class<T>` 是所有类类型的公共基类型。这种关系没有反映在生成的代码中。当 API 要求一个值是一个类，而不是一个特定的值时，这种类型很有用。这适用于 [Haxe 反射 API](https://haxe.org/manual/std-reflection.html) 的几种方法。

### 类构造器

类的实例是通过调用类构造函数来创建的——这个过程通常称为**实例化**。类实例的另一个名称是 **object**。然而，我们更喜欢用类实例这个术语来强调类/类实例和 [enum/enum 实例](haxe:types/enum-instance) 之间的相似性。

```haxe
var p = new Point(-1, 65);
```

上面的代码将产生一个 `Point` 类的实例，该实例被赋值给一个名为 `p` 的变量。`Point` 的构造函数接收 `-1` 和 `65` 两个参数，并将它们分别赋值给实例变量 `x` 和 `y`（比较它在类实例中的定义）。我们将在 [new](haxe:expression/new) 一节稍后的内容中重新讨论这个新表达式的确切含义。现在，可以把它看作调用类构造函数并返回适当的对象。

### 继承

类可以从其他类继承；这由 `extends` 关键字表示：

```haxe
class Point3 extends Point {
  var z:Int;

  public function new(x, y, z) {
    super(x, y);
    this.z = z;
  }
}
```

这种关系通常被描述为“is-a”：类 `Point3` 的任何实例也是 `Point` 的实例。`Point` 被称为 `Point3` 的父类，而 `Point3` 是 `Point` 的子类。一个类可以有许多子类，但只有一个父类。术语“类X的父类”通常指它的直接父类，其父类的父类，等等。

上面的代码非常类似于最初的 `Point` 类，其中显示了两个新的构造：

- `extends Point` 表示这个类继承自 `Point` 类
- `super(x, y)` 是对父类构造函数的调用，在本例中是 `Point.new`

子类不需要定义自己的构造函数，但如果定义了，就必须调用 `super()`。与其他一些面向对象语言不同，这个调用可以出现在构造函数代码的任何地方，而不必是第一个表达式。

一个类可以重写其父类的[方法](haxe:class-field/method)，这需要显式的 `override` 关键字。这种方法的影响和限制在[覆盖方法](haxe:class-field/overriding)中详细说明。

类可以用关键字`final`来声明，以防止它们被扩展。

### 接口

接口可以被理解为类的签名，因为它描述了类的公共字段。接口不提供实现，而是提供纯粹的结构信息：

```haxe
interface Printable {
    public function toString():String;
}
```

语法类似于类，但有以下例外：

- 使用`interface`关键字代替`class`关键字。
- 函数没有任何[表达式](haxe:expression)。
- 每个字段必须有一个显式类型。

与[结构子类型](haxe:type-system/structural-subtyping)不同，接口描述类之间的静态关系。一个给定的类只有在它显式声明如下的情况下才被认为与接口兼容：

```haxe
class Point implements Printable { }
```

这里，`implements` 关键字表示 `Point` 与 `Printable` 有“is-a”关系，即 `Point` 的每个实例也是 `Printable` 的一个实例。虽然一个类可能只有一个父类，但它可以通过多个 `implements` 关键字实现多个接口：

```haxe
class Point implements Printable
	implements Serializable
```

编译器检查 `implements` 假设是否成立。也就是说，它确保类确实实现了接口所需的所有字段。如果类或其任何父类提供了一个实现，则认为该字段已实现。

接口字段不限于方法。它们也可以是变量和属性：

```haxe
interface Placeable {
  public var x:Float;
  public var y:Float;
}

class Main implements Placeable {
  public var x:Float;
  public var y:Float;

  static public function main() {}
}
```

接口可以使用 `extends` 关键字扩展多个其他接口：

```haxe
interface Debuggable extends Printable extends Serializable
```

与类一样，接口可以用`final`关键字进行标记，以防止它们被扩展。

(haxe:types/enum-instance)=
## 枚举实例

Haxe 提供了强大的枚举类型（简称为 enum），实际上是一种**代数数据类型**（algebraic data type，简称 ADT）。虽然它们不能有任何表达式，但它们对于描述数据结构非常有用：

```haxe
enum Color {
  Red;
  Green;
  Blue;
  Rgb(r:Int, g:Int, b:Int);
}
```

从语义上讲，该枚举描述了一种颜色，它可以是红色、绿色、蓝色或指定的 RGB 值。其句法结构如下：

- 关键字 `enum` 表示我们正在声明一个 enum。
- `Color`是枚举的名称，可以是任何符合类型标识符规则的东西。
- 用大括号`{}`括起来的是**枚举构造函数**，它们是 Red、Green 和 Blue，没有参数，Rgb 有三个 Int 参数，分别是 r、g 和 b。

Haxe 类型系统提供了一个与所有枚举类型统一的类型：

`Enum<T>`
:	此类型兼容所有枚举类型。在编译时，`Enum<T>`可以被视为所有枚举类型的通用基类型。然而，这种关系并没有反映在生成的代码中。

### Enum 构造函数

与类及其构造函数类似，枚举可以使用它们的构造函数进行实例化。然而，与类不同的是，`enum` 提供了多个构造函数，可以通过它们的名称进行访问：

```haxe
var a = Red;
var b = Green;
var c = Rgb(255, 255, 0);
```

在这段代码中，变量 `a`、`b` 和 `c` 的类型是 `Color`。变量 `c` 使用 `Rgb` 构造函数的参数进行初始化。

所有枚举实例都可以分配给名为 `EnumValue` 的特殊类型。

`EnumValue`
:	`EnumValue` 是一种特殊类型，与所有枚举实例统一。它被 [Haxe 标准库](https://haxe.org/manual/std.html)用来为所有枚举实例提供特定的操作，当 API 需要枚举实例而不是特定的实例时，可以在用户代码中相应使用。

区别 enum 类型和 enum 构造函数是很重要的，如下例所示：

```haxe
enum Color {
  Red;
  Green;
  Blue;
  Rgb(r:Int, g:Int, b:Int);
}

class Main {
  static public function main() {
    var ec:EnumValue = Red; // valid
    var en:Enum<Color> = Color; // valid
    // Error: Color should be Enum<Color>
    // var x:Enum<Color> = Red;
  }
}
```

如果注释行未被注释，程序将无法编译，因为 `Red`（enum 构造函数）不能被赋值给 `Enum<Color>`（enum类型）的变量。这种关系类似于一个类及其实例。

### 使用枚举

如果只允许有限的值集，枚举是一个很好的选择。然后，各个构造函数表示允许的变量，并使编译器检查是否所有可能的值都被重视：

```haxe
enum Color {
  Red;
  Green;
  Blue;
  Rgb(r:Int, g:Int, b:Int);
}

class Main {
  static function main() {
    var color = getColor();
    switch (color) {
      case Red:
        trace("Color was red");
      case Green:
        trace("Color was green");
      case Blue:
        trace("Color was blue");
      case Rgb(r, g, b):
        trace("Color had a red value of " + r);
    }
  }

  static function getColor():Color {
    return Rgb(255, 0, 255);
  }
}
```

通过将`getColor()`的返回值赋值给`color`，获取`color`的值之后，使用 [`switch`表达式](haxe:expression/switch) 根据该值使用分支。前三种情况，`Red`、`Green` 和 `Blue`，都很简单，它们对应的是没有参数的 `Color` 构造函数。最后一种情况，`Rgb(r, g, b)`，显示了如何提取构造函数的参数值；它们作为 case body 表达式中的局部变量可用，就像使用了 [`var` 表达式](haxe:expression/var) 一样。

关于使用 `switch` 表达式的高级信息将在 [模式匹配](haxe:lf-pattern-matching) 一节中讨论。

(haxe:types/anonymous-structure)
## 匿名结构

匿名结构可用于对数据进行分组，而无需显式地创建类型。下面的例子创建了一个具有两个字段 `x` 和 `name` 的结构，并将它们的值分别初始化为 `12` 和`"foo"`：

```haxe
class Main {
  static public function main() {
    var myStructure = {x: 12, name: "foo"};
  }
}
```

一般的语法规则如下：

1. 结构包含在花括号`{}`中
2. 具有以逗号分隔的键-值对列表。
3. 冒号将键（必须是一个有效的[标识符](https://haxe.org/manual/expression.html#define-identifier)）与值隔开。
4. 取值为任意的 Haxe 表达式。

规则 4 暗示结构可以是嵌套的和复杂的，例如：

```haxe
var user = {
  name : "Nicolas",
    age : 32,
    pos : [
      { x : 0, y : 0 },
        { x : 1, y : -1 }
  ],
};
```

结构的字段，像类一样，可以使用点（`.`）来访问：

```haxe
// get value of name, which is "Nicolas"
user.name;
// set value of age to 33
user.age = 33;
```

值得注意的是，使用匿名结构并不会破坏类型系统。编译器确保只访问可用的字段，这意味着以下程序不能编译：

```haxe
class Test {
  static public function main() {
    var point = { x: 0.0, y: 12.0 };
    // { y : Float, x : Float } has no field z
    point.z;
  }
}
```

错误消息表明编译器知道点的类型：它是一个具有字段 `x` 和 `y` 类型为 `Float` 的结构。因为它没有`z`字段，所以访问失败。点的类型是通过类型推断知道的，这让我们不用为局部变量使用显式类型。如果`point`是一个字段，则需要显式类型：

```haxe
class Path {
    var start : { x : Int, y : Int };
    var target : { x : Int, y : Int };
    var current : { x : Int, y : Int };
}
```

为了避免这种冗余的类型声明，特别是对于更复杂的结构，建议使用 `typedef`：

```haxe
typedef Point = { x : Int, y : Int }

class Path {
    var start : Point;
    var target : Point;
    var current : Point;
}
```

你也可以使用 [](haxe:types/structure-extensions) 从其他结构中 "inherit" 字段：

```haxe
typedef Point3 = { > Point, z : Int }
```

### JSON 用于结构值

也可以使用 JavaScript 对象表示法为结构使用字符串字面量为键：

```haxe
var point = { "x" : 1, "y" : -5 };
```

虽然允许任何字符串字面值，但如果字段是有效的 Haxe 标识符，则该字段只被认为是类型的一部分。否则，Haxe 语法不允许对这样一个字段的表示访问，必须通过使用`Reflect.field` 和 `Reflect.setField` 代替。

### 结构类型的类表示法 

当定义结构类型时，Haxe 允许使用与 [Class Fields](haxe:class-field) 中描述的相同的语法。下面的 `typedef` 声明了一个 `Point` 类型，变量字段 `x` 和 `y` 的类型是 `Int`：

```haxe
typedef Point = {
    var x : Int;
    var y : Int;
}
```

结构的字段也可以用 `final` 声明，它只允许对它们赋值一次。只有当相应的字段也是 `final` 时，这样的结构才会与其他类型统一。

### 可选字段

结构类型的字段可以是可选的。在标准表示法中，这是通过在字段名前加上 `?` 来实现的：

```haxe
typedef User = {
  age : Int,
  name : String,
  ?phoneNumber : String
}
```

在类符号中，`@:optional` 元数据可以用来代替：

```haxe
typedef User = {
  var age : Int;
  var name : String;
  @:optional var phoneNumber : String;
```

结构字段可以在类表示法中声明为可选的，方法是在其名称前面加上问号`?`：

```js
typedef User = {
  var age : Int;
  var name : String;
  var ?phoneNumber : String;
}
```

### 对性能的影响

当编译到[动态目标](https://haxe.org/manual/types-nullability.html#define-dynamic-target)时，使用结构和[结构子类型](haxe:type-system/structural-subtyping)（通过扩展）对性能没有影响。然而，对[静态目标](https://haxe.org/manual/types-nullability.html#define-static-target)的访问通常比较慢。虽然其中一些（JVM、HL）优化了常见情况，但最坏的情况需要动态查找，这可能比类字段访问慢几个数量级。

(haxe:types/structure-extensions)=
### 扩展

扩展用于表示一个结构具有给定类型的所有字段以及它自己的一些附加字段：

```haxe
typedef IterableWithLength<T> = {
  > Iterable<T>,
  // read only property
  var length(default, null):Int;
}

class Main {
  static public function main() {
    var array = [1, 2, 3];
    var t:IterableWithLength<Int> = array;
  }
}
```

大于操作符`>`表示创建了一个`Iterable<T>`的扩展，后面是附加的类字段。在这种情况下，需要 `Int` 类型的只读 [属性](haxe:class-field/property) `length`。

为了与 `IterableWithLength<T>` 兼容，类型必须与 `Iterable<T>` 兼容，并提供 `Int` 类型的只读`length`属性。前面的示例分配了一个`Array`，它正好满足这些要求。

可以同时扩展多个结构（Haxe 3.1.0）：

```haxe
typedef WithLength = {
  var length(default, null):Int;
}

typedef IterableWithLengthAndPush<T> = {
  > Iterable<T>,
  > WithLength,
  function push(a:T):Int;
}

class Main {
  static public function main() {
    var array = [1, 2, 3];
    var t:IterableWithLengthAndPush<Int> = array;
  }
}
```

可以使用另一种扩展表示法，用`&`符号分隔每个扩展结构（Haxe 4.0.0）。

```js
typedef Point2D = {
  var x:Int;
  var y:Int;
}

typedef Point3D = Point2D & {z:Int};

class Main {
  static public function main() {
    var point:Point3D = {x: 5, y: 3, z: 1};
  }
}
```

(haxe:types/function)=
## 函数类型

函数类型，连同 [monomorph](haxe:types/monomorph)，是一种通常对 Haxe 用户隐藏得很好的类型，但却无处不在。我们可以使用 `$type` 让它浮出表面，这是一个特殊的 Haxe 标识符，它会在编译过程中输出其表达式的类型：

```haxe
class Main {
  static public function main() {
    // (i : Int, s : String) -> Bool
    $type(test);
    $type(test(1, "foo")); // Bool
  }

  static function test(i:Int, s:String):Bool {
    return true;
  }
}
```

函数`test`的声明和第一个`$type`表达式的输出之间有很大的相似之处，只有一个细微的区别：函数返回类型出现在`->`符号之后的末尾。

在这两种表示法中，很明显函数`test`接受一个`Int`类型的参数和一个`String`类型的参数，并返回`Bool`类型的值。如果在第二个`$type`表达式中调用这个函数，例如 `test(1, "foo")`， Haxe 类型将检查 `1` 是否可以被赋值给 `Int`，而`"foo"`是否可以被赋值给`String`。然后调用的类型等于`test`返回的值的类型，即`Bool`。

注意，参数名在函数类型中是可选的。如果函数类型有其他函数类型作为参数或返回类型，可以使用圆括号正确地对它们进行分组。例如，`(Int, ((Int) -> Void)) -> Void` 表示一个函数，该函数有一个 `Int` 型参数和一个 `Int -> Void` 型参数，返回类型为 `Void`。

没有参数的函数类型使用`()`表示参数列表：

```haxe
class Main {
  static public function main() {
    // () -> Bool
    $type(test2);
  }

  static function test2():Bool {
    return true;
  }
}
```

(haxe:types/function-optional-arguments)=
### 可选参数

可选参数通过在参数标识符前面加上`?`来声明：

```haxe
class Main {
  static public function main() {
    // (?i : Int, ?s : String) -> String
    $type(test);
    trace(test()); // i: null, s: null
    trace(test(1)); // i: 1, s: null
    trace(test(1, "foo")); // i: 1, s: foo
    trace(test("foo")); // i: null, s: foo
  }

  static function test(?i:Int, ?s:String) {
    return "i: " + i + ", s: " + s;
  }
}
```

函数`test`有两个可选参数：`Int`类型的`i`和`String`类型的`s`。这直接反映在第4行输出的函数类型中。这个示例程序调用`test`四次并打印它的返回值。

- 第一个调用是不带任何参数的。
- 第二次调用的单参数为`1`。
- 第三次调用有两个参数`1`和`"foo"`。
- 第四个调用使用单数参数`"foo"`。

输出显示调用中省略的可选参数的值为`null`。这意味着这些参数的类型必须承认`null`作为值，这就提出了其可为空性的问题。当编译到静态目标时，Haxe 编译器通过推断其类型为 `Null<T>` 来确保可选的基本类型参数为空。

虽然前三个回调是直观的，但第四个回调可能会让人感到惊讶；如果提供的值可分配给后面的参数，则可跳过可选参数。

### 默认值

Haxe 通过给参数赋一个常数值来允许参数的默认值：

```haxe
class Main {
  static public function main() {
    // (?i : Int, ?s : String) -> String
    $type(test);
    trace(test()); // i: 12, s: bar
    trace(test(1)); // i: 1, s: bar
    trace(test(1, "foo")); // i: 1, s: foo
    trace(test("foo")); // i: 12, s: foo
  }

  static function test(?i = 12, s = "bar") {
    return "i: " + i + ", s: " + s;
  }
}
```

这个例子与 [](haxe:types/function-optional-arguments) 中的例子非常相似，唯一的区别是值 `12` 和 `"bar"` 分别被赋值给函数参数`i`和`s`。其效果是，如果从调用中省略参数，则使用默认值，而不是`null`。

Haxe 中的默认值不是类型的一部分，除非函数是[内联](haxe:class-field/inline)的，否则不会在调用点被替换。在某些目标上，编译器可能仍然会为省略的参数值传递`null`，并在函数内部生成类似于此的代码：

```haxe
static function test(i = 12, s = "bar") {
	if (i == null)
		i = 12;
	if (s == null)
		s = "bar";
	return "i: " + i + ", s: " + s;
}
```

在性能关键的代码中应该考虑这一点，因为没有默认值的解决方案有时可能更可行。

(haxe:types/dynamic)=
## Dynamic

https://haxe.org/manual/types-dynamic.html

(haxe:types/abstract)=
## 抽象类型

https://haxe.org/manual/types-abstract.html

(haxe:types/monomorph)=
## Monomorph

Monomorph 是一种类型，通过[unification](https://haxe.org/manual/type-system-unification.html)，可以演变成另一种类型。关于该类型的更多细节将在[类型推断](haxe:type-system/type-inference)一节中解释。