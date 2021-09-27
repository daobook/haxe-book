(haxe:language-features)=
# Haxe 语言特性

参考：https://haxe.org/manual/lf.html & https://haxe.org/documentation/introduction/language-features.html

[抽象类型](https://haxe.org/manual/types-abstract.html)
:   抽象类型是编译时构造，在运行时以不同的方式表示。这允许赋予现有类型全新的含义。

[匿名结构](https://haxe.org/manual/types-anonymous-structure.html)
:   可以很容易地将数据分组到匿名结构中，从而最小化小数据类的必要性。

    ```haxe
    var point = { x: 0, y: 10 };
    point.x += 10;
    ```

[数组推导](https://haxe.org/manual/lf-array-comprehension.html)
:   使用 for 循环和逻辑快速创建和填充数组。

    ```haxe
    var evenNumbers = [ for (i in 0...100) if (i%2==0) i ];
    ```

[类、接口和继承](https://haxe.org/manual/types-class-instance.html)
:   Haxe 允许在类中构造代码，使其成为一种面向对象的语言。支持 Java 等语言中常见的相关特性，包括继承和接口。

[条件编译](https://haxe.org/manual/lf-condition-compilation.html)
:   条件编译允许根据编译参数编译特定的代码。这有助于抽象特定于目标的差异，但也可以用于其他目的，比如更详细的调试。

    ```haxe
    #if js
        js.Lib.alert("Hello");
    #elseif sys
        Sys.println("Hello");
    #end
    ```

[(广义)代数数据类型](https://haxe.org/manual/types-enum-instance.html)
:   结构可以通过代数数据类型（algebraic data types，简称 ADT）表示，在 Haxe 语言中称为枚举。此外，Haxe 支持它们的广义变体 GADT。

    ```haxe
    enum Result {
        Success(data:Array<Int>);
        UserError(msg:String);
        SystemError(msg:String, position:PosInfos);
    }
    ```

[内联调用](https://haxe.org/manual/class-field-inline.html)
:   函数可以被设计成内联的，允许它们的代码被插入到调用站点。这可以产生显著的性能优势，而无需通过手工内联进行代码复制。

[迭代器](https://haxe.org/manual/lf-iterators.html)
:   在 Haxe 中迭代一组值（例如数组元素）非常容易。自定义类可以快速实现迭代器功能以允许迭代。

    ```haxe
    for (i in [1, 2, 3]) {
        trace(i);
    }
    ```

[局部函数和闭包](https://haxe.org/manual/expression-function.html)
:   Haxe 中的函数不局限于类字段，也可以在表达式中声明，从而允许强大的闭包。

    ```haxe
    var buffer = "";
    function append(s:String) {
        buffer += s;
    }
    append("foo");
    append("bar");
    trace(buffer); // foobar
    ```

[元数据](https://haxe.org/manual/lf-metadata.html)
:   向字段、类或表达式中添加元数据。这可以将信息传递给编译器、宏或运行时类。

    ```haxe
    class MyClass {
        @range(1, 8) var value:Int;
    }
    trace(haxe.rtti.Meta.getFields(MyClass).value.range); // [1,8]
    ```

[静态扩展](https://haxe.org/manual/lf-static-extension.html)
:   通过使用静态扩展，现有的类和其他类型可以用额外的功能进行扩展。

    ```haxe
    using StringTools;
    "  Me & You    ".trim().htmlEscape();
    ```

[字符串插值](https://haxe.org/manual/lf-string-interpolation.html)
:   用单引号声明的字符串能够访问当前上下文中的变量。

    ```haxe
    trace('My name is $name and I work in ${job.industry}');
    ```

[部分函数应用程序](https://haxe.org/manual/lf-function-bindings.html)
:   任何函数都可以部分应用，提供一些参数的值，其余的留到以后填充。

    ```haxe
    var map = new haxe.ds.IntMap();
    var setToTwelve = map.set.bind(_, 12);
    setToTwelve(1);
    setToTwelve(2);
    ```

[模式匹配](https://haxe.org/manual/lf-pattern-matching.html)
:   可以根据模式匹配复杂结构，从枚举或结构中提取信息，并为特定的值组合定义特定的操作。

    ```haxe
    var a = { foo: 12 };
    switch (a) {
        case { foo: i }: trace(i);
        default:
    }
    ```

[属性](https://haxe.org/manual/class-field-property.html)
:   可以将变量类字段设计为具有自定义读写访问权限的属性，从而允许细粒度的访问控制。

    ```haxe
    public var color(get,set);
    function get_color() {
        return element.style.backgroundColor;
    }
    function set_color(c:String) {
        trace('Setting background of element to $c');
        return element.style.backgroundColor = c;
    }
    ```

[类型参数、约束和方差](https://haxe.org/manual/type-system-type-parameters.html)
:   可以使用类型参数对类型进行参数化，从而允许类型容器和其他复杂的数据结构。类型参数也可以被限制为特定的类型并遵循方差规则。

    ```haxe
    class Main<A> {
        static function main() {
            new Main<String>("foo");
            new Main(12); // use type inference
        }

        function new(a:A) { }
    }
    ```

(haxe:lf-pattern-matching)=
## Pattern Matching

https://haxe.org/manual/lf-pattern-matching.html