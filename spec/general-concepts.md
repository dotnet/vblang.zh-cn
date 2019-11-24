---
ms.openlocfilehash: 8599ffc0ad3313f4e7e42da220c22ffa7a37e56b
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306180"
---
# <a name="general-concepts"></a>一般概念

本章介绍了一些概念，这些概念需要了解 Microsoft Visual Basic 语言的语义。 Visual Basic 程序员或 C/C++程序员应熟悉许多概念，但它们的精确定义可能不同。

## <a name="declarations"></a>声明

Visual Basic 程序由命名实体组成。 这些实体是通过*声明*引入的，表示程序的 "含义"。

在顶级，*命名空间*是组织其他实体（如嵌套命名空间和类型）的实体。 *类型*是描述值和定义可执行代码的实体。 类型可能包含嵌套类型和类型成员。 *类型成员*包括常量、变量、方法、运算符、属性、事件、枚举值和构造函数。

可以包含其他实体的实体定义*声明空间*。 实体通过声明或继承引入声明空间;包含声明空间称为实体的*声明上下文*。 在声明空间中声明实体反过来会定义新的声明空间，该空间可以包含进一步嵌套的实体声明;因此，程序中的声明形成了声明空间的层次结构。

除重载类型成员的情况外，声明将相同类型的同名实体引入同一声明上下文中是无效的。 此外，声明空间不能包含具有相同名称的不同类型的实体;例如，声明空间不能包含变量和具有相同名称的方法。

__纪录.__ 其他语言可能会创建一个声明空间，其中包含具有相同名称的不同类型的实体（例如，如果该语言区分大小写，并且允许基于大小写的不同声明）。 在这种情况下，可访问的实体被视为绑定到该名称;如果有多种类型的实体可访问，则名称不明确。 `Public` 比 `Protected Friend`更易于访问，`Protected Friend` 比 `Protected` 或 `Friend`更易于访问，并且 `Protected` 或 `Friend` 比 `Private`更易于访问。

命名空间的声明空间为 "open 结束"，因此两个具有相同完全限定名称的命名空间声明会导致相同的声明空间。 在下面的示例中，两个命名空间声明构成相同的声明空间，在本例中，将两个具有完全限定名称的类声明 `Data.Customer` 和 `Data.Order:`

```vb
Namespace Data
    Class Customer
    End Class
End Namespace

Namespace Data
    Class Order
    End Class
End Namespace
```

由于这两个声明涉及到相同的声明空间，因此，如果每个声明都包含具有相同名称的类的声明，则会发生编译时错误。

### <a name="overloading-and-signatures"></a>重载和签名

在声明空间中声明同一种类的名称相同的实体的唯一方法是使用*重载*。 只能重载方法、运算符、实例构造函数和属性。

重载类型成员必须具有唯一的签名。 类型成员的签名包含类型参数的数目以及该成员的参数的数量和类型。 转换运算符还包括签名中运算符的返回类型。

以下内容不是成员签名的一部分，因此无法重载：

* 对类型成员的修饰符（例如 `Shared` 或 `Private`）。

* 参数的修饰符（例如 `ByVal` 或 `ByRef`）。

* 参数的名称。

* 方法或运算符（转换运算符除外）或属性的元素类型的返回类型。

* 类型参数上的约束。

下面的示例演示一组重载方法声明及其签名。 此声明无效，因为若干方法声明具有相同的签名。

```vb
Interface ITest
    Sub F1()                              ' Signature is ().
    Sub F2(x As Integer)                  ' Signature is (Integer).
    Sub F3(ByRef x As Integer)            ' Signature is (Integer).
    Sub F4(x As Integer, y As Integer)    ' Signature is (Integer, Integer).
    Function F5(s As String) As Integer   ' Signature is (String).
    Function F6(x As Integer) As Integer  ' Signature is (Integer).
    Sub F7(a() As String)                 ' Signature is (String()).
    Sub F8(ParamArray a() As String)      ' Signature is (String()).
    Sub F9(Of T)()                        ' Signature is !1().
    Sub F10(Of T, U)(x As T, y As U)      ' Signature is !2(!1, !2)
    Sub F11(Of U, T)(x As T, y As U)      ' Signature is !2(!2, !1)
    Sub F12(Of T)(x As T)                 ' Signature is !1(!1)
    Sub F13(Of T As IDisposable)(x As T)  ' Signature is !1(!1)
End Interface
```

定义泛型类型是有效的，该类型可能包含基于提供的类型参数的签名。 重载决策规则用于在这种重载之间进行重区分，但在某些情况下可能无法消除歧义。 例如：

```vb
Class C(Of T)
    Sub F(x As Integer)
    End Sub

    Sub F(x As T)
    End Sub

    Sub G(Of U)(x As T, y As U)
    End Sub

    Sub G(Of U)(x As U, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New C(Of Integer)
        x.F(10)                   ' Calls C(Of T).F(Integer)
        x.G(Of Integer)(10, 10)    ' Error: Can't choose between overloads
    End Sub
End Module
```

## <a name="scope"></a>范围

实体名称的*作用域*是所有声明空间的集合，在其中可以引用该名称，而无需进行限定。 通常情况下，实体名称的范围是它的整个声明上下文;但是，实体的声明可能包含具有相同名称的实体的嵌套声明。 在这种情况下，嵌套*实体隐藏或隐藏、外部*实体，并且只有通过限定才能访问隐藏的实体。

在嵌套于命名空间中的命名空间或类型、嵌套在其他类型中的类型和成员的主体中，将发生隐藏。 通过声明嵌套的隐藏操作始终隐式进行。不需要显式语法。

在下面的示例中，在 `F` 方法中，实例变量 `i` 由局部变量 `i`隐藏，但在 `G` 方法中，`i` 仍引用实例变量。

```vb
Class Test
    Private i As Integer = 0

    Sub F()
        Dim i As Integer = 1
    End Sub

    Sub G()
        i = 1
    End Sub
End Class
```

当内部范围中的名称隐藏外部范围内的名称时，它将隐藏该名称的所有重载匹配项。 在下面的示例中，调用 `F(1)` 调用在 `Inner` 中声明的 `F`，因为内部声明隐藏了 `F` 的所有外部匹配项。 由于同样的原因，调用 `F("Hello")` 错误。

```vb
Class Outer
    Shared Sub F(i As Integer)
    End Sub

    Shared Sub F(s As String)
    End Sub

    Class Inner
        Shared Sub F(l As Long)
        End Sub

        Sub G()
            F(1) ' Invokes Outer.Inner.F.
            F("Hello") ' Error.
        End Sub
    End Class
End Class
```

## <a name="inheritance"></a>继承

继承关系是指一个类型（*派生*类型）派生自另一个类型（*基*类型），因此派生类型的声明空间隐式包含可访问的非构造函数类型成员和其基类型的嵌套类型。 在下面的示例中，类 `A` 是 `B`的基类，`B` 派生自 `A`。

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

由于 `A` 未显式指定基类，因此它的基类隐式 `Object`。

下面是继承的重要方面：

* 继承是可传递的。 如果类型*c*派生自类型*b*，而类型*b*派生自类型*a*，则类型*c*继承在类型*B*中声明的类型成员以及在类型*a*中声明的类型成员。

* 派生类型扩展但不能缩小其基类型。 派生类型可以添加新的类型成员，并且它可以隐藏继承的类型成员，但不能删除继承的类型成员的定义。

* 因为类型的实例包含其基类型的所有类型成员，所以始终存在从派生类型到其基类型的转换。

* 除 `Object`类型外，所有类型都必须有基类型。 因此，`Object` 是所有类型的最终基本类型，所有类型都可以转换为。

* 不允许派生循环。 也就是说，当类型 `B` 从类型 `A`派生时，类型 `A` 是直接或间接从类型 `B`派生的错误。

* 类型不能直接或间接从嵌套在它内部的类型派生。

下面的示例将产生编译时错误，因为这些类会循环依赖。

```vb
Class A
    Inherits B
End Class

Class B
    Inherits C
End Class

Class C
    Inherits A
End Class
```

下面的示例还会产生编译时错误，因为 `B` 从其嵌套类 `C` 通过类 `A`间接派生。

```vb
Class A
    Inherits B.C
End Class

Class B
    Inherits A

    Public Class C
    End Class 
End Class
```

下一个示例不会产生错误，因为类 `A` 不从类 `B`派生。

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a>MustInherit 和 NotInheritable 类

