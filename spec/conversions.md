---
ms.openlocfilehash: 7aef52145a71bff1d489772e81eb786a9dbd23d1
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306199"
---
# <a name="conversions"></a>转换

转换是将值从一种类型更改为另一种类型的过程。 例如，可以将类型 @no__t 的值转换为 `Double` 类型的值，或类型 `Derived` 的值可转换为 `Base` 类型的值，假定 @no__t 和 @no__t 都是类，`Derived` 从 @no__t 中继承。 转换可能不需要更改值本身（如后面的示例中所示），或者它们可能需要值表示形式的重大更改（如前一示例中所示）。

转换可能是扩大或收缩转换。 *扩大转换*是指从一种类型到另一种类型的转换，该类型的值域至少与原始类型的值域相同。 扩大转换决不会失败。 *收缩转换*是指从一种类型到另一种类型的转换，该类型的值域要么小于原始类型的值域，要么完全不相关，因此在执行转换时，必须采取额外的措施（例如，转换从 `Integer` 到 `String`）。 收缩转换可能会导致信息丢失，这可能会导致信息丢失。

为所有类型定义标识转换（即从类型到自身的转换）和默认值转换（即从 `Nothing` 的转换）。

## <a name="implicit-and-explicit-conversions"></a>隐式转换和显式转换

转换可以是*隐式*的，也可以是*显式*的。 隐式转换在没有任何特殊语法的情况下发生。 下面的示例将 `Integer` 值隐式转换为 @no__t 值：

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

另一方面，显式转换需要强制转换运算符。 如果尝试在没有强制转换运算符的情况下显式转换，将导致编译时错误。 下面的示例使用显式转换将 @no__t 0 值转换为 @no__t 值。

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

尽管 `Boolean` 不是数值类型，但它确实与数值类型进行收缩转换，就好像它是枚举类型一样。 文本 `True` 会转换为 `Byte` 的文本 `255`; 对于 `UShort`，为 `65535`，`UInteger` `4294967295`，`ULong` 转换为表达式 `-1`，2，4，5，5和 6。 文本 `False` 会转换为文本 `0`。 零数值转换为文本 `False`。 所有其他数值均转换为 `True` 的文字。

存在从布尔值到字符串的收缩转换，转换为 `System.Boolean.TrueString` 或 @no__t 为-1。 还存在从 `String` 到 `Boolean` 的收缩转换：如果字符串等于 `TrueString` 或 `FalseString` （在当前区域性中，区分），则它将使用适当的值;否则，它会尝试将字符串分析为数值类型（如果可能，为十六进制或八进制），并使用上述规则;否则，会引发 `System.InvalidCastException`。

## <a name="numeric-conversions"></a>数值转换

类型 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、@no__t 为 @no__t、`Long`、`Decimal`、@no__t 和，以及所有枚举类型之间存在数字转换。 转换时，会将枚举类型视为它们的基础类型。 转换为枚举类型时，源值不需要符合枚举类型中定义的值集。 例如：

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

* 对于从数值类型到更大数值类型的转换，只需将该值转换为更大的类型。 从 `UInteger`、`Integer`、`ULong`、@no__t 或 @no__t 到 @no__t 或 @no__t 的转换将舍入到最接近的 `Single` 或 @no__t 8 值。 虽然这种转换可能会导致精度损失，但永远不会导致数量级损失。

* 如果从整型转换为另一整型类型，或从 `Single`、`Double` 或 `Decimal` 转换为整型类型，则结果取决于是否启用了整数溢出检查：

  *如果正在检查整数溢出：*

  * 如果源是整数类型，则当源参数在目标类型的范围内时，转换将成功。 如果 source 参数超出了目标类型的范围，则该转换会引发 `System.OverflowException` 异常。

  * 如果源 `Single`、@no__t 为-1 或 `Decimal`，则源值将向上或向下舍入到最接近的整数值，并且此整数值将成为转换的结果。 如果源值平均接近于两个整数值，则将值舍入到值在最小有效位位置的偶数。 如果生成的整数值超出了目标类型的范围，则会引发 `System.OverflowException` 异常。

  *如果未检查整数溢出：*

  * 如果源是整数类型，则转换始终会成功，并且只包括丢弃源值的最高有效位。

  * 如果源 `Single`、@no__t 为-1 或 `Decimal`，则转换始终会成功，并且只需将源值舍入到最接近的整数值。 如果源值平均接近于两个整数值，则始终将值舍入到最小有效位位置中具有偶数的值。

