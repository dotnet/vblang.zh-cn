---
ms.openlocfilehash: a7e901ccba9f89ff7c414cceeccb3660332b0d53
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426646"
---
# <a name="type-members"></a>类型成员

类型成员定义的存储位置和可执行代码。 它们可以是方法、 构造函数、 事件、 常量、 变量和属性。

## <a name="interface-method-implementation"></a>接口方法实现

方法、 事件和属性可以实现接口成员。 若要实现接口成员，成员声明指定`Implements`关键字，并列出一个或多个接口成员。

```antlr
ImplementsClause
    : ( 'Implements' ImplementsList )?
    ;

ImplementsList
    : InterfaceMemberSpecifier ( Comma InterfaceMemberSpecifier )*
    ;

InterfaceMemberSpecifier
    : NonArrayTypeName Period IdentifierOrKeyword
    ;
```

方法和实现接口成员的属性是隐式`NotOverridable`除非声明为`MustOverride`， `Overridable`，或重写另一个成员。 它是错误的成员实现接口成员为`Shared`。 成员的可访问性不起作用的能力实现接口成员上。

有效的接口实现，包含类型的实现列表必须将命名包含兼容的成员的接口。 兼容的成员是成员的一个其签名与实现的签名匹配。 如果正在实现泛型接口，然后 Implements 子句中提供的类型实参将被替换为签名时检查兼容性。 例如：

```vb
Interface I1(Of T)
    Sub F(x As T)
End Interface

Class C1
    Implements I1(Of Integer)

    Sub F(x As Integer) Implements I1(Of Integer).F
    End Sub
End Class

Class C2(Of U)
    Implements I1(Of U)

    Sub F(x As U) Implements I1(Of U).F
    End Sub
End Class
```

如果使用的事件声明委托类型实现接口的事件，则兼容事件是一个基础委托类型是相同的类型。 否则，该事件将使用它正在实现的接口事件的委托类型。 如果这种情况下实现接口的多个事件，所有的接口事件必须具有相同的基础委托类型。 例如：

```vb
Interface ClickEvents
    Event LeftClick(x As Integer, y As Integer)
    Event RightClick(x As Integer, y As Integer)
End Interface

Class Button
    Implements ClickEvents

    ' OK. Signatures match, delegate type = ClickEvents.LeftClickHandler.
    Event LeftClick(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick

    ' OK. Signatures match, delegate type = ClickEvents.RightClickHandler.
    Event RightClick(x As Integer, y As Integer) _
        Implements ClickEvents.RightClick
End Class

Class Label
    Implements ClickEvents

    ' Error. Signatures match, but can't be both delegate types.
    Event Click(x As Integer, y As Integer) _
        Implements ClickEvents.LeftClick, ClickEvents.RightClick
End Class
```

使用类型名称、 句点和标识符来指定接口成员实现列表中。 类型名称必须是接口实现列表中的或在实现列表中，接口的基接口和标识符必须是指定接口的成员。 单个成员可以实现多个匹配的接口成员。

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Class Test
    Implements ILeft, IRight

    Sub F() Implements ILeft.F, IRight.F
    End Sub
End Class
```

如果所实现的接口成员一直致力于为所有显式实现的接口，因为存在多个接口继承，实现成员必须显式引用该成员是在其可用的基接口。 例如，如果`I1`并`I2`都包含一个成员`M`，和`I3`继承`I1`并`I2`，类型实现`I3`将实现`I1.M`和`I2.M`。 如果接口隐藏多重继承的成员，将必须实现的类型实现的继承的成员和隐藏它们的成员。

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

Class Test
    Implements ILeftRight

    Sub LeftF() Implements ILeft.F
    End Sub

    Sub RightF() Implements IRight.F
    End Sub

    Sub LeftRightF() Implements ILeftRight.F
    End Sub
End Class
```

如果实现接口成员的包含接口为泛型，必须提供所实现的接口相同的类型参数。 例如：

```vb
Interface I1(Of T)
    Function F() As T
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)

    Function F1() As Integer Implements I1(Of Integer).F
    End Function

    Function F2() As Double Implements I1(Of Double).F
    End Function

    ' Error: I1(Of String) is not implemented by C1
    Function F3() As String Implements I1(Of String).F
    End Function
End Class

Class C2(Of U)
    Implements I1(Of U)

    Function F() As U Implements I1(Of U).F
    End Function
End Class
```


## <a name="methods"></a>方法

方法包含一个程序的可执行语句。

```antlr
MethodMemberDeclaration
    : MethodDeclaration
    | ExternalMethodDeclaration
    ;

InterfaceMethodMemberDeclaration
    : InterfaceMethodDeclaration
    ;

MethodDeclaration
    : SubDeclaration
    | MustOverrideSubDeclaration
    | FunctionDeclaration
    | MustOverrideFunctionDeclaration
    ;

InterfaceMethodDeclaration
    : InterfaceSubDeclaration
    | InterfaceFunctionDeclaration
    ;

SubSignature
    : 'Sub' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
    ;

FunctionSignature
    : 'Function' Identifier TypeParameterList?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

SubDeclaration
    : Attributes? ProcedureModifier* SubSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Sub' StatementTerminator
    ;

MustOverrideSubDeclaration
    : Attributes? MustOverrideProcedureModifier+ SubSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceSubDeclaration
    : Attributes? InterfaceProcedureModifier* SubSignature StatementTerminator
    ;

FunctionDeclaration
    : Attributes? ProcedureModifier* FunctionSignature
      HandlesOrImplements? LineTerminator
      Block
      'End' 'Function' StatementTerminator
    ;

MustOverrideFunctionDeclaration
    : Attributes? MustOverrideProcedureModifier+ FunctionSignature
      HandlesOrImplements? StatementTerminator
    ;

InterfaceFunctionDeclaration
    : Attributes? InterfaceProcedureModifier* FunctionSignature StatementTerminator
    ;

ProcedureModifier
    : AccessModifier | 'Shadows' | 'Shared' | 'Overridable' | 'NotOverridable' | 'Overrides'
    | 'Overloads' | 'Partial' | 'Iterator' | 'Async'
    ;

MustOverrideProcedureModifier
    : ProcedureModifier
    | 'MustOverride'
    ;

InterfaceProcedureModifier
    : 'Shadows' | 'Overloads'
    ;

HandlesOrImplements
    : HandlesClause
    | ImplementsClause
    ;
```

一系列可选参数和可选的返回值的方法共享或不共享。 通过类或类的实例访问的共享的方法。 类的实例可以通过访问非共享方法，也称为实例方法。 下面的示例演示一个类`Stack`具有多个共享的方法 (`Clone`并`Flip`)，和多个实例方法 (`Push`， `Pop`，并`ToString`):

```vb
Public Class Stack
    Public Shared Function Clone(s As Stack) As Stack
        ...
    End Function

    Public Shared Function Flip(s As Stack) As Stack
        ...
    End Function

    Public Function Pop() As Object
        ...
    End Function

    Public Sub Push(o As Object)
        ...
    End Sub 

    Public Overrides Function ToString() As String
        ...
    End Function 
End Class 

Module Test
    Sub Main()
        Dim s As Stack = New Stack()
        Dim i As Integer

        While i < 10
            s.Push(i)
        End While

        Dim flipped As Stack = Stack.Flip(s)
        Dim cloned As Stack = Stack.Clone(s)

        Console.WriteLine("Original stack: " & s.ToString())
        Console.WriteLine("Flipped stack: " & flipped.ToString())
        Console.WriteLine("Cloned stack: " & cloned.ToString())
    End Sub
End Module
```

可以重载方法，这意味着，只要它们具有唯一的签名，多个方法可能具有相同的名称。 方法签名组成的数量和其参数的类型。 方法签名特别不包括返回类型或如可选、 ByRef 或 ParamArray 参数修饰符。 下面的示例演示了大量的重载类：

```vb
Module Test
    Sub F()
        Console.WriteLine("F()")
    End Sub 

    Sub F(o As Object)
        Console.WriteLine("F(Object)")
    End Sub

    Sub F(value As Integer)
        Console.WriteLine("F(Integer)")
    End Sub 

    Sub F(a As Integer, b As Integer)
        Console.WriteLine("F(Integer, Integer)")
    End Sub 

    Sub F(values() As Integer)
        Console.WriteLine("F(Integer())")
    End Sub 

    Sub G(s As String, Optional s2 As String = 5)
        Console.WriteLine("G(String, Optional String")
    End Sub

    Sub G(s As String)
        Console.WriteLine("G(String)")
    End Sub


    Sub Main()
        F()
        F(1)
        F(CType(1, Object))
        F(1, 2)
        F(New Integer() { 1, 2, 3 })
        G("hello")
        G("hello", "world")
    End Sub
End Module
```

该程序的输出为：

```vb
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

重载的区别仅在于可选参数可用于库的"版本管理"。 例如，库的 v1 可能包括带可选参数的函数：

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

然后库的 v2 想要添加另一个可选参数"密码"，并想要执行此操作而不会破坏源兼容性 （因此，可以重新编译应用程序，用于指向 v1），并且不会断开二进制文件兼容性 （因此，使用到的应用程序参考 v1 现可引用无需重新编译 v2）。 这是 v2 的外观：

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

请注意，公共 API 中的可选参数不符合 CLS 規格。 但是，它们可供至少 Visual Basic 和 C# 4 和F#。



### <a name="regular-async-and-iterator-method-declarations"></a>Regular、 异步和迭代器方法声明

有两种类型的方法：*子例程*，这不会返回值，并*函数*，其执行操作。 正文并`End`可能仅省略构造的一种方法，如果方法在接口中定义或包含`MustOverride`修饰符。 如果在函数上指定任何返回类型和正在使用严格的语义，会发生编译时错误;否则该类型是隐式`Object`或类型的方法的类型字符。 返回类型和方法的参数类型的可访问域必须与相同或该方法本身的可访问域的超集。

一个__正则方法__是指既不具有`Async`也不`Iterator`修饰符。 它可能是一个子例程或函数。 部分[正则方法](statements.md#regular-methods)详细介绍了常规方法调用时，会发生什么情况。

__迭代器方法__是一个具有`Iterator`修饰符，但不`Async`修饰符。 它必须是一个函数，并且其返回类型必须是`IEnumerator`， `IEnumerable`，或`IEnumerator(Of T)`或`IEnumerable(Of T)`某些`T`，并且它必须无`ByRef`参数。 部分[迭代器方法](statements.md#iterator-methods)详细介绍了迭代器方法调用时，会发生什么情况。

__异步方法__是一个具有`Async`修饰符，但不`Iterator`修饰符。 它必须是一个子例程或函数返回类型为`Task`或`Task(Of T)`某些`T`，并且必须具有无`ByRef`参数。 部分[异步方法](statements.md#async-methods)详细介绍了在调用异步方法时，会发生什么情况。

如果方法不是这三种方法之一，它是一个编译时错误。

子例程和函数声明的特殊在于其开始和结束语句必须每个启动逻辑行的开头。 此外，正文非`MustOverride`子例程或函数声明必须以逻辑行的开头。 例如：

```vb
Module Test
    ' Illegal: Subroutine doesn't start the line
    Public x As Integer : Sub F() : End Sub

    ' Illegal: First statement doesn't start the line
    Sub G() : Console.WriteLine("G")
    End Sub

    ' Illegal: End Sub doesn't start the line
    Sub H() : End Sub
End Module
```


### <a name="external-method-declarations"></a>外部方法声明

外部方法声明引入了新程序的外部提供其实现的方法。

```antlr
ExternalMethodDeclaration
    : ExternalSubDeclaration
    | ExternalFunctionDeclaration
    ;

ExternalSubDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Sub'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )? StatementTerminator
    ;

ExternalFunctionDeclaration
    : Attributes? ExternalMethodModifier* 'Declare' CharsetModifier? 'Function'
      Identifier LibraryClause AliasClause?
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
      StatementTerminator
    ;

ExternalMethodModifier
    : AccessModifier
    | 'Shadows'
    | 'Overloads'
    ;

CharsetModifier
    : 'Ansi' | 'Unicode' | 'Auto'
    ;

LibraryClause
    : 'Lib' StringLiteral
    ;

AliasClause
    : 'Alias' StringLiteral
    ;
