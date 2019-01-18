---
ms.openlocfilehash: 56482503cc5ca005d7d0f405874e778cea0c3ed5
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426639"
---
### <a name="overloaded-method-resolution"></a>解决方法的重载的方法

在实践中，用于确定重载决策规则旨在查找是"最靠近"提供的实际自变量的重载。 如果其参数类型与参数类型匹配的方法，则显然最接近该方法。 不包括，一种方法更接近于另一个如果所有的参数类型都是窄于 （或相同） 的另一种方法的参数类型。 如果这两种方法的参数比其他更窄，则没有办法来确定哪种方法是更接近于自变量。

__请注意。__ 重载决策不会考虑该方法的预期返回类型。 

另请注意，由于命名的参数语法、 实参和形参参数的顺序可能不能相同。

给定方法组，使用以下步骤确定在组中的参数列表最适用的方法。 如果应用特定步骤之后, 没有成员保留在集中，则出现编译时错误。 如果只有一个成员保留在该集中，该成员是最适用的成员。 步骤如下：

1.  首先，如果已不提供任何类型实参，则可适用于任何具有类型参数的方法类型推理。 如果类型推理成功的方法，则推断的类型参数用于该特定方法。 如果类型推理失败的方法，然后该方法也将从一组。

2.  接下来，消除从集是不可访问或不适用的所有成员 (部分[适用性到参数列表](overload-resolution.md#applicability-to-argument-list)) 到参数列表

3.  下一步，如果一个或多个参数都`AddressOf`或 lambda 表达式，然后计算*委托一放宽级别*按如下所示的每个此类参数。 如果最差 （最低） 委派中的一放宽级别`N`更糟的是最低的委托一放宽级别中比`M`，然后消除`N`集中。 委托松散级别如下所示：

    31. *错误委托一放宽级别*--如果`AddressOf`或 lambda 不能转换为委托类型。
  
    32. *收缩的返回类型或参数的委托松散*--如果参数是`AddressOf`或与声明的类型和从其返回类型转换为委托的 lambda 返回类型为收缩转换; 或如果参数为正则 lambda 和从返回表达式的任何转换为委托返回类型为收缩转换，或如果参数为异步 lambda 和返回的委托类型是`Task(Of T)`和从其返回到表达式的任何转换`T`为收缩转换; 或者，如果参数为迭代器 lambda，并且委托返回类型`IEnumerator(Of T)`或`IEnumerable(Of T)`并从其 yield 操作数的任何转换`T`为收缩转换。

    33. *扩大转换委托放宽了不带签名委派*--如果委托类型是`System.Delegate`或`System.MultiCastDelegate`或`System.Object`。

    34. *删除返回或参数的委托松散*--如果参数是`AddressOf`或与声明的返回类型和委托类型的 lambda 缺少返回类型; 或者如果自变量是一个 lambda 或表达式和委托类型的详细信息返回缺少返回类型;如果参数为或`AddressOf`或没有参数，该委托类型的 lambda 的形参。

    35. *扩大转换的返回类型的委托松散*--如果参数是`AddressOf`或声明的返回类型，lambda，并且没有扩大转换从其返回类型为的委托; 如果参数为正则 lambda 或其中扩大转换或具有至少一个扩大转换; 标识不从所有返回表达式转换为委托返回类型或者该参数是异步 lambda，委托是`Task(Of T)`或`Task`并从到的所有返回表达式转换`T` / `Object`分别是扩大转换或标识与至少一个扩大转换; 或者，如果参数为迭代器 lambda，并且委托`IEnumerator(Of T)`或`IEnumerable(Of T)`或`IEnumerator`或`IEnumerable`并从到的所有返回表达式转换`T` / `Object`扩大转换或具有至少一个扩大转换的标识。

    36. *标识委托松散*--如果参数是`AddressOf`或其中没有完全匹配委托的 lambda 扩大或收缩或删除参数或返回或生成。接下来，如果在集中的某些成员执行此操作不需要收缩转换适用于任何参数，则排除执行操作的所有成员。 例如：

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

4.  接下来，消除操作是根据收缩，如下所示。 (请注意，是否 Option Strict 为 On，然后需要收缩的所有成员具有已被都评判不适用 (部分[适用性到参数列表](overload-resolution.md#applicability-to-argument-list)) 和按步骤 2 中删除。)

    41. 如果集的一些实例成员只需要收缩的转换其中的自变量表达式类型是`Object`，则排除所有其他成员。
    42. 如果该集包含多个成员，这要求只能从收缩`Object`、 然后调用目标表达式重新分类为后期绑定方法访问 （和如果包含方法组的类型是一个接口，或如果任一给出错误相应的成员已扩展成员）。
    43. 如果有任何只需要从数值文字收缩的候选项，然后选择最具体的所有剩余候选人之间按以下步骤。 如果入选方要求仅收缩从数字文本，然后它将选取它作为结果的重载决策;否则，它是一个错误。

    __请注意。__ 此规则的理由是，如果程序是松散类型化 (即，大部分或全部变量声明为`Object`)，重载决策时，可能会很难从许多转换`Object`收缩。 而不是具有重载决策失败 （需要强类型化方法调用的参数） 的许多情况下，解决方法的相应重载方法来调用延迟到运行时中。 这样，也不会额外的强制转换的松散类型化调用。 令人遗憾的副作用，但是，是执行后期绑定调用需要强制转换为调用目标`Object`。 对于结构值，这意味着，必须将值装箱到一个临时。 如果最终调用的方法尝试更改结构的字段，该方法返回后将丢失此更改。 接口不在此特殊规则，因为后期绑定始终解析对运行时类或结构类型，这可以使它们实现的接口的成员与不同的名称的成员。

5.  接下来，如果无需收缩集中保留的任何实例方法，则排除从集中的所有扩展方法。 例如：

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

    __请注意。__ 如果有适用的实例方法，以保证的添加导入 （这可能会将新的扩展方法引入作用域） 不会导致调用上现有的实例方法来重新绑定到扩展方法，将忽略扩展方法。 考虑到的某些扩展方法 （即定义接口和/或类型参数上的） 的广泛范围，这是与绑定到扩展方法更安全的方法。

6.  接下来，如果给定的集的任何两个成员`M`和`N`，`M`更多*特定*(部分[特异性成员/类型提供的参数列表的](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) 比`N`给定的参数列表，消除`N`集中。 如果多个成员保留在该集中的其余成员不是同样特定的给定参数列表，会导致编译时错误。

7.  否则，给定任意两个集的成员，`M`和`N`，将以下能够打破平局的规则，按顺序应用：

    71. 如果`M`不具有 ParamArray 参数，但`N`，或如果两者，但`M`将更少的参数传递到比 ParamArray 参数`N`，然后消除`N`集中。 例如：

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

        上面的示例生成以下输出：

        ```
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        __请注意。__ 当类声明具有 paramarray 参数的方法时，它不少见还包含一些扩展形式作为常规方法。 通过这样做可以避免的数组分配将调用具有 paramarray 参数的方法的扩展形式出现的实例。

    72. 如果`M`比的派生程度更大类型中定义`N`，消除`N`集中。 例如：

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

        此规则也适用于扩展方法定义的类型。 例如：

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

    73. 如果`M`并`N`扩展方法和的目标类型`M`的类或结构和目标类型的`N`是一个接口，消除`N`集中。 例如：

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

    74. 如果`M`并`N`扩展方法，和的目标类型`M`并`N`是相同的类型参数替换、 和的目标类型后`M`之前类型参数替换不包含类型参数化的目标类型`N`匹配，则具有较少的类型参数的目标类型比`N`，消除`N`集中。 例如：

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

    75. 在类型之前，参数都已替换，如果`M`是*小于泛型*(部分[泛型](overload-resolution.md#genericity)) 比`N`，消除`N`集中。

    76. 如果`M`不是扩展方法和`N`，消除`N`集中。

    77. 如果`M`并`N`扩展方法和`M`之前找到`N`(部分[扩展方法集合](overload-resolution.md#extension-method-collection))，消除`N`集中。 例如：

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

        如果在同一步骤中找到的扩展方法，这些扩展方法是不明确。 始终会产生的标准模块包含扩展方法和调用扩展方法，就像它是常规成员名称歧义的调用。 例如：

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

    78. 如果`M`并`N`必需的类型推断来生成类型参数和`M`不需要确定的基准任何的类型类型参数 （即每个到单个类型推断的类型参数），但`N`一样，消除`N`集中。

        __请注意。__ 此规则可确保已成功在早期版本中 （其中推断类型参数的多个类型会导致错误） 的重载决策，继续生成相同的结果。

    79. 如果重载解析措施来解决从委托创建表达式的目标`AddressOf`表达式，并且这两个委托并`M`是函数时`N`是一个子例程，消除`N`集中。 同样，如果这两个委托和`M`是子例程，虽然`N`是函数，消除`N`集中。

    710. 如果`M`未使用任何可选参数的默认值来代替显式参数，但`N`这样做，则排除`N`集中。

    711. 在类型之前，参数都已替换，如果`M`具有*的泛型的更深入地*(部分[泛型](overload-resolution.md#genericity)) 比`N`，然后消除`N`集中。

8. 否则为调用不明确，则发生编译时错误。

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a>特异性的成员/类型提供的参数列表

成员`M`被视为*同样特定*作为`N`，给定一个参数列表`A`，如果它们的签名相同或如果每个参数键入`M`等同于相应中的参数类型`N`。

__请注意。__ 两个成员可能具有相同的签名，因为扩展方法的方法组中。 两个成员可以也是同样特定但不是具有类型参数或参数组扩展由于相同的签名。

成员`M`被视为*更具体*比`N`如果它们的签名不同，并且至少一个参数中键入`M`比中的参数类型更具体`N`，且未中的参数类型`N`中的参数类型比更具体`M`。 给定一对参数`Mj`和`Nj`相匹配参数`Aj`的类型`Mj`被视为*更具体*比的类型`Nj`如果以下项之一条件为 true:

* 存在从类型的扩大转换`Mj`为类型`Nj`。 (__注意。__ 因为参数类型进行比较的而不考虑实际自变量在这种情况下，扩大转换从常量表达式为数值类型值适用于不被视为在这种情况下。）

* `Aj` 为字面值`0`，`Mj`为数值类型和`Nj`是一个枚举的类型。 (__注意。__ 此规则是必需的因为文本`0`加宽到任何枚举类型。 一个枚举的类型加宽到其基础类型，因为这意味着，在重载决策`0`将默认情况下，首选枚举的类型而不是数值类型。 我们收到了大量反馈，此行为是有悖常理）。

* `Mj` 和`Nj`均为数值类型，并`Mj`来自早于`Nj`列表中`Byte`， `SByte`， `Short`， `UShort`， `Integer`， `UInteger`， `Long`， `ULong`, `Decimal`, `Single`, `Double`. (__注意。__ 介绍了数字类型的规则是很有用，因为特定大小的有符号和无符号数值类型只能有收缩它们之间的转换。 上述规则将为支持多个"自然的"数值类型的两个类型之间平分处中断。 这是非常重要时执行操作的类型的重载决策的加宽到这两个符号和无符号数值类型的特定大小，例如，适用于两者的数字参数。）

* `Mj` 和`Nj`函数类型和返回类型是委托`Mj`的返回类型比更具体`Nj`如果`Aj`归类为 lambda 方法并`Mj`或`Nj`是`System.Linq.Expressions.Expression(Of T)`，然后（假设它委托类型） 的类型的类型参数替换为要比较的类型。

* `Mj` 类型相同`Aj`，和`Nj`不是。 (__注意。__ 有趣的是要注意前面的规则略有不同于 C#、 C# 中要求的委托函数类型进行比较的返回类型，而 Visual Basic 不前具有相同的参数列表。）

#### <a name="genericity"></a>泛型

成员`M`被确定为*小于泛型*比成员`N`，如下所示：

1. 如果匹配的参数的每个对`Mj`和`Nj`，`Mj`更短或平均比泛型`Nj`方法，以及至少一个类型参数对于`Mj`是低泛型类型的方面该方法的参数。
2. 否则为如果匹配的参数的每个对`Mj`并`Nj`，`Mj`更短或平均比泛型`Nj`与类型参数类型，以及至少一个有关`Mj`是低泛型类型的方面参数的类型，然后`M`是比低泛型`N`。

参数`M`被视为同样通用的参数`N`如果其类型`Mt`和`Nt`都引用类型参数或都不引用类型参数。 `M` 被视为可比低泛型`N`如果`Mt`不引用类型参数和`Nt`does。

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

已修复科期间的扩展方法类型参数被视为类型参数的类型，不是类型参数的方法。 例如：

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

#### <a name="depth-of-genericity"></a>泛型的深度

成员`M`被确定有*的泛型的更深入地*比成员`N`如果匹配的参数的每个对`Mj`和`Nj`，`Mj`具有更高版本或等于*的泛型深度*比`Nj`，且至少一个`Mj`是更高版本的深度为泛型。 泛型的深度定义，如下所示：

* 类型参数之外的任何内容有更深入的泛型类型参数;

* 以递归方式，如果至少一个类型参数具有更深入地泛型的并且没有类型参数具有更少深度比相应的类型构造的类型具有比另一个构造类型 （具有相同的类型参数） 更深入地泛型中的其他参数。

* 数组类型具有比 （具有相同维数的） 的另一个数组类型的泛型的更深入地，如果第一个元素类型具有比第二个元素类型的泛型的更深入地。

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

### <a name="applicability-to-argument-list"></a>适用于参数列表

一种方法是*适用*到一组类型参数，位置自变量和命名的参数，如果可以调用该方法使用自变量列表。 与参数列表匹配的自变量列表，如下所示：

1. 首先，匹配每个位置参数在方法参数的列表顺序。 如果有多个位置参数，而不是参数，最后一个参数不是一个参数数组，该方法不适用。 否则，要匹配的位置参数的数目的参数组元素类型的参数被展开 paramarray 参数。 如果省略位置自变量，会转到一个参数数组，该方法不适用。
2. 接下来，与具有给定名称的参数为每个命名的参数相匹配。 如果其中一个命名参数无法匹配、 匹配 paramarray 参数，或与已与另一个位置或命名参数匹配的参数，则该方法不适用。
3. 接下来，如果已指定类型参数，它们被匹配的类型参数列表。 如果两个列表不具有相同的元素数，方法不适用，除非类型参数列表为空。 如果类型参数列表为空，使用类型推理来尝试推断类型参数列表。 如果类型推断失败，则该方法不适用。 否则，类型参数填充到签名中的类型参数位置。如果不匹配的参数不是可选的则该方法不适用。
4. 如果参数表达式不是隐式转换的参数类型为它们匹配，则该方法不适用。
5. 如果参数是引用传递，并且不是从参数类型的隐式转换为自变量的类型，然后该方法不适用。
6. 如果类型参数违反 （包括步骤 3 中的推断的类型参数） 的方法的约束，该方法不适用。 例如：

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

如果单个自变量表达式匹配 paramarray 参数并且参数表达式的类型转换为 paramarray 参数的类型和参数数组元素类型，方法是适用于这两个其扩展和未扩展窗体，有两个例外。 如果从自变量表达式的类型到 paramarray 类型的转换收缩转换，然后该方法才适用以展开形式。 如果参数表达式为字面值`Nothing`，然后方法只是适用于其未扩展的窗体。 例如：

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

上面的示例生成以下输出：

```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

中的第一个和最后一个调用`F`的范式`F`是适用的因为存在从实参类型到形参类型扩大转换 (两个均为类型`Object()`)，并作为常规值传递自变量参数。 在第二个和第三个调用的范式`F`因为没有扩大转换存在从实参类型到形参类型不适用 (从转换`Object`到`Object()`收缩)。 但是的展开的形式`F`已适用，以及一个元素`Object()`创建的调用。 使用给定的参数值初始化数组的单个元素 (其本身是对的引用`Object()`)。

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a>传递自变量，并选择可选参数的自变量

如果参数为值参数，则匹配的参数表达式必须分类为一个值。 值转换为参数的类型，并在运行时作为参数传入。 如果参数为引用参数和匹配的自变量表达式分类为其类型为的参数相同的变量，然后对该变量的引用是在作为参数传递在运行时。

否则，如果匹配的参数表达式分类为变量、 值或属性访问，被分配临时变量的参数的类型。 之前在运行时方法调用，自变量表达式将重新分类为一个值，转换为类型参数，并分配给临时变量。 然后对临时变量的引用中传递作为参数。 如果自变量表达式分类为变量或属性访问评估方法调用后，临时变量分配给变量的表达式或属性访问表达式。 如果不具有属性访问表达式`Set`访问器，然后分配不会执行。

对于参数尚未提供的可选参数，编译器会选取参数如下所述。 在所有情况下它测试参数类型针对后泛型类型替换。

* 如果可选参数具有的属性`System.Runtime.CompilerServices.CallerLineNumber`，则调用不从源代码中的位置和表示该位置的行号的数值标识符具有内部函数转换的参数类型，然后使用数字文本。 如果调用跨多个行，选择要使用哪些行是依赖于实现的。

* 如果可选参数具有的属性`System.Runtime.CompilerServices.CallerFilePath`，则调用不从源代码中的位置和表示该位置的文件路径的字符串文字具有内部函数转换的参数类型，然后使用字符串文本。 文件路径的格式是依赖于实现的。

* 如果可选参数具有的属性`System.Runtime.CompilerServices.CallerMemberName`，并调用体内的类型成员或在属性应用于该类型成员的任何部分中，且是表示该成员名称的字符串文字到参数的内部函数转换类型，则使用字符串文本。 对于属性访问器或自定义事件处理程序的一部分的调用，然后使用的成员名称是属性或事件本身。 为属于运算符或构造函数的调用，则特定于实现的名称使用。

如果没有更高版本的应用，则使用可选参数的默认值 (或`Nothing`如果不提供任何默认值)。 如果超过其中一个更高版本的应用，然后选择要使用的是依赖于实现的。

`CallerLineNumber`和`CallerFilePath`属性可用于日志记录。 `CallerMemberName`可用于实现`INotifyPropertyChanged`。 下面是示例。

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

除了以上的可选参数，Microsoft Visual Basic 还可识别某些其他可选参数如果它们从元数据 （即从 DLL 引用） 导入：

* 从元数据导入，在 Visual Basic 还将参数`<Optional>`，指示该参数是可选： 可以导入声明它具有一个可选参数，但没有默认值，即使它不能是这种方式使用表示`Optional`关键字。

* 如果可选参数具有的属性`Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`，并且数字文本 1 或 0 具有转换为参数类型，则编译器将作为参数使用的文本 1 if`Option Compare Text`中的效果，或者文本 0 如果`Optional Compare Binary`生效。

* 如果可选参数具有的属性`System.Runtime.CompilerServices.IDispatchConstantAttribute`，并具有类型`Object`，和未指定默认值，则编译器将使用该参数`New System.Runtime.InteropServices.DispatchWrapper(Nothing)`。

* 如果可选参数具有的属性`System.Runtime.CompilerServices.IUnknownConstantAttribute`，并具有类型`Object`，和未指定默认值，则编译器将使用该参数`New System.Runtime.InteropServices.UnknownWrapper(Nothing)`。

* 如果可选参数具有类型`Object`，和未指定默认值，则编译器将使用该参数`System.Reflection.Missing.Value`。

### <a name="conditional-methods"></a>条件方法

如果调用表达式所引用的目标方法是不是接口成员的子例程，该方法具有一个或多个`System.Diagnostics.ConditionalAttribute`属性，表达式的计算取决于在定义的条件编译常量源文件中该点。 该属性的每个实例指定了条件编译常量的名称的字符串。 每个条件编译常量计算，就好像条件编译语句的一部分。 如果该常量的计算结果为`True`，在运行时通常计算表达式。 如果该常量的计算结果为`False`，根本不计算表达式。

当查找该属性，将检查派生程度最高的可重写方法声明。

__请注意。__ 特性不是函数或接口方法上无效，如果这两种方法上指定，则忽略。 因此，条件方法只会在调用语句中。

### <a name="type-argument-inference"></a>类型自变量推理

具有类型参数的方法调用而无需指定类型参数时*类型参数推理*用于尝试并推断类型参数的调用。 这样，若要用来调用具有类型参数的方法时可以不费力地推断出类型参数的语法更自然。 例如，给定以下方法声明：

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

可以调用`Choose`而无需显式指定类型参数的方法：

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

通过类型参数推断的类型实参`Integer`和`String`将根据对方法的参数。

类型实参推理发生*之前*由于重新分类的这些两种类型的表达式可能需要的类型，在 lambda 方法或方法参数列表中的指针上执行的表达式重新分类为已知的参数。  给定一组参数`A1,...,An`，一组匹配参数`P1,...,Pn`和一组方法类型参数`T1,...,Tn`，自变量和方法类型参数之间的依赖关系首先收集，如下所示：

* 如果`An`是`Nothing`文本，则会生成没有依赖关系。

* 如果`An`是的类型和 lambda 方法`Pn`是构造的委托类型或`System.Linq.Expressions.Expression(Of T)`，其中`T`是构造的委托类型，

* 如果将从相应的参数的类型推断出 lambda 方法参数的类型`Pn`，和参数的类型取决于方法类型参数`Tn`，然后`An`依赖`Tn`。

* 如果指定的 lambda 方法参数的类型和相应的参数的类型`Pn`取决于方法类型参数`Tn`，然后`Tn`依赖`An`。

* 如果的返回类型的`Pn`取决于方法类型参数`Tn`，然后`Tn`依赖`An`。

* 如果`An`是方法指针的类型和`Pn`是构造的委托类型，

* 如果的返回类型的`Pn`取决于方法类型参数`Tn`，然后`Tn`依赖`An`。

* 如果`Pn`是一个构造的类型的类型和`Pn`取决于方法类型参数`Tn`，然后`Tn`依赖`An`。

* 否则，生成无依赖关系。

收集依赖项之后, 将消除没有依赖关系的任何自变量。 如果任何方法类型参数 （即方法类型参数不依赖于自变量） 没有传出依赖项，则类型推理失败。 否则，其余的参数和方法类型参数被分组到紧密相连的组件。 紧密相连的组件是一组参数以及方法类型参数，该组件中的任何元素是可通过在其他元素上的依赖项。

然后，紧密相连的组件为拓扑结构上排序，按拓扑顺序处理：

* 如果强类型化的组件包含仅有一个元素

  * 如果该元素具有已被标记为完成，跳过它。

  * 如果该元素是自变量，然后将添加类型提示从参数到依赖于它，并将标记为已完成的元素的方法类型参数。 如果参数为具有参数，仍需要推断类型的 lambda 方法，然后推断`Object`有关这些参数的类型。

  * 如果该元素为方法类型参数，然后推断方法类型参数在参数类型提示之间的基准类型并将标记为已完成的元素。 类型提示对其具有数组元素限制，如果给定类型的数组之间有效的转换被视为 （即协变和内部数组转换）。 如果类型提示上有一个泛型参数的限制，则认为是仅标识转换。 如果选择没有基准类型推理失败。 如果任何 lambda 方法自变量类型依赖于此方法类型参数，该类型被传播到 lambda 方法。

* 如果强类型化的组件包含多个元素，该组件包含循环。

  * 为每个方法类型参数的组件中的元素，如果方法类型参数依赖于未标记为完成的参数，则将该依赖项转换为将进行检查，推断过程末尾的断言。

  * 重新启动的强类型化的组件已确定的点在推断过程。

如果类型推理成功的所有方法类型参数，则检查所做的更改到断言任何依赖项。 如果自变量的类型是隐式转换为方法类型参数的推断类型，成功断言。 如果断言失败，则类型自变量推理失败。

给定自变量类型`Ta`的参数`A`和参数类型`Tp`参数`P`，类型提示生成，如下所示：

* 如果`Tp`不涉及任何方法类型参数，则生成无提示。

* 如果`Tp`并`Ta`数组类型的相同的排名，然后将为`Ta`并`Tp`与元素类型的`Ta`和`Tp`并重新启动此流程的数组元素限制。

* 如果`Tp`为方法类型参数，则`Ta`如果任何添加为当前的限制，类型提示。

* 如果`A`是一种 lambda 方法并`Tp`是构造的委托类型或`System.Linq.Expressions.Expression(Of T)`，其中`T`是每个 lambda 方法参数类型的构造的委托类型，`TL`和相应的委托参数类型`TD`，替换`Ta`与`TL`并`Tp`与`TD`和重新启动该进程没有限制。 然后，替换`Ta`与 lambda 方法的返回类型和：

  * 如果`A`是一个正则 lambda 方法，将为`Tp`与委托类型; 的返回类型
  * 如果`A`是一个异步 lambda 方法和委托类型的返回类型具有窗体`Task(Of T)`某些`T`，将为`Tp`这`T`;
  * 如果`A`是一种迭代器 lambda 方法和委托类型的返回类型具有窗体`IEnumerator(Of T)`或`IEnumerable(Of T)`某些`T`，替换`Tp`这`T`。
  * 接下来，重新启动具有无限制的进程。

* 如果`A`是一个方法指针和`Tp`是构造委托类型，请使用的参数类型`Tp`来确定哪种方法指向是最适用于`Tp`。 如果是最适用的方法，请更换`Ta`与该方法的返回类型和`Tp`与委托类型和重启具有无限制的进程的返回类型。

* 否则为`Tp`必须是一个构造的类型。 给定`TG`的泛型类型`Tp`，

  * 如果`Ta`是`TG`，继承自`TG`，或者实现类型`TG`恰好一次，然后针对每个匹配的类型实参`Tax`从`Ta`并`Tpx`从`Tp`，替换`Ta`与`Tax`并`Tp`与`Tpx`并重新启动具有泛型参数限制的进程。

  * 否则，泛型方法的类型推理失败。

成功的类型推断不，在其本身而言，保证的方法是适用。