`MustInherit` 类是不完整的类型，它只能作为基类型。 不能对 `MustInherit` 类进行实例化，因此在一个上使用 `New` 运算符是错误的。 声明 `MustInherit` 类的变量是有效的;此类变量只能赋值 `Nothing` 或者是派生自 `MustInherit` 类的类的值。

当正则类派生自 `MustInherit` 类时，正则类必须重写所有继承的 `MustOverride` 成员。 例如：

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

MustInherit Class B
    Inherits A

    Public Sub G()
    End Sub
End Class 

Class C
    Inherits B

    Public Overrides Sub F()
    End Sub 
End Class
```

`MustInherit` 类 `A` 引入 `MustOverride` 方法 `F`。 类 `B` 引入了附加方法 `G`，但未提供 `F`的实现。 因此，必须 `MustInherit`声明类 `B`。 类 `C` 重写 `F` 并提供实际实现。 由于类 `C`中没有未完成的 `MustOverride` 成员，因此不需要 `MustInherit`。

`NotInheritable` 类是一个类，该类不能从中派生其他类。 `NotInheritable` 类主要用于防止无意的派生。

在此示例中，类 `B` 出错，因为它尝试从 `NotInheritable` 类 `A`派生。 不能将类标记 `MustInherit` 和 `NotInheritable`。

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a>接口和多重继承

与仅从单个基类型派生的其他类型不同，接口可能派生自多个基接口。 因此，接口可以从不同的基接口继承名称相同的类型成员。 在这种情况下，交叉继承名称在派生接口中不可用，并且通过派生接口引用这些类型成员中的任何成员都将导致编译时错误，而不考虑签名或重载。 相反，必须通过基接口名称来引用冲突的类型成员。

在下面的示例中，前两个语句会导致编译时错误，这是因为在接口 `IListCounter`中不提供多重继承成员 `Count`：

```vb
Interface IList
    Property Count() As Integer
End Interface

Interface ICounter
    Sub Count(i As Integer)
End Interface

Interface IListCounter
    Inherits IList
    Inherits ICounter 
End Interface 

Module Test
    Sub F(x As IListCounter)
        x.Count(1)                  ' Error, Count is not available.
        x.Count = 1                 ' Error, Count is not available.
        CType(x, IList).Count = 1   ' Ok, invokes IList.Count.
        CType(x, ICounter).Count(1) ' Ok, invokes ICounter.Count.
    End Sub 
End Module
```

如示例所示，通过将 `x` 强制转换为相应的基接口类型来解决歧义。 此类强制转换没有运行时成本;它们只是在编译时将该实例视为不太派生的类型。

如果通过多个路径从相同的基接口继承单个类型成员，则会将类型成员视为仅继承一次。 换言之，派生接口仅包含从特定基接口继承的每个类型成员的一个实例。 例如：

```vb
Interface IBase
    Sub F(i As Integer)
End Interface

Interface ILeft
    Inherits IBase
End Interface

Interface IRight
    Inherits IBase
End Interface

Interface IDerived
    Inherits ILeft, IRight
End Interface

Class Derived
    Implements IDerived

    ' Only have to implement F once.
    Sub F(i As Integer) Implements IDerived.F
    End Sub
End Class
```

如果某个类型成员名称在通过继承层次结构的一个路径中被隐藏，则该名称将隐藏在所有路径中。 在下面的示例中，`IBase.F` 成员由 `ILeft.F` 成员隐藏，但没有在 `IRight`中隐藏：

```vb
Interface IBase
    Sub F(i As Integer)
End Interface 

Interface ILeft
    Inherits IBase

    Shadows Sub F(i As Integer)
End Interface 

Interface IRight
    Inherits IBase

    Sub G()
End Interface 

Interface IDerived
    Inherits ILeft, IRight 
End Interface 

Class Test
    Sub H(d As IDerived)
        d.F(1)                  ' Invokes ILeft.F.
        CType(d, IBase).F(1)    ' Invokes IBase.F.
        CType(d, ILeft).F(1)    ' Invokes ILeft.F.
        CType(d, IRight).F(1)   ' Invokes IBase.F.
    End Sub 
End Class
```

调用 `d.F(1)` 将选择 `ILeft.F`，即使 `IBase.F` 在通过 `IRight`导致的访问路径中不会被隐藏。 由于从 `IDerived` 到 `ILeft` 的访问路径 `IBase` 阴影 `IBase.F`，因此，从 `IDerived` 到 `IRight` 到 `IBase`的访问路径中还会隐藏该成员。

### <a name="shadowing"></a>阴影操作

派生类型通过重新声明继承的类型成员的名称来隐藏该成员的名称。 隐藏名称不会删除使用该名称继承的类型成员;它只是使该名称的所有继承的类型成员在派生类中不可用。 隐藏声明可以是任何类型的实体。

可以重载的实体可以选择两种形式的隐藏之一。 使用 `Shadows` 关键字指定*按名称隐藏*。 按名称隐藏的实体隐藏了基类中该名称的所有内容，包括所有重载。 *按名称和签名进行隐藏*是使用 `Overloads` 关键字指定的。 按名称和签名隐藏的实体将使用与实体相同的签名隐藏该名称的所有内容。 例如：

```vb
Class Base
    Sub F()
    End Sub

    Sub F(i As Integer)
    End Sub

    Sub G()
    End Sub

    Sub G(i As Integer)
    End Sub
End Class

Class Derived
    Inherits Base

    ' Only hides F(Integer).
    Overloads Sub F(i As Integer)
    End Sub

    ' Hides G() and G(Integer).
    Shadows Sub G(i As Integer)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New Derived()

        x.F() ' Calls Base.F().
        x.G() ' Error: Missing parameter.
    End Sub
