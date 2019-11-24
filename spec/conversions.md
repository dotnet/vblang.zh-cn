---
ms.openlocfilehash: 7aef52145a71bff1d489772e81eb786a9dbd23d1
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306199"
---
# <a name="conversions"></a>转换

转换是将值从一种类型更改为另一种类型的过程。 例如，可以将类型 `Integer` 的值转换为 `Double`类型的值，也可以将 `Derived` 类型的值转换为类型 `Base`的值，假定 `Base` 和 `Derived` 都是类，`Derived` 从 `Base`继承。 转换可能不需要更改值本身（如后面的示例中所示），或者它们可能需要值表示形式的重大更改（如前一示例中所示）。

转换可能是扩大或收缩转换。 *扩大转换*是指从一种类型到另一种类型的转换，该类型的值域至少与原始类型的值域相同。 扩大转换决不会失败。 *收缩转换*是指从一种类型转换为另一种类型，该类型的值域小于原始类型的值域，或完全不相关，执行转换（例如，从 `Integer` 转换为 `String`时）必须进行额外的处理。 收缩转换可能会导致信息丢失，这可能会导致信息丢失。

为所有类型定义标识转换（即从类型到自身的转换）和默认值转换（即从 `Nothing`转换）。

## <a name="implicit-and-explicit-conversions"></a>隐式转换和显式转换

转换可以是*隐式*的，也可以是*显式*的。 隐式转换在没有任何特殊语法的情况下发生。 下面的示例将 `Integer` 值隐式转换为 `Long` 值：

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

另一方面，显式转换需要强制转换运算符。 如果尝试在没有强制转换运算符的情况下显式转换，将导致编译时错误。 下面的示例使用显式转换将 `Long` 值转换为 `Integer` 值。

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

隐式转换集取决于编译环境和 `Option Strict` 语句。 如果使用严格语义，则只能隐式转换。 如果使用的是可许可语义，则所有扩大转换和收缩转换（即所有转换）可能会隐式进行。

## <a name="boolean-conversions"></a>布尔值转换

尽管 `Boolean` 不是数值类型，但它确实与数值类型进行收缩转换，就好像它是枚举类型一样。 文本 `True` 转换为 `Byte`的文字 `255`，`65535` 用于 `UShort``4294967295` `UInteger``18446744073709551615` `ULong``-1`、`SByte`、`Short`、`Integer`、`Long`、`Decimal`、`Single`、`Double`、、、、和。 文本 `False` 转换为文本 `0`。 零数值转换为文本 `False`。 所有其他数值都转换为 `True`的文本。

存在从布尔值到字符串的收缩转换，转换为 `System.Boolean.TrueString` 或 `System.Boolean.FalseString`。 还存在从 `String` 到 `Boolean`的收缩转换：如果字符串等于 `TrueString` 或 `FalseString` （在当前区域性中为区分），则它将使用适当的值;否则，它会尝试将字符串分析为数值类型（如果可能，为十六进制或八进制），并使用上述规则;否则，会引发 `System.InvalidCastException`。

## <a name="numeric-conversions"></a>数值转换

类型 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single` 和 `Double`以及所有枚举类型之间存在数值转换。 转换时，会将枚举类型视为它们的基础类型。 转换为枚举类型时，源值不需要符合枚举类型中定义的值集。 例如：

```vb
Enum Values
    One
    Two
    Three
End Enum

Module Test
    Sub Main()
        Dim x As Integer = 5

        ' OK, even though there is no enumerated value for 5.
        Dim y As Values = CType(x, Values)
    End Sub
