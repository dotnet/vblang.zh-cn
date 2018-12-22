# <a name="conversions"></a><span data-ttu-id="ac1c1-101">转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-101">Conversions</span></span>

<span data-ttu-id="ac1c1-102">转换是从一种类型的值更改为另一个的过程。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-102">Conversion is the process of changing a value from one type to another.</span></span> <span data-ttu-id="ac1c1-103">例如，类型的值`Integer`可转换为类型的值`Double`，或类型的值`Derived`可转换为类型的值`Base`、 假设`Base`和`Derived`类和`Derived`继承自`Base`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-103">For example, a value of type `Integer` can be converted to a value of type `Double`, or a value of type `Derived` can be converted to a value of type `Base`, assuming that `Base` and `Derived` are both classes and `Derived` inherits from `Base`.</span></span> <span data-ttu-id="ac1c1-104">转换可能不需要值本身进行更改 （后者如示例所示），或者它们可能要求 （如前一示例中） 的值表示形式中的重大更改。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-104">Conversions may not require the value itself to change (as in the latter example), or they may require significant changes in the value representation (as in the former example).</span></span>

<span data-ttu-id="ac1c1-105">转换可能会扩大或收缩。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-105">Conversions may either be widening or narrowing.</span></span> <span data-ttu-id="ac1c1-106">一个*扩大转换*是从一种类型转换为另一种类型的值域必须是至少一样大，如果不大于，原始类型的值的域。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-106">A *widening conversion* is a conversion from a type to another type whose value domain is at least as big, if not bigger, than the original type's value domain.</span></span> <span data-ttu-id="ac1c1-107">扩大转换应永远不会失败。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-107">Widening conversions should never fail.</span></span> <span data-ttu-id="ac1c1-108">一个*收缩转换*从一种类型转换为另一种类型的值域必须是小于原始类型的值域或足够不相关的额外必须格外小心何时 （适用于中执行转换示例中，当从转换`Integer`到`String`)。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-108">A *narrowing conversion* is a conversion from a type to another type whose value domain is either smaller than the original type's value domain or sufficiently unrelated that extra care must be taken when doing the conversion (for example, when converting from `Integer` to `String`).</span></span> <span data-ttu-id="ac1c1-109">收缩转换，这可能需要将信息丢失，可能会失败。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-109">Narrowing conversions, which may entail loss of information, can fail.</span></span>

<span data-ttu-id="ac1c1-110">标识转换 （即从类型转换到其自身） 和默认值的转换 (即从转换`Nothing`) 的所有类型定义。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-110">The identity conversion (i.e. a conversion from a type to itself) and default value conversion (i.e. a conversion from `Nothing`) are defined for all types.</span></span>

## <a name="implicit-and-explicit-conversions"></a><span data-ttu-id="ac1c1-111">隐式转换和显式转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-111">Implicit and Explicit Conversions</span></span>

<span data-ttu-id="ac1c1-112">转换可以是*隐式*或*显式*。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-112">Conversions can be either *implicit* or *explicit*.</span></span> <span data-ttu-id="ac1c1-113">而无需任何特殊语法进行隐式转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-113">Implicit conversions occur without any special syntax.</span></span> <span data-ttu-id="ac1c1-114">以下是隐式转换的示例`Integer`值设为`Long`值：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-114">The following is an example of implicit conversion of an `Integer` value to a `Long` value:</span></span>

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

显式转换，但是，需要强制转换运算符。 尝试执行操作而无需强制转换运算符的值的显式转换会导致编译时错误。 <span data-ttu-id="ac1c1-117">下面的示例使用的显式转换来转换`Long`值设为`Integer`值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-117">The following example uses an explicit conversion to convert a `Long` value to an `Integer` value.</span></span>

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

隐式转换集取决于编译环境和`Option Strict`语句。 如果正在使用严格的语义，仅进行扩大转换可能会发生隐式。 <span data-ttu-id="ac1c1-120">如果要使用宽松的语义，所有的扩大转换和收缩转换 （即，所有转换） 可能会发生隐式。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-120">If permissive semantics are being used, all widening and narrowing conversions (in other words, all conversions) may occur implicitly.</span></span>

## <a name="boolean-conversions"></a><span data-ttu-id="ac1c1-121">布尔转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-121">Boolean Conversions</span></span>

<span data-ttu-id="ac1c1-122">尽管`Boolean`不是数值类型，但也有收缩转换与数值类型，就好像枚举的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-122">Although `Boolean` is not a numeric type, it does have narrowing conversions to and from the numeric types as if it were an enumerated type.</span></span> <span data-ttu-id="ac1c1-123">文字`True`将转换为文本`255`为`Byte`，`65535`有关`UShort`，`4294967295`的`UInteger`，`18446744073709551615`为`ULong`，和表达式`-1`的`SByte`， `Short`， `Integer`， `Long`， `Decimal`， `Single`，和`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-123">The literal `True` converts to the literal `255` for `Byte`, `65535` for `UShort`, `4294967295` for `UInteger`, `18446744073709551615` for `ULong`, and to the expression `-1` for `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, and `Double`.</span></span> <span data-ttu-id="ac1c1-124">文字`False`将转换为文本`0`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-124">The literal `False` converts to the literal `0`.</span></span> <span data-ttu-id="ac1c1-125">零数字值将转换为文本`False`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-125">A zero numeric value converts to the literal `False`.</span></span> <span data-ttu-id="ac1c1-126">所有其他数字值转换为文本`True`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-126">All other numeric values convert to the literal `True`.</span></span>

<span data-ttu-id="ac1c1-127">没有从布尔值转换为字符串，转换为收缩`System.Boolean.TrueString`或`System.Boolean.FalseString`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-127">There is a narrowing conversion from Boolean to String, converting to either `System.Boolean.TrueString` or `System.Boolean.FalseString`.</span></span> <span data-ttu-id="ac1c1-128">此外，还有从收缩转换`String`到`Boolean`： 如果该字符串等于`TrueString`或`FalseString`(当前区域性中不区分大小写) 然后使用适当的值; 否则它尝试将字符串分析为数值类型 (在十六进制或八进制如果可能，否则为浮点形式)，并使用上述规则;否则将引发`System.InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-128">There is also a narrowing conversion from `String` to `Boolean`: if the string was equal to `TrueString` or `FalseString` (in the current culture, case-insensitively) then it uses the appropriate value; otherwise it attempts to parse the string as a numeric type (in hex or octal if possible, otherwise as a float) and uses the above rules; otherwise it throws `System.InvalidCastException`.</span></span>

## <a name="numeric-conversions"></a><span data-ttu-id="ac1c1-129">数值转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-129">Numeric Conversions</span></span>

<span data-ttu-id="ac1c1-130">数值转换的类型之间存在`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`，`Single`和`Double`，和所有枚举的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-130">Numeric conversions exist between the types `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` and `Double`, and all enumerated types.</span></span> <span data-ttu-id="ac1c1-131">当正在转换时，就好像它们的基础类型将被视为枚举的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-131">When being converted, enumerated types are treated as if they were their underlying types.</span></span> <span data-ttu-id="ac1c1-132">转换为枚举类型时，源值不需要遵守的一组值的枚举类型中定义。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-132">When converting to an enumerated type, the source value is not required to conform to the set of values defined in the enumerated type.</span></span> <span data-ttu-id="ac1c1-133">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-133">For example:</span></span>

```vb
Enum Values
    One
    Two
    Three
End Enum

Module Test
    Sub Main()
        Dim x As Integer = 5

        ' OK, even though there is no enumerated value for 5.
        Dim y As Values = CType(x, Values)
    End Sub
End Module
```

<span data-ttu-id="ac1c1-134">按如下所示，将数值转换处理在运行时：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-134">Numeric conversions are processed at run-time as follows:</span></span>

* <span data-ttu-id="ac1c1-135">从数值类型转换为更广泛的数值类型，值只被转换为更广泛的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-135">For a conversion from a numeric type to a wider numeric type, the value is simply converted to the wider type.</span></span> <span data-ttu-id="ac1c1-136">从转换`UInteger`， `Integer`， `ULong`， `Long`，或`Decimal`到`Single`或者`Double`舍入为最接近`Single`或`Double`值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-136">Conversions from `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal` to `Single` or `Double` are rounded to the nearest `Single` or `Double` value.</span></span> <span data-ttu-id="ac1c1-137">虽然此转换可能会导致精度损失，它将永远不会导致丢失量值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-137">While this conversion may cause a loss of precision, it will never cause a loss of magnitude.</span></span>

* <span data-ttu-id="ac1c1-138">对于从一种整型类型转换到另一个整型类型，或从`Single`， `Double`，或`Decimal`为整型类型，结果取决于整数溢出检查是上：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-138">For a conversion from an integral type to another integral type, or from `Single`, `Double`, or `Decimal` to an integral type, the result depends on whether integer overflow checking is on:</span></span>

  <span data-ttu-id="ac1c1-139">*如果要检查整数溢出：*</span><span class="sxs-lookup"><span data-stu-id="ac1c1-139">*If integer overflow is being checked:*</span></span>

  * <span data-ttu-id="ac1c1-140">如果源是一种整型类型，转换会成功，如果源自变量被目标类型的范围内。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-140">If the source is an integral type, the conversion succeeds if the source argument is within the range of the destination type.</span></span> <span data-ttu-id="ac1c1-141">转换会引发`System.OverflowException`异常如果源自变量被目标类型的范围之外。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-141">The conversion throws a `System.OverflowException` exception if the source argument is outside the range of the destination type.</span></span>

  * <span data-ttu-id="ac1c1-142">如果源是`Single`， `Double`，或`Decimal`、 向上或向下最接近的整数值，舍入的源值和此整数值将成为转换的结果。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-142">If the source is `Single`, `Double`, or `Decimal`, the source value is rounded up or down to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="ac1c1-143">如果源值同样接近两个整数值，则会将值舍入到最低有效位数位置中具有偶数数目的值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-143">If the source value is equally close to two integral values, the value is rounded to the value that has an even number in the least significant digit position.</span></span> <span data-ttu-id="ac1c1-144">如果生成的整数值超出目标类型的范围`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-144">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` exception is thrown.</span></span>

  <span data-ttu-id="ac1c1-145">*如果未选中整数溢出：*</span><span class="sxs-lookup"><span data-stu-id="ac1c1-145">*If integer overflow is not being checked:*</span></span>

  * <span data-ttu-id="ac1c1-146">如果源是一种整型类型，转换始终成功，并且只包含放弃源值的最高有效位。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-146">If the source is an integral type, the conversion always succeeds and simply consists of discarding the most significant bits of the source value.</span></span>

  * <span data-ttu-id="ac1c1-147">如果源是`Single`， `Double`，或`Decimal`，转换始终成功，只包含舍入到最接近的整数值的源值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-147">If the source is `Single`, `Double`, or `Decimal`, the conversion always succeeds and simply consists of rounding the source value towards the nearest integral value.</span></span> <span data-ttu-id="ac1c1-148">如果源值同样接近两个整数值，该值是始终舍入到最低有效位数位置中具有偶数数目的值中。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-148">If the source value is equally close to two integral values, the value is always rounded to the value that has an even number in the least significant digit position.</span></span>