* 对于从 `Double` 到 `Single` 的转换，`Double` 值舍入为最接近的 @no__t 值。 如果 `Double` 值太小而无法表示为 `Single`，则结果将变为零或负零。 如果 @no__t 0 值太大，无法表示为 `Single`，则结果将变为正无穷或负无穷。 如果 @no__t 0 @no__t 值为-1，则结果也为 `NaN`。

* 对于从 @no__t 0 或 `Double` 到 `Decimal` 的转换，将源值转换为 `Decimal` 表示形式，并在需要时舍入为最接近的数字。 如果源值太小而无法表示为 `Decimal`，则结果将变为零。 如果源值 `NaN`、无限大或太大而无法表示为 `Decimal`，则会引发 @no__t 2 异常。

* 对于从 `Double` 到 `Single` 的转换，`Double` 值舍入为最接近的 @no__t 值。 如果 `Double` 值太小而无法表示为 `Single`，则结果将变为零或负零。 如果 @no__t 0 值太大，无法表示为 `Single`，则结果将变为正无穷或负无穷。 如果 @no__t 0 @no__t 值为-1，则结果也为 `NaN`。

## <a name="reference-conversions"></a>引用转换

引用类型可以转换为基类型，反之亦然。 如果要转换的值为 null 值、派生类型本身或派生程度更高的类型，则从基类型转换为派生程度更高的类型的转换只会在运行时成功。

类和接口类型可以在任何接口类型之间强制转换。 如果所涉及的实际类型具有继承或实现关系，则类型和接口类型之间的转换只会在运行时成功。 由于接口类型将始终包含从 @no__t 派生的类型的实例，因此接口类型也可以始终在 @no__t 之间来回转换。

__纪录.__ 将 @no__t 0 类与它不实现的接口相互转换不是错误的，因为表示 COM 类的类可能具有直到运行时未知的接口实现。 

如果引用转换在运行时失败，则会引发 `System.InvalidCastException` 异常。

### <a name="reference-variance-conversions"></a>引用差异转换

泛型接口或委托可能具有 variant 类型参数，它们允许在类型的兼容变体之间进行转换。 因此，在运行时，从类类型或接口类型到与它继承自或实现的接口类型的变量类型的转换将会成功。 同样，可以将委托类型强制转换为与变体兼容的委托类型。 例如，委托类型

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

允许从 `F(Of Object, Integer)` 到 @no__t 的转换。 也就是说，可以安全地将委托 `F` （采用 `Object`）用作委托 `F`，该委托采用 `String`。 调用委托时，目标方法应为对象，而字符串为对象。

如果存在以下情况，则泛型委托或接口类型 `S(Of S1,...,Sn)` 称为与泛型接口或委托类型的*变体兼容*`T(Of T1,...,Tn)`：

* `S` 和 `T` 均从相同的泛型类型 `U(Of U1,...,Un)` 构造。

* 对于每个类型参数 `Ux`：

  * 如果在声明类型参数时没有变体，则 `Sx`，`Tx` 必须是同一类型。

  * 如果类型参数已声明 `In`，则必须具有从 `Sx` 到 @no__t 的扩大标识、默认值、引用、数组或类型参数转换。

  * 如果类型参数已声明 `Out`，则必须具有从 `Tx` 到 @no__t 的扩大标识、默认值、引用、数组或类型参数转换。

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

