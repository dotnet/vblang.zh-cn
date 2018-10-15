### <a name="overloaded-method-resolution"></a><span data-ttu-id="4761b-101">解决方法的重载的方法</span><span class="sxs-lookup"><span data-stu-id="4761b-101">Overloaded Method Resolution</span></span>

<span data-ttu-id="4761b-102">在实践中，用于确定重载决策规则旨在查找是"最靠近"提供的实际自变量的重载。</span><span class="sxs-lookup"><span data-stu-id="4761b-102">In practice, the rules for determining overload resolution are intended to find the overload that is "closest" to the actual arguments supplied.</span></span> <span data-ttu-id="4761b-103">如果其参数类型与参数类型匹配的方法，则显然最接近该方法。</span><span class="sxs-lookup"><span data-stu-id="4761b-103">If there is a method whose parameter types match the argument types, then that method is obviously the closest.</span></span> <span data-ttu-id="4761b-104">不包括，一种方法更接近于另一个如果所有的参数类型都是窄于 （或相同） 的另一种方法的参数类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-104">Barring that, one method is closer than another if all of its parameter types are narrower than (or the same as) the parameter types of the other method.</span></span> <span data-ttu-id="4761b-105">如果这两种方法的参数比其他更窄，则没有办法来确定哪种方法是更接近于自变量。</span><span class="sxs-lookup"><span data-stu-id="4761b-105">If neither method's parameters are narrower than the other, then there is no way for to determine which method is closer to the arguments.</span></span>

<span data-ttu-id="4761b-106">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-106">__Note.__</span></span> <span data-ttu-id="4761b-107">重载决策不会考虑该方法的预期返回类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-107">Overload resolution does not take into account the expected return type of the method.</span></span> 

<span data-ttu-id="4761b-108">另请注意，由于命名的参数语法、 实参和形参参数的顺序可能不能相同。</span><span class="sxs-lookup"><span data-stu-id="4761b-108">Also note that because of the named parameter syntax, the ordering of the actual and formal parameters may not be the same.</span></span>

<span data-ttu-id="4761b-109">给定方法组，使用以下步骤确定在组中的参数列表最适用的方法。</span><span class="sxs-lookup"><span data-stu-id="4761b-109">Given a method group, the most applicable method in the group for an argument list is determined using the following steps.</span></span> <span data-ttu-id="4761b-110">如果应用特定步骤之后, 没有成员保留在集中，则出现编译时错误。</span><span class="sxs-lookup"><span data-stu-id="4761b-110">If, after applying a particular step, no members remain in the set, then a compile-time error occurs.</span></span> <span data-ttu-id="4761b-111">如果只有一个成员保留在该集中，该成员是最适用的成员。</span><span class="sxs-lookup"><span data-stu-id="4761b-111">If only one member remains in the set, then that member is the most applicable member.</span></span> <span data-ttu-id="4761b-112">步骤如下：</span><span class="sxs-lookup"><span data-stu-id="4761b-112">The steps are:</span></span>

1.  <span data-ttu-id="4761b-113">首先，如果已不提供任何类型实参，则可适用于任何具有类型参数的方法类型推理。</span><span class="sxs-lookup"><span data-stu-id="4761b-113">First, if no type arguments have been supplied, apply type inference to any methods which have type parameters.</span></span> <span data-ttu-id="4761b-114">如果类型推理成功的方法，则推断的类型参数用于该特定方法。</span><span class="sxs-lookup"><span data-stu-id="4761b-114">If type inference succeeds for a method, then the inferred type arguments are used for that particular method.</span></span> <span data-ttu-id="4761b-115">如果类型推理失败的方法，然后该方法也将从一组。</span><span class="sxs-lookup"><span data-stu-id="4761b-115">If type inference fails for a method, then that method is eliminated from the set.</span></span>

