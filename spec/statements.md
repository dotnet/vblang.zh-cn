---
ms.openlocfilehash: e49e116e60e724bcd8f1148c8aad9d11dfc92e74
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306059"
---
# <a name="statements"></a><span data-ttu-id="041a7-101">语句</span><span class="sxs-lookup"><span data-stu-id="041a7-101">Statements</span></span>

<span data-ttu-id="041a7-102">语句表示可执行代码。</span><span class="sxs-lookup"><span data-stu-id="041a7-102">Statements represent executable code.</span></span>

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

<span data-ttu-id="041a7-103">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-103">__Note.__</span></span> <span data-ttu-id="041a7-104">Microsoft Visual Basic 编译器仅允许以关键字或标识符开头的语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-104">The Microsoft Visual Basic Compiler only allows statements which start with a keyword or an identifier.</span></span> <span data-ttu-id="041a7-105">例如，允许调用语句 "`Call (Console).WriteLine`"，但调用语句 "`(Console).WriteLine`" 不是。</span><span class="sxs-lookup"><span data-stu-id="041a7-105">Thus, for instance, the invocation statement "`Call (Console).WriteLine`" is allowed, but the invocation statement "`(Console).WriteLine`" is not.</span></span>

## <a name="control-flow"></a><span data-ttu-id="041a7-106">控制流</span><span class="sxs-lookup"><span data-stu-id="041a7-106">Control Flow</span></span>

<span data-ttu-id="041a7-107">*控制流*是执行语句和表达式的顺序。</span><span class="sxs-lookup"><span data-stu-id="041a7-107">*Control flow* is the sequence in which statements and expressions are executed.</span></span> <span data-ttu-id="041a7-108">执行顺序取决于特定的语句或表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-108">The order of execution depends on the particular statement or expression.</span></span>

