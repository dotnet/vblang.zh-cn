---
ms.openlocfilehash: 7aef52145a71bff1d489772e81eb786a9dbd23d1
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306199"
---
# <a name="conversions"></a><span data-ttu-id="c2b88-101">转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-101">Conversions</span></span>

<span data-ttu-id="c2b88-102">转换是将值从一种类型更改为另一种类型的过程。</span><span class="sxs-lookup"><span data-stu-id="c2b88-102">Conversion is the process of changing a value from one type to another.</span></span> <span data-ttu-id="c2b88-103">例如，可以将类型 @no__t 的值转换为 `Double` 类型的值，或类型 `Derived` 的值可转换为 `Base` 类型的值，假定 @no__t 和 @no__t 都是类，`Derived` 从 @no__t 中继承。</span><span class="sxs-lookup"><span data-stu-id="c2b88-103">For example, a value of type `Integer` can be converted to a value of type `Double`, or a value of type `Derived` can be converted to a value of type `Base`, assuming that `Base` and `Derived` are both classes and `Derived` inherits from `Base`.</span></span> <span data-ttu-id="c2b88-104">转换可能不需要更改值本身（如后面的示例中所示），或者它们可能需要值表示形式的重大更改（如前一示例中所示）。</span><span class="sxs-lookup"><span data-stu-id="c2b88-104">Conversions may not require the value itself to change (as in the latter example), or they may require significant changes in the value representation (as in the former example).</span></span>

<span data-ttu-id="c2b88-105">转换可能是扩大或收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-105">Conversions may either be widening or narrowing.</span></span> <span data-ttu-id="c2b88-106">*扩大转换*是指从一种类型到另一种类型的转换，该类型的值域至少与原始类型的值域相同。</span><span class="sxs-lookup"><span data-stu-id="c2b88-106">A *widening conversion* is a conversion from a type to another type whose value domain is at least as big, if not bigger, than the original type's value domain.</span></span> <span data-ttu-id="c2b88-107">扩大转换决不会失败。</span><span class="sxs-lookup"><span data-stu-id="c2b88-107">Widening conversions should never fail.</span></span> <span data-ttu-id="c2b88-108">*收缩转换*是指从一种类型到另一种类型的转换，该类型的值域要么小于原始类型的值域，要么完全不相关，因此在执行转换时，必须采取额外的措施（例如，转换从 `Integer` 到 `String`）。</span><span class="sxs-lookup"><span data-stu-id="c2b88-108">A *narrowing conversion* is a conversion from a type to another type whose value domain is either smaller than the original type's value domain or sufficiently unrelated that extra care must be taken when doing the conversion (for example, when converting from `Integer` to `String`).</span></span> <span data-ttu-id="c2b88-109">收缩转换可能会导致信息丢失，这可能会导致信息丢失。</span><span class="sxs-lookup"><span data-stu-id="c2b88-109">Narrowing conversions, which may entail loss of information, can fail.</span></span>

<span data-ttu-id="c2b88-110">为所有类型定义标识转换（即从类型到自身的转换）和默认值转换（即从 `Nothing` 的转换）。</span><span class="sxs-lookup"><span data-stu-id="c2b88-110">The identity conversion (i.e. a conversion from a type to itself) and default value conversion (i.e. a conversion from `Nothing`) are defined for all types.</span></span>

## <a name="implicit-and-explicit-conversions"></a><span data-ttu-id="c2b88-111">隐式转换和显式转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-111">Implicit and Explicit Conversions</span></span>

<span data-ttu-id="c2b88-112">转换可以是*隐式*的，也可以是*显式*的。</span><span class="sxs-lookup"><span data-stu-id="c2b88-112">Conversions can be either *implicit* or *explicit*.</span></span> <span data-ttu-id="c2b88-113">隐式转换在没有任何特殊语法的情况下发生。</span><span class="sxs-lookup"><span data-stu-id="c2b88-113">Implicit conversions occur without any special syntax.</span></span> <span data-ttu-id="c2b88-114">下面的示例将 `Integer` 值隐式转换为 @no__t 值：</span><span class="sxs-lookup"><span data-stu-id="c2b88-114">The following is an example of implicit conversion of an `Integer` value to a `Long` value:</span></span>

```vb
Module Test
    Sub Main()
        Dim intValue As Integer = 123
        Dim longValue As Long = intValue

        Console.WriteLine(intValue & " = " & longValue)
    End Sub
End Module
```

另一方面，显式转换需要强制转换运算符。 如果尝试在没有强制转换运算符的情况下显式转换，将导致编译时错误。 <span data-ttu-id="c2b88-117">下面的示例使用显式转换将 @no__t 0 值转换为 @no__t 值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-117">The following example uses an explicit conversion to convert a `Long` value to an `Integer` value.</span></span>

```vb
Module Test
    Sub Main()
        Dim longValue As Long = 134
        Dim intValue As Integer = CInt(longValue)

        Console.WriteLine(longValue & " = " & intValue)
    End Sub
End Module
```

隐式转换集取决于编译环境和 `Option Strict` 语句。 如果使用严格语义，则只能隐式转换。 <span data-ttu-id="c2b88-120">如果使用的是可许可语义，则所有扩大转换和收缩转换（即所有转换）可能会隐式进行。</span><span class="sxs-lookup"><span data-stu-id="c2b88-120">If permissive semantics are being used, all widening and narrowing conversions (in other words, all conversions) may occur implicitly.</span></span>

## <a name="boolean-conversions"></a><span data-ttu-id="c2b88-121">布尔值转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-121">Boolean Conversions</span></span>

<span data-ttu-id="c2b88-122">尽管 `Boolean` 不是数值类型，但它确实与数值类型进行收缩转换，就好像它是枚举类型一样。</span><span class="sxs-lookup"><span data-stu-id="c2b88-122">Although `Boolean` is not a numeric type, it does have narrowing conversions to and from the numeric types as if it were an enumerated type.</span></span> <span data-ttu-id="c2b88-123">文本 `True` 会转换为 `Byte` 的文本 `255`; 对于 `UShort`，为 `65535`，`UInteger` `4294967295`，`ULong` 转换为表达式 `-1`，2，4，5，5和 6。</span><span class="sxs-lookup"><span data-stu-id="c2b88-123">The literal `True` converts to the literal `255` for `Byte`, `65535` for `UShort`, `4294967295` for `UInteger`, `18446744073709551615` for `ULong`, and to the expression `-1` for `SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, and `Double`.</span></span> <span data-ttu-id="c2b88-124">文本 `False` 会转换为文本 `0`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-124">The literal `False` converts to the literal `0`.</span></span> <span data-ttu-id="c2b88-125">零数值转换为文本 `False`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-125">A zero numeric value converts to the literal `False`.</span></span> <span data-ttu-id="c2b88-126">所有其他数值均转换为 `True` 的文字。</span><span class="sxs-lookup"><span data-stu-id="c2b88-126">All other numeric values convert to the literal `True`.</span></span>

<span data-ttu-id="c2b88-127">存在从布尔值到字符串的收缩转换，转换为 `System.Boolean.TrueString` 或 @no__t 为-1。</span><span class="sxs-lookup"><span data-stu-id="c2b88-127">There is a narrowing conversion from Boolean to String, converting to either `System.Boolean.TrueString` or `System.Boolean.FalseString`.</span></span> <span data-ttu-id="c2b88-128">还存在从 `String` 到 `Boolean` 的收缩转换：如果字符串等于 `TrueString` 或 `FalseString` （在当前区域性中，区分），则它将使用适当的值;否则，它会尝试将字符串分析为数值类型（如果可能，为十六进制或八进制），并使用上述规则;否则，会引发 `System.InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-128">There is also a narrowing conversion from `String` to `Boolean`: if the string was equal to `TrueString` or `FalseString` (in the current culture, case-insensitively) then it uses the appropriate value; otherwise it attempts to parse the string as a numeric type (in hex or octal if possible, otherwise as a float) and uses the above rules; otherwise it throws `System.InvalidCastException`.</span></span>

## <a name="numeric-conversions"></a><span data-ttu-id="c2b88-129">数值转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-129">Numeric Conversions</span></span>

<span data-ttu-id="c2b88-130">类型 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、@no__t 为 @no__t、`Long`、`Decimal`、@no__t 和，以及所有枚举类型之间存在数字转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-130">Numeric conversions exist between the types `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single` and `Double`, and all enumerated types.</span></span> <span data-ttu-id="c2b88-131">转换时，会将枚举类型视为它们的基础类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-131">When being converted, enumerated types are treated as if they were their underlying types.</span></span> <span data-ttu-id="c2b88-132">转换为枚举类型时，源值不需要符合枚举类型中定义的值集。</span><span class="sxs-lookup"><span data-stu-id="c2b88-132">When converting to an enumerated type, the source value is not required to conform to the set of values defined in the enumerated type.</span></span> <span data-ttu-id="c2b88-133">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-133">For example:</span></span>

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

<span data-ttu-id="c2b88-134">数字转换在运行时处理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="c2b88-134">Numeric conversions are processed at run-time as follows:</span></span>

* <span data-ttu-id="c2b88-135">对于从数值类型到更大数值类型的转换，只需将该值转换为更大的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-135">For a conversion from a numeric type to a wider numeric type, the value is simply converted to the wider type.</span></span> <span data-ttu-id="c2b88-136">从 `UInteger`、`Integer`、`ULong`、@no__t 或 @no__t 到 @no__t 或 @no__t 的转换将舍入到最接近的 `Single` 或 @no__t 8 值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-136">Conversions from `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal` to `Single` or `Double` are rounded to the nearest `Single` or `Double` value.</span></span> <span data-ttu-id="c2b88-137">虽然这种转换可能会导致精度损失，但永远不会导致数量级损失。</span><span class="sxs-lookup"><span data-stu-id="c2b88-137">While this conversion may cause a loss of precision, it will never cause a loss of magnitude.</span></span>

