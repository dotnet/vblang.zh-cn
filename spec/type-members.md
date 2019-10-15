---
ms.openlocfilehash: 3d5c1e90283b6d6ec8cdeccd35e32c78f997cc27
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306038"
---
# <a name="type-members"></a>类型成员

类型成员定义存储位置和可执行代码。 它们可以是方法、构造函数、事件、常量、变量和属性。

## <a name="interface-method-implementation"></a>接口方法实现

方法、事件和属性可以实现接口成员。 若要实现接口成员，成员声明将指定 @no__t 的关键字，并列出一个或多个接口成员。

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

实现接口成员的方法和属性将隐式 `NotOverridable`，除非声明为 `MustOverride`、`Overridable` 或重写另一个成员。 实现接口成员的成员应 `Shared`，这是错误的。 成员的可访问性不会影响其实现接口成员的能力。

要使接口实现有效，包含类型的实现列表必须命名包含兼容成员的接口。 兼容成员是其签名与实现成员的签名相匹配的成员。 如果正在实现泛型接口，则在检查兼容性时，实现子句中提供的类型参数会被替换为签名。 例如：

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

如果使用委托类型声明的事件是实现接口事件，则兼容事件是其基础委托类型为同一类型的事件之一。 否则，该事件将使用它所实现的接口事件中的委托类型。 如果此类事件实现了多个接口事件，则所有接口事件必须具有相同的基础委托类型。 例如：

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

使用类型名称、句点和标识符指定实现列表中的接口成员。 类型名称必须是实现列表中的接口或实现列表中接口的基接口，并且标识符必须是指定接口的成员。 单个成员可以实现一个以上的匹配接口成员。

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

如果实现的接口成员在所有显式实现的接口中都不可用，因为有多个接口继承，则实现成员必须显式引用可用于该成员的基接口。 例如，如果 @no__t 0，`I2` 包含成员 `M`，而 `I3` 从 @no__t 和 @no__t 继承，则实现 @no__t 的类型将实现 @no__t 7 和 @no__t。 如果接口隐藏了继承的成员，则实现类型将必须实现继承成员，并且成员将隐藏它们。

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

如果实现的接口成员的包含接口是泛型的，则必须提供与要实现的接口相同的类型参数。 例如：

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

方法包含程序的可执行语句。

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

具有可选参数列表和可选返回值的方法是 "共享" 或 "不共享"。 通过类或类的实例访问共享方法。 非共享方法（也称为实例方法）通过类的实例进行访问。 下面的示例显示了一个类 `Stack`，其中包含多个共享方法（`Clone` 和 `Flip`）以及若干实例方法（`Push`、`Pop` 和 `ToString`）：

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

可以重载方法，这意味着多个方法可能具有相同的名称，只要它们具有唯一的签名。 方法的签名包含其参数的数目和类型。 具体而言，方法的签名不包括返回类型或参数修饰符，如 Optional、ByRef 或 ParamArray。 下面的示例演示一个具有若干重载的类：

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

```console
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

仅可选参数不同的重载可用于库的 "版本控制"。 例如，库的 v1 可能包含带可选参数的函数：

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

然后，该库的 v2 要添加另一个可选参数 "password"，它想要这样做，而不会破坏源兼容性（因此可以重新编译用于目标 v1 的应用程序），并且不会中断二进制兼容性（因此，应用程序用于参考 v1 现在可以引用 v2 而不进行重新编译。 这就是 v2 的外观：

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

请注意，公共 API 中的可选参数不符合 CLS。 但是，它们至少可以由 Visual Basic 和 c # 4 和F#使用。



### <a name="regular-async-and-iterator-method-declarations"></a>常规、异步和迭代器方法声明

有两种类型的*方法：不*返回值的方法和*函数*。 如果在接口中定义了方法或具有 @no__t 的修饰符，则只能省略方法的 body 和 `End` 构造。 如果未在函数上指定返回类型，并且使用的是严格语义，则会发生编译时错误;否则，该类型将隐式 `Object` 或该方法类型字符的类型。 方法的返回类型和参数类型的可访问域必须与方法本身的可访问域的可访问域相同或超集。

__常规方法__是不 `Async` 和 `Iterator` 修饰符。 它可以是子例程或函数。 部分[常规方法](statements.md#regular-methods)详细说明了在调用常规方法时会发生的情况。

__迭代器方法__是一个具有 `Iterator` 修饰符并且没有 @no__t 的修饰符。 它必须是一个函数，并且它的返回类型必须为 `IEnumerator`、@no__t 为-1 或 `IEnumerator(Of T)` 或 `IEnumerable(Of T)` （对于某些 @no__t），并且它必须不包含 @no__t 参数。 节[迭代器方法](statements.md#iterator-methods)详细说明调用迭代器方法时所发生的情况。

__Async 方法__是一个具有 `Async` 修饰符并且没有 @no__t 的修饰符。 它必须是一个子例程，或是一个返回类型为 `Task` 或 @no__t 为（对于某些 @no__t）的函数，并且必须没有任何 @no__t 3 参数。 节[Async 方法](statements.md#async-methods)详细说明了在调用 Async 方法时会发生的情况。

如果某个方法不是以下三种方法之一，则会发生编译时错误。

子程序和函数声明特别适用于每个语句的开始和结束语句，都必须从逻辑行的开头开始。 此外，非 @no__t 的子程序或函数声明的主体必须从逻辑行的开头开始。 例如：

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

外部方法声明引入了一个新方法，该方法的实现是在程序外部提供的。

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

由于外部方法声明不提供实际的实现，因此它没有方法体或 `End` 构造。 外部方法是隐式共享的，不能具有类型参数，并且不能处理事件或实现接口成员。 如果未在函数上指定返回类型，并且使用了严格语义，则会发生编译时错误。 否则，该类型将隐式 `Object` 或该方法类型字符的类型。 外部方法的返回类型和参数类型的可访问域必须与外部方法本身的可访问性域的可访问域相同或超集。

外部方法声明的 library 子句指定了实现方法的外部文件的名称。 可选的 alias 子句是一个字符串，它指定外部文件中的数字序号（前缀为 @no__t 0 字符）或方法名称。 还可以指定单字符集修饰符，此修饰符控制在调用外部方法的过程中用于封送字符串的字符集。 @No__t-0 修饰符将所有字符串封送到 Unicode 值，`Ansi` 修饰符将所有字符串封送为 ANSI 值，@no__t 2 修饰符 .NET Framework 根据方法的名称或别名（如果已指定）封送字符串。 如果未指定修饰符，则默认值为 `Ansi`。

如果指定 `Ansi` 或 `Unicode`，则在外部文件中查找方法名称，并且不进行修改。 如果指定 `Auto`，则方法名称查找取决于平台。 如果平台被视为 ANSI （例如，Windows 95、Windows 98、Windows ME），则将查找方法名称，但不进行任何修改。 如果查找失败，则追加 @no__t 0，并再次尝试查找。 如果平台被视为 Unicode （例如，Windows NT、Windows 2000、Windows XP），则追加 @no__t 0，并查找该名称。 如果查找失败，则再次尝试查找，而不会 `W`。 例如：

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

传递给外部方法的数据类型根据 .NET Framework 数据封送处理约定进行封送处理，但有一个例外。 通过值传递的字符串变量（即 @no__t 0）将封送到 OLE 自动化 BSTR 类型，对外部方法中的 BSTR 所做的更改将反映在字符串参数中。 这是因为，外部方法中 `String` 的类型是可变的，而这种特殊的封送模拟该行为。 通过引用传递的字符串参数（即 @no__t 0）将作为指向 OLE 自动化 BSTR 类型的指针进行封送处理。 可以通过在参数上指定 `System.Runtime.InteropServices.MarshalAsAttribute` 特性来重写这些特殊行为。

该示例演示如何使用外部方法：

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

@No__t-0 修饰符指示方法是可重写的。 @No__t-0 修饰符指示方法重写具有相同签名的基类型可重写方法。 @No__t-0 修饰符指示无法进一步重写可重写的方法。 @No__t-0 修饰符指示必须在派生类中重写方法。

这些修饰符的某些组合无效：

* `Overridable` 和 `NotOverridable` 互相排斥，因此不能组合。

* `MustOverride` 表示 @no__t 为-1 （因此无法指定它），并且不能与 @no__t 2 进行组合。

* `NotOverridable` 不能与 @no__t 或 @no__t 结合，并且必须与 `Overrides` 组合。

* `Overrides` 表示 @no__t 为-1 （因此无法指定它），并且不能与 @no__t 2 进行组合。

对于可重写方法还存在其他限制：

* @No__t-0 方法不能包含方法体或 @no__t 构造，不能重写其他方法，并且只能出现在 @no__t 类中。

* 如果方法指定 `Overrides`，并且没有要重写的匹配基方法，则会发生编译时错误。 重写的方法不能指定 `Shadows`。

* 如果重写方法的可访问域与被重写的方法的可访问性域不相等，则方法不能重写另一个方法。 一种例外情况是，一个方法重写另一个程序集中没有 `Friend` 访问权限的 `Protected Friend` 方法，必须指定 `Protected` （不 `Protected Friend`）。

* `Private` 方法不能 `Overridable`、`NotOverridable` 或 `MustOverride`，也不能覆盖其他方法。

* @No__t-0 类中的方法不能声明为 `Overridable` 或 `MustOverride`。

下面的示例演示可重写方法和 nonoverridable 方法之间的差异：

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

在此示例中，类 `Base` 引入了一个方法 `F` 和一个 `Overridable` @no__t 方法。 类 @no__t 引入了新的方法 `F`，因此会将继承的 @no__t 为隐藏，同时还会重写继承的方法 `G`。 此示例产生以下输出：

```console
Base.F
Derived.F
Derived.G
Derived.G
```

请注意，语句 `b.G()` 调用 `Derived.G`，而不是 `Base.G`。 这是因为实例的运行时类型（`Derived`），而不是实例的编译时类型（即 @no__t 为1）确定要调用的实际方法实现。

### <a name="shared-methods"></a>共享方法

@No__t 的修饰符表示方法是一个*共享方法*。 共享方法不会对类型的特定实例进行操作，并且可以直接从类型调用，而不是通过类型的特定实例调用。 不过，它有效地使用实例来限定共享方法。 在共享方法中引用 `Me`、`MyClass` 或 `MyBase` 无效。 共享方法可能不 `Overridable`、`NotOverridable` 或 `MustOverride`，并且它们可能不会重写方法。 标准模块和接口中定义的方法不能指定 `Shared`，因为它们已隐式 `Shared`。

在没有 `Shared` 修饰符的结构或类中声明的方法是*实例方法*。 实例方法在类型的给定实例上操作。 仅可通过类型的实例调用实例方法，并可通过 `Me` 表达式引用实例。

下面的示例演示用于访问共享成员和实例成员的规则：

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

方法 `F` 显示在实例函数成员中，标识符可用于访问实例成员和共享成员。 方法 `G` 在共享函数成员中显示，通过标识符访问实例成员是错误的。 方法 `Main` 表示在成员访问表达式中，实例成员必须通过实例访问，但可以通过类型或实例访问共享成员。

### <a name="method-parameters"></a>方法参数

*参数*是一个变量，可用于将信息传入和传出方法。 方法的参数由方法的参数列表声明，该参数列表由一个或多个以逗号分隔的参数组成。

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

如果没有为参数指定类型，并且使用了严格语义，则会发生编译时错误。 否则，默认类型为 `Object` 或参数类型字符的类型。 即使在 "宽松语义" 下，如果一个参数包含 `As` 子句，所有参数都必须指定类型。

参数由修饰符指定为 value、reference、optional 或 paramarray 参数，分别为 `ByVal`、`ByRef`、`Optional` 和 @no__t 3。 不指定 `ByRef` 或 @no__t 默认为 `ByVal` 的参数。

参数名称的范围限定为方法的整个主体，并且始终可公开访问。 方法调用创建特定于该调用的副本，并且调用的参数列表将值或变量引用分配给新创建的参数。 由于外部方法声明和委托声明没有正文，因此参数列表中允许有重复的参数名，但不建议使用。

标识符后面可以是可以为 null 的 name 修饰符 `?` 指示它可以为 null，并且还通过数组名称修饰符来指示它是一个数组。 它们可能会组合，例如 "`ByVal x?() As Integer`"。 不允许使用显式数组界限;此外，如果有可以为 null 的 name 修饰符，则必须存在 @no__t 的-0 子句。


#### <a name="value-parameters"></a>值参数

*值参数*是使用显式 `ByVal` 修饰符声明的。 如果使用 `ByVal` 修饰符，则不能指定 @no__t 修饰符。 值参数与参数所属的成员的调用一起存在，并使用调用中给定的自变量的值初始化。 返回成员时，值参数将不再存在。

允许方法为值参数赋值。 此类分配只会影响 value 参数所表示的本地存储位置;它们不会影响方法调用中给定的实际参数。

当参数的值传递到方法中时，将使用值参数，并且参数的修改不会影响原始参数。 值参数引用其自己的变量，该变量与相应参数的变量不同。 通过复制相应参数的值来初始化此变量。 下面的示例演示一个方法 `F`，它具有名为 `p` 的值参数：

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

即使修改了 value 参数 `p`，该示例仍会生成以下输出：

```console
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a>引用参数