```

由于外部方法声明不提供任何实际的实现，它有没有方法体或`End`构造。 外部方法隐式共享，可能不具有类型参数和可能不处理事件或实现接口成员。 如果在函数上指定任何返回类型和正在使用严格的语义，发生编译时错误。 否则该类型是隐式`Object`或类型的方法的类型字符。 返回类型和参数类型的外部方法的可访问域必须与相同或外部方法本身的可访问域的超集。

外部方法声明的库子句指定实现方法的外部文件的名称。 可选别名子句是一个字符串，指定的数字序号 (加`#`字符) 或外部文件中的方法的名称。 单字符集修饰符还可以指定，它控制字符串封送到外部方法的调用期间使用的字符集。 `Unicode`修饰符将所有字符串转换为 Unicode 值封都送`Ansi`修饰符将封都送为 ANSI 值的所有字符串和`Auto`修饰符将根据.NET Framework 规则基于方法的名称的字符串封都都送或如果指定的别名名称。 如果不指定任何修饰符，则默认值是`Ansi`。

如果`Ansi`或`Unicode`指定，则无需修改了外部文件中查找方法名称。 如果`Auto`指定，则方法名称查找取决于平台。 如果该平台被视为 ANSI （例如，Windows 95、 Windows 98、 Windows ME），然后方法名称查找，进行任何修改。 如果在查找失败，`A`追加，然后重试查找。 如果平台被视为 Unicode (例如，Windows NT、 Windows 2000，Windows XP)，则`W`追加和查找名称。 如果在查找失败，则情况下再次尝试查找`W`。 例如：

```vb
Module Test
    ' All platforms bind to "ExternSub".
    Declare Ansi Sub ExternSub Lib "ExternDLL" ()

    ' All platforms bind to "ExternSub".
    Declare Unicode Sub ExternSub Lib "ExternDLL" ()

    ' ANSI platforms: bind to "ExternSub" then "ExternSubA".
    ' Unicode platforms: bind to "ExternSubW" then "ExternSub".
    Declare Auto Sub ExternSub Lib "ExternDLL" ()
End Module
```

根据.NET Framework 数据封送处理约定有一个例外情况下，传递给外部方法的数据类型进行封送。 按值传递的变量的字符串 (即， `ByVal x As String`) 封送到 OLE 自动化 BSTR 类型到 BSTR 外部方法中所做的更改会反映在字符串参数返回。 这是因为类型`String`在外部方法是可变的并且此特殊的封送模仿该行为。 通过引用传递的参数的字符串 (即`ByRef x As String`) 作为指向 OLE 自动化 BSTR 类型的指针进行封送。 可以通过指定重写这些特殊行为`System.Runtime.InteropServices.MarshalAsAttribute`参数上的属性。

此示例演示外部方法的使用：

```vb
Class Path
    Declare Function CreateDirectory Lib "kernel32" ( _
        Name As String, sa As SecurityAttributes) As Boolean
    Declare Function RemoveDirectory Lib "kernel32" ( _
        Name As String) As Boolean
    Declare Function GetCurrentDirectory Lib "kernel32" ( _
        BufSize As Integer, Buf As String) As Integer
    Declare Function SetCurrentDirectory Lib "kernel32" ( _
        Name As String) As Boolean
End Class
```


### <a name="overridable-methods"></a>可重写方法

`Overridable`修饰符指示一种方法是可重写。 `Overrides`修饰符指示一种方法重写基类型可重写方法具有相同的签名。 `NotOverridable`修饰符指示不能进一步重写可重写方法。 `MustOverride`修饰符指示必须在派生类中重写方法。

这些修饰符的某些组合不是有效的：

* `Overridable` 和`NotOverridable`是互斥的不能结合使用。

* `MustOverride` 暗指`Overridable`（并因此不能指定它），也不能与结合`NotOverridable`。

* `NotOverridable` 不能结合`Overridable`或`MustOverride`，也必须与结合`Overrides`。

* `Overrides` 暗指`Overridable`（并因此不能指定它），也不能与结合`MustOverride`。

也是可重写方法的其他限制：

* 一个`MustOverride`方法不能包含方法体或`End`构造，可能不会替代另一种方法，并可能只能出现在`MustInherit`类。

* 如果指定了一种方法`Overrides`，并且没有任何匹配的基方法以重写，则发生编译时错误。 重写方法不能指定`Shadows`。

* 一种方法可能会重写另一种方法重写方法的可访问性域是否不等于被重写方法的可访问域。 一个例外是，方法重写`Protected Friend`中没有的另一个程序集的方法`Friend`访问必须指定`Protected`(不`Protected Friend`)。

* `Private` 方法可能`Overridable`， `NotOverridable`，或`MustOverride`，它们可能会重写其他方法，也不。

* 中的方法`NotInheritable`类不能声明`Overridable`或`MustOverride`。

下面的示例说明了可重写和 nonoverridable 方法之间的差异：

```vb
Class Base
    Public Sub F()
        Console.WriteLine("Base.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("Base.G")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Shadows Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("Derived.G")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As Derived = New Derived()
        Dim b As Base = d

        b.F()
        d.F()
        b.G()
        d.G()
    End Sub
End Module
```

在示例中，类`Base`引入了一种方法`F`和一个`Overridable`方法`G`。 该类`Derived`引入了新方法`F`，从而隐藏继承`F`，并且还重写继承的方法`G`。 此示例产生以下输出：

```
Base.F
Derived.F
Derived.G
Derived.G
```

请注意，该语句`b.G()`调用`Derived.G`，而不`Base.G`。 这是因为运行时类型的实例 (即`Derived`) 而不是实例的编译时类型 (即`Base`) 确定要调用的实际方法实现。

### <a name="shared-methods"></a>共享的方法

`Shared`修饰符指示一种方法是*共享方法*。 共享的方法不对特定类型的实例进行操作，并可以直接从一种类型而不是通过一种类型的特定实例进行调用。 它是有效的但是，使用实例来限定共享的方法。 无效来指代`Me`， `MyClass`，或`MyBase`共享方法中。 共享的方法可能`Overridable`， `NotOverridable`，或`MustOverride`，并且它们可能不会替代方法。 标准模块和接口中定义的方法不能指定`Shared`，因为它们是隐式`Shared`已。

在结构或类，而声明的方法`Shared`修饰符*实例方法*。 一种类型的给定实例上运行的实例方法。 只能通过一种类型的实例调用的实例方法和通过实例可能引用`Me`表达式。

下面的示例阐释了用于访问共享和实例成员规则：

```vb
Class Test
    Private x As Integer
    Private Shared y As Integer

    Sub F()
        x = 1 ' Ok, same as Me.x = 1.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub G()
        x = 1 ' Error, cannot access Me.x.
        y = 1 ' Ok, same as Test.y = 1.
    End Sub

    Shared Sub Main()
        Dim t As Test = New Test()

        t.x = 1 ' Ok.
        t.y = 1 ' Ok.
        Test.x = 1 ' Error, cannot access instance member through type.
        Test.y = 1 ' Ok.
    End Sub
End Class
```

方法`F`显示，在实例函数成员，标识符可以用于访问实例成员和共享的成员。 方法`G`显示在共享的函数成员中，它是一个错误，以通过标识符访问实例成员。 方法`Main`显示在成员访问表达式中，必须通过情况下，访问实例成员，但可以通过类型或实例访问共享的成员。

### <a name="method-parameters"></a>方法参数

一个*参数*是一个变量，可用于传递执行和跳出执行方法的信息。 参数的一种方法被声明的方法的参数列表，其中包括一个或多个由逗号分隔的参数。

```antlr
ParameterList
    : Parameter ( Comma Parameter )*
    ;

Parameter
    : Attributes? ParameterModifier* ParameterIdentifier ( 'As' TypeName )?
      ( Equals ConstantExpression )?
    ;

ParameterModifier
    : 'ByVal' | 'ByRef' | 'Optional' | 'ParamArray'
    ;

ParameterIdentifier
    : Identifier IdentifierModifiers
    ;
```

如果未指定类型的参数和使用严格的语义，发生编译时错误。 否则默认类型是`Object`或类型的参数的类型字符。 如果一个参数包含在宽松的语义，即使`As`子句中，所有参数必须都指定类型。

参数指定为值、 引用中可选的或 paramarray 参数的修饰符`ByVal`， `ByRef`， `Optional`，和`ParamArray`分别。 未指定的参数`ByRef`或`ByVal`默认为`ByVal`。

参数名称的作用域为整个方法的正文，并且始终是可公开访问。 方法调用创建一个副本，特定于该调用的参数，并调用的参数列表的值或变量引用赋给新创建的参数。 由于外部方法声明和委托声明有没有正文，重复的参数名称是允许在参数列表中，但建议使用。

标识符可能跟的名称可以为 null 的修饰符`?`以指示它是可以为 null，并且还通过数组名称修饰符以指示它是一个数组。 它们可以结合使用，例如"`ByVal x?() As Integer`"。 不允许使用显式数组边界;此外，如果可以为 null 名称修饰符存在，则则`As`子句必须存在。


#### <a name="value-parameters"></a>值参数

一个*value 参数*使用显式声明`ByVal`修饰符。 如果`ByVal`使用修饰符，则`ByRef`修饰符不能指定。 Value 参数开始使用该参数属于，并且未被初始化的成员的调用替换调用中给定的参数的值的存在。 Value 参数将返回的成员时消失。

一种方法被允许将新值分配给 value 参数。 此类分配只会影响由值参数，则表示在本地存储位置它们对实际方法调用中给定的参数无效。

转换为方法，传递的参数的值和参数的修改不影响原始参数时，则使用值参数。 值参数是指其自己的变量，其中一个，它不同于相应的参数变量。 通过复制相应的参数值初始化此变量。 下面的示例演示一种方法`F`具有名为的值参数`p`:

```vb
Module Test
    Sub F(p As Integer)
        Console.WriteLine("p = " & p)
        p += 1
    End Sub 

    Sub Main()
        Dim a As Integer = 1

        Console.WriteLine("pre: a = " & a)
        F(a)
        Console.WriteLine("post: a = " & a)
    End Sub
End Module
```

该示例的生成以下输出，即使值参数`p`修改：

```
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a>引用参数

引用参数是用声明的参数`ByRef`修饰符。 如果`ByRef`指定修饰符，则`ByVal`修饰符不能使用。 引用参数不会创建新的存储位置。 相反，引用参数表示作为自变量在方法或构造函数调用中的变量。 从概念上讲，值的引用参数始终是相同的基础变量。

在两种模式，以作为引用参数采取行动*别名*或通过*副本中复制回。*

__别名。__ 参数作为调用方提供的参数的别名时，使用引用参数。 引用参数本身不定义一个变量，但而不是引用相应的参数变量。 修改引用参数直接并立即影响对应的参数。 下面的示例演示一种方法`Swap`具有两个引用参数：

```vb
Module Test
    Sub Swap(ByRef a As Integer, ByRef b As Integer)
        Dim t As Integer = a
        a = b
        b = t
    End Sub 

    Sub Main()
        Dim x As Integer = 1
        Dim y As Integer = 2

        Console.WriteLine("pre: x = " & x & ", y = " & y)
        Swap(x, y)
        Console.WriteLine("post: x = " & x & ", y = " & y)
    End Sub 
End Module
```

该程序的输出为：

```
pre: x = 1, y = 2
post: x = 2, y = 1
```

方法调用`Swap`类中`Main`，`a`表示`x,`并`b`表示`y`。 因此，调用具有交换的值的效果`x`和`y`。

在方法中采用引用参数，就可以为多个名称来表示相同的存储位置：

```vb
Module Test
    Private s As String

    Sub F(ByRef a As String, ByRef b As String)
        s = "One"
        a = "Two"
        b = "Three"
    End Sub

    Sub G()
        F(s, s)
    End Sub
End Module
```

在示例中的方法调用`F`中`G`将传递到引用`s`两个`a`和`b`。 因此，对于该调用，名称`s`， `a`，并`b`所有引用相同的存储位置，并且所有三个赋值修改实例变量`s`。

__复制在反向复制。__ 如果传递给引用参数的变量的类型不是与引用参数的类型兼容，或非变量 （如属性） 作为参数传递到引用参数，或调用为后期绑定的则临时分配的变量，并将其传递给引用参数。 之前该方法调用，并将复制回原始变量 （如果有一个以及是否可写） 方法返回时，正在传递的值将复制到此临时变量。 因此，引用参数一定不能包含对传入的变量的具体的存储的引用，该方法退出之前对引用参数的任何更改可能不在变量中会反映。 例如：

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(ByRef b As Base)
        b = New Base()
    End Sub

    Property G() As Base
        Get
        End Get
        Set
        End Set
    End Property

    Sub Main()
        Dim d As Derived

        F(G)   ' OK.
        F(d)   ' Throws System.InvalidCastException after F returns.
    End Sub
End Module
```