End Module
```

使用按名称和签名 `ParamArray` 参数隐藏方法仅隐藏单个签名，而不会隐藏所有可能的已展开签名。 即使隐藏方法的签名与隐藏方法的未展开签名匹配，也是如此。 如下示例中：

```vb
Class Base
    Sub F(ParamArray x() As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived 
    Inherits Base

    Overloads Sub F(x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

即使 `Derived.F` 与 `Base.F`的未展开形式具有相同的签名，也会打印 `Base`。

相反，带有 `ParamArray` 参数的方法只隐藏具有相同签名的方法，而不是所有可能的展开签名。 如下示例中：

```vb
Class Base
    Sub F(x As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived
    Inherits Base

    Overloads Sub F(ParamArray x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

即使 `Derived.F` 具有与 `Base.F`具有相同签名的扩展窗体，也会打印 `Base`。

如果 `Overloads` 方法或属性 `Overrides`，则不指定 `Shadows` 或的隐藏方法或属性将假定 `Overloads` `Shadows`，否则为。 如果一组重载实体中的一个成员指定 `Shadows` 或 `Overloads` 关键字，则它们都必须指定。 不能同时指定 `Shadows` 和 `Overloads` 关键字。 不能在标准模块中指定 `Shadows` 和 `Overloads`;标准模块中的成员隐式隐藏从 `Object`继承的成员。

这是一个有效的方法是，通过接口继承（因而不可用）隐藏已被乘法除继承的类型成员的名称，从而使该名称在派生接口中可用。

例如：

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight

    Shadows Sub F()
End Interface

Module Test
    Sub G(i As ILeftRight)
        i.F() ' Calls ILeftRight.F.
        CType(i, ILeft).F() ' Calls ILeft.F.
        CType(i, IRight).F() ' Calls IRight.F.
    End Sub
End Module
```

由于允许方法隐藏继承方法，因此类可能包含多个具有相同签名的 `Overridable` 方法。 这并不会带来多义性问题，因为只有最常派生的方法是可见的。 在下面的示例中，`C` 和 `D` 类包含两个具有相同签名的 `Overridable` 方法：

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub 
End Class 

Class B
    Inherits A

    Public Overrides Sub F()
        Console.WriteLine("B.F")
    End Sub 
End Class 

Class C
    Inherits B

    Public Shadows Overridable Sub F()
        Console.WriteLine("C.F")
    End Sub 
End Class 

Class D
    Inherits C

    Public Overrides Sub F()
        Console.WriteLine("D.F")
    End Sub 
End Class 

Module Test
    Sub Main()
        Dim d As New D()
        Dim a As A = d
        Dim b As B = d
        Dim c As C = d
        a.F()
        b.F()
        c.F()
        d.F()
    End Sub 
End Module
```

这里有两种 `Overridable` 方法：一个由类 `A` 引入，另一个由类 `C`引入。 类引入的方法 `C` 隐藏从类 `A`继承的方法。 因此，类 `D` 中的 `Overrides` 声明会重写由类 `C`引入的方法，而类 `D` 不可能重写类 `A`引入的方法。 该示例生成以下输出：

```console
B.F
B.F
D.F
D.F
```

可以通过以下方式调用隐藏的 `Overridable` 方法：通过不隐藏该方法的不太派生类型访问 `D` 类的实例。

隐藏 `MustOverride` 方法是无效的，因为在大多数情况下，这会使类不可用。 例如：

```vb
MustInherit Class Base
    Public MustOverride Sub F()
End Class

MustInherit Class Derived
    Inherits Base

    Public Shadows Sub F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    ' Error: MustOverride method Base.F is not overridden.
End Class
```

在这种情况下，需要类 `MoreDerived` `Base.F`重写 `MustOverride` 方法，但由于类 `Derived` 阴影 `Base.F`，因此这是不可能的。 无法声明 `Derived`的有效子代。

与隐藏外部作用域中的名称不同，从继承的作用域中隐藏可访问的名称会导致报告警告，如以下示例中所示：

```vb
Class Base
    Public Sub F()
    End Sub

    Private Sub G()
    End Sub 
End Class

Class Derived
    Inherits Base

    Public Sub F() ' Warning: shadowing an inherited name.
    End Sub

    Public Sub G() ' No warning, Base.G is not accessible here.
    End Sub
End Class
```

类 `Derived` 中 `F` 的声明会导致报告警告。 隐藏继承名称并不是一个错误，因为这会阻止基类的单独演化。 例如，由于类的更高版本 `Base` 引入了在类的早期版本中不存在的方法 `F`，因此可能会出现上述情况。 如果上述情况是错误的，则对单独的版本控制类库中的基类进行的*任何*更改都可能导致派生类无效。

隐藏继承名称导致的警告可以通过使用 `Shadows` 或 `Overloads` 修饰符来消除：

```vb
Class Base
    Public Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Public Shadows Sub F() 'OK.
    End Sub
End Class
```

`Shadows` 修饰符指示隐藏继承成员的意图。 如果没有要隐藏的类型成员名称，则指定 `Shadows` 或 `Overloads` 修饰符是错误的。

新成员的声明仅在新成员的范围内隐藏继承的成员，如以下示例中所示：

```vb
Class Base
    Public Shared Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Private Shared Shadows Sub F() ' Shadows Base.F in class Derived only.
    End Sub 
End Class 

Class MoreDerived
    Inherits Derived

    Shared Sub G()
        F() ' Invokes Base.F.
    End Sub 
End Class
```

在上面的示例中，类 `Derived` 中 `F` 的方法的声明隐藏了继承自类 `Base`的方法 `F`，但由于类 `F` 中的新方法 `Derived` 具有 `Private` 访问权限，因此它的作用域不会扩展到类 `MoreDerived`。 因此，`MoreDerived.G` 中 `F()` 调用有效，并将调用 `Base.F`。 在重载类型成员的情况下，会将整个重载类型成员集视为它们都具有用于隐藏的最宽松的访问权限。

```vb
Class Base
    Public Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F()
    End Sub

    Public Shadows Sub F(i As Integer)
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Sub G()
        F()   ' Error. No accessible member with this signature.
    End Sub
End Class
```

在此示例中，即使用 `Private` 访问声明了 `Derived` 中 `F()` 的声明，使用 `Public` 访问声明重载的 `F(Integer)`。 因此，为了进行隐藏，`Derived` 中的名称 `F` 被视为 `Public`，因此两个方法都在 `Base`中隐藏 `F`。

## <a name="implementation"></a>实现

当某个类型声明它实现接口并且该类型实现了该接口的所有类型成员时，就存在*实现*关系。 实现特定接口的类型可转换为该接口。 接口不能进行实例化，但声明接口的变量是有效的;此类变量只能分配给实现接口的类的值。 例如：

```vb
Interface ITestable
    Function Test(value As Byte) As Boolean
End Interface

Class TestableClass
    Implements ITestable

    Function Test(value As Byte) As Boolean Implements ITestable.Test
        Return value > 128
    End Function
End Class

Module Test
    Sub F()
        Dim x As ITestable = New TestableClass
        Dim b As Boolean

        b = x.Test(34)
    End Sub
End Module
```

实现带有交叉继承类型成员的接口的类型仍必须实现这些方法，即使无法直接从正在实现的派生接口中访问它们。 例如：

```vb
Interface ILeft
    Sub Test()
End Interface

Interface IRight
    Sub Test()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight
End Interface

Class LeftRight
    Implements ILeftRight

    ' Has to reference ILeft explicitly.
    Sub TestLeft() Implements ILeft.Test
    End Sub

    ' Has to reference IRight explicitly.
    Sub TestRight() Implements IRight.Test
    End Sub

    ' Error: Test is not available in ILeftRight.
    Sub TestLeftRight() Implements ILeftRight.Test
    End Sub
End Class
```

即使 `MustInherit` 类也必须提供实现的接口的所有成员的实现;但是，它们可以通过将它们声明为 `MustOverride`来推迟这些方法的实现。 例如：

```vb
Interface ITest
    Sub Test1()
    Sub Test2()
End Interface

MustInherit Class TestBase
    Implements ITest

    ' Provides an implementation.
    Sub Test1() Implements ITest.Test1
    End Sub

    ' Defers implementation.
    MustOverride Sub Test2() Implements ITest.Test2
End Class

Class TestDerived
    Inherits TestBase

    ' Have to implement MustOverride method.
    Overrides Sub Test2()
    End Sub
End Class
```

类型可以选择重新实现其基类型实现的接口。 若要重新实现接口，类型必须显式声明它实现接口。 类型重新实现接口可能会选择仅重新实现接口的部分（而不是全部）成员--任何未重新实现的成员都继续使用基类型的实现。 例如：

```vb
Class TestBase
    Implements ITest

    Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class TestDerived
    Inherits TestBase
    Implements ITest  ' Required to re-implement

    Sub DerivedTest1() Implements ITest.Test1
        Console.WriteLine("TestDerived.DerivedTest1")
    End Sub
End Class

Module Test
    Sub Main()
        Dim Test As ITest = New TestDerived()
        Test.Test1()
        Test.Test2()
    End Sub
End Module
```

此示例将打印：

```console
TestDerived.DerivedTest1
TestBase.Test2
```

当派生类型实现了一个接口，该接口的基接口由派生的类型的基类型实现时，派生的类型可选择仅实现基类型尚未实现的接口的类型成员。 例如：

```vb
Interface IBase
    Sub Base()
End Interface

Interface IDerived
    Inherits IBase

    Sub Derived()
End Interface

Class Base
    Implements IBase

    Public Sub Base() Implements IBase.Base
    End Sub
End Class

Class Derived
    Inherits Base
    Implements IDerived

    ' Required: IDerived.Derived not implemented by Base.
    Public Sub Derived() Implements IDerived.Derived
    End Sub
End Class
```

接口方法还可以使用基类型中的可重写方法实现。 在这种情况下，派生类型还可以重写可重写的方法，并更改接口的实现。 例如：

```vb
Class Base
    Implements ITest

    Public Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Public Overridable Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class Derived
    Inherits Base

    ' Overrides base implementation.
    Public Overrides Sub Test2()
        Console.WriteLine("TestDerived.Test2")
    End Sub
End Class
```

### <a name="implementing-methods"></a>实现方法

类型通过向 `Implements` 子句提供方法来*实现*已实现接口的类型成员。 这两个类型成员必须具有相同数量的参数，参数的所有类型和修饰符都必须匹配，包括可选参数的默认值、返回类型必须匹配以及方法参数的所有约束必须匹配。 例如：

```vb
Interface ITest
    Sub F(ByRef x As Integer)
    Sub G(Optional y As Integer = 20)
    Sub H(Paramarray z() As Integer)
End Interface

Class Test
    Implements ITest

    ' Error: ByRef/ByVal mismatch.
    Sub F(x As Integer) Implements ITest.F
    End Sub

    ' Error: Defaults do not match.
    Sub G(Optional y As Integer = 10) Implements ITest.G
    End Sub

    ' Error: Paramarray does not match.
    Sub H(z() As Integer) Implements ITest.H
    End Sub
End Class
```

如果一个方法满足上述条件，则可以实现任意数量的接口类型成员。 例如：

```vb
Interface ITest
    Sub F(i As Integer)
    Sub G(i As Integer)
End Interface

Class Test

    Implements ITest

    Sub F(i As Integer) Implements ITest.F, ITest.G
    End Sub
End Class
```

在泛型接口中实现方法时，实现方法必须提供与接口的类型参数相对应的类型参数。 例如：

```vb
Interface I1(Of U, V) 
    Sub M(x As U, y As List(Of V)) 
End Interface

Class C1(Of W, X)
    Implements I1(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Sub M(x As W, y As List(Of X)) Implements I1(Of W, X).M
    End Sub 
End Class

Class C2
    Implements I1(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Sub M(x As String, y As List(Of Integer)) _
        Implements I1(Of String, Integer).M
    End Sub
End Class
```

请注意，可能不会为某些类型参数集实施泛型接口。

```vb
Interface I1(Of T, U)
    Sub S1(x As T)
    Sub S1(y As U)
End Interface

Class C1
    ' Unable to implement because I1.S1 has two identical signatures
    Implements I1(Of Integer, Integer)
End Class
```

## <a name="polymorphism"></a>多态性

*多态性*提供了不同的方法或属性实现的功能。 对于多态性，相同的方法或属性可以执行不同的操作，具体取决于调用该方法的实例的运行时类型。 作为多态的方法或属性称为可*重写*。 与此相反，无法重写的方法或属性的实现是固定的;无论方法或属性是在声明它的类的实例上调用的还是派生类的实例，实现都是相同的。 调用不可重写的方法或属性时，实例的编译时类型是确定因素。 例如：

```vb
Class Base
    Public Overridable Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Class Derived
    Inherits Base

    Public Overrides Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Module Test
    Sub F()
        Dim Z As Base

        Z = New Base()
        Z.X = 10            ' Calls Base.X
        Z = New Derived()
        Z.X = 10            ' Calls Derived.X
    End Sub
End Module
```

可重写的方法也可能 `MustOverride`，这意味着它不提供方法体，因此必须重写。 仅 `MustInherit` 类中允许 `MustOverride` 方法。

在下面的示例中，类 `Shape` 定义可自行绘制的几何形状对象的抽象概念：

```vb
MustInherit Public Class Shape
    Public MustOverride Sub Paint(g As Graphics, r As Rectangle)
End Class 

Public Class Ellipse
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawEllipse(r)
    End Sub 
End Class 

Public Class Box
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawRect(r)
    End Sub 
End Class
```

`MustOverride` `Paint` 方法，因为没有有意义的默认实现。 `Ellipse` 和 `Box` 类是具体的 `Shape` 实现。 由于不 `MustInherit`这些类，因此它们需要重写 `Paint` 方法并提供实际实现。

引用 `MustOverride` 方法的基本访问是错误的，如下面的示例所示：

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

Class B
    Inherits A

    Public Overrides Sub F()
        MyBase.F() ' Error, MyBase.F is MustOverride.
    End Sub 
End Class
```

为 `MyBase.F()` 调用报告了错误，因为它引用 `MustOverride` 方法。

### <a name="overriding-methods"></a>重写方法

类型可以*重写*继承的可重写方法，方法是声明具有相同名称和签名的方法，并使用 `Overrides` 修饰符标记声明。下面列出了重写方法的其他要求。 `Overridable` 方法声明引入了一个新方法，而 `Overrides` 方法声明会替换该方法的继承实现。

重写方法可以 `NotOverridable`声明，这会阻止派生类型中的方法的任何进一步重写。 实际上，在任何更进一步的派生类中，`NotOverridable` 方法都将变为不可重写。

请看下面的示例：

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("A.G")
    End Sub
End Class

Class B
    Inherits A

    Public Overrides NotOverridable Sub F()
        Console.WriteLine("B.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class

Class C
    Inherits B

    Public Overrides Sub G()
        Console.WriteLine("C.G")
    End Sub
End Class
```

在此示例中，类 `B` 提供了两种 `Overrides` 方法：具有 `NotOverridable` 修饰符的方法 `F` 和不包含方法 `G` 的方法。 使用 `NotOverridable` 修饰符可防止类 `C` 其他重写方法 `F`。

重写方法还可以 `MustOverride`声明，即使未 `MustOverride`声明其重写的方法。 这要求 `MustInherit` 中声明包含类，并且任何未声明 `MustInherit` 的其他派生类必须重写方法。 例如：

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

MustInherit Class B
    Inherits A

    Public Overrides MustOverride Sub F()
End Class
```

在此示例中，类 `B` 用 `MustOverride` 方法重写 `A.F`。 这意味着，从 `B` 派生的任何类都必须重写 `F`，除非它们也 `MustInherit` 声明。

如果以下所有条件都适用，则会发生编译时错误：

* 声明上下文包含单个可访问的继承方法，该方法具有相同的签名和返回类型（如果有）作为重写方法。
* 要重写的继承方法是可重写的。 换言之，要重写的继承方法不是 `Shared` 或 `NotOverridable`。
* 所声明方法的可访问域与要重写的继承方法的可访问域相同。 有一种例外情况：如果另一个方法在重写方法不具有 `Friend` 访问权限的另一个程序集中，则 `Protected Friend` 方法必须由 `Protected` 方法重写。
* 重写方法的参数与 `ByVal`、`ByRef`、`ParamArray,` 和 `Optional` 修饰符（包括为可选参数提供的值）的使用情况相匹配的已重写方法的参数。
* 重写方法的类型参数与重写的方法的类型参数匹配，而不是类型约束。

当重写基泛型类型中的方法时，重写方法必须提供对应于基类型参数的类型参数。 例如：

```vb
Class Base(Of U, V) 
    Public Overridable Sub M(x As U, y As List(Of V)) 
    End Sub
End Class

Class Derived(Of W, X)
    Inherits Base(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Overrides Sub M(x As W, y As List(Of X)) 
    End Sub 
End Class

Class MoreDerived
    Inherits Derived(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Overrides Sub M(x As String, y As List(Of Integer))
    End Sub
End Class
```

请注意，对于某些类型参数集，可能无法重写泛型类中的可重写方法。 如果该方法 `MustOverride`声明，这意味着可能无法实现某些继承链。 例如：

```vb
MustInherit Class Base(Of T, U)
    Public MustOverride Sub S1(x As T)
    Public MustOverride Sub S1(y As U)
End Class

Class Derived
    Inherits Base(Of Integer, Integer)

    ' Error: Can't override both S1's at once
    Public Overrides Sub S1(x As Integer)
    End Sub
End Class
```

重写声明可使用基本访问权限访问重写的基方法，如以下示例中所示：

```vb
Class Base
    Private x As Integer

    Public Overridable Sub PrintVariables()
        Console.WriteLine("x = " & x)
    End Sub
End Class

Class Derived
    Inherits Base

    Private y As Integer

    Public Overrides Sub PrintVariables()
        MyBase.PrintVariables()
        Console.WriteLine("y = " & y)
    End Sub
End Class
```

在此示例中，类 `Derived` 中 `MyBase.PrintVariables()` 的调用调用在类 `Base`中声明的 `PrintVariables` 方法。 基本访问将禁用可重写的调用机制，并只将基方法视为不可重写的方法。 将 `Derived` 中的调用写入 `CType(Me, Base).PrintVariables()`，它会以递归方式调用在 `Derived`中声明的 `PrintVariables` 方法，而不是在 `Base`中声明的方法。

仅当包含 `Overrides` 修饰符时，方法才能重写另一个方法。 在所有其他情况下，具有与继承方法相同的签名的方法只是隐藏继承方法，如下例所示：

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overridable Sub F() ' Warning, shadowing inherited F().
    End Sub
End Class
```

在此示例中，类 `Derived` 中 `F` 方法不包括 `Overrides` 修饰符，因此不会重写类 `Base`中的方法 `F`。 相反，类 `Derived` 中的方法 `F` 隐藏类 `Base`中的方法，并且会报告警告，因为声明不包括 `Shadows` 或 `Overloads` 修饰符。

在下面的示例中，类 `Derived` 的方法 `F` 隐藏了 `F` 继承自类 `Base`的可重写方法：

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F() ' Shadows Base.F within Derived.
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F() ' Ok, overrides Base.F.
    End Sub
End Class
```

由于类 `Derived` 中的新方法 `F` 具有 `Private` 访问权限，因此它的作用域只包含 `Derived` 的类体，不会扩展到类 `MoreDerived`。 因此，`F` 类 `MoreDerived` 中的方法的声明可以重写 `F` 继承自类 `Base`的方法。

调用 `Overridable` 方法时，实例方法的派生程度最高的实现是根据实例的类型调用的，无论调用的是基类中的方法还是派生类中的方法。 与类 `R` `M` 的 `Overridable` 方法的派生程度最高的实现如下所示：

* 如果 `R` 包含 `M`的引入 `Overridable` 声明，则这是 `M`的派生程度最高的实现。

* 否则，如果 `R` 包含 `M`的重写，则这是 `M`的派生程度最高的实现。

* 否则，`M` 的派生程度最高的实现与 `R`的直接基类的实现相同。

## <a name="accessibility"></a>辅助功能

声明指定它声明的实体的*可访问性*。 实体的可访问性不会更改实体名称的作用域。 声明的*可访问域*是声明的实体可访问的所有声明空间的集合。

五种访问类型为 `Public`、`Protected`、`Friend`、`Protected Friend`和 `Private`。 `Public` 是最宽松的访问类型，其他四种类型都是 `Public`的子集。 最小的权限访问类型是 `Private`的，另外四种访问类型都是 `Private`的超集。

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

声明的访问类型通过可选的访问修饰符指定，该修饰符可以是 `Public`、`Protected`、`Friend`、`Private`或 `Protected` 和 `Friend`的组合。 如果未指定访问修饰符，则默认访问类型取决于声明上下文;允许的访问类型也依赖于声明上下文。

* 使用 `Public` 修饰符声明的实体具有 `Public` 访问权限。 对 `Public` 实体的使用没有限制。

* 使用 `Protected` 修饰符声明的实体具有 `Protected` 访问权限。 只能在类的成员（常规类型成员和嵌套类）或 `Overridable` 标准模块和结构成员（必须通过定义从 `System.Object` 或 `System.ValueType`继承）上指定 `Protected` 访问。 如果成员不是实例成员，或者通过派生类的实例进行访问，则派生类可以访问 `Protected` 成员。 `Protected` 访问不是 `Friend` 访问的超集。

* 使用 `Friend` 修饰符声明的实体具有 `Friend` 访问权限。 只能在包含实体声明的程序中或通过 `System.Runtime.CompilerServices.InternalsVisibleToAttribute` 特性提供 `Friend` 访问的任何程序集的程序中访问具有 `Friend` 访问权限的实体。

* 使用 `Protected Friend` 修饰符声明的实体具有 `Protected` 和 `Friend` 访问的联合。

* 使用 `Private` 修饰符声明的实体具有 `Private` 访问权限。 只能在其声明上下文内（包括任何嵌套实体）访问 `Private` 实体。

声明中的可访问性不依赖于声明上下文的可访问性。 例如，使用 `Private` 访问声明的类型可能包含具有 `Public` 访问权限的类型成员。

以下代码演示了各种可访问域：

```vb
Public Class A
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer
End Class

Friend Class B
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer

    Public Class C
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class

    Private Class D
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class
End Class
```

此示例中的类和成员具有以下可访问域：

* `A` 和 `A.X` 的可访问域是不受限制的。

* `A.Y`、`B`、`B.X`、`B.Y`、`B.C`、`B.C.X`和 `B.C.Y` 的可访问域是包含程序。

* `A.Z` 的可访问域 `A.`

* `B.Z`、`B.D`、`B.D.X`和 `B.D.Y` 的可访问域是 `B`，其中包括 `B.C` 和 `B.D`。

* `B.C``B.C.Z` 的可访问域。

* `B.D``B.D.Z` 的可访问域。

如示例所示，成员的可访问域绝不会超出包含类型的可访问域。 例如，即使所有 `X` 成员都 `Public` 声明的可访问性，但所有 `A.X` 都具有受包含类型约束的可访问域。

对 `Protected` 实例成员的访问必须通过派生类型的实例，以便不相关的类型无法访问对方的每个受保护成员。 例如：

```vb
Class User
    Protected Password As String
End Class

Class Employee
    Inherits User
End Class

Class Guest
    Inherits User

    Public Function GetPassword(u As User) As String
        ' Error: protected access has to go through derived type.
        Return U.Password
    End Function
End Class
```

在上面的示例中，类 `Guest` 仅有权访问受保护的 `Password` 字段（如果它使用 `Guest`的实例限定）。 这会阻止 `Guest` 仅通过将其强制转换为 `User`来访问 `Employee` 对象的 `Password` 字段。

为了 `Protected` 泛型类型中的成员访问，声明上下文包括类型参数。 这意味着，具有一组类型参数的派生类型无法访问具有一组不同类型参数的派生类型的 `Protected` 成员。 例如：

```vb
Class Base(Of T)
    Protected x As T
End Class

Class Derived(Of T)
    Inherits Base(Of T)

    Public Sub F(y As Derived(Of String))
        ' Error: Derived(Of T) cannot access Derived(Of String)'s 
        '     protected members
        y.x = "a"
    End Sub
End Class
```

__纪录.__ 无论C#提供哪些类型参数，语言（也可能还有其他语言）都允许泛型类型访问 `Protected` 成员。 设计包含 `Protected` 成员的泛型类时，应牢记这一点。


### <a name="constituent-types"></a>构成类型

声明的*构成类型*是声明引用的类型。 例如，常量的类型、方法的返回类型和构造函数的参数类型均为构成类型。 声明的构成类型的可访问域必须与声明自身的可访问域的可访问域相同或超集。 例如：

```vb
Public Class X
    Private Class Y
    End Class

    ' Error: Exposing private class Y outside of X.
    Public Function Z() As Y
    End Function

    ' Valid: Not exposing outside of X.
    Private Function A() As Y
    End Function
End Class

Friend Class B
    Private Class C
    End Class

    ' Error: Exposing private class Y outside of B.
    Public Function D() As C
    End Function
End Class
```

## <a name="type-and-namespace-names"></a>类型和命名空间名称

许多语言构造需要指定命名空间或类型;可以通过使用命名空间或类型的名称的限定形式来指定这些。 *限定名*由一系列用句点分隔的标识符组成;句点右侧的标识符在由时间段左侧的标识符指定的声明空间中解析。

命名空间或类型的*完全限定名称*是一个限定名称，其中包含所有包含命名空间和类型的名称。 换言之，命名空间或类型的完全限定名是 `N.T`的，其中 `T` 是实体的名称，`N` 是其包含实体的完全限定名称。

下面的示例演示了多个命名空间和类型声明及其在内联注释中的完全限定名称。

```vb
Class A            ' A.
End Class

Namespace X        ' X.
    Class B        ' X.B.
        Class C    ' X.B.C.
        End Class
    End Class

    Namespace Y    ' X.Y.
        Class D    ' X.Y.D.
        End Class
    End Namespace 
End Namespace 

Namespace X.Y      ' X.Y.
    Class E        ' X.Y.E.
    End Class
End Namespace
```

请注意，命名空间 X. Y 已在源代码中的两个不同位置声明，但这两个分部声明只包含一个名为 X.x 的命名空间，该命名空间同时包含类 D 和类 E。

在某些情况下，限定名称可能以关键字 `Global`开头。 关键字表示未命名的最外侧命名空间，这在声明隐藏封闭命名空间的情况下非常有用。 `Global` 关键字允许在这种情况下将 "转义" 到最外面的命名空间。 例如：

```vb
Namespace NS1
    Class System
    End Class

    Module Test
        Sub Main()
            ' Error: Class System does not contain Int32
            Dim x As System.Int32


            ' Legal, binds to System in outermost namespace
            Dim y As Global.System.Int32
        End Sub
    End Module
End Namespace
```

在上面的示例中，第一个方法调用无效，因为标识符 `System` 绑定到类 `System`，而不是命名空间 `System`。 访问 `System` 命名空间的唯一方法是使用 `Global` 来转义外部命名空间。 不能在 `Imports` 语句或 `Namespace` 声明中使用 `Global`。

由于其他语言可能会引入与语言中的关键字匹配的类型和命名空间，因此 Visual Basic 将关键字识别为限定名称的一部分（只要它们遵循句点即可）。 以这种方式使用的关键字被视为标识符。 例如，限定的标识符 `X.Default.Class` 是有效的限定标识符，而 `Default.Class` 则不是。

### <a name="qualified-name-resolution-for-namespaces-and-types"></a>命名空间和类型的限定名称解析

给定形式 `N.R(Of A)`的限定命名空间或类型名称，其中 `R` 是限定名称中最右边的标识符，而 `A` 是可选的类型参数列表，以下步骤描述如何确定限定名称是指哪个命名空间或类型：

1. 使用限定或非限定名称解析的规则解析 `N`。

2. 如果 `N` 的解析失败或解析为类型参数，则会发生编译时错误。

3. 否则，如果 `R` 与 N 中的命名空间的名称相匹配，但未提供任何类型实参，或 `R` 与 `N` 的类型形参相同的可访问类型（如果有），则限定名称指该命名空间或类型。

4. 否则，如果 `N` 包含一个或多个标准模块，并且 `R` 与具有相同数量的类型参数（如果有）的类型参数与类型参数（如果有）的名称相匹配，则限定名称指该类型。 如果 `R` 将具有相同数量的类型参数的可访问类型的名称与类型参数（如果有）的名称相匹配，则会发生编译时错误。

5. 否则，将发生编译时错误。

__纪录.__ 此解析过程的含义是，类型成员在解析命名空间或类型名称时不会隐藏命名空间或类型。

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a>命名空间和类型的非限定名称解析

如果给定了一个不限定的名称 `R(Of A)`（其中 `A` 是可选的类型参数列表），以下步骤将介绍如何确定非限定名称所引用的命名空间或类型：

1. 如果 R 与当前方法的类型参数的名称匹配，并且未提供任何类型参数，则非限定名称引用该类型参数。

2.  对于包含名称引用的每个嵌套类型，从最内层的类型开始，转到最外层：
    1. 如果 `R` 与当前类型中的类型参数的名称匹配，并且未提供任何类型参数，则非限定名称将引用该类型参数。
    2. 否则，如果 `R` 将具有相同数量的类型参数的可访问嵌套类型的名称匹配为类型参数（如果有），则非限定名称将引用该类型。

3. 对于包含名称引用的每个嵌套命名空间，从最内层命名空间开始，转到最外面的命名空间：
    1. 如果 `R` 与当前命名空间中的嵌套命名空间的名称匹配，并且未提供类型参数列表，则非限定名称将引用该嵌套命名空间。
    2. 否则，如果 `R` 将具有相同数量的类型参数的可访问类型的名称与当前命名空间中的类型参数（如果有）相匹配，则非限定名称将引用该类型。
    3. 否则，如果命名空间包含一个或多个可访问的标准模块，并且 `R` 与具有相同数量的类型参数（如果有）的类型参数与类型参数（如果有）的名称相匹配，则非限定名称将引用该嵌套类型。 如果 `R` 将具有相同数量的类型参数的可访问嵌套类型的名称与类型参数（如果有）的名称相匹配，则会发生编译时错误。

4. 如果源文件有一个或多个导入别名，并且 `R` 与其中一个别名的名称匹配，则非限定名称将引用该导入别名。 如果提供了类型参数列表，则会发生编译时错误。

5. 如果包含名称引用的源文件有一个或多个导入：
    1. 如果 `R` 将具有相同数量的类型参数的可访问类型的名称与类型参数（如果有）的名称相匹配（如果有），则只会将该类型命名为。 如果 `R` 匹配具有与类型参数相同数量的类型参数（如果有）的可访问类型的名称（如果有），则在多个导入和全部都不属于同一类型时，将发生编译时错误。
    2. 否则，如果未提供任何类型参数列表，并且 `R` 与只包含一个导入的可访问类型的命名空间的名称匹配，则非限定名称将引用该命名空间。 如果未提供任何类型参数列表，并且 `R` 与具有可访问类型的命名空间的名称相匹配，但在多个导入中，所有不是同一个命名空间，则会发生编译时错误。
    3. 否则，如果导入包含一个或多个可访问的标准模块，并且 `R` 与具有相同数量的类型参数（如果有）的类型参数与类型参数（如果有）的名称相匹配，则非限定名称引用该类型。 如果 `R` 将具有相同数量的类型参数的可访问嵌套类型的名称与类型参数（如果有）的名称相匹配，则会发生编译时错误。

6. 如果编译环境定义了一个或多个导入别名，并且 `R` 匹配其中一个的名称，则非限定名称将引用该导入别名。 如果提供了类型参数列表，则会发生编译时错误。

7. 如果编译环境定义了一个或多个导入：
    1. 如果 `R` 将具有相同数量的类型参数的可访问类型的名称与类型参数（如果有）的名称相匹配（如果有），则只会将该类型命名为。 如果 `R` 将具有相同数量的类型参数的可访问类型的名称与类型参数（如果有）相匹配，则会发生编译时错误。
    2. 否则，如果未提供任何类型参数列表，并且 `R` 与只包含一个导入的可访问类型的命名空间的名称匹配，则非限定名称将引用该命名空间。 如果未提供任何类型参数列表，并且 `R` 与具有可访问类型的命名空间的名称相匹配，则会发生编译时错误。
    3. 否则，如果导入包含一个或多个可访问的标准模块，并且 `R` 与具有相同数量的类型参数（如果有）的类型参数与类型参数（如果有）的名称相匹配，则非限定名称引用该类型。 如果 `R` 将具有相同数量的类型参数的可访问嵌套类型的名称与类型参数（如果有）的名称相匹配，则会发生编译时错误。

8. 否则，将发生编译时错误。

__纪录.__ 此解析过程的含义是，类型成员在解析命名空间或类型名称时不会隐藏命名空间或类型。

通常，名称只能在特定命名空间中出现一次。 但是，由于命名空间可以在多个 .NET 程序集中进行声明，因此在两个程序集定义具有相同完全限定名称的类型时，可能会出现这种情况。 在这种情况下，当前源文件集中声明的类型优先于在外部 .NET 程序集中声明的类型。 否则，名称是不明确的，没有办法消除名称的歧义。

## <a name="variables"></a>变量

*变量*表示存储位置。 每个变量都有一个类型，用于确定可以在变量中存储的值。 由于 Visual Basic 是一种类型安全的语言，因此，程序中的每个变量都具有类型，并且该语言保证变量中存储的值始终为适当的类型。 变量始终初始化为其类型的默认值，然后才可以对变量进行引用。 不能访问未初始化的内存。

## <a name="generic-types-and-methods"></a>泛型类型和方法

类型（标准模块和枚举类型除外）和方法可以声明*类型参数，类型参数*是在声明类型的实例或调用方法之前将不提供的类型。 具有类型参数的类型和方法也称为*泛型类型*和*泛型方法*，因为必须一般编写类型或方法，而不会对使用类型或方法的代码所提供的类型进行特定的了解。

__纪录.__ 目前，即使方法和委托可以是泛型，属性、事件和运算符本身也不能是泛型。 但是，它们可以使用包含类中的类型参数。

从泛型类型或方法的角度来看，类型参数是一个占位符类型，它将在使用类型或方法时用实际类型填充。 在使用类型或方法的点处，类型参数替换类型或方法中的类型参数。 例如，可以将泛型 stack 类实现为：

```vb
Public Class Stack(Of ItemType)
    Protected Items(0 To 99) As ItemType
    Protected CurrentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        If CurrentIndex = 100 Then
            Throw New ArgumentException("Stack is full.")
        End If

        Items(CurrentIndex) = Data
        CurrentIndex += 1
    End Sub

    Public Function Pop() As ItemType
        If CurrentIndex = 0 Then
            Throw New ArgumentException("Stack is empty.")
        End If

        CurrentIndex -= 1
        Return Items(CurrentIndex + 1) 
    End Function
End Class
```

使用 `Stack(Of ItemType)` 类的声明必须为类型参数 `ItemType`提供类型参数。 然后，将在类中使用 `ItemType` 的任何位置填充此类型：

```vb
Option Strict On

Module Test
    Sub Main()
        Dim s1 As New Stack(Of Integer)()
        Dim s2 As New Stack(Of Double)()

        s1.Push(10.10)   ' Error: Stack(Of Integer).Push takes an Integer
        s2.Push(10.10)   ' OK: Stack(Of Double).Push takes a Double
        Console.WriteLine(s2.Pop().GetType().ToString()) ' Prints: Double
    End Sub
End Module
```

### <a name="type-parameters"></a>类型参数

类型参数可以在类型或方法声明上提供。 每个类型参数都是一个标识符，它是为创建构造类型或方法提供的类型自变量的占位符。 与此相反，类型参数是在使用泛型类型或方法时替换类型参数的实际类型。

```antlr
TypeParameterList
    : OpenParenthesis 'Of' TypeParameter ( Comma TypeParameter )* CloseParenthesis
    ;

TypeParameter
    : VarianceModifier? Identifier TypeParameterConstraints?
    ;

VarianceModifier
    : 'In' | 'Out'
    ;
```

类型或方法声明中的每个类型参数在该类型或方法的声明空间中定义一个名称。 因此，它不能与另一个类型参数、类型成员、方法参数或局部变量具有相同的名称。 类型或方法的类型参数的作用域是整个类型或方法。 由于类型参数的作用域为整个类型声明，因此嵌套类型可以使用外部类型参数。 这也意味着在访问嵌套在泛型类型中的类型时，必须始终指定类型参数：

```vb
Public Class Outer(Of T)
    Public Class Inner
        Public Sub F(x As T)
            ...
        End Sub
    End Class
End Class

Module Test
    Sub Main()
        Dim x As New Outer(Of Integer).Inner()
        ...
    End Sub
End Module
```

与类的其他成员不同，类型参数不能继承。 类型中的类型参数只能由其简单名称引用;换句话说，不能用包含类型名称对它们进行限定。 尽管编程样式不正确，但嵌套类型中的类型参数可以隐藏在外部类型中声明的成员或类型参数：

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

类型和方法可基于类型或方法声明的类型参数（或*arity*）的数目进行重载。 例如，以下声明是合法的：

```vb
Module C
    Sub M()
    End Sub

    Sub M(Of T)()
    End Sub

    Sub M(Of T, U)()
    End Sub
End Module

Structure C(Of T)
    Dim x As T
End Structure

Class C(Of T, U)
End Class
```

对于类型，重载始终与指定的类型参数的数目匹配。 在同一程序中同时使用泛型和非泛型类时，这很有用：

```vb
Class Queue 
End Class      

Class Queue(Of T)
End Class

Class X
    Dim q1 As Queue                 ' Non-generic queue
    Dim q2 As Queue(Of Integer)     ' Generic queue
End Class
```

方法重载决策部分介绍了类型参数上的重载方法规则。

在包含声明中，类型参数被视为完全类型。 由于类型参数可以使用许多不同的实际类型参数进行实例化，因此类型参数与其他类型的操作和限制略有不同，如下所述：

* 类型参数不能直接用于声明基类或接口。

* 对类型参数进行成员查找的规则取决于应用于该类型参数的约束（如果有）。

* 类型参数的可用转换取决于应用于类型参数的约束（如果有）。

* 如果没有 `Structure` 约束，则可以使用 `Is` 和 `IsNot`将类型参数表示的值与 `Nothing` 进行比较。

* 如果类型参数受 `New` 或 `Structure` 约束的约束，则只能在 `New` 表达式中使用类型参数。

* 类型参数不能在 `GetType` 表达式内的属性异常中的任何位置使用。

* 类型参数可用作其他泛型类型和参数的类型参数。

下面的示例是一个扩展 `Stack(Of ItemType)` 类的泛型类型：

```vb
Class MyStack(Of ItemType)
    Inherits Stack(Of ItemType)

    Public ReadOnly Property Size() As Integer
        Get
            Return CurrentIndex
        End Get
    End Property
End Class
```

当声明向 `MyStack`提供类型参数时，相同的类型参数也将应用于 `Stack`。

类型参数只是一种编译时构造。 在运行时，每个类型参数都绑定到一个运行时类型，该类型是通过向泛型声明提供类型参数来指定的。 因此，使用类型参数声明的变量的类型将在运行时为非泛型类型或特定构造类型。 所有涉及类型参数的语句和表达式的运行时执行都使用作为该参数的类型参数提供的实际类型。


### <a name="type-constraints"></a>类型约束

因为类型参数可以是类型系统中的任何类型，所以泛型类型或方法无法对类型参数作出任何假设。 因此，类型参数的成员被视为 `Object`类型的成员，因为所有类型都派生自 `Object`。

对于诸如 `Stack(Of ItemType)`的集合，这种情况可能不是特别重要的限制，但在某些情况下，泛型类型可能想要对将作为类型参数提供的类型做出假设。 *类型约束*可放置在类型参数上，该类型参数限制可以作为类型参数提供的类型，并允许泛型类型或方法更多地使用类型参数。

```antlr
TypeParameterConstraints
    : 'As' Constraint
    | 'As' OpenCurlyBrace ConstraintList CloseCurlyBrace
    ;

ConstraintList
    : Constraint ( Comma Constraint )*
    ;

Constraint
    : TypeName
    | 'New'
    | 'Structure'
    | 'Class'
    ;
```


```vb
Public Class DisposableStack(Of ItemType As IDisposable)
    Implements IDisposable

    Private _items(0 To 99) As ItemType
    Private _currentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        ...
    End Sub

    Public Function Pop() As ItemType
        ...
    End Function

    Private Sub Dispose() Implements IDisposable.Dispose
        For Each item As IDisposable In _items
            If item IsNot Nothing Then
                item.Dispose()
            End If
        Next item
    End Sub
End Class
```

在此示例中，`DisposableStack(Of ItemType)` 仅将其类型参数限制为实现接口 `System.IDisposable`的类型。 因此，它可以实现 `Dispose` 方法，该方法可释放仍保留在队列中的任何对象。

类型约束必须是特殊约束之一 `Class`、`Structure`或 `New`，或者必须是一个 `T` 的类型其中：

* `T` 是类、接口或类型参数。

* `T` 不是 `NotInheritable`。

* `T` 不是或继承自以下特殊类型之一的类型： `System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`或 `System.ValueType`。

* `T` 不是 `Object`。 由于所有类型都派生自 `Object`，因此如果允许，此类约束将不起作用。

* `T` 必须至少与要声明的泛型类型或方法相同。

可以通过将类型约束括在大括号（`{}`）中，为单个类型形参指定多个类型约束。 给定类型参数仅有一个类型约束可以为类。 将 `Structure` 特殊约束与命名类约束或 `Class` 特殊约束合并是错误的。

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

类型约束可以使用包含类型或任何包含类型的类型参数。 在下面的示例中，约束要求提供的类型参数实现泛型接口，并使用其自身作为类型参数：

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

特殊类型约束 `Class` 将所提供的类型参数约束到任何引用类型。

__纪录.__ 特定类型约束 `Class` 可以通过接口满足。 结构可以实现接口。 因此，在 "T" 结构（不满足 "`Class` 特殊" 约束）和 "U" 实现的接口（它满足 `Class` 特殊约束）的情况下，可能满足约束 `(Of T As U, U As Class)`。

特殊类型约束 `Structure` 将提供的类型参数约束为除 `System.Nullable(Of T)`以外的任何值类型。

__纪录.__ 结构约束不允许 `System.Nullable(Of T)`，因此无法将 `System.Nullable(Of T)` 作为其自身的类型参数提供。

特殊类型约束 `New` 要求提供的类型参数必须具有可访问的无参数构造函数，并且不能 `MustInherit`声明。 例如：

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

类类型约束要求提供的类型参数必须是该类型或从其继承。 接口类型约束要求提供的类型参数必须实现该接口。 类型参数约束要求提供的类型参数必须派生自或实现为匹配的类型参数提供的所有边界。 例如：

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

在此示例中，`AddRange` 上 `S` 的类型参数被限制为 `List`的类型参数 `T`。 这意味着，`List(Of Control)` 会将 `AddRange`的类型参数约束为任何从 `Control`继承或继承的类型。

通过在除特殊约束（`Class`、`Structure`、`New`）以外的 `S`上传递所有 `T`的约束，可解析类型参数约束 `Of S As T`。 具有循环约束（例如 `Of S As T, T As S`）是错误的。 如果类型参数约束本身具有 `Structure` 约束，则是错误的。 添加约束后，可能会出现很多特殊情况：

* 如果存在多个类约束，派生程度最高的类将被视为约束。 如果一个或多个类约束没有继承关系，则约束为满足，这是一个错误。

 * 如果类型参数将 `Structure` 特殊约束与命名类约束或 `Class` 特殊约束合并，则是错误的。 类约束可以 `NotInheritable`，在这种情况下，将不接受该约束的派生类型并且它是错误。

该类型可以是或从继承的类型之一，以下特殊类型： `System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`或 `System.ValueType`。 在这种情况下，只接受类型或从其继承的类型。 约束为这些类型之一的类型参数只能使用 `DirectCast` 运算符允许的转换。 例如：

```vb
MustInherit Class Base(Of T)
    MustOverride Sub S1(Of U As T)(x As U)
End Class

Class Derived
    Inherits Base(Of Integer)

    ' The constraint of U must be Integer, which is normally not allowed.
    Overrides Sub S1(Of U As Integer)(x As U)
        Dim y As Integer = x    ' OK
        Dim z As Long = x       ' Error: Can't convert
    End Sub
End Class
```

此外，由于上述某个松弛法，类型参数被限制为值类型，因此无法调用对该值类型定义的任何方法。 例如：

```vb
Class C1(Of T)
    Overridable Sub F(Of G As T)(x As G)
    End Sub
End Class

Class C2
    Inherits C1(Of IntPtr)

    Overrides Sub F(Of G As IntPtr)(ByVal x As G)
        ' Error: Cannot access structure members
         x.ToInt32()
    End Sub
End Class
```

如果在替换后，约束结束为数组类型，则还允许任何协变数组类型。 例如：

```vb
Module Test
    Class B
    End Class

    Class D
        Inherits B
    End Class

    Function F(Of T, U As T)(x As U) As T
        Return x
    End Function

    Sub Main()
        Dim a(9) As B
        Dim b(9) As D

        a = F(Of B(), D())(b)
    End Sub
End Module
```

具有类或接口约束的类型参数被视为具有与该类或接口约束相同的成员。 如果类型参数具有多个约束，则类型参数被视为具有约束的所有成员的并集。 如果有多个约束中具有相同名称的成员，则成员将按以下顺序隐藏：类约束隐藏接口约束中的成员，这些成员隐藏 `System.ValueType` 中的成员（如果指定了 `Structure` 约束），这将隐藏 `Object`中的成员。 如果具有相同名称的成员出现在多个接口约束中，则成员不可用（如在多个接口继承中），并且类型参数必须强制转换为所需的接口。 例如：

```vb
Class C1
    Sub S1(x As Integer)
    End Sub
End Class

Interface I1
    Sub S1(x As Integer)
End Interface

Interface I2
    Sub S1(y As Double)
End Interface

Module Test
    Sub T1(Of T As {C1, I1, I2})()
        Dim a As T
        a.S1(10)       ' Calls C1.S1, which is preferred
        a.S1(10.10)    ' Also calls C1.S1, class is still preferred
    End Sub

    Sub T2(Of T As {I1, I2})()
        Dim a As T
        a.S1(10)    ' Error: Call is ambiguous between I1.S1, I2.S1
    End Sub
End Module
```

当提供类型参数作为类型参数时，类型参数必须满足匹配类型参数的约束。

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

受约束的类型参数的值可用于访问在约束中指定的实例成员（包括实例方法）。

```vb
Interface IPrintable
    Sub Print()
End Interface

Class Printer(Of V As IPrintable)
    Sub PrintOne(v1 As V)
        V1.Print()
    End Sub
End Class
```

### <a name="type-parameter-variance"></a>类型参数的方差

接口或委托类型声明中的类型参数可以选择指定*方差修饰符*。 具有变体修饰符的类型参数限制了类型参数可在接口或委托类型中的使用方式，但允许泛型接口或委托类型转换为具有变体兼容类型参数的另一个泛型类型。 例如：

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub Main()
        Dim x As IEnumerable(Of Derived) = ...

        ' OK, as IEnumerable(Of Base) is variant compatible
        ' with IEnumerable(Of Derived)
        Dim y As IEnumerable(Of Base) = x
    End Sub
End Module
```

具有具有变体修饰符的类型参数的泛型接口具有几个限制：

* 它们不能包含指定参数列表的事件声明（但允许使用委托类型的自定义事件声明或事件声明）。

* 它们不能包含嵌套的类、结构或枚举类型。

__纪录.__ 这些限制的原因在于，嵌套在泛型类型中的类型隐式复制其父级的泛型参数。 对于嵌套类、结构或枚举类型，这些类型的类型不能对其类型参数具有变体修饰符。 对于带有参数列表的事件声明，生成的嵌套委托类在 `In` 位置（即参数类型）中实际使用的类型实际用于 `Out` 位置（即事件的类型）时，可能会出现混淆错误。

使用 Out 修饰符声明的类型参数是*协变*的。 非正式类型参数只能在输出位置中使用--即从接口或委托类型返回的值，而不能在输入位置使用。 如果是以下情况，则将类型 `T` 视为*有效的 covariantly* ：

* `T` 为类、结构或枚举类型。

* `T` 为非泛型委托或接口类型。

* `T` 是其元素类型为 covariantly 有效的数组类型。

* `T` 是未声明为 `Out` 类型参数的类型参数。

* `T` 是使用类型参数 `X(Of P1,...,Pn)` 构造接口或委托类型，`A1,...,An` 这样：

  * 如果 `Pi` 声明为 Out 类型参数，则 `Ai` 是有效的 covariantly。

  * 如果 `Pi` 声明为 In 类型参数，则 `Ai` 是有效的 contravariantly。

以下内容必须是接口或委托类型中的有效 covariantly：

* 接口的基接口。

* 函数或委托类型的返回类型。

* 如果有 `Get` 访问器，则为属性的类型。

* 任何 `ByRef` 参数的类型。

例如：

```vb
Delegate Function D(Of Out T, U)(x As U) As T

Interface I1(Of Out T)
End Interface

Interface I2(Of Out T)
    Inherits I1(Of T)

    ' OK, T is only used in an Out position
    Function M1(x As I1(Of T)) As T

    ' Error: T is used in an In position
    Function M2(x As T) As T
End Interface
```

__纪录.__ `Out` 不是保留字。

使用 In 修饰符声明的类型参数是*逆变*的。 非正式类型参数（非正式类型参数）只能在输入位置使用--即传入接口或委托类型的值，不能用于输出位置。 如果是以下情况，则将类型 `T` 视为*有效的 contravariantly* ：

* `T` 为类、结构或枚举类型。

* `T` 为非泛型委托或接口类型。

* `T` 是其元素类型为 contravariantly 有效的数组类型。

* `T` 是未在类型参数中声明为的类型参数。

* `T` 是使用类型参数 `X(Of P1,...,Pn)` 构造接口或委托类型，`A1,...,An` 这样：

  * 如果 `Pi` 声明为 `Out` 类型参数，则 `Ai` 为有效的 contravariantly。

  * 如果 `Pi` 声明为 `In` 类型参数，则 `Ai` 为有效的 covariantly。

以下内容必须是接口或委托类型中的有效 contravariantly：

* 参数的类型。

* 方法类型参数上的类型约束。

* 如果属性具有 `Set` 访问器，则为属性的类型。

* 事件的类型。

例如：

```vb
Delegate Function D(Of T, In U)(x As U) As T

Interface I1(Of In T)
End Interface

Interface I2(Of In T)
    ' OK, T is only used in an In position
    Sub M1(x As I1(Of T))

    ' Error: T is used in an Out position
    Function M2() As T
End Interface
```

在类型必须有效的情况下为 contravariantly 和 covariantly （例如，具有 `Get` 和 `Set` 访问器或 `ByRef` 参数的属性）时，不能使用变体类型参数。


协方差和方差会给出 "钻石多义性问题"。 考虑下列代码：

```vb
Class C
    Implements IEnumerable(Of String)
    Implements IEnumerable(Of Exception)
     
    Public Function GetEnumerator1() As IEnumerator(Of String) _
       Implements IEnumerable(Of String).GetEnumerator
       Console.WriteLine("string")
    End Function
     
    Public Function GetEnumerator2() As IEnumerator(Of Exception) _
       Implements IEnumerable(Of Execption).GetEnumerator
       Console.WriteLine("exception")
    End Function
End Class
     
Dim c As IEnumerable(Of Object) = New C
c.GetEnumerator()
```

可以通过两种方式将类 `C` 转换为 `IEnumerable(Of Object)`，这两种方法都可以通过从 `IEnumerable(Of String)` 的协变转换和从 `IEnumerable(Of Exception)`到协变转换。 CLR 未指定 `c.GetEnumerator()`将调用这两种方法中的哪一种。 通常，每当将类声明为实现具有公共超类型的两个不同泛型参数的协变接口时（例如，在这种情况下 `String` 和 `Exception` 具有常见的父类型 `Object`），或者声明一个类来实现具有具有相同子类型的两个不同泛型参数的逆变接口时，可能会出现多义性。 编译器会对此类声明发出警告。
