---
ms.openlocfilehash: 3d5c1e90283b6d6ec8cdeccd35e32c78f997cc27
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306038"
---
# <a name="type-members"></a><span data-ttu-id="3eb9c-101">类型成员</span><span class="sxs-lookup"><span data-stu-id="3eb9c-101">Type Members</span></span>

<span data-ttu-id="3eb9c-102">类型成员定义存储位置和可执行代码。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-102">Type members define storage locations and executable code.</span></span> <span data-ttu-id="3eb9c-103">它们可以是方法、构造函数、事件、常量、变量和属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-103">They can be methods, constructors, events, constants, variables, and properties.</span></span>

## <a name="interface-method-implementation"></a><span data-ttu-id="3eb9c-104">接口方法实现</span><span class="sxs-lookup"><span data-stu-id="3eb9c-104">Interface Method Implementation</span></span>

<span data-ttu-id="3eb9c-105">方法、事件和属性可以实现接口成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-105">Methods, events, and properties can implement interface members.</span></span> <span data-ttu-id="3eb9c-106">若要实现接口成员，成员声明将指定 @no__t 的关键字，并列出一个或多个接口成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-106">To implement an interface member, a member declaration specifies the `Implements` keyword and lists one or more interface members.</span></span>

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

<span data-ttu-id="3eb9c-107">实现接口成员的方法和属性将隐式 `NotOverridable`，除非声明为 `MustOverride`、`Overridable` 或重写另一个成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-107">Methods and properties that implement interface members are implicitly `NotOverridable` unless declared to be `MustOverride`, `Overridable`, or overriding another member.</span></span> <span data-ttu-id="3eb9c-108">实现接口成员的成员应 `Shared`，这是错误的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-108">It is an error for a member implementing an interface member to be `Shared`.</span></span> <span data-ttu-id="3eb9c-109">成员的可访问性不会影响其实现接口成员的能力。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-109">A member's accessibility has no effect on its ability to implement interface members.</span></span>

<span data-ttu-id="3eb9c-110">要使接口实现有效，包含类型的实现列表必须命名包含兼容成员的接口。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-110">For an interface implementation to be valid, the implements list of the containing type must name an interface that contains a compatible member.</span></span> <span data-ttu-id="3eb9c-111">兼容成员是其签名与实现成员的签名相匹配的成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-111">A compatible member is one whose signature matches the signature of the implementing member.</span></span> <span data-ttu-id="3eb9c-112">如果正在实现泛型接口，则在检查兼容性时，实现子句中提供的类型参数会被替换为签名。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-112">If a generic interface is being implemented, then the type argument supplied in the Implements clause is substituted into the signature when checking compatibility.</span></span> <span data-ttu-id="3eb9c-113">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-113">For example:</span></span>

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

<span data-ttu-id="3eb9c-114">如果使用委托类型声明的事件是实现接口事件，则兼容事件是其基础委托类型为同一类型的事件之一。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-114">If an event declared using a delegate type is implementing an interface event, then a compatible event is one whose underlying delegate type is the same type.</span></span> <span data-ttu-id="3eb9c-115">否则，该事件将使用它所实现的接口事件中的委托类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-115">Otherwise, the event uses the delegate type from the interface event it is implementing.</span></span> <span data-ttu-id="3eb9c-116">如果此类事件实现了多个接口事件，则所有接口事件必须具有相同的基础委托类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-116">If such an event implements multiple interface events, all the interface events must have the same underlying delegate type.</span></span> <span data-ttu-id="3eb9c-117">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-117">For example:</span></span>

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

<span data-ttu-id="3eb9c-118">使用类型名称、句点和标识符指定实现列表中的接口成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-118">An interface member in the implements list is specified using a type name, a period, and an identifier.</span></span> <span data-ttu-id="3eb9c-119">类型名称必须是实现列表中的接口或实现列表中接口的基接口，并且标识符必须是指定接口的成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-119">The type name must be an interface in the implements list or a base interface of an interface in the implements list, and the identifier must be a member of the specified interface.</span></span> <span data-ttu-id="3eb9c-120">单个成员可以实现一个以上的匹配接口成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-120">A single member can implement more than one matching interface member.</span></span>

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

<span data-ttu-id="3eb9c-121">如果实现的接口成员在所有显式实现的接口中都不可用，因为有多个接口继承，则实现成员必须显式引用可用于该成员的基接口。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-121">If the interface member being implemented is unavailable in all explicitly implemented interfaces because of multiple interface inheritance, the implementing member must explicitly reference a base interface on which the member is available.</span></span> <span data-ttu-id="3eb9c-122">例如，如果 @no__t 0，`I2` 包含成员 `M`，而 `I3` 从 @no__t 和 @no__t 继承，则实现 @no__t 的类型将实现 @no__t 7 和 @no__t。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-122">For example, if `I1` and `I2` contain a member `M`, and `I3` inherits from `I1` and `I2`, a type implementing `I3` will implement `I1.M` and `I2.M`.</span></span> <span data-ttu-id="3eb9c-123">如果接口隐藏了继承的成员，则实现类型将必须实现继承成员，并且成员将隐藏它们。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-123">If an interface shadows multiply inherited members, an implementing type will have to implement the inherited members and the member(s) shadowing them.</span></span>

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

<span data-ttu-id="3eb9c-124">如果实现的接口成员的包含接口是泛型的，则必须提供与要实现的接口相同的类型参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-124">If the containing interface of the interface member be implemented is generic, the same type arguments as the interface being implements must be supplied.</span></span> <span data-ttu-id="3eb9c-125">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-125">For example:</span></span>

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


## <a name="methods"></a><span data-ttu-id="3eb9c-126">方法</span><span class="sxs-lookup"><span data-stu-id="3eb9c-126">Methods</span></span>

<span data-ttu-id="3eb9c-127">方法包含程序的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-127">Methods contain the executable statements of a program.</span></span>

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

<span data-ttu-id="3eb9c-128">具有可选参数列表和可选返回值的方法是 "共享" 或 "不共享"。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-128">Methods, which have an optional list of parameters and an optional return value, are either shared or not shared.</span></span> <span data-ttu-id="3eb9c-129">通过类或类的实例访问共享方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-129">Shared methods are accessed through the class or instances of the class.</span></span> <span data-ttu-id="3eb9c-130">非共享方法（也称为实例方法）通过类的实例进行访问。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-130">Non-shared methods, also called instance methods, are accessed through instances of the class.</span></span> <span data-ttu-id="3eb9c-131">下面的示例显示了一个类 `Stack`，其中包含多个共享方法（`Clone` 和 `Flip`）以及若干实例方法（`Push`、`Pop` 和 `ToString`）：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-131">The following example shows a class `Stack` that has several shared methods (`Clone` and `Flip`), and several instance methods (`Push`, `Pop`, and `ToString`):</span></span>

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

<span data-ttu-id="3eb9c-132">可以重载方法，这意味着多个方法可能具有相同的名称，只要它们具有唯一的签名。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-132">Methods can be overloaded, which means that multiple methods may have the same name so long as they have unique signatures.</span></span> <span data-ttu-id="3eb9c-133">方法的签名包含其参数的数目和类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-133">The signature of a method consists of the number and types of its parameters.</span></span> <span data-ttu-id="3eb9c-134">具体而言，方法的签名不包括返回类型或参数修饰符，如 Optional、ByRef 或 ParamArray。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-134">The signature of a method specifically does not include the return type or parameter modifiers such as Optional, ByRef or ParamArray.</span></span> <span data-ttu-id="3eb9c-135">下面的示例演示一个具有若干重载的类：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-135">The following example shows a class with a number of overloads:</span></span>

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

<span data-ttu-id="3eb9c-136">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-136">The output of the program is:</span></span>