End Module
```

数字转换在运行时处理，如下所示：

* 对于从数值类型到更大数值类型的转换，只需将该值转换为更大的类型。 从 `UInteger`、`Integer`、`ULong`、`Long`或 `Decimal` 到 `Single` 或 `Double` 的转换舍入为最接近的 `Single` 或 `Double` 值。 虽然这种转换可能会导致精度损失，但永远不会导致数量级损失。

* 对于从整型转换为另一整型类型，或从 `Single`、`Double`或 `Decimal` 转换为整型，结果取决于是否启用了整数溢出检查：

  *如果正在检查整数溢出：*

  * 如果源是整数类型，则当源参数在目标类型的范围内时，转换将成功。 如果源参数超出了目标类型的范围，则转换将引发 `System.OverflowException` 异常。

  * 如果源为 `Single`、`Double`或 `Decimal`，则源值将向上或向下舍入到最接近的整数值，并且此整数值将成为转换的结果。 如果源值平均接近于两个整数值，则将值舍入到值在最小有效位位置的偶数。 如果生成的整数值超出了目标类型的范围，则会引发 `System.OverflowException` 异常。

  *如果未检查整数溢出：*

  * 如果源是整数类型，则转换始终会成功，并且只包括丢弃源值的最高有效位。

  * 如果源是 `Single`、`Double`或 `Decimal`，则转换始终会成功，只需将源值舍入到最接近的整数值即可。 如果源值平均接近于两个整数值，则始终将值舍入到最小有效位位置中具有偶数的值。

* 对于从 `Double` 到 `Single`的转换，`Double` 值舍入为最接近的 `Single` 值。 如果 `Double` 值太小而无法表示为 `Single`，则结果将变为零或负零。 如果 `Double` 值太大而无法表示为 `Single`，则结果将变为正无穷或负无穷。 如果 `NaN``Double` 值，则还 `NaN`结果。

* 对于从 `Single` 或 `Double` 到 `Decimal`的转换，将源值转换为 `Decimal` 表示形式，并在需要时舍入为最接近的数字。 如果源值太小而无法表示为 `Decimal`，则结果将变为零。 如果源值 `NaN`、无限大或太大而无法表示为 `Decimal`，则会引发 `System.OverflowException` 异常。

* 对于从 `Double` 到 `Single`的转换，`Double` 值舍入为最接近的 `Single` 值。 如果 `Double` 值太小而无法表示为 `Single`，则结果将变为零或负零。 如果 `Double` 值太大而无法表示为 `Single`，则结果将变为正无穷或负无穷。 如果 `NaN``Double` 值，则还 `NaN`结果。

## <a name="reference-conversions"></a>引用转换

引用类型可以转换为基类型，反之亦然。 如果要转换的值为 null 值、派生类型本身或派生程度更高的类型，则从基类型转换为派生程度更高的类型的转换只会在运行时成功。

类和接口类型可以在任何接口类型之间强制转换。 如果所涉及的实际类型具有继承或实现关系，则类型和接口类型之间的转换只会在运行时成功。 由于接口类型将始终包含从 `Object`派生的类型的实例，因此也可以始终在 `Object`之间来回转换接口类型。

__纪录.__ 将 `NotInheritable` 类转换为与它不实现的接口不是错误的，因为表示 COM 类的类可能具有直到运行时未知的接口实现。 

如果引用转换在运行时失败，则会引发 `System.InvalidCastException` 异常。

### <a name="reference-variance-conversions"></a>引用差异转换

泛型接口或委托可能具有 variant 类型参数，它们允许在类型的兼容变体之间进行转换。 因此，在运行时，从类类型或接口类型到与它继承自或实现的接口类型的变量类型的转换将会成功。 同样，可以将委托类型强制转换为与变体兼容的委托类型。 例如，委托类型

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

允许从 `F(Of Object, Integer)` 到 `F(Of String, Integer)`的转换。 也就是说，可以安全地将 `Object` 的委托 `F` 用作采用 `String`的委托 `F`。 调用委托时，目标方法应为对象，而字符串为对象。

如果以下情况，泛型委托或接口类型 `S(Of S1,...,Sn)` 称为与泛型接口或委托类型的*变体兼容*`T(Of T1,...,Tn)`：

* `S` 和 `T` 都是通过相同的泛型类型 `U(Of U1,...,Un)`构造的。

* 对于每个类型参数 `Ux`：

  * 如果在声明类型参数时没有变体，则 `Sx` 和 `Tx` 必须是同一类型。

  * 如果 `In` 声明类型参数，则必须存在从 `Sx` 到 `Tx`的扩大标识、默认值、引用、数组或类型参数转换。

  * 如果 `Out` 声明类型参数，则必须存在从 `Tx` 到 `Sx`的扩大标识、默认值、引用、数组或类型参数转换。

在从类转换为具有 variant 类型参数的泛型接口时，如果类实现了多个变体兼容接口，如果不存在非变量转换，转换将不明确。 例如：

```vb
Class Base
End Class

Class Derived1
    Inherits Base
End Class

Class Derived2
    Inherits Base
End Class

Class OneAndTwo
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Class BaseAndOneAndTwo
    Implements IEnumerable(Of Base)
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Module Test
    Sub Main()
        ' Error: conversion is ambiguous
        Dim x As IEnumerable(Of Base) = New OneAndTwo()

        ' OK, will pick up the direct implementation of IEnumerable(Of Base)
        Dim y as IEnumerable(Of Base) = New BaseAndOneAndTwo()
    End Sub
End Module
```

### <a name="anonymous-delegate-conversions"></a>匿名委托转换

当归类为 lambda 方法的表达式在没有目标类型（例如 `Dim x = Function(a As Integer, b As Integer) a + b`）或目标类型不是委托类型的上下文中将其重新分类为值时，所生成表达式的类型将是等效于 lambda 方法签名的匿名委托类型。 此匿名委托类型具有到任何兼容的委托类型的转换：兼容的委托类型是可以使用委托创建表达式创建的任何委托类型，该委托类型的委托创建表达式具有匿名委托类型的 `Invoke` 方法作为参数。 例如：

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

请注意，`System.Delegate` 和 `System.MulticastDelegate` 类型本身不被视为委托类型（即使所有委托类型都从它们继承）。 另请注意，从匿名委托类型到兼容委托类型的转换不是引用转换。

## <a name="array-conversions"></a>数组转换

除了在数组上定义的转换，因为它们是引用类型，但对于数组有一些特殊转换。

对于 `A` 和 `B`的任意两个类型，如果它们既不是值类型的引用类型或类型参数，并且 `A` 具有对 `B`的引用、数组或类型参数转换，则从类型 `A` 的数组到类型的数组的转换是具有相同级别的 `B`。 此关系称为*数组协方差*。 具体说来，数组协方差意味着元素类型 `B` 的数组元素实际上可能是元素类型为 `A`的数组元素，前提是 `A` 和 `B` 都是引用类型，并且 `B` 具有到 `A`的引用转换或数组转换。 在下面的示例中，`F` 的第二次调用导致引发 `System.ArrayTypeMismatchException` 异常，因为 `b` 的实际元素类型是 `String`，而不是 `Object`：

```vb
Module Test
    Sub F(ByRef x As Object)
    End Sub

    Sub Main()
        Dim a(10) As Object
        Dim b() As Object = New String(10) {}
        F(a(0)) ' OK.
        F(b(1)) ' Not allowed: System.ArrayTypeMismatchException.
   End Sub
