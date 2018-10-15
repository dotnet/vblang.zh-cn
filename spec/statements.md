# <a name="statements"></a><span data-ttu-id="6f08f-101">语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-101">Statements</span></span>

<span data-ttu-id="6f08f-102">语句表示的可执行代码。</span><span class="sxs-lookup"><span data-stu-id="6f08f-102">Statements represent executable code.</span></span>

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

<span data-ttu-id="6f08f-103">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-103">__Note.__</span></span> <span data-ttu-id="6f08f-104">Microsoft Visual Basic 编译器仅允许一个关键字或标识符开头的语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-104">The Microsoft Visual Basic Compiler only allows statements which start with a keyword or an identifier.</span></span> <span data-ttu-id="6f08f-105">因此，例如，调用语句"`Call (Console).WriteLine`"允许，但调用语句"`(Console).WriteLine`"不是。</span><span class="sxs-lookup"><span data-stu-id="6f08f-105">Thus, for instance, the invocation statement "`Call (Console).WriteLine`" is allowed, but the invocation statement "`(Console).WriteLine`" is not.</span></span>

## <a name="control-flow"></a><span data-ttu-id="6f08f-106">控制流</span><span class="sxs-lookup"><span data-stu-id="6f08f-106">Control Flow</span></span>

<span data-ttu-id="6f08f-107">*控制流*是语句和表达式的执行的顺序。</span><span class="sxs-lookup"><span data-stu-id="6f08f-107">*Control flow* is the sequence in which statements and expressions are executed.</span></span> <span data-ttu-id="6f08f-108">执行顺序取决于特定的语句或表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-108">The order of execution depends on the particular statement or expression.</span></span>