```console
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

<span data-ttu-id="3eb9c-137">仅可选参数不同的重载可用于库的 "版本控制"。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-137">Overloads that differ only in optional parameters can be used for "versioning" of libraries.</span></span> <span data-ttu-id="3eb9c-138">例如，库的 v1 可能包含带可选参数的函数：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-138">For instance, v1 of a library might include a function with optional parameters:</span></span>

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

<span data-ttu-id="3eb9c-139">然后，该库的 v2 要添加另一个可选参数 "password"，它想要这样做，而不会破坏源兼容性（因此可以重新编译用于目标 v1 的应用程序），并且不会中断二进制兼容性（因此，应用程序用于参考 v1 现在可以引用 v2 而不进行重新编译。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-139">Then v2 of the library wants to add another optional parameter "password", and it wants to do so without breaking source compatibility (so applications that used to target v1 can be recompiled), and without breaking binary compatibility (so applications that used to reference v1 can now reference v2 without recompilation).</span></span> <span data-ttu-id="3eb9c-140">这就是 v2 的外观：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-140">This is how v2 will look:</span></span>

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

<span data-ttu-id="3eb9c-141">请注意，公共 API 中的可选参数不符合 CLS。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-141">Note that optional parameters in a public API are not CLS-compliant.</span></span> <span data-ttu-id="3eb9c-142">但是，它们至少可以由 Visual Basic 和 c # 4 和F#使用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-142">However, they can be consumed at least by Visual Basic and C#4 and F#.</span></span>



### <a name="regular-async-and-iterator-method-declarations"></a><span data-ttu-id="3eb9c-143">常规、异步和迭代器方法声明</span><span class="sxs-lookup"><span data-stu-id="3eb9c-143">Regular, Async and Iterator Method Declarations</span></span>

<span data-ttu-id="3eb9c-144">有两种类型的*方法：不*返回值的方法和*函数*。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-144">There are two types of methods: *subroutines*, which do not return values, and *functions*, which do.</span></span> <span data-ttu-id="3eb9c-145">如果在接口中定义了方法或具有 @no__t 的修饰符，则只能省略方法的 body 和 `End` 构造。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-145">The body and `End` construct of a method may only be omitted if the method is defined in an interface or has the `MustOverride` modifier.</span></span> <span data-ttu-id="3eb9c-146">如果未在函数上指定返回类型，并且使用的是严格语义，则会发生编译时错误;否则，该类型将隐式 `Object` 或该方法类型字符的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-146">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs; otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="3eb9c-147">方法的返回类型和参数类型的可访问域必须与方法本身的可访问域的可访问域相同或超集。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-147">The accessibility domain of the return type and parameter types of a method must be the same as or a superset of the accessibility domain of the method itself.</span></span>

<span data-ttu-id="3eb9c-148">__常规方法__是不 `Async` 和 `Iterator` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-148">A __regular method__ is one with neither `Async` nor `Iterator` modifiers.</span></span> <span data-ttu-id="3eb9c-149">它可以是子例程或函数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-149">It may be a subroutine or a function.</span></span> <span data-ttu-id="3eb9c-150">部分[常规方法](statements.md#regular-methods)详细说明了在调用常规方法时会发生的情况。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-150">Section [Regular Methods](statements.md#regular-methods) details what happens when a regular method is invoked.</span></span>

<span data-ttu-id="3eb9c-151">__迭代器方法__是一个具有 `Iterator` 修饰符并且没有 @no__t 的修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-151">An __iterator method__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="3eb9c-152">它必须是一个函数，并且它的返回类型必须为 `IEnumerator`、@no__t 为-1 或 `IEnumerator(Of T)` 或 `IEnumerable(Of T)` （对于某些 @no__t），并且它必须不包含 @no__t 参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-152">It must be a function, and its return type must be `IEnumerator`, `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and it must have no `ByRef` parameters.</span></span> <span data-ttu-id="3eb9c-153">节[迭代器方法](statements.md#iterator-methods)详细说明调用迭代器方法时所发生的情况。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-153">Section [Iterator Methods](statements.md#iterator-methods) details what happens when an iterator method is invoked.</span></span>

<span data-ttu-id="3eb9c-154">__Async 方法__是一个具有 `Async` 修饰符并且没有 @no__t 的修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-154">An __async method__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="3eb9c-155">它必须是一个子例程，或是一个返回类型为 `Task` 或 @no__t 为（对于某些 @no__t）的函数，并且必须没有任何 @no__t 3 参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-155">It must be either a subroutine, or a function with return type `Task` or `Task(Of T)` for some `T`, and must have no `ByRef` parameters.</span></span> <span data-ttu-id="3eb9c-156">节[Async 方法](statements.md#async-methods)详细说明了在调用 Async 方法时会发生的情况。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-156">Section [Async Methods](statements.md#async-methods) details what happens when an async method is invoked.</span></span>

<span data-ttu-id="3eb9c-157">如果某个方法不是以下三种方法之一，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-157">It is a compile-time error if a method is not one of these three kinds of method.</span></span>

<span data-ttu-id="3eb9c-158">子程序和函数声明特别适用于每个语句的开始和结束语句，都必须从逻辑行的开头开始。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-158">Subroutine and function declarations are special in that their beginning and end statements must each start at the beginning of a logical line.</span></span> <span data-ttu-id="3eb9c-159">此外，非 @no__t 的子程序或函数声明的主体必须从逻辑行的开头开始。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-159">Additionally, the body of a non-`MustOverride` subroutine or function declaration must start at the beginning of a logical line.</span></span> <span data-ttu-id="3eb9c-160">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-160">For example:</span></span>

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


### <a name="external-method-declarations"></a><span data-ttu-id="3eb9c-161">外部方法声明</span><span class="sxs-lookup"><span data-stu-id="3eb9c-161">External Method Declarations</span></span>

<span data-ttu-id="3eb9c-162">外部方法声明引入了一个新方法，该方法的实现是在程序外部提供的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-162">An external method declaration introduces a new method whose implementation is provided external to the program.</span></span>

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

<span data-ttu-id="3eb9c-163">由于外部方法声明不提供实际的实现，因此它没有方法体或 `End` 构造。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-163">Because an external method declaration provides no actual implementation, it has no method body or `End` construct.</span></span> <span data-ttu-id="3eb9c-164">外部方法是隐式共享的，不能具有类型参数，并且不能处理事件或实现接口成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-164">External methods are implicitly shared, may not have type parameters, and may not handle events or implement interface members.</span></span> <span data-ttu-id="3eb9c-165">如果未在函数上指定返回类型，并且使用了严格语义，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-165">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="3eb9c-166">否则，该类型将隐式 `Object` 或该方法类型字符的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-166">Otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="3eb9c-167">外部方法的返回类型和参数类型的可访问域必须与外部方法本身的可访问性域的可访问域相同或超集。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-167">The accessibility domain of the return type and parameter types of an external method must be the same as or a superset of the accessibility domain of the external method itself.</span></span>

<span data-ttu-id="3eb9c-168">外部方法声明的 library 子句指定了实现方法的外部文件的名称。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-168">The library clause of an external method declaration specifies the name of the external file that implements the method.</span></span> <span data-ttu-id="3eb9c-169">可选的 alias 子句是一个字符串，它指定外部文件中的数字序号（前缀为 @no__t 0 字符）或方法名称。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-169">The optional alias clause is a string that specifies the numeric ordinal (prefixed by a `#` character) or name of the method in the external file.</span></span> <span data-ttu-id="3eb9c-170">还可以指定单字符集修饰符，此修饰符控制在调用外部方法的过程中用于封送字符串的字符集。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-170">A single-character set modifier may also be specified, which governs the character set used to marshal strings during a call to the external method.</span></span> <span data-ttu-id="3eb9c-171">@No__t-0 修饰符将所有字符串封送到 Unicode 值，`Ansi` 修饰符将所有字符串封送为 ANSI 值，@no__t 2 修饰符 .NET Framework 根据方法的名称或别名（如果已指定）封送字符串。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-171">The `Unicode` modifier marshals all strings to Unicode values, the `Ansi` modifier marshals all strings to ANSI values, and the `Auto` modifier marshals the strings according to .NET Framework rules based on the name of the method, or the alias name if specified.</span></span> <span data-ttu-id="3eb9c-172">如果未指定修饰符，则默认值为 `Ansi`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-172">If no modifier is specified, the default is `Ansi`.</span></span>

<span data-ttu-id="3eb9c-173">如果指定 `Ansi` 或 `Unicode`，则在外部文件中查找方法名称，并且不进行修改。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-173">If `Ansi` or `Unicode` is specified, then the method name is looked up in the external file with no modification.</span></span> <span data-ttu-id="3eb9c-174">如果指定 `Auto`，则方法名称查找取决于平台。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-174">If `Auto` is specified, then method name lookup depends on the platform.</span></span> <span data-ttu-id="3eb9c-175">如果平台被视为 ANSI （例如，Windows 95、Windows 98、Windows ME），则将查找方法名称，但不进行任何修改。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-175">If the platform is considered to be ANSI (for example, Windows 95, Windows 98, Windows ME), then the method name is looked up with no modification.</span></span> <span data-ttu-id="3eb9c-176">如果查找失败，则追加 @no__t 0，并再次尝试查找。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-176">If the lookup fails, an `A` is appended and the lookup tried again.</span></span> <span data-ttu-id="3eb9c-177">如果平台被视为 Unicode （例如，Windows NT、Windows 2000、Windows XP），则追加 @no__t 0，并查找该名称。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-177">If the platform is considered to be Unicode (for example, Windows NT, Windows 2000, Windows XP), then a `W` is appended and the name is looked up.</span></span> <span data-ttu-id="3eb9c-178">如果查找失败，则再次尝试查找，而不会 `W`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-178">If the lookup fails, the lookup is tried again without the `W`.</span></span> <span data-ttu-id="3eb9c-179">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-179">For example:</span></span>

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

<span data-ttu-id="3eb9c-180">传递给外部方法的数据类型根据 .NET Framework 数据封送处理约定进行封送处理，但有一个例外。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-180">Data types being passed to external methods are marshaled according to the .NET Framework data marshalling conventions with one exception.</span></span> <span data-ttu-id="3eb9c-181">通过值传递的字符串变量（即 @no__t 0）将封送到 OLE 自动化 BSTR 类型，对外部方法中的 BSTR 所做的更改将反映在字符串参数中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-181">String variables that are passed by value (that is, `ByVal x As String`) are marshaled to the OLE Automation BSTR type, and changes made to the BSTR in the external method are reflected back in the string argument.</span></span> <span data-ttu-id="3eb9c-182">这是因为，外部方法中 `String` 的类型是可变的，而这种特殊的封送模拟该行为。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-182">This is because the type `String` in external methods is mutable, and this special marshalling mimics that behavior.</span></span> <span data-ttu-id="3eb9c-183">通过引用传递的字符串参数（即 @no__t 0）将作为指向 OLE 自动化 BSTR 类型的指针进行封送处理。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-183">String parameters that are passed by reference (i.e. `ByRef x As String`) are marshaled as a pointer to the OLE Automation BSTR type.</span></span> <span data-ttu-id="3eb9c-184">可以通过在参数上指定 `System.Runtime.InteropServices.MarshalAsAttribute` 特性来重写这些特殊行为。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-184">It is possible to override these special behaviors by specifying the `System.Runtime.InteropServices.MarshalAsAttribute` attribute on the parameter.</span></span>

<span data-ttu-id="3eb9c-185">该示例演示如何使用外部方法：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-185">The example demonstrates use of external methods:</span></span>

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


### <a name="overridable-methods"></a><span data-ttu-id="3eb9c-186">可重写方法</span><span class="sxs-lookup"><span data-stu-id="3eb9c-186">Overridable Methods</span></span>

<span data-ttu-id="3eb9c-187">@No__t-0 修饰符指示方法是可重写的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-187">The `Overridable` modifier indicates that a method is overridable.</span></span> <span data-ttu-id="3eb9c-188">@No__t-0 修饰符指示方法重写具有相同签名的基类型可重写方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-188">The `Overrides` modifier indicates that a method overrides a base-type overridable method that has the same signature.</span></span> <span data-ttu-id="3eb9c-189">@No__t-0 修饰符指示无法进一步重写可重写的方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-189">The `NotOverridable` modifier indicates that an overridable method cannot be further overridden.</span></span> <span data-ttu-id="3eb9c-190">@No__t-0 修饰符指示必须在派生类中重写方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-190">The `MustOverride` modifier indicates that a method must be overridden in derived classes.</span></span>

<span data-ttu-id="3eb9c-191">这些修饰符的某些组合无效：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-191">Certain combinations of these modifiers are not valid:</span></span>

* <span data-ttu-id="3eb9c-192">`Overridable` 和 `NotOverridable` 互相排斥，因此不能组合。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-192">`Overridable` and `NotOverridable` are mutually exclusive and cannot be combined.</span></span>

* <span data-ttu-id="3eb9c-193">`MustOverride` 表示 @no__t 为-1 （因此无法指定它），并且不能与 @no__t 2 进行组合。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-193">`MustOverride` implies `Overridable` (and so cannot specify it) and cannot be combined with `NotOverridable`.</span></span>

* <span data-ttu-id="3eb9c-194">`NotOverridable` 不能与 @no__t 或 @no__t 结合，并且必须与 `Overrides` 组合。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-194">`NotOverridable` cannot be combined with `Overridable` or `MustOverride` and must be combined with `Overrides`.</span></span>

* <span data-ttu-id="3eb9c-195">`Overrides` 表示 @no__t 为-1 （因此无法指定它），并且不能与 @no__t 2 进行组合。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-195">`Overrides` implies `Overridable` (and so cannot specify it) and cannot be combined with `MustOverride`.</span></span>

<span data-ttu-id="3eb9c-196">对于可重写方法还存在其他限制：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-196">There are also additional restrictions on overridable methods:</span></span>

* <span data-ttu-id="3eb9c-197">@No__t-0 方法不能包含方法体或 @no__t 构造，不能重写其他方法，并且只能出现在 @no__t 类中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-197">A `MustOverride` method may not include a method body or an `End` construct, may not override another method, and may only appear in `MustInherit` classes.</span></span>

* <span data-ttu-id="3eb9c-198">如果方法指定 `Overrides`，并且没有要重写的匹配基方法，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-198">If a method specifies `Overrides` and there is no matching base method to override, a compile-time error occurs.</span></span> <span data-ttu-id="3eb9c-199">重写的方法不能指定 `Shadows`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-199">An overriding method may not specify `Shadows`.</span></span>

* <span data-ttu-id="3eb9c-200">如果重写方法的可访问域与被重写的方法的可访问性域不相等，则方法不能重写另一个方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-200">A method may not override another method if the overriding method's accessibility domain is not equal to the accessibility domain of the method being overridden.</span></span> <span data-ttu-id="3eb9c-201">一种例外情况是，一个方法重写另一个程序集中没有 `Friend` 访问权限的 `Protected Friend` 方法，必须指定 `Protected` （不 `Protected Friend`）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-201">The one exception is that a method overriding a `Protected Friend` method in another assembly that does not have `Friend` access must specify `Protected` (not `Protected Friend`).</span></span>

* <span data-ttu-id="3eb9c-202">`Private` 方法不能 `Overridable`、`NotOverridable` 或 `MustOverride`，也不能覆盖其他方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-202">`Private` methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, nor may they override other methods.</span></span>

* <span data-ttu-id="3eb9c-203">@No__t-0 类中的方法不能声明为 `Overridable` 或 `MustOverride`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-203">Methods in `NotInheritable` classes may not be declared `Overridable` or `MustOverride`.</span></span>

<span data-ttu-id="3eb9c-204">下面的示例演示可重写方法和 nonoverridable 方法之间的差异：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-204">The following example illustrates the differences between overridable and nonoverridable methods:</span></span>

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

<span data-ttu-id="3eb9c-205">在此示例中，类 `Base` 引入了一个方法 `F` 和一个 `Overridable` @no__t 方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-205">In the example, class `Base` introduces a method `F` and an `Overridable` method `G`.</span></span> <span data-ttu-id="3eb9c-206">类 @no__t 引入了新的方法 `F`，因此会将继承的 @no__t 为隐藏，同时还会重写继承的方法 `G`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-206">The class `Derived` introduces a new method `F`, thus shadowing the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="3eb9c-207">此示例产生以下输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-207">The example produces the following output:</span></span>

```console
Base.F
Derived.F
Derived.G
Derived.G
```

<span data-ttu-id="3eb9c-208">请注意，语句 `b.G()` 调用 `Derived.G`，而不是 `Base.G`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-208">Notice that the statement `b.G()` invokes `Derived.G`, not `Base.G`.</span></span> <span data-ttu-id="3eb9c-209">这是因为实例的运行时类型（`Derived`），而不是实例的编译时类型（即 @no__t 为1）确定要调用的实际方法实现。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-209">This is because the run-time type of the instance (which is `Derived`) rather than the compile-time type of the instance (which is `Base`) determines the actual method implementation to invoke.</span></span>

### <a name="shared-methods"></a><span data-ttu-id="3eb9c-210">共享方法</span><span class="sxs-lookup"><span data-stu-id="3eb9c-210">Shared Methods</span></span>

<span data-ttu-id="3eb9c-211">@No__t 的修饰符表示方法是一个*共享方法*。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-211">The `Shared` modifier indicates a method is a *shared method*.</span></span> <span data-ttu-id="3eb9c-212">共享方法不会对类型的特定实例进行操作，并且可以直接从类型调用，而不是通过类型的特定实例调用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-212">A shared method does not operate on a specific instance of a type and may be invoked directly from a type rather than through a particular instance of a type.</span></span> <span data-ttu-id="3eb9c-213">不过，它有效地使用实例来限定共享方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-213">It is valid, however, to use an instance to qualify a shared method.</span></span> <span data-ttu-id="3eb9c-214">在共享方法中引用 `Me`、`MyClass` 或 `MyBase` 无效。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-214">It is invalid to refer to `Me`, `MyClass`, or `MyBase` in a shared method.</span></span> <span data-ttu-id="3eb9c-215">共享方法可能不 `Overridable`、`NotOverridable` 或 `MustOverride`，并且它们可能不会重写方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-215">Shared methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, and they may not override methods.</span></span> <span data-ttu-id="3eb9c-216">标准模块和接口中定义的方法不能指定 `Shared`，因为它们已隐式 `Shared`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-216">Methods defined in standard modules and interfaces may not specify `Shared`, because they are implicitly `Shared` already.</span></span>

<span data-ttu-id="3eb9c-217">在没有 `Shared` 修饰符的结构或类中声明的方法是*实例方法*。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-217">A method declared in a structure or class without a `Shared` modifier is an *instance method*.</span></span> <span data-ttu-id="3eb9c-218">实例方法在类型的给定实例上操作。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-218">An instance method operates on a given instance of a type.</span></span> <span data-ttu-id="3eb9c-219">仅可通过类型的实例调用实例方法，并可通过 `Me` 表达式引用实例。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-219">Instance methods can only be invoked through an instance of a type and may refer to the instance through the `Me` expression.</span></span>

<span data-ttu-id="3eb9c-220">下面的示例演示用于访问共享成员和实例成员的规则：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-220">The following example illustrates the rules for accessing shared and instance members:</span></span>

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

<span data-ttu-id="3eb9c-221">方法 `F` 显示在实例函数成员中，标识符可用于访问实例成员和共享成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-221">Method `F` shows that in an instance function member, an identifier can be used to access both instance members and shared members.</span></span> <span data-ttu-id="3eb9c-222">方法 `G` 在共享函数成员中显示，通过标识符访问实例成员是错误的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-222">Method `G` shows that in a shared function member, it is an error to access an instance member through an identifier.</span></span> <span data-ttu-id="3eb9c-223">方法 `Main` 表示在成员访问表达式中，实例成员必须通过实例访问，但可以通过类型或实例访问共享成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-223">Method `Main` shows that in a member access expression, instance members must be accessed through instances, but shared members can be accessed through types or instances.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="3eb9c-224">方法参数</span><span class="sxs-lookup"><span data-stu-id="3eb9c-224">Method Parameters</span></span>

<span data-ttu-id="3eb9c-225">*参数*是一个变量，可用于将信息传入和传出方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-225">A *parameter* is a variable that can be used to pass information into and out of a method.</span></span> <span data-ttu-id="3eb9c-226">方法的参数由方法的参数列表声明，该参数列表由一个或多个以逗号分隔的参数组成。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-226">Parameters of a method are declared by the method's parameter list, which consists of one or more parameters separated by commas.</span></span>

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

<span data-ttu-id="3eb9c-227">如果没有为参数指定类型，并且使用了严格语义，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-227">If no type is specified for a parameter and strict semantics are used, a compile-time error occurs.</span></span> <span data-ttu-id="3eb9c-228">否则，默认类型为 `Object` 或参数类型字符的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-228">Otherwise the default type is `Object` or the type of the parameter's type character.</span></span> <span data-ttu-id="3eb9c-229">即使在 "宽松语义" 下，如果一个参数包含 `As` 子句，所有参数都必须指定类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-229">Even under permissive semantics, if one parameter includes an `As` clause, all parameters must specify types.</span></span>

<span data-ttu-id="3eb9c-230">参数由修饰符指定为 value、reference、optional 或 paramarray 参数，分别为 `ByVal`、`ByRef`、`Optional` 和 @no__t 3。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-230">Parameters are specified as value, reference, optional, or paramarray parameters by the modifiers `ByVal`, `ByRef`, `Optional`, and `ParamArray`, respectively.</span></span> <span data-ttu-id="3eb9c-231">不指定 `ByRef` 或 @no__t 默认为 `ByVal` 的参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-231">A parameter that does not specify `ByRef` or `ByVal` defaults to `ByVal`.</span></span>

<span data-ttu-id="3eb9c-232">参数名称的范围限定为方法的整个主体，并且始终可公开访问。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-232">Parameter names are scoped to the entire body of the method and are always publicly accessible.</span></span> <span data-ttu-id="3eb9c-233">方法调用创建特定于该调用的副本，并且调用的参数列表将值或变量引用分配给新创建的参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-233">A method invocation creates a copy, specific to that invocation, of the parameters, and the argument list of the invocation assigns values or variable references to the newly created parameters.</span></span> <span data-ttu-id="3eb9c-234">由于外部方法声明和委托声明没有正文，因此参数列表中允许有重复的参数名，但不建议使用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-234">Because external method declarations and delegate declarations have no body, duplicate parameter names are allowed in parameter lists, but discouraged.</span></span>

<span data-ttu-id="3eb9c-235">标识符后面可以是可以为 null 的 name 修饰符 `?` 指示它可以为 null，并且还通过数组名称修饰符来指示它是一个数组。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-235">The identifier may be followed by the nullable name modifier `?` to indicate that it is nullable, and also by array name modifiers to indicate that it is an array.</span></span> <span data-ttu-id="3eb9c-236">它们可能会组合，例如 "`ByVal x?() As Integer`"。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-236">They may be combined, e.g. "`ByVal x?() As Integer`".</span></span> <span data-ttu-id="3eb9c-237">不允许使用显式数组界限;此外，如果有可以为 null 的 name 修饰符，则必须存在 @no__t 的-0 子句。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-237">It is not allowed to use explicit array bounds; also, if the nullable name modifier is present then an `As` clause must be present.</span></span>


#### <a name="value-parameters"></a><span data-ttu-id="3eb9c-238">值参数</span><span class="sxs-lookup"><span data-stu-id="3eb9c-238">Value Parameters</span></span>

<span data-ttu-id="3eb9c-239">*值参数*是使用显式 `ByVal` 修饰符声明的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-239">A *value parameter* is declared with an explicit `ByVal` modifier.</span></span> <span data-ttu-id="3eb9c-240">如果使用 `ByVal` 修饰符，则不能指定 @no__t 修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-240">If the `ByVal` modifier is used, the `ByRef` modifier may not be specified.</span></span> <span data-ttu-id="3eb9c-241">值参数与参数所属的成员的调用一起存在，并使用调用中给定的自变量的值初始化。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-241">A value parameter comes into existence with the invocation of the member the parameter belongs to, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="3eb9c-242">返回成员时，值参数将不再存在。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-242">A value parameter ceases to exist upon return of the member.</span></span>

<span data-ttu-id="3eb9c-243">允许方法为值参数赋值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-243">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="3eb9c-244">此类分配只会影响 value 参数所表示的本地存储位置;它们不会影响方法调用中给定的实际参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-244">Such assignments only affect the local storage location represented by the value parameter; they have no effect on the actual argument given in the method invocation.</span></span>

<span data-ttu-id="3eb9c-245">当参数的值传递到方法中时，将使用值参数，并且参数的修改不会影响原始参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-245">A value parameter is used when the value of an argument is passed into a method, and modifications of the parameter do not impact the original argument.</span></span> <span data-ttu-id="3eb9c-246">值参数引用其自己的变量，该变量与相应参数的变量不同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-246">A value parameter refers to its own variable, one that is distinct from the variable of the corresponding argument.</span></span> <span data-ttu-id="3eb9c-247">通过复制相应参数的值来初始化此变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-247">This variable is initialized by copying the value of the corresponding argument.</span></span> <span data-ttu-id="3eb9c-248">下面的示例演示一个方法 `F`，它具有名为 `p` 的值参数：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-248">The following example shows a method `F` that has a value parameter named `p`:</span></span>

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

<span data-ttu-id="3eb9c-249">即使修改了 value 参数 `p`，该示例仍会生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-249">The example produces the following output, even though the value parameter `p` is modified:</span></span>

```console
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a><span data-ttu-id="3eb9c-250">引用参数</span><span class="sxs-lookup"><span data-stu-id="3eb9c-250">Reference Parameters</span></span>

<span data-ttu-id="3eb9c-251">Reference 参数是用 `ByRef` 修饰符声明的参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-251">A reference parameter is a parameter declared with a `ByRef` modifier.</span></span> <span data-ttu-id="3eb9c-252">如果指定了 `ByRef` 修饰符，则不可能使用 @no__t 修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-252">If the `ByRef` modifier is specified, the `ByVal` modifier may not be used.</span></span> <span data-ttu-id="3eb9c-253">引用参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-253">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="3eb9c-254">相反，引用参数表示作为方法或构造函数调用中的自变量提供的变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-254">Instead, a reference parameter represents the variable given as the argument in the method or constructor invocation.</span></span> <span data-ttu-id="3eb9c-255">从概念上讲，引用参数的值始终与基础变量相同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-255">Conceptually, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="3eb9c-256">引用参数在两种模式下操作，可以是*别名*，也可以通过*复制回复制。*</span><span class="sxs-lookup"><span data-stu-id="3eb9c-256">Reference parameters act in two modes, either as *aliases* or through *copy-in copy-back.*</span></span>

<span data-ttu-id="3eb9c-257">__别名.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-257">__Aliases.__</span></span> <span data-ttu-id="3eb9c-258">如果参数充当调用方提供的参数的别名，则使用 reference 参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-258">A reference parameter is used when the parameter acts as an alias for a caller-provided argument.</span></span> <span data-ttu-id="3eb9c-259">引用参数本身不定义变量，而是指对应参数的变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-259">A reference parameter does not itself define a variable, but rather refers to the variable of the corresponding argument.</span></span> <span data-ttu-id="3eb9c-260">直接修改引用参数，并立即影响相应的参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-260">Modifications of a reference parameter directly and immediately impact the corresponding argument.</span></span> <span data-ttu-id="3eb9c-261">下面的示例演示一个具有两个引用参数 @no__t 的方法：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-261">The following example shows a method `Swap` that has two reference parameters:</span></span>

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

<span data-ttu-id="3eb9c-262">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-262">The output of the program is:</span></span>

```console
pre: x = 1, y = 2
post: x = 2, y = 1
```

<span data-ttu-id="3eb9c-263">对于类 `Main` 中方法 `Swap` 的调用，`a` 表示 @no__t，`b` 表示 `y`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-263">For the invocation of method `Swap` in class `Main`, `a` represents `x,` and `b` represents `y`.</span></span> <span data-ttu-id="3eb9c-264">因此，调用具有交换 @no__t 值和 `y` 的值的效果。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-264">Thus, the invocation has the effect of swapping the values of `x` and `y`.</span></span>

<span data-ttu-id="3eb9c-265">在采用引用参数的方法中，多个名称可能表示相同的存储位置：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-265">In a method that takes reference parameters, it is possible for multiple names to represent the same storage location:</span></span>

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

<span data-ttu-id="3eb9c-266">在此示例中，调用的方法 `F` 在 `G` 中为 @no__t 和 @no__t 传递对 `s` 的引用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-266">In the example the invocation of method `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="3eb9c-267">因此，对于该调用，名称 `s`、`a` 和 @no__t 均引用相同的存储位置，三个分配都将修改实例变量 `s`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-267">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance variable `s`.</span></span>

<span data-ttu-id="3eb9c-268">__复制回副本。__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-268">__Copy-in copy-back.__</span></span> <span data-ttu-id="3eb9c-269">如果传递到引用参数的变量类型与引用参数的类型不兼容，或者如果非变量（例如属性）作为参数传递给引用参数，则为; 如果调用是后期绑定的，则为临时变量被分配并传递给引用参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-269">If the type of the variable being passed to a reference parameter is not compatible with the reference parameter's type, or if a non-variable (e.g. a property) is passed as an argument to a reference parameter, or if the invocation is late-bound, then a temporary variable is allocated and passed to the reference parameter.</span></span> <span data-ttu-id="3eb9c-270">在调用方法之前，传入的值将复制到此临时变量中，并将在该方法返回时复制回原始变量（如果有）和原始变量（如果有）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-270">The value being passed in will be copied into this temporary variable before the method is invoked and will be copied back to the original variable (if there is one and if it's writable) when the method returns.</span></span> <span data-ttu-id="3eb9c-271">因此，引用参数可能不一定包含对要传入的变量的确切存储的引用，并且对引用参数所做的任何更改都不会反映在该方法退出之前的变量中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-271">Thus, a reference parameter may not necessarily contain a reference to the exact storage of the variable being passed in, and any changes to the reference parameter may not be reflected in the variable until the method exits.</span></span> <span data-ttu-id="3eb9c-272">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-272">For example:</span></span>

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

<span data-ttu-id="3eb9c-273">对于第一次调用 `F`，将创建一个临时变量，并将 @no__t 属性的值分配给它并将其传递到 `F`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-273">In the case of the first invocation of `F`, a temporary variable is created and the value of the property `G` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="3eb9c-274">从 `F` 返回后，会将临时变量中的值分配回 `G` 的属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-274">Upon return from `F`, the value in the temporary variable is assigned back to the property of `G`.</span></span> <span data-ttu-id="3eb9c-275">在第二种情况下，将创建另一个临时变量，并将 `d` 的值分配给它并将其传递到 `F` 中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-275">In the second case, another temporary variable is created and the value of `d` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="3eb9c-276">从 `F` 返回时，临时变量中的值会转换回变量类型，`Derived`，并分配给 `d`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-276">When returning from `F`, the value in the temporary variable is cast back to the type of the variable, `Derived`, and assigned to `d`.</span></span> <span data-ttu-id="3eb9c-277">由于传递回的值不能转换为 `Derived`，因此在运行时将引发异常。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-277">Since the value being passed back cannot be cast to `Derived`, an exception is thrown at run time.</span></span>

#### <a name="optional-parameters"></a><span data-ttu-id="3eb9c-278">可选参数</span><span class="sxs-lookup"><span data-stu-id="3eb9c-278">Optional Parameters</span></span>

<span data-ttu-id="3eb9c-279">使用 `Optional` 修饰符声明可选参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-279">An optional parameter is declared with the `Optional` modifier.</span></span> <span data-ttu-id="3eb9c-280">形参列表中可选参数之后的参数也必须是可选的;如果在以下参数上指定 `Optional` 修饰符，将触发编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-280">Parameters that follow an optional parameter in the formal parameter list must be optional as well; failure to specify the `Optional` modifier on the following parameters will trigger a compile-time error.</span></span> <span data-ttu-id="3eb9c-281">某些类型可以为 null 的类型的可选参数 `T?` 或不可为 null 的类型 @no__t 如果未指定参数，则必须指定一个 `e` 的常数表达式作为默认值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-281">An optional parameter of some type nullable type `T?` or non-nullable type `T` must specify a constant expression `e` to be used as a default value if no argument is specified.</span></span> <span data-ttu-id="3eb9c-282">如果 `e` 的计算结果为类型对象 `Nothing`，则*参数类型*的默认值将用作参数的默认值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-282">If `e` evaluates to `Nothing` of type Object, then the default value of the *parameter type* will be used as the default for the parameter.</span></span> <span data-ttu-id="3eb9c-283">否则，@no__t 0 必须是常量表达式，并将其作为参数的默认值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-283">Otherwise, `CType(e, T)` must be a constant expression and it is taken as the default for the parameter.</span></span>

<span data-ttu-id="3eb9c-284">可选参数是参数的初始值设定项有效的唯一情况。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-284">Optional parameters are the only situation in which an initializer on a parameter is valid.</span></span> <span data-ttu-id="3eb9c-285">初始化始终作为调用表达式的一部分完成，而不是在方法体自身内完成。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-285">The initialization is always done as a part of the invocation expression, not within the method body itself.</span></span>

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

<span data-ttu-id="3eb9c-286">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-286">The output of the program is:</span></span>

```console
x = 10, y = 20
x = 30, y = 40
```

<span data-ttu-id="3eb9c-287">可选参数不能在委托或事件声明中指定，也不能在 lambda 表达式中指定。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-287">Optional parameters may not be specified in delegate or event declarations, nor in lambda expressions.</span></span>

#### <a name="paramarray-parameters"></a><span data-ttu-id="3eb9c-288">ParamArray 参数</span><span class="sxs-lookup"><span data-stu-id="3eb9c-288">ParamArray Parameters</span></span>

<span data-ttu-id="3eb9c-289">@no__t 参数是用 `ParamArray` 修饰符声明的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-289">`ParamArray` parameters are declared with the `ParamArray` modifier.</span></span> <span data-ttu-id="3eb9c-290">如果 `ParamArray` 修饰符存在，则必须指定 `ByVal` 修饰符，而其他参数也不能使用 `ParamArray` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-290">If the `ParamArray` modifier is present, the `ByVal` modifier must be specified, and no other parameter may use the `ParamArray` modifier.</span></span> <span data-ttu-id="3eb9c-291">@No__t 参数的类型必须为一维数组，并且它必须是参数列表中的最后一个参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-291">The `ParamArray` parameter's type must be a one-dimensional array, and it must be the last parameter in the parameter list.</span></span>

<span data-ttu-id="3eb9c-292">@No__t-0 参数表示 @no__t 类型的类型不确定的参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-292">A `ParamArray` parameter represents an indeterminate number of parameters of the type of the `ParamArray`.</span></span> <span data-ttu-id="3eb9c-293">在方法本身中，@no__t 0 参数被视为其声明的类型并且没有特殊语义。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-293">Within the method itself, a `ParamArray` parameter is treated as its declared type and has no special semantics.</span></span> <span data-ttu-id="3eb9c-294">@No__t 的参数是隐式可选的，其默认值为 @no__t 的类型的一维数组的一维数组。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-294">A `ParamArray` parameter is implicitly optional, with a default value of an empty one-dimensional array of the type of the `ParamArray`.</span></span>

<span data-ttu-id="3eb9c-295">@No__t-0 允许在方法调用中使用以下两种方法之一指定参数：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-295">A `ParamArray` permits arguments to be specified in one of two ways in a method invocation:</span></span>

* <span data-ttu-id="3eb9c-296">为 @no__t 提供的参数可以是扩大到 @no__t 类型的类型的单个表达式。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-296">The argument given for a `ParamArray` can be a single expression of a type that widens to the `ParamArray` type.</span></span> <span data-ttu-id="3eb9c-297">在这种情况下，`ParamArray` 的作用与值形参完全相同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-297">In this case, the `ParamArray` acts precisely like a value parameter.</span></span>

* <span data-ttu-id="3eb9c-298">此外，调用可以为 @no__t 指定零个或多个参数，其中每个参数都是可隐式转换为 @no__t 的元素类型的类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-298">Alternatively, the invocation can specify zero or more arguments for the `ParamArray`, where each argument is an expression of a type that is implicitly convertible to the element type of the `ParamArray`.</span></span> <span data-ttu-id="3eb9c-299">在这种情况下，调用会创建一个 @no__t 0 类型的实例，该实例的长度与参数的数量相对应，使用给定的参数值初始化数组实例的元素，并使用新创建的数组实例作为实际实际.</span><span class="sxs-lookup"><span data-stu-id="3eb9c-299">In this case, the invocation creates an instance of the `ParamArray` type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="3eb9c-300">除了允许在调用中使用可变数量的自变量，@no__t 0 完全等效于同一类型的值形参，如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-300">Except for allowing a variable number of arguments in an invocation, a `ParamArray` is precisely equivalent to a value parameter of the same type, as the following example illustrates.</span></span>

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

<span data-ttu-id="3eb9c-301">该示例生成输出</span><span class="sxs-lookup"><span data-stu-id="3eb9c-301">The example produces the output</span></span>

```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="3eb9c-302">第一次调用 `F` 只是将数组 `a` 作为值参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-302">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="3eb9c-303">@No__t 的第二次调用会自动创建具有给定元素值的四元素数组，并将该数组实例作为值参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-303">The second invocation of `F` automatically creates a four-element array with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="3eb9c-304">同样，@no__t 的第三次调用会创建一个0元素数组，并将该实例作为值参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-304">Likewise, the third invocation of `F` creates a zero-element array and passes that instance as a value parameter.</span></span> <span data-ttu-id="3eb9c-305">第二次和第三次调用完全等效于写入：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-305">The second and third invocations are precisely equivalent to writing:</span></span>

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

<span data-ttu-id="3eb9c-306">不能在委托或事件声明中指定 @no__t 的参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-306">`ParamArray` parameters may not be specified in delegate or event declarations.</span></span>

### <a name="event-handling"></a><span data-ttu-id="3eb9c-307">事件处理</span><span class="sxs-lookup"><span data-stu-id="3eb9c-307">Event Handling</span></span>

<span data-ttu-id="3eb9c-308">方法可以声明方式处理实例中的对象或共享变量引发的事件。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-308">Methods can declaratively handle events raised by objects in instance or shared variables.</span></span> <span data-ttu-id="3eb9c-309">若要处理事件，方法声明会指定 @no__t 的关键字，并列出一个或多个事件。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-309">To handle events, a method declaration specifies the `Handles` keyword and lists one or more events.</span></span>

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

<span data-ttu-id="3eb9c-310">@No__t-0 列表中的事件由以句点分隔的两个标识符指定：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-310">An event in the `Handles` list is specified by two identifiers separated by a period:</span></span>

* <span data-ttu-id="3eb9c-311">第一个标识符必须为包含类型中的实例或共享变量，该类型指定 `WithEvents` 修饰符或 @no__t 或 `MyClass` 或 `Me` 关键字;否则，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-311">The first identifier must be an instance or shared variable in the containing type that specifies the `WithEvents` modifier or the `MyBase` or `MyClass` or `Me` keyword; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="3eb9c-312">此变量包含将引发此方法处理的事件的对象。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-312">This variable contains the object that will raise the events handled by this method.</span></span>

* <span data-ttu-id="3eb9c-313">第二个标识符必须指定第一个标识符的类型的成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-313">The second identifier must specify a member of the type of the first identifier.</span></span> <span data-ttu-id="3eb9c-314">该成员必须是事件，并且可以共享。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-314">The member must be an event, and may be shared.</span></span> <span data-ttu-id="3eb9c-315">如果为第一个标识符指定了共享变量，则必须共享该事件或错误结果。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-315">If a shared variable is specified for the first identifier, then the event must be shared, or an error results.</span></span>

<span data-ttu-id="3eb9c-316">如果 `AddHandler E, AddressOf M` 的语句也是有效的，则 `M` 的处理程序方法被视为事件 @no__t 的有效事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-316">A handler method `M` is considered a valid event handler for an event `E` if the statement `AddHandler E, AddressOf M` would also be valid.</span></span> <span data-ttu-id="3eb9c-317">但与 @no__t 0 语句不同，显式事件处理程序允许使用不带参数的方法处理事件，而不管是否使用了严格语义：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-317">Unlike an `AddHandler` statement, however, explicit event handlers allow handling an event with a method with no arguments regardless of whether strict semantics are being used or not:</span></span>

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

<span data-ttu-id="3eb9c-318">单个成员可以处理多个匹配事件，多个方法可能处理一个事件。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-318">A single member can handle multiple matching events, and multiple methods may handle a single event.</span></span> <span data-ttu-id="3eb9c-319">方法的可访问性不会影响其处理事件的能力。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-319">A method's accessibility has no effect on its ability to handle events.</span></span> <span data-ttu-id="3eb9c-320">下面的示例演示了方法如何处理事件：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-320">The following example shows how a method can handle events:</span></span>

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

<span data-ttu-id="3eb9c-321">这将输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-321">This will print out:</span></span>

```console
Raised
Raised
```

<span data-ttu-id="3eb9c-322">类型继承其基类型提供的所有事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-322">A type inherits all event handlers provided by its base type.</span></span> <span data-ttu-id="3eb9c-323">派生类型无法以任何方式更改它从其基类型继承的事件映射，但可能会向事件添加其他处理程序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-323">A derived type cannot in any way alter the event mappings it inherits from its base types, but may add additional handlers to the event.</span></span>


### <a name="extension-methods"></a><span data-ttu-id="3eb9c-324">扩展方法</span><span class="sxs-lookup"><span data-stu-id="3eb9c-324">Extension Methods</span></span>

<span data-ttu-id="3eb9c-325">可以使用*扩展方法*将方法添加到类型声明外部的类型中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-325">Methods can be added to types from outside of the type declaration using *extension methods*.</span></span> <span data-ttu-id="3eb9c-326">扩展方法是应用了 `System.Runtime.CompilerServices.ExtensionAttribute` 特性的方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-326">Extension methods are methods with the `System.Runtime.CompilerServices.ExtensionAttribute` attribute applied to them.</span></span> <span data-ttu-id="3eb9c-327">它们只能在标准模块中声明，并且必须至少有一个参数，该参数指定方法扩展的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-327">They can only be declared in standard modules and must have at least one parameter, which specifies the type the method extends.</span></span> <span data-ttu-id="3eb9c-328">例如，下面的扩展方法将类型扩展 `String`：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-328">For example, the following extension method extends the type `String`:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

<span data-ttu-id="3eb9c-329">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-329">__Note.__</span></span> <span data-ttu-id="3eb9c-330">尽管 Visual Basic 要求在标准模块中声明扩展方法，但其他语言（如） C#可能会允许在其他类型的类型中声明这些方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-330">Although Visual Basic requires extension methods to be declared in a standard module, other languages such as C# may allow them to be declared in other kinds of types.</span></span> <span data-ttu-id="3eb9c-331">只要这些方法遵循此处所述的其他约定并且包含类型不是开放式泛型类型并且无法实例化，Visual Basic 将识别扩展方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-331">As long as the methods follow the other conventions outlined here and the containing type is not an open generic type and cannot be instantiated, Visual Basic will recognize the extension methods.</span></span>

<span data-ttu-id="3eb9c-332">调用扩展方法时，对其调用的实例将传递给第一个参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-332">When an extension method is invoked, the instance it is being invoked on is passed to the first parameter.</span></span> <span data-ttu-id="3eb9c-333">第一个参数不能声明为 `Optional` 或 @no__t 为-1。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-333">The first parameter cannot be declared `Optional` or `ParamArray`.</span></span> <span data-ttu-id="3eb9c-334">任何类型（包括类型参数）都可以显示为扩展方法的第一个参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-334">Any type, including a type parameter, can appear as the first parameter of an extension method.</span></span> <span data-ttu-id="3eb9c-335">例如，以下方法将类型扩展 `Integer()`、实现 @no__t 的任何类型，以及所有类型：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-335">For example, the following methods extend the types `Integer()`, any type that implements `System.Collections.Generic.IEnumerable(Of T)`, and any type at all:</span></span>

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

<span data-ttu-id="3eb9c-336">如前面的示例所示，可以扩展接口。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-336">As the previous example shows, interfaces can be extended.</span></span> <span data-ttu-id="3eb9c-337">接口扩展方法提供方法的实现，因此，实现接口的类型在其上定义的扩展方法仍只能实现此接口最初声明的成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-337">Interface extension methods supply the implementation of the method, so types that implement an interface that has extension methods defined on it still only implement the members originally declared by the interface.</span></span> <span data-ttu-id="3eb9c-338">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-338">For example:</span></span>

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

<span data-ttu-id="3eb9c-339">扩展方法也可以对其类型参数具有类型约束，而与非扩展泛型方法一样，可以推断类型参数：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-339">Extension methods can also have type constraints on their type parameters and, just as with non-extension generic methods, type argument can be inferred:</span></span>

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

<span data-ttu-id="3eb9c-340">还可以通过要扩展的类型内的隐式实例表达式来访问扩展方法：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-340">Extension methods can also be accessed through implicit instance expressions within the type being extended:</span></span>

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

<span data-ttu-id="3eb9c-341">出于辅助功能的目的，扩展方法也被视为在中声明的标准模块的成员--它们不会对其进行扩展的成员以外的成员提供额外的访问权限。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-341">For the purposes of accessibility, extension methods are also treated as members of the standard module they are declared in -- they have no extra access to the members of the type they are extending beyond the access they have by virtue of their declaration context.</span></span>

<span data-ttu-id="3eb9c-342">只有在标准模块方法处于范围内时，扩展方法才可用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-342">Extensions methods are only available when the standard module method is in scope.</span></span> <span data-ttu-id="3eb9c-343">否则，原始类型将不会扩展。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-343">Otherwise, the original type will not appear to have been extended.</span></span> <span data-ttu-id="3eb9c-344">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-344">For example:</span></span>

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

<span data-ttu-id="3eb9c-345">当只有类型的扩展方法可用时，引用类型仍会生成编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-345">Referring to a type when only an extension method on the type is available will still produce a compile-time error.</span></span>

<span data-ttu-id="3eb9c-346">需要注意的是，扩展方法被视为属于成员的所有上下文中的类型的成员，例如强类型 `For Each` 模式。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-346">It is important to note that extension methods are considered to be members of the type in all contexts where members are bound, such as the strongly-typed `For Each` pattern.</span></span> <span data-ttu-id="3eb9c-347">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-347">For example:</span></span>

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

<span data-ttu-id="3eb9c-348">还可以创建引用扩展方法的委托。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-348">Delegates can also be created that refer to extension methods.</span></span> <span data-ttu-id="3eb9c-349">因此，代码如下：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-349">Thus, the code:</span></span>

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

<span data-ttu-id="3eb9c-350">大致等效于：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-350">is roughly equivalent to:</span></span>

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

<span data-ttu-id="3eb9c-351">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-351">__Note.__</span></span> <span data-ttu-id="3eb9c-352">Visual Basic 通常会在实例方法调用中插入检查，如果调用该方法的实例为，@no__t 则会导致 `System.NullReferenceException`，导致出现0。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-352">Visual Basic normally inserts a check on an instance method call that causes a `System.NullReferenceException` to occur if the instance the method is being invoked on is `Nothing`.</span></span> <span data-ttu-id="3eb9c-353">对于扩展方法，没有有效的方法来插入此项检查，因此扩展方法将需要显式检查 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-353">In the case of extension methods, there is no efficient way to insert this check, so extension methods will need to explicitly check for `Nothing`.</span></span> 

<span data-ttu-id="3eb9c-354">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-354">__Note.__</span></span> <span data-ttu-id="3eb9c-355">将值类型 @no__t 作为参数传递给类型为接口的参数时，将对其进行装箱。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-355">A value type will be boxed when being passed as a `ByVal` argument to a parameter typed as an interface.</span></span>  <span data-ttu-id="3eb9c-356">这意味着扩展方法的副作用将对结构的副本（而不是原始结构）进行操作。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-356">This implies that side effects of the extension method will operate on a copy of the structure instead of the original.</span></span> <span data-ttu-id="3eb9c-357">尽管语言对扩展方法的第一个参数没有任何限制，但建议不将扩展方法用于扩展值类型，或者在扩展值类型时，第一个参数将传递 `ByRef` 以确保副作用对原始值执行运算。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-357">While the language puts no restrictions on the first argument of an extension method, it is recommended that extension methods are not used to extend value types or that when extending value types, the first parameter is passed `ByRef` to ensure that side effects operate on the original value.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="3eb9c-358">分部方法</span><span class="sxs-lookup"><span data-stu-id="3eb9c-358">Partial Methods</span></span>

<span data-ttu-id="3eb9c-359">*分部方法*是指定签名但不指定方法主体的方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-359">A *partial method* is a method that specifies a signature but not the body of the method.</span></span> <span data-ttu-id="3eb9c-360">方法的主体可由具有相同名称和签名的另一种方法声明提供，最有可能出现在该类型的另一个分部声明中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-360">The body of the method can be supplied by another method declaration with the same name and signature, most likely in another partial declaration of the type.</span></span> <span data-ttu-id="3eb9c-361">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-361">For example:</span></span>

<span data-ttu-id="3eb9c-362">.vb：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-362">a.vb:</span></span>

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

<span data-ttu-id="3eb9c-363">b. .vb：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-363">b.vb:</span></span>

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

<span data-ttu-id="3eb9c-364">在此示例中，类的分部声明 `MyForm` 声明分部方法 `ValidateControls`，无实现。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-364">In this example, a partial declaration of the class `MyForm` declares a partial method `ValidateControls` with no implementation.</span></span> <span data-ttu-id="3eb9c-365">分部声明中的构造函数调用分部方法，即使文件中未提供正文也是如此。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-365">The constructor in the partial declaration calls the partial method, even though there is no body supplied in the file.</span></span> <span data-ttu-id="3eb9c-366">然后，@no__t 的其他分部声明提供方法的实现。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-366">The other partial declaration of `MyForm` then supplies the implementation of the method.</span></span>

<span data-ttu-id="3eb9c-367">无论是否已提供正文，都可以调用分部方法;如果未提供任何方法体，则忽略此调用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-367">Partial methods can be called regardless of whether a body has been supplied; if no method body is supplied, the call is ignored.</span></span> <span data-ttu-id="3eb9c-368">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-368">For example:</span></span>

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

<span data-ttu-id="3eb9c-369">作为参数传递给被忽略的分部方法调用的任何表达式也会被忽略，并且不会进行计算。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-369">Any expressions that are passed in as arguments to a partial method call that is ignored are ignored also and not evaluated.</span></span> <span data-ttu-id="3eb9c-370">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-370">(__Note.__</span></span> <span data-ttu-id="3eb9c-371">这意味着分部方法是一种非常有效的方法，它提供跨两个分部类型定义的行为，因为如果没有使用部分方法，则不会产生任何费用。）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-371">This means that partial methods are a very efficient way of providing behavior that is defined across two partial types, since the partial methods have no cost if they are not used.)</span></span>

<span data-ttu-id="3eb9c-372">分部方法声明必须声明为 `Private`，并且必须始终是在其主体中没有语句的子例程。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-372">The partial method declaration must be declared as `Private` and must always be a subroutine with no statements in its body.</span></span> <span data-ttu-id="3eb9c-373">分部方法本身不能实现接口方法，尽管提供其主体的方法可以。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-373">Partial methods cannot themselves implement interface methods, although the method that supplies their body can.</span></span>

<span data-ttu-id="3eb9c-374">只有一个方法可以向分部方法提供主体。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-374">Only one method can supply a body to a partial method.</span></span> <span data-ttu-id="3eb9c-375">向分部方法提供主体的方法必须具有与分部方法相同的签名、对任何类型参数的相同约束、相同的声明修饰符以及相同的参数和类型参数名称。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-375">A method supplying a body to a partial method must have the same signature as the partial method, the same constraints on any type parameters, the same declaration modifiers, and the same parameter and type parameter names.</span></span> <span data-ttu-id="3eb9c-376">分部方法的属性和提供其正文的方法将合并，这与方法的参数中的任何属性相同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-376">Attributes on the partial method and the method that supplies its body are merged, as are any attributes on the methods' parameters.</span></span> <span data-ttu-id="3eb9c-377">同样，将合并方法处理的事件列表。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-377">Similarly, the list of events that the methods handle is merged.</span></span> <span data-ttu-id="3eb9c-378">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-378">For example:</span></span>

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

## <a name="constructors"></a><span data-ttu-id="3eb9c-379">构造函数</span><span class="sxs-lookup"><span data-stu-id="3eb9c-379">Constructors</span></span>

<span data-ttu-id="3eb9c-380">*构造函数*是允许对初始化进行控制的特殊方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-380">*Constructors* are special methods that allow control over initialization.</span></span> <span data-ttu-id="3eb9c-381">在程序开始或创建类型的实例后，它们将运行。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-381">They are run after the program begins or when an instance of a type is created.</span></span> <span data-ttu-id="3eb9c-382">与其他成员不同，构造函数不会被继承，也不会在类型的声明空间中引入名称。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-382">Unlike other members, constructors are not inherited and do not introduce a name into a type's declaration space.</span></span> <span data-ttu-id="3eb9c-383">构造函数只能由对象创建表达式或 .NET Framework 调用;它们永远不能直接调用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-383">Constructors may only be invoked by object-creation expressions or by the .NET Framework; they may never be directly invoked.</span></span>

<span data-ttu-id="3eb9c-384">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-384">__Note.__</span></span> <span data-ttu-id="3eb9c-385">构造函数在子例程具有的行位置上具有相同的限制。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-385">Constructors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="3eb9c-386">开始语句、结束语句和块必须全部显示在逻辑行的开头。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-386">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

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

### <a name="instance-constructors"></a><span data-ttu-id="3eb9c-387">实例构造函数</span><span class="sxs-lookup"><span data-stu-id="3eb9c-387">Instance Constructors</span></span>

<span data-ttu-id="3eb9c-388">*实例构造函数*初始化类型的实例，并由 .NET Framework 在创建实例时运行。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-388">*Instance constructors* initialize instances of a type and are run by the .NET Framework when an instance is created.</span></span> <span data-ttu-id="3eb9c-389">构造函数的参数列表服从与方法的参数列表相同的规则。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-389">The parameter list of a constructor is subject to the same rules as the parameter list of a method.</span></span> <span data-ttu-id="3eb9c-390">实例构造函数可能会重载。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-390">Instance constructors may be overloaded.</span></span>

<span data-ttu-id="3eb9c-391">引用类型中的所有构造函数都必须调用另一个构造函数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-391">All constructors in reference types must invoke another constructor.</span></span> <span data-ttu-id="3eb9c-392">如果调用是显式的，则它必须是构造函数方法体中的第一条语句。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-392">If the invocation is explicit, it must be the first statement in the constructor method body.</span></span> <span data-ttu-id="3eb9c-393">语句可以调用类型的实例构造函数中的另一个（例如 `Me.New(...)` 或 @no__t--），或者，如果它不是结构，则它可以调用该类型的基类型的实例构造函数-例如，`MyBase.New(...)`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-393">The statement can either invoke another of the type's instance constructors -- for example, `Me.New(...)` or `MyClass.New(...)` -- or if it is not a structure it can invoke an instance constructor of the type's base type -- for example, `MyBase.New(...)`.</span></span> <span data-ttu-id="3eb9c-394">构造函数调用自身是无效的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-394">It is invalid for a constructor to invoke itself.</span></span> <span data-ttu-id="3eb9c-395">如果构造函数省略了对另一个构造函数的调用，则 `MyBase.New()` 是隐式的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-395">If a constructor omits a call to another constructor, `MyBase.New()` is implicit.</span></span> <span data-ttu-id="3eb9c-396">如果没有无参数的基类型构造函数，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-396">If there is no parameterless base type constructor, a compile-time error occurs.</span></span> <span data-ttu-id="3eb9c-397">由于在调用基类构造函数之前，不会将 `Me` 视为构造，因此构造函数调用语句的参数不能直接或显式引用 `Me`、@no__t 或 @no__t。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-397">Because `Me` is not considered to be constructed until after the call to a base class constructor, the parameters to a constructor invocation statement cannot reference `Me`, `MyClass`, or `MyBase` implicitly or explicitly.</span></span>

<span data-ttu-id="3eb9c-398">当构造函数的第一个语句的形式为 `MyBase.New(...)` 时，构造函数将隐式执行由类型中声明的实例变量的变量初始值设定项指定的初始化。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-398">When a constructor's first statement is of the form `MyBase.New(...)`, the constructor implicitly performs the initializations specified by the variable initializers of the instance variables declared in the type.</span></span> <span data-ttu-id="3eb9c-399">这对应于调用直接基类型构造函数之后立即执行的一系列赋值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-399">This corresponds to a sequence of assignments that are executed immediately after invoking the direct base type constructor.</span></span> <span data-ttu-id="3eb9c-400">此类排序可确保在执行具有对实例的访问权限的任何语句之前，通过其变量初始值设定项来初始化所有基实例变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-400">Such ordering ensures that all base instance variables are initialized by their variable initializers before any statements that have access to the instance are executed.</span></span> <span data-ttu-id="3eb9c-401">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-401">For example:</span></span>

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

<span data-ttu-id="3eb9c-402">当 `New B()` 用于创建 `B` 的实例时，将生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-402">When `New B()` is used to create an instance of `B`, the following output is produced:</span></span>

```console
x = 1, y = 1
```

<span data-ttu-id="3eb9c-403">@No__t-0 的值是 `1`，因为在调用基类构造函数之后执行变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-403">The value of `y` is `1` because the variable initializer is executed after the base class constructor is invoked.</span></span> <span data-ttu-id="3eb9c-404">变量初始值设定项将按照它们在类型声明中出现的文本顺序执行。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-404">Variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="3eb9c-405">当某个类型仅声明 `Private` 构造函数时，通常不能从类型派生其他类型或创建该类型的实例;唯一的例外是类型嵌套在类型中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-405">When a type declares only `Private` constructors, it is not possible in general for other types to derive from the type or create instances of the type; the only exception is types nested within the type.</span></span> <span data-ttu-id="3eb9c-406">`Private` 构造函数通常用于仅包含 @no__t 1 成员的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-406">`Private` constructors are commonly used in types that contain only `Shared` members.</span></span>

<span data-ttu-id="3eb9c-407">如果某个类型不包含任何实例构造函数声明，则会自动提供默认构造函数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-407">If a type contains no instance constructor declarations, a default constructor is automatically provided.</span></span> <span data-ttu-id="3eb9c-408">默认构造函数只调用直接基类型的无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-408">The default constructor simply invokes the parameterless constructor of the direct base type.</span></span> <span data-ttu-id="3eb9c-409">如果直接基类型没有可访问的无参数构造函数，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-409">If the direct base type does not have an accessible parameterless constructor, a compile-time error occurs.</span></span> <span data-ttu-id="3eb9c-410">默认构造函数的声明的访问类型为 `Public`，除非该类型为 `MustInherit`，在这种情况下，默认构造函数 @no__t 为-2。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-410">The declared access type for the default constructor is `Public` unless the type is `MustInherit`, in which case the default constructor is `Protected`.</span></span>

<span data-ttu-id="3eb9c-411">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-411">__Note.__</span></span> <span data-ttu-id="3eb9c-412">@No__t 类型的默认构造函数的默认访问 @no__t 为-1，因为不能直接创建 `MustInherit` 类。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-412">The default access for a `MustInherit` type's default constructor is `Protected` because `MustInherit` classes cannot be created directly.</span></span> <span data-ttu-id="3eb9c-413">因此，不需要将默认构造函数 `Public`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-413">So there is no point in making the default constructor `Public`.</span></span>

<span data-ttu-id="3eb9c-414">在下面的示例中，提供了一个默认构造函数，因为该类不包含任何构造函数声明：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-414">In the following example a default constructor is provided because the class contains no constructor declarations:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

<span data-ttu-id="3eb9c-415">因此，该示例完全等效于以下内容：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-415">Thus, the example is precisely equivalent to the following:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

<span data-ttu-id="3eb9c-416">发出到使用特性标记的设计器生成的类的默认构造函数 `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` 会在调用基构造函数后调用方法 `Sub InitializeComponent()` （如果存在）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-416">Default constructors that are emitted into a designer generated class marked with the attribute `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` will call the method `Sub InitializeComponent()`, if it exists, after the call to the base constructor.</span></span> <span data-ttu-id="3eb9c-417">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-417">(__Note.__</span></span> <span data-ttu-id="3eb9c-418">这使得设计器生成的文件（如 WinForms 设计器创建的文件）可以在设计器文件中省略该构造函数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-418">This allows designer generated files, such as those created by the WinForms designer, to omit the constructor in the designer file.</span></span> <span data-ttu-id="3eb9c-419">这样一来，程序员可以自行指定它。）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-419">This enables the programmer to specify it themselves, if they so choose.)</span></span>

### <a name="shared-constructors"></a><span data-ttu-id="3eb9c-420">共享构造函数</span><span class="sxs-lookup"><span data-stu-id="3eb9c-420">Shared Constructors</span></span>

<span data-ttu-id="3eb9c-421">*共享构造函数*初始化类型的共享变量;它们将在程序开始执行后、对类型成员的任何引用之前运行。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-421">*Shared constructors* initialize a type's shared variables; they are run after the program begins executing, but before any references to a member of the type.</span></span> <span data-ttu-id="3eb9c-422">共享构造函数指定 `Shared` 修饰符，除非它在标准模块中，这种情况下，`Shared` 修饰符是隐含的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-422">A shared constructor specifies the `Shared` modifier, unless it is in a standard module in which case the `Shared` modifier is implied.</span></span>

<span data-ttu-id="3eb9c-423">与实例构造函数不同，共享构造函数具有隐式公共访问权限，没有参数，也不能调用其他构造函数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-423">Unlike instance constructors, shared constructors have implicit public access, have no parameters, and may not call other constructors.</span></span> <span data-ttu-id="3eb9c-424">在共享构造函数中的第一个语句之前，共享构造函数会隐式执行由类型中声明的共享变量的变量初始值设定项指定的初始化。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-424">Before the first statement in a shared constructor, the shared constructor implicitly performs the initializations specified by the variable initializers of the shared variables declared in the type.</span></span> <span data-ttu-id="3eb9c-425">这对应于进入构造函数时立即执行的一系列赋值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-425">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor.</span></span> <span data-ttu-id="3eb9c-426">变量初始值设定项将按照它们在类型声明中出现的文本顺序执行。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-426">The variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="3eb9c-427">下面的示例演示了一个带有初始化共享变量的共享构造函数的 @no__t 0 类：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-427">The following example shows an `Employee` class with a shared constructor that initializes a shared variable:</span></span>

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

<span data-ttu-id="3eb9c-428">每个封闭式泛型类型都存在单独的共享构造函数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-428">A separate shared constructor exists for each closed generic type.</span></span> <span data-ttu-id="3eb9c-429">由于共享构造函数只对每个关闭类型执行一次，因此，在不能通过约束在编译时检查的类型参数上强制执行运行时检查是一个方便的位置。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-429">Because the shared constructor is executed exactly once for each closed type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints.</span></span> <span data-ttu-id="3eb9c-430">例如，下面的类型使用共享构造函数来强制类型参数 @no__t 为-0 或 `Double`：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-430">For example, the following type uses a shared constructor to enforce that the type parameter is `Integer` or `Double`:</span></span>

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

<span data-ttu-id="3eb9c-431">共享构造函数在运行时正好是依赖实现的，但如果已显式定义了一个共享构造函数，则会提供多个保证：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-431">Exactly when shared constructors are run is mostly implementation dependent, though several guarantees are provided if a shared constructor is explicitly defined:</span></span>

* <span data-ttu-id="3eb9c-432">共享构造函数在第一次访问类型的任何静态字段之前运行。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-432">Shared constructors are run before the first access to any static field of the type.</span></span>

* <span data-ttu-id="3eb9c-433">共享构造函数在第一次调用类型的任何静态方法之前运行。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-433">Shared constructors are run before the first invocation of any static method of the type.</span></span>

* <span data-ttu-id="3eb9c-434">在第一次调用该类型的任何构造函数之前，将运行共享构造函数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-434">Shared constructors are run before the first invocation of any constructor for the type.</span></span>

<span data-ttu-id="3eb9c-435">如果共享构造函数是为共享初始值设定项隐式创建的，则上述保证不适用于。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-435">The above guarantees do not apply in the situation where a shared constructor is implicitly created for shared initializers.</span></span> <span data-ttu-id="3eb9c-436">以下示例的输出是不确定的，因为未定义加载的准确顺序，因此不会定义执行共享构造函数：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-436">The output from the following example is uncertain, because the exact ordering of loading and therefore of shared constructor execution is not defined:</span></span>

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

<span data-ttu-id="3eb9c-437">输出可以是以下内容之一：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-437">The output could be either of the following:</span></span>

```console
Init A
A.F
Init B
B.F
```

<span data-ttu-id="3eb9c-438">或</span><span class="sxs-lookup"><span data-stu-id="3eb9c-438">or</span></span>

```console
Init B
Init A
A.F
B.F
```

<span data-ttu-id="3eb9c-439">与此相反，以下示例生成可预测的输出。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-439">By contrast, the following example produces predictable output.</span></span> <span data-ttu-id="3eb9c-440">请注意，尽管类 `B` 派生自类，但从不执行类的 @no__t @no__t 构造函数：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-440">Note that the `Shared` constructor for the class `A` never executes, even though class `B` derives from it:</span></span>

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

<span data-ttu-id="3eb9c-441">输出为：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-441">The output is:</span></span>

```console
Init B
B.G
```

<span data-ttu-id="3eb9c-442">还可以构造循环依赖关系，以便在其默认值状态中观察变量初始值设定项的 @no__t 0 变量，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-442">It is also possible to construct circular dependencies that allow `Shared` variables with variable initializers to be observed in their default value state, as in the following example:</span></span>

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

<span data-ttu-id="3eb9c-443">这会生成输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-443">This produces the output:</span></span>

```console
X = 1, Y = 2
```

<span data-ttu-id="3eb9c-444">若要执行 `Main` 方法，系统首先加载类 `B`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-444">To execute the `Main` method, the system first loads class `B`.</span></span> <span data-ttu-id="3eb9c-445">类 `B` 的 @no__t 构造函数将继续计算 `Y` 的初始值，这会以递归方式导致加载类 @no__t，因为引用了 `A.X` 的值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-445">The `Shared` constructor of class `B` proceeds to compute the initial value of `Y`, which recursively causes class `A` to be loaded because the value of `A.X` is referenced.</span></span> <span data-ttu-id="3eb9c-446">类 `A` 的 @no__t 类构造函数将继续计算 @no__t 2 的初始值，而在执行此操作时，将获取 `Y` 的*默认*值，该值为零。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-446">The `Shared` constructor of class `A` in turn proceeds to compute the initial value of `X`, and in doing so fetches the *default* value of `Y`, which is zero.</span></span> <span data-ttu-id="3eb9c-447">`A.X` 将初始化为 `1`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-447">`A.X` is thus initialized to `1`.</span></span> <span data-ttu-id="3eb9c-448">然后，将 @no__t 加载完成后，返回到 `Y` 的初始值计算的结果，结果为 `2`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-448">The process of loading `A` then completes, returning to the calculation of the initial value of `Y`, the result of which becomes `2`.</span></span>

<span data-ttu-id="3eb9c-449">如果有 `Main` 方法（而不是位于类 `A` 中），该示例将生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-449">Had the `Main` method instead been located in class `A`, the example would have produced the following output:</span></span>

```console
X = 2, Y = 1
```

<span data-ttu-id="3eb9c-450">避免 `Shared` 变量初始值设定项中的循环引用，因为通常无法确定包含此类引用的类的加载顺序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-450">Avoid circular references in `Shared` variable initializers since it is generally impossible to determine the order in which classes containing such references are loaded.</span></span>

## <a name="events"></a><span data-ttu-id="3eb9c-451">Events</span><span class="sxs-lookup"><span data-stu-id="3eb9c-451">Events</span></span>

<span data-ttu-id="3eb9c-452">事件用于向代码通知特定事件发生。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-452">Events are used to notify code of a particular occurrence.</span></span> <span data-ttu-id="3eb9c-453">事件声明由标识符（委托类型或参数列表）和可选 `Implements` 子句组成。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-453">An event declaration consists of an identifier, either a delegate type or a parameter list, and an optional `Implements` clause.</span></span>

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

<span data-ttu-id="3eb9c-454">如果指定了委托类型，则委托类型不能有返回类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-454">If a delegate type is specified, the delegate type may not have a return type.</span></span> <span data-ttu-id="3eb9c-455">如果指定了参数列表，则它不能包含 `Optional` 或 `ParamArray` 参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-455">If a parameter list is specified, it may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="3eb9c-456">参数类型和/或委托类型的可访问域必须与事件本身的可访问性域相同或超集。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-456">The accessibility domain of the parameter types and/or delegate type must be the same as, or a superset of, the accessibility domain of the event itself.</span></span> <span data-ttu-id="3eb9c-457">可以通过指定 `Shared` 修饰符来共享事件。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-457">Events may be shared by specifying the `Shared` modifier.</span></span>

<span data-ttu-id="3eb9c-458">除了添加到类型声明空间的成员名称外，事件声明还会隐式声明多个其他成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-458">In addition to the member name added to the type's declaration space, an event declaration implicitly declares several other members.</span></span> <span data-ttu-id="3eb9c-459">假设有一个名为 `X` 的事件，则会将以下成员添加到声明空间：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-459">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="3eb9c-460">如果声明的形式为方法声明，则会引入一个名为 `XEventHandler` 的嵌套委托类。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-460">If the form of the declaration is a method declaration, a nested delegate class named `XEventHandler` is introduced.</span></span> <span data-ttu-id="3eb9c-461">嵌套的委托类与方法声明匹配，并且具有与事件相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-461">The nested delegate class matches the method declaration and has the same accessibility as the event.</span></span> <span data-ttu-id="3eb9c-462">参数列表中的特性适用于委托类的参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-462">The attributes in the parameter list apply to the parameters of the delegate class.</span></span>

* <span data-ttu-id="3eb9c-463">类型化为委托的 @no__t 0 实例变量，名为 `XEvent`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-463">A `Private` instance variable typed as the delegate, named `XEvent`.</span></span>

* <span data-ttu-id="3eb9c-464">名为 @no__t 的两个方法不能调用、重写或重载 `remove_X`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-464">Two methods named `add_X` and `remove_X` which cannot be invoked, overridden or overloaded.</span></span>

<span data-ttu-id="3eb9c-465">如果某个类型尝试声明与上述某个名称匹配的名称，则会导致编译时错误，并将忽略隐式 `add_X` 和 `remove_X` 声明，以便进行名称绑定。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-465">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit `add_X` and `remove_X` declarations are ignored for the purposes of name binding.</span></span> <span data-ttu-id="3eb9c-466">不能重写或重载任何引入的成员，但可以在派生类型中隐藏它们。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-466">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span> <span data-ttu-id="3eb9c-467">例如，类声明</span><span class="sxs-lookup"><span data-stu-id="3eb9c-467">For example, the class declaration</span></span>

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

<span data-ttu-id="3eb9c-468">等效于以下声明</span><span class="sxs-lookup"><span data-stu-id="3eb9c-468">is equivalent to the following declaration</span></span>

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

<span data-ttu-id="3eb9c-469">在不指定委托类型的情况下声明事件是最简单且最简洁的语法，但具有为每个事件声明新委托类型的缺点。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-469">Declaring an event without specifying a delegate type is the simplest and most compact syntax, but has the disadvantage of declaring a new delegate type for each event.</span></span> <span data-ttu-id="3eb9c-470">例如，在下面的示例中，将创建三个隐藏的委托类型，即使所有三个事件都具有相同的参数列表：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-470">For example, in the following example, three hidden delegate types are created, even though all three events have the same parameter list:</span></span>

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

<span data-ttu-id="3eb9c-471">在下面的示例中，事件只使用同一个委托，`EventHandler`：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-471">In the following example, the events simply use the same delegate, `EventHandler`:</span></span>

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

<span data-ttu-id="3eb9c-472">可以通过以下两种方式之一来处理事件：静态或动态。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-472">Events can be handled in one of two ways: statically or dynamically.</span></span> <span data-ttu-id="3eb9c-473">静态处理事件更简单，只需要 `WithEvents` 变量和 `Handles` 子句。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-473">Statically handling events is simpler and only requires a `WithEvents` variable and a `Handles` clause.</span></span> <span data-ttu-id="3eb9c-474">在下面的示例中，类 `Form1` 静态处理对象 `Button` 的事件 @no__t：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-474">In the following example, class `Form1` statically handles the event `Click` of object `Button`:</span></span>

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

<span data-ttu-id="3eb9c-475">动态处理事件更复杂，因为必须显式连接事件，并在代码中断开与的连接。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-475">Dynamically handling events is more complex because the event must be explicitly connected and disconnected to in code.</span></span> <span data-ttu-id="3eb9c-476">语句 @no__t 为事件添加处理程序，语句 `RemoveHandler` 删除事件的处理程序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-476">The statement `AddHandler` adds a handler for an event, and the statement `RemoveHandler` removes a handler for an event.</span></span> <span data-ttu-id="3eb9c-477">下一个示例显示了一个类 `Form1`，它将 @no__t 添加为 @no__t 2 的 @no__t 事件的事件处理程序：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-477">The next example shows a class `Form1` that adds `Button1_Click` as an event handler for `Button1`'s `Click` event:</span></span>

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

<span data-ttu-id="3eb9c-478">在方法 `Disconnect` 中，将删除事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-478">In method `Disconnect`, the event handler is removed.</span></span>


### <a name="custom-events"></a><span data-ttu-id="3eb9c-479">自定义事件</span><span class="sxs-lookup"><span data-stu-id="3eb9c-479">Custom Events</span></span>

<span data-ttu-id="3eb9c-480">如上一节所述，事件声明隐式定义了一个字段、一个 @no__t 0 方法，以及一个用于跟踪事件处理程序的 @no__t 方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-480">As discussed in the previous section, event declarations implicitly define a field, an `add_` method, and a `remove_` method that are used to keep track of event handlers.</span></span> <span data-ttu-id="3eb9c-481">但在某些情况下，可能需要提供自定义代码来跟踪事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-481">In some situations, however, it may be desirable to provide custom code for tracking event handlers.</span></span> <span data-ttu-id="3eb9c-482">例如，如果某个类定义了只处理少量事件的40事件，则使用哈希表而不是40字段来跟踪每个事件的处理程序可能更高效。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-482">For example, if a class defines forty events of which only a few will ever be handled, using a hash table instead of forty fields to track the handlers for each event may be more efficient.</span></span> <span data-ttu-id="3eb9c-483">*自定义事件*允许显式定义 `add_X` 和 `remove_X` 方法，这为事件处理程序启用自定义存储。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-483">*Custom events* allow the `add_X` and `remove_X` methods to be defined explicitly, which enables custom storage for event handlers.</span></span>

<span data-ttu-id="3eb9c-484">声明自定义事件的方式与指定委托类型的事件的声明方式相同，但关键字 `Custom` 必须在 `Event` 关键字之前。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-484">Custom events are declared in the same way that events that specify a delegate type are declared, with the exception that the keyword `Custom` must precede the `Event` keyword.</span></span> <span data-ttu-id="3eb9c-485">自定义事件声明包含三个声明： `AddHandler` 声明、@no__t 声明和 @no__t 声明。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-485">A custom event declaration contains three declarations: an `AddHandler` declaration, a `RemoveHandler` declaration and a `RaiseEvent` declaration.</span></span> <span data-ttu-id="3eb9c-486">所有声明都不能具有任何修饰符，尽管它们可以具有属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-486">None of the declarations can have any modifiers, although they can have attributes.</span></span>

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

<span data-ttu-id="3eb9c-487">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-487">For example:</span></span>

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

<span data-ttu-id="3eb9c-488">@No__t，`RemoveHandler` 声明采用一个 @no__t 2 参数，该参数必须是该事件的委托类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-488">The `AddHandler` and `RemoveHandler` declaration take one `ByVal` parameter, which must be of the delegate type of the event.</span></span> <span data-ttu-id="3eb9c-489">当执行 `AddHandler` 或 @no__t 语句（或 `Handles` 子句自动处理事件）时，将调用相应的声明。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-489">When an `AddHandler` or `RemoveHandler` statement is executed (or a `Handles` clause automatically handles an event), the corresponding declaration will be called.</span></span> <span data-ttu-id="3eb9c-490">@No__t-0 声明采用与事件委托相同的参数，并将在执行 @no__t 1 语句时调用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-490">The `RaiseEvent` declaration takes the same parameters as the event delegate and will be called when a `RaiseEvent` statement is executed.</span></span> <span data-ttu-id="3eb9c-491">所有声明都必须提供并被视为子例程。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-491">All of the declarations must be provided and are considered to be subroutines.</span></span>

<span data-ttu-id="3eb9c-492">请注意，`AddHandler`、@no__t 和 `RaiseEvent` 声明在子例程具有的行位置上具有相同的限制。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-492">Note that `AddHandler`, `RemoveHandler` and `RaiseEvent` declarations have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="3eb9c-493">开始语句、结束语句和块必须全部显示在逻辑行的开头。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-493">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

<span data-ttu-id="3eb9c-494">除了添加到类型声明空间的成员名称外，自定义事件声明还会隐式声明多个其他成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-494">In addition to the member name added to the type's declaration space, a custom event declaration implicitly declares several other members.</span></span> <span data-ttu-id="3eb9c-495">假设有一个名为 `X` 的事件，则会将以下成员添加到声明空间：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-495">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="3eb9c-496">一个名为 `add_X` 的方法，对应于 `AddHandler` 声明。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-496">A method named `add_X`, corresponding to the `AddHandler` declaration.</span></span>

* <span data-ttu-id="3eb9c-497">一个名为 `remove_X` 的方法，对应于 `RemoveHandler` 声明。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-497">A method named `remove_X`, corresponding to the `RemoveHandler` declaration.</span></span>

* <span data-ttu-id="3eb9c-498">一个名为 `fire_X` 的方法，对应于 `RaiseEvent` 声明。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-498">A method named `fire_X`, corresponding to the `RaiseEvent` declaration.</span></span>

<span data-ttu-id="3eb9c-499">如果某个类型尝试声明一个与上述某个名称匹配的名称，则会导致编译时错误，并将忽略隐式声明，以便进行名称绑定。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-499">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit declarations are all ignored for the purposes of name binding.</span></span> <span data-ttu-id="3eb9c-500">不能重写或重载任何引入的成员，但可以在派生类型中隐藏它们。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-500">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span>

<span data-ttu-id="3eb9c-501">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-501">__Note.__</span></span> <span data-ttu-id="3eb9c-502">`Custom` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-502">`Custom` is not a reserved word.</span></span>

#### <a name="custom-events-in-winrt-assemblies"></a><span data-ttu-id="3eb9c-503">WinRT 程序集中的自定义事件</span><span class="sxs-lookup"><span data-stu-id="3eb9c-503">Custom events in WinRT assemblies</span></span>

<span data-ttu-id="3eb9c-504">从 Microsoft Visual Basic 11.0 开始，在使用 @no__t 编译的文件中声明的事件，或在此类文件中的接口中声明的事件，并在其他位置进行了其他处理。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-504">As of Microsoft Visual Basic 11.0, events declared in a file compiled with `/target:winmdobj`, or declared in an interface in such a file and then implemented elsewhere, are treated a little differently.</span></span>

* <span data-ttu-id="3eb9c-505">用于构建 winmd 的外部工具通常只允许某些委托类型，例如 `System.EventHandler(Of T)` 或 `System.TypedEventHandle(Of T, U)`，并且将禁止其他委托类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-505">External tools used to build the winmd will typically allow only certain delegate types such as `System.EventHandler(Of T)` or `System.TypedEventHandle(Of T, U)`, and will disallow others.</span></span>

* <span data-ttu-id="3eb9c-506">@No__t-0 字段具有类型 `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)`，其中 @no__t 为委托类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-506">The `XEvent` field has type `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` where `T` is the delegate type.</span></span>

* <span data-ttu-id="3eb9c-507">AddHandler 访问器返回 `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`，并且 RemoveHandler 访问器采用同一类型的单个参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-507">The AddHandler accessor returns a `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, and the RemoveHandler accessor takes a single parameter of the same type.</span></span>

