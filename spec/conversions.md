# <a name="conversions"></a>转换

转换是从一种类型的值更改为另一个的过程。 例如，类型的值`Integer`可转换为类型的值`Double`，或类型的值`Derived`可转换为类型的值`Base`、 假设`Base`和`Derived`类和`Derived`继承自`Base`。 转换可能不需要值本身进行更改 （后者如示例所示），或者它们可能要求 （如前一示例中） 的值表示形式中的重大更改。

转换可能会扩大或收缩。 一个*扩大转换*是从一种类型转换为另一种类型的值域必须是至少一样大，如果不大于，原始类型的值的域。 扩大转换应永远不会失败。 一个*收缩转换*从一种类型转换为另一种类型的值域必须是小于原始类型的值域或足够不相关的额外必须格外小心何时 （适用于中执行转换示例中，当从转换`Integer`到`String`)。 收缩转换，这可能需要将信息丢失，可能会失败。

标识转换 （即从类型转换到其自身） 和默认值的转换 (即从转换`Nothing`) 的所有类型定义。

## <a name="implicit-and-explicit-conversions"></a>隐式转换和显式转换

转换可以是*隐式*或*显式*。 而无需任何特殊语法进行隐式转换。 以下是隐式转换的示例`Integer`值设为`Long`值：

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

显式转换，但是，需要强制转换运算符。 尝试执行操作而无需强制转换运算符的值的显式转换会导致编译时错误。 下面的示例使用的显式转换来转换`Long`值设为`Integer`值。

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

隐式转换集取决于编译环境和`Option Strict`语句。 如果正在使用严格的语义，仅进行扩大转换可能会发生隐式。 如果要使用宽松的语义，所有的扩大转换和收缩转换 （即，所有转换） 可能会发生隐式。

## <a name="boolean-conversions"></a>布尔转换

尽管`Boolean`不是数值类型，但也有收缩转换与数值类型，就好像枚举的类型。 文字`True`将转换为文本`255`为`Byte`，`65535`有关`UShort`，`4294967295`的`UInteger`，`18446744073709551615`为`ULong`，和表达式`-1`的`SByte`， `Short`， `Integer`， `Long`， `Decimal`， `Single`，和`Double`。 文字`False`将转换为文本`0`。 零数字值将转换为文本`False`。 所有其他数字值转换为文本`True`。

没有从布尔值转换为字符串，转换为收缩`System.Boolean.TrueString`或`System.Boolean.FalseString`。 此外，还有从收缩转换`String`到`Boolean`： 如果该字符串等于`TrueString`或`FalseString`(当前区域性中不区分大小写) 然后使用适当的值; 否则它尝试将字符串分析为数值类型 (在十六进制或八进制如果可能，否则为浮点形式)，并使用上述规则;否则将引发`System.InvalidCastException`。

## <a name="numeric-conversions"></a>数值转换

数值转换的类型之间存在`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`，`Single`和`Double`，和所有枚举的类型。 当正在转换时，就好像它们的基础类型将被视为枚举的类型。 转换为枚举类型时，源值不需要遵守的一组值的枚举类型中定义。 例如：

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

按如下所示，将数值转换处理在运行时：

* 从数值类型转换为更广泛的数值类型，值只被转换为更广泛的类型。 从转换`UInteger`， `Integer`， `ULong`， `Long`，或`Decimal`到`Single`或者`Double`舍入为最接近`Single`或`Double`值。 虽然此转换可能会导致精度损失，它将永远不会导致丢失量值。

* 对于从一种整型类型转换到另一个整型类型，或从`Single`， `Double`，或`Decimal`为整型类型，结果取决于整数溢出检查是上：

  *如果要检查整数溢出：*

  * 如果源是一种整型类型，转换会成功，如果源自变量被目标类型的范围内。 转换会引发`System.OverflowException`异常如果源自变量被目标类型的范围之外。

  * 如果源是`Single`， `Double`，或`Decimal`、 向上或向下最接近的整数值，舍入的源值和此整数值将成为转换的结果。 如果源值同样接近两个整数值，则会将值舍入到最低有效位数位置中具有偶数数目的值。 如果生成的整数值超出目标类型的范围`System.OverflowException`引发异常。

  *如果未选中整数溢出：*

  * 如果源是一种整型类型，转换始终成功，并且只包含放弃源值的最高有效位。

  * 如果源是`Single`， `Double`，或`Decimal`，转换始终成功，只包含舍入到最接近的整数值的源值。 如果源值同样接近两个整数值，该值是始终舍入到最低有效位数位置中具有偶数数目的值中。