End Module
```

由于数组协方差，对引用类型数组的元素的赋值包含运行时检查，可确保赋给数组元素的值实际上是允许的类型。

```vb
Module Test
    Sub Fill(array() As Object, index As Integer, count As Integer, _
            value As Object)
        Dim i As Integer

        For i = index To (index + count) - 1
            array(i) = value
        Next i
    End Sub

    Sub Main()
        Dim strings(100) As String

        Fill(strings, 0, 101, "Undefined")
        Fill(strings, 0, 10, Nothing)
        Fill(strings, 91, 10, 0)
    End Sub
End Module
```

在此示例中，对 `Fill` 方法中 `array(i)` 的赋值隐式包含一个运行时检查，该检查可确保变量 `value` 引用的对象 `Nothing` 或与数组 `array`的实际元素类型兼容的类型的实例。 在方法 `Main`中，第一次调用方法 `Fill` 成功，但第三次调用会导致在执行第一次分配到 `array(i)`时引发 `System.ArrayTypeMismatchException` 异常。 发生此异常的原因是无法在 `String` 数组中存储 `Integer`。

如果某个数组元素类型是类型参数，而该类型参数的类型在运行时为值类型，则会引发 `System.InvalidCastException` 异常。 例如：

```vb
Module Test
    Sub F(Of T As U, U)(x() As T)
        Dim y() As U = x
    End Sub

    Sub Main()
        ' F will throw an exception because Integer() cannot be
        ' converted to Object()
        F(New Integer() { 1, 2, 3 })
    End Sub
End Module
```

如果数组具有相同的秩，则枚举类型的数组和枚举类型的基础类型的数组或具有相同基础类型的其他枚举类型的数组之间也存在转换。

```vb
Enum Color As Byte
    Red
    Green
    Blue
End Enum

Module Test
    Sub Main()
        Dim a(10) As Color
        Dim b() As Integer
        Dim c() As Byte

        b = a    ' Error: Integer is not the underlying type of Color
        c = a    ' OK
        a = c    ' OK
    End Sub
End Module
```

在此示例中，将 `Color` 数组转换为 `Byte`的数组，`Color`的基础类型。 但是，转换为 `Integer`的数组将是错误的，因为 `Integer` 不是 `Color`的基础类型。

类型为 `A()` 的秩为的数组将数组转换为集合接口类型 `IList(Of B)`、`IReadOnlyList(Of B)`、`ICollection(Of B)`、`IReadOnlyCollection(Of B)` 和 `IEnumerable(Of B)` 中找到的 `System.Collections.Generic`，前提是满足以下条件之一：

- `A` 和 `B` 既不是值类型的引用类型，也不知道类型参数;和 `A` 具有到 `B`的扩大引用、数组或类型参数转换;或
- `A` 和 `B` 均为同一基础类型的枚举类型;或
- `A` 和 `B` 之一是枚举类型，另一个是其基础类型。

具有任何秩的类型 A 的任何数组还会将数组转换为非泛型集合接口类型 `IList`、`ICollection` 和 `System.Collections`中的 `IEnumerable`。

可以使用 `For Each`或直接调用 `GetEnumerator` 方法来循环访问生成的接口。 如果秩为1的数组转换了 `IList` 或 `ICollection`的泛型或非泛型形式，也可以按索引获取元素。 对于转换为泛型或非泛型形式 `IList`的 rank 数组，还可以按索引设置元素，如上文所述，遵循相同的运行时数组协方差检查。 所有其他接口方法的行为都不是由 VB 语言规范定义的;这是由基础运行时组成的。

## <a name="value-type-conversions"></a>值类型转换

值类型值可以转换为它的基引用类型之一，或者转换为它通过一个名为*装箱*的进程实现的接口类型。 将值类型值装箱后，会将该值从它所在的位置复制到 .NET Framework 堆。 然后，将返回对堆的此位置的引用并可将其存储在引用类型变量中。 此引用也称为值类型的*装箱*实例。 装箱实例与引用类型（而不是值类型）具有相同的语义。

装箱的值类型可通过一种称为 "*取消装箱*" 的过程转换回其原始值类型。 装箱的值类型取消装箱后，会将值从堆复制到变量位置。 从此时起，它的行为就像它是值类型。 对值类型取消装箱时，该值必须为 null 值或值类型的实例。 否则，会引发 `System.InvalidCastException` 异常。 如果值是枚举类型的实例，则还可以取消装箱到枚举类型的基础类型或具有相同基础类型的另一个枚举类型。 Null 值被视为文本 `Nothing`。

若要正确支持可以为 null 的值类型，则在进行装箱和取消装箱时，将对值类型 `System.Nullable(Of T)` 进行特殊处理。 如果对类型为的值进行装箱，`Nullable(Of T)` 会生成类型为 `T` 的装箱值，如果值的 `HasValue` 属性 `True`，则为 `Nothing` 值。`HasValue``False` 取消对类型 `T` 的值的装箱 `Nullable(Of T)` 导致 `Nullable(Of T)` 的实例，该实例的 `Value` 属性为装箱值，`HasValue` 属性为 `True`。 值 `Nothing` 可以取消装箱，使其 `Nullable(Of T)` 为任意 `T`，并产生一个值，其 `HasValue` 属性为 `False`。 因为装箱值类型的行为类似于引用类型，所以可以创建对同一值的多个引用。 对于基元类型和枚举类型，这是不相关的，因为这些类型的实例是*不可变*的。 也就是说，不能修改这些类型的已装箱实例，因此，不可能发现存在对同一值的多个引用。

另一方面，如果其实例变量可访问，或者其方法或属性修改了其实例变量，则结构可以是可变的。 如果使用一个对装箱结构的引用来修改该结构，则对装箱结构的所有引用都将看到更改。 由于此结果可能是意外的，因此当类型为 `Object` 的值从一个位置复制到另一个装箱的值类型时，将自动在堆上进行克隆，而不是只复制其引用。 例如：

```vb
Class Class1
    Public Value As Integer = 0
