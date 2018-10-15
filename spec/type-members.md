# <a name="type-members"></a><span data-ttu-id="52ad7-101">类型成员</span><span class="sxs-lookup"><span data-stu-id="52ad7-101">Type Members</span></span>

<span data-ttu-id="52ad7-102">类型成员定义的存储位置和可执行代码。</span><span class="sxs-lookup"><span data-stu-id="52ad7-102">Type members define storage locations and executable code.</span></span> <span data-ttu-id="52ad7-103">它们可以是方法、 构造函数、 事件、 常量、 变量和属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-103">They can be methods, constructors, events, constants, variables, and properties.</span></span>

## <a name="interface-method-implementation"></a><span data-ttu-id="52ad7-104">接口方法实现</span><span class="sxs-lookup"><span data-stu-id="52ad7-104">Interface Method Implementation</span></span>

<span data-ttu-id="52ad7-105">方法、 事件和属性可以实现接口成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-105">Methods, events, and properties can implement interface members.</span></span> <span data-ttu-id="52ad7-106">若要实现接口成员，成员声明指定`Implements`关键字，并列出一个或多个接口成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-106">To implement an interface member, a member declaration specifies the `Implements` keyword and lists one or more interface members.</span></span>

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

<span data-ttu-id="52ad7-107">方法和实现接口成员的属性是隐式`NotOverridable`除非声明为`MustOverride`， `Overridable`，或重写另一个成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-107">Methods and properties that implement interface members are implicitly `NotOverridable` unless declared to be `MustOverride`, `Overridable`, or overriding another member.</span></span> <span data-ttu-id="52ad7-108">它是错误的成员实现接口成员为`Shared`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-108">It is an error for a member implementing an interface member to be `Shared`.</span></span> <span data-ttu-id="52ad7-109">成员的可访问性不起作用的能力实现接口成员上。</span><span class="sxs-lookup"><span data-stu-id="52ad7-109">A member's accessibility has no effect on its ability to implement interface members.</span></span>

<span data-ttu-id="52ad7-110">有效的接口实现，包含类型的实现列表必须将命名包含兼容的成员的接口。</span><span class="sxs-lookup"><span data-stu-id="52ad7-110">For an interface implementation to be valid, the implements list of the containing type must name an interface that contains a compatible member.</span></span> <span data-ttu-id="52ad7-111">兼容的成员是成员的一个其签名与实现的签名匹配。</span><span class="sxs-lookup"><span data-stu-id="52ad7-111">A compatible member is one whose signature matches the signature of the implementing member.</span></span> <span data-ttu-id="52ad7-112">如果正在实现泛型接口，然后 Implements 子句中提供的类型实参将被替换为签名时检查兼容性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-112">If a generic interface is being implemented, then the type argument supplied in the Implements clause is substituted into the signature when checking compatibility.</span></span> <span data-ttu-id="52ad7-113">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-113">For example:</span></span>

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

<span data-ttu-id="52ad7-114">如果使用的事件声明委托类型实现接口的事件，则兼容事件是一个基础委托类型是相同的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-114">If an event declared using a delegate type is implementing an interface event, then a compatible event is one whose underlying delegate type is the same type.</span></span> <span data-ttu-id="52ad7-115">否则，该事件将使用它正在实现的接口事件的委托类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-115">Otherwise, the event uses the delegate type from the interface event it is implementing.</span></span> <span data-ttu-id="52ad7-116">如果这种情况下实现接口的多个事件，所有的接口事件必须具有相同的基础委托类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-116">If such an event implements multiple interface events, all the interface events must have the same underlying delegate type.</span></span> <span data-ttu-id="52ad7-117">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-117">For example:</span></span>

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

<span data-ttu-id="52ad7-118">使用类型名称、 句点和标识符来指定接口成员实现列表中。</span><span class="sxs-lookup"><span data-stu-id="52ad7-118">An interface member in the implements list is specified using a type name, a period, and an identifier.</span></span> <span data-ttu-id="52ad7-119">类型名称必须是接口实现列表中的或在实现列表中，接口的基接口和标识符必须是指定接口的成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-119">The type name must be an interface in the implements list or a base interface of an interface in the implements list, and the identifier must be a member of the specified interface.</span></span> <span data-ttu-id="52ad7-120">单个成员可以实现多个匹配的接口成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-120">A single member can implement more than one matching interface member.</span></span>

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

<span data-ttu-id="52ad7-121">如果所实现的接口成员一直致力于为所有显式实现的接口，因为存在多个接口继承，实现成员必须显式引用该成员是在其可用的基接口。</span><span class="sxs-lookup"><span data-stu-id="52ad7-121">If the interface member being implemented is unavailable in all explicitly implemented interfaces because of multiple interface inheritance, the implementing member must explicitly reference a base interface on which the member is available.</span></span> <span data-ttu-id="52ad7-122">例如，如果`I1`并`I2`都包含一个成员`M`，和`I3`继承`I1`并`I2`，类型实现`I3`将实现`I1.M`和`I2.M`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-122">For example, if `I1` and `I2` contain a member `M`, and `I3` inherits from `I1` and `I2`, a type implementing `I3` will implement `I1.M` and `I2.M`.</span></span> <span data-ttu-id="52ad7-123">如果接口隐藏多重继承的成员，将必须实现的类型实现的继承的成员和隐藏它们的成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-123">If an interface shadows multiply inherited members, an implementing type will have to implement the inherited members and the member(s) shadowing them.</span></span>

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

<span data-ttu-id="52ad7-124">如果实现接口成员的包含接口为泛型，必须提供所实现的接口相同的类型参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-124">If the containing interface of the interface member be implemented is generic, the same type arguments as the interface being implements must be supplied.</span></span> <span data-ttu-id="52ad7-125">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-125">For example:</span></span>

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


## <a name="methods"></a><span data-ttu-id="52ad7-126">方法</span><span class="sxs-lookup"><span data-stu-id="52ad7-126">Methods</span></span>

<span data-ttu-id="52ad7-127">方法包含一个程序的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="52ad7-127">Methods contain the executable statements of a program.</span></span>

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

<span data-ttu-id="52ad7-128">一系列可选参数和可选的返回值的方法共享或不共享。</span><span class="sxs-lookup"><span data-stu-id="52ad7-128">Methods, which have an optional list of parameters and an optional return value, are either shared or not shared.</span></span> <span data-ttu-id="52ad7-129">通过类或类的实例访问的共享的方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-129">Shared methods are accessed through the class or instances of the class.</span></span> <span data-ttu-id="52ad7-130">类的实例可以通过访问非共享方法，也称为实例方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-130">Non-shared methods, also called instance methods, are accessed through instances of the class.</span></span> <span data-ttu-id="52ad7-131">下面的示例演示一个类`Stack`具有多个共享的方法 (`Clone`并`Flip`)，和多个实例方法 (`Push`， `Pop`，并`ToString`):</span><span class="sxs-lookup"><span data-stu-id="52ad7-131">The following example shows a class `Stack` that has several shared methods (`Clone` and `Flip`), and several instance methods (`Push`, `Pop`, and `ToString`):</span></span>

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

<span data-ttu-id="52ad7-132">可以重载方法，这意味着，只要它们具有唯一的签名，多个方法可能具有相同的名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-132">Methods can be overloaded, which means that multiple methods may have the same name so long as they have unique signatures.</span></span> <span data-ttu-id="52ad7-133">方法签名组成的数量和其参数的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-133">The signature of a method consists of the number and types of its parameters.</span></span> <span data-ttu-id="52ad7-134">方法签名特别不包括返回类型或如可选、 ByRef 或 ParamArray 参数修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-134">The signature of a method specifically does not include the return type or parameter modifiers such as Optional, ByRef or ParamArray.</span></span> <span data-ttu-id="52ad7-135">下面的示例演示了大量的重载类：</span><span class="sxs-lookup"><span data-stu-id="52ad7-135">The following example shows a class with a number of overloads:</span></span>

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

<span data-ttu-id="52ad7-136">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="52ad7-136">The output of the program is:</span></span>

```vb
F()
F(Integer)
F(Object)
F(Integer, Integer)
F(Integer())
G(String)
G(String, Optional String)
```

<span data-ttu-id="52ad7-137">重载的区别仅在于可选参数可用于库的"版本管理"。</span><span class="sxs-lookup"><span data-stu-id="52ad7-137">Overloads that differ only in optional parameters can be used for "versioning" of libraries.</span></span> <span data-ttu-id="52ad7-138">例如，库的 v1 可能包括带可选参数的函数：</span><span class="sxs-lookup"><span data-stu-id="52ad7-138">For instance, v1 of a library might include a function with optional parameters:</span></span>

```vb
Sub fopen(fileName As String, Optional accessMode as Integer = 0)
```

<span data-ttu-id="52ad7-139">然后库的 v2 想要添加另一个可选参数"密码"，并想要执行此操作而不会破坏源兼容性 （因此，可以重新编译应用程序，用于指向 v1），并且不会断开二进制文件兼容性 （因此，使用到的应用程序参考 v1 现可引用无需重新编译 v2）。</span><span class="sxs-lookup"><span data-stu-id="52ad7-139">Then v2 of the library wants to add another optional parameter "password", and it wants to do so without breaking source compatibility (so applications that used to target v1 can be recompiled), and without breaking binary compatibility (so applications that used to reference v1 can now reference v2 without recompilation).</span></span> <span data-ttu-id="52ad7-140">这是 v2 的外观：</span><span class="sxs-lookup"><span data-stu-id="52ad7-140">This is how v2 will look:</span></span>

```vb
Sub fopen(file As String, mode as Integer)
Sub fopen(file As String, Optional mode as Integer = 0, Optional pword As String = "")
```

<span data-ttu-id="52ad7-141">请注意，公共 API 中的可选参数不符合 CLS 規格。</span><span class="sxs-lookup"><span data-stu-id="52ad7-141">Note that optional parameters in a public API are not CLS-compliant.</span></span> <span data-ttu-id="52ad7-142">但是，它们可供至少 Visual Basic 和 C# 4 和 F #。</span><span class="sxs-lookup"><span data-stu-id="52ad7-142">However, they can be consumed at least by Visual Basic and C#4 and F#.</span></span>



### <a name="regular-async-and-iterator-method-declarations"></a><span data-ttu-id="52ad7-143">Regular、 异步和迭代器方法声明</span><span class="sxs-lookup"><span data-stu-id="52ad7-143">Regular, Async and Iterator Method Declarations</span></span>

<span data-ttu-id="52ad7-144">有两种类型的方法：*子例程*，这不会返回值，并*函数*，其执行操作。</span><span class="sxs-lookup"><span data-stu-id="52ad7-144">There are two types of methods: *subroutines*, which do not return values, and *functions*, which do.</span></span> <span data-ttu-id="52ad7-145">正文并`End`可能仅省略构造的一种方法，如果方法在接口中定义或包含`MustOverride`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-145">The body and `End` construct of a method may only be omitted if the method is defined in an interface or has the `MustOverride` modifier.</span></span> <span data-ttu-id="52ad7-146">如果在函数上指定任何返回类型和正在使用严格的语义，会发生编译时错误;否则该类型是隐式`Object`或类型的方法的类型字符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-146">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs; otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="52ad7-147">返回类型和方法的参数类型的可访问域必须与相同或该方法本身的可访问域的超集。</span><span class="sxs-lookup"><span data-stu-id="52ad7-147">The accessibility domain of the return type and parameter types of a method must be the same as or a superset of the accessibility domain of the method itself.</span></span>