Reference 参数是用 `ByRef` 修饰符声明的参数。 如果指定了 `ByRef` 修饰符，则不可能使用 @no__t 修饰符。 引用参数不会创建新的存储位置。 相反，引用参数表示作为方法或构造函数调用中的自变量提供的变量。 从概念上讲，引用参数的值始终与基础变量相同。

引用参数在两种模式下操作，可以是*别名*，也可以通过*复制回复制。*

__别名.__ 如果参数充当调用方提供的参数的别名，则使用 reference 参数。 引用参数本身不定义变量，而是指对应参数的变量。 直接修改引用参数，并立即影响相应的参数。 下面的示例演示一个具有两个引用参数 @no__t 的方法：

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

```console
pre: x = 1, y = 2
post: x = 2, y = 1
```

对于类 `Main` 中方法 `Swap` 的调用，`a` 表示 @no__t，`b` 表示 `y`。 因此，调用具有交换 @no__t 值和 `y` 的值的效果。

在采用引用参数的方法中，多个名称可能表示相同的存储位置：

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

在此示例中，调用的方法 `F` 在 `G` 中为 @no__t 和 @no__t 传递对 `s` 的引用。 因此，对于该调用，名称 `s`、`a` 和 @no__t 均引用相同的存储位置，三个分配都将修改实例变量 `s`。

__复制回副本。__ 如果传递到引用参数的变量类型与引用参数的类型不兼容，或者如果非变量（例如属性）作为参数传递给引用参数，则为; 如果调用是后期绑定的，则为临时变量被分配并传递给引用参数。 在调用方法之前，传入的值将复制到此临时变量中，并将在该方法返回时复制回原始变量（如果有）和原始变量（如果有）。 因此，引用参数可能不一定包含对要传入的变量的确切存储的引用，并且对引用参数所做的任何更改都不会反映在该方法退出之前的变量中。 例如：

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

对于第一次调用 `F`，将创建一个临时变量，并将 @no__t 属性的值分配给它并将其传递到 `F`。 从 `F` 返回后，会将临时变量中的值分配回 `G` 的属性。 在第二种情况下，将创建另一个临时变量，并将 `d` 的值分配给它并将其传递到 `F` 中。 从 `F` 返回时，临时变量中的值会转换回变量类型，`Derived`，并分配给 `d`。 由于传递回的值不能转换为 `Derived`，因此在运行时将引发异常。

#### <a name="optional-parameters"></a>可选参数

使用 `Optional` 修饰符声明可选参数。 形参列表中可选参数之后的参数也必须是可选的;如果在以下参数上指定 `Optional` 修饰符，将触发编译时错误。 某些类型可以为 null 的类型的可选参数 `T?` 或不可为 null 的类型 @no__t 如果未指定参数，则必须指定一个 `e` 的常数表达式作为默认值。 如果 `e` 的计算结果为类型对象 `Nothing`，则*参数类型*的默认值将用作参数的默认值。 否则，@no__t 0 必须是常量表达式，并将其作为参数的默认值。

可选参数是参数的初始值设定项有效的唯一情况。 初始化始终作为调用表达式的一部分完成，而不是在方法体自身内完成。

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

```console
x = 10, y = 20
x = 30, y = 40
```

