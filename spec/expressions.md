---
ms.openlocfilehash: 8f250f8cee957fa60e7970ace250e8d7ce145fd7
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306164"
---
# <a name="expressions"></a>表达式

表达式是运算符和操作数的序列，用于指定值的计算或指定变量或常数。 本章定义语法、操作数和运算符的计算顺序，以及表达式的含义。

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

每个表达式都分类为以下内容之一：

* *一个值。* 每个值都有关联的类型。

* *一个变量。* 每个变量都有关联的类型，即变量的声明类型。

* *命名空间。* 具有此分类的表达式只能显示为成员访问的左侧。 在任何其他上下文中，归类为命名空间的表达式会导致编译时错误。

* *类型。* 具有此分类的表达式只能显示为成员访问的左侧。 在其他任何上下文中，归类为类型的表达式会导致编译时错误。

* *一个方法组，* 它是在同一名称上重载的一组方法。 一个方法组可以有一个关联的目标表达式和一个关联的类型参数列表。

* *一个方法指针，* 表示方法的位置。 方法指针可以具有关联的目标表达式和关联的类型参数列表。

* *Lambda 方法，* 它是一种匿名方法。

* *属性组，* 它是在同一名称上重载的一组属性。 属性组可能具有关联的目标表达式。

* *属性访问。* 每个属性访问都有关联的类型，即属性的类型。 属性访问可能具有关联的目标表达式。

* *后期绑定访问，* 表示在运行时延迟的方法或属性访问。 后期绑定访问可能具有关联的目标表达式和关联的类型参数列表。 后期绑定访问的类型始终 `Object`。

* *事件访问。* 每个事件访问都有关联的类型，即事件的类型。 事件访问可能具有关联的目标表达式。 事件访问可能显示为 `RaiseEvent`、`AddHandler` 和 @no__t 语句的第一个参数。 在其他任何上下文中，归类为事件访问的表达式会导致编译时错误。

* *数组文本，* 表示其类型尚未确定的数组的初始值。

* *导致.* 当表达式为子程序的调用或没有结果的 await 运算符表达式时，会发生这种情况。 归类为 void 的表达式仅在调用语句或 await 语句的上下文中有效。

* *默认值。* 只有文本 `Nothing` 生成此分类。

表达式的最终结果通常是一个值或变量，其他类别的表达式作为仅在某些上下文中允许的中间值。

请注意，其类型为类型参数的表达式可用于需要表达式的类型具有特定特征（如引用类型、值类型、从某种类型派生等的语句和表达式）的表达式。在类型参数上，满足这些特性。

### <a name="expression-reclassification"></a>表达式重新分类

通常情况下，如果在需要与表达式的分类不同的上下文中使用表达式，则会发生编译时错误，例如，尝试为文本赋值。 但是，在许多情况下，可以通过重新*分类*的过程更改表达式的分类。

如果重新分类成功，则重新分类将被视为扩大或缩小。 除非另有说明，否则此列表中的所有 reclassifications 都是扩大的。

可对以下类型的表达式进行重新分类：

* 变量可以重新分类为值。 将提取存储在变量中的值。

* 可以将方法组重新分类为值。 方法组表达式被解释为带有关联目标表达式和类型参数列表的调用表达式，空括号（也就是说，`f` 将解释为 `f()`，`f(Of Integer)` 解释为 `f(Of Integer)()`）。 这种重新分类可能会导致表达式进一步重新分类为 void。

* 可以将方法指针重新分类为值。 这种重新分类只能在目标类型已知的转换上下文中发生。 方法指针表达式被解释为具有关联类型参数列表的适当类型的委托实例化表达式的参数。 例如：
    
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

* Lambda 方法可重新分类为值。 如果在目标类型已知的转换上下文中发生重新分类，则可能出现以下两个 reclassifications 之一：
    
  1. 如果目标类型为委托类型，则将 lambda 方法解释为适当类型的委托构造表达式的参数。
    
  2. 如果目标类型为 `System.Linq.Expressions.Expression(Of T)`，并且 @no__t 为委托类型，则将 lambda 方法解释为在 `T` 的委托构造表达式中使用它，然后将其转换为表达式树。
    
  如果委托没有 ByRef 参数，则异步或迭代器 lambda 方法只能解释为委托构造表达式的参数。
    
  如果从任何委托的参数类型到相应 lambda 参数类型的转换是收缩转换，则重新分类将被视为收缩转换;否则为扩大。
    
  __纪录.__ Lambda 方法和表达式树之间的确切转换可能不会在编译器版本之间固定，并且超出了本规范的范围。 对于 Microsoft Visual Basic 11.0，所有 lambda 表达式都可以转换为表达式树，但受到以下限制：(1) 1.  只有无 ByRef 参数的单行 lambda 表达式才能转换为表达式树。 对于单行 @no__t lambda，只能将调用语句转换为表达式树。 （2）如果使用早期的字段初始值设定项初始化后续字段初始值设定项（例如 `New With {.a=1, .b=.a}`），则不能将匿名类型表达式转换为表达式树。 （3）如果当前要初始化的对象的成员在一个字段初始值设定项中使用（例如 `New C1 With {.a=1, .b=.Method1()}`），则不能将其转换为表达式树。 （4）仅当多维数组创建表达式显式声明其元素类型时，才能转换为表达式树。 （5）后期绑定表达式不能转换为表达式树。 （6）当变量或字段将 ByRef 传递到调用表达式时，但其类型与 ByRef 参数的类型不完全相同时，或者当属性传递 ByRef 时，一般 VB 语义是参数的副本传递 ByRef，然后再复制其最终值 返回到变量或字段或属性。 在表达式树中，不会进行复制。 （7）所有这些限制也适用于嵌套的 lambda 表达式。
    
  如果目标类型未知，则将 lambda 方法解释为具有 lambda 方法的相同签名的匿名委托类型的委托实例化表达式的参数。 如果使用了严格语义并且省略了任何参数的类型，则会发生编译时错误;否则，`Object` 替换为缺少的任何参数类型。 例如：
    
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

* 可以将属性组重新分类为属性访问。 属性组表达式被解释为带有空括号的索引表达式（即，@no__t 为 `f()`）。

* 属性访问可以重新分类为值。 "属性访问表达式" 被解释为属性 `Get` 访问器的调用表达式。 如果该属性没有 getter，则会发生编译时错误。

* 可以将后期绑定访问重新分类为后期绑定方法或后期绑定属性访问。 如果可以将后期绑定访问同时作为方法访问和属性访问进行重新分类，则优先使用属性访问。

* 可以将后期绑定访问重新分类为值。

* 数组文本可以重新分类为值。 确定值的类型，如下所示：

  1. 如果在目标类型已知并且目标类型是数组类型的转换上下文中发生重新分类，则数组文本将被重新分类为 T （）类型的值。 如果目标类型为 `System.Collections.Generic.IList(Of T)`，`IReadOnlyList(Of T)`，`ICollection(Of T)`，`IReadOnlyCollection(Of T)` 或 @no__t，数组文本具有一级嵌套，则数组文本将被重新分类为类型为 `T()` 的值。

  2. 否则，数组文本将被重新分类为一个值，该值的类型为等于嵌套的级别的数组，元素类型是由初始值设定项中的元素的主导类型确定的;如果不能确定任何主导类型，则使用 `Object`。 例如：

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

  __纪录.__ 语言版本9.0 和版本10.0 之间的行为稍有不同。 在10.0 之前，数组元素初始值设定项不影响局部变量类型推理，现在不影响。 因此 `Dim a() = { 1, 2, 3 }` 将推断 @no__t 为9.0 版中的 `a` 的类型 @no__t，在版本10.0 中为。

  重新分类后，数组文本重新解释为数组创建表达式。 例如：

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  等效于：

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  如果从元素表达式到数组元素类型的任何转换都是收缩的，则重新分类将被视为收缩;否则，会将其视为扩大。

* 默认值 `Nothing` 可重新分类为值。 在目标类型已知的上下文中，结果是目标类型的默认值。 在目标类型未知的上下文中，结果为类型为 `Object` 的 null 值。

命名空间表达式、类型 expression、事件访问表达式或 void 表达式不能进行重新分类。 可以同时执行多个 reclassifications。 例如：

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

在这种情况下，属性组表达式 `P` 首先从属性组重新分类为属性访问，然后从属性访问值重新分类为值。 执行的 reclassifications 数最少，以达到上下文中的有效分类。

## <a name="constant-expressions"></a>常量表达式

*常数表达式*是一个表达式，其值可以在编译时完全计算。

```antlr
ConstantExpression
    : Expression
    ;
```

常数表达式的类型可以是 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、@no__t、@no__t、@no__t、@no__t、@no__t、@no__t、@no__t、@no__t、@no__t、@no__t、或任意枚举类型。 常数表达式中允许使用以下构造：

* 文本（包括 `Nothing`）。

* 对常量类型成员或常量局部变量的引用。

* 对枚举类型的成员的引用。

* 带括号的子表达式。

* 强制表达式，前提是目标类型是上面列出的类型之一。 与 `String` 的强制转换是此规则的例外情况，只允许在 null 值上使用，因为 @no__t 在运行时始终在执行环境的当前区域性中执行转换。 请注意，常量强制表达式只能使用内部转换。

* 如果操作数和结果为上面列出的类型，则为 `+`、`-` 和 `Not` 一元运算符。

* @No__t-0，`-`，`*`，@no__t，`Mod`，`/`，`\`，`<<`，`>>`，`&`，0，1，2，6，7，8，9，0 二元运算符，前提是每个操作数，结果均为上面列出的类型。

* 如果提供每个操作数，并且结果为上面列出的类型，则为条件运算符。

* 以下运行时函数： `Microsoft.VisualBasic.Strings.ChrW`;如果常量值介于0和128之间，则 `Microsoft.VisualBasic.Strings.Chr`;如果常量字符串不为空，则 `Microsoft.VisualBasic.Strings.AscW`;如果常量字符串不为空，则 `Microsoft.VisualBasic.Strings.Asc`。

常量表达式中*不*允许使用以下构造：

* 通过 `With` 上下文的隐式绑定。

整数类型的常量表达式（`ULong`、`Long`、`UInteger`、`Integer`、`UShort`、`Short`、`SByte` 或 @no__t）可隐式转换为较小的整数类型，可以将 `Double` 类型的常量表达式隐式转换为 @no__如果常量表达式的值在目标类型的范围内，则为 t-sql。 无论使用的是许可语义还是严格语义，都可以使用这些收缩转换。


## <a name="late-bound-expressions"></a>后期绑定表达式

如果成员访问表达式或索引表达式的目标类型为 `Object`，则对表达式的处理可能会延迟到运行时。 以这种方式延迟处理称作*后期绑定*。 后期绑定允许以无类型方式使用 @no__t 0 变量，其中*成员的所有*解析都基于变量中值的实际运行时类型。 如果通过编译环境或 `Option Strict` 指定了严格语义，后期绑定将导致编译时错误。 执行后期绑定时，将忽略非公共成员，包括用于重载决策的目的。 请注意，与早期绑定的情况不同，调用或访问后期绑定的 @no__t 0 成员将导致调用目标在运行时进行计算。 如果表达式是在 `System.Object` 上定义的成员的调用表达式，则不会进行后期绑定。

通常，在运行时，通过查找表达式的实际运行时类型的标识符来解析后期绑定访问。 如果后期绑定成员查找在运行时失败，则会引发 `System.MissingMemberException` 异常。 由于后期绑定成员查找只是通过关联目标表达式的运行时类型来完成的，因此对象的运行时类型永远不是接口。 因此，不可能访问后期绑定成员访问表达式中的接口成员。

后期绑定成员访问的参数将按照它们在成员访问表达式中出现的顺序进行计算：不是在后期绑定成员中声明参数的顺序。 下面的示例阐释了这种差异：

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

此代码显示：

```console
Early-bound: xy
Late-bound: yx
```

由于后期绑定重载解析是在参数的运行时类型上完成的，因此可能会根据在编译时或运行时计算表达式的结果来生成不同的结果。 下面的示例阐释了这种差异：

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

此代码显示：

```console
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a>简单表达式

简单表达式包括文本、带括号的表达式、实例表达式或简单名称表达式。

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

文本表达式的计算结果为文本所表示的值。 文本表达式归类为值，但文本 `Nothing`，它归类为默认值。

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a>带括号的表达式

带括号的表达式包含用括号括起来的表达式。 带括号的表达式归类为一个值，并且包含的表达式必须分类为值。 带括号的表达式的计算结果为括号内的表达式的值。

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a>实例表达式