<span data-ttu-id="52ad7-148">一个__正则方法__是指既不具有`Async`也不`Iterator`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-148">A __regular method__ is one with neither `Async` nor `Iterator` modifiers.</span></span> <span data-ttu-id="52ad7-149">它可能是一个子例程或函数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-149">It may be a subroutine or a function.</span></span> <span data-ttu-id="52ad7-150">部分[正则方法](statements.md#regular-methods)详细介绍了常规方法调用时，会发生什么情况。</span><span class="sxs-lookup"><span data-stu-id="52ad7-150">Section [Regular Methods](statements.md#regular-methods) details what happens when a regular method is invoked.</span></span>

<span data-ttu-id="52ad7-151">__迭代器方法__是一个具有`Iterator`修饰符，但不`Async`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-151">An __iterator method__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="52ad7-152">它必须是一个函数，并且其返回类型必须是`IEnumerator`， `IEnumerable`，或`IEnumerator(Of T)`或`IEnumerable(Of T)`某些`T`，并且它必须无`ByRef`参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-152">It must be a function, and its return type must be `IEnumerator`, `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and it must have no `ByRef` parameters.</span></span> <span data-ttu-id="52ad7-153">部分[迭代器方法](statements.md#iterator-methods)详细介绍了迭代器方法调用时，会发生什么情况。</span><span class="sxs-lookup"><span data-stu-id="52ad7-153">Section [Iterator Methods](statements.md#iterator-methods) details what happens when an iterator method is invoked.</span></span>

<span data-ttu-id="52ad7-154">__异步方法__是一个具有`Async`修饰符，但不`Iterator`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-154">An __async method__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="52ad7-155">它必须是一个子例程或函数返回类型为`Task`或`Task(Of T)`某些`T`，并且必须具有无`ByRef`参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-155">It must be either a subroutine, or a function with return type `Task` or `Task(Of T)` for some `T`, and must have no `ByRef` parameters.</span></span> <span data-ttu-id="52ad7-156">部分[异步方法](statements.md#async-methods)详细介绍了在调用异步方法时，会发生什么情况。</span><span class="sxs-lookup"><span data-stu-id="52ad7-156">Section [Async Methods](statements.md#async-methods) details what happens when an async method is invoked.</span></span>

<span data-ttu-id="52ad7-157">如果方法不是这三种方法之一，它是一个编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-157">It is a compile-time error if a method is not one of these three kinds of method.</span></span>

<span data-ttu-id="52ad7-158">子例程和函数声明的特殊在于其开始和结束语句必须每个启动逻辑行的开头。</span><span class="sxs-lookup"><span data-stu-id="52ad7-158">Subroutine and function declarations are special in that their beginning and end statements must each start at the beginning of a logical line.</span></span> <span data-ttu-id="52ad7-159">此外，正文非`MustOverride`子例程或函数声明必须以逻辑行的开头。</span><span class="sxs-lookup"><span data-stu-id="52ad7-159">Additionally, the body of a non-`MustOverride` subroutine or function declaration must start at the beginning of a logical line.</span></span> <span data-ttu-id="52ad7-160">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-160">For example:</span></span>

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


### <a name="external-method-declarations"></a><span data-ttu-id="52ad7-161">外部方法声明</span><span class="sxs-lookup"><span data-stu-id="52ad7-161">External Method Declarations</span></span>

<span data-ttu-id="52ad7-162">外部方法声明引入了新程序的外部提供其实现的方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-162">An external method declaration introduces a new method whose implementation is provided external to the program.</span></span>

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

<span data-ttu-id="52ad7-163">由于外部方法声明不提供任何实际的实现，它有没有方法体或`End`构造。</span><span class="sxs-lookup"><span data-stu-id="52ad7-163">Because an external method declaration provides no actual implementation, it has no method body or `End` construct.</span></span> <span data-ttu-id="52ad7-164">外部方法隐式共享，可能不具有类型参数和可能不处理事件或实现接口成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-164">External methods are implicitly shared, may not have type parameters, and may not handle events or implement interface members.</span></span> <span data-ttu-id="52ad7-165">如果在函数上指定任何返回类型和正在使用严格的语义，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-165">If no return type is specified on a function and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="52ad7-166">否则该类型是隐式`Object`或类型的方法的类型字符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-166">Otherwise the type is implicitly `Object` or the type of the method's type character.</span></span> <span data-ttu-id="52ad7-167">返回类型和参数类型的外部方法的可访问域必须与相同或外部方法本身的可访问域的超集。</span><span class="sxs-lookup"><span data-stu-id="52ad7-167">The accessibility domain of the return type and parameter types of an external method must be the same as or a superset of the accessibility domain of the external method itself.</span></span>

<span data-ttu-id="52ad7-168">外部方法声明的库子句指定实现方法的外部文件的名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-168">The library clause of an external method declaration specifies the name of the external file that implements the method.</span></span> <span data-ttu-id="52ad7-169">可选别名子句是一个字符串，指定的数字序号 (加`#`字符) 或外部文件中的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-169">The optional alias clause is a string that specifies the numeric ordinal (prefixed by a `#` character) or name of the method in the external file.</span></span> <span data-ttu-id="52ad7-170">单字符集修饰符还可以指定，它控制字符串封送到外部方法的调用期间使用的字符集。</span><span class="sxs-lookup"><span data-stu-id="52ad7-170">A single-character set modifier may also be specified, which governs the character set used to marshal strings during a call to the external method.</span></span> <span data-ttu-id="52ad7-171">`Unicode`修饰符将所有字符串转换为 Unicode 值封都送`Ansi`修饰符将封都送为 ANSI 值的所有字符串和`Auto`修饰符将根据.NET Framework 规则基于方法的名称的字符串封都都送或如果指定的别名名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-171">The `Unicode` modifier marshals all strings to Unicode values, the `Ansi` modifier marshals all strings to ANSI values, and the `Auto` modifier marshals the strings according to .NET Framework rules based on the name of the method, or the alias name if specified.</span></span> <span data-ttu-id="52ad7-172">如果不指定任何修饰符，则默认值是`Ansi`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-172">If no modifier is specified, the default is `Ansi`.</span></span>

<span data-ttu-id="52ad7-173">如果`Ansi`或`Unicode`指定，则无需修改了外部文件中查找方法名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-173">If `Ansi` or `Unicode` is specified, then the method name is looked up in the external file with no modification.</span></span> <span data-ttu-id="52ad7-174">如果`Auto`指定，则方法名称查找取决于平台。</span><span class="sxs-lookup"><span data-stu-id="52ad7-174">If `Auto` is specified, then method name lookup depends on the platform.</span></span> <span data-ttu-id="52ad7-175">如果该平台被视为 ANSI （例如，Windows 95、 Windows 98、 Windows ME），然后方法名称查找，进行任何修改。</span><span class="sxs-lookup"><span data-stu-id="52ad7-175">If the platform is considered to be ANSI (for example, Windows 95, Windows 98, Windows ME), then the method name is looked up with no modification.</span></span> <span data-ttu-id="52ad7-176">如果在查找失败，`A`追加，然后重试查找。</span><span class="sxs-lookup"><span data-stu-id="52ad7-176">If the lookup fails, an `A` is appended and the lookup tried again.</span></span> <span data-ttu-id="52ad7-177">如果平台被视为 Unicode (例如，Windows NT、 Windows 2000，Windows XP)，则`W`追加和查找名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-177">If the platform is considered to be Unicode (for example, Windows NT, Windows 2000, Windows XP), then a `W` is appended and the name is looked up.</span></span> <span data-ttu-id="52ad7-178">如果在查找失败，则情况下再次尝试查找`W`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-178">If the lookup fails, the lookup is tried again without the `W`.</span></span> <span data-ttu-id="52ad7-179">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-179">For example:</span></span>

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

<span data-ttu-id="52ad7-180">根据.NET Framework 数据封送处理约定有一个例外情况下，传递给外部方法的数据类型进行封送。</span><span class="sxs-lookup"><span data-stu-id="52ad7-180">Data types being passed to external methods are marshaled according to the .NET Framework data marshalling conventions with one exception.</span></span> <span data-ttu-id="52ad7-181">按值传递的变量的字符串 (即， `ByVal x As String`) 封送到 OLE 自动化 BSTR 类型到 BSTR 外部方法中所做的更改会反映在字符串参数返回。</span><span class="sxs-lookup"><span data-stu-id="52ad7-181">String variables that are passed by value (that is, `ByVal x As String`) are marshaled to the OLE Automation BSTR type, and changes made to the BSTR in the external method are reflected back in the string argument.</span></span> <span data-ttu-id="52ad7-182">这是因为类型`String`在外部方法是可变的并且此特殊的封送模仿该行为。</span><span class="sxs-lookup"><span data-stu-id="52ad7-182">This is because the type `String` in external methods is mutable, and this special marshalling mimics that behavior.</span></span> <span data-ttu-id="52ad7-183">通过引用传递的参数的字符串 (即`ByRef x As String`) 作为指向 OLE 自动化 BSTR 类型的指针进行封送。</span><span class="sxs-lookup"><span data-stu-id="52ad7-183">String parameters that are passed by reference (i.e. `ByRef x As String`) are marshaled as a pointer to the OLE Automation BSTR type.</span></span> <span data-ttu-id="52ad7-184">可以通过指定重写这些特殊行为`System.Runtime.InteropServices.MarshalAsAttribute`参数上的属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-184">It is possible to override these special behaviors by specifying the `System.Runtime.InteropServices.MarshalAsAttribute` attribute on the parameter.</span></span>

<span data-ttu-id="52ad7-185">此示例演示外部方法的使用：</span><span class="sxs-lookup"><span data-stu-id="52ad7-185">The example demonstrates use of external methods:</span></span>

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


### <a name="overridable-methods"></a><span data-ttu-id="52ad7-186">可重写方法</span><span class="sxs-lookup"><span data-stu-id="52ad7-186">Overridable Methods</span></span>

<span data-ttu-id="52ad7-187">`Overridable`修饰符指示一种方法是可重写。</span><span class="sxs-lookup"><span data-stu-id="52ad7-187">The `Overridable` modifier indicates that a method is overridable.</span></span> <span data-ttu-id="52ad7-188">`Overrides`修饰符指示一种方法重写基类型可重写方法具有相同的签名。</span><span class="sxs-lookup"><span data-stu-id="52ad7-188">The `Overrides` modifier indicates that a method overrides a base-type overridable method that has the same signature.</span></span> <span data-ttu-id="52ad7-189">`NotOverridable`修饰符指示不能进一步重写可重写方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-189">The `NotOverridable` modifier indicates that an overridable method cannot be further overridden.</span></span> <span data-ttu-id="52ad7-190">`MustOverride`修饰符指示必须在派生类中重写方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-190">The `MustOverride` modifier indicates that a method must be overridden in derived classes.</span></span>

<span data-ttu-id="52ad7-191">这些修饰符的某些组合不是有效的：</span><span class="sxs-lookup"><span data-stu-id="52ad7-191">Certain combinations of these modifiers are not valid:</span></span>

* <span data-ttu-id="52ad7-192">`Overridable` 和`NotOverridable`是互斥的不能结合使用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-192">`Overridable` and `NotOverridable` are mutually exclusive and cannot be combined.</span></span>

* <span data-ttu-id="52ad7-193">`MustOverride` 暗指`Overridable`（并因此不能指定它），也不能与结合`NotOverridable`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-193">`MustOverride` implies `Overridable` (and so cannot specify it) and cannot be combined with `NotOverridable`.</span></span>

* <span data-ttu-id="52ad7-194">`NotOverridable` 不能结合`Overridable`或`MustOverride`，也必须与结合`Overrides`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-194">`NotOverridable` cannot be combined with `Overridable` or `MustOverride` and must be combined with `Overrides`.</span></span>

* <span data-ttu-id="52ad7-195">`Overrides` 暗指`Overridable`（并因此不能指定它），也不能与结合`MustOverride`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-195">`Overrides` implies `Overridable` (and so cannot specify it) and cannot be combined with `MustOverride`.</span></span>

<span data-ttu-id="52ad7-196">也是可重写方法的其他限制：</span><span class="sxs-lookup"><span data-stu-id="52ad7-196">There are also additional restrictions on overridable methods:</span></span>

* <span data-ttu-id="52ad7-197">一个`MustOverride`方法不能包含方法体或`End`构造，可能不会替代另一种方法，并可能只能出现在`MustInherit`类。</span><span class="sxs-lookup"><span data-stu-id="52ad7-197">A `MustOverride` method may not include a method body or an `End` construct, may not override another method, and may only appear in `MustInherit` classes.</span></span>

* <span data-ttu-id="52ad7-198">如果指定了一种方法`Overrides`，并且没有任何匹配的基方法以重写，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-198">If a method specifies `Overrides` and there is no matching base method to override, a compile-time error occurs.</span></span> <span data-ttu-id="52ad7-199">重写方法不能指定`Shadows`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-199">An overriding method may not specify `Shadows`.</span></span>

* <span data-ttu-id="52ad7-200">一种方法可能会重写另一种方法重写方法的可访问性域是否不等于被重写方法的可访问域。</span><span class="sxs-lookup"><span data-stu-id="52ad7-200">A method may not override another method if the overriding method's accessibility domain is not equal to the accessibility domain of the method being overridden.</span></span> <span data-ttu-id="52ad7-201">一个例外是，方法重写`Protected Friend`中没有的另一个程序集的方法`Friend`访问必须指定`Protected`(不`Protected Friend`)。</span><span class="sxs-lookup"><span data-stu-id="52ad7-201">The one exception is that a method overriding a `Protected Friend` method in another assembly that does not have `Friend` access must specify `Protected` (not `Protected Friend`).</span></span>

* <span data-ttu-id="52ad7-202">`Private` 方法可能`Overridable`， `NotOverridable`，或`MustOverride`，它们可能会重写其他方法，也不。</span><span class="sxs-lookup"><span data-stu-id="52ad7-202">`Private` methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, nor may they override other methods.</span></span>

* <span data-ttu-id="52ad7-203">中的方法`NotInheritable`类不能声明`Overridable`或`MustOverride`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-203">Methods in `NotInheritable` classes may not be declared `Overridable` or `MustOverride`.</span></span>

<span data-ttu-id="52ad7-204">下面的示例说明了可重写和 nonoverridable 方法之间的差异：</span><span class="sxs-lookup"><span data-stu-id="52ad7-204">The following example illustrates the differences between overridable and nonoverridable methods:</span></span>

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

<span data-ttu-id="52ad7-205">在示例中，类`Base`引入了一种方法`F`和一个`Overridable`方法`G`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-205">In the example, class `Base` introduces a method `F` and an `Overridable` method `G`.</span></span> <span data-ttu-id="52ad7-206">该类`Derived`引入了新方法`F`，从而隐藏继承`F`，并且还重写继承的方法`G`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-206">The class `Derived` introduces a new method `F`, thus shadowing the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="52ad7-207">此示例产生以下输出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-207">The example produces the following output:</span></span>

```
Base.F
Derived.F
Derived.G
Derived.G
```

<span data-ttu-id="52ad7-208">请注意，该语句`b.G()`调用`Derived.G`，而不`Base.G`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-208">Notice that the statement `b.G()` invokes `Derived.G`, not `Base.G`.</span></span> <span data-ttu-id="52ad7-209">这是因为运行时类型的实例 (即`Derived`) 而不是实例的编译时类型 (即`Base`) 确定要调用的实际方法实现。</span><span class="sxs-lookup"><span data-stu-id="52ad7-209">This is because the run-time type of the instance (which is `Derived`) rather than the compile-time type of the instance (which is `Base`) determines the actual method implementation to invoke.</span></span>

### <a name="shared-methods"></a><span data-ttu-id="52ad7-210">共享的方法</span><span class="sxs-lookup"><span data-stu-id="52ad7-210">Shared Methods</span></span>

<span data-ttu-id="52ad7-211">`Shared`修饰符指示一种方法是*共享方法*。</span><span class="sxs-lookup"><span data-stu-id="52ad7-211">The `Shared` modifier indicates a method is a *shared method*.</span></span> <span data-ttu-id="52ad7-212">共享的方法不对特定类型的实例进行操作，并可以直接从一种类型而不是通过一种类型的特定实例进行调用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-212">A shared method does not operate on a specific instance of a type and may be invoked directly from a type rather than through a particular instance of a type.</span></span> <span data-ttu-id="52ad7-213">它是有效的但是，使用实例来限定共享的方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-213">It is valid, however, to use an instance to qualify a shared method.</span></span> <span data-ttu-id="52ad7-214">无效来指代`Me`， `MyClass`，或`MyBase`共享方法中。</span><span class="sxs-lookup"><span data-stu-id="52ad7-214">It is invalid to refer to `Me`, `MyClass`, or `MyBase` in a shared method.</span></span> <span data-ttu-id="52ad7-215">共享的方法可能`Overridable`， `NotOverridable`，或`MustOverride`，并且它们可能不会替代方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-215">Shared methods may not be `Overridable`, `NotOverridable`, or `MustOverride`, and they may not override methods.</span></span> <span data-ttu-id="52ad7-216">标准模块和接口中定义的方法不能指定`Shared`，因为它们是隐式`Shared`已。</span><span class="sxs-lookup"><span data-stu-id="52ad7-216">Methods defined in standard modules and interfaces may not specify `Shared`, because they are implicitly `Shared` already.</span></span>

<span data-ttu-id="52ad7-217">在结构或类，而声明的方法`Shared`修饰符*实例方法*。</span><span class="sxs-lookup"><span data-stu-id="52ad7-217">A method declared in a structure or class without a `Shared` modifier is an *instance method*.</span></span> <span data-ttu-id="52ad7-218">一种类型的给定实例上运行的实例方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-218">An instance method operates on a given instance of a type.</span></span> <span data-ttu-id="52ad7-219">只能通过一种类型的实例调用的实例方法和通过实例可能引用`Me`表达式。</span><span class="sxs-lookup"><span data-stu-id="52ad7-219">Instance methods can only be invoked through an instance of a type and may refer to the instance through the `Me` expression.</span></span>

<span data-ttu-id="52ad7-220">下面的示例阐释了用于访问共享和实例成员规则：</span><span class="sxs-lookup"><span data-stu-id="52ad7-220">The following example illustrates the rules for accessing shared and instance members:</span></span>

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

<span data-ttu-id="52ad7-221">方法`F`显示，在实例函数成员，标识符可以用于访问实例成员和共享的成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-221">Method `F` shows that in an instance function member, an identifier can be used to access both instance members and shared members.</span></span> <span data-ttu-id="52ad7-222">方法`G`显示在共享的函数成员中，它是一个错误，以通过标识符访问实例成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-222">Method `G` shows that in a shared function member, it is an error to access an instance member through an identifier.</span></span> <span data-ttu-id="52ad7-223">方法`Main`显示在成员访问表达式中，必须通过情况下，访问实例成员，但可以通过类型或实例访问共享的成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-223">Method `Main` shows that in a member access expression, instance members must be accessed through instances, but shared members can be accessed through types or instances.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="52ad7-224">方法参数</span><span class="sxs-lookup"><span data-stu-id="52ad7-224">Method Parameters</span></span>

<span data-ttu-id="52ad7-225">一个*参数*是一个变量，可用于传递执行和跳出执行方法的信息。</span><span class="sxs-lookup"><span data-stu-id="52ad7-225">A *parameter* is a variable that can be used to pass information into and out of a method.</span></span> <span data-ttu-id="52ad7-226">参数的一种方法被声明的方法的参数列表，其中包括一个或多个由逗号分隔的参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-226">Parameters of a method are declared by the method's parameter list, which consists of one or more parameters separated by commas.</span></span>

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

<span data-ttu-id="52ad7-227">如果未指定类型的参数和使用严格的语义，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-227">If no type is specified for a parameter and strict semantics are used, a compile-time error occurs.</span></span> <span data-ttu-id="52ad7-228">否则默认类型是`Object`或类型的参数的类型字符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-228">Otherwise the default type is `Object` or the type of the parameter's type character.</span></span> <span data-ttu-id="52ad7-229">如果一个参数包含在宽松的语义，即使`As`子句中，所有参数必须都指定类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-229">Even under permissive semantics, if one parameter includes an `As` clause, all parameters must specify types.</span></span>

<span data-ttu-id="52ad7-230">参数指定为值、 引用中可选的或 paramarray 参数的修饰符`ByVal`， `ByRef`， `Optional`，和`ParamArray`分别。</span><span class="sxs-lookup"><span data-stu-id="52ad7-230">Parameters are specified as value, reference, optional, or paramarray parameters by the modifiers `ByVal`, `ByRef`, `Optional`, and `ParamArray`, respectively.</span></span> <span data-ttu-id="52ad7-231">未指定的参数`ByRef`或`ByVal`默认为`ByVal`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-231">A parameter that does not specify `ByRef` or `ByVal` defaults to `ByVal`.</span></span>

<span data-ttu-id="52ad7-232">参数名称的作用域为整个方法的正文，并且始终是可公开访问。</span><span class="sxs-lookup"><span data-stu-id="52ad7-232">Parameter names are scoped to the entire body of the method and are always publicly accessible.</span></span> <span data-ttu-id="52ad7-233">方法调用创建一个副本，特定于该调用的参数，并调用的参数列表的值或变量引用赋给新创建的参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-233">A method invocation creates a copy, specific to that invocation, of the parameters, and the argument list of the invocation assigns values or variable references to the newly created parameters.</span></span> <span data-ttu-id="52ad7-234">由于外部方法声明和委托声明有没有正文，重复的参数名称是允许在参数列表中，但建议使用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-234">Because external method declarations and delegate declarations have no body, duplicate parameter names are allowed in parameter lists, but discouraged.</span></span>

<span data-ttu-id="52ad7-235">标识符可能跟的名称可以为 null 的修饰符`?`以指示它是可以为 null，并且还通过数组名称修饰符以指示它是一个数组。</span><span class="sxs-lookup"><span data-stu-id="52ad7-235">The identifier may be followed by the nullable name modifier `?` to indicate that it is nullable, and also by array name modifiers to indicate that it is an array.</span></span> <span data-ttu-id="52ad7-236">它们可以结合使用，例如"`ByVal x?() As Integer`"。</span><span class="sxs-lookup"><span data-stu-id="52ad7-236">They may be combined, e.g. "`ByVal x?() As Integer`".</span></span> <span data-ttu-id="52ad7-237">不允许使用显式数组边界;此外，如果可以为 null 名称修饰符存在，则则`As`子句必须存在。</span><span class="sxs-lookup"><span data-stu-id="52ad7-237">It is not allowed to use explicit array bounds; also, if the nullable name modifier is present then an `As` clause must be present.</span></span>


#### <a name="value-parameters"></a><span data-ttu-id="52ad7-238">值参数</span><span class="sxs-lookup"><span data-stu-id="52ad7-238">Value Parameters</span></span>

<span data-ttu-id="52ad7-239">一个*value 参数*使用显式声明`ByVal`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-239">A *value parameter* is declared with an explicit `ByVal` modifier.</span></span> <span data-ttu-id="52ad7-240">如果`ByVal`使用修饰符，则`ByRef`修饰符不能指定。</span><span class="sxs-lookup"><span data-stu-id="52ad7-240">If the `ByVal` modifier is used, the `ByRef` modifier may not be specified.</span></span> <span data-ttu-id="52ad7-241">Value 参数开始使用该参数属于，并且未被初始化的成员的调用替换调用中给定的参数的值的存在。</span><span class="sxs-lookup"><span data-stu-id="52ad7-241">A value parameter comes into existence with the invocation of the member the parameter belongs to, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="52ad7-242">Value 参数将返回的成员时消失。</span><span class="sxs-lookup"><span data-stu-id="52ad7-242">A value parameter ceases to exist upon return of the member.</span></span>

<span data-ttu-id="52ad7-243">一种方法被允许将新值分配给 value 参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-243">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="52ad7-244">此类分配只会影响由值参数，则表示在本地存储位置它们对实际方法调用中给定的参数无效。</span><span class="sxs-lookup"><span data-stu-id="52ad7-244">Such assignments only affect the local storage location represented by the value parameter; they have no effect on the actual argument given in the method invocation.</span></span>

<span data-ttu-id="52ad7-245">转换为方法，传递的参数的值和参数的修改不影响原始参数时，则使用值参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-245">A value parameter is used when the value of an argument is passed into a method, and modifications of the parameter do not impact the original argument.</span></span> <span data-ttu-id="52ad7-246">值参数是指其自己的变量，其中一个，它不同于相应的参数变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-246">A value parameter refers to its own variable, one that is distinct from the variable of the corresponding argument.</span></span> <span data-ttu-id="52ad7-247">通过复制相应的参数值初始化此变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-247">This variable is initialized by copying the value of the corresponding argument.</span></span> <span data-ttu-id="52ad7-248">下面的示例演示一种方法`F`具有名为的值参数`p`:</span><span class="sxs-lookup"><span data-stu-id="52ad7-248">The following example shows a method `F` that has a value parameter named `p`:</span></span>

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

<span data-ttu-id="52ad7-249">该示例的生成以下输出，即使值参数`p`修改：</span><span class="sxs-lookup"><span data-stu-id="52ad7-249">The example produces the following output, even though the value parameter `p` is modified:</span></span>

```
pre: a = 1
p = 1
post: a = 1
```

#### <a name="reference-parameters"></a><span data-ttu-id="52ad7-250">引用参数</span><span class="sxs-lookup"><span data-stu-id="52ad7-250">Reference Parameters</span></span>

<span data-ttu-id="52ad7-251">引用参数是用声明的参数`ByRef`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-251">A reference parameter is a parameter declared with a `ByRef` modifier.</span></span> <span data-ttu-id="52ad7-252">如果`ByRef`指定修饰符，则`ByVal`修饰符不能使用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-252">If the `ByRef` modifier is specified, the `ByVal` modifier may not be used.</span></span> <span data-ttu-id="52ad7-253">引用参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="52ad7-253">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="52ad7-254">相反，引用参数表示作为自变量在方法或构造函数调用中的变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-254">Instead, a reference parameter represents the variable given as the argument in the method or constructor invocation.</span></span> <span data-ttu-id="52ad7-255">从概念上讲，值的引用参数始终是相同的基础变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-255">Conceptually, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="52ad7-256">在两种模式，以作为引用参数采取行动*别名*或通过*副本中复制回。*</span><span class="sxs-lookup"><span data-stu-id="52ad7-256">Reference parameters act in two modes, either as *aliases* or through *copy-in copy-back.*</span></span>

<span data-ttu-id="52ad7-257">__别名。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-257">__Aliases.__</span></span> <span data-ttu-id="52ad7-258">参数作为调用方提供的参数的别名时，使用引用参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-258">A reference parameter is used when the parameter acts as an alias for a caller-provided argument.</span></span> <span data-ttu-id="52ad7-259">引用参数本身不定义一个变量，但而不是引用相应的参数变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-259">A reference parameter does not itself define a variable, but rather refers to the variable of the corresponding argument.</span></span> <span data-ttu-id="52ad7-260">修改引用参数直接并立即影响对应的参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-260">Modifications of a reference parameter directly and immediately impact the corresponding argument.</span></span> <span data-ttu-id="52ad7-261">下面的示例演示一种方法`Swap`具有两个引用参数：</span><span class="sxs-lookup"><span data-stu-id="52ad7-261">The following example shows a method `Swap` that has two reference parameters:</span></span>

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

<span data-ttu-id="52ad7-262">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="52ad7-262">The output of the program is:</span></span>

```
pre: x = 1, y = 2
post: x = 2, y = 1
```

<span data-ttu-id="52ad7-263">方法调用`Swap`类中`Main`，`a`表示`x,`并`b`表示`y`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-263">For the invocation of method `Swap` in class `Main`, `a` represents `x,` and `b` represents `y`.</span></span> <span data-ttu-id="52ad7-264">因此，调用具有交换的值的效果`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-264">Thus, the invocation has the effect of swapping the values of `x` and `y`.</span></span>

<span data-ttu-id="52ad7-265">在方法中采用引用参数，就可以为多个名称来表示相同的存储位置：</span><span class="sxs-lookup"><span data-stu-id="52ad7-265">In a method that takes reference parameters, it is possible for multiple names to represent the same storage location:</span></span>

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

<span data-ttu-id="52ad7-266">在示例中的方法调用`F`中`G`将传递到引用`s`两个`a`和`b`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-266">In the example the invocation of method `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="52ad7-267">因此，对于该调用，名称`s`， `a`，并`b`所有引用相同的存储位置，并且所有三个赋值修改实例变量`s`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-267">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance variable `s`.</span></span>

<span data-ttu-id="52ad7-268">__复制在反向复制。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-268">__Copy-in copy-back.__</span></span> <span data-ttu-id="52ad7-269">如果传递给引用参数的变量的类型不是与引用参数的类型兼容，或非变量 （如属性） 作为参数传递到引用参数，或调用为后期绑定的则临时分配的变量，并将其传递给引用参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-269">If the type of the variable being passed to a reference parameter is not compatible with the reference parameter's type, or if a non-variable (e.g. a property) is passed as an argument to a reference parameter, or if the invocation is late-bound, then a temporary variable is allocated and passed to the reference parameter.</span></span> <span data-ttu-id="52ad7-270">之前该方法调用，并将复制回原始变量 （如果有一个以及是否可写） 方法返回时，正在传递的值将复制到此临时变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-270">The value being passed in will be copied into this temporary variable before the method is invoked and will be copied back to the original variable (if there is one and if it's writable) when the method returns.</span></span> <span data-ttu-id="52ad7-271">因此，引用参数一定不能包含对传入的变量的具体的存储的引用，该方法退出之前对引用参数的任何更改可能不在变量中会反映。</span><span class="sxs-lookup"><span data-stu-id="52ad7-271">Thus, a reference parameter may not necessarily contain a reference to the exact storage of the variable being passed in, and any changes to the reference parameter may not be reflected in the variable until the method exits.</span></span> <span data-ttu-id="52ad7-272">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-272">For example:</span></span>

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

<span data-ttu-id="52ad7-273">在第一个调用的情况下`F`，创建一个临时变量和属性的值`G`可为其分配并传递给`F`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-273">In the case of the first invocation of `F`, a temporary variable is created and the value of the property `G` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="52ad7-274">从返回时`F`，在临时变量中的值分配到的属性`G`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-274">Upon return from `F`, the value in the temporary variable is assigned back to the property of `G`.</span></span> <span data-ttu-id="52ad7-275">在第二种情况下，创建另一个临时变量和值`d`可为其分配并传递给`F`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-275">In the second case, another temporary variable is created and the value of `d` is assigned to it and passed into `F`.</span></span> <span data-ttu-id="52ad7-276">从返回时`F`，在临时变量中的值转换回该变量的类型`Derived`，并分配给`d`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-276">When returning from `F`, the value in the temporary variable is cast back to the type of the variable, `Derived`, and assigned to `d`.</span></span> <span data-ttu-id="52ad7-277">自值后所传递回不能强制转换为`Derived`，在运行时引发异常。</span><span class="sxs-lookup"><span data-stu-id="52ad7-277">Since the value being passed back cannot be cast to `Derived`, an exception is thrown at run time.</span></span>

#### <a name="optional-parameters"></a><span data-ttu-id="52ad7-278">可选参数</span><span class="sxs-lookup"><span data-stu-id="52ad7-278">Optional Parameters</span></span>

<span data-ttu-id="52ad7-279">使用声明一个可选参数`Optional`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-279">An optional parameter is declared with the `Optional` modifier.</span></span> <span data-ttu-id="52ad7-280">后面的形参列表中的可选参数的参数也必须是可选的;如果未能指定`Optional`上的以下参数修饰符会触发编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-280">Parameters that follow an optional parameter in the formal parameter list must be optional as well; failure to specify the `Optional` modifier on the following parameters will trigger a compile-time error.</span></span> <span data-ttu-id="52ad7-281">类型为 null 的类型的一些可选参数`T?`或不可以为 null 的类型`T`必须指定一个常量表达式`e`要用作默认值，如果未指定参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-281">An optional parameter of some type nullable type `T?` or non-nullable type `T` must specify a constant expression `e` to be used as a default value if no argument is specified.</span></span> <span data-ttu-id="52ad7-282">如果`e`计算结果为`Nothing`对象，则默认值的类型*参数类型*将用作该参数默认值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-282">If `e` evaluates to `Nothing` of type Object, then the default value of the *parameter type* will be used as the default for the parameter.</span></span> <span data-ttu-id="52ad7-283">否则为`CType(e, T)`必须是常量表达式和结果作为参数的默认值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-283">Otherwise, `CType(e, T)` must be a constant expression and it is taken as the default for the parameter.</span></span>

<span data-ttu-id="52ad7-284">可选参数都是唯一一种参数上的初始值设定项无效。</span><span class="sxs-lookup"><span data-stu-id="52ad7-284">Optional parameters are the only situation in which an initializer on a parameter is valid.</span></span> <span data-ttu-id="52ad7-285">初始化始终调用表达式，不在方法主体本身的一部分完成。</span><span class="sxs-lookup"><span data-stu-id="52ad7-285">The initialization is always done as a part of the invocation expression, not within the method body itself.</span></span>

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

<span data-ttu-id="52ad7-286">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="52ad7-286">The output of the program is:</span></span>

```
x = 10, y = 20
x = 30, y = 40
```

<span data-ttu-id="52ad7-287">在委托或事件声明中，也不在 lambda 表达式中不能指定可选参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-287">Optional parameters may not be specified in delegate or event declarations, nor in lambda expressions.</span></span>

#### <a name="paramarray-parameters"></a><span data-ttu-id="52ad7-288">ParamArray 参数</span><span class="sxs-lookup"><span data-stu-id="52ad7-288">ParamArray Parameters</span></span>

<span data-ttu-id="52ad7-289">`ParamArray` 参数以声明`ParamArray`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-289">`ParamArray` parameters are declared with the `ParamArray` modifier.</span></span> <span data-ttu-id="52ad7-290">如果`ParamArray`修饰符，则`ByVal`必须在指定的修饰符，并可能会使用任何其他参数`ParamArray`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-290">If the `ParamArray` modifier is present, the `ByVal` modifier must be specified, and no other parameter may use the `ParamArray` modifier.</span></span> <span data-ttu-id="52ad7-291">`ParamArray`参数的类型必须是一维数组，并且它必须是参数列表中的最后一个参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-291">The `ParamArray` parameter's type must be a one-dimensional array, and it must be the last parameter in the parameter list.</span></span>

<span data-ttu-id="52ad7-292">一个`ParamArray`参数表示的不确定的类型的参数数目`ParamArray`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-292">A `ParamArray` parameter represents an indeterminate number of parameters of the type of the `ParamArray`.</span></span> <span data-ttu-id="52ad7-293">在该方法本身，`ParamArray`参数视为其声明的类型，并且没有任何特殊语义。</span><span class="sxs-lookup"><span data-stu-id="52ad7-293">Within the method itself, a `ParamArray` parameter is treated as its declared type and has no special semantics.</span></span> <span data-ttu-id="52ad7-294">一个`ParamArray`参数是隐式可选，默认值的类型的空一维数组的`ParamArray`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-294">A `ParamArray` parameter is implicitly optional, with a default value of an empty one-dimensional array of the type of the `ParamArray`.</span></span>

<span data-ttu-id="52ad7-295">一个`ParamArray`允许在方法调用中的两种方式之一中指定的参数：</span><span class="sxs-lookup"><span data-stu-id="52ad7-295">A `ParamArray` permits arguments to be specified in one of two ways in a method invocation:</span></span>

* <span data-ttu-id="52ad7-296">对于给定的实参`ParamArray`可以是单个表达式的类型扩大到`ParamArray`类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-296">The argument given for a `ParamArray` can be a single expression of a type that widens to the `ParamArray` type.</span></span> <span data-ttu-id="52ad7-297">在这种情况下，`ParamArray`作用与值参数的完全一样。</span><span class="sxs-lookup"><span data-stu-id="52ad7-297">In this case, the `ParamArray` acts precisely like a value parameter.</span></span>