可选参数不能在委托或事件声明中指定，也不能在 lambda 表达式中指定。

#### <a name="paramarray-parameters"></a>ParamArray 参数

@no__t 参数是用 `ParamArray` 修饰符声明的。 如果 `ParamArray` 修饰符存在，则必须指定 `ByVal` 修饰符，而其他参数也不能使用 `ParamArray` 修饰符。 @No__t 参数的类型必须为一维数组，并且它必须是参数列表中的最后一个参数。

@No__t-0 参数表示 @no__t 类型的类型不确定的参数。 在方法本身中，@no__t 0 参数被视为其声明的类型并且没有特殊语义。 @No__t 的参数是隐式可选的，其默认值为 @no__t 的类型的一维数组的一维数组。

@No__t-0 允许在方法调用中使用以下两种方法之一指定参数：

* 为 @no__t 提供的参数可以是扩大到 @no__t 类型的类型的单个表达式。 在这种情况下，`ParamArray` 的作用与值形参完全相同。

* 此外，调用可以为 @no__t 指定零个或多个参数，其中每个参数都是可隐式转换为 @no__t 的元素类型的类型的表达式。 在这种情况下，调用会创建一个 @no__t 0 类型的实例，该实例的长度与参数的数量相对应，使用给定的参数值初始化数组实例的元素，并使用新创建的数组实例作为实际实际.

除了允许在调用中使用可变数量的自变量，@no__t 0 完全等效于同一类型的值形参，如下面的示例所示。

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

