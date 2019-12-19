---
ms.openlocfilehash: a9499e51a67a9b311ae54410c001f8bd22c30d65
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306062"
---
# <a name="overloaded-method-resolution"></a><span data-ttu-id="dbb05-101">重载方法解析</span><span class="sxs-lookup"><span data-stu-id="dbb05-101">Overloaded Method Resolution</span></span>

<span data-ttu-id="dbb05-102">在实践中，确定重载决策的规则旨在查找与提供的实际参数 "最接近的" 重载。</span><span class="sxs-lookup"><span data-stu-id="dbb05-102">In practice, the rules for determining overload resolution are intended to find the overload that is "closest" to the actual arguments supplied.</span></span> <span data-ttu-id="dbb05-103">如果存在其参数类型与参数类型匹配的方法，则该方法显然是最接近的方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-103">If there is a method whose parameter types match the argument types, then that method is obviously the closest.</span></span> <span data-ttu-id="dbb05-104">如果不知道，如果其所有参数类型的范围比另一个方法的参数类型小（或与之相同），则一种方法比另一种方法要接近。</span><span class="sxs-lookup"><span data-stu-id="dbb05-104">Barring that, one method is closer than another if all of its parameter types are narrower than (or the same as) the parameter types of the other method.</span></span> <span data-ttu-id="dbb05-105">如果这两个方法的参数均小于另一个，则无法确定哪个方法接近于参数。</span><span class="sxs-lookup"><span data-stu-id="dbb05-105">If neither method's parameters are narrower than the other, then there is no way for to determine which method is closer to the arguments.</span></span>

<span data-ttu-id="dbb05-106">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="dbb05-106">__Note.__</span></span> <span data-ttu-id="dbb05-107">重载决策不考虑方法的预期返回类型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-107">Overload resolution does not take into account the expected return type of the method.</span></span> 

<span data-ttu-id="dbb05-108">另请注意，由于命名参数语法，实际参数和形参的顺序可能不相同。</span><span class="sxs-lookup"><span data-stu-id="dbb05-108">Also note that because of the named parameter syntax, the ordering of the actual and formal parameters may not be the same.</span></span>

<span data-ttu-id="dbb05-109">给定方法组后，可使用以下步骤确定参数列表组中最适合的方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-109">Given a method group, the most applicable method in the group for an argument list is determined using the following steps.</span></span> <span data-ttu-id="dbb05-110">如果在应用特定步骤后没有成员保留在集中，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="dbb05-110">If, after applying a particular step, no members remain in the set, then a compile-time error occurs.</span></span> <span data-ttu-id="dbb05-111">如果集中只有一个成员，则该成员是最适用的成员。</span><span class="sxs-lookup"><span data-stu-id="dbb05-111">If only one member remains in the set, then that member is the most applicable member.</span></span> <span data-ttu-id="dbb05-112">步骤如下：</span><span class="sxs-lookup"><span data-stu-id="dbb05-112">The steps are:</span></span>

1.  <span data-ttu-id="dbb05-113">首先，如果未提供任何类型自变量，则将类型推理应用于具有类型参数的任何方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-113">First, if no type arguments have been supplied, apply type inference to any methods which have type parameters.</span></span> <span data-ttu-id="dbb05-114">如果方法的类型推理成功，则推断出的类型参数用于该特定方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-114">If type inference succeeds for a method, then the inferred type arguments are used for that particular method.</span></span> <span data-ttu-id="dbb05-115">如果对方法的类型推理失败，则将从集合中删除该方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-115">If type inference fails for a method, then that method is eliminated from the set.</span></span>