* <span data-ttu-id="ac1c1-149">对于从`Double`到`Single`，则`Double`值舍入为最接近`Single`值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-149">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="ac1c1-150">如果`Double`值因过小而无法表示为`Single`，结果将成为正零或负零。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-150">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="ac1c1-151">如果`Double`该值太大而无法表示为`Single`，其结果成为正无穷或负无穷大。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-151">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="ac1c1-152">如果`Double`值是`NaN`，结果也是`NaN`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-152">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

* <span data-ttu-id="ac1c1-153">对于从`Single`或`Double`到`Decimal`，源值转换为`Decimal`表示形式和舍入为最接近的数字 28 位小数后根据需要。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-153">For a conversion from `Single` or `Double` to `Decimal`, the source value is converted to `Decimal` representation and rounded to the nearest number after the 28th decimal place if required.</span></span> <span data-ttu-id="ac1c1-154">如果源值就太小，无法表示为`Decimal`，结果则为零。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-154">If the source value is too small to represent as a `Decimal`, the result becomes zero.</span></span> <span data-ttu-id="ac1c1-155">如果源值为`NaN`，无穷大或太大而无法表示为`Decimal`、`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-155">If the source value is `NaN`, infinity, or too large to represent as a `Decimal`, a `System.OverflowException` exception is thrown.</span></span>

* <span data-ttu-id="ac1c1-156">对于从`Double`到`Single`，则`Double`值舍入为最接近`Single`值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-156">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="ac1c1-157">如果`Double`值因过小而无法表示为`Single`，结果将成为正零或负零。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-157">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="ac1c1-158">如果`Double`该值太大而无法表示为`Single`，其结果成为正无穷或负无穷大。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-158">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="ac1c1-159">如果`Double`值是`NaN`，结果也是`NaN`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-159">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

## <a name="reference-conversions"></a><span data-ttu-id="ac1c1-160">引用转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-160">Reference Conversions</span></span>

<span data-ttu-id="ac1c1-161">引用类型可能会转换为基类型，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-161">Reference types may be converted to a base type, and vice versa.</span></span> <span data-ttu-id="ac1c1-162">如果要转换的值为 null 值、 派生的类型本身或深入派生的类型从基类型到派生程度更大的类型的转换才成功在运行时。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-162">Conversions from a base type to a more derived type only succeed at run time if the value being converted is a null value, the derived type itself, or a more derived type.</span></span>

<span data-ttu-id="ac1c1-163">可以将类和接口类型转换到和从任何接口类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-163">Class and interface types can be cast to and from any interface type.</span></span> <span data-ttu-id="ac1c1-164">一种类型和接口类型之间的转换才成功所涉及的实际类型具有继承或实现关系在运行时。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-164">Conversions between a type and an interface type only succeed at run time if the actual types involved have an inheritance or implementation relationship.</span></span> <span data-ttu-id="ac1c1-165">因为接口类型将始终包含派生类型的实例`Object`，接口类型也始终与转换`Object`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-165">Because an interface type will always contain an instance of a type that derives from `Object`, an interface type can also always be cast to and from `Object`.</span></span>

<span data-ttu-id="ac1c1-166">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-166">__Note.__</span></span> <span data-ttu-id="ac1c1-167">它不是错误转换`NotInheritable`类与接口未实现表示 COM 类的类可能具有接口实现之前不知道是否因为运行时。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-167">It is not an error to convert a `NotInheritable` classes to and from interfaces that it does not implement because classes that represent COM classes may have interface implementations that are not known until run time.</span></span> 

<span data-ttu-id="ac1c1-168">如果在运行时，将引用转换失败`System.InvalidCastException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-168">If a reference conversion fails at run time, a `System.InvalidCastException` exception is thrown.</span></span>

### <a name="reference-variance-conversions"></a><span data-ttu-id="ac1c1-169">引用的变体转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-169">Reference Variance Conversions</span></span>

<span data-ttu-id="ac1c1-170">泛型接口或委托可能具有 variant 类型参数，允许兼容的类型的变体之间的转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-170">Generic interfaces or delegates may have variant type parameters that allow conversions between compatible variants of the type.</span></span> <span data-ttu-id="ac1c1-171">因此，在运行时从类类型或接口类型转换为是变体与它继承自或实现一个接口类型兼容的接口类型将会成功。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-171">Therefore, at runtime a conversion from a class type or an interface type to an interface type that is variant compatible with an interface type it inherits from or implements will succeed.</span></span> <span data-ttu-id="ac1c1-172">同样，委托类型可以转换为，并且从兼容的变体委托类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-172">Similarly, delegate types can be cast to and from variant compatible delegate types.</span></span> <span data-ttu-id="ac1c1-173">例如，委托类型</span><span class="sxs-lookup"><span data-stu-id="ac1c1-173">For example, the delegate type</span></span>

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

<span data-ttu-id="ac1c1-174">将允许从转换`F(Of Object, Integer)`到`F(Of String, Integer)`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-174">would allow a conversion from `F(Of Object, Integer)` to `F(Of String, Integer)`.</span></span> <span data-ttu-id="ac1c1-175">它是一个委托`F`此方法采用`Object`可以安全地使用委托`F`此方法采用`String`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-175">That is, a delegate `F` which takes `Object` may be safely used as a delegate `F` which takes `String`.</span></span> <span data-ttu-id="ac1c1-176">当调用委托时，目标方法将期望获得对象，而字符串是一个对象。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-176">When the delegate is invoked, the target method will be expecting an object, and a string is an object.</span></span>

<span data-ttu-id="ac1c1-177">泛型委托或接口类型`S(Of S1,...,Sn)`称为*变体兼容*泛型接口或委托类型与`T(Of T1,...,Tn)`如果：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-177">A generic delegate or interface type `S(Of S1,...,Sn)` is said to be *variant compatible* with a generic interface or delegate type `T(Of T1,...,Tn)` if:</span></span>

* <span data-ttu-id="ac1c1-178">`S` 并`T`同时从同一个泛型类型构造`U(Of U1,...,Un)`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-178">`S` and `T` are both constructed from the same generic type `U(Of U1,...,Un)`.</span></span>

* <span data-ttu-id="ac1c1-179">每个类型形参`Ux`:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-179">For each type parameter `Ux`:</span></span>

  * <span data-ttu-id="ac1c1-180">如果类型参数已声明而无需再方差`Sx`和`Tx`必须是同一类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-180">If the type parameter was declared without variance then `Sx` and `Tx` must be the same type.</span></span>

  * <span data-ttu-id="ac1c1-181">如果已声明的类型参数`In`必须有扩大标识、 默认值、 引用、 数组或类型参数转换从`Sx`到`Tx`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-181">If the type parameter was declared `In` then there must be a widening identity, default, reference, array, or type parameter conversion from `Sx` to `Tx`.</span></span>

  * <span data-ttu-id="ac1c1-182">如果已声明的类型参数`Out`必须有扩大标识、 默认值、 引用、 数组或类型参数转换从`Tx`到`Sx`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-182">If the type parameter was declared `Out` then there must be a widening identity, default, reference, array, or type parameter conversion from `Tx` to `Sx`.</span></span>

<span data-ttu-id="ac1c1-183">到泛型接口使用 variant 类型参数的转换从一个类，如果类实现多个变体兼容接口时转换不明确，如果不存在非变体转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-183">When converting from a class to a generic interface with variant type parameters, if the class implements more than one variant compatible interface the conversion is ambiguous if there is not a non-variant conversion.</span></span> <span data-ttu-id="ac1c1-184">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-184">For example:</span></span>

```vb
Class Base
End Class

Class Derived1
    Inherits Base
End Class

Class Derived2
    Inherits Base
End Class

Class OneAndTwo
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Class BaseAndOneAndTwo
    Implements IEnumerable(Of Base)
    Implements IEnumerable(Of Derived1)
    Implements IEnumerable(Of Derived2)
End Class

Module Test
    Sub Main()
        ' Error: conversion is ambiguous
        Dim x As IEnumerable(Of Base) = New OneAndTwo()

        ' OK, will pick up the direct implementation of IEnumerable(Of Base)
        Dim y as IEnumerable(Of Base) = New BaseAndOneAndTwo()
    End Sub
End Module
```

### <a name="anonymous-delegate-conversions"></a><span data-ttu-id="ac1c1-185">匿名委托转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-185">Anonymous Delegate Conversions</span></span>

<span data-ttu-id="ac1c1-186">表达式分类为 lambda 方法重新分类为值的上下文中时其中不存在目标的类型 (例如， `Dim x = Function(a As Integer, b As Integer) a + b`)，或者其中的目标类型不是委托类型，所生成的表达式的类型是匿名委托类型等效于 lambda 方法的签名。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-186">When an expression classified as a lambda method is reclassified as a value in a context where there is no target type (for example, `Dim x = Function(a As Integer, b As Integer) a + b`), or where the target type is not a delegate type, the type of the resulting expression is an anonymous delegate type equivalent to the signature of the lambda method.</span></span> <span data-ttu-id="ac1c1-187">此匿名委托类型具有到任何兼容的委托类型的转换： 兼容的委托类型是可以使用匿名委托类型的委托创建表达式创建任何委托类型`Invoke`方法作为参数。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-187">This anonymous delegate type has a conversion to any compatible delegate type: a compatible delegate type is any delegate type that can be created using a delegate creation expression with the anonymous delegate type's `Invoke` method as a parameter.</span></span> <span data-ttu-id="ac1c1-188">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-188">For example:</span></span>

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

<span data-ttu-id="ac1c1-189">请注意，类型`System.Delegate`和`System.MulticastDelegate`无法自行被认为是委托类型 （即使从它们继承所有委托类型）。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-189">Note that the types `System.Delegate` and `System.MulticastDelegate` are not themselves considered delegate types (even though all delegate types inherit from them).</span></span> <span data-ttu-id="ac1c1-190">另请注意，从匿名委托类型转换为兼容的委托类型不是引用转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-190">Also, note that the conversion from anonymous delegate type to a compatible delegate type is not a reference conversion.</span></span>

## <a name="array-conversions"></a><span data-ttu-id="ac1c1-191">数组转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-191">Array Conversions</span></span>

<span data-ttu-id="ac1c1-192">数组，因此它们是引用类型定义的转换，除了几个特殊的转换存在数组。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-192">Besides the conversions that are defined on arrays by virtue of the fact that they are reference types, several special conversions exist for arrays.</span></span>