*实例表达式*为关键字 `Me`。 它只能在非共享方法、构造函数或属性访问器的主体内使用。 它被分类为值。 关键字 `Me` 表示包含所执行的方法或属性访问器的类型的实例。 如果构造函数显式调用另一个构造函数（节[构造函数](type-members.md#constructors)），则在该构造函数调用之后，不能使用 `Me`，因为尚未构造该实例。

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a>简单名称表达式

*简单名称表达式*包含单个标识符，后跟可选的类型参数列表。

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

名称由以下 "简单名称解析规则" 解析和分类：

1.  从立即封闭的块开始，继续每个封闭的外部块（如果有），如果标识符与局部变量、静态变量、常量局部变量、方法类型参数或参数的名称匹配，则标识符引用匹配的实体。

    如果标识符与局部变量、静态变量或常量局部变量匹配，并且提供了类型参数列表，则会发生编译时错误。 如果标识符与方法类型参数匹配并且提供了类型参数列表，则不会发生匹配且无法继续进行解析。 如果标识符与局部变量匹配，则与之匹配的局部变量为隐式函数或 `Get` 取值函数返回局部变量，该表达式是调用表达式、调用语句或 @no__t 1 表达式的一部分，因此没有匹配项发生并且解决方法继续。

    如果表达式为本地变量、静态变量或参数，则该表达式归类为变量。 如果表达式为方法类型参数，则该表达式归类为类型。 如果表达式是常量，则该表达式归类为值。

2.  对于包含表达式的每个嵌套类型，从最内层开始到最外层，如果对该类型中的标识符的查找产生与可访问成员的匹配：

    21. 如果匹配类型成员是一个类型参数，则结果归类为类型并且是匹配的类型参数。 如果提供了类型参数列表，则不会发生匹配并且解决将继续。
    22. 否则，如果该类型是直接封闭类型，并且查找标识了非共享类型成员，则结果与 `Me.E(Of A)` 形式的成员访问相同，其中 @no__t 为标识符，`A` 为类型参数列表（如果有）。
    23. 否则，结果与 `T.E(Of A)` 形式的成员访问权限完全相同，其中 @no__t 为包含匹配成员的类型，`E` 为标识符，`A` 为类型参数列表（如果有）。 在这种情况下，标识符引用非共享成员是错误的。

3.  对于每个嵌套命名空间，从最内层到最外面的命名空间，执行以下操作：

    31. 如果命名空间包含具有给定名称的可访问类型，并且具有与类型参数列表中提供的相同数量的类型参数（如果有），则标识符将引用该类型，并将其分类为类型。
    32. 否则，如果未提供类型参数列表，并且命名空间包含具有给定名称的命名空间成员，则标识符将引用该命名空间，并归类为命名空间。
    33. 否则，如果命名空间包含一个或多个可访问的标准模块，并且标识符的成员名称查找只在一个标准模块中产生可访问的匹配项，则结果与 `M.E(Of A)` 形式的成员访问权限完全相同，其中 @no__t 是包含匹配成员的标准模块，@no__t 为标识符，`A` 为类型参数列表（如果有）。 如果标识符在多个标准模块中匹配可访问类型成员，则会发生编译时错误。

4.  如果源文件有一个或多个导入别名，并且标识符与其中一个的名称匹配，则标识符将引用该命名空间或类型。 如果提供了类型参数列表，则会发生编译时错误。

5. 如果包含名称引用的源文件有一个或多个导入：

    51. 如果标识符只匹配一个导入的可访问类型的名称，该类型的类型参数与在类型参数列表中提供的类型参数相同（如有）或类型成员，则标识符将引用该类型或类型成员。 如果标识符中的标识符匹配，则多个导入的可访问类型的名称与类型参数列表中提供的类型参数相同（如果有）或可访问类型成员，发生编译时错误。
    52. 否则，如果未提供任何类型参数列表，并且标识符只匹配一个导入具有可访问类型的命名空间的名称，则标识符将引用该命名空间。 如果未提供类型参数列表，并且标识符与多个导入中的标识符匹配，则会发生编译时错误。
    53. 否则，如果导入包含一个或多个可访问的标准模块，并且标识符的成员名称查找只在一个标准模块中产生可访问的匹配项，则结果与 `M.E(Of A)` 形式的成员访问权限完全相同，其中 `M` 是包含匹配成员的标准模块，@no__t 为标识符，`A` 为类型参数列表（如果有）。 如果标识符在多个标准模块中匹配可访问类型成员，则会发生编译时错误。

6.  如果编译环境定义了一个或多个导入别名，并且标识符与其中一个的名称匹配，则标识符将引用该命名空间或类型。 如果提供了类型参数列表，则会发生编译时错误。

7. 如果编译环境定义了一个或多个导入：

    71. 如果标识符只匹配一个导入的可访问类型的名称，该类型的类型参数与在类型参数列表中提供的类型参数相同（如有）或类型成员，则标识符将引用该类型或类型成员。 如果在多个导入中，标识符与类型参数列表中提供的类型参数（如果有）的名称相同，或类型成员发生编译时错误，则会发生编译时错误。
    72. 否则，如果未提供任何类型参数列表，并且标识符只匹配一个导入具有可访问类型的命名空间的名称，则标识符将引用该命名空间。 如果未提供类型参数列表，并且标识符与多个导入中的标识符匹配，则会发生编译时错误。
    73. 否则，如果导入包含一个或多个可访问的标准模块，并且标识符的成员名称查找只在一个标准模块中产生可访问的匹配项，则结果与 `M.E(Of A)` 形式的成员访问权限完全相同，其中 `M` 是包含匹配成员的标准模块，@no__t 为标识符，`A` 为类型参数列表（如果有）。 如果标识符在多个标准模块中匹配可访问类型成员，则会发生编译时错误。

8. 否则，标识符给定的名称是不确定的。

未定义的简单名称表达式是编译时错误。

通常，名称只能在特定命名空间中出现一次。 但是，由于命名空间可以在多个 .NET 程序集中进行声明，因此在两个程序集定义具有相同完全限定名称的类型时，可能会出现这种情况。 在这种情况下，当前源文件集中声明的类型优先于在外部 .NET 程序集中声明的类型。 否则，名称是不明确的，没有办法消除名称的歧义。


### <a name="addressof-expressions"></a>AddressOf 表达式

@No__t 0 表达式用于生成方法指针。 表达式由 `AddressOf` 关键字和必须归类为方法组或后期绑定访问的表达式组成。 方法组无法引用构造函数。

结果归类为方法指针，其关联的目标表达式和类型参数列表（如果有）将作为方法组。

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a>类型表达式

*类型表达式*是一个 @no__t 的表达式、一个 @no__t 2 表达式、一个 @no__t 表达式或一个 @no__t 4 表达式。

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a>GetType 表达式

@No__t 0 表达式包含关键字 `GetType` 和类型名称。

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

@No__t 0 表达式归类为一个值，其值为反射（`System.Type`）类，表示其*GetTypeTypeName*。 如果*GetTypeTypeName*是一个类型参数，则表达式将返回与在运行时为类型参数提供的类型参数相对应的 `System.Type` 对象。

*GetTypeTypeName*是一种特殊方式：

* 允许 @no__t 为-0，这是语言中可以引用此类型名称的唯一地方。

* 它可能是省略了类型参数的构造泛型类型。 这允许 @no__t 0 表达式返回对应于泛型类型本身的 @no__t 1 对象。

下面的示例演示 @no__t 0 表达式：

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

生成的输出为：

```console
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a>TypeOf .。。为表达式

@No__t 0 表达式用于检查值的运行时类型与给定类型是否兼容。 第一个操作数必须分类为一个值，不能是已重新分类的 lambda 方法，并且必须是引用类型或不受约束的类型参数类型。 第二个操作数必须是类型名称。 表达式的结果归类为值，是 `Boolean` 值。 如果操作数的运行时类型具有标识、默认值、引用、数组、值类型或类型参数转换为类型，则该表达式的计算结果为 `True` 否则，`False`。 如果表达式的类型与特定类型之间不存在转换，则会发生编译时错误。

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a>为表达式

@No__t 0 或 `IsNot` 表达式用于执行引用相等性比较。

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

每个表达式都必须分类为一个值，并且每个表达式的类型必须是引用类型、不受约束的类型参数类型或可以为 null 的值类型。 但是，如果一个表达式的类型是不受约束的类型参数类型或可以为 null 的值类型，则另一个表达式必须是文本 `Nothing`。

结果归类为值，类型为 `Boolean`。 如果两个值都引用同一个实例，或者两个值都是 `Nothing`，则 @no__t 0 运算的计算结果为 `True`; 否则为 `False`。 如果两个值都引用同一个实例，或者两个值都是 `Nothing`，则 @no__t 0 运算的计算结果为 `False`; 否则为 `True`。


### <a name="getxmlnamespace-expressions"></a>GetXmlNamespace 表达式

@No__t 0 表达式包含关键字 `GetXmlNamespace`，以及源文件或编译环境声明的 XML 命名空间的名称。

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

@No__t 0 表达式归类为一个值，其值为表示*XMLNamespaceName*的 @no__t 的实例。 如果该类型不可用，则会发生编译时错误。

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

括号之间的所有内容都被视为命名空间名称的一部分，因此，诸如空格之类的内容都适用于 XML 规则。 例如：

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

还可以省略 XML 命名空间表达式，在这种情况下，表达式将返回表示默认 XML 命名空间的对象。


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

格式 `E.I(Of A)` 的成员访问，其中 `E` 是一个表达式、一个非数组类型名称、关键字 `Global` 或省略，而 `I` 是具有可选类型参数列表 `A` 的标识符，按如下方式进行计算和分类：

1. 如果省略 `E`，则会将直接包含 `With` 语句的表达式替换为 `E`，并执行成员访问。 如果不包含 `With` 语句，则会发生编译时错误。

2. 如果 `E` 归类为命名空间或 @no__t 为关键字 `Global`，则成员查找是在指定命名空间的上下文中完成的。 如果 `I` 是该命名空间中具有与在类型参数列表中提供的相同数量的类型参数（如果有）的辅助性成员的名称，则结果为该成员。 结果归类为命名空间或类型，具体取决于成员。 否则，将发生编译时错误。

3. 如果 @no__t 为类型或分类为类型的表达式，则成员查找在指定类型的上下文中完成。 如果 @no__t 为 @no__t 的辅助性成员的名称，则按如下方式计算 `E.I` 的值并对其进行分类：

    31. 如果 @no__t 为关键字 `New`，`E` 不是枚举，则会发生编译时错误。
    32. 如果 `I` 标识与类型参数列表中提供的类型参数数目相同的类型（如果有），则结果为该类型。
    33. 如果 `I` 标识一个或多个方法，则结果为一个方法组，其中包含关联的类型参数列表，没有关联的目标表达式。
    34. 如果 @no__t 标识一个或多个属性，但未提供任何类型实参列表，则结果是没有关联目标表达式的属性组。
    35. 如果 `I` 标识一个共享变量且未提供任何类型实参列表，则结果为变量或值。 如果该变量是只读的，并且引用出现在声明该变量的类型的共享构造函数之外，则结果为 `E` 中的共享变量的值 `I`。 否则，结果为 `E` 中的共享变量 `I`。
    36. 如果 `I` 标识一个共享事件，但未提供任何类型实参列表，则结果是没有关联目标表达式的事件访问。
    37. 如果 `I` 标识一个常数且未提供任何类型参数列表，则结果为该常量的值。
    38. 如果 @no__t 标识枚举成员但未提供类型参数列表，则结果是该枚举成员的值。
    39. 否则，@no__t 为无效的成员引用，并发生编译时错误。

4. 如果 `E` 归类为变量或值，则其类型 @no__t 为-1，则成员查找在 @no__t 的上下文中完成。 如果 @no__t 为 @no__t 的辅助性成员的名称，则按如下方式计算 `E.I` 的值并对其进行分类：

    41. 如果 @no__t 为关键字 `New`，`E` 为 `Me`、`MyBase` 或 @no__t 5，并且未提供任何类型实参，则结果为一个方法组，该方法组表示 @no__t 为 `E` 的关联目标表达式的实例构造函数，和没有类型参数列表。 否则，将发生编译时错误。
    42. 如果 `I` 标识一个或多个方法（包括扩展方法，如果 `T` 不 `Object`），则结果为一个方法组，其中包含关联的类型自变量列表和一个 @no__t 为3的关联目标表达式。
    43. 如果 @no__t 标识一个或多个属性，但未提供任何类型实参，则结果为属性组，其关联的目标表达式为 `E`。
    44. 如果 `I` 标识一个共享变量或一个实例变量，但未提供任何类型实参，则结果为变量或值。 如果该变量是只读的，并且引用出现在类的构造函数之外，而该构造函数将变量声明为适用于变量的类型（共享或实例），则结果为所引用对象中的变量 `I` `E`。 如果 @no__t 为引用类型，则结果是 `E` 引用的对象中的变量 `I`。 否则，如果 @no__t 为值类型，并且表达式 `E` 归类为变量，则结果为变量;否则，结果为值。
    45. 如果 `I` 标识事件但未提供任何类型实参，则结果为具有 `E` 的关联目标表达式的事件访问。
    46. 如果 `I` 标识一个常数且未提供任何类型参数，则结果为该常量的值。
    47. 如果 @no__t 标识枚举成员但未提供任何类型参数，则结果是该枚举成员的值。
    48. 如果 `T` @no__t 为-1，则结果为使用关联类型参数列表和关联的目标表达式（`E`）归类为后期绑定访问的后期绑定成员查找。

5. 否则，@no__t 为无效的成员引用，并发生编译时错误。

形式 `MyClass.I(Of A)` 形式的成员访问等效于 `Me.I(Of A)`，但在其上访问的所有成员被视为不可重写的成员。 因此，访问的成员将不受访问其成员的值的运行时类型的影响。

形式 `MyBase.I(Of A)` 形式的成员访问等效于 `CType(Me, T).I(Of A)`，其中 `T` 是包含成员访问表达式的类型的直接基类型。 其上的所有方法调用都被视为不可重写的方法。 这种形式的成员访问也称为*基本访问*。

下面的示例演示 `Me`、@no__t 和 `MyClass` 的关系：

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

此代码输出：

```console
MoreDerived.F
Derived.F
Derived.F
```

如果成员访问表达式以关键字 `Global` 开始，则关键字表示最外面的未命名命名空间，这在声明隐藏封闭命名空间的情况下非常有用。 @No__t 的关键字允许在这种情况下将 "转义" 到最外面的命名空间。 例如：

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

在上面的示例中，第一个方法调用无效，因为标识符 `System` 绑定到类 `System`，而不是命名空间 `System`。 访问 `System` 命名空间的唯一方法是使用 `Global` 以转义到最外面的命名空间。

如果要访问的成员是共享的，则该期间左侧的任何表达式都是多余的，除非成员访问是后期绑定的，否则不会对其进行计算。 例如，考虑以下代码：

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

它将打印 `The value of F is: 10`，因为无需调用函数 `ReturnC` 来提供 `C` 的实例来访问共享成员 `F`。


### <a name="identical-type-and-member-names"></a>相同的类型和成员名称

使用与类型相同的名称命名成员并不少见。 但是，在这种情况下，可能会发生名称隐藏：

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

在上面的示例中，`DefaultColor` 中的简单名称 `Color` 绑定到实例属性，而不是类型。 由于在共享成员中无法引用实例成员，这通常是一个错误。

但是，在这种情况下，特殊规则允许访问类型。 如果成员访问表达式的基本表达式为简单名称，并绑定到类型具有相同名称的常量、字段、属性、局部变量或参数，则基表达式可以引用成员或类型。 这绝不会导致歧义，因为可以从任一成员访问的成员都是相同的。

### <a name="default-instances"></a>默认实例

在某些情况下，从公共基类派生的类通常或始终只有一个实例。 例如，用户界面中显示的大多数窗口在任何时候都只有一个实例显示在屏幕上。 为了简化此类类型的使用，Visual Basic 可以自动生成类的*默认实例*，这些类为每个类提供一个易于引用的实例。

始终为类型*系列*而不是一个特定类型创建默认实例。 因此，为从窗体派生的所有类创建默认实例，而不是为从窗体派生的类 Form1 创建默认实例。 这意味着，派生自基类的每个单独类不必专门标记为具有默认实例。

类的默认实例由编译器生成的属性表示，该属性返回该类的默认实例。 作为类的成员生成的属性称为*组类*，它管理从特定基类派生的所有类的默认实例。 例如，可能会在 @no__t 类中收集从 @no__t 派生的类的所有默认实例属性。 如果 `My.Forms` 的表达式返回 group 类的实例，则以下代码将访问派生类的默认实例，`Form1` 和 `Form2`：

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

在第一次引用之前，将不会创建默认实例;如果提取表示默认实例的属性，则会导致创建默认实例（如果尚未创建）或将其设置为 `Nothing`。 若要允许测试默认实例是否存在，当默认实例是 @no__t 0 或 `IsNot` 运算符的目标时，将不会创建默认实例。 因此，可以测试默认实例是否 @no__t 0 或其他引用，而不会导致创建默认实例。

默认实例旨在使从具有默认实例的类的外部引用默认实例变得更加容易。 如果使用中的默认实例，则定义该实例可能会导致对哪个实例的引用（即默认实例或当前实例）造成混淆。 例如，下面的代码仅修改默认实例中 `x` 的值，即使是从其他实例调用它。 因此，代码会将值打印 `5` 而不是 `10`：

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

若要避免这种混淆，从默认实例的类型的实例方法中引用默认实例是无效的。

#### <a name="default-instances-and-type-names"></a>默认实例和类型名称

还可以通过类型的名称直接访问默认实例。 在这种情况下，在不允许类型名称的任何表达式上下文中，表达式 `E`，其中 `E` 表示具有默认实例的类的完全限定名，则更改为 `E'`，其中 `E'` 表示获取的表达式默认实例属性。 例如，如果从 @no__t 派生的类的默认实例允许通过类型名称访问默认实例，则以下代码等效于上一示例中的代码：

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

这也意味着，通过类型名称访问的默认实例也可通过类型名称进行分配。 例如，以下代码将 @no__t 0 的默认实例设置为 `Nothing`：

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

请注意，`E.I` @no__t 表示类，`I` 表示共享成员不会更改。 此类表达式仍直接从类实例访问共享成员，并且不引用默认实例。

#### <a name="group-classes"></a>组类

@No__t-0 属性指示默认实例系列的组类。 该属性具有四个参数：

* 参数 `TypeToCollect` 指定组的基类。 所有不含开放类型参数的开放类型参数均派生自具有此名称的类型（不考虑类型参数）将自动具有默认实例。

* 参数 @no__t 指定要在组类中调用的方法，以便在默认实例属性中创建新实例。

* 参数 @no__t 指定要在组类中调用的方法，以释放默认实例属性（如果为默认实例属性分配了值 `Nothing`）。

* 参数 `DefaultInstanceAlias` 将表达式 @no__t 为-1，以便在可通过其类型名称直接访问默认实例时替换类名称。 如果此参数 `Nothing` 或空字符串，则不能通过其类型名称直接访问此组类型上的默认实例。 （__注意：__ 在 Visual Basic 语言的所有当前实现中，将忽略 @no__t 参数，但编译器提供的代码除外。）

可以通过使用逗号分隔前三个参数中的类型和方法的名称，将多个类型收集到同一个组中。 每个参数中必须有相同数量的项，并且列表元素按顺序匹配。 例如，下面的属性声明将从 `C1`、`C2` 或 `C3` 派生的类型收集到一个组中：

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

Create 方法的签名的格式必须为 `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`。 Dispose 方法的格式必须为 `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`。 因此，可以按如下所示声明前面部分中的示例的 group 类：

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

如果源文件声明派生类 `Form1`，则生成的 group 类将等效于：

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

通过收集在当前上下文中可用的名为 `I` 的所有扩展方法，可收集成员访问表达式 `E.I` 的扩展方法：

1. 首先，将检查包含表达式的每个嵌套类型，从最内层开始到最外面的。
2. 然后，将检查每个嵌套命名空间，从最内层开始到最外面的命名空间。
3. 然后，将检查源文件中的导入。
4. 然后，将检查编译环境定义的导入。

仅当存在从目标表达式类型到扩展方法的第一个参数类型的扩大本机转换时，才收集扩展方法。 和与常规简单名称表达式绑定不同，搜索将收集*所有*扩展方法;找到扩展方法时，集合不会停止。 例如：

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

在此示例中，即使在 `N1C1Extensions.M1` 之前找到 @no__t，它们都被视为扩展方法。 所有扩展方法一经收集后，它们就会*扩充*。 Currying 采用扩展方法调用的目标，并将其应用于扩展方法调用，从而生成新的方法签名，并删除第一个参数（因为已指定了该参数）。 例如：

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

在上面的示例中，将 `v` 应用到 @no__t 的扩充结果是 `Sub M(y As Integer)` 的方法签名。

除了删除扩展方法的第一个参数外，currying 还会删除作为第一个参数类型的一部分的任何方法类型参数。 当 currying 扩展方法与方法类型参数一起使用时，类型推理将应用于第一个参数，并为任何推断出的类型参数修复结果。 如果类型推理失败，则忽略方法。 例如：

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

在上面的示例中，将 `v` 应用到 @no__t 的扩充结果是 `Sub M(Of U)(y As U)` 的方法签名，因为在 currying 的结果中将推断类型参数 `T`，现已修复。 因为类型参数 `U` 未推断为 currying 的一部分，所以它仍是一个打开的参数。 同样，由于在将 @no__t 应用到 `Ext2.M` 的情况下推断类型参数 `T`，因此参数 @no__t 的类型将作为 `Integer` 进行固定。 它不会被推断为任何其他类型。 Currying 签名时，还会应用除 `New` 约束之外的所有约束。 如果未满足约束，或依赖于未推断为 currying 的一部分的类型，则将忽略扩展方法。 例如：

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

__纪录.__ 执行扩展方法 currying 的主要原因之一是它允许查询表达式在计算查询模式方法的参数之前推断迭代的类型。 由于大多数查询模式方法都采用 lambda 表达式，后者需要进行类型推理，这大大简化了计算查询表达式的过程。

与普通的接口继承不同，扩展两个接口之间相互关联的扩展方法是可用的，但前提是它们没有相同的扩充签名：

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

最后，请务必记住，执行后期绑定时不考虑扩展方法：

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

*字典成员访问表达式*用于查找集合的成员。 字典成员访问采用 `E!I` 的形式，其中 @no__t 为作为值分类并且 `I` 是标识符。

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

表达式的类型必须有一个默认属性，该属性由单个 @no__t 零参数进行索引。 字典成员访问表达式 `E!I` 转换为表达式 `E.D("I")`，其中 `D` 是 `E` 的默认属性。 例如：

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

如果在没有表达式的情况下指定了感叹号，则会假设立即包含 `With` 语句的表达式。 如果不包含 `With` 语句，则会发生编译时错误。


## <a name="invocation-expressions"></a>调用表达式

调用表达式由调用目标和可选参数列表组成。

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

目标表达式必须归类为方法组或类型为委托类型的值。 如果目标表达式是其类型为委托类型的值，则调用表达式的目标将成为委托类型的 @no__t 的方法组，而目标表达式将成为该方法组的关联目标表达式。

自变量列表具有两个部分：位置参数和命名参数。 *位置参数*是表达式，并且必须位于任何命名参数之前。 *命名参数*以可以匹配关键字的标识符开头，后跟 `:=` 和表达式。

如果方法组只包含一个可访问的方法（包括实例和扩展方法），并且该方法不使用参数，并且是一个函数，则方法组被解释为带有空参数列表的调用表达式，结果为用作带有所提供的参数列表的调用表达式的目标。 例如：

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

否则，重载决策将应用到方法，以选取最适用于给定自变量列表的方法。 如果最适用的方法是函数，则调用表达式的结果将分类为类型为函数的返回类型的值。 如果最适用的方法是子例程，则结果归类为 void。 如果最适用的方法是没有正文的分部方法，则将忽略调用表达式，并将结果归类为 void。

对于早期绑定调用表达式，将按照在目标方法中声明相应参数的顺序来计算参数。 对于后期绑定成员访问表达式，这些表达式将按照它们在成员访问表达式中出现的顺序进行计算：请参阅[后期绑定表达式](expressions.md#late-bound-expressions)部分。

## <a name="overloaded-method-resolution"></a>重载方法解析：
对于重载决策，指定了参数列表、Genericity、对参数列表的适用性、传递参数以及可选参数、条件方法和类型参数推理的选取参数的成员/类型的适用性：请参阅部分[重载解析](overload-resolution.md)。

## <a name="index-expressions"></a>索引表达式

*索引表达式*将导致数组元素，或将属性组发送器为属性访问。 索引表达式的顺序包括：表达式、左括号、索引参数列表和右括号。

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

索引表达式的目标必须分类为属性组或值。 索引表达式的处理方式如下：

* 如果目标表达式归类为一个值，并且它的类型不是数组类型，`Object` 或 `System.Array`，则该类型必须有一个默认属性。 索引在属性组上执行，该属性组表示该类型的所有默认属性。 尽管在 Visual Basic 中声明无参数的默认属性是无效的，但其他语言可能允许声明这种属性。 因此，允许为不带参数的属性编制索引。

* 如果表达式的值为数组类型的值，则参数列表中的参数数目必须与数组类型的秩相同，并且不能包含命名参数。 如果任何索引在运行时无效，则会引发 `System.IndexOutOfRangeException` 异常。 每个表达式必须可隐式转换为类型 `Integer`。 索引表达式的结果是指定索引处的变量，并归类为变量。

* 如果表达式归类为属性组，则使用重载决策来确定属性之一是否适用于索引参数列表。 如果属性组只包含一个具有 `Get` 访问器的属性，并且该访问器没有参数，则该属性组将被解释为带有空参数列表的索引表达式。 结果用作当前索引表达式的目标。 如果没有可用的属性，则会发生编译时错误。 否则，表达式将导致属性访问具有属性组的关联目标表达式（如果有）。

* 如果表达式归类为后期绑定属性组，或为其类型为 `Object` 或 `System.Array` 的值，则索引表达式的处理将延迟到运行时，并且索引后期绑定。 该表达式将导致后期绑定属性访问类型为 `Object`。 关联的目标表达式是目标表达式（如果它是一个值）或属性组的关联目标表达式。 在运行时，将按如下所示处理表达式：

* 如果表达式归类为后期绑定属性组，则该表达式可能会导致方法组、属性组或值（如果该成员是实例或共享变量）。 如果结果为方法组或属性组，则将重载决策应用于组，以确定参数列表的正确方法。 如果重载决策失败，则会引发 `System.Reflection.AmbiguousMatchException` 异常。 然后，将结果作为属性访问或作为调用进行处理，并返回结果。 如果调用的是子程序，则结果为 `Nothing`。

* 如果目标表达式的运行时类型是数组类型或 `System.Array`，则索引表达式的结果为指定索引处的变量的值。

* 否则，表达式的运行时类型必须有一个默认属性，并对表示该类型上所有默认属性的属性组执行索引。 如果该类型没有默认属性，则会引发 `System.MissingMemberException` 异常。


## <a name="new-expressions"></a>新表达式

@No__t-0 运算符用于创建类型的新实例。 有四种形式的 @no__t 0 表达式：

* 对象创建表达式用于创建类类型和值类型的新实例。

* 数组创建表达式用于创建数组类型的新实例。

* 委托创建表达式（不具有对象创建表达式中的不同语法）用于创建委托类型的新实例。

* 匿名对象创建表达式用于创建匿名类类型的新实例。

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

@No__t 0 表达式归类为值，结果为类型的新实例。


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

对象创建表达式的类型必须是类类型、结构类型或具有 `New` 约束的类型参数，不能是 @no__t 1 类。 给定形式为 `New T(A)` 的对象创建表达式，其中 @no__t 为类类型或结构类型，`A` 是可选参数列表，重载决策确定要调用的 `T` 的正确构造函数。 具有 `New` 约束的类型参数被视为具有一个无参数的构造函数。 如果没有可调用的构造函数，则会发生编译时错误;否则，表达式会导致使用所选构造函数创建 @no__t 的新实例。 如果没有参数，则可以省略括号。

分配实例的位置取决于实例是类类型还是值类型。 类类型 @no__t 0 实例是在系统堆上创建的，而值类型的新实例是直接在堆栈上创建的。

对象创建表达式可以选择在构造函数参数之后指定成员初始值设定项的列表。 这些成员初始值设定项以关键字 `With` 作为前缀，而初始值设定项列表被解释为在 @no__t 1 语句的上下文中。 例如，给定类：

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

大致等效于：

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

每个初始值设定项都必须指定一个要分配的名称，并且该名称必须是正在构造的类型的非 @no__t 实例变量或属性;如果正在构造的类型为 `Object`，则不会对成员访问进行后期绑定。 初始值设定项不能使用 @no__t 关键字。 类型中的每个成员只能初始化一次。 但是，初始值设定项表达式可能相互引用。 例如：

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

初始值设定项是从左到右赋值的，因此，如果初始值设定项引用尚未初始化的成员，则在构造函数运行后，将看到该实例变量的任何值：

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

初始值设定项可以嵌套：

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

如果创建的类型是集合类型，并且具有名为 `Add` 的实例方法（包括扩展方法和共享方法），则对象创建表达式可以指定以关键字 @no__t 为前缀的集合初始值设定项。 对象创建表达式不能同时指定成员初始值设定项和集合初始值设定项。 集合初始值设定项中的每个元素都作为参数传递给 `Add` 函数的调用。 例如：

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

如果某个元素是集合初始值设定项本身，则子集合初始值设定项的每个元素都将作为单个参数传递到 `Add` 函数。 例如，以下内容：

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

等效于：

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

这种展开操作始终完成，只是在一层深度上完成;之后，子初始值设定项被视为数组文本。 例如：

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a>数组表达式

数组表达式用于创建数组类型的新实例。 数组表达式有两种类型：数组创建表达式和数组文本。

#### <a name="array-creation-expressions"></a>数组创建表达式

如果提供了数组大小初始化修饰符，则会通过从数组大小初始化参数列表中删除各个参数来派生生成的数组类型。 每个参数的值确定新分配的数组实例中相应维度的上限。 如果表达式具有非空集合初始值设定项，则参数列表中的每个参数都必须是常量，并且表达式列表指定的秩和维度长度必须与集合初始值设定项的匹配项和维度长度匹配。

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

如果未提供数组大小初始化修饰符，则类型名称必须是数组类型，并且集合初始值设定项必须为空，或者具有与指定数组类型的秩相同的嵌套级别数。 最内层嵌套级别中的所有元素都必须能够隐式转换为数组的元素类型，并且必须分类为值。 每个嵌套集合初始值设定项中的元素数必须始终与处于同一级别的其他集合的大小一致。 各个维度长度是从集合初始值设定项的每个相应嵌套级别中的元素数推断出来的。 如果集合初始值设定项为空，则每个维度的长度为零。

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

集合初始值设定项的最外层嵌套级别对应于数组的最左边的维度，最内层嵌套级别对应于最右边的维度。 示例：

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

等效于以下内容：

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

如果集合初始值设定项为空（即，包含大括号但没有初始值设定项列表的集合），并且已知要初始化的数组的维度边界，则空集合初始值设定项表示指定大小的数组实例，其中所有元素都已初始化为元素类型的默认值。 如果要初始化的数组的维度界限未知，则空集合初始值设定项表示一个数组实例，其中所有维度的大小均为零。

在实例的整个生存期内，每个维度的数组实例的秩和长度都是固定的。 换句话说，不能更改现有数组实例的排名，也不能调整其维度的大小。

#### <a name="array-literals"></a>数组文本

数组文本表示一个数组，该数组的元素类型、秩和边界是从表达式上下文和集合初始值设定项的组合推断而来的。 这将在部分[表达式重新分类](expressions.md#expression-reclassification)中介绍。

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

数组文本中集合初始值设定项的格式和要求与数组创建表达式中的集合初始值设定项的格式和要求完全相同。

__纪录.__ 数组文本不会自行创建数组;相反，将表达式重新分类为导致创建数组的值。 例如，不可能进行转换 `CType(new Integer() {1,2,3}, Short())`，因为没有从 `Integer()` 到 `Short()` 的转换;但表达式 `CType({1,2,3},Short())` 是可能的，因为它首先将数组文本发送器到数组创建表达式 `New Short() {1,2,3}`。


### <a name="delegate-creation-expressions"></a>委托创建表达式

委托创建表达式用于创建委托类型的新实例。 委托创建表达式的参数必须是分类为方法指针或 lambda 方法的表达式。

如果参数是一个方法指针，则该方法指针引用的其中一个方法必须适用于委托类型的签名。 如果以下情况，则方法 `M` 适用于委托类型 `D`：

* `M` 不 @no__t 为-1 或具有正文。

* @No__t-0 和 @no__t 均为函数，或 `D` 是一个子例程。

* @no__t，`D` 具有相同数量的参数。

* @No__t 的参数类型，每个参数类型都具有从 `D` 的相应参数类型的类型到的转换，以及它们的修饰符（即，@no__t 2，`ByVal`）匹配项。

* @No__t 的返回类型（如果有）具有转换为 @no__t 的返回类型。

如果方法指针引用了后期绑定访问，则假定使用的参数数目与委托类型具有相同数量的参数，则假定使用后期绑定访问。

如果未使用 strict 语义并且方法指针只引用了一个方法，但它不适用，原因是它没有参数并且委托类型为，则此方法被视为适用，并且参数或返回值为只是被忽略。 例如：

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

__纪录.__ 仅当由于扩展方法而未使用严格语义时，才允许使用此 relaxation。 由于仅当某个正则方法不适用时才考虑扩展方法，因此，不带参数的实例方法可以使用参数隐藏扩展方法，以进行委托构造。

如果方法指针引用的多个方法适用于委托类型，则使用重载决策在候选方法之间选取。 委托的参数类型将用作重载决策的参数类型的参数。 如果没有一个候选方法，则会发生编译时错误。 在下面的示例中，用引用第二个 `Square` 方法的委托初始化本地变量，因为该方法更适用于 @no__t 的签名和返回类型。

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

如果第二个 `Square` 方法不存在，则将选择第一个 @no__t 方法。 如果通过编译环境或 `Option Strict` 指定了严格语义，则当方法指针引用的最特定方法比委托签名更窄时，会发生编译时错误。 如果为，则方法 `M` 被视为比委托类型窄 `D`：

* @No__t 的参数类型为 `D` 的相应参数类型的扩大转换。

* 或者，@no__t 的返回类型（如果有）具有到 @no__t 的返回类型的收缩转换。

如果类型参数与方法指针相关联，则仅考虑具有相同数量的类型参数的方法。 如果没有与方法指针关联的类型自变量，则在将签名与泛型方法进行匹配时，将使用类型推理。 与其他正常类型推理不同，在推断类型参数时将使用委托的返回类型，但在确定最小泛型重载时仍不会考虑返回类型。 下面的示例演示为委托创建表达式提供类型参数的两种方式：

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

在上面的示例中，非泛型委托类型是使用泛型方法实例化的。 还可以使用泛型方法创建构造委托类型的实例。 例如：

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

如果委托创建表达式的参数是 lambda 方法，则 lambda 方法必须适用于委托类型的签名。 Lambda 方法 `L` 适用于委托类型，`D`：

* 如果 `L` 具有参数，`D` 具有相同数量的参数。 （如果 `L` 没有参数，则忽略 @no__t 的参数。）

* @No__t 的参数类型每个都有一个转换为相应参数类型的类型，@no__t 为-1，以及它们的修饰符（即，@no__t 2，`ByVal`）匹配。

* 如果 `D` 是一个函数，则 @no__t 的返回类型将转换为 @no__t 的返回类型。 （如果 `D` 是一个子例程，则忽略 `L` 的返回值。）

如果省略 @no__t 参数的参数类型，则将推断 `D` 中相应参数的类型;如果 `L` 的参数具有数组或可为 null 的名称修饰符，则会生成编译时错误。 @No__t-0 的所有参数类型都可用后，将推断 lambda 方法中的表达式类型。 例如：

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

在某些情况下，如果委托签名与 lambda 方法或方法签名并不完全匹配，则 .NET Framework 可能不支持本机创建委托。 在这种情况下，使用 lambda 方法表达式来匹配这两种方法。 例如：

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

委托创建表达式的结果是一个委托实例，它从方法指针表达式引用具有关联目标表达式（如果有）的匹配方法。 如果目标表达式的类型为值类型，则将值类型复制到系统堆上，因为委托只能指向堆上的对象的方法。 委托引用的方法和对象在该委托的整个生存期内保持不变。 换句话说，创建委托后，不能更改其目标或对象。

### <a name="anonymous-object-creation-expressions"></a>匿名对象创建表达式

带有成员初始值设定项的对象创建表达式还可以完全省略类型名称。

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

在这种情况下，将基于作为表达式的一部分初始化的成员的类型和名称来构造匿名类型。 例如：

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

由匿名对象创建表达式创建的类型是没有名称的类，它直接从 `Object` 继承，并且具有与成员初始值设定项列表中分配给的成员同名的一组属性。 使用与局部变量类型推理相同的规则来推断每个属性的类型。 生成的匿名类型还会重写 `ToString`，并返回所有成员及其值的字符串表示形式。 （此字符串的准确格式超出了本规范的范围）。

默认情况下，匿名类型生成的属性为读写属性。 可以通过使用 `Key` 修饰符将匿名类型属性标记为只读。 @No__t 的修饰符指定字段可用于唯一标识匿名类型所表示的值。 除了将属性设为只读外，还会导致匿名类型覆盖 `Equals` 和 `GetHashCode`，并实现接口 `System.IEquatable(Of T)` （填充 `T` 的匿名类型）。 成员定义如下：

通过验证两个实例的类型相同，然后使用 `Object.Equals` 比较每个 @no__t 2 成员，实现 `Function Equals(obj As Object) As Boolean` 和 @no__t。 如果所有 `Key` 个成员相等，则 `Equals` 返回 `True`，否则 `Equals` 返回 `False`。

`Function GetHashCode() As Integer` 实现，这是因为如果 @no__t 对于匿名类型的两个实例都为 true，则 `GetHashCode` 将返回相同的值。 哈希值以种子值开头，然后对于每个 @no__t 0 成员，按顺序将哈希乘以31，并添加 @no__t 1 成员的哈希值（由 `GetHashCode` 提供）（如果该成员不是值为 `Nothing` 的引用类型或可以为 null 的值类型）。

例如，在语句中创建的类型：

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

创建一个与此类似的类（尽管完全实现可能会有所不同）：

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

若要简化从另一种类型的字段创建匿名类型的情况，可在以下情况下直接从表达式推断字段名称：

* 简单名称表达式 `x` 将推断名称 `x`。

* 成员访问表达式 `x.y` 将推断名称 @no__t 为-1。

* 字典查找表达式 `x!y` 将推断名称 @no__t 为-1。

* 不带参数 @no__t 的调用或索引表达式-0 将推断名称 `x`。

* XML 成员访问表达式 `x.<y>`，`x...<y>`，`x.@y` 推断名称 @no__t 为3。

* 作为成员访问表达式的目标的 XML 成员访问表达式 `x.<y>.z` 会将名称推断 `z`。

* 如果 XML 成员访问表达式是没有参数的调用表达式或索引表达式的目标 `x.<y>.z()` 会将名称推断 @no__t 为-1。

* 作为调用表达式或索引表达式的目标的 XML 成员访问表达式 `x.<y>(0)` 会将名称推断 @no__t 为-1。

初始值设定项被解释为推断名称的表达式赋值。 例如，以下初始值设定项是等效的：

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

如果推断到与类型的现有成员发生冲突的成员名称（如 `GetHashCode`），则会发生编译时错误。 与常规成员初始值设定项不同，匿名对象创建表达式不允许成员初始值设定项具有循环引用，或者在初始化之前引用成员。 例如：

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

如果两个匿名类创建表达式出现在同一方法中并生成相同的结果形状（如果属性顺序、属性名称和属性类型都匹配），则它们将引用同一个匿名类。 具有初始值设定项的实例或共享成员变量的方法作用域是在其中初始化变量的构造函数。

__纪录.__ 编译器可能会选择进一步统一匿名类型（如在程序集级别上），但此时不会依赖这种情况。


## <a name="cast-expressions"></a>强制转换表达式

强制转换表达式将表达式强制转换为给定类型。 特定强制转换关键字将表达式强制转换为基元类型。 三个常规强制转换关键字 `CType`，`TryCast` 和 `DirectCast`，将表达式强制转换为类型。

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

`DirectCast`，`TryCast` 具有特殊行为。 因此，它们仅支持本机转换。 此外，@no__t 0 表达式中的目标类型不能是值类型。 当使用 @no__t 0 或 @no__t 时，不考虑用户定义的转换运算符。 （__注意：__ @No__t-0 和 @no__t 1 支持的转换集受到限制，因为它们实现了 "本机 CLR" 转换。 @No__t 的目的是提供 "取消装箱" 指令的功能，而 `TryCast` 的目的是提供 "isinst" 指令的功能。 由于它们映射到 CLR 说明，因此不能直接支持的 CLR 不支持的转换将会违背预期目的。）

`DirectCast` 会将类型化为 `Object` 的表达式转换为与 @no__t 2 不同的。 转换其运行时类型为基元值类型 `Object` 类型的表达式时，如果指定的类型与表达式的运行时类型不相同，则 `DirectCast` 将引发 @no__t 2 异常; 如果表达式的计算结果为 `Nothing`，则将引发 @no__t。 （__注意：__ 如上所述，当表达式的类型 @no__t 为-1 时，@no__t 将直接映射到 CLR 指令 "取消装箱"。 与此相反，`CType` 会变为对运行时帮助程序的调用以执行转换，以便可以支持基元类型之间的转换。 如果将 @no__t 0 表达式转换为基元值类型，并且实际实例的类型与目标类型匹配，则 `DirectCast` 将明显快于 `CType`。）

@no__t，如果表达式无法转换为目标类型，则将转换表达式，但不会引发异常。 相反，如果在运行时无法转换表达式，`TryCast` 将导致 @no__t 为-1。 （__注意：__ 如上所述，`TryCast` 直接映射到 CLR 指令 "isinst"。 通过将类型检查和转换合并为单个操作，`TryCast` 比 `TypeOf ... Is` 然后 `CType` 的成本更低。）

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

如果不存在从表达式的类型到指定类型的转换，则会发生编译时错误。 否则，表达式归类为值，结果是转换生成的值。


## <a name="operator-expressions"></a>运算符表达式

有两种类型的运算符。 *一元运算符*采用一个操作数并使用前缀表示法（例如 `-x`）。 *二元运算符*采用两个操作数并使用中缀表示法（例如 `x + y`）。 除关系运算符（始终导致 `Boolean`）以外，为特定类型定义的运算符将导致该类型。 运算符的操作数必须始终归类为一个值;运算符表达式的结果分类为值。

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

当表达式包含多个二元运算符时，运算符的*优先级*控制单个二元运算符的计算顺序。 例如，表达式 `x + y * z` 相当于计算 `x + (y * z)`，因为 `*` 运算符的优先级高于 `+` 运算符。 下表按优先级的降序顺序列出了二元运算符：


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

如果表达式包含两个具有相同优先级的运算符，则运算符的*关联*性将控制运算的执行顺序。 所有二元运算符都是左结合运算符，这意味着运算从左至右执行。 优先级和结合性可以使用带括号的表达式进行控制。

### <a name="object-operands"></a>对象操作数

除每个运算符支持的常规类型外，所有运算符都支持类型为 `Object` 的操作数。 应用于 @no__t 0 操作数的运算符的处理方式类似于对 @no__t 1 值进行的方法调用：可以选择后期绑定方法调用，在这种情况下，操作数的运行时类型（而不是编译时类型）确定的有效性和类型运作. 如果通过编译环境或 `Option Strict` 指定了严格语义，则具有类型 `Object` 的操作数的任何运算符都将导致编译时错误，但 @no__t 2、@no__t 和 @no__t 运算符除外。

当运算符解析确定应该后期绑定执行操作时，如果操作数的运行时类型是运算符支持的类型，则操作的结果是将运算符应用于操作数类型的结果。 值 `Nothing` 被视为二元运算符表达式中另一操作数的类型的默认值。 在一元运算符表达式中，或者，如果两个操作数在二元运算符表达式中 `Nothing`，则操作的类型为 @no__t 为-1 或运算符的唯一结果类型（如果运算符未导致 `Integer`）。 然后，操作的结果将始终强制转换回 `Object`。 如果操作数类型没有有效的运算符，则会引发 `System.InvalidCastException` 异常。 在运行时执行转换，而不考虑它们是否为隐式或显式的。

如果数值二元运算的结果将生成溢出异常（无论是否打开还是关闭整数溢出检查），则结果类型将提升为下一较大的数值类型（如果可能）。 例如，考虑以下代码：

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

它打印以下结果：

```console
System.Int16 = 512
```

如果没有更大的数值类型可用于保存数字，则会引发 `System.OverflowException` 异常。

### <a name="operator-resolution"></a>操作员解析

给定运算符类型和操作数集，运算符解析将确定要用于操作数的运算符。 解析运算符时，将首先考虑用户定义的运算符，并使用以下步骤：

1. 首先，收集所有候选运算符。 候选运算符是源类型中特定运算符类型的所有用户定义的运算符，以及目标类型中特定类型的所有用户定义的运算符。 如果源类型与目标类型相关，则仅将常用运算符视为一次。

2. 然后，将重载决策应用于运算符和操作数，以选择最具体的运算符。 对于二元运算符，这可能会导致后期绑定调用。

当收集类型 @no__t 的候选运算符时，将改用 `T` 类型的运算符。 还会提升仅涉及不可为 null 的值类型的 @no__t 0 的用户定义运算符。 提升运算符使用任意值类型的可以为 null 的版本，但在返回类型为 `IsTrue` 和 `IsFalse` （必须 @no__t 为-2）的情况下除外。 通过将操作数转换为不可为 null 的版本，然后计算用户定义的运算符，然后将结果类型转换为可以为 null 的版本，来计算提升运算符。 如果网操作数为 `Nothing`，则表达式的结果为类型为 @no__t 为结果类型的可以为 null 的值。 例如：

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

如果运算符为二元运算符，并且其中一个操作数为引用类型，则也会提升运算符，但任何绑定到该运算符都将产生错误。 例如：

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

__纪录.__ 此规则存在的原因是我们是否希望在未来版本中添加 null 传播引用类型，在这种情况下，这两个类型之间的二元运算符的行为将会更改。

与转换一样，用户定义的运算符始终优于提升运算符。

解析重载运算符时，Visual Basic 中定义的类与在其他语言中定义的类之间可能存在差异：

* 在其他语言中，`Not`，`And`，`Or` 可以同时作为逻辑运算符和位运算符重载。 从外部程序集导入时，任何一个窗体都接受为这些运算符的有效重载。 但对于定义逻辑运算符和位运算符的类型，只考虑按位实现。

* 在其他语言中，`>>` 和 `<<` 可能会重载为有符号运算符和无符号运算符。 从外部程序集导入时，任何一个窗体都作为有效重载接受。 但对于定义有符号和无符号运算符的类型，将只考虑已签名的实现。

* 如果没有用户定义的运算符是最特定于操作数的，则将考虑内部运算符。 如果没有为操作数定义内部运算符，并且其中一个操作数具有类型 Object，则该运算符将解析后期绑定;否则，将产生编译时错误。

在 Visual Basic 的以前版本中，如果恰好有一个类型为 Object 的操作数，并且没有适用的用户定义运算符，并且没有适用的内部运算符，则是错误的。 从 Visual Basic 11 开始，现在已解析后期绑定。 例如：

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

具有内部运算符的类型 @no__t 也为 `T?` 定义同一运算符。 运算符 @no__t 上的结果与 `T` 相同，不同之处在于，如果其中一个操作数 @no__t 为-2，则运算符的结果将为 `Nothing` （即传播 null 值）。 为解析操作的类型，将从具有 @no__t 的任何操作数中删除-0，确定操作的类型，并且如果任何操作数为可为 null 的值类型，则将 `?` 添加到操作的类型。 例如：

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

每个运算符都列出为其定义的内部类型以及给定操作数类型执行的操作的类型。 内部操作类型的结果遵循以下常规规则：

* 如果所有操作数的类型相同，并且为该类型定义了运算符，则不发生转换并且使用该类型的运算符。

* 使用以下步骤转换其类型未为运算符定义的任何操作数，并使用新类型解析运算符：

  * 操作数将转换为为运算符和操作数定义的最大的最大类型，并将其隐式转换为。

  * 如果没有这样的类型，则操作数将转换为为运算符和操作数定义的下一个最小类型，并将其隐式转换为。

  * 如果没有此类类型或无法进行转换，则会发生编译时错误。

* 否则，操作数将转换为操作数类型的更宽，并使用该类型的运算符。 如果较窄的操作数类型无法隐式转换为更大的运算符类型，则会发生编译时错误。

不过，尽管使用了这些常规规则，但在操作员的结果表中有很多特例。

__纪录.__ 由于格式设置的原因，运算符类型表将预定义的名称缩写为它们的前两个字符。 因此 "By" `Byte`，"UI" `UInteger`，"St" `String`，等等。"Err" 表示没有为给定的操作数类型定义操作。

## <a name="arithmetic-operators"></a>算术运算符

@No__t，`/`，`\`，`^`，`Mod`，`+`，`-` 运算符为*算术运算符*。

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

与运算的结果类型相比，浮点算术运算的精度可能更高。 例如，某些硬件体系结构支持 "扩展" 或 "long double" 浮点类型，其范围和精度高于 @no__t 0 类型，并使用此更高精度类型隐式执行所有浮点运算。 可以采用更少的精度来执行浮点运算，只需在性能方面产生较高的成本;Visual Basic 允许将更高精度的类型用于所有浮点运算，而不是要求实现使的性能和精度。 除了提供更精确的结果，这种情况很少会带来任何实实在在的影响。 但是，在形式 `x * y / z` 的表达式中，乘法产生的结果不在 `Double` 范围内，而后续除法则将临时结果返回到 `Double` 范围内，这一事实是在较高范围的格式可能导致生成有限的结果，而不是无限大。


### <a name="unary-plus-operator"></a>一元加号运算符

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

为 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Single`、`Double` 和 0 类型定义了一元加运算符。

__操作类型：__


| __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | 间距 | Sh | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 


### <a name="unary-minus-operator"></a>一元减号运算符

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

为以下类型定义了一元减号运算符：

`SByte`、 `Short`、 `Integer`和 `Long`。 通过从零中减去操作数来计算结果。 如果启用了整数溢出检查，且操作数的值为最大负值 `SByte`、`Short`、`Integer` 或 `Long`，则会引发 `System.OverflowException` 异常。 否则，如果操作数的值为最大负值 `SByte`，`Short`，`Integer`，或 `Long`，则结果为相同的值，并且不报告溢出。

`Single` 和 `Double`。 结果是操作数的符号反转，其中包括值0和无限大。 如果操作数为 NaN，则结果也为 NaN。

`Decimal`。 通过从零中减去操作数来计算结果。

__操作类型：__

| __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 


### <a name="addition-operator"></a>加法运算符

加法运算符计算两个操作数之和。

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

为以下类型定义了加法运算符：

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。 如果启用了整数溢出检查，且总和超出了结果类型的范围，则会引发 `System.OverflowException` 异常。 否则，不会报告溢出，并且放弃结果的任何高序位。

* `Single` 和 `Double`。 Sum 根据 IEEE 754 算法的规则进行计算。

* `Decimal`。 如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。 如果结果值太小而无法表示为十进制格式，则结果为0。

* `String`。 两个 @no__t 0 个操作数连接在一起。

* `Date`。 @No__t-0 类型定义重载加法运算符。 由于 `System.DateTime` 等效于内部 @no__t 类型，因此，@no__t 类型上也提供了这些运算符。

__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __Bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __由__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __反馈__ |    |    |    |    | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __取消__ |    |    |    |    |    |    |    |    |    | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该 | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该 | Ob | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | 停止  | Err | 停止 | Ob | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | 停止  | 停止 | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 停止 | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


### <a name="subtraction-operator"></a>减法运算符

减法运算符从第一个操作数中减去第二个操作数。

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

减法运算符是为以下类型定义的：

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。 如果启用了整数溢出检查，并且差异超出了结果类型的范围，则会引发 `System.OverflowException` 异常。 否则，不会报告溢出，并且放弃结果的任何高序位。

* `Single` 和 `Double`。 根据 IEEE 754 算法的规则计算差异。

* `Decimal`。 如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。 如果结果值太小而无法表示为十进制格式，则结果为0。

* `Date`。 @No__t-0 类型定义重载减法运算符。 由于 `System.DateTime` 等效于内部 @no__t 类型，因此，@no__t 类型上也提供了这些运算符。

__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __由__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __反馈__ |    |    |    |    | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __取消__ |    |    |    |    |    |    |    |    |    | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="multiplication-operator"></a>乘法运算符

乘法运算符计算两个操作数的乘积。

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

乘法运算符是为以下类型定义的：

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。 如果启用了整数溢出检查，并且产品超出了结果类型的范围，则会引发 `System.OverflowException` 异常。 否则，不会报告溢出，并且放弃结果的任何高序位。

* `Single` 和 `Double`。 根据 IEEE 754 算法的规则计算产品。

* `Decimal`。 如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。 如果结果值太小而无法表示为十进制格式，则结果为0。

__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __由__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __反馈__ |    |    |    |    | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __取消__ |    |    |    |    |    |    |    |    |    | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="division-operators"></a>除法运算符

除法运算符计算两个操作数的商。 有两个除法运算符：常规（浮点）除法运算符和整数除法运算符。

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

为以下类型定义了常规除法运算符：

* `Single` 和 `Double`。 根据 IEEE 754 算法的规则计算商。

* `Decimal`。 如果右操作数的值为零，则会引发 `System.DivideByZeroException` 异常。 如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。 如果结果值太小而无法表示为十进制格式，则结果为零。 在任何舍入之前，结果的小数位数是最接近于首选比例的小数位数，这将使结果等于准确的结果。  首选刻度是第一个操作数的小数位数减去第二个操作数的小数位数。

根据正常的运算符解析规则，仅在类型的操作数（如 `Byte`、`Short`、`Integer` 和 `Long`）之间进行常规分割会导致两个操作数都转换为类型 @no__t。 但是，当两种类型都不 `Decimal` 时，对除法运算符执行运算符解析时，`Double` 被视为比 `Decimal` 窄。 遵循此约定是因为 `Double` 相除比 @no__t 1 个分部更有效。

__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Bo__ | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __由__ |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __反馈__ |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | 应该 | 应该 | 应该 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | 应该 | 应该 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | 应该 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __取消__ |    |    |    |    |    |    |    |    |    | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 

为 `Byte`、`SByte`、`UShort`、`Short`、@no__t、@no__t、@no__t、@no__t 和定义整数除法运算符。 如果右操作数的值为零，则会引发 `System.DivideByZeroException` 异常。 相除将结果舍入到零，结果的绝对值是小于两个操作数的商绝对值的最大整数。 如果两个操作数具有相同的符号，则结果为零或正; 如果两个操作数具有相反的符号，则结果为零或负。 如果左操作数是最大负值 `SByte`，`Short`，`Integer` 或 `Long`，右操作数为 `-1`，则发生溢出;如果启用了整数溢出检查，则会引发 `System.OverflowException` 异常。 否则，不会报告溢出，结果将改为左操作数的值。

__纪录.__ 由于无符号类型的两个操作数始终为零或正数，因此结果始终为零或正数。  由于表达式的结果将始终小于或等于两个操作数中的最大值，因此不可能发生溢出。  不会对具有两个无符号整数的整数除法执行此类整数溢出检查。 结果就是左操作数的类型。


__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __由__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __反馈__ |    |    |    |    | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __取消__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="mod-operator"></a>Mod 运算符

@No__t-0 （取模）运算符计算两个操作数之间的相除余数。

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

为以下类型定义了 `Mod` 运算符：

* `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`。 @No__t 的结果为 `x - (x \ y) * y` 生成的值。 如果 @no__t 为零，则将引发 `System.DivideByZeroException` 异常。 取模运算符从不导致溢出。

* `Single` 和 `Double`。 根据 IEEE 754 算法的规则计算余数。

* `Decimal`。 如果右操作数的值为零，则会引发 `System.DivideByZeroException` 异常。 如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。 如果结果值太小而无法表示为十进制格式，则结果为零。

__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Bo__ | Sh | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __由__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __反馈__ |    |    |    |    | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __取消__ |    |    |    |    |    |    |    |    |    | 取消 | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="exponentiation-operator"></a>求幂运算符

幂运算符计算第一个操作数的第二个操作数的幂。

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

为类型 `Double` 定义幂运算符。 根据 IEEE 754 算法的规则计算该值。

__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Bo__ | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __SB__ |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __由__ |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __Sh__ |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __反馈__ |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __In__ |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __UI__ |    |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | 应该 | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __取消__ |    |    |    |    |    |    |    |    |    | 应该 | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | 应该 | 应该 | Err | Err | 应该  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该  | Ob  | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 应该  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="relational-operators"></a>关系运算符

*关系运算符*将值相互比较。 比较运算符 `=`、`<>`、`<`、`>`、`<=` 和 @no__t。

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

所有关系运算符都产生 @no__t 值0。

关系运算符具有以下一般含义：

* @No__t-0 运算符测试两个操作数是否相等。

* @No__t-0 运算符测试两个操作数是否不相等。

* @No__t-0 运算符测试第一个操作数是否小于第二个操作数。

* @No__t-0 运算符测试第一个操作数是否大于第二个操作数。

* @No__t-0 运算符测试第一个操作数是否小于或等于第二个操作数。

* @No__t-0 运算符测试第一个操作数是否大于或等于第二个操作数。

关系运算符是为以下类型定义的：

* `Boolean`。 运算符比较两个操作数的真假值。 `True` 被视为小于 `False`，它与其数值匹配。

* `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。 运算符比较两个整数操作数的数值。

* `Single` 和 `Double`。 运算符根据 IEEE 754 标准的规则对操作数进行比较。

* `Decimal`。 运算符比较两个 decimal 操作数的数值。

* `Date`。 运算符返回比较两个日期/时间值的结果。

* `Char`。 运算符返回比较两个 Unicode 值的结果。

* `String`。 运算符返回使用二进制比较或文本比较来比较两个值的结果。 所使用的比较是由编译环境和 `Option Compare` 语句确定的。 二进制比较确定每个字符串中每个字符的数值 Unicode 值是否相同。 文本比较基于 .NET Framework 上使用的当前区域性执行 Unicode 文本比较。 在执行字符串比较时，null 值等效于字符串文本 `""`。

__操作类型：__
        
|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| __Bo__ | Bo | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | Bo | Ob | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __由__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __反馈__ |    |    |    |    | US | 内 | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __Lo__ |    |    |    |    |    |    |    | Lo | 取消 | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | UL | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __取消__ |    |    |    |    |    |    |    |    |    | 取消 | Si | 应该 | Err | Err | 应该 | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Si | 应该 | Err | Err | 应该 | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 应该 | Err | Err | 应该 | Ob | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | 特里斯坦  | Err | 特里斯坦 | Ob | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | 48  | 停止 | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | 停止 | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | Ob | 


## <a name="like-operator"></a>Like 运算符

@No__t-0 运算符确定字符串是否与给定的模式匹配。

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

为 `String` 类型定义 `Like` 运算符。 第一个操作数是要匹配的字符串，第二个操作数是要匹配的模式。 模式由 Unicode 字符组成。 以下字符序列具有特殊含义：

* 字符 `?` 匹配任何单个字符。

* 字符 @no__t 与零个或多个字符匹配。

* 字符 `#` 匹配任何单个数字（0-9）。

* 由方括号（`[ab...]`）括起来的字符列表与列表中的任何单个字符匹配。

* 由方括号括起来并以感叹号（`[!ab...]`）作为前缀的字符列表匹配字符列表中不存在的任何单个字符。

* 由连字符分隔的字符列表中的两个字符（`-`）指定从第一个字符开始、以第二个字符结尾的 Unicode 字符的范围。 如果第二个字符的排序顺序不是第一个字符，则会出现运行时异常。 出现在字符列表开头或结尾的连字符指定自身。

若要匹配特殊字符左方括号（`[`）、问号（`?`）、数字符号（`#`）和星号（`*`），括号必须将它们括起来。 不能在组内使用右括号（`]`）来匹配自身，但它可以作为单个字符在组外使用。 字符序列 @no__t 被视为字符串文本 @no__t 为-1。 

请注意，字符列表的字符比较和排序取决于所使用的比较类型。 如果使用二进制比较，则字符比较和排序基于数值 Unicode 值。 如果使用文本比较，则字符比较和排序基于 .NET Framework 上使用的当前区域设置。

在某些语言中，字母表中的特殊字符表示两个单独的字符，反之亦然。 例如，几种语言使用字符 `æ` 来表示字符 `a`，并在它们一起显示时 `e`，而字符 `^` 和 `O` 可用于表示字符 `Ô`。 使用文本比较时，`Like` 运算符可识别此类等效性。 在这种情况下，在模式或字符串中出现的单个特殊字符与另一个字符串中等效的双字符序列匹配。 同样，以方括号括起来的模式中的单个特殊字符（单独、列表或范围中）与字符串中等效的双字符序列匹配，反之亦然。

在 @no__t 0 表达式中，如果这两个操作数都 `Nothing` 或一个操作数具有到 `String` 的内部转换，而另一个操作数为 `Nothing`，则 `Nothing` 被视为空字符串文本 `""`。

__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __Bo__ | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __SB__ |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __由__ |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __Sh__ |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __反馈__ |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __In__ |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __UI__ |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __Lo__ |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __取消__ |    |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | Ob | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | Ob | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |    | 停止 | 停止 | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | 停止 | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="concatenation-operator"></a>串联运算符

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

*串联运算符*是为所有内部类型定义的，包括内部值类型的可以为 null 的版本。 它还定义为在上面提到的类型与 `System.DBNull` 之间的串联，该类型被视为 @no__t 字符串。 串联运算符将其所有操作数转换为 `String`;在表达式中，无论是否使用严格语义，都将所有转换为 `String` 的转换都视为扩大。 @No__t 值转换为类型为 `String` 的文本 @no__t。 值为 `Nothing` 的可以为 null 的值类型也转换为类型化为 `String` 的文本 @no__t，而不是引发运行时错误。

串联运算将生成一个字符串，该字符串是从左至右按顺序连接两个操作数。 值 `Nothing` 被视为空字符串文本 `""`。

__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| __Bo__ | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __SB__ |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __由__ |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __Sh__ |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __反馈__ |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __In__ |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __UI__ |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __Lo__ |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __UL__ |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __取消__ |    |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | 停止 | Ob | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | 停止 | Ob | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | 停止 | 停止 | 停止 | Ob | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |    | 停止 | 停止 | Ob | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    | 停止 | Ob | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Ob | 


## <a name="logical-operators"></a>逻辑运算符

@No__t，`Not`、`Or` 和 @no__t 3 运算符称为逻辑运算符。

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

逻辑运算符按如下方式计算：

* 对于 `Boolean` 类型：

  * 对两个操作数执行逻辑 @no__t 运算。

  * 逻辑 `Not` 操作在其操作数上执行。

  * 对两个操作数执行逻辑 @no__t 运算。

  * 对两个操作数执行逻辑异 @no__t 运算。

* 对于 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，以及所有枚举类型，将对两个操作数的二进制表示形式的每个位执行指定的操作：

  * `And`：如果两个位均为1，则结果位为 1;否则，结果位为0。

  * `Not`：如果位为0，则结果位为 1;否则，结果位为1。

  * `Or`：如果任一位为1，则结果位为 1;否则，结果位为0。

  * `Xor`：如果任一位为1，但不是两个位，则结果位为 1;否则，结果位为0（即 1 `Xor` 0 = 1，1 `Xor` 1 = 0）。

* 如果逻辑运算符 `And`，并且为类型 @no__t 提升 `Or`，则将其扩展为包含三值布尔逻辑，如下所示：

  * 如果两个操作数都为 true，则 `And` 的计算结果为 true;如果其中一个操作数为 false，则为 false;否则 `Nothing`。

  * 如果任意一个操作数为 true，则 `Or` 的计算结果为 true;false 是两个操作数均为 false;否则 `Nothing`。

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

__纪录.__ 理想情况下，将使用三值逻辑对逻辑运算符 `And` 和 `Or`，可以在布尔表达式中使用的任何类型（即实现 `IsTrue` 和 `IsFalse` 的类型）使用三值逻辑，其 @no__t 方式与在任何可在布尔表达式中使用的类型。 遗憾的是，三值提升仅适用于 `Boolean?`，因此需要三值逻辑的用户定义的类型必须通过为其可为 null 的版本定义 @no__t 和 `Or` 运算符来手动完成此操作。

这些操作不能有溢出。 枚举类型运算符对枚举类型的基础类型执行按位运算，但返回值为枚举类型。

__不是操作类型：__

| __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Bo | SB | 间距 | Sh | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 

__And、Or、Xor 运算类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Bo__ | Bo | SB | Sh | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Bo  | Ob  | 
| __SB__ |    | SB | Sh | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __由__ |    |    | 间距 | Sh | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Sh__ |    |    |    | Sh | 内 | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __反馈__ |    |    |    |    | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __In__ |    |    |    |    |    | 内 | Lo | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UI__ |    |    |    |    |    |    | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Lo | Lo | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | UL | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __取消__ |    |    |    |    |    |    |    |    |    | Lo | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Lo | Lo | Err | Err | Lo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Lo | Err | Err | Lo  | Ob  | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Lo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


### <a name="short-circuiting-logical-operators"></a>短路逻辑运算符

@No__t 0 和 @no__t 1 运算符是 `And` 和 @no__t 逻辑运算符的短路版本。

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

由于其短路行为，如果在计算第一个操作数后知道运算符结果，则不会在运行时计算第二个操作数。

短路逻辑运算符按如下方式计算：

* 如果 @no__t 运算中的第一个操作数的计算结果为 `False`，或从其 @no__t 2 运算符返回 True，则该表达式将返回其第一个操作数。 否则，将计算第二个操作数，并对两个结果执行逻辑 `And` 操作。

* 如果 @no__t 运算中的第一个操作数的计算结果为 `True`，或从其 @no__t 2 运算符返回 True，则该表达式将返回其第一个操作数。 否则，将计算第二个操作数，并对其两个结果执行逻辑 `Or` 运算。

为类型 `Boolean` 或为任何重载以下运算符的类型  定义 `AndAlso` 和 @no__t。

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

以及重载相应的 `And` 或 `Or` 运算符：

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

计算 `AndAlso` 或 `OrElse` 运算符时，第一个操作数只计算一次，第二个操作数不计算或只计算一次。 例如，考虑以下代码：

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

它打印以下结果：

```console
And: False True
Or: True False
AndAlso: False
OrElse: True
```

在 @no__t @no__t 的提升形式中，如果第一个操作数为 null `Boolean?`，则计算第二个操作数，但结果始终为 null `Boolean?`。

__操作类型：__

|        | __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| __Bo__ | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __SB__ |    | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __由__ |    |    | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __Sh__ |    |    |    | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __反馈__ |    |    |    |    | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __In__ |    |    |    |    |    | Bo | Bo | Bo | Bo | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __UI__ |    |    |    |    |    |    | Bo | Bo | Bo | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __Lo__ |    |    |    |    |    |    |    | Bo | Bo | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __UL__ |    |    |    |    |    |    |    |    | Bo | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __取消__ |    |    |    |    |    |    |    |    |    | Bo | Bo | Bo | Err | Err | Bo  | Ob  | 
| __Si__ |    |    |    |    |    |    |    |    |    |    | Bo | Bo | Err | Err | Bo  | Ob  | 
| __Do__ |    |    |    |    |    |    |    |    |    |    |    | Bo | Err | Err | Bo  | Ob  | 
| __特里斯坦__ |    |    |    |    |    |    |    |    |    |    |    |    | Err | Err | Err | Err | 
| __48__ |    |    |    |    |    |    |    |    |    |    |    |    |     | Err | Err | Err | 
| __St__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     | Bo  | Ob  | 
| __Ob__ |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | Ob  | 


## <a name="shift-operators"></a>移位运算符

二元运算符 `<<` 和 @no__t 执行移位运算。

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

为 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`ULong` 和 @no__t 7 类型定义运算符。 与其他二元运算符不同，移位运算的结果类型被确定为：运算符是只带有左操作数的一元运算符。 右操作数的类型必须可隐式转换为 `Integer`，并且不用于确定运算的结果类型。

@No__t-0 运算符导致第一个操作数中的位左移到移位量指定的位数。 结果类型范围外的高序位将被丢弃，且低顺序空出的位位置为零填充。

@No__t 0 运算符导致第一个操作数中的位向右移位由移位量指定的位数。 如果左操作数为正值或为负，则将丢弃低序位，并将高顺序空出的位位置设置为零。 如果左操作数的类型为 `Byte`，`UShort`，`UInteger` @no__t，则空的高序位为零。

移位运算符按第二个操作数的量位移第一个操作数的基础表示形式的位数。 如果第二个操作数的值大于第一个操作数中的位数或为负，则移位量计算为 `RightOperand And SizeMask`，其中 `SizeMask` 为：

| __LeftOperand 类型__  | __SizeMask__ | 
|-----------------------|--------------|
| `Byte`， `SByte`       | 7 (`&H7`)    | 
| `UShort`， `Short`     | 15 (`&HF`)   | 
| `UInteger`， `Integer` | 31 (`&H1F`)  | 
| `ULong`， `Long`       | 63 (`&H3F`)  | 

如果移位量为零，则操作的结果与第一个操作数的值相同。 这些操作不能有溢出。

__操作类型：__


| __Bo__ | __SB__ | __由__ | __Sh__ | __反馈__ | __In__ | __UI__ | __Lo__ | __UL__ | __取消__ | __Si__ | __Do__ | __特里斯坦__  | __48__  | __St__ | __Ob__ | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| Sh | SB | 间距 | Sh | US | 内 | UI | Lo | UL | Lo | Lo | Lo | Err | Err | Lo | Ob | 


## <a name="boolean-expressions"></a>布尔表达式

布尔表达式是一个表达式，可对其进行测试，以查看其是否为 true 或 false。

```antlr
BooleanExpression
    : Expression
    ;
```

如果按优先顺序排序，则可以在布尔表达式中使用类型 `T`：

* `T` @no__t 为-1 或 `Boolean?`

* `T` 具有到 @no__t 的扩大转换

* `T` 具有到 @no__t 的扩大转换

* @no__t 定义了两个伪运算符，`IsTrue` 和 `IsFalse`。

* `T` 会将收缩转换为 `Boolean?`，而不涉及从 `Boolean` 到 `Boolean?` 的转换。

* `T` 有一个到 @no__t 的收缩转换。

__纪录.__ 需要注意的是，如果 `Option Strict` 处于关闭状态，则将在没有编译时错误的情况下接受收缩转换到 `Boolean` 的表达式，但如果存在，则该语言仍将使用 @no__t 2 运算符。 这是因为 `Option Strict` 只更改语言所接受的内容，而不会更改表达式的实际含义。 因此，无论 @no__t @no__t，都必须始终优先于收缩转换。

例如，下面的类不定义要 `Boolean` 的扩大转换。 因此，它在 `If` 语句中的使用将导致调用 @no__t 1 运算符。

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

如果布尔表达式类型化为或转换为 `Boolean` 或 `Boolean?`，则如果值为 `True`，则为 true; 否则为 false。

否则，如果运算符返回 `True`，则布尔表达式将调用 `IsTrue` 运算符并返回 `True`;否则为 false （但绝不会调用 `IsFalse` 运算符）。

在下面的示例中，`Integer` 会将收缩转换为 `Boolean`，因此 null `Integer?` 会将收缩转换为 @no__t 3 （生成 null `Boolean`）以及 `Boolean` （引发异常）。 优先级为 `Boolean?` 的收缩转换为布尔表达式，因此 "`i`" 的值 @no__t 为-2。

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a>Lambda 表达式

*Lambda 表达式*定义名为*lambda 方法*的匿名方法。 使用 Lambda 方法可以轻松地将 "内联" 方法传递到采用委托类型的其他方法。

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

示例：

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

将打印输出：

```console
2 4 6 8
```

Lambda 表达式以可选修饰符开头 `Async` 或 `Iterator`，后跟关键字 `Function` 或 `Sub` 和参数列表。 Lambda 表达式中的参数不能声明为 `Optional` 或 `ParamArray`，且不能具有特性。 不同于常规方法，省略 lambda 方法的参数类型不会自动推断 `Object`。 相反，在对 lambda 方法重新分类后，会从目标类型中推断出省略的参数类型和 `ByRef` 修饰符。 在上面的示例中，lambda 表达式可以编写为 `Function(x) x * 2`，并且在使用 lambda 方法创建 `IntFunc` 委托类型的实例时，它将推断出 @no__t 为-2 @no__t 的类型。 与局部变量推理不同，如果 lambda 方法参数省略了类型但具有数组或可为 null 的名称修饰符，则会发生编译时错误。

__常规 lambda 表达式__是不 `Async` 和 `Iterator` 修饰符之一。

__迭代器 lambda 表达式__是一个具有 `Iterator` 修饰符并且没有 @no__t 的修饰符。 它必须是函数。 如果将它重新分类为某个值，则它只能重新分类为其返回类型为 `IEnumerator` 的委托类型的值，或 @no__t 为-1，@no__t 或者对于某些 @no__t，则不会重新分类为 `IEnumerable(Of T)`，并且没有 ByRef 参数。

__异步 lambda 表达式__是一个具有 `Async` 修饰符并且没有 @no__t 的修饰符。 Async sub lambda 只能重新分类为不带 ByRef 参数的子委托类型的值。 异步函数 lambda 只能重新分类为函数委托类型的值，该函数的返回类型为 `Task` 或 @no__t 为（对于某些 @no__t 为-2），但没有 ByRef 参数。

Lambda 表达式可以是单行或多行。 单行 `Function` lambda 表达式包含一个表达式，该表达式表示从 lambda 方法返回的值。 单行 `Sub` lambda 表达式包含单个语句，不包含其结束 `StatementTerminator`。 例如：

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

单行 lambda 构造与所有其他表达式和语句紧密绑定。 例如，"`Function() x + 5`" 等效于 "`Function() (x+5)"` 而不是" `(Function() x) + 5` "。 为了避免多义性，单行 `Sub` lambda 表达式不能包含 Dim 语句或标签声明语句。 此外，除非它括在括号中，否则单行 `Sub` lambda 表达式后面不能跟冒号 "："、成员访问运算符 "."、字典成员访问运算符 "！" 或左括号 "（"。 它不能包含任何 block 语句（@no__t 0，`SyncLock, If...EndIf`，`While`，`For`，`Do`，`Using`），也不能包含 `OnError`。

__纪录.__ 在 lambda 表达式 `Function(i) x=i` 中，主体被解释为*表达式*（测试 @no__t 2 和 `i` 是否相等）。 但在 lambda 表达式中 `Sub(i) x=i`，主体将被解释为语句（将 @no__t 分配到 `x`）。

多行 lambda 表达式包含语句块，必须以适当的 `End` 语句（即 `End Function` 或 `End Sub`）结束。 与常规方法一样，多行 lambda 方法的 `Function` 或 @no__t 语句和 `End` 语句必须在其自己的行上。 例如：

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

多行 `Function` lambda 表达式可以声明返回类型，但不能在其上放置属性。 如果多行 `Function` lambda 表达式未声明返回类型，但可以从使用 lambda 表达式的上下文推断返回类型，则使用该返回类型。 否则，函数的返回类型计算如下：

* 在常规 lambda 表达式中，返回类型是语句块中所有 `Return` 语句中表达式的主导类型。

* 在异步 lambda 表达式中，返回类型为 `Task(Of T)`，其中 `T` 是语句块中所有 `Return` 语句中的表达式的主导类型。

* 在迭代器 lambda 表达式中，返回类型为 `IEnumerable(Of T)`，其中 `T` 是语句块中所有 `Yield` 语句中的表达式的主导类型。

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

在所有情况下，如果没有 `Return` （分别 `Yield`）语句，或者它们之间没有主导类型并且使用了严格语义，则会发生编译时错误;否则，主导类型将隐式 `Object`。

请注意，从所有 `Return` 语句计算返回类型，即使它们不可访问也是如此。 例如：

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

没有隐式返回变量，因为变量没有名称。

多行 lambda 表达式中的语句块具有以下限制：

* 尽管允许 `Try` 语句，但不允许 `On Error` 和 @no__t 语句。

* 不能在多行 lambda 表达式中声明静态局部变量。

* 尽管在多行 lambda 表达式的语句块中应用正常的分支规则，但不能对其进行分支或跳出。 例如：

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

Lambda 表达式大致等效于对包含类型声明的匿名方法。 初始示例大致等效于：

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

Lambda 表达式可以访问范围内的所有变量，包括在包含方法和 lambda 表达式中定义的局部变量或参数。 当 lambda 表达式引用局部变量或参数时，lambda 表达式捕获引用为闭包的变量。 闭包是位于堆而不是堆栈上的对象，并且在捕获变量时，对该变量的所有引用都将重定向到闭包。 这样，即使在包含方法完成后，lambda 表达式也能继续引用局部变量和参数。 例如：

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

大致等效于：

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

闭包每次进入局部变量时都会捕获本地变量的新副本，但会使用上一个副本（如果有）的值初始化新副本。 例如：

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

显

```console
1 2 3 4 5 6 7 8 9 10
```

而不是

```console
9 9 9 9 9 9 9 9 9 9
```

因为闭包在输入块时必须进行初始化，因此不允许将从该块外 `GoTo` @no__t 为闭包，但允许在闭包中将-1 为块。 例如：

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

由于无法将它们捕获到闭包中，因此不能在 lambda 表达式中显示以下内容：

* 引用参数。

* 实例表达式（`Me`，`MyClass`，`MyBase`）（如果 @no__t 的类型不是类）。

如果 lambda 表达式是表达式的一部分，则为匿名类型创建表达式的成员。 例如：

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

实例构造函数中的 @no__t 0 实例变量，或共享构造函数中的 @no__t 1 共享变量，这些变量在非值上下文中使用。 例如：

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

*查询表达式*是将一系列*查询运算符*应用于可*查询*集合的元素的表达式。 例如，下面的表达式将获取 `Customer` 对象的集合，并返回位于华盛顿州的所有客户的姓名：

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

查询表达式必须以 @no__t 0 或 `Aggregate` 运算符开头，并可以任何查询运算符结尾。 查询表达式的结果归类为一个值;表达式的结果类型取决于表达式中最后一个查询运算符的结果类型。

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

某些查询运算符引入称为*范围变量*的特殊类型的变量。 范围变量不是实际变量;相反，它们表示在对输入集合进行查询的评估过程中的各个值。

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

范围变量的范围是从引入查询运算符到查询表达式的末尾，或者是对查询运算符（例如 `Select` 的隐藏它们）。 例如，在下面的查询中

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

@no__t 0 查询运算符引入了一个类型为 `Customer` 的范围变量，该 @no__t 变量表示 @no__t 集合中的每个客户。 下面的 @no__t 0 查询运算符引用筛选器表达式中的范围变量 `cust`，以确定是否要将单个客户从结果集合中进行筛选。

有两种类型的范围变量：*集合范围*变量和*表达式范围变量*。 集合范围变量从要查询的集合的元素中获取其值。 集合范围变量声明中的集合表达式必须分类为其类型可查询的值。 如果省略集合范围变量的类型，则会将其推断为集合表达式的元素类型，如果集合表达式不具有元素类型（即仅定义 @no__t 1 方法），则将 @no__t 为0。 如果集合表达式不可查询（即无法推断集合的元素类型），将产生编译时错误。

表达式范围变量是一个范围变量，其值由表达式而不是集合计算。 在下面的示例中，@no__t 0 查询运算符引入了一个名为 `cityState` 的表达式范围变量，该变量是从两个字段计算得出的：

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

不需要使用表达式范围变量来引用另一个范围变量，尽管此类变量可能是可疑值。 分配给表达式范围变量的表达式必须归类为值，并且必须能够隐式转换为范围变量的类型（如果给定）。

仅在 Let 运算符中可以指定表达式范围变量的类型。 在其他运算符中，或者，如果未指定其类型，则使用局部变量类型推理来确定范围变量的类型。

范围变量必须遵循用于声明隐藏的局部变量的规则。 因此，范围变量不能在封闭方法或其他范围变量中隐藏局部变量或参数的名称（除非查询运算符具体地隐藏作用域中的所有当前范围变量）。


### <a name="queryable-types"></a>可查询类型

查询表达式是通过将表达式转换为对集合类型的已知方法的调用来实现的。 这些定义完善的方法定义可查询集合的元素类型以及对集合执行的查询运算符的结果类型。 每个查询运算符都指定查询运算符通常转换为的方法，但特定翻译是依赖实现的。 这些方法是使用常规格式（如下所示）在规范中指定的：

```vb
Function Select(selector As Func(Of T, R)) As CR
```

以下各项适用于方法：

* 方法必须是集合类型的实例或扩展成员，并且必须是可访问的。

* 如果可以推断所有类型参数，则该方法可能是泛型的。

* 此方法可能会重载，在这种情况下，重载决策用于确定要使用的确切方法。

* 可以使用另一个委托类型来代替委托 `Func` 类型，前提是它具有与匹配的 `Func` 类型相同的签名（包括返回类型）。

* 如果 `D` 是具有相同签名的委托类型（包括返回类型），则可以 @no__t 使用类型 `System.Linq.Expressions.Expression(Of D)` 作为匹配的 `Func` 类型。

* 类型 `T` 表示输入集合的元素类型。 集合类型定义的所有方法都必须具有相同的输入元素类型，以便可以查询该集合类型。

* 如果执行联接的查询运算符，则类型 `S` 表示第二个输入集合的元素类型。

* 如果查询运算符包含一组充当键的范围变量，则类型 `K` 表示密钥类型。

* 类型 `N` 表示用作数值类型的类型（尽管它仍可以是用户定义的类型，而不是内部数值类型）。

* 类型 `B` 表示可在布尔表达式中使用的类型。

* 如果查询运算符生成结果集合，则类型 `R` 表示结果集合的元素类型。 `R` 取决于查询运算符结束时范围中的范围变量的数目。 如果单个范围变量在范围内，则 @no__t 为该范围变量的类型。 示例中

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  查询的结果将为元素类型为 `String` 的集合类型。 如果多个范围变量位于范围内，则 @no__t 为包含范围中所有范围变量（@no__t 字段）的匿名类型。 在下面的示例中：

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  查询的结果将是具有匿名类型的元素类型的集合类型，其类型为 `Name` 的只读属性，类型为 `String`，名为 `ProductName` 的只读属性的类型为 `String`。

  在查询表达式中，生成为包含范围变量的匿名类型是*透明*的，这意味着范围变量始终可用，无需进行限定。 例如，在前面的示例中，即使输入集合的元素类型是匿名类型，也可以在无需 `Select` 查询运算符的情况下访问 `c` 和 `o` 的范围变量。

* 类型 `CX` 表示集合类型，不一定是输入集合类型，其元素类型为 `X` 类型。

可查询的集合类型必须满足以下条件之一（按优先顺序）：

* 它必须定义一致的 @no__t 0 方法。

* 它必须具有以下方法之一

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  这可以调用来获取可查询集合。 如果同时提供这两种方法，`AsQueryable` 优先于 @no__t。

* 它必须具有方法

  ```vb
  Function Cast(Of T)() As CT
  ```

  可以使用范围变量的类型调用，以生成可查询的集合。

因为确定集合的元素类型与实际的方法调用无关，所以不能确定特定方法的适用性。 因此，在确定集合的元素类型时，如果存在与已知方法匹配的实例方法，则将忽略任何与已知方法匹配的扩展方法。

查询运算符转换按照查询运算符在表达式中出现的顺序进行。 集合对象不需要实现所有查询运算符所需的所有方法，尽管每个集合对象至少必须支持 @no__t 0 查询运算符。 如果所需的方法不存在，则会发生编译时错误。 绑定已知方法名称时，尽管隐藏语义仍适用，但会忽略非方法，以便在接口和扩展方法绑定中进行多重继承。 例如：

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

元素类型为 @no__t 为0但还没有默认属性的每个可查询集合类型都被视为具有以下常规形式的默认属性：

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

默认属性只能用默认属性访问语法来引用;默认属性不能按名称引用。 例如：

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

如果集合类型没有 @no__t 0 成员，则会发生编译时错误。

### <a name="from-query-operator"></a>From 查询运算符

@No__t 0 查询运算符引入了一个集合范围变量，该变量表示要查询的集合的各个成员。

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

例如，查询表达式：

```vb
From c As Customer In Customers ...
```

可以视为等效于

```vb
For Each c As Customer In Customers
        ...
Next c
```

如果 @no__t 0 查询运算符声明多个集合范围变量，或者不是查询表达式中的第一个 @no__t 查询运算符，则每个新的集合范围变量将*交叉联接*到现有的范围变量集。 结果就是对联接集合中所有元素的叉积计算查询。 例如，表达式：

```vb
From c In Customers _
From e In Employees _
...
```

可以视为等效于：

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

和完全等效于：

```vb
From c In Customers, e In Employees ...
```

在前面的查询运算符中引入的范围变量可用于更高 `From` 查询运算符。 例如，在下面的查询表达式中，第二个 @no__t 0 查询运算符引用第一个范围变量的值：

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

仅当集合类型包含以下一种或两种方法时，才支持 @no__t 0 查询运算符或多个 @no__t 查询运算符中的多个范围变量：

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

通常转换为

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

__纪录.__ `From` 不是保留字。


### <a name="join-query-operator"></a>Join 查询运算符

@No__t 0 查询运算符使用新的集合范围变量联接现有范围变量，并生成一个集合，该集合的元素基于相等表达式联接在一起。

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

相等表达式比正则相等表达式的限制更多：

* 这两个表达式都必须分类为值。

* 两个表达式必须引用至少一个范围变量。

* 联接查询运算符中声明的范围变量必须由其中一个表达式引用，并且该表达式不得引用任何其他范围变量。

如果两个表达式的类型不是完全相同的类型，则

* 如果为这两个类型定义了相等运算符，两个表达式都可以隐式转换为它，并且不 `Object`，然后将两个表达式都转换为该类型。

* 否则，如果存在可隐式转换为两个表达式的主导类型，请将这两个表达式都转换为该类型。

* 否则，将发生编译时错误。

使用哈希值（即通过调用 `GetHashCode()`）而不是使用相等运算符来比较表达式以提高效率。 @No__t 为0的查询运算符可以在同一运算符中执行多个联接或相等性条件。 仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

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

通常转换为

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

__纪录.__ `Join`，`On`，`Equals` 不是保留字。


### <a name="let-query-operator"></a>Let Query 运算符

@No__t 0 查询运算符引入了一个表达式范围变量。 这允许计算一次中间值，该值将在以后的查询运算符中多次使用。

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

可以视为等效于：

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

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

通常转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a>选择查询运算符

@No__t 查询运算符类似于 `Let` 查询运算符，因为它引入了表达式范围变量;但是，@no__t 2 查询运算符将隐藏当前可用的范围变量，而不是将其添加到其中。 此外，始终使用局部变量类型推理规则来推断 @no__t 0 查询运算符引入的表达式范围变量的类型;无法指定显式类型，如果不能推断任何类型，则会发生编译时错误。

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

@no__t 0 查询运算符仅有权访问 `Select` 运算符引入的 @no__t 1 范围变量;如果 @no__t 3 运算符尝试引用 `cust`，则会发生编译时错误。

不是显式指定范围变量的名称，@no__t 0 查询运算符可使用与匿名类型对象创建表达式相同的规则来推断范围变量的名称。 例如：

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

如果未提供范围变量的名称并且无法推断名称，则会发生编译时错误。 如果 @no__t 0 查询运算符只包含一个表达式，则不会发生错误，因为不能推断该范围变量的名称，而是不需要的范围变量。  例如：

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

如果在将名称分配给范围变量和相等表达式之间的 @no__t 0 查询运算符中存在多义性，则首选名称赋值。 例如：

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

@No__t 查询运算符中的每个表达式都必须分类为值。 仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

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

通常转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a>Distinct 查询运算符

@No__t 0 查询运算符仅会将集合中的值限制为具有不同值的值，这是通过比较元素类型是否相等来确定的。

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

例如，查询：

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

对于客户名称和订单价格的每个不同配对，将只返回一行，即使客户具有具有相同价格的多个订单也是如此。 仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

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

通常转换为

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

__纪录.__ `Distinct` 不是保留字。


### <a name="where-query-operator"></a>Where 查询运算符

@No__t 0 查询运算符将集合中的值限制为满足给定条件的值。

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

@No__t 0 查询运算符采用为每组范围变量值计算的布尔表达式;如果表达式的值为 true，则值将显示在输出集合中，否则将跳过这些值。 例如，查询表达式：

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

可以视为等效于嵌套循环

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

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

通常转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

__纪录.__ `Where` 不是保留字。


### <a name="partition-query-operators"></a>分区查询运算符

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

@No__t 0 查询运算符将生成集合的第一个 @no__t 元素。 与 `While` 修饰符一起使用时，`Take` 运算符会生成满足布尔表达式的集合的第一个 `n` 元素。 @No__t-0 运算符将跳过集合中第一个 @no__t 元素，然后返回集合的其余部分。  与 `While` 修饰符一起使用时，`Skip` 运算符将跳过集合中满足布尔表达式的第一个 `n` 元素，然后返回该集合的其余部分。 @No__t 0 或 @no__t 查询运算符中的表达式必须分类为值。

仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

```vb
Function Take(count As N) As CT
```

仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

```vb
Function Skip(count As N) As CT
```

仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

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

通常转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

__纪录.__ `Take` 和 @no__t 不是保留字。


### <a name="order-by-query-operator"></a>Order By 查询运算符

@No__t 查询运算符对范围变量中显示的值进行排序。 

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

@No__t 0 查询运算符使用指定应用于对迭代变量进行排序的键值的表达式。 例如，以下查询将返回按价格排序的产品：

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

排序可以标记为 `Ascending`，在这种情况下，较小的值早于较大的值，或者 `Descending`，在这种情况下，较大的值早于较小的值。 如果指定了 "无"，则排序的默认值为 `Ascending`。 例如，下面的查询返回按价格排序的产品，其最昂贵的产品首先：

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

@No__t 0 查询运算符可指定多个表达式进行排序，在这种情况下，将以嵌套方式对集合进行排序。 例如，以下查询按州对客户进行排序，然后按每个州的城市，然后按每个城市中的邮政编码排序：

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

@No__t 0 查询运算符中的表达式必须分类为值。 仅当集合类型包含以下一个或两个方法时，才支持 @no__t 0 查询运算符：

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

@No__t 的返回类型必须是*有序集合*。 有序集合是包含一个或两个方法的集合类型：

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

通常转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

__纪录.__ 由于查询运算符只是将语法映射到实现特定查询操作的方法，因此，排序不由语言决定，并且由运算符本身的实现确定。 这与用户定义的运算符非常相似，因为实现为用户定义的数值类型重载加法运算符可能不会执行任何类似于添加的操作。 当然，为保持可预测性，不建议实现不符合用户期望的内容。

__纪录.__ `Order` 和 @no__t 不是保留字。


### <a name="group-by-query-operator"></a>按查询运算符分组

@No__t 0 查询运算符根据一个或多个表达式对作用域中的范围变量进行分组，然后基于这些分组生成新的范围变量。

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

例如，下面的查询通过 `State` 对所有客户进行分组，然后计算每个组的计数和平均期限：

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

@No__t 0 查询运算符包含三个子句：可选的 `Group` 子句、`By` 子句和 `Into` 子句。 @No__t-0 子句与 @no__t 查询运算符具有相同的语法和效果，不同之处在于，它仅影响 `Into` 子句中可用的范围变量，而不影响 `By` 子句中的范围变量。 例如：

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

@No__t-0 子句声明用作分组操作中的键值的表达式范围变量。 @No__t-0 子句允许声明表达式范围变量，这些变量计算由 `By` 子句构成的每个组的聚合。 在 `Into` 子句内，只能为表达式范围变量指定一个表达式，该表达式是*聚合函数*的方法调用。 聚合函数是组的集合类型的函数（可能不一定是原始集合的相同集合类型），其外观类似于以下方法之一：

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

如果聚合函数采用委托参数，则调用表达式可以具有必须归类为值的参数表达式。  参数表达式可以使用范围内的范围变量;在对聚合函数的调用中，这些范围变量表示组中组成的值，而不是集合中的所有值。 例如，在本部分的原始示例中，`Average` 函数计算客户每个州的年龄，而不是所有客户的平均值。

所有集合类型都视为聚合函数在其上定义 `Group`，这不采用任何参数，只是返回组。 集合类型可以提供的其他标准聚合函数包括：

`Count` 和 `LongCount`，它返回组中的元素计数或满足布尔表达式的组中元素的计数。 仅当集合类型包含其中一种方法时，才支持 `Count` 和 `LongCount`：

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

`Sum`，它返回组中所有元素的表达式之和。 仅当集合类型包含其中一种方法时，才支持 `Sum`：

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

`Min`，它返回组中所有元素的表达式的最小值。 仅当集合类型包含其中一种方法时，才支持 `Min`：

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

`Max`，它将返回组中所有元素的表达式的最大值。 仅当集合类型包含其中一种方法时，才支持 `Max`：

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

`Average`，它返回组中所有元素的表达式平均值。 仅当集合类型包含其中一种方法时，才支持 `Average`：

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

`Any`，它确定某个组是否包含成员，或者是否在组中的任何元素上使用布尔表达式。 @no__t 返回一个值，该值可在布尔表达式中使用，并且仅当集合类型包含其中一种方法时才受支持：

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

`All`，确定对于组中的所有元素，布尔表达式是否为 true。 @no__t 返回一个值，该值可在布尔表达式中使用，并且仅当集合类型包含方法时才受支持：

```vb
Function All(predicate As Func(Of T, B)) As B
```

在 @no__t 0 查询运算符之后，范围中以前的范围变量将隐藏，并且 `By` 和 `Into` 子句引入的范围变量可用。 仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

仅当集合类型包含方法时，才支持 `Group` 子句中的范围变量声明：

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

通常转换为

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

__纪录.__ `Group`、`By` 和 @no__t 不是保留字。


### <a name="aggregate-query-operator"></a>聚合查询运算符

@No__t 0 查询运算符与 `Group By` 运算符执行类似的功能，但它允许对已形成的组进行聚合。 由于组已形成，因此，@no__t 查询运算符的 `Into` 子句不会隐藏范围内的范围变量（通过这种方式，`Aggregate` 更像是一个 `Let`，而 @no__t 则更像 @no__t 5）。

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

例如，以下查询聚合了位于华盛顿的客户所下的所有订单总计：

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

此查询的结果是一个集合，其元素类型是一个匿名类型，其元素类型为类型为 `cust`、类型为 `Customer` 的属性和一个名为 `Sum` 类型为  的属性。

与 `Group By` 不同的是，可以将其他查询运算符置于 @no__t 和 @no__t 子句之间。 在 `Aggregate` 子句和 `Into` 子句结束之间，范围内的所有范围变量（包括由 `Aggregate` 子句声明的变量）都可以使用。 例如，下面的查询在2006之前聚合了华盛顿州的客户所下的所有订单的总和总计：

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

@No__t-0 运算符还可用于启动查询表达式。 在这种情况下，查询表达式的结果将是由 `Into` 子句计算得出的单个值。 例如，下面的查询计算在2006年1月1日之前所有订单总计的总和：

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

查询的结果是单个 @no__t 0 值。 @No__t 0 查询运算符始终可用（尽管聚合函数还必须可供表达式有效）。 代码

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

通常转换为

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

__注意。__  `Aggregate` 和 @no__t 不是保留字。


### <a name="group-join-query-operator"></a>分组联接查询运算符

@No__t 0 查询运算符将 @no__t 的函数和 @no__t 的查询运算符组合成单个运算符。 `Group Join` 联接两个基于从元素中提取的匹配键的集合，同时将联接右侧与联接左侧的特定元素相匹配的所有元素组合在一起。 这样，运算符将生成一组分层结果。

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

例如，下面的查询生成元素，这些元素包含单个客户的名称、其所有订单的一组和所有订单的总金额：

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

查询的结果是一个集合，其元素类型为具有三个属性的匿名类型： `Name`，类型化为 `String`，`Orders` 类型化为元素类型为 `Order` 的集合，`OrdersTotal` 类型为 `Integer` 类型。 仅当集合类型包含方法时，才支持 @no__t 0 查询运算符：

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

通常转换为

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

__纪录.__ `Group`、`Join` 和 @no__t 不是保留字。


## <a name="conditional-expressions"></a>条件表达式

条件 @no__t 0 表达式测试表达式并返回值。

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

但与 @no__t 0 运行时函数不同，条件表达式仅在必要时才计算其操作数。 例如，如果 `c` 的值 @no__t 为-2，则表达式 `If(c Is Nothing, c.Name, "Unknown")` 不会引发异常。 条件表达式有两种形式：一个使用两个操作数，另一个采用三个操作数。

如果提供了三个操作数，则所有三个表达式都必须分类为值，第一个操作数必须为布尔表达式。 如果表达式的结果为 true，则第二个表达式将是运算符的结果，否则第三个表达式将是运算符的结果。 表达式的结果类型是第二个和第三个表达式的类型之间的基准类型。 如果没有主导类型，则会发生编译时错误。

如果提供了两个操作数，则两个操作数都必须分类为值，第一个操作数必须是引用类型或可以为 null 的值类型。 然后计算表达式 `If(x, y)`，如同表达式 `If(x IsNot Nothing, x, y)`，但有两个例外。 首先，第一个表达式只计算一次，第二个表达式的第二个操作数的类型为不可为 null 的值类型，第一个操作数的类型为，则从第一个操作数的类型中删除 `?` 会在确定结果的主导类型时从第一个操作数的类型中删除。表达式的类型。 例如：

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

在这两种形式的表达式中，如果操作数为 `Nothing`，则不使用其类型来确定基准类型。 如果表达式 `If(<expression>, Nothing, Nothing)`，则将主导类型视为 @no__t 为-1。


## <a name="xml-literal-expressions"></a>XML 文本表达式

XML 文本表达式表示 XML （可扩展标记语言）1.0 值。

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

XML 文本表达式的结果是一个类型为 `System.Xml.Linq` 命名空间中的类型之一的值。 如果该命名空间中的类型不可用，则 XML 文本表达式将导致编译时错误。 这些值是通过从 XML 文本表达式转换的构造函数调用生成的。 例如，以下代码：

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

大致等效于以下代码：

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

XML 文本表达式可以采用 XML 文档、XML 元素、XML 处理指令、XML 注释或 CDATA 节的形式。

__纪录.__ 此规范仅包含 XML 说明，用于描述 Visual Basic 语言的行为。 有关 XML 的详细信息，请参阅 http://www.w3.org/TR/REC-xml/ 。


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

使用 XML 的词法规则而不是常规 Visual Basic 代码的词法规则来解释 XML 文本表达式。 这两组规则通常在以下方面有所不同：

* 空白在 XML 中是有意义的。 因此，XML 文本表达式的语法明确指出允许使用空格。 空白不会保留，除非它出现在元素内的字符数据的上下文中。 例如：

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

* XML 的行尾空格根据 XML 规范规范化。

* XML 区分大小写。 关键字必须完全匹配大小写，否则将发生编译时错误。

* 行结束符被视为 XML 中的空白区域。 因此，XML 文本表达式中不需要行继续符。

* XML 不接受全角字符。 如果使用全角字符，则会发生编译时错误。


### <a name="embedded-expressions"></a>嵌入的表达式

XML 文本表达式可以包含*嵌入式表达式*。 嵌入式表达式是一种 Visual Basic 表达式，该表达式将进行计算并用于在嵌入表达式的位置填充一个或多个值。

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

例如，下面的代码将字符串 `John Smith` 作为 XML 元素的值。

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

表达式可以嵌入到多个上下文中。 例如，下面的代码生成一个名为 `customer` 的元素：

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

可以使用嵌入式表达式的每个上下文指定将接受的类型。 在嵌入表达式的表达式部分的上下文中，Visual Basic 代码的普通词法规则仍适用，例如，必须使用行继续符：

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

XML 文档会生成一个类型为 `System.Xml.Linq.XDocument` 的值。 与 XML 1.0 规范不同，XML 文本表达式中的 XML 文档需要指定 XML 文档序言;不带 XML 文档序言的 XML 文本表达式解释为其单独的实体。 例如：

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

XML 文档可以包含其类型可以是任何类型的嵌入式表达式;但是，在运行时，对象必须满足 `XDocument` 构造函数的要求，否则将发生运行时错误。

与常规 XML 不同，XML 文档表达式不支持 Dtd （文档类型声明）。 此外，如果提供了编码特性，则将被忽略，因为 Xml 文本表达式的编码始终与源文件本身的编码相同。

__纪录.__ 尽管编码属性被忽略，但它仍然是有效的属性，以便保持在源代码中包含任何有效 Xml 1.0 文档的能力。


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

XML 元素会生成类型为 `System.Xml.Linq.XElement` 的值。 与常规 XML 不同，XML 元素可以在结束标记中省略名称，当前的最嵌套元素将关闭。 例如：

```vb
Dim name = <name>Bob</>
```

XML 元素中的属性声明将生成类型为 `System.Xml.Linq.XAttribute` 的值。 属性值根据 XML 规范进行标准化。 当属性的值为 `Nothing` 时，将不会创建属性，因此不需要检查属性值表达式是否 @no__t 为1。 例如：

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

XML 元素和属性可以在以下位置包含嵌套表达式：

元素的名称，在这种情况下，嵌入式表达式必须是可隐式转换为 `System.Xml.Linq.XName` 的类型的值。 例如：

```vb
Dim name = <<%= "name" %>>Bob</>
```

元素的属性的名称，在这种情况下，嵌入式表达式必须是可隐式转换为 `System.Xml.Linq.XName` 的类型的值。 例如：

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

元素特性的值，在这种情况下，嵌入式表达式可以是任何类型的值。 例如：

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

元素的一个特性，在这种情况下，嵌入式表达式可以是任何类型的值。 例如：

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

元素的内容，在这种情况下，嵌入式表达式可以是任何类型的值。 例如：

```vb
Dim name = <name><%= "Bob" %></>
```

如果嵌入表达式的类型为 `Object()`，则数组将作为 paramarray 传递到 `XElement` 构造函数。


### <a name="xml-namespaces"></a>XML 命名空间

XML 元素可以包含 xml 命名空间声明，如 XML 命名空间1.0 规范所定义。

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

定义命名空间 `xml` 和 `xmlns` 的限制将被强制执行，并将产生编译时错误。 XML 命名空间声明不能有其值的嵌入式表达式;提供的值必须为非空字符串。 例如：

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

__纪录.__ 此规范只包含 XML 命名空间的足够说明，用于描述 Visual Basic 语言的行为。 有关 XML 命名空间的详细信息，请参阅 http://www.w3.org/TR/REC-xml-names/ 。

可以使用命名空间名称限定 XML 元素和属性名称。 命名空间作为常规 XML 绑定，但在文件级别声明的任何命名空间导入都被视为在包含声明的上下文中声明，该声明本身由编译声明的任何命名空间导入括起来。环境. 如果找不到命名空间名称，则会发生编译时错误。 例如：

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

元素中声明的 XML 命名空间不适用于嵌入式表达式中的 XML 文本。 例如：

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

__纪录.__ 这是因为嵌入式表达式可以是任何内容，包括函数调用。 如果函数调用包含 XML 文本表达式，则不清楚程序员是否需要应用或忽略 XML 命名空间。


### <a name="xml-processing-instructions"></a>XML 处理指令

XML 处理指令会生成一个类型为 `System.Xml.Linq.XProcessingInstruction` 的值。 XML 处理指令不能包含嵌入的表达式，因为它们是处理指令中的有效语法。

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

XML 注释会生成一个类型为 `System.Xml.Linq.XComment` 的值。 XML 注释不能包含嵌入的表达式，因为它们是注释中的有效语法。

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

CDATA 节会生成类型为 `System.Xml.Linq.XCData` 的值。 CDATA 节不能包含嵌入的表达式，因为它们是 CDATA 节中的有效语法。

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a>XML 成员访问表达式

XML 成员访问表达式访问 XML 值的成员。

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

有三种类型的 XML 成员访问表达式：

* *元素访问权限*，其中的 XML 名称跟在单点之后。 例如：

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  元素访问映射到函数：

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  因此上面的示例等效于：

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* *属性访问*，其中 Visual Basic 标识符跟在点和 at 符号后，或者 XML 名称后跟一个点和 at 符号。 例如：

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  特性访问映射到函数：

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  因此上面的示例等效于：

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  __纪录.__ 当前未在任何程序集中定义 `AttributeValue` 扩展方法（以及相关扩展属性 `Value`）。 如果需要扩展成员，则会在生成的程序集中自动定义这些成员。

* *后代访问*，其中的 XML 名称遵循三个点。 例如：

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

  后代访问映射到函数：

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  因此上面的示例等效于：

  ```vb
  Dim customers = company.Descendants("customer")
  ```

XML 成员访问表达式的基本表达式必须是值，并且必须是以下类型：

* 如果元素或后代访问，`System.Xml.Linq.XContainer` 或派生类型，或者 `System.Collections.Generic.IEnumerable(Of T)` 或派生类型，其中 `T` 为 @no__t 或派生类型。

* 如果属性访问、@no__t 0 或派生类型，或者 `System.Collections.Generic.IEnumerable(Of T)` 或派生类型，其中 `T` 为 `System.Xml.Linq.XElement` 或派生类型。

XML 成员访问表达式中的名称不能为空。 可以使用由 imports 定义的任何命名空间来限定命名空间。 例如：

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

在 XML 成员访问表达式中的点或尖括号和名称之间不允许使用空格。 例如：

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

如果 `System.Xml.Linq` 命名空间中的类型不可用，则 XML 成员访问表达式将导致编译时错误。


## <a name="await-operator"></a>Await 运算符

Await 运算符与异步方法相关，如[Async 方法](statements.md#async-methods)一节中所述。

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

如果 @no__t 为一个保留字，则如果其出现在其中的直接封闭方法或 lambda 表达式具有 `Async` 修饰符，并且 `Await` 出现在该 `Async` 修饰符之后，则为; 否则为。它不会保留在别处。 它也不会在预处理器指令中保留。 Await 运算符仅允许在它是保留字的方法或 lambda 表达式的主体中使用。 在紧靠后的方法或 lambda 中，await 表达式不能出现在 `Catch` 或 @no__t 块的主体内，也不能出现在 `SyncLock` 语句的主体内，也不在查询表达式中。

Await 运算符采用一个表达式，该表达式必须归类为值，其类型必须为*可等待*类型或 `Object`。 如果其类型为 `Object`，则所有处理都将延迟到运行时。 如果满足以下所有条件，则可等待类型 `C`：

* `C` 包含名为 @no__t 的可访问实例或扩展方法，该实例或扩展方法不具有参数，且返回某种类型 `E`;

* `E` 包含一个名为 @no__t 的可读实例或扩展属性，它不采用任何参数，且具有类型 Boolean;

* `E` 包含一个名为 @no__t 的可访问实例或扩展方法，它不采用参数;

* `E` 实现 @no__t 或 @no__t。

如果 @no__t 为 `Sub`，则 await 表达式归类为 void。 否则，await 表达式归类为值，其类型为 `GetResult` 方法的返回类型。

下面是一个可等待的类的示例：

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

__纪录.__ 建议库作者按照其在同一 @no__t 上调用延续委托的模式，因为 `OnCompleted` 本身在上调用。 此外，不应在 `OnCompleted` 方法中同步执行恢复委托，因为这可能导致堆栈溢出：相反，应将委托排队等待以后执行。

当控制流到达 `Await` 运算符时，行为如下所示。

1.  调用 await 操作数的 `GetAwaiter` 方法。 此调用的结果称为*awaiter*。

2.  将检索 awaiter 的 `IsCompleted` 属性。 如果结果为 true，则：

    21. 调用 awaiter 的 @no__t 0 方法。 如果 `GetResult` 是一个函数，则 await 表达式的值为此函数的返回值。

3.  如果 IsCompleted 属性不为 true，则：

    31. 在 awaiter 上调用 `ICriticalNotifyCompletion.UnsafeOnCompleted` （如果 awaiter 的 @no__t 类型为-1，则实现 `ICriticalNotifyCompletion`）或 `INotifyCompletion.OnCompleted` （否则为）。 在这两种情况下，它会传递与异步方法的当前实例关联的*恢复委托*。

    32. 当前 async 方法实例的控制点已挂起，并在*当前调用方*（在节[async 方法](statements.md#async-methods)中定义）中恢复控制流。

    33. 如果以后调用恢复委托，

        331. 恢复委托首先将 `System.Threading.Thread.CurrentThread.ExecutionContext` 还原到调用 `OnCompleted` 时的状态，
        332. 然后，它在异步方法实例的控制点处恢复控制流（请参阅[异步](statements.md#async-methods)方法部分）。
        333. 其中，它调用 awaiter 的 `GetResult` 方法，如以上2.1 中所示。

如果 await 操作数具有 type 对象，则此行为将推迟到运行时：

- 步骤1通过调用不带参数的 GetAwaiter （）来实现;因此，它可能会在运行时绑定到采用可选参数的实例方法。
- 步骤2是通过检索不带参数的 IsCompleted （）属性以及通过尝试将内部转换为布尔来完成的。
- 步骤 3. 通过尝试 `TryCast(awaiter, ICriticalNotifyCompletion)` 来完成，如果此操作失败，则 `DirectCast(awaiter, INotifyCompletion)`。

传入的恢复委托为3。只能调用一次。 如果多次调用此方法，则该行为是不确定的。