<span data-ttu-id="3eb9c-508">下面是此类自定义事件的示例。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-508">Here is an example of such a custom event.</span></span>

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


## <a name="constants"></a><span data-ttu-id="3eb9c-509">常量</span><span class="sxs-lookup"><span data-stu-id="3eb9c-509">Constants</span></span>

<span data-ttu-id="3eb9c-510">*常量*是属于类型成员的常量值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-510">A *constant* is a constant value that is a member of a type.</span></span>

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

<span data-ttu-id="3eb9c-511">常量将被隐式共享。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-511">Constants are implicitly shared.</span></span> <span data-ttu-id="3eb9c-512">如果声明包含 `As` 子句，子句将指定声明引入的成员类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-512">If the declaration contains an `As` clause, the clause specifies the type of the member introduced by the declaration.</span></span> <span data-ttu-id="3eb9c-513">如果省略该类型，则将推断常量的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-513">If the type is omitted then the type of the constant is inferred.</span></span> <span data-ttu-id="3eb9c-514">常数的类型只能是基元类型或 `Object`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-514">The type of a constant may only be a primitive type or `Object`.</span></span> <span data-ttu-id="3eb9c-515">如果常量的类型为 `Object`，并且没有类型字符，则常数的实类型将是常量表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-515">If a constant is typed as `Object` and there is no type character, the real type of the constant will be the type of the constant expression.</span></span> <span data-ttu-id="3eb9c-516">否则，常数的类型为常数类型字符的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-516">Otherwise, the type of the constant is the type of the constant's type character.</span></span>

