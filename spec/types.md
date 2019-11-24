---
ms.openlocfilehash: 4560eac9f4ab52d07c77724aeca696d0195da91a
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306024"
---
# <a name="types"></a><span data-ttu-id="d2dd3-101">类型</span><span class="sxs-lookup"><span data-stu-id="d2dd3-101">Types</span></span>

<span data-ttu-id="d2dd3-102">Visual Basic 中的类型的两个基本类别为*值类型*和*引用类型*。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-102">The two fundamental categories of types in Visual Basic are *value types* and *reference types*.</span></span> <span data-ttu-id="d2dd3-103">基元类型（字符串除外）、枚举和结构是值类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-103">Primitive types (except strings), enumerations, and structures are value types.</span></span> <span data-ttu-id="d2dd3-104">类、字符串、标准模块、接口、数组和委托是引用类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-104">Classes, strings, standard modules, interfaces, arrays, and delegates are reference types.</span></span>

<span data-ttu-id="d2dd3-105">每个类型都有一个*默认值*，该值是在初始化时分配给该类型的变量的值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-105">Every type has a *default value*, which is the value that is assigned to variables of that type upon initialization.</span></span>

```antlr
TypeName
    : ArrayTypeName
    | NonArrayTypeName
    ;

NonArrayTypeName
    : SimpleTypeName
    | NullableTypeName
    ;

SimpleTypeName
    : QualifiedTypeName
    | BuiltInTypeName
    ;

QualifiedTypeName
    : Identifier TypeArguments? (Period IdentifierOrKeyword TypeArguments?)*
    | 'Global' Period IdentifierOrKeyword TypeArguments?
      (Period IdentifierOrKeyword TypeArguments?)*
    ;

TypeArguments
    : OpenParenthesis 'Of' TypeArgumentList CloseParenthesis
    ;

TypeArgumentList
    : TypeName ( Comma TypeName )*
    ;

BuiltInTypeName
    : 'Object'
    | PrimitiveTypeName
    ;

TypeModifier
    : AccessModifier
    | 'Shadows'
    ;

IdentifierModifiers
    : NullableNameModifier? ArrayNameModifier?
    ;
```

## <a name="value-types-and-reference-types"></a><span data-ttu-id="d2dd3-106">Value Types and Reference Types</span><span class="sxs-lookup"><span data-stu-id="d2dd3-106">Value Types and Reference Types</span></span>

<span data-ttu-id="d2dd3-107">尽管值类型和引用类型在声明语法和用法方面可能类似，但它们的语义不同。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-107">Although value types and reference types can be similar in terms of declaration syntax and usage, their semantics are distinct.</span></span>

<span data-ttu-id="d2dd3-108">引用类型存储在运行时堆上;它们只能通过对该存储的引用进行访问。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-108">Reference types are stored on the run-time heap; they may only be accessed through a reference to that storage.</span></span> <span data-ttu-id="d2dd3-109">由于始终通过引用访问引用类型，因此，它们的生存期由 .NET Framework 管理。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-109">Because reference types are always accessed through references, their lifetime is managed by the .NET Framework.</span></span> <span data-ttu-id="d2dd3-110">将跟踪对特定实例的未完成引用，并且仅当不再有引用保留时，才销毁实例。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-110">Outstanding references to a particular instance are tracked and the instance is destroyed only when no more references remain.</span></span> <span data-ttu-id="d2dd3-111">引用类型的变量包含对该类型的值、更派生的类型的值或 null 值的引用。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-111">A variable of reference type contains a reference to a value of that type, a value of a more derived type, or a null value.</span></span> <span data-ttu-id="d2dd3-112">*空值*表示不是;不能使用 null 值执行任何操作，除非赋值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-112">A *null value* refers to nothing; it is not possible to do anything with a null value except assign it.</span></span> <span data-ttu-id="d2dd3-113">对引用类型的变量赋值会创建引用的副本，而不是引用的值的副本。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-113">Assignment to a variable of a reference type creates a copy of the reference rather than a copy of the referenced value.</span></span> <span data-ttu-id="d2dd3-114">对于引用类型的变量，默认值为 null 值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-114">For a variable of a reference type, the default value is a null value.</span></span>

<span data-ttu-id="d2dd3-115">值类型直接存储在堆栈中或另一类型内;只能直接访问其存储。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-115">Value types are stored directly on the stack, either within an array or within another type; their storage can only be accessed directly.</span></span> <span data-ttu-id="d2dd3-116">由于值类型直接存储在变量中，因此它们的生存期由包含它们的变量的生存期确定。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-116">Because value types are stored directly within variables, their lifetime is determined by the lifetime of the variable that contains them.</span></span> <span data-ttu-id="d2dd3-117">当包含值类型实例的位置被销毁时，还会销毁值类型实例。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-117">When the location containing a value type instance is destroyed, the value type instance is also destroyed.</span></span> <span data-ttu-id="d2dd3-118">始终直接访问值类型;不能创建对值类型的引用。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-118">Value types are always accessed directly; it is not possible to create a reference to a value type.</span></span> <span data-ttu-id="d2dd3-119">禁止此类引用无法引用已销毁的值类实例。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-119">Prohibiting such a reference makes it impossible to refer to a value class instance that has been destroyed.</span></span> <span data-ttu-id="d2dd3-120">由于值类型始终 `NotInheritable`，因此值类型的变量始终包含该类型的值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-120">Because value types are always `NotInheritable`, a variable of a value type always contains a value of that type.</span></span> <span data-ttu-id="d2dd3-121">因此，值类型的值不能为 null 值，也不能引用派生程度更高的类型的对象。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-121">Because of this, the value of a value type cannot be a null value, nor can it reference an object of a more derived type.</span></span> <span data-ttu-id="d2dd3-122">对值类型的变量赋值会创建要分配的值的副本。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-122">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="d2dd3-123">对于值类型的变量，默认值为将类型的每个变量成员初始化为其默认值的结果。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-123">For a variable of a value type, the default value is the result of initializing each variable member of the type to its default value.</span></span>

<span data-ttu-id="d2dd3-124">下面的示例演示引用类型和值类型之间的差异：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-124">The following example shows the difference between reference types and value types:</span></span>

```vb
Class Class1
    Public Value As Integer = 0
End Class

Module Test
    Sub Main()
        Dim val1 As Integer = 0
        Dim val2 As Integer = val1
        val2 = 123
        Dim ref1 As Class1 = New Class1()
        Dim ref2 As Class1 = ref1
        ref2.Value = 123
        Console.WriteLine("Values: " & val1 & ", " & val2)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

<span data-ttu-id="d2dd3-125">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-125">The output of the program is:</span></span>

```console
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="d2dd3-126">对局部变量 `val2` 的赋值不会影响局部变量 `val1`，因为两个局部变量都是值类型（类型 `Integer`），并且值类型的每个局部变量都有其自己的存储。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-126">The assignment to the local variable `val2` does not impact the local variable `val1` because both local variables are of a value type (the type `Integer`) and each local variable of a value type has its own storage.</span></span> <span data-ttu-id="d2dd3-127">与此相反，赋值 `ref2.Value = 123;` 会影响 `ref1` 和 `ref2` 引用的对象。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-127">In contrast, the assignment `ref2.Value = 123;` affects the object that both `ref1` and `ref2` reference.</span></span>

<span data-ttu-id="d2dd3-128">关于 .NET Framework 类型系统需要注意的一点是，尽管结构、枚举和基元类型（`String`除外）都是值类型，但它们都是从引用类型继承的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-128">One thing to note about the .NET Framework type system is that even though structures, enumerations and primitive types (except for `String`) are value types, they all inherit from reference types.</span></span> <span data-ttu-id="d2dd3-129">结构和基元类型继承自引用类型 `System.ValueType`，后者继承自 `Object`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-129">Structures and the primitive types inherit from the reference type `System.ValueType`, which inherits from `Object`.</span></span> <span data-ttu-id="d2dd3-130">枚举类型继承自引用类型 `System.Enum`，后者继承自 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-130">Enumerated types inherit from the reference type `System.Enum`, which inherits from `System.ValueType`.</span></span>

### <a name="nullable-value-types"></a><span data-ttu-id="d2dd3-131">可以为 null 的值类型</span><span class="sxs-lookup"><span data-stu-id="d2dd3-131">Nullable Value Types</span></span>

<span data-ttu-id="d2dd3-132">对于值类型，可以将 `?` 修饰符添加到类型名称，以表示该类型的*可以为 null*的版本。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-132">For value types, a `?` modifier can be added to a type name to represent the *nullable* version of that type.</span></span>

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

<span data-ttu-id="d2dd3-133">可以为 null 的值类型可以包含与类型的不可为 null 的版本相同的值，也可以包含 null 值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-133">A nullable value type can contain the same values as the non-nullable version of the type as well as the null value.</span></span> <span data-ttu-id="d2dd3-134">因此，对于可以为 null 的值类型，将 `Nothing` 分配给类型的变量会将变量的值设置为 null 值，而不是值类型的零值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-134">Thus, for a nullable value type, assigning `Nothing` to a variable of the type sets the value of the variable to the null value, not the zero value of the value type.</span></span> <span data-ttu-id="d2dd3-135">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-135">For example:</span></span>

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

<span data-ttu-id="d2dd3-136">还可以通过将可为 null 的类型修饰符放在变量名上，将变量声明为可以为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-136">A variable may also be declared to be of a nullable value type by putting a nullable type modifier on the variable name.</span></span> <span data-ttu-id="d2dd3-137">为清楚起见，对同一声明中的变量名称和类型名称都有可以为 null 的类型修饰符是无效的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-137">For clarity, it is not valid to have a nullable type modifier on both a variable name and a type name in the same declaration.</span></span> <span data-ttu-id="d2dd3-138">由于可以使用类型 `System.Nullable(Of T)`来实现可以为 null 的类型，因此 `T?` 类型 `System.Nullable(Of T)`，并且两个名称可互换使用。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-138">Since nullable types are implemented using the type `System.Nullable(Of T)`, the type `T?` is synonymous to the type `System.Nullable(Of T)`, and the two names can be used interchangeably.</span></span> <span data-ttu-id="d2dd3-139">`?` 修饰符不能放置在已可以为 null 的类型上;因此，不能声明类型 `Integer??` 或 `System.Nullable(Of Integer)?`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-139">The `?` modifier cannot be placed on a type that is already nullable; thus, it is not possible to declare the type `Integer??` or `System.Nullable(Of Integer)?`.</span></span>