End Class

Structure Struct1
    Public Value As Integer
End Structure

Module Test
    Sub Main()
        Dim val1 As Object = New Struct1()
        Dim val2 As Object = val1

        val2.Value = 123

        Dim ref1 As Object = New Class1()
        Dim ref2 As Object = ref1

        ref2.Value = 123

        Console.WriteLine("Values: " & val1.Value & ", " & val2.Value)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

该程序的输出为：

```console
Values: 0, 123
Refs: 123, 123
```

对局部变量 `val2` 的字段的赋值不会影响局部变量 `val1` 的字段，因为在将装箱的 `Struct1` 分配到 `val2`时，会生成值的副本。 与此相反，赋值 `ref2.Value = 123` 会影响 `ref1` 和 `ref2` 引用的对象。

__纪录.__ 对于类型为 `System.ValueType` 的装箱结构，不能进行结构复制，因为不能从 `System.ValueType`后期绑定。

在分配时将复制装箱值类型的规则有一个例外。 如果装箱值类型引用存储在另一类型中，则不会复制内部引用。 例如：

```vb
Structure Struct1
    Public Value As Object
End Structure

Module Test
    Sub Main()
        Dim val1 As Struct1
        Dim val2 As Struct1

        val1.Value = New Struct1()
        val1.Value.Value = 10

        val2 = val1
        val2.Value.Value = 123
        Console.WriteLine("Values: " & val1.Value.Value & ", " & _
            val2.Value.Value)
    End Sub
End Module
```

该程序的输出为：

```console
Values: 123, 123
```

这是因为在复制值时不会复制内部装箱的值。 因此，`val1.Value` 和 `val2.Value` 都具有对同一装箱值类型的引用。

__纪录.__ 不复制的内部装箱值类型是 .NET 类型系统的一项限制--确保复制 `Object` 的类型的值时，所有内部装箱的值类型都是非常昂贵的。

如前文所述，装箱值类型只能取消装箱为其原始类型。 但是，在类型化为 `Object`时，将对装箱的基元类型进行特殊处理。 它们可以转换为其转换为的任何其他基元类型。 例如：

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

通常，装箱 `Integer` 值 `5` 无法取消装箱到 `Byte` 变量中。 但是，因为 `Integer` 和 `Byte` 是基元类型并且具有转换，所以允许转换。

需要特别注意的是，将值类型转换为接口不同于约束为接口的泛型自变量。 在访问受约束的类型参数上的接口成员（或对 `Object`调用方法）时，不会发生装箱，这与在将值类型转换为接口和访问接口成员时的方式不同。 例如，假设接口 `ICounter` 包含可用于修改值的方法 `Increment`。 如果 `ICounter` 用作约束，则会调用 `Increment` 方法的实现，该方法具有对调用 `Increment` 的变量的引用，而不是装箱副本：

```vb
Interface ICounter
    Sub Increment()
    ReadOnly Property Value() As Integer
End Interface

Structure Counter
    Implements ICounter

    Dim _value As Integer

    Property Value() As Integer Implements ICounter.Value
        Get
            Return _value
        End Get
    End Property

    Sub Increment() Implements ICounter.Increment
       value += 1
    End Sub
End Structure

Module Test
      Sub Test(Of T As ICounter)(x As T)
         Console.WriteLine(x.value)
         x.Increment()                     ' Modify x
         Console.WriteLine(x.value)
         CType(x, ICounter).Increment()    ' Modify boxed copy of x
         Console.WriteLine(x.value)
      End Sub

      Sub Main()
         Dim x As Counter
         Test(x)
      End Sub
End Module
```

对 `Increment` 的第一次调用将修改变量 `x`中的值。 这不等同于对 `Increment`的第二次调用，后者修改 `x`的装箱副本中的值。 因此，该程序的输出为：

```console
0
1
1
```

### <a name="nullable-value-type-conversions"></a>可以为 null 的值类型转换

