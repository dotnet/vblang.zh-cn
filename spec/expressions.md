---
ms.openlocfilehash: 8f250f8cee957fa60e7970ace250e8d7ce145fd7
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306164"
---
# <a name="expressions"></a><span data-ttu-id="ba6f6-101">表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-101">Expressions</span></span>

<span data-ttu-id="ba6f6-102">表达式是运算符和操作数的序列，用于指定值的计算或指定变量或常数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-102">An expression is a sequence of operators and operands that specifies a computation of a value, or that designates a variable or constant.</span></span> <span data-ttu-id="ba6f6-103">本章定义语法、操作数和运算符的计算顺序，以及表达式的含义。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

```antlr
Expression
    : SimpleExpression
    | TypeExpression
    | MemberAccessExpression
    | DictionaryAccessExpression
    | InvocationExpression
    | IndexExpression
    | NewExpression
    | CastExpression
    | OperatorExpression
    | ConditionalExpression
    | LambdaExpression
    | QueryExpression
    | XMLLiteralExpression
    | XMLMemberAccessExpression
    ;
```

## <a name="expression-classifications"></a><span data-ttu-id="ba6f6-104">表达式分类</span><span class="sxs-lookup"><span data-stu-id="ba6f6-104">Expression Classifications</span></span>

<span data-ttu-id="ba6f6-105">每个表达式都分类为以下内容之一：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-105">Every expression is classified as one of the following:</span></span>

* <span data-ttu-id="ba6f6-106">*一个值。*</span><span class="sxs-lookup"><span data-stu-id="ba6f6-106">*A value.*</span></span> <span data-ttu-id="ba6f6-107">每个值都有关联的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-107">Every value has an associated type.</span></span>

* <span data-ttu-id="ba6f6-108">*一个变量。*</span><span class="sxs-lookup"><span data-stu-id="ba6f6-108">*A variable.*</span></span> <span data-ttu-id="ba6f6-109">每个变量都有关联的类型，即变量的声明类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-109">Every variable has an associated type, namely the declared type of the variable.</span></span>

* <span data-ttu-id="ba6f6-110">*命名空间。*</span><span class="sxs-lookup"><span data-stu-id="ba6f6-110">*A namespace.*</span></span> <span data-ttu-id="ba6f6-111">具有此分类的表达式只能显示为成员访问的左侧。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-111">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="ba6f6-112">在任何其他上下文中，归类为命名空间的表达式会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>

* <span data-ttu-id="ba6f6-113">*类型。*</span><span class="sxs-lookup"><span data-stu-id="ba6f6-113">*A type.*</span></span> <span data-ttu-id="ba6f6-114">具有此分类的表达式只能显示为成员访问的左侧。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-114">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="ba6f6-115">在其他任何上下文中，归类为类型的表达式会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>

* <span data-ttu-id="ba6f6-116">*一个方法组，* 它是在同一名称上重载的一组方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-116">*A method group,* which is a set of methods overloaded on the same name.</span></span> <span data-ttu-id="ba6f6-117">一个方法组可以有一个关联的目标表达式和一个关联的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-117">A method group may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="ba6f6-118">*一个方法指针，* 表示方法的位置。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-118">*A method pointer,* which represents the location of a method.</span></span> <span data-ttu-id="ba6f6-119">方法指针可以具有关联的目标表达式和关联的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-119">A method pointer may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="ba6f6-120">*Lambda 方法，* 它是一种匿名方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-120">*A lambda method,* which is an anonymous method.</span></span>

* <span data-ttu-id="ba6f6-121">*属性组，* 它是在同一名称上重载的一组属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-121">*A property group,* which is a set of properties overloaded on the same name.</span></span> <span data-ttu-id="ba6f6-122">属性组可能具有关联的目标表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-122">A property group may have an associated target expression.</span></span>

* <span data-ttu-id="ba6f6-123">*属性访问。*</span><span class="sxs-lookup"><span data-stu-id="ba6f6-123">*A property access.*</span></span> <span data-ttu-id="ba6f6-124">每个属性访问都有关联的类型，即属性的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-124">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="ba6f6-125">属性访问可能具有关联的目标表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-125">A property access may have an associated target expression.</span></span>

* <span data-ttu-id="ba6f6-126">*后期绑定访问，* 表示在运行时延迟的方法或属性访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-126">*A late-bound access,* which represents a method or property access deferred until run-time.</span></span> <span data-ttu-id="ba6f6-127">后期绑定访问可能具有关联的目标表达式和关联的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-127">A late-bound access may have an associated target expression and an associated type argument list.</span></span> <span data-ttu-id="ba6f6-128">后期绑定访问的类型始终 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-128">The type of a late-bound access is always `Object`.</span></span>

* <span data-ttu-id="ba6f6-129">*事件访问。*</span><span class="sxs-lookup"><span data-stu-id="ba6f6-129">*An event access.*</span></span> <span data-ttu-id="ba6f6-130">每个事件访问都有关联的类型，即事件的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="ba6f6-131">事件访问可能具有关联的目标表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-131">An event access may have an associated target expression.</span></span> <span data-ttu-id="ba6f6-132">事件访问可能显示为 `RaiseEvent`、`AddHandler`和 `RemoveHandler` 语句的第一个参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-132">An event access may appear as the first argument of the `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements.</span></span> <span data-ttu-id="ba6f6-133">在其他任何上下文中，归类为事件访问的表达式会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>

* <span data-ttu-id="ba6f6-134">*数组文本，* 表示其类型尚未确定的数组的初始值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-134">*An array literal,* which represents the initial values of an array whose type has not yet been determined.</span></span>

* <span data-ttu-id="ba6f6-135">*导致.*</span><span class="sxs-lookup"><span data-stu-id="ba6f6-135">*Void.*</span></span> <span data-ttu-id="ba6f6-136">当表达式为子程序的调用或没有结果的 await 运算符表达式时，会发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-136">This occurs when the expression is an invocation of a subroutine, or an await operator expression with no result.</span></span> <span data-ttu-id="ba6f6-137">归类为 void 的表达式仅在调用语句或 await 语句的上下文中有效。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-137">An expression classified as void is only valid in the context of an invocation statement or an await statement.</span></span>

* <span data-ttu-id="ba6f6-138">*默认值。*</span><span class="sxs-lookup"><span data-stu-id="ba6f6-138">*A default value.*</span></span> <span data-ttu-id="ba6f6-139">只有文本 `Nothing` 生成此分类。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-139">Only the literal `Nothing` produces this classification.</span></span>

<span data-ttu-id="ba6f6-140">表达式的最终结果通常是一个值或变量，其他类别的表达式作为仅在某些上下文中允许的中间值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-140">The final result of an expression is usually a value or a variable, with the other categories of expressions functioning as intermediate values that are only permitted in certain contexts.</span></span>

<span data-ttu-id="ba6f6-141">请注意，其类型为类型参数的表达式可用于需要表达式的类型具有特定特征（如引用类型、值类型、从某种类型派生等的语句和表达式）的表达式。在类型参数上，满足这些特性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-141">Note that expressions whose type is a type parameter can be used in statements and expressions that require the type of an expression to have certain characteristics (such as being a reference type, value type, deriving from some type, etc.) if the constraints imposed on the type parameter satisfy those characteristics.</span></span>

### <a name="expression-reclassification"></a><span data-ttu-id="ba6f6-142">表达式重新分类</span><span class="sxs-lookup"><span data-stu-id="ba6f6-142">Expression Reclassification</span></span>

<span data-ttu-id="ba6f6-143">通常情况下，如果在需要与表达式的分类不同的上下文中使用表达式，则会发生编译时错误，例如，尝试为文本赋值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-143">Normally, when an expression is used in a context that requires a classification different from that of the expression, a compile-time error occurs -- for example, attempting to assign a value to a literal.</span></span> <span data-ttu-id="ba6f6-144">但是，在许多情况下，可以通过重新*分类*的过程更改表达式的分类。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-144">However, in many cases it is possible to change an expression's classification through the process of *reclassification*.</span></span>

<span data-ttu-id="ba6f6-145">如果重新分类成功，则重新分类将被视为扩大或缩小。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-145">If reclassification succeeds, then the reclassification is judged as widening or narrowing.</span></span> <span data-ttu-id="ba6f6-146">除非另有说明，否则此列表中的所有 reclassifications 都是扩大的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-146">Unless otherwise noted, all the reclassifications in this list are widening.</span></span>

<span data-ttu-id="ba6f6-147">可对以下类型的表达式进行重新分类：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-147">The following types of expressions can be reclassified:</span></span>

* <span data-ttu-id="ba6f6-148">变量可以重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-148">A variable can be reclassified as a value.</span></span> <span data-ttu-id="ba6f6-149">将提取存储在变量中的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-149">The value stored in the variable is fetched.</span></span>

* <span data-ttu-id="ba6f6-150">可以将方法组重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-150">A method group can be reclassified as a value.</span></span> <span data-ttu-id="ba6f6-151">方法组表达式被解释为带有关联目标表达式和类型参数列表的调用表达式和空括号（即，`f` 被解释为 `f()` 并将 `f(Of Integer)` 解释为 `f(Of Integer)()`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-151">The method group expression is interpreted as an invocation expression with the associated target expression and type parameter list, and empty parentheses (that is, `f` is interpreted as `f()` and `f(Of Integer)` is interpreted as `f(Of Integer)()`).</span></span> <span data-ttu-id="ba6f6-152">这种重新分类可能会导致表达式进一步重新分类为 void。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-152">This reclassification may result in the expression being further reclassified as void.</span></span>

* <span data-ttu-id="ba6f6-153">可以将方法指针重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-153">A method pointer can be reclassified as a value.</span></span> <span data-ttu-id="ba6f6-154">这种重新分类只能在目标类型已知的转换上下文中发生。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-154">This reclassification can only occur in the context of a conversion where the target type is known.</span></span> <span data-ttu-id="ba6f6-155">方法指针表达式被解释为具有关联类型参数列表的适当类型的委托实例化表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-155">The method pointer expression is interpreted as the argument to a delegate instantiation expression of the appropriate type with the associated type argument list.</span></span> <span data-ttu-id="ba6f6-156">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-156">For example:</span></span>
    
    ```vb
    Delegate Sub D(i As Integer)
    
    Module Test
        Sub F(i As Integer)
        End Sub
    
        Sub Main()
            Dim del As D
    
            ' The next two lines are equivalent.
            del = AddressOf F
            del = New D(AddressOf F)
        End Sub
    End Module
    ```

* <span data-ttu-id="ba6f6-157">Lambda 方法可重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-157">A lambda method can be reclassified as a value.</span></span> <span data-ttu-id="ba6f6-158">如果在目标类型已知的转换上下文中发生重新分类，则可能出现以下两个 reclassifications 之一：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-158">If the reclassification occurs in the context of a conversion where the target type is known, then one of two reclassifications can occur:</span></span>
    
  1. <span data-ttu-id="ba6f6-159">如果目标类型为委托类型，则将 lambda 方法解释为适当类型的委托构造表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-159">If the target type is a delegate type, the lambda method is interpreted as the argument to a delegate-construction expression of the appropriate type.</span></span>
    
  2. <span data-ttu-id="ba6f6-160">如果目标类型为 `System.Linq.Expressions.Expression(Of T)`，且 `T` 为委托类型，则 lambda 方法将被解释为用于 `T` 的委托构造表达式中，然后将其转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-160">If the target type is `System.Linq.Expressions.Expression(Of T)`, and `T` is a delegate type, then the lambda method is interpreted as if it was being used in delegate-construction expression for `T` and then converted to an expression tree.</span></span>
    
  <span data-ttu-id="ba6f6-161">如果委托没有 ByRef 参数，则异步或迭代器 lambda 方法只能解释为委托构造表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-161">An async or iterator lambda method may only be interpreted as the argument to a delegate-construction expression, if the delegate has no ByRef parameters.</span></span>
    
  <span data-ttu-id="ba6f6-162">如果从任何委托的参数类型到相应 lambda 参数类型的转换是收缩转换，则重新分类将被视为收缩转换;否则为扩大。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-162">If conversion from any of the delegate's parameter types to the corresponding lambda parameter types is a narrowing conversion, then the reclassification is judged as narrowing; otherwise it is widening.</span></span>
    
  <span data-ttu-id="ba6f6-163">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-163">__Note.__</span></span> <span data-ttu-id="ba6f6-164">Lambda 方法和表达式树之间的确切转换可能不会在编译器版本之间固定，并且超出了本规范的范围。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-164">The exact translation between lambda methods and expression trees may not be fixed between versions of the compiler and is beyond the scope of this specification.</span></span> <span data-ttu-id="ba6f6-165">对于 Microsoft Visual Basic 11.0，所有 lambda 表达式都可以转换为表达式树，但有以下限制：（1）1。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-165">For Microsoft Visual Basic 11.0, all lambda expressions may be converted to expression trees subject to the following restrictions: (1) 1.</span></span>  <span data-ttu-id="ba6f6-166">只有无 ByRef 参数的单行 lambda 表达式才能转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-166">Only single-line lambda expressions without ByRef parameters may be converted to expression trees.</span></span> <span data-ttu-id="ba6f6-167">对于单行 `Sub` lambda，只能将调用语句转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-167">Of the single-line `Sub` lambdas, only invocation statements may be converted to expression trees.</span></span> <span data-ttu-id="ba6f6-168">（2）如果使用早期的字段初始值设定项初始化后续字段初始值设定项（例如 `New With {.a=1, .b=.a}`），则不能将匿名类型表达式转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-168">(2) Anonymous type expressions cannot be converted to expression trees if an earlier field initializer is used to initialize a subsequent field initializer, e.g. `New With {.a=1, .b=.a}`.</span></span> <span data-ttu-id="ba6f6-169">（3）如果当前要初始化的对象的成员在一个字段初始值设定项（例如 `New C1 With {.a=1, .b=.Method1()}`）中使用，则不能将其转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-169">(3) Object initializer expressions cannot be converted to expression trees if a member of the current object being initialized is used in one of the field initializers, e.g. `New C1 With {.a=1, .b=.Method1()}`.</span></span> <span data-ttu-id="ba6f6-170">（4）仅当多维数组创建表达式显式声明其元素类型时，才能转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-170">(4) Multi-dimensional array creation expressions can only be converted to expression trees if they declare their element type explicitly.</span></span> <span data-ttu-id="ba6f6-171">（5）后期绑定表达式不能转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-171">(5) Late-binding expressions cannot be converted to expression trees.</span></span> <span data-ttu-id="ba6f6-172">（6）当变量或字段将 ByRef 传递到调用表达式时，但其类型与 ByRef 参数的类型不完全相同时，或者当属性传递 ByRef 时，一般 VB 语义是参数的副本传递 ByRef，然后再复制其最终值 返回到变量或字段或属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-172">(6) When a variable or field is passed ByRef to an invocation expression but does not have exactly the same type as the ByRef parameter, or when a property is passed ByRef, normal VB semantics are that a copy of the argument is passed ByRef and its final value is then copied back into the variable or field or property.</span></span> <span data-ttu-id="ba6f6-173">在表达式树中，不会进行复制。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-173">In expression trees, the copy-back does not happen.</span></span> <span data-ttu-id="ba6f6-174">（7）所有这些限制也适用于嵌套的 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-174">(7) All these restrictions apply to nested lambda expressions as well.</span></span>
    
  <span data-ttu-id="ba6f6-175">如果目标类型未知，则将 lambda 方法解释为具有 lambda 方法的相同签名的匿名委托类型的委托实例化表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-175">If the target type is not known, then the lambda method is interpreted as the argument to a delegate instantiation expression of an anonymous delegate type with the same signature of the lambda method.</span></span> <span data-ttu-id="ba6f6-176">如果使用了严格语义并且省略了任何参数的类型，则会发生编译时错误;否则，会将 `Object` 替换为缺少的任何参数类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-176">If strict semantics are being used and the type of any of the parameters are omitted, a compile-time error occurs; otherwise, `Object` is substituted for any missing parameter type.</span></span> <span data-ttu-id="ba6f6-177">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-177">For example:</span></span>
    
  ```vb
  Module Test
      Sub Main()
          ' Type of x will be equivalent to Func(Of Object, Object, Object)
          Dim x = Function(a, b) a + b
  
          ' Type of y will be equivalent to Action(Of Object, Object)
          Dim y = Sub(a, b) Console.WriteLine(a + b)
      End Sub
  End Module
  ```

* <span data-ttu-id="ba6f6-178">可以将属性组重新分类为属性访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-178">A property group can be reclassified as a property access.</span></span> <span data-ttu-id="ba6f6-179">属性组表达式被解释为带有空括号的索引表达式（即，`f` 被解释为 `f()`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-179">The property group expression is interpreted as an index expression with empty parentheses (that is, `f` is interpreted as `f()`).</span></span>

* <span data-ttu-id="ba6f6-180">属性访问可以重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-180">A property access can be reclassified as a value.</span></span> <span data-ttu-id="ba6f6-181">属性访问表达式被解释为属性 `Get` 访问器的调用表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-181">The property access expression is interpreted as an invocation expression of the `Get` accessor of the property.</span></span> <span data-ttu-id="ba6f6-182">如果该属性没有 getter，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-182">If the property has no getter, then a compile-time error occurs.</span></span>

* <span data-ttu-id="ba6f6-183">可以将后期绑定访问重新分类为后期绑定方法或后期绑定属性访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-183">A late-bound access can be reclassified as a late-bound method or late-bound property access.</span></span> <span data-ttu-id="ba6f6-184">如果可以将后期绑定访问同时作为方法访问和属性访问进行重新分类，则优先使用属性访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-184">In a situation where a late-bound access can be reclassified both as a method access and as a property access, reclassification to a property access is preferred.</span></span>

* <span data-ttu-id="ba6f6-185">可以将后期绑定访问重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-185">A late-bound access can be reclassified as a value.</span></span>

* <span data-ttu-id="ba6f6-186">数组文本可以重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-186">An array literal can be reclassified as a value.</span></span> <span data-ttu-id="ba6f6-187">确定值的类型，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-187">The type of the value is determined as follows:</span></span>

  1. <span data-ttu-id="ba6f6-188">如果在目标类型已知并且目标类型是数组类型的转换上下文中发生重新分类，则数组文本将被重新分类为 T （）类型的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-188">If the reclassification occurs in the context of a conversion where the target type is known and the target type is an array type, then the array literal is reclassified as a value of type T().</span></span> <span data-ttu-id="ba6f6-189">如果目标类型为 `System.Collections.Generic.IList(Of T)`、`IReadOnlyList(Of T)`、`ICollection(Of T)`、`IReadOnlyCollection(Of T)`或 `IEnumerable(Of T)`，并且数组文本具有一级嵌套，则数组文本将被重新分类为类型 `T()`的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-189">If the target type is `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, or `IEnumerable(Of T)`, and the array literal has one level of nesting, then the array literal is reclassified as a value of type `T()`.</span></span>

  2. <span data-ttu-id="ba6f6-190">否则，数组文本将被重新分类为一个值，该值的类型为等于嵌套的级别的数组，元素类型是由初始值设定项中的元素的主导类型确定的;如果不能确定任何主导类型，则使用 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-190">Otherwise, the array literal is reclassified to a value whose type is an array of rank equal to the level of nesting is used, with element type determined by the dominant type of the elements in the initializer; if no dominant type can be determined, `Object` is used.</span></span> <span data-ttu-id="ba6f6-191">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-191">For example:</span></span>

     ```vb
     ' x Is GetType(Double(,,))
     Dim x = { { { 1, 2.0 }, { 3, 4 } }, { { 5, 6 }, { 7, 8 } } }.GetType()
        
     ' y Is GetType(Integer())
     Dim y = { 1, 2, 3 }.GetType()
        
     ' z Is GetType(Object())
     Dim z = { 1, "2" }.GetType()
        
     ' Error: Inconsistent nesting
     Dim a = { { 10 }, { 20, 30 } }.GetType()
     ```

  <span data-ttu-id="ba6f6-192">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-192">__Note.__</span></span> <span data-ttu-id="ba6f6-193">语言版本9.0 和版本10.0 之间的行为稍有不同。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-193">There is a slight change in behavior between version 9.0 and version 10.0 of the language.</span></span> <span data-ttu-id="ba6f6-194">在10.0 之前，数组元素初始值设定项不影响局部变量类型推理，现在不影响。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-194">Prior to 10.0, array element initializers did not affect local variable type inference and now they do.</span></span> <span data-ttu-id="ba6f6-195">因此 `Dim a() = { 1, 2, 3 }` 将 `Object()` 版本 `Integer()` 9.0 中的 `a` 类型推断为版本10.0 中的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-195">So `Dim a() = { 1, 2, 3 }` would have inferred `Object()` as the type of `a` in version 9.0 of the language and `Integer()` in version 10.0.</span></span>

  <span data-ttu-id="ba6f6-196">重新分类后，数组文本重新解释为数组创建表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-196">The reclassification then reinterprets the array literal as an array-creation expression.</span></span> <span data-ttu-id="ba6f6-197">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-197">So the examples:</span></span>

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  <span data-ttu-id="ba6f6-198">等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-198">are equivalent to:</span></span>

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  <span data-ttu-id="ba6f6-199">如果从元素表达式到数组元素类型的任何转换都是收缩的，则重新分类将被视为收缩;否则，会将其视为扩大。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-199">The reclassification is judged as narrowing if any conversion from an element expression to the array element type is narrowing; otherwise it is judged as widening.</span></span>

* <span data-ttu-id="ba6f6-200">`Nothing` 可以将默认值重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-200">The default value `Nothing` can be reclassified as a value.</span></span> <span data-ttu-id="ba6f6-201">在目标类型已知的上下文中，结果是目标类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-201">In a context where the target type is known, the result is the default value of the target type.</span></span> <span data-ttu-id="ba6f6-202">如果目标类型未知，则结果为类型 `Object`的 null 值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-202">In a context where the target type is not known, the result is a null value of type `Object`.</span></span>

<span data-ttu-id="ba6f6-203">命名空间表达式、类型 expression、事件访问表达式或 void 表达式不能进行重新分类。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-203">A namespace expression, type expression, event access expression, or void expression cannot be reclassified.</span></span> <span data-ttu-id="ba6f6-204">可以同时执行多个 reclassifications。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-204">Multiple reclassifications can be done at the same time.</span></span> <span data-ttu-id="ba6f6-205">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-205">For example:</span></span>

```vb
Module Test
    Sub F(i As Integer)
    End Sub

    ReadOnly Property P() As Integer
        Get
        End Get
    End Sub

    Sub Main()
        F(P)
    End Property
End Module
```

<span data-ttu-id="ba6f6-206">在这种情况下，属性组表达式 `P` 首先从属性组重新分类为属性访问，然后从属性访问值重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-206">In this case, the property group expression `P` is first reclassified from a property group to a property access and then reclassified from a property access to a value.</span></span> <span data-ttu-id="ba6f6-207">执行的 reclassifications 数最少，以达到上下文中的有效分类。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-207">The fewest number of reclassifications are performed to reach a valid classification in the context.</span></span>

## <a name="constant-expressions"></a><span data-ttu-id="ba6f6-208">常量表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-208">Constant Expressions</span></span>

<span data-ttu-id="ba6f6-209">*常数表达式*是一个表达式，其值可以在编译时完全计算。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-209">A *constant expression* is an expression whose value can be fully evaluated at compile time.</span></span>

```antlr
ConstantExpression
    : Expression
    ;
```

<span data-ttu-id="ba6f6-210">常数表达式的类型可以是 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Char`、`Single`、`Double`、`Decimal`、`Date`、`Boolean`、`String`、`Object`或任意枚举类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-210">The type of a constant expression can be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, or any enumeration type.</span></span> <span data-ttu-id="ba6f6-211">常数表达式中允许使用以下构造：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-211">The following constructs are permitted in constant expressions:</span></span>

* <span data-ttu-id="ba6f6-212">文本（包括 `Nothing`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-212">Literals (including `Nothing`).</span></span>

* <span data-ttu-id="ba6f6-213">对常量类型成员或常量局部变量的引用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-213">References to constant type members or constant locals.</span></span>

* <span data-ttu-id="ba6f6-214">对枚举类型的成员的引用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-214">References to members of enumeration types.</span></span>

* <span data-ttu-id="ba6f6-215">带括号的子表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-215">Parenthesized subexpressions.</span></span>

* <span data-ttu-id="ba6f6-216">强制表达式，前提是目标类型是上面列出的类型之一。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-216">Coercion expressions, provided the target type is one of the types listed above.</span></span> <span data-ttu-id="ba6f6-217">与 `String` 的强制转换是此规则的例外，只允许在 null 值上使用，因为 `String` 转换始终在运行时在执行环境的当前区域性中完成。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-217">Coercions to and from `String` are an exception to this rule and are only allowed on null values because `String` conversions are always done in the current culture of the execution environment at run time.</span></span> <span data-ttu-id="ba6f6-218">请注意，常量强制表达式只能使用内部转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-218">Note that constant coercion expressions can only ever use intrinsic conversions.</span></span>

* <span data-ttu-id="ba6f6-219">`+`、`-` 和 `Not` 一元运算符，前提是操作数和结果是上面列出的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-219">The `+`, `-` and `Not` unary operators, provided the operand and result is of a type listed above.</span></span>

* <span data-ttu-id="ba6f6-220">`+`、`-`、`*`、`^`、`Mod`、`/`、`\`、`<<`、`>>`、`&`、`And`、`Or`、`Xor`、`AndAlso`、`OrElse`、`=`、`<`、`>`、`<>`、`<=`和 `=>` 二进制运算符，则提供的每个操作数和结果均为上面列出的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-220">The `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, and `=>` binary operators, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="ba6f6-221">如果提供每个操作数，并且结果为上面列出的类型，则为条件运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-221">The conditional operator If, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="ba6f6-222">以下运行时函数： `Microsoft.VisualBasic.Strings.ChrW`;如果常量值介于0和128之间，则 `Microsoft.VisualBasic.Strings.Chr`;如果常量字符串不为空，则为 `Microsoft.VisualBasic.Strings.AscW`;如果常量字符串不为空，则 `Microsoft.VisualBasic.Strings.Asc`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-222">The following run-time functions: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` if the constant value is between 0 and 128; `Microsoft.VisualBasic.Strings.AscW` if the constant string is not empty; `Microsoft.VisualBasic.Strings.Asc` if the constant string is not empty.</span></span>

<span data-ttu-id="ba6f6-223">常量表达式中*不*允许使用以下构造：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-223">The following constructs are *not* permitted in constant expressions:</span></span>

* <span data-ttu-id="ba6f6-224">通过 `With` 上下文的隐式绑定。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-224">Implicit binding through a `With` context.</span></span>

<span data-ttu-id="ba6f6-225">整数类型的常量表达式（`ULong`、`Long`、`UInteger`、`Integer`、`UShort`、`Short`、`SByte`或 `Byte`）可隐式转换为较小的整数类型，并且如果常量表达式的值在目标类型的范围内，则可以将 `Double` 类型的常量表达式隐式转换为 `Single`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-225">Constant expressions of an integral type (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, or `Byte`) can be implicitly converted to a narrower integral type, and constant expressions of type `Double` can be implicitly converted to `Single`, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="ba6f6-226">无论使用的是许可语义还是严格语义，都可以使用这些收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-226">These narrowing conversions are allowed regardless of whether permissive or strict semantics are being used.</span></span>


## <a name="late-bound-expressions"></a><span data-ttu-id="ba6f6-227">后期绑定表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-227">Late-Bound Expressions</span></span>

<span data-ttu-id="ba6f6-228">当成员访问表达式或索引表达式的目标为类型 `Object`时，表达式的处理可能会延迟到运行时。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-228">When the target of a member access expression or index expression is of type `Object`, the processing of the expression may be deferred until run time.</span></span> <span data-ttu-id="ba6f6-229">以这种方式延迟处理称作*后期绑定*。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-229">Deferring processing this way is called *late binding*.</span></span> <span data-ttu-id="ba6f6-230">后期绑定*允许以无类型方式使用*`Object` 变量，其中成员的所有解析都基于变量中值的实际运行时类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-230">Late binding allows `Object` variables to be used in a *typeless* way, where all resolution of members is based on the actual run-time type of the value in the variable.</span></span> <span data-ttu-id="ba6f6-231">如果由编译环境或 `Option Strict`指定严格语义，后期绑定将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-231">If strict semantics are specified by the compilation environment or by `Option Strict`, late binding causes a compile-time error.</span></span> <span data-ttu-id="ba6f6-232">执行后期绑定时，将忽略非公共成员，包括用于重载决策的目的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-232">Non-public members are ignored when doing late-binding, including for the purposes of overload resolution.</span></span> <span data-ttu-id="ba6f6-233">请注意，与早期绑定的情况不同，调用或访问后期绑定的 `Shared` 成员将导致在运行时计算调用目标。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-233">Note that, unlike the early-bound case, invoking or accessing a `Shared` member late-bound will cause the invocation target to be evaluated at run time.</span></span><span data-ttu-id="ba6f6-234"> 如果表达式是 `System.Object`上定义的成员的调用表达式，则不会发生后期绑定。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-234"> If the expression is an invocation expression for a member defined on `System.Object`, late binding will not take place.</span></span>

<span data-ttu-id="ba6f6-235">通常，在运行时，通过查找表达式的实际运行时类型的标识符来解析后期绑定访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-235">In general, late-bound accesses are resolved at run time by looking up the identifier on the actual run-time type of the expression.</span></span> <span data-ttu-id="ba6f6-236">如果后期绑定成员查找在运行时失败，则会引发 `System.MissingMemberException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-236">If late-bound member lookup fails at run time, a `System.MissingMemberException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-237">由于后期绑定成员查找只是通过关联目标表达式的运行时类型来完成的，因此对象的运行时类型永远不是接口。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-237">Because late-bound member lookup is done solely off the run-time type of the associated target expression, an object's run-time type is never an interface.</span></span> <span data-ttu-id="ba6f6-238">因此，不可能访问后期绑定成员访问表达式中的接口成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-238">Therefore, it is impossible to access interface members in a late-bound member access expression.</span></span>

<span data-ttu-id="ba6f6-239">后期绑定成员访问的参数将按照它们在成员访问表达式中出现的顺序进行计算：不是在后期绑定成员中声明参数的顺序。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-239">The arguments to a late-bound member access are evaluated in the order they appear in the member access expression: not the order in which parameters are declared in the late-bound member.</span></span> <span data-ttu-id="ba6f6-240">下面的示例阐释了这种差异：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-240">The following example illustrates this difference:</span></span>

```vb
Class C
    Public Sub f(ByVal x As Integer, ByVal y As Integer)
    End Sub
End Class

Module Module1
    Sub Main()
        Console.Write("Early-bound: ")
        Dim c As C = New C
        c.f(y:=t("y"), x:=t("x"))

        Console.Write(vbCrLf & "Late-bound: ")
        Dim o As Object = New C
        o.f(y:=t("y"), x:=t("x"))
    End Sub

    Function t(ByVal s As String) As Integer
        Console.Write(s)
        Return 0
    End Function
End Module
```

<span data-ttu-id="ba6f6-241">此代码显示：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-241">This code displays:</span></span>

```console
Early-bound: xy
Late-bound: yx
```

<span data-ttu-id="ba6f6-242">由于后期绑定重载解析是在参数的运行时类型上完成的，因此可能会根据在编译时或运行时计算表达式的结果来生成不同的结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-242">Because late-bound overload resolution is done on the run-time type of the arguments, it is possible that an expression might produce different results based on whether it is evaluated at compile time or run time.</span></span> <span data-ttu-id="ba6f6-243">下面的示例阐释了这种差异：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-243">The following example illustrates this difference:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub F(b As Base)
        Console.WriteLine("F(Base)")
    End Sub

    Sub F(d As Derived)
        Console.WriteLine("F(Derived)")
    End Sub

    Sub Main()
        Dim b As Base = New Derived()
        Dim o As Object = b

        F(b)
        F(o)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-244">此代码显示：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-244">This code displays:</span></span>

```console
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a><span data-ttu-id="ba6f6-245">简单表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-245">Simple Expressions</span></span>

<span data-ttu-id="ba6f6-246">简单表达式包括文本、带括号的表达式、实例表达式或简单名称表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-246">Simple expressions are literals, parenthesized expressions, instance expressions, or simple name expressions.</span></span>

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a><span data-ttu-id="ba6f6-247">文本表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-247">Literal Expressions</span></span>

<span data-ttu-id="ba6f6-248">文本表达式的计算结果为文本所表示的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-248">Literal expressions evaluate to the value represented by the literal.</span></span> <span data-ttu-id="ba6f6-249">文本表达式归类为值（文本 `Nothing`除外，该文本归类为默认值）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-249">A literal expression is classified as a value, except for the literal `Nothing`, which is classified as a default value.</span></span>

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a><span data-ttu-id="ba6f6-250">带括号的表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-250">Parenthesized Expressions</span></span>

<span data-ttu-id="ba6f6-251">带括号的表达式包含用括号括起来的表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-251">A parenthesized expression consists of an expression enclosed in parentheses.</span></span> <span data-ttu-id="ba6f6-252">带括号的表达式归类为一个值，并且包含的表达式必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-252">A parenthesized expression is classified as a value, and the enclosed expression must be classified as a value.</span></span> <span data-ttu-id="ba6f6-253">带括号的表达式的计算结果为括号内的表达式的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-253">A parenthesized expression evaluates to the value of the expression within the parentheses.</span></span>

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a><span data-ttu-id="ba6f6-254">实例表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-254">Instance Expressions</span></span>

<span data-ttu-id="ba6f6-255">*实例表达式*是 `Me`的关键字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-255">An *instance expression* is the keyword `Me`.</span></span> <span data-ttu-id="ba6f6-256">它只能在非共享方法、构造函数或属性访问器的主体内使用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-256">It may only be used within the body of a non-shared method, constructor, or property accessor.</span></span> <span data-ttu-id="ba6f6-257">它被分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-257">It is classified as a value.</span></span> <span data-ttu-id="ba6f6-258">关键字 `Me` 表示包含所执行的方法或属性访问器的类型的实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-258">The keyword `Me` represents the instance of the type containing the method or property accessor being executed.</span></span> <span data-ttu-id="ba6f6-259">如果构造函数显式调用另一个构造函数（节[构造函数](type-members.md#constructors)），则在该构造函数调用之后，不能使用 `Me`，因为尚未构造该实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-259">If a constructor explicitly invokes another constructor (Section [Constructors](type-members.md#constructors)), `Me` cannot be used until after that constructor call, because the instance has not yet been constructed.</span></span>

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a><span data-ttu-id="ba6f6-260">简单名称表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-260">Simple Name Expressions</span></span>

<span data-ttu-id="ba6f6-261">*简单名称表达式*包含单个标识符，后跟可选的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-261">A *simple name expression* consists of a single identifier followed by an optional type argument list.</span></span>

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

<span data-ttu-id="ba6f6-262">名称由以下 "简单名称解析规则" 解析和分类：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-262">The name is resolved and classified by the following "simple name resolution rules":</span></span>

1.  <span data-ttu-id="ba6f6-263">从立即封闭的块开始，继续每个封闭的外部块（如果有），如果标识符与局部变量、静态变量、常量局部变量、方法类型参数或参数的名称匹配，则标识符引用匹配的实体。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-263">Starting with the immediately enclosing block and continuing with each enclosing outer block (if any), if the identifier matches the name of a local variable, static variable, constant local, method type parameter, or parameter, then the identifier refers to the matching entity.</span></span>

    <span data-ttu-id="ba6f6-264">如果标识符与局部变量、静态变量或常量局部变量匹配，并且提供了类型参数列表，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-264">If the identifier matches a local variable, static variable, or constant local and a type argument list was provided, a compile-time error occurs.</span></span> <span data-ttu-id="ba6f6-265">如果标识符与方法类型参数匹配并且提供了类型参数列表，则不会发生匹配且无法继续进行解析。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-265">If the identifier matches a method type parameter and a type argument list was provided, no match occurs and resolution continues.</span></span> <span data-ttu-id="ba6f6-266">如果标识符与局部变量匹配，则与之匹配的局部变量为隐式函数或 `Get` 访问器返回局部变量，并且该表达式是调用表达式、调用语句或 `AddressOf` 表达式的一部分，则不会发生匹配且无法继续进行解析。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-266">If the identifier matches a local variable, the local variable matched is the implicit function or `Get` accessor return local variable, and the expression is part of an invocation expression, invocation statement, or an `AddressOf` expression, then no match occurs and resolution continues.</span></span>

    <span data-ttu-id="ba6f6-267">如果表达式为本地变量、静态变量或参数，则该表达式归类为变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-267">The expression is classified as a variable if it is a local variable, static variable, or parameter.</span></span> <span data-ttu-id="ba6f6-268">如果表达式为方法类型参数，则该表达式归类为类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-268">The expression is classified as a type if it is a method type parameter.</span></span> <span data-ttu-id="ba6f6-269">如果表达式是常量，则该表达式归类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-269">The expression is classified as a value if it is a constant local.</span></span>

2.  <span data-ttu-id="ba6f6-270">对于包含表达式的每个嵌套类型，从最内层开始到最外层，如果对该类型中的标识符的查找产生与可访问成员的匹配：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-270">For each nested type containing the expression, starting from the innermost and going to the outermost, if a lookup of the identifier in the type produces a match with an accessible member:</span></span>

    21. <span data-ttu-id="ba6f6-271">如果匹配类型成员是一个类型参数，则结果归类为类型并且是匹配的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-271">If the matching type member is a type parameter, then the result is classified as a type and is the matching type parameter.</span></span> <span data-ttu-id="ba6f6-272">如果提供了类型参数列表，则不会发生匹配并且解决将继续。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-272">If a type argument list was provided, no match occurs and resolution continues.</span></span>
    22. <span data-ttu-id="ba6f6-273">否则，如果类型是直接封闭类型，并且查找标识了非共享类型成员，则结果与窗体 `Me.E(Of A)`的成员访问相同，其中 `E` 为标识符，`A` 为类型参数列表（如果有）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-273">Otherwise, if the type is the immediately enclosing type and the lookup identifies a non-shared type member, then the result is the same as a member access of the form `Me.E(Of A)`, where `E` is the identifier and `A` is the type argument list, if any.</span></span>
    23. <span data-ttu-id="ba6f6-274">否则，结果与窗体 `T.E(Of A)`的成员访问权限完全相同，其中 `T` 是包含匹配成员的类型，`E` 为标识符，`A` 为类型参数列表（如果有）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-274">Otherwise, the result is exactly the same as a member access of the form `T.E(Of A)`, where `T` is the type containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="ba6f6-275">在这种情况下，标识符引用非共享成员是错误的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-275">In this case, it is an error for the identifier to refer to a non-shared member.</span></span>

3.  <span data-ttu-id="ba6f6-276">对于每个嵌套命名空间，从最内层到最外面的命名空间，执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-276">For each nested namespace, starting from the innermost and going to the outermost namespace, do the following:</span></span>

    31. <span data-ttu-id="ba6f6-277">如果命名空间包含具有给定名称的可访问类型，并且具有与类型参数列表中提供的相同数量的类型参数（如果有），则标识符将引用该类型，并将其分类为类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-277">If the namespace contains an accessible type with the given name and has the same number of type parameters as was supplied in the type argument list, if any, then the identifier refers to that type and is classified as a type.</span></span>
    32. <span data-ttu-id="ba6f6-278">否则，如果未提供类型参数列表，并且命名空间包含具有给定名称的命名空间成员，则标识符将引用该命名空间，并归类为命名空间。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-278">Otherwise, if no type argument list was supplied and the namespace contains a namespace member with the given name, then the identifier refers to that namespace and is classified as a namespace.</span></span>
    33. <span data-ttu-id="ba6f6-279">否则，如果命名空间包含一个或多个可访问的标准模块，并且标识符的成员名称查找只在一个标准模块中生成一个可访问的匹配项，则结果与 `M.E(Of A)`形式的成员访问相同，其中 `M` 是包含匹配成员的标准模块，`E` 为标识符，`A` 为类型参数列表（如果有）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-279">Otherwise, if the namespace contains one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="ba6f6-280">如果标识符在多个标准模块中匹配可访问类型成员，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-280">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

4.  <span data-ttu-id="ba6f6-281">如果源文件有一个或多个导入别名，并且标识符与其中一个的名称匹配，则标识符将引用该命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-281">If the source file has one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="ba6f6-282">如果提供了类型参数列表，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-282">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="ba6f6-283">如果包含名称引用的源文件有一个或多个导入：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-283">If the source file containing the name reference has one or more imports:</span></span>

    51. <span data-ttu-id="ba6f6-284">如果标识符只匹配一个导入的可访问类型的名称，该类型的类型参数与在类型参数列表中提供的类型参数相同（如有）或类型成员，则标识符将引用该类型或类型成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-284">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="ba6f6-285">如果标识符中的标识符匹配，则多个导入的可访问类型的名称与类型参数列表中提供的类型参数相同（如果有）或可访问类型成员，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-285">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or an accessible type member, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="ba6f6-286">否则，如果未提供任何类型参数列表，并且标识符只匹配一个导入具有可访问类型的命名空间的名称，则标识符将引用该命名空间。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-286">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="ba6f6-287">如果未提供类型参数列表，并且标识符与多个导入中的标识符匹配，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-287">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="ba6f6-288">否则，如果导入包含一个或多个可访问的标准模块，并且标识符的成员名称查找只在一个标准模块中生成一个可访问的匹配项，则结果与 `M.E(Of A)`形式的成员访问相同，其中 `M` 是包含匹配成员的标准模块，`E` 为标识符，`A` 为类型参数列表（如果有）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-288">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="ba6f6-289">如果标识符在多个标准模块中匹配可访问类型成员，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-289">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

6.  <span data-ttu-id="ba6f6-290">如果编译环境定义了一个或多个导入别名，并且标识符与其中一个的名称匹配，则标识符将引用该命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-290">If the compilation environment defines one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="ba6f6-291">如果提供了类型参数列表，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-291">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="ba6f6-292">如果编译环境定义了一个或多个导入：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-292">If the compilation environment defines one or more imports:</span></span>

    71. <span data-ttu-id="ba6f6-293">如果标识符只匹配一个导入的可访问类型的名称，该类型的类型参数与在类型参数列表中提供的类型参数相同（如有）或类型成员，则标识符将引用该类型或类型成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-293">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="ba6f6-294">如果在多个导入中，标识符与类型参数列表中提供的类型参数（如果有）的名称相同，或类型成员发生编译时错误，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-294">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="ba6f6-295">否则，如果未提供任何类型参数列表，并且标识符只匹配一个导入具有可访问类型的命名空间的名称，则标识符将引用该命名空间。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-295">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="ba6f6-296">如果未提供类型参数列表，并且标识符与多个导入中的标识符匹配，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-296">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="ba6f6-297">否则，如果导入包含一个或多个可访问的标准模块，并且标识符的成员名称查找只在一个标准模块中生成一个可访问的匹配项，则结果与 `M.E(Of A)`形式的成员访问相同，其中 `M` 是包含匹配成员的标准模块，`E` 为标识符，`A` 为类型参数列表（如果有）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-297">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member,  `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="ba6f6-298">如果标识符在多个标准模块中匹配可访问类型成员，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-298">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="ba6f6-299">否则，标识符给定的名称是不确定的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-299">Otherwise, the name given by the identifier is undefined.</span></span>

<span data-ttu-id="ba6f6-300">未定义的简单名称表达式是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-300">A simple name expression that is undefined is a compile-time error.</span></span>

<span data-ttu-id="ba6f6-301">通常，名称只能在特定命名空间中出现一次。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-301">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="ba6f6-302">但是，由于命名空间可以在多个 .NET 程序集中进行声明，因此在两个程序集定义具有相同完全限定名称的类型时，可能会出现这种情况。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-302">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="ba6f6-303">在这种情况下，当前源文件集中声明的类型优先于在外部 .NET 程序集中声明的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-303">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="ba6f6-304">否则，名称是不明确的，没有办法消除名称的歧义。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-304">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>


### <a name="addressof-expressions"></a><span data-ttu-id="ba6f6-305">AddressOf 表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-305">AddressOf Expressions</span></span>

<span data-ttu-id="ba6f6-306">`AddressOf` 表达式用于生成方法指针。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-306">An `AddressOf` expression is used to produce a method pointer.</span></span> <span data-ttu-id="ba6f6-307">表达式包含 `AddressOf` 关键字和必须归类为方法组或后期绑定访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-307">The expression consists of the `AddressOf` keyword and an expression that must be classified as a method group or a late-bound access.</span></span> <span data-ttu-id="ba6f6-308">方法组无法引用构造函数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-308">The method group cannot refer to constructors.</span></span>

<span data-ttu-id="ba6f6-309">结果归类为方法指针，其关联的目标表达式和类型参数列表（如果有）将作为方法组。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-309">The result is classified as a method pointer, with the same associated target expression and type argument list (if any) as the method group.</span></span>

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a><span data-ttu-id="ba6f6-310">类型表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-310">Type Expressions</span></span>

<span data-ttu-id="ba6f6-311">*类型表达式*是 `GetType` 表达式、`TypeOf...Is` 表达式、`Is` 表达式或 `GetXmlNamespace` 表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-311">A *type expression* is a `GetType` expression, a `TypeOf...Is` expression, an `Is` expression, or a `GetXmlNamespace` expression.</span></span>

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a><span data-ttu-id="ba6f6-312">GetType 表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-312">GetType Expressions</span></span>

<span data-ttu-id="ba6f6-313">`GetType` 表达式由关键字 `GetType` 和类型名称组成。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-313">A `GetType` expression consists of the keyword `GetType` and the name of a type.</span></span>

```antlr
GetTypeExpression
    : 'GetType' OpenParenthesis GetTypeTypeName CloseParenthesis
    ;

GetTypeTypeName
    : TypeName
    | QualifiedOpenTypeName
    ;

QualifiedOpenTypeName
    : Identifier TypeArityList? (Period IdentifierOrKeyword TypeArityList?)*
    | 'Global' Period IdentifierOrKeyword TypeArityList?
      (Period IdentifierOrKeyword TypeArityList?)*
    ;

TypeArityList
    : OpenParenthesis 'Of' CommaList? CloseParenthesis
    ;

CommaList
    : Comma Comma*
    ;
```

<span data-ttu-id="ba6f6-314">`GetType` 表达式归类为一个值，其值为表示其*GetTypeTypeName*的反射（`System.Type`）类。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-314">A `GetType` expression is classified as a value, and its value is the reflection (`System.Type`) class that represents its *GetTypeTypeName*.</span></span> <span data-ttu-id="ba6f6-315">如果*GetTypeTypeName*是一个类型参数，则表达式将返回与在运行时为类型参数提供的类型参数相对应的 `System.Type` 对象。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-315">If the *GetTypeTypeName* is a type parameter, the expression will return the `System.Type` object that corresponds to the type argument supplied for the type parameter at run-time.</span></span>

<span data-ttu-id="ba6f6-316">*GetTypeTypeName*是一种特殊方式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-316">The *GetTypeTypeName* is special in two ways:</span></span>

* <span data-ttu-id="ba6f6-317">允许 `System.Void`，这是语言中可引用此类型名称的唯一地方。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-317">It is allowed to be `System.Void`, the only place in the language where this type name may be referenced.</span></span>

* <span data-ttu-id="ba6f6-318">它可能是省略了类型参数的构造泛型类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-318">It may be a constructed generic type with the type arguments omitted.</span></span> <span data-ttu-id="ba6f6-319">这允许 `GetType` 表达式返回对应于泛型类型本身的 `System.Type` 对象。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-319">This allows the `GetType` expression to return the `System.Type` object that corresponds to the generic type itself.</span></span>

<span data-ttu-id="ba6f6-320">下面的示例演示 `GetType` 表达式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-320">The following example demonstrates the `GetType` expression:</span></span>

```vb
Module Test
    Sub Main()
        Dim t As Type() = { GetType(Integer), GetType(System.Int32), _
            GetType(String), GetType(Double()) }
        Dim i As Integer

        For i = 0 To t.Length - 1
            Console.WriteLine(t(i).Name)
        Next i
    End Sub
End Module
```

<span data-ttu-id="ba6f6-321">生成的输出为：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-321">The resulting output is:</span></span>

```console
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a><span data-ttu-id="ba6f6-322">TypeOf .。。为表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-322">TypeOf...Is Expressions</span></span>

<span data-ttu-id="ba6f6-323">`TypeOf...Is` 表达式用于检查值的运行时类型与给定类型是否兼容。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-323">A `TypeOf...Is` expression is used to check whether the run-time type of a value is compatible with a given type.</span></span> <span data-ttu-id="ba6f6-324">第一个操作数必须分类为一个值，不能是已重新分类的 lambda 方法，并且必须是引用类型或不受约束的类型参数类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-324">The first operand must be classified as a value, cannot be a reclassified lambda method, and must be of a reference type or an unconstrained type parameter type.</span></span> <span data-ttu-id="ba6f6-325">第二个操作数必须是类型名称。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-325">The second operand must be a type name.</span></span> <span data-ttu-id="ba6f6-326">表达式的结果归类为值，是 `Boolean` 值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-326">The result of the expression is classified as a value and is a `Boolean` value.</span></span> <span data-ttu-id="ba6f6-327">表达式的计算结果为 `True` 如果操作数的运行时类型具有到类型的标识、默认值、引用、数组、值类型或类型参数转换，则为 `False` 否则为。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-327">The expression evaluates to `True` if the run-time type of the operand has an identity, default, reference, array, value type, or type parameter conversion to the type, `False` otherwise.</span></span> <span data-ttu-id="ba6f6-328">如果表达式的类型与特定类型之间不存在转换，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-328">A compile-time error occurs if no conversion exists between the type of the expression and the specific type.</span></span>

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a><span data-ttu-id="ba6f6-329">为表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-329">Is Expressions</span></span>

<span data-ttu-id="ba6f6-330">`Is` 或 `IsNot` 表达式用于执行引用相等性比较。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-330">An `Is` or `IsNot` expression is used to do a reference equality comparison.</span></span>

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-331">每个表达式都必须分类为一个值，并且每个表达式的类型必须是引用类型、不受约束的类型参数类型或可以为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-331">Each expression must be classified as a value and the type of each expression must be a reference type, an unconstrained type parameter type, or a nullable value type.</span></span> <span data-ttu-id="ba6f6-332">但是，如果一个表达式的类型是不受约束的类型参数类型或可以为 null 的值类型，则另一个表达式必须是文本 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-332">If the type of one expression is an unconstrained type parameter type or nullable value type, however, the other expression must be the literal `Nothing`.</span></span>

<span data-ttu-id="ba6f6-333">结果归类为一个值，并键入为 `Boolean`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-333">The result is classified as a value and is typed as `Boolean`.</span></span> <span data-ttu-id="ba6f6-334">如果两个值都引用同一个实例，或者这两个值都 `Nothing`，则 `Is` 运算的计算结果为 `True`; 否则为 `False`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-334">An `Is` operation evaluates to `True` if both values refer to the same instance or both values are `Nothing`, or `False` otherwise.</span></span> <span data-ttu-id="ba6f6-335">如果两个值都引用同一个实例，或者这两个值都 `Nothing`，则 `IsNot` 运算的计算结果为 `False`; 否则为 `True`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-335">An `IsNot` operation evaluates to `False` if both values refer to the same instance or both values are `Nothing`, or `True` otherwise.</span></span>


### <a name="getxmlnamespace-expressions"></a><span data-ttu-id="ba6f6-336">GetXmlNamespace 表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-336">GetXmlNamespace Expressions</span></span>

<span data-ttu-id="ba6f6-337">`GetXmlNamespace` 表达式包含关键字 `GetXmlNamespace`，以及源文件或编译环境所声明的 XML 命名空间的名称。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-337">A `GetXmlNamespace` expression consists of the keyword `GetXmlNamespace` and the name of an XML namespace declared by the source file or compilation environment.</span></span>

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

<span data-ttu-id="ba6f6-338">`GetXmlNamespace` 表达式归类为一个值，其值为表示*XMLNamespaceName*的 `System.Xml.Linq.XNamespace` 的实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-338">An `GetXmlNamespace` expression is classified as a value, and its value is an instance of `System.Xml.Linq.XNamespace` that represents the *XMLNamespaceName*.</span></span> <span data-ttu-id="ba6f6-339">如果该类型不可用，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-339">If that type is not available, then a compile-time error will occur.</span></span>

<span data-ttu-id="ba6f6-340">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-340">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim db = GetXmlNamespace(db)

        ' The following are equivalent
        Dim customer1 = _
            New System.Xml.Linq.XElement(db + "customer", "Bob")
        Dim customer2 = <db:customer>Bob</>
    End Sub
End Module
```

<span data-ttu-id="ba6f6-341">括号之间的所有内容都被视为命名空间名称的一部分，因此，诸如空格之类的内容都适用于 XML 规则。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-341">Everything between the parentheses is considered part of the namespace name, so XML rules around things such as whitespace apply.</span></span> <span data-ttu-id="ba6f6-342">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-342">For example:</span></span>

```vb
Imports <xmlns:db-ns="http://example.org/database">

Module Test
    Sub Main()

        ' Error, XML name expected
        Dim db1 = GetXmlNamespace( db-ns )

        ' Error, ')' expected
        Dim db2 = GetXmlNamespace(db _
            )

        ' OK.
        Dim db3 = GetXmlNamespace(db-ns)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-343">还可以省略 XML 命名空间表达式，在这种情况下，表达式将返回表示默认 XML 命名空间的对象。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-343">The XML namespace expression can also be omitted, in which case the expression returns the object that represents the default XML namespace.</span></span>


## <a name="member-access-expressions"></a><span data-ttu-id="ba6f6-344">成员访问表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-344">Member Access Expressions</span></span>

<span data-ttu-id="ba6f6-345">成员访问表达式用于访问实体的成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-345">A member access expression is used to access a member of an entity.</span></span>

```antlr
MemberAccessExpression
    : MemberAccessBase? Period IdentifierOrKeyword
      ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;

MemberAccessBase
    : Expression
    | NonArrayTypeName
    | 'Global'
    | 'MyClass'
    | 'MyBase'
    ;
```

<span data-ttu-id="ba6f6-346">格式 `E.I(Of A)`的成员访问，其中 `E` 为表达式、非数组类型名称、关键字 `Global`或省略，并且 `I` 是具有可选类型参数列表 `A`的标识符，按如下方式进行计算和分类：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-346">A member access of the form `E.I(Of A)`, where `E` is an expression, a non-array type name, the keyword `Global`, or omitted and `I` is an identifier with an optional type argument list `A`, is evaluated and classified as follows:</span></span>

1. <span data-ttu-id="ba6f6-347">如果省略 `E`，则直接包含 `With` 语句的表达式将被替换为 `E`，并执行成员访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-347">If `E` is omitted, then the expression from the immediately containing `With` statement is substituted for `E` and the member access is performed.</span></span> <span data-ttu-id="ba6f6-348">如果没有包含 `With` 语句，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-348">If there is no containing `With` statement, a compile-time error occurs.</span></span>

2. <span data-ttu-id="ba6f6-349">如果 `E` 归类为命名空间或 `E` 为关键字 `Global`，则成员查找在指定命名空间的上下文中完成。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-349">If `E` is classified as a namespace or `E` is the keyword `Global`, then the member lookup is done in the context of the specified namespace.</span></span> <span data-ttu-id="ba6f6-350">如果 `I` 是该命名空间中具有与在类型参数列表中提供的相同数量的类型参数（如果有）的可访问成员的名称，则结果为该成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-350">If `I` is the name of an accessible member of that namespace with the same number of type parameters as was supplied in the type argument list, if any, then the result is that member.</span></span> <span data-ttu-id="ba6f6-351">结果归类为命名空间或类型，具体取决于成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-351">The result is classified as a namespace or a type depending on the member.</span></span> <span data-ttu-id="ba6f6-352">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-352">Otherwise, a compile-time error occurs.</span></span>

3. <span data-ttu-id="ba6f6-353">如果 `E` 是类型或归类为类型的表达式，则成员查找在指定类型的上下文中完成。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-353">If `E` is a type or an expression classified as a type, then the member lookup is done in the context of the specified type.</span></span> <span data-ttu-id="ba6f6-354">如果 `I` 是 `E`的辅助性成员的名称，则按如下方式计算 `E.I`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-354">If `I` is the name of an accessible member of `E`, then `E.I` is evaluated and classified as follows:</span></span>

    31. <span data-ttu-id="ba6f6-355">如果 `I` 是关键字 `New` 并且 `E` 不是枚举，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-355">If `I` is the keyword `New` and `E` is not an enumeration then a compile-time error occurs.</span></span>
    32. <span data-ttu-id="ba6f6-356">如果 `I` 标识与类型参数列表中提供的类型参数数目相同的类型（如果有），则结果为该类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-356">If `I` identifies a type with the same number of type parameters as was supplied in the type argument list, if any, then the result is that type.</span></span>
    33. <span data-ttu-id="ba6f6-357">如果 `I` 标识一个或多个方法，则结果将是具有关联的类型自变量列表并且没有关联的目标表达式的方法组。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-357">If `I` identifies one or more methods, then the result is a method group with the associated type argument list and no associated target expression.</span></span>
    34. <span data-ttu-id="ba6f6-358">如果 `I` 标识一个或多个属性，但未提供任何类型实参列表，则结果是没有关联目标表达式的属性组。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-358">If `I` identifies one or more properties and no type argument list was supplied, then the result is a property group with no associated target expression.</span></span>
    35. <span data-ttu-id="ba6f6-359">如果 `I` 标识一个共享变量且未提供任何类型实参列表，则结果为变量或值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-359">If `I` identifies a shared variable and no type argument list was supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="ba6f6-360">如果变量是只读的，并且引用出现在声明变量的类型的共享构造函数之外，则结果为 `E`中 `I` 共享变量的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-360">If the variable is read-only, and the reference occurs outside the shared constructor of the type in which the variable is declared, then the result is the value of the shared variable `I` in `E`.</span></span> <span data-ttu-id="ba6f6-361">否则，结果为 `E`中 `I` 共享变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-361">Otherwise, the result is the shared variable `I` in `E`.</span></span>
    36. <span data-ttu-id="ba6f6-362">如果 `I` 标识了共享事件，但未提供任何类型参数列表，则结果是没有关联目标表达式的事件访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-362">If `I` identifies a shared event and no type argument list was supplied, the result is an event access with no associated target expression.</span></span>
    37. <span data-ttu-id="ba6f6-363">如果 `I` 标识一个常数且未提供任何类型参数列表，则结果为该常量的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-363">If `I` identifies a constant and no type argument list was supplied, then the result is the value of that constant.</span></span>
    38. <span data-ttu-id="ba6f6-364">如果 `I` 标识枚举成员但未提供类型参数列表，则结果是该枚举成员的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-364">If `I` identifies an enumeration member and no type argument list was supplied, then the result is the value of that enumeration member.</span></span>
    39. <span data-ttu-id="ba6f6-365">否则，`E.I` 是无效的成员引用，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-365">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

4. <span data-ttu-id="ba6f6-366">如果 `E` 归类为变量或值，则 `T`的类型，则成员查找在 `T`的上下文中完成。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-366">If `E` is classified as a variable or value, the type of which is `T`, then the member lookup is done in the context of `T`.</span></span> <span data-ttu-id="ba6f6-367">如果 `I` 是 `T`的辅助性成员的名称，则按如下方式计算 `E.I`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-367">If `I` is the name of an accessible member of `T`, then `E.I` is evaluated and classified as follows:</span></span>

    41. <span data-ttu-id="ba6f6-368">如果 `I` 是关键字 `New`，则 `E` 为 `Me`、`MyBase`或 `MyClass`，且未提供任何类型参数，则结果为一个方法组，该方法组表示 `E` 的关联目标表达式为 `E` 且无类型参数列表的类型的实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-368">If `I` is the keyword `New`, `E` is  `Me`, `MyBase`, or `MyClass`, and no type arguments were supplied, then the result is a method group representing the instance constructors of the type of `E` with an associated target expression of `E` and no type argument list.</span></span> <span data-ttu-id="ba6f6-369">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-369">Otherwise, a compile-time error occurs.</span></span>
    42. <span data-ttu-id="ba6f6-370">如果 `I` 标识一种或多种方法，包括 `T` 不 `Object`的扩展方法，则结果将是具有关联的类型自变量列表和 `E`的关联目标表达式的方法组。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-370">If `I` identifies one or more methods, including extension methods if `T` is not `Object`, then the result is a method group with the associated type argument list and an associated target expression of `E`.</span></span>
    43. <span data-ttu-id="ba6f6-371">如果 `I` 标识一个或多个属性，但未提供任何类型实参，则结果为具有 `E`的关联目标表达式的属性组。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-371">If `I` identifies one or more properties and no type arguments were supplied, then the result is a property group with an associated target expression of `E`.</span></span>
    44. <span data-ttu-id="ba6f6-372">如果 `I` 标识共享变量或实例变量，但未提供任何类型实参，则结果为变量或值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-372">If `I` identifies a shared variable or an instance variable and no type arguments were supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="ba6f6-373">如果该变量是只读的，并且引用出现在类的构造函数之外，而该构造函数将变量声明为适用于变量（shared 或 instance）的类型，则结果是 `E`引用的对象中 `I` 变量的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-373">If the variable is read-only, and the reference occurs outside a constructor of the class in which the variable is declared appropriate for the kind of variable (shared or instance), then the result is the value of the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="ba6f6-374">如果 `T` 是引用类型，则结果是 `E`引用的对象 `I` 的变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-374">If `T` is a reference type, then the result is the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="ba6f6-375">否则，如果 `T` 是值类型，并且表达式 `E` 归类为变量，则结果为变量;否则，结果为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-375">Otherwise, if `T` is a value type and the expression `E` is classified as a variable, the result is a variable; otherwise the result is a value.</span></span>
    45. <span data-ttu-id="ba6f6-376">如果 `I` 标识事件但未提供任何类型实参，则结果为具有 `E`的关联目标表达式的事件访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-376">If `I` identifies an event and no type arguments were supplied, the result is an event access with an associated target expression of `E`.</span></span>
    46. <span data-ttu-id="ba6f6-377">如果 `I` 标识一个常数且未提供任何类型参数，则结果为该常量的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-377">If `I` identifies a constant and no type arguments were supplied, then the result is the value of that constant.</span></span>
    47. <span data-ttu-id="ba6f6-378">如果 `I` 标识枚举成员但未提供任何类型参数，则结果是该枚举成员的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-378">If `I` identifies an enumeration member and no type arguments were supplied, then the result is the value of that enumeration member.</span></span>
    48. <span data-ttu-id="ba6f6-379">如果 `Object``T`，则结果是一个后期绑定成员查找，该查找归类为具有关联类型参数列表和关联目标表达式的 `E`的后期绑定访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-379">If `T` is `Object`, then the result is a late-bound member lookup classified as a late-bound access with the associated type argument list and an associated target expression of `E`.</span></span>

5. <span data-ttu-id="ba6f6-380">否则，`E.I` 是无效的成员引用，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-380">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

<span data-ttu-id="ba6f6-381">格式 `MyClass.I(Of A)` 的成员访问等效于 `Me.I(Of A)`，但在其上访问的所有成员被视为不可重写的成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-381">A member access of the form `MyClass.I(Of A)` is equivalent to `Me.I(Of A)`, but all members accessed on it are treated as if the members are non-overridable.</span></span> <span data-ttu-id="ba6f6-382">因此，访问的成员将不受访问其成员的值的运行时类型的影响。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-382">Thus, the member accessed will not be affected by the run-time type of the value on which the member is being accessed.</span></span>

<span data-ttu-id="ba6f6-383">格式 `MyBase.I(Of A)` 的成员访问等效于 `CType(Me, T).I(Of A)` 其中 `T` 是包含成员访问表达式的类型的直接基类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-383">A member access of the form `MyBase.I(Of A)` is equivalent to `CType(Me, T).I(Of A)` where `T` is the direct base type of the type containing the member access expression.</span></span> <span data-ttu-id="ba6f6-384">其上的所有方法调用都被视为不可重写的方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-384">All method invocations on it are treated as if the method being invoked is non-overridable.</span></span> <span data-ttu-id="ba6f6-385">这种形式的成员访问也称为*基本访问*。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-385">This form of member access is also called a *base access*.</span></span>

<span data-ttu-id="ba6f6-386">下面的示例演示如何 `Me`、`MyBase` 和 `MyClass` 相关：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-386">The following example demonstrates how `Me`, `MyBase` and `MyClass` relate:</span></span>

```vb
Class Base
    Public Overridable Sub F()
        Console.WriteLine("Base.F")
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overrides Sub F()
        Console.WriteLine("Derived.F")
    End Sub

    Public Sub G()
        MyClass.F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F()
        Console.WriteLine("MoreDerived.F")
    End Sub

    Public Sub H()
        MyBase.F()
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As MoreDerived = new MoreDerived()

        x.F()
        x.G()
        x.H()
    End Sub

End Module
```

<span data-ttu-id="ba6f6-387">此代码输出：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-387">This code prints out:</span></span>

```console
MoreDerived.F
Derived.F
Derived.F
```

<span data-ttu-id="ba6f6-388">如果成员访问表达式以关键字 `Global`开头，则关键字表示最外面的未命名命名空间，这在声明隐藏封闭命名空间的情况下非常有用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-388">When a member access expression begins with the keyword `Global`, the keyword represents the outermost unnamed namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="ba6f6-389">`Global` 关键字允许在这种情况下将 "转义" 到最外面的命名空间。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-389">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="ba6f6-390">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-390">For example:</span></span>

```vb
Class System
End Class

Module Test
    Sub Main()
        ' Error: Class System does not contain Console
        System.Console.WriteLine("Hello, world!") 


        ' Legal, binds to System in outermost namespace
        Global.System.Console.WriteLine("Hello, world!") 
    End Sub
End Module
```

<span data-ttu-id="ba6f6-391">在上面的示例中，第一个方法调用无效，因为标识符 `System` 绑定到类 `System`，而不是命名空间 `System`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-391">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="ba6f6-392">访问 `System` 命名空间的唯一方法是使用 `Global` 来转义外部命名空间。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-392">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span>

<span data-ttu-id="ba6f6-393">如果要访问的成员是共享的，则该期间左侧的任何表达式都是多余的，除非成员访问是后期绑定的，否则不会对其进行计算。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-393">If the member being accessed is shared, any expression on the left side of the period is superfluous and is not evaluated unless the member access is done late-bound.</span></span> <span data-ttu-id="ba6f6-394">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-394">For example, consider the following code:</span></span>

```vb
Class C
    Public Shared F As Integer = 10
End Class

Module Test
    Public Function ReturnC() As C
        Console.WriteLine("Returning a new instance of C.")
        Return New C()
    End Function

    Public Sub Main()
        Console.WriteLine("The value of F is: " & ReturnC().F)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-395">它将打印 `The value of F is: 10` 因为不需要调用函数 `ReturnC` 来提供 `C` 的实例来访问共享成员 `F`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-395">It prints `The value of F is: 10` because the function `ReturnC` does not need to be called to provide an instance of `C` to access the shared member `F`.</span></span>


### <a name="identical-type-and-member-names"></a><span data-ttu-id="ba6f6-396">相同的类型和成员名称</span><span class="sxs-lookup"><span data-stu-id="ba6f6-396">Identical Type and Member Names</span></span>

<span data-ttu-id="ba6f6-397">使用与类型相同的名称命名成员并不少见。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-397">It is not uncommon to name members using the same name as their type.</span></span> <span data-ttu-id="ba6f6-398">但是，在这种情况下，可能会发生名称隐藏：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-398">In that situation, however, inconvenient name hiding can occur:</span></span>

```vb
Enum Color
    Red
    Green
    Yellow
End Enum

Class Test
    ReadOnly Property Color() As Color
        Get
            Return Color.Red
        End Get
    End Property

    Shared Function DefaultColor() As Color
        Return Color.Green    ' Binds to the instance property!
    End Function
End Class
```

<span data-ttu-id="ba6f6-399">在前面的示例中，`DefaultColor` 中的简单名称 `Color` 绑定到实例属性，而不是类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-399">In the previous example, the simple name `Color` in `DefaultColor` binds to the instance property instead of the type.</span></span> <span data-ttu-id="ba6f6-400">由于在共享成员中无法引用实例成员，这通常是一个错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-400">Because an instance member cannot be referenced in a shared member, this would normally be an error.</span></span>

<span data-ttu-id="ba6f6-401">但是，在这种情况下，特殊规则允许访问类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-401">However, a special rule allows access to the type in this case.</span></span> <span data-ttu-id="ba6f6-402">如果成员访问表达式的基本表达式为简单名称，并绑定到类型具有相同名称的常量、字段、属性、局部变量或参数，则基表达式可以引用成员或类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-402">If the base expression of a member access expression is a simple name and binds to a constant, field, property, local variable or parameter whose type has the same name, then the base expression can refer either to the member or the type.</span></span> <span data-ttu-id="ba6f6-403">这绝不会导致歧义，因为可以从任一成员访问的成员都是相同的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-403">This can never result in ambiguity because the members that can be accessed off of either one are the same.</span></span>

### <a name="default-instances"></a><span data-ttu-id="ba6f6-404">默认实例</span><span class="sxs-lookup"><span data-stu-id="ba6f6-404">Default Instances</span></span>

<span data-ttu-id="ba6f6-405">在某些情况下，从公共基类派生的类通常或始终只有一个实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-405">In some situations, classes derived from a common base class usually or always have only a single instance.</span></span> <span data-ttu-id="ba6f6-406">例如，用户界面中显示的大多数窗口在任何时候都只有一个实例显示在屏幕上。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-406">For example, most windows shown in a user interface only ever have one instance showing on the screen at any time.</span></span> <span data-ttu-id="ba6f6-407">为了简化此类类型的使用，Visual Basic 可以自动生成类的*默认实例*，这些类为每个类提供一个易于引用的实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-407">To simplify working with these types of classes, Visual Basic can automatically generate *default instances* of the classes that provide a single, easily referenced instance for each class.</span></span>

<span data-ttu-id="ba6f6-408">始终为类型*系列*而不是一个特定类型创建默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-408">Default instances are always created for a *family* of types rather than for one particular type.</span></span> <span data-ttu-id="ba6f6-409">因此，为从窗体派生的所有类创建默认实例，而不是为从窗体派生的类 Form1 创建默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-409">So instead of creating a default instance for a class Form1 that derives from Form, default instances are created for all classes derived from Form.</span></span> <span data-ttu-id="ba6f6-410">这意味着，派生自基类的每个单独类不必专门标记为具有默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-410">This means that each individual class that derives from the base class does not have to be specially marked to have a default instance.</span></span>

<span data-ttu-id="ba6f6-411">类的默认实例由编译器生成的属性表示，该属性返回该类的默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-411">The default instance of a class is represented by a compiler-generated property that returns the default instance of that class.</span></span> <span data-ttu-id="ba6f6-412">作为类的成员生成的属性称为*组类*，它管理从特定基类派生的所有类的默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-412">The property generated as a member of a class called the *group class* that manages allocating and destroying default instances for all classes derived from the particular base class.</span></span> <span data-ttu-id="ba6f6-413">例如，可以在 `MyForms` 类中收集从 `Form` 派生的类的所有默认实例属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-413">For example, all of the default instance properties of classes derived from `Form` may be collected in the `MyForms` class.</span></span> <span data-ttu-id="ba6f6-414">如果 `My.Forms`的表达式返回 group 类的实例，则以下代码将访问派生类的默认实例 `Form1` 和 `Form2`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-414">If an instance of the group class is returned by the expression `My.Forms`, then the following code accesses the default instances of derived classes `Form1` and `Form2`:</span></span>

```vb
Class Form1
    Inherits Form
    Public x As Integer
End Class

Class Form2
    Inherits Form
    Public y As Integer
End Class

Module Main
    Sub Main()
        My.Forms.Form1.x = 10
        Console.WriteLine(My.Forms.Form2.y)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-415">在第一次引用之前，将不会创建默认实例;如果提取表示默认实例的属性，则会导致创建默认实例（如果尚未创建）或将其设置为 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-415">Default instances will not be created until the first reference to them; fetching the property representing the default instance causes the default instance to be created if it has not already been created or has been set to `Nothing`.</span></span> <span data-ttu-id="ba6f6-416">若要测试是否存在默认实例，当默认实例是 `Is` 或 `IsNot` 操作员的目标时，将不会创建默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-416">To allow testing for the existence of a default instance, when a default instance is the target of an `Is` or `IsNot` operator, the default instance will not be created.</span></span> <span data-ttu-id="ba6f6-417">因此，可以测试默认实例是否 `Nothing` 或某个其他引用，而不会导致创建默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-417">Thus, it is possible to test whether a default instance is `Nothing` or some other reference without causing the default instance to be created.</span></span>

<span data-ttu-id="ba6f6-418">默认实例旨在使从具有默认实例的类的外部引用默认实例变得更加容易。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-418">Default instances are intended to make it easy to refer to the default instance from outside of the class that has the default instance.</span></span> <span data-ttu-id="ba6f6-419">如果使用中的默认实例，则定义该实例可能会导致对哪个实例的引用（即默认实例或当前实例）造成混淆。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-419">Using a default instance from within a class that defines it might cause confusion as to which instance is being referred to, i.e. the default instance or the current instance.</span></span> <span data-ttu-id="ba6f6-420">例如，下面的代码仅修改默认实例中 `x` 的值，即使正在从另一个实例调用它。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-420">For example, the following code modifies only the value `x` in the default instance, even though it is being called from another instance.</span></span> <span data-ttu-id="ba6f6-421">这样，代码就 `5` 而不是 `10`来打印值：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-421">Thus the code would print the value `5` instead of `10`:</span></span>

```vb
Class Form1
    Inherits Form

    Public x As Integer = 5

    Public Sub ChangeX()
        Form1.x = 10
    End Sub
End Class

Module Main
    Sub Main()
        Dim f As Form1 = New Form1()
        f.ChangeX()
        Console.WriteLine(f.x)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-422">若要避免这种混淆，从默认实例的类型的实例方法中引用默认实例是无效的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-422">To prevent this kind of confusion, it is not valid to refer to a default instance from within an instance method of the default instance's type.</span></span>

#### <a name="default-instances-and-type-names"></a><span data-ttu-id="ba6f6-423">默认实例和类型名称</span><span class="sxs-lookup"><span data-stu-id="ba6f6-423">Default Instances and Type Names</span></span>

<span data-ttu-id="ba6f6-424">还可以通过类型的名称直接访问默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-424">A default instance may also be accessible directly through its type's name.</span></span> <span data-ttu-id="ba6f6-425">在这种情况下，在不允许使用类型名称的任何表达式上下文中，表达式 `E`，其中 `E` 表示具有默认实例的类的完全限定名称，则更改为 `E'`，其中 `E'` 表示获取默认实例属性的表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-425">In this case, in any expression context where the type name is not allowed the expression `E`, where `E` represents the fully qualified name of the class with a default instance, is changed to `E'`, where `E'` represents an expression that fetches the default instance property.</span></span> <span data-ttu-id="ba6f6-426">例如，如果从 `Form` 派生的类的默认实例允许通过类型名称访问默认实例，则以下代码等效于上一示例中的代码：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-426">For example, if default instances for classes derived from `Form` allow accessing the default instance through the type name, then the following code is equivalent to the code in the previous example:</span></span>

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-427">这也意味着，通过类型名称访问的默认实例也可通过类型名称进行分配。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-427">This also means that a default instance that is accessible through its type's name is also assignable through the type name.</span></span> <span data-ttu-id="ba6f6-428">例如，下面的代码将 `Form1` 的默认实例设置为 `Nothing`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-428">For example, the following code sets the default instance of `Form1` to `Nothing`:</span></span>

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

<span data-ttu-id="ba6f6-429">请注意，`E.I` `E` 表示类，而 `I` 表示共享成员不会更改。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-429">Note that the meaning of `E.I` were `E` represents a class and `I` represents a shared member does not change.</span></span> <span data-ttu-id="ba6f6-430">此类表达式仍直接从类实例访问共享成员，并且不引用默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-430">Such an expression still accesses the shared member directly off of the class instance and does not reference the default instance.</span></span>

#### <a name="group-classes"></a><span data-ttu-id="ba6f6-431">组类</span><span class="sxs-lookup"><span data-stu-id="ba6f6-431">Group Classes</span></span>

<span data-ttu-id="ba6f6-432">`Microsoft.VisualBasic.MyGroupCollectionAttribute` 特性指示默认实例系列的组类。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-432">The `Microsoft.VisualBasic.MyGroupCollectionAttribute` attribute indicates the group class for a family of default instances.</span></span> <span data-ttu-id="ba6f6-433">该属性具有四个参数：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-433">The attribute has four parameters:</span></span>

* <span data-ttu-id="ba6f6-434">参数 `TypeToCollect` 指定组的基类。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-434">The parameter `TypeToCollect` specifies the base class for the group.</span></span> <span data-ttu-id="ba6f6-435">所有不含开放类型参数的开放类型参数均派生自具有此名称的类型（不考虑类型参数）将自动具有默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-435">All instantiable classes without open type parameters that derive from a type with this name (regardless of type parameters) will automatically have a default instance.</span></span>

* <span data-ttu-id="ba6f6-436">参数 `CreateInstanceMethodName` 指定要在组类中调用的方法，以便在默认实例属性中创建新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-436">The parameter `CreateInstanceMethodName` specifies the method to call in the group class to create a new instance in a default instance property.</span></span>

* <span data-ttu-id="ba6f6-437">参数 `DisposeInstanceMethodName` 指定要在组类中调用的方法，以释放默认实例属性（如果为默认实例属性分配了值 `Nothing`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-437">The parameter `DisposeInstanceMethodName` specifies the method to call in the group class to dispose of a default instance property if the default instance property is assigned the value `Nothing`.</span></span>

* <span data-ttu-id="ba6f6-438">参数 `DefaultInstanceAlias` 详细说明表达式的详细信息 `E'` 如果可通过其类型名称直接访问默认实例，则替换类名称。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-438">The parameter `DefaultInstanceAlias` specifics the expression `E'` to substitute for the class name if the default instances are accessible directly through their type name.</span></span> <span data-ttu-id="ba6f6-439">如果此参数 `Nothing` 或空字符串，则无法通过其类型名称直接访问此组类型上的默认实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-439">If this parameter is `Nothing` or an empty string, default instances on this group type are not accessible directly through their type's name.</span></span> <span data-ttu-id="ba6f6-440">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-440">(__Note.__</span></span> <span data-ttu-id="ba6f6-441">在 Visual Basic 语言的所有当前实现中，将忽略 `DefaultInstanceAlias` 参数，但编译器提供的代码除外。）</span><span class="sxs-lookup"><span data-stu-id="ba6f6-441">In all current implementations of the Visual Basic language, the `DefaultInstanceAlias` parameter is ignored, except in compiler-provided code.)</span></span>

<span data-ttu-id="ba6f6-442">可以通过使用逗号分隔前三个参数中的类型和方法的名称，将多个类型收集到同一个组中。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-442">Multiple types can be collected into the same group by separating the names of the types and methods in the first three parameters using commas.</span></span> <span data-ttu-id="ba6f6-443">每个参数中必须有相同数量的项，并且列表元素按顺序匹配。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-443">There must be the same number of items in each parameter, and the list elements are matched in order.</span></span> <span data-ttu-id="ba6f6-444">例如，下面的属性声明将从 `C1`、`C2` 或 `C3` 派生的类型收集到单个组中：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-444">For example, the following attribute declaration collects types that derive from `C1`, `C2` or `C3` into a single group:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

<span data-ttu-id="ba6f6-445">Create 方法的签名的格式必须为 `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-445">The signature of the create method must be of the form `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`.</span></span> <span data-ttu-id="ba6f6-446">Dispose 方法的格式必须为 `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-446">The dispose method must be of the form `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`.</span></span> <span data-ttu-id="ba6f6-447">因此，可以按如下所示声明前面部分中的示例的 group 类：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-447">Thus, the group class for the example in the preceding section could be declared as follows:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub
End Class
```

<span data-ttu-id="ba6f6-448">如果源文件声明了派生类 `Form1`，则生成的组类将等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-448">If a source file declared a derived class `Form1`, the generated group class would be equivalent to:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("Form", "Create", _
    "Dispose", "My.Forms")> _
Public NotInheritable Class MyForms
    Private Shared Function Create(Of T As {New, Form}) _
        (Instance As T) As T
        If Instance Is Nothing Then
            Return New T()
        Else
            Return Instance
        End If
    End Function

    Private Shared Sub Dispose(Of T As Form)(ByRef Instance As T)
        Instance.Close()
        Instance = Nothing
    End Sub

    Private m_Form1 As Form1

    Public Property Form1() As Form1
        Get
            Return Create(m_Form1)
        End Get
        Set (Value As Form1)
            If Value IsNot Nothing AndAlso Value IsNot m_Form1 Then
                Throw New ArgumentException( _
                    "Property can only be set to Nothing.")
            End If
            Dispose(m_Form1)
        End Set
    End Property
End Class
```

### <a name="extension-method-collection"></a><span data-ttu-id="ba6f6-449">扩展方法集合</span><span class="sxs-lookup"><span data-stu-id="ba6f6-449">Extension Method Collection</span></span>

<span data-ttu-id="ba6f6-450">通过使用当前上下文中可用的名称 `I` 收集所有扩展方法，收集成员访问表达式 `E.I` 的扩展方法：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-450">Extension methods for the member access expression `E.I` are collected by gathering all of the extension methods with the name `I` that are available in the current context:</span></span>

1. <span data-ttu-id="ba6f6-451">首先，将检查包含表达式的每个嵌套类型，从最内层开始到最外面的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-451">First, each nested type containing the expression is checked, starting from the innermost and going to the outermost.</span></span>
2. <span data-ttu-id="ba6f6-452">然后，将检查每个嵌套命名空间，从最内层开始到最外面的命名空间。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-452">Then, each nested namespace is checked, starting from the innermost and going to the outermost namespace.</span></span>
3. <span data-ttu-id="ba6f6-453">然后，将检查源文件中的导入。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-453">Then, the imports in the source file are checked.</span></span>
4. <span data-ttu-id="ba6f6-454">然后，将检查编译环境定义的导入。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-454">Then, the imports defined by the compilation environment are checked.</span></span>

<span data-ttu-id="ba6f6-455">仅当存在从目标表达式类型到扩展方法的第一个参数类型的扩大本机转换时，才收集扩展方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-455">An extension method is collected only if there is a widening native conversion from the target expression type to the type of the first parameter of the extension method.</span></span> <span data-ttu-id="ba6f6-456">和与常规简单名称表达式绑定不同，搜索将收集*所有*扩展方法;找到扩展方法时，集合不会停止。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-456">And unlike regular simple name expression binding, the search collects *all* extension methods; the collection does not stop when an extension method is found.</span></span> <span data-ttu-id="ba6f6-457">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-457">For example:</span></span>

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
        Sub M1(c As C1, y As Double)
        End Sub
    End Module
End Namespace

Namespace N1.N2.N3
    Module Test
        Sub Main()
            Dim x As New C1()

            ' Calls N1C1Extensions.M1
            x.M1(10)
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="ba6f6-458">在此示例中，即使在 `N1C1Extensions.M1`之前找到 `N2C1Extensions.M1`，它们都被视为扩展方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-458">In this example, even though `N2C1Extensions.M1` is found before `N1C1Extensions.M1`, they both are considered as extension methods.</span></span> <span data-ttu-id="ba6f6-459">所有扩展方法一经收集后，它们就会*扩充*。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-459">Once all of the extension methods have been collected, they are then *curried*.</span></span> <span data-ttu-id="ba6f6-460">Currying 采用扩展方法调用的目标，并将其应用于扩展方法调用，从而生成新的方法签名，并删除第一个参数（因为已指定了该参数）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-460">Currying takes the target of the extension method call and applies it to the extension method call, resulting in a new method signature with the first parameter removed (because it has been specified).</span></span> <span data-ttu-id="ba6f6-461">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-461">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(x As Integer, y As Integer)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(x As Integer, y As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(y As Integer)
        '        Ext2.M(y As Double)
        v.M(10)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-462">在上面的示例中，将 `v` 应用到 `Ext1.M` 的扩充结果是 `Sub M(y As Integer)`的方法签名。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-462">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(y As Integer)`.</span></span>

<span data-ttu-id="ba6f6-463">除了删除扩展方法的第一个参数外，currying 还会删除作为第一个参数类型的一部分的任何方法类型参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-463">In addition to removing the first parameter of the extension method, currying also removes any method type parameters that are a part of the type of the first parameter.</span></span> <span data-ttu-id="ba6f6-464">当 currying 扩展方法与方法类型参数一起使用时，类型推理将应用于第一个参数，并为任何推断出的类型参数修复结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-464">When currying an extension method with method type parameter, type inference is applied to the first parameter and the result is fixed for any type parameters that are inferred.</span></span> <span data-ttu-id="ba6f6-465">如果类型推理失败，则忽略方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-465">If type inference fails, the method is ignored.</span></span> <span data-ttu-id="ba6f6-466">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-466">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M(Of T, U)(x As T, y As U)
    End Sub
End Module

Module Ext2
    <Extension> _
    Sub M(Of T)(x As T, y As T)
    End Sub
End Module

Module Main
    Sub Test()
        Dim v As Integer = 10

        ' The curried method signatures considered are:
        '        Ext1.M(Of U)(y As U)
        '        Ext2.M(y As Integer)
        v.M(10)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-467">在上面的示例中，将 `v` 应用到 `Ext1.M` 的扩充结果是 `Sub M(Of U)(y As U)`的方法签名，因为在 currying 的结果中推断出类型参数 `T`，现已修复。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-467">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(Of U)(y As U)`, because the type parameter `T` is inferred as a result of the currying and is now fixed.</span></span> <span data-ttu-id="ba6f6-468">由于未将类型参数 `U` 推断为 currying 的一部分，因此它仍是一个打开的参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-468">Because the type parameter `U` was not inferred as a part of the currying, it remains an open parameter.</span></span> <span data-ttu-id="ba6f6-469">同样，由于在将 `v` 应用到 `Ext2.M`的结果中推断出了类型参数 `T`，因此参数的类型 `y` 将作为 `Integer`固定。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-469">Similarly, because the type parameter `T` is inferred as a result of applying `v` to `Ext2.M`, the type of parameter `y` becomes fixed as `Integer`.</span></span> <span data-ttu-id="ba6f6-470">它不会被推断为任何其他类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-470">It will not be inferred to be any other type.</span></span> <span data-ttu-id="ba6f6-471">Currying 签名时，还会应用除 `New` 约束之外的所有约束。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-471">When currying the signature, all constraints except for `New` constraints are also applied.</span></span> <span data-ttu-id="ba6f6-472">如果未满足约束，或依赖于未推断为 currying 的一部分的类型，则将忽略扩展方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-472">If the constraints are not satisfied, or depend on a type that was not inferred as a part of currying, the extension method is ignored.</span></span> <span data-ttu-id="ba6f6-473">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-473">For example:</span></span>

```vb
Imports System.Runtime.CompilerServices

Module Ext1
    <Extension> _
    Sub M1(Of T As Structure)(x As T, y As Integer)
    End Sub

    <Extension> _
    Sub M2(Of T As U, U)(x As T, y As U)
    End Sub
End Module

Module Main
    Sub Test()
        Dim s As String = "abc"

        ' Error: String does not satisfy the Structure constraint
        s.M1(10)

        ' Error: T depends on U, which cannot be inferred
        s.M2(10)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-474">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-474">__Note.__</span></span> <span data-ttu-id="ba6f6-475">执行扩展方法 currying 的主要原因之一是它允许查询表达式在计算查询模式方法的参数之前推断迭代的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-475">One of the main reasons for doing currying of extension methods is that it allows query expressions to infer the type of the iteration before evaluating the arguments to a query pattern method.</span></span> <span data-ttu-id="ba6f6-476">由于大多数查询模式方法都采用 lambda 表达式，后者需要进行类型推理，这大大简化了计算查询表达式的过程。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-476">Since most query pattern methods take lambda expressions, which require type inference themselves, this greatly simplifies the process of evaluating a query expression.</span></span>

<span data-ttu-id="ba6f6-477">与普通的接口继承不同，扩展两个接口之间相互关联的扩展方法是可用的，但前提是它们没有相同的扩充签名：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-477">Unlike normal interface inheritance, extension methods that extend two interfaces that do not relate to one another are available, as long as they do not have the same curried signature:</span></span>

```vb
Imports System.Runtime.CompilerServices

Interface I1
End Interface

Interface I2
End Interface

Class C1
    Implements I1, I2
End Class

Module I1Ext
    <Extension> _
    Sub M1(i As I1, x As Integer)
    End Sub

    <Extension> _
    Sub M2(i As I1, x As Integer)
    End Sub
End Module

Module I2Ext
    <Extension> _
    Sub M1(i As I2, x As Integer)
    End Sub

    <Extension> _
    Sub M2(I As I2, x As Double)
    End Sub
End Module

Module Main
    Sub Test()
        Dim c As New C1()

        ' Error: M is ambiguous between I1Ext.M1 and I2Ext.M1.
        c.M1(10)

        ' Calls I1Ext.M2
        c.M2(10)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-478">最后，请务必记住，执行后期绑定时不考虑扩展方法：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-478">Finally, it is important to remember that extension methods are not considered when doing late binding:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a><span data-ttu-id="ba6f6-479">字典成员访问表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-479">Dictionary Member Access Expressions</span></span>

<span data-ttu-id="ba6f6-480">*字典成员访问表达式*用于查找集合的成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-480">A *dictionary member access expression* is used to look up a member of a collection.</span></span> <span data-ttu-id="ba6f6-481">字典成员访问采用 `E!I`的形式，其中 `E` 是一个归类为值的表达式，而 `I` 是一个标识符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-481">A dictionary member access takes the form of `E!I`, where `E` is an expression that is classified as a value and `I` is an identifier.</span></span>

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

<span data-ttu-id="ba6f6-482">表达式的类型必须具有由单个 `String` 参数索引的默认属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-482">The type of the expression must have a default property indexed by a single `String` parameter.</span></span> <span data-ttu-id="ba6f6-483">字典成员访问表达式 `E!I` 转换为表达式 `E.D("I")`，其中 `D` 是 `E`的默认属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-483">The dictionary member access expression `E!I` is transformed into the expression `E.D("I")`, where `D` is the default property of `E`.</span></span> <span data-ttu-id="ba6f6-484">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-484">For example:</span></span>

```vb
Class Keys
    Public ReadOnly Default Property Item(s As String) As Integer
        Get
            Return 10
        End Get
    End Property 
End Class

Module Test
    Sub Main()
        Dim x As Keys = new Keys()
        Dim y As Integer
        ' The two statements are equivalent.
        y = x!abc
        y = x("abc")
    End Sub
End Module
```

<span data-ttu-id="ba6f6-485">如果在没有表达式的情况下指定惊叹号，则假定为直接包含 `With` 语句中的表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-485">If an exclamation point is specified with no expression, the expression from the immediately containing `With` statement is assumed.</span></span> <span data-ttu-id="ba6f6-486">如果没有包含 `With` 语句，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-486">If there is no containing `With` statement, a compile-time error occurs.</span></span>


## <a name="invocation-expressions"></a><span data-ttu-id="ba6f6-487">调用表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-487">Invocation Expressions</span></span>

<span data-ttu-id="ba6f6-488">调用表达式由调用目标和可选参数列表组成。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-488">An invocation expression consists of an invocation target and an optional argument list.</span></span>

```antlr
InvocationExpression
    : Expression ( OpenParenthesis ArgumentList? CloseParenthesis )?
    ;

ArgumentList
    : PositionalArgumentList
    | PositionalArgumentList Comma NamedArgumentList
    | NamedArgumentList
    ;

PositionalArgumentList
    : Expression? ( Comma Expression? )*
    ;

NamedArgumentList
    : IdentifierOrKeyword ColonEquals Expression
      ( Comma IdentifierOrKeyword ColonEquals Expression )*
    ;
```

<span data-ttu-id="ba6f6-489">目标表达式必须归类为方法组或类型为委托类型的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-489">The target expression must be classified as a method group or a value whose type is a delegate type.</span></span> <span data-ttu-id="ba6f6-490">如果目标表达式是其类型为委托类型的值，则调用表达式的目标将成为委托类型的 `Invoke` 成员的方法组，目标表达式将成为该方法组的关联目标表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-490">If the target expression is a value whose type is a delegate type, then the target of the invocation expression becomes the method group for the `Invoke` member of the delegate type and the target expression becomes the associated target expression of the method group.</span></span>

<span data-ttu-id="ba6f6-491">自变量列表具有两个部分：位置参数和命名参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-491">An argument list has two sections: positional arguments and named arguments.</span></span> <span data-ttu-id="ba6f6-492">*位置参数*是表达式，并且必须位于任何命名参数之前。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-492">*Positional arguments* are expressions and must precede any named arguments.</span></span> <span data-ttu-id="ba6f6-493">*命名参数*以可以匹配关键字的标识符开头，后面跟有 `:=` 和表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-493">*Named arguments* start with an identifier that can match keywords, followed by `:=` and an expression.</span></span>

<span data-ttu-id="ba6f6-494">如果方法组只包含一个可访问的方法（包括实例和扩展方法），并且该方法不使用参数，并且是一个函数，则方法组被解释为带有空参数列表的调用表达式，结果为用作带有所提供的参数列表的调用表达式的目标。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-494">If the method group only contains one accessible method, including both instance and extension methods, and that method takes no arguments and is a function, then the method group is interpreted as an invocation expression with an empty argument list and the result is used as the target of an invocation expression with the provided argument list(s).</span></span> <span data-ttu-id="ba6f6-495">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-495">For example:</span></span>

```vb
Class C1
    Function M1() As Integer()
        Return New Integer() { 1, 2, 3 }
    End Sub
End Class

Module Test
    Sub Main()
        Dim c As New C1()

        ' Prints 3
        Console.WriteLine(c.M1(2))
    End Sub
End Module
```

<span data-ttu-id="ba6f6-496">否则，重载决策将应用到方法，以选取最适用于给定自变量列表的方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-496">Otherwise, overload resolution is applied to the methods to pick the most applicable method for the given argument list(s).</span></span> <span data-ttu-id="ba6f6-497">如果最适用的方法是函数，则调用表达式的结果将分类为类型为函数的返回类型的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-497">If the most applicable method is a function, then the result of the invocation expression is classified as a value typed as the return type of the function.</span></span> <span data-ttu-id="ba6f6-498">如果最适用的方法是子例程，则结果归类为 void。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-498">If the most applicable method is a subroutine, then the result is classified as void.</span></span> <span data-ttu-id="ba6f6-499">如果最适用的方法是没有正文的分部方法，则将忽略调用表达式，并将结果归类为 void。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-499">If the most applicable method is a partial method that has no body, then the invocation expression is ignored and the result is classified as void.</span></span>

<span data-ttu-id="ba6f6-500">对于早期绑定调用表达式，将按照在目标方法中声明相应参数的顺序来计算参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-500">For an early-bound invocation expression, the arguments are evaluated in the order in which the corresponding parameters are declared in the target method.</span></span> <span data-ttu-id="ba6f6-501">对于后期绑定成员访问表达式，这些表达式将按照它们在成员访问表达式中出现的顺序进行计算：请参阅[后期绑定表达式](expressions.md#late-bound-expressions)部分。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-501">For a late-bound member access expression, they are evaluated in the order in which they appear in the member access expression: see Section [Late-Bound Expressions](expressions.md#late-bound-expressions).</span></span>

## <a name="overloaded-method-resolution"></a><span data-ttu-id="ba6f6-502">重载方法解析：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-502">Overloaded Method Resolution:</span></span>
<span data-ttu-id="ba6f6-503">对于重载决策，给定自变量列表、Genericity、对参数列表的适用性、传递参数以及可选参数、条件方法和类型参数推理的选取参数的成员/类型的具体内容：请参阅部分[重载决策](overload-resolution.md)部分。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-503">For Overload Resolution, Specificity of members/types given an argument list, Genericity, Applicability to Argument List, Passing Arguments, and Picking Arguments for Optional Parameters, Conditional Methods, and Type Argument Inference: see Section [Overload Resolution](overload-resolution.md).</span></span>

## <a name="index-expressions"></a><span data-ttu-id="ba6f6-504">索引表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-504">Index Expressions</span></span>

<span data-ttu-id="ba6f6-505">*索引表达式*将导致数组元素，或将属性组发送器为属性访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-505">An *index expression* results in an array element or reclassifies a property group into a property access.</span></span> <span data-ttu-id="ba6f6-506">索引表达式的顺序包括：表达式、左括号、索引参数列表和右括号。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-506">An index expression consists of, in order, an expression, an opening parenthesis, an index argument list, and a closing parenthesis.</span></span>

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

<span data-ttu-id="ba6f6-507">索引表达式的目标必须分类为属性组或值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-507">The target of the index expression must be classified as either a property group or a value.</span></span> <span data-ttu-id="ba6f6-508">索引表达式的处理方式如下：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-508">An index expression is processed as follows:</span></span>

* <span data-ttu-id="ba6f6-509">如果目标表达式归类为一个值，并且它的类型不是数组类型、`Object`或 `System.Array`，则该类型必须有一个默认属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-509">If the target expression is classified as a value and if its type is not an array type, `Object`, or `System.Array`, the type must have a default property.</span></span> <span data-ttu-id="ba6f6-510">索引在属性组上执行，该属性组表示该类型的所有默认属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-510">The index is performed on a property group that represents all of the default properties of the type.</span></span> <span data-ttu-id="ba6f6-511">尽管在 Visual Basic 中声明无参数的默认属性是无效的，但其他语言可能允许声明这种属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-511">Although it is not valid to declare a parameterless default property in Visual Basic, other languages may allow declaring such a property.</span></span> <span data-ttu-id="ba6f6-512">因此，允许为不带参数的属性编制索引。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-512">Consequently, indexing a property with no arguments is allowed.</span></span>

* <span data-ttu-id="ba6f6-513">如果表达式的值为数组类型的值，则参数列表中的参数数目必须与数组类型的秩相同，并且不能包含命名参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-513">If the expression results in a value of an array type, the number of arguments in the argument list must be the same as the rank of the array type and may not include named arguments.</span></span> <span data-ttu-id="ba6f6-514">如果任何索引在运行时无效，则会引发 `System.IndexOutOfRangeException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-514">If any of the indexes are invalid at run time, a `System.IndexOutOfRangeException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-515">每个表达式必须可隐式转换为类型 `Integer`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-515">Each expression must be implicitly convertible to type `Integer`.</span></span> <span data-ttu-id="ba6f6-516">索引表达式的结果是指定索引处的变量，并归类为变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-516">The result of the index expression is the variable at the specified index and is classified as a variable.</span></span>

* <span data-ttu-id="ba6f6-517">如果表达式归类为属性组，则使用重载决策来确定属性之一是否适用于索引参数列表。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-517">If the expression is classified as a property group, overload resolution is used to determine whether one of the properties is applicable to the index argument list.</span></span> <span data-ttu-id="ba6f6-518">如果属性组只包含一个具有 `Get` 访问器的属性，并且该访问器没有参数，则该属性组将被解释为带有空参数列表的索引表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-518">If the property group only contains one property that has a `Get` accessor and if that accessor takes no arguments, then the property group is interpreted as an index expression with an empty argument list.</span></span> <span data-ttu-id="ba6f6-519">结果用作当前索引表达式的目标。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-519">The result is used as the target of the current index expression.</span></span> <span data-ttu-id="ba6f6-520">如果没有可用的属性，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-520">If no properties are applicable, then a compile-time error occurs.</span></span> <span data-ttu-id="ba6f6-521">否则，表达式将导致属性访问具有属性组的关联目标表达式（如果有）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-521">Otherwise, the expression results in a property access with the associated target expression (if any) of the property group.</span></span>

* <span data-ttu-id="ba6f6-522">如果表达式归类为后期绑定属性组，或作为 `Object` 或 `System.Array`类型的值，则索引表达式的处理将延迟到运行时，并且索引后期绑定。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-522">If the expression is classified as a late-bound property group or as a value whose type is `Object` or `System.Array`, the processing of the index expression is deferred until run time and the indexing is late-bound.</span></span> <span data-ttu-id="ba6f6-523">表达式将导致后期绑定属性访问类型化为 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-523">The expression results in a late-bound property access typed as `Object`.</span></span> <span data-ttu-id="ba6f6-524">关联的目标表达式是目标表达式（如果它是一个值）或属性组的关联目标表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-524">The associated target expression is either the target expression, if it is a value, or the associated target expression of the property group.</span></span> <span data-ttu-id="ba6f6-525">在运行时，将按如下所示处理表达式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-525">At run time the expression is processed as follows:</span></span>

* <span data-ttu-id="ba6f6-526">如果表达式归类为后期绑定属性组，则该表达式可能会导致方法组、属性组或值（如果该成员是实例或共享变量）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-526">If the expression is classified as a late-bound property group, the expression may result in a method group, a property group, or a value (if the member is an instance or shared variable).</span></span> <span data-ttu-id="ba6f6-527">如果结果为方法组或属性组，则将重载决策应用于组，以确定参数列表的正确方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-527">If the result is a method group or property group, overload resolution is applied to the group to determine the correct method for the argument list.</span></span> <span data-ttu-id="ba6f6-528">如果重载决策失败，则会引发 `System.Reflection.AmbiguousMatchException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-528">If overload resolution fails, a `System.Reflection.AmbiguousMatchException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-529">然后，将结果作为属性访问或作为调用进行处理，并返回结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-529">Then the result is processed either as a property access or as an invocation and the result is returned.</span></span> <span data-ttu-id="ba6f6-530">如果调用的是子程序，则结果为 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-530">If the invocation is of a subroutine, the result is `Nothing`.</span></span>

* <span data-ttu-id="ba6f6-531">如果目标表达式的运行时类型是数组类型或 `System.Array`，则索引表达式的结果为指定索引处的变量的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-531">If the run-time type of the target expression is an array type or `System.Array`, the result of the index expression is the value of the variable at the specified index.</span></span>

* <span data-ttu-id="ba6f6-532">否则，表达式的运行时类型必须有一个默认属性，并对表示该类型上所有默认属性的属性组执行索引。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-532">Otherwise, the run-time type of the expression must have a default property and the index is performed on the property group that represents all of the default properties on the type.</span></span> <span data-ttu-id="ba6f6-533">如果该类型没有默认属性，则会引发 `System.MissingMemberException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-533">If the type has no default property, then a `System.MissingMemberException` exception is thrown.</span></span>


## <a name="new-expressions"></a><span data-ttu-id="ba6f6-534">新表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-534">New Expressions</span></span>

<span data-ttu-id="ba6f6-535">`New` 运算符用于创建类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-535">The `New` operator is used to create new instances of types.</span></span> <span data-ttu-id="ba6f6-536">有四种形式的 `New` 表达式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-536">There are four forms of `New` expressions:</span></span>

* <span data-ttu-id="ba6f6-537">对象创建表达式用于创建类类型和值类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-537">Object-creation expressions are used to create new instances of class types and value types.</span></span>

* <span data-ttu-id="ba6f6-538">数组创建表达式用于创建数组类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-538">Array-creation expressions are used to create new instances of array types.</span></span>

* <span data-ttu-id="ba6f6-539">委托创建表达式（不具有对象创建表达式中的不同语法）用于创建委托类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-539">Delegate-creation expressions (which do not have a distinct syntax from object-creation expressions) are used to create new instances of delegate types.</span></span>

* <span data-ttu-id="ba6f6-540">匿名对象创建表达式用于创建匿名类类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-540">Anonymous object-creation expressions are used to create new instances of anonymous class types.</span></span>

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

<span data-ttu-id="ba6f6-541">`New` 表达式归类为值，结果是该类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-541">A `New` expression is classified as a value and the result is the new instance of the type.</span></span>


### <a name="object-creation-expressions"></a><span data-ttu-id="ba6f6-542">对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-542">Object-Creation Expressions</span></span>

<span data-ttu-id="ba6f6-543">对象创建表达式用于创建类类型或结构类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-543">An object-creation expression is used to create a new instance of a class type or a structure type.</span></span>

```antlr
ObjectCreationExpression
    : 'New' NonArrayTypeName ( OpenParenthesis ArgumentList? CloseParenthesis )?
      ObjectCreationExpressionInitializer?
    ;

ObjectCreationExpressionInitializer
    : ObjectMemberInitializer
    | ObjectCollectionInitializer
    ;

ObjectMemberInitializer
    : 'With' OpenCurlyBrace FieldInitializerList CloseCurlyBrace
    ;

FieldInitializerList
    : FieldInitializer ( Comma FieldInitializer )*
    ;

FieldInitializer
    : 'Key'? ('.' IdentifierOrKeyword Equals )? Expression
    ;

ObjectCollectionInitializer
    : 'From' CollectionInitializer
    ;

CollectionInitializer
    : OpenCurlyBrace CollectionElementList? CloseCurlyBrace
    ;

CollectionElementList
    : CollectionElement ( Comma CollectionElement )*
    ;

CollectionElement
    : Expression
    | CollectionInitializer
    ;
```

<span data-ttu-id="ba6f6-544">对象创建表达式的类型必须是类类型、结构类型或具有 `New` 约束的类型参数，不能是 `MustInherit` 类。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-544">The type of an object creation expression must be a class type, a structure type, or a type parameter with a `New` constraint and cannot be a `MustInherit` class.</span></span> <span data-ttu-id="ba6f6-545">给定形式为 `New T(A)`的对象创建表达式，其中 `T` 为类类型或结构类型，`A` 为可选参数列表，重载决策确定要调用的 `T` 的正确构造函数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-545">Given an object creation expression of the form `New T(A)`, where `T` is a class type or structure type and `A` is an optional argument list, overload resolution determines the correct constructor of `T` to call.</span></span> <span data-ttu-id="ba6f6-546">具有 `New` 约束的类型参数被视为具有一个无参数的构造函数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-546">A type parameter with a `New` constraint is considered to have a single, parameterless constructor.</span></span> <span data-ttu-id="ba6f6-547">如果没有可调用的构造函数，则会发生编译时错误;否则，表达式会导致使用所选构造函数创建 `T` 的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-547">If no constructor is callable, a compile-time error occurs; otherwise the expression results in the creation of a new instance of `T` using the chosen constructor.</span></span> <span data-ttu-id="ba6f6-548">如果没有参数，则可以省略括号。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-548">If there are no arguments, the parentheses may be omitted.</span></span>

<span data-ttu-id="ba6f6-549">分配实例的位置取决于实例是类类型还是值类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-549">Where an instance is allocated depends on whether the instance is a class type or a value type.</span></span> <span data-ttu-id="ba6f6-550">类类型的 `New` 实例是在系统堆上创建的，而值类型的新实例是直接在堆栈上创建的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-550">`New` instances of class types are created on the system heap, while new instances of value types are created directly on the stack.</span></span>

<span data-ttu-id="ba6f6-551">对象创建表达式可以选择在构造函数参数之后指定成员初始值设定项的列表。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-551">An object-creation expression can optionally specify a list of member initializers after the constructor arguments.</span></span> <span data-ttu-id="ba6f6-552">这些成员初始值设定项以关键字 `With`作为前缀，而初始值设定项列表被解释为在 `With` 语句的上下文中。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-552">These member initializers are prefixed with the keyword `With`, and the initializer list is interpreted as if it was in the context of a `With` statement.</span></span> <span data-ttu-id="ba6f6-553">例如，给定类：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-553">For example, given the class:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

<span data-ttu-id="ba6f6-554">代码：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-554">The code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

<span data-ttu-id="ba6f6-555">大致等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-555">Is roughly equivalent to:</span></span>

```vb
Module Test
    Sub Main()
        Dim x, _t1 As Customer

        _t1 = New Customer()
        With _t1
            .Name = "Bob Smith"
            .Address = "123 Main St."
        End With

        x = _t1
    End Sub
End Module
```

<span data-ttu-id="ba6f6-556">每个初始值设定项都必须指定一个要分配的名称，并且该名称必须是所构造的类型的非`ReadOnly` 实例变量或属性;如果正在构造的类型为 `Object`，则不会对成员访问进行后期绑定。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-556">Each initializer must specify a name to assign, and the name must be a non-`ReadOnly` instance variable or property of the type being constructed; the member access will not be late bound if the type being constructed is `Object`.</span></span> <span data-ttu-id="ba6f6-557">初始值设定项不能使用 `Key` 关键字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-557">Initializers may not use the `Key` keyword.</span></span> <span data-ttu-id="ba6f6-558">类型中的每个成员只能初始化一次。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-558">Each member in a type can only be initialized once.</span></span> <span data-ttu-id="ba6f6-559">但是，初始值设定项表达式可能相互引用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-559">The initializer expressions, however, may refer to each other.</span></span> <span data-ttu-id="ba6f6-560">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-560">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="ba6f6-561">初始值设定项是从左到右赋值的，因此，如果初始值设定项引用尚未初始化的成员，则在构造函数运行后，将看到该实例变量的任何值：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-561">The initializers are assigned left-to-right, so if an initializer refers to a member that has not been initialized yet, it will see whatever value the instance variable after the constructor ran:</span></span>

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="ba6f6-562">初始值设定项可以嵌套：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-562">Initializers can be nested:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As Address
    Dim Age As Integer
End Class

Class Address
    Dim Street As String
    Dim City As String
    Dim State As String
    Dim ZIP As String
End Class

Module Test
    Sub Main()
        Dim c As New Customer() With { _
            .Name = "John Smith", _
            .Address = New Address() With { _
                .Street = "23 Main St.", _
                .City = "Peoria", _
                .State = "IL", _
                .ZIP = "13934" }, _
            .Age = 34 }
    End Sub
End Module
```

<span data-ttu-id="ba6f6-563">如果创建的类型是集合类型并且具有名为 `Add` 的实例方法（包括扩展方法和共享方法），则对象创建表达式可以指定以关键字 `From`为前缀的集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-563">If the type being created is a collection type and has an instance method named `Add` (including extension methods and shared methods), then the object-creation expression can specify a collection initializer prefixed by the keyword `From`.</span></span> <span data-ttu-id="ba6f6-564">对象创建表达式不能同时指定成员初始值设定项和集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-564">An object-creation expression cannot specify both a member initializer and a collection initializer.</span></span> <span data-ttu-id="ba6f6-565">集合初始值设定项中的每个元素都作为参数传递给 `Add` 函数的调用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-565">Each element in the collection initializer is passed as an argument to an invocation of the `Add` function.</span></span> <span data-ttu-id="ba6f6-566">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-566">For example:</span></span>

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

<span data-ttu-id="ba6f6-567">等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-567">is equivalent to:</span></span>

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

<span data-ttu-id="ba6f6-568">如果某个元素是集合初始值设定项本身，则子集合初始值设定项的每个元素都将作为单个参数传递到 `Add` 函数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-568">If an element is a collection initializer itself, each element of the sub-collection initializer will be passed as an individual argument to the `Add` function.</span></span> <span data-ttu-id="ba6f6-569">例如，以下内容：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-569">For example, the following:</span></span>

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

<span data-ttu-id="ba6f6-570">等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-570">is equivalent to:</span></span>

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

<span data-ttu-id="ba6f6-571">这种展开操作始终完成，只是在一层深度上完成;之后，子初始值设定项被视为数组文本。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-571">This expansion is always done and is only ever done one level deep; after that, sub-initializers are considered array literals.</span></span> <span data-ttu-id="ba6f6-572">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-572">For example:</span></span>

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a><span data-ttu-id="ba6f6-573">数组表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-573">Array Expressions</span></span>

<span data-ttu-id="ba6f6-574">数组表达式用于创建数组类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-574">An array expression is used to create a new instance of an array type.</span></span> <span data-ttu-id="ba6f6-575">数组表达式有两种类型：数组创建表达式和数组文本。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-575">There are two types of array expressions: array creation expressions, and array literals.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="ba6f6-576">数组创建表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-576">Array creation expressions</span></span>

<span data-ttu-id="ba6f6-577">如果提供了数组大小初始化修饰符，则会通过从数组大小初始化参数列表中删除各个参数来派生生成的数组类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-577">If an array size initialization modifier is supplied, the resulting array type is derived by deleting each of the individual arguments from the array size initialization argument list.</span></span> <span data-ttu-id="ba6f6-578">每个参数的值确定新分配的数组实例中相应维度的上限。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-578">The value of each argument determines the upper bound of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="ba6f6-579">如果表达式具有非空集合初始值设定项，则参数列表中的每个参数都必须是常量，并且表达式列表指定的秩和维度长度必须与集合初始值设定项的匹配项和维度长度匹配。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-579">If the expression has a non-empty collection initializer, each argument in the argument list must be a constant, and the rank and dimension lengths specified by the expression list must match those of the collection initializer.</span></span>

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

<span data-ttu-id="ba6f6-580">如果未提供数组大小初始化修饰符，则类型名称必须是数组类型，并且集合初始值设定项必须为空，或者具有与指定数组类型的秩相同的嵌套级别数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-580">If an array size initialization modifier is not supplied, then the type name must be an array type and the collection initializer must be empty or have the same number of levels of nesting as the rank of the specified array type.</span></span> <span data-ttu-id="ba6f6-581">最内层嵌套级别中的所有元素都必须能够隐式转换为数组的元素类型，并且必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-581">All of the elements in the innermost nesting level must be implicitly convertible to the element type of the array and must be classified as a value.</span></span> <span data-ttu-id="ba6f6-582">每个嵌套集合初始值设定项中的元素数必须始终与处于同一级别的其他集合的大小一致。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-582">The number of elements in each nested collection initializer must always be consistent with the size of the other collections at the same level.</span></span> <span data-ttu-id="ba6f6-583">各个维度长度是从集合初始值设定项的每个相应嵌套级别中的元素数推断出来的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-583">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the collection initializer.</span></span> <span data-ttu-id="ba6f6-584">如果集合初始值设定项为空，则每个维度的长度为零。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-584">If the collection initializer is empty, the length of each dimension is zero.</span></span>

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

<span data-ttu-id="ba6f6-585">集合初始值设定项的最外层嵌套级别对应于数组的最左边的维度，最内层嵌套级别对应于最右边的维度。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-585">The outermost nesting level of a collection initializer corresponds to the leftmost dimension of an array, and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="ba6f6-586">示例：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-586">The example:</span></span>

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

<span data-ttu-id="ba6f6-587">等效于以下内容：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-587">Is equivalent to the following:</span></span>

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

<span data-ttu-id="ba6f6-588">如果集合初始值设定项为空（即，包含大括号但没有初始值设定项列表的集合），并且已知要初始化的数组的维度边界，则空集合初始值设定项表示指定大小的数组实例，其中所有元素都已初始化为元素类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-588">If the collection initializer is empty (that is, one that contains curly braces but no initializer list) and the bounds of the dimensions of the array being initialized are known, the empty collection initializer represents an array instance of the specified size where all the elements have been initialized to the element type's default value.</span></span> <span data-ttu-id="ba6f6-589">如果要初始化的数组的维度界限未知，则空集合初始值设定项表示一个数组实例，其中所有维度的大小均为零。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-589">If the bounds of the dimensions of the array being initialized are not known, the empty collection initializer represents an array instance in which all dimensions are size zero.</span></span>

<span data-ttu-id="ba6f6-590">在实例的整个生存期内，每个维度的数组实例的秩和长度都是固定的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-590">An array instance's rank and length of each dimension are constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="ba6f6-591">换句话说，不能更改现有数组实例的排名，也不能调整其维度的大小。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-591">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

#### <a name="array-literals"></a><span data-ttu-id="ba6f6-592">数组文本</span><span class="sxs-lookup"><span data-stu-id="ba6f6-592">Array Literals</span></span>

<span data-ttu-id="ba6f6-593">数组文本表示一个数组，该数组的元素类型、秩和边界是从表达式上下文和集合初始值设定项的组合推断而来的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-593">An array literal denotes an array whose element type, rank, and bounds are inferred from a combination of the expression context and a collection initializer.</span></span> <span data-ttu-id="ba6f6-594">这将在部分[表达式重新分类](expressions.md#expression-reclassification)中介绍。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-594">This is explained in Section [Expression Reclassification](expressions.md#expression-reclassification).</span></span>

```antlr
ArrayExpression
    : ArrayCreationExpression
    | ArrayLiteralExpression
    ;

ArrayCreationExpression
    : 'New' NonArrayTypeName ArrayNameModifier CollectionInitializer
    ;

ArrayLiteralExpression
    : CollectionInitializer
    ;
```

<span data-ttu-id="ba6f6-595">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-595">For example:</span></span>

```vb
' array of integers
Dim a = {1, 2, 3}

' array of shorts
Dim b = {1S, 2S, 3S}

' array of shorts whose type is taken from the context
Dim c As Short() = {1, 2, 3}

' array of type Integer(,)
Dim d = {{1, 0}, {0, 1}}

' jagged array of rank ()()
Dim e = {({1, 0}), ({0, 1})}

' error: inconsistent rank
Dim f = {{1}, {2, 3}}

' error: inconsistent rank
Dim g = {1, {2}}
```

<span data-ttu-id="ba6f6-596">数组文本中集合初始值设定项的格式和要求与数组创建表达式中的集合初始值设定项的格式和要求完全相同。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-596">The format and requirements for the collection initializer in an array literal is exactly the same as that for the collection initializer in an array creation expression.</span></span>

<span data-ttu-id="ba6f6-597">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-597">__Note.__</span></span> <span data-ttu-id="ba6f6-598">数组文本不会自行创建数组;相反，将表达式重新分类为导致创建数组的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-598">An array literal does not create the array in and of itself; instead, it is the reclassification of the expression into a value that causes the array to be created.</span></span> <span data-ttu-id="ba6f6-599">例如，由于没有从 `Integer()` 到 `Short()`的转换，因此不可能进行转换 `CType(new Integer() {1,2,3}, Short())`;但表达式 `CType({1,2,3},Short())` 是可能的，因为它首先将数组文本发送器数组创建表达式 `New Short() {1,2,3}`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-599">For instance, the conversion `CType(new Integer() {1,2,3}, Short())` is not possible because there is no conversion from `Integer()` to `Short()`; but the expression `CType({1,2,3},Short())` is possible because it first reclassifies the array literal into the array creation expression `New Short() {1,2,3}`.</span></span>


### <a name="delegate-creation-expressions"></a><span data-ttu-id="ba6f6-600">委托创建表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-600">Delegate-Creation Expressions</span></span>

<span data-ttu-id="ba6f6-601">委托创建表达式用于创建委托类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-601">A delegate-creation expression is used to create a new instance of a delegate type.</span></span> <span data-ttu-id="ba6f6-602">委托创建表达式的参数必须是分类为方法指针或 lambda 方法的表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-602">The argument of a delegate-creation expression must be an expression classified as a method pointer or a lambda method.</span></span>

<span data-ttu-id="ba6f6-603">如果参数是一个方法指针，则该方法指针引用的其中一个方法必须适用于委托类型的签名。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-603">If the argument is a method pointer, one of the methods referenced by the method pointer must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="ba6f6-604">`M` 适用于委托类型的方法 `D` 如果：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-604">A method `M` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="ba6f6-605">`M` 不 `Partial` 或具有正文。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-605">`M` is not `Partial` or has a body.</span></span>

* <span data-ttu-id="ba6f6-606">`M` 和 `D` 都是函数，或 `D` 是一个子例程。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-606">Both `M` and `D` are functions, or `D` is a subroutine.</span></span>

* <span data-ttu-id="ba6f6-607">`M` 和 `D` 具有相同数量的参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-607">`M` and `D` have the same number of parameters.</span></span>

* <span data-ttu-id="ba6f6-608">`M` 的参数类型都具有从相应参数类型的类型到 `D`的转换，以及它们的修饰符（即 `ByRef`，`ByVal`）匹配项。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-608">The parameter types of `M` each have a conversion from the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="ba6f6-609">`M`的返回类型（如果有）具有转换为 `D`的返回类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-609">The return type of `M`, if any, has a conversion to the return type of `D`.</span></span>

<span data-ttu-id="ba6f6-610">如果方法指针引用了后期绑定访问，则假定使用的参数数目与委托类型具有相同数量的参数，则假定使用后期绑定访问。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-610">If the method pointer references a late-bound access, then the late-bound access is assumed to be to a function that has the same number of parameters as the delegate type.</span></span>

<span data-ttu-id="ba6f6-611">如果未使用 strict 语义并且方法指针只引用了一个方法，但它不适用，原因是它没有参数并且委托类型为，则此方法被视为适用，并且参数或返回值为只是被忽略。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-611">If strict semantics are not being used and there is only one method referenced by the method pointer, but it is not applicable due to the fact that it has no parameters and the delegate type does, then the method is considered applicable and the parameters or return value are simply ignored.</span></span> <span data-ttu-id="ba6f6-612">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-612">For example:</span></span>

```vb
Delegate Sub F(x As Integer)

Module Test
    Sub M()
    End Sub

    Sub Main()
        ' Valid
        Dim x As F = AddressOf M
    End Sub
End Module
```

<span data-ttu-id="ba6f6-613">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-613">__Note.__</span></span> <span data-ttu-id="ba6f6-614">仅当由于扩展方法而未使用严格语义时，才允许使用此 relaxation。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-614">This relaxation is only allowed when strict semantics are not being used because of extension methods.</span></span> <span data-ttu-id="ba6f6-615">由于仅当某个正则方法不适用时才考虑扩展方法，因此，不带参数的实例方法可以使用参数隐藏扩展方法，以进行委托构造。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-615">Because extension methods are only considered if a regular method was not applicable, it is possible for an instance method with no parameters to hide an extension method with parameters for the purpose of delegate construction.</span></span>

<span data-ttu-id="ba6f6-616">如果方法指针引用的多个方法适用于委托类型，则使用重载决策在候选方法之间选取。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-616">If more than one method referenced by the method pointer is applicable to the delegate type, then overload resolution is used to pick between the candidate methods.</span></span> <span data-ttu-id="ba6f6-617">委托的参数类型将用作重载决策的参数类型的参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-617">The types of the parameters to the delegate are used as the types of the arguments for the purposes of overload resolution.</span></span> <span data-ttu-id="ba6f6-618">如果没有一个候选方法，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-618">If no one method candidate is most applicable, a compile-time error occurs.</span></span> <span data-ttu-id="ba6f6-619">在下面的示例中，用引用第二个 `Square` 方法的委托初始化本地变量，因为该方法更适用于 `DoubleFunc`的签名和返回类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-619">In the following example, the local variable is initialized with a delegate that refers to the second `Square` method because that method is more applicable to the signature and return type of `DoubleFunc`.</span></span>

```vb
Delegate Function DoubleFunc(x As Double) As Double

Module Test
    Function Square(x As Single) As Single
        Return x * x
    End Function 

    Function Square(x As Double) As Double
        Return x * x
    End Function

    Sub Main()
        Dim a As New DoubleFunc(AddressOf Square)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-620">如果第二个 `Square` 方法不存在，则将选择第一个 `Square` 方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-620">Had the second `Square` method not been present, the first `Square` method would have been chosen.</span></span> <span data-ttu-id="ba6f6-621">如果由编译环境或 `Option Strict`指定严格语义，则如果方法指针引用的最特定方法比委托签名更窄，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-621">If strict semantics are specified by the compilation environment or by `Option Strict`, then a compile-time error occurs if the most specific method referenced by the method pointer is narrower than the delegate signature.</span></span> <span data-ttu-id="ba6f6-622">如果以下情况，方法 `M` 被视为比委托类型更窄 `D`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-622">A method `M` is considered narrower than a delegate type `D` if:</span></span>

* <span data-ttu-id="ba6f6-623">`M` 的参数类型具有到 `D`的相应参数类型的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-623">A parameter type of `M` has a widening conversion to the corresponding parameter type of `D`.</span></span>

* <span data-ttu-id="ba6f6-624">或者，返回类型（如果有） `M` 具有到 `D`的返回类型的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-624">Or, the return type, if any, of `M` has a narrowing conversion to the return type of `D`.</span></span>

<span data-ttu-id="ba6f6-625">如果类型参数与方法指针相关联，则仅考虑具有相同数量的类型参数的方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-625">If type arguments are associated with the method pointer, only methods with the same number of type arguments are considered.</span></span> <span data-ttu-id="ba6f6-626">如果没有与方法指针关联的类型自变量，则在将签名与泛型方法进行匹配时，将使用类型推理。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-626">If no type arguments are associated with the method pointer, type inference is used when matching signatures against a generic method.</span></span> <span data-ttu-id="ba6f6-627">与其他正常类型推理不同，在推断类型参数时将使用委托的返回类型，但在确定最小泛型重载时仍不会考虑返回类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-627">Unlike other normal type inference, the return type of the delegate is used when inferring type arguments, but return types are still not considered when determining the least generic overload.</span></span> <span data-ttu-id="ba6f6-628">下面的示例演示为委托创建表达式提供类型参数的两种方式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-628">The following example shows both ways of supplying a type argument to a delegate-creation expression:</span></span>

```vb
Delegate Function D(s As String, i As Integer) As Integer
Delegate Function E() As Integer

Module Test
    Public Function F(Of T)(s As String, t1 As T) As T
    End Function

    Public Function G(Of T)() As T
    End Function

    Sub Main()
        Dim d1 As D = AddressOf f(Of Integer)    ' OK, type arg explicit
        Dim d2 As D = AddressOf f                ' OK, type arg inferred

        Dim e1 As E = AddressOf g(Of Integer)    ' OK, type arg explicit
        Dim e2 As E = AddressOf g                ' OK, infer from return
  End Sub
End Module
```

<span data-ttu-id="ba6f6-629">在上面的示例中，非泛型委托类型是使用泛型方法实例化的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-629">In the above example, a non-generic delegate type was instantiated using a generic method.</span></span> <span data-ttu-id="ba6f6-630">还可以使用泛型方法创建构造委托类型的实例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-630">It is also possible to create an instance of a constructed delegate type using a generic method.</span></span> <span data-ttu-id="ba6f6-631">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-631">For example:</span></span>

```vb
Delegate Function Predicate(Of U)(u1 As U, u2 As U) As Boolean

Module Test
    Function Compare(Of T)(t1 As List(of T), t2 As List(of T)) As Boolean
        ...
    End Function

    Sub Main()
        Dim p As Predicate(Of List(Of Integer))
        p = AddressOf Compare(Of Integer)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-632">如果委托创建表达式的参数是 lambda 方法，则 lambda 方法必须适用于委托类型的签名。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-632">If the argument to the delegate-creation expression is a lambda method, the lambda method must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="ba6f6-633">`L` 适用于委托类型的 lambda 方法 `D` if：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-633">A lambda method `L` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="ba6f6-634">如果 `L` 具有参数，则 `D` 具有相同数量的参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-634">If `L` has parameters, `D` has the same number of parameters.</span></span> <span data-ttu-id="ba6f6-635">（如果 `L` 没有参数，则将忽略 `D` 的参数。）</span><span class="sxs-lookup"><span data-stu-id="ba6f6-635">(If `L` has no parameters, the parameters of `D` are ignored.)</span></span>

* <span data-ttu-id="ba6f6-636">每个 `L` 的参数类型都有一个转换为相应参数类型的类型，`D`的类型以及它们的修饰符（即 `ByRef`，`ByVal`）匹配。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-636">The parameter types of `L` each have a conversion to the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="ba6f6-637">如果 `D` 是一个函数，则 `L` 的返回类型将转换为 `D`的返回类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-637">If `D` is a function, the return type of `L` has a conversion to the return type of `D`.</span></span> <span data-ttu-id="ba6f6-638">（如果 `D` 为子程序，则忽略 `L` 的返回值。）</span><span class="sxs-lookup"><span data-stu-id="ba6f6-638">(If `D` is a subroutine, the return value of `L` is ignored.)</span></span>

<span data-ttu-id="ba6f6-639">如果省略 `L` 的参数的参数类型，则将推断 `D` 中相应参数的类型;如果 `L` 的参数具有 array 或 nullable name 修饰符，则会生成编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-639">If the parameter type of a parameter of `L` is omitted, then the type of the corresponding parameter in `D` is inferred; if the parameter of `L` has array or nullable name modifiers, a compile-time error results.</span></span> <span data-ttu-id="ba6f6-640">`L` 的所有参数类型都可用后，将推断 lambda 方法中的表达式类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-640">Once all of the parameter types of `L` are available, then the type of the expression in the lambda method is inferred.</span></span> <span data-ttu-id="ba6f6-641">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-641">For example:</span></span>

```vb
Delegate Function F(x As Integer, y As Long) As Long

Module Test
    Sub Main()
        ' b inferred to Integer, c and return type inferred to Long
        Dim a As F = Function(b, c) b + c

        ' e and return type inferred to Integer, f inferred to Long
        Dim d As F = Function(e, f) e + CInt(f)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-642">在某些情况下，如果委托签名与 lambda 方法或方法签名并不完全匹配，则 .NET Framework 可能不支持本机创建委托。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-642">In some situations where delegate signature does not exactly match the lambda method or method signature, the .NET Framework may not support the delegate creation natively.</span></span> <span data-ttu-id="ba6f6-643">在这种情况下，使用 lambda 方法表达式来匹配这两种方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-643">In that situation, a lambda method expression is used to match the two methods.</span></span> <span data-ttu-id="ba6f6-644">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-644">For example:</span></span>

```vb
Delegate Function IntFunc(x As Integer) As Integer

Module Test
    Function SquareString(x As String) As String
        Return CInt(x) * CInt(x)
    End Function 

    Sub Main()
        ' The following two lines are equivalent
        Dim a As New IntFunc(AddressOf SquareString)
        Dim b As New IntFunc( _
            Function(x As Integer) CInt(SquareString(CStr(x))))
    End Sub
End Module
```

<span data-ttu-id="ba6f6-645">委托创建表达式的结果是一个委托实例，它从方法指针表达式引用具有关联目标表达式（如果有）的匹配方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-645">The result of a delegate-creation expression is a delegate instance that refers to the matching method with the associated target expression (if any) from the method pointer expression.</span></span> <span data-ttu-id="ba6f6-646">如果目标表达式的类型为值类型，则将值类型复制到系统堆上，因为委托只能指向堆上的对象的方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-646">If the target expression is typed as a value type, then the value type is copied onto the system heap because a delegate can only point to a method of an object on the heap.</span></span> <span data-ttu-id="ba6f6-647">委托引用的方法和对象在该委托的整个生存期内保持不变。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-647">The method and object to which a delegate refers remain constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="ba6f6-648">换句话说，创建委托后，不能更改其目标或对象。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-648">In other words, it is not possible to change the target or object of a delegate after it has been created.</span></span>

### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="ba6f6-649">匿名对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-649">Anonymous Object-Creation Expressions</span></span>

<span data-ttu-id="ba6f6-650">带有成员初始值设定项的对象创建表达式还可以完全省略类型名称。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-650">An object-creation expression with member initializers can also omit the type name entirely.</span></span>

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

<span data-ttu-id="ba6f6-651">在这种情况下，将基于作为表达式的一部分初始化的成员的类型和名称来构造匿名类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-651">In that case, an anonymous type is constructed based on the types and names of the members initialized as a part of the expression.</span></span> <span data-ttu-id="ba6f6-652">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-652">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-653">由匿名对象创建表达式创建的类型是没有名称的类，直接从 `Object`继承，并且具有一组与在成员初始值设定项列表中分配的成员同名的属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-653">The type created by an anonymous object-creation expression is a class that has no name, inherits directly from `Object`, and has a set of properties with the same name as the members assigned to in the member initializer list.</span></span> <span data-ttu-id="ba6f6-654">使用与局部变量类型推理相同的规则来推断每个属性的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-654">The type of each property is inferred using the same rules as local variable type inference.</span></span> <span data-ttu-id="ba6f6-655">生成的匿名类型还会重写 `ToString`，同时返回所有成员及其值的字符串表示形式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-655">Generated anonymous types also override `ToString`, returning a string representation of all members and their values.</span></span> <span data-ttu-id="ba6f6-656">（此字符串的准确格式超出了本规范的范围）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-656">(The exact format of this string is beyond the scope of this specification).</span></span>

<span data-ttu-id="ba6f6-657">默认情况下，匿名类型生成的属性为读写属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-657">By default, the properties generated by the anonymous type are read-write.</span></span> <span data-ttu-id="ba6f6-658">可以使用 `Key` 修饰符将匿名类型属性标记为只读。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-658">It is possible to mark an anonymous type property as read-only by using the `Key` modifier.</span></span> <span data-ttu-id="ba6f6-659">`Key` 修饰符指定字段可用于唯一标识匿名类型所表示的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-659">The `Key` modifier specifies that the field can be used to uniquely identify the value the anonymous type represents.</span></span> <span data-ttu-id="ba6f6-660">除了将属性设为只读外，还会导致匿名类型重写 `Equals` 和 `GetHashCode` 并实现接口 `System.IEquatable(Of T)` （为 `T`填充匿名类型）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-660">In addition to making the property read-only, it also causes the anonymous type to override `Equals` and  `GetHashCode` and to implement the interface `System.IEquatable(Of T)` (filling in the anonymous type for `T`).</span></span> <span data-ttu-id="ba6f6-661">成员定义如下：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-661">The members are defined as follows:</span></span>

<span data-ttu-id="ba6f6-662">`Function Equals(obj As Object) As Boolean` 和 `Function Equals(val As T) As Boolean` 是通过以下方式实现的：验证两个实例的类型相同，然后使用 `Object.Equals`比较每个 `Key` 成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-662">`Function Equals(obj As Object) As Boolean` and `Function Equals(val As T) As Boolean` are implemented by validating that the two instances are of the same type and then comparing each `Key` member using `Object.Equals`.</span></span> <span data-ttu-id="ba6f6-663">如果所有 `Key` 成员相等，则 `Equals` 返回 `True`，否则 `Equals` 返回 `False`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-663">If all `Key` members are equal, then `Equals` returns `True`, otherwise `Equals` returns `False`.</span></span>

<span data-ttu-id="ba6f6-664">实现 `Function GetHashCode() As Integer` 以便如果 `Equals` 对于匿名类型的两个实例都为 true，则 `GetHashCode` 将返回相同的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-664">`Function GetHashCode() As Integer` is implemented such that that if `Equals` is true for two instances of the anonymous type, then `GetHashCode` will return the same value.</span></span> <span data-ttu-id="ba6f6-665">哈希值以种子值开头，然后，对于每个 `Key` 成员，按顺序将哈希乘以31，并添加 `Key` 成员的哈希值（由 `GetHashCode`），前提是该成员不是值为 `Nothing`的引用类型或可以为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-665">The hash starts with a seed value and then, for each `Key` member, in order multiplies the hash by 31 and adds the `Key` member's hash value (provided by `GetHashCode`) if the member is not a reference type or nullable value type with the value of `Nothing`.</span></span>

<span data-ttu-id="ba6f6-666">例如，在语句中创建的类型：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-666">For example, the type created in the statement:</span></span>

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

<span data-ttu-id="ba6f6-667">创建一个与此类似的类（尽管完全实现可能会有所不同）：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-667">creates a class that looks approximately like this (although exact implementation may vary):</span></span>

```vb
Friend NotInheritable Class $Anonymous1
    Implements IEquatable(Of $Anonymous1)

    Private ReadOnly _zipCode As Integer
    Private _state As String

    Public Sub New(zipCode As Integer, state As String)
        _zipCode = zipcode
        _state = state
    End Sub

    Public ReadOnly Property ZipCode As Integer
        Get
            Return _zipCode
        End Get
    End Property

    Public Property State As String
        Get
            Return _state
        End Get
        Set (value As Integer)
            _state = value
        End Set
    End Property

    Public Overrides Function Equals(obj As Object) As Boolean
        Dim val As $Anonymous1 = TryCast(obj, $Anonymous1)
        Return Equals(val)
    End Function

    Public Overloads Function Equals(val As $Anonymous1) As Boolean _
        Implements IEquatable(Of $Anonymous1).Equals

        If val Is Nothing Then 
            Return False
        End If

        If Not Object.Equals(_zipCode, val._zipCode) Then 
            Return False
        End If

        Return True
    End Function

    Public Overrides Function GetHashCode() As Integer
        Dim hash As Integer = 0

        hash = hash Xor _zipCode.GetHashCode()

        Return hash
    End Function

    Public Overrides Function ToString() As String
        Return "{ Key .ZipCode = " & _zipCode & ", .State = " & _state & " }"
    End Function
End Class
```

<span data-ttu-id="ba6f6-668">若要简化从另一种类型的字段创建匿名类型的情况，可在以下情况下直接从表达式推断字段名称：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-668">To simplify the situation where an anonymous type is created from the fields of another type, field names can be inferred directly from expressions in the following cases:</span></span>

* <span data-ttu-id="ba6f6-669">简单的名称表达式 `x` 推导名称 `x`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-669">A simple name expression `x` infers the name `x`.</span></span>

* <span data-ttu-id="ba6f6-670">成员访问表达式 `x.y` 推导名称 `y`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-670">A member access expression `x.y` infers the name `y`.</span></span>

* <span data-ttu-id="ba6f6-671">字典查找表达式 `x!y` 推导名称 `y`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-671">A dictionary lookup expression `x!y` infers the name `y`.</span></span>

* <span data-ttu-id="ba6f6-672">不带参数 `x()` 的调用或索引表达式将推断名称 `x`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-672">An invocation or index expression with no arguments `x()` infers the name `x`.</span></span>

* <span data-ttu-id="ba6f6-673">XML 成员访问表达式 `x.<y>`、`x...<y>``x.@y` 将推断名称 `y`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-673">An XML member access expression `x.<y>`, `x...<y>`, `x.@y` infers the name `y`.</span></span>

* <span data-ttu-id="ba6f6-674">一个 XML 成员访问表达式，它是成员访问表达式的目标 `x.<y>.z` 推断 `z`的名称。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-674">An XML member access expression that is the target of a member access expression `x.<y>.z` infers the name `z`.</span></span>

* <span data-ttu-id="ba6f6-675">XML 成员访问表达式，它是没有参数的调用或索引表达式的目标 `x.<y>.z()` 将推断名称 `z`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-675">An XML member access expression that is the target of an invocation or index expression with no arguments `x.<y>.z()` infers the name `z`.</span></span>

* <span data-ttu-id="ba6f6-676">作为调用表达式或索引表达式的目标的 XML 成员访问表达式 `x.<y>(0)` 推导名称 `y`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-676">An XML member access expression that is the target of an invocation or index expression `x.<y>(0)` infers the name `y`.</span></span>

<span data-ttu-id="ba6f6-677">初始值设定项被解释为推断名称的表达式赋值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-677">The initializer is interpreted as an assignment of the expression to the inferred name.</span></span> <span data-ttu-id="ba6f6-678">例如，以下初始值设定项是等效的：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-678">For example, the following initializers are equivalent:</span></span>

```vb
Class Address
    Public Street As String
    Public City As String
    Public State As String
    Public ZIP As String
End Class

Class C1
    Sub Test(a As Address)
        Dim cityState1 = New With { .City = a.City, .State = a.State }
        Dim cityState2 = New With { a.City, a.State }
    End Sub
End Class
```

<span data-ttu-id="ba6f6-679">如果推理出的成员名称与类型的现有成员（如 `GetHashCode`）冲突，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-679">If a member name is inferred that conflicts with an existing member of the type, such as `GetHashCode`, then a compile time error occurs.</span></span> <span data-ttu-id="ba6f6-680">与常规成员初始值设定项不同，匿名对象创建表达式不允许成员初始值设定项具有循环引用，或者在初始化之前引用成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-680">Unlike regular member initializers, anonymous object-creation expressions do not allow member initializers to have circular references, or to refer to a member before it has been initialized.</span></span> <span data-ttu-id="ba6f6-681">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-681">For example:</span></span>

```vb
Module Test
    Sub Main()
        ' Error: Circular references
        Dim x = New With { .a = .b, .b = .a }

        ' Error: Referring to .b before it has been assigned to
        Dim y = New With { .a = .b, .b = 10 }

        ' Error: Referring to .a before it has been assigned to
        Dim z = New With { .a = .a }
    End Sub
End Module
```

<span data-ttu-id="ba6f6-682">如果两个匿名类创建表达式出现在同一方法中并生成相同的结果形状（如果属性顺序、属性名称和属性类型都匹配），则它们将引用同一个匿名类。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-682">If two anonymous class creation expressions occur within the same method and yield the same resulting shape -- if the property order, property names, and property types all match -- they will both refer to the same anonymous class.</span></span> <span data-ttu-id="ba6f6-683">具有初始值设定项的实例或共享成员变量的方法作用域是在其中初始化变量的构造函数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-683">The method scope of an instance or shared member variable with an initializer is the constructor in which the variable is initialized.</span></span>

<span data-ttu-id="ba6f6-684">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-684">__Note.__</span></span> <span data-ttu-id="ba6f6-685">编译器可能会选择进一步统一匿名类型（如在程序集级别上），但此时不会依赖这种情况。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-685">It is possible that a compiler may choose to unify anonymous types further, such as at the assembly level, but this cannot be relied upon at this time.</span></span>


## <a name="cast-expressions"></a><span data-ttu-id="ba6f6-686">强制转换表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-686">Cast Expressions</span></span>

<span data-ttu-id="ba6f6-687">强制转换表达式将表达式强制转换为给定类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-687">A cast expression coerces an expression to a given type.</span></span> <span data-ttu-id="ba6f6-688">特定强制转换关键字将表达式强制转换为基元类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-688">Specific cast keywords coerce expressions into the primitive types.</span></span> <span data-ttu-id="ba6f6-689">三个常规强制转换关键字 `CType`、`TryCast` 和 `DirectCast`，将表达式强制转换为类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-689">Three general cast keywords, `CType`, `TryCast` and `DirectCast`, coerce an expression into a type.</span></span>

```antlr
CastExpression
    : 'DirectCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'TryCast' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | 'CType' OpenParenthesis Expression Comma TypeName CloseParenthesis
    | CastTarget OpenParenthesis Expression CloseParenthesis
    ;

CastTarget
    : 'CBool' | 'CByte' | 'CChar'  | 'CDate'  | 'CDec' | 'CDbl' | 'CInt'
    | 'CLng'  | 'CObj'  | 'CSByte' | 'CShort' | 'CSng' | 'CStr' | 'CUInt'
    | 'CULng' | 'CUShort'
    ;
```

<span data-ttu-id="ba6f6-690">`DirectCast` 和 `TryCast` 具有特殊行为。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-690">`DirectCast` and `TryCast` have special behaviors.</span></span> <span data-ttu-id="ba6f6-691">因此，它们仅支持本机转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-691">Because of this, they only support native conversions.</span></span> <span data-ttu-id="ba6f6-692">此外，`TryCast` 表达式中的目标类型不能是值类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-692">Additionally, the target type in a `TryCast` expression cannot be a value type.</span></span> <span data-ttu-id="ba6f6-693">使用 `DirectCast` 或 `TryCast` 时，不考虑用户定义的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-693">User-defined conversion operators are not considered when `DirectCast` or `TryCast` is used.</span></span> <span data-ttu-id="ba6f6-694">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-694">(__Note.__</span></span> <span data-ttu-id="ba6f6-695">`DirectCast` 和 `TryCast` 支持的转换集受到限制，因为它们实现了 "本机 CLR" 转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-695">The conversion set that `DirectCast` and `TryCast` support are restricted because they implement "native CLR" conversions.</span></span> <span data-ttu-id="ba6f6-696">`DirectCast` 的目的是提供 "取消装箱" 指令的功能，而 `TryCast` 的目的是提供 "isinst" 指令的功能。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-696">The purpose of `DirectCast` is to provide the functionality of the "unbox" instruction, while the purpose of `TryCast` is to provide the functionality of the "isinst" instruction.</span></span> <span data-ttu-id="ba6f6-697">由于它们映射到 CLR 说明，因此不能直接支持的 CLR 不支持的转换将会违背预期目的。）</span><span class="sxs-lookup"><span data-stu-id="ba6f6-697">Since they map onto CLR instructions, supporting conversions not directly supported by the CLR would defeat the intended purpose.)</span></span>

<span data-ttu-id="ba6f6-698">`DirectCast` 将类型化为 `Object` 的表达式转换为与 `CType`不同的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-698">`DirectCast` converts expressions that are typed as `Object` differently than `CType`.</span></span> <span data-ttu-id="ba6f6-699">转换其运行时类型为基元值类型 `Object` 类型的表达式时，如果指定的类型与表达式的运行时类型不相同，则 `DirectCast` 引发 `System.InvalidCastException` 异常，如果表达式的计算结果为 `Nothing`，则引发 `System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-699">When converting an expression of type `Object` whose run-time type is a primitive value type, `DirectCast` throws a `System.InvalidCastException` exception if the specified type is not the same as the run-time type of the expression or a `System.NullReferenceException` if the expression evaluates to `Nothing`.</span></span> <span data-ttu-id="ba6f6-700">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-700">(__Note.__</span></span> <span data-ttu-id="ba6f6-701">如上所述，当 `Object`表达式的类型时，`DirectCast` 直接映射到 CLR 指令 "取消装箱"。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-701">As noted above, `DirectCast` maps directly onto the CLR instruction "unbox" when the type of the expression is `Object`.</span></span> <span data-ttu-id="ba6f6-702">与此相反，`CType` 会转换为运行时帮助器来执行转换，以便能够支持基元类型之间的转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-702">In contrast, `CType` turns into a call to a runtime helper to do the conversion so that conversions between primitive types can be supported.</span></span> <span data-ttu-id="ba6f6-703">如果将 `Object` 表达式转换为基元值类型，并且实际实例的类型与目标类型匹配，则 `DirectCast` 的速度将明显快于 `CType`。）</span><span class="sxs-lookup"><span data-stu-id="ba6f6-703">In the case when an `Object` expression is being converted to a primitive value type and the type of the actual instance match the target type, `DirectCast` will be significantly faster than `CType`.)</span></span>

<span data-ttu-id="ba6f6-704">如果表达式无法转换为目标类型，`TryCast` 将转换表达式，但不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-704">`TryCast` converts expressions but does not throw an exception if the expression cannot be converted to the target type.</span></span> <span data-ttu-id="ba6f6-705">相反，如果表达式不能在运行时转换，`TryCast` 将导致 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-705">Instead, `TryCast` will result in `Nothing` if the expression cannot be converted at runtime.</span></span> <span data-ttu-id="ba6f6-706">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-706">(__Note.__</span></span> <span data-ttu-id="ba6f6-707">如上所述，`TryCast` 直接映射到 CLR 指令 "isinst"。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-707">As noted above, `TryCast` maps directly onto the CLR instruction "isinst".</span></span> <span data-ttu-id="ba6f6-708">通过将类型检查和转换合并为单个操作，`TryCast` 比 `TypeOf ... Is` 然后 `CType`更便宜。）</span><span class="sxs-lookup"><span data-stu-id="ba6f6-708">By combining the type check and the conversion into a single operation, `TryCast` can be cheaper than doing a `TypeOf ... Is` and then a `CType`.)</span></span>

<span data-ttu-id="ba6f6-709">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-709">For example:</span></span>

```vb
Interface ITest
    Sub Test()
End Interface

Module Test
    Sub Convert(o As Object)
        Dim i As ITest = TryCast(o, ITest)

        If i IsNot Nothing Then
            i.Test()
        End If
    End Sub
End Module
```

<span data-ttu-id="ba6f6-710">如果不存在从表达式的类型到指定类型的转换，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-710">If no conversion exists from the type of the expression to the specified type, a compile-time error occurs.</span></span> <span data-ttu-id="ba6f6-711">否则，表达式归类为值，结果是转换生成的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-711">Otherwise, the expression is classified as a value and the result is the value produced by the conversion.</span></span>


## <a name="operator-expressions"></a><span data-ttu-id="ba6f6-712">运算符表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-712">Operator Expressions</span></span>

<span data-ttu-id="ba6f6-713">有两种类型的运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-713">There are two kinds of operators.</span></span> <span data-ttu-id="ba6f6-714">*一元运算符*采用一个操作数并使用前缀表示法（例如 `-x`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-714">*Unary operators* take one operand and use prefix notation (for example, `-x`).</span></span> <span data-ttu-id="ba6f6-715">*二元运算符*采用两个操作数并使用中缀表示法（例如 `x + y`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-715">*Binary operators* take two operands and use infix notation (for example, `x + y`).</span></span> <span data-ttu-id="ba6f6-716">除了始终导致 `Boolean`的关系运算符以外，为特定类型定义的运算符将导致该类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-716">With the exception of the relational operators, which always result in `Boolean`, an operator defined for a particular type results in that type.</span></span> <span data-ttu-id="ba6f6-717">运算符的操作数必须始终归类为一个值;运算符表达式的结果分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-717">The operands to an operator must always be classified as a value; the result of an operator expression is classified as a value.</span></span>

```antlr
OperatorExpression
    : ArithmeticOperatorExpression
    | RelationalOperatorExpression
    | LikeOperatorExpression
    | ConcatenationOperatorExpression
    | ShortCircuitLogicalOperatorExpression
    | LogicalOperatorExpression
    | ShiftOperatorExpression
    | AwaitOperatorExpression
    ;
```

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="ba6f6-718">运算符优先级和关联性</span><span class="sxs-lookup"><span data-stu-id="ba6f6-718">Operator Precedence and Associativity</span></span>

<span data-ttu-id="ba6f6-719">当表达式包含多个二元运算符时，运算符的*优先级*控制单个二元运算符的计算顺序。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-719">When an expression contains multiple binary operators, the *precedence* of the operators controls the order in which the individual binary operators are evaluated.</span></span> <span data-ttu-id="ba6f6-720">例如，表达式 `x + y * z` 相当于计算 `x + (y * z)`，因为 `*` 运算符的优先级高于 `+` 运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-720">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span> <span data-ttu-id="ba6f6-721">下表按优先级的降序顺序列出了二元运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-721">The following table lists the binary operators in descending order of precedence:</span></span>


| <span data-ttu-id="ba6f6-722">__类别__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-722">__Category__</span></span>     | <span data-ttu-id="ba6f6-723">__运算符__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-723">__Operators__</span></span>                                          | 
|------------------|--------------------------------------------------------|
| <span data-ttu-id="ba6f6-724">基本</span><span class="sxs-lookup"><span data-stu-id="ba6f6-724">Primary</span></span>          | <span data-ttu-id="ba6f6-725">所有非运算符表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-725">All non-operator expressions</span></span>                           |
| <span data-ttu-id="ba6f6-726">Await</span><span class="sxs-lookup"><span data-stu-id="ba6f6-726">Await</span></span>            | `Await`                                                |
| <span data-ttu-id="ba6f6-727">求幂</span><span class="sxs-lookup"><span data-stu-id="ba6f6-727">Exponentiation</span></span>   | `^`                                                    |
| <span data-ttu-id="ba6f6-728">一元求反</span><span class="sxs-lookup"><span data-stu-id="ba6f6-728">Unary negation</span></span>   | <span data-ttu-id="ba6f6-729">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-729">`+`, `-`</span></span>                                               |
| <span data-ttu-id="ba6f6-730">乘法</span><span class="sxs-lookup"><span data-stu-id="ba6f6-730">Multiplicative</span></span>   | <span data-ttu-id="ba6f6-731">`*`, `/`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-731">`*`, `/`</span></span>                                               |
| <span data-ttu-id="ba6f6-732">整数除法</span><span class="sxs-lookup"><span data-stu-id="ba6f6-732">Integer division</span></span> | `\`                                                    |
| <span data-ttu-id="ba6f6-733">取模</span><span class="sxs-lookup"><span data-stu-id="ba6f6-733">Modulus</span></span>          | `Mod`                                                  |
| <span data-ttu-id="ba6f6-734">加法</span><span class="sxs-lookup"><span data-stu-id="ba6f6-734">Additive</span></span>         | <span data-ttu-id="ba6f6-735">`+`, `-`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-735">`+`, `-`</span></span>                                               |
| <span data-ttu-id="ba6f6-736">串联</span><span class="sxs-lookup"><span data-stu-id="ba6f6-736">Concatenation</span></span>    | `&`                                                    |
| <span data-ttu-id="ba6f6-737">Shift</span><span class="sxs-lookup"><span data-stu-id="ba6f6-737">Shift</span></span>            | <span data-ttu-id="ba6f6-738">`<<`, `>>`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-738">`<<`, `>>`</span></span>                                             |
| <span data-ttu-id="ba6f6-739">关系</span><span class="sxs-lookup"><span data-stu-id="ba6f6-739">Relational</span></span>       | <span data-ttu-id="ba6f6-740">`=`、`<>`、`<`、`>`、`<=`、`>=`、`Like`、`Is`、`IsNot`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span></span> |
| <span data-ttu-id="ba6f6-741">逻辑“非”</span><span class="sxs-lookup"><span data-stu-id="ba6f6-741">Logical NOT</span></span>      | `Not`                                                  |
| <span data-ttu-id="ba6f6-742">逻辑“与”</span><span class="sxs-lookup"><span data-stu-id="ba6f6-742">Logical AND</span></span>      | <span data-ttu-id="ba6f6-743">`And`, `AndAlso`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-743">`And`, `AndAlso`</span></span>                                       |
| <span data-ttu-id="ba6f6-744">逻辑“或”</span><span class="sxs-lookup"><span data-stu-id="ba6f6-744">Logical OR</span></span>       | <span data-ttu-id="ba6f6-745">`Or`, `OrElse`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-745">`Or`, `OrElse`</span></span>                                         |
| <span data-ttu-id="ba6f6-746">逻辑“异或”</span><span class="sxs-lookup"><span data-stu-id="ba6f6-746">Logical XOR</span></span>      | `Xor`                                                  |

<span data-ttu-id="ba6f6-747">如果表达式包含两个具有相同优先级的运算符，则运算符的*关联*性将控制运算的执行顺序。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-747">When an expression contains two operators with the same precedence, the *associativity* of the operators controls the order in which the operations are performed.</span></span> <span data-ttu-id="ba6f6-748">所有二元运算符都是左结合运算符，这意味着运算从左至右执行。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-748">All binary operators are left-associative, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="ba6f6-749">优先级和结合性可以使用带括号的表达式进行控制。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-749">Precedence and associativity can be controlled using parenthetical expressions.</span></span>

### <a name="object-operands"></a><span data-ttu-id="ba6f6-750">对象操作数</span><span class="sxs-lookup"><span data-stu-id="ba6f6-750">Object Operands</span></span>

<span data-ttu-id="ba6f6-751">除每个运算符支持的常规类型外，所有运算符都支持 `Object`类型的操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-751">In addition to the regular types supported by each operator, all operators support operands of type `Object`.</span></span> <span data-ttu-id="ba6f6-752">应用于 `Object` 操作数的运算符的处理方式类似于对 `Object` 值进行的方法调用：可以选择后期绑定方法调用，在这种情况下，操作数的运行时类型（而不是编译时类型）将确定操作的有效性和类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-752">Operators applied to `Object` operands are handled similarly to method calls made on `Object` values: a late-bound method call might be chosen, in which case the run-time type of the operands, rather than the compile-time type, determines the validity and type of the operation.</span></span> <span data-ttu-id="ba6f6-753">如果通过编译环境或 `Option Strict`指定严格语义，则具有类型 `Object` 的操作数的任何运算符都将导致编译时错误，但 `TypeOf...Is`、`Is` 和 `IsNot` 运算符除外。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-753">If strict semantics are specified by the compilation environment or by `Option Strict`, any operators with operands of type `Object` cause a compile-time error, except for the `TypeOf...Is`, `Is` and `IsNot` operators.</span></span>

<span data-ttu-id="ba6f6-754">当运算符解析确定应该后期绑定执行操作时，如果操作数的运行时类型是运算符支持的类型，则操作的结果是将运算符应用于操作数类型的结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-754">When operator resolution determines that an operation should be performed late-bound, the outcome of the operation is the result of applying the operator to the operand types if the run-time types of the operands are types that are supported by the operator.</span></span> <span data-ttu-id="ba6f6-755">`Nothing` 的值被视为二元运算符表达式中另一操作数的类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-755">The value `Nothing` is treated as the default value of the type of the other operand in a binary operator expression.</span></span> <span data-ttu-id="ba6f6-756">在一元运算符表达式中，如果两个操作数都 `Nothing` 在二元运算符表达式中，则该运算的类型为 `Integer` 或运算符的唯一结果类型（如果运算符不会导致 `Integer`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-756">In a unary operator expression, or if both operands are `Nothing` in a binary operator expression, the type of the operation is `Integer` or the only result type of the operator, if the operator does not result in `Integer`.</span></span> <span data-ttu-id="ba6f6-757">然后，操作的结果将始终强制转换回 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-757">The result of the operation is always then cast back to `Object`.</span></span> <span data-ttu-id="ba6f6-758">如果操作数类型没有有效的运算符，则会引发 `System.InvalidCastException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-758">If the operand types have no valid operator, a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-759">在运行时执行转换，而不考虑它们是否为隐式或显式的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-759">Conversions at run time are done without regard to whether they are implicit or explicit.</span></span>

<span data-ttu-id="ba6f6-760">如果数值二元运算的结果将生成溢出异常（无论是否打开还是关闭整数溢出检查），则结果类型将提升为下一较大的数值类型（如果可能）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-760">If the result of a numeric binary operation would produce an overflow exception (regardless of whether integer overflow checking is on or off), then the result type is promoted to the next wider numeric type, if possible.</span></span> <span data-ttu-id="ba6f6-761">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-761">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

<span data-ttu-id="ba6f6-762">它打印以下结果：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-762">It prints the following result:</span></span>

```console
System.Int16 = 512
```

<span data-ttu-id="ba6f6-763">如果没有更大的数值类型可用于保存数字，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-763">If no wider numeric type is available to hold the number, a `System.OverflowException` exception is thrown.</span></span>

### <a name="operator-resolution"></a><span data-ttu-id="ba6f6-764">操作员解析</span><span class="sxs-lookup"><span data-stu-id="ba6f6-764">Operator Resolution</span></span>

<span data-ttu-id="ba6f6-765">给定运算符类型和操作数集，运算符解析将确定要用于操作数的运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-765">Given an operator type and a set of operands, operator resolution determines which operator to use for the operands.</span></span> <span data-ttu-id="ba6f6-766">解析运算符时，将首先考虑用户定义的运算符，并使用以下步骤：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-766">When resolving operators, user-defined operators will be considered first, using the following steps:</span></span>

1. <span data-ttu-id="ba6f6-767">首先，收集所有候选运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-767">First, all of the candidate operators are collected.</span></span> <span data-ttu-id="ba6f6-768">候选运算符是源类型中特定运算符类型的所有用户定义的运算符，以及目标类型中特定类型的所有用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-768">The candidate operators are all of the user-defined operators of the particular operator type in the source type and all of the user-defined operators of the particular type in the target type.</span></span> <span data-ttu-id="ba6f6-769">如果源类型与目标类型相关，则仅将常用运算符视为一次。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-769">If the source type and destination type are related, common operators are only considered once.</span></span>

2. <span data-ttu-id="ba6f6-770">然后，将重载决策应用于运算符和操作数，以选择最具体的运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-770">Then, overload resolution is applied to the operators and operands to select the most specific operator.</span></span> <span data-ttu-id="ba6f6-771">对于二元运算符，这可能会导致后期绑定调用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-771">In the case of binary operators, this may result in a late-bound call.</span></span>

<span data-ttu-id="ba6f6-772">为类型 `T?`收集候选运算符时，将改用 `T` 类型的运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-772">When collecting the candidate operators for a type `T?`, the operators of type `T` are used instead.</span></span> <span data-ttu-id="ba6f6-773">还会提升仅涉及不可为 null 的值类型的任何 `T`用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-773">Any of `T`'s user-defined operators that involve only non-nullable value types are also lifted.</span></span> <span data-ttu-id="ba6f6-774">提升运算符使用任意值类型的可以为 null 的版本，但 `IsTrue` 和 `IsFalse` （必须 `Boolean`）的返回类型除外。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-774">A lifted operator uses the nullable version of any value types, with the exception the return types of `IsTrue` and `IsFalse` (which must be `Boolean`).</span></span> <span data-ttu-id="ba6f6-775">通过将操作数转换为不可为 null 的版本，然后计算用户定义的运算符，然后将结果类型转换为可以为 null 的版本，来计算提升运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-775">Lifted operators are evaluated by converting the operands to their non-nullable version, then evaluating the user-defined operator and then converting the result type to its nullable version.</span></span> <span data-ttu-id="ba6f6-776">如果 `Nothing`网操作数，则表达式的结果为类型 `Nothing` 为结果类型的可以为 null 的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-776">If ether operand is `Nothing`, the result of the expression is a value of `Nothing` typed as the nullable version of the result type.</span></span> <span data-ttu-id="ba6f6-777">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-777">For example:</span></span>

```vb
Structure T
    ...
End Structure

Structure S
    Public Shared Operator +(ByVal op1 As S, ByVal op2 As T) As T
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As S?
        Dim y, z As T?

        ' Valid, as S + T = T is lifted to S? + T? = T?
        z = x + y 
    End Sub
End Module
```

<span data-ttu-id="ba6f6-778">如果运算符为二元运算符，并且其中一个操作数为引用类型，则也会提升运算符，但任何绑定到该运算符都将产生错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-778">If the operator is a binary operator and one of the operands is reference type, the operator is also lifted, but any binding to the operator produces an error.</span></span> <span data-ttu-id="ba6f6-779">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-779">For example:</span></span>

```vb
Structure S1
    Public F1 As Integer

    Public Shared Operator +(left As S1, right As String) As S1
       ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim a? As S1
        Dim s As String
        
        ' Error: '+' is not defined for S1? and String
        a = a + s
    End Sub
End Module
```

<span data-ttu-id="ba6f6-780">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-780">__Note.__</span></span> <span data-ttu-id="ba6f6-781">此规则存在的原因是我们是否希望在未来版本中添加 null 传播引用类型，在这种情况下，这两个类型之间的二元运算符的行为将会更改。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-781">This rule exists because there has been consideration whether we wish to add null-propagating reference types in a future version, in which case the behavior in the case of binary operators between the two types would change.</span></span>

<span data-ttu-id="ba6f6-782">与转换一样，用户定义的运算符始终优于提升运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-782">As with conversions, user-defined operators are always preferred over lifted operators.</span></span>

<span data-ttu-id="ba6f6-783">解析重载运算符时，Visual Basic 中定义的类与在其他语言中定义的类之间可能存在差异：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-783">When resolving overloaded operators, there may be differences between classes defined in Visual Basic and those defined in other languages:</span></span>

* <span data-ttu-id="ba6f6-784">在其他语言中，可以将 `Not`、`And`和 `Or` 同时作为逻辑运算符和位运算符进行重载。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-784">In other languages, `Not`, `And`, and `Or` may be overloaded both as logical operators and bitwise operators.</span></span> <span data-ttu-id="ba6f6-785">从外部程序集导入时，任何一个窗体都接受为这些运算符的有效重载。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-785">Upon import from an external assembly, either form is accepted as a valid overload for these operators.</span></span> <span data-ttu-id="ba6f6-786">但对于定义逻辑运算符和位运算符的类型，只考虑按位实现。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-786">However, for a type which defines both logical and bitwise operators, only the bitwise implementation will be considered.</span></span>

* <span data-ttu-id="ba6f6-787">在其他语言中，`>>` 和 `<<` 可能会重载为有符号运算符和无符号运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-787">In other languages, `>>` and `<<` may be overloaded both as signed operators and unsigned operators.</span></span> <span data-ttu-id="ba6f6-788">从外部程序集导入时，任何一个窗体都作为有效重载接受。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-788">Upon import from an external assembly, either form is accepted as a valid overload.</span></span> <span data-ttu-id="ba6f6-789">但对于定义有符号和无符号运算符的类型，将只考虑已签名的实现。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-789">However, for a type which defines both signed and unsigned operators, only the signed implementation will be considered.</span></span>

* <span data-ttu-id="ba6f6-790">如果没有用户定义的运算符是最特定于操作数的，则将考虑内部运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-790">If no user-defined operator is most specific to the operands, then intrinsic operators will be considered.</span></span> <span data-ttu-id="ba6f6-791">如果没有为操作数定义内部运算符，并且其中一个操作数具有类型 Object，则该运算符将解析后期绑定;否则，将产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-791">If no intrinsic operator is defined for the operands and either operand has type Object then the operator will be resolved late-bound; otherwise,  a compile-time error results.</span></span>

<span data-ttu-id="ba6f6-792">在 Visual Basic 的以前版本中，如果恰好有一个类型为 Object 的操作数，并且没有适用的用户定义运算符，并且没有适用的内部运算符，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-792">In prior versions of Visual Basic, if there was exactly one operand of type Object, and no applicable user-defined operators, and no applicable intrinsic operators, then it was an error.</span></span> <span data-ttu-id="ba6f6-793">从 Visual Basic 11 开始，现在已解析后期绑定。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-793">As of Visual Basic 11, it is now resolved late-bound.</span></span> <span data-ttu-id="ba6f6-794">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-794">For example:</span></span>

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

<span data-ttu-id="ba6f6-795">具有内部运算符的类型 `T` 也为 `T?`定义同一运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-795">A type `T` that has an intrinsic operator also defines that same operator for `T?`.</span></span> <span data-ttu-id="ba6f6-796">`T?` 运算符的结果将与 `T`相同，不同之处在于，如果其中一个操作数为 `Nothing`，则将 `Nothing` 运算符的结果（即，传播 null 值）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-796">The result of the operator on `T?` will be the same as for `T`, except that if either operand is `Nothing`, the result of the operator will be `Nothing` (i.e. the null value is propagated).</span></span> <span data-ttu-id="ba6f6-797">为了解析操作的类型，会从具有这些类型的任何操作数中删除 `?`，确定操作的类型，并且如果任何操作数为可为 null 的值类型，则将 `?` 添加到操作的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-797">For the purposes of resolving the type of an operation, the `?` is removed from any operands that have them, the type of the operation is determined, and a `?` is added to the type of the operation if any of the operands were nullable value types.</span></span> <span data-ttu-id="ba6f6-798">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-798">For example:</span></span>

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

<span data-ttu-id="ba6f6-799">每个运算符都列出为其定义的内部类型以及给定操作数类型执行的操作的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-799">Each operator lists the intrinsic types it is defined for and the type of the operation performed given the operand types.</span></span> <span data-ttu-id="ba6f6-800">内部操作类型的结果遵循以下常规规则：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-800">The result of type of a intrinsic operation follows these general rules:</span></span>

* <span data-ttu-id="ba6f6-801">如果所有操作数的类型相同，并且为该类型定义了运算符，则不发生转换并且使用该类型的运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-801">If all operands are of the same type, and the operator is defined for the type, then no conversion occurs and the operator for that type is used.</span></span>

* <span data-ttu-id="ba6f6-802">使用以下步骤转换其类型未为运算符定义的任何操作数，并使用新类型解析运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-802">Any operand whose type is not defined for the operator is converted using the following steps and the operator is resolved against the new types:</span></span>

  * <span data-ttu-id="ba6f6-803">操作数将转换为为运算符和操作数定义的最大的最大类型，并将其隐式转换为。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-803">The operand is converted to the next widest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="ba6f6-804">如果没有这样的类型，则操作数将转换为为运算符和操作数定义的下一个最小类型，并将其隐式转换为。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-804">If there is no such type, then the operand is converted to the next narrowest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="ba6f6-805">如果没有此类类型或无法进行转换，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-805">If there is no such type or the conversion cannot occur, a compile-time error occurs.</span></span>

* <span data-ttu-id="ba6f6-806">否则，操作数将转换为操作数类型的更宽，并使用该类型的运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-806">Otherwise, the operands are converted to the wider of the operand types and the operator for that type is used.</span></span> <span data-ttu-id="ba6f6-807">如果较窄的操作数类型无法隐式转换为更大的运算符类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-807">If the narrower operand type cannot be implicitly converted to the wider operator type, a compile-time error occurs.</span></span>

<span data-ttu-id="ba6f6-808">不过，尽管使用了这些常规规则，但在操作员的结果表中有很多特例。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-808">Despite these general rules, however, there are a number of special cases called out in the operator results tables.</span></span>

<span data-ttu-id="ba6f6-809">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-809">__Note.__</span></span> <span data-ttu-id="ba6f6-810">由于格式设置的原因，运算符类型表将预定义的名称缩写为它们的前两个字符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-810">For formatting reasons, the operator type tables abbreviate the predefined names to their first two characters.</span></span> <span data-ttu-id="ba6f6-811">因此 "By" 是 `Byte`的，"UI" `UInteger`，"St" `String`，等等。 "Err" 表示没有为给定的操作数类型定义操作。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-811">So "By" is `Byte`, "UI" is `UInteger`, "St" is `String`, etc. "Err" means that there is no operation defined for the given operand types.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="ba6f6-812">算术运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-812">Arithmetic Operators</span></span>

<span data-ttu-id="ba6f6-813">`*`、`/`、`\`、`^`、`Mod`、`+`和 `-` 运算符是*算术运算符*。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-813">The `*`, `/`, `\`, `^`, `Mod`, `+`, and `-` operators are the *arithmetic operators*.</span></span>

```antlr
ArithmeticOperatorExpression
    : UnaryPlusExpression
    | UnaryMinusExpression
    | AdditionOperatorExpression
    | SubtractionOperatorExpression
    | MultiplicationOperatorExpression
    | DivisionOperatorExpression
    | ModuloOperatorExpression
    | ExponentOperatorExpression
    ;
```

<span data-ttu-id="ba6f6-814">与运算的结果类型相比，浮点算术运算的精度可能更高。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-814">Floating-point arithmetic operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="ba6f6-815">例如，某些硬件体系结构支持比 `Double` 类型更大的范围和精度的 "扩展" 或 "long double" 浮点类型，并使用此更高精度的类型隐式执行所有浮点运算。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-815">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `Double` type, and implicitly perform all floating-point operations using this higher-precision type.</span></span> <span data-ttu-id="ba6f6-816">可以采用更少的精度来执行浮点运算，只需在性能方面产生较高的成本;Visual Basic 允许将更高精度的类型用于所有浮点运算，而不是要求实现使的性能和精度。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-816">Hardware architectures can be made to perform floating-point operations with less precision only at excessive cost in performance; rather than require an implementation to forfeit both performance and precision, Visual Basic allows the higher-precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="ba6f6-817">除了提供更精确的结果，这种情况很少会带来任何实实在在的影响。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-817">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="ba6f6-818">但是，在形式 `x * y / z`的表达式中，乘法产生的结果不在 `Double` 范围内，而后续除法会将临时结果返回到 `Double` 范围中，这种情况下，以较高范围的格式对表达式进行求值可能会导致生成有限的结果而不是无限大。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-818">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `Double` range, but the subsequent division brings the temporary result back into the `Double` range, the fact that the expression is evaluated in a higher-range format may cause a finite result to be produced instead of infinity.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="ba6f6-819">一元加号运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-819">Unary Plus Operator</span></span>

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

<span data-ttu-id="ba6f6-820">为 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`、`Single`、`Double`和 `Decimal` 类型定义了一元加运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-820">The unary plus operator is defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, and `Decimal` types.</span></span>

<span data-ttu-id="ba6f6-821">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-821">__Operation Type:__</span></span>


| <span data-ttu-id="ba6f6-822">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-822">__Bo__</span></span> | <span data-ttu-id="ba6f6-823">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-823">__SB__</span></span> | <span data-ttu-id="ba6f6-824">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-824">__By__</span></span> | <span data-ttu-id="ba6f6-825">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-825">__Sh__</span></span> | <span data-ttu-id="ba6f6-826">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-826">__US__</span></span> | <span data-ttu-id="ba6f6-827">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-827">__In__</span></span> | <span data-ttu-id="ba6f6-828">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-828">__UI__</span></span> | <span data-ttu-id="ba6f6-829">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-829">__Lo__</span></span> | <span data-ttu-id="ba6f6-830">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-830">__UL__</span></span> | <span data-ttu-id="ba6f6-831">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-831">__De__</span></span> | <span data-ttu-id="ba6f6-832">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-832">__Si__</span></span> | <span data-ttu-id="ba6f6-833">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-833">__Do__</span></span> | <span data-ttu-id="ba6f6-834">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-834">__Da__</span></span>  | <span data-ttu-id="ba6f6-835">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-835">__Ch__</span></span>  | <span data-ttu-id="ba6f6-836">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-836">__St__</span></span> | <span data-ttu-id="ba6f6-837">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-837">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ba6f6-838">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-838">Sh</span></span> | <span data-ttu-id="ba6f6-839">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-839">SB</span></span> | <span data-ttu-id="ba6f6-840">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-840">By</span></span> | <span data-ttu-id="ba6f6-841">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-841">Sh</span></span> | <span data-ttu-id="ba6f6-842">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-842">US</span></span> | <span data-ttu-id="ba6f6-843">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-843">In</span></span> | <span data-ttu-id="ba6f6-844">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-844">UI</span></span> | <span data-ttu-id="ba6f6-845">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-845">Lo</span></span> | <span data-ttu-id="ba6f6-846">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-846">UL</span></span> | <span data-ttu-id="ba6f6-847">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-847">De</span></span> | <span data-ttu-id="ba6f6-848">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-848">Si</span></span> | <span data-ttu-id="ba6f6-849">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-849">Do</span></span> | <span data-ttu-id="ba6f6-850">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-850">Err</span></span> | <span data-ttu-id="ba6f6-851">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-851">Err</span></span> | <span data-ttu-id="ba6f6-852">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-852">Do</span></span> | <span data-ttu-id="ba6f6-853">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-853">Ob</span></span> | 


### <a name="unary-minus-operator"></a><span data-ttu-id="ba6f6-854">一元减号运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-854">Unary Minus Operator</span></span>

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

<span data-ttu-id="ba6f6-855">为以下类型定义了一元减号运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-855">The unary minus operator is defined for the following types:</span></span>

<span data-ttu-id="ba6f6-856">`SByte`、 `Short`、 `Integer`和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-856">`SByte`, `Short`, `Integer`, and `Long`.</span></span> <span data-ttu-id="ba6f6-857">通过从零中减去操作数来计算结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-857">The result is computed by subtracting the operand from zero.</span></span> <span data-ttu-id="ba6f6-858">如果启用了整数溢出检查，且操作数的值为最大负值 `SByte`、`Short`、`Integer`或 `Long`，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-858">If integer overflow checking is on and the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-859">否则，如果操作数的值为最大负值 `SByte`、`Short`、`Integer`或 `Long`，则结果为相同的值，并且不报告溢出。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-859">Otherwise, if the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, the result is that same value, and the overflow is not reported.</span></span>

<span data-ttu-id="ba6f6-860">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-860">`Single` and `Double`.</span></span> <span data-ttu-id="ba6f6-861">结果是操作数的符号反转，其中包括值0和无限大。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-861">The result is the value of the operand with its sign inverted, including the values 0 and Infinity.</span></span> <span data-ttu-id="ba6f6-862">如果操作数为 NaN，则结果也为 NaN。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-862">If the operand is NaN, the result is also NaN.</span></span>

<span data-ttu-id="ba6f6-863">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-863">`Decimal`.</span></span> <span data-ttu-id="ba6f6-864">通过从零中减去操作数来计算结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-864">The result is computed by subtracting the operand from zero.</span></span>

<span data-ttu-id="ba6f6-865">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-865">__Operation Type:__</span></span>

| <span data-ttu-id="ba6f6-866">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-866">__Bo__</span></span> | <span data-ttu-id="ba6f6-867">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-867">__SB__</span></span> | <span data-ttu-id="ba6f6-868">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-868">__By__</span></span> | <span data-ttu-id="ba6f6-869">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-869">__Sh__</span></span> | <span data-ttu-id="ba6f6-870">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-870">__US__</span></span> | <span data-ttu-id="ba6f6-871">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-871">__In__</span></span> | <span data-ttu-id="ba6f6-872">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-872">__UI__</span></span> | <span data-ttu-id="ba6f6-873">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-873">__Lo__</span></span> | <span data-ttu-id="ba6f6-874">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-874">__UL__</span></span> | <span data-ttu-id="ba6f6-875">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-875">__De__</span></span> | <span data-ttu-id="ba6f6-876">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-876">__Si__</span></span> | <span data-ttu-id="ba6f6-877">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-877">__Do__</span></span> | <span data-ttu-id="ba6f6-878">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-878">__Da__</span></span>  | <span data-ttu-id="ba6f6-879">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-879">__Ch__</span></span>  | <span data-ttu-id="ba6f6-880">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-880">__St__</span></span> | <span data-ttu-id="ba6f6-881">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-881">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ba6f6-882">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-882">Sh</span></span> | <span data-ttu-id="ba6f6-883">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-883">SB</span></span> | <span data-ttu-id="ba6f6-884">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-884">Sh</span></span> | <span data-ttu-id="ba6f6-885">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-885">Sh</span></span> | <span data-ttu-id="ba6f6-886">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-886">In</span></span> | <span data-ttu-id="ba6f6-887">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-887">In</span></span> | <span data-ttu-id="ba6f6-888">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-888">Lo</span></span> | <span data-ttu-id="ba6f6-889">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-889">Lo</span></span> | <span data-ttu-id="ba6f6-890">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-890">De</span></span> | <span data-ttu-id="ba6f6-891">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-891">De</span></span> | <span data-ttu-id="ba6f6-892">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-892">Si</span></span> | <span data-ttu-id="ba6f6-893">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-893">Do</span></span> | <span data-ttu-id="ba6f6-894">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-894">Err</span></span> | <span data-ttu-id="ba6f6-895">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-895">Err</span></span> | <span data-ttu-id="ba6f6-896">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-896">Do</span></span> | <span data-ttu-id="ba6f6-897">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-897">Ob</span></span> | 


### <a name="addition-operator"></a><span data-ttu-id="ba6f6-898">加法运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-898">Addition Operator</span></span>

<span data-ttu-id="ba6f6-899">加法运算符计算两个操作数之和。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-899">The addition operator computes the sum of the two operands.</span></span>

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-900">为以下类型定义了加法运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-900">The addition operator is defined for the following types:</span></span>

* <span data-ttu-id="ba6f6-901">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ba6f6-902">如果启用了整数溢出检查，且总和超出了结果类型的范围，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-902">If integer overflow checking is on and the sum is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-903">否则，不会报告溢出，并且放弃结果的任何高序位。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-903">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="ba6f6-904">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-904">`Single` and `Double`.</span></span> <span data-ttu-id="ba6f6-905">Sum 根据 IEEE 754 算法的规则进行计算。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-905">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ba6f6-906">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-906">`Decimal`.</span></span> <span data-ttu-id="ba6f6-907">如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-907">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-908">如果结果值太小而无法表示为十进制格式，则结果为0。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-908">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="ba6f6-909">`String`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-909">`String`.</span></span> <span data-ttu-id="ba6f6-910">这两个 `String` 操作数连接在一起。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-910">The two `String` operands are concatenated together.</span></span>

* <span data-ttu-id="ba6f6-911">`Date`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-911">`Date`.</span></span> <span data-ttu-id="ba6f6-912">`System.DateTime` 类型定义重载加法运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-912">The `System.DateTime` type defines overloaded addition operators.</span></span> <span data-ttu-id="ba6f6-913">由于 `System.DateTime` 等效于内部 `Date` 类型，因此，`Date` 类型也提供这些运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-913">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="ba6f6-914">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-914">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-915">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-915">__Bo__</span></span> | <span data-ttu-id="ba6f6-916">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-916">__SB__</span></span> | <span data-ttu-id="ba6f6-917">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-917">__By__</span></span> | <span data-ttu-id="ba6f6-918">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-918">__Sh__</span></span> | <span data-ttu-id="ba6f6-919">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-919">__US__</span></span> | <span data-ttu-id="ba6f6-920">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-920">__In__</span></span> | <span data-ttu-id="ba6f6-921">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-921">__UI__</span></span> | <span data-ttu-id="ba6f6-922">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-922">__Lo__</span></span> | <span data-ttu-id="ba6f6-923">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-923">__UL__</span></span> | <span data-ttu-id="ba6f6-924">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-924">__De__</span></span> | <span data-ttu-id="ba6f6-925">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-925">__Si__</span></span> | <span data-ttu-id="ba6f6-926">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-926">__Do__</span></span> | <span data-ttu-id="ba6f6-927">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-927">__Da__</span></span>  | <span data-ttu-id="ba6f6-928">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-928">__Ch__</span></span>  | <span data-ttu-id="ba6f6-929">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-929">__St__</span></span> | <span data-ttu-id="ba6f6-930">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-930">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ba6f6-931">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-931">__Bo__</span></span> | <span data-ttu-id="ba6f6-932">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-932">Sh</span></span> | <span data-ttu-id="ba6f6-933">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-933">SB</span></span> | <span data-ttu-id="ba6f6-934">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-934">Sh</span></span> | <span data-ttu-id="ba6f6-935">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-935">Sh</span></span> | <span data-ttu-id="ba6f6-936">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-936">In</span></span> | <span data-ttu-id="ba6f6-937">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-937">In</span></span> | <span data-ttu-id="ba6f6-938">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-938">Lo</span></span> | <span data-ttu-id="ba6f6-939">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-939">Lo</span></span> | <span data-ttu-id="ba6f6-940">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-940">De</span></span> | <span data-ttu-id="ba6f6-941">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-941">De</span></span> | <span data-ttu-id="ba6f6-942">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-942">Si</span></span> | <span data-ttu-id="ba6f6-943">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-943">Do</span></span> | <span data-ttu-id="ba6f6-944">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-944">Err</span></span> | <span data-ttu-id="ba6f6-945">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-945">Err</span></span> | <span data-ttu-id="ba6f6-946">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-946">Do</span></span> | <span data-ttu-id="ba6f6-947">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-947">Ob</span></span> | 
| <span data-ttu-id="ba6f6-948">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-948">__SB__</span></span> |    | <span data-ttu-id="ba6f6-949">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-949">SB</span></span> | <span data-ttu-id="ba6f6-950">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-950">Sh</span></span> | <span data-ttu-id="ba6f6-951">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-951">Sh</span></span> | <span data-ttu-id="ba6f6-952">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-952">In</span></span> | <span data-ttu-id="ba6f6-953">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-953">In</span></span> | <span data-ttu-id="ba6f6-954">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-954">Lo</span></span> | <span data-ttu-id="ba6f6-955">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-955">Lo</span></span> | <span data-ttu-id="ba6f6-956">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-956">De</span></span> | <span data-ttu-id="ba6f6-957">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-957">De</span></span> | <span data-ttu-id="ba6f6-958">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-958">Si</span></span> | <span data-ttu-id="ba6f6-959">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-959">Do</span></span> | <span data-ttu-id="ba6f6-960">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-960">Err</span></span> | <span data-ttu-id="ba6f6-961">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-961">Err</span></span> | <span data-ttu-id="ba6f6-962">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-962">Do</span></span> | <span data-ttu-id="ba6f6-963">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-963">Ob</span></span> | 
| <span data-ttu-id="ba6f6-964">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-964">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-965">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-965">By</span></span> | <span data-ttu-id="ba6f6-966">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-966">Sh</span></span> | <span data-ttu-id="ba6f6-967">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-967">US</span></span> | <span data-ttu-id="ba6f6-968">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-968">In</span></span> | <span data-ttu-id="ba6f6-969">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-969">UI</span></span> | <span data-ttu-id="ba6f6-970">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-970">Lo</span></span> | <span data-ttu-id="ba6f6-971">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-971">UL</span></span> | <span data-ttu-id="ba6f6-972">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-972">De</span></span> | <span data-ttu-id="ba6f6-973">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-973">Si</span></span> | <span data-ttu-id="ba6f6-974">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-974">Do</span></span> | <span data-ttu-id="ba6f6-975">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-975">Err</span></span> | <span data-ttu-id="ba6f6-976">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-976">Err</span></span> | <span data-ttu-id="ba6f6-977">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-977">Do</span></span> | <span data-ttu-id="ba6f6-978">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-978">Ob</span></span> | 
| <span data-ttu-id="ba6f6-979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-979">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-980">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-980">Sh</span></span> | <span data-ttu-id="ba6f6-981">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-981">In</span></span> | <span data-ttu-id="ba6f6-982">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-982">In</span></span> | <span data-ttu-id="ba6f6-983">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-983">Lo</span></span> | <span data-ttu-id="ba6f6-984">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-984">Lo</span></span> | <span data-ttu-id="ba6f6-985">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-985">De</span></span> | <span data-ttu-id="ba6f6-986">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-986">De</span></span> | <span data-ttu-id="ba6f6-987">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-987">Si</span></span> | <span data-ttu-id="ba6f6-988">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-988">Do</span></span> | <span data-ttu-id="ba6f6-989">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-989">Err</span></span> | <span data-ttu-id="ba6f6-990">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-990">Err</span></span> | <span data-ttu-id="ba6f6-991">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-991">Do</span></span> | <span data-ttu-id="ba6f6-992">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-992">Ob</span></span> | 
| <span data-ttu-id="ba6f6-993">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-993">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-994">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-994">US</span></span> | <span data-ttu-id="ba6f6-995">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-995">In</span></span> | <span data-ttu-id="ba6f6-996">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-996">UI</span></span> | <span data-ttu-id="ba6f6-997">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-997">Lo</span></span> | <span data-ttu-id="ba6f6-998">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-998">UL</span></span> | <span data-ttu-id="ba6f6-999">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-999">De</span></span> | <span data-ttu-id="ba6f6-1000">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1000">Si</span></span> | <span data-ttu-id="ba6f6-1001">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1001">Do</span></span> | <span data-ttu-id="ba6f6-1002">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1002">Err</span></span> | <span data-ttu-id="ba6f6-1003">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1003">Err</span></span> | <span data-ttu-id="ba6f6-1004">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1004">Do</span></span> | <span data-ttu-id="ba6f6-1005">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1005">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1006">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1006">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-1007">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1007">In</span></span> | <span data-ttu-id="ba6f6-1008">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1008">Lo</span></span> | <span data-ttu-id="ba6f6-1009">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1009">Lo</span></span> | <span data-ttu-id="ba6f6-1010">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1010">De</span></span> | <span data-ttu-id="ba6f6-1011">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1011">De</span></span> | <span data-ttu-id="ba6f6-1012">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1012">Si</span></span> | <span data-ttu-id="ba6f6-1013">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1013">Do</span></span> | <span data-ttu-id="ba6f6-1014">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1014">Err</span></span> | <span data-ttu-id="ba6f6-1015">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1015">Err</span></span> | <span data-ttu-id="ba6f6-1016">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1016">Do</span></span> | <span data-ttu-id="ba6f6-1017">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1017">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1018">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1018">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1019">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1019">UI</span></span> | <span data-ttu-id="ba6f6-1020">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1020">Lo</span></span> | <span data-ttu-id="ba6f6-1021">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1021">UL</span></span> | <span data-ttu-id="ba6f6-1022">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1022">De</span></span> | <span data-ttu-id="ba6f6-1023">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1023">Si</span></span> | <span data-ttu-id="ba6f6-1024">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1024">Do</span></span> | <span data-ttu-id="ba6f6-1025">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1025">Err</span></span> | <span data-ttu-id="ba6f6-1026">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1026">Err</span></span> | <span data-ttu-id="ba6f6-1027">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1027">Do</span></span> | <span data-ttu-id="ba6f6-1028">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1028">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1029">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1029">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1030">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1030">Lo</span></span> | <span data-ttu-id="ba6f6-1031">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1031">De</span></span> | <span data-ttu-id="ba6f6-1032">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1032">De</span></span> | <span data-ttu-id="ba6f6-1033">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1033">Si</span></span> | <span data-ttu-id="ba6f6-1034">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1034">Do</span></span> | <span data-ttu-id="ba6f6-1035">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1035">Err</span></span> | <span data-ttu-id="ba6f6-1036">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1036">Err</span></span> | <span data-ttu-id="ba6f6-1037">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1037">Do</span></span> | <span data-ttu-id="ba6f6-1038">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1038">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1039">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1039">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1040">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1040">UL</span></span> | <span data-ttu-id="ba6f6-1041">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1041">De</span></span> | <span data-ttu-id="ba6f6-1042">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1042">Si</span></span> | <span data-ttu-id="ba6f6-1043">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1043">Do</span></span> | <span data-ttu-id="ba6f6-1044">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1044">Err</span></span> | <span data-ttu-id="ba6f6-1045">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1045">Err</span></span> | <span data-ttu-id="ba6f6-1046">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1046">Do</span></span> | <span data-ttu-id="ba6f6-1047">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1047">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1048">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1048">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1049">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1049">De</span></span> | <span data-ttu-id="ba6f6-1050">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1050">Si</span></span> | <span data-ttu-id="ba6f6-1051">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1051">Do</span></span> | <span data-ttu-id="ba6f6-1052">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1052">Err</span></span> | <span data-ttu-id="ba6f6-1053">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1053">Err</span></span> | <span data-ttu-id="ba6f6-1054">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1054">Do</span></span> | <span data-ttu-id="ba6f6-1055">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1055">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1056">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1056">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1057">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1057">Si</span></span> | <span data-ttu-id="ba6f6-1058">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1058">Do</span></span> | <span data-ttu-id="ba6f6-1059">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1059">Err</span></span> | <span data-ttu-id="ba6f6-1060">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1060">Err</span></span> | <span data-ttu-id="ba6f6-1061">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1061">Do</span></span> | <span data-ttu-id="ba6f6-1062">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1062">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1063">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1063">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1064">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1064">Do</span></span> | <span data-ttu-id="ba6f6-1065">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1065">Err</span></span> | <span data-ttu-id="ba6f6-1066">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1066">Err</span></span> | <span data-ttu-id="ba6f6-1067">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1067">Do</span></span> | <span data-ttu-id="ba6f6-1068">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1068">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1069">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1069">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1070">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1070">St</span></span>  | <span data-ttu-id="ba6f6-1071">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1071">Err</span></span> | <span data-ttu-id="ba6f6-1072">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1072">St</span></span> | <span data-ttu-id="ba6f6-1073">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1073">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1074">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1074">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-1075">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1075">St</span></span>  | <span data-ttu-id="ba6f6-1076">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1076">St</span></span> | <span data-ttu-id="ba6f6-1077">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1077">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1078">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1078">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-1079">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1079">St</span></span> | <span data-ttu-id="ba6f6-1080">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1080">Ob</span></span> | 
| <span data-ttu-id="ba6f6-1081">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1081">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="ba6f6-1082">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1082">Ob</span></span> | 


### <a name="subtraction-operator"></a><span data-ttu-id="ba6f6-1083">减法运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1083">Subtraction Operator</span></span>

<span data-ttu-id="ba6f6-1084">减法运算符从第一个操作数中减去第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1084">The subtraction operator subtracts the second operand from the first operand.</span></span>

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-1085">减法运算符是为以下类型定义的：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1085">The subtraction operator is defined for the following types:</span></span>

* <span data-ttu-id="ba6f6-1086">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ba6f6-1087">如果启用了整数溢出检查，并且差异超出了结果类型的范围，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1087">If integer overflow checking is on and the difference is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1088">否则，不会报告溢出，并且放弃结果的任何高序位。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1088">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="ba6f6-1089">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1089">`Single` and `Double`.</span></span> <span data-ttu-id="ba6f6-1090">根据 IEEE 754 算法的规则计算差异。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1090">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ba6f6-1091">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1091">`Decimal`.</span></span> <span data-ttu-id="ba6f6-1092">如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1092">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1093">如果结果值太小而无法表示为十进制格式，则结果为0。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1093">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="ba6f6-1094">`Date`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1094">`Date`.</span></span> <span data-ttu-id="ba6f6-1095">`System.DateTime` 类型定义重载减法运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1095">The `System.DateTime` type defines overloaded subtraction operators.</span></span> <span data-ttu-id="ba6f6-1096">由于 `System.DateTime` 等效于内部 `Date` 类型，因此，`Date` 类型也提供这些运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1096">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="ba6f6-1097">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1097">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-1098">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1098">__Bo__</span></span> | <span data-ttu-id="ba6f6-1099">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1099">__SB__</span></span> | <span data-ttu-id="ba6f6-1100">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1100">__By__</span></span> | <span data-ttu-id="ba6f6-1101">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1101">__Sh__</span></span> | <span data-ttu-id="ba6f6-1102">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1102">__US__</span></span> | <span data-ttu-id="ba6f6-1103">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1103">__In__</span></span> | <span data-ttu-id="ba6f6-1104">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1104">__UI__</span></span> | <span data-ttu-id="ba6f6-1105">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1105">__Lo__</span></span> | <span data-ttu-id="ba6f6-1106">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1106">__UL__</span></span> | <span data-ttu-id="ba6f6-1107">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1107">__De__</span></span> | <span data-ttu-id="ba6f6-1108">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1108">__Si__</span></span> | <span data-ttu-id="ba6f6-1109">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1109">__Do__</span></span> | <span data-ttu-id="ba6f6-1110">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1110">__Da__</span></span>  | <span data-ttu-id="ba6f6-1111">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1111">__Ch__</span></span>  | <span data-ttu-id="ba6f6-1112">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1112">__St__</span></span> | <span data-ttu-id="ba6f6-1113">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1113">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ba6f6-1114">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1114">__Bo__</span></span> | <span data-ttu-id="ba6f6-1115">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1115">Sh</span></span> | <span data-ttu-id="ba6f6-1116">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1116">SB</span></span> | <span data-ttu-id="ba6f6-1117">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1117">Sh</span></span> | <span data-ttu-id="ba6f6-1118">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1118">Sh</span></span> | <span data-ttu-id="ba6f6-1119">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1119">In</span></span> | <span data-ttu-id="ba6f6-1120">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1120">In</span></span> | <span data-ttu-id="ba6f6-1121">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1121">Lo</span></span> | <span data-ttu-id="ba6f6-1122">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1122">Lo</span></span> | <span data-ttu-id="ba6f6-1123">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1123">De</span></span> | <span data-ttu-id="ba6f6-1124">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1124">De</span></span> | <span data-ttu-id="ba6f6-1125">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1125">Si</span></span> | <span data-ttu-id="ba6f6-1126">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1126">Do</span></span> | <span data-ttu-id="ba6f6-1127">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1127">Err</span></span> | <span data-ttu-id="ba6f6-1128">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1128">Err</span></span> | <span data-ttu-id="ba6f6-1129">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1129">Do</span></span>  | <span data-ttu-id="ba6f6-1130">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1130">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1131">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1131">__SB__</span></span> |    | <span data-ttu-id="ba6f6-1132">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1132">SB</span></span> | <span data-ttu-id="ba6f6-1133">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1133">Sh</span></span> | <span data-ttu-id="ba6f6-1134">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1134">Sh</span></span> | <span data-ttu-id="ba6f6-1135">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1135">In</span></span> | <span data-ttu-id="ba6f6-1136">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1136">In</span></span> | <span data-ttu-id="ba6f6-1137">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1137">Lo</span></span> | <span data-ttu-id="ba6f6-1138">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1138">Lo</span></span> | <span data-ttu-id="ba6f6-1139">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1139">De</span></span> | <span data-ttu-id="ba6f6-1140">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1140">De</span></span> | <span data-ttu-id="ba6f6-1141">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1141">Si</span></span> | <span data-ttu-id="ba6f6-1142">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1142">Do</span></span> | <span data-ttu-id="ba6f6-1143">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1143">Err</span></span> | <span data-ttu-id="ba6f6-1144">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1144">Err</span></span> | <span data-ttu-id="ba6f6-1145">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1145">Do</span></span>  | <span data-ttu-id="ba6f6-1146">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1146">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1147">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1147">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-1148">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1148">By</span></span> | <span data-ttu-id="ba6f6-1149">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1149">Sh</span></span> | <span data-ttu-id="ba6f6-1150">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1150">US</span></span> | <span data-ttu-id="ba6f6-1151">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1151">In</span></span> | <span data-ttu-id="ba6f6-1152">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1152">UI</span></span> | <span data-ttu-id="ba6f6-1153">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1153">Lo</span></span> | <span data-ttu-id="ba6f6-1154">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1154">UL</span></span> | <span data-ttu-id="ba6f6-1155">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1155">De</span></span> | <span data-ttu-id="ba6f6-1156">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1156">Si</span></span> | <span data-ttu-id="ba6f6-1157">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1157">Do</span></span> | <span data-ttu-id="ba6f6-1158">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1158">Err</span></span> | <span data-ttu-id="ba6f6-1159">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1159">Err</span></span> | <span data-ttu-id="ba6f6-1160">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1160">Do</span></span>  | <span data-ttu-id="ba6f6-1161">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1161">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1162">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1162">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-1163">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1163">Sh</span></span> | <span data-ttu-id="ba6f6-1164">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1164">In</span></span> | <span data-ttu-id="ba6f6-1165">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1165">In</span></span> | <span data-ttu-id="ba6f6-1166">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1166">Lo</span></span> | <span data-ttu-id="ba6f6-1167">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1167">Lo</span></span> | <span data-ttu-id="ba6f6-1168">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1168">De</span></span> | <span data-ttu-id="ba6f6-1169">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1169">De</span></span> | <span data-ttu-id="ba6f6-1170">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1170">Si</span></span> | <span data-ttu-id="ba6f6-1171">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1171">Do</span></span> | <span data-ttu-id="ba6f6-1172">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1172">Err</span></span> | <span data-ttu-id="ba6f6-1173">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1173">Err</span></span> | <span data-ttu-id="ba6f6-1174">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1174">Do</span></span>  | <span data-ttu-id="ba6f6-1175">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1175">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1176">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1176">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-1177">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1177">US</span></span> | <span data-ttu-id="ba6f6-1178">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1178">In</span></span> | <span data-ttu-id="ba6f6-1179">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1179">UI</span></span> | <span data-ttu-id="ba6f6-1180">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1180">Lo</span></span> | <span data-ttu-id="ba6f6-1181">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1181">UL</span></span> | <span data-ttu-id="ba6f6-1182">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1182">De</span></span> | <span data-ttu-id="ba6f6-1183">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1183">Si</span></span> | <span data-ttu-id="ba6f6-1184">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1184">Do</span></span> | <span data-ttu-id="ba6f6-1185">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1185">Err</span></span> | <span data-ttu-id="ba6f6-1186">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1186">Err</span></span> | <span data-ttu-id="ba6f6-1187">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1187">Do</span></span>  | <span data-ttu-id="ba6f6-1188">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1188">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1189">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1189">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-1190">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1190">In</span></span> | <span data-ttu-id="ba6f6-1191">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1191">Lo</span></span> | <span data-ttu-id="ba6f6-1192">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1192">Lo</span></span> | <span data-ttu-id="ba6f6-1193">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1193">De</span></span> | <span data-ttu-id="ba6f6-1194">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1194">De</span></span> | <span data-ttu-id="ba6f6-1195">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1195">Si</span></span> | <span data-ttu-id="ba6f6-1196">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1196">Do</span></span> | <span data-ttu-id="ba6f6-1197">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1197">Err</span></span> | <span data-ttu-id="ba6f6-1198">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1198">Err</span></span> | <span data-ttu-id="ba6f6-1199">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1199">Do</span></span>  | <span data-ttu-id="ba6f6-1200">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1200">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1201">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1201">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1202">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1202">UI</span></span> | <span data-ttu-id="ba6f6-1203">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1203">Lo</span></span> | <span data-ttu-id="ba6f6-1204">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1204">UL</span></span> | <span data-ttu-id="ba6f6-1205">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1205">De</span></span> | <span data-ttu-id="ba6f6-1206">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1206">Si</span></span> | <span data-ttu-id="ba6f6-1207">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1207">Do</span></span> | <span data-ttu-id="ba6f6-1208">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1208">Err</span></span> | <span data-ttu-id="ba6f6-1209">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1209">Err</span></span> | <span data-ttu-id="ba6f6-1210">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1210">Do</span></span>  | <span data-ttu-id="ba6f6-1211">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1211">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1212">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1212">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1213">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1213">Lo</span></span> | <span data-ttu-id="ba6f6-1214">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1214">De</span></span> | <span data-ttu-id="ba6f6-1215">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1215">De</span></span> | <span data-ttu-id="ba6f6-1216">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1216">Si</span></span> | <span data-ttu-id="ba6f6-1217">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1217">Do</span></span> | <span data-ttu-id="ba6f6-1218">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1218">Err</span></span> | <span data-ttu-id="ba6f6-1219">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1219">Err</span></span> | <span data-ttu-id="ba6f6-1220">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1220">Do</span></span>  | <span data-ttu-id="ba6f6-1221">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1221">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1222">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1222">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1223">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1223">UL</span></span> | <span data-ttu-id="ba6f6-1224">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1224">De</span></span> | <span data-ttu-id="ba6f6-1225">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1225">Si</span></span> | <span data-ttu-id="ba6f6-1226">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1226">Do</span></span> | <span data-ttu-id="ba6f6-1227">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1227">Err</span></span> | <span data-ttu-id="ba6f6-1228">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1228">Err</span></span> | <span data-ttu-id="ba6f6-1229">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1229">Do</span></span>  | <span data-ttu-id="ba6f6-1230">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1230">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1231">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1231">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1232">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1232">De</span></span> | <span data-ttu-id="ba6f6-1233">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1233">Si</span></span> | <span data-ttu-id="ba6f6-1234">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1234">Do</span></span> | <span data-ttu-id="ba6f6-1235">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1235">Err</span></span> | <span data-ttu-id="ba6f6-1236">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1236">Err</span></span> | <span data-ttu-id="ba6f6-1237">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1237">Do</span></span>  | <span data-ttu-id="ba6f6-1238">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1238">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1239">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1239">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1240">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1240">Si</span></span> | <span data-ttu-id="ba6f6-1241">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1241">Do</span></span> | <span data-ttu-id="ba6f6-1242">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1242">Err</span></span> | <span data-ttu-id="ba6f6-1243">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1243">Err</span></span> | <span data-ttu-id="ba6f6-1244">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1244">Do</span></span>  | <span data-ttu-id="ba6f6-1245">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1245">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1246">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1246">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1247">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1247">Do</span></span> | <span data-ttu-id="ba6f6-1248">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1248">Err</span></span> | <span data-ttu-id="ba6f6-1249">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1249">Err</span></span> | <span data-ttu-id="ba6f6-1250">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1250">Do</span></span>  | <span data-ttu-id="ba6f6-1251">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1251">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1252">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1252">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1253">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1253">Err</span></span> | <span data-ttu-id="ba6f6-1254">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1254">Err</span></span> | <span data-ttu-id="ba6f6-1255">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1255">Err</span></span> | <span data-ttu-id="ba6f6-1256">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1256">Err</span></span> | 
| <span data-ttu-id="ba6f6-1257">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1257">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-1258">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1258">Err</span></span> | <span data-ttu-id="ba6f6-1259">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1259">Err</span></span> | <span data-ttu-id="ba6f6-1260">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1260">Err</span></span> | 
| <span data-ttu-id="ba6f6-1261">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1261">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-1262">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1262">Do</span></span>  | <span data-ttu-id="ba6f6-1263">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1263">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1264">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1264">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ba6f6-1265">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1265">Ob</span></span>  | 


### <a name="multiplication-operator"></a><span data-ttu-id="ba6f6-1266">乘法运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1266">Multiplication Operator</span></span>

<span data-ttu-id="ba6f6-1267">乘法运算符计算两个操作数的乘积。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1267">The multiplication operator computes the product of two operands.</span></span>

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-1268">乘法运算符是为以下类型定义的：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1268">The multiplication operator is defined for the following types:</span></span>

* <span data-ttu-id="ba6f6-1269">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ba6f6-1270">如果启用了整数溢出检查，并且产品超出了结果类型的范围，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1270">If integer overflow checking is on and the product is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1271">否则，不会报告溢出，并且放弃结果的任何高序位。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1271">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="ba6f6-1272">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1272">`Single` and `Double`.</span></span> <span data-ttu-id="ba6f6-1273">根据 IEEE 754 算法的规则计算产品。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1273">The product is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ba6f6-1274">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1274">`Decimal`.</span></span> <span data-ttu-id="ba6f6-1275">如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1275">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1276">如果结果值太小而无法表示为十进制格式，则结果为0。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1276">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

<span data-ttu-id="ba6f6-1277">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1277">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-1278">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1278">__Bo__</span></span> | <span data-ttu-id="ba6f6-1279">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1279">__SB__</span></span> | <span data-ttu-id="ba6f6-1280">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1280">__By__</span></span> | <span data-ttu-id="ba6f6-1281">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1281">__Sh__</span></span> | <span data-ttu-id="ba6f6-1282">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1282">__US__</span></span> | <span data-ttu-id="ba6f6-1283">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1283">__In__</span></span> | <span data-ttu-id="ba6f6-1284">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1284">__UI__</span></span> | <span data-ttu-id="ba6f6-1285">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1285">__Lo__</span></span> | <span data-ttu-id="ba6f6-1286">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1286">__UL__</span></span> | <span data-ttu-id="ba6f6-1287">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1287">__De__</span></span> | <span data-ttu-id="ba6f6-1288">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1288">__Si__</span></span> | <span data-ttu-id="ba6f6-1289">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1289">__Do__</span></span> | <span data-ttu-id="ba6f6-1290">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1290">__Da__</span></span>  | <span data-ttu-id="ba6f6-1291">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1291">__Ch__</span></span>  | <span data-ttu-id="ba6f6-1292">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1292">__St__</span></span> | <span data-ttu-id="ba6f6-1293">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1293">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ba6f6-1294">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1294">__Bo__</span></span> | <span data-ttu-id="ba6f6-1295">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1295">Sh</span></span> | <span data-ttu-id="ba6f6-1296">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1296">SB</span></span> | <span data-ttu-id="ba6f6-1297">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1297">Sh</span></span> | <span data-ttu-id="ba6f6-1298">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1298">Sh</span></span> | <span data-ttu-id="ba6f6-1299">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1299">In</span></span> | <span data-ttu-id="ba6f6-1300">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1300">In</span></span> | <span data-ttu-id="ba6f6-1301">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1301">Lo</span></span> | <span data-ttu-id="ba6f6-1302">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1302">Lo</span></span> | <span data-ttu-id="ba6f6-1303">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1303">De</span></span> | <span data-ttu-id="ba6f6-1304">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1304">De</span></span> | <span data-ttu-id="ba6f6-1305">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1305">Si</span></span> | <span data-ttu-id="ba6f6-1306">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1306">Do</span></span> | <span data-ttu-id="ba6f6-1307">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1307">Err</span></span> | <span data-ttu-id="ba6f6-1308">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1308">Err</span></span> | <span data-ttu-id="ba6f6-1309">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1309">Do</span></span>  | <span data-ttu-id="ba6f6-1310">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1310">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1311">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1311">__SB__</span></span> |    | <span data-ttu-id="ba6f6-1312">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1312">SB</span></span> | <span data-ttu-id="ba6f6-1313">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1313">Sh</span></span> | <span data-ttu-id="ba6f6-1314">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1314">Sh</span></span> | <span data-ttu-id="ba6f6-1315">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1315">In</span></span> | <span data-ttu-id="ba6f6-1316">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1316">In</span></span> | <span data-ttu-id="ba6f6-1317">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1317">Lo</span></span> | <span data-ttu-id="ba6f6-1318">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1318">Lo</span></span> | <span data-ttu-id="ba6f6-1319">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1319">De</span></span> | <span data-ttu-id="ba6f6-1320">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1320">De</span></span> | <span data-ttu-id="ba6f6-1321">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1321">Si</span></span> | <span data-ttu-id="ba6f6-1322">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1322">Do</span></span> | <span data-ttu-id="ba6f6-1323">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1323">Err</span></span> | <span data-ttu-id="ba6f6-1324">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1324">Err</span></span> | <span data-ttu-id="ba6f6-1325">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1325">Do</span></span>  | <span data-ttu-id="ba6f6-1326">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1326">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1327">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1327">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-1328">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1328">By</span></span> | <span data-ttu-id="ba6f6-1329">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1329">Sh</span></span> | <span data-ttu-id="ba6f6-1330">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1330">US</span></span> | <span data-ttu-id="ba6f6-1331">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1331">In</span></span> | <span data-ttu-id="ba6f6-1332">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1332">UI</span></span> | <span data-ttu-id="ba6f6-1333">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1333">Lo</span></span> | <span data-ttu-id="ba6f6-1334">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1334">UL</span></span> | <span data-ttu-id="ba6f6-1335">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1335">De</span></span> | <span data-ttu-id="ba6f6-1336">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1336">Si</span></span> | <span data-ttu-id="ba6f6-1337">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1337">Do</span></span> | <span data-ttu-id="ba6f6-1338">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1338">Err</span></span> | <span data-ttu-id="ba6f6-1339">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1339">Err</span></span> | <span data-ttu-id="ba6f6-1340">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1340">Do</span></span>  | <span data-ttu-id="ba6f6-1341">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1341">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1342">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1342">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-1343">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1343">Sh</span></span> | <span data-ttu-id="ba6f6-1344">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1344">In</span></span> | <span data-ttu-id="ba6f6-1345">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1345">In</span></span> | <span data-ttu-id="ba6f6-1346">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1346">Lo</span></span> | <span data-ttu-id="ba6f6-1347">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1347">Lo</span></span> | <span data-ttu-id="ba6f6-1348">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1348">De</span></span> | <span data-ttu-id="ba6f6-1349">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1349">De</span></span> | <span data-ttu-id="ba6f6-1350">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1350">Si</span></span> | <span data-ttu-id="ba6f6-1351">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1351">Do</span></span> | <span data-ttu-id="ba6f6-1352">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1352">Err</span></span> | <span data-ttu-id="ba6f6-1353">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1353">Err</span></span> | <span data-ttu-id="ba6f6-1354">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1354">Do</span></span>  | <span data-ttu-id="ba6f6-1355">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1355">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1356">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1356">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-1357">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1357">US</span></span> | <span data-ttu-id="ba6f6-1358">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1358">In</span></span> | <span data-ttu-id="ba6f6-1359">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1359">UI</span></span> | <span data-ttu-id="ba6f6-1360">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1360">Lo</span></span> | <span data-ttu-id="ba6f6-1361">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1361">UL</span></span> | <span data-ttu-id="ba6f6-1362">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1362">De</span></span> | <span data-ttu-id="ba6f6-1363">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1363">Si</span></span> | <span data-ttu-id="ba6f6-1364">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1364">Do</span></span> | <span data-ttu-id="ba6f6-1365">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1365">Err</span></span> | <span data-ttu-id="ba6f6-1366">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1366">Err</span></span> | <span data-ttu-id="ba6f6-1367">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1367">Do</span></span>  | <span data-ttu-id="ba6f6-1368">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1368">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1369">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1369">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-1370">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1370">In</span></span> | <span data-ttu-id="ba6f6-1371">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1371">Lo</span></span> | <span data-ttu-id="ba6f6-1372">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1372">Lo</span></span> | <span data-ttu-id="ba6f6-1373">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1373">De</span></span> | <span data-ttu-id="ba6f6-1374">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1374">De</span></span> | <span data-ttu-id="ba6f6-1375">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1375">Si</span></span> | <span data-ttu-id="ba6f6-1376">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1376">Do</span></span> | <span data-ttu-id="ba6f6-1377">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1377">Err</span></span> | <span data-ttu-id="ba6f6-1378">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1378">Err</span></span> | <span data-ttu-id="ba6f6-1379">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1379">Do</span></span>  | <span data-ttu-id="ba6f6-1380">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1380">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1381">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1381">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1382">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1382">UI</span></span> | <span data-ttu-id="ba6f6-1383">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1383">Lo</span></span> | <span data-ttu-id="ba6f6-1384">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1384">UL</span></span> | <span data-ttu-id="ba6f6-1385">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1385">De</span></span> | <span data-ttu-id="ba6f6-1386">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1386">Si</span></span> | <span data-ttu-id="ba6f6-1387">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1387">Do</span></span> | <span data-ttu-id="ba6f6-1388">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1388">Err</span></span> | <span data-ttu-id="ba6f6-1389">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1389">Err</span></span> | <span data-ttu-id="ba6f6-1390">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1390">Do</span></span>  | <span data-ttu-id="ba6f6-1391">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1391">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1392">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1392">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1393">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1393">Lo</span></span> | <span data-ttu-id="ba6f6-1394">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1394">De</span></span> | <span data-ttu-id="ba6f6-1395">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1395">De</span></span> | <span data-ttu-id="ba6f6-1396">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1396">Si</span></span> | <span data-ttu-id="ba6f6-1397">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1397">Do</span></span> | <span data-ttu-id="ba6f6-1398">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1398">Err</span></span> | <span data-ttu-id="ba6f6-1399">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1399">Err</span></span> | <span data-ttu-id="ba6f6-1400">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1400">Do</span></span>  | <span data-ttu-id="ba6f6-1401">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1401">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1402">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1402">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1403">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1403">UL</span></span> | <span data-ttu-id="ba6f6-1404">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1404">De</span></span> | <span data-ttu-id="ba6f6-1405">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1405">Si</span></span> | <span data-ttu-id="ba6f6-1406">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1406">Do</span></span> | <span data-ttu-id="ba6f6-1407">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1407">Err</span></span> | <span data-ttu-id="ba6f6-1408">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1408">Err</span></span> | <span data-ttu-id="ba6f6-1409">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1409">Do</span></span>  | <span data-ttu-id="ba6f6-1410">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1410">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1411">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1411">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1412">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1412">De</span></span> | <span data-ttu-id="ba6f6-1413">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1413">Si</span></span> | <span data-ttu-id="ba6f6-1414">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1414">Do</span></span> | <span data-ttu-id="ba6f6-1415">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1415">Err</span></span> | <span data-ttu-id="ba6f6-1416">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1416">Err</span></span> | <span data-ttu-id="ba6f6-1417">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1417">Do</span></span>  | <span data-ttu-id="ba6f6-1418">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1418">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1419">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1419">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1420">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1420">Si</span></span> | <span data-ttu-id="ba6f6-1421">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1421">Do</span></span> | <span data-ttu-id="ba6f6-1422">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1422">Err</span></span> | <span data-ttu-id="ba6f6-1423">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1423">Err</span></span> | <span data-ttu-id="ba6f6-1424">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1424">Do</span></span>  | <span data-ttu-id="ba6f6-1425">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1425">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1426">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1426">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1427">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1427">Do</span></span> | <span data-ttu-id="ba6f6-1428">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1428">Err</span></span> | <span data-ttu-id="ba6f6-1429">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1429">Err</span></span> | <span data-ttu-id="ba6f6-1430">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1430">Do</span></span>  | <span data-ttu-id="ba6f6-1431">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1431">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1432">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1432">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1433">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1433">Err</span></span> | <span data-ttu-id="ba6f6-1434">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1434">Err</span></span> | <span data-ttu-id="ba6f6-1435">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1435">Err</span></span> | <span data-ttu-id="ba6f6-1436">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1436">Err</span></span> | 
| <span data-ttu-id="ba6f6-1437">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1437">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-1438">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1438">Err</span></span> | <span data-ttu-id="ba6f6-1439">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1439">Err</span></span> | <span data-ttu-id="ba6f6-1440">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1440">Err</span></span> | 
| <span data-ttu-id="ba6f6-1441">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1441">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-1442">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1442">Do</span></span>  | <span data-ttu-id="ba6f6-1443">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1443">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1444">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1444">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ba6f6-1445">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1445">Ob</span></span>  | 


### <a name="division-operators"></a><span data-ttu-id="ba6f6-1446">除法运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1446">Division Operators</span></span>

<span data-ttu-id="ba6f6-1447">除法运算符计算两个操作数的商。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1447">Division operators compute the quotient of two operands.</span></span> <span data-ttu-id="ba6f6-1448">有两个除法运算符：常规（浮点）除法运算符和整数除法运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1448">There are two division operators: the regular (floating-point) division operator and the integer division operator.</span></span>

```antlr
DivisionOperatorExpression
    : FPDivisionOperatorExpression
    | IntegerDivisionOperatorExpression
    ;

FPDivisionOperatorExpression
    : Expression '/' LineTerminator? Expression
    ;

IntegerDivisionOperatorExpression
    : Expression '\\' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-1449">为以下类型定义了常规除法运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1449">The regular division operator is defined for the following types:</span></span>

* <span data-ttu-id="ba6f6-1450">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1450">`Single` and `Double`.</span></span> <span data-ttu-id="ba6f6-1451">根据 IEEE 754 算法的规则计算商。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1451">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ba6f6-1452">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1452">`Decimal`.</span></span> <span data-ttu-id="ba6f6-1453">如果右操作数的值为零，则会引发 `System.DivideByZeroException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1453">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1454">如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1454">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1455">如果结果值太小而无法表示为十进制格式，则结果为零。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1455">If the result value is too small to represent in the decimal format, the result is zero.</span></span> <span data-ttu-id="ba6f6-1456">在任何舍入之前，结果的小数位数是最接近于首选比例的小数位数，这将使结果等于准确的结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1456">The scale of the result, before any rounding, is the closest scale to the preferred scale which will preserve a result equal to the exact result.</span></span>  <span data-ttu-id="ba6f6-1457">首选刻度是第一个操作数的小数位数减去第二个操作数的小数位数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1457">The preferred scale is the scale of the first operand less the scale of the second operand.</span></span>

<span data-ttu-id="ba6f6-1458">根据正常的运算符解析规则，在 `Byte`、`Short`、`Integer`和 `Long` 等类型的操作数之间完全分割将导致这两个操作数都转换为类型 `Decimal`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1458">According to normal operator resolution rules, regular division purely between operands of types such as `Byte`, `Short`, `Integer`, and `Long` would cause both operands to be converted to type `Decimal`.</span></span> <span data-ttu-id="ba6f6-1459">但是，如果在这两种类型都不 `Decimal`的情况下在除法运算符上执行运算符解析，则 `Double` 被视为比 `Decimal`窄。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1459">However, when doing operator resolution on the division operator when neither type is `Decimal`, `Double` is considered narrower than `Decimal`.</span></span> <span data-ttu-id="ba6f6-1460">遵循此约定是因为 `Double` 除法比 `Decimal` 除法更有效。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1460">This convention is followed because `Double` division is more efficient than `Decimal` division.</span></span>

<span data-ttu-id="ba6f6-1461">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1461">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-1462">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1462">__Bo__</span></span> | <span data-ttu-id="ba6f6-1463">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1463">__SB__</span></span> | <span data-ttu-id="ba6f6-1464">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1464">__By__</span></span> | <span data-ttu-id="ba6f6-1465">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1465">__Sh__</span></span> | <span data-ttu-id="ba6f6-1466">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1466">__US__</span></span> | <span data-ttu-id="ba6f6-1467">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1467">__In__</span></span> | <span data-ttu-id="ba6f6-1468">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1468">__UI__</span></span> | <span data-ttu-id="ba6f6-1469">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1469">__Lo__</span></span> | <span data-ttu-id="ba6f6-1470">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1470">__UL__</span></span> | <span data-ttu-id="ba6f6-1471">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1471">__De__</span></span> | <span data-ttu-id="ba6f6-1472">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1472">__Si__</span></span> | <span data-ttu-id="ba6f6-1473">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1473">__Do__</span></span> | <span data-ttu-id="ba6f6-1474">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1474">__Da__</span></span>  | <span data-ttu-id="ba6f6-1475">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1475">__Ch__</span></span>  | <span data-ttu-id="ba6f6-1476">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1476">__St__</span></span> | <span data-ttu-id="ba6f6-1477">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1477">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ba6f6-1478">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1478">__Bo__</span></span> | <span data-ttu-id="ba6f6-1479">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1479">Do</span></span> | <span data-ttu-id="ba6f6-1480">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1480">Do</span></span> | <span data-ttu-id="ba6f6-1481">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1481">Do</span></span> | <span data-ttu-id="ba6f6-1482">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1482">Do</span></span> | <span data-ttu-id="ba6f6-1483">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1483">Do</span></span> | <span data-ttu-id="ba6f6-1484">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1484">Do</span></span> | <span data-ttu-id="ba6f6-1485">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1485">Do</span></span> | <span data-ttu-id="ba6f6-1486">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1486">Do</span></span> | <span data-ttu-id="ba6f6-1487">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1487">Do</span></span> | <span data-ttu-id="ba6f6-1488">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1488">De</span></span> | <span data-ttu-id="ba6f6-1489">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1489">Si</span></span> | <span data-ttu-id="ba6f6-1490">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1490">Do</span></span> | <span data-ttu-id="ba6f6-1491">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1491">Err</span></span> | <span data-ttu-id="ba6f6-1492">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1492">Err</span></span> | <span data-ttu-id="ba6f6-1493">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1493">Do</span></span>  | <span data-ttu-id="ba6f6-1494">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1494">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1495">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1495">__SB__</span></span> |    | <span data-ttu-id="ba6f6-1496">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1496">Do</span></span> | <span data-ttu-id="ba6f6-1497">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1497">Do</span></span> | <span data-ttu-id="ba6f6-1498">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1498">Do</span></span> | <span data-ttu-id="ba6f6-1499">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1499">Do</span></span> | <span data-ttu-id="ba6f6-1500">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1500">Do</span></span> | <span data-ttu-id="ba6f6-1501">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1501">Do</span></span> | <span data-ttu-id="ba6f6-1502">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1502">Do</span></span> | <span data-ttu-id="ba6f6-1503">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1503">Do</span></span> | <span data-ttu-id="ba6f6-1504">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1504">De</span></span> | <span data-ttu-id="ba6f6-1505">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1505">Si</span></span> | <span data-ttu-id="ba6f6-1506">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1506">Do</span></span> | <span data-ttu-id="ba6f6-1507">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1507">Err</span></span> | <span data-ttu-id="ba6f6-1508">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1508">Err</span></span> | <span data-ttu-id="ba6f6-1509">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1509">Do</span></span>  | <span data-ttu-id="ba6f6-1510">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1510">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1511">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1511">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-1512">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1512">Do</span></span> | <span data-ttu-id="ba6f6-1513">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1513">Do</span></span> | <span data-ttu-id="ba6f6-1514">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1514">Do</span></span> | <span data-ttu-id="ba6f6-1515">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1515">Do</span></span> | <span data-ttu-id="ba6f6-1516">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1516">Do</span></span> | <span data-ttu-id="ba6f6-1517">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1517">Do</span></span> | <span data-ttu-id="ba6f6-1518">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1518">Do</span></span> | <span data-ttu-id="ba6f6-1519">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1519">De</span></span> | <span data-ttu-id="ba6f6-1520">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1520">Si</span></span> | <span data-ttu-id="ba6f6-1521">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1521">Do</span></span> | <span data-ttu-id="ba6f6-1522">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1522">Err</span></span> | <span data-ttu-id="ba6f6-1523">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1523">Err</span></span> | <span data-ttu-id="ba6f6-1524">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1524">Do</span></span>  | <span data-ttu-id="ba6f6-1525">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1525">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1526">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1526">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-1527">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1527">Do</span></span> | <span data-ttu-id="ba6f6-1528">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1528">Do</span></span> | <span data-ttu-id="ba6f6-1529">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1529">Do</span></span> | <span data-ttu-id="ba6f6-1530">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1530">Do</span></span> | <span data-ttu-id="ba6f6-1531">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1531">Do</span></span> | <span data-ttu-id="ba6f6-1532">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1532">Do</span></span> | <span data-ttu-id="ba6f6-1533">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1533">De</span></span> | <span data-ttu-id="ba6f6-1534">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1534">Si</span></span> | <span data-ttu-id="ba6f6-1535">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1535">Do</span></span> | <span data-ttu-id="ba6f6-1536">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1536">Err</span></span> | <span data-ttu-id="ba6f6-1537">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1537">Err</span></span> | <span data-ttu-id="ba6f6-1538">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1538">Do</span></span>  | <span data-ttu-id="ba6f6-1539">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1539">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1540">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1540">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-1541">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1541">Do</span></span> | <span data-ttu-id="ba6f6-1542">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1542">Do</span></span> | <span data-ttu-id="ba6f6-1543">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1543">Do</span></span> | <span data-ttu-id="ba6f6-1544">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1544">Do</span></span> | <span data-ttu-id="ba6f6-1545">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1545">Do</span></span> | <span data-ttu-id="ba6f6-1546">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1546">De</span></span> | <span data-ttu-id="ba6f6-1547">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1547">Si</span></span> | <span data-ttu-id="ba6f6-1548">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1548">Do</span></span> | <span data-ttu-id="ba6f6-1549">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1549">Err</span></span> | <span data-ttu-id="ba6f6-1550">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1550">Err</span></span> | <span data-ttu-id="ba6f6-1551">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1551">Do</span></span>  | <span data-ttu-id="ba6f6-1552">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1552">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1553">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1553">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-1554">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1554">Do</span></span> | <span data-ttu-id="ba6f6-1555">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1555">Do</span></span> | <span data-ttu-id="ba6f6-1556">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1556">Do</span></span> | <span data-ttu-id="ba6f6-1557">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1557">Do</span></span> | <span data-ttu-id="ba6f6-1558">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1558">De</span></span> | <span data-ttu-id="ba6f6-1559">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1559">Si</span></span> | <span data-ttu-id="ba6f6-1560">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1560">Do</span></span> | <span data-ttu-id="ba6f6-1561">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1561">Err</span></span> | <span data-ttu-id="ba6f6-1562">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1562">Err</span></span> | <span data-ttu-id="ba6f6-1563">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1563">Do</span></span>  | <span data-ttu-id="ba6f6-1564">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1564">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1565">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1565">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1566">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1566">Do</span></span> | <span data-ttu-id="ba6f6-1567">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1567">Do</span></span> | <span data-ttu-id="ba6f6-1568">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1568">Do</span></span> | <span data-ttu-id="ba6f6-1569">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1569">De</span></span> | <span data-ttu-id="ba6f6-1570">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1570">Si</span></span> | <span data-ttu-id="ba6f6-1571">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1571">Do</span></span> | <span data-ttu-id="ba6f6-1572">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1572">Err</span></span> | <span data-ttu-id="ba6f6-1573">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1573">Err</span></span> | <span data-ttu-id="ba6f6-1574">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1574">Do</span></span>  | <span data-ttu-id="ba6f6-1575">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1575">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1576">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1577">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1577">Do</span></span> | <span data-ttu-id="ba6f6-1578">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1578">Do</span></span> | <span data-ttu-id="ba6f6-1579">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1579">De</span></span> | <span data-ttu-id="ba6f6-1580">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1580">Si</span></span> | <span data-ttu-id="ba6f6-1581">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1581">Do</span></span> | <span data-ttu-id="ba6f6-1582">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1582">Err</span></span> | <span data-ttu-id="ba6f6-1583">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1583">Err</span></span> | <span data-ttu-id="ba6f6-1584">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1584">Do</span></span>  | <span data-ttu-id="ba6f6-1585">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1585">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1586">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1586">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1587">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1587">Do</span></span> | <span data-ttu-id="ba6f6-1588">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1588">De</span></span> | <span data-ttu-id="ba6f6-1589">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1589">Si</span></span> | <span data-ttu-id="ba6f6-1590">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1590">Do</span></span> | <span data-ttu-id="ba6f6-1591">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1591">Err</span></span> | <span data-ttu-id="ba6f6-1592">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1592">Err</span></span> | <span data-ttu-id="ba6f6-1593">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1593">Do</span></span>  | <span data-ttu-id="ba6f6-1594">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1594">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1595">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1595">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1596">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1596">De</span></span> | <span data-ttu-id="ba6f6-1597">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1597">Si</span></span> | <span data-ttu-id="ba6f6-1598">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1598">Do</span></span> | <span data-ttu-id="ba6f6-1599">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1599">Err</span></span> | <span data-ttu-id="ba6f6-1600">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1600">Err</span></span> | <span data-ttu-id="ba6f6-1601">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1601">Do</span></span>  | <span data-ttu-id="ba6f6-1602">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1602">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1603">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1603">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1604">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1604">Si</span></span> | <span data-ttu-id="ba6f6-1605">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1605">Do</span></span> | <span data-ttu-id="ba6f6-1606">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1606">Err</span></span> | <span data-ttu-id="ba6f6-1607">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1607">Err</span></span> | <span data-ttu-id="ba6f6-1608">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1608">Do</span></span>  | <span data-ttu-id="ba6f6-1609">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1609">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1610">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1610">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1611">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1611">Do</span></span> | <span data-ttu-id="ba6f6-1612">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1612">Err</span></span> | <span data-ttu-id="ba6f6-1613">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1613">Err</span></span> | <span data-ttu-id="ba6f6-1614">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1614">Do</span></span>  | <span data-ttu-id="ba6f6-1615">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1615">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1616">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1616">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1617">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1617">Err</span></span> | <span data-ttu-id="ba6f6-1618">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1618">Err</span></span> | <span data-ttu-id="ba6f6-1619">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1619">Err</span></span> | <span data-ttu-id="ba6f6-1620">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1620">Err</span></span> | 
| <span data-ttu-id="ba6f6-1621">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1621">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-1622">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1622">Err</span></span> | <span data-ttu-id="ba6f6-1623">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1623">Err</span></span> | <span data-ttu-id="ba6f6-1624">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1624">Err</span></span> | 
| <span data-ttu-id="ba6f6-1625">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1625">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-1626">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1626">Do</span></span>  | <span data-ttu-id="ba6f6-1627">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1627">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1628">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1628">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ba6f6-1629">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1629">Ob</span></span>  | 

<span data-ttu-id="ba6f6-1630">为 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`和 `Long`定义了整数除法运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1630">The integer division operator is defined for `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ba6f6-1631">如果右操作数的值为零，则会引发 `System.DivideByZeroException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1631">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1632">相除将结果舍入到零，结果的绝对值是小于两个操作数的商绝对值的最大整数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1632">The division rounds the result towards zero, and the absolute value of the result is the largest possible integer that is less than the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="ba6f6-1633">如果两个操作数具有相同的符号，则结果为零或正; 如果两个操作数具有相反的符号，则结果为零或负。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1633">The result is zero or positive when the two operands have the same sign, and zero or negative when the two operands have opposite signs.</span></span> <span data-ttu-id="ba6f6-1634">如果左操作数是 `SByte`的最大负值、`Short`、`Integer`或 `Long`，右操作数为 `-1`，则发生溢出;如果启用了整数溢出检查，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1634">If the left operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, and the right operand is `-1`, an overflow occurs; if integer overflow checking is on, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1635">否则，不会报告溢出，结果将改为左操作数的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1635">Otherwise, the overflow is not reported and the result is instead the value of the left operand.</span></span>

<span data-ttu-id="ba6f6-1636">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1636">__Note.__</span></span> <span data-ttu-id="ba6f6-1637">由于无符号类型的两个操作数始终为零或正数，因此结果始终为零或正数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1637">As the two operands for unsigned types will always be zero or positive, the result is always zero or positive.</span></span>  <span data-ttu-id="ba6f6-1638">由于表达式的结果将始终小于或等于两个操作数中的最大值，因此不可能发生溢出。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1638">As the result of the expression will always be less than or equal to the largest of the two operands, it is not possible for an overflow to occur.</span></span>  <span data-ttu-id="ba6f6-1639">不会对具有两个无符号整数的整数除法执行此类整数溢出检查。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1639">As such integer overflow checking is not performed for integer divide with two unsigned integers.</span></span> <span data-ttu-id="ba6f6-1640">结果就是左操作数的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1640">The result is the type as that of the left operand.</span></span>


<span data-ttu-id="ba6f6-1641">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1641">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-1642">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1642">__Bo__</span></span> | <span data-ttu-id="ba6f6-1643">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1643">__SB__</span></span> | <span data-ttu-id="ba6f6-1644">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1644">__By__</span></span> | <span data-ttu-id="ba6f6-1645">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1645">__Sh__</span></span> | <span data-ttu-id="ba6f6-1646">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1646">__US__</span></span> | <span data-ttu-id="ba6f6-1647">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1647">__In__</span></span> | <span data-ttu-id="ba6f6-1648">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1648">__UI__</span></span> | <span data-ttu-id="ba6f6-1649">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1649">__Lo__</span></span> | <span data-ttu-id="ba6f6-1650">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1650">__UL__</span></span> | <span data-ttu-id="ba6f6-1651">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1651">__De__</span></span> | <span data-ttu-id="ba6f6-1652">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1652">__Si__</span></span> | <span data-ttu-id="ba6f6-1653">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1653">__Do__</span></span> | <span data-ttu-id="ba6f6-1654">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1654">__Da__</span></span>  | <span data-ttu-id="ba6f6-1655">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1655">__Ch__</span></span>  | <span data-ttu-id="ba6f6-1656">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1656">__St__</span></span> | <span data-ttu-id="ba6f6-1657">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1657">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ba6f6-1658">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1658">__Bo__</span></span> | <span data-ttu-id="ba6f6-1659">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1659">Sh</span></span> | <span data-ttu-id="ba6f6-1660">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1660">SB</span></span> | <span data-ttu-id="ba6f6-1661">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1661">Sh</span></span> | <span data-ttu-id="ba6f6-1662">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1662">Sh</span></span> | <span data-ttu-id="ba6f6-1663">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1663">In</span></span> | <span data-ttu-id="ba6f6-1664">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1664">In</span></span> | <span data-ttu-id="ba6f6-1665">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1665">Lo</span></span> | <span data-ttu-id="ba6f6-1666">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1666">Lo</span></span> | <span data-ttu-id="ba6f6-1667">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1667">Lo</span></span> | <span data-ttu-id="ba6f6-1668">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1668">Lo</span></span> | <span data-ttu-id="ba6f6-1669">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1669">Lo</span></span> | <span data-ttu-id="ba6f6-1670">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1670">Lo</span></span> | <span data-ttu-id="ba6f6-1671">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1671">Err</span></span> | <span data-ttu-id="ba6f6-1672">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1672">Err</span></span> | <span data-ttu-id="ba6f6-1673">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1673">Lo</span></span>  | <span data-ttu-id="ba6f6-1674">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1674">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1675">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1675">__SB__</span></span> |    | <span data-ttu-id="ba6f6-1676">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1676">SB</span></span> | <span data-ttu-id="ba6f6-1677">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1677">Sh</span></span> | <span data-ttu-id="ba6f6-1678">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1678">Sh</span></span> | <span data-ttu-id="ba6f6-1679">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1679">In</span></span> | <span data-ttu-id="ba6f6-1680">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1680">In</span></span> | <span data-ttu-id="ba6f6-1681">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1681">Lo</span></span> | <span data-ttu-id="ba6f6-1682">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1682">Lo</span></span> | <span data-ttu-id="ba6f6-1683">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1683">Lo</span></span> | <span data-ttu-id="ba6f6-1684">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1684">Lo</span></span> | <span data-ttu-id="ba6f6-1685">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1685">Lo</span></span> | <span data-ttu-id="ba6f6-1686">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1686">Lo</span></span> | <span data-ttu-id="ba6f6-1687">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1687">Err</span></span> | <span data-ttu-id="ba6f6-1688">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1688">Err</span></span> | <span data-ttu-id="ba6f6-1689">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1689">Lo</span></span>  | <span data-ttu-id="ba6f6-1690">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1690">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1691">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1691">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-1692">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1692">By</span></span> | <span data-ttu-id="ba6f6-1693">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1693">Sh</span></span> | <span data-ttu-id="ba6f6-1694">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1694">US</span></span> | <span data-ttu-id="ba6f6-1695">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1695">In</span></span> | <span data-ttu-id="ba6f6-1696">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1696">UI</span></span> | <span data-ttu-id="ba6f6-1697">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1697">Lo</span></span> | <span data-ttu-id="ba6f6-1698">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1698">UL</span></span> | <span data-ttu-id="ba6f6-1699">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1699">Lo</span></span> | <span data-ttu-id="ba6f6-1700">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1700">Lo</span></span> | <span data-ttu-id="ba6f6-1701">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1701">Lo</span></span> | <span data-ttu-id="ba6f6-1702">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1702">Err</span></span> | <span data-ttu-id="ba6f6-1703">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1703">Err</span></span> | <span data-ttu-id="ba6f6-1704">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1704">Lo</span></span>  | <span data-ttu-id="ba6f6-1705">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1705">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1706">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1706">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-1707">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1707">Sh</span></span> | <span data-ttu-id="ba6f6-1708">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1708">In</span></span> | <span data-ttu-id="ba6f6-1709">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1709">In</span></span> | <span data-ttu-id="ba6f6-1710">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1710">Lo</span></span> | <span data-ttu-id="ba6f6-1711">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1711">Lo</span></span> | <span data-ttu-id="ba6f6-1712">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1712">Lo</span></span> | <span data-ttu-id="ba6f6-1713">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1713">Lo</span></span> | <span data-ttu-id="ba6f6-1714">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1714">Lo</span></span> | <span data-ttu-id="ba6f6-1715">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1715">Lo</span></span> | <span data-ttu-id="ba6f6-1716">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1716">Err</span></span> | <span data-ttu-id="ba6f6-1717">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1717">Err</span></span> | <span data-ttu-id="ba6f6-1718">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1718">Lo</span></span>  | <span data-ttu-id="ba6f6-1719">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1719">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1720">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1720">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-1721">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1721">US</span></span> | <span data-ttu-id="ba6f6-1722">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1722">In</span></span> | <span data-ttu-id="ba6f6-1723">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1723">UI</span></span> | <span data-ttu-id="ba6f6-1724">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1724">Lo</span></span> | <span data-ttu-id="ba6f6-1725">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1725">UL</span></span> | <span data-ttu-id="ba6f6-1726">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1726">Lo</span></span> | <span data-ttu-id="ba6f6-1727">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1727">Lo</span></span> | <span data-ttu-id="ba6f6-1728">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1728">Lo</span></span> | <span data-ttu-id="ba6f6-1729">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1729">Err</span></span> | <span data-ttu-id="ba6f6-1730">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1730">Err</span></span> | <span data-ttu-id="ba6f6-1731">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1731">Lo</span></span>  | <span data-ttu-id="ba6f6-1732">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1732">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1733">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1733">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-1734">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1734">In</span></span> | <span data-ttu-id="ba6f6-1735">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1735">Lo</span></span> | <span data-ttu-id="ba6f6-1736">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1736">Lo</span></span> | <span data-ttu-id="ba6f6-1737">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1737">Lo</span></span> | <span data-ttu-id="ba6f6-1738">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1738">Lo</span></span> | <span data-ttu-id="ba6f6-1739">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1739">Lo</span></span> | <span data-ttu-id="ba6f6-1740">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1740">Lo</span></span> | <span data-ttu-id="ba6f6-1741">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1741">Err</span></span> | <span data-ttu-id="ba6f6-1742">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1742">Err</span></span> | <span data-ttu-id="ba6f6-1743">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1743">Lo</span></span>  | <span data-ttu-id="ba6f6-1744">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1744">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1745">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1745">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1746">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1746">UI</span></span> | <span data-ttu-id="ba6f6-1747">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1747">Lo</span></span> | <span data-ttu-id="ba6f6-1748">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1748">UL</span></span> | <span data-ttu-id="ba6f6-1749">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1749">Lo</span></span> | <span data-ttu-id="ba6f6-1750">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1750">Lo</span></span> | <span data-ttu-id="ba6f6-1751">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1751">Lo</span></span> | <span data-ttu-id="ba6f6-1752">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1752">Err</span></span> | <span data-ttu-id="ba6f6-1753">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1753">Err</span></span> | <span data-ttu-id="ba6f6-1754">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1754">Lo</span></span>  | <span data-ttu-id="ba6f6-1755">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1755">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1756">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1756">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1757">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1757">Lo</span></span> | <span data-ttu-id="ba6f6-1758">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1758">Lo</span></span> | <span data-ttu-id="ba6f6-1759">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1759">Lo</span></span> | <span data-ttu-id="ba6f6-1760">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1760">Lo</span></span> | <span data-ttu-id="ba6f6-1761">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1761">Lo</span></span> | <span data-ttu-id="ba6f6-1762">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1762">Err</span></span> | <span data-ttu-id="ba6f6-1763">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1763">Err</span></span> | <span data-ttu-id="ba6f6-1764">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1764">Lo</span></span>  | <span data-ttu-id="ba6f6-1765">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1765">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1766">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1766">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1767">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1767">UL</span></span> | <span data-ttu-id="ba6f6-1768">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1768">Lo</span></span> | <span data-ttu-id="ba6f6-1769">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1769">Lo</span></span> | <span data-ttu-id="ba6f6-1770">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1770">Lo</span></span> | <span data-ttu-id="ba6f6-1771">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1771">Err</span></span> | <span data-ttu-id="ba6f6-1772">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1772">Err</span></span> | <span data-ttu-id="ba6f6-1773">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1773">Lo</span></span>  | <span data-ttu-id="ba6f6-1774">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1774">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1775">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1775">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1776">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1776">Lo</span></span> | <span data-ttu-id="ba6f6-1777">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1777">Lo</span></span> | <span data-ttu-id="ba6f6-1778">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1778">Lo</span></span> | <span data-ttu-id="ba6f6-1779">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1779">Err</span></span> | <span data-ttu-id="ba6f6-1780">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1780">Err</span></span> | <span data-ttu-id="ba6f6-1781">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1781">Lo</span></span>  | <span data-ttu-id="ba6f6-1782">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1782">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1783">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1783">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1784">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1784">Lo</span></span> | <span data-ttu-id="ba6f6-1785">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1785">Lo</span></span> | <span data-ttu-id="ba6f6-1786">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1786">Err</span></span> | <span data-ttu-id="ba6f6-1787">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1787">Err</span></span> | <span data-ttu-id="ba6f6-1788">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1788">Lo</span></span>  | <span data-ttu-id="ba6f6-1789">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1789">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1790">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1790">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1791">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1791">Lo</span></span> | <span data-ttu-id="ba6f6-1792">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1792">Err</span></span> | <span data-ttu-id="ba6f6-1793">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1793">Err</span></span> | <span data-ttu-id="ba6f6-1794">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1794">Lo</span></span>  | <span data-ttu-id="ba6f6-1795">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1795">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1796">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1796">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1797">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1797">Err</span></span> | <span data-ttu-id="ba6f6-1798">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1798">Err</span></span> | <span data-ttu-id="ba6f6-1799">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1799">Err</span></span> | <span data-ttu-id="ba6f6-1800">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1800">Err</span></span> | 
| <span data-ttu-id="ba6f6-1801">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1801">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-1802">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1802">Err</span></span> | <span data-ttu-id="ba6f6-1803">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1803">Err</span></span> | <span data-ttu-id="ba6f6-1804">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1804">Err</span></span> | 
| <span data-ttu-id="ba6f6-1805">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1805">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-1806">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1806">Lo</span></span>  | <span data-ttu-id="ba6f6-1807">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1807">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1808">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ba6f6-1809">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1809">Ob</span></span>  | 


### <a name="mod-operator"></a><span data-ttu-id="ba6f6-1810">Mod 运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1810">Mod Operator</span></span>

<span data-ttu-id="ba6f6-1811">`Mod` （取模）运算符计算两个操作数之间相除的余数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1811">The `Mod` (modulo) operator computes the remainder of the division between two operands.</span></span>

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-1812">为以下类型定义 `Mod` 运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1812">The `Mod` operator is defined for the following types:</span></span>

* <span data-ttu-id="ba6f6-1813">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long`.</span></span> <span data-ttu-id="ba6f6-1814">`x Mod y` 的结果是 `x - (x \ y) * y`生成的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1814">The result of `x Mod y` is the value produced by `x - (x \ y) * y`.</span></span> <span data-ttu-id="ba6f6-1815">如果 `y` 为零，则将引发 `System.DivideByZeroException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1815">If `y` is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1816">取模运算符从不导致溢出。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1816">The modulo operator never causes an overflow.</span></span>

* <span data-ttu-id="ba6f6-1817">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1817">`Single` and `Double`.</span></span> <span data-ttu-id="ba6f6-1818">根据 IEEE 754 算法的规则计算余数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1818">The remainder is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="ba6f6-1819">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1819">`Decimal`.</span></span> <span data-ttu-id="ba6f6-1820">如果右操作数的值为零，则会引发 `System.DivideByZeroException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1820">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1821">如果生成的值太大而无法表示为十进制格式，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1821">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="ba6f6-1822">如果结果值太小而无法表示为十进制格式，则结果为零。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1822">If the result value is too small to represent in the decimal format, the result is zero.</span></span>

<span data-ttu-id="ba6f6-1823">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1823">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-1824">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1824">__Bo__</span></span> | <span data-ttu-id="ba6f6-1825">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1825">__SB__</span></span> | <span data-ttu-id="ba6f6-1826">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1826">__By__</span></span> | <span data-ttu-id="ba6f6-1827">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1827">__Sh__</span></span> | <span data-ttu-id="ba6f6-1828">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1828">__US__</span></span> | <span data-ttu-id="ba6f6-1829">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1829">__In__</span></span> | <span data-ttu-id="ba6f6-1830">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1830">__UI__</span></span> | <span data-ttu-id="ba6f6-1831">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1831">__Lo__</span></span> | <span data-ttu-id="ba6f6-1832">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1832">__UL__</span></span> | <span data-ttu-id="ba6f6-1833">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1833">__De__</span></span> | <span data-ttu-id="ba6f6-1834">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1834">__Si__</span></span> | <span data-ttu-id="ba6f6-1835">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1835">__Do__</span></span> | <span data-ttu-id="ba6f6-1836">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1836">__Da__</span></span>  | <span data-ttu-id="ba6f6-1837">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1837">__Ch__</span></span>  | <span data-ttu-id="ba6f6-1838">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1838">__St__</span></span> | <span data-ttu-id="ba6f6-1839">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1839">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ba6f6-1840">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1840">__Bo__</span></span> | <span data-ttu-id="ba6f6-1841">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1841">Sh</span></span> | <span data-ttu-id="ba6f6-1842">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1842">SB</span></span> | <span data-ttu-id="ba6f6-1843">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1843">Sh</span></span> | <span data-ttu-id="ba6f6-1844">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1844">Sh</span></span> | <span data-ttu-id="ba6f6-1845">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1845">In</span></span> | <span data-ttu-id="ba6f6-1846">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1846">In</span></span> | <span data-ttu-id="ba6f6-1847">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1847">Lo</span></span> | <span data-ttu-id="ba6f6-1848">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1848">Lo</span></span> | <span data-ttu-id="ba6f6-1849">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1849">De</span></span> | <span data-ttu-id="ba6f6-1850">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1850">De</span></span> | <span data-ttu-id="ba6f6-1851">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1851">Si</span></span> | <span data-ttu-id="ba6f6-1852">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1852">Do</span></span> | <span data-ttu-id="ba6f6-1853">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1853">Err</span></span> | <span data-ttu-id="ba6f6-1854">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1854">Err</span></span> | <span data-ttu-id="ba6f6-1855">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1855">Do</span></span>  | <span data-ttu-id="ba6f6-1856">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1856">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1857">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1857">__SB__</span></span> |    | <span data-ttu-id="ba6f6-1858">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1858">SB</span></span> | <span data-ttu-id="ba6f6-1859">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1859">Sh</span></span> | <span data-ttu-id="ba6f6-1860">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1860">Sh</span></span> | <span data-ttu-id="ba6f6-1861">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1861">In</span></span> | <span data-ttu-id="ba6f6-1862">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1862">In</span></span> | <span data-ttu-id="ba6f6-1863">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1863">Lo</span></span> | <span data-ttu-id="ba6f6-1864">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1864">Lo</span></span> | <span data-ttu-id="ba6f6-1865">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1865">De</span></span> | <span data-ttu-id="ba6f6-1866">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1866">De</span></span> | <span data-ttu-id="ba6f6-1867">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1867">Si</span></span> | <span data-ttu-id="ba6f6-1868">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1868">Do</span></span> | <span data-ttu-id="ba6f6-1869">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1869">Err</span></span> | <span data-ttu-id="ba6f6-1870">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1870">Err</span></span> | <span data-ttu-id="ba6f6-1871">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1871">Do</span></span>  | <span data-ttu-id="ba6f6-1872">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1872">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1873">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1873">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-1874">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1874">By</span></span> | <span data-ttu-id="ba6f6-1875">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1875">Sh</span></span> | <span data-ttu-id="ba6f6-1876">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1876">US</span></span> | <span data-ttu-id="ba6f6-1877">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1877">In</span></span> | <span data-ttu-id="ba6f6-1878">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1878">UI</span></span> | <span data-ttu-id="ba6f6-1879">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1879">Lo</span></span> | <span data-ttu-id="ba6f6-1880">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1880">UL</span></span> | <span data-ttu-id="ba6f6-1881">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1881">De</span></span> | <span data-ttu-id="ba6f6-1882">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1882">Si</span></span> | <span data-ttu-id="ba6f6-1883">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1883">Do</span></span> | <span data-ttu-id="ba6f6-1884">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1884">Err</span></span> | <span data-ttu-id="ba6f6-1885">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1885">Err</span></span> | <span data-ttu-id="ba6f6-1886">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1886">Do</span></span>  | <span data-ttu-id="ba6f6-1887">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1887">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1888">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1888">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-1889">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1889">Sh</span></span> | <span data-ttu-id="ba6f6-1890">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1890">In</span></span> | <span data-ttu-id="ba6f6-1891">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1891">In</span></span> | <span data-ttu-id="ba6f6-1892">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1892">Lo</span></span> | <span data-ttu-id="ba6f6-1893">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1893">Lo</span></span> | <span data-ttu-id="ba6f6-1894">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1894">De</span></span> | <span data-ttu-id="ba6f6-1895">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1895">De</span></span> | <span data-ttu-id="ba6f6-1896">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1896">Si</span></span> | <span data-ttu-id="ba6f6-1897">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1897">Do</span></span> | <span data-ttu-id="ba6f6-1898">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1898">Err</span></span> | <span data-ttu-id="ba6f6-1899">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1899">Err</span></span> | <span data-ttu-id="ba6f6-1900">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1900">Do</span></span>  | <span data-ttu-id="ba6f6-1901">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1901">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1902">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1902">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-1903">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1903">US</span></span> | <span data-ttu-id="ba6f6-1904">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1904">In</span></span> | <span data-ttu-id="ba6f6-1905">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1905">UI</span></span> | <span data-ttu-id="ba6f6-1906">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1906">Lo</span></span> | <span data-ttu-id="ba6f6-1907">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1907">UL</span></span> | <span data-ttu-id="ba6f6-1908">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1908">De</span></span> | <span data-ttu-id="ba6f6-1909">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1909">Si</span></span> | <span data-ttu-id="ba6f6-1910">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1910">Do</span></span> | <span data-ttu-id="ba6f6-1911">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1911">Err</span></span> | <span data-ttu-id="ba6f6-1912">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1912">Err</span></span> | <span data-ttu-id="ba6f6-1913">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1913">Do</span></span>  | <span data-ttu-id="ba6f6-1914">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1914">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1915">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1915">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-1916">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1916">In</span></span> | <span data-ttu-id="ba6f6-1917">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1917">Lo</span></span> | <span data-ttu-id="ba6f6-1918">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1918">Lo</span></span> | <span data-ttu-id="ba6f6-1919">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1919">De</span></span> | <span data-ttu-id="ba6f6-1920">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1920">De</span></span> | <span data-ttu-id="ba6f6-1921">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1921">Si</span></span> | <span data-ttu-id="ba6f6-1922">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1922">Do</span></span> | <span data-ttu-id="ba6f6-1923">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1923">Err</span></span> | <span data-ttu-id="ba6f6-1924">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1924">Err</span></span> | <span data-ttu-id="ba6f6-1925">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1925">Do</span></span>  | <span data-ttu-id="ba6f6-1926">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1926">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1927">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1927">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1928">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1928">UI</span></span> | <span data-ttu-id="ba6f6-1929">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1929">Lo</span></span> | <span data-ttu-id="ba6f6-1930">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1930">UL</span></span> | <span data-ttu-id="ba6f6-1931">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1931">De</span></span> | <span data-ttu-id="ba6f6-1932">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1932">Si</span></span> | <span data-ttu-id="ba6f6-1933">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1933">Do</span></span> | <span data-ttu-id="ba6f6-1934">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1934">Err</span></span> | <span data-ttu-id="ba6f6-1935">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1935">Err</span></span> | <span data-ttu-id="ba6f6-1936">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1936">Do</span></span>  | <span data-ttu-id="ba6f6-1937">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1937">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1938">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1938">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1939">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1939">Lo</span></span> | <span data-ttu-id="ba6f6-1940">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1940">De</span></span> | <span data-ttu-id="ba6f6-1941">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1941">De</span></span> | <span data-ttu-id="ba6f6-1942">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1942">Si</span></span> | <span data-ttu-id="ba6f6-1943">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1943">Do</span></span> | <span data-ttu-id="ba6f6-1944">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1944">Err</span></span> | <span data-ttu-id="ba6f6-1945">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1945">Err</span></span> | <span data-ttu-id="ba6f6-1946">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1946">Do</span></span>  | <span data-ttu-id="ba6f6-1947">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1947">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1948">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1948">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1949">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1949">UL</span></span> | <span data-ttu-id="ba6f6-1950">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1950">De</span></span> | <span data-ttu-id="ba6f6-1951">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1951">Si</span></span> | <span data-ttu-id="ba6f6-1952">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1952">Do</span></span> | <span data-ttu-id="ba6f6-1953">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1953">Err</span></span> | <span data-ttu-id="ba6f6-1954">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1954">Err</span></span> | <span data-ttu-id="ba6f6-1955">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1955">Do</span></span>  | <span data-ttu-id="ba6f6-1956">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1956">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1957">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1957">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1958">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1958">De</span></span> | <span data-ttu-id="ba6f6-1959">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1959">Si</span></span> | <span data-ttu-id="ba6f6-1960">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1960">Do</span></span> | <span data-ttu-id="ba6f6-1961">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1961">Err</span></span> | <span data-ttu-id="ba6f6-1962">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1962">Err</span></span> | <span data-ttu-id="ba6f6-1963">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1963">Do</span></span>  | <span data-ttu-id="ba6f6-1964">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1964">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1965">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1965">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1966">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1966">Si</span></span> | <span data-ttu-id="ba6f6-1967">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1967">Do</span></span> | <span data-ttu-id="ba6f6-1968">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1968">Err</span></span> | <span data-ttu-id="ba6f6-1969">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1969">Err</span></span> | <span data-ttu-id="ba6f6-1970">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1970">Do</span></span>  | <span data-ttu-id="ba6f6-1971">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1971">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1972">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1972">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1973">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1973">Do</span></span> | <span data-ttu-id="ba6f6-1974">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1974">Err</span></span> | <span data-ttu-id="ba6f6-1975">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1975">Err</span></span> | <span data-ttu-id="ba6f6-1976">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1976">Do</span></span>  | <span data-ttu-id="ba6f6-1977">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1977">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1978">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1978">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-1979">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1979">Err</span></span> | <span data-ttu-id="ba6f6-1980">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1980">Err</span></span> | <span data-ttu-id="ba6f6-1981">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1981">Err</span></span> | <span data-ttu-id="ba6f6-1982">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1982">Err</span></span> | 
| <span data-ttu-id="ba6f6-1983">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1983">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-1984">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1984">Err</span></span> | <span data-ttu-id="ba6f6-1985">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1985">Err</span></span> | <span data-ttu-id="ba6f6-1986">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1986">Err</span></span> | 
| <span data-ttu-id="ba6f6-1987">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1987">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-1988">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1988">Do</span></span>  | <span data-ttu-id="ba6f6-1989">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1989">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-1990">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1990">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ba6f6-1991">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1991">Ob</span></span>  | 


### <a name="exponentiation-operator"></a><span data-ttu-id="ba6f6-1992">求幂运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1992">Exponentiation Operator</span></span>

<span data-ttu-id="ba6f6-1993">幂运算符计算第一个操作数的第二个操作数的幂。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1993">The exponentiation operator computes the first operand raised to the power of the second operand.</span></span>

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-1994">为 `Double`类型定义幂运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1994">The exponentiation operator is defined for type `Double`.</span></span> <span data-ttu-id="ba6f6-1995">根据 IEEE 754 算法的规则计算该值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1995">The value is computed according to the rules of IEEE 754 arithmetic.</span></span>

<span data-ttu-id="ba6f6-1996">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1996">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-1997">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1997">__Bo__</span></span> | <span data-ttu-id="ba6f6-1998">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1998">__SB__</span></span> | <span data-ttu-id="ba6f6-1999">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-1999">__By__</span></span> | <span data-ttu-id="ba6f6-2000">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2000">__Sh__</span></span> | <span data-ttu-id="ba6f6-2001">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2001">__US__</span></span> | <span data-ttu-id="ba6f6-2002">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2002">__In__</span></span> | <span data-ttu-id="ba6f6-2003">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2003">__UI__</span></span> | <span data-ttu-id="ba6f6-2004">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2004">__Lo__</span></span> | <span data-ttu-id="ba6f6-2005">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2005">__UL__</span></span> | <span data-ttu-id="ba6f6-2006">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2006">__De__</span></span> | <span data-ttu-id="ba6f6-2007">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2007">__Si__</span></span> | <span data-ttu-id="ba6f6-2008">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2008">__Do__</span></span> | <span data-ttu-id="ba6f6-2009">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2009">__Da__</span></span>  | <span data-ttu-id="ba6f6-2010">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2010">__Ch__</span></span>  | <span data-ttu-id="ba6f6-2011">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2011">__St__</span></span> | <span data-ttu-id="ba6f6-2012">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2012">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ba6f6-2013">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2013">__Bo__</span></span> | <span data-ttu-id="ba6f6-2014">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2014">Do</span></span> | <span data-ttu-id="ba6f6-2015">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2015">Do</span></span> | <span data-ttu-id="ba6f6-2016">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2016">Do</span></span> | <span data-ttu-id="ba6f6-2017">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2017">Do</span></span> | <span data-ttu-id="ba6f6-2018">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2018">Do</span></span> | <span data-ttu-id="ba6f6-2019">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2019">Do</span></span> | <span data-ttu-id="ba6f6-2020">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2020">Do</span></span> | <span data-ttu-id="ba6f6-2021">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2021">Do</span></span> | <span data-ttu-id="ba6f6-2022">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2022">Do</span></span> | <span data-ttu-id="ba6f6-2023">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2023">Do</span></span> | <span data-ttu-id="ba6f6-2024">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2024">Do</span></span> | <span data-ttu-id="ba6f6-2025">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2025">Do</span></span> | <span data-ttu-id="ba6f6-2026">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2026">Err</span></span> | <span data-ttu-id="ba6f6-2027">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2027">Err</span></span> | <span data-ttu-id="ba6f6-2028">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2028">Do</span></span>  | <span data-ttu-id="ba6f6-2029">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2029">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2030">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2030">__SB__</span></span> |    | <span data-ttu-id="ba6f6-2031">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2031">Do</span></span> | <span data-ttu-id="ba6f6-2032">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2032">Do</span></span> | <span data-ttu-id="ba6f6-2033">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2033">Do</span></span> | <span data-ttu-id="ba6f6-2034">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2034">Do</span></span> | <span data-ttu-id="ba6f6-2035">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2035">Do</span></span> | <span data-ttu-id="ba6f6-2036">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2036">Do</span></span> | <span data-ttu-id="ba6f6-2037">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2037">Do</span></span> | <span data-ttu-id="ba6f6-2038">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2038">Do</span></span> | <span data-ttu-id="ba6f6-2039">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2039">Do</span></span> | <span data-ttu-id="ba6f6-2040">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2040">Do</span></span> | <span data-ttu-id="ba6f6-2041">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2041">Do</span></span> | <span data-ttu-id="ba6f6-2042">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2042">Err</span></span> | <span data-ttu-id="ba6f6-2043">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2043">Err</span></span> | <span data-ttu-id="ba6f6-2044">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2044">Do</span></span>  | <span data-ttu-id="ba6f6-2045">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2045">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2046">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2046">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-2047">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2047">Do</span></span> | <span data-ttu-id="ba6f6-2048">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2048">Do</span></span> | <span data-ttu-id="ba6f6-2049">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2049">Do</span></span> | <span data-ttu-id="ba6f6-2050">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2050">Do</span></span> | <span data-ttu-id="ba6f6-2051">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2051">Do</span></span> | <span data-ttu-id="ba6f6-2052">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2052">Do</span></span> | <span data-ttu-id="ba6f6-2053">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2053">Do</span></span> | <span data-ttu-id="ba6f6-2054">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2054">Do</span></span> | <span data-ttu-id="ba6f6-2055">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2055">Do</span></span> | <span data-ttu-id="ba6f6-2056">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2056">Do</span></span> | <span data-ttu-id="ba6f6-2057">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2057">Err</span></span> | <span data-ttu-id="ba6f6-2058">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2058">Err</span></span> | <span data-ttu-id="ba6f6-2059">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2059">Do</span></span>  | <span data-ttu-id="ba6f6-2060">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2060">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2061">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2061">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-2062">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2062">Do</span></span> | <span data-ttu-id="ba6f6-2063">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2063">Do</span></span> | <span data-ttu-id="ba6f6-2064">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2064">Do</span></span> | <span data-ttu-id="ba6f6-2065">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2065">Do</span></span> | <span data-ttu-id="ba6f6-2066">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2066">Do</span></span> | <span data-ttu-id="ba6f6-2067">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2067">Do</span></span> | <span data-ttu-id="ba6f6-2068">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2068">Do</span></span> | <span data-ttu-id="ba6f6-2069">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2069">Do</span></span> | <span data-ttu-id="ba6f6-2070">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2070">Do</span></span> | <span data-ttu-id="ba6f6-2071">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2071">Err</span></span> | <span data-ttu-id="ba6f6-2072">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2072">Err</span></span> | <span data-ttu-id="ba6f6-2073">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2073">Do</span></span>  | <span data-ttu-id="ba6f6-2074">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2074">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2075">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2075">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-2076">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2076">Do</span></span> | <span data-ttu-id="ba6f6-2077">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2077">Do</span></span> | <span data-ttu-id="ba6f6-2078">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2078">Do</span></span> | <span data-ttu-id="ba6f6-2079">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2079">Do</span></span> | <span data-ttu-id="ba6f6-2080">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2080">Do</span></span> | <span data-ttu-id="ba6f6-2081">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2081">Do</span></span> | <span data-ttu-id="ba6f6-2082">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2082">Do</span></span> | <span data-ttu-id="ba6f6-2083">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2083">Do</span></span> | <span data-ttu-id="ba6f6-2084">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2084">Err</span></span> | <span data-ttu-id="ba6f6-2085">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2085">Err</span></span> | <span data-ttu-id="ba6f6-2086">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2086">Do</span></span>  | <span data-ttu-id="ba6f6-2087">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2087">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2088">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2088">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-2089">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2089">Do</span></span> | <span data-ttu-id="ba6f6-2090">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2090">Do</span></span> | <span data-ttu-id="ba6f6-2091">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2091">Do</span></span> | <span data-ttu-id="ba6f6-2092">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2092">Do</span></span> | <span data-ttu-id="ba6f6-2093">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2093">Do</span></span> | <span data-ttu-id="ba6f6-2094">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2094">Do</span></span> | <span data-ttu-id="ba6f6-2095">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2095">Do</span></span> | <span data-ttu-id="ba6f6-2096">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2096">Err</span></span> | <span data-ttu-id="ba6f6-2097">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2097">Err</span></span> | <span data-ttu-id="ba6f6-2098">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2098">Do</span></span>  | <span data-ttu-id="ba6f6-2099">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2099">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2100">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2100">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2101">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2101">Do</span></span> | <span data-ttu-id="ba6f6-2102">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2102">Do</span></span> | <span data-ttu-id="ba6f6-2103">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2103">Do</span></span> | <span data-ttu-id="ba6f6-2104">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2104">Do</span></span> | <span data-ttu-id="ba6f6-2105">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2105">Do</span></span> | <span data-ttu-id="ba6f6-2106">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2106">Do</span></span> | <span data-ttu-id="ba6f6-2107">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2107">Err</span></span> | <span data-ttu-id="ba6f6-2108">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2108">Err</span></span> | <span data-ttu-id="ba6f6-2109">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2109">Do</span></span>  | <span data-ttu-id="ba6f6-2110">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2110">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2111">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2111">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2112">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2112">Do</span></span> | <span data-ttu-id="ba6f6-2113">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2113">Do</span></span> | <span data-ttu-id="ba6f6-2114">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2114">Do</span></span> | <span data-ttu-id="ba6f6-2115">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2115">Do</span></span> | <span data-ttu-id="ba6f6-2116">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2116">Do</span></span> | <span data-ttu-id="ba6f6-2117">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2117">Err</span></span> | <span data-ttu-id="ba6f6-2118">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2118">Err</span></span> | <span data-ttu-id="ba6f6-2119">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2119">Do</span></span>  | <span data-ttu-id="ba6f6-2120">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2120">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2121">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2121">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2122">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2122">Do</span></span> | <span data-ttu-id="ba6f6-2123">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2123">Do</span></span> | <span data-ttu-id="ba6f6-2124">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2124">Do</span></span> | <span data-ttu-id="ba6f6-2125">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2125">Do</span></span> | <span data-ttu-id="ba6f6-2126">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2126">Err</span></span> | <span data-ttu-id="ba6f6-2127">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2127">Err</span></span> | <span data-ttu-id="ba6f6-2128">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2128">Do</span></span>  | <span data-ttu-id="ba6f6-2129">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2129">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2130">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2130">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2131">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2131">Do</span></span> | <span data-ttu-id="ba6f6-2132">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2132">Do</span></span> | <span data-ttu-id="ba6f6-2133">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2133">Do</span></span> | <span data-ttu-id="ba6f6-2134">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2134">Err</span></span> | <span data-ttu-id="ba6f6-2135">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2135">Err</span></span> | <span data-ttu-id="ba6f6-2136">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2136">Do</span></span>  | <span data-ttu-id="ba6f6-2137">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2137">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2138">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2138">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2139">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2139">Do</span></span> | <span data-ttu-id="ba6f6-2140">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2140">Do</span></span> | <span data-ttu-id="ba6f6-2141">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2141">Err</span></span> | <span data-ttu-id="ba6f6-2142">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2142">Err</span></span> | <span data-ttu-id="ba6f6-2143">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2143">Do</span></span>  | <span data-ttu-id="ba6f6-2144">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2144">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2145">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2145">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2146">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2146">Do</span></span> | <span data-ttu-id="ba6f6-2147">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2147">Err</span></span> | <span data-ttu-id="ba6f6-2148">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2148">Err</span></span> | <span data-ttu-id="ba6f6-2149">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2149">Do</span></span>  | <span data-ttu-id="ba6f6-2150">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2150">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2151">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2151">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2152">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2152">Err</span></span> | <span data-ttu-id="ba6f6-2153">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2153">Err</span></span> | <span data-ttu-id="ba6f6-2154">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2154">Err</span></span> | <span data-ttu-id="ba6f6-2155">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2155">Err</span></span> | 
| <span data-ttu-id="ba6f6-2156">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2156">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-2157">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2157">Err</span></span> | <span data-ttu-id="ba6f6-2158">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2158">Err</span></span> | <span data-ttu-id="ba6f6-2159">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2159">Err</span></span> | 
| <span data-ttu-id="ba6f6-2160">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2160">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-2161">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2161">Do</span></span>  | <span data-ttu-id="ba6f6-2162">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2162">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2163">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2163">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ba6f6-2164">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2164">Ob</span></span>  | 


## <a name="relational-operators"></a><span data-ttu-id="ba6f6-2165">关系运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2165">Relational Operators</span></span>

<span data-ttu-id="ba6f6-2166">*关系运算符*将值相互比较。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2166">The *relational operators* compare values to one other.</span></span> <span data-ttu-id="ba6f6-2167">比较运算符为 `=`、`<>`、`<`、`>`、`<=`和 `>=`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2167">The comparison operators are `=`, `<>`, `<`, `>`, `<=`, and `>=`.</span></span>

```antlr
RelationalOperatorExpression
    : Expression '=' LineTerminator? Expression
    | Expression '<' '>' LineTerminator? Expression
    | Expression '<' LineTerminator? Expression
    | Expression '>' LineTerminator? Expression
    | Expression '<' '=' LineTerminator? Expression
    | Expression '>' '=' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-2168">所有关系运算符都产生 `Boolean` 值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2168">All of the relational operators result in a `Boolean` value.</span></span>

<span data-ttu-id="ba6f6-2169">关系运算符具有以下一般含义：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2169">The relational operators have the following general meaning:</span></span>

* <span data-ttu-id="ba6f6-2170">`=` 运算符测试两个操作数是否相等。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2170">The `=` operator tests whether the two operands are equal.</span></span>

* <span data-ttu-id="ba6f6-2171">`<>` 运算符测试两个操作数是否不相等。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2171">The `<>` operator tests whether the two operands are not equal.</span></span>

* <span data-ttu-id="ba6f6-2172">`<` 运算符测试第一个操作数是否小于第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2172">The `<` operator tests whether the first operand is less than the second operand.</span></span>

* <span data-ttu-id="ba6f6-2173">`>` 运算符测试第一个操作数是否大于第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2173">The `>` operator tests whether the first operand is greater than the second operand.</span></span>

* <span data-ttu-id="ba6f6-2174">`<=` 运算符测试第一个操作数是否小于或等于第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2174">The `<=` operator tests whether the first operand is less than or equal to the second operand.</span></span>

* <span data-ttu-id="ba6f6-2175">`>=` 运算符测试第一个操作数是否大于或等于第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2175">The `>=` operator tests whether the first operand is greater than or equal to the second operand.</span></span>

<span data-ttu-id="ba6f6-2176">关系运算符是为以下类型定义的：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2176">The relational operators are defined for the following types:</span></span>

* <span data-ttu-id="ba6f6-2177">`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2177">`Boolean`.</span></span> <span data-ttu-id="ba6f6-2178">运算符比较两个操作数的真假值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2178">The operators compare the truth values of the two operands.</span></span> <span data-ttu-id="ba6f6-2179">`True` 被视为小于 `False`，它与其数值匹配。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2179">`True` is considered to be less than `False`, which matches with their numeric values.</span></span>

* <span data-ttu-id="ba6f6-2180">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="ba6f6-2181">运算符比较两个整数操作数的数值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2181">The operators compare the numeric values of the two integral operands.</span></span>

* <span data-ttu-id="ba6f6-2182">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2182">`Single` and `Double`.</span></span> <span data-ttu-id="ba6f6-2183">运算符根据 IEEE 754 标准的规则对操作数进行比较。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2183">The operators compare the operands according to the rules of the IEEE 754 standard.</span></span>

* <span data-ttu-id="ba6f6-2184">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2184">`Decimal`.</span></span> <span data-ttu-id="ba6f6-2185">运算符比较两个 decimal 操作数的数值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2185">The operators compare the numeric values of the two decimal operands.</span></span>

* <span data-ttu-id="ba6f6-2186">`Date`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2186">`Date`.</span></span> <span data-ttu-id="ba6f6-2187">运算符返回比较两个日期/时间值的结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2187">The operators return the result of comparing the two date/time values.</span></span>

* <span data-ttu-id="ba6f6-2188">`Char`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2188">`Char`.</span></span> <span data-ttu-id="ba6f6-2189">运算符返回比较两个 Unicode 值的结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2189">The operators return the result of comparing the two Unicode values.</span></span>

* <span data-ttu-id="ba6f6-2190">`String`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2190">`String`.</span></span> <span data-ttu-id="ba6f6-2191">运算符返回使用二进制比较或文本比较来比较两个值的结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2191">The operators return the result of comparing the two values using either a binary comparison or a text comparison.</span></span> <span data-ttu-id="ba6f6-2192">所使用的比较是由编译环境和 `Option Compare` 语句确定的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2192">The comparison used is determined by the compilation environment and the `Option Compare` statement.</span></span> <span data-ttu-id="ba6f6-2193">二进制比较确定每个字符串中每个字符的数值 Unicode 值是否相同。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2193">A binary comparison determines whether the numeric Unicode value of each character in each string is the same.</span></span> <span data-ttu-id="ba6f6-2194">文本比较基于 .NET Framework 上使用的当前区域性执行 Unicode 文本比较。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2194">A text comparison does a Unicode text comparison based on the current culture in use on the .NET Framework.</span></span> <span data-ttu-id="ba6f6-2195">在执行字符串比较时，null 值等效于字符串文本 `""`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2195">When doing a string comparison, a null value is equivalent to the string literal `""`.</span></span>

<span data-ttu-id="ba6f6-2196">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2196">__Operation Type:__</span></span>
        
|        | <span data-ttu-id="ba6f6-2197">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2197">__Bo__</span></span> | <span data-ttu-id="ba6f6-2198">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2198">__SB__</span></span> | <span data-ttu-id="ba6f6-2199">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2199">__By__</span></span> | <span data-ttu-id="ba6f6-2200">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2200">__Sh__</span></span> | <span data-ttu-id="ba6f6-2201">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2201">__US__</span></span> | <span data-ttu-id="ba6f6-2202">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2202">__In__</span></span> | <span data-ttu-id="ba6f6-2203">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2203">__UI__</span></span> | <span data-ttu-id="ba6f6-2204">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2204">__Lo__</span></span> | <span data-ttu-id="ba6f6-2205">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2205">__UL__</span></span> | <span data-ttu-id="ba6f6-2206">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2206">__De__</span></span> | <span data-ttu-id="ba6f6-2207">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2207">__Si__</span></span> | <span data-ttu-id="ba6f6-2208">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2208">__Do__</span></span> | <span data-ttu-id="ba6f6-2209">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2209">__Da__</span></span>  | <span data-ttu-id="ba6f6-2210">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2210">__Ch__</span></span>  | <span data-ttu-id="ba6f6-2211">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2211">__St__</span></span> | <span data-ttu-id="ba6f6-2212">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2212">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ba6f6-2213">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2213">__Bo__</span></span> | <span data-ttu-id="ba6f6-2214">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2214">Bo</span></span> | <span data-ttu-id="ba6f6-2215">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2215">SB</span></span> | <span data-ttu-id="ba6f6-2216">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2216">Sh</span></span> | <span data-ttu-id="ba6f6-2217">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2217">Sh</span></span> | <span data-ttu-id="ba6f6-2218">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2218">In</span></span> | <span data-ttu-id="ba6f6-2219">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2219">In</span></span> | <span data-ttu-id="ba6f6-2220">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2220">Lo</span></span> | <span data-ttu-id="ba6f6-2221">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2221">Lo</span></span> | <span data-ttu-id="ba6f6-2222">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2222">De</span></span> | <span data-ttu-id="ba6f6-2223">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2223">De</span></span> | <span data-ttu-id="ba6f6-2224">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2224">Si</span></span> | <span data-ttu-id="ba6f6-2225">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2225">Do</span></span> | <span data-ttu-id="ba6f6-2226">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2226">Err</span></span> | <span data-ttu-id="ba6f6-2227">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2227">Err</span></span> | <span data-ttu-id="ba6f6-2228">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2228">Bo</span></span> | <span data-ttu-id="ba6f6-2229">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2229">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2230">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2230">__SB__</span></span> |    | <span data-ttu-id="ba6f6-2231">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2231">SB</span></span> | <span data-ttu-id="ba6f6-2232">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2232">Sh</span></span> | <span data-ttu-id="ba6f6-2233">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2233">Sh</span></span> | <span data-ttu-id="ba6f6-2234">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2234">In</span></span> | <span data-ttu-id="ba6f6-2235">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2235">In</span></span> | <span data-ttu-id="ba6f6-2236">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2236">Lo</span></span> | <span data-ttu-id="ba6f6-2237">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2237">Lo</span></span> | <span data-ttu-id="ba6f6-2238">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2238">De</span></span> | <span data-ttu-id="ba6f6-2239">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2239">De</span></span> | <span data-ttu-id="ba6f6-2240">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2240">Si</span></span> | <span data-ttu-id="ba6f6-2241">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2241">Do</span></span> | <span data-ttu-id="ba6f6-2242">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2242">Err</span></span> | <span data-ttu-id="ba6f6-2243">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2243">Err</span></span> | <span data-ttu-id="ba6f6-2244">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2244">Do</span></span> | <span data-ttu-id="ba6f6-2245">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2245">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2246">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2246">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-2247">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2247">By</span></span> | <span data-ttu-id="ba6f6-2248">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2248">Sh</span></span> | <span data-ttu-id="ba6f6-2249">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2249">US</span></span> | <span data-ttu-id="ba6f6-2250">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2250">In</span></span> | <span data-ttu-id="ba6f6-2251">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2251">UI</span></span> | <span data-ttu-id="ba6f6-2252">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2252">Lo</span></span> | <span data-ttu-id="ba6f6-2253">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2253">UL</span></span> | <span data-ttu-id="ba6f6-2254">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2254">De</span></span> | <span data-ttu-id="ba6f6-2255">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2255">Si</span></span> | <span data-ttu-id="ba6f6-2256">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2256">Do</span></span> | <span data-ttu-id="ba6f6-2257">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2257">Err</span></span> | <span data-ttu-id="ba6f6-2258">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2258">Err</span></span> | <span data-ttu-id="ba6f6-2259">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2259">Do</span></span> | <span data-ttu-id="ba6f6-2260">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2260">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2261">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2261">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-2262">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2262">Sh</span></span> | <span data-ttu-id="ba6f6-2263">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2263">In</span></span> | <span data-ttu-id="ba6f6-2264">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2264">In</span></span> | <span data-ttu-id="ba6f6-2265">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2265">Lo</span></span> | <span data-ttu-id="ba6f6-2266">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2266">Lo</span></span> | <span data-ttu-id="ba6f6-2267">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2267">De</span></span> | <span data-ttu-id="ba6f6-2268">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2268">De</span></span> | <span data-ttu-id="ba6f6-2269">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2269">Si</span></span> | <span data-ttu-id="ba6f6-2270">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2270">Do</span></span> | <span data-ttu-id="ba6f6-2271">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2271">Err</span></span> | <span data-ttu-id="ba6f6-2272">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2272">Err</span></span> | <span data-ttu-id="ba6f6-2273">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2273">Do</span></span> | <span data-ttu-id="ba6f6-2274">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2274">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2275">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2275">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-2276">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2276">US</span></span> | <span data-ttu-id="ba6f6-2277">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2277">In</span></span> | <span data-ttu-id="ba6f6-2278">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2278">UI</span></span> | <span data-ttu-id="ba6f6-2279">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2279">Lo</span></span> | <span data-ttu-id="ba6f6-2280">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2280">UL</span></span> | <span data-ttu-id="ba6f6-2281">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2281">De</span></span> | <span data-ttu-id="ba6f6-2282">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2282">Si</span></span> | <span data-ttu-id="ba6f6-2283">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2283">Do</span></span> | <span data-ttu-id="ba6f6-2284">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2284">Err</span></span> | <span data-ttu-id="ba6f6-2285">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2285">Err</span></span> | <span data-ttu-id="ba6f6-2286">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2286">Do</span></span> | <span data-ttu-id="ba6f6-2287">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2287">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2288">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2288">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-2289">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2289">In</span></span> | <span data-ttu-id="ba6f6-2290">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2290">Lo</span></span> | <span data-ttu-id="ba6f6-2291">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2291">Lo</span></span> | <span data-ttu-id="ba6f6-2292">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2292">De</span></span> | <span data-ttu-id="ba6f6-2293">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2293">De</span></span> | <span data-ttu-id="ba6f6-2294">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2294">Si</span></span> | <span data-ttu-id="ba6f6-2295">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2295">Do</span></span> | <span data-ttu-id="ba6f6-2296">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2296">Err</span></span> | <span data-ttu-id="ba6f6-2297">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2297">Err</span></span> | <span data-ttu-id="ba6f6-2298">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2298">Do</span></span> | <span data-ttu-id="ba6f6-2299">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2299">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2300">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2300">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2301">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2301">UI</span></span> | <span data-ttu-id="ba6f6-2302">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2302">Lo</span></span> | <span data-ttu-id="ba6f6-2303">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2303">UL</span></span> | <span data-ttu-id="ba6f6-2304">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2304">De</span></span> | <span data-ttu-id="ba6f6-2305">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2305">Si</span></span> | <span data-ttu-id="ba6f6-2306">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2306">Do</span></span> | <span data-ttu-id="ba6f6-2307">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2307">Err</span></span> | <span data-ttu-id="ba6f6-2308">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2308">Err</span></span> | <span data-ttu-id="ba6f6-2309">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2309">Do</span></span> | <span data-ttu-id="ba6f6-2310">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2310">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2311">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2311">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2312">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2312">Lo</span></span> | <span data-ttu-id="ba6f6-2313">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2313">De</span></span> | <span data-ttu-id="ba6f6-2314">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2314">De</span></span> | <span data-ttu-id="ba6f6-2315">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2315">Si</span></span> | <span data-ttu-id="ba6f6-2316">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2316">Do</span></span> | <span data-ttu-id="ba6f6-2317">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2317">Err</span></span> | <span data-ttu-id="ba6f6-2318">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2318">Err</span></span> | <span data-ttu-id="ba6f6-2319">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2319">Do</span></span> | <span data-ttu-id="ba6f6-2320">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2320">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2321">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2321">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2322">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2322">UL</span></span> | <span data-ttu-id="ba6f6-2323">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2323">De</span></span> | <span data-ttu-id="ba6f6-2324">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2324">Si</span></span> | <span data-ttu-id="ba6f6-2325">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2325">Do</span></span> | <span data-ttu-id="ba6f6-2326">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2326">Err</span></span> | <span data-ttu-id="ba6f6-2327">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2327">Err</span></span> | <span data-ttu-id="ba6f6-2328">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2328">Do</span></span> | <span data-ttu-id="ba6f6-2329">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2329">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2330">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2330">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2331">取消</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2331">De</span></span> | <span data-ttu-id="ba6f6-2332">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2332">Si</span></span> | <span data-ttu-id="ba6f6-2333">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2333">Do</span></span> | <span data-ttu-id="ba6f6-2334">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2334">Err</span></span> | <span data-ttu-id="ba6f6-2335">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2335">Err</span></span> | <span data-ttu-id="ba6f6-2336">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2336">Do</span></span> | <span data-ttu-id="ba6f6-2337">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2337">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2338">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2338">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2339">Si</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2339">Si</span></span> | <span data-ttu-id="ba6f6-2340">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2340">Do</span></span> | <span data-ttu-id="ba6f6-2341">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2341">Err</span></span> | <span data-ttu-id="ba6f6-2342">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2342">Err</span></span> | <span data-ttu-id="ba6f6-2343">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2343">Do</span></span> | <span data-ttu-id="ba6f6-2344">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2344">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2345">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2345">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2346">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2346">Do</span></span> | <span data-ttu-id="ba6f6-2347">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2347">Err</span></span> | <span data-ttu-id="ba6f6-2348">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2348">Err</span></span> | <span data-ttu-id="ba6f6-2349">应该</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2349">Do</span></span> | <span data-ttu-id="ba6f6-2350">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2350">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2351">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2351">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2352">特里斯坦</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2352">Da</span></span>  | <span data-ttu-id="ba6f6-2353">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2353">Err</span></span> | <span data-ttu-id="ba6f6-2354">特里斯坦</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2354">Da</span></span> | <span data-ttu-id="ba6f6-2355">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2355">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2356">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2356">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-2357">48</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2357">Ch</span></span>  | <span data-ttu-id="ba6f6-2358">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2358">St</span></span> | <span data-ttu-id="ba6f6-2359">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2359">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2360">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2360">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-2361">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2361">St</span></span> | <span data-ttu-id="ba6f6-2362">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2362">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2363">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2363">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="ba6f6-2364">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2364">Ob</span></span> | 


## <a name="like-operator"></a><span data-ttu-id="ba6f6-2365">Like 运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2365">Like Operator</span></span>

<span data-ttu-id="ba6f6-2366">`Like` 运算符确定字符串是否与给定的模式匹配。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2366">The `Like` operator determines whether a string matches a given pattern.</span></span>

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-2367">为 `String` 类型定义 `Like` 运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2367">The `Like` operator is defined for the `String` type.</span></span> <span data-ttu-id="ba6f6-2368">第一个操作数是要匹配的字符串，第二个操作数是要匹配的模式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2368">The first operand is the string being matched, and the second operand is the pattern to match against.</span></span> <span data-ttu-id="ba6f6-2369">模式由 Unicode 字符组成。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2369">The pattern is made up of Unicode characters.</span></span> <span data-ttu-id="ba6f6-2370">以下字符序列具有特殊含义：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2370">The following character sequences have special meanings:</span></span>

* <span data-ttu-id="ba6f6-2371">字符 `?` 匹配任何单个字符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2371">The character `?` matches any single character.</span></span>

* <span data-ttu-id="ba6f6-2372">字符 `*` 与零个或多个字符匹配。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2372">The character `*` matches zero or more characters.</span></span>

* <span data-ttu-id="ba6f6-2373">字符 `#` 匹配任何单个数字（0-9）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2373">The character `#` matches any single digit (0-9).</span></span>

* <span data-ttu-id="ba6f6-2374">方括号（`[ab...]`）括起来的字符列表与列表中的任何单个字符匹配。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2374">A list of characters surrounded by brackets (`[ab...]`) matches any single character in the list.</span></span>

* <span data-ttu-id="ba6f6-2375">由方括号括起来并以感叹号（`[!ab...]`）作为前缀的字符列表匹配字符列表中不存在的任何单个字符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2375">A list of characters surrounded by brackets and prefixed by an exclamation point (`[!ab...]`) matches any single character not in the character list.</span></span>

* <span data-ttu-id="ba6f6-2376">字符列表中由连字符（`-`）分隔的两个字符指定从第一个字符开始、以第二个字符结尾的 Unicode 字符的范围。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2376">Two characters in a character list separated by a hyphen (`-`) specify a range of Unicode characters starting with the first character and ending with the second character.</span></span> <span data-ttu-id="ba6f6-2377">如果第二个字符的排序顺序不是第一个字符，则会出现运行时异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2377">If the second character is not later in the sort order than the first character, a run-time exception occurs.</span></span> <span data-ttu-id="ba6f6-2378">出现在字符列表开头或结尾的连字符指定自身。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2378">A hyphen that appears at the beginning or end of a character list specifies itself.</span></span>

<span data-ttu-id="ba6f6-2379">若要匹配特殊字符左方括号（`[`）、问号（`?`）、数字符号（`#`）和星号（`*`），括号必须括起来。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2379">To match the special characters left bracket (`[`), question mark (`?`), number sign (`#`), and asterisk (`*`), brackets must enclose them.</span></span> <span data-ttu-id="ba6f6-2380">不能在组内使用右大括号（`]`）来匹配自身，但它可以作为单个字符在组外使用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2380">The right bracket (`]`) cannot be used within a group to match itself, but it can be used outside a group as an individual character.</span></span> <span data-ttu-id="ba6f6-2381">字符序列 `[]` 被视为字符串文本 `""`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2381">The character sequence `[]` is considered to be the string literal `""`.</span></span> 

<span data-ttu-id="ba6f6-2382">请注意，字符列表的字符比较和排序取决于所使用的比较类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2382">Note that character comparisons and ordering for character lists are dependent on the type of comparisons being used.</span></span> <span data-ttu-id="ba6f6-2383">如果使用二进制比较，则字符比较和排序基于数值 Unicode 值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2383">If binary comparisons are being used, character comparisons and ordering are based on the numeric Unicode values.</span></span> <span data-ttu-id="ba6f6-2384">如果使用文本比较，则字符比较和排序基于 .NET Framework 上使用的当前区域设置。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2384">If text comparisons are being used, character comparisons and ordering are based on the current locale being used on the .NET Framework.</span></span>

<span data-ttu-id="ba6f6-2385">在某些语言中，字母表中的特殊字符表示两个单独的字符，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2385">In some languages, special characters in the alphabet represent two separate characters and vice versa.</span></span> <span data-ttu-id="ba6f6-2386">例如，几种语言使用字符 `æ` 来表示字符在一起出现时 `a` 和 `e`，而字符 `^` 和 `O` 可用于表示字符 `Ô`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2386">For example, several languages use the character `æ` to represent the characters `a` and `e` when they appear together, while the characters `^` and `O` can be used to represent the character `Ô`.</span></span> <span data-ttu-id="ba6f6-2387">使用文本比较时，`Like` 运算符可识别此类等效性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2387">When using text comparisons, the `Like` operator recognizes such cultural equivalences.</span></span> <span data-ttu-id="ba6f6-2388">在这种情况下，在模式或字符串中出现的单个特殊字符与另一个字符串中等效的双字符序列匹配。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2388">In that case, an occurrence of the single special character in either pattern or string matches the equivalent two-character sequence in the other string.</span></span> <span data-ttu-id="ba6f6-2389">同样，以方括号括起来的模式中的单个特殊字符（单独、列表或范围中）与字符串中等效的双字符序列匹配，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2389">Similarly, a single special character in pattern enclosed in brackets (by itself, in a list, or in a range) matches the equivalent two-character sequence in the string and vice versa.</span></span>

<span data-ttu-id="ba6f6-2390">在 `Like` 表达式中，如果两个操作数均 `Nothing`，或者一个操作数具有到 `String` 的内部转换，而另一个操作数 `Nothing`，则 `Nothing` 被视为空字符串文本 `""`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2390">In a `Like` expression where both operands are `Nothing` or one operand has an intrinsic conversion to `String` and the other operand is `Nothing`, `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="ba6f6-2391">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2391">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-2392">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2392">__Bo__</span></span> | <span data-ttu-id="ba6f6-2393">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2393">__SB__</span></span> | <span data-ttu-id="ba6f6-2394">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2394">__By__</span></span> | <span data-ttu-id="ba6f6-2395">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2395">__Sh__</span></span> | <span data-ttu-id="ba6f6-2396">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2396">__US__</span></span> | <span data-ttu-id="ba6f6-2397">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2397">__In__</span></span> | <span data-ttu-id="ba6f6-2398">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2398">__UI__</span></span> | <span data-ttu-id="ba6f6-2399">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2399">__Lo__</span></span> | <span data-ttu-id="ba6f6-2400">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2400">__UL__</span></span> | <span data-ttu-id="ba6f6-2401">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2401">__De__</span></span> | <span data-ttu-id="ba6f6-2402">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2402">__Si__</span></span> | <span data-ttu-id="ba6f6-2403">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2403">__Do__</span></span> | <span data-ttu-id="ba6f6-2404">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2404">__Da__</span></span>  | <span data-ttu-id="ba6f6-2405">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2405">__Ch__</span></span>  | <span data-ttu-id="ba6f6-2406">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2406">__St__</span></span> | <span data-ttu-id="ba6f6-2407">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2407">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="ba6f6-2408">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2408">__Bo__</span></span> | <span data-ttu-id="ba6f6-2409">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2409">St</span></span> | <span data-ttu-id="ba6f6-2410">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2410">St</span></span> | <span data-ttu-id="ba6f6-2411">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2411">St</span></span> | <span data-ttu-id="ba6f6-2412">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2412">St</span></span> | <span data-ttu-id="ba6f6-2413">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2413">St</span></span> | <span data-ttu-id="ba6f6-2414">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2414">St</span></span> | <span data-ttu-id="ba6f6-2415">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2415">St</span></span> | <span data-ttu-id="ba6f6-2416">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2416">St</span></span> | <span data-ttu-id="ba6f6-2417">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2417">St</span></span> | <span data-ttu-id="ba6f6-2418">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2418">St</span></span> | <span data-ttu-id="ba6f6-2419">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2419">St</span></span> | <span data-ttu-id="ba6f6-2420">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2420">St</span></span> | <span data-ttu-id="ba6f6-2421">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2421">St</span></span> | <span data-ttu-id="ba6f6-2422">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2422">St</span></span> | <span data-ttu-id="ba6f6-2423">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2423">St</span></span> | <span data-ttu-id="ba6f6-2424">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2424">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2425">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2425">__SB__</span></span> |    | <span data-ttu-id="ba6f6-2426">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2426">St</span></span> | <span data-ttu-id="ba6f6-2427">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2427">St</span></span> | <span data-ttu-id="ba6f6-2428">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2428">St</span></span> | <span data-ttu-id="ba6f6-2429">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2429">St</span></span> | <span data-ttu-id="ba6f6-2430">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2430">St</span></span> | <span data-ttu-id="ba6f6-2431">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2431">St</span></span> | <span data-ttu-id="ba6f6-2432">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2432">St</span></span> | <span data-ttu-id="ba6f6-2433">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2433">St</span></span> | <span data-ttu-id="ba6f6-2434">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2434">St</span></span> | <span data-ttu-id="ba6f6-2435">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2435">St</span></span> | <span data-ttu-id="ba6f6-2436">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2436">St</span></span> | <span data-ttu-id="ba6f6-2437">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2437">St</span></span> | <span data-ttu-id="ba6f6-2438">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2438">St</span></span> | <span data-ttu-id="ba6f6-2439">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2439">St</span></span> | <span data-ttu-id="ba6f6-2440">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2440">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2441">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2441">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-2442">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2442">St</span></span> | <span data-ttu-id="ba6f6-2443">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2443">St</span></span> | <span data-ttu-id="ba6f6-2444">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2444">St</span></span> | <span data-ttu-id="ba6f6-2445">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2445">St</span></span> | <span data-ttu-id="ba6f6-2446">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2446">St</span></span> | <span data-ttu-id="ba6f6-2447">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2447">St</span></span> | <span data-ttu-id="ba6f6-2448">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2448">St</span></span> | <span data-ttu-id="ba6f6-2449">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2449">St</span></span> | <span data-ttu-id="ba6f6-2450">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2450">St</span></span> | <span data-ttu-id="ba6f6-2451">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2451">St</span></span> | <span data-ttu-id="ba6f6-2452">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2452">St</span></span> | <span data-ttu-id="ba6f6-2453">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2453">St</span></span> | <span data-ttu-id="ba6f6-2454">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2454">St</span></span> | <span data-ttu-id="ba6f6-2455">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2455">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2456">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2456">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-2457">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2457">St</span></span> | <span data-ttu-id="ba6f6-2458">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2458">St</span></span> | <span data-ttu-id="ba6f6-2459">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2459">St</span></span> | <span data-ttu-id="ba6f6-2460">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2460">St</span></span> | <span data-ttu-id="ba6f6-2461">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2461">St</span></span> | <span data-ttu-id="ba6f6-2462">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2462">St</span></span> | <span data-ttu-id="ba6f6-2463">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2463">St</span></span> | <span data-ttu-id="ba6f6-2464">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2464">St</span></span> | <span data-ttu-id="ba6f6-2465">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2465">St</span></span> | <span data-ttu-id="ba6f6-2466">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2466">St</span></span> | <span data-ttu-id="ba6f6-2467">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2467">St</span></span> | <span data-ttu-id="ba6f6-2468">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2468">St</span></span> | <span data-ttu-id="ba6f6-2469">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2469">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2470">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2470">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-2471">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2471">St</span></span> | <span data-ttu-id="ba6f6-2472">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2472">St</span></span> | <span data-ttu-id="ba6f6-2473">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2473">St</span></span> | <span data-ttu-id="ba6f6-2474">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2474">St</span></span> | <span data-ttu-id="ba6f6-2475">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2475">St</span></span> | <span data-ttu-id="ba6f6-2476">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2476">St</span></span> | <span data-ttu-id="ba6f6-2477">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2477">St</span></span> | <span data-ttu-id="ba6f6-2478">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2478">St</span></span> | <span data-ttu-id="ba6f6-2479">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2479">St</span></span> | <span data-ttu-id="ba6f6-2480">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2480">St</span></span> | <span data-ttu-id="ba6f6-2481">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2481">St</span></span> | <span data-ttu-id="ba6f6-2482">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2482">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2483">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2483">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-2484">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2484">St</span></span> | <span data-ttu-id="ba6f6-2485">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2485">St</span></span> | <span data-ttu-id="ba6f6-2486">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2486">St</span></span> | <span data-ttu-id="ba6f6-2487">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2487">St</span></span> | <span data-ttu-id="ba6f6-2488">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2488">St</span></span> | <span data-ttu-id="ba6f6-2489">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2489">St</span></span> | <span data-ttu-id="ba6f6-2490">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2490">St</span></span> | <span data-ttu-id="ba6f6-2491">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2491">St</span></span> | <span data-ttu-id="ba6f6-2492">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2492">St</span></span> | <span data-ttu-id="ba6f6-2493">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2493">St</span></span> | <span data-ttu-id="ba6f6-2494">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2494">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2495">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2495">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2496">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2496">St</span></span> | <span data-ttu-id="ba6f6-2497">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2497">St</span></span> | <span data-ttu-id="ba6f6-2498">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2498">St</span></span> | <span data-ttu-id="ba6f6-2499">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2499">St</span></span> | <span data-ttu-id="ba6f6-2500">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2500">St</span></span> | <span data-ttu-id="ba6f6-2501">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2501">St</span></span> | <span data-ttu-id="ba6f6-2502">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2502">St</span></span> | <span data-ttu-id="ba6f6-2503">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2503">St</span></span> | <span data-ttu-id="ba6f6-2504">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2504">St</span></span> | <span data-ttu-id="ba6f6-2505">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2505">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2506">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2506">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2507">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2507">St</span></span> | <span data-ttu-id="ba6f6-2508">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2508">St</span></span> | <span data-ttu-id="ba6f6-2509">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2509">St</span></span> | <span data-ttu-id="ba6f6-2510">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2510">St</span></span> | <span data-ttu-id="ba6f6-2511">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2511">St</span></span> | <span data-ttu-id="ba6f6-2512">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2512">St</span></span> | <span data-ttu-id="ba6f6-2513">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2513">St</span></span> | <span data-ttu-id="ba6f6-2514">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2514">St</span></span> | <span data-ttu-id="ba6f6-2515">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2515">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2516">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2516">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2517">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2517">St</span></span> | <span data-ttu-id="ba6f6-2518">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2518">St</span></span> | <span data-ttu-id="ba6f6-2519">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2519">St</span></span> | <span data-ttu-id="ba6f6-2520">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2520">St</span></span> | <span data-ttu-id="ba6f6-2521">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2521">St</span></span> | <span data-ttu-id="ba6f6-2522">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2522">St</span></span> | <span data-ttu-id="ba6f6-2523">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2523">St</span></span> | <span data-ttu-id="ba6f6-2524">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2524">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2525">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2525">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2526">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2526">St</span></span> | <span data-ttu-id="ba6f6-2527">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2527">St</span></span> | <span data-ttu-id="ba6f6-2528">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2528">St</span></span> | <span data-ttu-id="ba6f6-2529">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2529">St</span></span> | <span data-ttu-id="ba6f6-2530">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2530">St</span></span> | <span data-ttu-id="ba6f6-2531">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2531">St</span></span> | <span data-ttu-id="ba6f6-2532">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2532">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2533">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2533">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2534">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2534">St</span></span> | <span data-ttu-id="ba6f6-2535">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2535">St</span></span> | <span data-ttu-id="ba6f6-2536">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2536">St</span></span> | <span data-ttu-id="ba6f6-2537">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2537">St</span></span> | <span data-ttu-id="ba6f6-2538">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2538">St</span></span> | <span data-ttu-id="ba6f6-2539">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2539">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2540">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2540">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2541">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2541">St</span></span> | <span data-ttu-id="ba6f6-2542">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2542">St</span></span> | <span data-ttu-id="ba6f6-2543">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2543">St</span></span> | <span data-ttu-id="ba6f6-2544">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2544">St</span></span> | <span data-ttu-id="ba6f6-2545">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2545">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2546">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2546">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2547">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2547">St</span></span> | <span data-ttu-id="ba6f6-2548">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2548">St</span></span> | <span data-ttu-id="ba6f6-2549">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2549">St</span></span> | <span data-ttu-id="ba6f6-2550">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2550">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2551">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2551">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2552">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2552">St</span></span> | <span data-ttu-id="ba6f6-2553">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2553">St</span></span> | <span data-ttu-id="ba6f6-2554">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2554">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2555">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2555">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2556">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2556">St</span></span> | <span data-ttu-id="ba6f6-2557">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2557">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2558">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2558">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2559">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2559">Ob</span></span> | 


## <a name="concatenation-operator"></a><span data-ttu-id="ba6f6-2560">串联运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2560">Concatenation Operator</span></span>

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-2561">*串联运算符*是为所有内部类型定义的，包括内部值类型的可以为 null 的版本。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2561">The *concatenation operator* is defined for all of the intrinsic types, including the nullable versions of the intrinsic value types.</span></span> <span data-ttu-id="ba6f6-2562">它还定义为在上面提到的类型与 `System.DBNull`（被视为 `Nothing` 字符串）之间进行串联。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2562">It is also defined for concatenation between the types mentioned above and `System.DBNull`, which is treated as a `Nothing` string.</span></span> <span data-ttu-id="ba6f6-2563">串联运算符将其所有操作数转换为 `String`;在表达式中，无论是否使用严格语义，都可以将对 `String` 的所有转换视为扩大。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2563">The concatenation operator converts all of its operands to `String`; in the expression, all conversions to `String` are considered to be widening, regardless of whether strict semantics are used.</span></span> <span data-ttu-id="ba6f6-2564">`System.DBNull` 值转换为类型为 `String`的文本 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2564">A `System.DBNull` value is converted to the literal `Nothing` typed as `String`.</span></span> <span data-ttu-id="ba6f6-2565">值为 `Nothing` 的可以为 null 的值类型也转换为类型为 `String`的文本 `Nothing`，而不是引发运行时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2565">A nullable value type whose value is `Nothing` is also converted to the literal `Nothing` typed as `String`, rather than throwing a run-time error.</span></span>

<span data-ttu-id="ba6f6-2566">串联运算将生成一个字符串，该字符串是从左至右按顺序连接两个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2566">A concatenation operation results in a string that is the concatenation of the two operands in order from left to right.</span></span> <span data-ttu-id="ba6f6-2567">将 `Nothing` 值视为空字符串文本 `""`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2567">The value `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="ba6f6-2568">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2568">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-2569">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2569">__Bo__</span></span> | <span data-ttu-id="ba6f6-2570">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2570">__SB__</span></span> | <span data-ttu-id="ba6f6-2571">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2571">__By__</span></span> | <span data-ttu-id="ba6f6-2572">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2572">__Sh__</span></span> | <span data-ttu-id="ba6f6-2573">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2573">__US__</span></span> | <span data-ttu-id="ba6f6-2574">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2574">__In__</span></span> | <span data-ttu-id="ba6f6-2575">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2575">__UI__</span></span> | <span data-ttu-id="ba6f6-2576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2576">__Lo__</span></span> | <span data-ttu-id="ba6f6-2577">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2577">__UL__</span></span> | <span data-ttu-id="ba6f6-2578">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2578">__De__</span></span> | <span data-ttu-id="ba6f6-2579">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2579">__Si__</span></span> | <span data-ttu-id="ba6f6-2580">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2580">__Do__</span></span> | <span data-ttu-id="ba6f6-2581">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2581">__Da__</span></span>  | <span data-ttu-id="ba6f6-2582">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2582">__Ch__</span></span>  | <span data-ttu-id="ba6f6-2583">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2583">__St__</span></span> | <span data-ttu-id="ba6f6-2584">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2584">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="ba6f6-2585">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2585">__Bo__</span></span> | <span data-ttu-id="ba6f6-2586">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2586">St</span></span> | <span data-ttu-id="ba6f6-2587">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2587">St</span></span> | <span data-ttu-id="ba6f6-2588">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2588">St</span></span> | <span data-ttu-id="ba6f6-2589">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2589">St</span></span> | <span data-ttu-id="ba6f6-2590">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2590">St</span></span> | <span data-ttu-id="ba6f6-2591">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2591">St</span></span> | <span data-ttu-id="ba6f6-2592">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2592">St</span></span> | <span data-ttu-id="ba6f6-2593">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2593">St</span></span> | <span data-ttu-id="ba6f6-2594">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2594">St</span></span> | <span data-ttu-id="ba6f6-2595">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2595">St</span></span> | <span data-ttu-id="ba6f6-2596">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2596">St</span></span> | <span data-ttu-id="ba6f6-2597">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2597">St</span></span> | <span data-ttu-id="ba6f6-2598">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2598">St</span></span> | <span data-ttu-id="ba6f6-2599">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2599">St</span></span> | <span data-ttu-id="ba6f6-2600">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2600">St</span></span> | <span data-ttu-id="ba6f6-2601">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2601">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2602">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2602">__SB__</span></span> |    | <span data-ttu-id="ba6f6-2603">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2603">St</span></span> | <span data-ttu-id="ba6f6-2604">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2604">St</span></span> | <span data-ttu-id="ba6f6-2605">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2605">St</span></span> | <span data-ttu-id="ba6f6-2606">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2606">St</span></span> | <span data-ttu-id="ba6f6-2607">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2607">St</span></span> | <span data-ttu-id="ba6f6-2608">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2608">St</span></span> | <span data-ttu-id="ba6f6-2609">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2609">St</span></span> | <span data-ttu-id="ba6f6-2610">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2610">St</span></span> | <span data-ttu-id="ba6f6-2611">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2611">St</span></span> | <span data-ttu-id="ba6f6-2612">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2612">St</span></span> | <span data-ttu-id="ba6f6-2613">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2613">St</span></span> | <span data-ttu-id="ba6f6-2614">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2614">St</span></span> | <span data-ttu-id="ba6f6-2615">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2615">St</span></span> | <span data-ttu-id="ba6f6-2616">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2616">St</span></span> | <span data-ttu-id="ba6f6-2617">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2617">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2618">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2618">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-2619">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2619">St</span></span> | <span data-ttu-id="ba6f6-2620">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2620">St</span></span> | <span data-ttu-id="ba6f6-2621">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2621">St</span></span> | <span data-ttu-id="ba6f6-2622">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2622">St</span></span> | <span data-ttu-id="ba6f6-2623">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2623">St</span></span> | <span data-ttu-id="ba6f6-2624">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2624">St</span></span> | <span data-ttu-id="ba6f6-2625">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2625">St</span></span> | <span data-ttu-id="ba6f6-2626">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2626">St</span></span> | <span data-ttu-id="ba6f6-2627">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2627">St</span></span> | <span data-ttu-id="ba6f6-2628">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2628">St</span></span> | <span data-ttu-id="ba6f6-2629">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2629">St</span></span> | <span data-ttu-id="ba6f6-2630">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2630">St</span></span> | <span data-ttu-id="ba6f6-2631">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2631">St</span></span> | <span data-ttu-id="ba6f6-2632">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2632">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2633">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2633">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-2634">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2634">St</span></span> | <span data-ttu-id="ba6f6-2635">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2635">St</span></span> | <span data-ttu-id="ba6f6-2636">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2636">St</span></span> | <span data-ttu-id="ba6f6-2637">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2637">St</span></span> | <span data-ttu-id="ba6f6-2638">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2638">St</span></span> | <span data-ttu-id="ba6f6-2639">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2639">St</span></span> | <span data-ttu-id="ba6f6-2640">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2640">St</span></span> | <span data-ttu-id="ba6f6-2641">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2641">St</span></span> | <span data-ttu-id="ba6f6-2642">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2642">St</span></span> | <span data-ttu-id="ba6f6-2643">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2643">St</span></span> | <span data-ttu-id="ba6f6-2644">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2644">St</span></span> | <span data-ttu-id="ba6f6-2645">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2645">St</span></span> | <span data-ttu-id="ba6f6-2646">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2646">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2647">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2647">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-2648">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2648">St</span></span> | <span data-ttu-id="ba6f6-2649">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2649">St</span></span> | <span data-ttu-id="ba6f6-2650">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2650">St</span></span> | <span data-ttu-id="ba6f6-2651">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2651">St</span></span> | <span data-ttu-id="ba6f6-2652">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2652">St</span></span> | <span data-ttu-id="ba6f6-2653">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2653">St</span></span> | <span data-ttu-id="ba6f6-2654">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2654">St</span></span> | <span data-ttu-id="ba6f6-2655">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2655">St</span></span> | <span data-ttu-id="ba6f6-2656">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2656">St</span></span> | <span data-ttu-id="ba6f6-2657">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2657">St</span></span> | <span data-ttu-id="ba6f6-2658">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2658">St</span></span> | <span data-ttu-id="ba6f6-2659">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2659">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2660">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2660">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-2661">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2661">St</span></span> | <span data-ttu-id="ba6f6-2662">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2662">St</span></span> | <span data-ttu-id="ba6f6-2663">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2663">St</span></span> | <span data-ttu-id="ba6f6-2664">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2664">St</span></span> | <span data-ttu-id="ba6f6-2665">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2665">St</span></span> | <span data-ttu-id="ba6f6-2666">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2666">St</span></span> | <span data-ttu-id="ba6f6-2667">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2667">St</span></span> | <span data-ttu-id="ba6f6-2668">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2668">St</span></span> | <span data-ttu-id="ba6f6-2669">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2669">St</span></span> | <span data-ttu-id="ba6f6-2670">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2670">St</span></span> | <span data-ttu-id="ba6f6-2671">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2671">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2672">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2672">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2673">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2673">St</span></span> | <span data-ttu-id="ba6f6-2674">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2674">St</span></span> | <span data-ttu-id="ba6f6-2675">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2675">St</span></span> | <span data-ttu-id="ba6f6-2676">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2676">St</span></span> | <span data-ttu-id="ba6f6-2677">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2677">St</span></span> | <span data-ttu-id="ba6f6-2678">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2678">St</span></span> | <span data-ttu-id="ba6f6-2679">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2679">St</span></span> | <span data-ttu-id="ba6f6-2680">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2680">St</span></span> | <span data-ttu-id="ba6f6-2681">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2681">St</span></span> | <span data-ttu-id="ba6f6-2682">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2682">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2683">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2683">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2684">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2684">St</span></span> | <span data-ttu-id="ba6f6-2685">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2685">St</span></span> | <span data-ttu-id="ba6f6-2686">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2686">St</span></span> | <span data-ttu-id="ba6f6-2687">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2687">St</span></span> | <span data-ttu-id="ba6f6-2688">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2688">St</span></span> | <span data-ttu-id="ba6f6-2689">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2689">St</span></span> | <span data-ttu-id="ba6f6-2690">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2690">St</span></span> | <span data-ttu-id="ba6f6-2691">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2691">St</span></span> | <span data-ttu-id="ba6f6-2692">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2692">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2693">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2693">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2694">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2694">St</span></span> | <span data-ttu-id="ba6f6-2695">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2695">St</span></span> | <span data-ttu-id="ba6f6-2696">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2696">St</span></span> | <span data-ttu-id="ba6f6-2697">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2697">St</span></span> | <span data-ttu-id="ba6f6-2698">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2698">St</span></span> | <span data-ttu-id="ba6f6-2699">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2699">St</span></span> | <span data-ttu-id="ba6f6-2700">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2700">St</span></span> | <span data-ttu-id="ba6f6-2701">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2701">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2702">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2702">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2703">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2703">St</span></span> | <span data-ttu-id="ba6f6-2704">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2704">St</span></span> | <span data-ttu-id="ba6f6-2705">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2705">St</span></span> | <span data-ttu-id="ba6f6-2706">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2706">St</span></span> | <span data-ttu-id="ba6f6-2707">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2707">St</span></span> | <span data-ttu-id="ba6f6-2708">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2708">St</span></span> | <span data-ttu-id="ba6f6-2709">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2709">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2710">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2710">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2711">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2711">St</span></span> | <span data-ttu-id="ba6f6-2712">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2712">St</span></span> | <span data-ttu-id="ba6f6-2713">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2713">St</span></span> | <span data-ttu-id="ba6f6-2714">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2714">St</span></span> | <span data-ttu-id="ba6f6-2715">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2715">St</span></span> | <span data-ttu-id="ba6f6-2716">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2716">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2717">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2717">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2718">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2718">St</span></span> | <span data-ttu-id="ba6f6-2719">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2719">St</span></span> | <span data-ttu-id="ba6f6-2720">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2720">St</span></span> | <span data-ttu-id="ba6f6-2721">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2721">St</span></span> | <span data-ttu-id="ba6f6-2722">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2722">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2723">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2723">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2724">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2724">St</span></span> | <span data-ttu-id="ba6f6-2725">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2725">St</span></span> | <span data-ttu-id="ba6f6-2726">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2726">St</span></span> | <span data-ttu-id="ba6f6-2727">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2727">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2728">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2728">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2729">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2729">St</span></span> | <span data-ttu-id="ba6f6-2730">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2730">St</span></span> | <span data-ttu-id="ba6f6-2731">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2731">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2732">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2732">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2733">停止</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2733">St</span></span> | <span data-ttu-id="ba6f6-2734">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2734">Ob</span></span> | 
| <span data-ttu-id="ba6f6-2735">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2735">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2736">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2736">Ob</span></span> | 


## <a name="logical-operators"></a><span data-ttu-id="ba6f6-2737">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2737">Logical Operators</span></span>

<span data-ttu-id="ba6f6-2738">`And`、`Not`、`Or`和 `Xor` 运算符称为逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2738">The `And`, `Not`, `Or`, and `Xor` operators are called the logical operators.</span></span>

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-2739">逻辑运算符按如下方式计算：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2739">The logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="ba6f6-2740">对于 "`Boolean` 类型：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2740">For the `Boolean` type:</span></span>

  * <span data-ttu-id="ba6f6-2741">逻辑 `And` 操作是在其两个操作数上执行的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2741">A logical `And` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="ba6f6-2742">逻辑 `Not` 操作在其操作数上执行。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2742">A logical `Not` operation is performed on its operand.</span></span>

  * <span data-ttu-id="ba6f6-2743">逻辑 `Or` 操作是在其两个操作数上执行的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2743">A logical `Or` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="ba6f6-2744">对其两个操作数执行逻辑异`Or` 运算。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2744">A logical exclusive-`Or` operation is performed on its two operands.</span></span>

* <span data-ttu-id="ba6f6-2745">对于 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong`、`Long`和所有枚举类型，将对两个操作数的二进制表示形式的每个位执行指定的操作：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2745">For `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, and all enumerated types, the specified operation is performed on each bit of the binary representation of the two operand(s):</span></span>

  * <span data-ttu-id="ba6f6-2746">`And`：如果两个位均为1，则结果位为 1;否则，结果位为0。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2746">`And`: The result bit is 1 if both bits are 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="ba6f6-2747">`Not`：如果位为0，则结果位为 1;否则，结果位为1。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2747">`Not`: The result bit is 1 if the bit is 0; otherwise the result bit is 1.</span></span>

  * <span data-ttu-id="ba6f6-2748">`Or`：如果任一位为1，则结果位为 1;否则，结果位为0。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2748">`Or`: The result bit is 1 if either bit is 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="ba6f6-2749">`Xor`：如果任一位为1，但不是两个位，则结果位为 1;否则，结果位为0（即 1 `Xor` 0 = 1，1 `Xor` 1 = 0）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2749">`Xor`: The result bit is 1 if either bit is 1 but not both bits; otherwise the result bit is 0 (that is, 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).</span></span>

* <span data-ttu-id="ba6f6-2750">当对类型 `Boolean?``And` 和 `Or` 提升逻辑运算符时，将对其进行扩展，使其包含三值布尔逻辑，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2750">When the logical operators `And` and `Or` are lifted for the type `Boolean?`, they are extended to encompass three-valued Boolean logic as such:</span></span>

  * <span data-ttu-id="ba6f6-2751">如果两个操作数都为 true，则 `And` 的计算结果为 true;如果其中一个操作数为 false，则为 false;否则 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2751">`And` evaluates to true if both operands are true; false if one of the operands is false; `Nothing` otherwise.</span></span>

  * <span data-ttu-id="ba6f6-2752">如果任意一个操作数为 true，则 `Or` 的计算结果为 true;false 是两个操作数均为 false;否则 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2752">`Or` evaluates to true if either operand is true; false is both operands are false; `Nothing` otherwise.</span></span>

<span data-ttu-id="ba6f6-2753">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2753">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x?, y? As Boolean

        x = Nothing
        y = True 

        If x Or y Then
            ' Will execute
        End If
    End Sub
End Module
```

<span data-ttu-id="ba6f6-2754">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2754">__Note.__</span></span> <span data-ttu-id="ba6f6-2755">理想情况下，将使用三值逻辑对逻辑运算符 `And` 和 `Or` 使用三值逻辑，可用于在布尔表达式中使用的任何类型（即实现 `IsTrue` 和 `IsFalse`的类型），其方式与在可在布尔表达式中使用的任何类型之间 `AndAlso` 和 `OrElse` 短线路的方式相同。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2755">Ideally, the logical operators `And` and `Or` would be lifted using three-valued logic for any type that can be used in a Boolean expression (i.e. a type that implements `IsTrue` and `IsFalse`), in the same way that `AndAlso` and `OrElse` short circuit across any type that can be used in a Boolean expression.</span></span> <span data-ttu-id="ba6f6-2756">遗憾的是，三值提升仅适用于 `Boolean?`，因此需要三值逻辑的用户定义类型必须通过定义 `And` 和 `Or` 运算符作为可为 null 的版本来手动完成此操作。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2756">Unfortunately, three-valued lifting is only applied to `Boolean?`, so user-defined types that desire three-valued logic must do so manually by defining `And` and `Or` operators for their nullable version.</span></span>

<span data-ttu-id="ba6f6-2757">这些操作不能有溢出。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2757">No overflows are possible from these operations.</span></span> <span data-ttu-id="ba6f6-2758">枚举类型运算符对枚举类型的基础类型执行按位运算，但返回值为枚举类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2758">The enumerated type operators do the bitwise operation on the underlying type of the enumerated type, but the return value is the enumerated type.</span></span>

<span data-ttu-id="ba6f6-2759">__不是操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2759">__Not Operation Type:__</span></span>

| <span data-ttu-id="ba6f6-2760">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2760">__Bo__</span></span> | <span data-ttu-id="ba6f6-2761">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2761">__SB__</span></span> | <span data-ttu-id="ba6f6-2762">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2762">__By__</span></span> | <span data-ttu-id="ba6f6-2763">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2763">__Sh__</span></span> | <span data-ttu-id="ba6f6-2764">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2764">__US__</span></span> | <span data-ttu-id="ba6f6-2765">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2765">__In__</span></span> | <span data-ttu-id="ba6f6-2766">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2766">__UI__</span></span> | <span data-ttu-id="ba6f6-2767">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2767">__Lo__</span></span> | <span data-ttu-id="ba6f6-2768">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2768">__UL__</span></span> | <span data-ttu-id="ba6f6-2769">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2769">__De__</span></span> | <span data-ttu-id="ba6f6-2770">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2770">__Si__</span></span> | <span data-ttu-id="ba6f6-2771">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2771">__Do__</span></span> | <span data-ttu-id="ba6f6-2772">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2772">__Da__</span></span>  | <span data-ttu-id="ba6f6-2773">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2773">__Ch__</span></span>  | <span data-ttu-id="ba6f6-2774">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2774">__St__</span></span> | <span data-ttu-id="ba6f6-2775">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2775">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ba6f6-2776">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2776">Bo</span></span> | <span data-ttu-id="ba6f6-2777">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2777">SB</span></span> | <span data-ttu-id="ba6f6-2778">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2778">By</span></span> | <span data-ttu-id="ba6f6-2779">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2779">Sh</span></span> | <span data-ttu-id="ba6f6-2780">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2780">US</span></span> | <span data-ttu-id="ba6f6-2781">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2781">In</span></span> | <span data-ttu-id="ba6f6-2782">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2782">UI</span></span> | <span data-ttu-id="ba6f6-2783">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2783">Lo</span></span> | <span data-ttu-id="ba6f6-2784">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2784">UL</span></span> | <span data-ttu-id="ba6f6-2785">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2785">Lo</span></span> | <span data-ttu-id="ba6f6-2786">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2786">Lo</span></span> | <span data-ttu-id="ba6f6-2787">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2787">Lo</span></span> | <span data-ttu-id="ba6f6-2788">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2788">Err</span></span> | <span data-ttu-id="ba6f6-2789">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2789">Err</span></span> | <span data-ttu-id="ba6f6-2790">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2790">Lo</span></span> | <span data-ttu-id="ba6f6-2791">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2791">Ob</span></span> | 

<span data-ttu-id="ba6f6-2792">__And、Or、Xor 运算类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2792">__And, Or, Xor Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-2793">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2793">__Bo__</span></span> | <span data-ttu-id="ba6f6-2794">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2794">__SB__</span></span> | <span data-ttu-id="ba6f6-2795">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2795">__By__</span></span> | <span data-ttu-id="ba6f6-2796">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2796">__Sh__</span></span> | <span data-ttu-id="ba6f6-2797">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2797">__US__</span></span> | <span data-ttu-id="ba6f6-2798">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2798">__In__</span></span> | <span data-ttu-id="ba6f6-2799">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2799">__UI__</span></span> | <span data-ttu-id="ba6f6-2800">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2800">__Lo__</span></span> | <span data-ttu-id="ba6f6-2801">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2801">__UL__</span></span> | <span data-ttu-id="ba6f6-2802">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2802">__De__</span></span> | <span data-ttu-id="ba6f6-2803">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2803">__Si__</span></span> | <span data-ttu-id="ba6f6-2804">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2804">__Do__</span></span> | <span data-ttu-id="ba6f6-2805">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2805">__Da__</span></span>  | <span data-ttu-id="ba6f6-2806">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2806">__Ch__</span></span>  | <span data-ttu-id="ba6f6-2807">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2807">__St__</span></span> | <span data-ttu-id="ba6f6-2808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2808">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ba6f6-2809">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2809">__Bo__</span></span> | <span data-ttu-id="ba6f6-2810">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2810">Bo</span></span> | <span data-ttu-id="ba6f6-2811">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2811">SB</span></span> | <span data-ttu-id="ba6f6-2812">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2812">Sh</span></span> | <span data-ttu-id="ba6f6-2813">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2813">Sh</span></span> | <span data-ttu-id="ba6f6-2814">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2814">In</span></span> | <span data-ttu-id="ba6f6-2815">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2815">In</span></span> | <span data-ttu-id="ba6f6-2816">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2816">Lo</span></span> | <span data-ttu-id="ba6f6-2817">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2817">Lo</span></span> | <span data-ttu-id="ba6f6-2818">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2818">Lo</span></span> | <span data-ttu-id="ba6f6-2819">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2819">Lo</span></span> | <span data-ttu-id="ba6f6-2820">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2820">Lo</span></span> | <span data-ttu-id="ba6f6-2821">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2821">Lo</span></span> | <span data-ttu-id="ba6f6-2822">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2822">Err</span></span> | <span data-ttu-id="ba6f6-2823">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2823">Err</span></span> | <span data-ttu-id="ba6f6-2824">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2824">Bo</span></span>  | <span data-ttu-id="ba6f6-2825">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2825">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2826">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2826">__SB__</span></span> |    | <span data-ttu-id="ba6f6-2827">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2827">SB</span></span> | <span data-ttu-id="ba6f6-2828">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2828">Sh</span></span> | <span data-ttu-id="ba6f6-2829">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2829">Sh</span></span> | <span data-ttu-id="ba6f6-2830">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2830">In</span></span> | <span data-ttu-id="ba6f6-2831">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2831">In</span></span> | <span data-ttu-id="ba6f6-2832">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2832">Lo</span></span> | <span data-ttu-id="ba6f6-2833">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2833">Lo</span></span> | <span data-ttu-id="ba6f6-2834">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2834">Lo</span></span> | <span data-ttu-id="ba6f6-2835">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2835">Lo</span></span> | <span data-ttu-id="ba6f6-2836">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2836">Lo</span></span> | <span data-ttu-id="ba6f6-2837">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2837">Lo</span></span> | <span data-ttu-id="ba6f6-2838">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2838">Err</span></span> | <span data-ttu-id="ba6f6-2839">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2839">Err</span></span> | <span data-ttu-id="ba6f6-2840">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2840">Lo</span></span>  | <span data-ttu-id="ba6f6-2841">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2841">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2842">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2842">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-2843">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2843">By</span></span> | <span data-ttu-id="ba6f6-2844">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2844">Sh</span></span> | <span data-ttu-id="ba6f6-2845">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2845">US</span></span> | <span data-ttu-id="ba6f6-2846">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2846">In</span></span> | <span data-ttu-id="ba6f6-2847">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2847">UI</span></span> | <span data-ttu-id="ba6f6-2848">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2848">Lo</span></span> | <span data-ttu-id="ba6f6-2849">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2849">UL</span></span> | <span data-ttu-id="ba6f6-2850">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2850">Lo</span></span> | <span data-ttu-id="ba6f6-2851">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2851">Lo</span></span> | <span data-ttu-id="ba6f6-2852">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2852">Lo</span></span> | <span data-ttu-id="ba6f6-2853">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2853">Err</span></span> | <span data-ttu-id="ba6f6-2854">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2854">Err</span></span> | <span data-ttu-id="ba6f6-2855">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2855">Lo</span></span>  | <span data-ttu-id="ba6f6-2856">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2856">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2857">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2857">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-2858">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2858">Sh</span></span> | <span data-ttu-id="ba6f6-2859">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2859">In</span></span> | <span data-ttu-id="ba6f6-2860">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2860">In</span></span> | <span data-ttu-id="ba6f6-2861">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2861">Lo</span></span> | <span data-ttu-id="ba6f6-2862">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2862">Lo</span></span> | <span data-ttu-id="ba6f6-2863">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2863">Lo</span></span> | <span data-ttu-id="ba6f6-2864">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2864">Lo</span></span> | <span data-ttu-id="ba6f6-2865">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2865">Lo</span></span> | <span data-ttu-id="ba6f6-2866">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2866">Lo</span></span> | <span data-ttu-id="ba6f6-2867">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2867">Err</span></span> | <span data-ttu-id="ba6f6-2868">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2868">Err</span></span> | <span data-ttu-id="ba6f6-2869">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2869">Lo</span></span>  | <span data-ttu-id="ba6f6-2870">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2870">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2871">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2871">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-2872">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2872">US</span></span> | <span data-ttu-id="ba6f6-2873">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2873">In</span></span> | <span data-ttu-id="ba6f6-2874">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2874">UI</span></span> | <span data-ttu-id="ba6f6-2875">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2875">Lo</span></span> | <span data-ttu-id="ba6f6-2876">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2876">UL</span></span> | <span data-ttu-id="ba6f6-2877">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2877">Lo</span></span> | <span data-ttu-id="ba6f6-2878">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2878">Lo</span></span> | <span data-ttu-id="ba6f6-2879">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2879">Lo</span></span> | <span data-ttu-id="ba6f6-2880">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2880">Err</span></span> | <span data-ttu-id="ba6f6-2881">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2881">Err</span></span> | <span data-ttu-id="ba6f6-2882">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2882">Lo</span></span>  | <span data-ttu-id="ba6f6-2883">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2883">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2884">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2884">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-2885">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2885">In</span></span> | <span data-ttu-id="ba6f6-2886">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2886">Lo</span></span> | <span data-ttu-id="ba6f6-2887">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2887">Lo</span></span> | <span data-ttu-id="ba6f6-2888">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2888">Lo</span></span> | <span data-ttu-id="ba6f6-2889">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2889">Lo</span></span> | <span data-ttu-id="ba6f6-2890">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2890">Lo</span></span> | <span data-ttu-id="ba6f6-2891">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2891">Lo</span></span> | <span data-ttu-id="ba6f6-2892">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2892">Err</span></span> | <span data-ttu-id="ba6f6-2893">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2893">Err</span></span> | <span data-ttu-id="ba6f6-2894">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2894">Lo</span></span>  | <span data-ttu-id="ba6f6-2895">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2895">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2896">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2896">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2897">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2897">UI</span></span> | <span data-ttu-id="ba6f6-2898">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2898">Lo</span></span> | <span data-ttu-id="ba6f6-2899">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2899">UL</span></span> | <span data-ttu-id="ba6f6-2900">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2900">Lo</span></span> | <span data-ttu-id="ba6f6-2901">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2901">Lo</span></span> | <span data-ttu-id="ba6f6-2902">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2902">Lo</span></span> | <span data-ttu-id="ba6f6-2903">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2903">Err</span></span> | <span data-ttu-id="ba6f6-2904">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2904">Err</span></span> | <span data-ttu-id="ba6f6-2905">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2905">Lo</span></span>  | <span data-ttu-id="ba6f6-2906">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2906">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2907">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2907">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2908">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2908">Lo</span></span> | <span data-ttu-id="ba6f6-2909">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2909">Lo</span></span> | <span data-ttu-id="ba6f6-2910">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2910">Lo</span></span> | <span data-ttu-id="ba6f6-2911">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2911">Lo</span></span> | <span data-ttu-id="ba6f6-2912">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2912">Lo</span></span> | <span data-ttu-id="ba6f6-2913">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2913">Err</span></span> | <span data-ttu-id="ba6f6-2914">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2914">Err</span></span> | <span data-ttu-id="ba6f6-2915">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2915">Lo</span></span>  | <span data-ttu-id="ba6f6-2916">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2916">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2917">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2917">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2918">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2918">UL</span></span> | <span data-ttu-id="ba6f6-2919">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2919">Lo</span></span> | <span data-ttu-id="ba6f6-2920">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2920">Lo</span></span> | <span data-ttu-id="ba6f6-2921">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2921">Lo</span></span> | <span data-ttu-id="ba6f6-2922">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2922">Err</span></span> | <span data-ttu-id="ba6f6-2923">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2923">Err</span></span> | <span data-ttu-id="ba6f6-2924">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2924">Lo</span></span>  | <span data-ttu-id="ba6f6-2925">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2925">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2926">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2926">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2927">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2927">Lo</span></span> | <span data-ttu-id="ba6f6-2928">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2928">Lo</span></span> | <span data-ttu-id="ba6f6-2929">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2929">Lo</span></span> | <span data-ttu-id="ba6f6-2930">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2930">Err</span></span> | <span data-ttu-id="ba6f6-2931">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2931">Err</span></span> | <span data-ttu-id="ba6f6-2932">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2932">Lo</span></span>  | <span data-ttu-id="ba6f6-2933">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2933">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2934">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2934">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2935">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2935">Lo</span></span> | <span data-ttu-id="ba6f6-2936">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2936">Lo</span></span> | <span data-ttu-id="ba6f6-2937">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2937">Err</span></span> | <span data-ttu-id="ba6f6-2938">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2938">Err</span></span> | <span data-ttu-id="ba6f6-2939">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2939">Lo</span></span>  | <span data-ttu-id="ba6f6-2940">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2940">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2941">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2941">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2942">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2942">Lo</span></span> | <span data-ttu-id="ba6f6-2943">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2943">Err</span></span> | <span data-ttu-id="ba6f6-2944">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2944">Err</span></span> | <span data-ttu-id="ba6f6-2945">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2945">Lo</span></span>  | <span data-ttu-id="ba6f6-2946">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2946">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2947">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2947">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-2948">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2948">Err</span></span> | <span data-ttu-id="ba6f6-2949">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2949">Err</span></span> | <span data-ttu-id="ba6f6-2950">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2950">Err</span></span> | <span data-ttu-id="ba6f6-2951">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2951">Err</span></span> | 
| <span data-ttu-id="ba6f6-2952">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2952">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-2953">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2953">Err</span></span> | <span data-ttu-id="ba6f6-2954">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2954">Err</span></span> | <span data-ttu-id="ba6f6-2955">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2955">Err</span></span> | 
| <span data-ttu-id="ba6f6-2956">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2956">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-2957">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2957">Lo</span></span>  | <span data-ttu-id="ba6f6-2958">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2958">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-2959">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2959">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ba6f6-2960">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2960">Ob</span></span>  | 


### <a name="short-circuiting-logical-operators"></a><span data-ttu-id="ba6f6-2961">短路逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2961">Short-circuiting Logical Operators</span></span>

<span data-ttu-id="ba6f6-2962">`AndAlso` 和 `OrElse` 运算符是 `And` 和 `Or` 逻辑运算符的短路版本。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2962">The `AndAlso` and `OrElse` operators are the short-circuiting versions of the `And` and `Or` logical operators.</span></span>

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-2963">由于其短路行为，如果在计算第一个操作数后知道运算符结果，则不会在运行时计算第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2963">Because of their short circuiting behavior, the second operand is not evaluated at run time if the operator result is known after evaluating the first operand.</span></span>

<span data-ttu-id="ba6f6-2964">短路逻辑运算符按如下方式计算：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2964">The short-circuiting logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="ba6f6-2965">如果 `AndAlso` 运算中的第一个操作数的计算结果为 `False` 或从其 `IsFalse` 运算符返回 True，则该表达式将返回其第一个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2965">If the first operand in an `AndAlso` operation evaluates to `False` or returns True from its `IsFalse` operator, the expression returns its first operand.</span></span> <span data-ttu-id="ba6f6-2966">否则，将计算第二个操作数，并对两个结果执行逻辑 `And` 运算。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2966">Otherwise, the second operand is evaluated and a logical `And` operation is performed on the two results.</span></span>

* <span data-ttu-id="ba6f6-2967">如果 `OrElse` 运算中的第一个操作数的计算结果为 `True` 或从其 `IsTrue` 运算符返回 True，则该表达式将返回其第一个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2967">If the first operand in an `OrElse` operation evaluates to `True` or returns True from its `IsTrue` operator, the expression returns its first operand.</span></span> <span data-ttu-id="ba6f6-2968">否则，将计算第二个操作数，并对其两个结果执行逻辑 `Or` 运算。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2968">Otherwise, the second operand is evaluated and a logical `Or` operation is performed on its two results.</span></span>

<span data-ttu-id="ba6f6-2969">`AndAlso` 和 `OrElse` 运算符是为类型 `Boolean`或重载以下运算符的任何类型 `T` 定义的：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2969">The `AndAlso` and `OrElse` operators are defined for the type `Boolean`, or for any type `T` that overloads the following operators:</span></span>

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

<span data-ttu-id="ba6f6-2970">以及重载相应的 `And` 或 `Or` 运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2970">as well as overloading the corresponding `And` or `Or` operator:</span></span>

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

<span data-ttu-id="ba6f6-2971">计算 `AndAlso` 或 `OrElse` 运算符时，第一个操作数只计算一次，第二个操作数不计算或只计算一次。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2971">When evaluating the `AndAlso` or `OrElse` operators, the first operand is evaluated only once, and the second operand is either not evaluated or evaluated exactly once.</span></span> <span data-ttu-id="ba6f6-2972">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2972">For example, consider the following code:</span></span>

```vb
Module Test
    Function TrueValue() As Boolean
        Console.Write(" True")
        Return True
    End Function

    Function FalseValue() As Boolean
        Console.Write(" False")
        Return False
    End Function

    Sub Main()
        Console.Write("And:")
        If FalseValue() And TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("Or:")
        If TrueValue() Or FalseValue() Then
        End If
        Console.WriteLine()

        Console.Write("AndAlso:")
        If FalseValue() AndAlso TrueValue() Then
        End If
        Console.WriteLine()

        Console.Write("OrElse:")
        If TrueValue() OrElse FalseValue() Then
        End If
        Console.WriteLine()
    End Sub
End Module
```

<span data-ttu-id="ba6f6-2973">它打印以下结果：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2973">It prints the following result:</span></span>

```console
And: False True
Or: True False
AndAlso: False
OrElse: True
```

<span data-ttu-id="ba6f6-2974">在 `AndAlso` 和 `OrElse` 运算符的提升形式中，如果第一个操作数为 null `Boolean?`，则计算第二个操作数，但结果始终为 null `Boolean?`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2974">In the lifted form of the `AndAlso` and `OrElse` operators, if the first operand was a null `Boolean?`, then the second operand is evaluated but the result is always a null `Boolean?`.</span></span>

<span data-ttu-id="ba6f6-2975">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2975">__Operation Type:__</span></span>

|        | <span data-ttu-id="ba6f6-2976">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2976">__Bo__</span></span> | <span data-ttu-id="ba6f6-2977">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2977">__SB__</span></span> | <span data-ttu-id="ba6f6-2978">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2978">__By__</span></span> | <span data-ttu-id="ba6f6-2979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2979">__Sh__</span></span> | <span data-ttu-id="ba6f6-2980">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2980">__US__</span></span> | <span data-ttu-id="ba6f6-2981">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2981">__In__</span></span> | <span data-ttu-id="ba6f6-2982">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2982">__UI__</span></span> | <span data-ttu-id="ba6f6-2983">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2983">__Lo__</span></span> | <span data-ttu-id="ba6f6-2984">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2984">__UL__</span></span> | <span data-ttu-id="ba6f6-2985">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2985">__De__</span></span> | <span data-ttu-id="ba6f6-2986">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2986">__Si__</span></span> | <span data-ttu-id="ba6f6-2987">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2987">__Do__</span></span> | <span data-ttu-id="ba6f6-2988">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2988">__Da__</span></span>  | <span data-ttu-id="ba6f6-2989">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2989">__Ch__</span></span>  | <span data-ttu-id="ba6f6-2990">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2990">__St__</span></span> | <span data-ttu-id="ba6f6-2991">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2991">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="ba6f6-2992">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2992">__Bo__</span></span> | <span data-ttu-id="ba6f6-2993">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2993">Bo</span></span> | <span data-ttu-id="ba6f6-2994">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2994">Bo</span></span> | <span data-ttu-id="ba6f6-2995">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2995">Bo</span></span> | <span data-ttu-id="ba6f6-2996">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2996">Bo</span></span> | <span data-ttu-id="ba6f6-2997">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2997">Bo</span></span> | <span data-ttu-id="ba6f6-2998">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2998">Bo</span></span> | <span data-ttu-id="ba6f6-2999">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-2999">Bo</span></span> | <span data-ttu-id="ba6f6-3000">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3000">Bo</span></span> | <span data-ttu-id="ba6f6-3001">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3001">Bo</span></span> | <span data-ttu-id="ba6f6-3002">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3002">Bo</span></span> | <span data-ttu-id="ba6f6-3003">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3003">Bo</span></span> | <span data-ttu-id="ba6f6-3004">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3004">Bo</span></span> | <span data-ttu-id="ba6f6-3005">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3005">Err</span></span> | <span data-ttu-id="ba6f6-3006">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3006">Err</span></span> | <span data-ttu-id="ba6f6-3007">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3007">Bo</span></span>  | <span data-ttu-id="ba6f6-3008">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3008">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3009">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3009">__SB__</span></span> |    | <span data-ttu-id="ba6f6-3010">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3010">Bo</span></span> | <span data-ttu-id="ba6f6-3011">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3011">Bo</span></span> | <span data-ttu-id="ba6f6-3012">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3012">Bo</span></span> | <span data-ttu-id="ba6f6-3013">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3013">Bo</span></span> | <span data-ttu-id="ba6f6-3014">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3014">Bo</span></span> | <span data-ttu-id="ba6f6-3015">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3015">Bo</span></span> | <span data-ttu-id="ba6f6-3016">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3016">Bo</span></span> | <span data-ttu-id="ba6f6-3017">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3017">Bo</span></span> | <span data-ttu-id="ba6f6-3018">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3018">Bo</span></span> | <span data-ttu-id="ba6f6-3019">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3019">Bo</span></span> | <span data-ttu-id="ba6f6-3020">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3020">Bo</span></span> | <span data-ttu-id="ba6f6-3021">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3021">Err</span></span> | <span data-ttu-id="ba6f6-3022">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3022">Err</span></span> | <span data-ttu-id="ba6f6-3023">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3023">Bo</span></span>  | <span data-ttu-id="ba6f6-3024">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3024">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3025">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3025">__By__</span></span> |    |    | <span data-ttu-id="ba6f6-3026">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3026">Bo</span></span> | <span data-ttu-id="ba6f6-3027">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3027">Bo</span></span> | <span data-ttu-id="ba6f6-3028">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3028">Bo</span></span> | <span data-ttu-id="ba6f6-3029">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3029">Bo</span></span> | <span data-ttu-id="ba6f6-3030">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3030">Bo</span></span> | <span data-ttu-id="ba6f6-3031">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3031">Bo</span></span> | <span data-ttu-id="ba6f6-3032">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3032">Bo</span></span> | <span data-ttu-id="ba6f6-3033">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3033">Bo</span></span> | <span data-ttu-id="ba6f6-3034">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3034">Bo</span></span> | <span data-ttu-id="ba6f6-3035">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3035">Bo</span></span> | <span data-ttu-id="ba6f6-3036">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3036">Err</span></span> | <span data-ttu-id="ba6f6-3037">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3037">Err</span></span> | <span data-ttu-id="ba6f6-3038">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3038">Bo</span></span>  | <span data-ttu-id="ba6f6-3039">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3039">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3040">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3040">__Sh__</span></span> |    |    |    | <span data-ttu-id="ba6f6-3041">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3041">Bo</span></span> | <span data-ttu-id="ba6f6-3042">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3042">Bo</span></span> | <span data-ttu-id="ba6f6-3043">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3043">Bo</span></span> | <span data-ttu-id="ba6f6-3044">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3044">Bo</span></span> | <span data-ttu-id="ba6f6-3045">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3045">Bo</span></span> | <span data-ttu-id="ba6f6-3046">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3046">Bo</span></span> | <span data-ttu-id="ba6f6-3047">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3047">Bo</span></span> | <span data-ttu-id="ba6f6-3048">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3048">Bo</span></span> | <span data-ttu-id="ba6f6-3049">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3049">Bo</span></span> | <span data-ttu-id="ba6f6-3050">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3050">Err</span></span> | <span data-ttu-id="ba6f6-3051">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3051">Err</span></span> | <span data-ttu-id="ba6f6-3052">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3052">Bo</span></span>  | <span data-ttu-id="ba6f6-3053">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3053">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3054">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3054">__US__</span></span> |    |    |    |    | <span data-ttu-id="ba6f6-3055">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3055">Bo</span></span> | <span data-ttu-id="ba6f6-3056">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3056">Bo</span></span> | <span data-ttu-id="ba6f6-3057">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3057">Bo</span></span> | <span data-ttu-id="ba6f6-3058">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3058">Bo</span></span> | <span data-ttu-id="ba6f6-3059">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3059">Bo</span></span> | <span data-ttu-id="ba6f6-3060">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3060">Bo</span></span> | <span data-ttu-id="ba6f6-3061">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3061">Bo</span></span> | <span data-ttu-id="ba6f6-3062">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3062">Bo</span></span> | <span data-ttu-id="ba6f6-3063">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3063">Err</span></span> | <span data-ttu-id="ba6f6-3064">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3064">Err</span></span> | <span data-ttu-id="ba6f6-3065">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3065">Bo</span></span>  | <span data-ttu-id="ba6f6-3066">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3066">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3067">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3067">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="ba6f6-3068">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3068">Bo</span></span> | <span data-ttu-id="ba6f6-3069">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3069">Bo</span></span> | <span data-ttu-id="ba6f6-3070">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3070">Bo</span></span> | <span data-ttu-id="ba6f6-3071">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3071">Bo</span></span> | <span data-ttu-id="ba6f6-3072">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3072">Bo</span></span> | <span data-ttu-id="ba6f6-3073">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3073">Bo</span></span> | <span data-ttu-id="ba6f6-3074">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3074">Bo</span></span> | <span data-ttu-id="ba6f6-3075">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3075">Err</span></span> | <span data-ttu-id="ba6f6-3076">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3076">Err</span></span> | <span data-ttu-id="ba6f6-3077">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3077">Bo</span></span>  | <span data-ttu-id="ba6f6-3078">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3078">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3079">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3079">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="ba6f6-3080">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3080">Bo</span></span> | <span data-ttu-id="ba6f6-3081">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3081">Bo</span></span> | <span data-ttu-id="ba6f6-3082">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3082">Bo</span></span> | <span data-ttu-id="ba6f6-3083">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3083">Bo</span></span> | <span data-ttu-id="ba6f6-3084">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3084">Bo</span></span> | <span data-ttu-id="ba6f6-3085">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3085">Bo</span></span> | <span data-ttu-id="ba6f6-3086">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3086">Err</span></span> | <span data-ttu-id="ba6f6-3087">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3087">Err</span></span> | <span data-ttu-id="ba6f6-3088">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3088">Bo</span></span>  | <span data-ttu-id="ba6f6-3089">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3089">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3090">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3090">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-3091">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3091">Bo</span></span> | <span data-ttu-id="ba6f6-3092">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3092">Bo</span></span> | <span data-ttu-id="ba6f6-3093">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3093">Bo</span></span> | <span data-ttu-id="ba6f6-3094">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3094">Bo</span></span> | <span data-ttu-id="ba6f6-3095">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3095">Bo</span></span> | <span data-ttu-id="ba6f6-3096">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3096">Err</span></span> | <span data-ttu-id="ba6f6-3097">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3097">Err</span></span> | <span data-ttu-id="ba6f6-3098">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3098">Bo</span></span>  | <span data-ttu-id="ba6f6-3099">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3099">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3100">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3100">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-3101">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3101">Bo</span></span> | <span data-ttu-id="ba6f6-3102">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3102">Bo</span></span> | <span data-ttu-id="ba6f6-3103">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3103">Bo</span></span> | <span data-ttu-id="ba6f6-3104">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3104">Bo</span></span> | <span data-ttu-id="ba6f6-3105">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3105">Err</span></span> | <span data-ttu-id="ba6f6-3106">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3106">Err</span></span> | <span data-ttu-id="ba6f6-3107">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3107">Bo</span></span>  | <span data-ttu-id="ba6f6-3108">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3108">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3109">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3109">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-3110">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3110">Bo</span></span> | <span data-ttu-id="ba6f6-3111">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3111">Bo</span></span> | <span data-ttu-id="ba6f6-3112">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3112">Bo</span></span> | <span data-ttu-id="ba6f6-3113">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3113">Err</span></span> | <span data-ttu-id="ba6f6-3114">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3114">Err</span></span> | <span data-ttu-id="ba6f6-3115">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3115">Bo</span></span>  | <span data-ttu-id="ba6f6-3116">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3116">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3117">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3117">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-3118">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3118">Bo</span></span> | <span data-ttu-id="ba6f6-3119">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3119">Bo</span></span> | <span data-ttu-id="ba6f6-3120">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3120">Err</span></span> | <span data-ttu-id="ba6f6-3121">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3121">Err</span></span> | <span data-ttu-id="ba6f6-3122">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3122">Bo</span></span>  | <span data-ttu-id="ba6f6-3123">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3123">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3124">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3124">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-3125">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3125">Bo</span></span> | <span data-ttu-id="ba6f6-3126">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3126">Err</span></span> | <span data-ttu-id="ba6f6-3127">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3127">Err</span></span> | <span data-ttu-id="ba6f6-3128">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3128">Bo</span></span>  | <span data-ttu-id="ba6f6-3129">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3129">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3130">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3130">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="ba6f6-3131">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3131">Err</span></span> | <span data-ttu-id="ba6f6-3132">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3132">Err</span></span> | <span data-ttu-id="ba6f6-3133">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3133">Err</span></span> | <span data-ttu-id="ba6f6-3134">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3134">Err</span></span> | 
| <span data-ttu-id="ba6f6-3135">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3135">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="ba6f6-3136">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3136">Err</span></span> | <span data-ttu-id="ba6f6-3137">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3137">Err</span></span> | <span data-ttu-id="ba6f6-3138">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3138">Err</span></span> | 
| <span data-ttu-id="ba6f6-3139">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3139">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="ba6f6-3140">Bo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3140">Bo</span></span>  | <span data-ttu-id="ba6f6-3141">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3141">Ob</span></span>  | 
| <span data-ttu-id="ba6f6-3142">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3142">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="ba6f6-3143">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3143">Ob</span></span>  | 


## <a name="shift-operators"></a><span data-ttu-id="ba6f6-3144">移位运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3144">Shift Operators</span></span>

<span data-ttu-id="ba6f6-3145">二元运算符 `<<` 和 `>>` 执行移位运算。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3145">The binary operators `<<` and `>>` perform bit shifting operations.</span></span>

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-3146">为 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long` 类型定义运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3146">The operators are defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long` types.</span></span> <span data-ttu-id="ba6f6-3147">与其他二元运算符不同，移位运算的结果类型被确定为：运算符是只带有左操作数的一元运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3147">Unlike the other binary operators, the result type of a shift operation is determined as if the operator was a unary operator with just the left operand.</span></span> <span data-ttu-id="ba6f6-3148">右操作数的类型必须可隐式转换为 `Integer`，而不用于确定运算的结果类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3148">The type of the right operand must be implicitly convertible to `Integer` and is not used in determining the result type of the operation.</span></span>

<span data-ttu-id="ba6f6-3149">`<<` 运算符导致第一个操作数中的位左移到移位量指定的位数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3149">The `<<` operator causes the bits in the first operand to be shifted left the number of places specified by the shift amount.</span></span> <span data-ttu-id="ba6f6-3150">结果类型范围外的高序位将被丢弃，且低顺序空出的位位置为零填充。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3150">The high-order bits outside the range of the result type are discarded and the low-order vacated bit positions are zero-filled.</span></span>

<span data-ttu-id="ba6f6-3151">`>>` 运算符导致第一个操作数中的位右移由移位量指定的位数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3151">The `>>` operator causes the bits in the first operand to be shifted right the number of places specified by the shift amount.</span></span> <span data-ttu-id="ba6f6-3152">如果左操作数为正值或为负，则将丢弃低序位，并将高顺序空出的位位置设置为零。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3152">The low-order bits are discarded and the high-order vacated bit positions are set to zero if the left operand is positive or to one if negative.</span></span> <span data-ttu-id="ba6f6-3153">如果左操作数的类型为 `Byte`，`UShort`、`UInteger`或 `ULong` 空的高序位为零。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3153">If the left operand is of type `Byte`, `UShort`, `UInteger`, or `ULong` the vacant high-order bits are zero-filled.</span></span>

<span data-ttu-id="ba6f6-3154">移位运算符按第二个操作数的量位移第一个操作数的基础表示形式的位数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3154">The shift operators shift the bits of the underlying representation of the first operand by the amount of the second operand.</span></span> <span data-ttu-id="ba6f6-3155">如果第二个操作数的值大于第一个操作数中的位数，或者为负，则移位量计算为 `RightOperand And SizeMask`，其中 `SizeMask` 为：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3155">If the value of the second operand is greater than the number of bits in the first operand, or is negative, then the shift amount is computed as `RightOperand And SizeMask` where `SizeMask` is:</span></span>

| <span data-ttu-id="ba6f6-3156">__LeftOperand 类型__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3156">__LeftOperand Type__</span></span>  | <span data-ttu-id="ba6f6-3157">__SizeMask__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3157">__SizeMask__</span></span> | 
|-----------------------|--------------|
| <span data-ttu-id="ba6f6-3158">`Byte`, `SByte`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3158">`Byte`, `SByte`</span></span>       | <span data-ttu-id="ba6f6-3159">7 (`&H7`)</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3159">7 (`&H7`)</span></span>    | 
| <span data-ttu-id="ba6f6-3160">`UShort`, `Short`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3160">`UShort`, `Short`</span></span>     | <span data-ttu-id="ba6f6-3161">15 (`&HF`)</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3161">15 (`&HF`)</span></span>   | 
| <span data-ttu-id="ba6f6-3162">`UInteger`, `Integer`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3162">`UInteger`, `Integer`</span></span> | <span data-ttu-id="ba6f6-3163">31 (`&H1F`)</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3163">31 (`&H1F`)</span></span>  | 
| <span data-ttu-id="ba6f6-3164">`ULong`, `Long`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3164">`ULong`, `Long`</span></span>       | <span data-ttu-id="ba6f6-3165">63 (`&H3F`)</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3165">63 (`&H3F`)</span></span>  | 

<span data-ttu-id="ba6f6-3166">如果移位量为零，则操作的结果与第一个操作数的值相同。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3166">If the shift amount is zero, the result of the operation is identical to the value of the first operand.</span></span> <span data-ttu-id="ba6f6-3167">这些操作不能有溢出。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3167">No overflows are possible from these operations.</span></span>

<span data-ttu-id="ba6f6-3168">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3168">__Operation Type:__</span></span>


| <span data-ttu-id="ba6f6-3169">__Bo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3169">__Bo__</span></span> | <span data-ttu-id="ba6f6-3170">__SB__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3170">__SB__</span></span> | <span data-ttu-id="ba6f6-3171">__由__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3171">__By__</span></span> | <span data-ttu-id="ba6f6-3172">__Sh__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3172">__Sh__</span></span> | <span data-ttu-id="ba6f6-3173">__反馈__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3173">__US__</span></span> | <span data-ttu-id="ba6f6-3174">__In__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3174">__In__</span></span> | <span data-ttu-id="ba6f6-3175">__UI__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3175">__UI__</span></span> | <span data-ttu-id="ba6f6-3176">__Lo__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3176">__Lo__</span></span> | <span data-ttu-id="ba6f6-3177">__UL__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3177">__UL__</span></span> | <span data-ttu-id="ba6f6-3178">__取消__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3178">__De__</span></span> | <span data-ttu-id="ba6f6-3179">__Si__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3179">__Si__</span></span> | <span data-ttu-id="ba6f6-3180">__Do__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3180">__Do__</span></span> | <span data-ttu-id="ba6f6-3181">__特里斯坦__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3181">__Da__</span></span>  | <span data-ttu-id="ba6f6-3182">__48__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3182">__Ch__</span></span>  | <span data-ttu-id="ba6f6-3183">__St__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3183">__St__</span></span> | <span data-ttu-id="ba6f6-3184">__Ob__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3184">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="ba6f6-3185">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3185">Sh</span></span> | <span data-ttu-id="ba6f6-3186">SB</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3186">SB</span></span> | <span data-ttu-id="ba6f6-3187">间距</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3187">By</span></span> | <span data-ttu-id="ba6f6-3188">Sh</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3188">Sh</span></span> | <span data-ttu-id="ba6f6-3189">US</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3189">US</span></span> | <span data-ttu-id="ba6f6-3190">在</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3190">In</span></span> | <span data-ttu-id="ba6f6-3191">UI</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3191">UI</span></span> | <span data-ttu-id="ba6f6-3192">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3192">Lo</span></span> | <span data-ttu-id="ba6f6-3193">UL</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3193">UL</span></span> | <span data-ttu-id="ba6f6-3194">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3194">Lo</span></span> | <span data-ttu-id="ba6f6-3195">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3195">Lo</span></span> | <span data-ttu-id="ba6f6-3196">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3196">Lo</span></span> | <span data-ttu-id="ba6f6-3197">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3197">Err</span></span> | <span data-ttu-id="ba6f6-3198">Err</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3198">Err</span></span> | <span data-ttu-id="ba6f6-3199">Lo</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3199">Lo</span></span> | <span data-ttu-id="ba6f6-3200">Ob</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3200">Ob</span></span> | 


## <a name="boolean-expressions"></a><span data-ttu-id="ba6f6-3201">布尔表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3201">Boolean Expressions</span></span>

<span data-ttu-id="ba6f6-3202">布尔表达式是一个表达式，可对其进行测试，以查看其是否为 true 或 false。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3202">A Boolean expression is an expression that can be tested to see if it is true or if it is false.</span></span>

```antlr
BooleanExpression
    : Expression
    ;
```

<span data-ttu-id="ba6f6-3203">如果按优先顺序排序，则可以在布尔表达式中使用类型 `T`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3203">A type `T` can be used in a Boolean expression if, in order of preference:</span></span>

* <span data-ttu-id="ba6f6-3204">`T` 为 `Boolean` 或 `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3204">`T` is `Boolean` or `Boolean?`</span></span>

* <span data-ttu-id="ba6f6-3205">`T` 具有到 `Boolean` 的扩大转换</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3205">`T` has a widening conversion to `Boolean`</span></span>

* <span data-ttu-id="ba6f6-3206">`T` 具有到 `Boolean?` 的扩大转换</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3206">`T` has a widening conversion to `Boolean?`</span></span>

* <span data-ttu-id="ba6f6-3207">`T` 定义了两个伪运算符： `IsTrue` 和 `IsFalse`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3207">`T` defines two pseudo operators, `IsTrue` and `IsFalse`.</span></span>

* <span data-ttu-id="ba6f6-3208">`T` 具有到不涉及从 `Boolean` 到 `Boolean?`转换的 `Boolean?` 的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3208">`T` has a narrowing conversion to `Boolean?` that does not involve a conversion from `Boolean` to `Boolean?`.</span></span>

* <span data-ttu-id="ba6f6-3209">`T` 具有到 `Boolean`的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3209">`T` has a narrowing conversion to `Boolean`.</span></span>

<span data-ttu-id="ba6f6-3210">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3210">__Note.__</span></span> <span data-ttu-id="ba6f6-3211">需要注意的是，如果 `Option Strict` 为 off，则将在没有编译时错误的情况下接受收缩转换到 `Boolean` 的表达式，但如果存在，则该语言仍优先于 `IsTrue` 运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3211">It is interesting to note that if `Option Strict` is off, an expression that has a narrowing conversion to `Boolean` will be accepted without a compile-time error but the language will still prefer an `IsTrue` operator if it exists.</span></span> <span data-ttu-id="ba6f6-3212">这是因为 `Option Strict` 仅更改语言不接受的内容，并且永远不会更改表达式的实际含义。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3212">This is because `Option Strict` only changes what is and isn't accepted by the language, and never changes the actual meaning of an expression.</span></span> <span data-ttu-id="ba6f6-3213">因此，无论是否 `Option Strict`，`IsTrue` 都必须始终优先于收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3213">Thus, `IsTrue` has to always be preferred over a narrowing conversion, regardless of `Option Strict`.</span></span>

<span data-ttu-id="ba6f6-3214">例如，下面的类不定义要 `Boolean`的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3214">For example, the following class does not define a widening conversion to `Boolean`.</span></span> <span data-ttu-id="ba6f6-3215">因此，它在 `If` 语句中的使用将导致调用 `IsTrue` 运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3215">As a result, its use in the `If` statement causes a call to the `IsTrue` operator.</span></span>

```vb
Class MyBool
    Public Shared Widening Operator CType(b As Boolean) As MyBool
        ...
    End Operator

    Public Shared Narrowing Operator CType(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsTrue(b As MyBool) As Boolean
        ...
    End Operator

    Public Shared Operator IsFalse(b As MyBool) As Boolean
        ...
    End Operator
End Class

Module Test
    Sub Main()
        Dim b As New MyBool

        If b Then Console.WriteLine("True")
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3216">如果布尔表达式的类型为或转换为 `Boolean` 或 `Boolean?`，则如果值为 `True`，则为 true; 否则为 false。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3216">If a Boolean expression is typed as or converted to `Boolean` or `Boolean?`, then it is true if the value is `True` and false otherwise.</span></span>

<span data-ttu-id="ba6f6-3217">否则，如果运算符返回 `True`，则布尔表达式将调用 `IsTrue` 运算符并返回 `True`;否则为 false （但绝不会调用 `IsFalse` 运算符）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3217">Otherwise, a Boolean expression calls the `IsTrue` operator and returns `True` if the operator returned `True`; otherwise it is false (but never calls the `IsFalse` operator).</span></span>

<span data-ttu-id="ba6f6-3218">在下面的示例中，`Integer` 具有一个到 `Boolean`的收缩转换，因此 null `Integer?` 会将收缩转换为 `Boolean?` （生成 null `Boolean`）和 `Boolean` （这将引发异常）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3218">In the following example, `Integer` has a narrowing conversion to `Boolean`, so a null `Integer?` has a narrowing conversion to both `Boolean?` (yielding a null `Boolean`) and to `Boolean` (which throws an exception).</span></span> <span data-ttu-id="ba6f6-3219">首选收缩转换到 `Boolean?`，因此 "`i`" 的值为布尔表达式是 `False`的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3219">The narrowing conversion to `Boolean?` is preferred, and so the value of "`i`" as a Boolean expression is therefore `False`.</span></span>

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a><span data-ttu-id="ba6f6-3220">Lambda 表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3220">Lambda Expressions</span></span>

<span data-ttu-id="ba6f6-3221">*Lambda 表达式*定义名为*lambda 方法*的匿名方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3221">A *lambda expression* defines an anonymous method called a *lambda method*.</span></span> <span data-ttu-id="ba6f6-3222">使用 Lambda 方法可以轻松地将 "内联" 方法传递到采用委托类型的其他方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3222">Lambda methods make it easy to pass "in-line" methods to other methods that take delegate types.</span></span>

```antlr
LambdaExpression
    : SingleLineLambda
    | MultiLineLambda
    ;

SingleLineLambda
    : LambdaModifier* 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? Expression
    | 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? Statement
    ;

MultiLineLambda
    : MultiLineFunctionLambda
    | MultiLineSubLambda
    ;

MultiLineFunctionLambda
    : LambdaModifier? 'Function' ( OpenParenthesis ParameterList? CloseParenthesis )? ( 'As' TypeName )? LineTerminator
      Block
      'End' 'Function'
    ;

MultiLineSubLambda
    : LambdaModifier? 'Sub' ( OpenParenthesis ParameterList? CloseParenthesis )? LineTerminator
      Block
      'End' 'Sub'
    ;

LambdaModifier
    : 'Async' | 'Iterator'
    ;
```

<span data-ttu-id="ba6f6-3223">示例：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3223">The example:</span></span>

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, Function(x As Integer) x * 2)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3224">将打印输出：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3224">will print out:</span></span>

```console
2 4 6 8
```

<span data-ttu-id="ba6f6-3225">Lambda 表达式以可选修饰符开头 `Async` 或 `Iterator`，后跟关键字 `Function` 或 `Sub` 和参数列表。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3225">A lambda expression begins with the optional modifiers `Async` or `Iterator`, followed by the keyword `Function` or `Sub` and a parameter list.</span></span> <span data-ttu-id="ba6f6-3226">Lambda 表达式中的参数不能 `Optional` 或 `ParamArray` 声明，也不能具有特性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3226">Parameters in a lambda expression cannot be declared `Optional` or `ParamArray` and cannot have attributes.</span></span> <span data-ttu-id="ba6f6-3227">不同于常规方法，省略 lambda 方法的参数类型不会自动推断 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3227">Unlike regular methods, omitting a parameter type for a lambda method does not automatically infer `Object`.</span></span> <span data-ttu-id="ba6f6-3228">相反，当重新分类 lambda 方法时，将从目标类型推断省略的参数类型和 `ByRef` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3228">Instead, when a lambda method is reclassified, the omitted parameter types and `ByRef` modifiers are inferred from the target type.</span></span> <span data-ttu-id="ba6f6-3229">在前面的示例中，lambda 表达式可以编写为 `Function(x) x * 2`，它将推断在使用 lambda 方法创建 `IntFunc` 委托类型的实例时要 `Integer` 的 `x` 类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3229">In the previous example, the lambda expression could have been written as `Function(x) x * 2`, and it would have inferred the type of `x` to be `Integer` when the lambda method was used to create an instance of the `IntFunc` delegate type.</span></span> <span data-ttu-id="ba6f6-3230">与局部变量推理不同，如果 lambda 方法参数省略了类型但具有数组或可为 null 的名称修饰符，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3230">Unlike local variable inference, if a lambda method parameter omits a type but has an array or nullable name modifier, a compile-time error occurs.</span></span>

<span data-ttu-id="ba6f6-3231">__常规 lambda 表达式__是指既没有 `Async` 也没有 `Iterator` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3231">A __regular lambda expression__ is one with neither `Async` nor `Iterator` modifiers.</span></span>

<span data-ttu-id="ba6f6-3232">__迭代器 lambda 表达式__是 `Iterator` 修饰符，没有 `Async` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3232">An __iterator lambda expression__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="ba6f6-3233">它必须是函数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3233">It must be a function.</span></span> <span data-ttu-id="ba6f6-3234">如果将它重新分类为某个值，则只能将其返回类型为 `IEnumerator`或 `IEnumerable`的委托类型的值重新分类，或对某些 `T`使用 `IEnumerator(Of T)` 或 `IEnumerable(Of T)`，并且没有 ByRef 参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3234">When it is reclassified to a value, it can only be reclassified to a value of delegate type whose return type is `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="ba6f6-3235">__异步 lambda 表达式__是 `Async` 修饰符，没有 `Iterator` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3235">An __async lambda expression__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="ba6f6-3236">Async sub lambda 只能重新分类为不带 ByRef 参数的子委托类型的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3236">An async sub lambda may only be reclassified to a value of sub delegate type with no ByRef parameters.</span></span> <span data-ttu-id="ba6f6-3237">异步函数 lambda 只能重新分类为函数委托类型的值，该类型的返回类型为 `Task` 或 `Task(Of T)` 用于某些 `T`，并且没有 ByRef 参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3237">An async function lambda may only be reclassified to a value of function delegate type whose return type is `Task` or `Task(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="ba6f6-3238">Lambda 表达式可以是单行或多行。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3238">Lambda expressions can either be single-line or multi-line.</span></span> <span data-ttu-id="ba6f6-3239">单行 `Function` lambda 表达式包含一个表达式，该表达式表示从 lambda 方法返回的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3239">Single-line `Function` lambda expressions contain a single expression that represents the value returned from the lambda method.</span></span> <span data-ttu-id="ba6f6-3240">单行 `Sub` lambda 表达式包含单个语句，而不包含其结束 `StatementTerminator`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3240">Single-line `Sub` lambda expressions contain a single statement without its closing `StatementTerminator`.</span></span> <span data-ttu-id="ba6f6-3241">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3241">For example:</span></span>

```vb
Module Test
    Sub Do(a() As Integer, action As Action(Of Integer))
        For index As Integer = 0 To a.Length - 1
            action(a(index))
        Next index
    End Sub

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Do(a, Sub(x As Integer) Console.WriteLine(x))
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3242">单行 lambda 构造与所有其他表达式和语句紧密绑定。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3242">Single-line lambda constructs bind less tightly than all other expressions and statements.</span></span> <span data-ttu-id="ba6f6-3243">例如，"`Function() x + 5`" 等效于 "`Function() (x+5)"` 而不是"`(Function() x) + 5`"。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3243">Thus, for example, "`Function() x + 5`" is equivalent to "`Function() (x+5)"` rather than "`(Function() x) + 5`".</span></span> <span data-ttu-id="ba6f6-3244">为了避免多义性，单行 `Sub` lambda 表达式不能包含 Dim 语句或标签声明语句。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3244">To avoid ambiguity, a single-line `Sub` lambda expression may not contain a Dim statement or a label declaration statement.</span></span> <span data-ttu-id="ba6f6-3245">此外，除非它括在括号中，否则单行 `Sub` lambda 表达式后面不能跟冒号 "："、成员访问运算符 "."、字典成员访问运算符 "！" 或左括号 "（"。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3245">Also, unless it is enclosed in parentheses, a single-line `Sub` lambda expression may not be immediately followed by a colon ":", a member access operator ".", a dictionary member access operator "!" or an open parenthesis "(".</span></span> <span data-ttu-id="ba6f6-3246">它不能包含任何 block 语句（`With`、`SyncLock, If...EndIf`、`While`、`For`、`Do`、`Using`），也不能 `OnError` 和 `Resume`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3246">It may not contain any block statement (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) nor `OnError` nor `Resume`.</span></span>

<span data-ttu-id="ba6f6-3247">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3247">__Note.__</span></span> <span data-ttu-id="ba6f6-3248">在 lambda 表达式 `Function(i) x=i`中，主体被解释为*表达式*（测试 `x` 和 `i` 是否相等）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3248">In the lambda expression `Function(i) x=i`, the body is interpreted as an *expression* (which tests whether `x` and `i` are equal).</span></span> <span data-ttu-id="ba6f6-3249">但在 lambda 表达式 `Sub(i) x=i`中，主体被解释为语句（将 `i` 分配到 `x`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3249">But in the lambda expression `Sub(i) x=i`, the body is interpreted as a statement (which assigns `i` to `x`).</span></span>

<span data-ttu-id="ba6f6-3250">多行 lambda 表达式包含语句块，必须以适当的 `End` 语句（即 `End Function` 或 `End Sub`）结束。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3250">A multi-line lambda expression contains a statement block and must end with an appropriate `End` statement (i.e. `End Function` or `End Sub`).</span></span> <span data-ttu-id="ba6f6-3251">与常规方法一样，多行 lambda 方法的 `Function` 或 `Sub` 语句和 `End` 语句必须在其自己的行上。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3251">As with regular methods, a multi-line lambda method's `Function` or `Sub` statement and `End` statements must be on their own lines.</span></span> <span data-ttu-id="ba6f6-3252">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3252">For example:</span></span>

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

<span data-ttu-id="ba6f6-3253">多行 `Function` lambda 表达式可以声明返回类型，但不能在其上放置属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3253">Multi-line `Function` lambda expressions can declare a return type but cannot put attributes on it.</span></span> <span data-ttu-id="ba6f6-3254">如果多行 `Function` lambda 表达式未声明返回类型，但可以从使用 lambda 表达式的上下文推断返回类型，则使用该返回类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3254">If a multi-line `Function` lambda expression does not declare a return type but the return type can be inferred from the context in which the lambda expression is used , then that return type is used.</span></span> <span data-ttu-id="ba6f6-3255">否则，函数的返回类型计算如下：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3255">Otherwise the return type of the function is calculated as follows:</span></span>

* <span data-ttu-id="ba6f6-3256">在常规 lambda 表达式中，返回类型是语句块中所有 `Return` 语句中表达式的主导类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3256">In a regular lambda expression, the return type is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="ba6f6-3257">在异步 lambda 表达式中，返回类型为 `Task(Of T)`，其中 `T` 是语句块中所有 `Return` 语句中的表达式的主导类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3257">In an async lambda expression, the return type is `Task(Of T)` where `T` is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="ba6f6-3258">在迭代器 lambda 表达式中，返回类型为 `IEnumerable(Of T)`，其中 `T` 是语句块中所有 `Yield` 语句中的表达式的主导类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3258">In an iterator lambda expression, the return type is `IEnumerable(Of T)` where `T` is the dominant type of the expressions in all the `Yield` statements in the statement block.</span></span>

<span data-ttu-id="ba6f6-3259">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3259">For example:</span></span>

```vb
Function f(min As Integer, max As Integer) As IEnumerable(Of Integer)
    If min > max Then Throw New ArgumentException()
    Dim x = Iterator Function()
                  For i = min To max
                    Yield i
                Next
               End Function

    ' infers x to be a delegate with return type IEnumerable(Of Integer)
    Return x()
End Function
```

<span data-ttu-id="ba6f6-3260">在所有情况下，如果没有 `Return` （分别 `Yield`）语句，或者它们之间没有主导类型，并且使用了严格语义，则会发生编译时错误;否则，将隐式 `Object`主导类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3260">In all cases, if there are no `Return` (respectively `Yield`) statements, or if there is no dominant type among them, and strict semantics are being used, a compile-time error occurs; otherwise the dominant type is implicitly `Object`.</span></span>

<span data-ttu-id="ba6f6-3261">请注意，从所有 `Return` 语句计算返回类型，即使它们不可访问也是如此。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3261">Note that the return type is calculated from all `Return` statements, even if they are not reachable.</span></span> <span data-ttu-id="ba6f6-3262">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3262">For example:</span></span>

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

<span data-ttu-id="ba6f6-3263">没有隐式返回变量，因为变量没有名称。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3263">There is no implicit return variable, as there is no name for the variable.</span></span>

<span data-ttu-id="ba6f6-3264">多行 lambda 表达式中的语句块具有以下限制：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3264">The statement blocks inside multi-line lambda expressions have the following restrictions:</span></span>

* <span data-ttu-id="ba6f6-3265">尽管允许 `Try` 语句，但不允许 `On Error` 和 `Resume` 语句。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3265">`On Error` and `Resume` statements are not allowed, although `Try` statements are allowed.</span></span>

* <span data-ttu-id="ba6f6-3266">不能在多行 lambda 表达式中声明静态局部变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3266">Static locals cannot be declared in multi-line lambda expressions.</span></span>

* <span data-ttu-id="ba6f6-3267">尽管在多行 lambda 表达式的语句块中应用正常的分支规则，但不能对其进行分支或跳出。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3267">It is not possible to branch into or out of the statement block of a multi-line lambda expression, although the normal branching rules apply within it.</span></span> <span data-ttu-id="ba6f6-3268">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3268">For example:</span></span>

  ```vb
  Label1:
  Dim x = Sub()
                 ' Error: Cannot branch out
                 GoTo Label1

                 ' OK: Wholly within the lamba.
                 GoTo Label2:
            Label2:
            End Sub

  ' Error: Cannot branch in
  GoTo Label2
  ```

<span data-ttu-id="ba6f6-3269">Lambda 表达式大致等效于对包含类型声明的匿名方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3269">A lambda expression is roughly equivalent to an anonymous method declared on the containing type.</span></span> <span data-ttu-id="ba6f6-3270">初始示例大致等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3270">The initial example is roughly equivalent to:</span></span>

```vb
Module Test
    Delegate Function IntFunc(x As Integer) As Integer

    Sub Apply(a() As Integer, func As IntFunc)
        For index As Integer = 0 To a.Length - 1
            a(index) = func(a(index))
        Next index
    End Sub

    Function $Lambda1(x As Integer) As Integer
        Return x * 2
    End Function

    Sub Main()
        Dim a() As Integer = { 1, 2, 3, 4 }

        Apply(a, AddressOf $Lambda1)

        For Each value In a
            Console.Write(value & " ")
        Next value
    End Sub
End Module
```


### <a name="closures"></a><span data-ttu-id="ba6f6-3271">闭包</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3271">Closures</span></span>

<span data-ttu-id="ba6f6-3272">Lambda 表达式可以访问范围内的所有变量，包括在包含方法和 lambda 表达式中定义的局部变量或参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3272">Lambda expressions have access to all of the variables in scope, including local variables or parameters defined in the containing method and lambda expressions.</span></span> <span data-ttu-id="ba6f6-3273">当 lambda 表达式引用局部变量或参数时，lambda 表达式捕获引用为闭包的变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3273">When a lambda expression refers to a local variable or parameter, the lambda expression captures the variable being referred to into a closure.</span></span> <span data-ttu-id="ba6f6-3274">闭包是位于堆而不是堆栈上的对象，并且在捕获变量时，对该变量的所有引用都将重定向到闭包。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3274">A closure is an object that lives on the heap instead of on the stack, and when a variable is captured, all references to the variable are redirected to the closure.</span></span> <span data-ttu-id="ba6f6-3275">这样，即使在包含方法完成后，lambda 表达式也能继续引用局部变量和参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3275">This enables lambda expressions to continue to refer to local variables and parameters even after the containing method is complete.</span></span> <span data-ttu-id="ba6f6-3276">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3276">For example:</span></span>

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D
        Dim x As Integer = 10
        Return Function() x
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3277">大致等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3277">is roughly equivalent to:</span></span>

```vb
Module Test
    Delegate Function D() As Integer

    Class $Closure1
        Public x As Integer

        Function $Lambda1() As Integer
            Return x
        End Function
    End Class

    Function M() As D
        Dim c As New $Closure1()
        c.x = 10
        Return AddressOf c.$Lambda1
    End Function

    Sub Main()
        Dim y As D = M()

        ' Prints 10
        Console.WriteLine(y())
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3278">闭包每次进入局部变量时都会捕获本地变量的新副本，但会使用上一个副本（如果有）的值初始化新副本。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3278">A closure captures a new copy of a local variable each time it enters the block in which the local variable is declared, but the new copy is initialized with the value of the previous copy, if any.</span></span> <span data-ttu-id="ba6f6-3279">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3279">For example:</span></span>

```vb
Module Test
    Delegate Function D() As Integer

    Function M() As D()
        Dim a(9) As D

        For i As Integer = 0 To 9
            Dim x
            a(i) = Function() x
            x += 1
        Next i

        Return a
    End Function

    Sub Main()
        Dim y() As D = M()

        For i As Integer = 0 To 9
            Console.Write(y(i)() & " ")
        Next i
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3280">显</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3280">prints</span></span>

```console
1 2 3 4 5 6 7 8 9 10
```

<span data-ttu-id="ba6f6-3281">而不是</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3281">instead of</span></span>

```console
9 9 9 9 9 9 9 9 9 9
```

<span data-ttu-id="ba6f6-3282">因为闭包在输入块时必须进行初始化，因此不允许将它从该块的外部 `GoTo` 到具有闭包的块中，但允许 `Resume` 带闭包的块。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3282">Because closures have to be initialized when entering a block, it is not allowed to `GoTo` into a block with a closure from outside of that block, although it is allowed to `Resume` into a block with a closure.</span></span> <span data-ttu-id="ba6f6-3283">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3283">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim a = 10

        If a = 10 Then
L1:
            Dim x = Function() a

            ' Valid, source is within block
            GoTo L2
L2:
        End If

        ' ERROR: target is inside block with closure
        GoTo L1
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3284">由于无法将它们捕获到闭包中，因此不能在 lambda 表达式中显示以下内容：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3284">Because they cannot be captured into a closure, the following cannot appear inside of a lambda expression:</span></span>

* <span data-ttu-id="ba6f6-3285">引用参数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3285">Reference parameters.</span></span>

* <span data-ttu-id="ba6f6-3286">实例表达式（`Me`、`MyClass``MyBase`）（如果 `Me` 类型不是类）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3286">Instance expressions (`Me`, `MyClass`, `MyBase`), if the type of `Me` is not a class.</span></span>

<span data-ttu-id="ba6f6-3287">如果 lambda 表达式是表达式的一部分，则为匿名类型创建表达式的成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3287">The members of an anonymous type-creation expression, if the lambda expression is part of the expression.</span></span> <span data-ttu-id="ba6f6-3288">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3288">For example:</span></span>

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

<span data-ttu-id="ba6f6-3289">实例构造函数中的 `ReadOnly` 实例变量，或共享构造函数中的 `ReadOnly` 共享变量，这些变量用于非值上下文中。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3289">`ReadOnly` instance variables in instance constructors or `ReadOnly` shared variables in shared constructors where the variables are used in a non-value context.</span></span> <span data-ttu-id="ba6f6-3290">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3290">For example:</span></span>

```vb
Class C1
    ReadOnly F1 As Integer

    Sub New()
        ' Valid, doesn't modify F1
        Dim x = Function() F1

        ' Error, tries to modify F1
        Dim f = Function() ModifyValue(F1)
    End Sub

    Sub ModifyValue(ByRef x As Integer)
    End Sub
End Class
```

## <a name="query-expressions"></a><span data-ttu-id="ba6f6-3291">查询表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3291">Query Expressions</span></span>

<span data-ttu-id="ba6f6-3292">*查询表达式*是将一系列*查询运算符*应用于可*查询*集合的元素的表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3292">A *query expression* is an expression that applies a series of *query operators* to the elements of a *queryable* collection.</span></span> <span data-ttu-id="ba6f6-3293">例如，下面的表达式使用 `Customer` 对象的集合，并返回位于华盛顿州的所有客户的姓名：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3293">For example, the following expression takes a collection of `Customer` objects and returns the names of all the customers in the state of Washington:</span></span>

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

<span data-ttu-id="ba6f6-3294">查询表达式必须以 `From` 或 `Aggregate` 运算符开头，并可以任何查询运算符结尾。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3294">A query expression must start with a `From` or an `Aggregate` operator and can end with any query operator.</span></span> <span data-ttu-id="ba6f6-3295">查询表达式的结果归类为一个值;表达式的结果类型取决于表达式中最后一个查询运算符的结果类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3295">The result of a query expression is classified as a value; the result type of the expression depends on the result type of the last query operator in the expression.</span></span>

```antlr
QueryExpression
    : FromOrAggregateQueryOperator QueryOperator*
    ;

FromOrAggregateQueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    ;

QueryOperator
    : FromQueryOperator
    | AggregateQueryOperator
    | SelectQueryOperator
    | DistinctQueryOperator
    | WhereQueryOperator
    | OrderByQueryOperator
    | PartitionQueryOperator
    | LetQueryOperator
    | GroupByQueryOperator
    | JoinOrGroupJoinQueryOperator
    ;

JoinOrGroupJoinQueryOperator
    : JoinQueryOperator
    | GroupJoinQueryOperator
    ;
```

### <a name="range-variables"></a><span data-ttu-id="ba6f6-3296">范围变量</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3296">Range Variables</span></span>

<span data-ttu-id="ba6f6-3297">某些查询运算符引入称为*范围变量*的特殊类型的变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3297">Some query operators introduce a special kind of variable called a *range variable*.</span></span> <span data-ttu-id="ba6f6-3298">范围变量不是实际变量;相反，它们表示在对输入集合进行查询的评估过程中的各个值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3298">Range variables are not real variables; instead, they represent the individual values during the evaluation of the query over the input collections.</span></span>

```antlr
CollectionRangeVariableDeclarationList
    : CollectionRangeVariableDeclaration ( Comma CollectionRangeVariableDeclaration )*
    ;

CollectionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? 'In' LineTerminator? Expression
    ;

ExpressionRangeVariableDeclarationList
    : ExpressionRangeVariableDeclaration ( Comma ExpressionRangeVariableDeclaration )*
    ;

ExpressionRangeVariableDeclaration
    : Identifier ( 'As' TypeName )? Equals Expression
    ;
```

<span data-ttu-id="ba6f6-3299">范围变量的范围是从引入查询运算符到查询表达式的末尾，或者是对查询运算符（如 `Select` 隐藏它们）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3299">Range variables are scoped from the introducing query operator to the end of a query expression, or to a query operator such as `Select` that hides them.</span></span> <span data-ttu-id="ba6f6-3300">例如，在下面的查询中</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3300">For example, in the following query</span></span>

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

<span data-ttu-id="ba6f6-3301">`From` 查询运算符引入了一个 `cust` 类型为 `Customer` 的范围变量，该变量表示 `Customers` 集合中的每个客户。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3301">the `From` query operator introduces a range variable `cust` typed as `Customer` that represents each customer in the `Customers` collection.</span></span> <span data-ttu-id="ba6f6-3302">下面的 `Where` 查询运算符引用筛选器表达式中的范围变量 `cust`，以确定是否要将单个客户从结果集合中进行筛选。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3302">The following `Where` query operator then refers to the range variable `cust` in the filter expression to determine whether to filter an individual customer out of the resulting collection.</span></span>

<span data-ttu-id="ba6f6-3303">有两种类型的范围变量：*集合范围*变量和*表达式范围变量*。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3303">There are two types of range variables: *collection range variables* and *expression range variables*.</span></span> <span data-ttu-id="ba6f6-3304">集合范围变量从要查询的集合的元素中获取其值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3304">Collection range variables take their values from the elements of the collections being queried.</span></span> <span data-ttu-id="ba6f6-3305">集合范围变量声明中的集合表达式必须分类为其类型可查询的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3305">The collection expression in a collection range variable declaration must be classified as a value whose type is queryable.</span></span> <span data-ttu-id="ba6f6-3306">如果省略集合范围变量的类型，则会将其推断为集合表达式的元素类型，如果集合表达式不具有元素类型（即仅定义 `Cast` 方法），则将其推断 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3306">If the type of a collection range variable is omitted, it is inferred to be the element type of the collection expression, or `Object` if the collection expression does not have an element type (i.e. only defines a `Cast` method).</span></span> <span data-ttu-id="ba6f6-3307">如果集合表达式不可查询（即无法推断集合的元素类型），将产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3307">If the collection expression is not queryable (i.e. the element type of the collection cannot be inferred), a compile-time error results.</span></span>

<span data-ttu-id="ba6f6-3308">表达式范围变量是一个范围变量，其值由表达式而不是集合计算。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3308">An expression range variable is a range variable whose value is calculated by an expression rather than a collection.</span></span> <span data-ttu-id="ba6f6-3309">在下面的示例中，`Select` 查询运算符引入了一个名为 `cityState` 从两个字段计算得出的表达式范围变量：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3309">In the following example, the `Select` query operator introduces an expression range variable named `cityState` calculated from two fields:</span></span>

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

<span data-ttu-id="ba6f6-3310">不需要使用表达式范围变量来引用另一个范围变量，尽管此类变量可能是可疑值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3310">An expression range variable is not required to reference another range variable, although such a variable may be of dubious value.</span></span> <span data-ttu-id="ba6f6-3311">分配给表达式范围变量的表达式必须归类为值，并且必须能够隐式转换为范围变量的类型（如果给定）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3311">The expression assigned to an expression range variable must be classified as a value and must be implicitly convertible to the type of the range variable, if given.</span></span>

<span data-ttu-id="ba6f6-3312">仅在 Let 运算符中可以指定表达式范围变量的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3312">Only in a Let operator may an expression range variable have its type specified.</span></span> <span data-ttu-id="ba6f6-3313">在其他运算符中，或者，如果未指定其类型，则使用局部变量类型推理来确定范围变量的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3313">In other operators, or if its type is not specified, then local variable type inference is used to determine the type of the range variable.</span></span>

<span data-ttu-id="ba6f6-3314">范围变量必须遵循用于声明隐藏的局部变量的规则。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3314">A range variable must follow the rules for declaring local variables in respect to shadowing.</span></span> <span data-ttu-id="ba6f6-3315">因此，范围变量不能在封闭方法或其他范围变量中隐藏局部变量或参数的名称（除非查询运算符具体地隐藏作用域中的所有当前范围变量）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3315">Thus, a range variable cannot hide the name of a local variable or parameter in the enclosing method or another range variable (unless the query operator specifically hides all current range variables in scope).</span></span>


### <a name="queryable-types"></a><span data-ttu-id="ba6f6-3316">可查询类型</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3316">Queryable Types</span></span>

<span data-ttu-id="ba6f6-3317">查询表达式是通过将表达式转换为对集合类型的已知方法的调用来实现的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3317">Query expressions are implemented by translating the expression into calls to well-known methods on a collection type.</span></span> <span data-ttu-id="ba6f6-3318">这些定义完善的方法定义可查询集合的元素类型以及对集合执行的查询运算符的结果类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3318">These well-defined methods define the element type of the queryable collection as well as the result types of query operators executed on the collection.</span></span> <span data-ttu-id="ba6f6-3319">每个查询运算符都指定查询运算符通常转换为的方法，但特定翻译是依赖实现的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3319">Each query operator specifies the method or methods that the query operator is generally translated into, although the specific translation is implementation dependent.</span></span> <span data-ttu-id="ba6f6-3320">这些方法是使用常规格式（如下所示）在规范中指定的：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3320">The methods are given in the specification using a general format that looks like:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="ba6f6-3321">以下各项适用于方法：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3321">The following applies to the methods:</span></span>

* <span data-ttu-id="ba6f6-3322">方法必须是集合类型的实例或扩展成员，并且必须是可访问的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3322">The method must be an instance or extension member of the collection type and must be accessible.</span></span>

* <span data-ttu-id="ba6f6-3323">如果可以推断所有类型参数，则该方法可能是泛型的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3323">The method may be generic, provided that is possible to infer all type arguments.</span></span>

* <span data-ttu-id="ba6f6-3324">此方法可能会重载，在这种情况下，重载决策用于确定要使用的确切方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3324">The method may be overloaded, in which case overload resolution is used to determine the exactly method to use.</span></span>

* <span data-ttu-id="ba6f6-3325">可以使用另一个委托类型来代替委托 `Func` 类型，前提是它具有与匹配 `Func` 类型相同的签名（包括返回类型）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3325">Another delegate type may be used in place of the delegate `Func` type, provided that it has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="ba6f6-3326">类型 `System.Linq.Expressions.Expression(Of D)` 可用于代替委托 `Func` 类型，前提是 `D` 是一个委托类型，该类型具有与匹配的 `Func` 类型相同的签名（包括返回类型）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3326">The type `System.Linq.Expressions.Expression(Of D)` may be used in place of the delegate `Func` type, provided that `D` is a delegate type that has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="ba6f6-3327">类型 `T` 表示输入集合的元素类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3327">The type `T` represents the element type of the input collection.</span></span> <span data-ttu-id="ba6f6-3328">集合类型定义的所有方法都必须具有相同的输入元素类型，以便可以查询该集合类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3328">All of the methods defined by a collection type must have the same input element type for the collection type to be queryable.</span></span>

* <span data-ttu-id="ba6f6-3329">如果执行联接的查询运算符，则类型 `S` 表示第二个输入集合的元素类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3329">The type `S` represents the element type of the second input collection in the case of query operators that perform joins.</span></span>

* <span data-ttu-id="ba6f6-3330">如果查询运算符包含一组充当键的范围变量，则类型 `K` 表示密钥类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3330">The type `K` represents a key type in the case of query operators that have a set of range variables that act as keys.</span></span>

* <span data-ttu-id="ba6f6-3331">类型 `N` 表示用作数值类型的类型（尽管它仍可以是用户定义的类型，而不是内部数值类型）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3331">The type `N` represents a type that is used as a numeric type (although it could still be a user-defined type and not an intrinsic numeric type).</span></span>

* <span data-ttu-id="ba6f6-3332">类型 `B` 表示可在布尔表达式中使用的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3332">The type `B` represents a type that can be used in a Boolean expression.</span></span>

* <span data-ttu-id="ba6f6-3333">如果查询运算符生成结果集合，则类型 `R` 表示结果集合的元素类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3333">The type `R` represents the element type of the result collection, if the query operator produces a result collection.</span></span> <span data-ttu-id="ba6f6-3334">`R` 依赖于查询运算符结束时范围中的范围变量的数目。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3334">`R` depends on the number of range variables in scope at the conclusion of the query operator.</span></span> <span data-ttu-id="ba6f6-3335">如果单个范围变量在范围内，则 `R` 是该范围变量的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3335">If a single range variable is in scope, then `R` is the type of that range variable.</span></span> <span data-ttu-id="ba6f6-3336">示例中</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3336">In the example</span></span>

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  <span data-ttu-id="ba6f6-3337">查询的结果将为元素类型为 `String`的集合类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3337">the result of the query will be a collection type with an element type of `String`.</span></span> <span data-ttu-id="ba6f6-3338">如果多个范围变量位于范围内，则 `R` 为包含范围中所有范围变量（`Key` 字段）的匿名类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3338">If multiple range variables are in scope, then `R` is an anonymous type that contains all of the range variables in scope as `Key` fields.</span></span> <span data-ttu-id="ba6f6-3339">在下面的示例中：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3339">In the example:</span></span>

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  <span data-ttu-id="ba6f6-3340">查询的结果将是一个具有匿名类型的元素类型的集合类型，该类型具有一个名为 `Name` 类型 `String` 的只读属性和一个名为 `String`类型 `ProductName` 的只读属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3340">the result of the query will be a collection type with an element type of an anonymous type with a read-only property named `Name` of type `String` and a read-only property named `ProductName` of type `String`.</span></span>

  <span data-ttu-id="ba6f6-3341">在查询表达式中，生成为包含范围变量的匿名类型是*透明*的，这意味着范围变量始终可用，无需进行限定。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3341">Within a query expression, anonymous types generated to contain range variables are *transparent*, which means that range variables are always available without qualification.</span></span> <span data-ttu-id="ba6f6-3342">例如，在前面的示例中，即使输入集合的元素类型是匿名类型，也可以在 `Select` 查询运算符中访问 `c` 和 `o` 的范围变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3342">For example, in the previous example the range variables `c` and `o` could be accessed without qualification in the `Select` query operator, even though the input collection's element type was an anonymous type.</span></span>

* <span data-ttu-id="ba6f6-3343">类型 `CX` 表示集合类型，而不一定是输入集合类型，其元素类型为 `X`的某种类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3343">The type `CX` represents a collection type, not necessarily the input collection type, whose element type is some type `X`.</span></span>

<span data-ttu-id="ba6f6-3344">可查询的集合类型必须满足以下条件之一（按优先顺序）：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3344">A queryable collection type must satisfy one of the following conditions, in order of preference:</span></span>

* <span data-ttu-id="ba6f6-3345">它必须定义一致的 `Select` 方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3345">It must define a conforming `Select` method.</span></span>

* <span data-ttu-id="ba6f6-3346">它必须具有以下方法之一</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3346">It must have have one of the following methods</span></span>

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  <span data-ttu-id="ba6f6-3347">这可以调用来获取可查询集合。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3347">which can be called to obtain a queryable collection.</span></span> <span data-ttu-id="ba6f6-3348">如果同时提供这两种方法，`AsQueryable` 优先于 `AsEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3348">If both methods are provided, `AsQueryable` is preferred over `AsEnumerable`.</span></span>

* <span data-ttu-id="ba6f6-3349">它必须具有方法</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3349">It must have a method</span></span>

  ```vb
  Function Cast(Of T)() As CT
  ```

  <span data-ttu-id="ba6f6-3350">可以使用范围变量的类型调用，以生成可查询的集合。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3350">which can be called with the type of the range variable to produce a queryable collection.</span></span>

<span data-ttu-id="ba6f6-3351">因为确定集合的元素类型与实际的方法调用无关，所以不能确定特定方法的适用性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3351">Because determining the element type of a collection occurs independently of an actual method invocation, the applicability of specific methods cannot be determined.</span></span> <span data-ttu-id="ba6f6-3352">因此，在确定集合的元素类型时，如果存在与已知方法匹配的实例方法，则将忽略任何与已知方法匹配的扩展方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3352">Thus, when determining the element type of a collection if there are instance methods that match well-known methods, then any extension methods that match well-known methods are ignored.</span></span>

<span data-ttu-id="ba6f6-3353">查询运算符转换按照查询运算符在表达式中出现的顺序进行。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3353">Query operator translation occurs in the order in which the query operators occur in the expression.</span></span> <span data-ttu-id="ba6f6-3354">集合对象不需要实现所有查询运算符所需的所有方法，尽管每个集合对象至少必须支持 `Select` 查询运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3354">It is not necessary for a collection object to implement all of the methods needed by all the query operators, although every collection object must at least support the `Select` query operator.</span></span> <span data-ttu-id="ba6f6-3355">如果所需的方法不存在，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3355">If a needed method is not present, a compile-time error occurs.</span></span> <span data-ttu-id="ba6f6-3356">绑定已知方法名称时，尽管隐藏语义仍适用，但会忽略非方法，以便在接口和扩展方法绑定中进行多重继承。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3356">When binding well-known method names, non-methods are ignored for the purpose of multiple inheritance in interfaces and extension method binding, although shadowing semantics still apply.</span></span> <span data-ttu-id="ba6f6-3357">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3357">For example:</span></span>

```vb
Class Q1
    Public Function [Select](selector As Func(Of Integer, Integer)) As Q1
    End Function
End Class

Class Q2
    Inherits Q1

    Public [Select] As Integer
End Class

Module Test
    Sub Main()
        Dim qs As New Q2()

        ' Error: Q2.Select still hides Q1.Select
        Dim zs = From q In qs Select q
    End Sub
End Module
```

### <a name="default-query-indexer"></a><span data-ttu-id="ba6f6-3358">默认查询索引器</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3358">Default Query Indexer</span></span>

<span data-ttu-id="ba6f6-3359">其元素类型为 `T` 并且还没有默认属性的每个可查询集合类型都被视为具有以下常规形式的默认属性：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3359">Every queryable collection type whose element type is `T` and does not already have a default property is considered to have a default property of the following general form:</span></span>

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

<span data-ttu-id="ba6f6-3360">默认属性只能用默认属性访问语法来引用;默认属性不能按名称引用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3360">The default property can only be referred to using the default property access syntax; the default property cannot be referred to by name.</span></span> <span data-ttu-id="ba6f6-3361">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3361">For example:</span></span>

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

<span data-ttu-id="ba6f6-3362">如果集合类型没有 `ElementAtOrDefault` 成员，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3362">If the collection type does not have an `ElementAtOrDefault` member, a compile-time error will occur.</span></span>

### <a name="from-query-operator"></a><span data-ttu-id="ba6f6-3363">From 查询运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3363">From Query Operator</span></span>

<span data-ttu-id="ba6f6-3364">`From` 查询运算符引入一个集合范围变量，该变量表示要查询的集合的各个成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3364">The `From` query operator introduces a collection range variable that represents the individual members of a collection to be queried.</span></span>

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ba6f6-3365">例如，查询表达式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3365">For example, the query expression:</span></span>

```vb
From c As Customer In Customers ...
```

<span data-ttu-id="ba6f6-3366">可以视为等效于</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3366">can be thought of as equivalent to</span></span>

```vb
For Each c As Customer In Customers
        ...
Next c
```

<span data-ttu-id="ba6f6-3367">如果 `From` 查询运算符声明多个集合范围变量，或者不是查询表达式中的第一个 `From` 查询运算符，则每个新的集合范围变量将*交叉联接*到现有的范围变量集。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3367">When a `From` query operator declares multiple collection range variables or is not the first `From` query operator in the query expression, each new collection range variable is *cross joined* to the existing set of range variables.</span></span> <span data-ttu-id="ba6f6-3368">结果就是对联接集合中所有元素的叉积计算查询。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3368">The result is that the query is evaluated over the cross-product of all the elements in the joined collections.</span></span> <span data-ttu-id="ba6f6-3369">例如，表达式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3369">For example, the expression:</span></span>

```vb
From c In Customers _
From e In Employees _
...
```

<span data-ttu-id="ba6f6-3370">可以视为等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3370">can be thought of as equivalent to:</span></span>

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

<span data-ttu-id="ba6f6-3371">和完全等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3371">and is exactly equivalent to:</span></span>

```vb
From c In Customers, e In Employees ...
```

<span data-ttu-id="ba6f6-3372">可在后面的 `From` 查询运算符中使用前面的查询运算符中引入的范围变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3372">The range variables introduced in previous query operators can be used in a later `From` query operator.</span></span> <span data-ttu-id="ba6f6-3373">例如，在下面的查询表达式中，第二个 `From` 查询运算符引用第一个范围变量的值：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3373">For example, in the following query expression the second `From` query operator refers to the value of the first range variable:</span></span>

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

<span data-ttu-id="ba6f6-3374">仅当集合类型包含以下一个或两个方法时，才支持 `From` 查询运算符或多个 `From` 查询运算符中的多个范围变量：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3374">Multiple range variables in a `From` query operator or multiple `From` query operators are only supported if the collection type contains one or both of the following methods:</span></span>

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="ba6f6-3375">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3375">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

<span data-ttu-id="ba6f6-3376">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3376">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="ba6f6-3377">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3377">__Note.__</span></span> <span data-ttu-id="ba6f6-3378">`From` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3378">`From` is not a reserved word.</span></span>


### <a name="join-query-operator"></a><span data-ttu-id="ba6f6-3379">Join 查询运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3379">Join Query Operator</span></span>

<span data-ttu-id="ba6f6-3380">`Join` 查询运算符使用新的集合范围变量联接现有范围变量，并生成一个集合，该集合的元素基于相等表达式联接在一起。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3380">The `Join` query operator joins existing range variables with a new collection range variable, producing a single collection whose elements have been joined together based on an equality expression.</span></span>

```antlr
JoinQueryOperator
    : LineTerminator? 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
    ;

JoinConditionList
    : JoinCondition ( 'And' LineTerminator? JoinCondition )*
    ;

JoinCondition
    : Expression 'Equals' LineTerminator? Expression
    ;
```

<span data-ttu-id="ba6f6-3381">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3381">For example:</span></span>

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

<span data-ttu-id="ba6f6-3382">相等表达式比正则相等表达式的限制更多：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3382">The equality expression is more restricted than a regular equality expression:</span></span>

* <span data-ttu-id="ba6f6-3383">这两个表达式都必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3383">Both expressions must be classified as a value.</span></span>

* <span data-ttu-id="ba6f6-3384">两个表达式必须引用至少一个范围变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3384">Both expressions must reference at least one range variable.</span></span>

* <span data-ttu-id="ba6f6-3385">联接查询运算符中声明的范围变量必须由其中一个表达式引用，并且该表达式不得引用任何其他范围变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3385">The range variable declared in the join query operator must be referenced by one of the expressions, and that expression must not reference any other range variables.</span></span>

<span data-ttu-id="ba6f6-3386">如果两个表达式的类型不是完全相同的类型，则</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3386">If the types of the two expressions are not the exact same type, then</span></span>

* <span data-ttu-id="ba6f6-3387">如果为这两个类型定义了相等运算符，两个表达式都可以隐式转换为它，并且不 `Object`，然后将两个表达式都转换为该类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3387">If the equality operator is defined for the two types, both expressions are implicitly convertible to it, and it is not `Object`, then convert both expressions to that type.</span></span>

* <span data-ttu-id="ba6f6-3388">否则，如果存在可隐式转换为两个表达式的主导类型，请将这两个表达式都转换为该类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3388">Otherwise, if there is a dominant type that both expressions can be implicitly converted to, then convert both expressions to that type.</span></span>

* <span data-ttu-id="ba6f6-3389">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3389">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ba6f6-3390">使用哈希值（即通过调用 `GetHashCode()`）而不是使用相等运算符来比较表达式以提高效率。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3390">The expressions are compared using hash values (i.e. by calling `GetHashCode()`) rather than by using equality operators for efficiency.</span></span> <span data-ttu-id="ba6f6-3391">`Join` 查询运算符可在同一运算符中执行多个联接或相等性条件。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3391">A `Join` query operator may do multiple joins or equality conditions in the same operator.</span></span> <span data-ttu-id="ba6f6-3392">仅当集合类型包含方法时，才支持 `Join` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3392">A `Join` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="ba6f6-3393">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3393">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

<span data-ttu-id="ba6f6-3394">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3394">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.Join( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="ba6f6-3395">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3395">__Note.__</span></span> <span data-ttu-id="ba6f6-3396">`Join`，`On` 和 `Equals` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3396">`Join`, `On` and `Equals` are not reserved words.</span></span>


### <a name="let-query-operator"></a><span data-ttu-id="ba6f6-3397">Let Query 运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3397">Let Query Operator</span></span>

<span data-ttu-id="ba6f6-3398">`Let` 查询运算符引入了一个表达式范围变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3398">The `Let` query operator introduces an expression range variable.</span></span> <span data-ttu-id="ba6f6-3399">这允许计算一次中间值，该值将在以后的查询运算符中多次使用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3399">This allows calculating an intermediate value once that will be used multiple times in later query operators.</span></span>

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ba6f6-3400">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3400">For example:</span></span>

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

<span data-ttu-id="ba6f6-3401">可以视为等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3401">can be thought of as equivalent to:</span></span>

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

<span data-ttu-id="ba6f6-3402">仅当集合类型包含方法时，才支持 `Let` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3402">A `Let` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="ba6f6-3403">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3403">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

<span data-ttu-id="ba6f6-3404">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3404">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a><span data-ttu-id="ba6f6-3405">选择查询运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3405">Select Query Operator</span></span>

<span data-ttu-id="ba6f6-3406">`Select` 查询运算符与 `Let` query 运算符类似，因为它引入了表达式范围变量;但是，`Select` 查询运算符将隐藏当前可用的范围变量，而不是将其添加到其中。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3406">The `Select` query operator is like the `Let` query operator in that it introduces expression range variables; however, a `Select` query operator hides the currently available range variables instead of adding to them.</span></span> <span data-ttu-id="ba6f6-3407">此外，始终使用局部变量类型推理规则推断 `Select` 查询运算符引入的表达式范围变量的类型;无法指定显式类型，如果不能推断任何类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3407">Also, the type of an expression range variable introduced by a `Select` query operator is always inferred using local variable type inference rules; an explicit type cannot be specified, and if no type can be inferred, a compile-time error occurs.</span></span>

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ba6f6-3408">例如，在查询中：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3408">For example, in the query:</span></span>

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

<span data-ttu-id="ba6f6-3409">`Where` 查询运算符仅有权访问 `Select` 运算符引入的 `name` 范围变量;如果 `Where` 运算符尝试引用 `cust`，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3409">the `Where` query operator only has access to the `name` range variable introduced by the `Select` operator; if the `Where` operator had tried to reference `cust`, a compile-time error would have occurred.</span></span>

<span data-ttu-id="ba6f6-3410">`Select` 查询运算符不是显式指定范围变量的名称，而是使用与匿名类型对象创建表达式相同的规则来推导范围变量的名称。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3410">Instead of explicitly specifying the names of the range variables, a `Select` query operator can infer the names of the range variables, using the same rules as anonymous type object creation expressions.</span></span> <span data-ttu-id="ba6f6-3411">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3411">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

<span data-ttu-id="ba6f6-3412">如果未提供范围变量的名称并且无法推断名称，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3412">If the name of the range variable is not supplied and a name cannot be inferred, a compile-time error occurs.</span></span> <span data-ttu-id="ba6f6-3413">如果 `Select` 查询运算符只包含一个表达式，则不会发生错误，因为不能推断该范围变量的名称，但该范围变量是无名称的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3413">If the `Select` query operator contains only a single expression, no error occurs if a name for that range variable cannot be inferred but the range variable is nameless.</span></span>  <span data-ttu-id="ba6f6-3414">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3414">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

<span data-ttu-id="ba6f6-3415">如果在将名称分配给范围变量和相等表达式之间 `Select` 查询运算符存在歧义，则首选名称赋值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3415">If there is an ambiguity in a `Select` query operator between assigning a name to a range variable and an equality expression, the name assignment is preferred.</span></span> <span data-ttu-id="ba6f6-3416">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3416">For example:</span></span>

```vb
Dim badCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select name = c.Name ' Creates a range variable named "name"


Dim goodCustNames = _
      From c In Customers _
        Let name = "John Smith" _
      Select match = (name = c.Name)
```

<span data-ttu-id="ba6f6-3417">`Select` 查询运算符中的每个表达式都必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3417">Each expression in the `Select` query operator must be classified as a value.</span></span> <span data-ttu-id="ba6f6-3418">仅当集合类型包含方法时，才支持 `Select` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3418">A `Select` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="ba6f6-3419">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3419">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

<span data-ttu-id="ba6f6-3420">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3420">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a><span data-ttu-id="ba6f6-3421">Distinct 查询运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3421">Distinct Query Operator</span></span>

<span data-ttu-id="ba6f6-3422">`Distinct` 查询运算符仅将集合中的值限制为具有不同值的值，这是通过比较元素类型是否相等来确定的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3422">The `Distinct` query operator restricts the values in a collection only to those with distinct values, as determined by comparing the element type for equality.</span></span>

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

<span data-ttu-id="ba6f6-3423">例如，查询：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3423">For example, the query:</span></span>

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

<span data-ttu-id="ba6f6-3424">对于客户名称和订单价格的每个不同配对，将只返回一行，即使客户具有具有相同价格的多个订单也是如此。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3424">will only return one row for each distinct pairing of customer name and order price, even if the customer has multiple orders with the same price.</span></span> <span data-ttu-id="ba6f6-3425">仅当集合类型包含方法时，才支持 `Distinct` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3425">A `Distinct` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Distinct() As CT
```

<span data-ttu-id="ba6f6-3426">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3426">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

<span data-ttu-id="ba6f6-3427">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3427">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

<span data-ttu-id="ba6f6-3428">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3428">__Note.__</span></span> <span data-ttu-id="ba6f6-3429">`Distinct` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3429">`Distinct` is not a reserved word.</span></span>


### <a name="where-query-operator"></a><span data-ttu-id="ba6f6-3430">Where 查询运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3430">Where Query Operator</span></span>

<span data-ttu-id="ba6f6-3431">`Where` 查询运算符将集合中的值限制为满足给定条件的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3431">The `Where` query operator restricts the values in a collection to those that satisfy a given condition.</span></span>

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="ba6f6-3432">`Where` 查询运算符采用为每组范围变量值计算的布尔表达式;如果表达式的值为 true，则值将显示在输出集合中，否则将跳过这些值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3432">A `Where` query operator takes a Boolean expression that is evaluated for each set of range variable values; if the value of the expression is true, then the values appear in the output collection, otherwise the values are skipped.</span></span> <span data-ttu-id="ba6f6-3433">例如，查询表达式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3433">For example, the query expression:</span></span>

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

<span data-ttu-id="ba6f6-3434">可以视为等效于嵌套循环</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3434">can be thought of as equivalent to the nested loop</span></span>

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

<span data-ttu-id="ba6f6-3435">仅当集合类型包含方法时，才支持 `Where` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3435">A `Where` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="ba6f6-3436">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3436">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

<span data-ttu-id="ba6f6-3437">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3437">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

<span data-ttu-id="ba6f6-3438">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3438">__Note.__</span></span> <span data-ttu-id="ba6f6-3439">`Where` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3439">`Where` is not a reserved word.</span></span>


### <a name="partition-query-operators"></a><span data-ttu-id="ba6f6-3440">分区查询运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3440">Partition Query Operators</span></span>

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="ba6f6-3441">`Take` 查询运算符将生成集合的第一个 `n` 元素。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3441">The `Take` query operator results in the first `n` elements of a collection.</span></span> <span data-ttu-id="ba6f6-3442">与 `While` 修饰符一起使用时，`Take` 运算符会生成满足布尔表达式的集合中的第一个 `n` 元素。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3442">When used with the `While` modifier, the `Take` operator results in the first `n` elements of a collection that satisfy a Boolean expression.</span></span> <span data-ttu-id="ba6f6-3443">`Skip` 运算符将跳过集合中的第一个 `n` 元素，然后返回该集合的其余部分。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3443">The `Skip` operator skips the first `n` elements of a collection and then returns the remainder of the collection.</span></span>  <span data-ttu-id="ba6f6-3444">当与 `While` 修饰符结合使用时，`Skip` 运算符将跳过满足布尔表达式的集合中的第一个 `n` 元素，然后返回该集合的其余部分。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3444">When used in conjunction with the `While` modifier, the `Skip` operator skips the first `n` elements of a collection that satisfy a Boolean expression and then returns the rest of the collection.</span></span> <span data-ttu-id="ba6f6-3445">`Take` 或 `Skip` 查询运算符中的表达式必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3445">The expressions in a `Take` or `Skip` query operator must be classified as a value.</span></span>

<span data-ttu-id="ba6f6-3446">仅当集合类型包含方法时，才支持 `Take` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3446">A `Take` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Take(count As N) As CT
```

<span data-ttu-id="ba6f6-3447">仅当集合类型包含方法时，才支持 `Skip` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3447">A `Skip` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Skip(count As N) As CT
```

<span data-ttu-id="ba6f6-3448">仅当集合类型包含方法时，才支持 `Take While` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3448">A `Take While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="ba6f6-3449">仅当集合类型包含方法时，才支持 `Skip While` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3449">A `Skip While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="ba6f6-3450">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3450">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

<span data-ttu-id="ba6f6-3451">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3451">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

<span data-ttu-id="ba6f6-3452">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3452">__Note.__</span></span> <span data-ttu-id="ba6f6-3453">`Take` 和 `Skip` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3453">`Take` and `Skip` are not reserved words.</span></span>


### <a name="order-by-query-operator"></a><span data-ttu-id="ba6f6-3454">Order By 查询运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3454">Order By Query Operator</span></span>

<span data-ttu-id="ba6f6-3455">`Order By` query 运算符对出现在范围变量中的值进行排序。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3455">The `Order By` query operator orders the values that appear in the range variables.</span></span> 

```antlr
OrderByQueryOperator
    : LineTerminator? 'Order' 'By' LineTerminator? OrderExpressionList
    ;

OrderExpressionList
    : OrderExpression ( Comma OrderExpression )*
    ;

OrderExpression
    : Expression Ordering?
    ;

Ordering
    : 'Ascending' | 'Descending'
    ;
```

<span data-ttu-id="ba6f6-3456">`Order By` 查询运算符使用指定应用于对迭代变量进行排序的键值的表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3456">An `Order By` query operator takes expressions that specify the key values that should be used to order the iteration variables.</span></span> <span data-ttu-id="ba6f6-3457">例如，以下查询将返回按价格排序的产品：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3457">For example, the following query returns products sorted by price:</span></span>

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

<span data-ttu-id="ba6f6-3458">排序可以标记为 "`Ascending`"，在这种情况下，较小的值可能在较大值之前，或者 `Descending`，在这种情况下，较大值早于较小值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3458">An ordering can be marked as `Ascending`, in which case smaller values come before larger values, or `Descending`, in which case larger values come before smaller values.</span></span> <span data-ttu-id="ba6f6-3459">如果指定了 "无"，则默认为排序 `Ascending`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3459">The default for an ordering if none is specified is `Ascending`.</span></span> <span data-ttu-id="ba6f6-3460">例如，下面的查询返回按价格排序的产品，其最昂贵的产品首先：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3460">For example, the following query returns products sorted by price with the most expensive product first:</span></span>

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

<span data-ttu-id="ba6f6-3461">`Order By` 查询运算符可指定多个表达式进行排序，在这种情况下，将以嵌套方式对集合进行排序。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3461">The `Order By` query operator may specify multiple expressions for ordering, in which case the collection is ordered in a nested manner.</span></span> <span data-ttu-id="ba6f6-3462">例如，以下查询按州对客户进行排序，然后按每个州的城市，然后按每个城市中的邮政编码排序：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3462">For example, the following query orders customers by state, then by city within each state and then by ZIP code within each city:</span></span>

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

<span data-ttu-id="ba6f6-3463">`Order By` 查询运算符中的表达式必须分类为值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3463">The expressions in an `Order By` query operator must be classified as a value.</span></span> <span data-ttu-id="ba6f6-3464">仅当集合类型包含以下一个或两个方法时，才支持 `Order By` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3464">An `Order By` query operator is supported only if the collection type contains one or both of the following methods:</span></span>

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="ba6f6-3465">`CT` 的返回类型必须是*有序集合*。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3465">The return type `CT` must be an *ordered collection*.</span></span> <span data-ttu-id="ba6f6-3466">有序集合是包含一个或两个方法的集合类型：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3466">An ordered collection is a collection type that contains one or both of the methods:</span></span>

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="ba6f6-3467">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3467">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

<span data-ttu-id="ba6f6-3468">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3468">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

<span data-ttu-id="ba6f6-3469">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3469">__Note.__</span></span> <span data-ttu-id="ba6f6-3470">由于查询运算符只是将语法映射到实现特定查询操作的方法，因此，排序不由语言决定，并且由运算符本身的实现确定。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3470">Because query operators simply map syntax to methods that implement a particular query operation, order preservation is not dictated by the language and is determined by the implementation of the operator itself.</span></span> <span data-ttu-id="ba6f6-3471">这与用户定义的运算符非常相似，因为实现为用户定义的数值类型重载加法运算符可能不会执行任何类似于添加的操作。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3471">This is very similar to user-defined operators in that the implementation to overload the addition operator for a user-defined numeric type may not perform anything resembling an addition.</span></span> <span data-ttu-id="ba6f6-3472">当然，为保持可预测性，不建议实现不符合用户期望的内容。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3472">Of course, to preserve predictability, implementing something that does not match user expectations is not recommended.</span></span>

<span data-ttu-id="ba6f6-3473">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3473">__Note.__</span></span> <span data-ttu-id="ba6f6-3474">`Order` 和 `By` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3474">`Order` and `By` are not reserved words.</span></span>


### <a name="group-by-query-operator"></a><span data-ttu-id="ba6f6-3475">按查询运算符分组</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3475">Group By Query Operator</span></span>

<span data-ttu-id="ba6f6-3476">`Group By` 查询运算符根据一个或多个表达式对作用域中的范围变量进行分组，然后基于这些分组生成新的范围变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3476">The `Group By` query operator groups the range variables in scope based on one or more expressions, and then produces new range variables based on those groupings.</span></span>

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ba6f6-3477">例如，以下查询将 `State`的所有客户分组，然后计算每个组的计数和平均期限：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3477">For example, the following query groups all customers by `State`, and then computes the count and average age of each group:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

<span data-ttu-id="ba6f6-3478">`Group By` 查询运算符包含三个子句：可选的 `Group` 子句、`By` 子句和 `Into` 子句。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3478">The `Group By` query operator has three clauses: the optional `Group` clause, the `By` clause, and the `Into` clause.</span></span> <span data-ttu-id="ba6f6-3479">`Group` 子句与 `Select` 查询运算符具有相同的语法和效果，不同之处在于，它仅影响 `Into` 子句中可用的范围变量，而不影响 `By` 子句中的范围变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3479">The `Group` clause has the same syntax and effect as a `Select` query operator, except that it only affects the range variables available in the `Into` clause and not the `By` clause.</span></span> <span data-ttu-id="ba6f6-3480">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3480">For example:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

<span data-ttu-id="ba6f6-3481">`By` 子句声明用作分组操作中的键值的表达式范围变量。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3481">The `By` clause declares expression range variables that are used as key values in the grouping operation.</span></span> <span data-ttu-id="ba6f6-3482">`Into` 子句允许声明表达式范围变量，这些变量计算由 `By` 子句构成的每个组的聚合。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3482">The `Into` clause allows the declaration of expression range variables that calculate aggregations over each of the groups formed by the `By` clause.</span></span> <span data-ttu-id="ba6f6-3483">在 `Into` 子句中，只能为表达式范围变量指定一个表达式，该表达式是*聚合函数*的方法调用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3483">Within the `Into` clause, the expression range variable can only be assigned an expression which is a method invocation of an *aggregate function*.</span></span> <span data-ttu-id="ba6f6-3484">聚合函数是组的集合类型的函数（可能不一定是原始集合的相同集合类型），其外观类似于以下方法之一：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3484">An aggregate function is a function on the collection type of the group (which may not necessarily be the same collection type of the original collection) which looks like either of the following methods:</span></span>

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

<span data-ttu-id="ba6f6-3485">如果聚合函数采用委托参数，则调用表达式可以具有必须归类为值的参数表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3485">If an aggregate function takes a delegate argument, then the invocation expression can have an argument expression that must be classified as a value.</span></span>  <span data-ttu-id="ba6f6-3486">参数表达式可以使用范围内的范围变量;在对聚合函数的调用中，这些范围变量表示组中组成的值，而不是集合中的所有值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3486">The argument expression can use the range variables that are in scope; within the call to an aggregate function, those range variables represent the values in the group being formed, not all of the values in the collection.</span></span> <span data-ttu-id="ba6f6-3487">例如，在本部分的原始示例中，`Average` 函数将计算客户每个州的年龄（而不是所有客户）的平均值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3487">For example, in the original example in this section the `Average` function calculates the average of the customers' ages per state rather than for all of the customers together.</span></span>

<span data-ttu-id="ba6f6-3488">所有集合类型都被视为 `Group` 上定义聚合函数，这不采用任何参数，只是返回组。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3488">All collection types are considered to have the aggregate function `Group` defined on it, which takes no parameters and simply returns the group.</span></span> <span data-ttu-id="ba6f6-3489">集合类型可以提供的其他标准聚合函数包括：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3489">Other standard aggregate functions that a collection type may provide are:</span></span>

<span data-ttu-id="ba6f6-3490">`Count` 和 `LongCount`，它将返回组中的元素计数或满足布尔表达式的组中元素的计数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3490">`Count` and `LongCount`, which return the count of the elements in the group or the count of the elements in the group that satisfy a Boolean expression.</span></span> <span data-ttu-id="ba6f6-3491">仅当集合类型包含其中一种方法时，才支持 `Count` 和 `LongCount`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3491">`Count` and `LongCount` are supported only if the collection type contains one of the methods:</span></span>

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

<span data-ttu-id="ba6f6-3492">`Sum`，它返回组中所有元素的表达式之和。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3492">`Sum`, which returns the sum of an expression across all the elements in the group.</span></span> <span data-ttu-id="ba6f6-3493">仅当集合类型包含其中一种方法时，才支持 `Sum`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3493">`Sum` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

<span data-ttu-id="ba6f6-3494">`Min`，它跨组中的所有元素返回表达式的最小值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3494">`Min` which returns the minimum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="ba6f6-3495">仅当集合类型包含其中一种方法时，才支持 `Min`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3495">`Min` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

<span data-ttu-id="ba6f6-3496">`Max`，它将返回组中所有元素的表达式的最大值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3496">`Max`, which returns the maximum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="ba6f6-3497">仅当集合类型包含其中一种方法时，才支持 `Max`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3497">`Max` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

<span data-ttu-id="ba6f6-3498">`Average`，它返回组中所有元素的表达式平均值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3498">`Average`, which returns the average of an expression across all the elements in the group.</span></span> <span data-ttu-id="ba6f6-3499">仅当集合类型包含其中一种方法时，才支持 `Average`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3499">`Average` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

<span data-ttu-id="ba6f6-3500">`Any`，它确定组是否包含成员，或者是否在组中的任何元素上使用布尔表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3500">`Any`, which determines whether a group contains members or if a Boolean expression is true for any element in the group.</span></span> <span data-ttu-id="ba6f6-3501">`Any` 返回一个值，该值可在布尔表达式中使用，并且仅当集合类型包含其中一种方法时才受支持：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3501">`Any` returns a value that can be used in a Boolean expression and is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="ba6f6-3502">`All`，它确定对于组中的所有元素，布尔表达式是否为 true。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3502">`All`, which determines whether a Boolean expression is true for all elements in the group.</span></span> <span data-ttu-id="ba6f6-3503">`All` 返回一个值，该值可在布尔表达式中使用，并且仅当集合类型包含方法时才受支持：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3503">`All` returns a value that can be used in a Boolean expression and is supported only if the collection type contains a method:</span></span>

```vb
Function All(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="ba6f6-3504">在 `Group By` 查询运算符之后，范围中以前的范围变量将隐藏，并且 `By` 和 `Into` 子句引入的范围变量可用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3504">After a `Group By` query operator, the range variables previously in scope are hidden, and the range variables introduced by the `By` and `Into` clauses are available.</span></span> <span data-ttu-id="ba6f6-3505">仅当集合类型包含方法时，才支持 `Group By` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3505">A `Group By` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

<span data-ttu-id="ba6f6-3506">仅当集合类型包含方法时，才支持 `Group` 子句中的范围变量声明：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3506">Range variable declarations in the `Group` clause are supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

<span data-ttu-id="ba6f6-3507">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3507">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

<span data-ttu-id="ba6f6-3508">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3508">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.GroupBy( _
        Function(x As Integer) x Mod 2, _
        Function(x As Integer) New With {.y = x * 10, .z = x / 10}, _
        Function(evenOdd, group) New With { _
            evenOdd, _
            .Sum = group.Sum(Function(e) e.y), _
            .Average = group.Average(Function(e) e.z)})...
```

<span data-ttu-id="ba6f6-3509">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3509">__Note.__</span></span> <span data-ttu-id="ba6f6-3510">`Group`、`By`和 `Into` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3510">`Group`, `By`, and `Into` are not reserved words.</span></span>


### <a name="aggregate-query-operator"></a><span data-ttu-id="ba6f6-3511">聚合查询运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3511">Aggregate Query Operator</span></span>

<span data-ttu-id="ba6f6-3512">`Aggregate` 查询运算符执行与 `Group By` 运算符类似的函数，但它允许对已形成的组进行聚合。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3512">The `Aggregate` query operator performs a similar function as the `Group By` operator, except it allows aggregating over groups that have already been formed.</span></span> <span data-ttu-id="ba6f6-3513">由于已形成了组，因此 `Aggregate` 查询运算符的 `Into` 子句不会隐藏范围内的范围变量（通过这种方式，`Aggregate` 更像是 `Let`，`Group By` 更像是 `Select`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3513">Because the group has already been formed, the `Into` clause of an `Aggregate` query operator does not hide the range variables in scope (in this way, `Aggregate` is more like a `Let`, and `Group By` is more like a `Select`).</span></span>

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ba6f6-3514">例如，以下查询聚合了位于华盛顿的客户所下的所有订单总计：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3514">For example, the following query aggregates the total of all the orders placed by customers in Washington:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

<span data-ttu-id="ba6f6-3515">此查询的结果是一个集合，该集合的元素类型是一个匿名类型，其元素类型为类型为 `cust` 类型为 `Customer` 的属性，以及一个名为 `Sum` 类型为 `Integer`的名为的属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3515">The result of this query is a collection whose element type is an anonymous type with a property named `cust` typed as `Customer` and a property named `Sum` typed as `Integer`.</span></span>

<span data-ttu-id="ba6f6-3516">与 `Group By`不同，可以在 `Aggregate` 和 `Into` 子句之间放置其他查询运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3516">Unlike `Group By`, additional query operators can be placed between the `Aggregate` and `Into` clauses.</span></span> <span data-ttu-id="ba6f6-3517">在 `Aggregate` 子句和 `Into` 子句结束之间，范围内的所有范围变量（包括 `Aggregate` 子句声明的变量）都可以使用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3517">Between an `Aggregate` clause and the end of the `Into` clause, all range variables in scope, including those declared by the `Aggregate` clause can be used.</span></span> <span data-ttu-id="ba6f6-3518">例如，下面的查询在2006之前聚合了华盛顿州的客户所下的所有订单的总和总计：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3518">For example, the following query aggregates the sum total of all the orders placed by customers in Washington before 2006:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

<span data-ttu-id="ba6f6-3519">`Aggregate` 运算符还可用于启动查询表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3519">The `Aggregate` operator can also be used to start a query expression.</span></span> <span data-ttu-id="ba6f6-3520">在这种情况下，查询表达式的结果将是 `Into` 子句计算得出的单个值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3520">In this case, the result of the query expression will be the single value computed by the `Into` clause.</span></span> <span data-ttu-id="ba6f6-3521">例如，下面的查询计算在2006年1月1日之前所有订单总计的总和：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3521">For example, the following query calculates the sum of all the order totals before January 1st, 2006:</span></span>

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

<span data-ttu-id="ba6f6-3522">查询的结果是单个 `Integer` 值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3522">The result of the query is a single `Integer` value.</span></span> <span data-ttu-id="ba6f6-3523">`Aggregate` 查询运算符始终可用（尽管聚合函数还必须可供表达式有效）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3523">An `Aggregate` query operator is always available (although the aggregate function must be also be available for the expression to be valid).</span></span> <span data-ttu-id="ba6f6-3524">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3524">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

<span data-ttu-id="ba6f6-3525">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3525">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

<span data-ttu-id="ba6f6-3526">__注意。__  `Aggregate`</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3526">__Note.__ `Aggregate`</span></span> <span data-ttu-id="ba6f6-3527">和 `Into` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3527">and `Into` are not reserved words.</span></span>


### <a name="group-join-query-operator"></a><span data-ttu-id="ba6f6-3528">分组联接查询运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3528">Group Join Query Operator</span></span>

<span data-ttu-id="ba6f6-3529">`Group Join` 查询运算符将 `Join` 的函数和 `Group By` 查询运算符组合成单个运算符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3529">The `Group Join` query operator combines the functions of the `Join` and `Group By` query operators into a single operator.</span></span> <span data-ttu-id="ba6f6-3530">`Group Join` 联接两个集合，这些集合基于从元素中提取的匹配键，将联接右侧与联接左侧的特定元素相匹配的所有元素组合在一起。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3530">`Group Join` joins two collections based on matching keys extracted from the elements, grouping together all of the elements on the right side of the join that match a particular element on the left side of the join.</span></span> <span data-ttu-id="ba6f6-3531">这样，运算符将生成一组分层结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3531">Thus, the operator produces a set of hierarchical results.</span></span>

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="ba6f6-3532">例如，下面的查询生成元素，这些元素包含单个客户的名称、其所有订单的一组和所有订单的总金额：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3532">For example, the following query produces elements that contain a single customer's name, a group of all of their orders, and the total amount of all of those orders:</span></span>

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

<span data-ttu-id="ba6f6-3533">查询的结果是一个集合，其元素类型为具有三个属性的匿名类型： `Name`、类型化为 `String`、`Orders` 类型化为元素类型为 `Order`的集合和 `OrdersTotal`类型为 `Integer`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3533">The result of the query is a collection whose element type is an anonymous type with three properties: `Name`, typed as `String`, `Orders` typed as a collection whose element type is `Order`, and `OrdersTotal`, typed as `Integer`.</span></span> <span data-ttu-id="ba6f6-3534">仅当集合类型包含方法时，才支持 `Group Join` 查询运算符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3534">A `Group Join` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

<span data-ttu-id="ba6f6-3535">代码</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3535">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

<span data-ttu-id="ba6f6-3536">通常转换为</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3536">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.GroupJoin( _
        ys, _
        Function(x As Integer) x, _
        Function(y As Integer) y, _
        Function(x, group) New With {x, .g = group})...
```

<span data-ttu-id="ba6f6-3537">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3537">__Note.__</span></span> <span data-ttu-id="ba6f6-3538">`Group`、`Join`和 `Into` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3538">`Group`, `Join`, and `Into` are not reserved words.</span></span>


## <a name="conditional-expressions"></a><span data-ttu-id="ba6f6-3539">条件表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3539">Conditional Expressions</span></span>

<span data-ttu-id="ba6f6-3540">条件 `If` 表达式可测试表达式并返回值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3540">A conditional `If` expression tests an expression and returns a value.</span></span>

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

<span data-ttu-id="ba6f6-3541">但与 `IIF` 运行时函数不同，条件表达式仅在必要时才计算其操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3541">Unlike the `IIF` runtime function, however, a conditional expression only evaluates its operands if necessary.</span></span> <span data-ttu-id="ba6f6-3542">例如，如果 `c` 的值 `Nothing`，则表达式 `If(c Is Nothing, c.Name, "Unknown")` 将不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3542">Thus, for example, the expression `If(c Is Nothing, c.Name, "Unknown")` will not throw an exception if the value of `c` is `Nothing`.</span></span> <span data-ttu-id="ba6f6-3543">条件表达式有两种形式：一个使用两个操作数，另一个采用三个操作数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3543">The conditional expression has two forms: one that takes two operands and one that takes three operands.</span></span>

<span data-ttu-id="ba6f6-3544">如果提供了三个操作数，则所有三个表达式都必须分类为值，第一个操作数必须为布尔表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3544">If three operands are provided, all three expressions must be classified as values, and the first operand must be a Boolean expression.</span></span> <span data-ttu-id="ba6f6-3545">如果表达式的结果为 true，则第二个表达式将是运算符的结果，否则第三个表达式将是运算符的结果。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3545">If the result is of the expression is true, then the second expression will be the result of the operator, otherwise the third expression will be the result of the operator.</span></span> <span data-ttu-id="ba6f6-3546">表达式的结果类型是第二个和第三个表达式的类型之间的基准类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3546">The result type of the expression is the dominant type between the types of the second and third expression.</span></span> <span data-ttu-id="ba6f6-3547">如果没有主导类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3547">If there is no dominant type, then a compile-time error occurs.</span></span>

<span data-ttu-id="ba6f6-3548">如果提供了两个操作数，则两个操作数都必须分类为值，第一个操作数必须是引用类型或可以为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3548">If two operands are provided, both of the operands must be classified as values, and the first operand must be either a reference type or a nullable value type.</span></span> <span data-ttu-id="ba6f6-3549">然后计算表达式 `If(x, y)`，就像 `If(x IsNot Nothing, x, y)`表达式一样，但有两个例外。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3549">The expression `If(x, y)` is then evaluated as if was the expression `If(x IsNot Nothing, x, y)`, with two exceptions.</span></span> <span data-ttu-id="ba6f6-3550">首先，第一个表达式只计算一次，第二种情况是，如果第二个操作数的类型是不可为 null 的值类型，第一个操作数的类型为，则在确定该表达式的结果类型的主导类型时，将从第一个操作数的类型中删除 `?`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3550">First, the first expression is only ever evaluated once, and second, if the second operand's type is a non-nullable value type and the first operand's type is, the `?` is removed from the type of the first operand when determining the dominant type for the result type of the expression.</span></span> <span data-ttu-id="ba6f6-3551">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3551">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x?, y As Integer
        Dim a?, b As Long

        a = If(x, a)        ' Result type: Long?
        y = If(x, 0)        ' Result type: Integer
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3552">在这两种形式的表达式中，如果操作数是 `Nothing`的，则不会使用其类型来确定基准类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3552">In both forms of the expression, if an operand is `Nothing`, its type is not used to determine the dominant type.</span></span> <span data-ttu-id="ba6f6-3553">如果表达式 `If(<expression>, Nothing, Nothing)`，则将主导类型视为 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3553">In the case of the expression `If(<expression>, Nothing, Nothing)`, the dominant type is considered to be `Object`.</span></span>


## <a name="xml-literal-expressions"></a><span data-ttu-id="ba6f6-3554">XML 文本表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3554">XML Literal Expressions</span></span>

<span data-ttu-id="ba6f6-3555">XML 文本表达式表示 XML （可扩展标记语言）1.0 值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3555">An XML literal expression represents an XML (eXtensible Markup Language) 1.0 value.</span></span>

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

<span data-ttu-id="ba6f6-3556">XML 文本表达式的结果是类型化为 `System.Xml.Linq` 命名空间中的类型之一的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3556">The result of an XML literal expression is a value typed as one of the types from the `System.Xml.Linq` namespace.</span></span> <span data-ttu-id="ba6f6-3557">如果该命名空间中的类型不可用，则 XML 文本表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3557">If the types in that namespace are not available, then an XML literal expression will cause a compile-time error.</span></span> <span data-ttu-id="ba6f6-3558">这些值是通过从 XML 文本表达式转换的构造函数调用生成的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3558">The values are generated through constructor calls translated from the XML literal expression.</span></span> <span data-ttu-id="ba6f6-3559">例如，以下代码：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3559">For example, the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

<span data-ttu-id="ba6f6-3560">大致等效于以下代码：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3560">is roughly equivalent to the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

<span data-ttu-id="ba6f6-3561">XML 文本表达式可以采用 XML 文档、XML 元素、XML 处理指令、XML 注释或 CDATA 节的形式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3561">An XML literal expression can take the form of an XML document, an XML element, an XML processing instruction, an XML comment, or a CDATA section.</span></span>

<span data-ttu-id="ba6f6-3562">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3562">__Note.__</span></span> <span data-ttu-id="ba6f6-3563">此规范仅包含 XML 说明，用于描述 Visual Basic 语言的行为。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3563">This specification contains only enough of a description of XML to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="ba6f6-3564">有关 XML 的详细信息，请参阅 http://www.w3.org/TR/REC-xml/。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3564">More information on XML can be found at http://www.w3.org/TR/REC-xml/.</span></span>


### <a name="lexical-rules"></a><span data-ttu-id="ba6f6-3565">词法规则</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3565">Lexical rules</span></span>

```antlr
XMLCharacter
    : '<Unicode tab character (0x0009)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode carriage return character (0x000D)>'
    | '<Unicode characters 0x0020 - 0xD7FF>'
    | '<Unicode characters 0xE000 - 0xFFFD>'
    | '<Unicode characters 0x10000 - 0x10FFFF>'
    ;

XMLString
    : XMLCharacter+
    ;

XMLWhitespace
    : XMLWhitespaceCharacter+
    ;

XMLWhitespaceCharacter
    : '<Unicode carriage return character (0x000D)>'
    | '<Unicode linefeed character (0x000A)>'
    | '<Unicode space character (0x0020)>'
    | '<Unicode tab character (0x0009)>'
    ;

XMLNameCharacter
    : XMLLetter
    | XMLDigit
    | '.'
    | '-'
    | '_'
    | ':'
    | XMLCombiningCharacter
    | XMLExtender
    ;

XMLNameStartCharacter
    : XMLLetter
    | '_'
    | ':'
    ;

XMLName
    : XMLNameStartCharacter XMLNameCharacter*
    ;

XMLLetter
    : '<Unicode character as defined in the Letter production of the XML 1.0 specification>'
    ;

XMLDigit
    : '<Unicode character as defined in the Digit production of the XML 1.0 specification>'
    ;

XMLCombiningCharacter
    : '<Unicode character as defined in the CombiningChar production of the XML 1.0 specification>'
    ;

XMLExtender
    : '<Unicode character as defined in the Extender production of the XML 1.0 specification>'
    ;
```

<span data-ttu-id="ba6f6-3566">使用 XML 的词法规则而不是常规 Visual Basic 代码的词法规则来解释 XML 文本表达式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3566">XML literal expressions are interpreted using the lexical rules of XML instead of the lexical rules of regular Visual Basic code.</span></span> <span data-ttu-id="ba6f6-3567">这两组规则通常在以下方面有所不同：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3567">The two sets of rules generally differ in the following ways:</span></span>

* <span data-ttu-id="ba6f6-3568">空白在 XML 中是有意义的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3568">White space is significant in XML.</span></span> <span data-ttu-id="ba6f6-3569">因此，XML 文本表达式的语法明确指出允许使用空格。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3569">As a result, the grammar for XML literal expressions explicitly states where white space is allowed.</span></span> <span data-ttu-id="ba6f6-3570">空白不会保留，除非它出现在元素内的字符数据的上下文中。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3570">Whitespace is not preserved, except when it occurs in the context of character data within an element.</span></span> <span data-ttu-id="ba6f6-3571">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3571">For example:</span></span>

  ```vb
  ' The following element preserves no whitespace
  Dim e1 = _
      <customer>
          <name>Bob</>
      </>

  ' The following element preserves all of the whitespace
  Dim e2 = _
      <customer>
          Bob
      </>
  ```

* <span data-ttu-id="ba6f6-3572">XML 的行尾空格根据 XML 规范规范化。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3572">XML end-of-line whitespace is normalized according to the XML specification.</span></span>

* <span data-ttu-id="ba6f6-3573">XML 区分大小写。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3573">XML is case-sensitive.</span></span> <span data-ttu-id="ba6f6-3574">关键字必须完全匹配大小写，否则将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3574">Keywords must match casing exactly, or else a compile-time error will occur.</span></span>

* <span data-ttu-id="ba6f6-3575">行结束符被视为 XML 中的空白区域。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3575">Line terminators are considered white space in XML.</span></span> <span data-ttu-id="ba6f6-3576">因此，XML 文本表达式中不需要行继续符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3576">As a result, no line-continuation characters are needed in XML literal expressions.</span></span>

* <span data-ttu-id="ba6f6-3577">XML 不接受全角字符。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3577">XML does not accept full-width characters.</span></span> <span data-ttu-id="ba6f6-3578">如果使用全角字符，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3578">If full-width characters are used, a compile-time error will occur.</span></span>


### <a name="embedded-expressions"></a><span data-ttu-id="ba6f6-3579">嵌入的表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3579">Embedded expressions</span></span>

<span data-ttu-id="ba6f6-3580">XML 文本表达式可以包含*嵌入式表达式*。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3580">XML literal expressions can contain *embedded expressions*.</span></span> <span data-ttu-id="ba6f6-3581">嵌入式表达式是一种 Visual Basic 表达式，该表达式将进行计算并用于在嵌入表达式的位置填充一个或多个值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3581">An embedded expression is a Visual Basic expression that is evaluated and used to fill in one or more values at the location of embedded expression.</span></span>

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

<span data-ttu-id="ba6f6-3582">例如，下面的代码将字符串作为 XML 元素的值 `John Smith`：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3582">For example, the following code places the string `John Smith` as the value of the XML element:</span></span>

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

<span data-ttu-id="ba6f6-3583">表达式可以嵌入到多个上下文中。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3583">Expressions can be embedded in a number of contexts.</span></span> <span data-ttu-id="ba6f6-3584">例如，下面的代码生成一个名为 `customer`的元素：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3584">For example, the following code produces an element named `customer`:</span></span>

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

<span data-ttu-id="ba6f6-3585">可以使用嵌入式表达式的每个上下文指定将接受的类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3585">Each context where an embedded expression can be used specifies the types that will be accepted.</span></span> <span data-ttu-id="ba6f6-3586">在嵌入表达式的表达式部分的上下文中，Visual Basic 代码的普通词法规则仍适用，例如，必须使用行继续符：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3586">When within the context of the expression part of an embedded expression, the normal lexical rules for Visual Basic code still apply so, for example, line continuations must be used:</span></span>

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a><span data-ttu-id="ba6f6-3587">XML 文档</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3587">XML Documents</span></span>

```antlr
XMLDocument
    : XMLDocumentPrologue XMLMisc* XMLDocumentBody XMLMisc*
    ;

XMLDocumentPrologue
    : '<' '?' 'xml' XMLVersion XMLEncoding? XMLStandalone? XMLWhitespace? '?' '>'
    ;

XMLVersion
    : XMLWhitespace 'version' XMLWhitespace? '=' XMLWhitespace? XMLVersionNumberValue
    ;

XMLVersionNumberValue
    : SingleQuoteCharacter '1' '.' '0' SingleQuoteCharacter
    | DoubleQuoteCharacter '1' '.' '0' DoubleQuoteCharacter
    ;

XMLEncoding
    : XMLWhitespace 'encoding' XMLWhitespace? '=' XMLWhitespace? XMLEncodingNameValue
    ;

XMLEncodingNameValue
    : SingleQuoteCharacter XMLEncodingName SingleQuoteCharacter
    | DoubleQuoteCharacter XMLEncodingName DoubleQuoteCharacter
    ;

XMLEncodingName
    : XMLLatinAlphaCharacter XMLEncodingNameCharacter*
    ;

XMLEncodingNameCharacter
    : XMLUnderscoreCharacter
    | XMLLatinAlphaCharacter
    | XMLNumericCharacter
    | XMLPeriodCharacter
    | XMLDashCharacter
    ;

XMLLatinAlphaCharacter
    : '<Unicode Latin alphabetic character (0x0041-0x005a, 0x0061-0x007a)>'
    ;

XMLNumericCharacter
    : '<Unicode digit character (0x0030-0x0039)>'
    ;

XMLHexNumericCharacter
    : XMLNumericCharacter
    | '<Unicode Latin hex alphabetic character (0x0041-0x0046, 0x0061-0x0066)>'
    ;

XMLPeriodCharacter
    : '<Unicode period character (0x002e)>'
    ;

XMLUnderscoreCharacter
    : '<Unicode underscore character (0x005f)>'
    ;

XMLDashCharacter
    : '<Unicode dash character (0x002d)>'
    ;

XMLStandalone
    : XMLWhitespace 'standalone' XMLWhitespace? '=' XMLWhitespace? XMLYesNoValue
    ;

XMLYesNoValue
    : SingleQuoteCharacter XMLYesNo SingleQuoteCharacter
    | DoubleQuoteCharacter XMLYesNo DoubleQuoteCharacter
    ;

XMLYesNo
    : 'yes'
    | 'no'
    ;

XMLMisc
    : XMLComment
    | XMLProcessingInstruction
    | XMLWhitespace
    ;

XMLDocumentBody
    : XMLElement
    | XMLEmbeddedExpression
    ;
```

<span data-ttu-id="ba6f6-3588">XML 文档会生成一个类型为 `System.Xml.Linq.XDocument`的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3588">An XML document results in a value typed as `System.Xml.Linq.XDocument`.</span></span> <span data-ttu-id="ba6f6-3589">与 XML 1.0 规范不同，XML 文本表达式中的 XML 文档需要指定 XML 文档序言;不带 XML 文档序言的 XML 文本表达式解释为其单独的实体。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3589">Unlike the XML 1.0 specification, XML documents in XML literal expressions are required to specify the XML document prologue; XML literal expressions without the XML document prologue are interpreted as their individual entity.</span></span> <span data-ttu-id="ba6f6-3590">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3590">For example:</span></span>

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

<span data-ttu-id="ba6f6-3591">XML 文档可以包含其类型可以是任何类型的嵌入式表达式;但是，在运行时，对象必须满足 `XDocument` 构造函数的要求，否则将发生运行时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3591">An XML document can contain an embedded expression whose type can be any type; at runtime, however, the object must satisfy the requirements of the `XDocument` constructor or a run-time error will occur.</span></span>

<span data-ttu-id="ba6f6-3592">与常规 XML 不同，XML 文档表达式不支持 Dtd （文档类型声明）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3592">Unlike regular XML, XML document expressions do not support DTDs (Document Type Declarations).</span></span> <span data-ttu-id="ba6f6-3593">此外，如果提供了编码特性，则将被忽略，因为 Xml 文本表达式的编码始终与源文件本身的编码相同。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3593">Also, the encoding attribute, if supplied, will be ignored since the encoding of the Xml literal expression is always the same as the encoding of the source file itself.</span></span>

<span data-ttu-id="ba6f6-3594">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3594">__Note.__</span></span> <span data-ttu-id="ba6f6-3595">尽管编码属性被忽略，但它仍然是有效的属性，以便保持在源代码中包含任何有效 Xml 1.0 文档的能力。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3595">Although the encoding attribute is ignored, it is still valid attribute in order to maintain the ability to include any valid Xml 1.0 documents in source code.</span></span>


### <a name="xml-elements"></a><span data-ttu-id="ba6f6-3596">XML 元素</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3596">XML Elements</span></span>

```antlr
XMLElement
    : XMLEmptyElement
    | XMLElementStart XMLContent XMLElementEnd
    ;

XMLEmptyElement
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '/' '>'
    ;

XMLElementStart
    : '<' XMLQualifiedNameOrExpression XMLAttribute* XMLWhitespace? '>'
    ;

XMLElementEnd
    : '<' '/' '>'
    | '<' '/' XMLQualifiedName XMLWhitespace? '>'
    ;

XMLContent
    : XMLCharacterData? ( XMLNestedContent XMLCharacterData? )+
    ;

XMLCharacterData
    : '<Any XMLCharacterDataString that does not contain the string "]]>">'
    ;

XMLCharacterDataString
    : '<Any Unicode character except < or &>'+
    ;

XMLNestedContent
    : XMLElement
    | XMLReference
    | XMLCDATASection
    | XMLProcessingInstruction
    | XMLComment
    | XMLEmbeddedExpression
    ;

XMLAttribute
    : XMLWhitespace XMLAttributeName XMLWhitespace? '=' XMLWhitespace? XMLAttributeValue
    | XMLWhitespace XMLEmbeddedExpression
    ;

XMLAttributeName
    : XMLQualifiedNameOrExpression
    | XMLNamespaceAttributeName
    ;

XMLAttributeValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    | XMLEmbeddedExpression
    ;

XMLAttributeDoubleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or DoubleQuoteCharacter>'
    | XMLReference
    ;

XMLAttributeSingleQuoteValueCharacter
    : '<Any XMLCharacter except <, &, or SingleQuoteCharacter>'
    | XMLReference
    ;

XMLReference
    : XMLEntityReference
    | XMLCharacterReference
    ;

XMLEntityReference
    : '&' XMLEntityName ';'
    ;

XMLEntityName
    : 'lt' | 'gt' | 'amp' | 'apos' | 'quot'
    ;

XMLCharacterReference
    : '&' '#' XMLNumericCharacter+ ';'
    | '&' '#' 'x' XMLHexNumericCharacter+ ';'
    ;
```

<span data-ttu-id="ba6f6-3597">XML 元素会生成一个类型为 `System.Xml.Linq.XElement`的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3597">An XML element results in a value typed as `System.Xml.Linq.XElement`.</span></span> <span data-ttu-id="ba6f6-3598">与常规 XML 不同，XML 元素可以在结束标记中省略名称，当前的最嵌套元素将关闭。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3598">Unlike regular XML, XML elements can omit the name in the closing tag and the current most-nested element will be closed.</span></span> <span data-ttu-id="ba6f6-3599">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3599">For example:</span></span>

```vb
Dim name = <name>Bob</>
```

<span data-ttu-id="ba6f6-3600">XML 元素中的属性声明将生成类型为 `System.Xml.Linq.XAttribute`的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3600">Attribute declarations in an XML element result in values typed as `System.Xml.Linq.XAttribute`.</span></span> <span data-ttu-id="ba6f6-3601">属性值根据 XML 规范进行标准化。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3601">Attribute values are normalized according to the XML specification.</span></span> <span data-ttu-id="ba6f6-3602">如果 `Nothing` 属性的值，则不会创建属性，因此不需要检查属性值表达式的 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3602">When the value of an attribute is `Nothing` the attribute will not be created, so the attribute value expression will not have to be checked for `Nothing`.</span></span> <span data-ttu-id="ba6f6-3603">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3603">For example:</span></span>

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

<span data-ttu-id="ba6f6-3604">XML 元素和属性可以在以下位置包含嵌套表达式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3604">XML elements and attributes can contain nested expressions in the following places:</span></span>

<span data-ttu-id="ba6f6-3605">元素的名称，在这种情况下，嵌入式表达式必须是可隐式转换为 `System.Xml.Linq.XName`的类型的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3605">The name of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="ba6f6-3606">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3606">For example:</span></span>

```vb
Dim name = <<%= "name" %>>Bob</>
```

<span data-ttu-id="ba6f6-3607">元素的属性名称，在这种情况下，嵌入式表达式必须是可隐式转换为 `System.Xml.Linq.XName`的类型的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3607">The name of an attribute of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="ba6f6-3608">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3608">For example:</span></span>

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

<span data-ttu-id="ba6f6-3609">元素特性的值，在这种情况下，嵌入式表达式可以是任何类型的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3609">The value of an attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="ba6f6-3610">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3610">For example:</span></span>

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

<span data-ttu-id="ba6f6-3611">元素的一个特性，在这种情况下，嵌入式表达式可以是任何类型的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3611">An attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="ba6f6-3612">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3612">For example:</span></span>

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

<span data-ttu-id="ba6f6-3613">元素的内容，在这种情况下，嵌入式表达式可以是任何类型的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3613">The content of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="ba6f6-3614">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3614">For example:</span></span>

```vb
Dim name = <name><%= "Bob" %></>
```

<span data-ttu-id="ba6f6-3615">如果 `Object()`嵌入的表达式的类型，则该数组将作为 paramarray 传递到 `XElement` 构造函数。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3615">If the type of the embedded expression is `Object()`, the array will be passed as a paramarray to the `XElement` constructor.</span></span>


### <a name="xml-namespaces"></a><span data-ttu-id="ba6f6-3616">XML 命名空间</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3616">XML Namespaces</span></span>

<span data-ttu-id="ba6f6-3617">XML 元素可以包含 xml 命名空间声明，如 XML 命名空间1.0 规范所定义。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3617">XML elements can contain XML namespace declarations, as defined by the XML namespaces 1.0 specification.</span></span>

```antlr
XMLNamespaceAttributeName
    : XMLPrefixedNamespaceAttributeName
    | XMLDefaultNamespaceAttributeName
    ;

XMLPrefixedNamespaceAttributeName
    : 'xmlns' ':' XMLNamespaceName
    ;

XMLDefaultNamespaceAttributeName
    : 'xmlns'
    ;

XMLNamespaceName
    : XMLNamespaceNameStartCharacter XMLNamespaceNameCharacter*
    ;

XMLNamespaceNameStartCharacter
    : '<Any XMLNameCharacter except :>'
    ;

XMLNamespaceNameCharacter
    : XMLLetter
    | '_'
    ;

XMLQualifiedNameOrExpression
    : XMLQualifiedName
    | XMLEmbeddedExpression
    ;

XMLQualifiedName
    : XMLPrefixedName
    | XMLUnprefixedName
    ;

XMLPrefixedName
    : XMLNamespaceName ':' XMLNamespaceName
    ;

XMLUnprefixedName
    : XMLNamespaceName
    ;
```

<span data-ttu-id="ba6f6-3618">`xml` 和 `xmlns` 定义命名空间的限制将被强制执行，并将产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3618">The restrictions on defining the namespaces `xml` and `xmlns` are enforced and will produce compile-time errors.</span></span> <span data-ttu-id="ba6f6-3619">XML 命名空间声明不能有其值的嵌入式表达式;提供的值必须为非空字符串。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3619">XML namespace declarations cannot have an embedded expression for their value; the value supplied must be a non-empty string literal.</span></span> <span data-ttu-id="ba6f6-3620">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3620">For example:</span></span>

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

<span data-ttu-id="ba6f6-3621">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3621">__Note.__</span></span> <span data-ttu-id="ba6f6-3622">此规范只包含 XML 命名空间的足够说明，用于描述 Visual Basic 语言的行为。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3622">This specification contains only enough of a description of XML namespace to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="ba6f6-3623">有关 XML 命名空间的详细信息，请参阅 http://www.w3.org/TR/REC-xml-names/。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3623">More information on XML namespaces can be found at http://www.w3.org/TR/REC-xml-names/.</span></span>

<span data-ttu-id="ba6f6-3624">可以使用命名空间名称限定 XML 元素和属性名称。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3624">XML element and attribute names can be qualified using namespace names.</span></span> <span data-ttu-id="ba6f6-3625">命名空间作为常规 XML 绑定，但在文件级别声明的任何命名空间导入都被视为在包含声明的上下文中声明，该声明本身由编译声明的任何命名空间导入括起来。环境.</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3625">Namespaces are bound as in regular XML, with the exception that any namespace imports declared at the file level are considered to be declared in a context enclosing the declaration, which is itself enclosed by any namespace imports declared by the compilation environment.</span></span> <span data-ttu-id="ba6f6-3626">如果找不到命名空间名称，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3626">If a namespace name cannot be found, a compile-time error occurs.</span></span> <span data-ttu-id="ba6f6-3627">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3627">For example:</span></span>

```vb
Imports System.Xml.Linq
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' Binds to the imported namespace above.
        Dim c1 = <db:customer>Bob</>

        ' Binds to the namespace declaration in the element
        Dim c2 = _
            <db:customer xmlns:db="http://example.org/database-other">Mary</>

        ' Binds to the inner namespace declaration
        Dim c3 = _
            <database xmlns:db="http://example.org/database-one">
                <db:customer xmlns:db="http://example.org/database-two">Joe</>
            </>

        ' Error: namespace db2 cannot be found
        Dim c4 = _
            <db2:customer>Jim</>
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3628">元素中声明的 XML 命名空间不适用于嵌入式表达式中的 XML 文本。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3628">XML namespaces declared in an element do not apply to XML literals inside embedded expressions.</span></span> <span data-ttu-id="ba6f6-3629">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3629">For example:</span></span>

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

<span data-ttu-id="ba6f6-3630">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3630">__Note.__</span></span> <span data-ttu-id="ba6f6-3631">这是因为嵌入式表达式可以是任何内容，包括函数调用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3631">This is because the embedded expression can be anything, including a function call.</span></span> <span data-ttu-id="ba6f6-3632">如果函数调用包含 XML 文本表达式，则不清楚程序员是否需要应用或忽略 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3632">If the function call contained an XML literal expression, it is not clear whether programmers would expect the XML namespace to be applied or ignored.</span></span>


### <a name="xml-processing-instructions"></a><span data-ttu-id="ba6f6-3633">XML 处理指令</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3633">XML Processing Instructions</span></span>

<span data-ttu-id="ba6f6-3634">XML 处理指令会生成一个类型为 `System.Xml.Linq.XProcessingInstruction`的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3634">An XML processing instruction results in a value typed as `System.Xml.Linq.XProcessingInstruction`.</span></span> <span data-ttu-id="ba6f6-3635">XML 处理指令不能包含嵌入的表达式，因为它们是处理指令中的有效语法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3635">XML processing instructions cannot contain embedded expressions, as they are valid syntax within the processing instruction.</span></span>

```antlr
XMLProcessingInstruction
    : '<' '?' XMLProcessingTarget ( XMLWhitespace XMLProcessingValue? )? '?' '>'
    ;

XMLProcessingTarget
    : '<Any XMLName except a casing permutation of the string "xml">'
    ;

XMLProcessingValue
    : '<Any XMLString that does not contain a question-mark followed by ">">'
    ;
```

### <a name="xml-comments"></a><span data-ttu-id="ba6f6-3636">XML 注释</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3636">XML Comments</span></span>

<span data-ttu-id="ba6f6-3637">XML 注释会生成一个类型为 `System.Xml.Linq.XComment`的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3637">An XML comment results in a value typed as `System.Xml.Linq.XComment`.</span></span> <span data-ttu-id="ba6f6-3638">XML 注释不能包含嵌入的表达式，因为它们是注释中的有效语法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3638">XML comments cannot contain embedded expressions, as they are valid syntax within the comment.</span></span>

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a><span data-ttu-id="ba6f6-3639">CDATA 节</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3639">CDATA sections</span></span>

<span data-ttu-id="ba6f6-3640">CDATA 节会生成一个类型为 `System.Xml.Linq.XCData`的值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3640">A CDATA section results in a value typed as `System.Xml.Linq.XCData`.</span></span> <span data-ttu-id="ba6f6-3641">CDATA 节不能包含嵌入的表达式，因为它们是 CDATA 节中的有效语法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3641">CDATA sections cannot contain embedded expressions, as they are valid syntax within the CDATA section.</span></span>

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a><span data-ttu-id="ba6f6-3642">XML 成员访问表达式</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3642">XML Member Access Expressions</span></span>

<span data-ttu-id="ba6f6-3643">XML 成员访问表达式访问 XML 值的成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3643">An XML member access expression accesses the members of an XML value.</span></span>

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

<span data-ttu-id="ba6f6-3644">有三种类型的 XML 成员访问表达式：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3644">There are three types of XML member access expressions:</span></span>

* <span data-ttu-id="ba6f6-3645">*元素访问权限*，其中的 XML 名称跟在单点之后。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3645">*Element access*, in which an XML name follows a single dot.</span></span> <span data-ttu-id="ba6f6-3646">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3646">For example:</span></span>

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  <span data-ttu-id="ba6f6-3647">元素访问映射到函数：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3647">Element access maps to the function:</span></span>

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  <span data-ttu-id="ba6f6-3648">因此上面的示例等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3648">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* <span data-ttu-id="ba6f6-3649">*属性访问*，其中 Visual Basic 标识符跟在点和 at 符号后，或者 XML 名称后跟一个点和 at 符号。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3649">*Attribute access*, in which a Visual Basic identifier follows a dot and an at sign, or an XML name follows a dot and an at sign.</span></span> <span data-ttu-id="ba6f6-3650">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3650">For example:</span></span>

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  <span data-ttu-id="ba6f6-3651">特性访问映射到函数：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3651">Attribute access maps to the function:</span></span>

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  <span data-ttu-id="ba6f6-3652">因此上面的示例等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3652">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  <span data-ttu-id="ba6f6-3653">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3653">__Note.__</span></span> <span data-ttu-id="ba6f6-3654">当前未在任何程序集中定义 `AttributeValue` 扩展方法（以及相关的扩展属性 `Value`）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3654">The `AttributeValue` extension method (as well as the related extension property `Value`) is not currently defined in any assembly.</span></span> <span data-ttu-id="ba6f6-3655">如果需要扩展成员，则会在生成的程序集中自动定义这些成员。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3655">If the extension members are needed, they are automatically defined in the assembly being produced.</span></span>

* <span data-ttu-id="ba6f6-3656">*后代访问*，其中的 XML 名称遵循三个点。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3656">*Descendents access*, in which an XML names follows three dots.</span></span> <span data-ttu-id="ba6f6-3657">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3657">For example:</span></span>

  ```vb
  Dim company = _
      <company>
          <customers>
              <customer>Bob</>
              <customer>Mary</>
              <customer>Joe</>
          </>
      </>
  Dim customers = company...<customer>
  ```

  <span data-ttu-id="ba6f6-3658">后代访问映射到函数：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3658">Descendents access maps to the function:</span></span>

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  <span data-ttu-id="ba6f6-3659">因此上面的示例等效于：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3659">So the above example is equivalent to:</span></span>

  ```vb
  Dim customers = company.Descendants("customer")
  ```

<span data-ttu-id="ba6f6-3660">XML 成员访问表达式的基本表达式必须是值，并且必须是以下类型：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3660">The base expression of an XML member access expression must be a value and must be of the type:</span></span>

* <span data-ttu-id="ba6f6-3661">如果元素或子代访问、`System.Xml.Linq.XContainer` 或派生类型，或者 `System.Collections.Generic.IEnumerable(Of T)` 或派生类型，其中 `T` 为 `System.Xml.Linq.XContainer` 或派生类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3661">If an element or descendents access,  `System.Xml.Linq.XContainer` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XContainer` or a derived type.</span></span>

* <span data-ttu-id="ba6f6-3662">如果属性访问、`System.Xml.Linq.XElement` 或派生类型，或者 `System.Collections.Generic.IEnumerable(Of T)` 或派生类型（其中 `T` 是 `System.Xml.Linq.XElement` 或派生类型），则为。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3662">If an attribute access, `System.Xml.Linq.XElement` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XElement` or a derived type.</span></span>

<span data-ttu-id="ba6f6-3663">XML 成员访问表达式中的名称不能为空。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3663">Names in XML member access expressions cannot be empty.</span></span> <span data-ttu-id="ba6f6-3664">可以使用由 imports 定义的任何命名空间来限定命名空间。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3664">They can be namespace qualified, using any namespaces defined by imports.</span></span> <span data-ttu-id="ba6f6-3665">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3665">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        Dim customer = _
            <db:customer>
                <db:name>Bob</>
            </>
        Dim name = customer.<db:name>
    End Sub
End Module
```

<span data-ttu-id="ba6f6-3666">在 XML 成员访问表达式中的点或尖括号和名称之间不允许使用空格。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3666">Whitespace is not allowed after the dot(s) in an XML member access expression, or between the angle brackets and the name.</span></span> <span data-ttu-id="ba6f6-3667">例如：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3667">For example:</span></span>

```vb
Dim customer = _
    <customer age="30">
        <name>Bob</>
    </>
' All the following are error cases
Dim age = customer.@ age
Dim name = customer.< name >
Dim names = customer...< name >
```

<span data-ttu-id="ba6f6-3668">如果 `System.Xml.Linq` 命名空间中的类型不可用，则 XML 成员访问表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3668">If the types in the `System.Xml.Linq` namespace are not available, then an XML member access expression will cause a compile-time error.</span></span>


## <a name="await-operator"></a><span data-ttu-id="ba6f6-3669">Await 运算符</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3669">Await Operator</span></span>

<span data-ttu-id="ba6f6-3670">Await 运算符与异步方法相关，如[Async 方法](statements.md#async-methods)一节中所述。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3670">The await operator is related to async methods, which are described in Section [Async Methods](statements.md#async-methods).</span></span>

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

<span data-ttu-id="ba6f6-3671">`Await` 是一个保留字，如果其出现在其中的直接封闭方法或 lambda 表达式具有 `Async` 修饰符，并且 `Await` 出现在该 `Async` 修饰符之后，则为; 否则为。它不会保留在别处。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3671">`Await` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Async` modifier, and if the `Await` appears after that `Async` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="ba6f6-3672">它也不会在预处理器指令中保留。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3672">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="ba6f6-3673">Await 运算符仅允许在它是保留字的方法或 lambda 表达式的主体中使用。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3673">The await operator is only allowed in the body of a method or lambda expressions where it is a reserved word.</span></span> <span data-ttu-id="ba6f6-3674">在直接封闭方法或 lambda 中，await 表达式不能出现在 `Catch` 或 `Finally` 块的主体内，也不能出现在 `SyncLock` 语句的主体内，也不在查询表达式中。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3674">Within the immediately enclosing method or lambda, an await expression may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement, nor inside a query expression.</span></span>

<span data-ttu-id="ba6f6-3675">Await 运算符采用一个表达式，该表达式必须分类为值，并且其类型必须为*可等待*类型或 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3675">The await operator takes a single expression which must be classified as a value and whose type must be an *awaitable* type, or `Object`.</span></span> <span data-ttu-id="ba6f6-3676">如果其类型 `Object`，则所有处理都将推迟到运行时。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3676">If its type is `Object` then all processing is deferred until run-time.</span></span> <span data-ttu-id="ba6f6-3677">如果满足以下所有条件，则称 `C` 类型为可等待：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3677">A type `C` is said to be awaitable if all of the following are true:</span></span>

* <span data-ttu-id="ba6f6-3678">`C` 包含一个名为 `GetAwaiter` 的可访问实例或扩展方法，该实例或扩展方法没有参数，并返回 `E`类型;</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3678">`C` contains an accessible instance or extension method named `GetAwaiter` which has no arguments and which returns some type `E`;</span></span>

* <span data-ttu-id="ba6f6-3679">`E` 包含一个名为 `IsCompleted` 的可读实例或扩展属性，它不采用任何参数，且具有类型 Boolean;</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3679">`E` contains a readable instance or extension property named `IsCompleted` which takes no arguments and has type Boolean;</span></span>

* <span data-ttu-id="ba6f6-3680">`E` 包含一个名为 `GetResult` 的可访问实例或扩展方法，它不采用参数;</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3680">`E` contains an accessible instance or extension method named `GetResult` which takes no arguments;</span></span>

* <span data-ttu-id="ba6f6-3681">`E` 实现 `System.Runtime.CompilerServices.INotifyCompletion` 或 `ICriticalNotifyCompletion`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3681">`E` implements either `System.Runtime.CompilerServices.INotifyCompletion` or `ICriticalNotifyCompletion`.</span></span>

<span data-ttu-id="ba6f6-3682">如果 `GetResult` 是 `Sub`，则 await 表达式归类为 void。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3682">If `GetResult` was a `Sub`, then the await expression is classified as void.</span></span> <span data-ttu-id="ba6f6-3683">否则，await 表达式归类为值，其类型为 `GetResult` 方法的返回类型。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3683">Otherwise, the await expression is classified as a value and its type is the return type of the `GetResult` method.</span></span>

<span data-ttu-id="ba6f6-3684">下面是一个可等待的类的示例：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3684">Here is an example of a class that can be awaited:</span></span>

```vb
Class MyTask(Of T)
    Function GetAwaiter() As MyTaskAwaiter(Of T)
        Return New MyTaskAwaiter With {.m_Task = Me}
    End Function

    ...
End Class

Structure MyTaskAwaiter(Of T)
    Implements INotifyCompletion

    Friend m_Task As MyTask(Of T)

    ReadOnly Property IsCompleted As Boolean
        Get
            Return m_Task.IsCompleted
        End Get
    End Property

    Sub OnCompleted(r As Action) Implements INotifyCompletion.OnCompleted
        ' r is the "resumptionDelegate"
        Dim sc = SynchronizationContext.Current
        If sc Is Nothing Then
            m_Task.ContinueWith(Sub() r())
        Else
            m_Task.ContinueWith(Sub() sc.Post(Sub() r(), Nothing))
        End If
    End Sub

    Function GetResult() As T
        If m_Task.IsCanceled Then Throw New TaskCanceledException(m_Task)
        If m_Task.IsFaulted Then Throw m_Task.Exception.InnerException
        Return m_Task.Result
    End Function
End Structure
```

<span data-ttu-id="ba6f6-3685">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3685">__Note.__</span></span> <span data-ttu-id="ba6f6-3686">建议将库作者按照其在其 `OnCompleted` 在其上调用时的相同 `SynchronizationContext` 上调用延续委托的模式。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3686">Library authors are recommended to follow the pattern that they invoke the continuation delegate on the same `SynchronizationContext` as their `OnCompleted` was itself invoked on.</span></span> <span data-ttu-id="ba6f6-3687">此外，不应在 `OnCompleted` 方法中同步执行恢复委托，因为这可能导致堆栈溢出：相反，应将委托排队等候以后执行。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3687">Also, the resumption delegate should not be executed synchronously within the `OnCompleted` method since that can lead to stack overflow: instead, the delegate should be queued for subsequent execution.</span></span>

<span data-ttu-id="ba6f6-3688">当控制流到达 `Await` 运算符时，行为如下所示。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3688">When control flow reaches an `Await` operator, behavior is as follows.</span></span>

1.  <span data-ttu-id="ba6f6-3689">调用 await 操作数的 `GetAwaiter` 方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3689">The `GetAwaiter` method of the await operand is invoked.</span></span> <span data-ttu-id="ba6f6-3690">此调用的结果称为*awaiter*。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3690">The result of this invocation is termed the *awaiter*.</span></span>

2.  <span data-ttu-id="ba6f6-3691">检索 awaiter 的 `IsCompleted` 属性。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3691">The awaiter's `IsCompleted` property is retrieved.</span></span> <span data-ttu-id="ba6f6-3692">如果结果为 true，则：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3692">If the result is true then:</span></span>

    21. <span data-ttu-id="ba6f6-3693">调用 awaiter 的 `GetResult` 方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3693">The `GetResult` method of the awaiter is invoked.</span></span> <span data-ttu-id="ba6f6-3694">如果 `GetResult` 是一个函数，则 await 表达式的值为此函数的返回值。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3694">If `GetResult` was a function, then the value of the await expression is the return value of this function.</span></span>

3.  <span data-ttu-id="ba6f6-3695">如果 IsCompleted 属性不为 true，则：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3695">If the IsCompleted property isn't true then:</span></span>

    31. <span data-ttu-id="ba6f6-3696">对 awaiter 调用 `ICriticalNotifyCompletion.UnsafeOnCompleted` （如果 awaiter 的类型 `E` 实现 `ICriticalNotifyCompletion`）或 `INotifyCompletion.OnCompleted` （否则为）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3696">Either `ICriticalNotifyCompletion.UnsafeOnCompleted` is invoked on the awaiter (if the awaiter's type `E` implements `ICriticalNotifyCompletion`) or `INotifyCompletion.OnCompleted` (otherwise).</span></span> <span data-ttu-id="ba6f6-3697">在这两种情况下，它会传递与异步方法的当前实例关联的*恢复委托*。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3697">In both cases it passes a *resumption delegate* associated with the current instance of the async method.</span></span>

    32. <span data-ttu-id="ba6f6-3698">当前 async 方法实例的控制点已挂起，并在*当前调用方*（在节[async 方法](statements.md#async-methods)中定义）中恢复控制流。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3698">The control point of the current async method instance is suspended, and control flow resumes in the *current caller* (defined in Section [Async Methods](statements.md#async-methods)).</span></span>

    33. <span data-ttu-id="ba6f6-3699">如果以后调用恢复委托，</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3699">If later the resumption delegate is invoked,</span></span>

        331. <span data-ttu-id="ba6f6-3700">恢复委托首先将 `System.Threading.Thread.CurrentThread.ExecutionContext` 恢复为调用 `OnCompleted` 时的状态，</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3700">the resumption delegate first restores `System.Threading.Thread.CurrentThread.ExecutionContext` to what it was at the time `OnCompleted` was called,</span></span>
        332. <span data-ttu-id="ba6f6-3701">然后，它在异步方法实例的控制点处恢复控制流（请参阅[异步](statements.md#async-methods)方法部分）。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3701">then it resumes control flow at the control point of the async method instance (see Section [Async Methods](statements.md#async-methods)),</span></span>
        333. <span data-ttu-id="ba6f6-3702">它调用 awaiter 的 `GetResult` 方法，如以上2.1 中所示。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3702">where it calls the `GetResult` method of the awaiter, as in 2.1 above.</span></span>

<span data-ttu-id="ba6f6-3703">如果 await 操作数具有 type 对象，则此行为将推迟到运行时：</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3703">If the await operand has type Object, then this behavior is deferred until runtime:</span></span>

- <span data-ttu-id="ba6f6-3704">步骤1通过调用不带参数的 GetAwaiter （）来实现;因此，它可能会在运行时绑定到采用可选参数的实例方法。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3704">Step 1 is accomplished by calling GetAwaiter() with no arguments; it may therefore bind at runtime to instance methods which take optional parameters.</span></span>
- <span data-ttu-id="ba6f6-3705">步骤2是通过检索不带参数的 IsCompleted （）属性以及通过尝试将内部转换为布尔来完成的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3705">Step 2 is accomplished by retrieving the IsCompleted() property with no arguments, and by attempting an intrinsic conversion to Boolean.</span></span>
- <span data-ttu-id="ba6f6-3706">步骤 3. 通过尝试 `TryCast(awaiter, ICriticalNotifyCompletion)`来完成，如果此操作失败，则 `DirectCast(awaiter, INotifyCompletion)`。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3706">Step 3.a is accomplished by attempting `TryCast(awaiter, ICriticalNotifyCompletion)`, and if this fails then `DirectCast(awaiter, INotifyCompletion)`.</span></span>

<span data-ttu-id="ba6f6-3707">传入的恢复委托为3。只能调用一次。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3707">The resumption delegate passed in 3.a may only be invoked once.</span></span> <span data-ttu-id="ba6f6-3708">如果多次调用此方法，则该行为是不确定的。</span><span class="sxs-lookup"><span data-stu-id="ba6f6-3708">If it is invoked more than once, the behavior is undefined.</span></span>

