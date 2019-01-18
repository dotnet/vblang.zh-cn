---
ms.openlocfilehash: 68237df3f66ba1cec661e6580c27bb5beedd6b9a
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426647"
---
# <a name="statements"></a>语句

语句表示的可执行代码。

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

__请注意。__ Microsoft Visual Basic 编译器仅允许一个关键字或标识符开头的语句。 因此，例如，调用语句"`Call (Console).WriteLine`"允许，但调用语句"`(Console).WriteLine`"不是。

## <a name="control-flow"></a>控制流

*控制流*是语句和表达式的执行的顺序。 执行顺序取决于特定的语句或表达式。

例如，当评估加法运算符 (部分[加法运算符](expressions.md#addition-operator))，第一个左的操作数的求值，然后右操作数，然后运算符本身。 块被执行 (部分[块和标签](statements.md#blocks-and-labels)) 方法是首先执行其第一个子语句，然后继续操作的块的语句通过逐个。

在此隐式排序是这一概念*控制点*，这是要执行的下一步操作。 当一个方法调用 （或"调用"） 时，我们说它会创建*实例*的方法。 方法实例由其自己的方法的参数和本地变量和其自己的控制点副本组成。

### <a name="regular-methods"></a>正则方法

下面是正则方法的示例

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

常规方法调用时，

1. 首先创建特定于该调用的方法的实例。 此实例包括一份所有参数和局部变量的方法。
2. 然后它的所有参数都初始化为提供的值，以及所有局部变量及其类型的默认值。
3. 情况下`Function`，还初始化隐式的本地变量调用*函数返回变量*其名称是函数的名称，其类型为该函数的返回类型，其初始值为默认值其类型。
4. 第一个语句的方法体中，然后设置方法实例的控点并将方法主体立即开始从该处执行 (部分[块和标签](statements.md#blocks-and-labels))。

控制流退出时的方法体通常-通过到达`End Function`或`End Sub`标记其结束时，或通过使用显式`Return`或`Exit`语句的控制流返回到调用方的方法实例。 如果函数返回变量，调用的结果将是此变量的最终值。

当控制流退出方法正文通过未经处理的异常时，该异常将传播到调用方。

控制流已退出后，不再有任何实时引用方法的实例。 如果方法实例保留到其本地变量或参数的副本的唯一引用，则它们可能是垃圾回收。

### <a name="iterator-methods"></a>迭代器方法

为方便地生成的序列，其中一个可供使用迭代器方法`For Each`语句。 迭代器方法使用`Yield`语句 (部分[Yield 语句](statements.md#yield-statement)) 以提供序列的元素。 (没有迭代器方法`Yield`语句将生成一个空序列)。 下面是迭代器方法的示例：

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

迭代器方法调用时其返回类型是`IEnumerator(Of T)`，

1. 首先被创建特定于该调用迭代器方法的实例。 此实例包括一份所有参数和局部变量的方法。
2. 然后它的所有参数都初始化为提供的值，以及所有局部变量及其类型的默认值。
3. 此外初始化隐式的本地变量调用*迭代器当前变量*，其类型是`T`且其初始值为其类型的默认值。
4. 然后在方法体中的第一个语句设置方法实例的控制点。
5. *迭代器对象*然后创建，则与此方法实例相关联。 迭代器对象实现声明的返回类型，并具有如下所述的行为。
6. 然后继续运行控制流*立即*在调用方，并且调用的结果是迭代器对象。 请注意，此传输不退出迭代器方法实例，便可完成并不会导致最后处理程序执行。 方法实例仍在引用的迭代器对象，并将不会垃圾收集，只要存在对迭代器对象的实时引用。

当迭代器对象`Current`访问属性时，*当前变量*返回调用。

当迭代器对象的`MoveNext`调用方法、 调用不会创建新的方法实例。 而使用现有的方法实例 （和其控点和本地变量和参数） 的第一次调用迭代器方法时创建的实例。 控制流继续执行在该方法实例的控制点，并像平常一样的迭代器方法的正文中继续进行。

当迭代器对象的`Dispose`调用方法，再次使用现有的方法实例。 控制流继续在该方法实例的控制点，但然后立即行为方式如同`Exit Function`语句的下一步操作。

以上描述的行为的调用`MoveNext`或`Dispose`迭代器对象仅适用于以前的所有调用`MoveNext`或`Dispose`该迭代器对象上均已返回到其调用方。 如果它们不具备，则行为是未定义。

控制流退出时的迭代器方法体通常-通过到达`End Function`标记其结束时，或通过显式`Return`或`Exit`语句-它必须具有执行此操作的调用的上下文中`MoveNext`或`Dispose`函数在迭代器对象以恢复迭代器方法实例，并且它将一直在使用时第一次调用迭代器方法创建方法实例。 该实例的控制点将保留为`End Function`语句，并在调用方; 中的控制流恢复并且它必须通过调用已恢复`MoveNext`则将值`False`返回给调用方。

当控制流的退出迭代器方法正文通过未经处理的异常，则异常会传播给调用方，再次将为调用`MoveNext`或`Dispose`。

与其他可能的返回类型的迭代器函数

* 迭代器方法调用时其返回类型是`IEnumerable(Of T)`对于某些`T`、 首次创建实例-特定于迭代器方法--的所有参数在方法中，该调用和初始化使用提供的值。 调用的结果是实现的返回类型的对象。 应此对象的`GetEnumerator`调用方法，它会创建实例-特定于该调用的`GetEnumerator`-的所有参数和局部变量的方法中。 它初始化为已保存的值的参数，并继续执行与上面的迭代器方法。
* 迭代器方法调用时的返回类型为非泛型接口`IEnumerator`，则行为将与用于完全`IEnumerator(Of Object)`。
* 迭代器方法调用时的返回类型为非泛型接口`IEnumerable`，则行为将与用于完全`IEnumerable(Of Object)`。

### <a name="async-methods"></a>异步方法

异步方法是执行长时间运行的工作而不会如阻止的应用程序 UI 的简便方法。 异步是缩写*异步*-表示异步方法的调用方将立即，继续执行，但在将来某些更高版本时，最终完成的异步方法的实例可能会发生。 按照约定异步方法的名称带有后缀"Async"。

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

__请注意。__ 异步方法*不*后台线程上运行。 相反，它们允许方法将自身通过挂起`Await`运算符和计划本身以响应某些事件恢复。

调用异步方法时

1. 首先创建特定于该调用异步方法的实例。 此实例包括一份所有参数和局部变量的方法。
2. 然后它的所有参数都初始化为提供的值，以及所有局部变量及其类型的默认值。
3. 在异步方法具有返回类型的情况下`Task(Of T)`对于某些`T`，还初始化隐式的本地变量调用*任务返回变量*，其类型是`T`且其初始值为默认值为`T`。
4. 如果异步方法`Function`返回类型`Task`或`Task(Of T)`某些`T`，则该类型的对象隐式创建，与当前调用相关联。 这称为*异步对象*和表示将来会通过执行异步方法的实例来完成的工作。 当控件继续在此异步方法实例的调用方时，调用方将收到此异步对象作为其调用的结果。
5. 实例的控点的第一个语句的异步方法正文，然后将设置并立即开始从该处执行方法主体 (部分[块和标签](statements.md#blocks-and-labels))。

__恢复委托和当前调用方__

部分中所述[Await 运算符](expressions.md#await-operator)，执行`Await`表达式具有能够挂起方法实例的控点离开控制流以转到其他位置。 控制流可以更高版本继续点通过调用同一实例的控制点*恢复委托*。 请注意，此挂起不退出异步方法中，便可完成并不会导致最后处理程序执行。 方法实例仍在引用恢复委托和`Task`或`Task(Of T)`结果 （如果有），或将不被作为垃圾收集，只要存在要委托或结果的实时引用。

不妨想象语句`Dim x = Await WorkAsync()`大约为以下语法简写形式：

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

在下面的示例，*当前调用方*方法的实例定义为原始调用方或恢复委托，调用方晚。

当控制流退出异步方法正文--通过到达`End Sub`或`End Function`标记其结束时，或通过使用显式`Return`或`Exit`语句，或通过未经处理的异常-实例的控点设置为方法的末尾。 然后，行为取决于异步方法的返回类型。

* 情况下`Async Function`返回类型`Task`:
  1. 如果控制流通过未经处理的异常，然后退出异步对象的状态将设置为`TaskStatus.Faulted`并将其`Exception.InnerException`属性设置为异常 (除： 如特定实现定义的异常`OperationCanceledException`将其更改为`TaskStatus.Canceled`). 在当前调用方中的控制流继续。
  2. 否则，将异步对象的状态设置为`TaskStatus.Completed`。 在当前调用方中的控制流继续。

     (__注意。__ 整点的任务和异步方法的有趣的是，使是一项任务变得时已完成，然后等待它的任何方法目前具有执行其恢复委托，即它们将不再受阻止。）

* 情况下`Async Function`返回类型`Task(Of T)`某些`T`： 的行为是作为更高版本，除外，在非异常情况下异步对象`Result`属性也设置为任务返回变量的最终值。

* 情况下`Async Sub`:
  1. 如果控制流通过未经处理的异常，然后退出该异常会传播到环境中某些特定于实现的方式。 在当前调用方中的控制流继续。
  2. 否则，控制流只需在当前调用方中恢复。

#### <a name="async-sub"></a>Async Sub

没有的一些特定于 Microsoft 的行为`Async Sub`。

如果`SynchronizationContext.Current`是`Nothing`在调用开始时，然后从 Async Sub 任何未经处理的异常将被发布到线程池。

如果`SynchronizationContext.Current`不是`Nothing`开头的调用，然后`OperationStarted()`上的方法的开头之前该上下文调用和`OperationCompleted()`结束后。 此外，任何未经处理的异常将发布的同步上下文上再次引发异常。

这意味着，在 UI 应用程序中为`Async Sub`UI 线程上调用，它无法处理任何异常都将重新发布 UI 线程。

#### <a name="mutable-structures-in-async-and-iterator-methods"></a>Async 或 iterator 方法中的可变结构

结构通常被视为不良实践和 async 或 iterator 方法不支持可变。 具体而言，每次调用一个结构中的 async 或 iterator 方法将隐式对*复制*在调用其时刻复制该结构。 因此，对于示例中，

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

可执行语句的一组称为一个语句块。 执行语句块的开头的第一个语句块中。 一旦执行一个语句后，下一步按词汇顺序执行语句，除非语句将执行其他位置，或发生异常。

在语句块中，在逻辑行的语句的部门并不重要标签声明语句除外。 标签是一个用于标识可以用作分支语句的目标如语句块内的特定位置标识符`GoTo`。

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


标签声明语句必须位于逻辑行的开头和标签可以是标识符或整数文本。 标签声明语句和调用语句可以包含单个标识符，因为本地行开头的单个标识符始终被视为标签声明语句。 标签声明语句必须始终后跟一个冒号，即使在没有语句遵循同一逻辑线上。

标签具有其自己的声明空间并不会干扰其他标识符。 下面的示例有效，并使用名称变量`x`作为一个参数和一个标签。

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

标签的范围是方法的包含它的正文。

为了便于阅读，涉及多个 substatements 语句生产即使 substatements 每个可能本身上标记为行作为在此规范中，单个生产处理。


### <a name="local-variables-and-parameters"></a>本地变量和参数

如何在前面各节的详细信息时创建方法实例，并使用这些方法的局部变量和参数的副本。 此外，每次输入循环的正文时，新副本的每个部分中所述，在该循环中声明的局部变量[循环语句](statements.md#loop-statements)，并且方法实例现在包含其本地变量的此副本而不是比以前的复制。

所有局部变量将都初始化为其类型的默认值。 本地变量和参数始终是可公开访问的。 它是错误指其声明前面的文本位置中的本地变量，如以下示例所示：

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

在中`F`方法更高版本中，第一个分配给`i`一定不是指在外部作用域声明的字段。 相反，它是指本地变量，并且因为文本上位于前面的变量声明位于错误。 在`G`方法中，后续变量声明是指早期变量声明中相同的本地变量声明中声明的本地变量。

每个块在方法中的创建本地变量声明空间。 名称引入此声明空间，通过在方法体中的本地变量声明和方法，该名称引入的最外层块声明空间方法的参数列表。 块不允许通过嵌套名称的隐藏： 在块中声明一个名称后, 可能不重新声明名称在任何嵌套块。

因此，在以下示例中，`F`并`G`方法是在错误是因为名称`i`在外部块中声明并不能在内部块中重新声明。 但是，`H`并`I`有效方法是因为这两个`i`的在单独的非嵌套块中声明。

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

函数方法时，一个特殊的本地变量是隐式声明与表示该函数的返回值的方法同名的方法主体的声明空间中。 本地变量具有特殊名称解析语义时在表达式中使用。 如果在应被归类为方法组，如调用表达式，为表达式的上下文中使用本地变量，名称解析，该函数，而不是本地变量。 例如：

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

使用括号可能会导致不明确的情况下 (如`F(1)`，其中`F`是其返回类型是一个一维数组的函数); 在所有不明确的情况下，此名称解析为该函数而不是本地变量。 例如：

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

当控制流离开方法主体时，本地变量的值被传递回调用表达式。 如果该方法是一个子例程，没有任何此类隐式局部变量，并且控件只是返回到调用表达式。

## <a name="local-declaration-statements"></a>局部声明语句

局部声明语句声明新的本地变量、 局部常量或静态变量。 *本地变量*并*局部常量*相当于实例变量和常量作用域为该方法和声明相同的方式。 *静态变量*类似于`Shared`是声明的变量和使用`Static`修饰符。

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

静态变量是在方法的调用之间保留其值的局部变量。 非共享方法中声明的静态变量是每个实例： 包含方法的类型的每个实例都有其自己的静态变量副本。 中声明的静态变量`Shared`方法是每种类型的; 只有一个副本的所有实例的静态变量。 虽然本地变量初始化为其类型的默认值为该方法在每个输入时，静态变量初始化的类型或类型实例时仅初始化为其类型的默认值。 不能在结构或泛型方法中声明静态变量。

本地变量、 局部常量和静态变量始终具有公共可访问性和不能指定可访问性修饰符。 如果在局部声明语句上不指定任何类型，以下步骤确定本地声明的类型：

1. 如果声明具有类型字符，字符类型的类型是局部声明的类型。

2. 如果本地声明的局部常量，或如果局部声明使用初始值设定项的本地变量以及正在使用本地变量的类型推理，从初始值设定项的类型推断局部声明的类型。 如果初始值设定项所引用的局部声明，则会发生编译时错误。 （局部常量都需要有初始值设定项。）

3. 如果未使用严格的语义，局部声明语句的类型是隐式`Object`。

4. 否则，将发生编译时错误。

如果未指定类型上有数组大小或数组的类型修饰符的局部声明语句，然后本地声明的类型是具有指定秩的数组和前面的步骤用于确定数组的元素类型。 如果使用本地变量的类型推理，则初始值设定项的类型必须具有与局部声明语句在同一数组形状 （即数组的类型修饰符） 的数组类型。 请注意，可能的推断的元素类型可能仍是数组类型。 例如：

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

如果未指定类型的局部声明语句上具有可以为 null 的类型修饰符，则本地声明的类型是推断出的类型或推断的类型本身可以为 null 的版本，如果它可以为 null 的值类型已中。  如果推断出的类型不是可为 null 的值类型，发生编译时错误。 如果为 null 的类型修饰符和数组大小或数组类型修饰符放置在具有任何类型的局部声明语句中，然后为 null 的类型修饰符视为要应用于数组的元素类型，并使用前面的步骤来确定元素nt 类型。

变量初始值设定项在局部声明语句上的是等效于赋值语句放在声明的文本的位置。 因此，如果执行分支过程局部声明语句中，变量的初始值设定项不会执行。 如果超过一次执行局部声明语句，则执行变量初始值设定项相同数目的时间。 静态变量仅执行其初始值设定项第一次。 如果初始化静态变量时出现异常，静态变量被视为使用静态变量的类型的默认值初始化。

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

```
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

静态局部变量初始值设定项初始化过程是线程安全和异常对受保护。 如果在本地的静态初始值设定项期间会发生异常，静态本地版本将保持其默认值和未初始化。 静态本地的初始值设定项

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

本地变量、 局部常量和静态变量的作用域为该语句块中声明它们。 静态变量的特殊在于它们的名称只能使用一次在整个方法。 例如，不能用来指定具有相同名称的两个静态变量声明，即使它们处于不同的块。


### <a name="implicit-local-declarations"></a>隐式的局部声明

除了局部声明语句、 本地变量也可以声明隐式使用。 使用的名称，不能解决其他事项的简单名称表达式声明该名称的本地变量。 例如：

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

在可以接受表达式分类为变量的表达式上下文中才会发生隐式的局部声明。 此规则的例外是，本地变量不能隐式声明的函数调用表达式、 索引表达式或成员访问表达式目标时。

隐式局部变量将被视为包含方法的开头声明它们。 因此，它们始终作用于整个方法体中，即使内部 lambda 表达式声明。 例如，以下代码：

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

将打印的值`10`。 隐式局部变量被类型化为`Object`如果没有类型字符已附加到的变量名称; 否则变量的类型是字符类型的类型。 本地变量的类型推断不用于隐式局部变量。

如果通过编译环境或指定了显式的局部声明`Option Explicit`、 必须显式声明所有本地变量和不允许隐式变量声明。

## <a name="with-statement"></a>与语句一起使用

一个`With`语句，而无需指定多个时间的表达式的表达式的成员对多个引用。

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

表达式必须分类为一个值，计算一次，在进入块时。 内`With`语句块、 成员访问表达式或开始一段或感叹号字典访问表达式进行计算像`With`它前面有表达式。 例如：

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

它是无效的分支到`With`从块之外的语句块。


## <a name="synclock-statement"></a>SyncLock 语句

一个`SyncLock`语句允许在表达式中，确保多个执行线程不会在同一时间中执行相同的语句上同步语句。

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

表达式必须分类为一个值，计算一次，进入块。 输入时`SyncLock`块中，`Shared`方法`System.Threading.Monitor.Enter`上指定的表达式之前执行的线程具有由表达式返回的对象上的排他锁会阻止调用。 中的表达式的类型`SyncLock`语句必须是引用类型。 例如：

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

上面的示例中同步特定类的实例上`Test`以确保不超过一个执行线程可以添加或减去计数变量中的特定实例一次。

`SyncLock`隐式包含块`Try`语句的`Finally`块调用`Shared`方法`System.Threading.Monitor.Exit`的表达式。 这可确保即使在引发异常时，才会释放锁。 结果是，它是无效的分支到`SyncLock`阻止其块之外和一个`SyncLock`块均被视为单个语句出于`Resume`和`Resume Next`。 上面的示例等同于以下代码：

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

`RaiseEvent`， `AddHandler`，和`RemoveHandler`语句引发事件和动态处理事件。

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a>RaiseEvent 语句

一个`RaiseEvent`语句会通知已发生的特定事件的事件处理程序。

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

中的简单名称表达式`RaiseEvent`语句上被解释为成员查找`Me`。 因此， `RaiseEvent x` ，就好像解释`RaiseEvent Me.x`。 该表达式的结果必须属于事件访问类本身; 中定义的事件不能用于对基类型中定义的事件`RaiseEvent`语句。

`RaiseEvent`语句处理与调用`Invoke`的事件的委托，如果有使用所提供的参数的方法。 如果该委托的值为`Nothing`，不会引发异常。 如果没有任何自变量，则可省略括号。 例如：

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

类`Raiser`更高版本相当于：

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

尽管大多数事件处理程序将自动挂钩通过`WithEvents`变量，它可能需要动态添加和移除事件处理程序在运行时。 `AddHandler` 和`RemoveHandler`语句执行此操作。

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

每个语句采用两个参数： 第一个参数必须属于事件访问的表达式和第二个参数必须是分类为一个值的表达式。 第二个参数的类型必须是与事件访问关联的委托类型。 例如：

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

给定事件`E,`语句调用相关`add_E`或`remove_E`上要添加或删除为该事件处理程序委托的实例方法。 因此，上面的代码等效于：

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

赋值语句将表达式的值分配给一个变量。 有几种类型的分配。

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

赋值运算符右侧的表达式必须分类为一个值时，必须为变量或属性访问类别的赋值运算符左侧和右侧表达式。 表达式的类型必须是隐式转换为的变量或属性访问类型。

如果分配到的变量是引用类型的数组元素，将执行运行时检查以确保表达式与数组元素的类型兼容。 在以下示例中，最后一个分配会导致`System.ArrayTypeMismatchException`引发，因为实例`ArrayList`不能存储在一个元素的`String`数组。

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

如果左侧和右侧的赋值运算符的表达式分类为变量，在赋值语句对变量中存储的值。 如果表达式分类为属性访问，则在赋值语句将属性访问的调用转换`Set`访问器的属性的值替换为参数的值。 例如：

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

如果变量或属性访问的目标类型为值类型，但未归类为变量，发生编译时错误。 例如：

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

请注意分配的语义依赖于类型的变量或正在分配的属性。 如果被赋值的变量是值类型，分配将表达式的值复制到该变量。 如果被赋值的变量为引用类型，分配将的参考，不是值本身，复制到该变量。 如果变量的类型为`Object`，由赋值语义值的类型在运行时是否是值类型或引用类型。


__请注意。__ 对于内部类型，如`Integer`和`Date`，引用和值分配语义是相同，因为类型是固定不变。 因此，语言是随意装箱的内部类型上使用引用赋值，作为一种优化。 从值的角度，结果是相同的。

因为等号字符 (`=`) 用于分配和为确定相等性，没有简单的赋值和调用语句中的情况下之间产生歧义如`x = y.ToString()`。 在这种情况下，在赋值语句将优先级高于相等运算符。 这意味着该示例表达式被视为`x = (y.ToString())`而非`(x = y).ToString()`。


### <a name="compound-assignment-statements"></a>复合赋值语句

一个*复合赋值语句*采用形式`V op= E`(其中`op`是一个有效的二进制运算符)。

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

赋值运算符左侧和右侧的表达式必须分类为变量或属性的访问权限，而赋值运算符右侧的表达式必须分类为一个值。 复合赋值语句是等效于语句`V = V op E`二者的区别，复合赋值运算符左侧的变量只计算一次。 以下示例演示了这一差异：

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

表达式`a(GetIndex())`计算两次以简单的赋值，但仅一的复合赋值，因此该代码会输出：

```
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a>Mid 赋值语句

一个`Mid`赋值语句将字符串分配到另一个字符串。 分配的左侧和右侧具有相同的语法与函数调用`Microsoft.VisualBasic.Strings.Mid`。

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

第一个参数是赋值的目标，必须分类为变量或属性访问的类型是隐式转换到和从`String`。 第二个参数是对应于分配的应在目标字符串起始位置和必须分类为其类型必须可隐式转换为一个值从 1 开始的起始位置`Integer`。 可选的第三个参数是从右侧的字符数要将分配到目标字符串的值，必须被归类为值的类型隐式转换为`Integer`。 右侧是源字符串，必须被归类为值的类型隐式转换为`String`。 右侧的数字将被截断为长度参数，如果指定，并替换在左侧的字符串中，从开始位置开始的字符。 如果右侧字符串包含更少字符数的第三个参数，仅从右侧的字符串的字符将被复制。

下面的示例显示`ab123fg`:

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

__请注意。__ `Mid` 不是保留的字。


## <a name="invocation-statements"></a>调用语句

调用语句调用的方法前面加上可选关键字`Call`。 有一些区别，如下所示情况下，调用语句处理函数调用表达式的方式相同。 调用表达式必须分类，作为一个值或 void。 调用表达式计算得到的任何值将被放弃。

如果`Call`省略了关键字，则调用表达式必须启动与标识符或关键字，或使用`.`内`With`块。 因此，例如，"`Call 1.ToString()`"是有效的语句，但"`1.ToString()`"不是。 （请注意，在表达式上下文中，调用表达式还需要启动标识符。 例如，"`Dim x = 1.ToString()`"是有效的语句)。

没有调用语句和调用表达式的另一个区别： 如果调用语句包括参数列表，则始终被视为调用的参数列表。 下面的示例阐释了差异：

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

条件语句允许基于在运行时计算的表达式的语句的条件执行。

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a>如果...然后...Else 语句

`If...Then...Else`语句是基本的条件语句。

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

在每个表达式`If...Then...Else`语句必须是布尔表达式，部分所述[布尔表达式](expressions.md#boolean-expressions)。 (注意： 这不需要具有布尔类型的表达式)。 如果中的表达式`If`语句为 true，这些语句括`If`块执行。 如果表达式为 false，每个`ElseIf`计算表达式。 如果一个的`ElseIf`表达式的计算结果为 true，则执行相应的块。 如果任何表达式的计算结果为 true，并且没有`Else`块中，`Else`执行块。 完成一个块执行后，执行将传递到末尾`If...Then...Else`语句。

行版本`If`语句具有语句时要执行的一组`If`表达式是`True`和一组可选如果表达式是执行语句`False`。 例如：

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

If 的行版本语句绑定小于紧密":"，并将其`Else`绑定到从词法上紧靠在前面`If`的允许的语法。 例如，以下两个版本是等效的：

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

标签声明语句以外的所有语句都允许在行内`If`语句，其中包括块语句。 但是，他们不能将 LineTerminators StatementTerminators 作为多行 lambda 表达式内部除外。 例如：

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

一个`Select Case`语句执行基于表达式的值的语句。

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

表达式必须分类为一个值。 当`Select Case`执行语句时，`Select`首先，计算表达式和`Case`文本声明顺序然后计算语句。 第一个`Case`语句的计算结果为`True`具有执行其块。 如果没有`Case`语句的计算结果为`True`并且没有`Case Else`语句中，执行此块。 后一个块执行完成后，执行将传递到末尾`Select`语句。

执行`Case`块不允许"贯穿"到下一步的开关部分。 这可以防止在其他语言中出现的 bug 的公共类时`Case`终止语句无意中遗漏了。 下面的示例阐释了这种行为：

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

该代码会输出：

```
x = 10
```

尽管`Case 10`并`Case 20 - 10`相同的值，选择`Case 10`因为它位于之前执行`Case 20 - 10`文本上。 在下一步`Case`是达到之后, 继续执行`Select`语句。

一个`Case`子句可能采用两种形式。 一种形式是一个可选`Is`关键字、 比较运算符和表达式。 转换为的类型的表达式`Select`表达式; 如果表达式不是隐式转换为的类型`Select`表达式中，会发生编译时错误。 如果`Select`表达式是*E*，比较运算符是*Op*，和`Case`表达式是*E1*，这种情况被评估为*EOP E1*。 运算符必须是有效类型的两个表达式中;否则会发生编译时错误。

另一种形式是可以选择后跟关键字表达式`To`和第二个表达式。 这两个表达式被转换为的类型`Select`表达式; 如果任一表达式不能隐式转换为的类型`Select`表达式中，会发生编译时错误。 如果`Select`表达式是`E`，第一个`Case`表达式`E1`，，第二个`Case`表达式是`E2`，则`Case`计算为`E = E1`(如果没有`E2`指定) 或`(E >= E1) And (E <= E2)`。 必须是有效类型的两个表达式中;否则会发生编译时错误。


## <a name="loop-statements"></a>循环语句

循环语句在其主体中允许重复的执行语句。

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

每次输入循环主体时，所有本地声明的变量初始化的变量的以前的值为该正文中所做的最新副本。 为循环主体内的变量的任何引用都将使用最近发出的副本。 此代码显示了示例：

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

此代码生成输出：

```
31    32    33
```

当执行循环主体时，它使用变量的任何副本是最新。 例如，语句`Dim y = x`的最新副本是指`y`的原始副本和`x`。 并创建 lambda 时，它会记住的变量的任何副本时的当前的创建的时间。 因此，每个 lambda 使用的相同的共享的副本`x`，但另一份`y`。 在程序结束时，lambda，在执行时，共享的副本`x`它们都代表现在位于其最终值 3。

请注意，是否没有任何 lambda 或 LINQ 表达式，则不可能知道的最新副本对循环条目。 实际上，编译器优化可以避免这种情况下创建副本。 另请注意，它是到非法`GoTo`到循环包含 lambda 或 LINQ 表达式。


### <a name="whileend-while-and-doloop-statements"></a>段时间...结束时和执行操作...循环语句

一个`While`或`Do`循环语句基于布尔表达式的循环。

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

一个`While`循环语句循环，只要布尔表达式的计算结果为 true;`Do`循环语句可以包含更复杂的条件。 表达式可能会放置后`Do`关键字后或`Loop`关键字，但这两者不之后。 布尔表达式计算部分所述[布尔表达式](expressions.md#boolean-expressions)。 (注意： 这不需要具有布尔类型的表达式)。 它也是有效; 所有指定的表达式在这种情况下，将永远不会退出循环。 如果表达式位于后`Do`，它将计算在每次迭代执行循环块之前。 如果表达式位于后`Loop`，在每次迭代执行循环块后它进行评估。 将后面的表达式`Loop`因此将生成一个详细循环之后放置于`Do`。 下面的示例演示此行为：

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

此代码生成输出：

```
Second Loop
```

在第一个循环的情况下对条件进行评估之前循环执行。 对于第二个循环，循环执行之后执行条件。 条件表达式必须带有前缀`While`关键字或`Until`关键字。 前者会中断该循环，如果条件的计算结果为 false，后者的条件的计算结果为 true 时。

__请注意。__ `Until` 不是保留的字。


### <a name="fornext-statements"></a>为...下一语句

一个`For...Next`基于边界的一组语句循环。 一个`For`语句指定循环控制变量、 更低绑定表达式、 一个上限绑定表达式，以及可选步骤值表达式。 指定循环控制变量通过标识符后跟一个可选`As`子句或表达式。

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

按照以下规则，循环控制变量是指到特定的新本地变量这`For...Next`语句，或到预先存在的变量，或表达式。

* 如果循环控制变量的标识符`As`子句中，标识符定义中指定的类型的一个新局部变量`As`子句，作用域为整个`For`循环。

* 如果循环控制变量是标识符，而无需`As`子句，则该标识符首次使用解析简单名称解析规则 (请参阅部分[简单名称表达式](expressions.md#simple-name-expressions))，只不过，此匹配项标识符将不本身会导致要创建一个隐式局部变量 (部分[隐式的局部声明](statements.md#implicit-local-declarations))。

  * 如果此解析成功，并且结果分类为变量，然后循环控制变量是该预先存在的变量。

  * 如果解析失败，或如果解析成功，并且结果分类为一种类型，然后：
    * 如果正在使用本地变量的类型推理，标识符会定义从绑定推断其类型的新本地变量和单步表达式，作用域为整个`For`循环;
    * 如果不使用本地变量的类型推理但隐式的局部声明，则隐式的本地变量，将创建其作用域是整个方法 (部分[隐式的局部声明](statements.md#implicit-local-declarations))，并循环控制变量引用此预先存在的变量;
    * 如果使用本地变量的类型推理既不隐式本地声明，它是错误的。

  * 如果解析成功且内容分类为一个类型既不变量，它是错误的。

* 如果循环控制变量是一个表达式，表达式必须分类为变量中。

循环控制变量不能由另一个封闭`For...Next`语句。 循环控制变量的类型`For`语句确定迭代的类型，并且必须之一：

* `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`
* 一个枚举的类型
* `Object`
* 一种类型`T`具有以下运算符，其中`B`是可以使用布尔表达式中的类型：

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

绑定和步长表达式必须可隐式转换为循环控制变量的类型，必须被归类为值。 在编译时，通过选择更低绑定、 上边界和步骤表达式类型之间最宽的类型推断的循环控制变量的类型。 如果有两个类型之间没有扩大转换，则会发生编译时错误。

在运行时，如果循环控制变量的类型为`Object`，迭代的类型将被推断为在编译时，使用两种例外情况相同。 首先，如果边界和步骤表达式是整型类型的所有但具有无最宽的类型，则将推断最大的类型包含所有三种类型。 其次，如果循环控制变量的类型推断为`String`，`Double`将改为推断。 如果在运行时，可以确定没有循环控件类型或任何表达式不能转换为循环控制类型`System.InvalidCastException`会发生。 循环控件类型选择循环的开头后, 将在迭代中，而不考虑为循环控制变量中的值所做的更改使用相同的类型。

一个`For`语句必须是匹配的关闭`Next`语句。 一个`Next`而无需变量的语句匹配最内部的开放`For`语句，而`Next`与一个或多个循环控制变量的语句，从左到右，匹配`For`匹配每个变量的循环。 如果变量与匹配`For`此时不嵌套最深的循环的循环，会导致编译时错误。

在循环开始时，三个表达式按文本顺序计算和更低绑定表达式被分配给循环控制变量。 如果省略步骤值，它隐式为字面值`1`，已转换为循环控制变量的类型。 三个表达式只计算循环的开头。

在每个循环开始时，控制变量进行比较，以查看其是否大于终结点的步骤表达式是肯定的还是小于终结点，如果步骤表达式为负。 如果是，`For`循环将终止; 否则该循环块执行。 如果循环控制变量不是基元类型，比较运算符将由是否表达式`step >= step - step`是 true 还是 false。 在`Next`语句的步长值添加到控件变量和执行将返回到循环的顶部。

请注意，循环控制变量的新副本*不*创建循环块的每次迭代。 在这一方面`For`语句不同于`For Each`(部分[为每个...下一语句](statements.md#for-eachnext-statements))。

它不是有效的分支到`For`从循环在循环外。


### <a name="for-eachnext-statements"></a>每个...下一语句

一个`For Each...Next`语句基于在表达式中的元素的循环。 一个`For Each`语句指定循环控制变量和枚举器表达式。 指定循环控制变量通过标识符后跟一个可选`As`子句或表达式。

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

遵循相同的规则`For...Next`语句 (部分[对于。 ...下一语句](statements.md#fornext-statements))，循环控制变量是指到特定的新本地变量这对于每个...下一个语句，或到预先存在的变量，或表达式。

枚举器表达式必须分类为一个值，并且其类型必须是集合类型或`Object`。 如果枚举器表达式的类型为`Object`，则所有的处理延迟到运行时。 否则，必须存在从转换集合的元素类型为循环控制变量的类型。

循环控制变量不能由另一个封闭`For Each`语句。 一个`For Each`语句必须是匹配的关闭`Next`语句。 一个`Next`语句而循环控制变量不匹配的最内部打开`For Each`。 一个`Next`与一个或多个循环控制变量的语句，从左到右，匹配`For Each`具有相同的循环控制变量的循环。 如果变量与匹配`For Each`此时不嵌套最深的循环的循环，会发生编译时错误。

一种类型`C`称为*集合类型*如果一个的：

* 以下所有项都为 true:
  * `C` 包含可访问的实例，共享或具有签名的扩展方法`GetEnumerator()`返回类型`E`。
  * `E` 包含可访问的实例，共享或具有签名的扩展方法`MoveNext()`和返回类型`Boolean`。
  * `E` 包含可访问的实例或共享的属性名为`Current`有一个 getter。 此属性的类型是集合类型的元素类型。

* 实现接口`System.Collections.Generic.IEnumerable(Of T)`，在这种情况下被视为集合的元素类型可`T`。

* 实现接口`System.Collections.IEnumerable`，在这种情况下被视为集合的元素类型可`Object`。

以下是可以枚举某个类的示例：

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

在循环开始之前，枚举器表达式计算。 如果表达式的类型不满足设计模式，则表达式转换为`System.Collections.IEnumerable`或`System.Collections.Generic.IEnumerable(Of T)`。 如果表达式类型实现泛型接口，泛型接口是在编译时首选，但非泛型接口是在运行时首选。 如果表达式类型实现泛型接口多次，该语句被视为不明确，则发生编译时错误。

__请注意。__ 因为选取泛型接口意味着对接口方法的所有调用将都涉及类型参数，将在后期绑定的情况下，首选的非泛型接口。 由于不可能知道在运行时类型参数匹配，所有此类调用将需要进行使用后期绑定调用。 这会慢于调用非泛型接口，因为无法使用编译时调用调用非泛型接口。

`GetEnumerator` 对生成的值并返回调用函数的值存储在临时的。 然后在每次迭代开头`MoveNext`称为临时磁盘上。 如果它返回`False`，循环将终止。 否则，每次迭代循环的执行，如下所示：

1. 如果循环控制变量确定新的本地变量 （而不是一个预先存在），然后创建这个本地变量的最新副本。 对于当前迭代循环块内的所有引用将都引用到此副本。
2. `Current`检索属性，强制转换为的类型的循环控制变量 （无论转换是隐式或显式），并分配给循环控制变量。
3. 循环块执行。

__请注意。__ 没有版本 10.0 和 11.0 的语言之间的行为会略有变化。 在 11.0 之前, 是全新的迭代变量*不*创建每个迭代的循环。 这种差异的迭代变量捕获的 lambda 或 LINQ 表达式，然后调用循环后，才是可观察量：

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

Visual Basic 10.0 中，到此生成在编译时警告并打印"3"三次。 这是循环的因为时只有一个单一变量"x"共享的所有迭代中，并将所有三个 lambda 捕获的同一个"x"，然后由 lambda 的执行的时间它然后保留在数字 3。 截至 Visual Basic 11.0，会输出"1、 2、 3"。 这是因为每个 lambda 捕获不同的变量"x"。


__请注意。__ 迭代的当前元素被转换为循环控制变量的类型，即使该转换是显式的因为没有引入在语句中的转换运算符不方便的位置。 使用现已过时类型时，这成为觉得很麻烦`System.Collections.ArrayList`，因为其元素类型是`Object`。 这会需要强制转换中很多次，这正是我们感觉不是理想之选。 泛型讽刺的是，启用创建强类型集合， `System.Collections.Generic.List(Of T)`，可能会这做我们重新考虑此设计点，但为兼容性的起见，不能更改此现在。


当`Next`语句到达时，执行将返回到循环的顶部。 如果指定的变量后`Next`关键字，它必须是后的第一个变量相同`For Each`。 例如，考虑以下代码：

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

如果类型`E`枚举器的实现`System.IDisposable`，然后通过调用退出循环时释放枚举器`Dispose`方法。 这可确保释放枚举数控制的资源。 如果方法包含`For Each`语句不使用非结构化的错误处理，则`For Each`语句包装在`Try`语句`Dispose`方法中调用`Finally`以确保清理。

__请注意。__ `System.Array`类型是集合类型，并且由于所有数组类型派生`System.Array`，数组类型的任何表达式中允许`For Each`语句。 对于一维数组，`For Each`语句枚举中递增的索引顺序，从索引 0 开始和结束索引长度为-1 的数组元素。 对于多维数组，第一次增加的最右边的维度的索引。

例如，下面的代码输出`1 2 3 4`:

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

它不是有效的分支到`For Each`从块外的语句块。


## <a name="exception-handling-statements"></a>异常处理语句

Visual Basic 支持结构化的异常处理和非结构化的异常处理。 只有一个样式异常处理中使用的是一种方法，但`Error`语句中使用的是结构化的异常处理。 如果一种方法使用两种风格的异常处理，会编译时错误。

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a>结构化的异常处理语句

结构化的异常处理是一种通过声明将在其中处理特定异常的显式代码块来处理错误的方法。 结构化的异常处理是通过`Try`语句。

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

一个`Try`语句由三种类型的块组成： try 块，catch 块和 finally 块。 一个*try 块*是包含要执行的语句的语句块。 一个*catch 块*是处理的异常的语句块。 一个*finally 块*是包含要在运行时语句的语句块`Try`退出语句后，无论异常是否已发生，并且已处理。 一个`Try`语句只能包含一个 try 块和一个 finally 块，必须包含至少一个 catch 块或 finally 块。 若要显式将执行转移到 try 块除外从在同一语句中的 catch 块中无效。


#### <a name="finally-blocks"></a>Finally 块

一个`Finally`当执行离开的任何部分时始终执行块`Try`语句。 执行所需的任何明确的操作`Finally`块; 当执行离开`Try`语句中，系统将自动执行`Finally`阻止，然后将执行传输到其预期目标。 `Finally`而不考虑执行的会保留执行块`Try`语句： 月底`Try`块中的，通过末尾`Catch`阻止，请通过`Exit Try`语句，通过`GoTo`语句，或通过不处理引发的异常。

请注意，`Await`异步方法的表达式和`Yield`语句中迭代器方法，可能会导致在 async 或 iterator 方法实例暂停和恢复方法是其他一些实例中的控制流。 但是，这是只是执行的挂起和不涉及退出各自的异步方法或迭代器方法实例，并因此不会导致`Finally`块执行。

是无效的显式传输到执行`Finally`阻止; 也是无效的传输共执行`Finally`阻止除通过异常。

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a>catch 块

如果在处理时出现异常`Try`块，每个`Catch`语句检查按文本顺序，以确定它是否处理异常。

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

中指定的标识符`Catch`子句表示已引发的异常。 如果该标识符含有`As`子句，则该标识符被视为在内部进行声明`Catch`块的局部声明空间。 否则，标识符必须是包含块中定义的本地变量 （而不是静态变量）。

一个`Catch`不带标识符的子句将捕获所有异常派生自`System.Exception`。 一个`Catch`标识符子句仅将捕获的异常的类型将与相同或派生自类型的标识符。 类型必须是`System.Exception`，或从派生的类型`System.Exception`。 当捕获了一个异常派生自`System.Exception`，对异常对象的引用存储在该函数返回的对象中`Microsoft.VisualBasic.Information.Err`。

一个`Catch`子句`When`子句仅将捕获异常时表达式计算结果为`True`; 表达式的类型必须是部分所述的布尔表达式[布尔表达式](expressions.md#boolean-expressions)。 一个`When`子句只应用在检查异常的类型之后，该表达式可能指表示异常的标识符，如本示例所示：

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

此示例输出：

```
Third handler
```

如果`Catch`子句处理异常、 执行会转移到`Catch`块。 在末尾`Catch`阻止到后面的第一个语句执行传输`Try`语句。 `Try`语句将处理中引发任何异常`Catch`块。 如果没有`Catch`子句处理的异常执行传输到一个位置通过系统。

是无效的显式传输到执行`Catch`块。

当子句通常计算之前所引发的异常筛选器。 例如，下面的代码将打印"筛选器，最后，Catch"。

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

 但是，Async 或 Iterator 方法导致所有最后在它们在之外的任何筛选器之前执行中的块。 例如，如果上面的代码有`Async Sub Foo()`，则输出将为"最后，筛选器中，捕获"。


#### <a name="throw-statement"></a>Throw 语句

`Throw`语句将引发异常，它派生自类型的实例所表示`System.Exception`。

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

如果表达式不分类为一个值，或者不是类型派生自`System.Exception`，则会发生编译时错误。 如果在运行时，该表达式的计算结果为 null 值则`System.NullReferenceException`改为引发异常。

一个`Throw`语句可能会省略的 catch 块中的表达式`Try`语句，前提是没有任何干预性 finally 块。 在这种情况下，该语句重新引发在 catch 块中当前正在处理的异常。 例如：

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


### <a name="unstructured-exception-handling-statements"></a>非结构化的异常处理语句

非结构化的异常处理是通过指示语句可分支到发生异常时处理错误的方法。 使用三个语句实现非结构化的异常处理：`Error`语句中，`On Error`语句，和`Resume`语句。

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

当一种方法使用非结构化的异常处理时，单个结构化的异常处理程序建立的整个捕获所有异常的方法。 (请注意，在构造函数中此处理程序不会扩展到对调用的调用通过`New`构造函数的开头。)该方法然后跟踪的最新的异常处理程序位置和最近已引发的异常。 在进入方法时，异常处理程序位置和异常均设置为`Nothing`。 使用非结构化的异常处理的方法中引发异常，异常对象的引用存储在该函数返回的对象`Microsoft.VisualBasic.Information.Err`。

迭代器或异步方法中不允许非结构化的错误处理语句。


#### <a name="error-statement"></a>Error 语句

`Error`语句，则会引发`System.Exception`异常包含 Visual Basic 6 异常数。 表达式必须分类为一个值，其类型必须是隐式转换为`Integer`。

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a>On Error 语句

`On Error`的语句修改最新的异常处理状态。

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

可在四种方法之一：

* `On Error GoTo -1` 重置的最新例外`Nothing`。

* `On Error GoTo 0` 重置为最新的异常处理程序位置`Nothing`。

* `On Error GoTo LabelName` 确定为最新的异常处理程序位置的标签。 此语句不能包含 lambda 或查询表达式的方法中使用。

* `On Error Resume Next` 建立`Resume Next`最新的异常处理程序位置的行为。


#### <a name="resume-statement"></a>Resume 语句

一个`Resume`语句执行返回给导致的最新异常的语句。

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

如果`Next`指定修饰符，则执行将返回到会导致最新的异常的语句之后执行的语句。 如果指定标签名称，则执行将返回到标签。

因为`SyncLock`语句包含一个隐式结构化的错误处理块`Resume`并`Resume Next`具有特殊行为中发生的异常`SyncLock`语句。 `Resume` 执行返回给开头`SyncLock`语句，而`Resume Next`后面的下一个语句将执行返回`SyncLock`语句。 例如，考虑以下代码：

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

它将打印以下结果。

```
Before exception
Before exception
After SyncLock
```

第一次`SyncLock`语句中，`Resume`执行返回给开头`SyncLock`语句。 第二次遍历`SyncLock`语句中，`Resume Next`执行返回给末尾`SyncLock`语句。 `Resume` 并`Resume Next`中不允许使用`SyncLock`语句。

在所有情况下，当`Resume`执行语句，最新的异常将设置为`Nothing`。 如果`Resume`会有任何最新异常执行语句，该语句将引发`System.Exception`异常包含 Visual Basic 错误号`20`（无错误恢复）。


## <a name="branch-statements"></a>分支语句

分支语句修改的方法中的执行流。 有六个分支语句：

1. 一个`GoTo`语句会导致执行传递到方法中指定的标签。 不允许向`GoTo`成`Try`， `Using`， `SyncLock`， `With`，`For`或`For Each`块中，也不到任何循环块，如果 lambda 或 LINQ 表达式中捕获本地变量的块。
2. `Exit`语句将执行转移到的指定类型的直接包含的块语句结束后的下一个语句。 如果块是方法的块，则控制流退出方法部分中所述[控制流](statements.md#control-flow)。 如果`Exit`语句不包含在指定在语句中，将发生编译时错误的块类型。
3. 一个`Continue`语句将执行转移到的指定类型的直接包含的块循环语句的末尾。 如果`Continue`语句不包含在指定在语句中，将发生编译时错误的块类型。
4. 一个`Stop`语句会导致调试器异常发生。
5. `End`语句将终止该程序。 终结器运行之前关闭，但 finally 块的任何当前正在执行的`Try`不执行语句。 不可能在不是可执行文件 （例如 Dll） 的程序中使用此语句。
6. 一个`Return`不包含表达式的语句是等效于`Exit Sub`或`Exit Function`语句。 一个`Return`是一个函数的正则方法中仅允许使用表达式的语句或在异步方法的是具有返回类型的函数`Task(Of T)`某些`T`。 表达式必须分类为一个值，该值是隐式转换为*函数返回变量*（在正则方法中） 或*任务返回变量*（在异步方法）. 其行为是评估其表达式，然后将其存储在返回的变量，然后执行隐式`Exit Function`语句。

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

两个语句简化与数组一起使用的工作：`ReDim`语句和`Erase`语句。

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a>ReDim 语句

一个`ReDim`语句实例化新数组。

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

在语句中的每个子句必须分类为变量或属性访问其类型是数组类型或`Object`，并后跟一系列数组界限。 边界数必须符合的类型的变量，例如：允许任意数量的边界`Object`。 在运行时，数组为每个表达式从左到右指定的边界内实例化，然后分配给变量或属性。 如果该变量的类型是`Object`的维数是指定，维数的数组元素类型是`Object`。 如果给定的维度数与不兼容的变量或属性在运行时将发生编译时错误。 例如：

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

如果`Preserve`指定关键字，则表达式必须也是与可分类为一个值，且除最右侧的每个维度的新大小必须为现有数组的大小相同。 现有数组中的值复制到新数组： 如果较小的新数组，现有值将被丢弃;如果更大的新数组，额外的元素将初始化为数组的元素类型的默认值。 例如，考虑以下代码：

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

它将打印以下结果：

```
3, 0
```

如果现有数组引用 null 值在运行时，会给不出任何错误。 最右边的维度，维度的大小发生更改，如果之外`System.ArrayTypeMismatchException`将引发。

__请注意。__ `Preserve` 不是保留的字。


### <a name="erase-statement"></a>Erase 语句

`Erase`语句设置的每个数组变量或属性为语句中指定`Nothing`。 在语句中的每个表达式必须分类为其类型是数组类型的变量或属性访问或`Object`。 例如：

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

运行一个集合并找到对实例的实时引用时，垃圾回收器自动释放类型的实例。 如果类型为尤其有价值且稀缺资源 （如数据库连接或文件句柄） 上，它可能不需要等到下一步的垃圾回收，以清理不再使用的类型的特定实例。 若要提供释放集合前的资源的一个简便的方法，一种类型可能会实现`System.IDisposable`接口。 公开的类型`Dispose`可以调用来强制立即，这种情况下释放资源有价值的方法：

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

`Using`语句可获取资源、 执行一组语句，然后释放资源的过程进行自动化。 该语句可以采用两种形式： 在其中一个，资源是本地变量声明语句的一部分为和视为常规的本地变量声明语句;中的其他资源是表达式的结果。

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

如果资源是本地变量声明语句则局部变量声明的类型必须可以隐式转换为类型`System.IDisposable`。 声明的局部变量是只读的作用域为`Using`语句块，并且必须包含初始值设定项。 如果资源是表达式的结果，则该表达式必须分类为一个值，并且必须可以隐式转换为类型的`System.IDisposable`。 在语句开始处仅一次，计算表达式。

`Using`隐式包含块`Try`其 finally 块的语句调用的方法`IDisposable.Dispose`资源上。 这可确保即使在引发异常时释放资源。 结果是，它是无效的分支到`Using`阻止其块之外和一个`Using`块均被视为单个语句出于`Resume`和`Resume Next`。 如果资源是`Nothing`，则没有调用`Dispose`进行。 因此，下面的示例：

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

一个`Using`具有的本地变量声明语句的语句可能获取多个资源，一次，这是相当于嵌套`Using`语句。  例如，`Using`窗体的语句：

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

Await 语句具有与 await 运算符表达式相同的语法 (部分[Await 运算符](expressions.md#await-operator))，只能在还允许 await 表达式的方法中，并且具有与 await 运算符表达式相同的行为。

但是，它会归类为值或 void。 Await 运算符表达式计算得到的任何值将被放弃。

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a>Yield 语句

Yield 语句与迭代器方法，部分所述[迭代器方法](statements.md#iterator-methods)。

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

`Yield` 是一个保留的字，如果它在其中出现的立即封闭方法或 lambda 表达式具有`Iterator`修饰符，并且如果`Yield`后，将显示`Iterator`修饰符; 它在其他位置未保留空间。 它也是在预处理器指令未保留空间。 Yield 语句仅允许在其所在的保留的字的方法或 lambda 表达式的正文中。 在最近的封闭方法或 lambda，yield 语句可能不会发生的表体内`Catch`或`Finally`块中，也不内部的正文`SyncLock`语句。

Yield 语句采用单个表达式这必须分类为一个值，且其类型为隐式转换为的类型*迭代器当前变量*(部分[迭代器方法](statements.md#iterator-methods)) 的其封闭迭代器方法。

控制流只达到`Yield`语句时`MoveNext`迭代器对象上调用方法。 (这是因为迭代器方法实例只会执行其语句由于`MoveNext`或`Dispose`迭代器对象; 调用的方法和`Dispose`方法将只会执行中的代码`Finally`块，其中`Yield`不允许)。

当`Yield`执行语句、 计算和存储在其表达式*迭代器当前变量*的与该迭代器对象关联的迭代器方法实例。 该值`True`返回到调用方`MoveNext`，和此实例的控制点停止之前的下一个调用前调`MoveNext`上迭代器对象。