2.  <span data-ttu-id="dbb05-116">接下来，消除集中无法访问或不适用的所有成员（[自变量列表的适用性](overload-resolution.md#applicability-to-argument-list)部分）到参数列表</span><span class="sxs-lookup"><span data-stu-id="dbb05-116">Next, eliminate all members from the set that are inaccessible or not applicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) to the argument list</span></span>

3.  <span data-ttu-id="dbb05-117">接下来，如果一个或多个参数 `AddressOf` 或 lambda 表达式，则为每个此类参数计算*委托 relaxation 级别*，如下所示。</span><span class="sxs-lookup"><span data-stu-id="dbb05-117">Next, if one or more arguments are `AddressOf` or lambda expressions, then calculate the *delegate relaxation levels* for each such argument as below.</span></span> <span data-ttu-id="dbb05-118">如果 `N` 中的最差（最低）委托 relaxation 级别低于 `M`中的最低委托 relaxation 级别，则从集中消除 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-118">If the worst (lowest) delegate relaxation level in `N` is worse than the lowest delegate relaxation level in `M`, then eliminate `N` from the set.</span></span> <span data-ttu-id="dbb05-119">委托 relaxation 级别如下所示：</span><span class="sxs-lookup"><span data-stu-id="dbb05-119">The delegate relaxation levels are as follows:</span></span>

    31. <span data-ttu-id="dbb05-120">*错误委托 relaxation 级别*--如果 `AddressOf` 或 lambda 无法转换为委托类型，则为。</span><span class="sxs-lookup"><span data-stu-id="dbb05-120">*Error delegate relaxation level* -- if the `AddressOf` or lambda cannot be converted to the delegate type.</span></span>
  
    32. <span data-ttu-id="dbb05-121">*收缩委托 relaxation 返回类型或参数*--如果自变量 `AddressOf` 或具有声明类型的 lambda，并且从其返回类型到委托返回类型的转换为收缩转换，则为;或者，如果该参数是一个常规 lambda，并且从其任何返回表达式到委托返回类型的转换是收缩的，或者，如果该参数是一个异步 lambda，并且委托返回类型为 `Task(Of T)`，并且从其任何返回表达式到 `T` 的转换为收缩，则为;或者，如果参数是迭代器 lambda，并且委托返回类型 `IEnumerator(Of T)` 或 `IEnumerable(Of T)` 并且从其任何 yield 操作数到 `T` 的转换为收缩。</span><span class="sxs-lookup"><span data-stu-id="dbb05-121">*Narrowing delegate relaxation of return type or parameters* -- if the argument is `AddressOf` or a lambda with a declared type and the conversion from its return type to the delegate return type is narrowing; or if the argument is a regular lambda and the conversion from any of its return expressions to the delegate return type is narrowing, or if the argument is an async lambda and the delegate return type is `Task(Of T)` and the conversion from any of its return expressions to `T` is narrowing; or if the argument is an iterator lambda and the delegate return type `IEnumerator(Of T)` or `IEnumerable(Of T)` and the conversion from any of its yield operands to `T` is narrowing.</span></span>

    33. <span data-ttu-id="dbb05-122">如果委托类型为 `System.Delegate` 或 `System.MultiCastDelegate` 或 `System.Object`，则*将委托 relaxation 为委托而不进行签名*。</span><span class="sxs-lookup"><span data-stu-id="dbb05-122">*Widening delegate relaxation to delegate without signature* -- if delegate type is `System.Delegate` or `System.MultiCastDelegate` or `System.Object`.</span></span>

    34. <span data-ttu-id="dbb05-123">*Drop return 或 arguments 委托 relaxation* --如果参数 `AddressOf` 或具有已声明返回类型的 lambda，并且委托类型缺少返回类型，则为;或者，如果该参数是具有一个或多个返回表达式的 lambda，并且委托类型缺少返回类型，则为; 否则为。如果参数为 `AddressOf` 或不带参数的 lambda，并且委托类型具有参数，则为。</span><span class="sxs-lookup"><span data-stu-id="dbb05-123">*Drop return or arguments delegate relaxation* -- if the argument is `AddressOf` or a lambda with a declared return type and the delegate type lacks a return type; or if the argument is a lambda with one or more return expressions and the delegate type lacks a return type; or if the argument is `AddressOf` or lambda with no parameters and the delegate type has parameters.</span></span>

    35. <span data-ttu-id="dbb05-124">*扩大委托 relaxation 返回类型*--如果参数 `AddressOf` 或具有已声明返回类型的 lambda，并且存在从其返回类型到委托的返回类型的扩大转换，则为;或者，如果参数是一个常规 lambda，则从所有返回表达式到委托返回类型的转换都是扩大或至少具有一个扩大的标识;或者，如果该参数是异步 lambda，并且委托是 `Task(Of T)` 或 `Task`，并且从所有返回表达式到的转换分别为 `T`/`Object` 则使用至少一个扩大的扩大或标识;或者，如果该参数是迭代器 lambda，并且委托是 `IEnumerator(Of T)` 或 `IEnumerable(Of T)` 或 `IEnumerator` 或 `IEnumerable`，并且从所有返回表达式到 `T`/`Object` 的转换至少是一个扩大或标识。</span><span class="sxs-lookup"><span data-stu-id="dbb05-124">*Widening delegate relaxation of return type* -- if the argument is `AddressOf` or a lambda with a declared return type, and there is a widening conversion from its return type to that of the delegate; or if the argument is a regular lambda where the conversion from all return expressions to the delegate return type is widening or identity with at least one widening; or if the argument is an async lambda and the delegate is `Task(Of T)` or `Task` and the conversion from all return expressions to `T`/`Object` respectively is widening or identity with at least one widening; or if the argument is an iterator lambda and the delegate is `IEnumerator(Of T)` or `IEnumerable(Of T)` or `IEnumerator` or `IEnumerable` and the conversion from all return expressions to `T`/`Object` is widening or identity with at least one widening.</span></span>

    36. <span data-ttu-id="dbb05-125">*标识委托 relaxation* --如果参数是与委托完全匹配的 `AddressOf` 或 lambda，则不会扩大或缩小参数或返回或生成。接下来，如果集的某些成员不需要收缩转换即可适用于任何参数，则将消除所有执行的成员。</span><span class="sxs-lookup"><span data-stu-id="dbb05-125">*Identity delegate relaxation* -- if the argument is an `AddressOf` or a lambda which matches the delegate exactly, with no widening or narrowing or dropping of parameters or returns or yields.Next, if some members of the set do not requiring narrowing conversions to be applicable to any of the arguments, then eliminate all members that do.</span></span> <span data-ttu-id="dbb05-126">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-126">For example:</span></span>

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

4.  <span data-ttu-id="dbb05-127">接下来，将根据下面的收缩完成排除。</span><span class="sxs-lookup"><span data-stu-id="dbb05-127">Next, elimination is done based on narrowing as follows.</span></span> <span data-ttu-id="dbb05-128">（请注意，如果 Option Strict 为 On，则需要进行收缩的所有成员已被判断为不适用（部分[对实参列表的适用性](overload-resolution.md#applicability-to-argument-list)）并已被步骤2删除。）</span><span class="sxs-lookup"><span data-stu-id="dbb05-128">(Note that, if Option Strict is On, then all members that require narrowing have already been judged inapplicable (Section [Applicability To Argument List](overload-resolution.md#applicability-to-argument-list)) and removed by Step 2.)</span></span>

    41. <span data-ttu-id="dbb05-129">如果集的某些实例成员只需要收缩转换，而参数表达式类型为 `Object`，则将消除所有其他成员。</span><span class="sxs-lookup"><span data-stu-id="dbb05-129">If some instance members of the set only require narrowing conversions where the argument expression type is `Object`, then eliminate all other members.</span></span>
    42. <span data-ttu-id="dbb05-130">如果集包含多个只需要从 `Object`收缩的成员，则调用目标表达式将被重新分类为后期绑定方法访问（如果包含该方法组的类型是接口，则为，如果有任何适用成员是扩展成员，则会提供错误）。</span><span class="sxs-lookup"><span data-stu-id="dbb05-130">If the set contains more than one member which requires narrowing only from `Object`, then the invocation target expression is reclassified as a late-bound method access (and an error is given if the type containing the method group is an interface, or if any of the applicable members were extension members).</span></span>
    43. <span data-ttu-id="dbb05-131">如果有任何候选项只需要从数字文本进行收缩，则在下面的步骤中选择最具体的候选项。</span><span class="sxs-lookup"><span data-stu-id="dbb05-131">If there are any candidates that only require narrowing from numeric literals, then chose the most specific among all remaining candidates by the steps below.</span></span> <span data-ttu-id="dbb05-132">如果入选方只需要从数字文本进行收缩，则会将其作为重载决策的结果选取;否则为错误。</span><span class="sxs-lookup"><span data-stu-id="dbb05-132">If the winner requires only narrowing from numeric literals, then it is picked as the result of overload resolution; otherwise it is an error.</span></span>

    <span data-ttu-id="dbb05-133">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="dbb05-133">__Note.__</span></span> <span data-ttu-id="dbb05-134">此规则的理由是，如果程序的类型为松类型（即，大多数或所有变量都声明为 `Object`），则从 `Object` 进行很多转换时，重载决策可能会很困难。</span><span class="sxs-lookup"><span data-stu-id="dbb05-134">The justification for this rule is that if a program is loosely-typed (that is, most or all variables are declared as `Object`), overload resolution can be difficult when many conversions from `Object` are narrowing.</span></span> <span data-ttu-id="dbb05-135">在许多情况下（要求强类型化方法调用的参数），而不是让重载决策在很多情况下失败，因此，解析合适的重载方法会推迟到运行时进行调用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-135">Rather than have the overload resolution fail in many situations (requiring strong typing of the arguments to the method call), resolution the appropriate overloaded method to call is deferred until run time.</span></span> <span data-ttu-id="dbb05-136">这允许松散类型的调用成功，无需进行其他强制转换。</span><span class="sxs-lookup"><span data-stu-id="dbb05-136">This allows the loosely-typed call to succeed without additional casts.</span></span> <span data-ttu-id="dbb05-137">但这种情况的副作用是，执行后期绑定调用需要将调用目标强制转换为 `Object`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-137">An unfortunate side-effect of this, however, is that performing the late-bound call requires casting the call target to `Object`.</span></span> <span data-ttu-id="dbb05-138">在结构值的情况下，这意味着必须将值装箱为临时值。</span><span class="sxs-lookup"><span data-stu-id="dbb05-138">In the case of a structure value, this means that the value must be boxed to a temporary.</span></span> <span data-ttu-id="dbb05-139">如果最终调用方法尝试更改结构字段，则此更改将在该方法返回后丢失。</span><span class="sxs-lookup"><span data-stu-id="dbb05-139">If the method eventually called tries to change a field of the structure, this change will be lost once the method returns.</span></span> <span data-ttu-id="dbb05-140">从这一特殊规则中排除了接口，因为后期绑定始终针对运行时类或结构类型的成员进行解析，其名称可能与它们所实现的接口的成员名称不同。</span><span class="sxs-lookup"><span data-stu-id="dbb05-140">Interfaces are excluded from this special rule because late binding always resolves against the members of the runtime class or structure type, which may have different names than the members of the interfaces they implement.</span></span>

5.  <span data-ttu-id="dbb05-141">接下来，如果任何实例方法保留在不需要收缩的集中，则从集中消除所有扩展方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-141">Next, if any instance methods remain in the set which do not require narrowing, then eliminate all extension methods from the set.</span></span> <span data-ttu-id="dbb05-142">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-142">For example:</span></span>

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

    <span data-ttu-id="dbb05-143">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="dbb05-143">__Note.__</span></span> <span data-ttu-id="dbb05-144">如果有适用的实例方法来保证添加导入（这可能会使新的扩展方法进入范围），则将忽略扩展方法，而不会导致对现有实例方法的调用重新绑定到扩展方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-144">Extension methods are ignored if there are applicable instance methods to guarantee that adding an import (that might bring new extension methods into scope) will not cause a call on an existing instance method to rebind to an extension method.</span></span> <span data-ttu-id="dbb05-145">给定一些扩展方法（即，在接口和/或类型参数上定义的方法）的广泛范围，这是绑定到扩展方法的更安全方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-145">Given the broad scope of some extension methods (i.e. those defined on interfaces and/or type parameters), this is a safer approach to binding to extension methods.</span></span>

6.  <span data-ttu-id="dbb05-146">接下来，如果给定 `M` 和 `N`集的任意两个成员，`M` 比给定自变量列表的 `N` 更*具体*（[给定自变量列表的成员/类型的部分类型](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)），则将从该集中消除 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-146">Next, if, given any two members of the set `M` and `N`, `M` is more *specific* (Section [Specificity of members/types given an argument list](overload-resolution.md#specificity-of-memberstypes-given-an-argument-list)) than `N` given the argument list, eliminate `N` from the set.</span></span> <span data-ttu-id="dbb05-147">如果集合中保留有多个成员，并且在给定参数列表的情况下，其余成员不是同样特定的，则会产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="dbb05-147">If more than one member remains in the set and the remaining members are not equally specific given the argument list, a compile-time error results.</span></span>

7.  <span data-ttu-id="dbb05-148">否则，给定 `M` 和 `N`这两个集的任意两个成员，按顺序应用以下附加规则规则：</span><span class="sxs-lookup"><span data-stu-id="dbb05-148">Otherwise, given any two members of the set, `M` and `N`, apply the following tie-breaking rules, in order:</span></span>

    71. <span data-ttu-id="dbb05-149">如果 `M` 没有 ParamArray 参数，但 `N` 这样做，或者 `M` 将较少的参数传递到 ParamArray 参数而不是 `N` 的参数，则从集中消除 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-149">If `M` does not have a ParamArray parameter but `N` does, or if both do but `M` passes fewer arguments into the ParamArray parameter than `N` does, then eliminate `N` from the set.</span></span> <span data-ttu-id="dbb05-150">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-150">For example:</span></span>

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

        <span data-ttu-id="dbb05-151">以上示例生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="dbb05-151">The above example produces the following output:</span></span>

        ```console
        F(Object, Object())
        F(Object, Object, Object())
        F(Object, Object, Object())
        G(Object)
        ```

        <span data-ttu-id="dbb05-152">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="dbb05-152">__Note.__</span></span> <span data-ttu-id="dbb05-153">当类声明具有 paramarray 参数的方法时，也不太常见，还会将某些扩展的窗体包含为常规方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-153">When a class declares a method with a paramarray parameter, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="dbb05-154">这样一来，就可以避免分配在调用具有 paramarray 参数的方法的展开形式时发生的数组实例。</span><span class="sxs-lookup"><span data-stu-id="dbb05-154">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a paramarray parameter is invoked.</span></span>

    72. <span data-ttu-id="dbb05-155">如果 `M` 是在派生程度高于 `N`的类型中定义的，则从集中消除 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-155">If `M` is defined in a more derived type than `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="dbb05-156">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-156">For example:</span></span>

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

        <span data-ttu-id="dbb05-157">此规则也适用于在其上定义扩展方法的类型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-157">This rule also applies to the types that extension methods are defined on.</span></span> <span data-ttu-id="dbb05-158">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-158">For example:</span></span>

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

    73. <span data-ttu-id="dbb05-159">如果 `M` 和 `N` 为扩展方法，并且 `M` 的目标类型为类或结构，并且 `N` 的目标类型是一个接口，则消除该集的 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-159">If `M` and `N` are extension methods and the target type of `M` is a class or structure and the target type of `N` is an interface, eliminate `N` from the set.</span></span> <span data-ttu-id="dbb05-160">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-160">For example:</span></span>

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

    74. <span data-ttu-id="dbb05-161">如果 `M` 和 `N` 为扩展方法，并且在类型参数替换之后 `M` 和 `N` 的目标类型相同，并且在类型参数替换之前 `M` 的目标类型不包含类型参数，但 `N` 的目标类型为，则将从该集合中消除 `N`的类型参数。`N`</span><span class="sxs-lookup"><span data-stu-id="dbb05-161">If `M` and `N` are extension methods, and the target type of `M` and `N` are identical after type parameter substitution, and the target type of `M` before type parameter substitution does not contain type parameters but the target type of `N` does, then has fewer type parameters than the target type of `N`, eliminate `N` from the set.</span></span> <span data-ttu-id="dbb05-162">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-162">For example:</span></span>

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

    75. <span data-ttu-id="dbb05-163">在替换类型参数之前，如果 `M` 比 `N`*更少*，则[泛型](overload-resolution.md#genericity)会从集中消除 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-163">Before type arguments have been substituted, if `M` is *less generic* (Section [Genericity](overload-resolution.md#genericity)) than `N`, eliminate `N` from the set.</span></span>

    76. <span data-ttu-id="dbb05-164">如果 `M` 不是扩展方法并且 `N` 为，则从集中消除 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-164">If `M` is not an extension method and `N` is, eliminate `N` from the set.</span></span>

    77. <span data-ttu-id="dbb05-165">如果 `M` 和 `N` 为扩展方法，并且在 `N` （部分[扩展方法集合](expressions.md#extension-method-collection)）之前找到了 `M`，则消除了该集的 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-165">If `M` and `N` are extension methods and `M` was found before `N` (Section [Extension Method Collection](expressions.md#extension-method-collection)), eliminate `N` from the set.</span></span> <span data-ttu-id="dbb05-166">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-166">For example:</span></span>

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

        <span data-ttu-id="dbb05-167">如果在同一步骤中找到扩展方法，则这些扩展方法是不明确的。</span><span class="sxs-lookup"><span data-stu-id="dbb05-167">If the extension methods were found in the same step, then those extension methods are ambiguous.</span></span> <span data-ttu-id="dbb05-168">此调用可能始终使用包含扩展方法的标准模块的名称，并调用扩展方法，就像它是常规成员一样。</span><span class="sxs-lookup"><span data-stu-id="dbb05-168">The call may always be disambiguated using the name of the standard module containing the extension method and calling the extension method as if it was a regular member.</span></span> <span data-ttu-id="dbb05-169">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-169">For example:</span></span>

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

    78. <span data-ttu-id="dbb05-170">如果 `M` 和 `N` 所需的类型推理来生成类型参数，并且 `M` 不需要为其任何类型参数（即，每个推断为单个类型的类型参数）确定基准类型，但 `N` 确实如此，则消除集的 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-170">If `M` and `N` both required type inference to produce type arguments, and `M` did not require determining the dominant type for any of its type arguments (i.e. each the type arguments inferred to a single type), but `N` did, eliminate `N` from the set.</span></span>

        <span data-ttu-id="dbb05-171">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="dbb05-171">__Note.__</span></span> <span data-ttu-id="dbb05-172">此规则可确保在以前版本中成功的重载决策（在其中推断类型参数的多个类型会导致错误），继续生成相同的结果。</span><span class="sxs-lookup"><span data-stu-id="dbb05-172">This rule ensures that overload resolution that succeeded in previous versions (where inferring multiple types for a type argument would cause an error), continue to produce the same results.</span></span>

    79. <span data-ttu-id="dbb05-173">如果要解析重载决策以解析 `AddressOf` 表达式中的委托创建表达式的目标，并且当 `N` 是一个子例程时，委托和 `M` 都是函数，则消除该集的 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-173">If overload resolution is being done to resolve the target of a delegate-creation expression from an `AddressOf` expression, and both the delegate and `M` are functions while `N` is a subroutine, eliminate `N` from the set.</span></span> <span data-ttu-id="dbb05-174">同样，如果委托和 `M` 都是子例程，而 `N` 是函数，则将从集中消除 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-174">Likewise, if both the delegate and `M` are subroutines, while `N` is a function, eliminate `N` from the set.</span></span>

    710. <span data-ttu-id="dbb05-175">如果 `M` 未使用任何可选参数来代替显式参数，但 `N` 为，则从集中消除 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-175">If `M` did not use any optional parameter defaults in place of explicit arguments, but `N` did, then eliminate `N` from the set.</span></span>

    711. <span data-ttu-id="dbb05-176">替换类型参数之前，如果 `M` 比 `N`具有*更 genericity* （节[genericity](overload-resolution.md#genericity)）的深度，则从集中消除 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-176">Before type arguments have been substituted, if `M` has *greater depth of genericity* (Section [Genericity](overload-resolution.md#genericity)) than `N`, then eliminate `N` from the set.</span></span>

8. <span data-ttu-id="dbb05-177">否则，调用是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="dbb05-177">Otherwise, the call is ambiguous and a compile-time error occurs.</span></span>

#### <a name="specificity-of-memberstypes-given-an-argument-list"></a><span data-ttu-id="dbb05-178">给定自变量列表的成员/类型的具体类型</span><span class="sxs-lookup"><span data-stu-id="dbb05-178">Specificity of members/types given an argument list</span></span>

<span data-ttu-id="dbb05-179">如果在给定参数列表 `A`的情况下，成员 `M` 被视为与 `N`*相等*，则为; 如果 `M` 中的每个参数类型与 `N`中的相应参数类型相同，则视为相同。</span><span class="sxs-lookup"><span data-stu-id="dbb05-179">A member `M` is considered *equally specific* as `N`, given an argument-list `A`, if their signatures are the same or if each parameter type in `M` is the same as the corresponding parameter type in `N`.</span></span>

<span data-ttu-id="dbb05-180">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="dbb05-180">__Note.__</span></span> <span data-ttu-id="dbb05-181">由于扩展方法的原因，两个成员可以在具有相同签名的方法组中结束。</span><span class="sxs-lookup"><span data-stu-id="dbb05-181">Two members can end up in a method group with the same signature due to extension methods.</span></span> <span data-ttu-id="dbb05-182">除了类型参数或 paramarray 扩展，两个成员还可以平等指定，但不能具有相同的签名。</span><span class="sxs-lookup"><span data-stu-id="dbb05-182">Two members can also be equally specific but not have the same signature due to type parameters or paramarray expansion.</span></span>

<span data-ttu-id="dbb05-183">如果成员 `M` 的签名不同，并且 `M` 中的至少一个参数类型比 `N`中的参数类型更具体，并且 `N` 中没有参数类型比 `M`中的参数类型更具体，则认为该成员的 `N`*特定程度更高*。</span><span class="sxs-lookup"><span data-stu-id="dbb05-183">A member `M` is considered *more specific* than `N` if their signatures are different and at least one parameter type in `M` is more specific than a parameter type in `N`, and no parameter type in `N` is more specific than a parameter type in `M`.</span></span> <span data-ttu-id="dbb05-184">给定一对与参数 `Aj`匹配 `Mj` 和 `Nj` 的参数时，如果满足以下条件之一，则 `Mj` 的类型将被视为比 `Nj` 类型*更具体*：</span><span class="sxs-lookup"><span data-stu-id="dbb05-184">Given a pair of parameters `Mj` and `Nj` that matches an argument `Aj`, the type of `Mj` is considered *more specific* than the type of `Nj` if one of the following conditions is true:</span></span>

* <span data-ttu-id="dbb05-185">存在从 `Mj` 类型到类型 `Nj`的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="dbb05-185">There exists a widening conversion from the type of `Mj` to the type `Nj`.</span></span> <span data-ttu-id="dbb05-186">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="dbb05-186">(__Note.__</span></span> <span data-ttu-id="dbb05-187">由于在这种情况下，将对参数类型进行比较而不考虑实参类型，因此，在这种情况下，将不考虑从常量表达式扩大到数值类型的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="dbb05-187">Because parameters types are being compared without regard to the actual argument in this case, the widening conversion from constant expressions to a numeric type the value fits into is not considered in this case.)</span></span>

* <span data-ttu-id="dbb05-188">`Aj` 是文本 `0`，`Mj` 为数值类型，`Nj` 为枚举类型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-188">`Aj` is the literal `0`, `Mj` is a numeric type and `Nj` is an enumerated type.</span></span> <span data-ttu-id="dbb05-189">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="dbb05-189">(__Note.__</span></span> <span data-ttu-id="dbb05-190">此规则是必需的，因为文本 `0` 扩大到任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-190">This rule is necessary because the literal `0` widens to any enumerated type.</span></span> <span data-ttu-id="dbb05-191">枚举类型扩大到其基础类型，这意味着 `0` 上的重载决策默认情况下，对数值类型使用枚举类型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-191">Since an enumerated type widens to its underlying type, this means that overload resolution on `0` will, by default, prefer enumerated types over numeric types.</span></span> <span data-ttu-id="dbb05-192">我们收到了大量反馈，指出此行为是不够直观的。）</span><span class="sxs-lookup"><span data-stu-id="dbb05-192">We received a lot of feedback that this behavior was counterintuitive.)</span></span>

* <span data-ttu-id="dbb05-193">`Mj` 和 `Nj` 均为数值类型，`Mj` 早于列表中的 `Nj` `Byte`、`SByte`、`Short`、`UShort`、`Integer`、`UInteger`、`Long`、`ULong`、`Decimal`、`Single`、`Double`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-193">`Mj` and `Nj` are both numeric types, and `Mj` comes earlier than `Nj` in the list `Byte`, `SByte`, `Short`, `UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`.</span></span> <span data-ttu-id="dbb05-194">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="dbb05-194">(__Note.__</span></span> <span data-ttu-id="dbb05-195">有关数值类型的规则非常有用，因为特定大小的有符号和无符号的数值类型仅在它们之间进行收缩转换。</span><span class="sxs-lookup"><span data-stu-id="dbb05-195">The rule about the numeric types is useful because the signed and unsigned numeric types of a particular size only have narrowing conversions between them.</span></span> <span data-ttu-id="dbb05-196">上述规则打破了两种类型之间的关联，以支持更多的 "自然" 数值类型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-196">The above rule breaks the tie between the two types in favor of the more "natural" numeric type.</span></span> <span data-ttu-id="dbb05-197">当对扩展到特定大小的有符号和无符号数字类型的类型（例如，同时满足这两个的数字文本）执行重载解析时，这一点尤其重要。</span><span class="sxs-lookup"><span data-stu-id="dbb05-197">This is particularly important when doing overload resolution on a type that widens to both the signed and unsigned numeric types of a particular size, for example, a numeric literal that fits into both.)</span></span>

* <span data-ttu-id="dbb05-198">`Mj` 和 `Nj` 是委托函数类型，`Mj` 的返回类型比 `Nj` 的返回类型更具体：如果 `Aj` 归类为 lambda 方法，`Mj` 或 `Nj` 为 `System.Linq.Expressions.Expression(Of T)`，则类型的类型参数（假设为委托类型）将替换为要比较的类型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-198">`Mj` and `Nj` are delegate function types and the return type of `Mj` is more specific than the return type of `Nj` If `Aj` is classified as a lambda method, and `Mj` or `Nj` is `System.Linq.Expressions.Expression(Of T)`, then the type argument of the type (assuming it is a delegate type) is substituted for the type being compared.</span></span>

* <span data-ttu-id="dbb05-199">`Mj` 与 `Aj`的类型相同，`Nj` 不相同。</span><span class="sxs-lookup"><span data-stu-id="dbb05-199">`Mj` is identical to the type of `Aj`, and `Nj` is not.</span></span> <span data-ttu-id="dbb05-200">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="dbb05-200">(__Note.__</span></span> <span data-ttu-id="dbb05-201">需要注意的是，上一规则与之间C#略有不同，在C#这种情况下，需要在比较返回类型之前委托函数类型具有相同的参数列表，而 Visual Basic。）</span><span class="sxs-lookup"><span data-stu-id="dbb05-201">It is interesting to note that the previous rule differs slightly from C#, in that C# requires that the delegate function types have identical parameter lists before comparing return types, while Visual Basic does not.)</span></span>

#### <a name="genericity"></a><span data-ttu-id="dbb05-202">Genericity</span><span class="sxs-lookup"><span data-stu-id="dbb05-202">Genericity</span></span>

<span data-ttu-id="dbb05-203">将成员 `M` 确定为*不如*成员 `N`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dbb05-203">A member `M` is determined to be *less generic* than a member `N` as follows:</span></span>

1. <span data-ttu-id="dbb05-204">如果为每对匹配参数 `Mj` 和 `Nj`，则 `Mj` 比方法上的类型形参更少或等同于 `Nj`，并且对于方法的类型参数，至少有一个 `Mj` 的泛型更少。</span><span class="sxs-lookup"><span data-stu-id="dbb05-204">If, for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the method, and at least one `Mj` is less generic with respect to type parameters on the method.</span></span>
2. <span data-ttu-id="dbb05-205">否则，如果为每对匹配参数 `Mj` 和 `Nj`，则 `Mj` 比类型参数上的类型参数更少或同等通用，与类型上的类型参数 `Nj` 相比，对于类型参数而言，`Mj` 的泛型更少，因此 `M` 不如 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-205">Otherwise, if for each pair of matching parameters `Mj` and `Nj`, `Mj` is less or equally generic than `Nj` with respect to type parameters on the type, and at least one `Mj` is less generic with respect to type parameters on the type, then `M` is less generic than `N`.</span></span>

<span data-ttu-id="dbb05-206">如果参数的类型 `Mt` 和 `Nt` 都引用类型参数，或者两者都不引用类型参数，则将参数 `M` 视为与参数 `N` 相同的泛型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-206">A parameter `M` is considered to be equally generic to a parameter `N` if their types `Mt` and `Nt` both refer to type parameters or both don't refer to type parameters.</span></span> <span data-ttu-id="dbb05-207">如果 `Mt` 未引用类型参数，并且 `Nt` 执行，则将 `M` 视为低于 `N`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-207">`M` is considered to be less generic than `N` if `Mt` does not refer to a type parameter and `Nt` does.</span></span>

<span data-ttu-id="dbb05-208">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-208">For example:</span></span>

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

<span data-ttu-id="dbb05-209">在 currying 过程中固定的扩展方法类型参数被视为类型参数，而不是方法上的类型参数。</span><span class="sxs-lookup"><span data-stu-id="dbb05-209">Extension method type parameters that were fixed during currying are considered type parameters on the type, not type parameters on the method.</span></span> <span data-ttu-id="dbb05-210">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-210">For example:</span></span>

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

#### <a name="depth-of-genericity"></a><span data-ttu-id="dbb05-211">Genericity 的深度</span><span class="sxs-lookup"><span data-stu-id="dbb05-211">Depth of genericity</span></span>

<span data-ttu-id="dbb05-212">确定成员 `M` 的*深度*超过了成员 `N` 如果对于每对匹配参数 `Mj` 和 `Nj`，`Mj` 的*genericity 深度*等于或等于 `Nj`，并且至少有一个 `Mj` 具有更深层 genericity。</span><span class="sxs-lookup"><span data-stu-id="dbb05-212">A member `M` is determined to have *greater depth of genericity* than a member `N` if, for each pair of matching parameters  `Mj` and `Nj`, `Mj` has greater or equal *depth of genericity* than `Nj`, and at least one `Mj` is has greater depth of genericity.</span></span> <span data-ttu-id="dbb05-213">Genericity 的深度定义如下：</span><span class="sxs-lookup"><span data-stu-id="dbb05-213">Depth of genericity is defined as follows:</span></span>

* <span data-ttu-id="dbb05-214">除了类型参数外，任何内容都具有比类型参数更深层的 genericity;</span><span class="sxs-lookup"><span data-stu-id="dbb05-214">Anything other than a type parameter has greater depth of genericity than a type parameter;</span></span>

* <span data-ttu-id="dbb05-215">与其他构造类型（具有相同数量的类型参数），如果至少有一个类型自变量的深度为 genericity，并且没有类型参数的深度低于对应的类型，则递归使用构造类型的 genericity 的深度更高参数。</span><span class="sxs-lookup"><span data-stu-id="dbb05-215">Recursively, a constructed type has greater depth of genericity than another constructed type (with the same number of type arguments) if at least one type argument has greater depth of genericity and no type argument has less depth than the corresponding type argument in the other.</span></span>

* <span data-ttu-id="dbb05-216">如果第一个数组的元素类型具有比第二个元素类型更高的 genericity 深度，则数组类型具有比另一个数组类型（具有相同维数）更多的 genericity。</span><span class="sxs-lookup"><span data-stu-id="dbb05-216">An array type has greater depth of genericity than another array type (with the same number of dimensions) if the element type of the first has greater depth of genericity than the element type of the second.</span></span>

<span data-ttu-id="dbb05-217">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-217">For example:</span></span>

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

### <a name="applicability-to-argument-list"></a><span data-ttu-id="dbb05-218">自变量列表的适用性</span><span class="sxs-lookup"><span data-stu-id="dbb05-218">Applicability To Argument List</span></span>

<span data-ttu-id="dbb05-219">如果可使用参数列表调用方法，则方法*适用*于一组类型参数、位置参数和命名参数。</span><span class="sxs-lookup"><span data-stu-id="dbb05-219">A method is *applicable* to a set of type arguments, positional arguments, and named arguments if the method can be invoked using the argument lists.</span></span> <span data-ttu-id="dbb05-220">参数列表与参数列表匹配，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dbb05-220">The argument lists are matched against the parameter lists as follows:</span></span>

1. <span data-ttu-id="dbb05-221">首先，将每个位置参数与方法参数列表进行匹配。</span><span class="sxs-lookup"><span data-stu-id="dbb05-221">First, match each positional argument in order to the list of method parameters.</span></span> <span data-ttu-id="dbb05-222">如果有比参数更多的位置参数，且最后一个参数不是 paramarray，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-222">If there are more positional arguments than parameters and the last parameter is not a paramarray, the method is not applicable.</span></span> <span data-ttu-id="dbb05-223">否则，会通过 paramarray 元素类型的参数来扩展 paramarray 参数，使之与位置参数的数目匹配。</span><span class="sxs-lookup"><span data-stu-id="dbb05-223">Otherwise, the paramarray parameter is expanded with parameters of the paramarray element type to match the number of positional arguments.</span></span> <span data-ttu-id="dbb05-224">如果省略将进入 paramarray 的位置参数，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-224">If a positional argument is omitted that would go into a paramarray, the method is not applicable.</span></span>
2. <span data-ttu-id="dbb05-225">接下来，将每个命名参数与具有给定名称的参数相匹配。</span><span class="sxs-lookup"><span data-stu-id="dbb05-225">Next, match each named argument to a parameter with the given name.</span></span> <span data-ttu-id="dbb05-226">如果某个命名参数无法匹配、匹配 paramarray 参数或匹配已与另一个位置或命名参数匹配的参数，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-226">If one of the named arguments fails to match, matches a paramarray parameter, or matches an argument already matched with another positional or named argument, the method is not applicable.</span></span>
3. <span data-ttu-id="dbb05-227">接下来，如果指定了类型参数，则将其与类型参数列表进行匹配。</span><span class="sxs-lookup"><span data-stu-id="dbb05-227">Next, if type arguments have been specified, they are matched against the type parameter list .</span></span> <span data-ttu-id="dbb05-228">如果两个列表的元素数不相同，则该方法不适用，除非类型参数列表为空。</span><span class="sxs-lookup"><span data-stu-id="dbb05-228">If the two lists do not have the same number of elements, the method is not applicable, unless the type argument list is empty.</span></span> <span data-ttu-id="dbb05-229">如果类型参数列表为空，则使用类型推理来尝试并推断类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="dbb05-229">If the type argument list is empty, type inference is used to try and infer the type argument list.</span></span> <span data-ttu-id="dbb05-230">如果类型推断失败，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-230">If type inferencing fails, the method is not applicable.</span></span> <span data-ttu-id="dbb05-231">否则，将在签名中填充类型参数的位置。如果未匹配的参数不是可选的，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-231">Otherwise, the type arguments are filled in the place of the type parameters in the signature.If parameters that have not been matched are not optional, the method is not applicable.</span></span>
4. <span data-ttu-id="dbb05-232">如果自变量表达式不能隐式转换为它们匹配的参数类型，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-232">If the argument expressions are not implicitly convertible to the types of the parameters they match, then the method is not applicable.</span></span>
5. <span data-ttu-id="dbb05-233">如果参数是 ByRef，并且没有从参数类型到参数类型的隐式转换，则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-233">If a parameter is ByRef, and there is not an implicit conversion from the type of the parameter to the type of the argument, then the method is not applicable.</span></span>
6. <span data-ttu-id="dbb05-234">如果类型参数违反了方法的约束（包括步骤3中推断出的类型参数），则该方法不适用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-234">If type arguments violate the method's constraints (including the inferred type arguments from Step 3), the method is not applicable.</span></span> <span data-ttu-id="dbb05-235">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-235">For example:</span></span>

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

<span data-ttu-id="dbb05-236">如果单个自变量表达式与 paramarray 参数匹配并且参数表达式的类型可转换为 paramarray 参数和 paramarray 元素类型的类型，则该方法适用于其展开和未扩展的形式。有两个例外。</span><span class="sxs-lookup"><span data-stu-id="dbb05-236">If a single argument expression matches a paramarray parameter and the type of the argument expression is convertible to both the type of the paramarray parameter and the paramarray element type, the method is applicable in both its expanded and unexpanded forms, with two exceptions.</span></span> <span data-ttu-id="dbb05-237">如果从参数表达式的类型到 paramarray 类型的转换是收缩转换，则该方法仅适用于其扩展形式。</span><span class="sxs-lookup"><span data-stu-id="dbb05-237">If the conversion from the type of the argument expression to the paramarray type is narrowing, then the method is only applicable in its expanded form.</span></span> <span data-ttu-id="dbb05-238">如果参数表达式是 `Nothing`的文本，则该方法仅适用于其未展开的形式。</span><span class="sxs-lookup"><span data-stu-id="dbb05-238">If the argument expression is the literal `Nothing`, then the method is only applicable in its unexpanded form.</span></span> <span data-ttu-id="dbb05-239">例如：</span><span class="sxs-lookup"><span data-stu-id="dbb05-239">For example:</span></span>

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

<span data-ttu-id="dbb05-240">以上示例生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="dbb05-240">The above example produces the following output:</span></span>

```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="dbb05-241">在 `F`的第一次和最后一次调用中，`F` 的正常形式适用，因为存在从参数类型到参数类型的扩大转换（两者都是 `Object()`类型），并且参数将作为常规值参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="dbb05-241">In the first and last invocations of `F`, the normal form of `F` is applicable because a widening conversion exists from the argument type to the parameter type (both are of type `Object()`), and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="dbb05-242">在第二次和第三次调用中，`F` 的正常形式不适用，因为不存在从参数类型到参数类型的扩大转换（从 `Object` 到 `Object()` 的转换是收缩转换）。</span><span class="sxs-lookup"><span data-stu-id="dbb05-242">In the second and third invocations, the normal form of `F` is not applicable because no widening conversion exists from the argument type to the parameter type (conversions from `Object` to `Object()` are narrowing).</span></span> <span data-ttu-id="dbb05-243">不过，`F` 的展开形式适用，并且由调用创建一个单元素 `Object()`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-243">However, the expanded form of `F` is applicable, and a one-element `Object()` is created by the invocation.</span></span> <span data-ttu-id="dbb05-244">使用给定的参数值（其本身是对 `Object()`的引用）初始化数组的单个元素。</span><span class="sxs-lookup"><span data-stu-id="dbb05-244">The single element of the array is initialized with the given argument value (which itself is a reference to an `Object()`).</span></span>

### <a name="passing-arguments-and-picking-arguments-for-optional-parameters"></a><span data-ttu-id="dbb05-245">传递参数和可选参数的选取参数</span><span class="sxs-lookup"><span data-stu-id="dbb05-245">Passing Arguments, and Picking Arguments for Optional Parameters</span></span>

<span data-ttu-id="dbb05-246">如果参数是值参数，则匹配参数表达式必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="dbb05-246">If a parameter is a value parameter, the matching argument expression must be classified as a value.</span></span> <span data-ttu-id="dbb05-247">该值将转换为参数的类型并在运行时作为参数传入。</span><span class="sxs-lookup"><span data-stu-id="dbb05-247">The value is converted to the type of the parameter and passed in as the parameter at run time.</span></span> <span data-ttu-id="dbb05-248">如果参数是引用参数，且匹配参数表达式归类为类型与参数相同的变量，则在运行时将作为参数传入对变量的引用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-248">If the parameter is a reference parameter and the matching argument expression is classified as a variable whose type is the same as the parameter, then a reference to the variable is passed in as the parameter at run time.</span></span>

<span data-ttu-id="dbb05-249">否则，如果匹配的参数表达式归类为变量、值或属性访问，则会分配参数类型的临时变量。</span><span class="sxs-lookup"><span data-stu-id="dbb05-249">Otherwise, if the matching argument expression is classified as a variable, value, or property access, then a temporary variable of the type of the parameter is allocated.</span></span> <span data-ttu-id="dbb05-250">在运行时调用方法之前，将参数表达式重新分类为值，转换为参数的类型，并将其分配给临时变量。</span><span class="sxs-lookup"><span data-stu-id="dbb05-250">Before the method invocation at run time, the argument expression is reclassified as a value, converted to the type of the parameter, and assigned to the temporary variable.</span></span> <span data-ttu-id="dbb05-251">然后，将作为参数传入对临时变量的引用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-251">Then a reference to the temporary variable is passed in as the parameter.</span></span> <span data-ttu-id="dbb05-252">计算方法调用后，如果参数表达式归类为变量或属性访问，则会将临时变量分配给变量表达式或属性访问表达式。</span><span class="sxs-lookup"><span data-stu-id="dbb05-252">After the method invocation is evaluated, if the argument expression is classified as a variable or property access, the temporary variable is assigned to the variable expression or the property access expression.</span></span> <span data-ttu-id="dbb05-253">如果属性访问表达式没有 `Set` 访问器，则不执行赋值。</span><span class="sxs-lookup"><span data-stu-id="dbb05-253">If the property access expression has no `Set` accessor, then the assignment is not performed.</span></span>

<span data-ttu-id="dbb05-254">对于未提供参数的可选参数，编译器将选取如下所述的参数。</span><span class="sxs-lookup"><span data-stu-id="dbb05-254">For optional parameters where an argument has not been provided, the compiler picks arguments as described below.</span></span> <span data-ttu-id="dbb05-255">在所有情况下，它针对泛型类型替换之后的参数类型进行测试。</span><span class="sxs-lookup"><span data-stu-id="dbb05-255">In all cases it tests against the parameter type after generic type substitution.</span></span>

* <span data-ttu-id="dbb05-256">如果可选参数具有 `System.Runtime.CompilerServices.CallerLineNumber`的属性，并且调用来自源代码中的位置，并且表示该位置的行号的数值文本具有到参数类型的内部转换，则使用数值文本。</span><span class="sxs-lookup"><span data-stu-id="dbb05-256">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerLineNumber`, and the invocation is from a location in source code, and a numeric literal representing that location's line number has an intrinsic conversion to the parameter type, then the numeric literal is used.</span></span> <span data-ttu-id="dbb05-257">如果调用跨多行，则选择要使用的行是依赖于实现的。</span><span class="sxs-lookup"><span data-stu-id="dbb05-257">If the invocation spans multiple lines, then the choice of which line to use is implementation-dependent.</span></span>

* <span data-ttu-id="dbb05-258">如果可选参数 `System.Runtime.CompilerServices.CallerFilePath`的属性，并且调用来自源代码中的位置，并且表示该位置的文件路径的字符串文本包含到参数类型的内部转换，则使用字符串文字。</span><span class="sxs-lookup"><span data-stu-id="dbb05-258">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerFilePath`, and the invocation is from a location in source code, and a string literal representing that location's file path has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="dbb05-259">文件路径的格式取决于实现。</span><span class="sxs-lookup"><span data-stu-id="dbb05-259">The format of the file path is implementation-dependent.</span></span>

* <span data-ttu-id="dbb05-260">如果可选参数 `System.Runtime.CompilerServices.CallerMemberName`的属性，并且调用位于类型成员的主体内或应用于该类型成员的任何部分的特性中，并且表示该成员名称的字符串文本具有到参数类型的内部转换，则使用字符串文本。</span><span class="sxs-lookup"><span data-stu-id="dbb05-260">If the optional parameter has the attribute `System.Runtime.CompilerServices.CallerMemberName`, and the invocation is within the body of a type member or in an attribute applied to any part of that type member, and a string literal representing that member name has an intrinsic conversion to the parameter type, then the string literal is used.</span></span> <span data-ttu-id="dbb05-261">对于属性访问器或自定义事件处理程序中的调用，使用的成员名称是属性或事件本身的成员名称。</span><span class="sxs-lookup"><span data-stu-id="dbb05-261">For invocations that are part of property accessors or custom event handlers, then the member name used is that of the property or event itself.</span></span> <span data-ttu-id="dbb05-262">对于属于运算符或构造函数的调用，将使用特定于实现的名称。</span><span class="sxs-lookup"><span data-stu-id="dbb05-262">For invocations that are part of an operator or constructor, then an implementation-specific name is used.</span></span>

<span data-ttu-id="dbb05-263">如果以上均不适用，则使用可选参数的默认值（如果未提供默认值，则为 `Nothing`）。</span><span class="sxs-lookup"><span data-stu-id="dbb05-263">If none of the above apply, then the optional parameter's default value is used (or `Nothing` if no default value is supplied).</span></span> <span data-ttu-id="dbb05-264">如果以上应用了多个，则所选的使用是依赖于实现的。</span><span class="sxs-lookup"><span data-stu-id="dbb05-264">And if more than one of the above apply, then the choice of which to use is implementation-dependent.</span></span>

<span data-ttu-id="dbb05-265">`CallerLineNumber` 和 `CallerFilePath` 特性对于日志记录很有用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-265">The `CallerLineNumber` and `CallerFilePath` attributes are useful for logging.</span></span> <span data-ttu-id="dbb05-266">`CallerMemberName` 可用于实现 `INotifyPropertyChanged`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-266">The `CallerMemberName` is useful for implementing `INotifyPropertyChanged`.</span></span> <span data-ttu-id="dbb05-267">下面是一些示例。</span><span class="sxs-lookup"><span data-stu-id="dbb05-267">Here are examples.</span></span>

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

<span data-ttu-id="dbb05-268">除了上面的可选参数外，Microsoft Visual Basic 还可识别某些其他可选参数（如果这些参数是从元数据导入的（即从 DLL 引用）：</span><span class="sxs-lookup"><span data-stu-id="dbb05-268">In addition to the optional parameters above, Microsoft Visual Basic also recognizes some additional optional parameters if they are imported from metadata (i.e. from a DLL reference):</span></span>

* <span data-ttu-id="dbb05-269">从元数据中导入时，Visual Basic 还会将参数 `<Optional>` 视为可选参数：通过这种方式，可以导入具有可选参数但没有默认值的声明，即使无法使用 `Optional` 关键字表示此参数。</span><span class="sxs-lookup"><span data-stu-id="dbb05-269">Upon importing from metadata, Visual Basic also treats the parameter `<Optional>` as indicative that the parameter is optional: in this way it is possible to import a declaration which has an optional parameter but no default value, even though this can't be expressed using the `Optional` keyword.</span></span>

* <span data-ttu-id="dbb05-270">如果可选参数具有 `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`的属性，并且数字文本1或0具有到参数类型的转换，则编译器将使用原义参数（如果 `Option Compare Text` 有效），或者如果 `Optional Compare Binary` 有效，则使用原义字符串。</span><span class="sxs-lookup"><span data-stu-id="dbb05-270">If the optional parameter has the attribute `Microsoft.VisualBasic.CompilerServices.OptionCompareAttribute`, and the numeric literal 1 or 0 has a conversion to the parameter type, then the compiler uses as argument either the literal 1 if `Option Compare Text` is in effect, or the literal 0 if `Optional Compare Binary` is in effect.</span></span>

* <span data-ttu-id="dbb05-271">如果可选参数具有 `System.Runtime.CompilerServices.IDispatchConstantAttribute`的属性，并且它具有类型 `Object`，并且它未指定默认值，则编译器将使用参数 `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-271">If the optional parameter has the attribute `System.Runtime.CompilerServices.IDispatchConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.DispatchWrapper(Nothing)`.</span></span>

* <span data-ttu-id="dbb05-272">如果可选参数具有 `System.Runtime.CompilerServices.IUnknownConstantAttribute`的属性，并且它具有类型 `Object`，并且它未指定默认值，则编译器将使用参数 `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-272">If the optional parameter has the attribute `System.Runtime.CompilerServices.IUnknownConstantAttribute`, and it has type `Object`, and it does not specify a default value, then the compiler uses the argument `New System.Runtime.InteropServices.UnknownWrapper(Nothing)`.</span></span>

* <span data-ttu-id="dbb05-273">如果可选参数具有类型 `Object`，且未指定默认值，则编译器将使用参数 `System.Reflection.Missing.Value`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-273">If the optional parameter has type `Object`, and it does not specify a default value, then the compiler uses the argument `System.Reflection.Missing.Value`.</span></span>

### <a name="conditional-methods"></a><span data-ttu-id="dbb05-274">条件方法</span><span class="sxs-lookup"><span data-stu-id="dbb05-274">Conditional Methods</span></span>

<span data-ttu-id="dbb05-275">如果调用表达式引用的目标方法是不是接口成员的子程序，并且如果该方法具有一个或多个 `System.Diagnostics.ConditionalAttribute` 属性，则表达式的计算取决于在源文件中的该点处定义的条件编译常量。</span><span class="sxs-lookup"><span data-stu-id="dbb05-275">If the target method to which an invocation expression refers is a subroutine that is not a member of an interface and if the method has one or more `System.Diagnostics.ConditionalAttribute` attributes, evaluation of the expression depends on the conditional compilation constants defined at that point in the source file.</span></span> <span data-ttu-id="dbb05-276">特性的每个实例指定一个字符串，该字符串命名条件编译常量。</span><span class="sxs-lookup"><span data-stu-id="dbb05-276">Each instance of the attribute specifies a string, which names a conditional compilation constant.</span></span> <span data-ttu-id="dbb05-277">计算每个条件编译常量，就像它是条件编译语句的一部分一样。</span><span class="sxs-lookup"><span data-stu-id="dbb05-277">Each conditional compilation constant is evaluated as if it were part of a conditional compilation statement.</span></span> <span data-ttu-id="dbb05-278">如果常量的计算结果为 `True`，则会在运行时正常计算表达式。</span><span class="sxs-lookup"><span data-stu-id="dbb05-278">If the constant evaluates to `True`, the expression is evaluated normally at run time.</span></span> <span data-ttu-id="dbb05-279">如果常量的计算结果为 `False`，则不会计算表达式。</span><span class="sxs-lookup"><span data-stu-id="dbb05-279">If the constant evaluates to `False`, the expression is not evaluated at all.</span></span>

<span data-ttu-id="dbb05-280">查找特性时，将检查可重写方法的派生程度最高的声明。</span><span class="sxs-lookup"><span data-stu-id="dbb05-280">When looking for the attribute, the most derived declaration of an overridable method is checked.</span></span>

<span data-ttu-id="dbb05-281">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="dbb05-281">__Note.__</span></span> <span data-ttu-id="dbb05-282">特性在函数或接口方法上无效，如果在这两种方法中指定，则将被忽略。</span><span class="sxs-lookup"><span data-stu-id="dbb05-282">The attribute is not valid on functions or interface methods and is ignored if specified on either kind of method.</span></span> <span data-ttu-id="dbb05-283">因此，条件方法将仅出现在调用语句中。</span><span class="sxs-lookup"><span data-stu-id="dbb05-283">Thus, conditional methods will only appear in invocation statements.</span></span>

### <a name="type-argument-inference"></a><span data-ttu-id="dbb05-284">类型参数推理</span><span class="sxs-lookup"><span data-stu-id="dbb05-284">Type Argument Inference</span></span>

<span data-ttu-id="dbb05-285">如果在未指定类型实参的情况下调用具有类型参数的方法，则使用*类型参数推理*来尝试并推断调用的类型参数。</span><span class="sxs-lookup"><span data-stu-id="dbb05-285">When a method with type parameters is called without specifying type arguments, *type argument inference* is used to try and infer type arguments for the call.</span></span> <span data-ttu-id="dbb05-286">这允许在完全推断类型参数时使用更自然的语法来调用具有类型参数的方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-286">This allows a more natural syntax to be used for calling a method with type parameters when the type arguments can be trivially inferred.</span></span> <span data-ttu-id="dbb05-287">例如，给定以下方法声明：</span><span class="sxs-lookup"><span data-stu-id="dbb05-287">For example, given the following method declaration:</span></span>

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

<span data-ttu-id="dbb05-288">无需显式指定类型参数即可调用 `Choose` 方法：</span><span class="sxs-lookup"><span data-stu-id="dbb05-288">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>

```vb
' calls Choose(Of Integer)
Dim i As Integer = Util.Choose(True, 5, 213)
' calls Choose(Of String)
Dim s As String = Util.Choose(False, "a", "b") 
```

<span data-ttu-id="dbb05-289">通过类型参数推理，`Integer` 和 `String` 的类型参数由方法的参数决定。</span><span class="sxs-lookup"><span data-stu-id="dbb05-289">Through type argument inference, the type arguments `Integer` and `String` are determined from the arguments to the method.</span></span>

<span data-ttu-id="dbb05-290">类型参数推理发生在对参数列表中的 lambda 方法或方法指针执行表达式重新分类*之前*，因为这两种表达式的重新分类可能需要已知参数的类型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-290">Type argument inference occurs *before* expression reclassification is performed on lambda methods or method pointers in the argument list, since reclassification of those two kinds of expressions may require the type of the parameter to be known.</span></span>  <span data-ttu-id="dbb05-291">给定一组参数 `A1,...,An`、一组匹配参数 `P1,...,Pn` 和一组方法类型参数 `T1,...,Tn`，将首先收集自变量和方法类型参数之间的依赖关系，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dbb05-291">Given a set of arguments `A1,...,An`, a set of matching parameters `P1,...,Pn` and a set of method type parameters `T1,...,Tn`, the dependencies between the arguments and method type parameters are first collected as follows:</span></span>

* <span data-ttu-id="dbb05-292">如果 `An` 是 `Nothing` 文本，则不会生成任何依赖项。</span><span class="sxs-lookup"><span data-stu-id="dbb05-292">If `An` is the `Nothing` literal, no dependencies are generated.</span></span>

* <span data-ttu-id="dbb05-293">如果 `An` 是 lambda 方法并且 `Pn` 的类型是构造的委托类型或 `System.Linq.Expressions.Expression(Of T)`，其中 `T` 是构造的委托类型，</span><span class="sxs-lookup"><span data-stu-id="dbb05-293">If `An` is a lambda method and the type of `Pn` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type,</span></span>

* <span data-ttu-id="dbb05-294">如果将从相应参数 `Pn`的类型推断出 lambda 方法参数的类型，并且参数的类型依赖于方法类型参数 `Tn`，则 `An` 依赖于 `Tn`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-294">If the type of a lambda method parameter will be inferred from the type of the corresponding parameter `Pn`, and the type of the parameter depends on a method type parameter `Tn`, then `An` has a dependency on `Tn`.</span></span>

* <span data-ttu-id="dbb05-295">如果指定了 lambda 方法参数的类型，并且相应参数的类型 `Pn` 依赖于 `Tn`的方法类型参数，则 `Tn` 依赖于 `An`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-295">If the type of a lambda method parameter is specified and the type of the corresponding parameter `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="dbb05-296">如果 `Pn` 的返回类型取决于 `Tn`的方法类型参数，则 `Tn` 依赖于 `An`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-296">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="dbb05-297">如果 `An` 是方法指针，并且 `Pn` 的类型是构造的委托类型，</span><span class="sxs-lookup"><span data-stu-id="dbb05-297">If `An` is a method pointer and the type of `Pn` is a constructed delegate type,</span></span>

* <span data-ttu-id="dbb05-298">如果 `Pn` 的返回类型取决于 `Tn`的方法类型参数，则 `Tn` 依赖于 `An`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-298">If the return type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="dbb05-299">如果 `Pn` 是构造类型，并且 `Pn` 的类型依赖于方法类型参数 `Tn`，则 `Tn` 对 `An`具有依赖关系。</span><span class="sxs-lookup"><span data-stu-id="dbb05-299">If `Pn` is a constructed type and the type of `Pn` depends on a method type parameter `Tn`, then `Tn` has a dependency on `An`.</span></span>

* <span data-ttu-id="dbb05-300">否则，不会生成依赖项。</span><span class="sxs-lookup"><span data-stu-id="dbb05-300">Otherwise, no dependency is generated.</span></span>

<span data-ttu-id="dbb05-301">收集依赖项后，不会删除任何依赖关系的参数。</span><span class="sxs-lookup"><span data-stu-id="dbb05-301">After collecting dependencies, any arguments that have no dependencies are eliminated.</span></span> <span data-ttu-id="dbb05-302">如果任何方法类型参数没有传出依赖项（即方法类型参数不依赖于某个参数），则类型推理将失败。</span><span class="sxs-lookup"><span data-stu-id="dbb05-302">If any method type parameters have no outgoing dependencies (i.e. the method type parameter does not depend on an argument), then type inference fails.</span></span> <span data-ttu-id="dbb05-303">否则，其余的参数和方法类型参数将分组为强连接组件。</span><span class="sxs-lookup"><span data-stu-id="dbb05-303">Otherwise, the remaining arguments and method type parameters are grouped into strongly connected components.</span></span> <span data-ttu-id="dbb05-304">强连接组件是一组自变量和方法类型参数，可通过其他元素上的依赖项访问组件中的任何元素。</span><span class="sxs-lookup"><span data-stu-id="dbb05-304">A strongly connected component is a set of arguments and method type parameters, where any element in the component is reachable via dependencies on other elements.</span></span>

<span data-ttu-id="dbb05-305">然后按拓扑顺序对强连接组件进行界定闭合排序和处理：</span><span class="sxs-lookup"><span data-stu-id="dbb05-305">The strongly connected components are then topologically sorted and processed in topological order:</span></span>

* <span data-ttu-id="dbb05-306">如果强类型组件只包含一个元素，</span><span class="sxs-lookup"><span data-stu-id="dbb05-306">If the strongly typed component contains only one element,</span></span>

  * <span data-ttu-id="dbb05-307">如果该元素已标记为已完成，则跳过它。</span><span class="sxs-lookup"><span data-stu-id="dbb05-307">If the element has already been marked complete, skip it.</span></span>

  * <span data-ttu-id="dbb05-308">如果该元素是参数，则从参数向依赖于它的方法类型参数添加类型提示，并将该元素标记为完成。</span><span class="sxs-lookup"><span data-stu-id="dbb05-308">If the element is an argument, then add type hints from the argument to the method type parameters that depend on it and mark the element as complete.</span></span> <span data-ttu-id="dbb05-309">如果参数是一个 lambda 方法，其参数仍需要推断类型，则推断这些参数的类型 `Object`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-309">If the argument is a lambda method with parameters that still need inferred types, then infer `Object` for the types of those parameters.</span></span>

  * <span data-ttu-id="dbb05-310">如果该元素是一个方法类型参数，则将该方法类型参数推断为参数类型提示中的主导类型，并将该元素标记为 complete。</span><span class="sxs-lookup"><span data-stu-id="dbb05-310">If the element is a method type parameter, then infer the method type parameter to be the dominant type among the argument type hints and mark the element as complete.</span></span> <span data-ttu-id="dbb05-311">如果类型提示在其上具有数组元素限制，则仅考虑在给定类型的数组之间有效的转换（即协变和内部数组转换）。</span><span class="sxs-lookup"><span data-stu-id="dbb05-311">If a type hint has an array element restriction on it, then only conversions that are valid between arrays of the given type are considered (i.e. covariant and intrinsic array conversions).</span></span> <span data-ttu-id="dbb05-312">如果类型提示对其具有泛型参数限制，则仅考虑标识转换。</span><span class="sxs-lookup"><span data-stu-id="dbb05-312">If a type hint has a generic argument restriction on it, then only identity conversions are considered.</span></span> <span data-ttu-id="dbb05-313">如果没有可选择的主导类型，推理将失败。</span><span class="sxs-lookup"><span data-stu-id="dbb05-313">If no dominant type can be chosen, inference fails.</span></span> <span data-ttu-id="dbb05-314">如果任何 lambda 方法参数类型依赖于此方法类型参数，则会将该类型传播到 lambda 方法。</span><span class="sxs-lookup"><span data-stu-id="dbb05-314">If any lambda method argument types depend on this method type parameter, the type is propagated to the lambda method.</span></span>

* <span data-ttu-id="dbb05-315">如果强类型化组件包含多个元素，则组件将包含一个循环。</span><span class="sxs-lookup"><span data-stu-id="dbb05-315">If the strongly typed component contains more than one element, then the component contains a cycle.</span></span>

  * <span data-ttu-id="dbb05-316">对于属于组件中的元素的每个方法类型参数，如果方法类型参数依赖于未标记为 "已完成" 的参数，请将该依赖项转换为断言，在推理过程结束时将检查该依赖项。</span><span class="sxs-lookup"><span data-stu-id="dbb05-316">For each method type parameter that is an element in the component, if the method type parameter depends on an argument that is not marked complete, convert that dependency into an assertion that will be checked at the end of the inference process.</span></span>

  * <span data-ttu-id="dbb05-317">在确定强类型化组件的点重新启动推理过程。</span><span class="sxs-lookup"><span data-stu-id="dbb05-317">Restart the inference process at the point at which the strongly typed components were determined.</span></span>

<span data-ttu-id="dbb05-318">如果所有方法类型参数的类型推理均已成功，则将检查已更改为断言的任何依赖项。</span><span class="sxs-lookup"><span data-stu-id="dbb05-318">If type inference succeeds for all of the method type parameters, then any dependencies that were changed into assertions are checked.</span></span> <span data-ttu-id="dbb05-319">如果参数的类型可隐式转换为方法类型参数的推断类型，则断言成功。</span><span class="sxs-lookup"><span data-stu-id="dbb05-319">An assertion succeeds if the type of the argument is implicitly convertible to the inferred type of the method type parameter.</span></span> <span data-ttu-id="dbb05-320">如果断言失败，则类型参数推理将失败。</span><span class="sxs-lookup"><span data-stu-id="dbb05-320">If an assertion fails, then type argument inference fails.</span></span>

<span data-ttu-id="dbb05-321">给定参数类型 `Ta` `A` 参数类型和参数类型 `Tp` 参数 `P`，按如下所示生成类型提示：</span><span class="sxs-lookup"><span data-stu-id="dbb05-321">Given an argument type `Ta` for an argument `A` and a parameter type `Tp` for a parameter `P`, type hints are generated as follows:</span></span>

* <span data-ttu-id="dbb05-322">如果 `Tp` 不涉及任何方法类型参数，则不会生成任何提示。</span><span class="sxs-lookup"><span data-stu-id="dbb05-322">If `Tp` does not involve any method type parameters then no hints are generated.</span></span>

* <span data-ttu-id="dbb05-323">如果 `Tp` 和 `Ta` 是相同级别的数组类型，则将 `Ta` 和 `Tp` 替换为 `Ta` 和 `Tp` 的元素类型，并并使用数组元素限制重新启动此进程。</span><span class="sxs-lookup"><span data-stu-id="dbb05-323">If `Tp` and `Ta` are array types of the same rank, then replace `Ta` and `Tp` with the element types of `Ta` and `Tp` and restart this process with an array element restriction.</span></span>

* <span data-ttu-id="dbb05-324">如果 `Tp` 是方法类型参数，则 `Ta` 添加为当前限制的类型提示（如果有）。</span><span class="sxs-lookup"><span data-stu-id="dbb05-324">If `Tp` is a method type parameter, then `Ta` is added as a type hint with the current restriction, if any.</span></span>

* <span data-ttu-id="dbb05-325">如果 `A` 是 lambda 方法，而 `Tp` 是构造的委托类型或 `System.Linq.Expressions.Expression(Of T)`（其中 `T` 是构造的委托类型），则对于每个 lambda 方法参数类型 `TL` 和对应的委托参数类型 `TD`，将 `Ta` 替换为 `TL`，并使用 `Tp` 重新启动该过程。`TD`</span><span class="sxs-lookup"><span data-stu-id="dbb05-325">If `A` is a lambda method and `Tp` is a constructed delegate type or `System.Linq.Expressions.Expression(Of T)`, where `T` is a constructed delegate type, for each lambda method parameter type `TL` and corresponding delegate parameter type `TD`, replace `Ta` with `TL` and `Tp` with `TD` and restart the process with no restriction.</span></span> <span data-ttu-id="dbb05-326">然后，将 `Ta` 替换为 lambda 方法的返回类型，并执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="dbb05-326">Then, replace `Ta` with the return type of the lambda method and:</span></span>

  * <span data-ttu-id="dbb05-327">如果 `A` 是常规 lambda 方法，则将 `Tp` 替换为委托类型的返回类型;</span><span class="sxs-lookup"><span data-stu-id="dbb05-327">if `A` is a regular lambda method, replace `Tp` with the return type of the delegate type;</span></span>
  * <span data-ttu-id="dbb05-328">如果 `A` 是异步 lambda 方法，并且委托类型的返回类型对某些 `T`具有窗体 `Task(Of T)`，请将 `Tp` 替换为该 `T`;</span><span class="sxs-lookup"><span data-stu-id="dbb05-328">if `A` is an async lambda method and the return type of the delegate type has form `Task(Of T)` for some `T`, replace `Tp` with that `T`;</span></span>
  * <span data-ttu-id="dbb05-329">如果 `A` 是迭代器 lambda 方法，并且委托类型的返回类型对某些 `T`具有形式 `IEnumerator(Of T)` 或 `IEnumerable(Of T)`，则将 `Tp` 替换为该 `T`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-329">if `A` is an iterator lambda method and the return type of the delegate type has form `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, replace `Tp` with that `T`.</span></span>
  * <span data-ttu-id="dbb05-330">接下来，重新启动无限制的进程。</span><span class="sxs-lookup"><span data-stu-id="dbb05-330">Next, restart the process with no restriction.</span></span>

* <span data-ttu-id="dbb05-331">如果 `A` 是方法指针，并且 `Tp` 是构造的委托类型，请使用 `Tp` 的参数类型来确定所指向的哪种方法最适用于 `Tp`。</span><span class="sxs-lookup"><span data-stu-id="dbb05-331">If `A` is a method pointer and `Tp` is a constructed delegate type, use the parameter types of `Tp` to determine which method pointed is most applicable to `Tp`.</span></span> <span data-ttu-id="dbb05-332">如果有一个最适用的方法，请将 `Ta` 替换为该方法的返回类型，并将其 `Tp` 为委托类型的返回类型，并在没有限制的情况下重新启动该过程。</span><span class="sxs-lookup"><span data-stu-id="dbb05-332">If there is a method that is most applicable, replace `Ta` with the return type of the method and `Tp` with the return type of the delegate type and restart the process with no restriction.</span></span>

* <span data-ttu-id="dbb05-333">否则，`Tp` 必须为构造类型。</span><span class="sxs-lookup"><span data-stu-id="dbb05-333">Otherwise, `Tp` must be a constructed type.</span></span> <span data-ttu-id="dbb05-334">给定的 `TG`，`Tp`的泛型类型，</span><span class="sxs-lookup"><span data-stu-id="dbb05-334">Given `TG`, the generic type of `Tp`,</span></span>

  * <span data-ttu-id="dbb05-335">如果 `TG``Ta`、从 `TG`继承或只 `TG` 实现一次类型，则对于每个匹配的类型自变量 `Tax` 从 `Ta` `Tpx` 和 `Tp`，请将 `Ta` 替换为 `Tax`，并使用 `Tp` 来重新启动该过程。`Tpx`</span><span class="sxs-lookup"><span data-stu-id="dbb05-335">If `Ta` is `TG`, inherits from `TG`, or implements the type `TG` exactly once, then for each matching type argument `Tax` from `Ta` and `Tpx` from `Tp`, replace `Ta` with `Tax` and `Tp` with `Tpx` and restart the process with a generic argument restriction.</span></span>

  * <span data-ttu-id="dbb05-336">否则，泛型方法的类型推理将失败。</span><span class="sxs-lookup"><span data-stu-id="dbb05-336">Otherwise, type inference fails for the generic method.</span></span>

<span data-ttu-id="dbb05-337">类型推理的成功本身并不保证此方法适用。</span><span class="sxs-lookup"><span data-stu-id="dbb05-337">The success of type inference does not, in and of itself, guarantee that the method is applicable.</span></span>
