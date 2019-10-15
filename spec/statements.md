---
ms.openlocfilehash: e49e116e60e724bcd8f1148c8aad9d11dfc92e74
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306059"
---
# <a name="statements"></a>语句

语句表示可执行代码。

```antlr
Statement
    : LabelDeclarationStatement
    | LocalDeclarationStatement
    | WithStatement
    | SyncLockStatement
    | EventStatement
    | AssignmentStatement
    | InvocationStatement
    | ConditionalStatement
    | LoopStatement
    | ErrorHandlingStatement
    | BranchStatement
    | ArrayHandlingStatement
    | UsingStatement
    | AwaitStatement
    | YieldStatement
    ;
```

__纪录.__ Microsoft Visual Basic 编译器仅允许以关键字或标识符开头的语句。 例如，允许调用语句 "`Call (Console).WriteLine`"，但调用语句 "`(Console).WriteLine`" 不是。

## <a name="control-flow"></a>控制流

*控制流*是执行语句和表达式的顺序。 执行顺序取决于特定的语句或表达式。

例如，在计算加法运算符（第[加法运算符](expressions.md#addition-operator)）时，首先计算左操作数，然后计算右操作数，然后计算运算符本身。 块通过首先执行其第一个标签，然后通过块的语句逐个执行，来执行（部分[块和标签](statements.md#blocks-and-labels)）。

此顺序中的隐式是*控制点*的概念，即要执行的下一个操作。 如果调用了方法（或 "调用"），则我们说它创建了方法的*实例*。 方法实例由其自己的方法的参数、局部变量和其自己的控制点的副本组成。

### <a name="regular-methods"></a>常规方法

下面是常规方法的示例

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

调用常规方法时，

1. 首先，创建该方法的实例，该实例特定于调用。 此实例包含方法的所有参数和局部变量的副本。
2. 然后，将其所有参数初始化为提供的值，并将其所有局部变量初始化为其类型的默认值。
3. 对于 `Function`，隐式局部变量也被初始化称为函数*返回变量*，其名称为函数的名称，其类型为函数的返回类型，其初始值为其类型的默认值。
4. 然后，在方法体的第一条语句处设置方法实例的控制点，方法体会立即开始从该位置执行（节[块和标签](statements.md#blocks-and-labels)）。

当控制流在正常情况下退出方法体时，会通过到达 `End Function` 或 `End Sub` 标记其末尾，或者通过显式 `Return` 或 @no__t 的语句控制流返回到方法实例的调用方。 如果存在函数返回变量，则调用的结果是此变量的最终值。

当控制流通过未经处理的异常退出方法体时，该异常会传播到调用方。

当控制流退出后，不再有任何对方法实例的实时引用。 如果方法实例仅保留对其局部变量或参数副本的引用，则可能会对它们进行垃圾回收。

### <a name="iterator-methods"></a>迭代器方法

迭代器方法用作生成序列的一种简便方法，可由 `For Each` 语句使用。 迭代器方法使用 `Yield` 语句（节[Yield 语句](statements.md#yield-statement)）来提供序列的元素。 （没有 `Yield` 语句的迭代器方法将生成一个空序列）。 下面是迭代器方法的示例：

```vb
Iterator Function Test() As IEnumerable(Of Integer)
    Console.WriteLine("hello")
    Yield 1
    Yield 2
End Function

Dim en = Test()
For Each x In en          ' prints "hello" before the first x
    Console.WriteLine(x)  ' prints "1" and then "2"
Next
```

如果调用的迭代器方法的返回类型为 `IEnumerator(Of T)`，

1. 首先，创建一个迭代器方法的实例，该实例特定于调用。 此实例包含方法的所有参数和局部变量的副本。
2. 然后，将其所有参数初始化为提供的值，并将其所有局部变量初始化为其类型的默认值。
3. 隐式局部变量也被初始化称为*迭代器当前变量*，该变量的类型为 `T`，其初始值为其类型的默认值。
4. 然后，在方法体的第一条语句处设置方法实例的控制点。
5. 然后创建一个*迭代器对象*，该对象与此方法实例相关联。 迭代器对象实现声明的返回类型，并具有如下所述的行为。
6. 然后，在调用方中*立即*恢复控制流，并且调用的结果是迭代器对象。 请注意，此传输在不退出迭代器方法实例的情况下完成，并且不会导致执行 finally 处理程序。 方法实例仍由迭代器对象引用，并且将不会对其进行垃圾回收，只要存在对迭代器对象的实时引用。

当访问迭代器对象的 `Current` 属性时，将返回调用的*当前变量*。

调用迭代器对象的 @no__t 0 方法时，调用不会创建新的方法实例。 而是使用现有的方法实例（及其控制点和局部变量和参数）-第一次调用迭代器方法时创建的实例。 控制流在该方法实例的控制点处继续执行，并按正常方式继续执行迭代器方法的正文。

调用迭代器对象的 `Dispose` 方法时，将再次使用现有的方法实例。 控制流在该方法实例的控制点处恢复，但随后会立即表现为 `Exit Function` 语句是下一操作。

如果对迭代器对象上的 `MoveNext` 或 @no__t 的以前的所有调用都已返回到其调用方，则在迭代器对象上调用 @no__t 0 或 `Dispose` 的上述行为说明仅适用于。 如果没有，则行为是不确定的。

当控制流在正常情况下退出迭代器方法体时--通过到达 `End Function` 来标记其结束，或者通过显式 `Return` 或 `Exit` 语句，必须在调用方的 `MoveNext` 或 `Dispose` 函数的上下文中执行此操作。要恢复迭代器方法实例的对象，并且该对象将使用首次调用迭代器方法时创建的方法实例。 该实例的控制点将保留在 `End Function` 语句上，并在调用方恢复控制流;如果已通过调用 `MoveNext` 恢复，则会将值 `False` 返回给调用方。

当控制流通过未经处理的异常退出迭代器方法体时，异常将传播到调用方，后者再次为 `MoveNext` 或 `Dispose` 的调用。

对于 iterator 函数的其他可能的返回类型，

* 调用迭代器方法时，如果调用的返回类型对某些 @no__t 为 @no__t 为0，则将首先创建一个实例，该实例特定于方法中所有参数的调用方方法，并使用提供的值对其进行初始化。 调用的结果是实现返回类型的对象。 如果调用此对象的 @no__t 0 方法，则它将创建一个实例，该实例特定于该方法中所有参数和局部变量的调用 @no__t。 它将参数初始化为已保存的值，并继续执行以上迭代器方法。
* 调用迭代器方法时，如果调用的返回类型为非泛型接口 `IEnumerator`，则该行为与 `IEnumerator(Of Object)` 的行为完全相同。
* 调用迭代器方法时，如果调用的返回类型为非泛型接口 `IEnumerable`，则该行为与 `IEnumerable(Of Object)` 的行为完全相同。

### <a name="async-methods"></a>异步方法

异步方法是执行长时间运行的工作的一种简便方法，无需阻止应用程序的 UI。 异步是异步的*异步*方法，这意味着异步方法的调用方会立即继续执行，但异步方法的实例的最终完成可能会在将来的某个时间之后发生。 按照约定，异步方法用后缀 "Async" 命名。

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

__纪录.__ 异步方法*不*会在后台线程上运行。 相反，它们允许方法通过 `Await` 运算符来挂起自身，并计划在响应某个事件时恢复自身。

调用 async 方法时

1. 首先，创建一个异步方法的实例，该实例特定于调用。 此实例包含方法的所有参数和局部变量的副本。
2. 然后，将其所有参数初始化为提供的值，并将其所有局部变量初始化为其类型的默认值。
3. 如果异步方法的返回类型为 `Task(Of T)` （对于某些 @no__t 为-1），则隐式局部变量也被初始化称为*任务返回变量*，该变量的类型为 `T`，其初始值为 `T` 的默认值。
4. 如果异步方法为 `Function`，并且返回类型为 `Task`，或 @no__t 为-2 （对于某些 `T`），则为与当前调用关联的、隐式创建的该类型的对象。 这称为*async 对象*，表示将通过执行 async 方法的实例完成的未来工作。 当控件在此 async 方法实例的调用方中恢复时，调用方将接收此 async 对象作为其调用结果。
5. 然后，在异步方法主体的第一条语句处设置实例的控制点，并立即开始从其中执行方法体（部分[块和标签](statements.md#blocks-and-labels)）。

__恢复委托和当前调用方__

如[Await 运算符](expressions.md#await-operator)部分中所述，执行 @no__t 1 表达式可以挂起方法实例的控制点，使控制流离开其他位置。 稍后，控制流可以通过调用*恢复委托*在同一实例的控制点上恢复。 请注意，此挂起在不退出 async 方法的情况下完成，并且不会导致执行 finally 处理程序。 此方法实例仍由恢复委托和 `Task` 或 @no__t 结果（如果有）引用，并且将不会进行垃圾回收，这是因为存在对委托或结果的实时引用。

假设语句 `Dim x = Await WorkAsync()`，这一点对于以下情况很有帮助：

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

在下面的示例中，方法实例的*当前调用方*定义为原始调用方或恢复委托的调用方，两者都是最新的。

当控制流退出 async 方法体时--通过到达 `End Sub` 或 `End Function` 标记其结束时，或者通过显式的 `Return` 或 `Exit` 语句或未经处理的异常，将实例的控制点设置为方法的末尾。 然后，行为取决于异步方法的返回类型。

* 对于返回类型为 @no__t 的 @no__t 0：
  1. 如果控制流通过未经处理的异常退出，则异步对象的状态将设置为 `TaskStatus.Faulted`，其 @no__t 属性设置为异常（除外：某些实现定义的异常，如 `OperationCanceledException` 将其更改为 `TaskStatus.Canceled`）。 当前调用方中的控制流恢复。
  2. 否则，async 对象的状态将设置为 `TaskStatus.Completed`。 当前调用方中的控制流恢复。

     （__注意：__ 整个任务点以及异步方法的含义是什么，这是因为当任务完成时，任何等待它的方法都将执行其恢复委托，即，它们将被取消阻止。）

* 对于某些 `T` 的返回类型为 @no__t 的 @no__t 0：行为如上所示，除了在非异常情况下，async 对象的 `Result` 属性还设置为任务返回变量的最终值。

* 对于 `Async Sub`：
  1. 如果控制流在未经处理的异常中退出，则该异常将以某种特定于实现的方式传播到环境。 当前调用方中的控制流恢复。
  2. 否则，控制流只需在当前调用方中恢复即可。

#### <a name="async-sub"></a>Async Sub

有一些特定于 Microsoft 的 @no__t 的行为。

如果在调用开始时 `SynchronizationContext.Current` @no__t 为-1，则异步 Sub 中的任何未经处理的异常都将发布到 Threadpool。

如果在调用开始时 `SynchronizationContext.Current` 不 `Nothing`，则在该方法开始之前，在该上下文上调用 @no__t，并在结束后 @no__t 为3。 此外，将会发布在同步上下文中重新引发的任何未经处理的异常。

这意味着，在 UI 应用程序中，对于在 UI 线程上调用的 @no__t 0，它无法处理的任何异常都将 reposted UI 线程。

#### <a name="mutable-structures-in-async-and-iterator-methods"></a>异步和迭代器方法中的可变结构

可变结构一般被视为错误的做法，异步或迭代器方法不支持。 特别是，结构中的异步或迭代器方法的每个调用都将隐式地在调用时复制的该结构的*副本*上运行。 例如，

```vb
Structure S
       Dim x As Integer
       Async Sub Mutate()
           x = 2
       End Sub
End Structure

Dim s As New S With {.x = 1}
s.Mutate()
Console.WriteLine(s.x)   ' prints "1"
```

### <a name="blocks-and-labels"></a>块和标签

一组可执行语句称为语句块。 语句块的执行从块中的第一个语句开始。 执行语句后，将执行词法顺序中的下一条语句，除非语句将执行转移到其他位置或发生异常。

在语句块中，对逻辑行的语句相除对于标签声明语句除外是不重要的。 标签是标识语句块内特定位置的标识符，该语句块可用作分支语句的目标，例如 `GoTo`。

```antlr
Block
    : Statements*
    ;

LabelDeclarationStatement
    : LabelName ':'
    ;

LabelName
    : Identifier
    | IntLiteral
    ;

Statements
    : Statement? ( ':' Statement? )*
    ;
```


标签声明语句必须出现在逻辑行的开头，并且标签可以是标识符或整数文本。 由于标签声明语句和调用语句都可以包含单个标识符，因此，本地行开头的单个标识符始终被视为标签声明语句。 标签声明语句必须始终后跟一个冒号，即使在同一逻辑行上没有语句。

标签具有自己的声明空间，不会干扰其他标识符。 下面的示例是有效的，并且使用名称变量 `x` 均作为参数，并使用作为标签。

```vb
Function F(x As Integer) As Integer
    If x >= 0 Then
        GoTo x
    End If
    x = -x
x: 
    Return x
End Function
```

标签的作用域是包含它的方法的正文。

为方便起见，在此规范中，涉及多个 substatements 的语句生产将被视为单个生产，即使 substatements 各自在标记的行中也是如此。


### <a name="local-variables-and-parameters"></a>局部变量和参数

前面几节详细说明了如何和何时创建方法实例，以及如何将方法实例与方法的局部变量和参数一起使用。 此外，每次输入循环的主体时，都会在该循环中声明的每个局部变量（如 "[循环语句](statements.md#loop-statements)" 一节中所述）创建一个新的副本，并且该方法实例现在包含其本地变量（而不是以前的副本）的此副本。

所有局部变量都初始化为其类型的默认值。 局部变量和参数始终可公开访问。 在其声明之前的文本位置引用本地变量是错误的，如下面的示例所示：

```vb
Class A
    Private i As Integer = 0

    Sub F()
        i = 1
        Dim i As Integer       ' Error, use precedes declaration.
        i = 2
    End Sub

    Sub G()
        Dim a As Integer = 1
        Dim b As Integer = a   ' This is valid.
    End Sub
End Class
```

在上面的 `F` 方法中，第一次分配给 @no__t 的第1项并未引用在外部作用域中声明的字段。 相反，它引用局部变量，并且它是错误的，因为它在此变量的声明之前位于变量之前。 在 `G` 方法中，后面的变量声明引用在同一局部变量声明内的早期变量声明中声明的局部变量。

方法中的每个块都为局部变量创建声明空间。 名称通过方法体中的局部变量声明和方法的参数列表引入到此声明空间，该方法将名称引入最外层块的声明空间。 块不允许通过嵌套隐藏名称：在块中声明名称后，可能不会在任何嵌套块中重新声明该名称。

因此，在下面的示例中，由于名称 `i` 是在外部块中声明的，因此不能在内部块中重新声明，因此在下面的示例中，`F` 和 @no__t 方法是错误的。 但 `H` 和 @no__t 1 方法是有效的，因为这两个 @no__t 在单独的非嵌套块中声明。

```vb
Class A
    Sub F()
        Dim i As Integer = 0
        If True Then
               Dim i As Integer = 1
        End If
    End Sub

    Sub G()
        If True Then
            Dim i As Integer = 0
        End If
        Dim i As Integer = 1
    End Sub 

    Sub H()
        If True Then
            Dim i As Integer = 0
        End If
        If True Then
            Dim i As Integer = 1
        End If
    End Sub

    Sub I() 
        For i As Integer = 0 To 9
            H()
        Next i

        For i As Integer = 0 To 9
            H()
        Next i
    End Sub 
End Class
```

如果该方法是一个函数，则在方法体的声明空间中隐式声明一个特殊的局部变量，该局部变量的名称与表示该函数的返回值的方法的名称相同。 在表达式中使用时，局部变量具有特殊名称解析语义。 如果在需要将表达式归类为方法组的上下文（如调用表达式）中使用本地变量，则该名称将解析为函数而不是局部变量。 例如：

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

使用括号可能导致不明确的情况（例如 `F(1)`，其中 `F` 是返回类型为一维数组的函数）;在所有不明确的情况下，名称解析为函数而不是局部变量。 例如：

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

当控制流离开方法体时，本地变量的值将传递回调用表达式。 如果该方法是一个子例程，则没有此类隐式局部变量，并且控件只返回到调用表达式。

## <a name="local-declaration-statements"></a>局部声明语句

局部声明语句声明一个新的局部变量、局部常量或静态变量。 *局部变量*和*本地常量*等效于作用域为方法的实例变量和常量，并以相同的方式进行声明。 *静态变量*与 `Shared` 变量类似，并且是使用 @no__t 2 修饰符声明的。

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

静态变量是在方法调用之间保留其值的局部变量。 非共享方法中声明的静态变量是每个实例的：包含该方法的类型的每个实例都有自己的静态变量副本。 在 `Shared` 方法中声明的静态变量是按类型进行的;所有实例只有一个静态变量副本。 在方法中，将局部变量初始化为其类型的默认值，而在初始化类型或类型实例时，静态变量将初始化为其类型的默认值。 静态变量不能在结构或泛型方法中声明。

局部变量、本地常量和静态变量始终具有公共可访问性，并且不能指定可访问性修饰符。 如果未在局部声明语句上指定类型，则以下步骤将确定局部声明的类型：

1. 如果声明具有类型字符，则类型字符的类型是局部声明的类型。

2. 如果本地声明为本地常量，或者本地声明是使用初始值设定项的局部变量，并且使用的是局部变量类型推理，则本地声明的类型将从初始值设定项的类型中推断出来。 如果初始化表达式引用本地声明，则会发生编译时错误。 （本地常量需要有初始值设定项。）

3. 如果未使用 strict 语义，则局部声明语句的类型将隐式 `Object`。

4. 否则，将发生编译时错误。

如果未在具有数组大小或数组类型修饰符的本地声明语句上指定类型，则本地声明的类型是具有指定秩的数组，而前面的步骤用于确定数组的元素类型。 如果使用局部变量类型推理，则初始值设定项的类型必须是具有相同数组形状（即数组类型修饰符）作为局部声明语句的数组类型。 请注意，推断元素类型可能仍为数组类型。 例如：

```vb
Option Infer On

Module Test
    Sub Main()
        ' Error: initializer is not an array type
        Dim x() = 1

        ' Type is Integer()
        Dim y() = New Integer() {}

        ' Type is Integer()()
        Dim z() = New Integer()() {}

        ' Type is Integer()()()

        Dim a()() = New Integer()()() {}

        ' Error: initializer does not have same array shape
        Dim b()() = New Integer(,)() {}
    End Sub
End Module
```

如果在具有可为 null 的类型修饰符的局部声明语句上未指定任何类型，则本地声明的类型为推断类型的可为 null 的版本，如果已为 null 的值类型，则为推断类型本身。  如果推断类型不是可以为 null 的值类型，则会发生编译时错误。 如果可为 null 的类型修饰符和数组大小或数组类型修饰符都放置在没有类型的局部声明语句上，则将可为 null 的类型修饰符应用于数组的元素类型，并使用前面的步骤来确定包含nt 类型。

局部声明语句中的变量初始值设定项等效于放置在声明的文本位置的赋值语句。 因此，如果执行分支在局部声明语句上，则不会执行变量初始值设定项。 如果多次执行局部声明语句，则执行变量初始值设定项的次数相等。 静态变量首次只执行其初始值设定项。 如果在初始化静态变量时发生异常，则会将静态变量视为使用静态变量类型的默认值初始化。

下面的示例演示如何使用初始值设定项：

```vb
Module Test
    Sub F()
        Static x As Integer = 5

        Console.WriteLine("Static variable x = " & x)
        x += 1
    End Sub

    Sub Main()
        Dim i As Integer

        For i = 1 to 3
            F()
        Next i

        i = 3
label:
        Dim y As Integer = 8

        If i > 0 Then
            Console.WriteLine("Local variable y = " & y)
            y -= 1
            i -= 1
            GoTo label
        End If
    End Sub
End Module
```

此程序将打印：

```console
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

静态局部变量上的初始值设定项在初始化期间是线程安全的，并受其保护。 如果在静态局部初始值设定项期间发生异常，则静态局部变量将具有其默认值，而不会进行初始化。 静态局部初始值设定项

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

等效于

```vb
Imports System.Threading
Imports Microsoft.VisualBasic.CompilerServices

Module Test
    Class InitFlag
        Public State As Short
    End Class

    Private xInitFlag As InitFlag = New InitFlag()

    Sub F()
        Dim x As Integer

        If xInitFlag.State <> 1 Then
            Monitor.Enter(xInitFlag)
            Try
                If xInitFlag.State = 0 Then
                    xInitFlag.State = 2
                    x = 5
                Else If xInitFlag.State = 2 Then
                    Throw New IncompleteInitialization()
                End If
            Finally
                xInitFlag.State = 1
                Monitor.Exit(xInitFlag)
            End Try
        End If
    End Sub
End Module
```

局部变量、本地常量和静态变量的范围限定为在其中声明它们的语句块。 静态变量特别适用于其名称在整个方法中只能使用一次。 例如，如果两个静态变量声明的名称相同，即使它们在不同的块中，这是无效的。


### <a name="implicit-local-declarations"></a>隐式局部声明

除了局部声明语句以外，还可以通过使用隐式声明局部变量。 使用不解析为其他内容的名称的简单名称表达式会按该名称声明局部变量。 例如：

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 10
        y = 20
        Console.WriteLine(x + y)
    End Sub
End Module
```

隐式局部声明仅出现在可接受分类为变量的表达式的表达式上下文中。 此规则的例外是，当局部变量是函数调用表达式、索引表达式或成员访问表达式的目标时，可能不会隐式声明该局部变量。

隐式局部变量被视为在包含方法的开头声明的局部变量。 因此，它们始终作用于整个方法主体，即使在 lambda 表达式内声明也是如此。 例如，以下代码：

```vb
Option Explicit Off 

Module Test
    Sub Main()
        Dim x = Sub()
                    a = 10
                End Sub
        Dim y = Sub()
                    Console.WriteLine(a)
                End Sub

        x()
        y()
    End Sub
End Module
```

将 `10` 打印该值。 如果未将任何类型字符附加到变量名称，则隐式局部变量将类型化为 `Object`;否则，该变量的类型为类型字符的类型。 局部变量类型推理不用于隐式局部变量。

如果显式局部声明由编译环境或 `Option Explicit` 指定，则所有局部变量都必须显式声明并且不允许隐式变量声明。

## <a name="with-statement"></a>With 语句

@No__t-0 语句允许多次引用表达式的成员而无需多次指定表达式。

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

表达式必须分类为一个值，并在进入块后计算一次。 在 `With` 语句块中，计算以句点或感叹号开头的成员访问表达式或字典访问表达式的计算方式就像前面的 @no__t 1 表达式。 例如：

```vb
Structure Test
    Public x As Integer

    Function F() As Integer
        Return 10
    End Sub
End Structure

Module TestModule
    Sub Main()
        Dim y As Test

        With y
            .x = 10
            Console.WriteLine(.x)
            .x = .F()
        End With
    End Sub
End Module
```

分支到块外的 @no__t 0 语句块中是无效的。


## <a name="synclock-statement"></a>SyncLock 语句

@No__t-0 语句允许在表达式上同步语句，这样可确保多个执行线程不会同时执行相同的语句。

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

表达式必须归类为一个值，并在进入到块时计算一次。 输入 `SyncLock` 块时，将在指定的表达式中调用 `System.Threading.Monitor.Enter` 的 @no__t 方法，直到执行线程在表达式返回的对象上具有排他锁。 @No__t-0 语句中表达式的类型必须是引用类型。 例如：

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        SyncLock Me
            count += 1
            Add = count
        End SyncLock
    End Function

    Public Function Subtract() As Integer
        SyncLock Me
            count -= 1
            Subtract = count
        End SyncLock
    End Function
End Class
```

上面的示例将在类的特定实例上进行同步 `Test`，以确保每个特定实例的执行线程不能同时在计数变量中增加或减少。

@No__t-0 块隐式包含在 @no__t 1 条语句中，其 @no__t 2 块调用表达式上的 @no__t 3 方法 @no__t。 这可以确保即使在引发异常时也会释放锁。 因此，从块外分支到 `SyncLock` 块并将 @no__t 1 块视为单个语句以用于 `Resume` 和 `Resume Next` 是无效的。 上面的示例等效于以下代码：

```vb
Class Test
    Private count As Integer = 0

    Public Function Add() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count += 1
            Add = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function

    Public Function Subtract() As Integer
        Try
            System.Threading.Monitor.Enter(Me)

            count -= 1
            Subtract = count
        Finally
            System.Threading.Monitor.Exit(Me)
        End Try
    End Function
End Class
```


## <a name="event-statements"></a>Event 语句

@No__t，`AddHandler` 和 `RemoveHandler` 语句会引发事件并动态处理事件。

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a>RaiseEvent 语句

@No__t-0 语句通知事件处理程序发生了特定事件。

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

@No__t-0 语句中的简单名称表达式被解释为 `Me` 上的成员查找。 因此，`RaiseEvent x` 被解释为 @no__t 为-1。 表达式的结果必须归类为对类本身中定义的事件的事件访问;在基类型上定义的事件不能在 `RaiseEvent` 语句中使用。

使用提供的参数（如果有），将 `RaiseEvent` 语句作为对事件委托的 @no__t 的方法的调用进行处理。 如果委托的值为 `Nothing`，则不会引发异常。 如果没有参数，则可以省略括号。 例如：

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0

        RaiseCount += 1
        RaiseEvent E1(RaiseCount)
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler(Count As Integer) Handles x.E1
        Console.WriteLine("Raise #" & Count)
    End Sub

    Public Sub Main()
        x = New Raiser
        x.Raise()        ' Prints "Raise #1".
        x.Raise()        ' Prints "Raise #2".
        x.Raise()        ' Prints "Raise #3".
    End Sub
End Module
```

上面 `Raiser` 类等效于：

```vb
Class Raiser
    Public Event E1(Count As Integer)

    Public Sub Raise()
        Static RaiseCount As Integer = 0
        Dim TemporaryDelegate As E1EventHandler

        RaiseCount += 1

        ' Use a temporary to avoid a race condition.
        TemporaryDelegate = E1Event
        If Not TemporaryDelegate Is Nothing Then
            TemporaryDelegate.Invoke(RaiseCount)
        End If
    End Sub
End Class
```


### <a name="addhandler-and-removehandler-statements"></a>AddHandler 和 RemoveHandler 语句

尽管大多数事件处理程序都通过 `WithEvents` 变量自动挂钩，但可能需要在运行时动态添加和删除事件处理程序。 `AddHandler` 和 `RemoveHandler` 语句执行此操作。

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

每个语句都采用两个参数：第一个参数必须是分类为事件访问的表达式，第二个参数必须是分类为值的表达式。 第二个参数的类型必须是与事件访问关联的委托类型。 例如：

```vb
Public Class Form1
    Public Sub New()
        ' Add Button1_Click as an event handler for Button1's Click event.
        AddHandler Button1.Click, AddressOf Button1_Click
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        RemoveHandler Button1.Click, AddressOf Button1_Click
    End Sub
End Class
```

给定事件 `E,` 语句对实例调用相关的 @no__t 或 `remove_E` 方法，以添加或删除作为事件处理程序的委托。 因此，上述代码等效于：

```vb
Public Class Form1
    Public Sub New()
        Button1.add_Click(AddressOf Button1_Click)
    End Sub 

    Private Button1 As Button = New Button()

    Sub Button1_Click(sender As Object, e As EventArgs)
        Console.WriteLine("Button1 was clicked!")
    End Sub

    Public Sub Disconnect()
        Button1.remove_Click(AddressOf Button1_Click)
    End Sub
End Class
```


## <a name="assignment-statements"></a>赋值语句

赋值语句将表达式的值分配给变量。 有几种类型的赋值。

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a>常规赋值语句

简单的赋值语句将表达式的结果存储在变量中。

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

赋值运算符左侧的表达式必须归类为变量或属性访问权限，而赋值运算符右侧的表达式必须归类为值的值值。 表达式的类型必须可隐式转换为变量或属性访问的类型。

如果要赋给的变量是引用类型的数组元素，则将执行运行时检查以确保表达式与数组元素类型兼容。 在下面的示例中，上一次分配导致引发 `System.ArrayTypeMismatchException`，因为 @no__t 的实例无法存储在 @no__t 数组的元素中。

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

如果赋值运算符左侧的表达式归类为变量，则赋值语句将值存储在变量中。 如果表达式归类为属性访问，则赋值语句会将属性访问权限转换为对属性的 @no__t 值访问器的调用，并将替换值参数的值。 例如：

```vb
Module Test
    Private PValue As Integer

    Public Property P As Integer
        Get
            Return PValue
        End Get

        Set (Value As Integer)
            PValue = Value
        End Set
    End Property

    Sub Main()
        ' The following two lines are equivalent.
        P = 10
        set_P(10)
    End Sub
End Module
```

如果变量或属性访问的目标类型为值类型，但未分类为变量，则会发生编译时错误。 例如：

```vb
Structure S
    Public F As Integer
End Structure

Class C
    Private PValue As S

    Public Property P As S
        Get
            Return PValue
        End Get

        Set (Value As S)
            PValue = Value
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim ct As C = New C()
        Dim rt As Object = new C()

        ' Compile-time error: ct.P not classified as variable.
        ct.P.F = 10

        ' Run-time exception.
        rt.P.F = 10
    End Sub
End Module
```

请注意，赋值的语义取决于将其分配到的变量或属性的类型。 如果为其分配的变量是值类型，则赋值将表达式的值复制到变量中。 如果为其分配的变量是引用类型，则赋值将引用（而不是值本身）复制到变量中。 如果变量的类型为 `Object`，则赋值语义取决于值的类型在运行时是否为值类型或引用类型。


__纪录.__ 对于内部类型（如 `Integer` 和 `Date`），引用和值赋值语义是相同的，因为这些类型是不可变的。 这样一来，就可以将装箱的内部类型的引用分配用作一种优化语言。 从值角度来看，结果是相同的。

因为等号字符（`=`）同时用于赋值和相等，所以，在 `x = y.ToString()` 的情况下，简单赋值与调用语句之间存在歧义。 在所有这类情况下，赋值语句优先于相等运算符。 这意味着，示例表达式被解释为 `x = (y.ToString())` 而不是 `(x = y).ToString()`。


### <a name="compound-assignment-statements"></a>复合赋值语句

*复合赋值语句*采用 `V op= E` （其中 `op` 是有效的二进制运算符）的形式。

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

赋值运算符左侧的表达式必须归类为变量或属性访问权限，而赋值运算符右侧的表达式必须分类为值值。 复合赋值语句等效于语句 `V = V op E`，与复合赋值运算符左侧的变量只计算一次时的差异。 以下示例演示了这一差异：

```vb
Module Test
    Function GetIndex() As Integer
        Console.WriteLine("Getting index")
        Return 1
    End Function

    Sub Main()
        Dim a(2) As Integer

        Console.WriteLine("Simple assignment")
        a(GetIndex()) = a(GetIndex()) + 1

        Console.WriteLine("Compound assignment")
        a(GetIndex()) += 1
    End Sub
End Module
```

对于简单赋值，表达式 `a(GetIndex())` 计算两次，但对于复合赋值，只计算一次，因此代码将打印：

```console
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a>Mid 赋值语句

@No__t 0 赋值语句将一个字符串赋给另一个字符串。 赋值的左侧与函数调用 `Microsoft.VisualBasic.Strings.Mid` 的语法相同。

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

第一个参数是赋值目标，并且必须归类为变量或属性访问，其类型可隐式转换为 `String`。 第二个参数是从1开始的开始位置，它对应于赋值在目标字符串中的开始位置，并且必须分类为其类型必须可隐式转换为 `Integer` 的值。 可选的第三个参数是要分配给目标字符串的右侧值中的字符数，并且必须归类为其类型可隐式转换为 `Integer` 的值。 右侧是源字符串，必须归类为其类型可隐式转换为 `String` 的值。 如果已指定，右侧将被截断为长度参数，并从起始位置开始替换左侧字符串中的字符。 如果右侧字符串包含的字符数少于第三个参数，则仅复制来自右侧字符串的字符。

下面的示例显示 `ab123fg`：

```vb
Module Test
    Sub Main()
        Dim s1 As String = "abcdefg"
        Dim s2 As String = "1234567"

        Mid$(s1, 3, 3) = s2
        Console.WriteLine(s1)
    End Sub
End Module
```

__纪录.__ `Mid` 不是保留字。


## <a name="invocation-statements"></a>调用语句

调用语句调用一个方法，该方法前面有一个可选关键字 `Call`。 调用语句的处理方式与函数调用表达式的处理方式相同，但有一些区别。 调用表达式必须分类为值或 void。 对调用表达式求值后得出的任何值都将被丢弃。

如果省略 @no__t 0 关键字，则调用表达式必须以标识符或关键字开头，或在 `With` 块内以 `.` 开头。 例如，"`Call 1.ToString()`" 是有效的语句，但不是 "`1.ToString()`"。 （请注意，在表达式上下文中，调用表达式也不需要以标识符开头。 例如，"`Dim x = 1.ToString()`" 是有效的语句）。

调用语句和调用表达式之间还有另一个区别：如果调用语句包含参数列表，则此操作将始终作为调用的参数列表。 下面的示例阐释了差异：

```vb
Module Test
    Sub Main()
        Call {Function() 15}(0)
        ' error: (0) is taken as argument list, but array is not invokable

        Call ({Function() 15}(0))
        ' valid, since the invocation statement has no argument list

        Dim x = {Function() 15}(0)
        ' valid as an expression, since (0) is taken as an array-indexing

        Call f("a")
        ' error: ("a") is taken as argument list to the invocation of f

        Call f()("a")
        ' valid, since () is the argument list for the invocation of f

        Dim y = f("a")
        ' valid as an expression, since f("a") is interpreted as f()("a")
    End Sub

    Sub f() As Func(Of String,String)
        Return Function(x) x
    End Sub
End Module
```

```antlr
InvocationStatement
    : 'Call'? InvocationExpression StatementTerminator
    ;
```

## <a name="conditional-statements"></a>条件语句

条件语句允许基于在运行时计算的表达式执行语句的条件执行。

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a>If .。。Then .。。Else 语句

@No__t-0 语句是基本的条件语句。

```antlr
IfStatement
    : BlockIfStatement
    | LineIfThenStatement
    ;

BlockIfStatement
    : 'If' BooleanExpression 'Then'? StatementTerminator
      Block?
      ElseIfStatement*
      ElseStatement?
      'End' 'If' StatementTerminator
    ;

ElseIfStatement
    : ElseIf BooleanExpression 'Then'? StatementTerminator
      Block?
    ;

ElseStatement
    : 'Else' StatementTerminator
      Block?
    ;

LineIfThenStatement
    : 'If' BooleanExpression 'Then' Statements ( 'Else' Statements )? StatementTerminator
    ;
    
ElseIf      
    : 'ElseIf'      
    | 'Else' 'If'   
   ;
```

@No__t-0 语句中的每个表达式都必须为布尔表达式，如每个部分的[布尔表达式](expressions.md#boolean-expressions)。 （注意：这不需要表达式具有布尔类型）。 如果 `If` 语句中的表达式为 true，则执行 @no__t 中包含的语句。 如果表达式为 false，则计算每个 @no__t 0 表达式。 如果 @no__t 0 表达式之一的计算结果为 true，则执行相应的块。 如果表达式的计算结果都不为 true，并且有 `Else` 块，则执行 @no__t 的块。 块完成执行后，将传递到 `If...Then...Else` 语句的末尾。

如果 @no__t 的表达式 @no__t 为-2，则 @no__t 的语句的行版本包含一组要执行的语句，如果表达式为 `False`，则执行一组可选的语句来执行。 例如：

```vb
Module Test
    Sub Main()
        Dim a As Integer = 10
        Dim b As Integer = 20

        ' Block If statement.
        If a < b Then
            a = b
        Else
            b = a
        End If

        ' Line If statement
        If a < b Then a = b Else b = a
    End Sub
End Module
```

If 语句的行版本与 "：" 紧密绑定，其 `Else` 绑定到语法允许的上一个前面的 `If`。 例如，以下两个版本是等效的：

```vb
If True Then _
If True Then Console.WriteLine("a") Else Console.WriteLine("b") _
Else Console.WriteLine("c") : Console.WriteLine("d")

If True Then
    If True Then
        Console.WriteLine("a")
    Else
        Console.WriteLine("b")
    End If
    Console.WriteLine("c") : Console.WriteLine("d")
End If
```

除标签声明语句以外的所有语句都允许在 `If` 语句（包括块语句）的行中使用。 但在多行 lambda 表达式中，它们不能将 LineTerminators 用作 StatementTerminators。 例如：

```vb
' Allowed, since it uses : instead of LineTerminator to separate statements
If b Then With New String("a"(0),5) : Console.WriteLine(.Length) : End With

' Disallowed, since it uses a LineTerminator
If b then With New String("a"(0), 5)
              Console.WriteLine(.Length)
          End With

' Allowed, since it only uses LineTerminator inside a multi-line lambda
If b Then Call Sub()
                   Console.WriteLine("a")
               End Sub.Invoke()
```

### <a name="select-case-statements"></a>Select Case 语句

@No__t-0 语句基于表达式的值执行语句。

```antlr
SelectStatement
    : 'Select' 'Case'? Expression StatementTerminator
      CaseStatement*
      CaseElseStatement?
      'End' 'Select' StatementTerminator
    ;

CaseStatement
    : 'Case' CaseClauses StatementTerminator
      Block?
    ;

CaseClauses
    : CaseClause ( Comma CaseClause )*
    ;

CaseClause
    : ( 'Is' LineTerminator? )? ComparisonOperator LineTerminator? Expression
    | Expression ( 'To' Expression )?
    ;

ComparisonOperator
    : '=' | '<' '>' | '<' | '>' | '>' '=' | '<' '='
    ;

CaseElseStatement
    : 'Case' 'Else' StatementTerminator
      Block?
    ;
```

表达式必须分类为值。 执行 `Select Case` 语句时，将首先计算 @no__t 1 表达式，然后按文本声明顺序计算 `Case` 语句。 计算结果为 `True` 的第一个 `Case` 语句的块已执行。 如果没有 `Case` 语句的计算结果为 `True`，并且存在 @no__t 2 语句，则将执行该块。 块执行完毕后，将传递到 `Select` 语句的末尾。

不允许执行 `Case` 块到下一个开关部分。 这可以防止在意外省略 `Case` 终止语句时使用其他语言发生的常见错误类。 下面的示例阐释了这种行为：

```vb
Module Test
    Sub Main()
        Dim x As Integer = 10

        Select Case x
            Case 5
                Console.WriteLine("x = 5")
            Case 10
                Console.WriteLine("x = 10")
            Case 20 - 10
                Console.WriteLine("x = 20 - 10")
            Case 30
                Console.WriteLine("x = 30")
        End Select
    End Sub
End Module
```

代码将打印：

```console
x = 10
```

尽管 @no__t 为相同的值，`Case 20 - 10`，但会执行 `Case 10`，因为它在每个的第3个值 @no__t 之前。 当到达下一个 `Case` 时，执行将在 @no__t 1 语句后继续。

@No__t-0 子句可以采用两种形式。 一个窗体是可选的 @no__t 0 关键字、比较运算符和表达式。 表达式转换为 @no__t 0 表达式的类型;如果表达式不能隐式转换为 `Select` 表达式的类型，则会发生编译时错误。 如果 @no__t 0 表达式为*E*，则比较运算符为*Op*，`Case` 表达式为*E1*，则将事例计算为*E Op E1*。 对于两个表达式的类型，该运算符必须有效;否则，会发生编译时错误。

另一种形式是表达式，后跟关键字 `To` 和第二个表达式。 两个表达式都转换为 @no__t 0 表达式的类型;如果任一表达式不能隐式转换为 `Select` 表达式的类型，则会发生编译时错误。 如果 @no__t 0 表达式 @no__t 为-1，则第一个 `Case` 表达式为 `E1`，第二个 @no__t 表达式为 `E2`，则计算 `E = E1` （如果未指定 `E2`）或 `(E >= E1) And (E <= E2)`。 对于两个表达式的类型，这些运算符必须有效;否则，会发生编译时错误。


## <a name="loop-statements"></a>Loop 语句

循环语句允许在其主体中重复执行语句。

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

每次输入循环正文时，都将在该正文中声明的所有局部变量上都生成一个全新的副本，并将其初始化为变量以前的值。 对循环体中的变量的任何引用都将使用最近创建的副本。 此代码显示了一个示例：

```vb
Module Test
    Sub Main()
        Dim lambdas As New List(Of Action)
        Dim x = 1

        For i = 1 To 3
            x = i
            Dim y = x
            lambdas.Add(Sub() Console.WriteLine(x & y))
        Next

        For Each lambda In lambdas
            lambda()
        Next
    End Sub
End Module
```

此代码生成以下输出：

```console
31    32    33
```

当执行循环体时，它将使用该变量的任何副本是最新的。 例如，语句 `Dim y = x` 指 @no__t 的最新副本和 @no__t 的原始副本。 创建 lambda 后，它会记住变量在创建时的哪个副本是最新的。 因此，每个 lambda 使用 `x` 的相同共享副本，但 `y` 的不同副本。 在程序结束时，当它执行 lambda 时，它们所引用的 @no__t 的共享副本现在为其最终值3。

请注意，如果没有 lambda 或 LINQ 表达式，则不可能知道在循环条目上进行了全新的复制。 事实上，在这种情况下，编译器优化将避免生成副本。 请注意，将 `GoTo` 到包含 lambda 或 LINQ 表达式的循环中是非法的。


### <a name="whileend-while-and-doloop-statements"></a>.。。结束时间和执行时间 .。。Loop 语句

@No__t-0 或 @no__t 循环语句基于布尔表达式循环。

```antlr
WhileStatement
    : 'While' BooleanExpression StatementTerminator
      Block?
      'End' 'While' StatementTerminator
    ;

DoLoopStatement
    : DoTopLoopStatement
    | DoBottomLoopStatement
    ;

DoTopLoopStatement
    : 'Do' ( WhileOrUntil BooleanExpression )? StatementTerminator
      Block?
      'Loop' StatementTerminator
    ;

DoBottomLoopStatement
    : 'Do' StatementTerminator
      Block?
      'Loop' WhileOrUntil BooleanExpression StatementTerminator
    ;

WhileOrUntil
    : 'While' | 'Until'
    ;
```

如果布尔表达式的计算结果为 true，则 `While` 循环语句会循环;`Do` loop 语句可能包含更复杂的条件。 表达式可以放在 `Do` 关键字之后或 `Loop` 关键字之后，但不能位于这两者之后。 布尔表达式的计算方式为[布尔](expressions.md#boolean-expressions)表达式。 （注意：这不需要表达式具有布尔类型）。 同时指定 no 表达式也是有效的;在这种情况下，循环永远不会退出。 如果表达式位于 `Do` 之后，则在每次迭代上执行循环块之前，将计算该表达式。 如果表达式位于 `Loop` 之后，则在每次迭代中执行循环块后，将计算该表达式。 因此，将表达式放在 `Loop` 后，将生成比 `Do` 后面的位置更多的循环。 下面的示例演示了此行为：

```vb
Module Test
    Sub Main()
        Dim x As Integer

        x = 3
        Do While x = 1
            Console.WriteLine("First loop")
        Loop

        Do
            Console.WriteLine("Second loop")
        Loop While x = 1
    End Sub
End Module
```

此代码生成以下输出：

```console
Second Loop
```

如果是第一次循环，则在循环执行之前计算条件。 对于第二个循环，该条件在循环执行后执行。 条件表达式必须以 @no__t 关键字或 @no__t 关键字为前缀。 如果条件的计算结果为 false，则前者会断开循环，条件的计算结果为 true。

__纪录.__ `Until` 不是保留字。


### <a name="fornext-statements"></a>对于 .。。Next 语句

@No__t-0 语句根据一组界限循环。 @No__t-0 语句指定循环控制变量、下限表达式、上限表达式和可选步骤值表达式。 循环控制变量是通过标识符指定的，后跟可选的 `As` 子句或表达式。

```antlr
ForStatement
    : 'For' LoopControlVariable Equals Expression 'To' Expression
      ( 'Step' Expression )? StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;

LoopControlVariable
    : Identifier ( IdentifierModifiers 'As' TypeName )?
    | Expression
    ;

NextExpressionList
    : Expression ( Comma Expression )*
    ;
```

根据以下规则，循环控制变量是指特定于此 `For...Next` 语句或预先存在的变量或表达式的新局部变量。

* 如果循环控制变量是带有 `As` 子句的标识符，则该标识符将定义在 `As` 子句中指定的类型的新局部变量，其作用域为整个 `For` 循环。

* 如果循环控制变量是没有 `As` 子句的标识符，则首先使用简单名称解析规则解析该标识符（请参阅 "[简单名称表达式](expressions.md#simple-name-expressions)" 部分），只不过该标识符的此匹配项不在和中本身导致创建一个隐式局部变量（节[隐式局部声明](statements.md#implicit-local-declarations)）。

  * 如果此解析成功并且结果归类为变量，则循环控制变量是预先存在的变量。

  * 如果解析失败，或者如果解析成功并且结果归类为类型，则：
    * 如果正在使用本地变量类型推理，则标识符将定义一个新的局部变量，该局部变量的类型将从绑定和步骤表达式中推断出，范围为整个 `For` 循环;
    * 如果未使用局部变量类型推理，但隐式局部声明为，则将创建一个隐式局部变量，其作用域为整个方法（部分[隐式局部声明](statements.md#implicit-local-declarations)），并且循环控制变量引用此预先存在的变量;
    * 如果不使用本地变量类型推理和隐式本地声明，则是错误的。

  * 如果解析成功，同时分类为类型和变量，则是错误的。

* 如果循环控制变量是一个表达式，则该表达式必须归类为变量。

循环控制变量不能由另一个封闭 `For...Next` 语句使用。 @No__t-0 语句的循环控制变量的类型确定迭代的类型，必须是以下类型之一：

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`
* 枚举类型
* `Object`
* 一个类型 `T`，其中包含以下运算符，其中 `B` 是可在布尔表达式中使用的类型：

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

绑定表达式和步骤表达式必须可隐式转换为循环控制变量的类型，并且必须分类为值。 在编译时，循环控制变量的类型是通过在下限、上限和步骤表达式类型之间选择最宽类型来推断的。 如果两种类型之间没有扩大转换，则会发生编译时错误。

在运行时，如果循环控制变量的类型为 `Object`，则迭代的类型在编译时将推断为，但有两个例外。 首先，如果绑定表达式和步骤表达式均为整型类型，但其类型不是最大，则将推断包含所有三个类型的最宽类型。 第二种情况下，如果将循环控制变量的类型推断为 `String`，则改为推断 `Double`。 如果在运行时无法确定循环控制类型，或者如果任何表达式不能转换为 loop 控件类型，则会出现 @no__t 0。 一旦在循环开始选择了循环控件类型，就会在整个迭代中使用相同的类型，而不考虑对循环控制变量中的值所做的更改。

@No__t-0 语句必须通过匹配的 `Next` 语句来关闭。 不带变量的 @no__t 0 语句与最内层的 open `For` 语句匹配，而具有一个或多个循环控制变量的 `Next` 语句将与每个变量匹配的 `For` 循环匹配。 如果变量与非最嵌套循环的 @no__t 0 循环匹配，则会产生编译时错误。

在循环开始时，将按文本顺序计算三个表达式，并将下限表达式分配给循环控制变量。 如果省略步长值，则它将隐式赋值 `1`，转换为循环控制变量的类型。 这三个表达式仅在循环开始时进行计算。

在每个循环开始时，将对控制变量进行比较，以查看如果步骤表达式为正，则为; 如果步骤表达式为负，则为小于结束点。 如果已终止，则为 `For` 循环;否则，将执行循环块。 如果循环控制变量不是基元类型，则比较运算符取决于表达式 `step >= step - step` 是 true 还是 false。 在 `Next` 语句中，步骤值将添加到控件变量，并且执行将返回到循环的顶部。

请注意，*不*会在循环块的每个迭代上创建循环控制变量的新副本。 在这方面，`For` 语句不同于 `For Each` （[每个的部分 .。。Next 语句](statements.md#for-eachnext-statements)）。

从循环外分支到 `For` 循环是无效的。


### <a name="for-eachnext-statements"></a>对于每个 .。。Next 语句

@No__t-0 语句基于表达式中的元素循环。 @No__t-0 语句指定循环控制变量和枚举器表达式。 循环控制变量是通过标识符指定的，后跟可选的 `As` 子句或表达式。

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

遵循相同的规则`For...Next`语句 (部分[对于。 ...下一语句](statements.md#fornext-statements))，循环控制变量是指到特定的新本地变量这对于每个...下一个语句，或到预先存在的变量，或表达式。

枚举器表达式必须归类为值，其类型必须为集合类型或 `Object`。 如果枚举器表达式的类型为 `Object`，则所有处理会推迟到运行时。 否则，必须从集合的元素类型中将转换转换为循环控制变量的类型。

循环控制变量不能被另一个封闭 `For Each` 语句使用。 @No__t-0 语句必须通过匹配的 `Next` 语句来关闭。 不含循环控制变量的 @no__t 0 语句匹配最内层的打开 `For Each`。 具有一个或多个循环控制变量的 @no__t 0 语句将从左到右与具有相同循环控制变量的 @no__t 1 循环匹配。 如果变量与非最嵌套循环的 @no__t 0 循环匹配，则会发生编译时错误。

如果类型 @no__t，则称为*集合类型*，前提是：

* 以下所有条件均为 true：
  * `C` 包含可访问的实例、具有签名 `GetEnumerator()` 的共享或扩展方法，该方法返回类型 `E`。
  * `E` 包含可访问的实例、共享或扩展方法，签名 @no__t 为-1，返回类型为 `Boolean`。
  * `E` 包含具有 getter 的名为 `Current` 的可访问实例或共享属性。 此属性的类型是集合类型的元素类型。

* 它实现接口 `System.Collections.Generic.IEnumerable(Of T)`，在这种情况下，集合的元素类型被视为 @no__t 为-1。

* 它实现接口 `System.Collections.IEnumerable`，在这种情况下，集合的元素类型被视为 @no__t 为-1。

下面是一个可枚举的类的示例：

```vb
Public Class IntegerCollection
    Private integers(10) As Integer

    Public Class IntegerCollectionEnumerator
        Private collection As IntegerCollection
        Private index As Integer = -1

        Friend Sub New(c As IntegerCollection)
            collection = c
        End Sub

        Public Function MoveNext() As Boolean
            index += 1

            Return index <= 10
        End Function

        Public ReadOnly Property Current As Integer
            Get
                If index < 0 OrElse index > 10 Then
                    Throw New System.InvalidOperationException()
                End If

                Return collection.integers(index)
            End Get
        End Property
    End Class

    Public Sub New()
        Dim i As Integer

        For i = 0 To 10
            integers(i) = I
        Next i
    End Sub

    Public Function GetEnumerator() As IntegerCollectionEnumerator
        Return New IntegerCollectionEnumerator(Me)
    End Function
End Class
```

循环开始之前，计算枚举器表达式。 如果表达式的类型不满足设计模式，则会将表达式强制转换为 `System.Collections.IEnumerable` 或 @no__t 为-1。 如果表达式类型实现泛型接口，则在编译时首选泛型接口，但在运行时首选非泛型接口。 如果表达式类型多次实现泛型接口，则该语句将被视为不明确，并发生编译时错误。

__纪录.__ 在后期绑定情况中首选非泛型接口，因为选取泛型接口意味着对接口方法的所有调用都涉及到类型参数。 由于不能在运行时知道匹配类型参数，因此必须使用后期绑定调用来进行所有此类调用。 这会比调用非泛型接口慢，因为使用编译时调用可以调用非泛型接口。

对结果值调用 `GetEnumerator`，并且函数的返回值存储在临时中。 然后，在每次迭代开始时，将对临时调用 `MoveNext`。 如果它返回 `False`，则循环将终止。 否则，循环的每次迭代都按如下方式执行：

1. 如果循环控制变量识别了新的本地变量（而不是预先存在的变量），则将创建该局部变量的新副本。 对于当前迭代，循环块内的所有引用都将引用此副本。
2. 检索 `Current` 属性，将其强制转换为循环控制变量的类型（不管是隐式还是显式转换），并赋给循环控制变量。
3. 循环块执行。

__纪录.__ 语言版本10.0 和11.0 之间的行为稍有不同。 在11.0 之前，*不会*为循环的每次迭代创建新的迭代变量。 仅当迭代变量是通过 lambda 或 LINQ 表达式捕获的，然后在循环后调用后，这种差异才可观察：

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

最多 Visual Basic 10.0，这会在编译时生成警告，并将其打印三次。 这是因为循环的所有迭代都只共享了一个变量 "x"，而所有三个 lambda 捕获了相同的 "x"，并在执行了 lambda 后，则将其保留为3。 从 Visual Basic 11.0，将打印 "1，2，3"。 这是因为每个 lambda 都捕获不同的变量 "x"。


__纪录.__ 即使在语句中没有方便地引入转换运算符，迭代的当前元素也会转换为循环控制变量的类型，即使转换是显式的。 当使用过时的类型 `System.Collections.ArrayList` 时，这会变得非常麻烦，因为其元素类型 @no__t 为-1。 这会在很多循环中产生必需的强制转换，但我们认为这并不理想。 具有讽刺意味，泛型启用了强类型集合 `System.Collections.Generic.List(Of T)`，这可能使我们重新思考此设计点，但为了实现兼容性，现在无法更改。


当到达 `Next` 语句时，执行将返回到循环的顶部。 如果在 `Next` 关键字之后指定了变量，则它必须与 `For Each` 之后的第一个变量相同。 例如，考虑以下代码：

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        For Each i In c
            Console.WriteLine(i)
        Next i
    End Sub
End Module
```

它等效于以下代码：

```vb
Module Test
    Sub Main()
        Dim i As Integer
        Dim c As IntegerCollection = New IntegerCollection()

        Dim e As IntegerCollection.IntegerCollectionEnumerator

        e = c.GetEnumerator()
        While e.MoveNext()
            i = e.Current

            Console.WriteLine(i)
        End While
    End Sub
End Module
```

如果枚举器的类型 `E` 实现 `System.IDisposable`，则通过调用 `Dispose` 方法，在退出循环时释放枚举器。 这可确保释放枚举器所持有的资源。 如果包含 `For Each` 语句的方法不使用非结构化错误处理，则将 @no__t 1 语句包装在第2条语句 @no__t 中，该语句具有 `Finally` 中调用的 `Dispose` 方法，以确保进行清理。

__纪录.__ @No__t 类型是集合类型，并且由于所有数组类型都派生自 `System.Array`，因此在 `For Each` 语句中允许任何数组类型表达式。 对于一维数组，`For Each` 语句会按递增的索引顺序枚举数组元素，从索引0开始，以索引长度-1 结束。 对于多维数组，首先增加最右侧维度的索引。

例如，以下代码将打印 `1 2 3 4`：

```vb
Module Test
    Sub Main()
        Dim x(,) As Integer = { { 1, 2 }, { 3, 4 } }
        Dim i As Integer

        For Each i In x
            Console.Write(i & " ")
        Next i
    End Sub
End Module
```

分支到块外的 @no__t 0 语句块中是无效的。


## <a name="exception-handling-statements"></a>异常处理语句

Visual Basic 支持结构化异常处理和非结构化异常处理。 在方法中只能使用一种类型的异常处理，但 `Error` 语句可用于结构化异常处理。 如果方法使用这两种样式的异常处理，则会产生编译时错误。

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a>结构化异常处理语句

结构化异常处理是一种处理错误的方法，即在其中处理某些异常的显式块。 结构化异常处理通过 `Try` 语句来完成。

```antlr
StructuredErrorStatement
    : ThrowStatement
    | TryStatement
    ;

TryStatement
    : 'Try' StatementTerminator
      Block?
      CatchStatement*
      FinallyStatement?
      'End' 'Try' StatementTerminator
    ;
```


例如：

```vb
Module Test
    Sub ThrowException()
        Throw New Exception()
    End Sub

    Sub Main()
        Try
            ThrowException()
        Catch e As Exception
            Console.WriteLine("Caught exception!")
        Finally
            Console.WriteLine("Exiting try.")
        End Try
    End Sub
End Module
```

@No__t-0 语句由三种块组成： try 块、catch 块和 finally 块。 *Try 块*是包含要执行的语句的语句块。 *Catch 块*是处理异常的语句块。 *Finally 块*是一个语句块，其中包含在退出 `Try` 语句时要运行的语句，而不考虑是否已发生和处理异常。 @No__t-0 语句，只能包含一个 try 块和一个 finally 块，必须至少包含一个 catch 块或 finally 块。 将执行显式传输到 try 块中是无效的，除非在同一语句中的 catch 块内。


#### <a name="finally-blocks"></a>Finally 块

当执行离开 @no__t 语句的任何部分时，始终会执行 `Finally` 块。 不需要显式操作即可执行 `Finally` 块;当执行离开 `Try` 语句时，系统将自动执行 @no__t 2 块，然后将执行转移到其预期目标。 无论执行如何离开 @no__t 语句，都将执行 `Finally` 块：通过 `Try` 块的末尾 @no__t，通过 @no__t 语句，通过 @no__t 语句，或不处理引发的异常而执行的操作的时间块为。

请注意，异步方法中的 @no__t 0 表达式和迭代器方法中的 @no__t 语句会导致在 async 或 iterator 方法实例中挂起控制流，并在其他方法实例中恢复。 但是，这只是一种暂停执行，不涉及退出各自的 async 方法或迭代器方法实例，因此不会导致执行 `Finally` 块。

将执行显式传输到 `Finally` 块无效：除了通过异常以外，将执行从 `Finally` 块传输的操作也无效。

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a>catch 块

如果处理 `Try` 块时出现异常，则将按文本顺序检查每个 @no__t 1 语句，以确定它是否处理异常。

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

@No__t-0 子句中指定的标识符表示已引发的异常。 如果标识符包含 `As` 子句，则认为该标识符是在 @no__t 1 块的本地声明空间内声明的。 否则，标识符必须是在包含块中定义的局部变量（而非静态变量）。

不带标识符的 @no__t 0 子句将捕获从 @no__t 派生的所有异常。 带有标识符的 @no__t 0 子句仅捕获其类型与标识符的类型相同或派生的异常。 类型必须为 `System.Exception` 或从 @no__t 派生的类型。 当捕获派生自 `System.Exception` 的异常时，对异常对象的引用将存储在函数返回的对象 `Microsoft.VisualBasic.Information.Err` 中。

带 @no__t 子句的 `Catch` 子句仅会在表达式的计算结果为 `True` 时捕获异常;表达式的类型必须为布尔表达式，如每节[布尔表达式](expressions.md#boolean-expressions)。 仅在检查异常的类型后才应用 `When` 子句，并且表达式可以引用表示异常的标识符，如以下示例所示：

```vb
Module Test
    Sub Main()
        Dim i As Integer = 5

        Try
            Throw New ArgumentException()
        Catch e As OverflowException When i = 5
            Console.WriteLine("First handler")
        Catch e As ArgumentException When i = 4
            Console.WriteLine("Second handler")
        Catch When i = 5
            Console.WriteLine("Third handler")
        End Try

    End Sub
End Module
```

此示例将打印：

```console
Third handler
```

如果 @no__t 0 子句处理异常，执行将传输到 @no__t 块。 在 `Catch` 块的末尾，执行将传输到 @no__t 语句后面的第一条语句。 @No__t-0 语句将不处理 @no__t 块中引发的任何异常。 如果没有 `Catch` 子句处理异常，则执行会传输到系统确定的位置。

将执行显式传输到 `Catch` 块中是无效的。

通常在引发异常之前计算 When 子句中的筛选器。 例如，以下代码将打印 "Filter，Finally，Catch"。

```vb
Sub Main()
   Try
       Foo()
   Catch ex As Exception When F()
       Console.WriteLine("Catch")
   End Try
End Sub

Sub Foo()
    Try
        Throw New Exception
    Finally
        Console.WriteLine("Finally")
    End Try
End Sub

Function F() As Boolean
    Console.WriteLine("Filter")
    Return True
End Function
```

 但是，Async 和 Iterator 方法会导致在任何筛选器之外的任何筛选器之前执行它们内部的所有 finally 块。 例如，如果上面的代码 `Async Sub Foo()`，则输出将为 "Finally，Filter，Catch"。


#### <a name="throw-statement"></a>Throw 语句

@No__t-0 语句引发异常，该异常由派生自 @no__t 的类型的实例表示。

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

如果表达式未分类为值或不是派生自 `System.Exception` 的类型，则会发生编译时错误。 如果表达式在运行时计算为 null 值，则改为引发 `System.NullReferenceException` 异常。

只要没有干预 finally 块，`Throw` 语句就可以省略 `Try` 语句 catch 块内的表达式。 在这种情况下，该语句重新引发 catch 块中当前正在处理的异常。 例如：

```vb
Sub Test(x As Integer)
    Try
        Throw New Exception()
    Catch
        If x = 0 Then
            Throw    ' OK, rethrows exception from above.
        Else
            Try
                If x = 1 Then
                    Throw   ' OK, rethrows exception from above.
                End If
            Finally
                Throw    ' Invalid, inside of a Finally.
            End Try
        End If
    End Try
End Sub
```


### <a name="unstructured-exception-handling-statements"></a>非结构化异常处理语句

非结构化异常处理是一种处理错误的方法，指示在发生异常时要分支到的语句。 非结构化异常处理是使用三个语句实现的： `Error` 语句、@no__t 1 语句和 `Resume` 语句。

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

例如：

```vb
Module Test
    Sub ThrowException()
        Error 5
    End Sub

    Sub Main()
        On Error GoTo GotException

        ThrowException()
        Exit Sub

GotException:
        Console.WriteLine("Caught exception!")
        Resume Next
    End Sub
End Module
```

当方法使用非结构化异常处理时，将为全部捕获所有异常的方法建立单个结构化异常处理程序。 （请注意，在构造函数中，此处理程序不会将对的调用扩展到对构造函数开头 `New` 的调用。）然后，方法跟踪最近引发的异常处理程序位置和最近发生的异常。 进入方法时，异常处理程序位置和异常均设置为 `Nothing`。 当使用非结构化异常处理的方法中引发异常时，对异常对象的引用存储在函数返回的对象中，`Microsoft.VisualBasic.Information.Err`。

迭代器或异步方法中不允许使用非结构化错误处理语句。


#### <a name="error-statement"></a>Error 语句

@No__t-0 语句将引发一个 @no__t 为1的异常，该异常包含一个 Visual Basic 6 异常号。 表达式必须分类为值，并且其类型必须可隐式转换为 `Integer`。

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a>On Error 语句

@No__t-0 语句修改最新的异常处理状态。

```antlr
OnErrorStatement
    : 'On' 'Error' ErrorClause StatementTerminator
    ;

ErrorClause
    : 'GoTo' '-' '1'
    | 'GoTo' '0'
    | GoToStatement
    | 'Resume' 'Next'
    ;
```

它可以通过以下四种方式之一使用：

* `On Error GoTo -1` 会将最近的异常重置为 `Nothing`。

* `On Error GoTo 0` 会将最新的异常处理程序位置重置为 `Nothing`。

* `On Error GoTo LabelName` 将标签建立为最新的异常处理程序位置。 此语句不能用于包含 lambda 或查询表达式的方法中。

* `On Error Resume Next` 会将 @no__t 一行为建立为最新的异常处理程序位置。


#### <a name="resume-statement"></a>Resume 语句

@No__t-0 语句返回对导致最近异常的语句执行。

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

如果指定了 `Next` 修饰符，则执行将返回到在导致最近异常的语句之后执行的语句。 如果指定了标签名称，则执行将返回到标签。

因为 `SyncLock` 语句包含隐式结构化错误处理块，所以 @no__t 为-1，`Resume Next` 对于在 @no__t 3 语句中发生的异常具有特殊行为。 `Resume` 会将执行返回到 `SyncLock` 语句的开头，而 `Resume Next` 则将执行返回到 @no__t 3 语句后面的下一个语句。 例如，考虑以下代码：

```vb
Class LockClass
End Class

Module Test
    Sub Main()
        Dim FirstTime As Boolean = True
        Dim Lock As LockClass = New LockClass()

        On Error GoTo Handler

        SyncLock Lock
            Console.WriteLine("Before exception")
            Throw New Exception()
            Console.WriteLine("After exception")
        End SyncLock

        Console.WriteLine("After SyncLock")
        Exit Sub

Handler:
        If FirstTime Then
            FirstTime = False
            Resume
        Else
            Resume Next
        End If
    End Sub
End Module
```

它打印以下结果。

```console
Before exception
Before exception
After SyncLock
```

第一次通过 `SyncLock` 语句时，`Resume` 会将执行返回到 @no__t 语句的开头。 第二次通过 `SyncLock` 语句时，`Resume Next` 会将执行返回到 @no__t 语句的末尾。 不允许在 `SyncLock` 语句中使用 `Resume` 和 @no__t。

在所有情况下，当执行 `Resume` 语句时，最新异常将设置为 `Nothing`。 如果在没有最近的异常的情况下执行 `Resume` 语句，该语句将引发一个 @no__t 为1的异常，该异常包含 Visual Basic 错误号 `20` （Resume，无错误）。


## <a name="branch-statements"></a>Branch 语句

Branch 语句修改方法中的执行流。 有六个分支语句：

1. @No__t-0 语句会导致执行传输到方法中的指定标签。 不允许在 lambda 或 LINQ 表达式中捕获某个块的本地变量的情况下，将 @no__t-@no__t 0、`Using`、`SyncLock`、`With`、`For` 或 `For Each` 块以及任何循环块。
2. @No__t-0 语句将执行转移到指定种类的直接包含块语句末尾后的下一条语句。 如果块为方法块，则控制流将退出方法，如[控制流](statements.md#control-flow)中所述。 如果未在语句中指定的块类型内包含 `Exit` 语句，则会发生编译时错误。
3. @No__t-0 语句将执行转移到指定种类的立即包含块循环体语句的末尾。 如果未在语句中指定的块类型内包含 `Continue` 语句，则会发生编译时错误。
4. @No__t-0 语句导致引发调试器异常。
5. @No__t-0 语句将终止该程序。 终结器将在关闭前运行，但不会执行任何当前正在执行的 @no__t 0 语句的 finally 块。 此语句不能用于不可执行的程序（例如 Dll）。
6. 不带 expression 的 @no__t 0 语句等效于 @no__t 或 `Exit Function` 语句。 带有表达式的 @no__t 0 语句仅允许在作为函数的常规方法中，或在异步方法中使用，此方法在某种 `T` 的返回类型为 `Task(Of T)` 的函数。 表达式必须归类为一个值，该值可隐式转换为*函数返回变量*（对于常规方法）或*任务返回变量*（对于异步方法）。 其行为是评估其表达式，然后将其存储在返回变量中，然后执行隐式 @no__t 0 语句。

```antlr
BranchStatement
    : GoToStatement
    | ExitStatement
    | ContinueStatement
    | StopStatement
    | EndStatement
    | ReturnStatement
    ;

GoToStatement
    : 'GoTo' LabelName StatementTerminator
    ;

ExitStatement
    : 'Exit' ExitKind StatementTerminator
    ;

ExitKind
    : 'Do' | 'For' | 'While' | 'Select' | 'Sub' | 'Function' | 'Property' | 'Try'
    ;

ContinueStatement
    : 'Continue' ContinueKind StatementTerminator
    ;

ContinueKind
    : 'Do' | 'For' | 'While'
    ;

StopStatement
    : 'Stop' StatementTerminator
    ;

EndStatement
    : 'End' StatementTerminator
    ;

ReturnStatement
    : 'Return' Expression? StatementTerminator
    ;
```

## <a name="array-handling-statements"></a>数组处理语句

以下两个语句简化了使用数组的操作： `ReDim` 语句和 `Erase` 语句。

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a>ReDim 语句

@No__t-0 语句实例化新的数组。

```antlr
RedimStatement
    : 'ReDim' 'Preserve'? RedimClauses StatementTerminator
    ;

RedimClauses
    : RedimClause ( Comma RedimClause )*
    ;

RedimClause
    : Expression ArraySizeInitializationModifier
    ;
```

语句中的每个子句都必须归类为变量或属性访问（其类型为数组类型或 `Object`），后跟数组界限的列表。 边界的数目必须与变量的类型一致;@no__t 允许使用任意数量的界限。 在运行时，将使用指定的边界从左到右对每个表达式实例化一个数组，然后将其分配给变量或属性。 如果变量类型为 `Object`，则维度数量为指定的维数，数组元素类型为 `Object`。 如果给定的维度数在运行时与变量或属性不兼容，则会发生编译时错误。 例如：

```vb
Module Test
    Sub Main()
        Dim o As Object
        Dim b() As Byte
        Dim i(,) As Integer

        ' The next two statements are equivalent.
        ReDim o(10,30)
        o = New Object(10, 30) {}

        ' The next two statements are equivalent.
        ReDim b(10)
        b = New Byte(10) {}

        ' Error: Incorrect number of dimensions.
        ReDim i(10, 30, 40)
    End Sub
End Module
```

如果指定了 @no__t 0 关键字，则还必须将表达式 classifiable 为值，并且除最右边的维度之外的每个维度的新大小必须与现有数组的大小相同。 现有数组中的值将复制到新数组中：如果新数组较小，则放弃现有值;如果新数组较大，则会将额外的元素初始化为数组元素类型的默认值。 例如，考虑以下代码：

```vb
Module Test
    Sub Main()
        Dim x(5, 5) As Integer

        x(3, 3) = 3

        ReDim Preserve x(5, 6)
        Console.WriteLine(x(3, 3) & ", " & x(3, 6))
    End Sub
End Module
```

它打印以下结果：

```console
3, 0
```

如果在运行时现有数组引用为 null 值，则不会给出任何错误。 如果维度的大小发生更改，则不是最右边的维度，将引发 `System.ArrayTypeMismatchException`。

__纪录.__ `Preserve` 不是保留字。


### <a name="erase-statement"></a>Erase 语句

@No__t-0 语句将语句中指定的每个数组变量或属性设置为 `Nothing`。 语句中的每个表达式都必须归类为变量或属性访问，其类型为数组类型或 `Object`。 例如：

```vb
Module Test
    Sub Main()
        Dim x() As Integer = New Integer(5) {}

        ' The following two statements are equivalent.
        Erase x
        x = Nothing
    End Sub
End Module
```

```antlr
EraseStatement
    : 'Erase' EraseExpressions StatementTerminator
    ;

EraseExpressions
    : Expression ( Comma Expression )*
    ;
```

## <a name="using-statement"></a>Using 语句

当集合运行且未找到对实例的实时引用时，垃圾回收器将自动释放类型的实例。 如果类型保存在特别宝贵的资源上（例如数据库连接或文件句柄），则可能不需要等待下一次垃圾回收，从而清除不再使用的类型的特定实例。 为了提供一种在集合之前释放资源的轻型方法，类型可以实现 `System.IDisposable` 接口。 这样做的一种类型将公开一个 @no__t 0 方法，该方法可通过调用来强制立即释放宝贵的资源，如下所示：

```vb
Module Test
    Sub Main()
        Dim x As DBConnection = New DBConnection("...")

        ' Do some work
        ...

        x.Dispose()        ' Free the connection
    End Sub
End Module
```

@No__t-0 语句会自动执行获取资源、执行一组语句，然后释放资源的过程。 语句可以采用两种形式：在一种情况下，资源是声明为语句的一部分并被视为常规局部变量声明语句的本地变量;而在另一种情况下，资源是表达式的结果。

```antlr
UsingStatement
    : 'Using' UsingResources StatementTerminator
      Block?
      'End' 'Using' StatementTerminator
    ;

UsingResources
    : VariableDeclarators
    | Expression
    ;
```

如果资源是局部变量声明语句，则局部变量声明的类型必须是可以隐式转换为 `System.IDisposable` 的类型。 已声明的局部变量是只读的，其作用域为 `Using` 语句块，必须包含初始值设定项。 如果资源是表达式的结果，则表达式必须分类为值，并且必须是可隐式转换为 `System.IDisposable` 的类型。 表达式只在语句的开头计算一次。

@No__t-0 块隐式包含在 @no__t 1 语句中，其 finally 块调用资源上 `IDisposable.Dispose` 方法。 这可确保在引发异常时释放资源。 因此，从块外分支到 `Using` 块并将 @no__t 1 块视为单个语句以用于 `Resume` 和 `Resume Next` 是无效的。 如果资源 `Nothing`，则不会对 `Dispose` 进行调用。 因此，示例如下：

```vb
Using f As C = New C()
    ...
End Using
```

等效于：

```vb
Dim f As C = New C()
Try
    ...
Finally
    If f IsNot Nothing Then
        f.Dispose()
    End If
End Try
```

具有局部变量声明语句的 @no__t 0 语句可以一次获取多个资源，这等同于嵌套的 @no__t 1 语句。  例如，一种形式的 @no__t 0 语句：

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

等效于：

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a>Await 语句

Await 语句与 await 运算符表达式（Section [Await 运算符](expressions.md#await-operator)）具有相同的语法，只允许在也允许使用 await 表达式的方法中使用，并且与 await 运算符表达式具有相同的行为。

但是，它可以归类为值或 void。 对 await 运算符表达式求值后得出的任何值都将被丢弃。

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a>Yield 语句

Yield 语句与迭代器方法相关，详见部分[迭代器方法](statements.md#iterator-methods)。

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

如果 @no__t 为一个保留字，则如果其出现在其中的直接封闭方法或 lambda 表达式具有 `Iterator` 修饰符，并且 `Yield` 出现在该 `Iterator` 修饰符之后，则为; 否则为。它不会保留在别处。 它也不会在预处理器指令中保留。 Yield 语句仅允许在它是保留字的方法或 lambda 表达式的主体中使用。 在直接的封闭方法或 lambda 中，yield 语句不能出现在 `Catch` 或 @no__t 块的主体内，也不能出现在 @no__t 语句的主体内。

Yield 语句采用一个表达式，该表达式必须归类为值，并且其类型可隐式转换为其封闭迭代器方法的*迭代器当前变量*（部分[迭代器方法](statements.md#iterator-methods)）的类型。

在迭代器对象上调用 `MoveNext` 方法时，控制流只会到达 `Yield` 语句。 （这是因为迭代器方法实例只是因为在迭代器对象上调用了 @no__t 0 或 @no__t 方法而执行其语句; 而 @no__t 2 方法只是在不允许使用 `Yield` 的块 @no__t 中执行代码。

执行 `Yield` 语句时，将计算其表达式并将其存储在与该迭代器对象关联的迭代器方法实例的*迭代器当前变量*中。 值 `True` 返回到 @no__t 的调用程序，此实例的控制点将停止前进，直到迭代器对象上的 `MoveNext` 的下一次调用。