<span data-ttu-id="ac1c1-193">任何两个类型为`A`和`B`，如果它们是引用类型或类型参数不知道为值类型，并且如果`A`具有的引用，则数组或类型参数转换到`B`，数组中的已存在的转换类型`A`类型的数组到`B`使用相同的排名。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-193">For any two types `A` and `B`, if they are both reference types or type parameters not known to be value types, and if `A` has a reference, array, or type parameter conversion to `B`, a conversion exists from an array of type `A` to an array of type `B` with the same rank.</span></span> <span data-ttu-id="ac1c1-194">这种关系称为*数组协方差*。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-194">This relationship is known as *array covariance*.</span></span> <span data-ttu-id="ac1c1-195">数组协方差尤其意味着，其元素类型是数组的元素`B`实际上可能是其元素类型是数组的元素`A`，前提是这两`A`和`B`是引用类型和该`B`具有引用转换或数组转换为`A`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-195">Array covariance in particular means that an element of an array whose element type is `B` may actually be an element of an array whose element type is `A`, provided that both `A` and `B` are reference types and that `B` has a reference conversion or array conversion to `A`.</span></span> <span data-ttu-id="ac1c1-196">在下面的示例中，第二次调用`F`会导致`System.ArrayTypeMismatchException`因为实际的元素类型的引发异常`b`是`String`，而不`Object`:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-196">In the following example, the second invocation of `F` causes a `System.ArrayTypeMismatchException` exception to be thrown because the actual element type of `b` is `String`, not `Object`:</span></span>

```vb
Module Test
    Sub F(ByRef x As Object)
    End Sub

    Sub Main()
        Dim a(10) As Object
        Dim b() As Object = New String(10) {}
        F(a(0)) ' OK.
        F(b(1)) ' Not allowed: System.ArrayTypeMismatchException.
   End Sub
End Module
```

<span data-ttu-id="ac1c1-197">数组协方差，由于引用类型数组的元素赋值中包括运行时检查，以确保分配给数组元素的值实际上是允许的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-197">Because of array covariance, assignments to elements of reference type arrays include a run-time check that ensures that the value being assigned to the array element is actually of a permitted type.</span></span>

```vb
Module Test
    Sub Fill(array() As Object, index As Integer, count As Integer, _
            value As Object)
        Dim i As Integer

        For i = index To (index + count) - 1
            array(i) = value
        Next i
    End Sub

    Sub Main()
        Dim strings(100) As String

        Fill(strings, 0, 101, "Undefined")
        Fill(strings, 0, 10, Nothing)
        Fill(strings, 91, 10, 0)
    End Sub
End Module
```

<span data-ttu-id="ac1c1-198">在此示例中，分配给`array(i)`方法中`Fill`隐式包括确保该变量所引用的对象的运行时检查`value`是`Nothing`或与兼容的类型的实例数组的实际元素类型`array`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-198">In this example, the assignment to `array(i)` in method `Fill` implicitly includes a run-time check that ensures that the object referenced by the variable `value` is either `Nothing` or an instance of a type that is compatible with the actual element type of array `array`.</span></span> <span data-ttu-id="ac1c1-199">在方法中`Main`前, 两个方法的调用`Fill`成功，但第三个调用会导致`System.ArrayTypeMismatchException`异常引发时执行的第一个分配到`array(i)`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-199">In method `Main`, the first two invocations of method `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` exception to be thrown upon executing the first assignment to `array(i)`.</span></span> <span data-ttu-id="ac1c1-200">发生异常的原因`Integer`不能存储在`String`数组。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-200">The exception occurs because an `Integer` cannot be stored in a `String` array.</span></span>

<span data-ttu-id="ac1c1-201">如果其中一个数组元素类型是其类型证明是在运行时，值类型的类型参数`System.InvalidCastException`将引发异常。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-201">If one of the array element types is a type parameter whose type turns out to be a value type at runtime, a `System.InvalidCastException` exception will be thrown.</span></span> <span data-ttu-id="ac1c1-202">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-202">For example:</span></span>

```vb
Module Test
    Sub F(Of T As U, U)(x() As T)
        Dim y() As U = x
    End Sub

    Sub Main()
        ' F will throw an exception because Integer() cannot be
        ' converted to Object()
        F(New Integer() { 1, 2, 3 })
    End Sub
End Module
```

<span data-ttu-id="ac1c1-203">枚举类型的数组之间还存在转换和枚举类型的数组的基础类型或具有相同的基础类型，另一个枚举类型的数组，前提是数组具有相同的排名。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-203">Conversions also exist between an array of an enumerated type and an array of the enumerated type's underlying type or an array of another enumerated type with the same underlying type, provided the arrays have the same rank.</span></span>

```vb
Enum Color As Byte
    Red
    Green
    Blue
End Enum

Module Test
    Sub Main()
        Dim a(10) As Color
        Dim b() As Integer
        Dim c() As Byte

        b = a    ' Error: Integer is not the underlying type of Color
        c = a    ' OK
        a = c    ' OK
    End Sub
End Module
```

<span data-ttu-id="ac1c1-204">在此示例中，数组`Color`转换到数组中的`Byte`，`Color`的基础类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-204">In this example, an array of `Color` is converted to and from an array of `Byte`, `Color`'s underlying type.</span></span> <span data-ttu-id="ac1c1-205">转换为数组`Integer`，但是，将出现错误因为`Integer`的基础类型不是`Color`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-205">The conversion to an array of `Integer`, however, will be an error because `Integer` is not the underlying type of `Color`.</span></span>

<span data-ttu-id="ac1c1-206">级别 1 类型数组`A()`还具有到集合接口类型的数组转换`IList(Of B)`， `IReadOnlyList(Of B)`， `ICollection(Of B)`，`IReadOnlyCollection(Of B)`并`IEnumerable(Of B)`中找到`System.Collections.Generic`，但前提是以下之一为 true:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-206">A rank-1 array of type `A()` also has an array conversion to the collection interface types `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` and `IEnumerable(Of B)` found in `System.Collections.Generic`, so long as one of the following is true:</span></span>

- <span data-ttu-id="ac1c1-207">`A` 并`B`是否都引用类型或未知值类型; 的类型参数和`A`扩大引用、 数组或类型参数转换为`B`; 或</span><span class="sxs-lookup"><span data-stu-id="ac1c1-207">`A` and `B` are both reference types or type parameters not known to be value types; and `A` has a widening reference, array or type parameter conversion to `B`; or</span></span>
- <span data-ttu-id="ac1c1-208">`A` 和`B`是相同的基础类型; 这两种枚举的类型或</span><span class="sxs-lookup"><span data-stu-id="ac1c1-208">`A` and `B` are both enumerated types of the same underlying type; or</span></span>
- <span data-ttu-id="ac1c1-209">之一`A`和`B`是一个枚举的类型，另一个是其基础类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-209">one of `A` and `B` is an enumerated type, and the other is its underlying type.</span></span>

<span data-ttu-id="ac1c1-210">任何级别的类型的任何数组还具有到非泛型集合接口类型的数组转换`IList`，`ICollection`并`IEnumerable`中找到`System.Collections`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-210">Any array of type A with any rank also has an array conversion to the non-generic collection interface types `IList`, `ICollection` and `IEnumerable` found in `System.Collections`.</span></span>

<span data-ttu-id="ac1c1-211">可以循环访问由此产生的接口使用`For Each`，或通过调用`GetEnumerator`直接方法。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-211">It is possible to iterate over the resulting interfaces using `For Each`, or through invoking the `GetEnumerator` methods directly.</span></span> <span data-ttu-id="ac1c1-212">在级别 1 阵列的情况下转换的泛型或非泛型窗体`IList`或`ICollection`，还有可能要按索引获取元素。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-212">In the case of rank-1 arrays converted generic or non-generic forms of `IList` or `ICollection`, it is also possible to get elements by index.</span></span> <span data-ttu-id="ac1c1-213">对于级别 1 数组转换为泛型或非泛型形式的`IList`，则还可以按索引设置元素，遵循相同的运行时数组协方差检查上文所述。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-213">In the case of rank-1 arrays converted to generic or non-generic forms of `IList`, it is also possible to set elements by index, subject to the same runtime array covariance checks as described above.</span></span> <span data-ttu-id="ac1c1-214">所有其他接口方法的行为是不明确的 VB 语言规范;负责基础运行时。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-214">The behavior of all other interface methods is undefined by the VB language specification; it is up to the underlying runtime.</span></span>

## <a name="value-type-conversions"></a><span data-ttu-id="ac1c1-215">值类型转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-215">Value Type Conversions</span></span>

<span data-ttu-id="ac1c1-216">值类型值可以转换为其基引用类型或接口类型，它实现了通过一个称为进程之一*装箱*。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-216">A value type value can be converted to one of its base reference types or an interface type that it implements through a process called *boxing*.</span></span> <span data-ttu-id="ac1c1-217">值类型值进行装箱时，值被复制从何处到.NET Framework 堆上的位置。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-217">When a value type value is boxed, the value is copied from the location where it lives onto the .NET Framework heap.</span></span> <span data-ttu-id="ac1c1-218">对堆上的此位置的引用然后返回，并且可以存储在引用类型变量。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-218">A reference to this location on the heap is then returned and can be stored in a reference type variable.</span></span> <span data-ttu-id="ac1c1-219">此引用也称为*装箱*值类型的实例。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-219">This reference is also referred to as a *boxed* instance of the value type.</span></span> <span data-ttu-id="ac1c1-220">装箱的实例具有相同的语义而不是值类型是引用类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-220">The boxed instance has the same semantics as a reference type instead of a value type.</span></span>

<span data-ttu-id="ac1c1-221">装箱的值类型可以转换回其原始值类型通过名为的进程*取消装箱*。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-221">Boxed value types can be converted back to their original value type through a process called *unboxing*.</span></span> <span data-ttu-id="ac1c1-222">未装箱的装箱的值类型时，值从堆复制到变量位置中。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-222">When a boxed value type is unboxed, the value is copied from the heap into a variable location.</span></span> <span data-ttu-id="ac1c1-223">此后，它的行为就像它是值类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-223">From that point on, it behaves as if it was a value type.</span></span> <span data-ttu-id="ac1c1-224">取消装箱值类型，值必须为 null 值或值类型的实例。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-224">When unboxing a value type, the value must be a null value or an instance of the value type.</span></span> <span data-ttu-id="ac1c1-225">否则为`System.InvalidCastException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-225">Otherwise a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="ac1c1-226">如果值为枚举类型的实例，此值也可以是未装箱为枚举类型的基础类型或另一种枚举的类型具有相同的基础类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-226">If the value is an instance of an enumerated type, that value can also be unboxed to the enumerated type's underlying type or another enumerated type that has the same underlying type.</span></span> <span data-ttu-id="ac1c1-227">Null 值被视为文字像`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-227">A null value is treated as if it were the literal `Nothing`.</span></span>

