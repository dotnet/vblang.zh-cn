# <a name="general-concepts"></a>一般概念

本章我们将介绍数都必需能识别的 Microsoft Visual Basic 语言的语义的概念。 许多概念应该熟悉的 Visual Basic 编程人员或 C/c + + 程序员提供的但其精确定义可能会不同。

## <a name="declarations"></a>声明

Visual Basic 程序由多个命名实体组成。 这些实体通过引入*声明*和表示该程序的"含义"。

在高级别中，请*命名空间*是组织其他实体，如嵌套的命名空间和类型的实体。 *类型*是描述值并定义可执行代码的实体。 类型可能包含嵌套的类型和类型成员。 *类型成员*是常量、 变量、 方法、 运算符、 属性、 事件、 枚举值和构造函数。

可以包含其他实体的实体定义*声明空间*。 实体被引入声明空间通过声明或继承;包含声明空间称为实体的*声明上下文*。 反过来声明声明空间中的实体定义新的声明空间可以包含更多嵌套的 entity 声明;因此，在程序中的声明组成声明空间的层次的结构。

除在重载的类型成员的情况下是无效的声明，可将相同类型的名称相同的实体到同一个声明上下文。 此外，声明空间可能永远不会包含不同类型的实体具有相同的名称;例如，声明空间可能永远不会包含一个变量和方法按相同的名称。

__请注意。__ 它可能在其他语言来创建一个包含不同类型的具有相同名称的实体 （例如，如果语言区分大小写，并且允许基于大小写不同的声明） 的声明空间中。 在这种情况下，多人可访问的实体将被视为绑定到该名称时;如果多个实体的类型是大多数可访问名称不明确。 `Public` 更可访问性比`Protected Friend`，`Protected Friend`更可访问性比`Protected`或`Friend`，并`Protected`或`Friend`更可访问性比`Private`。

命名空间声明空间是"已结束，打开"具有相同的完全限定名称的两个命名空间声明分配到同一声明空间。 在下面的示例中，两个命名空间声明参与到同一声明空间，在这种情况下声明两个类的完全限定名称`Data.Customer`和 `Data.Order:`

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

由于向同一声明空间分配的两个声明，如果每个包含具有相同名称的类的声明会发生编译时错误。

### <a name="overloading-and-signatures"></a>重载和签名

若要声明空间中声明具有相同名称的实体的相同类型的唯一方法是通过*重载*。 可以重载方法、 运算符、 实例构造函数和属性。

重载的类型成员必须具有唯一的签名。 类型成员的签名包含类型参数和数字的数量和类型成员的参数。 转换运算符还包括在签名中的运算符的返回类型。

以下不是成员的签名的一部分，因此不能重载上：

* 对类型成员的修饰符 (例如，`Shared`或`Private`)。

* 参数修饰符 (例如，`ByVal`或`ByRef`)。

* 参数的名称。

* 一种方法或运算符 （除转换运算符） 或属性的元素类型的返回类型。

* 在类型参数的约束。

下面的示例显示了一组重载的方法声明，以及它们的签名。 此声明不会有效，因为多个方法声明具有相同的签名。

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

若要定义的泛型类型，可能会使用基于提供的类型参数完全相同的签名包含成员无效。 重载决策规则用于尝试，此类重载之间产生歧义尽管可能存在的情况下在其中无法消除歧义。 例如：

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

*作用域*的实体的名称是一的组的所有声明空间，在其中就可以引用而无需限定该名称。 一般情况下，实体的作用域是名称的整个声明其上下文中;但是，实体的声明可以包含嵌套的声明具有相同名称的实体。 在这种情况下，嵌套的 entity *shadows*，或隐藏、 外部实体，以及对隐藏实体访问仅通过限定。

通过嵌套隐藏出现在命名空间或类型嵌套在命名空间、 嵌套在其他类型的类型和类型成员的正文。 通过嵌套声明始终隐藏隐式; 发生不不需要任何显式语法。

在以下示例中内,`F`方法中，实例变量`i`隐藏由本地变量`i`，但内`G`方法，`i`仍是指实例变量。

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

如果内层作用域中的名称将隐藏外层作用域中的名称，它会隐藏该名称的所有重载的匹配项。 在下面的示例中，在调用`F(1)`调用`F`中声明`Inner`因为外部出现的所有`F`隐藏内部声明。 出于相同原因，在调用`F("Hello")`出现错误。

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

继承关系是在哪一种类型之一 (*派生*类型) 派生自另一个 (*基*类型)，以便派生的类型的声明空间隐式包含可访问非构造函数类型成员和嵌套的类型及其基类型。 在以下示例中，类`A`是类的基类`B`，并`B`派生自`A`。

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

由于`A`does 未显式指定的基类，其基本类是隐式`Object`。

以下是继承的重要方面：

* 继承是可传递的。 如果类型*C*派生自类型*B*，然后键入*B*派生自类型*A*，类型*C*继承键入类型中声明成员*B*以及类型中声明的类型成员*A*。

* 派生的类型扩展了，但不能缩小，其基类型。 派生的类型可以添加新的类型成员，它都可隐藏继承的类型成员，但它不能删除继承的类型成员的定义。

* 由于类型的实例包含的所有其基类型的类型成员，因此转换始终存在从派生类型与其基类型。

* 所有类型必须都具有基类型，类型除外`Object`。 因此，`Object`是所有类型的 ultimate 基类型和所有类型可以都转换为它。

* 不允许派生中的循环。 它是类型`B`派生自类型`A`，它是错误的类型`A`以直接或间接派生自类型`B`。