```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

第一次调用 `F` 只是将数组 `a` 作为值参数进行传递。 @No__t 的第二次调用会自动创建具有给定元素值的四元素数组，并将该数组实例作为值参数进行传递。 同样，@no__t 的第三次调用会创建一个0元素数组，并将该实例作为值参数进行传递。 第二次和第三次调用完全等效于写入：

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

不能在委托或事件声明中指定 @no__t 的参数。

### <a name="event-handling"></a>事件处理

方法可以声明方式处理实例中的对象或共享变量引发的事件。 若要处理事件，方法声明会指定 @no__t 的关键字，并列出一个或多个事件。

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

@No__t-0 列表中的事件由以句点分隔的两个标识符指定：

* 第一个标识符必须为包含类型中的实例或共享变量，该类型指定 `WithEvents` 修饰符或 @no__t 或 `MyClass` 或 `Me` 关键字;否则，会发生编译时错误。 此变量包含将引发此方法处理的事件的对象。

* 第二个标识符必须指定第一个标识符的类型的成员。 该成员必须是事件，并且可以共享。 如果为第一个标识符指定了共享变量，则必须共享该事件或错误结果。

如果 `AddHandler E, AddressOf M` 的语句也是有效的，则 `M` 的处理程序方法被视为事件 @no__t 的有效事件处理程序。 但与 @no__t 0 语句不同，显式事件处理程序允许使用不带参数的方法处理事件，而不管是否使用了严格语义：

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

单个成员可以处理多个匹配事件，多个方法可能处理一个事件。 方法的可访问性不会影响其处理事件的能力。 下面的示例演示了方法如何处理事件：

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

这将输出：

```console
Raised
Raised
```

类型继承其基类型提供的所有事件处理程序。 派生类型无法以任何方式更改它从其基类型继承的事件映射，但可能会向事件添加其他处理程序。


### <a name="extension-methods"></a>扩展方法

可以使用*扩展方法*将方法添加到类型声明外部的类型中。 扩展方法是应用了 `System.Runtime.CompilerServices.ExtensionAttribute` 特性的方法。 它们只能在标准模块中声明，并且必须至少有一个参数，该参数指定方法扩展的类型。 例如，下面的扩展方法将类型扩展 `String`：

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

__纪录.__ 尽管 Visual Basic 要求在标准模块中声明扩展方法，但其他语言（如） C#可能会允许在其他类型的类型中声明这些方法。 只要这些方法遵循此处所述的其他约定并且包含类型不是开放式泛型类型并且无法实例化，Visual Basic 将识别扩展方法。

调用扩展方法时，对其调用的实例将传递给第一个参数。 第一个参数不能声明为 `Optional` 或 @no__t 为-1。 任何类型（包括类型参数）都可以显示为扩展方法的第一个参数。 例如，以下方法将类型扩展 `Integer()`、实现 @no__t 的任何类型，以及所有类型：

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

如前面的示例所示，可以扩展接口。 接口扩展方法提供方法的实现，因此，实现接口的类型在其上定义的扩展方法仍只能实现此接口最初声明的成员。 例如：

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

扩展方法也可以对其类型参数具有类型约束，而与非扩展泛型方法一样，可以推断类型参数：

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

还可以通过要扩展的类型内的隐式实例表达式来访问扩展方法：

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

出于辅助功能的目的，扩展方法也被视为在中声明的标准模块的成员--它们不会对其进行扩展的成员以外的成员提供额外的访问权限。

只有在标准模块方法处于范围内时，扩展方法才可用。 否则，原始类型将不会扩展。 例如：

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

当只有类型的扩展方法可用时，引用类型仍会生成编译时错误。

需要注意的是，扩展方法被视为属于成员的所有上下文中的类型的成员，例如强类型 `For Each` 模式。 例如：

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

还可以创建引用扩展方法的委托。 因此，代码如下：

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

大致等效于：

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

__纪录.__ Visual Basic 通常会在实例方法调用中插入检查，如果调用该方法的实例为，@no__t 则会导致 `System.NullReferenceException`，导致出现0。 对于扩展方法，没有有效的方法来插入此项检查，因此扩展方法将需要显式检查 `Nothing`。 

__纪录.__ 将值类型 @no__t 作为参数传递给类型为接口的参数时，将对其进行装箱。  这意味着扩展方法的副作用将对结构的副本（而不是原始结构）进行操作。 尽管语言对扩展方法的第一个参数没有任何限制，但建议不将扩展方法用于扩展值类型，或者在扩展值类型时，第一个参数将传递 `ByRef` 以确保副作用对原始值执行运算。

### <a name="partial-methods"></a>分部方法

*分部方法*是指定签名但不指定方法主体的方法。 方法的主体可由具有相同名称和签名的另一种方法声明提供，最有可能出现在该类型的另一个分部声明中。 例如：

.vb：

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

b. .vb：

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

在此示例中，类的分部声明 `MyForm` 声明分部方法 `ValidateControls`，无实现。 分部声明中的构造函数调用分部方法，即使文件中未提供正文也是如此。 然后，@no__t 的其他分部声明提供方法的实现。

无论是否已提供正文，都可以调用分部方法;如果未提供任何方法体，则忽略此调用。 例如：

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

作为参数传递给被忽略的分部方法调用的任何表达式也会被忽略，并且不会进行计算。 （__注意：__ 这意味着分部方法是一种非常有效的方法，它提供跨两个分部类型定义的行为，因为如果没有使用部分方法，则不会产生任何费用。）

分部方法声明必须声明为 `Private`，并且必须始终是在其主体中没有语句的子例程。 分部方法本身不能实现接口方法，尽管提供其主体的方法可以。

只有一个方法可以向分部方法提供主体。 向分部方法提供主体的方法必须具有与分部方法相同的签名、对任何类型参数的相同约束、相同的声明修饰符以及相同的参数和类型参数名称。 分部方法的属性和提供其正文的方法将合并，这与方法的参数中的任何属性相同。 同样，将合并方法处理的事件列表。 例如：

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

*构造函数*是允许对初始化进行控制的特殊方法。 在程序开始或创建类型的实例后，它们将运行。 与其他成员不同，构造函数不会被继承，也不会在类型的声明空间中引入名称。 构造函数只能由对象创建表达式或 .NET Framework 调用;它们永远不能直接调用。

__纪录.__ 构造函数在子例程具有的行位置上具有相同的限制。 开始语句、结束语句和块必须全部显示在逻辑行的开头。

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

*实例构造函数*初始化类型的实例，并由 .NET Framework 在创建实例时运行。 构造函数的参数列表服从与方法的参数列表相同的规则。 实例构造函数可能会重载。

引用类型中的所有构造函数都必须调用另一个构造函数。 如果调用是显式的，则它必须是构造函数方法体中的第一条语句。 语句可以调用类型的实例构造函数中的另一个（例如 `Me.New(...)` 或 @no__t--），或者，如果它不是结构，则它可以调用该类型的基类型的实例构造函数-例如，`MyBase.New(...)`。 构造函数调用自身是无效的。 如果构造函数省略了对另一个构造函数的调用，则 `MyBase.New()` 是隐式的。 如果没有无参数的基类型构造函数，则会发生编译时错误。 由于在调用基类构造函数之前，不会将 `Me` 视为构造，因此构造函数调用语句的参数不能直接或显式引用 `Me`、@no__t 或 @no__t。

当构造函数的第一个语句的形式为 `MyBase.New(...)` 时，构造函数将隐式执行由类型中声明的实例变量的变量初始值设定项指定的初始化。 这对应于调用直接基类型构造函数之后立即执行的一系列赋值。 此类排序可确保在执行具有对实例的访问权限的任何语句之前，通过其变量初始值设定项来初始化所有基实例变量。 例如：

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

当 `New B()` 用于创建 `B` 的实例时，将生成以下输出：

```console
x = 1, y = 1
```

@No__t-0 的值是 `1`，因为在调用基类构造函数之后执行变量初始值设定项。 变量初始值设定项将按照它们在类型声明中出现的文本顺序执行。

当某个类型仅声明 `Private` 构造函数时，通常不能从类型派生其他类型或创建该类型的实例;唯一的例外是类型嵌套在类型中。 `Private` 构造函数通常用于仅包含 @no__t 1 成员的类型。

如果某个类型不包含任何实例构造函数声明，则会自动提供默认构造函数。 默认构造函数只调用直接基类型的无参数构造函数。 如果直接基类型没有可访问的无参数构造函数，则会发生编译时错误。 默认构造函数的声明的访问类型为 `Public`，除非该类型为 `MustInherit`，在这种情况下，默认构造函数 @no__t 为-2。

__纪录.__ @No__t 类型的默认构造函数的默认访问 @no__t 为-1，因为不能直接创建 `MustInherit` 类。 因此，不需要将默认构造函数 `Public`。

在下面的示例中，提供了一个默认构造函数，因为该类不包含任何构造函数声明：

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

因此，该示例完全等效于以下内容：

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

发出到使用特性标记的设计器生成的类的默认构造函数 `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` 会在调用基构造函数后调用方法 `Sub InitializeComponent()` （如果存在）。 （__注意：__ 这使得设计器生成的文件（如 WinForms 设计器创建的文件）可以在设计器文件中省略该构造函数。 这样一来，程序员可以自行指定它。）

### <a name="shared-constructors"></a>共享构造函数

*共享构造函数*初始化类型的共享变量;它们将在程序开始执行后、对类型成员的任何引用之前运行。 共享构造函数指定 `Shared` 修饰符，除非它在标准模块中，这种情况下，`Shared` 修饰符是隐含的。

与实例构造函数不同，共享构造函数具有隐式公共访问权限，没有参数，也不能调用其他构造函数。 在共享构造函数中的第一个语句之前，共享构造函数会隐式执行由类型中声明的共享变量的变量初始值设定项指定的初始化。 这对应于进入构造函数时立即执行的一系列赋值。 变量初始值设定项将按照它们在类型声明中出现的文本顺序执行。

下面的示例演示了一个带有初始化共享变量的共享构造函数的 @no__t 0 类：

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

每个封闭式泛型类型都存在单独的共享构造函数。 由于共享构造函数只对每个关闭类型执行一次，因此，在不能通过约束在编译时检查的类型参数上强制执行运行时检查是一个方便的位置。 例如，下面的类型使用共享构造函数来强制类型参数 @no__t 为-0 或 `Double`：

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

共享构造函数在运行时正好是依赖实现的，但如果已显式定义了一个共享构造函数，则会提供多个保证：

* 共享构造函数在第一次访问类型的任何静态字段之前运行。

* 共享构造函数在第一次调用类型的任何静态方法之前运行。

* 在第一次调用该类型的任何构造函数之前，将运行共享构造函数。

如果共享构造函数是为共享初始值设定项隐式创建的，则上述保证不适用于。 以下示例的输出是不确定的，因为未定义加载的准确顺序，因此不会定义执行共享构造函数：

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

输出可以是以下内容之一：

```console
Init A
A.F
Init B
B.F
```

或

```console
Init B
Init A
A.F
B.F
```

与此相反，以下示例生成可预测的输出。 请注意，尽管类 `B` 派生自类，但从不执行类的 @no__t @no__t 构造函数：

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

```console
Init B
B.G
```

还可以构造循环依赖关系，以便在其默认值状态中观察变量初始值设定项的 @no__t 0 变量，如以下示例中所示：

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

这会生成输出：

```console
X = 1, Y = 2
```

若要执行 `Main` 方法，系统首先加载类 `B`。 类 `B` 的 @no__t 构造函数将继续计算 `Y` 的初始值，这会以递归方式导致加载类 @no__t，因为引用了 `A.X` 的值。 类 `A` 的 @no__t 类构造函数将继续计算 @no__t 2 的初始值，而在执行此操作时，将获取 `Y` 的*默认*值，该值为零。 `A.X` 将初始化为 `1`。 然后，将 @no__t 加载完成后，返回到 `Y` 的初始值计算的结果，结果为 `2`。

如果有 `Main` 方法（而不是位于类 `A` 中），该示例将生成以下输出：

```console
X = 2, Y = 1
```

避免 `Shared` 变量初始值设定项中的循环引用，因为通常无法确定包含此类引用的类的加载顺序。

## <a name="events"></a>Events

事件用于向代码通知特定事件发生。 事件声明由标识符（委托类型或参数列表）和可选 `Implements` 子句组成。

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

如果指定了委托类型，则委托类型不能有返回类型。 如果指定了参数列表，则它不能包含 `Optional` 或 `ParamArray` 参数。 参数类型和/或委托类型的可访问域必须与事件本身的可访问性域相同或超集。 可以通过指定 `Shared` 修饰符来共享事件。

除了添加到类型声明空间的成员名称外，事件声明还会隐式声明多个其他成员。 假设有一个名为 `X` 的事件，则会将以下成员添加到声明空间：

* 如果声明的形式为方法声明，则会引入一个名为 `XEventHandler` 的嵌套委托类。 嵌套的委托类与方法声明匹配，并且具有与事件相同的可访问性。 参数列表中的特性适用于委托类的参数。

* 类型化为委托的 @no__t 0 实例变量，名为 `XEvent`。

* 名为 @no__t 的两个方法不能调用、重写或重载 `remove_X`。

如果某个类型尝试声明与上述某个名称匹配的名称，则会导致编译时错误，并将忽略隐式 `add_X` 和 `remove_X` 声明，以便进行名称绑定。 不能重写或重载任何引入的成员，但可以在派生类型中隐藏它们。 例如，类声明

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

等效于以下声明

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

在不指定委托类型的情况下声明事件是最简单且最简洁的语法，但具有为每个事件声明新委托类型的缺点。 例如，在下面的示例中，将创建三个隐藏的委托类型，即使所有三个事件都具有相同的参数列表：

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

在下面的示例中，事件只使用同一个委托，`EventHandler`：

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

可以通过以下两种方式之一来处理事件：静态或动态。 静态处理事件更简单，只需要 `WithEvents` 变量和 `Handles` 子句。 在下面的示例中，类 `Form1` 静态处理对象 `Button` 的事件 @no__t：

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

动态处理事件更复杂，因为必须显式连接事件，并在代码中断开与的连接。 语句 @no__t 为事件添加处理程序，语句 `RemoveHandler` 删除事件的处理程序。 下一个示例显示了一个类 `Form1`，它将 @no__t 添加为 @no__t 2 的 @no__t 事件的事件处理程序：

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

在方法 `Disconnect` 中，将删除事件处理程序。


### <a name="custom-events"></a>自定义事件

如上一节所述，事件声明隐式定义了一个字段、一个 @no__t 0 方法，以及一个用于跟踪事件处理程序的 @no__t 方法。 但在某些情况下，可能需要提供自定义代码来跟踪事件处理程序。 例如，如果某个类定义了只处理少量事件的40事件，则使用哈希表而不是40字段来跟踪每个事件的处理程序可能更高效。 *自定义事件*允许显式定义 `add_X` 和 `remove_X` 方法，这为事件处理程序启用自定义存储。

声明自定义事件的方式与指定委托类型的事件的声明方式相同，但关键字 `Custom` 必须在 `Event` 关键字之前。 自定义事件声明包含三个声明： `AddHandler` 声明、@no__t 声明和 @no__t 声明。 所有声明都不能具有任何修饰符，尽管它们可以具有属性。

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

@No__t，`RemoveHandler` 声明采用一个 @no__t 2 参数，该参数必须是该事件的委托类型。 当执行 `AddHandler` 或 @no__t 语句（或 `Handles` 子句自动处理事件）时，将调用相应的声明。 @No__t-0 声明采用与事件委托相同的参数，并将在执行 @no__t 1 语句时调用。 所有声明都必须提供并被视为子例程。

请注意，`AddHandler`、@no__t 和 `RaiseEvent` 声明在子例程具有的行位置上具有相同的限制。 开始语句、结束语句和块必须全部显示在逻辑行的开头。

除了添加到类型声明空间的成员名称外，自定义事件声明还会隐式声明多个其他成员。 假设有一个名为 `X` 的事件，则会将以下成员添加到声明空间：

* 一个名为 `add_X` 的方法，对应于 `AddHandler` 声明。

* 一个名为 `remove_X` 的方法，对应于 `RemoveHandler` 声明。

* 一个名为 `fire_X` 的方法，对应于 `RaiseEvent` 声明。

如果某个类型尝试声明一个与上述某个名称匹配的名称，则会导致编译时错误，并将忽略隐式声明，以便进行名称绑定。 不能重写或重载任何引入的成员，但可以在派生类型中隐藏它们。

__纪录.__ `Custom` 不是保留字。

#### <a name="custom-events-in-winrt-assemblies"></a>WinRT 程序集中的自定义事件

从 Microsoft Visual Basic 11.0 开始，在使用 @no__t 编译的文件中声明的事件，或在此类文件中的接口中声明的事件，并在其他位置进行了其他处理。

* 用于构建 winmd 的外部工具通常只允许某些委托类型，例如 `System.EventHandler(Of T)` 或 `System.TypedEventHandle(Of T, U)`，并且将禁止其他委托类型。

* @No__t-0 字段具有类型 `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)`，其中 @no__t 为委托类型。

* AddHandler 访问器返回 `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`，并且 RemoveHandler 访问器采用同一类型的单个参数。

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

*常量*是属于类型成员的常量值。

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

常量将被隐式共享。 如果声明包含 `As` 子句，子句将指定声明引入的成员类型。 如果省略该类型，则将推断常量的类型。 常数的类型只能是基元类型或 `Object`。 如果常量的类型为 `Object`，并且没有类型字符，则常数的实类型将是常量表达式的类型。 否则，常数的类型为常数类型字符的类型。

下面的示例演示一个名为 `Constants` 的类，该类具有两个公共常量：

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

可以通过类访问常量，如以下示例中所示，它输出 `Constants.A` 和 `Constants.B` 的值。

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

声明多个常量的常量声明等效于单个常量的多个声明。 下面的示例在一个声明语句中声明了三个常量。

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

此声明等效于以下内容：

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

常量类型的可访问域必须与常量本身的可访问域的可访问域相同或超集。 常数表达式必须产生常数的类型或可隐式转换为常量类型的类型的值。 常数表达式不能是循环的;也就是说，不能以本身来定义常量。

编译器会按适当的顺序自动计算常量声明。 在下面的示例中，编译器首先计算 `Y`，然后 `Z`，最后 `X`，分别生成值10、11和12。

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

如果需要常数值的符号名称，但是不允许在常数声明中使用该值的类型，或者如果不能在编译时通过常数表达式计算该值，则可以改为使用只读变量。


## <a name="instance-and-shared-variables"></a>实例和共享变量

实例或共享变量是可存储信息的类型的成员。

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

如果未指定任何修饰符，则必须指定 `Dim` 修饰符，否则可省略。 单个变量声明可能包含多个变量声明符;每个变量声明符引入一个新的实例或共享成员。

如果指定初始值设定项，则变量声明符只能声明一个实例或共享变量：

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

使用 `Shared` 修饰符声明的变量是*共享变量*。 共享变量只标识一个存储位置，而不考虑创建的类型实例数。 当程序开始执行时，共享变量就会存在，在程序终止时停止存在。

共享变量仅在特定封闭式泛型类型的实例之间共享。 例如，程序：

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

打印输出：

```console
1
1
2
```

在没有 `Shared` 修饰符的情况下声明的变量称为*实例变量*。 类的每个实例都包含该类的所有实例变量的单独副本。 当创建该类型的新实例时，引用类型的实例变量就会成为存在，如果没有对该实例的引用，并且已执行 `Finalize` 方法，则不再存在。 值类型的实例变量与它所属的变量具有完全相同的生存期。 换言之，当某个值类型的变量变为存在或不存在时，将执行值类型的实例变量。

如果声明符包含 `As` 子句，子句将指定声明引入的成员类型。 如果省略该类型并使用了严格语义，则会发生编译时错误。 否则，成员的类型将隐式 `Object` 或成员类型字符的类型。

__纪录.__ 语法中没有歧义：如果声明符省略了类型，它将始终使用以下声明符的类型。

实例或共享变量的类型或数组元素类型的可访问域必须与实例或共享变量本身的可访问域的可访问域相同或超集。

下面的示例演示了一个 @no__t 0 类，该类具有名为 `redPart`、@no__t 和 @no__t 的内部实例变量：

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

当某个实例或共享变量声明包含 `ReadOnly` 修饰符时，对声明引入的变量的赋值只能作为声明的一部分出现，或者出现在同一类的构造函数中。 具体而言，在以下情况下，只允许对只读实例或共享变量进行赋值：

* 在引入实例或共享变量的变量声明中（通过在声明中包含变量初始值设定项）。

* 对于实例变量，在包含变量声明的类的实例构造函数中。 只能以非限定的方式或通过 `Me` 或 `MyClass` 来访问实例变量。

* 对于共享变量，在包含共享变量声明的类的共享构造函数中。

如果需要常数值的符号名称，但不允许在常数声明中使用该类型的值，或者不能通过常量表达式在编译时计算该值，则共享只读变量非常有用。

下面是第一个这样的应用程序的示例，其中，颜色共享变量 @no__t 声明为-0，以防止其他程序更改它们：

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

常量和只读共享变量具有不同的语义。 当表达式引用常量时，将在编译时获取常量的值，但当表达式引用只读共享变量时，在运行时之前不会获取共享变量的值。 请考虑以下应用程序，其中包含两个单独的程序。

file1：

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

file2：

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

命名空间 `Program1`，`Program2` 表示单独编译两个程序。 由于变量 `Program1.Utils.X` 声明为 `Shared ReadOnly`，因此在编译时，`Console.WriteLine` 语句输出的值是未知的，而是在运行时获取。 因此，如果更改了 `X` 的值并重新编译了 `Program1`，则即使未重新编译 `Console.WriteLine` 语句，@no__t 也将输出新值。 但是，如果 @no__t 为常量，则在编译 `Program2` 时，将获得 `X` 的值，并且在重新编译 @no__t 之前，将不会影响 `Program1` 中的更改。

### <a name="withevents-variables"></a>WithEvents 变量

类型可以声明它处理通过使用 `WithEvents` 修饰符引发事件的实例或共享变量来处理其实例或共享变量之一所引发的一组事件。 例如：

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

在此示例中，方法 `E1Handler` 处理事件 `E1`，该事件由存储在实例变量 @no__t 中的类型 `Raiser` 的实例引发。

@No__t 的修饰符导致使用前导下划线重命名变量，并将其替换为与事件挂钩同名的属性。 例如，如果变量的名称为 `F`，则将其重命名为 `_F`，并且隐式声明一个属性 `F`。 如果变量的新名称和其他声明之间存在冲突，则将报告编译时错误。 应用到变量的任何属性都将传送到已重命名的变量。

@No__t 声明创建的隐式属性负责挂钩和解除相关的事件处理程序。 将值分配给变量时，属性将首先对变量中当前实例上的事件调用 @no__t 的方法（如果有），则为解除。 接下来，将进行分配，并且属性将为变量中的新实例上的事件调用 @no__t 的方法（正在挂钩新的事件处理程序）。 下面的代码等效于标准模块 @no__t 的代码-0：

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

如果将实例或共享变量声明为结构，则将该变量声明为 `WithEvents` 是无效的。 此外，不能在结构中指定 `WithEvents`，并且不能将 `ReadOnly` 与 @no__t 组合。

### <a name="variable-initializers"></a>变量初始值设定项

结构中的类和实例变量声明（而不是共享变量声明）中的实例和共享变量声明可能包含变量初始值设定项。 对于 @no__t 0 变量，变量初始值设定项对应于在程序开始后、第一次引用第一次引用 @no__t 之前执行的赋值语句。 对于实例变量，变量初始值设定项对应于创建类的实例时执行的赋值语句。 结构不能具有实例变量初始值设定项，因为无法修改其无参数构造函数。

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

```console
x = 1.4142135623731, i = 100, s = Hello
```

在加载类时，将分配到 `x`，并在创建类的新实例时，将分配给 `i` 和 `s`。

将变量初始值设定项视为自动插入到类型构造函数的块中的赋值语句是非常有用的。 下面的示例包含多个实例变量初始值设定项。

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

该示例与下面显示的代码相对应，其中每个注释指示一个自动插入的语句。

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

在执行任何变量初始值设定项之前，所有变量都将初始化为其类型的默认值。 例如：

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

由于在类加载时 `b` 会自动初始化为其默认值，因此在创建类的实例时，`i` 会自动初始化为其默认值，前面的代码生成以下输出：

```console
b = False, i = 0
```

每个变量初始值设定项必须生成变量类型或可隐式转换为变量类型的类型的值。 变量初始值设定项可以是循环的，也可以引用将在其后面进行初始化的变量（在这种情况下，被引用变量的值为初始值设定项的默认值）。 此类初始值设定项为可疑值。

有三种形式的变量初始值设定项：常规初始值设定项、数组大小初始值设定项和对象初始值设定项。 前两个窗体出现在类型名称后的等号之后，后者是声明自身的一部分。 对于任何特定声明，只能使用一种形式的初始值设定项。

#### <a name="regular-initializers"></a>正则表达式

*正则初始值设定项*是可隐式转换为变量类型的表达式。 它出现在类型名称后的等号之后，并且必须分类为值。 例如：

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

此程序生成以下输出：

```console
x = 10, y = 20
```

如果变量声明有一个规则初始值设定项，则一次只能声明一个变量。 例如：

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

*对象初始值设定项*是使用对象创建表达式在类型名称的位置指定的。 对象初始值设定项等效于将对象创建表达式的结果分配给变量的常规初始值设定项。 因此，

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

对象初始值设定项中的括号始终解释为构造函数的参数列表，而不是数组类型修饰符。 使用对象初始值设定项的变量名称不能具有数组类型修饰符或可以为 null 的类型修饰符。

#### <a name="array-size-initializers"></a>数组大小初始值设定项

*数组大小初始值设定项*是变量名称的修饰符，它提供一组由表达式表示的维度上限。

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

上限表达式必须分类为值，并且必须能够隐式转换为 `Integer`。 上限集等效于具有给定上限的数组创建表达式的变量初始值设定项。 数组类型的维数是从数组大小初始值设定项推断出来的。 因此，

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

所有上限必须等于或大于-1，并且所有维度都必须具有指定的上限。 如果要初始化的数组的元素类型本身是数组类型，则数组类型修饰符将会跳到数组大小初始值设定项的右侧。 例如

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

声明一个局部变量 `x`，该变量的类型是一维数组，该数组的类型为三 @no__t 维数组，其类型为第一个维度中 `0..5` @no__t 和第二个维度中的-2 的数组。 不能使用数组大小初始值设定项来初始化其类型为数组的数组的变量元素。

具有数组大小初始值设定项的变量声明不能对其类型或常规初始值设定项使用数组类型修饰符。


### <a name="systemmarshalbyrefobject-classes"></a>MarshalByRefObject 类

派生自类 `System.MarshalByRefObject` 的类将使用代理（即按引用）在上下文边界之间进行封送，而不是通过复制（即通过值）进行封送处理。 这意味着此类类的实例可能不是真正的实例，而可能只是在上下文边界封送变量访问和方法调用的存根。

因此，不能创建对此类类中定义的变量的存储位置的引用。 这意味着，不能将类型化为派生自 `System.MarshalByRefObject` 的类的变量传递到引用参数，也不能访问类型化为值类型的变量的方法和变量。 相反，Visual Basic 将此类类上定义的变量视为属性（因为这些限制在属性上是相同的）。

此规则有一个例外：使用 @no__t 的隐式或显式限定的成员不受上述限制，因为 `Me` 始终保证是实际的对象，而不是代理。

## <a name="properties"></a>属性

*属性*是变量的自然扩展;两者都是具有关联类型的命名成员，并且用于访问变量和属性的语法相同。 但与变量不同，属性不表示存储位置。 相反，属性具有*访问器*，它指定要执行的语句，以便读取或写入其值。

属性通过属性声明进行定义。 属性声明的第一部分类似于字段声明。 第二部分包括 @no__t 的访问器和/或 @no__t 访问器。

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

在下面的示例中，`Button` 类定义 @no__t 1 属性。

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

根据上面的 `Button` 类，以下是使用 @no__t 属性的示例：

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

此处，通过将值分配给属性来调用 `Set` 访问器，通过引用表达式中的属性来调用 @no__t 1 访问器。

如果没有为属性指定类型，并且使用的是严格语义，则会发生编译时错误;否则，属性的类型将隐式 `Object` 或属性类型字符的类型。 属性声明可以包含 `Get` 取值函数，该访问器用于检索属性的值、@no__t 1 访问器（存储属性的值）或同时用于这两者。 由于属性隐式声明方法，因此可以使用与方法相同的修饰符来声明属性。 如果在接口中定义该属性或使用 `MustOverride` 修饰符定义该属性，则必须省略属性体和 @no__t 构造;否则，会发生编译时错误。

索引参数列表构成属性的签名，因此属性可能会在索引参数上重载，而不能在属性的类型上重载。 索引参数列表与常规方法相同。 但是，不能用 `ByRef` 修饰符修改任何参数，也不能将任何参数命名为 `Value` （在 `Set` 访问器中为隐式值参数保留）。

可按如下所示声明属性：

* 如果属性未指定属性类型修饰符，则该属性必须同时具有 `Get` 访问器和 @no__t。 此属性被称为读写属性。

* 如果该属性指定 `ReadOnly` 修饰符，则该属性必须具有 @no__t 访问器，并且不能具有 @no__t 2 访问器。 此属性被称为只读属性。 只读属性是赋值的目标时，这是一个编译时错误。

* 如果该属性指定 `WriteOnly` 修饰符，则该属性必须具有 @no__t 访问器，并且不能具有 @no__t 2 访问器。 此属性被称为只写属性。 在表达式中引用只写属性（作为赋值的目标或作为方法的参数）时，会发生编译时错误。

属性的 @no__t 和 @no__t 访问器不是不同的成员，并且不能单独声明属性的访问器。 下面的示例未声明单个读写属性。 相反，它声明了两个具有相同名称的属性，一个为只读，一个只写：

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

因为在同一个类中声明的两个成员不能具有相同的名称，所以该示例会导致编译时错误。

默认情况下，属性的 `Get` 和 `Set` 访问器的可访问性与属性本身的可访问性相同。 但 @no__t @no__t 的访问器也可以从属性单独指定可访问性。 在这种情况下，访问器的可访问性必须比属性的可访问性更严格，而且只有一个访问器可以具有与属性不同的可访问性级别。 访问类型被视为更多或更少的限制，如下所示：

* `Private` 比 `Public`、`Protected Friend`、`Protected` 或 @no__t 的限制更严格。

* `Friend` 比 @no__t 或 @no__t 的限制更严格。

* `Protected` 比 @no__t 或 @no__t 的限制更严格。

* `Protected Friend` 比 `Public` 的限制更强。

当属性的访问器之一可访问，而另一个访问器不是可访问的时，该属性将视为只读或只写。 例如：

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

当派生类型隐藏属性时，派生属性将隐藏与读取和写入相关的隐藏属性。 在下面的示例中，`B` 中的 `P` 属性隐藏 `A` 中与读取和写入有关的 `P` 属性：

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

返回类型或参数类型的可访问域必须与属性本身的可访问域的可访问域相同或超集。 一个属性只能有一个 `Set` 访问器和一个 @no__t 的访问器。

除了声明和调用语法中的差异外，`Overridable`、`NotOverridable`、`Overrides`、`MustOverride` 和 @no__t 属性的行为与 `Overridable`、`NotOverridable`、`Overrides`、`MustOverride` 和 @no__t 9 方法完全相同。 当重写属性时，重写属性的类型必须相同（读写、只读、只写）。 @No__t-0 属性不能包含 @no__t 1 访问器。

在下面的示例中 `X` 是一个 @no__t 为只读属性，`Y` 是一个 @no__t 的读写属性，而 @no__t 为 @no__t 读写属性。

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

因为 `Z` @no__t 为-1，所以必须将包含类 `A` @no__t 声明为3。

与此相反，派生自类 `A` 的类如下所示：

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

此处，属性的声明 `X`、`Y` 和 `Z` 替代基属性。 每个属性声明都与相应的继承属性的可访问性修饰符、类型和名称完全匹配。 属性 @no__t 的 @no__t 取值函数为-1，属性的 `Set` 访问器 `Y` 使用 @no__t 关键字访问继承的属性。 属性 @no__t 的声明会重写 `MustOverride` 属性，因此，在类 `B` 中没有未完成的 `MustOverride` 成员，并且 @no__t 允许成为一个常规类。

在第一次引用资源之前，可以使用属性来延迟资源的初始化。 例如：

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

@No__t-0 类包含三个属性，分别分别表示标准输入、输出和错误设备，分别为 `In`、@no__t 和 @no__t。 通过将这些成员作为属性公开，@no__t 0 类可以将其初始化延迟到实际使用。 例如，在第一次引用 `Out` 属性（如 `ConsoleStreams.Out.WriteLine("hello, world")` 中）时，将初始化输出设备的基础 `TextWriter`。 但如果应用程序不引用 `In` 和 `Error` 属性，则不会为这些设备创建任何对象。


### <a name="get-accessor-declarations"></a>获取访问器声明

@No__t-0 取值函数（getter）是通过使用属性 `Get` 声明来声明的。 属性 `Get` 声明由关键字 `Get` 后跟语句块组成。 给定一个名为 `P` 的属性，@no__t 1 访问器声明隐式声明了名称 @no__t 为-2 且具有相同修饰符、类型和参数列表的方法作为属性。 如果该类型包含具有该名称的声明，则会生成编译时错误，但会忽略隐式声明以便进行名称绑定。

在 `Get` 访问器主体的声明空间中隐式声明的、与属性同名的特殊局部变量表示属性的返回值。 在表达式中使用时，局部变量具有特殊名称解析语义。 如果本地变量用于需要归类为方法组的表达式的上下文中（如调用表达式），则名称解析为函数而不是局部变量。 例如：

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

使用括号可能导致不明确的情况（例如 `F(1)`，其中 @no__t 为类型为一维数组的属性）。 在所有不明确的情况下，名称解析为属性而不是局部变量。 例如：

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

当控制流离开 `Get` 访问器主体时，本地变量的值将传递回调用表达式。 由于调用 @no__t 0 取值函数在概念上等同于读取变量的值，因此，`Get` 访问器的编程样式被视为具有明显副作用的错误编程方式，如以下示例中所示：

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

@No__t-0 属性的值取决于属性以前被访问的次数。 因此，访问属性会产生一个可观察到的副作用，而该属性应改为作为方法实现。

@No__t-0 访问器的 "无副作用" 约定并不意味着应始终编写 @no__t 的访问器，以便只返回存储在变量中的值。 实际上，`Get` 访问器通过访问多个变量或调用方法来计算属性的值。 但是，正确设计的 `Get` 访问器不会执行任何操作，从而导致对象的状态发生变化。

__纪录.__ `Get` 访问器在子例程具有的行位置上具有相同的限制。 开始语句、结束语句和块必须全部显示在逻辑行的开头。

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a>设置访问器声明

使用属性集声明声明 @no__t 0 取值函数（setter）。 属性集声明由关键字 `Set`、可选参数列表和语句块组成。 给定一个名为 `P` 的属性，setter 声明将使用与属性相同的修饰符和参数列表隐式声明名为 `set_P` 的方法。 如果该类型包含具有该名称的声明，则会生成编译时错误，但会忽略隐式声明以便进行名称绑定。

如果指定了参数列表，则它必须具有一个成员，该成员必须不包含除 `ByVal` 以外的任何修饰符，并且其类型必须与属性的类型相同。 参数表示要设置的属性值。 如果省略该参数，则将隐式声明一个名为 `Value` 的参数。

__纪录.__ `Set` 访问器在子例程具有的行位置上具有相同的限制。 开始语句、结束语句和块必须全部显示在逻辑行的开头。

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a>默认属性

指定修饰符 @no__t 的属性称为 "*默认属性*"。 允许属性的任何类型都可以具有默认属性，包括接口。 可以引用默认属性，而无需用属性的名称限定该实例。 因此，给定一个类

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

一旦将某个属性声明 `Default`，在继承层次结构中该名称上重载的所有属性都将成为默认属性，无论它们是否已声明为 @no__t。 如果基类通过其他名称声明了默认属性，则在派生类中声明属性 `Default` 不需要任何其他修饰符，如 `Shadows` 或 `Overrides`。 这是因为默认属性没有标识或签名，因此不能被隐藏或重载。 例如：

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

此程序将生成输出：

```console
MoreDerived = 10
Derived = 10
Base = 10
```

类型中声明的所有默认属性都必须具有相同的名称，为清楚起见，必须指定 `Default` 修饰符。 由于没有索引参数的默认属性会导致在分配包含类的实例时出现不明确的情况，因此默认属性必须具有索引参数。 此外，如果在特定名称上重载的一个属性包含 `Default` 修饰符，则在该名称上重载的所有属性都必须指定它。 默认属性不能 `Shared`，并且属性的至少一个取值函数不得为-1 @no__t。

### <a name="automatically-implemented-properties"></a>自动实现的属性

如果属性省略了任何访问器的声明，则将自动提供属性的实现，除非该属性是在接口中声明的，或者声明为 `MustOverride`。 只有没有参数的读/写属性才能自动实现;否则，会发生编译时错误。

自动实现的属性 `x`，甚至一个重写另一个属性，引入了与属性具有相同类型的专用局部变量 `_x`。 如果局部变量名称和其他声明之间存在冲突，则将报告编译时错误。 自动实现的属性的 `Get` 访问器返回设置本地的值的本地和属性的 `Set` 访问器的值。 例如，声明：

```vb
Public Property x() As Integer
```

大致等效于：

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

与变量声明一样，自动实现的属性可以包含初始值设定项。 例如：

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

__纪录.__ 当初始化自动实现的属性时，它将通过属性而不是基础字段进行初始化。 这是因为，如果需要，重写属性可以截获初始化。

自动实现的属性上允许使用数组初始值设定项，但无法显式指定数组界限。  例如：

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a>迭代器属性

*Iterator 属性*是具有 `Iterator` 修饰符的属性。 使用迭代器方法（部分[迭代器方法](statements.md#iterator-methods)）的相同原因是使用它作为生成序列的一种简便方法，该方法可由 `For Each` 语句使用。 迭代器属性的 `Get` 访问器的解释方式与迭代器方法相同。

迭代器属性必须具有显式 @no__t 0 取值函数，并且它的类型必须为 `IEnumerator` 或 `IEnumerable`，或者对于某些 @no__t 为 @no__t 为 @no__t。

下面是迭代器属性的示例：

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

*运算符*是为包含类定义现有 Visual Basic 运算符的含义的方法。 当运算符应用到表达式中的类时，运算符将编译为对类中定义的运算符方法的调用。 为类定义运算符也称为*重载*运算符。

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

不能重载已经存在的运算符;实际上，这主要适用于转换运算符。 例如，不能重载从派生类到基类的转换：

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

运算符也可以在 word 的常见意义上重载：

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

运算符声明不会将名称显式添加到包含类型的声明空间;但是，它们隐式声明了一个以字符 "op_" 开头的相应方法。 以下部分列出了每个运算符对应的方法名称。

可以定义三类运算符：一元运算符、二元运算符和转换运算符。 所有运算符声明都具有某些限制：

* 运算符声明必须始终 `Public` 并 `Shared`。 可以在将采用修饰符的上下文中省略 `Public` 修饰符。

* 运算符的参数不能声明为 `ByRef`、`Optional` 或 `ParamArray`。

* 至少一个操作数或返回值的类型必须为包含运算符的类型。

* 没有为运算符定义函数返回变量。 因此，必须使用 `Return` 语句从运算符体返回值。

这些限制的唯一例外情况适用于可为 null 的值类型。 由于可以为 null 的值类型没有实际类型定义，因此值类型可以为类型的可以为 null 的版本声明用户定义的运算符。 在确定类型是否可以声明特定的用户定义运算符时，将从声明中涉及的所有类型中首次删除 @no__t 的修饰符，目的是进行有效性检查。 此 relaxation 不适用于 @no__t 0 和 `IsFalse` 运算符的返回类型;它们仍必须返回 `Boolean`，而不是 `Boolean?`。

运算符声明不能修改运算符的优先级和关联性。

__纪录.__ 对于子程序具有的行位置，运算符具有相同的限制。 开始语句、结束语句和块必须全部显示在逻辑行的开头。


### <a name="unary-operators"></a>一元运算符

可以重载以下一元运算符：

* 一元加运算符 `+` （对应的方法： `op_UnaryPlus`）

* 一元减号运算符 `-` （对应的方法： `op_UnaryNegation`）

* 逻辑 `Not` 运算符（对应的方法： `op_OnesComplement`）

* @No__t 0 和 `IsFalse` 运算符（相应的方法： `op_True`、`op_False`）

所有重载一元运算符都必须采用包含类型的单个参数，并可能返回任何类型，@no__t 0 和 `IsFalse` 除外，必须返回 `Boolean`。 如果包含类型是泛型类型，则类型参数必须与包含类型的类型参数匹配。 例如，应用于对象的

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

如果某一类型重载 `IsTrue` 或 `IsFalse` 中的一个，则它也必须重载另一个。 如果只有一个重载，则会产生编译时错误。

__纪录.__ `IsTrue` 和 @no__t 不是保留字。

### <a name="binary-operators"></a>二元运算符

可以重载以下二进制运算符：

* 加法 `+`、减法 `-`、乘法 `*`、除 `/`、整数除法 `\`、取 `Mod` 和求幂 `^` 运算符（相应方法： `op_Addition`、`op_Subtraction`、`op_Multiply`、0 @no__t_t-12，3）

* 关系运算符 `=`，`<>`，`<`，`>`，`<=`，`>=` （相应的方法： `op_Equality`，`op_Inequality`，`op_LessThan`，`op_GreaterThan`，0）。 __纪录.__ 尽管相等运算符可重载，但赋值运算符（仅用于赋值语句）无法重载。

* @No__t-0 运算符（对应的方法： `op_Like`）

* 串联运算符 `&` （对应的方法： `op_Concatenate`）

* 逻辑 `And`、`Or` 和 @no__t 2 运算符（相应的方法： `op_BitwiseAnd`、`op_BitwiseOr`、`op_ExclusiveOr`）

* 移位运算符 `<<` 并 `>>` （相应方法： `op_LeftShift`、`op_RightShift`）

所有重载的二元运算符都必须采用包含类型作为参数之一。 如果包含类型是泛型类型，则类型参数必须与包含类型的类型参数匹配。 移位运算符会进一步限制此规则，要求第一个参数为包含类型;第二个参数的类型必须始终为 `Integer`。

以下二元运算符必须成对声明：

* 运算符 `=`，运算符 `<>`

* 运算符 `>`，运算符 `<`

* 运算符 `>=`，运算符 `<=`

如果声明其中一个对，则另一个还必须使用匹配的参数和返回类型进行声明，否则将导致编译时错误。 （__注意：__ 要求成对的关系运算符声明的目的是尝试确保在重载运算符中至少具有最低级别的逻辑一致性。）

与关系运算符不同，强烈不建议同时重载除法运算符和整数除法运算符，尽管不是错误。 （__注意：__ 通常，两种类型的除法应完全不同：支持除法运算的类型是整型（在这种情况下，它应支持 `\`），而不是（在这种情况下，它应支持 `/`）。 我们认为定义这两个运算符是错误的，但由于其语言通常不能区分 Visual Basic 的两种划分方式，因此，我们认为这种做法是最安全的方法，但这种做法非常反对。）

不能直接重载复合赋值运算符。 相反，当重载相应的二元运算符时，复合赋值运算符将使用重载运算符。 例如：

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

转换运算符定义类型之间的新转换。 这些新转换称为*用户定义的转换*。 转换运算符从转换运算符的参数类型指示的源类型转换为目标类型，由转换运算符的返回类型指示。 转换必须分类为扩大或收缩。 包含 @no__t 关键字的转换运算符声明引入了用户定义的扩大转换（对应的方法： `op_Implicit`）。 包含 @no__t 关键字的转换运算符声明引入了用户定义的收缩转换（对应的方法： `op_Explicit`）。

通常，用户定义的扩大转换应设计为绝不会引发异常，并且永远不会丢失信息。 如果用户定义的转换可能会导致异常（例如，因为 source 参数超出范围）或丢失信息（如丢弃高阶位），则应将该转换定义为收缩转换。 在下面的示例中：

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

从 `Digit` 到 @no__t 的转换是一种扩大转换，因为它从不引发异常或丢失信息，但从 `Byte` 到 `Digit` 的转换是收缩转换，因为 `Digit` 只能表示的可能值的子集`Byte`。

与可重载的所有其他类型成员不同，转换运算符的签名包含转换的目标类型。 这是返回类型在签名中参与的唯一类型成员。 但是，转换运算符的扩大或收缩分类不是运算符签名的一部分。 因此，类或结构不能同时声明扩大转换运算符和具有相同源和目标类型的收缩转换运算符。

用户定义的转换运算符必须转换为包含类型或从包含类型转换--例如，类可能 `C` 定义从 @no__t 到 `Integer` 的转换和从 @no__t 3 到 @no__t 的转换，而不是从 @no__t 到 @no__t。 如果包含类型是泛型类型，则类型参数必须与包含类型的类型参数匹配。 此外，不能重新定义内部函数（即非用户定义的）转换。 因此，类型不能声明转换，其中：

* 源类型和目标类型是相同的。

* 源类型和目标类型都不是定义转换运算符的类型。

* 源类型或目标类型是接口类型。

* 源类型和目标类型通过继承（包括 `Object`）相关。

这些规则的唯一例外情况适用于可为 null 的值类型。 由于可以为 null 的值类型没有实际类型定义，因此值类型可以为类型的可以为 null 的版本声明用户定义的转换。 在确定类型是否可以声明特定的用户定义的转换时，将从声明中涉及的所有类型中第一次删除 @no__t 的修饰符，目的是进行有效性检查。 因此，以下声明是有效的，因为 `S` 可以定义从 @no__t 到 `T` 的转换：

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

但是，以下声明无效，因为结构 `S` 不能定义从 @no__t 到 `S` 的转换：

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a>运算符映射

由于 Visual Basic 支持的运算符集可能与 .NET Framework 上其他语言的运算符集不完全匹配，因此，在定义或使用某些运算符时，某些运算符将专门映射到其他运算符。 尤其是在下列情况下：

* 定义整数除法运算符将自动定义将调用整数除法运算符的常规除法运算符（仅可用于其他语言）。

* 重载 `Not`、`And` 和 @no__t 2 运算符将仅从区分逻辑与按位运算符的其他语言的角度重载位运算符。

* 一个类，它仅重载一种语言中的逻辑运算符，该运算符区分逻辑运算符和位运算符（即，使用 `op_LogicalNot`、`op_LogicalAnd` 的语言，以及分别为 `Not`、`And` 和 @no__t 的 `op_LogicalOr`）将具有逻辑映射到 Visual Basic 逻辑运算符上的运算符。 如果同时重载逻辑运算符和按位运算符，则只使用位运算符。

* 重载 `<<` 和 @no__t 1 运算符将仅从区分符号和无符号移位运算符的其他语言的角度重载签名运算符。

* 仅重载无符号移位运算符的类将有一个映射到相应 Visual Basic 移位运算符的无符号移位运算符。 如果未签名和带符号的移位运算符都重载，则只使用带符号的移位运算符。