<span data-ttu-id="d2dd3-140">可以为 null 的值类型 `T?` 具有 `System.Nullable(Of T)` 的成员，以及从基础类型 `T`*提升*为类型 `T?`的任何运算符或转换。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-140">A nullable value type `T?` has the members of `System.Nullable(Of T)` as well as any operators or conversions *lifted* from the underlying type `T` into the type `T?`.</span></span> <span data-ttu-id="d2dd3-141">从基础类型中抬起复制运算符和转换，在大多数情况下，会将可为 null 的值类型替换为不可为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-141">Lifting copies operators and conversions from the underlying type, in most cases substituting nullable value types for non-nullable value types.</span></span> <span data-ttu-id="d2dd3-142">这允许应用于 `T` 的许多相同的转换和操作也适用于 `T?`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-142">This allows many of the same conversions and operations that apply to `T` to apply to `T?` as well.</span></span>


## <a name="interface-implementation"></a><span data-ttu-id="d2dd3-143">接口实现</span><span class="sxs-lookup"><span data-stu-id="d2dd3-143">Interface Implementation</span></span>

<span data-ttu-id="d2dd3-144">结构和类声明可以声明它们通过一个或多个 `Implements` 子句实现一组接口类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-144">Structure and class declarations may declare that they implement a set of interface types through one or more `Implements` clauses.</span></span>

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="d2dd3-145">`Implements` 子句中指定的所有类型都必须是接口，并且该类型必须实现接口的所有成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-145">All the types specified in the `Implements` clause must be interfaces, and the type must implement all members of the interfaces.</span></span> <span data-ttu-id="d2dd3-146">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-146">For example:</span></span>

```vb
Interface ICloneable
    Function Clone() As Object
End Interface 

Interface IComparable
    Function CompareTo(other As Object) As Integer
End Interface 

Structure ListEntry
    Implements ICloneable, IComparable

    ...

    Public Function Clone() As Object Implements ICloneable.Clone
        ...
    End Function 

    Public Function CompareTo(other As Object) As Integer _
        Implements IComparable.CompareTo
        ...
    End Function 
End Structure
```

<span data-ttu-id="d2dd3-147">实现接口的类型还隐式实现了接口的所有基接口。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-147">A type that implements an interface also implicitly implements all of the interface's base interfaces.</span></span> <span data-ttu-id="d2dd3-148">即使类型未在 `Implements` 子句中显式列出所有基接口，也是如此。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-148">This is true even if the type does not explicitly list all base interfaces in the `Implements` clause.</span></span> <span data-ttu-id="d2dd3-149">在此示例中，`TextBox` 结构同时实现 `IControl` 和 `ITextBox`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-149">In this example, the `TextBox` structure implements both `IControl` and `ITextBox`.</span></span>

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Structure TextBox
    Implements ITextBox

    ...

    Public Sub Paint() Implements ITextBox.Paint
        ...
    End Sub

    Public Sub SetText(text As String) Implements ITextBox.SetText
        ...
    End Sub
End Structure
```

<span data-ttu-id="d2dd3-150">声明某个类型在和本身实现接口时，不会在该类型的声明空间中声明任何内容。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-150">Declaring that a type implements an interface in and of itself does not declare anything in the declaration space of the type.</span></span> <span data-ttu-id="d2dd3-151">因此，使用具有相同名称的方法实现两个接口是有效的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-151">Thus, it is valid to implement two interfaces with a method by the same name.</span></span>

<span data-ttu-id="d2dd3-152">类型本身不能实现类型参数，但它可能涉及范围内的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-152">Types cannot implement a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

<span data-ttu-id="d2dd3-153">泛型接口可使用不同的类型参数实现多次。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-153">Generic interfaces can be implemented multiple times using different type arguments.</span></span> <span data-ttu-id="d2dd3-154">但是，如果提供的类型参数（无论类型约束如何）与该接口的另一实现重叠，则泛型类型不能使用类型参数实现泛型接口。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-154">However, a generic type cannot implement a generic interface using a type parameter if the supplied type parameter (regardless of type constraints) could overlap with another implementation of that interface.</span></span> <span data-ttu-id="d2dd3-155">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-155">For example:</span></span>

```vb
Interface I1(Of T)
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)    ' OK, no overlap
End Class

Class C2(Of T)
    Implements I1(Of Integer)
    Implements I1(Of T)         ' Error, T could be Integer
End Class
```


## <a name="primitive-types"></a><span data-ttu-id="d2dd3-156">基元类型</span><span class="sxs-lookup"><span data-stu-id="d2dd3-156">Primitive Types</span></span>

<span data-ttu-id="d2dd3-157">*基元类型*通过关键字标识，关键字是 `System` 命名空间中预定义类型的别名。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-157">The *primitive types* are identified through keywords, which are aliases for predefined types in the `System` namespace.</span></span> <span data-ttu-id="d2dd3-158">基元类型与它的别名类型完全不同：编写保留字 `Byte` 与写入 `System.Byte`完全相同。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-158">A primitive type is completely indistinguishable from the type it aliases: writing the reserved word `Byte` is exactly the same as writing `System.Byte`.</span></span> <span data-ttu-id="d2dd3-159">基元类型也称为 "*内部类型*"。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-159">Primitive types are also known as *intrinsic types*.</span></span>

```antlr
PrimitiveTypeName
    : NumericTypeName
    | 'Boolean'
    | 'Date'
    | 'Char'
    | 'String'
    ;

NumericTypeName
    : IntegralTypeName
    | FloatingPointTypeName
    | 'Decimal'
    ;

IntegralTypeName
    : 'Byte' | 'SByte' | 'UShort' | 'Short' | 'UInteger'
    | 'Integer' | 'ULong' | 'Long'
    ;

FloatingPointTypeName
    : 'Single' | 'Double'
    ;
```


<span data-ttu-id="d2dd3-160">由于基元类型会将常规类型作为别名，因此每个基元类型都具有成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-160">Because a primitive type aliases a regular type, every primitive type has members.</span></span> <span data-ttu-id="d2dd3-161">例如，`Integer` 具有在 `System.Int32`中声明的成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-161">For example, `Integer` has the members declared in `System.Int32`.</span></span> <span data-ttu-id="d2dd3-162">可以将文本视为其对应类型的实例。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-162">Literals can be treated as instances of their corresponding types.</span></span>

* <span data-ttu-id="d2dd3-163">基元类型不同于其他结构类型，因为它们允许执行某些其他操作：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-163">The primitive types differ from other structure types in that they permit certain additional operations:</span></span>

* <span data-ttu-id="d2dd3-164">基元类型允许通过编写文本来创建值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-164">Primitive types permit values to be created by writing literals.</span></span> <span data-ttu-id="d2dd3-165">例如，`123I` 是 `Integer`类型的文本。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-165">For example, `123I` is a literal of type `Integer`.</span></span>

* <span data-ttu-id="d2dd3-166">可以声明基元类型的常量。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-166">It is possible to declare constants of the primitive types.</span></span>

* <span data-ttu-id="d2dd3-167">如果表达式的操作数均为基元类型常量，则编译器可能会在编译时计算表达式。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-167">When the operands of an expression are all primitive type constants, it is possible for the compiler to evaluate the expression at compile time.</span></span> <span data-ttu-id="d2dd3-168">此类表达式称为常量表达式。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-168">Such an expression is known as a constant expression.</span></span>

<span data-ttu-id="d2dd3-169">Visual Basic 定义以下基元类型：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-169">Visual Basic defines the following primitive types:</span></span>

* <span data-ttu-id="d2dd3-170">整数值类型 `Byte` （1字节无符号整数）、`SByte` （1个字节的有符号整数）、`UShort` （2字节无符号整数）、`Short` （2字节有符号整数）、`UInteger` （4字节无符号整数）、`Integer` （8字节无符号整数）和 `ULong` （8字节有符号整数）。`Long`</span><span class="sxs-lookup"><span data-stu-id="d2dd3-170">The integral value types `Byte` (1-byte unsigned integer), `SByte` (1-byte signed integer), `UShort` (2-byte unsigned integer), `Short` (2-byte signed integer), `UInteger` (4-byte unsigned integer), `Integer` (4-byte signed integer), `ULong` (8-byte unsigned integer), and `Long` (8-byte signed integer).</span></span> <span data-ttu-id="d2dd3-171">这些类型分别映射到 `System.Byte`、`System.SByte`、`System.UInt16`、`System.Int16`、`System.UInt32`、`System.Int32`、`System.UInt64` 和 `System.Int64`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-171">These types map to `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` and `System.Int64`, respectively.</span></span> <span data-ttu-id="d2dd3-172">整数类型的默认值等效于文本 `0`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-172">The default value of an integral type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="d2dd3-173">浮点值类型 `Single` （4字节浮点数）和 `Double` （8字节浮点）。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-173">The floating-point value types `Single` (4-byte floating point) and `Double` (8-byte floating point).</span></span> <span data-ttu-id="d2dd3-174">这些类型分别映射到 `System.Single` 和 `System.Double`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-174">These types map to `System.Single` and `System.Double`, respectively.</span></span> <span data-ttu-id="d2dd3-175">浮点类型的默认值等效于文本 `0`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-175">The default value of a floating-point type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="d2dd3-176">`Decimal` 类型（16字节小数值），映射到 `System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-176">The `Decimal` type (16-byte decimal value), which maps to `System.Decimal`.</span></span> <span data-ttu-id="d2dd3-177">默认值 decimal 等效于文本 `0D`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-177">The default value of decimal is equivalent to the literal `0D`.</span></span>

* <span data-ttu-id="d2dd3-178">`Boolean` 值类型，它表示一个真值，通常是关系或逻辑运算的结果。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-178">The `Boolean` value type, which represents a truth value, typically the result of a relational or logical operation.</span></span> <span data-ttu-id="d2dd3-179">文本的类型为 `System.Boolean`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-179">The literal is of type `System.Boolean`.</span></span> <span data-ttu-id="d2dd3-180">`Boolean` 类型的默认值等效于文本 `False`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-180">The default value of the `Boolean` type is equivalent to the literal `False`.</span></span>

* <span data-ttu-id="d2dd3-181">`Date` 值类型，它表示日期和/或时间，并映射到 `System.DateTime`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-181">The `Date` value type, which represents a date and/or a time and maps to `System.DateTime`.</span></span> <span data-ttu-id="d2dd3-182">`Date` 类型的默认值等效于文本 `# 01/01/0001 12:00:00AM #`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-182">The default value of the `Date` type is equivalent to the literal `# 01/01/0001 12:00:00AM #`.</span></span>

* <span data-ttu-id="d2dd3-183">`Char` 值类型，它表示单个 Unicode 字符并映射到 `System.Char`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-183">The `Char` value type, which represents a single Unicode character and maps to `System.Char`.</span></span> <span data-ttu-id="d2dd3-184">`Char` 类型的默认值等效于 `ChrW(0)`的常量表达式。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-184">The default value of the `Char` type is equivalent to the constant expression `ChrW(0)`.</span></span>

* <span data-ttu-id="d2dd3-185">`String` 引用类型，它表示一个 Unicode 字符序列并映射到 `System.String`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-185">The `String` reference type, which represents a sequence of Unicode characters and maps to `System.String`.</span></span> <span data-ttu-id="d2dd3-186">`String` 类型的默认值为 null 值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-186">The default value of the `String` type is a null value.</span></span>