在第一个调用的情况下`F`，创建一个临时变量和属性的值`G`可为其分配并传递给`F`。 从返回时`F`，在临时变量中的值分配到的属性`G`。 在第二种情况下，创建另一个临时变量和值`d`可为其分配并传递给`F`。 从返回时`F`，在临时变量中的值转换回该变量的类型`Derived`，并分配给`d`。 自值后所传递回不能强制转换为`Derived`，在运行时引发异常。

#### <a name="optional-parameters"></a>可选参数

使用声明一个可选参数`Optional`修饰符。 后面的形参列表中的可选参数的参数也必须是可选的;如果未能指定`Optional`上的以下参数修饰符会触发编译时错误。 类型为 null 的类型的一些可选参数`T?`或不可以为 null 的类型`T`必须指定一个常量表达式`e`要用作默认值，如果未指定参数。 如果`e`计算结果为`Nothing`对象，则默认值的类型*参数类型*将用作该参数默认值。 否则为`CType(e, T)`必须是常量表达式和结果作为参数的默认值。

可选参数都是唯一一种参数上的初始值设定项无效。 初始化始终调用表达式，不在方法主体本身的一部分完成。

```vb
Module Test
    Sub F(x As Integer, Optional y As Integer = 20)
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub

    Sub Main()
        F(10)
        F(30,40)
    End Sub
End Module
```

该程序的输出为：

```
x = 10, y = 20
x = 30, y = 40
```

在委托或事件声明中，也不在 lambda 表达式中不能指定可选参数。

#### <a name="paramarray-parameters"></a>ParamArray 参数

`ParamArray` 参数以声明`ParamArray`修饰符。 如果`ParamArray`修饰符，则`ByVal`必须在指定的修饰符，并可能会使用任何其他参数`ParamArray`修饰符。 `ParamArray`参数的类型必须是一维数组，并且它必须是参数列表中的最后一个参数。

一个`ParamArray`参数表示的不确定的类型的参数数目`ParamArray`。 在该方法本身，`ParamArray`参数视为其声明的类型，并且没有任何特殊语义。 一个`ParamArray`参数是隐式可选，默认值的类型的空一维数组的`ParamArray`。

一个`ParamArray`允许在方法调用中的两种方式之一中指定的参数：

* 对于给定的实参`ParamArray`可以是单个表达式的类型扩大到`ParamArray`类型。 在这种情况下，`ParamArray`作用与值参数的完全一样。

* 或者，此调用可以指定零个或多个参数`ParamArray`，其中每个自变量是隐式转换为的元素类型的类型的表达式`ParamArray`。 在这种情况下，调用创建的实例`ParamArray`使用长度为对应的参数、 初始化该数组的元素实例与给定的参数值，并使用新创建的数组实例作为实际数类型自变量。

除了允许数目可变的参数在调用`ParamArray`是恰好等同于值参数的相同的类型，如以下示例所示。

```vb
Module Test
    Sub F(ParamArray args() As Integer)
        Dim i As Integer

        Console.Write("Array contains " & args.Length & " elements:")
        For Each i In args
            Console.Write(" " & i)
        Next i
        Console.WriteLine()
    End Sub

    Sub Main()
        Dim a As Integer() = { 1, 2, 3 }

        F(a)
        F(10, 20, 30, 40)
        F()
    End Sub
End Module
```

该示例生成输出

```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

第一个调用`F`只需将该数组传递`a`作为值参数。 第二个调用`F`自动创建具有给定的元素值的四个元素数组并将该数组实例作为值参数传递。 同样，第三个调用的`F`创建一个零元素的数组，并将该实例作为值参数传递。 第二个和第三个调用都完全等效于编写：

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

`ParamArray` 不能在委托或事件声明中指定的参数。

### <a name="event-handling"></a>事件处理

方法可以以声明方式处理事件引发的对象实例或共享的变量中。 若要处理的事件，方法声明指定`Handles`关键字，并列出一个或多个事件。

```antlr
HandlesClause
    : ( 'Handles' EventHandlesList )?
    ;

EventHandlesList
    : EventMemberSpecifier ( Comma EventMemberSpecifier )*
    ;

EventMemberSpecifier
    : Identifier Period IdentifierOrKeyword
    | 'MyBase' Period IdentifierOrKeyword
    | 'MyClass' Period IdentifierOrKeyword
    | 'Me' Period IdentifierOrKeyword
    ;
```

中的事件`Handles`由句点分隔的两个标识符指定列表：

* 第一个标识符必须是实例或共享中包含指定的类型的变量`WithEvents`修饰符或`MyBase`或`MyClass`或`Me`关键字; 否则，会发生编译时错误。 此变量包含的对象，将引发此方法处理的事件。

* 第二个标识符必须指定的第一个标识符类型的成员。 该成员必须是一个事件，并可能会共享。 如果共享的变量指定为第一个标识符，然后必须共享该事件，或则会导致错误。

处理程序方法`M`被视为一个事件的有效的事件处理程序`E`如果语句`AddHandler E, AddressOf M`也会有效。 与不同`AddHandler`语句，但是，显式事件处理程序允许处理具有不带任何参数而不考虑是否严格的语义正在使用或不方法的事件：

```vb
Option Strict On

Class C1
    Event E(x As Integer)
End Class

Class C2
    withEvents C1 As New C1()

    ' Valid
    Sub M1() Handles C1.E
    End Sub

    Sub M2()
        ' Invalid
        AddHandler C1.E, AddressOf M1
    End Sub
End Class
```

单个成员可以处理多个匹配的事件，并且多个方法可处理单个事件。 方法的可访问性对其能够处理事件无效。 下面的示例显示了一种方法可以如何处理事件：

```vb
Class Raiser
    Event E1()

    Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    WithEvents x As Raiser

    Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
        x.Raise()
        x.Raise()
    End Sub
End Module
```

这将打印出：

```
Raised
Raised
```

一个类型继承其基类型由提供的所有事件处理程序。 派生的类型以任何方式不能更改它继承自其基类型，但可能会将附加处理程序添加到该事件的事件映射。


### <a name="extension-methods"></a>扩展方法

方法可以添加到从外部使用类型声明的类型*扩展方法*。 扩展方法是使用方法`System.Runtime.CompilerServices.ExtensionAttribute`特性应用于它们。 它们只能在标准模块中声明，并且必须具有至少一个参数，指定该方法的类型扩展。 例如，以下扩展方法扩展了类型`String`:

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

__请注意。__ 尽管 Visual Basic 要求标准模块中声明的扩展方法，但 C# 等其他语言可能会给他们在其他类型的类型中声明。 只要方法遵循此处所述的其他约定，并包含类型不是开放式泛型类型，不能被实例化，Visual Basic 可以识别的扩展方法。

调用扩展方法时，对其调用它的实例传递到第一个参数。 不能声明为第一个参数`Optional`或`ParamArray`。 任何类型，包括一个类型参数，可以显示为扩展方法的第一个参数。 例如，以下方法来扩展类型`Integer()`，实现的任何类型`System.Collections.Generic.IEnumerable(Of T)`，并在所有的任何类型：

```vb
Imports System.Runtime.CompilerServices

Module Extensions
    <Extension> _
    Sub PrintArray(a() As Integer)
        ...
    End Sub

    <Extension> _
    Sub PrintList(Of T)(a As IEnumerable(Of T))
        ...
    End Sub

    <Extension> _
    Sub Print(Of T)(a As T)
        ...
    End Sub
End Module
```

如前面的示例所示，可以扩展接口。 接口的扩展方法提供方法的实现，因此实现具有仍然只在其上定义的扩展方法的接口的类型可实现由接口最初声明的成员。 例如：

```vb
Imports System.Runtime.CompilerServices

Interface IAction
  Sub DoAction()
End Interface

Module IActionExtensions 
    <Extension> _
    Public Sub DoAnotherAction(i As IAction) 
        i.DoAction()
    End Sub
End Module

Class C
  Implements IAction

  Sub DoAction() Implements IAction.DoAction
    ...
  End Sub

  ' ERROR: Cannot implement extension method IAction.DoAnotherAction
  Sub DoAnotherAction() Implements IAction.DoAnotherAction
    ...
  End Sub
End Class
```

扩展方法也具有其类型参数的类型约束，并只需为使用非扩展泛型方法类型参数可以推断出：

```vb
Imports System.Runtime.CompilerServices

Module IEnumerableComparableExtensions
    <Extension> _
    Public Function Sort(Of T As IComparable(Of T))(i As IEnumerable(Of T)) _
        As IEnumerable(Of T)
        ...
    End Function
End Module
```

此外可以通过在要扩展的类型的隐式实例表达式访问扩展方法：

```vb
Imports System.Runtime.CompilerServices

Class C1
    Sub M1()
        Me.M2()
        M2()
    End Sub
End Class

Module C1Extensions
    <Extension>
    Sub M2(c As C1)
        ...
    End Sub
End Module
```

用于可访问性的目的，扩展方法也被视为标准模块中声明-他们无权额外访问到它们所扩展超出凭借其声明上下文的访问类型的成员的成员。

标准模块方法在作用域中时，扩展方法才可用。 否则，原始类型将不显示已扩展。 例如：

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Namespace N1
    Module C1Extensions
        <Extension> _
        Sub M1(c As C1)
            ...
        End Sub
    End Module
End Namespace

Module Test
    Sub Main()
        Dim c As New C1()

        ' Error: c has no member named "M1"
        c.M1()
    End Sub
End Module
```

当仅类型上的扩展方法可用时引用类型仍将生成编译时错误。

务必要注意的扩展方法被视为是所有上下文中的类型的成员成员绑定到的位置，如强类型`For Each`模式。 例如：

```vb
Imports System.Runtime.CompilerServices

Class C1
End Class

Class C1Enumerator
    ReadOnly Property Current() As C1
        Get
            ...
        End Get
    End Property

    Function MoveNext() As Boolean
        ...
    End Function
End Class

Module C1Extensions
    <Extension> _
    Function GetEnumerator(c As C1) As C1Enumerator
        ...
    End Function
End Module

Module Test
    Sub Main()
        Dim c As New C1()

        ' Valid
        For Each o As Object In c
            ...
        Next o
    End Sub
End Module
```

委托也可以创建引用的扩展方法。 因此，代码：

```vb
Delegate Sub D1()

Module Test
    Sub Main()
        Dim s As String = "Hello, World!"
        Dim d As D1

        d = AddressOf s.Print
        d()
    End Sub
End Module
```

是大致相当于：

```vb
Delegate Sub D1()

Module Test
    Sub Main()
      Dim s As String = "Hello, World!"
      Dim d As D1

      d = CType([Delegate].CreateDelegate(GetType(D1), s, _
                GetType(StringExtensions).GetMethod("Print")), D1)
      d()
    End Sub
End Module
```

__请注意。__ 通常情况下 Visual Basic 会插入会导致一个实例方法调用的检查`System.NullReferenceException`实例调用方法时是否发生`Nothing`。 在扩展方法的情况下没有有效方法来插入此检查，因此需要显式检查扩展方法`Nothing`。 

__请注意。__ 将装箱值类型，当作为传递`ByVal`到参数的参数类型化为接口。  这意味着副作用的扩展方法将对而不是原始结构的副本。 虽然语言将在扩展方法的第一个参数上不受限制，但建议的扩展方法不使用扩展值类型或在扩展时的值类型，第一个参数传递`ByRef`以确保该侧对原始值执行操作效果。

### <a name="partial-methods"></a>分部方法

一个*分部方法*是一种方法，指定一个签名，但没有方法正文。 方法的主体可以提供另一个方法声明具有相同的名称和签名，很可能出在另一个分部声明的类型。 例如：

a.vb:

```vb
' Designer generated code
Public Partial Class MyForm
    Private Partial Sub ValidateControls()
    End Sub

    Public Sub New()
        ' Initialize controls
        ...

        ValidateControls()
    End Sub    
End Class
```

b.vb:

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

在此示例中，类的分部声明`MyForm`声明的分部方法`ValidateControls`没有实现。 分部声明中的构造函数调用的分部方法，即使没有正文文件中提供。 其他分部声明`MyForm`然后提供该方法的实现。

