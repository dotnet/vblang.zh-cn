---
ms.openlocfilehash: a9499e51a67a9b311ae54410c001f8bd22c30d65
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306062"
---
# <a name="overloaded-method-resolution"></a>重载方法解析

在实践中，确定重载决策的规则旨在查找与提供的实际参数 "最接近的" 重载。 如果存在其参数类型与参数类型匹配的方法，则该方法显然是最接近的方法。 如果不知道，如果其所有参数类型的范围比另一个方法的参数类型小（或与之相同），则一种方法比另一种方法要接近。 如果这两个方法的参数均小于另一个，则无法确定哪个方法接近于参数。

__纪录.__ 重载决策不考虑方法的预期返回类型。 

另请注意，由于命名参数语法，实际参数和形参的顺序可能不相同。

给定方法组后，可使用以下步骤确定参数列表组中最适合的方法。 如果在应用特定步骤后没有成员保留在集中，则会发生编译时错误。 如果集中只有一个成员，则该成员是最适用的成员。 步骤如下：

1.  首先，如果未提供任何类型自变量，则将类型推理应用于具有类型参数的任何方法。 如果方法的类型推理成功，则推断出的类型参数用于该特定方法。 如果对方法的类型推理失败，则将从集合中删除该方法。

2.  接下来，消除集中无法访问或不适用的所有成员（[自变量列表的适用性](overload-resolution.md#applicability-to-argument-list)部分）到参数列表

3.  接下来，如果一个或多个参数 `AddressOf` 或 lambda 表达式，则为每个此类参数计算*委托 relaxation 级别*，如下所示。 如果 `N` 中的最差（最低）委托 relaxation 级别低于 `M` 中的最低委托 relaxation 级别，则从集中消除 `N`。 委托 relaxation 级别如下所示：

    31. *错误委托 relaxation 级别*--如果 `AddressOf` 或 lambda 无法转换为委托类型，则为。
  
    32. *收缩委托 relaxation 返回类型或参数*--如果参数 @no__t 为-1 或具有声明类型的 lambda，并且从其返回类型到委托返回类型的转换为收缩转换，则为;或者，如果该参数是常规 lambda，并且从其任何返回表达式到委托返回类型的转换是收缩的，或者，如果该参数是异步 lambda，并且委托返回类型为 `Task(Of T)` 并且从其任何返回表达式转换为 @no__t 是收缩的;或者，如果该参数是迭代器 lambda，并且委托返回类型为 `IEnumerator(Of T)` 或 `IEnumerable(Of T)`，并且从其任何 yield 操作数到 `T` 的转换为收缩。

    33. 如果委托类型为 `System.Delegate` 或 `System.MultiCastDelegate` 或 `System.Object`，则*将委托 Relaxation 扩展到委托*。

    34. *Drop return 或 arguments 委托 relaxation* --如果参数 @no__t 为-1 或具有已声明返回类型的 lambda，并且委托类型缺少返回类型，则为;或者，如果该参数是具有一个或多个返回表达式的 lambda，并且委托类型缺少返回类型，则为; 否则为。或者，如果参数 @no__t 为-2 或 lambda，并且委托类型具有参数，则为。

    35. *扩大委托 relaxation 返回类型*--如果参数 @no__t 为-1 或具有已声明返回类型的 lambda，并且存在从其返回类型到委托的扩大转换，则为;或者，如果参数是一个常规 lambda，则从所有返回表达式到委托返回类型的转换都是扩大或至少具有一个扩大的标识;或者，如果该参数是一个异步 lambda，并且该委托 @no__t 为-2 或 `Task`，并且从所有返回表达式到的转换分别为 `T` @ no__t-5 @ no__t;或者，如果该参数是迭代器 lambda，并且委托是 @no__t 7 或 `IEnumerable(Of T)` 或 `IEnumerator` 或 `AddressOf`0，则从所有返回表达式到 1 @ no__t @ no__t 的转换或具有至少一个扩大的标识。

    36. *标识委托 relaxation* --如果参数是一个 `AddressOf`，或者是完全匹配委托的 lambda，则不会扩大或缩小参数或返回或生成。接下来，如果集的某些成员不需要收缩转换即可适用于任何参数，则将消除所有执行的成员。 例如：

    ```vb
    Sub f(x As Object)
    End Sub

    Sub f(x As Short)
    End Sub

    Sub f(x As Short())
    End Sub

    f("5") ' picks the Object overload, since String->Short is narrowing
    f(5)   ' picks the Object overload, since Integer->Short is narrowing
    f({5}) ' picks the Object overload, since Integer->Short is narrowing
    f({})  ' a tie-breaker rule subsequent to [3] picks the Short() overload

    ```

4.  接下来，将根据下面的收缩完成排除。 （请注意，如果 Option Strict 为 On，则需要进行收缩的所有成员已被判断为不适用（部分[对实参列表的适用性](overload-resolution.md#applicability-to-argument-list)）并已被步骤2删除。）

    41. 如果集的某些实例成员只需要收缩转换（其中参数表达式类型为 `Object`），则将消除所有其他成员。
    42. 如果集包含多个只需要从 `Object` 收缩的成员，则调用目标表达式将被重新分类为后期绑定方法访问（如果包含方法组的类型是接口，则会提供错误，如果有任何适用的成员是扩展成员）。
    43. 如果有任何候选项只需要从数字文本进行收缩，则在下面的步骤中选择最具体的候选项。 如果入选方只需要从数字文本进行收缩，则会将其作为重载决策的结果选取;否则为错误。

    __纪录.__ 此规则的理由是，如果程序的类型为松类型（即，大多数或所有变量都声明为 `Object`），则从 `Object` 进行的多个转换是收缩时，重载决策可能会很困难。 在许多情况下（要求强类型化方法调用的参数），而不是让重载决策在很多情况下失败，因此，解析合适的重载方法会推迟到运行时进行调用。 这允许松散类型的调用成功，无需进行其他强制转换。 但这种情况的副作用是，执行后期绑定调用需要将调用目标强制转换为 `Object`。 在结构值的情况下，这意味着必须将值装箱为临时值。 如果最终调用方法尝试更改结构字段，则此更改将在该方法返回后丢失。 从这一特殊规则中排除了接口，因为后期绑定始终针对运行时类或结构类型的成员进行解析，其名称可能与它们所实现的接口的成员名称不同。

5.  接下来，如果任何实例方法保留在不需要收缩的集中，则从集中消除所有扩展方法。 例如：

    ```vb
    Imports System.Runtime.CompilerServices

    Class C3
        Sub M1(d As Integer)
        End Sub
    End Class

    Module C3Extensions
        <Extension> _
        Sub M1(c3 As C3, c As Long)
        End Sub

        <Extension> _
        Sub M1(c3 As C3, c As Short)
        End Sub
    End Module

    Module Test
        Sub Main()
            Dim c As New C3()
            Dim sVal As Short = 10
            Dim lVal As Long = 20

            ' Calls C3.M1, since C3.M1 is applicable.
            c.M1(sVal)

            ' Calls C3Extensions.M1 since C3.M1 requires a narrowing conversion
            c.M1(lVal)
        End Sub
    End Module
    ```

    __纪录.__ 如果有适用的实例方法来保证添加导入（这可能会使新的扩展方法进入范围），则将忽略扩展方法，而不会导致对现有实例方法的调用重新绑定到扩展方法。 给定一些扩展方法（即，在接口和/或类型参数上定义的方法）的广泛范围，这是绑定到扩展方法的更安全方法。

6.  接下来，如果在给定参数列表的情况下，在给定参数列表的情况下，如果将集合的任意两个成员 `M` 并 `N`，`M` 则更*具体*（[给定自变量列表的成员/类型的部分的具体内容/类型](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)@no__t）将从该集合中去掉 @no__t 6。 如果集合中保留有多个成员，并且在给定参数列表的情况下，其余成员不是同样特定的，则会产生编译时错误。

7.  否则，如果集合的任意两个成员为 `M` 并 `N`，则按顺序应用以下附加规则规则：

    71. 如果 `M` 没有 ParamArray 参数，但 @no__t 为-1，或者两者同时 `M` 将更少的参数传递到 ParamArray 参数中，而不是 `N`，则从集中消除 @no__t。 例如：

        ```vb
        Module Test
            Sub F(a As Object, ParamArray b As Object())
                Console.WriteLine("F(Object, Object())")
            End Sub

            Sub F(a As Object, b As Object, ParamArray c As Object())
                Console.WriteLine("F(Object, Object, Object())")
            End Sub

           Sub G(Optional a As Object = Nothing)
              Console.WriteLine("G(Object)")
           End Sub

           Sub G(ParamArray a As Object())
              Console.WriteLine("G(Object())")
           End Sub    Sub Main()
                F(1)
                F(1, 2)
                F(1, 2, 3)
              G()
            End Sub
        End Module
        ```

        以上示例生成以下输出：

        ```console
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        __纪录.__ 当类声明具有 paramarray 参数的方法时，也不太常见，还会将某些扩展的窗体包含为常规方法。 这样一来，就可以避免分配在调用具有 paramarray 参数的方法的展开形式时发生的数组实例。

    72. 如果在派生程度高于 `N` 的派生类型中定义 `M`，则从集中消除 `N`。 例如：

        ```vb
        Class Base
            Sub F(Of T, U)(x As T, y As U)
            End Sub
        End Class

        Class Derived
            Inherits Base

            Overloads Sub F(Of T, U)(x As U, y As T)
            End Sub
        End Class

        Module Test
            Sub Main()
                Dim d As New Derived()

                ' Calls Derived.F
                d.F(10, 10)
            End Sub
        End Module
        ```

        此规则也适用于在其上定义扩展方法的类型。 例如：

        ```vb
        Imports System.Runtime.CompilerServices

        Class Base
        End Class

        Class Derived
            Inherits Base
        End Class

        Module BaseExt
            <Extension> _
            Sub M(b As Base, x As Integer)
            End Sub
        End Module

        Module DerivedExt
            <Extension> _
            Sub M(d As Derived, x As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim b As New Base()
                Dim d As New Derived()

                ' Calls BaseExt.M
                b.M(10)

                ' Calls DerivedExt.M 
                d.M(10)
            End Sub
        End Module
        ```

    73. 如果 @no__t 0 和 @no__t 为扩展方法，并且 @no__t 的目标类型是一个类或结构，并且 `N` 的目标类型是一个接口，则消除该集中的 `N`。 例如：

        ```vb
        Imports System.Runtime.CompilerServices

        Interface I1
        End Interface

        Class C1
            Implements I1
        End Class

        Module Ext1
            <Extension> _
            Sub M(i As I1, x As Integer)
            End Sub
        End Module

        Module Ext2
            <Extension> _
            Sub M(c As C1, y As Integer)
            End Sub
        End Module

        Module Test
            Sub Main()
                Dim c As New C1()

                ' Calls Ext2.M, because Ext1.M is hidden since it extends
                ' an interface.
                c.M(10)

                ' Calls Ext1.M
                CType(c, I1).M(10)
            End Sub
        End Module
        ```

    74. 如果 @no__t 0 和 @no__t 为扩展方法，并且在类型参数替换后，`M` 和 @no__t 的目标类型都相同，并且在类型参数替换之前 `M` 的目标类型不包含类型参数，但目标类型为`N`，则具有比 `N` 目标类型更少的类型参数，从而消除集中的 @no__t。 例如：

        ```vb
        Imports System.Runtime.CompilerServices

        Module Module1
            Sub Main()
                Dim x As Integer = 1
                x.f(1) ' Calls first "f" extension method

                Dim y As New Dictionary(Of Integer, Integer)
                y.g(1) ' Ambiguity error
            End Sub

            <Extension()> Sub f(x As Integer, z As Integer)
            End Sub

            <Extension()> Sub f(Of T)(x As T, z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, Integer), z As T)
            End Sub

            <Extension()> Sub g(Of T)(y As Dictionary(Of T, T), z As T)
            End Sub
        End Module
        ```

    75. 在替换类型参数之前，如果 `M` 的*泛型*（Section [Genericity](overload-resolution.md#genericity)）低于 @no__t，则从集中消除 @no__t。

    76. 如果 `M` 不是扩展方法，并且 @no__t 为-1，则从集中消除 `N`。

    77. 如果 @no__t 为0，并且 @no__t 为扩展方法，并且在 @no__t 3 （节[扩展方法集合](expressions.md#extension-method-collection)）之前找到 `M`，则从集中消除 @no__t 5。 例如：

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
                Sub M1(c As C1, y As Integer)
                End Sub
            End Module
        End Namespace

        Namespace N1.N2.N3
            Module Test
                Sub Main()
                    Dim x As New C1()

                    ' Calls N2C1Extensions.M1
                    x.M1(10)
                End Sub
            End Module
        End Namespace
        ```

        如果在同一步骤中找到扩展方法，则这些扩展方法是不明确的。 此调用可能始终使用包含扩展方法的标准模块的名称，并调用扩展方法，就像它是常规成员一样。 例如：

        ```vb
        Imports System.Runtime.CompilerServices

        Class C1
        End Class

        Module C1ExtA
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module C1ExtB
            <Extension> _
            Sub M(c As C1)
            End Sub
        End Module

        Module Main
            Sub Test()
                Dim c As New C1()

                C1.M()               ' Ambiguous between C1ExtA.M and BExtB.M
                C1ExtA.M(c)          ' Calls C1ExtA.M
                C1ExtB.M(c)          ' Calls C1ExtB.M
            End Sub
        End Module
        ```

    78. 如果 @no__t 0 和 `N` 都是所需的类型推理来生成类型参数，并且 `M` 不需要为其任何类型参数（即推断为单个类型的每个类型参数）确定基准类型，但 @no__t 为3则消除 `N`从集。

        __纪录.__ 此规则可确保在以前版本中成功的重载决策（在其中推断类型参数的多个类型会导致错误），继续生成相同的结果。

    79. 如果要解析重载决策来解析 @no__t 0 表达式中的委托创建表达式的目标，并且在 `N` 是一个子例程时，委托和 @no__t 都为函数，则消除该集中的 @no__t。 同样，如果委托和 @no__t 都是子例程，而 `N` 是一个函数，则从该集中消除 `N`。

    710. 如果 `M` 未使用任何可选的参数默认值来代替显式参数，但 @no__t 为，则从集中消除 `N`。

    711. 在替换类型参数之前，如果 `M` 的深度 genericity （节[genericity](overload-resolution.md#genericity)）比 `N`*更多*，则将从该集中消除 @no__t。

8. 否则，调用是不明确的，并发生编译时错误。

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a>给定自变量列表的成员/类型的具体类型

如果在给定参数列表 `A` 的情况下，成员 `M` 被视为*等效于*`N`，或者如果 `M` 中的每个参数类型与 `N` 中的相应参数类型相同，则被视为相同。

__纪录.__ 由于扩展方法的原因，两个成员可以在具有相同签名的方法组中结束。 除了类型参数或 paramarray 扩展，两个成员还可以平等指定，但不能具有相同的签名。

如果成员 @no__t 的签名不同，并且 `M` 中的至少一个参数类型比 `N` 中的参数类型更具体，并且 `N` 中没有参数类型比参数更具体，则将其视为*更具体*的 @no__t键入 `M`。 给定一对与参数 `Aj` 匹配的参数 `Mj` 和 `Nj`，如果满足以下条件之一，则会将 `Mj` 的类型视为比 @no__t 的类型*更具体*：

* 存在从类型 `Mj` 到类型 `Nj` 的扩大转换。 （__注意：__ 由于在这种情况下，将对参数类型进行比较而不考虑实参类型，因此，在这种情况下，将不考虑从常量表达式扩大到数值类型的扩大转换。

* @no__t 为文本 `0`，`Mj` 为数值类型，`Nj` 为枚举类型。 （__注意：__ 此规则是必需的，因为文本 `0` 扩大到任何枚举类型。 由于枚举类型扩大到其基础类型，因此，默认情况下，`0` 的重载决策默认情况下，对数值类型使用枚举类型。 我们收到了大量反馈，指出此行为是不够直观的。）

* `Mj` 和 `Nj` 均为数值类型，`Mj` 早于列表中的 `Nj`、`SByte`、`Short`、`UShort`、`Integer`、`UInteger`、0、1、2，4。 （__注意：__ 有关数值类型的规则非常有用，因为特定大小的有符号和无符号的数值类型仅在它们之间进行收缩转换。 上述规则打破了两种类型之间的关联，以支持更多的 "自然" 数值类型。 当对扩展到特定大小的有符号和无符号数字类型的类型（例如，同时满足这两个的数字文本）执行重载解析时，这一点尤其重要。

* @no__t 为0，`Nj` 为委托函数类型，`Mj` 的返回类型比 `Nj` 的返回类型更具体，但前提是 `Aj` 归类为 lambda 方法，`Mj` 或 `Nj`，则为类型的类型参数（假设它是一个委托类型）替换为要比较的类型。

* `Mj` 与 @no__t 的类型相同，并且不 `Nj`。 （__注意：__ 需要注意的是，上一规则与之间C#略有不同，在C#这种情况下，需要在比较返回类型之前委托函数类型具有相同的参数列表，而 Visual Basic。）

#### <a name="genericity"></a>Genericity

成员 `M` 确定为不如成员 `N` 的*泛型*，如下所示：

1. 如果为每对匹配参数 `Mj` 和 `Nj`，`Mj` 相对于方法的类型参数而言，-2 小于或等于 `Nj`，并且至少有一个 @no__t 与方法的类型参数有关的泛型。
2. 否则，如果为每对匹配参数 `Mj` 和 `Nj`，`Mj` 相对于类型参数而言，-2 小于或等于 `Nj`，并且至少有一个 `Mj` 对于类型上的类型参数而言不太通用。，则 `M` 的泛型低于 `N`。

如果参数 `M` 的类型 `Mt` 和 `Nt` 都引用类型参数，或者两者都不引用类型参数，则该参数将被视为与参数 @no__t 相同。 如果 `Mt` 未引用类型参数，则 `M` 被视为低于 `N`，而 @no__t 为。

例如：

```vb
Class C1(Of T)
    Sub S1(Of U)(x As U, y As T)
    End Sub

    Sub S1(Of U)(x As U, y As U)
    End Sub

    Sub S2(x As Integer, y As T)
    End Sub

    Sub S2(x As T, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As C1(Of Integer) = New C1(Of Integer)

        x.S1(10, 10)    ' Calls S1(U, T)
        x.S2(10, 10)    ' Calls S2(Integer, T)
    End Sub
End Module
```

在 currying 过程中固定的扩展方法类型参数被视为类型参数，而不是方法上的类型参数。 例如：

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M1(Of T, U)(x As T, y As U, z As T)
    End Sub
End Module

Module Test
    Sub Main()
        Dim i As Integer = 10

        i.M1(10, 10)
    End Sub
End Module
```

#### <a name="depth-of-genericity"></a>Genericity 的深度

确定成员 `M` 的*深度*超过了成员 `N` 如果，则对于每对匹配参数 `Mj` 和 `Nj`，`Mj` 的深度等于或等于*genericity 的深度*，至少有一个 @no__t具有更高的 genericity 深度。 Genericity 的深度定义如下：

* 除了类型参数外，任何内容都具有比类型参数更深层的 genericity;

* 与其他构造类型（具有相同数量的类型参数），如果至少有一个类型自变量的深度为 genericity，并且没有类型参数的深度低于对应的类型，则递归使用构造类型的 genericity 的深度更高参数。

* 如果第一个数组的元素类型具有比第二个元素类型更高的 genericity 深度，则数组类型具有比另一个数组类型（具有相同维数）更多的 genericity。

例如：

```vb
Module Test

    Sub f(Of T)(x As Task(Of T))
    End Sub

    Sub f(Of T)(x As T)
    End Sub

    Sub Main()
        Dim x As Task(Of Integer) = Nothing
        f(x)            ' Calls the first overload
    End Sub
End Module
```

### <a name="applicability-to-argument-list"></a>自变量列表的适用性

如果可使用参数列表调用方法，则方法*适用*于一组类型参数、位置参数和命名参数。 参数列表与参数列表匹配，如下所示：

1. 首先，将每个位置参数与方法参数列表进行匹配。 如果有比参数更多的位置参数，且最后一个参数不是 paramarray，则该方法不适用。 否则，会通过 paramarray 元素类型的参数来扩展 paramarray 参数，使之与位置参数的数目匹配。 如果省略将进入 paramarray 的位置参数，则该方法不适用。
2. 接下来，将每个命名参数与具有给定名称的参数相匹配。 如果某个命名参数无法匹配、匹配 paramarray 参数或匹配已与另一个位置或命名参数匹配的参数，则该方法不适用。
3. 接下来，如果指定了类型参数，则将其与类型参数列表进行匹配。 如果两个列表的元素数不相同，则该方法不适用，除非类型参数列表为空。 如果类型参数列表为空，则使用类型推理来尝试并推断类型参数列表。 如果类型推断失败，则该方法不适用。 否则，将在签名中填充类型参数的位置。如果未匹配的参数不是可选的，则该方法不适用。
4. 如果自变量表达式不能隐式转换为它们匹配的参数类型，则该方法不适用。
5. 如果参数是 ByRef，并且没有从参数类型到参数类型的隐式转换，则该方法不适用。
6. 如果类型参数违反了方法的约束（包括步骤3中推断出的类型参数），则该方法不适用。 例如：

```vb
Module Module1
    Sub Main()
        f(Of Integer)(New Exception)
        ' picks the first overload (narrowing),
        ' since the second overload (widening) violates constraints 
    End Sub

    Sub f(Of T)(x As IComparable)
    End Sub

    Sub f(Of T As Class)(x As Object)
    End Sub
End Module
```

如果单个自变量表达式与 paramarray 参数匹配并且参数表达式的类型可转换为 paramarray 参数和 paramarray 元素类型的类型，则该方法适用于其展开和未扩展的形式。有两个例外。 如果从参数表达式的类型到 paramarray 类型的转换是收缩转换，则该方法仅适用于其扩展形式。 如果参数表达式是文本 `Nothing`，则该方法仅适用于其未展开的形式。 例如：

```vb
Module Test
    Sub F(ParamArray a As Object())
        Dim o As Object

        For Each o In a
            Console.Write(o.GetType().FullName)
            Console.Write(" ")
        Next o
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Object() = { 1, "Hello", 123.456 }
        Dim o As Object = a

        F(a)
        F(CType(a, Object))
        F(o)
        F(CType(o, Object()))
    End Sub
End Module
```

以上示例生成以下输出：

```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

在 @no__t 的第一次和最后一次调用中，`F` 的正常形式适用，因为存在从参数类型到参数类型的扩大转换（两者都为类型 `Object()`），并且参数作为常规值参数传递。 在第二次和第三次调用中，`F` 的正常形式不适用，因为不存在从参数类型到参数类型的扩大转换（从 `Object` 到 `Object()` 的转换是收缩）。 不过，@no__t 的展开形式适用，并且由调用创建一个单元素 `Object()`。 使用给定的参数值（其本身是对 @no__t 的引用）初始化数组的单个元素。

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a>传递参数和可选参数的选取参数

如果参数是值参数，则匹配参数表达式必须分类为值。 该值将转换为参数的类型并在运行时作为参数传入。 如果参数是引用参数，且匹配参数表达式归类为类型与参数相同的变量，则在运行时将作为参数传入对变量的引用。

否则，如果匹配的参数表达式归类为变量、值或属性访问，则会分配参数类型的临时变量。 在运行时调用方法之前，将参数表达式重新分类为值，转换为参数的类型，并将其分配给临时变量。 然后，将作为参数传入对临时变量的引用。 计算方法调用后，如果参数表达式归类为变量或属性访问，则会将临时变量分配给变量表达式或属性访问表达式。 如果属性访问表达式没有 @no__t 0 取值函数，则不执行赋值。

对于未提供参数的可选参数，编译器将选取如下所述的参数。 在所有情况下，它针对泛型类型替换之后的参数类型进行测试。

* 如果可选参数的属性 `System.Runtime.CompilerServices.CallerLineNumber`，并且调用来自源代码中的位置，并且表示该位置的行号的数值文本具有到参数类型的内部转换，则使用数字文本。 如果调用跨多行，则选择要使用的行是依赖于实现的。

* 如果可选参数的属性 `System.Runtime.CompilerServices.CallerFilePath`，并且调用来自源代码中的位置，并且表示该位置的文件路径的字符串文本包含到参数类型的内部转换，则使用字符串文字。 文件路径的格式取决于实现。

* 如果可选参数的属性 `System.Runtime.CompilerServices.CallerMemberName`，并且调用位于类型成员的主体内或应用于该类型成员的任何部分的属性中，则表示该成员名称的字符串文本具有到参数类型的内部转换，则使用字符串文字。 对于属性访问器或自定义事件处理程序中的调用，使用的成员名称是属性或事件本身的成员名称。 对于属于运算符或构造函数的调用，将使用特定于实现的名称。

如果以上均不适用，则使用可选参数的默认值（如果未提供默认值，则为 `Nothing`）。 如果以上应用了多个，则所选的使用是依赖于实现的。

@No__t-0 和 @no__t 属性对于日志记录很有用。 @No__t-0 可用于实现 @no__t。 下面是一些示例。

```vb
Sub Log(msg As String,
        <CallerFilePath> Optional file As String = Nothing,
        <CallerLineNumber> Optional line As Integer? = Nothing)
    Console.WriteLine("{0}:{1} - {2}", file, line, msg)
End Sub

WriteOnly Property p As Integer
    Set(value As Integer)
        Notify(_p, value)
    End Set
End Property

Private _p As Integer

Sub Notify(Of T As IEquatable(Of T))(ByRef v1 As T, v2 As T,
        <CallerMemberName> Optional prop As String = Nothing)
    If v1 IsNot Nothing AndAlso v1.Equals(v2) Then Return
    If v1 Is Nothing AndAlso v2 Is Nothing Then Return
    v1 = v2
    RaiseEvent PropertyChanged(Me, New PropertyChangedEventArgs(prop))
End Sub
```

除了上面的可选参数外，Microsoft Visual Basic 还可识别某些其他可选参数（如果这些参数是从元数据导入的（即从 DLL 引用）：

* 从元数据中导入时，Visual Basic 还会将参数 `<Optional>` 视为可选参数：通过这种方式，可以导入具有可选参数但没有默认值的声明，即使无法表示此参数使用 `Optional` 关键字。

* 如果可选参数的属性 `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`，并且数值文本1或0具有到参数类型的转换，则编译器将使用 as 参数，如果 `Option Compare Text` 有效，则使用原义参数; 如果 `Optional Compare Binary` 有效，则使用原义参数。

* 如果可选参数的属性 `System.Runtime.CompilerServices.IDispatchConstantAttribute`，并且它的类型 @no__t 为-1，并且它未指定默认值，则编译器将使用参数 `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`。

* 如果可选参数的属性 `System.Runtime.CompilerServices.IUnknownConstantAttribute`，并且它的类型 @no__t 为-1，并且它未指定默认值，则编译器将使用参数 `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`。

* 如果可选参数的类型为 `Object`，并且它未指定默认值，则编译器将使用参数 `System.Reflection.Missing.Value`。

### <a name="conditional-methods"></a>条件方法

如果调用表达式引用的目标方法是不是接口成员的子程序，并且如果该方法具有一个或多个 `System.Diagnostics.ConditionalAttribute` 属性，则表达式的计算取决于在该点定义的条件编译常量。在源文件中。 特性的每个实例指定一个字符串，该字符串命名条件编译常量。 计算每个条件编译常量，就像它是条件编译语句的一部分一样。 如果常量的计算结果为 `True`，则在运行时将正常计算表达式。 如果常量的计算结果为 `False`，则不会计算表达式。

查找特性时，将检查可重写方法的派生程度最高的声明。

__纪录.__ 特性在函数或接口方法上无效，如果在这两种方法中指定，则将被忽略。 因此，条件方法将仅出现在调用语句中。

### <a name="type-argument-inference"></a>类型参数推理

如果在未指定类型实参的情况下调用具有类型参数的方法，则使用*类型参数推理*来尝试并推断调用的类型参数。 这允许在完全推断类型参数时使用更自然的语法来调用具有类型参数的方法。 例如，给定以下方法声明：

```vb
Module Util
    Function Choose(Of T)(b As Boolean, first As T, second As T) As T
        If b Then
            Return first
        Else
            Return second
        End If
    End Function
End Class
```

无需显式指定类型参数即可调用 `Choose` 方法：

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

通过类型参数推理，将 `Integer` 和 `String` 的类型参数从方法的自变量确定。

类型参数推理发生在对参数列表中的 lambda 方法或方法指针执行表达式重新分类*之前*，因为这两种表达式的重新分类可能需要已知参数的类型。  给定一组参数 `A1,...,An`、一组匹配参数 `P1,...,Pn` 和一组方法类型参数 `T1,...,Tn`，参数和方法类型参数之间的依赖关系将首先按如下方式收集：

* 如果 @no__t 为 @no__t 1，则不会生成任何依赖项。

* 如果 @no__t 为 lambda 方法，则 @no__t 的类型为构造委托类型或 `System.Linq.Expressions.Expression(Of T)`，其中 `T` 是构造的委托类型。

* 如果将从相应参数的类型推断出 lambda 方法参数的类型（`Pn`），并且参数的类型依赖于方法类型参数 `Tn`，则 `An` 依赖于 @no__t。

* 如果指定了 lambda 方法参数的类型，并且相应的参数 `Pn` 的类型依赖于方法类型参数 `Tn`，则 `Tn` 依赖于 `An`。

* 如果 @no__t 的返回类型依赖于方法类型参数 `Tn`，则 `Tn` 依赖于 `An`。

* 如果 `An` 是一个方法指针，并且 @no__t 的类型为构造委托类型，则为，

* 如果 @no__t 的返回类型依赖于方法类型参数 `Tn`，则 `Tn` 依赖于 `An`。

* 如果 @no__t 为构造类型，`Pn` 的类型依赖于方法类型参数 `Tn`，则 `Tn` 依赖于 @no__t。

* 否则，不会生成依赖项。

收集依赖项后，不会删除任何依赖关系的参数。 如果任何方法类型参数没有传出依赖项（即方法类型参数不依赖于某个参数），则类型推理将失败。 否则，其余的参数和方法类型参数将分组为强连接组件。 强连接组件是一组自变量和方法类型参数，可通过其他元素上的依赖项访问组件中的任何元素。

然后按拓扑顺序对强连接组件进行界定闭合排序和处理：

* 如果强类型组件只包含一个元素，

  * 如果该元素已标记为已完成，则跳过它。

  * 如果该元素是参数，则从参数向依赖于它的方法类型参数添加类型提示，并将该元素标记为完成。 如果参数是一个 lambda 方法，该方法的参数仍需要推断类型，则对这些参数的类型推断 `Object`。

  * 如果该元素是一个方法类型参数，则将该方法类型参数推断为参数类型提示中的主导类型，并将该元素标记为 complete。 如果类型提示在其上具有数组元素限制，则仅考虑在给定类型的数组之间有效的转换（即协变和内部数组转换）。 如果类型提示对其具有泛型参数限制，则仅考虑标识转换。 如果没有可选择的主导类型，推理将失败。 如果任何 lambda 方法参数类型依赖于此方法类型参数，则会将该类型传播到 lambda 方法。

* 如果强类型化组件包含多个元素，则组件将包含一个循环。

  * 对于属于组件中的元素的每个方法类型参数，如果方法类型参数依赖于未标记为 "已完成" 的参数，请将该依赖项转换为断言，在推理过程结束时将检查该依赖项。

  * 在确定强类型化组件的点重新启动推理过程。

如果所有方法类型参数的类型推理均已成功，则将检查已更改为断言的任何依赖项。 如果参数的类型可隐式转换为方法类型参数的推断类型，则断言成功。 如果断言失败，则类型参数推理将失败。

给定参数类型 `Ta` 表示参数 `A` 和参数类型 `Tp` 对于参数 `P`，按如下所示生成类型提示：

* 如果 `Tp` 不涉及任何方法类型参数，则不会生成任何提示。

* 如果 @no__t 为0，`Ta` 是相同级别的数组类型，则将 `Ta` 和 @no__t 3 替换为元素类型 `Ta` 和 @no__t 5 并使用数组元素限制重新启动此过程。

* 如果 @no__t 为方法类型参数，则 `Ta` 将添加为当前限制的类型提示（如果有）。

* 如果 @no__t 为 lambda 方法，而 `Tp` 是构造的委托类型或 `System.Linq.Expressions.Expression(Of T)`，其中 `T` 是构造的委托类型，则对于每个 lambda 方法参数类型 `TL` 和对应的委托参数类型 `TD`，请将 @no__t 替换为 @no__t-sql 和 `Tp` @no__t 为9，并重新启动该过程而无限制。 然后，将 `Ta` 替换为 lambda 方法的返回类型，并执行以下操作：

  * 如果 @no__t 为常规 lambda 方法，则将 `Tp` 替换为委托类型的返回类型;
  * 如果 @no__t 为 async lambda 方法，并且委托类型的返回类型具有形式 @no__t 对于某些 @no__t 为-1，则将 @no__t 3 替换为 `T`;
  * 如果 `A` 是迭代器 lambda 方法，并且委托类型的返回类型具有形式 @no__t 为-1 或 @no__t 为-2 （对于某些 @no__t），请将 @no__t 替换为 `T`。
  * 接下来，重新启动无限制的进程。

* 如果 @no__t 为方法指针，`Tp` 是构造的委托类型，请使用 `Tp` 的参数类型来确定所指向的哪种方法最适用于 @no__t 3。 如果有一个最适用的方法，请将 `Ta` 替换为该方法的返回类型，并 `Tp` 替换为委托类型的返回类型，并在没有限制的情况下重新启动该过程。

* 否则，`Tp` 必须为构造类型。 给定的 `TG`，@no__t 的泛型类型，

  * 如果 @no__t 为 `TG`，则从 `TG` 继承，或仅实现一次类型 @no__t，然后对于每个匹配的类型自变量 `Ta` 和 `Tp` 之间的第4个匹配的参数，将 @no__t 替换为 `Tax`，将 @no__t 替换为 1，并重新启动具有泛型参数限制的进程。

  * 否则，泛型方法的类型推理将失败。

类型推理的成功本身并不保证此方法适用。