当归类为 lambda 方法的表达式在没有目标类型（例如 `Dim x = Function(a As Integer, b As Integer) a + b`）或目标类型不是委托类型的上下文中将其重新归类为某个值时，生成的表达式的类型是等效的匿名委托类型lambda 方法的签名。 此匿名委托类型具有到任何兼容的委托类型的转换：兼容的委托类型是可以使用委托创建表达式创建的任何委托类型，该委托类型将匿名委托类型的 `Invoke` 方法作为参数。 例如：

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

请注意，类型 `System.Delegate` 和 `System.MulticastDelegate` 本身不被视为委托类型（即使所有委托类型都从它们继承）。 另请注意，从匿名委托类型到兼容委托类型的转换不是引用转换。

## <a name="array-conversions"></a>数组转换

除了在数组上定义的转换，因为它们是引用类型，但对于数组有一些特殊转换。

对于任意两个类型 `A` 和 `B`，如果它们既不是值类型的引用类型或类型参数，并且如果 `A` 具有对 `B` 类型的引用、数组或类型参数转换，则存在从 `A` 类型的数组到数组的转换键入 `B` 的相同级别。 此关系称为*数组协方差*。 具体说来，数组协方差意味着元素类型为 `B` 的数组元素可能是元素类型为 `A` 的数组元素，前提是 `A` 和 @no__t 都是引用类型，并且 `B` 具有引用转换或数组转换为 @no__t 5。 在下面的示例中，第二次调用 `F` 会导致引发 @no__t 1 异常，因为 `b` 的实际元素类型为 `String`，而不是 `Object`：

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

在此示例中，对方法 `Fill` 中 @no__t 的赋值隐式包含一个运行时检查，该检查可确保变量 @no__t 引用的对象 @no__t 为3，或与数组的实际元素类型兼容的类型的实例 `array`。 在方法 `Main` 中，第一次调用方法 `Fill` 成功，但第三次调用会导致在执行第一次到 `array(i)` 的赋值运算时引发 @no__t 2 异常。 发生此异常的原因是，@no__t 0 不能存储在 @no__t 的数组中。

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

在此示例中，将 `Color` 的数组转换为 @no__t 的数组，@no__t 2 的基础类型。 但是，转换为 `Integer` 的数组将是错误的，因为 `Integer` 不是 @no__t 的基础类型。

类型为 `A()` 的秩为的数组将转换为集合接口类型，@no__t 为-1、`IReadOnlyList(Of B)`、`ICollection(Of B)`、`IReadOnlyCollection(Of B)` 和 `IEnumerable(Of B)`，但前提是满足以下条件之一：

- `A` 和 `B` 既是引用类型，也不是值类型未知的类型参数;和 `A` 的扩展引用、数组或类型参数转换为 `B`;或
- `A` 和 `B` 都是同一基础类型的枚举类型;或
- @no__t 0 和 `B` 中的一个是枚举类型，另一个是其基础类型。

具有任何秩的类型 A 的任何数组还会将数组转换为非泛型集合接口类型 `IList`、`ICollection` 和 `IEnumerable` 中找到的 。

可以使用 `For Each` 或直接调用 @no__t 1 方法来循环访问生成的接口。 对于第1级数组，将 `IList` 或 `ICollection` 的泛型或非泛型形式转换为，也可以按索引获取元素。 对于转换为泛型或非泛型形式 `IList` 的 rank 数组，还可以按索引设置元素，如上文所述，受相同的运行时数组协变检查的限制。 所有其他接口方法的行为都不是由 VB 语言规范定义的;这是由基础运行时组成的。

## <a name="value-type-conversions"></a>值类型转换

值类型值可以转换为它的基引用类型之一，或者转换为它通过一个名为*装箱*的进程实现的接口类型。 将值类型值装箱后，会将该值从它所在的位置复制到 .NET Framework 堆。 然后，将返回对堆的此位置的引用并可将其存储在引用类型变量中。 此引用也称为值类型的*装箱*实例。 装箱实例与引用类型（而不是值类型）具有相同的语义。