<span data-ttu-id="3eb9c-517">下面的示例演示一个名为 `Constants` 的类，该类具有两个公共常量：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-517">The following example shows a class named `Constants` that has two public constants:</span></span>

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

<span data-ttu-id="3eb9c-518">可以通过类访问常量，如以下示例中所示，它输出 `Constants.A` 和 `Constants.B` 的值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-518">Constants can be accessed through the class, as in the following example, which prints out the values of `Constants.A` and `Constants.B`.</span></span>

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

<span data-ttu-id="3eb9c-519">声明多个常量的常量声明等效于单个常量的多个声明。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-519">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants.</span></span> <span data-ttu-id="3eb9c-520">下面的示例在一个声明语句中声明了三个常量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-520">The following example declares three constants in one declaration statement.</span></span>

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

<span data-ttu-id="3eb9c-521">此声明等效于以下内容：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-521">This declaration is equivalent to the following:</span></span>

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

<span data-ttu-id="3eb9c-522">常量类型的可访问域必须与常量本身的可访问域的可访问域相同或超集。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-522">The accessibility domain of the type of the constant must be the same as or a superset of the accessibility domain of the constant itself.</span></span> <span data-ttu-id="3eb9c-523">常数表达式必须产生常数的类型或可隐式转换为常量类型的类型的值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-523">The constant expression must yield a value of the constant's type or of a type that is implicitly convertible to the constant's type.</span></span> <span data-ttu-id="3eb9c-524">常数表达式不能是循环的;也就是说，不能以本身来定义常量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-524">The constant expression may not be circular; that is, a constant may not be defined in terms of itself.</span></span>