* 一种类型可能不直接或间接派生自嵌套在它的类型。

以下示例会生成编译时错误，因为类循环彼此依赖。

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

下面的示例也生成编译时错误，因为`B`间接派生自其嵌套类`C`通过类`A`。

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

下一个示例不会生成错误，因为类`A`不是派生自类`B`。

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a>MustInherit 和 NotInheritable 类

一个`MustInherit`类是可以只作为基类型的不完整类型。 一个`MustInherit`类不能进行实例化，因此它是使用错误`New`运算符之一。 可以声明的变量`MustInherit`类; 仅可以将此类变量分配`Nothing`是派生自的类的一个值或`MustInherit`类。

当常规类派生自`MustInherit`类，正则类必须重写所有继承`MustOverride`成员。 例如：

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

`MustInherit`类`A`引入`MustOverride`方法`F`。 类`B`引入了其他方法`G`，但不提供的实现`F`。 类`B`也因此必须声明`MustInherit`。 类`C`重写`F`并提供实际实现。 由于有没有未完成`MustOverride`类中的成员`C`，不需要为`MustInherit`。

一个`NotInheritable`类是不能与另一个类派生一个类。 `NotInheritable` 类主要用于防止无意的派生。

在此示例中，类`B`是错误，因为它会尝试派生自`NotInheritable`类`A`。 类不能将标记都`MustInherit`和`NotInheritable`。

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a>接口和多重继承

与其他类型，只能从单个基类型派生，不同接口可能派生自多个基接口。 因此，接口可从不同的基接口继承具有相同名称的类型成员。 在这种情况下，乘继承名称可用在派生接口中，并不指派生接口通过这些类型任何的成员会导致编译时错误，而不考虑签名或重载。 相反，必须通过基接口名称引用冲突的类型成员。

在以下示例中前, 两个语句导致编译时错误，因为乘法继承成员`Count`界面中不可用`IListCounter`:

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

如示例所示，通过强制转换来解决二义性`x`为适当的基接口类型。 这种转换具有不运行时成本;它们只是包含在编译时查看作为无派生类型的实例。

当一个类型成员继承自相同的基接口通过多个路径时，类型成员被视为如同它仅继承一次。 换而言之，派生的接口仅包含从特定的基接口继承的每个类型成员的一个实例。 例如：

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

如果一条路径的继承层次结构中隐藏的类型成员名称，名称被阴影中的所有路径。 在以下示例中，`IBase.F`隐藏成员`ILeft.F`成员，但不是隐藏在`IRight`:

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

在调用`d.F(1)`中选择`ILeft.F`，即使`IBase.F`似乎不通过将导致在访问路径中阴影`IRight`。 因为从的访问路径`IDerived`到`ILeft`到`IBase`阴影`IBase.F`，在访问路径中还隐藏该成员`IDerived`到`IRight`到`IBase`。

### <a name="shadowing"></a>隐藏

派生的类型的重新声明隐藏继承的类型成员的名称。 隐藏名称不会删除具有该名称; 继承的类型成员它只是使所有具有该名称的继承的类型成员在派生类中不可用。 隐藏的声明可以是实体的任何类型。

实体不是可以进行重载可以选择隐藏的两种形式之一。 *按名称隐藏*使用指定的`Shadows`关键字。 按名称隐藏的实体将隐藏所有内容中的基类，包括所有重载该名称的。 *按名称和签名隐藏*使用指定的`Overloads`关键字。 遮盖了具有的名称和签名的实体通过与实体相同的签名与该名称隐藏的所有内容。 例如：

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

隐藏方法替换`ParamArray`参数按名称和签名隐藏单个签名、 不是所有可能的扩展的签名。 即使隐藏方法的签名与隐藏方法的未扩展的签名匹配，这是如此。 下面的示例：

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

将打印`Base`，即使`Derived.F`具有相同的签名的未扩展的窗体`Base.F`。

相反，具有的方法`ParamArray`参数仅隐藏具有相同的签名不是所有可能的扩展签名的方法。 下面的示例：

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

将打印`Base`，即使`Derived.F`已具有相同的签名以展开的形式`Base.F`。

隐藏方法或未指定的属性`Shadows`或`Overloads`假定`Overloads`如果方法或属性声明`Overrides`，`Shadows`否则为。 如果指定的一组重载实体的一个成员`Shadows`或`Overloads`关键字，它们都必须指定它。 `Shadows`和`Overloads`关键字不能同时指定。 既不`Shadows`也不`Overloads`可以指定标准模块中; 标准模块中的成员隐式隐藏成员继承自`Object`。

它是有效的类型成员的已通过接口继承相乘的继承 （和即从而不可用），名称的卷影从而使该名称在派生接口。

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

由于卷影继承方法允许使用方法，所以有可能类可以包含多个`Overridable`具有相同签名的方法。 这不会造成多义性问题，由于只有派生程度最高的方法是可见。 在以下示例中，`C`并`D`类包含两个`Overridable`具有相同签名的方法：

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

有两个`Overridable`方法： 一个由类引入`A`和一个方法由类`C`。 引入的类的方法`C`从类继承的方法将隐藏`A`。 因此，`Overrides`类中的声明`D`重写方法由类引入`C`，并不能为类`D`若要重写方法由类引入`A`。 该示例生成输出：

```
B.F
B.F
D.F
D.F
```

可以调用隐藏`Overridable`通过访问类的实例方法`D`通过该方法不在其中隐藏无派生的类型。

它不是有效的卷影`MustOverride`方法，因为在大多数情况下这会导致类不可用。 例如：

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