装箱的值类型可通过一种称为 "*取消装箱*" 的过程转换回其原始值类型。 装箱的值类型取消装箱后，会将值从堆复制到变量位置。 从此时起，它的行为就像它是值类型。 对值类型取消装箱时，该值必须为 null 值或值类型的实例。 否则，会引发 `System.InvalidCastException` 异常。 如果值是枚举类型的实例，则还可以取消装箱到枚举类型的基础类型或具有相同基础类型的另一个枚举类型。 Null 值被视为文本 `Nothing`。

若要正确支持可以为 null 的值类型，则在进行装箱和取消装箱时，将对值类型 `System.Nullable(Of T)` 进行特殊处理。 如果将类型 @no__t 的值装箱为类型，则会生成类型为 @no__t 的装箱值。如果值的 `HasValue` 属性为 `True`，则返回值为 `Nothing` 的值，如果该值的 `HasValue` 属性为 `False`。 取消对类型 `T` 到 `Nullable(Of T)` 的值将导致 `Nullable(Of T)` 的实例，该实例的 `Value` 属性为装箱值，其 @no__t 属性为 `True`。 值 `Nothing` 可以取消装箱，以便对任何 `T` `Nullable(Of T)`，并生成一个值，其 @no__t 3 属性 @no__t。 因为装箱值类型的行为类似于引用类型，所以可以创建对同一值的多个引用。 对于基元类型和枚举类型，这是不相关的，因为这些类型的实例是*不可变*的。 也就是说，不能修改这些类型的已装箱实例，因此，不可能发现存在对同一值的多个引用。

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

对局部变量 `val2` 的赋值不会影响局部变量 `val1`，因为在向 `Struct1` 分配装箱  时，会生成值的副本。 与此相反，赋值 `ref2.Value = 123` 会影响 `ref1` 和 @no__t 2 引用的对象。

__纪录.__ 对于类型为 `System.ValueType` 的装箱结构，不能进行结构复制，因为不能从 @no__t 的后期绑定。

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

这是因为在复制值时不会复制内部装箱的值。 因此，@no__t 0 和 `val2.Value` 都具有对同一装箱值类型的引用。

__纪录.__ 不复制的内部装箱值类型是 .NET 类型系统的一项限制--为了确保在复制 `Object` 类型的值时，将复制所有内部装箱的值类型。

如前文所述，装箱值类型只能取消装箱为其原始类型。 但是，在类型化为 `Object` 时，将对装箱的基元类型进行特殊处理。 它们可以转换为其转换为的任何其他基元类型。 例如：

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

通常情况下，不能将装箱 `Integer` 值 @no__t 取消装箱到 @no__t 的变量中。 但是，因为 `Integer`，`Byte` 是基元类型并且具有转换，所以允许转换。

需要特别注意的是，将值类型转换为接口不同于约束为接口的泛型自变量。 在访问受约束的类型参数上的接口成员（或调用 @no__t 的方法）时，不会发生装箱，这与访问接口的值类型和访问接口成员时的方式不同。 例如，假设接口 `ICounter` 包含可用于修改值的方法 `Increment`。 如果 `ICounter` 用作约束，则会调用 @no__t 的方法的实现，该方法是对在上调用 `Increment` 的变量的引用，而不是装箱副本：

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

对 @no__t 的第一次调用将修改变量 `x` 中的值。 这不等同于第二次调用 `Increment`，这会修改 @no__t 的装箱副本中的值。 因此，该程序的输出为：

```console
0
1
1
```

### <a name="nullable-value-type-conversions"></a>可以为 null 的值类型转换

值类型 `T` 可以转换为类型的可以为 null 的类型，@no__t 为-1。 如果要转换的值为 `Nothing`，则从 `T?` 到 `T` 的转换将引发 2 @no__t 异常。 此外，`T?` 会转换为类型 @no__t 如果 @no__t 为-2，则会将内部转换为 @no__t。 如果 `S` 是值类型，则 `T?` 与 `S?` 之间存在以下内部转换：

* 从 `T?` 到 @no__t 的相同分类（收缩或扩大）的转换。