分部方法可以调用而不考虑是否提供了一个主体;如果提供没有方法正文，则将忽略此调用。 例如：

```vb
Public Class C1
    Private Partial Sub M1()
    End Sub

    Public Sub New()
        ' Since no implementation is supplied, this call will not be made.
        M1()
    End Sub
End Class
```

此外忽略并不会评估中作为参数传递到分部方法调用，将忽略任何表达式。 (__注意。__ 也就是说，分部方法是提供跨两个分部类型定义，因为分部方法具有无需付费，如果不使用它们的行为的极其有效的方式。）

分部方法声明必须声明为`Private`和必须始终为不包含任何语句在其主体中的子例程。 分部方法不能自行实现接口方法，虽然可以提供其主体的方法。

只有一种方法可以提供分部方法的正文。 方法提供正文转换为分部方法必须具有相同的签名作为分部方法、 任何类型参数、 相同的声明修饰符，以及相同的参数和类型参数名称相同的约束。 将合并分部方法，并提供其正文的方法的属性，因为这些方法的参数的任何属性。 同样，合并的方法处理的事件列表。 例如：

```vb
Class C1
    Event E1()
    Event E2()

    Private Partial Sub S() Handles Me.E1
    End Sub

    ' Handles both E1 and E2
    Private Sub S() Handles Me.E2
        ...
    End Sub
End Class
```

## <a name="constructors"></a>构造函数

*构造函数*是允许控制初始化的特殊方法。 运行程序开始运行后，或创建类型的实例时。 与其他成员不同构造函数不会继承和不到类型的声明空间引入一个名称。 通过对象创建表达式或.NET Framework 中; 只被调用构造函数可能永远不会直接调用它们。

__请注意。__ 构造函数具有同样的限制在子例程具有的行位置。 开始语句，end 语句和块必须出现在逻辑行的开头开始。

```antlr
ConstructorMemberDeclaration
    : Attributes? ConstructorModifier* 'Sub' 'New'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Sub' StatementTerminator
    ;

ConstructorModifier
    : AccessModifier
    | 'Shared'
    ;
```

### <a name="instance-constructors"></a>实例构造函数

*实例构造函数*初始化类型的实例并创建实例时运行的.NET Framework。 构造函数的参数列表受到一种方法的参数列表相同的规则。 可能会重载实例构造函数。

引用类型中的所有构造函数必须调用另一个构造函数。 如果显式调用，它必须是在构造函数方法主体中的第一个语句。 该语句可以调用另一个类型的实例构造函数-例如，`Me.New(...)`或`MyClass.New(...)`-或如果它不是一种结构它可以调用类型的基类型的实例构造函数-例如， `MyBase.New(...)`。 它是无效的构造函数调用自身。 如果一个构造函数忽略另一个构造函数的调用`MyBase.New()`是隐式的。 如果没有无参数的基类型的构造函数，发生编译时错误。 因为`Me`将不被视为之前对基类构造函数调用后, 向构造函数调用语句参数不能引用构造`Me`， `MyClass`，或`MyBase`隐式或显式。

构造函数的第一条语句时窗体的`MyBase.New(...)`，构造函数隐式执行指定的变量的初始值设定项的类型中声明的实例变量的初始化。 这对应于一系列调用的直接基类型构造函数后立即执行的分配。 这种顺序可确保有权访问该实例的任何语句执行之前，所有基本实例变量进行初始化其变量初始值设定项。 例如：

```vb
Class A
    Protected x As Integer = 1
End Class

Class B
    Inherits A

    Private y As Integer = x

    Public Sub New()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Class
```

当`New B()`用于创建实例`B`，将生成以下输出：

```
x = 1, y = 1
```

值`y`是`1`因为调用基类构造函数后，将执行变量初始值设定项。 变量初始值设定项与它们在类型声明中显示文本的顺序执行。

一种类型只声明`Private`构造函数，它不能在一般情况下对于其他类型派生的类型或创建该类型的实例; 唯一的例外是嵌套类型中的类型。 `Private` 构造函数通常用在仅包含类型`Shared`成员。

如果类型不包含任何实例构造函数声明，会自动提供默认构造函数。 默认构造函数只是调用的直接基类型的无参数构造函数。 如果直接基类型不具有可访问的无参数构造函数，发生编译时错误。 默认构造函数的声明的访问类型是`Public`除非该类型是`MustInherit`，在这种情况下的默认构造函数是`Protected`。

__请注意。__ 默认访问权限`MustInherit`类型的默认构造函数是`Protected`因为`MustInherit`不能直接创建类。 因此将默认构造函数没有必要`Public`。

在下面的示例提供一个默认构造函数，因为此类不包含任何构造函数声明：

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

因此，此示例是完全等效于以下：

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

将发送到设计器的默认构造函数生成的类中使用特性标记`Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute`将调用的方法`Sub InitializeComponent()`，如果它存在于基构造函数的调用之后。 (__注意。__ 这允许设计器生成的文件，例如那些通过 WinForms 设计器，以忽略此参数的构造函数中的设计器文件。 这使程序员能够指定其自身，选择性地。）

### <a name="shared-constructors"></a>共享的构造函数

*共享的构造函数*初始化类型共享变量; 它们运行该程序开始执行之后,、 但在对类型的成员的任何引用。 共享的构造函数指定`Shared`修饰符，除非它在这种情况下处于标准模块`Shared`修饰符为隐式。

与实例构造函数，不同共享构造函数具有隐式的公共访问，没有任何参数，和可以不调用其他构造函数。 之前共享的构造函数中的第一个语句，共享的构造函数隐式执行指定的共享变量类型中声明的变量的初始值设定项初始化。 这对应于分配给构造函数在输入时立即执行的一系列。 变量初始值设定项与它们在类型声明中显示文本的顺序执行。

下面的示例演示`Employee`使用共享的构造函数初始化共享的变量的类：

```vb
Imports System.Data

Class Employee
    Private Shared ds As DataSet

    Shared Sub New()
        ds = New DataSet()
    End Sub

    Public Name As String
    Public Salary As Decimal
End Class
```

对于每个关闭的泛型类型存在单独的共享构造函数。 因为共享的构造函数执行一次性的每个封闭类型，它是很方便地强制执行不能在编译时通过约束检查的类型参数的运行时检查。 例如，以下类型使用共享的构造函数强制执行，类型参数是`Integer`或`Double`:

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

完全共享的构造函数运行时是最依赖，实现的即使如果显式定义共享的构造函数提供了一些保证：

* 共享的构造函数之前在首次访问运行到类型的任何静态字段。

* 共享的构造函数的类型的任何静态方法的第一个调用之前运行。

* 共享的构造任何的函数类型的构造函数的第一个调用之前运行。

在共享的构造函数隐式创建的共享初始值设定项的情况下不适用的上述保证。 下面的示例的输出是不确定的因为加载的确切顺序并因此共享的构造函数的执行未定义：

```vb
Module Test
    Sub Main()
        A.F()
        B.F()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

Class B
    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub F()
        Console.WriteLine("B.F")
    End Sub
End Class
```

输出可能是以下之一：

```
Init A
A.F
Init B
B.F
```

或

```
Init B
Init A
A.F
B.F
```

与此相反，下面的示例生成可预测的输出。 请注意，`Shared`类构造函数`A`永远不会执行，即使类`B`从其派生：

```vb
Module Test
    Sub Main()
        B.G()
    End Sub
End Module

Class A
    Shared Sub New()
        Console.WriteLine("Init A")
    End Sub
End Class

Class B
    Inherits A

    Shared Sub New()
        Console.WriteLine("Init B")
    End Sub

    Public Shared Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class
```

输出为：

```
Init B
B.G
```

还有可能构造循环依赖项，以便`Shared`具有变量初始值设定项在其默认观察到的变量值状态，如以下示例所示：

```vb
Class A
    Public Shared X As Integer = B.Y + 1
End Class

Class B
    Public Shared Y As Integer = A.X + 1

    Shared Sub Main()
        Console.WriteLine("X = " & A.X & ", Y = " & B.Y)
    End Sub
End Class
```

这将生成输出：

```
X = 1, Y = 2
```

若要执行`Main`方法，系统首先加载类`B`。 `Shared`类的构造函数`B`计算的初始值将继续`Y`，哪些以递归方式导致类`A`若要加载，因为值的`A.X`引用。 `Shared`类的构造函数`A`又继续计算的初始值`X`，并在此过程中，提取*默认*的值`Y`，这是零。 `A.X` 因此初始化为`1`。 加载过程`A`然后结束，返回到初始值的计算`Y`，其结果成为`2`。

有`Main`方法改为已经找到在类中`A`，该示例将会生成以下输出：

```
X = 2, Y = 1
```

避免循环引用中的`Shared`变量初始值设定项由于它是通常无法确定哪些类中包含此类引用的加载的顺序。

## <a name="events"></a>事件

事件用于通知的特定匹配项的代码。 事件声明包含的标识符，一个委托类型或参数列表中，和一个可选`Implements`子句。

```antlr
EventMemberDeclaration
    : RegularEventMemberDeclaration
    | CustomEventMemberDeclaration
    ;

RegularEventMemberDeclaration
    : Attributes? EventModifiers* 'Event'
      Identifier ParametersOrType ImplementsClause? StatementTerminator
    ;

InterfaceEventMemberDeclaration
    : Attributes? InterfaceEventModifiers* 'Event'
      Identifier ParametersOrType StatementTerminator
    ;

ParametersOrType
    : ( OpenParenthesis ParameterList? CloseParenthesis )?
    | 'As' NonArrayTypeName
    ;

EventModifiers
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    ;

InterfaceEventModifiers
    : 'Shadows'
    ;
```

如果指定的委托类型，则委托类型可能没有返回类型。 如果指定的参数列表，则可能不包含`Optional`或`ParamArray`参数。 参数类型和/或委托类型的可访问域必须相同，或超集，事件本身的可访问域。 可以通过指定共享事件`Shared`修饰符。

除了成员名称添加到该类型的声明空间，事件声明隐式声明多个其他成员。 给定事件名为`X`，以下成员添加到声明空间：

* 如果声明的形式，方法声明嵌套的委托类名为`XEventHandler`引入的。 嵌套的委托类匹配的方法声明，并且包含与事件相同的可访问性。 在参数列表中的特性应用于委托类的参数。

* 一个`Private`实例变量类型为委托，名为`XEvent`。

* 两个方法名为`add_X`和`remove_X`这不能调用、 重写或重载。

如果类型尝试声明与上述名称之一匹配的名称，将导致编译时错误，并隐式`add_X`和`remove_X`声明将名称绑定用于忽略。 不能重写或重载任何引入的成员，但也可以在派生类型中隐藏它们。 例如，在类声明

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

等效于下面的声明

```vb
Class Raiser
    Public Delegate Sub ConstructedEventHandler(i As Integer)

    Protected ConstructedEvent As ConstructedEventHandler

    Public Sub add_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Combine(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub

    Public Sub remove_Constructed(d As ConstructedEventHandler)
        ConstructedEvent = _
            CType( _
                [Delegate].Remove(ConstructedEvent, d), _
                    Raiser.ConstructedEventHandler)
    End Sub
End Class
```

无需指定委托类型声明一个事件是最简单且最紧凑的语法，但缺点是声明新的委托类型的每个事件。 例如，在以下示例中，三个隐藏的委托类型创建，即使所有三个事件都具有相同的参数列表：

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

在以下示例中，事件只需使用相同的委托， `EventHandler`:

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

可以在以下两种方式处理事件： 静态或动态。 以静态方式处理事件是更简单，只需要`WithEvents`变量和一个`Handles`子句。 在以下示例中，类`Form1`以静态方式处理事件`Click`对象的`Button`:

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

动态处理事件是更复杂，因为必须显式连接和断开连接到代码中的事件。 该语句`AddHandler`添加一个事件和语句的处理程序`RemoveHandler`移除事件处理程序。 下一个示例显示了一个类`Form1`，它将`Button1_Click`的事件处理程序作为`Button1`的`Click`事件：

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

在方法中`Disconnect`，移除事件处理程序。


### <a name="custom-events"></a>自定义事件

如前一部分中所述，事件声明隐式定义的字段中，`add_`方法，和一个`remove_`方法，用于跟踪的事件处理程序。 在某些情况下，但是，它可能需要提供自定义代码的跟踪事件处理程序。 例如，如果某个类定义四十事件，其中仅有少数曾经将处理过程，而不四十字段使用哈希表来跟踪每个事件处理程序可能更高效。 *自定义事件*允许`add_X`和`remove_X`方法来显式定义这样的事件处理程序的自定义存储。

自定义事件声明指定委托类型的事件被声明，使用异常的方式相同的关键字`Custom`必须位于之前`Event`关键字。 自定义事件声明中包含的三个声明：`AddHandler`声明中，`RemoveHandler`声明和一个`RaiseEvent`声明。 尽管它们可能具有属性，都不声明可以包含任何修饰符。

```antlr
CustomEventMemberDeclaration
    : Attributes? EventModifiers* 'Custom' 'Event'
      Identifier 'As' TypeName ImplementsClause? StatementTerminator
      EventAccessorDeclaration+
      'End' 'Event' StatementTerminator
    ;

EventAccessorDeclaration
    : AddHandlerDeclaration
    | RemoveHandlerDeclaration
    | RaiseEventDeclaration
    ;

AddHandlerDeclaration
    : Attributes? 'AddHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'AddHandler' StatementTerminator
    ;

RemoveHandlerDeclaration
    : Attributes? 'RemoveHandler'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RemoveHandler' StatementTerminator
    ;

RaiseEventDeclaration
    : Attributes? 'RaiseEvent'
      OpenParenthesis ParameterList CloseParenthesis LineTerminator
      Block?
      'End' 'RaiseEvent' StatementTerminator
    ;
```

例如：

```vb
Class Test
    Private Handlers As EventHandler

    Public Custom Event TestEvent As EventHandler
        AddHandler(value As EventHandler)
            Handlers = CType([Delegate].Combine(Handlers, value), _
                EventHandler)
        End AddHandler

        RemoveHandler(value as EventHandler)
            Handlers = CType([Delegate].Remove(Handlers, value), _
                EventHandler)
        End RemoveHandler

        RaiseEvent(sender As Object, e As EventArgs)
            Dim TempHandlers As EventHandler = Handlers

            If TempHandlers IsNot Nothing Then
                TempHandlers(sender, e)
            End If
        End RaiseEvent
    End Event
End Class
```

`AddHandler`并`RemoveHandler`声明接受一个`ByVal`参数，它必须为该事件的委托类型。 当`AddHandler`或`RemoveHandler`执行语句 (或`Handles`子句自动处理的事件)，将调用相应的声明。 `RaiseEvent`声明采用相同的事件委托的参数和时将调用`RaiseEvent`执行语句。 所有声明必须提供并被视为子例程。

请注意， `AddHandler`，`RemoveHandler`和`RaiseEvent`声明子例程具有的行位置上具有同样的限制。 开始语句，end 语句和块必须出现在逻辑行的开头开始。

除了成员名称添加到该类型的声明空间，自定义事件声明隐式声明多个其他成员。 给定事件名为`X`，以下成员添加到声明空间：

* 一个名为方法`add_X`，对应于`AddHandler`声明。

* 一个名为方法`remove_X`，对应于`RemoveHandler`声明。

* 一个名为方法`fire_X`，对应于`RaiseEvent`声明。

如果类型尝试声明与上述名称之一匹配的名称，将导致编译时错误，并隐式声明将名称绑定用于忽略。 不能重写或重载任何引入的成员，但也可以在派生类型中隐藏它们。

__请注意。__ `Custom` 不是保留的字。

#### <a name="custom-events-in-winrt-assemblies"></a>WinRT 程序集中的自定义事件

使用编译的文件中声明的事件从 Microsoft Visual Basic 11.0 开始`/target:winmdobj`，或在此类文件中的接口中声明和其他位置，则实现，被视为略有不同。

* 外部工具用来生成 winmd 通常将允许仅特定委托类型如`System.EventHandler(Of T)`或`System.TypedEventHandle(Of T, U)`，，将不允许其他人。

* `XEvent`字段都具有类型`System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)`其中`T`是委托类型。

* AddHandler 访问器返回`System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`，和 RemoveHandler 取值函数采用单个参数的类型相同。

下面是此类自定义事件的示例。

```vb
Imports System.Runtime.InteropServices.WindowsRuntime

Public NotInheritable Class ClassInWinMD
    Private XEvent As EventRegistrationTokenTable(Of EventHandler(Of Integer))

    Public Custom Event X As EventHandler(Of Integer)
        AddHandler(handler As EventHandler(Of Integer))
            Return EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                   GetOrCreateEventRegistrationTokenTable(XEvent).
                   AddEventHandler(handler)
        End AddHandler

        RemoveHandler(token As EventRegistrationToken)
            EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                RemoveEventHandler(token)
        End RemoveHandler

        RaiseEvent(sender As Object, i As Integer)
            Dim table = EventRegistrationTokenTable(Of EventHandler(Of Integer)).
                GetOrCreateEventRegistrationTokenTable(XEvent).
                InvocationList
            If table IsNot Nothing Then table(sender, i)
        End RaiseEvent
    End Event
End Class
```


## <a name="constants"></a>常量

一个*常量*是一种类型的成员的常量值。

```antlr
ConstantMemberDeclaration
    : Attributes? ConstantModifier* 'Const' ConstantDeclarators StatementTerminator
    ;

ConstantModifier
    : AccessModifier
    | 'Shadows'
    ;

ConstantDeclarators
    : ConstantDeclarator ( Comma ConstantDeclarator )*
    ;

ConstantDeclarator
    : Identifier ( 'As' TypeName )? Equals ConstantExpression StatementTerminator
    ;
```

隐式共享常量。 如果声明包含`As`子句，子句可以指定该声明引入的成员的类型。 如果省略的类型，该常量的类型推断。 一个常量的类型可能只能是基元类型或`Object`。 如果一个常量的类型为`Object`又没有类型字符，该常量的实际类型将常量表达式的类型。 否则，该常量的类型是字符常量的类型的类型。

下面的示例演示一个名为类`Constants`具有两个公共常量：

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

可以通过类，如下面的示例，打印出的值中所示访问常量`Constants.A`和`Constants.B`。

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

声明多个常数在常量声明是等效于单个常量的多个声明。 下面的示例声明一个声明语句中的三个常量。

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

此声明是等效于以下：

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

常量的类型的可访问性域必须与相同或常量本身的可访问域的超集。 常量表达式必须产生一个值或隐式转换为常量的类型的类型的常量的类型。 常量表达式可能不是循环;也就是说，就本身而言可能没有定义一个常量。

编译器将自动计算的常量声明中的相应顺序。 在以下示例中，编译器将首先计算`Y`，然后`Z`，最后`X`，分别生成值 10、 11 和 12。

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

需要常量值的符号名称，但在常量声明或当该值不能在编译时由计算常量表达式中不允许的值类型，可能会改为使用只读变量。


## <a name="instance-and-shared-variables"></a>实例和共享的变量

实例或共享的变量是可以存储的信息类型的成员。

```antlr
VariableMemberDeclaration
    : Attributes? VariableModifier+ VariableDeclarators StatementTerminator
    ;

VariableModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'ReadOnly'
    | 'WithEvents'
    | 'Dim'
    ;

VariableDeclarators
    : VariableDeclarator ( Comma VariableDeclarator )*
    ;

VariableDeclarator
    : VariableIdentifiers 'As' ObjectCreationExpression
    | VariableIdentifiers ( 'As' TypeName )? ( Equals Expression )?
    ;

VariableIdentifiers
    : VariableIdentifier ( Comma VariableIdentifier )*
    ;

VariableIdentifier
    : Identifier IdentifierModifiers
    ;
```

`Dim`修饰符必须指定是否没有修饰符指定了，但可能否则省略。 单个变量声明可能包括多个变量的声明符;每个变量的声明符引入一个新实例或共享的成员。

如果指定一个初始值设定项，则变量的声明符可能声明仅一个实例或共享的变量：

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

此限制不适用于对象初始值设定项：

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

与声明的变量`Shared`修饰符*共享的变量*。 共享的变量标识恰好一个存储位置，而不考虑创建的类型的实例数。 共享的变量进入存在，当程序开始执行，并将该程序终止时消失。

共享的变量仅在特定的封闭式泛型类型的实例之间共享。 例如，该计划：

```vb
Class C(Of V) 
    Shared InstanceCount As Integer = 0

    Public Sub New()  
        InstanceCount += 1 
    End Sub

    Public Shared ReadOnly Property Count() As Integer 
        Get
            Return InstanceCount
        End Get
    End Property
End Class

Class Application 
    Shared Sub Main() 
        Dim x1 As New C(Of Integer)()
        Console.WriteLine(C(Of Integer).Count)

        Dim x2 As New C(Of Double)() 
        Console.WriteLine(C(Of Integer).Count)

        Dim x3 As New C(Of Integer)() 
        Console.WriteLine(C(Of Integer).Count)
    End Sub
End Class
```

打印出：

```
1
1
2
```

未声明的变量`Shared`调用修饰符*实例变量*。 类的每个实例包含一个单独的类的所有实例变量副本。 引用类型的一个实例变量开始时的新实例的存在类型创建，并且将不再存在时对该实例的引用和`Finalize`方法是否已执行。 值类型的实例变量具有其所属的变量的生存期完全相同。 换而言之，当值类型的变量存在到或不能存在，因此执行值类型的实例变量。

如果声明符包含`As`子句，子句可以指定该声明引入的成员的类型。 如果省略的类型以及正在使用严格的语义，发生编译时错误。 否则成员的类型是隐式`Object`或类型的成员的类型字符。

__请注意。__ 在语法中没有任何二义性： 如果一个声明符省略了一种类型，它将始终使用下面的声明符的类型。

实例或共享的变量的类型或数组元素类型的可访问域必须与相同或实例或共享的变量本身的可访问域的超集。

下面的示例演示`Color`类具有名为的内部实例变量`redPart`， `greenPart`，和`bluePart`:

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub
End Class
```


### <a name="read-only-variables"></a>只读变量

当实例或共享的变量声明中包括`ReadOnly`修饰符，该声明引入的变量赋值可能仅出现在声明或同一个类的构造函数中的过程。 具体而言，仅在以下情况下允许分配到只读实例或共享的变量：

* 中引入了 （通过在声明中包括变量的初始值设定项） 的实例或共享的变量的变量声明。

* 对于实例变量，包含的变量声明的类的实例构造函数中。 实例变量只能以非限定方式或通过访问`Me`或`MyClass`。

* 共享包含共享的变量声明的类的构造函数中的共享变量。

时需要常量值的符号名称，但在常量声明中，不允许的值类型时或由常数表达式，不能在编译时计算值时，共享的只读变量非常有用。

示例的第一个此类应用程序如下所示，共享的变量声明中哪种颜色`ReadOnly`以防止其被其他程序正在更改：

```vb
Class Color
    Friend redPart As Short
    Friend bluePart As Short
    Friend greenPart As Short

    Public Sub New(red As Short, blue As Short, green As Short)
        redPart = red
        bluePart = blue
        greenPart = green
    End Sub 

    Public Shared ReadOnly Red As Color = New Color(&HFF, 0, 0)
    Public Shared ReadOnly Blue As Color = New Color(0, &HFF, 0)
    Public Shared ReadOnly Green As Color = New Color(0, 0, &HFF)
    Public Shared ReadOnly White As Color = New Color(&HFF, &HFF, &HFF)
End Class
```

常量和共享的只读变量具有不同的语义。 当表达式引用一个常量时，在编译时，获取常量的值，但当表达式引用只读共享的变量，要等到运行时不获取共享变量的值。 请考虑下面的应用程序，其中包括两个单独的程序。

File1.vb:

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

File2.vb:

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

命名空间`Program1`和`Program2`表示两个单独编译的程序。 因为变量`Program1.Utils.X`被声明为`Shared ReadOnly`，通过生成的值输出`Console.WriteLine`语句不在编译时已知，但而不是在运行时获取。 因此，如果的值`X`发生更改并`Program1`重新编译`Console.WriteLine`语句会输出新值即使`Program2`不重新编译。 但是，如果`X`曾常数的值`X`将时获取`Program2`编译，并会一直在的中的更改不会影响`Program1`直到`Program2`已重新编译。

### <a name="withevents-variables"></a>WithEvents 变量

类型可声明它处理引发的事件由它的一个实例或共享的变量声明的实例或共享的变量引发的事件与某些组`WithEvents`修饰符。 例如：

```vb
Class Raiser
    Public Event E1()

    Public Sub Raise()
        RaiseEvent E1
    End Sub
End Class

Module Test
    Private WithEvents x As Raiser

    Private Sub E1Handler() Handles x.E1
        Console.WriteLine("Raised")
    End Sub

    Public Sub Main()
        x = New Raiser()
    End Sub
End Module
```

在此示例中，该方法`E1Handler`处理事件`E1`类型的实例引发`Raiser`实例变量中存储`x`。

`WithEvents`修饰符导致要用前导下划线重命名并替换具有相同名称的事件挂钩的属性的变量。 例如，如果变量的名称是`F`，重命名为`_F`和属性`F`隐式声明。 如果变量的新名称与另一个声明之间的冲突，将报告一个编译时错误。 应用于该变量的任何特性将转移至已重命名的变量。

通过创建的隐式属性`WithEvents`声明负责挂接和方法取消的相关事件处理程序。 该属性值分配给变量时，首先调用`remove`变量 （如果有，方法取消现有的事件处理程序） 中的当前实例上的事件的方法。 接下来进行分配，并且该属性会调用`add`（挂接新的事件处理程序） 的变量中的新实例上的事件的方法。 下面的代码是等效于标准模块上面的代码`Test`:

```vb
Module Test
    Private _x As Raiser

    Public Property x() As Raiser
        Get
            Return _x
        End Get

        Set (Value As Raiser)
            ' Unhook any existing handlers.
            If _x IsNot Nothing Then
                RemoveHandler _x.E1, AddressOf E1Handler
            End If

            ' Change value.
            _x = Value

            ' Hook-up new handlers.
            If _x IsNot Nothing Then
                AddHandler _x.E1, AddressOf E1Handler
            End If
        End Set
    End Property

    Sub E1Handler()
        Console.WriteLine("Raised")
    End Sub

    Sub Main()
        x = New Raiser()
    End Sub
End Module
```

不能用来声明实例或共享的变量作为`WithEvents`如果该变量的类型为结构。 此外，`WithEvents`不能指定在结构中，并`WithEvents`和`ReadOnly`不能结合使用。

### <a name="variable-initializers"></a>变量初始值设定项

实例和共享变量声明中类和实例变量声明 （而非共享变量的声明） 结构中可能包含变量的初始值设定项。 有关`Shared`变量，变量初始值设定项对应于后程序开始，之前执行的赋值语句`Shared`第一次引用变量。 对于实例变量，变量初始值设定项对应于在创建类的实例时执行的赋值语句。 结构不能有实例变量的初始值设定项，因为不能修改其无参数构造函数。

请看下面的示例：

```vb
Class Test
    Public Shared x As Double = Math.Sqrt(2.0)
    Public i As Integer = 100
    Public s As String = "Hello"
End Class

Module TestModule
    Sub Main()
        Dim a As New Test()

        Console.WriteLine("x = " & Test.x & ", i = " & a.i & ", s = " & a.s)
    End Sub
End Module
```

此示例产生以下输出：

```
x = 1.4142135623731, i = 100, s = Hello
```

向赋值`x`加载类时，会发生和分配到`i`和`s`发生时创建类的新实例。

它可用于将自动插入的块类型的构造函数中的赋值语句视为变量初始值设定项。 下面的示例包含多个实例变量初始值设定项。

```vb
Class A
    Private x As Integer = 1
    Private y As Integer = -1
    Private count As Integer

    Public Sub New()
        count = 0
    End Sub

    Public Sub New(n As Integer)
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double = Math.Sqrt(2.0)
    Private items As ArrayList = New ArrayList(100)
    Private max As Integer

    Public Sub New()
        Me.New(100)
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1)
        max = n
    End Sub
End Class
```

此示例对应于代码如下所示，每个注释指示的自动插入的语句的位置。

```vb
Class A
    Private x, y, count As Integer

    Public Sub New()
        MyBase.New ' Invoke object() constructor.
        x = 1 ' This is a variable initializer.
        y = -1 ' This is a variable initializer.
        count = 0
    End Sub

    Public Sub New(n As Integer)
        MyBase.New ' Invoke object() constructor. 
        x = 1 ' This is a variable initializer.
        y = - 1 ' This is a variable initializer.
        count = n
    End Sub
End Class

Class B
    Inherits A

    Private sqrt2 As Double
    Private items As ArrayList
    Private max As Integer

    Public Sub New()
        Me.New(100) 
        items.Add("default")
    End Sub

    Public Sub New(n As Integer)
        MyBase.New(n - 1) 
        sqrt2 = Math.Sqrt(2.0) ' This is a variable initializer.
        items = New ArrayList(100) ' This is a variable initializer.
        max = n
    End Sub
End Class
```

执行任何变量的初始值设定项之前，所有变量都初始化为其类型的默认值。 例如：

```vb
Class Test
    Public Shared b As Boolean
    Public i As Integer
End Class

Module TestModule
    Sub Main()
        Dim t As New Test()
        Console.WriteLine("b = " & Test.b & ", i = " & t.i)
    End Sub
End Module
```

因为`b`类加载时自动初始化为其默认值和`i`将自动初始化为其默认值创建类的实例时，前面的代码产生以下输出：

```
b = False, i = 0
```

每个变量的初始值设定项必须生成值或隐式转换为变量的类型的类型的变量的类型。 变量的初始值设定项可能循环或将在其后，在其中被引用变量的情况下该值是其默认值的初始值设定项用于初始化的变量，请参阅。 此类初始值设定项是值的可疑。

有三种形式的变量的初始值设定项： 正则初始值设定项，数组大小初始值设定项和对象初始值设定项。 前两个窗体出现在等号后面的类型名称之后后, 两个本身声明的一部分。 可能在任何特定声明使用初始值设定项只有一个窗体。

#### <a name="regular-initializers"></a>正则初始值设定项

一个*正则初始值设定项*是隐式转换为变量的类型的表达式。 它出现在等号后面的类型名称，必须分类为一个值之后。 例如：

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

此程序将生成输出：

```vb
x = 10, y = 20
```

如果变量声明具有正则初始值设定项，则可以一次声明一个变量。 例如：

```vb
Module Test
    Sub Main()
        ' OK, only one variable declared at a time.
        Dim x As Integer = 10, y As Integer = 20

        ' Error: Can't initialize multiple variables at once.
        Dim a, b As Integer = 10
    End Sub
End Module
```

#### <a name="object-initializers"></a>对象初始值设定项

*对象初始值设定项*使用到位置的类型名称的对象创建表达式指定。 对象初始值设定项是等效于的对象创建表达式结果赋给变量的正则初始值设定项。 So

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

等效于

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

在对象初始值设定项中的括号始终解释为构造函数的参数列表和永远不会作为数组的类型修饰符。 使用对象初始值设定项的变量名称不能具有数组类型修饰符或为 null 的类型修饰符。

#### <a name="array-size-initializers"></a>数组大小初始值设定项

*数组大小初始值设定项*是为提供的维度设置上限，由表达式表示一组变量的名称上的修饰符。

```antlr
ArraySizeInitializationModifier
    : OpenParenthesis BoundList CloseParenthesis ArrayTypeModifiers?
    ;

BoundList
    : Bound ( Comma Bound )*
    ;

Bound
    : Expression
    | '0' 'To' Expression
    ;
```

上限表达式必须分类为值和必须可隐式转换为`Integer`。 设置上限，组相当于给定的上限的数组创建表达式的变量初始值设定项。 从数组大小初始值设定项推断数组类型的维度数。 So

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

等效于

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

所有的上限必须等于或大于-1，并且所有维度都必须指定上限。 如果正在初始化数组的元素类型本身是数组类型，数组类型修饰符转到的数组大小初始值设定项的右侧。 例如

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

声明本地变量`x`其类型是一个三维数组的元素的二维数组`Integer`，将初始化为一个包含边界的数组`0..5`中的第一个维度和`0..10`第二个维度中。 不能使用数组大小初始值设定项初始化类型为数组的数组变量中的元素。

具有数组大小初始值设定项的变量声明不能具有其类型或正则初始值设定项的数组类型修饰符。


### <a name="systemmarshalbyrefobject-classes"></a>System.MarshalByRefObject 类

从类派生的类`System.MarshalByRefObject`跨使用代理服务器的上下文边界封送 (即，通过引用) 而不是通过复制 (即，按值)。 这意味着类的实例可能不是 true 的实例，但只能是存根封送变量访问和方法调用跨上下文边界。

因此，不能创建对此类类上定义的变量的存储位置的引用。 这意味着，变量类型，如类派生自`System.MarshalByRefObject`不能传递引用参数和方法和变量的类型为值类型不可以进行访问的变量。 相反，Visual Basic 将就好像属性 （因为限制是在属性上相同），这样的类上定义的变量。

此规则的例外： 成员隐式或显式使用限定`Me`已免除上述限制，因为`Me`始终保证是一个实际对象，不是一个代理。

## <a name="properties"></a>Properties

*属性*是变量的自然扩展; 两者都命名成员与相关的类型，而且用于访问变量和属性的语法是一样的。 与变量不同，但是，属性不表示存储位置。 相反，属性有*访问器*，它指定要读取或写入它们的值执行的语句。

与属性声明定义属性。 属性声明的第一部分类似于字段声明。 第二部分包括`Get`访问器和/或`Set`访问器。

```antlr
PropertyMemberDeclaration
    : RegularPropertyMemberDeclaration
    | MustOverridePropertyMemberDeclaration
    | AutoPropertyMemberDeclaration
    ;

PropertySignature
    : 'Property'
      Identifier ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )?
    ;