<span data-ttu-id="041a7-109">例如，在计算加法运算符（第[加法运算符](expressions.md#addition-operator)）时，首先计算左操作数，然后计算右操作数，然后计算运算符本身。</span><span class="sxs-lookup"><span data-stu-id="041a7-109">For example, when evaluating an addition operator (Section [Addition Operator](expressions.md#addition-operator)), first the left operand is evaluated, then the right operand, and then the operator itself.</span></span> <span data-ttu-id="041a7-110">块通过首先执行其第一个标签，然后通过块的语句逐个执行，来执行（部分[块和标签](statements.md#blocks-and-labels)）。</span><span class="sxs-lookup"><span data-stu-id="041a7-110">Blocks are executed (Section [Blocks and Labels](statements.md#blocks-and-labels)) by first executing their first substatement, and then proceeding one by one through the statements of the block.</span></span>

<span data-ttu-id="041a7-111">此顺序中的隐式是*控制点*的概念，即要执行的下一个操作。</span><span class="sxs-lookup"><span data-stu-id="041a7-111">Implicit in this ordering is the concept of a *control point*, which is the next operation to be executed.</span></span> <span data-ttu-id="041a7-112">如果调用了方法（或 "调用"），则我们说它创建了方法的*实例*。</span><span class="sxs-lookup"><span data-stu-id="041a7-112">When a method is invoked (or "called"), we say it creates an *instance* of the method.</span></span> <span data-ttu-id="041a7-113">方法实例由其自己的方法的参数、局部变量和其自己的控制点的副本组成。</span><span class="sxs-lookup"><span data-stu-id="041a7-113">A method instance consists of its own copy of the method's parameters and local variables, and its own control point.</span></span>

### <a name="regular-methods"></a><span data-ttu-id="041a7-114">常规方法</span><span class="sxs-lookup"><span data-stu-id="041a7-114">Regular Methods</span></span>

<span data-ttu-id="041a7-115">下面是常规方法的示例</span><span class="sxs-lookup"><span data-stu-id="041a7-115">Here is an example of a regular method</span></span>

```vb
Function Test() As Integer
    Console.WriteLine("hello")
    Return 1
End Sub

Dim x = Test()    ' invokes the function, prints "hello", assigns 1 to x
```

<span data-ttu-id="041a7-116">调用常规方法时，</span><span class="sxs-lookup"><span data-stu-id="041a7-116">When a regular method is invoked,</span></span>

1. <span data-ttu-id="041a7-117">首先，创建该方法的实例，该实例特定于调用。</span><span class="sxs-lookup"><span data-stu-id="041a7-117">First an instance of the method is created specific to that invocation.</span></span> <span data-ttu-id="041a7-118">此实例包含方法的所有参数和局部变量的副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-118">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="041a7-119">然后，将其所有参数初始化为提供的值，并将其所有局部变量初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="041a7-119">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="041a7-120">对于 `Function`，隐式局部变量也被初始化称为函数*返回变量*，其名称为函数的名称，其类型为函数的返回类型，其初始值为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="041a7-120">In the case of a `Function`, an implicit local variable is also initialized called the *function return variable* whose name is the function's name, whose type is the return type of the function and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="041a7-121">然后，在方法体的第一条语句处设置方法实例的控制点，方法体会立即开始从该位置执行（节[块和标签](statements.md#blocks-and-labels)）。</span><span class="sxs-lookup"><span data-stu-id="041a7-121">The method instance's control point is then set at the first statement of the method body, and the method body immediately starts to execute from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="041a7-122">当控制流在正常情况下退出方法体时，会通过到达 `End Function` 或 `End Sub` 标记其末尾，或者通过显式 `Return` 或 @no__t 的语句控制流返回到方法实例的调用方。</span><span class="sxs-lookup"><span data-stu-id="041a7-122">When control flow exits the method body normally - through reaching the `End Function` or `End Sub` that mark its end, or through an explicit `Return` or `Exit` statement - control flow returns to the caller of the method instance.</span></span> <span data-ttu-id="041a7-123">如果存在函数返回变量，则调用的结果是此变量的最终值。</span><span class="sxs-lookup"><span data-stu-id="041a7-123">If there is a function return variable, the result of the invocation is the final value of this variable.</span></span>

<span data-ttu-id="041a7-124">当控制流通过未经处理的异常退出方法体时，该异常会传播到调用方。</span><span class="sxs-lookup"><span data-stu-id="041a7-124">When control flow exits the method body through an unhandled exception, that exception is propagated to the caller.</span></span>

<span data-ttu-id="041a7-125">当控制流退出后，不再有任何对方法实例的实时引用。</span><span class="sxs-lookup"><span data-stu-id="041a7-125">After control flow has exited, there are no longer any live references to the method instance.</span></span> <span data-ttu-id="041a7-126">如果方法实例仅保留对其局部变量或参数副本的引用，则可能会对它们进行垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="041a7-126">If the method instance held the only references to its copy of local variables or parameters, then they may be garbage collected.</span></span>

### <a name="iterator-methods"></a><span data-ttu-id="041a7-127">迭代器方法</span><span class="sxs-lookup"><span data-stu-id="041a7-127">Iterator Methods</span></span>

<span data-ttu-id="041a7-128">迭代器方法用作生成序列的一种简便方法，可由 `For Each` 语句使用。</span><span class="sxs-lookup"><span data-stu-id="041a7-128">Iterator methods are used as a convenient way to generate a sequence, one which can be consumed by the `For Each` statement.</span></span> <span data-ttu-id="041a7-129">迭代器方法使用 `Yield` 语句（节[Yield 语句](statements.md#yield-statement)）来提供序列的元素。</span><span class="sxs-lookup"><span data-stu-id="041a7-129">Iterator methods use the `Yield` statement (Section [Yield Statement](statements.md#yield-statement)) to provide elements of the sequence.</span></span> <span data-ttu-id="041a7-130">（没有 `Yield` 语句的迭代器方法将生成一个空序列）。</span><span class="sxs-lookup"><span data-stu-id="041a7-130">(An iterator method with no `Yield` statements will produce an empty sequence).</span></span> <span data-ttu-id="041a7-131">下面是迭代器方法的示例：</span><span class="sxs-lookup"><span data-stu-id="041a7-131">Here is an example of an iterator method:</span></span>

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

<span data-ttu-id="041a7-132">如果调用的迭代器方法的返回类型为 `IEnumerator(Of T)`，</span><span class="sxs-lookup"><span data-stu-id="041a7-132">When an iterator method is invoked whose return type is `IEnumerator(Of T)`,</span></span>

1. <span data-ttu-id="041a7-133">首先，创建一个迭代器方法的实例，该实例特定于调用。</span><span class="sxs-lookup"><span data-stu-id="041a7-133">First an instance of the iterator method is created specific to that invocation.</span></span> <span data-ttu-id="041a7-134">此实例包含方法的所有参数和局部变量的副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-134">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="041a7-135">然后，将其所有参数初始化为提供的值，并将其所有局部变量初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="041a7-135">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="041a7-136">隐式局部变量也被初始化称为*迭代器当前变量*，该变量的类型为 `T`，其初始值为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="041a7-136">An implicit local variable is also initialized called the *iterator current variable*, whose type is `T` and whose initial value is the default of its type.</span></span>
4. <span data-ttu-id="041a7-137">然后，在方法体的第一条语句处设置方法实例的控制点。</span><span class="sxs-lookup"><span data-stu-id="041a7-137">The method instance's control point is then set at the first statement of the method body.</span></span>
5. <span data-ttu-id="041a7-138">然后创建一个*迭代器对象*，该对象与此方法实例相关联。</span><span class="sxs-lookup"><span data-stu-id="041a7-138">An *iterator object* is then created, associated with this method instance.</span></span> <span data-ttu-id="041a7-139">迭代器对象实现声明的返回类型，并具有如下所述的行为。</span><span class="sxs-lookup"><span data-stu-id="041a7-139">The iterator object implements the declared return type and has behavior as described below.</span></span>
6. <span data-ttu-id="041a7-140">然后，在调用方中*立即*恢复控制流，并且调用的结果是迭代器对象。</span><span class="sxs-lookup"><span data-stu-id="041a7-140">Control flow is then resumed *immediately* in the caller, and the result of the invocation is the iterator object.</span></span> <span data-ttu-id="041a7-141">请注意，此传输在不退出迭代器方法实例的情况下完成，并且不会导致执行 finally 处理程序。</span><span class="sxs-lookup"><span data-stu-id="041a7-141">Note that this transfer is done without exiting the iterator method instance, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="041a7-142">方法实例仍由迭代器对象引用，并且将不会对其进行垃圾回收，只要存在对迭代器对象的实时引用。</span><span class="sxs-lookup"><span data-stu-id="041a7-142">The method instance is still referenced by the iterator object, and will not be garbage collected so long as there exists a live reference to the iterator object.</span></span>

<span data-ttu-id="041a7-143">当访问迭代器对象的 `Current` 属性时，将返回调用的*当前变量*。</span><span class="sxs-lookup"><span data-stu-id="041a7-143">When the iterator object's `Current` property is accessed, the *current variable* of the invocation is returned.</span></span>

<span data-ttu-id="041a7-144">调用迭代器对象的 @no__t 0 方法时，调用不会创建新的方法实例。</span><span class="sxs-lookup"><span data-stu-id="041a7-144">When the iterator object's `MoveNext` method is invoked, the invocation does not create a new method instance.</span></span> <span data-ttu-id="041a7-145">而是使用现有的方法实例（及其控制点和局部变量和参数）-第一次调用迭代器方法时创建的实例。</span><span class="sxs-lookup"><span data-stu-id="041a7-145">Instead the existing method instance is used (and its control point and local variables and parameters) - the instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="041a7-146">控制流在该方法实例的控制点处继续执行，并按正常方式继续执行迭代器方法的正文。</span><span class="sxs-lookup"><span data-stu-id="041a7-146">Control flow resumes execution at the control point of that method instance, and proceeds through the body of the iterator method as normal.</span></span>

<span data-ttu-id="041a7-147">调用迭代器对象的 `Dispose` 方法时，将再次使用现有的方法实例。</span><span class="sxs-lookup"><span data-stu-id="041a7-147">When the iterator object's `Dispose` method is invoked, again the existing method instance is used.</span></span> <span data-ttu-id="041a7-148">控制流在该方法实例的控制点处恢复，但随后会立即表现为 `Exit Function` 语句是下一操作。</span><span class="sxs-lookup"><span data-stu-id="041a7-148">Control flow resumes at the control point of that method instance, but then immediately behaves as if an `Exit Function` statement were the next operation.</span></span>

<span data-ttu-id="041a7-149">如果对迭代器对象上的 `MoveNext` 或 @no__t 的以前的所有调用都已返回到其调用方，则在迭代器对象上调用 @no__t 0 或 `Dispose` 的上述行为说明仅适用于。</span><span class="sxs-lookup"><span data-stu-id="041a7-149">The above descriptions of behavior for invocation of `MoveNext` or `Dispose` on an iterator object only apply if all previous invocations of `MoveNext` or `Dispose` on that iterator object have already returned to their callers.</span></span> <span data-ttu-id="041a7-150">如果没有，则行为是不确定的。</span><span class="sxs-lookup"><span data-stu-id="041a7-150">If they haven't, then the behavior is undefined.</span></span>

<span data-ttu-id="041a7-151">当控制流在正常情况下退出迭代器方法体时--通过到达 `End Function` 来标记其结束，或者通过显式 `Return` 或 `Exit` 语句，必须在调用方的 `MoveNext` 或 `Dispose` 函数的上下文中执行此操作。要恢复迭代器方法实例的对象，并且该对象将使用首次调用迭代器方法时创建的方法实例。</span><span class="sxs-lookup"><span data-stu-id="041a7-151">When control flow exits the iterator method body normally -- through reaching the `End Function` that mark its end, or through an explicit `Return` or `Exit` statement -- it must have done so in the context of an invocation of `MoveNext` or `Dispose` function on an iterator object to resume the iterator method instance, and it will have been using the method instance that was created when the iterator method was first invoked.</span></span> <span data-ttu-id="041a7-152">该实例的控制点将保留在 `End Function` 语句上，并在调用方恢复控制流;如果已通过调用 `MoveNext` 恢复，则会将值 `False` 返回给调用方。</span><span class="sxs-lookup"><span data-stu-id="041a7-152">The control point of that instance is left at the `End Function` statement, and control flow resumes in the caller; and if it had been resumed by a call to `MoveNext` then the value `False` is returned to the caller.</span></span>

<span data-ttu-id="041a7-153">当控制流通过未经处理的异常退出迭代器方法体时，异常将传播到调用方，后者再次为 `MoveNext` 或 `Dispose` 的调用。</span><span class="sxs-lookup"><span data-stu-id="041a7-153">When control flow exits the iterator method body through an unhandled exception, then the exception is propagated to the caller, which again will be either an invocation of `MoveNext` or of `Dispose`.</span></span>

<span data-ttu-id="041a7-154">对于 iterator 函数的其他可能的返回类型，</span><span class="sxs-lookup"><span data-stu-id="041a7-154">As for the other possible return types of an iterator function,</span></span>

* <span data-ttu-id="041a7-155">调用迭代器方法时，如果调用的返回类型对某些 @no__t 为 @no__t 为0，则将首先创建一个实例，该实例特定于方法中所有参数的调用方方法，并使用提供的值对其进行初始化。</span><span class="sxs-lookup"><span data-stu-id="041a7-155">When an iterator method is invoked whose return type is `IEnumerable(Of T)` for some `T`, an instance is first created -- specific to that invocation of the iterator method -- of all parameters in the method, and they are initialized with the supplied values.</span></span> <span data-ttu-id="041a7-156">调用的结果是实现返回类型的对象。</span><span class="sxs-lookup"><span data-stu-id="041a7-156">The result of the invocation is an an object which implements the return type.</span></span> <span data-ttu-id="041a7-157">如果调用此对象的 @no__t 0 方法，则它将创建一个实例，该实例特定于该方法中所有参数和局部变量的调用 @no__t。</span><span class="sxs-lookup"><span data-stu-id="041a7-157">Should this object's `GetEnumerator` method be called, it creates an instance -- specific to that invocation of `GetEnumerator` -- of all parameters and local variables in the method.</span></span> <span data-ttu-id="041a7-158">它将参数初始化为已保存的值，并继续执行以上迭代器方法。</span><span class="sxs-lookup"><span data-stu-id="041a7-158">It initializes the parameters to the values already saved, and proceeds as for the iterator method above.</span></span>
* <span data-ttu-id="041a7-159">调用迭代器方法时，如果调用的返回类型为非泛型接口 `IEnumerator`，则该行为与 `IEnumerator(Of Object)` 的行为完全相同。</span><span class="sxs-lookup"><span data-stu-id="041a7-159">When an iterator method is invoked whose return type is the non-generic interface `IEnumerator`, the behavior is exactly as for `IEnumerator(Of Object)`.</span></span>
* <span data-ttu-id="041a7-160">调用迭代器方法时，如果调用的返回类型为非泛型接口 `IEnumerable`，则该行为与 `IEnumerable(Of Object)` 的行为完全相同。</span><span class="sxs-lookup"><span data-stu-id="041a7-160">When an iterator method is invoked whose return type is the non-generic interface `IEnumerable`, the behavior is exactly as for `IEnumerable(Of Object)`.</span></span>

### <a name="async-methods"></a><span data-ttu-id="041a7-161">异步方法</span><span class="sxs-lookup"><span data-stu-id="041a7-161">Async Methods</span></span>

<span data-ttu-id="041a7-162">异步方法是执行长时间运行的工作的一种简便方法，无需阻止应用程序的 UI。</span><span class="sxs-lookup"><span data-stu-id="041a7-162">Async methods are a convenient way to do long-running work without for example blocking the UI of an application.</span></span> <span data-ttu-id="041a7-163">异步是异步的*异步*方法，这意味着异步方法的调用方会立即继续执行，但异步方法的实例的最终完成可能会在将来的某个时间之后发生。</span><span class="sxs-lookup"><span data-stu-id="041a7-163">Async is short for *Asynchronous* - it means that the caller of the async method will resume its execution promptly, but the eventual completion of the async method's instance may happen at some later time in the future.</span></span> <span data-ttu-id="041a7-164">按照约定，异步方法用后缀 "Async" 命名。</span><span class="sxs-lookup"><span data-stu-id="041a7-164">By convention async methods are named with the suffix "Async".</span></span>

```vb
Async Function TestAsync() As Task(Of String)
    Console.WriteLine("hello")
    Await Task.Delay(100)
    Return "world"
End Function

Dim t = TestAsync()         ' prints "hello"
Console.WriteLine(Await t)  ' prints "world"
```

<span data-ttu-id="041a7-165">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-165">__Note.__</span></span> <span data-ttu-id="041a7-166">异步方法*不*会在后台线程上运行。</span><span class="sxs-lookup"><span data-stu-id="041a7-166">Async methods are *not* run on a background thread.</span></span> <span data-ttu-id="041a7-167">相反，它们允许方法通过 `Await` 运算符来挂起自身，并计划在响应某个事件时恢复自身。</span><span class="sxs-lookup"><span data-stu-id="041a7-167">Instead they allow a method to suspend itself through the `Await` operator, and schedule itself to be resumed in response to some event.</span></span>

<span data-ttu-id="041a7-168">调用 async 方法时</span><span class="sxs-lookup"><span data-stu-id="041a7-168">When an async method is invoked</span></span>

1. <span data-ttu-id="041a7-169">首先，创建一个异步方法的实例，该实例特定于调用。</span><span class="sxs-lookup"><span data-stu-id="041a7-169">First an instance of the async method is created specific to that invocation.</span></span> <span data-ttu-id="041a7-170">此实例包含方法的所有参数和局部变量的副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-170">This instance includes a copy of all parameters and local variables of the method.</span></span>
2. <span data-ttu-id="041a7-171">然后，将其所有参数初始化为提供的值，并将其所有局部变量初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="041a7-171">Then all of its parameters are initialized to the supplied values, and all of its local variables to the default values of their types.</span></span>
3. <span data-ttu-id="041a7-172">如果异步方法的返回类型为 `Task(Of T)` （对于某些 @no__t 为-1），则隐式局部变量也被初始化称为*任务返回变量*，该变量的类型为 `T`，其初始值为 `T` 的默认值。</span><span class="sxs-lookup"><span data-stu-id="041a7-172">In the case of an async method with return type `Task(Of T)` for some `T`, an implicit local variable is also initialized called the *task return variable*, whose type is `T` and whose initial value is the default of `T`.</span></span>
4. <span data-ttu-id="041a7-173">如果异步方法为 `Function`，并且返回类型为 `Task`，或 @no__t 为-2 （对于某些 `T`），则为与当前调用关联的、隐式创建的该类型的对象。</span><span class="sxs-lookup"><span data-stu-id="041a7-173">If the async method is a `Function` with return type `Task` or `Task(Of T)` for some `T`, then an object of that type implicitly created, associated with the current invocation.</span></span> <span data-ttu-id="041a7-174">这称为*async 对象*，表示将通过执行 async 方法的实例完成的未来工作。</span><span class="sxs-lookup"><span data-stu-id="041a7-174">This is called an *async object* and represents the future work that will be done by executing the instance of the async method.</span></span> <span data-ttu-id="041a7-175">当控件在此 async 方法实例的调用方中恢复时，调用方将接收此 async 对象作为其调用结果。</span><span class="sxs-lookup"><span data-stu-id="041a7-175">When control resumes in the caller of this async method instance, the caller will receive this async object as the result of its invocation.</span></span>
5. <span data-ttu-id="041a7-176">然后，在异步方法主体的第一条语句处设置实例的控制点，并立即开始从其中执行方法体（部分[块和标签](statements.md#blocks-and-labels)）。</span><span class="sxs-lookup"><span data-stu-id="041a7-176">The instance's control point is then set at the first statement of the async method body, and immediately starts to execute the method body from there (Section [Blocks and Labels](statements.md#blocks-and-labels)).</span></span>

<span data-ttu-id="041a7-177">__恢复委托和当前调用方__</span><span class="sxs-lookup"><span data-stu-id="041a7-177">__Resumption Delegate and Current Caller__</span></span>

<span data-ttu-id="041a7-178">如[Await 运算符](expressions.md#await-operator)部分中所述，执行 @no__t 1 表达式可以挂起方法实例的控制点，使控制流离开其他位置。</span><span class="sxs-lookup"><span data-stu-id="041a7-178">As detailed in Section [Await Operator](expressions.md#await-operator), execution of an `Await` expression has the ability to suspend the method instance's control point leaving control flow to go elsewhere.</span></span> <span data-ttu-id="041a7-179">稍后，控制流可以通过调用*恢复委托*在同一实例的控制点上恢复。</span><span class="sxs-lookup"><span data-stu-id="041a7-179">Control flow can later resume at the same instance's control point through invocation of a *resumption delegate*.</span></span> <span data-ttu-id="041a7-180">请注意，此挂起在不退出 async 方法的情况下完成，并且不会导致执行 finally 处理程序。</span><span class="sxs-lookup"><span data-stu-id="041a7-180">Note that this suspension is done without exiting the async method, and does not cause finally handlers to execute.</span></span> <span data-ttu-id="041a7-181">此方法实例仍由恢复委托和 `Task` 或 @no__t 结果（如果有）引用，并且将不会进行垃圾回收，这是因为存在对委托或结果的实时引用。</span><span class="sxs-lookup"><span data-stu-id="041a7-181">The method instance is still referenced by both the resumption delegate and the `Task` or `Task(Of T)` result (if any), and will not be garbage collected so long as there exists a live reference to either delegate or result.</span></span>

<span data-ttu-id="041a7-182">假设语句 `Dim x = Await WorkAsync()`，这一点对于以下情况很有帮助：</span><span class="sxs-lookup"><span data-stu-id="041a7-182">It is helpful to imagine the statement `Dim x = Await WorkAsync()` approximately as syntactic shorthand for the following:</span></span>

```vb
Dim temp = WorkAsync().GetAwaiter()
If Not temp.IsCompleted Then
       temp.OnCompleted(resumptionDelegate)
       Return [task]
       CONT:   ' invocation of 'resumptionDelegate' will resume here
End If
Dim x = temp.GetResult()
```

<span data-ttu-id="041a7-183">在下面的示例中，方法实例的*当前调用方*定义为原始调用方或恢复委托的调用方，两者都是最新的。</span><span class="sxs-lookup"><span data-stu-id="041a7-183">In the following, the *current caller* of the method instance is defined as either the original caller, or the caller of the resumption delegate, whichever is more recent.</span></span>

<span data-ttu-id="041a7-184">当控制流退出 async 方法体时--通过到达 `End Sub` 或 `End Function` 标记其结束时，或者通过显式的 `Return` 或 `Exit` 语句或未经处理的异常，将实例的控制点设置为方法的末尾。</span><span class="sxs-lookup"><span data-stu-id="041a7-184">When control flow exits the async method body -- through reaching the `End Sub` or `End Function` that mark its end, or through an explicit `Return` or `Exit` statement, or through an unhandled exception -- the instance's control point is set to the end of the method.</span></span> <span data-ttu-id="041a7-185">然后，行为取决于异步方法的返回类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-185">Behavior then depends on the return type of the async method.</span></span>

* <span data-ttu-id="041a7-186">对于返回类型为 @no__t 的 @no__t 0：</span><span class="sxs-lookup"><span data-stu-id="041a7-186">In the case of an `Async Function` with return type `Task`:</span></span>
  1. <span data-ttu-id="041a7-187">如果控制流通过未经处理的异常退出，则异步对象的状态将设置为 `TaskStatus.Faulted`，其 @no__t 属性设置为异常（除外：某些实现定义的异常，如 `OperationCanceledException` 将其更改为 `TaskStatus.Canceled`）。</span><span class="sxs-lookup"><span data-stu-id="041a7-187">If control flow exits through an unhandled exception, then the async object's status is set to `TaskStatus.Faulted` and its `Exception.InnerException` property is set to the exception (except: certain implementation-defined exceptions such as `OperationCanceledException` change it to `TaskStatus.Canceled`).</span></span> <span data-ttu-id="041a7-188">当前调用方中的控制流恢复。</span><span class="sxs-lookup"><span data-stu-id="041a7-188">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="041a7-189">否则，async 对象的状态将设置为 `TaskStatus.Completed`。</span><span class="sxs-lookup"><span data-stu-id="041a7-189">Otherwise, the async object's status is set to `TaskStatus.Completed`.</span></span> <span data-ttu-id="041a7-190">当前调用方中的控制流恢复。</span><span class="sxs-lookup"><span data-stu-id="041a7-190">Control flow resumes in the current caller.</span></span>

     <span data-ttu-id="041a7-191">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="041a7-191">(__Note.__</span></span> <span data-ttu-id="041a7-192">整个任务点以及异步方法的含义是什么，这是因为当任务完成时，任何等待它的方法都将执行其恢复委托，即，它们将被取消阻止。）</span><span class="sxs-lookup"><span data-stu-id="041a7-192">The whole point of Task, and what makes async methods interesting, is that when a task becomes Completed then any methods that were awaiting it will presently have their resumption delegates executed, i.e. they will become unblocked.)</span></span>

* <span data-ttu-id="041a7-193">对于某些 `T` 的返回类型为 @no__t 的 @no__t 0：行为如上所示，除了在非异常情况下，async 对象的 `Result` 属性还设置为任务返回变量的最终值。</span><span class="sxs-lookup"><span data-stu-id="041a7-193">In the case of an `Async Function` with return type `Task(Of T)` for some `T`: the behavior is as above, except that in non-exception cases the async object's `Result` property is also set to the final value of the task return variable.</span></span>

* <span data-ttu-id="041a7-194">对于 `Async Sub`：</span><span class="sxs-lookup"><span data-stu-id="041a7-194">In the case of an `Async Sub`:</span></span>
  1. <span data-ttu-id="041a7-195">如果控制流在未经处理的异常中退出，则该异常将以某种特定于实现的方式传播到环境。</span><span class="sxs-lookup"><span data-stu-id="041a7-195">If control flow exits through an unhandled exception, then that exception is propagated to the environment in some implementation-specific manner.</span></span> <span data-ttu-id="041a7-196">当前调用方中的控制流恢复。</span><span class="sxs-lookup"><span data-stu-id="041a7-196">Control flow resumes in the current caller.</span></span>
  2. <span data-ttu-id="041a7-197">否则，控制流只需在当前调用方中恢复即可。</span><span class="sxs-lookup"><span data-stu-id="041a7-197">Otherwise, control flow simply resumes in the current caller.</span></span>

#### <a name="async-sub"></a><span data-ttu-id="041a7-198">Async Sub</span><span class="sxs-lookup"><span data-stu-id="041a7-198">Async Sub</span></span>

<span data-ttu-id="041a7-199">有一些特定于 Microsoft 的 @no__t 的行为。</span><span class="sxs-lookup"><span data-stu-id="041a7-199">There is some Microsoft-specific behavior of an `Async Sub`.</span></span>

<span data-ttu-id="041a7-200">如果在调用开始时 `SynchronizationContext.Current` @no__t 为-1，则异步 Sub 中的任何未经处理的异常都将发布到 Threadpool。</span><span class="sxs-lookup"><span data-stu-id="041a7-200">If `SynchronizationContext.Current` is `Nothing` at the start of the invocation, then any unhandled exceptions from an Async Sub will be posted to the Threadpool.</span></span>

<span data-ttu-id="041a7-201">如果在调用开始时 `SynchronizationContext.Current` 不 `Nothing`，则在该方法开始之前，在该上下文上调用 @no__t，并在结束后 @no__t 为3。</span><span class="sxs-lookup"><span data-stu-id="041a7-201">If `SynchronizationContext.Current` is not `Nothing` at the start of the invocation, then `OperationStarted()` is invoked on that context before the start of the method and `OperationCompleted()` after the end.</span></span> <span data-ttu-id="041a7-202">此外，将会发布在同步上下文中重新引发的任何未经处理的异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-202">Additionally, any unhandled exceptions will be posted to be rethrown on the synchronization context.</span></span>

<span data-ttu-id="041a7-203">这意味着，在 UI 应用程序中，对于在 UI 线程上调用的 @no__t 0，它无法处理的任何异常都将 reposted UI 线程。</span><span class="sxs-lookup"><span data-stu-id="041a7-203">This means that in UI applications, for an `Async Sub` that is invoked on the UI thread, any exceptions it fails to handle will be reposted the UI thread.</span></span>

#### <a name="mutable-structures-in-async-and-iterator-methods"></a><span data-ttu-id="041a7-204">异步和迭代器方法中的可变结构</span><span class="sxs-lookup"><span data-stu-id="041a7-204">Mutable structures in async and iterator methods</span></span>

<span data-ttu-id="041a7-205">可变结构一般被视为错误的做法，异步或迭代器方法不支持。</span><span class="sxs-lookup"><span data-stu-id="041a7-205">Mutable structures in general are considered bad practice, and they are not supported by async or iterator methods.</span></span> <span data-ttu-id="041a7-206">特别是，结构中的异步或迭代器方法的每个调用都将隐式地在调用时复制的该结构的*副本*上运行。</span><span class="sxs-lookup"><span data-stu-id="041a7-206">In particular, each invocation of an async or iterator method in a structure will implicitly operate on a *copy* of that structure that is copied at its moment of invocation.</span></span> <span data-ttu-id="041a7-207">例如，</span><span class="sxs-lookup"><span data-stu-id="041a7-207">Thus, for example,</span></span>

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

### <a name="blocks-and-labels"></a><span data-ttu-id="041a7-208">块和标签</span><span class="sxs-lookup"><span data-stu-id="041a7-208">Blocks and Labels</span></span>

<span data-ttu-id="041a7-209">一组可执行语句称为语句块。</span><span class="sxs-lookup"><span data-stu-id="041a7-209">A group of executable statements is called a statement block.</span></span> <span data-ttu-id="041a7-210">语句块的执行从块中的第一个语句开始。</span><span class="sxs-lookup"><span data-stu-id="041a7-210">Execution of a statement block begins with the first statement in the block.</span></span> <span data-ttu-id="041a7-211">执行语句后，将执行词法顺序中的下一条语句，除非语句将执行转移到其他位置或发生异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-211">Once a statement has been executed, the next statement in lexical order is executed, unless a statement transfers execution elsewhere or an exception occurs.</span></span>

<span data-ttu-id="041a7-212">在语句块中，对逻辑行的语句相除对于标签声明语句除外是不重要的。</span><span class="sxs-lookup"><span data-stu-id="041a7-212">Within a statement block, the division of statements on logical lines is not significant with the exception of label declaration statements.</span></span> <span data-ttu-id="041a7-213">标签是标识语句块内特定位置的标识符，该语句块可用作分支语句的目标，例如 `GoTo`。</span><span class="sxs-lookup"><span data-stu-id="041a7-213">A label is an identifier that identifies a particular position within the statement block that can be used as the target of a branch statement such as `GoTo`.</span></span>

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


<span data-ttu-id="041a7-214">标签声明语句必须出现在逻辑行的开头，并且标签可以是标识符或整数文本。</span><span class="sxs-lookup"><span data-stu-id="041a7-214">Label declaration statements must appear at the beginning of a logical line and labels may be either an identifier or an integer literal.</span></span> <span data-ttu-id="041a7-215">由于标签声明语句和调用语句都可以包含单个标识符，因此，本地行开头的单个标识符始终被视为标签声明语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-215">Because both label declaration statements and invocation statements can consist of a single identifier, a single identifier at the beginning of a local line is always considered a label declaration statement.</span></span> <span data-ttu-id="041a7-216">标签声明语句必须始终后跟一个冒号，即使在同一逻辑行上没有语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-216">Label declaration statements must always be followed by a colon, even if no statements follow on the same logical line.</span></span>

<span data-ttu-id="041a7-217">标签具有自己的声明空间，不会干扰其他标识符。</span><span class="sxs-lookup"><span data-stu-id="041a7-217">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="041a7-218">下面的示例是有效的，并且使用名称变量 `x` 均作为参数，并使用作为标签。</span><span class="sxs-lookup"><span data-stu-id="041a7-218">The following example is valid and uses the name variable `x` both as a parameter and as a label.</span></span>

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

<span data-ttu-id="041a7-219">标签的作用域是包含它的方法的正文。</span><span class="sxs-lookup"><span data-stu-id="041a7-219">The scope of a label is the body of the method containing it.</span></span>

<span data-ttu-id="041a7-220">为方便起见，在此规范中，涉及多个 substatements 的语句生产将被视为单个生产，即使 substatements 各自在标记的行中也是如此。</span><span class="sxs-lookup"><span data-stu-id="041a7-220">For the sake of readability, statement productions that involve multiple substatements are treated as a single production in this specification, even though the substatements may each be by themselves on a labeled line.</span></span>


### <a name="local-variables-and-parameters"></a><span data-ttu-id="041a7-221">局部变量和参数</span><span class="sxs-lookup"><span data-stu-id="041a7-221">Local Variables and Parameters</span></span>

<span data-ttu-id="041a7-222">前面几节详细说明了如何和何时创建方法实例，以及如何将方法实例与方法的局部变量和参数一起使用。</span><span class="sxs-lookup"><span data-stu-id="041a7-222">The preceding sections detail how and when method instances are created, and with them the copies of a method's local variables and parameters.</span></span> <span data-ttu-id="041a7-223">此外，每次输入循环的主体时，都会在该循环中声明的每个局部变量（如 "[循环语句](statements.md#loop-statements)" 一节中所述）创建一个新的副本，并且该方法实例现在包含其本地变量（而不是以前的副本）的此副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-223">In addition, each time the body of a loop is entered, a new copy is made of each local variable declared inside that loop as described in Section [Loop Statements](statements.md#loop-statements), and the method instance now contains this copy of its local variable rather than the previous copy.</span></span>

<span data-ttu-id="041a7-224">所有局部变量都初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="041a7-224">All locals are initialized to their type's default value.</span></span> <span data-ttu-id="041a7-225">局部变量和参数始终可公开访问。</span><span class="sxs-lookup"><span data-stu-id="041a7-225">Local variables and parameters are always publicly accessible.</span></span> <span data-ttu-id="041a7-226">在其声明之前的文本位置引用本地变量是错误的，如下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="041a7-226">It is an error to refer to a local variable in a textual position that precedes its declaration, as the following example illustrates:</span></span>

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

<span data-ttu-id="041a7-227">在上面的 `F` 方法中，第一次分配给 @no__t 的第1项并未引用在外部作用域中声明的字段。</span><span class="sxs-lookup"><span data-stu-id="041a7-227">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="041a7-228">相反，它引用局部变量，并且它是错误的，因为它在此变量的声明之前位于变量之前。</span><span class="sxs-lookup"><span data-stu-id="041a7-228">Rather, it refers to the local variable, and it is in error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="041a7-229">在 `G` 方法中，后面的变量声明引用在同一局部变量声明内的早期变量声明中声明的局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-229">In the `G` method, a subsequent variable declaration refers to a local variable declared in an earlier variable declaration within the same local variable declaration.</span></span>

<span data-ttu-id="041a7-230">方法中的每个块都为局部变量创建声明空间。</span><span class="sxs-lookup"><span data-stu-id="041a7-230">Each block in a method creates a declaration space for local variables.</span></span> <span data-ttu-id="041a7-231">名称通过方法体中的局部变量声明和方法的参数列表引入到此声明空间，该方法将名称引入最外层块的声明空间。</span><span class="sxs-lookup"><span data-stu-id="041a7-231">Names are introduced into this declaration space through local variable declarations in the method body and through the parameter list of the method, which introduces names into the outermost block's declaration space.</span></span> <span data-ttu-id="041a7-232">块不允许通过嵌套隐藏名称：在块中声明名称后，可能不会在任何嵌套块中重新声明该名称。</span><span class="sxs-lookup"><span data-stu-id="041a7-232">Blocks do not allow shadowing of names through nesting: once a name has been declared in a block, the name may not be redeclared in any nested blocks.</span></span>

<span data-ttu-id="041a7-233">因此，在下面的示例中，由于名称 `i` 是在外部块中声明的，因此不能在内部块中重新声明，因此在下面的示例中，`F` 和 @no__t 方法是错误的。</span><span class="sxs-lookup"><span data-stu-id="041a7-233">Thus, in the following example, the `F` and `G` methods are in error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="041a7-234">但 `H` 和 @no__t 1 方法是有效的，因为这两个 @no__t 在单独的非嵌套块中声明。</span><span class="sxs-lookup"><span data-stu-id="041a7-234">However, the `H` and `I` methods are valid because the two `i`'s are declared in separate non-nested blocks.</span></span>

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

<span data-ttu-id="041a7-235">如果该方法是一个函数，则在方法体的声明空间中隐式声明一个特殊的局部变量，该局部变量的名称与表示该函数的返回值的方法的名称相同。</span><span class="sxs-lookup"><span data-stu-id="041a7-235">When the method is a function, a special local variable is implicitly declared in the method body's declaration space with the same name as the method representing the return value of the function.</span></span> <span data-ttu-id="041a7-236">在表达式中使用时，局部变量具有特殊名称解析语义。</span><span class="sxs-lookup"><span data-stu-id="041a7-236">The local variable has special name resolution semantics when used in expressions.</span></span> <span data-ttu-id="041a7-237">如果在需要将表达式归类为方法组的上下文（如调用表达式）中使用本地变量，则该名称将解析为函数而不是局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-237">If the local variable is used in a context that expects an expression classified as a method group, such as an invocation expression, then the name resolves to the function rather than to the local variable.</span></span> <span data-ttu-id="041a7-238">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-238">For example:</span></span>

```vb
Function F(i As Integer) As Integer
    If i = 0 Then
        F = 1        ' Sets the return value.
    Else
        F = F(i - 1) ' Recursive call.
    End If
End Function
```

<span data-ttu-id="041a7-239">使用括号可能导致不明确的情况（例如 `F(1)`，其中 `F` 是返回类型为一维数组的函数）;在所有不明确的情况下，名称解析为函数而不是局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-239">The use of parentheses can cause ambiguous situations (such as `F(1)`, where `F` is a function whose return type is a one-dimensional array); in all ambiguous situations, the name resolves to the function rather than the local variable.</span></span> <span data-ttu-id="041a7-240">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-240">For example:</span></span>

```vb
Function F(i As Integer) As Integer()
    If i = 0 Then
        F = new Integer(2) { 1, 2, 3 }
    Else
        F = F(i - 1) ' Recursive call, not an index.
    End If
End Function
```

<span data-ttu-id="041a7-241">当控制流离开方法体时，本地变量的值将传递回调用表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-241">When control flow leaves the method body, the value of the local variable is passed back to the invocation expression.</span></span> <span data-ttu-id="041a7-242">如果该方法是一个子例程，则没有此类隐式局部变量，并且控件只返回到调用表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-242">If the method is a subroutine, there is no such implicit local variable, and control simply returns to the invocation expression.</span></span>

## <a name="local-declaration-statements"></a><span data-ttu-id="041a7-243">局部声明语句</span><span class="sxs-lookup"><span data-stu-id="041a7-243">Local Declaration Statements</span></span>

<span data-ttu-id="041a7-244">局部声明语句声明一个新的局部变量、局部常量或静态变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-244">A local declaration statement declares a new local variable, local constant, or static variable.</span></span> <span data-ttu-id="041a7-245">*局部变量*和*本地常量*等效于作用域为方法的实例变量和常量，并以相同的方式进行声明。</span><span class="sxs-lookup"><span data-stu-id="041a7-245">*Local variables* and *local constants* are equivalent to instance variables and constants scoped to the method and are declared in the same way.</span></span> <span data-ttu-id="041a7-246">*静态变量*与 `Shared` 变量类似，并且是使用 @no__t 2 修饰符声明的。</span><span class="sxs-lookup"><span data-stu-id="041a7-246">*Static variables* are similar to `Shared` variables and are declared using the `Static` modifier.</span></span>

```antlr
LocalDeclarationStatement
    : LocalModifier VariableDeclarators StatementTerminator
    ;

LocalModifier
    : 'Static' | 'Dim' | 'Const'
    ;
```

<span data-ttu-id="041a7-247">静态变量是在方法调用之间保留其值的局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-247">Static variables are locals that retain their value across invocations of the method.</span></span> <span data-ttu-id="041a7-248">非共享方法中声明的静态变量是每个实例的：包含该方法的类型的每个实例都有自己的静态变量副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-248">Static variables declared within non-shared methods are per instance: each instance of the type that contains the method has its own copy of the static variable.</span></span> <span data-ttu-id="041a7-249">在 `Shared` 方法中声明的静态变量是按类型进行的;所有实例只有一个静态变量副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-249">Static variables declared within `Shared` methods are per type; there is only one copy of the static variable for all instances.</span></span> <span data-ttu-id="041a7-250">在方法中，将局部变量初始化为其类型的默认值，而在初始化类型或类型实例时，静态变量将初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="041a7-250">While local variables are initialized to their type's default value upon each entry into the method, static variables are only initialized to their type's default value when the type or type instance is initialized.</span></span> <span data-ttu-id="041a7-251">静态变量不能在结构或泛型方法中声明。</span><span class="sxs-lookup"><span data-stu-id="041a7-251">Static variables may not be declared in structures or generic methods.</span></span>

<span data-ttu-id="041a7-252">局部变量、本地常量和静态变量始终具有公共可访问性，并且不能指定可访问性修饰符。</span><span class="sxs-lookup"><span data-stu-id="041a7-252">Local variables, local constants, and static variables always have public accessibility and may not specify accessibility modifiers.</span></span> <span data-ttu-id="041a7-253">如果未在局部声明语句上指定类型，则以下步骤将确定局部声明的类型：</span><span class="sxs-lookup"><span data-stu-id="041a7-253">If no type is specified on a local declaration statement, then the following steps determine the type of the local declaration:</span></span>

1. <span data-ttu-id="041a7-254">如果声明具有类型字符，则类型字符的类型是局部声明的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-254">If the declaration has a type character, the type of the type character is the type of the local declaration.</span></span>

2. <span data-ttu-id="041a7-255">如果本地声明为本地常量，或者本地声明是使用初始值设定项的局部变量，并且使用的是局部变量类型推理，则本地声明的类型将从初始值设定项的类型中推断出来。</span><span class="sxs-lookup"><span data-stu-id="041a7-255">If the local declaration is a local constant, or if the local declaration is a local variable with an initializer and local variable type inference is being used, the type of the local declaration is inferred from the type of the initializer.</span></span> <span data-ttu-id="041a7-256">如果初始化表达式引用本地声明，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-256">If the initializer refers to the local declaration, a compile-time error occurs.</span></span> <span data-ttu-id="041a7-257">（本地常量需要有初始值设定项。）</span><span class="sxs-lookup"><span data-stu-id="041a7-257">(Local constants are required to have initializers.)</span></span>

3. <span data-ttu-id="041a7-258">如果未使用 strict 语义，则局部声明语句的类型将隐式 `Object`。</span><span class="sxs-lookup"><span data-stu-id="041a7-258">If strict semantics are not being used, the type of the local declaration statement is implicitly `Object`.</span></span>

4. <span data-ttu-id="041a7-259">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-259">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="041a7-260">如果未在具有数组大小或数组类型修饰符的本地声明语句上指定类型，则本地声明的类型是具有指定秩的数组，而前面的步骤用于确定数组的元素类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-260">If no type is specified on a local declaration statement that has an array size or array type modifier, then the type of the local declaration is an array with the specified rank and the previous steps are used to determine the element type of the array.</span></span> <span data-ttu-id="041a7-261">如果使用局部变量类型推理，则初始值设定项的类型必须是具有相同数组形状（即数组类型修饰符）作为局部声明语句的数组类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-261">If local variable type inference is used, the type of the initializer must be an array type with the same array shape (i.e. array type modifiers) as the local declaration statement.</span></span> <span data-ttu-id="041a7-262">请注意，推断元素类型可能仍为数组类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-262">Note that it is possible that the inferred element type may still be an array type.</span></span> <span data-ttu-id="041a7-263">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-263">For example:</span></span>

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

<span data-ttu-id="041a7-264">如果在具有可为 null 的类型修饰符的局部声明语句上未指定任何类型，则本地声明的类型为推断类型的可为 null 的版本，如果已为 null 的值类型，则为推断类型本身。</span><span class="sxs-lookup"><span data-stu-id="041a7-264">If no type is specified on a local declaration statement that has a nullable type modifier, then the type of the local declaration is the nullable version of the inferred type or the inferred type itself if it a nullable value type already.</span></span>  <span data-ttu-id="041a7-265">如果推断类型不是可以为 null 的值类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-265">If the inferred type is not a value type that can be made nullable, a compile-time error occurs.</span></span> <span data-ttu-id="041a7-266">如果可为 null 的类型修饰符和数组大小或数组类型修饰符都放置在没有类型的局部声明语句上，则将可为 null 的类型修饰符应用于数组的元素类型，并使用前面的步骤来确定包含nt 类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-266">If both a nullable type modifier and an array size or array type modifier are placed on a local declaration statement with no type, then the nullable type modifier is considered to apply to the element type of the array and the previous steps are used to determine the element type.</span></span>

<span data-ttu-id="041a7-267">局部声明语句中的变量初始值设定项等效于放置在声明的文本位置的赋值语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-267">Variable initializers on local declaration statements are equivalent to assignment statements placed at the textual location of the declaration.</span></span> <span data-ttu-id="041a7-268">因此，如果执行分支在局部声明语句上，则不会执行变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="041a7-268">Thus, if execution branches over the local declaration statement, the variable initializer is not executed.</span></span> <span data-ttu-id="041a7-269">如果多次执行局部声明语句，则执行变量初始值设定项的次数相等。</span><span class="sxs-lookup"><span data-stu-id="041a7-269">If the local declaration statement is executed more than once, the variable initializer is executed an equal number of times.</span></span> <span data-ttu-id="041a7-270">静态变量首次只执行其初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="041a7-270">Static variables only execute their initializer the first time.</span></span> <span data-ttu-id="041a7-271">如果在初始化静态变量时发生异常，则会将静态变量视为使用静态变量类型的默认值初始化。</span><span class="sxs-lookup"><span data-stu-id="041a7-271">If an exception occurs while initializing a static variable, the static variable is considered initialized with the default value of the static variable's type.</span></span>

<span data-ttu-id="041a7-272">下面的示例演示如何使用初始值设定项：</span><span class="sxs-lookup"><span data-stu-id="041a7-272">The following example shows the use of initializers:</span></span>

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

<span data-ttu-id="041a7-273">此程序将打印：</span><span class="sxs-lookup"><span data-stu-id="041a7-273">This program prints:</span></span>

```console
Static variable x = 5
Static variable x = 6
Static variable x = 7
Local variable y = 8
Local variable y = 8
Local variable y = 8
```

<span data-ttu-id="041a7-274">静态局部变量上的初始值设定项在初始化期间是线程安全的，并受其保护。</span><span class="sxs-lookup"><span data-stu-id="041a7-274">Initializers on static locals are thread-safe and protected against exceptions during initialization.</span></span> <span data-ttu-id="041a7-275">如果在静态局部初始值设定项期间发生异常，则静态局部变量将具有其默认值，而不会进行初始化。</span><span class="sxs-lookup"><span data-stu-id="041a7-275">If an exception occurs during a static local initializer, the static local will have its default value and not be initialized.</span></span> <span data-ttu-id="041a7-276">静态局部初始值设定项</span><span class="sxs-lookup"><span data-stu-id="041a7-276">A static local initializer</span></span>

```vb
Module Test
    Sub F()
        Static x As Integer = 5
    End Sub
End Module
```

<span data-ttu-id="041a7-277">等效于</span><span class="sxs-lookup"><span data-stu-id="041a7-277">is equivalent to</span></span>

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

<span data-ttu-id="041a7-278">局部变量、本地常量和静态变量的范围限定为在其中声明它们的语句块。</span><span class="sxs-lookup"><span data-stu-id="041a7-278">Local variables, local constants, and static variables are scoped to the statement block in which they are declared.</span></span> <span data-ttu-id="041a7-279">静态变量特别适用于其名称在整个方法中只能使用一次。</span><span class="sxs-lookup"><span data-stu-id="041a7-279">Static variables are special in that their names may only be used once throughout the entire method.</span></span> <span data-ttu-id="041a7-280">例如，如果两个静态变量声明的名称相同，即使它们在不同的块中，这是无效的。</span><span class="sxs-lookup"><span data-stu-id="041a7-280">For example, it is not valid to specify two static variable declarations with the same name even if they are in different blocks.</span></span>


### <a name="implicit-local-declarations"></a><span data-ttu-id="041a7-281">隐式局部声明</span><span class="sxs-lookup"><span data-stu-id="041a7-281">Implicit Local Declarations</span></span>

<span data-ttu-id="041a7-282">除了局部声明语句以外，还可以通过使用隐式声明局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-282">In addition to local declaration statements, local variables can also be declared implicitly through use.</span></span> <span data-ttu-id="041a7-283">使用不解析为其他内容的名称的简单名称表达式会按该名称声明局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-283">A simple name expression that uses a name that does not resolve to something else declares a local variable by that name.</span></span> <span data-ttu-id="041a7-284">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-284">For example:</span></span>

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

<span data-ttu-id="041a7-285">隐式局部声明仅出现在可接受分类为变量的表达式的表达式上下文中。</span><span class="sxs-lookup"><span data-stu-id="041a7-285">Implicit local declaration only occurs in expression contexts that can accept an expression classified as a variable.</span></span> <span data-ttu-id="041a7-286">此规则的例外是，当局部变量是函数调用表达式、索引表达式或成员访问表达式的目标时，可能不会隐式声明该局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-286">The exception to this rule is that a local variable may not be implicitly declared when it is the target of a function invocation expression, indexing expression, or a member access expression.</span></span>

<span data-ttu-id="041a7-287">隐式局部变量被视为在包含方法的开头声明的局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-287">Implicit locals are treated as if they are declared at the beginning of the containing method.</span></span> <span data-ttu-id="041a7-288">因此，它们始终作用于整个方法主体，即使在 lambda 表达式内声明也是如此。</span><span class="sxs-lookup"><span data-stu-id="041a7-288">Thus, they are always scoped to the entire method body, even if declared inside of a lambda expression.</span></span> <span data-ttu-id="041a7-289">例如，以下代码：</span><span class="sxs-lookup"><span data-stu-id="041a7-289">For example, the following code:</span></span>

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

<span data-ttu-id="041a7-290">将 `10` 打印该值。</span><span class="sxs-lookup"><span data-stu-id="041a7-290">will print the value `10`.</span></span> <span data-ttu-id="041a7-291">如果未将任何类型字符附加到变量名称，则隐式局部变量将类型化为 `Object`;否则，该变量的类型为类型字符的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-291">Implicit locals are typed as `Object` if no type character was attached to the variable name; otherwise the type of the variable is the type of the type character.</span></span> <span data-ttu-id="041a7-292">局部变量类型推理不用于隐式局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-292">Local variable type inference is not used for implicit locals.</span></span>

<span data-ttu-id="041a7-293">如果显式局部声明由编译环境或 `Option Explicit` 指定，则所有局部变量都必须显式声明并且不允许隐式变量声明。</span><span class="sxs-lookup"><span data-stu-id="041a7-293">If explicit local declaration is specified by the compilation environment or by `Option Explicit`, all local variables must be explicitly declared and implicit variable declaration is disallowed.</span></span>

## <a name="with-statement"></a><span data-ttu-id="041a7-294">With 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-294">With Statement</span></span>

<span data-ttu-id="041a7-295">@No__t-0 语句允许多次引用表达式的成员而无需多次指定表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-295">A `With` statement allows multiple references to an expression's members without specifying the expression multiple times.</span></span>

```antlr
WithStatement
    : 'With' Expression StatementTerminator
      Block?
      'End' 'With' StatementTerminator
    ;
```

<span data-ttu-id="041a7-296">表达式必须分类为一个值，并在进入块后计算一次。</span><span class="sxs-lookup"><span data-stu-id="041a7-296">The expression must be classified as a value and is evaluated once, upon entry into the block.</span></span> <span data-ttu-id="041a7-297">在 `With` 语句块中，计算以句点或感叹号开头的成员访问表达式或字典访问表达式的计算方式就像前面的 @no__t 1 表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-297">Within the `With` statement block, a member access expression or dictionary access expression starting with a period or an exclamation point is evaluated as if the `With` expression preceded it.</span></span> <span data-ttu-id="041a7-298">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-298">For example:</span></span>

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

<span data-ttu-id="041a7-299">分支到块外的 @no__t 0 语句块中是无效的。</span><span class="sxs-lookup"><span data-stu-id="041a7-299">It is invalid to branch into a `With` statement block from outside of the block.</span></span>


## <a name="synclock-statement"></a><span data-ttu-id="041a7-300">SyncLock 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-300">SyncLock Statement</span></span>

<span data-ttu-id="041a7-301">@No__t-0 语句允许在表达式上同步语句，这样可确保多个执行线程不会同时执行相同的语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-301">A `SyncLock` statement allows statements to be synchronized on an expression, which ensures that multiple threads of execution do not execute the same statements at the same time.</span></span>

```antlr
SyncLockStatement
    : 'SyncLock' Expression StatementTerminator
      Block?
      'End' 'SyncLock' StatementTerminator
    ;
```

<span data-ttu-id="041a7-302">表达式必须归类为一个值，并在进入到块时计算一次。</span><span class="sxs-lookup"><span data-stu-id="041a7-302">The expression must be classified as a value and is evaluated once, upon entry to the block.</span></span> <span data-ttu-id="041a7-303">输入 `SyncLock` 块时，将在指定的表达式中调用 `System.Threading.Monitor.Enter` 的 @no__t 方法，直到执行线程在表达式返回的对象上具有排他锁。</span><span class="sxs-lookup"><span data-stu-id="041a7-303">When entering the `SyncLock` block, the `Shared` method `System.Threading.Monitor.Enter` is called on the specified expression, which blocks until the thread of execution has an exclusive lock on the object returned by the expression.</span></span> <span data-ttu-id="041a7-304">@No__t-0 语句中表达式的类型必须是引用类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-304">The type of the expression in a `SyncLock` statement must be a reference type.</span></span> <span data-ttu-id="041a7-305">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-305">For example:</span></span>

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

<span data-ttu-id="041a7-306">上面的示例将在类的特定实例上进行同步 `Test`，以确保每个特定实例的执行线程不能同时在计数变量中增加或减少。</span><span class="sxs-lookup"><span data-stu-id="041a7-306">The example above synchronizes on the specific instance of the class `Test` to ensure that no more than one thread of execution can add or subtract from the count variable at a time for a particular instance.</span></span>

<span data-ttu-id="041a7-307">@No__t-0 块隐式包含在 @no__t 1 条语句中，其 @no__t 2 块调用表达式上的 @no__t 3 方法 @no__t。</span><span class="sxs-lookup"><span data-stu-id="041a7-307">The `SyncLock` block is implicitly contained by a `Try` statement whose `Finally` block calls the `Shared` method `System.Threading.Monitor.Exit` on the expression.</span></span> <span data-ttu-id="041a7-308">这可以确保即使在引发异常时也会释放锁。</span><span class="sxs-lookup"><span data-stu-id="041a7-308">This ensures the lock is freed even when an exception is thrown.</span></span> <span data-ttu-id="041a7-309">因此，从块外分支到 `SyncLock` 块并将 @no__t 1 块视为单个语句以用于 `Resume` 和 `Resume Next` 是无效的。</span><span class="sxs-lookup"><span data-stu-id="041a7-309">As a result, it is invalid to branch into a `SyncLock` block from outside of the block, and a `SyncLock` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="041a7-310">上面的示例等效于以下代码：</span><span class="sxs-lookup"><span data-stu-id="041a7-310">The above example is equivalent to the following code:</span></span>

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


## <a name="event-statements"></a><span data-ttu-id="041a7-311">Event 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-311">Event Statements</span></span>

<span data-ttu-id="041a7-312">@No__t，`AddHandler` 和 `RemoveHandler` 语句会引发事件并动态处理事件。</span><span class="sxs-lookup"><span data-stu-id="041a7-312">The `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements raise events and handle events dynamically.</span></span>

```antlr
EventStatement
    : RaiseEventStatement
    | AddHandlerStatement
    | RemoveHandlerStatement
    ;
```

### <a name="raiseevent-statement"></a><span data-ttu-id="041a7-313">RaiseEvent 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-313">RaiseEvent Statement</span></span>

<span data-ttu-id="041a7-314">@No__t-0 语句通知事件处理程序发生了特定事件。</span><span class="sxs-lookup"><span data-stu-id="041a7-314">A `RaiseEvent` statement notifies event handlers that a particular event has occurred.</span></span>

```antlr
RaiseEventStatement
    : 'RaiseEvent' IdentifierOrKeyword
      ( OpenParenthesis ArgumentList? CloseParenthesis )? StatementTerminator
    ;
```

<span data-ttu-id="041a7-315">@No__t-0 语句中的简单名称表达式被解释为 `Me` 上的成员查找。</span><span class="sxs-lookup"><span data-stu-id="041a7-315">The simple name expression in a `RaiseEvent` statement is interpreted as a member lookup on `Me`.</span></span> <span data-ttu-id="041a7-316">因此，`RaiseEvent x` 被解释为 @no__t 为-1。</span><span class="sxs-lookup"><span data-stu-id="041a7-316">Thus, `RaiseEvent x` is interpreted as if it were `RaiseEvent Me.x`.</span></span> <span data-ttu-id="041a7-317">表达式的结果必须归类为对类本身中定义的事件的事件访问;在基类型上定义的事件不能在 `RaiseEvent` 语句中使用。</span><span class="sxs-lookup"><span data-stu-id="041a7-317">The result of the expression must be classified as an event access for an event defined in the class itself; events defined on base types cannot be used in a `RaiseEvent` statement.</span></span>

<span data-ttu-id="041a7-318">使用提供的参数（如果有），将 `RaiseEvent` 语句作为对事件委托的 @no__t 的方法的调用进行处理。</span><span class="sxs-lookup"><span data-stu-id="041a7-318">The `RaiseEvent` statement is processed as a call to the `Invoke` method of the event's delegate, using the supplied parameters, if any.</span></span> <span data-ttu-id="041a7-319">如果委托的值为 `Nothing`，则不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-319">If the delegate's value is `Nothing`, no exception is thrown.</span></span> <span data-ttu-id="041a7-320">如果没有参数，则可以省略括号。</span><span class="sxs-lookup"><span data-stu-id="041a7-320">If there are no arguments, the parentheses may be omitted.</span></span> <span data-ttu-id="041a7-321">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-321">For example:</span></span>

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

<span data-ttu-id="041a7-322">上面 `Raiser` 类等效于：</span><span class="sxs-lookup"><span data-stu-id="041a7-322">The class `Raiser` above is equivalent to:</span></span>

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


### <a name="addhandler-and-removehandler-statements"></a><span data-ttu-id="041a7-323">AddHandler 和 RemoveHandler 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-323">AddHandler and RemoveHandler Statements</span></span>

<span data-ttu-id="041a7-324">尽管大多数事件处理程序都通过 `WithEvents` 变量自动挂钩，但可能需要在运行时动态添加和删除事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="041a7-324">Although most event handlers are automatically hooked up through `WithEvents` variables, it may be necessary to dynamically add and remove event handlers at run time.</span></span> <span data-ttu-id="041a7-325">`AddHandler` 和 `RemoveHandler` 语句执行此操作。</span><span class="sxs-lookup"><span data-stu-id="041a7-325">`AddHandler` and `RemoveHandler` statements do this.</span></span>

```antlr
AddHandlerStatement
    : 'AddHandler' Expression Comma Expression StatementTerminator
    ;

RemoveHandlerStatement
    : 'RemoveHandler' Expression Comma Expression StatementTerminator
    ;
```

<span data-ttu-id="041a7-326">每个语句都采用两个参数：第一个参数必须是分类为事件访问的表达式，第二个参数必须是分类为值的表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-326">Each statement takes two arguments: the first argument must be an expression that is classified as an event access and the second argument must be an expression that is classified as a value.</span></span> <span data-ttu-id="041a7-327">第二个参数的类型必须是与事件访问关联的委托类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-327">The second argument's type must be the delegate type associated with the event access.</span></span> <span data-ttu-id="041a7-328">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-328">For example:</span></span>

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

<span data-ttu-id="041a7-329">给定事件 `E,` 语句对实例调用相关的 @no__t 或 `remove_E` 方法，以添加或删除作为事件处理程序的委托。</span><span class="sxs-lookup"><span data-stu-id="041a7-329">Given an event `E,` the statement calls the relevant `add_E` or `remove_E` method on the instance to add or remove the delegate as a handler for the event.</span></span> <span data-ttu-id="041a7-330">因此，上述代码等效于：</span><span class="sxs-lookup"><span data-stu-id="041a7-330">Thus, the above code is equivalent to:</span></span>

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


## <a name="assignment-statements"></a><span data-ttu-id="041a7-331">赋值语句</span><span class="sxs-lookup"><span data-stu-id="041a7-331">Assignment Statements</span></span>

<span data-ttu-id="041a7-332">赋值语句将表达式的值分配给变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-332">An assignment statement assigns the value of an expression to a variable.</span></span> <span data-ttu-id="041a7-333">有几种类型的赋值。</span><span class="sxs-lookup"><span data-stu-id="041a7-333">There are several types of assignment.</span></span>

```antlr
AssignmentStatement
    : RegularAssignmentStatement
    | CompoundAssignmentStatement
    | MidAssignmentStatement
    ;
```

### <a name="regular-assignment-statements"></a><span data-ttu-id="041a7-334">常规赋值语句</span><span class="sxs-lookup"><span data-stu-id="041a7-334">Regular Assignment Statements</span></span>

<span data-ttu-id="041a7-335">简单的赋值语句将表达式的结果存储在变量中。</span><span class="sxs-lookup"><span data-stu-id="041a7-335">A simple assignment statement stores the result of an expression in a variable.</span></span>

```antlr
RegularAssignmentStatement
    : Expression Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="041a7-336">赋值运算符左侧的表达式必须归类为变量或属性访问权限，而赋值运算符右侧的表达式必须归类为值的值值。</span><span class="sxs-lookup"><span data-stu-id="041a7-336">The expression on the left side of the assignment operator must be classified as a variable or a property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="041a7-337">表达式的类型必须可隐式转换为变量或属性访问的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-337">The type of the expression must be implicitly convertible to the type of the variable or property access.</span></span>

<span data-ttu-id="041a7-338">如果要赋给的变量是引用类型的数组元素，则将执行运行时检查以确保表达式与数组元素类型兼容。</span><span class="sxs-lookup"><span data-stu-id="041a7-338">If the variable being assigned into is an array element of a reference type, a run-time check will be performed to ensure that the expression is compatible with the array-element type.</span></span> <span data-ttu-id="041a7-339">在下面的示例中，上一次分配导致引发 `System.ArrayTypeMismatchException`，因为 @no__t 的实例无法存储在 @no__t 数组的元素中。</span><span class="sxs-lookup"><span data-stu-id="041a7-339">In the following example, the last assignment causes a `System.ArrayTypeMismatchException` to be thrown, because an instance of `ArrayList` cannot be stored in an element of a `String` array.</span></span>

```vb
Dim sa(10) As String
Dim oa As Object() = sa
oa(0) = Nothing         ' This is allowed.
oa(1) = "Hello"         ' This is allowed.
oa(2) = New ArrayList() ' System.ArrayTypeMismatchException is thrown.
```

<span data-ttu-id="041a7-340">如果赋值运算符左侧的表达式归类为变量，则赋值语句将值存储在变量中。</span><span class="sxs-lookup"><span data-stu-id="041a7-340">If the expression on the left side of the assignment operator is classified as a variable, then the assignment statement stores the value in the variable.</span></span> <span data-ttu-id="041a7-341">如果表达式归类为属性访问，则赋值语句会将属性访问权限转换为对属性的 @no__t 值访问器的调用，并将替换值参数的值。</span><span class="sxs-lookup"><span data-stu-id="041a7-341">If the expression is classified as a property access, then the assignment statement turns the property access into an invocation of the `Set` accessor of the property with the value substituted for the value parameter.</span></span> <span data-ttu-id="041a7-342">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-342">For example:</span></span>

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

<span data-ttu-id="041a7-343">如果变量或属性访问的目标类型为值类型，但未分类为变量，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-343">If the target of the variable or property access is typed as a value type but not classified as a variable, a compile-time error occurs.</span></span> <span data-ttu-id="041a7-344">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-344">For example:</span></span>

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

<span data-ttu-id="041a7-345">请注意，赋值的语义取决于将其分配到的变量或属性的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-345">Note that the semantics of the assignment depend on the type of the variable or property to which it is being assigned.</span></span> <span data-ttu-id="041a7-346">如果为其分配的变量是值类型，则赋值将表达式的值复制到变量中。</span><span class="sxs-lookup"><span data-stu-id="041a7-346">If the variable to which it is being assigned is a value type, the assignment copies the value of the expression into the variable.</span></span> <span data-ttu-id="041a7-347">如果为其分配的变量是引用类型，则赋值将引用（而不是值本身）复制到变量中。</span><span class="sxs-lookup"><span data-stu-id="041a7-347">If the variable to which it is being assigned is a reference type, the assignment copies the reference, not the value itself, into the variable.</span></span> <span data-ttu-id="041a7-348">如果变量的类型为 `Object`，则赋值语义取决于值的类型在运行时是否为值类型或引用类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-348">If the type of the variable is `Object`, the assignment semantics are determined by whether the value's type is a value type or a reference type at run time.</span></span>


<span data-ttu-id="041a7-349">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-349">__Note.__</span></span> <span data-ttu-id="041a7-350">对于内部类型（如 `Integer` 和 `Date`），引用和值赋值语义是相同的，因为这些类型是不可变的。</span><span class="sxs-lookup"><span data-stu-id="041a7-350">For intrinsic types such as `Integer` and `Date`, reference and value assignment semantics are the same because the types are immutable.</span></span> <span data-ttu-id="041a7-351">这样一来，就可以将装箱的内部类型的引用分配用作一种优化语言。</span><span class="sxs-lookup"><span data-stu-id="041a7-351">As a result, the language is free to use reference assignment on boxed intrinsic types as an optimization.</span></span> <span data-ttu-id="041a7-352">从值角度来看，结果是相同的。</span><span class="sxs-lookup"><span data-stu-id="041a7-352">From a value perspective, the result is the same.</span></span>

<span data-ttu-id="041a7-353">因为等号字符（`=`）同时用于赋值和相等，所以，在 `x = y.ToString()` 的情况下，简单赋值与调用语句之间存在歧义。</span><span class="sxs-lookup"><span data-stu-id="041a7-353">Because the equals character (`=`) is used both for assignment and for equality, there is an ambiguity between a simple assignment and an invocation statement in situations such as `x = y.ToString()`.</span></span> <span data-ttu-id="041a7-354">在所有这类情况下，赋值语句优先于相等运算符。</span><span class="sxs-lookup"><span data-stu-id="041a7-354">In all such cases, the assignment statement takes precedence over the equality operator.</span></span> <span data-ttu-id="041a7-355">这意味着，示例表达式被解释为 `x = (y.ToString())` 而不是 `(x = y).ToString()`。</span><span class="sxs-lookup"><span data-stu-id="041a7-355">This means that the example expression is interpreted as `x = (y.ToString())` rather than `(x = y).ToString()`.</span></span>


### <a name="compound-assignment-statements"></a><span data-ttu-id="041a7-356">复合赋值语句</span><span class="sxs-lookup"><span data-stu-id="041a7-356">Compound Assignment Statements</span></span>

<span data-ttu-id="041a7-357">*复合赋值语句*采用 `V op= E` （其中 `op` 是有效的二进制运算符）的形式。</span><span class="sxs-lookup"><span data-stu-id="041a7-357">A *compound assignment statement* takes the form `V op= E` (where `op` is a valid binary operator).</span></span>

```antlr
CompoundAssignmentStatement
    : Expression CompoundBinaryOperator LineTerminator? Expression StatementTerminator
    ;

CompoundBinaryOperator
    : '^' '=' | '*' '=' | '/' '=' | '\\' '=' | '+' '=' | '-' '='
    | '&' '=' | '<' '<' '=' | '>' '>' '='
    ;
```

<span data-ttu-id="041a7-358">赋值运算符左侧的表达式必须归类为变量或属性访问权限，而赋值运算符右侧的表达式必须分类为值值。</span><span class="sxs-lookup"><span data-stu-id="041a7-358">The expression on the left side of the assignment operator must be classified as a variable or property access, while the expression on the right side of the assignment operator must be classified as a value.</span></span> <span data-ttu-id="041a7-359">复合赋值语句等效于语句 `V = V op E`，与复合赋值运算符左侧的变量只计算一次时的差异。</span><span class="sxs-lookup"><span data-stu-id="041a7-359">The compound assignment statement is equivalent to the statement `V = V op E` with the difference that the variable on the left side of the compound assignment operator is only evaluated once.</span></span> <span data-ttu-id="041a7-360">以下示例演示了这一差异：</span><span class="sxs-lookup"><span data-stu-id="041a7-360">The following example demonstrates this difference:</span></span>

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

<span data-ttu-id="041a7-361">对于简单赋值，表达式 `a(GetIndex())` 计算两次，但对于复合赋值，只计算一次，因此代码将打印：</span><span class="sxs-lookup"><span data-stu-id="041a7-361">The expression `a(GetIndex())` is evaluated twice for simple assignment but only once for compound assignment, so the code prints:</span></span>

```console
Simple assignment
Getting index
Getting index
Compound assignment
Getting index
```


### <a name="mid-assignment-statement"></a><span data-ttu-id="041a7-362">Mid 赋值语句</span><span class="sxs-lookup"><span data-stu-id="041a7-362">Mid Assignment Statement</span></span>

<span data-ttu-id="041a7-363">@No__t 0 赋值语句将一个字符串赋给另一个字符串。</span><span class="sxs-lookup"><span data-stu-id="041a7-363">A `Mid` assignment statement assigns a string into another string.</span></span> <span data-ttu-id="041a7-364">赋值的左侧与函数调用 `Microsoft.VisualBasic.Strings.Mid` 的语法相同。</span><span class="sxs-lookup"><span data-stu-id="041a7-364">The left side of the assignment has the same syntax as a call to the function `Microsoft.VisualBasic.Strings.Mid`.</span></span>

```antlr
MidAssignmentStatement
    : 'Mid' '$'? OpenParenthesis Expression Comma Expression
      ( Comma Expression )? CloseParenthesis Equals Expression StatementTerminator
    ;
```

<span data-ttu-id="041a7-365">第一个参数是赋值目标，并且必须归类为变量或属性访问，其类型可隐式转换为 `String`。</span><span class="sxs-lookup"><span data-stu-id="041a7-365">The first argument is the target of the assignment and must be classified as a variable or a property access whose type is implicitly convertible to and from `String`.</span></span> <span data-ttu-id="041a7-366">第二个参数是从1开始的开始位置，它对应于赋值在目标字符串中的开始位置，并且必须分类为其类型必须可隐式转换为 `Integer` 的值。</span><span class="sxs-lookup"><span data-stu-id="041a7-366">The second parameter is the 1-based start position that corresponds to where the assignment should begin in the target string and must be classified as a value whose type must be implicitly convertible to `Integer`.</span></span> <span data-ttu-id="041a7-367">可选的第三个参数是要分配给目标字符串的右侧值中的字符数，并且必须归类为其类型可隐式转换为 `Integer` 的值。</span><span class="sxs-lookup"><span data-stu-id="041a7-367">The optional third parameter is the number of characters from the right-side value to assign into the target string and must be classified as a value whose type is implicitly convertible to `Integer`.</span></span> <span data-ttu-id="041a7-368">右侧是源字符串，必须归类为其类型可隐式转换为 `String` 的值。</span><span class="sxs-lookup"><span data-stu-id="041a7-368">The right side is the source string and must be classified as a value whose type is implicitly convertible to `String`.</span></span> <span data-ttu-id="041a7-369">如果已指定，右侧将被截断为长度参数，并从起始位置开始替换左侧字符串中的字符。</span><span class="sxs-lookup"><span data-stu-id="041a7-369">The right side is truncated to the length parameter, if specified, and replaces the characters in the left-side string, starting at the start position.</span></span> <span data-ttu-id="041a7-370">如果右侧字符串包含的字符数少于第三个参数，则仅复制来自右侧字符串的字符。</span><span class="sxs-lookup"><span data-stu-id="041a7-370">If the right side string contained fewer characters than the third parameter, only the characters from the right side string will be copied.</span></span>

<span data-ttu-id="041a7-371">下面的示例显示 `ab123fg`：</span><span class="sxs-lookup"><span data-stu-id="041a7-371">The following example displays `ab123fg`:</span></span>

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

<span data-ttu-id="041a7-372">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-372">__Note.__</span></span> <span data-ttu-id="041a7-373">`Mid` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="041a7-373">`Mid` is not a reserved word.</span></span>


## <a name="invocation-statements"></a><span data-ttu-id="041a7-374">调用语句</span><span class="sxs-lookup"><span data-stu-id="041a7-374">Invocation Statements</span></span>

<span data-ttu-id="041a7-375">调用语句调用一个方法，该方法前面有一个可选关键字 `Call`。</span><span class="sxs-lookup"><span data-stu-id="041a7-375">An invocation statement invokes a method preceded by the optional keyword `Call`.</span></span> <span data-ttu-id="041a7-376">调用语句的处理方式与函数调用表达式的处理方式相同，但有一些区别。</span><span class="sxs-lookup"><span data-stu-id="041a7-376">The invocation statement is processed in the same way as the function invocation expression, with some differences noted below.</span></span> <span data-ttu-id="041a7-377">调用表达式必须分类为值或 void。</span><span class="sxs-lookup"><span data-stu-id="041a7-377">The invocation expression must be classified as a value or void.</span></span> <span data-ttu-id="041a7-378">对调用表达式求值后得出的任何值都将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="041a7-378">Any value resulting from the evaluation of the invocation expression is discarded.</span></span>

<span data-ttu-id="041a7-379">如果省略 @no__t 0 关键字，则调用表达式必须以标识符或关键字开头，或在 `With` 块内以 `.` 开头。</span><span class="sxs-lookup"><span data-stu-id="041a7-379">If the `Call` keyword is omitted, then the invocation expression must start with an identifier or keyword, or with `.` inside a `With` block.</span></span> <span data-ttu-id="041a7-380">例如，"`Call 1.ToString()`" 是有效的语句，但不是 "`1.ToString()`"。</span><span class="sxs-lookup"><span data-stu-id="041a7-380">Thus, for instance, "`Call 1.ToString()`" is a valid statement but "`1.ToString()`" is not.</span></span> <span data-ttu-id="041a7-381">（请注意，在表达式上下文中，调用表达式也不需要以标识符开头。</span><span class="sxs-lookup"><span data-stu-id="041a7-381">(Note that in an expression context, invocation expressions also need not start with an identifier.</span></span> <span data-ttu-id="041a7-382">例如，"`Dim x = 1.ToString()`" 是有效的语句）。</span><span class="sxs-lookup"><span data-stu-id="041a7-382">For example, "`Dim x = 1.ToString()`" is a valid statement).</span></span>

<span data-ttu-id="041a7-383">调用语句和调用表达式之间还有另一个区别：如果调用语句包含参数列表，则此操作将始终作为调用的参数列表。</span><span class="sxs-lookup"><span data-stu-id="041a7-383">There is another difference between the invocation statements and invocation expressions: if an invocation statement includes an argument list, then this is always taken as the argument list of the invocation.</span></span> <span data-ttu-id="041a7-384">下面的示例阐释了差异：</span><span class="sxs-lookup"><span data-stu-id="041a7-384">The following example illustrates the difference:</span></span>

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

## <a name="conditional-statements"></a><span data-ttu-id="041a7-385">条件语句</span><span class="sxs-lookup"><span data-stu-id="041a7-385">Conditional Statements</span></span>

<span data-ttu-id="041a7-386">条件语句允许基于在运行时计算的表达式执行语句的条件执行。</span><span class="sxs-lookup"><span data-stu-id="041a7-386">Conditional statements allow conditional execution of statements based on expressions evaluated at run time.</span></span>

```antlr
ConditionalStatement
    : IfStatement
    | SelectStatement
    ;
```

### <a name="ifthenelse-statements"></a><span data-ttu-id="041a7-387">If .。。Then .。。Else 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-387">If...Then...Else Statements</span></span>

<span data-ttu-id="041a7-388">@No__t-0 语句是基本的条件语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-388">An `If...Then...Else` statement is the basic conditional statement.</span></span>

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

<span data-ttu-id="041a7-389">@No__t-0 语句中的每个表达式都必须为布尔表达式，如每个部分的[布尔表达式](expressions.md#boolean-expressions)。</span><span class="sxs-lookup"><span data-stu-id="041a7-389">Each expression in an `If...Then...Else` statement must be a Boolean expression, as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="041a7-390">（注意：这不需要表达式具有布尔类型）。</span><span class="sxs-lookup"><span data-stu-id="041a7-390">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="041a7-391">如果 `If` 语句中的表达式为 true，则执行 @no__t 中包含的语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-391">If the expression in the `If` statement is true, the statements enclosed by the `If` block are executed.</span></span> <span data-ttu-id="041a7-392">如果表达式为 false，则计算每个 @no__t 0 表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-392">If the expression is false, each of the `ElseIf` expressions is evaluated.</span></span> <span data-ttu-id="041a7-393">如果 @no__t 0 表达式之一的计算结果为 true，则执行相应的块。</span><span class="sxs-lookup"><span data-stu-id="041a7-393">If one of the `ElseIf` expressions evaluates to true, the corresponding block is executed.</span></span> <span data-ttu-id="041a7-394">如果表达式的计算结果都不为 true，并且有 `Else` 块，则执行 @no__t 的块。</span><span class="sxs-lookup"><span data-stu-id="041a7-394">If no expression evaluates to true and there is an `Else` block, the `Else` block is executed.</span></span> <span data-ttu-id="041a7-395">块完成执行后，将传递到 `If...Then...Else` 语句的末尾。</span><span class="sxs-lookup"><span data-stu-id="041a7-395">Once a block finishes executing, execution passes to the end of the `If...Then...Else` statement.</span></span>

<span data-ttu-id="041a7-396">如果 @no__t 的表达式 @no__t 为-2，则 @no__t 的语句的行版本包含一组要执行的语句，如果表达式为 `False`，则执行一组可选的语句来执行。</span><span class="sxs-lookup"><span data-stu-id="041a7-396">The line version of the `If` statement has a single set of statements to be executed if the `If` expression is `True` and an optional set of statements to be executed if the expression is `False`.</span></span> <span data-ttu-id="041a7-397">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-397">For example:</span></span>

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

<span data-ttu-id="041a7-398">If 语句的行版本与 "：" 紧密绑定，其 `Else` 绑定到语法允许的上一个前面的 `If`。</span><span class="sxs-lookup"><span data-stu-id="041a7-398">The line version of the If statement binds less tightly than ":", and its `Else` binds to the lexically nearest preceding `If` that is allowed by the syntax.</span></span> <span data-ttu-id="041a7-399">例如，以下两个版本是等效的：</span><span class="sxs-lookup"><span data-stu-id="041a7-399">For example, the following two versions are equivalent:</span></span>

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

<span data-ttu-id="041a7-400">除标签声明语句以外的所有语句都允许在 `If` 语句（包括块语句）的行中使用。</span><span class="sxs-lookup"><span data-stu-id="041a7-400">All statements other than label declaration statements are allowed inside a line `If` statement, including block statements.</span></span> <span data-ttu-id="041a7-401">但在多行 lambda 表达式中，它们不能将 LineTerminators 用作 StatementTerminators。</span><span class="sxs-lookup"><span data-stu-id="041a7-401">However, they may not use LineTerminators as StatementTerminators except inside multi-line lambda expressions.</span></span> <span data-ttu-id="041a7-402">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-402">For example:</span></span>

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

### <a name="select-case-statements"></a><span data-ttu-id="041a7-403">Select Case 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-403">Select Case Statements</span></span>

<span data-ttu-id="041a7-404">@No__t-0 语句基于表达式的值执行语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-404">A `Select Case` statement executes statements based on the value of an expression.</span></span>

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

<span data-ttu-id="041a7-405">表达式必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="041a7-405">The expression must be classified as a value.</span></span> <span data-ttu-id="041a7-406">执行 `Select Case` 语句时，将首先计算 @no__t 1 表达式，然后按文本声明顺序计算 `Case` 语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-406">When a `Select Case` statement is executed, the `Select` expression is evaluated first, and the `Case` statements are then evaluated in order of textual declaration.</span></span> <span data-ttu-id="041a7-407">计算结果为 `True` 的第一个 `Case` 语句的块已执行。</span><span class="sxs-lookup"><span data-stu-id="041a7-407">The first `Case` statement that evaluates to `True` has its block executed.</span></span> <span data-ttu-id="041a7-408">如果没有 `Case` 语句的计算结果为 `True`，并且存在 @no__t 2 语句，则将执行该块。</span><span class="sxs-lookup"><span data-stu-id="041a7-408">If no `Case` statement evaluates to `True` and there is a `Case Else` statement, that block is executed.</span></span> <span data-ttu-id="041a7-409">块执行完毕后，将传递到 `Select` 语句的末尾。</span><span class="sxs-lookup"><span data-stu-id="041a7-409">Once a block has finished executing, execution passes to the end of the `Select` statement.</span></span>

<span data-ttu-id="041a7-410">不允许执行 `Case` 块到下一个开关部分。</span><span class="sxs-lookup"><span data-stu-id="041a7-410">Execution of a `Case` block is not permitted to "fall through" to the next switch section.</span></span> <span data-ttu-id="041a7-411">这可以防止在意外省略 `Case` 终止语句时使用其他语言发生的常见错误类。</span><span class="sxs-lookup"><span data-stu-id="041a7-411">This prevents a common class of bugs that occur in other languages when a `Case` terminating statement is accidentally omitted.</span></span> <span data-ttu-id="041a7-412">下面的示例阐释了这种行为：</span><span class="sxs-lookup"><span data-stu-id="041a7-412">The following example illustrates this behavior:</span></span>

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

<span data-ttu-id="041a7-413">代码将打印：</span><span class="sxs-lookup"><span data-stu-id="041a7-413">The code prints:</span></span>

```console
x = 10
```

<span data-ttu-id="041a7-414">尽管 @no__t 为相同的值，`Case 20 - 10`，但会执行 `Case 10`，因为它在每个的第3个值 @no__t 之前。</span><span class="sxs-lookup"><span data-stu-id="041a7-414">Although `Case 10` and `Case 20 - 10` select for the same value, `Case 10` is executed because it precedes `Case 20 - 10` textually.</span></span> <span data-ttu-id="041a7-415">当到达下一个 `Case` 时，执行将在 @no__t 1 语句后继续。</span><span class="sxs-lookup"><span data-stu-id="041a7-415">When the next `Case` is reached, execution continues after the `Select` statement.</span></span>

<span data-ttu-id="041a7-416">@No__t-0 子句可以采用两种形式。</span><span class="sxs-lookup"><span data-stu-id="041a7-416">A `Case` clause may take two forms.</span></span> <span data-ttu-id="041a7-417">一个窗体是可选的 @no__t 0 关键字、比较运算符和表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-417">One form is an optional `Is` keyword, a comparison operator, and an expression.</span></span> <span data-ttu-id="041a7-418">表达式转换为 @no__t 0 表达式的类型;如果表达式不能隐式转换为 `Select` 表达式的类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-418">The expression is converted to the type of the `Select` expression; if the expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="041a7-419">如果 @no__t 0 表达式为*E*，则比较运算符为*Op*，`Case` 表达式为*E1*，则将事例计算为*E Op E1*。</span><span class="sxs-lookup"><span data-stu-id="041a7-419">If the `Select` expression is *E*, the comparison operator is *Op*, and the `Case` expression is *E1*, the case is evaluated as *E OP E1*.</span></span> <span data-ttu-id="041a7-420">对于两个表达式的类型，该运算符必须有效;否则，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-420">The operator must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="041a7-421">另一种形式是表达式，后跟关键字 `To` 和第二个表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-421">The other form is an expression optionally followed by the keyword `To` and a second expression.</span></span> <span data-ttu-id="041a7-422">两个表达式都转换为 @no__t 0 表达式的类型;如果任一表达式不能隐式转换为 `Select` 表达式的类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-422">Both expressions are converted to the type of the `Select` expression; if either expression is not implicitly convertible to the type of the `Select` expression, a compile-time error occurs.</span></span> <span data-ttu-id="041a7-423">如果 @no__t 0 表达式 @no__t 为-1，则第一个 `Case` 表达式为 `E1`，第二个 @no__t 表达式为 `E2`，则计算 `E = E1` （如果未指定 `E2`）或 `(E >= E1) And (E <= E2)`。</span><span class="sxs-lookup"><span data-stu-id="041a7-423">If the `Select` expression is `E`, the first `Case` expression is `E1`, and the second `Case` expression is `E2`, the `Case` is evaluated either as `E = E1` (if no `E2` is specified) or `(E >= E1) And (E <= E2)`.</span></span> <span data-ttu-id="041a7-424">对于两个表达式的类型，这些运算符必须有效;否则，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-424">The operators must be valid for the types of the two expressions; otherwise a compile-time error occurs.</span></span>


## <a name="loop-statements"></a><span data-ttu-id="041a7-425">Loop 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-425">Loop Statements</span></span>

<span data-ttu-id="041a7-426">循环语句允许在其主体中重复执行语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-426">Loop statements allow repeated execution of the statements in their body.</span></span>

```antlr
LoopStatement
    : WhileStatement
    | DoLoopStatement
    | ForStatement
    | ForEachStatement
    ;
```

<span data-ttu-id="041a7-427">每次输入循环正文时，都将在该正文中声明的所有局部变量上都生成一个全新的副本，并将其初始化为变量以前的值。</span><span class="sxs-lookup"><span data-stu-id="041a7-427">Each time a loop body is entered, a fresh copy is made of all local variables declared in that body, initialized to the previous values of the variables.</span></span> <span data-ttu-id="041a7-428">对循环体中的变量的任何引用都将使用最近创建的副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-428">Any reference to a variable within the loop body will use the most recently made copy.</span></span> <span data-ttu-id="041a7-429">此代码显示了一个示例：</span><span class="sxs-lookup"><span data-stu-id="041a7-429">This code shows an example:</span></span>

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

<span data-ttu-id="041a7-430">此代码生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="041a7-430">The code produces the output:</span></span>

```console
31    32    33
```

<span data-ttu-id="041a7-431">当执行循环体时，它将使用该变量的任何副本是最新的。</span><span class="sxs-lookup"><span data-stu-id="041a7-431">When the loop body is executed, it uses whichever copy of the variable is current.</span></span> <span data-ttu-id="041a7-432">例如，语句 `Dim y = x` 指 @no__t 的最新副本和 @no__t 的原始副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-432">For example, the statement  `Dim y = x` refers to the latest copy of `y` and the original copy of `x`.</span></span> <span data-ttu-id="041a7-433">创建 lambda 后，它会记住变量在创建时的哪个副本是最新的。</span><span class="sxs-lookup"><span data-stu-id="041a7-433">And when a lambda is created, it remembers whichever copy of a variable was current at the time it was created.</span></span> <span data-ttu-id="041a7-434">因此，每个 lambda 使用 `x` 的相同共享副本，但 `y` 的不同副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-434">Therefore, each lambda uses the same shared copy of `x`, but a different copy of `y`.</span></span> <span data-ttu-id="041a7-435">在程序结束时，当它执行 lambda 时，它们所引用的 @no__t 的共享副本现在为其最终值3。</span><span class="sxs-lookup"><span data-stu-id="041a7-435">At the end of the program, when it executes the lambdas, that shared copy of `x` that they all refer to is now at its final value 3.</span></span>

<span data-ttu-id="041a7-436">请注意，如果没有 lambda 或 LINQ 表达式，则不可能知道在循环条目上进行了全新的复制。</span><span class="sxs-lookup"><span data-stu-id="041a7-436">Note that if there are no lambdas or LINQ expressions, then it's impossible to know that a fresh copy is made on loop entry.</span></span> <span data-ttu-id="041a7-437">事实上，在这种情况下，编译器优化将避免生成副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-437">Indeed, compiler optimizations will avoid making copies in this case.</span></span> <span data-ttu-id="041a7-438">请注意，将 `GoTo` 到包含 lambda 或 LINQ 表达式的循环中是非法的。</span><span class="sxs-lookup"><span data-stu-id="041a7-438">Note too that it's illegal to `GoTo` into a loop that contains lambdas or LINQ expressions.</span></span>


### <a name="whileend-while-and-doloop-statements"></a><span data-ttu-id="041a7-439">.。。结束时间和执行时间 .。。Loop 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-439">While...End While and Do...Loop Statements</span></span>

<span data-ttu-id="041a7-440">@No__t-0 或 @no__t 循环语句基于布尔表达式循环。</span><span class="sxs-lookup"><span data-stu-id="041a7-440">A `While` or `Do` loop statement loops based on a Boolean expression.</span></span>

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

<span data-ttu-id="041a7-441">如果布尔表达式的计算结果为 true，则 `While` 循环语句会循环;`Do` loop 语句可能包含更复杂的条件。</span><span class="sxs-lookup"><span data-stu-id="041a7-441">A `While` loop statement loops as long as the Boolean expression evaluates to true; a `Do` loop statement may contain a more complex condition.</span></span> <span data-ttu-id="041a7-442">表达式可以放在 `Do` 关键字之后或 `Loop` 关键字之后，但不能位于这两者之后。</span><span class="sxs-lookup"><span data-stu-id="041a7-442">An expression may be placed after the `Do` keyword or after the `Loop` keyword, but not after both.</span></span> <span data-ttu-id="041a7-443">布尔表达式的计算方式为[布尔](expressions.md#boolean-expressions)表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-443">The Boolean expression is evaluated as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="041a7-444">（注意：这不需要表达式具有布尔类型）。</span><span class="sxs-lookup"><span data-stu-id="041a7-444">(Note: this does not require the expression to have Boolean type).</span></span> <span data-ttu-id="041a7-445">同时指定 no 表达式也是有效的;在这种情况下，循环永远不会退出。</span><span class="sxs-lookup"><span data-stu-id="041a7-445">It is also valid to specify no expression at all; in that case, the loop will never exit.</span></span> <span data-ttu-id="041a7-446">如果表达式位于 `Do` 之后，则在每次迭代上执行循环块之前，将计算该表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-446">If the expression is placed after `Do`, it will be evaluated before the loop block is executed on each iteration.</span></span> <span data-ttu-id="041a7-447">如果表达式位于 `Loop` 之后，则在每次迭代中执行循环块后，将计算该表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-447">If the expression is placed after `Loop`, it will be evaluated after the loop block has executed on each iteration.</span></span> <span data-ttu-id="041a7-448">因此，将表达式放在 `Loop` 后，将生成比 `Do` 后面的位置更多的循环。</span><span class="sxs-lookup"><span data-stu-id="041a7-448">Placing the expression after `Loop` will therefore generate one more loop than placement after `Do`.</span></span> <span data-ttu-id="041a7-449">下面的示例演示了此行为：</span><span class="sxs-lookup"><span data-stu-id="041a7-449">The following example demonstrates this behavior:</span></span>

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

<span data-ttu-id="041a7-450">此代码生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="041a7-450">The code produces the output:</span></span>

```console
Second Loop
```

<span data-ttu-id="041a7-451">如果是第一次循环，则在循环执行之前计算条件。</span><span class="sxs-lookup"><span data-stu-id="041a7-451">In the case of the first loop, the condition is evaluated before the loop executes.</span></span> <span data-ttu-id="041a7-452">对于第二个循环，该条件在循环执行后执行。</span><span class="sxs-lookup"><span data-stu-id="041a7-452">In the case of the second loop, the condition is executed after the loop executes.</span></span> <span data-ttu-id="041a7-453">条件表达式必须以 @no__t 关键字或 @no__t 关键字为前缀。</span><span class="sxs-lookup"><span data-stu-id="041a7-453">The conditional expression must be prefixed with either a `While` keyword or an `Until` keyword.</span></span> <span data-ttu-id="041a7-454">如果条件的计算结果为 false，则前者会断开循环，条件的计算结果为 true。</span><span class="sxs-lookup"><span data-stu-id="041a7-454">The former breaks the loop if the condition evaluates to false, the latter when the condition evaluates to true.</span></span>

<span data-ttu-id="041a7-455">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-455">__Note.__</span></span> <span data-ttu-id="041a7-456">`Until` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="041a7-456">`Until` is not a reserved word.</span></span>


### <a name="fornext-statements"></a><span data-ttu-id="041a7-457">对于 .。。Next 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-457">For...Next Statements</span></span>

<span data-ttu-id="041a7-458">@No__t-0 语句根据一组界限循环。</span><span class="sxs-lookup"><span data-stu-id="041a7-458">A `For...Next` statement loops based on a set of bounds.</span></span> <span data-ttu-id="041a7-459">@No__t-0 语句指定循环控制变量、下限表达式、上限表达式和可选步骤值表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-459">A `For` statement specifies a loop control variable, a lower bound expression, an upper bound expression, and an optional step value expression.</span></span> <span data-ttu-id="041a7-460">循环控制变量是通过标识符指定的，后跟可选的 `As` 子句或表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-460">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

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

<span data-ttu-id="041a7-461">根据以下规则，循环控制变量是指特定于此 `For...Next` 语句或预先存在的变量或表达式的新局部变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-461">As per the following rules, the loop control variable refers either to a new local variable specific to this `For...Next` statement, or to a pre-existing variable, or to an expression.</span></span>

* <span data-ttu-id="041a7-462">如果循环控制变量是带有 `As` 子句的标识符，则该标识符将定义在 `As` 子句中指定的类型的新局部变量，其作用域为整个 `For` 循环。</span><span class="sxs-lookup"><span data-stu-id="041a7-462">If the loop control variable is an identifier with an `As` clause, the identifier defines a new local variable of the type specified in the `As` clause, scoped to the entire `For` loop.</span></span>

* <span data-ttu-id="041a7-463">如果循环控制变量是没有 `As` 子句的标识符，则首先使用简单名称解析规则解析该标识符（请参阅 "[简单名称表达式](expressions.md#simple-name-expressions)" 部分），只不过该标识符的此匹配项不在和中本身导致创建一个隐式局部变量（节[隐式局部声明](statements.md#implicit-local-declarations)）。</span><span class="sxs-lookup"><span data-stu-id="041a7-463">If the loop control variable is an identifier without an `As` clause, then the identifier is first resolved using the simple name resolution rules (see Section [Simple Name Expressions](expressions.md#simple-name-expressions)), excepting that this occurrence of the identifier would not in and of itself cause an implicit local variable to be created (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)).</span></span>

  * <span data-ttu-id="041a7-464">如果此解析成功并且结果归类为变量，则循环控制变量是预先存在的变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-464">If this resolution succeeds and the result is classified as a variable, then the loop control variable is that pre-existing variable.</span></span>

  * <span data-ttu-id="041a7-465">如果解析失败，或者如果解析成功并且结果归类为类型，则：</span><span class="sxs-lookup"><span data-stu-id="041a7-465">If resolution fails, or if resolution succeeds and the result is classified as a type, then:</span></span>
    * <span data-ttu-id="041a7-466">如果正在使用本地变量类型推理，则标识符将定义一个新的局部变量，该局部变量的类型将从绑定和步骤表达式中推断出，范围为整个 `For` 循环;</span><span class="sxs-lookup"><span data-stu-id="041a7-466">if local variable type inference is being used, the identifier defines a new local variable whose type is inferred from the bound and step expressions, scoped to the entire `For` loop;</span></span>
    * <span data-ttu-id="041a7-467">如果未使用局部变量类型推理，但隐式局部声明为，则将创建一个隐式局部变量，其作用域为整个方法（部分[隐式局部声明](statements.md#implicit-local-declarations)），并且循环控制变量引用此预先存在的变量;</span><span class="sxs-lookup"><span data-stu-id="041a7-467">if local variable type inference is not being used but implicit local declaration is, then an implicit local variable is created whose scope is the entire method (Section [Implicit Local Declarations](statements.md#implicit-local-declarations)), and the loop control variable refers to this pre-existing variable;</span></span>
    * <span data-ttu-id="041a7-468">如果不使用本地变量类型推理和隐式本地声明，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="041a7-468">if neither local variable type inference nor implicit local declarations are used, it is an error.</span></span>

  * <span data-ttu-id="041a7-469">如果解析成功，同时分类为类型和变量，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="041a7-469">If resolution succeeds with something classified as neither a type nor a variable, it is an error.</span></span>

* <span data-ttu-id="041a7-470">如果循环控制变量是一个表达式，则该表达式必须归类为变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-470">If the loop control variable is an expression, the expression must be classified as a variable.</span></span>

<span data-ttu-id="041a7-471">循环控制变量不能由另一个封闭 `For...Next` 语句使用。</span><span class="sxs-lookup"><span data-stu-id="041a7-471">A loop control variable cannot be used by another enclosing `For...Next` statement.</span></span> <span data-ttu-id="041a7-472">@No__t-0 语句的循环控制变量的类型确定迭代的类型，必须是以下类型之一：</span><span class="sxs-lookup"><span data-stu-id="041a7-472">The type of the loop control variable of a `For` statement determines the type of the iteration and must be one of:</span></span>

* <span data-ttu-id="041a7-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span><span class="sxs-lookup"><span data-stu-id="041a7-473">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`</span></span>
* <span data-ttu-id="041a7-474">枚举类型</span><span class="sxs-lookup"><span data-stu-id="041a7-474">An enumerated type</span></span>
* `Object`
* <span data-ttu-id="041a7-475">一个类型 `T`，其中包含以下运算符，其中 `B` 是可在布尔表达式中使用的类型：</span><span class="sxs-lookup"><span data-stu-id="041a7-475">A type `T` that has the following operators, where `B` is a type that can be used in a Boolean expression:</span></span>

```vb
Public Shared Operator >= (op1 As T, op2 As T) As B
Public Shared Operator <= (op1 As T, op2 As T) As B
Public Shared Operator - (op1 As T, op2 As T) As T
Public Shared Operator + (op1 As T, op2 As T) As T
```

<span data-ttu-id="041a7-476">绑定表达式和步骤表达式必须可隐式转换为循环控制变量的类型，并且必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="041a7-476">The bound and step expressions must be implicitly convertible to the type of the loop control variable and must be classified as values.</span></span> <span data-ttu-id="041a7-477">在编译时，循环控制变量的类型是通过在下限、上限和步骤表达式类型之间选择最宽类型来推断的。</span><span class="sxs-lookup"><span data-stu-id="041a7-477">At compile time, the type of the loop control variable is inferred by choosing the widest type among the lower bound, upper bound, and step expression types.</span></span> <span data-ttu-id="041a7-478">如果两种类型之间没有扩大转换，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-478">If there is no widening conversion between two of the types, then a compile-time error occurs.</span></span>

<span data-ttu-id="041a7-479">在运行时，如果循环控制变量的类型为 `Object`，则迭代的类型在编译时将推断为，但有两个例外。</span><span class="sxs-lookup"><span data-stu-id="041a7-479">At run time, if the type of the loop control variable is `Object`, then the type of the iteration is inferred the same as at compile time, with two exceptions.</span></span> <span data-ttu-id="041a7-480">首先，如果绑定表达式和步骤表达式均为整型类型，但其类型不是最大，则将推断包含所有三个类型的最宽类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-480">First, if the bound and step expressions are all of integral types but have no widest type, then the widest type that encompasses all three types will be inferred.</span></span> <span data-ttu-id="041a7-481">第二种情况下，如果将循环控制变量的类型推断为 `String`，则改为推断 `Double`。</span><span class="sxs-lookup"><span data-stu-id="041a7-481">And second, if the type of the loop control variable is inferred to be `String`, `Double` will be inferred instead.</span></span> <span data-ttu-id="041a7-482">如果在运行时无法确定循环控制类型，或者如果任何表达式不能转换为 loop 控件类型，则会出现 @no__t 0。</span><span class="sxs-lookup"><span data-stu-id="041a7-482">If, at run time, no loop control type can be determined or if any of the expressions cannot be converted to the loop control type, a `System.InvalidCastException` will occur.</span></span> <span data-ttu-id="041a7-483">一旦在循环开始选择了循环控件类型，就会在整个迭代中使用相同的类型，而不考虑对循环控制变量中的值所做的更改。</span><span class="sxs-lookup"><span data-stu-id="041a7-483">Once a loop control type has been chosen at the beginning of the loop, the same type will be used throughout the iteration, regardless of changes made to the value in the loop control variable.</span></span>

<span data-ttu-id="041a7-484">@No__t-0 语句必须通过匹配的 `Next` 语句来关闭。</span><span class="sxs-lookup"><span data-stu-id="041a7-484">A `For` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="041a7-485">不带变量的 @no__t 0 语句与最内层的 open `For` 语句匹配，而具有一个或多个循环控制变量的 `Next` 语句将与每个变量匹配的 `For` 循环匹配。</span><span class="sxs-lookup"><span data-stu-id="041a7-485">A `Next` statement without a variable matches the innermost open `For` statement, while a `Next` statement with one or more loop control variables will, from left to right, match the `For` loops that match each variable.</span></span> <span data-ttu-id="041a7-486">如果变量与非最嵌套循环的 @no__t 0 循环匹配，则会产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-486">If a variable matches a `For` loop that is not the most nested loop at that point, a compile-time error results.</span></span>

<span data-ttu-id="041a7-487">在循环开始时，将按文本顺序计算三个表达式，并将下限表达式分配给循环控制变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-487">At the beginning of the loop, the three expressions are evaluated in textual order and the lower bound expression is assigned to the loop control variable.</span></span> <span data-ttu-id="041a7-488">如果省略步长值，则它将隐式赋值 `1`，转换为循环控制变量的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-488">If the step value is omitted, it is implicitly the literal `1`, converted to the type of the loop control variable.</span></span> <span data-ttu-id="041a7-489">这三个表达式仅在循环开始时进行计算。</span><span class="sxs-lookup"><span data-stu-id="041a7-489">The three expressions are only ever evaluated at the beginning of the loop.</span></span>

<span data-ttu-id="041a7-490">在每个循环开始时，将对控制变量进行比较，以查看如果步骤表达式为正，则为; 如果步骤表达式为负，则为小于结束点。</span><span class="sxs-lookup"><span data-stu-id="041a7-490">At the beginning of each loop, the control variable is compared to see if it is greater than the end point if the step expression is positive, or less than the end point if the step expression is negative.</span></span> <span data-ttu-id="041a7-491">如果已终止，则为 `For` 循环;否则，将执行循环块。</span><span class="sxs-lookup"><span data-stu-id="041a7-491">If it is, the `For` loop terminates; otherwise the loop block executes.</span></span> <span data-ttu-id="041a7-492">如果循环控制变量不是基元类型，则比较运算符取决于表达式 `step >= step - step` 是 true 还是 false。</span><span class="sxs-lookup"><span data-stu-id="041a7-492">If the loop control variable is not a primitive type, the comparison operator is determined by whether the expression `step >= step - step` is true or false.</span></span> <span data-ttu-id="041a7-493">在 `Next` 语句中，步骤值将添加到控件变量，并且执行将返回到循环的顶部。</span><span class="sxs-lookup"><span data-stu-id="041a7-493">At the `Next` statement, the step value is added to the control variable and execution returns to the top of the loop.</span></span>

<span data-ttu-id="041a7-494">请注意，*不*会在循环块的每个迭代上创建循环控制变量的新副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-494">Note that a new copy of the loop control variable is *not* created on each iteration of the loop block.</span></span> <span data-ttu-id="041a7-495">在这方面，`For` 语句不同于 `For Each` （[每个的部分 .。。Next 语句](statements.md#for-eachnext-statements)）。</span><span class="sxs-lookup"><span data-stu-id="041a7-495">In this respect, the `For` statement differs from `For Each` (Section [For Each...Next Statements](statements.md#for-eachnext-statements)).</span></span>

<span data-ttu-id="041a7-496">从循环外分支到 `For` 循环是无效的。</span><span class="sxs-lookup"><span data-stu-id="041a7-496">It is not valid to branch into a `For` loop from outside the loop.</span></span>


### <a name="for-eachnext-statements"></a><span data-ttu-id="041a7-497">对于每个 .。。Next 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-497">For Each...Next Statements</span></span>

<span data-ttu-id="041a7-498">@No__t-0 语句基于表达式中的元素循环。</span><span class="sxs-lookup"><span data-stu-id="041a7-498">A `For Each...Next` statement loops based on the elements in an expression.</span></span> <span data-ttu-id="041a7-499">@No__t-0 语句指定循环控制变量和枚举器表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-499">A `For Each` statement specifies a loop control variable and an enumerator expression.</span></span> <span data-ttu-id="041a7-500">循环控制变量是通过标识符指定的，后跟可选的 `As` 子句或表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-500">The loop control variable is specified either through an identifier followed by an optional `As` clause or an expression.</span></span>

```antlr
ForEachStatement
    : 'For' 'Each' LoopControlVariable 'In' LineTerminator? Expression StatementTerminator
      Block?
      ( 'Next' NextExpressionList? StatementTerminator )?
    ;
```

<span data-ttu-id="041a7-501">遵循相同的规则`For...Next`语句 (部分[对于。 ...下一语句](statements.md#fornext-statements))，循环控制变量是指到特定的新本地变量这对于每个...下一个语句，或到预先存在的变量，或表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-501">Following the same rules as `For...Next` statements (Section [For...Next Statements](statements.md#fornext-statements)), the loop control variable refers either to a new local variable specific to this For Each...Next statement, or to a pre-existing variable, or to an expression.</span></span>

<span data-ttu-id="041a7-502">枚举器表达式必须归类为值，其类型必须为集合类型或 `Object`。</span><span class="sxs-lookup"><span data-stu-id="041a7-502">The enumerator expression must be classified as a value and its type must be a collection type or `Object`.</span></span> <span data-ttu-id="041a7-503">如果枚举器表达式的类型为 `Object`，则所有处理会推迟到运行时。</span><span class="sxs-lookup"><span data-stu-id="041a7-503">If the type of the enumerator expression is `Object`, then all processing is deferred until run-time.</span></span> <span data-ttu-id="041a7-504">否则，必须从集合的元素类型中将转换转换为循环控制变量的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-504">Otherwise, a conversion must exist from the element type of the collection to the type of the loop control variable.</span></span>

<span data-ttu-id="041a7-505">循环控制变量不能被另一个封闭 `For Each` 语句使用。</span><span class="sxs-lookup"><span data-stu-id="041a7-505">The loop control variable cannot be used by another enclosing `For Each` statement.</span></span> <span data-ttu-id="041a7-506">@No__t-0 语句必须通过匹配的 `Next` 语句来关闭。</span><span class="sxs-lookup"><span data-stu-id="041a7-506">A `For Each` statement must be closed by a matching `Next` statement.</span></span> <span data-ttu-id="041a7-507">不含循环控制变量的 @no__t 0 语句匹配最内层的打开 `For Each`。</span><span class="sxs-lookup"><span data-stu-id="041a7-507">A `Next` statement without a loop control variable matches the innermost open `For Each`.</span></span> <span data-ttu-id="041a7-508">具有一个或多个循环控制变量的 @no__t 0 语句将从左到右与具有相同循环控制变量的 @no__t 1 循环匹配。</span><span class="sxs-lookup"><span data-stu-id="041a7-508">A `Next` statement with one or more loop control variables will, from left to right, match the `For Each` loops that have the same loop control variable.</span></span> <span data-ttu-id="041a7-509">如果变量与非最嵌套循环的 @no__t 0 循环匹配，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-509">If a variable matches a `For Each` loop that is not the most nested loop at that point, a compile-time error occurs.</span></span>

<span data-ttu-id="041a7-510">如果类型 @no__t，则称为*集合类型*，前提是：</span><span class="sxs-lookup"><span data-stu-id="041a7-510">A type `C` is said to be a *collection type* if one of:</span></span>

* <span data-ttu-id="041a7-511">以下所有条件均为 true：</span><span class="sxs-lookup"><span data-stu-id="041a7-511">All of the following are true:</span></span>
  * <span data-ttu-id="041a7-512">`C` 包含可访问的实例、具有签名 `GetEnumerator()` 的共享或扩展方法，该方法返回类型 `E`。</span><span class="sxs-lookup"><span data-stu-id="041a7-512">`C` contains an accessible instance, shared or extension method with the signature `GetEnumerator()` that returns a type `E`.</span></span>
  * <span data-ttu-id="041a7-513">`E` 包含可访问的实例、共享或扩展方法，签名 @no__t 为-1，返回类型为 `Boolean`。</span><span class="sxs-lookup"><span data-stu-id="041a7-513">`E` contains an accessible instance, shared or extension method with the signature `MoveNext()` and the return type `Boolean`.</span></span>
  * <span data-ttu-id="041a7-514">`E` 包含具有 getter 的名为 `Current` 的可访问实例或共享属性。</span><span class="sxs-lookup"><span data-stu-id="041a7-514">`E` contains an accessible instance or shared property named `Current` that has a getter.</span></span> <span data-ttu-id="041a7-515">此属性的类型是集合类型的元素类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-515">The type of this property is the element type of the collection type.</span></span>

* <span data-ttu-id="041a7-516">它实现接口 `System.Collections.Generic.IEnumerable(Of T)`，在这种情况下，集合的元素类型被视为 @no__t 为-1。</span><span class="sxs-lookup"><span data-stu-id="041a7-516">It implements the interface `System.Collections.Generic.IEnumerable(Of T)`, in which case the element type of the collection is considered to be `T`.</span></span>

* <span data-ttu-id="041a7-517">它实现接口 `System.Collections.IEnumerable`，在这种情况下，集合的元素类型被视为 @no__t 为-1。</span><span class="sxs-lookup"><span data-stu-id="041a7-517">It implements the interface `System.Collections.IEnumerable`, in which case the element type of the collection is considered to be `Object`.</span></span>

<span data-ttu-id="041a7-518">下面是一个可枚举的类的示例：</span><span class="sxs-lookup"><span data-stu-id="041a7-518">Following is an example of a class that can be enumerated:</span></span>

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

<span data-ttu-id="041a7-519">循环开始之前，计算枚举器表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-519">Before the loop begins, the enumerator expression is evaluated.</span></span> <span data-ttu-id="041a7-520">如果表达式的类型不满足设计模式，则会将表达式强制转换为 `System.Collections.IEnumerable` 或 @no__t 为-1。</span><span class="sxs-lookup"><span data-stu-id="041a7-520">If the type of the expression does not satisfy the design pattern, then the expression is cast to `System.Collections.IEnumerable` or `System.Collections.Generic.IEnumerable(Of T)`.</span></span> <span data-ttu-id="041a7-521">如果表达式类型实现泛型接口，则在编译时首选泛型接口，但在运行时首选非泛型接口。</span><span class="sxs-lookup"><span data-stu-id="041a7-521">If the expression type implements the generic interface, the generic interface is preferred at compile-time but the non-generic interface is preferred at run-time.</span></span> <span data-ttu-id="041a7-522">如果表达式类型多次实现泛型接口，则该语句将被视为不明确，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-522">If the expression type implements the generic interface multiple times, the statement is considered ambiguous and a compile-time error occurs.</span></span>

<span data-ttu-id="041a7-523">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-523">__Note.__</span></span> <span data-ttu-id="041a7-524">在后期绑定情况中首选非泛型接口，因为选取泛型接口意味着对接口方法的所有调用都涉及到类型参数。</span><span class="sxs-lookup"><span data-stu-id="041a7-524">The non-generic interface is preferred in the late bound case, because picking the generic interface would mean that all the calls to the interface methods would involve type parameters.</span></span> <span data-ttu-id="041a7-525">由于不能在运行时知道匹配类型参数，因此必须使用后期绑定调用来进行所有此类调用。</span><span class="sxs-lookup"><span data-stu-id="041a7-525">Since it is not possible to know the matching type arguments at run-time, all such calls would have to be made using late-bound calls.</span></span> <span data-ttu-id="041a7-526">这会比调用非泛型接口慢，因为使用编译时调用可以调用非泛型接口。</span><span class="sxs-lookup"><span data-stu-id="041a7-526">This would be slower than calling the non-generic interface because the non-generic interface could be called using compile-time calls.</span></span>

<span data-ttu-id="041a7-527">对结果值调用 `GetEnumerator`，并且函数的返回值存储在临时中。</span><span class="sxs-lookup"><span data-stu-id="041a7-527">`GetEnumerator` is called on the resulting value and the return value of the function is stored in a temporary.</span></span> <span data-ttu-id="041a7-528">然后，在每次迭代开始时，将对临时调用 `MoveNext`。</span><span class="sxs-lookup"><span data-stu-id="041a7-528">Then at the beginning of each iteration, `MoveNext` is called on the temporary.</span></span> <span data-ttu-id="041a7-529">如果它返回 `False`，则循环将终止。</span><span class="sxs-lookup"><span data-stu-id="041a7-529">If it returns `False`, the loop terminates.</span></span> <span data-ttu-id="041a7-530">否则，循环的每次迭代都按如下方式执行：</span><span class="sxs-lookup"><span data-stu-id="041a7-530">Otherwise, each iteration of the loop is executed as follows:</span></span>

1. <span data-ttu-id="041a7-531">如果循环控制变量识别了新的本地变量（而不是预先存在的变量），则将创建该局部变量的新副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-531">If the loop control variable identified a new local variable (rather than a pre-existing one), then a fresh copy of this local variable is created.</span></span> <span data-ttu-id="041a7-532">对于当前迭代，循环块内的所有引用都将引用此副本。</span><span class="sxs-lookup"><span data-stu-id="041a7-532">For the current iteration, all references within the loop block will refer to this copy.</span></span>
2. <span data-ttu-id="041a7-533">检索 `Current` 属性，将其强制转换为循环控制变量的类型（不管是隐式还是显式转换），并赋给循环控制变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-533">The `Current` property is retrieved, coerced to the type of the loop control variable (regardless of whether the conversion is implicit or explicit), and assigned to the loop control variable.</span></span>
3. <span data-ttu-id="041a7-534">循环块执行。</span><span class="sxs-lookup"><span data-stu-id="041a7-534">The loop block executes.</span></span>

<span data-ttu-id="041a7-535">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-535">__Note.__</span></span> <span data-ttu-id="041a7-536">语言版本10.0 和11.0 之间的行为稍有不同。</span><span class="sxs-lookup"><span data-stu-id="041a7-536">There is a slight change in behavior between version 10.0 and 11.0 of the language.</span></span> <span data-ttu-id="041a7-537">在11.0 之前，*不会*为循环的每次迭代创建新的迭代变量。</span><span class="sxs-lookup"><span data-stu-id="041a7-537">Prior to 11.0, a fresh iteration variable was *not* created for each iteration of the loop.</span></span> <span data-ttu-id="041a7-538">仅当迭代变量是通过 lambda 或 LINQ 表达式捕获的，然后在循环后调用后，这种差异才可观察：</span><span class="sxs-lookup"><span data-stu-id="041a7-538">This difference is observable only if the iteration variable is captured by a lambda or a LINQ expression which is then invoked after the loop:</span></span>

```vb
Dim lambdas As New List(Of Action)
For Each x In {1,2,3}
   lambdas.Add(Sub() Console.WriteLine(x)
Next
lambdas(0).Invoke()
lambdas(1).Invoke()
lambdas(2).Invoke()
```

<span data-ttu-id="041a7-539">最多 Visual Basic 10.0，这会在编译时生成警告，并将其打印三次。</span><span class="sxs-lookup"><span data-stu-id="041a7-539">Up to Visual Basic 10.0, this produced a warning at compile-time and printed "3" three times.</span></span> <span data-ttu-id="041a7-540">这是因为循环的所有迭代都只共享了一个变量 "x"，而所有三个 lambda 捕获了相同的 "x"，并在执行了 lambda 后，则将其保留为3。</span><span class="sxs-lookup"><span data-stu-id="041a7-540">That was because there was only a single variable "x" shared by all iterations of the loop, and all three lambdas captured the same "x", and by the time the lambdas were executed it then held the number 3.</span></span> <span data-ttu-id="041a7-541">从 Visual Basic 11.0，将打印 "1，2，3"。</span><span class="sxs-lookup"><span data-stu-id="041a7-541">As of Visual Basic 11.0, it prints "1, 2, 3".</span></span> <span data-ttu-id="041a7-542">这是因为每个 lambda 都捕获不同的变量 "x"。</span><span class="sxs-lookup"><span data-stu-id="041a7-542">That is because each lambda captures a different variable "x".</span></span>


<span data-ttu-id="041a7-543">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-543">__Note.__</span></span> <span data-ttu-id="041a7-544">即使在语句中没有方便地引入转换运算符，迭代的当前元素也会转换为循环控制变量的类型，即使转换是显式的。</span><span class="sxs-lookup"><span data-stu-id="041a7-544">The current element of the iteration is converted to the type of the loop control variable even if the conversion is explicit because there is no convenient place to introduce a conversion operator in the statement.</span></span> <span data-ttu-id="041a7-545">当使用过时的类型 `System.Collections.ArrayList` 时，这会变得非常麻烦，因为其元素类型 @no__t 为-1。</span><span class="sxs-lookup"><span data-stu-id="041a7-545">This became particularly troublesome when working with the now-obsolete type `System.Collections.ArrayList`, because its element type is `Object`.</span></span> <span data-ttu-id="041a7-546">这会在很多循环中产生必需的强制转换，但我们认为这并不理想。</span><span class="sxs-lookup"><span data-stu-id="041a7-546">This would have required casts in a great many loops, something we felt was not ideal.</span></span> <span data-ttu-id="041a7-547">具有讽刺意味，泛型启用了强类型集合 `System.Collections.Generic.List(Of T)`，这可能使我们重新思考此设计点，但为了实现兼容性，现在无法更改。</span><span class="sxs-lookup"><span data-stu-id="041a7-547">Ironically, generics enabled the creation of a strongly-typed collection, `System.Collections.Generic.List(Of T)`, which might have made us rethink this design point, but for compatibility's sake, this cannot be changed now.</span></span>


<span data-ttu-id="041a7-548">当到达 `Next` 语句时，执行将返回到循环的顶部。</span><span class="sxs-lookup"><span data-stu-id="041a7-548">When the `Next` statement is reached, execution returns to the top of the loop.</span></span> <span data-ttu-id="041a7-549">如果在 `Next` 关键字之后指定了变量，则它必须与 `For Each` 之后的第一个变量相同。</span><span class="sxs-lookup"><span data-stu-id="041a7-549">If a variable is specified after the `Next` keyword, it must be the same as the first variable after the `For Each`.</span></span> <span data-ttu-id="041a7-550">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="041a7-550">For example, consider the following code:</span></span>

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

<span data-ttu-id="041a7-551">它等效于以下代码：</span><span class="sxs-lookup"><span data-stu-id="041a7-551">It is equivalent to the following code:</span></span>

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

<span data-ttu-id="041a7-552">如果枚举器的类型 `E` 实现 `System.IDisposable`，则通过调用 `Dispose` 方法，在退出循环时释放枚举器。</span><span class="sxs-lookup"><span data-stu-id="041a7-552">If the type `E` of the enumerator implements `System.IDisposable`, then the enumerator is disposed upon exiting the loop by calling the `Dispose` method.</span></span> <span data-ttu-id="041a7-553">这可确保释放枚举器所持有的资源。</span><span class="sxs-lookup"><span data-stu-id="041a7-553">This ensures that resources held by the enumerator are released.</span></span> <span data-ttu-id="041a7-554">如果包含 `For Each` 语句的方法不使用非结构化错误处理，则将 @no__t 1 语句包装在第2条语句 @no__t 中，该语句具有 `Finally` 中调用的 `Dispose` 方法，以确保进行清理。</span><span class="sxs-lookup"><span data-stu-id="041a7-554">If the method containing the `For Each` statement does not use unstructured error handling, then the `For Each` statement is wrapped in a `Try` statement with the `Dispose` method called in the `Finally` to ensure cleanup.</span></span>

<span data-ttu-id="041a7-555">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-555">__Note.__</span></span> <span data-ttu-id="041a7-556">@No__t 类型是集合类型，并且由于所有数组类型都派生自 `System.Array`，因此在 `For Each` 语句中允许任何数组类型表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-556">The `System.Array` type is a collection type, and since all array types derive from `System.Array`, any array type expression is permitted in a `For Each` statement.</span></span> <span data-ttu-id="041a7-557">对于一维数组，`For Each` 语句会按递增的索引顺序枚举数组元素，从索引0开始，以索引长度-1 结束。</span><span class="sxs-lookup"><span data-stu-id="041a7-557">For single-dimensional arrays, the `For Each` statement enumerates the array elements in increasing index order, starting with index 0 and ending with index Length - 1.</span></span> <span data-ttu-id="041a7-558">对于多维数组，首先增加最右侧维度的索引。</span><span class="sxs-lookup"><span data-stu-id="041a7-558">For multidimensional arrays, the indices of the rightmost dimension are increased first.</span></span>

<span data-ttu-id="041a7-559">例如，以下代码将打印 `1 2 3 4`：</span><span class="sxs-lookup"><span data-stu-id="041a7-559">For example, the following code prints `1 2 3 4`:</span></span>

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

<span data-ttu-id="041a7-560">分支到块外的 @no__t 0 语句块中是无效的。</span><span class="sxs-lookup"><span data-stu-id="041a7-560">It is not valid to branch into a `For Each` statement block from outside the block.</span></span>


## <a name="exception-handling-statements"></a><span data-ttu-id="041a7-561">异常处理语句</span><span class="sxs-lookup"><span data-stu-id="041a7-561">Exception-Handling Statements</span></span>

<span data-ttu-id="041a7-562">Visual Basic 支持结构化异常处理和非结构化异常处理。</span><span class="sxs-lookup"><span data-stu-id="041a7-562">Visual Basic supports structured exception handling and unstructured exception handling.</span></span> <span data-ttu-id="041a7-563">在方法中只能使用一种类型的异常处理，但 `Error` 语句可用于结构化异常处理。</span><span class="sxs-lookup"><span data-stu-id="041a7-563">Only one style of exception handling may be used in a method, but the `Error` statement may be used in structured exception handling.</span></span> <span data-ttu-id="041a7-564">如果方法使用这两种样式的异常处理，则会产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-564">If a method uses both styles of exception handling, a compile-time error results.</span></span>

```antlr
ErrorHandlingStatement
    : StructuredErrorStatement
    | UnstructuredErrorStatement
    ;
```

### <a name="structured-exception-handling-statements"></a><span data-ttu-id="041a7-565">结构化异常处理语句</span><span class="sxs-lookup"><span data-stu-id="041a7-565">Structured Exception-Handling Statements</span></span>

<span data-ttu-id="041a7-566">结构化异常处理是一种处理错误的方法，即在其中处理某些异常的显式块。</span><span class="sxs-lookup"><span data-stu-id="041a7-566">Structured exception handling is a method of handling errors by declaring explicit blocks within which certain exceptions will be handled.</span></span> <span data-ttu-id="041a7-567">结构化异常处理通过 `Try` 语句来完成。</span><span class="sxs-lookup"><span data-stu-id="041a7-567">Structured exception handling is done through a `Try` statement.</span></span>

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


<span data-ttu-id="041a7-568">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-568">For example:</span></span>

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

<span data-ttu-id="041a7-569">@No__t-0 语句由三种块组成： try 块、catch 块和 finally 块。</span><span class="sxs-lookup"><span data-stu-id="041a7-569">A `Try` statement is made up of three kinds of blocks: try blocks, catch blocks, and finally blocks.</span></span> <span data-ttu-id="041a7-570">*Try 块*是包含要执行的语句的语句块。</span><span class="sxs-lookup"><span data-stu-id="041a7-570">A *try block* is a statement block that contains the statements to be executed.</span></span> <span data-ttu-id="041a7-571">*Catch 块*是处理异常的语句块。</span><span class="sxs-lookup"><span data-stu-id="041a7-571">A *catch block* is a statement block that handles an exception.</span></span> <span data-ttu-id="041a7-572">*Finally 块*是一个语句块，其中包含在退出 `Try` 语句时要运行的语句，而不考虑是否已发生和处理异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-572">A *finally block* is a statement block that contains statements to be run when the `Try` statement is exited, regardless of whether an exception has occurred and been handled.</span></span> <span data-ttu-id="041a7-573">@No__t-0 语句，只能包含一个 try 块和一个 finally 块，必须至少包含一个 catch 块或 finally 块。</span><span class="sxs-lookup"><span data-stu-id="041a7-573">A `Try` statement, which can only contain one try block and one finally block, must contain at least one catch block or finally block.</span></span> <span data-ttu-id="041a7-574">将执行显式传输到 try 块中是无效的，除非在同一语句中的 catch 块内。</span><span class="sxs-lookup"><span data-stu-id="041a7-574">It is invalid to explicitly transfer execution into a try block except from within a catch block in the same statement.</span></span>


#### <a name="finally-blocks"></a><span data-ttu-id="041a7-575">Finally 块</span><span class="sxs-lookup"><span data-stu-id="041a7-575">Finally Blocks</span></span>

<span data-ttu-id="041a7-576">当执行离开 @no__t 语句的任何部分时，始终会执行 `Finally` 块。</span><span class="sxs-lookup"><span data-stu-id="041a7-576">A `Finally` block is always executed when execution leaves any part of the `Try` statement.</span></span> <span data-ttu-id="041a7-577">不需要显式操作即可执行 `Finally` 块;当执行离开 `Try` 语句时，系统将自动执行 @no__t 2 块，然后将执行转移到其预期目标。</span><span class="sxs-lookup"><span data-stu-id="041a7-577">No explicit action is required to execute the `Finally` block; when execution leaves the `Try` statement, the system will automatically execute the `Finally` block and then transfer execution to its intended destination.</span></span> <span data-ttu-id="041a7-578">无论执行如何离开 @no__t 语句，都将执行 `Finally` 块：通过 `Try` 块的末尾 @no__t，通过 @no__t 语句，通过 @no__t 语句，或不处理引发的异常而执行的操作的时间块为。</span><span class="sxs-lookup"><span data-stu-id="041a7-578">The `Finally` block is executed regardless of how execution leaves the `Try` statement: through the end of the `Try` block, through the end of a `Catch` block, through an `Exit Try` statement, through a `GoTo` statement, or by not handling a thrown exception.</span></span>

<span data-ttu-id="041a7-579">请注意，异步方法中的 @no__t 0 表达式和迭代器方法中的 @no__t 语句会导致在 async 或 iterator 方法实例中挂起控制流，并在其他方法实例中恢复。</span><span class="sxs-lookup"><span data-stu-id="041a7-579">Note that the `Await` expression in an async method, and the `Yield` statement in an iterator method, can cause flow of control to suspend in the async or iterator method instance and resume in some other method instance.</span></span> <span data-ttu-id="041a7-580">但是，这只是一种暂停执行，不涉及退出各自的 async 方法或迭代器方法实例，因此不会导致执行 `Finally` 块。</span><span class="sxs-lookup"><span data-stu-id="041a7-580">However, this is merely a suspension of execution and does not involve exiting the respective async method or iterator method instance, and so does not cause `Finally` blocks to be executed.</span></span>

<span data-ttu-id="041a7-581">将执行显式传输到 `Finally` 块无效：除了通过异常以外，将执行从 `Finally` 块传输的操作也无效。</span><span class="sxs-lookup"><span data-stu-id="041a7-581">It is invalid to explicitly transfer execution into a `Finally` block; it is also invalid to transfer execution out of a `Finally` block except through an exception.</span></span>

```antlr
FinallyStatement
    : 'Finally' StatementTerminator
      Block?
    ;
```

#### <a name="catch-blocks"></a><span data-ttu-id="041a7-582">catch 块</span><span class="sxs-lookup"><span data-stu-id="041a7-582">Catch Blocks</span></span>

<span data-ttu-id="041a7-583">如果处理 `Try` 块时出现异常，则将按文本顺序检查每个 @no__t 1 语句，以确定它是否处理异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-583">If an exception occurs while processing the `Try` block, each `Catch` statement is examined in textual order to determine if it handles the exception.</span></span>

```antlr
CatchStatement
    : 'Catch' ( Identifier ( 'As' NonArrayTypeName )? )?
      ( 'When' BooleanExpression )? StatementTerminator
      Block?
    ;
```

<span data-ttu-id="041a7-584">@No__t-0 子句中指定的标识符表示已引发的异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-584">The identifier specified in a `Catch` clause represents the exception that has been thrown.</span></span> <span data-ttu-id="041a7-585">如果标识符包含 `As` 子句，则认为该标识符是在 @no__t 1 块的本地声明空间内声明的。</span><span class="sxs-lookup"><span data-stu-id="041a7-585">If the identifier contains an `As` clause, then the identifier is considered to be declared within the `Catch` block's local declaration space.</span></span> <span data-ttu-id="041a7-586">否则，标识符必须是在包含块中定义的局部变量（而非静态变量）。</span><span class="sxs-lookup"><span data-stu-id="041a7-586">Otherwise, the identifier must be a local variable (not a static variable) that was defined in a containing block.</span></span>

<span data-ttu-id="041a7-587">不带标识符的 @no__t 0 子句将捕获从 @no__t 派生的所有异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-587">A `Catch` clause with no identifier will catch all exceptions derived from `System.Exception`.</span></span> <span data-ttu-id="041a7-588">带有标识符的 @no__t 0 子句仅捕获其类型与标识符的类型相同或派生的异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-588">A `Catch` clause with an identifier will only catch exceptions whose types are the same as or derived from the type of the identifier.</span></span> <span data-ttu-id="041a7-589">类型必须为 `System.Exception` 或从 @no__t 派生的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-589">The type must be `System.Exception`, or a type derived from `System.Exception`.</span></span> <span data-ttu-id="041a7-590">当捕获派生自 `System.Exception` 的异常时，对异常对象的引用将存储在函数返回的对象 `Microsoft.VisualBasic.Information.Err` 中。</span><span class="sxs-lookup"><span data-stu-id="041a7-590">When an exception is caught that derives from `System.Exception`, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="041a7-591">带 @no__t 子句的 `Catch` 子句仅会在表达式的计算结果为 `True` 时捕获异常;表达式的类型必须为布尔表达式，如每节[布尔表达式](expressions.md#boolean-expressions)。</span><span class="sxs-lookup"><span data-stu-id="041a7-591">A `Catch` clause with a `When` clause will only catch exceptions when the expression evaluates to `True`; the type of the expression must be a Boolean expression as per Section [Boolean Expressions](expressions.md#boolean-expressions).</span></span> <span data-ttu-id="041a7-592">仅在检查异常的类型后才应用 `When` 子句，并且表达式可以引用表示异常的标识符，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="041a7-592">A `When` clause is only applied after checking the type of the exception, and the expression may refer to the identifier representing the exception, as this example demonstrates:</span></span>

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

<span data-ttu-id="041a7-593">此示例将打印：</span><span class="sxs-lookup"><span data-stu-id="041a7-593">This example prints:</span></span>

```console
Third handler
```

<span data-ttu-id="041a7-594">如果 @no__t 0 子句处理异常，执行将传输到 @no__t 块。</span><span class="sxs-lookup"><span data-stu-id="041a7-594">If a `Catch` clause handles the exception, execution transfers to the `Catch` block.</span></span> <span data-ttu-id="041a7-595">在 `Catch` 块的末尾，执行将传输到 @no__t 语句后面的第一条语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-595">At the end of the `Catch` block, execution transfers to the first statement following the `Try` statement.</span></span> <span data-ttu-id="041a7-596">@No__t-0 语句将不处理 @no__t 块中引发的任何异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-596">The `Try` statement will not handle any exceptions thrown in a `Catch` block.</span></span> <span data-ttu-id="041a7-597">如果没有 `Catch` 子句处理异常，则执行会传输到系统确定的位置。</span><span class="sxs-lookup"><span data-stu-id="041a7-597">If no `Catch` clause handles the exception, execution transfers to a location determined by the system.</span></span>

<span data-ttu-id="041a7-598">将执行显式传输到 `Catch` 块中是无效的。</span><span class="sxs-lookup"><span data-stu-id="041a7-598">It is invalid to explicitly transfer execution into a `Catch` block.</span></span>

<span data-ttu-id="041a7-599">通常在引发异常之前计算 When 子句中的筛选器。</span><span class="sxs-lookup"><span data-stu-id="041a7-599">The filters in When clauses are normally evaluated prior to the exception being thrown.</span></span> <span data-ttu-id="041a7-600">例如，以下代码将打印 "Filter，Finally，Catch"。</span><span class="sxs-lookup"><span data-stu-id="041a7-600">For instance, the following code will print "Filter, Finally, Catch".</span></span>

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

 <span data-ttu-id="041a7-601">但是，Async 和 Iterator 方法会导致在任何筛选器之外的任何筛选器之前执行它们内部的所有 finally 块。</span><span class="sxs-lookup"><span data-stu-id="041a7-601">However, Async and Iterator methods cause all finally blocks inside them to be executed prior to any filters outside.</span></span> <span data-ttu-id="041a7-602">例如，如果上面的代码 `Async Sub Foo()`，则输出将为 "Finally，Filter，Catch"。</span><span class="sxs-lookup"><span data-stu-id="041a7-602">For instance, if the above code had `Async Sub Foo()`, then the output would be "Finally, Filter, Catch".</span></span>


#### <a name="throw-statement"></a><span data-ttu-id="041a7-603">Throw 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-603">Throw Statement</span></span>

<span data-ttu-id="041a7-604">@No__t-0 语句引发异常，该异常由派生自 @no__t 的类型的实例表示。</span><span class="sxs-lookup"><span data-stu-id="041a7-604">The `Throw` statement raises an exception, which is represented by an instance of a type derived from `System.Exception`.</span></span>

```antlr
ThrowStatement
    : 'Throw' Expression? StatementTerminator
    ;
```

<span data-ttu-id="041a7-605">如果表达式未分类为值或不是派生自 `System.Exception` 的类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-605">If the expression is not classified as a value or is not a type derived from `System.Exception`, then a compile-time error occurs.</span></span> <span data-ttu-id="041a7-606">如果表达式在运行时计算为 null 值，则改为引发 `System.NullReferenceException` 异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-606">If the expression evaluates to a null value at run time, then a `System.NullReferenceException` exception is raised instead.</span></span>

<span data-ttu-id="041a7-607">只要没有干预 finally 块，`Throw` 语句就可以省略 `Try` 语句 catch 块内的表达式。</span><span class="sxs-lookup"><span data-stu-id="041a7-607">A `Throw` statement may omit the expression within a catch block of a `Try` statement, as long as there is no intervening finally block.</span></span> <span data-ttu-id="041a7-608">在这种情况下，该语句重新引发 catch 块中当前正在处理的异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-608">In that case, the statement rethrows the exception currently being handled within the catch block.</span></span> <span data-ttu-id="041a7-609">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-609">For example:</span></span>

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


### <a name="unstructured-exception-handling-statements"></a><span data-ttu-id="041a7-610">非结构化异常处理语句</span><span class="sxs-lookup"><span data-stu-id="041a7-610">Unstructured Exception-Handling Statements</span></span>

<span data-ttu-id="041a7-611">非结构化异常处理是一种处理错误的方法，指示在发生异常时要分支到的语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-611">Unstructured exception handling is a method of handling errors by indicating statements to branch to when an exception occurs.</span></span> <span data-ttu-id="041a7-612">非结构化异常处理是使用三个语句实现的： `Error` 语句、@no__t 1 语句和 `Resume` 语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-612">Unstructured exception handling is implemented using three statements: the `Error` statement, the `On Error` statement, and the `Resume` statement.</span></span>

```antlr
UnstructuredErrorStatement
    : ErrorStatement
    | OnErrorStatement
    | ResumeStatement
    ;
```

<span data-ttu-id="041a7-613">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-613">For example:</span></span>

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

<span data-ttu-id="041a7-614">当方法使用非结构化异常处理时，将为全部捕获所有异常的方法建立单个结构化异常处理程序。</span><span class="sxs-lookup"><span data-stu-id="041a7-614">When a method uses unstructured exception handling, a single structured exception handler is established for the entire method that catches all exceptions.</span></span> <span data-ttu-id="041a7-615">（请注意，在构造函数中，此处理程序不会将对的调用扩展到对构造函数开头 `New` 的调用。）然后，方法跟踪最近引发的异常处理程序位置和最近发生的异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-615">(Note that in constructors this handler does not extend over the call to the call to `New` at the beginning of the constructor.) The method then keeps track of the most recent exception-handler location and the most recent exception that has been thrown.</span></span> <span data-ttu-id="041a7-616">进入方法时，异常处理程序位置和异常均设置为 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="041a7-616">At entry to the method, the exception-handler location and the exception are both set to `Nothing`.</span></span> <span data-ttu-id="041a7-617">当使用非结构化异常处理的方法中引发异常时，对异常对象的引用存储在函数返回的对象中，`Microsoft.VisualBasic.Information.Err`。</span><span class="sxs-lookup"><span data-stu-id="041a7-617">When an exception is thrown in a method that uses unstructured exception handling, a reference to the exception object is stored in the object returned by the function `Microsoft.VisualBasic.Information.Err`.</span></span>

<span data-ttu-id="041a7-618">迭代器或异步方法中不允许使用非结构化错误处理语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-618">Unstructured error handling statements are not allowed in iterator or async methods.</span></span>


#### <a name="error-statement"></a><span data-ttu-id="041a7-619">Error 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-619">Error Statement</span></span>

<span data-ttu-id="041a7-620">@No__t-0 语句将引发一个 @no__t 为1的异常，该异常包含一个 Visual Basic 6 异常号。</span><span class="sxs-lookup"><span data-stu-id="041a7-620">An `Error` statement throws a `System.Exception` exception containing a Visual Basic 6 exception number.</span></span> <span data-ttu-id="041a7-621">表达式必须分类为值，并且其类型必须可隐式转换为 `Integer`。</span><span class="sxs-lookup"><span data-stu-id="041a7-621">The expression must be classified as a value and its type must be implicitly convertible to `Integer`.</span></span>

```antlr
ErrorStatement
    : 'Error' Expression StatementTerminator
    ;
```

#### <a name="on-error-statement"></a><span data-ttu-id="041a7-622">On Error 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-622">On Error Statement</span></span>

<span data-ttu-id="041a7-623">@No__t-0 语句修改最新的异常处理状态。</span><span class="sxs-lookup"><span data-stu-id="041a7-623">An `On Error` statement modifies the most recent exception-handling state.</span></span>

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

<span data-ttu-id="041a7-624">它可以通过以下四种方式之一使用：</span><span class="sxs-lookup"><span data-stu-id="041a7-624">It may be used in one of four ways:</span></span>

* <span data-ttu-id="041a7-625">`On Error GoTo -1` 会将最近的异常重置为 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="041a7-625">`On Error GoTo -1` resets the most recent exception to `Nothing`.</span></span>

* <span data-ttu-id="041a7-626">`On Error GoTo 0` 会将最新的异常处理程序位置重置为 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="041a7-626">`On Error GoTo 0` resets the most recent exception-handler location to `Nothing`.</span></span>

* <span data-ttu-id="041a7-627">`On Error GoTo LabelName` 将标签建立为最新的异常处理程序位置。</span><span class="sxs-lookup"><span data-stu-id="041a7-627">`On Error GoTo LabelName` establishes the label as the most recent exception-handler location.</span></span> <span data-ttu-id="041a7-628">此语句不能用于包含 lambda 或查询表达式的方法中。</span><span class="sxs-lookup"><span data-stu-id="041a7-628">This statement cannot be used in a method that contains a lambda or query expression.</span></span>

* <span data-ttu-id="041a7-629">`On Error Resume Next` 会将 @no__t 一行为建立为最新的异常处理程序位置。</span><span class="sxs-lookup"><span data-stu-id="041a7-629">`On Error Resume Next` establishes the `Resume Next` behavior as the most recent exception-handler location.</span></span>


#### <a name="resume-statement"></a><span data-ttu-id="041a7-630">Resume 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-630">Resume Statement</span></span>

<span data-ttu-id="041a7-631">@No__t-0 语句返回对导致最近异常的语句执行。</span><span class="sxs-lookup"><span data-stu-id="041a7-631">A `Resume` statement returns execution to the statement that caused the most recent exception.</span></span>

```antlr
ResumeStatement
    : 'Resume' ResumeClause? StatementTerminator
    ;

ResumeClause
    : 'Next'
    | LabelName
    ;
```

<span data-ttu-id="041a7-632">如果指定了 `Next` 修饰符，则执行将返回到在导致最近异常的语句之后执行的语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-632">If the `Next` modifier is specified, execution returns to the statement that would have been executed after the statement that caused the most recent exception.</span></span> <span data-ttu-id="041a7-633">如果指定了标签名称，则执行将返回到标签。</span><span class="sxs-lookup"><span data-stu-id="041a7-633">If a label name is specified, execution returns to the label.</span></span>

<span data-ttu-id="041a7-634">因为 `SyncLock` 语句包含隐式结构化错误处理块，所以 @no__t 为-1，`Resume Next` 对于在 @no__t 3 语句中发生的异常具有特殊行为。</span><span class="sxs-lookup"><span data-stu-id="041a7-634">Because the `SyncLock` statement contains an implicit structured error-handling block, `Resume` and `Resume Next` have special behaviors for exceptions that occur in `SyncLock` statements.</span></span> <span data-ttu-id="041a7-635">`Resume` 会将执行返回到 `SyncLock` 语句的开头，而 `Resume Next` 则将执行返回到 @no__t 3 语句后面的下一个语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-635">`Resume` returns execution to the beginning of the `SyncLock` statement, while `Resume Next` returns execution to the next statement following the `SyncLock` statement.</span></span> <span data-ttu-id="041a7-636">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="041a7-636">For example, consider the following code:</span></span>

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

<span data-ttu-id="041a7-637">它打印以下结果。</span><span class="sxs-lookup"><span data-stu-id="041a7-637">It prints the following result.</span></span>

```console
Before exception
Before exception
After SyncLock
```

<span data-ttu-id="041a7-638">第一次通过 `SyncLock` 语句时，`Resume` 会将执行返回到 @no__t 语句的开头。</span><span class="sxs-lookup"><span data-stu-id="041a7-638">The first time through the `SyncLock` statement, `Resume` returns execution to the beginning of the `SyncLock` statement.</span></span> <span data-ttu-id="041a7-639">第二次通过 `SyncLock` 语句时，`Resume Next` 会将执行返回到 @no__t 语句的末尾。</span><span class="sxs-lookup"><span data-stu-id="041a7-639">The second time through the `SyncLock` statement, `Resume Next` returns execution to the end of the `SyncLock` statement.</span></span> <span data-ttu-id="041a7-640">不允许在 `SyncLock` 语句中使用 `Resume` 和 @no__t。</span><span class="sxs-lookup"><span data-stu-id="041a7-640">`Resume` and `Resume Next` are not allowed within a `SyncLock` statement.</span></span>

<span data-ttu-id="041a7-641">在所有情况下，当执行 `Resume` 语句时，最新异常将设置为 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="041a7-641">In all cases, when a `Resume` statement is executed, the most recent exception is set to `Nothing`.</span></span> <span data-ttu-id="041a7-642">如果在没有最近的异常的情况下执行 `Resume` 语句，该语句将引发一个 @no__t 为1的异常，该异常包含 Visual Basic 错误号 `20` （Resume，无错误）。</span><span class="sxs-lookup"><span data-stu-id="041a7-642">If a `Resume` statement is executed with no most recent exception, the statement raises a `System.Exception` exception containing the Visual Basic error number `20` (Resume without error).</span></span>


## <a name="branch-statements"></a><span data-ttu-id="041a7-643">Branch 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-643">Branch Statements</span></span>

<span data-ttu-id="041a7-644">Branch 语句修改方法中的执行流。</span><span class="sxs-lookup"><span data-stu-id="041a7-644">Branch statements modify the flow of execution in a method.</span></span> <span data-ttu-id="041a7-645">有六个分支语句：</span><span class="sxs-lookup"><span data-stu-id="041a7-645">There are six branch statements:</span></span>

1. <span data-ttu-id="041a7-646">@No__t-0 语句会导致执行传输到方法中的指定标签。</span><span class="sxs-lookup"><span data-stu-id="041a7-646">A `GoTo` statement causes execution to transfer to the specified label in the method.</span></span> <span data-ttu-id="041a7-647">不允许在 lambda 或 LINQ 表达式中捕获某个块的本地变量的情况下，将 @no__t-@no__t 0、`Using`、`SyncLock`、`With`、`For` 或 `For Each` 块以及任何循环块。</span><span class="sxs-lookup"><span data-stu-id="041a7-647">It is not allowed to `GoTo` into a `Try`, `Using`, `SyncLock`, `With`, `For` or `For Each` block, nor into any loop block if a local variable of that block is captured in a lambda or LINQ expression.</span></span>
2. <span data-ttu-id="041a7-648">@No__t-0 语句将执行转移到指定种类的直接包含块语句末尾后的下一条语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-648">An `Exit` statement transfers execution to the next statement after the end of the immediately containing block statement of the specified kind.</span></span> <span data-ttu-id="041a7-649">如果块为方法块，则控制流将退出方法，如[控制流](statements.md#control-flow)中所述。</span><span class="sxs-lookup"><span data-stu-id="041a7-649">If the block is the method block, then control flow exits the method as described in Section [Control Flow](statements.md#control-flow).</span></span> <span data-ttu-id="041a7-650">如果未在语句中指定的块类型内包含 `Exit` 语句，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-650">If the `Exit` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
3. <span data-ttu-id="041a7-651">@No__t-0 语句将执行转移到指定种类的立即包含块循环体语句的末尾。</span><span class="sxs-lookup"><span data-stu-id="041a7-651">A `Continue` statement transfers execution to the end of the immediately containing block loop statement of the specified kind.</span></span> <span data-ttu-id="041a7-652">如果未在语句中指定的块类型内包含 `Continue` 语句，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-652">If the `Continue` statement is not contained within the kind of block specified in the statement, a compile-time error occurs.</span></span>
4. <span data-ttu-id="041a7-653">@No__t-0 语句导致引发调试器异常。</span><span class="sxs-lookup"><span data-stu-id="041a7-653">A `Stop` statement causes a debugger exception to occur.</span></span>
5. <span data-ttu-id="041a7-654">@No__t-0 语句将终止该程序。</span><span class="sxs-lookup"><span data-stu-id="041a7-654">An `End` statement terminates the program.</span></span> <span data-ttu-id="041a7-655">终结器将在关闭前运行，但不会执行任何当前正在执行的 @no__t 0 语句的 finally 块。</span><span class="sxs-lookup"><span data-stu-id="041a7-655">Finalizers are run before shutdown, but the finally blocks of any currently executing `Try` statements are not executed.</span></span> <span data-ttu-id="041a7-656">此语句不能用于不可执行的程序（例如 Dll）。</span><span class="sxs-lookup"><span data-stu-id="041a7-656">This statement may not be used in programs that are not executable (for example, DLLs).</span></span>
6. <span data-ttu-id="041a7-657">不带 expression 的 @no__t 0 语句等效于 @no__t 或 `Exit Function` 语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-657">A `Return` statement with no expression is equivalent to an `Exit Sub` or `Exit Function` statement.</span></span> <span data-ttu-id="041a7-658">带有表达式的 @no__t 0 语句仅允许在作为函数的常规方法中，或在异步方法中使用，此方法在某种 `T` 的返回类型为 `Task(Of T)` 的函数。</span><span class="sxs-lookup"><span data-stu-id="041a7-658">A `Return` statement with an expression is only allowed in a regular method that is a function, or in an async method that is a function with return type `Task(Of T)` for some `T`.</span></span> <span data-ttu-id="041a7-659">表达式必须归类为一个值，该值可隐式转换为*函数返回变量*（对于常规方法）或*任务返回变量*（对于异步方法）。</span><span class="sxs-lookup"><span data-stu-id="041a7-659">The expression must be classified as a value which is implicitly convertible to the *function return variable* (in the case of regular methods) or to the *task return variable* (in the case of async methods).</span></span> <span data-ttu-id="041a7-660">其行为是评估其表达式，然后将其存储在返回变量中，然后执行隐式 @no__t 0 语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-660">Its behavior is to evaluate its expression, then store it in the return variable, then execute an implicit `Exit Function` statement.</span></span>

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

## <a name="array-handling-statements"></a><span data-ttu-id="041a7-661">数组处理语句</span><span class="sxs-lookup"><span data-stu-id="041a7-661">Array-Handling Statements</span></span>

<span data-ttu-id="041a7-662">以下两个语句简化了使用数组的操作： `ReDim` 语句和 `Erase` 语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-662">Two statements simplify working with arrays: `ReDim` statements and `Erase` statements.</span></span>

```antlr
ArrayHandlingStatement
    : RedimStatement
    | EraseStatement
    ;
```

### <a name="redim-statement"></a><span data-ttu-id="041a7-663">ReDim 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-663">ReDim Statement</span></span>

<span data-ttu-id="041a7-664">@No__t-0 语句实例化新的数组。</span><span class="sxs-lookup"><span data-stu-id="041a7-664">A `ReDim` statement instantiates new arrays.</span></span>

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

<span data-ttu-id="041a7-665">语句中的每个子句都必须归类为变量或属性访问（其类型为数组类型或 `Object`），后跟数组界限的列表。</span><span class="sxs-lookup"><span data-stu-id="041a7-665">Each clause in the statement must be classified as a variable or a property access whose type is an array type or `Object`, and be followed by a list of array bounds.</span></span> <span data-ttu-id="041a7-666">边界的数目必须与变量的类型一致;@no__t 允许使用任意数量的界限。</span><span class="sxs-lookup"><span data-stu-id="041a7-666">The number of the bounds must be consistent with the type of the variable; any number of bounds is allowed for `Object`.</span></span> <span data-ttu-id="041a7-667">在运行时，将使用指定的边界从左到右对每个表达式实例化一个数组，然后将其分配给变量或属性。</span><span class="sxs-lookup"><span data-stu-id="041a7-667">At run time, an array is instantiated for each expression from left to right with the specified bounds and then assigned to the variable or property.</span></span> <span data-ttu-id="041a7-668">如果变量类型为 `Object`，则维度数量为指定的维数，数组元素类型为 `Object`。</span><span class="sxs-lookup"><span data-stu-id="041a7-668">If the variable type is `Object`, the number of dimensions is the number of dimensions specified, and the array element type is `Object`.</span></span> <span data-ttu-id="041a7-669">如果给定的维度数在运行时与变量或属性不兼容，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-669">If the given number of dimensions is incompatible with the variable or property at run time a compile-time error occurs.</span></span> <span data-ttu-id="041a7-670">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-670">For example:</span></span>

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

<span data-ttu-id="041a7-671">如果指定了 @no__t 0 关键字，则还必须将表达式 classifiable 为值，并且除最右边的维度之外的每个维度的新大小必须与现有数组的大小相同。</span><span class="sxs-lookup"><span data-stu-id="041a7-671">If the `Preserve` keyword is specified, then the expressions must also be classifiable as a value, and the new size for each dimension except for the rightmost one must be the same as the size of the existing array.</span></span> <span data-ttu-id="041a7-672">现有数组中的值将复制到新数组中：如果新数组较小，则放弃现有值;如果新数组较大，则会将额外的元素初始化为数组元素类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="041a7-672">The values in the existing array are copied into the new array: if the new array is smaller, the existing values are discarded; if the new array is bigger, the extra elements will be initialized to the default value of the element type of the array.</span></span> <span data-ttu-id="041a7-673">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="041a7-673">For example, consider the following code:</span></span>

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

<span data-ttu-id="041a7-674">它打印以下结果：</span><span class="sxs-lookup"><span data-stu-id="041a7-674">It prints the following result:</span></span>

```console
3, 0
```

<span data-ttu-id="041a7-675">如果在运行时现有数组引用为 null 值，则不会给出任何错误。</span><span class="sxs-lookup"><span data-stu-id="041a7-675">If the existing array reference is a null value at run time, no error is given.</span></span> <span data-ttu-id="041a7-676">如果维度的大小发生更改，则不是最右边的维度，将引发 `System.ArrayTypeMismatchException`。</span><span class="sxs-lookup"><span data-stu-id="041a7-676">Other than the rightmost dimension, if the size of a dimension changes, a `System.ArrayTypeMismatchException` will be thrown.</span></span>

<span data-ttu-id="041a7-677">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="041a7-677">__Note.__</span></span> <span data-ttu-id="041a7-678">`Preserve` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="041a7-678">`Preserve` is not a reserved word.</span></span>


### <a name="erase-statement"></a><span data-ttu-id="041a7-679">Erase 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-679">Erase Statement</span></span>

<span data-ttu-id="041a7-680">@No__t-0 语句将语句中指定的每个数组变量或属性设置为 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="041a7-680">An `Erase` statement sets each of the array variables or properties specified in the statement to `Nothing`.</span></span> <span data-ttu-id="041a7-681">语句中的每个表达式都必须归类为变量或属性访问，其类型为数组类型或 `Object`。</span><span class="sxs-lookup"><span data-stu-id="041a7-681">Each expression in the statement must be classified as a variable or property access whose type is an array type or `Object`.</span></span> <span data-ttu-id="041a7-682">例如：</span><span class="sxs-lookup"><span data-stu-id="041a7-682">For example:</span></span>

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

## <a name="using-statement"></a><span data-ttu-id="041a7-683">Using 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-683">Using statement</span></span>

<span data-ttu-id="041a7-684">当集合运行且未找到对实例的实时引用时，垃圾回收器将自动释放类型的实例。</span><span class="sxs-lookup"><span data-stu-id="041a7-684">Instances of types are automatically released by the garbage collector when a collection is run and no live references to the instance are found.</span></span> <span data-ttu-id="041a7-685">如果类型保存在特别宝贵的资源上（例如数据库连接或文件句柄），则可能不需要等待下一次垃圾回收，从而清除不再使用的类型的特定实例。</span><span class="sxs-lookup"><span data-stu-id="041a7-685">If a type holds on a particularly valuable and scarce resource (such as database connections or file handles), it may not be desirable to wait until the next garbage collection to clean up a particular instance of the type that is no longer in use.</span></span> <span data-ttu-id="041a7-686">为了提供一种在集合之前释放资源的轻型方法，类型可以实现 `System.IDisposable` 接口。</span><span class="sxs-lookup"><span data-stu-id="041a7-686">To provide a lightweight way of releasing resources before a collection, a type may implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="041a7-687">这样做的一种类型将公开一个 @no__t 0 方法，该方法可通过调用来强制立即释放宝贵的资源，如下所示：</span><span class="sxs-lookup"><span data-stu-id="041a7-687">A type that does so exposes a `Dispose` method that can be called to force valuable resources to be released immediately, as such:</span></span>

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

<span data-ttu-id="041a7-688">@No__t-0 语句会自动执行获取资源、执行一组语句，然后释放资源的过程。</span><span class="sxs-lookup"><span data-stu-id="041a7-688">The `Using` statement automates the process of acquiring a resource, executing a set of statements, and then disposing of the resource.</span></span> <span data-ttu-id="041a7-689">语句可以采用两种形式：在一种情况下，资源是声明为语句的一部分并被视为常规局部变量声明语句的本地变量;而在另一种情况下，资源是表达式的结果。</span><span class="sxs-lookup"><span data-stu-id="041a7-689">The statement can take two forms: in one, the resource is a local variable declared as a part of the statement and treated as a regular local variable declaration statement; in the other, the resource is the result of an expression.</span></span>

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

<span data-ttu-id="041a7-690">如果资源是局部变量声明语句，则局部变量声明的类型必须是可以隐式转换为 `System.IDisposable` 的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-690">If the resource is a local variable declaration statement then the type of the local variable declaration must be a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="041a7-691">已声明的局部变量是只读的，其作用域为 `Using` 语句块，必须包含初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="041a7-691">The declared local variables are read-only, scoped to the `Using` statement block and must include an initializer.</span></span> <span data-ttu-id="041a7-692">如果资源是表达式的结果，则表达式必须分类为值，并且必须是可隐式转换为 `System.IDisposable` 的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-692">If the resource is the result of an expression then the expression must be classified as a value and must be of a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="041a7-693">表达式只在语句的开头计算一次。</span><span class="sxs-lookup"><span data-stu-id="041a7-693">The expression is only evaluated once, at the beginning of the statement.</span></span>

<span data-ttu-id="041a7-694">@No__t-0 块隐式包含在 @no__t 1 语句中，其 finally 块调用资源上 `IDisposable.Dispose` 方法。</span><span class="sxs-lookup"><span data-stu-id="041a7-694">The `Using` block is implicitly contained by a `Try` statement whose finally block calls the method `IDisposable.Dispose` on the resource.</span></span> <span data-ttu-id="041a7-695">这可确保在引发异常时释放资源。</span><span class="sxs-lookup"><span data-stu-id="041a7-695">This ensures the resource is disposed even when an exception is thrown.</span></span> <span data-ttu-id="041a7-696">因此，从块外分支到 `Using` 块并将 @no__t 1 块视为单个语句以用于 `Resume` 和 `Resume Next` 是无效的。</span><span class="sxs-lookup"><span data-stu-id="041a7-696">As a result, it is invalid to branch into a `Using` block from outside of the block, and a `Using` block is treated as a single statement for the purposes of `Resume` and `Resume Next`.</span></span> <span data-ttu-id="041a7-697">如果资源 `Nothing`，则不会对 `Dispose` 进行调用。</span><span class="sxs-lookup"><span data-stu-id="041a7-697">If the resource is `Nothing`, then no call to `Dispose` is made.</span></span> <span data-ttu-id="041a7-698">因此，示例如下：</span><span class="sxs-lookup"><span data-stu-id="041a7-698">Thus, the example:</span></span>

```vb
Using f As C = New C()
    ...
End Using
```

<span data-ttu-id="041a7-699">等效于：</span><span class="sxs-lookup"><span data-stu-id="041a7-699">is equivalent to:</span></span>

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

<span data-ttu-id="041a7-700">具有局部变量声明语句的 @no__t 0 语句可以一次获取多个资源，这等同于嵌套的 @no__t 1 语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-700">A `Using` statement that has a local variable declaration statement may acquire multiple resources at a time, which is equivalent to nested `Using` statements.</span></span>  <span data-ttu-id="041a7-701">例如，一种形式的 @no__t 0 语句：</span><span class="sxs-lookup"><span data-stu-id="041a7-701">For example, a `Using` statement of the form:</span></span>

```vb
Using r1 As R = New R(), r2 As R = New R()
    r1.F()
    r2.F()
End Using
```

<span data-ttu-id="041a7-702">等效于：</span><span class="sxs-lookup"><span data-stu-id="041a7-702">is equivalent to:</span></span>

```vb
Using r1 As R = New R()
    Using r2 As R = New R()
        r1.F()
        r2.F()
    End Using
End Using
```


## <a name="await-statement"></a><span data-ttu-id="041a7-703">Await 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-703">Await Statement</span></span>

<span data-ttu-id="041a7-704">Await 语句与 await 运算符表达式（Section [Await 运算符](expressions.md#await-operator)）具有相同的语法，只允许在也允许使用 await 表达式的方法中使用，并且与 await 运算符表达式具有相同的行为。</span><span class="sxs-lookup"><span data-stu-id="041a7-704">An await statement has the same syntax as an await operator expression (Section [Await Operator](expressions.md#await-operator)), is allowed only in methods that also allow await expressions, and has the same behavior as an await operator expression.</span></span>

<span data-ttu-id="041a7-705">但是，它可以归类为值或 void。</span><span class="sxs-lookup"><span data-stu-id="041a7-705">However, it may be classified as either a value or void.</span></span> <span data-ttu-id="041a7-706">对 await 运算符表达式求值后得出的任何值都将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="041a7-706">Any value resulting from evaluation of the await operator expression is discarded.</span></span>

```antlr
AwaitStatement
    : AwaitOperatorExpression StatementTerminator
    ;
```

## <a name="yield-statement"></a><span data-ttu-id="041a7-707">Yield 语句</span><span class="sxs-lookup"><span data-stu-id="041a7-707">Yield Statement</span></span>

<span data-ttu-id="041a7-708">Yield 语句与迭代器方法相关，详见部分[迭代器方法](statements.md#iterator-methods)。</span><span class="sxs-lookup"><span data-stu-id="041a7-708">Yield statements are related to iterator methods, which are described in Section [Iterator Methods](statements.md#iterator-methods).</span></span>

```antlr
YieldStatement
    : 'Yield' Expression StatementTerminator
    ;
```

<span data-ttu-id="041a7-709">如果 @no__t 为一个保留字，则如果其出现在其中的直接封闭方法或 lambda 表达式具有 `Iterator` 修饰符，并且 `Yield` 出现在该 `Iterator` 修饰符之后，则为; 否则为。它不会保留在别处。</span><span class="sxs-lookup"><span data-stu-id="041a7-709">`Yield` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Iterator` modifier, and if the `Yield` appears after that `Iterator` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="041a7-710">它也不会在预处理器指令中保留。</span><span class="sxs-lookup"><span data-stu-id="041a7-710">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="041a7-711">Yield 语句仅允许在它是保留字的方法或 lambda 表达式的主体中使用。</span><span class="sxs-lookup"><span data-stu-id="041a7-711">The yield statement is only allowed in the body of a method or lambda expression where it is a reserved word.</span></span> <span data-ttu-id="041a7-712">在直接的封闭方法或 lambda 中，yield 语句不能出现在 `Catch` 或 @no__t 块的主体内，也不能出现在 @no__t 语句的主体内。</span><span class="sxs-lookup"><span data-stu-id="041a7-712">Within the immediately enclosing method or lambda, the yield statement may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement.</span></span>

<span data-ttu-id="041a7-713">Yield 语句采用一个表达式，该表达式必须归类为值，并且其类型可隐式转换为其封闭迭代器方法的*迭代器当前变量*（部分[迭代器方法](statements.md#iterator-methods)）的类型。</span><span class="sxs-lookup"><span data-stu-id="041a7-713">The yield statement takes a single expression which must be classified as a value and whose type is implicitly convertible to the type of the *iterator current variable* (Section [Iterator Methods](statements.md#iterator-methods)) of its enclosing iterator method.</span></span>

<span data-ttu-id="041a7-714">在迭代器对象上调用 `MoveNext` 方法时，控制流只会到达 `Yield` 语句。</span><span class="sxs-lookup"><span data-stu-id="041a7-714">Control flow only ever reaches a `Yield` statement when the `MoveNext` method is invoked on an iterator object.</span></span> <span data-ttu-id="041a7-715">（这是因为迭代器方法实例只是因为在迭代器对象上调用了 @no__t 0 或 @no__t 方法而执行其语句; 而 @no__t 2 方法只是在不允许使用 `Yield` 的块 @no__t 中执行代码。</span><span class="sxs-lookup"><span data-stu-id="041a7-715">(This is because an iterator method instance only ever executes its statements due to the `MoveNext` or `Dispose` methods being called on an iterator object; and the `Dispose` method will only ever execute code in `Finally` blocks, where `Yield` is not allowed).</span></span>

<span data-ttu-id="041a7-716">执行 `Yield` 语句时，将计算其表达式并将其存储在与该迭代器对象关联的迭代器方法实例的*迭代器当前变量*中。</span><span class="sxs-lookup"><span data-stu-id="041a7-716">When a `Yield` statement is executed, its expression is evaluated and stored in the *iterator current variable* of the iterator method instance associated with that iterator object.</span></span> <span data-ttu-id="041a7-717">值 `True` 返回到 @no__t 的调用程序，此实例的控制点将停止前进，直到迭代器对象上的 `MoveNext` 的下一次调用。</span><span class="sxs-lookup"><span data-stu-id="041a7-717">The value `True` is returned to the invoker of `MoveNext`, and the control point of this instance stops advancing until the next invocation of `MoveNext` on the iterator object.</span></span>