* 对于从`Double`到`Single`，则`Double`值舍入为最接近`Single`值。 如果`Double`值因过小而无法表示为`Single`，结果将成为正零或负零。 如果`Double`该值太大而无法表示为`Single`，其结果成为正无穷或负无穷大。 如果`Double`值是`NaN`，结果也是`NaN`。

* 对于从`Single`或`Double`到`Decimal`，源值转换为`Decimal`表示形式和舍入为最接近的数字 28 位小数后根据需要。 如果源值就太小，无法表示为`Decimal`，结果则为零。 如果源值为`NaN`，无穷大或太大而无法表示为`Decimal`、`System.OverflowException`引发异常。

* 对于从`Double`到`Single`，则`Double`值舍入为最接近`Single`值。 如果`Double`值因过小而无法表示为`Single`，结果将成为正零或负零。 如果`Double`该值太大而无法表示为`Single`，其结果成为正无穷或负无穷大。 如果`Double`值是`NaN`，结果也是`NaN`。

## <a name="reference-conversions"></a>引用转换

引用类型可能会转换为基类型，反之亦然。 如果要转换的值为 null 值、 派生的类型本身或深入派生的类型从基类型到派生程度更大的类型的转换才成功在运行时。

可以将类和接口类型转换到和从任何接口类型。 一种类型和接口类型之间的转换才成功所涉及的实际类型具有继承或实现关系在运行时。 因为接口类型将始终包含派生类型的实例`Object`，接口类型也始终与转换`Object`。

__请注意。__ 它不是错误转换`NotInheritable`类与接口未实现表示 COM 类的类可能具有接口实现之前不知道是否因为运行时。 

如果在运行时，将引用转换失败`System.InvalidCastException`引发异常。

### <a name="reference-variance-conversions"></a>引用的变体转换

泛型接口或委托可能具有 variant 类型参数，允许兼容的类型的变体之间的转换。 因此，在运行时从类类型或接口类型转换为是变体与它继承自或实现一个接口类型兼容的接口类型将会成功。 同样，委托类型可以转换为，并且从兼容的变体委托类型。 例如，委托类型

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

将允许从转换`F(Of Object, Integer)`到`F(Of String, Integer)`。 它是一个委托`F`此方法采用`Object`可以安全地使用委托`F`此方法采用`String`。 当调用委托时，目标方法将期望获得对象，而字符串是一个对象。

泛型委托或接口类型`S(Of S1,...,Sn)`称为*变体兼容*泛型接口或委托类型与`T(Of T1,...,Tn)`如果：

* `S` 并`T`同时从同一个泛型类型构造`U(Of U1,...,Un)`。

* 每个类型形参`Ux`:

  * 如果类型参数已声明而无需再方差`Sx`和`Tx`必须是同一类型。

  * 如果已声明的类型参数`In`必须有扩大标识、 默认值、 引用、 数组或类型参数转换从`Sx`到`Tx`。

  * 如果已声明的类型参数`Out`必须有扩大标识、 默认值、 引用、 数组或类型参数转换从`Tx`到`Sx`。

到泛型接口使用 variant 类型参数的转换从一个类，如果类实现多个变体兼容接口时转换不明确，如果不存在非变体转换。 例如：

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

表达式分类为 lambda 方法重新分类为值的上下文中时其中不存在目标的类型 (例如， `Dim x = Function(a As Integer, b As Integer) a + b`)，或者其中的目标类型不是委托类型，所生成的表达式的类型是匿名委托类型等效于 lambda 方法的签名。 此匿名委托类型具有到任何兼容的委托类型的转换： 兼容的委托类型是可以使用匿名委托类型的委托创建表达式创建任何委托类型`Invoke`方法作为参数。 例如：

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

请注意，类型`System.Delegate`和`System.MulticastDelegate`无法自行被认为是委托类型 （即使从它们继承所有委托类型）。 另请注意，从匿名委托类型转换为兼容的委托类型不是引用转换。

## <a name="array-conversions"></a>数组转换

数组，因此它们是引用类型定义的转换，除了几个特殊的转换存在数组。