在此情况下，该类`MoreDerived`需重写`MustOverride`方法`Base.F`，而是因为类`Derived`阴影`Base.F`，无法做到这一点。 没有方法来声明是有效的子代`Derived`。

隐藏来自外部范围的名称，与隐藏继承的作用域中的可访问名称导致警告报告，如以下示例所示：

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

方法的声明`F`类中`Derived`导致报告警告。 隐藏继承的名称不具体而言是错误，因为那将排除的基类，这些类的单独演变。 例如，如果上述情况可能会发生因为类的更高版本`Base`引入了一种方法`F`未出现在早期版本的类。 如果上述情况曾出现错误*任何*对基类中单独进行版本控制的类库所做的更改可能会导致派生的类变得无效。

可以通过使用消除隐藏继承的名称由导致该警告`Shadows`或`Overloads`修饰符：

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

`Shadows`修饰符指示想要隐藏继承的成员。 它不是指定的错误`Shadows`或`Overloads`修饰符如果以便隐藏任何类型成员的名称。

新成员的声明隐藏继承的成员只能在新成员，如以下示例所示的范围中：

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

在上面的方法声明的示例`F`类中`Derived`隐藏方法`F`类的继承`Base`，但由于新方法`F`类中`Derived`具有`Private`访问权限，其作用域内不会扩展到类`MoreDerived`。 因此，调用`F()`中`MoreDerived.G`有效，并且将调用`Base.F`。 在重载的类型成员的情况下重载的类型成员的整个集视为如同是它们都具有用于隐藏的最宽松的访问权限。

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

在此示例中，即使的声明`F()`中`Derived`使用声明`Private`访问，请重载`F(Integer)`使用声明`Public`访问。 因此，为了隐藏名称`F`中`Derived`就像它是被视为`Public`，因此这两种方法卷影`F`中`Base`。

## <a name="implementation"></a>实现

*实现*类型声明它实现了接口，则该类型实现的接口的所有类型成员时，存在关系。 实现特定接口的类型是转换为该接口。 不能实例化接口，但可以声明变量的接口;此类变量只能分配一个值，是实现接口的类。 例如：

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

与乘继承类型成员实现接口的类型仍必须实现这些方法，即使它们不能直接从要实现的派生接口访问。 例如：

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

甚至`MustInherit`类必须提供的所有成员的实现的接口实现; 但是，可以通过将它们声明为延迟这些方法的实现`MustOverride`。 例如：

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

可以选择一种类型重新实现其基类型实现的接口。 若要重新实现该接口，该类型必须显式声明它实现该接口。 可以选择重新实现接口的类型重新实现只有某些但并非所有接口-不重新实现的任何成员的成员继续使用基类型实现。 例如：

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

此示例输出：

```
TestDerived.DerivedTest1
TestBase.Test2
```

当派生的类型实现的基接口由派生的类型的基类型实现的接口时，可以选择仅实现已经不由基类型实现的接口的类型成员的派生的类型。 例如：

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

此外可以使用可重写方法中的基类型实现接口方法。 在这种情况下，派生的类型也可能会重写可重写方法并更改接口的实现。 例如：

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

### <a name="implementing-methods"></a>实现的方法

一种类型*实现*通过提供具有的方法实现的接口的类型成员`Implements`子句。 两个类型成员必须具有相同数量的参数，所有类型和参数的修饰符必须匹配，包括可选参数的默认值、 返回类型必须与匹配，，所有方法参数的约束必须匹配。 例如：

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

如果它们都符合上述条件，一个方法可以实现任意数量的接口类型成员。 例如：

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

当在泛型接口中实现一种方法，实现方法必须提供对应于接口的类型参数的类型参数。 例如：

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

请注意，可能不可能实施的一些组类型参数的泛型接口。

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

*多态性*提供了不同的方法或属性的实现的功能。 使用多态性，相同的方法或属性可以执行不同的操作，具体取决于对其进行调用的实例的运行时类型。 调用方法或属性是多态*可重写*。 与此相反，非可重写方法或属性的实现是不变;实现都是相同的方法或属性调用中的类的实例上是否已声明的或派生类的实例。 调用的非可重写方法或属性时，该实例的编译时类型是决定性因素。 例如：

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

可重写方法也可能是`MustOverride`，这意味着它提供没有方法正文，并必须重写。 `MustOverride` 方法只允许在`MustInherit`类。

在下面的示例中，类`Shape`定义本身就可以绘制一个几何形状对象的抽象概念：

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

`Paint`方法是`MustOverride`因为没有有意义的默认实现。 `Ellipse`并`Box`类是具体`Shape`实现。 因为这些类不是`MustInherit`，他们需要对重写`Paint`方法，并提供实际实现。

它是错误的基本的访问权限，以引用`MustOverride`方法，如下面的示例演示：

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

有关报告的错误`MyBase.F()`调用因为它引用了`MustOverride`方法。

### <a name="overriding-methods"></a>重写方法

一种类型可能*重写*继承的可重写方法通过声明的方法具有相同的名称和，签名，并将标记与声明`Overrides`修饰符。没有重写方法，下面列出的其他要求。 而`Overridable`方法声明中引入的新方法，`Overrides`方法声明替换继承的方法实现。

重写方法可能会声明`NotOverridable`，以防止进一步重写的任何派生类型中的方法。 实际上，`NotOverridable`方法成为中不可重写任何进一步派生类。

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

在示例中，类`B`提供了两个`Overrides`方法： 一种方法`F`具有`NotOverridable`修饰符和方法`G`不执行的。 利用`NotOverridable`修饰符阻止类`C`进一步重写方法`F`。

