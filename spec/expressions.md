# <a name="expressions"></a><span data-ttu-id="2ada7-101">表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-101">Expressions</span></span>

<span data-ttu-id="2ada7-102">表达式是一系列运算符和操作数指定的一个值，计算值或指定变量或常量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-102">An expression is a sequence of operators and operands that specifies a computation of a value, or that designates a variable or constant.</span></span> <span data-ttu-id="2ada7-103">本章定义的语法，操作数和运算符的求值和表达式的含义的顺序。</span><span class="sxs-lookup"><span data-stu-id="2ada7-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

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

## <a name="expression-classifications"></a><span data-ttu-id="2ada7-104">表达式分类</span><span class="sxs-lookup"><span data-stu-id="2ada7-104">Expression Classifications</span></span>

<span data-ttu-id="2ada7-105">每个表达式分类为以下值之一：</span><span class="sxs-lookup"><span data-stu-id="2ada7-105">Every expression is classified as one of the following:</span></span>

* <span data-ttu-id="2ada7-106">*一个值。*</span><span class="sxs-lookup"><span data-stu-id="2ada7-106">*A value.*</span></span> <span data-ttu-id="2ada7-107">每个值都有关联的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-107">Every value has an associated type.</span></span>

* <span data-ttu-id="2ada7-108">*一个变量。*</span><span class="sxs-lookup"><span data-stu-id="2ada7-108">*A variable.*</span></span> <span data-ttu-id="2ada7-109">每个变量具有关联的类型，即该变量的声明类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-109">Every variable has an associated type, namely the declared type of the variable.</span></span>

* <span data-ttu-id="2ada7-110">*一个命名空间。*</span><span class="sxs-lookup"><span data-stu-id="2ada7-110">*A namespace.*</span></span> <span data-ttu-id="2ada7-111">具有此分类的表达式只可以显示为左侧和右侧的成员访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-111">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="2ada7-112">在任何其他上下文中，分类为一个命名空间的表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>

* <span data-ttu-id="2ada7-113">*一种类型。*</span><span class="sxs-lookup"><span data-stu-id="2ada7-113">*A type.*</span></span> <span data-ttu-id="2ada7-114">具有此分类的表达式只可以显示为左侧和右侧的成员访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-114">An expression with this classification can only appear as the left side of a member access.</span></span> <span data-ttu-id="2ada7-115">在任何其他上下文中，分类为一种类型的表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>

* <span data-ttu-id="2ada7-116">*方法组，* 这是一组相同的名称上重载的方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-116">*A method group,* which is a set of methods overloaded on the same name.</span></span> <span data-ttu-id="2ada7-117">方法组可能有一个相关联的目标表达式和关联的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="2ada7-117">A method group may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="2ada7-118">*方法指针*表示一种方法的位置。</span><span class="sxs-lookup"><span data-stu-id="2ada7-118">*A method pointer,* which represents the location of a method.</span></span> <span data-ttu-id="2ada7-119">方法指针可能有一个相关联的目标表达式和关联的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="2ada7-119">A method pointer may have an associated target expression and an associated type argument list.</span></span>

* <span data-ttu-id="2ada7-120">*Lambda 方法*即匿名方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-120">*A lambda method,* which is an anonymous method.</span></span>

* <span data-ttu-id="2ada7-121">*属性组中，* 这是一组相同的名称上重载的属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-121">*A property group,* which is a set of properties overloaded on the same name.</span></span> <span data-ttu-id="2ada7-122">属性组可能具有相关联的目标表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-122">A property group may have an associated target expression.</span></span>

* <span data-ttu-id="2ada7-123">*属性访问。*</span><span class="sxs-lookup"><span data-stu-id="2ada7-123">*A property access.*</span></span> <span data-ttu-id="2ada7-124">每个属性访问具有关联的类型，即属性的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-124">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="2ada7-125">属性访问可能会有相关联的目标表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-125">A property access may have an associated target expression.</span></span>

* <span data-ttu-id="2ada7-126">*后期绑定访问*表示延迟到运行时的方法或属性访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-126">*A late-bound access,* which represents a method or property access deferred until run-time.</span></span> <span data-ttu-id="2ada7-127">后期绑定访问可能有一个相关联的目标表达式和关联的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="2ada7-127">A late-bound access may have an associated target expression and an associated type argument list.</span></span> <span data-ttu-id="2ada7-128">后期绑定访问的类型始终是`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-128">The type of a late-bound access is always `Object`.</span></span>

* <span data-ttu-id="2ada7-129">*事件的访问。*</span><span class="sxs-lookup"><span data-stu-id="2ada7-129">*An event access.*</span></span> <span data-ttu-id="2ada7-130">每个事件访问具有关联的类型，即该事件的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="2ada7-131">事件访问可能会有相关联的目标表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-131">An event access may have an associated target expression.</span></span> <span data-ttu-id="2ada7-132">事件访问可能显示的第一个参数为`RaiseEvent`， `AddHandler`，和`RemoveHandler`语句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-132">An event access may appear as the first argument of the `RaiseEvent`, `AddHandler`, and `RemoveHandler` statements.</span></span> <span data-ttu-id="2ada7-133">在任何其他上下文中，分类为事件访问的表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>

* <span data-ttu-id="2ada7-134">*数组文本，* 表示尚未确定其类型的数组的初始值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-134">*An array literal,* which represents the initial values of an array whose type has not yet been determined.</span></span>

* <span data-ttu-id="2ada7-135">*Void。*</span><span class="sxs-lookup"><span data-stu-id="2ada7-135">*Void.*</span></span> <span data-ttu-id="2ada7-136">表达式的子例程或无结果与 await 运算符表达式调用时，将发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="2ada7-136">This occurs when the expression is an invocation of a subroutine, or an await operator expression with no result.</span></span> <span data-ttu-id="2ada7-137">归类为 void 的表达式只是在调用语句或 await 语句的上下文中无效。</span><span class="sxs-lookup"><span data-stu-id="2ada7-137">An expression classified as void is only valid in the context of an invocation statement or an await statement.</span></span>

* <span data-ttu-id="2ada7-138">*默认值。*</span><span class="sxs-lookup"><span data-stu-id="2ada7-138">*A default value.*</span></span> <span data-ttu-id="2ada7-139">仅文本`Nothing`生成此分类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-139">Only the literal `Nothing` produces this classification.</span></span>

<span data-ttu-id="2ada7-140">一个表达式的最终结果通常是一个值或具有其他类别的表达式作为仅允许在某些上下文中的中间值的变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-140">The final result of an expression is usually a value or a variable, with the other categories of expressions functioning as intermediate values that are only permitted in certain contexts.</span></span>

<span data-ttu-id="2ada7-141">请注意，可以在语句和需要的要具有某些特征 （如正在引用类型、 值类型、 派生一些类型，等等） 的表达式类型的表达式中使用的类型为类型参数的表达式如果施加约束对类型参数满足这些特征。</span><span class="sxs-lookup"><span data-stu-id="2ada7-141">Note that expressions whose type is a type parameter can be used in statements and expressions that require the type of an expression to have certain characteristics (such as being a reference type, value type, deriving from some type, etc.) if the constraints imposed on the type parameter satisfy those characteristics.</span></span>

### <a name="expression-reclassification"></a><span data-ttu-id="2ada7-142">表达式重新分类</span><span class="sxs-lookup"><span data-stu-id="2ada7-142">Expression Reclassification</span></span>

<span data-ttu-id="2ada7-143">通常情况下，需要从该表达式的不同分类的上下文中使用表达式时，编译时错误发生-例如，尝试将值分配为文本。</span><span class="sxs-lookup"><span data-stu-id="2ada7-143">Normally, when an expression is used in a context that requires a classification different from that of the expression, a compile-time error occurs -- for example, attempting to assign a value to a literal.</span></span> <span data-ttu-id="2ada7-144">但是，在许多情况下就可以更改表达式的分类的过程通过*重新分类*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-144">However, in many cases it is possible to change an expression's classification through the process of *reclassification*.</span></span>

<span data-ttu-id="2ada7-145">如果成功重新分类，然后重新分类以作为扩大或收缩。</span><span class="sxs-lookup"><span data-stu-id="2ada7-145">If reclassification succeeds, then the reclassification is judged as widening or narrowing.</span></span> <span data-ttu-id="2ada7-146">除非另行说明，否则此列表中的所有 reclassifications 扩大都转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-146">Unless otherwise noted, all the reclassifications in this list are widening.</span></span>

<span data-ttu-id="2ada7-147">下列类型的表达式可以重新分类：</span><span class="sxs-lookup"><span data-stu-id="2ada7-147">The following types of expressions can be reclassified:</span></span>

* <span data-ttu-id="2ada7-148">变量可以重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-148">A variable can be reclassified as a value.</span></span> <span data-ttu-id="2ada7-149">将提取存储在变量中的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-149">The value stored in the variable is fetched.</span></span>

* <span data-ttu-id="2ada7-150">方法组可以重新分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-150">A method group can be reclassified as a value.</span></span> <span data-ttu-id="2ada7-151">方法组表达式被解释为具有相关联的目标表达式和类型形参列表和空括号调用表达式 (即`f`被解释为`f()`和`f(Of Integer)`被解释为`f(Of Integer)()`).</span><span class="sxs-lookup"><span data-stu-id="2ada7-151">The method group expression is interpreted as an invocation expression with the associated target expression and type parameter list, and empty parentheses (that is, `f` is interpreted as `f()` and `f(Of Integer)` is interpreted as `f(Of Integer)()`).</span></span> <span data-ttu-id="2ada7-152">此重新分类可能会导致正在进一步重新分类为 void 的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-152">This reclassification may result in the expression being further reclassified as void.</span></span>

* <span data-ttu-id="2ada7-153">方法指针可以重新分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-153">A method pointer can be reclassified as a value.</span></span> <span data-ttu-id="2ada7-154">此重新分类只能发生在知道目标类型的转换的上下文。</span><span class="sxs-lookup"><span data-stu-id="2ada7-154">This reclassification can only occur in the context of a conversion where the target type is known.</span></span> <span data-ttu-id="2ada7-155">方法的指针表达式被解释为具有关联的类型参数列表的相应类型的委托实例化表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-155">The method pointer expression is interpreted as the argument to a delegate instantiation expression of the appropriate type with the associated type argument list.</span></span> <span data-ttu-id="2ada7-156">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-156">For example:</span></span>
    
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

* <span data-ttu-id="2ada7-157">Lambda 方法可以重新分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-157">A lambda method can be reclassified as a value.</span></span> <span data-ttu-id="2ada7-158">如果知道目标类型的转换的上下文中发生重新分类，然后两个 reclassifications 可能会出现之一：</span><span class="sxs-lookup"><span data-stu-id="2ada7-158">If the reclassification occurs in the context of a conversion where the target type is known, then one of two reclassifications can occur:</span></span>
    
  1. <span data-ttu-id="2ada7-159">如果目标类型是委托类型，lambda 方法被解释为相应类型的委托构造表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-159">If the target type is a delegate type, the lambda method is interpreted as the argument to a delegate-construction expression of the appropriate type.</span></span>
    
  2. <span data-ttu-id="2ada7-160">如果目标类型是`System.Linq.Expressions.Expression(Of T)`，并`T`为委托类型，则像委托构造表达式中使用它解释 lambda 方法`T`，然后转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="2ada7-160">If the target type is `System.Linq.Expressions.Expression(Of T)`, and `T` is a delegate type, then the lambda method is interpreted as if it was being used in delegate-construction expression for `T` and then converted to an expression tree.</span></span>
    
  <span data-ttu-id="2ada7-161">如果该委托没有 ByRef 参数，async 或 iterator lambda 方法可能仅被解释为委托构造表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-161">An async or iterator lambda method may only be interpreted as the argument to a delegate-construction expression, if the delegate has no ByRef parameters.</span></span>
    
  <span data-ttu-id="2ada7-162">如果从任何委托的参数类型到相应的 lambda 参数类型的转换是收缩转换，然后重新分类被判定为收缩;否则它为扩大转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-162">If conversion from any of the delegate's parameter types to the corresponding lambda parameter types is a narrowing conversion, then the reclassification is judged as narrowing; otherwise it is widening.</span></span>
    
  <span data-ttu-id="2ada7-163">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-163">__Note.__</span></span> <span data-ttu-id="2ada7-164">Lambda 方法和表达式树之间的确切转换可能不会修复之间版本的编译器和超出了本规范的范畴。</span><span class="sxs-lookup"><span data-stu-id="2ada7-164">The exact translation between lambda methods and expression trees may not be fixed between versions of the compiler and is beyond the scope of this specification.</span></span> <span data-ttu-id="2ada7-165">For Microsoft Visual Basic 11.0，所有的 lambda 表达式可能会转换为表达式树有以下限制：（1) 1。</span><span class="sxs-lookup"><span data-stu-id="2ada7-165">For Microsoft Visual Basic 11.0, all lambda expressions may be converted to expression trees subject to the following restrictions: (1) 1.</span></span>  <span data-ttu-id="2ada7-166">仅单行 lambda 表达式而无需 ByRef 参数可能会转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="2ada7-166">Only single-line lambda expressions without ByRef parameters may be converted to expression trees.</span></span> <span data-ttu-id="2ada7-167">单个行的`Sub`lambda，唯一的调用语句可能会转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="2ada7-167">Of the single-line `Sub` lambdas, only invocation statements may be converted to expression trees.</span></span> <span data-ttu-id="2ada7-168">（如果早期的字段初始值设定项用于初始化的后续字段初始值设定项，例如 2） 匿名类型的表达式不能转换为表达式树`New With {.a=1, .b=.a}`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-168">(2) Anonymous type expressions cannot be converted to expression trees if an earlier field initializer is used to initialize a subsequent field initializer, e.g. `New With {.a=1, .b=.a}`.</span></span> <span data-ttu-id="2ada7-169">（3） 对象初始值设定项表达式无法转换为表达式树如果当前对象进行初始化的成员将用在一个字段初始值设定项，例如`New C1 With {.a=1, .b=.Method1()}`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-169">(3) Object initializer expressions cannot be converted to expression trees if a member of the current object being initialized is used in one of the field initializers, e.g. `New C1 With {.a=1, .b=.Method1()}`.</span></span> <span data-ttu-id="2ada7-170">（4） 多维数组创建表达式只可以转换为表达式树，如果它们显式声明它们的元素类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-170">(4) Multi-dimensional array creation expressions can only be converted to expression trees if they declare their element type explicitly.</span></span> <span data-ttu-id="2ada7-171">（5） Late 绑定表达式无法转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="2ada7-171">(5) Late-binding expressions cannot be converted to expression trees.</span></span> <span data-ttu-id="2ada7-172">（一般 VB 语义 6） 当变量或字段到调用表达式传递 ByRef 但不具有 ByRef 参数的类型完全相同或 ByRef 传递属性时，都将 ByRef 传递自变量的副本，然后复制其最终值 恢复到的变量或字段或属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-172">(6) When a variable or field is passed ByRef to an invocation expression but does not have exactly the same type as the ByRef parameter, or when a property is passed ByRef, normal VB semantics are that a copy of the argument is passed ByRef and its final value is then copied back into the variable or field or property.</span></span> <span data-ttu-id="2ada7-173">在表达式树中，复制后不会发生。</span><span class="sxs-lookup"><span data-stu-id="2ada7-173">In expression trees, the copy-back does not happen.</span></span> <span data-ttu-id="2ada7-174">（7） 所有这些限制适用于以及嵌套的 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-174">(7) All these restrictions apply to nested lambda expressions as well.</span></span>
    
  <span data-ttu-id="2ada7-175">如果不知道目标类型，则 lambda 方法解释为匿名委托类型的委托实例化表达式具有相同签名的 lambda 方法的参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-175">If the target type is not known, then the lambda method is interpreted as the argument to a delegate instantiation expression of an anonymous delegate type with the same signature of the lambda method.</span></span> <span data-ttu-id="2ada7-176">如果正在使用严格的语义，并且会省略任何参数的类型，会发生编译时错误;否则为`Object`替换为任何缺少的参数类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-176">If strict semantics are being used and the type of any of the parameters are omitted, a compile-time error occurs; otherwise, `Object` is substituted for any missing parameter type.</span></span> <span data-ttu-id="2ada7-177">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-177">For example:</span></span>
    
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

* <span data-ttu-id="2ada7-178">属性组可以重新分类为属性访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-178">A property group can be reclassified as a property access.</span></span> <span data-ttu-id="2ada7-179">属性组表达式解释为索引表达式使用空括号 (即`f`被解释为`f()`)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-179">The property group expression is interpreted as an index expression with empty parentheses (that is, `f` is interpreted as `f()`).</span></span>

* <span data-ttu-id="2ada7-180">属性访问可以重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-180">A property access can be reclassified as a value.</span></span> <span data-ttu-id="2ada7-181">属性访问表达式被解释为调用表达式的`Get`属性访问器。</span><span class="sxs-lookup"><span data-stu-id="2ada7-181">The property access expression is interpreted as an invocation expression of the `Get` accessor of the property.</span></span> <span data-ttu-id="2ada7-182">如果属性没有 getter，因此，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-182">If the property has no getter, then a compile-time error occurs.</span></span>

* <span data-ttu-id="2ada7-183">后期绑定访问可以重新分类为后期绑定方法或后期绑定属性访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-183">A late-bound access can be reclassified as a late-bound method or late-bound property access.</span></span> <span data-ttu-id="2ada7-184">在其中后期绑定访问可以重新分类作为方法访问和属性访问的情况下，重新分类到属性访问是首选。</span><span class="sxs-lookup"><span data-stu-id="2ada7-184">In a situation where a late-bound access can be reclassified both as a method access and as a property access, reclassification to a property access is preferred.</span></span>

* <span data-ttu-id="2ada7-185">后期绑定访问可以重新分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-185">A late-bound access can be reclassified as a value.</span></span>

* <span data-ttu-id="2ada7-186">数组文本可以重新分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-186">An array literal can be reclassified as a value.</span></span> <span data-ttu-id="2ada7-187">值的类型确定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-187">The type of the value is determined as follows:</span></span>

  1. <span data-ttu-id="2ada7-188">如果其中的目标类型已知时，目标类型是数组类型的转换的上下文中发生重新分类，然后数组文本是重新分类为 T() 类型的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-188">If the reclassification occurs in the context of a conversion where the target type is known and the target type is an array type, then the array literal is reclassified as a value of type T().</span></span> <span data-ttu-id="2ada7-189">如果目标类型是`System.Collections.Generic.IList(Of T)`， `IReadOnlyList(Of T)`， `ICollection(Of T)`， `IReadOnlyCollection(Of T)`，或`IEnumerable(Of T)`，并且数组文本具有一级嵌套，则数组文本重新分类为值类型为`T()`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-189">If the target type is `System.Collections.Generic.IList(Of T)`, `IReadOnlyList(Of T)`, `ICollection(Of T)`, `IReadOnlyCollection(Of T)`, or `IEnumerable(Of T)`, and the array literal has one level of nesting, then the array literal is reclassified as a value of type `T()`.</span></span>

  2. <span data-ttu-id="2ada7-190">否则，数组文本是重新分类到其类型是数组的秩等于的嵌套级别使用，并且使用初始值设定项; 中的元素的基准类型确定的元素类型的值如果没有基准类型不能确定，`Object`使用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-190">Otherwise, the array literal is reclassified to a value whose type is an array of rank equal to the level of nesting is used, with element type determined by the dominant type of the elements in the initializer; if no dominant type can be determined, `Object` is used.</span></span> <span data-ttu-id="2ada7-191">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-191">For example:</span></span>

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

  <span data-ttu-id="2ada7-192">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-192">__Note.__</span></span> <span data-ttu-id="2ada7-193">没有版本 9.0 和 10.0 版本的语言之间的行为会略有变化。</span><span class="sxs-lookup"><span data-stu-id="2ada7-193">There is a slight change in behavior between version 9.0 and version 10.0 of the language.</span></span> <span data-ttu-id="2ada7-194">低于 10.0，数组元素初始值设定项不会影响本地变量的类型推理，现在它们执行操作。</span><span class="sxs-lookup"><span data-stu-id="2ada7-194">Prior to 10.0, array element initializers did not affect local variable type inference and now they do.</span></span> <span data-ttu-id="2ada7-195">因此`Dim a() = { 1, 2, 3 }`将具有推断`Object()`的类型为`a`中的语言的 9.0 版和`Integer()`版本 10.0。</span><span class="sxs-lookup"><span data-stu-id="2ada7-195">So `Dim a() = { 1, 2, 3 }` would have inferred `Object()` as the type of `a` in version 9.0 of the language and `Integer()` in version 10.0.</span></span>

  <span data-ttu-id="2ada7-196">重新分类然后重新解释数组文本作为数组创建表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-196">The reclassification then reinterprets the array literal as an array-creation expression.</span></span> <span data-ttu-id="2ada7-197">因此这些示例：</span><span class="sxs-lookup"><span data-stu-id="2ada7-197">So the examples:</span></span>

  ```vb
  Dim x As Double = { 1, 2, 3, 4 }
  Dim y = { "a", "b" }
  ```

  <span data-ttu-id="2ada7-198">是到等效的：</span><span class="sxs-lookup"><span data-stu-id="2ada7-198">are equivalent to:</span></span>

  ```vb
  Dim x As Double = New Double() { 1, 2, 3, 4 }
  Dim y = New String() { "a", "b" }
  ```

  <span data-ttu-id="2ada7-199">为收缩从元素表达式为数组元素类型的任何转换为收缩转换; 如果以重新分类否则，它被判定为扩大转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-199">The reclassification is judged as narrowing if any conversion from an element expression to the array element type is narrowing; otherwise it is judged as widening.</span></span>

* <span data-ttu-id="2ada7-200">默认值`Nothing`可以重新分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-200">The default value `Nothing` can be reclassified as a value.</span></span> <span data-ttu-id="2ada7-201">在上下文中的目标类型已知的时，结果是目标类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-201">In a context where the target type is known, the result is the default value of the target type.</span></span> <span data-ttu-id="2ada7-202">在上下文中无法知道目标类型，结果为空值类型的`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-202">In a context where the target type is not known, the result is a null value of type `Object`.</span></span>

<span data-ttu-id="2ada7-203">命名空间表达式、 类型表达式中，事件访问表达式或 void 表达式不能重新分类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-203">A namespace expression, type expression, event access expression, or void expression cannot be reclassified.</span></span> <span data-ttu-id="2ada7-204">可在同一时间完成多个 reclassifications。</span><span class="sxs-lookup"><span data-stu-id="2ada7-204">Multiple reclassifications can be done at the same time.</span></span> <span data-ttu-id="2ada7-205">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-205">For example:</span></span>

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

<span data-ttu-id="2ada7-206">在这种情况下，该属性组表达式`P`属性访问从属性组首先重新分类并为值属性访问从然后重新分类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-206">In this case, the property group expression `P` is first reclassified from a property group to a property access and then reclassified from a property access to a value.</span></span> <span data-ttu-id="2ada7-207">Reclassifications 数最少执行以访问上下文中的有效分类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-207">The fewest number of reclassifications are performed to reach a valid classification in the context.</span></span>

## <a name="constant-expressions"></a><span data-ttu-id="2ada7-208">常量表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-208">Constant Expressions</span></span>

<span data-ttu-id="2ada7-209">一个*常量表达式*是可以在编译时完全计算其值的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-209">A *constant expression* is an expression whose value can be fully evaluated at compile time.</span></span>

```antlr
ConstantExpression
    : Expression
    ;
```

<span data-ttu-id="2ada7-210">常量表达式的类型可以是`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Char`， `Single`， `Double`， `Decimal`， `Date`， `Boolean`， `String`， `Object`，或任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-210">The type of a constant expression can be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Char`, `Single`, `Double`, `Decimal`, `Date`, `Boolean`, `String`, `Object`, or any enumeration type.</span></span> <span data-ttu-id="2ada7-211">常量表达式中允许以下构造：</span><span class="sxs-lookup"><span data-stu-id="2ada7-211">The following constructs are permitted in constant expressions:</span></span>

* <span data-ttu-id="2ada7-212">文本 (包括`Nothing`)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-212">Literals (including `Nothing`).</span></span>

* <span data-ttu-id="2ada7-213">对常量类型成员或常量局部变量的引用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-213">References to constant type members or constant locals.</span></span>

* <span data-ttu-id="2ada7-214">对枚举类型的成员的引用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-214">References to members of enumeration types.</span></span>

* <span data-ttu-id="2ada7-215">带括号的子表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-215">Parenthesized subexpressions.</span></span>

* <span data-ttu-id="2ada7-216">强制转换表达式，提供目标类型是上面列出的类型之一。</span><span class="sxs-lookup"><span data-stu-id="2ada7-216">Coercion expressions, provided the target type is one of the types listed above.</span></span> <span data-ttu-id="2ada7-217">强制来回`String`是此规则的例外，因为只允许对 null 值`String`始终完成转换的执行环境的当前区域性中在运行时。</span><span class="sxs-lookup"><span data-stu-id="2ada7-217">Coercions to and from `String` are an exception to this rule and are only allowed on null values because `String` conversions are always done in the current culture of the execution environment at run time.</span></span> <span data-ttu-id="2ada7-218">请注意，常量强制转换表达式都可以只使用内部函数转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-218">Note that constant coercion expressions can only ever use intrinsic conversions.</span></span>

* <span data-ttu-id="2ada7-219">`+`，`-`和`Not`一元运算符提供操作数和结果是上面列出的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-219">The `+`, `-` and `Not` unary operators, provided the operand and result is of a type listed above.</span></span>

* <span data-ttu-id="2ada7-220">`+`， `-`， `*`， `^`， `Mod`， `/`， `\`， `<<`， `>>`， `&`， `And`， `Or`， `Xor`， `AndAlso`， `OrElse`， `=`， `<`， `>`， `<>`， `<=`，和`=>`二元运算符，提供每个操作数和结果是上面列出的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-220">The `+`, `-`, `*`, `^`, `Mod`, `/`, `\`, `<<`, `>>`, `&`, `And`, `Or`, `Xor`, `AndAlso`, `OrElse`, `=`, `<`, `>`, `<>`, `<=`, and `=>` binary operators, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="2ada7-221">条件运算符如果提供每个操作数和结果是上面列出的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-221">The conditional operator If, provided each operand and result is of a type listed above.</span></span>

* <span data-ttu-id="2ada7-222">以下运行时函数： `Microsoft.VisualBasic.Strings.ChrW`;`Microsoft.VisualBasic.Strings.Chr`如果常量的值介于 0 和 128;`Microsoft.VisualBasic.Strings.AscW`常量字符串是否不为空;`Microsoft.VisualBasic.Strings.Asc`常量字符串是否不为空。</span><span class="sxs-lookup"><span data-stu-id="2ada7-222">The following run-time functions: `Microsoft.VisualBasic.Strings.ChrW`; `Microsoft.VisualBasic.Strings.Chr` if the constant value is between 0 and 128; `Microsoft.VisualBasic.Strings.AscW` if the constant string is not empty; `Microsoft.VisualBasic.Strings.Asc` if the constant string is not empty.</span></span>

<span data-ttu-id="2ada7-223">以下构造都*不*允许在常数表达式中：</span><span class="sxs-lookup"><span data-stu-id="2ada7-223">The following constructs are *not* permitted in constant expressions:</span></span>

* <span data-ttu-id="2ada7-224">通过隐式绑定`With`上下文。</span><span class="sxs-lookup"><span data-stu-id="2ada7-224">Implicit binding through a `With` context.</span></span>

<span data-ttu-id="2ada7-225">一种整型类型的常量表达式 (`ULong`， `Long`， `UInteger`， `Integer`， `UShort`， `Short`， `SByte`，或`Byte`) 可以隐式转换为较窄的整数类型、 和类型的常量表达式`Double`可以隐式转换为`Single`，前提是目标类型的范围内的常量表达式的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-225">Constant expressions of an integral type (`ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `SByte`, or `Byte`) can be implicitly converted to a narrower integral type, and constant expressions of type `Double` can be implicitly converted to `Single`, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="2ada7-226">无论是否使用宽松或严格语义允许这些收缩转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-226">These narrowing conversions are allowed regardless of whether permissive or strict semantics are being used.</span></span>


## <a name="late-bound-expressions"></a><span data-ttu-id="2ada7-227">后期绑定表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-227">Late-Bound Expressions</span></span>

<span data-ttu-id="2ada7-228">成员访问表达式或索引表达式的目标时的类型`Object`，该表达式的处理可能延迟到运行时。</span><span class="sxs-lookup"><span data-stu-id="2ada7-228">When the target of a member access expression or index expression is of type `Object`, the processing of the expression may be deferred until run time.</span></span> <span data-ttu-id="2ada7-229">推迟处理这种方式调用*后期绑定*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-229">Deferring processing this way is called *late binding*.</span></span> <span data-ttu-id="2ada7-230">后期绑定允许`Object`要在中使用的变量*无类型*成员的所有解决方法根据变量中的值的实际运行时类型的方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-230">Late binding allows `Object` variables to be used in a *typeless* way, where all resolution of members is based on the actual run-time type of the value in the variable.</span></span> <span data-ttu-id="2ada7-231">如果通过编译环境或通过指定严格的语义`Option Strict`，后期绑定会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-231">If strict semantics are specified by the compilation environment or by `Option Strict`, late binding causes a compile-time error.</span></span> <span data-ttu-id="2ada7-232">执行后期绑定，包括以重载决策的目的时，将忽略非公共成员。</span><span class="sxs-lookup"><span data-stu-id="2ada7-232">Non-public members are ignored when doing late-binding, including for the purposes of overload resolution.</span></span> <span data-ttu-id="2ada7-233">请注意，与早期绑定的情况下，调用或访问不同`Shared`后期绑定成员将导致调用目标，以在运行时计算。</span><span class="sxs-lookup"><span data-stu-id="2ada7-233">Note that, unlike the early-bound case, invoking or accessing a `Shared` member late-bound will cause the invocation target to be evaluated at run time.</span></span><span data-ttu-id="2ada7-234"> 如果表达式是定义上的成员的调用表达式`System.Object\`，后期绑定将不会发生。</span><span class="sxs-lookup"><span data-stu-id="2ada7-234"> If the expression is an invocation expression for a member defined on `System.Object\`, late binding will not take place.</span></span>

<span data-ttu-id="2ada7-235">一般情况下，后期绑定访问的解析在运行时查找该表达式的实际运行时类型中的标识符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-235">In general, late-bound accesses are resolved at run time by looking up the identifier on the actual run-time type of the expression.</span></span> <span data-ttu-id="2ada7-236">如果在运行时，后期绑定成员查找失败`System.MissingMemberException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-236">If late-bound member lookup fails at run time, a `System.MissingMemberException` exception is thrown.</span></span> <span data-ttu-id="2ada7-237">因为执行后期绑定成员查找仅关闭关联的目标表达式的运行时类型，对象的运行时类型永远不会是一个接口。</span><span class="sxs-lookup"><span data-stu-id="2ada7-237">Because late-bound member lookup is done solely off the run-time type of the associated target expression, an object's run-time type is never an interface.</span></span> <span data-ttu-id="2ada7-238">因此，就无法访问后期绑定成员访问表达式中的接口成员。</span><span class="sxs-lookup"><span data-stu-id="2ada7-238">Therefore, it is impossible to access interface members in a late-bound member access expression.</span></span>

<span data-ttu-id="2ada7-239">它们在成员访问表达式中出现的顺序计算这些实参后期绑定成员访问： 不在后期绑定成员声明参数的顺序。</span><span class="sxs-lookup"><span data-stu-id="2ada7-239">The arguments to a late-bound member access are evaluated in the order they appear in the member access expression: not the order in which parameters are declared in the late-bound member.</span></span> <span data-ttu-id="2ada7-240">下面的示例说明了这种差异：</span><span class="sxs-lookup"><span data-stu-id="2ada7-240">The following example illustrates this difference:</span></span>

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

<span data-ttu-id="2ada7-241">此代码将显示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-241">This code displays:</span></span>

```
Early-bound: xy
Late-bound: yx
```

<span data-ttu-id="2ada7-242">因为后期绑定重载解决方法在运行时类型的参数来完成，就可以表达式可能会产生不同的结果基于它是否将在编译时或运行的时进行计算。</span><span class="sxs-lookup"><span data-stu-id="2ada7-242">Because late-bound overload resolution is done on the run-time type of the arguments, it is possible that an expression might produce different results based on whether it is evaluated at compile time or run time.</span></span> <span data-ttu-id="2ada7-243">下面的示例说明了这种差异：</span><span class="sxs-lookup"><span data-stu-id="2ada7-243">The following example illustrates this difference:</span></span>

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

<span data-ttu-id="2ada7-244">此代码将显示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-244">This code displays:</span></span>

```
F(Base)
F(Derived)
```

## <a name="simple-expressions"></a><span data-ttu-id="2ada7-245">简单表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-245">Simple Expressions</span></span>

<span data-ttu-id="2ada7-246">简单表达式是文本、 带括号的表达式、 实例表达式或简单名称表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-246">Simple expressions are literals, parenthesized expressions, instance expressions, or simple name expressions.</span></span>

```antlr
SimpleExpression
    : LiteralExpression
    | ParenthesizedExpression
    | InstanceExpression
    | SimpleNameExpression
    | AddressOfExpression
    ;
```

### <a name="literal-expressions"></a><span data-ttu-id="2ada7-247">文本表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-247">Literal Expressions</span></span>

<span data-ttu-id="2ada7-248">文本表达式的计算结果为文本所表示的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-248">Literal expressions evaluate to the value represented by the literal.</span></span> <span data-ttu-id="2ada7-249">文本表达式分类为一个值，除了文本`Nothing`，其分类为默认值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-249">A literal expression is classified as a value, except for the literal `Nothing`, which is classified as a default value.</span></span>

```antlr
LiteralExpression
    : Literal
    ;
```

### <a name="parenthesized-expressions"></a><span data-ttu-id="2ada7-250">带括号的表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-250">Parenthesized Expressions</span></span>

<span data-ttu-id="2ada7-251">带括号的表达式包含一个表达式括在括号中。</span><span class="sxs-lookup"><span data-stu-id="2ada7-251">A parenthesized expression consists of an expression enclosed in parentheses.</span></span> <span data-ttu-id="2ada7-252">带括号的表达式分类为一个值，并带括号的表达式必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-252">A parenthesized expression is classified as a value, and the enclosed expression must be classified as a value.</span></span> <span data-ttu-id="2ada7-253">带括号的表达式计算结果为在括号中的表达式的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-253">A parenthesized expression evaluates to the value of the expression within the parentheses.</span></span>

```antlr
ParenthesizedExpression
    : OpenParenthesis Expression CloseParenthesis
    ;
```

### <a name="instance-expressions"></a><span data-ttu-id="2ada7-254">实例表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-254">Instance Expressions</span></span>

<span data-ttu-id="2ada7-255">*实例表达式*关键字`Me`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-255">An *instance expression* is the keyword `Me`.</span></span> <span data-ttu-id="2ada7-256">它可能只是在正文中使用的非共享方法、 构造函数或属性访问器。</span><span class="sxs-lookup"><span data-stu-id="2ada7-256">It may only be used within the body of a non-shared method, constructor, or property accessor.</span></span> <span data-ttu-id="2ada7-257">它被分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-257">It is classified as a value.</span></span> <span data-ttu-id="2ada7-258">关键字`Me`表示包含正在执行的方法或属性访问器的类型的实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-258">The keyword `Me` represents the instance of the type containing the method or property accessor being executed.</span></span> <span data-ttu-id="2ada7-259">如果一个构造函数将显式调用另一个构造函数 (部分[构造函数](type-members.md#constructors))，`Me`不能使用直到该构造函数调用后，因为未尚未构造实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-259">If a constructor explicitly invokes another constructor (Section [Constructors](type-members.md#constructors)), `Me` cannot be used until after that constructor call, because the instance has not yet been constructed.</span></span>

```antlr
InstanceExpression
    : 'Me'
    ;
```

### <a name="simple-name-expressions"></a><span data-ttu-id="2ada7-260">简单名称表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-260">Simple Name Expressions</span></span>

<span data-ttu-id="2ada7-261">一个*的简单名称表达式*包含单个标识符后跟可选类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="2ada7-261">A *simple name expression* consists of a single identifier followed by an optional type argument list.</span></span>

```antlr
SimpleNameExpression
    : Identifier ( OpenParenthesis 'Of' TypeArgumentList CloseParenthesis )?
    ;
```

<span data-ttu-id="2ada7-262">名称是已解决，并且根据以下"简单名称解析规则"进行了分类：</span><span class="sxs-lookup"><span data-stu-id="2ada7-262">The name is resolved and classified by the following "simple name resolution rules":</span></span>

1.  <span data-ttu-id="2ada7-263">开头立即封闭块并继续 （如果有） 每个封闭外部块，如果标识符匹配的本地变量、 静态变量、 常量局部变量的名称，方法类型参数或参数，则标识符引用匹配的实体。</span><span class="sxs-lookup"><span data-stu-id="2ada7-263">Starting with the immediately enclosing block and continuing with each enclosing outer block (if any), if the identifier matches the name of a local variable, static variable, constant local, method type parameter, or parameter, then the identifier refers to the matching entity.</span></span>

    <span data-ttu-id="2ada7-264">如果标识符匹配的本地变量、 静态变量或常量局部变量并提供类型实参列表时，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-264">If the identifier matches a local variable, static variable, or constant local and a type argument list was provided, a compile-time error occurs.</span></span> <span data-ttu-id="2ada7-265">如果标识符与方法类型参数相匹配，未提供任何类型实参列表，没有出现匹配项，并解析继续运行。</span><span class="sxs-lookup"><span data-stu-id="2ada7-265">If the identifier matches a method type parameter and a type argument list was provided, no match occurs and resolution continues.</span></span> <span data-ttu-id="2ada7-266">如果标识符匹配的本地变量，匹配的本地变量是隐式函数或`Get`访问器返回本地变量和表达式是调用表达式，调用语句的一部分或`AddressOf`然后表达式没有出现匹配项，并且解决将继续。</span><span class="sxs-lookup"><span data-stu-id="2ada7-266">If the identifier matches a local variable, the local variable matched is the implicit function or `Get` accessor return local variable, and the expression is part of an invocation expression, invocation statement, or an `AddressOf` expression, then no match occurs and resolution continues.</span></span>

    <span data-ttu-id="2ada7-267">表达式分类为变量，如果它是本地变量、 静态变量或参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-267">The expression is classified as a variable if it is a local variable, static variable, or parameter.</span></span> <span data-ttu-id="2ada7-268">表达式分类为一个类型，如果它是方法的类型参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-268">The expression is classified as a type if it is a method type parameter.</span></span> <span data-ttu-id="2ada7-269">表达式分类为一个值，如果它是常量局部变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-269">The expression is classified as a value if it is a constant local.</span></span>

2.  <span data-ttu-id="2ada7-270">对于每个包含的表达式的嵌套类型，从最里层开始并转到最外面，如果类型中的标识符的查找会生成与可访问的成员匹配项：</span><span class="sxs-lookup"><span data-stu-id="2ada7-270">For each nested type containing the expression, starting from the innermost and going to the outermost, if a lookup of the identifier in the type produces a match with an accessible member:</span></span>

    21. <span data-ttu-id="2ada7-271">如果匹配的类型成员是类型参数，结果分类为一个类型，并且是匹配的类型参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-271">If the matching type member is a type parameter, then the result is classified as a type and is the matching type parameter.</span></span> <span data-ttu-id="2ada7-272">如果未提供类型实参列表，没有出现匹配项，并解析继续运行。</span><span class="sxs-lookup"><span data-stu-id="2ada7-272">If a type argument list was provided, no match occurs and resolution continues.</span></span>
    22. <span data-ttu-id="2ada7-273">否则，如果该类型是最近的封闭类型，并查找标识出非共享类型成员，则结果是与成员访问窗体的相同`Me.E(Of A)`，其中`E`标识符和`A`是类型参数列表如果有的话。</span><span class="sxs-lookup"><span data-stu-id="2ada7-273">Otherwise, if the type is the immediately enclosing type and the lookup identifies a non-shared type member, then the result is the same as a member access of the form `Me.E(Of A)`, where `E` is the identifier and `A` is the type argument list, if any.</span></span>
    23. <span data-ttu-id="2ada7-274">否则，结果应完全相同的窗体成员访问`T.E(Of A)`，其中`T`是包含匹配的成员，类型`E`的标识符和`A`是类型参数列表中，如果有的话。</span><span class="sxs-lookup"><span data-stu-id="2ada7-274">Otherwise, the result is exactly the same as a member access of the form `T.E(Of A)`, where `T` is the type containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="2ada7-275">在这种情况下，它是要引用的非共享成员的标识符为错误的。</span><span class="sxs-lookup"><span data-stu-id="2ada7-275">In this case, it is an error for the identifier to refer to a non-shared member.</span></span>

3.  <span data-ttu-id="2ada7-276">对于每个嵌套命名空间，从最里层开始，然后转到最外层命名空间，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="2ada7-276">For each nested namespace, starting from the innermost and going to the outermost namespace, do the following:</span></span>

    31. <span data-ttu-id="2ada7-277">如果命名空间包含具有给定名称的可访问类型，并且具有相同数量的类型参数，如已提供在类型参数列表中，如果有，则标识符引用该类型和分类为一个类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-277">If the namespace contains an accessible type with the given name and has the same number of type parameters as was supplied in the type argument list, if any, then the identifier refers to that type and is classified as a type.</span></span>
    32. <span data-ttu-id="2ada7-278">否则为如果已提供任何类型参数列表，该命名空间包含具有给定名称的命名空间成员，然后该标识符引用该命名空间，分类为一个命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-278">Otherwise, if no type argument list was supplied and the namespace contains a namespace member with the given name, then the identifier refers to that namespace and is classified as a namespace.</span></span>
    33. <span data-ttu-id="2ada7-279">否则，如果该命名空间包含一个或多个可访问标准模块，并且成员名称查找的标识符会生成一个标准模块中的可访问的匹配项，则结果是完全与窗体的成员访问相同`M.E(Of A)`，其中`M`是包含匹配的成员的标准模块`E`的标识符和`A`是类型参数列表中，如果有的话。</span><span class="sxs-lookup"><span data-stu-id="2ada7-279">Otherwise, if the namespace contains one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="2ada7-280">如果标识符匹配多个标准模块中的可访问类型成员，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-280">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

4.  <span data-ttu-id="2ada7-281">如果源文件具有一个或多个导入别名，并且标识符匹配的其中一个名称，然后标识符是指为该命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-281">If the source file has one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="2ada7-282">如果提供类型实参列表，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-282">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="2ada7-283">如果包含名称引用的源文件具有一个或多个导入：</span><span class="sxs-lookup"><span data-stu-id="2ada7-283">If the source file containing the name reference has one or more imports:</span></span>

    51. <span data-ttu-id="2ada7-284">如果标识符匹配中只有一个导入具有相同数量的类型参数的可访问类型的名称，如提供中类型参数列表中，如果有或类型成员，则标识符是指该类型或类型成员。</span><span class="sxs-lookup"><span data-stu-id="2ada7-284">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="2ada7-285">如果标识符匹配多个导入中具有相同数量的类型参数的可访问类型的名称作为已提供在类型参数列表中，如果有，或一个可访问类型成员，编译时错误时发生。</span><span class="sxs-lookup"><span data-stu-id="2ada7-285">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or an accessible type member, a compile-time error occurs.</span></span>
    52. <span data-ttu-id="2ada7-286">否则，如果不提供任何类型参数列表和一个导入具有可访问类型的命名空间的名称与标识符匹配，然后标识符引用该命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-286">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="2ada7-287">如果不提供任何类型参数列表和多个导入具有可访问类型的命名空间的名称与标识符匹配，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-287">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    53. <span data-ttu-id="2ada7-288">否则，如果导入包含一个或多个可访问标准模块，并且成员名称查找的标识符会生成一个标准模块中的可访问的匹配项，则结果是完全与窗体的成员访问相同`M.E(Of A)`，其中`M`是包含匹配的成员的标准模块`E`的标识符和`A`是类型参数列表中，如果有的话。</span><span class="sxs-lookup"><span data-stu-id="2ada7-288">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member, `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="2ada7-289">如果标识符匹配多个标准模块中的可访问类型成员，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-289">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

6.  <span data-ttu-id="2ada7-290">如果编译环境定义了一个或多个导入别名，并标识符匹配的其中一个名称，然后标识符引用该命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-290">If the compilation environment defines one or more import aliases, and the identifier matches the name of one of them, then the identifier refers to that namespace or type.</span></span> <span data-ttu-id="2ada7-291">如果提供类型实参列表，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-291">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="2ada7-292">如果编译环境定义了一个或多个导入：</span><span class="sxs-lookup"><span data-stu-id="2ada7-292">If the compilation environment defines one or more imports:</span></span>

    71. <span data-ttu-id="2ada7-293">如果标识符匹配中只有一个导入具有相同数量的类型参数的可访问类型的名称，如提供中类型参数列表中，如果有或类型成员，则标识符是指该类型或类型成员。</span><span class="sxs-lookup"><span data-stu-id="2ada7-293">If the identifier matches in exactly one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, then the identifier refers to that type or type member.</span></span> <span data-ttu-id="2ada7-294">如果标识符匹配多个导入中具有相同数量的类型参数的可访问类型的名称作为已提供在类型参数列表中，如果有，或类型成员，编译时错误时发生。</span><span class="sxs-lookup"><span data-stu-id="2ada7-294">If the identifier matches in more than one import the name of an accessible type with the same number of type parameters as was supplied in the type argument list, if any, or a type member, a compile-time error occurs.</span></span>
    72. <span data-ttu-id="2ada7-295">否则，如果不提供任何类型参数列表和一个导入具有可访问类型的命名空间的名称与标识符匹配，然后标识符引用该命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-295">Otherwise, if no type argument list was supplied and the identifier matches in exactly one import the name of a namespace with accessible types, then the identifier refers to that namespace.</span></span> <span data-ttu-id="2ada7-296">如果不提供任何类型参数列表和多个导入具有可访问类型的命名空间的名称与标识符匹配，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-296">If no type argument list was supplied and the identifier matches in more than one import the name of a namespace with accessible types, a compile-time error occurs.</span></span>
    73. <span data-ttu-id="2ada7-297">否则，如果导入包含一个或多个可访问标准模块，并且成员名称查找的标识符会生成一个标准模块中的可访问的匹配项，则结果是完全与窗体的成员访问相同`M.E(Of A)`，其中`M`是包含匹配的成员的标准模块`E`的标识符和`A`是类型参数列表中，如果有的话。</span><span class="sxs-lookup"><span data-stu-id="2ada7-297">Otherwise, if the imports contain one or more accessible standard modules, and a member name lookup of the identifier produces an accessible match in exactly one standard module, then the result is exactly the same as a member access of the form `M.E(Of A)`, where `M` is the standard module containing the matching member,  `E` is the identifier, and `A` is the type argument list, if any.</span></span> <span data-ttu-id="2ada7-298">如果标识符匹配多个标准模块中的可访问类型成员，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-298">If the identifier matches accessible type members in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="2ada7-299">否则，给定标识符的名称是不确定的。</span><span class="sxs-lookup"><span data-stu-id="2ada7-299">Otherwise, the name given by the identifier is undefined.</span></span>

<span data-ttu-id="2ada7-300">未定义的简单名称表达式是一个编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-300">A simple name expression that is undefined is a compile-time error.</span></span>

<span data-ttu-id="2ada7-301">通常情况下，名称可仅出现一次在特定的命名空间中。</span><span class="sxs-lookup"><span data-stu-id="2ada7-301">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="2ada7-302">但是，可以跨多个.NET 程序集声明命名空间，因此很可能有两个程序集在其中定义具有相同的完全限定名称的类型的情况。</span><span class="sxs-lookup"><span data-stu-id="2ada7-302">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="2ada7-303">在这种情况下，在当前组源代码文件中声明的类型优于外部.NET 程序集中声明的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-303">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="2ada7-304">否则为名称不明确，则不能消除名称歧义。</span><span class="sxs-lookup"><span data-stu-id="2ada7-304">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>


### <a name="addressof-expressions"></a><span data-ttu-id="2ada7-305">AddressOf 表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-305">AddressOf Expressions</span></span>

<span data-ttu-id="2ada7-306">`AddressOf`表达式用于生成方法的指针。</span><span class="sxs-lookup"><span data-stu-id="2ada7-306">An `AddressOf` expression is used to produce a method pointer.</span></span> <span data-ttu-id="2ada7-307">表达式组成`AddressOf`关键字和必须归类为方法组或后期绑定访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-307">The expression consists of the `AddressOf` keyword and an expression that must be classified as a method group or a late-bound access.</span></span> <span data-ttu-id="2ada7-308">构造函数不能引用方法组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-308">The method group cannot refer to constructors.</span></span>

<span data-ttu-id="2ada7-309">结果被归类为方法指针，使用相同的关联的目标表达式和类型参数列表 （如果有） 为方法组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-309">The result is classified as a method pointer, with the same associated target expression and type argument list (if any) as the method group.</span></span>

```antlr
AddressOfExpression
    : 'AddressOf' Expression
    ;
```

## <a name="type-expressions"></a><span data-ttu-id="2ada7-310">类型表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-310">Type Expressions</span></span>

<span data-ttu-id="2ada7-311">一个*键入表达式*是`GetType`表达式`TypeOf...Is`表达式`Is`表达式，或`GetXmlNamespace`表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-311">A *type expression* is a `GetType` expression, a `TypeOf...Is` expression, an `Is` expression, or a `GetXmlNamespace` expression.</span></span>

```antlr
TypeExpression
    : GetTypeExpression
    | TypeOfIsExpression
    | IsExpression
    | GetXmlNamespaceExpression
    ;
```

### <a name="gettype-expressions"></a><span data-ttu-id="2ada7-312">GetType 表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-312">GetType Expressions</span></span>

<span data-ttu-id="2ada7-313">一个`GetType`表达式包含关键字的`GetType`和类型的名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-313">A `GetType` expression consists of the keyword `GetType` and the name of a type.</span></span>

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

<span data-ttu-id="2ada7-314">一个`GetType`表达式分类为一个值，并且其值为反射 (`System.Type`) 表示的类及其*GetTypeTypeName*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-314">A `GetType` expression is classified as a value, and its value is the reflection (`System.Type`) class that represents its *GetTypeTypeName*.</span></span> <span data-ttu-id="2ada7-315">如果*GetTypeTypeName*是一个类型参数，该表达式将返回`System.Type`对应于为在运行时类型形参提供类型参数的对象。</span><span class="sxs-lookup"><span data-stu-id="2ada7-315">If the *GetTypeTypeName* is a type parameter, the expression will return the `System.Type` object that corresponds to the type argument supplied for the type parameter at run-time.</span></span>

<span data-ttu-id="2ada7-316">*GetTypeTypeName*是特殊两种方式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-316">The *GetTypeTypeName* is special in two ways:</span></span>

* <span data-ttu-id="2ada7-317">它是否可为`System.Void`，可能会引用此类型名称的位置在语言中的唯一位置。</span><span class="sxs-lookup"><span data-stu-id="2ada7-317">It is allowed to be `System.Void`, the only place in the language where this type name may be referenced.</span></span>

* <span data-ttu-id="2ada7-318">它可能具有省略的类型参数的构造泛型类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-318">It may be a constructed generic type with the type arguments omitted.</span></span> <span data-ttu-id="2ada7-319">这允许`GetType`表达式以返回`System.Type`对应于泛型类型本身的对象。</span><span class="sxs-lookup"><span data-stu-id="2ada7-319">This allows the `GetType` expression to return the `System.Type` object that corresponds to the generic type itself.</span></span>

<span data-ttu-id="2ada7-320">下面的示例演示`GetType`表达式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-320">The following example demonstrates the `GetType` expression:</span></span>

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

<span data-ttu-id="2ada7-321">生成的输出是：</span><span class="sxs-lookup"><span data-stu-id="2ada7-321">The resulting output is:</span></span>

```
Int32
Int32
String
Double[]
```


### <a name="typeofis-expressions"></a><span data-ttu-id="2ada7-322">TypeOf...为表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-322">TypeOf...Is Expressions</span></span>

<span data-ttu-id="2ada7-323">一个`TypeOf...Is`表达式用于检查一个值的运行时类型是否与给定类型兼容。</span><span class="sxs-lookup"><span data-stu-id="2ada7-323">A `TypeOf...Is` expression is used to check whether the run-time type of a value is compatible with a given type.</span></span> <span data-ttu-id="2ada7-324">第一个操作数必须分类为一个值，不能重新分类的 lambda 方法和必须是引用类型或不受约束的类型参数类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-324">The first operand must be classified as a value, cannot be a reclassified lambda method, and must be of a reference type or an unconstrained type parameter type.</span></span> <span data-ttu-id="2ada7-325">第二个操作数必须是类型名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-325">The second operand must be a type name.</span></span> <span data-ttu-id="2ada7-326">表达式的结果分类为一个值，并且是`Boolean`值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-326">The result of the expression is classified as a value and is a `Boolean` value.</span></span> <span data-ttu-id="2ada7-327">表达式的计算结果`True`操作数的运行时类型标识、 默认值、 引用、 数组、 值类型或类型参数转换为类型，如果`False`否则为。</span><span class="sxs-lookup"><span data-stu-id="2ada7-327">The expression evaluates to `True` if the run-time type of the operand has an identity, default, reference, array, value type, or type parameter conversion to the type, `False` otherwise.</span></span> <span data-ttu-id="2ada7-328">如果不存在转换表达式的类型和特定类型之间，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-328">A compile-time error occurs if no conversion exists between the type of the expression and the specific type.</span></span>

```antlr
TypeOfIsExpression
    : 'TypeOf' Expression 'Is' LineTerminator? TypeName
    ;
```

### <a name="is-expressions"></a><span data-ttu-id="2ada7-329">为表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-329">Is Expressions</span></span>

<span data-ttu-id="2ada7-330">`Is`或`IsNot`表达式用于执行引用相等性比较。</span><span class="sxs-lookup"><span data-stu-id="2ada7-330">An `Is` or `IsNot` expression is used to do a reference equality comparison.</span></span>

```antlr
IsExpression
    : Expression 'Is' LineTerminator? Expression
    | Expression 'IsNot' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-331">每个表达式必须分类为一个值，每个表达式的类型必须是引用类型、 不受约束的类型参数类型或为空值类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-331">Each expression must be classified as a value and the type of each expression must be a reference type, an unconstrained type parameter type, or a nullable value type.</span></span> <span data-ttu-id="2ada7-332">如果一个表达式的类型是不受约束的类型参数类型或可以为 null 值类型，但是，则另一个表达式必须文字`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-332">If the type of one expression is an unconstrained type parameter type or nullable value type, however, the other expression must be the literal `Nothing`.</span></span>

<span data-ttu-id="2ada7-333">结果分类为一个值，并被类型化为`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-333">The result is classified as a value and is typed as `Boolean`.</span></span> <span data-ttu-id="2ada7-334">`Is`运算计算结果为`True`如果这两个值引用的相同实例或两个值均`Nothing`，或`False`否则为。</span><span class="sxs-lookup"><span data-stu-id="2ada7-334">An `Is` operation evaluates to `True` if both values refer to the same instance or both values are `Nothing`, or `False` otherwise.</span></span> <span data-ttu-id="2ada7-335">`IsNot`运算计算结果为`False`如果这两个值引用的相同实例或两个值均`Nothing`，或`True`否则为。</span><span class="sxs-lookup"><span data-stu-id="2ada7-335">An `IsNot` operation evaluates to `False` if both values refer to the same instance or both values are `Nothing`, or `True` otherwise.</span></span>


### <a name="getxmlnamespace-expressions"></a><span data-ttu-id="2ada7-336">GetXmlNamespace 表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-336">GetXmlNamespace Expressions</span></span>

<span data-ttu-id="2ada7-337">一个`GetXmlNamespace`表达式包含关键字的`GetXmlNamespace`和 XML 命名空间声明的源文件或编译环境的名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-337">A `GetXmlNamespace` expression consists of the keyword `GetXmlNamespace` and the name of an XML namespace declared by the source file or compilation environment.</span></span>

```antlr
GetXmlNamespaceExpression
    : 'GetXmlNamespace' OpenParenthesis XMLNamespaceName? CloseParenthesis
    ;
```

<span data-ttu-id="2ada7-338">`GetXmlNamespace`表达式分类为一个值，且其值的实例`System.Xml.Linq.XNamespace`，它表示*XMLNamespaceName*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-338">An `GetXmlNamespace` expression is classified as a value, and its value is an instance of `System.Xml.Linq.XNamespace` that represents the *XMLNamespaceName*.</span></span> <span data-ttu-id="2ada7-339">如果该类型不可用，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-339">If that type is not available, then a compile-time error will occur.</span></span>

<span data-ttu-id="2ada7-340">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-340">For example:</span></span>

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

<span data-ttu-id="2ada7-341">括号之间的所有内容被认为命名空间名称的一部分，因此 XML 规则周围的空格等应用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-341">Everything between the parentheses is considered part of the namespace name, so XML rules around things such as whitespace apply.</span></span> <span data-ttu-id="2ada7-342">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-342">For example:</span></span>

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

<span data-ttu-id="2ada7-343">XML 命名空间表达式也会省略，则表达式在此情况下返回该对象表示的默认 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-343">The XML namespace expression can also be omitted, in which case the expression returns the object that represents the default XML namespace.</span></span>


## <a name="member-access-expressions"></a><span data-ttu-id="2ada7-344">成员访问表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-344">Member Access Expressions</span></span>

<span data-ttu-id="2ada7-345">成员访问表达式用于访问实体的成员。</span><span class="sxs-lookup"><span data-stu-id="2ada7-345">A member access expression is used to access a member of an entity.</span></span>

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

<span data-ttu-id="2ada7-346">成员访问窗体`E.I(Of A)`，其中`E`是一个表达式，非数组类型名称，该关键字`Global`，或省略和`I`是可选的类型参数列表的标识符`A`、 计算和分类按如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-346">A member access of the form `E.I(Of A)`, where `E` is an expression, a non-array type name, the keyword `Global`, or omitted and `I` is an identifier with an optional type argument list `A`, is evaluated and classified as follows:</span></span>

1. <span data-ttu-id="2ada7-347">如果`E`省略，然后立即包含中的表达式`With`语句替换为`E`和执行成员访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-347">If `E` is omitted, then the expression from the immediately containing `With` statement is substituted for `E` and the member access is performed.</span></span> <span data-ttu-id="2ada7-348">如果没有包含`With`语句中，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-348">If there is no containing `With` statement, a compile-time error occurs.</span></span>

2. <span data-ttu-id="2ada7-349">如果`E`归类为命名空间或`E`关键字`Global`，然后在指定的命名空间的上下文中执行成员查找。</span><span class="sxs-lookup"><span data-stu-id="2ada7-349">If `E` is classified as a namespace or `E` is the keyword `Global`, then the member lookup is done in the context of the specified namespace.</span></span> <span data-ttu-id="2ada7-350">如果`I`中提供类型参数列表中，如果有，则结果为该成员是该命名空间具有相同数量的类型参数的可访问成员的名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-350">If `I` is the name of an accessible member of that namespace with the same number of type parameters as was supplied in the type argument list, if any, then the result is that member.</span></span> <span data-ttu-id="2ada7-351">结果被归类为命名空间或具体取决于该成员的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-351">The result is classified as a namespace or a type depending on the member.</span></span> <span data-ttu-id="2ada7-352">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-352">Otherwise, a compile-time error occurs.</span></span>

3. <span data-ttu-id="2ada7-353">如果`E`为的类型或属于一个类型的表达式，则在指定类型的上下文中执行成员查找。</span><span class="sxs-lookup"><span data-stu-id="2ada7-353">If `E` is a type or an expression classified as a type, then the member lookup is done in the context of the specified type.</span></span> <span data-ttu-id="2ada7-354">如果`I`是可访问成员的名称`E`，然后`E.I`计算和分类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-354">If `I` is the name of an accessible member of `E`, then `E.I` is evaluated and classified as follows:</span></span>

    31. <span data-ttu-id="2ada7-355">如果`I`是关键字`New`和`E`不是一个枚举，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-355">If `I` is the keyword `New` and `E` is not an enumeration then a compile-time error occurs.</span></span>
    32. <span data-ttu-id="2ada7-356">如果`I`已提供在类型参数列表中，如果有，则结果为该类型具有相同数量的类型参数标识的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-356">If `I` identifies a type with the same number of type parameters as was supplied in the type argument list, if any, then the result is that type.</span></span>
    33. <span data-ttu-id="2ada7-357">如果`I`标识一个或多个方法，则结果为具有关联的类型参数列表而不是相关联的目标表达式的方法组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-357">If `I` identifies one or more methods, then the result is a method group with the associated type argument list and no associated target expression.</span></span>
    34. <span data-ttu-id="2ada7-358">如果`I`标识一个或多个属性和任何类型提供的参数列表，则结果为无关联的目标表达式的属性组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-358">If `I` identifies one or more properties and no type argument list was supplied, then the result is a property group with no associated target expression.</span></span>
    35. <span data-ttu-id="2ada7-359">如果`I`标识共享的变量和任何类型提供的参数列表，则结果为变量或值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-359">If `I` identifies a shared variable and no type argument list was supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="2ada7-360">如果变量是只读的并且引用发生的外部共享的构造函数的声明该变量的类型，则结果为共享变量的值`I`在`E`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-360">If the variable is read-only, and the reference occurs outside the shared constructor of the type in which the variable is declared, then the result is the value of the shared variable `I` in `E`.</span></span> <span data-ttu-id="2ada7-361">否则，结果是共享的变量`I`在`E`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-361">Otherwise, the result is the shared variable `I` in `E`.</span></span>
    36. <span data-ttu-id="2ada7-362">如果`I`标识共享的事件和任何类型提供的参数列表，则结果为无关联的目标表达式的事件访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-362">If `I` identifies a shared event and no type argument list was supplied, the result is an event access with no associated target expression.</span></span>
    37. <span data-ttu-id="2ada7-363">如果`I`标识一个常量和任何类型提供的参数列表，则结果为该常量的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-363">If `I` identifies a constant and no type argument list was supplied, then the result is the value of that constant.</span></span>
    38. <span data-ttu-id="2ada7-364">如果`I`标识枚举成员和任何类型提供的参数列表，则结果为该枚举成员的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-364">If `I` identifies an enumeration member and no type argument list was supplied, then the result is the value of that enumeration member.</span></span>
    39. <span data-ttu-id="2ada7-365">否则为`E.I`是无效的成员引用，并且将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-365">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

4. <span data-ttu-id="2ada7-366">如果`E`归类为变量或值的类型是`T`，然后在上下文中执行成员查找`T`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-366">If `E` is classified as a variable or value, the type of which is `T`, then the member lookup is done in the context of `T`.</span></span> <span data-ttu-id="2ada7-367">如果`I`是可访问成员的名称`T`，然后`E.I`计算和分类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-367">If `I` is the name of an accessible member of `T`, then `E.I` is evaluated and classified as follows:</span></span>

    41. <span data-ttu-id="2ada7-368">如果`I`是关键字`New`，`E`是`Me`， `MyBase`，或`MyClass`，并提供没有类型参数，则结果为表示的类型的实例构造函数的方法组`E`相关联的目标表达式`E`和任何类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="2ada7-368">If `I` is the keyword `New`, `E` is  `Me`, `MyBase`, or `MyClass`, and no type arguments were supplied, then the result is a method group representing the instance constructors of the type of `E` with an associated target expression of `E` and no type argument list.</span></span> <span data-ttu-id="2ada7-369">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-369">Otherwise, a compile-time error occurs.</span></span>
    42. <span data-ttu-id="2ada7-370">如果`I`标识一个或多个方法，包括扩展方法，如果`T`不是`Object`，则结果为方法组具有关联的类型参数列表和相关联的目标表达式的`E`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-370">If `I` identifies one or more methods, including extension methods if `T` is not `Object`, then the result is a method group with the associated type argument list and an associated target expression of `E`.</span></span>
    43. <span data-ttu-id="2ada7-371">如果`I`标识一个或多个属性和任何类型提供参数，则结果为包含的相关联的目标表达式的属性组`E`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-371">If `I` identifies one or more properties and no type arguments were supplied, then the result is a property group with an associated target expression of `E`.</span></span>
    44. <span data-ttu-id="2ada7-372">如果`I`标识共享的变量或一个实例变量，并且提供参数，没有类型，则结果为变量或值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-372">If `I` identifies a shared variable or an instance variable and no type arguments were supplied, then the result is either a variable or a value.</span></span> <span data-ttu-id="2ada7-373">如果变量是只读的并且引用出现在其中将变量声明适当类型的变量 （共享或实例），类的构造函数外，则结果为变量的值`I`中引用的对象通过`E`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-373">If the variable is read-only, and the reference occurs outside a constructor of the class in which the variable is declared appropriate for the kind of variable (shared or instance), then the result is the value of the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="2ada7-374">如果`T`是引用类型，则结果为变量`I`中所引用的对象`E`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-374">If `T` is a reference type, then the result is the variable `I` in the object referenced by `E`.</span></span> <span data-ttu-id="2ada7-375">否则为如果`T`是值类型和表达式`E`是分类为变量，则结果为变量; 否则结果是一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-375">Otherwise, if `T` is a value type and the expression `E` is classified as a variable, the result is a variable; otherwise the result is a value.</span></span>
    45. <span data-ttu-id="2ada7-376">如果`I`标识的事件和任何类型提供的参数，则结果为的相关联的目标表达式的事件访问`E`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-376">If `I` identifies an event and no type arguments were supplied, the result is an event access with an associated target expression of `E`.</span></span>
    46. <span data-ttu-id="2ada7-377">如果`I`标识一个常量和提供参数，没有类型，则结果为该常量的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-377">If `I` identifies a constant and no type arguments were supplied, then the result is the value of that constant.</span></span>
    47. <span data-ttu-id="2ada7-378">如果`I`标识枚举成员，并提供参数，没有类型，则结果为该枚举成员的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-378">If `I` identifies an enumeration member and no type arguments were supplied, then the result is the value of that enumeration member.</span></span>
    48. <span data-ttu-id="2ada7-379">如果`T`是`Object`，则结果为后期绑定成员查找归类为具有关联的类型参数列表和相关联的目标表达式的后期绑定访问`E`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-379">If `T` is `Object`, then the result is a late-bound member lookup classified as a late-bound access with the associated type argument list and an associated target expression of `E`.</span></span>

5. <span data-ttu-id="2ada7-380">否则为`E.I`是无效的成员引用，并且将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-380">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>

<span data-ttu-id="2ada7-381">成员访问窗体`MyClass.I(Of A)`等效于`Me.I(Of A)`，但在其上访问的所有成员都被都看作，就好像成员是非可重写。</span><span class="sxs-lookup"><span data-stu-id="2ada7-381">A member access of the form `MyClass.I(Of A)` is equivalent to `Me.I(Of A)`, but all members accessed on it are treated as if the members are non-overridable.</span></span> <span data-ttu-id="2ada7-382">因此，访问的成员不会受访问该成员的值的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-382">Thus, the member accessed will not be affected by the run-time type of the value on which the member is being accessed.</span></span>

<span data-ttu-id="2ada7-383">成员访问窗体`MyBase.I(Of A)`等效于`CType(Me, T).I(Of A)`其中`T`是包含成员访问表达式的类型的直接基类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-383">A member access of the form `MyBase.I(Of A)` is equivalent to `CType(Me, T).I(Of A)` where `T` is the direct base type of the type containing the member access expression.</span></span> <span data-ttu-id="2ada7-384">其上的所有方法调用都被都看作正在调用的方法是非可重写。</span><span class="sxs-lookup"><span data-stu-id="2ada7-384">All method invocations on it are treated as if the method being invoked is non-overridable.</span></span> <span data-ttu-id="2ada7-385">这种形式的成员访问也称为*基访问*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-385">This form of member access is also called a *base access*.</span></span>

<span data-ttu-id="2ada7-386">下面的示例演示如何`Me`，`MyBase`和`MyClass`相关：</span><span class="sxs-lookup"><span data-stu-id="2ada7-386">The following example demonstrates how `Me`, `MyBase` and `MyClass` relate:</span></span>

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

<span data-ttu-id="2ada7-387">此代码会输出：</span><span class="sxs-lookup"><span data-stu-id="2ada7-387">This code prints out:</span></span>

```
MoreDerived.F
Derived.F
Derived.F
```

<span data-ttu-id="2ada7-388">成员访问表达式时以关键字开头`Global`，关键字表示最外层的未命名命名空间，这是在其中声明会覆盖封闭命名空间的情况下很有用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-388">When a member access expression begins with the keyword `Global`, the keyword represents the outermost unnamed namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="2ada7-389">`Global`关键字使"转义"出到这种情况中的最外层命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-389">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="2ada7-390">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-390">For example:</span></span>

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

<span data-ttu-id="2ada7-391">在上述示例中，第一个方法调用无效因为标识符`System`绑定到该类`System`，不是命名空间`System`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-391">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="2ada7-392">访问的唯一办法`System`命名空间是使用`Global`来转义扩展到最外层命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-392">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span>

<span data-ttu-id="2ada7-393">如果正在访问的成员共享的左侧和右侧期间的任何表达式是多余的则不计算，除非执行成员访问后期绑定。</span><span class="sxs-lookup"><span data-stu-id="2ada7-393">If the member being accessed is shared, any expression on the left side of the period is superfluous and is not evaluated unless the member access is done late-bound.</span></span> <span data-ttu-id="2ada7-394">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="2ada7-394">For example, consider the following code:</span></span>

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

<span data-ttu-id="2ada7-395">它将打印`The value of F is: 10`因为函数`ReturnC`无需调用来提供的一个实例`C`来访问共享的成员`F`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-395">It prints `The value of F is: 10` because the function `ReturnC` does not need to be called to provide an instance of `C` to access the shared member `F`.</span></span>


### <a name="identical-type-and-member-names"></a><span data-ttu-id="2ada7-396">相同类型和成员名称</span><span class="sxs-lookup"><span data-stu-id="2ada7-396">Identical Type and Member Names</span></span>

<span data-ttu-id="2ada7-397">不使用相同的名称作为其类型的名称成员少见。</span><span class="sxs-lookup"><span data-stu-id="2ada7-397">It is not uncommon to name members using the same name as their type.</span></span> <span data-ttu-id="2ada7-398">在这种情况下，但是，隐藏不方便的名称会发生：</span><span class="sxs-lookup"><span data-stu-id="2ada7-398">In that situation, however, inconvenient name hiding can occur:</span></span>

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

<span data-ttu-id="2ada7-399">在上一示例中，简单名称`Color`在`DefaultColor`绑定到实例属性，而不是类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-399">In the previous example, the simple name `Color` in `DefaultColor` binds to the instance property instead of the type.</span></span> <span data-ttu-id="2ada7-400">由于不能在共享成员中引用的实例成员，这通常会出现错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-400">Because an instance member cannot be referenced in a shared member, this would normally be an error.</span></span>

<span data-ttu-id="2ada7-401">但是，特殊规则在这种情况下允许访问类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-401">However, a special rule allows access to the type in this case.</span></span> <span data-ttu-id="2ada7-402">如果成员访问表达式的基表达式是一个简单名称，并将绑定到常量、 字段、 属性、 本地变量或参数的类型具有相同的名称，然后在到成员或类型的基表达式可以引用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-402">If the base expression of a member access expression is a simple name and binds to a constant, field, property, local variable or parameter whose type has the same name, then the base expression can refer either to the member or the type.</span></span> <span data-ttu-id="2ada7-403">这永远不可能导致二义性，因为可以访问从两者中任何一个的成员相同。</span><span class="sxs-lookup"><span data-stu-id="2ada7-403">This can never result in ambiguity because the members that can be accessed off of either one are the same.</span></span>

### <a name="default-instances"></a><span data-ttu-id="2ada7-404">默认实例</span><span class="sxs-lookup"><span data-stu-id="2ada7-404">Default Instances</span></span>

<span data-ttu-id="2ada7-405">在某些情况下，从一种常见派生的类通常基类或始终具有只有一个实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-405">In some situations, classes derived from a common base class usually or always have only a single instance.</span></span> <span data-ttu-id="2ada7-406">例如，只会显示用户界面中的大多数窗口都具有在任何时间在屏幕上显示的一个实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-406">For example, most windows shown in a user interface only ever have one instance showing on the screen at any time.</span></span> <span data-ttu-id="2ada7-407">若要简化这些类型的类的处理，Visual Basic 可以自动生成*默认实例*的每个类提供一个轻松地引用实例的类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-407">To simplify working with these types of classes, Visual Basic can automatically generate *default instances* of the classes that provide a single, easily referenced instance for each class.</span></span>

<span data-ttu-id="2ada7-408">默认实例始终为创建*系列*的类型而不是一种特定类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-408">Default instances are always created for a *family* of types rather than for one particular type.</span></span> <span data-ttu-id="2ada7-409">因此而不被创建一个类派生自窗体的 Form1 的默认实例，默认实例创建的所有类派生自窗体。</span><span class="sxs-lookup"><span data-stu-id="2ada7-409">So instead of creating a default instance for a class Form1 that derives from Form, default instances are created for all classes derived from Form.</span></span> <span data-ttu-id="2ada7-410">这意味着每个派生自的基类的单个类不需要专门标记为具有默认实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-410">This means that each individual class that derives from the base class does not have to be specially marked to have a default instance.</span></span>

<span data-ttu-id="2ada7-411">由编译器生成的属性，返回该类的默认实例表示一个类的默认实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-411">The default instance of a class is represented by a compiler-generated property that returns the default instance of that class.</span></span> <span data-ttu-id="2ada7-412">生成的类成员的属性名为*分组类*，管理分配并销毁所有类的默认实例派生自特定基类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-412">The property generated as a member of a class called the *group class* that manages allocating and destroying default instances for all classes derived from the particular base class.</span></span> <span data-ttu-id="2ada7-413">例如，所有的类的默认实例属性派生自`Form`可能会收集在`MyForms`类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-413">For example, all of the default instance properties of classes derived from `Form` may be collected in the `MyForms` class.</span></span> <span data-ttu-id="2ada7-414">如果表达式返回组类的实例`My.Forms`，则下面的代码访问派生类的默认实例`Form1`和`Form2`:</span><span class="sxs-lookup"><span data-stu-id="2ada7-414">If an instance of the group class is returned by the expression `My.Forms`, then the following code accesses the default instances of derived classes `Form1` and `Form2`:</span></span>

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

<span data-ttu-id="2ada7-415">默认实例将不会创建之前对它们; 的第一个引用获取表示默认实例的属性会导致如果它尚未创建或已被设置为要创建的默认实例`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-415">Default instances will not be created until the first reference to them; fetching the property representing the default instance causes the default instance to be created if it has not already been created or has been set to `Nothing`.</span></span> <span data-ttu-id="2ada7-416">以便测试是否存在默认实例，默认实例时的目标`Is`或`IsNot`运算符，不会创建默认实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-416">To allow testing for the existence of a default instance, when a default instance is the target of an `Is` or `IsNot` operator, the default instance will not be created.</span></span> <span data-ttu-id="2ada7-417">因此，就可以测试是否默认实例为`Nothing`或某些其他引用而不会导致要创建的默认实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-417">Thus, it is possible to test whether a default instance is `Nothing` or some other reference without causing the default instance to be created.</span></span>

<span data-ttu-id="2ada7-418">默认实例旨在方便地引用从具有默认实例的类的外部的默认实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-418">Default instances are intended to make it easy to refer to the default instance from outside of the class that has the default instance.</span></span> <span data-ttu-id="2ada7-419">使用从定义它的类中的默认实例可能会导致的混淆对于哪些实例所引用，即默认实例或当前实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-419">Using a default instance from within a class that defines it might cause confusion as to which instance is being referred to, i.e. the default instance or the current instance.</span></span> <span data-ttu-id="2ada7-420">例如，下面的代码修改仅值`x`在默认情况下，即使它正在调用另一个实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-420">For example, the following code modifies only the value `x` in the default instance, even though it is being called from another instance.</span></span> <span data-ttu-id="2ada7-421">因此，代码将打印的值`5`而不是`10`:</span><span class="sxs-lookup"><span data-stu-id="2ada7-421">Thus the code would print the value `5` instead of `10`:</span></span>

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

<span data-ttu-id="2ada7-422">若要防止这种混淆，它不是类型的有效来指代的默认实例从实例方法中的默认实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-422">To prevent this kind of confusion, it is not valid to refer to a default instance from within an instance method of the default instance's type.</span></span>

#### <a name="default-instances-and-type-names"></a><span data-ttu-id="2ada7-423">默认实例和类型名称</span><span class="sxs-lookup"><span data-stu-id="2ada7-423">Default Instances and Type Names</span></span>

<span data-ttu-id="2ada7-424">默认实例也可能是可直接通过其类型的名称访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-424">A default instance may also be accessible directly through its type's name.</span></span> <span data-ttu-id="2ada7-425">在这种情况下，在任何表达式上下文中的类型名称不允许在表达式`E`，其中`E`表示具有默认实例的类的完全限定的名称，更改为`E'`，其中`E'`表示一个表达式，提取的默认实例属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-425">In this case, in any expression context where the type name is not allowed the expression `E`, where `E` represents the fully qualified name of the class with a default instance, is changed to `E'`, where `E'` represents an expression that fetches the default instance property.</span></span> <span data-ttu-id="2ada7-426">例如，如果默认实例的类派生自`Form`允许通过类型名称，访问默认实例，则下面的代码是等效于在前面的示例代码：</span><span class="sxs-lookup"><span data-stu-id="2ada7-426">For example, if default instances for classes derived from `Form` allow accessing the default instance through the type name, then the following code is equivalent to the code in the previous example:</span></span>

```vb
Module Main
    Sub Main()
        Form1.x = 10
        Console.WriteLine(Form2.y)
    End Sub
End Module
```

<span data-ttu-id="2ada7-427">这还意味着可通过其类型的名称访问的默认实例也会通过类型名称可分配。</span><span class="sxs-lookup"><span data-stu-id="2ada7-427">This also means that a default instance that is accessible through its type's name is also assignable through the type name.</span></span> <span data-ttu-id="2ada7-428">例如，下面的代码设置的默认实例`Form1`到`Nothing`:</span><span class="sxs-lookup"><span data-stu-id="2ada7-428">For example, the following code sets the default instance of `Form1` to `Nothing`:</span></span>

```vb
Module Main
    Sub Main()
        Form1 = Nothing
    End Sub
End Module
```

<span data-ttu-id="2ada7-429">请注意的含义`E.I`已`E`表示的类和`I`表示共享的成员不会更改。</span><span class="sxs-lookup"><span data-stu-id="2ada7-429">Note that the meaning of `E.I` were `E` represents a class and `I` represents a shared member does not change.</span></span> <span data-ttu-id="2ada7-430">此类表达式仍可访问共享的成员直接从类实例，并且未引用的默认实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-430">Such an expression still accesses the shared member directly off of the class instance and does not reference the default instance.</span></span>

#### <a name="group-classes"></a><span data-ttu-id="2ada7-431">组类</span><span class="sxs-lookup"><span data-stu-id="2ada7-431">Group Classes</span></span>

<span data-ttu-id="2ada7-432">`Microsoft.VisualBasic.MyGroupCollectionAttribute`属性指示一系列的默认实例的组类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-432">The `Microsoft.VisualBasic.MyGroupCollectionAttribute` attribute indicates the group class for a family of default instances.</span></span> <span data-ttu-id="2ada7-433">该属性具有四个参数：</span><span class="sxs-lookup"><span data-stu-id="2ada7-433">The attribute has four parameters:</span></span>

* <span data-ttu-id="2ada7-434">参数`TypeToCollect`指定组的基类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-434">The parameter `TypeToCollect` specifies the base class for the group.</span></span> <span data-ttu-id="2ada7-435">不带开放类型参数派生自具有此名称 （而不考虑类型参数） 的类型的所有可实例化类会自动具有默认实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-435">All instantiable classes without open type parameters that derive from a type with this name (regardless of type parameters) will automatically have a default instance.</span></span>

* <span data-ttu-id="2ada7-436">参数`CreateInstanceMethodName`指定要在要创建的新实例的默认实例属性中的组类中调用的方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-436">The parameter `CreateInstanceMethodName` specifies the method to call in the group class to create a new instance in a default instance property.</span></span>

* <span data-ttu-id="2ada7-437">将参数`DisposeInstanceMethodName`指定要在要释放的默认实例属性，如果默认实例属性分配值的组类中调用的方法`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-437">The parameter `DisposeInstanceMethodName` specifies the method to call in the group class to dispose of a default instance property if the default instance property is assigned the value `Nothing`.</span></span>

* <span data-ttu-id="2ada7-438">将参数`DefaultInstanceAlias`详细信息表达式`E'`来代替类名称的默认实例是否可以访问直接通过其类型名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-438">The parameter `DefaultInstanceAlias` specifics the expression `E'` to substitute for the class name if the default instances are accessible directly through their type name.</span></span> <span data-ttu-id="2ada7-439">如果此参数为`Nothing`或空字符串，此组类型上的默认实例不是可直接通过其类型的名称访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-439">If this parameter is `Nothing` or an empty string, default instances on this group type are not accessible directly through their type's name.</span></span> <span data-ttu-id="2ada7-440">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-440">(__Note.__</span></span> <span data-ttu-id="2ada7-441">在 Visual Basic 语言的所有当前实现中`DefaultInstanceAlias`参数将被忽略，编译器提供的代码中除外。)</span><span class="sxs-lookup"><span data-stu-id="2ada7-441">In all current implementations of the Visual Basic language, the `DefaultInstanceAlias` parameter is ignored, except in compiler-provided code.)</span></span>

<span data-ttu-id="2ada7-442">通过分隔这些名称中使用逗号分隔的前三个参数的类型和方法，多个类型可以收集到同一个组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-442">Multiple types can be collected into the same group by separating the names of the types and methods in the first three parameters using commas.</span></span> <span data-ttu-id="2ada7-443">必须有相同数目的项在每个参数，并将列表元素按顺序进行比较。</span><span class="sxs-lookup"><span data-stu-id="2ada7-443">There must be the same number of items in each parameter, and the list elements are matched in order.</span></span> <span data-ttu-id="2ada7-444">例如，下面的特性声明收集派生的类型`C1`，`C2`或`C3`入一个组：</span><span class="sxs-lookup"><span data-stu-id="2ada7-444">For example, the following attribute declaration collects types that derive from `C1`, `C2` or `C3` into a single group:</span></span>

```vb
<Microsoft.VisualBasic.MyGroupCollection("C1, C2, C3", _
    "CreateC1, CreateC2, CreateC3", _
    "DisposeC1, DisposeC2, DisposeC3", "My.Cs")>
Public NotInheritable Class MyCs
    ...
End Class
```

<span data-ttu-id="2ada7-445">创建方法的签名必须采用以下形式`Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-445">The signature of the create method must be of the form `Shared Function <Name>(Of T As {New, <Type>})(Instance Of T) As T`.</span></span> <span data-ttu-id="2ada7-446">Dispose 方法必须采用以下形式`Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-446">The dispose method must be of the form `Shared Sub <Name>(Of T As <Type>)(ByRef Instance Of T)`.</span></span> <span data-ttu-id="2ada7-447">因此，对于上一节中的示例组类可以声明，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-447">Thus, the group class for the example in the preceding section could be declared as follows:</span></span>

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

<span data-ttu-id="2ada7-448">如果源代码文件声明派生的类`Form1`，生成的组类应该是等效于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-448">If a source file declared a derived class `Form1`, the generated group class would be equivalent to:</span></span>

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

### <a name="extension-method-collection"></a><span data-ttu-id="2ada7-449">扩展方法集合</span><span class="sxs-lookup"><span data-stu-id="2ada7-449">Extension Method Collection</span></span>

<span data-ttu-id="2ada7-450">扩展方法的成员访问表达式`E.I`收集的收集的所有扩展方法名称`I`当前上下文中提供：</span><span class="sxs-lookup"><span data-stu-id="2ada7-450">Extension methods for the member access expression `E.I` are collected by gathering all of the extension methods with the name `I` that are available in the current context:</span></span>

1. <span data-ttu-id="2ada7-451">首先，每个包含的表达式的嵌套的类型检查，从最里层开始，并转到最外面。</span><span class="sxs-lookup"><span data-stu-id="2ada7-451">First, each nested type containing the expression is checked, starting from the innermost and going to the outermost.</span></span>
2. <span data-ttu-id="2ada7-452">然后，每个嵌套的命名空间被检查时，从最里层开始，然后转到最外层命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-452">Then, each nested namespace is checked, starting from the innermost and going to the outermost namespace.</span></span>
3. <span data-ttu-id="2ada7-453">然后，检查源文件中的导入。</span><span class="sxs-lookup"><span data-stu-id="2ada7-453">Then, the imports in the source file are checked.</span></span>
4. <span data-ttu-id="2ada7-454">然后，将检查由编译环境定义的导入。</span><span class="sxs-lookup"><span data-stu-id="2ada7-454">Then, the imports defined by the compilation environment are checked.</span></span>

<span data-ttu-id="2ada7-455">仅当没有扩大本机转换从目标表达式类型为第一个参数的类型的扩展方法收集的扩展方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-455">An extension method is collected only if there is a widening native conversion from the target expression type to the type of the first parameter of the extension method.</span></span> <span data-ttu-id="2ada7-456">并搜索与正则简单名称表达式绑定不同收集*所有*扩展方法; 找到的扩展方法时，集合不会停止。</span><span class="sxs-lookup"><span data-stu-id="2ada7-456">And unlike regular simple name expression binding, the search collects *all* extension methods; the collection does not stop when an extension method is found.</span></span> <span data-ttu-id="2ada7-457">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-457">For example:</span></span>

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

<span data-ttu-id="2ada7-458">在此示例中，即使`N2C1Extensions.M1`之前找到`N1C1Extensions.M1`，它们都被视为作为扩展方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-458">In this example, even though `N2C1Extensions.M1` is found before `N1C1Extensions.M1`, they both are considered as extension methods.</span></span> <span data-ttu-id="2ada7-459">一旦收集的所有扩展方法，然后，他们就*科里化*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-459">Once all of the extension methods have been collected, they are then *curried*.</span></span> <span data-ttu-id="2ada7-460">科采用扩展方法调用的目标，并将其应用到扩展方法调用时，生成新的方法签名中删除 （因为它已指定） 的第一个参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-460">Currying takes the target of the extension method call and applies it to the extension method call, resulting in a new method signature with the first parameter removed (because it has been specified).</span></span> <span data-ttu-id="2ada7-461">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-461">For example:</span></span>

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

<span data-ttu-id="2ada7-462">在上面的示例中，应用的扩充的结果`v`到`Ext1.M`是方法签名`Sub M(y As Integer)`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-462">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(y As Integer)`.</span></span>

<span data-ttu-id="2ada7-463">除了删除扩展方法的第一个参数，科还会删除任何属于第一个参数的类型的方法类型参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-463">In addition to removing the first parameter of the extension method, currying also removes any method type parameters that are a part of the type of the first parameter.</span></span> <span data-ttu-id="2ada7-464">类型推理时科具有方法类型参数的扩展方法，应用于第一个参数和结果固定推断出任何类型参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-464">When currying an extension method with method type parameter, type inference is applied to the first parameter and the result is fixed for any type parameters that are inferred.</span></span> <span data-ttu-id="2ada7-465">如果类型推理失败，此方法将被忽略。</span><span class="sxs-lookup"><span data-stu-id="2ada7-465">If type inference fails, the method is ignored.</span></span> <span data-ttu-id="2ada7-466">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-466">For example:</span></span>

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

<span data-ttu-id="2ada7-467">在上面的示例中，应用的扩充的结果`v`到`Ext1.M`是方法签名`Sub M(Of U)(y As U)`，因为类型形参`T`由于科推断出，现已修复。</span><span class="sxs-lookup"><span data-stu-id="2ada7-467">In the above example, the curried result of applying `v` to `Ext1.M` is the method signature `Sub M(Of U)(y As U)`, because the type parameter `T` is inferred as a result of the currying and is now fixed.</span></span> <span data-ttu-id="2ada7-468">因为类型形参`U`时不会推断出作为一部分科，它将保持打开的参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-468">Because the type parameter `U` was not inferred as a part of the currying, it remains an open parameter.</span></span> <span data-ttu-id="2ada7-469">同样，因为类型形参`T`推断结果应用`v`到`Ext2.M`，参数类型`y`将成为固定为`Integer`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-469">Similarly, because the type parameter `T` is inferred as a result of applying `v` to `Ext2.M`, the type of parameter `y` becomes fixed as `Integer`.</span></span> <span data-ttu-id="2ada7-470">它不将被推断为任何其他类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-470">It will not be inferred to be any other type.</span></span> <span data-ttu-id="2ada7-471">科签名，但所有约束时`New`约束也适用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-471">When currying the signature, all constraints except for `New` constraints are also applied.</span></span> <span data-ttu-id="2ada7-472">如果未满足，或依赖于未被推断为一部分科类型的约束，则忽略该扩展方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-472">If the constraints are not satisfied, or depend on a type that was not inferred as a part of currying, the extension method is ignored.</span></span> <span data-ttu-id="2ada7-473">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-473">For example:</span></span>

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

<span data-ttu-id="2ada7-474">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-474">__Note.__</span></span> <span data-ttu-id="2ada7-475">用于执行科的扩展方法的主要原因之一是迭代的它允许查询表达式推断的类型计算查询模式方法的参数之前。</span><span class="sxs-lookup"><span data-stu-id="2ada7-475">One of the main reasons for doing currying of extension methods is that it allows query expressions to infer the type of the iteration before evaluating the arguments to a query pattern method.</span></span> <span data-ttu-id="2ada7-476">由于查询模式的大多数方法采用 lambda 表达式，这需要类型推理本身，这极大地简化了查询表达式的计算过程。</span><span class="sxs-lookup"><span data-stu-id="2ada7-476">Since most query pattern methods take lambda expressions, which require type inference themselves, this greatly simplifies the process of evaluating a query expression.</span></span>

<span data-ttu-id="2ada7-477">与普通接口继承，不同扩展与另一个不相关的两个接口的扩展方法提供了，只要它们不具有相同的扩充的签名：</span><span class="sxs-lookup"><span data-stu-id="2ada7-477">Unlike normal interface inheritance, extension methods that extend two interfaces that do not relate to one another are available, as long as they do not have the same curried signature:</span></span>

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

<span data-ttu-id="2ada7-478">最后，务必记住该方法被认为执行后期绑定时不扩展：</span><span class="sxs-lookup"><span data-stu-id="2ada7-478">Finally, it is important to remember that extension methods are not considered when doing late binding:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = ...

        ' Ignores extension methods
        o.M1()
    End Sub
End Module
```

## <a name="dictionary-member-access-expressions"></a><span data-ttu-id="2ada7-479">字典成员访问表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-479">Dictionary Member Access Expressions</span></span>

<span data-ttu-id="2ada7-480">一个*字典成员访问表达式*用于查找的集合的成员。</span><span class="sxs-lookup"><span data-stu-id="2ada7-480">A *dictionary member access expression* is used to look up a member of a collection.</span></span> <span data-ttu-id="2ada7-481">字典成员访问采用的格式`E!I`，其中`E`分类为一个值的表达式和`I`是一个标识符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-481">A dictionary member access takes the form of `E!I`, where `E` is an expression that is classified as a value and `I` is an identifier.</span></span>

```antlr
DictionaryAccessExpression
    : Expression? '!' IdentifierOrKeyword
    ;
```

<span data-ttu-id="2ada7-482">表达式的类型必须具有默认属性由单个索引`String`参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-482">The type of the expression must have a default property indexed by a single `String` parameter.</span></span> <span data-ttu-id="2ada7-483">字典成员访问表达式`E!I`转换为表达式`E.D("I")`，其中`D`是默认属性`E`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-483">The dictionary member access expression `E!I` is transformed into the expression `E.D("I")`, where `D` is the default property of `E`.</span></span> <span data-ttu-id="2ada7-484">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-484">For example:</span></span>

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

<span data-ttu-id="2ada7-485">如果使用任何表达式中直接包含的表达式指定一个感叹号`With`假定语句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-485">If an exclamation point is specified with no expression, the expression from the immediately containing `With` statement is assumed.</span></span> <span data-ttu-id="2ada7-486">如果没有包含`With`语句中，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-486">If there is no containing `With` statement, a compile-time error occurs.</span></span>


## <a name="invocation-expressions"></a><span data-ttu-id="2ada7-487">调用表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-487">Invocation Expressions</span></span>

<span data-ttu-id="2ada7-488">调用表达式由一个调用目标和一个可选参数列表组成。</span><span class="sxs-lookup"><span data-stu-id="2ada7-488">An invocation expression consists of an invocation target and an optional argument list.</span></span>

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

<span data-ttu-id="2ada7-489">目标表达式必须分类为方法组或其类型为委托类型的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-489">The target expression must be classified as a method group or a value whose type is a delegate type.</span></span> <span data-ttu-id="2ada7-490">如果目标表达式是委托类型，其类型的值，则调用表达式的目标会变为为方法组`Invoke`委托类型和目标表达式的成员将成为该方法的相关联的目标表达式组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-490">If the target expression is a value whose type is a delegate type, then the target of the invocation expression becomes the method group for the `Invoke` member of the delegate type and the target expression becomes the associated target expression of the method group.</span></span>

<span data-ttu-id="2ada7-491">参数列表具有两个部分： 位置参数和命名自变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-491">An argument list has two sections: positional arguments and named arguments.</span></span> <span data-ttu-id="2ada7-492">*位置参数*是表达式，必须在任何命名的参数之前。</span><span class="sxs-lookup"><span data-stu-id="2ada7-492">*Positional arguments* are expressions and must precede any named arguments.</span></span> <span data-ttu-id="2ada7-493">*命名的自变量*开头的标识符，可以匹配关键字后, 跟`:=`和表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-493">*Named arguments* start with an identifier that can match keywords, followed by `:=` and an expression.</span></span>

<span data-ttu-id="2ada7-494">如果方法组仅包含一个可访问的方法，包括实例和扩展方法，以及该方法不采用任何参数是一个函数、 方法组将被解释为表达式调用参数列表为空并结果是用作具有提供的参数列表的调用表达式的目标。</span><span class="sxs-lookup"><span data-stu-id="2ada7-494">If the method group only contains one accessible method, including both instance and extension methods, and that method takes no arguments and is a function, then the method group is interpreted as an invocation expression with an empty argument list and the result is used as the target of an invocation expression with the provided argument list(s).</span></span> <span data-ttu-id="2ada7-495">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-495">For example:</span></span>

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

<span data-ttu-id="2ada7-496">否则，重载决策被应用于方法，以便选择最适用的方法为给定的参数列表。</span><span class="sxs-lookup"><span data-stu-id="2ada7-496">Otherwise, overload resolution is applied to the methods to pick the most applicable method for the given argument list(s).</span></span> <span data-ttu-id="2ada7-497">如果最适用的方法是函数，然后调用表达式的结果被归类为类型为该函数的返回类型的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-497">If the most applicable method is a function, then the result of the invocation expression is classified as a value typed as the return type of the function.</span></span> <span data-ttu-id="2ada7-498">如果最适用的方法是一个子例程，结果被归类为 void。</span><span class="sxs-lookup"><span data-stu-id="2ada7-498">If the most applicable method is a subroutine, then the result is classified as void.</span></span> <span data-ttu-id="2ada7-499">如果最适用的方法是没有正文的分部方法，然后调用表达式被忽略并且结果被归类为 void。</span><span class="sxs-lookup"><span data-stu-id="2ada7-499">If the most applicable method is a partial method that has no body, then the invocation expression is ignored and the result is classified as void.</span></span>

<span data-ttu-id="2ada7-500">对于早期绑定调用表达式而言，目标方法中声明的相应参数的顺序计算这些实参。</span><span class="sxs-lookup"><span data-stu-id="2ada7-500">For an early-bound invocation expression, the arguments are evaluated in the order in which the corresponding parameters are declared in the target method.</span></span> <span data-ttu-id="2ada7-501">对于后期绑定成员访问表达式，按它们在成员访问表达式中出现的顺序计算： 请参阅部分[Late 绑定表达式](expressions.md#late-bound-expressions)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-501">For a late-bound member access expression, they are evaluated in the order in which they appear in the member access expression: see Section [Late-Bound Expressions](expressions.md#late-bound-expressions).</span></span>

## <a name="overloaded-method-resolution"></a><span data-ttu-id="2ada7-502">解决方法的重载的方法：</span><span class="sxs-lookup"><span data-stu-id="2ada7-502">Overloaded Method Resolution:</span></span>
<span data-ttu-id="2ada7-503">重载解析、 特异性成员/类型指定一个参数的列表中，泛型，是否适用于参数列表、 传递参数和可选参数，条件方法的选择参数和类型自变量推理： 请参阅部分[重载决策](overload-resolution.md)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-503">For Overload Resolution, Specificity of members/types given an argument list, Genericity, Applicability to Argument List, Passing Arguments, and Picking Arguments for Optional Parameters, Conditional Methods, and Type Argument Inference: see Section [Overload Resolution](overload-resolution.md).</span></span>

## <a name="index-expressions"></a><span data-ttu-id="2ada7-504">索引表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-504">Index Expressions</span></span>

<span data-ttu-id="2ada7-505">*表达式创建索引*结果的数组元素中或进行了重新分类到属性访问的属性组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-505">An *index expression* results in an array element or reclassifies a property group into a property access.</span></span> <span data-ttu-id="2ada7-506">索引表达式组成，在订单、 表达式、 左括号、 索引参数列表和右括号。</span><span class="sxs-lookup"><span data-stu-id="2ada7-506">An index expression consists of, in order, an expression, an opening parenthesis, an index argument list, and a closing parenthesis.</span></span>

```antlr
IndexExpression
    : Expression OpenParenthesis ArgumentList? CloseParenthesis
    ;
```

<span data-ttu-id="2ada7-507">目标索引表达式必须分类为属性组或值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-507">The target of the index expression must be classified as either a property group or a value.</span></span> <span data-ttu-id="2ada7-508">索引表达式进行处理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-508">An index expression is processed as follows:</span></span>

* <span data-ttu-id="2ada7-509">如果目标表达式分类为一个值，并且其类型不是数组类型， `Object`，或`System.Array`，类型必须具有默认属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-509">If the target expression is classified as a value and if its type is not an array type, `Object`, or `System.Array`, the type must have a default property.</span></span> <span data-ttu-id="2ada7-510">表示所有类型的默认属性的属性组上执行索引。</span><span class="sxs-lookup"><span data-stu-id="2ada7-510">The index is performed on a property group that represents all of the default properties of the type.</span></span> <span data-ttu-id="2ada7-511">虽然不能用来声明在 Visual Basic 中的无参数的默认属性，但其他语言可能会允许声明此类属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-511">Although it is not valid to declare a parameterless default property in Visual Basic, other languages may allow declaring such a property.</span></span> <span data-ttu-id="2ada7-512">因此，允许索引不带任何参数的属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-512">Consequently, indexing a property with no arguments is allowed.</span></span>

* <span data-ttu-id="2ada7-513">如果表达式的结果值为数组类型，自变量列表中的参数数目必须与数组类型的秩的相同，并且可能不包括命名的参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-513">If the expression results in a value of an array type, the number of arguments in the argument list must be the same as the rank of the array type and may not include named arguments.</span></span> <span data-ttu-id="2ada7-514">如果是任何索引在运行时，无效`System.IndexOutOfRangeException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-514">If any of the indexes are invalid at run time, a `System.IndexOutOfRangeException` exception is thrown.</span></span> <span data-ttu-id="2ada7-515">每个表达式必须是隐式转换为键入`Integer`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-515">Each expression must be implicitly convertible to type `Integer`.</span></span> <span data-ttu-id="2ada7-516">索引表达式的结果是位于指定索引处的变量和分类为变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-516">The result of the index expression is the variable at the specified index and is classified as a variable.</span></span>

* <span data-ttu-id="2ada7-517">如果表达式分类为属性组中，重载决策用于确定是否其中一个属性是适用于索引参数列表。</span><span class="sxs-lookup"><span data-stu-id="2ada7-517">If the expression is classified as a property group, overload resolution is used to determine whether one of the properties is applicable to the index argument list.</span></span> <span data-ttu-id="2ada7-518">如果属性组只包含一个属性具有`Get`访问器，如果该访问器不采用任何参数，则属性组将被解释为具有参数列表为空的索引表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-518">If the property group only contains one property that has a `Get` accessor and if that accessor takes no arguments, then the property group is interpreted as an index expression with an empty argument list.</span></span> <span data-ttu-id="2ada7-519">此结果用作目标的当前索引表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-519">The result is used as the target of the current index expression.</span></span> <span data-ttu-id="2ada7-520">如果任何属性不都适用，则出现编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-520">If no properties are applicable, then a compile-time error occurs.</span></span> <span data-ttu-id="2ada7-521">否则，表达式的结果具有相关联的目标表达式的属性组 （如果有） 的属性访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-521">Otherwise, the expression results in a property access with the associated target expression (if any) of the property group.</span></span>

* <span data-ttu-id="2ada7-522">如果表达式分类为后期绑定属性组或值的类型`Object`或`System.Array`、 索引表达式的处理延迟到运行时和索引方式是后期绑定。</span><span class="sxs-lookup"><span data-stu-id="2ada7-522">If the expression is classified as a late-bound property group or as a value whose type is `Object` or `System.Array`, the processing of the index expression is deferred until run time and the indexing is late-bound.</span></span> <span data-ttu-id="2ada7-523">表达式将导致后期绑定属性访问类型化为`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-523">The expression results in a late-bound property access typed as `Object`.</span></span> <span data-ttu-id="2ada7-524">相关联的目标表达式是目标表达式，如果它是一个值或属性组相关联的目标表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-524">The associated target expression is either the target expression, if it is a value, or the associated target expression of the property group.</span></span> <span data-ttu-id="2ada7-525">在运行时表达式进行处理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-525">At run time the expression is processed as follows:</span></span>

* <span data-ttu-id="2ada7-526">如果表达式分类为后期绑定属性组，该表达式可能会导致方法组、 属性组或值 （如果该成员是一个实例或共享的变量）。</span><span class="sxs-lookup"><span data-stu-id="2ada7-526">If the expression is classified as a late-bound property group, the expression may result in a method group, a property group, or a value (if the member is an instance or shared variable).</span></span> <span data-ttu-id="2ada7-527">如果结果为方法组或属性组，重载决策被应用到组，以确定自变量列表的正确方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-527">If the result is a method group or property group, overload resolution is applied to the group to determine the correct method for the argument list.</span></span> <span data-ttu-id="2ada7-528">如果重载决策失败，`System.Reflection.AmbiguousMatchException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-528">If overload resolution fails, a `System.Reflection.AmbiguousMatchException` exception is thrown.</span></span> <span data-ttu-id="2ada7-529">然后为属性访问或调用的形式处理结果并返回的结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-529">Then the result is processed either as a property access or as an invocation and the result is returned.</span></span> <span data-ttu-id="2ada7-530">如果该调用是子例程的则结果是`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-530">If the invocation is of a subroutine, the result is `Nothing`.</span></span>

* <span data-ttu-id="2ada7-531">如果目标表达式的运行时类型是数组类型或`System.Array`，索引表达式的结果是变量中指定索引处的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-531">If the run-time type of the target expression is an array type or `System.Array`, the result of the index expression is the value of the variable at the specified index.</span></span>

* <span data-ttu-id="2ada7-532">否则为该表达式的运行时类型必须具有默认属性和索引表示所有类型的默认属性的属性组上执行。</span><span class="sxs-lookup"><span data-stu-id="2ada7-532">Otherwise, the run-time type of the expression must have a default property and the index is performed on the property group that represents all of the default properties on the type.</span></span> <span data-ttu-id="2ada7-533">如果类型不具有默认属性，则`System.MissingMemberException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-533">If the type has no default property, then a `System.MissingMemberException` exception is thrown.</span></span>


## <a name="new-expressions"></a><span data-ttu-id="2ada7-534">新的表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-534">New Expressions</span></span>

<span data-ttu-id="2ada7-535">`New`运算符用于创建类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-535">The `New` operator is used to create new instances of types.</span></span> <span data-ttu-id="2ada7-536">有四种形式的`New`表达式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-536">There are four forms of `New` expressions:</span></span>

* <span data-ttu-id="2ada7-537">对象创建表达式用于创建类类型和值类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-537">Object-creation expressions are used to create new instances of class types and value types.</span></span>

* <span data-ttu-id="2ada7-538">数组创建表达式用于创建数组类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-538">Array-creation expressions are used to create new instances of array types.</span></span>

* <span data-ttu-id="2ada7-539">委托创建表达式 （它们不具有从对象创建表达式不同语法） 用于创建类型的委托的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-539">Delegate-creation expressions (which do not have a distinct syntax from object-creation expressions) are used to create new instances of delegate types.</span></span>

* <span data-ttu-id="2ada7-540">匿名对象创建表达式用于创建匿名类类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-540">Anonymous object-creation expressions are used to create new instances of anonymous class types.</span></span>

```antlr
NewExpression
    : ObjectCreationExpression
    | ArrayExpression
    | AnonymousObjectCreationExpression
    ;
```

<span data-ttu-id="2ada7-541">一个`New`表达式分类为一个值，结果是该类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-541">A `New` expression is classified as a value and the result is the new instance of the type.</span></span>


### <a name="object-creation-expressions"></a><span data-ttu-id="2ada7-542">对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-542">Object-Creation Expressions</span></span>

<span data-ttu-id="2ada7-543">对象创建表达式用于创建类类型或结构类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-543">An object-creation expression is used to create a new instance of a class type or a structure type.</span></span>

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

<span data-ttu-id="2ada7-544">对象创建表达式的类型必须是类类型、 结构类型或具有的类型参数`New`约束，不能为`MustInherit`类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-544">The type of an object creation expression must be a class type, a structure type, or a type parameter with a `New` constraint and cannot be a `MustInherit` class.</span></span> <span data-ttu-id="2ada7-545">提供了窗体的一个对象创建表达式`New T(A)`，其中`T`是类类型或结构类型和`A`是一个可选参数列表中，重载决策确定正确的构造函数的`T`调用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-545">Given an object creation expression of the form `New T(A)`, where `T` is a class type or structure type and `A` is an optional argument list, overload resolution determines the correct constructor of `T` to call.</span></span> <span data-ttu-id="2ada7-546">类型参数`New`约束被视为具有一个单一的无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-546">A type parameter with a `New` constraint is considered to have a single, parameterless constructor.</span></span> <span data-ttu-id="2ada7-547">如果没有构造函数可调用时，会发生编译时错误;否则表达式的结果的新实例创建`T`使用所选的构造函数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-547">If no constructor is callable, a compile-time error occurs; otherwise the expression results in the creation of a new instance of `T` using the chosen constructor.</span></span> <span data-ttu-id="2ada7-548">如果没有任何自变量，则可省略括号。</span><span class="sxs-lookup"><span data-stu-id="2ada7-548">If there are no arguments, the parentheses may be omitted.</span></span>

<span data-ttu-id="2ada7-549">将分配一个实例取决于该实例是类类型或值类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-549">Where an instance is allocated depends on whether the instance is a class type or a value type.</span></span> <span data-ttu-id="2ada7-550">`New` 类类型的实例上系统堆中，创建时直接在堆栈上创建值类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-550">`New` instances of class types are created on the system heap, while new instances of value types are created directly on the stack.</span></span>

<span data-ttu-id="2ada7-551">对象创建表达式可以选择后构造函数参数指定成员初始值设定项的列表。</span><span class="sxs-lookup"><span data-stu-id="2ada7-551">An object-creation expression can optionally specify a list of member initializers after the constructor arguments.</span></span> <span data-ttu-id="2ada7-552">这些成员初始值设定项使用关键字作为前缀`With`，而就像它是在上下文中的初始值设定项列表将被解释`With`语句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-552">These member initializers are prefixed with the keyword `With`, and the initializer list is interpreted as if it was in the context of a `With` statement.</span></span> <span data-ttu-id="2ada7-553">例如，给定类：</span><span class="sxs-lookup"><span data-stu-id="2ada7-553">For example, given the class:</span></span>

```vb
Class Customer
    Dim Name As String
    Dim Address As String
End Class
```

<span data-ttu-id="2ada7-554">代码：</span><span class="sxs-lookup"><span data-stu-id="2ada7-554">The code:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = "123 Main St." }
    End Sub
End Module
```

<span data-ttu-id="2ada7-555">是大致相当于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-555">Is roughly equivalent to:</span></span>

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

<span data-ttu-id="2ada7-556">每个初始值设定项必须指定一个名称以将分配，并且该名称必须是一个非`ReadOnly`实例变量或正在构造; 类型的属性的成员访问权限将不后期绑定正在构造的类型是否`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-556">Each initializer must specify a name to assign, and the name must be a non-`ReadOnly` instance variable or property of the type being constructed; the member access will not be late bound if the type being constructed is `Object`.</span></span> <span data-ttu-id="2ada7-557">初始值设定项可能不会使用`Key`关键字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-557">Initializers may not use the `Key` keyword.</span></span> <span data-ttu-id="2ada7-558">在类型中的每个成员仅初始化一次。</span><span class="sxs-lookup"><span data-stu-id="2ada7-558">Each member in a type can only be initialized once.</span></span> <span data-ttu-id="2ada7-559">初始值设定项表达式中，但是，可能会相互引用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-559">The initializer expressions, however, may refer to each other.</span></span> <span data-ttu-id="2ada7-560">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-560">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim x As New Customer() With { .Name = "Bob Smith", _
            .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="2ada7-561">从左到右分配初始值设定项，因此如果初始值设定项引用尚未初始化的成员，它会看到任何值的实例变量构造函数运行后：</span><span class="sxs-lookup"><span data-stu-id="2ada7-561">The initializers are assigned left-to-right, so if an initializer refers to a member that has not been initialized yet, it will see whatever value the instance variable after the constructor ran:</span></span>

```vb
Module Test
    Sub Main()
        ' The value of Address will be " St." since Name has not been
        ' assigned yet.
        Dim x As New Customer() With { .Address = .Name & " St." }
    End Sub
End Module
```

<span data-ttu-id="2ada7-562">可以嵌套初始值设定项：</span><span class="sxs-lookup"><span data-stu-id="2ada7-562">Initializers can be nested:</span></span>

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

<span data-ttu-id="2ada7-563">如果正在创建的类型是集合类型和实例方法名为`Add`（包括扩展方法和共享的方法），然后对象创建表达式，可以指定由关键字作为前缀的集合初始值设定项`From`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-563">If the type being created is a collection type and has an instance method named `Add` (including extension methods and shared methods), then the object-creation expression can specify a collection initializer prefixed by the keyword `From`.</span></span> <span data-ttu-id="2ada7-564">对象创建表达式不能指定成员初始值设定项和集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="2ada7-564">An object-creation expression cannot specify both a member initializer and a collection initializer.</span></span> <span data-ttu-id="2ada7-565">集合初始值设定项中的每个元素作为参数传递给调用`Add`函数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-565">Each element in the collection initializer is passed as an argument to an invocation of the `Add` function.</span></span> <span data-ttu-id="2ada7-566">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-566">For example:</span></span>

```vb
Dim list = New List(Of Integer)() From { 1, 2, 3, 4 }
```

<span data-ttu-id="2ada7-567">等效于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-567">is equivalent to:</span></span>

```vb
Dim list = New List(Of Integer)()
list.Add(1)
list.Add(2)
list.Add(3)
```

<span data-ttu-id="2ada7-568">如果元素是集合初始值设定项本身，将作为的单个参数传递的子集合初始值设定项的每个元素`Add`函数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-568">If an element is a collection initializer itself, each element of the sub-collection initializer will be passed as an individual argument to the `Add` function.</span></span> <span data-ttu-id="2ada7-569">例如，以下内容：</span><span class="sxs-lookup"><span data-stu-id="2ada7-569">For example, the following:</span></span>

```vb
Dim dict = Dictionary(Of Integer, String) From { { 1, "One" },{ 2, "Two" } }
```

<span data-ttu-id="2ada7-570">等效于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-570">is equivalent to:</span></span>

```vb
Dim dict = New Dictionary(Of Integer, String)
dict.Add(1, "One")
dict.Add(2, "Two")
```

<span data-ttu-id="2ada7-571">此扩展始终执行且只执行一个层次深度;之后，子初始值设定项被视为数组文本。</span><span class="sxs-lookup"><span data-stu-id="2ada7-571">This expansion is always done and is only ever done one level deep; after that, sub-initializers are considered array literals.</span></span> <span data-ttu-id="2ada7-572">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-572">For example:</span></span>

```vb
' Error: List(Of T) does not have an Add method that takes two parameters.
Dim list = New List(Of Integer())() From { { 1, 2 }, { 3, 4 } }

' OK, this initializes the dictionary with (Integer, Integer()) pairs.
Dim dict = New Dictionary(Of Integer, Integer())() From _
        { {  1, { 2, 3 } }, { 3, { 4, 5 } } }
```


### <a name="array-expressions"></a><span data-ttu-id="2ada7-573">数组表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-573">Array Expressions</span></span>

<span data-ttu-id="2ada7-574">一个数组表达式用于创建数组类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-574">An array expression is used to create a new instance of an array type.</span></span> <span data-ttu-id="2ada7-575">有两种类型的数组表达式： 数组创建表达式和数组文本。</span><span class="sxs-lookup"><span data-stu-id="2ada7-575">There are two types of array expressions: array creation expressions, and array literals.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="2ada7-576">数组创建表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-576">Array creation expressions</span></span>

<span data-ttu-id="2ada7-577">如果提供数组大小初始化修饰符，则生成的数组类型派生的数组大小初始化参数列表中删除每个单独的自变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-577">If an array size initialization modifier is supplied, the resulting array type is derived by deleting each of the individual arguments from the array size initialization argument list.</span></span> <span data-ttu-id="2ada7-578">每个自变量的值确定的新分配的数组实例中的相应维度的上限。</span><span class="sxs-lookup"><span data-stu-id="2ada7-578">The value of each argument determines the upper bound of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="2ada7-579">如果表达式具有非空集合初始值设定项，自变量列表中的每个参数必须是常量，并且指定的表达式列表的秩和维度长度必须匹配集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="2ada7-579">If the expression has a non-empty collection initializer, each argument in the argument list must be a constant, and the rank and dimension lengths specified by the expression list must match those of the collection initializer.</span></span>

```vb
Dim a() As Integer = New Integer(2) {}
Dim b() As Integer = New Integer(2) { 1, 2, 3 }
Dim c(,) As Integer = New Integer(1, 2) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error, length/initializer mismatch.
Dim d() As Integer = New Integer(2) { 0, 1, 2, 3 }
```

<span data-ttu-id="2ada7-580">如果未提供数组大小初始化修饰符，然后类型名称必须是数组类型和集合初始值设定项必须为空或具有相同的为指定的数组类型的秩的嵌套级别数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-580">If an array size initialization modifier is not supplied, then the type name must be an array type and the collection initializer must be empty or have the same number of levels of nesting as the rank of the specified array type.</span></span> <span data-ttu-id="2ada7-581">所有中最内部的嵌套级别的元素必须是隐式转换为数组的元素类型，并且必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-581">All of the elements in the innermost nesting level must be implicitly convertible to the element type of the array and must be classified as a value.</span></span> <span data-ttu-id="2ada7-582">每个嵌套的集合初始值设定项中的元素数必须始终为与同一级别的其他集合的大小保持一致。</span><span class="sxs-lookup"><span data-stu-id="2ada7-582">The number of elements in each nested collection initializer must always be consistent with the size of the other collections at the same level.</span></span> <span data-ttu-id="2ada7-583">单独的维度长度推断出的每个相应的嵌套级别的集合初始值设定项中的元素数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-583">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the collection initializer.</span></span> <span data-ttu-id="2ada7-584">如果集合初始值设定项为空，每个维的长度为零。</span><span class="sxs-lookup"><span data-stu-id="2ada7-584">If the collection initializer is empty, the length of each dimension is zero.</span></span>

```vb
Dim e() As Integer = New Integer() { 1, 2, 3 }
Dim f(,) As Integer = New Integer(,) { { 1, 2, 3 } , { 4, 5, 6 } }

' Error: Inconsistent numbers of elements!
Dim g(,) As Integer = New Integer(,) { { 1, 2 }, { 4, 5, 6 } }

' Error: Inconsistent levels of nesting!
Dim h(,) As Integer = New Integer(,) { 1, 2, { 3, 4 } }
```

<span data-ttu-id="2ada7-585">集合初始值设定项的最外层嵌套级别对应的数组，最左边的维度，最内部的嵌套级别对应于最右边的维度。</span><span class="sxs-lookup"><span data-stu-id="2ada7-585">The outermost nesting level of a collection initializer corresponds to the leftmost dimension of an array, and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="2ada7-586">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="2ada7-586">The example:</span></span>

```vb
Dim array As Integer(,) = _
    { { 0, 1 }, { 2, 3 }, { 4, 5 }, { 6, 7 }, { 8, 9 } }
```

<span data-ttu-id="2ada7-587">等效于以下：</span><span class="sxs-lookup"><span data-stu-id="2ada7-587">Is equivalent to the following:</span></span>

```vb
Dim array(4, 1) As Integer

array(0, 0) = 0: array(0, 1) = 1
array(1, 0) = 2: array(1, 1) = 3
array(2, 0) = 4: array(2, 1) = 5
array(3, 0) = 6: array(3, 1) = 7
array(4, 0) = 8: array(4, 1) = 9
```

<span data-ttu-id="2ada7-588">如果集合初始值设定项为空 （即，一个包含大括号），但没有初始值设定项列表，边界已知的正在初始化数组的维数，空集合初始值设定项表示的指定大小的数组实例其中的所有元素具有已都初始化为元素类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-588">If the collection initializer is empty (that is, one that contains curly braces but no initializer list) and the bounds of the dimensions of the array being initialized are known, the empty collection initializer represents an array instance of the specified size where all the elements have been initialized to the element type's default value.</span></span> <span data-ttu-id="2ada7-589">如果不知道正在初始化数组的维数的边界，空集合初始值设定项表示在其中所有维度都都为大小为零的数组实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-589">If the bounds of the dimensions of the array being initialized are not known, the empty collection initializer represents an array instance in which all dimensions are size zero.</span></span>

<span data-ttu-id="2ada7-590">实例的整个生存期内是常量数组实例的级别和每个维的长度。</span><span class="sxs-lookup"><span data-stu-id="2ada7-590">An array instance's rank and length of each dimension are constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="2ada7-591">换而言之，不能更改现有数组实例，秩也不可能以调整其尺寸。</span><span class="sxs-lookup"><span data-stu-id="2ada7-591">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

#### <a name="array-literals"></a><span data-ttu-id="2ada7-592">数组文本</span><span class="sxs-lookup"><span data-stu-id="2ada7-592">Array Literals</span></span>

<span data-ttu-id="2ada7-593">数组文本表示一个数组，其元素类型、 秩和界限进行推断表达式上下文和集合初始值设定项的组合。</span><span class="sxs-lookup"><span data-stu-id="2ada7-593">An array literal denotes an array whose element type, rank, and bounds are inferred from a combination of the expression context and a collection initializer.</span></span> <span data-ttu-id="2ada7-594">这部分所述[表达式重新分类](expressions.md#expression-reclassification)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-594">This is explained in Section [Expression Reclassification](expressions.md#expression-reclassification).</span></span>

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

<span data-ttu-id="2ada7-595">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-595">For example:</span></span>

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

<span data-ttu-id="2ada7-596">格式和集合初始值设定项数组文本中的要求是完全相同的集合初始值设定项中的数组创建表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-596">The format and requirements for the collection initializer in an array literal is exactly the same as that for the collection initializer in an array creation expression.</span></span>

<span data-ttu-id="2ada7-597">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-597">__Note.__</span></span> <span data-ttu-id="2ada7-598">数组文本不会创建该数组本身;相反，它是值会导致数组创建表达式的重新分类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-598">An array literal does not create the array in and of itself; instead, it is the reclassification of the expression into a value that causes the array to be created.</span></span> <span data-ttu-id="2ada7-599">例如，转换`CType(new Integer() {1,2,3}, Short())`由于没有从转换不可能`Integer()`到`Short()`; 但表达式`CType({1,2,3},Short())`可能是因为它首先进行了重新分类数组文本到数组创建表达式`New Short() {1,2,3}`.</span><span class="sxs-lookup"><span data-stu-id="2ada7-599">For instance, the conversion `CType(new Integer() {1,2,3}, Short())` is not possible because there is no conversion from `Integer()` to `Short()`; but the expression `CType({1,2,3},Short())` is possible because it first reclassifies the array literal into the array creation expression `New Short() {1,2,3}`.</span></span>


### <a name="delegate-creation-expressions"></a><span data-ttu-id="2ada7-600">委托创建表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-600">Delegate-Creation Expressions</span></span>

<span data-ttu-id="2ada7-601">委托创建表达式用于创建委托类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-601">A delegate-creation expression is used to create a new instance of a delegate type.</span></span> <span data-ttu-id="2ada7-602">委托创建表达式的参数必须属于方法指针或 lambda 方法的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-602">The argument of a delegate-creation expression must be an expression classified as a method pointer or a lambda method.</span></span>

<span data-ttu-id="2ada7-603">如果参数为方法的指针，由方法指针所引用的方法之一必须是适用于委托类型的签名。</span><span class="sxs-lookup"><span data-stu-id="2ada7-603">If the argument is a method pointer, one of the methods referenced by the method pointer must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="2ada7-604">一种方法`M`适用于委托类型`D`如果：</span><span class="sxs-lookup"><span data-stu-id="2ada7-604">A method `M` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="2ada7-605">`M` 不是`Partial`或具有一个主体。</span><span class="sxs-lookup"><span data-stu-id="2ada7-605">`M` is not `Partial` or has a body.</span></span>

* <span data-ttu-id="2ada7-606">这两`M`并`D`是函数，或`D`是一个子例程。</span><span class="sxs-lookup"><span data-stu-id="2ada7-606">Both `M` and `D` are functions, or `D` is a subroutine.</span></span>

* <span data-ttu-id="2ada7-607">`M` 和`D`具有相同数量的参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-607">`M` and `D` have the same number of parameters.</span></span>

* <span data-ttu-id="2ada7-608">参数类型`M`每个都可以从相应参数类型的类型的类型转换`D`，和其修饰符 (即`ByRef`， `ByVal`) 匹配。</span><span class="sxs-lookup"><span data-stu-id="2ada7-608">The parameter types of `M` each have a conversion from the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="2ada7-609">返回类型`M`，如果任何，具有到的返回类型的转换`D`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-609">The return type of `M`, if any, has a conversion to the return type of `D`.</span></span>

<span data-ttu-id="2ada7-610">如果方法指针引用后期绑定访问，然后被假定后期绑定访问可具有与委托类型相同数量的参数的函数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-610">If the method pointer references a late-bound access, then the late-bound access is assumed to be to a function that has the same number of parameters as the delegate type.</span></span>

<span data-ttu-id="2ada7-611">如果未使用严格的语义和方法指针，所引用的只有一个方法，但不适用由于这一事实，它具有无参数和委托类型的作用，则此方法被认为适用的参数或返回值重新将被忽略。</span><span class="sxs-lookup"><span data-stu-id="2ada7-611">If strict semantics are not being used and there is only one method referenced by the method pointer, but it is not applicable due to the fact that it has no parameters and the delegate type does, then the method is considered applicable and the parameters or return value are simply ignored.</span></span> <span data-ttu-id="2ada7-612">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-612">For example:</span></span>

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

<span data-ttu-id="2ada7-613">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-613">__Note.__</span></span> <span data-ttu-id="2ada7-614">由于扩展方法不使用严格的语义时，仅允许这一放宽。</span><span class="sxs-lookup"><span data-stu-id="2ada7-614">This relaxation is only allowed when strict semantics are not being used because of extension methods.</span></span> <span data-ttu-id="2ada7-615">因为如果正则方法不适用，仅被视为扩展方法，就可以使用任何参数，隐藏具有用于委托构造参数的扩展方法的实例方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-615">Because extension methods are only considered if a regular method was not applicable, it is possible for an instance method with no parameters to hide an extension method with parameters for the purpose of delegate construction.</span></span>

<span data-ttu-id="2ada7-616">如果有多个方法指针所引用的一种方法是适用于该委托类型，然后使用重载解析的候选方法之间进行选取。</span><span class="sxs-lookup"><span data-stu-id="2ada7-616">If more than one method referenced by the method pointer is applicable to the delegate type, then overload resolution is used to pick between the candidate methods.</span></span> <span data-ttu-id="2ada7-617">委托的参数的类型用作出于重载解决方法的参数的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-617">The types of the parameters to the delegate are used as the types of the arguments for the purposes of overload resolution.</span></span> <span data-ttu-id="2ada7-618">如果没有一种方法的候选是最适用，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-618">If no one method candidate is most applicable, a compile-time error occurs.</span></span> <span data-ttu-id="2ada7-619">在以下示例中，本地变量初始化的委托来指的是第二个`Square`方法因为该方法更适用于签名和返回类型的`DoubleFunc`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-619">In the following example, the local variable is initialized with a delegate that refers to the second `Square` method because that method is more applicable to the signature and return type of `DoubleFunc`.</span></span>

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

<span data-ttu-id="2ada7-620">具有第二个`Square`尚未存在的方法，第一个`Square`已选择方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-620">Had the second `Square` method not been present, the first `Square` method would have been chosen.</span></span> <span data-ttu-id="2ada7-621">如果通过编译环境或通过指定严格的语义`Option Strict`，然后如果方法指针所引用的最具体方法是窄于委托签名会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-621">If strict semantics are specified by the compilation environment or by `Option Strict`, then a compile-time error occurs if the most specific method referenced by the method pointer is narrower than the delegate signature.</span></span> <span data-ttu-id="2ada7-622">一种方法`M`被视为比委托类型窄`D`如果：</span><span class="sxs-lookup"><span data-stu-id="2ada7-622">A method `M` is considered narrower than a delegate type `D` if:</span></span>

* <span data-ttu-id="2ada7-623">参数类型为`M`已扩大转换的相应参数类型为`D`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-623">A parameter type of `M` has a widening conversion to the corresponding parameter type of `D`.</span></span>

* <span data-ttu-id="2ada7-624">或者，返回类型，如果有的`M`收缩转换的返回类型为`D`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-624">Or, the return type, if any, of `M` has a narrowing conversion to the return type of `D`.</span></span>

<span data-ttu-id="2ada7-625">如果类型参数与方法指针相关联，被视为仅具有相同数量的类型参数的方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-625">If type arguments are associated with the method pointer, only methods with the same number of type arguments are considered.</span></span> <span data-ttu-id="2ada7-626">如果没有类型参数与方法指针相关联，匹配对泛型方法的签名时是否使用类型推理。</span><span class="sxs-lookup"><span data-stu-id="2ada7-626">If no type arguments are associated with the method pointer, type inference is used when matching signatures against a generic method.</span></span> <span data-ttu-id="2ada7-627">与其他普通类型推理，不同的委托的返回类型推断类型参数时使用，但仍确定至少泛型重载时不考虑返回类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-627">Unlike other normal type inference, the return type of the delegate is used when inferring type arguments, but return types are still not considered when determining the least generic overload.</span></span> <span data-ttu-id="2ada7-628">下面的示例显示了这两种提供类型实参于委托创建表达式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-628">The following example shows both ways of supplying a type argument to a delegate-creation expression:</span></span>

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

<span data-ttu-id="2ada7-629">在上述示例中，非泛型委托类型是使用泛型方法实例化。</span><span class="sxs-lookup"><span data-stu-id="2ada7-629">In the above example, a non-generic delegate type was instantiated using a generic method.</span></span> <span data-ttu-id="2ada7-630">还有可能要创建使用泛型方法的构造的委托类型的实例。</span><span class="sxs-lookup"><span data-stu-id="2ada7-630">It is also possible to create an instance of a constructed delegate type using a generic method.</span></span> <span data-ttu-id="2ada7-631">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-631">For example:</span></span>

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

<span data-ttu-id="2ada7-632">如果委托创建表达式的参数是 lambda 方法，lambda 方法必须是适用于委托类型的签名。</span><span class="sxs-lookup"><span data-stu-id="2ada7-632">If the argument to the delegate-creation expression is a lambda method, the lambda method must be applicable to the signature of the delegate type.</span></span> <span data-ttu-id="2ada7-633">Lambda 方法`L`适用于委托类型`D`如果：</span><span class="sxs-lookup"><span data-stu-id="2ada7-633">A lambda method `L` is applicable to a delegate type `D` if:</span></span>

* <span data-ttu-id="2ada7-634">如果`L`有参数，`D`具有相同数量的参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-634">If `L` has parameters, `D` has the same number of parameters.</span></span> <span data-ttu-id="2ada7-635">(如果`L`没有参数的参数`D`将被忽略。)</span><span class="sxs-lookup"><span data-stu-id="2ada7-635">(If `L` has no parameters, the parameters of `D` are ignored.)</span></span>

* <span data-ttu-id="2ada7-636">参数类型`L`每个都可以转换为相应参数类型的类型的类型`D`，和其修饰符 (即`ByRef`， `ByVal`) 匹配。</span><span class="sxs-lookup"><span data-stu-id="2ada7-636">The parameter types of `L` each have a conversion to the type of the corresponding parameter type of `D`, and their modifiers (i.e. `ByRef`, `ByVal`) match.</span></span>

* <span data-ttu-id="2ada7-637">如果`D`是一个函数的返回类型`L`转换为的返回类型`D`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-637">If `D` is a function, the return type of `L` has a conversion to the return type of `D`.</span></span> <span data-ttu-id="2ada7-638">(如果`D`是一个子例程的返回值`L`将被忽略。)</span><span class="sxs-lookup"><span data-stu-id="2ada7-638">(If `D` is a subroutine, the return value of `L` is ignored.)</span></span>

<span data-ttu-id="2ada7-639">如果参数的类型参数`L`省略，则中的相应参数的类型`D`推断; 如果参数的`L`具有数组或名称可为 null 的修饰符，导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-639">If the parameter type of a parameter of `L` is omitted, then the type of the corresponding parameter in `D` is inferred; if the parameter of `L` has array or nullable name modifiers, a compile-time error results.</span></span> <span data-ttu-id="2ada7-640">一次所有的参数类型`L`都可用，则推断 lambda 方法中的表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-640">Once all of the parameter types of `L` are available, then the type of the expression in the lambda method is inferred.</span></span> <span data-ttu-id="2ada7-641">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-641">For example:</span></span>

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

<span data-ttu-id="2ada7-642">在某些情况下，其中委托签名不 lambda 方法或方法签名完全匹配，.NET Framework 可能不支持委托创建本机。</span><span class="sxs-lookup"><span data-stu-id="2ada7-642">In some situations where delegate signature does not exactly match the lambda method or method signature, the .NET Framework may not support the delegate creation natively.</span></span> <span data-ttu-id="2ada7-643">在这种情况下，lambda 方法表达式用于匹配两种方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-643">In that situation, a lambda method expression is used to match the two methods.</span></span> <span data-ttu-id="2ada7-644">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-644">For example:</span></span>

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

<span data-ttu-id="2ada7-645">委托创建表达式的结果是一个委托实例，从方法指针表达式是指与相关联的目标表达式 （如果有） 匹配的方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-645">The result of a delegate-creation expression is a delegate instance that refers to the matching method with the associated target expression (if any) from the method pointer expression.</span></span> <span data-ttu-id="2ada7-646">如果目标表达式的类型为值类型，则值类型被复制到系统堆上因为委托仅指向堆上对象的方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-646">If the target expression is typed as a value type, then the value type is copied onto the system heap because a delegate can only point to a method of an object on the heap.</span></span> <span data-ttu-id="2ada7-647">方法和委托所引用的对象的委托的整个生存期内保持常量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-647">The method and object to which a delegate refers remain constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="2ada7-648">换而言之，不能创建它后更改目标或委托的对象。</span><span class="sxs-lookup"><span data-stu-id="2ada7-648">In other words, it is not possible to change the target or object of a delegate after it has been created.</span></span>

### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="2ada7-649">匿名对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-649">Anonymous Object-Creation Expressions</span></span>

<span data-ttu-id="2ada7-650">具有成员初始值设定项的对象创建表达式还可以完全忽略类型名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-650">An object-creation expression with member initializers can also omit the type name entirely.</span></span>

```antlr
AnonymousObjectCreationExpression
    : 'New' ObjectMemberInitializer
    ;
```

<span data-ttu-id="2ada7-651">在这种情况下，匿名类型被构造基于的类型和初始化表达式的一部分的成员的名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-651">In that case, an anonymous type is constructed based on the types and names of the members initialized as a part of the expression.</span></span> <span data-ttu-id="2ada7-652">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-652">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim Customer = New With { .Name = "John Smith", .Age = 34 }

        Console.WriteLine(Customer.Name)
    End Sub
End Module
```

<span data-ttu-id="2ada7-653">创建由匿名对象创建表达式的类型是没有名称，直接继承的类`Object`，，具有一组具有相同的名称分配给成员初始值设定项列表中的成员的属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-653">The type created by an anonymous object-creation expression is a class that has no name, inherits directly from `Object`, and has a set of properties with the same name as the members assigned to in the member initializer list.</span></span> <span data-ttu-id="2ada7-654">使用相同的规则作为本地变量的类型推理来推断每个属性的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-654">The type of each property is inferred using the same rules as local variable type inference.</span></span> <span data-ttu-id="2ada7-655">生成的匿名类型还重写`ToString`，返回所有成员及其值的字符串表示形式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-655">Generated anonymous types also override `ToString`, returning a string representation of all members and their values.</span></span> <span data-ttu-id="2ada7-656">（此字符串的确切格式超出了本规范的范畴是）。</span><span class="sxs-lookup"><span data-stu-id="2ada7-656">(The exact format of this string is beyond the scope of this specification).</span></span>

<span data-ttu-id="2ada7-657">默认情况下，生成的匿名类型的属性是读写。</span><span class="sxs-lookup"><span data-stu-id="2ada7-657">By default, the properties generated by the anonymous type are read-write.</span></span> <span data-ttu-id="2ada7-658">可以使用将标记为只读的匿名类型属性`Key`修饰符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-658">It is possible to mark an anonymous type property as read-only by using the `Key` modifier.</span></span> <span data-ttu-id="2ada7-659">`Key`修饰符指定该字段可用于唯一地标识匿名类型表示的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-659">The `Key` modifier specifies that the field can be used to uniquely identify the value the anonymous type represents.</span></span> <span data-ttu-id="2ada7-660">除了使该属性只读的它还会导致匿名类型来重写`Equals`并`GetHashCode`和实现接口`System.IEquatable(Of T)`(匿名类型中用于填充`T`)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-660">In addition to making the property read-only, it also causes the anonymous type to override `Equals` and  `GetHashCode` and to implement the interface `System.IEquatable(Of T)` (filling in the anonymous type for `T`).</span></span> <span data-ttu-id="2ada7-661">成员定义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-661">The members are defined as follows:</span></span>

<span data-ttu-id="2ada7-662">`Function Equals(obj As Object) As Boolean` 并`Function Equals(val As T) As Boolean`实现可以通过验证的两个实例都属于相同类型和并将每个比较`Key`使用成员`Object.Equals`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-662">`Function Equals(obj As Object) As Boolean` and `Function Equals(val As T) As Boolean` are implemented by validating that the two instances are of the same type and then comparing each `Key` member using `Object.Equals`.</span></span> <span data-ttu-id="2ada7-663">如果所有`Key`成员是否相等，然后`Equals`返回`True`; 否则为`Equals`返回`False`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-663">If all `Key` members are equal, then `Equals` returns `True`, otherwise `Equals` returns `False`.</span></span>

<span data-ttu-id="2ada7-664">`Function GetHashCode() As Integer` 实现这样，如果`Equals`为 true 的两个实例的匿名类型，则`GetHashCode`将返回相同的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-664">`Function GetHashCode() As Integer` is implemented such that that if `Equals` is true for two instances of the anonymous type, then `GetHashCode` will return the same value.</span></span> <span data-ttu-id="2ada7-665">哈希启动带有种子值，然后为每个`Key`成员，按顺序相乘 31 的哈希，并添加`Key`成员的哈希值 (由提供`GetHashCode`) 如果成员不是引用类型或值为可以为null值类型`Nothing`.</span><span class="sxs-lookup"><span data-stu-id="2ada7-665">The hash starts with a seed value and then, for each `Key` member, in order multiplies the hash by 31 and adds the `Key` member's hash value (provided by `GetHashCode`) if the member is not a reference type or nullable value type with the value of `Nothing`.</span></span>

<span data-ttu-id="2ada7-666">例如，在语句中创建的类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-666">For example, the type created in the statement:</span></span>

```vb
Dim zipState = New With { Key .ZipCode = 98112, .State = "WA" }
```

<span data-ttu-id="2ada7-667">创建一个类，如下所示大约 （尽管确切的实现可能会有所不同）：</span><span class="sxs-lookup"><span data-stu-id="2ada7-667">creates a class that looks approximately like this (although exact implementation may vary):</span></span>

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

<span data-ttu-id="2ada7-668">若要简化从另一种类型的字段创建了匿名类型的情况，可以直接从以下情况中的表达式推断字段名称：</span><span class="sxs-lookup"><span data-stu-id="2ada7-668">To simplify the situation where an anonymous type is created from the fields of another type, field names can be inferred directly from expressions in the following cases:</span></span>

* <span data-ttu-id="2ada7-669">简单名称表达式`x`推断名称`x`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-669">A simple name expression `x` infers the name `x`.</span></span>

* <span data-ttu-id="2ada7-670">成员访问表达式`x.y`推断名称`y`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-670">A member access expression `x.y` infers the name `y`.</span></span>

* <span data-ttu-id="2ada7-671">字典查找表达式`x!y`推断名称`y`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-671">A dictionary lookup expression `x!y` infers the name `y`.</span></span>

* <span data-ttu-id="2ada7-672">不带任何参数的调用或索引表达式`x()`推断名称`x`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-672">An invocation or index expression with no arguments `x()` infers the name `x`.</span></span>

* <span data-ttu-id="2ada7-673">XML 成员访问表达式`x.<y>`， `x...<y>`，`x.@y`推断名称`y`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-673">An XML member access expression `x.<y>`, `x...<y>`, `x.@y` infers the name `y`.</span></span>

* <span data-ttu-id="2ada7-674">为目标的成员访问表达式的 XML 成员访问表达式`x.<y>.z`推断名称`z`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-674">An XML member access expression that is the target of a member access expression `x.<y>.z` infers the name `z`.</span></span>

* <span data-ttu-id="2ada7-675">为不带任何参数调用或索引表达式的目标的 XML 成员访问表达式`x.<y>.z()`推断名称`z`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-675">An XML member access expression that is the target of an invocation or index expression with no arguments `x.<y>.z()` infers the name `z`.</span></span>

* <span data-ttu-id="2ada7-676">为目标的一个调用或索引表达式的 XML 成员访问表达式`x.<y>(0)`推断名称`y`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-676">An XML member access expression that is the target of an invocation or index expression `x.<y>(0)` infers the name `y`.</span></span>

<span data-ttu-id="2ada7-677">初始值设定项被解释为赋值的表达式推断名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-677">The initializer is interpreted as an assignment of the expression to the inferred name.</span></span> <span data-ttu-id="2ada7-678">例如，以下初始值设定项是等效的：</span><span class="sxs-lookup"><span data-stu-id="2ada7-678">For example, the following initializers are equivalent:</span></span>

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

<span data-ttu-id="2ada7-679">如果成员名称推断出的与冲突的现有成员的类型，如`GetHashCode`，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-679">If a member name is inferred that conflicts with an existing member of the type, such as `GetHashCode`, then a compile time error occurs.</span></span> <span data-ttu-id="2ada7-680">与常规成员初始值设定项，不同匿名对象创建表达式不允许成员初始值设定项具有循环引用，或之前已初始化引用成员。</span><span class="sxs-lookup"><span data-stu-id="2ada7-680">Unlike regular member initializers, anonymous object-creation expressions do not allow member initializers to have circular references, or to refer to a member before it has been initialized.</span></span> <span data-ttu-id="2ada7-681">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-681">For example:</span></span>

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

<span data-ttu-id="2ada7-682">如果两个匿名类创建表达式相同的方法内发生，并且如果的属性顺序、 属性名称和属性类型的所有匹配项-产生相同结果的形状-它们将同时引用同一个匿名类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-682">If two anonymous class creation expressions occur within the same method and yield the same resulting shape -- if the property order, property names, and property types all match -- they will both refer to the same anonymous class.</span></span> <span data-ttu-id="2ada7-683">实例或共享的成员变量的初始值设定项的方法作用域是在其中的变量进行初始化的构造函数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-683">The method scope of an instance or shared member variable with an initializer is the constructor in which the variable is initialized.</span></span>

<span data-ttu-id="2ada7-684">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-684">__Note.__</span></span> <span data-ttu-id="2ada7-685">可以一个编译器可以选择统一匿名类型进一步，例如在程序集级别，但这不能依赖这一次。</span><span class="sxs-lookup"><span data-stu-id="2ada7-685">It is possible that a compiler may choose to unify anonymous types further, such as at the assembly level, but this cannot be relied upon at this time.</span></span>


## <a name="cast-expressions"></a><span data-ttu-id="2ada7-686">强制转换表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-686">Cast Expressions</span></span>

<span data-ttu-id="2ada7-687">强制转换表达式强制转换为给定类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-687">A cast expression coerces an expression to a given type.</span></span> <span data-ttu-id="2ada7-688">特定强制转换关键字强制转换为基元类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-688">Specific cast keywords coerce expressions into the primitive types.</span></span> <span data-ttu-id="2ada7-689">三个常规转换关键字， `CType`，`TryCast`和`DirectCast`，转换为类型的表达式强制转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-689">Three general cast keywords, `CType`, `TryCast` and `DirectCast`, coerce an expression into a type.</span></span>

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

<span data-ttu-id="2ada7-690">`DirectCast` 和`TryCast`有特殊的行为。</span><span class="sxs-lookup"><span data-stu-id="2ada7-690">`DirectCast` and `TryCast` have special behaviors.</span></span> <span data-ttu-id="2ada7-691">因此，它们仅支持本机的转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-691">Because of this, they only support native conversions.</span></span> <span data-ttu-id="2ada7-692">此外中的目标类型`TryCast`表达式不能是值类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-692">Additionally, the target type in a `TryCast` expression cannot be a value type.</span></span> <span data-ttu-id="2ada7-693">用户定义的转换运算符时不考虑`DirectCast`或`TryCast`使用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-693">User-defined conversion operators are not considered when `DirectCast` or `TryCast` is used.</span></span> <span data-ttu-id="2ada7-694">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-694">(__Note.__</span></span> <span data-ttu-id="2ada7-695">转换设置`DirectCast`和`TryCast`支持受到限制，因为它们实现"本机 CLR"的转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-695">The conversion set that `DirectCast` and `TryCast` support are restricted because they implement "native CLR" conversions.</span></span> <span data-ttu-id="2ada7-696">目的`DirectCast`是提供"取消装箱"指令时的用途的功能`TryCast`是提供"isinst"指令的功能。</span><span class="sxs-lookup"><span data-stu-id="2ada7-696">The purpose of `DirectCast` is to provide the functionality of the "unbox" instruction, while the purpose of `TryCast` is to provide the functionality of the "isinst" instruction.</span></span> <span data-ttu-id="2ada7-697">因为它们映射到 CLR 指令，支持不直接支持的 CLR 的转换，将无法实现的预期的用途。）</span><span class="sxs-lookup"><span data-stu-id="2ada7-697">Since they map onto CLR instructions, supporting conversions not directly supported by the CLR would defeat the intended purpose.)</span></span>

<span data-ttu-id="2ada7-698">`DirectCast` 将转换为键入的表达式`Object`处理方式不同于`CType`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-698">`DirectCast` converts expressions that are typed as `Object` differently than `CType`.</span></span> <span data-ttu-id="2ada7-699">转换类型的表达式时`Object`其运行时类型是基元值类型，`DirectCast`将引发`System.InvalidCastException`异常，如果指定的类型不是表达式的运行时类型相同或`System.NullReferenceException`如果表达式计算结果为`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-699">When converting an expression of type `Object` whose run-time type is a primitive value type, `DirectCast` throws a `System.InvalidCastException` exception if the specified type is not the same as the run-time type of the expression or a `System.NullReferenceException` if the expression evaluates to `Nothing`.</span></span> <span data-ttu-id="2ada7-700">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-700">(__Note.__</span></span> <span data-ttu-id="2ada7-701">如上所述，`DirectCast`映射到 CLR 指令直接"取消装箱"当表达式的类型为`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-701">As noted above, `DirectCast` maps directly onto the CLR instruction "unbox" when the type of the expression is `Object`.</span></span> <span data-ttu-id="2ada7-702">与此相反，`CType`将转换为运行时帮助器调用以执行转换，以便可以支持基元类型之间进行转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-702">In contrast, `CType` turns into a call to a runtime helper to do the conversion so that conversions between primitive types can be supported.</span></span> <span data-ttu-id="2ada7-703">在这种情况时`Object`表达式要转换为基元值类型和实际的实例匹配的类型的目标类型，`DirectCast`将明显快于`CType`。)</span><span class="sxs-lookup"><span data-stu-id="2ada7-703">In the case when an `Object` expression is being converted to a primitive value type and the type of the actual instance match the target type, `DirectCast` will be significantly faster than `CType`.)</span></span>

<span data-ttu-id="2ada7-704">`TryCast` 将表达式转换，但不会引发异常，如果不能转换为目标类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-704">`TryCast` converts expressions but does not throw an exception if the expression cannot be converted to the target type.</span></span> <span data-ttu-id="2ada7-705">相反，`TryCast`将导致`Nothing`如果不能在运行时转换的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-705">Instead, `TryCast` will result in `Nothing` if the expression cannot be converted at runtime.</span></span> <span data-ttu-id="2ada7-706">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-706">(__Note.__</span></span> <span data-ttu-id="2ada7-707">如上所述，`TryCast`映射直接到 CLR 指令"isinst"。</span><span class="sxs-lookup"><span data-stu-id="2ada7-707">As noted above, `TryCast` maps directly onto the CLR instruction "isinst".</span></span> <span data-ttu-id="2ada7-708">通过组合类型检查和转换为单个操作中，`TryCast`可以是这样做比便宜`TypeOf ... Is`，然后`CType`。)</span><span class="sxs-lookup"><span data-stu-id="2ada7-708">By combining the type check and the conversion into a single operation, `TryCast` can be cheaper than doing a `TypeOf ... Is` and then a `CType`.)</span></span>

<span data-ttu-id="2ada7-709">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-709">For example:</span></span>

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

<span data-ttu-id="2ada7-710">如果不存在从转换表达式的类型为指定的类型，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-710">If no conversion exists from the type of the expression to the specified type, a compile-time error occurs.</span></span> <span data-ttu-id="2ada7-711">否则为该表达式分类为一个值，结果是由转换生成的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-711">Otherwise, the expression is classified as a value and the result is the value produced by the conversion.</span></span>


## <a name="operator-expressions"></a><span data-ttu-id="2ada7-712">运算符表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-712">Operator Expressions</span></span>

<span data-ttu-id="2ada7-713">有两种类型的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-713">There are two kinds of operators.</span></span> <span data-ttu-id="2ada7-714">*一元运算符*采用一个操作数，并使用前缀表示法 (例如， `-x`)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-714">*Unary operators* take one operand and use prefix notation (for example, `-x`).</span></span> <span data-ttu-id="2ada7-715">*二元运算符*采用两个操作数，并使用中缀表示法 (例如， `x + y`)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-715">*Binary operators* take two operands and use infix notation (for example, `x + y`).</span></span> <span data-ttu-id="2ada7-716">关系运算符，但这始终导致`Boolean`，为特定类型会导致该类型定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-716">With the exception of the relational operators, which always result in `Boolean`, an operator defined for a particular type results in that type.</span></span> <span data-ttu-id="2ada7-717">运算符的操作数必须始终分类为一个值;运算符表达式的结果分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-717">The operands to an operator must always be classified as a value; the result of an operator expression is classified as a value.</span></span>

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

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="2ada7-718">运算符优先级和关联性</span><span class="sxs-lookup"><span data-stu-id="2ada7-718">Operator Precedence and Associativity</span></span>

<span data-ttu-id="2ada7-719">如果表达式包含多个二进制运算符*优先*运算符的控制单个二进制运算符的计算顺序。</span><span class="sxs-lookup"><span data-stu-id="2ada7-719">When an expression contains multiple binary operators, the *precedence* of the operators controls the order in which the individual binary operators are evaluated.</span></span> <span data-ttu-id="2ada7-720">例如，表达式 `x + y * z` 相当于计算 `x + (y * z)`，因为 `*` 运算符的优先级高于 `+` 运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-720">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span> <span data-ttu-id="2ada7-721">下表列出了按降序的优先顺序的二进制运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-721">The following table lists the binary operators in descending order of precedence:</span></span>


| <span data-ttu-id="2ada7-722">__类别__</span><span class="sxs-lookup"><span data-stu-id="2ada7-722">__Category__</span></span>     | <span data-ttu-id="2ada7-723">__运算符__</span><span class="sxs-lookup"><span data-stu-id="2ada7-723">__Operators__</span></span>                                          | 
|------------------|--------------------------------------------------------|
| <span data-ttu-id="2ada7-724">基本</span><span class="sxs-lookup"><span data-stu-id="2ada7-724">Primary</span></span>          | <span data-ttu-id="2ada7-725">所有非运算符表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-725">All non-operator expressions</span></span>                           |
| <span data-ttu-id="2ada7-726">Await</span><span class="sxs-lookup"><span data-stu-id="2ada7-726">Await</span></span>            | `Await`                                                |
| <span data-ttu-id="2ada7-727">求幂</span><span class="sxs-lookup"><span data-stu-id="2ada7-727">Exponentiation</span></span>   | `^`                                                    |
| <span data-ttu-id="2ada7-728">一元求反</span><span class="sxs-lookup"><span data-stu-id="2ada7-728">Unary negation</span></span>   | <span data-ttu-id="2ada7-729">`+`， `-`</span><span class="sxs-lookup"><span data-stu-id="2ada7-729">`+`, `-`</span></span>                                               |
| <span data-ttu-id="2ada7-730">乘法</span><span class="sxs-lookup"><span data-stu-id="2ada7-730">Multiplicative</span></span>   | <span data-ttu-id="2ada7-731">`*`， `/`</span><span class="sxs-lookup"><span data-stu-id="2ada7-731">`*`, `/`</span></span>                                               |
| <span data-ttu-id="2ada7-732">整数除法</span><span class="sxs-lookup"><span data-stu-id="2ada7-732">Integer division</span></span> | `\`                                                    |
| <span data-ttu-id="2ada7-733">取模</span><span class="sxs-lookup"><span data-stu-id="2ada7-733">Modulus</span></span>          | `Mod`                                                  |
| <span data-ttu-id="2ada7-734">加法</span><span class="sxs-lookup"><span data-stu-id="2ada7-734">Additive</span></span>         | <span data-ttu-id="2ada7-735">`+`， `-`</span><span class="sxs-lookup"><span data-stu-id="2ada7-735">`+`, `-`</span></span>                                               |
| <span data-ttu-id="2ada7-736">串联</span><span class="sxs-lookup"><span data-stu-id="2ada7-736">Concatenation</span></span>    | `&`                                                    |
| <span data-ttu-id="2ada7-737">移位</span><span class="sxs-lookup"><span data-stu-id="2ada7-737">Shift</span></span>            | <span data-ttu-id="2ada7-738">`<<`， `>>`</span><span class="sxs-lookup"><span data-stu-id="2ada7-738">`<<`, `>>`</span></span>                                             |
| <span data-ttu-id="2ada7-739">关系</span><span class="sxs-lookup"><span data-stu-id="2ada7-739">Relational</span></span>       | <span data-ttu-id="2ada7-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span><span class="sxs-lookup"><span data-stu-id="2ada7-740">`=`, `<>`, `<`, `>`, `<=`, `>=`, `Like`, `Is`, `IsNot`</span></span> |
| <span data-ttu-id="2ada7-741">逻辑“非”</span><span class="sxs-lookup"><span data-stu-id="2ada7-741">Logical NOT</span></span>      | `Not`                                                  |
| <span data-ttu-id="2ada7-742">逻辑“与”</span><span class="sxs-lookup"><span data-stu-id="2ada7-742">Logical AND</span></span>      | <span data-ttu-id="2ada7-743">`And`， `AndAlso`</span><span class="sxs-lookup"><span data-stu-id="2ada7-743">`And`, `AndAlso`</span></span>                                       |
| <span data-ttu-id="2ada7-744">逻辑“或”</span><span class="sxs-lookup"><span data-stu-id="2ada7-744">Logical OR</span></span>       | <span data-ttu-id="2ada7-745">`Or`， `OrElse`</span><span class="sxs-lookup"><span data-stu-id="2ada7-745">`Or`, `OrElse`</span></span>                                         |
| <span data-ttu-id="2ada7-746">逻辑 XOR</span><span class="sxs-lookup"><span data-stu-id="2ada7-746">Logical XOR</span></span>      | `Xor`                                                  |

<span data-ttu-id="2ada7-747">如果表达式包含两个运算符具有相同优先级*关联性*决定执行的操作的顺序。</span><span class="sxs-lookup"><span data-stu-id="2ada7-747">When an expression contains two operators with the same precedence, the *associativity* of the operators controls the order in which the operations are performed.</span></span> <span data-ttu-id="2ada7-748">所有二元运算符是左结合，即操作执行从左到右。</span><span class="sxs-lookup"><span data-stu-id="2ada7-748">All binary operators are left-associative, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="2ada7-749">使用带括号的表达式可以控制优先级和结合性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-749">Precedence and associativity can be controlled using parenthetical expressions.</span></span>

### <a name="object-operands"></a><span data-ttu-id="2ada7-750">对象操作数</span><span class="sxs-lookup"><span data-stu-id="2ada7-750">Object Operands</span></span>

<span data-ttu-id="2ada7-751">除了常规的类型支持的每个运算符，所有运算符都支持类型的操作数`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-751">In addition to the regular types supported by each operator, all operators support operands of type `Object`.</span></span> <span data-ttu-id="2ada7-752">运算符应用于`Object`操作数进行处理同样上进行方法调用`Object`值： 可能选择的后期绑定方法调用，在这种情况下运行时类型的操作数，而不是编译时类型，确定有效性和操作的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-752">Operators applied to `Object` operands are handled similarly to method calls made on `Object` values: a late-bound method call might be chosen, in which case the run-time type of the operands, rather than the compile-time type, determines the validity and type of the operation.</span></span> <span data-ttu-id="2ada7-753">如果通过编译环境或通过指定严格的语义`Option Strict`，任何类型的操作数的运算符`Object`导致编译时错误，除了`TypeOf...Is`，`Is`和`IsNot`运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-753">If strict semantics are specified by the compilation environment or by `Option Strict`, any operators with operands of type `Object` cause a compile-time error, except for the `TypeOf...Is`, `Is` and `IsNot` operators.</span></span>

<span data-ttu-id="2ada7-754">当运算符决策确定后期绑定应执行操作时，操作的结果是操作数的运行时类型是否支持的运算符类型的操作数类型应用运算符的结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-754">When operator resolution determines that an operation should be performed late-bound, the outcome of the operation is the result of applying the operator to the operand types if the run-time types of the operands are types that are supported by the operator.</span></span> <span data-ttu-id="2ada7-755">值`Nothing`视为二元运算符表达式中另一个操作数的类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-755">The value `Nothing` is treated as the default value of the type of the other operand in a binary operator expression.</span></span> <span data-ttu-id="2ada7-756">在一元运算符表达式中，或如果两个操作数都`Nothing`在二元运算符表达式中，该操作的类型是`Integer`或唯一的运算符，如果该运算符不会导致的结果类型`Integer`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-756">In a unary operator expression, or if both operands are `Nothing` in a binary operator expression, the type of the operation is `Integer` or the only result type of the operator, if the operator does not result in `Integer`.</span></span> <span data-ttu-id="2ada7-757">操作的结果始终然后转换回`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-757">The result of the operation is always then cast back to `Object`.</span></span> <span data-ttu-id="2ada7-758">如果操作数类型有没有有效的运算符，`System.InvalidCastException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-758">If the operand types have no valid operator, a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="2ada7-759">在运行时转换已完成而不考虑它们是隐式或显式转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-759">Conversions at run time are done without regard to whether they are implicit or explicit.</span></span>

<span data-ttu-id="2ada7-760">如果数值的二元运算的结果将生成溢出异常 （无论整数溢出检查是否打开或关闭），然后结果类型被提升到下一步的更广泛数值类型，如有可能。</span><span class="sxs-lookup"><span data-stu-id="2ada7-760">If the result of a numeric binary operation would produce an overflow exception (regardless of whether integer overflow checking is on or off), then the result type is promoted to the next wider numeric type, if possible.</span></span> <span data-ttu-id="2ada7-761">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="2ada7-761">For example, consider the following code:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = CObj(CByte(2)) * CObj(CByte(255))

        Console.WriteLine(o.GetType().ToString() & " = " & o)
    End Sub
End Module
```

<span data-ttu-id="2ada7-762">它将打印以下结果：</span><span class="sxs-lookup"><span data-stu-id="2ada7-762">It prints the following result:</span></span>

```
System.Int16 = 512
```

<span data-ttu-id="2ada7-763">如果没有更广泛的数值类型可用来存储数，`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-763">If no wider numeric type is available to hold the number, a `System.OverflowException` exception is thrown.</span></span>

### <a name="operator-resolution"></a><span data-ttu-id="2ada7-764">运算符决策</span><span class="sxs-lookup"><span data-stu-id="2ada7-764">Operator Resolution</span></span>

<span data-ttu-id="2ada7-765">运算符解析给定运算符类型和操作数的一组，确定要使用的操作数的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-765">Given an operator type and a set of operands, operator resolution determines which operator to use for the operands.</span></span> <span data-ttu-id="2ada7-766">在解析运算符，则用户定义的运算符将被视为第一，使用以下步骤：</span><span class="sxs-lookup"><span data-stu-id="2ada7-766">When resolving operators, user-defined operators will be considered first, using the following steps:</span></span>

1. <span data-ttu-id="2ada7-767">首先，收集所有候选运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-767">First, all of the candidate operators are collected.</span></span> <span data-ttu-id="2ada7-768">候选运算符是所有用户定义类型的运算符的特定运算符中的源类型和所有目标类型中的特定类型的用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-768">The candidate operators are all of the user-defined operators of the particular operator type in the source type and all of the user-defined operators of the particular type in the target type.</span></span> <span data-ttu-id="2ada7-769">如果源类型和目标类型相关，常见的运算符是仅视为一次。</span><span class="sxs-lookup"><span data-stu-id="2ada7-769">If the source type and destination type are related, common operators are only considered once.</span></span>

2. <span data-ttu-id="2ada7-770">然后，重载决策被应用于要选择的最特定运算符的运算符和操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-770">Then, overload resolution is applied to the operators and operands to select the most specific operator.</span></span> <span data-ttu-id="2ada7-771">在二元运算符的情况下这可能会导致后期绑定调用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-771">In the case of binary operators, this may result in a late-bound call.</span></span>

<span data-ttu-id="2ada7-772">收集类型的候选运算符时`T?`，类型的运算符的`T`改为使用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-772">When collecting the candidate operators for a type `T?`, the operators of type `T` are used instead.</span></span> <span data-ttu-id="2ada7-773">任一`T`的用户定义还提升涉及只有不可为 null 的值类型的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-773">Any of `T`'s user-defined operators that involve only non-nullable value types are also lifted.</span></span> <span data-ttu-id="2ada7-774">提升的运算符使用的任何值类型，可以为 null 的版本与异常的返回类型`IsTrue`并`IsFalse`(它必须是`Boolean`)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-774">A lifted operator uses the nullable version of any value types, with the exception the return types of `IsTrue` and `IsFalse` (which must be `Boolean`).</span></span> <span data-ttu-id="2ada7-775">提升的运算符计算通过将操作数转换为其不可为 null 的版本，然后评估用户定义的运算符，然后将结果转换键入到其可以为 null 的版本。</span><span class="sxs-lookup"><span data-stu-id="2ada7-775">Lifted operators are evaluated by converting the operands to their non-nullable version, then evaluating the user-defined operator and then converting the result type to its nullable version.</span></span> <span data-ttu-id="2ada7-776">如果经不起考验操作数是`Nothing`，该表达式的结果是值为`Nothing`类型为可以为 null 的类型版本的结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-776">If ether operand is `Nothing`, the result of the expression is a value of `Nothing` typed as the nullable version of the result type.</span></span> <span data-ttu-id="2ada7-777">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-777">For example:</span></span>

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

<span data-ttu-id="2ada7-778">如果运算符是二元运算符，其中一个操作数是引用类型，还提升运算符，但任何绑定到该运算符会产生错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-778">If the operator is a binary operator and one of the operands is reference type, the operator is also lifted, but any binding to the operator produces an error.</span></span> <span data-ttu-id="2ada7-779">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-779">For example:</span></span>

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

<span data-ttu-id="2ada7-780">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-780">__Note.__</span></span> <span data-ttu-id="2ada7-781">此规则存在是因为我们想要在未来版本中，将用例在两种类型的二元运算符的情况下的行为中添加空传播引用类型是否已被考虑。</span><span class="sxs-lookup"><span data-stu-id="2ada7-781">This rule exists because there has been consideration whether we wish to add null-propagating reference types in a future version, in which case the behavior in the case of binary operators between the two types would change.</span></span>

<span data-ttu-id="2ada7-782">使用转换，如用户定义的运算符始终是首选通过提升运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-782">As with conversions, user-defined operators are always preferred over lifted operators.</span></span>

<span data-ttu-id="2ada7-783">在解析重载运算符，可在 Visual Basic 中定义的类和其他语言中定义的那些之间的差异：</span><span class="sxs-lookup"><span data-stu-id="2ada7-783">When resolving overloaded operators, there may be differences between classes defined in Visual Basic and those defined in other languages:</span></span>

* <span data-ttu-id="2ada7-784">在其他语言中， `Not`， `And`，和`Or`可能同时作为逻辑运算符和位运算符重载。</span><span class="sxs-lookup"><span data-stu-id="2ada7-784">In other languages, `Not`, `And`, and `Or` may be overloaded both as logical operators and bitwise operators.</span></span> <span data-ttu-id="2ada7-785">在从外部程序集导入时，这些运算符的任一形式接受为有效的重载。</span><span class="sxs-lookup"><span data-stu-id="2ada7-785">Upon import from an external assembly, either form is accepted as a valid overload for these operators.</span></span> <span data-ttu-id="2ada7-786">但是，对于类型，用于定义逻辑和位运算符，将视为仅按位的实现。</span><span class="sxs-lookup"><span data-stu-id="2ada7-786">However, for a type which defines both logical and bitwise operators, only the bitwise implementation will be considered.</span></span>

* <span data-ttu-id="2ada7-787">在其他语言中，`>>`和`<<`可能同时作为已签名的运算符和无符号的运算符重载。</span><span class="sxs-lookup"><span data-stu-id="2ada7-787">In other languages, `>>` and `<<` may be overloaded both as signed operators and unsigned operators.</span></span> <span data-ttu-id="2ada7-788">在从外部程序集导入时，两种形式接受为有效的重载。</span><span class="sxs-lookup"><span data-stu-id="2ada7-788">Upon import from an external assembly, either form is accepted as a valid overload.</span></span> <span data-ttu-id="2ada7-789">但是，定义了符号和无符号的运算符的类型，将视为仅有符号的实现。</span><span class="sxs-lookup"><span data-stu-id="2ada7-789">However, for a type which defines both signed and unsigned operators, only the signed implementation will be considered.</span></span>

* <span data-ttu-id="2ada7-790">如果没有用户定义的运算符是最具体到操作数，然后将被视为内部运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-790">If no user-defined operator is most specific to the operands, then intrinsic operators will be considered.</span></span> <span data-ttu-id="2ada7-791">如果没有内部运算符的操作数定义和其中一个操作数具有类型为 Object，则运算符将会解决后期绑定;否则，会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-791">If no intrinsic operator is defined for the operands and either operand has type Object then the operator will be resolved late-bound; otherwise,  a compile-time error results.</span></span>

<span data-ttu-id="2ada7-792">在以前版本的 Visual Basic 中，如果有且只有一个操作数的类型为 Object，和任何适用的用户定义运算符和任何适用的内部运算符，然后它时出现错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-792">In prior versions of Visual Basic, if there was exactly one operand of type Object, and no applicable user-defined operators, and no applicable intrinsic operators, then it was an error.</span></span> <span data-ttu-id="2ada7-793">截至 Visual Basic 11，它现在解析后期绑定。</span><span class="sxs-lookup"><span data-stu-id="2ada7-793">As of Visual Basic 11, it is now resolved late-bound.</span></span> <span data-ttu-id="2ada7-794">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-794">For example:</span></span>

```vb
Module Module1
  Sub Main()
      Dim p As Object = Nothing
      Dim U As New Uri("http://www.microsoft.com")
      Dim j = U * p  ' is now resolved late-bound
   End Sub
End Module
```

<span data-ttu-id="2ada7-795">一种类型`T`具有内部函数运算符还定义了该相同运算符`T?`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-795">A type `T` that has an intrinsic operator also defines that same operator for `T?`.</span></span> <span data-ttu-id="2ada7-796">在运算符的结果`T?`将与相同`T`，只不过其中一个操作数是`Nothing`，运算符的结果将是`Nothing`（即传播 null 值）。</span><span class="sxs-lookup"><span data-stu-id="2ada7-796">The result of the operator on `T?` will be the same as for `T`, except that if either operand is `Nothing`, the result of the operator will be `Nothing` (i.e. the null value is propagated).</span></span> <span data-ttu-id="2ada7-797">用于解析的操作的类型的目的`?`中删除具有它们的任何操作数，从确定操作的类型，和一个`?`如果任何操作数为 null 的值类型将添加到操作的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-797">For the purposes of resolving the type of an operation, the `?` is removed from any operands that have them, the type of the operation is determined, and a `?` is added to the type of the operation if any of the operands were nullable value types.</span></span> <span data-ttu-id="2ada7-798">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-798">For example:</span></span>

```vb
Dim v1? As Integer = 10
Dim v2 As Long = 20

' Type of operation will be Long?
Console.WriteLine(v1 + v2)
```

<span data-ttu-id="2ada7-799">每个运算符列出了为定义的内部类型和给定的操作数类型执行的操作的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-799">Each operator lists the intrinsic types it is defined for and the type of the operation performed given the operand types.</span></span> <span data-ttu-id="2ada7-800">类型的内部操作的结果都遵循以下通用规则：</span><span class="sxs-lookup"><span data-stu-id="2ada7-800">The result of type of a intrinsic operation follows these general rules:</span></span>

* <span data-ttu-id="2ada7-801">如果所有操作数都是相同的类型，并为类型定义运算符，然后会进行任何转换，并使用该类型的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-801">If all operands are of the same type, and the operator is defined for the type, then no conversion occurs and the operator for that type is used.</span></span>

* <span data-ttu-id="2ada7-802">使用以下步骤转换任何其类型未定义的运算符的操作数和运算符是根据新的类型解析：</span><span class="sxs-lookup"><span data-stu-id="2ada7-802">Any operand whose type is not defined for the operator is converted using the following steps and the operator is resolved against the new types:</span></span>

  * <span data-ttu-id="2ada7-803">操作数将转换为下一步为运算符和操作数定义的最宽类型和所隐式转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-803">The operand is converted to the next widest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="2ada7-804">如果不存在此类的类型，然后操作数将转换为下一步为运算符和操作数定义的最小类型和所隐式转换。</span><span class="sxs-lookup"><span data-stu-id="2ada7-804">If there is no such type, then the operand is converted to the next narrowest type that is defined for both the operator and the operand and to which it is implicitly convertible.</span></span>

  * <span data-ttu-id="2ada7-805">如果没有这样的类型，或者不能发生转换，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-805">If there is no such type or the conversion cannot occur, a compile-time error occurs.</span></span>

* <span data-ttu-id="2ada7-806">否则，将操作数转换为使用更广泛的操作数类型和该类型的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-806">Otherwise, the operands are converted to the wider of the operand types and the operator for that type is used.</span></span> <span data-ttu-id="2ada7-807">如果更窄的操作数类型不能隐式转换为更广泛的运算符类型，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-807">If the narrower operand type cannot be implicitly converted to the wider operator type, a compile-time error occurs.</span></span>

<span data-ttu-id="2ada7-808">尽管这些常规规则，但是，有许多特殊情况下调用运算符的结果表中。</span><span class="sxs-lookup"><span data-stu-id="2ada7-808">Despite these general rules, however, there are a number of special cases called out in the operator results tables.</span></span>

<span data-ttu-id="2ada7-809">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-809">__Note.__</span></span> <span data-ttu-id="2ada7-810">因格式原因而，运算符类型表将简化到其前两个字符的预定义的名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-810">For formatting reasons, the operator type tables abbreviate the predefined names to their first two characters.</span></span> <span data-ttu-id="2ada7-811">因此"通过"是`Byte`，"UI"是`UInteger`，"St"是`String`，等等。"Err"意味着没有为给定的操作数类型定义操作。</span><span class="sxs-lookup"><span data-stu-id="2ada7-811">So "By" is `Byte`, "UI" is `UInteger`, "St" is `String`, etc. "Err" means that there is no operation defined for the given operand types.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="2ada7-812">算术运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-812">Arithmetic Operators</span></span>

<span data-ttu-id="2ada7-813">`*`， `/`， `\`， `^`， `Mod`， `+`，以及`-`运算符是*算术运算符*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-813">The `*`, `/`, `\`, `^`, `Mod`, `+`, and `-` operators are the *arithmetic operators*.</span></span>

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

<span data-ttu-id="2ada7-814">可能会较高的精度比该操作的结果类型与执行浮点算术运算。</span><span class="sxs-lookup"><span data-stu-id="2ada7-814">Floating-point arithmetic operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="2ada7-815">例如，某些硬件体系结构支持"扩展"或"长双精度"浮点类型，具有更大的范围和精度比`Double`类型，并且隐式地执行所有使用此较高精度类型的浮点运算。</span><span class="sxs-lookup"><span data-stu-id="2ada7-815">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `Double` type, and implicitly perform all floating-point operations using this higher-precision type.</span></span> <span data-ttu-id="2ada7-816">可以进行硬件体系结构来执行浮点运算仅在开销过大的精度降低性能;而不是需要实现只好性能和精度，Visual Basic 允许使用所有浮点操作的更高精度类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-816">Hardware architectures can be made to perform floating-point operations with less precision only at excessive cost in performance; rather than require an implementation to forfeit both performance and precision, Visual Basic allows the higher-precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="2ada7-817">不提供更精确的结果，这很少会有任何显著的影响。</span><span class="sxs-lookup"><span data-stu-id="2ada7-817">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="2ada7-818">但是，在窗体的表达式`x * y / z`，其中该乘法运算生成结果超出`Double`范围，但后面的除法使临时结果返回到`Double`范围，该表达式是这一事实在较高范围中计算格式可能会导致有限生成而不是无限的结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-818">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `Double` range, but the subsequent division brings the temporary result back into the `Double` range, the fact that the expression is evaluated in a higher-range format may cause a finite result to be produced instead of infinity.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="2ada7-819">一元加号运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-819">Unary Plus Operator</span></span>

```antlr
UnaryPlusExpression
    : '+' Expression
    ;
```

<span data-ttu-id="2ada7-820">为定义一元加运算符`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Single`， `Double`，和`Decimal`类型.</span><span class="sxs-lookup"><span data-stu-id="2ada7-820">The unary plus operator is defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Single`, `Double`, and `Decimal` types.</span></span>

<span data-ttu-id="2ada7-821">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-821">__Operation Type:__</span></span>


| <span data-ttu-id="2ada7-822">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-822">__Bo__</span></span> | <span data-ttu-id="2ada7-823">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-823">__SB__</span></span> | <span data-ttu-id="2ada7-824">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-824">__By__</span></span> | <span data-ttu-id="2ada7-825">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-825">__Sh__</span></span> | <span data-ttu-id="2ada7-826">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-826">__US__</span></span> | <span data-ttu-id="2ada7-827">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-827">__In__</span></span> | <span data-ttu-id="2ada7-828">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-828">__UI__</span></span> | <span data-ttu-id="2ada7-829">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-829">__Lo__</span></span> | <span data-ttu-id="2ada7-830">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-830">__UL__</span></span> | <span data-ttu-id="2ada7-831">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-831">__De__</span></span> | <span data-ttu-id="2ada7-832">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-832">__Si__</span></span> | <span data-ttu-id="2ada7-833">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-833">__Do__</span></span> | <span data-ttu-id="2ada7-834">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-834">__Da__</span></span>  | <span data-ttu-id="2ada7-835">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-835">__Ch__</span></span>  | <span data-ttu-id="2ada7-836">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-836">__St__</span></span> | <span data-ttu-id="2ada7-837">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-837">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="2ada7-838">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-838">Sh</span></span> | <span data-ttu-id="2ada7-839">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-839">SB</span></span> | <span data-ttu-id="2ada7-840">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-840">By</span></span> | <span data-ttu-id="2ada7-841">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-841">Sh</span></span> | <span data-ttu-id="2ada7-842">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-842">US</span></span> | <span data-ttu-id="2ada7-843">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-843">In</span></span> | <span data-ttu-id="2ada7-844">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-844">UI</span></span> | <span data-ttu-id="2ada7-845">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-845">Lo</span></span> | <span data-ttu-id="2ada7-846">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-846">UL</span></span> | <span data-ttu-id="2ada7-847">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-847">De</span></span> | <span data-ttu-id="2ada7-848">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-848">Si</span></span> | <span data-ttu-id="2ada7-849">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-849">Do</span></span> | <span data-ttu-id="2ada7-850">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-850">Err</span></span> | <span data-ttu-id="2ada7-851">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-851">Err</span></span> | <span data-ttu-id="2ada7-852">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-852">Do</span></span> | <span data-ttu-id="2ada7-853">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-853">Ob</span></span> | 


### <a name="unary-minus-operator"></a><span data-ttu-id="2ada7-854">一元负运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-854">Unary Minus Operator</span></span>

```antlr
UnaryMinusExpression
    : '-' Expression
    ;
```

<span data-ttu-id="2ada7-855">一元负运算符定义为以下类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-855">The unary minus operator is defined for the following types:</span></span>

<span data-ttu-id="2ada7-856">`SByte`、 `Short`、 `Integer`和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-856">`SByte`, `Short`, `Integer`, and `Long`.</span></span> <span data-ttu-id="2ada7-857">通过减去从零开始的操作数计算结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-857">The result is computed by subtracting the operand from zero.</span></span> <span data-ttu-id="2ada7-858">如果整数溢出检查已打开并操作数的值是最大负`SByte`， `Short`， `Integer`，或`Long`、`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-858">If integer overflow checking is on and the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-859">否则为如果操作数的值为最大负`SByte`， `Short`， `Integer`，或`Long`，结果是该相同的值，则不会报告溢出。</span><span class="sxs-lookup"><span data-stu-id="2ada7-859">Otherwise, if the value of the operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, the result is that same value, and the overflow is not reported.</span></span>

<span data-ttu-id="2ada7-860">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-860">`Single` and `Double`.</span></span> <span data-ttu-id="2ada7-861">结果是操作数的符号相反，包括值 0 和无穷大的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-861">The result is the value of the operand with its sign inverted, including the values 0 and Infinity.</span></span> <span data-ttu-id="2ada7-862">如果操作数为 NaN，则结果也为 NaN。</span><span class="sxs-lookup"><span data-stu-id="2ada7-862">If the operand is NaN, the result is also NaN.</span></span>

<span data-ttu-id="2ada7-863">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-863">`Decimal`.</span></span> <span data-ttu-id="2ada7-864">通过减去从零开始的操作数计算结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-864">The result is computed by subtracting the operand from zero.</span></span>

<span data-ttu-id="2ada7-865">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-865">__Operation Type:__</span></span>

| <span data-ttu-id="2ada7-866">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-866">__Bo__</span></span> | <span data-ttu-id="2ada7-867">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-867">__SB__</span></span> | <span data-ttu-id="2ada7-868">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-868">__By__</span></span> | <span data-ttu-id="2ada7-869">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-869">__Sh__</span></span> | <span data-ttu-id="2ada7-870">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-870">__US__</span></span> | <span data-ttu-id="2ada7-871">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-871">__In__</span></span> | <span data-ttu-id="2ada7-872">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-872">__UI__</span></span> | <span data-ttu-id="2ada7-873">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-873">__Lo__</span></span> | <span data-ttu-id="2ada7-874">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-874">__UL__</span></span> | <span data-ttu-id="2ada7-875">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-875">__De__</span></span> | <span data-ttu-id="2ada7-876">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-876">__Si__</span></span> | <span data-ttu-id="2ada7-877">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-877">__Do__</span></span> | <span data-ttu-id="2ada7-878">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-878">__Da__</span></span>  | <span data-ttu-id="2ada7-879">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-879">__Ch__</span></span>  | <span data-ttu-id="2ada7-880">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-880">__St__</span></span> | <span data-ttu-id="2ada7-881">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-881">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="2ada7-882">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-882">Sh</span></span> | <span data-ttu-id="2ada7-883">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-883">SB</span></span> | <span data-ttu-id="2ada7-884">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-884">Sh</span></span> | <span data-ttu-id="2ada7-885">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-885">Sh</span></span> | <span data-ttu-id="2ada7-886">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-886">In</span></span> | <span data-ttu-id="2ada7-887">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-887">In</span></span> | <span data-ttu-id="2ada7-888">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-888">Lo</span></span> | <span data-ttu-id="2ada7-889">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-889">Lo</span></span> | <span data-ttu-id="2ada7-890">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-890">De</span></span> | <span data-ttu-id="2ada7-891">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-891">De</span></span> | <span data-ttu-id="2ada7-892">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-892">Si</span></span> | <span data-ttu-id="2ada7-893">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-893">Do</span></span> | <span data-ttu-id="2ada7-894">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-894">Err</span></span> | <span data-ttu-id="2ada7-895">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-895">Err</span></span> | <span data-ttu-id="2ada7-896">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-896">Do</span></span> | <span data-ttu-id="2ada7-897">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-897">Ob</span></span> | 


### <a name="addition-operator"></a><span data-ttu-id="2ada7-898">加法运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-898">Addition Operator</span></span>

<span data-ttu-id="2ada7-899">加法运算符计算两个操作数之和。</span><span class="sxs-lookup"><span data-stu-id="2ada7-899">The addition operator computes the sum of the two operands.</span></span>

```antlr
AdditionOperatorExpression
    : Expression '+' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-900">加法运算符定义为以下类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-900">The addition operator is defined for the following types:</span></span>

* <span data-ttu-id="2ada7-901">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-901">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="2ada7-902">如果整数溢出检查位于和为结果类型的范围之外`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-902">If integer overflow checking is on and the sum is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-903">否则为不报告溢出，并且结果的任何重要的高顺序位将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="2ada7-903">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="2ada7-904">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-904">`Single` and `Double`.</span></span> <span data-ttu-id="2ada7-905">根据 IEEE 754 算法的规则计算总和。</span><span class="sxs-lookup"><span data-stu-id="2ada7-905">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="2ada7-906">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-906">`Decimal`.</span></span> <span data-ttu-id="2ada7-907">如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-907">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-908">如果结果值过小，以十进制格式表示，则结果为 0。</span><span class="sxs-lookup"><span data-stu-id="2ada7-908">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="2ada7-909">`String`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-909">`String`.</span></span> <span data-ttu-id="2ada7-910">这两个`String`操作数会连接在一起。</span><span class="sxs-lookup"><span data-stu-id="2ada7-910">The two `String` operands are concatenated together.</span></span>

* <span data-ttu-id="2ada7-911">`Date`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-911">`Date`.</span></span> <span data-ttu-id="2ada7-912">`System.DateTime`类型定义重载的加法运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-912">The `System.DateTime` type defines overloaded addition operators.</span></span> <span data-ttu-id="2ada7-913">因为`System.DateTime`等效于内部`Date`类型，这些运算符，还可以在`Date`类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-913">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="2ada7-914">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-914">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-915">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-915">__Bo__</span></span> | <span data-ttu-id="2ada7-916">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-916">__SB__</span></span> | <span data-ttu-id="2ada7-917">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-917">__By__</span></span> | <span data-ttu-id="2ada7-918">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-918">__Sh__</span></span> | <span data-ttu-id="2ada7-919">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-919">__US__</span></span> | <span data-ttu-id="2ada7-920">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-920">__In__</span></span> | <span data-ttu-id="2ada7-921">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-921">__UI__</span></span> | <span data-ttu-id="2ada7-922">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-922">__Lo__</span></span> | <span data-ttu-id="2ada7-923">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-923">__UL__</span></span> | <span data-ttu-id="2ada7-924">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-924">__De__</span></span> | <span data-ttu-id="2ada7-925">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-925">__Si__</span></span> | <span data-ttu-id="2ada7-926">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-926">__Do__</span></span> | <span data-ttu-id="2ada7-927">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-927">__Da__</span></span>  | <span data-ttu-id="2ada7-928">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-928">__Ch__</span></span>  | <span data-ttu-id="2ada7-929">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-929">__St__</span></span> | <span data-ttu-id="2ada7-930">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-930">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="2ada7-931">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-931">__Bo__</span></span> | <span data-ttu-id="2ada7-932">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-932">Sh</span></span> | <span data-ttu-id="2ada7-933">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-933">SB</span></span> | <span data-ttu-id="2ada7-934">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-934">Sh</span></span> | <span data-ttu-id="2ada7-935">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-935">Sh</span></span> | <span data-ttu-id="2ada7-936">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-936">In</span></span> | <span data-ttu-id="2ada7-937">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-937">In</span></span> | <span data-ttu-id="2ada7-938">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-938">Lo</span></span> | <span data-ttu-id="2ada7-939">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-939">Lo</span></span> | <span data-ttu-id="2ada7-940">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-940">De</span></span> | <span data-ttu-id="2ada7-941">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-941">De</span></span> | <span data-ttu-id="2ada7-942">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-942">Si</span></span> | <span data-ttu-id="2ada7-943">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-943">Do</span></span> | <span data-ttu-id="2ada7-944">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-944">Err</span></span> | <span data-ttu-id="2ada7-945">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-945">Err</span></span> | <span data-ttu-id="2ada7-946">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-946">Do</span></span> | <span data-ttu-id="2ada7-947">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-947">Ob</span></span> | 
| <span data-ttu-id="2ada7-948">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-948">__SB__</span></span> |    | <span data-ttu-id="2ada7-949">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-949">SB</span></span> | <span data-ttu-id="2ada7-950">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-950">Sh</span></span> | <span data-ttu-id="2ada7-951">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-951">Sh</span></span> | <span data-ttu-id="2ada7-952">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-952">In</span></span> | <span data-ttu-id="2ada7-953">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-953">In</span></span> | <span data-ttu-id="2ada7-954">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-954">Lo</span></span> | <span data-ttu-id="2ada7-955">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-955">Lo</span></span> | <span data-ttu-id="2ada7-956">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-956">De</span></span> | <span data-ttu-id="2ada7-957">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-957">De</span></span> | <span data-ttu-id="2ada7-958">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-958">Si</span></span> | <span data-ttu-id="2ada7-959">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-959">Do</span></span> | <span data-ttu-id="2ada7-960">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-960">Err</span></span> | <span data-ttu-id="2ada7-961">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-961">Err</span></span> | <span data-ttu-id="2ada7-962">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-962">Do</span></span> | <span data-ttu-id="2ada7-963">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-963">Ob</span></span> | 
| <span data-ttu-id="2ada7-964">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-964">__By__</span></span> |    |    | <span data-ttu-id="2ada7-965">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-965">By</span></span> | <span data-ttu-id="2ada7-966">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-966">Sh</span></span> | <span data-ttu-id="2ada7-967">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-967">US</span></span> | <span data-ttu-id="2ada7-968">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-968">In</span></span> | <span data-ttu-id="2ada7-969">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-969">UI</span></span> | <span data-ttu-id="2ada7-970">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-970">Lo</span></span> | <span data-ttu-id="2ada7-971">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-971">UL</span></span> | <span data-ttu-id="2ada7-972">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-972">De</span></span> | <span data-ttu-id="2ada7-973">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-973">Si</span></span> | <span data-ttu-id="2ada7-974">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-974">Do</span></span> | <span data-ttu-id="2ada7-975">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-975">Err</span></span> | <span data-ttu-id="2ada7-976">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-976">Err</span></span> | <span data-ttu-id="2ada7-977">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-977">Do</span></span> | <span data-ttu-id="2ada7-978">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-978">Ob</span></span> | 
| <span data-ttu-id="2ada7-979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-979">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-980">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-980">Sh</span></span> | <span data-ttu-id="2ada7-981">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-981">In</span></span> | <span data-ttu-id="2ada7-982">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-982">In</span></span> | <span data-ttu-id="2ada7-983">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-983">Lo</span></span> | <span data-ttu-id="2ada7-984">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-984">Lo</span></span> | <span data-ttu-id="2ada7-985">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-985">De</span></span> | <span data-ttu-id="2ada7-986">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-986">De</span></span> | <span data-ttu-id="2ada7-987">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-987">Si</span></span> | <span data-ttu-id="2ada7-988">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-988">Do</span></span> | <span data-ttu-id="2ada7-989">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-989">Err</span></span> | <span data-ttu-id="2ada7-990">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-990">Err</span></span> | <span data-ttu-id="2ada7-991">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-991">Do</span></span> | <span data-ttu-id="2ada7-992">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-992">Ob</span></span> | 
| <span data-ttu-id="2ada7-993">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-993">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-994">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-994">US</span></span> | <span data-ttu-id="2ada7-995">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-995">In</span></span> | <span data-ttu-id="2ada7-996">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-996">UI</span></span> | <span data-ttu-id="2ada7-997">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-997">Lo</span></span> | <span data-ttu-id="2ada7-998">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-998">UL</span></span> | <span data-ttu-id="2ada7-999">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-999">De</span></span> | <span data-ttu-id="2ada7-1000">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1000">Si</span></span> | <span data-ttu-id="2ada7-1001">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1001">Do</span></span> | <span data-ttu-id="2ada7-1002">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1002">Err</span></span> | <span data-ttu-id="2ada7-1003">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1003">Err</span></span> | <span data-ttu-id="2ada7-1004">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1004">Do</span></span> | <span data-ttu-id="2ada7-1005">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1005">Ob</span></span> | 
| <span data-ttu-id="2ada7-1006">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1006">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-1007">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1007">In</span></span> | <span data-ttu-id="2ada7-1008">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1008">Lo</span></span> | <span data-ttu-id="2ada7-1009">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1009">Lo</span></span> | <span data-ttu-id="2ada7-1010">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1010">De</span></span> | <span data-ttu-id="2ada7-1011">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1011">De</span></span> | <span data-ttu-id="2ada7-1012">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1012">Si</span></span> | <span data-ttu-id="2ada7-1013">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1013">Do</span></span> | <span data-ttu-id="2ada7-1014">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1014">Err</span></span> | <span data-ttu-id="2ada7-1015">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1015">Err</span></span> | <span data-ttu-id="2ada7-1016">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1016">Do</span></span> | <span data-ttu-id="2ada7-1017">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1017">Ob</span></span> | 
| <span data-ttu-id="2ada7-1018">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1018">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-1019">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1019">UI</span></span> | <span data-ttu-id="2ada7-1020">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1020">Lo</span></span> | <span data-ttu-id="2ada7-1021">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1021">UL</span></span> | <span data-ttu-id="2ada7-1022">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1022">De</span></span> | <span data-ttu-id="2ada7-1023">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1023">Si</span></span> | <span data-ttu-id="2ada7-1024">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1024">Do</span></span> | <span data-ttu-id="2ada7-1025">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1025">Err</span></span> | <span data-ttu-id="2ada7-1026">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1026">Err</span></span> | <span data-ttu-id="2ada7-1027">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1027">Do</span></span> | <span data-ttu-id="2ada7-1028">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1028">Ob</span></span> | 
| <span data-ttu-id="2ada7-1029">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1029">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1030">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1030">Lo</span></span> | <span data-ttu-id="2ada7-1031">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1031">De</span></span> | <span data-ttu-id="2ada7-1032">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1032">De</span></span> | <span data-ttu-id="2ada7-1033">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1033">Si</span></span> | <span data-ttu-id="2ada7-1034">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1034">Do</span></span> | <span data-ttu-id="2ada7-1035">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1035">Err</span></span> | <span data-ttu-id="2ada7-1036">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1036">Err</span></span> | <span data-ttu-id="2ada7-1037">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1037">Do</span></span> | <span data-ttu-id="2ada7-1038">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1038">Ob</span></span> | 
| <span data-ttu-id="2ada7-1039">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1039">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1040">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1040">UL</span></span> | <span data-ttu-id="2ada7-1041">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1041">De</span></span> | <span data-ttu-id="2ada7-1042">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1042">Si</span></span> | <span data-ttu-id="2ada7-1043">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1043">Do</span></span> | <span data-ttu-id="2ada7-1044">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1044">Err</span></span> | <span data-ttu-id="2ada7-1045">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1045">Err</span></span> | <span data-ttu-id="2ada7-1046">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1046">Do</span></span> | <span data-ttu-id="2ada7-1047">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1047">Ob</span></span> | 
| <span data-ttu-id="2ada7-1048">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1048">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1049">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1049">De</span></span> | <span data-ttu-id="2ada7-1050">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1050">Si</span></span> | <span data-ttu-id="2ada7-1051">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1051">Do</span></span> | <span data-ttu-id="2ada7-1052">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1052">Err</span></span> | <span data-ttu-id="2ada7-1053">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1053">Err</span></span> | <span data-ttu-id="2ada7-1054">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1054">Do</span></span> | <span data-ttu-id="2ada7-1055">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1055">Ob</span></span> | 
| <span data-ttu-id="2ada7-1056">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1056">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1057">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1057">Si</span></span> | <span data-ttu-id="2ada7-1058">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1058">Do</span></span> | <span data-ttu-id="2ada7-1059">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1059">Err</span></span> | <span data-ttu-id="2ada7-1060">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1060">Err</span></span> | <span data-ttu-id="2ada7-1061">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1061">Do</span></span> | <span data-ttu-id="2ada7-1062">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1062">Ob</span></span> | 
| <span data-ttu-id="2ada7-1063">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1063">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1064">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1064">Do</span></span> | <span data-ttu-id="2ada7-1065">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1065">Err</span></span> | <span data-ttu-id="2ada7-1066">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1066">Err</span></span> | <span data-ttu-id="2ada7-1067">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1067">Do</span></span> | <span data-ttu-id="2ada7-1068">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1068">Ob</span></span> | 
| <span data-ttu-id="2ada7-1069">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1069">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1070">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-1070">St</span></span>  | <span data-ttu-id="2ada7-1071">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1071">Err</span></span> | <span data-ttu-id="2ada7-1072">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-1072">St</span></span> | <span data-ttu-id="2ada7-1073">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1073">Ob</span></span> | 
| <span data-ttu-id="2ada7-1074">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1074">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-1075">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-1075">St</span></span>  | <span data-ttu-id="2ada7-1076">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-1076">St</span></span> | <span data-ttu-id="2ada7-1077">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1077">Ob</span></span> | 
| <span data-ttu-id="2ada7-1078">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1078">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-1079">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-1079">St</span></span> | <span data-ttu-id="2ada7-1080">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1080">Ob</span></span> | 
| <span data-ttu-id="2ada7-1081">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1081">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="2ada7-1082">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1082">Ob</span></span> | 


### <a name="subtraction-operator"></a><span data-ttu-id="2ada7-1083">减法运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-1083">Subtraction Operator</span></span>

<span data-ttu-id="2ada7-1084">减法运算符中减去第二个操作数，从第一个操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1084">The subtraction operator subtracts the second operand from the first operand.</span></span>

```antlr
SubtractionOperatorExpression
    : Expression '-' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-1085">减法运算符定义为以下类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-1085">The subtraction operator is defined for the following types:</span></span>

* <span data-ttu-id="2ada7-1086">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1086">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="2ada7-1087">如果整数溢出检查已打开并之处在于结果类型的范围之外`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1087">If integer overflow checking is on and the difference is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1088">否则为不报告溢出，并且结果的任何重要的高顺序位将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1088">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="2ada7-1089">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1089">`Single` and `Double`.</span></span> <span data-ttu-id="2ada7-1090">根据 IEEE 754 算法的规则计算差。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1090">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="2ada7-1091">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1091">`Decimal`.</span></span> <span data-ttu-id="2ada7-1092">如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1092">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1093">如果结果值过小，以十进制格式表示，则结果为 0。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1093">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

* <span data-ttu-id="2ada7-1094">`Date`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1094">`Date`.</span></span> <span data-ttu-id="2ada7-1095">`System.DateTime`类型定义重载的减法运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1095">The `System.DateTime` type defines overloaded subtraction operators.</span></span> <span data-ttu-id="2ada7-1096">因为`System.DateTime`等效于内部`Date`类型，这些运算符，还可以在`Date`类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1096">Because `System.DateTime` is equivalent to the intrinsic `Date` type, these operators is also available on the `Date` type.</span></span>

<span data-ttu-id="2ada7-1097">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1097">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-1098">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1098">__Bo__</span></span> | <span data-ttu-id="2ada7-1099">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1099">__SB__</span></span> | <span data-ttu-id="2ada7-1100">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1100">__By__</span></span> | <span data-ttu-id="2ada7-1101">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1101">__Sh__</span></span> | <span data-ttu-id="2ada7-1102">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1102">__US__</span></span> | <span data-ttu-id="2ada7-1103">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1103">__In__</span></span> | <span data-ttu-id="2ada7-1104">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1104">__UI__</span></span> | <span data-ttu-id="2ada7-1105">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1105">__Lo__</span></span> | <span data-ttu-id="2ada7-1106">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1106">__UL__</span></span> | <span data-ttu-id="2ada7-1107">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1107">__De__</span></span> | <span data-ttu-id="2ada7-1108">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1108">__Si__</span></span> | <span data-ttu-id="2ada7-1109">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1109">__Do__</span></span> | <span data-ttu-id="2ada7-1110">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1110">__Da__</span></span>  | <span data-ttu-id="2ada7-1111">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1111">__Ch__</span></span>  | <span data-ttu-id="2ada7-1112">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1112">__St__</span></span> | <span data-ttu-id="2ada7-1113">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1113">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="2ada7-1114">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1114">__Bo__</span></span> | <span data-ttu-id="2ada7-1115">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1115">Sh</span></span> | <span data-ttu-id="2ada7-1116">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-1116">SB</span></span> | <span data-ttu-id="2ada7-1117">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1117">Sh</span></span> | <span data-ttu-id="2ada7-1118">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1118">Sh</span></span> | <span data-ttu-id="2ada7-1119">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1119">In</span></span> | <span data-ttu-id="2ada7-1120">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1120">In</span></span> | <span data-ttu-id="2ada7-1121">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1121">Lo</span></span> | <span data-ttu-id="2ada7-1122">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1122">Lo</span></span> | <span data-ttu-id="2ada7-1123">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1123">De</span></span> | <span data-ttu-id="2ada7-1124">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1124">De</span></span> | <span data-ttu-id="2ada7-1125">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1125">Si</span></span> | <span data-ttu-id="2ada7-1126">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1126">Do</span></span> | <span data-ttu-id="2ada7-1127">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1127">Err</span></span> | <span data-ttu-id="2ada7-1128">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1128">Err</span></span> | <span data-ttu-id="2ada7-1129">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1129">Do</span></span>  | <span data-ttu-id="2ada7-1130">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1130">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1131">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1131">__SB__</span></span> |    | <span data-ttu-id="2ada7-1132">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-1132">SB</span></span> | <span data-ttu-id="2ada7-1133">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1133">Sh</span></span> | <span data-ttu-id="2ada7-1134">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1134">Sh</span></span> | <span data-ttu-id="2ada7-1135">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1135">In</span></span> | <span data-ttu-id="2ada7-1136">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1136">In</span></span> | <span data-ttu-id="2ada7-1137">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1137">Lo</span></span> | <span data-ttu-id="2ada7-1138">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1138">Lo</span></span> | <span data-ttu-id="2ada7-1139">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1139">De</span></span> | <span data-ttu-id="2ada7-1140">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1140">De</span></span> | <span data-ttu-id="2ada7-1141">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1141">Si</span></span> | <span data-ttu-id="2ada7-1142">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1142">Do</span></span> | <span data-ttu-id="2ada7-1143">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1143">Err</span></span> | <span data-ttu-id="2ada7-1144">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1144">Err</span></span> | <span data-ttu-id="2ada7-1145">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1145">Do</span></span>  | <span data-ttu-id="2ada7-1146">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1146">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1147">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1147">__By__</span></span> |    |    | <span data-ttu-id="2ada7-1148">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-1148">By</span></span> | <span data-ttu-id="2ada7-1149">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1149">Sh</span></span> | <span data-ttu-id="2ada7-1150">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-1150">US</span></span> | <span data-ttu-id="2ada7-1151">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1151">In</span></span> | <span data-ttu-id="2ada7-1152">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1152">UI</span></span> | <span data-ttu-id="2ada7-1153">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1153">Lo</span></span> | <span data-ttu-id="2ada7-1154">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1154">UL</span></span> | <span data-ttu-id="2ada7-1155">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1155">De</span></span> | <span data-ttu-id="2ada7-1156">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1156">Si</span></span> | <span data-ttu-id="2ada7-1157">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1157">Do</span></span> | <span data-ttu-id="2ada7-1158">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1158">Err</span></span> | <span data-ttu-id="2ada7-1159">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1159">Err</span></span> | <span data-ttu-id="2ada7-1160">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1160">Do</span></span>  | <span data-ttu-id="2ada7-1161">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1161">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1162">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1162">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-1163">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1163">Sh</span></span> | <span data-ttu-id="2ada7-1164">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1164">In</span></span> | <span data-ttu-id="2ada7-1165">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1165">In</span></span> | <span data-ttu-id="2ada7-1166">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1166">Lo</span></span> | <span data-ttu-id="2ada7-1167">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1167">Lo</span></span> | <span data-ttu-id="2ada7-1168">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1168">De</span></span> | <span data-ttu-id="2ada7-1169">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1169">De</span></span> | <span data-ttu-id="2ada7-1170">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1170">Si</span></span> | <span data-ttu-id="2ada7-1171">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1171">Do</span></span> | <span data-ttu-id="2ada7-1172">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1172">Err</span></span> | <span data-ttu-id="2ada7-1173">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1173">Err</span></span> | <span data-ttu-id="2ada7-1174">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1174">Do</span></span>  | <span data-ttu-id="2ada7-1175">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1175">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1176">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1176">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-1177">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-1177">US</span></span> | <span data-ttu-id="2ada7-1178">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1178">In</span></span> | <span data-ttu-id="2ada7-1179">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1179">UI</span></span> | <span data-ttu-id="2ada7-1180">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1180">Lo</span></span> | <span data-ttu-id="2ada7-1181">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1181">UL</span></span> | <span data-ttu-id="2ada7-1182">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1182">De</span></span> | <span data-ttu-id="2ada7-1183">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1183">Si</span></span> | <span data-ttu-id="2ada7-1184">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1184">Do</span></span> | <span data-ttu-id="2ada7-1185">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1185">Err</span></span> | <span data-ttu-id="2ada7-1186">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1186">Err</span></span> | <span data-ttu-id="2ada7-1187">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1187">Do</span></span>  | <span data-ttu-id="2ada7-1188">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1188">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1189">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1189">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-1190">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1190">In</span></span> | <span data-ttu-id="2ada7-1191">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1191">Lo</span></span> | <span data-ttu-id="2ada7-1192">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1192">Lo</span></span> | <span data-ttu-id="2ada7-1193">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1193">De</span></span> | <span data-ttu-id="2ada7-1194">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1194">De</span></span> | <span data-ttu-id="2ada7-1195">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1195">Si</span></span> | <span data-ttu-id="2ada7-1196">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1196">Do</span></span> | <span data-ttu-id="2ada7-1197">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1197">Err</span></span> | <span data-ttu-id="2ada7-1198">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1198">Err</span></span> | <span data-ttu-id="2ada7-1199">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1199">Do</span></span>  | <span data-ttu-id="2ada7-1200">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1200">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1201">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1201">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-1202">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1202">UI</span></span> | <span data-ttu-id="2ada7-1203">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1203">Lo</span></span> | <span data-ttu-id="2ada7-1204">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1204">UL</span></span> | <span data-ttu-id="2ada7-1205">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1205">De</span></span> | <span data-ttu-id="2ada7-1206">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1206">Si</span></span> | <span data-ttu-id="2ada7-1207">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1207">Do</span></span> | <span data-ttu-id="2ada7-1208">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1208">Err</span></span> | <span data-ttu-id="2ada7-1209">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1209">Err</span></span> | <span data-ttu-id="2ada7-1210">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1210">Do</span></span>  | <span data-ttu-id="2ada7-1211">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1211">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1212">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1212">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1213">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1213">Lo</span></span> | <span data-ttu-id="2ada7-1214">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1214">De</span></span> | <span data-ttu-id="2ada7-1215">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1215">De</span></span> | <span data-ttu-id="2ada7-1216">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1216">Si</span></span> | <span data-ttu-id="2ada7-1217">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1217">Do</span></span> | <span data-ttu-id="2ada7-1218">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1218">Err</span></span> | <span data-ttu-id="2ada7-1219">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1219">Err</span></span> | <span data-ttu-id="2ada7-1220">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1220">Do</span></span>  | <span data-ttu-id="2ada7-1221">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1221">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1222">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1222">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1223">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1223">UL</span></span> | <span data-ttu-id="2ada7-1224">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1224">De</span></span> | <span data-ttu-id="2ada7-1225">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1225">Si</span></span> | <span data-ttu-id="2ada7-1226">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1226">Do</span></span> | <span data-ttu-id="2ada7-1227">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1227">Err</span></span> | <span data-ttu-id="2ada7-1228">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1228">Err</span></span> | <span data-ttu-id="2ada7-1229">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1229">Do</span></span>  | <span data-ttu-id="2ada7-1230">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1230">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1231">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1231">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1232">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1232">De</span></span> | <span data-ttu-id="2ada7-1233">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1233">Si</span></span> | <span data-ttu-id="2ada7-1234">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1234">Do</span></span> | <span data-ttu-id="2ada7-1235">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1235">Err</span></span> | <span data-ttu-id="2ada7-1236">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1236">Err</span></span> | <span data-ttu-id="2ada7-1237">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1237">Do</span></span>  | <span data-ttu-id="2ada7-1238">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1238">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1239">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1239">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1240">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1240">Si</span></span> | <span data-ttu-id="2ada7-1241">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1241">Do</span></span> | <span data-ttu-id="2ada7-1242">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1242">Err</span></span> | <span data-ttu-id="2ada7-1243">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1243">Err</span></span> | <span data-ttu-id="2ada7-1244">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1244">Do</span></span>  | <span data-ttu-id="2ada7-1245">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1245">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1246">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1246">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1247">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1247">Do</span></span> | <span data-ttu-id="2ada7-1248">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1248">Err</span></span> | <span data-ttu-id="2ada7-1249">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1249">Err</span></span> | <span data-ttu-id="2ada7-1250">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1250">Do</span></span>  | <span data-ttu-id="2ada7-1251">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1251">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1252">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1252">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1253">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1253">Err</span></span> | <span data-ttu-id="2ada7-1254">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1254">Err</span></span> | <span data-ttu-id="2ada7-1255">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1255">Err</span></span> | <span data-ttu-id="2ada7-1256">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1256">Err</span></span> | 
| <span data-ttu-id="2ada7-1257">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1257">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-1258">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1258">Err</span></span> | <span data-ttu-id="2ada7-1259">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1259">Err</span></span> | <span data-ttu-id="2ada7-1260">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1260">Err</span></span> | 
| <span data-ttu-id="2ada7-1261">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1261">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-1262">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1262">Do</span></span>  | <span data-ttu-id="2ada7-1263">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1263">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1264">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1264">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="2ada7-1265">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1265">Ob</span></span>  | 


### <a name="multiplication-operator"></a><span data-ttu-id="2ada7-1266">乘法运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-1266">Multiplication Operator</span></span>

<span data-ttu-id="2ada7-1267">乘法运算符计算两个操作数的乘积。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1267">The multiplication operator computes the product of two operands.</span></span>

```antlr
MultiplicationOperatorExpression
    : Expression '*' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-1268">乘法运算符定义为以下类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-1268">The multiplication operator is defined for the following types:</span></span>

* <span data-ttu-id="2ada7-1269">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1269">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="2ada7-1270">如果整数溢出检查，且该产品是结果类型的范围之外`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1270">If integer overflow checking is on and the product is outside the range of the result type, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1271">否则为不报告溢出，并且结果的任何重要的高顺序位将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1271">Otherwise, overflows are not reported, and any significant high-order bits of the result are discarded.</span></span>

* <span data-ttu-id="2ada7-1272">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1272">`Single` and `Double`.</span></span> <span data-ttu-id="2ada7-1273">根据 IEEE 754 算法的规则计算乘积。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1273">The product is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="2ada7-1274">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1274">`Decimal`.</span></span> <span data-ttu-id="2ada7-1275">如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1275">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1276">如果结果值过小，以十进制格式表示，则结果为 0。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1276">If the result value is too small to represent in the decimal format, the result is 0.</span></span>

<span data-ttu-id="2ada7-1277">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1277">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-1278">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1278">__Bo__</span></span> | <span data-ttu-id="2ada7-1279">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1279">__SB__</span></span> | <span data-ttu-id="2ada7-1280">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1280">__By__</span></span> | <span data-ttu-id="2ada7-1281">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1281">__Sh__</span></span> | <span data-ttu-id="2ada7-1282">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1282">__US__</span></span> | <span data-ttu-id="2ada7-1283">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1283">__In__</span></span> | <span data-ttu-id="2ada7-1284">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1284">__UI__</span></span> | <span data-ttu-id="2ada7-1285">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1285">__Lo__</span></span> | <span data-ttu-id="2ada7-1286">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1286">__UL__</span></span> | <span data-ttu-id="2ada7-1287">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1287">__De__</span></span> | <span data-ttu-id="2ada7-1288">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1288">__Si__</span></span> | <span data-ttu-id="2ada7-1289">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1289">__Do__</span></span> | <span data-ttu-id="2ada7-1290">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1290">__Da__</span></span>  | <span data-ttu-id="2ada7-1291">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1291">__Ch__</span></span>  | <span data-ttu-id="2ada7-1292">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1292">__St__</span></span> | <span data-ttu-id="2ada7-1293">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1293">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="2ada7-1294">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1294">__Bo__</span></span> | <span data-ttu-id="2ada7-1295">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1295">Sh</span></span> | <span data-ttu-id="2ada7-1296">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-1296">SB</span></span> | <span data-ttu-id="2ada7-1297">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1297">Sh</span></span> | <span data-ttu-id="2ada7-1298">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1298">Sh</span></span> | <span data-ttu-id="2ada7-1299">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1299">In</span></span> | <span data-ttu-id="2ada7-1300">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1300">In</span></span> | <span data-ttu-id="2ada7-1301">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1301">Lo</span></span> | <span data-ttu-id="2ada7-1302">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1302">Lo</span></span> | <span data-ttu-id="2ada7-1303">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1303">De</span></span> | <span data-ttu-id="2ada7-1304">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1304">De</span></span> | <span data-ttu-id="2ada7-1305">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1305">Si</span></span> | <span data-ttu-id="2ada7-1306">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1306">Do</span></span> | <span data-ttu-id="2ada7-1307">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1307">Err</span></span> | <span data-ttu-id="2ada7-1308">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1308">Err</span></span> | <span data-ttu-id="2ada7-1309">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1309">Do</span></span>  | <span data-ttu-id="2ada7-1310">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1310">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1311">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1311">__SB__</span></span> |    | <span data-ttu-id="2ada7-1312">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-1312">SB</span></span> | <span data-ttu-id="2ada7-1313">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1313">Sh</span></span> | <span data-ttu-id="2ada7-1314">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1314">Sh</span></span> | <span data-ttu-id="2ada7-1315">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1315">In</span></span> | <span data-ttu-id="2ada7-1316">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1316">In</span></span> | <span data-ttu-id="2ada7-1317">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1317">Lo</span></span> | <span data-ttu-id="2ada7-1318">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1318">Lo</span></span> | <span data-ttu-id="2ada7-1319">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1319">De</span></span> | <span data-ttu-id="2ada7-1320">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1320">De</span></span> | <span data-ttu-id="2ada7-1321">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1321">Si</span></span> | <span data-ttu-id="2ada7-1322">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1322">Do</span></span> | <span data-ttu-id="2ada7-1323">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1323">Err</span></span> | <span data-ttu-id="2ada7-1324">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1324">Err</span></span> | <span data-ttu-id="2ada7-1325">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1325">Do</span></span>  | <span data-ttu-id="2ada7-1326">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1326">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1327">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1327">__By__</span></span> |    |    | <span data-ttu-id="2ada7-1328">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-1328">By</span></span> | <span data-ttu-id="2ada7-1329">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1329">Sh</span></span> | <span data-ttu-id="2ada7-1330">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-1330">US</span></span> | <span data-ttu-id="2ada7-1331">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1331">In</span></span> | <span data-ttu-id="2ada7-1332">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1332">UI</span></span> | <span data-ttu-id="2ada7-1333">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1333">Lo</span></span> | <span data-ttu-id="2ada7-1334">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1334">UL</span></span> | <span data-ttu-id="2ada7-1335">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1335">De</span></span> | <span data-ttu-id="2ada7-1336">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1336">Si</span></span> | <span data-ttu-id="2ada7-1337">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1337">Do</span></span> | <span data-ttu-id="2ada7-1338">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1338">Err</span></span> | <span data-ttu-id="2ada7-1339">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1339">Err</span></span> | <span data-ttu-id="2ada7-1340">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1340">Do</span></span>  | <span data-ttu-id="2ada7-1341">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1341">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1342">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1342">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-1343">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1343">Sh</span></span> | <span data-ttu-id="2ada7-1344">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1344">In</span></span> | <span data-ttu-id="2ada7-1345">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1345">In</span></span> | <span data-ttu-id="2ada7-1346">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1346">Lo</span></span> | <span data-ttu-id="2ada7-1347">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1347">Lo</span></span> | <span data-ttu-id="2ada7-1348">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1348">De</span></span> | <span data-ttu-id="2ada7-1349">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1349">De</span></span> | <span data-ttu-id="2ada7-1350">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1350">Si</span></span> | <span data-ttu-id="2ada7-1351">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1351">Do</span></span> | <span data-ttu-id="2ada7-1352">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1352">Err</span></span> | <span data-ttu-id="2ada7-1353">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1353">Err</span></span> | <span data-ttu-id="2ada7-1354">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1354">Do</span></span>  | <span data-ttu-id="2ada7-1355">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1355">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1356">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1356">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-1357">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-1357">US</span></span> | <span data-ttu-id="2ada7-1358">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1358">In</span></span> | <span data-ttu-id="2ada7-1359">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1359">UI</span></span> | <span data-ttu-id="2ada7-1360">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1360">Lo</span></span> | <span data-ttu-id="2ada7-1361">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1361">UL</span></span> | <span data-ttu-id="2ada7-1362">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1362">De</span></span> | <span data-ttu-id="2ada7-1363">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1363">Si</span></span> | <span data-ttu-id="2ada7-1364">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1364">Do</span></span> | <span data-ttu-id="2ada7-1365">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1365">Err</span></span> | <span data-ttu-id="2ada7-1366">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1366">Err</span></span> | <span data-ttu-id="2ada7-1367">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1367">Do</span></span>  | <span data-ttu-id="2ada7-1368">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1368">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1369">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1369">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-1370">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1370">In</span></span> | <span data-ttu-id="2ada7-1371">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1371">Lo</span></span> | <span data-ttu-id="2ada7-1372">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1372">Lo</span></span> | <span data-ttu-id="2ada7-1373">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1373">De</span></span> | <span data-ttu-id="2ada7-1374">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1374">De</span></span> | <span data-ttu-id="2ada7-1375">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1375">Si</span></span> | <span data-ttu-id="2ada7-1376">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1376">Do</span></span> | <span data-ttu-id="2ada7-1377">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1377">Err</span></span> | <span data-ttu-id="2ada7-1378">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1378">Err</span></span> | <span data-ttu-id="2ada7-1379">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1379">Do</span></span>  | <span data-ttu-id="2ada7-1380">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1380">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1381">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1381">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-1382">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1382">UI</span></span> | <span data-ttu-id="2ada7-1383">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1383">Lo</span></span> | <span data-ttu-id="2ada7-1384">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1384">UL</span></span> | <span data-ttu-id="2ada7-1385">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1385">De</span></span> | <span data-ttu-id="2ada7-1386">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1386">Si</span></span> | <span data-ttu-id="2ada7-1387">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1387">Do</span></span> | <span data-ttu-id="2ada7-1388">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1388">Err</span></span> | <span data-ttu-id="2ada7-1389">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1389">Err</span></span> | <span data-ttu-id="2ada7-1390">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1390">Do</span></span>  | <span data-ttu-id="2ada7-1391">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1391">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1392">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1392">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1393">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1393">Lo</span></span> | <span data-ttu-id="2ada7-1394">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1394">De</span></span> | <span data-ttu-id="2ada7-1395">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1395">De</span></span> | <span data-ttu-id="2ada7-1396">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1396">Si</span></span> | <span data-ttu-id="2ada7-1397">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1397">Do</span></span> | <span data-ttu-id="2ada7-1398">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1398">Err</span></span> | <span data-ttu-id="2ada7-1399">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1399">Err</span></span> | <span data-ttu-id="2ada7-1400">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1400">Do</span></span>  | <span data-ttu-id="2ada7-1401">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1401">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1402">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1402">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1403">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1403">UL</span></span> | <span data-ttu-id="2ada7-1404">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1404">De</span></span> | <span data-ttu-id="2ada7-1405">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1405">Si</span></span> | <span data-ttu-id="2ada7-1406">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1406">Do</span></span> | <span data-ttu-id="2ada7-1407">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1407">Err</span></span> | <span data-ttu-id="2ada7-1408">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1408">Err</span></span> | <span data-ttu-id="2ada7-1409">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1409">Do</span></span>  | <span data-ttu-id="2ada7-1410">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1410">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1411">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1411">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1412">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1412">De</span></span> | <span data-ttu-id="2ada7-1413">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1413">Si</span></span> | <span data-ttu-id="2ada7-1414">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1414">Do</span></span> | <span data-ttu-id="2ada7-1415">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1415">Err</span></span> | <span data-ttu-id="2ada7-1416">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1416">Err</span></span> | <span data-ttu-id="2ada7-1417">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1417">Do</span></span>  | <span data-ttu-id="2ada7-1418">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1418">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1419">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1419">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1420">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1420">Si</span></span> | <span data-ttu-id="2ada7-1421">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1421">Do</span></span> | <span data-ttu-id="2ada7-1422">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1422">Err</span></span> | <span data-ttu-id="2ada7-1423">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1423">Err</span></span> | <span data-ttu-id="2ada7-1424">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1424">Do</span></span>  | <span data-ttu-id="2ada7-1425">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1425">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1426">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1426">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1427">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1427">Do</span></span> | <span data-ttu-id="2ada7-1428">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1428">Err</span></span> | <span data-ttu-id="2ada7-1429">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1429">Err</span></span> | <span data-ttu-id="2ada7-1430">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1430">Do</span></span>  | <span data-ttu-id="2ada7-1431">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1431">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1432">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1432">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1433">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1433">Err</span></span> | <span data-ttu-id="2ada7-1434">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1434">Err</span></span> | <span data-ttu-id="2ada7-1435">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1435">Err</span></span> | <span data-ttu-id="2ada7-1436">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1436">Err</span></span> | 
| <span data-ttu-id="2ada7-1437">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1437">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-1438">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1438">Err</span></span> | <span data-ttu-id="2ada7-1439">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1439">Err</span></span> | <span data-ttu-id="2ada7-1440">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1440">Err</span></span> | 
| <span data-ttu-id="2ada7-1441">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1441">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-1442">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1442">Do</span></span>  | <span data-ttu-id="2ada7-1443">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1443">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1444">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1444">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="2ada7-1445">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1445">Ob</span></span>  | 


### <a name="division-operators"></a><span data-ttu-id="2ada7-1446">除法运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-1446">Division Operators</span></span>

<span data-ttu-id="2ada7-1447">除法运算符计算两个操作数的商。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1447">Division operators compute the quotient of two operands.</span></span> <span data-ttu-id="2ada7-1448">有两个除法运算符: （浮点） 的正则除法运算符和整数除法运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1448">There are two division operators: the regular (floating-point) division operator and the integer division operator.</span></span>

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

<span data-ttu-id="2ada7-1449">正则除法运算符定义为以下类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-1449">The regular division operator is defined for the following types:</span></span>

* <span data-ttu-id="2ada7-1450">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1450">`Single` and `Double`.</span></span> <span data-ttu-id="2ada7-1451">根据 IEEE 754 算法的规则计算商。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1451">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="2ada7-1452">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1452">`Decimal`.</span></span> <span data-ttu-id="2ada7-1453">如果右操作数的值为零，`System.DivideByZeroException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1453">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1454">如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1454">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1455">如果结果值过小，以十进制格式表示，则结果为零。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1455">If the result value is too small to represent in the decimal format, the result is zero.</span></span> <span data-ttu-id="2ada7-1456">结果，任何舍入之前, 的小数位数是首选缩放的保留结果等于确切结果最接近的规模。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1456">The scale of the result, before any rounding, is the closest scale to the preferred scale which will preserve a result equal to the exact result.</span></span>  <span data-ttu-id="2ada7-1457">首选的小数位数是第一个操作数小于第二个操作数的小数位数的小数位数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1457">The preferred scale is the scale of the first operand less the scale of the second operand.</span></span>

<span data-ttu-id="2ada7-1458">根据常规运算符解析规则，纯粹之间操作数的类型，如进行常规分隔`Byte`， `Short`， `Integer`，和`Long`会导致两个操作数转换为类型`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1458">According to normal operator resolution rules, regular division purely between operands of types such as `Byte`, `Short`, `Integer`, and `Long` would cause both operands to be converted to type `Decimal`.</span></span> <span data-ttu-id="2ada7-1459">但是，在这两种类型时进行除法运算符在运算符解析`Decimal`，`Double`被视为窄于`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1459">However, when doing operator resolution on the division operator when neither type is `Decimal`, `Double` is considered narrower than `Decimal`.</span></span> <span data-ttu-id="2ada7-1460">因为遵循此约定`Double`部门是比效率更高`Decimal`除法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1460">This convention is followed because `Double` division is more efficient than `Decimal` division.</span></span>

<span data-ttu-id="2ada7-1461">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1461">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-1462">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1462">__Bo__</span></span> | <span data-ttu-id="2ada7-1463">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1463">__SB__</span></span> | <span data-ttu-id="2ada7-1464">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1464">__By__</span></span> | <span data-ttu-id="2ada7-1465">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1465">__Sh__</span></span> | <span data-ttu-id="2ada7-1466">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1466">__US__</span></span> | <span data-ttu-id="2ada7-1467">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1467">__In__</span></span> | <span data-ttu-id="2ada7-1468">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1468">__UI__</span></span> | <span data-ttu-id="2ada7-1469">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1469">__Lo__</span></span> | <span data-ttu-id="2ada7-1470">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1470">__UL__</span></span> | <span data-ttu-id="2ada7-1471">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1471">__De__</span></span> | <span data-ttu-id="2ada7-1472">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1472">__Si__</span></span> | <span data-ttu-id="2ada7-1473">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1473">__Do__</span></span> | <span data-ttu-id="2ada7-1474">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1474">__Da__</span></span>  | <span data-ttu-id="2ada7-1475">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1475">__Ch__</span></span>  | <span data-ttu-id="2ada7-1476">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1476">__St__</span></span> | <span data-ttu-id="2ada7-1477">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1477">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="2ada7-1478">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1478">__Bo__</span></span> | <span data-ttu-id="2ada7-1479">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1479">Do</span></span> | <span data-ttu-id="2ada7-1480">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1480">Do</span></span> | <span data-ttu-id="2ada7-1481">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1481">Do</span></span> | <span data-ttu-id="2ada7-1482">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1482">Do</span></span> | <span data-ttu-id="2ada7-1483">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1483">Do</span></span> | <span data-ttu-id="2ada7-1484">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1484">Do</span></span> | <span data-ttu-id="2ada7-1485">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1485">Do</span></span> | <span data-ttu-id="2ada7-1486">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1486">Do</span></span> | <span data-ttu-id="2ada7-1487">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1487">Do</span></span> | <span data-ttu-id="2ada7-1488">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1488">De</span></span> | <span data-ttu-id="2ada7-1489">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1489">Si</span></span> | <span data-ttu-id="2ada7-1490">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1490">Do</span></span> | <span data-ttu-id="2ada7-1491">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1491">Err</span></span> | <span data-ttu-id="2ada7-1492">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1492">Err</span></span> | <span data-ttu-id="2ada7-1493">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1493">Do</span></span>  | <span data-ttu-id="2ada7-1494">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1494">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1495">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1495">__SB__</span></span> |    | <span data-ttu-id="2ada7-1496">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1496">Do</span></span> | <span data-ttu-id="2ada7-1497">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1497">Do</span></span> | <span data-ttu-id="2ada7-1498">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1498">Do</span></span> | <span data-ttu-id="2ada7-1499">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1499">Do</span></span> | <span data-ttu-id="2ada7-1500">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1500">Do</span></span> | <span data-ttu-id="2ada7-1501">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1501">Do</span></span> | <span data-ttu-id="2ada7-1502">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1502">Do</span></span> | <span data-ttu-id="2ada7-1503">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1503">Do</span></span> | <span data-ttu-id="2ada7-1504">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1504">De</span></span> | <span data-ttu-id="2ada7-1505">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1505">Si</span></span> | <span data-ttu-id="2ada7-1506">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1506">Do</span></span> | <span data-ttu-id="2ada7-1507">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1507">Err</span></span> | <span data-ttu-id="2ada7-1508">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1508">Err</span></span> | <span data-ttu-id="2ada7-1509">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1509">Do</span></span>  | <span data-ttu-id="2ada7-1510">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1510">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1511">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1511">__By__</span></span> |    |    | <span data-ttu-id="2ada7-1512">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1512">Do</span></span> | <span data-ttu-id="2ada7-1513">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1513">Do</span></span> | <span data-ttu-id="2ada7-1514">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1514">Do</span></span> | <span data-ttu-id="2ada7-1515">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1515">Do</span></span> | <span data-ttu-id="2ada7-1516">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1516">Do</span></span> | <span data-ttu-id="2ada7-1517">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1517">Do</span></span> | <span data-ttu-id="2ada7-1518">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1518">Do</span></span> | <span data-ttu-id="2ada7-1519">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1519">De</span></span> | <span data-ttu-id="2ada7-1520">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1520">Si</span></span> | <span data-ttu-id="2ada7-1521">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1521">Do</span></span> | <span data-ttu-id="2ada7-1522">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1522">Err</span></span> | <span data-ttu-id="2ada7-1523">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1523">Err</span></span> | <span data-ttu-id="2ada7-1524">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1524">Do</span></span>  | <span data-ttu-id="2ada7-1525">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1525">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1526">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1526">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-1527">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1527">Do</span></span> | <span data-ttu-id="2ada7-1528">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1528">Do</span></span> | <span data-ttu-id="2ada7-1529">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1529">Do</span></span> | <span data-ttu-id="2ada7-1530">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1530">Do</span></span> | <span data-ttu-id="2ada7-1531">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1531">Do</span></span> | <span data-ttu-id="2ada7-1532">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1532">Do</span></span> | <span data-ttu-id="2ada7-1533">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1533">De</span></span> | <span data-ttu-id="2ada7-1534">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1534">Si</span></span> | <span data-ttu-id="2ada7-1535">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1535">Do</span></span> | <span data-ttu-id="2ada7-1536">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1536">Err</span></span> | <span data-ttu-id="2ada7-1537">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1537">Err</span></span> | <span data-ttu-id="2ada7-1538">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1538">Do</span></span>  | <span data-ttu-id="2ada7-1539">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1539">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1540">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1540">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-1541">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1541">Do</span></span> | <span data-ttu-id="2ada7-1542">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1542">Do</span></span> | <span data-ttu-id="2ada7-1543">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1543">Do</span></span> | <span data-ttu-id="2ada7-1544">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1544">Do</span></span> | <span data-ttu-id="2ada7-1545">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1545">Do</span></span> | <span data-ttu-id="2ada7-1546">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1546">De</span></span> | <span data-ttu-id="2ada7-1547">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1547">Si</span></span> | <span data-ttu-id="2ada7-1548">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1548">Do</span></span> | <span data-ttu-id="2ada7-1549">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1549">Err</span></span> | <span data-ttu-id="2ada7-1550">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1550">Err</span></span> | <span data-ttu-id="2ada7-1551">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1551">Do</span></span>  | <span data-ttu-id="2ada7-1552">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1552">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1553">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1553">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-1554">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1554">Do</span></span> | <span data-ttu-id="2ada7-1555">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1555">Do</span></span> | <span data-ttu-id="2ada7-1556">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1556">Do</span></span> | <span data-ttu-id="2ada7-1557">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1557">Do</span></span> | <span data-ttu-id="2ada7-1558">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1558">De</span></span> | <span data-ttu-id="2ada7-1559">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1559">Si</span></span> | <span data-ttu-id="2ada7-1560">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1560">Do</span></span> | <span data-ttu-id="2ada7-1561">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1561">Err</span></span> | <span data-ttu-id="2ada7-1562">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1562">Err</span></span> | <span data-ttu-id="2ada7-1563">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1563">Do</span></span>  | <span data-ttu-id="2ada7-1564">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1564">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1565">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1565">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-1566">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1566">Do</span></span> | <span data-ttu-id="2ada7-1567">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1567">Do</span></span> | <span data-ttu-id="2ada7-1568">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1568">Do</span></span> | <span data-ttu-id="2ada7-1569">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1569">De</span></span> | <span data-ttu-id="2ada7-1570">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1570">Si</span></span> | <span data-ttu-id="2ada7-1571">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1571">Do</span></span> | <span data-ttu-id="2ada7-1572">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1572">Err</span></span> | <span data-ttu-id="2ada7-1573">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1573">Err</span></span> | <span data-ttu-id="2ada7-1574">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1574">Do</span></span>  | <span data-ttu-id="2ada7-1575">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1575">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1576">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1577">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1577">Do</span></span> | <span data-ttu-id="2ada7-1578">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1578">Do</span></span> | <span data-ttu-id="2ada7-1579">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1579">De</span></span> | <span data-ttu-id="2ada7-1580">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1580">Si</span></span> | <span data-ttu-id="2ada7-1581">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1581">Do</span></span> | <span data-ttu-id="2ada7-1582">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1582">Err</span></span> | <span data-ttu-id="2ada7-1583">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1583">Err</span></span> | <span data-ttu-id="2ada7-1584">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1584">Do</span></span>  | <span data-ttu-id="2ada7-1585">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1585">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1586">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1586">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1587">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1587">Do</span></span> | <span data-ttu-id="2ada7-1588">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1588">De</span></span> | <span data-ttu-id="2ada7-1589">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1589">Si</span></span> | <span data-ttu-id="2ada7-1590">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1590">Do</span></span> | <span data-ttu-id="2ada7-1591">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1591">Err</span></span> | <span data-ttu-id="2ada7-1592">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1592">Err</span></span> | <span data-ttu-id="2ada7-1593">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1593">Do</span></span>  | <span data-ttu-id="2ada7-1594">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1594">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1595">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1595">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1596">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1596">De</span></span> | <span data-ttu-id="2ada7-1597">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1597">Si</span></span> | <span data-ttu-id="2ada7-1598">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1598">Do</span></span> | <span data-ttu-id="2ada7-1599">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1599">Err</span></span> | <span data-ttu-id="2ada7-1600">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1600">Err</span></span> | <span data-ttu-id="2ada7-1601">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1601">Do</span></span>  | <span data-ttu-id="2ada7-1602">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1602">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1603">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1603">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1604">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1604">Si</span></span> | <span data-ttu-id="2ada7-1605">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1605">Do</span></span> | <span data-ttu-id="2ada7-1606">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1606">Err</span></span> | <span data-ttu-id="2ada7-1607">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1607">Err</span></span> | <span data-ttu-id="2ada7-1608">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1608">Do</span></span>  | <span data-ttu-id="2ada7-1609">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1609">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1610">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1610">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1611">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1611">Do</span></span> | <span data-ttu-id="2ada7-1612">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1612">Err</span></span> | <span data-ttu-id="2ada7-1613">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1613">Err</span></span> | <span data-ttu-id="2ada7-1614">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1614">Do</span></span>  | <span data-ttu-id="2ada7-1615">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1615">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1616">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1616">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1617">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1617">Err</span></span> | <span data-ttu-id="2ada7-1618">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1618">Err</span></span> | <span data-ttu-id="2ada7-1619">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1619">Err</span></span> | <span data-ttu-id="2ada7-1620">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1620">Err</span></span> | 
| <span data-ttu-id="2ada7-1621">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1621">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-1622">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1622">Err</span></span> | <span data-ttu-id="2ada7-1623">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1623">Err</span></span> | <span data-ttu-id="2ada7-1624">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1624">Err</span></span> | 
| <span data-ttu-id="2ada7-1625">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1625">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-1626">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1626">Do</span></span>  | <span data-ttu-id="2ada7-1627">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1627">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1628">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1628">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="2ada7-1629">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1629">Ob</span></span>  | 

<span data-ttu-id="2ada7-1630">为定义整数除法运算符`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`，和`Long`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1630">The integer division operator is defined for `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="2ada7-1631">如果右操作数的值为零，`System.DivideByZeroException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1631">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1632">该除法运算将舍入到零，结果和结果的绝对值是小于两个操作数的商的绝对值的最大可能整数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1632">The division rounds the result towards zero, and the absolute value of the result is the largest possible integer that is less than the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="2ada7-1633">两个操作数具有相同的符号和零或负数时两个操作数具有符号相反时，则结果为零或正数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1633">The result is zero or positive when the two operands have the same sign, and zero or negative when the two operands have opposite signs.</span></span> <span data-ttu-id="2ada7-1634">如果左的操作数是最大负`SByte`， `Short`， `Integer`，或`Long`，和右操作数是`-1`，则发生溢出; 如果整数溢出检查位于`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1634">If the left operand is the maximum negative `SByte`, `Short`, `Integer`, or `Long`, and the right operand is `-1`, an overflow occurs; if integer overflow checking is on, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1635">否则为不报告溢出，结果而是为左操作数的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1635">Otherwise, the overflow is not reported and the result is instead the value of the left operand.</span></span>

<span data-ttu-id="2ada7-1636">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1636">__Note.__</span></span> <span data-ttu-id="2ada7-1637">无符号类型的两个操作数始终为零或正数，结果始终是零或正数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1637">As the two operands for unsigned types will always be zero or positive, the result is always zero or positive.</span></span>  <span data-ttu-id="2ada7-1638">因为该表达式的结果将始终小于或等于最大的两个操作数，不能发生溢出。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1638">As the result of the expression will always be less than or equal to the largest of the two operands, it is not possible for an overflow to occur.</span></span>  <span data-ttu-id="2ada7-1639">这种情况下整数溢出检查不是执行使用两个无符号整数整除。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1639">As such integer overflow checking is not performed for integer divide with two unsigned integers.</span></span> <span data-ttu-id="2ada7-1640">结果是与左操作数具有类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1640">The result is the type as that of the left operand.</span></span>


<span data-ttu-id="2ada7-1641">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1641">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-1642">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1642">__Bo__</span></span> | <span data-ttu-id="2ada7-1643">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1643">__SB__</span></span> | <span data-ttu-id="2ada7-1644">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1644">__By__</span></span> | <span data-ttu-id="2ada7-1645">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1645">__Sh__</span></span> | <span data-ttu-id="2ada7-1646">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1646">__US__</span></span> | <span data-ttu-id="2ada7-1647">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1647">__In__</span></span> | <span data-ttu-id="2ada7-1648">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1648">__UI__</span></span> | <span data-ttu-id="2ada7-1649">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1649">__Lo__</span></span> | <span data-ttu-id="2ada7-1650">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1650">__UL__</span></span> | <span data-ttu-id="2ada7-1651">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1651">__De__</span></span> | <span data-ttu-id="2ada7-1652">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1652">__Si__</span></span> | <span data-ttu-id="2ada7-1653">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1653">__Do__</span></span> | <span data-ttu-id="2ada7-1654">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1654">__Da__</span></span>  | <span data-ttu-id="2ada7-1655">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1655">__Ch__</span></span>  | <span data-ttu-id="2ada7-1656">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1656">__St__</span></span> | <span data-ttu-id="2ada7-1657">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1657">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="2ada7-1658">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1658">__Bo__</span></span> | <span data-ttu-id="2ada7-1659">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1659">Sh</span></span> | <span data-ttu-id="2ada7-1660">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-1660">SB</span></span> | <span data-ttu-id="2ada7-1661">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1661">Sh</span></span> | <span data-ttu-id="2ada7-1662">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1662">Sh</span></span> | <span data-ttu-id="2ada7-1663">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1663">In</span></span> | <span data-ttu-id="2ada7-1664">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1664">In</span></span> | <span data-ttu-id="2ada7-1665">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1665">Lo</span></span> | <span data-ttu-id="2ada7-1666">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1666">Lo</span></span> | <span data-ttu-id="2ada7-1667">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1667">Lo</span></span> | <span data-ttu-id="2ada7-1668">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1668">Lo</span></span> | <span data-ttu-id="2ada7-1669">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1669">Lo</span></span> | <span data-ttu-id="2ada7-1670">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1670">Lo</span></span> | <span data-ttu-id="2ada7-1671">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1671">Err</span></span> | <span data-ttu-id="2ada7-1672">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1672">Err</span></span> | <span data-ttu-id="2ada7-1673">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1673">Lo</span></span>  | <span data-ttu-id="2ada7-1674">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1674">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1675">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1675">__SB__</span></span> |    | <span data-ttu-id="2ada7-1676">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-1676">SB</span></span> | <span data-ttu-id="2ada7-1677">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1677">Sh</span></span> | <span data-ttu-id="2ada7-1678">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1678">Sh</span></span> | <span data-ttu-id="2ada7-1679">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1679">In</span></span> | <span data-ttu-id="2ada7-1680">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1680">In</span></span> | <span data-ttu-id="2ada7-1681">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1681">Lo</span></span> | <span data-ttu-id="2ada7-1682">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1682">Lo</span></span> | <span data-ttu-id="2ada7-1683">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1683">Lo</span></span> | <span data-ttu-id="2ada7-1684">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1684">Lo</span></span> | <span data-ttu-id="2ada7-1685">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1685">Lo</span></span> | <span data-ttu-id="2ada7-1686">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1686">Lo</span></span> | <span data-ttu-id="2ada7-1687">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1687">Err</span></span> | <span data-ttu-id="2ada7-1688">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1688">Err</span></span> | <span data-ttu-id="2ada7-1689">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1689">Lo</span></span>  | <span data-ttu-id="2ada7-1690">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1690">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1691">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1691">__By__</span></span> |    |    | <span data-ttu-id="2ada7-1692">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-1692">By</span></span> | <span data-ttu-id="2ada7-1693">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1693">Sh</span></span> | <span data-ttu-id="2ada7-1694">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-1694">US</span></span> | <span data-ttu-id="2ada7-1695">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1695">In</span></span> | <span data-ttu-id="2ada7-1696">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1696">UI</span></span> | <span data-ttu-id="2ada7-1697">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1697">Lo</span></span> | <span data-ttu-id="2ada7-1698">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1698">UL</span></span> | <span data-ttu-id="2ada7-1699">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1699">Lo</span></span> | <span data-ttu-id="2ada7-1700">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1700">Lo</span></span> | <span data-ttu-id="2ada7-1701">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1701">Lo</span></span> | <span data-ttu-id="2ada7-1702">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1702">Err</span></span> | <span data-ttu-id="2ada7-1703">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1703">Err</span></span> | <span data-ttu-id="2ada7-1704">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1704">Lo</span></span>  | <span data-ttu-id="2ada7-1705">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1705">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1706">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1706">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-1707">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1707">Sh</span></span> | <span data-ttu-id="2ada7-1708">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1708">In</span></span> | <span data-ttu-id="2ada7-1709">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1709">In</span></span> | <span data-ttu-id="2ada7-1710">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1710">Lo</span></span> | <span data-ttu-id="2ada7-1711">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1711">Lo</span></span> | <span data-ttu-id="2ada7-1712">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1712">Lo</span></span> | <span data-ttu-id="2ada7-1713">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1713">Lo</span></span> | <span data-ttu-id="2ada7-1714">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1714">Lo</span></span> | <span data-ttu-id="2ada7-1715">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1715">Lo</span></span> | <span data-ttu-id="2ada7-1716">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1716">Err</span></span> | <span data-ttu-id="2ada7-1717">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1717">Err</span></span> | <span data-ttu-id="2ada7-1718">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1718">Lo</span></span>  | <span data-ttu-id="2ada7-1719">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1719">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1720">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1720">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-1721">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-1721">US</span></span> | <span data-ttu-id="2ada7-1722">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1722">In</span></span> | <span data-ttu-id="2ada7-1723">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1723">UI</span></span> | <span data-ttu-id="2ada7-1724">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1724">Lo</span></span> | <span data-ttu-id="2ada7-1725">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1725">UL</span></span> | <span data-ttu-id="2ada7-1726">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1726">Lo</span></span> | <span data-ttu-id="2ada7-1727">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1727">Lo</span></span> | <span data-ttu-id="2ada7-1728">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1728">Lo</span></span> | <span data-ttu-id="2ada7-1729">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1729">Err</span></span> | <span data-ttu-id="2ada7-1730">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1730">Err</span></span> | <span data-ttu-id="2ada7-1731">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1731">Lo</span></span>  | <span data-ttu-id="2ada7-1732">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1732">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1733">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1733">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-1734">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1734">In</span></span> | <span data-ttu-id="2ada7-1735">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1735">Lo</span></span> | <span data-ttu-id="2ada7-1736">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1736">Lo</span></span> | <span data-ttu-id="2ada7-1737">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1737">Lo</span></span> | <span data-ttu-id="2ada7-1738">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1738">Lo</span></span> | <span data-ttu-id="2ada7-1739">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1739">Lo</span></span> | <span data-ttu-id="2ada7-1740">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1740">Lo</span></span> | <span data-ttu-id="2ada7-1741">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1741">Err</span></span> | <span data-ttu-id="2ada7-1742">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1742">Err</span></span> | <span data-ttu-id="2ada7-1743">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1743">Lo</span></span>  | <span data-ttu-id="2ada7-1744">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1744">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1745">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1745">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-1746">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1746">UI</span></span> | <span data-ttu-id="2ada7-1747">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1747">Lo</span></span> | <span data-ttu-id="2ada7-1748">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1748">UL</span></span> | <span data-ttu-id="2ada7-1749">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1749">Lo</span></span> | <span data-ttu-id="2ada7-1750">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1750">Lo</span></span> | <span data-ttu-id="2ada7-1751">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1751">Lo</span></span> | <span data-ttu-id="2ada7-1752">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1752">Err</span></span> | <span data-ttu-id="2ada7-1753">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1753">Err</span></span> | <span data-ttu-id="2ada7-1754">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1754">Lo</span></span>  | <span data-ttu-id="2ada7-1755">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1755">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1756">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1756">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1757">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1757">Lo</span></span> | <span data-ttu-id="2ada7-1758">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1758">Lo</span></span> | <span data-ttu-id="2ada7-1759">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1759">Lo</span></span> | <span data-ttu-id="2ada7-1760">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1760">Lo</span></span> | <span data-ttu-id="2ada7-1761">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1761">Lo</span></span> | <span data-ttu-id="2ada7-1762">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1762">Err</span></span> | <span data-ttu-id="2ada7-1763">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1763">Err</span></span> | <span data-ttu-id="2ada7-1764">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1764">Lo</span></span>  | <span data-ttu-id="2ada7-1765">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1765">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1766">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1766">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1767">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1767">UL</span></span> | <span data-ttu-id="2ada7-1768">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1768">Lo</span></span> | <span data-ttu-id="2ada7-1769">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1769">Lo</span></span> | <span data-ttu-id="2ada7-1770">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1770">Lo</span></span> | <span data-ttu-id="2ada7-1771">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1771">Err</span></span> | <span data-ttu-id="2ada7-1772">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1772">Err</span></span> | <span data-ttu-id="2ada7-1773">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1773">Lo</span></span>  | <span data-ttu-id="2ada7-1774">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1774">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1775">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1775">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1776">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1776">Lo</span></span> | <span data-ttu-id="2ada7-1777">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1777">Lo</span></span> | <span data-ttu-id="2ada7-1778">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1778">Lo</span></span> | <span data-ttu-id="2ada7-1779">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1779">Err</span></span> | <span data-ttu-id="2ada7-1780">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1780">Err</span></span> | <span data-ttu-id="2ada7-1781">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1781">Lo</span></span>  | <span data-ttu-id="2ada7-1782">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1782">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1783">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1783">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1784">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1784">Lo</span></span> | <span data-ttu-id="2ada7-1785">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1785">Lo</span></span> | <span data-ttu-id="2ada7-1786">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1786">Err</span></span> | <span data-ttu-id="2ada7-1787">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1787">Err</span></span> | <span data-ttu-id="2ada7-1788">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1788">Lo</span></span>  | <span data-ttu-id="2ada7-1789">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1789">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1790">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1790">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1791">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1791">Lo</span></span> | <span data-ttu-id="2ada7-1792">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1792">Err</span></span> | <span data-ttu-id="2ada7-1793">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1793">Err</span></span> | <span data-ttu-id="2ada7-1794">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1794">Lo</span></span>  | <span data-ttu-id="2ada7-1795">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1795">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1796">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1796">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1797">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1797">Err</span></span> | <span data-ttu-id="2ada7-1798">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1798">Err</span></span> | <span data-ttu-id="2ada7-1799">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1799">Err</span></span> | <span data-ttu-id="2ada7-1800">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1800">Err</span></span> | 
| <span data-ttu-id="2ada7-1801">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1801">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-1802">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1802">Err</span></span> | <span data-ttu-id="2ada7-1803">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1803">Err</span></span> | <span data-ttu-id="2ada7-1804">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1804">Err</span></span> | 
| <span data-ttu-id="2ada7-1805">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1805">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-1806">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1806">Lo</span></span>  | <span data-ttu-id="2ada7-1807">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1807">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1808">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="2ada7-1809">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1809">Ob</span></span>  | 


### <a name="mod-operator"></a><span data-ttu-id="2ada7-1810">Mod 运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-1810">Mod Operator</span></span>

<span data-ttu-id="2ada7-1811">`Mod` （取模） 运算符计算两个操作数相除的余数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1811">The `Mod` (modulo) operator computes the remainder of the division between two operands.</span></span>

```antlr
ModuloOperatorExpression
    : Expression 'Mod' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-1812">`Mod`运算符定义为以下类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-1812">The `Mod` operator is defined for the following types:</span></span>

* <span data-ttu-id="2ada7-1813">`Byte``SByte`， `UShort`， `Short`， `UInteger`， `Integer`，`ULong`和`Long`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1813">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long`.</span></span> <span data-ttu-id="2ada7-1814">结果`x Mod y`由生成的值`x - (x \ y) * y`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1814">The result of `x Mod y` is the value produced by `x - (x \ y) * y`.</span></span> <span data-ttu-id="2ada7-1815">如果`y`为零，`System.DivideByZeroException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1815">If `y` is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1816">取模运算符永远不会导致溢出。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1816">The modulo operator never causes an overflow.</span></span>

* <span data-ttu-id="2ada7-1817">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1817">`Single` and `Double`.</span></span> <span data-ttu-id="2ada7-1818">根据 IEEE 754 算法的规则计算余数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1818">The remainder is computed according to the rules of IEEE 754 arithmetic.</span></span>

* <span data-ttu-id="2ada7-1819">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1819">`Decimal`.</span></span> <span data-ttu-id="2ada7-1820">如果右操作数的值为零，`System.DivideByZeroException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1820">If the value of the right operand is zero, a `System.DivideByZeroException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1821">如果生成的值太大，以十进制格式，表示`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1821">If the resulting value is too large to represent in the decimal format, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="2ada7-1822">如果结果值过小，以十进制格式表示，则结果为零。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1822">If the result value is too small to represent in the decimal format, the result is zero.</span></span>

<span data-ttu-id="2ada7-1823">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1823">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-1824">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1824">__Bo__</span></span> | <span data-ttu-id="2ada7-1825">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1825">__SB__</span></span> | <span data-ttu-id="2ada7-1826">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1826">__By__</span></span> | <span data-ttu-id="2ada7-1827">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1827">__Sh__</span></span> | <span data-ttu-id="2ada7-1828">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1828">__US__</span></span> | <span data-ttu-id="2ada7-1829">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1829">__In__</span></span> | <span data-ttu-id="2ada7-1830">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1830">__UI__</span></span> | <span data-ttu-id="2ada7-1831">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1831">__Lo__</span></span> | <span data-ttu-id="2ada7-1832">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1832">__UL__</span></span> | <span data-ttu-id="2ada7-1833">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1833">__De__</span></span> | <span data-ttu-id="2ada7-1834">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1834">__Si__</span></span> | <span data-ttu-id="2ada7-1835">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1835">__Do__</span></span> | <span data-ttu-id="2ada7-1836">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1836">__Da__</span></span>  | <span data-ttu-id="2ada7-1837">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1837">__Ch__</span></span>  | <span data-ttu-id="2ada7-1838">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1838">__St__</span></span> | <span data-ttu-id="2ada7-1839">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1839">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="2ada7-1840">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1840">__Bo__</span></span> | <span data-ttu-id="2ada7-1841">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1841">Sh</span></span> | <span data-ttu-id="2ada7-1842">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-1842">SB</span></span> | <span data-ttu-id="2ada7-1843">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1843">Sh</span></span> | <span data-ttu-id="2ada7-1844">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1844">Sh</span></span> | <span data-ttu-id="2ada7-1845">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1845">In</span></span> | <span data-ttu-id="2ada7-1846">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1846">In</span></span> | <span data-ttu-id="2ada7-1847">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1847">Lo</span></span> | <span data-ttu-id="2ada7-1848">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1848">Lo</span></span> | <span data-ttu-id="2ada7-1849">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1849">De</span></span> | <span data-ttu-id="2ada7-1850">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1850">De</span></span> | <span data-ttu-id="2ada7-1851">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1851">Si</span></span> | <span data-ttu-id="2ada7-1852">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1852">Do</span></span> | <span data-ttu-id="2ada7-1853">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1853">Err</span></span> | <span data-ttu-id="2ada7-1854">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1854">Err</span></span> | <span data-ttu-id="2ada7-1855">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1855">Do</span></span>  | <span data-ttu-id="2ada7-1856">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1856">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1857">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1857">__SB__</span></span> |    | <span data-ttu-id="2ada7-1858">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-1858">SB</span></span> | <span data-ttu-id="2ada7-1859">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1859">Sh</span></span> | <span data-ttu-id="2ada7-1860">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1860">Sh</span></span> | <span data-ttu-id="2ada7-1861">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1861">In</span></span> | <span data-ttu-id="2ada7-1862">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1862">In</span></span> | <span data-ttu-id="2ada7-1863">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1863">Lo</span></span> | <span data-ttu-id="2ada7-1864">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1864">Lo</span></span> | <span data-ttu-id="2ada7-1865">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1865">De</span></span> | <span data-ttu-id="2ada7-1866">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1866">De</span></span> | <span data-ttu-id="2ada7-1867">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1867">Si</span></span> | <span data-ttu-id="2ada7-1868">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1868">Do</span></span> | <span data-ttu-id="2ada7-1869">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1869">Err</span></span> | <span data-ttu-id="2ada7-1870">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1870">Err</span></span> | <span data-ttu-id="2ada7-1871">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1871">Do</span></span>  | <span data-ttu-id="2ada7-1872">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1872">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1873">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1873">__By__</span></span> |    |    | <span data-ttu-id="2ada7-1874">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-1874">By</span></span> | <span data-ttu-id="2ada7-1875">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1875">Sh</span></span> | <span data-ttu-id="2ada7-1876">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-1876">US</span></span> | <span data-ttu-id="2ada7-1877">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1877">In</span></span> | <span data-ttu-id="2ada7-1878">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1878">UI</span></span> | <span data-ttu-id="2ada7-1879">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1879">Lo</span></span> | <span data-ttu-id="2ada7-1880">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1880">UL</span></span> | <span data-ttu-id="2ada7-1881">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1881">De</span></span> | <span data-ttu-id="2ada7-1882">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1882">Si</span></span> | <span data-ttu-id="2ada7-1883">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1883">Do</span></span> | <span data-ttu-id="2ada7-1884">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1884">Err</span></span> | <span data-ttu-id="2ada7-1885">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1885">Err</span></span> | <span data-ttu-id="2ada7-1886">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1886">Do</span></span>  | <span data-ttu-id="2ada7-1887">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1887">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1888">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1888">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-1889">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-1889">Sh</span></span> | <span data-ttu-id="2ada7-1890">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1890">In</span></span> | <span data-ttu-id="2ada7-1891">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1891">In</span></span> | <span data-ttu-id="2ada7-1892">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1892">Lo</span></span> | <span data-ttu-id="2ada7-1893">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1893">Lo</span></span> | <span data-ttu-id="2ada7-1894">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1894">De</span></span> | <span data-ttu-id="2ada7-1895">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1895">De</span></span> | <span data-ttu-id="2ada7-1896">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1896">Si</span></span> | <span data-ttu-id="2ada7-1897">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1897">Do</span></span> | <span data-ttu-id="2ada7-1898">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1898">Err</span></span> | <span data-ttu-id="2ada7-1899">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1899">Err</span></span> | <span data-ttu-id="2ada7-1900">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1900">Do</span></span>  | <span data-ttu-id="2ada7-1901">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1901">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1902">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1902">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-1903">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-1903">US</span></span> | <span data-ttu-id="2ada7-1904">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1904">In</span></span> | <span data-ttu-id="2ada7-1905">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1905">UI</span></span> | <span data-ttu-id="2ada7-1906">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1906">Lo</span></span> | <span data-ttu-id="2ada7-1907">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1907">UL</span></span> | <span data-ttu-id="2ada7-1908">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1908">De</span></span> | <span data-ttu-id="2ada7-1909">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1909">Si</span></span> | <span data-ttu-id="2ada7-1910">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1910">Do</span></span> | <span data-ttu-id="2ada7-1911">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1911">Err</span></span> | <span data-ttu-id="2ada7-1912">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1912">Err</span></span> | <span data-ttu-id="2ada7-1913">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1913">Do</span></span>  | <span data-ttu-id="2ada7-1914">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1914">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1915">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1915">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-1916">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-1916">In</span></span> | <span data-ttu-id="2ada7-1917">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1917">Lo</span></span> | <span data-ttu-id="2ada7-1918">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1918">Lo</span></span> | <span data-ttu-id="2ada7-1919">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1919">De</span></span> | <span data-ttu-id="2ada7-1920">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1920">De</span></span> | <span data-ttu-id="2ada7-1921">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1921">Si</span></span> | <span data-ttu-id="2ada7-1922">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1922">Do</span></span> | <span data-ttu-id="2ada7-1923">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1923">Err</span></span> | <span data-ttu-id="2ada7-1924">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1924">Err</span></span> | <span data-ttu-id="2ada7-1925">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1925">Do</span></span>  | <span data-ttu-id="2ada7-1926">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1926">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1927">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1927">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-1928">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-1928">UI</span></span> | <span data-ttu-id="2ada7-1929">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1929">Lo</span></span> | <span data-ttu-id="2ada7-1930">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1930">UL</span></span> | <span data-ttu-id="2ada7-1931">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1931">De</span></span> | <span data-ttu-id="2ada7-1932">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1932">Si</span></span> | <span data-ttu-id="2ada7-1933">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1933">Do</span></span> | <span data-ttu-id="2ada7-1934">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1934">Err</span></span> | <span data-ttu-id="2ada7-1935">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1935">Err</span></span> | <span data-ttu-id="2ada7-1936">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1936">Do</span></span>  | <span data-ttu-id="2ada7-1937">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1937">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1938">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1938">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1939">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-1939">Lo</span></span> | <span data-ttu-id="2ada7-1940">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1940">De</span></span> | <span data-ttu-id="2ada7-1941">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1941">De</span></span> | <span data-ttu-id="2ada7-1942">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1942">Si</span></span> | <span data-ttu-id="2ada7-1943">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1943">Do</span></span> | <span data-ttu-id="2ada7-1944">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1944">Err</span></span> | <span data-ttu-id="2ada7-1945">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1945">Err</span></span> | <span data-ttu-id="2ada7-1946">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1946">Do</span></span>  | <span data-ttu-id="2ada7-1947">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1947">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1948">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1948">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1949">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-1949">UL</span></span> | <span data-ttu-id="2ada7-1950">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1950">De</span></span> | <span data-ttu-id="2ada7-1951">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1951">Si</span></span> | <span data-ttu-id="2ada7-1952">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1952">Do</span></span> | <span data-ttu-id="2ada7-1953">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1953">Err</span></span> | <span data-ttu-id="2ada7-1954">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1954">Err</span></span> | <span data-ttu-id="2ada7-1955">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1955">Do</span></span>  | <span data-ttu-id="2ada7-1956">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1956">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1957">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1957">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1958">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-1958">De</span></span> | <span data-ttu-id="2ada7-1959">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1959">Si</span></span> | <span data-ttu-id="2ada7-1960">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1960">Do</span></span> | <span data-ttu-id="2ada7-1961">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1961">Err</span></span> | <span data-ttu-id="2ada7-1962">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1962">Err</span></span> | <span data-ttu-id="2ada7-1963">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1963">Do</span></span>  | <span data-ttu-id="2ada7-1964">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1964">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1965">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1965">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1966">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-1966">Si</span></span> | <span data-ttu-id="2ada7-1967">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1967">Do</span></span> | <span data-ttu-id="2ada7-1968">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1968">Err</span></span> | <span data-ttu-id="2ada7-1969">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1969">Err</span></span> | <span data-ttu-id="2ada7-1970">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1970">Do</span></span>  | <span data-ttu-id="2ada7-1971">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1971">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1972">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1972">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1973">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1973">Do</span></span> | <span data-ttu-id="2ada7-1974">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1974">Err</span></span> | <span data-ttu-id="2ada7-1975">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1975">Err</span></span> | <span data-ttu-id="2ada7-1976">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1976">Do</span></span>  | <span data-ttu-id="2ada7-1977">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1977">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1978">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1978">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-1979">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1979">Err</span></span> | <span data-ttu-id="2ada7-1980">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1980">Err</span></span> | <span data-ttu-id="2ada7-1981">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1981">Err</span></span> | <span data-ttu-id="2ada7-1982">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1982">Err</span></span> | 
| <span data-ttu-id="2ada7-1983">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1983">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-1984">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1984">Err</span></span> | <span data-ttu-id="2ada7-1985">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1985">Err</span></span> | <span data-ttu-id="2ada7-1986">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-1986">Err</span></span> | 
| <span data-ttu-id="2ada7-1987">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1987">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-1988">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-1988">Do</span></span>  | <span data-ttu-id="2ada7-1989">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1989">Ob</span></span>  | 
| <span data-ttu-id="2ada7-1990">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1990">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="2ada7-1991">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-1991">Ob</span></span>  | 


### <a name="exponentiation-operator"></a><span data-ttu-id="2ada7-1992">求幂运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-1992">Exponentiation Operator</span></span>

<span data-ttu-id="2ada7-1993">求幂运算符计算的次幂的第二个操作数的第一个操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1993">The exponentiation operator computes the first operand raised to the power of the second operand.</span></span>

```antlr
ExponentOperatorExpression
    : Expression '^' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-1994">为类型定义求幂运算符`Double`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1994">The exponentiation operator is defined for type `Double`.</span></span> <span data-ttu-id="2ada7-1995">根据 IEEE 754 算法的规则计算的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-1995">The value is computed according to the rules of IEEE 754 arithmetic.</span></span>

<span data-ttu-id="2ada7-1996">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1996">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-1997">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1997">__Bo__</span></span> | <span data-ttu-id="2ada7-1998">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1998">__SB__</span></span> | <span data-ttu-id="2ada7-1999">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-1999">__By__</span></span> | <span data-ttu-id="2ada7-2000">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2000">__Sh__</span></span> | <span data-ttu-id="2ada7-2001">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2001">__US__</span></span> | <span data-ttu-id="2ada7-2002">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2002">__In__</span></span> | <span data-ttu-id="2ada7-2003">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2003">__UI__</span></span> | <span data-ttu-id="2ada7-2004">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2004">__Lo__</span></span> | <span data-ttu-id="2ada7-2005">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2005">__UL__</span></span> | <span data-ttu-id="2ada7-2006">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2006">__De__</span></span> | <span data-ttu-id="2ada7-2007">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2007">__Si__</span></span> | <span data-ttu-id="2ada7-2008">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2008">__Do__</span></span> | <span data-ttu-id="2ada7-2009">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2009">__Da__</span></span>  | <span data-ttu-id="2ada7-2010">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2010">__Ch__</span></span>  | <span data-ttu-id="2ada7-2011">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2011">__St__</span></span> | <span data-ttu-id="2ada7-2012">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2012">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="2ada7-2013">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2013">__Bo__</span></span> | <span data-ttu-id="2ada7-2014">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2014">Do</span></span> | <span data-ttu-id="2ada7-2015">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2015">Do</span></span> | <span data-ttu-id="2ada7-2016">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2016">Do</span></span> | <span data-ttu-id="2ada7-2017">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2017">Do</span></span> | <span data-ttu-id="2ada7-2018">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2018">Do</span></span> | <span data-ttu-id="2ada7-2019">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2019">Do</span></span> | <span data-ttu-id="2ada7-2020">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2020">Do</span></span> | <span data-ttu-id="2ada7-2021">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2021">Do</span></span> | <span data-ttu-id="2ada7-2022">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2022">Do</span></span> | <span data-ttu-id="2ada7-2023">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2023">Do</span></span> | <span data-ttu-id="2ada7-2024">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2024">Do</span></span> | <span data-ttu-id="2ada7-2025">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2025">Do</span></span> | <span data-ttu-id="2ada7-2026">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2026">Err</span></span> | <span data-ttu-id="2ada7-2027">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2027">Err</span></span> | <span data-ttu-id="2ada7-2028">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2028">Do</span></span>  | <span data-ttu-id="2ada7-2029">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2029">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2030">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2030">__SB__</span></span> |    | <span data-ttu-id="2ada7-2031">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2031">Do</span></span> | <span data-ttu-id="2ada7-2032">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2032">Do</span></span> | <span data-ttu-id="2ada7-2033">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2033">Do</span></span> | <span data-ttu-id="2ada7-2034">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2034">Do</span></span> | <span data-ttu-id="2ada7-2035">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2035">Do</span></span> | <span data-ttu-id="2ada7-2036">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2036">Do</span></span> | <span data-ttu-id="2ada7-2037">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2037">Do</span></span> | <span data-ttu-id="2ada7-2038">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2038">Do</span></span> | <span data-ttu-id="2ada7-2039">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2039">Do</span></span> | <span data-ttu-id="2ada7-2040">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2040">Do</span></span> | <span data-ttu-id="2ada7-2041">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2041">Do</span></span> | <span data-ttu-id="2ada7-2042">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2042">Err</span></span> | <span data-ttu-id="2ada7-2043">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2043">Err</span></span> | <span data-ttu-id="2ada7-2044">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2044">Do</span></span>  | <span data-ttu-id="2ada7-2045">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2045">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2046">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2046">__By__</span></span> |    |    | <span data-ttu-id="2ada7-2047">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2047">Do</span></span> | <span data-ttu-id="2ada7-2048">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2048">Do</span></span> | <span data-ttu-id="2ada7-2049">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2049">Do</span></span> | <span data-ttu-id="2ada7-2050">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2050">Do</span></span> | <span data-ttu-id="2ada7-2051">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2051">Do</span></span> | <span data-ttu-id="2ada7-2052">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2052">Do</span></span> | <span data-ttu-id="2ada7-2053">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2053">Do</span></span> | <span data-ttu-id="2ada7-2054">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2054">Do</span></span> | <span data-ttu-id="2ada7-2055">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2055">Do</span></span> | <span data-ttu-id="2ada7-2056">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2056">Do</span></span> | <span data-ttu-id="2ada7-2057">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2057">Err</span></span> | <span data-ttu-id="2ada7-2058">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2058">Err</span></span> | <span data-ttu-id="2ada7-2059">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2059">Do</span></span>  | <span data-ttu-id="2ada7-2060">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2060">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2061">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2061">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-2062">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2062">Do</span></span> | <span data-ttu-id="2ada7-2063">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2063">Do</span></span> | <span data-ttu-id="2ada7-2064">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2064">Do</span></span> | <span data-ttu-id="2ada7-2065">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2065">Do</span></span> | <span data-ttu-id="2ada7-2066">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2066">Do</span></span> | <span data-ttu-id="2ada7-2067">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2067">Do</span></span> | <span data-ttu-id="2ada7-2068">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2068">Do</span></span> | <span data-ttu-id="2ada7-2069">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2069">Do</span></span> | <span data-ttu-id="2ada7-2070">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2070">Do</span></span> | <span data-ttu-id="2ada7-2071">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2071">Err</span></span> | <span data-ttu-id="2ada7-2072">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2072">Err</span></span> | <span data-ttu-id="2ada7-2073">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2073">Do</span></span>  | <span data-ttu-id="2ada7-2074">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2074">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2075">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2075">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-2076">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2076">Do</span></span> | <span data-ttu-id="2ada7-2077">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2077">Do</span></span> | <span data-ttu-id="2ada7-2078">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2078">Do</span></span> | <span data-ttu-id="2ada7-2079">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2079">Do</span></span> | <span data-ttu-id="2ada7-2080">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2080">Do</span></span> | <span data-ttu-id="2ada7-2081">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2081">Do</span></span> | <span data-ttu-id="2ada7-2082">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2082">Do</span></span> | <span data-ttu-id="2ada7-2083">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2083">Do</span></span> | <span data-ttu-id="2ada7-2084">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2084">Err</span></span> | <span data-ttu-id="2ada7-2085">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2085">Err</span></span> | <span data-ttu-id="2ada7-2086">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2086">Do</span></span>  | <span data-ttu-id="2ada7-2087">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2087">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2088">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2088">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-2089">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2089">Do</span></span> | <span data-ttu-id="2ada7-2090">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2090">Do</span></span> | <span data-ttu-id="2ada7-2091">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2091">Do</span></span> | <span data-ttu-id="2ada7-2092">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2092">Do</span></span> | <span data-ttu-id="2ada7-2093">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2093">Do</span></span> | <span data-ttu-id="2ada7-2094">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2094">Do</span></span> | <span data-ttu-id="2ada7-2095">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2095">Do</span></span> | <span data-ttu-id="2ada7-2096">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2096">Err</span></span> | <span data-ttu-id="2ada7-2097">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2097">Err</span></span> | <span data-ttu-id="2ada7-2098">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2098">Do</span></span>  | <span data-ttu-id="2ada7-2099">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2099">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2100">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2100">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-2101">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2101">Do</span></span> | <span data-ttu-id="2ada7-2102">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2102">Do</span></span> | <span data-ttu-id="2ada7-2103">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2103">Do</span></span> | <span data-ttu-id="2ada7-2104">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2104">Do</span></span> | <span data-ttu-id="2ada7-2105">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2105">Do</span></span> | <span data-ttu-id="2ada7-2106">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2106">Do</span></span> | <span data-ttu-id="2ada7-2107">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2107">Err</span></span> | <span data-ttu-id="2ada7-2108">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2108">Err</span></span> | <span data-ttu-id="2ada7-2109">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2109">Do</span></span>  | <span data-ttu-id="2ada7-2110">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2110">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2111">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2111">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2112">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2112">Do</span></span> | <span data-ttu-id="2ada7-2113">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2113">Do</span></span> | <span data-ttu-id="2ada7-2114">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2114">Do</span></span> | <span data-ttu-id="2ada7-2115">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2115">Do</span></span> | <span data-ttu-id="2ada7-2116">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2116">Do</span></span> | <span data-ttu-id="2ada7-2117">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2117">Err</span></span> | <span data-ttu-id="2ada7-2118">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2118">Err</span></span> | <span data-ttu-id="2ada7-2119">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2119">Do</span></span>  | <span data-ttu-id="2ada7-2120">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2120">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2121">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2121">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2122">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2122">Do</span></span> | <span data-ttu-id="2ada7-2123">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2123">Do</span></span> | <span data-ttu-id="2ada7-2124">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2124">Do</span></span> | <span data-ttu-id="2ada7-2125">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2125">Do</span></span> | <span data-ttu-id="2ada7-2126">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2126">Err</span></span> | <span data-ttu-id="2ada7-2127">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2127">Err</span></span> | <span data-ttu-id="2ada7-2128">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2128">Do</span></span>  | <span data-ttu-id="2ada7-2129">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2129">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2130">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2130">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2131">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2131">Do</span></span> | <span data-ttu-id="2ada7-2132">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2132">Do</span></span> | <span data-ttu-id="2ada7-2133">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2133">Do</span></span> | <span data-ttu-id="2ada7-2134">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2134">Err</span></span> | <span data-ttu-id="2ada7-2135">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2135">Err</span></span> | <span data-ttu-id="2ada7-2136">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2136">Do</span></span>  | <span data-ttu-id="2ada7-2137">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2137">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2138">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2138">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2139">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2139">Do</span></span> | <span data-ttu-id="2ada7-2140">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2140">Do</span></span> | <span data-ttu-id="2ada7-2141">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2141">Err</span></span> | <span data-ttu-id="2ada7-2142">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2142">Err</span></span> | <span data-ttu-id="2ada7-2143">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2143">Do</span></span>  | <span data-ttu-id="2ada7-2144">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2144">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2145">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2145">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2146">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2146">Do</span></span> | <span data-ttu-id="2ada7-2147">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2147">Err</span></span> | <span data-ttu-id="2ada7-2148">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2148">Err</span></span> | <span data-ttu-id="2ada7-2149">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2149">Do</span></span>  | <span data-ttu-id="2ada7-2150">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2150">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2151">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2151">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2152">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2152">Err</span></span> | <span data-ttu-id="2ada7-2153">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2153">Err</span></span> | <span data-ttu-id="2ada7-2154">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2154">Err</span></span> | <span data-ttu-id="2ada7-2155">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2155">Err</span></span> | 
| <span data-ttu-id="2ada7-2156">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2156">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-2157">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2157">Err</span></span> | <span data-ttu-id="2ada7-2158">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2158">Err</span></span> | <span data-ttu-id="2ada7-2159">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2159">Err</span></span> | 
| <span data-ttu-id="2ada7-2160">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2160">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-2161">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2161">Do</span></span>  | <span data-ttu-id="2ada7-2162">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2162">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2163">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2163">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="2ada7-2164">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2164">Ob</span></span>  | 


## <a name="relational-operators"></a><span data-ttu-id="2ada7-2165">关系运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-2165">Relational Operators</span></span>

<span data-ttu-id="2ada7-2166">*关系运算符*比较到一个其他的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2166">The *relational operators* compare values to one other.</span></span> <span data-ttu-id="2ada7-2167">比较运算符不`=`， `<>`， `<`， `>`， `<=`，并`>=`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2167">The comparison operators are `=`, `<>`, `<`, `>`, `<=`, and `>=`.</span></span>

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

<span data-ttu-id="2ada7-2168">所有关系运算符导致`Boolean`值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2168">All of the relational operators result in a `Boolean` value.</span></span>

<span data-ttu-id="2ada7-2169">关系运算符具有以下常规含义：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2169">The relational operators have the following general meaning:</span></span>

* <span data-ttu-id="2ada7-2170">`=`运算符测试两个操作数是否相等。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2170">The `=` operator tests whether the two operands are equal.</span></span>

* <span data-ttu-id="2ada7-2171">`<>`运算符测试两个操作数是否不相等。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2171">The `<>` operator tests whether the two operands are not equal.</span></span>

* <span data-ttu-id="2ada7-2172">`<`运算符测试第一个操作数是否小于第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2172">The `<` operator tests whether the first operand is less than the second operand.</span></span>

* <span data-ttu-id="2ada7-2173">`>`运算符测试第一个操作数是否大于第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2173">The `>` operator tests whether the first operand is greater than the second operand.</span></span>

* <span data-ttu-id="2ada7-2174">`<=`运算符测试第一个操作数是否小于或等于第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2174">The `<=` operator tests whether the first operand is less than or equal to the second operand.</span></span>

* <span data-ttu-id="2ada7-2175">`>=`运算符测试第一个操作数是否大于或等于第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2175">The `>=` operator tests whether the first operand is greater than or equal to the second operand.</span></span>

<span data-ttu-id="2ada7-2176">关系运算符定义为以下类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2176">The relational operators are defined for the following types:</span></span>

* <span data-ttu-id="2ada7-2177">`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2177">`Boolean`.</span></span> <span data-ttu-id="2ada7-2178">运算符比较两个操作数的真值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2178">The operators compare the truth values of the two operands.</span></span> <span data-ttu-id="2ada7-2179">`True` 被视为小于`False`，可使用其数字值匹配。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2179">`True` is considered to be less than `False`, which matches with their numeric values.</span></span>

* <span data-ttu-id="2ada7-2180">`Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、`ULong` 和 `Long`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2180">`Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, and `Long`.</span></span> <span data-ttu-id="2ada7-2181">运算符比较两个整数操作数的数值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2181">The operators compare the numeric values of the two integral operands.</span></span>

* <span data-ttu-id="2ada7-2182">`Single` 和 `Double`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2182">`Single` and `Double`.</span></span> <span data-ttu-id="2ada7-2183">这些运算符来比较根据 IEEE 754 标准规则操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2183">The operators compare the operands according to the rules of the IEEE 754 standard.</span></span>

* <span data-ttu-id="2ada7-2184">`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2184">`Decimal`.</span></span> <span data-ttu-id="2ada7-2185">运算符比较两个十进制操作数的数值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2185">The operators compare the numeric values of the two decimal operands.</span></span>

* <span data-ttu-id="2ada7-2186">`Date`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2186">`Date`.</span></span> <span data-ttu-id="2ada7-2187">运算符将返回两个日期/时间值的比较结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2187">The operators return the result of comparing the two date/time values.</span></span>

* <span data-ttu-id="2ada7-2188">`Char`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2188">`Char`.</span></span> <span data-ttu-id="2ada7-2189">运算符将返回两个的 Unicode 值的比较结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2189">The operators return the result of comparing the two Unicode values.</span></span>

* <span data-ttu-id="2ada7-2190">`String`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2190">`String`.</span></span> <span data-ttu-id="2ada7-2191">运算符将返回使用二进制比较或文本比较两个值的比较结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2191">The operators return the result of comparing the two values using either a binary comparison or a text comparison.</span></span> <span data-ttu-id="2ada7-2192">用于比较由编译环境和`Option Compare`语句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2192">The comparison used is determined by the compilation environment and the `Option Compare` statement.</span></span> <span data-ttu-id="2ada7-2193">二进制比较确定每个字符串中的每个字符的数值的 Unicode 值是否相同。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2193">A binary comparison determines whether the numeric Unicode value of each character in each string is the same.</span></span> <span data-ttu-id="2ada7-2194">文本比较执行 Unicode 文本比较基于当前区域性中使用的.NET Framework。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2194">A text comparison does a Unicode text comparison based on the current culture in use on the .NET Framework.</span></span> <span data-ttu-id="2ada7-2195">在进行字符串比较时，null 值相当于字符串文字`""`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2195">When doing a string comparison, a null value is equivalent to the string literal `""`.</span></span>

<span data-ttu-id="2ada7-2196">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2196">__Operation Type:__</span></span>
        
|        | <span data-ttu-id="2ada7-2197">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2197">__Bo__</span></span> | <span data-ttu-id="2ada7-2198">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2198">__SB__</span></span> | <span data-ttu-id="2ada7-2199">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2199">__By__</span></span> | <span data-ttu-id="2ada7-2200">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2200">__Sh__</span></span> | <span data-ttu-id="2ada7-2201">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2201">__US__</span></span> | <span data-ttu-id="2ada7-2202">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2202">__In__</span></span> | <span data-ttu-id="2ada7-2203">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2203">__UI__</span></span> | <span data-ttu-id="2ada7-2204">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2204">__Lo__</span></span> | <span data-ttu-id="2ada7-2205">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2205">__UL__</span></span> | <span data-ttu-id="2ada7-2206">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2206">__De__</span></span> | <span data-ttu-id="2ada7-2207">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2207">__Si__</span></span> | <span data-ttu-id="2ada7-2208">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2208">__Do__</span></span> | <span data-ttu-id="2ada7-2209">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2209">__Da__</span></span>  | <span data-ttu-id="2ada7-2210">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2210">__Ch__</span></span>  | <span data-ttu-id="2ada7-2211">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2211">__St__</span></span> | <span data-ttu-id="2ada7-2212">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2212">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="2ada7-2213">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2213">__Bo__</span></span> | <span data-ttu-id="2ada7-2214">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2214">Bo</span></span> | <span data-ttu-id="2ada7-2215">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-2215">SB</span></span> | <span data-ttu-id="2ada7-2216">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2216">Sh</span></span> | <span data-ttu-id="2ada7-2217">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2217">Sh</span></span> | <span data-ttu-id="2ada7-2218">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2218">In</span></span> | <span data-ttu-id="2ada7-2219">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2219">In</span></span> | <span data-ttu-id="2ada7-2220">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2220">Lo</span></span> | <span data-ttu-id="2ada7-2221">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2221">Lo</span></span> | <span data-ttu-id="2ada7-2222">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2222">De</span></span> | <span data-ttu-id="2ada7-2223">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2223">De</span></span> | <span data-ttu-id="2ada7-2224">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2224">Si</span></span> | <span data-ttu-id="2ada7-2225">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2225">Do</span></span> | <span data-ttu-id="2ada7-2226">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2226">Err</span></span> | <span data-ttu-id="2ada7-2227">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2227">Err</span></span> | <span data-ttu-id="2ada7-2228">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2228">Bo</span></span> | <span data-ttu-id="2ada7-2229">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2229">Ob</span></span> | 
| <span data-ttu-id="2ada7-2230">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2230">__SB__</span></span> |    | <span data-ttu-id="2ada7-2231">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-2231">SB</span></span> | <span data-ttu-id="2ada7-2232">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2232">Sh</span></span> | <span data-ttu-id="2ada7-2233">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2233">Sh</span></span> | <span data-ttu-id="2ada7-2234">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2234">In</span></span> | <span data-ttu-id="2ada7-2235">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2235">In</span></span> | <span data-ttu-id="2ada7-2236">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2236">Lo</span></span> | <span data-ttu-id="2ada7-2237">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2237">Lo</span></span> | <span data-ttu-id="2ada7-2238">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2238">De</span></span> | <span data-ttu-id="2ada7-2239">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2239">De</span></span> | <span data-ttu-id="2ada7-2240">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2240">Si</span></span> | <span data-ttu-id="2ada7-2241">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2241">Do</span></span> | <span data-ttu-id="2ada7-2242">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2242">Err</span></span> | <span data-ttu-id="2ada7-2243">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2243">Err</span></span> | <span data-ttu-id="2ada7-2244">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2244">Do</span></span> | <span data-ttu-id="2ada7-2245">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2245">Ob</span></span> | 
| <span data-ttu-id="2ada7-2246">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2246">__By__</span></span> |    |    | <span data-ttu-id="2ada7-2247">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-2247">By</span></span> | <span data-ttu-id="2ada7-2248">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2248">Sh</span></span> | <span data-ttu-id="2ada7-2249">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-2249">US</span></span> | <span data-ttu-id="2ada7-2250">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2250">In</span></span> | <span data-ttu-id="2ada7-2251">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-2251">UI</span></span> | <span data-ttu-id="2ada7-2252">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2252">Lo</span></span> | <span data-ttu-id="2ada7-2253">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-2253">UL</span></span> | <span data-ttu-id="2ada7-2254">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2254">De</span></span> | <span data-ttu-id="2ada7-2255">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2255">Si</span></span> | <span data-ttu-id="2ada7-2256">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2256">Do</span></span> | <span data-ttu-id="2ada7-2257">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2257">Err</span></span> | <span data-ttu-id="2ada7-2258">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2258">Err</span></span> | <span data-ttu-id="2ada7-2259">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2259">Do</span></span> | <span data-ttu-id="2ada7-2260">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2260">Ob</span></span> | 
| <span data-ttu-id="2ada7-2261">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2261">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-2262">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2262">Sh</span></span> | <span data-ttu-id="2ada7-2263">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2263">In</span></span> | <span data-ttu-id="2ada7-2264">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2264">In</span></span> | <span data-ttu-id="2ada7-2265">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2265">Lo</span></span> | <span data-ttu-id="2ada7-2266">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2266">Lo</span></span> | <span data-ttu-id="2ada7-2267">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2267">De</span></span> | <span data-ttu-id="2ada7-2268">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2268">De</span></span> | <span data-ttu-id="2ada7-2269">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2269">Si</span></span> | <span data-ttu-id="2ada7-2270">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2270">Do</span></span> | <span data-ttu-id="2ada7-2271">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2271">Err</span></span> | <span data-ttu-id="2ada7-2272">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2272">Err</span></span> | <span data-ttu-id="2ada7-2273">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2273">Do</span></span> | <span data-ttu-id="2ada7-2274">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2274">Ob</span></span> | 
| <span data-ttu-id="2ada7-2275">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2275">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-2276">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-2276">US</span></span> | <span data-ttu-id="2ada7-2277">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2277">In</span></span> | <span data-ttu-id="2ada7-2278">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-2278">UI</span></span> | <span data-ttu-id="2ada7-2279">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2279">Lo</span></span> | <span data-ttu-id="2ada7-2280">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-2280">UL</span></span> | <span data-ttu-id="2ada7-2281">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2281">De</span></span> | <span data-ttu-id="2ada7-2282">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2282">Si</span></span> | <span data-ttu-id="2ada7-2283">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2283">Do</span></span> | <span data-ttu-id="2ada7-2284">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2284">Err</span></span> | <span data-ttu-id="2ada7-2285">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2285">Err</span></span> | <span data-ttu-id="2ada7-2286">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2286">Do</span></span> | <span data-ttu-id="2ada7-2287">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2287">Ob</span></span> | 
| <span data-ttu-id="2ada7-2288">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2288">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-2289">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2289">In</span></span> | <span data-ttu-id="2ada7-2290">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2290">Lo</span></span> | <span data-ttu-id="2ada7-2291">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2291">Lo</span></span> | <span data-ttu-id="2ada7-2292">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2292">De</span></span> | <span data-ttu-id="2ada7-2293">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2293">De</span></span> | <span data-ttu-id="2ada7-2294">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2294">Si</span></span> | <span data-ttu-id="2ada7-2295">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2295">Do</span></span> | <span data-ttu-id="2ada7-2296">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2296">Err</span></span> | <span data-ttu-id="2ada7-2297">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2297">Err</span></span> | <span data-ttu-id="2ada7-2298">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2298">Do</span></span> | <span data-ttu-id="2ada7-2299">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2299">Ob</span></span> | 
| <span data-ttu-id="2ada7-2300">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2300">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-2301">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-2301">UI</span></span> | <span data-ttu-id="2ada7-2302">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2302">Lo</span></span> | <span data-ttu-id="2ada7-2303">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-2303">UL</span></span> | <span data-ttu-id="2ada7-2304">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2304">De</span></span> | <span data-ttu-id="2ada7-2305">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2305">Si</span></span> | <span data-ttu-id="2ada7-2306">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2306">Do</span></span> | <span data-ttu-id="2ada7-2307">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2307">Err</span></span> | <span data-ttu-id="2ada7-2308">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2308">Err</span></span> | <span data-ttu-id="2ada7-2309">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2309">Do</span></span> | <span data-ttu-id="2ada7-2310">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2310">Ob</span></span> | 
| <span data-ttu-id="2ada7-2311">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2311">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2312">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2312">Lo</span></span> | <span data-ttu-id="2ada7-2313">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2313">De</span></span> | <span data-ttu-id="2ada7-2314">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2314">De</span></span> | <span data-ttu-id="2ada7-2315">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2315">Si</span></span> | <span data-ttu-id="2ada7-2316">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2316">Do</span></span> | <span data-ttu-id="2ada7-2317">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2317">Err</span></span> | <span data-ttu-id="2ada7-2318">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2318">Err</span></span> | <span data-ttu-id="2ada7-2319">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2319">Do</span></span> | <span data-ttu-id="2ada7-2320">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2320">Ob</span></span> | 
| <span data-ttu-id="2ada7-2321">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2321">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2322">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-2322">UL</span></span> | <span data-ttu-id="2ada7-2323">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2323">De</span></span> | <span data-ttu-id="2ada7-2324">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2324">Si</span></span> | <span data-ttu-id="2ada7-2325">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2325">Do</span></span> | <span data-ttu-id="2ada7-2326">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2326">Err</span></span> | <span data-ttu-id="2ada7-2327">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2327">Err</span></span> | <span data-ttu-id="2ada7-2328">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2328">Do</span></span> | <span data-ttu-id="2ada7-2329">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2329">Ob</span></span> | 
| <span data-ttu-id="2ada7-2330">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2330">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2331">de</span><span class="sxs-lookup"><span data-stu-id="2ada7-2331">De</span></span> | <span data-ttu-id="2ada7-2332">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2332">Si</span></span> | <span data-ttu-id="2ada7-2333">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2333">Do</span></span> | <span data-ttu-id="2ada7-2334">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2334">Err</span></span> | <span data-ttu-id="2ada7-2335">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2335">Err</span></span> | <span data-ttu-id="2ada7-2336">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2336">Do</span></span> | <span data-ttu-id="2ada7-2337">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2337">Ob</span></span> | 
| <span data-ttu-id="2ada7-2338">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2338">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2339">Si</span><span class="sxs-lookup"><span data-stu-id="2ada7-2339">Si</span></span> | <span data-ttu-id="2ada7-2340">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2340">Do</span></span> | <span data-ttu-id="2ada7-2341">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2341">Err</span></span> | <span data-ttu-id="2ada7-2342">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2342">Err</span></span> | <span data-ttu-id="2ada7-2343">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2343">Do</span></span> | <span data-ttu-id="2ada7-2344">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2344">Ob</span></span> | 
| <span data-ttu-id="2ada7-2345">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2345">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2346">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2346">Do</span></span> | <span data-ttu-id="2ada7-2347">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2347">Err</span></span> | <span data-ttu-id="2ada7-2348">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2348">Err</span></span> | <span data-ttu-id="2ada7-2349">应该</span><span class="sxs-lookup"><span data-stu-id="2ada7-2349">Do</span></span> | <span data-ttu-id="2ada7-2350">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2350">Ob</span></span> | 
| <span data-ttu-id="2ada7-2351">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2351">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2352">da</span><span class="sxs-lookup"><span data-stu-id="2ada7-2352">Da</span></span>  | <span data-ttu-id="2ada7-2353">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2353">Err</span></span> | <span data-ttu-id="2ada7-2354">da</span><span class="sxs-lookup"><span data-stu-id="2ada7-2354">Da</span></span> | <span data-ttu-id="2ada7-2355">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2355">Ob</span></span> | 
| <span data-ttu-id="2ada7-2356">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2356">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-2357">ch</span><span class="sxs-lookup"><span data-stu-id="2ada7-2357">Ch</span></span>  | <span data-ttu-id="2ada7-2358">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2358">St</span></span> | <span data-ttu-id="2ada7-2359">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2359">Ob</span></span> | 
| <span data-ttu-id="2ada7-2360">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2360">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-2361">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2361">St</span></span> | <span data-ttu-id="2ada7-2362">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2362">Ob</span></span> | 
| <span data-ttu-id="2ada7-2363">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2363">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |    | <span data-ttu-id="2ada7-2364">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2364">Ob</span></span> | 


## <a name="like-operator"></a><span data-ttu-id="2ada7-2365">Like 运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-2365">Like Operator</span></span>

<span data-ttu-id="2ada7-2366">`Like`运算符确定一个字符串是否与给定的模式匹配。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2366">The `Like` operator determines whether a string matches a given pattern.</span></span>

```antlr
LikeOperatorExpression
    : Expression 'Like' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-2367">`Like`运算符定义为`String`类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2367">The `Like` operator is defined for the `String` type.</span></span> <span data-ttu-id="2ada7-2368">第一个操作数是要匹配的字符串和第二个操作数是要匹配的模式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2368">The first operand is the string being matched, and the second operand is the pattern to match against.</span></span> <span data-ttu-id="2ada7-2369">该模式由 Unicode 字符组成。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2369">The pattern is made up of Unicode characters.</span></span> <span data-ttu-id="2ada7-2370">以下字符序列具有特殊含义：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2370">The following character sequences have special meanings:</span></span>

* <span data-ttu-id="2ada7-2371">字符`?`任何单个字符匹配。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2371">The character `?` matches any single character.</span></span>

* <span data-ttu-id="2ada7-2372">字符`*`匹配零个或多个字符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2372">The character `*` matches zero or more characters.</span></span>

* <span data-ttu-id="2ada7-2373">字符`#`匹配任何单个数字 (0-9)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2373">The character `#` matches any single digit (0-9).</span></span>

* <span data-ttu-id="2ada7-2374">用方括号括起来的字符的列表 (`[ab...]`) 列表中的任何单个字符匹配。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2374">A list of characters surrounded by brackets (`[ab...]`) matches any single character in the list.</span></span>

* <span data-ttu-id="2ada7-2375">字符的列表，由括号圈定且前缀为感叹号 (`[!ab...]`) 字符列表中没有任何单个字符匹配。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2375">A list of characters surrounded by brackets and prefixed by an exclamation point (`[!ab...]`) matches any single character not in the character list.</span></span>

* <span data-ttu-id="2ada7-2376">连字符字符列表中的两个字符分隔值 (`-`) 指定范围的 Unicode 字符的第一个字符开头和结尾的第二个字符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2376">Two characters in a character list separated by a hyphen (`-`) specify a range of Unicode characters starting with the first character and ending with the second character.</span></span> <span data-ttu-id="2ada7-2377">如果第二个字符不比第一个字符在排序顺序中的更高版本，会发生运行时异常。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2377">If the second character is not later in the sort order than the first character, a run-time exception occurs.</span></span> <span data-ttu-id="2ada7-2378">连字符开头或字符列表的末尾显示指定本身。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2378">A hyphen that appears at the beginning or end of a character list specifies itself.</span></span>

<span data-ttu-id="2ada7-2379">若要匹配的特殊字符的左的括号 (`[`)，问号 (`?`)，数字符号 (`#`)，和星号 (`*`)，方括号必须将它们。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2379">To match the special characters left bracket (`[`), question mark (`?`), number sign (`#`), and asterisk (`*`), brackets must enclose them.</span></span> <span data-ttu-id="2ada7-2380">右方括号 (`]`) 不能用于组中与自身匹配，但它可以在组外使用，作为单个字符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2380">The right bracket (`]`) cannot be used within a group to match itself, but it can be used outside a group as an individual character.</span></span> <span data-ttu-id="2ada7-2381">字符序列`[]`被视为字符串文本`""`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2381">The character sequence `[]` is considered to be the string literal `""`.</span></span> 

<span data-ttu-id="2ada7-2382">请注意，字符比较和排序字符列表取决于所使用的比较类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2382">Note that character comparisons and ordering for character lists are dependent on the type of comparisons being used.</span></span> <span data-ttu-id="2ada7-2383">如果要使用二进制比较，字符比较和排序基于数值的 Unicode 值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2383">If binary comparisons are being used, character comparisons and ordering are based on the numeric Unicode values.</span></span> <span data-ttu-id="2ada7-2384">如果使用文本比较时，字符比较和排序根据所使用的.NET Framework 的当前区域设置。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2384">If text comparisons are being used, character comparisons and ordering are based on the current locale being used on the .NET Framework.</span></span>

<span data-ttu-id="2ada7-2385">在某些语言中，特殊字符的字母表示的两个不同字符中，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2385">In some languages, special characters in the alphabet represent two separate characters and vice versa.</span></span> <span data-ttu-id="2ada7-2386">例如，有几种语言使用字符`æ`来表示字符`a`和`e`出现时，它们在一起，而字符`^`和`O`可用于表示字符`Ô`.</span><span class="sxs-lookup"><span data-stu-id="2ada7-2386">For example, several languages use the character `æ` to represent the characters `a` and `e` when they appear together, while the characters `^` and `O` can be used to represent the character `Ô`.</span></span> <span data-ttu-id="2ada7-2387">使用文本比较时`Like`运算符可以识别此类文化的等效项。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2387">When using text comparisons, the `Like` operator recognizes such cultural equivalences.</span></span> <span data-ttu-id="2ada7-2388">在这种情况下，模式或字符串中的一个特殊字符的匹配项匹配其他字符串中等效的两个字符序列。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2388">In that case, an occurrence of the single special character in either pattern or string matches the equivalent two-character sequence in the other string.</span></span> <span data-ttu-id="2ada7-2389">同样，单个的特殊字符模式括在括号中 （本身，在列表中，或某个范围内） 匹配等效的两个字符序列的字符串中，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2389">Similarly, a single special character in pattern enclosed in brackets (by itself, in a list, or in a range) matches the equivalent two-character sequence in the string and vice versa.</span></span>

<span data-ttu-id="2ada7-2390">在中`Like`表达式，其中两个操作数都`Nothing`或者一个操作数具有内部函数转换为`String`和另一个操作数是`Nothing`，`Nothing`视为如同它是空的字符串文字是`""`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2390">In a `Like` expression where both operands are `Nothing` or one operand has an intrinsic conversion to `String` and the other operand is `Nothing`, `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="2ada7-2391">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2391">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-2392">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2392">__Bo__</span></span> | <span data-ttu-id="2ada7-2393">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2393">__SB__</span></span> | <span data-ttu-id="2ada7-2394">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2394">__By__</span></span> | <span data-ttu-id="2ada7-2395">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2395">__Sh__</span></span> | <span data-ttu-id="2ada7-2396">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2396">__US__</span></span> | <span data-ttu-id="2ada7-2397">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2397">__In__</span></span> | <span data-ttu-id="2ada7-2398">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2398">__UI__</span></span> | <span data-ttu-id="2ada7-2399">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2399">__Lo__</span></span> | <span data-ttu-id="2ada7-2400">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2400">__UL__</span></span> | <span data-ttu-id="2ada7-2401">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2401">__De__</span></span> | <span data-ttu-id="2ada7-2402">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2402">__Si__</span></span> | <span data-ttu-id="2ada7-2403">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2403">__Do__</span></span> | <span data-ttu-id="2ada7-2404">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2404">__Da__</span></span>  | <span data-ttu-id="2ada7-2405">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2405">__Ch__</span></span>  | <span data-ttu-id="2ada7-2406">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2406">__St__</span></span> | <span data-ttu-id="2ada7-2407">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2407">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="2ada7-2408">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2408">__Bo__</span></span> | <span data-ttu-id="2ada7-2409">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2409">St</span></span> | <span data-ttu-id="2ada7-2410">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2410">St</span></span> | <span data-ttu-id="2ada7-2411">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2411">St</span></span> | <span data-ttu-id="2ada7-2412">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2412">St</span></span> | <span data-ttu-id="2ada7-2413">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2413">St</span></span> | <span data-ttu-id="2ada7-2414">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2414">St</span></span> | <span data-ttu-id="2ada7-2415">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2415">St</span></span> | <span data-ttu-id="2ada7-2416">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2416">St</span></span> | <span data-ttu-id="2ada7-2417">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2417">St</span></span> | <span data-ttu-id="2ada7-2418">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2418">St</span></span> | <span data-ttu-id="2ada7-2419">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2419">St</span></span> | <span data-ttu-id="2ada7-2420">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2420">St</span></span> | <span data-ttu-id="2ada7-2421">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2421">St</span></span> | <span data-ttu-id="2ada7-2422">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2422">St</span></span> | <span data-ttu-id="2ada7-2423">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2423">St</span></span> | <span data-ttu-id="2ada7-2424">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2424">Ob</span></span> | 
| <span data-ttu-id="2ada7-2425">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2425">__SB__</span></span> |    | <span data-ttu-id="2ada7-2426">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2426">St</span></span> | <span data-ttu-id="2ada7-2427">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2427">St</span></span> | <span data-ttu-id="2ada7-2428">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2428">St</span></span> | <span data-ttu-id="2ada7-2429">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2429">St</span></span> | <span data-ttu-id="2ada7-2430">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2430">St</span></span> | <span data-ttu-id="2ada7-2431">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2431">St</span></span> | <span data-ttu-id="2ada7-2432">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2432">St</span></span> | <span data-ttu-id="2ada7-2433">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2433">St</span></span> | <span data-ttu-id="2ada7-2434">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2434">St</span></span> | <span data-ttu-id="2ada7-2435">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2435">St</span></span> | <span data-ttu-id="2ada7-2436">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2436">St</span></span> | <span data-ttu-id="2ada7-2437">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2437">St</span></span> | <span data-ttu-id="2ada7-2438">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2438">St</span></span> | <span data-ttu-id="2ada7-2439">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2439">St</span></span> | <span data-ttu-id="2ada7-2440">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2440">Ob</span></span> | 
| <span data-ttu-id="2ada7-2441">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2441">__By__</span></span> |    |    | <span data-ttu-id="2ada7-2442">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2442">St</span></span> | <span data-ttu-id="2ada7-2443">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2443">St</span></span> | <span data-ttu-id="2ada7-2444">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2444">St</span></span> | <span data-ttu-id="2ada7-2445">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2445">St</span></span> | <span data-ttu-id="2ada7-2446">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2446">St</span></span> | <span data-ttu-id="2ada7-2447">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2447">St</span></span> | <span data-ttu-id="2ada7-2448">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2448">St</span></span> | <span data-ttu-id="2ada7-2449">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2449">St</span></span> | <span data-ttu-id="2ada7-2450">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2450">St</span></span> | <span data-ttu-id="2ada7-2451">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2451">St</span></span> | <span data-ttu-id="2ada7-2452">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2452">St</span></span> | <span data-ttu-id="2ada7-2453">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2453">St</span></span> | <span data-ttu-id="2ada7-2454">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2454">St</span></span> | <span data-ttu-id="2ada7-2455">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2455">Ob</span></span> | 
| <span data-ttu-id="2ada7-2456">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2456">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-2457">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2457">St</span></span> | <span data-ttu-id="2ada7-2458">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2458">St</span></span> | <span data-ttu-id="2ada7-2459">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2459">St</span></span> | <span data-ttu-id="2ada7-2460">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2460">St</span></span> | <span data-ttu-id="2ada7-2461">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2461">St</span></span> | <span data-ttu-id="2ada7-2462">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2462">St</span></span> | <span data-ttu-id="2ada7-2463">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2463">St</span></span> | <span data-ttu-id="2ada7-2464">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2464">St</span></span> | <span data-ttu-id="2ada7-2465">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2465">St</span></span> | <span data-ttu-id="2ada7-2466">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2466">St</span></span> | <span data-ttu-id="2ada7-2467">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2467">St</span></span> | <span data-ttu-id="2ada7-2468">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2468">St</span></span> | <span data-ttu-id="2ada7-2469">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2469">Ob</span></span> | 
| <span data-ttu-id="2ada7-2470">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2470">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-2471">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2471">St</span></span> | <span data-ttu-id="2ada7-2472">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2472">St</span></span> | <span data-ttu-id="2ada7-2473">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2473">St</span></span> | <span data-ttu-id="2ada7-2474">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2474">St</span></span> | <span data-ttu-id="2ada7-2475">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2475">St</span></span> | <span data-ttu-id="2ada7-2476">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2476">St</span></span> | <span data-ttu-id="2ada7-2477">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2477">St</span></span> | <span data-ttu-id="2ada7-2478">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2478">St</span></span> | <span data-ttu-id="2ada7-2479">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2479">St</span></span> | <span data-ttu-id="2ada7-2480">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2480">St</span></span> | <span data-ttu-id="2ada7-2481">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2481">St</span></span> | <span data-ttu-id="2ada7-2482">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2482">Ob</span></span> | 
| <span data-ttu-id="2ada7-2483">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2483">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-2484">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2484">St</span></span> | <span data-ttu-id="2ada7-2485">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2485">St</span></span> | <span data-ttu-id="2ada7-2486">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2486">St</span></span> | <span data-ttu-id="2ada7-2487">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2487">St</span></span> | <span data-ttu-id="2ada7-2488">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2488">St</span></span> | <span data-ttu-id="2ada7-2489">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2489">St</span></span> | <span data-ttu-id="2ada7-2490">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2490">St</span></span> | <span data-ttu-id="2ada7-2491">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2491">St</span></span> | <span data-ttu-id="2ada7-2492">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2492">St</span></span> | <span data-ttu-id="2ada7-2493">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2493">St</span></span> | <span data-ttu-id="2ada7-2494">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2494">Ob</span></span> | 
| <span data-ttu-id="2ada7-2495">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2495">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-2496">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2496">St</span></span> | <span data-ttu-id="2ada7-2497">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2497">St</span></span> | <span data-ttu-id="2ada7-2498">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2498">St</span></span> | <span data-ttu-id="2ada7-2499">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2499">St</span></span> | <span data-ttu-id="2ada7-2500">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2500">St</span></span> | <span data-ttu-id="2ada7-2501">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2501">St</span></span> | <span data-ttu-id="2ada7-2502">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2502">St</span></span> | <span data-ttu-id="2ada7-2503">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2503">St</span></span> | <span data-ttu-id="2ada7-2504">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2504">St</span></span> | <span data-ttu-id="2ada7-2505">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2505">Ob</span></span> | 
| <span data-ttu-id="2ada7-2506">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2506">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2507">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2507">St</span></span> | <span data-ttu-id="2ada7-2508">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2508">St</span></span> | <span data-ttu-id="2ada7-2509">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2509">St</span></span> | <span data-ttu-id="2ada7-2510">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2510">St</span></span> | <span data-ttu-id="2ada7-2511">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2511">St</span></span> | <span data-ttu-id="2ada7-2512">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2512">St</span></span> | <span data-ttu-id="2ada7-2513">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2513">St</span></span> | <span data-ttu-id="2ada7-2514">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2514">St</span></span> | <span data-ttu-id="2ada7-2515">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2515">Ob</span></span> | 
| <span data-ttu-id="2ada7-2516">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2516">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2517">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2517">St</span></span> | <span data-ttu-id="2ada7-2518">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2518">St</span></span> | <span data-ttu-id="2ada7-2519">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2519">St</span></span> | <span data-ttu-id="2ada7-2520">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2520">St</span></span> | <span data-ttu-id="2ada7-2521">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2521">St</span></span> | <span data-ttu-id="2ada7-2522">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2522">St</span></span> | <span data-ttu-id="2ada7-2523">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2523">St</span></span> | <span data-ttu-id="2ada7-2524">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2524">Ob</span></span> | 
| <span data-ttu-id="2ada7-2525">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2525">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2526">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2526">St</span></span> | <span data-ttu-id="2ada7-2527">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2527">St</span></span> | <span data-ttu-id="2ada7-2528">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2528">St</span></span> | <span data-ttu-id="2ada7-2529">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2529">St</span></span> | <span data-ttu-id="2ada7-2530">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2530">St</span></span> | <span data-ttu-id="2ada7-2531">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2531">St</span></span> | <span data-ttu-id="2ada7-2532">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2532">Ob</span></span> | 
| <span data-ttu-id="2ada7-2533">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2533">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2534">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2534">St</span></span> | <span data-ttu-id="2ada7-2535">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2535">St</span></span> | <span data-ttu-id="2ada7-2536">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2536">St</span></span> | <span data-ttu-id="2ada7-2537">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2537">St</span></span> | <span data-ttu-id="2ada7-2538">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2538">St</span></span> | <span data-ttu-id="2ada7-2539">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2539">Ob</span></span> | 
| <span data-ttu-id="2ada7-2540">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2540">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2541">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2541">St</span></span> | <span data-ttu-id="2ada7-2542">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2542">St</span></span> | <span data-ttu-id="2ada7-2543">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2543">St</span></span> | <span data-ttu-id="2ada7-2544">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2544">St</span></span> | <span data-ttu-id="2ada7-2545">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2545">Ob</span></span> | 
| <span data-ttu-id="2ada7-2546">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2546">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2547">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2547">St</span></span> | <span data-ttu-id="2ada7-2548">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2548">St</span></span> | <span data-ttu-id="2ada7-2549">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2549">St</span></span> | <span data-ttu-id="2ada7-2550">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2550">Ob</span></span> | 
| <span data-ttu-id="2ada7-2551">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2551">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2552">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2552">St</span></span> | <span data-ttu-id="2ada7-2553">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2553">St</span></span> | <span data-ttu-id="2ada7-2554">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2554">Ob</span></span> | 
| <span data-ttu-id="2ada7-2555">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2555">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2556">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2556">St</span></span> | <span data-ttu-id="2ada7-2557">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2557">Ob</span></span> | 
| <span data-ttu-id="2ada7-2558">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2558">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2559">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2559">Ob</span></span> | 


## <a name="concatenation-operator"></a><span data-ttu-id="2ada7-2560">串联运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-2560">Concatenation Operator</span></span>

```antlr
ConcatenationOperatorExpression
    : Expression '&' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-2561">*串联运算符*定义所有内部类型，包括内部函数的值类型的可以为 null 的版本。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2561">The *concatenation operator* is defined for all of the intrinsic types, including the nullable versions of the intrinsic value types.</span></span> <span data-ttu-id="2ada7-2562">它还定义的前面提及的类型之间的串联和`System.DBNull`，也被视为`Nothing`字符串。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2562">It is also defined for concatenation between the types mentioned above and `System.DBNull`, which is treated as a `Nothing` string.</span></span> <span data-ttu-id="2ada7-2563">串联运算符将转换为其操作数的所有`String`; 在表达式中，所有转换到`String`被视为扩大转换，而不管是否使用严格的语义。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2563">The concatenation operator converts all of its operands to `String`; in the expression, all conversions to `String` are considered to be widening, regardless of whether strict semantics are used.</span></span> <span data-ttu-id="2ada7-2564">一个`System.DBNull`值转换为文本`Nothing`化为`String`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2564">A `System.DBNull` value is converted to the literal `Nothing` typed as `String`.</span></span> <span data-ttu-id="2ada7-2565">值为 null 的值类型`Nothing`也转换为文本`Nothing`化为`String`，而不是引发运行时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2565">A nullable value type whose value is `Nothing` is also converted to the literal `Nothing` typed as `String`, rather than throwing a run-time error.</span></span>

<span data-ttu-id="2ada7-2566">串联运算会导致是从左到右的顺序对两个操作数的串联的字符串。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2566">A concatenation operation results in a string that is the concatenation of the two operands in order from left to right.</span></span> <span data-ttu-id="2ada7-2567">该值`Nothing`视为如同它是空的字符串文字是`""`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2567">The value `Nothing` is treated as if it were the empty string literal `""`.</span></span>

<span data-ttu-id="2ada7-2568">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2568">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-2569">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2569">__Bo__</span></span> | <span data-ttu-id="2ada7-2570">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2570">__SB__</span></span> | <span data-ttu-id="2ada7-2571">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2571">__By__</span></span> | <span data-ttu-id="2ada7-2572">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2572">__Sh__</span></span> | <span data-ttu-id="2ada7-2573">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2573">__US__</span></span> | <span data-ttu-id="2ada7-2574">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2574">__In__</span></span> | <span data-ttu-id="2ada7-2575">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2575">__UI__</span></span> | <span data-ttu-id="2ada7-2576">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2576">__Lo__</span></span> | <span data-ttu-id="2ada7-2577">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2577">__UL__</span></span> | <span data-ttu-id="2ada7-2578">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2578">__De__</span></span> | <span data-ttu-id="2ada7-2579">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2579">__Si__</span></span> | <span data-ttu-id="2ada7-2580">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2580">__Do__</span></span> | <span data-ttu-id="2ada7-2581">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2581">__Da__</span></span>  | <span data-ttu-id="2ada7-2582">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2582">__Ch__</span></span>  | <span data-ttu-id="2ada7-2583">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2583">__St__</span></span> | <span data-ttu-id="2ada7-2584">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2584">__Ob__</span></span> | 
|--------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| <span data-ttu-id="2ada7-2585">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2585">__Bo__</span></span> | <span data-ttu-id="2ada7-2586">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2586">St</span></span> | <span data-ttu-id="2ada7-2587">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2587">St</span></span> | <span data-ttu-id="2ada7-2588">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2588">St</span></span> | <span data-ttu-id="2ada7-2589">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2589">St</span></span> | <span data-ttu-id="2ada7-2590">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2590">St</span></span> | <span data-ttu-id="2ada7-2591">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2591">St</span></span> | <span data-ttu-id="2ada7-2592">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2592">St</span></span> | <span data-ttu-id="2ada7-2593">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2593">St</span></span> | <span data-ttu-id="2ada7-2594">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2594">St</span></span> | <span data-ttu-id="2ada7-2595">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2595">St</span></span> | <span data-ttu-id="2ada7-2596">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2596">St</span></span> | <span data-ttu-id="2ada7-2597">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2597">St</span></span> | <span data-ttu-id="2ada7-2598">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2598">St</span></span> | <span data-ttu-id="2ada7-2599">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2599">St</span></span> | <span data-ttu-id="2ada7-2600">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2600">St</span></span> | <span data-ttu-id="2ada7-2601">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2601">Ob</span></span> | 
| <span data-ttu-id="2ada7-2602">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2602">__SB__</span></span> |    | <span data-ttu-id="2ada7-2603">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2603">St</span></span> | <span data-ttu-id="2ada7-2604">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2604">St</span></span> | <span data-ttu-id="2ada7-2605">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2605">St</span></span> | <span data-ttu-id="2ada7-2606">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2606">St</span></span> | <span data-ttu-id="2ada7-2607">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2607">St</span></span> | <span data-ttu-id="2ada7-2608">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2608">St</span></span> | <span data-ttu-id="2ada7-2609">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2609">St</span></span> | <span data-ttu-id="2ada7-2610">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2610">St</span></span> | <span data-ttu-id="2ada7-2611">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2611">St</span></span> | <span data-ttu-id="2ada7-2612">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2612">St</span></span> | <span data-ttu-id="2ada7-2613">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2613">St</span></span> | <span data-ttu-id="2ada7-2614">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2614">St</span></span> | <span data-ttu-id="2ada7-2615">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2615">St</span></span> | <span data-ttu-id="2ada7-2616">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2616">St</span></span> | <span data-ttu-id="2ada7-2617">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2617">Ob</span></span> | 
| <span data-ttu-id="2ada7-2618">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2618">__By__</span></span> |    |    | <span data-ttu-id="2ada7-2619">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2619">St</span></span> | <span data-ttu-id="2ada7-2620">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2620">St</span></span> | <span data-ttu-id="2ada7-2621">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2621">St</span></span> | <span data-ttu-id="2ada7-2622">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2622">St</span></span> | <span data-ttu-id="2ada7-2623">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2623">St</span></span> | <span data-ttu-id="2ada7-2624">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2624">St</span></span> | <span data-ttu-id="2ada7-2625">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2625">St</span></span> | <span data-ttu-id="2ada7-2626">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2626">St</span></span> | <span data-ttu-id="2ada7-2627">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2627">St</span></span> | <span data-ttu-id="2ada7-2628">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2628">St</span></span> | <span data-ttu-id="2ada7-2629">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2629">St</span></span> | <span data-ttu-id="2ada7-2630">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2630">St</span></span> | <span data-ttu-id="2ada7-2631">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2631">St</span></span> | <span data-ttu-id="2ada7-2632">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2632">Ob</span></span> | 
| <span data-ttu-id="2ada7-2633">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2633">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-2634">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2634">St</span></span> | <span data-ttu-id="2ada7-2635">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2635">St</span></span> | <span data-ttu-id="2ada7-2636">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2636">St</span></span> | <span data-ttu-id="2ada7-2637">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2637">St</span></span> | <span data-ttu-id="2ada7-2638">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2638">St</span></span> | <span data-ttu-id="2ada7-2639">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2639">St</span></span> | <span data-ttu-id="2ada7-2640">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2640">St</span></span> | <span data-ttu-id="2ada7-2641">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2641">St</span></span> | <span data-ttu-id="2ada7-2642">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2642">St</span></span> | <span data-ttu-id="2ada7-2643">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2643">St</span></span> | <span data-ttu-id="2ada7-2644">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2644">St</span></span> | <span data-ttu-id="2ada7-2645">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2645">St</span></span> | <span data-ttu-id="2ada7-2646">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2646">Ob</span></span> | 
| <span data-ttu-id="2ada7-2647">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2647">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-2648">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2648">St</span></span> | <span data-ttu-id="2ada7-2649">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2649">St</span></span> | <span data-ttu-id="2ada7-2650">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2650">St</span></span> | <span data-ttu-id="2ada7-2651">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2651">St</span></span> | <span data-ttu-id="2ada7-2652">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2652">St</span></span> | <span data-ttu-id="2ada7-2653">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2653">St</span></span> | <span data-ttu-id="2ada7-2654">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2654">St</span></span> | <span data-ttu-id="2ada7-2655">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2655">St</span></span> | <span data-ttu-id="2ada7-2656">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2656">St</span></span> | <span data-ttu-id="2ada7-2657">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2657">St</span></span> | <span data-ttu-id="2ada7-2658">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2658">St</span></span> | <span data-ttu-id="2ada7-2659">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2659">Ob</span></span> | 
| <span data-ttu-id="2ada7-2660">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2660">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-2661">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2661">St</span></span> | <span data-ttu-id="2ada7-2662">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2662">St</span></span> | <span data-ttu-id="2ada7-2663">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2663">St</span></span> | <span data-ttu-id="2ada7-2664">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2664">St</span></span> | <span data-ttu-id="2ada7-2665">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2665">St</span></span> | <span data-ttu-id="2ada7-2666">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2666">St</span></span> | <span data-ttu-id="2ada7-2667">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2667">St</span></span> | <span data-ttu-id="2ada7-2668">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2668">St</span></span> | <span data-ttu-id="2ada7-2669">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2669">St</span></span> | <span data-ttu-id="2ada7-2670">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2670">St</span></span> | <span data-ttu-id="2ada7-2671">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2671">Ob</span></span> | 
| <span data-ttu-id="2ada7-2672">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2672">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-2673">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2673">St</span></span> | <span data-ttu-id="2ada7-2674">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2674">St</span></span> | <span data-ttu-id="2ada7-2675">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2675">St</span></span> | <span data-ttu-id="2ada7-2676">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2676">St</span></span> | <span data-ttu-id="2ada7-2677">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2677">St</span></span> | <span data-ttu-id="2ada7-2678">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2678">St</span></span> | <span data-ttu-id="2ada7-2679">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2679">St</span></span> | <span data-ttu-id="2ada7-2680">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2680">St</span></span> | <span data-ttu-id="2ada7-2681">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2681">St</span></span> | <span data-ttu-id="2ada7-2682">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2682">Ob</span></span> | 
| <span data-ttu-id="2ada7-2683">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2683">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2684">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2684">St</span></span> | <span data-ttu-id="2ada7-2685">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2685">St</span></span> | <span data-ttu-id="2ada7-2686">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2686">St</span></span> | <span data-ttu-id="2ada7-2687">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2687">St</span></span> | <span data-ttu-id="2ada7-2688">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2688">St</span></span> | <span data-ttu-id="2ada7-2689">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2689">St</span></span> | <span data-ttu-id="2ada7-2690">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2690">St</span></span> | <span data-ttu-id="2ada7-2691">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2691">St</span></span> | <span data-ttu-id="2ada7-2692">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2692">Ob</span></span> | 
| <span data-ttu-id="2ada7-2693">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2693">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2694">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2694">St</span></span> | <span data-ttu-id="2ada7-2695">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2695">St</span></span> | <span data-ttu-id="2ada7-2696">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2696">St</span></span> | <span data-ttu-id="2ada7-2697">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2697">St</span></span> | <span data-ttu-id="2ada7-2698">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2698">St</span></span> | <span data-ttu-id="2ada7-2699">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2699">St</span></span> | <span data-ttu-id="2ada7-2700">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2700">St</span></span> | <span data-ttu-id="2ada7-2701">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2701">Ob</span></span> | 
| <span data-ttu-id="2ada7-2702">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2702">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2703">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2703">St</span></span> | <span data-ttu-id="2ada7-2704">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2704">St</span></span> | <span data-ttu-id="2ada7-2705">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2705">St</span></span> | <span data-ttu-id="2ada7-2706">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2706">St</span></span> | <span data-ttu-id="2ada7-2707">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2707">St</span></span> | <span data-ttu-id="2ada7-2708">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2708">St</span></span> | <span data-ttu-id="2ada7-2709">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2709">Ob</span></span> | 
| <span data-ttu-id="2ada7-2710">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2710">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2711">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2711">St</span></span> | <span data-ttu-id="2ada7-2712">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2712">St</span></span> | <span data-ttu-id="2ada7-2713">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2713">St</span></span> | <span data-ttu-id="2ada7-2714">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2714">St</span></span> | <span data-ttu-id="2ada7-2715">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2715">St</span></span> | <span data-ttu-id="2ada7-2716">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2716">Ob</span></span> | 
| <span data-ttu-id="2ada7-2717">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2717">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2718">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2718">St</span></span> | <span data-ttu-id="2ada7-2719">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2719">St</span></span> | <span data-ttu-id="2ada7-2720">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2720">St</span></span> | <span data-ttu-id="2ada7-2721">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2721">St</span></span> | <span data-ttu-id="2ada7-2722">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2722">Ob</span></span> | 
| <span data-ttu-id="2ada7-2723">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2723">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2724">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2724">St</span></span> | <span data-ttu-id="2ada7-2725">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2725">St</span></span> | <span data-ttu-id="2ada7-2726">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2726">St</span></span> | <span data-ttu-id="2ada7-2727">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2727">Ob</span></span> | 
| <span data-ttu-id="2ada7-2728">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2728">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2729">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2729">St</span></span> | <span data-ttu-id="2ada7-2730">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2730">St</span></span> | <span data-ttu-id="2ada7-2731">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2731">Ob</span></span> | 
| <span data-ttu-id="2ada7-2732">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2732">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2733">st</span><span class="sxs-lookup"><span data-stu-id="2ada7-2733">St</span></span> | <span data-ttu-id="2ada7-2734">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2734">Ob</span></span> | 
| <span data-ttu-id="2ada7-2735">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2735">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2736">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2736">Ob</span></span> | 


## <a name="logical-operators"></a><span data-ttu-id="2ada7-2737">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-2737">Logical Operators</span></span>

<span data-ttu-id="2ada7-2738">`And`， `Not`， `Or`，和`Xor`操作员称为逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2738">The `And`, `Not`, `Or`, and `Xor` operators are called the logical operators.</span></span>

```antlr
LogicalOperatorExpression
    : 'Not' Expression
    | Expression 'And' LineTerminator? Expression
    | Expression 'Or' LineTerminator? Expression
    | Expression 'Xor' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-2739">逻辑运算符计算，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2739">The logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="2ada7-2740">有关`Boolean`类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2740">For the `Boolean` type:</span></span>

  * <span data-ttu-id="2ada7-2741">一个逻辑`And`对其两个操作数执行操作。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2741">A logical `And` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="2ada7-2742">一个逻辑`Not`对其操作数执行操作。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2742">A logical `Not` operation is performed on its operand.</span></span>

  * <span data-ttu-id="2ada7-2743">一个逻辑`Or`对其两个操作数执行操作。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2743">A logical `Or` operation is performed on its two operands.</span></span>

  * <span data-ttu-id="2ada7-2744">逻辑异-`Or`对其两个操作数执行操作。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2744">A logical exclusive-`Or` operation is performed on its two operands.</span></span>

* <span data-ttu-id="2ada7-2745">有关`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`，和所有枚举类型，将指定的操作执行的二进制表示形式的每一位两个操作数：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2745">For `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, and all enumerated types, the specified operation is performed on each bit of the binary representation of the two operand(s):</span></span>

  * <span data-ttu-id="2ada7-2746">`And`：结果位是 1，如果两个位都为 1;否则，结果位为 0。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2746">`And`: The result bit is 1 if both bits are 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="2ada7-2747">`Not`：结果位是 1，如果位为 0;否则，结果位为 1。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2747">`Not`: The result bit is 1 if the bit is 0; otherwise the result bit is 1.</span></span>

  * <span data-ttu-id="2ada7-2748">`Or`：结果位是 1，如果任一位为 1;否则，结果位为 0。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2748">`Or`: The result bit is 1 if either bit is 1; otherwise the result bit is 0.</span></span>

  * <span data-ttu-id="2ada7-2749">`Xor`：结果位是 1，如果任一位为 1，但不是能同时位;否则，结果位为 0 (即 1 `Xor` 0 = 1，1 `Xor` 1 = 0)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2749">`Xor`: The result bit is 1 if either bit is 1 but not both bits; otherwise the result bit is 0 (that is, 1 `Xor` 0 = 1, 1 `Xor` 1 = 0).</span></span>

* <span data-ttu-id="2ada7-2750">当逻辑运算符`And`并`Or`类型提升`Boolean?`，它们已得到扩展以包含三个值布尔逻辑这种情况下：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2750">When the logical operators `And` and `Or` are lifted for the type `Boolean?`, they are extended to encompass three-valued Boolean logic as such:</span></span>

  * <span data-ttu-id="2ada7-2751">`And` 如果这两个操作数都为 true; 计算结果为 true其中一个操作数为 false; 否则，false`Nothing`否则为。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2751">`And` evaluates to true if both operands are true; false if one of the operands is false; `Nothing` otherwise.</span></span>

  * <span data-ttu-id="2ada7-2752">`Or` 计算结果为 true，如果其中一个操作数为 true;false 是两个操作数都为 false;`Nothing`否则为。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2752">`Or` evaluates to true if either operand is true; false is both operands are false; `Nothing` otherwise.</span></span>

<span data-ttu-id="2ada7-2753">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2753">For example:</span></span>

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

<span data-ttu-id="2ada7-2754">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2754">__Note.__</span></span> <span data-ttu-id="2ada7-2755">理想情况下，逻辑运算符`And`和`Or`将使用三值逻辑可以使用布尔表达式中任何类型提升 (即实现的类型`IsTrue`和`IsFalse`)，在相同的方式`AndAlso`和`OrElse`短路在可以使用布尔表达式中所有类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2755">Ideally, the logical operators `And` and `Or` would be lifted using three-valued logic for any type that can be used in a Boolean expression (i.e. a type that implements `IsTrue` and `IsFalse`), in the same way that `AndAlso` and `OrElse` short circuit across any type that can be used in a Boolean expression.</span></span> <span data-ttu-id="2ada7-2756">遗憾的是，三值提升只应用于`Boolean?`，因此需三值逻辑的用户定义类型必须手动执行此操作通过定义`And`和`Or`它们可以为 null 的版本的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2756">Unfortunately, three-valued lifting is only applied to `Boolean?`, so user-defined types that desire three-valued logic must do so manually by defining `And` and `Or` operators for their nullable version.</span></span>

<span data-ttu-id="2ada7-2757">从这些操作可能不会出现任何溢出。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2757">No overflows are possible from these operations.</span></span> <span data-ttu-id="2ada7-2758">枚举的类型运算符执行按位运算的基础类型的枚举类型，但返回的值是枚举的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2758">The enumerated type operators do the bitwise operation on the underlying type of the enumerated type, but the return value is the enumerated type.</span></span>

<span data-ttu-id="2ada7-2759">__不是操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2759">__Not Operation Type:__</span></span>

| <span data-ttu-id="2ada7-2760">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2760">__Bo__</span></span> | <span data-ttu-id="2ada7-2761">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2761">__SB__</span></span> | <span data-ttu-id="2ada7-2762">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2762">__By__</span></span> | <span data-ttu-id="2ada7-2763">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2763">__Sh__</span></span> | <span data-ttu-id="2ada7-2764">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2764">__US__</span></span> | <span data-ttu-id="2ada7-2765">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2765">__In__</span></span> | <span data-ttu-id="2ada7-2766">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2766">__UI__</span></span> | <span data-ttu-id="2ada7-2767">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2767">__Lo__</span></span> | <span data-ttu-id="2ada7-2768">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2768">__UL__</span></span> | <span data-ttu-id="2ada7-2769">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2769">__De__</span></span> | <span data-ttu-id="2ada7-2770">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2770">__Si__</span></span> | <span data-ttu-id="2ada7-2771">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2771">__Do__</span></span> | <span data-ttu-id="2ada7-2772">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2772">__Da__</span></span>  | <span data-ttu-id="2ada7-2773">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2773">__Ch__</span></span>  | <span data-ttu-id="2ada7-2774">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2774">__St__</span></span> | <span data-ttu-id="2ada7-2775">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2775">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="2ada7-2776">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2776">Bo</span></span> | <span data-ttu-id="2ada7-2777">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-2777">SB</span></span> | <span data-ttu-id="2ada7-2778">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-2778">By</span></span> | <span data-ttu-id="2ada7-2779">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2779">Sh</span></span> | <span data-ttu-id="2ada7-2780">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-2780">US</span></span> | <span data-ttu-id="2ada7-2781">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2781">In</span></span> | <span data-ttu-id="2ada7-2782">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-2782">UI</span></span> | <span data-ttu-id="2ada7-2783">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2783">Lo</span></span> | <span data-ttu-id="2ada7-2784">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-2784">UL</span></span> | <span data-ttu-id="2ada7-2785">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2785">Lo</span></span> | <span data-ttu-id="2ada7-2786">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2786">Lo</span></span> | <span data-ttu-id="2ada7-2787">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2787">Lo</span></span> | <span data-ttu-id="2ada7-2788">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2788">Err</span></span> | <span data-ttu-id="2ada7-2789">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2789">Err</span></span> | <span data-ttu-id="2ada7-2790">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2790">Lo</span></span> | <span data-ttu-id="2ada7-2791">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2791">Ob</span></span> | 

<span data-ttu-id="2ada7-2792">__并且，或 Xor 操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2792">__And, Or, Xor Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-2793">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2793">__Bo__</span></span> | <span data-ttu-id="2ada7-2794">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2794">__SB__</span></span> | <span data-ttu-id="2ada7-2795">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2795">__By__</span></span> | <span data-ttu-id="2ada7-2796">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2796">__Sh__</span></span> | <span data-ttu-id="2ada7-2797">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2797">__US__</span></span> | <span data-ttu-id="2ada7-2798">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2798">__In__</span></span> | <span data-ttu-id="2ada7-2799">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2799">__UI__</span></span> | <span data-ttu-id="2ada7-2800">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2800">__Lo__</span></span> | <span data-ttu-id="2ada7-2801">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2801">__UL__</span></span> | <span data-ttu-id="2ada7-2802">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2802">__De__</span></span> | <span data-ttu-id="2ada7-2803">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2803">__Si__</span></span> | <span data-ttu-id="2ada7-2804">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2804">__Do__</span></span> | <span data-ttu-id="2ada7-2805">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2805">__Da__</span></span>  | <span data-ttu-id="2ada7-2806">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2806">__Ch__</span></span>  | <span data-ttu-id="2ada7-2807">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2807">__St__</span></span> | <span data-ttu-id="2ada7-2808">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2808">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="2ada7-2809">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2809">__Bo__</span></span> | <span data-ttu-id="2ada7-2810">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2810">Bo</span></span> | <span data-ttu-id="2ada7-2811">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-2811">SB</span></span> | <span data-ttu-id="2ada7-2812">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2812">Sh</span></span> | <span data-ttu-id="2ada7-2813">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2813">Sh</span></span> | <span data-ttu-id="2ada7-2814">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2814">In</span></span> | <span data-ttu-id="2ada7-2815">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2815">In</span></span> | <span data-ttu-id="2ada7-2816">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2816">Lo</span></span> | <span data-ttu-id="2ada7-2817">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2817">Lo</span></span> | <span data-ttu-id="2ada7-2818">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2818">Lo</span></span> | <span data-ttu-id="2ada7-2819">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2819">Lo</span></span> | <span data-ttu-id="2ada7-2820">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2820">Lo</span></span> | <span data-ttu-id="2ada7-2821">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2821">Lo</span></span> | <span data-ttu-id="2ada7-2822">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2822">Err</span></span> | <span data-ttu-id="2ada7-2823">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2823">Err</span></span> | <span data-ttu-id="2ada7-2824">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2824">Bo</span></span>  | <span data-ttu-id="2ada7-2825">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2825">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2826">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2826">__SB__</span></span> |    | <span data-ttu-id="2ada7-2827">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-2827">SB</span></span> | <span data-ttu-id="2ada7-2828">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2828">Sh</span></span> | <span data-ttu-id="2ada7-2829">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2829">Sh</span></span> | <span data-ttu-id="2ada7-2830">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2830">In</span></span> | <span data-ttu-id="2ada7-2831">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2831">In</span></span> | <span data-ttu-id="2ada7-2832">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2832">Lo</span></span> | <span data-ttu-id="2ada7-2833">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2833">Lo</span></span> | <span data-ttu-id="2ada7-2834">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2834">Lo</span></span> | <span data-ttu-id="2ada7-2835">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2835">Lo</span></span> | <span data-ttu-id="2ada7-2836">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2836">Lo</span></span> | <span data-ttu-id="2ada7-2837">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2837">Lo</span></span> | <span data-ttu-id="2ada7-2838">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2838">Err</span></span> | <span data-ttu-id="2ada7-2839">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2839">Err</span></span> | <span data-ttu-id="2ada7-2840">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2840">Lo</span></span>  | <span data-ttu-id="2ada7-2841">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2841">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2842">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2842">__By__</span></span> |    |    | <span data-ttu-id="2ada7-2843">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-2843">By</span></span> | <span data-ttu-id="2ada7-2844">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2844">Sh</span></span> | <span data-ttu-id="2ada7-2845">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-2845">US</span></span> | <span data-ttu-id="2ada7-2846">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2846">In</span></span> | <span data-ttu-id="2ada7-2847">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-2847">UI</span></span> | <span data-ttu-id="2ada7-2848">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2848">Lo</span></span> | <span data-ttu-id="2ada7-2849">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-2849">UL</span></span> | <span data-ttu-id="2ada7-2850">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2850">Lo</span></span> | <span data-ttu-id="2ada7-2851">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2851">Lo</span></span> | <span data-ttu-id="2ada7-2852">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2852">Lo</span></span> | <span data-ttu-id="2ada7-2853">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2853">Err</span></span> | <span data-ttu-id="2ada7-2854">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2854">Err</span></span> | <span data-ttu-id="2ada7-2855">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2855">Lo</span></span>  | <span data-ttu-id="2ada7-2856">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2856">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2857">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2857">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-2858">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-2858">Sh</span></span> | <span data-ttu-id="2ada7-2859">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2859">In</span></span> | <span data-ttu-id="2ada7-2860">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2860">In</span></span> | <span data-ttu-id="2ada7-2861">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2861">Lo</span></span> | <span data-ttu-id="2ada7-2862">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2862">Lo</span></span> | <span data-ttu-id="2ada7-2863">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2863">Lo</span></span> | <span data-ttu-id="2ada7-2864">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2864">Lo</span></span> | <span data-ttu-id="2ada7-2865">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2865">Lo</span></span> | <span data-ttu-id="2ada7-2866">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2866">Lo</span></span> | <span data-ttu-id="2ada7-2867">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2867">Err</span></span> | <span data-ttu-id="2ada7-2868">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2868">Err</span></span> | <span data-ttu-id="2ada7-2869">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2869">Lo</span></span>  | <span data-ttu-id="2ada7-2870">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2870">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2871">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2871">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-2872">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-2872">US</span></span> | <span data-ttu-id="2ada7-2873">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2873">In</span></span> | <span data-ttu-id="2ada7-2874">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-2874">UI</span></span> | <span data-ttu-id="2ada7-2875">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2875">Lo</span></span> | <span data-ttu-id="2ada7-2876">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-2876">UL</span></span> | <span data-ttu-id="2ada7-2877">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2877">Lo</span></span> | <span data-ttu-id="2ada7-2878">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2878">Lo</span></span> | <span data-ttu-id="2ada7-2879">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2879">Lo</span></span> | <span data-ttu-id="2ada7-2880">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2880">Err</span></span> | <span data-ttu-id="2ada7-2881">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2881">Err</span></span> | <span data-ttu-id="2ada7-2882">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2882">Lo</span></span>  | <span data-ttu-id="2ada7-2883">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2883">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2884">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2884">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-2885">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-2885">In</span></span> | <span data-ttu-id="2ada7-2886">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2886">Lo</span></span> | <span data-ttu-id="2ada7-2887">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2887">Lo</span></span> | <span data-ttu-id="2ada7-2888">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2888">Lo</span></span> | <span data-ttu-id="2ada7-2889">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2889">Lo</span></span> | <span data-ttu-id="2ada7-2890">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2890">Lo</span></span> | <span data-ttu-id="2ada7-2891">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2891">Lo</span></span> | <span data-ttu-id="2ada7-2892">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2892">Err</span></span> | <span data-ttu-id="2ada7-2893">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2893">Err</span></span> | <span data-ttu-id="2ada7-2894">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2894">Lo</span></span>  | <span data-ttu-id="2ada7-2895">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2895">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2896">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2896">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-2897">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-2897">UI</span></span> | <span data-ttu-id="2ada7-2898">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2898">Lo</span></span> | <span data-ttu-id="2ada7-2899">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-2899">UL</span></span> | <span data-ttu-id="2ada7-2900">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2900">Lo</span></span> | <span data-ttu-id="2ada7-2901">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2901">Lo</span></span> | <span data-ttu-id="2ada7-2902">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2902">Lo</span></span> | <span data-ttu-id="2ada7-2903">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2903">Err</span></span> | <span data-ttu-id="2ada7-2904">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2904">Err</span></span> | <span data-ttu-id="2ada7-2905">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2905">Lo</span></span>  | <span data-ttu-id="2ada7-2906">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2906">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2907">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2907">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2908">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2908">Lo</span></span> | <span data-ttu-id="2ada7-2909">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2909">Lo</span></span> | <span data-ttu-id="2ada7-2910">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2910">Lo</span></span> | <span data-ttu-id="2ada7-2911">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2911">Lo</span></span> | <span data-ttu-id="2ada7-2912">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2912">Lo</span></span> | <span data-ttu-id="2ada7-2913">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2913">Err</span></span> | <span data-ttu-id="2ada7-2914">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2914">Err</span></span> | <span data-ttu-id="2ada7-2915">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2915">Lo</span></span>  | <span data-ttu-id="2ada7-2916">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2916">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2917">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2917">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2918">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-2918">UL</span></span> | <span data-ttu-id="2ada7-2919">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2919">Lo</span></span> | <span data-ttu-id="2ada7-2920">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2920">Lo</span></span> | <span data-ttu-id="2ada7-2921">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2921">Lo</span></span> | <span data-ttu-id="2ada7-2922">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2922">Err</span></span> | <span data-ttu-id="2ada7-2923">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2923">Err</span></span> | <span data-ttu-id="2ada7-2924">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2924">Lo</span></span>  | <span data-ttu-id="2ada7-2925">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2925">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2926">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2926">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2927">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2927">Lo</span></span> | <span data-ttu-id="2ada7-2928">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2928">Lo</span></span> | <span data-ttu-id="2ada7-2929">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2929">Lo</span></span> | <span data-ttu-id="2ada7-2930">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2930">Err</span></span> | <span data-ttu-id="2ada7-2931">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2931">Err</span></span> | <span data-ttu-id="2ada7-2932">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2932">Lo</span></span>  | <span data-ttu-id="2ada7-2933">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2933">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2934">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2934">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2935">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2935">Lo</span></span> | <span data-ttu-id="2ada7-2936">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2936">Lo</span></span> | <span data-ttu-id="2ada7-2937">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2937">Err</span></span> | <span data-ttu-id="2ada7-2938">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2938">Err</span></span> | <span data-ttu-id="2ada7-2939">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2939">Lo</span></span>  | <span data-ttu-id="2ada7-2940">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2940">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2941">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2941">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2942">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2942">Lo</span></span> | <span data-ttu-id="2ada7-2943">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2943">Err</span></span> | <span data-ttu-id="2ada7-2944">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2944">Err</span></span> | <span data-ttu-id="2ada7-2945">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2945">Lo</span></span>  | <span data-ttu-id="2ada7-2946">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2946">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2947">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2947">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-2948">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2948">Err</span></span> | <span data-ttu-id="2ada7-2949">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2949">Err</span></span> | <span data-ttu-id="2ada7-2950">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2950">Err</span></span> | <span data-ttu-id="2ada7-2951">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2951">Err</span></span> | 
| <span data-ttu-id="2ada7-2952">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2952">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-2953">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2953">Err</span></span> | <span data-ttu-id="2ada7-2954">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2954">Err</span></span> | <span data-ttu-id="2ada7-2955">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-2955">Err</span></span> | 
| <span data-ttu-id="2ada7-2956">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2956">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-2957">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2957">Lo</span></span>  | <span data-ttu-id="2ada7-2958">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2958">Ob</span></span>  | 
| <span data-ttu-id="2ada7-2959">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2959">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="2ada7-2960">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-2960">Ob</span></span>  | 


### <a name="short-circuiting-logical-operators"></a><span data-ttu-id="2ada7-2961">短路逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-2961">Short-circuiting Logical Operators</span></span>

<span data-ttu-id="2ada7-2962">`AndAlso`并`OrElse`运算符会短路新版`And`和`Or`逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2962">The `AndAlso` and `OrElse` operators are the short-circuiting versions of the `And` and `Or` logical operators.</span></span>

```antlr
ShortCircuitLogicalOperatorExpression
    : Expression 'AndAlso' LineTerminator? Expression
    | Expression 'OrElse' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-2963">由于其短短路行为，第二个操作数是未在运行时计算如果评估的第一个操作数后已知的运算符的结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2963">Because of their short circuiting behavior, the second operand is not evaluated at run time if the operator result is known after evaluating the first operand.</span></span>

<span data-ttu-id="2ada7-2964">短路逻辑运算符计算，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2964">The short-circuiting logical operators are evaluated as follows:</span></span>

* <span data-ttu-id="2ada7-2965">如果中的第一个操作数`AndAlso`运算计算结果为`False`，则返回 True 中的或其`IsFalse`运算符，该表达式将返回第一个操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2965">If the first operand in an `AndAlso` operation evaluates to `False` or returns True from its `IsFalse` operator, the expression returns its first operand.</span></span> <span data-ttu-id="2ada7-2966">否则，第二个操作数是计算和逻辑`And`两个结果上执行操作。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2966">Otherwise, the second operand is evaluated and a logical `And` operation is performed on the two results.</span></span>

* <span data-ttu-id="2ada7-2967">如果中的第一个操作数`OrElse`运算计算结果为`True`，则返回 True 中的或其`IsTrue`运算符，该表达式将返回第一个操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2967">If the first operand in an `OrElse` operation evaluates to `True` or returns True from its `IsTrue` operator, the expression returns its first operand.</span></span> <span data-ttu-id="2ada7-2968">否则，第二个操作数是计算和逻辑`Or`其两个结果上执行操作。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2968">Otherwise, the second operand is evaluated and a logical `Or` operation is performed on its two results.</span></span>

<span data-ttu-id="2ada7-2969">`AndAlso`并`OrElse`类型定义运算符`Boolean`，或任何类型的`T`重载以下运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2969">The `AndAlso` and `OrElse` operators are defined for the type `Boolean`, or for any type `T` that overloads the following operators:</span></span>

```vb
Public Shared Operator IsTrue(op As T) As Boolean
Public Shared Operator IsFalse(op As T) As Boolean
```

<span data-ttu-id="2ada7-2970">以及重载相应`And`或`Or`运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2970">as well as overloading the corresponding `And` or `Or` operator:</span></span>

```vb
Public Shared Operator And(op1 As T, op2 As T) As T
Public Shared Operator Or(op1 As T, op2 As T) As T
```

<span data-ttu-id="2ada7-2971">评估时`AndAlso`或`OrElse`运算符，只有一次计算的第一个操作数和第二个操作数是不会计算或仅计算一次。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2971">When evaluating the `AndAlso` or `OrElse` operators, the first operand is evaluated only once, and the second operand is either not evaluated or evaluated exactly once.</span></span> <span data-ttu-id="2ada7-2972">例如，考虑以下代码：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2972">For example, consider the following code:</span></span>

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

<span data-ttu-id="2ada7-2973">它将打印以下结果：</span><span class="sxs-lookup"><span data-stu-id="2ada7-2973">It prints the following result:</span></span>

```
And: False True
Or: True False
AndAlso: False
OrElse: True
```

<span data-ttu-id="2ada7-2974">形式的提升`AndAlso`并`OrElse`运算符，如果第一个操作数为 null `Boolean?`，然后第二个操作数的求值，但结果始终为 null `Boolean?`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-2974">In the lifted form of the `AndAlso` and `OrElse` operators, if the first operand was a null `Boolean?`, then the second operand is evaluated but the result is always a null `Boolean?`.</span></span>

<span data-ttu-id="2ada7-2975">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2975">__Operation Type:__</span></span>

|        | <span data-ttu-id="2ada7-2976">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2976">__Bo__</span></span> | <span data-ttu-id="2ada7-2977">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2977">__SB__</span></span> | <span data-ttu-id="2ada7-2978">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2978">__By__</span></span> | <span data-ttu-id="2ada7-2979">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2979">__Sh__</span></span> | <span data-ttu-id="2ada7-2980">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2980">__US__</span></span> | <span data-ttu-id="2ada7-2981">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2981">__In__</span></span> | <span data-ttu-id="2ada7-2982">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2982">__UI__</span></span> | <span data-ttu-id="2ada7-2983">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2983">__Lo__</span></span> | <span data-ttu-id="2ada7-2984">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2984">__UL__</span></span> | <span data-ttu-id="2ada7-2985">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2985">__De__</span></span> | <span data-ttu-id="2ada7-2986">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2986">__Si__</span></span> | <span data-ttu-id="2ada7-2987">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2987">__Do__</span></span> | <span data-ttu-id="2ada7-2988">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2988">__Da__</span></span>  | <span data-ttu-id="2ada7-2989">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2989">__Ch__</span></span>  | <span data-ttu-id="2ada7-2990">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2990">__St__</span></span> | <span data-ttu-id="2ada7-2991">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2991">__Ob__</span></span> |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|-----|-----|
| <span data-ttu-id="2ada7-2992">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-2992">__Bo__</span></span> | <span data-ttu-id="2ada7-2993">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2993">Bo</span></span> | <span data-ttu-id="2ada7-2994">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2994">Bo</span></span> | <span data-ttu-id="2ada7-2995">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2995">Bo</span></span> | <span data-ttu-id="2ada7-2996">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2996">Bo</span></span> | <span data-ttu-id="2ada7-2997">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2997">Bo</span></span> | <span data-ttu-id="2ada7-2998">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2998">Bo</span></span> | <span data-ttu-id="2ada7-2999">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-2999">Bo</span></span> | <span data-ttu-id="2ada7-3000">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3000">Bo</span></span> | <span data-ttu-id="2ada7-3001">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3001">Bo</span></span> | <span data-ttu-id="2ada7-3002">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3002">Bo</span></span> | <span data-ttu-id="2ada7-3003">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3003">Bo</span></span> | <span data-ttu-id="2ada7-3004">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3004">Bo</span></span> | <span data-ttu-id="2ada7-3005">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3005">Err</span></span> | <span data-ttu-id="2ada7-3006">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3006">Err</span></span> | <span data-ttu-id="2ada7-3007">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3007">Bo</span></span>  | <span data-ttu-id="2ada7-3008">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3008">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3009">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3009">__SB__</span></span> |    | <span data-ttu-id="2ada7-3010">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3010">Bo</span></span> | <span data-ttu-id="2ada7-3011">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3011">Bo</span></span> | <span data-ttu-id="2ada7-3012">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3012">Bo</span></span> | <span data-ttu-id="2ada7-3013">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3013">Bo</span></span> | <span data-ttu-id="2ada7-3014">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3014">Bo</span></span> | <span data-ttu-id="2ada7-3015">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3015">Bo</span></span> | <span data-ttu-id="2ada7-3016">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3016">Bo</span></span> | <span data-ttu-id="2ada7-3017">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3017">Bo</span></span> | <span data-ttu-id="2ada7-3018">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3018">Bo</span></span> | <span data-ttu-id="2ada7-3019">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3019">Bo</span></span> | <span data-ttu-id="2ada7-3020">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3020">Bo</span></span> | <span data-ttu-id="2ada7-3021">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3021">Err</span></span> | <span data-ttu-id="2ada7-3022">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3022">Err</span></span> | <span data-ttu-id="2ada7-3023">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3023">Bo</span></span>  | <span data-ttu-id="2ada7-3024">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3024">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3025">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3025">__By__</span></span> |    |    | <span data-ttu-id="2ada7-3026">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3026">Bo</span></span> | <span data-ttu-id="2ada7-3027">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3027">Bo</span></span> | <span data-ttu-id="2ada7-3028">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3028">Bo</span></span> | <span data-ttu-id="2ada7-3029">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3029">Bo</span></span> | <span data-ttu-id="2ada7-3030">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3030">Bo</span></span> | <span data-ttu-id="2ada7-3031">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3031">Bo</span></span> | <span data-ttu-id="2ada7-3032">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3032">Bo</span></span> | <span data-ttu-id="2ada7-3033">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3033">Bo</span></span> | <span data-ttu-id="2ada7-3034">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3034">Bo</span></span> | <span data-ttu-id="2ada7-3035">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3035">Bo</span></span> | <span data-ttu-id="2ada7-3036">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3036">Err</span></span> | <span data-ttu-id="2ada7-3037">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3037">Err</span></span> | <span data-ttu-id="2ada7-3038">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3038">Bo</span></span>  | <span data-ttu-id="2ada7-3039">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3039">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3040">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3040">__Sh__</span></span> |    |    |    | <span data-ttu-id="2ada7-3041">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3041">Bo</span></span> | <span data-ttu-id="2ada7-3042">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3042">Bo</span></span> | <span data-ttu-id="2ada7-3043">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3043">Bo</span></span> | <span data-ttu-id="2ada7-3044">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3044">Bo</span></span> | <span data-ttu-id="2ada7-3045">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3045">Bo</span></span> | <span data-ttu-id="2ada7-3046">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3046">Bo</span></span> | <span data-ttu-id="2ada7-3047">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3047">Bo</span></span> | <span data-ttu-id="2ada7-3048">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3048">Bo</span></span> | <span data-ttu-id="2ada7-3049">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3049">Bo</span></span> | <span data-ttu-id="2ada7-3050">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3050">Err</span></span> | <span data-ttu-id="2ada7-3051">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3051">Err</span></span> | <span data-ttu-id="2ada7-3052">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3052">Bo</span></span>  | <span data-ttu-id="2ada7-3053">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3053">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3054">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3054">__US__</span></span> |    |    |    |    | <span data-ttu-id="2ada7-3055">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3055">Bo</span></span> | <span data-ttu-id="2ada7-3056">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3056">Bo</span></span> | <span data-ttu-id="2ada7-3057">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3057">Bo</span></span> | <span data-ttu-id="2ada7-3058">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3058">Bo</span></span> | <span data-ttu-id="2ada7-3059">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3059">Bo</span></span> | <span data-ttu-id="2ada7-3060">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3060">Bo</span></span> | <span data-ttu-id="2ada7-3061">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3061">Bo</span></span> | <span data-ttu-id="2ada7-3062">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3062">Bo</span></span> | <span data-ttu-id="2ada7-3063">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3063">Err</span></span> | <span data-ttu-id="2ada7-3064">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3064">Err</span></span> | <span data-ttu-id="2ada7-3065">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3065">Bo</span></span>  | <span data-ttu-id="2ada7-3066">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3066">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3067">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3067">__In__</span></span> |    |    |    |    |    | <span data-ttu-id="2ada7-3068">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3068">Bo</span></span> | <span data-ttu-id="2ada7-3069">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3069">Bo</span></span> | <span data-ttu-id="2ada7-3070">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3070">Bo</span></span> | <span data-ttu-id="2ada7-3071">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3071">Bo</span></span> | <span data-ttu-id="2ada7-3072">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3072">Bo</span></span> | <span data-ttu-id="2ada7-3073">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3073">Bo</span></span> | <span data-ttu-id="2ada7-3074">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3074">Bo</span></span> | <span data-ttu-id="2ada7-3075">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3075">Err</span></span> | <span data-ttu-id="2ada7-3076">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3076">Err</span></span> | <span data-ttu-id="2ada7-3077">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3077">Bo</span></span>  | <span data-ttu-id="2ada7-3078">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3078">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3079">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3079">__UI__</span></span> |    |    |    |    |    |    | <span data-ttu-id="2ada7-3080">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3080">Bo</span></span> | <span data-ttu-id="2ada7-3081">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3081">Bo</span></span> | <span data-ttu-id="2ada7-3082">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3082">Bo</span></span> | <span data-ttu-id="2ada7-3083">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3083">Bo</span></span> | <span data-ttu-id="2ada7-3084">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3084">Bo</span></span> | <span data-ttu-id="2ada7-3085">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3085">Bo</span></span> | <span data-ttu-id="2ada7-3086">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3086">Err</span></span> | <span data-ttu-id="2ada7-3087">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3087">Err</span></span> | <span data-ttu-id="2ada7-3088">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3088">Bo</span></span>  | <span data-ttu-id="2ada7-3089">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3089">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3090">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3090">__Lo__</span></span> |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-3091">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3091">Bo</span></span> | <span data-ttu-id="2ada7-3092">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3092">Bo</span></span> | <span data-ttu-id="2ada7-3093">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3093">Bo</span></span> | <span data-ttu-id="2ada7-3094">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3094">Bo</span></span> | <span data-ttu-id="2ada7-3095">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3095">Bo</span></span> | <span data-ttu-id="2ada7-3096">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3096">Err</span></span> | <span data-ttu-id="2ada7-3097">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3097">Err</span></span> | <span data-ttu-id="2ada7-3098">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3098">Bo</span></span>  | <span data-ttu-id="2ada7-3099">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3099">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3100">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3100">__UL__</span></span> |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-3101">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3101">Bo</span></span> | <span data-ttu-id="2ada7-3102">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3102">Bo</span></span> | <span data-ttu-id="2ada7-3103">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3103">Bo</span></span> | <span data-ttu-id="2ada7-3104">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3104">Bo</span></span> | <span data-ttu-id="2ada7-3105">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3105">Err</span></span> | <span data-ttu-id="2ada7-3106">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3106">Err</span></span> | <span data-ttu-id="2ada7-3107">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3107">Bo</span></span>  | <span data-ttu-id="2ada7-3108">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3108">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3109">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3109">__De__</span></span> |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-3110">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3110">Bo</span></span> | <span data-ttu-id="2ada7-3111">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3111">Bo</span></span> | <span data-ttu-id="2ada7-3112">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3112">Bo</span></span> | <span data-ttu-id="2ada7-3113">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3113">Err</span></span> | <span data-ttu-id="2ada7-3114">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3114">Err</span></span> | <span data-ttu-id="2ada7-3115">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3115">Bo</span></span>  | <span data-ttu-id="2ada7-3116">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3116">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3117">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3117">__Si__</span></span> |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-3118">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3118">Bo</span></span> | <span data-ttu-id="2ada7-3119">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3119">Bo</span></span> | <span data-ttu-id="2ada7-3120">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3120">Err</span></span> | <span data-ttu-id="2ada7-3121">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3121">Err</span></span> | <span data-ttu-id="2ada7-3122">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3122">Bo</span></span>  | <span data-ttu-id="2ada7-3123">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3123">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3124">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3124">__Do__</span></span> |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-3125">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3125">Bo</span></span> | <span data-ttu-id="2ada7-3126">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3126">Err</span></span> | <span data-ttu-id="2ada7-3127">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3127">Err</span></span> | <span data-ttu-id="2ada7-3128">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3128">Bo</span></span>  | <span data-ttu-id="2ada7-3129">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3129">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3130">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3130">__Da__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    | <span data-ttu-id="2ada7-3131">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3131">Err</span></span> | <span data-ttu-id="2ada7-3132">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3132">Err</span></span> | <span data-ttu-id="2ada7-3133">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3133">Err</span></span> | <span data-ttu-id="2ada7-3134">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3134">Err</span></span> | 
| <span data-ttu-id="2ada7-3135">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3135">__Ch__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     | <span data-ttu-id="2ada7-3136">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3136">Err</span></span> | <span data-ttu-id="2ada7-3137">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3137">Err</span></span> | <span data-ttu-id="2ada7-3138">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3138">Err</span></span> | 
| <span data-ttu-id="2ada7-3139">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3139">__St__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     | <span data-ttu-id="2ada7-3140">bo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3140">Bo</span></span>  | <span data-ttu-id="2ada7-3141">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3141">Ob</span></span>  | 
| <span data-ttu-id="2ada7-3142">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3142">__Ob__</span></span> |    |    |    |    |    |    |    |    |    |    |    |    |     |     |     | <span data-ttu-id="2ada7-3143">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3143">Ob</span></span>  | 


## <a name="shift-operators"></a><span data-ttu-id="2ada7-3144">移位运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3144">Shift Operators</span></span>

<span data-ttu-id="2ada7-3145">二元运算符`<<`和`>>`执行移位运算。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3145">The binary operators `<<` and `>>` perform bit shifting operations.</span></span>

```antlr
ShiftOperatorExpression
    : Expression '<' '<' LineTerminator? Expression
    | Expression '>' '>' LineTerminator? Expression
    ;
```

<span data-ttu-id="2ada7-3146">有关定义了运算符`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`，`ULong`和`Long`类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3146">The operators are defined for the `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong` and `Long` types.</span></span> <span data-ttu-id="2ada7-3147">不同于其他二进制运算符，就像该运算符是只是左操作数的一元运算符确定移位运算的结果类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3147">Unlike the other binary operators, the result type of a shift operation is determined as if the operator was a unary operator with just the left operand.</span></span> <span data-ttu-id="2ada7-3148">右操作数的类型必须可隐式转换为`Integer`并不用于确定该操作的结果类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3148">The type of the right operand must be implicitly convertible to `Integer` and is not used in determining the result type of the operation.</span></span>

<span data-ttu-id="2ada7-3149">`<<`运算符使位要移动的第一个操作数中剩余的位移量由指定的位数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3149">The `<<` operator causes the bits in the first operand to be shifted left the number of places specified by the shift amount.</span></span> <span data-ttu-id="2ada7-3150">结果类型超出范围的高顺序位将被放弃且低位空出的位用零填充。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3150">The high-order bits outside the range of the result type are discarded and the low-order vacated bit positions are zero-filled.</span></span>

<span data-ttu-id="2ada7-3151">`>>`运算符的原因要移动的第一个操作数中的位右键的位移量由指定的位数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3151">The `>>` operator causes the bits in the first operand to be shifted right the number of places specified by the shift amount.</span></span> <span data-ttu-id="2ada7-3152">低顺序位将被放弃且高序位空出的位设置为零，如果左的操作数为正或到某个如果为负数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3152">The low-order bits are discarded and the high-order vacated bit positions are set to zero if the left operand is positive or to one if negative.</span></span> <span data-ttu-id="2ada7-3153">如果左的操作数的类型`Byte`， `UShort`， `UInteger`，或`ULong`空缺的高顺序位用零填充。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3153">If the left operand is of type `Byte`, `UShort`, `UInteger`, or `ULong` the vacant high-order bits are zero-filled.</span></span>

<span data-ttu-id="2ada7-3154">移位运算符移位底层的表示形式的第一个操作数的第二个操作数的数量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3154">The shift operators shift the bits of the underlying representation of the first operand by the amount of the second operand.</span></span> <span data-ttu-id="2ada7-3155">如果第二个操作数的值大于第一个操作数中的位数或为负，则移位量计算为`RightOperand And SizeMask`其中`SizeMask`是：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3155">If the value of the second operand is greater than the number of bits in the first operand, or is negative, then the shift amount is computed as `RightOperand And SizeMask` where `SizeMask` is:</span></span>

| <span data-ttu-id="2ada7-3156">__LeftOperand 类型__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3156">__LeftOperand Type__</span></span>  | <span data-ttu-id="2ada7-3157">__SizeMask__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3157">__SizeMask__</span></span> | 
|-----------------------|--------------|
| <span data-ttu-id="2ada7-3158">`Byte`， `SByte`</span><span class="sxs-lookup"><span data-stu-id="2ada7-3158">`Byte`, `SByte`</span></span>       | <span data-ttu-id="2ada7-3159">7 (`&H7`)</span><span class="sxs-lookup"><span data-stu-id="2ada7-3159">7 (`&H7`)</span></span>    | 
| <span data-ttu-id="2ada7-3160">`UShort`， `Short`</span><span class="sxs-lookup"><span data-stu-id="2ada7-3160">`UShort`, `Short`</span></span>     | <span data-ttu-id="2ada7-3161">15 (`&HF`)</span><span class="sxs-lookup"><span data-stu-id="2ada7-3161">15 (`&HF`)</span></span>   | 
| <span data-ttu-id="2ada7-3162">`UInteger`， `Integer`</span><span class="sxs-lookup"><span data-stu-id="2ada7-3162">`UInteger`, `Integer`</span></span> | <span data-ttu-id="2ada7-3163">31 (`&H1F`)</span><span class="sxs-lookup"><span data-stu-id="2ada7-3163">31 (`&H1F`)</span></span>  | 
| <span data-ttu-id="2ada7-3164">`ULong`， `Long`</span><span class="sxs-lookup"><span data-stu-id="2ada7-3164">`ULong`, `Long`</span></span>       | <span data-ttu-id="2ada7-3165">63 (`&H3F`)</span><span class="sxs-lookup"><span data-stu-id="2ada7-3165">63 (`&H3F`)</span></span>  | 

<span data-ttu-id="2ada7-3166">移位量为零，如果操作的结果是第一个操作数的值相同。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3166">If the shift amount is zero, the result of the operation is identical to the value of the first operand.</span></span> <span data-ttu-id="2ada7-3167">从这些操作可能不会出现任何溢出。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3167">No overflows are possible from these operations.</span></span>

<span data-ttu-id="2ada7-3168">__操作类型：__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3168">__Operation Type:__</span></span>


| <span data-ttu-id="2ada7-3169">__bo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3169">__Bo__</span></span> | <span data-ttu-id="2ada7-3170">__SB__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3170">__SB__</span></span> | <span data-ttu-id="2ada7-3171">__通过__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3171">__By__</span></span> | <span data-ttu-id="2ada7-3172">__Sh__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3172">__Sh__</span></span> | <span data-ttu-id="2ada7-3173">__我们__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3173">__US__</span></span> | <span data-ttu-id="2ada7-3174">__In__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3174">__In__</span></span> | <span data-ttu-id="2ada7-3175">__UI__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3175">__UI__</span></span> | <span data-ttu-id="2ada7-3176">__Lo__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3176">__Lo__</span></span> | <span data-ttu-id="2ada7-3177">__UL__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3177">__UL__</span></span> | <span data-ttu-id="2ada7-3178">__de__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3178">__De__</span></span> | <span data-ttu-id="2ada7-3179">__si__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3179">__Si__</span></span> | <span data-ttu-id="2ada7-3180">__Do__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3180">__Do__</span></span> | <span data-ttu-id="2ada7-3181">__da__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3181">__Da__</span></span>  | <span data-ttu-id="2ada7-3182">__ch__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3182">__Ch__</span></span>  | <span data-ttu-id="2ada7-3183">__st__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3183">__St__</span></span> | <span data-ttu-id="2ada7-3184">__Ob__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3184">__Ob__</span></span> | 
|----|----|----|----|----|----|----|----|----|----|----|----|-----|-----|----|----|
| <span data-ttu-id="2ada7-3185">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-3185">Sh</span></span> | <span data-ttu-id="2ada7-3186">SB</span><span class="sxs-lookup"><span data-stu-id="2ada7-3186">SB</span></span> | <span data-ttu-id="2ada7-3187">间距</span><span class="sxs-lookup"><span data-stu-id="2ada7-3187">By</span></span> | <span data-ttu-id="2ada7-3188">Sh</span><span class="sxs-lookup"><span data-stu-id="2ada7-3188">Sh</span></span> | <span data-ttu-id="2ada7-3189">US</span><span class="sxs-lookup"><span data-stu-id="2ada7-3189">US</span></span> | <span data-ttu-id="2ada7-3190">内</span><span class="sxs-lookup"><span data-stu-id="2ada7-3190">In</span></span> | <span data-ttu-id="2ada7-3191">UI</span><span class="sxs-lookup"><span data-stu-id="2ada7-3191">UI</span></span> | <span data-ttu-id="2ada7-3192">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3192">Lo</span></span> | <span data-ttu-id="2ada7-3193">UL</span><span class="sxs-lookup"><span data-stu-id="2ada7-3193">UL</span></span> | <span data-ttu-id="2ada7-3194">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3194">Lo</span></span> | <span data-ttu-id="2ada7-3195">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3195">Lo</span></span> | <span data-ttu-id="2ada7-3196">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3196">Lo</span></span> | <span data-ttu-id="2ada7-3197">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3197">Err</span></span> | <span data-ttu-id="2ada7-3198">Err</span><span class="sxs-lookup"><span data-stu-id="2ada7-3198">Err</span></span> | <span data-ttu-id="2ada7-3199">Lo</span><span class="sxs-lookup"><span data-stu-id="2ada7-3199">Lo</span></span> | <span data-ttu-id="2ada7-3200">Ob</span><span class="sxs-lookup"><span data-stu-id="2ada7-3200">Ob</span></span> | 


## <a name="boolean-expressions"></a><span data-ttu-id="2ada7-3201">布尔表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-3201">Boolean Expressions</span></span>

<span data-ttu-id="2ada7-3202">一个布尔表达式是可以进行测试以查看是否为 true，或如果为 false 的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3202">A Boolean expression is an expression that can be tested to see if it is true or if it is false.</span></span>

```antlr
BooleanExpression
    : Expression
    ;
```

<span data-ttu-id="2ada7-3203">一种类型`T`布尔表达式中可以使用 if、 按优先顺序：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3203">A type `T` can be used in a Boolean expression if, in order of preference:</span></span>

* <span data-ttu-id="2ada7-3204">`T` 是`Boolean`或 `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="2ada7-3204">`T` is `Boolean` or `Boolean?`</span></span>

* <span data-ttu-id="2ada7-3205">`T` 扩大转换为 `Boolean`</span><span class="sxs-lookup"><span data-stu-id="2ada7-3205">`T` has a widening conversion to `Boolean`</span></span>

* <span data-ttu-id="2ada7-3206">`T` 扩大转换为 `Boolean?`</span><span class="sxs-lookup"><span data-stu-id="2ada7-3206">`T` has a widening conversion to `Boolean?`</span></span>

* <span data-ttu-id="2ada7-3207">`T` 定义两个伪运算符，`IsTrue`和`IsFalse`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3207">`T` defines two pseudo operators, `IsTrue` and `IsFalse`.</span></span>

* <span data-ttu-id="2ada7-3208">`T` 收缩转换为`Boolean?`，并不涉及从转换`Boolean`到`Boolean?`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3208">`T` has a narrowing conversion to `Boolean?` that does not involve a conversion from `Boolean` to `Boolean?`.</span></span>

* <span data-ttu-id="2ada7-3209">`T` 收缩转换为`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3209">`T` has a narrowing conversion to `Boolean`.</span></span>

<span data-ttu-id="2ada7-3210">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3210">__Note.__</span></span> <span data-ttu-id="2ada7-3211">请注意，如果有趣的是`Option Strict`处于关闭状态，具有到收缩转换的表达式`Boolean`没有编译时错误，但语言仍喜欢将接受`IsTrue`如果它存在的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3211">It is interesting to note that if `Option Strict` is off, an expression that has a narrowing conversion to `Boolean` will be accepted without a compile-time error but the language will still prefer an `IsTrue` operator if it exists.</span></span> <span data-ttu-id="2ada7-3212">这是因为`Option Strict`只会更改什么是语言，将不被接受，并且永远不会更改表达式的实际含义。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3212">This is because `Option Strict` only changes what is and isn't accepted by the language, and never changes the actual meaning of an expression.</span></span> <span data-ttu-id="2ada7-3213">因此，`IsTrue`必须始终首选通过收缩转换，而不考虑`Option Strict`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3213">Thus, `IsTrue` has to always be preferred over a narrowing conversion, regardless of `Option Strict`.</span></span>

<span data-ttu-id="2ada7-3214">例如，下面的类不定义扩大转换为`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3214">For example, the following class does not define a widening conversion to `Boolean`.</span></span> <span data-ttu-id="2ada7-3215">因此，在中使用它`If`语句将导致调用`IsTrue`运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3215">As a result, its use in the `If` statement causes a call to the `IsTrue` operator.</span></span>

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

<span data-ttu-id="2ada7-3216">如果布尔表达式是类型为或转换成`Boolean`或`Boolean?`，则它是 true，如果值为`True`和 false 否则为。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3216">If a Boolean expression is typed as or converted to `Boolean` or `Boolean?`, then it is true if the value is `True` and false otherwise.</span></span>

<span data-ttu-id="2ada7-3217">否则，布尔表达式会调用`IsTrue`运算符，并返回`True`如果运算符返回`True`; 否则为 false (但永远不会调用`IsFalse`运算符)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3217">Otherwise, a Boolean expression calls the `IsTrue` operator and returns `True` if the operator returned `True`; otherwise it is false (but never calls the `IsFalse` operator).</span></span>

<span data-ttu-id="2ada7-3218">在以下示例中，`Integer`收缩转换为`Boolean`，因此 null`Integer?`具有收缩转换`Boolean?`(生成 null `Boolean`) 和`Boolean`（这将引发异常）。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3218">In the following example, `Integer` has a narrowing conversion to `Boolean`, so a null `Integer?` has a narrowing conversion to both `Boolean?` (yielding a null `Boolean`) and to `Boolean` (which throws an exception).</span></span> <span data-ttu-id="2ada7-3219">收缩转换为`Boolean?`是首选方法，因此的值"`i`"为的布尔值表达式是因此`False`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3219">The narrowing conversion to `Boolean?` is preferred, and so the value of "`i`" as a Boolean expression is therefore `False`.</span></span>

```vb
Dim i As Integer? = Nothing
If i Then Console.WriteLine()
```


## <a name="lambda-expressions"></a><span data-ttu-id="2ada7-3220">Lambda 表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-3220">Lambda Expressions</span></span>

<span data-ttu-id="2ada7-3221">一个*lambda 表达式*定义匿名方法称为*lambda 方法*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3221">A *lambda expression* defines an anonymous method called a *lambda method*.</span></span> <span data-ttu-id="2ada7-3222">Lambda 方法轻松地将"行中"方法传递到采用委托类型的其他方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3222">Lambda methods make it easy to pass "in-line" methods to other methods that take delegate types.</span></span>

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

<span data-ttu-id="2ada7-3223">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3223">The example:</span></span>

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

<span data-ttu-id="2ada7-3224">将打印出：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3224">will print out:</span></span>

```
2 4 6 8
```

<span data-ttu-id="2ada7-3225">Lambda 表达式开头的可选修饰符`Async`或`Iterator`后, 跟关键字`Function`或`Sub`和参数列表。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3225">A lambda expression begins with the optional modifiers `Async` or `Iterator`, followed by the keyword `Function` or `Sub` and a parameter list.</span></span> <span data-ttu-id="2ada7-3226">不能声明为 lambda 表达式中的参数`Optional`或`ParamArray`并且不能具有属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3226">Parameters in a lambda expression cannot be declared `Optional` or `ParamArray` and cannot have attributes.</span></span> <span data-ttu-id="2ada7-3227">与正则方法不同省略 lambda 方法的参数类型不会自动推断`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3227">Unlike regular methods, omitting a parameter type for a lambda method does not automatically infer `Object`.</span></span> <span data-ttu-id="2ada7-3228">相反，lambda 方法重新分类，省略的参数类型和`ByRef`修饰符推断出的目标类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3228">Instead, when a lambda method is reclassified, the omitted parameter types and `ByRef` modifiers are inferred from the target type.</span></span> <span data-ttu-id="2ada7-3229">在上一示例中，lambda 表达式编写时本来可以作为`Function(x) x * 2`，它将推断的类型和`x`要`Integer`lambda 方法已用于创建实例`IntFunc`委托类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3229">In the previous example, the lambda expression could have been written as `Function(x) x * 2`, and it would have inferred the type of `x` to be `Integer` when the lambda method was used to create an instance of the `IntFunc` delegate type.</span></span> <span data-ttu-id="2ada7-3230">与不同的本地变量推断，如果 lambda 方法参数省略了一种类型，但具有数组或名称可为 null 的修饰符，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3230">Unlike local variable inference, if a lambda method parameter omits a type but has an array or nullable name modifier, a compile-time error occurs.</span></span>

<span data-ttu-id="2ada7-3231">一个__正则 lambda 表达式__是指既不具有`Async`也不`Iterator`修饰符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3231">A __regular lambda expression__ is one with neither `Async` nor `Iterator` modifiers.</span></span>

<span data-ttu-id="2ada7-3232">__迭代器 lambda 表达式__是一个具有`Iterator`修饰符，但不`Async`修饰符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3232">An __iterator lambda expression__ is one with the `Iterator` modifier and no `Async` modifier.</span></span> <span data-ttu-id="2ada7-3233">它必须是一个函数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3233">It must be a function.</span></span> <span data-ttu-id="2ada7-3234">当它对重新分类为一个值时，它可以仅重新分类为其返回类型是委托类型的值`IEnumerator`，或`IEnumerable`，或`IEnumerator(Of T)`或`IEnumerable(Of T)`某些`T`，并且不具有任何 ByRef 参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3234">When it is reclassified to a value, it can only be reclassified to a value of delegate type whose return type is `IEnumerator`, or `IEnumerable`, or `IEnumerator(Of T)` or `IEnumerable(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="2ada7-3235">__异步 lambda 表达式__是一个具有`Async`修饰符，但不`Iterator`修饰符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3235">An __async lambda expression__ is one with the `Async` modifier and no `Iterator` modifier.</span></span> <span data-ttu-id="2ada7-3236">Async sub lambda 只是为不带任何 ByRef 参数的 sub 委托类型的值重新分类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3236">An async sub lambda may only be reclassified to a value of sub delegate type with no ByRef parameters.</span></span> <span data-ttu-id="2ada7-3237">仅可以为其返回类型是函数委托类型的值重新分类异步函数 lambda`Task`或`Task(Of T)`某些`T`，并且不具有任何 ByRef 参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3237">An async function lambda may only be reclassified to a value of function delegate type whose return type is `Task` or `Task(Of T)` for some `T`, and which has no ByRef parameters.</span></span>

<span data-ttu-id="2ada7-3238">Lambda 表达式可以是单行或多行。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3238">Lambda expressions can either be single-line or multi-line.</span></span> <span data-ttu-id="2ada7-3239">单行`Function`lambda 表达式包含一个表示从 lambda 方法返回的值的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3239">Single-line `Function` lambda expressions contain a single expression that represents the value returned from the lambda method.</span></span> <span data-ttu-id="2ada7-3240">单行`Sub`lambda 表达式包含单个语句无需其关闭`StatementTerminator`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3240">Single-line `Sub` lambda expressions contain a single statement without its closing `StatementTerminator`.</span></span> <span data-ttu-id="2ada7-3241">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3241">For example:</span></span>

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

<span data-ttu-id="2ada7-3242">单行 lambda 紧密比所有其他表达式和语句构造绑定。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3242">Single-line lambda constructs bind less tightly than all other expressions and statements.</span></span> <span data-ttu-id="2ada7-3243">因此，对于示例中，"`Function() x + 5`"是等效于"`Function() (x+5)"`而非"`(Function() x) + 5`"。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3243">Thus, for example, "`Function() x + 5`" is equivalent to "`Function() (x+5)"` rather than "`(Function() x) + 5`".</span></span> <span data-ttu-id="2ada7-3244">为避免混淆的单行`Sub`Dim 语句或标签声明语句不能包含 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3244">To avoid ambiguity, a single-line `Sub` lambda expression may not contain a Dim statement or a label declaration statement.</span></span> <span data-ttu-id="2ada7-3245">此外，除非将它括在括号内的单行`Sub`lambda 表达式可能会不立即后跟一个冒号":"，成员访问运算符"。"，字典成员访问运算符"！"或左括号"(".</span><span class="sxs-lookup"><span data-stu-id="2ada7-3245">Also, unless it is enclosed in parentheses, a single-line `Sub` lambda expression may not be immediately followed by a colon ":", a member access operator ".", a dictionary member access operator "!" or an open parenthesis "(".</span></span> <span data-ttu-id="2ada7-3246">它不能包含任何块语句 (`With`， `SyncLock, If...EndIf`， `While`， `For`， `Do`， `Using`)，也不`OnError`也不`Resume`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3246">It may not contain any block statement (`With`, `SyncLock, If...EndIf`, `While`, `For`, `Do`, `Using`) nor `OnError` nor `Resume`.</span></span>

<span data-ttu-id="2ada7-3247">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3247">__Note.__</span></span> <span data-ttu-id="2ada7-3248">在 lambda 表达式`Function(i) x=i`，正文将被解释为*表达式*(测试是否`x`和`i`相等)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3248">In the lambda expression `Function(i) x=i`, the body is interpreted as an *expression* (which tests whether `x` and `i` are equal).</span></span> <span data-ttu-id="2ada7-3249">Lambda 表达式中，但是`Sub(i) x=i`，正文将被解释为一个语句 (哪个分配`i`到`x`)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3249">But in the lambda expression `Sub(i) x=i`, the body is interpreted as a statement (which assigns `i` to `x`).</span></span>

<span data-ttu-id="2ada7-3250">多行 lambda 表达式包含语句块，相应结尾`End`语句 (即`End Function`或`End Sub`)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3250">A multi-line lambda expression contains a statement block and must end with an appropriate `End` statement (i.e. `End Function` or `End Sub`).</span></span> <span data-ttu-id="2ada7-3251">与正则方法，多行 lambda 方法的一样`Function`或`Sub`语句和`End`语句必须置于其自己的行。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3251">As with regular methods, a multi-line lambda method's `Function` or `Sub` statement and `End` statements must be on their own lines.</span></span> <span data-ttu-id="2ada7-3252">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3252">For example:</span></span>

```vb
' Error: Function statement must be on its own line!
Dim x = Sub(x As Integer) : Console.WriteLine(x) : End Sub

' OK
Dim y = Sub(x As Integer)
               Console.WriteLine(x)
          End Sub
```

<span data-ttu-id="2ada7-3253">多行`Function`lambda 表达式可以声明返回类型，但不能将其上的属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3253">Multi-line `Function` lambda expressions can declare a return type but cannot put attributes on it.</span></span> <span data-ttu-id="2ada7-3254">如果多行`Function`lambda 表达式不声明返回类型，但可以从在其中使用 lambda 表达式，则使用该返回类型的上下文推断返回类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3254">If a multi-line `Function` lambda expression does not declare a return type but the return type can be inferred from the context in which the lambda expression is used , then that return type is used.</span></span> <span data-ttu-id="2ada7-3255">否则该函数的返回类型的计算方式如下：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3255">Otherwise the return type of the function is calculated as follows:</span></span>

* <span data-ttu-id="2ada7-3256">在正则 lambda 表达式中，返回类型是中的表达式中所有的主要类型`Return`语句块中的语句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3256">In a regular lambda expression, the return type is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="2ada7-3257">在异步 lambda 表达式中，返回类型是`Task(Of T)`其中`T`是基准类型中所有的表达式`Return`语句块中的语句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3257">In an async lambda expression, the return type is `Task(Of T)` where `T` is the dominant type of the expressions in all the `Return` statements in the statement block.</span></span>

* <span data-ttu-id="2ada7-3258">在迭代器 lambda 表达式中，返回类型是`IEnumerable(Of T)`其中`T`是基准类型中所有的表达式`Yield`语句块中的语句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3258">In an iterator lambda expression, the return type is `IEnumerable(Of T)` where `T` is the dominant type of the expressions in all the `Yield` statements in the statement block.</span></span>

<span data-ttu-id="2ada7-3259">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3259">For example:</span></span>

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

<span data-ttu-id="2ada7-3260">在所有情况下，如果有任何`Return`(分别`Yield`) 语句，或如果没有基准类型之间，并且正在使用严格的语义，会发生编译时错误; 否则基准类型是隐式`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3260">In all cases, if there are no `Return` (respectively `Yield`) statements, or if there is no dominant type among them, and strict semantics are being used, a compile-time error occurs; otherwise the dominant type is implicitly `Object`.</span></span>

<span data-ttu-id="2ada7-3261">请注意，从所有计算的返回类型`Return`语句，即使它们不是可访问。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3261">Note that the return type is calculated from all `Return` statements, even if they are not reachable.</span></span> <span data-ttu-id="2ada7-3262">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3262">For example:</span></span>

```vb
' Return type is Double
Dim x = Function()
              Return 10
               Return 10.50
          End Function
```

<span data-ttu-id="2ada7-3263">隐式返回变量，因为没有将变量的名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3263">There is no implicit return variable, as there is no name for the variable.</span></span>

<span data-ttu-id="2ada7-3264">在多行 lambda 表达式内的语句块具有以下限制：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3264">The statement blocks inside multi-line lambda expressions have the following restrictions:</span></span>

* <span data-ttu-id="2ada7-3265">`On Error` 并`Resume`语句不允许，尽管`Try`允许使用的语句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3265">`On Error` and `Resume` statements are not allowed, although `Try` statements are allowed.</span></span>

* <span data-ttu-id="2ada7-3266">不能在多行 lambda 表达式中声明静态局部变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3266">Static locals cannot be declared in multi-line lambda expressions.</span></span>

* <span data-ttu-id="2ada7-3267">虽然一般分支规则可以应用在其中不可能进行分支传入或传出的多行 lambda 表达式、 语句块。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3267">It is not possible to branch into or out of the statement block of a multi-line lambda expression, although the normal branching rules apply within it.</span></span> <span data-ttu-id="2ada7-3268">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3268">For example:</span></span>

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

<span data-ttu-id="2ada7-3269">Lambda 表达式是大致相当于匿名方法声明上包含的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3269">A lambda expression is roughly equivalent to an anonymous method declared on the containing type.</span></span> <span data-ttu-id="2ada7-3270">最初的示例中是大致相当于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3270">The initial example is roughly equivalent to:</span></span>

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


### <a name="closures"></a><span data-ttu-id="2ada7-3271">闭包</span><span class="sxs-lookup"><span data-stu-id="2ada7-3271">Closures</span></span>

<span data-ttu-id="2ada7-3272">Lambda 表达式作用域，包括本地变量或参数中包含的方法和 lambda 表达式定义中有权访问的所有变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3272">Lambda expressions have access to all of the variables in scope, including local variables or parameters defined in the containing method and lambda expressions.</span></span> <span data-ttu-id="2ada7-3273">当 lambda 表达式引用的局部变量或参数时，lambda 表达式捕获到闭包所引用的变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3273">When a lambda expression refers to a local variable or parameter, the lambda expression captures the variable being referred to into a closure.</span></span> <span data-ttu-id="2ada7-3274">闭包是位于堆栈上，而不是堆上的一个对象，当捕获变量时，将对该变量的所有引用重都定向到闭包。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3274">A closure is an object that lives on the heap instead of on the stack, and when a variable is captured, all references to the variable are redirected to the closure.</span></span> <span data-ttu-id="2ada7-3275">这使 lambda 表达式，以继续包含方法完成后即使引用局部变量和参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3275">This enables lambda expressions to continue to refer to local variables and parameters even after the containing method is complete.</span></span> <span data-ttu-id="2ada7-3276">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3276">For example:</span></span>

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

<span data-ttu-id="2ada7-3277">是大致相当于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3277">is roughly equivalent to:</span></span>

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

<span data-ttu-id="2ada7-3278">闭包捕获新副本的本地变量进入块中的每次本地声明变量，但如果有的话，使用以前复制的值初始化的新副本。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3278">A closure captures a new copy of a local variable each time it enters the block in which the local variable is declared, but the new copy is initialized with the value of the previous copy, if any.</span></span> <span data-ttu-id="2ada7-3279">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3279">For example:</span></span>

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

<span data-ttu-id="2ada7-3280">将打印</span><span class="sxs-lookup"><span data-stu-id="2ada7-3280">prints</span></span>

```
1 2 3 4 5 6 7 8 9 10
```

<span data-ttu-id="2ada7-3281">而不是</span><span class="sxs-lookup"><span data-stu-id="2ada7-3281">instead of</span></span>

```
9 9 9 9 9 9 9 9 9 9
```

<span data-ttu-id="2ada7-3282">闭包需要输入块时初始化，因为不允许对`GoTo`到具有从该块中，外部的闭包的块虽然允许到`Resume`到闭包中包含的块。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3282">Because closures have to be initialized when entering a block, it is not allowed to `GoTo` into a block with a closure from outside of that block, although it is allowed to `Resume` into a block with a closure.</span></span> <span data-ttu-id="2ada7-3283">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3283">For example:</span></span>

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

<span data-ttu-id="2ada7-3284">因为它们不能捕获到闭包，以下不能出现在 lambda 表达式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3284">Because they cannot be captured into a closure, the following cannot appear inside of a lambda expression:</span></span>

* <span data-ttu-id="2ada7-3285">引用参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3285">Reference parameters.</span></span>

* <span data-ttu-id="2ada7-3286">实例表达式 (`Me`， `MyClass`， `MyBase`)，如果类型的`Me`不是类。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3286">Instance expressions (`Me`, `MyClass`, `MyBase`), if the type of `Me` is not a class.</span></span>

<span data-ttu-id="2ada7-3287">匿名类型创建表达式，如果 lambda 表达式的表达式的一部分的成员。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3287">The members of an anonymous type-creation expression, if the lambda expression is part of the expression.</span></span> <span data-ttu-id="2ada7-3288">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3288">For example:</span></span>

```vb
' Error: Lambda cannot refer to anonymous type field
Dim x = New With { .a = 12, .b = Function() .a }
```

<span data-ttu-id="2ada7-3289">`ReadOnly` 实例构造函数中的实例变量或`ReadOnly`共享共享其中的非值上下文中使用变量的构造函数中的变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3289">`ReadOnly` instance variables in instance constructors or `ReadOnly` shared variables in shared constructors where the variables are used in a non-value context.</span></span> <span data-ttu-id="2ada7-3290">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3290">For example:</span></span>

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

## <a name="query-expressions"></a><span data-ttu-id="2ada7-3291">查询表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-3291">Query Expressions</span></span>

<span data-ttu-id="2ada7-3292">一个*查询表达式*是一个表达式，适用的一系列*查询运算符*于的元素*可查询*集合。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3292">A *query expression* is an expression that applies a series of *query operators* to the elements of a *queryable* collection.</span></span> <span data-ttu-id="2ada7-3293">例如，以下表达式使用的集合`Customer`对象，并返回华盛顿州中的所有客户的名称：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3293">For example, the following expression takes a collection of `Customer` objects and returns the names of all the customers in the state of Washington:</span></span>

```vb
Dim names = _
    From cust In Customers _
    Where cust.State = "WA" _
    Select cust.Name
```

<span data-ttu-id="2ada7-3294">查询表达式必须开头`From`或`Aggregate`运算符可以开头和结尾的任何查询运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3294">A query expression must start with a `From` or an `Aggregate` operator and can end with any query operator.</span></span> <span data-ttu-id="2ada7-3295">查询表达式的结果分类为一个值;该表达式的结果类型取决于表达式中的最后一个查询运算符的结果类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3295">The result of a query expression is classified as a value; the result type of the expression depends on the result type of the last query operator in the expression.</span></span>

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

### <a name="range-variables"></a><span data-ttu-id="2ada7-3296">范围变量</span><span class="sxs-lookup"><span data-stu-id="2ada7-3296">Range Variables</span></span>

<span data-ttu-id="2ada7-3297">一些查询运算符引入了一种特殊的变量称为*范围变量*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3297">Some query operators introduce a special kind of variable called a *range variable*.</span></span> <span data-ttu-id="2ada7-3298">范围变量不是实际的变量;相反，它们代表各个值在查询的计算期间针对输入集合。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3298">Range variables are not real variables; instead, they represent the individual values during the evaluation of the query over the input collections.</span></span>

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

<span data-ttu-id="2ada7-3299">范围变量的作用域中引入的查询运算符到查询表达式的末尾或查询运算符如`Select`，以隐藏它们。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3299">Range variables are scoped from the introducing query operator to the end of a query expression, or to a query operator such as `Select` that hides them.</span></span> <span data-ttu-id="2ada7-3300">例如，在下面的查询</span><span class="sxs-lookup"><span data-stu-id="2ada7-3300">For example, in the following query</span></span>

```vb
Dim waCusts = _
    From cust As Customer In Customers _
    Where cust.State = "WA"
```

<span data-ttu-id="2ada7-3301">`From`查询运算符引入的范围变量`cust`化为`Customer`，表示在每个客户`Customers`集合。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3301">the `From` query operator introduces a range variable `cust` typed as `Customer` that represents each customer in the `Customers` collection.</span></span> <span data-ttu-id="2ada7-3302">以下`Where`查询运算符则引用范围变量`cust`以确定是否筛选出结果的集合与单个客户的筛选器表达式中。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3302">The following `Where` query operator then refers to the range variable `cust` in the filter expression to determine whether to filter an individual customer out of the resulting collection.</span></span>

<span data-ttu-id="2ada7-3303">有两种类型的范围变量：*集合的范围变量*并*表达式范围变量*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3303">There are two types of range variables: *collection range variables* and *expression range variables*.</span></span> <span data-ttu-id="2ada7-3304">集合的范围变量需要它们正在查询的集合的元素的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3304">Collection range variables take their values from the elements of the collections being queried.</span></span> <span data-ttu-id="2ada7-3305">在集合范围变量声明中的集合表达式必须分类为值的类型可查询。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3305">The collection expression in a collection range variable declaration must be classified as a value whose type is queryable.</span></span> <span data-ttu-id="2ada7-3306">如果省略集合范围变量的类型，则它被推断为集合表达式的元素类型或`Object`如果集合表达式不具有元素类型 (也就是说，只能定义`Cast`方法)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3306">If the type of a collection range variable is omitted, it is inferred to be the element type of the collection expression, or `Object` if the collection expression does not have an element type (i.e. only defines a `Cast` method).</span></span> <span data-ttu-id="2ada7-3307">如果集合表达式不可查询 （即集合的元素类型无法推断），编译时错误结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3307">If the collection expression is not queryable (i.e. the element type of the collection cannot be inferred), a compile-time error results.</span></span>

<span data-ttu-id="2ada7-3308">表达式范围变量是其值计算表达式而不是集合的范围变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3308">An expression range variable is a range variable whose value is calculated by an expression rather than a collection.</span></span> <span data-ttu-id="2ada7-3309">在以下示例中，`Select`运算符引入了一个名为的表达式范围变量的查询`cityState`计算从两个字段：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3309">In the following example, the `Select` query operator introduces an expression range variable named `cityState` calculated from two fields:</span></span>

```vb
Dim cityStates = _
    From cust As Customer In Customers _
    Select cityState = cust.City & "," & cust.State _
    Where cityState.Length() < 10
```

<span data-ttu-id="2ada7-3310">表达式范围变量不需要引用另一个范围变量，尽管此类变量可能是可疑的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3310">An expression range variable is not required to reference another range variable, although such a variable may be of dubious value.</span></span> <span data-ttu-id="2ada7-3311">分配给表达式的范围变量的表达式必须分类为一个值，并且必须隐式转换为的类型的范围变量，如果给定。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3311">The expression assigned to an expression range variable must be classified as a value and must be implicitly convertible to the type of the range variable, if given.</span></span>

<span data-ttu-id="2ada7-3312">仅在 Let 运算符中表达式的范围变量可能指定其类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3312">Only in a Let operator may an expression range variable have its type specified.</span></span> <span data-ttu-id="2ada7-3313">在其他运算符中，或如果未指定其类型，则使用本地变量的类型推理来确定范围变量的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3313">In other operators, or if its type is not specified, then local variable type inference is used to determine the type of the range variable.</span></span>

<span data-ttu-id="2ada7-3314">范围变量必须遵循声明在到隐藏的方面的本地变量的规则。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3314">A range variable must follow the rules for declaring local variables in respect to shadowing.</span></span> <span data-ttu-id="2ada7-3315">因此，范围变量不能隐藏本地变量或参数的封闭方法或另一个范围变量中的名称 （除非在查询运算符专门隐藏所有作用域中的当前范围变量）。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3315">Thus, a range variable cannot hide the name of a local variable or parameter in the enclosing method or another range variable (unless the query operator specifically hides all current range variables in scope).</span></span>


### <a name="queryable-types"></a><span data-ttu-id="2ada7-3316">可查询类型</span><span class="sxs-lookup"><span data-stu-id="2ada7-3316">Queryable Types</span></span>

<span data-ttu-id="2ada7-3317">查询表达式是通过将转换为对集合类型的已知方法的调用的表达式实现的。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3317">Query expressions are implemented by translating the expression into calls to well-known methods on a collection type.</span></span> <span data-ttu-id="2ada7-3318">这些定义完善的方法定义的可查询集合的元素类型以及集合上执行的查询运算符的结果类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3318">These well-defined methods define the element type of the queryable collection as well as the result types of query operators executed on the collection.</span></span> <span data-ttu-id="2ada7-3319">每个查询运算符指定方法的查询运算符通常会转换到，尽管具体的转换是依赖于实现。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3319">Each query operator specifies the method or methods that the query operator is generally translated into, although the specific translation is implementation dependent.</span></span> <span data-ttu-id="2ada7-3320">使用如下所示的常规格式规范中给出了这些方法：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3320">The methods are given in the specification using a general format that looks like:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="2ada7-3321">对方法执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3321">The following applies to the methods:</span></span>

* <span data-ttu-id="2ada7-3322">方法必须是实例或集合类型的扩展成员，并且必须可访问性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3322">The method must be an instance or extension member of the collection type and must be accessible.</span></span>

* <span data-ttu-id="2ada7-3323">该方法可以是泛型类型，提供可能推断出所有类型参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3323">The method may be generic, provided that is possible to infer all type arguments.</span></span>

* <span data-ttu-id="2ada7-3324">可以重载方法，在这种情况下重载决策用于确定完全要使用的方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3324">The method may be overloaded, in which case overload resolution is used to determine the exactly method to use.</span></span>

* <span data-ttu-id="2ada7-3325">另一种委托类型可能会代替委托`Func`类型，前提是它具有相同的签名，包括返回类型，为匹配`Func`类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3325">Another delegate type may be used in place of the delegate `Func` type, provided that it has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="2ada7-3326">类型`System.Linq.Expressions.Expression(Of D)`可能会用它来替换该委托`Func`提供的类型`D`是委托类型具有相同的签名，包括返回类型，为匹配`Func`类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3326">The type `System.Linq.Expressions.Expression(Of D)` may be used in place of the delegate `Func` type, provided that `D` is a delegate type that has the same signature, including return type, as the matching `Func` type.</span></span>

* <span data-ttu-id="2ada7-3327">类型`T`表示输入集合的元素类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3327">The type `T` represents the element type of the input collection.</span></span> <span data-ttu-id="2ada7-3328">所有定义按集合类型的方法必须具有相同的输入的元素类型是可查询的集合类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3328">All of the methods defined by a collection type must have the same input element type for the collection type to be queryable.</span></span>

* <span data-ttu-id="2ada7-3329">类型`S`表示在执行联接的查询运算符的情况下的第二个输入集合的元素类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3329">The type `S` represents the element type of the second input collection in the case of query operators that perform joins.</span></span>

* <span data-ttu-id="2ada7-3330">类型`K`表示在具有一组充当密钥的范围变量的查询运算符的情况下密钥类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3330">The type `K` represents a key type in the case of query operators that have a set of range variables that act as keys.</span></span>

* <span data-ttu-id="2ada7-3331">类型`N`表示 （尽管仍可能是用户定义类型，不是内部函数的数值类型） 用作数值类型的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3331">The type `N` represents a type that is used as a numeric type (although it could still be a user-defined type and not an intrinsic numeric type).</span></span>

* <span data-ttu-id="2ada7-3332">类型`B`表示可以使用布尔表达式中的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3332">The type `B` represents a type that can be used in a Boolean expression.</span></span>

* <span data-ttu-id="2ada7-3333">类型`R`表示结果集合的元素类型，如果查询运算符生成的结果集合。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3333">The type `R` represents the element type of the result collection, if the query operator produces a result collection.</span></span> <span data-ttu-id="2ada7-3334">`R` 取决于在结束时的查询运算符的作用域中的范围变量的数量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3334">`R` depends on the number of range variables in scope at the conclusion of the query operator.</span></span> <span data-ttu-id="2ada7-3335">如果一个范围变量是在范围内，然后`R`是该范围变量的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3335">If a single range variable is in scope, then `R` is the type of that range variable.</span></span> <span data-ttu-id="2ada7-3336">在示例</span><span class="sxs-lookup"><span data-stu-id="2ada7-3336">In the example</span></span>

  ```vb
  Dim custNames = From c In Customers
                  Select c.Name
  ```

  <span data-ttu-id="2ada7-3337">查询的结果将是具有的元素类型的集合类型`String`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3337">the result of the query will be a collection type with an element type of `String`.</span></span> <span data-ttu-id="2ada7-3338">如果多个范围变量，则在范围内，然后`R`是包含所有与作用域中的范围变量的匿名类型`Key`字段。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3338">If multiple range variables are in scope, then `R` is an anonymous type that contains all of the range variables in scope as `Key` fields.</span></span> <span data-ttu-id="2ada7-3339">在下面的示例中：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3339">In the example:</span></span>

  ```vb
  Dim custNames = From c In Customers, o In c.Orders 
                  Select Name = c.Name, ProductName = o.ProductName
  ```

  <span data-ttu-id="2ada7-3340">查询的结果将是具有一个名为只读属性的匿名类型的元素类型的集合类型`Name`类型的`String`和名为的只读属性`ProductName`类型的`String`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3340">the result of the query will be a collection type with an element type of an anonymous type with a read-only property named `Name` of type `String` and a read-only property named `ProductName` of type `String`.</span></span>

  <span data-ttu-id="2ada7-3341">在查询表达式中，生成的包含范围变量的匿名类型是*透明*，这意味着，范围变量都无需限定即可。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3341">Within a query expression, anonymous types generated to contain range variables are *transparent*, which means that range variables are always available without qualification.</span></span> <span data-ttu-id="2ada7-3342">例如，在前面的示例的范围变量`c`并`o`无法访问而无需限定在`Select`查询运算符，即使输入的集合的元素类型是匿名类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3342">For example, in the previous example the range variables `c` and `o` could be accessed without qualification in the `Select` query operator, even though the input collection's element type was an anonymous type.</span></span>

* <span data-ttu-id="2ada7-3343">类型`CX`表示集合类型，不一定是输入的集合类型，其元素类型是某些类型`X`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3343">The type `CX` represents a collection type, not necessarily the input collection type, whose element type is some type `X`.</span></span>

<span data-ttu-id="2ada7-3344">可查询集合类型必须满足以下条件之一，按优先顺序：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3344">A queryable collection type must satisfy one of the following conditions, in order of preference:</span></span>

* <span data-ttu-id="2ada7-3345">它必须定义符合`Select`方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3345">It must define a conforming `Select` method.</span></span>

* <span data-ttu-id="2ada7-3346">它必须已具有以下方法之一</span><span class="sxs-lookup"><span data-stu-id="2ada7-3346">It must have have one of the following methods</span></span>

  ```vb
  Function AsEnumerable() As CT
  Function AsQueryable() As CT
  ```

  <span data-ttu-id="2ada7-3347">可以调用以获取可查询的集合。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3347">which can be called to obtain a queryable collection.</span></span> <span data-ttu-id="2ada7-3348">如果提供了这两种方法，`AsQueryable`最好通过`AsEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3348">If both methods are provided, `AsQueryable` is preferred over `AsEnumerable`.</span></span>

* <span data-ttu-id="2ada7-3349">它必须具有一种方法</span><span class="sxs-lookup"><span data-stu-id="2ada7-3349">It must have a method</span></span>

  ```vb
  Function Cast(Of T)() As CT
  ```

  <span data-ttu-id="2ada7-3350">可以调用与要生成可查询的集合的范围变量的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3350">which can be called with the type of the range variable to produce a queryable collection.</span></span>

<span data-ttu-id="2ada7-3351">确定集合的元素类型会独立于实际方法调用，因为无法确定特定的方法的适用性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3351">Because determining the element type of a collection occurs independently of an actual method invocation, the applicability of specific methods cannot be determined.</span></span> <span data-ttu-id="2ada7-3352">因此，在确定集合的元素类型时如果有匹配的已知方法的实例方法，则会忽略任何匹配的已知方法的扩展方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3352">Thus, when determining the element type of a collection if there are instance methods that match well-known methods, then any extension methods that match well-known methods are ignored.</span></span>

<span data-ttu-id="2ada7-3353">查询运算符转换其中的查询运算符在表达式中出现的顺序发生。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3353">Query operator translation occurs in the order in which the query operators occur in the expression.</span></span> <span data-ttu-id="2ada7-3354">不需要的集合对象，若要实现所有所需的所有查询运算符方法，尽管每个集合对象至少必须支持`Select`查询运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3354">It is not necessary for a collection object to implement all of the methods needed by all the query operators, although every collection object must at least support the `Select` query operator.</span></span> <span data-ttu-id="2ada7-3355">如果所需的方法不存在，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3355">If a needed method is not present, a compile-time error occurs.</span></span> <span data-ttu-id="2ada7-3356">绑定时的已知方法名称，非方法将忽略在接口和绑定，扩展方法中的多个继承，尽管隐藏的语义仍适用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3356">When binding well-known method names, non-methods are ignored for the purpose of multiple inheritance in interfaces and extension method binding, although shadowing semantics still apply.</span></span> <span data-ttu-id="2ada7-3357">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3357">For example:</span></span>

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

### <a name="default-query-indexer"></a><span data-ttu-id="2ada7-3358">默认查询索引器</span><span class="sxs-lookup"><span data-stu-id="2ada7-3358">Default Query Indexer</span></span>

<span data-ttu-id="2ada7-3359">每个可查询集合类型，其元素类型为`T`尚不包含默认值和属性都被认为具有如下常规形式的默认属性：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3359">Every queryable collection type whose element type is `T` and does not already have a default property is considered to have a default property of the following general form:</span></span>

```vb
Public ReadOnly Default Property Item(index As Integer) As T
    Get
        Return Me.ElementAtOrDefault(index)
    End Get
End Property
```

<span data-ttu-id="2ada7-3360">默认属性只被指使用默认属性访问语法;默认属性不能按名称引用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3360">The default property can only be referred to using the default property access syntax; the default property cannot be referred to by name.</span></span> <span data-ttu-id="2ada7-3361">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3361">For example:</span></span>

```vb
Dim customers As IEnumerable(Of Customer) = ...
Dim customerThree = customers(2)

' Error, no such property
Dim customerFour = customers.Item(4)
```

<span data-ttu-id="2ada7-3362">如果集合类型不具有`ElementAtOrDefault`成员，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3362">If the collection type does not have an `ElementAtOrDefault` member, a compile-time error will occur.</span></span>

### <a name="from-query-operator"></a><span data-ttu-id="2ada7-3363">从查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3363">From Query Operator</span></span>

<span data-ttu-id="2ada7-3364">`From`运算符引入了一个集合范围变量，表示集合的单个成员要查询的查询。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3364">The `From` query operator introduces a collection range variable that represents the individual members of a collection to be queried.</span></span>

```antlr
FromQueryOperator
    : LineTerminator? 'From' LineTerminator? CollectionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="2ada7-3365">例如，查询表达式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3365">For example, the query expression:</span></span>

```vb
From c As Customer In Customers ...
```

<span data-ttu-id="2ada7-3366">可被视为等效于</span><span class="sxs-lookup"><span data-stu-id="2ada7-3366">can be thought of as equivalent to</span></span>

```vb
For Each c As Customer In Customers
        ...
Next c
```

<span data-ttu-id="2ada7-3367">当`From`查询运算符声明范围变量的多个集合，或者不是第一个`From`在查询表达式中的查询运算符，每个新的集合范围变量是*交叉联接*到一组现有的范围变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3367">When a `From` query operator declares multiple collection range variables or is not the first `From` query operator in the query expression, each new collection range variable is *cross joined* to the existing set of range variables.</span></span> <span data-ttu-id="2ada7-3368">结果是通过联接集合中的所有元素的叉积计算查询。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3368">The result is that the query is evaluated over the cross-product of all the elements in the joined collections.</span></span> <span data-ttu-id="2ada7-3369">例如，表达式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3369">For example, the expression:</span></span>

```vb
From c In Customers _
From e In Employees _
...
```

<span data-ttu-id="2ada7-3370">可被视为等效于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3370">can be thought of as equivalent to:</span></span>

```vb
For Each c In Customers
    For Each e In Employees
            ...
    Next e
Next c
```

<span data-ttu-id="2ada7-3371">完全等效于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3371">and is exactly equivalent to:</span></span>

```vb
From c In Customers, e In Employees ...
```

<span data-ttu-id="2ada7-3372">可以在更高版本中使用在先前的查询运算符中引入的范围变量`From`查询运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3372">The range variables introduced in previous query operators can be used in a later `From` query operator.</span></span> <span data-ttu-id="2ada7-3373">例如，在下面的查询表达式中第二个`From`查询运算符是指第一个范围变量的值：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3373">For example, in the following query expression the second `From` query operator refers to the value of the first range variable:</span></span>

```vb
From c As Customer In Customers _
From o As Order In c.Orders _
Select c.Name, o
```

<span data-ttu-id="2ada7-3374">中的多个范围变量`From`查询运算符或多个`From`仅支持查询运算符，如果集合类型包含一个或两个以下方法：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3374">Multiple range variables in a `From` query operator or multiple `From` query operators are only supported if the collection type contains one or both of the following methods:</span></span>

```vb
Function SelectMany(selector As Func(Of T, CR)) As CR
Function SelectMany(selector As Func(Of T, CS), _
                          resultsSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="2ada7-3375">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3375">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs, y In ys ...
```

<span data-ttu-id="2ada7-3376">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3376">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = _
    xs.SelectMany( _
        Function(x As Integer) ys, _
        Function(x As Integer, y As Integer) New With {x, y})...
```

<span data-ttu-id="2ada7-3377">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3377">__Note.__</span></span> <span data-ttu-id="2ada7-3378">`From` 不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3378">`From` is not a reserved word.</span></span>


### <a name="join-query-operator"></a><span data-ttu-id="2ada7-3379">Join 查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3379">Join Query Operator</span></span>

<span data-ttu-id="2ada7-3380">`Join`运算符联接现有范围变量使用新集合的范围变量，并生成其元素被连接在一起的单个集合的查询基于相等表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3380">The `Join` query operator joins existing range variables with a new collection range variable, producing a single collection whose elements have been joined together based on an equality expression.</span></span>

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

<span data-ttu-id="2ada7-3381">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3381">For example:</span></span>

```vb
Dim customersAndOrders = _
    From cust In Customers _
    Join ord In Orders On cust.ID Equals ord.CustomerID
```

<span data-ttu-id="2ada7-3382">相等表达式是比常规相等表达式更受限制：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3382">The equality expression is more restricted than a regular equality expression:</span></span>

* <span data-ttu-id="2ada7-3383">这两个表达式必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3383">Both expressions must be classified as a value.</span></span>

* <span data-ttu-id="2ada7-3384">这两个表达式必须引用至少一个范围变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3384">Both expressions must reference at least one range variable.</span></span>

* <span data-ttu-id="2ada7-3385">必须由一个表达式，引用查询运算符和表达式不能引用任何其他范围变量，在联接中声明范围变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3385">The range variable declared in the join query operator must be referenced by one of the expressions, and that expression must not reference any other range variables.</span></span>

<span data-ttu-id="2ada7-3386">如果两个表达式的类型不完全相同的类型，然后</span><span class="sxs-lookup"><span data-stu-id="2ada7-3386">If the types of the two expressions are not the exact same type, then</span></span>

* <span data-ttu-id="2ada7-3387">如果为两种类型定义相等运算符，这两个表达式都可以隐式转换为它，并且它不是`Object`，然后将这两个表达式转换为该类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3387">If the equality operator is defined for the two types, both expressions are implicitly convertible to it, and it is not `Object`, then convert both expressions to that type.</span></span>

* <span data-ttu-id="2ada7-3388">否则，如果两个表达式可以隐式转换为基准类型，然后将转换两个表达式为该类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3388">Otherwise, if there is a dominant type that both expressions can be implicitly converted to, then convert both expressions to that type.</span></span>

* <span data-ttu-id="2ada7-3389">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3389">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="2ada7-3390">使用哈希值进行比较的表达式 (即通过调用`GetHashCode()`) 而不是使用相等运算符以提高效率。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3390">The expressions are compared using hash values (i.e. by calling `GetHashCode()`) rather than by using equality operators for efficiency.</span></span> <span data-ttu-id="2ada7-3391">一个`Join`查询运算符可能会执行多个联接或中相同的运算符的相等性条件。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3391">A `Join` query operator may do multiple joins or equality conditions in the same operator.</span></span> <span data-ttu-id="2ada7-3392">一个`Join`如果集合类型包含的方法，则仅支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3392">A `Join` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Join(inner As CS, _
                  outerSelector As Func(Of T, K), _
                  innerSelector As Func(Of S, K), _
                  resultSelector As Func(Of T, S, R)) As CR
```

<span data-ttu-id="2ada7-3393">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3393">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Join y In ys On x Equals y _
            ...
```

<span data-ttu-id="2ada7-3394">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3394">is generally translated to</span></span>

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

<span data-ttu-id="2ada7-3395">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3395">__Note.__</span></span> <span data-ttu-id="2ada7-3396">`Join``On`和`Equals`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3396">`Join`, `On` and `Equals` are not reserved words.</span></span>


### <a name="let-query-operator"></a><span data-ttu-id="2ada7-3397">让查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3397">Let Query Operator</span></span>

<span data-ttu-id="2ada7-3398">`Let`查询运算符引入的表达式范围变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3398">The `Let` query operator introduces an expression range variable.</span></span> <span data-ttu-id="2ada7-3399">这样，将在更高版本的查询运算符中使用多个时间后计算中间值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3399">This allows calculating an intermediate value once that will be used multiple times in later query operators.</span></span>

```antlr
LetQueryOperator
    : LineTerminator? 'Let' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="2ada7-3400">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3400">For example:</span></span>

```vb
Dim taxedPrices = _
    From o In Orders _
    Let tax = o.Price * 0.088 _
    Where tax > 3.50 _
    Select o.Price, tax, total = o.Price + tax
```

<span data-ttu-id="2ada7-3401">可被视为等效于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3401">can be thought of as equivalent to:</span></span>

```vb
For Each o In Orders
    Dim tax = o.Price * 0.088
    ...
Next o
```

<span data-ttu-id="2ada7-3402">一个`Let`如果集合类型包含的方法，则仅支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3402">A `Let` query operator is only supported if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="2ada7-3403">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3403">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Let y = x * 10 _
            ...
```

<span data-ttu-id="2ada7-3404">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3404">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="select-query-operator"></a><span data-ttu-id="2ada7-3405">选择查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3405">Select Query Operator</span></span>

<span data-ttu-id="2ada7-3406">`Select`查询运算符就像`Let`查询运算符，因为它引入了表达式的范围变量; 但是，`Select`查询运算符会隐藏当前可用的范围变量而不是将添加到它们。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3406">The `Select` query operator is like the `Let` query operator in that it introduces expression range variables; however, a `Select` query operator hides the currently available range variables instead of adding to them.</span></span> <span data-ttu-id="2ada7-3407">此外，通过引入了表达式范围变量的类型`Select`始终使用本地变量的类型推断规则推断查询运算符; 不能指定显式类型，并且如果没有类型可以推断出，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3407">Also, the type of an expression range variable introduced by a `Select` query operator is always inferred using local variable type inference rules; an explicit type cannot be specified, and if no type can be inferred, a compile-time error occurs.</span></span>

```antlr
SelectQueryOperator
    : LineTerminator? 'Select' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="2ada7-3408">例如，在查询中：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3408">For example, in the query:</span></span>

```vb
Dim smiths = _
    From cust In Customers _
    Select name = cust.name _
    Where name.EndsWith("Smith")
```

<span data-ttu-id="2ada7-3409">`Where`查询运算符仅有权访问`name`引入的范围变量`Select`运算符; 如果`Where`运算符尝试引用`cust`，会出现编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3409">the `Where` query operator only has access to the `name` range variable introduced by the `Select` operator; if the `Where` operator had tried to reference `cust`, a compile-time error would have occurred.</span></span>

<span data-ttu-id="2ada7-3410">而不是显式指定范围变量的名称`Select`查询运算符可以推断范围变量的名称作为匿名类型对象创建表达式中使用相同的规则。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3410">Instead of explicitly specifying the names of the range variables, a `Select` query operator can infer the names of the range variables, using the same rules as anonymous type object creation expressions.</span></span> <span data-ttu-id="2ada7-3411">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3411">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.name, ord.ProductName _
        Where name.EndsWith("Smith")
```

<span data-ttu-id="2ada7-3412">如果未提供范围变量的名称，不能推断名称，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3412">If the name of the range variable is not supplied and a name cannot be inferred, a compile-time error occurs.</span></span> <span data-ttu-id="2ada7-3413">如果`Select`查询运算符包含单个表达式，如果无法推断该范围变量的名称，但没有名称，范围变量将会发生任何错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3413">If the `Select` query operator contains only a single expression, no error occurs if a name for that range variable cannot be inferred but the range variable is nameless.</span></span>  <span data-ttu-id="2ada7-3414">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3414">For example:</span></span>

```vb
Dim custAndOrderNames = _
      From cust In Customers, ord In cust.Orders _
      Select cust.Name & " bought " & ord.ProductName _
        Take 10
```

<span data-ttu-id="2ada7-3415">如果在多义性`Select`之间将名称分配给范围变量和表达式是否相等的查询运算符，该名称分配是首选。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3415">If there is an ambiguity in a `Select` query operator between assigning a name to a range variable and an equality expression, the name assignment is preferred.</span></span> <span data-ttu-id="2ada7-3416">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3416">For example:</span></span>

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

<span data-ttu-id="2ada7-3417">在每个表达式`Select`查询运算符必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3417">Each expression in the `Select` query operator must be classified as a value.</span></span> <span data-ttu-id="2ada7-3418">一个`Select`集合类型包含一种方法时才支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3418">A `Select` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Select(selector As Func(Of T, R)) As CR
```

<span data-ttu-id="2ada7-3419">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3419">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Select x, y = x * 10 _
            ...
```

<span data-ttu-id="2ada7-3420">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3420">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Select(Function(x As Integer) New With {x, .y = x * 10})...
```


### <a name="distinct-query-operator"></a><span data-ttu-id="2ada7-3421">Distinct 查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3421">Distinct Query Operator</span></span>

<span data-ttu-id="2ada7-3422">`Distinct`查询运算符限制仅对具有非重复值，确定比较是否相等的元素类型的集合中的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3422">The `Distinct` query operator restricts the values in a collection only to those with distinct values, as determined by comparing the element type for equality.</span></span>

```antlr
DistinctQueryOperator
    : LineTerminator? 'Distinct' LineTerminator?
    ;
```

<span data-ttu-id="2ada7-3423">例如，以下查询：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3423">For example, the query:</span></span>

```vb
Dim distinctCustomerPrice = _
    From cust In Customers, ord In cust.Orders _
    Select cust.Name, ord.Price _
    Distinct
```

<span data-ttu-id="2ada7-3424">只会返回一个配对的客户名称和订单价格，每个非重复行，即使客户所具有的多个订单的价格相同。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3424">will only return one row for each distinct pairing of customer name and order price, even if the customer has multiple orders with the same price.</span></span> <span data-ttu-id="2ada7-3425">一个`Distinct`集合类型包含一种方法时才支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3425">A `Distinct` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Distinct() As CT
```

<span data-ttu-id="2ada7-3426">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3426">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Distinct _
            ...
```

<span data-ttu-id="2ada7-3427">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3427">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = xs.Distinct()...
```

<span data-ttu-id="2ada7-3428">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3428">__Note.__</span></span> <span data-ttu-id="2ada7-3429">`Distinct` 不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3429">`Distinct` is not a reserved word.</span></span>


### <a name="where-query-operator"></a><span data-ttu-id="2ada7-3430">Where 查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3430">Where Query Operator</span></span>

<span data-ttu-id="2ada7-3431">`Where`查询运算符限制为满足给定的条件的集合中的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3431">The `Where` query operator restricts the values in a collection to those that satisfy a given condition.</span></span>

```antlr
WhereQueryOperator
    : LineTerminator? 'Where' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="2ada7-3432">一个`Where`查询运算符采用布尔表达式，计算每个集的范围变量的值; 如果表达式的值为 true，则输出集合中的值显示，否则这些值将跳过。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3432">A `Where` query operator takes a Boolean expression that is evaluated for each set of range variable values; if the value of the expression is true, then the values appear in the output collection, otherwise the values are skipped.</span></span> <span data-ttu-id="2ada7-3433">例如，查询表达式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3433">For example, the query expression:</span></span>

```vb
From cust In Customers, ord In Orders _
Where cust.ID = ord.CustomerID _
...
```

<span data-ttu-id="2ada7-3434">可以认为的视为等效于嵌套循环</span><span class="sxs-lookup"><span data-stu-id="2ada7-3434">can be thought of as equivalent to the nested loop</span></span>

```vb
For Each cust In Customers
    For Each ord In Orders
            If cust.ID = ord.CustomerID Then
                ...
            End If
    Next ord
Next cust
```

<span data-ttu-id="2ada7-3435">一个`Where`集合类型包含一种方法时才支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3435">A `Where` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Where(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="2ada7-3436">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3436">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Where x < 10 _
            ...
```

<span data-ttu-id="2ada7-3437">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3437">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x As Integer) x < 10)...
```

<span data-ttu-id="2ada7-3438">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3438">__Note.__</span></span> <span data-ttu-id="2ada7-3439">`Where` 不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3439">`Where` is not a reserved word.</span></span>


### <a name="partition-query-operators"></a><span data-ttu-id="2ada7-3440">分区查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3440">Partition Query Operators</span></span>

```antlr
PartitionQueryOperator
    : LineTerminator? 'Take' LineTerminator? Expression
    | LineTerminator? 'Take' 'While' LineTerminator? BooleanExpression
    | LineTerminator? 'Skip' LineTerminator? Expression
    | LineTerminator? 'Skip' 'While' LineTerminator? BooleanExpression
    ;
```

<span data-ttu-id="2ada7-3441">`Take`查询运算符中第一个结果`n`集合中的元素。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3441">The `Take` query operator results in the first `n` elements of a collection.</span></span> <span data-ttu-id="2ada7-3442">与一起使用时`While`修饰符，`Take`运算符在第一个结果`n`满足布尔表达式的集合的元素。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3442">When used with the `While` modifier, the `Take` operator results in the first `n` elements of a collection that satisfy a Boolean expression.</span></span> <span data-ttu-id="2ada7-3443">`Skip`运算符将跳过第一个`n`集合中的元素，然后返回集合的其余部分。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3443">The `Skip` operator skips the first `n` elements of a collection and then returns the remainder of the collection.</span></span>  <span data-ttu-id="2ada7-3444">当结合使用时`While`修饰符，`Skip`运算符将跳过第一个`n`满足的布尔表达式，然后返回集合的其余部分的元素的集合。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3444">When used in conjunction with the `While` modifier, the `Skip` operator skips the first `n` elements of a collection that satisfy a Boolean expression and then returns the rest of the collection.</span></span> <span data-ttu-id="2ada7-3445">中的表达式`Take`或`Skip`查询运算符必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3445">The expressions in a `Take` or `Skip` query operator must be classified as a value.</span></span>

<span data-ttu-id="2ada7-3446">一个`Take`集合类型包含一种方法时才支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3446">A `Take` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Take(count As N) As CT
```

<span data-ttu-id="2ada7-3447">一个`Skip`集合类型包含一种方法时才支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3447">A `Skip` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function Skip(count As N) As CT
```

<span data-ttu-id="2ada7-3448">一个`Take While`集合类型包含一种方法时才支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3448">A `Take While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function TakeWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="2ada7-3449">一个`Skip While`集合类型包含一种方法时才支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3449">A `Skip While` query operator is supported only if the collection type contains a method:</span></span>

```vb
Function SkipWhile(predicate As Func(Of T, B)) As CT
```

<span data-ttu-id="2ada7-3450">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3450">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Skip 10 _
            Take 5 _
            Skip While x < 10 _
            Take While x > 5 _
            ...
```

<span data-ttu-id="2ada7-3451">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3451">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Skip(10). _
        Take(5). _
        SkipWhile(Function(x) x < 10). _
        TakeWhile(Function(x) x > 5)...
```

<span data-ttu-id="2ada7-3452">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3452">__Note.__</span></span> <span data-ttu-id="2ada7-3453">`Take` 和`Skip`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3453">`Take` and `Skip` are not reserved words.</span></span>


### <a name="order-by-query-operator"></a><span data-ttu-id="2ada7-3454">Order By 查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3454">Order By Query Operator</span></span>

<span data-ttu-id="2ada7-3455">`Order By`查询运算符对出现在范围变量的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3455">The `Order By` query operator orders the values that appear in the range variables.</span></span> 

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

<span data-ttu-id="2ada7-3456">`Order By`查询运算符具有指定应该用于排序的迭代变量的键值的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3456">An `Order By` query operator takes expressions that specify the key values that should be used to order the iteration variables.</span></span> <span data-ttu-id="2ada7-3457">例如，以下查询返回产品价格按排序：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3457">For example, the following query returns products sorted by price:</span></span>

```vb
Dim productsByPrice = _
    From p In Products _
    Order By p.Price _
    Select p.Name
```

<span data-ttu-id="2ada7-3458">排序可以标记为`Ascending`，在这种情况下较小的值出现在之前更大的值，或`Descending`，在这种情况下更大的值出现在之前较小的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3458">An ordering can be marked as `Ascending`, in which case smaller values come before larger values, or `Descending`, in which case larger values come before smaller values.</span></span> <span data-ttu-id="2ada7-3459">如果未指定任何排序的默认值是`Ascending`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3459">The default for an ordering if none is specified is `Ascending`.</span></span> <span data-ttu-id="2ada7-3460">例如，以下查询返回产品首先按成本最高的产品的价格：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3460">For example, the following query returns products sorted by price with the most expensive product first:</span></span>

```vb
Dim productsByPriceDesc = _
    From p In Products _
    Order By p.Price Descending _
    Select p.Name
```

<span data-ttu-id="2ada7-3461">`Order By`查询运算符可以指定多个表达式进行排序，在这种情况下，集合进行排序以嵌套方式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3461">The `Order By` query operator may specify multiple expressions for ordering, in which case the collection is ordered in a nested manner.</span></span> <span data-ttu-id="2ada7-3462">例如，下面的查询进行排序的状态，然后按市/县内每个状态，然后通过在每个城市的邮政编码的客户：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3462">For example, the following query orders customers by state, then by city within each state and then by ZIP code within each city:</span></span>

```vb
Dim customersByLocation = _
    From c In Customers _
    Order By c.State, c.City, c.ZIP _
    Select c.Name, c.State, c.City, c.ZIP
```

<span data-ttu-id="2ada7-3463">中的表达式`Order By`查询运算符必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3463">The expressions in an `Order By` query operator must be classified as a value.</span></span> <span data-ttu-id="2ada7-3464">`Order By`只有集合类型中包含一个或两个以下方法支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3464">An `Order By` query operator is supported only if the collection type contains one or both of the following methods:</span></span>

```vb
Function OrderBy(keySelector As Func(Of T, K)) As CT
Function OrderByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="2ada7-3465">返回类型`CT`必须是*有序集合*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3465">The return type `CT` must be an *ordered collection*.</span></span> <span data-ttu-id="2ada7-3466">有序的集合是包含一个或两种方法的集合类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3466">An ordered collection is a collection type that contains one or both of the methods:</span></span>

```vb
Function ThenBy(keySelector As Func(Of T, K)) As CT
Function ThenByDescending(keySelector As Func(Of T, K)) As CT
```

<span data-ttu-id="2ada7-3467">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3467">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Order By x Ascending, x Mod 2 Descending _
            ...
```

<span data-ttu-id="2ada7-3468">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3468">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.OrderBy(Function(x) x).ThenByDescending(Function(x) x Mod 2)...
```

<span data-ttu-id="2ada7-3469">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3469">__Note.__</span></span> <span data-ttu-id="2ada7-3470">因为查询运算符只需映射到实现特定的查询操作的方法的语法，顺序暂留不规定语言，并由运算符本身的实现。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3470">Because query operators simply map syntax to methods that implement a particular query operation, order preservation is not dictated by the language and is determined by the implementation of the operator itself.</span></span> <span data-ttu-id="2ada7-3471">要重载加法运算符的用户定义的数值类型的实现不执行任何操作类似于一个补充，这是非常类似于用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3471">This is very similar to user-defined operators in that the implementation to overload the addition operator for a user-defined numeric type may not perform anything resembling an addition.</span></span> <span data-ttu-id="2ada7-3472">当然，若要保留可预测性，实现不符合用户预期的内容不建议。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3472">Of course, to preserve predictability, implementing something that does not match user expectations is not recommended.</span></span>

<span data-ttu-id="2ada7-3473">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3473">__Note.__</span></span> <span data-ttu-id="2ada7-3474">`Order` 和`By`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3474">`Order` and `By` are not reserved words.</span></span>


### <a name="group-by-query-operator"></a><span data-ttu-id="2ada7-3475">通过查询运算符的组</span><span class="sxs-lookup"><span data-stu-id="2ada7-3475">Group By Query Operator</span></span>

<span data-ttu-id="2ada7-3476">`Group By`查询运算符进行分组作用域基于一个或多个表达式中的范围变量，然后生成新范围变量基于这些分组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3476">The `Group By` query operator groups the range variables in scope based on one or more expressions, and then produces new range variables based on those groupings.</span></span>

```antlr
GroupByQueryOperator
    : LineTerminator? 'Group' ( LineTerminator? ExpressionRangeVariableDeclarationList )?
      LineTerminator? 'By' LineTerminator? ExpressionRangeVariableDeclarationList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="2ada7-3477">例如，下面的查询进行分组的所有客户`State`，然后计算每个组的计数和平均期限：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3477">For example, the following query groups all customers by `State`, and then computes the count and average age of each group:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group By cust.State _
    Into Count(), Average(cust.Age)
```

<span data-ttu-id="2ada7-3478">`Group By`查询运算符具有三个子句： 可选`Group`子句`By`子句，和`Into`子句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3478">The `Group By` query operator has three clauses: the optional `Group` clause, the `By` clause, and the `Into` clause.</span></span> <span data-ttu-id="2ada7-3479">`Group`子句具有相同的语法和效果`Select`查询运算符，不同之处在于它只影响中可用的范围变量`Into`子句，而不`By`子句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3479">The `Group` clause has the same syntax and effect as a `Select` query operator, except that it only affects the range variables available in the `Into` clause and not the `By` clause.</span></span> <span data-ttu-id="2ada7-3480">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3480">For example:</span></span>

```vb
Dim averageAges = _
    From cust In Customers _
    Group cust.Age By cust.State _
    Into Count(), Average(Age)
```

<span data-ttu-id="2ada7-3481">`By`子句声明表达式用作在分组操作的键值的范围变量。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3481">The `By` clause declares expression range variables that are used as key values in the grouping operation.</span></span> <span data-ttu-id="2ada7-3482">`Into`子句允许通过格式正确的组的每个计算聚合的范围变量的表达式声明`By`子句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3482">The `Into` clause allows the declaration of expression range variables that calculate aggregations over each of the groups formed by the `By` clause.</span></span> <span data-ttu-id="2ada7-3483">内`Into`子句，表达式范围变量只能分配是方法调用的表达式的*聚合函数*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3483">Within the `Into` clause, the expression range variable can only be assigned an expression which is a method invocation of an *aggregate function*.</span></span> <span data-ttu-id="2ada7-3484">聚合函数是在组 （其中不一定是相同的集合类型的原始集合） 其外观类似于以下方法之一的集合类型的函数：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3484">An aggregate function is a function on the collection type of the group (which may not necessarily be the same collection type of the original collection) which looks like either of the following methods:</span></span>

```vb
Function _name_() As _type_
Function _name_(selector As Func(Of T, R)) As R
```

<span data-ttu-id="2ada7-3485">如果聚合函数采用委托参数，则调用表达式可以具有一个参数表达式必须分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3485">If an aggregate function takes a delegate argument, then the invocation expression can have an argument expression that must be classified as a value.</span></span>  <span data-ttu-id="2ada7-3486">参数表达式可以使用作用域; 中的范围变量在为聚合函数的调用，这些范围变量表示格式正确，并非所有集合中的值的组中的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3486">The argument expression can use the range variables that are in scope; within the call to an aggregate function, those range variables represent the values in the group being formed, not all of the values in the collection.</span></span> <span data-ttu-id="2ada7-3487">例如，在本部分中的原始示例`Average`函数计算每个状态而不是为所有客户的客户的年龄段的平均值在一起。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3487">For example, in the original example in this section the `Average` function calculates the average of the customers' ages per state rather than for all of the customers together.</span></span>

<span data-ttu-id="2ada7-3488">所有集合类型被都视为具有聚合函数`Group`定义它，它不采用任何参数，并只需返回的组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3488">All collection types are considered to have the aggregate function `Group` defined on it, which takes no parameters and simply returns the group.</span></span> <span data-ttu-id="2ada7-3489">集合类型可能会提供其他标准聚合函数是：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3489">Other standard aggregate functions that a collection type may provide are:</span></span>

<span data-ttu-id="2ada7-3490">`Count` 和`LongCount`，这组或组中满足条件的布尔表达式的元素数中返回的元素的计数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3490">`Count` and `LongCount`, which return the count of the elements in the group or the count of the elements in the group that satisfy a Boolean expression.</span></span> <span data-ttu-id="2ada7-3491">`Count` 和`LongCount`集合类型包含的方法之一时才受支持：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3491">`Count` and `LongCount` are supported only if the collection type contains one of the methods:</span></span>

```vb
Function Count() As N
Function Count(selector As Func(Of T, B)) As N
Function LongCount() As N
Function LongCount(selector As Func(Of T, B)) As N
```

<span data-ttu-id="2ada7-3492">`Sum`它返回的表达式的和跨组中的所有元素。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3492">`Sum`, which returns the sum of an expression across all the elements in the group.</span></span> <span data-ttu-id="2ada7-3493">`Sum` 集合类型包含的方法之一时才支持：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3493">`Sum` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Sum() As N
Function Sum(selector As Func(Of T, N)) As N
```

<span data-ttu-id="2ada7-3494">`Min` 这组中的所有元素返回表达式的最小值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3494">`Min` which returns the minimum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="2ada7-3495">`Min` 集合类型包含的方法之一时才支持：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3495">`Min` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Min() As N
Function Min(selector As Func(Of T, N)) As N
```

<span data-ttu-id="2ada7-3496">`Max`表示组中的所有元素返回表达式的最大值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3496">`Max`, which returns the maximum value of an expression across all the elements in the group.</span></span> <span data-ttu-id="2ada7-3497">`Max` 集合类型包含的方法之一时才支持：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3497">`Max` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Max() As N
Function Max(selector As Func(Of T, N)) As N
```

<span data-ttu-id="2ada7-3498">`Average`表示组中的所有元素返回表达式的平均值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3498">`Average`, which returns the average of an expression across all the elements in the group.</span></span> <span data-ttu-id="2ada7-3499">`Average` 集合类型包含的方法之一时才支持：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3499">`Average` is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Average() As N
Function Average(selector As Func(Of T, N)) As N
```

<span data-ttu-id="2ada7-3500">`Any`用于确定组是否包含成员，或如果布尔表达式为 true 的组中任何元素。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3500">`Any`, which determines whether a group contains members or if a Boolean expression is true for any element in the group.</span></span> <span data-ttu-id="2ada7-3501">`Any` 返回一个值，可以使用布尔表达式中，集合类型包含的方法之一时才受支持：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3501">`Any` returns a value that can be used in a Boolean expression and is supported only if the collection type contains one of the methods:</span></span>

```vb
Function Any() As B
Function Any(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="2ada7-3502">`All`用于确定是否在组中的所有元素，则返回 true 的布尔表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3502">`All`, which determines whether a Boolean expression is true for all elements in the group.</span></span> <span data-ttu-id="2ada7-3503">`All` 返回一个值，可以使用布尔表达式中，集合类型包含一种方法时才受支持：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3503">`All` returns a value that can be used in a Boolean expression and is supported only if the collection type contains a method:</span></span>

```vb
Function All(predicate As Func(Of T, B)) As B
```

<span data-ttu-id="2ada7-3504">之后`Group By`查询运算符，作用域中以前的范围变量被隐藏，并且范围变量引入`By`和`Into`子句都可用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3504">After a `Group By` query operator, the range variables previously in scope are hidden, and the range variables introduced by the `By` and `Into` clauses are available.</span></span> <span data-ttu-id="2ada7-3505">一个`Group By`集合类型包含的方法时，才支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3505">A `Group By` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      resultSelector As Func(Of K, CT, R)) As CR
```

<span data-ttu-id="2ada7-3506">范围变量声明中的`Group`子句支持仅当集合类型包含的方法：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3506">Range variable declarations in the `Group` clause are supported only if the collection type contains the method:</span></span>

```vb
Function GroupBy(keySelector As Func(Of T, K), _
                      elementSelector As Func(Of T, S), _
                      resultSelector As Func(Of K, CS, R)) As CR
```

<span data-ttu-id="2ada7-3507">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3507">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = From x In xs _
            Group y = x * 10, z = x / 10 By evenOdd = x Mod 2 _
            Into Sum(y), Average(z) _
            ...
```

<span data-ttu-id="2ada7-3508">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3508">is generally translated to</span></span>

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

<span data-ttu-id="2ada7-3509">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3509">__Note.__</span></span> <span data-ttu-id="2ada7-3510">`Group``By`，和`Into`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3510">`Group`, `By`, and `Into` are not reserved words.</span></span>


### <a name="aggregate-query-operator"></a><span data-ttu-id="2ada7-3511">聚合查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3511">Aggregate Query Operator</span></span>

<span data-ttu-id="2ada7-3512">`Aggregate`查询运算符执行类似的功能作为`Group By`运算符，但它允许聚合已形成的组。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3512">The `Aggregate` query operator performs a similar function as the `Group By` operator, except it allows aggregating over groups that have already been formed.</span></span> <span data-ttu-id="2ada7-3513">因为已正确组，`Into`子句`Aggregate`运算符不会隐藏范围内的范围变量的查询 (这种方式，`Aggregate`更像是`Let`，并`Group By`更像是`Select`)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3513">Because the group has already been formed, the `Into` clause of an `Aggregate` query operator does not hide the range variables in scope (in this way, `Aggregate` is more like a `Let`, and `Group By` is more like a `Select`).</span></span>

```antlr
AggregateQueryOperator
    : LineTerminator? 'Aggregate' LineTerminator? CollectionRangeVariableDeclaration QueryOperator*
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="2ada7-3514">例如，下面的查询聚合华盛顿州的客户的订单的总计：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3514">For example, the following query aggregates the total of all the orders placed by customers in Washington:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Into Sum(order.Total)
```

<span data-ttu-id="2ada7-3515">此查询的结果是其元素类型的集合是具有一个名为属性的匿名类型`cust`化为`Customer`和名为的属性`Sum`化为`Integer`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3515">The result of this query is a collection whose element type is an anonymous type with a property named `cust` typed as `Customer` and a property named `Sum` typed as `Integer`.</span></span>

<span data-ttu-id="2ada7-3516">与不同`Group By`，其他查询运算符可以置于之间`Aggregate`和`Into`子句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3516">Unlike `Group By`, additional query operators can be placed between the `Aggregate` and `Into` clauses.</span></span> <span data-ttu-id="2ada7-3517">之间`Aggregate`子句和末尾`Into`子句中，作用域，包括那些由声明中的所有范围变量`Aggregate`子句只能用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3517">Between an `Aggregate` clause and the end of the `Into` clause, all range variables in scope, including those declared by the `Aggregate` clause can be used.</span></span> <span data-ttu-id="2ada7-3518">例如，以下查询聚合放置的所有订单的总和，按华盛顿州在 2006 年之前的客户：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3518">For example, the following query aggregates the sum total of all the orders placed by customers in Washington before 2006:</span></span>

```vb
Dim orderTotals = _
    From cust In Customers _
    Where cust.State = "WA" _
    Aggregate order In cust.Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum = Sum(order.Total)
```

<span data-ttu-id="2ada7-3519">`Aggregate`运算符还可用于开始查询表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3519">The `Aggregate` operator can also be used to start a query expression.</span></span> <span data-ttu-id="2ada7-3520">在这种情况下，查询表达式的结果将是通过计算出的单个值`Into`子句。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3520">In this case, the result of the query expression will be the single value computed by the `Into` clause.</span></span> <span data-ttu-id="2ada7-3521">例如，以下查询将计算在 2006 年 1 月 1 日之前的所有订单总计值之和：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3521">For example, the following query calculates the sum of all the order totals before January 1st, 2006:</span></span>

```vb
Dim ordersTotal = _
    Aggregate order In Orders _
    Where order.Date <= #01/01/2006# _
    Into Sum(order.Total)
```

<span data-ttu-id="2ada7-3522">查询的结果是单个`Integer`值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3522">The result of the query is a single `Integer` value.</span></span> <span data-ttu-id="2ada7-3523">`Aggregate`查询运算符都始终可用 （尽管聚合函数是也必须提供有效的表达式）。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3523">An `Aggregate` query operator is always available (although the aggregate function must be also be available for the expression to be valid).</span></span> <span data-ttu-id="2ada7-3524">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3524">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    Aggregate x In xs _
    Where x < 5 _
    Into Sum()
```

<span data-ttu-id="2ada7-3525">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3525">is generally translated to</span></span>

```vb
Dim xs() As Integer = ...
Dim zs = _
    xs.Where(Function(x) x < 5).Sum()
```

<span data-ttu-id="2ada7-3526">__请注意。__ `Aggregate`</span><span class="sxs-lookup"><span data-stu-id="2ada7-3526">__Note.__ `Aggregate`</span></span> <span data-ttu-id="2ada7-3527">和`Into`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3527">and `Into` are not reserved words.</span></span>


### <a name="group-join-query-operator"></a><span data-ttu-id="2ada7-3528">组联接查询运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3528">Group Join Query Operator</span></span>

<span data-ttu-id="2ada7-3529">`Group Join`查询运算符的函数组合起来`Join`和`Group By`到单个运算符的查询运算符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3529">The `Group Join` query operator combines the functions of the `Join` and `Group By` query operators into a single operator.</span></span> <span data-ttu-id="2ada7-3530">`Group Join` 联接两个集合根据匹配项从组合在一起的元素中提取所有匹配联接左侧的特定元素联接右侧元素。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3530">`Group Join` joins two collections based on matching keys extracted from the elements, grouping together all of the elements on the right side of the join that match a particular element on the left side of the join.</span></span> <span data-ttu-id="2ada7-3531">因此，则运算符产生层次结构的结果的集。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3531">Thus, the operator produces a set of hierarchical results.</span></span>

```antlr
GroupJoinQueryOperator
    : LineTerminator? 'Group' 'Join' LineTerminator? CollectionRangeVariableDeclaration
      JoinOrGroupJoinQueryOperator? LineTerminator? 'On' LineTerminator? JoinConditionList
      LineTerminator? 'Into' LineTerminator? ExpressionRangeVariableDeclarationList
    ;
```

<span data-ttu-id="2ada7-3532">例如，下面的查询生成包含单个客户的名称，一组所有其订单和所有这些订单的总量的元素：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3532">For example, the following query produces elements that contain a single customer's name, a group of all of their orders, and the total amount of all of those orders:</span></span>

```vb
Dim custsWithOrders = _
    From cust In Customers _
    Group Join order In Orders On cust.ID Equals order.CustomerID _ 
    Into Orders = Group, OrdersTotal = Sum(order.Total) _
    Select cust.Name, Orders, OrdersTotal
```

<span data-ttu-id="2ada7-3533">查询的结果是其元素类型是具有三个属性的匿名类型集合：`Name`作为类型化`String`，`Orders`其元素类型的集合类型化为`Order`，和`OrdersTotal`作为类型化`Integer`.</span><span class="sxs-lookup"><span data-stu-id="2ada7-3533">The result of the query is a collection whose element type is an anonymous type with three properties: `Name`, typed as `String`, `Orders` typed as a collection whose element type is `Order`, and `OrdersTotal`, typed as `Integer`.</span></span> <span data-ttu-id="2ada7-3534">一个`Group Join`集合类型包含的方法时，才支持查询运算符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3534">A `Group Join` query operator is supported only if the collection type contains the method:</span></span>

```vb
Function GroupJoin(inner As CS, _
                         outerSelector As Func(Of T, K), _
                         innerSelector As Func(Of S, K), _
                         resultSelector As Func(Of T, CS, R)) As CR
```

<span data-ttu-id="2ada7-3535">代码</span><span class="sxs-lookup"><span data-stu-id="2ada7-3535">The code</span></span>

```vb
Dim xs() As Integer = ...
Dim ys() As Integer = ...
Dim zs = From x In xs _
            Group Join y in ys On x Equals y _
            Into g = Group _
            ...
```

<span data-ttu-id="2ada7-3536">通常会转换为</span><span class="sxs-lookup"><span data-stu-id="2ada7-3536">is generally translated to</span></span>

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

<span data-ttu-id="2ada7-3537">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3537">__Note.__</span></span> <span data-ttu-id="2ada7-3538">`Group``Join`，和`Into`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3538">`Group`, `Join`, and `Into` are not reserved words.</span></span>


## <a name="conditional-expressions"></a><span data-ttu-id="2ada7-3539">条件表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-3539">Conditional Expressions</span></span>

<span data-ttu-id="2ada7-3540">条件`If`表达式测试表达式并返回一个值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3540">A conditional `If` expression tests an expression and returns a value.</span></span>

```antlr
ConditionalExpression
    : 'If' OpenParenthesis BooleanExpression Comma Expression Comma Expression CloseParenthesis
    | 'If' OpenParenthesis Expression Comma Expression CloseParenthesis
    ;
```

<span data-ttu-id="2ada7-3541">与不同`IIF`运行时函数，但是，条件表达式仅计算其操作数如有必要。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3541">Unlike the `IIF` runtime function, however, a conditional expression only evaluates its operands if necessary.</span></span> <span data-ttu-id="2ada7-3542">因此，对于示例中，表达式`If(c Is Nothing, c.Name, "Unknown")`如果不会引发异常的值`c`是`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3542">Thus, for example, the expression `If(c Is Nothing, c.Name, "Unknown")` will not throw an exception if the value of `c` is `Nothing`.</span></span> <span data-ttu-id="2ada7-3543">条件表达式有两种形式： 一种采用两个操作数和一个采用三个操作数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3543">The conditional expression has two forms: one that takes two operands and one that takes three operands.</span></span>

<span data-ttu-id="2ada7-3544">如果提供了三个操作数，所有三个表达式必须分类为值，并在第一个操作数必须是布尔表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3544">If three operands are provided, all three expressions must be classified as values, and the first operand must be a Boolean expression.</span></span> <span data-ttu-id="2ada7-3545">如果结果为的表达式为 true，则第二个表达式将是结果的运算符，否则第三个表达式将运算符的结果。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3545">If the result is of the expression is true, then the second expression will be the result of the operator, otherwise the third expression will be the result of the operator.</span></span> <span data-ttu-id="2ada7-3546">该表达式的结果类型是第二个和第三个表达式的类型之间的基准类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3546">The result type of the expression is the dominant type between the types of the second and third expression.</span></span> <span data-ttu-id="2ada7-3547">如果没有基准类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3547">If there is no dominant type, then a compile-time error occurs.</span></span>

<span data-ttu-id="2ada7-3548">如果提供了两个操作数，两个操作数必须分类为值，并在第一个操作数必须是引用类型或可以为 null 值类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3548">If two operands are provided, both of the operands must be classified as values, and the first operand must be either a reference type or a nullable value type.</span></span> <span data-ttu-id="2ada7-3549">表达式`If(x, y)`然后评估像是表达式`If(x IsNot Nothing, x, y)`，有两个例外。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3549">The expression `If(x, y)` is then evaluated as if was the expression `If(x IsNot Nothing, x, y)`, with two exceptions.</span></span> <span data-ttu-id="2ada7-3550">首先，第一个表达式只计算一次，第二，如果第二个操作数的类型是不可以为 null 的值类型，并且第一个操作数的类型为，`?`从第一个操作数的类型中删除时确定的基准类型表达式的结果类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3550">First, the first expression is only ever evaluated once, and second, if the second operand's type is a non-nullable value type and the first operand's type is, the `?` is removed from the type of the first operand when determining the dominant type for the result type of the expression.</span></span> <span data-ttu-id="2ada7-3551">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3551">For example:</span></span>

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

<span data-ttu-id="2ada7-3552">在这两种形式的表达式，如果一个操作数为`Nothing`，其类型不用于确定基准类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3552">In both forms of the expression, if an operand is `Nothing`, its type is not used to determine the dominant type.</span></span> <span data-ttu-id="2ada7-3553">如果表达式`If(<expression>, Nothing, Nothing)`，基准类型被视为可`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3553">In the case of the expression `If(<expression>, Nothing, Nothing)`, the dominant type is considered to be `Object`.</span></span>


## <a name="xml-literal-expressions"></a><span data-ttu-id="2ada7-3554">XML 文本表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-3554">XML Literal Expressions</span></span>

<span data-ttu-id="2ada7-3555">XML 文本表达式表示的 XML （可扩展标记语言） 1.0 的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3555">An XML literal expression represents an XML (eXtensible Markup Language) 1.0 value.</span></span>

```antlr
XMLLiteralExpression
    : XMLDocument
    | XMLElement
    | XMLProcessingInstruction
    | XMLComment
    | XMLCDATASection
    ;
```

<span data-ttu-id="2ada7-3556">XML 文本表达式的结果是作为一种从类型类型化值`System.Xml.Linq`命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3556">The result of an XML literal expression is a value typed as one of the types from the `System.Xml.Linq` namespace.</span></span> <span data-ttu-id="2ada7-3557">如果该命名空间中的类型不可用，XML 文本表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3557">If the types in that namespace are not available, then an XML literal expression will cause a compile-time error.</span></span> <span data-ttu-id="2ada7-3558">通过从 XML 文本表达式转换构造函数调用所生成的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3558">The values are generated through constructor calls translated from the XML literal expression.</span></span> <span data-ttu-id="2ada7-3559">例如，代码：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3559">For example, the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    <book title="My book"></book>
```

<span data-ttu-id="2ada7-3560">是大致等效于代码：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3560">is roughly equivalent to the code:</span></span>

```vb
Dim book As System.Xml.Linq.XElement = _
    New System.Xml.Linq.XElement( _
        "book", _
        New System.Xml.Linq.XAttribute("title", "My book"))
```

<span data-ttu-id="2ada7-3561">XML 文本表达式可以采用 XML 文档、 XML 元素、 XML 处理指令，XML 注释或 CDATA 节的形式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3561">An XML literal expression can take the form of an XML document, an XML element, an XML processing instruction, an XML comment, or a CDATA section.</span></span>

<span data-ttu-id="2ada7-3562">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3562">__Note.__</span></span> <span data-ttu-id="2ada7-3563">此规范仅包含足够多的 XML 来描述的 Visual Basic 语言的行为的说明。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3563">This specification contains only enough of a description of XML to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="2ada7-3564">XML 的详细信息可从 http://www.w3.org/TR/REC-xml/。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3564">More information on XML can be found at http://www.w3.org/TR/REC-xml/.</span></span>


### <a name="lexical-rules"></a><span data-ttu-id="2ada7-3565">词法规则</span><span class="sxs-lookup"><span data-stu-id="2ada7-3565">Lexical rules</span></span>

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

<span data-ttu-id="2ada7-3566">使用 XML 的词汇规则而不正则 Visual Basic 代码的词汇规则解释 XML 文本表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3566">XML literal expressions are interpreted using the lexical rules of XML instead of the lexical rules of regular Visual Basic code.</span></span> <span data-ttu-id="2ada7-3567">两组规则通常按以下方式不同：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3567">The two sets of rules generally differ in the following ways:</span></span>

* <span data-ttu-id="2ada7-3568">空白是重要的 XML 中。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3568">White space is significant in XML.</span></span> <span data-ttu-id="2ada7-3569">因此，XML 文本表达式的语法显式声明允许有空格。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3569">As a result, the grammar for XML literal expressions explicitly states where white space is allowed.</span></span> <span data-ttu-id="2ada7-3570">不保留空格，除非在其出现在元素内的字符数据的上下文中。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3570">Whitespace is not preserved, except when it occurs in the context of character data within an element.</span></span> <span data-ttu-id="2ada7-3571">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3571">For example:</span></span>

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

* <span data-ttu-id="2ada7-3572">根据 XML 规范，XML 行尾空格进行规范化。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3572">XML end-of-line whitespace is normalized according to the XML specification.</span></span>

* <span data-ttu-id="2ada7-3573">XML 区分大小写。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3573">XML is case-sensitive.</span></span> <span data-ttu-id="2ada7-3574">关键字必须匹配大小写，否则将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3574">Keywords must match casing exactly, or else a compile-time error will occur.</span></span>

* <span data-ttu-id="2ada7-3575">行终止符被视为 XML 中的空白区域。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3575">Line terminators are considered white space in XML.</span></span> <span data-ttu-id="2ada7-3576">因此，没有行继续符的字符都需要 XML 文本表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3576">As a result, no line-continuation characters are needed in XML literal expressions.</span></span>

* <span data-ttu-id="2ada7-3577">XML 不接受全角字符。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3577">XML does not accept full-width characters.</span></span> <span data-ttu-id="2ada7-3578">如果使用了全宽字符，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3578">If full-width characters are used, a compile-time error will occur.</span></span>


### <a name="embedded-expressions"></a><span data-ttu-id="2ada7-3579">使用嵌入式的表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-3579">Embedded expressions</span></span>

<span data-ttu-id="2ada7-3580">XML 文本表达式可以包含*嵌入表达式*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3580">XML literal expressions can contain *embedded expressions*.</span></span> <span data-ttu-id="2ada7-3581">嵌入的表达式是 Visual Basic 表达式的计算和用于在嵌入式表达式的位置的一个或多个值中填充。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3581">An embedded expression is a Visual Basic expression that is evaluated and used to fill in one or more values at the location of embedded expression.</span></span>

```antlr
XMLEmbeddedExpression
    : '<' '%' '=' LineTerminator? Expression LineTerminator? '%' '>'
    ;
```

<span data-ttu-id="2ada7-3582">例如，下面的代码将字符串`John Smith`作为 XML 元素的值：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3582">For example, the following code places the string `John Smith` as the value of the XML element:</span></span>

```vb
Dim name as String = "John Smith"
Dim element As System.Xml.Linq.XElement = <customer><%= name %></customer>
```

<span data-ttu-id="2ada7-3583">可以在许多上下文中嵌入表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3583">Expressions can be embedded in a number of contexts.</span></span> <span data-ttu-id="2ada7-3584">例如，下面的代码生成名为的元素`customer`:</span><span class="sxs-lookup"><span data-stu-id="2ada7-3584">For example, the following code produces an element named `customer`:</span></span>

```vb
Dim name As String = "customer"
Dim element As System.Xml.Linq.XElement = <<%= name %>>John Smith</>
```

<span data-ttu-id="2ada7-3585">可以使用嵌入式的表达式，其中每个上下文指定将接受的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3585">Each context where an embedded expression can be used specifies the types that will be accepted.</span></span> <span data-ttu-id="2ada7-3586">当在嵌入式表达式的表达式一部分的上下文中，Visual Basic 代码的正常词法规则仍适用，，必须为例，使用行继续符：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3586">When within the context of the expression part of an embedded expression, the normal lexical rules for Visual Basic code still apply so, for example, line continuations must be used:</span></span>

```vb
' Visual Basic expression uses line continuation, XML does not
Dim element As System.Xml.Linq.XElement = _
    <<%= name & _
          name %>>John 
                     Smith</>
```


### <a name="xml-documents"></a><span data-ttu-id="2ada7-3587">XML 文档</span><span class="sxs-lookup"><span data-stu-id="2ada7-3587">XML Documents</span></span>

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

<span data-ttu-id="2ada7-3588">XML 文档的结果值的类型为`System.Xml.Linq.XDocument`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3588">An XML document results in a value typed as `System.Xml.Linq.XDocument`.</span></span> <span data-ttu-id="2ada7-3589">XML 文本表达式中的 XML 文档所需与 XML 1.0 规范中，指定 XML 文档序言;XML 文本表达式，而无需在 XML 文档序言被解释为单个实体。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3589">Unlike the XML 1.0 specification, XML documents in XML literal expressions are required to specify the XML document prologue; XML literal expressions without the XML document prologue are interpreted as their individual entity.</span></span> <span data-ttu-id="2ada7-3590">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3590">For example:</span></span>

```vb
Dim doc As System.Xml.Linq.XDocument = _
    <?xml version="1.0"?>
    <?instruction?>
    <customer>Bob</>

Dim pi As System.Xml.Linq.XProcessingInstruction = _
    <?instruction?>
```

<span data-ttu-id="2ada7-3591">一个 XML 文档可以包含嵌入式的表达式的类型可以是任何类型;在运行时，但是，该对象必须满足的要求`XDocument`构造函数或运行时错误会发生。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3591">An XML document can contain an embedded expression whose type can be any type; at runtime, however, the object must satisfy the requirements of the `XDocument` constructor or a run-time error will occur.</span></span>

<span data-ttu-id="2ada7-3592">与常规 XML 不同 XML 文档表达式不支持 Dtd （文档类型声明）。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3592">Unlike regular XML, XML document expressions do not support DTDs (Document Type Declarations).</span></span> <span data-ttu-id="2ada7-3593">此外，编码属性，如果提供，将忽略由于编码的 Xml 文本表达式始终与源文件本身的编码相同。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3593">Also, the encoding attribute, if supplied, will be ignored since the encoding of the Xml literal expression is always the same as the encoding of the source file itself.</span></span>

<span data-ttu-id="2ada7-3594">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3594">__Note.__</span></span> <span data-ttu-id="2ada7-3595">尽管将忽略 encoding 属性，它将是仍有效的特性，以便维护源代码中包含任何有效的 Xml 1.0 文档的功能。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3595">Although the encoding attribute is ignored, it is still valid attribute in order to maintain the ability to include any valid Xml 1.0 documents in source code.</span></span>


### <a name="xml-elements"></a><span data-ttu-id="2ada7-3596">XML 元素</span><span class="sxs-lookup"><span data-stu-id="2ada7-3596">XML Elements</span></span>

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

<span data-ttu-id="2ada7-3597">XML 元素的结果值的类型为`System.Xml.Linq.XElement`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3597">An XML element results in a value typed as `System.Xml.Linq.XElement`.</span></span> <span data-ttu-id="2ada7-3598">与常规 XML 不同 XML 元素可以省略的结束标记中的名称，并将关闭当前的大多数嵌套元素。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3598">Unlike regular XML, XML elements can omit the name in the closing tag and the current most-nested element will be closed.</span></span> <span data-ttu-id="2ada7-3599">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3599">For example:</span></span>

```vb
Dim name = <name>Bob</>
```

<span data-ttu-id="2ada7-3600">特性声明中 XML 元素中生成值的类型为`System.Xml.Linq.XAttribute`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3600">Attribute declarations in an XML element result in values typed as `System.Xml.Linq.XAttribute`.</span></span> <span data-ttu-id="2ada7-3601">根据 XML 规范，属性值进行规范化。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3601">Attribute values are normalized according to the XML specification.</span></span> <span data-ttu-id="2ada7-3602">当属性的值是`Nothing`属性将不会创建，因此不需要检查的属性值表达式`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3602">When the value of an attribute is `Nothing` the attribute will not be created, so the attribute value expression will not have to be checked for `Nothing`.</span></span> <span data-ttu-id="2ada7-3603">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3603">For example:</span></span>

```vb
Dim expr = Nothing

' Throws null argument exception
Dim direct = New System.Xml.Linq.XElement( _
    "Name", _
    New System.Xml.Linq.XAttribute("Length", expr))

' Doesn't throw exception, the result is <Name/>
Dim literal = <Name Length=<%= expr %>/>
```

<span data-ttu-id="2ada7-3604">XML 元素和属性可以包含在以下位置中的嵌套的表达式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3604">XML elements and attributes can contain nested expressions in the following places:</span></span>

<span data-ttu-id="2ada7-3605">在这种情况下嵌入式表达式必须隐式转换为类型的值的元素的名称`System.Xml.Linq.XName`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3605">The name of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="2ada7-3606">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3606">For example:</span></span>

```vb
Dim name = <<%= "name" %>>Bob</>
```

<span data-ttu-id="2ada7-3607">在这种情况下嵌入式表达式必须隐式转换为类型的值的元素的属性的名称`System.Xml.Linq.XName`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3607">The name of an attribute of the element, in which case the embedded expression must be a value of a type implicitly convertible to `System.Xml.Linq.XName`.</span></span> <span data-ttu-id="2ada7-3608">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3608">For example:</span></span>

```vb
Dim name = <name <%= "length" %>="3">Bob</>
```

<span data-ttu-id="2ada7-3609">在其中用例嵌入式表达式可以是任何类型的值的元素的属性的值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3609">The value of an attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="2ada7-3610">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3610">For example:</span></span>

```vb
Dim name = <name length=<%= 3 %>>Bob</>
```

<span data-ttu-id="2ada7-3611">在其中用例嵌入式表达式可以是任何类型的值的元素的属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3611">An attribute of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="2ada7-3612">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3612">For example:</span></span>

```vb
Dim name = <name <%= new XAttribute("length", 3) %>>Bob</>
```

<span data-ttu-id="2ada7-3613">在其中用例嵌入式表达式可以是任何类型的值的元素的内容。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3613">The content of the element, in which case the embedded expression can be a value of any type.</span></span> <span data-ttu-id="2ada7-3614">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3614">For example:</span></span>

```vb
Dim name = <name><%= "Bob" %></>
```

<span data-ttu-id="2ada7-3615">如果嵌入式表达式的类型为`Object()`，该数组将传递到 paramarray 作为`XElement`构造函数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3615">If the type of the embedded expression is `Object()`, the array will be passed as a paramarray to the `XElement` constructor.</span></span>


### <a name="xml-namespaces"></a><span data-ttu-id="2ada7-3616">XML 命名空间</span><span class="sxs-lookup"><span data-stu-id="2ada7-3616">XML Namespaces</span></span>

<span data-ttu-id="2ada7-3617">由 XML 命名空间 1.0 规范定义的 XML 元素可以包含 XML 命名空间声明。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3617">XML elements can contain XML namespace declarations, as defined by the XML namespaces 1.0 specification.</span></span>

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

<span data-ttu-id="2ada7-3618">定义命名空间的限制`xml`和`xmlns`强制的并且将产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3618">The restrictions on defining the namespaces `xml` and `xmlns` are enforced and will produce compile-time errors.</span></span> <span data-ttu-id="2ada7-3619">XML 命名空间声明不能有它们的值; 一个嵌入式的表达式提供的值必须为非空字符串。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3619">XML namespace declarations cannot have an embedded expression for their value; the value supplied must be a non-empty string literal.</span></span> <span data-ttu-id="2ada7-3620">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3620">For example:</span></span>

```vb
' Declares a valid namespace
Dim customer = <db:customer xmlns:db="http://example.org/database">Bob</>

' Error: xmlns cannot be re-defined
Dim bad1 = <elem xmlns:xmlns="http://example.org/namespace"/>

' Error: cannot have an embedded expression
Dim bad2 = <elem xmlns:db=<%= "http://example.org/database" %>>Bob</>
```

<span data-ttu-id="2ada7-3621">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3621">__Note.__</span></span> <span data-ttu-id="2ada7-3622">此规范仅包含足够多的 XML 命名空间来介绍了 Visual Basic 语言的行为的说明。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3622">This specification contains only enough of a description of XML namespace to describe the behavior of the Visual Basic language.</span></span> <span data-ttu-id="2ada7-3623">XML 命名空间的详细信息可从 http://www.w3.org/TR/REC-xml-names/。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3623">More information on XML namespaces can be found at http://www.w3.org/TR/REC-xml-names/.</span></span>

<span data-ttu-id="2ada7-3624">可以使用命名空间名称限定 XML 元素和属性名称。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3624">XML element and attribute names can be qualified using namespace names.</span></span> <span data-ttu-id="2ada7-3625">命名空间如下所示常规 XML 绑定时，请在文件级别声明的任何命名空间导入的异常被视为在封闭的声明，本身包含的声明由编译的任何命名空间导入上下文中声明环境。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3625">Namespaces are bound as in regular XML, with the exception that any namespace imports declared at the file level are considered to be declared in a context enclosing the declaration, which is itself enclosed by any namespace imports declared by the compilation environment.</span></span> <span data-ttu-id="2ada7-3626">如果找不到命名空间名称，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3626">If a namespace name cannot be found, a compile-time error occurs.</span></span> <span data-ttu-id="2ada7-3627">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3627">For example:</span></span>

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

<span data-ttu-id="2ada7-3628">在元素中声明的 XML 命名空间不适用于 XML 文本内嵌入的表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3628">XML namespaces declared in an element do not apply to XML literals inside embedded expressions.</span></span> <span data-ttu-id="2ada7-3629">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3629">For example:</span></span>

```vb
' Error: Namespace prefix 'db' is not declared
Dim customer = _
    <db:customer xmlns:db="http://example.org/database">
        <%= <db:customer>Bob</> %>
    </>
```

<span data-ttu-id="2ada7-3630">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3630">__Note.__</span></span> <span data-ttu-id="2ada7-3631">这是因为在嵌入式的表达式可以是任何内容，包括函数调用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3631">This is because the embedded expression can be anything, including a function call.</span></span> <span data-ttu-id="2ada7-3632">如果函数调用包含 XML 文本表达式，它是不清楚是否程序员希望要应用或忽略的 XML 命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3632">If the function call contained an XML literal expression, it is not clear whether programmers would expect the XML namespace to be applied or ignored.</span></span>


### <a name="xml-processing-instructions"></a><span data-ttu-id="2ada7-3633">XML 处理指令</span><span class="sxs-lookup"><span data-stu-id="2ada7-3633">XML Processing Instructions</span></span>

<span data-ttu-id="2ada7-3634">XML 处理指令的结果值的类型为`System.Xml.Linq.XProcessingInstruction`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3634">An XML processing instruction results in a value typed as `System.Xml.Linq.XProcessingInstruction`.</span></span> <span data-ttu-id="2ada7-3635">XML 处理指令不能包含嵌入的表达式，因为它们是处理指令中的有效语法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3635">XML processing instructions cannot contain embedded expressions, as they are valid syntax within the processing instruction.</span></span>

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

### <a name="xml-comments"></a><span data-ttu-id="2ada7-3636">XML 注释</span><span class="sxs-lookup"><span data-stu-id="2ada7-3636">XML Comments</span></span>

<span data-ttu-id="2ada7-3637">XML 注释的结果值的类型为`System.Xml.Linq.XComment`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3637">An XML comment results in a value typed as `System.Xml.Linq.XComment`.</span></span> <span data-ttu-id="2ada7-3638">XML 注释不能包含嵌入的表达式，因为它们是注释中的有效语法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3638">XML comments cannot contain embedded expressions, as they are valid syntax within the comment.</span></span>

```antlr
XMLComment
    : '<' '!' '-' '-' XMLCommentCharacter* '-' '-' '>'
    ;

XMLCommentCharacter
    : '<Any XMLCharacter except dash (0x002D)>'
    | '-' '<Any XMLCharacter except dash (0x002D)>'
    ;
```

### <a name="cdata-sections"></a><span data-ttu-id="2ada7-3639">CDATA 节</span><span class="sxs-lookup"><span data-stu-id="2ada7-3639">CDATA sections</span></span>

<span data-ttu-id="2ada7-3640">CDATA 节的结果值的类型为`System.Xml.Linq.XCData`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3640">A CDATA section results in a value typed as `System.Xml.Linq.XCData`.</span></span> <span data-ttu-id="2ada7-3641">CDATA 节不能包含嵌入的表达式，因为它们是将 CDATA 节中的有效语法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3641">CDATA sections cannot contain embedded expressions, as they are valid syntax within the CDATA section.</span></span>

```antlr
XMLCDATASection
    : '<' '!' ( 'CDATA' '[' XMLCDATASectionString? ']' )? '>'
    ;

XMLCDATASectionString
    : '<Any XMLString that does not contain the string "]]>">'
    ;
```

## <a name="xml-member-access-expressions"></a><span data-ttu-id="2ada7-3642">XML 成员访问表达式</span><span class="sxs-lookup"><span data-stu-id="2ada7-3642">XML Member Access Expressions</span></span>

<span data-ttu-id="2ada7-3643">XML 成员访问表达式可以访问 XML 值的成员。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3643">An XML member access expression accesses the members of an XML value.</span></span>

```antlr
XMLMemberAccessExpression
    : Expression '.' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? '<' XMLQualifiedName '>'
    | Expression '.' LineTerminator? '@' LineTerminator? IdentifierOrKeyword
    | Expression '.' '.' '.' LineTerminator? '<' XMLQualifiedName '>'
    ;
```

<span data-ttu-id="2ada7-3644">有三种类型的 XML 成员访问表达式：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3644">There are three types of XML member access expressions:</span></span>

* <span data-ttu-id="2ada7-3645">*元素访问*，在其中的 XML 名称遵循单一点。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3645">*Element access*, in which an XML name follows a single dot.</span></span> <span data-ttu-id="2ada7-3646">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3646">For example:</span></span>

  ```vb
  Dim customer = _
      <customer>
          <name>Bob</>
      </>
  Dim customerName = customer.<name>.Value
  ```

  <span data-ttu-id="2ada7-3647">元素访问映射到的函数：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3647">Element access maps to the function:</span></span>

  ```vb
  Function Elements(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XNode)
  ```

  <span data-ttu-id="2ada7-3648">因此上面的示例等同于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3648">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerName = customer.Elements("name").Value
  ```

* <span data-ttu-id="2ada7-3649">*属性访问*，在其中是 Visual Basic 标识符遵循一个圆点和一个在登录或 XML 名称遵循一个圆点和一个 at 符号。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3649">*Attribute access*, in which a Visual Basic identifier follows a dot and an at sign, or an XML name follows a dot and an at sign.</span></span> <span data-ttu-id="2ada7-3650">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3650">For example:</span></span>

  ```vb
  Dim customer = <customer age="30"/>
  Dim customerAge = customer.@age
  ```

  <span data-ttu-id="2ada7-3651">属性访问映射到函数：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3651">Attribute access maps to the function:</span></span>

  ```vb
  Function AttributeValue(name As System.Xml.Linq.XName) as String
  ```

  <span data-ttu-id="2ada7-3652">因此上面的示例等同于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3652">So the above example is equivalent to:</span></span>

  ```vb
  Dim customerAge = customer.AttributeValue("age")
  ```

  <span data-ttu-id="2ada7-3653">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3653">__Note.__</span></span> <span data-ttu-id="2ada7-3654">`AttributeValue`扩展方法 (以及相关的扩展属性`Value`) 目前未在任何程序集中定义。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3654">The `AttributeValue` extension method (as well as the related extension property `Value`) is not currently defined in any assembly.</span></span> <span data-ttu-id="2ada7-3655">如果需要扩展成员，它们会自动定义正在生成的程序集。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3655">If the extension members are needed, they are automatically defined in the assembly being produced.</span></span>

* <span data-ttu-id="2ada7-3656">*后代访问*，在其中 XML 名称遵循三个点。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3656">*Descendents access*, in which an XML names follows three dots.</span></span> <span data-ttu-id="2ada7-3657">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3657">For example:</span></span>

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

  <span data-ttu-id="2ada7-3658">后代访问映射到的函数：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3658">Descendents access maps to the function:</span></span>

  ```vb
  Function Descendents(name As System.Xml.Linq.XName) As _
      System.Collections.Generic.IEnumerable(Of _
          System.Xml.Linq.XElement)
  ```

  <span data-ttu-id="2ada7-3659">因此上面的示例等同于：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3659">So the above example is equivalent to:</span></span>

  ```vb
  Dim customers = company.Descendants("customer")
  ```

<span data-ttu-id="2ada7-3660">XML 成员访问表达式的基表达式必须是一个值，并必须为类型：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3660">The base expression of an XML member access expression must be a value and must be of the type:</span></span>

* <span data-ttu-id="2ada7-3661">如果元素或其子代访问，请`System.Xml.Linq.XContainer`或派生的类型，或`System.Collections.Generic.IEnumerable(Of T)`或派生的类型，其中`T`是`System.Xml.Linq.XContainer`或派生的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3661">If an element or descendents access,  `System.Xml.Linq.XContainer` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XContainer` or a derived type.</span></span>

* <span data-ttu-id="2ada7-3662">如果属性访问`System.Xml.Linq.XElement`或派生的类型，或`System.Collections.Generic.IEnumerable(Of T)`或派生的类型，其中`T`是`System.Xml.Linq.XElement`或派生的类型。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3662">If an attribute access, `System.Xml.Linq.XElement` or a derived type, or `System.Collections.Generic.IEnumerable(Of T)` or a derived type, where `T` is `System.Xml.Linq.XElement` or a derived type.</span></span>

<span data-ttu-id="2ada7-3663">XML 成员访问表达式中的名称不能为空。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3663">Names in XML member access expressions cannot be empty.</span></span> <span data-ttu-id="2ada7-3664">它们可以使用任何由导入定义的命名空间的限定命名空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3664">They can be namespace qualified, using any namespaces defined by imports.</span></span> <span data-ttu-id="2ada7-3665">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3665">For example:</span></span>

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

<span data-ttu-id="2ada7-3666">后 dot(s) 在 XML 成员访问表达式中，或在尖括号内和名称之间不允许有空格。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3666">Whitespace is not allowed after the dot(s) in an XML member access expression, or between the angle brackets and the name.</span></span> <span data-ttu-id="2ada7-3667">例如：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3667">For example:</span></span>

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

<span data-ttu-id="2ada7-3668">如果在类型`System.Xml.Linq`命名空间不可用，则 XML 成员访问表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3668">If the types in the `System.Xml.Linq` namespace are not available, then an XML member access expression will cause a compile-time error.</span></span>


## <a name="await-operator"></a><span data-ttu-id="2ada7-3669">Await 运算符</span><span class="sxs-lookup"><span data-stu-id="2ada7-3669">Await Operator</span></span>

<span data-ttu-id="2ada7-3670">Await 运算符与异步方法，部分所述[异步方法](statements.md#async-methods)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3670">The await operator is related to async methods, which are described in Section [Async Methods](statements.md#async-methods).</span></span>

```antlr
AwaitOperatorExpression
    : 'Await' Expression
    ;
```

<span data-ttu-id="2ada7-3671">`Await` 是一个保留的字，如果它在其中出现的立即封闭方法或 lambda 表达式具有`Async`修饰符，并且如果`Await`后，将显示`Async`修饰符; 它在其他位置未保留空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3671">`Await` is a reserved word if the immediately enclosing method or lambda expression in which it appears has an `Async` modifier, and if the `Await` appears after that `Async` modifier; it is unreserved elsewhere.</span></span> <span data-ttu-id="2ada7-3672">它也是在预处理器指令未保留空间。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3672">It is also unreserved in preprocessor directives.</span></span> <span data-ttu-id="2ada7-3673">Await 运算符仅允许在其所在的保留的字方法或 lambda 表达式的正文中。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3673">The await operator is only allowed in the body of a method or lambda expressions where it is a reserved word.</span></span> <span data-ttu-id="2ada7-3674">在最近的封闭方法或 lambda，await 表达式可能不会发生的表体内`Catch`或`Finally`块中，也不内部的正文`SyncLock`语句，也不在查询表达式。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3674">Within the immediately enclosing method or lambda, an await expression may not occur inside the body of a `Catch` or `Finally` block, nor inside the body of a `SyncLock` statement, nor inside a query expression.</span></span>

<span data-ttu-id="2ada7-3675">Await 运算符采用单个表达式，其必须分类为一个值，其类型必须是*awaitable*类型，或`Object`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3675">The await operator takes a single expression which must be classified as a value and whose type must be an *awaitable* type, or `Object`.</span></span> <span data-ttu-id="2ada7-3676">如果其类型为`Object`然后所有处理都延迟到运行时。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3676">If its type is `Object` then all processing is deferred until run-time.</span></span> <span data-ttu-id="2ada7-3677">一种类型`C`说是可满足以下所有条件时，可等待操作：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3677">A type `C` is said to be awaitable if all of the following are true:</span></span>

* <span data-ttu-id="2ada7-3678">`C` 包含名为可访问的实例或扩展方法`GetAwaiter`其中没有参数，这会返回一些类型`E`;</span><span class="sxs-lookup"><span data-stu-id="2ada7-3678">`C` contains an accessible instance or extension method named `GetAwaiter` which has no arguments and which returns some type `E`;</span></span>

* <span data-ttu-id="2ada7-3679">`E` 包含可读的实例或扩展属性，将名为`IsCompleted`它不采用任何参数，并具有类型布尔值;</span><span class="sxs-lookup"><span data-stu-id="2ada7-3679">`E` contains a readable instance or extension property named `IsCompleted` which takes no arguments and has type Boolean;</span></span>

* <span data-ttu-id="2ada7-3680">`E` 包含可访问实例或扩展的方法名为`GetResult`这不采用任何参数;</span><span class="sxs-lookup"><span data-stu-id="2ada7-3680">`E` contains an accessible instance or extension method named `GetResult` which takes no arguments;</span></span>

* <span data-ttu-id="2ada7-3681">`E` 实现`System.Runtime.CompilerServices.INotifyCompletion`或`ICriticalNotifyCompletion`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3681">`E` implements either `System.Runtime.CompilerServices.INotifyCompletion` or `ICriticalNotifyCompletion`.</span></span>

<span data-ttu-id="2ada7-3682">如果`GetResult`已`Sub`，则 await 表达式分类为 void。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3682">If `GetResult` was a `Sub`, then the await expression is classified as void.</span></span> <span data-ttu-id="2ada7-3683">否则为 await 表达式分类为一个值，且其类型为的返回类型`GetResult`方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3683">Otherwise, the await expression is classified as a value and its type is the return type of the `GetResult` method.</span></span>

<span data-ttu-id="2ada7-3684">下面是类的可以处于等待状态的示例：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3684">Here is an example of a class that can be awaited:</span></span>

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

<span data-ttu-id="2ada7-3685">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="2ada7-3685">__Note.__</span></span> <span data-ttu-id="2ada7-3686">建议将库作者，以遵循模式，它们将调用同一个延续委托`SynchronizationContext`作为其`OnCompleted`本身上调用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3686">Library authors are recommended to follow the pattern that they invoke the continuation delegate on the same `SynchronizationContext` as their `OnCompleted` was itself invoked on.</span></span> <span data-ttu-id="2ada7-3687">此外，恢复委托不应执行以同步方式内`OnCompleted`方法因为这可能会导致堆栈溢出： 相反，应进行委托排队以便后续执行。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3687">Also, the resumption delegate should not be executed synchronously within the `OnCompleted` method since that can lead to stack overflow: instead, the delegate should be queued for subsequent execution.</span></span>

<span data-ttu-id="2ada7-3688">当控制流到达`Await`运算符，行为如下所示。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3688">When control flow reaches an `Await` operator, behavior is as follows.</span></span>

1.  <span data-ttu-id="2ada7-3689">`GetAwaiter`调用 await 操作数的方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3689">The `GetAwaiter` method of the await operand is invoked.</span></span> <span data-ttu-id="2ada7-3690">此调用的结果称为*awaiter*。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3690">The result of this invocation is termed the *awaiter*.</span></span>

2.  <span data-ttu-id="2ada7-3691">Awaiter 的`IsCompleted`检索属性。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3691">The awaiter's `IsCompleted` property is retrieved.</span></span> <span data-ttu-id="2ada7-3692">如果结果为 true 则：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3692">If the result is true then:</span></span>

    21. <span data-ttu-id="2ada7-3693">`GetResult` Awaiter 的方法调用。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3693">The `GetResult` method of the awaiter is invoked.</span></span> <span data-ttu-id="2ada7-3694">如果`GetResult`为一个函数，则 await 表达式的值是此函数的返回值。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3694">If `GetResult` was a function, then the value of the await expression is the return value of this function.</span></span>

3.  <span data-ttu-id="2ada7-3695">如果不是这样的 IsCompleted 属性然后：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3695">If the IsCompleted property isn't true then:</span></span>

    31. <span data-ttu-id="2ada7-3696">任一`ICriticalNotifyCompletion.UnsafeOnCompleted`等待程序上调用 (如果 awaiter 的类型`E`实现`ICriticalNotifyCompletion`) 或`INotifyCompletion.OnCompleted`（否则为）。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3696">Either `ICriticalNotifyCompletion.UnsafeOnCompleted` is invoked on the awaiter (if the awaiter's type `E` implements `ICriticalNotifyCompletion`) or `INotifyCompletion.OnCompleted` (otherwise).</span></span> <span data-ttu-id="2ada7-3697">在这种情况下，它将传递*恢复委托*与异步方法的当前实例相关联。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3697">In both cases it passes a *resumption delegate* associated with the current instance of the async method.</span></span>

    32. <span data-ttu-id="2ada7-3698">当前的异步方法实例的控制点已挂起，但控制流中的简历*当前调用方*(部分中定义[异步方法](statements.md#async-methods))。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3698">The control point of the current async method instance is suspended, and control flow resumes in the *current caller* (defined in Section [Async Methods](statements.md#async-methods)).</span></span>

    33. <span data-ttu-id="2ada7-3699">当调用恢复委托时更高版本</span><span class="sxs-lookup"><span data-stu-id="2ada7-3699">If later the resumption delegate is invoked,</span></span>

        331. <span data-ttu-id="2ada7-3700">首先还原恢复委托`System.Threading.Thread.CurrentThread.ExecutionContext`到它时为`OnCompleted`调用，</span><span class="sxs-lookup"><span data-stu-id="2ada7-3700">the resumption delegate first restores `System.Threading.Thread.CurrentThread.ExecutionContext` to what it was at the time `OnCompleted` was called,</span></span>
        332. <span data-ttu-id="2ada7-3701">然后它会继续在异步方法实例的控制点控制流 (请参阅部分[异步方法](statements.md#async-methods))，</span><span class="sxs-lookup"><span data-stu-id="2ada7-3701">then it resumes control flow at the control point of the async method instance (see Section [Async Methods](statements.md#async-methods)),</span></span>
        333. <span data-ttu-id="2ada7-3702">它将调用其中`GetResult`awaiter，如上面的 2.1 中所示的方法。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3702">where it calls the `GetResult` method of the awaiter, as in 2.1 above.</span></span>

<span data-ttu-id="2ada7-3703">如果 await 操作数具有类型为 Object，此行为将推迟到运行时：</span><span class="sxs-lookup"><span data-stu-id="2ada7-3703">If the await operand has type Object, then this behavior is deferred until runtime:</span></span>

- <span data-ttu-id="2ada7-3704">通过调用不带任何参数; GetAwaiter() 完成步骤 1它可能会因此绑定在运行时实例方法采用可选参数。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3704">Step 1 is accomplished by calling GetAwaiter() with no arguments; it may therefore bind at runtime to instance methods which take optional parameters.</span></span>
- <span data-ttu-id="2ada7-3705">第 2 步是通过检索 IsCompleted() 属性不带任何参数，以及通过尝试的内部函数转换为布尔值来完成。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3705">Step 2 is accomplished by retrieving the IsCompleted() property with no arguments, and by attempting an intrinsic conversion to Boolean.</span></span>
- <span data-ttu-id="2ada7-3706">步骤 3.a 通过尝试`TryCast(awaiter, ICriticalNotifyCompletion)`，并且如果此失败然后`DirectCast(awaiter, INotifyCompletion)`。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3706">Step 3.a is accomplished by attempting `TryCast(awaiter, ICriticalNotifyCompletion)`, and if this fails then `DirectCast(awaiter, INotifyCompletion)`.</span></span>

<span data-ttu-id="2ada7-3707">只可以一次调用恢复委托 3.a 中传递。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3707">The resumption delegate passed in 3.a may only be invoked once.</span></span> <span data-ttu-id="2ada7-3708">如果多次调用它，该行为不确定。</span><span class="sxs-lookup"><span data-stu-id="2ada7-3708">If it is invoked more than once, the behavior is undefined.</span></span>