任何两个类型为`A`和`B`，如果它们是引用类型或类型参数不知道为值类型，并且如果`A`具有的引用，则数组或类型参数转换到`B`，数组中的已存在的转换类型`A`类型的数组到`B`使用相同的排名。 这种关系称为*数组协方差*。 数组协方差尤其意味着，其元素类型是数组的元素`B`实际上可能是其元素类型是数组的元素`A`，前提是这两`A`和`B`是引用类型和该`B`具有引用转换或数组转换为`A`。 在下面的示例中，第二次调用`F`会导致`System.ArrayTypeMismatchException`因为实际的元素类型的引发异常`b`是`String`，而不`Object`:

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

数组协方差，由于引用类型数组的元素赋值中包括运行时检查，以确保分配给数组元素的值实际上是允许的类型。

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

在此示例中，分配给`array(i)`方法中`Fill`隐式包括确保该变量所引用的对象的运行时检查`value`是`Nothing`或与兼容的类型的实例数组的实际元素类型`array`。 在方法中`Main`前, 两个方法的调用`Fill`成功，但第三个调用会导致`System.ArrayTypeMismatchException`异常引发时执行的第一个分配到`array(i)`。 发生异常的原因`Integer`不能存储在`String`数组。

如果其中一个数组元素类型是其类型证明是在运行时，值类型的类型参数`System.InvalidCastException`将引发异常。 例如：

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

枚举类型的数组之间还存在转换和枚举类型的数组的基础类型或具有相同的基础类型，另一个枚举类型的数组，前提是数组具有相同的排名。

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

在此示例中，数组`Color`转换到数组中的`Byte`，`Color`的基础类型。 转换为数组`Integer`，但是，将出现错误因为`Integer`的基础类型不是`Color`。

级别 1 类型数组`A()`还具有到集合接口类型的数组转换`IList(Of B)`， `IReadOnlyList(Of B)`， `ICollection(Of B)`，`IReadOnlyCollection(Of B)`并`IEnumerable(Of B)`中找到`System.Collections.Generic`，但前提是以下之一为 true:

- `A` 并`B`是否都引用类型或未知值类型; 的类型参数和`A`扩大引用、 数组或类型参数转换为`B`; 或
- `A` 和`B`是相同的基础类型; 这两种枚举的类型或
- 之一`A`和`B`是一个枚举的类型，另一个是其基础类型。

任何级别的类型的任何数组还具有到非泛型集合接口类型的数组转换`IList`，`ICollection`并`IEnumerable`中找到`System.Collections`。

可以循环访问由此产生的接口使用`For Each`，或通过调用`GetEnumerator`直接方法。 在级别 1 阵列的情况下转换的泛型或非泛型窗体`IList`或`ICollection`，还有可能要按索引获取元素。 对于级别 1 数组转换为泛型或非泛型形式的`IList`，则还可以按索引设置元素，遵循相同的运行时数组协方差检查上文所述。 所有其他接口方法的行为是不明确的 VB 语言规范;负责基础运行时。

## <a name="value-type-conversions"></a>值类型转换

值类型值可以转换为其基引用类型或接口类型，它实现了通过一个称为进程之一*装箱*。 值类型值进行装箱时，值被复制从何处到.NET Framework 堆上的位置。 对堆上的此位置的引用然后返回，并且可以存储在引用类型变量。 此引用也称为*装箱*值类型的实例。 装箱的实例具有相同的语义而不是值类型是引用类型。

装箱的值类型可以转换回其原始值类型通过名为的进程*取消装箱*。 未装箱的装箱的值类型时，值从堆复制到变量位置中。 此后，它的行为就像它是值类型。 取消装箱值类型，值必须为 null 值或值类型的实例。 否则为`System.InvalidCastException`引发异常。 如果值为枚举类型的实例，此值也可以是未装箱为枚举类型的基础类型或另一种枚举的类型具有相同的基础类型。 Null 值被视为文字像`Nothing`。

若要支持可以为 null 值类型，值类型`System.Nullable(Of T)`被视为特殊时执行装箱和取消装箱。 装箱类型的值`Nullable(Of T)`类型的装箱的值会导致`T`如果的值`HasValue`属性是`True`，则值为`Nothing`如果的值`HasValue`属性是`False`。 类型的值取消装箱`T`到`Nullable(Of T)`的实例会导致`Nullable(Of T)`其`Value`属性为装箱的值，它的`HasValue`属性是`True`。 值`Nothing`可以为取消装箱`Nullable(Of T)`任何`T`和它的产生的值`HasValue`属性是`False`。 因为装箱的值类型的行为类似引用类型，它是可以创建多个引用为相同的值。 对于基元类型和枚举的类型，这是不相关因为这些类型的实例*不可变*。 也就是说，不可能需要修改这些类型的装箱的实例，因此不能遵守这一事实有多个引用为相同的值。