此外可以声明重写方法`MustOverride`，即使它在重写的方法未声明`MustOverride`。 这需要声明包含的类`MustInherit`和任何进一步派生类未声明为`MustInherit`必须重写方法。 例如：

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

在示例中，类`B`重写`A.F`与`MustOverride`方法。 这意味着任何类派生自`B`将必须重写`F`，除非它们声明为`MustInherit`也。

除非以下条件都重写方法，则返回 true，否则，将发生编译时错误：

* 声明上下文包含具有相同的签名和返回类型 （如果有） 的单个可访问继承的方法作为重写方法。
* 正在重写继承的方法是可重写。 换而言之，正在重写继承的方法不是`Shared`或`NotOverridable`。
* 所声明的可访问域是方法的被重写继承方法的可访问性域相同。 没有一种情况例外：`Protected Friend`必须通过重写方法`Protected`方法，如果另一种方法是在另一个程序集重写方法不具有`Friend`访问权限。
* 重写方法的参数匹配的使用情况有关的重写的方法的参数`ByVal`， `ByRef`，`ParamArray,`和`Optional`修饰符，其中包括为可选参数提供的值。
* 重写方法的类型参数与类型约束有关的重写的方法类型参数匹配。

当重写基的泛型类型中的方法，重写方法必须提供对应于基类型参数的类型参数。 例如：

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

请注意，可能在泛型类中重写的方法可能无法被覆盖的一些组类型参数。 如果该方法声明为`MustOverride`，这意味着，某些继承链不可能。 例如：

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

重写声明可以访问使用基本的访问权限，如以下示例所示重写基方法：

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

在示例中，调用`MyBase.PrintVariables()`类中`Derived`调用`PrintVariables`类中声明方法`Base`。 基访问禁用的可重写调用机制，并只需将视为非可重写方法的基方法。 必须调用`Derived`已写入`CType(Me, Base).PrintVariables()`，它会以递归方式调用`PrintVariables`方法中声明`Derived`中, 声明一个不`Base`。

仅当它包括`Overrides`修饰符可以一种方法重写另一种方法。 在所有其他情况下，具有与继承的方法相同的签名的方法只是重写继承的方法，如下面的示例中所示：

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

在示例中，该方法`F`类中`Derived`不包括`Overrides`修饰符，因此不重写方法`F`类中`Base`。 相反，方法`F`类中`Derived`隐藏类中的方法`Base`，并将报告警告，因为在声明中不包含`Shadows`或`Overloads`修饰符。

在下面的示例中，方法`F`类中`Derived`隐藏可重写方法`F`继承自类`Base`:

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

以来的新方法`F`类中`Derived`已`Private`访问权限，其作用域仅包括类正文`Derived`并不会扩展到类`MoreDerived`。 方法的声明`F`类中`MoreDerived`因此可以重写该方法`F`继承自类`Base`。

当`Overridable`调用方法、 调用实例方法的派生程度最高的实现，基于实例，而不考虑调用是对类的基类或派生的类中方法的类型。 派生程度最高的实现`Overridable`方法`M`相对于一个类`R`，如下所示确定：

* 如果`R`包含引入`Overridable`的声明`M`，这是派生程度最高的实现`M`。

* 否则为如果`R`包含的重写`M`，这是派生程度最高的实现`M`。

* 否则，大多数派生的实现`M`是相同的类的直接基类`R`。

## <a name="accessibility"></a>可访问性

声明还指定*可访问性*它声明的实体。 实体的可访问性不会更改实体的名称的作用域。 *可访问性域*声明的是一的组的所有声明空间中声明的实体是可访问。

五种访问类型是`Public`， `Protected`， `Friend`， `Protected Friend`，和`Private`。 `Public` 是最宽松的访问类型和四种类型是所有子集`Public`。 最低权限的访问类型是`Private`，和四个其他访问类型是所有的超集`Private`。

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

声明的访问类型指定一个可选的访问修饰符，可通过`Public`， `Protected`， `Friend`， `Private`，或为的组合`Protected`和`Friend`。 如果指定没有访问修饰符，则默认访问类型取决于声明上下文中;允许访问权限类型还取决于声明上下文。

* 使用实体声明`Public`修饰符有`Public`访问。 没有任何限制的使用`Public`实体。

* 使用实体声明`Protected`修饰符有`Protected`访问。 `Protected` 只能对成员的类 （正则类型成员和嵌套的类） 或在指定访问权限`Overridable`标准模块和结构的成员 (这必须根据定义，继承自`System.Object`或`System.ValueType`)。 一个`Protected`成员是派生类中，可以访问，但前提是该成员不是实例成员，或者访问都是通过派生类的实例。 `Protected` 访问不是一个超集`Friend`访问。

* 使用实体声明`Friend`修饰符有`Friend`访问。 如果一个实体具有`Friend`访问权限是只能在包含实体声明或已被授予的任何程序集的程序中访问`Friend`通过访问`System.Runtime.CompilerServices.InternalsVisibleToAttribute`属性。

* 使用实体声明`Protected Friend`修饰符有的联合`Protected`和`Friend`访问。

* 使用实体声明`Private`修饰符有`Private`访问。 一个`Private`实体是只能在其声明上下文，包括任何嵌套的实体中访问。

在声明中的可访问性不依赖于声明上下文的可访问性。 例如，与声明的类型`Private`访问可能包含的类型成员`Public`访问。

下面的代码演示了各种可访问性域：

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

类和在此示例中的成员具有以下可访问性域：

* 可访问性域`A`和`A.X`不受限制。