* <span data-ttu-id="c2b88-138">如果从整型转换为另一整型类型，或从 `Single`、`Double` 或 `Decimal` 转换为整型类型，则结果取决于是否启用了整数溢出检查：</span><span class="sxs-lookup"><span data-stu-id="c2b88-138">For a conversion from an integral type to another integral type, or from `Single`, `Double`, or `Decimal` to an integral type, the result depends on whether integer overflow checking is on:</span></span>

  <span data-ttu-id="c2b88-139">*如果正在检查整数溢出：*</span><span class="sxs-lookup"><span data-stu-id="c2b88-139">*If integer overflow is being checked:*</span></span>

  * <span data-ttu-id="c2b88-140">如果源是整数类型，则当源参数在目标类型的范围内时，转换将成功。</span><span class="sxs-lookup"><span data-stu-id="c2b88-140">If the source is an integral type, the conversion succeeds if the source argument is within the range of the destination type.</span></span> <span data-ttu-id="c2b88-141">如果 source 参数超出了目标类型的范围，则该转换会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="c2b88-141">The conversion throws a `System.OverflowException` exception if the source argument is outside the range of the destination type.</span></span>

  * <span data-ttu-id="c2b88-142">如果源 `Single`、@no__t 为-1 或 `Decimal`，则源值将向上或向下舍入到最接近的整数值，并且此整数值将成为转换的结果。</span><span class="sxs-lookup"><span data-stu-id="c2b88-142">If the source is `Single`, `Double`, or `Decimal`, the source value is rounded up or down to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="c2b88-143">如果源值平均接近于两个整数值，则将值舍入到值在最小有效位位置的偶数。</span><span class="sxs-lookup"><span data-stu-id="c2b88-143">If the source value is equally close to two integral values, the value is rounded to the value that has an even number in the least significant digit position.</span></span> <span data-ttu-id="c2b88-144">如果生成的整数值超出了目标类型的范围，则会引发 `System.OverflowException` 异常。</span><span class="sxs-lookup"><span data-stu-id="c2b88-144">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` exception is thrown.</span></span>

  <span data-ttu-id="c2b88-145">*如果未检查整数溢出：*</span><span class="sxs-lookup"><span data-stu-id="c2b88-145">*If integer overflow is not being checked:*</span></span>

  * <span data-ttu-id="c2b88-146">如果源是整数类型，则转换始终会成功，并且只包括丢弃源值的最高有效位。</span><span class="sxs-lookup"><span data-stu-id="c2b88-146">If the source is an integral type, the conversion always succeeds and simply consists of discarding the most significant bits of the source value.</span></span>

  * <span data-ttu-id="c2b88-147">如果源 `Single`、@no__t 为-1 或 `Decimal`，则转换始终会成功，并且只需将源值舍入到最接近的整数值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-147">If the source is `Single`, `Double`, or `Decimal`, the conversion always succeeds and simply consists of rounding the source value towards the nearest integral value.</span></span> <span data-ttu-id="c2b88-148">如果源值平均接近于两个整数值，则始终将值舍入到最小有效位位置中具有偶数的值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-148">If the source value is equally close to two integral values, the value is always rounded to the value that has an even number in the least significant digit position.</span></span>

* <span data-ttu-id="c2b88-149">对于从 `Double` 到 `Single` 的转换，`Double` 值舍入为最接近的 @no__t 值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-149">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="c2b88-150">如果 `Double` 值太小而无法表示为 `Single`，则结果将变为零或负零。</span><span class="sxs-lookup"><span data-stu-id="c2b88-150">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="c2b88-151">如果 @no__t 0 值太大，无法表示为 `Single`，则结果将变为正无穷或负无穷。</span><span class="sxs-lookup"><span data-stu-id="c2b88-151">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="c2b88-152">如果 @no__t 0 @no__t 值为-1，则结果也为 `NaN`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-152">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

* <span data-ttu-id="c2b88-153">对于从 @no__t 0 或 `Double` 到 `Decimal` 的转换，将源值转换为 `Decimal` 表示形式，并在需要时舍入为最接近的数字。</span><span class="sxs-lookup"><span data-stu-id="c2b88-153">For a conversion from `Single` or `Double` to `Decimal`, the source value is converted to `Decimal` representation and rounded to the nearest number after the 28th decimal place if required.</span></span> <span data-ttu-id="c2b88-154">如果源值太小而无法表示为 `Decimal`，则结果将变为零。</span><span class="sxs-lookup"><span data-stu-id="c2b88-154">If the source value is too small to represent as a `Decimal`, the result becomes zero.</span></span> <span data-ttu-id="c2b88-155">如果源值 `NaN`、无限大或太大而无法表示为 `Decimal`，则会引发 @no__t 2 异常。</span><span class="sxs-lookup"><span data-stu-id="c2b88-155">If the source value is `NaN`, infinity, or too large to represent as a `Decimal`, a `System.OverflowException` exception is thrown.</span></span>

* <span data-ttu-id="c2b88-156">对于从 `Double` 到 `Single` 的转换，`Double` 值舍入为最接近的 @no__t 值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-156">For a conversion from `Double` to `Single`, the `Double` value is rounded to the nearest `Single` value.</span></span> <span data-ttu-id="c2b88-157">如果 `Double` 值太小而无法表示为 `Single`，则结果将变为零或负零。</span><span class="sxs-lookup"><span data-stu-id="c2b88-157">If the `Double` value is too small to represent as a `Single`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="c2b88-158">如果 @no__t 0 值太大，无法表示为 `Single`，则结果将变为正无穷或负无穷。</span><span class="sxs-lookup"><span data-stu-id="c2b88-158">If the `Double` value is too large to represent as a `Single`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="c2b88-159">如果 @no__t 0 @no__t 值为-1，则结果也为 `NaN`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-159">If the `Double` value is `NaN`, the result is also `NaN`.</span></span>

## <a name="reference-conversions"></a><span data-ttu-id="c2b88-160">引用转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-160">Reference Conversions</span></span>

<span data-ttu-id="c2b88-161">引用类型可以转换为基类型，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="c2b88-161">Reference types may be converted to a base type, and vice versa.</span></span> <span data-ttu-id="c2b88-162">如果要转换的值为 null 值、派生类型本身或派生程度更高的类型，则从基类型转换为派生程度更高的类型的转换只会在运行时成功。</span><span class="sxs-lookup"><span data-stu-id="c2b88-162">Conversions from a base type to a more derived type only succeed at run time if the value being converted is a null value, the derived type itself, or a more derived type.</span></span>

<span data-ttu-id="c2b88-163">类和接口类型可以在任何接口类型之间强制转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-163">Class and interface types can be cast to and from any interface type.</span></span> <span data-ttu-id="c2b88-164">如果所涉及的实际类型具有继承或实现关系，则类型和接口类型之间的转换只会在运行时成功。</span><span class="sxs-lookup"><span data-stu-id="c2b88-164">Conversions between a type and an interface type only succeed at run time if the actual types involved have an inheritance or implementation relationship.</span></span> <span data-ttu-id="c2b88-165">由于接口类型将始终包含从 @no__t 派生的类型的实例，因此接口类型也可以始终在 @no__t 之间来回转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-165">Because an interface type will always contain an instance of a type that derives from `Object`, an interface type can also always be cast to and from `Object`.</span></span>

<span data-ttu-id="c2b88-166">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="c2b88-166">__Note.__</span></span> <span data-ttu-id="c2b88-167">将 @no__t 0 类与它不实现的接口相互转换不是错误的，因为表示 COM 类的类可能具有直到运行时未知的接口实现。</span><span class="sxs-lookup"><span data-stu-id="c2b88-167">It is not an error to convert a `NotInheritable` classes to and from interfaces that it does not implement because classes that represent COM classes may have interface implementations that are not known until run time.</span></span> 

<span data-ttu-id="c2b88-168">如果引用转换在运行时失败，则会引发 `System.InvalidCastException` 异常。</span><span class="sxs-lookup"><span data-stu-id="c2b88-168">If a reference conversion fails at run time, a `System.InvalidCastException` exception is thrown.</span></span>

### <a name="reference-variance-conversions"></a><span data-ttu-id="c2b88-169">引用差异转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-169">Reference Variance Conversions</span></span>

<span data-ttu-id="c2b88-170">泛型接口或委托可能具有 variant 类型参数，它们允许在类型的兼容变体之间进行转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-170">Generic interfaces or delegates may have variant type parameters that allow conversions between compatible variants of the type.</span></span> <span data-ttu-id="c2b88-171">因此，在运行时，从类类型或接口类型到与它继承自或实现的接口类型的变量类型的转换将会成功。</span><span class="sxs-lookup"><span data-stu-id="c2b88-171">Therefore, at runtime a conversion from a class type or an interface type to an interface type that is variant compatible with an interface type it inherits from or implements will succeed.</span></span> <span data-ttu-id="c2b88-172">同样，可以将委托类型强制转换为与变体兼容的委托类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-172">Similarly, delegate types can be cast to and from variant compatible delegate types.</span></span> <span data-ttu-id="c2b88-173">例如，委托类型</span><span class="sxs-lookup"><span data-stu-id="c2b88-173">For example, the delegate type</span></span>

```vb
Delegate Function F(Of In A, Out R)(a As A) As R
```

<span data-ttu-id="c2b88-174">允许从 `F(Of Object, Integer)` 到 @no__t 的转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-174">would allow a conversion from `F(Of Object, Integer)` to `F(Of String, Integer)`.</span></span> <span data-ttu-id="c2b88-175">也就是说，可以安全地将委托 `F` （采用 `Object`）用作委托 `F`，该委托采用 `String`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-175">That is, a delegate `F` which takes `Object` may be safely used as a delegate `F` which takes `String`.</span></span> <span data-ttu-id="c2b88-176">调用委托时，目标方法应为对象，而字符串为对象。</span><span class="sxs-lookup"><span data-stu-id="c2b88-176">When the delegate is invoked, the target method will be expecting an object, and a string is an object.</span></span>

<span data-ttu-id="c2b88-177">如果存在以下情况，则泛型委托或接口类型 `S(Of S1,...,Sn)` 称为与泛型接口或委托类型的*变体兼容*`T(Of T1,...,Tn)`：</span><span class="sxs-lookup"><span data-stu-id="c2b88-177">A generic delegate or interface type `S(Of S1,...,Sn)` is said to be *variant compatible* with a generic interface or delegate type `T(Of T1,...,Tn)` if:</span></span>

* <span data-ttu-id="c2b88-178">`S` 和 `T` 均从相同的泛型类型 `U(Of U1,...,Un)` 构造。</span><span class="sxs-lookup"><span data-stu-id="c2b88-178">`S` and `T` are both constructed from the same generic type `U(Of U1,...,Un)`.</span></span>

* <span data-ttu-id="c2b88-179">对于每个类型参数 `Ux`：</span><span class="sxs-lookup"><span data-stu-id="c2b88-179">For each type parameter `Ux`:</span></span>

  * <span data-ttu-id="c2b88-180">如果在声明类型参数时没有变体，则 `Sx`，`Tx` 必须是同一类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-180">If the type parameter was declared without variance then `Sx` and `Tx` must be the same type.</span></span>

  * <span data-ttu-id="c2b88-181">如果类型参数已声明 `In`，则必须具有从 `Sx` 到 @no__t 的扩大标识、默认值、引用、数组或类型参数转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-181">If the type parameter was declared `In` then there must be a widening identity, default, reference, array, or type parameter conversion from `Sx` to `Tx`.</span></span>

  * <span data-ttu-id="c2b88-182">如果类型参数已声明 `Out`，则必须具有从 `Tx` 到 @no__t 的扩大标识、默认值、引用、数组或类型参数转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-182">If the type parameter was declared `Out` then there must be a widening identity, default, reference, array, or type parameter conversion from `Tx` to `Sx`.</span></span>

<span data-ttu-id="c2b88-183">在从类转换为具有 variant 类型参数的泛型接口时，如果类实现了多个变体兼容接口，如果不存在非变量转换，转换将不明确。</span><span class="sxs-lookup"><span data-stu-id="c2b88-183">When converting from a class to a generic interface with variant type parameters, if the class implements more than one variant compatible interface the conversion is ambiguous if there is not a non-variant conversion.</span></span> <span data-ttu-id="c2b88-184">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-184">For example:</span></span>

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

### <a name="anonymous-delegate-conversions"></a><span data-ttu-id="c2b88-185">匿名委托转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-185">Anonymous Delegate Conversions</span></span>

<span data-ttu-id="c2b88-186">当归类为 lambda 方法的表达式在没有目标类型（例如 `Dim x = Function(a As Integer, b As Integer) a + b`）或目标类型不是委托类型的上下文中将其重新归类为某个值时，生成的表达式的类型是等效的匿名委托类型lambda 方法的签名。</span><span class="sxs-lookup"><span data-stu-id="c2b88-186">When an expression classified as a lambda method is reclassified as a value in a context where there is no target type (for example, `Dim x = Function(a As Integer, b As Integer) a + b`), or where the target type is not a delegate type, the type of the resulting expression is an anonymous delegate type equivalent to the signature of the lambda method.</span></span> <span data-ttu-id="c2b88-187">此匿名委托类型具有到任何兼容的委托类型的转换：兼容的委托类型是可以使用委托创建表达式创建的任何委托类型，该委托类型将匿名委托类型的 `Invoke` 方法作为参数。</span><span class="sxs-lookup"><span data-stu-id="c2b88-187">This anonymous delegate type has a conversion to any compatible delegate type: a compatible delegate type is any delegate type that can be created using a delegate creation expression with the anonymous delegate type's `Invoke` method as a parameter.</span></span> <span data-ttu-id="c2b88-188">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-188">For example:</span></span>

```vb
' Anonymous delegate type similar to Func(Of Object, Object, Object)
Dim x = Function(x, y) x + y