结构，但是，可能是可变是否可以访问它的实例变量，或它的方法或属性修改它的实例变量。 如果对已装箱的结构的一个引用用于修改该结构，对已装箱的结构的所有引用将都看到更改。 此结果可能是意外，因为当一个值，类型化为`Object`从一个位置复制到另一个装箱的值类型会自动将克隆而不是只让复制其引用堆。 例如：

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

```
Values: 0, 123
Refs: 123, 123
```

本地变量的字段的赋值`val2`不会影响本地变量的字段`val1`因为时已装箱`Struct1`已分配给`val2`，已创建的值的副本。 与之相反，赋值`ref2.Value = 123`影响的对象，同时`ref1`和`ref2`的引用。

__请注意。__ 复制结构不是装箱结构类型化为`System.ValueType`因为无法将后期绑定的中断`System.ValueType`。

没有已装箱的值类型将在赋值时复制规则的例外。 如果已装箱的值类型引用存储在另一种类型内，将不会复制内部引用。 例如：

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

```
Values: 123, 123
```

这是因为复制值时不会复制内部装箱的值。 因此，两者`val1.Value`和`val2.Value`具有相同的装箱的值类型的引用。

__请注意。__ 这一事实，将不会复制内部装箱的值类型是一个限制的.net 类型系统-若要确保只要类型的值复制所有内部装箱的值类型`Object`复制是成本过高。

为前面所述，装箱值类型只能是未为其原始类型装箱。 装箱的基元类型，但是时, 处理的特殊类型化为`Object`。 它们可以转换为它们都可以转换为任何其他基元类型。 例如：

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

通常情况下，装箱`Integer`值`5`不可能到取消装箱`Byte`变量。 但是，由于`Integer`和`Byte`是基元类型和具有的转换，允许进行转换。

请务必注意，将值类型转换为一个接口是不是泛型参数约束为接口不同。 访问接口成员，受约束的类型参数时 (或上调用方法`Object`)，装箱值类型转换为接口和接口成员访问时的表现不会出现。 例如，假设一个接口`ICounter`包含一个方法，`Increment`这可用于修改值。 如果`ICounter`用作约束的实现`Increment`使用对变量的引用来调用方法的`Increment`对不是装箱副本调用：

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

首次调用`Increment`修改变量中的值`x`。 这并不等同于第二次调用`Increment`，它修改的已装箱副本中的值`x`。 因此，程序的输出为：

```
0
1
1
```

### <a name="nullable-value-type-conversions"></a>可以为 null 值的类型转换

值类型`T`与可以为 null 的类型版本的可以转换`T?`。 从转换`T?`到`T`将引发`System.InvalidOperationException`异常，如果要转换的值为`Nothing`。 此外，`T?`转换为类型`S`如果`T`内部函数转换为`S`。 如果`S`为值类型，则之间存在以下内部函数转换`T?`和`S?`:

* 从同一分类 （缩小或扩大转换） 的转换`T?`到`S?`。

* 从同一分类 （缩小或扩大转换） 的转换`T`到`S?`。

* 收缩转换`S?`到`T`。

例如，内部函数的扩大转换存在从`Integer?`到`Long?`因为存在从内部函数的扩大转换`Integer`到`Long`:

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

从转换时`T?`到`S?`，如果的值`T?`是`Nothing`，然后的值`S?`将`Nothing`。 从转换时`S?`到`T`或`T?`到`S`，如果的值`T?`或`S?`是`Nothing`、`System.InvalidCastException`将引发异常。

由于基础类型的行为`System.Nullable(Of T)`，可以为 null 值类型时`T?`是装箱，结果是类型的装箱的值`T`，不是装箱的值类型的`T?`。 和与之相反，取消装箱的可以为 null 的值类型`T?`，值将由包装`System.Nullable(Of T)`，并`Nothing`将为 null 值类型的未装箱`T?`。 例如：

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

此行为的一个副作用是，可以为 null 的值类型`T?`显示实现的接口的所有`T`，因为将值类型转换为一个接口需要要进行装箱的类型。 因此，`T?`可转换为所有接口的`T`可转换为。 务必要注意，null 值，则键入`T?`实际实现的接口`T`用于泛型约束检查或反射的目的。 例如：

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