## <a name="enumerations"></a><span data-ttu-id="d2dd3-187">枚举</span><span class="sxs-lookup"><span data-stu-id="d2dd3-187">Enumerations</span></span>

<span data-ttu-id="d2dd3-188">*枚举*是继承自 `System.Enum` 的值类型，符号表示基元整型类型之一的一组值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-188">*Enumerations* are value types that inherit from `System.Enum` and symbolically represent a set of values of one of the primitive integral types.</span></span>

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

<span data-ttu-id="d2dd3-189">对于 `E`的枚举类型，默认值是由表达式 `CType(0, E)`生成的值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-189">For an enumeration type `E`, the default value is the value produced by the expression `CType(0, E)`.</span></span>

<span data-ttu-id="d2dd3-190">枚举的基础类型必须是可表示枚举中定义的所有枚举器值的整型类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-190">The underlying type of an enumeration must be an integral type that can represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="d2dd3-191">如果指定了基础类型，则它必须在 `UInteger`命名空间中 `Byte`、`SByte`、`UShort`、`Short`、`Integer`、`ULong`、`Long`、`System` 或其对应的类型之一。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-191">If an underlying type is specified, it must be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or one of their corresponding types in the `System` namespace.</span></span> <span data-ttu-id="d2dd3-192">如果未显式指定基础类型，则默认值为 `Integer`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-192">If no underlying type is explicitly specified, the default is `Integer`.</span></span>

<span data-ttu-id="d2dd3-193">下面的示例声明一个 `Long`的基础类型的枚举：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-193">The following example declares an enumeration with an underlying type of `Long`:</span></span>

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

<span data-ttu-id="d2dd3-194">开发人员可以选择使用基础类型的 `Long`（如本示例所示），以允许使用 `Long`范围内但不在 `Integer`范围内的值，或保留此选项以供将来使用。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-194">A developer might choose to use an underlying type of `Long`, as in the example, to enable the use of values that are in the range of `Long`, but not in the range of `Integer`, or to preserve this option for the future.</span></span>


### <a name="enumeration-members"></a><span data-ttu-id="d2dd3-195">枚举成员</span><span class="sxs-lookup"><span data-stu-id="d2dd3-195">Enumeration Members</span></span>

<span data-ttu-id="d2dd3-196">枚举的成员是枚举中声明的值和从类 `System.Enum`继承的成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-196">The members of an enumeration are the enumerated values declared in the enumeration and the members inherited from class `System.Enum`.</span></span>

<span data-ttu-id="d2dd3-197">枚举成员的范围是枚举声明体。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-197">The scope of an enumeration member is the enumeration declaration body.</span></span> <span data-ttu-id="d2dd3-198">这意味着，枚举成员必须始终是限定的（除非该类型通过命名空间导入专门导入到命名空间）。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-198">This means that outside of an enumeration declaration, an enumeration member must always be qualified (unless the type is specifically imported into a namespace through a namespace import).</span></span>

<span data-ttu-id="d2dd3-199">省略常量表达式值时，枚举成员声明的声明顺序非常重要。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-199">Declaration order for enumeration member declarations is significant when constant expression values are omitted.</span></span> <span data-ttu-id="d2dd3-200">枚举成员隐式具有 `Public` 访问权限;枚举成员声明中不允许使用访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-200">Enumeration members implicitly have `Public` access only; no access modifiers are allowed on enumeration member declarations.</span></span>

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a><span data-ttu-id="d2dd3-201">枚举值</span><span class="sxs-lookup"><span data-stu-id="d2dd3-201">Enumeration Values</span></span>

<span data-ttu-id="d2dd3-202">枚举成员列表中的枚举值被声明为类型为基础枚举类型的常量，它们可以出现在需要常量的位置。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-202">The enumerated values in an enumeration member list are declared as constants typed as the underlying enumeration type, and they can appear wherever constants are required.</span></span> <span data-ttu-id="d2dd3-203">具有 `=` 的枚举成员定义为关联成员提供常量表达式所指示的值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-203">An enumeration member definition with `=` gives the associated member the value indicated by the constant expression.</span></span> <span data-ttu-id="d2dd3-204">常数表达式的计算结果必须是可隐式转换为基础类型并且必须位于可由基础类型表示的值范围内的整数类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-204">The constant expression must evaluate to an integral type that is implicitly convertible to the underlying type and must be within the range of values that can be represented by the underlying type.</span></span> <span data-ttu-id="d2dd3-205">下面的示例是错误的，因为常量值 `1.5`、`2.3`和 `3.3` 不能隐式转换为具有严格语义的基础整型类型 `Long`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-205">The following example is in error because the constant values `1.5`, `2.3`, and `3.3` are not implicitly convertible to the underlying integral type `Long` with strict semantics.</span></span>

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

<span data-ttu-id="d2dd3-206">多个枚举成员可能共享相同的关联值，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-206">Multiple enumeration members may share the same associated value, as shown below:</span></span>

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

<span data-ttu-id="d2dd3-207">该示例显示一个枚举，该枚举具有两个枚举成员（`Blue` 和 `Max`），它们具有相同的关联值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-207">The example shows an enumeration that has two enumeration members -- `Blue` and `Max` -- that have the same associated value.</span></span>

<span data-ttu-id="d2dd3-208">如果枚举中的第一个枚举器值定义没有初始值设定项，则将 `0`相应的常量的值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-208">If the first enumerator value definition in the enumeration has no initializer, the value of the corresponding constant is `0`.</span></span> <span data-ttu-id="d2dd3-209">如果枚举值定义没有初始值设定项，则会通过 `1`增加上一个枚举值的值来为枚举器提供值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-209">An enumeration value definition without an initializer gives the enumerator the value obtained by increasing the value of the previous enumeration value by `1`.</span></span> <span data-ttu-id="d2dd3-210">这一增加的值必须在可由基础类型表示的值范围内。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-210">This increased value must be within the range of values that can be represented by the underlying type.</span></span>

```vb
Enum Color
    Red
    Green = 10
    Blue
End Enum 

Module Test
    Sub Main()
        Console.WriteLine(StringFromColor(Color.Red))
        Console.WriteLine(StringFromColor(Color.Green))
        Console.WriteLine(StringFromColor(Color.Blue))
    End Sub

    Function StringFromColor(c As Color) As String
        Select Case c
            Case Color.Red
                Return String.Format("Red = " & CInt(c))

            Case Color.Green
                Return String.Format("Green = " & CInt(c))

            Case Color.Blue
                Return String.Format("Blue = " & CInt(c))

            Case Else
                Return "Invalid color"
        End Select
    End Function
End Module
```

<span data-ttu-id="d2dd3-211">上面的示例输出枚举值及其关联值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-211">The example above prints the enumeration values and their associated values.</span></span> <span data-ttu-id="d2dd3-212">输出为：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-212">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="d2dd3-213">这些值的原因如下：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-213">The reasons for the values are as follows:</span></span>

* <span data-ttu-id="d2dd3-214">`Red` 的枚举值自动分配给 `0` 的值（因为它没有初始值设定项，并且是第一个枚举值成员）。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-214">The enumeration value `Red` is automatically assigned the value `0` (since it has no initializer and is the first enumeration value member).</span></span>

* <span data-ttu-id="d2dd3-215">`Green` 的枚举值显式给定 `10`值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-215">The enumeration value `Green` is explicitly given the value `10`.</span></span>

* <span data-ttu-id="d2dd3-216">`Blue` 的枚举值会自动赋予一个大于在其上以多值的枚举值的值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-216">The enumeration value `Blue` is automatically assigned the value one greater than the enumeration value that textually precedes it.</span></span>

<span data-ttu-id="d2dd3-217">常数表达式不能直接或间接地使用其自身关联的枚举值的值（也就是说，不允许常量表达式中的循环）。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-217">The constant expression may not directly or indirectly use the value of its own associated enumeration value (that is, circularity in the constant expression is not allowed).</span></span> <span data-ttu-id="d2dd3-218">下面的示例无效，因为 `A` 和 `B` 的声明是循环的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-218">The following example is invalid because the declarations of `A` and `B` are circular.</span></span>

```vb
Enum Circular
    A = B
    B
End Enum
```

<span data-ttu-id="d2dd3-219">`A` 显式依赖于 `B`，`B` 依赖于 `A` 隐式。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-219">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

## <a name="classes"></a><span data-ttu-id="d2dd3-220">类</span><span class="sxs-lookup"><span data-stu-id="d2dd3-220">Classes</span></span>

<span data-ttu-id="d2dd3-221">*类*是一种数据结构，它可以包含数据成员（常量、变量和事件）、函数成员（方法、属性、索引器、运算符和构造函数）以及嵌套类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-221">A *class* is a data structure that may contain data members (constants, variables, and events), function members (methods, properties, indexers, operators, and constructors), and nested types.</span></span> <span data-ttu-id="d2dd3-222">类是引用类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-222">Classes are reference types.</span></span>

```antlr
ClassDeclaration
    : Attributes? ClassModifier* 'Class' Identifier TypeParameterList? StatementTerminator
      ClassBase?
      TypeImplementsClause*
      ClassMemberDeclaration*
      'End' 'Class' StatementTerminator
    ;

ClassModifier
    : TypeModifier
    | 'MustInherit'
    | 'NotInheritable'
    | 'Partial'
    ;
```

<span data-ttu-id="d2dd3-223">下面的示例演示包含每种成员的类：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-223">The following example shows a class that contains each kind of member:</span></span>

```vb
Class AClass
    Public Sub New()
        Console.WriteLine("Constructor")
    End Sub

    Public Sub New(value As Integer)
        MyVariable = value
        Console.WriteLine("Constructor")
    End Sub

    Public Const MyConst As Integer = 12
    Public MyVariable As Integer = 34

    Public Sub MyMethod()
        Console.WriteLine("MyClass.MyMethod")
    End Sub

    Public Property MyProperty() As Integer
        Get
            Return MyVariable
        End Get

        Set (value As Integer)
            MyVariable = value
        End Set
    End Property

    Default Public Property Item(index As Integer) As Integer
        Get
            Return 0
        End Get

        Set (value As Integer)
            Console.WriteLine("Item(" & index & ") = " & value)
        End Set
    End Property

    Public Event MyEvent()

    Friend Class MyNestedClass
    End Class 
End Class
```

<span data-ttu-id="d2dd3-224">下面的示例演示了这些成员的用法：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-224">The following example shows uses of these members:</span></span>