值类型 `T` 可以转换为类型的可以为 null 的类型，`T?`。 如果要转换的值为 `Nothing`，则从 `T?` 到 `T` 的转换引发 `System.InvalidOperationException` 异常。 此外，如果 `T` 具有到 `S`的内部转换，`T?` 会转换为类型 `S`。 如果 `S` 是值类型，则 `T?` 和 `S?`之间存在以下内部转换：

* 从 `T?` 到 `S?`的相同分类（收缩或扩大）的转换。

* 从 `T` 到 `S?`的相同分类（收缩或扩大）的转换。

* 从 `S?` 到 `T`的收缩转换。

例如，存在从 `Integer?` 到 `Long?` 的内部扩大转换，因为存在从 `Integer` 到 `Long`的内部扩大转换：

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

从 `T?` 转换为 `S?`时，如果 `T?` 的值 `Nothing`，则 `S?` 的值将为 `Nothing`。 从 `S?` 转换为 `T` 或 `T?` `S`时，如果 `T?` 或 `S?` 的值 `Nothing`，则会引发 `System.InvalidCastException` 异常。

由于基础类型 `System.Nullable(Of T)`的行为，当将可为 null 的值类型 `T?` 装箱时，结果为类型 `T`的装箱值，而不是类型 `T?`的装箱值。 相反，当取消对 `T?`的可以为 null 的值类型的装箱时，该值将被 `System.Nullable(Of T)`包装，`Nothing` 将取消装箱为类型 `T?`的 null 值。 例如：

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

此行为的副作用是，可为 null 的值类型 `T?` 显示为实现 `T`的所有接口，因为将值类型转换为接口需要对类型进行装箱。 因此，`T?` 可转换为 `T` 可转换为的所有接口。 但是，请务必注意，可以为 null 的值类型 `T?` 实际上不会实现 `T` 的接口以实现泛型约束检查或反射。 例如：

```vb
Interface I1
End Interface

Structure T1
    Implements I1
    ...
End Structure

Module Test
    Sub M1(Of T As I1)(ByVal x As T)
    End Sub

    Sub Main()
        Dim x? As T1 = Nothing
        Dim y As I1 = x                ' Valid
        M1(x)                          ' Error: x? does not satisfy I1 constraint
    End Sub
End Module
```

## <a name="string-conversions"></a>字符串转换

将 `Char` 转换为 `String` 会导致字符串的第一个字符为字符值。 将 `String` 转换为 `Char` 会生成一个字符，其值为字符串的第一个字符。 将 `Char` 数组转换为 `String` 会生成一个字符串，其字符是数组的元素。 将 `String` 转换为 `Char` 的数组将导致其元素为字符串的字符的字符数组。

`String` 和 `Boolean`、`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`、`Double`、`Date`等之间的精确转换超出了本规范的范围，因此它的实现依赖于一种详细的异常。 字符串转换始终考虑运行时环境的当前区域性。 因此，它们必须在运行时执行。

## <a name="widening-conversions"></a>扩大转换

扩大转换永不溢出，但可能会损失精度。 下面的转换是扩大转换：

__标识/默认转换__

* 从类型到自身。

* 从为 lambda 方法生成的匿名委托类型重新分类为具有相同签名的任何委托类型。

* 从文本 `Nothing` 到类型。

__数值转换__

* 从 `Byte` 到 `UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`或 `Double`。

* 从 `SByte` 到 `Short`、`Integer`、`Long`、`Decimal`、`Single`或 `Double`。

* 从 `UShort` 到 `UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`或 `Double`。

* 从 `Short` 到 `Integer`、`Long`、`Decimal`、`Single` 或 `Double`。

* 从 `UInteger` 到 `ULong`、`Long`、`Decimal`、`Single`或 `Double`。

* 从 `Integer` 到 `Long`、`Decimal`、`Single` 或 `Double`。

* 从 `ULong` 到 `Decimal`、`Single`或 `Double`。

* 从 `Long` 到 `Decimal`、`Single` 或 `Double`。

* 从 `Decimal` 到 `Single` 或 `Double`。

* 从 `Single` 到 `Double`。

* 从文本 `0` 到枚举的类型。 （__注意：__ 从 `0` 到任何枚举类型的转换都是扩大，以简化测试标志。 例如，如果 `Values` 是值为 `One`的枚举类型，则可以通过口述 `(v And Values.One) = 0`来测试类型 `Values` 的变量 `v`。）

* 从枚举类型转换为其基础数值类型，或转换为其基础数值类型的扩大转换为的数值类型。

* 从类型 `ULong`、`Long`、`UInteger`、`Integer`、`UShort`、`Short`、`Byte`或 `SByte` 到较小类型的常量表达式，前提是常量表达式的值在目标类型的范围内。 （__注意：__ 从 `UInteger` 或 `Integer` 到 `Single`、`ULong` 或 `Long` 到 `Single` 或 `Double`或 `Decimal` 到 `Single` 或 `Double` 的转换可能会导致精度损失，但永远不会导致数量级损失。 其他扩大数值转换决不会丢失任何信息。）

__引用转换__

* 从引用类型到基类型。

* 从引用类型到接口类型，前提是该类型实现接口或与变体兼容的接口。

* 从接口类型到 `Object`。

* 从接口类型到与变体兼容的接口类型。