<span data-ttu-id="6f08f-109">例如，当评估加法运算符 (部分[加法运算符](expressions.md#addition-operator))，第一个左的操作数的求值，然后右操作数，然后运算符本身。</span><span class="sxs-lookup"><span data-stu-id="6f08f-109">For example, when evaluating an addition operator (Section [Addition Operator](expressions.md#addition-operator)), first the left operand is evaluated, then the right operand, and then the operator itself.</span></span> <span data-ttu-id="6f08f-110">块被执行 (部分[块和标签](statements.md#blocks-and-labels)) 方法是首先执行其第一个子语句，然后继续操作的块的语句通过逐个。</span><span class="sxs-lookup"><span data-stu-id="6f08f-110">Blocks are executed (Section [Blocks and Labels](statements.md#blocks-and-labels)) by first executing their first substatement, and then proceeding one by one through the statements of the block.</span></span>

<span data-ttu-id="6f08f-111">在此隐式排序是这一概念*控制点*，这是要执行的下一步操作。</span><span class="sxs-lookup"><span data-stu-id="6f08f-111">Implicit in this ordering is the concept of a *control point*, which is the next operation to be executed.</span></span> <span data-ttu-id="6f08f-112">当一个方法调用 （或"调用"） 时，我们说它会创建*实例*的方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-112">When a method is invoked (or "called"), we say it creates an *instance* of the method.</span></span> <span data-ttu-id="6f08f-113">方法实例由其自己的方法的参数和本地变量和其自己的控制点副本组成。</span><span class="sxs-lookup"><span data-stu-id="6f08f-113">A method instance consists of its own copy of the method's parameters and local variables, and its own control point.</span></span>

### <a name="regular-methods"></a><span data-ttu-id="6f08f-114">正则方法</span><span class="sxs-lookup"><span data-stu-id="6f08f-114">Regular Methods</span></span>

<span data-ttu-id="6f08f-115">下面是正则方法的示例</span><span class="sxs-lookup"><span data-stu-id="6f08f-115">Here is an example of a regular method</span></span>

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

<span data-ttu-id="6f08f-116">常规方法调用时，</span><span class="sxs-lookup"><span data-stu-id="6f08f-116">When a regular method is invoked,</span></span>

1. <span data-ttu-id="6f08f-117">首先创建特定于该调用的方法的实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-117">First an instance of the method is created specific to that invocation.</span></span> <span data-ttu-id="6f08f-118">此实例包括一份所有参数和局部变量的方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-118">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="6f08f-119">然后它的所有参数都初始化为提供的值，以及所有局部变量及其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-119">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="6f08f-120">情况下`Function`，还初始化隐式的本地变量调用*函数返回变量*其名称是函数的名称，其类型为该函数的返回类型，其初始值为默认值其类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-120">In the case of a `Function`, an implicit local variable is also initialized called the *function return variable* whose name is the function's name, whose type is the return type of the function and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="6f08f-121">第一个语句的方法体中，然后设置方法实例的控点并将方法主体立即开始从该处执行 (部分[块和标签](statements.md#blocks-and-labels))。</span><span class="sxs-lookup"><span data-stu-id="6f08f-121">The method instance's control point is then set at the first statement of the method body, and the method body immediately starts to execute from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="6f08f-122">控制流退出时的方法体通常-通过到达`End Function`或`End Sub`标记其结束时，或通过使用显式`Return`或`Exit`语句的控制流返回到调用方的方法实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-122">When control flow exits the method body normally - through reaching the `End Function` or `End Sub` that mark its end, or through an explicit `Return` or `Exit` statement - control flow returns to the caller of the method instance.</span></span> <span data-ttu-id="6f08f-123">如果函数返回变量，调用的结果将是此变量的最终值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-123">If there is a function return variable, the result of the invocation is the final value of this variable.</span></span>

<span data-ttu-id="6f08f-124">当控制流退出方法正文通过未经处理的异常时，该异常将传播到调用方。</span><span class="sxs-lookup"><span data-stu-id="6f08f-124">When control flow exits the method body through an unhandled exception, that exception is propagated to the caller.</span></span>

<span data-ttu-id="6f08f-125">控制流已退出后，不再有任何实时引用方法的实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-125">After control flow has exited, there are no longer any live references to the method instance.</span></span> <span data-ttu-id="6f08f-126">如果方法实例保留到其本地变量或参数的副本的唯一引用，则它们可能是垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="6f08f-126">If the method instance held the only references to its copy of local variables or parameters, then they may be garbage collected.</span></span>

### <a name="iterator-methods"></a><span data-ttu-id="6f08f-127">迭代器方法</span><span class="sxs-lookup"><span data-stu-id="6f08f-127">Iterator Methods</span></span>

<span data-ttu-id="6f08f-128">为方便地生成的序列，其中一个可供使用迭代器方法`For Each`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-128">Iterator methods are used as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="6f08f-129">迭代器方法使用`Yield`语句 (部分[Yield 语句](statements.md#yield-statement)) 以提供序列的元素。</span><span class="sxs-lookup"><span data-stu-id="6f08f-129">Iterator methods use the `Yield` statement (Section [Yield Statement](statements.md#yield-statement)) to provide elements of the sequence.</span></span> <span data-ttu-id="6f08f-130">(没有迭代器方法`Yield`语句将生成一个空序列)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-130">(An iterator method with no `Yield` statements will produce an empty sequence).</span></span> <span data-ttu-id="6f08f-131">下面是迭代器方法的示例：</span><span class="sxs-lookup"><span data-stu-id="6f08f-131">Here is an example of an iterator method:</span></span>

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

<span data-ttu-id="6f08f-132">迭代器方法调用时其返回类型是`IEnumerator(Of T)`，</span><span class="sxs-lookup"><span data-stu-id="6f08f-132">When an iterator method is invoked whose return type is `IEnumerator(Of T)`,</span></span>

1. <span data-ttu-id="6f08f-133">首先被创建特定于该调用迭代器方法的实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-133">First an instance of the iterator method is created specific to that invocation.</span></span> <span data-ttu-id="6f08f-134">此实例包括一份所有参数和局部变量的方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-134">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="6f08f-135">然后它的所有参数都初始化为提供的值，以及所有局部变量及其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-135">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="6f08f-136">此外初始化隐式的本地变量调用*迭代器当前变量*，其类型是`T`且其初始值为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-136">An implicit local variable is also initialized called the *iterator current variable*, whose type is `T` and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="6f08f-137">然后在方法体中的第一个语句设置方法实例的控制点。</span><span class="sxs-lookup"><span data-stu-id="6f08f-137">The method instance's control point is then set at the first statement of the method body.</span></span>
5. <span data-ttu-id="6f08f-138">*迭代器对象*然后创建，则与此方法实例相关联。</span><span class="sxs-lookup"><span data-stu-id="6f08f-138">An *iterator object* is then created, associated with this method instance.</span></span> <span data-ttu-id="6f08f-139">迭代器对象实现声明的返回类型，并具有如下所述的行为。</span><span class="sxs-lookup"><span data-stu-id="6f08f-139">The iterator object implements the declared return type and has behavior as described below.</span></span>
6. <span data-ttu-id="6f08f-140">然后继续运行控制流*立即*在调用方，并且调用的结果是迭代器对象。</span><span class="sxs-lookup"><span data-stu-id="6f08f-140">Control flow is then resumed *immediately* in the caller, and the result of the invocation is the iterator object.</span></span> <span data-ttu-id="6f08f-141">请注意，此传输不退出迭代器方法实例，便可完成并不会导致最后处理程序执行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-141">Note that this transfer is done without exiting the iterator method instance, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="6f08f-142">方法实例仍在引用的迭代器对象，并将不会垃圾收集，只要存在对迭代器对象的实时引用。</span><span class="sxs-lookup"><span data-stu-id="6f08f-142">The method instance is still referenced by the iterator object, and will not be garbage collected so long as there exists a live reference to the iterator object.</span></span>

<span data-ttu-id="6f08f-143">当迭代器对象`Current`访问属性时，*当前变量*返回调用。</span><span class="sxs-lookup"><span data-stu-id="6f08f-143">When the iterator object's `Current` property is accessed, the *current variable* of the invocation is returned.</span></span>

<span data-ttu-id="6f08f-144">当迭代器对象的`MoveNext`调用方法、 调用不会创建新的方法实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-144">When the iterator object's `MoveNext` method is invoked, the invocation does not create a new method instance.</span></span> <span data-ttu-id="6f08f-145">而使用现有的方法实例 （和其控点和本地变量和参数） 的第一次调用迭代器方法时创建的实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-145">Instead the existing method instance is used (and its control point and local variables and parameters) - the instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="6f08f-146">控制流继续执行在该方法实例的控制点，并像平常一样的迭代器方法的正文中继续进行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-146">Control flow resumes execution at the control point of that method instance, and proceeds through the body of the iterator method as normal.</span></span>

<span data-ttu-id="6f08f-147">当迭代器对象的`Dispose`调用方法，再次使用现有的方法实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-147">When the iterator object's `Dispose` method is invoked, again the existing method instance is used.</span></span> <span data-ttu-id="6f08f-148">控制流继续在该方法实例的控制点，但然后立即行为方式如同`Exit Function`语句的下一步操作。</span><span class="sxs-lookup"><span data-stu-id="6f08f-148">Control flow resumes at the control point of that method instance, but then immediately behaves as if an `Exit Function` statement were the next operation.</span></span>

<span data-ttu-id="6f08f-149">以上描述的行为的调用`MoveNext`或`Dispose`迭代器对象仅适用于以前的所有调用`MoveNext`或`Dispose`该迭代器对象上均已返回到其调用方。</span><span class="sxs-lookup"><span data-stu-id="6f08f-149">The above descriptions of behavior for invocation of `MoveNext` or `Dispose` on an iterator object only apply if all previous invocations of `MoveNext` or `Dispose` on that iterator object have already returned to their callers.</span></span> <span data-ttu-id="6f08f-150">如果它们不具备，则行为是未定义。</span><span class="sxs-lookup"><span data-stu-id="6f08f-150">If they haven't, then the behavior is undefined.</span></span>

<span data-ttu-id="6f08f-151">控制流退出时的迭代器方法体通常-通过到达`End Function`标记其结束时，或通过显式`Return`或`Exit`语句-它必须具有执行此操作的调用的上下文中`MoveNext`或`Dispose`函数在迭代器对象以恢复迭代器方法实例，并且它将一直在使用时第一次调用迭代器方法创建方法实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-151">When control flow exits the iterator method body normally -- through reaching the `End Function` that mark its end, or through an explicit `Return` or `Exit` statement -- it must have done so in the context of an invocation of `MoveNext` or `Dispose` function on an iterator object to resume the iterator method instance, and it will have been using the method instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="6f08f-152">该实例的控制点将保留为`End Function`语句，并在调用方; 中的控制流恢复并且它必须通过调用已恢复`MoveNext`则将值`False`返回给调用方。</span><span class="sxs-lookup"><span data-stu-id="6f08f-152">The control point of that instance is left at the `End Function` statement, and control flow resumes in the caller; and if it had been resumed by a call to `MoveNext` then the value `False` is returned to the caller.</span></span>

<span data-ttu-id="6f08f-153">当控制流的退出迭代器方法正文通过未经处理的异常，则异常会传播给调用方，再次将为调用`MoveNext`或`Dispose`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-153">When control flow exits the iterator method body through an unhandled exception, then the exception is propagated to the caller, which again will be either an invocation of `MoveNext` or of `Dispose`.</span></span>

<span data-ttu-id="6f08f-154">与其他可能的返回类型的迭代器函数</span><span class="sxs-lookup"><span data-stu-id="6f08f-154">As for the other possible return types of an iterator function,</span></span>

* <span data-ttu-id="6f08f-155">迭代器方法调用时其返回类型是`IEnumerable(Of T)`对于某些`T`、 首次创建实例-特定于迭代器方法--的所有参数在方法中，该调用和初始化使用提供的值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-155">When an iterator method is invoked whose return type is `IEnumerable(Of T)` for some `T`, an instance is first created -- specific to that invocation of the iterator method -- of all parameters in the method, and they are initialized with the supplied values.</span></span> <span data-ttu-id="6f08f-156">调用的结果是实现的返回类型的对象。</span><span class="sxs-lookup"><span data-stu-id="6f08f-156">The result of the invocation is an an object which implements the return type.</span></span> <span data-ttu-id="6f08f-157">应此对象的`GetEnumerator`调用方法，它会创建实例-特定于该调用的`GetEnumerator`-的所有参数和局部变量的方法中。</span><span class="sxs-lookup"><span data-stu-id="6f08f-157">Should this object's `GetEnumerator` method be called, it creates an instance -- specific to that invocation of `GetEnumerator` -- of all parameters and local variables in the method.</span></span> <span data-ttu-id="6f08f-158">它初始化为已保存的值的参数，并继续执行与上面的迭代器方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-158">It initializes the parameters to the values already saved, and proceeds as for the iterator method above.</span></span>
* <span data-ttu-id="6f08f-159">迭代器方法调用时的返回类型为非泛型接口`IEnumerator`，则行为将与用于完全`IEnumerator(Of Object)`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-159">When an iterator method is invoked whose return type is the non-generic interface `IEnumerator`, the behavior is exactly as for `IEnumerator(Of Object)`.</span></span>
* <span data-ttu-id="6f08f-160">迭代器方法调用时的返回类型为非泛型接口`IEnumerable`，则行为将与用于完全`IEnumerable(Of Object)`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-160">When an iterator method is invoked whose return type is the non-generic interface `IEnumerable`, the behavior is exactly as for `IEnumerable(Of Object)`.</span></span>

### <a name="async-methods"></a><span data-ttu-id="6f08f-161">异步方法</span><span class="sxs-lookup"><span data-stu-id="6f08f-161">Async Methods</span></span>

<span data-ttu-id="6f08f-162">异步方法是执行长时间运行的工作而不会如阻止的应用程序 UI 的简便方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-162">Async methods are a convenient way to do long-running work without for example blocking the UI of an application.</span></span> <span data-ttu-id="6f08f-163">异步是缩写*异步*-表示异步方法的调用方将立即，继续执行，但在将来某些更高版本时，最终完成的异步方法的实例可能会发生。</span><span class="sxs-lookup"><span data-stu-id="6f08f-163">Async is short for *Asynchronous* - it means that the caller of the async method will resume its execution promptly, but the eventual completion of the async method's instance may happen at some later time in the future.</span></span> <span data-ttu-id="6f08f-164">按照约定异步方法的名称带有后缀"Async"。</span><span class="sxs-lookup"><span data-stu-id="6f08f-164">By convention async methods are named with the suffix "Async".</span></span>

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

<span data-ttu-id="6f08f-165">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-165">__Note.__</span></span> <span data-ttu-id="6f08f-166">异步方法*不*后台线程上运行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-166">Async methods are *not* run on a background thread.</span></span> <span data-ttu-id="6f08f-167">相反，它们允许方法将自身通过挂起`Await`运算符和计划本身以响应某些事件恢复。</span><span class="sxs-lookup"><span data-stu-id="6f08f-167">Instead they allow a method to suspend itself through the `Await` operator, and schedule itself to be resumed in response to some event.</span></span>

<span data-ttu-id="6f08f-168">调用异步方法时</span><span class="sxs-lookup"><span data-stu-id="6f08f-168">When an async method is invoked</span></span>

1. <span data-ttu-id="6f08f-169">首先创建特定于该调用异步方法的实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-169">First an instance of the async method is created specific to that invocation.</span></span> <span data-ttu-id="6f08f-170">此实例包括一份所有参数和局部变量的方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-170">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="6f08f-171">然后它的所有参数都初始化为提供的值，以及所有局部变量及其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-171">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="6f08f-172">在异步方法具有返回类型的情况下`Task(Of T)`对于某些`T`，还初始化隐式的本地变量调用*任务返回变量*，其类型是`T`且其初始值为默认值为`T`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-172">In the case of an async method with return type `Task(Of T)` for some `T`, an implicit local variable is also initialized called the *task return variable*, whose type is `T` and whose initial value is the default of `T`.</span></span>
4. <span data-ttu-id="6f08f-173">如果异步方法`Function`返回类型`Task`或`Task(Of T)`某些`T`，则该类型的对象隐式创建，与当前调用相关联。</span><span class="sxs-lookup"><span data-stu-id="6f08f-173">If the async method is a `Function` with return type `Task` or `Task(Of T)` for some `T`, then an object of that type implicitly created, associated with the current invocation.</span></span> <span data-ttu-id="6f08f-174">这称为*异步对象*和表示将来会通过执行异步方法的实例来完成的工作。</span><span class="sxs-lookup"><span data-stu-id="6f08f-174">This is called an *async object* and represents the future work that will be done by executing the instance of the async method.</span></span> <span data-ttu-id="6f08f-175">当控件继续在此异步方法实例的调用方时，调用方将收到此异步对象作为其调用的结果。</span><span class="sxs-lookup"><span data-stu-id="6f08f-175">When control resumes in the caller of this async method instance, the caller will receive this async object as the result of its invocation.</span></span>
5. <span data-ttu-id="6f08f-176">实例的控点的第一个语句的异步方法正文，然后将设置并立即开始从该处执行方法主体 (部分[块和标签](statements.md#blocks-and-labels))。</span><span class="sxs-lookup"><span data-stu-id="6f08f-176">The instance's control point is then set at the first statement of the async method body, and immediately starts to execute the method body from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="6f08f-177">__恢复委托和当前调用方__</span><span class="sxs-lookup"><span data-stu-id="6f08f-177">__Resumption Delegate and Current Caller__</span></span>

<span data-ttu-id="6f08f-178">部分中所述[Await 运算符](expressions.md#await-operator)，执行`Await`表达式具有能够挂起方法实例的控点离开控制流以转到其他位置。</span><span class="sxs-lookup"><span data-stu-id="6f08f-178">As detailed in Section [Await Operator](expressions.md#await-operator), execution of an `Await` expression has the ability to suspend the method instance's control point leaving control flow to go elsewhere.</span></span> <span data-ttu-id="6f08f-179">控制流可以更高版本继续点通过调用同一实例的控制点*恢复委托*。</span><span class="sxs-lookup"><span data-stu-id="6f08f-179">Control flow can later resume at the same instance's control point through invocation of a *resumption delegate*.</span></span> <span data-ttu-id="6f08f-180">请注意，此挂起不退出异步方法中，便可完成并不会导致最后处理程序执行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-180">Note that this suspension is done without exiting the async method, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="6f08f-181">方法实例仍在引用恢复委托和`Task`或`Task(Of T)`结果 （如果有），或将不被作为垃圾收集，只要存在要委托或结果的实时引用。</span><span class="sxs-lookup"><span data-stu-id="6f08f-181">The method instance is still referenced by both the resumption delegate and the `Task` or `Task(Of T)` result (if any), and will not be garbage collected so long as there exists a live reference to either delegate or result.</span></span>

<span data-ttu-id="6f08f-182">不妨想象语句`Dim x = Await WorkAsync()`大约为以下语法简写形式：</span><span class="sxs-lookup"><span data-stu-id="6f08f-182">It is helpful to imagine the statement `Dim x = Await WorkAsync()` approximately as syntactic shorthand for the following:</span></span>

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

<span data-ttu-id="6f08f-183">在下面的示例，*当前调用方*方法的实例定义为原始调用方或恢复委托，调用方晚。</span><span class="sxs-lookup"><span data-stu-id="6f08f-183">In the following, the *current caller* of the method instance is defined as either the original caller, or the caller of the resumption delegate, whichever is more recent.</span></span>

<span data-ttu-id="6f08f-184">当控制流退出异步方法正文--通过到达`End Sub`或`End Function`标记其结束时，或通过使用显式`Return`或`Exit`语句，或通过未经处理的异常-实例的控点设置为方法的末尾。</span><span class="sxs-lookup"><span data-stu-id="6f08f-184">When control flow exits the async method body -- through reaching the `End Sub` or `End Function` that mark its end, or through an explicit `Return` or `Exit` statement, or through an unhandled exception -- the instance's control point is set to the end of the method.</span></span> <span data-ttu-id="6f08f-185">然后，行为取决于异步方法的返回类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-185">Behavior then depends on the return type of the async method.</span></span>

* <span data-ttu-id="6f08f-186">情况下`Async Function`返回类型`Task`:</span><span class="sxs-lookup"><span data-stu-id="6f08f-186">In the case of an `Async Function` with return type `Task`:</span></span>
  1. <span data-ttu-id="6f08f-187">如果控制流通过未经处理的异常，然后退出异步对象的状态将设置为`TaskStatus.Faulted`并将其`Exception.InnerException`属性设置为异常 (除： 如特定实现定义的异常`OperationCanceledException`将其更改为`TaskStatus.Canceled`).</span><span class="sxs-lookup"><span data-stu-id="6f08f-187">If control flow exits through an unhandled exception, then the async object's status is set to `TaskStatus.Faulted` and its `Exception.InnerException` property is set to the exception (except: certain implementation-defined exceptions such as `OperationCanceledException` change it to `TaskStatus.Canceled`).</span></span> <span data-ttu-id="6f08f-188">在当前调用方中的控制流继续。</span><span class="sxs-lookup"><span data-stu-id="6f08f-188">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="6f08f-189">否则，将异步对象的状态设置为`TaskStatus.Completed`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-189">Otherwise, the async object's status is set to `TaskStatus.Completed`.</span></span> <span data-ttu-id="6f08f-190">在当前调用方中的控制流继续。</span><span class="sxs-lookup"><span data-stu-id="6f08f-190">Control flow resumes in the current caller.</span></span>

     <span data-ttu-id="6f08f-191">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-191">(__Note.__</span></span> <span data-ttu-id="6f08f-192">整点的任务和异步方法的有趣的是，使是一项任务变得时已完成，然后等待它的任何方法目前具有执行其恢复委托，即它们将不再受阻止。）</span><span class="sxs-lookup"><span data-stu-id="6f08f-192">The whole point of Task, and what makes async methods interesting, is that when a task becomes Completed then any methods that were awaiting it will presently have their resumption delegates executed, i.e. they will become unblocked.)</span></span>

* <span data-ttu-id="6f08f-193">情况下`Async Function`返回类型`Task(Of T)`某些`T`： 的行为是作为更高版本，除外，在非异常情况下异步对象`Result`属性也设置为任务返回变量的最终值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-193">In the case of an `Async Function` with return type `Task(Of T)` for some `T`: the behavior is as above, except that in non-exception cases the async object's `Result` property is also set to the final value of the task return variable.</span></span>

* <span data-ttu-id="6f08f-194">情况下`Async Sub`:</span><span class="sxs-lookup"><span data-stu-id="6f08f-194">In the case of an `Async Sub`:</span></span>
  1. <span data-ttu-id="6f08f-195">如果控制流通过未经处理的异常，然后退出该异常会传播到环境中某些特定于实现的方式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-195">If control flow exits through an unhandled exception, then that exception is propagated to the environment in some implementation-specific manner.</span></span> <span data-ttu-id="6f08f-196">在当前调用方中的控制流继续。</span><span class="sxs-lookup"><span data-stu-id="6f08f-196">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="6f08f-197">否则，控制流只需在当前调用方中恢复。</span><span class="sxs-lookup"><span data-stu-id="6f08f-197">Otherwise, control flow simply resumes in the current caller.</span></span>

#### <a name="async-sub"></a><span data-ttu-id="6f08f-198">Async Sub</span><span class="sxs-lookup"><span data-stu-id="6f08f-198">Async Sub</span></span>

<span data-ttu-id="6f08f-199">没有的一些特定于 Microsoft 的行为`Async Sub`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-199">There is some Microsoft-specific behavior of an `Async Sub`.</span></span>

<span data-ttu-id="6f08f-200">如果`SynchronizationContext.Current`是`Nothing`在调用开始时，然后从 Async Sub 任何未经处理的异常将被发布到线程池。</span><span class="sxs-lookup"><span data-stu-id="6f08f-200">If `SynchronizationContext.Current` is `Nothing` at the start of the invocation, then any unhandled exceptions from an Async Sub will be posted to the Threadpool.</span></span>

<span data-ttu-id="6f08f-201">如果`SynchronizationContext.Current`不是`Nothing`开头的调用，然后`OperationStarted()`上的方法的开头之前该上下文调用和`OperationCompleted()`结束后。</span><span class="sxs-lookup"><span data-stu-id="6f08f-201">If `SynchronizationContext.Current` is not `Nothing` at the start of the invocation, then `OperationStarted()` is invoked on that context before the start of the method and `OperationCompleted()` after the end.</span></span> <span data-ttu-id="6f08f-202">此外，任何未经处理的异常将发布的同步上下文上再次引发异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-202">Additionally, any unhandled exceptions will be posted to be rethrown on the synchronization context.</span></span>

<span data-ttu-id="6f08f-203">这意味着，在 UI 应用程序中为`Async Sub`UI 线程上调用，它无法处理任何异常都将重新发布 UI 线程。</span><span class="sxs-lookup"><span data-stu-id="6f08f-203">This means that in UI applications, for an `Async Sub` that is invoked on the UI thread, any exceptions it fails to handle will be reposted the UI thread.</span></span>

#### <a name="mutable-structures-in-async-and-iterator-methods"></a><span data-ttu-id="6f08f-204">Async 或 iterator 方法中的可变结构</span><span class="sxs-lookup"><span data-stu-id="6f08f-204">Mutable structures in async and iterator methods</span></span>

<span data-ttu-id="6f08f-205">结构通常被视为不良实践和 async 或 iterator 方法不支持可变。</span><span class="sxs-lookup"><span data-stu-id="6f08f-205">Mutable structures in general are considered bad practice, and they are not supported by async or iterator methods.</span></span> <span data-ttu-id="6f08f-206">具体而言，每次调用一个结构中的 async 或 iterator 方法将隐式对*复制*在调用其时刻复制该结构。</span><span class="sxs-lookup"><span data-stu-id="6f08f-206">In particular, each invocation of an async or iterator method in a structure will implicitly operate on a *copy* of that structure that is copied at its moment of invocation.</span></span> <span data-ttu-id="6f08f-207">因此，对于示例中，</span><span class="sxs-lookup"><span data-stu-id="6f08f-207">Thus, for example,</span></span>

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

### <a name="blocks-and-labels"></a><span data-ttu-id="6f08f-208">块和标签</span><span class="sxs-lookup"><span data-stu-id="6f08f-208">Blocks and Labels</span></span>

<span data-ttu-id="6f08f-209">可执行语句的一组称为一个语句块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-209">A group of executable statements is called a statement block.</span></span> <span data-ttu-id="6f08f-210">执行语句块的开头的第一个语句块中。</span><span class="sxs-lookup"><span data-stu-id="6f08f-210">Execution of a statement block begins with the first statement in the block.</span></span> <span data-ttu-id="6f08f-211">一旦执行一个语句后，下一步按词汇顺序执行语句，除非语句将执行其他位置，或发生异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-211">Once a statement has been executed, the next statement in lexical order is executed, unless a statement transfers execution elsewhere or an exception occurs.</span></span>

<span data-ttu-id="6f08f-212">在语句块中，在逻辑行的语句的部门并不重要标签声明语句除外。</span><span class="sxs-lookup"><span data-stu-id="6f08f-212">Within a statement block, the division of statements on logical lines is not significant with the exception of label declaration statements.</span></span> <span data-ttu-id="6f08f-213">标签是一个用于标识可以用作分支语句的目标如语句块内的特定位置标识符`GoTo`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-213">A label is an identifier that identifies a particular position within the statement block that can be used as the target of a branch statement such as `GoTo`.</span></span>

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


<span data-ttu-id="6f08f-214">标签声明语句必须位于逻辑行的开头和标签可以是标识符或整数文本。</span><span class="sxs-lookup"><span data-stu-id="6f08f-214">Label declaration statements must appear at the beginning of a logical line and labels may be either an identifier or an integer literal.</span></span> <span data-ttu-id="6f08f-215">标签声明语句和调用语句可以包含单个标识符，因为本地行开头的单个标识符始终被视为标签声明语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-215">Because both label declaration statements and invocation statements can consist of a single identifier, a single identifier at the beginning of a local line is always considered a label declaration statement.</span></span> <span data-ttu-id="6f08f-216">标签声明语句必须始终后跟一个冒号，即使在没有语句遵循同一逻辑线上。</span><span class="sxs-lookup"><span data-stu-id="6f08f-216">Label declaration statements must always be followed by a colon, even if no statements follow on the same logical line.</span></span>

<span data-ttu-id="6f08f-217">标签具有其自己的声明空间并不会干扰其他标识符。</span><span class="sxs-lookup"><span data-stu-id="6f08f-217">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="6f08f-218">下面的示例有效，并使用名称变量`x`作为一个参数和一个标签。</span><span class="sxs-lookup"><span data-stu-id="6f08f-218">The following example is valid and uses the name variable `x` both as a parameter and as a label.</span></span>

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

<span data-ttu-id="6f08f-219">标签的范围是方法的包含它的正文。</span><span class="sxs-lookup"><span data-stu-id="6f08f-219">The scope of a label is the body of the method containing it.</span></span>

<span data-ttu-id="6f08f-220">为了便于阅读，涉及多个 substatements 语句生产即使 substatements 每个可能本身上标记为行作为在此规范中，单个生产处理。</span><span class="sxs-lookup"><span data-stu-id="6f08f-220">For the sake of readability, statement productions that involve multiple substatements are treated as a single production in this specification, even though the substatements may each be by themselves on a labeled line.</span></span>


### <a name="local-variables-and-parameters"></a><span data-ttu-id="6f08f-221">本地变量和参数</span><span class="sxs-lookup"><span data-stu-id="6f08f-221">Local Variables and Parameters</span></span>

<span data-ttu-id="6f08f-222">如何在前面各节的详细信息时创建方法实例，并使用这些方法的局部变量和参数的副本。</span><span class="sxs-lookup"><span data-stu-id="6f08f-222">The preceding sections detail how and when method instances are created, and with them the copies of a method's local variables and parameters.</span></span> <span data-ttu-id="6f08f-223">此外，每次输入循环的正文时，新副本的每个部分中所述，在该循环中声明的局部变量[循环语句](statements.md#loop-statements)，并且方法实例现在包含其本地变量的此副本而不是比以前的复制。</span><span class="sxs-lookup"><span data-stu-id="6f08f-223">In addition, each time the body of a loop is entered, a new copy is made of each local variable declared inside that loop as described in Section [Loop Statements](statements.md#loop-statements), and the method instance now contains this copy of its local variable rather than the previous copy.</span></span>

<span data-ttu-id="6f08f-224">所有局部变量将都初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-224">All locals are initialized to their type's default value.</span></span> <span data-ttu-id="6f08f-225">本地变量和参数始终是可公开访问的。</span><span class="sxs-lookup"><span data-stu-id="6f08f-225">Local variables and parameters are always publicly accessible.</span></span> <span data-ttu-id="6f08f-226">它是错误指其声明前面的文本位置中的本地变量，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="6f08f-226">It is an error to refer to a local variable in a textual position that precedes its declaration, as the following example illustrates:</span></span>

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

<span data-ttu-id="6f08f-227">在中`F`方法更高版本中，第一个分配给`i`一定不是指在外部作用域声明的字段。</span><span class="sxs-lookup"><span data-stu-id="6f08f-227">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="6f08f-228">相反，它是指本地变量，并且因为文本上位于前面的变量声明位于错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-228">Rather, it refers to the local variable, and it is in error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="6f08f-229">在`G`方法中，后续变量声明是指早期变量声明中相同的本地变量声明中声明的本地变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-229">In the `G` method, a subsequent variable declaration refers to a local variable declared in an earlier variable declaration within the same local variable declaration.</span></span>

<span data-ttu-id="6f08f-230">每个块在方法中的创建本地变量声明空间。</span><span class="sxs-lookup"><span data-stu-id="6f08f-230">Each block in a method creates a declaration space for local variables.</span></span> <span data-ttu-id="6f08f-231">名称引入此声明空间，通过在方法体中的本地变量声明和方法，该名称引入的最外层块声明空间方法的参数列表。</span><span class="sxs-lookup"><span data-stu-id="6f08f-231">Names are introduced into this declaration space through local variable declarations in the method body and through the parameter list of the method, which introduces names into the outermost block's declaration space.</span></span> <span data-ttu-id="6f08f-232">块不允许通过嵌套名称的隐藏： 在块中声明一个名称后, 可能不重新声明名称在任何嵌套块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-232">Blocks do not allow shadowing of names through nesting: once a name has been declared in a block, the name may not be redeclared in any nested blocks.</span></span>

<span data-ttu-id="6f08f-233">因此，在以下示例中，`F`并`G`方法是在错误是因为名称`i`在外部块中声明并不能在内部块中重新声明。</span><span class="sxs-lookup"><span data-stu-id="6f08f-233">Thus, in the following example, the `F` and `G` methods are in error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="6f08f-234">但是，`H`并`I`有效方法是因为这两个`i`的在单独的非嵌套块中声明。</span><span class="sxs-lookup"><span data-stu-id="6f08f-234">However, the `H` and `I` methods are valid because the two `i`'s are declared in separate non-nested blocks.</span></span>

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

<span data-ttu-id="6f08f-235">函数方法时，一个特殊的本地变量是隐式声明与表示该函数的返回值的方法同名的方法主体的声明空间中。</span><span class="sxs-lookup"><span data-stu-id="6f08f-235">When the method is a function, a special local variable is implicitly declared in the method body's declaration space with the same name as the method representing the return value of the function.</span></span> <span data-ttu-id="6f08f-236">本地变量具有特殊名称解析语义时在表达式中使用。</span><span class="sxs-lookup"><span data-stu-id="6f08f-236">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="6f08f-237">如果在应被归类为方法组，如调用表达式，为表达式的上下文中使用本地变量，名称解析，该函数，而不是本地变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-237">If the local variable is used in a context that expects an expression classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="6f08f-238">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-238">For example:</span></span>

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

<span data-ttu-id="6f08f-239">使用括号可能会导致不明确的情况下 (如`F(1)`，其中`F`是其返回类型是一个一维数组的函数); 在所有不明确的情况下，此名称解析为该函数而不是本地变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-239">The use of parentheses can cause ambiguous situations (such as `F(1)`, where `F` is a function whose return type is a one-dimensional array); in all ambiguous situations, the name resolves to the function rather than the local variable.</span></span> <span data-ttu-id="6f08f-240">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-240">For example:</span></span>

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

<span data-ttu-id="6f08f-241">当控制流离开方法主体时，本地变量的值被传递回调用表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-241">When control flow leaves the method body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="6f08f-242">如果该方法是一个子例程，没有任何此类隐式局部变量，并且控件只是返回到调用表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-242">If the method is a subroutine, there is no such implicit local variable, and control simply returns to the invocation expression.</span></span>

## <a name="local-declaration-statements"></a><span data-ttu-id="6f08f-243">局部声明语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-243">Local Declaration Statements</span></span>

<span data-ttu-id="6f08f-244">局部声明语句声明新的本地变量、 局部常量或静态变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-244">A local declaration statement declares a new local variable, local constant, or static variable.</span></span> <span data-ttu-id="6f08f-245">*本地变量*并*局部常量*相当于实例变量和常量作用域为该方法和声明相同的方式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-245">*Local variables* and *local constants* are equivalent to instance variables and constants scoped to the method and are declared in the same way.</span></span> <span data-ttu-id="6f08f-246">*静态变量*类似于`Shared`是声明的变量和使用`Static`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f08f-246">*Static variables* are similar to `Shared` variables and are declared using the `Static` modifier.</span></span>

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

<span data-ttu-id="6f08f-247">静态变量是在方法的调用之间保留其值的局部变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-247">Static variables are locals that retain their value across invocations of the method.</span></span> <span data-ttu-id="6f08f-248">非共享方法中声明的静态变量是每个实例： 包含方法的类型的每个实例都有其自己的静态变量副本。</span><span class="sxs-lookup"><span data-stu-id="6f08f-248">Static variables declared within non-shared methods are per instance: each instance of the type that contains the method has its own copy of the static variable.</span></span> <span data-ttu-id="6f08f-249">中声明的静态变量`Shared`方法是每种类型的; 只有一个副本的所有实例的静态变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-249">Static variables declared within `Shared` methods are per type; there is only one copy of the static variable for all instances.</span></span> <span data-ttu-id="6f08f-250">虽然本地变量初始化为其类型的默认值为该方法在每个输入时，静态变量初始化的类型或类型实例时仅初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-250">While local variables are initialized to their type's default value upon each entry into the method, static variables are only initialized to their type's default value when the type or type instance is initialized.</span></span> <span data-ttu-id="6f08f-251">不能在结构或泛型方法中声明静态变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-251">Static variables may not be declared in structures or generic methods.</span></span>

<span data-ttu-id="6f08f-252">本地变量、 局部常量和静态变量始终具有公共可访问性和不能指定可访问性修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f08f-252">Local variables, local constants, and static variables always have public accessibility and may not specify accessibility modifiers.</span></span> <span data-ttu-id="6f08f-253">如果在局部声明语句上不指定任何类型，以下步骤确定本地声明的类型：</span><span class="sxs-lookup"><span data-stu-id="6f08f-253">If no type is specified on a local declaration statement, then the following steps determine the type of the local declaration:</span></span>

1. <span data-ttu-id="6f08f-254">如果声明具有类型字符，字符类型的类型是局部声明的类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-254">If the declaration has a type character, the type of the type character is the type of the local declaration.</span></span>

2. <span data-ttu-id="6f08f-255">如果本地声明的局部常量，或如果局部声明使用初始值设定项的本地变量以及正在使用本地变量的类型推理，从初始值设定项的类型推断局部声明的类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-255">If the local declaration is a local constant, or if the local declaration is a local variable with an initializer and local variable type inference is being used, the type of the local declaration is inferred from the type of the initializer.</span></span> <span data-ttu-id="6f08f-256">如果初始值设定项所引用的局部声明，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-256">If the initializer refers to the local declaration, a compile-time error occurs.</span></span> <span data-ttu-id="6f08f-257">（局部常量都需要有初始值设定项。）</span><span class="sxs-lookup"><span data-stu-id="6f08f-257">(Local constants are required to have initializers.)</span></span>

3. <span data-ttu-id="6f08f-258">如果未使用严格的语义，局部声明语句的类型是隐式`Object`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-258">If strict semantics are not being used, the type of the local declaration statement is implicitly `Object`.</span></span>

4. <span data-ttu-id="6f08f-259">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-259">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="6f08f-260">如果未指定类型上有数组大小或数组的类型修饰符的局部声明语句，然后本地声明的类型是具有指定秩的数组和前面的步骤用于确定数组的元素类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-260">If no type is specified on a local declaration statement that has an array size or array type modifier, then the type of the local declaration is an array with the specified rank and the previous steps are used to determine the element type of the array.</span></span> <span data-ttu-id="6f08f-261">如果使用本地变量的类型推理，则初始值设定项的类型必须具有与局部声明语句在同一数组形状 （即数组的类型修饰符） 的数组类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-261">If local variable type inference is used, the type of the initializer must be an array type with the same array shape (i.e. array type modifiers) as the local declaration statement.</span></span> <span data-ttu-id="6f08f-262">请注意，可能的推断的元素类型可能仍是数组类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-262">Note that it is possible that the inferred element type may still be an array type.</span></span> <span data-ttu-id="6f08f-263">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-263">For example:</span></span>

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

<span data-ttu-id="6f08f-264">如果未指定类型的局部声明语句上具有可以为 null 的类型修饰符，则本地声明的类型是推断出的类型或推断的类型本身可以为 null 的版本，如果它可以为 null 的值类型已中。</span><span class="sxs-lookup"><span data-stu-id="6f08f-264">If no type is specified on a local declaration statement that has a nullable type modifier, then the type of the local declaration is the nullable version of the inferred type or the inferred type itself if it a nullable value type already.</span></span>  <span data-ttu-id="6f08f-265">如果推断出的类型不是可为 null 的值类型，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-265">If the inferred type is not a value type that can be made nullable, a compile-time error occurs.</span></span> <span data-ttu-id="6f08f-266">如果为 null 的类型修饰符和数组大小或数组类型修饰符放置在具有任何类型的局部声明语句中，然后为 null 的类型修饰符视为要应用于数组的元素类型，并使用前面的步骤来确定元素nt 类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-266">If both a nullable type modifier and an array size or array type modifier are placed on a local declaration statement with no type, then the nullable type modifier is considered to apply to the element type of the array and the previous steps are used to determine the element type.</span></span>

<span data-ttu-id="6f08f-267">变量初始值设定项在局部声明语句上的是等效于赋值语句放在声明的文本的位置。</span><span class="sxs-lookup"><span data-stu-id="6f08f-267">Variable initializers on local declaration statements are equivalent to assignment statements placed at the textual location of the declaration.</span></span> <span data-ttu-id="6f08f-268">因此，如果执行分支过程局部声明语句中，变量的初始值设定项不会执行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-268">Thus, if execution branches over the local declaration statement, the variable initializer is not executed.</span></span> <span data-ttu-id="6f08f-269">如果超过一次执行局部声明语句，则执行变量初始值设定项相同数目的时间。</span><span class="sxs-lookup"><span data-stu-id="6f08f-269">If the local declaration statement is executed more than once, the variable initializer is executed an equal number of times.</span></span> <span data-ttu-id="6f08f-270">静态变量仅执行其初始值设定项第一次。</span><span class="sxs-lookup"><span data-stu-id="6f08f-270">Static variables only execute their initializer the first time.</span></span> <span data-ttu-id="6f08f-271">如果初始化静态变量时出现异常，静态变量被视为使用静态变量的类型的默认值初始化。</span><span class="sxs-lookup"><span data-stu-id="6f08f-271">If an exception occurs while initializing a static variable, the static variable is considered initialized with the default value of the static variable's type.</span></span>

<span data-ttu-id="6f08f-272">下面的示例演示如何使用初始值设定项：</span><span class="sxs-lookup"><span data-stu-id="6f08f-272">The following example shows the use of initializers:</span></span>

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

<span data-ttu-id="6f08f-273">此程序将打印：</span><span class="sxs-lookup"><span data-stu-id="6f08f-273">This program prints:</span></span>

```
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

<span data-ttu-id="6f08f-274">静态局部变量初始值设定项初始化过程是线程安全和异常对受保护。</span><span class="sxs-lookup"><span data-stu-id="6f08f-274">Initializers on static locals are thread-safe and protected against exceptions during initialization.</span></span> <span data-ttu-id="6f08f-275">如果在本地的静态初始值设定项期间会发生异常，静态本地版本将保持其默认值和未初始化。</span><span class="sxs-lookup"><span data-stu-id="6f08f-275">If an exception occurs during a static local initializer, the static local will have its default value and not be initialized.</span></span> <span data-ttu-id="6f08f-276">静态本地的初始值设定项</span><span class="sxs-lookup"><span data-stu-id="6f08f-276">A static local initializer</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

<span data-ttu-id="6f08f-277">等效于</span><span class="sxs-lookup"><span data-stu-id="6f08f-277">is equivalent to</span></span>

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

<span data-ttu-id="6f08f-278">本地变量、 局部常量和静态变量的作用域为该语句块中声明它们。</span><span class="sxs-lookup"><span data-stu-id="6f08f-278">Local variables, local constants, and static variables are scoped to the statement block in which they are declared.</span></span> <span data-ttu-id="6f08f-279">静态变量的特殊在于它们的名称只能使用一次在整个方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-279">Static variables are special in that their names may only be used once throughout the entire method.</span></span> <span data-ttu-id="6f08f-280">例如，不能用来指定具有相同名称的两个静态变量声明，即使它们处于不同的块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-280">For example, it is not valid to specify two static variable declarations with the same name even if they are in different blocks.</span></span>


### <a name="implicit-local-declarations"></a><span data-ttu-id="6f08f-281">隐式的局部声明</span><span class="sxs-lookup"><span data-stu-id="6f08f-281">Implicit Local Declarations</span></span>

<span data-ttu-id="6f08f-282">除了局部声明语句、 本地变量也可以声明隐式使用。</span><span class="sxs-lookup"><span data-stu-id="6f08f-282">In addition to local declaration statements, local variables can also be declared implicitly through use.</span></span> <span data-ttu-id="6f08f-283">使用的名称，不能解决其他事项的简单名称表达式声明该名称的本地变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-283">A simple name expression that uses a name that does not resolve to something else declares a local variable by that name.</span></span> <span data-ttu-id="6f08f-284">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-284">For example:</span></span>

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

<span data-ttu-id="6f08f-285">在可以接受表达式分类为变量的表达式上下文中才会发生隐式的局部声明。</span><span class="sxs-lookup"><span data-stu-id="6f08f-285">Implicit local declaration only occurs in expression contexts that can accept an expression classified as a variable.</span></span> <span data-ttu-id="6f08f-286">此规则的例外是，本地变量不能隐式声明的函数调用表达式、 索引表达式或成员访问表达式目标时。</span><span class="sxs-lookup"><span data-stu-id="6f08f-286">The exception to this rule is that a local variable may not be implicitly declared when it is the target of a function invocation expression, indexing expression, or a member access expression.</span></span>

<span data-ttu-id="6f08f-287">隐式局部变量将被视为包含方法的开头声明它们。</span><span class="sxs-lookup"><span data-stu-id="6f08f-287">Implicit locals are treated as if they are declared at the beginning of the containing method.</span></span> <span data-ttu-id="6f08f-288">因此，它们始终作用于整个方法体中，即使内部 lambda 表达式声明。</span><span class="sxs-lookup"><span data-stu-id="6f08f-288">Thus, they are always scoped to the entire method body, even if declared inside of a lambda expression.</span></span> <span data-ttu-id="6f08f-289">例如，以下代码：</span><span class="sxs-lookup"><span data-stu-id="6f08f-289">For example, the following code:</span></span>

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

<span data-ttu-id="6f08f-290">将打印的值`10`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-290">will print the value `10`.</span></span> <span data-ttu-id="6f08f-291">隐式局部变量被类型化为`Object`如果没有类型字符已附加到的变量名称; 否则变量的类型是字符类型的类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-291">Implicit locals are typed as `Object` if no type character was attached to the variable name; otherwise the type of the variable is the type of the type character.</span></span> <span data-ttu-id="6f08f-292">本地变量的类型推断不用于隐式局部变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-292">Local variable type inference is not used for implicit locals.</span></span>

<span data-ttu-id="6f08f-293">如果通过编译环境或指定了显式的局部声明`Option Explicit`、 必须显式声明所有本地变量和不允许隐式变量声明。</span><span class="sxs-lookup"><span data-stu-id="6f08f-293">If explicit local declaration is specified by the compilation environment or by `Option Explicit`, all local variables must be explicitly declared and implicit variable declaration is disallowed.</span></span>

## <a name="with-statement"></a><span data-ttu-id="6f08f-294">与语句一起使用</span><span class="sxs-lookup"><span data-stu-id="6f08f-294">With Statement</span></span>

<span data-ttu-id="6f08f-295">一个`With`语句，而无需指定多个时间的表达式的表达式的成员对多个引用。</span><span class="sxs-lookup"><span data-stu-id="6f08f-295">A `With` statement allows multiple references to an expression's members without specifying the expression multiple times.</span></span>

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

<span data-ttu-id="6f08f-296">表达式必须分类为一个值，计算一次，在进入块时。</span><span class="sxs-lookup"><span data-stu-id="6f08f-296">The expression must be classified as a value and is evaluated once, upon entry into the block.</span></span> <span data-ttu-id="6f08f-297">内`With`语句块、 成员访问表达式或开始一段或感叹号字典访问表达式进行计算像`With`它前面有表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-297">Within the `With` statement block, a member access expression or dictionary access expression starting with a period or an exclamation point is evaluated as if the `With` expression preceded it.</span></span> <span data-ttu-id="6f08f-298">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-298">For example:</span></span>

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

<span data-ttu-id="6f08f-299">它是无效的分支到`With`从块之外的语句块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-299">It is invalid to branch into a `With` statement block from outside of the block.</span></span>


## <a name="synclock-statement"></a><span data-ttu-id="6f08f-300">SyncLock 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-300">SyncLock Statement</span></span>

<span data-ttu-id="6f08f-301">一个`SyncLock`语句允许在表达式中，确保多个执行线程不会在同一时间中执行相同的语句上同步语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-301">A `SyncLock` statement allows statements to be synchronized on an expression, which ensures that multiple threads of execution do not execute the same statements at the same time.</span></span>

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

<span data-ttu-id="6f08f-302">表达式必须分类为一个值，计算一次，进入块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-302">The expression must be classified as a value and is evaluated once, upon entry to the block.</span></span> <span data-ttu-id="6f08f-303">输入时`SyncLock`块中，`Shared`方法`System.Threading.Monitor.Enter`上指定的表达式之前执行的线程具有由表达式返回的对象上的排他锁会阻止调用。</span><span class="sxs-lookup"><span data-stu-id="6f08f-303">When entering the `SyncLock` block, the `Shared` method `System.Threading.Monitor.Enter` is called on the specified expression, which blocks until the thread of execution has an exclusive lock on the object returned by the expression.</span></span> <span data-ttu-id="6f08f-304">中的表达式的类型`SyncLock`语句必须是引用类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-304">The type of the expression in a `SyncLock` statement must be a reference type.</span></span> <span data-ttu-id="6f08f-305">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-305">For example:</span></span>

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

<span data-ttu-id="6f08f-306">上面的示例中同步特定类的实例上`Test`以确保不超过一个执行线程可以添加或减去计数变量中的特定实例一次。</span><span class="sxs-lookup"><span data-stu-id="6f08f-306">The example above synchronizes on the specific instance of the class `Test` to ensure that no more than one thread of execution can add or subtract from the count variable at a time for a particular instance.</span></span>

<span data-ttu-id="6f08f-307">`SyncLock`隐式包含块`Try`语句的`Finally`块调用`Shared`方法`System.Threading.Monitor.Exit`的表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-307">The `SyncLock` block is implicitly contained by a `Try` statement whose `Finally` block calls the `Shared` method `System.Threading.Monitor.Exit` on the expression.</span></span> <span data-ttu-id="6f08f-308">这可确保即使在引发异常时，才会释放锁。</span><span class="sxs-lookup"><span data-stu-id="6f08f-308">This ensures the lock is freed even when an exception is thrown.</span></span> <span data-ttu-id="6f08f-309">结果是，它是无效的分支到`SyncLock`阻止其块之外和一个`SyncLock`块均被视为单个语句出于`Resume`和`Resume Next`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-309">As a result, it is invalid to branch into a `SyncLock` block from outside of the block, and a `SyncLock` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="6f08f-310">上面的示例等同于以下代码：</span><span class="sxs-lookup"><span data-stu-id="6f08f-310">The above example is equivalent to the following code:</span></span>

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


## <a name="event-statements"></a><span data-ttu-id="6f08f-311">Event 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-311">Event Statements</span></span>

<span data-ttu-id="6f08f-312">`RaiseEvent`， `AddHandler`，和`RemoveHandler`语句引发事件和动态处理事件。</span><span class="sxs-lookup"><span data-stu-id="6f08f-312">The `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements raise events and handle events dynamically.</span></span>

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a><span data-ttu-id="6f08f-313">RaiseEvent 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-313">RaiseEvent Statement</span></span>

<span data-ttu-id="6f08f-314">一个`RaiseEvent`语句会通知已发生的特定事件的事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="6f08f-314">A `RaiseEvent` statement notifies event handlers that a particular event has occurred.</span></span>

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

<span data-ttu-id="6f08f-315">中的简单名称表达式`RaiseEvent`语句上被解释为成员查找`Me`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-315">The simple name expression in a `RaiseEvent` statement is interpreted as a member lookup on `Me`.</span></span> <span data-ttu-id="6f08f-316">因此， `RaiseEvent x` ，就好像解释`RaiseEvent Me.x`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-316">Thus, `RaiseEvent x` is interpreted as if it were `RaiseEvent Me.x`.</span></span> <span data-ttu-id="6f08f-317">该表达式的结果必须属于事件访问类本身; 中定义的事件不能用于对基类型中定义的事件`RaiseEvent`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-317">The result of the expression must be classified as an event access for an event defined in the class itself; events defined on base types cannot be used in a `RaiseEvent` statement.</span></span>

<span data-ttu-id="6f08f-318">`RaiseEvent`语句处理与调用`Invoke`的事件的委托，如果有使用所提供的参数的方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-318">The `RaiseEvent` statement is processed as a call to the `Invoke` method of the event's delegate, using the supplied parameters, if any.</span></span> <span data-ttu-id="6f08f-319">如果该委托的值为`Nothing`，不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-319">If the delegate's value is `Nothing`, no exception is thrown.</span></span> <span data-ttu-id="6f08f-320">如果没有任何自变量，则可省略括号。</span><span class="sxs-lookup"><span data-stu-id="6f08f-320">If there are no arguments, the parentheses may be omitted.</span></span> <span data-ttu-id="6f08f-321">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-321">For example:</span></span>

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

<span data-ttu-id="6f08f-322">类`Raiser`更高版本相当于：</span><span class="sxs-lookup"><span data-stu-id="6f08f-322">The class `Raiser` above is equivalent to:</span></span>

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


### <a name="addhandler-and-removehandler-statements"></a><span data-ttu-id="6f08f-323">AddHandler 和 RemoveHandler 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-323">AddHandler and RemoveHandler Statements</span></span>

<span data-ttu-id="6f08f-324">尽管大多数事件处理程序将自动挂钩通过`WithEvents`变量，它可能需要动态添加和移除事件处理程序在运行时。</span><span class="sxs-lookup"><span data-stu-id="6f08f-324">Although most event handlers are automatically hooked up through `WithEvents` variables, it may be necessary to dynamically add and remove event handlers at run time.</span></span> <span data-ttu-id="6f08f-325">`AddHandler` 和`RemoveHandler`语句执行此操作。</span><span class="sxs-lookup"><span data-stu-id="6f08f-325">`AddHandler` and `RemoveHandler` statements do this.</span></span>

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

<span data-ttu-id="6f08f-326">每个语句采用两个参数： 第一个参数必须属于事件访问的表达式和第二个参数必须是分类为一个值的表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-326">Each statement takes two arguments: the first argument must be an expression that is classified as an event access and the second argument must be an expression that is classified as a value.</span></span> <span data-ttu-id="6f08f-327">第二个参数的类型必须是与事件访问关联的委托类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-327">The second argument's type must be the delegate type associated with the event access.</span></span> <span data-ttu-id="6f08f-328">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-328">For example:</span></span>

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

<span data-ttu-id="6f08f-329">给定事件`E,`语句调用相关`add_E`或`remove_E`上要添加或删除为该事件处理程序委托的实例方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-329">Given an event `E,` the statement calls the relevant `add_E` or `remove_E` method on the instance to add or remove the delegate as a handler for the event.</span></span> <span data-ttu-id="6f08f-330">因此，上面的代码等效于：</span><span class="sxs-lookup"><span data-stu-id="6f08f-330">Thus, the above code is equivalent to:</span></span>

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


## <a name="assignment-statements"></a><span data-ttu-id="6f08f-331">赋值语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-331">Assignment Statements</span></span>

<span data-ttu-id="6f08f-332">赋值语句将表达式的值分配给一个变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-332">An assignment statement assigns the value of an expression to a variable.</span></span> <span data-ttu-id="6f08f-333">有几种类型的分配。</span><span class="sxs-lookup"><span data-stu-id="6f08f-333">There are several types of assignment.</span></span>

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a><span data-ttu-id="6f08f-334">常规赋值语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-334">Regular Assignment Statements</span></span>

<span data-ttu-id="6f08f-335">简单的赋值语句将表达式的结果存储在变量中。</span><span class="sxs-lookup"><span data-stu-id="6f08f-335">A simple assignment statement stores the result of an expression in a variable.</span></span>

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="6f08f-336">赋值运算符右侧的表达式必须分类为一个值时，必须为变量或属性访问类别的赋值运算符左侧和右侧表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-336">The expression on the left side of the assignment operator must be classified as a variable or a property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="6f08f-337">表达式的类型必须是隐式转换为的变量或属性访问类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-337">The type of the expression must be implicitly convertible to the type of the variable or property access.</span></span>

<span data-ttu-id="6f08f-338">如果分配到的变量是引用类型的数组元素，将执行运行时检查以确保表达式与数组元素的类型兼容。</span><span class="sxs-lookup"><span data-stu-id="6f08f-338">If the variable being assigned into is an array element of a reference type, a run-time check will be performed to ensure that the expression is compatible with the array-element type.</span></span> <span data-ttu-id="6f08f-339">在以下示例中，最后一个分配会导致`System.ArrayTypeMismatchException`引发，因为实例`ArrayList`不能存储在一个元素的`String`数组。</span><span class="sxs-lookup"><span data-stu-id="6f08f-339">In the following example, the last assignment causes a `System.ArrayTypeMismatchException` to be thrown, because an instance of `ArrayList` cannot be stored in an element of a `String` array.</span></span>

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

<span data-ttu-id="6f08f-340">如果左侧和右侧的赋值运算符的表达式分类为变量，在赋值语句对变量中存储的值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-340">If the expression on the left side of the assignment operator is classified as a variable, then the assignment statement stores the value in the variable.</span></span> <span data-ttu-id="6f08f-341">如果表达式分类为属性访问，则在赋值语句将属性访问的调用转换`Set`访问器的属性的值替换为参数的值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-341">If the expression is classified as a property access, then the assignment statement turns the property access into an invocation of the `Set` accessor of the property with the value substituted for the value parameter.</span></span> <span data-ttu-id="6f08f-342">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-342">For example:</span></span>

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

<span data-ttu-id="6f08f-343">如果变量或属性访问的目标类型为值类型，但未归类为变量，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-343">If the target of the variable or property access is typed as a value type but not classified as a variable, a compile-time error occurs.</span></span> <span data-ttu-id="6f08f-344">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-344">For example:</span></span>

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

<span data-ttu-id="6f08f-345">请注意分配的语义依赖于类型的变量或正在分配的属性。</span><span class="sxs-lookup"><span data-stu-id="6f08f-345">Note that the semantics of the assignment depend on the type of the variable or property to which it is being assigned.</span></span> <span data-ttu-id="6f08f-346">如果被赋值的变量是值类型，分配将表达式的值复制到该变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-346">If the variable to which it is being assigned is a value type, the assignment copies the value of the expression into the variable.</span></span> <span data-ttu-id="6f08f-347">如果被赋值的变量为引用类型，分配将的参考，不是值本身，复制到该变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-347">If the variable to which it is being assigned is a reference type, the assignment copies the reference, not the value itself, into the variable.</span></span> <span data-ttu-id="6f08f-348">如果变量的类型为`Object`，由赋值语义值的类型在运行时是否是值类型或引用类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-348">If the type of the variable is `Object`, the assignment semantics are determined by whether the value's type is a value type or a reference type at run time.</span></span>


<span data-ttu-id="6f08f-349">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-349">__Note.__</span></span> <span data-ttu-id="6f08f-350">对于内部类型，如`Integer`和`Date`，引用和值分配语义是相同，因为类型是固定不变。</span><span class="sxs-lookup"><span data-stu-id="6f08f-350">For intrinsic types such as `Integer` and `Date`, reference and value assignment semantics are the same because the types are immutable.</span></span> <span data-ttu-id="6f08f-351">因此，语言是随意装箱的内部类型上使用引用赋值，作为一种优化。</span><span class="sxs-lookup"><span data-stu-id="6f08f-351">As a result, the language is free to use reference assignment on boxed intrinsic types as an optimization.</span></span> <span data-ttu-id="6f08f-352">从值的角度，结果是相同的。</span><span class="sxs-lookup"><span data-stu-id="6f08f-352">From a value perspective, the result is the same.</span></span>

<span data-ttu-id="6f08f-353">因为等号字符 (`=`) 用于分配和为确定相等性，没有简单的赋值和调用语句中的情况下之间产生歧义如`x = y.ToString()`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-353">Because the equals character (`=`) is used both for assignment and for equality, there is an ambiguity between a simple assignment and an invocation statement in situations such as `x = y.ToString()`.</span></span> <span data-ttu-id="6f08f-354">在这种情况下，在赋值语句将优先级高于相等运算符。</span><span class="sxs-lookup"><span data-stu-id="6f08f-354">In all such cases, the assignment statement takes precedence over the equality operator.</span></span> <span data-ttu-id="6f08f-355">这意味着该示例表达式被视为`x = (y.ToString())`而非`(x = y).ToString()`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-355">This means that the example expression is interpreted as `x = (y.ToString())` rather than `(x = y).ToString()`.</span></span>


### <a name="compound-assignment-statements"></a><span data-ttu-id="6f08f-356">复合赋值语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-356">Compound Assignment Statements</span></span>

<span data-ttu-id="6f08f-357">一个*复合赋值语句*采用形式`V op= E`(其中`op`是一个有效的二进制运算符)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-357">A *compound assignment statement* takes the form `V op= E` (where `op` is a valid binary operator).</span></span>

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

<span data-ttu-id="6f08f-358">赋值运算符左侧和右侧的表达式必须分类为变量或属性的访问权限，而赋值运算符右侧的表达式必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-358">The expression on the left side of the assignment operator must be classified as a variable or property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="6f08f-359">复合赋值语句是等效于语句`V = V op E`二者的区别，复合赋值运算符左侧的变量只计算一次。</span><span class="sxs-lookup"><span data-stu-id="6f08f-359">The compound assignment statement is equivalent to the statement `V = V op E` with the difference that the variable on the left side of the compound assignment operator is only evaluated once.</span></span> <span data-ttu-id="6f08f-360">以下示例演示了这一差异：</span><span class="sxs-lookup"><span data-stu-id="6f08f-360">The following example demonstrates this difference:</span></span>

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

<span data-ttu-id="6f08f-361">表达式`a(GetIndex())`计算两次以简单的赋值，但仅一的复合赋值，因此该代码会输出：</span><span class="sxs-lookup"><span data-stu-id="6f08f-361">The expression `a(GetIndex())` is evaluated twice for simple assignment but only once for compound assignment, so the code prints:</span></span>

```
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a><span data-ttu-id="6f08f-362">Mid 赋值语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-362">Mid Assignment Statement</span></span>

<span data-ttu-id="6f08f-363">一个`Mid`赋值语句将字符串分配到另一个字符串。</span><span class="sxs-lookup"><span data-stu-id="6f08f-363">A `Mid` assignment statement assigns a string into another string.</span></span> <span data-ttu-id="6f08f-364">分配的左侧和右侧具有相同的语法与函数调用`Microsoft.VisualBasic.Strings.Mid`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-364">The left side of the assignment has the same syntax as a call to the function `Microsoft.VisualBasic.Strings.Mid`.</span></span>

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="6f08f-365">第一个参数是赋值的目标，必须分类为变量或属性访问的类型是隐式转换到和从`String`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-365">The first argument is the target of the assignment and must be classified as a variable or a property access whose type is implicitly convertible to and from `String`.</span></span> <span data-ttu-id="6f08f-366">第二个参数是对应于分配的应在目标字符串起始位置和必须分类为其类型必须可隐式转换为一个值从 1 开始的起始位置`Integer`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-366">The second parameter is the 1-based start position that corresponds to where the assignment should begin in the target string and must be classified as a value whose type must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="6f08f-367">可选的第三个参数是从右侧的字符数要将分配到目标字符串的值，必须被归类为值的类型隐式转换为`Integer`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-367">The optional third parameter is the number of characters from the right-side value to assign into the target string and must be classified as a value whose type is implicitly convertible to `Integer`.</span></span> <span data-ttu-id="6f08f-368">右侧是源字符串，必须被归类为值的类型隐式转换为`String`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-368">The right side is the source string and must be classified as a value whose type is implicitly convertible to `String`.</span></span> <span data-ttu-id="6f08f-369">右侧的数字将被截断为长度参数，如果指定，并替换在左侧的字符串中，从开始位置开始的字符。</span><span class="sxs-lookup"><span data-stu-id="6f08f-369">The right side is truncated to the length parameter, if specified, and replaces the characters in the left-side string, starting at the start position.</span></span> <span data-ttu-id="6f08f-370">如果右侧字符串包含更少字符数的第三个参数，仅从右侧的字符串的字符将被复制。</span><span class="sxs-lookup"><span data-stu-id="6f08f-370">If the right side string contained fewer characters than the third parameter, only the characters from the right side string will be copied.</span></span>

<span data-ttu-id="6f08f-371">下面的示例显示`ab123fg`:</span><span class="sxs-lookup"><span data-stu-id="6f08f-371">The following example displays `ab123fg`:</span></span>

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

<span data-ttu-id="6f08f-372">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-372">__Note.__</span></span> <span data-ttu-id="6f08f-373">`Mid` 不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="6f08f-373">`Mid` is not a reserved word.</span></span>


## <a name="invocation-statements"></a><span data-ttu-id="6f08f-374">调用语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-374">Invocation Statements</span></span>

<span data-ttu-id="6f08f-375">调用语句调用的方法前面加上可选关键字`Call`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-375">An invocation statement invokes a method preceded by the optional keyword `Call`.</span></span> <span data-ttu-id="6f08f-376">有一些区别，如下所示情况下，调用语句处理函数调用表达式的方式相同。</span><span class="sxs-lookup"><span data-stu-id="6f08f-376">The invocation statement is processed in the same way as the function invocation expression, with some differences noted below.</span></span> <span data-ttu-id="6f08f-377">调用表达式必须分类，作为一个值或 void。</span><span class="sxs-lookup"><span data-stu-id="6f08f-377">The invocation expression must be classified as a value or void.</span></span> <span data-ttu-id="6f08f-378">调用表达式计算得到的任何值将被放弃。</span><span class="sxs-lookup"><span data-stu-id="6f08f-378">Any value resulting from the evaluation of the invocation expression is discarded.</span></span>

<span data-ttu-id="6f08f-379">如果`Call`省略了关键字，则调用表达式必须启动与标识符或关键字，或使用`.`内`With`块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-379">If the `Call` keyword is omitted, then the invocation expression must start with an identifier or keyword, or with `.` inside a `With` block.</span></span> <span data-ttu-id="6f08f-380">因此，例如，"`Call 1.ToString()`"是有效的语句，但"`1.ToString()`"不是。</span><span class="sxs-lookup"><span data-stu-id="6f08f-380">Thus, for instance, "`Call 1.ToString()`" is a valid statement but "`1.ToString()`" is not.</span></span> <span data-ttu-id="6f08f-381">（请注意，在表达式上下文中，调用表达式还需要启动标识符。</span><span class="sxs-lookup"><span data-stu-id="6f08f-381">(Note that in an expression context, invocation expressions also need not start with an identifier.</span></span> <span data-ttu-id="6f08f-382">例如，"`Dim x = 1.ToString()`"是有效的语句)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-382">For example, "`Dim x = 1.ToString()`" is a valid statement).</span></span>

<span data-ttu-id="6f08f-383">没有调用语句和调用表达式的另一个区别： 如果调用语句包括参数列表，则始终被视为调用的参数列表。</span><span class="sxs-lookup"><span data-stu-id="6f08f-383">There is another difference between the invocation statements and invocation expressions: if an invocation statement includes an argument list, then this is always taken as the argument list of the invocation.</span></span> <span data-ttu-id="6f08f-384">下面的示例阐释了差异：</span><span class="sxs-lookup"><span data-stu-id="6f08f-384">The following example illustrates the difference:</span></span>

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

## <a name="conditional-statements"></a><span data-ttu-id="6f08f-385">条件语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-385">Conditional Statements</span></span>

<span data-ttu-id="6f08f-386">条件语句允许基于在运行时计算的表达式的语句的条件执行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-386">Conditional statements allow conditional execution of statements based on expressions evaluated at run time.</span></span>

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a><span data-ttu-id="6f08f-387">如果...然后...Else 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-387">If...Then...Else Statements</span></span>

<span data-ttu-id="6f08f-388">`If...Then...Else`语句是基本的条件语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-388">An `If...Then...Else` statement is the basic conditional statement.</span></span>

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

<span data-ttu-id="6f08f-389">在每个表达式`If...Then...Else`语句必须是布尔表达式，部分所述[布尔表达式](expressions.md#boolean-expressions)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-389">Each expression in an `If...Then...Else` statement must be a Boolean expression, as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="6f08f-390">(注意： 这不需要具有布尔类型的表达式)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-390">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="6f08f-391">如果中的表达式`If`语句为 true，这些语句括`If`块执行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-391">If the expression in the `If` statement is true, the statements enclosed by the `If` block are executed.</span></span> <span data-ttu-id="6f08f-392">如果表达式为 false，每个`ElseIf`计算表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-392">If the expression is false, each of the `ElseIf` expressions is evaluated.</span></span> <span data-ttu-id="6f08f-393">如果一个的`ElseIf`表达式的计算结果为 true，则执行相应的块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-393">If one of the `ElseIf` expressions evaluates to true, the corresponding block is executed.</span></span> <span data-ttu-id="6f08f-394">如果任何表达式的计算结果为 true，并且没有`Else`块中，`Else`执行块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-394">If no expression evaluates to true and there is an `Else` block, the `Else` block is executed.</span></span> <span data-ttu-id="6f08f-395">完成一个块执行后，执行将传递到末尾`If...Then...Else`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-395">Once a block finishes executing, execution passes to the end of the `If...Then...Else` statement.</span></span>

<span data-ttu-id="6f08f-396">行版本`If`语句具有语句时要执行的一组`If`表达式是`True`和一组可选如果表达式是执行语句`False`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-396">The line version of the `If` statement has a single set of statements to be executed if the `If` expression is `True` and an optional set of statements to be executed if the expression is `False`.</span></span> <span data-ttu-id="6f08f-397">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-397">For example:</span></span>

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

<span data-ttu-id="6f08f-398">If 的行版本语句绑定小于紧密":"，并将其`Else`绑定到从词法上紧靠在前面`If`的允许的语法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-398">The line version of the If statement binds less tightly than ":", and its `Else` binds to the lexically nearest preceding `If` that is allowed by the syntax.</span></span> <span data-ttu-id="6f08f-399">例如，以下两个版本是等效的：</span><span class="sxs-lookup"><span data-stu-id="6f08f-399">For example, the following two versions are equivalent:</span></span>

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

<span data-ttu-id="6f08f-400">标签声明语句以外的所有语句都允许在行内`If`语句，其中包括块语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-400">All statements other than label declaration statements are allowed inside a line `If` statement, including block statements.</span></span> <span data-ttu-id="6f08f-401">但是，他们不能将 LineTerminators StatementTerminators 作为多行 lambda 表达式内部除外。</span><span class="sxs-lookup"><span data-stu-id="6f08f-401">However, they may not use LineTerminators as StatementTerminators except inside multi-line lambda expressions.</span></span> <span data-ttu-id="6f08f-402">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-402">For example:</span></span>

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

### <a name="select-case-statements"></a><span data-ttu-id="6f08f-403">Select Case 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-403">Select Case Statements</span></span>

<span data-ttu-id="6f08f-404">一个`Select Case`语句执行基于表达式的值的语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-404">A `Select Case` statement executes statements based on the value of an expression.</span></span>

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

<span data-ttu-id="6f08f-405">表达式必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-405">The expression must be classified as a value.</span></span> <span data-ttu-id="6f08f-406">当`Select Case`执行语句时，`Select`首先，计算表达式和`Case`文本声明顺序然后计算语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-406">When a `Select Case` statement is executed, the `Select` expression is evaluated first, and the `Case` statements are then evaluated in order of textual declaration.</span></span> <span data-ttu-id="6f08f-407">第一个`Case`语句的计算结果为`True`具有执行其块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-407">The first `Case` statement that evaluates to `True` has its block executed.</span></span> <span data-ttu-id="6f08f-408">如果没有`Case`语句的计算结果为`True`并且没有`Case Else`语句中，执行此块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-408">If no `Case` statement evaluates to `True` and there is a `Case Else` statement, that block is executed.</span></span> <span data-ttu-id="6f08f-409">后一个块执行完成后，执行将传递到末尾`Select`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-409">Once a block has finished executing, execution passes to the end of the `Select` statement.</span></span>

<span data-ttu-id="6f08f-410">执行`Case`块不允许"贯穿"到下一步的开关部分。</span><span class="sxs-lookup"><span data-stu-id="6f08f-410">Execution of a `Case` block is not permitted to "fall through" to the next switch section.</span></span> <span data-ttu-id="6f08f-411">这可以防止在其他语言中出现的 bug 的公共类时`Case`终止语句无意中遗漏了。</span><span class="sxs-lookup"><span data-stu-id="6f08f-411">This prevents a common class of bugs that occur in other languages when a `Case` terminating statement is accidentally omitted.</span></span> <span data-ttu-id="6f08f-412">下面的示例阐释了这种行为：</span><span class="sxs-lookup"><span data-stu-id="6f08f-412">The following example illustrates this behavior:</span></span>

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

<span data-ttu-id="6f08f-413">该代码会输出：</span><span class="sxs-lookup"><span data-stu-id="6f08f-413">The code prints:</span></span>

```
x = 10
```

<span data-ttu-id="6f08f-414">尽管`Case 10`并`Case 20 - 10`相同的值，选择`Case 10`因为它位于之前执行`Case 20 - 10`文本上。</span><span class="sxs-lookup"><span data-stu-id="6f08f-414">Although `Case 10` and `Case 20 - 10` select for the same value, `Case 10` is executed because it precedes `Case 20 - 10` textually.</span></span> <span data-ttu-id="6f08f-415">在下一步`Case`是达到之后, 继续执行`Select`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-415">When the next `Case` is reached, execution continues after the `Select` statement.</span></span>

<span data-ttu-id="6f08f-416">一个`Case`子句可能采用两种形式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-416">A `Case` clause may take two forms.</span></span> <span data-ttu-id="6f08f-417">一种形式是一个可选`Is`关键字、 比较运算符和表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-417">One form is an optional `Is` keyword, a comparison operator, and an expression.</span></span> <span data-ttu-id="6f08f-418">转换为的类型的表达式`Select`表达式; 如果表达式不是隐式转换为的类型`Select`表达式中，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-418">The expression is converted to the type of the `Select` expression; if the expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="6f08f-419">如果`Select`表达式是*E*，比较运算符是*Op*，和`Case`表达式是*E1*，这种情况被评估为*EOP E1*。</span><span class="sxs-lookup"><span data-stu-id="6f08f-419">If the `Select` expression is *E*, the comparison operator is *Op*, and the `Case` expression is *E1*, the case is evaluated as *E OP E1*.</span></span> <span data-ttu-id="6f08f-420">运算符必须是有效类型的两个表达式中;否则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-420">The operator must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="6f08f-421">另一种形式是可以选择后跟关键字表达式`To`和第二个表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-421">The other form is an expression optionally followed by the keyword `To` and a second expression.</span></span> <span data-ttu-id="6f08f-422">这两个表达式被转换为的类型`Select`表达式; 如果任一表达式不能隐式转换为的类型`Select`表达式中，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-422">Both expressions are converted to the type of the `Select` expression; if either expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="6f08f-423">如果`Select`表达式是`E`，第一个`Case`表达式`E1`，，第二个`Case`表达式是`E2`，则`Case`计算为`E = E1`(如果没有`E2`指定) 或`(E >= E1) And (E <= E2)`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-423">If the `Select` expression is `E`, the first `Case` expression is `E1`, and the second `Case` expression is `E2`, the `Case` is evaluated either as `E = E1` (if no `E2` is specified) or `(E >= E1) And (E <= E2)`.</span></span> <span data-ttu-id="6f08f-424">必须是有效类型的两个表达式中;否则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-424">The operators must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>


## <a name="loop-statements"></a><span data-ttu-id="6f08f-425">循环语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-425">Loop Statements</span></span>

<span data-ttu-id="6f08f-426">循环语句在其主体中允许重复的执行语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-426">Loop statements allow repeated execution of the statements in their body.</span></span>

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

<span data-ttu-id="6f08f-427">每次输入循环主体时，所有本地声明的变量初始化的变量的以前的值为该正文中所做的最新副本。</span><span class="sxs-lookup"><span data-stu-id="6f08f-427">Each time a loop body is entered, a fresh copy is made of all local variables declared in that body, initialized to the previous values of the variables.</span></span> <span data-ttu-id="6f08f-428">为循环主体内的变量的任何引用都将使用最近发出的副本。</span><span class="sxs-lookup"><span data-stu-id="6f08f-428">Any reference to a variable within the loop body will use the most recently made copy.</span></span> <span data-ttu-id="6f08f-429">此代码显示了示例：</span><span class="sxs-lookup"><span data-stu-id="6f08f-429">This code shows an example:</span></span>

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

<span data-ttu-id="6f08f-430">此代码生成输出：</span><span class="sxs-lookup"><span data-stu-id="6f08f-430">The code produces the output:</span></span>

```
31    32    33
```

<span data-ttu-id="6f08f-431">当执行循环主体时，它使用变量的任何副本是最新。</span><span class="sxs-lookup"><span data-stu-id="6f08f-431">When the loop body is executed, it uses whichever copy of the variable is current.</span></span> <span data-ttu-id="6f08f-432">例如，语句`Dim y = x`的最新副本是指`y`的原始副本和`x`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-432">For example, the statement  `Dim y = x` refers to the latest copy of `y` and the original copy of `x`.</span></span> <span data-ttu-id="6f08f-433">并创建 lambda 时，它会记住的变量的任何副本时的当前的创建的时间。</span><span class="sxs-lookup"><span data-stu-id="6f08f-433">And when a lambda is created, it remembers whichever copy of a variable was current at the time it was created.</span></span> <span data-ttu-id="6f08f-434">因此，每个 lambda 使用的相同的共享的副本`x`，但另一份`y`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-434">Therefore, each lambda uses the same shared copy of `x`, but a different copy of `y`.</span></span> <span data-ttu-id="6f08f-435">在程序结束时，lambda，在执行时，共享的副本`x`它们都代表现在位于其最终值 3。</span><span class="sxs-lookup"><span data-stu-id="6f08f-435">At the end of the program, when it executes the lambdas, that shared copy of `x` that they all refer to is now at its final value 3.</span></span>

<span data-ttu-id="6f08f-436">请注意，是否没有任何 lambda 或 LINQ 表达式，则不可能知道的最新副本对循环条目。</span><span class="sxs-lookup"><span data-stu-id="6f08f-436">Note that if there are no lambdas or LINQ expressions, then it's impossible to know that a fresh copy is made on loop entry.</span></span> <span data-ttu-id="6f08f-437">实际上，编译器优化可以避免这种情况下创建副本。</span><span class="sxs-lookup"><span data-stu-id="6f08f-437">Indeed, compiler optimizations will avoid making copies in this case.</span></span> <span data-ttu-id="6f08f-438">另请注意，它是到非法`GoTo`到循环包含 lambda 或 LINQ 表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-438">Note too that it's illegal to `GoTo` into a loop that contains lambdas or LINQ expressions.</span></span>


### <a name="whileend-while-and-doloop-statements"></a><span data-ttu-id="6f08f-439">段时间...结束时和执行操作...循环语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-439">While...End While and Do...Loop Statements</span></span>

<span data-ttu-id="6f08f-440">一个`While`或`Do`循环语句基于布尔表达式的循环。</span><span class="sxs-lookup"><span data-stu-id="6f08f-440">A `While` or `Do` loop statement loops based on a Boolean expression.</span></span>

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

<span data-ttu-id="6f08f-441">一个`While`循环语句循环，只要布尔表达式的计算结果为 true;`Do`循环语句可以包含更复杂的条件。</span><span class="sxs-lookup"><span data-stu-id="6f08f-441">A `While` loop statement loops as long as the Boolean expression evaluates to true; a `Do` loop statement may contain a more complex condition.</span></span> <span data-ttu-id="6f08f-442">表达式可能会放置后`Do`关键字后或`Loop`关键字，但这两者不之后。</span><span class="sxs-lookup"><span data-stu-id="6f08f-442">An expression may be placed after the `Do` keyword or after the `Loop` keyword, but not after both.</span></span> <span data-ttu-id="6f08f-443">布尔表达式计算部分所述[布尔表达式](expressions.md#boolean-expressions)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-443">The Boolean expression is evaluated as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="6f08f-444">(注意： 这不需要具有布尔类型的表达式)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-444">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="6f08f-445">它也是有效; 所有指定的表达式在这种情况下，将永远不会退出循环。</span><span class="sxs-lookup"><span data-stu-id="6f08f-445">It is also valid to specify no expression at all; in that case, the loop will never exit.</span></span> <span data-ttu-id="6f08f-446">如果表达式位于后`Do`，它将计算在每次迭代执行循环块之前。</span><span class="sxs-lookup"><span data-stu-id="6f08f-446">If the expression is placed after `Do`, it will be evaluated before the loop block is executed on each iteration.</span></span> <span data-ttu-id="6f08f-447">如果表达式位于后`Loop`，在每次迭代执行循环块后它进行评估。</span><span class="sxs-lookup"><span data-stu-id="6f08f-447">If the expression is placed after `Loop`, it will be evaluated after the loop block has executed on each iteration.</span></span> <span data-ttu-id="6f08f-448">将后面的表达式`Loop`因此将生成一个详细循环之后放置于`Do`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-448">Placing the expression after `Loop` will therefore generate one more loop than placement after `Do`.</span></span> <span data-ttu-id="6f08f-449">下面的示例演示此行为：</span><span class="sxs-lookup"><span data-stu-id="6f08f-449">The following example demonstrates this behavior:</span></span>

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

<span data-ttu-id="6f08f-450">此代码生成输出：</span><span class="sxs-lookup"><span data-stu-id="6f08f-450">The code produces the output:</span></span>

```
Second Loop
```

<span data-ttu-id="6f08f-451">在第一个循环的情况下对条件进行评估之前循环执行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-451">In the case of the first loop, the condition is evaluated before the loop executes.</span></span> <span data-ttu-id="6f08f-452">对于第二个循环，循环执行之后执行条件。</span><span class="sxs-lookup"><span data-stu-id="6f08f-452">In the case of the second loop, the condition is executed after the loop executes.</span></span> <span data-ttu-id="6f08f-453">条件表达式必须带有前缀`While`关键字或`Until`关键字。</span><span class="sxs-lookup"><span data-stu-id="6f08f-453">The conditional expression must be prefixed with either a `While` keyword or an `Until` keyword.</span></span> <span data-ttu-id="6f08f-454">前者会中断该循环，如果条件的计算结果为 false，后者的条件的计算结果为 true 时。</span><span class="sxs-lookup"><span data-stu-id="6f08f-454">The former breaks the loop if the condition evaluates to false, the latter when the condition evaluates to true.</span></span>

<span data-ttu-id="6f08f-455">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-455">__Note.__</span></span> <span data-ttu-id="6f08f-456">`Until` 不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="6f08f-456">`Until` is not a reserved word.</span></span>


### <a name="fornext-statements"></a><span data-ttu-id="6f08f-457">为...下一语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-457">For...Next Statements</span></span>

<span data-ttu-id="6f08f-458">一个`For...Next`基于边界的一组语句循环。</span><span class="sxs-lookup"><span data-stu-id="6f08f-458">A `For...Next` statement loops based on a set of bounds.</span></span> <span data-ttu-id="6f08f-459">一个`For`语句指定循环控制变量、 更低绑定表达式、 一个上限绑定表达式，以及可选步骤值表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-459">A `For` statement specifies a loop control variable, a lower bound expression, an upper bound expression, and an optional step value expression.</span></span> <span data-ttu-id="6f08f-460">指定循环控制变量通过标识符后跟一个可选`As`子句或表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-460">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

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

<span data-ttu-id="6f08f-461">按照以下规则，循环控制变量是指到特定的新本地变量这`For...Next`语句，或到预先存在的变量，或表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-461">As per the following rules, the loop control variable refers either to a new local variable specific to this `For...Next` statement, or to a pre-existing variable, or to an expression.</span></span>

* <span data-ttu-id="6f08f-462">如果循环控制变量的标识符`As`子句中，标识符定义中指定的类型的一个新局部变量`As`子句，作用域为整个`For`循环。</span><span class="sxs-lookup"><span data-stu-id="6f08f-462">If the loop control variable is an identifier with an `As` clause, the identifier defines a new local variable of the type specified in the `As` clause, scoped to the entire `For` loop.</span></span>

* <span data-ttu-id="6f08f-463">如果循环控制变量是标识符，而无需`As`子句，则该标识符首次使用解析简单名称解析规则 (请参阅部分[简单名称表达式](expressions.md#simple-name-expressions))，只不过，此匹配项标识符将不本身会导致要创建一个隐式局部变量 (部分[隐式的局部声明](statements.md#implicit-local-declarations))。</span><span class="sxs-lookup"><span data-stu-id="6f08f-463">If the loop control variable is an identifier without an `As` clause, then the identifier is first resolved using the simple name resolution rules (see Section [Simple Name Expressions](expressions.md#simple-name-expressions)), excepting that this occurrence of the identifier would not in and of itself cause an implicit local variable to be created (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)).</span></span>

  * <span data-ttu-id="6f08f-464">如果此解析成功，并且结果分类为变量，然后循环控制变量是该预先存在的变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-464">If this resolution succeeds and the result is classified as a variable, then the loop control variable is that pre-existing variable.</span></span>

  * <span data-ttu-id="6f08f-465">如果解析失败，或如果解析成功，并且结果分类为一种类型，然后：</span><span class="sxs-lookup"><span data-stu-id="6f08f-465">If resolution fails, or if resolution succeeds and the result is classified as a type, then:</span></span>
    * <span data-ttu-id="6f08f-466">如果正在使用本地变量的类型推理，标识符会定义从绑定推断其类型的新本地变量和单步表达式，作用域为整个`For`循环;</span><span class="sxs-lookup"><span data-stu-id="6f08f-466">if local variable type inference is being used, the identifier defines a new local variable whose type is inferred from the bound and step expressions, scoped to the entire `For` loop;</span></span>
    * <span data-ttu-id="6f08f-467">如果不使用本地变量的类型推理但隐式的局部声明，则隐式的本地变量，将创建其作用域是整个方法 (部分[隐式的局部声明](statements.md#implicit-local-declarations))，并循环控制变量引用此预先存在的变量;</span><span class="sxs-lookup"><span data-stu-id="6f08f-467">if local variable type inference is not being used but implicit local declaration is, then an implicit local variable is created whose scope is the entire method (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)), and the loop control variable refers to this pre-existing variable;</span></span>
    * <span data-ttu-id="6f08f-468">如果使用本地变量的类型推理既不隐式本地声明，它是错误的。</span><span class="sxs-lookup"><span data-stu-id="6f08f-468">if neither local variable type inference nor implicit local declarations are used, it is an error.</span></span>

  * <span data-ttu-id="6f08f-469">如果解析成功且内容分类为一个类型既不变量，它是错误的。</span><span class="sxs-lookup"><span data-stu-id="6f08f-469">If resolution succeeds with something classified as neither a type nor a variable, it is an error.</span></span>

* <span data-ttu-id="6f08f-470">如果循环控制变量是一个表达式，表达式必须分类为变量中。</span><span class="sxs-lookup"><span data-stu-id="6f08f-470">If the loop control variable is an expression, the expression must be classified as a variable.</span></span>

<span data-ttu-id="6f08f-471">循环控制变量不能由另一个封闭`For...Next`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-471">A loop control variable cannot be used by another enclosing `For...Next` statement.</span></span> <span data-ttu-id="6f08f-472">循环控制变量的类型`For`语句确定迭代的类型，并且必须之一：</span><span class="sxs-lookup"><span data-stu-id="6f08f-472">The type of the loop control variable of a `For` statement determines the type of the iteration and must be one of:</span></span>

* <span data-ttu-id="6f08f-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span><span class="sxs-lookup"><span data-stu-id="6f08f-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span></span>
* <span data-ttu-id="6f08f-474">一个枚举的类型</span><span class="sxs-lookup"><span data-stu-id="6f08f-474">An enumerated type</span></span>
* `Object`
* <span data-ttu-id="6f08f-475">一种类型`T`具有以下运算符，其中`B`是可以使用布尔表达式中的类型：</span><span class="sxs-lookup"><span data-stu-id="6f08f-475">A type `T` that has the following operators, where `B` is a type that can be used in a Boolean expression:</span></span>

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

<span data-ttu-id="6f08f-476">绑定和步长表达式必须可隐式转换为循环控制变量的类型，必须被归类为值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-476">The bound and step expressions must be implicitly convertible to the type of the loop control variable and must be classified as values.</span></span> <span data-ttu-id="6f08f-477">在编译时，通过选择更低绑定、 上边界和步骤表达式类型之间最宽的类型推断的循环控制变量的类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-477">At compile time, the type of the loop control variable is inferred by choosing the widest type among the lower bound, upper bound, and step expression types.</span></span> <span data-ttu-id="6f08f-478">如果有两个类型之间没有扩大转换，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-478">If there is no widening conversion between two of the types, then a compile-time error occurs.</span></span>

<span data-ttu-id="6f08f-479">在运行时，如果循环控制变量的类型为`Object`，迭代的类型将被推断为在编译时，使用两种例外情况相同。</span><span class="sxs-lookup"><span data-stu-id="6f08f-479">At run time, if the type of the loop control variable is `Object`, then the type of the iteration is inferred the same as at compile time, with two exceptions.</span></span> <span data-ttu-id="6f08f-480">首先，如果边界和步骤表达式是整型类型的所有但具有无最宽的类型，则将推断最大的类型包含所有三种类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-480">First, if the bound and step expressions are all of integral types but have no widest type, then the widest type that encompasses all three types will be inferred.</span></span> <span data-ttu-id="6f08f-481">其次，如果循环控制变量的类型推断为`String`，`Double`将改为推断。</span><span class="sxs-lookup"><span data-stu-id="6f08f-481">And second, if the type of the loop control variable is inferred to be `String`, `Double` will be inferred instead.</span></span> <span data-ttu-id="6f08f-482">如果在运行时，可以确定没有循环控件类型或任何表达式不能转换为循环控制类型`System.InvalidCastException`会发生。</span><span class="sxs-lookup"><span data-stu-id="6f08f-482">If, at run time, no loop control type can be determined or if any of the expressions cannot be converted to the loop control type, a `System.InvalidCastException` will occur.</span></span> <span data-ttu-id="6f08f-483">循环控件类型选择循环的开头后, 将在迭代中，而不考虑为循环控制变量中的值所做的更改使用相同的类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-483">Once a loop control type has been chosen at the beginning of the loop, the same type will be used throughout the iteration, regardless of changes made to the value in the loop control variable.</span></span>

<span data-ttu-id="6f08f-484">一个`For`语句必须是匹配的关闭`Next`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-484">A `For` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="6f08f-485">一个`Next`而无需变量的语句匹配最内部的开放`For`语句，而`Next`与一个或多个循环控制变量的语句，从左到右，匹配`For`匹配每个变量的循环。</span><span class="sxs-lookup"><span data-stu-id="6f08f-485">A `Next` statement without a variable matches the innermost open `For` statement, while a `Next` statement with one or more loop control variables will, from left to right, match the `For` loops that match each variable.</span></span> <span data-ttu-id="6f08f-486">如果变量与匹配`For`此时不嵌套最深的循环的循环，会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-486">If a variable matches a `For` loop that is not the most nested loop at that point, a compile-time error results.</span></span>

<span data-ttu-id="6f08f-487">在循环开始时，三个表达式按文本顺序计算和更低绑定表达式被分配给循环控制变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-487">At the beginning of the loop, the three expressions are evaluated in textual order and the lower bound expression is assigned to the loop control variable.</span></span> <span data-ttu-id="6f08f-488">如果省略步骤值，它隐式为字面值`1`，已转换为循环控制变量的类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-488">If the step value is omitted, it is implicitly the literal `1`, converted to the type of the loop control variable.</span></span> <span data-ttu-id="6f08f-489">三个表达式只计算循环的开头。</span><span class="sxs-lookup"><span data-stu-id="6f08f-489">The three expressions are only ever evaluated at the beginning of the loop.</span></span>

<span data-ttu-id="6f08f-490">在每个循环开始时，控制变量进行比较，以查看其是否大于终结点的步骤表达式是肯定的还是小于终结点，如果步骤表达式为负。</span><span class="sxs-lookup"><span data-stu-id="6f08f-490">At the beginning of each loop, the control variable is compared to see if it is greater than the end point if the step expression is positive, or less than the end point if the step expression is negative.</span></span> <span data-ttu-id="6f08f-491">如果是，`For`循环将终止; 否则该循环块执行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-491">If it is, the `For` loop terminates; otherwise the loop block executes.</span></span> <span data-ttu-id="6f08f-492">如果循环控制变量不是基元类型，比较运算符将由是否表达式`step >= step - step`是 true 还是 false。</span><span class="sxs-lookup"><span data-stu-id="6f08f-492">If the loop control variable is not a primitive type, the comparison operator is determined by whether the expression `step >= step - step` is true or false.</span></span> <span data-ttu-id="6f08f-493">在`Next`语句的步长值添加到控件变量和执行将返回到循环的顶部。</span><span class="sxs-lookup"><span data-stu-id="6f08f-493">At the `Next` statement, the step value is added to the control variable and execution returns to the top of the loop.</span></span>

<span data-ttu-id="6f08f-494">请注意，循环控制变量的新副本*不*创建循环块的每次迭代。</span><span class="sxs-lookup"><span data-stu-id="6f08f-494">Note that a new copy of the loop control variable is *not* created on each iteration of the loop block.</span></span> <span data-ttu-id="6f08f-495">在这一方面`For`语句不同于`For Each`(部分[为每个...下一语句](statements.md#for-eachnext-statements))。</span><span class="sxs-lookup"><span data-stu-id="6f08f-495">In this respect, the `For` statement differs from `For Each` (Section [For Each...Next Statements](statements.md#for-eachnext-statements)).</span></span>

<span data-ttu-id="6f08f-496">它不是有效的分支到`For`从循环在循环外。</span><span class="sxs-lookup"><span data-stu-id="6f08f-496">It is not valid to branch into a `For` loop from outside the loop.</span></span>


### <a name="for-eachnext-statements"></a><span data-ttu-id="6f08f-497">每个...下一语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-497">For Each...Next Statements</span></span>

<span data-ttu-id="6f08f-498">一个`For Each...Next`语句基于在表达式中的元素的循环。</span><span class="sxs-lookup"><span data-stu-id="6f08f-498">A `For Each...Next` statement loops based on the elements in an expression.</span></span> <span data-ttu-id="6f08f-499">一个`For Each`语句指定循环控制变量和枚举器表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-499">A `For Each` statement specifies a loop control variable and an enumerator expression.</span></span> <span data-ttu-id="6f08f-500">指定循环控制变量通过标识符后跟一个可选`As`子句或表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-500">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

<span data-ttu-id="6f08f-501">遵循相同的规则`For...Next`语句 (部分[对于。...下一语句](statements.md#fornext-statements))，循环控制变量是指到特定的新本地变量这对于每个...下一个语句，或到预先存在的变量，或表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-501">Following the same rules as `For...Next` statements (Section [For...Next Statements](statements.md#fornext-statements)), the loop control variable refers either to a new local variable specific to this For Each...Next statement, or to a pre-existing variable, or to an expression.</span></span>

<span data-ttu-id="6f08f-502">枚举器表达式必须分类为一个值，并且其类型必须是集合类型或`Object`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-502">The enumerator expression must be classified as a value and its type must be a collection type or `Object`.</span></span> <span data-ttu-id="6f08f-503">如果枚举器表达式的类型为`Object`，则所有的处理延迟到运行时。</span><span class="sxs-lookup"><span data-stu-id="6f08f-503">If the type of the enumerator expression is `Object`, then all processing is deferred until run-time.</span></span> <span data-ttu-id="6f08f-504">否则，必须存在从转换集合的元素类型为循环控制变量的类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-504">Otherwise, a conversion must exist from the element type of the collection to the type of the loop control variable.</span></span>

<span data-ttu-id="6f08f-505">循环控制变量不能由另一个封闭`For Each`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-505">The loop control variable cannot be used by another enclosing `For Each` statement.</span></span> <span data-ttu-id="6f08f-506">一个`For Each`语句必须是匹配的关闭`Next`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-506">A `For Each` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="6f08f-507">一个`Next`语句而循环控制变量不匹配的最内部打开`For Each`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-507">A `Next` statement without a loop control variable matches the innermost open `For Each`.</span></span> <span data-ttu-id="6f08f-508">一个`Next`与一个或多个循环控制变量的语句，从左到右，匹配`For Each`具有相同的循环控制变量的循环。</span><span class="sxs-lookup"><span data-stu-id="6f08f-508">A `Next` statement with one or more loop control variables will, from left to right, match the `For Each` loops that have the same loop control variable.</span></span> <span data-ttu-id="6f08f-509">如果变量与匹配`For Each`此时不嵌套最深的循环的循环，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-509">If a variable matches a `For Each` loop that is not the most nested loop at that point, a compile-time error occurs.</span></span>

<span data-ttu-id="6f08f-510">一种类型`C`称为*集合类型*如果一个的：</span><span class="sxs-lookup"><span data-stu-id="6f08f-510">A type `C` is said to be a *collection type* if one of:</span></span>

* <span data-ttu-id="6f08f-511">以下所有项都为 true:</span><span class="sxs-lookup"><span data-stu-id="6f08f-511">All of the following are true:</span></span>
  * <span data-ttu-id="6f08f-512">`C` 包含可访问的实例，共享或具有签名的扩展方法`GetEnumerator()`返回类型`E`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-512">`C` contains an accessible instance, shared or extension method with the signature `GetEnumerator()` that returns a type `E`.</span></span>
  * <span data-ttu-id="6f08f-513">`E` 包含可访问的实例，共享或具有签名的扩展方法`MoveNext()`和返回类型`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-513">`E` contains an accessible instance, shared or extension method with the signature `MoveNext()` and the return type `Boolean`.</span></span>
  * <span data-ttu-id="6f08f-514">`E` 包含可访问的实例或共享的属性名为`Current`有一个 getter。</span><span class="sxs-lookup"><span data-stu-id="6f08f-514">`E` contains an accessible instance or shared property named `Current` that has a getter.</span></span> <span data-ttu-id="6f08f-515">此属性的类型是集合类型的元素类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-515">The type of this property is the element type of the collection type.</span></span>

* <span data-ttu-id="6f08f-516">实现接口`System.Collections.Generic.IEnumerable(Of T)`，在这种情况下被视为集合的元素类型可`T`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-516">It implements the interface `System.Collections.Generic.IEnumerable(Of T)`, in which case the element type of the collection is considered to be `T`.</span></span>

* <span data-ttu-id="6f08f-517">实现接口`System.Collections.IEnumerable`，在这种情况下被视为集合的元素类型可`Object`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-517">It implements the interface `System.Collections.IEnumerable`, in which case the element type of the collection is considered to be `Object`.</span></span>

<span data-ttu-id="6f08f-518">以下是可以枚举某个类的示例：</span><span class="sxs-lookup"><span data-stu-id="6f08f-518">Following is an example of a class that can be enumerated:</span></span>

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

<span data-ttu-id="6f08f-519">在循环开始之前，枚举器表达式计算。</span><span class="sxs-lookup"><span data-stu-id="6f08f-519">Before the loop begins, the enumerator expression is evaluated.</span></span> <span data-ttu-id="6f08f-520">如果表达式的类型不满足设计模式，则表达式转换为`System.Collections.IEnumerable`或`System.Collections.Generic.IEnumerable(Of T)`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-520">If the type of the expression does not satisfy the design pattern, then the expression is cast to `System.Collections.IEnumerable` or `System.Collections.Generic.IEnumerable(Of T)`.</span></span> <span data-ttu-id="6f08f-521">如果表达式类型实现泛型接口，泛型接口是在编译时首选，但非泛型接口是在运行时首选。</span><span class="sxs-lookup"><span data-stu-id="6f08f-521">If the expression type implements the generic interface, the generic interface is preferred at compile-time but the non-generic interface is preferred at run-time.</span></span> <span data-ttu-id="6f08f-522">如果表达式类型实现泛型接口多次，该语句被视为不明确，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-522">If the expression type implements the generic interface multiple times, the statement is considered ambiguous and a compile-time error occurs.</span></span>

<span data-ttu-id="6f08f-523">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-523">__Note.__</span></span> <span data-ttu-id="6f08f-524">因为选取泛型接口意味着对接口方法的所有调用将都涉及类型参数，将在后期绑定的情况下，首选的非泛型接口。</span><span class="sxs-lookup"><span data-stu-id="6f08f-524">The non-generic interface is preferred in the late bound case, because picking the generic interface would mean that all the calls to the interface methods would involve type parameters.</span></span> <span data-ttu-id="6f08f-525">由于不可能知道在运行时类型参数匹配，所有此类调用将需要进行使用后期绑定调用。</span><span class="sxs-lookup"><span data-stu-id="6f08f-525">Since it is not possible to know the matching type arguments at run-time, all such calls would have to be made using late-bound calls.</span></span> <span data-ttu-id="6f08f-526">这会慢于调用非泛型接口，因为无法使用编译时调用调用非泛型接口。</span><span class="sxs-lookup"><span data-stu-id="6f08f-526">This would be slower than calling the non-generic interface because the non-generic interface could be called using compile-time calls.</span></span>

<span data-ttu-id="6f08f-527">`GetEnumerator` 对生成的值并返回调用函数的值存储在临时的。</span><span class="sxs-lookup"><span data-stu-id="6f08f-527">`GetEnumerator` is called on the resulting value and the return value of the function is stored in a temporary.</span></span> <span data-ttu-id="6f08f-528">然后在每次迭代开头`MoveNext`称为临时磁盘上。</span><span class="sxs-lookup"><span data-stu-id="6f08f-528">Then at the beginning of each iteration, `MoveNext` is called on the temporary.</span></span> <span data-ttu-id="6f08f-529">如果它返回`False`，循环将终止。</span><span class="sxs-lookup"><span data-stu-id="6f08f-529">If it returns `False`, the loop terminates.</span></span> <span data-ttu-id="6f08f-530">否则，每次迭代循环的执行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6f08f-530">Otherwise, each iteration of the loop is executed as follows:</span></span>

1. <span data-ttu-id="6f08f-531">如果循环控制变量确定新的本地变量 （而不是一个预先存在），然后创建这个本地变量的最新副本。</span><span class="sxs-lookup"><span data-stu-id="6f08f-531">If the loop control variable identified a new local variable (rather than a pre-existing one), then a fresh copy of this local variable is created.</span></span> <span data-ttu-id="6f08f-532">对于当前迭代循环块内的所有引用将都引用到此副本。</span><span class="sxs-lookup"><span data-stu-id="6f08f-532">For the current iteration, all references within the loop block will refer to this copy.</span></span>
2. <span data-ttu-id="6f08f-533">`Current`检索属性，强制转换为的类型的循环控制变量 （无论转换是隐式或显式），并分配给循环控制变量。</span><span class="sxs-lookup"><span data-stu-id="6f08f-533">The `Current` property is retrieved, coerced to the type of the loop control variable (regardless of whether the conversion is implicit or explicit), and assigned to the loop control variable.</span></span>
3. <span data-ttu-id="6f08f-534">循环块执行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-534">The loop block executes.</span></span>

<span data-ttu-id="6f08f-535">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-535">__Note.__</span></span> <span data-ttu-id="6f08f-536">没有版本 10.0 和 11.0 的语言之间的行为会略有变化。</span><span class="sxs-lookup"><span data-stu-id="6f08f-536">There is a slight change in behavior between version 10.0 and 11.0 of the language.</span></span> <span data-ttu-id="6f08f-537">在 11.0 之前, 是全新的迭代变量*不*创建每个迭代的循环。</span><span class="sxs-lookup"><span data-stu-id="6f08f-537">Prior to 11.0, a fresh iteration variable was *not* created for each iteration of the loop.</span></span> <span data-ttu-id="6f08f-538">这种差异的迭代变量捕获的 lambda 或 LINQ 表达式，然后调用循环后，才是可观察量：</span><span class="sxs-lookup"><span data-stu-id="6f08f-538">This difference is observable only if the iteration variable is captured by a lambda or a LINQ expression which is then invoked after the loop:</span></span>

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

<span data-ttu-id="6f08f-539">Visual Basic 10.0 中，到此生成在编译时警告并打印"3"三次。</span><span class="sxs-lookup"><span data-stu-id="6f08f-539">Up to Visual Basic 10.0, this produced a warning at compile-time and printed "3" three times.</span></span> <span data-ttu-id="6f08f-540">这是循环的因为时只有一个单一变量"x"共享的所有迭代中，并将所有三个 lambda 捕获的同一个"x"，然后由 lambda 的执行的时间它然后保留在数字 3。</span><span class="sxs-lookup"><span data-stu-id="6f08f-540">That was because there was only a single variable "x" shared by all iterations of the loop, and all three lambdas captured the same "x", and by the time the lambdas were executed it then held the number 3.</span></span> <span data-ttu-id="6f08f-541">截至 Visual Basic 11.0，会输出"1、 2、 3"。</span><span class="sxs-lookup"><span data-stu-id="6f08f-541">As of Visual Basic 11.0, it prints "1, 2, 3".</span></span> <span data-ttu-id="6f08f-542">这是因为每个 lambda 捕获不同的变量"x"。</span><span class="sxs-lookup"><span data-stu-id="6f08f-542">That is because each lambda captures a different variable "x".</span></span>


<span data-ttu-id="6f08f-543">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-543">__Note.__</span></span> <span data-ttu-id="6f08f-544">迭代的当前元素被转换为循环控制变量的类型，即使该转换是显式的因为没有引入在语句中的转换运算符不方便的位置。</span><span class="sxs-lookup"><span data-stu-id="6f08f-544">The current element of the iteration is converted to the type of the loop control variable even if the conversion is explicit because there is no convenient place to introduce a conversion operator in the statement.</span></span> <span data-ttu-id="6f08f-545">使用现已过时类型时，这成为觉得很麻烦`System.Collections.ArrayList`，因为其元素类型是`Object`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-545">This became particularly troublesome when working with the now-obsolete type `System.Collections.ArrayList`, because its element type is `Object`.</span></span> <span data-ttu-id="6f08f-546">这会需要强制转换中很多次，这正是我们感觉不是理想之选。</span><span class="sxs-lookup"><span data-stu-id="6f08f-546">This would have required casts in a great many loops, something we felt was not ideal.</span></span> <span data-ttu-id="6f08f-547">泛型讽刺的是，启用创建强类型集合， `System.Collections.Generic.List(Of T)`，可能会这做我们重新考虑此设计点，但为兼容性的起见，不能更改此现在。</span><span class="sxs-lookup"><span data-stu-id="6f08f-547">Ironically, generics enabled the creation of a strongly-typed collection, `System.Collections.Generic.List(Of T)`, which might have made us rethink this design point, but for compatibility's sake, this cannot be changed now.</span></span>


<span data-ttu-id="6f08f-548">当`Next`语句到达时，执行将返回到循环的顶部。</span><span class="sxs-lookup"><span data-stu-id="6f08f-548">When the `Next` statement is reached, execution returns to the top of the loop.</span></span> <span data-ttu-id="6f08f-549">如果指定的变量后`Next`关键字，它必须是后的第一个变量相同`For Each`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-549">If a variable is specified after the `Next` keyword, it must be the same as the first variable after the `For Each`.</span></span> <span data-ttu-id="6f08f-550">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="6f08f-550">For example, consider the following code:</span></span>

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

<span data-ttu-id="6f08f-551">它等效于以下代码：</span><span class="sxs-lookup"><span data-stu-id="6f08f-551">It is equivalent to the following code:</span></span>

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

<span data-ttu-id="6f08f-552">如果类型`E`枚举器的实现`System.IDisposable`，然后通过调用退出循环时释放枚举器`Dispose`方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-552">If the type `E` of the enumerator implements `System.IDisposable`, then the enumerator is disposed upon exiting the loop by calling the `Dispose` method.</span></span> <span data-ttu-id="6f08f-553">这可确保释放枚举数控制的资源。</span><span class="sxs-lookup"><span data-stu-id="6f08f-553">This ensures that resources held by the enumerator are released.</span></span> <span data-ttu-id="6f08f-554">如果方法包含`For Each`语句不使用非结构化的错误处理，则`For Each`语句包装在`Try`语句`Dispose`方法中调用`Finally`以确保清理。</span><span class="sxs-lookup"><span data-stu-id="6f08f-554">If the method containing the `For Each` statement does not use unstructured error handling, then the `For Each` statement is wrapped in a `Try` statement with the `Dispose` method called in the `Finally` to ensure cleanup.</span></span>

<span data-ttu-id="6f08f-555">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-555">__Note.__</span></span> <span data-ttu-id="6f08f-556">`System.Array`类型是集合类型，并且由于所有数组类型派生`System.Array`，数组类型的任何表达式中允许`For Each`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-556">The `System.Array` type is a collection type, and since all array types derive from `System.Array`, any array type expression is permitted in a `For Each` statement.</span></span> <span data-ttu-id="6f08f-557">对于一维数组，`For Each`语句枚举中递增的索引顺序，从索引 0 开始和结束索引长度为-1 的数组元素。</span><span class="sxs-lookup"><span data-stu-id="6f08f-557">For single-dimensional arrays, the `For Each` statement enumerates the array elements in increasing index order, starting with index 0 and ending with index Length - 1.</span></span> <span data-ttu-id="6f08f-558">对于多维数组，第一次增加的最右边的维度的索引。</span><span class="sxs-lookup"><span data-stu-id="6f08f-558">For multidimensional arrays, the indices of the rightmost dimension are increased first.</span></span>

<span data-ttu-id="6f08f-559">例如，下面的代码输出`1 2 3 4`:</span><span class="sxs-lookup"><span data-stu-id="6f08f-559">For example, the following code prints `1 2 3 4`:</span></span>

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

<span data-ttu-id="6f08f-560">它不是有效的分支到`For Each`从块外的语句块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-560">It is not valid to branch into a `For Each` statement block from outside the block.</span></span>


## <a name="exception-handling-statements"></a><span data-ttu-id="6f08f-561">异常处理语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-561">Exception-Handling Statements</span></span>

<span data-ttu-id="6f08f-562">Visual Basic 支持结构化的异常处理和非结构化的异常处理。</span><span class="sxs-lookup"><span data-stu-id="6f08f-562">Visual Basic supports structured exception handling and unstructured exception handling.</span></span> <span data-ttu-id="6f08f-563">只有一个样式异常处理中使用的是一种方法，但`Error`语句中使用的是结构化的异常处理。</span><span class="sxs-lookup"><span data-stu-id="6f08f-563">Only one style of exception handling may be used in a method, but the `Error` statement may be used in structured exception handling.</span></span> <span data-ttu-id="6f08f-564">如果一种方法使用两种风格的异常处理，会编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-564">If a method uses both styles of exception handling, a compile-time error results.</span></span>

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a><span data-ttu-id="6f08f-565">结构化的异常处理语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-565">Structured Exception-Handling Statements</span></span>

<span data-ttu-id="6f08f-566">结构化的异常处理是一种通过声明将在其中处理特定异常的显式代码块来处理错误的方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-566">Structured exception handling is a method of handling errors by declaring explicit blocks within which certain exceptions will be handled.</span></span> <span data-ttu-id="6f08f-567">结构化的异常处理是通过`Try`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-567">Structured exception handling is done through a `Try` statement.</span></span>

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


<span data-ttu-id="6f08f-568">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-568">For example:</span></span>

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

<span data-ttu-id="6f08f-569">一个`Try`语句由三种类型的块组成： try 块，catch 块和 finally 块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-569">A `Try` statement is made up of three kinds of blocks: try blocks, catch blocks, and finally blocks.</span></span> <span data-ttu-id="6f08f-570">一个*try 块*是包含要执行的语句的语句块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-570">A *try block* is a statement block that contains the statements to be executed.</span></span> <span data-ttu-id="6f08f-571">一个*catch 块*是处理的异常的语句块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-571">A *catch block* is a statement block that handles an exception.</span></span> <span data-ttu-id="6f08f-572">一个*finally 块*是包含要在运行时语句的语句块`Try`退出语句后，无论异常是否已发生，并且已处理。</span><span class="sxs-lookup"><span data-stu-id="6f08f-572">A *finally block* is a statement block that contains statements to be run when the `Try` statement is exited, regardless of whether an exception has occurred and been handled.</span></span> <span data-ttu-id="6f08f-573">一个`Try`语句只能包含一个 try 块和一个 finally 块，必须包含至少一个 catch 块或 finally 块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-573">A `Try` statement, which can only contain one try block and one finally block, must contain at least one catch block or finally block.</span></span> <span data-ttu-id="6f08f-574">若要显式将执行转移到 try 块除外从在同一语句中的 catch 块中无效。</span><span class="sxs-lookup"><span data-stu-id="6f08f-574">It is invalid to explicitly transfer execution into a try block except from within a catch block in the same statement.</span></span>


#### <a name="finally-blocks"></a><span data-ttu-id="6f08f-575">Finally 块</span><span class="sxs-lookup"><span data-stu-id="6f08f-575">Finally Blocks</span></span>

<span data-ttu-id="6f08f-576">一个`Finally`当执行离开的任何部分时始终执行块`Try`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-576">A `Finally` block is always executed when execution leaves any part of the `Try` statement.</span></span> <span data-ttu-id="6f08f-577">执行所需的任何明确的操作`Finally`块; 当执行离开`Try`语句中，系统将自动执行`Finally`阻止，然后将执行传输到其预期目标。</span><span class="sxs-lookup"><span data-stu-id="6f08f-577">No explicit action is required to execute the `Finally` block; when execution leaves the `Try` statement, the system will automatically execute the `Finally` block and then transfer execution to its intended destination.</span></span> <span data-ttu-id="6f08f-578">`Finally`而不考虑执行的会保留执行块`Try`语句： 月底`Try`块中的，通过末尾`Catch`阻止，请通过`Exit Try`语句，通过`GoTo`语句，或通过不处理引发的异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-578">The `Finally` block is executed regardless of how execution leaves the `Try` statement: through the end of the `Try` block, through the end of a `Catch` block, through an `Exit Try` statement, through a `GoTo` statement, or by not handling a thrown exception.</span></span>

<span data-ttu-id="6f08f-579">请注意，`Await`异步方法的表达式和`Yield`语句中迭代器方法，可能会导致在 async 或 iterator 方法实例暂停和恢复方法是其他一些实例中的控制流。</span><span class="sxs-lookup"><span data-stu-id="6f08f-579">Note that the `Await` expression in an async method, and the `Yield` statement in an iterator method, can cause flow of control to suspend in the async or iterator method instance and resume in some other method instance.</span></span> <span data-ttu-id="6f08f-580">但是，这是只是执行的挂起和不涉及退出各自的异步方法或迭代器方法实例，并因此不会导致`Finally`块执行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-580">However, this is merely a suspension of execution and does not involve exiting the respective async method or iterator method instance, and so does not cause `Finally` blocks to be executed.</span></span>

<span data-ttu-id="6f08f-581">是无效的显式传输到执行`Finally`阻止; 也是无效的传输共执行`Finally`阻止除通过异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-581">It is invalid to explicitly transfer execution into a `Finally` block; it is also invalid to transfer execution out of a `Finally` block except through an exception.</span></span>

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a><span data-ttu-id="6f08f-582">catch 块</span><span class="sxs-lookup"><span data-stu-id="6f08f-582">Catch Blocks</span></span>

<span data-ttu-id="6f08f-583">如果在处理时出现异常`Try`块，每个`Catch`语句检查按文本顺序，以确定它是否处理异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-583">If an exception occurs while processing the `Try` block, each `Catch` statement is examined in textual order to determine if it handles the exception.</span></span>

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

<span data-ttu-id="6f08f-584">中指定的标识符`Catch`子句表示已引发的异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-584">The identifier specified in a `Catch` clause represents the exception that has been thrown.</span></span> <span data-ttu-id="6f08f-585">如果该标识符含有`As`子句，则该标识符被视为在内部进行声明`Catch`块的局部声明空间。</span><span class="sxs-lookup"><span data-stu-id="6f08f-585">If the identifier contains an `As` clause, then the identifier is considered to be declared within the `Catch` block's local declaration space.</span></span> <span data-ttu-id="6f08f-586">否则，标识符必须是包含块中定义的本地变量 （而不是静态变量）。</span><span class="sxs-lookup"><span data-stu-id="6f08f-586">Otherwise, the identifier must be a local variable (not a static variable) that was defined in a containing block.</span></span>

<span data-ttu-id="6f08f-587">一个`Catch`不带标识符的子句将捕获所有异常派生自`System.Exception`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-587">A `Catch` clause with no identifier will catch all exceptions derived from `System.Exception`.</span></span> <span data-ttu-id="6f08f-588">一个`Catch`标识符子句仅将捕获的异常的类型将与相同或派生自类型的标识符。</span><span class="sxs-lookup"><span data-stu-id="6f08f-588">A `Catch` clause with an identifier will only catch exceptions whose types are the same as or derived from the type of the identifier.</span></span> <span data-ttu-id="6f08f-589">类型必须是`System.Exception`，或从派生的类型`System.Exception`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-589">The type must be `System.Exception`, or a type derived from `System.Exception`.</span></span> <span data-ttu-id="6f08f-590">当捕获了一个异常派生自`System.Exception`，对异常对象的引用存储在该函数返回的对象中`Microsoft.VisualBasic.Information.Err`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-590">When an exception is caught that derives from `System.Exception`, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="6f08f-591">一个`Catch`子句`When`子句仅将捕获异常时表达式计算结果为`True`; 表达式的类型必须是部分所述的布尔表达式[布尔表达式](expressions.md#boolean-expressions)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-591">A `Catch` clause with a `When` clause will only catch exceptions when the expression evaluates to `True`; the type of the expression must be a Boolean expression as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="6f08f-592">一个`When`子句只应用在检查异常的类型之后，该表达式可能指表示异常的标识符，如本示例所示：</span><span class="sxs-lookup"><span data-stu-id="6f08f-592">A `When` clause is only applied after checking the type of the exception, and the expression may refer to the identifier representing the exception, as this example demonstrates:</span></span>

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

<span data-ttu-id="6f08f-593">此示例输出：</span><span class="sxs-lookup"><span data-stu-id="6f08f-593">This example prints:</span></span>

```
Third handler
```

<span data-ttu-id="6f08f-594">如果`Catch`子句处理异常、 执行会转移到`Catch`块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-594">If a `Catch` clause handles the exception, execution transfers to the `Catch` block.</span></span> <span data-ttu-id="6f08f-595">在末尾`Catch`阻止到后面的第一个语句执行传输`Try`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-595">At the end of the `Catch` block, execution transfers to the first statement following the `Try` statement.</span></span> <span data-ttu-id="6f08f-596">`Try`语句将处理中引发任何异常`Catch`块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-596">The `Try` statement will not handle any exceptions thrown in a `Catch` block.</span></span> <span data-ttu-id="6f08f-597">如果没有`Catch`子句处理的异常执行传输到一个位置通过系统。</span><span class="sxs-lookup"><span data-stu-id="6f08f-597">If no `Catch` clause handles the exception, execution transfers to a location determined by the system.</span></span>

<span data-ttu-id="6f08f-598">是无效的显式传输到执行`Catch`块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-598">It is invalid to explicitly transfer execution into a `Catch` block.</span></span>

<span data-ttu-id="6f08f-599">当子句通常计算之前所引发的异常筛选器。</span><span class="sxs-lookup"><span data-stu-id="6f08f-599">The filters in When clauses are normally evaluated prior to the exception being thrown.</span></span> <span data-ttu-id="6f08f-600">例如，下面的代码将打印"筛选器，最后，Catch"。</span><span class="sxs-lookup"><span data-stu-id="6f08f-600">For instance, the following code will print "Filter, Finally, Catch".</span></span>

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

 <span data-ttu-id="6f08f-601">但是，Async 或 Iterator 方法导致所有最后在它们在之外的任何筛选器之前执行中的块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-601">However, Async and Iterator methods cause all finally blocks inside them to be executed prior to any filters outside.</span></span> <span data-ttu-id="6f08f-602">例如，如果上面的代码有`Async Sub Foo()`，则输出将为"最后，筛选器中，捕获"。</span><span class="sxs-lookup"><span data-stu-id="6f08f-602">For instance, if the above code had `Async Sub Foo()`, then the output would be "Finally, Filter, Catch".</span></span>


#### <a name="throw-statement"></a><span data-ttu-id="6f08f-603">Throw 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-603">Throw Statement</span></span>

<span data-ttu-id="6f08f-604">`Throw`语句将引发异常，它派生自类型的实例所表示`System.Exception`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-604">The `Throw` statement raises an exception, which is represented by an instance of a type derived from `System.Exception`.</span></span>

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

<span data-ttu-id="6f08f-605">如果表达式不分类为一个值，或者不是类型派生自`System.Exception`，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-605">If the expression is not classified as a value or is not a type derived from `System.Exception`, then a compile-time error occurs.</span></span> <span data-ttu-id="6f08f-606">如果在运行时，该表达式的计算结果为 null 值则`System.NullReferenceException`改为引发异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-606">If the expression evaluates to a null value at run time, then a `System.NullReferenceException` exception is raised instead.</span></span>

<span data-ttu-id="6f08f-607">一个`Throw`语句可能会省略的 catch 块中的表达式`Try`语句，前提是没有任何干预性 finally 块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-607">A `Throw` statement may omit the expression within a catch block of a `Try` statement, as long as there is no intervening finally block.</span></span> <span data-ttu-id="6f08f-608">在这种情况下，该语句重新引发在 catch 块中当前正在处理的异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-608">In that case, the statement rethrows the exception currently being handled within the catch block.</span></span> <span data-ttu-id="6f08f-609">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-609">For example:</span></span>

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


### <a name="unstructured-exception-handling-statements"></a><span data-ttu-id="6f08f-610">非结构化的异常处理语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-610">Unstructured Exception-Handling Statements</span></span>

<span data-ttu-id="6f08f-611">非结构化的异常处理是通过指示语句可分支到发生异常时处理错误的方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-611">Unstructured exception handling is a method of handling errors by indicating statements to branch to when an exception occurs.</span></span> <span data-ttu-id="6f08f-612">使用三个语句实现非结构化的异常处理：`Error`语句中，`On Error`语句，和`Resume`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-612">Unstructured exception handling is implemented using three statements: the `Error` statement, the `On Error` statement, and the `Resume` statement.</span></span>

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

<span data-ttu-id="6f08f-613">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-613">For example:</span></span>

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

<span data-ttu-id="6f08f-614">当一种方法使用非结构化的异常处理时，单个结构化的异常处理程序建立的整个捕获所有异常的方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-614">When a method uses unstructured exception handling, a single structured exception handler is established for the entire method that catches all exceptions.</span></span> <span data-ttu-id="6f08f-615">(请注意，在构造函数中此处理程序不会扩展到对调用的调用通过`New`构造函数的开头。)该方法然后跟踪的最新的异常处理程序位置和最近已引发的异常。</span><span class="sxs-lookup"><span data-stu-id="6f08f-615">(Note that in constructors this handler does not extend over the call to the call to `New` at the beginning of the constructor.) The method then keeps track of the most recent exception-handler location and the most recent exception that has been thrown.</span></span> <span data-ttu-id="6f08f-616">在进入方法时，异常处理程序位置和异常均设置为`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-616">At entry to the method, the exception-handler location and the exception are both set to `Nothing`.</span></span> <span data-ttu-id="6f08f-617">使用非结构化的异常处理的方法中引发异常，异常对象的引用存储在该函数返回的对象`Microsoft.VisualBasic.Information.Err`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-617">When an exception is thrown in a method that uses unstructured exception handling, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="6f08f-618">迭代器或异步方法中不允许非结构化的错误处理语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-618">Unstructured error handling statements are not allowed in iterator or async methods.</span></span>


#### <a name="error-statement"></a><span data-ttu-id="6f08f-619">Error 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-619">Error Statement</span></span>

<span data-ttu-id="6f08f-620">`Error`语句，则会引发`System.Exception`异常包含 Visual Basic 6 异常数。</span><span class="sxs-lookup"><span data-stu-id="6f08f-620">An `Error` statement throws a `System.Exception` exception containing a Visual Basic 6 exception number.</span></span> <span data-ttu-id="6f08f-621">表达式必须分类为一个值，其类型必须是隐式转换为`Integer`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-621">The expression must be classified as a value and its type must be implicitly convertible to `Integer`.</span></span>

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a><span data-ttu-id="6f08f-622">On Error 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-622">On Error Statement</span></span>

<span data-ttu-id="6f08f-623">`On Error`的语句修改最新的异常处理状态。</span><span class="sxs-lookup"><span data-stu-id="6f08f-623">An `On Error` statement modifies the most recent exception-handling state.</span></span>

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

<span data-ttu-id="6f08f-624">可在四种方法之一：</span><span class="sxs-lookup"><span data-stu-id="6f08f-624">It may be used in one of four ways:</span></span>

* <span data-ttu-id="6f08f-625">`On Error GoTo -1` 重置的最新例外`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-625">`On Error GoTo -1` resets the most recent exception to `Nothing`.</span></span>

* <span data-ttu-id="6f08f-626">`On Error GoTo 0` 重置为最新的异常处理程序位置`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-626">`On Error GoTo 0` resets the most recent exception-handler location to `Nothing`.</span></span>

* <span data-ttu-id="6f08f-627">`On Error GoTo LabelName` 确定为最新的异常处理程序位置的标签。</span><span class="sxs-lookup"><span data-stu-id="6f08f-627">`On Error GoTo LabelName` establishes the label as the most recent exception-handler location.</span></span> <span data-ttu-id="6f08f-628">此语句不能包含 lambda 或查询表达式的方法中使用。</span><span class="sxs-lookup"><span data-stu-id="6f08f-628">This statement cannot be used in a method that contains a lambda or query expression.</span></span>

* <span data-ttu-id="6f08f-629">`On Error Resume Next` 建立`Resume Next`最新的异常处理程序位置的行为。</span><span class="sxs-lookup"><span data-stu-id="6f08f-629">`On Error Resume Next` establishes the `Resume Next` behavior as the most recent exception-handler location.</span></span>


#### <a name="resume-statement"></a><span data-ttu-id="6f08f-630">Resume 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-630">Resume Statement</span></span>

<span data-ttu-id="6f08f-631">一个`Resume`语句执行返回给导致的最新异常的语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-631">A `Resume` statement returns execution to the statement that caused the most recent exception.</span></span>

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

<span data-ttu-id="6f08f-632">如果`Next`指定修饰符，则执行将返回到会导致最新的异常的语句之后执行的语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-632">If the `Next` modifier is specified, execution returns to the statement that would have been executed after the statement that caused the most recent exception.</span></span> <span data-ttu-id="6f08f-633">如果指定标签名称，则执行将返回到标签。</span><span class="sxs-lookup"><span data-stu-id="6f08f-633">If a label name is specified, execution returns to the label.</span></span>

<span data-ttu-id="6f08f-634">因为`SyncLock`语句包含一个隐式结构化的错误处理块`Resume`并`Resume Next`具有特殊行为中发生的异常`SyncLock`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-634">Because the `SyncLock` statement contains an implicit structured error-handling block, `Resume` and `Resume Next` have special behaviors for exceptions that occur in `SyncLock` statements.</span></span> <span data-ttu-id="6f08f-635">`Resume` 执行返回给开头`SyncLock`语句，而`Resume Next`后面的下一个语句将执行返回`SyncLock`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-635">`Resume` returns execution to the beginning of the `SyncLock` statement, while `Resume Next` returns execution to the next statement following the `SyncLock` statement.</span></span> <span data-ttu-id="6f08f-636">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="6f08f-636">For example, consider the following code:</span></span>

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

<span data-ttu-id="6f08f-637">它将打印以下结果。</span><span class="sxs-lookup"><span data-stu-id="6f08f-637">It prints the following result.</span></span>

```
Before exception
Before exception
After SyncLock
```

<span data-ttu-id="6f08f-638">第一次`SyncLock`语句中，`Resume`执行返回给开头`SyncLock`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-638">The first time through the `SyncLock` statement, `Resume` returns execution to the beginning of the `SyncLock` statement.</span></span> <span data-ttu-id="6f08f-639">第二次遍历`SyncLock`语句中，`Resume Next`执行返回给末尾`SyncLock`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-639">The second time through the `SyncLock` statement, `Resume Next` returns execution to the end of the `SyncLock` statement.</span></span> <span data-ttu-id="6f08f-640">`Resume` 并`Resume Next`中不允许使用`SyncLock`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-640">`Resume` and `Resume Next` are not allowed within a `SyncLock` statement.</span></span>

<span data-ttu-id="6f08f-641">在所有情况下，当`Resume`执行语句，最新的异常将设置为`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-641">In all cases, when a `Resume` statement is executed, the most recent exception is set to `Nothing`.</span></span> <span data-ttu-id="6f08f-642">如果`Resume`会有任何最新异常执行语句，该语句将引发`System.Exception`异常包含 Visual Basic 错误号`20`（无错误恢复）。</span><span class="sxs-lookup"><span data-stu-id="6f08f-642">If a `Resume` statement is executed with no most recent exception, the statement raises a `System.Exception` exception containing the Visual Basic error number `20` (Resume without error).</span></span>


## <a name="branch-statements"></a><span data-ttu-id="6f08f-643">分支语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-643">Branch Statements</span></span>

<span data-ttu-id="6f08f-644">分支语句修改的方法中的执行流。</span><span class="sxs-lookup"><span data-stu-id="6f08f-644">Branch statements modify the flow of execution in a method.</span></span> <span data-ttu-id="6f08f-645">有六个分支语句：</span><span class="sxs-lookup"><span data-stu-id="6f08f-645">There are six branch statements:</span></span>

1. <span data-ttu-id="6f08f-646">一个`GoTo`语句会导致执行传递到方法中指定的标签。</span><span class="sxs-lookup"><span data-stu-id="6f08f-646">A `GoTo` statement causes execution to transfer to the specified label in the method.</span></span> <span data-ttu-id="6f08f-647">不允许向`GoTo`成`Try`， `Using`， `SyncLock`， `With`，`For`或`For Each`块中，也不到任何循环块，如果 lambda 或 LINQ 表达式中捕获本地变量的块。</span><span class="sxs-lookup"><span data-stu-id="6f08f-647">It is not allowed to `GoTo` into a `Try`, `Using`, `SyncLock`, `With`, `For` or `For Each` block, nor into any loop block if a local variable of that block is captured in a lambda or LINQ expression.</span></span>
2. <span data-ttu-id="6f08f-648">`Exit`语句将执行转移到的指定类型的直接包含的块语句结束后的下一个语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-648">An `Exit` statement transfers execution to the next statement after the end of the immediately containing block statement of the specified kind.</span></span> <span data-ttu-id="6f08f-649">如果块是方法的块，则控制流退出方法部分中所述[控制流](statements.md#control-flow)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-649">If the block is the method block, then control flow exits the method as described in Section [Control Flow](statements.md#control-flow).</span></span> <span data-ttu-id="6f08f-650">如果`Exit`语句不包含在指定在语句中，将发生编译时错误的块类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-650">If the `Exit` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
3. <span data-ttu-id="6f08f-651">一个`Continue`语句将执行转移到的指定类型的直接包含的块循环语句的末尾。</span><span class="sxs-lookup"><span data-stu-id="6f08f-651">A `Continue` statement transfers execution to the end of the immediately containing block loop statement of the specified kind.</span></span> <span data-ttu-id="6f08f-652">如果`Continue`语句不包含在指定在语句中，将发生编译时错误的块类型。</span><span class="sxs-lookup"><span data-stu-id="6f08f-652">If the `Continue` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
4. <span data-ttu-id="6f08f-653">一个`Stop`语句会导致调试器异常发生。</span><span class="sxs-lookup"><span data-stu-id="6f08f-653">A `Stop` statement causes a debugger exception to occur.</span></span>
5. <span data-ttu-id="6f08f-654">`End`语句将终止该程序。</span><span class="sxs-lookup"><span data-stu-id="6f08f-654">An `End` statement terminates the program.</span></span> <span data-ttu-id="6f08f-655">终结器运行之前关闭，但 finally 块的任何当前正在执行的`Try`不执行语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-655">Finalizers are run before shutdown, but the finally blocks of any currently executing `Try` statements are not executed.</span></span> <span data-ttu-id="6f08f-656">不可能在不是可执行文件 （例如 Dll） 的程序中使用此语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-656">This statement may not be used in programs that are not executable (for example, DLLs).</span></span>
6. <span data-ttu-id="6f08f-657">一个`Return`不包含表达式的语句是等效于`Exit Sub`或`Exit Function`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-657">A `Return` statement with no expression is equivalent to an `Exit Sub` or `Exit Function` statement.</span></span> <span data-ttu-id="6f08f-658">一个`Return`是一个函数的正则方法中仅允许使用表达式的语句或在异步方法的是具有返回类型的函数`Task(Of T)`某些`T`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-658">A `Return` statement with an expression is only allowed in a regular method that is a function, or in an async method that is a function with return type `Task(Of T)` for some `T`.</span></span> <span data-ttu-id="6f08f-659">表达式必须分类为一个值，该值是隐式转换为*函数返回变量*（在正则方法中） 或*任务返回变量*（在异步方法）.</span><span class="sxs-lookup"><span data-stu-id="6f08f-659">The expression must be classified as a value which is implicitly convertible to the *function return variable* (in the case of regular methods) or to the *task return variable* (in the case of async methods).</span></span> <span data-ttu-id="6f08f-660">其行为是评估其表达式，然后将其存储在返回的变量，然后执行隐式`Exit Function`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-660">Its behavior is to evaluate its expression, then store it in the return variable, then execute an implicit `Exit Function` statement.</span></span>

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

## <a name="array-handling-statements"></a><span data-ttu-id="6f08f-661">数组处理语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-661">Array-Handling Statements</span></span>

<span data-ttu-id="6f08f-662">两个语句简化与数组一起使用的工作：`ReDim`语句和`Erase`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-662">Two statements simplify working with arrays: `ReDim` statements and `Erase` statements.</span></span>

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a><span data-ttu-id="6f08f-663">ReDim 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-663">ReDim Statement</span></span>

<span data-ttu-id="6f08f-664">一个`ReDim`语句实例化新数组。</span><span class="sxs-lookup"><span data-stu-id="6f08f-664">A `ReDim` statement instantiates new arrays.</span></span>

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

<span data-ttu-id="6f08f-665">在语句中的每个子句必须分类为变量或属性访问其类型是数组类型或`Object`，并后跟一系列数组界限。</span><span class="sxs-lookup"><span data-stu-id="6f08f-665">Each clause in the statement must be classified as a variable or a property access whose type is an array type or `Object`, and be followed by a list of array bounds.</span></span> <span data-ttu-id="6f08f-666">边界数必须符合的类型的变量，例如：允许任意数量的边界`Object`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-666">The number of the bounds must be consistent with the type of the variable; any number of bounds is allowed for `Object`.</span></span> <span data-ttu-id="6f08f-667">在运行时，数组为每个表达式从左到右指定的边界内实例化，然后分配给变量或属性。</span><span class="sxs-lookup"><span data-stu-id="6f08f-667">At run time, an array is instantiated for each expression from left to right with the specified bounds and then assigned to the variable or property.</span></span> <span data-ttu-id="6f08f-668">如果该变量的类型是`Object`的维数是指定，维数的数组元素类型是`Object`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-668">If the variable type is `Object`, the number of dimensions is the number of dimensions specified, and the array element type is `Object`.</span></span> <span data-ttu-id="6f08f-669">如果给定的维度数与不兼容的变量或属性在运行时将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-669">If the given number of dimensions is incompatible with the variable or property at run time a compile-time error occurs.</span></span> <span data-ttu-id="6f08f-670">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-670">For example:</span></span>

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

<span data-ttu-id="6f08f-671">如果`Preserve`指定关键字，则表达式必须也是与可分类为一个值，且除最右侧的每个维度的新大小必须为现有数组的大小相同。</span><span class="sxs-lookup"><span data-stu-id="6f08f-671">If the `Preserve` keyword is specified, then the expressions must also be classifiable as a value, and the new size for each dimension except for the rightmost one must be the same as the size of the existing array.</span></span> <span data-ttu-id="6f08f-672">现有数组中的值复制到新数组： 如果较小的新数组，现有值将被丢弃;如果更大的新数组，额外的元素将初始化为数组的元素类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="6f08f-672">The values in the existing array are copied into the new array: if the new array is smaller, the existing values are discarded; if the new array is bigger, the extra elements will be initialized to the default value of the element type of the array.</span></span> <span data-ttu-id="6f08f-673">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="6f08f-673">For example, consider the following code:</span></span>

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

<span data-ttu-id="6f08f-674">它将打印以下结果：</span><span class="sxs-lookup"><span data-stu-id="6f08f-674">It prints the following result:</span></span>

```
3, 0
```

<span data-ttu-id="6f08f-675">如果现有数组引用 null 值在运行时，会给不出任何错误。</span><span class="sxs-lookup"><span data-stu-id="6f08f-675">If the existing array reference is a null value at run time, no error is given.</span></span> <span data-ttu-id="6f08f-676">最右边的维度，维度的大小发生更改，如果之外`System.ArrayTypeMismatchException`将引发。</span><span class="sxs-lookup"><span data-stu-id="6f08f-676">Other than the rightmost dimension, if the size of a dimension changes, a `System.ArrayTypeMismatchException` will be thrown.</span></span>

<span data-ttu-id="6f08f-677">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="6f08f-677">__Note.__</span></span> <span data-ttu-id="6f08f-678">`Preserve` 不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="6f08f-678">`Preserve` is not a reserved word.</span></span>


### <a name="erase-statement"></a><span data-ttu-id="6f08f-679">Erase 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-679">Erase Statement</span></span>

<span data-ttu-id="6f08f-680">`Erase`语句设置的每个数组变量或属性为语句中指定`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-680">An `Erase` statement sets each of the array variables or properties specified in the statement to `Nothing`.</span></span> <span data-ttu-id="6f08f-681">在语句中的每个表达式必须分类为其类型是数组类型的变量或属性访问或`Object`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-681">Each expression in the statement must be classified as a variable or property access whose type is an array type or `Object`.</span></span> <span data-ttu-id="6f08f-682">例如：</span><span class="sxs-lookup"><span data-stu-id="6f08f-682">For example:</span></span>

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

## <a name="using-statement"></a><span data-ttu-id="6f08f-683">Using 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-683">Using statement</span></span>

<span data-ttu-id="6f08f-684">运行一个集合并找到对实例的实时引用时，垃圾回收器自动释放类型的实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-684">Instances of types are automatically released by the garbage collector when a collection is run and no live references to the instance are found.</span></span> <span data-ttu-id="6f08f-685">如果类型为尤其有价值且稀缺资源 （如数据库连接或文件句柄） 上，它可能不需要等到下一步的垃圾回收，以清理不再使用的类型的特定实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-685">If a type holds on a particularly valuable and scarce resource (such as database connections or file handles), it may not be desirable to wait until the next garbage collection to clean up a particular instance of the type that is no longer in use.</span></span> <span data-ttu-id="6f08f-686">若要提供释放集合前的资源的一个简便的方法，一种类型可能会实现`System.IDisposable`接口。</span><span class="sxs-lookup"><span data-stu-id="6f08f-686">To provide a lightweight way of releasing resources before a collection, a type may implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="6f08f-687">公开的类型`Dispose`可以调用来强制立即，这种情况下释放资源有价值的方法：</span><span class="sxs-lookup"><span data-stu-id="6f08f-687">A type that does so exposes a `Dispose` method that can be called to force valuable resources to be released immediately, as such:</span></span>

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

<span data-ttu-id="6f08f-688">`Using`语句可获取资源、 执行一组语句，然后释放资源的过程进行自动化。</span><span class="sxs-lookup"><span data-stu-id="6f08f-688">The `Using` statement automates the process of acquiring a resource, executing a set of statements, and then disposing of the resource.</span></span> <span data-ttu-id="6f08f-689">该语句可以采用两种形式： 在其中一个，资源是本地变量声明语句的一部分为和视为常规的本地变量声明语句;中的其他资源是表达式的结果。</span><span class="sxs-lookup"><span data-stu-id="6f08f-689">The statement can take two forms: in one, the resource is a local variable declared as a part of the statement and treated as a regular local variable declaration statement; in the other, the resource is the result of an expression.</span></span>

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

<span data-ttu-id="6f08f-690">如果资源是本地变量声明语句则局部变量声明的类型必须可以隐式转换为类型`System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-690">If the resource is a local variable declaration statement then the type of the local variable declaration must be a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="6f08f-691">声明的局部变量是只读的作用域为`Using`语句块，并且必须包含初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="6f08f-691">The declared local variables are read-only, scoped to the `Using` statement block and must include an initializer.</span></span> <span data-ttu-id="6f08f-692">如果资源是表达式的结果，则该表达式必须分类为一个值，并且必须可以隐式转换为类型的`System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-692">If the resource is the result of an expression then the expression must be classified as a value and must be of a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="6f08f-693">在语句开始处仅一次，计算表达式。</span><span class="sxs-lookup"><span data-stu-id="6f08f-693">The expression is only evaluated once, at the beginning of the statement.</span></span>

<span data-ttu-id="6f08f-694">`Using`隐式包含块`Try`其 finally 块的语句调用的方法`IDisposable.Dispose`资源上。</span><span class="sxs-lookup"><span data-stu-id="6f08f-694">The `Using` block is implicitly contained by a `Try` statement whose finally block calls the method `IDisposable.Dispose` on the resource.</span></span> <span data-ttu-id="6f08f-695">这可确保即使在引发异常时释放资源。</span><span class="sxs-lookup"><span data-stu-id="6f08f-695">This ensures the resource is disposed even when an exception is thrown.</span></span> <span data-ttu-id="6f08f-696">结果是，它是无效的分支到`Using`阻止其块之外和一个`Using`块均被视为单个语句出于`Resume`和`Resume Next`。</span><span class="sxs-lookup"><span data-stu-id="6f08f-696">As a result, it is invalid to branch into a `Using` block from outside of the block, and a `Using` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="6f08f-697">如果资源是`Nothing`，则没有调用`Dispose`进行。</span><span class="sxs-lookup"><span data-stu-id="6f08f-697">If the resource is `Nothing`, then no call to `Dispose` is made.</span></span> <span data-ttu-id="6f08f-698">因此，下面的示例：</span><span class="sxs-lookup"><span data-stu-id="6f08f-698">Thus, the example:</span></span>

```vb
Using f As C = New C()
    ...
End Using
```

<span data-ttu-id="6f08f-699">等效于：</span><span class="sxs-lookup"><span data-stu-id="6f08f-699">is equivalent to:</span></span>

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

<span data-ttu-id="6f08f-700">一个`Using`具有的本地变量声明语句的语句可能获取多个资源，一次，这是相当于嵌套`Using`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-700">A `Using` statement that has a local variable declaration statement may acquire multiple resources at a time, which is equivalent to nested `Using` statements.</span></span>  <span data-ttu-id="6f08f-701">例如，`Using`窗体的语句：</span><span class="sxs-lookup"><span data-stu-id="6f08f-701">For example, a `Using` statement of the form:</span></span>

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

<span data-ttu-id="6f08f-702">等效于：</span><span class="sxs-lookup"><span data-stu-id="6f08f-702">is equivalent to:</span></span>

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a><span data-ttu-id="6f08f-703">Await 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-703">Await Statement</span></span>

<span data-ttu-id="6f08f-704">Await 语句具有与 await 运算符表达式相同的语法 (部分[Await 运算符](expressions.md#await-operator))，只能在还允许 await 表达式的方法中，并且具有与 await 运算符表达式相同的行为。</span><span class="sxs-lookup"><span data-stu-id="6f08f-704">An await statement has the same syntax as an await operator expression (Section [Await Operator](expressions.md#await-operator)), is allowed only in methods that also allow await expressions, and has the same behavior as an await operator expression.</span></span>

<span data-ttu-id="6f08f-705">但是，它会归类为值或 void。</span><span class="sxs-lookup"><span data-stu-id="6f08f-705">However, it may be classified as either a value or void.</span></span> <span data-ttu-id="6f08f-706">Await 运算符表达式计算得到的任何值将被放弃。</span><span class="sxs-lookup"><span data-stu-id="6f08f-706">Any value resulting from evaluation of the await operator expression is discarded.</span></span>

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a><span data-ttu-id="6f08f-707">Yield 语句</span><span class="sxs-lookup"><span data-stu-id="6f08f-707">Yield Statement</span></span>

<span data-ttu-id="6f08f-708">Yield 语句与迭代器方法，部分所述[迭代器方法](statements.md#iterator-methods)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-708">Yield statements are related to iterator methods, which are described in Section [Iterator Methods](statements.md#iterator-methods).</span></span>

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

<span data-ttu-id="6f08f-709">`Yield` 是一个保留的字，如果它在其中出现的立即封闭方法或 lambda 表达式具有`Iterator`修饰符，并且如果`Yield`后，将显示`Iterator`修饰符; 它在其他位置未保留空间。</span><span class="sxs-lookup"><span data-stu-id="6f08f-709">`Yield` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Iterator` modifier, and if the `Yield` appears after that `Iterator` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="6f08f-710">它也是在预处理器指令未保留空间。</span><span class="sxs-lookup"><span data-stu-id="6f08f-710">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="6f08f-711">Yield 语句仅允许在其所在的保留的字的方法或 lambda 表达式的正文中。</span><span class="sxs-lookup"><span data-stu-id="6f08f-711">The yield statement is only allowed in the body of a method or lambda expression where it is a reserved word.</span></span> <span data-ttu-id="6f08f-712">在最近的封闭方法或 lambda，yield 语句可能不会发生的表体内`Catch`或`Finally`块中，也不内部的正文`SyncLock`语句。</span><span class="sxs-lookup"><span data-stu-id="6f08f-712">Within the immediately enclosing method or lambda, the yield statement may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement.</span></span>

<span data-ttu-id="6f08f-713">Yield 语句采用单个表达式这必须分类为一个值，且其类型为隐式转换为的类型*迭代器当前变量*(部分[迭代器方法](statements.md#iterator-methods)) 的其封闭迭代器方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-713">The yield statement takes a single expression which must be classified as a value and whose type is implicitly convertible to the type of the *iterator current variable* (Section [Iterator Methods](statements.md#iterator-methods)) of its enclosing iterator method.</span></span>

<span data-ttu-id="6f08f-714">控制流只达到`Yield`语句时`MoveNext`迭代器对象上调用方法。</span><span class="sxs-lookup"><span data-stu-id="6f08f-714">Control flow only ever reaches a `Yield` statement when the `MoveNext` method is invoked on an iterator object.</span></span> <span data-ttu-id="6f08f-715">(这是因为迭代器方法实例只会执行其语句由于`MoveNext`或`Dispose`迭代器对象; 调用的方法和`Dispose`方法将只会执行中的代码`Finally`块，其中`Yield`不允许)。</span><span class="sxs-lookup"><span data-stu-id="6f08f-715">(This is because an iterator method instance only ever executes its statements due to the `MoveNext` or `Dispose` methods being called on an iterator object; and the `Dispose` method will only ever execute code in `Finally` blocks, where `Yield` is not allowed).</span></span>

<span data-ttu-id="6f08f-716">当`Yield`执行语句、 计算和存储在其表达式*迭代器当前变量*的与该迭代器对象关联的迭代器方法实例。</span><span class="sxs-lookup"><span data-stu-id="6f08f-716">When a `Yield` statement is executed, its expression is evaluated and stored in the *iterator current variable* of the iterator method instance associated with that iterator object.</span></span> <span data-ttu-id="6f08f-717">该值`True`返回到调用方`MoveNext`，和此实例的控制点停止之前的下一个调用前调`MoveNext`上迭代器对象。</span><span class="sxs-lookup"><span data-stu-id="6f08f-717">The value `True` is returned to the invoker of `MoveNext`, and the control point of this instance stops advancing until the next invocation of `MoveNext` on the iterator object.</span></span>