<span data-ttu-id="3eb9c-525">编译器会按适当的顺序自动计算常量声明。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-525">The compiler automatically evaluates the constant declarations in the appropriate order.</span></span> <span data-ttu-id="3eb9c-526">在下面的示例中，编译器首先计算 `Y`，然后 `Z`，最后 `X`，分别生成值10、11和12。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-526">In the following example, the compiler first evaluates `Y`, then `Z`, and finally `X`, producing the values 10, 11, and 12, respectively.</span></span>

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

<span data-ttu-id="3eb9c-527">如果需要常数值的符号名称，但是不允许在常数声明中使用该值的类型，或者如果不能在编译时通过常数表达式计算该值，则可以改为使用只读变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-527">When a symbolic name for a constant value is desired, but the type of the value is not permitted in a constant declaration or when the value cannot be computed at compile time by a constant expression, a read-only variable may be used instead.</span></span>


## <a name="instance-and-shared-variables"></a><span data-ttu-id="3eb9c-528">实例和共享变量</span><span class="sxs-lookup"><span data-stu-id="3eb9c-528">Instance and Shared Variables</span></span>

<span data-ttu-id="3eb9c-529">实例或共享变量是可存储信息的类型的成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-529">An instance or shared variable is a member of a type that can store information.</span></span>

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

<span data-ttu-id="3eb9c-530">如果未指定任何修饰符，则必须指定 `Dim` 修饰符，否则可省略。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-530">The `Dim` modifier must be specified if no modifiers are specified, but may be omitted otherwise.</span></span> <span data-ttu-id="3eb9c-531">单个变量声明可能包含多个变量声明符;每个变量声明符引入一个新的实例或共享成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-531">A single variable declaration may include multiple variable declarators; each variable declarator introduces a new instance or shared member.</span></span>

<span data-ttu-id="3eb9c-532">如果指定初始值设定项，则变量声明符只能声明一个实例或共享变量：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-532">If an initializer is specified, only one instance or shared variable may be declared by the variable declarator:</span></span>

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

<span data-ttu-id="3eb9c-533">此限制不适用于对象初始值设定项：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-533">This restriction does not apply to object initializers:</span></span>

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

<span data-ttu-id="3eb9c-534">使用 `Shared` 修饰符声明的变量是*共享变量*。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-534">A variable declared with the `Shared` modifier is a *shared variable*.</span></span> <span data-ttu-id="3eb9c-535">共享变量只标识一个存储位置，而不考虑创建的类型实例数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-535">A shared variable identifies exactly one storage location regardless of the number of instances of the type that are created.</span></span> <span data-ttu-id="3eb9c-536">当程序开始执行时，共享变量就会存在，在程序终止时停止存在。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-536">A shared variable comes into existence when a program begins executing, and ceases to exist when the program terminates.</span></span>

<span data-ttu-id="3eb9c-537">共享变量仅在特定封闭式泛型类型的实例之间共享。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-537">A shared variable is shared only among instances of a particular closed generic type.</span></span> <span data-ttu-id="3eb9c-538">例如，程序：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-538">For example, the program:</span></span>

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

<span data-ttu-id="3eb9c-539">打印输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-539">Prints out:</span></span>

```console
1
1
2
```

<span data-ttu-id="3eb9c-540">在没有 `Shared` 修饰符的情况下声明的变量称为*实例变量*。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-540">A variable declared without the `Shared` modifier is called an *instance variable*.</span></span> <span data-ttu-id="3eb9c-541">类的每个实例都包含该类的所有实例变量的单独副本。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-541">Every instance of a class contains a separate copy of all instance variables of the class.</span></span> <span data-ttu-id="3eb9c-542">当创建该类型的新实例时，引用类型的实例变量就会成为存在，如果没有对该实例的引用，并且已执行 `Finalize` 方法，则不再存在。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-542">An instance variable of a reference type comes into existence when a new instance of that type is created, and ceases to exist when there are no references to that instance and the `Finalize` method has executed.</span></span> <span data-ttu-id="3eb9c-543">值类型的实例变量与它所属的变量具有完全相同的生存期。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-543">An instance variable of a value type has exactly the same lifetime as the variable to which it belongs.</span></span> <span data-ttu-id="3eb9c-544">换言之，当某个值类型的变量变为存在或不存在时，将执行值类型的实例变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-544">In other words, when a variable of a value type comes into existence or ceases to exist, so does the instance variable of the value type.</span></span>

<span data-ttu-id="3eb9c-545">如果声明符包含 `As` 子句，子句将指定声明引入的成员类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-545">If the declarator contains an `As` clause, the clause specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="3eb9c-546">如果省略该类型并使用了严格语义，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-546">If the type is omitted and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="3eb9c-547">否则，成员的类型将隐式 `Object` 或成员类型字符的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-547">Otherwise the type of the members is implicitly `Object` or the type of the members' type character.</span></span>

<span data-ttu-id="3eb9c-548">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-548">__Note.__</span></span> <span data-ttu-id="3eb9c-549">语法中没有歧义：如果声明符省略了类型，它将始终使用以下声明符的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-549">There is no ambiguity in the syntax: if a declarator omits a type, it will always use the type of a following declarator.</span></span>

<span data-ttu-id="3eb9c-550">实例或共享变量的类型或数组元素类型的可访问域必须与实例或共享变量本身的可访问域的可访问域相同或超集。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-550">The accessibility domain of an instance or shared variable's type or array element type must be the same as or a superset of the accessibility domain of the instance or shared variable itself.</span></span>

<span data-ttu-id="3eb9c-551">下面的示例演示了一个 @no__t 0 类，该类具有名为 `redPart`、@no__t 和 @no__t 的内部实例变量：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-551">The following example shows a `Color` class that has internal instance variables named `redPart`, `greenPart`, and `bluePart`:</span></span>

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


### <a name="read-only-variables"></a><span data-ttu-id="3eb9c-552">只读变量</span><span class="sxs-lookup"><span data-stu-id="3eb9c-552">Read-Only Variables</span></span>

<span data-ttu-id="3eb9c-553">当某个实例或共享变量声明包含 `ReadOnly` 修饰符时，对声明引入的变量的赋值只能作为声明的一部分出现，或者出现在同一类的构造函数中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-553">When an instance or shared variable declaration includes a `ReadOnly` modifier, assignments to the variables introduced by the declaration may only occur as part of the declaration or in a constructor in the same class.</span></span> <span data-ttu-id="3eb9c-554">具体而言，在以下情况下，只允许对只读实例或共享变量进行赋值：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-554">Specifically, assignments to a read-only instance or shared variable are permitted only in the following situations:</span></span>

* <span data-ttu-id="3eb9c-555">在引入实例或共享变量的变量声明中（通过在声明中包含变量初始值设定项）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-555">In the variable declaration that introduces the instance or shared variable (by including a variable initializer in the declaration).</span></span>

* <span data-ttu-id="3eb9c-556">对于实例变量，在包含变量声明的类的实例构造函数中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-556">For an instance variable, in the instance constructors of the class that contains the variable declaration.</span></span> <span data-ttu-id="3eb9c-557">只能以非限定的方式或通过 `Me` 或 `MyClass` 来访问实例变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-557">The instance variable can only be accessed in an unqualified manner or through `Me` or `MyClass`.</span></span>

* <span data-ttu-id="3eb9c-558">对于共享变量，在包含共享变量声明的类的共享构造函数中。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-558">For a shared variable, in the shared constructor of the class that contains the shared variable declaration.</span></span>

<span data-ttu-id="3eb9c-559">如果需要常数值的符号名称，但不允许在常数声明中使用该类型的值，或者不能通过常量表达式在编译时计算该值，则共享只读变量非常有用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-559">A shared read-only variable is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a constant declaration, or when the value cannot be computed at compile time by a constant expression.</span></span>

<span data-ttu-id="3eb9c-560">下面是第一个这样的应用程序的示例，其中，颜色共享变量 @no__t 声明为-0，以防止其他程序更改它们：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-560">An example of the first such application follows, in which color shared variables are declared `ReadOnly` to prevent them from being changed by other programs:</span></span>

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

<span data-ttu-id="3eb9c-561">常量和只读共享变量具有不同的语义。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-561">Constants and read-only shared variables have different semantics.</span></span> <span data-ttu-id="3eb9c-562">当表达式引用常量时，将在编译时获取常量的值，但当表达式引用只读共享变量时，在运行时之前不会获取共享变量的值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-562">When an expression references a constant, the value of the constant is obtained at compile time, but when an expression references a read-only shared variable, the value of the shared variable is not obtained until run time.</span></span> <span data-ttu-id="3eb9c-563">请考虑以下应用程序，其中包含两个单独的程序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-563">Consider the following application, which consists of two separate programs.</span></span>

<span data-ttu-id="3eb9c-564">file1：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-564">file1.vb:</span></span>

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

<span data-ttu-id="3eb9c-565">file2：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-565">file2.vb:</span></span>

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="3eb9c-566">命名空间 `Program1`，`Program2` 表示单独编译两个程序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-566">The namespaces `Program1` and `Program2` denote two programs that are compiled separately.</span></span> <span data-ttu-id="3eb9c-567">由于变量 `Program1.Utils.X` 声明为 `Shared ReadOnly`，因此在编译时，`Console.WriteLine` 语句输出的值是未知的，而是在运行时获取。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-567">Because variable `Program1.Utils.X` is declared as `Shared ReadOnly`, the value output by the `Console.WriteLine` statement is not known at compile time, but rather is obtained at run time.</span></span> <span data-ttu-id="3eb9c-568">因此，如果更改了 `X` 的值并重新编译了 `Program1`，则即使未重新编译 `Console.WriteLine` 语句，@no__t 也将输出新值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-568">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` is not recompiled.</span></span> <span data-ttu-id="3eb9c-569">但是，如果 @no__t 为常量，则在编译 `Program2` 时，将获得 `X` 的值，并且在重新编译 @no__t 之前，将不会影响 `Program1` 中的更改。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-569">However, if `X` had been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would have remained unaffected by changes in `Program1` until `Program2` was recompiled.</span></span>

### <a name="withevents-variables"></a><span data-ttu-id="3eb9c-570">WithEvents 变量</span><span class="sxs-lookup"><span data-stu-id="3eb9c-570">WithEvents Variables</span></span>

<span data-ttu-id="3eb9c-571">类型可以声明它处理通过使用 `WithEvents` 修饰符引发事件的实例或共享变量来处理其实例或共享变量之一所引发的一组事件。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-571">A type can declare that it handles some set of events raised by one of its instance or shared variables by declaring the instance or shared variable that raises the events with the `WithEvents` modifier.</span></span> <span data-ttu-id="3eb9c-572">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-572">For example:</span></span>

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

<span data-ttu-id="3eb9c-573">在此示例中，方法 `E1Handler` 处理事件 `E1`，该事件由存储在实例变量 @no__t 中的类型 `Raiser` 的实例引发。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-573">In this example, the method `E1Handler` handles the event `E1` that is raised by the instance of the type `Raiser` stored in the instance variable `x`.</span></span>

<span data-ttu-id="3eb9c-574">@No__t 的修饰符导致使用前导下划线重命名变量，并将其替换为与事件挂钩同名的属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-574">The `WithEvents` modifier causes the variable to be renamed with a leading underscore and replaced with a property of the same name that does the event hookup.</span></span> <span data-ttu-id="3eb9c-575">例如，如果变量的名称为 `F`，则将其重命名为 `_F`，并且隐式声明一个属性 `F`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-575">For example, if the variable's name is `F`, it is renamed to `_F` and a property `F` is implicitly declared.</span></span> <span data-ttu-id="3eb9c-576">如果变量的新名称和其他声明之间存在冲突，则将报告编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-576">If there is a collision between the variable's new name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="3eb9c-577">应用到变量的任何属性都将传送到已重命名的变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-577">Any attributes applied to the variable are carried over to the renamed variable.</span></span>