* <span data-ttu-id="52ad7-298">或者，此调用可以指定零个或多个参数`ParamArray`，其中每个自变量是隐式转换为的元素类型的类型的表达式`ParamArray`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-298">Alternatively, the invocation can specify zero or more arguments for the `ParamArray`, where each argument is an expression of a type that is implicitly convertible to the element type of the `ParamArray`.</span></span> <span data-ttu-id="52ad7-299">在这种情况下，调用创建的实例`ParamArray`使用长度为对应的参数、 初始化该数组的元素实例与给定的参数值，并使用新创建的数组实例作为实际数类型自变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-299">In this case, the invocation creates an instance of the `ParamArray` type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="52ad7-300">除了允许数目可变的参数在调用`ParamArray`是恰好等同于值参数的相同的类型，如以下示例所示。</span><span class="sxs-lookup"><span data-stu-id="52ad7-300">Except for allowing a variable number of arguments in an invocation, a `ParamArray` is precisely equivalent to a value parameter of the same type, as the following example illustrates.</span></span>

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

<span data-ttu-id="52ad7-301">该示例生成输出</span><span class="sxs-lookup"><span data-stu-id="52ad7-301">The example produces the output</span></span>

```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="52ad7-302">第一个调用`F`只需将该数组传递`a`作为值参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-302">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="52ad7-303">第二个调用`F`自动创建具有给定的元素值的四个元素数组并将该数组实例作为值参数传递。</span><span class="sxs-lookup"><span data-stu-id="52ad7-303">The second invocation of `F` automatically creates a four-element array with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="52ad7-304">同样，第三个调用的`F`创建一个零元素的数组，并将该实例作为值参数传递。</span><span class="sxs-lookup"><span data-stu-id="52ad7-304">Likewise, the third invocation of `F` creates a zero-element array and passes that instance as a value parameter.</span></span> <span data-ttu-id="52ad7-305">第二个和第三个调用都完全等效于编写：</span><span class="sxs-lookup"><span data-stu-id="52ad7-305">The second and third invocations are precisely equivalent to writing:</span></span>

```vb
F(New Integer() {10, 20, 30, 40})
F(New Integer() {})
```

<span data-ttu-id="52ad7-306">`ParamArray` 不能在委托或事件声明中指定的参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-306">`ParamArray` parameters may not be specified in delegate or event declarations.</span></span>

### <a name="event-handling"></a><span data-ttu-id="52ad7-307">事件处理</span><span class="sxs-lookup"><span data-stu-id="52ad7-307">Event Handling</span></span>

<span data-ttu-id="52ad7-308">方法可以以声明方式处理事件引发的对象实例或共享的变量中。</span><span class="sxs-lookup"><span data-stu-id="52ad7-308">Methods can declaratively handle events raised by objects in instance or shared variables.</span></span> <span data-ttu-id="52ad7-309">若要处理的事件，方法声明指定`Handles`关键字，并列出一个或多个事件。</span><span class="sxs-lookup"><span data-stu-id="52ad7-309">To handle events, a method declaration specifies the `Handles` keyword and lists one or more events.</span></span>

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

<span data-ttu-id="52ad7-310">中的事件`Handles`由句点分隔的两个标识符指定列表：</span><span class="sxs-lookup"><span data-stu-id="52ad7-310">An event in the `Handles` list is specified by two identifiers separated by a period:</span></span>

* <span data-ttu-id="52ad7-311">第一个标识符必须是实例或共享中包含指定的类型的变量`WithEvents`修饰符或`MyBase`或`MyClass`或`Me`关键字; 否则，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-311">The first identifier must be an instance or shared variable in the containing type that specifies the `WithEvents` modifier or the `MyBase` or `MyClass` or `Me` keyword; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="52ad7-312">此变量包含的对象，将引发此方法处理的事件。</span><span class="sxs-lookup"><span data-stu-id="52ad7-312">This variable contains the object that will raise the events handled by this method.</span></span>

* <span data-ttu-id="52ad7-313">第二个标识符必须指定的第一个标识符类型的成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-313">The second identifier must specify a member of the type of the first identifier.</span></span> <span data-ttu-id="52ad7-314">该成员必须是一个事件，并可能会共享。</span><span class="sxs-lookup"><span data-stu-id="52ad7-314">The member must be an event, and may be shared.</span></span> <span data-ttu-id="52ad7-315">如果共享的变量指定为第一个标识符，然后必须共享该事件，或则会导致错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-315">If a shared variable is specified for the first identifier, then the event must be shared, or an error results.</span></span>

<span data-ttu-id="52ad7-316">处理程序方法`M`被视为一个事件的有效的事件处理程序`E`如果语句`AddHandler E, AddressOf M`也会有效。</span><span class="sxs-lookup"><span data-stu-id="52ad7-316">A handler method `M` is considered a valid event handler for an event `E` if the statement `AddHandler E, AddressOf M` would also be valid.</span></span> <span data-ttu-id="52ad7-317">与不同`AddHandler`语句，但是，显式事件处理程序允许处理具有不带任何参数而不考虑是否严格的语义正在使用或不方法的事件：</span><span class="sxs-lookup"><span data-stu-id="52ad7-317">Unlike an `AddHandler` statement, however, explicit event handlers allow handling an event with a method with no arguments regardless of whether strict semantics are being used or not:</span></span>

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

<span data-ttu-id="52ad7-318">单个成员可以处理多个匹配的事件，并且多个方法可处理单个事件。</span><span class="sxs-lookup"><span data-stu-id="52ad7-318">A single member can handle multiple matching events, and multiple methods may handle a single event.</span></span> <span data-ttu-id="52ad7-319">方法的可访问性对其能够处理事件无效。</span><span class="sxs-lookup"><span data-stu-id="52ad7-319">A method's accessibility has no effect on its ability to handle events.</span></span> <span data-ttu-id="52ad7-320">下面的示例显示了一种方法可以如何处理事件：</span><span class="sxs-lookup"><span data-stu-id="52ad7-320">The following example shows how a method can handle events:</span></span>

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

<span data-ttu-id="52ad7-321">这将打印出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-321">This will print out:</span></span>

```
Raised
Raised
```

<span data-ttu-id="52ad7-322">一个类型继承其基类型由提供的所有事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-322">A type inherits all event handlers provided by its base type.</span></span> <span data-ttu-id="52ad7-323">派生的类型以任何方式不能更改它继承自其基类型，但可能会将附加处理程序添加到该事件的事件映射。</span><span class="sxs-lookup"><span data-stu-id="52ad7-323">A derived type cannot in any way alter the event mappings it inherits from its base types, but may add additional handlers to the event.</span></span>


### <a name="extension-methods"></a><span data-ttu-id="52ad7-324">扩展方法</span><span class="sxs-lookup"><span data-stu-id="52ad7-324">Extension Methods</span></span>

<span data-ttu-id="52ad7-325">方法可以添加到从外部使用类型声明的类型*扩展方法*。</span><span class="sxs-lookup"><span data-stu-id="52ad7-325">Methods can be added to types from outside of the type declaration using *extension methods*.</span></span> <span data-ttu-id="52ad7-326">扩展方法是使用方法`System.Runtime.CompilerServices.ExtensionAttribute`特性应用于它们。</span><span class="sxs-lookup"><span data-stu-id="52ad7-326">Extension methods are methods with the `System.Runtime.CompilerServices.ExtensionAttribute` attribute applied to them.</span></span> <span data-ttu-id="52ad7-327">它们只能在标准模块中声明，并且必须具有至少一个参数，指定该方法的类型扩展。</span><span class="sxs-lookup"><span data-stu-id="52ad7-327">They can only be declared in standard modules and must have at least one parameter, which specifies the type the method extends.</span></span> <span data-ttu-id="52ad7-328">例如，以下扩展方法扩展了类型`String`:</span><span class="sxs-lookup"><span data-stu-id="52ad7-328">For example, the following extension method extends the type `String`:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension> _
    Sub Print(s As String)
        Console.WriteLine(s)
    End Sub
End Module
```

<span data-ttu-id="52ad7-329">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-329">__Note.__</span></span> <span data-ttu-id="52ad7-330">尽管 Visual Basic 要求标准模块中声明的扩展方法，但 C# 等其他语言可能会给他们在其他类型的类型中声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-330">Although Visual Basic requires extension methods to be declared in a standard module, other languages such as C# may allow them to be declared in other kinds of types.</span></span> <span data-ttu-id="52ad7-331">只要方法遵循此处所述的其他约定，并包含类型不是开放式泛型类型，不能被实例化，Visual Basic 可以识别的扩展方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-331">As long as the methods follow the other conventions outlined here and the containing type is not an open generic type and cannot be instantiated, Visual Basic will recognize the extension methods.</span></span>

<span data-ttu-id="52ad7-332">调用扩展方法时，对其调用它的实例传递到第一个参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-332">When an extension method is invoked, the instance it is being invoked on is passed to the first parameter.</span></span> <span data-ttu-id="52ad7-333">不能声明为第一个参数`Optional`或`ParamArray`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-333">The first parameter cannot be declared `Optional` or `ParamArray`.</span></span> <span data-ttu-id="52ad7-334">任何类型，包括一个类型参数，可以显示为扩展方法的第一个参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-334">Any type, including a type parameter, can appear as the first parameter of an extension method.</span></span> <span data-ttu-id="52ad7-335">例如，以下方法来扩展类型`Integer()`，实现的任何类型`System.Collections.Generic.IEnumerable(Of T)`，并在所有的任何类型：</span><span class="sxs-lookup"><span data-stu-id="52ad7-335">For example, the following methods extend the types `Integer()`, any type that implements `System.Collections.Generic.IEnumerable(Of T)`, and any type at all:</span></span>

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

<span data-ttu-id="52ad7-336">如前面的示例所示，可以扩展接口。</span><span class="sxs-lookup"><span data-stu-id="52ad7-336">As the previous example shows, interfaces can be extended.</span></span> <span data-ttu-id="52ad7-337">接口的扩展方法提供方法的实现，因此实现具有仍然只在其上定义的扩展方法的接口的类型可实现由接口最初声明的成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-337">Interface extension methods supply the implementation of the method, so types that implement an interface that has extension methods defined on it still only implement the members originally declared by the interface.</span></span> <span data-ttu-id="52ad7-338">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-338">For example:</span></span>

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

<span data-ttu-id="52ad7-339">扩展方法也具有其类型参数的类型约束，并只需为使用非扩展泛型方法类型参数可以推断出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-339">Extension methods can also have type constraints on their type parameters and, just as with non-extension generic methods, type argument can be inferred:</span></span>

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

<span data-ttu-id="52ad7-340">此外可以通过在要扩展的类型的隐式实例表达式访问扩展方法：</span><span class="sxs-lookup"><span data-stu-id="52ad7-340">Extension methods can also be accessed through implicit instance expressions within the type being extended:</span></span>

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

<span data-ttu-id="52ad7-341">用于可访问性的目的，扩展方法也被视为标准模块中声明-他们无权额外访问到它们所扩展超出凭借其声明上下文的访问类型的成员的成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-341">For the purposes of accessibility, extension methods are also treated as members of the standard module they are declared in -- they have no extra access to the members of the type they are extending beyond the access they have by virtue of their declaration context.</span></span>

<span data-ttu-id="52ad7-342">标准模块方法在作用域中时，扩展方法才可用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-342">Extensions methods are only available when the standard module method is in scope.</span></span> <span data-ttu-id="52ad7-343">否则，原始类型将不显示已扩展。</span><span class="sxs-lookup"><span data-stu-id="52ad7-343">Otherwise, the original type will not appear to have been extended.</span></span> <span data-ttu-id="52ad7-344">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-344">For example:</span></span>

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

<span data-ttu-id="52ad7-345">当仅类型上的扩展方法可用时引用类型仍将生成编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-345">Referring to a type when only an extension method on the type is available will still produce a compile-time error.</span></span>

<span data-ttu-id="52ad7-346">务必要注意的扩展方法被视为是所有上下文中的类型的成员成员绑定到的位置，如强类型`For Each`模式。</span><span class="sxs-lookup"><span data-stu-id="52ad7-346">It is important to note that extension methods are considered to be members of the type in all contexts where members are bound, such as the strongly-typed `For Each` pattern.</span></span> <span data-ttu-id="52ad7-347">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-347">For example:</span></span>

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

<span data-ttu-id="52ad7-348">委托也可以创建引用的扩展方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-348">Delegates can also be created that refer to extension methods.</span></span> <span data-ttu-id="52ad7-349">因此，代码：</span><span class="sxs-lookup"><span data-stu-id="52ad7-349">Thus, the code:</span></span>

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

<span data-ttu-id="52ad7-350">是大致相当于：</span><span class="sxs-lookup"><span data-stu-id="52ad7-350">is roughly equivalent to:</span></span>

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

<span data-ttu-id="52ad7-351">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-351">__Note.__</span></span> <span data-ttu-id="52ad7-352">通常情况下 Visual Basic 会插入会导致一个实例方法调用的检查`System.NullReferenceException`实例调用方法时是否发生`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-352">Visual Basic normally inserts a check on an instance method call that causes a `System.NullReferenceException` to occur if the instance the method is being invoked on is `Nothing`.</span></span> <span data-ttu-id="52ad7-353">在扩展方法的情况下没有有效方法来插入此检查，因此需要显式检查扩展方法`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-353">In the case of extension methods, there is no efficient way to insert this check, so extension methods will need to explicitly check for `Nothing`.</span></span> 

<span data-ttu-id="52ad7-354">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-354">__Note.__</span></span> <span data-ttu-id="52ad7-355">将装箱值类型，当作为传递`ByVal`到参数的参数类型化为接口。</span><span class="sxs-lookup"><span data-stu-id="52ad7-355">A value type will be boxed when being passed as a `ByVal` argument to a parameter typed as an interface.</span></span>  <span data-ttu-id="52ad7-356">这意味着副作用的扩展方法将对而不是原始结构的副本。</span><span class="sxs-lookup"><span data-stu-id="52ad7-356">This implies that side effects of the extension method will operate on a copy of the structure instead of the original.</span></span> <span data-ttu-id="52ad7-357">虽然语言将在扩展方法的第一个参数上不受限制，但建议的扩展方法不使用扩展值类型或在扩展时的值类型，第一个参数传递`ByRef`以确保该侧对原始值执行操作效果。</span><span class="sxs-lookup"><span data-stu-id="52ad7-357">While the language puts no restrictions on the first argument of an extension method, it is recommended that extension methods are not used to extend value types or that when extending value types, the first parameter is passed `ByRef` to ensure that side effects operate on the original value.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="52ad7-358">分部方法</span><span class="sxs-lookup"><span data-stu-id="52ad7-358">Partial Methods</span></span>

<span data-ttu-id="52ad7-359">一个*分部方法*是一种方法，指定一个签名，但没有方法正文。</span><span class="sxs-lookup"><span data-stu-id="52ad7-359">A *partial method* is a method that specifies a signature but not the body of the method.</span></span> <span data-ttu-id="52ad7-360">方法的主体可以提供另一个方法声明具有相同的名称和签名，很可能出在另一个分部声明的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-360">The body of the method can be supplied by another method declaration with the same name and signature, most likely in another partial declaration of the type.</span></span> <span data-ttu-id="52ad7-361">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-361">For example:</span></span>

<span data-ttu-id="52ad7-362">a.vb:</span><span class="sxs-lookup"><span data-stu-id="52ad7-362">a.vb:</span></span>

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

<span data-ttu-id="52ad7-363">b.vb:</span><span class="sxs-lookup"><span data-stu-id="52ad7-363">b.vb:</span></span>

```vb
Public Partial Class MyForm
    Public Sub ValidateControls()
        ' Validation logic goes here
        ...
    End Sub
End Class
```

<span data-ttu-id="52ad7-364">在此示例中，类的分部声明`MyForm`声明的分部方法`ValidateControls`没有实现。</span><span class="sxs-lookup"><span data-stu-id="52ad7-364">In this example, a partial declaration of the class `MyForm` declares a partial method `ValidateControls` with no implementation.</span></span> <span data-ttu-id="52ad7-365">分部声明中的构造函数调用的分部方法，即使没有正文文件中提供。</span><span class="sxs-lookup"><span data-stu-id="52ad7-365">The constructor in the partial declaration calls the partial method, even though there is no body supplied in the file.</span></span> <span data-ttu-id="52ad7-366">其他分部声明`MyForm`然后提供该方法的实现。</span><span class="sxs-lookup"><span data-stu-id="52ad7-366">The other partial declaration of `MyForm` then supplies the implementation of the method.</span></span>

<span data-ttu-id="52ad7-367">分部方法可以调用而不考虑是否提供了一个主体;如果提供没有方法正文，则将忽略此调用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-367">Partial methods can be called regardless of whether a body has been supplied; if no method body is supplied, the call is ignored.</span></span> <span data-ttu-id="52ad7-368">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-368">For example:</span></span>

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

<span data-ttu-id="52ad7-369">此外忽略并不会评估中作为参数传递到分部方法调用，将忽略任何表达式。</span><span class="sxs-lookup"><span data-stu-id="52ad7-369">Any expressions that are passed in as arguments to a partial method call that is ignored are ignored also and not evaluated.</span></span> <span data-ttu-id="52ad7-370">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-370">(__Note.__</span></span> <span data-ttu-id="52ad7-371">也就是说，分部方法是提供跨两个分部类型定义，因为分部方法具有无需付费，如果不使用它们的行为的极其有效的方式。）</span><span class="sxs-lookup"><span data-stu-id="52ad7-371">This means that partial methods are a very efficient way of providing behavior that is defined across two partial types, since the partial methods have no cost if they are not used.)</span></span>

<span data-ttu-id="52ad7-372">分部方法声明必须声明为`Private`和必须始终为不包含任何语句在其主体中的子例程。</span><span class="sxs-lookup"><span data-stu-id="52ad7-372">The partial method declaration must be declared as `Private` and must always be a subroutine with no statements in its body.</span></span> <span data-ttu-id="52ad7-373">分部方法不能自行实现接口方法，虽然可以提供其主体的方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-373">Partial methods cannot themselves implement interface methods, although the method that supplies their body can.</span></span>

<span data-ttu-id="52ad7-374">只有一种方法可以提供分部方法的正文。</span><span class="sxs-lookup"><span data-stu-id="52ad7-374">Only one method can supply a body to a partial method.</span></span> <span data-ttu-id="52ad7-375">方法提供正文转换为分部方法必须具有相同的签名作为分部方法、 任何类型参数、 相同的声明修饰符，以及相同的参数和类型参数名称相同的约束。</span><span class="sxs-lookup"><span data-stu-id="52ad7-375">A method supplying a body to a partial method must have the same signature as the partial method, the same constraints on any type parameters, the same declaration modifiers, and the same parameter and type parameter names.</span></span> <span data-ttu-id="52ad7-376">将合并分部方法，并提供其正文的方法的属性，因为这些方法的参数的任何属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-376">Attributes on the partial method and the method that supplies its body are merged, as are any attributes on the methods' parameters.</span></span> <span data-ttu-id="52ad7-377">同样，合并的方法处理的事件列表。</span><span class="sxs-lookup"><span data-stu-id="52ad7-377">Similarly, the list of events that the methods handle is merged.</span></span> <span data-ttu-id="52ad7-378">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-378">For example:</span></span>

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

## <a name="constructors"></a><span data-ttu-id="52ad7-379">构造函数</span><span class="sxs-lookup"><span data-stu-id="52ad7-379">Constructors</span></span>

<span data-ttu-id="52ad7-380">*构造函数*是允许控制初始化的特殊方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-380">*Constructors* are special methods that allow control over initialization.</span></span> <span data-ttu-id="52ad7-381">运行程序开始运行后，或创建类型的实例时。</span><span class="sxs-lookup"><span data-stu-id="52ad7-381">They are run after the program begins or when an instance of a type is created.</span></span> <span data-ttu-id="52ad7-382">与其他成员不同构造函数不会继承和不到类型的声明空间引入一个名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-382">Unlike other members, constructors are not inherited and do not introduce a name into a type's declaration space.</span></span> <span data-ttu-id="52ad7-383">通过对象创建表达式或.NET Framework 中; 只被调用构造函数可能永远不会直接调用它们。</span><span class="sxs-lookup"><span data-stu-id="52ad7-383">Constructors may only be invoked by object-creation expressions or by the .NET Framework; they may never be directly invoked.</span></span>

<span data-ttu-id="52ad7-384">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-384">__Note.__</span></span> <span data-ttu-id="52ad7-385">构造函数具有同样的限制在子例程具有的行位置。</span><span class="sxs-lookup"><span data-stu-id="52ad7-385">Constructors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="52ad7-386">开始语句，end 语句和块必须出现在逻辑行的开头开始。</span><span class="sxs-lookup"><span data-stu-id="52ad7-386">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

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

### <a name="instance-constructors"></a><span data-ttu-id="52ad7-387">实例构造函数</span><span class="sxs-lookup"><span data-stu-id="52ad7-387">Instance Constructors</span></span>

<span data-ttu-id="52ad7-388">*实例构造函数*初始化类型的实例并创建实例时运行的.NET Framework。</span><span class="sxs-lookup"><span data-stu-id="52ad7-388">*Instance constructors* initialize instances of a type and are run by the .NET Framework when an instance is created.</span></span> <span data-ttu-id="52ad7-389">构造函数的参数列表受到一种方法的参数列表相同的规则。</span><span class="sxs-lookup"><span data-stu-id="52ad7-389">The parameter list of a constructor is subject to the same rules as the parameter list of a method.</span></span> <span data-ttu-id="52ad7-390">可能会重载实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-390">Instance constructors may be overloaded.</span></span>

<span data-ttu-id="52ad7-391">引用类型中的所有构造函数必须调用另一个构造函数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-391">All constructors in reference types must invoke another constructor.</span></span> <span data-ttu-id="52ad7-392">如果显式调用，它必须是在构造函数方法主体中的第一个语句。</span><span class="sxs-lookup"><span data-stu-id="52ad7-392">If the invocation is explicit, it must be the first statement in the constructor method body.</span></span> <span data-ttu-id="52ad7-393">该语句可以调用另一个类型的实例构造函数-例如，`Me.New(...)`或`MyClass.New(...)`-或如果它不是一种结构它可以调用类型的基类型的实例构造函数-例如， `MyBase.New(...)`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-393">The statement can either invoke another of the type's instance constructors -- for example, `Me.New(...)` or `MyClass.New(...)` -- or if it is not a structure it can invoke an instance constructor of the type's base type -- for example, `MyBase.New(...)`.</span></span> <span data-ttu-id="52ad7-394">它是无效的构造函数调用自身。</span><span class="sxs-lookup"><span data-stu-id="52ad7-394">It is invalid for a constructor to invoke itself.</span></span> <span data-ttu-id="52ad7-395">如果一个构造函数忽略另一个构造函数的调用`MyBase.New()`是隐式的。</span><span class="sxs-lookup"><span data-stu-id="52ad7-395">If a constructor omits a call to another constructor, `MyBase.New()` is implicit.</span></span> <span data-ttu-id="52ad7-396">如果没有无参数的基类型的构造函数，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-396">If there is no parameterless base type constructor, a compile-time error occurs.</span></span> <span data-ttu-id="52ad7-397">因为`Me`将不被视为之前对基类构造函数调用后, 向构造函数调用语句参数不能引用构造`Me`， `MyClass`，或`MyBase`隐式或显式。</span><span class="sxs-lookup"><span data-stu-id="52ad7-397">Because `Me` is not considered to be constructed until after the call to a base class constructor, the parameters to a constructor invocation statement cannot reference `Me`, `MyClass`, or `MyBase` implicitly or explicitly.</span></span>

<span data-ttu-id="52ad7-398">构造函数的第一条语句时窗体的`MyBase.New(...)`，构造函数隐式执行指定的变量的初始值设定项的类型中声明的实例变量的初始化。</span><span class="sxs-lookup"><span data-stu-id="52ad7-398">When a constructor's first statement is of the form `MyBase.New(...)`, the constructor implicitly performs the initializations specified by the variable initializers of the instance variables declared in the type.</span></span> <span data-ttu-id="52ad7-399">这对应于一系列调用的直接基类型构造函数后立即执行的分配。</span><span class="sxs-lookup"><span data-stu-id="52ad7-399">This corresponds to a sequence of assignments that are executed immediately after invoking the direct base type constructor.</span></span> <span data-ttu-id="52ad7-400">这种顺序可确保有权访问该实例的任何语句执行之前，所有基本实例变量进行初始化其变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="52ad7-400">Such ordering ensures that all base instance variables are initialized by their variable initializers before any statements that have access to the instance are executed.</span></span> <span data-ttu-id="52ad7-401">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-401">For example:</span></span>

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

<span data-ttu-id="52ad7-402">当`New B()`用于创建实例`B`，将生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-402">When `New B()` is used to create an instance of `B`, the following output is produced:</span></span>

```
x = 1, y = 1
```

<span data-ttu-id="52ad7-403">值`y`是`1`因为调用基类构造函数后，将执行变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="52ad7-403">The value of `y` is `1` because the variable initializer is executed after the base class constructor is invoked.</span></span> <span data-ttu-id="52ad7-404">变量初始值设定项与它们在类型声明中显示文本的顺序执行。</span><span class="sxs-lookup"><span data-stu-id="52ad7-404">Variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="52ad7-405">一种类型只声明`Private`构造函数，它不能在一般情况下对于其他类型派生的类型或创建该类型的实例; 唯一的例外是嵌套类型中的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-405">When a type declares only `Private` constructors, it is not possible in general for other types to derive from the type or create instances of the type; the only exception is types nested within the type.</span></span> <span data-ttu-id="52ad7-406">`Private` 构造函数通常用在仅包含类型`Shared`成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-406">`Private` constructors are commonly used in types that contain only `Shared` members.</span></span>

<span data-ttu-id="52ad7-407">如果类型不包含任何实例构造函数声明，会自动提供默认构造函数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-407">If a type contains no instance constructor declarations, a default constructor is automatically provided.</span></span> <span data-ttu-id="52ad7-408">默认构造函数只是调用的直接基类型的无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-408">The default constructor simply invokes the parameterless constructor of the direct base type.</span></span> <span data-ttu-id="52ad7-409">如果直接基类型不具有可访问的无参数构造函数，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-409">If the direct base type does not have an accessible parameterless constructor, a compile-time error occurs.</span></span> <span data-ttu-id="52ad7-410">默认构造函数的声明的访问类型是`Public`除非该类型是`MustInherit`，在这种情况下的默认构造函数是`Protected`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-410">The declared access type for the default constructor is `Public` unless the type is `MustInherit`, in which case the default constructor is `Protected`.</span></span>

<span data-ttu-id="52ad7-411">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-411">__Note.__</span></span> <span data-ttu-id="52ad7-412">默认访问权限`MustInherit`类型的默认构造函数是`Protected`因为`MustInherit`不能直接创建类。</span><span class="sxs-lookup"><span data-stu-id="52ad7-412">The default access for a `MustInherit` type's default constructor is `Protected` because `MustInherit` classes cannot be created directly.</span></span> <span data-ttu-id="52ad7-413">因此将默认构造函数没有必要`Public`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-413">So there is no point in making the default constructor `Public`.</span></span>

<span data-ttu-id="52ad7-414">在下面的示例提供一个默认构造函数，因为此类不包含任何构造函数声明：</span><span class="sxs-lookup"><span data-stu-id="52ad7-414">In the following example a default constructor is provided because the class contains no constructor declarations:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String
End Class
```