* 从委托类型到与变体兼容的委托类型。 （__注意：__ 许多其他引用转换都是由这些规则隐式的。 例如，匿名委托是继承自 `System.MulticastDelegate`的引用类型;数组类型是继承自 `System.Array`的引用类型;匿名类型是继承自 `System.Object`的引用类型。）

__匿名委托转换__

* 从为 lambda 方法生成的匿名委托类型重新分类为任何更宽的委托类型。

__数组转换__

* 如果满足以下所有条件，则可以从数组类型 `S` 具有元素类型 `Se` 到 `T` `Te`元素类型为的数组类型：

  * `S` 和 `T` 仅在元素类型上存在差异。

  * `Se` 和 `Te` 都是引用类型，或者是已知为引用类型的类型参数。

  * 存在从 `Se` 到 `Te`的扩大引用、数组或类型参数转换。

* 如果满足以下所有条件，则可以使用枚举元素类型 `S` 具有枚举元素类型 `Se` 到数组类型 `T` 元素类型 `Te`）：

  * `S` 和 `T` 仅在元素类型上存在差异。

  * `Te` 是 `Se`的基础类型。

* 在数组类型 `S`，其枚举元素类型 `Se`为，如果 `System.Collections.Generic.IList(Of Te)`、`IReadOnlyList(Of Te)`、`ICollection(Of Te)`、`IReadOnlyCollection(Of Te)`和 `IEnumerable(Of Te)`，则满足以下条件之一：

  * `Se` 和 `Te` 都是引用类型，或者是已知为引用类型的类型参数，并且存在从 `Se` 到 `Te`的扩大引用、数组或类型参数转换。或

  * `Te` 是 `Se`的基础类型;或

  * `Te` 等于 `Se`

__值类型转换__

* 从值类型到基类型。

* 从值类型到类型实现的接口类型。

__可以为 null 的值类型转换__

* 从类型 `T` `T?`的类型。

* 从类型 `T?` 到类型 `S?`，其中存在从类型 `T` 到类型 `S`的扩大转换。

* 从类型 `T` 到类型 `S?`，其中存在从类型 `T` 到类型 `S`的扩大转换。

* 从类型 `T?` 到类型 `T` 实现的接口类型。

__字符串转换__

* 从 `Char` 到 `String`。

* 从 `Char()` 到 `String`。

__类型参数转换__

* 从类型参数 `Object`。

* 从类型参数到接口类型约束或任何与接口类型约束兼容的接口变体。

* 从类型参数到由类约束实现的接口。

* 从类型参数到与类约束实现的接口类型兼容的接口变体。

* 从类型参数到类约束或类约束的基类型。

* 从类型参数 `T` 到类型参数约束 `Tx`，或 `Tx` 的任何内容都是扩大的转换。

## <a name="narrowing-conversions"></a>收缩转换

收缩转换是指无法证明始终成功、已知的转换可能会丢失信息的转换，以及跨类型的域之间的转换非常不同于采用收缩表示法。 下面的转换归类为收缩转换：

__布尔值转换__

* 从 `Boolean` 到 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`或 `Double`。

* 从 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`或 `Double` 到 `Boolean`。

__数值转换__

* 从 `Byte` 到 `SByte`。

* 从 `SByte` 到 `Byte`、`UShort`、`UInteger`或 `ULong`。

* 从 `UShort` 到 `Byte`、`SByte`或 `Short`。

* 从 `Short` 到 `Byte`、`SByte`、`UShort`、`UInteger`或 `ULong`。

* 从 `UInteger` 到 `Byte`、`SByte`、`UShort`、`Short`或 `Integer`。

* 从 `Integer` 到 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`或 `ULong`。

* 从 `ULong` 到 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`或 `Long`。

* 从 `Long` 到 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`或 `ULong`。

* 从 `Decimal` 到 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`或 `Long`。

* 从 `Single` 到 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`或 `Decimal`。

* 从 `Double` 到 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`或 `Single`。

* 从数值类型到枚举类型。

* 从枚举类型到数值类型，其基础数值类型的收缩转换为。

* 从枚举类型到另一个枚举类型。

__引用转换__

* 从引用类型到派生程度更高的类型。

* 从类类型到接口类型，前提是类类型不实现接口类型或与之兼容的接口类型 variant。

* 从接口类型到类类型。

* 从接口类型到另一个接口类型，但前提是这两个类型之间没有继承关系，并且提供它们不是变体兼容的。

__匿名委托转换__

* 从为 lambda 方法生成的匿名委托类型重新分类为任何较窄的委托类型。

__数组转换__

* 从 `Se`的元素类型 `S` 到数组类型，`T` 元素类型为 `Te`的数组类型，前提是满足以下所有条件：

  * `S` 和 `T` 仅在元素类型上存在差异。
  * `Se` 和 `Te` 都是引用类型，或者是不知道其值类型的类型参数。
  * 存在从 `Se` 到 `Te`的收缩引用、数组或类型参数转换。

* 如果满足以下所有条件，则可以从数组类型 `S` 具有元素类型 `Se` 到 `T` 枚举 `Te`元素类型为的数组类型：

  * `S` 和 `T` 仅在元素类型上存在差异。
  * `Se` 是 `Te` 的基础类型，或者它们都是共享同一基础类型的不同枚举类型。