<span data-ttu-id="3eb9c-578">@No__t 声明创建的隐式属性负责挂钩和解除相关的事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-578">The implicit property created by a `WithEvents` declaration takes care of hooking and unhooking the relevant event handlers.</span></span> <span data-ttu-id="3eb9c-579">将值分配给变量时，属性将首先对变量中当前实例上的事件调用 @no__t 的方法（如果有），则为解除。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-579">When a value is assigned to the variable, the property first calls the `remove` method for the event on the instance currently in the variable (unhooking the existing event handler, if any).</span></span> <span data-ttu-id="3eb9c-580">接下来，将进行分配，并且属性将为变量中的新实例上的事件调用 @no__t 的方法（正在挂钩新的事件处理程序）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-580">Next the assignment is made, and the property calls the `add` method for the event on the new instance in the variable (hooking up the new event handler).</span></span> <span data-ttu-id="3eb9c-581">下面的代码等效于标准模块 @no__t 的代码-0：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-581">The following code is equivalent to the code above for the standard module `Test`:</span></span>

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

<span data-ttu-id="3eb9c-582">如果将实例或共享变量声明为结构，则将该变量声明为 `WithEvents` 是无效的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-582">It is not valid to declare an instance or shared variable as `WithEvents` if the variable is typed as a structure.</span></span> <span data-ttu-id="3eb9c-583">此外，不能在结构中指定 `WithEvents`，并且不能将 `ReadOnly` 与 @no__t 组合。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-583">In addition, `WithEvents` may not be specified in a structure, and `WithEvents` and `ReadOnly` cannot be combined.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="3eb9c-584">变量初始值设定项</span><span class="sxs-lookup"><span data-stu-id="3eb9c-584">Variable Initializers</span></span>

<span data-ttu-id="3eb9c-585">结构中的类和实例变量声明（而不是共享变量声明）中的实例和共享变量声明可能包含变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-585">Instance and shared variable declarations in classes and instance variable declarations (but not shared variable declarations) in structures may include variable initializers.</span></span> <span data-ttu-id="3eb9c-586">对于 @no__t 0 变量，变量初始值设定项对应于在程序开始后、第一次引用第一次引用 @no__t 之前执行的赋值语句。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-586">For `Shared` variables, variable initializers correspond to assignment statements that are executed after the program begins, but before the `Shared` variable is first referenced.</span></span> <span data-ttu-id="3eb9c-587">对于实例变量，变量初始值设定项对应于创建类的实例时执行的赋值语句。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-587">For instance variables, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span> <span data-ttu-id="3eb9c-588">结构不能具有实例变量初始值设定项，因为无法修改其无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-588">Structures cannot have instance variable initializers because their parameterless constructors cannot be modified.</span></span>

<span data-ttu-id="3eb9c-589">请看下面的示例：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-589">Consider the following example:</span></span>

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

<span data-ttu-id="3eb9c-590">此示例产生以下输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-590">The example produces the following output:</span></span>

```console
x = 1.4142135623731, i = 100, s = Hello
```

<span data-ttu-id="3eb9c-591">在加载类时，将分配到 `x`，并在创建类的新实例时，将分配给 `i` 和 `s`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-591">An assignment to `x` occurs when the class is loaded, and assignments to `i` and `s` occur when a new instance of the class is created.</span></span>

<span data-ttu-id="3eb9c-592">将变量初始值设定项视为自动插入到类型构造函数的块中的赋值语句是非常有用的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-592">It is useful to think of variable initializers as assignment statements that are automatically inserted in the block of the type's constructor.</span></span> <span data-ttu-id="3eb9c-593">下面的示例包含多个实例变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-593">The following example contains several instance variable initializers.</span></span>

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

<span data-ttu-id="3eb9c-594">该示例与下面显示的代码相对应，其中每个注释指示一个自动插入的语句。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-594">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement.</span></span>

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

<span data-ttu-id="3eb9c-595">在执行任何变量初始值设定项之前，所有变量都将初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-595">All variables are initialized to the default value of their type before any variable initializers are executed.</span></span> <span data-ttu-id="3eb9c-596">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-596">For example:</span></span>

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

<span data-ttu-id="3eb9c-597">由于在类加载时 `b` 会自动初始化为其默认值，因此在创建类的实例时，`i` 会自动初始化为其默认值，前面的代码生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-597">Because `b` is automatically initialized to its default value when the class is loaded and `i` is automatically initialized to its default value when an instance of the class is created, the preceding code produces the following output:</span></span>

```console
b = False, i = 0
```

<span data-ttu-id="3eb9c-598">每个变量初始值设定项必须生成变量类型或可隐式转换为变量类型的类型的值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-598">Each variable initializer must yield a value of the variable's type or of a type that is implicitly convertible to the variable's type.</span></span> <span data-ttu-id="3eb9c-599">变量初始值设定项可以是循环的，也可以引用将在其后面进行初始化的变量（在这种情况下，被引用变量的值为初始值设定项的默认值）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-599">A variable initializer may be circular or refer to a variable that will be initialized after it, in which case the value of the referenced variable is its default value for the purposes of the initializer.</span></span> <span data-ttu-id="3eb9c-600">此类初始值设定项为可疑值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-600">Such an initializer is of dubious value.</span></span>

<span data-ttu-id="3eb9c-601">有三种形式的变量初始值设定项：常规初始值设定项、数组大小初始值设定项和对象初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-601">There are three forms of variable initializers: regular initializers, array-size initializers, and object initializers.</span></span> <span data-ttu-id="3eb9c-602">前两个窗体出现在类型名称后的等号之后，后者是声明自身的一部分。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-602">The first two forms appear after an equal sign that follows the type name, the latter two are part of the declaration itself.</span></span> <span data-ttu-id="3eb9c-603">对于任何特定声明，只能使用一种形式的初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-603">Only one form of initializer may be used on any particular declaration.</span></span>

#### <a name="regular-initializers"></a><span data-ttu-id="3eb9c-604">正则表达式</span><span class="sxs-lookup"><span data-stu-id="3eb9c-604">Regular Initializers</span></span>

<span data-ttu-id="3eb9c-605">*正则初始值设定项*是可隐式转换为变量类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-605">A *regular initializer* is an expression that is implicitly convertible to the type of the variable.</span></span> <span data-ttu-id="3eb9c-606">它出现在类型名称后的等号之后，并且必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-606">It appears after an equal sign that follows the type name and must be classified as a value.</span></span> <span data-ttu-id="3eb9c-607">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-607">For example:</span></span>

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

<span data-ttu-id="3eb9c-608">此程序生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-608">This program produces the output:</span></span>

```console
x = 10, y = 20
```

<span data-ttu-id="3eb9c-609">如果变量声明有一个规则初始值设定项，则一次只能声明一个变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-609">If a variable declaration has a regular initializer, then only a single variable can be declared at a time.</span></span> <span data-ttu-id="3eb9c-610">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-610">For example:</span></span>

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

#### <a name="object-initializers"></a><span data-ttu-id="3eb9c-611">对象初始值设定项</span><span class="sxs-lookup"><span data-stu-id="3eb9c-611">Object Initializers</span></span>

<span data-ttu-id="3eb9c-612">*对象初始值设定项*是使用对象创建表达式在类型名称的位置指定的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-612">An *object initializer* is specified using an object creation expression in the place of the type name.</span></span> <span data-ttu-id="3eb9c-613">对象初始值设定项等效于将对象创建表达式的结果分配给变量的常规初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-613">An object initializer is equivalent to a regular initializer assigning the result of the object creation expression to the variable.</span></span> <span data-ttu-id="3eb9c-614">因此，</span><span class="sxs-lookup"><span data-stu-id="3eb9c-614">So</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

<span data-ttu-id="3eb9c-615">等效于</span><span class="sxs-lookup"><span data-stu-id="3eb9c-615">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

<span data-ttu-id="3eb9c-616">对象初始值设定项中的括号始终解释为构造函数的参数列表，而不是数组类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-616">The parenthesis in an object initializer is always interpreted as the argument list for the constructor and never as array type modifiers.</span></span> <span data-ttu-id="3eb9c-617">使用对象初始值设定项的变量名称不能具有数组类型修饰符或可以为 null 的类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-617">A variable name with an object initializer cannot have an array type modifier or a nullable type modifier.</span></span>

#### <a name="array-size-initializers"></a><span data-ttu-id="3eb9c-618">数组大小初始值设定项</span><span class="sxs-lookup"><span data-stu-id="3eb9c-618">Array-Size Initializers</span></span>

<span data-ttu-id="3eb9c-619">*数组大小初始值设定项*是变量名称的修饰符，它提供一组由表达式表示的维度上限。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-619">An *array-size initializer* is a modifier on the name of the variable that gives a set of dimension upper bounds denoted by expressions.</span></span>

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

<span data-ttu-id="3eb9c-620">上限表达式必须分类为值，并且必须能够隐式转换为 `Integer`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-620">The upper bound expressions must be classified as values and must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="3eb9c-621">上限集等效于具有给定上限的数组创建表达式的变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-621">The set of upper bounds is equivalent to a variable initializer of an array-creation expression with the given upper bounds.</span></span> <span data-ttu-id="3eb9c-622">数组类型的维数是从数组大小初始值设定项推断出来的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-622">The number of dimensions of the array type is inferred from the array size initializer.</span></span> <span data-ttu-id="3eb9c-623">因此，</span><span class="sxs-lookup"><span data-stu-id="3eb9c-623">So</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

<span data-ttu-id="3eb9c-624">等效于</span><span class="sxs-lookup"><span data-stu-id="3eb9c-624">is equivalent to</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

<span data-ttu-id="3eb9c-625">所有上限必须等于或大于-1，并且所有维度都必须具有指定的上限。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-625">All upper bounds must be equal to or greater than -1, and all dimensions must have an upper bound specified.</span></span> <span data-ttu-id="3eb9c-626">如果要初始化的数组的元素类型本身是数组类型，则数组类型修饰符将会跳到数组大小初始值设定项的右侧。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-626">If the element type of the array being initialized is itself an array type, the array-type modifiers go to the right of the array-size initializer.</span></span> <span data-ttu-id="3eb9c-627">例如</span><span class="sxs-lookup"><span data-stu-id="3eb9c-627">For example</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

<span data-ttu-id="3eb9c-628">声明一个局部变量 `x`，该变量的类型是一维数组，该数组的类型为三 @no__t 维数组，其类型为第一个维度中 `0..5` @no__t 和第二个维度中的-2 的数组。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-628">declares a local variable `x` whose type is a two-dimensional array of three-dimensional arrays of `Integer`, initialized to an array with bounds of `0..5` in the first dimension and `0..10` in the second dimension.</span></span> <span data-ttu-id="3eb9c-629">不能使用数组大小初始值设定项来初始化其类型为数组的数组的变量元素。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-629">It is not possible to use an array size initializer to initialize the elements of a variable whose type is an array of arrays.</span></span>

<span data-ttu-id="3eb9c-630">具有数组大小初始值设定项的变量声明不能对其类型或常规初始值设定项使用数组类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-630">A variable declaration with an array-size initializer cannot have an array type modifier on its type or a regular initializer.</span></span>


### <a name="systemmarshalbyrefobject-classes"></a><span data-ttu-id="3eb9c-631">MarshalByRefObject 类</span><span class="sxs-lookup"><span data-stu-id="3eb9c-631">System.MarshalByRefObject Classes</span></span>

<span data-ttu-id="3eb9c-632">派生自类 `System.MarshalByRefObject` 的类将使用代理（即按引用）在上下文边界之间进行封送，而不是通过复制（即通过值）进行封送处理。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-632">Classes that derive from the class `System.MarshalByRefObject` are marshaled across context boundaries using proxies (that is, by reference) rather than through copying (that is, by value).</span></span> <span data-ttu-id="3eb9c-633">这意味着此类类的实例可能不是真正的实例，而可能只是在上下文边界封送变量访问和方法调用的存根。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-633">This means that an instance of such a class may not be a true instance but instead may just be a stub that marshals variable accesses and method calls across a context boundary.</span></span>

<span data-ttu-id="3eb9c-634">因此，不能创建对此类类中定义的变量的存储位置的引用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-634">As a result, it is not possible to create a reference to the storage location of variables defined on such classes.</span></span> <span data-ttu-id="3eb9c-635">这意味着，不能将类型化为派生自 `System.MarshalByRefObject` 的类的变量传递到引用参数，也不能访问类型化为值类型的变量的方法和变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-635">This means that variables typed as classes derived from `System.MarshalByRefObject` cannot be passed to reference parameters, and methods and variables of variables typed as value types may not be accessed.</span></span> <span data-ttu-id="3eb9c-636">相反，Visual Basic 将此类类上定义的变量视为属性（因为这些限制在属性上是相同的）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-636">Instead, Visual Basic treats variables defined on such classes as if they were properties (since the restrictions are the same on properties).</span></span>

<span data-ttu-id="3eb9c-637">此规则有一个例外：使用 @no__t 的隐式或显式限定的成员不受上述限制，因为 `Me` 始终保证是实际的对象，而不是代理。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-637">There is one exception to this rule: a member implicitly or explicitly qualified with `Me` is exempt from the above restrictions, because `Me` is always guaranteed to be an actual object, not a proxy.</span></span>

## <a name="properties"></a><span data-ttu-id="3eb9c-638">属性</span><span class="sxs-lookup"><span data-stu-id="3eb9c-638">Properties</span></span>

<span data-ttu-id="3eb9c-639">*属性*是变量的自然扩展;两者都是具有关联类型的命名成员，并且用于访问变量和属性的语法相同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-639">*Properties* are a natural extension of variables; both are named members with associated types, and the syntax for accessing variables and properties is the same.</span></span> <span data-ttu-id="3eb9c-640">但与变量不同，属性不表示存储位置。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-640">Unlike variables, however, properties do not denote storage locations.</span></span> <span data-ttu-id="3eb9c-641">相反，属性具有*访问器*，它指定要执行的语句，以便读取或写入其值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-641">Instead, properties have *accessors*, which specify the statements to execute in order to read or write their values.</span></span>

<span data-ttu-id="3eb9c-642">属性通过属性声明进行定义。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-642">Properties are defined with property declarations.</span></span> <span data-ttu-id="3eb9c-643">属性声明的第一部分类似于字段声明。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-643">The first part of a property declaration resembles a field declaration.</span></span> <span data-ttu-id="3eb9c-644">第二部分包括 @no__t 的访问器和/或 @no__t 访问器。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-644">The second part includes a `Get` accessor and/or a `Set` accessor.</span></span>

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

<span data-ttu-id="3eb9c-645">在下面的示例中，`Button` 类定义 @no__t 1 属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-645">In the example below, the `Button` class defines a `Caption` property.</span></span>

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

<span data-ttu-id="3eb9c-646">根据上面的 `Button` 类，以下是使用 @no__t 属性的示例：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-646">Based on the `Button` class above, the following is an example of use of the `Caption` property:</span></span>

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

<span data-ttu-id="3eb9c-647">此处，通过将值分配给属性来调用 `Set` 访问器，通过引用表达式中的属性来调用 @no__t 1 访问器。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-647">Here, the `Set` accessor is invoked by assigning a value to the property, and the `Get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="3eb9c-648">如果没有为属性指定类型，并且使用的是严格语义，则会发生编译时错误;否则，属性的类型将隐式 `Object` 或属性类型字符的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-648">If no type is specified for a property and strict semantics are being used, a compile-time error occurs; otherwise the type of the property is implicitly `Object` or the type of the property's type character.</span></span> <span data-ttu-id="3eb9c-649">属性声明可以包含 `Get` 取值函数，该访问器用于检索属性的值、@no__t 1 访问器（存储属性的值）或同时用于这两者。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-649">A property declaration may contain either a `Get` accessor, which retrieves the value of the property, a `Set` accessor, which stores the value of the property, or both.</span></span> <span data-ttu-id="3eb9c-650">由于属性隐式声明方法，因此可以使用与方法相同的修饰符来声明属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-650">Because a property implicitly declares methods, a property may be declared with the same modifiers as a method.</span></span> <span data-ttu-id="3eb9c-651">如果在接口中定义该属性或使用 `MustOverride` 修饰符定义该属性，则必须省略属性体和 @no__t 构造;否则，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-651">If the property is defined in an interface or defined with the `MustOverride` modifier, the property body and the `End` construct must be omitted; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="3eb9c-652">索引参数列表构成属性的签名，因此属性可能会在索引参数上重载，而不能在属性的类型上重载。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-652">The index parameter list makes up the signature of the property, so properties may be overloaded on index parameters but not on the type of the property.</span></span> <span data-ttu-id="3eb9c-653">索引参数列表与常规方法相同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-653">The index parameter list is the same as for a regular method.</span></span> <span data-ttu-id="3eb9c-654">但是，不能用 `ByRef` 修饰符修改任何参数，也不能将任何参数命名为 `Value` （在 `Set` 访问器中为隐式值参数保留）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-654">However, none of the parameters may be modified with the `ByRef` modifier and none of them may be named `Value` (which is reserved for the implicit value parameter in the `Set` accessor).</span></span>

<span data-ttu-id="3eb9c-655">可按如下所示声明属性：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-655">A property may be declared as follows:</span></span>

* <span data-ttu-id="3eb9c-656">如果属性未指定属性类型修饰符，则该属性必须同时具有 `Get` 访问器和 @no__t。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-656">If the property specifies no property type modifier, the property must have both a `Get` accessor and a `Set` accessor.</span></span> <span data-ttu-id="3eb9c-657">此属性被称为读写属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-657">The property is said to be a read-write property.</span></span>

* <span data-ttu-id="3eb9c-658">如果该属性指定 `ReadOnly` 修饰符，则该属性必须具有 @no__t 访问器，并且不能具有 @no__t 2 访问器。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-658">If the property specifies the `ReadOnly` modifier, the property must have a `Get` accessor and may not have a `Set` accessor.</span></span> <span data-ttu-id="3eb9c-659">此属性被称为只读属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-659">The property is said to be read-only property.</span></span> <span data-ttu-id="3eb9c-660">只读属性是赋值的目标时，这是一个编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-660">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>

* <span data-ttu-id="3eb9c-661">如果该属性指定 `WriteOnly` 修饰符，则该属性必须具有 @no__t 访问器，并且不能具有 @no__t 2 访问器。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-661">If the property specifies the `WriteOnly` modifier, the property must have a `Set` accessor and may not have a `Get` accessor.</span></span> <span data-ttu-id="3eb9c-662">此属性被称为只写属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-662">The property is said to be write-only property.</span></span> <span data-ttu-id="3eb9c-663">在表达式中引用只写属性（作为赋值的目标或作为方法的参数）时，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-663">It is a compile-time error to reference a write-only property in an expression except as the target of an assignment or as an argument to a method.</span></span>

<span data-ttu-id="3eb9c-664">属性的 @no__t 和 @no__t 访问器不是不同的成员，并且不能单独声明属性的访问器。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-664">The `Get` and `Set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="3eb9c-665">下面的示例未声明单个读写属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-665">The following example does not declare a single read-write property.</span></span> <span data-ttu-id="3eb9c-666">相反，它声明了两个具有相同名称的属性，一个为只读，一个只写：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-666">Rather, it declares two properties with the same name, one read-only and one write-only:</span></span>

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

<span data-ttu-id="3eb9c-667">因为在同一个类中声明的两个成员不能具有相同的名称，所以该示例会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-667">Since two members declared in the same class cannot have the same name, the example causes a compile-time error.</span></span>

<span data-ttu-id="3eb9c-668">默认情况下，属性的 `Get` 和 `Set` 访问器的可访问性与属性本身的可访问性相同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-668">By default, the accessibility of a property's `Get` and `Set` accessors is the same as the accessibility of the property itself.</span></span> <span data-ttu-id="3eb9c-669">但 @no__t @no__t 的访问器也可以从属性单独指定可访问性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-669">However, the `Get` and `Set` accessors can also specify accessibility separately from the property.</span></span> <span data-ttu-id="3eb9c-670">在这种情况下，访问器的可访问性必须比属性的可访问性更严格，而且只有一个访问器可以具有与属性不同的可访问性级别。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-670">In that case, the accessibility of an accessor must be more restrictive than the accessibility of the property and only one accessor can have a different accessibility level from the property.</span></span> <span data-ttu-id="3eb9c-671">访问类型被视为更多或更少的限制，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-671">Access types are considered more or less restrictive as follows:</span></span>

* <span data-ttu-id="3eb9c-672">`Private` 比 `Public`、`Protected Friend`、`Protected` 或 @no__t 的限制更严格。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-672">`Private` is more restrictive than `Public`, `Protected Friend`, `Protected`, or `Friend`.</span></span>