RegularPropertyMemberDeclaration
    : Attributes? PropertyModifier* PropertySignature
      ImplementsClause? LineTerminator
      PropertyAccessorDeclaration+
      'End' 'Property' StatementTerminator
    ;

MustOverridePropertyMemberDeclaration
    : Attributes? MustOverridePropertyModifier+ PropertySignature
      ImplementsClause? StatementTerminator
    ;

AutoPropertyMemberDeclaration
    : Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      ( 'As' Attributes? TypeName )? ( Equals Expression )?
      ImplementsClause? LineTerminator
    | Attributes? AutoPropertyModifier* 'Property' Identifier
      ( OpenParenthesis ParameterList? CloseParenthesis )?
      'As' Attributes? 'New'
      ( NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )? )?
      ObjectCreationExpressionInitializer?
      ImplementsClause? LineTerminator
    ;

InterfacePropertyMemberDeclaration
    : Attributes? InterfacePropertyModifier* PropertySignature StatementTerminator
    ;

AutoPropertyModifier
    : AccessModifier
    | 'Shadows'
    | 'Shared'
    | 'Overridable'
    | 'NotOverridable'
    | 'Overrides'
    | 'Overloads'
    ;

PropertyModifier
    : AutoPropertyModifier
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    | 'Iterator'
    ;