' OK because delegate type is compatible
Dim y As Func(Of Integer, Integer, Integer) = x
```

<span data-ttu-id="c2b88-189">请注意，类型 `System.Delegate` 和 `System.MulticastDelegate` 本身不被视为委托类型（即使所有委托类型都从它们继承）。</span><span class="sxs-lookup"><span data-stu-id="c2b88-189">Note that the types `System.Delegate` and `System.MulticastDelegate` are not themselves considered delegate types (even though all delegate types inherit from them).</span></span> <span data-ttu-id="c2b88-190">另请注意，从匿名委托类型到兼容委托类型的转换不是引用转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-190">Also, note that the conversion from anonymous delegate type to a compatible delegate type is not a reference conversion.</span></span>

## <a name="array-conversions"></a><span data-ttu-id="c2b88-191">数组转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-191">Array Conversions</span></span>

<span data-ttu-id="c2b88-192">除了在数组上定义的转换，因为它们是引用类型，但对于数组有一些特殊转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-192">Besides the conversions that are defined on arrays by virtue of the fact that they are reference types, several special conversions exist for arrays.</span></span>

<span data-ttu-id="c2b88-193">对于任意两个类型 `A` 和 `B`，如果它们既不是值类型的引用类型或类型参数，并且如果 `A` 具有对 `B` 类型的引用、数组或类型参数转换，则存在从 `A` 类型的数组到数组的转换键入 `B` 的相同级别。</span><span class="sxs-lookup"><span data-stu-id="c2b88-193">For any two types `A` and `B`, if they are both reference types or type parameters not known to be value types, and if `A` has a reference, array, or type parameter conversion to `B`, a conversion exists from an array of type `A` to an array of type `B` with the same rank.</span></span> <span data-ttu-id="c2b88-194">此关系称为*数组协方差*。</span><span class="sxs-lookup"><span data-stu-id="c2b88-194">This relationship is known as *array covariance*.</span></span> <span data-ttu-id="c2b88-195">具体说来，数组协方差意味着元素类型为 `B` 的数组元素可能是元素类型为 `A` 的数组元素，前提是 `A` 和 @no__t 都是引用类型，并且 `B` 具有引用转换或数组转换为 @no__t 5。</span><span class="sxs-lookup"><span data-stu-id="c2b88-195">Array covariance in particular means that an element of an array whose element type is `B` may actually be an element of an array whose element type is `A`, provided that both `A` and `B` are reference types and that `B` has a reference conversion or array conversion to `A`.</span></span> <span data-ttu-id="c2b88-196">在下面的示例中，第二次调用 `F` 会导致引发 @no__t 1 异常，因为 `b` 的实际元素类型为 `String`，而不是 `Object`：</span><span class="sxs-lookup"><span data-stu-id="c2b88-196">In the following example, the second invocation of `F` causes a `System.ArrayTypeMismatchException` exception to be thrown because the actual element type of `b` is `String`, not `Object`:</span></span>

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

<span data-ttu-id="c2b88-197">由于数组协方差，对引用类型数组的元素的赋值包含运行时检查，可确保赋给数组元素的值实际上是允许的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-197">Because of array covariance, assignments to elements of reference type arrays include a run-time check that ensures that the value being assigned to the array element is actually of a permitted type.</span></span>

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

<span data-ttu-id="c2b88-198">在此示例中，对方法 `Fill` 中 @no__t 的赋值隐式包含一个运行时检查，该检查可确保变量 @no__t 引用的对象 @no__t 为3，或与数组的实际元素类型兼容的类型的实例 `array`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-198">In this example, the assignment to `array(i)` in method `Fill` implicitly includes a run-time check that ensures that the object referenced by the variable `value` is either `Nothing` or an instance of a type that is compatible with the actual element type of array `array`.</span></span> <span data-ttu-id="c2b88-199">在方法 `Main` 中，第一次调用方法 `Fill` 成功，但第三次调用会导致在执行第一次到 `array(i)` 的赋值运算时引发 @no__t 2 异常。</span><span class="sxs-lookup"><span data-stu-id="c2b88-199">In method `Main`, the first two invocations of method `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` exception to be thrown upon executing the first assignment to `array(i)`.</span></span> <span data-ttu-id="c2b88-200">发生此异常的原因是，@no__t 0 不能存储在 @no__t 的数组中。</span><span class="sxs-lookup"><span data-stu-id="c2b88-200">The exception occurs because an `Integer` cannot be stored in a `String` array.</span></span>

<span data-ttu-id="c2b88-201">如果某个数组元素类型是类型参数，而该类型参数的类型在运行时为值类型，则会引发 `System.InvalidCastException` 异常。</span><span class="sxs-lookup"><span data-stu-id="c2b88-201">If one of the array element types is a type parameter whose type turns out to be a value type at runtime, a `System.InvalidCastException` exception will be thrown.</span></span> <span data-ttu-id="c2b88-202">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-202">For example:</span></span>

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

<span data-ttu-id="c2b88-203">如果数组具有相同的秩，则枚举类型的数组和枚举类型的基础类型的数组或具有相同基础类型的其他枚举类型的数组之间也存在转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-203">Conversions also exist between an array of an enumerated type and an array of the enumerated type's underlying type or an array of another enumerated type with the same underlying type, provided the arrays have the same rank.</span></span>

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

<span data-ttu-id="c2b88-204">在此示例中，将 `Color` 的数组转换为 @no__t 的数组，@no__t 2 的基础类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-204">In this example, an array of `Color` is converted to and from an array of `Byte`, `Color`'s underlying type.</span></span> <span data-ttu-id="c2b88-205">但是，转换为 `Integer` 的数组将是错误的，因为 `Integer` 不是 @no__t 的基础类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-205">The conversion to an array of `Integer`, however, will be an error because `Integer` is not the underlying type of `Color`.</span></span>

<span data-ttu-id="c2b88-206">类型为 `A()` 的秩为的数组将转换为集合接口类型，@no__t 为-1、`IReadOnlyList(Of B)`、`ICollection(Of B)`、`IReadOnlyCollection(Of B)` 和 `IEnumerable(Of B)`，但前提是满足以下条件之一：</span><span class="sxs-lookup"><span data-stu-id="c2b88-206">A rank-1 array of type `A()` also has an array conversion to the collection interface types `IList(Of B)`, `IReadOnlyList(Of B)`, `ICollection(Of B)`, `IReadOnlyCollection(Of B)` and `IEnumerable(Of B)` found in `System.Collections.Generic`, so long as one of the following is true:</span></span>

- <span data-ttu-id="c2b88-207">`A` 和 `B` 既是引用类型，也不是值类型未知的类型参数;和 `A` 的扩展引用、数组或类型参数转换为 `B`;或</span><span class="sxs-lookup"><span data-stu-id="c2b88-207">`A` and `B` are both reference types or type parameters not known to be value types; and `A` has a widening reference, array or type parameter conversion to `B`; or</span></span>
- <span data-ttu-id="c2b88-208">`A` 和 `B` 都是同一基础类型的枚举类型;或</span><span class="sxs-lookup"><span data-stu-id="c2b88-208">`A` and `B` are both enumerated types of the same underlying type; or</span></span>
- <span data-ttu-id="c2b88-209">@no__t 0 和 `B` 中的一个是枚举类型，另一个是其基础类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-209">one of `A` and `B` is an enumerated type, and the other is its underlying type.</span></span>

<span data-ttu-id="c2b88-210">具有任何秩的类型 A 的任何数组还会将数组转换为非泛型集合接口类型 `IList`、`ICollection` 和 `IEnumerable` 中找到的 。</span><span class="sxs-lookup"><span data-stu-id="c2b88-210">Any array of type A with any rank also has an array conversion to the non-generic collection interface types `IList`, `ICollection` and `IEnumerable` found in `System.Collections`.</span></span>

<span data-ttu-id="c2b88-211">可以使用 `For Each` 或直接调用 @no__t 1 方法来循环访问生成的接口。</span><span class="sxs-lookup"><span data-stu-id="c2b88-211">It is possible to iterate over the resulting interfaces using `For Each`, or through invoking the `GetEnumerator` methods directly.</span></span> <span data-ttu-id="c2b88-212">对于第1级数组，将 `IList` 或 `ICollection` 的泛型或非泛型形式转换为，也可以按索引获取元素。</span><span class="sxs-lookup"><span data-stu-id="c2b88-212">In the case of rank-1 arrays converted generic or non-generic forms of `IList` or `ICollection`, it is also possible to get elements by index.</span></span> <span data-ttu-id="c2b88-213">对于转换为泛型或非泛型形式 `IList` 的 rank 数组，还可以按索引设置元素，如上文所述，受相同的运行时数组协变检查的限制。</span><span class="sxs-lookup"><span data-stu-id="c2b88-213">In the case of rank-1 arrays converted to generic or non-generic forms of `IList`, it is also possible to set elements by index, subject to the same runtime array covariance checks as described above.</span></span> <span data-ttu-id="c2b88-214">所有其他接口方法的行为都不是由 VB 语言规范定义的;这是由基础运行时组成的。</span><span class="sxs-lookup"><span data-stu-id="c2b88-214">The behavior of all other interface methods is undefined by the VB language specification; it is up to the underlying runtime.</span></span>

## <a name="value-type-conversions"></a><span data-ttu-id="c2b88-215">值类型转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-215">Value Type Conversions</span></span>

<span data-ttu-id="c2b88-216">值类型值可以转换为它的基引用类型之一，或者转换为它通过一个名为*装箱*的进程实现的接口类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-216">A value type value can be converted to one of its base reference types or an interface type that it implements through a process called *boxing*.</span></span> <span data-ttu-id="c2b88-217">将值类型值装箱后，会将该值从它所在的位置复制到 .NET Framework 堆。</span><span class="sxs-lookup"><span data-stu-id="c2b88-217">When a value type value is boxed, the value is copied from the location where it lives onto the .NET Framework heap.</span></span> <span data-ttu-id="c2b88-218">然后，将返回对堆的此位置的引用并可将其存储在引用类型变量中。</span><span class="sxs-lookup"><span data-stu-id="c2b88-218">A reference to this location on the heap is then returned and can be stored in a reference type variable.</span></span> <span data-ttu-id="c2b88-219">此引用也称为值类型的*装箱*实例。</span><span class="sxs-lookup"><span data-stu-id="c2b88-219">This reference is also referred to as a *boxed* instance of the value type.</span></span> <span data-ttu-id="c2b88-220">装箱实例与引用类型（而不是值类型）具有相同的语义。</span><span class="sxs-lookup"><span data-stu-id="c2b88-220">The boxed instance has the same semantics as a reference type instead of a value type.</span></span>

<span data-ttu-id="c2b88-221">装箱的值类型可通过一种称为 "*取消装箱*" 的过程转换回其原始值类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-221">Boxed value types can be converted back to their original value type through a process called *unboxing*.</span></span> <span data-ttu-id="c2b88-222">装箱的值类型取消装箱后，会将值从堆复制到变量位置。</span><span class="sxs-lookup"><span data-stu-id="c2b88-222">When a boxed value type is unboxed, the value is copied from the heap into a variable location.</span></span> <span data-ttu-id="c2b88-223">从此时起，它的行为就像它是值类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-223">From that point on, it behaves as if it was a value type.</span></span> <span data-ttu-id="c2b88-224">对值类型取消装箱时，该值必须为 null 值或值类型的实例。</span><span class="sxs-lookup"><span data-stu-id="c2b88-224">When unboxing a value type, the value must be a null value or an instance of the value type.</span></span> <span data-ttu-id="c2b88-225">否则，会引发 `System.InvalidCastException` 异常。</span><span class="sxs-lookup"><span data-stu-id="c2b88-225">Otherwise a `System.InvalidCastException` exception is thrown.</span></span> <span data-ttu-id="c2b88-226">如果值是枚举类型的实例，则还可以取消装箱到枚举类型的基础类型或具有相同基础类型的另一个枚举类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-226">If the value is an instance of an enumerated type, that value can also be unboxed to the enumerated type's underlying type or another enumerated type that has the same underlying type.</span></span> <span data-ttu-id="c2b88-227">Null 值被视为文本 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-227">A null value is treated as if it were the literal `Nothing`.</span></span>

<span data-ttu-id="c2b88-228">若要正确支持可以为 null 的值类型，则在进行装箱和取消装箱时，将对值类型 `System.Nullable(Of T)` 进行特殊处理。</span><span class="sxs-lookup"><span data-stu-id="c2b88-228">To support nullable value types well, the value type `System.Nullable(Of T)` is treated specially when doing boxing and unboxing.</span></span> <span data-ttu-id="c2b88-229">如果将类型 @no__t 的值装箱为类型，则会生成类型为 @no__t 的装箱值。如果值的 `HasValue` 属性为 `True`，则返回值为 `Nothing` 的值，如果该值的 `HasValue` 属性为 `False`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-229">Boxing a value of type `Nullable(Of T)` results in a boxed value of type `T` if the value's `HasValue` property is `True` or a value of `Nothing` if the value's `HasValue` property is `False`.</span></span> <span data-ttu-id="c2b88-230">取消对类型 `T` 到 `Nullable(Of T)` 的值将导致 `Nullable(Of T)` 的实例，该实例的 `Value` 属性为装箱值，其 @no__t 属性为 `True`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-230">Unboxing a value of type `T` to `Nullable(Of T)` results in an instance of `Nullable(Of T)` whose `Value` property is the boxed value and whose `HasValue` property is `True`.</span></span> <span data-ttu-id="c2b88-231">值 `Nothing` 可以取消装箱，以便对任何 `T` `Nullable(Of T)`，并生成一个值，其 @no__t 3 属性 @no__t。</span><span class="sxs-lookup"><span data-stu-id="c2b88-231">The value `Nothing` can be unboxed to `Nullable(Of T)` for any `T` and results in a value whose `HasValue` property is `False`.</span></span> <span data-ttu-id="c2b88-232">因为装箱值类型的行为类似于引用类型，所以可以创建对同一值的多个引用。</span><span class="sxs-lookup"><span data-stu-id="c2b88-232">Because boxed value types behave like reference types, it is possible to create multiple references to the same value.</span></span> <span data-ttu-id="c2b88-233">对于基元类型和枚举类型，这是不相关的，因为这些类型的实例是*不可变*的。</span><span class="sxs-lookup"><span data-stu-id="c2b88-233">For the primitive types and enumerated types, this is irrelevant because instances of those types are *immutable*.</span></span> <span data-ttu-id="c2b88-234">也就是说，不能修改这些类型的已装箱实例，因此，不可能发现存在对同一值的多个引用。</span><span class="sxs-lookup"><span data-stu-id="c2b88-234">That is, it is not possible to modify a boxed instance of those types, so it is not possible to observe the fact that there are multiple references to the same value.</span></span>

<span data-ttu-id="c2b88-235">另一方面，如果其实例变量可访问，或者其方法或属性修改了其实例变量，则结构可以是可变的。</span><span class="sxs-lookup"><span data-stu-id="c2b88-235">Structures, on the other hand, may be mutable if its instance variables are accessible or if its methods or properties modify its instance variables.</span></span> <span data-ttu-id="c2b88-236">如果使用一个对装箱结构的引用来修改该结构，则对装箱结构的所有引用都将看到更改。</span><span class="sxs-lookup"><span data-stu-id="c2b88-236">If one reference to a boxed structure is used to modify the structure, then all references to the boxed structure will see the change.</span></span> <span data-ttu-id="c2b88-237">由于此结果可能是意外的，因此当类型为 `Object` 的值从一个位置复制到另一个装箱的值类型时，将自动在堆上进行克隆，而不是只复制其引用。</span><span class="sxs-lookup"><span data-stu-id="c2b88-237">Because this result may be unexpected, when a value typed as `Object` is copied from one location to another boxed value types will automatically be cloned on the heap instead of merely having their references copied.</span></span> <span data-ttu-id="c2b88-238">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-238">For example:</span></span>

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

<span data-ttu-id="c2b88-239">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="c2b88-239">The output of the program is:</span></span>

```console
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="c2b88-240">对局部变量 `val2` 的赋值不会影响局部变量 `val1`，因为在向 `Struct1` 分配装箱  时，会生成值的副本。</span><span class="sxs-lookup"><span data-stu-id="c2b88-240">The assignment to the field of the local variable `val2` does not impact the field of the local variable `val1` because when the boxed `Struct1` was assigned to `val2`, a copy of the value was made.</span></span> <span data-ttu-id="c2b88-241">与此相反，赋值 `ref2.Value = 123` 会影响 `ref1` 和 @no__t 2 引用的对象。</span><span class="sxs-lookup"><span data-stu-id="c2b88-241">In contrast, the assignment `ref2.Value = 123` affects the object that both `ref1` and `ref2` references.</span></span>