<span data-ttu-id="ac1c1-228">若要支持可以为 null 值类型，值类型`System.Nullable(Of T)`被视为特殊时执行装箱和取消装箱。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-228">To support nullable value types well, the value type `System.Nullable(Of T)` is treated specially when doing boxing and unboxing.</span></span> <span data-ttu-id="ac1c1-229">装箱类型的值`Nullable(Of T)`类型的装箱的值会导致`T`如果的值`HasValue`属性是`True`，则值为`Nothing`如果的值`HasValue`属性是`False`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-229">Boxing a value of type `Nullable(Of T)` results in a boxed value of type `T` if the value's `HasValue` property is `True` or a value of `Nothing` if the value's `HasValue` property is `False`.</span></span> <span data-ttu-id="ac1c1-230">类型的值取消装箱`T`到`Nullable(Of T)`的实例会导致`Nullable(Of T)`其`Value`属性为装箱的值，它的`HasValue`属性是`True`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-230">Unboxing a value of type `T` to `Nullable(Of T)` results in an instance of `Nullable(Of T)` whose `Value` property is the boxed value and whose `HasValue` property is `True`.</span></span> <span data-ttu-id="ac1c1-231">值`Nothing`可以为取消装箱`Nullable(Of T)`任何`T`和它的产生的值`HasValue`属性是`False`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-231">The value `Nothing` can be unboxed to `Nullable(Of T)` for any `T` and results in a value whose `HasValue` property is `False`.</span></span> <span data-ttu-id="ac1c1-232">因为装箱的值类型的行为类似引用类型，它是可以创建多个引用为相同的值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-232">Because boxed value types behave like reference types, it is possible to create multiple references to the same value.</span></span> <span data-ttu-id="ac1c1-233">对于基元类型和枚举的类型，这是不相关因为这些类型的实例*不可变*。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-233">For the primitive types and enumerated types, this is irrelevant because instances of those types are *immutable*.</span></span> <span data-ttu-id="ac1c1-234">也就是说，不可能需要修改这些类型的装箱的实例，因此不能遵守这一事实有多个引用为相同的值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-234">That is, it is not possible to modify a boxed instance of those types, so it is not possible to observe the fact that there are multiple references to the same value.</span></span>

<span data-ttu-id="ac1c1-235">结构，但是，可能是可变是否可以访问它的实例变量，或它的方法或属性修改它的实例变量。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-235">Structures, on the other hand, may be mutable if its instance variables are accessible or if its methods or properties modify its instance variables.</span></span> <span data-ttu-id="ac1c1-236">如果对已装箱的结构的一个引用用于修改该结构，对已装箱的结构的所有引用将都看到更改。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-236">If one reference to a boxed structure is used to modify the structure, then all references to the boxed structure will see the change.</span></span> <span data-ttu-id="ac1c1-237">此结果可能是意外，因为当一个值，类型化为`Object`从一个位置复制到另一个装箱的值类型会自动将克隆而不是只让复制其引用堆。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-237">Because this result may be unexpected, when a value typed as `Object` is copied from one location to another boxed value types will automatically be cloned on the heap instead of merely having their references copied.</span></span> <span data-ttu-id="ac1c1-238">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-238">For example:</span></span>

```vb
Class Class1
    Public Value As Integer = 0
End Class

Structure Struct1
    Public Value As Integer
End Structure

Module Test
    Sub Main()
        Dim val1 As Object = New Struct1()
        Dim val2 As Object = val1

        val2.Value = 123

        Dim ref1 As Object = New Class1()
        Dim ref2 As Object = ref1

        ref2.Value = 123

        Console.WriteLine("Values: " & val1.Value & ", " & val2.Value)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

<span data-ttu-id="ac1c1-239">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-239">The output of the program is:</span></span>

```
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="ac1c1-240">本地变量的字段的赋值`val2`不会影响本地变量的字段`val1`因为时已装箱`Struct1`已分配给`val2`，已创建的值的副本。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-240">The assignment to the field of the local variable `val2` does not impact the field of the local variable `val1` because when the boxed `Struct1` was assigned to `val2`, a copy of the value was made.</span></span> <span data-ttu-id="ac1c1-241">与之相反，赋值`ref2.Value = 123`影响的对象，同时`ref1`和`ref2`的引用。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-241">In contrast, the assignment `ref2.Value = 123` affects the object that both `ref1` and `ref2` references.</span></span>

<span data-ttu-id="ac1c1-242">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-242">__Note.__</span></span> <span data-ttu-id="ac1c1-243">复制结构不是装箱结构类型化为`System.ValueType`因为无法将后期绑定的中断`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-243">Structure copying is not done for boxed structures typed as `System.ValueType` because it is not possible to late bind off of `System.ValueType`.</span></span>

<span data-ttu-id="ac1c1-244">没有已装箱的值类型将在赋值时复制规则的例外。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-244">There is one exception to the rule that boxed value types will be copied on assignment.</span></span> <span data-ttu-id="ac1c1-245">如果已装箱的值类型引用存储在另一种类型内，将不会复制内部引用。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-245">If a boxed value type reference is stored within another type, the inner reference will not be copied.</span></span> <span data-ttu-id="ac1c1-246">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-246">For example:</span></span>

```vb
Structure Struct1
    Public Value As Object
End Structure

Module Test
    Sub Main()
        Dim val1 As Struct1
        Dim val2 As Struct1

        val1.Value = New Struct1()
        val1.Value.Value = 10

        val2 = val1
        val2.Value.Value = 123
        Console.WriteLine("Values: " & val1.Value.Value & ", " & _
            val2.Value.Value)
    End Sub
End Module
```

<span data-ttu-id="ac1c1-247">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-247">The output of the program is:</span></span>

```
Values: 123, 123
```

<span data-ttu-id="ac1c1-248">这是因为复制值时不会复制内部装箱的值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-248">This is because the inner boxed value is not copied when the value is copied.</span></span> <span data-ttu-id="ac1c1-249">因此，两者`val1.Value`和`val2.Value`具有相同的装箱的值类型的引用。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-249">Thus, both `val1.Value` and `val2.Value` have a reference to the same boxed value type.</span></span>

<span data-ttu-id="ac1c1-250">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-250">__Note.__</span></span> <span data-ttu-id="ac1c1-251">这一事实，将不会复制内部装箱的值类型是一个限制的.net 类型系统-若要确保只要类型的值复制所有内部装箱的值类型`Object`复制是成本过高。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-251">The fact that inner boxed value types are not copied is a limitation of the .NET type system -- to ensure that all inner boxed value types were copied whenever a value of type `Object` was copied would be prohibitively expensive.</span></span>

<span data-ttu-id="ac1c1-252">为前面所述，装箱值类型只能是未为其原始类型装箱。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-252">As previously described, boxed value types can only be unboxed to their original type.</span></span> <span data-ttu-id="ac1c1-253">装箱的基元类型，但是时, 处理的特殊类型化为`Object`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-253">Boxed primitive types, however, are treated specially when typed as `Object`.</span></span> <span data-ttu-id="ac1c1-254">它们可以转换为它们都可以转换为任何其他基元类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-254">They can be converted to any other primitive type that they have a conversion to.</span></span> <span data-ttu-id="ac1c1-255">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-255">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

<span data-ttu-id="ac1c1-256">通常情况下，装箱`Integer`值`5`不可能到取消装箱`Byte`变量。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-256">Normally, the boxed `Integer` value `5` could not be unboxed into a `Byte` variable.</span></span> <span data-ttu-id="ac1c1-257">但是，由于`Integer`和`Byte`是基元类型和具有的转换，允许进行转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-257">However, because `Integer` and `Byte` are primitive types and have a conversion, the conversion is allowed.</span></span>

<span data-ttu-id="ac1c1-258">请务必注意，将值类型转换为一个接口是不是泛型参数约束为接口不同。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-258">It is important to note that converting a value type to an interface is different than a generic argument constrained to an interface.</span></span> <span data-ttu-id="ac1c1-259">访问接口成员，受约束的类型参数时 (或上调用方法`Object`)，装箱值类型转换为接口和接口成员访问时的表现不会出现。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-259">When accessing interface members on a constrained type parameter (or calling methods on `Object`), boxing does not occur as it does when a value type is converted to an interface and an interface member is accessed.</span></span> <span data-ttu-id="ac1c1-260">例如，假设一个接口`ICounter`包含一个方法，`Increment`这可用于修改值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-260">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="ac1c1-261">如果`ICounter`用作约束的实现`Increment`使用对变量的引用来调用方法的`Increment`对不是装箱副本调用：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-261">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, not a boxed copy:</span></span>

```vb
Interface ICounter
    Sub Increment()
    ReadOnly Property Value() As Integer
End Interface

Structure Counter
    Implements ICounter

    Dim _value As Integer

    Property Value() As Integer Implements ICounter.Value
        Get
            Return _value
        End Get
    End Property

    Sub Increment() Implements ICounter.Increment
       value += 1
    End Sub
End Structure

Module Test
      Sub Test(Of T As ICounter)(x As T)
         Console.WriteLine(x.value)
         x.Increment()                     ' Modify x
         Console.WriteLine(x.value)
         CType(x, ICounter).Increment()    ' Modify boxed copy of x
         Console.WriteLine(x.value)
      End Sub

      Sub Main()
         Dim x As Counter
         Test(x)
      End Sub
End Module
```

<span data-ttu-id="ac1c1-262">首次调用`Increment`修改变量中的值`x`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-262">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="ac1c1-263">这并不等同于第二次调用`Increment`，它修改的已装箱副本中的值`x`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-263">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="ac1c1-264">因此，程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-264">Thus, the output of the program is:</span></span>

```
0
1
1
```

### <a name="nullable-value-type-conversions"></a><span data-ttu-id="ac1c1-265">可以为 null 值的类型转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-265">Nullable Value Type Conversions</span></span>

<span data-ttu-id="ac1c1-266">值类型`T`与可以为 null 的类型版本的可以转换`T?`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-266">A value type `T` can convert to and from the nullable version of the type, `T?`.</span></span> <span data-ttu-id="ac1c1-267">从转换`T?`到`T`将引发`System.InvalidOperationException`异常，如果要转换的值为`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-267">The conversion from `T?` to `T` throws a `System.InvalidOperationException` exception if the value being converted is `Nothing`.</span></span> <span data-ttu-id="ac1c1-268">此外，`T?`转换为类型`S`如果`T`内部函数转换为`S`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-268">Also, `T?` has a conversion to a type `S` if `T` has an intrinsic conversion to `S`.</span></span> <span data-ttu-id="ac1c1-269">如果`S`为值类型，则之间存在以下内部函数转换`T?`和`S?`:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-269">And if `S` is a value type, then the following intrinsic conversions exist between `T?` and `S?`:</span></span>

* <span data-ttu-id="ac1c1-270">从同一分类 （缩小或扩大转换） 的转换`T?`到`S?`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-270">A conversion of the same classification (narrowing or widening) from `T?` to `S?`.</span></span>

* <span data-ttu-id="ac1c1-271">从同一分类 （缩小或扩大转换） 的转换`T`到`S?`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-271">A conversion of the same classification (narrowing or widening) from `T` to `S?`.</span></span>

* <span data-ttu-id="ac1c1-272">收缩转换`S?`到`T`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-272">A narrowing conversion from `S?` to `T`.</span></span>