转换`Char`到`String`导致其第一个字符是字符值的字符串。 转换`String`到`Char`得到其值是字符串的第一个字符一个字符。 将转换的数组`Char`到`String`导致其字符数组的元素的字符串。 转换`String`成一个数组`Char`导致其元素是字符串的字符的字符数组。

之间的确切转换`String`并`Boolean`， `Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`， `Double`， `Date`，反之亦然，超出了本规范的范围和是实现依赖除了一个详细信息。 字符串转换，应始终考虑在运行时环境的当前区域性。 在这种情况下，它们必须在运行时执行。

## <a name="widening-conversions"></a>扩大转换

扩大转换永远不会溢出，但会导致精度损失。 以下转换扩大转换：

__标识/默认转换__

* 从类型到本身。

* 从匿名委托类型生成到任何具有相同签名的委托类型 lambda 方法重新分类。

* 从文本`Nothing`为类型。

__数值转换__

* 从`Byte`到`UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`。

* 从`SByte`到`Short`， `Integer`， `Long`， `Decimal`， `Single`，或`Double`。

* 从`UShort`到`UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`。

* 从`Short`到`Integer`， `Long`， `Decimal`，`Single`或`Double`。

* 从`UInteger`到`ULong`， `Long`， `Decimal`， `Single`，或`Double`。

* 从`Integer`到`Long`， `Decimal`，`Single`或`Double`。

* 从`ULong`到`Decimal`， `Single`，或`Double`。

* 从`Long`到`Decimal`，`Single`或`Double`。

* 从`Decimal`到`Single`或`Double`。

* 从`Single`到`Double`。

* 从文本`0`对枚举类型。 (__注意。__ 从转换`0`到任何枚举类型正在扩大以简化测试的标志。 例如，如果`Values`是一个枚举的类型使用值`One`，可以测试变量`v`类型的`Values`语`(v And Values.One) = 0`。)

* 从为其基础的数值类型，或为其基础的数值类型没有扩大转换为数值类型的枚举类型。