<span data-ttu-id="c2b88-242">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="c2b88-242">__Note.__</span></span> <span data-ttu-id="c2b88-243">对于类型为 `System.ValueType` 的装箱结构，不能进行结构复制，因为不能从 @no__t 的后期绑定。</span><span class="sxs-lookup"><span data-stu-id="c2b88-243">Structure copying is not done for boxed structures typed as `System.ValueType` because it is not possible to late bind off of `System.ValueType`.</span></span>

<span data-ttu-id="c2b88-244">在分配时将复制装箱值类型的规则有一个例外。</span><span class="sxs-lookup"><span data-stu-id="c2b88-244">There is one exception to the rule that boxed value types will be copied on assignment.</span></span> <span data-ttu-id="c2b88-245">如果装箱值类型引用存储在另一类型中，则不会复制内部引用。</span><span class="sxs-lookup"><span data-stu-id="c2b88-245">If a boxed value type reference is stored within another type, the inner reference will not be copied.</span></span> <span data-ttu-id="c2b88-246">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-246">For example:</span></span>

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

<span data-ttu-id="c2b88-247">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="c2b88-247">The output of the program is:</span></span>

```console
Values: 123, 123
```

<span data-ttu-id="c2b88-248">这是因为在复制值时不会复制内部装箱的值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-248">This is because the inner boxed value is not copied when the value is copied.</span></span> <span data-ttu-id="c2b88-249">因此，@no__t 0 和 `val2.Value` 都具有对同一装箱值类型的引用。</span><span class="sxs-lookup"><span data-stu-id="c2b88-249">Thus, both `val1.Value` and `val2.Value` have a reference to the same boxed value type.</span></span>

<span data-ttu-id="c2b88-250">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="c2b88-250">__Note.__</span></span> <span data-ttu-id="c2b88-251">不复制的内部装箱值类型是 .NET 类型系统的一项限制--为了确保在复制 `Object` 类型的值时，将复制所有内部装箱的值类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-251">The fact that inner boxed value types are not copied is a limitation of the .NET type system -- to ensure that all inner boxed value types were copied whenever a value of type `Object` was copied would be prohibitively expensive.</span></span>

<span data-ttu-id="c2b88-252">如前文所述，装箱值类型只能取消装箱为其原始类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-252">As previously described, boxed value types can only be unboxed to their original type.</span></span> <span data-ttu-id="c2b88-253">但是，在类型化为 `Object` 时，将对装箱的基元类型进行特殊处理。</span><span class="sxs-lookup"><span data-stu-id="c2b88-253">Boxed primitive types, however, are treated specially when typed as `Object`.</span></span> <span data-ttu-id="c2b88-254">它们可以转换为其转换为的任何其他基元类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-254">They can be converted to any other primitive type that they have a conversion to.</span></span> <span data-ttu-id="c2b88-255">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-255">For example:</span></span>

```vb
Module Test
    Sub Main()
        Dim o As Object = 5
        Dim b As Byte = CByte(o)  ' Legal
        Console.WriteLine(b) ' Prints 5
    End Sub
End Module
```

<span data-ttu-id="c2b88-256">通常情况下，不能将装箱 `Integer` 值 @no__t 取消装箱到 @no__t 的变量中。</span><span class="sxs-lookup"><span data-stu-id="c2b88-256">Normally, the boxed `Integer` value `5` could not be unboxed into a `Byte` variable.</span></span> <span data-ttu-id="c2b88-257">但是，因为 `Integer`，`Byte` 是基元类型并且具有转换，所以允许转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-257">However, because `Integer` and `Byte` are primitive types and have a conversion, the conversion is allowed.</span></span>

<span data-ttu-id="c2b88-258">需要特别注意的是，将值类型转换为接口不同于约束为接口的泛型自变量。</span><span class="sxs-lookup"><span data-stu-id="c2b88-258">It is important to note that converting a value type to an interface is different than a generic argument constrained to an interface.</span></span> <span data-ttu-id="c2b88-259">在访问受约束的类型参数上的接口成员（或调用 @no__t 的方法）时，不会发生装箱，这与访问接口的值类型和访问接口成员时的方式不同。</span><span class="sxs-lookup"><span data-stu-id="c2b88-259">When accessing interface members on a constrained type parameter (or calling methods on `Object`), boxing does not occur as it does when a value type is converted to an interface and an interface member is accessed.</span></span> <span data-ttu-id="c2b88-260">例如，假设接口 `ICounter` 包含可用于修改值的方法 `Increment`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-260">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="c2b88-261">如果 `ICounter` 用作约束，则会调用 @no__t 的方法的实现，该方法是对在上调用 `Increment` 的变量的引用，而不是装箱副本：</span><span class="sxs-lookup"><span data-stu-id="c2b88-261">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, not a boxed copy:</span></span>

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

<span data-ttu-id="c2b88-262">对 @no__t 的第一次调用将修改变量 `x` 中的值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-262">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="c2b88-263">这不等同于第二次调用 `Increment`，这会修改 @no__t 的装箱副本中的值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-263">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="c2b88-264">因此，该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="c2b88-264">Thus, the output of the program is:</span></span>

```console
0
1
1
```

### <a name="nullable-value-type-conversions"></a><span data-ttu-id="c2b88-265">可以为 null 的值类型转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-265">Nullable Value Type Conversions</span></span>

<span data-ttu-id="c2b88-266">值类型 `T` 可以转换为类型的可以为 null 的类型，@no__t 为-1。</span><span class="sxs-lookup"><span data-stu-id="c2b88-266">A value type `T` can convert to and from the nullable version of the type, `T?`.</span></span> <span data-ttu-id="c2b88-267">如果要转换的值为 `Nothing`，则从 `T?` 到 `T` 的转换将引发 2 @no__t 异常。</span><span class="sxs-lookup"><span data-stu-id="c2b88-267">The conversion from `T?` to `T` throws a `System.InvalidOperationException` exception if the value being converted is `Nothing`.</span></span> <span data-ttu-id="c2b88-268">此外，`T?` 会转换为类型 @no__t 如果 @no__t 为-2，则会将内部转换为 @no__t。</span><span class="sxs-lookup"><span data-stu-id="c2b88-268">Also, `T?` has a conversion to a type `S` if `T` has an intrinsic conversion to `S`.</span></span> <span data-ttu-id="c2b88-269">如果 `S` 是值类型，则 `T?` 与 `S?` 之间存在以下内部转换：</span><span class="sxs-lookup"><span data-stu-id="c2b88-269">And if `S` is a value type, then the following intrinsic conversions exist between `T?` and `S?`:</span></span>

* <span data-ttu-id="c2b88-270">从 `T?` 到 @no__t 的相同分类（收缩或扩大）的转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-270">A conversion of the same classification (narrowing or widening) from `T?` to `S?`.</span></span>

* <span data-ttu-id="c2b88-271">从 `T` 到 @no__t 的相同分类（收缩或扩大）的转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-271">A conversion of the same classification (narrowing or widening) from `T` to `S?`.</span></span>

* <span data-ttu-id="c2b88-272">从 `S?` 到 @no__t 的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-272">A narrowing conversion from `S?` to `T`.</span></span>

<span data-ttu-id="c2b88-273">例如，存在从 `Integer?` 到 `Long?` 的内部扩大转换，因为存在从 `Integer` 到 `Long` 的内部扩大转换：</span><span class="sxs-lookup"><span data-stu-id="c2b88-273">For example, an intrinsic widening conversion exists from `Integer?` to `Long?` because an intrinsic widening conversion exists from `Integer` to `Long`:</span></span>

```vb
Dim i As Integer? = 10
Dim l As Long? = i
```

<span data-ttu-id="c2b88-274">从 `T?` 转换到 `S?` 时，如果 @no__t 的值为 `Nothing`，则 @no__t 的值将为 `Nothing`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-274">When converting from `T?` to `S?`, if the value of `T?` is `Nothing`, then the value of `S?` will be `Nothing`.</span></span> <span data-ttu-id="c2b88-275">从 `S?` 转换为 `T` 或 `T?` 转换为 `S` 时，如果 `T?` 或 @no__t 5 的值为 `Nothing`，则会引发 @no__t 7 异常。</span><span class="sxs-lookup"><span data-stu-id="c2b88-275">When converting from `S?` to `T` or `T?` to `S`, if the value of `T?` or `S?` is `Nothing`, a `System.InvalidCastException` exception will be thrown.</span></span>

<span data-ttu-id="c2b88-276">由于基础类型 `System.Nullable(Of T)` 的行为，如果将可为 null 的值类型 @no__t 为-1，则结果为类型 `T` 的装箱值，而不是类型 @no__t 为的装箱值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-276">Because of the behavior of the underlying type `System.Nullable(Of T)`, when a nullable value type `T?` is boxed, the result is a boxed value of type `T`, not a boxed value of type `T?`.</span></span> <span data-ttu-id="c2b88-277">与此相反，当取消装箱到可为 null 的值类型 `T?` 时，该值将由 `System.Nullable(Of T)` 进行包装，`Nothing` 将取消装箱为类型 @no__t 为 null 的 null 值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-277">And, conversely, when unboxing to a nullable value type `T?`, the value will be wrapped by `System.Nullable(Of T)`, and `Nothing` will be unboxed to a null value of type `T?`.</span></span> <span data-ttu-id="c2b88-278">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-278">For example:</span></span>

```vb
Dim i1? As Integer = Nothing
Dim o1 As Object = i1

Console.WriteLine(o1 Is Nothing)                    ' Will print True
o1 = 10
i1 = CType(o1, Integer?)
Console.WriteLine(i1)                               ' Will print 10
```