MustOverridePropertyModifier
    : PropertyModifier
    | 'MustOverride'
    ;

InterfacePropertyModifier
    : 'Shadows'
    | 'Overloads'
    | 'Default'
    | 'ReadOnly'
    | 'WriteOnly'
    ;

PropertyAccessorDeclaration
    : PropertyGetDeclaration
    | PropertySetDeclaration
    ;
```

在以下示例中，`Button`类定义`Caption`属性。

```vb
Public Class Button
    Private captionValue As String

    Public Property Caption() As String
        Get
            Return captionValue
        End Get

        Set (Value As String)
            captionValue = value
            Repaint()
        End Set
    End Property

    ...
End Class
```

基于`Button`上面的类，以下是使用的示例`Caption`属性：

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

在这里，`Set`通过将值分配给属性，调用访问器和`Get`通过引用在表达式中的属性来调用访问器。

如果未指定类型的属性，并且正在使用严格的语义，将发生编译时错误;否则该属性的类型是隐式`Object`或类型的属性的类型字符。 属性声明可以包含以下任一`Get`访问器，它检索的属性值，`Set`访问器，用于存储和 / 或属性的值。 属性隐式声明的方法，因为可能会使用一种方法与相同的修饰符声明属性。 如果属性是在接口中定义或定义与`MustOverride`修饰符、 属性体和`End`必须省略构造; 否则，会发生编译时错误。

索引参数列表组成的属性的签名，以便在索引参数但不是在属性的类型可重载属性。 索引参数列表与正则方法相同。 不过，可以使用修改无参数`ByRef`修饰符和其中任何一个名为`Value`(这中的隐式值参数为保留`Set`访问器)。

可能按如下所示声明一个属性：

* 如果该属性不指定任何属性的类型修饰符，属性必须同时具有`Get`访问器和一个`Set`访问器。 该属性称为为读写属性。

* 如果该属性指定`ReadOnly`修饰符，该属性必须具有`Get`访问器并且可能不`Set`访问器。 该属性称为为只读属性。 它是只读的属性赋值的目标的编译时错误。

* 如果该属性指定`WriteOnly`修饰符，该属性必须具有`Set`访问器并且可能不`Get`访问器。 该属性称为为只写属性。 它是以引用作为赋值目标或作为一种方法的参数除外表达式中只写属性的编译时错误。

`Get`和`Set`属性访问器不是不同的成员，并且不能单独声明属性访问器。 下面的示例不声明一个读写属性。 相反，它声明了两个具有相同的名称，其中一个只读属性，另一个只写：

```vb
Class A
    Private nameValue As String

    ' Error, contains a duplicate member name.
    Public ReadOnly Property Name() As String 
        Get
            Return nameValue
        End Get
    End Property

    ' Error, contains a duplicate member name.
    Public WriteOnly Property Name() As String 
        Set (Value As String)
            nameValue = value
        End Set
    End Property