* 从 `T` 到 @no__t 的相同分类（收缩或扩大）的转换。

* 从 `S?` 到 @no__t 的收缩转换。

例如，存在从 `Integer?` 到 `Long?` 的内部扩大转换，因为存在从 `Integer` 到 `Long` 的内部扩大转换：

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

从 `T?` 转换到 `S?` 时，如果 @no__t 的值为 `Nothing`，则 @no__t 的值将为 `Nothing`。 从 `S?` 转换为 `T` 或 `T?` 转换为 `S` 时，如果 `T?` 或 @no__t 5 的值为 `Nothing`，则会引发 @no__t 7 异常。

由于基础类型 `System.Nullable(Of T)` 的行为，如果将可为 null 的值类型 @no__t 为-1，则结果为类型 `T` 的装箱值，而不是类型 @no__t 为的装箱值。 与此相反，当取消装箱到可为 null 的值类型 `T?` 时，该值将由 `System.Nullable(Of T)` 进行包装，`Nothing` 将取消装箱为类型 @no__t 为 null 的 null 值。 例如：

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

此行为的一个副作用是，可为 null 的值类型 `T?` 将显示为实现 @no__t 的所有接口，因为将值类型转换为接口需要对类型进行装箱。 因此，`T?` 可转换为 `T` 可转换为的所有接口。 但是，请务必注意，可以为 null 的值类型 `T?` 实际上不会实现 @no__t 的接口，目的是进行泛型约束检查或反射。 例如：

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

将 `Char` 转换为 @no__t 时，会生成一个字符串，其第一个字符为字符值。 将 `String` 转换为 `Char` 将导致字符为字符串的第一个字符。 将 @no__t 的数组转换为 @no__t 1 将导致字符串，其字符是数组的元素。 如果将 @no__t 0 转换为 @no__t 的数组，则会生成一个字符数组，其元素是字符串的字符。