<span data-ttu-id="c2b88-279">此行为的一个副作用是，可为 null 的值类型 `T?` 将显示为实现 @no__t 的所有接口，因为将值类型转换为接口需要对类型进行装箱。</span><span class="sxs-lookup"><span data-stu-id="c2b88-279">A side effect of this behavior is that a nullable value type `T?` appears to implement all of the interfaces of `T`, because converting a value type to an interface requires the type to be boxed.</span></span> <span data-ttu-id="c2b88-280">因此，`T?` 可转换为 `T` 可转换为的所有接口。</span><span class="sxs-lookup"><span data-stu-id="c2b88-280">As a result, `T?` is convertible to all the interfaces that `T` is convertible to.</span></span> <span data-ttu-id="c2b88-281">但是，请务必注意，可以为 null 的值类型 `T?` 实际上不会实现 @no__t 的接口，目的是进行泛型约束检查或反射。</span><span class="sxs-lookup"><span data-stu-id="c2b88-281">It is important to note, however, that a nullable value type `T?` does not actually implement the interfaces of `T` for the purposes of generic constraint checking or reflection.</span></span> <span data-ttu-id="c2b88-282">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-282">For example:</span></span>

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

## <a name="string-conversions"></a><span data-ttu-id="c2b88-283">字符串转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-283">String Conversions</span></span>

<span data-ttu-id="c2b88-284">将 `Char` 转换为 @no__t 时，会生成一个字符串，其第一个字符为字符值。</span><span class="sxs-lookup"><span data-stu-id="c2b88-284">Converting `Char` into `String` results in a string whose first character is the character value.</span></span> <span data-ttu-id="c2b88-285">将 `String` 转换为 `Char` 将导致字符为字符串的第一个字符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-285">Converting `String` into `Char` results in a character whose value is the first character of the string.</span></span> <span data-ttu-id="c2b88-286">将 @no__t 的数组转换为 @no__t 1 将导致字符串，其字符是数组的元素。</span><span class="sxs-lookup"><span data-stu-id="c2b88-286">Converting an array of `Char` into `String` results in a string whose characters are the elements of the array.</span></span> <span data-ttu-id="c2b88-287">如果将 @no__t 0 转换为 @no__t 的数组，则会生成一个字符数组，其元素是字符串的字符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-287">Converting `String` into an array of `Char` results in an array of characters whose elements are the characters of the string.</span></span>

<span data-ttu-id="c2b88-288">@No__t-0 和 @no__t @no__t 之间的精确转换，-2，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Boolean`0，1，2，3，反之亦然）在此规范的范围之外，实现依赖于一种情况除外。</span><span class="sxs-lookup"><span data-stu-id="c2b88-288">The exact conversions between `String` and `Boolean`, `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, `Double`, `Date`, and vice versa, are beyond the scope of this specification and are implementation dependent with the exception of one detail.</span></span> <span data-ttu-id="c2b88-289">字符串转换始终考虑运行时环境的当前区域性。</span><span class="sxs-lookup"><span data-stu-id="c2b88-289">String conversions always consider the current culture of the run-time environment.</span></span> <span data-ttu-id="c2b88-290">因此，它们必须在运行时执行。</span><span class="sxs-lookup"><span data-stu-id="c2b88-290">As such, they must be performed at run time.</span></span>

## <a name="widening-conversions"></a><span data-ttu-id="c2b88-291">扩大转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-291">Widening Conversions</span></span>

<span data-ttu-id="c2b88-292">扩大转换永不溢出，但可能会损失精度。</span><span class="sxs-lookup"><span data-stu-id="c2b88-292">Widening conversions never overflow but may entail a loss of precision.</span></span> <span data-ttu-id="c2b88-293">下面的转换是扩大转换：</span><span class="sxs-lookup"><span data-stu-id="c2b88-293">The following conversions are widening conversions:</span></span>

<span data-ttu-id="c2b88-294">__标识/默认转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-294">__Identity/Default conversions__</span></span>

* <span data-ttu-id="c2b88-295">从类型到自身。</span><span class="sxs-lookup"><span data-stu-id="c2b88-295">From a type to itself.</span></span>

* <span data-ttu-id="c2b88-296">从为 lambda 方法生成的匿名委托类型重新分类为具有相同签名的任何委托类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-296">From an anonymous delegate type generated for a lambda method reclassification to any delegate type with an identical signature.</span></span>

* <span data-ttu-id="c2b88-297">从文本 `Nothing` 到类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-297">From the literal `Nothing` to a type.</span></span>

<span data-ttu-id="c2b88-298">__数值转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-298">__Numeric conversions__</span></span>

* <span data-ttu-id="c2b88-299">从 `Byte` 到 `UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Decimal`，`Single`，或 @no__t 9。</span><span class="sxs-lookup"><span data-stu-id="c2b88-299">From `Byte` to `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="c2b88-300">从 `SByte` 到 `Short`，`Integer`，`Long`，`Decimal`，`Single`，或者 `Double`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-300">From `SByte` to `Short`, `Integer`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="c2b88-301">从 `UShort` 到 `UInteger`，`Integer`，`ULong`，`Long`，`Decimal`，`Single` 或 @no__t 7。</span><span class="sxs-lookup"><span data-stu-id="c2b88-301">From `UShort` to `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="c2b88-302">从 `Short` 到 `Integer`，`Long`，`Decimal`，`Single` 或 @no__t 5。</span><span class="sxs-lookup"><span data-stu-id="c2b88-302">From `Short` to `Integer`, `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="c2b88-303">从 `UInteger` 到 `ULong`，`Long`、`Decimal`、`Single` 或 @no__t 5。</span><span class="sxs-lookup"><span data-stu-id="c2b88-303">From `UInteger` to `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="c2b88-304">从 `Integer` 到 `Long`，`Decimal`，`Single` 或 `Double`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-304">From `Integer` to `Long`, `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="c2b88-305">从 `ULong` 到 `Decimal`、`Single` 或 @no__t 为3。</span><span class="sxs-lookup"><span data-stu-id="c2b88-305">From `ULong` to `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="c2b88-306">从 `Long` 到 `Decimal`，`Single` 或 @no__t 为3。</span><span class="sxs-lookup"><span data-stu-id="c2b88-306">From `Long` to `Decimal`, `Single` or `Double`.</span></span>

* <span data-ttu-id="c2b88-307">从 `Decimal` 到 @no__t 或 `Double`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-307">From `Decimal` to `Single` or `Double`.</span></span>

* <span data-ttu-id="c2b88-308">从 `Single` 到 `Double`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-308">From `Single` to `Double`.</span></span>

* <span data-ttu-id="c2b88-309">从文本 `0` 到枚举的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-309">From the literal `0` to an enumerated type.</span></span> <span data-ttu-id="c2b88-310">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="c2b88-310">(__Note.__</span></span> <span data-ttu-id="c2b88-311">从 `0` 到任何枚举类型的转换都是扩大，以简化测试标志。</span><span class="sxs-lookup"><span data-stu-id="c2b88-311">The conversion from `0` to any enumerated type is widening to simplify testing flags.</span></span> <span data-ttu-id="c2b88-312">例如，如果 `Values` 是值为 `One` 的枚举类型，则可以通过口述 `(v And Values.One) = 0` 来测试类型 `Values` 的变量 @no__t。）</span><span class="sxs-lookup"><span data-stu-id="c2b88-312">For example, if `Values` is an enumerated type with a value `One`, you could test a variable `v` of type `Values` by saying `(v And Values.One) = 0`.)</span></span>

* <span data-ttu-id="c2b88-313">从枚举类型转换为其基础数值类型，或转换为其基础数值类型的扩大转换为的数值类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-313">From an enumerated type to its underlying numeric type, or to a numeric type that its underlying numeric type has a widening conversion to.</span></span>