* 可访问性域`A.Y`， `B`， `B.X`， `B.Y`， `B.C`， `B.C.X`，和`B.C.Y`是包含程序。

* 可访问域`A.Z`是 `A.`

* 可访问性域`B.Z`， `B.D`， `B.D.X`，和`B.D.Y`是`B`，其中包括`B.C`和`B.D`。

* 可访问性域`B.C.Z`是`B.C`。

* 可访问性域`B.D.Z`是`B.D`。

如示例所示，成员的可访问域是类型的永远不会超过包含。 例如，即使所有`X`的成员具有`Public`声明可访问性，以外的所有`A.X`有包含类型受约束的可访问性域。

访问`Protected`成员必须通过派生类型的实例，以便不相关的类型不能访问彼此的实例具有受保护的成员。 例如：

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

在上面的示例中，类`Guest`仅有权访问受保护`Password`如果它受限定的实例字段`Guest`。 这可以防止`Guest`获得对访问权限`Password`字段`Employee`只需通过它强制转换为对象`User`。

出于`Protected`成员访问声明上下文中的泛型类型，包括类型参数。 这意味着，使用一组类型参数派生的类型不能访问到`Protected`派生类型的成员与一组不同的类型参数。 例如：

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

__请注意。__ C# 语言 （和可能是其他语言） 允许访问的泛型类型`Protected`而不考虑提供类型自变量的成员。 在设计包含的泛型类时应记住保留这`Protected`成员。


### <a name="constituent-types"></a>构成类型

*构成类型*声明的是由声明引用的类型。 例如，一个常量、 方法的返回类型和构造函数的参数类型的类型是所有构成的类型。 声明的构成类型的可访问域必须与相同或声明本身的可访问域的超集。 例如：

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

## <a name="type-and-namespace-names"></a>类型和 Namespace 名称

许多语言构造需要命名空间或类型来指定;可以通过使用命名空间或类型的名称的限定的形式指定这些选项。 一个*限定的名*组成的标识符的一系列用句点分隔的; 在一段右侧的标识符解析上左侧和右侧的段标识符指定的声明空间中。

*完全限定的名称*的命名空间或类型包含所有的名称的限定的名称包含命名空间和类型。 换而言之，命名空间或类型的完全限定的名称是`N.T`，其中`T`是实体的名称和`N`是其包含的实体的完全限定的名称。

下面的示例显示在行中的注释以及其关联的完全限定名称的多个命名空间和类型声明。

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

观察命名空间 X.Y 已声明在源代码中的两个不同位置，但这两个分部声明构成只需调用 X.Y 包含 D 类和类 E.单个命名空间

在某些情况下，限定的名可能会开始使用关键字`Global`。 关键字表示未命名的最外层命名空间，在其中声明会覆盖封闭命名空间的情况下很有用。 `Global`关键字使"转义"出到这种情况中的最外层命名空间。 例如：

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

在上述示例中，第一个方法调用无效因为标识符`System`绑定到该类`System`，不是命名空间`System`。 访问的唯一办法`System`命名空间是使用`Global`来转义扩展到最外层命名空间。 `Global` 不能用于`Imports`语句或`Namespace`声明。

因为类型和与语言中的关键字匹配的命名空间，可能会造成其他语言，Visual Basic 可以识别的关键字以将其限定名称的一部分，只要它们遵循一段。 在这种方式中使用的关键字被视为标识符。 例如，限定的标识符`X.Default.Class`是一个有效的限定的标识符，而`Default.Class`不是。

### <a name="qualified-name-resolution-for-namespaces-and-types"></a>命名空间和类型的限定的名称解析

指定限定命名空间或类型名称的窗体`N.R(Of A)`，其中`R`是限定名称中最右边的标识符和`A`是可选的类型参数列表，以下步骤介绍如何确定哪个命名空间或类型的限定名称指的是：

1. 解决`N`，使用任一限定或非限定名称解析规则。

2. 如果解析`N`出现故障，或者解析为类型参数，则发生编译时错误。

3. 否则为如果`R`匹配项提供了 N 且不使用类型参数中的命名空间的名称，或`R`匹配中的可访问类型`N`与作为类型参数相同数量的类型参数，如果有，则限定的名称指该命名空间或类型。

4. 否则为如果`N`包含一个或多个标准模块和`R`如果任何一个标准模块，然后的限定的名中引用匹配的类型参数的数作为类型参数相同的可访问类型的名称类型。 如果`R`与具有相同数量的类型参数作为类型参数的可访问类型的名称相匹配，如果发生任何在多个标准模块，编译时错误。

5. 否则，将发生编译时错误。

__请注意。__ 此解析过程的含义是，类型成员不隐藏命名空间或类型解析命名空间或类型的名称。

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a>命名空间和类型的非限定的名称解析

非限定名称`R(Of A)`，其中`A`是可选的类型参数列表，以下步骤介绍如何确定向哪些命名空间或类型的非限定的名称引用：

1. 如果 R 与当前方法的类型参数的名称相匹配，提供没有类型参数的非限定的名称指该类型参数。

2.  为每个嵌套类型，其中名称引用，从最里面的类型开始，并转到最外面：
    1. 如果`R`匹配中当前的类型并提供参数，任何类型的类型参数的名称，则该类型参数的非限定的名称引用。
    2. 否则为如果`R`匹配项的可访问名称嵌套具有相同数量的类型作为类型参数，类型参数，如果有，则该类型的非限定的名称引用。