<span data-ttu-id="ac1c1-273">例如，内部函数的扩大转换存在从`Integer?`到`Long?`因为存在从内部函数的扩大转换`Integer`到`Long`:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-273">For example, an intrinsic widening conversion exists from `Integer?` to `Long?` because an intrinsic widening conversion exists from `Integer` to `Long`:</span></span>

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

<span data-ttu-id="ac1c1-274">从转换时`T?`到`S?`，如果的值`T?`是`Nothing`，然后的值`S?`将`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-274">When converting from `T?` to `S?`, if the value of `T?` is `Nothing`, then the value of `S?` will be `Nothing`.</span></span> <span data-ttu-id="ac1c1-275">从转换时`S?`到`T`或`T?`到`S`，如果的值`T?`或`S?`是`Nothing`、`System.InvalidCastException`将引发异常。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-275">When converting from `S?` to `T` or `T?` to `S`, if the value of `T?` or `S?` is `Nothing`, a `System.InvalidCastException` exception will be thrown.</span></span>

<span data-ttu-id="ac1c1-276">由于基础类型的行为`System.Nullable(Of T)`，可以为 null 值类型时`T?`是装箱，结果是类型的装箱的值`T`，不是装箱的值类型的`T?`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-276">Because of the behavior of the underlying type `System.Nullable(Of T)`, when a nullable value type `T?` is boxed, the result is a boxed value of type `T`, not a boxed value of type `T?`.</span></span> <span data-ttu-id="ac1c1-277">和与之相反，取消装箱的可以为 null 的值类型`T?`，值将由包装`System.Nullable(Of T)`，并`Nothing`将为 null 值类型的未装箱`T?`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-277">And, conversely, when unboxing to a nullable value type `T?`, the value will be wrapped by `System.Nullable(Of T)`, and `Nothing` will be unboxed to a null value of type `T?`.</span></span> <span data-ttu-id="ac1c1-278">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-278">For example:</span></span>

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

<span data-ttu-id="ac1c1-279">此行为的一个副作用是，可以为 null 的值类型`T?`显示实现的接口的所有`T`，因为将值类型转换为一个接口需要要进行装箱的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-279">A side effect of this behavior is that a nullable value type `T?` appears to implement all of the interfaces of `T`, because converting a value type to an interface requires the type to be boxed.</span></span> <span data-ttu-id="ac1c1-280">因此，`T?`可转换为所有接口的`T`可转换为。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-280">As a result, `T?` is convertible to all the interfaces that `T` is convertible to.</span></span> <span data-ttu-id="ac1c1-281">务必要注意，null 值，则键入`T?`实际实现的接口`T`用于泛型约束检查或反射的目的。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-281">It is important to note, however, that a nullable value type `T?` does not actually implement the interfaces of `T` for the purposes of generic constraint checking or reflection.</span></span> <span data-ttu-id="ac1c1-282">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-282">For example:</span></span>

```vb
Interface I1
End Interface

Structure T1
    Implements I1
    ...
End Structure

Module Test
    Sub M1(Of T As I1)(ByVal x As T)
    End Sub

    Sub Main()
        Dim x? As T1 = Nothing
        Dim y As I1 = x                ' Valid
        M1(x)                          ' Error: x? does not satisfy I1 constraint
    End Sub
End Module
```

## <a name="string-conversions"></a><span data-ttu-id="ac1c1-283">字符串转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-283">String Conversions</span></span>

<span data-ttu-id="ac1c1-284">转换`Char`到`String`导致其第一个字符是字符值的字符串。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-284">Converting `Char` into `String` results in a string whose first character is the character value.</span></span> <span data-ttu-id="ac1c1-285">转换`String`到`Char`得到其值是字符串的第一个字符一个字符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-285">Converting `String` into `Char` results in a character whose value is the first character of the string.</span></span> <span data-ttu-id="ac1c1-286">将转换的数组`Char`到`String`导致其字符数组的元素的字符串。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-286">Converting an array of `Char` into `String` results in a string whose characters are the elements of the array.</span></span> <span data-ttu-id="ac1c1-287">转换`String`成一个数组`Char`导致其元素是字符串的字符的字符数组。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-287">Converting `String` into an array of `Char` results in an array of characters whose elements are the characters of the string.</span></span>

<span data-ttu-id="ac1c1-288">之间的确切转换`String`并`Boolean`， `Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`， `Double`， `Date`，反之亦然，超出了本规范的范围和是实现依赖除了一个详细信息。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-288">The exact conversions between `String` and `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`, and vice versa, are beyond the scope of this specification and are implementation dependent with the exception of one detail.</span></span> <span data-ttu-id="ac1c1-289">字符串转换，应始终考虑在运行时环境的当前区域性。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-289">String conversions always consider the current culture of the run-time environment.</span></span> <span data-ttu-id="ac1c1-290">在这种情况下，它们必须在运行时执行。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-290">As such, they must be performed at run time.</span></span>

## <a name="widening-conversions"></a><span data-ttu-id="ac1c1-291">扩大转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-291">Widening Conversions</span></span>

<span data-ttu-id="ac1c1-292">扩大转换永远不会溢出，但会导致精度损失。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-292">Widening conversions never overflow but may entail a loss of precision.</span></span> <span data-ttu-id="ac1c1-293">以下转换扩大转换：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-293">The following conversions are widening conversions:</span></span>

<span data-ttu-id="ac1c1-294">__标识/默认转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-294">__Identity/Default conversions__</span></span>

* <span data-ttu-id="ac1c1-295">从类型到本身。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-295">From a type to itself.</span></span>

* <span data-ttu-id="ac1c1-296">从匿名委托类型生成到任何具有相同签名的委托类型 lambda 方法重新分类。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-296">From an anonymous delegate type generated for a lambda method reclassification to any delegate type with an identical signature.</span></span>

* <span data-ttu-id="ac1c1-297">从文本`Nothing`为类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-297">From the literal `Nothing` to a type.</span></span>

<span data-ttu-id="ac1c1-298">__数值转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-298">__Numeric conversions__</span></span>

* <span data-ttu-id="ac1c1-299">从`Byte`到`UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-299">From `Byte` to `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ac1c1-300">从`SByte`到`Short`， `Integer`， `Long`， `Decimal`， `Single`，或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-300">From `SByte` to `Short`, `Integer`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ac1c1-301">从`UShort`到`UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-301">From `UShort` to `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ac1c1-302">从`Short`到`Integer`， `Long`， `Decimal`，`Single`或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-302">From `Short` to `Integer`, `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="ac1c1-303">从`UInteger`到`ULong`， `Long`， `Decimal`， `Single`，或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-303">From `UInteger` to `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ac1c1-304">从`Integer`到`Long`， `Decimal`，`Single`或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-304">From `Integer` to `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="ac1c1-305">从`ULong`到`Decimal`， `Single`，或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-305">From `ULong` to `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ac1c1-306">从`Long`到`Decimal`，`Single`或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-306">From `Long` to `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="ac1c1-307">从`Decimal`到`Single`或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-307">From `Decimal` to `Single` or `Double`.</span></span>

* <span data-ttu-id="ac1c1-308">从`Single`到`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-308">From `Single` to `Double`.</span></span>