* <span data-ttu-id="3eb9c-673">`Friend` 比 @no__t 或 @no__t 的限制更严格。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-673">`Friend` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="3eb9c-674">`Protected` 比 @no__t 或 @no__t 的限制更严格。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-674">`Protected` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="3eb9c-675">`Protected Friend` 比 `Public` 的限制更强。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-675">`Protected Friend` is more restrictive than `Public`.</span></span>

<span data-ttu-id="3eb9c-676">当属性的访问器之一可访问，而另一个访问器不是可访问的时，该属性将视为只读或只写。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-676">When one of a property's accessors is accessible but the other one is not, the property is treated as if it was read-only or write-only.</span></span> <span data-ttu-id="3eb9c-677">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-677">For example:</span></span>

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

<span data-ttu-id="3eb9c-678">当派生类型隐藏属性时，派生属性将隐藏与读取和写入相关的隐藏属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-678">When a derived type shadows a property, the derived property hides the shadowed property with respect to both reading and writing.</span></span> <span data-ttu-id="3eb9c-679">在下面的示例中，`B` 中的 `P` 属性隐藏 `A` 中与读取和写入有关的 `P` 属性：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-679">In the following example, the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing:</span></span>

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

<span data-ttu-id="3eb9c-680">返回类型或参数类型的可访问域必须与属性本身的可访问域的可访问域相同或超集。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-680">The accessibility domain of the return type or parameter types must be the same as or a superset of the accessibility domain of the property itself.</span></span> <span data-ttu-id="3eb9c-681">一个属性只能有一个 `Set` 访问器和一个 @no__t 的访问器。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-681">A property may only have one `Set` accessor and one `Get` accessor.</span></span>

<span data-ttu-id="3eb9c-682">除了声明和调用语法中的差异外，`Overridable`、`NotOverridable`、`Overrides`、`MustOverride` 和 @no__t 属性的行为与 `Overridable`、`NotOverridable`、`Overrides`、`MustOverride` 和 @no__t 9 方法完全相同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-682">Except for differences in declaration and invocation syntax, `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` properties behave exactly like `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` methods.</span></span> <span data-ttu-id="3eb9c-683">当重写属性时，重写属性的类型必须相同（读写、只读、只写）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-683">When a property is overridden, the overriding property must be of the same type (read-write, read-only, write-only).</span></span> <span data-ttu-id="3eb9c-684">@No__t-0 属性不能包含 @no__t 1 访问器。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-684">An `Overridable` property cannot contain a `Private` accessor.</span></span>

<span data-ttu-id="3eb9c-685">在下面的示例中 `X` 是一个 @no__t 为只读属性，`Y` 是一个 @no__t 的读写属性，而 @no__t 为 @no__t 读写属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-685">In the following example `X` is an `Overridable` read-only property, `Y` is an `Overridable` read-write property, and `Z` is a `MustOverride` read-write property.</span></span>

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

<span data-ttu-id="3eb9c-686">因为 `Z` @no__t 为-1，所以必须将包含类 `A` @no__t 声明为3。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-686">Because `Z` is `MustOverride`, the containing class `A` must be declared `MustInherit`.</span></span>

<span data-ttu-id="3eb9c-687">与此相反，派生自类 `A` 的类如下所示：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-687">By contrast, a class that derives from class `A` is shown below:</span></span>

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

<span data-ttu-id="3eb9c-688">此处，属性的声明 `X`、`Y` 和 `Z` 替代基属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-688">Here, the declarations of properties `X`,`Y`, and `Z` override the base properties.</span></span> <span data-ttu-id="3eb9c-689">每个属性声明都与相应的继承属性的可访问性修饰符、类型和名称完全匹配。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-689">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="3eb9c-690">属性 @no__t 的 @no__t 取值函数为-1，属性的 `Set` 访问器 `Y` 使用 @no__t 关键字访问继承的属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-690">The `Get` accessor of property `X` and the `Set` accessor of property `Y` use the `MyBase` keyword to access the inherited properties.</span></span> <span data-ttu-id="3eb9c-691">属性 @no__t 的声明会重写 `MustOverride` 属性，因此，在类 `B` 中没有未完成的 `MustOverride` 成员，并且 @no__t 允许成为一个常规类。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-691">The declaration of property `Z` overrides the `MustOverride` property -- thus, there are no outstanding `MustOverride` members in class `B`, and `B` is permitted to be a regular class.</span></span>

<span data-ttu-id="3eb9c-692">在第一次引用资源之前，可以使用属性来延迟资源的初始化。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-692">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="3eb9c-693">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-693">For example:</span></span>

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

<span data-ttu-id="3eb9c-694">@No__t-0 类包含三个属性，分别分别表示标准输入、输出和错误设备，分别为 `In`、@no__t 和 @no__t。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-694">The `ConsoleStreams` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="3eb9c-695">通过将这些成员作为属性公开，@no__t 0 类可以将其初始化延迟到实际使用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-695">By exposing these members as properties, the `ConsoleStreams` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="3eb9c-696">例如，在第一次引用 `Out` 属性（如 `ConsoleStreams.Out.WriteLine("hello, world")` 中）时，将初始化输出设备的基础 `TextWriter`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-696">For example, upon first referencing the `Out` property, as in `ConsoleStreams.Out.WriteLine("hello, world")`, the underlying `TextWriter` for the output device is initialized.</span></span> <span data-ttu-id="3eb9c-697">但如果应用程序不引用 `In` 和 `Error` 属性，则不会为这些设备创建任何对象。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-697">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>


### <a name="get-accessor-declarations"></a><span data-ttu-id="3eb9c-698">获取访问器声明</span><span class="sxs-lookup"><span data-stu-id="3eb9c-698">Get Accessor Declarations</span></span>

<span data-ttu-id="3eb9c-699">@No__t-0 取值函数（getter）是通过使用属性 `Get` 声明来声明的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-699">A `Get` accessor (getter) is declared by using a property `Get` declaration.</span></span> <span data-ttu-id="3eb9c-700">属性 `Get` 声明由关键字 `Get` 后跟语句块组成。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-700">A property `Get` declaration consists of the keyword `Get` followed by a statement block.</span></span> <span data-ttu-id="3eb9c-701">给定一个名为 `P` 的属性，@no__t 1 访问器声明隐式声明了名称 @no__t 为-2 且具有相同修饰符、类型和参数列表的方法作为属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-701">Given a property named `P`, a `Get` accessor declaration implicitly declares a method with the name `get_P` with the same modifiers, type, and parameter list as the property.</span></span> <span data-ttu-id="3eb9c-702">如果该类型包含具有该名称的声明，则会生成编译时错误，但会忽略隐式声明以便进行名称绑定。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-702">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="3eb9c-703">在 `Get` 访问器主体的声明空间中隐式声明的、与属性同名的特殊局部变量表示属性的返回值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-703">A special local variable, which is implicitly declared in the `Get` accessor body's declaration space with the same name as the property, represents the return value of the property.</span></span> <span data-ttu-id="3eb9c-704">在表达式中使用时，局部变量具有特殊名称解析语义。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-704">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="3eb9c-705">如果本地变量用于需要归类为方法组的表达式的上下文中（如调用表达式），则名称解析为函数而不是局部变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-705">If the local variable is used in a context that expects an expression that is classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="3eb9c-706">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-706">For example:</span></span>

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

<span data-ttu-id="3eb9c-707">使用括号可能导致不明确的情况（例如 `F(1)`，其中 @no__t 为类型为一维数组的属性）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-707">The use of parentheses can cause ambiguous situations (such as `F(1)` where `F` is a property whose type is a one-dimensional array).</span></span> <span data-ttu-id="3eb9c-708">在所有不明确的情况下，名称解析为属性而不是局部变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-708">In all ambiguous situations, the name resolves to the property rather than the local variable.</span></span> <span data-ttu-id="3eb9c-709">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-709">For example:</span></span>

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

<span data-ttu-id="3eb9c-710">当控制流离开 `Get` 访问器主体时，本地变量的值将传递回调用表达式。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-710">When control flow leaves the `Get` accessor body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="3eb9c-711">由于调用 @no__t 0 取值函数在概念上等同于读取变量的值，因此，`Get` 访问器的编程样式被视为具有明显副作用的错误编程方式，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-711">Because invoking a `Get` accessor is conceptually equivalent to reading the value of a variable, it is considered bad programming style for `Get` accessors to have observable side effects, as illustrated in the following example:</span></span>

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

<span data-ttu-id="3eb9c-712">@No__t-0 属性的值取决于属性以前被访问的次数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-712">The value of the `NextValue` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="3eb9c-713">因此，访问属性会产生一个可观察到的副作用，而该属性应改为作为方法实现。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-713">Thus, accessing the property produces an observable side effect, and the property should instead be implemented as a method.</span></span>

<span data-ttu-id="3eb9c-714">@No__t-0 访问器的 "无副作用" 约定并不意味着应始终编写 @no__t 的访问器，以便只返回存储在变量中的值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-714">The "no side effects" convention for `Get` accessors does not mean that `Get` accessors should always be written to simply return values stored in variables.</span></span> <span data-ttu-id="3eb9c-715">实际上，`Get` 访问器通过访问多个变量或调用方法来计算属性的值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-715">Indeed, `Get` accessors often compute the value of a property by accessing multiple variables or invoking methods.</span></span> <span data-ttu-id="3eb9c-716">但是，正确设计的 `Get` 访问器不会执行任何操作，从而导致对象的状态发生变化。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-716">However, a properly designed `Get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="3eb9c-717">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-717">__Note.__</span></span> <span data-ttu-id="3eb9c-718">`Get` 访问器在子例程具有的行位置上具有相同的限制。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-718">`Get` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="3eb9c-719">开始语句、结束语句和块必须全部显示在逻辑行的开头。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-719">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a><span data-ttu-id="3eb9c-720">设置访问器声明</span><span class="sxs-lookup"><span data-stu-id="3eb9c-720">Set Accessor Declarations</span></span>

<span data-ttu-id="3eb9c-721">使用属性集声明声明 @no__t 0 取值函数（setter）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-721">A `Set` accessor (setter) is declared by using a property set declaration.</span></span> <span data-ttu-id="3eb9c-722">属性集声明由关键字 `Set`、可选参数列表和语句块组成。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-722">A property set declaration consists of the keyword `Set`, an optional parameter list, and a statement block.</span></span> <span data-ttu-id="3eb9c-723">给定一个名为 `P` 的属性，setter 声明将使用与属性相同的修饰符和参数列表隐式声明名为 `set_P` 的方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-723">Given a property named `P`, a setter declaration implicitly declares a method with the name `set_P` with the same modifiers and parameter list as the property.</span></span> <span data-ttu-id="3eb9c-724">如果该类型包含具有该名称的声明，则会生成编译时错误，但会忽略隐式声明以便进行名称绑定。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-724">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="3eb9c-725">如果指定了参数列表，则它必须具有一个成员，该成员必须不包含除 `ByVal` 以外的任何修饰符，并且其类型必须与属性的类型相同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-725">If a parameter list is specified, it must have one member, that member must have no modifiers except `ByVal`, and its type must be the same as the type of the property.</span></span> <span data-ttu-id="3eb9c-726">参数表示要设置的属性值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-726">The parameter represents the property value being set.</span></span> <span data-ttu-id="3eb9c-727">如果省略该参数，则将隐式声明一个名为 `Value` 的参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-727">If the parameter is omitted, a parameter named `Value` is implicitly declared.</span></span>

<span data-ttu-id="3eb9c-728">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-728">__Note.__</span></span> <span data-ttu-id="3eb9c-729">`Set` 访问器在子例程具有的行位置上具有相同的限制。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-729">`Set` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="3eb9c-730">开始语句、结束语句和块必须全部显示在逻辑行的开头。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-730">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a><span data-ttu-id="3eb9c-731">默认属性</span><span class="sxs-lookup"><span data-stu-id="3eb9c-731">Default Properties</span></span>

<span data-ttu-id="3eb9c-732">指定修饰符 @no__t 的属性称为 "*默认属性*"。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-732">A property that specifies the modifier `Default` is called a *default property*.</span></span> <span data-ttu-id="3eb9c-733">允许属性的任何类型都可以具有默认属性，包括接口。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-733">Any type that allows properties may have a default property, including interfaces.</span></span> <span data-ttu-id="3eb9c-734">可以引用默认属性，而无需用属性的名称限定该实例。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-734">The default property may be referenced without having to qualify the instance with the name of the property.</span></span> <span data-ttu-id="3eb9c-735">因此，给定一个类</span><span class="sxs-lookup"><span data-stu-id="3eb9c-735">Thus, given a class</span></span>

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

<span data-ttu-id="3eb9c-736">代码</span><span class="sxs-lookup"><span data-stu-id="3eb9c-736">the code</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

<span data-ttu-id="3eb9c-737">等效于</span><span class="sxs-lookup"><span data-stu-id="3eb9c-737">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

<span data-ttu-id="3eb9c-738">一旦将某个属性声明 `Default`，在继承层次结构中该名称上重载的所有属性都将成为默认属性，无论它们是否已声明为 @no__t。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-738">Once a property is declared `Default`, all of the properties overloaded on that name in the inheritance hierarchy become the default property, whether they have been declared `Default` or not.</span></span> <span data-ttu-id="3eb9c-739">如果基类通过其他名称声明了默认属性，则在派生类中声明属性 `Default` 不需要任何其他修饰符，如 `Shadows` 或 `Overrides`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-739">Declaring a property `Default` in a derived class when the base class declared a default property by another name does not require any other modifiers such as `Shadows` or `Overrides`.</span></span> <span data-ttu-id="3eb9c-740">这是因为默认属性没有标识或签名，因此不能被隐藏或重载。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-740">This is because the default property has no identity or signature and so cannot be shadowed or overloaded.</span></span> <span data-ttu-id="3eb9c-741">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-741">For example:</span></span>

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

<span data-ttu-id="3eb9c-742">此程序将生成输出：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-742">This program will produce the output:</span></span>

```console
MoreDerived = 10
Derived = 10
Base = 10
```

<span data-ttu-id="3eb9c-743">类型中声明的所有默认属性都必须具有相同的名称，为清楚起见，必须指定 `Default` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-743">All default properties declared within a type must have the same name and, for clarity, must specify the `Default` modifier.</span></span> <span data-ttu-id="3eb9c-744">由于没有索引参数的默认属性会导致在分配包含类的实例时出现不明确的情况，因此默认属性必须具有索引参数。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-744">Because a default property with no index parameters would cause an ambiguous situation when assigning instances of the containing class, default properties must have index parameters.</span></span> <span data-ttu-id="3eb9c-745">此外，如果在特定名称上重载的一个属性包含 `Default` 修饰符，则在该名称上重载的所有属性都必须指定它。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-745">Furthermore, if one property overloaded on a particular name includes the `Default` modifier, all properties overloaded on that name must specify it.</span></span> <span data-ttu-id="3eb9c-746">默认属性不能 `Shared`，并且属性的至少一个取值函数不得为-1 @no__t。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-746">Default properties may not be `Shared`, and at least one accessor of the property must not be `Private`.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="3eb9c-747">自动实现的属性</span><span class="sxs-lookup"><span data-stu-id="3eb9c-747">Automatically Implemented Properties</span></span>

<span data-ttu-id="3eb9c-748">如果属性省略了任何访问器的声明，则将自动提供属性的实现，除非该属性是在接口中声明的，或者声明为 `MustOverride`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-748">If a property omits declaration of any accessors, an implementation of the property will be automatically supplied unless the property is declared in an interface or is declared `MustOverride`.</span></span> <span data-ttu-id="3eb9c-749">只有没有参数的读/写属性才能自动实现;否则，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-749">Only read/write properties with no arguments can be automatically implemented; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="3eb9c-750">自动实现的属性 `x`，甚至一个重写另一个属性，引入了与属性具有相同类型的专用局部变量 `_x`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-750">An automatically implemented property `x`, even one overriding another property, introduces a private local variable `_x` with the same type as the property.</span></span> <span data-ttu-id="3eb9c-751">如果局部变量名称和其他声明之间存在冲突，则将报告编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-751">If there is a collision between the local variable's name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="3eb9c-752">自动实现的属性的 `Get` 访问器返回设置本地的值的本地和属性的 `Set` 访问器的值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-752">The automatically implemented property's `Get` accessor returns the value of the local and the property's `Set` accessor that sets the value of the local.</span></span> <span data-ttu-id="3eb9c-753">例如，声明：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-753">For example, the declaration:</span></span>

```vb
Public Property x() As Integer
```

<span data-ttu-id="3eb9c-754">大致等效于：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-754">is roughly equivalent to:</span></span>

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

<span data-ttu-id="3eb9c-755">与变量声明一样，自动实现的属性可以包含初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-755">As with variable declarations, an automatically implemented property can include an initializer.</span></span> <span data-ttu-id="3eb9c-756">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-756">For example:</span></span>

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

<span data-ttu-id="3eb9c-757">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-757">__Note.__</span></span> <span data-ttu-id="3eb9c-758">当初始化自动实现的属性时，它将通过属性而不是基础字段进行初始化。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-758">When an automatically implemented property is initialized, it is initialized through the property, not the underlying field.</span></span> <span data-ttu-id="3eb9c-759">这是因为，如果需要，重写属性可以截获初始化。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-759">This is so overriding properties can intercept the initialization if they need to.</span></span>

<span data-ttu-id="3eb9c-760">自动实现的属性上允许使用数组初始值设定项，但无法显式指定数组界限。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-760">Array initializers are allowed on automatically implemented properties, except that there is no way to specify the array bounds explicitly.</span></span>  <span data-ttu-id="3eb9c-761">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-761">For example:</span></span>

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a><span data-ttu-id="3eb9c-762">迭代器属性</span><span class="sxs-lookup"><span data-stu-id="3eb9c-762">Iterator Properties</span></span>