* <span data-ttu-id="c2b88-314">从类型为 `ULong`、`Long`、`UInteger`、`Integer`、`UShort`、`Short`、@no__t 为或 @no__t 到较小的类型的常量表达式，前提是常量表达式的值在目标类型的范围内。</span><span class="sxs-lookup"><span data-stu-id="c2b88-314">From a constant expression of type `ULong`, `Long`, `UInteger`, `Integer`, `UShort`, `Short`, `Byte`, or `SByte` to a narrower type, provided the value of the constant expression is within the range of the destination type.</span></span> <span data-ttu-id="c2b88-315">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="c2b88-315">(__Note.__</span></span> <span data-ttu-id="c2b88-316">从 @no__t 0 或 `Integer` 转换为 `Single`、`ULong` 或 `Long` 转换为 `Single` 或 `Double`，或者 `Decimal` 转换为 `Single` 或 @no__t 9 可能导致精度损失，但绝不会导致数量级损失。</span><span class="sxs-lookup"><span data-stu-id="c2b88-316">Conversions from `UInteger` or `Integer` to `Single`, `ULong` or `Long` to `Single` or `Double`, or `Decimal` to `Single` or `Double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="c2b88-317">其他扩大数值转换决不会丢失任何信息。）</span><span class="sxs-lookup"><span data-stu-id="c2b88-317">The other widening numeric conversions never lose any information.)</span></span>

<span data-ttu-id="c2b88-318">__引用转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-318">__Reference conversions__</span></span>

* <span data-ttu-id="c2b88-319">从引用类型到基类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-319">From a reference type to a base type.</span></span>

* <span data-ttu-id="c2b88-320">从引用类型到接口类型，前提是该类型实现接口或与变体兼容的接口。</span><span class="sxs-lookup"><span data-stu-id="c2b88-320">From a reference type to an interface type, provided that the type implements the interface or a variant compatible interface.</span></span>

* <span data-ttu-id="c2b88-321">从接口类型到 `Object`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-321">From an interface type to `Object`.</span></span>

* <span data-ttu-id="c2b88-322">从接口类型到与变体兼容的接口类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-322">From an interface type to a variant compatible interface type.</span></span>

* <span data-ttu-id="c2b88-323">从委托类型到与变体兼容的委托类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-323">From a delegate type to a variant compatible delegate type.</span></span> <span data-ttu-id="c2b88-324">（__注意：__</span><span class="sxs-lookup"><span data-stu-id="c2b88-324">(__Note.__</span></span> <span data-ttu-id="c2b88-325">许多其他引用转换都是由这些规则隐式的。</span><span class="sxs-lookup"><span data-stu-id="c2b88-325">Many other reference conversions are implied by these rules.</span></span> <span data-ttu-id="c2b88-326">例如，匿名委托是继承自 @no__t 的引用类型;数组类型是继承自 `System.Array` 的引用类型;匿名类型是继承自 @no__t 的引用类型。）</span><span class="sxs-lookup"><span data-stu-id="c2b88-326">For example, anonymous delegates are reference types that inherit from `System.MulticastDelegate`; array types are reference types that inherit from `System.Array`; anonymous types are reference types that inherit from `System.Object`.)</span></span>

<span data-ttu-id="c2b88-327">__匿名委托转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-327">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="c2b88-328">从为 lambda 方法生成的匿名委托类型重新分类为任何更宽的委托类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-328">From an anonymous delegate type generated for a lambda method reclassification to any wider delegate type.</span></span>

<span data-ttu-id="c2b88-329">__数组转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-329">__Array conversions__</span></span>

* <span data-ttu-id="c2b88-330">在数组类型中 `S`，元素类型 `Se` 到数组类型 `T`，元素类型为 `Te`，前提是满足以下所有条件：</span><span class="sxs-lookup"><span data-stu-id="c2b88-330">From an array type `S` with an element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="c2b88-331">`S` 和 `T` 仅不同于元素类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-331">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="c2b88-332">@No__t-0 和 @no__t 均为引用类型，或者是已知为引用类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="c2b88-332">Both `Se` and `Te` are reference types or are type parameters known to be a reference type.</span></span>

  * <span data-ttu-id="c2b88-333">从 `Se` 到 @no__t 的扩大引用、数组或类型参数转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-333">A widening reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="c2b88-334">在数组类型中 `S`，其枚举元素类型 `Se` 到数组类型 `T`，元素类型为 `Te`，前提是满足以下所有条件：</span><span class="sxs-lookup"><span data-stu-id="c2b88-334">From an array type `S` with an enumerated element type `Se` to an array type `T` with an element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="c2b88-335">`S` 和 `T` 仅不同于元素类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-335">`S` and `T` differ only in element type.</span></span>

  * <span data-ttu-id="c2b88-336">@no__t 为 @no__t 的基础类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-336">`Te` is the underlying type of `Se`.</span></span>

* <span data-ttu-id="c2b88-337">如果满足以下条件之一，则从数组类型 @no__t 第0个，其中枚举元素类型为 `Se`，`System.Collections.Generic.IList(Of Te)`，`IReadOnlyList(Of Te)`，`ICollection(Of Te)`，`IReadOnlyCollection(Of Te)`，和 `IEnumerable(Of Te)`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-337">From an array type `S` of rank 1 with an enumerated element type `Se`, to `System.Collections.Generic.IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)`, and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="c2b88-338">@No__t-0 和 @no__t 均为引用类型，或者是已知为引用类型的类型参数，并且存在从 `Se` 到 `Te` 的扩大引用、数组或类型参数转换。或</span><span class="sxs-lookup"><span data-stu-id="c2b88-338">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a widening reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>

  * <span data-ttu-id="c2b88-339">@no__t 为 @no__t 的基础类型，则为-1;或</span><span class="sxs-lookup"><span data-stu-id="c2b88-339">`Te` is the underlying type of `Se`; or</span></span>

  * <span data-ttu-id="c2b88-340">`Te` 等于 `Se`</span><span class="sxs-lookup"><span data-stu-id="c2b88-340">`Te` is identical to `Se`</span></span>

<span data-ttu-id="c2b88-341">__值类型转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-341">__Value Type conversions__</span></span>

* <span data-ttu-id="c2b88-342">从值类型到基类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-342">From a value type to a base type.</span></span>

* <span data-ttu-id="c2b88-343">从值类型到类型实现的接口类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-343">From a value type to an interface type that the type implements.</span></span>

<span data-ttu-id="c2b88-344">__可以为 null 的值类型转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-344">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="c2b88-345">从类型 `T` 到类型 `T?`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-345">From a type `T` to the type `T?`.</span></span>

* <span data-ttu-id="c2b88-346">从类型 `T?` 到类型 `S?`，其中存在从类型 `T` 到类型  的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-346">From a type `T?` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="c2b88-347">从类型 `T` 到类型 `S?`，其中存在从类型 `T` 到类型  的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-347">From a type `T` to a type `S?`, where there is a widening conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="c2b88-348">从类型 `T?` 到类型 `T` 实现的接口类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-348">From a type `T?` to an interface type that the type `T` implements.</span></span>

<span data-ttu-id="c2b88-349">__字符串转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-349">__String conversions__</span></span>

* <span data-ttu-id="c2b88-350">从 `Char` 到 `String`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-350">From `Char` to `String`.</span></span>

* <span data-ttu-id="c2b88-351">从 `Char()` 到 `String`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-351">From `Char()` to `String`.</span></span>

<span data-ttu-id="c2b88-352">__类型参数转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-352">__Type Parameter conversions__</span></span>

* <span data-ttu-id="c2b88-353">从类型参数到 `Object`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-353">From a type parameter to `Object`.</span></span>

* <span data-ttu-id="c2b88-354">从类型参数到接口类型约束或任何与接口类型约束兼容的接口变体。</span><span class="sxs-lookup"><span data-stu-id="c2b88-354">From a type parameter to an interface type constraint or any interface variant compatible with an interface type constraint.</span></span>

* <span data-ttu-id="c2b88-355">从类型参数到由类约束实现的接口。</span><span class="sxs-lookup"><span data-stu-id="c2b88-355">From a type parameter to an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="c2b88-356">从类型参数到与类约束实现的接口类型兼容的接口变体。</span><span class="sxs-lookup"><span data-stu-id="c2b88-356">From a type parameter to an interface variant compatible with an interface implemented by a class constraint.</span></span>

* <span data-ttu-id="c2b88-357">从类型参数到类约束或类约束的基类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-357">From a type parameter to a class constraint, or a base type of the class constraint.</span></span>

* <span data-ttu-id="c2b88-358">从类型参数 `T` 到类型参数约束 @no__t 为-1，或 `Tx` 的任何内容都是到的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-358">From a type parameter `T` to a type parameter constraint `Tx`, or anything `Tx` has a widening conversion to.</span></span>

## <a name="narrowing-conversions"></a><span data-ttu-id="c2b88-359">收缩转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-359">Narrowing Conversions</span></span>

<span data-ttu-id="c2b88-360">收缩转换是指无法证明始终成功、已知的转换可能会丢失信息的转换，以及跨类型的域之间的转换非常不同于采用收缩表示法。</span><span class="sxs-lookup"><span data-stu-id="c2b88-360">Narrowing conversions are conversions that cannot be proved to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit narrowing notation.</span></span> <span data-ttu-id="c2b88-361">下面的转换归类为收缩转换：</span><span class="sxs-lookup"><span data-stu-id="c2b88-361">The following conversions are classified as narrowing conversions:</span></span>

<span data-ttu-id="c2b88-362">__布尔值转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-362">__Boolean conversions__</span></span>

* <span data-ttu-id="c2b88-363">从 `Boolean` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Decimal`，0，或 1。</span><span class="sxs-lookup"><span data-stu-id="c2b88-363">From `Boolean` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="c2b88-364">From `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Decimal`，`Single`，或者 0 到 1。</span><span class="sxs-lookup"><span data-stu-id="c2b88-364">From `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double` to `Boolean`.</span></span>

<span data-ttu-id="c2b88-365">__数值转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-365">__Numeric conversions__</span></span>

* <span data-ttu-id="c2b88-366">从 `Byte` 到 `SByte`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-366">From `Byte` to `SByte`.</span></span>

* <span data-ttu-id="c2b88-367">从 `SByte` 到 `Byte`、`UShort`、`UInteger` 或 @no__t 为4。</span><span class="sxs-lookup"><span data-stu-id="c2b88-367">From `SByte` to `Byte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="c2b88-368">从 `UShort` 到 `Byte`、`SByte` 或 @no__t 为3。</span><span class="sxs-lookup"><span data-stu-id="c2b88-368">From `UShort` to `Byte`, `SByte`, or `Short`.</span></span>

* <span data-ttu-id="c2b88-369">从 `Short` 到 `Byte`，`SByte`、`UShort`、`UInteger` 或 @no__t 5。</span><span class="sxs-lookup"><span data-stu-id="c2b88-369">From `Short` to `Byte`, `SByte`, `UShort`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="c2b88-370">从 `UInteger` 到 `Byte`，`SByte`、`UShort`、`Short` 或 @no__t 5。</span><span class="sxs-lookup"><span data-stu-id="c2b88-370">From `UInteger` to `Byte`, `SByte`, `UShort`, `Short`, or `Integer`.</span></span>

* <span data-ttu-id="c2b88-371">从 `Integer` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，或者 `ULong`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-371">From `Integer` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, or `ULong`.</span></span>

* <span data-ttu-id="c2b88-372">从 `ULong` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer` 或 @no__t 7。</span><span class="sxs-lookup"><span data-stu-id="c2b88-372">From `ULong` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `Long`.</span></span>

* <span data-ttu-id="c2b88-373">从 `Long` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer` 或 @no__t 7。</span><span class="sxs-lookup"><span data-stu-id="c2b88-373">From `Long` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, or `ULong`.</span></span>

* <span data-ttu-id="c2b88-374">From `Decimal` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，或 `Long`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-374">From `Decimal` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, or `Long`.</span></span>

* <span data-ttu-id="c2b88-375">从 `Single` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，或 @no__t 9。</span><span class="sxs-lookup"><span data-stu-id="c2b88-375">From `Single` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or `Decimal`.</span></span>

* <span data-ttu-id="c2b88-376">从 `Double` 到 `Byte`，`SByte`，`UShort`，`Short`，`UInteger`，`Integer`，`ULong`，`Long`，`Decimal` 或 0。</span><span class="sxs-lookup"><span data-stu-id="c2b88-376">From `Double` to `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, or `Single`.</span></span>

* <span data-ttu-id="c2b88-377">从数值类型到枚举类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-377">From a numeric type to an enumerated type.</span></span>

* <span data-ttu-id="c2b88-378">从枚举类型到数值类型，其基础数值类型的收缩转换为。</span><span class="sxs-lookup"><span data-stu-id="c2b88-378">From an enumerated type to a numeric type its underlying numeric type has a narrowing conversion to.</span></span>

* <span data-ttu-id="c2b88-379">从枚举类型到另一个枚举类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-379">From an enumerated type to another enumerated type.</span></span>

<span data-ttu-id="c2b88-380">__引用转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-380">__Reference conversions__</span></span>

* <span data-ttu-id="c2b88-381">从引用类型到派生程度更高的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-381">From a reference type to a more derived type.</span></span>

* <span data-ttu-id="c2b88-382">从类类型到接口类型，前提是类类型不实现接口类型或与之兼容的接口类型 variant。</span><span class="sxs-lookup"><span data-stu-id="c2b88-382">From a class type to an interface type, provided the class type does not implement the interface type or an interface type variant compatible with it.</span></span>

* <span data-ttu-id="c2b88-383">从接口类型到类类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-383">From an interface type to a class type.</span></span>

* <span data-ttu-id="c2b88-384">从接口类型到另一个接口类型，但前提是这两个类型之间没有继承关系，并且提供它们不是变体兼容的。</span><span class="sxs-lookup"><span data-stu-id="c2b88-384">From an interface type to another interface type, provided there is no inheritance relationship between the two types and provided they are not variant compatible.</span></span>

<span data-ttu-id="c2b88-385">__匿名委托转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-385">__Anonymous Delegate conversions__</span></span>

* <span data-ttu-id="c2b88-386">从为 lambda 方法生成的匿名委托类型重新分类为任何较窄的委托类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-386">From an anonymous delegate type generated for a lambda method reclassification to any narrower delegate type.</span></span>

<span data-ttu-id="c2b88-387">__数组转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-387">__Array conversions__</span></span>

* <span data-ttu-id="c2b88-388">从数组类型 `S`，元素类型 `Se`）到数组类型 `T`，元素类型为 `Te`，前提是满足以下所有条件：</span><span class="sxs-lookup"><span data-stu-id="c2b88-388">From an array type `S` with an element type `Se`, to an array type `T` with an element type `Te`, provided that all of the following are true:</span></span>

  * <span data-ttu-id="c2b88-389">`S` 和 `T` 仅不同于元素类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-389">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="c2b88-390">@No__t-0 和 @no__t 均为引用类型，或者是不被称为值类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="c2b88-390">Both `Se` and `Te` are reference types or are type parameters not known to be value types.</span></span>
  * <span data-ttu-id="c2b88-391">从 `Se` 到 @no__t 的收缩引用、数组或类型参数转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-391">A narrowing reference, array, or type parameter conversion exists from `Se` to `Te`.</span></span>

* <span data-ttu-id="c2b88-392">在数组类型中 `S`，元素类型 `Se` 到数组类型 `T`，其中枚举元素类型为 `Te`，前提是满足以下所有条件：</span><span class="sxs-lookup"><span data-stu-id="c2b88-392">From an array type `S` with an element type `Se` to an array type `T` with an enumerated element type `Te`, provided all of the following are true:</span></span>

  * <span data-ttu-id="c2b88-393">`S` 和 `T` 仅不同于元素类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-393">`S` and `T` differ only in element type.</span></span>
  * <span data-ttu-id="c2b88-394">@no__t 为 @no__t 的基础类型，或者它们都是共享同一基础类型的不同枚举类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-394">`Se` is the underlying type of `Te` , or they are both different enumerated types that share the same underlying type.</span></span>

* <span data-ttu-id="c2b88-395">如果满足以下条件之一，则从数组类型 `S`，其中枚举元素类型为 `Se`，`IList(Of Te)`，`IReadOnlyList(Of Te)`，`ICollection(Of Te)`，`IReadOnlyCollection(Of Te)`，`IEnumerable(Of Te)`）：</span><span class="sxs-lookup"><span data-stu-id="c2b88-395">From an array type `S` of rank 1 with an enumerated element type `Se`, to `IList(Of Te)`, `IReadOnlyList(Of Te)`, `ICollection(Of Te)`, `IReadOnlyCollection(Of Te)` and `IEnumerable(Of Te)`, provided one of the following is true:</span></span>

  * <span data-ttu-id="c2b88-396">@No__t-0 和 @no__t 均为引用类型，或者是已知为引用类型的类型参数，并且存在从 `Se` 到 `Te` 的收缩引用、数组或类型参数转换;或</span><span class="sxs-lookup"><span data-stu-id="c2b88-396">Both `Se` and `Te` are reference types or are type parameters known to be a reference type, and a narrowing reference, array, or type parameter conversion exists from `Se` to `Te`; or</span></span>
  * <span data-ttu-id="c2b88-397">@no__t 为 @no__t 的基础类型，或者它们都是共享同一基础类型的不同枚举类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-397">`Se` is the underlying type of `Te`, or they are both different enumerated types that share the same underlying type.</span></span>