<span data-ttu-id="52ad7-415">因此，此示例是完全等效于以下：</span><span class="sxs-lookup"><span data-stu-id="52ad7-415">Thus, the example is precisely equivalent to the following:</span></span>

```vb
Class Message
    Dim sender As Object
    Dim text As String

    Sub New()
    End Sub
End Class
```

<span data-ttu-id="52ad7-416">将发送到设计器的默认构造函数生成的类中使用特性标记`Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute`将调用的方法`Sub InitializeComponent()`，如果它存在于基构造函数的调用之后。</span><span class="sxs-lookup"><span data-stu-id="52ad7-416">Default constructors that are emitted into a designer generated class marked with the attribute `Microsoft.VisualBasic.CompilerServices.DesignerGeneratedAttribute` will call the method `Sub InitializeComponent()`, if it exists, after the call to the base constructor.</span></span> <span data-ttu-id="52ad7-417">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-417">(__Note.__</span></span> <span data-ttu-id="52ad7-418">这允许设计器生成的文件，例如那些通过 WinForms 设计器，以忽略此参数的构造函数中的设计器文件。</span><span class="sxs-lookup"><span data-stu-id="52ad7-418">This allows designer generated files, such as those created by the WinForms designer, to omit the constructor in the designer file.</span></span> <span data-ttu-id="52ad7-419">这使程序员能够指定其自身，选择性地。）</span><span class="sxs-lookup"><span data-stu-id="52ad7-419">This enables the programmer to specify it themselves, if they so choose.)</span></span>

### <a name="shared-constructors"></a><span data-ttu-id="52ad7-420">共享的构造函数</span><span class="sxs-lookup"><span data-stu-id="52ad7-420">Shared Constructors</span></span>

<span data-ttu-id="52ad7-421">*共享的构造函数*初始化类型共享变量; 它们运行该程序开始执行之后,、 但在对类型的成员的任何引用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-421">*Shared constructors* initialize a type's shared variables; they are run after the program begins executing, but before any references to a member of the type.</span></span> <span data-ttu-id="52ad7-422">共享的构造函数指定`Shared`修饰符，除非它在这种情况下处于标准模块`Shared`修饰符为隐式。</span><span class="sxs-lookup"><span data-stu-id="52ad7-422">A shared constructor specifies the `Shared` modifier, unless it is in a standard module in which case the `Shared` modifier is implied.</span></span>

<span data-ttu-id="52ad7-423">与实例构造函数，不同共享构造函数具有隐式的公共访问，没有任何参数，和可以不调用其他构造函数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-423">Unlike instance constructors, shared constructors have implicit public access, have no parameters, and may not call other constructors.</span></span> <span data-ttu-id="52ad7-424">之前共享的构造函数中的第一个语句，共享的构造函数隐式执行指定的共享变量类型中声明的变量的初始值设定项初始化。</span><span class="sxs-lookup"><span data-stu-id="52ad7-424">Before the first statement in a shared constructor, the shared constructor implicitly performs the initializations specified by the variable initializers of the shared variables declared in the type.</span></span> <span data-ttu-id="52ad7-425">这对应于分配给构造函数在输入时立即执行的一系列。</span><span class="sxs-lookup"><span data-stu-id="52ad7-425">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor.</span></span> <span data-ttu-id="52ad7-426">变量初始值设定项与它们在类型声明中显示文本的顺序执行。</span><span class="sxs-lookup"><span data-stu-id="52ad7-426">The variable initializers are executed in the textual order they appear in the type declaration.</span></span>

<span data-ttu-id="52ad7-427">下面的示例演示`Employee`使用共享的构造函数初始化共享的变量的类：</span><span class="sxs-lookup"><span data-stu-id="52ad7-427">The following example shows an `Employee` class with a shared constructor that initializes a shared variable:</span></span>

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

<span data-ttu-id="52ad7-428">对于每个关闭的泛型类型存在单独的共享构造函数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-428">A separate shared constructor exists for each closed generic type.</span></span> <span data-ttu-id="52ad7-429">因为共享的构造函数执行一次性的每个封闭类型，它是很方便地强制执行不能在编译时通过约束检查的类型参数的运行时检查。</span><span class="sxs-lookup"><span data-stu-id="52ad7-429">Because the shared constructor is executed exactly once for each closed type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints.</span></span> <span data-ttu-id="52ad7-430">例如，以下类型使用共享的构造函数强制执行，类型参数是`Integer`或`Double`:</span><span class="sxs-lookup"><span data-stu-id="52ad7-430">For example, the following type uses a shared constructor to enforce that the type parameter is `Integer` or `Double`:</span></span>

```vb
Class EnumHolder(Of T)
    Shared Sub New() 
        If Not GetType(T).IsEnum() Then
            Throw New ArgumentException("T must be an enumerated type.")
        End If
    End Sub
End Class
```

<span data-ttu-id="52ad7-431">完全共享的构造函数运行时是最依赖，实现的即使如果显式定义共享的构造函数提供了一些保证：</span><span class="sxs-lookup"><span data-stu-id="52ad7-431">Exactly when shared constructors are run is mostly implementation dependent, though several guarantees are provided if a shared constructor is explicitly defined:</span></span>

* <span data-ttu-id="52ad7-432">共享的构造函数之前在首次访问运行到类型的任何静态字段。</span><span class="sxs-lookup"><span data-stu-id="52ad7-432">Shared constructors are run before the first access to any static field of the type.</span></span>

* <span data-ttu-id="52ad7-433">共享的构造函数的类型的任何静态方法的第一个调用之前运行。</span><span class="sxs-lookup"><span data-stu-id="52ad7-433">Shared constructors are run before the first invocation of any static method of the type.</span></span>

* <span data-ttu-id="52ad7-434">共享的构造任何的函数类型的构造函数的第一个调用之前运行。</span><span class="sxs-lookup"><span data-stu-id="52ad7-434">Shared constructors are run before the first invocation of any constructor for the type.</span></span>

<span data-ttu-id="52ad7-435">在共享的构造函数隐式创建的共享初始值设定项的情况下不适用的上述保证。</span><span class="sxs-lookup"><span data-stu-id="52ad7-435">The above guarantees do not apply in the situation where a shared constructor is implicitly created for shared initializers.</span></span> <span data-ttu-id="52ad7-436">下面的示例的输出是不确定的因为加载的确切顺序并因此共享的构造函数的执行未定义：</span><span class="sxs-lookup"><span data-stu-id="52ad7-436">The output from the following example is uncertain, because the exact ordering of loading and therefore of shared constructor execution is not defined:</span></span>

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

<span data-ttu-id="52ad7-437">输出可能是以下之一：</span><span class="sxs-lookup"><span data-stu-id="52ad7-437">The output could be either of the following:</span></span>

```
Init A
A.F
Init B
B.F
```

<span data-ttu-id="52ad7-438">或</span><span class="sxs-lookup"><span data-stu-id="52ad7-438">or</span></span>

```
Init B
Init A
A.F
B.F
```

<span data-ttu-id="52ad7-439">与此相反，下面的示例生成可预测的输出。</span><span class="sxs-lookup"><span data-stu-id="52ad7-439">By contrast, the following example produces predictable output.</span></span> <span data-ttu-id="52ad7-440">请注意，`Shared`类构造函数`A`永远不会执行，即使类`B`从其派生：</span><span class="sxs-lookup"><span data-stu-id="52ad7-440">Note that the `Shared` constructor for the class `A` never executes, even though class `B` derives from it:</span></span>

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

<span data-ttu-id="52ad7-441">输出为：</span><span class="sxs-lookup"><span data-stu-id="52ad7-441">The output is:</span></span>

```
Init B
B.G
```

<span data-ttu-id="52ad7-442">还有可能构造循环依赖项，以便`Shared`具有变量初始值设定项在其默认观察到的变量值状态，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="52ad7-442">It is also possible to construct circular dependencies that allow `Shared` variables with variable initializers to be observed in their default value state, as in the following example:</span></span>

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

<span data-ttu-id="52ad7-443">这将生成输出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-443">This produces the output:</span></span>

```
X = 1, Y = 2
```

<span data-ttu-id="52ad7-444">若要执行`Main`方法，系统首先加载类`B`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-444">To execute the `Main` method, the system first loads class `B`.</span></span> <span data-ttu-id="52ad7-445">`Shared`类的构造函数`B`计算的初始值将继续`Y`，哪些以递归方式导致类`A`若要加载，因为值的`A.X`引用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-445">The `Shared` constructor of class `B` proceeds to compute the initial value of `Y`, which recursively causes class `A` to be loaded because the value of `A.X` is referenced.</span></span> <span data-ttu-id="52ad7-446">`Shared`类的构造函数`A`又继续计算的初始值`X`，并在此过程中，提取*默认*的值`Y`，这是零。</span><span class="sxs-lookup"><span data-stu-id="52ad7-446">The `Shared` constructor of class `A` in turn proceeds to compute the initial value of `X`, and in doing so fetches the *default* value of `Y`, which is zero.</span></span> <span data-ttu-id="52ad7-447">`A.X` 因此初始化为`1`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-447">`A.X` is thus initialized to `1`.</span></span> <span data-ttu-id="52ad7-448">加载过程`A`然后结束，返回到初始值的计算`Y`，其结果成为`2`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-448">The process of loading `A` then completes, returning to the calculation of the initial value of `Y`, the result of which becomes `2`.</span></span>

<span data-ttu-id="52ad7-449">有`Main`方法改为已经找到在类中`A`，该示例将会生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-449">Had the `Main` method instead been located in class `A`, the example would have produced the following output:</span></span>

```
X = 2, Y = 1
```

<span data-ttu-id="52ad7-450">避免循环引用中的`Shared`变量初始值设定项由于它是通常无法确定哪些类中包含此类引用的加载的顺序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-450">Avoid circular references in `Shared` variable initializers since it is generally impossible to determine the order in which classes containing such references are loaded.</span></span>

## <a name="events"></a><span data-ttu-id="52ad7-451">事件</span><span class="sxs-lookup"><span data-stu-id="52ad7-451">Events</span></span>

<span data-ttu-id="52ad7-452">事件用于通知的特定匹配项的代码。</span><span class="sxs-lookup"><span data-stu-id="52ad7-452">Events are used to notify code of a particular occurrence.</span></span> <span data-ttu-id="52ad7-453">事件声明包含的标识符，一个委托类型或参数列表中，和一个可选`Implements`子句。</span><span class="sxs-lookup"><span data-stu-id="52ad7-453">An event declaration consists of an identifier, either a delegate type or a parameter list, and an optional `Implements` clause.</span></span>

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

<span data-ttu-id="52ad7-454">如果指定的委托类型，则委托类型可能没有返回类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-454">If a delegate type is specified, the delegate type may not have a return type.</span></span> <span data-ttu-id="52ad7-455">如果指定的参数列表，则可能不包含`Optional`或`ParamArray`参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-455">If a parameter list is specified, it may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="52ad7-456">参数类型和/或委托类型的可访问域必须相同，或超集，事件本身的可访问域。</span><span class="sxs-lookup"><span data-stu-id="52ad7-456">The accessibility domain of the parameter types and/or delegate type must be the same as, or a superset of, the accessibility domain of the event itself.</span></span> <span data-ttu-id="52ad7-457">可以通过指定共享事件`Shared`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-457">Events may be shared by specifying the `Shared` modifier.</span></span>

<span data-ttu-id="52ad7-458">除了成员名称添加到该类型的声明空间，事件声明隐式声明多个其他成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-458">In addition to the member name added to the type's declaration space, an event declaration implicitly declares several other members.</span></span> <span data-ttu-id="52ad7-459">给定事件名为`X`，以下成员添加到声明空间：</span><span class="sxs-lookup"><span data-stu-id="52ad7-459">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="52ad7-460">如果声明的形式，方法声明嵌套的委托类名为`XEventHandler`引入的。</span><span class="sxs-lookup"><span data-stu-id="52ad7-460">If the form of the declaration is a method declaration, a nested delegate class named `XEventHandler` is introduced.</span></span> <span data-ttu-id="52ad7-461">嵌套的委托类匹配的方法声明，并且包含与事件相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-461">The nested delegate class matches the method declaration and has the same accessibility as the event.</span></span> <span data-ttu-id="52ad7-462">在参数列表中的特性应用于委托类的参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-462">The attributes in the parameter list apply to the parameters of the delegate class.</span></span>

* <span data-ttu-id="52ad7-463">一个`Private`实例变量类型为委托，名为`XEvent`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-463">A `Private` instance variable typed as the delegate, named `XEvent`.</span></span>

* <span data-ttu-id="52ad7-464">两个方法名为`add_X`和`remove_X`这不能调用、 重写或重载。</span><span class="sxs-lookup"><span data-stu-id="52ad7-464">Two methods named `add_X` and `remove_X` which cannot be invoked, overridden or overloaded.</span></span>

<span data-ttu-id="52ad7-465">如果类型尝试声明与上述名称之一匹配的名称，将导致编译时错误，并隐式`add_X`和`remove_X`声明将名称绑定用于忽略。</span><span class="sxs-lookup"><span data-stu-id="52ad7-465">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit `add_X` and `remove_X` declarations are ignored for the purposes of name binding.</span></span> <span data-ttu-id="52ad7-466">不能重写或重载任何引入的成员，但也可以在派生类型中隐藏它们。</span><span class="sxs-lookup"><span data-stu-id="52ad7-466">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span> <span data-ttu-id="52ad7-467">例如，在类声明</span><span class="sxs-lookup"><span data-stu-id="52ad7-467">For example, the class declaration</span></span>

```vb
Class Raiser
    Public Event Constructed(i As Integer)
End Class
```

<span data-ttu-id="52ad7-468">等效于下面的声明</span><span class="sxs-lookup"><span data-stu-id="52ad7-468">is equivalent to the following declaration</span></span>

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

<span data-ttu-id="52ad7-469">无需指定委托类型声明一个事件是最简单且最紧凑的语法，但缺点是声明新的委托类型的每个事件。</span><span class="sxs-lookup"><span data-stu-id="52ad7-469">Declaring an event without specifying a delegate type is the simplest and most compact syntax, but has the disadvantage of declaring a new delegate type for each event.</span></span> <span data-ttu-id="52ad7-470">例如，在以下示例中，三个隐藏的委托类型创建，即使所有三个事件都具有相同的参数列表：</span><span class="sxs-lookup"><span data-stu-id="52ad7-470">For example, in the following example, three hidden delegate types are created, even though all three events have the same parameter list:</span></span>

```vb
Public Class Button
    Public Event Click(sender As Object, e As EventArgs)
    Public Event DoubleClick(sender As Object, e As EventArgs)
    Public Event RightClick(sender As Object, e As EventArgs)
End Class
```

<span data-ttu-id="52ad7-471">在以下示例中，事件只需使用相同的委托， `EventHandler`:</span><span class="sxs-lookup"><span data-stu-id="52ad7-471">In the following example, the events simply use the same delegate, `EventHandler`:</span></span>

```vb
Public Delegate Sub EventHandler(sender As Object, e As EventArgs)

Public Class Button
    Public Event Click As EventHandler
    Public Event DoubleClick As EventHandler
    Public Event RightClick As EventHandler
End Class
```

<span data-ttu-id="52ad7-472">可以在以下两种方式处理事件： 静态或动态。</span><span class="sxs-lookup"><span data-stu-id="52ad7-472">Events can be handled in one of two ways: statically or dynamically.</span></span> <span data-ttu-id="52ad7-473">以静态方式处理事件是更简单，只需要`WithEvents`变量和一个`Handles`子句。</span><span class="sxs-lookup"><span data-stu-id="52ad7-473">Statically handling events is simpler and only requires a `WithEvents` variable and a `Handles` clause.</span></span> <span data-ttu-id="52ad7-474">在以下示例中，类`Form1`以静态方式处理事件`Click`对象的`Button`:</span><span class="sxs-lookup"><span data-stu-id="52ad7-474">In the following example, class `Form1` statically handles the event `Click` of object `Button`:</span></span>

```vb
Public Class Form1
    Public WithEvents Button1 As New Button()

    Public Sub Button1_Click(sender As Object, e As EventArgs) _
           Handles Button1.Click
        Console.WriteLine("Button1 was clicked!")
    End Sub
End Class
```

<span data-ttu-id="52ad7-475">动态处理事件是更复杂，因为必须显式连接和断开连接到代码中的事件。</span><span class="sxs-lookup"><span data-stu-id="52ad7-475">Dynamically handling events is more complex because the event must be explicitly connected and disconnected to in code.</span></span> <span data-ttu-id="52ad7-476">该语句`AddHandler`添加一个事件和语句的处理程序`RemoveHandler`移除事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-476">The statement `AddHandler` adds a handler for an event, and the statement `RemoveHandler` removes a handler for an event.</span></span> <span data-ttu-id="52ad7-477">下一个示例显示了一个类`Form1`，它将`Button1_Click`的事件处理程序作为`Button1`的`Click`事件：</span><span class="sxs-lookup"><span data-stu-id="52ad7-477">The next example shows a class `Form1` that adds `Button1_Click` as an event handler for `Button1`'s `Click` event:</span></span>

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

<span data-ttu-id="52ad7-478">在方法中`Disconnect`，移除事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-478">In method `Disconnect`, the event handler is removed.</span></span>


### <a name="custom-events"></a><span data-ttu-id="52ad7-479">自定义事件</span><span class="sxs-lookup"><span data-stu-id="52ad7-479">Custom Events</span></span>

<span data-ttu-id="52ad7-480">如前一部分中所述，事件声明隐式定义的字段中，`add_`方法，和一个`remove_`方法，用于跟踪的事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-480">As discussed in the previous section, event declarations implicitly define a field, an `add_` method, and a `remove_` method that are used to keep track of event handlers.</span></span> <span data-ttu-id="52ad7-481">在某些情况下，但是，它可能需要提供自定义代码的跟踪事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-481">In some situations, however, it may be desirable to provide custom code for tracking event handlers.</span></span> <span data-ttu-id="52ad7-482">例如，如果某个类定义四十事件，其中仅有少数曾经将处理过程，而不四十字段使用哈希表来跟踪每个事件处理程序可能更高效。</span><span class="sxs-lookup"><span data-stu-id="52ad7-482">For example, if a class defines forty events of which only a few will ever be handled, using a hash table instead of forty fields to track the handlers for each event may be more efficient.</span></span> <span data-ttu-id="52ad7-483">*自定义事件*允许`add_X`和`remove_X`方法来显式定义这样的事件处理程序的自定义存储。</span><span class="sxs-lookup"><span data-stu-id="52ad7-483">*Custom events* allow the `add_X` and `remove_X` methods to be defined explicitly, which enables custom storage for event handlers.</span></span>

<span data-ttu-id="52ad7-484">自定义事件声明指定委托类型的事件被声明，使用异常的方式相同的关键字`Custom`必须位于之前`Event`关键字。</span><span class="sxs-lookup"><span data-stu-id="52ad7-484">Custom events are declared in the same way that events that specify a delegate type are declared, with the exception that the keyword `Custom` must precede the `Event` keyword.</span></span> <span data-ttu-id="52ad7-485">自定义事件声明中包含的三个声明：`AddHandler`声明中，`RemoveHandler`声明和一个`RaiseEvent`声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-485">A custom event declaration contains three declarations: an `AddHandler` declaration, a `RemoveHandler` declaration and a `RaiseEvent` declaration.</span></span> <span data-ttu-id="52ad7-486">尽管它们可能具有属性，都不声明可以包含任何修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-486">None of the declarations can have any modifiers, although they can have attributes.</span></span>

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

<span data-ttu-id="52ad7-487">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-487">For example:</span></span>

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