@No__t-0 和 @no__t @no__t 之间的精确转换，-2，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Boolean`0，1，2，3，反之亦然）在此规范的范围之外，实现依赖于一种情况除外。 字符串转换始终考虑运行时环境的当前区域性。 因此，它们必须在运行时执行。

## <a name="widening-conversions"></a>扩大转换

扩大转换永不溢出，但可能会损失精度。 下面的转换是扩大转换：

__标识/默认转换__

* 从类型到自身。

* 从为 lambda 方法生成的匿名委托类型重新分类为具有相同签名的任何委托类型。

* 从文本 `Nothing` 到类型。

__数值转换__

* 从 `Byte` 到 `UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Decimal`，`Single`，或 @no__t 9。

* 从 `SByte` 到 `Short`，`Integer`，`Long`，`Decimal`，`Single`，或者 `Double`。

* 从 `UShort` 到 `UInteger`，`Integer`，`ULong`，`Long`，`Decimal`，`Single` 或 @no__t 7。

* 从 `Short` 到 `Integer`，`Long`，`Decimal`，`Single` 或 @no__t 5。

* 从 `UInteger` 到 `ULong`，`Long`、`Decimal`、`Single` 或 @no__t 5。

* 从 `Integer` 到 `Long`，`Decimal`，`Single` 或 `Double`。

* 从 `ULong` 到 `Decimal`、`Single` 或 @no__t 为3。

* 从 `Long` 到 `Decimal`，`Single` 或 @no__t 为3。

* 从 `Decimal` 到 @no__t 或 `Double`。

* 从 `Single` 到 `Double`。

* 从文本 `0` 到枚举的类型。 （__注意：__ 从 `0` 到任何枚举类型的转换都是扩大，以简化测试标志。 例如，如果 `Values` 是值为 `One` 的枚举类型，则可以通过口述 `(v And Values.One) = 0` 来测试类型 `Values` 的变量 @no__t。）

* 从枚举类型转换为其基础数值类型，或转换为其基础数值类型的扩大转换为的数值类型。

* 从类型为 `ULong`、`Long`、`UInteger`、`Integer`、`UShort`、`Short`、@no__t 为或 @no__t 到较小的类型的常量表达式，前提是常量表达式的值在目标类型的范围内。 （__注意：__ 从 @no__t 0 或 `Integer` 转换为 `Single`、`ULong` 或 `Long` 转换为 `Single` 或 `Double`，或者 `Decimal` 转换为 `Single` 或 @no__t 9 可能导致精度损失，但绝不会导致数量级损失。 其他扩大数值转换决不会丢失任何信息。）

__引用转换__

* 从引用类型到基类型。

* 从引用类型到接口类型，前提是该类型实现接口或与变体兼容的接口。

* 从接口类型到 `Object`。

* 从接口类型到与变体兼容的接口类型。

* 从委托类型到与变体兼容的委托类型。 （__注意：__ 许多其他引用转换都是由这些规则隐式的。 例如，匿名委托是继承自 @no__t 的引用类型;数组类型是继承自 `System.Array` 的引用类型;匿名类型是继承自 @no__t 的引用类型。）

__匿名委托转换__

* 从为 lambda 方法生成的匿名委托类型重新分类为任何更宽的委托类型。

__数组转换__

* 在数组类型中 `S`，元素类型 `Se` 到数组类型 `T`，元素类型为 `Te`，前提是满足以下所有条件：

  * `S` 和 `T` 仅不同于元素类型。

  * @No__t-0 和 @no__t 均为引用类型，或者是已知为引用类型的类型参数。

  * 从 `Se` 到 @no__t 的扩大引用、数组或类型参数转换。

* 在数组类型中 `S`，其枚举元素类型 `Se` 到数组类型 `T`，元素类型为 `Te`，前提是满足以下所有条件：

  * `S` 和 `T` 仅不同于元素类型。

  * @no__t 为 @no__t 的基础类型。

* 如果满足以下条件之一，则从数组类型 @no__t 第0个，其中枚举元素类型为 `Se`，`System.Collections.Generic.IList(Of Te)`，`IReadOnlyList(Of Te)`，`ICollection(Of Te)`，`IReadOnlyCollection(Of Te)`，和 `IEnumerable(Of Te)`。

  * @No__t-0 和 @no__t 均为引用类型，或者是已知为引用类型的类型参数，并且存在从 `Se` 到 `Te` 的扩大引用、数组或类型参数转换。或

  * @no__t 为 @no__t 的基础类型，则为-1;或

  * `Te` 等于 `Se`

__值类型转换__

* 从值类型到基类型。

* 从值类型到类型实现的接口类型。

__可以为 null 的值类型转换__

* 从类型 `T` 到类型 `T?`。

* 从类型 `T?` 到类型 `S?`，其中存在从类型 `T` 到类型  的扩大转换。

* 从类型 `T` 到类型 `S?`，其中存在从类型 `T` 到类型  的扩大转换。

* 从类型 `T?` 到类型 `T` 实现的接口类型。

__字符串转换__

* 从 `Char` 到 `String`。

* 从 `Char()` 到 `String`。

__类型参数转换__

* 从类型参数到 `Object`。

* 从类型参数到接口类型约束或任何与接口类型约束兼容的接口变体。

* 从类型参数到由类约束实现的接口。

* 从类型参数到与类约束实现的接口类型兼容的接口变体。

* 从类型参数到类约束或类约束的基类型。

* 从类型参数 `T` 到类型参数约束 @no__t 为-1，或 `Tx` 的任何内容都是到的扩大转换。

## <a name="narrowing-conversions"></a>收缩转换

收缩转换是指无法证明始终成功、已知的转换可能会丢失信息的转换，以及跨类型的域之间的转换非常不同于采用收缩表示法。 下面的转换归类为收缩转换：

__布尔值转换__

* 从 `Boolean` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Decimal`，0，或 1。

* From `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Decimal`，`Single`，或者 0 到 1。

__数值转换__

* 从 `Byte` 到 `SByte`。