3. 对于每个嵌套的命名空间包含的最内部的命名空间从开始，然后转到最外层命名空间的名称引用：
    1. 如果`R`提供当前命名空间和任何类型参数列表中的嵌套命名空间的名称，则非限定的名称引用该嵌套命名空间的匹配项。
    2. 否则为如果`R`如果任何在当前命名空间，则非限定的名称指该类型的可访问类型作为类型参数的类型参数数量与名称匹配。
    3. 否则为如果该命名空间包含一个或多个可访问标准模块，和`R`的可访问嵌套类型作为类型参数的类型参数数量与名称匹配，如果有的话，在一个标准模块则不合格该嵌套类型引用名称。 如果`R`与具有相同数量的类型参数作为类型参数的可访问嵌套类型的名称相匹配，如果发生任何在多个标准模块，编译时错误。

4. 如果源文件具有一个或多个导入别名和`R`的其中之一，名称匹配，则非限定的名称引用该导入别名。 如果提供类型实参列表，则会发生编译时错误。

5. 如果包含名称引用的源文件具有一个或多个导入：
    1. 如果`R`匹配任何的恰好一个导入，则以非限定的名称指该类型具有相同数量的类型参数作为类型参数的可访问类型的名称。 如果`R`与具有相同数量的类型参数作为类型参数的可访问类型的名称相匹配，如果所有中的多个导入和所有不是相同的类型，则发生编译时错误。
    2. 否则为如果不提供任何类型参数列表和`R`的非限定的名称指该命名空间，然后与一个导入过程中访问的类型相匹配的命名空间名称。 如果不提供任何类型参数列表和`R`匹配项的多个导入中的可访问类型和所有包含的命名空间名称不是相同的命名空间，则发生编译时错误。
    3. 否则为如果导入包含一个或多个可访问标准模块，和`R`匹配具有相同数量的类型参数作为类型参数，如果有一个标准模块中的可访问嵌套类型的名称，然后的非限定的名称引用该类型。 如果`R`与具有相同数量的类型参数作为类型参数的可访问嵌套类型的名称相匹配，如果发生任何在多个标准模块，编译时错误。

6. 如果编译环境定义了一个或多个导入别名和`R`的其中之一，名称匹配，则非限定的名称引用该导入别名。 如果提供类型实参列表，则会发生编译时错误。

7. 如果编译环境定义了一个或多个导入：
    1. 如果`R`匹配任何的恰好一个导入，则以非限定的名称指该类型具有相同数量的类型参数作为类型参数的可访问类型的名称。 如果`R`与具有相同数量的类型参数作为类型参数的可访问类型的名称相匹配，如果发生任何在多个导入、 编译时错误。
    2. 否则为如果不提供任何类型参数列表和`R`的非限定的名称指该命名空间，然后与一个导入过程中访问的类型相匹配的命名空间名称。 如果不提供任何类型参数列表和`R`出现编译时错误，则与在多个导入中访问的类型匹配的命名空间名称。
    3. 否则为如果导入包含一个或多个可访问标准模块，和`R`匹配具有相同数量的类型参数作为类型参数，如果有一个标准模块中的可访问嵌套类型的名称，然后的非限定的名称引用该类型。 如果`R`与具有相同数量的类型参数作为类型参数的可访问嵌套类型的名称相匹配，如果发生任何在多个标准模块，编译时错误。

8. 否则，将发生编译时错误。

__请注意。__ 此解析过程的含义是，类型成员不隐藏命名空间或类型解析命名空间或类型的名称。

通常情况下，名称可仅出现一次在特定的命名空间中。 但是，可以跨多个.NET 程序集声明命名空间，因此很可能有两个程序集在其中定义具有相同的完全限定名称的类型的情况。 在这种情况下，在当前组源代码文件中声明的类型优于外部.NET 程序集中声明的类型。 否则为名称不明确，则不能消除名称歧义。

## <a name="variables"></a>变量

一个*变量*表示的存储位置。 每个变量具有类型，用于确定值可以是存储在变量中。 因为 Visual Basic 是类型的类型安全语言、 程序中的每个变量具有类型和语言保证值存储在变量是类型的适当始终。 始终为它们的类型的默认值初始化变量，变量的任何引用之前。 不能访问未初始化的内存。

## <a name="generic-types-and-methods"></a>泛型类型和方法

（除标准模块和枚举的类型） 的类型和方法可以声明*类型参数*、 哪些不提供类型的实例之前的类型声明或调用的方法。 类型和使用类型参数的方法是也称为*泛型类型*并*泛型方法*分别，因为类型或方法必须编写以一般方式，而无需知道特定使用的类型或方法的代码将提供的类型。

__请注意。__ 在此期间，即使方法和委托可以是泛型，属性、 事件和运算符不能是泛型本身。 但是，它们可能，使用自包含类的类型参数。

从泛型类型或方法的角度来看，类型参数是使用的类型或方法时将使用实际类型填充占位符类型。 类型实参替换类型或方法，而使用的类型或方法的点中的类型参数。 例如，一个泛型堆栈类就可以实现为：

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

使用的声明`Stack(Of ItemType)`类必须提供类型参数的类型参数`ItemType`。 此类型然后填写无论在何处`ItemType`类中使用：

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

在类型或方法声明上可能会提供类型参数。 每个类型参数是提供创建构造的类型或方法的类型参数的占位符的标识符。 与此相反，类型参数是泛型类型或方法使用时替换为类型参数的实际类型。

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

在类型或方法声明中的每个类型参数定义该类型或方法的声明空间中的名称。 因此，它不能具有相同名称作为另一个类型参数、 类型成员、 方法参数或局部变量。 上一个类型或方法的类型参数的作用域是整个类型或方法。 由于类型参数的作用域为整个类型声明，嵌套的类型可以使用外部类型参数。 这也意味着访问类型嵌套在泛型类型时，必须始终指定类型参数：

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