<span data-ttu-id="52ad7-488">`AddHandler`并`RemoveHandler`声明接受一个`ByVal`参数，它必须为该事件的委托类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-488">The `AddHandler` and `RemoveHandler` declaration take one `ByVal` parameter, which must be of the delegate type of the event.</span></span> <span data-ttu-id="52ad7-489">当`AddHandler`或`RemoveHandler`执行语句 (或`Handles`子句自动处理的事件)，将调用相应的声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-489">When an `AddHandler` or `RemoveHandler` statement is executed (or a `Handles` clause automatically handles an event), the corresponding declaration will be called.</span></span> <span data-ttu-id="52ad7-490">`RaiseEvent`声明采用相同的事件委托的参数和时将调用`RaiseEvent`执行语句。</span><span class="sxs-lookup"><span data-stu-id="52ad7-490">The `RaiseEvent` declaration takes the same parameters as the event delegate and will be called when a `RaiseEvent` statement is executed.</span></span> <span data-ttu-id="52ad7-491">所有声明必须提供并被视为子例程。</span><span class="sxs-lookup"><span data-stu-id="52ad7-491">All of the declarations must be provided and are considered to be subroutines.</span></span>

<span data-ttu-id="52ad7-492">请注意， `AddHandler`，`RemoveHandler`和`RaiseEvent`声明子例程具有的行位置上具有同样的限制。</span><span class="sxs-lookup"><span data-stu-id="52ad7-492">Note that `AddHandler`, `RemoveHandler` and `RaiseEvent` declarations have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="52ad7-493">开始语句，end 语句和块必须出现在逻辑行的开头开始。</span><span class="sxs-lookup"><span data-stu-id="52ad7-493">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

<span data-ttu-id="52ad7-494">除了成员名称添加到该类型的声明空间，自定义事件声明隐式声明多个其他成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-494">In addition to the member name added to the type's declaration space, a custom event declaration implicitly declares several other members.</span></span> <span data-ttu-id="52ad7-495">给定事件名为`X`，以下成员添加到声明空间：</span><span class="sxs-lookup"><span data-stu-id="52ad7-495">Given an event named `X`, the following members are added to the declaration space:</span></span>

* <span data-ttu-id="52ad7-496">一个名为方法`add_X`，对应于`AddHandler`声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-496">A method named `add_X`, corresponding to the `AddHandler` declaration.</span></span>

* <span data-ttu-id="52ad7-497">一个名为方法`remove_X`，对应于`RemoveHandler`声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-497">A method named `remove_X`, corresponding to the `RemoveHandler` declaration.</span></span>

* <span data-ttu-id="52ad7-498">一个名为方法`fire_X`，对应于`RaiseEvent`声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-498">A method named `fire_X`, corresponding to the `RaiseEvent` declaration.</span></span>

<span data-ttu-id="52ad7-499">如果类型尝试声明与上述名称之一匹配的名称，将导致编译时错误，并隐式声明将名称绑定用于忽略。</span><span class="sxs-lookup"><span data-stu-id="52ad7-499">If a type attempts to declare a name that matches one of the above names, a compile-time error will result, and the implicit declarations are all ignored for the purposes of name binding.</span></span> <span data-ttu-id="52ad7-500">不能重写或重载任何引入的成员，但也可以在派生类型中隐藏它们。</span><span class="sxs-lookup"><span data-stu-id="52ad7-500">It is not possible to override or overload any of the introduced members, although it is possible to shadow them in derived types.</span></span>

<span data-ttu-id="52ad7-501">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-501">__Note.__</span></span> <span data-ttu-id="52ad7-502">`Custom` 不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="52ad7-502">`Custom` is not a reserved word.</span></span>

#### <a name="custom-events-in-winrt-assemblies"></a><span data-ttu-id="52ad7-503">WinRT 程序集中的自定义事件</span><span class="sxs-lookup"><span data-stu-id="52ad7-503">Custom events in WinRT assemblies</span></span>

<span data-ttu-id="52ad7-504">使用编译的文件中声明的事件从 Microsoft Visual Basic 11.0 开始`/target:winmdobj`，或在此类文件中的接口中声明和其他位置，则实现，被视为略有不同。</span><span class="sxs-lookup"><span data-stu-id="52ad7-504">As of Microsoft Visual Basic 11.0, events declared in a file compiled with `/target:winmdobj`, or declared in an interface in such a file and then implemented elsewhere, are treated a little differently.</span></span>

* <span data-ttu-id="52ad7-505">外部工具用来生成 winmd 通常将允许仅特定委托类型如`System.EventHandler(Of T)`或`System.TypedEventHandle(Of T, U)`，，将不允许其他人。</span><span class="sxs-lookup"><span data-stu-id="52ad7-505">External tools used to build the winmd will typically allow only certain delegate types such as `System.EventHandler(Of T)` or `System.TypedEventHandle(Of T, U)`, and will disallow others.</span></span>

* <span data-ttu-id="52ad7-506">`XEvent`字段都具有类型`System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)`其中`T`是委托类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-506">The `XEvent` field has type `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable(Of T)` where `T` is the delegate type.</span></span>

* <span data-ttu-id="52ad7-507">AddHandler 访问器返回`System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`，和 RemoveHandler 取值函数采用单个参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="52ad7-507">The AddHandler accessor returns a `System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken`, and the RemoveHandler accessor takes a single parameter of the same type.</span></span>

<span data-ttu-id="52ad7-508">下面是此类自定义事件的示例。</span><span class="sxs-lookup"><span data-stu-id="52ad7-508">Here is an example of such a custom event.</span></span>

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


## <a name="constants"></a><span data-ttu-id="52ad7-509">常量</span><span class="sxs-lookup"><span data-stu-id="52ad7-509">Constants</span></span>

<span data-ttu-id="52ad7-510">一个*常量*是一种类型的成员的常量值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-510">A *constant* is a constant value that is a member of a type.</span></span>

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

<span data-ttu-id="52ad7-511">隐式共享常量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-511">Constants are implicitly shared.</span></span> <span data-ttu-id="52ad7-512">如果声明包含`As`子句，子句可以指定该声明引入的成员的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-512">If the declaration contains an `As` clause, the clause specifies the type of the member introduced by the declaration.</span></span> <span data-ttu-id="52ad7-513">如果省略的类型，该常量的类型推断。</span><span class="sxs-lookup"><span data-stu-id="52ad7-513">If the type is omitted then the type of the constant is inferred.</span></span> <span data-ttu-id="52ad7-514">一个常量的类型可能只能是基元类型或`Object`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-514">The type of a constant may only be a primitive type or `Object`.</span></span> <span data-ttu-id="52ad7-515">如果一个常量的类型为`Object`又没有类型字符，该常量的实际类型将常量表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-515">If a constant is typed as `Object` and there is no type character, the real type of the constant will be the type of the constant expression.</span></span> <span data-ttu-id="52ad7-516">否则，该常量的类型是字符常量的类型的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-516">Otherwise, the type of the constant is the type of the constant's type character.</span></span>

<span data-ttu-id="52ad7-517">下面的示例演示一个名为类`Constants`具有两个公共常量：</span><span class="sxs-lookup"><span data-stu-id="52ad7-517">The following example shows a class named `Constants` that has two public constants:</span></span>

```vb
Class Constants
    Public Const A As Integer = 1
    Public Const B As Integer = A + 1
End Class
```

<span data-ttu-id="52ad7-518">可以通过类，如下面的示例，打印出的值中所示访问常量`Constants.A`和`Constants.B`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-518">Constants can be accessed through the class, as in the following example, which prints out the values of `Constants.A` and `Constants.B`.</span></span>

```vb
Module Test
    Sub Main()
        Console.WriteLine(Constants.A & ", " & Constants.B)
    End Sub 
End Module
```

<span data-ttu-id="52ad7-519">声明多个常数在常量声明是等效于单个常量的多个声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-519">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants.</span></span> <span data-ttu-id="52ad7-520">下面的示例声明一个声明语句中的三个常量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-520">The following example declares three constants in one declaration statement.</span></span>

```vb
Class A
    Protected Const x As Integer = 1, y As Long = 2, z As Short = 3
End Class
```

<span data-ttu-id="52ad7-521">此声明是等效于以下：</span><span class="sxs-lookup"><span data-stu-id="52ad7-521">This declaration is equivalent to the following:</span></span>

```vb
Class A
    Protected Const x As Integer = 1
    Protected Const y As Long = 2
    Protected Const z As Short = 3
End Class
```

<span data-ttu-id="52ad7-522">常量的类型的可访问性域必须与相同或常量本身的可访问域的超集。</span><span class="sxs-lookup"><span data-stu-id="52ad7-522">The accessibility domain of the type of the constant must be the same as or a superset of the accessibility domain of the constant itself.</span></span> <span data-ttu-id="52ad7-523">常量表达式必须产生一个值或隐式转换为常量的类型的类型的常量的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-523">The constant expression must yield a value of the constant's type or of a type that is implicitly convertible to the constant's type.</span></span> <span data-ttu-id="52ad7-524">常量表达式可能不是循环;也就是说，就本身而言可能没有定义一个常量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-524">The constant expression may not be circular; that is, a constant may not be defined in terms of itself.</span></span>

<span data-ttu-id="52ad7-525">编译器将自动计算的常量声明中的相应顺序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-525">The compiler automatically evaluates the constant declarations in the appropriate order.</span></span> <span data-ttu-id="52ad7-526">在以下示例中，编译器将首先计算`Y`，然后`Z`，最后`X`，分别生成值 10、 11 和 12。</span><span class="sxs-lookup"><span data-stu-id="52ad7-526">In the following example, the compiler first evaluates `Y`, then `Z`, and finally `X`, producing the values 10, 11, and 12, respectively.</span></span>

```vb
Class A
    Public Const X As Integer = B.Z + 1
    Public Const Y As Integer = 10
End Class

Class B
    Public Const Z As Integer = A.Y + 1
End Class
```

<span data-ttu-id="52ad7-527">需要常量值的符号名称，但在常量声明或当该值不能在编译时由计算常量表达式中不允许的值类型，可能会改为使用只读变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-527">When a symbolic name for a constant value is desired, but the type of the value is not permitted in a constant declaration or when the value cannot be computed at compile time by a constant expression, a read-only variable may be used instead.</span></span>


## <a name="instance-and-shared-variables"></a><span data-ttu-id="52ad7-528">实例和共享的变量</span><span class="sxs-lookup"><span data-stu-id="52ad7-528">Instance and Shared Variables</span></span>

<span data-ttu-id="52ad7-529">实例或共享的变量是可以存储的信息类型的成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-529">An instance or shared variable is a member of a type that can store information.</span></span>

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

<span data-ttu-id="52ad7-530">`Dim`修饰符必须指定是否没有修饰符指定了，但可能否则省略。</span><span class="sxs-lookup"><span data-stu-id="52ad7-530">The `Dim` modifier must be specified if no modifiers are specified, but may be omitted otherwise.</span></span> <span data-ttu-id="52ad7-531">单个变量声明可能包括多个变量的声明符;每个变量的声明符引入一个新实例或共享的成员。</span><span class="sxs-lookup"><span data-stu-id="52ad7-531">A single variable declaration may include multiple variable declarators; each variable declarator introduces a new instance or shared member.</span></span>

<span data-ttu-id="52ad7-532">如果指定一个初始值设定项，则变量的声明符可能声明仅一个实例或共享的变量：</span><span class="sxs-lookup"><span data-stu-id="52ad7-532">If an initializer is specified, only one instance or shared variable may be declared by the variable declarator:</span></span>

```vb
Class Test
    Dim a, b, c, d As Integer = 10  ' Invalid: multiple initialization
End Class
```

<span data-ttu-id="52ad7-533">此限制不适用于对象初始值设定项：</span><span class="sxs-lookup"><span data-stu-id="52ad7-533">This restriction does not apply to object initializers:</span></span>

```vb
Class Test
    Dim a, b, c, d As New Collection() ' OK
End Class
```

<span data-ttu-id="52ad7-534">与声明的变量`Shared`修饰符*共享的变量*。</span><span class="sxs-lookup"><span data-stu-id="52ad7-534">A variable declared with the `Shared` modifier is a *shared variable*.</span></span> <span data-ttu-id="52ad7-535">共享的变量标识恰好一个存储位置，而不考虑创建的类型的实例数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-535">A shared variable identifies exactly one storage location regardless of the number of instances of the type that are created.</span></span> <span data-ttu-id="52ad7-536">共享的变量进入存在，当程序开始执行，并将该程序终止时消失。</span><span class="sxs-lookup"><span data-stu-id="52ad7-536">A shared variable comes into existence when a program begins executing, and ceases to exist when the program terminates.</span></span>

<span data-ttu-id="52ad7-537">共享的变量仅在特定的封闭式泛型类型的实例之间共享。</span><span class="sxs-lookup"><span data-stu-id="52ad7-537">A shared variable is shared only among instances of a particular closed generic type.</span></span> <span data-ttu-id="52ad7-538">例如，该计划：</span><span class="sxs-lookup"><span data-stu-id="52ad7-538">For example, the program:</span></span>

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

<span data-ttu-id="52ad7-539">打印出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-539">Prints out:</span></span>

```
1
1
2
```

<span data-ttu-id="52ad7-540">未声明的变量`Shared`调用修饰符*实例变量*。</span><span class="sxs-lookup"><span data-stu-id="52ad7-540">A variable declared without the `Shared` modifier is called an *instance variable*.</span></span> <span data-ttu-id="52ad7-541">类的每个实例包含一个单独的类的所有实例变量副本。</span><span class="sxs-lookup"><span data-stu-id="52ad7-541">Every instance of a class contains a separate copy of all instance variables of the class.</span></span> <span data-ttu-id="52ad7-542">引用类型的一个实例变量开始时的新实例的存在类型创建，并且将不再存在时对该实例的引用和`Finalize`方法是否已执行。</span><span class="sxs-lookup"><span data-stu-id="52ad7-542">An instance variable of a reference type comes into existence when a new instance of that type is created, and ceases to exist when there are no references to that instance and the `Finalize` method has executed.</span></span> <span data-ttu-id="52ad7-543">值类型的实例变量具有其所属的变量的生存期完全相同。</span><span class="sxs-lookup"><span data-stu-id="52ad7-543">An instance variable of a value type has exactly the same lifetime as the variable to which it belongs.</span></span> <span data-ttu-id="52ad7-544">换而言之，当值类型的变量存在到或不能存在，因此执行值类型的实例变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-544">In other words, when a variable of a value type comes into existence or ceases to exist, so does the instance variable of the value type.</span></span>

<span data-ttu-id="52ad7-545">如果声明符包含`As`子句，子句可以指定该声明引入的成员的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-545">If the declarator contains an `As` clause, the clause specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="52ad7-546">如果省略的类型以及正在使用严格的语义，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-546">If the type is omitted and strict semantics are being used, a compile-time error occurs.</span></span> <span data-ttu-id="52ad7-547">否则成员的类型是隐式`Object`或类型的成员的类型字符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-547">Otherwise the type of the members is implicitly `Object` or the type of the members' type character.</span></span>

<span data-ttu-id="52ad7-548">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-548">__Note.__</span></span> <span data-ttu-id="52ad7-549">在语法中没有任何二义性： 如果一个声明符省略了一种类型，它将始终使用下面的声明符的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-549">There is no ambiguity in the syntax: if a declarator omits a type, it will always use the type of a following declarator.</span></span>

<span data-ttu-id="52ad7-550">实例或共享的变量的类型或数组元素类型的可访问域必须与相同或实例或共享的变量本身的可访问域的超集。</span><span class="sxs-lookup"><span data-stu-id="52ad7-550">The accessibility domain of an instance or shared variable's type or array element type must be the same as or a superset of the accessibility domain of the instance or shared variable itself.</span></span>

<span data-ttu-id="52ad7-551">下面的示例演示`Color`类具有名为的内部实例变量`redPart`， `greenPart`，和`bluePart`:</span><span class="sxs-lookup"><span data-stu-id="52ad7-551">The following example shows a `Color` class that has internal instance variables named `redPart`, `greenPart`, and `bluePart`:</span></span>

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


### <a name="read-only-variables"></a><span data-ttu-id="52ad7-552">只读变量</span><span class="sxs-lookup"><span data-stu-id="52ad7-552">Read-Only Variables</span></span>

<span data-ttu-id="52ad7-553">当实例或共享的变量声明中包括`ReadOnly`修饰符，该声明引入的变量赋值可能仅出现在声明或同一个类的构造函数中的过程。</span><span class="sxs-lookup"><span data-stu-id="52ad7-553">When an instance or shared variable declaration includes a `ReadOnly` modifier, assignments to the variables introduced by the declaration may only occur as part of the declaration or in a constructor in the same class.</span></span> <span data-ttu-id="52ad7-554">具体而言，仅在以下情况下允许分配到只读实例或共享的变量：</span><span class="sxs-lookup"><span data-stu-id="52ad7-554">Specifically, assignments to a read-only instance or shared variable are permitted only in the following situations:</span></span>

* <span data-ttu-id="52ad7-555">中引入了 （通过在声明中包括变量的初始值设定项） 的实例或共享的变量的变量声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-555">In the variable declaration that introduces the instance or shared variable (by including a variable initializer in the declaration).</span></span>

* <span data-ttu-id="52ad7-556">对于实例变量，包含的变量声明的类的实例构造函数中。</span><span class="sxs-lookup"><span data-stu-id="52ad7-556">For an instance variable, in the instance constructors of the class that contains the variable declaration.</span></span> <span data-ttu-id="52ad7-557">实例变量只能以非限定方式或通过访问`Me`或`MyClass`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-557">The instance variable can only be accessed in an unqualified manner or through `Me` or `MyClass`.</span></span>

* <span data-ttu-id="52ad7-558">共享包含共享的变量声明的类的构造函数中的共享变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-558">For a shared variable, in the shared constructor of the class that contains the shared variable declaration.</span></span>

<span data-ttu-id="52ad7-559">时需要常量值的符号名称，但在常量声明中，不允许的值类型时或由常数表达式，不能在编译时计算值时，共享的只读变量非常有用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-559">A shared read-only variable is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a constant declaration, or when the value cannot be computed at compile time by a constant expression.</span></span>

<span data-ttu-id="52ad7-560">示例的第一个此类应用程序如下所示，共享的变量声明中哪种颜色`ReadOnly`以防止其被其他程序正在更改：</span><span class="sxs-lookup"><span data-stu-id="52ad7-560">An example of the first such application follows, in which color shared variables are declared `ReadOnly` to prevent them from being changed by other programs:</span></span>

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

<span data-ttu-id="52ad7-561">常量和共享的只读变量具有不同的语义。</span><span class="sxs-lookup"><span data-stu-id="52ad7-561">Constants and read-only shared variables have different semantics.</span></span> <span data-ttu-id="52ad7-562">当表达式引用一个常量时，在编译时，获取常量的值，但当表达式引用只读共享的变量，要等到运行时不获取共享变量的值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-562">When an expression references a constant, the value of the constant is obtained at compile time, but when an expression references a read-only shared variable, the value of the shared variable is not obtained until run time.</span></span> <span data-ttu-id="52ad7-563">请考虑下面的应用程序，其中包括两个单独的程序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-563">Consider the following application, which consists of two separate programs.</span></span>

<span data-ttu-id="52ad7-564">File1.vb:</span><span class="sxs-lookup"><span data-stu-id="52ad7-564">file1.vb:</span></span>

```vb
Namespace Program1
    Public Class Utils
        Public Shared ReadOnly X As Integer = 1
    End Class
End Namespace
```

<span data-ttu-id="52ad7-565">File2.vb:</span><span class="sxs-lookup"><span data-stu-id="52ad7-565">file2.vb:</span></span>

```vb
Namespace Program2
    Module Test
        Sub Main()
            Console.WriteLine(Program1.Utils.X)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="52ad7-566">命名空间`Program1`和`Program2`表示两个单独编译的程序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-566">The namespaces `Program1` and `Program2` denote two programs that are compiled separately.</span></span> <span data-ttu-id="52ad7-567">因为变量`Program1.Utils.X`被声明为`Shared ReadOnly`，通过生成的值输出`Console.WriteLine`语句不在编译时已知，但而不是在运行时获取。</span><span class="sxs-lookup"><span data-stu-id="52ad7-567">Because variable `Program1.Utils.X` is declared as `Shared ReadOnly`, the value output by the `Console.WriteLine` statement is not known at compile time, but rather is obtained at run time.</span></span> <span data-ttu-id="52ad7-568">因此，如果的值`X`发生更改并`Program1`重新编译`Console.WriteLine`语句会输出新值即使`Program2`不重新编译。</span><span class="sxs-lookup"><span data-stu-id="52ad7-568">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` is not recompiled.</span></span> <span data-ttu-id="52ad7-569">但是，如果`X`曾常数的值`X`将时获取`Program2`编译，并会一直在的中的更改不会影响`Program1`直到`Program2`已重新编译。</span><span class="sxs-lookup"><span data-stu-id="52ad7-569">However, if `X` had been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would have remained unaffected by changes in `Program1` until `Program2` was recompiled.</span></span>

### <a name="withevents-variables"></a><span data-ttu-id="52ad7-570">WithEvents 变量</span><span class="sxs-lookup"><span data-stu-id="52ad7-570">WithEvents Variables</span></span>

<span data-ttu-id="52ad7-571">类型可声明它处理引发的事件由它的一个实例或共享的变量声明的实例或共享的变量引发的事件与某些组`WithEvents`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-571">A type can declare that it handles some set of events raised by one of its instance or shared variables by declaring the instance or shared variable that raises the events with the `WithEvents` modifier.</span></span> <span data-ttu-id="52ad7-572">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-572">For example:</span></span>

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

<span data-ttu-id="52ad7-573">在此示例中，该方法`E1Handler`处理事件`E1`类型的实例引发`Raiser`实例变量中存储`x`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-573">In this example, the method `E1Handler` handles the event `E1` that is raised by the instance of the type `Raiser` stored in the instance variable `x`.</span></span>

<span data-ttu-id="52ad7-574">`WithEvents`修饰符导致要用前导下划线重命名并替换具有相同名称的事件挂钩的属性的变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-574">The `WithEvents` modifier causes the variable to be renamed with a leading underscore and replaced with a property of the same name that does the event hookup.</span></span> <span data-ttu-id="52ad7-575">例如，如果变量的名称是`F`，重命名为`_F`和属性`F`隐式声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-575">For example, if the variable's name is `F`, it is renamed to `_F` and a property `F` is implicitly declared.</span></span> <span data-ttu-id="52ad7-576">如果变量的新名称与另一个声明之间的冲突，将报告一个编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-576">If there is a collision between the variable's new name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="52ad7-577">应用于该变量的任何特性将转移至已重命名的变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-577">Any attributes applied to the variable are carried over to the renamed variable.</span></span>

<span data-ttu-id="52ad7-578">通过创建的隐式属性`WithEvents`声明负责挂接和方法取消的相关事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="52ad7-578">The implicit property created by a `WithEvents` declaration takes care of hooking and unhooking the relevant event handlers.</span></span> <span data-ttu-id="52ad7-579">该属性值分配给变量时，首先调用`remove`变量 （如果有，方法取消现有的事件处理程序） 中的当前实例上的事件的方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-579">When a value is assigned to the variable, the property first calls the `remove` method for the event on the instance currently in the variable (unhooking the existing event handler, if any).</span></span> <span data-ttu-id="52ad7-580">接下来进行分配，并且该属性会调用`add`（挂接新的事件处理程序） 的变量中的新实例上的事件的方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-580">Next the assignment is made, and the property calls the `add` method for the event on the new instance in the variable (hooking up the new event handler).</span></span> <span data-ttu-id="52ad7-581">下面的代码是等效于标准模块上面的代码`Test`:</span><span class="sxs-lookup"><span data-stu-id="52ad7-581">The following code is equivalent to the code above for the standard module `Test`:</span></span>

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

