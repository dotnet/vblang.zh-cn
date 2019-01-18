---
ms.openlocfilehash: 2cfc4380cd0a27f0aed9011406b2aa0ef4f7d23f
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426659"
---
# <a name="expressions"></a>表达式

表达式是一系列运算符和操作数指定的一个值，计算值或指定变量或常量。 本章定义的语法，操作数和运算符的求值和表达式的含义的顺序。

```antlr
Expression
    : SimpleExpression
    | TypeExpression
    | MemberAccessExpression
    | DictionaryAccessExpression
    | InvocationExpression
    | IndexExpression
    | NewExpression
    | CastExpression
    | OperatorExpression
    | ConditionalExpression
    | LambdaExpression
    | QueryExpression
    | XMLLiteralExpression
    | XMLMemberAccessExpression
    ;
```

## <a name="expression-classifications"></a>表达式分类

每个表达式分类为以下值之一：

* *一个值。* 每个值都有关联的类型。

* *一个变量。* 每个变量具有关联的类型，即该变量的声明类型。

* *一个命名空间。* 具有此分类的表达式只可以显示为左侧和右侧的成员访问。 在任何其他上下文中，分类为一个命名空间的表达式将导致编译时错误。

* *一种类型。* 具有此分类的表达式只可以显示为左侧和右侧的成员访问。 在任何其他上下文中，分类为一种类型的表达式将导致编译时错误。

* *方法组，* 这是一组相同的名称上重载的方法。 方法组可能有一个相关联的目标表达式和关联的类型参数列表。

* *方法指针*表示一种方法的位置。 方法指针可能有一个相关联的目标表达式和关联的类型参数列表。

* *Lambda 方法*即匿名方法。

* *属性组中，* 这是一组相同的名称上重载的属性。 属性组可能具有相关联的目标表达式。

* *属性访问。* 每个属性访问具有关联的类型，即属性的类型。 属性访问可能会有相关联的目标表达式。

* *后期绑定访问*表示延迟到运行时的方法或属性访问。 后期绑定访问可能有一个相关联的目标表达式和关联的类型参数列表。 后期绑定访问的类型始终是`Object`。

* *事件的访问。* 每个事件访问具有关联的类型，即该事件的类型。 事件访问可能会有相关联的目标表达式。 事件访问可能显示的第一个参数为`RaiseEvent`， `AddHandler`，和`RemoveHandler`语句。 在任何其他上下文中，分类为事件访问的表达式将导致编译时错误。

* *数组文本，* 表示尚未确定其类型的数组的初始值。

* *Void。* 表达式的子例程或无结果与 await 运算符表达式调用时，将发生这种情况。 归类为 void 的表达式只是在调用语句或 await 语句的上下文中无效。

* *默认值。* 仅文本`Nothing`生成此分类。

一个表达式的最终结果通常是一个值或具有其他类别的表达式作为仅允许在某些上下文中的中间值的变量。

请注意，可以在语句和需要的要具有某些特征 （如正在引用类型、 值类型、 派生一些类型，等等） 的表达式类型的表达式中使用的类型为类型参数的表达式如果施加约束对类型参数满足这些特征。

### <a name="expression-reclassification"></a>表达式重新分类

通常情况下，需要从该表达式的不同分类的上下文中使用表达式时，编译时错误发生-例如，尝试将值分配为文本。 但是，在许多情况下就可以更改表达式的分类的过程通过*重新分类*。

如果成功重新分类，然后重新分类以作为扩大或收缩。 除非另行说明，否则此列表中的所有 reclassifications 扩大都转换。

下列类型的表达式可以重新分类：

* 变量可以重新分类为值。 将提取存储在变量中的值。

* 方法组可以重新分类为一个值。 方法组表达式被解释为具有相关联的目标表达式和类型形参列表和空括号调用表达式 (即`f`被解释为`f()`和`f(Of Integer)`被解释为`f(Of Integer)()`). 此重新分类可能会导致正在进一步重新分类为 void 的表达式。

* 方法指针可以重新分类为一个值。 此重新分类只能发生在知道目标类型的转换的上下文。 方法的指针表达式被解释为具有关联的类型参数列表的相应类型的委托实例化表达式的参数。 例如：
    
    ```vb
    Delegate Sub D(i As Integer)
    
    Module Test
        Sub F(i As Integer)
        End Sub
    
        Sub Main()
            Dim del As D
    
            ' The next two lines are equivalent.
            del = AddressOf F
            del = New D(AddressOf F)
        End Sub
    End Module
    ```

* Lambda 方法可以重新分类为一个值。 如果知道目标类型的转换的上下文中发生重新分类，然后两个 reclassifications 可能会出现之一：
    
  1. 如果目标类型是委托类型，lambda 方法被解释为相应类型的委托构造表达式的参数。
    
  2. 如果目标类型是`System.Linq.Expressions.Expression(Of T)`，并`T`为委托类型，则像委托构造表达式中使用它解释 lambda 方法`T`，然后转换为表达式树。
    
  如果该委托没有 ByRef 参数，async 或 iterator lambda 方法可能仅被解释为委托构造表达式的参数。
    
  如果从任何委托的参数类型到相应的 lambda 参数类型的转换是收缩转换，然后重新分类被判定为收缩;否则它为扩大转换。
    
  __请注意。__ Lambda 方法和表达式树之间的确切转换可能不会修复之间版本的编译器和超出了本规范的范畴。 For Microsoft Visual Basic 11.0，所有的 lambda 表达式可能会转换为表达式树有以下限制：(1) 1.  仅单行 lambda 表达式而无需 ByRef 参数可能会转换为表达式树。 单个行的`Sub`lambda，唯一的调用语句可能会转换为表达式树。 （如果早期的字段初始值设定项用于初始化的后续字段初始值设定项，例如 2） 匿名类型的表达式不能转换为表达式树`New With {.a=1, .b=.a}`。 （3） 对象初始值设定项表达式无法转换为表达式树如果当前对象进行初始化的成员将用在一个字段初始值设定项，例如`New C1 With {.a=1, .b=.Method1()}`。 （4） 多维数组创建表达式只可以转换为表达式树，如果它们显式声明它们的元素类型。 （5） Late 绑定表达式无法转换为表达式树。 （一般 VB 语义 6） 当变量或字段到调用表达式传递 ByRef 但不具有 ByRef 参数的类型完全相同或 ByRef 传递属性时，都将 ByRef 传递自变量的副本，然后复制其最终值 恢复到的变量或字段或属性。 在表达式树中，复制后不会发生。 （7） 所有这些限制适用于以及嵌套的 lambda 表达式。
    
  如果不知道目标类型，则 lambda 方法解释为匿名委托类型的委托实例化表达式具有相同签名的 lambda 方法的参数。 如果正在使用严格的语义，并且会省略任何参数的类型，会发生编译时错误;否则为`Object`替换为任何缺少的参数类型。 例如：
    
  ```vb
  Module Test
      Sub Main()
          ' Type of x will be equivalent to Func(Of Object, Object, Object)
          Dim x = Function(a, b) a + b
  
          ' Type of y will be equivalent to Action(Of Object, Object)
          Dim y = Sub(a, b) Console.WriteLine(a + b)
      End Sub
  End Module
  ```

* 属性组可以重新分类为属性访问。 属性组表达式解释为索引表达式使用空括号 (即`f`被解释为`f()`)。

* 属性访问可以重新分类为值。 属性访问表达式被解释为调用表达式的`Get`属性访问器。 如果属性没有 getter，因此，则会发生编译时错误。

* 后期绑定访问可以重新分类为后期绑定方法或后期绑定属性访问。 在其中后期绑定访问可以重新分类作为方法访问和属性访问的情况下，重新分类到属性访问是首选。

* 后期绑定访问可以重新分类为一个值。

* 数组文本可以重新分类为一个值。 值的类型确定，如下所示：

  1. 如果其中的目标类型已知时，目标类型是数组类型的转换的上下文中发生重新分类，然后数组文本是重新分类为 T() 类型的值。 如果目标类型是`System.Collections.Generic.IList(Of T)`， `IReadOnlyList(Of T)`， `ICollection(Of T)`， `IReadOnlyCollection(Of T)`，或`IEnumerable(Of T)`，并且数组文本具有一级嵌套，则数组文本重新分类为值类型为`T()`。

  2. 否则，数组文本是重新分类到其类型是数组的秩等于的嵌套级别使用，并且使用初始值设定项; 中的元素的基准类型确定的元素类型的值如果没有基准类型不能确定，`Object`使用。 例如：

     ```vb
     ' x Is GetType(Double(,,))
     Dim x = { { { 1, 2.0 }, { 3, 4 } }, { { 5, 6 }, { 7, 8 } } }.GetType()
        
     ' y Is GetType(Integer())
     Dim y = { 1, 2, 3 }.GetType()
        
     ' z Is GetType(Object())
     Dim z = { 1, "2" }.GetType()
        
     ' Error: Inconsistent nesting
     Dim a = { { 10 }, { 20, 30 } }.GetType()
     ```

  __请注意。__ 没有版本 9.0 和 10.0 版本的语言之间的行为会略有变化。 低于 10.0，数组元素初始值设定项不会影响本地变量的类型推理，现在它们执行操作。 因此`Dim a() = { 1, 2, 3 }`将具有推断`Object()`的类型为`a`中的语言的 9.0 版和`Integer()`版本 10.0。

  重新分类然后重新解释数组文本作为数组创建表达式。 因此这些示例：

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  是到等效的：

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  为收缩从元素表达式为数组元素类型的任何转换为收缩转换; 如果以重新分类否则，它被判定为扩大转换。

* 默认值`Nothing`可以重新分类为一个值。 在上下文中的目标类型已知的时，结果是目标类型的默认值。 在上下文中无法知道目标类型，结果为空值类型的`Object`。

命名空间表达式、 类型表达式中，事件访问表达式或 void 表达式不能重新分类。 可在同一时间完成多个 reclassifications。 例如：

```vb
Module Test
    Sub F(i As Integer)
    End Sub

    ReadOnly Property P() As Integer
        Get
        End Get
    End Sub

    Sub Main()
        F(P)
    End Property
End Module
```

在这种情况下，该属性组表达式`P`属性访问从属性组首先重新分类并为值属性访问从然后重新分类。 Reclassifications 数最少执行以访问上下文中的有效分类。

## <a name="constant-expressions"></a>常量表达式

一个*常量表达式*是可以在编译时完全计算其值的表达式。

```antlr
ConstantExpression
    : Expression
    ;
```

常量表达式的类型可以是`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Char`， `Single`， `Double`， `Decimal`， `Date`， `Boolean`， `String`， `Object`，或任何枚举类型。 常量表达式中允许以下构造：

* 文本 (包括`Nothing`)。

* 对常量类型成员或常量局部变量的引用。

* 对枚举类型的成员的引用。

* 带括号的子表达式。

* 强制转换表达式，提供目标类型是上面列出的类型之一。 强制来回`String`是此规则的例外，因为只允许对 null 值`String`始终完成转换的执行环境的当前区域性中在运行时。 请注意，常量强制转换表达式都可以只使用内部函数转换。

* `+`，`-`和`Not`一元运算符提供操作数和结果是上面列出的类型。

* `+`， `-`， `*`， `^`， `Mod`， `/`， `\`， `<<`， `>>`， `&`， `And`， `Or`， `Xor`， `AndAlso`， `OrElse`， `=`， `<`， `>`， `<>`， `<=`，和`=>`二元运算符，提供每个操作数和结果是上面列出的类型。

* 条件运算符如果提供每个操作数和结果是上面列出的类型。

* 以下运行时函数： `Microsoft.VisualBasic.Strings.ChrW`;`Microsoft.VisualBasic.Strings.Chr`如果常量的值介于 0 和 128;`Microsoft.VisualBasic.Strings.AscW`常量字符串是否不为空;`Microsoft.VisualBasic.Strings.Asc`常量字符串是否不为空。

以下构造都*不*允许在常数表达式中：

* 通过隐式绑定`With`上下文。

一种整型类型的常量表达式 (`ULong`， `Long`， `UInteger`， `Integer`， `UShort`， `Short`， `SByte`，或`Byte`) 可以隐式转换为较窄的整数类型、 和类型的常量表达式`Double`可以隐式转换为`Single`，前提是目标类型的范围内的常量表达式的值。 无论是否使用宽松或严格语义允许这些收缩转换。


## <a name="late-bound-expressions"></a>后期绑定表达式

成员访问表达式或索引表达式的目标时的类型`Object`，该表达式的处理可能延迟到运行时。 推迟处理这种方式调用*后期绑定*。 后期绑定允许`Object`要在中使用的变量*无类型*成员的所有解决方法根据变量中的值的实际运行时类型的方法。 如果通过编译环境或通过指定严格的语义`Option Strict`，后期绑定会导致编译时错误。 执行后期绑定，包括以重载决策的目的时，将忽略非公共成员。 请注意，与早期绑定的情况下，调用或访问不同`Shared`后期绑定成员将导致调用目标，以在运行时计算。 如果表达式是定义上的成员的调用表达式`System.Object`，后期绑定将不会发生。

一般情况下，后期绑定访问的解析在运行时查找该表达式的实际运行时类型中的标识符。 如果在运行时，后期绑定成员查找失败`System.MissingMemberException`引发异常。 因为执行后期绑定成员查找仅关闭关联的目标表达式的运行时类型，对象的运行时类型永远不会是一个接口。 因此，就无法访问后期绑定成员访问表达式中的接口成员。

它们在成员访问表达式中出现的顺序计算这些实参后期绑定成员访问： 不在后期绑定成员声明参数的顺序。 下面的示例说明了这种差异：

```vb
Class C
    Public Sub f(ByVal x As Integer, ByVal y As Integer)
    End Sub
End Class

Module Module1
    Sub Main()
        Console.Write("Early-bound: ")
        Dim c As C = New C
        c.f(y:=t("y"), x:=t("x"))

        Console.Write(vbCrLf & "Late-bound: ")
        Dim o As Object = New C
        o.f(y:=t("y"), x:=t("x"))
    End Sub

    Function t(ByVal s As String) As Integer
        Console.Write(s)
        Return 0
    End Function
End Module
```

此代码将显示：

```
Early-bound: xy
Late-bound: yx
```

因为后期绑定重载解决方法在运行时类型的参数来完成，就可以表达式可能会产生不同的结果基于它是否将在编译时或运行的时进行计算。 下面的示例说明了这种差异：

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(b As Base)
        Console.WriteLine("F(Base)")
    End Sub

    Sub F(d As Derived)
        Console.WriteLine("F(Derived)")
    End Sub

    Sub Main()
        Dim b As Base = New Derived()
        Dim o As Object = b

        F(b)
        F(o)
    End Sub
End Module
```

此代码将显示：

```
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a>简单表达式

简单表达式是文本、 带括号的表达式、 实例表达式或简单名称表达式。

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a>文本表达式

文本表达式的计算结果为文本所表示的值。 文本表达式分类为一个值，除了文本`Nothing`，其分类为默认值。

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a>带括号的表达式

带括号的表达式包含一个表达式括在括号中。 带括号的表达式分类为一个值，并带括号的表达式必须分类为一个值。 带括号的表达式计算结果为在括号中的表达式的值。

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a>实例表达式

*实例表达式*关键字`Me`。 它可能只是在正文中使用的非共享方法、 构造函数或属性访问器。 它被分类为一个值。 关键字`Me`表示包含正在执行的方法或属性访问器的类型的实例。 如果一个构造函数将显式调用另一个构造函数 (部分[构造函数](type-members.md#constructors))，`Me`不能使用直到该构造函数调用后，因为未尚未构造实例。

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a>简单名称表达式

一个*的简单名称表达式*包含单个标识符后跟可选类型参数列表。

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

名称是已解决，并且根据以下"简单名称解析规则"进行了分类：

1.  开头立即封闭块并继续 （如果有） 每个封闭外部块，如果标识符匹配的本地变量、 静态变量、 常量局部变量的名称，方法类型参数或参数，则标识符引用匹配的实体。

    如果标识符匹配的本地变量、 静态变量或常量局部变量并提供类型实参列表时，发生编译时错误。 如果标识符与方法类型参数相匹配，未提供任何类型实参列表，没有出现匹配项，并解析继续运行。 如果标识符匹配的本地变量，匹配的本地变量是隐式函数或`Get`访问器返回本地变量和表达式是调用表达式，调用语句的一部分或`AddressOf`然后表达式没有出现匹配项，并且解决将继续。

    表达式分类为变量，如果它是本地变量、 静态变量或参数。 表达式分类为一个类型，如果它是方法的类型参数。 表达式分类为一个值，如果它是常量局部变量。

2.  对于每个包含的表达式的嵌套类型，从最里层开始并转到最外面，如果类型中的标识符的查找会生成与可访问的成员匹配项：

    21. 如果匹配的类型成员是类型参数，结果分类为一个类型，并且是匹配的类型参数。 如果未提供类型实参列表，没有出现匹配项，并解析继续运行。
    22. 否则，如果该类型是最近的封闭类型，并查找标识出非共享类型成员，则结果是与成员访问窗体的相同`Me.E(Of A)`，其中`E`标识符和`A`是类型参数列表如果有的话。
    23. 否则，结果应完全相同的窗体成员访问`T.E(Of A)`，其中`T`是包含匹配的成员，类型`E`的标识符和`A`是类型参数列表中，如果有的话。 在这种情况下，它是要引用的非共享成员的标识符为错误的。

3.  对于每个嵌套命名空间，从最里层开始，然后转到最外层命名空间，请执行以下操作：

    31. 如果命名空间包含具有给定名称的可访问类型，并且具有相同数量的类型参数，如已提供在类型参数列表中，如果有，则标识符引用该类型和分类为一个类型。
    32. 否则为如果已提供任何类型参数列表，该命名空间包含具有给定名称的命名空间成员，然后该标识符引用该命名空间，分类为一个命名空间。
    33. 否则，如果该命名空间包含一个或多个可访问标准模块，并且成员名称查找的标识符会生成一个标准模块中的可访问的匹配项，则结果是完全与窗体的成员访问相同`M.E(Of A)`，其中`M`是包含匹配的成员的标准模块`E`的标识符和`A`是类型参数列表中，如果有的话。 如果标识符匹配多个标准模块中的可访问类型成员，发生编译时错误。

4.  如果源文件具有一个或多个导入别名，并且标识符匹配的其中一个名称，然后标识符是指为该命名空间或类型。 如果提供类型实参列表，则会发生编译时错误。

5. 如果包含名称引用的源文件具有一个或多个导入：

    51. 如果标识符匹配中只有一个导入具有相同数量的类型参数的可访问类型的名称，如提供中类型参数列表中，如果有或类型成员，则标识符是指该类型或类型成员。 如果标识符匹配多个导入中具有相同数量的类型参数的可访问类型的名称作为已提供在类型参数列表中，如果有，或一个可访问类型成员，编译时错误时发生。
    52. 否则，如果不提供任何类型参数列表和一个导入具有可访问类型的命名空间的名称与标识符匹配，然后标识符引用该命名空间。 如果不提供任何类型参数列表和多个导入具有可访问类型的命名空间的名称与标识符匹配，发生编译时错误。
    53. 否则，如果导入包含一个或多个可访问标准模块，并且成员名称查找的标识符会生成一个标准模块中的可访问的匹配项，则结果是完全与窗体的成员访问相同`M.E(Of A)`，其中`M`是包含匹配的成员的标准模块`E`的标识符和`A`是类型参数列表中，如果有的话。 如果标识符匹配多个标准模块中的可访问类型成员，发生编译时错误。

6.  如果编译环境定义了一个或多个导入别名，并标识符匹配的其中一个名称，然后标识符引用该命名空间或类型。 如果提供类型实参列表，则会发生编译时错误。

7. 如果编译环境定义了一个或多个导入：

    71. 如果标识符匹配中只有一个导入具有相同数量的类型参数的可访问类型的名称，如提供中类型参数列表中，如果有或类型成员，则标识符是指该类型或类型成员。 如果标识符匹配多个导入中具有相同数量的类型参数的可访问类型的名称作为已提供在类型参数列表中，如果有，或类型成员，编译时错误时发生。
    72. 否则，如果不提供任何类型参数列表和一个导入具有可访问类型的命名空间的名称与标识符匹配，然后标识符引用该命名空间。 如果不提供任何类型参数列表和多个导入具有可访问类型的命名空间的名称与标识符匹配，发生编译时错误。
    73. 否则，如果导入包含一个或多个可访问标准模块，并且成员名称查找的标识符会生成一个标准模块中的可访问的匹配项，则结果是完全与窗体的成员访问相同`M.E(Of A)`，其中`M`是包含匹配的成员的标准模块`E`的标识符和`A`是类型参数列表中，如果有的话。 如果标识符匹配多个标准模块中的可访问类型成员，发生编译时错误。

8. 否则，给定标识符的名称是不确定的。

未定义的简单名称表达式是一个编译时错误。

通常情况下，名称可仅出现一次在特定的命名空间中。 但是，可以跨多个.NET 程序集声明命名空间，因此很可能有两个程序集在其中定义具有相同的完全限定名称的类型的情况。 在这种情况下，在当前组源代码文件中声明的类型优于外部.NET 程序集中声明的类型。 否则为名称不明确，则不能消除名称歧义。


### <a name="addressof-expressions"></a>AddressOf 表达式

`AddressOf`表达式用于生成方法的指针。 表达式组成`AddressOf`关键字和必须归类为方法组或后期绑定访问的表达式。 构造函数不能引用方法组。

结果被归类为方法指针，使用相同的关联的目标表达式和类型参数列表 （如果有） 为方法组。

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a>类型表达式

一个*键入表达式*是`GetType`表达式`TypeOf...Is`表达式`Is`表达式，或`GetXmlNamespace`表达式。

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a>GetType 表达式

一个`GetType`表达式包含关键字的`GetType`和类型的名称。

```antlr
GetTypeExpression
    : 'GetType' OpenParenthesis GetTypeTypeName CloseParenthesis
    ;