与不同的类的其他成员，不会继承类型参数。 类型中的类型参数仅对由它们的简单名称; 引用换而言之，它们不能为与包含类型名称限定。 尽管错误编程风格，嵌套类型中的类型参数可以隐藏成员或类型参数中的外部类型声明：

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

类型和方法可以重载基于类型参数的数目 (或*arity*) 类型或方法声明。 例如，以下声明是合法的：

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

对于类型，将始终针对指定的类型参数的数目匹配重载。 在同一个程序一起使用泛型和非泛型类时，这非常有用：

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

类型参数上重载的方法的规则的方法重载解析部分所述。

在包含的声明中，类型参数被视为完整的类型。 由于类型参数可以具有多个不同的实际类型参数实例化，类型参数具有与其他类型，如下所述略有不同的操作和限制：

* 类型参数不能用于直接声明基类或接口。

* 参数取决于该约束，如果有，成员查找类型上的规则应用于类型参数。

* 可用的转换为类型参数取决于该约束，如果有的话，应用于类型参数。

* 如果没有`Structure`约束，由类型参数表示的类型值可以与比较`Nothing`使用`Is`和`IsNot`。

* 类型参数仅可在`New`表达式，如果类型参数受到`New`或`Structure`约束。

* 类型参数无法在任何位置中使用属性异常内`GetType`表达式。

* 类型参数可以用作其他泛型类型和参数的类型参数。

下面的示例是泛型类型扩展`Stack(Of ItemType)`类：

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

在声明时提供的类型参数`MyStack`，相同的类型参数将应用于`Stack`也。

作为一种类型，类型参数都是纯粹是一个编译时构造。 在运行时，每个类型参数绑定到通过提供泛型声明的类型参数指定的运行时类型。 因此，使用类型参数声明的变量的类型，在运行时，将非泛型类型或特定的构造的类型。 运行时执行的所有语句和包含类型参数的表达式使用提供的实际类型作为该参数的类型参数。


### <a name="type-constraints"></a>类型约束

由于类型系统中，类型参数可以是任何类型，泛型类型或方法不能进行任何假设类型参数。 因此，类型参数的成员都被认为是该类型的成员`Object`，因为所有类型都派生`Object`。

在类似集合的情况下`Stack(Of ItemType)`，这一事实可能不是一个特别重要的限制，但可能有可能希望进行假设将作为类型自变量提供的类型的泛型类型的情况。 *类型约束*可以施加限制哪些类型可以作为类型参数提供，并允许泛型类型或方法假设类型参数有关的详细信息的类型参数。

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

在此示例中，`DisposableStack(Of ItemType)`其类型参数限制为实现接口的类型`System.IDisposable`。 因此，它可以实现`Dispose`仍保留在队列中释放的任何对象的方法。

类型约束必须是一个特殊约束`Class`， `Structure`，或`New`，或者它必须是类型`T`其中：

* `T` 是一个类、 接口或类型参数。

* `T` 不是 `NotInheritable`。

* `T` 不是一个，或从一个以下特殊类型的继承类型： `System.Array`， `System.Delegate`， `System.MulticastDelegate`， `System.Enum`，或`System.ValueType`。

* `T` 不是 `Object`。 由于所有类型都派生`Object`，如果已允许此类约束会产生任何影响。

* `T` 必须至少与可访问性的泛型类型或所声明的方法。

可以为单个类型参数括在大括号中的类型约束指定多个类型约束 (`{}`)... 只有一种类型约束为给定的类型参数可以是类。 它是错误合并`Structure`命名的类约束的特殊约束或`Class`特殊约束。

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

包含类型或任何包含类型的类型参数，可以使用类型约束。 在以下示例中，该约束要求提供的类型实参实现泛型接口使用自身作为类型参数：

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

特殊类型约束`Class`约束任何引用类型的提供的类型参数。

__请注意。__ 特殊类型约束`Class`可以满足的接口。 和结构可以实现接口。 因此，该约束`(Of T As U, U As Class)`可能是"T"结构感到满意 (这不满足`Class`特殊约束)，和"U"它实现的接口 (其满足`Class`特殊约束)。

特殊类型约束`Structure`以外的任意值类型的提供的类型参数约束，使之`System.Nullable(Of T)`。

__请注意。__ 不允许结构约束`System.Nullable(Of T)`，以便不能提供`System.Nullable(Of T)`用作到其自身的类型参数。

特殊类型约束`New`要求提供的类型参数必须具有可访问的无参数构造函数并不能声明为`MustInherit`。 例如：

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

类类型约束要求提供的类型参数必须为类型或从其继承。 接口类型约束要求提供的类型实参必须实现该接口。 类型参数约束要求提供的类型参数必须派生自或实现所有参数指定的匹配的类型参数的边界。 例如：

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

在此示例中，类型参数`S`上`AddRange`为类型参数约束`T`的`List`。 这意味着`List(Of Control)`会约束`AddRange`的类型或继承的任何类型的参数`Control`。

类型参数约束`Of S As T`通过它传递性地添加的所有已解决`T`的约束到`S`，而非特殊约束 (`Class`， `Structure`， `New`)。 它是错误的循环约束 (例如`Of S As T, T As S`)。 它是其本身具有错误的类型参数约束`Structure`约束。 添加约束之后, 就可以很多特殊情况下的，可能会发生：