* 在数组类型 `S`，其枚举元素类型 `Se`为 `IList(Of Te)`、`IReadOnlyList(Of Te)`、`ICollection(Of Te)`、`IReadOnlyCollection(Of Te)` 和 `IEnumerable(Of Te)`，前提是满足以下条件之一：

  * `Se` 和 `Te` 都是引用类型，或者是已知为引用类型的类型参数，并且存在从 `Se` 到 `Te`的收缩引用、数组或类型参数转换。或
  * `Se` 是 `Te`的基础类型，或者它们都是共享同一基础类型的不同枚举类型。

__值类型转换__

* 从引用类型到派生程度更高的值类型。

* 从接口类型到值类型（如果值类型实现接口类型）。

__可以为 null 的值类型转换__

* 从类型 `T?` 到 `T`类型。

* 从类型 `T?` 到类型 `S?`，其中存在从类型 `T` 到类型 `S`的收缩转换。

* 从类型 `T` 到类型 `S?`，其中存在从类型 `T` 到类型 `S`的收缩转换。

* 从类型 `S?` 到类型 `T`，其中存在从类型 `S` 到类型 `T`的转换。

__字符串转换__

* 从 `String` 到 `Char`。

* 从 `String` 到 `Char()`。

* 从 `String` 到 `Boolean` 和从 `Boolean` 到 `String`。

* `String` 和 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Decimal`、`Single`或 `Double`之间的转换。

* 从 `String` 到 `Date` 和从 `Date` 到 `String`。

__类型参数转换__

* 从 `Object` 到类型参数。

* 从类型参数到接口类型，前提是类型参数不受限于该接口或约束为实现该接口的类。

* 从接口类型到类型参数。

* 从类型参数到类约束的派生类型。

* 从类型参数 `T` 到类型参数约束 `Tx` 具有到的收缩转换。

## <a name="type-parameter-conversions"></a>类型参数转换

类型参数的转换由约束（如果有）决定。 类型参数 `T` 始终可以从 `Object`转换为自身，也可以从任何接口类型转换为和。 请注意，如果类型 `T` 在运行时是值类型，则从 `T` 转换为 `Object` 或接口类型将是装箱转换，并从 `Object` 或接口类型转换为 `T` 将是取消装箱转换。 具有类约束的类型参数 `C` 定义从类型参数到 `C` 及其基类的附加转换，反之亦然。 类型参数 `T` 带有类型参数约束 `Tx` 定义转换为 `Tx`，以及 `Tx` 转换为的任何内容。

元素类型为类型参数的数组，该类型参数的接口约束 `I` 与元素类型为 `I`的数组具有相同的协变数组转换，前提是该类型参数还具有 `Class` 或类约束（因为只有引用数组元素类型可以是协变的）。 一个数组，其元素类型为具有类约束的类型参数 `C` 与元素类型为 `C`的数组具有相同的协变数组转换。

上述转换规则不允许将不受约束的类型参数转换为非接口类型，这可能会令人吃惊。 这样做的原因是为了防止混淆此类转换的语义。 例如，请考虑以下声明：

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

如果允许 `T` 转换为 `Integer`，则很可能会认为 `X(Of Integer).F(7)` 会返回 `7L`。 但是，它不会，因为只有在编译时已知类型为数字的情况下才考虑数值转换。 为了使语义清晰明了，必须改为编写以上示例：

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a>用户定义的转换

*内部转换*是由语言（即在此规范中列出）定义的转换，而*用户定义的转换*是通过重载 `CType` 运算符来定义的。 在类型之间进行转换时，如果没有任何内部转换适用，则将考虑用户定义的转换。 如果存在*最*适用于源和目标类型的用户定义的转换，则将使用用户定义的转换。 否则，将产生编译时错误。 最具体的转换是操作数与源类型 "最接近" 的转换，并且其结果类型与目标类型 "最近"。 确定要使用的用户定义的转换时，将使用最特定的扩大转换;如果没有最具体的扩大转换，将使用最具体的收缩转换。 如果没有最具体的收缩转换，则转换是不确定的，并发生编译时错误。

以下部分介绍如何确定最具体的转换。 它们使用以下术语：

如果存在从类型 `A` 到类型 `B`的内部扩大转换，并且无论 `A` 和 `B` 都是接口，则 `A`*包含*`B`，`B`*包含*`A`。

一组类型中包含的*最大*的类型是一种包含集内所有其他类型的类型。 如果没有一种类型包含所有其他类型，则该集没有最大的包含类型。 直观地说，最包含的类型是集中的 "最大" 类型，这种类型是每个其他类型可通过扩大转换进行转换的一种类型。

类型集中*最常被包含*的类型是一种类型，它由集中的所有其他类型所包含。 如果所有其他类型都不包含任何一种类型，则该集不包含大多数被包含的类型。 简而言之，最常被包含的类型是集合中的 "最小" 类型，一种类型可以通过收缩转换转换为其他类型。

在为类型 `T?`收集候选用户定义的转换时，将改用 `T` 定义的用户定义的转换运算符。 如果要转换为的类型也是可以为 null 的值类型，则会提升 `T`的任何仅涉及不可以为 null 的值类型的用户定义的转换运算符。 从 `T` 到 `S` 的转换运算符被提升为从 `T?` 到 `S?` 的转换，并通过将 `T?` 转换为 `T`（如有必要），然后将用户定义的转换运算符从 `T` 转换为 `S`，然后将 `S` 转换为 `S?`（如有必要）。 但是，如果要转换的值为 `Nothing`，则提升转换运算符会直接转换为类型为 `Nothing` 类型为 `S?`的值。 例如：

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Legal: y is still null
        x = New T()
        y = x                ' Legal: Converts from T to S
    End Sub
End Module
```