```vb
Module Test

    ' Event usage.
    Dim WithEvents aInstance As AClass

    Sub Main()
        ' Constructor usage.
        Dim a As AClass = New AClass()
        Dim b As AClass = New AClass(123)

        ' Constant usage.
        Console.WriteLine("MyConst = " & AClass.MyConst)

        ' Variable usage.
        a.MyVariable += 1
        Console.WriteLine("a.MyVariable = " & a.MyVariable)

        ' Method usage.
        a.MyMethod()

        ' Property usage.
        a.MyProperty += 1
        Console.WriteLine("a.MyProperty = " & a.MyProperty)
        a(1) = 1

        ' Event usage.
        aInstance = a
    End Sub 

    Sub MyHandler() Handles aInstance.MyEvent
        Console.WriteLine("Test.MyHandler")
    End Sub 
End Module
```

<span data-ttu-id="d2dd3-225">`MustInherit` 和 `NotInheritable`有两个特定于类的修饰符。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-225">There are two class-specific modifiers, `MustInherit` and `NotInheritable`.</span></span> <span data-ttu-id="d2dd3-226">同时指定它们是无效的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-226">It is invalid to specify them both.</span></span>


### <a name="class-base-specification"></a><span data-ttu-id="d2dd3-227">类基规范</span><span class="sxs-lookup"><span data-stu-id="d2dd3-227">Class Base Specification</span></span>

<span data-ttu-id="d2dd3-228">类声明可以包含定义类的直接基类型的基类型规范。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-228">A class declaration may include a base type specification that defines the direct base type of the class.</span></span>

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

<span data-ttu-id="d2dd3-229">如果类声明没有显式基类型，则直接基类型将被隐式 `Object`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-229">If a class declaration has no explicit base type, the direct base type is implicitly `Object`.</span></span> <span data-ttu-id="d2dd3-230">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-230">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

<span data-ttu-id="d2dd3-231">基类型本身不能是类型参数，但它可能涉及范围内的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-231">Base types cannot be a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

```vb
Class C1(Of V) 
End Class

Class C2(Of V)
    Inherits V    ' Error, type parameter used as base class 
End Class

Class C3(Of V)
    Inherits C1(Of V)    ' OK: not directly inheriting from V.
End Class
```

<span data-ttu-id="d2dd3-232">类只能从 `Object` 和类派生。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-232">Classes may only derive from `Object` and classes.</span></span> <span data-ttu-id="d2dd3-233">从 `System.ValueType`、`System.Enum`、`System.Array`、`System.MulticastDelegate` 或 `System.Delegate`派生类无效。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-233">It is invalid for a class to derive from `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` or `System.Delegate`.</span></span> <span data-ttu-id="d2dd3-234">泛型类不能从 `System.Attribute` 派生或从派生的类派生。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-234">A generic class cannot derive from `System.Attribute` or from a class that derives from it.</span></span>

<span data-ttu-id="d2dd3-235">每个类都有一个直接基类，并且不允许派生循环。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-235">Every class has exactly one direct base class, and circularity in derivation is prohibited.</span></span> <span data-ttu-id="d2dd3-236">不能从 `NotInheritable` 类派生，并且基类的可访问域必须与类本身的可访问性域的超集或相同。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-236">It is not possible to derive from a `NotInheritable` class, and the accessibility domain of the base class must be the same as or a superset of the accessibility domain of the class itself.</span></span>


### <a name="class-members"></a><span data-ttu-id="d2dd3-237">类成员</span><span class="sxs-lookup"><span data-stu-id="d2dd3-237">Class Members</span></span>

<span data-ttu-id="d2dd3-238">类的成员由其类成员声明引入的成员和从其直接基类继承的成员组成。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-238">The members of a class consist of the members introduced by its class member declarations and the members inherited from its direct base class.</span></span>

```antlr
ClassMemberDeclaration
    : NonModuleDeclaration
    | EventMemberDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

<span data-ttu-id="d2dd3-239">类成员声明可以有 `Public`、`Protected`、`Friend`、`Protected Friend`或 `Private` 访问。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-239">A class member declaration may have `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` access.</span></span> <span data-ttu-id="d2dd3-240">如果类成员声明不包括访问修饰符，则声明默认为 `Public` 访问，除非它是变量声明;在这种情况下，它默认 `Private` 访问。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-240">When a class member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable declaration; in that case it defaults to `Private` access.</span></span>

<span data-ttu-id="d2dd3-241">类成员的作用域是在其中进行成员声明的类体，加上该类的约束列表（如果它是泛型的并且具有约束）。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-241">The scope of a class member is the class body in which the member declaration occurs, plus the constraint list of that class (if it is generic and has constraints).</span></span> <span data-ttu-id="d2dd3-242">如果成员具有 `Friend` 访问权限，则其范围将扩展到同一程序中任何派生类的类体或 `Friend` 访问的任何程序集中，并且如果成员具有 `Public`、`Protected`或 `Protected Friend` 访问权限，则其范围将扩展到任何程序中任何派生类的类体。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-242">If the member has `Friend` access, its scope extends to the class body of any derived class in the same program or any assembly that has been given `Friend` access, and if the member has `Public`, `Protected`, or `Protected Friend` access, its scope extends to the class body of any derived class in any program.</span></span>


## <a name="structures"></a><span data-ttu-id="d2dd3-243">结构</span><span class="sxs-lookup"><span data-stu-id="d2dd3-243">Structures</span></span>

<span data-ttu-id="d2dd3-244">*结构*是继承自 `System.ValueType`的值类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-244">*Structures* are value types that inherit from `System.ValueType`.</span></span> <span data-ttu-id="d2dd3-245">结构与类相似，因为它们表示可以包含数据成员和函数成员的数据结构。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-245">Structures are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="d2dd3-246">但与类不同的是，结构不需要进行堆分配。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-246">Unlike classes, however, structures do not require heap allocation.</span></span>

```antlr
StructureDeclaration
    : Attributes? StructureModifier* 'Structure' Identifier
      TypeParameterList? StatementTerminator
      TypeImplementsClause*
      StructMemberDeclaration*
      'End' 'Structure' StatementTerminator
    ;

StructureModifier
    : TypeModifier
    | 'Partial'
    ;
```

<span data-ttu-id="d2dd3-247">对于类，两个变量可以引用同一对象，因此，对一个变量执行的运算可能会影响另一个变量所引用的对象。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-247">In the case of classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="d2dd3-248">使用结构，每个变量都有自己的非`Shared` 数据的副本，因此，对一个变量执行的操作不会影响另一个，如下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-248">With structures, the variables each have their own copy of the non-`Shared` data, so it is not possible for operations on one to affect the other, as the following example illustrates:</span></span>

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="d2dd3-249">给定上述声明后，以下代码将 `10`输出值：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-249">Given the above declaration the following code outputs the value `10`:</span></span>

```vb
Module Test
    Sub Main()
        Dim a As New Point(10, 10)
        Dim b As Point = a

        a.x = 100
        Console.WriteLine(b.x)
    End Sub
End Module
```

<span data-ttu-id="d2dd3-250">将 `a` 分配给 `b` 会创建值的副本，`b` 因此不受 `a.x`赋值的影响。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-250">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="d2dd3-251">`Point` 改为作为类进行声明，输出将 `100`，因为 `a` 和 `b` 将引用相同的对象。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-251">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>


### <a name="structure-members"></a><span data-ttu-id="d2dd3-252">结构成员</span><span class="sxs-lookup"><span data-stu-id="d2dd3-252">Structure Members</span></span>

<span data-ttu-id="d2dd3-253">结构的成员是由其结构成员声明引入的成员以及从 `System.ValueType`继承的成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-253">The members of a structure are the members introduced by its structure member declarations and the members inherited from `System.ValueType`.</span></span>

```antlr
StructMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

<span data-ttu-id="d2dd3-254">每个结构都隐式具有一个生成结构的默认值的 `Public` 无参数实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-254">Every structure implicitly has a `Public` parameterless instance constructor that produces the default value of the structure.</span></span> <span data-ttu-id="d2dd3-255">因此，结构类型声明不可能声明无参数的实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-255">As a result, it is not possible for a structure type declaration to declare a parameterless instance constructor.</span></span> <span data-ttu-id="d2dd3-256">但结构类型允许声明*参数化*实例构造函数，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-256">A structure type is, however, permitted to declare *parameterized* instance constructors, as in the following example:</span></span>

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="d2dd3-257">给定上述声明，以下语句将创建一个 `Point`，并将 `x` 和 `y` 初始化为零。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-257">Given the above declaration, the following statements both create a `Point` with `x` and `y` initialized to zero.</span></span>

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

<span data-ttu-id="d2dd3-258">由于结构直接包含其字段值（而不是对这些值的引用），因此结构不能包含直接或间接引用自身的字段。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-258">Because structures directly contain their field values (rather than references to those values), structures cannot contain fields that directly or indirectly reference themselves.</span></span> <span data-ttu-id="d2dd3-259">例如，以下代码无效：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-259">For example, the following code is not valid:</span></span>

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

<span data-ttu-id="d2dd3-260">通常，结构成员声明只能具有 `Public`、`Friend`或 `Private` 访问权限，但当重写继承自 `Object`的成员时，还可以使用 `Protected` 和 `Protected Friend` 访问。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-260">Normally, a structure member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, `Protected` and `Protected Friend` access may also be used.</span></span> <span data-ttu-id="d2dd3-261">当结构成员声明不包括访问修饰符时，该声明默认为 `Public` 访问。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-261">When a structure member declaration does not include an access modifier, the declaration defaults to `Public` access.</span></span> <span data-ttu-id="d2dd3-262">结构声明的成员的作用域是在其中进行声明的结构主体，以及该结构的约束（如果它是泛型的并且具有约束）。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-262">The scope of a member declared by a structure is the structure body in which the declaration occurs, plus the constraints of that structure (if it was generic and had constraints).</span></span>


## <a name="standard-modules"></a><span data-ttu-id="d2dd3-263">标准模块</span><span class="sxs-lookup"><span data-stu-id="d2dd3-263">Standard Modules</span></span>

<span data-ttu-id="d2dd3-264">*标准模块*是一种类型，其成员隐式地 `Shared`，范围限定为标准模块的包含命名空间的声明空间，而不只是标准模块声明本身。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-264">A *standard module* is a type whose members are implicitly `Shared` and scoped to the declaration space of the standard module's containing namespace, rather than just to the standard module declaration itself.</span></span> <span data-ttu-id="d2dd3-265">标准模块永远不能实例化。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-265">Standard modules may never be instantiated.</span></span> <span data-ttu-id="d2dd3-266">声明标准模块类型的变量是错误的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-266">It is an error to declare a variable of a standard module type.</span></span>

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

<span data-ttu-id="d2dd3-267">标准模块的成员有两个完全限定的名称，一个没有标准模块名称，另一个具有标准模块名称。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-267">A member of a standard module has two fully qualified names, one without the standard module name and one with the standard module name.</span></span> <span data-ttu-id="d2dd3-268">命名空间中的多个标准模块可以定义具有特定名称的成员;对任一模块外的名称的非限定引用不明确。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-268">More than one standard module in a namespace may define a member with a particular name; unqualified references to the name outside of either module are ambiguous.</span></span> <span data-ttu-id="d2dd3-269">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-269">For example:</span></span>