2.  <span data-ttu-id="4761b-116">接下来，消除从集是不可访问或不适用的所有成员 (部分[适用性到参数列表](overload-resolution.md#applicability-to-argument-list)) 到参数列表</span><span class="sxs-lookup"><span data-stu-id="4761b-116">Next, eliminate all members from the set that are inaccessible or not applicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) to the argument list</span></span>

3.  <span data-ttu-id="4761b-117">下一步，如果一个或多个参数都`AddressOf`或 lambda 表达式，然后计算*委托一放宽级别*按如下所示的每个此类参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-117">Next, if one or more arguments are `AddressOf` or lambda expressions, then calculate the *delegate relaxation levels* for each such argument as below.</span></span> <span data-ttu-id="4761b-118">如果最差 （最低） 委派中的一放宽级别`N`更糟的是最低的委托一放宽级别中比`M`，然后消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-118">If the worst (lowest) delegate relaxation level in `N` is worse than the lowest delegate relaxation level in `M`, then eliminate `N` from the set.</span></span> <span data-ttu-id="4761b-119">委托松散级别如下所示：</span><span class="sxs-lookup"><span data-stu-id="4761b-119">The delegate relaxation levels are as follows:</span></span>

    31. <span data-ttu-id="4761b-120">*错误委托一放宽级别*--如果`AddressOf`或 lambda 不能转换为委托类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-120">*Error delegate relaxation level* -- if the `AddressOf` or lambda cannot be converted to the delegate type.</span></span>
  
    32. <span data-ttu-id="4761b-121">*收缩的返回类型或参数的委托松散*--如果参数是`AddressOf`或与声明的类型和从其返回类型转换为委托的 lambda 返回类型为收缩转换; 或如果参数为正则 lambda 和从返回表达式的任何转换为委托返回类型为收缩转换，或如果参数为异步 lambda 和返回的委托类型是`Task(Of T)`和从其返回到表达式的任何转换`T`为收缩转换; 或者，如果参数为迭代器 lambda，并且委托返回类型`IEnumerator(Of T)`或`IEnumerable(Of T)`并从其 yield 操作数的任何转换`T`为收缩转换。</span><span class="sxs-lookup"><span data-stu-id="4761b-121">*Narrowing delegate relaxation of return type or parameters* -- if the argument is `AddressOf` or a lambda with a declared type and the conversion from its return type to the delegate return type is narrowing; or if the argument is a regular lambda and the conversion from any of its return expressions to the delegate return type is narrowing, or if the argument is an async lambda and the delegate return type is `Task(Of T)` and the conversion from any of its return expressions to `T` is narrowing; or if the argument is an iterator lambda and the delegate return type `IEnumerator(Of T)` or `IEnumerable(Of T)` and the conversion from any of its yield operands to `T` is narrowing.</span></span>

    33. <span data-ttu-id="4761b-122">*扩大转换委托放宽了不带签名委派*--如果委托类型是`System.Delegate`或`System.MultiCastDelegate`或`System.Object`。</span><span class="sxs-lookup"><span data-stu-id="4761b-122">*Widening delegate relaxation to delegate without signature* -- if delegate type is `System.Delegate` or `System.MultiCastDelegate` or `System.Object`.</span></span>

    34. <span data-ttu-id="4761b-123">*删除返回或参数的委托松散*--如果参数是`AddressOf`或与声明的返回类型和委托类型的 lambda 缺少返回类型; 或者如果自变量是一个 lambda 或表达式和委托类型的详细信息返回缺少返回类型;如果参数为或`AddressOf`或没有参数，该委托类型的 lambda 的形参。</span><span class="sxs-lookup"><span data-stu-id="4761b-123">*Drop return or arguments delegate relaxation* -- if the argument is `AddressOf` or a lambda with a declared return type and the delegate type lacks a return type; or if the argument is a lambda with one or more return expressions and the delegate type lacks a return type; or if the argument is `AddressOf` or lambda with no parameters and the delegate type has parameters.</span></span>

    35. <span data-ttu-id="4761b-124">*扩大转换的返回类型的委托松散*--如果参数是`AddressOf`或声明的返回类型，lambda，并且没有扩大转换从其返回类型为的委托; 如果参数为正则 lambda 或其中扩大转换或具有至少一个扩大转换; 标识不从所有返回表达式转换为委托返回类型或者该参数是异步 lambda，委托是`Task(Of T)`或`Task`并从到的所有返回表达式转换`T` / `Object`分别是扩大转换或标识与至少一个扩大转换; 或者，如果参数为迭代器 lambda，并且委托`IEnumerator(Of T)`或`IEnumerable(Of T)`或`IEnumerator`或`IEnumerable`并从到的所有返回表达式转换`T` / `Object`扩大转换或具有至少一个扩大转换的标识。</span><span class="sxs-lookup"><span data-stu-id="4761b-124">*Widening delegate relaxation of return type* -- if the argument is `AddressOf` or a lambda with a declared return type, and there is a widening conversion from its return type to that of the delegate; or if the argument is a regular lambda where the conversion from all return expressions to the delegate return type is widening or identity with at least one widening; or if the argument is an async lambda and the delegate is `Task(Of T)` or `Task` and the conversion from all return expressions to `T`/`Object` respectively is widening or identity with at least one widening; or if the argument is an iterator lambda and the delegate is `IEnumerator(Of T)` or `IEnumerable(Of T)` or `IEnumerator` or `IEnumerable` and the conversion from all return expressions to `T`/`Object` is widening or identity with at least one widening.</span></span>

    36. <span data-ttu-id="4761b-125">*标识委托松散*--如果参数是`AddressOf`或其中没有完全匹配委托的 lambda 扩大或收缩或删除参数或返回或生成。接下来，如果在集中的某些成员执行此操作不需要收缩转换适用于任何参数，则排除执行操作的所有成员。</span><span class="sxs-lookup"><span data-stu-id="4761b-125">*Identity delegate relaxation* -- if the argument is an `AddressOf` or a lambda which matches the delegate exactly, with no widening or narrowing or dropping of parameters or returns or yields.Next, if some members of the set do not requiring narrowing conversions to be applicable to any of the arguments, then eliminate all members that do.</span></span> <span data-ttu-id="4761b-126">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-126">For example:</span></span>

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

4.  <span data-ttu-id="4761b-127">接下来，消除操作是根据收缩，如下所示。</span><span class="sxs-lookup"><span data-stu-id="4761b-127">Next, elimination is done based on narrowing as follows.</span></span> <span data-ttu-id="4761b-128">(请注意，是否 Option Strict 为 On，然后需要收缩的所有成员具有已被都评判不适用 (部分[适用性到参数列表](overload-resolution.md#applicability-to-argument-list)) 和按步骤 2 中删除。)</span><span class="sxs-lookup"><span data-stu-id="4761b-128">(Note that, if Option Strict is On, then all members that require narrowing have already been judged inapplicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) and removed by Step 2.)</span></span>

    41. <span data-ttu-id="4761b-129">如果集的一些实例成员只需要收缩的转换其中的自变量表达式类型是`Object`，则排除所有其他成员。</span><span class="sxs-lookup"><span data-stu-id="4761b-129">If some instance members of the set only require narrowing conversions where the argument expression type is `Object`, then eliminate all other members.</span></span>
    42. <span data-ttu-id="4761b-130">如果该集包含多个成员，这要求只能从收缩`Object`、 然后调用目标表达式重新分类为后期绑定方法访问 （和如果包含方法组的类型是一个接口，或如果任一给出错误相应的成员已扩展成员）。</span><span class="sxs-lookup"><span data-stu-id="4761b-130">If the set contains more than one member which requires narrowing only from `Object`, then the invocation target expression is reclassified as a late-bound method access (and an error is given if the type containing the method group is an interface, or if any of the applicable members were extension members).</span></span>
    43. <span data-ttu-id="4761b-131">如果有任何只需要从数值文字收缩的候选项，然后选择最具体的所有剩余候选人之间按以下步骤。</span><span class="sxs-lookup"><span data-stu-id="4761b-131">If there are any candidates that only require narrowing from numeric literals, then chose the most specific among all remaining candidates by the steps below.</span></span> <span data-ttu-id="4761b-132">如果入选方要求仅收缩从数字文本，然后它将选取它作为结果的重载决策;否则，它是一个错误。</span><span class="sxs-lookup"><span data-stu-id="4761b-132">If the winner requires only narrowing from numeric literals, then it is picked as the result of overload resolution; otherwise it is an error.</span></span>

    <span data-ttu-id="4761b-133">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-133">__Note.__</span></span> <span data-ttu-id="4761b-134">此规则的理由是，如果程序是松散类型化 (即，大部分或全部变量声明为`Object`)，重载决策时，可能会很难从许多转换`Object`收缩。</span><span class="sxs-lookup"><span data-stu-id="4761b-134">The justification for this rule is that if a program is loosely-typed (that is, most or all variables are declared as `Object`), overload resolution can be difficult when many conversions from `Object` are narrowing.</span></span> <span data-ttu-id="4761b-135">而不是具有重载决策失败 （需要强类型化方法调用的参数） 的许多情况下，解决方法的相应重载方法来调用延迟到运行时中。</span><span class="sxs-lookup"><span data-stu-id="4761b-135">Rather than have the overload resolution fail in many situations (requiring strong typing of the arguments to the method call), resolution the appropriate overloaded method to call is deferred until run time.</span></span> <span data-ttu-id="4761b-136">这样，也不会额外的强制转换的松散类型化调用。</span><span class="sxs-lookup"><span data-stu-id="4761b-136">This allows the loosely-typed call to succeed without additional casts.</span></span> <span data-ttu-id="4761b-137">令人遗憾的副作用，但是，是执行后期绑定调用需要强制转换为调用目标`Object`。</span><span class="sxs-lookup"><span data-stu-id="4761b-137">An unfortunate side-effect of this, however, is that performing the late-bound call requires casting the call target to `Object`.</span></span> <span data-ttu-id="4761b-138">对于结构值，这意味着，必须将值装箱到一个临时。</span><span class="sxs-lookup"><span data-stu-id="4761b-138">In the case of a structure value, this means that the value must be boxed to a temporary.</span></span> <span data-ttu-id="4761b-139">如果最终调用的方法尝试更改结构的字段，该方法返回后将丢失此更改。</span><span class="sxs-lookup"><span data-stu-id="4761b-139">If the method eventually called tries to change a field of the structure, this change will be lost once the method returns.</span></span> <span data-ttu-id="4761b-140">接口不在此特殊规则，因为后期绑定始终解析对运行时类或结构类型，这可以使它们实现的接口的成员与不同的名称的成员。</span><span class="sxs-lookup"><span data-stu-id="4761b-140">Interfaces are excluded from this special rule because late binding always resolves against the members of the runtime class or structure type, which may have different names than the members of the interfaces they implement.</span></span>

5.  <span data-ttu-id="4761b-141">接下来，如果无需收缩集中保留的任何实例方法，则排除从集中的所有扩展方法。</span><span class="sxs-lookup"><span data-stu-id="4761b-141">Next, if any instance methods remain in the set which do not require narrowing, then eliminate all extension methods from the set.</span></span> <span data-ttu-id="4761b-142">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-142">For example:</span></span>

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

    <span data-ttu-id="4761b-143">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-143">__Note.__</span></span> <span data-ttu-id="4761b-144">如果有适用的实例方法，以保证的添加导入 （这可能会将新的扩展方法引入作用域） 不会导致调用上现有的实例方法来重新绑定到扩展方法，将忽略扩展方法。</span><span class="sxs-lookup"><span data-stu-id="4761b-144">Extension methods are ignored if there are applicable instance methods to guarantee that adding an import (that might bring new extension methods into scope) will not cause a call on an existing instance method to rebind to an extension method.</span></span> <span data-ttu-id="4761b-145">考虑到的某些扩展方法 （即定义接口和/或类型参数上的） 的广泛范围，这是与绑定到扩展方法更安全的方法。</span><span class="sxs-lookup"><span data-stu-id="4761b-145">Given the broad scope of some extension methods (i.e. those defined on interfaces and/or type parameters), this is a safer approach to binding to extension methods.</span></span>

6.  <span data-ttu-id="4761b-146">接下来，如果给定的集的任何两个成员`M`和`N`，`M`更多*特定*(部分[特异性成员/类型提供的参数列表的](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) 比`N`给定的参数列表，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-146">Next, if, given any two members of the set `M` and `N`, `M` is more *specific* (Section [Specificity of members/types given an argument list](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) than `N` given the argument list, eliminate `N` from the set.</span></span> <span data-ttu-id="4761b-147">如果多个成员保留在该集中的其余成员不是同样特定的给定参数列表，会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="4761b-147">If more than one member remains in the set and the remaining members are not equally specific given the argument list, a compile-time error results.</span></span>

7.  <span data-ttu-id="4761b-148">否则，给定任意两个集的成员，`M`和`N`，将以下能够打破平局的规则，按顺序应用：</span><span class="sxs-lookup"><span data-stu-id="4761b-148">Otherwise, given any two members of the set, `M` and `N`, apply the following tie-breaking rules, in order:</span></span>

    71. <span data-ttu-id="4761b-149">如果`M`不具有 ParamArray 参数，但`N`，或如果两者，但`M`将更少的参数传递到比 ParamArray 参数`N`，然后消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-149">If `M` does not have a ParamArray parameter but `N` does, or if both do but `M` passes fewer arguments into the ParamArray parameter than `N` does, then eliminate `N` from the set.</span></span> <span data-ttu-id="4761b-150">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-150">For example:</span></span>

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

        <span data-ttu-id="4761b-151">上面的示例生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="4761b-151">The above example produces the following output:</span></span>

        ```
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        <span data-ttu-id="4761b-152">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-152">__Note.__</span></span> <span data-ttu-id="4761b-153">当类声明具有 paramarray 参数的方法时，它不少见还包含一些扩展形式作为常规方法。</span><span class="sxs-lookup"><span data-stu-id="4761b-153">When a class declares a method with a paramarray parameter, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="4761b-154">通过这样做可以避免的数组分配将调用具有 paramarray 参数的方法的扩展形式出现的实例。</span><span class="sxs-lookup"><span data-stu-id="4761b-154">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a paramarray parameter is invoked.</span></span>

    72. <span data-ttu-id="4761b-155">如果`M`比的派生程度更大类型中定义`N`，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-155">If `M` is defined in a more derived type than `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="4761b-156">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-156">For example:</span></span>

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

        <span data-ttu-id="4761b-157">此规则也适用于扩展方法定义的类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-157">This rule also applies to the types that extension methods are defined on.</span></span> <span data-ttu-id="4761b-158">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-158">For example:</span></span>

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

    73. <span data-ttu-id="4761b-159">如果`M`并`N`扩展方法和的目标类型`M`的类或结构和目标类型的`N`是一个接口，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-159">If `M` and `N` are extension methods and the target type of `M` is a class or structure and the target type of `N` is an interface, eliminate `N` from the set.</span></span> <span data-ttu-id="4761b-160">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-160">For example:</span></span>

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

    74. <span data-ttu-id="4761b-161">如果`M`并`N`扩展方法，和的目标类型`M`并`N`是相同的类型参数替换、 和的目标类型后`M`之前类型参数替换不包含类型参数化的目标类型`N`匹配，则具有较少的类型参数的目标类型比`N`，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-161">If `M` and `N` are extension methods, and the target type of `M` and `N` are identical after type parameter substitution, and the target type of `M` before type parameter substitution does not contain type parameters but the target type of `N` does, then has fewer type parameters than the target type of `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="4761b-162">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-162">For example:</span></span>

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

    75. <span data-ttu-id="4761b-163">在类型之前，参数都已替换，如果`M`是*小于泛型*(部分[泛型](overload-resolution.md#genericity)) 比`N`，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-163">Before type arguments have been substituted, if `M` is *less generic* (Section [Genericity](overload-resolution.md#genericity)) than `N`, eliminate `N` from the set.</span></span>

    76. <span data-ttu-id="4761b-164">如果`M`不是扩展方法和`N`，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-164">If `M` is not an extension method and `N` is, eliminate `N` from the set.</span></span>

    77. <span data-ttu-id="4761b-165">如果`M`并`N`扩展方法和`M`之前找到`N`(部分[扩展方法集合](overload-resolution.md#extension-method-collection))，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-165">If `M` and `N` are extension methods and `M` was found before `N` (Section [Extension Method Collection](overload-resolution.md#extension-method-collection)), eliminate `N` from the set.</span></span> <span data-ttu-id="4761b-166">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-166">For example:</span></span>

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

        <span data-ttu-id="4761b-167">如果在同一步骤中找到的扩展方法，这些扩展方法是不明确。</span><span class="sxs-lookup"><span data-stu-id="4761b-167">If the extension methods were found in the same step, then those extension methods are ambiguous.</span></span> <span data-ttu-id="4761b-168">始终会产生的标准模块包含扩展方法和调用扩展方法，就像它是常规成员名称歧义的调用。</span><span class="sxs-lookup"><span data-stu-id="4761b-168">The call may always be disambiguated using the name of the standard module containing the extension method and calling the extension method as if it was a regular member.</span></span> <span data-ttu-id="4761b-169">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-169">For example:</span></span>

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

    78. <span data-ttu-id="4761b-170">如果`M`并`N`必需的类型推断来生成类型参数和`M`不需要确定的基准任何的类型类型参数 （即每个到单个类型推断的类型参数），但`N`一样，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-170">If `M` and `N` both required type inference to produce type arguments, and `M` did not require determining the dominant type for any of its type arguments (i.e. each the type arguments inferred to a single type), but `N` did, eliminate `N` from the set.</span></span>

        <span data-ttu-id="4761b-171">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-171">__Note.__</span></span> <span data-ttu-id="4761b-172">此规则可确保已成功在早期版本中 （其中推断类型参数的多个类型会导致错误） 的重载决策，继续生成相同的结果。</span><span class="sxs-lookup"><span data-stu-id="4761b-172">This rule ensures that overload resolution that succeeded in previous versions (where inferring multiple types for a type argument would cause an error), continue to produce the same results.</span></span>

    79. <span data-ttu-id="4761b-173">如果重载解析措施来解决从委托创建表达式的目标`AddressOf`表达式，并且这两个委托并`M`是函数时`N`是一个子例程，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-173">If overload resolution is being done to resolve the target of a delegate-creation expression from an `AddressOf` expression, and both the delegate and `M` are functions while `N` is a subroutine, eliminate `N` from the set.</span></span> <span data-ttu-id="4761b-174">同样，如果这两个委托和`M`是子例程，虽然`N`是函数，消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-174">Likewise, if both the delegate and `M` are subroutines, while `N` is a function, eliminate `N` from the set.</span></span>

    710. <span data-ttu-id="4761b-175">如果`M`未使用任何可选参数的默认值来代替显式参数，但`N`这样做，则排除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-175">If `M` did not use any optional parameter defaults in place of explicit arguments, but `N` did, then eliminate `N` from the set.</span></span>

    711. <span data-ttu-id="4761b-176">在类型之前，参数都已替换，如果`M`具有*的泛型的更深入地*(部分[泛型](overload-resolution.md#genericity)) 比`N`，然后消除`N`集中。</span><span class="sxs-lookup"><span data-stu-id="4761b-176">Before type arguments have been substituted, if `M` has *greater depth of genericity* (Section [Genericity](overload-resolution.md#genericity)) than `N`, then eliminate `N` from the set.</span></span>

8. <span data-ttu-id="4761b-177">否则为调用不明确，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="4761b-177">Otherwise, the call is ambiguous and a compile-time error occurs.</span></span>

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a><span data-ttu-id="4761b-178">特异性的成员/类型提供的参数列表</span><span class="sxs-lookup"><span data-stu-id="4761b-178">Specificity of members/types given an argument list</span></span>

<span data-ttu-id="4761b-179">成员`M`被视为*同样特定*作为`N`，给定一个参数列表`A`，如果它们的签名相同或如果每个参数键入`M`等同于相应中的参数类型`N`。</span><span class="sxs-lookup"><span data-stu-id="4761b-179">A member `M` is considered *equally specific* as `N`, given an argument-list `A`, if their signatures are the same or if each parameter type in `M` is the same as the corresponding parameter type in `N`.</span></span>

<span data-ttu-id="4761b-180">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-180">__Note.__</span></span> <span data-ttu-id="4761b-181">两个成员可能具有相同的签名，因为扩展方法的方法组中。</span><span class="sxs-lookup"><span data-stu-id="4761b-181">Two members can end up in a method group with the same signature due to extension methods.</span></span> <span data-ttu-id="4761b-182">两个成员可以也是同样特定但不是具有类型参数或参数组扩展由于相同的签名。</span><span class="sxs-lookup"><span data-stu-id="4761b-182">Two members can also be equally specific but not have the same signature due to type parameters or paramarray expansion.</span></span>

<span data-ttu-id="4761b-183">成员`M`被视为*更具体*比`N`如果它们的签名不同，并且至少一个参数中键入`M`比中的参数类型更具体`N`，且未中的参数类型`N`中的参数类型比更具体`M`。</span><span class="sxs-lookup"><span data-stu-id="4761b-183">A member `M` is considered *more specific* than `N` if their signatures are different and at least one parameter type in `M` is more specific than a parameter type in `N`, and no parameter type in `N` is more specific than a parameter type in `M`.</span></span> <span data-ttu-id="4761b-184">给定一对参数`Mj`和`Nj`相匹配参数`Aj`的类型`Mj`被视为*更具体*比的类型`Nj`如果以下项之一条件为 true:</span><span class="sxs-lookup"><span data-stu-id="4761b-184">Given a pair of parameters `Mj` and `Nj` that matches an argument `Aj`, the type of `Mj` is considered *more specific* than the type of `Nj` if one of the following conditions is true:</span></span>

* <span data-ttu-id="4761b-185">存在从类型的扩大转换`Mj`为类型`Nj`。</span><span class="sxs-lookup"><span data-stu-id="4761b-185">There exists a widening conversion from the type of `Mj` to the type `Nj`.</span></span> <span data-ttu-id="4761b-186">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-186">(__Note.__</span></span> <span data-ttu-id="4761b-187">因为参数类型进行比较的而不考虑实际自变量在这种情况下，扩大转换从常量表达式为数值类型值适用于不被视为在这种情况下。）</span><span class="sxs-lookup"><span data-stu-id="4761b-187">Because parameters types are being compared without regard to the actual argument in this case, the widening conversion from constant expressions to a numeric type the value fits into is not considered in this case.)</span></span>

* <span data-ttu-id="4761b-188">`Aj` 为字面值`0`，`Mj`为数值类型和`Nj`是一个枚举的类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-188">`Aj` is the literal `0`, `Mj` is a numeric type and `Nj` is an enumerated type.</span></span> <span data-ttu-id="4761b-189">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-189">(__Note.__</span></span> <span data-ttu-id="4761b-190">此规则是必需的因为文本`0`加宽到任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-190">This rule is necessary because the literal `0` widens to any enumerated type.</span></span> <span data-ttu-id="4761b-191">一个枚举的类型加宽到其基础类型，因为这意味着，在重载决策`0`将默认情况下，首选枚举的类型而不是数值类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-191">Since an enumerated type widens to its underlying type, this means that overload resolution on `0` will, by default, prefer enumerated types over numeric types.</span></span> <span data-ttu-id="4761b-192">我们收到了大量反馈，此行为是有悖常理）。</span><span class="sxs-lookup"><span data-stu-id="4761b-192">We received a lot of feedback that this behavior was counterintuitive.)</span></span>

* <span data-ttu-id="4761b-193">`Mj` 和`Nj`均为数值类型，并`Mj`来自早于`Nj`列表中`Byte`， `SByte`， `Short`， `UShort`， `Integer`， `UInteger`， `Long`， `ULong`, `Decimal`, `Single`, `Double`.</span><span class="sxs-lookup"><span data-stu-id="4761b-193">`Mj` and `Nj` are both numeric types, and `Mj` comes earlier than `Nj` in the list `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`.</span></span> <span data-ttu-id="4761b-194">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-194">(__Note.__</span></span> <span data-ttu-id="4761b-195">介绍了数字类型的规则是很有用，因为特定大小的有符号和无符号数值类型只能有收缩它们之间的转换。</span><span class="sxs-lookup"><span data-stu-id="4761b-195">The rule about the numeric types is useful because the signed and unsigned numeric types of a particular size only have narrowing conversions between them.</span></span> <span data-ttu-id="4761b-196">上述规则将为支持多个"自然的"数值类型的两个类型之间平分处中断。</span><span class="sxs-lookup"><span data-stu-id="4761b-196">The above rule breaks the tie between the two types in favor of the more "natural" numeric type.</span></span> <span data-ttu-id="4761b-197">这是非常重要时执行操作的类型的重载决策的加宽到这两个符号和无符号数值类型的特定大小，例如，适用于两者的数字参数。）</span><span class="sxs-lookup"><span data-stu-id="4761b-197">This is particularly important when doing overload resolution on a type that widens to both the signed and unsigned numeric types of a particular size, for example, a numeric literal that fits into both.)</span></span>

* <span data-ttu-id="4761b-198">`Mj` 和`Nj`函数类型和返回类型是委托`Mj`的返回类型比更具体`Nj`如果`Aj`归类为 lambda 方法并`Mj`或`Nj`是`System.Linq.Expressions.Expression(Of T)`，然后（假设它委托类型） 的类型的类型参数替换为要比较的类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-198">`Mj` and `Nj` are delegate function types and the return type of `Mj` is more specific than the return type of `Nj` If `Aj` is classified as a lambda method, and `Mj` or `Nj` is `System.Linq.Expressions.Expression(Of T)`, then the type argument of the type (assuming it is a delegate type) is substituted for the type being compared.</span></span>

* <span data-ttu-id="4761b-199">`Mj` 类型相同`Aj`，和`Nj`不是。</span><span class="sxs-lookup"><span data-stu-id="4761b-199">`Mj` is identical to the type of `Aj`, and `Nj` is not.</span></span> <span data-ttu-id="4761b-200">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-200">(__Note.__</span></span> <span data-ttu-id="4761b-201">有趣的是要注意前面的规则略有不同于 C#、 C# 中要求的委托函数类型进行比较的返回类型，而 Visual Basic 不前具有相同的参数列表。）</span><span class="sxs-lookup"><span data-stu-id="4761b-201">It is interesting to note that the previous rule differs slightly from C#, in that C# requires that the delegate function types have identical parameter lists before comparing return types, while Visual Basic does not.)</span></span>

#### <a name="genericity"></a><span data-ttu-id="4761b-202">泛型</span><span class="sxs-lookup"><span data-stu-id="4761b-202">Genericity</span></span>

<span data-ttu-id="4761b-203">成员`M`被确定为*小于泛型*比成员`N`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4761b-203">A member `M` is determined to be *less generic* than a member `N` as follows:</span></span>

1. <span data-ttu-id="4761b-204">如果匹配的参数的每个对`Mj`和`Nj`，`Mj`更短或平均比泛型`Nj`方法，以及至少一个类型参数对于`Mj`是低泛型类型的方面该方法的参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-204">If, for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the method, and at least one `Mj` is less generic with respect to type parameters on the method.</span></span>
2. <span data-ttu-id="4761b-205">否则为如果匹配的参数的每个对`Mj`并`Nj`，`Mj`更短或平均比泛型`Nj`与类型参数类型，以及至少一个有关`Mj`是低泛型类型的方面参数的类型，然后`M`是比低泛型`N`。</span><span class="sxs-lookup"><span data-stu-id="4761b-205">Otherwise, if for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the type, and at least one `Mj` is less generic with respect to type parameters on the type, then `M` is less generic than `N`.</span></span>

<span data-ttu-id="4761b-206">参数`M`被视为同样通用的参数`N`如果其类型`Mt`和`Nt`都引用类型参数或都不引用类型参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-206">A parameter `M` is considered to be equally generic to a parameter `N` if their types `Mt` and `Nt` both refer to type parameters or both don't refer to type parameters.</span></span> <span data-ttu-id="4761b-207">`M` 被视为可比低泛型`N`如果`Mt`不引用类型参数和`Nt`does。</span><span class="sxs-lookup"><span data-stu-id="4761b-207">`M` is considered to be less generic than `N` if `Mt` does not refer to a type parameter and `Nt` does.</span></span>

<span data-ttu-id="4761b-208">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-208">For example:</span></span>

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

<span data-ttu-id="4761b-209">已修复科期间的扩展方法类型参数被视为类型参数的类型，不是类型参数的方法。</span><span class="sxs-lookup"><span data-stu-id="4761b-209">Extension method type parameters that were fixed during currying are considered type parameters on the type, not type parameters on the method.</span></span> <span data-ttu-id="4761b-210">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-210">For example:</span></span>

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

#### <a name="depth-of-genericity"></a><span data-ttu-id="4761b-211">泛型的深度</span><span class="sxs-lookup"><span data-stu-id="4761b-211">Depth of genericity</span></span>

<span data-ttu-id="4761b-212">成员`M`被确定有*的泛型的更深入地*比成员`N`如果匹配的参数的每个对`Mj`和`Nj`，`Mj`具有更高版本或等于*的泛型深度*比`Nj`，且至少一个`Mj`是更高版本的深度为泛型。</span><span class="sxs-lookup"><span data-stu-id="4761b-212">A member `M` is determined to have *greater depth of genericity* than a member `N` if, for each pair of matching parameters  `Mj` and `Nj`, `Mj` has greater or equal *depth of genericity* than `Nj`, and at least one `Mj` is has greater depth of genericity.</span></span> <span data-ttu-id="4761b-213">泛型的深度定义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4761b-213">Depth of genericity is defined as follows:</span></span>

* <span data-ttu-id="4761b-214">类型参数之外的任何内容有更深入的泛型类型参数;</span><span class="sxs-lookup"><span data-stu-id="4761b-214">Anything other than a type parameter has greater depth of genericity than a type parameter;</span></span>

* <span data-ttu-id="4761b-215">以递归方式，如果至少一个类型参数具有更深入地泛型的并且没有类型参数具有更少深度比相应的类型构造的类型具有比另一个构造类型 （具有相同的类型参数） 更深入地泛型中的其他参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-215">Recursively, a constructed type has greater depth of genericity than another constructed type (with the same number of type arguments) if at least one type argument has greater depth of genericity and no type argument has less depth than the corresponding type argument in the other.</span></span>

* <span data-ttu-id="4761b-216">数组类型具有比 （具有相同维数的） 的另一个数组类型的泛型的更深入地，如果第一个元素类型具有比第二个元素类型的泛型的更深入地。</span><span class="sxs-lookup"><span data-stu-id="4761b-216">An array type has greater depth of genericity than another array type (with the same number of dimensions) if the element type of the first has greater depth of genericity than the element type of the second.</span></span>

<span data-ttu-id="4761b-217">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-217">For example:</span></span>

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

### <a name="applicability-to-argument-list"></a><span data-ttu-id="4761b-218">适用于参数列表</span><span class="sxs-lookup"><span data-stu-id="4761b-218">Applicability To Argument List</span></span>

<span data-ttu-id="4761b-219">一种方法是*适用*到一组类型参数，位置自变量和命名的参数，如果可以调用该方法使用自变量列表。</span><span class="sxs-lookup"><span data-stu-id="4761b-219">A method is *applicable* to a set of type arguments, positional arguments, and named arguments if the method can be invoked using the argument lists.</span></span> <span data-ttu-id="4761b-220">与参数列表匹配的自变量列表，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4761b-220">The argument lists are matched against the parameter lists as follows:</span></span>

1. <span data-ttu-id="4761b-221">首先，匹配每个位置参数在方法参数的列表顺序。</span><span class="sxs-lookup"><span data-stu-id="4761b-221">First, match each positional argument in order to the list of method parameters.</span></span> <span data-ttu-id="4761b-222">如果有多个位置参数，而不是参数，最后一个参数不是一个参数数组，该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="4761b-222">If there are more positional arguments than parameters and the last parameter is not a paramarray, the method is not applicable.</span></span> <span data-ttu-id="4761b-223">否则，要匹配的位置参数的数目的参数组元素类型的参数被展开 paramarray 参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-223">Otherwise, the paramarray parameter is expanded with parameters of the paramarray element type to match the number of positional arguments.</span></span> <span data-ttu-id="4761b-224">如果省略位置自变量，会转到一个参数数组，该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="4761b-224">If a positional argument is omitted that would go into a paramarray, the method is not applicable.</span></span>
2. <span data-ttu-id="4761b-225">接下来，与具有给定名称的参数为每个命名的参数相匹配。</span><span class="sxs-lookup"><span data-stu-id="4761b-225">Next, match each named argument to a parameter with the given name.</span></span> <span data-ttu-id="4761b-226">如果其中一个命名参数无法匹配、 匹配 paramarray 参数，或与已与另一个位置或命名参数匹配的参数，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="4761b-226">If one of the named arguments fails to match, matches a paramarray parameter, or matches an argument already matched with another positional or named argument, the method is not applicable.</span></span>
3. <span data-ttu-id="4761b-227">接下来，如果已指定类型参数，它们被匹配的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="4761b-227">Next, if type arguments have been specified, they are matched against the type parameter list .</span></span> <span data-ttu-id="4761b-228">如果两个列表不具有相同的元素数，方法不适用，除非类型参数列表为空。</span><span class="sxs-lookup"><span data-stu-id="4761b-228">If the two lists do not have the same number of elements, the method is not applicable, unless the type argument list is empty.</span></span> <span data-ttu-id="4761b-229">如果类型参数列表为空，使用类型推理来尝试推断类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="4761b-229">If the type argument list is empty, type inference is used to try and infer the type argument list.</span></span> <span data-ttu-id="4761b-230">如果类型推断失败，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="4761b-230">If type inferencing fails, the method is not applicable.</span></span> <span data-ttu-id="4761b-231">否则，类型参数填充到签名中的类型参数位置。如果不匹配的参数不是可选的则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="4761b-231">Otherwise, the type arguments are filled in the place of the type parameters in the signature.If parameters that have not been matched are not optional, the method is not applicable.</span></span>
4. <span data-ttu-id="4761b-232">如果参数表达式不是隐式转换的参数类型为它们匹配，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="4761b-232">If the argument expressions are not implicitly convertible to the types of the parameters they match, then the method is not applicable.</span></span>
5. <span data-ttu-id="4761b-233">如果参数是引用传递，并且不是从参数类型的隐式转换为自变量的类型，然后该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="4761b-233">If a parameter is ByRef, and there is not an implicit conversion from the type of the parameter to the type of the argument, then the method is not applicable.</span></span>
6. <span data-ttu-id="4761b-234">如果类型参数违反 （包括步骤 3 中的推断的类型参数） 的方法的约束，该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="4761b-234">If type arguments violate the method's constraints (including the inferred type arguments from Step 3), the method is not applicable.</span></span> <span data-ttu-id="4761b-235">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-235">For example:</span></span>

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

<span data-ttu-id="4761b-236">如果单个自变量表达式匹配 paramarray 参数并且参数表达式的类型转换为 paramarray 参数的类型和参数数组元素类型，方法是适用于这两个其扩展和未扩展窗体，有两个例外。</span><span class="sxs-lookup"><span data-stu-id="4761b-236">If a single argument expression matches a paramarray parameter and the type of the argument expression is convertible to both the type of the paramarray parameter and the paramarray element type, the method is applicable in both its expanded and unexpanded forms, with two exceptions.</span></span> <span data-ttu-id="4761b-237">如果从自变量表达式的类型到 paramarray 类型的转换收缩转换，然后该方法才适用以展开形式。</span><span class="sxs-lookup"><span data-stu-id="4761b-237">If the conversion from the type of the argument expression to the paramarray type is narrowing, then the method is only applicable in its expanded form.</span></span> <span data-ttu-id="4761b-238">如果参数表达式为字面值`Nothing`，然后方法只是适用于其未扩展的窗体。</span><span class="sxs-lookup"><span data-stu-id="4761b-238">If the argument expression is the literal `Nothing`, then the method is only applicable in its unexpanded form.</span></span> <span data-ttu-id="4761b-239">例如：</span><span class="sxs-lookup"><span data-stu-id="4761b-239">For example:</span></span>

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

<span data-ttu-id="4761b-240">上面的示例生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="4761b-240">The above example produces the following output:</span></span>

```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="4761b-241">中的第一个和最后一个调用`F`的范式`F`是适用的因为存在从实参类型到形参类型扩大转换 (两个均为类型`Object()`)，并作为常规值传递自变量参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-241">In the first and last invocations of `F`, the normal form of `F` is applicable because a widening conversion exists from the argument type to the parameter type (both are of type `Object()`), and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="4761b-242">在第二个和第三个调用的范式`F`因为没有扩大转换存在从实参类型到形参类型不适用 (从转换`Object`到`Object()`收缩)。</span><span class="sxs-lookup"><span data-stu-id="4761b-242">In the second and third invocations, the normal form of `F` is not applicable because no widening conversion exists from the argument type to the parameter type (conversions from `Object` to `Object()` are narrowing).</span></span> <span data-ttu-id="4761b-243">但是的展开的形式`F`已适用，以及一个元素`Object()`创建的调用。</span><span class="sxs-lookup"><span data-stu-id="4761b-243">However, the expanded form of `F` is applicable, and a one-element `Object()` is created by the invocation.</span></span> <span data-ttu-id="4761b-244">使用给定的参数值初始化数组的单个元素 (其本身是对的引用`Object()`)。</span><span class="sxs-lookup"><span data-stu-id="4761b-244">The single element of the array is initialized with the given argument value (which itself is a reference to an `Object()`).</span></span>

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a><span data-ttu-id="4761b-245">传递自变量，并选择可选参数的自变量</span><span class="sxs-lookup"><span data-stu-id="4761b-245">Passing Arguments, and Picking Arguments for Optional Parameters</span></span>

<span data-ttu-id="4761b-246">如果参数为值参数，则匹配的参数表达式必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="4761b-246">If a parameter is a value parameter, the matching argument expression must be classified as a value.</span></span> <span data-ttu-id="4761b-247">值转换为参数的类型，并在运行时作为参数传入。</span><span class="sxs-lookup"><span data-stu-id="4761b-247">The value is converted to the type of the parameter and passed in as the parameter at run time.</span></span> <span data-ttu-id="4761b-248">如果参数为引用参数和匹配的自变量表达式分类为其类型为的参数相同的变量，然后对该变量的引用是在作为参数传递在运行时。</span><span class="sxs-lookup"><span data-stu-id="4761b-248">If the parameter is a reference parameter and the matching argument expression is classified as a variable whose type is the same as the parameter, then a reference to the variable is passed in as the parameter at run time.</span></span>

<span data-ttu-id="4761b-249">否则，如果匹配的参数表达式分类为变量、 值或属性访问，被分配临时变量的参数的类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-249">Otherwise, if the matching argument expression is classified as a variable, value, or property access, then a temporary variable of the type of the parameter is allocated.</span></span> <span data-ttu-id="4761b-250">之前在运行时方法调用，自变量表达式将重新分类为一个值，转换为类型参数，并分配给临时变量。</span><span class="sxs-lookup"><span data-stu-id="4761b-250">Before the method invocation at run time, the argument expression is reclassified as a value, converted to the type of the parameter, and assigned to the temporary variable.</span></span> <span data-ttu-id="4761b-251">然后对临时变量的引用中传递作为参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-251">Then a reference to the temporary variable is passed in as the parameter.</span></span> <span data-ttu-id="4761b-252">如果自变量表达式分类为变量或属性访问评估方法调用后，临时变量分配给变量的表达式或属性访问表达式。</span><span class="sxs-lookup"><span data-stu-id="4761b-252">After the method invocation is evaluated, if the argument expression is classified as a variable or property access, the temporary variable is assigned to the variable expression or the property access expression.</span></span> <span data-ttu-id="4761b-253">如果不具有属性访问表达式`Set`访问器，然后分配不会执行。</span><span class="sxs-lookup"><span data-stu-id="4761b-253">If the property access expression has no `Set` accessor, then the assignment is not performed.</span></span>

<span data-ttu-id="4761b-254">对于参数尚未提供的可选参数，编译器会选取参数如下所述。</span><span class="sxs-lookup"><span data-stu-id="4761b-254">For optional parameters where an argument has not been provided, the compiler picks arguments as described below.</span></span> <span data-ttu-id="4761b-255">在所有情况下它测试参数类型针对后泛型类型替换。</span><span class="sxs-lookup"><span data-stu-id="4761b-255">In all cases it tests against the parameter type after generic type substitution.</span></span>

* <span data-ttu-id="4761b-256">如果可选参数具有的属性`System.Runtime.CompilerServices.CallerLineNumber`，则调用不从源代码中的位置和表示该位置的行号的数值标识符具有内部函数转换的参数类型，然后使用数字文本。</span><span class="sxs-lookup"><span data-stu-id="4761b-256">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerLineNumber`, and the invocation is from a location in source code, and a numeric literal representing that location's line number has an intrinsic conversion to the parameter type, then the numeric literal is used.</span></span> <span data-ttu-id="4761b-257">如果调用跨多个行，选择要使用哪些行是依赖于实现的。</span><span class="sxs-lookup"><span data-stu-id="4761b-257">If the invocation spans multiple lines, then the choice of which line to use is implementation-dependent.</span></span>

* <span data-ttu-id="4761b-258">如果可选参数具有的属性`System.Runtime.CompilerServices.CallerFilePath`，则调用不从源代码中的位置和表示该位置的文件路径的字符串文字具有内部函数转换的参数类型，然后使用字符串文本。</span><span class="sxs-lookup"><span data-stu-id="4761b-258">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerFilePath`, and the invocation is from a location in source code, and a string literal representing that location's file path has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="4761b-259">文件路径的格式是依赖于实现的。</span><span class="sxs-lookup"><span data-stu-id="4761b-259">The format of the file path is implementation-dependent.</span></span>

* <span data-ttu-id="4761b-260">如果可选参数具有的属性`System.Runtime.CompilerServices.CallerMemberName`，并调用体内的类型成员或在属性应用于该类型成员的任何部分中，且是表示该成员名称的字符串文字到参数的内部函数转换类型，则使用字符串文本。</span><span class="sxs-lookup"><span data-stu-id="4761b-260">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerMemberName`, and the invocation is within the body of a type member or in an attribute applied to any part of that type member, and a string literal representing that member name has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="4761b-261">对于属性访问器或自定义事件处理程序的一部分的调用，然后使用的成员名称是属性或事件本身。</span><span class="sxs-lookup"><span data-stu-id="4761b-261">For invocations that are part of property accessors or custom event handlers, then the member name used is that of the property or event itself.</span></span> <span data-ttu-id="4761b-262">为属于运算符或构造函数的调用，则特定于实现的名称使用。</span><span class="sxs-lookup"><span data-stu-id="4761b-262">For invocations that are part of an operator or constructor, then an implementation-specific name is used.</span></span>

<span data-ttu-id="4761b-263">如果没有更高版本的应用，则使用可选参数的默认值 (或`Nothing`如果不提供任何默认值)。</span><span class="sxs-lookup"><span data-stu-id="4761b-263">If none of the above apply, then the optional parameter's default value is used (or `Nothing` if no default value is supplied).</span></span> <span data-ttu-id="4761b-264">如果超过其中一个更高版本的应用，然后选择要使用的是依赖于实现的。</span><span class="sxs-lookup"><span data-stu-id="4761b-264">And if more than one of the above apply, then the choice of which to use is implementation-dependent.</span></span>

<span data-ttu-id="4761b-265">`CallerLineNumber`和`CallerFilePath`属性可用于日志记录。</span><span class="sxs-lookup"><span data-stu-id="4761b-265">The `CallerLineNumber` and `CallerFilePath` attributes are useful for logging.</span></span> <span data-ttu-id="4761b-266">`CallerMemberName`可用于实现`INotifyPropertyChanged`。</span><span class="sxs-lookup"><span data-stu-id="4761b-266">The `CallerMemberName` is useful for implementing `INotifyPropertyChanged`.</span></span> <span data-ttu-id="4761b-267">下面是示例。</span><span class="sxs-lookup"><span data-stu-id="4761b-267">Here are examples.</span></span>

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

<span data-ttu-id="4761b-268">除了以上的可选参数，Microsoft Visual Basic 还可识别某些其他可选参数如果它们从元数据 （即从 DLL 引用） 导入：</span><span class="sxs-lookup"><span data-stu-id="4761b-268">In addition to the optional parameters above, Microsoft Visual Basic also recognizes some additional optional parameters if they are imported from metadata (i.e. from a DLL reference):</span></span>

* <span data-ttu-id="4761b-269">从元数据导入，在 Visual Basic 还将参数`<Optional>`，指示该参数是可选： 可以导入声明它具有一个可选参数，但没有默认值，即使它不能是这种方式使用表示`Optional`关键字。</span><span class="sxs-lookup"><span data-stu-id="4761b-269">Upon importing from metadata, Visual Basic also treats the parameter `<Optional>` as indicative that the parameter is optional: in this way it is possible to import a declaration which has an optional parameter but no default value, even though this can't be expressed using the `Optional` keyword.</span></span>

* <span data-ttu-id="4761b-270">如果可选参数具有的属性`Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`，并且数字文本 1 或 0 具有转换为参数类型，则编译器将作为参数使用的文本 1 if`Option Compare Text`中的效果，或者文本 0 如果`Optional Compare Binary`生效。</span><span class="sxs-lookup"><span data-stu-id="4761b-270">If the optional parameter has the attribute `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`, and the numeric literal 1 or 0 has a conversion to the parameter type, then the compiler uses as argument either the literal 1 if `Option Compare Text` is in effect, or the literal 0 if `Optional Compare Binary` is in effect.</span></span>

* <span data-ttu-id="4761b-271">如果可选参数具有的属性`System.Runtime.CompilerServices.IDispatchConstantAttribute`，并具有类型`Object`，和未指定默认值，则编译器将使用该参数`New System.Runtime.InteropServices.DispatchWrapper(Nothing)`。</span><span class="sxs-lookup"><span data-stu-id="4761b-271">If the optional parameter has the attribute `System.Runtime.CompilerServices.IDispatchConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span></span>

* <span data-ttu-id="4761b-272">如果可选参数具有的属性`System.Runtime.CompilerServices.IUnknownConstantAttribute`，并具有类型`Object`，和未指定默认值，则编译器将使用该参数`New System.Runtime.InteropServices.UnknownWrapper(Nothing)`。</span><span class="sxs-lookup"><span data-stu-id="4761b-272">If the optional parameter has the attribute `System.Runtime.CompilerServices.IUnknownConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span></span>

* <span data-ttu-id="4761b-273">如果可选参数具有类型`Object`，和未指定默认值，则编译器将使用该参数`System.Reflection.Missing.Value`。</span><span class="sxs-lookup"><span data-stu-id="4761b-273">If the optional parameter has type `Object`, and it does not specify a default value, then the compiler uses the argument `System.Reflection.Missing.Value`.</span></span>

### <a name="conditional-methods"></a><span data-ttu-id="4761b-274">条件方法</span><span class="sxs-lookup"><span data-stu-id="4761b-274">Conditional Methods</span></span>

<span data-ttu-id="4761b-275">如果调用表达式所引用的目标方法是不是接口成员的子例程，该方法具有一个或多个`System.Diagnostics.ConditionalAttribute`属性，表达式的计算取决于在定义的条件编译常量源文件中该点。</span><span class="sxs-lookup"><span data-stu-id="4761b-275">If the target method to which an invocation expression refers is a subroutine that is not a member of an interface and if the method has one or more `System.Diagnostics.ConditionalAttribute` attributes, evaluation of the expression depends on the conditional compilation constants defined at that point in the source file.</span></span> <span data-ttu-id="4761b-276">该属性的每个实例指定了条件编译常量的名称的字符串。</span><span class="sxs-lookup"><span data-stu-id="4761b-276">Each instance of the attribute specifies a string, which names a conditional compilation constant.</span></span> <span data-ttu-id="4761b-277">每个条件编译常量计算，就好像条件编译语句的一部分。</span><span class="sxs-lookup"><span data-stu-id="4761b-277">Each conditional compilation constant is evaluated as if it were part of a conditional compilation statement.</span></span> <span data-ttu-id="4761b-278">如果该常量的计算结果为`True`，在运行时通常计算表达式。</span><span class="sxs-lookup"><span data-stu-id="4761b-278">If the constant evaluates to `True`, the expression is evaluated normally at run time.</span></span> <span data-ttu-id="4761b-279">如果该常量的计算结果为`False`，根本不计算表达式。</span><span class="sxs-lookup"><span data-stu-id="4761b-279">If the constant evaluates to `False`, the expression is not evaluated at all.</span></span>

<span data-ttu-id="4761b-280">当查找该属性，将检查派生程度最高的可重写方法声明。</span><span class="sxs-lookup"><span data-stu-id="4761b-280">When looking for the attribute, the most derived declaration of an overridable method is checked.</span></span>

<span data-ttu-id="4761b-281">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="4761b-281">__Note.__</span></span> <span data-ttu-id="4761b-282">特性不是函数或接口方法上无效，如果这两种方法上指定，则忽略。</span><span class="sxs-lookup"><span data-stu-id="4761b-282">The attribute is not valid on functions or interface methods and is ignored if specified on either kind of method.</span></span> <span data-ttu-id="4761b-283">因此，条件方法只会在调用语句中。</span><span class="sxs-lookup"><span data-stu-id="4761b-283">Thus, conditional methods will only appear in invocation statements.</span></span>

### <a name="type-argument-inference"></a><span data-ttu-id="4761b-284">类型自变量推理</span><span class="sxs-lookup"><span data-stu-id="4761b-284">Type Argument Inference</span></span>

<span data-ttu-id="4761b-285">具有类型参数的方法调用而无需指定类型参数时*类型参数推理*用于尝试并推断类型参数的调用。</span><span class="sxs-lookup"><span data-stu-id="4761b-285">When a method with type parameters is called without specifying type arguments, *type argument inference* is used to try and infer type arguments for the call.</span></span> <span data-ttu-id="4761b-286">这样，若要用来调用具有类型参数的方法时可以不费力地推断出类型参数的语法更自然。</span><span class="sxs-lookup"><span data-stu-id="4761b-286">This allows a more natural syntax to be used for calling a method with type parameters when the type arguments can be trivially inferred.</span></span> <span data-ttu-id="4761b-287">例如，给定以下方法声明：</span><span class="sxs-lookup"><span data-stu-id="4761b-287">For example, given the following method declaration:</span></span>

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

<span data-ttu-id="4761b-288">可以调用`Choose`而无需显式指定类型参数的方法：</span><span class="sxs-lookup"><span data-stu-id="4761b-288">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

<span data-ttu-id="4761b-289">通过类型参数推断的类型实参`Integer`和`String`将根据对方法的参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-289">Through type argument inference, the type arguments `Integer` and `String` are determined from the arguments to the method.</span></span>

<span data-ttu-id="4761b-290">类型实参推理发生*之前*由于重新分类的这些两种类型的表达式可能需要的类型，在 lambda 方法或方法参数列表中的指针上执行的表达式重新分类为已知的参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-290">Type argument inference occurs *before* expression reclassification is performed on lambda methods or method pointers in the argument list, since reclassification of those two kinds of expressions may require the type of the parameter to be known.</span></span>  <span data-ttu-id="4761b-291">给定一组参数`A1,...,An`，一组匹配参数`P1,...,Pn`和一组方法类型参数`T1,...,Tn`，自变量和方法类型参数之间的依赖关系首先收集，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4761b-291">Given a set of arguments `A1,...,An`, a set of matching parameters `P1,...,Pn` and a set of method type parameters `T1,...,Tn`, the dependencies between the arguments and method type parameters are first collected as follows:</span></span>

* <span data-ttu-id="4761b-292">如果`An`是`Nothing`文本，则会生成没有依赖关系。</span><span class="sxs-lookup"><span data-stu-id="4761b-292">If `An` is the `Nothing` literal, no dependencies are generated.</span></span>

* <span data-ttu-id="4761b-293">如果`An`是的类型和 lambda 方法`Pn`是构造的委托类型或`System.Linq.Expressions.Expression(Of T)`，其中`T`是构造的委托类型，</span><span class="sxs-lookup"><span data-stu-id="4761b-293">If `An` is a lambda method and the type of `Pn` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type,</span></span>

* <span data-ttu-id="4761b-294">如果将从相应的参数的类型推断出 lambda 方法参数的类型`Pn`，和参数的类型取决于方法类型参数`Tn`，然后`An`依赖`Tn`。</span><span class="sxs-lookup"><span data-stu-id="4761b-294">If the type of a lambda method parameter will be inferred from the type of the corresponding parameter `Pn`, and the type of the parameter depends on a method type parameter `Tn`, then `An` has a dependency on `Tn`.</span></span>

* <span data-ttu-id="4761b-295">如果指定的 lambda 方法参数的类型和相应的参数的类型`Pn`取决于方法类型参数`Tn`，然后`Tn`依赖`An`。</span><span class="sxs-lookup"><span data-stu-id="4761b-295">If the type of a lambda method parameter is specified and the type of the corresponding parameter `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="4761b-296">如果的返回类型的`Pn`取决于方法类型参数`Tn`，然后`Tn`依赖`An`。</span><span class="sxs-lookup"><span data-stu-id="4761b-296">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="4761b-297">如果`An`是方法指针的类型和`Pn`是构造的委托类型，</span><span class="sxs-lookup"><span data-stu-id="4761b-297">If `An` is a method pointer and the type of `Pn` is a constructed delegate type,</span></span>

* <span data-ttu-id="4761b-298">如果的返回类型的`Pn`取决于方法类型参数`Tn`，然后`Tn`依赖`An`。</span><span class="sxs-lookup"><span data-stu-id="4761b-298">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="4761b-299">如果`Pn`是一个构造的类型的类型和`Pn`取决于方法类型参数`Tn`，然后`Tn`依赖`An`。</span><span class="sxs-lookup"><span data-stu-id="4761b-299">If `Pn` is a constructed type and the type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="4761b-300">否则，生成无依赖关系。</span><span class="sxs-lookup"><span data-stu-id="4761b-300">Otherwise, no dependency is generated.</span></span>

<span data-ttu-id="4761b-301">收集依赖项之后, 将消除没有依赖关系的任何自变量。</span><span class="sxs-lookup"><span data-stu-id="4761b-301">After collecting dependencies, any arguments that have no dependencies are eliminated.</span></span> <span data-ttu-id="4761b-302">如果任何方法类型参数 （即方法类型参数不依赖于自变量） 没有传出依赖项，则类型推理失败。</span><span class="sxs-lookup"><span data-stu-id="4761b-302">If any method type parameters have no outgoing dependencies (i.e. the method type parameter does not depend on an argument), then type inference fails.</span></span> <span data-ttu-id="4761b-303">否则，其余的参数和方法类型参数被分组到紧密相连的组件。</span><span class="sxs-lookup"><span data-stu-id="4761b-303">Otherwise, the remaining arguments and method type parameters are grouped into strongly connected components.</span></span> <span data-ttu-id="4761b-304">紧密相连的组件是一组参数以及方法类型参数，该组件中的任何元素是可通过在其他元素上的依赖项。</span><span class="sxs-lookup"><span data-stu-id="4761b-304">A strongly connected component is a set of arguments and method type parameters, where any element in the component is reachable via dependencies on other elements.</span></span>

<span data-ttu-id="4761b-305">然后，紧密相连的组件为拓扑结构上排序，按拓扑顺序处理：</span><span class="sxs-lookup"><span data-stu-id="4761b-305">The strongly connected components are then topologically sorted and processed in topological order:</span></span>

* <span data-ttu-id="4761b-306">如果强类型化的组件包含仅有一个元素</span><span class="sxs-lookup"><span data-stu-id="4761b-306">If the strongly typed component contains only one element,</span></span>

  * <span data-ttu-id="4761b-307">如果该元素具有已被标记为完成，跳过它。</span><span class="sxs-lookup"><span data-stu-id="4761b-307">If the element has already been marked complete, skip it.</span></span>

  * <span data-ttu-id="4761b-308">如果该元素是自变量，然后将添加类型提示从参数到依赖于它，并将标记为已完成的元素的方法类型参数。</span><span class="sxs-lookup"><span data-stu-id="4761b-308">If the element is an argument, then add type hints from the argument to the method type parameters that depend on it and mark the element as complete.</span></span> <span data-ttu-id="4761b-309">如果参数为具有参数，仍需要推断类型的 lambda 方法，然后推断`Object`有关这些参数的类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-309">If the argument is a lambda method with parameters that still need inferred types, then infer `Object` for the types of those parameters.</span></span>

  * <span data-ttu-id="4761b-310">如果该元素为方法类型参数，然后推断方法类型参数在参数类型提示之间的基准类型并将标记为已完成的元素。</span><span class="sxs-lookup"><span data-stu-id="4761b-310">If the element is a method type parameter, then infer the method type parameter to be the dominant type among the argument type hints and mark the element as complete.</span></span> <span data-ttu-id="4761b-311">类型提示对其具有数组元素限制，如果给定类型的数组之间有效的转换被视为 （即协变和内部数组转换）。</span><span class="sxs-lookup"><span data-stu-id="4761b-311">If a type hint has an array element restriction on it, then only conversions that are valid between arrays of the given type are considered (i.e. covariant and intrinsic array conversions).</span></span> <span data-ttu-id="4761b-312">如果类型提示上有一个泛型参数的限制，则认为是仅标识转换。</span><span class="sxs-lookup"><span data-stu-id="4761b-312">If a type hint has a generic argument restriction on it, then only identity conversions are considered.</span></span> <span data-ttu-id="4761b-313">如果选择没有基准类型推理失败。</span><span class="sxs-lookup"><span data-stu-id="4761b-313">If no dominant type can be chosen, inference fails.</span></span> <span data-ttu-id="4761b-314">如果任何 lambda 方法自变量类型依赖于此方法类型参数，该类型被传播到 lambda 方法。</span><span class="sxs-lookup"><span data-stu-id="4761b-314">If any lambda method argument types depend on this method type parameter, the type is propagated to the lambda method.</span></span>

* <span data-ttu-id="4761b-315">如果强类型化的组件包含多个元素，该组件包含循环。</span><span class="sxs-lookup"><span data-stu-id="4761b-315">If the strongly typed component contains more than one element, then the component contains a cycle.</span></span>

  * <span data-ttu-id="4761b-316">为每个方法类型参数的组件中的元素，如果方法类型参数依赖于未标记为完成的参数，则将该依赖项转换为将进行检查，推断过程末尾的断言。</span><span class="sxs-lookup"><span data-stu-id="4761b-316">For each method type parameter that is an element in the component, if the method type parameter depends on an argument that is not marked complete, convert that dependency into an assertion that will be checked at the end of the inference process.</span></span>

  * <span data-ttu-id="4761b-317">重新启动的强类型化的组件已确定的点在推断过程。</span><span class="sxs-lookup"><span data-stu-id="4761b-317">Restart the inference process at the point at which the strongly typed components were determined.</span></span>

<span data-ttu-id="4761b-318">如果类型推理成功的所有方法类型参数，则检查所做的更改到断言任何依赖项。</span><span class="sxs-lookup"><span data-stu-id="4761b-318">If type inference succeeds for all of the method type parameters, then any dependencies that were changed into assertions are checked.</span></span> <span data-ttu-id="4761b-319">如果自变量的类型是隐式转换为方法类型参数的推断类型，成功断言。</span><span class="sxs-lookup"><span data-stu-id="4761b-319">An assertion succeeds if the type of the argument is implicitly convertible to the inferred type of the method type parameter.</span></span> <span data-ttu-id="4761b-320">如果断言失败，则类型自变量推理失败。</span><span class="sxs-lookup"><span data-stu-id="4761b-320">If an assertion fails, then type argument inference fails.</span></span>

<span data-ttu-id="4761b-321">给定自变量类型`Ta`的参数`A`和参数类型`Tp`参数`P`，类型提示生成，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4761b-321">Given an argument type `Ta` for an argument `A` and a parameter type `Tp` for a parameter `P`, type hints are generated as follows:</span></span>

* <span data-ttu-id="4761b-322">如果`Tp`不涉及任何方法类型参数，则生成无提示。</span><span class="sxs-lookup"><span data-stu-id="4761b-322">If `Tp` does not involve any method type parameters then no hints are generated.</span></span>

* <span data-ttu-id="4761b-323">如果`Tp`并`Ta`数组类型的相同的排名，然后将为`Ta`并`Tp`与元素类型的`Ta`和`Tp`并重新启动此流程的数组元素限制。</span><span class="sxs-lookup"><span data-stu-id="4761b-323">If `Tp` and `Ta` are array types of the same rank, then replace `Ta` and `Tp` with the element types of `Ta` and `Tp` and restart this process with an array element restriction.</span></span>

* <span data-ttu-id="4761b-324">如果`Tp`为方法类型参数，则`Ta`如果任何添加为当前的限制，类型提示。</span><span class="sxs-lookup"><span data-stu-id="4761b-324">If `Tp` is a method type parameter, then `Ta` is added as a type hint with the current restriction, if any.</span></span>

* <span data-ttu-id="4761b-325">如果`A`是一种 lambda 方法并`Tp`是构造的委托类型或`System.Linq.Expressions.Expression(Of T)`，其中`T`是每个 lambda 方法参数类型的构造的委托类型，`TL`和相应的委托参数类型`TD`，替换`Ta`与`TL`并`Tp`与`TD`和重新启动该进程没有限制。</span><span class="sxs-lookup"><span data-stu-id="4761b-325">If `A` is a lambda method and `Tp` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type, for each lambda method parameter type `TL` and corresponding delegate parameter type `TD`, replace `Ta` with `TL` and `Tp` with `TD` and restart the process with no restriction.</span></span> <span data-ttu-id="4761b-326">然后，替换`Ta`与 lambda 方法的返回类型和：</span><span class="sxs-lookup"><span data-stu-id="4761b-326">Then, replace `Ta` with the return type of the lambda method and:</span></span>

  * <span data-ttu-id="4761b-327">如果`A`是一个正则 lambda 方法，将为`Tp`与委托类型; 的返回类型</span><span class="sxs-lookup"><span data-stu-id="4761b-327">if `A` is a regular lambda method, replace `Tp` with the return type of the delegate type;</span></span>
  * <span data-ttu-id="4761b-328">如果`A`是一个异步 lambda 方法和委托类型的返回类型具有窗体`Task(Of T)`某些`T`，将为`Tp`这`T`;</span><span class="sxs-lookup"><span data-stu-id="4761b-328">if `A` is an async lambda method and the return type of the delegate type has form `Task(Of T)` for some `T`, replace `Tp` with that `T`;</span></span>
  * <span data-ttu-id="4761b-329">如果`A`是一种迭代器 lambda 方法和委托类型的返回类型具有窗体`IEnumerator(Of T)`或`IEnumerable(Of T)`某些`T`，替换`Tp`这`T`。</span><span class="sxs-lookup"><span data-stu-id="4761b-329">if `A` is an iterator lambda method and the return type of the delegate type has form `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, replace `Tp` with that `T`.</span></span>
  * <span data-ttu-id="4761b-330">接下来，重新启动具有无限制的进程。</span><span class="sxs-lookup"><span data-stu-id="4761b-330">Next, restart the process with no restriction.</span></span>

* <span data-ttu-id="4761b-331">如果`A`是一个方法指针和`Tp`是构造委托类型，请使用的参数类型`Tp`来确定哪种方法指向是最适用于`Tp`。</span><span class="sxs-lookup"><span data-stu-id="4761b-331">If `A` is a method pointer and `Tp` is a constructed delegate type, use the parameter types of `Tp` to determine which method pointed is most applicable to `Tp`.</span></span> <span data-ttu-id="4761b-332">如果是最适用的方法，请更换`Ta`与该方法的返回类型和`Tp`与委托类型和重启具有无限制的进程的返回类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-332">If there is a method that is most applicable, replace `Ta` with the return type of the method and `Tp` with the return type of the delegate type and restart the process with no restriction.</span></span>

* <span data-ttu-id="4761b-333">否则为`Tp`必须是一个构造的类型。</span><span class="sxs-lookup"><span data-stu-id="4761b-333">Otherwise, `Tp` must be a constructed type.</span></span> <span data-ttu-id="4761b-334">给定`TG`的泛型类型`Tp`，</span><span class="sxs-lookup"><span data-stu-id="4761b-334">Given `TG`, the generic type of `Tp`,</span></span>

  * <span data-ttu-id="4761b-335">如果`Ta`是`TG`，继承自`TG`，或者实现类型`TG`恰好一次，然后针对每个匹配的类型实参`Tax`从`Ta`并`Tpx`从`Tp`，替换`Ta`与`Tax`并`Tp`与`Tpx`并重新启动具有泛型参数限制的进程。</span><span class="sxs-lookup"><span data-stu-id="4761b-335">If `Ta` is `TG`, inherits from `TG`, or implements the type `TG` exactly once, then for each matching type argument `Tax` from `Ta` and `Tpx` from `Tp`, replace `Ta` with `Tax` and `Tp` with `Tpx` and restart the process with a generic argument restriction.</span></span>

  * <span data-ttu-id="4761b-336">否则，泛型方法的类型推理失败。</span><span class="sxs-lookup"><span data-stu-id="4761b-336">Otherwise, type inference fails for the generic method.</span></span>

<span data-ttu-id="4761b-337">成功的类型推断不，在其本身而言，保证的方法是适用。</span><span class="sxs-lookup"><span data-stu-id="4761b-337">The success of type inference does not, in and of itself, guarantee that the method is applicable.</span></span>