* 从类型的常量表达式`ULong`， `Long`， `UInteger`， `Integer`， `UShort`， `Short`， `Byte`，或`SByte`到更窄的类型，提供常量表达式的值位于目标类型的范围。 (__注意。__ 从转换`UInteger`或`Integer`到`Single`，`ULong`或`Long`到`Single`或者`Double`，或`Decimal`到`Single`或`Double`可能会导致丢失精度，但将永远不会会导致丢失量值。 其他扩大数值转换永远不会丢失任何信息。）

__引用转换__

* 从引用类型到基类型。

* 从接口类型的引用类型，前提是该类型实现的接口或变体兼容接口。

* 从接口类型到`Object`。

* 从接口类型到变体兼容接口类型。

* 从委托类型到兼容的变体委托类型。 (__注意。__ 这些规则被隐含的许多其他引用转换。 例如，匿名委托是引用类型继承自`System.MulticastDelegate`; 数组类型是引用类型继承自`System.Array`; 匿名类型是引用类型继承自`System.Object`。)

__匿名委托转换__

* 从匿名委托的 lambda 方法重新分类到任何更广的委托类型生成类型。

__数组转换__

* 从数组类型`S`的元素类型与`Se`为数组类型`T`的元素类型与`Te`，提供以下所有项都为 true:

  * `S` 和`T`的区别只在于元素类型。

  * 这两`Se`和`Te`是引用类型或类型参数已知为引用类型。

  * 扩大引用、 数组或类型参数转换存在从`Se`到`Te`。

* 从数组类型`S`枚举的元素类型与`Se`为数组类型`T`的元素类型与`Te`，提供以下所有项都为 true:

  * `S` 和`T`的区别只在于元素类型。

  * `Te` 是的基础类型`Se`。

* 从数组类型`S`秩为 1 的枚举的元素类型的`Se`给`System.Collections.Generic.IList(Of Te)`， `IReadOnlyList(Of Te)`， `ICollection(Of Te)`， `IReadOnlyCollection(Of Te)`，和`IEnumerable(Of Te)`、 提供的以下项之一为 true:

  * 这两`Se`并`Te`是引用类型或类型形参已知的引用类型和扩大的引用，数组，或存在从类型参数转换`Se`到`Te`; 或

  * `Te` 是的基础类型`Se`; 或

  * `Te` 等于 `Se`

__值类型转换__

* 从值类型到基类型。

* 从值类型到该类型实现的接口类型。

__可以为 null 的值类型转换__

* 从类型`T`为类型`T?`。

* 从类型`T?`为某种`S?`，其中不存在从类型扩大转换`T`为类型`S`。

* 从类型`T`为某种`S?`，其中不存在从类型扩大转换`T`为类型`S`。

* 从类型`T?`向接口类型的类型`T`实现。

__字符串转换__

* 从`Char`到`String`。

* 从`Char()`到`String`。

__类型参数转换__

* 从类型参数为`Object`。

* 从接口类型约束或任何接口类型约束与兼容的接口变体的类型参数。

* 从实现接口的类约束的类型参数。

* 从类约束实现的接口与兼容接口变体类型参数。

* 从类约束或类约束的基类型的类型参数。

* 从类型参数`T`到类型参数约束`Tx`，或任何`Tx`已扩大转换为。

## <a name="narrowing-conversions"></a>收缩转换

收缩转换是在类型明显不同值得使用收缩表示法的域之间不能证明始终成功的转换，转换可能会丢失信息，已知的和转换。 以下转换被归类为收缩转换：

__布尔转换__

* 从`Boolean`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`。

* 从`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`到`Boolean`。

__数值转换__

* 从`Byte`到`SByte`。

* 从`SByte`到`Byte`， `UShort`， `UInteger`，或`ULong`。

* 从`UShort`到`Byte`， `SByte`，或`Short`。

* 从`Short`到`Byte`， `SByte`， `UShort`， `UInteger`，或`ULong`。

* 从`UInteger`到`Byte`， `SByte`， `UShort`， `Short`，或`Integer`。

* 从`Integer`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`，或`ULong`。

* 从`ULong`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`，或`Long`。

* 从`Long`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`，或`ULong`。

* 从`Decimal`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`，或`Long`。

* 从`Single`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`，或`Decimal`。

* 从`Double`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`，或`Single`。

* 从对枚举类型的数值类型。

* 从枚举类型为数值类型及其基础数值类型没有为收缩转换。

* 从枚举类型到另一种枚举类型。

__引用转换__

* 从引用类型到派生程度更大的类型。

* 从类类型为接口类型，前提是类类型不实现接口类型或接口类型变体与它兼容。

* 从接口类型到类类型。

* 从接口类型到另一个接口类型，提供那里两种类型之间没有继承关系，并提供它们都不会变体兼容。

__匿名委托转换__

* 从任何更窄的委托类型到 lambda 方法重新分类为生成的匿名委托类型。

__数组转换__

* 从数组类型`S`的元素类型与`Se`，为数组类型`T`的元素类型与`Te`，前提是所有以下都为 true:

  * `S` 和`T`的区别只在于元素类型。
  * 这两`Se`和`Te`是引用类型或类型参数不知道是值类型。
  * 收缩引用、 数组或类型参数转换存在从`Se`到`Te`。

* 从数组类型`S`的元素类型与`Se`为数组类型`T`枚举的元素类型与`Te`，提供以下所有项都为 true:

  * `S` 和`T`的区别只在于元素类型。
  * `Se` 是的基础类型`Te`，或它们就是这两个不同的枚举的类型共享相同的基础类型。

* 从数组类型`S`秩为 1 的枚举的元素类型的`Se`给`IList(Of Te)`， `IReadOnlyList(Of Te)`， `ICollection(Of Te)`，`IReadOnlyCollection(Of Te)`和`IEnumerable(Of Te)`、 提供的以下项之一为 true:

  * 这两`Se`并`Te`是引用类型或类型参数已知为引用类型，并且收缩引用、 数组或类型参数转换存在一个从`Se`到`Te`; 或
  * `Se` 是的基础类型`Te`，或它们就是这两个不同的枚举的类型共享相同的基础类型。

__值类型转换__

* 从引用类型到派生程度更大的值类型。

* 从接口类型到值类型，前提是值类型实现的接口类型。

__可以为 null 的值类型转换__

* 从类型`T?`为某种`T`。

* 从类型`T?`为某种`S?`，其中不存在从类型收缩转换`T`为类型`S`。

* 从类型`T`为某种`S?`，其中不存在从类型收缩转换`T`为类型`S`。

* 从类型`S?`为某种`T`，其中不存在从类型转换`S`为类型`T`。

__字符串转换__

* 从`String`到`Char`。

* 从`String`到`Char()`。

* 从`String`到`Boolean`来回`Boolean`到`String`。

* 之间的转换`String`并`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`。

* 从`String`到`Date`来回`Date`到`String`。

__类型参数转换__

* 从`Object`类型参数。

* 从类型为接口类型，提供类型参数的参数不为该接口约束或约束为实现该接口的类。

* 从接口类型到类型参数。

* 从类约束的派生类型的类型参数。

* 从类型参数`T`为任何类型参数约束`Tx`收缩转换为。

## <a name="type-parameter-conversions"></a>类型参数转换

如果任何，放在其上由该约束，确定类型参数转换。 类型参数`T`始终可以在与转换到其自身， `Object`，以及任何接口类型。 请注意，如果类型`T`是值类型在运行时，将从转换`T`到`Object`或接口类型将装箱转换和从转换`Object`或接口类型到`T`将取消装箱转换。 具有类约束的类型参数`C`定义的类型参数从其他转换`C`及其基类，反之亦然。 类型参数`T`具有类型参数约束`Tx`定义的转换`Tx`以及任何`Tx`将转换为。

其元素类型是接口约束的类型参数的数组`I`具有相同的协变的数组转换为数组的元素类型是`I`，前提是该类型形参也具有`Class`类约束 （或由于仅引用数组元素类型可以是协变）。 其元素类型是类约束的类型参数的数组`C`具有相同的协变的数组转换为数组的元素类型是`C`。

上述的转换规则不允许从受约束的类型参数转换为非接口类型，这可能会感到惊讶。 这样做的原因是为了防止混淆的此类转换语义。 例如，请考虑以下声明：

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

如果转换`T`到`Integer`允许，极有可能认为该`X(Of Integer).F(7)`将返回`7L`。 但是，不这样做，因为已知类型在编译时是数字时，将仅考虑数值转换。 为了使语义清晰、 上面的示例中必须改为编写：

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a>用户定义的转换

*内部函数转换*是由时 （即此规范中列出），语言定义的转换*用户定义的转换*通过重载定义`CType`运算符。 如果没有内部函数转换适用类型之间进行转换时将视为用户定义的转换。 如果不是用户定义转换*最具体*的源和目标类型，则用户定义的转换将使用。 否则，会导致编译时错误。 最具体的转换是其操作数是"最靠近"的源类型，其结果类型是"最靠近"目标类型。 在确定使用何种用户定义的转换时，将使用最具体的扩大转换;如果没有扩大转换是最具体的将使用最具体的收缩转换。 如果没有不最具体收缩转换，然后进行转换，则未定义，并将发生编译时错误。

以下部分介绍如何确定最特定的转换。 它们将使用以下术语：

如果内部函数的扩大转换存在从类型转换`A`为某种`B`，和如果既没有`A`也不`B`是接口，然后`A`是*包含*通过`B`，并`B`*包含* `A`。

*最完备*中的一组类型的类型是包含在集中的所有其他类型的一个类型。 如果没有一个类型包含所有其他类型，然后集都有没有最大的类型。 直观地讲，最大的类型是在 set--一种类型的每种其他类型可以转换成通过扩大转换中的"最大"类型。

*最包含*中的一组类型的类型是在集中的所有其他类型包含的一个类型。 如果没有一个类型被包含所有其他类型，一组具有最不包含类型。 直观地讲，包含程度最大的类型为 set--可以转换为每个通过收缩转换的其他类型的一个类型中的"最小"类型。

收集候选用户定义的转换类型时`T?`，定义的用户定义的转换运算符`T`改为使用。 如果要转换为的类型也可以为 null 的值类型，则任一`T`的用户定义涉及只有不可为 null 的值类型的转换运算符提升。 转换运算符`T`到`S`提升为从转换`T?`到`S?`而通过将转换在评估`T?`到`T`，如有必要，然后评估用户定义的转换从运算符`T`到`S`，然后进行转换`S`到`S?`，如果有必要。 如果要转换的值是`Nothing`，但是，提升的转换运算符将直接转换的值为`Nothing`化为`S?`。 例如：

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

解析的转换，用户定义转换运算符时，始终首选通过提升的转换运算符。 例如：

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

在运行时计算的用户定义的转换可能涉及最多三个步骤：

1. 首先，值的源类型转换为必要时使用的内部函数转换的操作数类型。

2. 然后，调用用户定义的转换。

3. 最后，用户定义转换的结果转换为目标类型使用的内部函数转换，如有必要。

请务必注意，计算的用户定义的转换将永远不会涉及多个用户定义的转换运算符。

### <a name="most-specific-widening-conversion"></a>最具体的扩大转换

使用以下步骤完成确定最特定用户扩大转换之间的运算符两种类型：

1. 首先，收集所有候选转换运算符。 候选转换运算符是所有的用户定义的扩大转换运算符，将源类型和所有用户定义的扩大转换运算符，为目标类型。

2. 然后，从集中移除所有不适用的转换运算符。 转换运算符时，适用于源类型和目标类型的内部扩大转换运算符从源类型到操作数类型并且没有一个内部扩大转换运算符从运算符的结果为目标类型。 如果不有任何适用的转换运算符，则没有最具体扩大转换。

3. 然后，确定适用的转换运算符的最特定的源类型：

   * 如果任何转换运算符将直接从源类型转换，源类型是最具体的源类型。

   * 否则，最具体的源类型是在源类型的转换运算符的组合集包含程度最大的类型。 如果不需要最包含可以找到类型，则会出现不最具体扩大转换。

4. 然后，确定适用的转换运算符最具体的目标类型：

   * 如果转换运算符的任何转换直接到目标类型，目标类型是最具体的目标类型。

   * 否则，最具体的目标类型为目标类型的转换运算符的组合集最大的类型。 如果找不到任何最大的类型，则没有最具体扩大转换。

5. 然后，如果只有一个转换运算符将从最具体的源类型转换为最具体的目标类型，则这是最具体的转换运算符。 如果存在多个此类运算符，则没有最具体的扩大转换。

### <a name="most-specific-narrowing-conversion"></a>最具体的收缩转换

使用以下步骤完成确定最特定用户定义的收缩转换之间的运算符两种类型：

1. 首先，收集所有候选转换运算符。 候选转换运算符是所有的源类型中的用户定义的转换运算符和所有目标类型中的用户定义的转换运算符。

2. 然后，从集中移除所有不适用的转换运算符。 转换运算符时，适用于源类型和目标类型的源类型的操作数类型到一个内部函数转换运算符，并且没有从运算符的结果为目标类型的内部函数转换运算符。 如果不有任何适用的转换运算符，则没有最具体的收缩转换。

3. 然后，确定适用的转换运算符的最特定的源类型：

   * 如果任何转换运算符将直接从源类型转换，源类型是最具体的源类型。

   * 否则，如果转换运算符的任何转换中包含的源类型的类型，最具体的源类型是中的源类型的这些转换运算符的组合集包含程度最大的类型。 如果不需要最包含可以找到类型，则没有最具体的收缩转换。

   * 否则，最具体的源类型是在源类型的转换运算符的组合集最大的类型。 如果找不到任何最大的类型，则没有最具体的收缩转换。

4. 然后，确定适用的转换运算符最具体的目标类型：

   * 如果转换运算符的任何转换直接到目标类型，目标类型是最具体的目标类型。

   * 否则，如果任何转换运算符将转换为目标类型包含的类型，最具体的目标类型是在源类型的这些转换运算符的组合集最大的类型。 如果找不到任何最大的类型，则没有最具体的收缩转换。

   * 否则，最具体的目标类型是在目标类型的转换运算符的组合集包含程度最大的类型。 如果不需要最包含可以找到类型，则没有最具体的收缩转换。

5. 然后，如果只有一个转换运算符将从最具体的源类型转换为最具体的目标类型，则这是最具体的转换运算符。 如果存在多个此类运算符，则没有最具体的收缩转换。

## <a name="native-conversions"></a>本机转换

多个转换都属于*本机转换*因为本机支持的.NET Framework。 这些转换都是可以使用优化的那些`DirectCast`和`TryCast`转换运算符，以及其他特殊行为。 归类为本机转换的转换： 标识转换、 默认转换、 引用转换、 数组转换、 值类型转换和类型的参数转换。

## <a name="dominant-type"></a>基准类型

给定的一组类型，通常很有必要情况下，例如类型推断来确定在*基准类型*的集。 类型的一组基准类型确定通过先删除一个或多个其他类型不具有隐式转换为任何类型。 如果不存在类型左此时，则没有基准类型。 最多包含剩余类型的则基准类型。 如果存在多个类型最包含，则没有基准类型。