```vb
Namespace N1
    Module M1
        Sub S1()
        End Sub

        Sub S2()
        End Sub
    End Module

    Module M2
        Sub S2()
        End Sub
    End Module

    Module M3
        Sub Main()
            S1()       ' Valid: Calls N1.M1.S1.
            N1.S1()    ' Valid: Calls N1.M1.S1.
            S2()       ' Not valid: ambiguous.
            N1.S2()    ' Not valid: ambiguous.
            N1.M2.S2() ' Valid: Calls N1.M2.S2.
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="d2dd3-270">模块只能在命名空间中声明，而不能嵌套在其他类型中。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-270">A module may only be declared in a namespace and may not be nested in another type.</span></span> <span data-ttu-id="d2dd3-271">标准模块不能实现接口，它们从 `Object`隐式派生，并且只有 `Shared` 构造函数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-271">Standard modules may not implement interfaces, they implicitly derive from `Object`, and they have only `Shared` constructors.</span></span>


### <a name="standard-module-members"></a><span data-ttu-id="d2dd3-272">标准模块成员</span><span class="sxs-lookup"><span data-stu-id="d2dd3-272">Standard Module Members</span></span>

<span data-ttu-id="d2dd3-273">标准模块的成员是由其成员声明引入的成员以及从 `Object`继承的成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-273">The members of a standard module are the members introduced by its member declarations and the members inherited from `Object`.</span></span> <span data-ttu-id="d2dd3-274">标准模块可能具有除实例构造函数之外的任何类型的成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-274">Standard modules may have any type of member except instance constructors.</span></span> <span data-ttu-id="d2dd3-275">所有标准模块类型成员均隐式 `Shared`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-275">All standard module type members are implicitly `Shared`.</span></span>

```antlr
ModuleMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    ;
```

<span data-ttu-id="d2dd3-276">通常，标准模块成员声明只能具有 `Public`、`Friend`或 `Private` 访问权限，但当重写继承自 `Object`的成员时，可以指定 `Protected` 和 `Protected Friend` 访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-276">Normally, a standard module member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, the `Protected` and `Protected Friend` access modifiers may be specified.</span></span> <span data-ttu-id="d2dd3-277">如果标准模块成员声明不包括访问修饰符，则声明默认为 `Public` 访问，除非它是一个变量，默认为 `Private` 访问。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-277">When a standard module member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable, which defaults to `Private` access.</span></span>

<span data-ttu-id="d2dd3-278">如前文所述，标准模块成员的作用域是包含标准模块声明的声明。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-278">As previously noted, the scope of a standard module member is the declaration containing the standard module declaration.</span></span> <span data-ttu-id="d2dd3-279">继承自 `Object` 的成员不包含在此特殊范围内;这些成员没有作用域，必须始终使用模块的名称进行限定。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-279">Members inherited from `Object` are not included in this special scoping; those members have no scope and must always be qualified with the name of the module.</span></span> <span data-ttu-id="d2dd3-280">如果成员具有 `Friend` 访问权限，则其作用域只扩展到在 `Friend` 访问权限的同一程序或程序集中声明的命名空间成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-280">If the member has `Friend` access, its scope extends only to namespace members declared in the same program or assemblies that have been given `Friend` access.</span></span>


## <a name="interfaces"></a><span data-ttu-id="d2dd3-281">接口</span><span class="sxs-lookup"><span data-stu-id="d2dd3-281">Interfaces</span></span>

<span data-ttu-id="d2dd3-282">*接口*是其他类型实现以保证它们支持某些方法的引用类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-282">*Interfaces* are reference types that other types implement to guarantee that they support certain methods.</span></span> <span data-ttu-id="d2dd3-283">接口绝不会直接创建且没有实际表示形式，其他类型必须转换为接口类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-283">An interface is never directly created and has no actual representation -- other types must be converted to an interface type.</span></span> <span data-ttu-id="d2dd3-284">接口定义协定。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-284">An interface defines a contract.</span></span> <span data-ttu-id="d2dd3-285">实现接口的类或结构必须遵循它的协定。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-285">A class or structure that implements an interface must adhere to its contract.</span></span>

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


<span data-ttu-id="d2dd3-286">下面的示例演示了一个接口，该接口包含 `Item`的默认属性、事件 `E`、`F`方法和属性 `P`：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-286">The following example shows an interface that contains a default property `Item`, an event `E`, a method `F`, and a property `P`:</span></span>

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

<span data-ttu-id="d2dd3-287">接口可能使用多重继承。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-287">Interfaces may employ multiple inheritance.</span></span> <span data-ttu-id="d2dd3-288">在下面的示例中，接口 `IComboBox` 从 `ITextBox` 和 `IListBox`继承：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-288">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`:</span></span>

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

<span data-ttu-id="d2dd3-289">类和结构可以实现多个接口。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-289">Classes and structures can implement multiple interfaces.</span></span> <span data-ttu-id="d2dd3-290">在下面的示例中，类 `EditBox` 派生自类 `Control` 并实现 `IControl` 和 `IDataBound`：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-290">In the following example, the class `EditBox` derives from the class `Control` and implements both `IControl` and `IDataBound`:</span></span>

```vb
Interface IDataBound
    Sub Bind(b As Binder)
End Interface 

Public Class EditBox
    Inherits Control
    Implements IControl, IDataBound

    Public Sub Paint() Implements IControl.Paint
        ...
    End Sub

    Public Sub Bind(b As Binder) Implements IDataBound.Bind
        ...
    End Sub
End Class
```


### <a name="interface-inheritance"></a><span data-ttu-id="d2dd3-291">接口继承</span><span class="sxs-lookup"><span data-stu-id="d2dd3-291">Interface Inheritance</span></span>

<span data-ttu-id="d2dd3-292">接口的基接口是显式基接口及其基接口。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-292">The base interfaces of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="d2dd3-293">换言之，基接口集是显式基接口的完全可传递的闭包、其显式基接口等。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-293">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="d2dd3-294">如果接口声明没有显式的接口基，则类型的基接口不会从 `Object` 继承（尽管它们确实具有到 `Object`的扩大转换）。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-294">If an interface declaration has no explicit interface base, then there is no base interface for the type -- interfaces do not inherit from `Object` (although they do have a widening conversion to `Object`).</span></span>

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="d2dd3-295">在下面的示例中，`IComboBox` 的基本接口 `IControl`、`ITextBox`和 `IListBox`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-295">In the following example, the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

<span data-ttu-id="d2dd3-296">接口继承其基接口的所有成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-296">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="d2dd3-297">换言之，上面的 `IComboBox` 接口继承成员 `SetText` 和 `SetItems` 以及 `Paint`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-297">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="d2dd3-298">实现接口的类或结构还隐式实现了接口的所有基接口。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-298">A class or structure that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

<span data-ttu-id="d2dd3-299">如果接口在基接口的传递闭包中多次出现，它只会将其成员分配给派生接口一次。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-299">If an interface appears more than once in the transitive closure of the base interfaces, it only contributes its members to the derived interface once.</span></span> <span data-ttu-id="d2dd3-300">实现派生接口的类型只需要实现多次定义的基接口的方法。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-300">A type implementing the derived interface only has to implement the methods of the multiply defined base interface once.</span></span> <span data-ttu-id="d2dd3-301">在下面的示例中，`Paint` 只需要实现一次，即使类实现 `IComboBox` 和 `IControl`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-301">In the following example, `Paint` only needs to be implemented once, even though the class implements `IComboBox` and `IControl`.</span></span>

```vb
Class ComboBox
    Implements IControl, IComboBox

    Sub SetText(text As String) Implements IComboBox.SetText
    End Sub

    Sub SetItems(items() As String) Implements IComboBox.SetItems
    End Sub

    Sub Print() Implements IComboBox.Paint
    End Sub
End Class
```

<span data-ttu-id="d2dd3-302">`Inherits` 子句对其他 `Inherits` 子句不起作用。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-302">An `Inherits` clause has no effect on other `Inherits` clauses.</span></span> <span data-ttu-id="d2dd3-303">在下面的示例中，`IDerived` 必须用 `IBase`限定 `INested` 的名称。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-303">In the following example, `IDerived` must qualify the name of `INested` with `IBase`.</span></span>

```vb
Interface IBase
    Interface INested
        Sub Nested()
    End Interface

    Sub Base()
End Interface

Interface IDerived
    Inherits IBase, INested   ' Error: Must specify IBase.INested.
End Interface
```

<span data-ttu-id="d2dd3-304">基接口的可访问域必须与接口本身的可访问域的可访问域相同或超集。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-304">The accessibility domain of a base interface must be the same as or a superset of the accessibility domain of the interface itself.</span></span>


### <a name="interface-members"></a><span data-ttu-id="d2dd3-305">接口成员</span><span class="sxs-lookup"><span data-stu-id="d2dd3-305">Interface Members</span></span>

<span data-ttu-id="d2dd3-306">接口的成员由其成员声明引入的成员和从其基接口继承的成员组成。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-306">The members of an interface consist of the members introduced by its member declarations and the members inherited from its base interfaces.</span></span>

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

<span data-ttu-id="d2dd3-307">尽管接口不会从 `Object`继承成员，但实现接口的每个类或结构都从 `Object`继承，`Object`的成员（包括扩展方法）被视为接口的成员，并且可以直接在接口上调用，而无需强制转换为 `Object`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-307">Although interfaces do not inherit members from `Object`, because every class or structure that implements an interface does inherit from `Object`, the members of `Object`, including extension methods, are considered members of an interface and can be called on an interface directly without requiring a cast to `Object`.</span></span> <span data-ttu-id="d2dd3-308">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-308">For example:</span></span>

```vb
Interface I1
End Interface

Class C1
    Implements I1
End Class

Module Test
    Sub Main()
        Dim i As I1 = New C1()
        Dim h As Integer = i.GetHashCode()
    End Sub
End Module
```

<span data-ttu-id="d2dd3-309">与 `Object` 的成员同名的接口成员将 `Object` 成员隐式隐藏。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-309">Members of an interface with the same name as members of `Object` implicitly shadow `Object` members.</span></span> <span data-ttu-id="d2dd3-310">只有嵌套类型、方法、属性和事件才能作为接口成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-310">Only nested types, methods, properties, and events may be members of an interface.</span></span> <span data-ttu-id="d2dd3-311">方法和属性可能没有主体。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-311">Methods and properties may not have a body.</span></span> <span data-ttu-id="d2dd3-312">接口成员是隐式 `Public` 的，并且不能指定访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-312">Interface members are implicitly `Public` and may not specify an access modifier.</span></span> <span data-ttu-id="d2dd3-313">在接口中声明的成员的作用域是在其中进行声明的接口主体，以及该接口的约束列表（如果它是泛型并且具有约束）。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-313">The scope of a member declared in an interface is the interface body in which the declaration occurs, plus the constraint list of that interface (if it is generic and has constraints).</span></span>