* <span data-ttu-id="ac1c1-309">从文本`0`对枚举类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-309">From the literal `0` to an enumerated type.</span></span> <span data-ttu-id="ac1c1-310">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-310">(__Note.__</span></span> <span data-ttu-id="ac1c1-311">从转换`0`到任何枚举类型正在扩大以简化测试的标志。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-311">The conversion from `0` to any enumerated type is widening to simplify testing flags.</span></span> <span data-ttu-id="ac1c1-312">例如，如果`Values`是一个枚举的类型使用值`One`，可以测试变量`v`类型的`Values`语`(v And Values.One) = 0`。)</span><span class="sxs-lookup"><span data-stu-id="ac1c1-312">For example, if `Values` is an enumerated type with a value `One`, you could test a variable `v` of type `Values` by saying `(v And Values.One) = 0`.)</span></span>

* <span data-ttu-id="ac1c1-313">从为其基础的数值类型，或为其基础的数值类型没有扩大转换为数值类型的枚举类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-313">From an enumerated type to its underlying numeric type, or to a numeric type that its underlying numeric type has a widening conversion to.</span></span>

* <span data-ttu-id="ac1c1-314">从类型的常量表达式`ULong`， `Long`， `UInteger`， `Integer`， `UShort`， `Short`， `Byte`，或`SByte`到更窄的类型，提供常量表达式的值位于目标类型的范围。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-314">From a constant expression of type `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, or `SByte` to a narrower type, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="ac1c1-315">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-315">(__Note.__</span></span> <span data-ttu-id="ac1c1-316">从转换`UInteger`或`Integer`到`Single`，`ULong`或`Long`到`Single`或者`Double`，或`Decimal`到`Single`或`Double`可能会导致丢失精度，但将永远不会会导致丢失量值。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-316">Conversions from `UInteger` or `Integer` to `Single`, `ULong` or `Long` to `Single` or `Double`, or `Decimal` to `Single` or `Double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="ac1c1-317">其他扩大数值转换永远不会丢失任何信息。）</span><span class="sxs-lookup"><span data-stu-id="ac1c1-317">The other widening numeric conversions never lose any information.)</span></span>

<span data-ttu-id="ac1c1-318">__引用转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-318">__Reference conversions__</span></span>

* <span data-ttu-id="ac1c1-319">从引用类型到基类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-319">From a reference type to a base type.</span></span>

* <span data-ttu-id="ac1c1-320">从接口类型的引用类型，前提是该类型实现的接口或变体兼容接口。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-320">From a reference type to an interface type, provided that the type implements the interface or a variant compatible interface.</span></span>

* <span data-ttu-id="ac1c1-321">从接口类型到`Object`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-321">From an interface type to `Object`.</span></span>

* <span data-ttu-id="ac1c1-322">从接口类型到变体兼容接口类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-322">From an interface type to a variant compatible interface type.</span></span>

* <span data-ttu-id="ac1c1-323">从委托类型到兼容的变体委托类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-323">From a delegate type to a variant compatible delegate type.</span></span> <span data-ttu-id="ac1c1-324">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-324">(__Note.__</span></span> <span data-ttu-id="ac1c1-325">这些规则被隐含的许多其他引用转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-325">Many other reference conversions are implied by these rules.</span></span> <span data-ttu-id="ac1c1-326">例如，匿名委托是引用类型继承自`System.MulticastDelegate`; 数组类型是引用类型继承自`System.Array`; 匿名类型是引用类型继承自`System.Object`。)</span><span class="sxs-lookup"><span data-stu-id="ac1c1-326">For example, anonymous delegates are reference types that inherit from `System.MulticastDelegate`; array types are reference types that inherit from `System.Array`; anonymous types are reference types that inherit from `System.Object`.)</span></span>

<span data-ttu-id="ac1c1-327">__匿名委托转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-327">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="ac1c1-328">从匿名委托的 lambda 方法重新分类到任何更广的委托类型生成类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-328">From an anonymous delegate type generated for a lambda method reclassification to any wider delegate type.</span></span>

<span data-ttu-id="ac1c1-329">__数组转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-329">__Array conversions__</span></span>

* <span data-ttu-id="ac1c1-330">从数组类型`S`的元素类型与`Se`为数组类型`T`的元素类型与`Te`，提供以下所有项都为 true:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-330">From an array type `S` with an element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="ac1c1-331">`S` 和`T`的区别只在于元素类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-331">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="ac1c1-332">这两`Se`和`Te`是引用类型或类型参数已知为引用类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-332">Both `Se` and `Te` are reference types or are type parameters known to be a reference type.</span></span>

  * <span data-ttu-id="ac1c1-333">扩大引用、 数组或类型参数转换存在从`Se`到`Te`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-333">A widening reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="ac1c1-334">从数组类型`S`枚举的元素类型与`Se`为数组类型`T`的元素类型与`Te`，提供以下所有项都为 true:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-334">From an array type `S` with an enumerated element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="ac1c1-335">`S` 和`T`的区别只在于元素类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-335">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="ac1c1-336">`Te` 是的基础类型`Se`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-336">`Te` is the underlying type of `Se`.</span></span>

* <span data-ttu-id="ac1c1-337">从数组类型`S`秩为 1 的枚举的元素类型的`Se`给`System.Collections.Generic.IList(Of Te)`， `IReadOnlyList(Of Te)`， `ICollection(Of Te)`， `IReadOnlyCollection(Of Te)`，和`IEnumerable(Of Te)`、 提供的以下项之一为 true:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-337">From an array type `S` of rank 1 with an enumerated element type `Se`, to `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="ac1c1-338">这两`Se`并`Te`是引用类型或类型形参已知的引用类型和扩大的引用，数组，或存在从类型参数转换`Se`到`Te`; 或</span><span class="sxs-lookup"><span data-stu-id="ac1c1-338">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a widening reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>

  * <span data-ttu-id="ac1c1-339">`Te` 是的基础类型`Se`; 或</span><span class="sxs-lookup"><span data-stu-id="ac1c1-339">`Te` is the underlying type of `Se`; or</span></span>

  * <span data-ttu-id="ac1c1-340">`Te` 等于 `Se`</span><span class="sxs-lookup"><span data-stu-id="ac1c1-340">`Te` is identical to `Se`</span></span>

<span data-ttu-id="ac1c1-341">__值类型转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-341">__Value Type conversions__</span></span>

* <span data-ttu-id="ac1c1-342">从值类型到基类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-342">From a value type to a base type.</span></span>

* <span data-ttu-id="ac1c1-343">从值类型到该类型实现的接口类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-343">From a value type to an interface type that the type implements.</span></span>

<span data-ttu-id="ac1c1-344">__可以为 null 的值类型转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-344">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="ac1c1-345">从类型`T`为类型`T?`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-345">From a type `T` to the type `T?`.</span></span>

* <span data-ttu-id="ac1c1-346">从类型`T?`为某种`S?`，其中不存在从类型扩大转换`T`为类型`S`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-346">From a type `T?` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="ac1c1-347">从类型`T`为某种`S?`，其中不存在从类型扩大转换`T`为类型`S`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-347">From a type `T` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="ac1c1-348">从类型`T?`向接口类型的类型`T`实现。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-348">From a type `T?` to an interface type that the type `T` implements.</span></span>

<span data-ttu-id="ac1c1-349">__字符串转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-349">__String conversions__</span></span>

* <span data-ttu-id="ac1c1-350">从`Char`到`String`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-350">From `Char` to `String`.</span></span>

* <span data-ttu-id="ac1c1-351">从`Char()`到`String`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-351">From `Char()` to `String`.</span></span>

<span data-ttu-id="ac1c1-352">__类型参数转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-352">__Type Parameter conversions__</span></span>

* <span data-ttu-id="ac1c1-353">从类型参数为`Object`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-353">From a type parameter to `Object`.</span></span>

* <span data-ttu-id="ac1c1-354">从接口类型约束或任何接口类型约束与兼容的接口变体的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-354">From a type parameter to an interface type constraint or any interface variant compatible with an interface type constraint.</span></span>

* <span data-ttu-id="ac1c1-355">从实现接口的类约束的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-355">From a type parameter to an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="ac1c1-356">从类约束实现的接口与兼容接口变体类型参数。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-356">From a type parameter to an interface variant compatible with an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="ac1c1-357">从类约束或类约束的基类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-357">From a type parameter to a class constraint, or a base type of the class constraint.</span></span>

* <span data-ttu-id="ac1c1-358">从类型参数`T`到类型参数约束`Tx`，或任何`Tx`已扩大转换为。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-358">From a type parameter `T` to a type parameter constraint `Tx`, or anything `Tx` has a widening conversion to.</span></span>

## <a name="narrowing-conversions"></a><span data-ttu-id="ac1c1-359">收缩转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-359">Narrowing Conversions</span></span>

<span data-ttu-id="ac1c1-360">收缩转换是在类型明显不同值得使用收缩表示法的域之间不能证明始终成功的转换，转换可能会丢失信息，已知的和转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-360">Narrowing conversions are conversions that cannot be proved to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit narrowing notation.</span></span> <span data-ttu-id="ac1c1-361">以下转换被归类为收缩转换：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-361">The following conversions are classified as narrowing conversions:</span></span>

<span data-ttu-id="ac1c1-362">__布尔转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-362">__Boolean conversions__</span></span>

* <span data-ttu-id="ac1c1-363">从`Boolean`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-363">From `Boolean` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ac1c1-364">从`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`到`Boolean`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-364">From `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double` to `Boolean`.</span></span>

<span data-ttu-id="ac1c1-365">__数值转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-365">__Numeric conversions__</span></span>

* <span data-ttu-id="ac1c1-366">从`Byte`到`SByte`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-366">From `Byte` to `SByte`.</span></span>

* <span data-ttu-id="ac1c1-367">从`SByte`到`Byte`， `UShort`， `UInteger`，或`ULong`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-367">From `SByte` to `Byte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="ac1c1-368">从`UShort`到`Byte`， `SByte`，或`Short`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-368">From `UShort` to `Byte`, `SByte`, or `Short`.</span></span>

* <span data-ttu-id="ac1c1-369">从`Short`到`Byte`， `SByte`， `UShort`， `UInteger`，或`ULong`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-369">From `Short` to `Byte`, `SByte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="ac1c1-370">从`UInteger`到`Byte`， `SByte`， `UShort`， `Short`，或`Integer`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-370">From `UInteger` to `Byte`, `SByte`, `UShort`, `Short`, or `Integer`.</span></span>

* <span data-ttu-id="ac1c1-371">从`Integer`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`，或`ULong`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-371">From `Integer` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="ac1c1-372">从`ULong`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`，或`Long`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-372">From `ULong` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `Long`.</span></span>

* <span data-ttu-id="ac1c1-373">从`Long`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`，或`ULong`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-373">From `Long` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `ULong`.</span></span>

* <span data-ttu-id="ac1c1-374">从`Decimal`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`，或`Long`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-374">From `Decimal` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, or `Long`.</span></span>

* <span data-ttu-id="ac1c1-375">从`Single`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`，或`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-375">From `Single` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal`.</span></span>

* <span data-ttu-id="ac1c1-376">从`Double`到`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`，或`Single`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-376">From `Double` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, or `Single`.</span></span>

* <span data-ttu-id="ac1c1-377">从对枚举类型的数值类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-377">From a numeric type to an enumerated type.</span></span>

* <span data-ttu-id="ac1c1-378">从枚举类型为数值类型及其基础数值类型没有为收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-378">From an enumerated type to a numeric type its underlying numeric type has a narrowing conversion to.</span></span>

* <span data-ttu-id="ac1c1-379">从枚举类型到另一种枚举类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-379">From an enumerated type to another enumerated type.</span></span>

<span data-ttu-id="ac1c1-380">__引用转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-380">__Reference conversions__</span></span>

* <span data-ttu-id="ac1c1-381">从引用类型到派生程度更大的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-381">From a reference type to a more derived type.</span></span>

* <span data-ttu-id="ac1c1-382">从类类型为接口类型，前提是类类型不实现接口类型或接口类型变体与它兼容。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-382">From a class type to an interface type, provided the class type does not implement the interface type or an interface type variant compatible with it.</span></span>

* <span data-ttu-id="ac1c1-383">从接口类型到类类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-383">From an interface type to a class type.</span></span>

* <span data-ttu-id="ac1c1-384">从接口类型到另一个接口类型，提供那里两种类型之间没有继承关系，并提供它们都不会变体兼容。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-384">From an interface type to another interface type, provided there is no inheritance relationship between the two types and provided they are not variant compatible.</span></span>

<span data-ttu-id="ac1c1-385">__匿名委托转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-385">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="ac1c1-386">从任何更窄的委托类型到 lambda 方法重新分类为生成的匿名委托类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-386">From an anonymous delegate type generated for a lambda method reclassification to any narrower delegate type.</span></span>

<span data-ttu-id="ac1c1-387">__数组转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-387">__Array conversions__</span></span>

* <span data-ttu-id="ac1c1-388">从数组类型`S`的元素类型与`Se`，为数组类型`T`的元素类型与`Te`，前提是所有以下都为 true:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-388">From an array type `S` with an element type `Se`, to an array type `T` with an element type `Te`, provided that all of the following are true:</span></span>

  * <span data-ttu-id="ac1c1-389">`S` 和`T`的区别只在于元素类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-389">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="ac1c1-390">这两`Se`和`Te`是引用类型或类型参数不知道是值类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-390">Both `Se` and `Te` are reference types or are type parameters not known to be value types.</span></span>
  * <span data-ttu-id="ac1c1-391">收缩引用、 数组或类型参数转换存在从`Se`到`Te`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-391">A narrowing reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="ac1c1-392">从数组类型`S`的元素类型与`Se`为数组类型`T`枚举的元素类型与`Te`，提供以下所有项都为 true:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-392">From an array type `S` with an element type `Se` to an array type `T` with an enumerated element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="ac1c1-393">`S` 和`T`的区别只在于元素类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-393">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="ac1c1-394">`Se` 是的基础类型`Te`，或它们就是这两个不同的枚举的类型共享相同的基础类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-394">`Se` is the underlying type of `Te` , or they are both different enumerated types that share the same underlying type.</span></span>

* <span data-ttu-id="ac1c1-395">从数组类型`S`秩为 1 的枚举的元素类型的`Se`给`IList(Of Te)`， `IReadOnlyList(Of Te)`， `ICollection(Of Te)`，`IReadOnlyCollection(Of Te)`和`IEnumerable(Of Te)`、 提供的以下项之一为 true:</span><span class="sxs-lookup"><span data-stu-id="ac1c1-395">From an array type `S` of rank 1 with an enumerated element type `Se`, to `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="ac1c1-396">这两`Se`并`Te`是引用类型或类型参数已知为引用类型，并且收缩引用、 数组或类型参数转换存在一个从`Se`到`Te`; 或</span><span class="sxs-lookup"><span data-stu-id="ac1c1-396">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a narrowing reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>
  * <span data-ttu-id="ac1c1-397">`Se` 是的基础类型`Te`，或它们就是这两个不同的枚举的类型共享相同的基础类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-397">`Se` is the underlying type of `Te`, or they are both different enumerated types that share the same underlying type.</span></span>

<span data-ttu-id="ac1c1-398">__值类型转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-398">__Value type conversions__</span></span>

* <span data-ttu-id="ac1c1-399">从引用类型到派生程度更大的值类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-399">From a reference type to a more derived value type.</span></span>

* <span data-ttu-id="ac1c1-400">从接口类型到值类型，前提是值类型实现的接口类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-400">From an interface type to a value type, provided the value type implements the interface type.</span></span>

<span data-ttu-id="ac1c1-401">__可以为 null 的值类型转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-401">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="ac1c1-402">从类型`T?`为某种`T`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-402">From a type `T?` to a type `T`.</span></span>

* <span data-ttu-id="ac1c1-403">从类型`T?`为某种`S?`，其中不存在从类型收缩转换`T`为类型`S`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-403">From a type `T?` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="ac1c1-404">从类型`T`为某种`S?`，其中不存在从类型收缩转换`T`为类型`S`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-404">From a type `T` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="ac1c1-405">从类型`S?`为某种`T`，其中不存在从类型转换`S`为类型`T`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-405">From a type `S?` to a type `T`, where there is a conversion from the type `S` to the type `T`.</span></span>

<span data-ttu-id="ac1c1-406">__字符串转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-406">__String conversions__</span></span>

* <span data-ttu-id="ac1c1-407">从`String`到`Char`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-407">From `String` to `Char`.</span></span>

* <span data-ttu-id="ac1c1-408">从`String`到`Char()`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-408">From `String` to `Char()`.</span></span>

* <span data-ttu-id="ac1c1-409">从`String`到`Boolean`来回`Boolean`到`String`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-409">From `String` to `Boolean` and from `Boolean` to `String`.</span></span>

* <span data-ttu-id="ac1c1-410">之间的转换`String`并`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`， `Decimal`， `Single`，或`Double`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-410">Conversions between `String` and `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="ac1c1-411">从`String`到`Date`来回`Date`到`String`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-411">From `String` to `Date` and from `Date` to `String`.</span></span>

<span data-ttu-id="ac1c1-412">__类型参数转换__</span><span class="sxs-lookup"><span data-stu-id="ac1c1-412">__Type Parameter conversions__</span></span>

* <span data-ttu-id="ac1c1-413">从`Object`类型参数。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-413">From `Object` to a type parameter.</span></span>

* <span data-ttu-id="ac1c1-414">从类型为接口类型，提供类型参数的参数不为该接口约束或约束为实现该接口的类。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-414">From a type parameter to an interface type, provided the type parameter is not constrained to that interface or constrained to a class that implements that interface.</span></span>

* <span data-ttu-id="ac1c1-415">从接口类型到类型参数。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-415">From an interface type to a type parameter.</span></span>

* <span data-ttu-id="ac1c1-416">从类约束的派生类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-416">From a type parameter to a derived type of a class constraint.</span></span>

* <span data-ttu-id="ac1c1-417">从类型参数`T`为任何类型参数约束`Tx`收缩转换为。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-417">From a type parameter `T` to anything a type parameter constraint `Tx` has a narrowing conversion to.</span></span>

## <a name="type-parameter-conversions"></a><span data-ttu-id="ac1c1-418">类型参数转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-418">Type Parameter Conversions</span></span>

<span data-ttu-id="ac1c1-419">如果任何，放在其上由该约束，确定类型参数转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-419">Type parameters' conversions are determined by the constraints, if any, put on them.</span></span> <span data-ttu-id="ac1c1-420">类型参数`T`始终可以在与转换到其自身， `Object`，以及任何接口类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-420">A type parameter `T` can always be converted to itself, to and from `Object`, and to and from any interface type.</span></span> <span data-ttu-id="ac1c1-421">请注意，如果类型`T`是值类型在运行时，将从转换`T`到`Object`或接口类型将装箱转换和从转换`Object`或接口类型到`T`将取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-421">Note that if the type `T` is a value type at run-time, converting from `T` to `Object` or an interface type will be a boxing conversion and converting from `Object` or an interface type to `T` will be an unboxing conversion.</span></span> <span data-ttu-id="ac1c1-422">具有类约束的类型参数`C`定义的类型参数从其他转换`C`及其基类，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-422">A type parameter with a class constraint `C` defines additional conversions from the type parameter to `C` and its base classes, and vice versa.</span></span> <span data-ttu-id="ac1c1-423">类型参数`T`具有类型参数约束`Tx`定义的转换`Tx`以及任何`Tx`将转换为。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-423">A type parameter `T` with a type parameter constraint `Tx` defines a conversion to `Tx` and anything `Tx` converts to.</span></span>

<span data-ttu-id="ac1c1-424">其元素类型是接口约束的类型参数的数组`I`具有相同的协变的数组转换为数组的元素类型是`I`，前提是该类型形参也具有`Class`类约束 （或由于仅引用数组元素类型可以是协变）。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-424">An array whose element type is a type parameter with an interface constraint `I` has the same covariant array conversions as an array whose element type is `I`, provided that the type parameter also has a `Class` or class constraint (since only reference array element types can be covariant).</span></span> <span data-ttu-id="ac1c1-425">其元素类型是类约束的类型参数的数组`C`具有相同的协变的数组转换为数组的元素类型是`C`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-425">An array whose element type is a type parameter with a class constraint `C` has the same covariant array conversions as an array whose element type is `C`.</span></span>

<span data-ttu-id="ac1c1-426">上述的转换规则不允许从受约束的类型参数转换为非接口类型，这可能会感到惊讶。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-426">The above conversions rules do not permit conversions from unconstrained type parameters to non-interface types, which may be surprising.</span></span> <span data-ttu-id="ac1c1-427">这样做的原因是为了防止混淆的此类转换语义。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-427">The reason for this is to prevent confusion about the semantics of such conversions.</span></span> <span data-ttu-id="ac1c1-428">例如，请考虑以下声明：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-428">For example, consider the following declaration:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

<span data-ttu-id="ac1c1-429">如果转换`T`到`Integer`允许，极有可能认为该`X(Of Integer).F(7)`将返回`7L`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-429">If the conversion of `T` to `Integer` were permitted, one might easily expect that `X(Of Integer).F(7)` would return `7L`.</span></span> <span data-ttu-id="ac1c1-430">但是，不这样做，因为已知类型在编译时是数字时，将仅考虑数值转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-430">However, it would not, because numeric conversions are only considered when the types are known to be numeric at compile time.</span></span> <span data-ttu-id="ac1c1-431">为了使语义清晰、 上面的示例中必须改为编写：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-431">In order to make the semantics clear, the above example must instead be written:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a><span data-ttu-id="ac1c1-432">用户定义的转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-432">User-Defined Conversions</span></span>

<span data-ttu-id="ac1c1-433">*内部函数转换*是由时 （即此规范中列出），语言定义的转换*用户定义的转换*通过重载定义`CType`运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-433">*Intrinsic conversions* are conversions defined by the language (i.e. listed in this specification), while *user-defined conversions* are defined by overloading the `CType` operator.</span></span> <span data-ttu-id="ac1c1-434">如果没有内部函数转换适用类型之间进行转换时将视为用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-434">When converting between types, if no intrinsic conversions are applicable then user-defined conversions will be considered.</span></span> <span data-ttu-id="ac1c1-435">如果不是用户定义转换*最具体*的源和目标类型，则用户定义的转换将使用。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-435">If there is a user-defined conversion that is *most specific* for the source and target types, then the user-defined conversion will be used.</span></span> <span data-ttu-id="ac1c1-436">否则，会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-436">Otherwise, a compile-time error results.</span></span> <span data-ttu-id="ac1c1-437">最具体的转换是其操作数是"最靠近"的源类型，其结果类型是"最靠近"目标类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-437">The most specific conversion is the one whose operand is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="ac1c1-438">在确定使用何种用户定义的转换时，将使用最具体的扩大转换;如果没有扩大转换是最具体的将使用最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-438">When determining what user-defined conversion to use, the most specific widening conversion will be used; if no widening conversion is most specific, the most specific narrowing conversion will be used.</span></span> <span data-ttu-id="ac1c1-439">如果没有不最具体收缩转换，然后进行转换，则未定义，并将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-439">If there is no most specific narrowing conversion, then the conversion is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="ac1c1-440">以下部分介绍如何确定最特定的转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-440">The following sections cover how the most specific conversions are determined.</span></span> <span data-ttu-id="ac1c1-441">它们将使用以下术语：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-441">They use the following terms:</span></span>

<span data-ttu-id="ac1c1-442">如果内部函数的扩大转换存在从类型转换`A`为某种`B`，和如果既没有`A`也不`B`是接口，然后`A`是*包含*通过`B`，并`B`*包含* `A`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-442">If an intrinsic widening conversion exists from a type `A` to a type `B`, and if neither `A` nor `B` are interfaces, then `A` is *encompassed* by `B`, and `B` *encompasses* `A`.</span></span>

<span data-ttu-id="ac1c1-443">*最完备*中的一组类型的类型是包含在集中的所有其他类型的一个类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-443">The *most encompassing* type in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="ac1c1-444">如果没有一个类型包含所有其他类型，然后集都有没有最大的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-444">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="ac1c1-445">直观地讲，最大的类型是在 set--一种类型的每种其他类型可以转换成通过扩大转换中的"最大"类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-445">In intuitive terms, the most encompassing type is the "largest" type in the set -- the one type to which each of the other types can be converted through a widening conversion.</span></span>

<span data-ttu-id="ac1c1-446">*最包含*中的一组类型的类型是在集中的所有其他类型包含的一个类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-446">The *most encompassed* type in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="ac1c1-447">如果没有一个类型被包含所有其他类型，一组具有最不包含类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-447">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="ac1c1-448">直观地讲，包含程度最大的类型为 set--可以转换为每个通过收缩转换的其他类型的一个类型中的"最小"类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-448">In intuitive terms, the most encompassed type is the "smallest" type in the set -- the one type that can be converted to each of the other types through a narrowing conversion.</span></span>

<span data-ttu-id="ac1c1-449">收集候选用户定义的转换类型时`T?`，定义的用户定义的转换运算符`T`改为使用。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-449">When collecting the candidate user-defined conversions for a type `T?`, the user-defined conversion operators defined by `T` are used instead.</span></span> <span data-ttu-id="ac1c1-450">如果要转换为的类型也可以为 null 的值类型，则任一`T`的用户定义涉及只有不可为 null 的值类型的转换运算符提升。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-450">If the type being converted to is also a nullable value type, then any of `T`'s user-defined conversions operators that involve only non-nullable value types are lifted.</span></span> <span data-ttu-id="ac1c1-451">转换运算符`T`到`S`提升为从转换`T?`到`S?`而通过将转换在评估`T?`到`T`，如有必要，然后评估用户定义的转换从运算符`T`到`S`，然后进行转换`S`到`S?`，如果有必要。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-451">A conversion operator from `T` to `S` is lifted to be a conversion from `T?` to `S?` and is evaluated by converting `T?` to `T`, if necessary, then evaluating the user-defined conversion operator from `T` to `S` and then converting `S` to `S?`, if necessary.</span></span> <span data-ttu-id="ac1c1-452">如果要转换的值是`Nothing`，但是，提升的转换运算符将直接转换的值为`Nothing`化为`S?`。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-452">If the value being converted is `Nothing`, however, a lifted conversion operator converts directly into a value of `Nothing` typed as `S?`.</span></span> <span data-ttu-id="ac1c1-453">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-453">For example:</span></span>

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Legal: y is still null
        x = New T()
        y = x                ' Legal: Converts from T to S
    End Sub
End Module
```

<span data-ttu-id="ac1c1-454">解析的转换，用户定义转换运算符时，始终首选通过提升的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-454">When resolving conversions, user-defined conversions operators are always preferred over lifted conversion operators.</span></span> <span data-ttu-id="ac1c1-455">例如：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-455">For example:</span></span>

```vb
Structure S
    ...
End Structure

Structure T
    Public Shared Widening Operator CType(ByVal v As T) As S
        ...
    End Operator

    Public Shared Widening Operator CType(ByVal v As T?) As S?
        ...
    End Operator
End Structure

Module Test
    Sub Main()
        Dim x As T?
        Dim y As S?

        y = x                ' Calls user-defined conversion, not lifted conversion
    End Sub
End Module
```

<span data-ttu-id="ac1c1-456">在运行时计算的用户定义的转换可能涉及最多三个步骤：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-456">At run-time, evaluating a user-defined conversion can involve up to three steps:</span></span>

1. <span data-ttu-id="ac1c1-457">首先，值的源类型转换为必要时使用的内部函数转换的操作数类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-457">First, the value is converted from the source type to the operand type using an intrinsic conversion, if necessary.</span></span>

2. <span data-ttu-id="ac1c1-458">然后，调用用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-458">Then, the user-defined conversion is invoked.</span></span>

3. <span data-ttu-id="ac1c1-459">最后，用户定义转换的结果转换为目标类型使用的内部函数转换，如有必要。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-459">Finally, the result of the user-defined conversion is converted to the target type using an intrinsic conversion, if necessary.</span></span>

<span data-ttu-id="ac1c1-460">请务必注意，计算的用户定义的转换将永远不会涉及多个用户定义的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-460">It is important to note that evaluation of a user-defined conversion will never involve more than one user-defined conversion operator.</span></span>

### <a name="most-specific-widening-conversion"></a><span data-ttu-id="ac1c1-461">最具体的扩大转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-461">Most Specific Widening Conversion</span></span>

<span data-ttu-id="ac1c1-462">使用以下步骤完成确定最特定用户扩大转换之间的运算符两种类型：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-462">Determining the most specific user-defined widening conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="ac1c1-463">首先，收集所有候选转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-463">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="ac1c1-464">候选转换运算符是所有的用户定义的扩大转换运算符，将源类型和所有用户定义的扩大转换运算符，为目标类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-464">The candidate conversion operators are all of the user-defined widening conversion operators in the source type and all of the user-defined widening conversion operators in the target type.</span></span>

2. <span data-ttu-id="ac1c1-465">然后，从集中移除所有不适用的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-465">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="ac1c1-466">转换运算符时，适用于源类型和目标类型的内部扩大转换运算符从源类型到操作数类型并且没有一个内部扩大转换运算符从运算符的结果为目标类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-466">A conversion operator is applicable to a source type and target type if there is an intrinsic widening conversion operator from the source type to the operand type and there is an intrinsic widening conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="ac1c1-467">如果不有任何适用的转换运算符，则没有最具体扩大转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-467">If there are no applicable conversion operators, then there is no most specific widening conversion.</span></span>

3. <span data-ttu-id="ac1c1-468">然后，确定适用的转换运算符的最特定的源类型：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-468">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="ac1c1-469">如果任何转换运算符将直接从源类型转换，源类型是最具体的源类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-469">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="ac1c1-470">否则，最具体的源类型是在源类型的转换运算符的组合集包含程度最大的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-470">Otherwise, the most specific source type is the most encompassed type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="ac1c1-471">如果不需要最包含可以找到类型，则会出现不最具体扩大转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-471">If no most encompassed type can be found, then there is no most specific widening conversion.</span></span>

4. <span data-ttu-id="ac1c1-472">然后，确定适用的转换运算符最具体的目标类型：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-472">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="ac1c1-473">如果转换运算符的任何转换直接到目标类型，目标类型是最具体的目标类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-473">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="ac1c1-474">否则，最具体的目标类型为目标类型的转换运算符的组合集最大的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-474">Otherwise, the most specific target type is the most encompassing type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="ac1c1-475">如果找不到任何最大的类型，则没有最具体扩大转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-475">If no most encompassing type can be found, then there is no most specific widening conversion.</span></span>

5. <span data-ttu-id="ac1c1-476">然后，如果只有一个转换运算符将从最具体的源类型转换为最具体的目标类型，则这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-476">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="ac1c1-477">如果存在多个此类运算符，则没有最具体的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-477">If more than one such operator exists, then there is no most specific widening conversion.</span></span>

### <a name="most-specific-narrowing-conversion"></a><span data-ttu-id="ac1c1-478">最具体的收缩转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-478">Most Specific Narrowing Conversion</span></span>

<span data-ttu-id="ac1c1-479">使用以下步骤完成确定最特定用户定义的收缩转换之间的运算符两种类型：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-479">Determining the most specific user-defined narrowing conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="ac1c1-480">首先，收集所有候选转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-480">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="ac1c1-481">候选转换运算符是所有的源类型中的用户定义的转换运算符和所有目标类型中的用户定义的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-481">The candidate conversion operators are all of the user-defined conversion operators in the source type and all of the user-defined conversion operators in the target type.</span></span>

2. <span data-ttu-id="ac1c1-482">然后，从集中移除所有不适用的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-482">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="ac1c1-483">转换运算符时，适用于源类型和目标类型的源类型的操作数类型到一个内部函数转换运算符，并且没有从运算符的结果为目标类型的内部函数转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-483">A conversion operator is applicable to a source type and target type if there is an intrinsic conversion operator from the source type to the operand type and there is an intrinsic conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="ac1c1-484">如果不有任何适用的转换运算符，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-484">If there are no applicable conversion operators, then there is no most specific narrowing conversion.</span></span>

3. <span data-ttu-id="ac1c1-485">然后，确定适用的转换运算符的最特定的源类型：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-485">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="ac1c1-486">如果任何转换运算符将直接从源类型转换，源类型是最具体的源类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-486">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="ac1c1-487">否则，如果转换运算符的任何转换中包含的源类型的类型，最具体的源类型是中的源类型的这些转换运算符的组合集包含程度最大的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-487">Otherwise, if any of the conversion operators convert from types that encompass the source type, then the most specific source type is the most encompassed type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="ac1c1-488">如果不需要最包含可以找到类型，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-488">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="ac1c1-489">否则，最具体的源类型是在源类型的转换运算符的组合集最大的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-489">Otherwise, the most specific source type is the most encompassing type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="ac1c1-490">如果找不到任何最大的类型，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-490">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

4. <span data-ttu-id="ac1c1-491">然后，确定适用的转换运算符最具体的目标类型：</span><span class="sxs-lookup"><span data-stu-id="ac1c1-491">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="ac1c1-492">如果转换运算符的任何转换直接到目标类型，目标类型是最具体的目标类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-492">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="ac1c1-493">否则，如果任何转换运算符将转换为目标类型包含的类型，最具体的目标类型是在源类型的这些转换运算符的组合集最大的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-493">Otherwise, if any of the conversion operators convert to types that are encompassed by the target type, then the most specific target type is the most encompassing type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="ac1c1-494">如果找不到任何最大的类型，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-494">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="ac1c1-495">否则，最具体的目标类型是在目标类型的转换运算符的组合集包含程度最大的类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-495">Otherwise, the most specific target type is the most encompassed type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="ac1c1-496">如果不需要最包含可以找到类型，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-496">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

5. <span data-ttu-id="ac1c1-497">然后，如果只有一个转换运算符将从最具体的源类型转换为最具体的目标类型，则这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-497">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="ac1c1-498">如果存在多个此类运算符，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-498">If more than one such operator exists, then there is no most specific narrowing conversion.</span></span>

## <a name="native-conversions"></a><span data-ttu-id="ac1c1-499">本机转换</span><span class="sxs-lookup"><span data-stu-id="ac1c1-499">Native Conversions</span></span>

<span data-ttu-id="ac1c1-500">多个转换都属于*本机转换*因为本机支持的.NET Framework。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-500">Several of the conversions are classified as *native conversions* because they are supported natively by the .NET Framework.</span></span> <span data-ttu-id="ac1c1-501">这些转换都是可以使用优化的那些`DirectCast`和`TryCast`转换运算符，以及其他特殊行为。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-501">These conversions are ones that can be optimized through the use of the `DirectCast` and `TryCast` conversion operators, as well as other special behaviors.</span></span> <span data-ttu-id="ac1c1-502">归类为本机转换的转换： 标识转换、 默认转换、 引用转换、 数组转换、 值类型转换和类型的参数转换。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-502">The conversions classified as native conversions are: identity conversions, default conversions, reference conversions, array conversions, value type conversions, and type parameter conversions.</span></span>

## <a name="dominant-type"></a><span data-ttu-id="ac1c1-503">基准类型</span><span class="sxs-lookup"><span data-stu-id="ac1c1-503">Dominant Type</span></span>

<span data-ttu-id="ac1c1-504">给定的一组类型，通常很有必要情况下，例如类型推断来确定在*基准类型*的集。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-504">Given a set of types, it is often necessary in situations such as type inference to determine the *dominant type* of the set.</span></span> <span data-ttu-id="ac1c1-505">类型的一组基准类型确定通过先删除一个或多个其他类型不具有隐式转换为任何类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-505">The dominant type of a set of types is determined by first removing any types that one or more other types do not have an implicit conversion to.</span></span> <span data-ttu-id="ac1c1-506">如果不存在类型左此时，则没有基准类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-506">If there are no types left at this point, there is no dominant type.</span></span> <span data-ttu-id="ac1c1-507">最多包含剩余类型的则基准类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-507">The dominant type is then the most encompassed of the remaining types.</span></span> <span data-ttu-id="ac1c1-508">如果存在多个类型最包含，则没有基准类型。</span><span class="sxs-lookup"><span data-stu-id="ac1c1-508">If there is more than one type that is most encompassed, then there is no dominant type.</span></span>