End Class
```

由于在同一类中声明的两个成员不能具有相同的名称，则示例将导致编译时错误。

默认情况下，一个属性的可访问性`Get`和`Set`访问器是与该属性本身的可访问性相同。 但是，`Get`和`Set`访问器还可以指定独立于属性的可访问性。 在这种情况下，取值函数的可访问性必须是属性的可访问性比限制性更强，并且只有一个访问器可以具有不同的可访问性级别从属性。 访问类型会被视为增加或减少限制，如下所示：

* `Private` 没有更严格`Public`， `Protected Friend`， `Protected`，或`Friend`。

* `Friend` 没有更严格`Protected Friend`或`Public`。

* `Protected` 没有更严格`Protected Friend`或`Public`。

* `Protected Friend` 没有更严格`Public`。

如果属性的访问器之一是可访问，而另一个却没有，则会将属性处理就像它是只读或只写。 例如：

```vb
Class A
    Public Property P() As Integer
        Get
            ...
        End Get

        Private Set (Value As Integer)
            ...
        End Set
    End Property
End Class

Module Test
    Sub Main()
        Dim a As A = New A()

        ' Error: A.P is read-only in this context.
        a.P = 10
    End Sub
End Module
```

当派生的类型隐藏属性时，派生的属性隐藏与读取和写入相关的隐藏的属性。 在以下示例中，`P`中的属性`B`隐藏`P`中的属性`A`同时读取和写入方面：

```vb
Class A
    Public WriteOnly Property P() As Integer
        Set (Value As Integer)
        End Set
    End Property
End Class

Class B
    Inherits A

    Public Shadows ReadOnly Property P() As Integer
       Get
       End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As B = New B

        B.P = 10     ' Error, B.P is read-only.
    End Sub
End Module
```

返回类型或参数类型的可访问域必须与相同或该属性本身的可访问域的超集。 属性只能有一个`Set`访问器，另一个`Get`访问器。

声明和调用语法之间的差异除外`Overridable`， `NotOverridable`， `Overrides`， `MustOverride`，并`MustInherit`属性的行为完全相同`Overridable`， `NotOverridable`， `Overrides`， `MustOverride`，和`MustInherit`方法。 属性重写时，重写属性必须是相同类型 （可读写、 只读、 只写）。 `Overridable`属性不能包含`Private`访问器。

在下面的示例`X`是`Overridable`只读属性`Y`是`Overridable`读-写属性，并且`Z`是`MustOverride`读-写属性。

```vb
MustInherit Class A
    Private _y As Integer

    Public Overridable ReadOnly Property X() As Integer
        Get
            Return 0
        End Get
    End Property

    Public Overridable Property Y() As Integer
        Get
            Return _y
         End Get
        Set (Value As Integer)
            _y = value
        End Set
    End Property

    Public MustOverride Property Z() As Integer
End Class
```

因为`Z`是`MustOverride`，则包含类`A`必须声明`MustInherit`。

与此相反，一个类派生自类`A`如下所示：

```vb
Class B
    Inherits A

    Private _z As Integer

    Public Overrides ReadOnly Property X() As Integer
        Get
            Return MyBase.X + 1
        End Get
    End Property

    Public Overrides Property Y() As Integer
        Get
            Return MyBase.Y
        End Get
        Set (Value As Integer)
            If value < 0 Then
                MyBase.Y = 0
            Else
                MyBase.Y = Value
            End If
        End Set
    End Property

    Public Overrides Property Z() As Integer
        Get
            Return _z
        End Get
        Set (Value As Integer)
            _z = Value
        End Set
    End Property
End Class
```

此处，属性的声明`X`，`Y`，和`Z`重写基属性。 每个属性声明完全匹配可访问性修饰符、 类型和相应的继承属性的名称。 `Get`属性访问器`X`并`Set`属性访问器`Y`使用`MyBase`关键字访问继承的属性。 属性的声明`Z`重写`MustOverride`属性--因此，有没有未完成`MustOverride`类中的成员`B`，和`B`允许为常规类。

属性可用于资源直到首次引用的时的初始化延迟。 例如：

```vb
Imports System.IO

Public Class ConsoleStreams
    Private Shared reader As TextReader
    Private Shared writer As TextWriter
    Private Shared errors As TextWriter

    Public Shared ReadOnly Property [In]() As TextReader
        Get
            If reader Is Nothing Then
                reader = Console.In
            End If
            Return reader
        End Get
    End Property

    Public Shared ReadOnly Property Out() As TextWriter
        Get
            If writer Is Nothing Then
                writer = Console.Out
            End If
            Return writer
        End Get
    End Property

    Public Shared ReadOnly Property [Error]() As TextWriter
        Get
            If errors Is Nothing Then
                errors = Console.Error
            End If
            Return errors
        End Get
    End Property
End Class
```

`ConsoleStreams`类包含三个属性`In`， `Out`，和`Error`，分别表示标准输入、 输出和错误设备。 通过公开为属性，这些成员`ConsoleStreams`类可以延迟其初始化，直到它们被实际使用。 例如，在第一次引用时`Out`属性，如下所示`ConsoleStreams.Out.WriteLine("hello, world")`，基础`TextWriter`初始化输出设备。 但如果应用程序不引用`In`和`Error`为这些设备创建属性，则任何对象。


### <a name="get-accessor-declarations"></a>Get 访问器声明

一个`Get`通过使用属性声明访问器 (getter)`Get`声明。 属性`Get`关键字声明包含`Get`后, 跟一个语句块。 给定一个名为属性`P`、 一个`Get`访问器声明隐式声明具有名称的方法`get_P`使用相同的修饰符、 类型和参数列表的属性。 如果该类型包含具有该名称的声明，会导致编译时错误，但名称绑定用于忽略隐式声明。

特殊的本地变量，隐式声明在`Get`具有相同名称为属性访问器正文声明空间表示属性的返回值。 本地变量具有特殊名称解析语义时在表达式中使用。 如果应归类为方法组，如调用表达式，为表达式的上下文中使用本地变量的函数，而不是本地变量将解析名称。 例如：

```vb
ReadOnly Property F(i As Integer) As Integer
    Get
        If i = 0 Then
            F = 1    ' Sets the return value.
        Else
            F = F(i - 1) ' Recursive call.
        End If
    End Get
End Property
```

使用括号可能会导致不明确的情况下 (如`F(1)`其中`F`是其类型是一个一维数组属性)。 在所有不明确的情况下，名称将解析为该属性而不是本地变量。 例如：

```vb
ReadOnly Property F(i As Integer) As Integer()
    Get
        If i = 0 Then
            F = new Integer(2) { 1, 2, 3 }
        Else
            F = F(i - 1) ' Recursive call, not index.
        End If
    End Get
End Property
```

当控制流叶`Get`访问器正文本地变量的值传递回调用表达式。 因为调用了`Get`访问器，从概念上讲相当于读取变量的值，它被认为是好的编程风格的`Get`访问器具有明显的副作用，如下面的示例中所示：

```vb
Class Counter
    Private Value As Integer

    Public ReadOnly Property NextValue() As Integer
        Get
            Value += 1
            Return Value
        End Get
    End Property
End Class
```

值`NextValue`属性依赖于以前访问的属性的次数。 因此，访问该属性会产生明显的副作用，并且此属性应改为作为一种方法实现。

为"无副作用"约定`Get`访问器并不意味着`Get`访问器始终应编写为只返回存储在变量中的值。 实际上，`Get`访问器通常通过访问多个变量，或调用方法计算属性的值。 但是，在正确设计`Get`访问器执行的任何操作都导致中对象的状态的可观察的更改。

__请注意。__ `Get` 访问器具有行位置子例程具有同样的限制。 开始语句，end 语句和块必须出现在逻辑行的开头开始。

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a>设置访问器声明

一个`Set`使用属性组声明来声明访问器 (setter)。 属性组声明包含关键字`Set`，可选的参数列表和语句块。 给定一个名为属性`P`，setter 声明隐式声明具有名称的方法`set_P`使用相同的修饰符和作为属性的参数列表。 如果该类型包含具有该名称的声明，会导致编译时错误，但名称绑定用于忽略隐式声明。

如果指定的参数列表，则它必须具有一个成员，该成员必须具有以外的任何修饰符`ByVal`，并且其类型必须为该属性的类型相同。 该参数表示要设置的属性值。 如果省略此参数，参数名为`Value`隐式声明。

__请注意。__ `Set` 访问器具有行位置子例程具有同样的限制。 开始语句，end 语句和块必须出现在逻辑行的开头开始。

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a>默认属性

一个属性，指定修饰符`Default`称为*默认属性*。 任何类型都允许属性可能具有默认属性，包括接口。 而无需限定属性名称的实例可以引用的默认属性。 因此，给定一个类

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

代码

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

等效于

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

一旦某个属性声明`Default`，是否已声明的所有属性的继承层次结构中该名称上重载成为默认属性，`Default`与否。 声明属性`Default`中派生的类的基类声明为另一个名称的默认属性不需要任何其他修饰符如时`Shadows`或`Overrides`。 这是因为默认属性，也没有任何标识签名，因此不能被隐藏或重载。 例如：

```vb
Class Base
    Public ReadOnly Default Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Base = " & i)
        End Get
    End Property
End Class

Class Derived
    Inherits Base

    ' This hides Item, but does not change the default property.
    Public Shadows ReadOnly Property Item(i As Integer) As Integer
        Get
            Console.WriteLine("Derived = " & i)
        End Get
    End Property
End Class

Class MoreDerived
    Inherits Derived

    ' This declares a new default property, but not Item.
    ' This does not need to be declared Shadows
    Public ReadOnly Default Property Value(i As Integer) As Integer
        Get
            Console.WriteLine("MoreDerived = " & i)
        End Get
    End Property
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = New MoreDerived()
        Dim y As Integer
        Dim z As Derived = x

        y = x(10)        ' Calls MoreDerived.Value.
        y = x.Item(10)   ' Calls Derived.Item
        y = z(10)        ' Calls Base.Item
    End Sub
End Module
```

此程序将生成的输出：

```
MoreDerived = 10
Derived = 10
Base = 10
```

所有类型中声明的默认属性必须具有相同的名称，并为清楚起见，必须指定`Default`修饰符。 原因是不带索引参数的默认属性将导致不明确的情况下，分配包含类的实例时，默认属性必须具有索引参数。 此外，如果一个属性重载上特定名称中包含`Default`修饰符，该名称上重载的所有属性必须都指定它。 默认属性可能不是`Shared`，并且至少一个访问器的属性不得`Private`。

### <a name="automatically-implemented-properties"></a>自动实现的属性

如果属性的任何访问器声明中省略，该属性的实现将自动提供除非属性在接口中声明或声明`MustOverride`。 可以自动实现仅读/写属性不带任何参数;否则将发生编译时错误。

自动实现的属性`x`，即使其中一个重写另一个属性，引入了专用的本地变量`_x`具有相同类型的属性。 如果没有本地变量的名称与另一个声明之间的冲突，将报告一个编译时错误。 自动实现的属性`Get`访问器返回的本地版本和该属性的值`Set`设置该局部变量的值的访问器。 例如，以下声明中：

```vb
Public Property x() As Integer
```

是大致相当于：

```vb
Private _x As Integer
Public Property x() As Integer
    Get
        Return _x
    End Get
    Set (value As Integer)
        _x = value
    End Set