## <a name="arrays"></a><span data-ttu-id="d2dd3-314">阵列</span><span class="sxs-lookup"><span data-stu-id="d2dd3-314">Arrays</span></span>

<span data-ttu-id="d2dd3-315">*数组*是一种引用类型，它包含通过与数组中的变量顺序相对应的*索引*来访问的变量。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-315">An *array* is a reference type that contains variables accessed through *indices* corresponding in a one-to-one fashion with the order of the variables in the array.</span></span> <span data-ttu-id="d2dd3-316">数组中包含的变量（也称为数组的*元素*）必须都属于同一类型，并且此类型称为数组的*元素类型*。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-316">The variables contained in an array, also called the *elements* of the array, must all be of the same type, and this type is called the *element type* of the array.</span></span>

```antlr
ArrayTypeName
    : NonArrayTypeName ArrayTypeModifiers
    ;

ArrayTypeModifiers
    : ArrayTypeModifier+
    ;

ArrayTypeModifier
    : OpenParenthesis RankList? CloseParenthesis
    ;

RankList
    : Comma*
    ;

ArrayNameModifier
    : ArrayTypeModifiers
    | ArraySizeInitializationModifier
    ;
```

<span data-ttu-id="d2dd3-317">数组元素在创建数组实例后就会存在，并且在销毁数组实例时停止存在。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-317">The elements of an array come into existence when an array instance is created, and cease to exist when the array instance is destroyed.</span></span> <span data-ttu-id="d2dd3-318">数组的每个元素都初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-318">Each element of an array is initialized to the default value of its type.</span></span> <span data-ttu-id="d2dd3-319">类型 `System.Array` 是所有数组类型的基类型，不能进行实例化。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-319">The type `System.Array` is the base type of all array types and may not be instantiated.</span></span> <span data-ttu-id="d2dd3-320">每个数组类型都继承由 `System.Array` 类型声明的成员，并且可以转换为它（和 `Object`）。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-320">Every array type inherits the members declared by the `System.Array` type and is convertible to it (and `Object`).</span></span> <span data-ttu-id="d2dd3-321">带有元素的一维数组类型 `T` 还实现 `System.Collections.Generic.IList(Of T)` 和 `IReadOnlyList(Of T)`的接口;如果 `T` 是引用类型，则数组类型还实现从 `T`进行扩大引用转换的任何 `U` 的 `IList(Of U)` 和 `IReadOnlyList(Of U)`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-321">A one-dimensional array type with element `T` also implements the interfaces `System.Collections.Generic.IList(Of T)` and `IReadOnlyList(Of T)`; if `T` is a reference type, then the array type also implements `IList(Of U)` and `IReadOnlyList(Of U)` for any `U` that has a widening  reference conversion from `T`.</span></span>

<span data-ttu-id="d2dd3-322">数组具有确定与每个数组元素关联的索引数的*级别*。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-322">An array has a *rank* that determines the number of indices associated with each array element.</span></span> <span data-ttu-id="d2dd3-323">数组的秩决定数组的*维数*。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-323">The rank of an array determines the number of *dimensions* of the array.</span></span> <span data-ttu-id="d2dd3-324">例如，秩为 one 的数组称为一维数组，而秩大于1的数组称为多维数组。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-324">For example, an array with a rank of one is called a single-dimensional array, and an array with a rank greater than one is called a multidimensional array.</span></span>

<span data-ttu-id="d2dd3-325">下面的示例创建一个整数值的一维数组，初始化数组元素，然后将每个元素输出到其中：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-325">The following example creates a single-dimensional array of integer values, initializes the array elements, and then prints each of them out:</span></span>

```vb
Module Test
    Sub Main()
        Dim arr(5) As Integer
        Dim i As Integer

        For i = 0 To arr.Length - 1
            arr(i) = i * i
        Next i

        For i = 0 To arr.Length - 1
            Console.WriteLine("arr(" & i & ") = " & arr(i))
        Next i
    End Sub
End Module
```

<span data-ttu-id="d2dd3-326">程序输出以下内容：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-326">The program outputs the following:</span></span>

```console
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

<span data-ttu-id="d2dd3-327">数组的每个维度都具有关联的长度。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-327">Each dimension of an array has an associated length.</span></span> <span data-ttu-id="d2dd3-328">维度长度不是数组类型的一部分，而是在运行时创建数组类型的实例时建立的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-328">Dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run time.</span></span> <span data-ttu-id="d2dd3-329">维度的长度决定了该维度的有效索引范围：对于长度 `N`的维度，索引范围可以介于0到 `N-1`之间。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-329">The length of a dimension determines the valid range of indices for that dimension: for a dimension of length `N`, indices can range from zero to `N-1`.</span></span> <span data-ttu-id="d2dd3-330">如果维度的长度为零，则该维度没有有效的索引。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-330">If a dimension is of length zero, there are no valid indices for that dimension.</span></span> <span data-ttu-id="d2dd3-331">数组中元素的总数为数组中每个维度的长度的乘积。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-331">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="d2dd3-332">如果数组的任意维度的长度为零，则该数组称为空。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-332">If any of the dimensions of an array has a length of zero, the array is said to be empty.</span></span> <span data-ttu-id="d2dd3-333">数组的元素类型可以是任何类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-333">The element type of an array can be any type.</span></span>

<span data-ttu-id="d2dd3-334">通过向现有类型名称添加修饰符来指定数组类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-334">Array types are specified by adding a modifier to an existing type name.</span></span> <span data-ttu-id="d2dd3-335">修饰符由左括号、一组零个或多个逗号和一个右括号组成。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-335">The modifier consists of a left parenthesis, a set of zero or more commas, and a right parenthesis.</span></span> <span data-ttu-id="d2dd3-336">所修改的类型是数组的元素类型，维数是逗号的数目加1。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-336">The type modified is the element type of the array, and the number of dimensions is the number of commas plus one.</span></span> <span data-ttu-id="d2dd3-337">如果指定了多个修饰符，则数组的元素类型为数组。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-337">If more than one modifier is specified, then the element type of the array is an array.</span></span> <span data-ttu-id="d2dd3-338">修饰符从左到右读取，最左边的修饰符是最外面的数组。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-338">The modifiers are read left to right, with the leftmost modifier being the outermost array.</span></span> <span data-ttu-id="d2dd3-339">示例中</span><span class="sxs-lookup"><span data-stu-id="d2dd3-339">In the example</span></span>

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

<span data-ttu-id="d2dd3-340">`arr` 的元素类型是一维数组的一维数组，该数组是一 `Integer`维数组的一维数组。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-340">the element type of `arr` is a two-dimensional array of three-dimensional arrays of one-dimensional arrays of `Integer`.</span></span>

<span data-ttu-id="d2dd3-341">还可以通过将数组类型修饰符或数组大小的初始化修饰符放在变量名上，将变量声明为数组类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-341">A variable may also be declared to be of an array type by putting an array type modifier or an array-size initialization modifier on the variable name.</span></span> <span data-ttu-id="d2dd3-342">在这种情况下，数组元素类型是声明中提供的类型，数组维度由变量名称修饰符决定。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-342">In that case, the array element type is the type given in the declaration, and the array dimensions are determined by the variable name modifier.</span></span> <span data-ttu-id="d2dd3-343">为清楚起见，对同一声明中的变量名称和类型名称具有数组类型修饰符是无效的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-343">For clarity, it is not valid to have an array type modifier on both a variable name and a type name in the same declaration.</span></span>

<span data-ttu-id="d2dd3-344">下面的示例演示了将数组类型与 `Integer` 作为元素类型的各种局部变量声明：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-344">The following example shows a variety of local variable declarations that use array types with `Integer` as the element type:</span></span>

```vb
Module Test
    Sub Main()
        Dim a1() As Integer    ' Declares 1-dimensional array of integers.
        Dim a2(,) As Integer   ' Declares 2-dimensional array of integers.
        Dim a3(,,) As Integer  ' Declares 3-dimensional array of integers.

        Dim a4 As Integer()    ' Declares 1-dimensional array of integers.
        Dim a5 As Integer(,)   ' Declares 2-dimensional array of integers.
        Dim a6 As Integer(,,)  ' Declares 3-dimensional array of integers.

        ' Declare 1-dimensional array of 2-dimensional arrays of integers 
        Dim a7()(,) As Integer
        ' Declare 2-dimensional array of 1-dimensional arrays of integers.
        Dim a8(,)() As Integer

        Dim a9() As Integer() ' Not allowed.
    End Sub
End Module
```

<span data-ttu-id="d2dd3-345">数组类型名称修饰符扩展到其后的所有括号。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-345">An array type name modifier extends to all sets of parentheses that follow it.</span></span> <span data-ttu-id="d2dd3-346">这意味着，在类型名称后允许使用括在括号中的一组参数的情况下，不可能为数组类型名称指定参数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-346">This means that in the situations where a set of arguments enclosed in parenthesis is allowed after a type name, it is not possible to specify the arguments for an array type name.</span></span> <span data-ttu-id="d2dd3-347">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-347">For example:</span></span>

```vb
Module Test
    Sub Main()
        ' This calls the Integer constructor.
        Dim x As New Integer(3)

        ' This declares a variable of Integer().
        Dim y As Integer()

        ' This gives an error.
        ' Array sizes can not be specified in a type name.
        Dim z As Integer()(3)
    End Sub