<span data-ttu-id="52ad7-582">不能用来声明实例或共享的变量作为`WithEvents`如果该变量的类型为结构。</span><span class="sxs-lookup"><span data-stu-id="52ad7-582">It is not valid to declare an instance or shared variable as `WithEvents` if the variable is typed as a structure.</span></span> <span data-ttu-id="52ad7-583">此外，`WithEvents`不能指定在结构中，并`WithEvents`和`ReadOnly`不能结合使用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-583">In addition, `WithEvents` may not be specified in a structure, and `WithEvents` and `ReadOnly` cannot be combined.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="52ad7-584">变量初始值设定项</span><span class="sxs-lookup"><span data-stu-id="52ad7-584">Variable Initializers</span></span>

<span data-ttu-id="52ad7-585">实例和共享变量声明中类和实例变量声明 （而非共享变量的声明） 结构中可能包含变量的初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="52ad7-585">Instance and shared variable declarations in classes and instance variable declarations (but not shared variable declarations) in structures may include variable initializers.</span></span> <span data-ttu-id="52ad7-586">有关`Shared`变量，变量初始值设定项对应于后程序开始，之前执行的赋值语句`Shared`第一次引用变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-586">For `Shared` variables, variable initializers correspond to assignment statements that are executed after the program begins, but before the `Shared` variable is first referenced.</span></span> <span data-ttu-id="52ad7-587">对于实例变量，变量初始值设定项对应于在创建类的实例时执行的赋值语句。</span><span class="sxs-lookup"><span data-stu-id="52ad7-587">For instance variables, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span> <span data-ttu-id="52ad7-588">结构不能有实例变量的初始值设定项，因为不能修改其无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-588">Structures cannot have instance variable initializers because their parameterless constructors cannot be modified.</span></span>

<span data-ttu-id="52ad7-589">请看下面的示例：</span><span class="sxs-lookup"><span data-stu-id="52ad7-589">Consider the following example:</span></span>

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

<span data-ttu-id="52ad7-590">此示例产生以下输出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-590">The example produces the following output:</span></span>

```
x = 1.4142135623731, i = 100, s = Hello
```

<span data-ttu-id="52ad7-591">向赋值`x`加载类时，会发生和分配到`i`和`s`发生时创建类的新实例。</span><span class="sxs-lookup"><span data-stu-id="52ad7-591">An assignment to `x` occurs when the class is loaded, and assignments to `i` and `s` occur when a new instance of the class is created.</span></span>

<span data-ttu-id="52ad7-592">它可用于将自动插入的块类型的构造函数中的赋值语句视为变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="52ad7-592">It is useful to think of variable initializers as assignment statements that are automatically inserted in the block of the type's constructor.</span></span> <span data-ttu-id="52ad7-593">下面的示例包含多个实例变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="52ad7-593">The following example contains several instance variable initializers.</span></span>

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

<span data-ttu-id="52ad7-594">此示例对应于代码如下所示，每个注释指示的自动插入的语句的位置。</span><span class="sxs-lookup"><span data-stu-id="52ad7-594">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement.</span></span>

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

<span data-ttu-id="52ad7-595">执行任何变量的初始值设定项之前，所有变量都初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-595">All variables are initialized to the default value of their type before any variable initializers are executed.</span></span> <span data-ttu-id="52ad7-596">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-596">For example:</span></span>

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

<span data-ttu-id="52ad7-597">因为`b`类加载时自动初始化为其默认值和`i`将自动初始化为其默认值创建类的实例时，前面的代码产生以下输出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-597">Because `b` is automatically initialized to its default value when the class is loaded and `i` is automatically initialized to its default value when an instance of the class is created, the preceding code produces the following output:</span></span>

```
b = False, i = 0
```

<span data-ttu-id="52ad7-598">每个变量的初始值设定项必须生成值或隐式转换为变量的类型的类型的变量的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-598">Each variable initializer must yield a value of the variable's type or of a type that is implicitly convertible to the variable's type.</span></span> <span data-ttu-id="52ad7-599">变量的初始值设定项可能循环或将在其后，在其中被引用变量的情况下该值是其默认值的初始值设定项用于初始化的变量，请参阅。</span><span class="sxs-lookup"><span data-stu-id="52ad7-599">A variable initializer may be circular or refer to a variable that will be initialized after it, in which case the value of the referenced variable is its default value for the purposes of the initializer.</span></span> <span data-ttu-id="52ad7-600">此类初始值设定项是值的可疑。</span><span class="sxs-lookup"><span data-stu-id="52ad7-600">Such an initializer is of dubious value.</span></span>

<span data-ttu-id="52ad7-601">有三种形式的变量的初始值设定项： 正则初始值设定项，数组大小初始值设定项和对象初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="52ad7-601">There are three forms of variable initializers: regular initializers, array-size initializers, and object initializers.</span></span> <span data-ttu-id="52ad7-602">前两个窗体出现在等号后面的类型名称之后后, 两个本身声明的一部分。</span><span class="sxs-lookup"><span data-stu-id="52ad7-602">The first two forms appear after an equal sign that follows the type name, the latter two are part of the declaration itself.</span></span> <span data-ttu-id="52ad7-603">可能在任何特定声明使用初始值设定项只有一个窗体。</span><span class="sxs-lookup"><span data-stu-id="52ad7-603">Only one form of initializer may be used on any particular declaration.</span></span>

#### <a name="regular-initializers"></a><span data-ttu-id="52ad7-604">正则初始值设定项</span><span class="sxs-lookup"><span data-stu-id="52ad7-604">Regular Initializers</span></span>

<span data-ttu-id="52ad7-605">一个*正则初始值设定项*是隐式转换为变量的类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="52ad7-605">A *regular initializer* is an expression that is implicitly convertible to the type of the variable.</span></span> <span data-ttu-id="52ad7-606">它出现在等号后面的类型名称，必须分类为一个值之后。</span><span class="sxs-lookup"><span data-stu-id="52ad7-606">It appears after an equal sign that follows the type name and must be classified as a value.</span></span> <span data-ttu-id="52ad7-607">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-607">For example:</span></span>

```vb
Module Test
    Dim x As Integer = 10
    Dim y As Integer = 20

    Sub Main()
        Console.WriteLine("x = " & x & ", y = " & y)
    End Sub
End Module
```

<span data-ttu-id="52ad7-608">此程序将生成输出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-608">This program produces the output:</span></span>

```vb
x = 10, y = 20
```

<span data-ttu-id="52ad7-609">如果变量声明具有正则初始值设定项，则可以一次声明一个变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-609">If a variable declaration has a regular initializer, then only a single variable can be declared at a time.</span></span> <span data-ttu-id="52ad7-610">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-610">For example:</span></span>

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

#### <a name="object-initializers"></a><span data-ttu-id="52ad7-611">对象初始值设定项</span><span class="sxs-lookup"><span data-stu-id="52ad7-611">Object Initializers</span></span>

<span data-ttu-id="52ad7-612">*对象初始值设定项*使用到位置的类型名称的对象创建表达式指定。</span><span class="sxs-lookup"><span data-stu-id="52ad7-612">An *object initializer* is specified using an object creation expression in the place of the type name.</span></span> <span data-ttu-id="52ad7-613">对象初始值设定项是等效于的对象创建表达式结果赋给变量的正则初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="52ad7-613">An object initializer is equivalent to a regular initializer assigning the result of the object creation expression to the variable.</span></span> <span data-ttu-id="52ad7-614">So</span><span class="sxs-lookup"><span data-stu-id="52ad7-614">So</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As New Test(10)
    End Sub
End Module
```

<span data-ttu-id="52ad7-615">等效于</span><span class="sxs-lookup"><span data-stu-id="52ad7-615">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test(10)
    End Sub
End Module
```

<span data-ttu-id="52ad7-616">在对象初始值设定项中的括号始终解释为构造函数的参数列表和永远不会作为数组的类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-616">The parenthesis in an object initializer is always interpreted as the argument list for the constructor and never as array type modifiers.</span></span> <span data-ttu-id="52ad7-617">使用对象初始值设定项的变量名称不能具有数组类型修饰符或为 null 的类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-617">A variable name with an object initializer cannot have an array type modifier or a nullable type modifier.</span></span>

#### <a name="array-size-initializers"></a><span data-ttu-id="52ad7-618">数组大小初始值设定项</span><span class="sxs-lookup"><span data-stu-id="52ad7-618">Array-Size Initializers</span></span>

<span data-ttu-id="52ad7-619">*数组大小初始值设定项*是为提供的维度设置上限，由表达式表示一组变量的名称上的修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-619">An *array-size initializer* is a modifier on the name of the variable that gives a set of dimension upper bounds denoted by expressions.</span></span>

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

<span data-ttu-id="52ad7-620">上限表达式必须分类为值和必须可隐式转换为`Integer`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-620">The upper bound expressions must be classified as values and must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="52ad7-621">设置上限，组相当于给定的上限的数组创建表达式的变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="52ad7-621">The set of upper bounds is equivalent to a variable initializer of an array-creation expression with the given upper bounds.</span></span> <span data-ttu-id="52ad7-622">从数组大小初始值设定项推断数组类型的维度数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-622">The number of dimensions of the array type is inferred from the array size initializer.</span></span> <span data-ttu-id="52ad7-623">So</span><span class="sxs-lookup"><span data-stu-id="52ad7-623">So</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5, 10) As Integer
    End Sub
End Module
```

<span data-ttu-id="52ad7-624">等效于</span><span class="sxs-lookup"><span data-stu-id="52ad7-624">is equivalent to</span></span>

```vb
Module Test
    Sub Main()
        Dim x As Integer(,) = New Integer(5, 10) {}
    End Sub
End Module
```

<span data-ttu-id="52ad7-625">所有的上限必须等于或大于-1，并且所有维度都必须指定上限。</span><span class="sxs-lookup"><span data-stu-id="52ad7-625">All upper bounds must be equal to or greater than -1, and all dimensions must have an upper bound specified.</span></span> <span data-ttu-id="52ad7-626">如果正在初始化数组的元素类型本身是数组类型，数组类型修饰符转到的数组大小初始值设定项的右侧。</span><span class="sxs-lookup"><span data-stu-id="52ad7-626">If the element type of the array being initialized is itself an array type, the array-type modifiers go to the right of the array-size initializer.</span></span> <span data-ttu-id="52ad7-627">例如</span><span class="sxs-lookup"><span data-stu-id="52ad7-627">For example</span></span>

```vb
Module Test
    Sub Main()
        Dim x(5,10)(,,) As Integer
    End Sub
End Module
```

<span data-ttu-id="52ad7-628">声明本地变量`x`其类型是一个三维数组的元素的二维数组`Integer`，将初始化为一个包含边界的数组`0..5`中的第一个维度和`0..10`第二个维度中。</span><span class="sxs-lookup"><span data-stu-id="52ad7-628">declares a local variable `x` whose type is a two-dimensional array of three-dimensional arrays of `Integer`, initialized to an array with bounds of `0..5` in the first dimension and `0..10` in the second dimension.</span></span> <span data-ttu-id="52ad7-629">不能使用数组大小初始值设定项初始化类型为数组的数组变量中的元素。</span><span class="sxs-lookup"><span data-stu-id="52ad7-629">It is not possible to use an array size initializer to initialize the elements of a variable whose type is an array of arrays.</span></span>

<span data-ttu-id="52ad7-630">具有数组大小初始值设定项的变量声明不能具有其类型或正则初始值设定项的数组类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-630">A variable declaration with an array-size initializer cannot have an array type modifier on its type or a regular initializer.</span></span>


### <a name="systemmarshalbyrefobject-classes"></a><span data-ttu-id="52ad7-631">System.MarshalByRefObject 类</span><span class="sxs-lookup"><span data-stu-id="52ad7-631">System.MarshalByRefObject Classes</span></span>

<span data-ttu-id="52ad7-632">从类派生的类`System.MarshalByRefObject`跨使用代理服务器的上下文边界封送 (即，通过引用) 而不是通过复制 (即，按值)。</span><span class="sxs-lookup"><span data-stu-id="52ad7-632">Classes that derive from the class `System.MarshalByRefObject` are marshaled across context boundaries using proxies (that is, by reference) rather than through copying (that is, by value).</span></span> <span data-ttu-id="52ad7-633">这意味着类的实例可能不是 true 的实例，但只能是存根封送变量访问和方法调用跨上下文边界。</span><span class="sxs-lookup"><span data-stu-id="52ad7-633">This means that an instance of such a class may not be a true instance but instead may just be a stub that marshals variable accesses and method calls across a context boundary.</span></span>

<span data-ttu-id="52ad7-634">因此，不能创建对此类类上定义的变量的存储位置的引用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-634">As a result, it is not possible to create a reference to the storage location of variables defined on such classes.</span></span> <span data-ttu-id="52ad7-635">这意味着，变量类型，如类派生自`System.MarshalByRefObject`不能传递引用参数和方法和变量的类型为值类型不可以进行访问的变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-635">This means that variables typed as classes derived from `System.MarshalByRefObject` cannot be passed to reference parameters, and methods and variables of variables typed as value types may not be accessed.</span></span> <span data-ttu-id="52ad7-636">相反，Visual Basic 将就好像属性 （因为限制是在属性上相同），这样的类上定义的变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-636">Instead, Visual Basic treats variables defined on such classes as if they were properties (since the restrictions are the same on properties).</span></span>

<span data-ttu-id="52ad7-637">此规则的例外： 成员隐式或显式使用限定`Me`已免除上述限制，因为`Me`始终保证是一个实际对象，不是一个代理。</span><span class="sxs-lookup"><span data-stu-id="52ad7-637">There is one exception to this rule: a member implicitly or explicitly qualified with `Me` is exempt from the above restrictions, because `Me` is always guaranteed to be an actual object, not a proxy.</span></span>

## <a name="properties"></a><span data-ttu-id="52ad7-638">属性</span><span class="sxs-lookup"><span data-stu-id="52ad7-638">Properties</span></span>

<span data-ttu-id="52ad7-639">*属性*是变量的自然扩展; 两者都命名成员与相关的类型，而且用于访问变量和属性的语法是一样的。</span><span class="sxs-lookup"><span data-stu-id="52ad7-639">*Properties* are a natural extension of variables; both are named members with associated types, and the syntax for accessing variables and properties is the same.</span></span> <span data-ttu-id="52ad7-640">与变量不同，但是，属性不表示存储位置。</span><span class="sxs-lookup"><span data-stu-id="52ad7-640">Unlike variables, however, properties do not denote storage locations.</span></span> <span data-ttu-id="52ad7-641">相反，属性有*访问器*，它指定要读取或写入它们的值执行的语句。</span><span class="sxs-lookup"><span data-stu-id="52ad7-641">Instead, properties have *accessors*, which specify the statements to execute in order to read or write their values.</span></span>

<span data-ttu-id="52ad7-642">与属性声明定义属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-642">Properties are defined with property declarations.</span></span> <span data-ttu-id="52ad7-643">属性声明的第一部分类似于字段声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-643">The first part of a property declaration resembles a field declaration.</span></span> <span data-ttu-id="52ad7-644">第二部分包括`Get`访问器和/或`Set`访问器。</span><span class="sxs-lookup"><span data-stu-id="52ad7-644">The second part includes a `Get` accessor and/or a `Set` accessor.</span></span>

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

<span data-ttu-id="52ad7-645">在以下示例中，`Button`类定义`Caption`属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-645">In the example below, the `Button` class defines a `Caption` property.</span></span>

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

<span data-ttu-id="52ad7-646">基于`Button`上面的类，以下是使用的示例`Caption`属性：</span><span class="sxs-lookup"><span data-stu-id="52ad7-646">Based on the `Button` class above, the following is an example of use of the `Caption` property:</span></span>

```vb
Dim okButton As Button = New Button()

okButton.Caption = "OK" ' Invokes Set accessor.
Dim s As String = okButton.Caption ' Invokes Get accessor.
```

<span data-ttu-id="52ad7-647">在这里，`Set`通过将值分配给属性，调用访问器和`Get`通过引用在表达式中的属性来调用访问器。</span><span class="sxs-lookup"><span data-stu-id="52ad7-647">Here, the `Set` accessor is invoked by assigning a value to the property, and the `Get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="52ad7-648">如果未指定类型的属性，并且正在使用严格的语义，将发生编译时错误;否则该属性的类型是隐式`Object`或类型的属性的类型字符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-648">If no type is specified for a property and strict semantics are being used, a compile-time error occurs; otherwise the type of the property is implicitly `Object` or the type of the property's type character.</span></span> <span data-ttu-id="52ad7-649">属性声明可以包含以下任一`Get`访问器，它检索的属性值，`Set`访问器，用于存储和 / 或属性的值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-649">A property declaration may contain either a `Get` accessor, which retrieves the value of the property, a `Set` accessor, which stores the value of the property, or both.</span></span> <span data-ttu-id="52ad7-650">属性隐式声明的方法，因为可能会使用一种方法与相同的修饰符声明属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-650">Because a property implicitly declares methods, a property may be declared with the same modifiers as a method.</span></span> <span data-ttu-id="52ad7-651">如果属性是在接口中定义或定义与`MustOverride`修饰符、 属性体和`End`必须省略构造; 否则，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-651">If the property is defined in an interface or defined with the `MustOverride` modifier, the property body and the `End` construct must be omitted; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="52ad7-652">索引参数列表组成的属性的签名，以便在索引参数但不是在属性的类型可重载属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-652">The index parameter list makes up the signature of the property, so properties may be overloaded on index parameters but not on the type of the property.</span></span> <span data-ttu-id="52ad7-653">索引参数列表与正则方法相同。</span><span class="sxs-lookup"><span data-stu-id="52ad7-653">The index parameter list is the same as for a regular method.</span></span> <span data-ttu-id="52ad7-654">不过，可以使用修改无参数`ByRef`修饰符和其中任何一个名为`Value`(这中的隐式值参数为保留`Set`访问器)。</span><span class="sxs-lookup"><span data-stu-id="52ad7-654">However, none of the parameters may be modified with the `ByRef` modifier and none of them may be named `Value` (which is reserved for the implicit value parameter in the `Set` accessor).</span></span>

<span data-ttu-id="52ad7-655">可能按如下所示声明一个属性：</span><span class="sxs-lookup"><span data-stu-id="52ad7-655">A property may be declared as follows:</span></span>

* <span data-ttu-id="52ad7-656">如果该属性不指定任何属性的类型修饰符，属性必须同时具有`Get`访问器和一个`Set`访问器。</span><span class="sxs-lookup"><span data-stu-id="52ad7-656">If the property specifies no property type modifier, the property must have both a `Get` accessor and a `Set` accessor.</span></span> <span data-ttu-id="52ad7-657">该属性称为为读写属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-657">The property is said to be a read-write property.</span></span>

* <span data-ttu-id="52ad7-658">如果该属性指定`ReadOnly`修饰符，该属性必须具有`Get`访问器并且可能不`Set`访问器。</span><span class="sxs-lookup"><span data-stu-id="52ad7-658">If the property specifies the `ReadOnly` modifier, the property must have a `Get` accessor and may not have a `Set` accessor.</span></span> <span data-ttu-id="52ad7-659">该属性称为为只读属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-659">The property is said to be read-only property.</span></span> <span data-ttu-id="52ad7-660">它是只读的属性赋值的目标的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-660">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>

* <span data-ttu-id="52ad7-661">如果该属性指定`WriteOnly`修饰符，该属性必须具有`Set`访问器并且可能不`Get`访问器。</span><span class="sxs-lookup"><span data-stu-id="52ad7-661">If the property specifies the `WriteOnly` modifier, the property must have a `Set` accessor and may not have a `Get` accessor.</span></span> <span data-ttu-id="52ad7-662">该属性称为为只写属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-662">The property is said to be write-only property.</span></span> <span data-ttu-id="52ad7-663">它是以引用作为赋值目标或作为一种方法的参数除外表达式中只写属性的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-663">It is a compile-time error to reference a write-only property in an expression except as the target of an assignment or as an argument to a method.</span></span>