* 从 `SByte` 到 `Byte`、`UShort`、`UInteger` 或 @no__t 为4。

* 从 `UShort` 到 `Byte`、`SByte` 或 @no__t 为3。

* 从 `Short` 到 `Byte`，`SByte`、`UShort`、`UInteger` 或 @no__t 5。

* 从 `UInteger` 到 `Byte`，`SByte`、`UShort`、`Short` 或 @no__t 5。

* 从 `Integer` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，或者 `ULong`。

* 从 `ULong` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer` 或 @no__t 7。

* 从 `Long` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer` 或 @no__t 7。

* From `Decimal` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，或 `Long`。

* 从 `Single` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，或 @no__t 9。

* 从 `Double` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Decimal` 或 0。

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

* 从数组类型 `S`，元素类型 `Se`）到数组类型 `T`，元素类型为 `Te`，前提是满足以下所有条件：

  * `S` 和 `T` 仅不同于元素类型。
  * @No__t-0 和 @no__t 均为引用类型，或者是不被称为值类型的类型参数。
  * 从 `Se` 到 @no__t 的收缩引用、数组或类型参数转换。

* 在数组类型中 `S`，元素类型 `Se` 到数组类型 `T`，其中枚举元素类型为 `Te`，前提是满足以下所有条件：

  * `S` 和 `T` 仅不同于元素类型。
  * @no__t 为 @no__t 的基础类型，或者它们都是共享同一基础类型的不同枚举类型。

* 如果满足以下条件之一，则从数组类型 `S`，其中枚举元素类型为 `Se`，`IList(Of Te)`，`IReadOnlyList(Of Te)`，`ICollection(Of Te)`，`IReadOnlyCollection(Of Te)`，`IEnumerable(Of Te)`）：

  * @No__t-0 和 @no__t 均为引用类型，或者是已知为引用类型的类型参数，并且存在从 `Se` 到 `Te` 的收缩引用、数组或类型参数转换;或
  * @no__t 为 @no__t 的基础类型，或者它们都是共享同一基础类型的不同枚举类型。

__值类型转换__

* 从引用类型到派生程度更高的值类型。

* 从接口类型到值类型（如果值类型实现接口类型）。

__可以为 null 的值类型转换__

* 从类型 `T?` 到类型 `T`。

* 从类型 `T?` 到类型 `S?`，其中存在从类型 `T` 到类型  的收缩转换。

* 从类型 `T` 到类型 `S?`，其中存在从类型 `T` 到类型  的收缩转换。

* 从类型 `S?` 到类型 `T`，其中存在从类型 `S` 到类型  的转换。

__字符串转换__

* 从 `String` 到 `Char`。

* 从 `String` 到 `Char()`。

* 从 `String` 到 `Boolean` 和从 `Boolean` 到 @no__t。

* @No__t-0 与 @no__t 之间的转换、`SByte`、`UShort`、`Short`、@no__t、@no__t、@no__t、@no__t、@no__t、@no__t、@no__t、或为。

* 从 `String` 到 `Date` 和从 `Date` 到 @no__t。

__类型参数转换__

* 从 `Object` 到类型参数。

* 从类型参数到接口类型，前提是类型参数不受限于该接口或约束为实现该接口的类。

* 从接口类型到类型参数。

* 从类型参数到类约束的派生类型。

* 从类型参数 `T` 到类型参数约束的任何内容 `Tx` 将收缩转换为。

## <a name="type-parameter-conversions"></a>类型参数转换

类型参数的转换由约束（如果有）决定。 类型参数 `T` 始终可以从 `Object` 转换为自身，以及从任何接口类型转换为和。 请注意，如果类型 @no__t 为在运行时的值类型，则从 `T` 转换为 `Object` 或接口类型将为装箱转换，并从 @no__t 3 转换或接口类型转换为 @no__t。 类约束 `C` 的类型参数定义了从类型参数到 `C` 及其基类的其他转换，反之亦然。 类型参数 `T` 与类型参数约束一起使用 `Tx` 定义转换为 `Tx` 和 @no__t 转换为的任何内容。

如果类型参数还具有 @no__t 2 或类约束，则其元素类型为具有接口约束 `I` 的类型形参具有相同的协变数组 @no__t 转换，因为该类型参数还具有2或类约束（因为仅引用数组元素类型可以是协变的。 其元素类型为类型参数 @no__t 为-0 的类型参数与元素类型为 `C` 的数组具有相同的协变数组转换。

上述转换规则不允许将不受约束的类型参数转换为非接口类型，这可能会令人吃惊。 这样做的原因是为了防止混淆此类转换的语义。 例如，请考虑以下声明：

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

如果允许 `T` 到 `Integer` 的转换，则很可能会认为 `X(Of Integer).F(7)` 返回 `7L`。 但是，它不会，因为只有在编译时已知类型为数字的情况下才考虑数值转换。 为了使语义清晰明了，必须改为编写以上示例：

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a>用户定义的转换

*内部转换*是由语言（即在此规范中列出）定义的转换，而*用户定义的转换*是通过重载 @no__t 2 运算符来定义的。 在类型之间进行转换时，如果没有任何内部转换适用，则将考虑用户定义的转换。 如果存在*最*适用于源和目标类型的用户定义的转换，则将使用用户定义的转换。 否则，将产生编译时错误。 最具体的转换是操作数与源类型 "最接近" 的转换，并且其结果类型与目标类型 "最近"。 确定要使用的用户定义的转换时，将使用最特定的扩大转换;如果没有最具体的扩大转换，将使用最具体的收缩转换。 如果没有最具体的收缩转换，则转换是不确定的，并发生编译时错误。

以下部分介绍如何确定最具体的转换。 它们使用以下术语：

如果存在从类型 `A` 到类型 `B` 的内部扩大转换，并且如果 @no__t @no__t 两者都不为接口，则 *@no__t 为 @no__t，`B`* *包含*`A`。

一组类型中包含的*最大*的类型是一种包含集内所有其他类型的类型。 如果没有一种类型包含所有其他类型，则该集没有最大的包含类型。 直观地说，最包含的类型是集中的 "最大" 类型，这种类型是每个其他类型可通过扩大转换进行转换的一种类型。

类型集中*最常被包含*的类型是一种类型，它由集中的所有其他类型所包含。 如果所有其他类型都不包含任何一种类型，则该集不包含大多数被包含的类型。 简而言之，最常被包含的类型是集合中的 "最小" 类型，一种类型可以通过收缩转换转换为其他类型。

收集 @no__t 为类型的候选用户定义的转换时，将改用 @no__t 定义的用户定义的转换运算符。 如果要转换为的类型也是可以为 null 的值类型，则会提升仅涉及不可为 null 的值类型的 @no__t 0 的用户定义转换运算符。 从 `T` 到 `S` 的转换运算符被提升为从 `T?` 到 `S?` 的转换，并通过将 @no__t 转换为 `T` （如有必要），然后将用户定义的转换运算符从 @no__t 6 转换到 `S`，然后如有必要，将 `S` 转换为 `S?`。 但是，如果要转换的值为 `Nothing`，则提升转换运算符会直接转换为类型为 `Nothing` 的值，类型为 `S?`。 例如：

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

一些转换归类为*本机转换*，因为这些转换在 .NET Framework 本机支持。 这些转换是可通过使用 @no__t 0 和 @no__t 转换运算符以及其他特殊行为进行优化的转换。 归类为本机转换的转换包括：标识转换、默认转换、引用转换、数组转换、值类型转换和类型参数转换。

## <a name="dominant-type"></a>主导类型

给定一组类型，通常在某些情况下（如类型推理）确定集的*主导类型*是必需的。 类型集的主导类型是先删除一个或多个其他类型没有隐式转换为的类型。 如果此时没有任何类型，则没有主导类型。 然后，主要类型是其余类型中包含的最大的类型。 如果有多个最常包含的类型，则没有主导类型。