解析转换时，用户定义的转换运算符始终优于提升转换运算符。 例如：

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator

    Public Shared Widening Operator CType(ByVal v As T?) As S?
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Calls user-defined conversion, not lifted conversion
    End Sub
End Module
```

在运行时，评估用户定义的转换最多可以涉及三个步骤：

1. 首先，根据需要，使用内部转换将值从源类型转换为操作数类型。

2. 然后，调用用户定义的转换。

3. 最后，如果需要，用户定义的转换的结果将使用内部转换转换为目标类型。

务必要注意，用户定义的转换的计算决不会涉及多个用户定义的转换运算符。

### <a name="most-specific-widening-conversion"></a>最特定的扩大转换

使用以下步骤来确定两种类型之间最特定的用户定义扩大转换运算符：

1. 首先，收集所有候选转换运算符。 候选转换运算符是源类型中所有用户定义的扩大转换运算符，以及目标类型中所有用户定义的扩大转换运算符。

2. 然后，将从该集中删除所有不适用的转换运算符。 如果存在从源类型到操作数类型的内部扩大转换运算符，并且存在从运算符结果到目标类型的内部扩大转换运算符，则转换运算符适用于源类型和目标类型。 如果没有适用的转换运算符，则没有最特定的扩大转换。

3. 然后，确定适用转换运算符的最特定源类型：

   * 如果任何转换运算符直接从源类型转换，则源类型将是最具体的源类型。

   * 否则，最特定的源类型是转换运算符的组合源类型集中最常被包含的类型。 如果找不到最能包含的类型，则没有最特定的扩大转换。

4. 然后，确定适用转换运算符的最特定目标类型：

   * 如果任何转换运算符直接转换为目标类型，则目标类型为最特定的目标类型。

   * 否则，最特定目标类型是转换运算符的组合目标类型集中最包含的类型。 如果找不到大多数包含的类型，则没有最特定的扩大转换。

5. 然后，如果只有一个转换运算符从最具体的源类型转换为最具体的目标类型，则这是最具体的转换运算符。 如果存在多个这样的运算符，则没有最特定的扩大转换。

### <a name="most-specific-narrowing-conversion"></a>最特定的收缩转换

使用以下步骤来确定两种类型之间最特定的用户定义的收缩转换运算符：

1. 首先，收集所有候选转换运算符。 候选转换运算符是源类型中所有用户定义的转换运算符，以及目标类型中所有用户定义的转换运算符。

2. 然后，将从该集中删除所有不适用的转换运算符。 如果存在从源类型到操作数类型的内部转换运算符，并且存在从运算符结果到目标类型的内部转换运算符，则转换运算符适用于源类型和目标类型。 如果没有适用的转换运算符，则没有最具体的收缩转换。

3. 然后，确定适用转换运算符的最特定源类型：

   * 如果任何转换运算符直接从源类型转换，则源类型将是最具体的源类型。

   * 否则，如果任何转换运算符从包含源类型的类型转换，则最具体的源类型是那些转换运算符的一组源类型中包含程度最高的类型。 如果找不到最能包含的类型，则没有最具体的收缩转换。

   * 否则，最特定的源类型是转换运算符的组合源类型集中最包含的类型。 如果找不到大多数包含的类型，则没有最具体的收缩转换。

4. 然后，确定适用转换运算符的最特定目标类型：

   * 如果任何转换运算符直接转换为目标类型，则目标类型为最特定的目标类型。

   * 否则，如果任何转换运算符转换为目标类型所包含的类型，则最特定目标类型是这些转换运算符的组合源类型集中最包含的类型。 如果找不到大多数包含的类型，则没有最具体的收缩转换。

   * 否则，最特定目标类型是转换运算符的组合目标类型集中最常被包含的类型。 如果找不到最能包含的类型，则没有最具体的收缩转换。

5. 然后，如果只有一个转换运算符从最具体的源类型转换为最具体的目标类型，则这是最具体的转换运算符。 如果存在多个这样的运算符，则没有最具体的收缩转换。

## <a name="native-conversions"></a>本机转换

一些转换归类为*本机转换*，因为这些转换在 .NET Framework 本机支持。 这些转换是可通过使用 `DirectCast` 和 `TryCast` 转换运算符以及其他特殊行为进行优化的转换。 归类为本机转换的转换包括：标识转换、默认转换、引用转换、数组转换、值类型转换和类型参数转换。

## <a name="dominant-type"></a>主导类型

给定一组类型，通常在某些情况下（如类型推理）确定集的*主导类型*是必需的。 类型集的主导类型是先删除一个或多个其他类型没有隐式转换为的类型。 如果此时没有任何类型，则没有主导类型。 然后，主要类型是其余类型中包含的最大的类型。 如果有多个最常包含的类型，则没有主导类型。