<span data-ttu-id="52ad7-664">`Get`和`Set`属性访问器不是不同的成员，并且不能单独声明属性访问器。</span><span class="sxs-lookup"><span data-stu-id="52ad7-664">The `Get` and `Set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="52ad7-665">下面的示例不声明一个读写属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-665">The following example does not declare a single read-write property.</span></span> <span data-ttu-id="52ad7-666">相反，它声明了两个具有相同的名称，其中一个只读属性，另一个只写：</span><span class="sxs-lookup"><span data-stu-id="52ad7-666">Rather, it declares two properties with the same name, one read-only and one write-only:</span></span>

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

<span data-ttu-id="52ad7-667">由于在同一类中声明的两个成员不能具有相同的名称，则示例将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-667">Since two members declared in the same class cannot have the same name, the example causes a compile-time error.</span></span>

<span data-ttu-id="52ad7-668">默认情况下，一个属性的可访问性`Get`和`Set`访问器是与该属性本身的可访问性相同。</span><span class="sxs-lookup"><span data-stu-id="52ad7-668">By default, the accessibility of a property's `Get` and `Set` accessors is the same as the accessibility of the property itself.</span></span> <span data-ttu-id="52ad7-669">但是，`Get`和`Set`访问器还可以指定独立于属性的可访问性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-669">However, the `Get` and `Set` accessors can also specify accessibility separately from the property.</span></span> <span data-ttu-id="52ad7-670">在这种情况下，取值函数的可访问性必须是属性的可访问性比限制性更强，并且只有一个访问器可以具有不同的可访问性级别从属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-670">In that case, the accessibility of an accessor must be more restrictive than the accessibility of the property and only one accessor can have a different accessibility level from the property.</span></span> <span data-ttu-id="52ad7-671">访问类型会被视为增加或减少限制，如下所示：</span><span class="sxs-lookup"><span data-stu-id="52ad7-671">Access types are considered more or less restrictive as follows:</span></span>

* <span data-ttu-id="52ad7-672">`Private` 没有更严格`Public`， `Protected Friend`， `Protected`，或`Friend`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-672">`Private` is more restrictive than `Public`, `Protected Friend`, `Protected`, or `Friend`.</span></span>

* <span data-ttu-id="52ad7-673">`Friend` 没有更严格`Protected Friend`或`Public`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-673">`Friend` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="52ad7-674">`Protected` 没有更严格`Protected Friend`或`Public`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-674">`Protected` is more restrictive than `Protected Friend` or `Public`.</span></span>

* <span data-ttu-id="52ad7-675">`Protected Friend` 没有更严格`Public`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-675">`Protected Friend` is more restrictive than `Public`.</span></span>

<span data-ttu-id="52ad7-676">如果属性的访问器之一是可访问，而另一个却没有，则会将属性处理就像它是只读或只写。</span><span class="sxs-lookup"><span data-stu-id="52ad7-676">When one of a property's accessors is accessible but the other one is not, the property is treated as if it was read-only or write-only.</span></span> <span data-ttu-id="52ad7-677">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-677">For example:</span></span>

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

<span data-ttu-id="52ad7-678">当派生的类型隐藏属性时，派生的属性隐藏与读取和写入相关的隐藏的属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-678">When a derived type shadows a property, the derived property hides the shadowed property with respect to both reading and writing.</span></span> <span data-ttu-id="52ad7-679">在以下示例中，`P`中的属性`B`隐藏`P`中的属性`A`同时读取和写入方面：</span><span class="sxs-lookup"><span data-stu-id="52ad7-679">In the following example, the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing:</span></span>

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

<span data-ttu-id="52ad7-680">返回类型或参数类型的可访问域必须与相同或该属性本身的可访问域的超集。</span><span class="sxs-lookup"><span data-stu-id="52ad7-680">The accessibility domain of the return type or parameter types must be the same as or a superset of the accessibility domain of the property itself.</span></span> <span data-ttu-id="52ad7-681">属性只能有一个`Set`访问器，另一个`Get`访问器。</span><span class="sxs-lookup"><span data-stu-id="52ad7-681">A property may only have one `Set` accessor and one `Get` accessor.</span></span>

<span data-ttu-id="52ad7-682">声明和调用语法之间的差异除外`Overridable`， `NotOverridable`， `Overrides`， `MustOverride`，并`MustInherit`属性的行为完全相同`Overridable`， `NotOverridable`， `Overrides`， `MustOverride`，和`MustInherit`方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-682">Except for differences in declaration and invocation syntax, `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` properties behave exactly like `Overridable`, `NotOverridable`, `Overrides`, `MustOverride`, and `MustInherit` methods.</span></span> <span data-ttu-id="52ad7-683">属性重写时，重写属性必须是相同类型 （可读写、 只读、 只写）。</span><span class="sxs-lookup"><span data-stu-id="52ad7-683">When a property is overridden, the overriding property must be of the same type (read-write, read-only, write-only).</span></span> <span data-ttu-id="52ad7-684">`Overridable`属性不能包含`Private`访问器。</span><span class="sxs-lookup"><span data-stu-id="52ad7-684">An `Overridable` property cannot contain a `Private` accessor.</span></span>

<span data-ttu-id="52ad7-685">在下面的示例`X`是`Overridable`只读属性`Y`是`Overridable`读-写属性，并且`Z`是`MustOverride`读-写属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-685">In the following example `X` is an `Overridable` read-only property, `Y` is an `Overridable` read-write property, and `Z` is a `MustOverride` read-write property.</span></span>

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

<span data-ttu-id="52ad7-686">因为`Z`是`MustOverride`，则包含类`A`必须声明`MustInherit`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-686">Because `Z` is `MustOverride`, the containing class `A` must be declared `MustInherit`.</span></span>

<span data-ttu-id="52ad7-687">与此相反，一个类派生自类`A`如下所示：</span><span class="sxs-lookup"><span data-stu-id="52ad7-687">By contrast, a class that derives from class `A` is shown below:</span></span>

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

<span data-ttu-id="52ad7-688">此处，属性的声明`X`，`Y`，和`Z`重写基属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-688">Here, the declarations of properties `X`,`Y`, and `Z` override the base properties.</span></span> <span data-ttu-id="52ad7-689">每个属性声明完全匹配可访问性修饰符、 类型和相应的继承属性的名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-689">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="52ad7-690">`Get`属性访问器`X`并`Set`属性访问器`Y`使用`MyBase`关键字访问继承的属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-690">The `Get` accessor of property `X` and the `Set` accessor of property `Y` use the `MyBase` keyword to access the inherited properties.</span></span> <span data-ttu-id="52ad7-691">属性的声明`Z`重写`MustOverride`属性--因此，有没有未完成`MustOverride`类中的成员`B`，和`B`允许为常规类。</span><span class="sxs-lookup"><span data-stu-id="52ad7-691">The declaration of property `Z` overrides the `MustOverride` property -- thus, there are no outstanding `MustOverride` members in class `B`, and `B` is permitted to be a regular class.</span></span>

<span data-ttu-id="52ad7-692">属性可用于资源直到首次引用的时的初始化延迟。</span><span class="sxs-lookup"><span data-stu-id="52ad7-692">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="52ad7-693">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-693">For example:</span></span>

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

<span data-ttu-id="52ad7-694">`ConsoleStreams`类包含三个属性`In`， `Out`，和`Error`，分别表示标准输入、 输出和错误设备。</span><span class="sxs-lookup"><span data-stu-id="52ad7-694">The `ConsoleStreams` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="52ad7-695">通过公开为属性，这些成员`ConsoleStreams`类可以延迟其初始化，直到它们被实际使用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-695">By exposing these members as properties, the `ConsoleStreams` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="52ad7-696">例如，在第一次引用时`Out`属性，如下所示`ConsoleStreams.Out.WriteLine("hello, world")`，基础`TextWriter`初始化输出设备。</span><span class="sxs-lookup"><span data-stu-id="52ad7-696">For example, upon first referencing the `Out` property, as in `ConsoleStreams.Out.WriteLine("hello, world")`, the underlying `TextWriter` for the output device is initialized.</span></span> <span data-ttu-id="52ad7-697">但如果应用程序不引用`In`和`Error`为这些设备创建属性，则任何对象。</span><span class="sxs-lookup"><span data-stu-id="52ad7-697">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>


### <a name="get-accessor-declarations"></a><span data-ttu-id="52ad7-698">Get 访问器声明</span><span class="sxs-lookup"><span data-stu-id="52ad7-698">Get Accessor Declarations</span></span>

<span data-ttu-id="52ad7-699">一个`Get`通过使用属性声明访问器 (getter)`Get`声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-699">A `Get` accessor (getter) is declared by using a property `Get` declaration.</span></span> <span data-ttu-id="52ad7-700">属性`Get`关键字声明包含`Get`后, 跟一个语句块。</span><span class="sxs-lookup"><span data-stu-id="52ad7-700">A property `Get` declaration consists of the keyword `Get` followed by a statement block.</span></span> <span data-ttu-id="52ad7-701">给定一个名为属性`P`、 一个`Get`访问器声明隐式声明具有名称的方法`get_P`使用相同的修饰符、 类型和参数列表的属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-701">Given a property named `P`, a `Get` accessor declaration implicitly declares a method with the name `get_P` with the same modifiers, type, and parameter list as the property.</span></span> <span data-ttu-id="52ad7-702">如果该类型包含具有该名称的声明，会导致编译时错误，但名称绑定用于忽略隐式声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-702">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="52ad7-703">特殊的本地变量，隐式声明在`Get`具有相同名称为属性访问器正文声明空间表示属性的返回值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-703">A special local variable, which is implicitly declared in the `Get` accessor body's declaration space with the same name as the property, represents the return value of the property.</span></span> <span data-ttu-id="52ad7-704">本地变量具有特殊名称解析语义时在表达式中使用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-704">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="52ad7-705">如果应归类为方法组，如调用表达式，为表达式的上下文中使用本地变量的函数，而不是本地变量将解析名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-705">If the local variable is used in a context that expects an expression that is classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="52ad7-706">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-706">For example:</span></span>

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

<span data-ttu-id="52ad7-707">使用括号可能会导致不明确的情况下 (如`F(1)`其中`F`是其类型是一个一维数组属性)。</span><span class="sxs-lookup"><span data-stu-id="52ad7-707">The use of parentheses can cause ambiguous situations (such as `F(1)` where `F` is a property whose type is a one-dimensional array).</span></span> <span data-ttu-id="52ad7-708">在所有不明确的情况下，名称将解析为该属性而不是本地变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-708">In all ambiguous situations, the name resolves to the property rather than the local variable.</span></span> <span data-ttu-id="52ad7-709">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-709">For example:</span></span>

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

<span data-ttu-id="52ad7-710">当控制流叶`Get`访问器正文本地变量的值传递回调用表达式。</span><span class="sxs-lookup"><span data-stu-id="52ad7-710">When control flow leaves the `Get` accessor body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="52ad7-711">因为调用了`Get`访问器，从概念上讲相当于读取变量的值，它被认为是好的编程风格的`Get`访问器具有明显的副作用，如下面的示例中所示：</span><span class="sxs-lookup"><span data-stu-id="52ad7-711">Because invoking a `Get` accessor is conceptually equivalent to reading the value of a variable, it is considered bad programming style for `Get` accessors to have observable side effects, as illustrated in the following example:</span></span>

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

<span data-ttu-id="52ad7-712">值`NextValue`属性依赖于以前访问的属性的次数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-712">The value of the `NextValue` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="52ad7-713">因此，访问该属性会产生明显的副作用，并且此属性应改为作为一种方法实现。</span><span class="sxs-lookup"><span data-stu-id="52ad7-713">Thus, accessing the property produces an observable side effect, and the property should instead be implemented as a method.</span></span>

<span data-ttu-id="52ad7-714">为"无副作用"约定`Get`访问器并不意味着`Get`访问器始终应编写为只返回存储在变量中的值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-714">The "no side effects" convention for `Get` accessors does not mean that `Get` accessors should always be written to simply return values stored in variables.</span></span> <span data-ttu-id="52ad7-715">实际上，`Get`访问器通常通过访问多个变量，或调用方法计算属性的值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-715">Indeed, `Get` accessors often compute the value of a property by accessing multiple variables or invoking methods.</span></span> <span data-ttu-id="52ad7-716">但是，在正确设计`Get`访问器执行的任何操作都导致中对象的状态的可观察的更改。</span><span class="sxs-lookup"><span data-stu-id="52ad7-716">However, a properly designed `Get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="52ad7-717">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-717">__Note.__</span></span> <span data-ttu-id="52ad7-718">`Get` 访问器具有行位置子例程具有同样的限制。</span><span class="sxs-lookup"><span data-stu-id="52ad7-718">`Get` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="52ad7-719">开始语句，end 语句和块必须出现在逻辑行的开头开始。</span><span class="sxs-lookup"><span data-stu-id="52ad7-719">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertyGetDeclaration
    : Attributes? AccessModifier? 'Get' LineTerminator
      Block?
      'End' 'Get' StatementTerminator
    ;
```

### <a name="set-accessor-declarations"></a><span data-ttu-id="52ad7-720">设置访问器声明</span><span class="sxs-lookup"><span data-stu-id="52ad7-720">Set Accessor Declarations</span></span>

<span data-ttu-id="52ad7-721">一个`Set`使用属性组声明来声明访问器 (setter)。</span><span class="sxs-lookup"><span data-stu-id="52ad7-721">A `Set` accessor (setter) is declared by using a property set declaration.</span></span> <span data-ttu-id="52ad7-722">属性组声明包含关键字`Set`，可选的参数列表和语句块。</span><span class="sxs-lookup"><span data-stu-id="52ad7-722">A property set declaration consists of the keyword `Set`, an optional parameter list, and a statement block.</span></span> <span data-ttu-id="52ad7-723">给定一个名为属性`P`，setter 声明隐式声明具有名称的方法`set_P`使用相同的修饰符和作为属性的参数列表。</span><span class="sxs-lookup"><span data-stu-id="52ad7-723">Given a property named `P`, a setter declaration implicitly declares a method with the name `set_P` with the same modifiers and parameter list as the property.</span></span> <span data-ttu-id="52ad7-724">如果该类型包含具有该名称的声明，会导致编译时错误，但名称绑定用于忽略隐式声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-724">If the type contains a declaration with that name, a compile-time error results, but the implicit declaration is ignored for the purposes of name binding.</span></span>

<span data-ttu-id="52ad7-725">如果指定的参数列表，则它必须具有一个成员，该成员必须具有以外的任何修饰符`ByVal`，并且其类型必须为该属性的类型相同。</span><span class="sxs-lookup"><span data-stu-id="52ad7-725">If a parameter list is specified, it must have one member, that member must have no modifiers except `ByVal`, and its type must be the same as the type of the property.</span></span> <span data-ttu-id="52ad7-726">该参数表示要设置的属性值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-726">The parameter represents the property value being set.</span></span> <span data-ttu-id="52ad7-727">如果省略此参数，参数名为`Value`隐式声明。</span><span class="sxs-lookup"><span data-stu-id="52ad7-727">If the parameter is omitted, a parameter named `Value` is implicitly declared.</span></span>

<span data-ttu-id="52ad7-728">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-728">__Note.__</span></span> <span data-ttu-id="52ad7-729">`Set` 访问器具有行位置子例程具有同样的限制。</span><span class="sxs-lookup"><span data-stu-id="52ad7-729">`Set` accessors have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="52ad7-730">开始语句，end 语句和块必须出现在逻辑行的开头开始。</span><span class="sxs-lookup"><span data-stu-id="52ad7-730">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>

```antlr
PropertySetDeclaration
    : Attributes? AccessModifier? 'Set'
      ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block?
      'End' 'Set' StatementTerminator
    ;
```

### <a name="default-properties"></a><span data-ttu-id="52ad7-731">默认属性</span><span class="sxs-lookup"><span data-stu-id="52ad7-731">Default Properties</span></span>

<span data-ttu-id="52ad7-732">一个属性，指定修饰符`Default`称为*默认属性*。</span><span class="sxs-lookup"><span data-stu-id="52ad7-732">A property that specifies the modifier `Default` is called a *default property*.</span></span> <span data-ttu-id="52ad7-733">任何类型都允许属性可能具有默认属性，包括接口。</span><span class="sxs-lookup"><span data-stu-id="52ad7-733">Any type that allows properties may have a default property, including interfaces.</span></span> <span data-ttu-id="52ad7-734">而无需限定属性名称的实例可以引用的默认属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-734">The default property may be referenced without having to qualify the instance with the name of the property.</span></span> <span data-ttu-id="52ad7-735">因此，给定一个类</span><span class="sxs-lookup"><span data-stu-id="52ad7-735">Thus, given a class</span></span>

```vb
Class Test
    Public Default ReadOnly Property Item(i As Integer) As Integer
        Get
            Return i
        End Get
    End Property
End Class
```

<span data-ttu-id="52ad7-736">代码</span><span class="sxs-lookup"><span data-stu-id="52ad7-736">the code</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x(10)
    End Sub
End Module
```

<span data-ttu-id="52ad7-737">等效于</span><span class="sxs-lookup"><span data-stu-id="52ad7-737">is equivalent to</span></span>

```vb
Module TestModule
    Sub Main()
        Dim x As Test = New Test()
        Dim y As Integer

        y = x.Item(10)
    End Sub
End Module
```

<span data-ttu-id="52ad7-738">一旦某个属性声明`Default`，是否已声明的所有属性的继承层次结构中该名称上重载成为默认属性，`Default`与否。</span><span class="sxs-lookup"><span data-stu-id="52ad7-738">Once a property is declared `Default`, all of the properties overloaded on that name in the inheritance hierarchy become the default property, whether they have been declared `Default` or not.</span></span> <span data-ttu-id="52ad7-739">声明属性`Default`中派生的类的基类声明为另一个名称的默认属性不需要任何其他修饰符如时`Shadows`或`Overrides`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-739">Declaring a property `Default` in a derived class when the base class declared a default property by another name does not require any other modifiers such as `Shadows` or `Overrides`.</span></span> <span data-ttu-id="52ad7-740">这是因为默认属性，也没有任何标识签名，因此不能被隐藏或重载。</span><span class="sxs-lookup"><span data-stu-id="52ad7-740">This is because the default property has no identity or signature and so cannot be shadowed or overloaded.</span></span> <span data-ttu-id="52ad7-741">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-741">For example:</span></span>

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

<span data-ttu-id="52ad7-742">此程序将生成的输出：</span><span class="sxs-lookup"><span data-stu-id="52ad7-742">This program will produce the output:</span></span>

```
MoreDerived = 10
Derived = 10
Base = 10
```

<span data-ttu-id="52ad7-743">所有类型中声明的默认属性必须具有相同的名称，并为清楚起见，必须指定`Default`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-743">All default properties declared within a type must have the same name and, for clarity, must specify the `Default` modifier.</span></span> <span data-ttu-id="52ad7-744">原因是不带索引参数的默认属性将导致不明确的情况下，分配包含类的实例时，默认属性必须具有索引参数。</span><span class="sxs-lookup"><span data-stu-id="52ad7-744">Because a default property with no index parameters would cause an ambiguous situation when assigning instances of the containing class, default properties must have index parameters.</span></span> <span data-ttu-id="52ad7-745">此外，如果一个属性重载上特定名称中包含`Default`修饰符，该名称上重载的所有属性必须都指定它。</span><span class="sxs-lookup"><span data-stu-id="52ad7-745">Furthermore, if one property overloaded on a particular name includes the `Default` modifier, all properties overloaded on that name must specify it.</span></span> <span data-ttu-id="52ad7-746">默认属性可能不是`Shared`，并且至少一个访问器的属性不得`Private`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-746">Default properties may not be `Shared`, and at least one accessor of the property must not be `Private`.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="52ad7-747">自动实现的属性</span><span class="sxs-lookup"><span data-stu-id="52ad7-747">Automatically Implemented Properties</span></span>

<span data-ttu-id="52ad7-748">如果属性的任何访问器声明中省略，该属性的实现将自动提供除非属性在接口中声明或声明`MustOverride`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-748">If a property omits declaration of any accessors, an implementation of the property will be automatically supplied unless the property is declared in an interface or is declared `MustOverride`.</span></span> <span data-ttu-id="52ad7-749">可以自动实现仅读/写属性不带任何参数;否则将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-749">Only read/write properties with no arguments can be automatically implemented; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="52ad7-750">自动实现的属性`x`，即使其中一个重写另一个属性，引入了专用的本地变量`_x`具有相同类型的属性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-750">An automatically implemented property `x`, even one overriding another property, introduces a private local variable `_x` with the same type as the property.</span></span> <span data-ttu-id="52ad7-751">如果没有本地变量的名称与另一个声明之间的冲突，将报告一个编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-751">If there is a collision between the local variable's name and another declaration, a compile-time error will be reported.</span></span> <span data-ttu-id="52ad7-752">自动实现的属性`Get`访问器返回的本地版本和该属性的值`Set`设置该局部变量的值的访问器。</span><span class="sxs-lookup"><span data-stu-id="52ad7-752">The automatically implemented property's `Get` accessor returns the value of the local and the property's `Set` accessor that sets the value of the local.</span></span> <span data-ttu-id="52ad7-753">例如，以下声明中：</span><span class="sxs-lookup"><span data-stu-id="52ad7-753">For example, the declaration:</span></span>

```vb
Public Property x() As Integer
```

<span data-ttu-id="52ad7-754">是大致相当于：</span><span class="sxs-lookup"><span data-stu-id="52ad7-754">is roughly equivalent to:</span></span>

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

<span data-ttu-id="52ad7-755">与变量声明自动实现的属性可以包含初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="52ad7-755">As with variable declarations, an automatically implemented property can include an initializer.</span></span> <span data-ttu-id="52ad7-756">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-756">For example:</span></span>

```vb
Public Property x() As Integer = 10
Public Shared Property y() As New Customer() With { .Name = "Bob" }
```

<span data-ttu-id="52ad7-757">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-757">__Note.__</span></span> <span data-ttu-id="52ad7-758">初始化自动实现的属性时，它是通过属性，而不是基础的字段进行初始化。</span><span class="sxs-lookup"><span data-stu-id="52ad7-758">When an automatically implemented property is initialized, it is initialized through the property, not the underlying field.</span></span> <span data-ttu-id="52ad7-759">这是因此重写属性可以拦截初始化在需要时。</span><span class="sxs-lookup"><span data-stu-id="52ad7-759">This is so overriding properties can intercept the initialization if they need to.</span></span>

<span data-ttu-id="52ad7-760">数组初始值设定项允许在自动实现的属性，只不过没有方法来显式指定数组界限。</span><span class="sxs-lookup"><span data-stu-id="52ad7-760">Array initializers are allowed on automatically implemented properties, except that there is no way to specify the array bounds explicitly.</span></span>  <span data-ttu-id="52ad7-761">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-761">For example:</span></span>

```vb
' Valid
Property x As Integer() = {1, 2, 3}
Property y As Integer(,) = {{1, 2, 3}, {12, 13, 14}, {11, 10, 9}}

' Invalid
Property x4(5) As Short
```

### <a name="iterator-properties"></a><span data-ttu-id="52ad7-762">迭代器属性</span><span class="sxs-lookup"><span data-stu-id="52ad7-762">Iterator Properties</span></span>

<span data-ttu-id="52ad7-763">*迭代器属性*是属性与`Iterator`修饰符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-763">An *iterator property* is a property with the `Iterator` modifier.</span></span> <span data-ttu-id="52ad7-764">它使用相同的原因的迭代器方法 (部分[迭代器方法](statements.md#iterator-methods)) 作为生成的序列的简便方法使用其中一个可供`For Each`语句。</span><span class="sxs-lookup"><span data-stu-id="52ad7-764">It is used for the same reason an iterator method (Section [Iterator Methods](statements.md#iterator-methods)) is used -- as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="52ad7-765">`Get`迭代器属性的访问器解释迭代器方法的方式相同。</span><span class="sxs-lookup"><span data-stu-id="52ad7-765">The `Get` accessor of an iterator property is interpreted in the same way as an iterator method.</span></span>

<span data-ttu-id="52ad7-766">一个迭代器属性必须具有一个显式`Get`访问器，并且其类型必须是`IEnumerator`，或`IEnumerable`，或`IEnumerator(Of T)`或`IEnumerable(Of T)`某些`T`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-766">An iterator property must have an explicit `Get` accessor, and its type must be `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`.</span></span>

<span data-ttu-id="52ad7-767">下面是一个迭代器属性的示例：</span><span class="sxs-lookup"><span data-stu-id="52ad7-767">Here is an example of an iterator property:</span></span>

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

## <a name="operators"></a><span data-ttu-id="52ad7-768">运算符</span><span class="sxs-lookup"><span data-stu-id="52ad7-768">Operators</span></span>

<span data-ttu-id="52ad7-769">*运算符*定义包含类的现有 Visual Basic 运算符的含义的方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-769">*Operators* are methods that define the meaning of an existing Visual Basic operator for the containing class.</span></span> <span data-ttu-id="52ad7-770">运算符应用于表达式中的类，运算符会编译到的类中定义的运算符方法调用。</span><span class="sxs-lookup"><span data-stu-id="52ad7-770">When the operator is applied to the class in an expression, the operator is compiled into a call to the operator method defined in the class.</span></span> <span data-ttu-id="52ad7-771">定义一个运算符的类是也称为*重载*运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-771">Defining an operator for a class is also known as *overloading* the operator.</span></span>

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

<span data-ttu-id="52ad7-772">不能重载已经存在; 运算符在实践中，这主要适用于转换运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-772">It is not possible to overload an operator that already exists; in practice, this primarily applies to conversion operators.</span></span> <span data-ttu-id="52ad7-773">例如，不能重载从派生类到基类的转换：</span><span class="sxs-lookup"><span data-stu-id="52ad7-773">For example, it is not possible to overload the conversion from a derived class to a base class:</span></span>

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

<span data-ttu-id="52ad7-774">此外可以在 word 的常识重载运算符：</span><span class="sxs-lookup"><span data-stu-id="52ad7-774">Operators can also be overloaded in the common sense of the word:</span></span>

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

<span data-ttu-id="52ad7-775">运算符声明不显式添加名称为包含类型声明空间;但是它们隐式声明开头的字符"op_"的相应方法。</span><span class="sxs-lookup"><span data-stu-id="52ad7-775">Operator declarations do not explicitly add names to the containing type's declaration space; however they do implicitly declare a corresponding method starting with the characters "op_".</span></span> <span data-ttu-id="52ad7-776">以下部分列出了每个运算符与相应的方法名称。</span><span class="sxs-lookup"><span data-stu-id="52ad7-776">The following sections list the corresponding method names with each operator.</span></span>

<span data-ttu-id="52ad7-777">有三个类可以定义的运算符： 一元运算符、 二元运算符和转换运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-777">There are three classes of operators that can be defined: unary operators, binary operators and conversion operators.</span></span> <span data-ttu-id="52ad7-778">所有运算符声明都共享某些限制：</span><span class="sxs-lookup"><span data-stu-id="52ad7-778">All operator declarations share certain restrictions:</span></span>