<span data-ttu-id="c2b88-398">__值类型转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-398">__Value type conversions__</span></span>

* <span data-ttu-id="c2b88-399">从引用类型到派生程度更高的值类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-399">From a reference type to a more derived value type.</span></span>

* <span data-ttu-id="c2b88-400">从接口类型到值类型（如果值类型实现接口类型）。</span><span class="sxs-lookup"><span data-stu-id="c2b88-400">From an interface type to a value type, provided the value type implements the interface type.</span></span>

<span data-ttu-id="c2b88-401">__可以为 null 的值类型转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-401">__Nullable Value Type conversions__</span></span>

* <span data-ttu-id="c2b88-402">从类型 `T?` 到类型 `T`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-402">From a type `T?` to a type `T`.</span></span>

* <span data-ttu-id="c2b88-403">从类型 `T?` 到类型 `S?`，其中存在从类型 `T` 到类型  的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-403">From a type `T?` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="c2b88-404">从类型 `T` 到类型 `S?`，其中存在从类型 `T` 到类型  的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-404">From a type `T` to a type `S?`, where there is a narrowing conversion from the type `T` to the type `S`.</span></span>

* <span data-ttu-id="c2b88-405">从类型 `S?` 到类型 `T`，其中存在从类型 `S` 到类型  的转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-405">From a type `S?` to a type `T`, where there is a conversion from the type `S` to the type `T`.</span></span>

<span data-ttu-id="c2b88-406">__字符串转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-406">__String conversions__</span></span>

* <span data-ttu-id="c2b88-407">从 `String` 到 `Char`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-407">From `String` to `Char`.</span></span>

* <span data-ttu-id="c2b88-408">从 `String` 到 `Char()`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-408">From `String` to `Char()`.</span></span>

* <span data-ttu-id="c2b88-409">从 `String` 到 `Boolean` 和从 `Boolean` 到 @no__t。</span><span class="sxs-lookup"><span data-stu-id="c2b88-409">From `String` to `Boolean` and from `Boolean` to `String`.</span></span>

* <span data-ttu-id="c2b88-410">@No__t-0 与 @no__t 之间的转换、`SByte`、`UShort`、`Short`、@no__t、@no__t、@no__t、@no__t、@no__t、@no__t、@no__t、或为。</span><span class="sxs-lookup"><span data-stu-id="c2b88-410">Conversions between `String` and `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, `Decimal`, `Single`, or `Double`.</span></span>

* <span data-ttu-id="c2b88-411">从 `String` 到 `Date` 和从 `Date` 到 @no__t。</span><span class="sxs-lookup"><span data-stu-id="c2b88-411">From `String` to `Date` and from `Date` to `String`.</span></span>

<span data-ttu-id="c2b88-412">__类型参数转换__</span><span class="sxs-lookup"><span data-stu-id="c2b88-412">__Type Parameter conversions__</span></span>

* <span data-ttu-id="c2b88-413">从 `Object` 到类型参数。</span><span class="sxs-lookup"><span data-stu-id="c2b88-413">From `Object` to a type parameter.</span></span>

* <span data-ttu-id="c2b88-414">从类型参数到接口类型，前提是类型参数不受限于该接口或约束为实现该接口的类。</span><span class="sxs-lookup"><span data-stu-id="c2b88-414">From a type parameter to an interface type, provided the type parameter is not constrained to that interface or constrained to a class that implements that interface.</span></span>

* <span data-ttu-id="c2b88-415">从接口类型到类型参数。</span><span class="sxs-lookup"><span data-stu-id="c2b88-415">From an interface type to a type parameter.</span></span>

* <span data-ttu-id="c2b88-416">从类型参数到类约束的派生类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-416">From a type parameter to a derived type of a class constraint.</span></span>

* <span data-ttu-id="c2b88-417">从类型参数 `T` 到类型参数约束的任何内容 `Tx` 将收缩转换为。</span><span class="sxs-lookup"><span data-stu-id="c2b88-417">From a type parameter `T` to anything a type parameter constraint `Tx` has a narrowing conversion to.</span></span>

## <a name="type-parameter-conversions"></a><span data-ttu-id="c2b88-418">类型参数转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-418">Type Parameter Conversions</span></span>

<span data-ttu-id="c2b88-419">类型参数的转换由约束（如果有）决定。</span><span class="sxs-lookup"><span data-stu-id="c2b88-419">Type parameters' conversions are determined by the constraints, if any, put on them.</span></span> <span data-ttu-id="c2b88-420">类型参数 `T` 始终可以从 `Object` 转换为自身，以及从任何接口类型转换为和。</span><span class="sxs-lookup"><span data-stu-id="c2b88-420">A type parameter `T` can always be converted to itself, to and from `Object`, and to and from any interface type.</span></span> <span data-ttu-id="c2b88-421">请注意，如果类型 @no__t 为在运行时的值类型，则从 `T` 转换为 `Object` 或接口类型将为装箱转换，并从 @no__t 3 转换或接口类型转换为 @no__t。</span><span class="sxs-lookup"><span data-stu-id="c2b88-421">Note that if the type `T` is a value type at run-time, converting from `T` to `Object` or an interface type will be a boxing conversion and converting from `Object` or an interface type to `T` will be an unboxing conversion.</span></span> <span data-ttu-id="c2b88-422">类约束 `C` 的类型参数定义了从类型参数到 `C` 及其基类的其他转换，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="c2b88-422">A type parameter with a class constraint `C` defines additional conversions from the type parameter to `C` and its base classes, and vice versa.</span></span> <span data-ttu-id="c2b88-423">类型参数 `T` 与类型参数约束一起使用 `Tx` 定义转换为 `Tx` 和 @no__t 转换为的任何内容。</span><span class="sxs-lookup"><span data-stu-id="c2b88-423">A type parameter `T` with a type parameter constraint `Tx` defines a conversion to `Tx` and anything `Tx` converts to.</span></span>

<span data-ttu-id="c2b88-424">如果类型参数还具有 @no__t 2 或类约束，则其元素类型为具有接口约束 `I` 的类型形参具有相同的协变数组 @no__t 转换，因为该类型参数还具有2或类约束（因为仅引用数组元素类型可以是协变的。</span><span class="sxs-lookup"><span data-stu-id="c2b88-424">An array whose element type is a type parameter with an interface constraint `I` has the same covariant array conversions as an array whose element type is `I`, provided that the type parameter also has a `Class` or class constraint (since only reference array element types can be covariant).</span></span> <span data-ttu-id="c2b88-425">其元素类型为类型参数 @no__t 为-0 的类型参数与元素类型为 `C` 的数组具有相同的协变数组转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-425">An array whose element type is a type parameter with a class constraint `C` has the same covariant array conversions as an array whose element type is `C`.</span></span>

<span data-ttu-id="c2b88-426">上述转换规则不允许将不受约束的类型参数转换为非接口类型，这可能会令人吃惊。</span><span class="sxs-lookup"><span data-stu-id="c2b88-426">The above conversions rules do not permit conversions from unconstrained type parameters to non-interface types, which may be surprising.</span></span> <span data-ttu-id="c2b88-427">这样做的原因是为了防止混淆此类转换的语义。</span><span class="sxs-lookup"><span data-stu-id="c2b88-427">The reason for this is to prevent confusion about the semantics of such conversions.</span></span> <span data-ttu-id="c2b88-428">例如，请考虑以下声明：</span><span class="sxs-lookup"><span data-stu-id="c2b88-428">For example, consider the following declaration:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long 
        Return CLng(t)    ' Error, explicit conversion not permitted
    End Function
End Class
```

<span data-ttu-id="c2b88-429">如果允许 `T` 到 `Integer` 的转换，则很可能会认为 `X(Of Integer).F(7)` 返回 `7L`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-429">If the conversion of `T` to `Integer` were permitted, one might easily expect that `X(Of Integer).F(7)` would return `7L`.</span></span> <span data-ttu-id="c2b88-430">但是，它不会，因为只有在编译时已知类型为数字的情况下才考虑数值转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-430">However, it would not, because numeric conversions are only considered when the types are known to be numeric at compile time.</span></span> <span data-ttu-id="c2b88-431">为了使语义清晰明了，必须改为编写以上示例：</span><span class="sxs-lookup"><span data-stu-id="c2b88-431">In order to make the semantics clear, the above example must instead be written:</span></span>

```vb
Class X(Of T)
    Public Shared Function F(t As T) As Long
        Return CLng(CObj(t))    ' OK, conversions permitted
    End Function
End Class
```

## <a name="user-defined-conversions"></a><span data-ttu-id="c2b88-432">用户定义的转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-432">User-Defined Conversions</span></span>

<span data-ttu-id="c2b88-433">*内部转换*是由语言（即在此规范中列出）定义的转换，而*用户定义的转换*是通过重载 @no__t 2 运算符来定义的。</span><span class="sxs-lookup"><span data-stu-id="c2b88-433">*Intrinsic conversions* are conversions defined by the language (i.e. listed in this specification), while *user-defined conversions* are defined by overloading the `CType` operator.</span></span> <span data-ttu-id="c2b88-434">在类型之间进行转换时，如果没有任何内部转换适用，则将考虑用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-434">When converting between types, if no intrinsic conversions are applicable then user-defined conversions will be considered.</span></span> <span data-ttu-id="c2b88-435">如果存在*最*适用于源和目标类型的用户定义的转换，则将使用用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-435">If there is a user-defined conversion that is *most specific* for the source and target types, then the user-defined conversion will be used.</span></span> <span data-ttu-id="c2b88-436">否则，将产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="c2b88-436">Otherwise, a compile-time error results.</span></span> <span data-ttu-id="c2b88-437">最具体的转换是操作数与源类型 "最接近" 的转换，并且其结果类型与目标类型 "最近"。</span><span class="sxs-lookup"><span data-stu-id="c2b88-437">The most specific conversion is the one whose operand is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="c2b88-438">确定要使用的用户定义的转换时，将使用最特定的扩大转换;如果没有最具体的扩大转换，将使用最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-438">When determining what user-defined conversion to use, the most specific widening conversion will be used; if no widening conversion is most specific, the most specific narrowing conversion will be used.</span></span> <span data-ttu-id="c2b88-439">如果没有最具体的收缩转换，则转换是不确定的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="c2b88-439">If there is no most specific narrowing conversion, then the conversion is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="c2b88-440">以下部分介绍如何确定最具体的转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-440">The following sections cover how the most specific conversions are determined.</span></span> <span data-ttu-id="c2b88-441">它们使用以下术语：</span><span class="sxs-lookup"><span data-stu-id="c2b88-441">They use the following terms:</span></span>

<span data-ttu-id="c2b88-442">如果存在从类型 `A` 到类型 `B` 的内部扩大转换，并且如果 @no__t @no__t 两者都不为接口，则 *@no__t 为 @no__t，`B`* *包含*`A`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-442">If an intrinsic widening conversion exists from a type `A` to a type `B`, and if neither `A` nor `B` are interfaces, then `A` is *encompassed* by `B`, and `B` *encompasses* `A`.</span></span>

<span data-ttu-id="c2b88-443">一组类型中包含的*最大*的类型是一种包含集内所有其他类型的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-443">The *most encompassing* type in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="c2b88-444">如果没有一种类型包含所有其他类型，则该集没有最大的包含类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-444">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="c2b88-445">直观地说，最包含的类型是集中的 "最大" 类型，这种类型是每个其他类型可通过扩大转换进行转换的一种类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-445">In intuitive terms, the most encompassing type is the "largest" type in the set -- the one type to which each of the other types can be converted through a widening conversion.</span></span>

<span data-ttu-id="c2b88-446">类型集中*最常被包含*的类型是一种类型，它由集中的所有其他类型所包含。</span><span class="sxs-lookup"><span data-stu-id="c2b88-446">The *most encompassed* type in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="c2b88-447">如果所有其他类型都不包含任何一种类型，则该集不包含大多数被包含的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-447">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="c2b88-448">简而言之，最常被包含的类型是集合中的 "最小" 类型，一种类型可以通过收缩转换转换为其他类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-448">In intuitive terms, the most encompassed type is the "smallest" type in the set -- the one type that can be converted to each of the other types through a narrowing conversion.</span></span>