<span data-ttu-id="3eb9c-763">*Iterator 属性*是具有 `Iterator` 修饰符的属性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-763">An *iterator property* is a property with the `Iterator` modifier.</span></span> <span data-ttu-id="3eb9c-764">使用迭代器方法（部分[迭代器方法](statements.md#iterator-methods)）的相同原因是使用它作为生成序列的一种简便方法，该方法可由 `For Each` 语句使用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-764">It is used for the same reason an iterator method (Section [Iterator Methods](statements.md#iterator-methods)) is used -- as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="3eb9c-765">迭代器属性的 `Get` 访问器的解释方式与迭代器方法相同。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-765">The `Get` accessor of an iterator property is interpreted in the same way as an iterator method.</span></span>

<span data-ttu-id="3eb9c-766">迭代器属性必须具有显式 @no__t 0 取值函数，并且它的类型必须为 `IEnumerator` 或 `IEnumerable`，或者对于某些 @no__t 为 @no__t 为 @no__t。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-766">An iterator property must have an explicit `Get` accessor, and its type must be `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`.</span></span>

<span data-ttu-id="3eb9c-767">下面是迭代器属性的示例：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-767">Here is an example of an iterator property:</span></span>

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

## <a name="operators"></a><span data-ttu-id="3eb9c-768">运算符</span><span class="sxs-lookup"><span data-stu-id="3eb9c-768">Operators</span></span>

<span data-ttu-id="3eb9c-769">*运算符*是为包含类定义现有 Visual Basic 运算符的含义的方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-769">*Operators* are methods that define the meaning of an existing Visual Basic operator for the containing class.</span></span> <span data-ttu-id="3eb9c-770">当运算符应用到表达式中的类时，运算符将编译为对类中定义的运算符方法的调用。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-770">When the operator is applied to the class in an expression, the operator is compiled into a call to the operator method defined in the class.</span></span> <span data-ttu-id="3eb9c-771">为类定义运算符也称为*重载*运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-771">Defining an operator for a class is also known as *overloading* the operator.</span></span>

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

<span data-ttu-id="3eb9c-772">不能重载已经存在的运算符;实际上，这主要适用于转换运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-772">It is not possible to overload an operator that already exists; in practice, this primarily applies to conversion operators.</span></span> <span data-ttu-id="3eb9c-773">例如，不能重载从派生类到基类的转换：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-773">For example, it is not possible to overload the conversion from a derived class to a base class:</span></span>

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

<span data-ttu-id="3eb9c-774">运算符也可以在 word 的常见意义上重载：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-774">Operators can also be overloaded in the common sense of the word:</span></span>

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

<span data-ttu-id="3eb9c-775">运算符声明不会将名称显式添加到包含类型的声明空间;但是，它们隐式声明了一个以字符 "op_" 开头的相应方法。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-775">Operator declarations do not explicitly add names to the containing type's declaration space; however they do implicitly declare a corresponding method starting with the characters "op_".</span></span> <span data-ttu-id="3eb9c-776">以下部分列出了每个运算符对应的方法名称。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-776">The following sections list the corresponding method names with each operator.</span></span>

<span data-ttu-id="3eb9c-777">可以定义三类运算符：一元运算符、二元运算符和转换运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-777">There are three classes of operators that can be defined: unary operators, binary operators and conversion operators.</span></span> <span data-ttu-id="3eb9c-778">所有运算符声明都具有某些限制：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-778">All operator declarations share certain restrictions:</span></span>

* <span data-ttu-id="3eb9c-779">运算符声明必须始终 `Public` 并 `Shared`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-779">Operator declarations must always be `Public` and `Shared`.</span></span> <span data-ttu-id="3eb9c-780">可以在将采用修饰符的上下文中省略 `Public` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-780">The `Public` modifier can be omitted in contexts where the modifier will be assumed.</span></span>

* <span data-ttu-id="3eb9c-781">运算符的参数不能声明为 `ByRef`、`Optional` 或 `ParamArray`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-781">The parameters of an operator cannot be declared `ByRef`, `Optional` or `ParamArray`.</span></span>

* <span data-ttu-id="3eb9c-782">至少一个操作数或返回值的类型必须为包含运算符的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-782">The type of at least one of the operands or the return value must be the type that contains the operator.</span></span>

* <span data-ttu-id="3eb9c-783">没有为运算符定义函数返回变量。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-783">There is no function return variable defined for operators.</span></span> <span data-ttu-id="3eb9c-784">因此，必须使用 `Return` 语句从运算符体返回值。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-784">Therefore, the `Return` statement must be used to return values from an operator body.</span></span>

<span data-ttu-id="3eb9c-785">这些限制的唯一例外情况适用于可为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-785">The only exception to these restrictions applies to nullable value types.</span></span> <span data-ttu-id="3eb9c-786">由于可以为 null 的值类型没有实际类型定义，因此值类型可以为类型的可以为 null 的版本声明用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-786">Since nullable value types do not have an actual type definition, a value type can declare user-defined operators for the nullable version of the type.</span></span> <span data-ttu-id="3eb9c-787">在确定类型是否可以声明特定的用户定义运算符时，将从声明中涉及的所有类型中首次删除 @no__t 的修饰符，目的是进行有效性检查。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-787">When determining whether a type can declare a particular user-defined operator, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="3eb9c-788">此 relaxation 不适用于 @no__t 0 和 `IsFalse` 运算符的返回类型;它们仍必须返回 `Boolean`，而不是 `Boolean?`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-788">This relaxation does not apply to the return type of the `IsTrue` and `IsFalse` operators; they must still return `Boolean`, not `Boolean?`.</span></span>

<span data-ttu-id="3eb9c-789">运算符声明不能修改运算符的优先级和关联性。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-789">The precedence and associativity of an operator cannot be modified by an operator declaration.</span></span>

<span data-ttu-id="3eb9c-790">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-790">__Note.__</span></span> <span data-ttu-id="3eb9c-791">对于子程序具有的行位置，运算符具有相同的限制。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-791">Operators have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="3eb9c-792">开始语句、结束语句和块必须全部显示在逻辑行的开头。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-792">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>


### <a name="unary-operators"></a><span data-ttu-id="3eb9c-793">一元运算符</span><span class="sxs-lookup"><span data-stu-id="3eb9c-793">Unary Operators</span></span>

<span data-ttu-id="3eb9c-794">可以重载以下一元运算符：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-794">The following unary operators can be overloaded:</span></span>

* <span data-ttu-id="3eb9c-795">一元加运算符 `+` （对应的方法： `op_UnaryPlus`）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-795">The unary plus operator `+` (corresponding method: `op_UnaryPlus`)</span></span>

* <span data-ttu-id="3eb9c-796">一元减号运算符 `-` （对应的方法： `op_UnaryNegation`）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-796">The unary minus operator `-` (corresponding method: `op_UnaryNegation`)</span></span>

* <span data-ttu-id="3eb9c-797">逻辑 `Not` 运算符（对应的方法： `op_OnesComplement`）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-797">The logical `Not` operator (corresponding method: `op_OnesComplement`)</span></span>

* <span data-ttu-id="3eb9c-798">@No__t 0 和 `IsFalse` 运算符（相应的方法： `op_True`、`op_False`）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-798">The `IsTrue` and `IsFalse` operators (corresponding methods: `op_True`, `op_False`)</span></span>

<span data-ttu-id="3eb9c-799">所有重载一元运算符都必须采用包含类型的单个参数，并可能返回任何类型，@no__t 0 和 `IsFalse` 除外，必须返回 `Boolean`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-799">All overloaded unary operators must take a single parameter of the containing type and may return any type, except for `IsTrue` and `IsFalse`, which must return `Boolean`.</span></span> <span data-ttu-id="3eb9c-800">如果包含类型是泛型类型，则类型参数必须与包含类型的类型参数匹配。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-800">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="3eb9c-801">例如，应用于对象的</span><span class="sxs-lookup"><span data-stu-id="3eb9c-801">For example,</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

<span data-ttu-id="3eb9c-802">如果某一类型重载 `IsTrue` 或 `IsFalse` 中的一个，则它也必须重载另一个。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-802">If a type overloads one of `IsTrue` or `IsFalse`, then it must overload the other as well.</span></span> <span data-ttu-id="3eb9c-803">如果只有一个重载，则会产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-803">If only one is overloaded, a compile-time error results.</span></span>

<span data-ttu-id="3eb9c-804">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-804">__Note.__</span></span> <span data-ttu-id="3eb9c-805">`IsTrue` 和 @no__t 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-805">`IsTrue` and `IsFalse` are not reserved words.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="3eb9c-806">二元运算符</span><span class="sxs-lookup"><span data-stu-id="3eb9c-806">Binary Operators</span></span>

<span data-ttu-id="3eb9c-807">可以重载以下二进制运算符：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-807">The following binary operators can be overloaded:</span></span>

* <span data-ttu-id="3eb9c-808">加法 `+`、减法 `-`、乘法 `*`、除 `/`、整数除法 `\`、取 `Mod` 和求幂 `^` 运算符（相应方法： `op_Addition`、`op_Subtraction`、`op_Multiply`、0 @no__t_t-12，3）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-808">The addition `+`, subtraction `-`, multiplication `*`, division `/`, integral division `\`, modulo `Mod` and exponentiation `^` operators (corresponding method: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span></span>

* <span data-ttu-id="3eb9c-809">关系运算符 `=`，`<>`，`<`，`>`，`<=`，`>=` （相应的方法： `op_Equality`，`op_Inequality`，`op_LessThan`，`op_GreaterThan`，0）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-809">The relational operators `=`, `<>`, `<`, `>`, `<=`, `>=` (corresponding methods: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual`, `op_GreaterThanOrEqual`).</span></span> <span data-ttu-id="3eb9c-810">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-810">__Note.__</span></span> <span data-ttu-id="3eb9c-811">尽管相等运算符可重载，但赋值运算符（仅用于赋值语句）无法重载。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-811">While the equality operator can be overloaded, the assignment operator (used only in assignment statements) cannot be overloaded.</span></span>

* <span data-ttu-id="3eb9c-812">@No__t-0 运算符（对应的方法： `op_Like`）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-812">The `Like` operator (corresponding method: `op_Like`)</span></span>

* <span data-ttu-id="3eb9c-813">串联运算符 `&` （对应的方法： `op_Concatenate`）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-813">The concatenation operator `&` (corresponding method: `op_Concatenate`)</span></span>

* <span data-ttu-id="3eb9c-814">逻辑 `And`、`Or` 和 @no__t 2 运算符（相应的方法： `op_BitwiseAnd`、`op_BitwiseOr`、`op_ExclusiveOr`）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-814">The logical `And`, `Or` and `Xor` operators (corresponding methods: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span></span>

* <span data-ttu-id="3eb9c-815">移位运算符 `<<` 并 `>>` （相应方法： `op_LeftShift`、`op_RightShift`）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-815">The shift operators `<<` and `>>` (corresponding methods: `op_LeftShift`, `op_RightShift`)</span></span>

<span data-ttu-id="3eb9c-816">所有重载的二元运算符都必须采用包含类型作为参数之一。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-816">All overloaded binary operators must take the containing type as one of the parameters.</span></span> <span data-ttu-id="3eb9c-817">如果包含类型是泛型类型，则类型参数必须与包含类型的类型参数匹配。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-817">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="3eb9c-818">移位运算符会进一步限制此规则，要求第一个参数为包含类型;第二个参数的类型必须始终为 `Integer`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-818">The shift operators further restrict this rule to require the first parameter to be of the containing type; the second parameter must always be of type `Integer`.</span></span>

<span data-ttu-id="3eb9c-819">以下二元运算符必须成对声明：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-819">The following binary operators must be declared in pairs:</span></span>

* <span data-ttu-id="3eb9c-820">运算符 `=`，运算符 `<>`</span><span class="sxs-lookup"><span data-stu-id="3eb9c-820">Operator `=` and operator `<>`</span></span>

* <span data-ttu-id="3eb9c-821">运算符 `>`，运算符 `<`</span><span class="sxs-lookup"><span data-stu-id="3eb9c-821">Operator `>` and operator `<`</span></span>

* <span data-ttu-id="3eb9c-822">运算符 `>=`，运算符 `<=`</span><span class="sxs-lookup"><span data-stu-id="3eb9c-822">Operator `>=` and operator `<=`</span></span>

<span data-ttu-id="3eb9c-823">如果声明其中一个对，则另一个还必须使用匹配的参数和返回类型进行声明，否则将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-823">If one of the pair is declared, then the other must also be declared with matching parameter and return types, or a compile-time error will result.</span></span> <span data-ttu-id="3eb9c-824">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-824">(__Note.__</span></span> <span data-ttu-id="3eb9c-825">要求成对的关系运算符声明的目的是尝试确保在重载运算符中至少具有最低级别的逻辑一致性。）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-825">The purpose of requiring paired declarations of relational operators is to try and ensure at least a minimum level of logical consistency in overloaded operators.)</span></span>

<span data-ttu-id="3eb9c-826">与关系运算符不同，强烈不建议同时重载除法运算符和整数除法运算符，尽管不是错误。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-826">In contrast to the relational operators, overloading both the division and integral division operators is strongly discouraged, although not an error.</span></span> <span data-ttu-id="3eb9c-827">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="3eb9c-827">(__Note.__</span></span> <span data-ttu-id="3eb9c-828">通常，两种类型的除法应完全不同：支持除法运算的类型是整型（在这种情况下，它应支持 `\`），而不是（在这种情况下，它应支持 `/`）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-828">In general, the two types of division should be entirely distinct: a type that supports division is either integral (in which case it should support `\`) or not (in which case it should support `/`).</span></span> <span data-ttu-id="3eb9c-829">我们认为定义这两个运算符是错误的，但由于其语言通常不能区分 Visual Basic 的两种划分方式，因此，我们认为这种做法是最安全的方法，但这种做法非常反对。）</span><span class="sxs-lookup"><span data-stu-id="3eb9c-829">We considered making it an error to define both operators, but because their languages do not generally distinguish between two types of division the way Visual Basic does, we felt it was safest to allow the practice but strongly discourage it.)</span></span>

<span data-ttu-id="3eb9c-830">不能直接重载复合赋值运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-830">Compound assignment operators cannot be overloaded directly.</span></span> <span data-ttu-id="3eb9c-831">相反，当重载相应的二元运算符时，复合赋值运算符将使用重载运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-831">Instead, when the corresponding binary operator is overloaded, the compound assignment operator will use the overloaded operator.</span></span> <span data-ttu-id="3eb9c-832">例如：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-832">For example:</span></span>

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

### <a name="conversion-operators"></a><span data-ttu-id="3eb9c-833">转换运算符</span><span class="sxs-lookup"><span data-stu-id="3eb9c-833">Conversion Operators</span></span>

<span data-ttu-id="3eb9c-834">转换运算符定义类型之间的新转换。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-834">Conversion operators define new conversions between types.</span></span> <span data-ttu-id="3eb9c-835">这些新转换称为*用户定义的转换*。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-835">These new conversions are called *user-defined conversions*.</span></span> <span data-ttu-id="3eb9c-836">转换运算符从转换运算符的参数类型指示的源类型转换为目标类型，由转换运算符的返回类型指示。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-836">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span> <span data-ttu-id="3eb9c-837">转换必须分类为扩大或收缩。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-837">Conversions must be classified as either widening or narrowing.</span></span> <span data-ttu-id="3eb9c-838">包含 @no__t 关键字的转换运算符声明引入了用户定义的扩大转换（对应的方法： `op_Implicit`）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-838">A conversion operator declaration that includes the `Widening` keyword introduces a user-defined widening conversion (corresponding method: `op_Implicit`).</span></span> <span data-ttu-id="3eb9c-839">包含 @no__t 关键字的转换运算符声明引入了用户定义的收缩转换（对应的方法： `op_Explicit`）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-839">A conversion operator declaration that includes the `Narrowing` keyword introduces a user-defined narrowing conversion (corresponding method: `op_Explicit`).</span></span>

<span data-ttu-id="3eb9c-840">通常，用户定义的扩大转换应设计为绝不会引发异常，并且永远不会丢失信息。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-840">In general, user-defined widening conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="3eb9c-841">如果用户定义的转换可能会导致异常（例如，因为 source 参数超出范围）或丢失信息（如丢弃高阶位），则应将该转换定义为收缩转换。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-841">If a user-defined conversion can cause exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as a narrowing conversion.</span></span> <span data-ttu-id="3eb9c-842">在下面的示例中：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-842">In the example:</span></span>

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

<span data-ttu-id="3eb9c-843">从 `Digit` 到 @no__t 的转换是一种扩大转换，因为它从不引发异常或丢失信息，但从 `Byte` 到 `Digit` 的转换是收缩转换，因为 `Digit` 只能表示的可能值的子集`Byte`。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-843">the conversion from `Digit` to `Byte` is a widening conversion because it never throws exceptions or loses information, but the conversion from `Byte` to `Digit` is a narrowing conversion since `Digit` can only represent a subset of the possible values of a `Byte`.</span></span>

<span data-ttu-id="3eb9c-844">与可重载的所有其他类型成员不同，转换运算符的签名包含转换的目标类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-844">Unlike all other type members that can be overloaded, the signature of a conversion operator includes the target type of the conversion.</span></span> <span data-ttu-id="3eb9c-845">这是返回类型在签名中参与的唯一类型成员。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-845">This is the only type member for which the return type participates in the signature.</span></span> <span data-ttu-id="3eb9c-846">但是，转换运算符的扩大或收缩分类不是运算符签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-846">The widening or narrowing classification of a conversion operator, however, is not part of the operator's signature.</span></span> <span data-ttu-id="3eb9c-847">因此，类或结构不能同时声明扩大转换运算符和具有相同源和目标类型的收缩转换运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-847">Thus, a class or structure cannot declare both a widening conversion operator and a narrowing conversion operator with the same source and target types.</span></span>

<span data-ttu-id="3eb9c-848">用户定义的转换运算符必须转换为包含类型或从包含类型转换--例如，类可能 `C` 定义从 @no__t 到 `Integer` 的转换和从 @no__t 3 到 @no__t 的转换，而不是从 @no__t 到 @no__t。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-848">A user-defined conversion operator must convert either to or from the containing type -- for example, it is possible for a class `C` to define a conversion from `C` to `Integer` and from `Integer` to `C`, but not from `Integer` to `Boolean`.</span></span> <span data-ttu-id="3eb9c-849">如果包含类型是泛型类型，则类型参数必须与包含类型的类型参数匹配。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-849">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="3eb9c-850">此外，不能重新定义内部函数（即非用户定义的）转换。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-850">Also, it is not possible to redefine an intrinsic (i.e. non-user-defined) conversion.</span></span> <span data-ttu-id="3eb9c-851">因此，类型不能声明转换，其中：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-851">As a result, a type cannot declare a conversion where:</span></span>

* <span data-ttu-id="3eb9c-852">源类型和目标类型是相同的。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-852">The source type and the destination type are the same.</span></span>

* <span data-ttu-id="3eb9c-853">源类型和目标类型都不是定义转换运算符的类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-853">Both the source type and the destination type are not the type that defines the conversion operator.</span></span>

* <span data-ttu-id="3eb9c-854">源类型或目标类型是接口类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-854">The source type or the destination type is an interface type.</span></span>

* <span data-ttu-id="3eb9c-855">源类型和目标类型通过继承（包括 `Object`）相关。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-855">The source type and destination types are related by inheritance (including `Object`).</span></span>

<span data-ttu-id="3eb9c-856">这些规则的唯一例外情况适用于可为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-856">The only exception to these rules applies to nullable value types.</span></span> <span data-ttu-id="3eb9c-857">由于可以为 null 的值类型没有实际类型定义，因此值类型可以为类型的可以为 null 的版本声明用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-857">Since nullable value types do not have an actual type definition, a value type can declare user-defined conversions for the nullable version of the type.</span></span> <span data-ttu-id="3eb9c-858">在确定类型是否可以声明特定的用户定义的转换时，将从声明中涉及的所有类型中第一次删除 @no__t 的修饰符，目的是进行有效性检查。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-858">When determining whether a type can declare a particular user-defined conversion, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="3eb9c-859">因此，以下声明是有效的，因为 `S` 可以定义从 @no__t 到 `T` 的转换：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-859">Thus, the following declaration is valid because `S` can define a conversion from `S` to `T`:</span></span>

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

<span data-ttu-id="3eb9c-860">但是，以下声明无效，因为结构 `S` 不能定义从 @no__t 到 `S` 的转换：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-860">The following declaration is not valid, however, because structure `S` cannot define a conversion from `S` to `S`:</span></span>

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a><span data-ttu-id="3eb9c-861">运算符映射</span><span class="sxs-lookup"><span data-stu-id="3eb9c-861">Operator Mapping</span></span>

<span data-ttu-id="3eb9c-862">由于 Visual Basic 支持的运算符集可能与 .NET Framework 上其他语言的运算符集不完全匹配，因此，在定义或使用某些运算符时，某些运算符将专门映射到其他运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-862">Because the set of operators that Visual Basic supports may not exactly match the set of operators that other languages on the .NET Framework, some operators are mapped specially onto other operators when being defined or used.</span></span> <span data-ttu-id="3eb9c-863">尤其是在下列情况下：</span><span class="sxs-lookup"><span data-stu-id="3eb9c-863">Specifically:</span></span>

* <span data-ttu-id="3eb9c-864">定义整数除法运算符将自动定义将调用整数除法运算符的常规除法运算符（仅可用于其他语言）。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-864">Defining an integral division operator will automatically define a regular division operator (usable only from other languages) that will call the integral division operator.</span></span>

* <span data-ttu-id="3eb9c-865">重载 `Not`、`And` 和 @no__t 2 运算符将仅从区分逻辑与按位运算符的其他语言的角度重载位运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-865">Overloading the `Not`, `And`, and `Or` operators will overload only the bitwise operator from the perspective of other languages that distinguish between logical and bitwise operators.</span></span>

* <span data-ttu-id="3eb9c-866">一个类，它仅重载一种语言中的逻辑运算符，该运算符区分逻辑运算符和位运算符（即，使用 `op_LogicalNot`、`op_LogicalAnd` 的语言，以及分别为 `Not`、`And` 和 @no__t 的 `op_LogicalOr`）将具有逻辑映射到 Visual Basic 逻辑运算符上的运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-866">A class that overloads only the logical operators in a language that distinguishes between logical and bitwise operators (i.e. a languages that uses `op_LogicalNot`, `op_LogicalAnd`, and `op_LogicalOr` for `Not`, `And`, and `Or`, respectively) will have their logical operators mapped onto the Visual Basic logical operators.</span></span> <span data-ttu-id="3eb9c-867">如果同时重载逻辑运算符和按位运算符，则只使用位运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-867">If both the logical and bitwise operators are overloaded, only the bitwise operators will be used.</span></span>

* <span data-ttu-id="3eb9c-868">重载 `<<` 和 @no__t 1 运算符将仅从区分符号和无符号移位运算符的其他语言的角度重载签名运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-868">Overloading the `<<` and `>>` operators will overload only the signed operators from the perspective of other languages that distinguish between signed and unsigned shift operators.</span></span>

* <span data-ttu-id="3eb9c-869">仅重载无符号移位运算符的类将有一个映射到相应 Visual Basic 移位运算符的无符号移位运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-869">A class that overloads only an unsigned shift operator will have the unsigned shift operator mapped onto the corresponding Visual Basic shift operator.</span></span> <span data-ttu-id="3eb9c-870">如果未签名和带符号的移位运算符都重载，则只使用带符号的移位运算符。</span><span class="sxs-lookup"><span data-stu-id="3eb9c-870">If both an unsigned and signed shift operator is overloaded, only the signed shift operator will be used.</span></span>