* <span data-ttu-id="52ad7-779">运算符声明都必须始终是`Public`和`Shared`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-779">Operator declarations must always be `Public` and `Shared`.</span></span> <span data-ttu-id="52ad7-780">`Public`修饰符，则可以省略在其中，将假设修饰符的上下文中。</span><span class="sxs-lookup"><span data-stu-id="52ad7-780">The `Public` modifier can be omitted in contexts where the modifier will be assumed.</span></span>

* <span data-ttu-id="52ad7-781">不能声明运算符的参数`ByRef`，`Optional`或`ParamArray`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-781">The parameters of an operator cannot be declared `ByRef`, `Optional` or `ParamArray`.</span></span>

* <span data-ttu-id="52ad7-782">在至少一个操作数或返回值的类型必须包含运算符的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-782">The type of at least one of the operands or the return value must be the type that contains the operator.</span></span>

* <span data-ttu-id="52ad7-783">没有为运算符定义的函数返回变量。</span><span class="sxs-lookup"><span data-stu-id="52ad7-783">There is no function return variable defined for operators.</span></span> <span data-ttu-id="52ad7-784">因此，`Return`语句必须用于从一个运算符正文返回值。</span><span class="sxs-lookup"><span data-stu-id="52ad7-784">Therefore, the `Return` statement must be used to return values from an operator body.</span></span>

<span data-ttu-id="52ad7-785">这些限制的唯一例外适用于可以为 null 值类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-785">The only exception to these restrictions applies to nullable value types.</span></span> <span data-ttu-id="52ad7-786">可以为 null 值类型不具有实际类型定义，因为值类型可以声明为可以为 null 的类型版本的用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-786">Since nullable value types do not have an actual type definition, a value type can declare user-defined operators for the nullable version of the type.</span></span> <span data-ttu-id="52ad7-787">确定某一特定的用户定义运算符，可以将类型声明时`?`修饰符出于有效性检查的第一次删除从所有声明中所涉及的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-787">When determining whether a type can declare a particular user-defined operator, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="52ad7-788">这一放宽不适用于的返回类型`IsTrue`并`IsFalse`运算符; 它们必须仍返回`Boolean`，而不`Boolean?`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-788">This relaxation does not apply to the return type of the `IsTrue` and `IsFalse` operators; they must still return `Boolean`, not `Boolean?`.</span></span>

<span data-ttu-id="52ad7-789">在运算符声明不能修改的优先顺序和运算符的关联性。</span><span class="sxs-lookup"><span data-stu-id="52ad7-789">The precedence and associativity of an operator cannot be modified by an operator declaration.</span></span>

<span data-ttu-id="52ad7-790">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-790">__Note.__</span></span> <span data-ttu-id="52ad7-791">运算符具有子例程的行位置上具有同样的限制。</span><span class="sxs-lookup"><span data-stu-id="52ad7-791">Operators have the same restriction on line placement that subroutines have.</span></span> <span data-ttu-id="52ad7-792">开始语句，end 语句和块必须出现在逻辑行的开头开始。</span><span class="sxs-lookup"><span data-stu-id="52ad7-792">The beginning statement, end statement and block must all appear at the beginning of a logical line.</span></span>


### <a name="unary-operators"></a><span data-ttu-id="52ad7-793">一元运算符</span><span class="sxs-lookup"><span data-stu-id="52ad7-793">Unary Operators</span></span>

<span data-ttu-id="52ad7-794">可以重载下列一元运算符：</span><span class="sxs-lookup"><span data-stu-id="52ad7-794">The following unary operators can be overloaded:</span></span>

* <span data-ttu-id="52ad7-795">一元加运算符`+`(相应的方法： `op_UnaryPlus`)</span><span class="sxs-lookup"><span data-stu-id="52ad7-795">The unary plus operator `+` (corresponding method: `op_UnaryPlus`)</span></span>

* <span data-ttu-id="52ad7-796">一元负运算符`-`(相应的方法： `op_UnaryNegation`)</span><span class="sxs-lookup"><span data-stu-id="52ad7-796">The unary minus operator `-` (corresponding method: `op_UnaryNegation`)</span></span>

* <span data-ttu-id="52ad7-797">逻辑`Not`运算符 (相应的方法： `op_OnesComplement`)</span><span class="sxs-lookup"><span data-stu-id="52ad7-797">The logical `Not` operator (corresponding method: `op_OnesComplement`)</span></span>

* <span data-ttu-id="52ad7-798">`IsTrue`并`IsFalse`运算符 (相应的方法： `op_True`， `op_False`)</span><span class="sxs-lookup"><span data-stu-id="52ad7-798">The `IsTrue` and `IsFalse` operators (corresponding methods: `op_True`, `op_False`)</span></span>

<span data-ttu-id="52ad7-799">所有重载的一元运算符必须采用一个参数包含类型，并可能会返回任何类型，除了`IsTrue`并`IsFalse`，其必须返回`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-799">All overloaded unary operators must take a single parameter of the containing type and may return any type, except for `IsTrue` and `IsFalse`, which must return `Boolean`.</span></span> <span data-ttu-id="52ad7-800">如果包含类型是泛型类型，类型参数必须与包含类型的类型参数匹配。</span><span class="sxs-lookup"><span data-stu-id="52ad7-800">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="52ad7-801">例如，应用于对象的</span><span class="sxs-lookup"><span data-stu-id="52ad7-801">For example,</span></span>

```vb
Structure Complex
    ...

    Public Shared Operator +(v As Complex) As Complex
        Return v
    End Operator
End Structure
```

<span data-ttu-id="52ad7-802">如果某个类型重载之一`IsTrue`或`IsFalse`，则它必须对其他重载。</span><span class="sxs-lookup"><span data-stu-id="52ad7-802">If a type overloads one of `IsTrue` or `IsFalse`, then it must overload the other as well.</span></span> <span data-ttu-id="52ad7-803">如果只有一个已重载，会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-803">If only one is overloaded, a compile-time error results.</span></span>

<span data-ttu-id="52ad7-804">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-804">__Note.__</span></span> <span data-ttu-id="52ad7-805">`IsTrue` 和`IsFalse`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="52ad7-805">`IsTrue` and `IsFalse` are not reserved words.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="52ad7-806">二元运算符</span><span class="sxs-lookup"><span data-stu-id="52ad7-806">Binary Operators</span></span>

<span data-ttu-id="52ad7-807">以下二进制运算符可以进行重载：</span><span class="sxs-lookup"><span data-stu-id="52ad7-807">The following binary operators can be overloaded:</span></span>

* <span data-ttu-id="52ad7-808">加法`+`，减法`-`，乘法`*`，除`/`，整数除法`\`，取模`Mod`和求幂`^`运算符 (相应的方法：`op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span><span class="sxs-lookup"><span data-stu-id="52ad7-808">The addition `+`, subtraction `-`, multiplication `*`, division `/`, integral division `\`, modulo `Mod` and exponentiation `^` operators (corresponding method: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_IntegerDivision`, `op_Modulus`, `op_Exponent`)</span></span>

* <span data-ttu-id="52ad7-809">关系运算符`=`， `<>`， `<`， `>`， `<=`， `>=` (相应的方法： `op_Equality`， `op_Inequality`， `op_LessThan`， `op_GreaterThan`， `op_LessThanOrEqual`, `op_GreaterThanOrEqual`).</span><span class="sxs-lookup"><span data-stu-id="52ad7-809">The relational operators `=`, `<>`, `<`, `>`, `<=`, `>=` (corresponding methods: `op_Equality`, `op_Inequality`, `op_LessThan`, `op_GreaterThan`, `op_LessThanOrEqual`, `op_GreaterThanOrEqual`).</span></span> <span data-ttu-id="52ad7-810">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-810">__Note.__</span></span> <span data-ttu-id="52ad7-811">虽然可以重载相等运算符，不能重载赋值运算符 （仅在赋值语句中使用）。</span><span class="sxs-lookup"><span data-stu-id="52ad7-811">While the equality operator can be overloaded, the assignment operator (used only in assignment statements) cannot be overloaded.</span></span>

* <span data-ttu-id="52ad7-812">`Like`运算符 (相应的方法： `op_Like`)</span><span class="sxs-lookup"><span data-stu-id="52ad7-812">The `Like` operator (corresponding method: `op_Like`)</span></span>

* <span data-ttu-id="52ad7-813">串联运算符`&`(相应的方法： `op_Concatenate`)</span><span class="sxs-lookup"><span data-stu-id="52ad7-813">The concatenation operator `&` (corresponding method: `op_Concatenate`)</span></span>

* <span data-ttu-id="52ad7-814">逻辑`And`，`Or`并`Xor`运算符 (相应的方法： `op_BitwiseAnd`， `op_BitwiseOr`， `op_ExclusiveOr`)</span><span class="sxs-lookup"><span data-stu-id="52ad7-814">The logical `And`, `Or` and `Xor` operators (corresponding methods: `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`)</span></span>

* <span data-ttu-id="52ad7-815">移位运算符`<<`并`>>`(相应的方法： `op_LeftShift`， `op_RightShift`)</span><span class="sxs-lookup"><span data-stu-id="52ad7-815">The shift operators `<<` and `>>` (corresponding methods: `op_LeftShift`, `op_RightShift`)</span></span>

<span data-ttu-id="52ad7-816">所有重载的二元运算符必须将包含类型作为参数之一。</span><span class="sxs-lookup"><span data-stu-id="52ad7-816">All overloaded binary operators must take the containing type as one of the parameters.</span></span> <span data-ttu-id="52ad7-817">如果包含类型是泛型类型，类型参数必须与包含类型的类型参数匹配。</span><span class="sxs-lookup"><span data-stu-id="52ad7-817">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="52ad7-818">移位运算符进一步限制此规则，要求第一个参数应包含类型;第二个参数必须始终为类型的`Integer`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-818">The shift operators further restrict this rule to require the first parameter to be of the containing type; the second parameter must always be of type `Integer`.</span></span>

<span data-ttu-id="52ad7-819">必须在对声明以下二进制运算符：</span><span class="sxs-lookup"><span data-stu-id="52ad7-819">The following binary operators must be declared in pairs:</span></span>

* <span data-ttu-id="52ad7-820">运算符`=`和运算符 `<>`</span><span class="sxs-lookup"><span data-stu-id="52ad7-820">Operator `=` and operator `<>`</span></span>

* <span data-ttu-id="52ad7-821">运算符`>`和运算符 `<`</span><span class="sxs-lookup"><span data-stu-id="52ad7-821">Operator `>` and operator `<`</span></span>

* <span data-ttu-id="52ad7-822">运算符`>=`和运算符 `<=`</span><span class="sxs-lookup"><span data-stu-id="52ad7-822">Operator `>=` and operator `<=`</span></span>

<span data-ttu-id="52ad7-823">如果该对的其中一个被声明，则还必须具有匹配的参数和返回类型声明的其他或将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-823">If one of the pair is declared, then the other must also be declared with matching parameter and return types, or a compile-time error will result.</span></span> <span data-ttu-id="52ad7-824">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-824">(__Note.__</span></span> <span data-ttu-id="52ad7-825">需要配对关系运算符的声明的目的是尝试并确保至少最低级别的重载运算符的逻辑一致性。）</span><span class="sxs-lookup"><span data-stu-id="52ad7-825">The purpose of requiring paired declarations of relational operators is to try and ensure at least a minimum level of logical consistency in overloaded operators.)</span></span>

<span data-ttu-id="52ad7-826">与关系运算符不同部门和整数除法运算符重载是强烈建议不要使用的但不是错误。</span><span class="sxs-lookup"><span data-stu-id="52ad7-826">In contrast to the relational operators, overloading both the division and integral division operators is strongly discouraged, although not an error.</span></span> <span data-ttu-id="52ad7-827">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="52ad7-827">(__Note.__</span></span> <span data-ttu-id="52ad7-828">一般情况下，两种类型的部门应为完全不同： 支持部门的类型为整数 (在这种情况下它应支持`\`) 或不 (在这种情况下它应支持`/`)。</span><span class="sxs-lookup"><span data-stu-id="52ad7-828">In general, the two types of division should be entirely distinct: a type that supports division is either integral (in which case it should support `\`) or not (in which case it should support `/`).</span></span> <span data-ttu-id="52ad7-829">我们考虑使其错误来定义这两个运算符，但因为其语言通常不区分两种类型的除法 Visual Basic 的方式，我们认为它是最安全，若要允许的做法，但强烈不建议这样做）。</span><span class="sxs-lookup"><span data-stu-id="52ad7-829">We considered making it an error to define both operators, but because their languages do not generally distinguish between two types of division the way Visual Basic does, we felt it was safest to allow the practice but strongly discourage it.)</span></span>

<span data-ttu-id="52ad7-830">复合的赋值运算符不能直接重载。</span><span class="sxs-lookup"><span data-stu-id="52ad7-830">Compound assignment operators cannot be overloaded directly.</span></span> <span data-ttu-id="52ad7-831">相反，当重载对应的二元运算符时，复合赋值运算符将重载的运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-831">Instead, when the corresponding binary operator is overloaded, the compound assignment operator will use the overloaded operator.</span></span> <span data-ttu-id="52ad7-832">例如：</span><span class="sxs-lookup"><span data-stu-id="52ad7-832">For example:</span></span>

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

### <a name="conversion-operators"></a><span data-ttu-id="52ad7-833">转换运算符</span><span class="sxs-lookup"><span data-stu-id="52ad7-833">Conversion Operators</span></span>

<span data-ttu-id="52ad7-834">转换运算符定义类型之间的新转换。</span><span class="sxs-lookup"><span data-stu-id="52ad7-834">Conversion operators define new conversions between types.</span></span> <span data-ttu-id="52ad7-835">这些新转换称为*用户定义的转换*。</span><span class="sxs-lookup"><span data-stu-id="52ad7-835">These new conversions are called *user-defined conversions*.</span></span> <span data-ttu-id="52ad7-836">转换运算符将源类型，即转换运算符，为目标类型，指示转换运算符的返回类型的参数类型转换。</span><span class="sxs-lookup"><span data-stu-id="52ad7-836">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span> <span data-ttu-id="52ad7-837">转换必须归类为扩大转换或收缩。</span><span class="sxs-lookup"><span data-stu-id="52ad7-837">Conversions must be classified as either widening or narrowing.</span></span> <span data-ttu-id="52ad7-838">包括的转换运算符声明`Widening`关键字引入的用户定义的扩大转换 (相应的方法： `op_Implicit`)。</span><span class="sxs-lookup"><span data-stu-id="52ad7-838">A conversion operator declaration that includes the `Widening` keyword introduces a user-defined widening conversion (corresponding method: `op_Implicit`).</span></span> <span data-ttu-id="52ad7-839">包括的转换运算符声明`Narrowing`关键字引入的用户定义的收缩转换 (相应的方法： `op_Explicit`)。</span><span class="sxs-lookup"><span data-stu-id="52ad7-839">A conversion operator declaration that includes the `Narrowing` keyword introduces a user-defined narrowing conversion (corresponding method: `op_Explicit`).</span></span>

<span data-ttu-id="52ad7-840">通常情况下，应设计用户定义的扩大转换，永远不会引发异常，并且永远不会丢失信息。</span><span class="sxs-lookup"><span data-stu-id="52ad7-840">In general, user-defined widening conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="52ad7-841">如果 （例如，因为源参数不在范围内），用户定义的转换可能会导致异常或丢失的信息 （如放弃高顺序位），则该转换应定义为收缩转换。</span><span class="sxs-lookup"><span data-stu-id="52ad7-841">If a user-defined conversion can cause exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as a narrowing conversion.</span></span> <span data-ttu-id="52ad7-842">在下面的示例中：</span><span class="sxs-lookup"><span data-stu-id="52ad7-842">In the example:</span></span>

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

<span data-ttu-id="52ad7-843">从转换`Digit`到`Byte`是一个扩大转换，因为它永远不会引发异常或丢失信息，但从转换`Byte`到`Digit`以来的收缩转换`Digit`只能表示可能值的子集`Byte`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-843">the conversion from `Digit` to `Byte` is a widening conversion because it never throws exceptions or loses information, but the conversion from `Byte` to `Digit` is a narrowing conversion since `Digit` can only represent a subset of the possible values of a `Byte`.</span></span>

<span data-ttu-id="52ad7-844">与其他类型的所有成员可进行重载，不同的转换运算符的签名包含转换的目标类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-844">Unlike all other type members that can be overloaded, the signature of a conversion operator includes the target type of the conversion.</span></span> <span data-ttu-id="52ad7-845">这是唯一的类型成员签名中参与的返回类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-845">This is the only type member for which the return type participates in the signature.</span></span> <span data-ttu-id="52ad7-846">扩大转换或收缩分类的转换运算符，但是，不是签名的运算符的一部分。</span><span class="sxs-lookup"><span data-stu-id="52ad7-846">The widening or narrowing classification of a conversion operator, however, is not part of the operator's signature.</span></span> <span data-ttu-id="52ad7-847">因此，类或结构無法宣告的扩大转换运算符和收缩转换运算符具有相同的源和目标类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-847">Thus, a class or structure cannot declare both a widening conversion operator and a narrowing conversion operator with the same source and target types.</span></span>

<span data-ttu-id="52ad7-848">用户定义的转换运算符必须转换为或从包含类型--例如，它是由于可以将类`C`定义的转换从`C`到`Integer`并从`Integer`到`C`，但不能从`Integer`到`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="52ad7-848">A user-defined conversion operator must convert either to or from the containing type -- for example, it is possible for a class `C` to define a conversion from `C` to `Integer` and from `Integer` to `C`, but not from `Integer` to `Boolean`.</span></span> <span data-ttu-id="52ad7-849">如果包含类型是泛型类型，类型参数必须与包含类型的类型参数匹配。</span><span class="sxs-lookup"><span data-stu-id="52ad7-849">If the containing type is a generic type, the type parameters must match the containing type's type parameters.</span></span> <span data-ttu-id="52ad7-850">此外，不能重新定义的内部函数 （即非-用户定义的） 转换。</span><span class="sxs-lookup"><span data-stu-id="52ad7-850">Also, it is not possible to redefine an intrinsic (i.e. non-user-defined) conversion.</span></span> <span data-ttu-id="52ad7-851">一个类型不能声明转换的结果，其中：</span><span class="sxs-lookup"><span data-stu-id="52ad7-851">As a result, a type cannot declare a conversion where:</span></span>

* <span data-ttu-id="52ad7-852">源类型和目标类型是相同的。</span><span class="sxs-lookup"><span data-stu-id="52ad7-852">The source type and the destination type are the same.</span></span>

* <span data-ttu-id="52ad7-853">源类型和目标类型不是定义转换运算符的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-853">Both the source type and the destination type are not the type that defines the conversion operator.</span></span>

* <span data-ttu-id="52ad7-854">源类型或目标类型是一个接口类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-854">The source type or the destination type is an interface type.</span></span>

* <span data-ttu-id="52ad7-855">通过继承相关联的源类型和目标类型 (包括`Object`)。</span><span class="sxs-lookup"><span data-stu-id="52ad7-855">The source type and destination types are related by inheritance (including `Object`).</span></span>

<span data-ttu-id="52ad7-856">这些规则的唯一例外适用于可以为 null 值类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-856">The only exception to these rules applies to nullable value types.</span></span> <span data-ttu-id="52ad7-857">可以为 null 值类型不具有实际类型定义，因为值类型可以声明为可以为 null 的类型版本的用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="52ad7-857">Since nullable value types do not have an actual type definition, a value type can declare user-defined conversions for the nullable version of the type.</span></span> <span data-ttu-id="52ad7-858">确定特定的用户定义转换，可以将类型声明时`?`修饰符出于有效性检查的第一次删除从所有声明中所涉及的类型。</span><span class="sxs-lookup"><span data-stu-id="52ad7-858">When determining whether a type can declare a particular user-defined conversion, the `?` modifiers are first dropped off of all of the types involved in the declaration for the purposes of validity checking.</span></span> <span data-ttu-id="52ad7-859">因此，以下声明是有效的因为`S`可以定义从转换`S`到`T`:</span><span class="sxs-lookup"><span data-stu-id="52ad7-859">Thus, the following declaration is valid because `S` can define a conversion from `S` to `T`:</span></span>

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

<span data-ttu-id="52ad7-860">下面的声明不是有效的但是，因为结构`S`不能定义从转换`S`到`S`:</span><span class="sxs-lookup"><span data-stu-id="52ad7-860">The following declaration is not valid, however, because structure `S` cannot define a conversion from `S` to `S`:</span></span>

```vb
Structure S
    Public Shared Widening Operator CType(ByVal v As S) As S?
        ...
    End Operator
End Structure
```

### <a name="operator-mapping"></a><span data-ttu-id="52ad7-861">运算符映射</span><span class="sxs-lookup"><span data-stu-id="52ad7-861">Operator Mapping</span></span>

<span data-ttu-id="52ad7-862">由于 Visual Basic 支持的运算符集可能不完全匹配的运算符集在.NET Framework 上的其他语言，某些运算符映射到其他运算符时正在定义或使用专门。</span><span class="sxs-lookup"><span data-stu-id="52ad7-862">Because the set of operators that Visual Basic supports may not exactly match the set of operators that other languages on the .NET Framework, some operators are mapped specially onto other operators when being defined or used.</span></span> <span data-ttu-id="52ad7-863">尤其是在下列情况下：</span><span class="sxs-lookup"><span data-stu-id="52ad7-863">Specifically:</span></span>

* <span data-ttu-id="52ad7-864">定义一个整数除法运算符将自动定义正则除法运算符 （仅从其他语言使用），将调用整数除法运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-864">Defining an integral division operator will automatically define a regular division operator (usable only from other languages) that will call the integral division operator.</span></span>

* <span data-ttu-id="52ad7-865">重载`Not`， `And`，和`Or`运算符将重载仅从其他语言之间的逻辑和按位运算符区分开来的角度来看的按位运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-865">Overloading the `Not`, `And`, and `Or` operators will overload only the bitwise operator from the perspective of other languages that distinguish between logical and bitwise operators.</span></span>

* <span data-ttu-id="52ad7-866">重载仅逻辑和位运算符可区分的语言中的逻辑运算符的类 (即使用语言`op_LogicalNot`， `op_LogicalAnd`，并`op_LogicalOr`有关`Not`， `And`，和`Or`分别) 将具有其映射到 Visual Basic 逻辑运算符的逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-866">A class that overloads only the logical operators in a language that distinguishes between logical and bitwise operators (i.e. a languages that uses `op_LogicalNot`, `op_LogicalAnd`, and `op_LogicalOr` for `Not`, `And`, and `Or`, respectively) will have their logical operators mapped onto the Visual Basic logical operators.</span></span> <span data-ttu-id="52ad7-867">如果重载逻辑和位运算符时，将使用只有按位运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-867">If both the logical and bitwise operators are overloaded, only the bitwise operators will be used.</span></span>

* <span data-ttu-id="52ad7-868">重载`<<`和`>>`运算符将重载运算符从签名和未签名的移位运算符区分其他语言的角度来看仅签名。</span><span class="sxs-lookup"><span data-stu-id="52ad7-868">Overloading the `<<` and `>>` operators will overload only the signed operators from the perspective of other languages that distinguish between signed and unsigned shift operators.</span></span>

* <span data-ttu-id="52ad7-869">重载的无符号的移位运算符的类将有映射到相应的 Visual Basic 移位运算符的无符号的移位运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-869">A class that overloads only an unsigned shift operator will have the unsigned shift operator mapped onto the corresponding Visual Basic shift operator.</span></span> <span data-ttu-id="52ad7-870">如果这两个未签名和签名移位运算符被重载，将使用仅为有符号的移位运算符。</span><span class="sxs-lookup"><span data-stu-id="52ad7-870">If both an unsigned and signed shift operator is overloaded, only the signed shift operator will be used.</span></span>