<span data-ttu-id="c2b88-449">收集 @no__t 为类型的候选用户定义的转换时，将改用 @no__t 定义的用户定义的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-449">When collecting the candidate user-defined conversions for a type `T?`, the user-defined conversion operators defined by `T` are used instead.</span></span> <span data-ttu-id="c2b88-450">如果要转换为的类型也是可以为 null 的值类型，则会提升仅涉及不可为 null 的值类型的 @no__t 0 的用户定义转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-450">If the type being converted to is also a nullable value type, then any of `T`'s user-defined conversions operators that involve only non-nullable value types are lifted.</span></span> <span data-ttu-id="c2b88-451">从 `T` 到 `S` 的转换运算符被提升为从 `T?` 到 `S?` 的转换，并通过将 @no__t 转换为 `T` （如有必要），然后将用户定义的转换运算符从 @no__t 6 转换到 `S`，然后如有必要，将 `S` 转换为 `S?`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-451">A conversion operator from `T` to `S` is lifted to be a conversion from `T?` to `S?` and is evaluated by converting `T?` to `T`, if necessary, then evaluating the user-defined conversion operator from `T` to `S` and then converting `S` to `S?`, if necessary.</span></span> <span data-ttu-id="c2b88-452">但是，如果要转换的值为 `Nothing`，则提升转换运算符会直接转换为类型为 `Nothing` 的值，类型为 `S?`。</span><span class="sxs-lookup"><span data-stu-id="c2b88-452">If the value being converted is `Nothing`, however, a lifted conversion operator converts directly into a value of `Nothing` typed as `S?`.</span></span> <span data-ttu-id="c2b88-453">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-453">For example:</span></span>

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

<span data-ttu-id="c2b88-454">解析转换时，用户定义的转换运算符始终优于提升转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-454">When resolving conversions, user-defined conversions operators are always preferred over lifted conversion operators.</span></span> <span data-ttu-id="c2b88-455">例如：</span><span class="sxs-lookup"><span data-stu-id="c2b88-455">For example:</span></span>

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

<span data-ttu-id="c2b88-456">在运行时，评估用户定义的转换最多可以涉及三个步骤：</span><span class="sxs-lookup"><span data-stu-id="c2b88-456">At run-time, evaluating a user-defined conversion can involve up to three steps:</span></span>

1. <span data-ttu-id="c2b88-457">首先，根据需要，使用内部转换将值从源类型转换为操作数类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-457">First, the value is converted from the source type to the operand type using an intrinsic conversion, if necessary.</span></span>

2. <span data-ttu-id="c2b88-458">然后，调用用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-458">Then, the user-defined conversion is invoked.</span></span>

3. <span data-ttu-id="c2b88-459">最后，如果需要，用户定义的转换的结果将使用内部转换转换为目标类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-459">Finally, the result of the user-defined conversion is converted to the target type using an intrinsic conversion, if necessary.</span></span>

<span data-ttu-id="c2b88-460">务必要注意，用户定义的转换的计算决不会涉及多个用户定义的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-460">It is important to note that evaluation of a user-defined conversion will never involve more than one user-defined conversion operator.</span></span>

### <a name="most-specific-widening-conversion"></a><span data-ttu-id="c2b88-461">最特定的扩大转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-461">Most Specific Widening Conversion</span></span>

<span data-ttu-id="c2b88-462">使用以下步骤来确定两种类型之间最特定的用户定义扩大转换运算符：</span><span class="sxs-lookup"><span data-stu-id="c2b88-462">Determining the most specific user-defined widening conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="c2b88-463">首先，收集所有候选转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-463">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="c2b88-464">候选转换运算符是源类型中所有用户定义的扩大转换运算符，以及目标类型中所有用户定义的扩大转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-464">The candidate conversion operators are all of the user-defined widening conversion operators in the source type and all of the user-defined widening conversion operators in the target type.</span></span>

2. <span data-ttu-id="c2b88-465">然后，将从该集中删除所有不适用的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-465">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="c2b88-466">如果存在从源类型到操作数类型的内部扩大转换运算符，并且存在从运算符结果到目标类型的内部扩大转换运算符，则转换运算符适用于源类型和目标类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-466">A conversion operator is applicable to a source type and target type if there is an intrinsic widening conversion operator from the source type to the operand type and there is an intrinsic widening conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="c2b88-467">如果没有适用的转换运算符，则没有最特定的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-467">If there are no applicable conversion operators, then there is no most specific widening conversion.</span></span>

3. <span data-ttu-id="c2b88-468">然后，确定适用转换运算符的最特定源类型：</span><span class="sxs-lookup"><span data-stu-id="c2b88-468">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="c2b88-469">如果任何转换运算符直接从源类型转换，则源类型将是最具体的源类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-469">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="c2b88-470">否则，最特定的源类型是转换运算符的组合源类型集中最常被包含的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-470">Otherwise, the most specific source type is the most encompassed type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="c2b88-471">如果找不到最能包含的类型，则没有最特定的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-471">If no most encompassed type can be found, then there is no most specific widening conversion.</span></span>

4. <span data-ttu-id="c2b88-472">然后，确定适用转换运算符的最特定目标类型：</span><span class="sxs-lookup"><span data-stu-id="c2b88-472">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="c2b88-473">如果任何转换运算符直接转换为目标类型，则目标类型为最特定的目标类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-473">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="c2b88-474">否则，最特定目标类型是转换运算符的组合目标类型集中最包含的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-474">Otherwise, the most specific target type is the most encompassing type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="c2b88-475">如果找不到大多数包含的类型，则没有最特定的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-475">If no most encompassing type can be found, then there is no most specific widening conversion.</span></span>

5. <span data-ttu-id="c2b88-476">然后，如果只有一个转换运算符从最具体的源类型转换为最具体的目标类型，则这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-476">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="c2b88-477">如果存在多个这样的运算符，则没有最特定的扩大转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-477">If more than one such operator exists, then there is no most specific widening conversion.</span></span>

### <a name="most-specific-narrowing-conversion"></a><span data-ttu-id="c2b88-478">最特定的收缩转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-478">Most Specific Narrowing Conversion</span></span>

<span data-ttu-id="c2b88-479">使用以下步骤来确定两种类型之间最特定的用户定义的收缩转换运算符：</span><span class="sxs-lookup"><span data-stu-id="c2b88-479">Determining the most specific user-defined narrowing conversion operator between two types is accomplished using the following steps:</span></span>

1. <span data-ttu-id="c2b88-480">首先，收集所有候选转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-480">First, all of the candidate conversion operators are collected.</span></span> <span data-ttu-id="c2b88-481">候选转换运算符是源类型中所有用户定义的转换运算符，以及目标类型中所有用户定义的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-481">The candidate conversion operators are all of the user-defined conversion operators in the source type and all of the user-defined conversion operators in the target type.</span></span>

2. <span data-ttu-id="c2b88-482">然后，将从该集中删除所有不适用的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-482">Then, all non-applicable conversion operators are removed from the set.</span></span> <span data-ttu-id="c2b88-483">如果存在从源类型到操作数类型的内部转换运算符，并且存在从运算符结果到目标类型的内部转换运算符，则转换运算符适用于源类型和目标类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-483">A conversion operator is applicable to a source type and target type if there is an intrinsic conversion operator from the source type to the operand type and there is an intrinsic conversion operator from the result of the operator to the target type.</span></span> <span data-ttu-id="c2b88-484">如果没有适用的转换运算符，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-484">If there are no applicable conversion operators, then there is no most specific narrowing conversion.</span></span>

3. <span data-ttu-id="c2b88-485">然后，确定适用转换运算符的最特定源类型：</span><span class="sxs-lookup"><span data-stu-id="c2b88-485">Then, the most specific source type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="c2b88-486">如果任何转换运算符直接从源类型转换，则源类型将是最具体的源类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-486">If any of the conversion operators convert directly from the source type, then the source type is the most specific source type.</span></span>

   * <span data-ttu-id="c2b88-487">否则，如果任何转换运算符从包含源类型的类型转换，则最具体的源类型是那些转换运算符的一组源类型中包含程度最高的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-487">Otherwise, if any of the conversion operators convert from types that encompass the source type, then the most specific source type is the most encompassed type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="c2b88-488">如果找不到最能包含的类型，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-488">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="c2b88-489">否则，最特定的源类型是转换运算符的组合源类型集中最包含的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-489">Otherwise, the most specific source type is the most encompassing type in the combined set of source types of the conversion operators.</span></span> <span data-ttu-id="c2b88-490">如果找不到大多数包含的类型，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-490">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

4. <span data-ttu-id="c2b88-491">然后，确定适用转换运算符的最特定目标类型：</span><span class="sxs-lookup"><span data-stu-id="c2b88-491">Then, the most specific target type of the applicable conversion operators is determined:</span></span>

   * <span data-ttu-id="c2b88-492">如果任何转换运算符直接转换为目标类型，则目标类型为最特定的目标类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-492">If any of the conversion operators convert directly to the target type, then the target type is the most specific target type.</span></span>

   * <span data-ttu-id="c2b88-493">否则，如果任何转换运算符转换为目标类型所包含的类型，则最特定目标类型是这些转换运算符的组合源类型集中最包含的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-493">Otherwise, if any of the conversion operators convert to types that are encompassed by the target type, then the most specific target type is the most encompassing type in the combined set of source types of those conversion operators.</span></span> <span data-ttu-id="c2b88-494">如果找不到大多数包含的类型，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-494">If no most encompassing type can be found, then there is no most specific narrowing conversion.</span></span>

   * <span data-ttu-id="c2b88-495">否则，最特定目标类型是转换运算符的组合目标类型集中最常被包含的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-495">Otherwise, the most specific target type is the most encompassed type in the combined set of target types of the conversion operators.</span></span> <span data-ttu-id="c2b88-496">如果找不到最能包含的类型，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-496">If no most encompassed type can be found, then there is no most specific narrowing conversion.</span></span>

5. <span data-ttu-id="c2b88-497">然后，如果只有一个转换运算符从最具体的源类型转换为最具体的目标类型，则这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="c2b88-497">Then, if exactly one conversion operator converts from the most specific source type to the most specific target type, then this is the most specific conversion operator.</span></span> <span data-ttu-id="c2b88-498">如果存在多个这样的运算符，则没有最具体的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-498">If more than one such operator exists, then there is no most specific narrowing conversion.</span></span>

## <a name="native-conversions"></a><span data-ttu-id="c2b88-499">本机转换</span><span class="sxs-lookup"><span data-stu-id="c2b88-499">Native Conversions</span></span>

<span data-ttu-id="c2b88-500">一些转换归类为*本机转换*，因为这些转换在 .NET Framework 本机支持。</span><span class="sxs-lookup"><span data-stu-id="c2b88-500">Several of the conversions are classified as *native conversions* because they are supported natively by the .NET Framework.</span></span> <span data-ttu-id="c2b88-501">这些转换是可通过使用 @no__t 0 和 @no__t 转换运算符以及其他特殊行为进行优化的转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-501">These conversions are ones that can be optimized through the use of the `DirectCast` and `TryCast` conversion operators, as well as other special behaviors.</span></span> <span data-ttu-id="c2b88-502">归类为本机转换的转换包括：标识转换、默认转换、引用转换、数组转换、值类型转换和类型参数转换。</span><span class="sxs-lookup"><span data-stu-id="c2b88-502">The conversions classified as native conversions are: identity conversions, default conversions, reference conversions, array conversions, value type conversions, and type parameter conversions.</span></span>

## <a name="dominant-type"></a><span data-ttu-id="c2b88-503">主导类型</span><span class="sxs-lookup"><span data-stu-id="c2b88-503">Dominant Type</span></span>

<span data-ttu-id="c2b88-504">给定一组类型，通常在某些情况下（如类型推理）确定集的*主导类型*是必需的。</span><span class="sxs-lookup"><span data-stu-id="c2b88-504">Given a set of types, it is often necessary in situations such as type inference to determine the *dominant type* of the set.</span></span> <span data-ttu-id="c2b88-505">类型集的主导类型是先删除一个或多个其他类型没有隐式转换为的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-505">The dominant type of a set of types is determined by first removing any types that one or more other types do not have an implicit conversion to.</span></span> <span data-ttu-id="c2b88-506">如果此时没有任何类型，则没有主导类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-506">If there are no types left at this point, there is no dominant type.</span></span> <span data-ttu-id="c2b88-507">然后，主要类型是其余类型中包含的最大的类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-507">The dominant type is then the most encompassed of the remaining types.</span></span> <span data-ttu-id="c2b88-508">如果有多个最常包含的类型，则没有主导类型。</span><span class="sxs-lookup"><span data-stu-id="c2b88-508">If there is more than one type that is most encompassed, then there is no dominant type.</span></span>