GetTypeTypeName
    : TypeName
    | QualifiedOpenTypeName
    ;

QualifiedOpenTypeName
    : Identifier TypeArityList? (Period IdentifierOrKeyword TypeArityList?)*
    | 'Global' Period IdentifierOrKeyword TypeArityList?
      (Period IdentifierOrKeyword TypeArityList?)*
    ;

TypeArityList
    : OpenParenthesis 'Of' CommaList? CloseParenthesis
    ;

CommaList
    : Comma Comma*
    ;
```

一个`GetType`表达式分类为一个值，并且其值为反射 (`System.Type`) 表示的类及其*GetTypeTypeName*。 如果*GetTypeTypeName*是一个类型参数，该表达式将返回`System.Type`对应于为在运行时类型形参提供类型参数的对象。

*GetTypeTypeName*是特殊两种方式：

* 它是否可为`System.Void`，可能会引用此类型名称的位置在语言中的唯一位置。

* 它可能具有省略的类型参数的构造泛型类型。 这允许`GetType`表达式以返回`System.Type`对应于泛型类型本身的对象。

下面的示例演示`GetType`表达式：

```vb
Module Test
    Sub Main()
        Dim t As Type() = { GetType(Integer), GetType(System.Int32), _
            GetType(String), GetType(Double()) }
        Dim i As Integer

        For i = 0 To t.Length - 1
            Console.WriteLine(t(i).Name)
        Next i
    End Sub
End Module
```

生成的输出是：

```
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a>TypeOf...为表达式

一个`TypeOf...Is`表达式用于检查一个值的运行时类型是否与给定类型兼容。 第一个操作数必须分类为一个值，不能重新分类的 lambda 方法和必须是引用类型或不受约束的类型参数类型。 第二个操作数必须是类型名称。 表达式的结果分类为一个值，并且是`Boolean`值。 表达式的计算结果`True`操作数的运行时类型标识、 默认值、 引用、 数组、 值类型或类型参数转换为类型，如果`False`否则为。 如果不存在转换表达式的类型和特定类型之间，会发生编译时错误。

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a>为表达式

`Is`或`IsNot`表达式用于执行引用相等性比较。

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

每个表达式必须分类为一个值，每个表达式的类型必须是引用类型、 不受约束的类型参数类型或为空值类型。 如果一个表达式的类型是不受约束的类型参数类型或可以为 null 值类型，但是，则另一个表达式必须文字`Nothing`。

结果分类为一个值，并被类型化为`Boolean`。 `Is`运算计算结果为`True`如果这两个值引用的相同实例或两个值均`Nothing`，或`False`否则为。 `IsNot`运算计算结果为`False`如果这两个值引用的相同实例或两个值均`Nothing`，或`True`否则为。


### <a name="getxmlnamespace-expressions"></a>GetXmlNamespace 表达式

一个`GetXmlNamespace`表达式包含关键字的`GetXmlNamespace`和 XML 命名空间声明的源文件或编译环境的名称。

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

`GetXmlNamespace`表达式分类为一个值，且其值的实例`System.Xml.Linq.XNamespace`，它表示*XMLNamespaceName*。 如果该类型不可用，则会发生编译时错误。

例如：

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim db = GetXmlNamespace(db)

        ' The following are equivalent
        Dim customer1 = _
            New System.Xml.Linq.XElement(db + "customer", "Bob")
        Dim customer2 = <db:customer>Bob</>
    End Sub
End Module
```

括号之间的所有内容被认为命名空间名称的一部分，因此 XML 规则周围的空格等应用。 例如：

```vb
Imports <xmlns:db-ns="http://example.org/database">

Module Test
    Sub Main()

        ' Error, XML name expected
        Dim db1 = GetXmlNamespace( db-ns )

        ' Error, ')' expected
        Dim db2 = GetXmlNamespace(db _
            )

        ' OK.
        Dim db3 = GetXmlNamespace(db-ns)
    End Sub
End Module
```

XML 命名空间表达式也会省略，则表达式在此情况下返回该对象表示的默认 XML 命名空间。


## <a name="member-access-expressions"></a>成员访问表达式

成员访问表达式用于访问实体的成员。

```antlr
MemberAccessExpression
    : MemberAccessBase? Period IdentifierOrKeyword
      ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;

MemberAccessBase
    : Expression
    | NonArrayTypeName
    | 'Global'
    | 'MyClass'
    | 'MyBase'
    ;
```

成员访问窗体`E.I(Of A)`，其中`E`是一个表达式，非数组类型名称，该关键字`Global`，或省略和`I`是可选的类型参数列表的标识符`A`、 计算和分类按如下所示：

1. 如果`E`省略，然后立即包含中的表达式`With`语句替换为`E`和执行成员访问。 如果没有包含`With`语句中，会发生编译时错误。

2. 如果`E`归类为命名空间或`E`关键字`Global`，然后在指定的命名空间的上下文中执行成员查找。 如果`I`中提供类型参数列表中，如果有，则结果为该成员是该命名空间具有相同数量的类型参数的可访问成员的名称。 结果被归类为命名空间或具体取决于该成员的类型。 否则，将发生编译时错误。

3. 如果`E`为的类型或属于一个类型的表达式，则在指定类型的上下文中执行成员查找。 如果`I`是可访问成员的名称`E`，然后`E.I`计算和分类，如下所示：

    31. 如果`I`是关键字`New`和`E`不是一个枚举，则会发生编译时错误。
    32. 如果`I`已提供在类型参数列表中，如果有，则结果为该类型具有相同数量的类型参数标识的类型。
    33. 如果`I`标识一个或多个方法，则结果为具有关联的类型参数列表而不是相关联的目标表达式的方法组。
    34. 如果`I`标识一个或多个属性和任何类型提供的参数列表，则结果为无关联的目标表达式的属性组。
    35. 如果`I`标识共享的变量和任何类型提供的参数列表，则结果为变量或值。 如果变量是只读的并且引用发生的外部共享的构造函数的声明该变量的类型，则结果为共享变量的值`I`在`E`。 否则，结果是共享的变量`I`在`E`。
    36. 如果`I`标识共享的事件和任何类型提供的参数列表，则结果为无关联的目标表达式的事件访问。
    37. 如果`I`标识一个常量和任何类型提供的参数列表，则结果为该常量的值。
    38. 如果`I`标识枚举成员和任何类型提供的参数列表，则结果为该枚举成员的值。
    39. 否则为`E.I`是无效的成员引用，并且将发生编译时错误。

4. 如果`E`归类为变量或值的类型是`T`，然后在上下文中执行成员查找`T`。 如果`I`是可访问成员的名称`T`，然后`E.I`计算和分类，如下所示：

    41. 如果`I`是关键字`New`，`E`是`Me`， `MyBase`，或`MyClass`，并提供没有类型参数，则结果为表示的类型的实例构造函数的方法组`E`相关联的目标表达式`E`和任何类型参数列表。 否则，将发生编译时错误。
    42. 如果`I`标识一个或多个方法，包括扩展方法，如果`T`不是`Object`，则结果为方法组具有关联的类型参数列表和相关联的目标表达式的`E`。
    43. 如果`I`标识一个或多个属性和任何类型提供参数，则结果为包含的相关联的目标表达式的属性组`E`。
    44. 如果`I`标识共享的变量或一个实例变量，并且提供参数，没有类型，则结果为变量或值。 如果变量是只读的并且引用出现在其中将变量声明适当类型的变量 （共享或实例），类的构造函数外，则结果为变量的值`I`中引用的对象通过`E`。 如果`T`是引用类型，则结果为变量`I`中所引用的对象`E`。 否则为如果`T`是值类型和表达式`E`是分类为变量，则结果为变量; 否则结果是一个值。
    45. 如果`I`标识的事件和任何类型提供的参数，则结果为的相关联的目标表达式的事件访问`E`。
    46. 如果`I`标识一个常量和提供参数，没有类型，则结果为该常量的值。
    47. 如果`I`标识枚举成员，并提供参数，没有类型，则结果为该枚举成员的值。
    48. 如果`T`是`Object`，则结果为后期绑定成员查找归类为具有关联的类型参数列表和相关联的目标表达式的后期绑定访问`E`。

5. 否则为`E.I`是无效的成员引用，并且将发生编译时错误。

成员访问窗体`MyClass.I(Of A)`等效于`Me.I(Of A)`，但在其上访问的所有成员都被都看作，就好像成员是非可重写。 因此，访问的成员不会受访问该成员的值的运行时类型。

成员访问窗体`MyBase.I(Of A)`等效于`CType(Me, T).I(Of A)`其中`T`是包含成员访问表达式的类型的直接基类型。 其上的所有方法调用都被都看作正在调用的方法是非可重写。 这种形式的成员访问也称为*基访问*。

下面的示例演示如何`Me`，`MyBase`和`MyClass`相关：

```vb
Class Base
    Public Overridable Sub F()
        Console.WriteLine("Base.F")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overrides Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Sub G()
        MyClass.F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F()
        Console.WriteLine("MoreDerived.F")
    End Sub

    Public Sub H()
        MyBase.F()
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = new MoreDerived()

        x.F()
        x.G()
        x.H()
    End Sub

End Module
```

此代码会输出：

```
MoreDerived.F
Derived.F
Derived.F
```

成员访问表达式时以关键字开头`Global`，关键字表示最外层的未命名命名空间，这是在其中声明会覆盖封闭命名空间的情况下很有用。 `Global`关键字使"转义"出到这种情况中的最外层命名空间。 例如：

```vb
Class System
End Class

Module Test
    Sub Main()
        ' Error: Class System does not contain Console
        System.Console.WriteLine("Hello, world!") 


        ' Legal, binds to System in outermost namespace
        Global.System.Console.WriteLine("Hello, world!") 
    End Sub
End Module
```

在上述示例中，第一个方法调用无效因为标识符`System`绑定到该类`System`，不是命名空间`System`。 访问的唯一办法`System`命名空间是使用`Global`来转义扩展到最外层命名空间。

如果正在访问的成员共享的左侧和右侧期间的任何表达式是多余的则不计算，除非执行成员访问后期绑定。 例如，考虑以下代码：

```vb
Class C
    Public Shared F As Integer = 10
End Class

Module Test
    Public Function ReturnC() As C
        Console.WriteLine("Returning a new instance of C.")
        Return New C()
    End Function

    Public Sub Main()
        Console.WriteLine("The value of F is: " & ReturnC().F)
    End Sub
End Module
```

它将打印`The value of F is: 10`因为函数`ReturnC`无需调用来提供的一个实例`C`来访问共享的成员`F`。


### <a name="identical-type-and-member-names"></a>相同类型和成员名称

不使用相同的名称作为其类型的名称成员少见。 在这种情况下，但是，隐藏不方便的名称会发生：

```vb
Enum Color
    Red
    Green
    Yellow
End Enum

Class Test
    ReadOnly Property Color() As Color
        Get
            Return Color.Red
        End Get
    End Property

    Shared Function DefaultColor() As Color
        Return Color.Green    ' Binds to the instance property!
    End Function
End Class
```

在上一示例中，简单名称`Color`在`DefaultColor`绑定到实例属性，而不是类型。 由于不能在共享成员中引用的实例成员，这通常会出现错误。

但是，特殊规则在这种情况下允许访问类型。 如果成员访问表达式的基表达式是一个简单名称，并将绑定到常量、 字段、 属性、 本地变量或参数的类型具有相同的名称，然后在到成员或类型的基表达式可以引用。 这永远不可能导致二义性，因为可以访问从两者中任何一个的成员相同。

### <a name="default-instances"></a>默认实例

在某些情况下，从一种常见派生的类通常基类或始终具有只有一个实例。 例如，只会显示用户界面中的大多数窗口都具有在任何时间在屏幕上显示的一个实例。 若要简化这些类型的类的处理，Visual Basic 可以自动生成*默认实例*的每个类提供一个轻松地引用实例的类。

默认实例始终为创建*系列*的类型而不是一种特定类型。 因此而不被创建一个类派生自窗体的 Form1 的默认实例，默认实例创建的所有类派生自窗体。 这意味着每个派生自的基类的单个类不需要专门标记为具有默认实例。

由编译器生成的属性，返回该类的默认实例表示一个类的默认实例。 生成的类成员的属性名为*分组类*，管理分配并销毁所有类的默认实例派生自特定基类。 例如，所有的类的默认实例属性派生自`Form`可能会收集在`MyForms`类。 如果表达式返回组类的实例`My.Forms`，则下面的代码访问派生类的默认实例`Form1`和`Form2`:

```vb
Class Form1
    Inherits Form
    Public x As Integer
End Class

Class Form2
    Inherits Form
    Public y As Integer
End Class

Module Main
    Sub Main()
        My.Forms.Form1.x = 10
        Console.WriteLine(My.Forms.Form2.y)
    End Sub
End Module
```

默认实例将不会创建之前对它们; 的第一个引用获取表示默认实例的属性会导致如果它尚未创建或已被设置为要创建的默认实例`Nothing`。 以便测试是否存在默认实例，默认实例时的目标`Is`或`IsNot`运算符，不会创建默认实例。 因此，就可以测试是否默认实例为`Nothing`或某些其他引用而不会导致要创建的默认实例。

默认实例旨在方便地引用从具有默认实例的类的外部的默认实例。 使用从定义它的类中的默认实例可能会导致的混淆对于哪些实例所引用，即默认实例或当前实例。 例如，下面的代码修改仅值`x`在默认情况下，即使它正在调用另一个实例。 因此，代码将打印的值`5`而不是`10`:

```vb
Class Form1
    Inherits Form

    Public x As Integer = 5

    Public Sub ChangeX()
        Form1.x = 10
    End Sub
End Class

Module Main
    Sub Main()
        Dim f As Form1 = New Form1()
        f.ChangeX()
        Console.WriteLine(f.x)
    End Sub
End Module
```

若要防止这种混淆，它不是类型的有效来指代的默认实例从实例方法中的默认实例。

#### <a name="default-instances-and-type-names"></a>默认实例和类型名称

默认实例也可能是可直接通过其类型的名称访问。 在这种情况下，在任何表达式上下文中的类型名称不允许在表达式`E`，其中`E`表示具有默认实例的类的完全限定的名称，更改为`E'`，其中`E'`表示一个表达式，提取的默认实例属性。 例如，如果默认实例的类派生自`Form`允许通过类型名称，访问默认实例，则下面的代码是等效于在前面的示例代码：

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

这还意味着可通过其类型的名称访问的默认实例也会通过类型名称可分配。 例如，下面的代码设置的默认实例`Form1`到`Nothing`:

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

请注意的含义`E.I`已`E`表示的类和`I`表示共享的成员不会更改。 此类表达式仍可访问共享的成员直接从类实例，并且未引用的默认实例。

#### <a name="group-classes"></a>组类

`Microsoft.VisualBasic.MyGroupCollectionAttribute`属性指示一系列的默认实例的组类。 该属性具有四个参数：

* 参数`TypeToCollect`指定组的基类。 不带开放类型参数派生自具有此名称 （而不考虑类型参数） 的类型的所有可实例化类会自动具有默认实例。

* 参数`CreateInstanceMethodName`指定要在要创建的新实例的默认实例属性中的组类中调用的方法。

* 将参数`DisposeInstanceMethodName`指定要在要释放的默认实例属性，如果默认实例属性分配值的组类中调用的方法`Nothing`。

* 将参数`DefaultInstanceAlias`详细信息表达式`E'`来代替类名称的默认实例是否可以访问直接通过其类型名称。 如果此参数为`Nothing`或空字符串，此组类型上的默认实例不是可直接通过其类型的名称访问。 (__注意。__ 在 Visual Basic 语言的所有当前实现中`DefaultInstanceAlias`参数将被忽略，编译器提供的代码中除外。)

通过分隔这些名称中使用逗号分隔的前三个参数的类型和方法，多个类型可以收集到同一个组。 必须有相同数目的项在每个参数，并将列表元素按顺序进行比较。 例如，下面的特性声明收集派生的类型`C1`，`C2`或`C3`入一个组：

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

创建方法的签名必须采用以下形式`Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`。 Dispose 方法必须采用以下形式`Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`。 因此，对于上一节中的示例组类可以声明，如下所示：

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub
End Class
```

如果源代码文件声明派生的类`Form1`，生成的组类应该是等效于：

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub

    Private m_Form1 As Form1

    Public Property Form1() As Form1
        Get
            Return Create(m_Form1)
        End Get
        Set (Value As Form1)
            If Value IsNot Nothing AndAlso Value IsNot m_Form1 Then
                Throw New ArgumentException( _
                    "Property can only be set to Nothing.")
            End If
            Dispose(m_Form1)
        End Set
    End Property
End Class
```

### <a name="extension-method-collection"></a>扩展方法集合

扩展方法的成员访问表达式`E.I`收集的收集的所有扩展方法名称`I`当前上下文中提供：

1. 首先，每个包含的表达式的嵌套的类型检查，从最里层开始，并转到最外面。
2. 然后，每个嵌套的命名空间被检查时，从最里层开始，然后转到最外层命名空间。
3. 然后，检查源文件中的导入。
4. 然后，将检查由编译环境定义的导入。

仅当没有扩大本机转换从目标表达式类型为第一个参数的类型的扩展方法收集的扩展方法。 并搜索与正则简单名称表达式绑定不同收集*所有*扩展方法; 找到的扩展方法时，集合不会停止。 例如：

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class