End Property
```

与变量声明自动实现的属性可以包含初始值设定项。 例如：

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

__请注意。__ 初始化自动实现的属性时，它是通过属性，而不是基础的字段进行初始化。 这是因此重写属性可以拦截初始化在需要时。

数组初始值设定项允许在自动实现的属性，只不过没有方法来显式指定数组界限。  例如：

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a>迭代器属性

*迭代器属性*是属性与`Iterator`修饰符。 它使用相同的原因的迭代器方法 (部分[迭代器方法](statements.md#iterator-methods)) 作为生成的序列的简便方法使用其中一个可供`For Each`语句。 `Get`迭代器属性的访问器解释迭代器方法的方式相同。

一个迭代器属性必须具有一个显式`Get`访问器，并且其类型必须是`IEnumerator`，或`IEnumerable`，或`IEnumerator(Of T)`或`IEnumerable(Of T)`某些`T`。

下面是一个迭代器属性的示例：

```vb
Class Family
    Property Daughters As New List(Of String) From {"Beth", "Diane"}
    Property Sons As New List(Of String) From {"Abe", "Carl"}

    ReadOnly Iterator Property Children As IEnumerable(Of String)
        Get
            For Each name In Daughters : Yield name : Next
            For Each name In Sons : Yield name : Next
        End Get
    End Property
End Class

Module Module1
    Sub Main()
        Dim x As New Family
        For Each c In x.Children
            Console.WriteLine(c) ' prints Beth, Diane, Abe, Carl
        Next
    End Sub
End Module
```

## <a name="operators"></a>运算符

*运算符*定义包含类的现有 Visual Basic 运算符的含义的方法。 运算符应用于表达式中的类，运算符会编译到的类中定义的运算符方法调用。 定义一个运算符的类是也称为*重载*运算符。

```antlr
OperatorDeclaration
    : Attributes? OperatorModifier* 'Operator' OverloadableOperator
      OpenParenthesis ParameterList CloseParenthesis
      ( 'As' Attributes? TypeName )? LineTerminator
      Block?
      'End' 'Operator' StatementTerminator
    ;

OperatorModifier
    : 'Public' | 'Shared' | 'Overloads' | 'Shadows' | 'Widening' | 'Narrowing'
    ;

OverloadableOperator
    : '+' | '-' | '*' | '/' | '\\' | '&' | 'Like' | 'Mod' | 'And' | 'Or' | 'Xor'
    | '^' | '<' '<' | '>' '>' | '=' | '<' '>' | '>' | '<' | '>' '=' | '<' '='
    | 'Not' | 'IsTrue' | 'IsFalse' | 'CType'
    ;
```

不能重载已经存在; 运算符在实践中，这主要适用于转换运算符。 例如，不能重载从派生类到基类的转换：

```vb
Class Base
End Class

Class Derived
    ' Cannot redefine conversion from Derived to Base,
    ' conversion will be ignored.
    Public Shared Widening Operator CType(s As Derived) As Base
        ...
    End Operator
End Class
```

此外可以在 word 的常识重载运算符：

```vb
Class Base
    Public Shared Widening Operator CType(b As Base) As Integer
        ...
    End Operator

    Public Shared Narrowing Operator CType(i As Integer) As Base
        ...
    End Operator
End Class
```

运算符声明不显式添加名称为包含类型声明空间;但是它们隐式声明开头的字符"op_"的相应方法。 以下部分列出了每个运算符与相应的方法名称。

有三个类可以定义的运算符： 一元运算符、 二元运算符和转换运算符。 所有运算符声明都共享某些限制：

* 运算符声明都必须始终是`Public`和`Shared`。 `Public`修饰符，则可以省略在其中，将假设修饰符的上下文中。

* 不能声明运算符的参数`ByRef`，`Optional`或`ParamArray`。

* 在至少一个操作数或返回值的类型必须包含运算符的类型。

* 没有为运算符定义的函数返回变量。 因此，`Return`语句必须用于从一个运算符正文返回值。

这些限制的唯一例外适用于可以为 null 值类型。 可以为 null 值类型不具有实际类型定义，因为值类型可以声明为可以为 null 的类型版本的用户定义的运算符。 确定某一特定的用户定义运算符，可以将类型声明时`?`修饰符出于有效性检查的第一次删除从所有声明中所涉及的类型。 这一放宽不适用于的返回类型`IsTrue`并`IsFalse`运算符; 它们必须仍返回`Boolean`，而不`Boolean?`。

在运算符声明不能修改的优先顺序和运算符的关联性。

__请注意。__ 运算符具有子例程的行位置上具有同样的限制。 开始语句，end 语句和块必须出现在逻辑行的开头开始。


### <a name="unary-operators"></a>一元运算符

可以重载下列一元运算符：

* 一元加运算符`+`(相应的方法： `op_UnaryPlus`)

* 一元负运算符`-`(相应的方法： `op_UnaryNegation`)

* 逻辑`Not`运算符 (相应的方法： `op_OnesComplement`)

* `IsTrue`并`IsFalse`运算符 (相应的方法： `op_True`， `op_False`)

所有重载的一元运算符必须采用一个参数包含类型，并可能会返回任何类型，除了`IsTrue`并`IsFalse`，其必须返回`Boolean`。 如果包含类型是泛型类型，类型参数必须与包含类型的类型参数匹配。 例如，应用于对象的

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

如果某个类型重载之一`IsTrue`或`IsFalse`，则它必须对其他重载。 如果只有一个已重载，会导致编译时错误。

__请注意。__ `IsTrue` 和`IsFalse`不是保留的字。

### <a name="binary-operators"></a>二元运算符

以下二进制运算符可以进行重载：

* 加法`+`，减法`-`，乘法`*`，除`/`，整数除法`\`，取模`Mod`和求幂`^`运算符 (相应的方法：`op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)

* 关系运算符`=`， `<>`， `<`， `>`， `<=`， `>=` (相应的方法： `op_Equality`， `op_Inequality`， `op_LessThan`， `op_GreaterThan`， `op_LessThanOrEqual`, `op_GreaterThanOrEqual`). __请注意。__ 虽然可以重载相等运算符，不能重载赋值运算符 （仅在赋值语句中使用）。

* `Like`运算符 (相应的方法： `op_Like`)

* 串联运算符`&`(相应的方法： `op_Concatenate`)

* 逻辑`And`，`Or`并`Xor`运算符 (相应的方法： `op_BitwiseAnd`， `op_BitwiseOr`， `op_ExclusiveOr`)

* 移位运算符`<<`并`>>`(相应的方法： `op_LeftShift`， `op_RightShift`)

所有重载的二元运算符必须将包含类型作为参数之一。 如果包含类型是泛型类型，类型参数必须与包含类型的类型参数匹配。 移位运算符进一步限制此规则，要求第一个参数应包含类型;第二个参数必须始终为类型的`Integer`。

必须在对声明以下二进制运算符：

* 运算符`=`和运算符 `<>`

* 运算符`>`和运算符 `<`

* 运算符`>=`和运算符 `<=`

如果该对的其中一个被声明，则还必须具有匹配的参数和返回类型声明的其他或将导致编译时错误。 (__注意。__ 需要配对关系运算符的声明的目的是尝试并确保至少最低级别的重载运算符的逻辑一致性。）

与关系运算符不同部门和整数除法运算符重载是强烈建议不要使用的但不是错误。 (__注意。__ 一般情况下，两种类型的部门应为完全不同： 支持部门的类型为整数 (在这种情况下它应支持`\`) 或不 (在这种情况下它应支持`/`)。 我们考虑使其错误来定义这两个运算符，但因为其语言通常不区分两种类型的除法 Visual Basic 的方式，我们认为它是最安全，若要允许的做法，但强烈不建议这样做）。

复合的赋值运算符不能直接重载。 相反，当重载对应的二元运算符时，复合赋值运算符将重载的运算符。 例如：

```vb
Structure Complex
    ...

    Public Shared Operator +(x As Complex, y As Complex) _
        As Complex
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim c1, c2 As Complex
        ' Calls the overloaded + operator
        c1 += c2
    End Sub
End Module
```

### <a name="conversion-operators"></a>转换运算符

转换运算符定义类型之间的新转换。 这些新转换称为*用户定义的转换*。 转换运算符将源类型，即转换运算符，为目标类型，指示转换运算符的返回类型的参数类型转换。 转换必须归类为扩大转换或收缩。 包括的转换运算符声明`Widening`关键字引入的用户定义的扩大转换 (相应的方法： `op_Implicit`)。 包括的转换运算符声明`Narrowing`关键字引入的用户定义的收缩转换 (相应的方法： `op_Explicit`)。

通常情况下，应设计用户定义的扩大转换，永远不会引发异常，并且永远不会丢失信息。 如果 （例如，因为源参数不在范围内），用户定义的转换可能会导致异常或丢失的信息 （如放弃高顺序位），则该转换应定义为收缩转换。 在下面的示例中：

```vb
Structure Digit
    Dim value As Byte

    Public Sub New(value As Byte)
        if value < 0 OrElse value > 9 Then Throw New ArgumentException()
        Me.value = value
    End Sub

    Public Shared Widening Operator CType(d As Digit) As Byte
        Return d.value
    End Operator

    Public Shared Narrowing Operator CType(b As Byte) As Digit
        Return New Digit(b)
    End Operator
End Structure
```

从转换`Digit`到`Byte`是一个扩大转换，因为它永远不会引发异常或丢失信息，但从转换`Byte`到`Digit`以来的收缩转换`Digit`只能表示可能值的子集`Byte`。

与其他类型的所有成员可进行重载，不同的转换运算符的签名包含转换的目标类型。 这是唯一的类型成员签名中参与的返回类型。 扩大转换或收缩分类的转换运算符，但是，不是签名的运算符的一部分。 因此，类或结构無法宣告的扩大转换运算符和收缩转换运算符具有相同的源和目标类型。

用户定义的转换运算符必须转换为或从包含类型--例如，它是由于可以将类`C`定义的转换从`C`到`Integer`并从`Integer`到`C`，但不能从`Integer`到`Boolean`。 如果包含类型是泛型类型，类型参数必须与包含类型的类型参数匹配。 此外，不能重新定义的内部函数 （即非-用户定义的） 转换。 一个类型不能声明转换的结果，其中：

* 源类型和目标类型是相同的。

* 源类型和目标类型不是定义转换运算符的类型。

* 源类型或目标类型是一个接口类型。

* 通过继承相关联的源类型和目标类型 (包括`Object`)。

这些规则的唯一例外适用于可以为 null 值类型。 可以为 null 值类型不具有实际类型定义，因为值类型可以声明为可以为 null 的类型版本的用户定义的转换。 确定特定的用户定义转换，可以将类型声明时`?`修饰符出于有效性检查的第一次删除从所有声明中所涉及的类型。 因此，以下声明是有效的因为`S`可以定义从转换`S`到`T`:

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Widening Operator CType(ByVal v As S?) As T
    ...
    End Operator
End Structure
```

下面的声明不是有效的但是，因为结构`S`不能定义从转换`S`到`S`:

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a>运算符映射

由于 Visual Basic 支持的运算符集可能不完全匹配的运算符集在.NET Framework 上的其他语言，某些运算符映射到其他运算符时正在定义或使用专门。 尤其是在下列情况下：

* 定义一个整数除法运算符将自动定义正则除法运算符 （仅从其他语言使用），将调用整数除法运算符。

* 重载`Not`， `And`，和`Or`运算符将重载仅从其他语言之间的逻辑和按位运算符区分开来的角度来看的按位运算符。

* 重载仅逻辑和位运算符可区分的语言中的逻辑运算符的类 (即使用语言`op_LogicalNot`， `op_LogicalAnd`，并`op_LogicalOr`有关`Not`， `And`，和`Or`分别) 将具有其映射到 Visual Basic 逻辑运算符的逻辑运算符。 如果重载逻辑和位运算符时，将使用只有按位运算符。

* 重载`<<`和`>>`运算符将重载运算符从签名和未签名的移位运算符区分其他语言的角度来看仅签名。

* 重载的无符号的移位运算符的类将有映射到相应的 Visual Basic 移位运算符的无符号的移位运算符。 如果这两个未签名和签名移位运算符被重载，将使用仅为有符号的移位运算符。