End Module
```

<span data-ttu-id="d2dd3-348">在最后一种情况下，`(3)` 被解释为类型名称的一部分，而不是作为一组构造函数参数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-348">In the last case, `(3)` is interpreted as part of the type name rather than as a set of constructor arguments.</span></span>


## <a name="delegates"></a><span data-ttu-id="d2dd3-349">委派</span><span class="sxs-lookup"><span data-stu-id="d2dd3-349">Delegates</span></span>

<span data-ttu-id="d2dd3-350">*委托*是一种引用类型，引用类型的 `Shared` 方法或对象的实例方法。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-350">A *delegate* is a reference type that refers to a `Shared` method of a type or to an instance method of an object.</span></span>

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 <span data-ttu-id="d2dd3-351">与其他语言中的委托最接近的等效项是函数指针，但函数指针只能引用 `Shared` 函数，而委托可以引用 `Shared` 和实例方法。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-351">The closest equivalent of a delegate in other languages is a function pointer, but whereas a function pointer can only reference `Shared` functions, a delegate can reference both `Shared` and instance methods.</span></span> <span data-ttu-id="d2dd3-352">在后一种情况下，委托不仅存储对方法入口点的引用，还存储对用来调用该方法的对象实例的引用。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-352">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance with which to invoke the method.</span></span>

<span data-ttu-id="d2dd3-353">委托声明不能有 `Handles` 子句、`Implements` 子句、方法体或 `End` 构造。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-353">The delegate declaration may not have  a `Handles` clause, an `Implements` clause, a method body, or an `End` construct.</span></span> <span data-ttu-id="d2dd3-354">委托声明的参数列表不能包含 `Optional` 或 `ParamArray` 参数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-354">The parameter list of the delegate declaration may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="d2dd3-355">返回类型和参数类型的可访问域必须与委托本身的可访问域的可访问域相同或超集。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-355">The accessibility domain of the return type and parameter types must be the same as or a superset of the accessibility domain of the delegate itself.</span></span>

<span data-ttu-id="d2dd3-356">委托的成员是继承自类 `System.Delegate`的成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-356">The members of a delegate are the members inherited from class `System.Delegate`.</span></span> <span data-ttu-id="d2dd3-357">委托还定义以下方法：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-357">A delegate also defines the following methods:</span></span>

* <span data-ttu-id="d2dd3-358">一个构造函数，该构造函数采用两个参数，一个为类型 `Object`，一个类型为 `System.IntPtr`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-358">A constructor that takes two parameters, one of type `Object` and one of type `System.IntPtr`.</span></span>

* <span data-ttu-id="d2dd3-359">与委托具有相同签名的 `Invoke` 方法。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-359">An `Invoke` method that has the same signature as the delegate.</span></span>

* <span data-ttu-id="d2dd3-360">一个 `BeginInvoke` 方法，其签名为委托签名，具有三个不同之处。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-360">A `BeginInvoke` method whose signature is the delegate signature, with three differences.</span></span> <span data-ttu-id="d2dd3-361">首先，返回类型更改为 `System.IAsyncResult`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-361">First, the return type is changed to `System.IAsyncResult`.</span></span> <span data-ttu-id="d2dd3-362">其次，将两个附加参数添加到参数列表的末尾：类型的第一个 `System.AsyncCallback` 和类型 `Object`的第二个参数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-362">Second, two additional parameters are added to the end of the parameter list: the first of type `System.AsyncCallback` and the second of type `Object`.</span></span> <span data-ttu-id="d2dd3-363">最后，所有 `ByRef` 参数都将更改为 `ByVal`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-363">And finally, all `ByRef` parameters are changed to be `ByVal`.</span></span>

* <span data-ttu-id="d2dd3-364">返回类型与委托相同的 `EndInvoke` 方法。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-364">An `EndInvoke` method whose return type is the same as the delegate.</span></span> <span data-ttu-id="d2dd3-365">方法的参数只是与参数 `ByRef` 参数完全相同的委托参数，其顺序与委托签名中出现的顺序相同。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-365">The parameters of the method are only the delegate parameters exactly that are `ByRef` parameters, in the same order they occur in the delegate signature.</span></span>  <span data-ttu-id="d2dd3-366">除了这些参数外，还存在参数列表末尾 `System.IAsyncResult` 类型的附加参数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-366">In addition to those parameters, there is an additional parameter of type `System.IAsyncResult` at the end of the parameter list.</span></span>

<span data-ttu-id="d2dd3-367">定义和使用委托时有三个步骤：声明、实例化和调用。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-367">There are three steps in defining and using delegates: declaration, instantiation, and invocation.</span></span>

<span data-ttu-id="d2dd3-368">委托是使用委托声明语法来声明的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-368">Delegates are declared using delegate declaration syntax.</span></span> <span data-ttu-id="d2dd3-369">下面的示例声明一个名为 `SimpleDelegate` 的委托，该委托不采用任何参数：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-369">The following example declares a delegate named `SimpleDelegate` that takes no arguments:</span></span>

```vb
Delegate Sub SimpleDelegate()
```

<span data-ttu-id="d2dd3-370">下一个示例创建一个 `SimpleDelegate` 实例，然后立即调用它：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-370">The next example creates a `SimpleDelegate` instance and then immediately calls it:</span></span>

```vb
Module Test
    Sub F()
        System.Console.WriteLine("Test.F")
    End Sub 

    Sub Main()
        Dim d As SimpleDelegate = AddressOf F
        d()
    End Sub 
End Module
```

<span data-ttu-id="d2dd3-371">为方法实例化委托并随后立即通过委托调用的情况并不多，因为直接调用方法更为简单。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-371">There is not much point in instantiating a delegate for a method and then immediately calling via the delegate, as it would be simpler to call the method directly.</span></span> <span data-ttu-id="d2dd3-372">委托在使用匿名时显示其有用性。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-372">Delegates show their usefulness when their anonymity is used.</span></span> <span data-ttu-id="d2dd3-373">下一个示例演示了一个 `MultiCall` 方法，该方法将重复调用 `SimpleDelegate` 实例：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-373">The next example shows a `MultiCall` method that repeatedly calls a `SimpleDelegate` instance:</span></span>

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

<span data-ttu-id="d2dd3-374">`MultiCall` 方法 `SimpleDelegate` 的目标方法是什么，此方法具有哪些可访问性，或者该方法是否 `Shared`，这一点并不重要。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-374">It is unimportant to the `MultiCall` method what the target method for the `SimpleDelegate` is, what accessibility this method has, or whether the method is `Shared` or not.</span></span> <span data-ttu-id="d2dd3-375">重要的是，目标方法的签名与 `SimpleDelegate`兼容。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-375">All that matters is that the signature of the target method is compatible with `SimpleDelegate`.</span></span>


## <a name="partial-types"></a><span data-ttu-id="d2dd3-376">分部类型</span><span class="sxs-lookup"><span data-stu-id="d2dd3-376">Partial types</span></span>

<span data-ttu-id="d2dd3-377">类和结构声明可以是*分部*声明。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-377">Class and structure declarations can be *partial* declarations.</span></span> <span data-ttu-id="d2dd3-378">分部声明不一定会完全描述声明中的声明类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-378">A partial declaration may or may not fully describe the declared type within the declaration.</span></span> <span data-ttu-id="d2dd3-379">相反，类型的声明可以分布在程序内的多个分部声明中;不能跨程序边界声明分部类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-379">Instead, the declaration of the type may be spread across multiple partial declarations within the program; partial types cannot be declared across program boundaries.</span></span> <span data-ttu-id="d2dd3-380">分部类型声明指定声明上的 `Partial` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-380">A partial type declaration specifies the `Partial` modifier on the declaration.</span></span> <span data-ttu-id="d2dd3-381">然后，程序中具有相同完全限定名称的类型的任何其他声明将在编译时与分部声明一起合并，以形成单个类型声明。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-381">Then, any other declarations in the program for a type with the same fully-qualified name will be merged together with the partial declaration at compile-time to form a single type declaration.</span></span> <span data-ttu-id="d2dd3-382">例如，以下代码声明了一个类，`Test` 成员 `Test.C1` 和 `Test.C2`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-382">For example, the following code declares a single class `Test` with members `Test.C1` and `Test.C2`.</span></span>

<span data-ttu-id="d2dd3-383">.vb：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-383">a.vb:</span></span>

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

<span data-ttu-id="d2dd3-384">b. .vb：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-384">b.vb:</span></span>

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

<span data-ttu-id="d2dd3-385">组合分部类型声明时，至少一个声明必须有 `Partial` 修饰符，否则将产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-385">When combining partial type declarations, at least one of the declarations must have a `Partial` modifier, otherwise a compile-time error results.</span></span>

<span data-ttu-id="d2dd3-386">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="d2dd3-386">__Note.__</span></span> <span data-ttu-id="d2dd3-387">尽管只能在多个分部声明中的一个声明上指定 `Partial`，但最好是在所有分部声明中指定该声明。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-387">Although it is possible to specify `Partial` on only one declaration among many partial declarations, it is better form to specify it on all partial declarations.</span></span> <span data-ttu-id="d2dd3-388">如果一个分部声明可见，但一个或多个分部声明隐藏（例如，扩展工具生成的代码的情况），则可以接受将 `Partial` 修饰符从可见声明中退出，但在隐藏的声明上指定它。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-388">In the situation where one partial declaration is visible but one or more partial declarations are hidden (such as the case of extending tool-generated code), it is acceptable to leave the `Partial` modifier off of the visible declaration but specify it on the hidden declarations.</span></span>

<span data-ttu-id="d2dd3-389">只有类和结构可以使用分部声明来声明。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-389">Only classes and structures can be declared using partial declarations.</span></span> <span data-ttu-id="d2dd3-390">当匹配部分声明时，将考虑类型的 arity：两个名称相同但类型参数不同的类的类不被视为属于同一时间的分部声明。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-390">The arity of a type is considered when matching partial declarations together: two classes with the same name but different numbers of type parameters are not considered to be partial declarations of the same time.</span></span> <span data-ttu-id="d2dd3-391">分部声明可以指定属性、类修饰符、`Inherits` 语句或 `Implements` 语句。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-391">Partial declarations can specify attributes, class modifiers, `Inherits` statement or `Implements` statement.</span></span> <span data-ttu-id="d2dd3-392">在编译时，分部声明的所有部分组合在一起，并用作类型声明的一部分。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-392">At compile time, all of the pieces of the partial declarations are combined together and used as a part of the type declaration.</span></span> <span data-ttu-id="d2dd3-393">如果特性、修饰符、基、接口或类型成员之间存在任何冲突，则会产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-393">If there are any conflicts between attributes, modifiers, bases, interfaces, or type members, a compile-time error results.</span></span> <span data-ttu-id="d2dd3-394">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-394">For example:</span></span>

```vb
Public Partial Class Test1
    Implements IDisposable
End Class

Class Test1
    Inherits Object
    Implements IComparable
End Class

Public Partial Class Test2
End Class

Private Partial Class Test2
End Class
```

<span data-ttu-id="d2dd3-395">前面的示例声明一个 `Public`类型 `Test1`，该类型继承自 `Object` 并实现 `System.IDisposable` 和 `System.IComparable`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-395">The previous example declares a type `Test1` that is `Public`, inherits from `Object` and implements `System.IDisposable` and `System.IComparable`.</span></span> <span data-ttu-id="d2dd3-396">`Test2` 的分部声明会导致编译时错误，这是因为其中一个声明指出 `Test2` 是 `Public`，另 `Test2` 一个声明是 `Private`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-396">The partial declarations of `Test2` will cause a compile-time error because one of the declarations says that `Test2` is `Public` and another says that `Test2` is `Private`.</span></span>

<span data-ttu-id="d2dd3-397">具有类型参数的分部类型可以声明类型参数的约束和方差，但每个分部声明的约束和方差必须匹配。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-397">Partial types with type parameters can declare constraints and variance for the type parameters, but the constraints and variance from each partial declaration must match.</span></span> <span data-ttu-id="d2dd3-398">因此，约束和方差是特殊的，因为它们不会自动组合起来，如其他修饰符：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-398">Thus, constraints and variance are special in that they are not automatically combined like other modifiers:</span></span>

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

<span data-ttu-id="d2dd3-399">使用多个分部声明声明类型的事实不影响类型中的名称查找规则。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-399">The fact that a type is declared using multiple partial declarations does not affect the name lookup rules within the type.</span></span> <span data-ttu-id="d2dd3-400">因此，分部类型声明可以使用在其他分部类型声明中声明的成员，或者可以对其他分部类型声明中声明的接口实现方法。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-400">As a result, a partial type declaration can use members declared in other partial type declarations, or may implement methods on interfaces declared in other partial type declarations.</span></span> <span data-ttu-id="d2dd3-401">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-401">For example:</span></span>

```vb
Public Partial Class Test1
    Implements IDisposable

    Private IsDisposed As Boolean = False