* 如果存在多个类约束，派生程度最高的类被视为可约束。 如果一个或多个类约束没有任何继承关系，该约束无法满足，它是错误。

 * 如果类型参数结合`Structure`命名的类约束的特殊约束或`Class`特殊约束，则返回错误。 可能是类约束`NotInheritable`、 在这种情况下接受该约束的任何派生的类型和，则是错误。

类型可能是一种，或从以下特殊类型继承的一种类型： `System.Array`， `System.Delegate`， `System.MulticastDelegate`， `System.Enum`，或`System.ValueType`。 在这种情况下，接受仅的类型或由其继承的类型。 类型参数约束为其中一种类型只能使用允许的转换`DirectCast`运算符。 例如：

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

此外，类型参数约束为值类型，因为上述松弛法之一不能调用该值类型上定义任何方法。 例如：

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

如果该约束后替换，最终为数组类型，也可以是任何协变的数组类型。 例如：

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

类或接口约束的类型参数被视为具有相同的成员的类或接口约束。 如果类型参数有多个约束，然后将类型参数视为具有约束的所有成员的并集。 如果有多个约束，一个同名的成员，则成员隐藏按以下顺序： 类约束隐藏接口约束，隐藏中的成员中的成员`System.ValueType`(如果`Structure`指定约束)，这将隐藏中的成员`Object`。 如果具有相同名称的成员显示在多个接口约束该成员是不可用 （如在多个接口继承） 和类型参数必须强制转换为所需的接口。 例如：

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

提供类型参数作为类型参数时, 的类型参数必须满足的约束匹配的类型参数。

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

受约束的类型参数的值可以用于访问实例成员，包括约束中指定的实例方法。

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

### <a name="type-parameter-variance"></a>类型参数的变体

可以选择指定的接口或委托类型声明中的类型参数*方差修饰符*。 方差修饰符的类型参数限制如何类型参数可在接口或委托类型，但允许泛型接口或委托类型转换为另一个具有 variant 兼容类型参数的泛型类型。 例如：

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

具有与方差修饰符的类型参数的泛型接口具有多个限制：

* 它们不能包含指定的参数列表的事件声明 （但允许使用自定义事件或事件声明委托类型）。

* 它们不能包含嵌套的类、 结构或枚举的类型。

__请注意。__ 这些限制是因为，类型隐式嵌套在泛型类型中复制其父级的泛型参数。 对于嵌套的类、 结构或枚举的类型，这些类型的类型不能具有类型参数上的方差修饰符。 对于使用参数列表的事件声明，生成嵌套的委托类可以具有混淆时将显示要在中使用的类型的错误`In`位置 （例如参数类型） 实际上在使用`Out`位置 (即事件类型）。

使用 Out 修饰符声明的类型参数是*协变*。 通俗地说，协变类型参数仅可用于在输出位置-即从接口或委托类型返回值--，并且不能在输入位置使用。 一种类型`T`被视为*有效 covariantly*如果：

* `T` 是类、 结构或枚举的类型。

* `T` 为非泛型委托或接口类型。

* `T` 为数组类型的元素类型是 covariantly 有效。

* `T` 是一个类型参数的未声明为`Out`类型参数。

* `T` 为构造的接口或委托类型`X(Of P1,...,Pn)`具有类型参数`A1,...,An`以便：

  * 如果`Pi`然后声明为 Out 类型参数`Ai`covariantly 是否有效。

  * 如果`Pi`然后声明为 In 类型参数`Ai`是有效的逆变方式起作用。

以下中必须是有效 covariantly 接口或委托类型：

* 接口的基接口。

* 一个函数的返回类型或委托类型。

* 如果没有属性的类型`Get`访问器。

* 类型为 any`ByRef`参数。

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

__请注意。__ `Out` 不是保留的字。

使用 In 修饰符声明的类型参数是*逆变*。 通俗地说，逆变类型参数仅可用于在输入的位置-即传递给接口或委托类型值--，并且不能在输出位置使用。 一种类型`T`被视为*有效逆变方式起作用*如果：

* `T` 是类、 结构或枚举的类型。

* `T` 为非泛型委托或接口类型。

* `T` 为数组类型的元素类型是有效的逆变方式起作用。

* `T` 是未声明为 In 类型参数的类型参数。

* `T` 为构造的接口或委托类型`X(Of P1,...,Pn)`具有类型参数`A1,...,An`以便：

  * 如果`Pi`被声明为`Out`然后键入参数`Ai`是有效的逆变方式起作用。

  * 如果`Pi`被声明为`In`然后键入参数`Ai`covariantly 是否有效。

下列情况必须为有效逆变方式起作用的接口或委托类型中：

* 参数的类型。

* 方法类型形参的类型约束。

* 如果它具有一个属性的类型`Set`访问器。

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

在其中一种类型必须是有效的情况下是逆变方式起作用和 covariantly (如两个属性`Get`并`Set`访问器或`ByRef`参数)，不能使用变体类型参数。


协变和逆变体为会导致"菱形二义性问题"。 考虑下列代码：

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

该类`C`可以转换为`IEnumerable(Of Object)`两种方式，通过从协变转换`IEnumerable(Of String)`和通过从协变转换`IEnumerable(Of Exception)`。 CLR 不指定这两种方法将调用`c.GetEnumerator()`。 一般情况下，每当声明的类来实现协变接口使用两个不同的泛型参数具有通用超类型 (例如在这种情况下`String`和`Exception`具有通用超类型`Object`)，或对声明的类实现逆变接口使用两个不同的泛型参数具有通用的子类型，则可能会出现多义性。 编译器提供了有关此类声明一条警告。