Namespace N1
    Module N1C1Extensions
        <Extension> _
        Sub M1(c As C1, x As Integer)
        End Sub
    End Module
End Namespace

Namespace N1.N2
    Module N2C1Extensions
        <Extension> _
        Sub M1(c As C1, y As Double)
        End Sub
    End Module
End Namespace

Namespace N1.N2.N3
    Module Test
        Sub Main()
            Dim x As New C1()

            ' Calls N1C1Extensions.M1
            x.M1(10)
        End Sub
    End Module
End Namespace
```

在此示例中，即使`N2C1Extensions.M1`之前找到`N1C1Extensions.M1`，它们都被视为作为扩展方法。 一旦收集的所有扩展方法，然后，他们就*科里化*。 科采用扩展方法调用的目标，并将其应用到扩展方法调用时，生成新的方法签名中删除 （因为它已指定） 的第一个参数。 例如：

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(x As Integer, y As Integer)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(x As Integer, y As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(y As Integer)
        '        Ext2.M(y As Double)
        v.M(10)
    End Sub
End Module
```

在上面的示例中，应用的扩充的结果`v`到`Ext1.M`是方法签名`Sub M(y As Integer)`。

除了删除扩展方法的第一个参数，科还会删除任何属于第一个参数的类型的方法类型参数。 类型推理时科具有方法类型参数的扩展方法，应用于第一个参数和结果固定推断出任何类型参数。 如果类型推理失败，此方法将被忽略。 例如：

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(Of T, U)(x As T, y As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(Of T)(x As T, y As T)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(Of U)(y As U)
        '        Ext2.M(y As Integer)
        v.M(10)
    End Sub
End Module
```

在上面的示例中，应用的扩充的结果`v`到`Ext1.M`是方法签名`Sub M(Of U)(y As U)`，因为类型形参`T`由于科推断出，现已修复。 因为类型形参`U`时不会推断出作为一部分科，它将保持打开的参数。 同样，因为类型形参`T`推断结果应用`v`到`Ext2.M`，参数类型`y`将成为固定为`Integer`。 它不将被推断为任何其他类型。 科签名，但所有约束时`New`约束也适用。 如果未满足，或依赖于未被推断为一部分科类型的约束，则忽略该扩展方法。 例如：

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T As Structure)(x As T, y As Integer)
    End Sub

    <Extension> _
    Sub M2(Of T As U, U)(x As T, y As U)
    End Sub
End Module

Module Main
    Sub Test()
        Dim s As String = "abc"

        ' Error: String does not satisfy the Structure constraint
        s.M1(10)

        ' Error: T depends on U, which cannot be inferred
        s.M2(10)
    End Sub
End Module
```

__请注意。__ 用于执行科的扩展方法的主要原因之一是迭代的它允许查询表达式推断的类型计算查询模式方法的参数之前。 由于查询模式的大多数方法采用 lambda 表达式，这需要类型推理本身，这极大地简化了查询表达式的计算过程。

与普通接口继承，不同扩展与另一个不相关的两个接口的扩展方法提供了，只要它们不具有相同的扩充的签名：

```vb
Imports System.Runtime.CompilerServices

Interface I1
End Interface

Interface I2
End Interface

Class C1
    Implements I1, I2
End Class

Module I1Ext
    <Extension> _
    Sub M1(i As I1, x As Integer)
    End Sub

    <Extension> _
    Sub M2(i As I1, x As Integer)
    End Sub
End Module

Module I2Ext
    <Extension> _
    Sub M1(i As I2, x As Integer)
    End Sub

    <Extension> _
    Sub M2(I As I2, x As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim c As New C1()

        ' Error: M is ambiguous between I1Ext.M1 and I2Ext.M1.
        c.M1(10)

        ' Calls I1Ext.M2
        c.M2(10)
    End Sub
End Module
```

最后，务必记住该方法被认为执行后期绑定时不扩展：

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a>字典成员访问表达式

一个*字典成员访问表达式*用于查找的集合的成员。 字典成员访问采用的格式`E!I`，其中`E`分类为一个值的表达式和`I`是一个标识符。

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

表达式的类型必须具有默认属性由单个索引`String`参数。 字典成员访问表达式`E!I`转换为表达式`E.D("I")`，其中`D`是默认属性`E`。 例如：

```vb
Class Keys
    Public ReadOnly Default Property Item(s As String) As Integer
        Get
            Return 10
        End Get
    End Property 
End Class

Module Test
    Sub Main()
        Dim x As Keys = new Keys()
        Dim y As Integer
        ' The two statements are equivalent.
        y = x!abc
        y = x("abc")
    End Sub
End Module
```

如果使用任何表达式中直接包含的表达式指定一个感叹号`With`假定语句。 如果没有包含`With`语句中，会发生编译时错误。


## <a name="invocation-expressions"></a>调用表达式

调用表达式由一个调用目标和一个可选参数列表组成。

```antlr
InvocationExpression
    : Expression ( OpenParenthesis ArgumentList? CloseParenthesis )?
    ;

ArgumentList
    : PositionalArgumentList
    | PositionalArgumentList Comma NamedArgumentList
    | NamedArgumentList
    ;

PositionalArgumentList
    : Expression? ( Comma Expression? )*
    ;

NamedArgumentList
    : IdentifierOrKeyword ColonEquals Expression
      ( Comma IdentifierOrKeyword ColonEquals Expression )*
    ;
```

目标表达式必须分类为方法组或其类型为委托类型的值。 如果目标表达式是委托类型，其类型的值，则调用表达式的目标会变为为方法组`Invoke`委托类型和目标表达式的成员将成为该方法的相关联的目标表达式组。

参数列表具有两个部分： 位置参数和命名自变量。 *位置参数*是表达式，必须在任何命名的参数之前。 *命名的自变量*开头的标识符，可以匹配关键字后, 跟`:=`和表达式。

如果方法组仅包含一个可访问的方法，包括实例和扩展方法，以及该方法不采用任何参数是一个函数、 方法组将被解释为表达式调用参数列表为空并结果是用作具有提供的参数列表的调用表达式的目标。 例如：

```vb
Class C1
    Function M1() As Integer()
        Return New Integer() { 1, 2, 3 }
    End Sub
End Class

Module Test
    Sub Main()
        Dim c As New C1()

        ' Prints 3
        Console.WriteLine(c.M1(2))
    End Sub
End Module
```

否则，重载决策被应用于方法，以便选择最适用的方法为给定的参数列表。 如果最适用的方法是函数，然后调用表达式的结果被归类为类型为该函数的返回类型的值。 如果最适用的方法是一个子例程，结果被归类为 void。 如果最适用的方法是没有正文的分部方法，然后调用表达式被忽略并且结果被归类为 void。

对于早期绑定调用表达式而言，目标方法中声明的相应参数的顺序计算这些实参。 对于后期绑定成员访问表达式，按它们在成员访问表达式中出现的顺序计算： 请参阅部分[Late 绑定表达式](expressions.md#late-bound-expressions)。

## <a name="overloaded-method-resolution"></a>解决方法的重载的方法：
重载解析、 特异性成员/类型指定一个参数的列表中，泛型，是否适用于参数列表、 传递参数和可选参数，条件方法的选择参数和类型自变量推理： 请参阅部分[重载决策](overload-resolution.md)。

## <a name="index-expressions"></a>索引表达式

*表达式创建索引*结果的数组元素中或进行了重新分类到属性访问的属性组。 索引表达式组成，在订单、 表达式、 左括号、 索引参数列表和右括号。

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

目标索引表达式必须分类为属性组或值。 索引表达式进行处理，如下所示：

* 如果目标表达式分类为一个值，并且其类型不是数组类型， `Object`，或`System.Array`，类型必须具有默认属性。 表示所有类型的默认属性的属性组上执行索引。 虽然不能用来声明在 Visual Basic 中的无参数的默认属性，但其他语言可能会允许声明此类属性。 因此，允许索引不带任何参数的属性。

* 如果表达式的结果值为数组类型，自变量列表中的参数数目必须与数组类型的秩的相同，并且可能不包括命名的参数。 如果是任何索引在运行时，无效`System.IndexOutOfRangeException`引发异常。 每个表达式必须是隐式转换为键入`Integer`。 索引表达式的结果是位于指定索引处的变量和分类为变量。

* 如果表达式分类为属性组中，重载决策用于确定是否其中一个属性是适用于索引参数列表。 如果属性组只包含一个属性具有`Get`访问器，如果该访问器不采用任何参数，则属性组将被解释为具有参数列表为空的索引表达式。 此结果用作目标的当前索引表达式。 如果任何属性不都适用，则出现编译时错误。 否则，表达式的结果具有相关联的目标表达式的属性组 （如果有） 的属性访问。

* 如果表达式分类为后期绑定属性组或值的类型`Object`或`System.Array`、 索引表达式的处理延迟到运行时和索引方式是后期绑定。 表达式将导致后期绑定属性访问类型化为`Object`。 相关联的目标表达式是目标表达式，如果它是一个值或属性组相关联的目标表达式。 在运行时表达式进行处理，如下所示：

* 如果表达式分类为后期绑定属性组，该表达式可能会导致方法组、 属性组或值 （如果该成员是一个实例或共享的变量）。 如果结果为方法组或属性组，重载决策被应用到组，以确定自变量列表的正确方法。 如果重载决策失败，`System.Reflection.AmbiguousMatchException`引发异常。 然后为属性访问或调用的形式处理结果并返回的结果。 如果该调用是子例程的则结果是`Nothing`。

* 如果目标表达式的运行时类型是数组类型或`System.Array`，索引表达式的结果是变量中指定索引处的值。

* 否则为该表达式的运行时类型必须具有默认属性和索引表示所有类型的默认属性的属性组上执行。 如果类型不具有默认属性，则`System.MissingMemberException`引发异常。


## <a name="new-expressions"></a>新的表达式

`New`运算符用于创建类型的新实例。 有四种形式的`New`表达式：

* 对象创建表达式用于创建类类型和值类型的新实例。

* 数组创建表达式用于创建数组类型的新实例。

* 委托创建表达式 （它们不具有从对象创建表达式不同语法） 用于创建类型的委托的新实例。

* 匿名对象创建表达式用于创建匿名类类型的新实例。

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

一个`New`表达式分类为一个值，结果是该类型的新实例。


### <a name="object-creation-expressions"></a>对象创建表达式

对象创建表达式用于创建类类型或结构类型的新实例。

```antlr
ObjectCreationExpression
    : 'New' NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )?
      ObjectCreationExpressionInitializer?
    ;

ObjectCreationExpressionInitializer
    : ObjectMemberInitializer
    | ObjectCollectionInitializer
    ;

ObjectMemberInitializer
    : 'With' OpenCurlyBrace FieldInitializerList CloseCurlyBrace
    ;

FieldInitializerList
    : FieldInitializer ( Comma FieldInitializer )*
    ;

FieldInitializer
    : 'Key'? ('.' IdentifierOrKeyword Equals )? Expression
    ;

ObjectCollectionInitializer
    : 'From' CollectionInitializer
    ;

CollectionInitializer
    : OpenCurlyBrace CollectionElementList? CloseCurlyBrace
    ;

CollectionElementList
    : CollectionElement ( Comma CollectionElement )*
    ;

CollectionElement
    : Expression
    | CollectionInitializer
    ;
```

对象创建表达式的类型必须是类类型、 结构类型或具有的类型参数`New`约束，不能为`MustInherit`类。 提供了窗体的一个对象创建表达式`New T(A)`，其中`T`是类类型或结构类型和`A`是一个可选参数列表中，重载决策确定正确的构造函数的`T`调用。 类型参数`New`约束被视为具有一个单一的无参数构造函数。 如果没有构造函数可调用时，会发生编译时错误;否则表达式的结果的新实例创建`T`使用所选的构造函数。 如果没有任何自变量，则可省略括号。

将分配一个实例取决于该实例是类类型或值类型。 `New` 类类型的实例上系统堆中，创建时直接在堆栈上创建值类型的新实例。

对象创建表达式可以选择后构造函数参数指定成员初始值设定项的列表。 这些成员初始值设定项使用关键字作为前缀`With`，而就像它是在上下文中的初始值设定项列表将被解释`With`语句。 例如，给定类：

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

代码：

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

是大致相当于：

```vb
Module Test
    Sub Main()
        Dim x, _t1 As Customer

        _t1 = New Customer()
        With _t1
            .Name = "Bob Smith"
            .Address = "123 Main St."
        End With

        x = _t1
    End Sub
End Module
```

每个初始值设定项必须指定一个名称以将分配，并且该名称必须是一个非`ReadOnly`实例变量或正在构造; 类型的属性的成员访问权限将不后期绑定正在构造的类型是否`Object`。 初始值设定项可能不会使用`Key`关键字。 在类型中的每个成员仅初始化一次。 初始值设定项表达式中，但是，可能会相互引用。 例如：

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

从左到右分配初始值设定项，因此如果初始值设定项引用尚未初始化的成员，它会看到任何值的实例变量构造函数运行后：

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

可以嵌套初始值设定项：

```vb
Class Customer
    Dim Name As String
    Dim Address As Address
    Dim Age As Integer
End Class

Class Address
    Dim Street As String
    Dim City As String
    Dim State As String
    Dim ZIP As String
End Class

Module Test
    Sub Main()
        Dim c As New Customer() With { _
            .Name = "John Smith", _
            .Address = New Address() With { _
                .Street = "23 Main St.", _
                .City = "Peoria", _
                .State = "IL", _
                .ZIP = "13934" }, _
            .Age = 34 }
    End Sub
End Module
```

如果正在创建的类型是集合类型和实例方法名为`Add`（包括扩展方法和共享的方法），然后对象创建表达式，可以指定由关键字作为前缀的集合初始值设定项`From`。 对象创建表达式不能指定成员初始值设定项和集合初始值设定项。 集合初始值设定项中的每个元素作为参数传递给调用`Add`函数。 例如：

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

等效于：

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

如果元素是集合初始值设定项本身，将作为的单个参数传递的子集合初始值设定项的每个元素`Add`函数。 例如，以下内容：

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

等效于：

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

此扩展始终执行且只执行一个层次深度;之后，子初始值设定项被视为数组文本。 例如：

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a>数组表达式

一个数组表达式用于创建数组类型的新实例。 有两种类型的数组表达式： 数组创建表达式和数组文本。

#### <a name="array-creation-expressions"></a>数组创建表达式

如果提供数组大小初始化修饰符，则生成的数组类型派生的数组大小初始化参数列表中删除每个单独的自变量。 每个自变量的值确定的新分配的数组实例中的相应维度的上限。 如果表达式具有非空集合初始值设定项，自变量列表中的每个参数必须是常量，并且指定的表达式列表的秩和维度长度必须匹配集合初始值设定项。

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

如果未提供数组大小初始化修饰符，然后类型名称必须是数组类型和集合初始值设定项必须为空或具有相同的为指定的数组类型的秩的嵌套级别数。 所有中最内部的嵌套级别的元素必须是隐式转换为数组的元素类型，并且必须分类为一个值。 每个嵌套的集合初始值设定项中的元素数必须始终为与同一级别的其他集合的大小保持一致。 单独的维度长度推断出的每个相应的嵌套级别的集合初始值设定项中的元素数。 如果集合初始值设定项为空，每个维的长度为零。

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

集合初始值设定项的最外层嵌套级别对应的数组，最左边的维度，最内部的嵌套级别对应于最右边的维度。 下面的示例：

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

等效于以下：

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

如果集合初始值设定项为空 （即，一个包含大括号），但没有初始值设定项列表，边界已知的正在初始化数组的维数，空集合初始值设定项表示的指定大小的数组实例其中的所有元素具有已都初始化为元素类型的默认值。 如果不知道正在初始化数组的维数的边界，空集合初始值设定项表示在其中所有维度都都为大小为零的数组实例。

实例的整个生存期内是常量数组实例的级别和每个维的长度。 换而言之，不能更改现有数组实例，秩也不可能以调整其尺寸。

#### <a name="array-literals"></a>数组文本

数组文本表示一个数组，其元素类型、 秩和界限进行推断表达式上下文和集合初始值设定项的组合。 这部分所述[表达式重新分类](expressions.md#expression-reclassification)。

```antlr
ArrayExpression
    : ArrayCreationExpression
    | ArrayLiteralExpression
    ;

ArrayCreationExpression
    : 'New' NonArrayTypeName ArrayNameModifier CollectionInitializer
    ;

ArrayLiteralExpression
    : CollectionInitializer
    ;
```

例如：

```vb
' array of integers
Dim a = {1, 2, 3}

' array of shorts
Dim b = {1S, 2S, 3S}

' array of shorts whose type is taken from the context
Dim c As Short() = {1, 2, 3}

' array of type Integer(,)
Dim d = {{1, 0}, {0, 1}}

' jagged array of rank ()()
Dim e = {({1, 0}), ({0, 1})}

' error: inconsistent rank
Dim f = {{1}, {2, 3}}

' error: inconsistent rank
Dim g = {1, {2}}
```

格式和集合初始值设定项数组文本中的要求是完全相同的集合初始值设定项中的数组创建表达式。

__请注意。__ 数组文本不会创建该数组本身;相反，它是值会导致数组创建表达式的重新分类。 例如，转换`CType(new Integer() {1,2,3}, Short())`由于没有从转换不可能`Integer()`到`Short()`; 但表达式`CType({1,2,3},Short())`可能是因为它首先进行了重新分类数组文本到数组创建表达式`New Short() {1,2,3}`.


### <a name="delegate-creation-expressions"></a>委托创建表达式

委托创建表达式用于创建委托类型的新实例。 委托创建表达式的参数必须属于方法指针或 lambda 方法的表达式。

如果参数为方法的指针，由方法指针所引用的方法之一必须是适用于委托类型的签名。 一种方法`M`适用于委托类型`D`如果：

* `M` 不是`Partial`或具有一个主体。

* 这两`M`并`D`是函数，或`D`是一个子例程。

* `M` 和`D`具有相同数量的参数。

* 参数类型`M`每个都可以从相应参数类型的类型的类型转换`D`，和其修饰符 (即`ByRef`， `ByVal`) 匹配。

* 返回类型`M`，如果任何，具有到的返回类型的转换`D`。

如果方法指针引用后期绑定访问，然后被假定后期绑定访问可具有与委托类型相同数量的参数的函数。

如果未使用严格的语义和方法指针，所引用的只有一个方法，但不适用由于这一事实，它具有无参数和委托类型的作用，则此方法被认为适用的参数或返回值重新将被忽略。 例如：

```vb
Delegate Sub F(x As Integer)

Module Test
    Sub M()
    End Sub

    Sub Main()
        ' Valid
        Dim x As F = AddressOf M
    End Sub
End Module
```

__请注意。__ 由于扩展方法不使用严格的语义时，仅允许这一放宽。 因为如果正则方法不适用，仅被视为扩展方法，就可以使用任何参数，隐藏具有用于委托构造参数的扩展方法的实例方法。

如果有多个方法指针所引用的一种方法是适用于该委托类型，然后使用重载解析的候选方法之间进行选取。 委托的参数的类型用作出于重载解决方法的参数的类型。 如果没有一种方法的候选是最适用，则发生编译时错误。 在以下示例中，本地变量初始化的委托来指的是第二个`Square`方法因为该方法更适用于签名和返回类型的`DoubleFunc`。

```vb
Delegate Function DoubleFunc(x As Double) As Double

Module Test
    Function Square(x As Single) As Single
        Return x * x
    End Function 

    Function Square(x As Double) As Double
        Return x * x
    End Function

    Sub Main()
        Dim a As New DoubleFunc(AddressOf Square)
    End Sub
End Module
```

具有第二个`Square`尚未存在的方法，第一个`Square`已选择方法。 如果通过编译环境或通过指定严格的语义`Option Strict`，然后如果方法指针所引用的最具体方法是窄于委托签名会发生编译时错误。 一种方法`M`被视为比委托类型窄`D`如果：

* 参数类型为`M`已扩大转换的相应参数类型为`D`。

* 或者，返回类型，如果有的`M`收缩转换的返回类型为`D`。

如果类型参数与方法指针相关联，被视为仅具有相同数量的类型参数的方法。 如果没有类型参数与方法指针相关联，匹配对泛型方法的签名时是否使用类型推理。 与其他普通类型推理，不同的委托的返回类型推断类型参数时使用，但仍确定至少泛型重载时不考虑返回类型。 下面的示例显示了这两种提供类型实参于委托创建表达式：

```vb
Delegate Function D(s As String, i As Integer) As Integer
Delegate Function E() As Integer

Module Test
    Public Function F(Of T)(s As String, t1 As T) As T
    End Function

    Public Function G(Of T)() As T
    End Function

    Sub Main()
        Dim d1 As D = AddressOf f(Of Integer)    ' OK, type arg explicit
        Dim d2 As D = AddressOf f                ' OK, type arg inferred

        Dim e1 As E = AddressOf g(Of Integer)    ' OK, type arg explicit
        Dim e2 As E = AddressOf g                ' OK, infer from return
  End Sub
End Module
```

在上述示例中，非泛型委托类型是使用泛型方法实例化。 还有可能要创建使用泛型方法的构造的委托类型的实例。 例如：

```vb
Delegate Function Predicate(Of U)(u1 As U, u2 As U) As Boolean

Module Test
    Function Compare(Of T)(t1 As List(of T), t2 As List(of T)) As Boolean
        ...
    End Function

    Sub Main()
        Dim p As Predicate(Of List(Of Integer))
        p = AddressOf Compare(Of Integer)
    End Sub
End Module
```

如果委托创建表达式的参数是 lambda 方法，lambda 方法必须是适用于委托类型的签名。 Lambda 方法`L`适用于委托类型`D`如果：

* 如果`L`有参数，`D`具有相同数量的参数。 (如果`L`没有参数的参数`D`将被忽略。)

* 参数类型`L`每个都可以转换为相应参数类型的类型的类型`D`，和其修饰符 (即`ByRef`， `ByVal`) 匹配。

* 如果`D`是一个函数的返回类型`L`转换为的返回类型`D`。 (如果`D`是一个子例程的返回值`L`将被忽略。)

如果参数的类型参数`L`省略，则中的相应参数的类型`D`推断; 如果参数的`L`具有数组或名称可为 null 的修饰符，导致编译时错误。 一次所有的参数类型`L`都可用，则推断 lambda 方法中的表达式的类型。 例如：

```vb
Delegate Function F(x As Integer, y As Long) As Long

Module Test
    Sub Main()
        ' b inferred to Integer, c and return type inferred to Long
        Dim a As F = Function(b, c) b + c

        ' e and return type inferred to Integer, f inferred to Long
        Dim d As F = Function(e, f) e + CInt(f)
    End Sub
End Module
```

在某些情况下，其中委托签名不 lambda 方法或方法签名完全匹配，.NET Framework 可能不支持委托创建本机。 在这种情况下，lambda 方法表达式用于匹配两种方法。 例如：

```vb
Delegate Function IntFunc(x As Integer) As Integer

Module Test
    Function SquareString(x As String) As String
        Return CInt(x) * CInt(x)
    End Function 

    Sub Main()
        ' The following two lines are equivalent
        Dim a As New IntFunc(AddressOf SquareString)
        Dim b As New IntFunc( _
            Function(x As Integer) CInt(SquareString(CStr(x))))
    End Sub
End Module
```

委托创建表达式的结果是一个委托实例，从方法指针表达式是指与相关联的目标表达式 （如果有） 匹配的方法。 如果目标表达式的类型为值类型，则值类型被复制到系统堆上因为委托仅指向堆上对象的方法。 方法和委托所引用的对象的委托的整个生存期内保持常量。 换而言之，不能创建它后更改目标或委托的对象。

### <a name="anonymous-object-creation-expressions"></a>匿名对象创建表达式

具有成员初始值设定项的对象创建表达式还可以完全忽略类型名称。

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

在这种情况下，匿名类型被构造基于的类型和初始化表达式的一部分的成员的名称。 例如：

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

创建由匿名对象创建表达式的类型是没有名称，直接继承的类`Object`，，具有一组具有相同的名称分配给成员初始值设定项列表中的成员的属性。 使用相同的规则作为本地变量的类型推理来推断每个属性的类型。 生成的匿名类型还重写`ToString`，返回所有成员及其值的字符串表示形式。 （此字符串的确切格式超出了本规范的范畴是）。

默认情况下，生成的匿名类型的属性是读写。 可以使用将标记为只读的匿名类型属性`Key`修饰符。 `Key`修饰符指定该字段可用于唯一地标识匿名类型表示的值。 除了使该属性只读的它还会导致匿名类型来重写`Equals`并`GetHashCode`和实现接口`System.IEquatable(Of T)`(匿名类型中用于填充`T`)。 成员定义，如下所示：

`Function Equals(obj As Object) As Boolean` 并`Function Equals(val As T) As Boolean`实现可以通过验证的两个实例都属于相同类型和并将每个比较`Key`使用成员`Object.Equals`。 如果所有`Key`成员是否相等，然后`Equals`返回`True`; 否则为`Equals`返回`False`。

`Function GetHashCode() As Integer` 实现这样，如果`Equals`为 true 的两个实例的匿名类型，则`GetHashCode`将返回相同的值。 哈希启动带有种子值，然后为每个`Key`成员，按顺序相乘 31 的哈希，并添加`Key`成员的哈希值 (由提供`GetHashCode`) 如果成员不是引用类型或值为可以为null值类型`Nothing`.

例如，在语句中创建的类型：

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

创建一个类，如下所示大约 （尽管确切的实现可能会有所不同）：

```vb
Friend NotInheritable Class $Anonymous1
    Implements IEquatable(Of $Anonymous1)

    Private ReadOnly _zipCode As Integer
    Private _state As String

    Public Sub New(zipCode As Integer, state As String)
        _zipCode = zipcode
        _state = state
    End Sub

    Public ReadOnly Property ZipCode As Integer
        Get
            Return _zipCode
        End Get
    End Property

    Public Property State As String
        Get
            Return _state
        End Get
        Set (value As Integer)
            _state = value
        End Set
    End Property

    Public Overrides Function Equals(obj As Object) As Boolean
        Dim val As $Anonymous1 = TryCast(obj, $Anonymous1)
        Return Equals(val)
    End Function

    Public Overloads Function Equals(val As $Anonymous1) As Boolean _
        Implements IEquatable(Of $Anonymous1).Equals

        If val Is Nothing Then 
            Return False
        End If

        If Not Object.Equals(_zipCode, val._zipCode) Then 
            Return False
        End If

        Return True
    End Function

    Public Overrides Function GetHashCode() As Integer
        Dim hash As Integer = 0

        hash = hash Xor _zipCode.GetHashCode()

        Return hash
    End Function

    Public Overrides Function ToString() As String
        Return "{ Key .ZipCode = " & _zipCode & ", .State = " & _state & " }"
    End Function
End Class
```

若要简化从另一种类型的字段创建了匿名类型的情况，可以直接从以下情况中的表达式推断字段名称：

* 简单名称表达式`x`推断名称`x`。

* 成员访问表达式`x.y`推断名称`y`。

* 字典查找表达式`x!y`推断名称`y`。

* 不带任何参数的调用或索引表达式`x()`推断名称`x`。

* XML 成员访问表达式`x.<y>`， `x...<y>`，`x.@y`推断名称`y`。

* 为目标的成员访问表达式的 XML 成员访问表达式`x.<y>.z`推断名称`z`。

* 为不带任何参数调用或索引表达式的目标的 XML 成员访问表达式`x.<y>.z()`推断名称`z`。

* 为目标的一个调用或索引表达式的 XML 成员访问表达式`x.<y>(0)`推断名称`y`。

初始值设定项被解释为赋值的表达式推断名称。 例如，以下初始值设定项是等效的：

```vb
Class Address
    Public Street As String
    Public City As String
    Public State As String
    Public ZIP As String
End Class

Class C1
    Sub Test(a As Address)
        Dim cityState1 = New With { .City = a.City, .State = a.State }
        Dim cityState2 = New With { a.City, a.State }
    End Sub
End Class
```

如果成员名称推断出的与冲突的现有成员的类型，如`GetHashCode`，则会发生编译时错误。 与常规成员初始值设定项，不同匿名对象创建表达式不允许成员初始值设定项具有循环引用，或之前已初始化引用成员。 例如：

```vb
Module Test
    Sub Main()
        ' Error: Circular references
        Dim x = New With { .a = .b, .b = .a }

        ' Error: Referring to .b before it has been assigned to
        Dim y = New With { .a = .b, .b = 10 }

        ' Error: Referring to .a before it has been assigned to
        Dim z = New With { .a = .a }
    End Sub
End Module
```

如果两个匿名类创建表达式相同的方法内发生，并且如果的属性顺序、 属性名称和属性类型的所有匹配项-产生相同结果的形状-它们将同时引用同一个匿名类。 实例或共享的成员变量的初始值设定项的方法作用域是在其中的变量进行初始化的构造函数。

__请注意。__ 可以一个编译器可以选择统一匿名类型进一步，例如在程序集级别，但这不能依赖这一次。


## <a name="cast-expressions"></a>强制转换表达式

强制转换表达式强制转换为给定类型的表达式。 特定强制转换关键字强制转换为基元类型的表达式。 三个常规转换关键字， `CType`，`TryCast`和`DirectCast`，转换为类型的表达式强制转换。

```antlr
CastExpression
    : 'DirectCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'TryCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'CType' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | CastTarget OpenParenthesis Expression CloseParenthesis
    ;

CastTarget
    : 'CBool' | 'CByte' | 'CChar'  | 'CDate'  | 'CDec' | 'CDbl' | 'CInt'
    | 'CLng'  | 'CObj'  | 'CSByte' | 'CShort' | 'CSng' | 'CStr' | 'CUInt'
    | 'CULng' | 'CUShort'
    ;
```

`DirectCast` 和`TryCast`有特殊的行为。 因此，它们仅支持本机的转换。 此外中的目标类型`TryCast`表达式不能是值类型。 用户定义的转换运算符时不考虑`DirectCast`或`TryCast`使用。 (__注意。__ 转换设置`DirectCast`和`TryCast`支持受到限制，因为它们实现"本机 CLR"的转换。 目的`DirectCast`是提供"取消装箱"指令时的用途的功能`TryCast`是提供"isinst"指令的功能。 因为它们映射到 CLR 指令，支持不直接支持的 CLR 的转换，将无法实现的预期的用途。）

`DirectCast` 将转换为键入的表达式`Object`处理方式不同于`CType`。 转换类型的表达式时`Object`其运行时类型是基元值类型，`DirectCast`将引发`System.InvalidCastException`异常，如果指定的类型不是表达式的运行时类型相同或`System.NullReferenceException`如果表达式计算结果为`Nothing`。 (__注意。__ 如上所述，`DirectCast`映射到 CLR 指令直接"取消装箱"当表达式的类型为`Object`。 与此相反，`CType`将转换为运行时帮助器调用以执行转换，以便可以支持基元类型之间进行转换。 在这种情况时`Object`表达式要转换为基元值类型和实际的实例匹配的类型的目标类型，`DirectCast`将明显快于`CType`。)

`TryCast` 将表达式转换，但不会引发异常，如果不能转换为目标类型的表达式。 相反，`TryCast`将导致`Nothing`如果不能在运行时转换的表达式。 (__注意。__ 如上所述，`TryCast`映射直接到 CLR 指令"isinst"。 通过组合类型检查和转换为单个操作中，`TryCast`可以是这样做比便宜`TypeOf ... Is`，然后`CType`。)

例如：

```vb
Interface ITest
    Sub Test()
End Interface

Module Test
    Sub Convert(o As Object)
        Dim i As ITest = TryCast(o, ITest)

        If i IsNot Nothing Then
            i.Test()
        End If
    End Sub
End Module
```

如果不存在从转换表达式的类型为指定的类型，发生编译时错误。 否则为该表达式分类为一个值，结果是由转换生成的值。


## <a name="operator-expressions"></a>运算符表达式

有两种类型的运算符。 *一元运算符*采用一个操作数，并使用前缀表示法 (例如， `-x`)。 *二元运算符*采用两个操作数，并使用中缀表示法 (例如， `x + y`)。 关系运算符，但这始终导致`Boolean`，为特定类型会导致该类型定义的运算符。 运算符的操作数必须始终分类为一个值;运算符表达式的结果分类为一个值。

```antlr
OperatorExpression
    : ArithmeticOperatorExpression
    | RelationalOperatorExpression
    | LikeOperatorExpression
    | ConcatenationOperatorExpression
    | ShortCircuitLogicalOperatorExpression
    | LogicalOperatorExpression
    | ShiftOperatorExpression
    | AwaitOperatorExpression
    ;
```

### <a name="operator-precedence-and-associativity"></a>运算符优先级和关联性

如果表达式包含多个二进制运算符*优先*运算符的控制单个二进制运算符的计算顺序。 例如，表达式 `x + y * z` 相当于计算 `x + (y * z)`，因为 `*` 运算符的优先级高于 `+` 运算符。 下表列出了按降序的优先顺序的二进制运算符：


| __类别__     | __运算符__                                          | 
|------------------|--------------------------------------------------------|
| 基本          | 所有非运算符表达式                           |
| Await            | `Await`                                                |
| 求幂   | `^`                                                    |
| 一元求反   | `+`， `-`                                               |
| 乘法   | `*`， `/`                                               |
| 整数除法 | `\`                                                    |
| 取模          | `Mod`                                                  |
| 加法         | `+`， `-`                                               |
| 串联    | `&`                                                    |
| 移位            | `<<`， `>>`                                             |
| 关系       | `=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot` |
| 逻辑“非”      | `Not`                                                  |
| 逻辑“与”      | `And`， `AndAlso`                                       |
| 逻辑“或”       | `Or`， `OrElse`                                         |
| 逻辑 XOR      | `Xor`                                                  |

如果表达式包含两个运算符具有相同优先级*关联性*决定执行的操作的顺序。 所有二元运算符是左结合，即操作执行从左到右。 使用带括号的表达式可以控制优先级和结合性。

### <a name="object-operands"></a>对象操作数

除了常规的类型支持的每个运算符，所有运算符都支持类型的操作数`Object`。 运算符应用于`Object`操作数进行处理同样上进行方法调用`Object`值： 可能选择的后期绑定方法调用，在这种情况下运行时类型的操作数，而不是编译时类型，确定有效性和操作的类型。 如果通过编译环境或通过指定严格的语义`Option Strict`，任何类型的操作数的运算符`Object`导致编译时错误，除了`TypeOf...Is`，`Is`和`IsNot`运算符。

当运算符决策确定后期绑定应执行操作时，操作的结果是操作数的运行时类型是否支持的运算符类型的操作数类型应用运算符的结果。 值`Nothing`视为二元运算符表达式中另一个操作数的类型的默认值。 在一元运算符表达式中，或如果两个操作数都`Nothing`在二元运算符表达式中，该操作的类型是`Integer`或唯一的运算符，如果该运算符不会导致的结果类型`Integer`。 操作的结果始终然后转换回`Object`。 如果操作数类型有没有有效的运算符，`System.InvalidCastException`引发异常。 在运行时转换已完成而不考虑它们是隐式或显式转换。

如果数值的二元运算的结果将生成溢出异常 （无论整数溢出检查是否打开或关闭），然后结果类型被提升到下一步的更广泛数值类型，如有可能。 例如，考虑以下代码：

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

它将打印以下结果：

```
System.Int16 = 512
```

如果没有更广泛的数值类型可用来存储数，`System.OverflowException`引发异常。

### <a name="operator-resolution"></a>运算符决策

运算符解析给定运算符类型和操作数的一组，确定要使用的操作数的运算符。 在解析运算符，则用户定义的运算符将被视为第一，使用以下步骤：

1. 首先，收集所有候选运算符。 候选运算符是所有用户定义类型的运算符的特定运算符中的源类型和所有目标类型中的特定类型的用户定义的运算符。 如果源类型和目标类型相关，常见的运算符是仅视为一次。

2. 然后，重载决策被应用于要选择的最特定运算符的运算符和操作数。 在二元运算符的情况下这可能会导致后期绑定调用。

收集类型的候选运算符时`T?`，类型的运算符的`T`改为使用。 任一`T`的用户定义还提升涉及只有不可为 null 的值类型的运算符。 提升的运算符使用的任何值类型，可以为 null 的版本与异常的返回类型`IsTrue`并`IsFalse`(它必须是`Boolean`)。 提升的运算符计算通过将操作数转换为其不可为 null 的版本，然后评估用户定义的运算符，然后将结果转换键入到其可以为 null 的版本。 如果经不起考验操作数是`Nothing`，该表达式的结果是值为`Nothing`类型为可以为 null 的类型版本的结果。 例如：

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Operator +(ByVal op1 As S, ByVal op2 As T) As T
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As S?
        Dim y, z As T?

        ' Valid, as S + T = T is lifted to S? + T? = T?
        z = x + y 
    End Sub
End Module
```

如果运算符是二元运算符，其中一个操作数是引用类型，还提升运算符，但任何绑定到该运算符会产生错误。 例如：

```vb
Structure S1
    Public F1 As Integer

    Public Shared Operator +(left As S1, right As String) As S1
       ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim a? As S1
        Dim s As String
        
        ' Error: '+' is not defined for S1? and String
        a = a + s
    End Sub
End Module
```

__请注意。__ 此规则存在是因为我们想要在未来版本中，将用例在两种类型的二元运算符的情况下的行为中添加空传播引用类型是否已被考虑。

使用转换，如用户定义的运算符始终是首选通过提升运算符。

在解析重载运算符，可在 Visual Basic 中定义的类和其他语言中定义的那些之间的差异：

* 在其他语言中， `Not`， `And`，和`Or`可能同时作为逻辑运算符和位运算符重载。 在从外部程序集导入时，这些运算符的任一形式接受为有效的重载。 但是，对于类型，用于定义逻辑和位运算符，将视为仅按位的实现。

* 在其他语言中，`>>`和`<<`可能同时作为已签名的运算符和无符号的运算符重载。 在从外部程序集导入时，两种形式接受为有效的重载。 但是，定义了符号和无符号的运算符的类型，将视为仅有符号的实现。

* 如果没有用户定义的运算符是最具体到操作数，然后将被视为内部运算符。 如果没有内部运算符的操作数定义和其中一个操作数具有类型为 Object，则运算符将会解决后期绑定;否则，会导致编译时错误。

在以前版本的 Visual Basic 中，如果有且只有一个操作数的类型为 Object，和任何适用的用户定义运算符和任何适用的内部运算符，然后它时出现错误。 截至 Visual Basic 11，它现在解析后期绑定。 例如：

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

一种类型`T`具有内部函数运算符还定义了该相同运算符`T?`。 在运算符的结果`T?`将与相同`T`，只不过其中一个操作数是`Nothing`，运算符的结果将是`Nothing`（即传播 null 值）。 用于解析的操作的类型的目的`?`中删除具有它们的任何操作数，从确定操作的类型，和一个`?`如果任何操作数为 null 的值类型将添加到操作的类型。 例如：

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

每个运算符列出了为定义的内部类型和给定的操作数类型执行的操作的类型。 类型的内部操作的结果都遵循以下通用规则：

* 如果所有操作数都是相同的类型，并为类型定义运算符，然后会进行任何转换，并使用该类型的运算符。

* 使用以下步骤转换任何其类型未定义的运算符的操作数和运算符是根据新的类型解析：

  * 操作数将转换为下一步为运算符和操作数定义的最宽类型和所隐式转换。

  * 如果不存在此类的类型，然后操作数将转换为下一步为运算符和操作数定义的最小类型和所隐式转换。

  * 如果没有这样的类型，或者不能发生转换，发生编译时错误。

* 否则，将操作数转换为使用更广泛的操作数类型和该类型的运算符。 如果更窄的操作数类型不能隐式转换为更广泛的运算符类型，会发生编译时错误。

尽管这些常规规则，但是，有许多特殊情况下调用运算符的结果表中。

__请注意。__ 因格式原因而，运算符类型表将简化到其前两个字符的预定义的名称。 因此"通过"是`Byte`，"UI"是`UInteger`，"St"是`String`，等等。"Err"意味着没有为给定的操作数类型定义操作。

## <a name="arithmetic-operators"></a>算术运算符

`*`， `/`， `\`， `^`， `Mod`， `+`，以及`-`运算符是*算术运算符*。

```antlr
ArithmeticOperatorExpression
    : UnaryPlusExpression
    | UnaryMinusExpression
    | AdditionOperatorExpression
    | SubtractionOperatorExpression
    | MultiplicationOperatorExpression
    | DivisionOperatorExpression
    | ModuloOperatorExpression
    | ExponentOperatorExpression
    ;
```

可能会较高的精度比该操作的结果类型与执行浮点算术运算。 例如，某些硬件体系结构支持"扩展"或"长双精度"浮点类型，具有更大的范围和精度比`Double`类型，并且隐式地执行所有使用此较高精度类型的浮点运算。 可以进行硬件体系结构来执行浮点运算仅在开销过大的精度降低性能;而不是需要实现只好性能和精度，Visual Basic 允许使用所有浮点操作的更高精度类型。 不提供更精确的结果，这很少会有任何显著的影响。 但是，在窗体的表达式`x * y / z`，其中该乘法运算生成结果超出`Double`范围，但后面的除法使临时结果返回到`Double`范围，该表达式是这一事实在较高范围中计算格式可能会导致有限生成而不是无限的结果。


### <a name="unary-plus-operator"></a>一元加号运算符

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

为定义一元加运算符`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Single`， `Double`，和`Decimal`类型.

__操作类型：__


| __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | 间距 | Sh | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该 | Ob | 


### <a name="unary-minus-operator"></a>一元负运算符

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

一元负运算符定义为以下类型：

`SByte`、 `Short`、 `Integer`和 `Long`。 通过减去从零开始的操作数计算结果。 如果整数溢出检查已打开并操作数的值是最大负`SByte`， `Short`， `Integer`，或`Long`、`System.OverflowException`引发异常。 否则为如果操作数的值为最大负`SByte`， `Short`， `Integer`，或`Long`，结果是该相同的值，则不会报告溢出。

`Single` 和 `Double`。 结果是操作数的符号相反，包括值 0 和无穷大的值。 如果操作数为 NaN，则结果也为 NaN。

`Decimal`。 通过减去从零开始的操作数计算结果。

__操作类型：__

| __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 


### <a name="addition-operator"></a>加法运算符

加法运算符计算两个操作数之和。

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

加法运算符定义为以下类型：

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。 如果整数溢出检查位于和为结果类型的范围之外`System.OverflowException`引发异常。 否则为不报告溢出，并且结果的任何重要的高顺序位将被丢弃。

* `Single` 和 `Double`。 根据 IEEE 754 算法的规则计算总和。

* `Decimal`。 如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。 如果结果值过小，以十进制格式表示，则结果为 0。

* `String`。 这两个`String`操作数会连接在一起。

* `Date`。 `System.DateTime`类型定义重载的加法运算符。 因为`System.DateTime`等效于内部`Date`类型，这些运算符，还可以在`Date`类型。

__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __通过__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __US__ |    |    |    |    | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __De__ |    |    |    |    |    |    |    |    |    | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该 | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该 | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | St  | Err | St | Ob | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | St  | St | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | St | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


### <a name="subtraction-operator"></a>减法运算符

减法运算符中减去第二个操作数，从第一个操作数。

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

减法运算符定义为以下类型：

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。 如果整数溢出检查已打开并之处在于结果类型的范围之外`System.OverflowException`引发异常。 否则为不报告溢出，并且结果的任何重要的高顺序位将被丢弃。

* `Single` 和 `Double`。 根据 IEEE 754 算法的规则计算差。

* `Decimal`。 如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。 如果结果值过小，以十进制格式表示，则结果为 0。

* `Date`。 `System.DateTime`类型定义重载的减法运算符。 因为`System.DateTime`等效于内部`Date`类型，这些运算符，还可以在`Date`类型。

__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __通过__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __US__ |    |    |    |    | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __De__ |    |    |    |    |    |    |    |    |    | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="multiplication-operator"></a>乘法运算符

乘法运算符计算两个操作数的乘积。

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

乘法运算符定义为以下类型：

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。 如果整数溢出检查，且该产品是结果类型的范围之外`System.OverflowException`引发异常。 否则为不报告溢出，并且结果的任何重要的高顺序位将被丢弃。

* `Single` 和 `Double`。 根据 IEEE 754 算法的规则计算乘积。

* `Decimal`。 如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。 如果结果值过小，以十进制格式表示，则结果为 0。

__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __通过__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __US__ |    |    |    |    | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __De__ |    |    |    |    |    |    |    |    |    | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="division-operators"></a>除法运算符

除法运算符计算两个操作数的商。 有两个除法运算符: （浮点） 的正则除法运算符和整数除法运算符。

```antlr
DivisionOperatorExpression
    : FPDivisionOperatorExpression
    | IntegerDivisionOperatorExpression
    ;

FPDivisionOperatorExpression
    : Expression '/' LineTerminator? Expression
    ;

IntegerDivisionOperatorExpression
    : Expression '\\' LineTerminator? Expression
    ;
```

正则除法运算符定义为以下类型：

* `Single` 和 `Double`。 根据 IEEE 754 算法的规则计算商。

* `Decimal`。 如果右操作数的值为零，`System.DivideByZeroException`引发异常。 如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。 如果结果值过小，以十进制格式表示，则结果为零。 结果，任何舍入之前, 的小数位数是首选缩放的保留结果等于确切结果最接近的规模。  首选的小数位数是第一个操作数小于第二个操作数的小数位数的小数位数。

根据常规运算符解析规则，纯粹之间操作数的类型，如进行常规分隔`Byte`， `Short`， `Integer`，和`Long`会导致两个操作数转换为类型`Decimal`。 但是，在这两种类型时进行除法运算符在运算符解析`Decimal`，`Double`被视为窄于`Decimal`。 因为遵循此约定`Double`部门是比效率更高`Decimal`除法。

__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __通过__ |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __US__ |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | 应该 | 应该 | 应该 | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | 应该 | 应该 | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | 应该 | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __De__ |    |    |    |    |    |    |    |    |    | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 

为定义整数除法运算符`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`，和`Long`。 如果右操作数的值为零，`System.DivideByZeroException`引发异常。 该除法运算将舍入到零，结果和结果的绝对值是小于两个操作数的商的绝对值的最大可能整数。 两个操作数具有相同的符号和零或负数时两个操作数具有符号相反时，则结果为零或正数。 如果左的操作数是最大负`SByte`， `Short`， `Integer`，或`Long`，和右操作数是`-1`，则发生溢出; 如果整数溢出检查位于`System.OverflowException`引发异常。 否则为不报告溢出，结果而是为左操作数的值。

__请注意。__ 无符号类型的两个操作数始终为零或正数，结果始终是零或正数。  因为该表达式的结果将始终小于或等于最大的两个操作数，不能发生溢出。  这种情况下整数溢出检查不是执行使用两个无符号整数整除。 结果是与左操作数具有类型。


__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __通过__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __US__ |    |    |    |    | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __De__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="mod-operator"></a>Mod 运算符

`Mod` （取模） 运算符计算两个操作数相除的余数。

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

`Mod`运算符定义为以下类型：

* `Byte``SByte`， `UShort`， `Short`， `UInteger`， `Integer`，`ULong`和`Long`。 结果`x Mod y`由生成的值`x - (x \ y) * y`。 如果`y`为零，`System.DivideByZeroException`引发异常。 取模运算符永远不会导致溢出。

* `Single` 和 `Double`。 根据 IEEE 754 算法的规则计算余数。

* `Decimal`。 如果右操作数的值为零，`System.DivideByZeroException`引发异常。 如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。 如果结果值过小，以十进制格式表示，则结果为零。

__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __通过__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __US__ |    |    |    |    | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | de | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __De__ |    |    |    |    |    |    |    |    |    | de | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="exponentiation-operator"></a>求幂运算符

求幂运算符计算的次幂的第二个操作数的第一个操作数。

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

为类型定义求幂运算符`Double`。 根据 IEEE 754 算法的规则计算的值。

__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __通过__ |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __US__ |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __De__ |    |    |    |    |    |    |    |    |    | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="relational-operators"></a>关系运算符

*关系运算符*比较到一个其他的值。 比较运算符不`=`， `<>`， `<`， `>`， `<=`，并`>=`。

```antlr
RelationalOperatorExpression
    : Expression '=' LineTerminator? Expression
    | Expression '<' '>' LineTerminator? Expression
    | Expression '<' LineTerminator? Expression
    | Expression '>' LineTerminator? Expression
    | Expression '<' '=' LineTerminator? Expression
    | Expression '>' '=' LineTerminator? Expression
    ;
```

所有关系运算符导致`Boolean`值。

关系运算符具有以下常规含义：

* `=`运算符测试两个操作数是否相等。

* `<>`运算符测试两个操作数是否不相等。

* `<`运算符测试第一个操作数是否小于第二个操作数。

* `>`运算符测试第一个操作数是否大于第二个操作数。

* `<=`运算符测试第一个操作数是否小于或等于第二个操作数。

* `>=`运算符测试第一个操作数是否大于或等于第二个操作数。

关系运算符定义为以下类型：

* `Boolean`。 运算符比较两个操作数的真值。 `True` 被视为小于`False`，可使用其数字值匹配。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。 运算符比较两个整数操作数的数值。

* `Single` 和 `Double`。 这些运算符来比较根据 IEEE 754 标准规则操作数。

* `Decimal`。 运算符比较两个十进制操作数的数值。

* `Date`。 运算符将返回两个日期/时间值的比较结果。

* `Char`。 运算符将返回两个的 Unicode 值的比较结果。

* `String`。 运算符将返回使用二进制比较或文本比较两个值的比较结果。 用于比较由编译环境和`Option Compare`语句。 二进制比较确定每个字符串中的每个字符的数值的 Unicode 值是否相同。 文本比较执行 Unicode 文本比较基于当前区域性中使用的.NET Framework。 在进行字符串比较时，null 值相当于字符串文字`""`。

__操作类型：__
        
|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __bo__ | bo | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | bo | Ob | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __通过__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __US__ |    |    |    |    | US | 内 | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | de | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __De__ |    |    |    |    |    |    |    |    |    | de | Si | 应该 | Err | Err | 应该 | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该 | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该 | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | da  | Err | da | Ob | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | ch  | St | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | St | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


## <a name="like-operator"></a>Like 运算符

`Like`运算符确定一个字符串是否与给定的模式匹配。

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

`Like`运算符定义为`String`类型。 第一个操作数是要匹配的字符串和第二个操作数是要匹配的模式。 该模式由 Unicode 字符组成。 以下字符序列具有特殊含义：

* 字符`?`任何单个字符匹配。

* 字符`*`匹配零个或多个字符。

* 字符`#`匹配任何单个数字 (0-9)。

* 用方括号括起来的字符的列表 (`[ab...]`) 列表中的任何单个字符匹配。

* 字符的列表，由括号圈定且前缀为感叹号 (`[!ab...]`) 字符列表中没有任何单个字符匹配。

* 连字符字符列表中的两个字符分隔值 (`-`) 指定范围的 Unicode 字符的第一个字符开头和结尾的第二个字符。 如果第二个字符不比第一个字符在排序顺序中的更高版本，会发生运行时异常。 连字符开头或字符列表的末尾显示指定本身。

若要匹配的特殊字符的左的括号 (`[`)，问号 (`?`)，数字符号 (`#`)，和星号 (`*`)，方括号必须将它们。 右方括号 (`]`) 不能用于组中与自身匹配，但它可以在组外使用，作为单个字符。 字符序列`[]`被视为字符串文本`""`。 

请注意，字符比较和排序字符列表取决于所使用的比较类型。 如果要使用二进制比较，字符比较和排序基于数值的 Unicode 值。 如果使用文本比较时，字符比较和排序根据所使用的.NET Framework 的当前区域设置。

在某些语言中，特殊字符的字母表示的两个不同字符中，反之亦然。 例如，有几种语言使用字符`æ`来表示字符`a`和`e`出现时，它们在一起，而字符`^`和`O`可用于表示字符`Ô`. 使用文本比较时`Like`运算符可以识别此类文化的等效项。 在这种情况下，模式或字符串中的一个特殊字符的匹配项匹配其他字符串中等效的两个字符序列。 同样，单个的特殊字符模式括在括号中 （本身，在列表中，或某个范围内） 匹配等效的两个字符序列的字符串中，反之亦然。

在中`Like`表达式，其中两个操作数都`Nothing`或者一个操作数具有内部函数转换为`String`和另一个操作数是`Nothing`，`Nothing`视为如同它是空的字符串文字是`""`。

__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __bo__ | St | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __SB__ |    | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __通过__ |    |    | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __Sh__ |    |    |    | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __US__ |    |    |    |    | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __In__ |    |    |    |    |    | St | St | St | St | St | St | St | St | St | St | Ob | 
| __UI__ |    |    |    |    |    |    | St | St | St | St | St | St | St | St | St | Ob | 
| __Lo__ |    |    |    |    |    |    |    | St | St | St | St | St | St | St | St | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | St | St | St | St | St | St | St | Ob | 
| __De__ |    |    |    |    |    |    |    |    |    | St | St | St | St | St | St | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | St | St | St | St | St | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | St | St | St | St | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | St | St | St | Ob | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |    | St | St | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | St | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="concatenation-operator"></a>串联运算符

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

*串联运算符*定义所有内部类型，包括内部函数的值类型的可以为 null 的版本。 它还定义的前面提及的类型之间的串联和`System.DBNull`，也被视为`Nothing`字符串。 串联运算符将转换为其操作数的所有`String`; 在表达式中，所有转换到`String`被视为扩大转换，而不管是否使用严格的语义。 一个`System.DBNull`值转换为文本`Nothing`化为`String`。 值为 null 的值类型`Nothing`也转换为文本`Nothing`化为`String`，而不是引发运行时错误。

串联运算会导致是从左到右的顺序对两个操作数的串联的字符串。 该值`Nothing`视为如同它是空的字符串文字是`""`。

__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __bo__ | St | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __SB__ |    | St | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __通过__ |    |    | St | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __Sh__ |    |    |    | St | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __US__ |    |    |    |    | St | St | St | St | St | St | St | St | St | St | St | Ob | 
| __In__ |    |    |    |    |    | St | St | St | St | St | St | St | St | St | St | Ob | 
| __UI__ |    |    |    |    |    |    | St | St | St | St | St | St | St | St | St | Ob | 
| __Lo__ |    |    |    |    |    |    |    | St | St | St | St | St | St | St | St | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | St | St | St | St | St | St | St | Ob | 
| __De__ |    |    |    |    |    |    |    |    |    | St | St | St | St | St | St | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | St | St | St | St | St | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | St | St | St | St | Ob | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | St | St | St | Ob | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |    | St | St | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | St | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="logical-operators"></a>逻辑运算符

`And`， `Not`， `Or`，和`Xor`操作员称为逻辑运算符。

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

逻辑运算符计算，如下所示：

* 有关`Boolean`类型：

  * 一个逻辑`And`对其两个操作数执行操作。

  * 一个逻辑`Not`对其操作数执行操作。

  * 一个逻辑`Or`对其两个操作数执行操作。

  * 逻辑异-`Or`对其两个操作数执行操作。

* 有关`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`，和所有枚举类型，将指定的操作执行的二进制表示形式的每一位两个操作数：

  * `And`：结果位是 1，如果两个位都为 1;否则，结果位为 0。

  * `Not`：结果位是 1，如果位为 0;否则，结果位为 1。

  * `Or`：结果位是 1，如果任一位为 1;否则，结果位为 0。

  * `Xor`：结果位是 1，如果任一位为 1，但不是能同时位;否则，结果位为 0 (即 1 `Xor` 0 = 1，1 `Xor` 1 = 0)。

* 当逻辑运算符`And`并`Or`类型提升`Boolean?`，它们已得到扩展以包含三个值布尔逻辑这种情况下：

  * `And` 如果这两个操作数都为 true; 计算结果为 true其中一个操作数为 false; 否则，false`Nothing`否则为。

  * `Or` 计算结果为 true，如果其中一个操作数为 true;false 是两个操作数都为 false;`Nothing`否则为。

例如：

```vb
Module Test
    Sub Main()
        Dim x?, y? As Boolean

        x = Nothing
        y = True 

        If x Or y Then
            ' Will execute
        End If
    End Sub
End Module
```

__请注意。__ 理想情况下，逻辑运算符`And`和`Or`将使用三值逻辑可以使用布尔表达式中任何类型提升 (即实现的类型`IsTrue`和`IsFalse`)，在相同的方式`AndAlso`和`OrElse`短路在可以使用布尔表达式中所有类型。 遗憾的是，三值提升只应用于`Boolean?`，因此需三值逻辑的用户定义类型必须手动执行此操作通过定义`And`和`Or`它们可以为 null 的版本的运算符。

从这些操作可能不会出现任何溢出。 枚举的类型运算符执行按位运算的基础类型的枚举类型，但返回的值是枚举的类型。

__不是操作类型：__

| __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| bo | SB | 间距 | Sh | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 

__并且，或 Xor 操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | bo | SB | Sh | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | bo  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __通过__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __US__ |    |    |    |    | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __De__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="short-circuiting-logical-operators"></a>短路逻辑运算符

`AndAlso`并`OrElse`运算符会短路新版`And`和`Or`逻辑运算符。

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

由于其短短路行为，第二个操作数是未在运行时计算如果评估的第一个操作数后已知的运算符的结果。

短路逻辑运算符计算，如下所示：

* 如果中的第一个操作数`AndAlso`运算计算结果为`False`，则返回 True 中的或其`IsFalse`运算符，该表达式将返回第一个操作数。 否则，第二个操作数是计算和逻辑`And`两个结果上执行操作。

* 如果中的第一个操作数`OrElse`运算计算结果为`True`，则返回 True 中的或其`IsTrue`运算符，该表达式将返回第一个操作数。 否则，第二个操作数是计算和逻辑`Or`其两个结果上执行操作。

`AndAlso`并`OrElse`类型定义运算符`Boolean`，或任何类型的`T`重载以下运算符：

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

以及重载相应`And`或`Or`运算符：

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

评估时`AndAlso`或`OrElse`运算符，只有一次计算的第一个操作数和第二个操作数是不会计算或仅计算一次。 例如，考虑以下代码：

```vb
Module Test
    Function TrueValue() As Boolean
        Console.Write(" True")
        Return True
    End Function

    Function FalseValue() As Boolean
        Console.Write(" False")
        Return False
    End Function

    Sub Main()
        Console.Write("And:")
        If FalseValue() And TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("Or:")
        If TrueValue() Or FalseValue() Then
        End If
        Console.WriteLine()

        Console.Write("AndAlso:")
        If FalseValue() AndAlso TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("OrElse:")
        If TrueValue() OrElse FalseValue() Then
        End If
        Console.WriteLine()
    End Sub
End Module
```

它将打印以下结果：

```
And: False True
Or: True False
AndAlso: False
OrElse: True
```

形式的提升`AndAlso`并`OrElse`运算符，如果第一个操作数为 null `Boolean?`，然后第二个操作数的求值，但结果始终为 null `Boolean?`。

__操作类型：__

|        | __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __bo__ | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __SB__ |    | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __通过__ |    |    | bo | bo | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __Sh__ |    |    |    | bo | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __US__ |    |    |    |    | bo | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __In__ |    |    |    |    |    | bo | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __UI__ |    |    |    |    |    |    | bo | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | bo | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | bo | bo | bo | bo | Err | Err | bo  | Ob  | 
| __De__ |    |    |    |    |    |    |    |    |    | bo | bo | bo | Err | Err | bo  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | bo | bo | Err | Err | bo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | bo | Err | Err | bo  | Ob  | 
| __Da__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __Ch__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | bo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="shift-operators"></a>移位运算符

二元运算符`<<`和`>>`执行移位运算。

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

有关定义了运算符`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`，`ULong`和`Long`类型。 不同于其他二进制运算符，就像该运算符是只是左操作数的一元运算符确定移位运算的结果类型。 右操作数的类型必须可隐式转换为`Integer`并不用于确定该操作的结果类型。

`<<`运算符使位要移动的第一个操作数中剩余的位移量由指定的位数。 结果类型超出范围的高顺序位将被放弃且低位空出的位用零填充。

`>>`运算符的原因要移动的第一个操作数中的位右键的位移量由指定的位数。 低顺序位将被放弃且高序位空出的位设置为零，如果左的操作数为正或到某个如果为负数。 如果左的操作数的类型`Byte`， `UShort`， `UInteger`，或`ULong`空缺的高顺序位用零填充。

移位运算符移位底层的表示形式的第一个操作数的第二个操作数的数量。 如果第二个操作数的值大于第一个操作数中的位数或为负，则移位量计算为`RightOperand And SizeMask`其中`SizeMask`是：

| __LeftOperand 类型__  | __SizeMask__ | 
|-----------------------|--------------|
| `Byte`， `SByte`       | 7 (`&H7`)    | 
| `UShort`， `Short`     | 15 (`&HF`)   | 
| `UInteger`， `Integer` | 31 (`&H1F`)  | 
| `ULong`， `Long`       | 63 (`&H3F`)  | 

移位量为零，如果操作的结果是第一个操作数的值相同。 从这些操作可能不会出现任何溢出。

__操作类型：__


| __bo__ | __SB__ | __通过__ | __Sh__ | __US__ | __In__ | __UI__ | __Lo__ | __UL__ | __De__ | __Si__ | __Do__ | __Da__  | __Ch__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | 间距 | Sh | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 


## <a name="boolean-expressions"></a>布尔表达式

一个布尔表达式是可以进行测试以查看是否为 true，或如果为 false 的表达式。

```antlr
BooleanExpression
    : Expression
    ;
```

一种类型`T`布尔表达式中可以使用 if、 按优先顺序：

* `T` 是`Boolean`或 `Boolean?`

* `T` 扩大转换为 `Boolean`

* `T` 扩大转换为 `Boolean?`

* `T` 定义两个伪运算符，`IsTrue`和`IsFalse`。

* `T` 收缩转换为`Boolean?`，并不涉及从转换`Boolean`到`Boolean?`。

* `T` 收缩转换为`Boolean`。

__请注意。__ 请注意，如果有趣的是`Option Strict`处于关闭状态，具有到收缩转换的表达式`Boolean`没有编译时错误，但语言仍喜欢将接受`IsTrue`如果它存在的运算符。 这是因为`Option Strict`只会更改什么是语言，将不被接受，并且永远不会更改表达式的实际含义。 因此，`IsTrue`必须始终首选通过收缩转换，而不考虑`Option Strict`。

例如，下面的类不定义扩大转换为`Boolean`。 因此，在中使用它`If`语句将导致调用`IsTrue`运算符。

```vb
Class MyBool
    Public Shared Widening Operator CType(b As Boolean) As MyBool
        ...
    End Operator

    Public Shared Narrowing Operator CType(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsTrue(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsFalse(b As MyBool) As Boolean
        ...
    End Operator
End Class

Module Test
    Sub Main()
        Dim b As New MyBool

        If b Then Console.WriteLine("True")
    End Sub
End Module
```

如果布尔表达式是类型为或转换成`Boolean`或`Boolean?`，则它是 true，如果值为`True`和 false 否则为。

否则，布尔表达式会调用`IsTrue`运算符，并返回`True`如果运算符返回`True`; 否则为 false (但永远不会调用`IsFalse`运算符)。

在以下示例中，`Integer`收缩转换为`Boolean`，因此 null`Integer?`具有收缩转换`Boolean?`(生成 null `Boolean`) 和`Boolean`（这将引发异常）。 收缩转换为`Boolean?`是首选方法，因此的值"`i`"为的布尔值表达式是因此`False`。

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a>Lambda 表达式

一个*lambda 表达式*定义匿名方法称为*lambda 方法*。 Lambda 方法轻松地将"行中"方法传递到采用委托类型的其他方法。

```antlr
LambdaExpression
    : SingleLineLambda
    | MultiLineLambda
    ;

SingleLineLambda
    : LambdaModifier* 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? Expression
    | 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? Statement
    ;

MultiLineLambda
    : MultiLineFunctionLambda
    | MultiLineSubLambda
    ;

MultiLineFunctionLambda
    : LambdaModifier? 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? ( 'As' TypeName )? LineTerminator
      Block
      'End' 'Function'
    ;

MultiLineSubLambda
    : LambdaModifier? 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block
      'End' 'Sub'
    ;

LambdaModifier
    : 'Async' | 'Iterator'
    ;
```

下面的示例：

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, Function(x As Integer) x * 2)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```

将打印出：

```
2 4 6 8
```

Lambda 表达式开头的可选修饰符`Async`或`Iterator`后, 跟关键字`Function`或`Sub`和参数列表。 不能声明为 lambda 表达式中的参数`Optional`或`ParamArray`并且不能具有属性。 与正则方法不同省略 lambda 方法的参数类型不会自动推断`Object`。 相反，lambda 方法重新分类，省略的参数类型和`ByRef`修饰符推断出的目标类型。 在上一示例中，lambda 表达式编写时本来可以作为`Function(x) x * 2`，它将推断的类型和`x`要`Integer`lambda 方法已用于创建实例`IntFunc`委托类型。 与不同的本地变量推断，如果 lambda 方法参数省略了一种类型，但具有数组或名称可为 null 的修饰符，会发生编译时错误。

一个__正则 lambda 表达式__是指既不具有`Async`也不`Iterator`修饰符。

__迭代器 lambda 表达式__是一个具有`Iterator`修饰符，但不`Async`修饰符。 它必须是一个函数。 当它对重新分类为一个值时，它可以仅重新分类为其返回类型是委托类型的值`IEnumerator`，或`IEnumerable`，或`IEnumerator(Of T)`或`IEnumerable(Of T)`某些`T`，并且不具有任何 ByRef 参数。

__异步 lambda 表达式__是一个具有`Async`修饰符，但不`Iterator`修饰符。 Async sub lambda 只是为不带任何 ByRef 参数的 sub 委托类型的值重新分类。 仅可以为其返回类型是函数委托类型的值重新分类异步函数 lambda`Task`或`Task(Of T)`某些`T`，并且不具有任何 ByRef 参数。

Lambda 表达式可以是单行或多行。 单行`Function`lambda 表达式包含一个表示从 lambda 方法返回的值的表达式。 单行`Sub`lambda 表达式包含单个语句无需其关闭`StatementTerminator`。 例如：

```vb
Module Test
    Sub Do(a() As Integer, action As Action(Of Integer))
        For index As Integer = 0 To a.Length - 1
            action(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Do(a, Sub(x As Integer) Console.WriteLine(x))
    End Sub
End Module
```

单行 lambda 紧密比所有其他表达式和语句构造绑定。 因此，对于示例中，"`Function() x + 5`"是等效于"`Function() (x+5)"`而非"`(Function() x) + 5`"。 为避免混淆的单行`Sub`Dim 语句或标签声明语句不能包含 lambda 表达式。 此外，除非将它括在括号内的单行`Sub`lambda 表达式可能会不立即后跟一个冒号":"，成员访问运算符"。"，字典成员访问运算符"！"或左括号"(". 它不能包含任何块语句 (`With`， `SyncLock, If...EndIf`， `While`， `For`， `Do`， `Using`)，也不`OnError`也不`Resume`。

__请注意。__ 在 lambda 表达式`Function(i) x=i`，正文将被解释为*表达式*(测试是否`x`和`i`相等)。 Lambda 表达式中，但是`Sub(i) x=i`，正文将被解释为一个语句 (哪个分配`i`到`x`)。

多行 lambda 表达式包含语句块，相应结尾`End`语句 (即`End Function`或`End Sub`)。 与正则方法，多行 lambda 方法的一样`Function`或`Sub`语句和`End`语句必须置于其自己的行。 例如：

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

多行`Function`lambda 表达式可以声明返回类型，但不能将其上的属性。 如果多行`Function`lambda 表达式不声明返回类型，但可以从在其中使用 lambda 表达式，则使用该返回类型的上下文推断返回类型。 否则该函数的返回类型的计算方式如下：

* 在正则 lambda 表达式中，返回类型是中的表达式中所有的主要类型`Return`语句块中的语句。

* 在异步 lambda 表达式中，返回类型是`Task(Of T)`其中`T`是基准类型中所有的表达式`Return`语句块中的语句。

* 在迭代器 lambda 表达式中，返回类型是`IEnumerable(Of T)`其中`T`是基准类型中所有的表达式`Yield`语句块中的语句。

例如：

```vb
Function f(min As Integer, max As Integer) As IEnumerable(Of Integer)
    If min > max Then Throw New ArgumentException()
    Dim x = Iterator Function()
                  For i = min To max
                    Yield i
                Next
               End Function

    ' infers x to be a delegate with return type IEnumerable(Of Integer)
    Return x()
End Function
```

在所有情况下，如果有任何`Return`(分别`Yield`) 语句，或如果没有基准类型之间，并且正在使用严格的语义，会发生编译时错误; 否则基准类型是隐式`Object`。

请注意，从所有计算的返回类型`Return`语句，即使它们不是可访问。 例如：

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

隐式返回变量，因为没有将变量的名称。

在多行 lambda 表达式内的语句块具有以下限制：

* `On Error` 并`Resume`语句不允许，尽管`Try`允许使用的语句。

* 不能在多行 lambda 表达式中声明静态局部变量。

* 虽然一般分支规则可以应用在其中不可能进行分支传入或传出的多行 lambda 表达式、 语句块。 例如：

  ```vb
  Label1:
  Dim x = Sub()
                 ' Error: Cannot branch out
                 GoTo Label1

                 ' OK: Wholly within the lamba.
                 GoTo Label2:
            Label2:
            End Sub

  ' Error: Cannot branch in
  GoTo Label2
  ```

Lambda 表达式是大致相当于匿名方法声明上包含的类型。 最初的示例中是大致相当于：

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Function $Lambda1(x As Integer) As Integer
        Return x * 2
    End Function

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, AddressOf $Lambda1)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```


### <a name="closures"></a>闭包

Lambda 表达式作用域，包括本地变量或参数中包含的方法和 lambda 表达式定义中有权访问的所有变量。 当 lambda 表达式引用的局部变量或参数时，lambda 表达式捕获到闭包所引用的变量。 闭包是位于堆栈上，而不是堆上的一个对象，当捕获变量时，将对该变量的所有引用重都定向到闭包。 这使 lambda 表达式，以继续包含方法完成后即使引用局部变量和参数。 例如：

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D
        Dim x As Integer = 10
        Return Function() x
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

是大致相当于：

```vb
Module Test
    Delegate Function D() As Integer

    Class $Closure1
        Public x As Integer

        Function $Lambda1() As Integer
            Return x
        End Function
    End Class

    Function M() As D
        Dim c As New $Closure1()
        c.x = 10
        Return AddressOf c.$Lambda1
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

闭包捕获新副本的本地变量进入块中的每次本地声明变量，但如果有的话，使用以前复制的值初始化的新副本。 例如：

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D()
        Dim a(9) As D

        For i As Integer = 0 To 9
            Dim x
            a(i) = Function() x
            x += 1
        Next i

        Return a
    End Function

    Sub Main()
        Dim y() As D = M()

        For i As Integer = 0 To 9
            Console.Write(y(i)() & " ")
        Next i
    End Sub
End Module
```

将打印

```
1 2 3 4 5 6 7 8 9 10
```

而不是

```
9 9 9 9 9 9 9 9 9 9
```

闭包需要输入块时初始化，因为不允许对`GoTo`到具有从该块中，外部的闭包的块虽然允许到`Resume`到闭包中包含的块。 例如：

```vb
Module Test
    Sub Main()
        Dim a = 10

        If a = 10 Then
L1:
            Dim x = Function() a

            ' Valid, source is within block
            GoTo L2
L2:
        End If

        ' ERROR: target is inside block with closure
        GoTo L1
    End Sub
End Module
```

因为它们不能捕获到闭包，以下不能出现在 lambda 表达式：

* 引用参数。

* 实例表达式 (`Me`， `MyClass`， `MyBase`)，如果类型的`Me`不是类。

匿名类型创建表达式，如果 lambda 表达式的表达式的一部分的成员。 例如：

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

`ReadOnly` 实例构造函数中的实例变量或`ReadOnly`共享共享其中的非值上下文中使用变量的构造函数中的变量。 例如：

```vb
Class C1
    ReadOnly F1 As Integer

    Sub New()
        ' Valid, doesn't modify F1
        Dim x = Function() F1

        ' Error, tries to modify F1
        Dim f = Function() ModifyValue(F1)
    End Sub

    Sub ModifyValue(ByRef x As Integer)
    End Sub
End Class
```

## <a name="query-expressions"></a>查询表达式

一个*查询表达式*是一个表达式，适用的一系列*查询运算符*于的元素*可查询*集合。 例如，以下表达式使用的集合`Customer`对象，并返回华盛顿州中的所有客户的名称：

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

查询表达式必须开头`From`或`Aggregate`运算符可以开头和结尾的任何查询运算符。 查询表达式的结果分类为一个值;该表达式的结果类型取决于表达式中的最后一个查询运算符的结果类型。

```antlr
QueryExpression
    : FromOrAggregateQueryOperator QueryOperator*
    ;

FromOrAggregateQueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    ;

QueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    | SelectQueryOperator
    | DistinctQueryOperator
    | WhereQueryOperator
    | OrderByQueryOperator
    | PartitionQueryOperator
    | LetQueryOperator
    | GroupByQueryOperator
    | JoinOrGroupJoinQueryOperator
    ;

JoinOrGroupJoinQueryOperator
    : JoinQueryOperator
    | GroupJoinQueryOperator
    ;
```

### <a name="range-variables"></a>范围变量

一些查询运算符引入了一种特殊的变量称为*范围变量*。 范围变量不是实际的变量;相反，它们代表各个值在查询的计算期间针对输入集合。

```antlr
CollectionRangeVariableDeclarationList
    : CollectionRangeVariableDeclaration ( Comma CollectionRangeVariableDeclaration )*
    ;

CollectionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? 'In' LineTerminator? Expression
    ;

ExpressionRangeVariableDeclarationList
    : ExpressionRangeVariableDeclaration ( Comma ExpressionRangeVariableDeclaration )*
    ;

ExpressionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? Equals Expression
    ;
```

范围变量的作用域中引入的查询运算符到查询表达式的末尾或查询运算符如`Select`，以隐藏它们。 例如，在下面的查询

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

`From`查询运算符引入的范围变量`cust`化为`Customer`，表示在每个客户`Customers`集合。 以下`Where`查询运算符则引用范围变量`cust`以确定是否筛选出结果的集合与单个客户的筛选器表达式中。

有两种类型的范围变量：*集合的范围变量*并*表达式范围变量*。 集合的范围变量需要它们正在查询的集合的元素的值。 在集合范围变量声明中的集合表达式必须分类为值的类型可查询。 如果省略集合范围变量的类型，则它被推断为集合表达式的元素类型或`Object`如果集合表达式不具有元素类型 (也就是说，只能定义`Cast`方法)。 如果集合表达式不可查询 （即集合的元素类型无法推断），编译时错误结果。

表达式范围变量是其值计算表达式而不是集合的范围变量。 在以下示例中，`Select`运算符引入了一个名为的表达式范围变量的查询`cityState`计算从两个字段：

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

表达式范围变量不需要引用另一个范围变量，尽管此类变量可能是可疑的值。 分配给表达式的范围变量的表达式必须分类为一个值，并且必须隐式转换为的类型的范围变量，如果给定。

仅在 Let 运算符中表达式的范围变量可能指定其类型。 在其他运算符中，或如果未指定其类型，则使用本地变量的类型推理来确定范围变量的类型。

范围变量必须遵循声明在到隐藏的方面的本地变量的规则。 因此，范围变量不能隐藏本地变量或参数的封闭方法或另一个范围变量中的名称 （除非在查询运算符专门隐藏所有作用域中的当前范围变量）。


### <a name="queryable-types"></a>可查询类型

查询表达式是通过将转换为对集合类型的已知方法的调用的表达式实现的。 这些定义完善的方法定义的可查询集合的元素类型以及集合上执行的查询运算符的结果类型。 每个查询运算符指定方法的查询运算符通常会转换到，尽管具体的转换是依赖于实现。 使用如下所示的常规格式规范中给出了这些方法：

```vb
Function Select(selector As Func(Of T, R)) As CR
```

对方法执行以下操作：

* 方法必须是实例或集合类型的扩展成员，并且必须可访问性。

* 该方法可以是泛型类型，提供可能推断出所有类型参数。

* 可以重载方法，在这种情况下重载决策用于确定完全要使用的方法。

* 另一种委托类型可能会代替委托`Func`类型，前提是它具有相同的签名，包括返回类型，为匹配`Func`类型。

* 类型`System.Linq.Expressions.Expression(Of D)`可能会用它来替换该委托`Func`提供的类型`D`是委托类型具有相同的签名，包括返回类型，为匹配`Func`类型。

* 类型`T`表示输入集合的元素类型。 所有定义按集合类型的方法必须具有相同的输入的元素类型是可查询的集合类型。

* 类型`S`表示在执行联接的查询运算符的情况下的第二个输入集合的元素类型。

* 类型`K`表示在具有一组充当密钥的范围变量的查询运算符的情况下密钥类型。

* 类型`N`表示 （尽管仍可能是用户定义类型，不是内部函数的数值类型） 用作数值类型的类型。

* 类型`B`表示可以使用布尔表达式中的类型。

* 类型`R`表示结果集合的元素类型，如果查询运算符生成的结果集合。 `R` 取决于在结束时的查询运算符的作用域中的范围变量的数量。 如果一个范围变量是在范围内，然后`R`是该范围变量的类型。 在示例

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  查询的结果将是具有的元素类型的集合类型`String`。 如果多个范围变量，则在范围内，然后`R`是包含所有与作用域中的范围变量的匿名类型`Key`字段。 在下面的示例中：

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  查询的结果将是具有一个名为只读属性的匿名类型的元素类型的集合类型`Name`类型的`String`和名为的只读属性`ProductName`类型的`String`。

  在查询表达式中，生成的包含范围变量的匿名类型是*透明*，这意味着，范围变量都无需限定即可。 例如，在前面的示例的范围变量`c`并`o`无法访问而无需限定在`Select`查询运算符，即使输入的集合的元素类型是匿名类型。

* 类型`CX`表示集合类型，不一定是输入的集合类型，其元素类型是某些类型`X`。

可查询集合类型必须满足以下条件之一，按优先顺序：

* 它必须定义符合`Select`方法。

* 它必须已具有以下方法之一

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  可以调用以获取可查询的集合。 如果提供了这两种方法，`AsQueryable`最好通过`AsEnumerable`。

* 它必须具有一种方法

  ```vb
  Function Cast(Of T)() As CT
  ```

  可以调用与要生成可查询的集合的范围变量的类型。

确定集合的元素类型会独立于实际方法调用，因为无法确定特定的方法的适用性。 因此，在确定集合的元素类型时如果有匹配的已知方法的实例方法，则会忽略任何匹配的已知方法的扩展方法。

查询运算符转换其中的查询运算符在表达式中出现的顺序发生。 不需要的集合对象，若要实现所有所需的所有查询运算符方法，尽管每个集合对象至少必须支持`Select`查询运算符。 如果所需的方法不存在，则发生编译时错误。 绑定时的已知方法名称，非方法将忽略在接口和绑定，扩展方法中的多个继承，尽管隐藏的语义仍适用。 例如：

```vb
Class Q1
    Public Function [Select](selector As Func(Of Integer, Integer)) As Q1
    End Function
End Class

Class Q2
    Inherits Q1

    Public [Select] As Integer
End Class

Module Test
    Sub Main()
        Dim qs As New Q2()

        ' Error: Q2.Select still hides Q1.Select
        Dim zs = From q In qs Select q
    End Sub
End Module
```

### <a name="default-query-indexer"></a>默认查询索引器

每个可查询集合类型，其元素类型为`T`尚不包含默认值和属性都被认为具有如下常规形式的默认属性：

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

默认属性只被指使用默认属性访问语法;默认属性不能按名称引用。 例如：

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

如果集合类型不具有`ElementAtOrDefault`成员，将发生编译时错误。

### <a name="from-query-operator"></a>从查询运算符

`From`运算符引入了一个集合范围变量，表示集合的单个成员要查询的查询。

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

例如，查询表达式：

```vb
From c As Customer In Customers ...
```

可被视为等效于

```vb
For Each c As Customer In Customers
        ...
Next c
```

当`From`查询运算符声明范围变量的多个集合，或者不是第一个`From`在查询表达式中的查询运算符，每个新的集合范围变量是*交叉联接*到一组现有的范围变量。 结果是通过联接集合中的所有元素的叉积计算查询。 例如，表达式：

```vb
From c In Customers _
From e In Employees _
...
```

可被视为等效于：

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

完全等效于：

```vb
From c In Customers, e In Employees ...
```

可以在更高版本中使用在先前的查询运算符中引入的范围变量`From`查询运算符。 例如，在下面的查询表达式中第二个`From`查询运算符是指第一个范围变量的值：

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

中的多个范围变量`From`查询运算符或多个`From`仅支持查询运算符，如果集合类型包含一个或两个以下方法：

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

代码

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__请注意。__ `From` 不是保留的字。


### <a name="join-query-operator"></a>Join 查询运算符

`Join`运算符联接现有范围变量使用新集合的范围变量，并生成其元素被连接在一起的单个集合的查询基于相等表达式。

```antlr
JoinQueryOperator
    : LineTerminator? 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
    ;

JoinConditionList
    : JoinCondition ( 'And' LineTerminator? JoinCondition )*
    ;

JoinCondition
    : Expression 'Equals' LineTerminator? Expression
    ;
```

例如：

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

相等表达式是比常规相等表达式更受限制：

* 这两个表达式必须分类为一个值。

* 这两个表达式必须引用至少一个范围变量。

* 必须由一个表达式，引用查询运算符和表达式不能引用任何其他范围变量，在联接中声明范围变量。

如果两个表达式的类型不完全相同的类型，然后

* 如果为两种类型定义相等运算符，这两个表达式都可以隐式转换为它，并且它不是`Object`，然后将这两个表达式转换为该类型。

* 否则，如果两个表达式可以隐式转换为基准类型，然后将转换两个表达式为该类型。

* 否则，将发生编译时错误。

使用哈希值进行比较的表达式 (即通过调用`GetHashCode()`) 而不是使用相等运算符以提高效率。 一个`Join`查询运算符可能会执行多个联接或中相同的运算符的相等性条件。 一个`Join`如果集合类型包含的方法，则仅支持查询运算符：

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

代码

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.Join( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__请注意。__ `Join``On`和`Equals`不是保留的字。


### <a name="let-query-operator"></a>让查询运算符

`Let`查询运算符引入的表达式范围变量。 这样，将在更高版本的查询运算符中使用多个时间后计算中间值。

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

例如：

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

可被视为等效于：

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

一个`Let`如果集合类型包含的方法，则仅支持查询运算符：

```vb
Function Select(selector As Func(Of T, R)) As CR
```

代码

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a>选择查询运算符

`Select`查询运算符就像`Let`查询运算符，因为它引入了表达式的范围变量; 但是，`Select`查询运算符会隐藏当前可用的范围变量而不是将添加到它们。 此外，通过引入了表达式范围变量的类型`Select`始终使用本地变量的类型推断规则推断查询运算符; 不能指定显式类型，并且如果没有类型可以推断出，会发生编译时错误。

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

例如，在查询中：

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

`Where`查询运算符仅有权访问`name`引入的范围变量`Select`运算符; 如果`Where`运算符尝试引用`cust`，会出现编译时错误。

而不是显式指定范围变量的名称`Select`查询运算符可以推断范围变量的名称作为匿名类型对象创建表达式中使用相同的规则。 例如：

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

如果未提供范围变量的名称，不能推断名称，将发生编译时错误。 如果`Select`查询运算符包含单个表达式，如果无法推断该范围变量的名称，但没有名称，范围变量将会发生任何错误。  例如：

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

如果在多义性`Select`之间将名称分配给范围变量和表达式是否相等的查询运算符，该名称分配是首选。 例如：

```vb
Dim badCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select name = c.Name ' Creates a range variable named "name"


Dim goodCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select match = (name = c.Name)
```

在每个表达式`Select`查询运算符必须分类为一个值。 一个`Select`集合类型包含一种方法时才支持查询运算符：

```vb
Function Select(selector As Func(Of T, R)) As CR
```

代码

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a>Distinct 查询运算符

`Distinct`查询运算符限制仅对具有非重复值，确定比较是否相等的元素类型的集合中的值。

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

例如，以下查询：

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

只会返回一个配对的客户名称和订单价格，每个非重复行，即使客户所具有的多个订单的价格相同。 一个`Distinct`集合类型包含一种方法时才支持查询运算符：

```vb
Function Distinct() As CT
```

代码

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

__请注意。__ `Distinct` 不是保留的字。


### <a name="where-query-operator"></a>Where 查询运算符

`Where`查询运算符限制为满足给定的条件的集合中的值。

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

一个`Where`查询运算符采用布尔表达式，计算每个集的范围变量的值; 如果表达式的值为 true，则输出集合中的值显示，否则这些值将跳过。 例如，查询表达式：

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

可以认为的视为等效于嵌套循环

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

一个`Where`集合类型包含一种方法时才支持查询运算符：

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

代码

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

__请注意。__ `Where` 不是保留的字。


### <a name="partition-query-operators"></a>分区查询运算符

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

`Take`查询运算符中第一个结果`n`集合中的元素。 与一起使用时`While`修饰符，`Take`运算符在第一个结果`n`满足布尔表达式的集合的元素。 `Skip`运算符将跳过第一个`n`集合中的元素，然后返回集合的其余部分。  当结合使用时`While`修饰符，`Skip`运算符将跳过第一个`n`满足的布尔表达式，然后返回集合的其余部分的元素的集合。 中的表达式`Take`或`Skip`查询运算符必须分类为一个值。

一个`Take`集合类型包含一种方法时才支持查询运算符：

```vb
Function Take(count As N) As CT
```

一个`Skip`集合类型包含一种方法时才支持查询运算符：

```vb
Function Skip(count As N) As CT
```

一个`Take While`集合类型包含一种方法时才支持查询运算符：

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

一个`Skip While`集合类型包含一种方法时才支持查询运算符：

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

代码

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

__请注意。__ `Take` 和`Skip`不是保留的字。


### <a name="order-by-query-operator"></a>Order By 查询运算符

`Order By`查询运算符对出现在范围变量的值。 

```antlr
OrderByQueryOperator
    : LineTerminator? 'Order' 'By' LineTerminator? OrderExpressionList
    ;

OrderExpressionList
    : OrderExpression ( Comma OrderExpression )*
    ;

OrderExpression
    : Expression Ordering?
    ;

Ordering
    : 'Ascending' | 'Descending'
    ;
```

`Order By`查询运算符具有指定应该用于排序的迭代变量的键值的表达式。 例如，以下查询返回产品价格按排序：

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

排序可以标记为`Ascending`，在这种情况下较小的值出现在之前更大的值，或`Descending`，在这种情况下更大的值出现在之前较小的值。 如果未指定任何排序的默认值是`Ascending`。 例如，以下查询返回产品首先按成本最高的产品的价格：

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

`Order By`查询运算符可以指定多个表达式进行排序，在这种情况下，集合进行排序以嵌套方式。 例如，下面的查询进行排序的状态，然后按市/县内每个状态，然后通过在每个城市的邮政编码的客户：

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

中的表达式`Order By`查询运算符必须分类为一个值。 `Order By`只有集合类型中包含一个或两个以下方法支持查询运算符：

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

返回类型`CT`必须是*有序集合*。 有序的集合是包含一个或两种方法的集合类型：

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

代码

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

__请注意。__ 因为查询运算符只需映射到实现特定的查询操作的方法的语法，顺序暂留不规定语言，并由运算符本身的实现。 要重载加法运算符的用户定义的数值类型的实现不执行任何操作类似于一个补充，这是非常类似于用户定义的运算符。 当然，若要保留可预测性，实现不符合用户预期的内容不建议。

__请注意。__ `Order` 和`By`不是保留的字。


### <a name="group-by-query-operator"></a>通过查询运算符的组

`Group By`查询运算符进行分组作用域基于一个或多个表达式中的范围变量，然后生成新范围变量基于这些分组。

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

例如，下面的查询进行分组的所有客户`State`，然后计算每个组的计数和平均期限：

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

`Group By`查询运算符具有三个子句： 可选`Group`子句`By`子句，和`Into`子句。 `Group`子句具有相同的语法和效果`Select`查询运算符，不同之处在于它只影响中可用的范围变量`Into`子句，而不`By`子句。 例如：

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

`By`子句声明表达式用作在分组操作的键值的范围变量。 `Into`子句允许通过格式正确的组的每个计算聚合的范围变量的表达式声明`By`子句。 内`Into`子句，表达式范围变量只能分配是方法调用的表达式的*聚合函数*。 聚合函数是在组 （其中不一定是相同的集合类型的原始集合） 其外观类似于以下方法之一的集合类型的函数：

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

如果聚合函数采用委托参数，则调用表达式可以具有一个参数表达式必须分类为一个值。  参数表达式可以使用作用域; 中的范围变量在为聚合函数的调用，这些范围变量表示格式正确，并非所有集合中的值的组中的值。 例如，在本部分中的原始示例`Average`函数计算每个状态而不是为所有客户的客户的年龄段的平均值在一起。

所有集合类型被都视为具有聚合函数`Group`定义它，它不采用任何参数，并只需返回的组。 集合类型可能会提供其他标准聚合函数是：

`Count` 和`LongCount`，这组或组中满足条件的布尔表达式的元素数中返回的元素的计数。 `Count` 和`LongCount`集合类型包含的方法之一时才受支持：

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

`Sum`它返回的表达式的和跨组中的所有元素。 `Sum` 集合类型包含的方法之一时才支持：

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

`Min` 这组中的所有元素返回表达式的最小值。 `Min` 集合类型包含的方法之一时才支持：

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

`Max`表示组中的所有元素返回表达式的最大值。 `Max` 集合类型包含的方法之一时才支持：

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

`Average`表示组中的所有元素返回表达式的平均值。 `Average` 集合类型包含的方法之一时才支持：

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

`Any`用于确定组是否包含成员，或如果布尔表达式为 true 的组中任何元素。 `Any` 返回一个值，可以使用布尔表达式中，集合类型包含的方法之一时才受支持：

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

`All`用于确定是否在组中的所有元素，则返回 true 的布尔表达式。 `All` 返回一个值，可以使用布尔表达式中，集合类型包含一种方法时才受支持：

```vb
Function All(predicate As Func(Of T, B)) As B
```

之后`Group By`查询运算符，作用域中以前的范围变量被隐藏，并且范围变量引入`By`和`Into`子句都可用。 一个`Group By`集合类型包含的方法时，才支持查询运算符：

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

范围变量声明中的`Group`子句支持仅当集合类型包含的方法：

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

代码

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.GroupBy( _
        Function(x As Integer) x Mod 2, _
        Function(x As Integer) New With {.y = x * 10, .z = x / 10}, _
        Function(evenOdd, group) New With { _
            evenOdd, _
            .Sum = group.Sum(Function(e) e.y), _
            .Average = group.Average(Function(e) e.z)})...
```

__请注意。__ `Group``By`，和`Into`不是保留的字。


### <a name="aggregate-query-operator"></a>聚合查询运算符

`Aggregate`查询运算符执行类似的功能作为`Group By`运算符，但它允许聚合已形成的组。 因为已正确组，`Into`子句`Aggregate`运算符不会隐藏范围内的范围变量的查询 (这种方式，`Aggregate`更像是`Let`，并`Group By`更像是`Select`)。

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

例如，下面的查询聚合华盛顿州的客户的订单的总计：

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

此查询的结果是其元素类型的集合是具有一个名为属性的匿名类型`cust`化为`Customer`和名为的属性`Sum`化为`Integer`。

与不同`Group By`，其他查询运算符可以置于之间`Aggregate`和`Into`子句。 之间`Aggregate`子句和末尾`Into`子句中，作用域，包括那些由声明中的所有范围变量`Aggregate`子句只能用。 例如，以下查询聚合放置的所有订单的总和，按华盛顿州在 2006 年之前的客户：

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

`Aggregate`运算符还可用于开始查询表达式。 在这种情况下，查询表达式的结果将是通过计算出的单个值`Into`子句。 例如，以下查询将计算在 2006 年 1 月 1 日之前的所有订单总计值之和：

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

查询的结果是单个`Integer`值。 `Aggregate`查询运算符都始终可用 （尽管聚合函数是也必须提供有效的表达式）。 代码

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

__请注意。__ `Aggregate` 和`Into`不是保留的字。


### <a name="group-join-query-operator"></a>组联接查询运算符

`Group Join`查询运算符的函数组合起来`Join`和`Group By`到单个运算符的查询运算符。 `Group Join` 联接两个集合根据匹配项从组合在一起的元素中提取所有匹配联接左侧的特定元素联接右侧元素。 因此，则运算符产生层次结构的结果的集。

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

例如，下面的查询生成包含单个客户的名称，一组所有其订单和所有这些订单的总量的元素：

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

查询的结果是其元素类型是具有三个属性的匿名类型集合：`Name`作为类型化`String`，`Orders`其元素类型的集合类型化为`Order`，和`OrdersTotal`作为类型化`Integer`. 一个`Group Join`集合类型包含的方法时，才支持查询运算符：

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

代码

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

通常会转换为

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.GroupJoin( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x, group) New With {x, .g = group})...
```

__请注意。__ `Group``Join`，和`Into`不是保留的字。


## <a name="conditional-expressions"></a>条件表达式

条件`If`表达式测试表达式并返回一个值。

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

与不同`IIF`运行时函数，但是，条件表达式仅计算其操作数如有必要。 因此，对于示例中，表达式`If(c Is Nothing, c.Name, "Unknown")`如果不会引发异常的值`c`是`Nothing`。 条件表达式有两种形式： 一种采用两个操作数和一个采用三个操作数。

如果提供了三个操作数，所有三个表达式必须分类为值，并在第一个操作数必须是布尔表达式。 如果结果为的表达式为 true，则第二个表达式将是结果的运算符，否则第三个表达式将运算符的结果。 该表达式的结果类型是第二个和第三个表达式的类型之间的基准类型。 如果没有基准类型，则会发生编译时错误。

如果提供了两个操作数，两个操作数必须分类为值，并在第一个操作数必须是引用类型或可以为 null 值类型。 表达式`If(x, y)`然后评估像是表达式`If(x IsNot Nothing, x, y)`，有两个例外。 首先，第一个表达式只计算一次，第二，如果第二个操作数的类型是不可以为 null 的值类型，并且第一个操作数的类型为，`?`从第一个操作数的类型中删除时确定的基准类型表达式的结果类型。 例如：

```vb
Module Test
    Sub Main()
        Dim x?, y As Integer
        Dim a?, b As Long

        a = If(x, a)        ' Result type: Long?
        y = If(x, 0)        ' Result type: Integer
    End Sub
End Module
```

在这两种形式的表达式，如果一个操作数为`Nothing`，其类型不用于确定基准类型。 如果表达式`If(<expression>, Nothing, Nothing)`，基准类型被视为可`Object`。


## <a name="xml-literal-expressions"></a>XML 文本表达式

XML 文本表达式表示的 XML （可扩展标记语言） 1.0 的值。

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

XML 文本表达式的结果是作为一种从类型类型化值`System.Xml.Linq`命名空间。 如果该命名空间中的类型不可用，XML 文本表达式将导致编译时错误。 通过从 XML 文本表达式转换构造函数调用所生成的值。 例如，代码：

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

是大致等效于代码：

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

XML 文本表达式可以采用 XML 文档、 XML 元素、 XML 处理指令，XML 注释或 CDATA 节的形式。

__请注意。__ 此规范仅包含足够多的 XML 来描述的 Visual Basic 语言的行为的说明。 XML 的详细信息可从 http://www.w3.org/TR/REC-xml/。


### <a name="lexical-rules"></a>词法规则

```antlr
XMLCharacter
    : '<Unicode tab character (0x0009)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode carriage return character (0x000D)>'
    | '<Unicode characters 0x0020 - 0xD7FF>'
    | '<Unicode characters 0xE000 - 0xFFFD>'
    | '<Unicode characters 0x10000 - 0x10FFFF>'
    ;

XMLString
    : XMLCharacter+
    ;

XMLWhitespace
    : XMLWhitespaceCharacter+
    ;

XMLWhitespaceCharacter
    : '<Unicode carriage return character (0x000D)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode space character (0x0020)>'
    | '<Unicode tab character (0x0009)>'
    ;

XMLNameCharacter
    : XMLLetter
    | XMLDigit
    | '.'
    | '-'
    | '_'
    | ':'
    | XMLCombiningCharacter
    | XMLExtender
    ;

XMLNameStartCharacter
    : XMLLetter
    | '_'
    | ':'
    ;

XMLName
    : XMLNameStartCharacter XMLNameCharacter*
    ;

XMLLetter
    : '<Unicode character as defined in the Letter production of the XML 1.0 specification>'
    ;

XMLDigit
    : '<Unicode character as defined in the Digit production of the XML 1.0 specification>'
    ;

XMLCombiningCharacter
    : '<Unicode character as defined in the CombiningChar production of the XML 1.0 specification>'
    ;

XMLExtender
    : '<Unicode character as defined in the Extender production of the XML 1.0 specification>'
    ;
```

使用 XML 的词汇规则而不正则 Visual Basic 代码的词汇规则解释 XML 文本表达式。 两组规则通常按以下方式不同：

* 空白是重要的 XML 中。 因此，XML 文本表达式的语法显式声明允许有空格。 不保留空格，除非在其出现在元素内的字符数据的上下文中。 例如：

  ```vb
  ' The following element preserves no whitespace
  Dim e1 = _
      <customer>
          <name>Bob</>
      </>

  ' The following element preserves all of the whitespace
  Dim e2 = _
      <customer>
          Bob
      </>
  ```

* 根据 XML 规范，XML 行尾空格进行规范化。

* XML 区分大小写。 关键字必须匹配大小写，否则将发生编译时错误。

* 行终止符被视为 XML 中的空白区域。 因此，没有行继续符的字符都需要 XML 文本表达式。

* XML 不接受全角字符。 如果使用了全宽字符，将发生编译时错误。


### <a name="embedded-expressions"></a>使用嵌入式的表达式

XML 文本表达式可以包含*嵌入表达式*。 嵌入的表达式是 Visual Basic 表达式的计算和用于在嵌入式表达式的位置的一个或多个值中填充。

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

例如，下面的代码将字符串`John Smith`作为 XML 元素的值：

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

可以在许多上下文中嵌入表达式。 例如，下面的代码生成名为的元素`customer`:

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

可以使用嵌入式的表达式，其中每个上下文指定将接受的类型。 当在嵌入式表达式的表达式一部分的上下文中，Visual Basic 代码的正常词法规则仍适用，，必须为例，使用行继续符：

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a>XML 文档

```antlr
XMLDocument
    : XMLDocumentPrologue XMLMisc* XMLDocumentBody XMLMisc*
    ;

XMLDocumentPrologue
    : '<' '?' 'xml' XMLVersion XMLEncoding? XMLStandalone? XMLWhitespace? '?' '>'
    ;

XMLVersion
    : XMLWhitespace 'version' XMLWhitespace? '=' XMLWhitespace? XMLVersionNumberValue
    ;

XMLVersionNumberValue
    : SingleQuoteCharacter '1' '.' '0' SingleQuoteCharacter
    | DoubleQuoteCharacter '1' '.' '0' DoubleQuoteCharacter
    ;

XMLEncoding
    : XMLWhitespace 'encoding' XMLWhitespace? '=' XMLWhitespace? XMLEncodingNameValue
    ;

XMLEncodingNameValue
    : SingleQuoteCharacter XMLEncodingName SingleQuoteCharacter
    | DoubleQuoteCharacter XMLEncodingName DoubleQuoteCharacter
    ;

XMLEncodingName
    : XMLLatinAlphaCharacter XMLEncodingNameCharacter*
    ;

XMLEncodingNameCharacter
    : XMLUnderscoreCharacter
    | XMLLatinAlphaCharacter
    | XMLNumericCharacter
    | XMLPeriodCharacter
    | XMLDashCharacter
    ;

XMLLatinAlphaCharacter
    : '<Unicode Latin alphabetic character (0x0041-0x005a, 0x0061-0x007a)>'
    ;

XMLNumericCharacter
    : '<Unicode digit character (0x0030-0x0039)>'
    ;

XMLHexNumericCharacter
    : XMLNumericCharacter
    | '<Unicode Latin hex alphabetic character (0x0041-0x0046, 0x0061-0x0066)>'
    ;

XMLPeriodCharacter
    : '<Unicode period character (0x002e)>'
    ;

XMLUnderscoreCharacter
    : '<Unicode underscore character (0x005f)>'
    ;

XMLDashCharacter
    : '<Unicode dash character (0x002d)>'
    ;

XMLStandalone
    : XMLWhitespace 'standalone' XMLWhitespace? '=' XMLWhitespace? XMLYesNoValue
    ;

XMLYesNoValue
    : SingleQuoteCharacter XMLYesNo SingleQuoteCharacter
    | DoubleQuoteCharacter XMLYesNo DoubleQuoteCharacter
    ;

XMLYesNo
    : 'yes'
    | 'no'
    ;

XMLMisc
    : XMLComment
    | XMLProcessingInstruction
    | XMLWhitespace
    ;

XMLDocumentBody
    : XMLElement
    | XMLEmbeddedExpression
    ;
```

XML 文档的结果值的类型为`System.Xml.Linq.XDocument`。 XML 文本表达式中的 XML 文档所需与 XML 1.0 规范中，指定 XML 文档序言;XML 文本表达式，而无需在 XML 文档序言被解释为单个实体。 例如：

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

一个 XML 文档可以包含嵌入式的表达式的类型可以是任何类型;在运行时，但是，该对象必须满足的要求`XDocument`构造函数或运行时错误会发生。

与常规 XML 不同 XML 文档表达式不支持 Dtd （文档类型声明）。 此外，编码属性，如果提供，将忽略由于编码的 Xml 文本表达式始终与源文件本身的编码相同。

__请注意。__ 尽管将忽略 encoding 属性，它将是仍有效的特性，以便维护源代码中包含任何有效的 Xml 1.0 文档的功能。


### <a name="xml-elements"></a>XML 元素

```antlr
XMLElement
    : XMLEmptyElement
    | XMLElementStart XMLContent XMLElementEnd
    ;

XMLEmptyElement
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '/' '>'
    ;

XMLElementStart
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '>'
    ;

XMLElementEnd
    : '<' '/' '>'
    | '<' '/' XMLQualifiedName XMLWhitespace? '>'
    ;

XMLContent
    : XMLCharacterData? ( XMLNestedContent XMLCharacterData? )+
    ;

XMLCharacterData
    : '<Any XMLCharacterDataString that does not contain the string "]]>">'
    ;

XMLCharacterDataString
    : '<Any Unicode character except < or &>'+
    ;

XMLNestedContent
    : XMLElement
    | XMLReference
    | XMLCDATASection
    | XMLProcessingInstruction
    | XMLComment
    | XMLEmbeddedExpression
    ;

XMLAttribute
    : XMLWhitespace XMLAttributeName XMLWhitespace? '=' XMLWhitespace? XMLAttributeValue
    | XMLWhitespace XMLEmbeddedExpression
    ;

XMLAttributeName
    : XMLQualifiedNameOrExpression
    | XMLNamespaceAttributeName
    ;

XMLAttributeValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    | XMLEmbeddedExpression
    ;

XMLAttributeDoubleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or DoubleQuoteCharacter>'
    | XMLReference
    ;

XMLAttributeSingleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or SingleQuoteCharacter>'
    | XMLReference
    ;

XMLReference
    : XMLEntityReference
    | XMLCharacterReference
    ;

XMLEntityReference
    : '&' XMLEntityName ';'
    ;

XMLEntityName
    : 'lt' | 'gt' | 'amp' | 'apos' | 'quot'
    ;

XMLCharacterReference
    : '&' '#' XMLNumericCharacter+ ';'
    | '&' '#' 'x' XMLHexNumericCharacter+ ';'
    ;
```

XML 元素的结果值的类型为`System.Xml.Linq.XElement`。 与常规 XML 不同 XML 元素可以省略的结束标记中的名称，并将关闭当前的大多数嵌套元素。 例如：

```vb
Dim name = <name>Bob</>
```

特性声明中 XML 元素中生成值的类型为`System.Xml.Linq.XAttribute`。 根据 XML 规范，属性值进行规范化。 当属性的值是`Nothing`属性将不会创建，因此不需要检查的属性值表达式`Nothing`。 例如：

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

XML 元素和属性可以包含在以下位置中的嵌套的表达式：

在这种情况下嵌入式表达式必须隐式转换为类型的值的元素的名称`System.Xml.Linq.XName`。 例如：

```vb
Dim name = <<%= "name" %>>Bob</>
```

在这种情况下嵌入式表达式必须隐式转换为类型的值的元素的属性的名称`System.Xml.Linq.XName`。 例如：

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

在其中用例嵌入式表达式可以是任何类型的值的元素的属性的值。 例如：

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

在其中用例嵌入式表达式可以是任何类型的值的元素的属性。 例如：

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

在其中用例嵌入式表达式可以是任何类型的值的元素的内容。 例如：

```vb
Dim name = <name><%= "Bob" %></>
```

如果嵌入式表达式的类型为`Object()`，该数组将传递到 paramarray 作为`XElement`构造函数。


### <a name="xml-namespaces"></a>XML 命名空间

由 XML 命名空间 1.0 规范定义的 XML 元素可以包含 XML 命名空间声明。

```antlr
XMLNamespaceAttributeName
    : XMLPrefixedNamespaceAttributeName
    | XMLDefaultNamespaceAttributeName
    ;

XMLPrefixedNamespaceAttributeName
    : 'xmlns' ':' XMLNamespaceName
    ;

XMLDefaultNamespaceAttributeName
    : 'xmlns'
    ;

XMLNamespaceName
    : XMLNamespaceNameStartCharacter XMLNamespaceNameCharacter*
    ;

XMLNamespaceNameStartCharacter
    : '<Any XMLNameCharacter except :>'
    ;

XMLNamespaceNameCharacter
    : XMLLetter
    | '_'
    ;

XMLQualifiedNameOrExpression
    : XMLQualifiedName
    | XMLEmbeddedExpression
    ;

XMLQualifiedName
    : XMLPrefixedName
    | XMLUnprefixedName
    ;

XMLPrefixedName
    : XMLNamespaceName ':' XMLNamespaceName
    ;

XMLUnprefixedName
    : XMLNamespaceName
    ;
```

定义命名空间的限制`xml`和`xmlns`强制的并且将产生编译时错误。 XML 命名空间声明不能有它们的值; 一个嵌入式的表达式提供的值必须为非空字符串。 例如：

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

__请注意。__ 此规范仅包含足够多的 XML 命名空间来介绍了 Visual Basic 语言的行为的说明。 XML 命名空间的详细信息可从 http://www.w3.org/TR/REC-xml-names/。

可以使用命名空间名称限定 XML 元素和属性名称。 命名空间如下所示常规 XML 绑定时，请在文件级别声明的任何命名空间导入的异常被视为在封闭的声明，本身包含的声明由编译的任何命名空间导入上下文中声明环境。 如果找不到命名空间名称，将发生编译时错误。 例如：

```vb
Imports System.Xml.Linq
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' Binds to the imported namespace above.
        Dim c1 = <db:customer>Bob</>

        ' Binds to the namespace declaration in the element
        Dim c2 = _
            <db:customer xmlns:db="http://example.org/database-other">Mary</>

        ' Binds to the inner namespace declaration
        Dim c3 = _
            <database xmlns:db="http://example.org/database-one">
                <db:customer xmlns:db="http://example.org/database-two">Joe</>
            </>

        ' Error: namespace db2 cannot be found
        Dim c4 = _
            <db2:customer>Jim</>
    End Sub
End Module
```

在元素中声明的 XML 命名空间不适用于 XML 文本内嵌入的表达式。 例如：

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

__请注意。__ 这是因为在嵌入式的表达式可以是任何内容，包括函数调用。 如果函数调用包含 XML 文本表达式，它是不清楚是否程序员希望要应用或忽略的 XML 命名空间。


### <a name="xml-processing-instructions"></a>XML 处理指令

XML 处理指令的结果值的类型为`System.Xml.Linq.XProcessingInstruction`。 XML 处理指令不能包含嵌入的表达式，因为它们是处理指令中的有效语法。

```antlr
XMLProcessingInstruction
    : '<' '?' XMLProcessingTarget ( XMLWhitespace XMLProcessingValue? )? '?' '>'
    ;

XMLProcessingTarget
    : '<Any XMLName except a casing permutation of the string "xml">'
    ;

XMLProcessingValue
    : '<Any XMLString that does not contain a question-mark followed by ">">'
    ;
```

### <a name="xml-comments"></a>XML 注释

XML 注释的结果值的类型为`System.Xml.Linq.XComment`。 XML 注释不能包含嵌入的表达式，因为它们是注释中的有效语法。

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a>CDATA 节

CDATA 节的结果值的类型为`System.Xml.Linq.XCData`。 CDATA 节不能包含嵌入的表达式，因为它们是将 CDATA 节中的有效语法。

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a>XML 成员访问表达式

XML 成员访问表达式可以访问 XML 值的成员。

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

有三种类型的 XML 成员访问表达式：

* *元素访问*，在其中的 XML 名称遵循单一点。 例如：

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  元素访问映射到的函数：

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  因此上面的示例等同于：

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* *属性访问*，在其中是 Visual Basic 标识符遵循一个圆点和一个在登录或 XML 名称遵循一个圆点和一个 at 符号。 例如：

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  属性访问映射到函数：

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  因此上面的示例等同于：

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  __请注意。__ `AttributeValue`扩展方法 (以及相关的扩展属性`Value`) 目前未在任何程序集中定义。 如果需要扩展成员，它们会自动定义正在生成的程序集。

* *后代访问*，在其中 XML 名称遵循三个点。 例如：

  ```vb
  Dim company = _
      <company>
          <customers>
              <customer>Bob</>
              <customer>Mary</>
              <customer>Joe</>
          </>
      </>
  Dim customers = company...<customer>
  ```

  后代访问映射到的函数：

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  因此上面的示例等同于：

  ```vb
  Dim customers = company.Descendants("customer")
  ```

XML 成员访问表达式的基表达式必须是一个值，并必须为类型：

* 如果元素或其子代访问，请`System.Xml.Linq.XContainer`或派生的类型，或`System.Collections.Generic.IEnumerable(Of T)`或派生的类型，其中`T`是`System.Xml.Linq.XContainer`或派生的类型。

* 如果属性访问`System.Xml.Linq.XElement`或派生的类型，或`System.Collections.Generic.IEnumerable(Of T)`或派生的类型，其中`T`是`System.Xml.Linq.XElement`或派生的类型。

XML 成员访问表达式中的名称不能为空。 它们可以使用任何由导入定义的命名空间的限定命名空间。 例如：

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim customer = _
            <db:customer>
                <db:name>Bob</>
            </>
        Dim name = customer.<db:name>
    End Sub
End Module
```

后 dot(s) 在 XML 成员访问表达式中，或在尖括号内和名称之间不允许有空格。 例如：

```vb
Dim customer = _
    <customer age="30">
        <name>Bob</>
    </>
' All the following are error cases
Dim age = customer.@ age
Dim name = customer.< name >
Dim names = customer...< name >
```

如果在类型`System.Xml.Linq`命名空间不可用，则 XML 成员访问表达式将导致编译时错误。


## <a name="await-operator"></a>Await 运算符

Await 运算符与异步方法，部分所述[异步方法](statements.md#async-methods)。

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

`Await` 是一个保留的字，如果它在其中出现的立即封闭方法或 lambda 表达式具有`Async`修饰符，并且如果`Await`后，将显示`Async`修饰符; 它在其他位置未保留空间。 它也是在预处理器指令未保留空间。 Await 运算符仅允许在其所在的保留的字方法或 lambda 表达式的正文中。 在最近的封闭方法或 lambda，await 表达式可能不会发生的表体内`Catch`或`Finally`块中，也不内部的正文`SyncLock`语句，也不在查询表达式。

Await 运算符采用单个表达式，其必须分类为一个值，其类型必须是*awaitable*类型，或`Object`。 如果其类型为`Object`然后所有处理都延迟到运行时。 一种类型`C`说是可满足以下所有条件时，可等待操作：

* `C` 包含名为可访问的实例或扩展方法`GetAwaiter`其中没有参数，这会返回一些类型`E`;

* `E` 包含可读的实例或扩展属性，将名为`IsCompleted`它不采用任何参数，并具有类型布尔值;

* `E` 包含可访问实例或扩展的方法名为`GetResult`这不采用任何参数;

* `E` 实现`System.Runtime.CompilerServices.INotifyCompletion`或`ICriticalNotifyCompletion`。

如果`GetResult`已`Sub`，则 await 表达式分类为 void。 否则为 await 表达式分类为一个值，且其类型为的返回类型`GetResult`方法。

下面是类的可以处于等待状态的示例：

```vb
Class MyTask(Of T)
    Function GetAwaiter() As MyTaskAwaiter(Of T)
        Return New MyTaskAwaiter With {.m_Task = Me}
    End Function

    ...
End Class

Structure MyTaskAwaiter(Of T)
    Implements INotifyCompletion

    Friend m_Task As MyTask(Of T)

    ReadOnly Property IsCompleted As Boolean
        Get
            Return m_Task.IsCompleted
        End Get
    End Property

    Sub OnCompleted(r As Action) Implements INotifyCompletion.OnCompleted
        ' r is the "resumptionDelegate"
        Dim sc = SynchronizationContext.Current
        If sc Is Nothing Then
            m_Task.ContinueWith(Sub() r())
        Else
            m_Task.ContinueWith(Sub() sc.Post(Sub() r(), Nothing))
        End If
    End Sub

    Function GetResult() As T
        If m_Task.IsCanceled Then Throw New TaskCanceledException(m_Task)
        If m_Task.IsFaulted Then Throw m_Task.Exception.InnerException
        Return m_Task.Result
    End Function
End Structure
```

__请注意。__ 建议将库作者，以遵循模式，它们将调用同一个延续委托`SynchronizationContext`作为其`OnCompleted`本身上调用。 此外，恢复委托不应执行以同步方式内`OnCompleted`方法因为这可能会导致堆栈溢出： 相反，应进行委托排队以便后续执行。

当控制流到达`Await`运算符，行为如下所示。

1.  `GetAwaiter`调用 await 操作数的方法。 此调用的结果称为*awaiter*。

2.  Awaiter 的`IsCompleted`检索属性。 如果结果为 true 则：

    21. `GetResult` Awaiter 的方法调用。 如果`GetResult`为一个函数，则 await 表达式的值是此函数的返回值。

3.  如果不是这样的 IsCompleted 属性然后：

    31. 任一`ICriticalNotifyCompletion.UnsafeOnCompleted`等待程序上调用 (如果 awaiter 的类型`E`实现`ICriticalNotifyCompletion`) 或`INotifyCompletion.OnCompleted`（否则为）。 在这种情况下，它将传递*恢复委托*与异步方法的当前实例相关联。

    32. 当前的异步方法实例的控制点已挂起，但控制流中的简历*当前调用方*(部分中定义[异步方法](statements.md#async-methods))。

    33. 当调用恢复委托时更高版本

        331. 首先还原恢复委托`System.Threading.Thread.CurrentThread.ExecutionContext`到它时为`OnCompleted`调用，
        332. 然后它会继续在异步方法实例的控制点控制流 (请参阅部分[异步方法](statements.md#async-methods))，
        333. 它将调用其中`GetResult`awaiter，如上面的 2.1 中所示的方法。

如果 await 操作数具有类型为 Object，此行为将推迟到运行时：

- 通过调用不带任何参数; GetAwaiter() 完成步骤 1它可能会因此绑定在运行时实例方法采用可选参数。
- 第 2 步是通过检索 IsCompleted() 属性不带任何参数，以及通过尝试的内部函数转换为布尔值来完成。
- 步骤 3.a 通过尝试`TryCast(awaiter, ICriticalNotifyCompletion)`，并且如果此失败然后`DirectCast(awaiter, INotifyCompletion)`。

只可以一次调用恢复委托 3.a 中传递。 如果多次调用它，该行为不确定。