End Class

Class Test1
    Private Sub Dispose() Implements IDisposable.Dispose
        If Not IsDisposed Then
            ...
        End If
    End Sub
End Class
```

<span data-ttu-id="d2dd3-402">嵌套类型也可以具有分部声明。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-402">Nested types can have partial declarations as well.</span></span> <span data-ttu-id="d2dd3-403">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-403">For example:</span></span>

```vb
Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S1()
        End Sub
    End Class
End Class

Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S2()
        End Sub
    End Class
End Class
```

<span data-ttu-id="d2dd3-404">分部声明中的初始值设定项仍将按声明顺序执行;但对于出现在单独分部声明中的初始值设定项，没有保证的执行顺序。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-404">Initializers within a partial declaration will still be executed in declaration order; however, there is no guaranteed order of execution for initializers that occur in separate partial declarations.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="d2dd3-405">构造类型</span><span class="sxs-lookup"><span data-stu-id="d2dd3-405">Constructed Types</span></span>

<span data-ttu-id="d2dd3-406">泛型类型声明本身不表示类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-406">A generic type declaration, by itself, does not denote a type.</span></span> <span data-ttu-id="d2dd3-407">相反，可以通过应用类型参数，将泛型类型声明用作 "蓝图" 以形成多种不同类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-407">Instead, a generic type declaration can be used as a "blueprint" to form many different types by applying type arguments.</span></span> <span data-ttu-id="d2dd3-408">具有应用到它的类型参数的泛型类型称为*构造类型*。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-408">A generic type that has type arguments applied to it is called a *constructed type*.</span></span> <span data-ttu-id="d2dd3-409">构造类型中的类型参数必须始终满足对它们匹配的类型参数上的约束。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-409">The type arguments in a constructed type must always satisfy the constraints placed on the type parameters they match to.</span></span>

<span data-ttu-id="d2dd3-410">类型名称可以标识构造类型，即使它不直接指定类型参数也是如此。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-410">A type name might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="d2dd3-411">如果类型嵌套在泛型类声明中，并且包含声明的实例类型隐式用于名称查找，则可能会发生这种情况：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-411">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup:</span></span>

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

<span data-ttu-id="d2dd3-412">当可以访问泛型类型和所有类型参数时，可以访问构造类型 `C(Of T1,...,Tn)`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-412">A constructed type `C(Of T1,...,Tn)` is accessible when the generic type and all the type arguments are accessible.</span></span> <span data-ttu-id="d2dd3-413">例如，如果泛型类型 `C` `Public` 并且 `T1,...,Tn` 的所有类型参数都是 `Public`的，则构造的类型为 `Public`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-413">For instance, if the generic type `C` is `Public` and all of the type arguments `T1,...,Tn` are `Public`, then the constructed type is `Public`.</span></span> <span data-ttu-id="d2dd3-414">但是，如果类型名称或类型参数之一 `Private`，则构造类型的可访问性将 `Private`。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-414">If either the type name or one of the type arguments is `Private`, however, then the accessibility of the constructed type is `Private`.</span></span> <span data-ttu-id="d2dd3-415">如果构造类型的一个类型参数为 `Protected`，另一个类型参数为 `Friend`，则构造类型只能在此程序集中的类及其子类中访问，或在已提供 `Friend` 访问的任何程序集中访问。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-415">If one type argument of the constructed type is `Protected` and another type argument is `Friend`, then the constructed type is accessible only in the class and its subclasses in this assembly or any assembly that has been given `Friend` access.</span></span> <span data-ttu-id="d2dd3-416">换句话说，构造类型的可访问域是其构成部分的可访问性域的交集。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-416">In other words, the accessibility domain for a constructed type is the intersection of the accessibility domains of its constituent parts.</span></span>

<span data-ttu-id="d2dd3-417">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="d2dd3-417">__Note.__</span></span> <span data-ttu-id="d2dd3-418">构造类型的可访问域是其构成部分的交集，这是定义新的可访问性级别所产生的副作用。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-418">The fact that the accessibility domain of constructed type is the intersection of its constituted parts has the interesting side effect of defining a new accessibility level.</span></span> <span data-ttu-id="d2dd3-419">一个构造类型，它包含 `Protected` 的元素，并且只能在可访问 `Friend`*和*`Protected` 成员的上下文*中访问 `Friend`* 的元素。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-419">A constructed type that contains an element that is `Protected` and an element that is `Friend` can only be accessed in contexts that can access *both* `Friend` *and* `Protected` members.</span></span> <span data-ttu-id="d2dd3-420">但是，无法以语言表达此辅助功能级别，因为辅助功能 `Protected Friend` 意味着可以在可访问 `Friend`*或*`Protected` 成员的上下文*中访问实体*。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-420">However, there is no way to express this accessibility level in the language, as the accessibility `Protected Friend` means that an entity can be accessed in a context that can access *either* `Friend` *or* `Protected` members.</span></span>

<span data-ttu-id="d2dd3-421">通过为泛型类型中的类型参数的每个匹配项替换提供的类型自变量，来确定基实现接口和构造类型的成员。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-421">The base, implemented interfaces and members of constructed types are determined by substituting the supplied type arguments for each occurrence of the type parameter in the generic type.</span></span>

### <a name="open-types-and-closed-types"></a><span data-ttu-id="d2dd3-422">开放式类型和闭合类型</span><span class="sxs-lookup"><span data-stu-id="d2dd3-422">Open Types and Closed Types</span></span>

<span data-ttu-id="d2dd3-423">一个或多个类型自变量是包含类型或方法的类型参数的构造类型称为*开放类型*。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-423">A constructed type for who one or more type arguments are type parameters of a containing type or method is called an *open type*.</span></span> <span data-ttu-id="d2dd3-424">这是因为类型的某些类型参数仍是未知的，因此该类型的实际形状还不是已知的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-424">This is because some of the type parameters of the type are still not known, so the actual shape of the type is not yet fully known.</span></span> <span data-ttu-id="d2dd3-425">相反，其类型参数为所有非类型参数的泛型类型称为*闭合类型*。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-425">In contrast, a generic type whose type arguments are all non-type parameters is called a *closed type*.</span></span> <span data-ttu-id="d2dd3-426">闭合类型的形状始终是完全已知的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-426">The shape of a closed type is always fully known.</span></span> <span data-ttu-id="d2dd3-427">例如：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-427">For example:</span></span>

```vb
Class Base(Of T, V)
End Class

Class Derived(Of V)
    Inherits Base(Of Integer, V)
End Class

Class MoreDerived
    Inherits Derived(Of Double)
End Class
```

<span data-ttu-id="d2dd3-428">构造类型 `Base(Of Integer, V)` 是开放类型，因为虽然提供了 type 参数 `T`，但 `U` 提供了另一个类型参数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-428">The constructed type `Base(Of Integer, V)` is an open type because although the type parameter `T` has been supplied, the type parameter `U` has been supplied another type parameter.</span></span> <span data-ttu-id="d2dd3-429">因此，该类型的完整形状是未知的。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-429">Thus, the full shape of the type is not yet known.</span></span> <span data-ttu-id="d2dd3-430">但是，构造类型 `Derived(Of Double)`是关闭类型，因为已提供了继承层次结构中的所有类型参数。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-430">The constructed type `Derived(Of Double)`, however, is a closed type because all type parameters in the inheritance hierarchy have been supplied.</span></span>

<span data-ttu-id="d2dd3-431">开放式类型定义如下：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-431">Open types are defined as follows:</span></span>

* <span data-ttu-id="d2dd3-432">类型参数是开放类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-432">A type parameter is an open type.</span></span>

* <span data-ttu-id="d2dd3-433">如果数组的元素类型为开放类型，则数组类型为开放类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-433">An array type is an open type if its element type is an open type.</span></span>

* <span data-ttu-id="d2dd3-434">如果构造类型的一个或多个类型参数是开放类型，则构造类型是开放类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-434">A constructed type is an open type if one or more of its type arguments are an open type.</span></span>

* <span data-ttu-id="d2dd3-435">关闭的类型是不是开放类型的类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-435">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="d2dd3-436">由于程序入口点不能是泛型类型，因此在运行时使用的所有类型都将为关闭类型。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-436">Because the program entry point cannot be in a generic type, all types used at run-time will be closed types.</span></span>

## <a name="special-types"></a><span data-ttu-id="d2dd3-437">特殊类型</span><span class="sxs-lookup"><span data-stu-id="d2dd3-437">Special Types</span></span>

<span data-ttu-id="d2dd3-438">.NET Framework 包含许多由 .NET Framework 和 Visual Basic 语言专门处理的类：</span><span class="sxs-lookup"><span data-stu-id="d2dd3-438">The .NET Framework contains a number of classes that are treated specially by the .NET Framework and by the Visual Basic language:</span></span>

<span data-ttu-id="d2dd3-439">类型 `System.Void`表示 .NET Framework 中的 void 类型，只能在 `GetType` 表达式中直接引用。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-439">The type `System.Void`, which represents a void type in the .NET Framework, can be directly referenced only in `GetType` expressions.</span></span>

<span data-ttu-id="d2dd3-440">类型 `System.RuntimeArgumentHandle`、`System.ArgIterator` 和 `System.TypedReference` 都可以包含指向堆栈的指针，因此不能出现在 .NET Framework 堆上。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-440">The types `System.RuntimeArgumentHandle`, `System.ArgIterator` and `System.TypedReference` all can contain pointers into the stack and so cannot appear on the .NET Framework heap.</span></span> <span data-ttu-id="d2dd3-441">因此，它们不能用作数组元素类型、返回类型、字段类型、泛型类型参数、可以为 null 的类型、`ByRef` 参数类型、要转换为 `Object` 或 `System.ValueType`的值的类型、对 `Object` 或 `System.ValueType`的实例成员的调用的目标，或提升为闭包。</span><span class="sxs-lookup"><span data-stu-id="d2dd3-441">Therefore, they cannot be used as array element types, return types, field types, generic type arguments, nullable types, `ByRef` parameter types, the type of a value being converted to `Object` or `System.ValueType`, the target of a call to instance members of `Object` or `System.ValueType`, or lifted into a closure.</span></span>
