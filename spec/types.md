# <a name="types"></a><span data-ttu-id="e75ac-101">类型</span><span class="sxs-lookup"><span data-stu-id="e75ac-101">Types</span></span>

<span data-ttu-id="e75ac-102">在 Visual Basic 中的类型的两个基本类别如下*值类型*并*引用类型*。</span><span class="sxs-lookup"><span data-stu-id="e75ac-102">The two fundamental categories of types in Visual Basic are *value types* and *reference types*.</span></span> <span data-ttu-id="e75ac-103">基元类型 （除了字符串）、 枚举和结构是值类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-103">Primitive types (except strings), enumerations, and structures are value types.</span></span> <span data-ttu-id="e75ac-104">类、 字符串、 标准模块、 接口、 数组和委托是引用类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-104">Classes, strings, standard modules, interfaces, arrays, and delegates are reference types.</span></span>

<span data-ttu-id="e75ac-105">每个类型具有*默认值*，这是分配给在初始化时该类型的变量的值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-105">Every type has a *default value*, which is the value that is assigned to variables of that type upon initialization.</span></span>

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

## <a name="value-types-and-reference-types"></a><span data-ttu-id="e75ac-106">Value Types and Reference Types</span><span class="sxs-lookup"><span data-stu-id="e75ac-106">Value Types and Reference Types</span></span>

<span data-ttu-id="e75ac-107">值类型和引用类型可以是在声明语法和用法方面相似，尽管它们的语义是截然不同的。</span><span class="sxs-lookup"><span data-stu-id="e75ac-107">Although value types and reference types can be similar in terms of declaration syntax and usage, their semantics are distinct.</span></span>

<span data-ttu-id="e75ac-108">引用类型存储在运行时堆中;它们可以通过引用该存储访问。</span><span class="sxs-lookup"><span data-stu-id="e75ac-108">Reference types are stored on the run-time heap; they may only be accessed through a reference to that storage.</span></span> <span data-ttu-id="e75ac-109">由于引用类型始终通过引用访问，由.NET Framework 管理其生存期。</span><span class="sxs-lookup"><span data-stu-id="e75ac-109">Because reference types are always accessed through references, their lifetime is managed by the .NET Framework.</span></span> <span data-ttu-id="e75ac-110">未完成的跟踪对特定实例的引用，并且仅当没有更多的引用保持时销毁该实例。</span><span class="sxs-lookup"><span data-stu-id="e75ac-110">Outstanding references to a particular instance are tracked and the instance is destroyed only when no more references remain.</span></span> <span data-ttu-id="e75ac-111">引用类型的变量包含对该类型的值，派生程度更大的类型的值或 null 值的引用。</span><span class="sxs-lookup"><span data-stu-id="e75ac-111">A variable of reference type contains a reference to a value of that type, a value of a more derived type, or a null value.</span></span> <span data-ttu-id="e75ac-112">一个*null 值*指的是执行任何操作; 不能使用 null 值，但将其分配执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="e75ac-112">A *null value* refers to nothing; it is not possible to do anything with a null value except assign it.</span></span> <span data-ttu-id="e75ac-113">引用类型的变量进行赋值创建引用的副本而不是非引用值的副本。</span><span class="sxs-lookup"><span data-stu-id="e75ac-113">Assignment to a variable of a reference type creates a copy of the reference rather than a copy of the referenced value.</span></span> <span data-ttu-id="e75ac-114">对于引用类型的变量，默认值为 null 值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-114">For a variable of a reference type, the default value is a null value.</span></span>

<span data-ttu-id="e75ac-115">值类型存储在直接在堆栈上，数组内或在另一个类型;仅可以直接访问其存储。</span><span class="sxs-lookup"><span data-stu-id="e75ac-115">Value types are stored directly on the stack, either within an array or within another type; their storage can only be accessed directly.</span></span> <span data-ttu-id="e75ac-116">由于直接在变量中存储的值类型，其生存期由包含它们的变量的生存期确定。</span><span class="sxs-lookup"><span data-stu-id="e75ac-116">Because value types are stored directly within variables, their lifetime is determined by the lifetime of the variable that contains them.</span></span> <span data-ttu-id="e75ac-117">当销毁包含值类型实例的位置时，值类型实例也会被销毁。</span><span class="sxs-lookup"><span data-stu-id="e75ac-117">When the location containing a value type instance is destroyed, the value type instance is also destroyed.</span></span> <span data-ttu-id="e75ac-118">值类型始终进行直接调用访问不能创建对值类型的引用。</span><span class="sxs-lookup"><span data-stu-id="e75ac-118">Value types are always accessed directly; it is not possible to create a reference to a value type.</span></span> <span data-ttu-id="e75ac-119">禁止这种引用会导致无法引用已销毁的值类实例。</span><span class="sxs-lookup"><span data-stu-id="e75ac-119">Prohibiting such a reference makes it impossible to refer to a value class instance that has been destroyed.</span></span> <span data-ttu-id="e75ac-120">由于值类型始终是`NotInheritable`，值类型的变量始终包含该类型的值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-120">Because value types are always `NotInheritable`, a variable of a value type always contains a value of that type.</span></span> <span data-ttu-id="e75ac-121">因此，值类型的值不能为 null 值，也不可以引用派生程度更大的类型的对象。</span><span class="sxs-lookup"><span data-stu-id="e75ac-121">Because of this, the value of a value type cannot be a null value, nor can it reference an object of a more derived type.</span></span> <span data-ttu-id="e75ac-122">值类型的变量进行赋值会创建一份所赋的值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-122">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="e75ac-123">对于值类型的变量，默认值为初始化为其默认值的类型的每个变量成员的结果。</span><span class="sxs-lookup"><span data-stu-id="e75ac-123">For a variable of a value type, the default value is the result of initializing each variable member of the type to its default value.</span></span>

<span data-ttu-id="e75ac-124">下面的示例显示引用类型和值类型之间的差异：</span><span class="sxs-lookup"><span data-stu-id="e75ac-124">The following example shows the difference between reference types and value types:</span></span>

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

<span data-ttu-id="e75ac-125">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="e75ac-125">The output of the program is:</span></span>

```
Values: 0, 123
Refs: 123, 123
```

<span data-ttu-id="e75ac-126">对本地变量的赋值`val2`不会影响本地变量`val1`由于这两个本地变量的值类型 (类型`Integer`) 和值类型的每个本地变量都有其自己的存储。</span><span class="sxs-lookup"><span data-stu-id="e75ac-126">The assignment to the local variable `val2` does not impact the local variable `val1` because both local variables are of a value type (the type `Integer`) and each local variable of a value type has its own storage.</span></span> <span data-ttu-id="e75ac-127">与之相反，赋值`ref2.Value = 123;`影响的对象，同时`ref1`和`ref2`引用。</span><span class="sxs-lookup"><span data-stu-id="e75ac-127">In contrast, the assignment `ref2.Value = 123;` affects the object that both `ref1` and `ref2` reference.</span></span>

<span data-ttu-id="e75ac-128">关于.NET Framework 类型系统需要注意的一点是，即使结构、 枚举和基元类型 (除`String`) 是值类型，它们都继承自引用类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-128">One thing to note about the .NET Framework type system is that even though structures, enumerations and primitive types (except for `String`) are value types, they all inherit from reference types.</span></span> <span data-ttu-id="e75ac-129">结构和基元类型继承自引用类型`System.ValueType`，后者又继承`Object`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-129">Structures and the primitive types inherit from the reference type `System.ValueType`, which inherits from `Object`.</span></span> <span data-ttu-id="e75ac-130">枚举的类型都继承自引用类型`System.Enum`，后者又继承`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-130">Enumerated types inherit from the reference type `System.Enum`, which inherits from `System.ValueType`.</span></span>

### <a name="nullable-value-types"></a><span data-ttu-id="e75ac-131">可以为 Null 的值类型</span><span class="sxs-lookup"><span data-stu-id="e75ac-131">Nullable Value Types</span></span>

<span data-ttu-id="e75ac-132">对于值类型，`?`修饰符可以添加到类型名称来表示*可以为 null*该类型的版本。</span><span class="sxs-lookup"><span data-stu-id="e75ac-132">For value types, a `?` modifier can be added to a type name to represent the *nullable* version of that type.</span></span>

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

<span data-ttu-id="e75ac-133">可以为 null 值类型可以包含相同的值不可以为 null 的类型的版本以及 null 值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-133">A nullable value type can contain the same values as the non-nullable version of the type as well as the null value.</span></span> <span data-ttu-id="e75ac-134">因此，对于可以为 null 的值类型，将分配`Nothing`到类型的变量设置为 null 值，而不是零个值的值类型的变量的值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-134">Thus, for a nullable value type, assigning `Nothing` to a variable of the type sets the value of the variable to the null value, not the zero value of the value type.</span></span> <span data-ttu-id="e75ac-135">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-135">For example:</span></span>

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

<span data-ttu-id="e75ac-136">此外可能会将变量声明为可以为 null 的值类型的通过将为 null 的类型修饰符放在变量名称。</span><span class="sxs-lookup"><span data-stu-id="e75ac-136">A variable may also be declared to be of a nullable value type by putting a nullable type modifier on the variable name.</span></span> <span data-ttu-id="e75ac-137">为清楚起见，不能用来在同一声明中有变量名称和类型名称为 null 的类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="e75ac-137">For clarity, it is not valid to have a nullable type modifier on both a variable name and a type name in the same declaration.</span></span> <span data-ttu-id="e75ac-138">由于可以为 null 的类型使用该类型实现`System.Nullable(Of T)`，类型`T?`等效于类型`System.Nullable(Of T)`，并且这两个名称可以互换使用。</span><span class="sxs-lookup"><span data-stu-id="e75ac-138">Since nullable types are implemented using the type `System.Nullable(Of T)`, the type `T?` is synonymous to the type `System.Nullable(Of T)`, and the two names can be used interchangeably.</span></span> <span data-ttu-id="e75ac-139">`?`修饰符不能将其置于可以为 null 的类型; 因此，不能声明类型`Integer??`或`System.Nullable(Of Integer)?`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-139">The `?` modifier cannot be placed on a type that is already nullable; thus, it is not possible to declare the type `Integer??` or `System.Nullable(Of Integer)?`.</span></span>

<span data-ttu-id="e75ac-140">可以为 null 值类型`T?`具有的成员`System.Nullable(Of T)`以及任何运算符或转换*提升*从基础类型`T`类型到`T?`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-140">A nullable value type `T?` has the members of `System.Nullable(Of T)` as well as any operators or conversions *lifted* from the underlying type `T` into the type `T?`.</span></span> <span data-ttu-id="e75ac-141">提升副本运算符和转换从基础类型，在大多数情况下替换的不可以为 null 的值类型可以为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-141">Lifting copies operators and conversions from the underlying type, in most cases substituting nullable value types for non-nullable value types.</span></span> <span data-ttu-id="e75ac-142">这样，相同的转换和适用于操作的许多`T`要应用于`T?`也。</span><span class="sxs-lookup"><span data-stu-id="e75ac-142">This allows many of the same conversions and operations that apply to `T` to apply to `T?` as well.</span></span>


## <a name="interface-implementation"></a><span data-ttu-id="e75ac-143">接口实现</span><span class="sxs-lookup"><span data-stu-id="e75ac-143">Interface Implementation</span></span>

<span data-ttu-id="e75ac-144">结构和类声明可以声明它们实现的一组通过一个或多个接口类型`Implements`子句。</span><span class="sxs-lookup"><span data-stu-id="e75ac-144">Structure and class declarations may declare that they implement a set of interface types through one or more `Implements` clauses.</span></span>

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="e75ac-145">中指定的所有类型`Implements`子句必须是接口，并且该类型必须实现的接口的所有成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-145">All the types specified in the `Implements` clause must be interfaces, and the type must implement all members of the interfaces.</span></span> <span data-ttu-id="e75ac-146">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-146">For example:</span></span>

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

<span data-ttu-id="e75ac-147">此外将隐式实现接口的类型实现的所有接口的基接口。</span><span class="sxs-lookup"><span data-stu-id="e75ac-147">A type that implements an interface also implicitly implements all of the interface's base interfaces.</span></span> <span data-ttu-id="e75ac-148">即使该类型不会显式列出中的所有基接口是 true`Implements`子句。</span><span class="sxs-lookup"><span data-stu-id="e75ac-148">This is true even if the type does not explicitly list all base interfaces in the `Implements` clause.</span></span> <span data-ttu-id="e75ac-149">在此示例中，`TextBox`结构同时实现了`IControl`和`ITextBox`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-149">In this example, the `TextBox` structure implements both `IControl` and `ITextBox`.</span></span>

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

<span data-ttu-id="e75ac-150">声明一个类型实现的接口，本身不声明类型的声明空间中的任何内容。</span><span class="sxs-lookup"><span data-stu-id="e75ac-150">Declaring that a type implements an interface in and of itself does not declare anything in the declaration space of the type.</span></span> <span data-ttu-id="e75ac-151">因此，它是有效使用的方法实现两个接口，通过相同的名称。</span><span class="sxs-lookup"><span data-stu-id="e75ac-151">Thus, it is valid to implement two interfaces with a method by the same name.</span></span>

<span data-ttu-id="e75ac-152">类型不能实现自身、 类型参数，尽管它可能涉及到作用域中的类型参数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-152">Types cannot implement a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

<span data-ttu-id="e75ac-153">泛型接口可以实现的多次使用不同的类型参数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-153">Generic interfaces can be implemented multiple times using different type arguments.</span></span> <span data-ttu-id="e75ac-154">但是，泛型类型不能实现泛型接口使用类型参数，如果提供的类型参数 （而不考虑类型约束） 无法与另一个实现该接口的重叠。</span><span class="sxs-lookup"><span data-stu-id="e75ac-154">However, a generic type cannot implement a generic interface using a type parameter if the supplied type parameter (regardless of type constraints) could overlap with another implementation of that interface.</span></span> <span data-ttu-id="e75ac-155">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-155">For example:</span></span>

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


## <a name="primitive-types"></a><span data-ttu-id="e75ac-156">基元类型</span><span class="sxs-lookup"><span data-stu-id="e75ac-156">Primitive Types</span></span>

<span data-ttu-id="e75ac-157">*基元类型*通过是预定义别名的关键字进行标识中的类型`System`命名空间。</span><span class="sxs-lookup"><span data-stu-id="e75ac-157">The *primitive types* are identified through keywords, which are aliases for predefined types in the `System` namespace.</span></span> <span data-ttu-id="e75ac-158">基元类型是完全无法区分类型从其别名： 编写保留的字`Byte`正是写入相同`System.Byte`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-158">A primitive type is completely indistinguishable from the type it aliases: writing the reserved word `Byte` is exactly the same as writing `System.Byte`.</span></span> <span data-ttu-id="e75ac-159">基元类型也被称为*内部类型*。</span><span class="sxs-lookup"><span data-stu-id="e75ac-159">Primitive types are also known as *intrinsic types*.</span></span>

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


<span data-ttu-id="e75ac-160">由于是基元类型的别名是常规类型，每个基元类型具有成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-160">Because a primitive type aliases a regular type, every primitive type has members.</span></span> <span data-ttu-id="e75ac-161">例如，`Integer`具有中声明成员`System.Int32`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-161">For example, `Integer` has the members declared in `System.Int32`.</span></span> <span data-ttu-id="e75ac-162">文字可以视为其相应类型的实例。</span><span class="sxs-lookup"><span data-stu-id="e75ac-162">Literals can be treated as instances of their corresponding types.</span></span>

* <span data-ttu-id="e75ac-163">基元类型不同于其他结构类型，不允许某些附加的操作：</span><span class="sxs-lookup"><span data-stu-id="e75ac-163">The primitive types differ from other structure types in that they permit certain additional operations:</span></span>

* <span data-ttu-id="e75ac-164">基元类型允许使用值来创建的编写文本。</span><span class="sxs-lookup"><span data-stu-id="e75ac-164">Primitive types permit values to be created by writing literals.</span></span> <span data-ttu-id="e75ac-165">例如，`123I`是类型的文字`Integer`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-165">For example, `123I` is a literal of type `Integer`.</span></span>

* <span data-ttu-id="e75ac-166">它是可以声明基元类型的常量。</span><span class="sxs-lookup"><span data-stu-id="e75ac-166">It is possible to declare constants of the primitive types.</span></span>

* <span data-ttu-id="e75ac-167">所有基元类型常量表达式的操作数时，就可以为编译器在编译时表达式进行求值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-167">When the operands of an expression are all primitive type constants, it is possible for the compiler to evaluate the expression at compile time.</span></span> <span data-ttu-id="e75ac-168">此类表达式被称为常量表达式。</span><span class="sxs-lookup"><span data-stu-id="e75ac-168">Such an expression is known as a constant expression.</span></span>

<span data-ttu-id="e75ac-169">Visual Basic 定义以下基元类型：</span><span class="sxs-lookup"><span data-stu-id="e75ac-169">Visual Basic defines the following primitive types:</span></span>

* <span data-ttu-id="e75ac-170">整数值类型`Byte`（1 字节无符号的整数）， `SByte` （1 字节有符号的整数）， `UShort` （2 字节无符号的整数）， `Short` （2 字节有符号的整数）， `UInteger` （4 字节无符号的整数）， `Integer` (4 字节有符号的整数)， `ULong` （8 字节无符号的整数） 和`Long`（8 字节有符号的整数）。</span><span class="sxs-lookup"><span data-stu-id="e75ac-170">The integral value types `Byte` (1-byte unsigned integer), `SByte` (1-byte signed integer), `UShort` (2-byte unsigned integer), `Short` (2-byte signed integer), `UInteger` (4-byte unsigned integer), `Integer` (4-byte signed integer), `ULong` (8-byte unsigned integer), and `Long` (8-byte signed integer).</span></span> <span data-ttu-id="e75ac-171">这些类型将映射到`System.Byte`， `System.SByte`， `System.UInt16`， `System.Int16`， `System.UInt32`， `System.Int32`，`System.UInt64`和`System.Int64`分别。</span><span class="sxs-lookup"><span data-stu-id="e75ac-171">These types map to `System.Byte`, `System.SByte`, `System.UInt16`, `System.Int16`, `System.UInt32`, `System.Int32`, `System.UInt64` and `System.Int64`, respectively.</span></span> <span data-ttu-id="e75ac-172">一种整型类型的默认值相当于文字`0`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-172">The default value of an integral type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="e75ac-173">浮点值类型`Single`（4 字节浮点数） 和`Double`（8 字节浮点数）。</span><span class="sxs-lookup"><span data-stu-id="e75ac-173">The floating-point value types `Single` (4-byte floating point) and `Double` (8-byte floating point).</span></span> <span data-ttu-id="e75ac-174">这些类型将映射到`System.Single`和`System.Double`分别。</span><span class="sxs-lookup"><span data-stu-id="e75ac-174">These types map to `System.Single` and `System.Double`, respectively.</span></span> <span data-ttu-id="e75ac-175">浮点类型的默认值相当于文字`0`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-175">The default value of a floating-point type is equivalent to the literal `0`.</span></span>

* <span data-ttu-id="e75ac-176">`Decimal`类型 （16 字节十进制值） 映射到`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-176">The `Decimal` type (16-byte decimal value), which maps to `System.Decimal`.</span></span> <span data-ttu-id="e75ac-177">默认值的小数位数相当于文字`0D`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-177">The default value of decimal is equivalent to the literal `0D`.</span></span>

* <span data-ttu-id="e75ac-178">`Boolean`值类型，表示真值，通常关系或逻辑运算的结果。</span><span class="sxs-lookup"><span data-stu-id="e75ac-178">The `Boolean` value type, which represents a truth value, typically the result of a relational or logical operation.</span></span> <span data-ttu-id="e75ac-179">文本属于类型`System.Boolean`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-179">The literal is of type `System.Boolean`.</span></span> <span data-ttu-id="e75ac-180">默认值`Boolean`类型等效于文字`False`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-180">The default value of the `Boolean` type is equivalent to the literal `False`.</span></span>

* <span data-ttu-id="e75ac-181">`Date`值类型，用于表示日期和/或时间，并将映射到`System.DateTime`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-181">The `Date` value type, which represents a date and/or a time and maps to `System.DateTime`.</span></span> <span data-ttu-id="e75ac-182">默认值`Date`类型等效于文字`# 01/01/0001 12:00:00AM #`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-182">The default value of the `Date` type is equivalent to the literal `# 01/01/0001 12:00:00AM #`.</span></span>

* <span data-ttu-id="e75ac-183">`Char`值类型，用于表示单个 Unicode 字符，并将映射到`System.Char`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-183">The `Char` value type, which represents a single Unicode character and maps to `System.Char`.</span></span> <span data-ttu-id="e75ac-184">默认值`Char`类型等效于常量的表达式`ChrW(0)`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-184">The default value of the `Char` type is equivalent to the constant expression `ChrW(0)`.</span></span>

* <span data-ttu-id="e75ac-185">`String`引用类型，用于表示 Unicode 字符序列，并将映射到`System.String`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-185">The `String` reference type, which represents a sequence of Unicode characters and maps to `System.String`.</span></span> <span data-ttu-id="e75ac-186">默认值`String`类型是一个 null 值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-186">The default value of the `String` type is a null value.</span></span>



## <a name="enumerations"></a><span data-ttu-id="e75ac-187">枚举</span><span class="sxs-lookup"><span data-stu-id="e75ac-187">Enumerations</span></span>

<span data-ttu-id="e75ac-188">*枚举*是继承的值类型`System.Enum`和符号表示一组基元整型类型之一的值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-188">*Enumerations* are value types that inherit from `System.Enum` and symbolically represent a set of values of one of the primitive integral types.</span></span>

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

<span data-ttu-id="e75ac-189">对于枚举类型`E`，默认值是由表达式生成的值`CType(0, E)`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-189">For an enumeration type `E`, the default value is the value produced by the expression `CType(0, E)`.</span></span>

<span data-ttu-id="e75ac-190">枚举的基础类型必须是一种整型类型，可以表示枚举中定义的所有枚举器值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-190">The underlying type of an enumeration must be an integral type that can represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="e75ac-191">如果指定的基础类型，则它必须是`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`，或在其相应的类型之一`System`命名空间。</span><span class="sxs-lookup"><span data-stu-id="e75ac-191">If an underlying type is specified, it must be `Byte`, `SByte`, `UShort`, `Short`, `UInteger`, `Integer`, `ULong`, `Long`, or one of their corresponding types in the `System` namespace.</span></span> <span data-ttu-id="e75ac-192">如果显式不指定任何基础类型，默认值是`Integer`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-192">If no underlying type is explicitly specified, the default is `Integer`.</span></span>

<span data-ttu-id="e75ac-193">下面的示例声明了一个基础类型的枚举`Long`:</span><span class="sxs-lookup"><span data-stu-id="e75ac-193">The following example declares an enumeration with an underlying type of `Long`:</span></span>

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

<span data-ttu-id="e75ac-194">开发人员可以选择使用的基础类型`Long`，如下所示的示例中，若要启用的范围内的值的用法`Long`，但不是在的范围内`Integer`，还是保留此选项可在将来。</span><span class="sxs-lookup"><span data-stu-id="e75ac-194">A developer might choose to use an underlying type of `Long`, as in the example, to enable the use of values that are in the range of `Long`, but not in the range of `Integer`, or to preserve this option for the future.</span></span>


### <a name="enumeration-members"></a><span data-ttu-id="e75ac-195">枚举成员</span><span class="sxs-lookup"><span data-stu-id="e75ac-195">Enumeration Members</span></span>

<span data-ttu-id="e75ac-196">枚举的成员是在枚举中声明的枚举的值和从类继承的成员`System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-196">The members of an enumeration are the enumerated values declared in the enumeration and the members inherited from class `System.Enum`.</span></span>

<span data-ttu-id="e75ac-197">枚举成员的作用域是枚举声明主体。</span><span class="sxs-lookup"><span data-stu-id="e75ac-197">The scope of an enumeration member is the enumeration declaration body.</span></span> <span data-ttu-id="e75ac-198">这意味着，外部枚举声明，枚举成员始终必须限定 （除非该类型是专门导入通过命名空间导入的命名空间）。</span><span class="sxs-lookup"><span data-stu-id="e75ac-198">This means that outside of an enumeration declaration, an enumeration member must always be qualified (unless the type is specifically imported into a namespace through a namespace import).</span></span>

<span data-ttu-id="e75ac-199">当省略常数表达式的值时，枚举成员声明的声明顺序非常重要。</span><span class="sxs-lookup"><span data-stu-id="e75ac-199">Declaration order for enumeration member declarations is significant when constant expression values are omitted.</span></span> <span data-ttu-id="e75ac-200">枚举的成员隐式具有`Public`仅访问; 没有访问修饰符允许对枚举成员声明。</span><span class="sxs-lookup"><span data-stu-id="e75ac-200">Enumeration members implicitly have `Public` access only; no access modifiers are allowed on enumeration member declarations.</span></span>

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a><span data-ttu-id="e75ac-201">枚举值</span><span class="sxs-lookup"><span data-stu-id="e75ac-201">Enumeration Values</span></span>

<span data-ttu-id="e75ac-202">枚举成员列表中的枚举的值被声明为常量类型为基础的枚举类型，并且它们可以出现常数的所需的任何位置。</span><span class="sxs-lookup"><span data-stu-id="e75ac-202">The enumerated values in an enumeration member list are declared as constants typed as the underlying enumeration type, and they can appear wherever constants are required.</span></span> <span data-ttu-id="e75ac-203">一个枚举成员定义`=`为关联的成员提供了所指示的常量表达式的值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-203">An enumeration member definition with `=` gives the associated member the value indicated by the constant expression.</span></span> <span data-ttu-id="e75ac-204">常数表达式的计算结果必须为整型类型的隐式转换为基础类型，而且必须在基础类型可以表示的值的范围内。</span><span class="sxs-lookup"><span data-stu-id="e75ac-204">The constant expression must evaluate to an integral type that is implicitly convertible to the underlying type and must be within the range of values that can be represented by the underlying type.</span></span> <span data-ttu-id="e75ac-205">下面的示例是在错误，因为常量值`1.5`， `2.3`，并`3.3`不是隐式转换为的基础整型类型`Long`具有严格的语义。</span><span class="sxs-lookup"><span data-stu-id="e75ac-205">The following example is in error because the constant values `1.5`, `2.3`, and `3.3` are not implicitly convertible to the underlying integral type `Long` with strict semantics.</span></span>

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

<span data-ttu-id="e75ac-206">多个枚举成员可能会共享相同的关联的值，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e75ac-206">Multiple enumeration members may share the same associated value, as shown below:</span></span>

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

<span data-ttu-id="e75ac-207">该示例演示具有两个枚举成员-枚举`Blue`和`Max`-，具有相同的关联值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-207">The example shows an enumeration that has two enumeration members -- `Blue` and `Max` -- that have the same associated value.</span></span>

<span data-ttu-id="e75ac-208">如果第一个枚举器值定义枚举中的没有初始值设定项，相应的常量的值是`0`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-208">If the first enumerator value definition in the enumeration has no initializer, the value of the corresponding constant is `0`.</span></span> <span data-ttu-id="e75ac-209">没有初始值设定项的枚举值定义为枚举器提供了通过增加由以前的枚举值的值获取的值`1`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-209">An enumeration value definition without an initializer gives the enumerator the value obtained by increasing the value of the previous enumeration value by `1`.</span></span> <span data-ttu-id="e75ac-210">增加的该值必须在基础类型可以表示的值的范围内。</span><span class="sxs-lookup"><span data-stu-id="e75ac-210">This increased value must be within the range of values that can be represented by the underlying type.</span></span>

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

<span data-ttu-id="e75ac-211">上面的示例输出的枚举值和相关联的值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-211">The example above prints the enumeration values and their associated values.</span></span> <span data-ttu-id="e75ac-212">输出为：</span><span class="sxs-lookup"><span data-stu-id="e75ac-212">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="e75ac-213">值的原因是，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e75ac-213">The reasons for the values are as follows:</span></span>

* <span data-ttu-id="e75ac-214">枚举值`Red`自动分配值`0`（因为它没有初始值设定项，并且是第一个枚举值成员）。</span><span class="sxs-lookup"><span data-stu-id="e75ac-214">The enumeration value `Red` is automatically assigned the value `0` (since it has no initializer and is the first enumeration value member).</span></span>

* <span data-ttu-id="e75ac-215">枚举值`Green`显式提供了值`10`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-215">The enumeration value `Green` is explicitly given the value `10`.</span></span>

* <span data-ttu-id="e75ac-216">枚举值`Blue`自动分配的值大于文本上位于前面的枚举值之一。</span><span class="sxs-lookup"><span data-stu-id="e75ac-216">The enumeration value `Blue` is automatically assigned the value one greater than the enumeration value that textually precedes it.</span></span>

<span data-ttu-id="e75ac-217">常量表达式可能会不直接或间接使用其自身关联的枚举值的值 （即，不允许在常量表达式中的循环）。</span><span class="sxs-lookup"><span data-stu-id="e75ac-217">The constant expression may not directly or indirectly use the value of its own associated enumeration value (that is, circularity in the constant expression is not allowed).</span></span> <span data-ttu-id="e75ac-218">下面的示例是无效因为的声明`A`和`B`是循环的。</span><span class="sxs-lookup"><span data-stu-id="e75ac-218">The following example is invalid because the declarations of `A` and `B` are circular.</span></span>

```vb
Enum Circular
    A = B
    B
End Enum
```

<span data-ttu-id="e75ac-219">`A` 取决于`B`显式，并且`B`取决于`A`隐式。</span><span class="sxs-lookup"><span data-stu-id="e75ac-219">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

## <a name="classes"></a><span data-ttu-id="e75ac-220">类</span><span class="sxs-lookup"><span data-stu-id="e75ac-220">Classes</span></span>

<span data-ttu-id="e75ac-221">一个*类*是可能包含数据成员 （常量、 变量和事件）、 函数成员 （方法、 属性、 索引器、 运算符和构造函数） 和嵌套的类型的数据结构。</span><span class="sxs-lookup"><span data-stu-id="e75ac-221">A *class* is a data structure that may contain data members (constants, variables, and events), function members (methods, properties, indexers, operators, and constructors), and nested types.</span></span> <span data-ttu-id="e75ac-222">类是引用类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-222">Classes are reference types.</span></span>

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

<span data-ttu-id="e75ac-223">下面的示例显示了包含每个类型的成员的类：</span><span class="sxs-lookup"><span data-stu-id="e75ac-223">The following example shows a class that contains each kind of member:</span></span>

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

<span data-ttu-id="e75ac-224">下面的示例显示了这些成员的使用：</span><span class="sxs-lookup"><span data-stu-id="e75ac-224">The following example shows uses of these members:</span></span>

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

<span data-ttu-id="e75ac-225">有两个类专用的修饰符，`MustInherit`和`NotInheritable`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-225">There are two class-specific modifiers, `MustInherit` and `NotInheritable`.</span></span> <span data-ttu-id="e75ac-226">若要指定这两个无效。</span><span class="sxs-lookup"><span data-stu-id="e75ac-226">It is invalid to specify them both.</span></span>


### <a name="class-base-specification"></a><span data-ttu-id="e75ac-227">类的基规范</span><span class="sxs-lookup"><span data-stu-id="e75ac-227">Class Base Specification</span></span>

<span data-ttu-id="e75ac-228">类声明可以包含定义类的直接基类型的基类型规范。</span><span class="sxs-lookup"><span data-stu-id="e75ac-228">A class declaration may include a base type specification that defines the direct base type of the class.</span></span>

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

<span data-ttu-id="e75ac-229">如果类声明没有显式的基类型，则直接基类型是隐式`Object`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-229">If a class declaration has no explicit base type, the direct base type is implicitly `Object`.</span></span> <span data-ttu-id="e75ac-230">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-230">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

<span data-ttu-id="e75ac-231">基类型不能为其自身的、 上的类型参数，尽管它可能涉及到作用域中的类型参数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-231">Base types cannot be a type parameter on its own, although it may involve the type parameters that are in scope.</span></span>

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

<span data-ttu-id="e75ac-232">只能从派生类可能`Object`和类。</span><span class="sxs-lookup"><span data-stu-id="e75ac-232">Classes may only derive from `Object` and classes.</span></span> <span data-ttu-id="e75ac-233">是无效的类进行派生`System.ValueType`， `System.Enum`， `System.Array`，`System.MulticastDelegate`或`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-233">It is invalid for a class to derive from `System.ValueType`, `System.Enum`, `System.Array`, `System.MulticastDelegate` or `System.Delegate`.</span></span> <span data-ttu-id="e75ac-234">泛型类不能从派生`System.Attribute`或从其派生的类。</span><span class="sxs-lookup"><span data-stu-id="e75ac-234">A generic class cannot derive from `System.Attribute` or from a class that derives from it.</span></span>

<span data-ttu-id="e75ac-235">每个类具有一个直接基类，并禁止循环中派生。</span><span class="sxs-lookup"><span data-stu-id="e75ac-235">Every class has exactly one direct base class, and circularity in derivation is prohibited.</span></span> <span data-ttu-id="e75ac-236">不能从派生`NotInheritable`类和基类的可访问域必须与相同或类本身的可访问域的超集。</span><span class="sxs-lookup"><span data-stu-id="e75ac-236">It is not possible to derive from a `NotInheritable` class, and the accessibility domain of the base class must be the same as or a superset of the accessibility domain of the class itself.</span></span>


### <a name="class-members"></a><span data-ttu-id="e75ac-237">类成员</span><span class="sxs-lookup"><span data-stu-id="e75ac-237">Class Members</span></span>

<span data-ttu-id="e75ac-238">类的成员由其类成员声明引入的成员和从其直接基类继承的成员组成。</span><span class="sxs-lookup"><span data-stu-id="e75ac-238">The members of a class consist of the members introduced by its class member declarations and the members inherited from its direct base class.</span></span>

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

<span data-ttu-id="e75ac-239">类成员声明可能具有`Public`， `Protected`， `Friend`， `Protected Friend`，或`Private`访问。</span><span class="sxs-lookup"><span data-stu-id="e75ac-239">A class member declaration may have `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` access.</span></span> <span data-ttu-id="e75ac-240">如果类成员声明不包括访问修饰符，声明将默认为`Public`访问权限，除非它是变量的声明; 在这种情况下它将默认为`Private`访问。</span><span class="sxs-lookup"><span data-stu-id="e75ac-240">When a class member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable declaration; in that case it defaults to `Private` access.</span></span>

<span data-ttu-id="e75ac-241">类成员的作用域是类的主体中的成员声明发生，以及此类的约束列表 （如果它是泛型，并具有约束）。</span><span class="sxs-lookup"><span data-stu-id="e75ac-241">The scope of a class member is the class body in which the member declaration occurs, plus the constraint list of that class (if it is generic and has constraints).</span></span> <span data-ttu-id="e75ac-242">如果成员具有`Friend`访问权限，其范围扩展到在同一程序中的任何派生的类或它已被授予的任何程序集的类主体`Friend`访问权限，并且如果成员具有`Public`， `Protected`，或`Protected Friend`访问，其作用域将扩展到任何程序中任何派生类的类的主体。</span><span class="sxs-lookup"><span data-stu-id="e75ac-242">If the member has `Friend` access, its scope extends to the class body of any derived class in the same program or any assembly that has been given `Friend` access, and if the member has `Public`, `Protected`, or `Protected Friend` access, its scope extends to the class body of any derived class in any program.</span></span>


## <a name="structures"></a><span data-ttu-id="e75ac-243">结构</span><span class="sxs-lookup"><span data-stu-id="e75ac-243">Structures</span></span>

<span data-ttu-id="e75ac-244">*结构*是继承的值类型`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-244">*Structures* are value types that inherit from `System.ValueType`.</span></span> <span data-ttu-id="e75ac-245">结构与类很相似，因为它们表示可以包含数据成员和函数成员的数据结构。</span><span class="sxs-lookup"><span data-stu-id="e75ac-245">Structures are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="e75ac-246">与类不同，但是，结构不需要堆分配。</span><span class="sxs-lookup"><span data-stu-id="e75ac-246">Unlike classes, however, structures do not require heap allocation.</span></span>

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

<span data-ttu-id="e75ac-247">对于类，它是两个变量来引用同一对象，并因此可能会对一个变量以影响其他变量所引用的对象的操作。</span><span class="sxs-lookup"><span data-stu-id="e75ac-247">In the case of classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="e75ac-248">对于结构，每个变量都有其自己副本非`Shared`数据，因此它不可能实现的操作的另一个用于会影响其他，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="e75ac-248">With structures, the variables each have their own copy of the non-`Shared` data, so it is not possible for operations on one to affect the other, as the following example illustrates:</span></span>

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="e75ac-249">给定以下代码输出的值的上述声明`10`:</span><span class="sxs-lookup"><span data-stu-id="e75ac-249">Given the above declaration the following code outputs the value `10`:</span></span>

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

<span data-ttu-id="e75ac-250">分配`a`到`b`会创建一份值，并`b`因此不受分配到`a.x`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-250">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="e75ac-251">有`Point`改为已声明为一个类，则输出将为`100`因为`a`和`b`将引用同一对象。</span><span class="sxs-lookup"><span data-stu-id="e75ac-251">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>


### <a name="structure-members"></a><span data-ttu-id="e75ac-252">结构成员</span><span class="sxs-lookup"><span data-stu-id="e75ac-252">Structure Members</span></span>

<span data-ttu-id="e75ac-253">一种结构的成员是其结构成员声明引入的成员和成员继承自`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-253">The members of a structure are the members introduced by its structure member declarations and the members inherited from `System.ValueType`.</span></span>

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

<span data-ttu-id="e75ac-254">每个结构都隐式具有`Public`生成结构的默认值的无参数实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-254">Every structure implicitly has a `Public` parameterless instance constructor that produces the default value of the structure.</span></span> <span data-ttu-id="e75ac-255">因此，不能为结构类型声明可以声明一个无参数实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-255">As a result, it is not possible for a structure type declaration to declare a parameterless instance constructor.</span></span> <span data-ttu-id="e75ac-256">结构类型，但是，允许以声明*参数化*实例构造函数，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="e75ac-256">A structure type is, however, permitted to declare *parameterized* instance constructors, as in the following example:</span></span>

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

<span data-ttu-id="e75ac-257">给定上述声明，以下语句都创建`Point`与`x`和`y`初始化为零。</span><span class="sxs-lookup"><span data-stu-id="e75ac-257">Given the above declaration, the following statements both create a `Point` with `x` and `y` initialized to zero.</span></span>

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

<span data-ttu-id="e75ac-258">由于结构直接包含其字段值 （而非对这些值的引用），结构不能包含直接或间接地引用其自身的字段。</span><span class="sxs-lookup"><span data-stu-id="e75ac-258">Because structures directly contain their field values (rather than references to those values), structures cannot contain fields that directly or indirectly reference themselves.</span></span> <span data-ttu-id="e75ac-259">例如，下面的代码不是有效的：</span><span class="sxs-lookup"><span data-stu-id="e75ac-259">For example, the following code is not valid:</span></span>

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

<span data-ttu-id="e75ac-260">通常情况下，只能有一个结构成员声明`Public`， `Friend`，或`Private`访问权限，但当重写成员继承自`Object`，`Protected`和`Protected Friend`访问，也可以使用。</span><span class="sxs-lookup"><span data-stu-id="e75ac-260">Normally, a structure member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, `Protected` and `Protected Friend` access may also be used.</span></span> <span data-ttu-id="e75ac-261">结构成员声明中不包括访问修饰符，声明将默认为`Public`访问。</span><span class="sxs-lookup"><span data-stu-id="e75ac-261">When a structure member declaration does not include an access modifier, the declaration defaults to `Public` access.</span></span> <span data-ttu-id="e75ac-262">声明为结构成员的作用域是结构正文在其中进行声明，加上该结构的约束 （如果它是泛型类型和有约束）。</span><span class="sxs-lookup"><span data-stu-id="e75ac-262">The scope of a member declared by a structure is the structure body in which the declaration occurs, plus the constraints of that structure (if it was generic and had constraints).</span></span>


## <a name="standard-modules"></a><span data-ttu-id="e75ac-263">标准模块</span><span class="sxs-lookup"><span data-stu-id="e75ac-263">Standard Modules</span></span>

<span data-ttu-id="e75ac-264">一个*标准模块*是一种类型的成员是隐式`Shared`且范围限定到标准模块包含的命名空间声明空间而不是只需标准模块声明本身。</span><span class="sxs-lookup"><span data-stu-id="e75ac-264">A *standard module* is a type whose members are implicitly `Shared` and scoped to the declaration space of the standard module's containing namespace, rather than just to the standard module declaration itself.</span></span> <span data-ttu-id="e75ac-265">标准模块可能永远不会实例化。</span><span class="sxs-lookup"><span data-stu-id="e75ac-265">Standard modules may never be instantiated.</span></span> <span data-ttu-id="e75ac-266">它是错误声明的变量的标准模块类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-266">It is an error to declare a variable of a standard module type.</span></span>

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

<span data-ttu-id="e75ac-267">标准模块的成员具有两个完全限定的名称，一个没有标准的模块名称，另一个具有标准模块名称。</span><span class="sxs-lookup"><span data-stu-id="e75ac-267">A member of a standard module has two fully qualified names, one without the standard module name and one with the standard module name.</span></span> <span data-ttu-id="e75ac-268">多个命名空间中的标准模块可以定义特定的名称; 的成员对任一模块的外部名称的非限定的引用不明确。</span><span class="sxs-lookup"><span data-stu-id="e75ac-268">More than one standard module in a namespace may define a member with a particular name; unqualified references to the name outside of either module are ambiguous.</span></span> <span data-ttu-id="e75ac-269">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-269">For example:</span></span>

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

<span data-ttu-id="e75ac-270">模块可能只声明了命名空间中，可能不会嵌套在另一种类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-270">A module may only be declared in a namespace and may not be nested in another type.</span></span> <span data-ttu-id="e75ac-271">标准模块不能实现接口，它们隐式派生`Object`，并且仅具有`Shared`构造函数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-271">Standard modules may not implement interfaces, they implicitly derive from `Object`, and they have only `Shared` constructors.</span></span>


### <a name="standard-module-members"></a><span data-ttu-id="e75ac-272">标准模块成员</span><span class="sxs-lookup"><span data-stu-id="e75ac-272">Standard Module Members</span></span>

<span data-ttu-id="e75ac-273">标准模块的成员是其成员声明引入的成员和成员继承自`Object`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-273">The members of a standard module are the members introduced by its member declarations and the members inherited from `Object`.</span></span> <span data-ttu-id="e75ac-274">标准模块可能会有任何类型的实例构造函数除外的成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-274">Standard modules may have any type of member except instance constructors.</span></span> <span data-ttu-id="e75ac-275">所有标准模块类型成员都是隐式`Shared`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-275">All standard module type members are implicitly `Shared`.</span></span>

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

<span data-ttu-id="e75ac-276">通常情况下，只能有一个标准的模块成员声明`Public`， `Friend`，或`Private`访问权限，但当重写成员继承自`Object`，则`Protected`和`Protected Friend`可能指定访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="e75ac-276">Normally, a standard module member declaration may only have `Public`, `Friend`, or `Private` access, but when overriding members inherited from `Object`, the `Protected` and `Protected Friend` access modifiers may be specified.</span></span> <span data-ttu-id="e75ac-277">如果标准模块成员声明中不包括访问修饰符，声明将默认为`Public`访问，除非它是变量，默认为`Private`访问。</span><span class="sxs-lookup"><span data-stu-id="e75ac-277">When a standard module member declaration does not include an access modifier, the declaration defaults to `Public` access, unless it is a variable, which defaults to `Private` access.</span></span>

<span data-ttu-id="e75ac-278">如上文所述，标准模块成员的作用域是包含标准模块声明的声明。</span><span class="sxs-lookup"><span data-stu-id="e75ac-278">As previously noted, the scope of a standard module member is the declaration containing the standard module declaration.</span></span> <span data-ttu-id="e75ac-279">成员继承自`Object`未包含在此特殊作用域; 这些成员具有无作用域，始终必须使用的模块的名称限定。</span><span class="sxs-lookup"><span data-stu-id="e75ac-279">Members inherited from `Object` are not included in this special scoping; those members have no scope and must always be qualified with the name of the module.</span></span> <span data-ttu-id="e75ac-280">如果成员具有`Friend`仅对在同一个程序或程序集的具有给定中声明的命名空间成员的访问权限，其作用域扩展`Friend`访问。</span><span class="sxs-lookup"><span data-stu-id="e75ac-280">If the member has `Friend` access, its scope extends only to namespace members declared in the same program or assemblies that have been given `Friend` access.</span></span>


## <a name="interfaces"></a><span data-ttu-id="e75ac-281">接口</span><span class="sxs-lookup"><span data-stu-id="e75ac-281">Interfaces</span></span>

<span data-ttu-id="e75ac-282">*接口*是引用类型的其他类型实现此方法可以保证它们支持某些方法。</span><span class="sxs-lookup"><span data-stu-id="e75ac-282">*Interfaces* are reference types that other types implement to guarantee that they support certain methods.</span></span> <span data-ttu-id="e75ac-283">永远不会直接创建一个接口，并没有实际的表示形式--其他类型必须转换为接口类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-283">An interface is never directly created and has no actual representation -- other types must be converted to an interface type.</span></span> <span data-ttu-id="e75ac-284">接口定义一个协定。</span><span class="sxs-lookup"><span data-stu-id="e75ac-284">An interface defines a contract.</span></span> <span data-ttu-id="e75ac-285">类或结构实现的接口必须遵守其协定。</span><span class="sxs-lookup"><span data-stu-id="e75ac-285">A class or structure that implements an interface must adhere to its contract.</span></span>

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


<span data-ttu-id="e75ac-286">下面的示例演示包含默认属性的接口`Item`，事件`E`，一种方法`F`，和一个属性`P`:</span><span class="sxs-lookup"><span data-stu-id="e75ac-286">The following example shows an interface that contains a default property `Item`, an event `E`, a method `F`, and a property `P`:</span></span>

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

<span data-ttu-id="e75ac-287">接口可以采用多个继承。</span><span class="sxs-lookup"><span data-stu-id="e75ac-287">Interfaces may employ multiple inheritance.</span></span> <span data-ttu-id="e75ac-288">在下面的示例中，该接口`IComboBox`同时继承`ITextBox`和`IListBox`:</span><span class="sxs-lookup"><span data-stu-id="e75ac-288">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`:</span></span>

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

<span data-ttu-id="e75ac-289">类和结构可以实现多个接口。</span><span class="sxs-lookup"><span data-stu-id="e75ac-289">Classes and structures can implement multiple interfaces.</span></span> <span data-ttu-id="e75ac-290">在下面的示例中，类`EditBox`派生自类`Control`并同时实现`IControl`和`IDataBound`:</span><span class="sxs-lookup"><span data-stu-id="e75ac-290">In the following example, the class `EditBox` derives from the class `Control` and implements both `IControl` and `IDataBound`:</span></span>

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


### <a name="interface-inheritance"></a><span data-ttu-id="e75ac-291">接口继承</span><span class="sxs-lookup"><span data-stu-id="e75ac-291">Interface Inheritance</span></span>

<span data-ttu-id="e75ac-292">接口的基接口是显式的基接口和它们的基接口。</span><span class="sxs-lookup"><span data-stu-id="e75ac-292">The base interfaces of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="e75ac-293">换而言之，组的基接口是完全的显式基接口，其显式基接口，诸如此类的可传递闭包。</span><span class="sxs-lookup"><span data-stu-id="e75ac-293">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="e75ac-294">如果接口声明具有没有显式基接口，则该类型没有基接口-接口不继承自`Object`(尽管它们具有扩大转换为`Object`)。</span><span class="sxs-lookup"><span data-stu-id="e75ac-294">If an interface declaration has no explicit interface base, then there is no base interface for the type -- interfaces do not inherit from `Object` (although they do have a widening conversion to `Object`).</span></span>

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

<span data-ttu-id="e75ac-295">在下面的示例中，接口的基`IComboBox`都`IControl`， `ITextBox`，和`IListBox`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-295">In the following example, the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

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

<span data-ttu-id="e75ac-296">接口继承其基接口的所有成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-296">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="e75ac-297">换而言之，`IComboBox`上述接口继承的成员`SetText`并`SetItems`以及`Paint`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-297">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="e75ac-298">由类或结构，它还将隐式实现接口实现的所有接口的基接口。</span><span class="sxs-lookup"><span data-stu-id="e75ac-298">A class or structure that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

<span data-ttu-id="e75ac-299">如果接口的基接口的传递闭包中多次出现，它仅分配给派生接口及其成员一次。</span><span class="sxs-lookup"><span data-stu-id="e75ac-299">If an interface appears more than once in the transitive closure of the base interfaces, it only contributes its members to the derived interface once.</span></span> <span data-ttu-id="e75ac-300">类型实现派生的接口仅有以实现 multiply 方法定义一次基接口。</span><span class="sxs-lookup"><span data-stu-id="e75ac-300">A type implementing the derived interface only has to implement the methods of the multiply defined base interface once.</span></span> <span data-ttu-id="e75ac-301">在以下示例中，`Paint`只需实现一次，即使类实现`IComboBox`和`IControl`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-301">In the following example, `Paint` only needs to be implemented once, even though the class implements `IComboBox` and `IControl`.</span></span>

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

<span data-ttu-id="e75ac-302">`Inherits`子句没有任何影响其他`Inherits`子句。</span><span class="sxs-lookup"><span data-stu-id="e75ac-302">An `Inherits` clause has no effect on other `Inherits` clauses.</span></span> <span data-ttu-id="e75ac-303">在以下示例中，`IDerived`必须限定的名称`INested`与`IBase`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-303">In the following example, `IDerived` must qualify the name of `INested` with `IBase`.</span></span>

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

<span data-ttu-id="e75ac-304">基接口的可访问域必须与相同或接口本身的可访问域的超集。</span><span class="sxs-lookup"><span data-stu-id="e75ac-304">The accessibility domain of a base interface must be the same as or a superset of the accessibility domain of the interface itself.</span></span>


### <a name="interface-members"></a><span data-ttu-id="e75ac-305">接口成员</span><span class="sxs-lookup"><span data-stu-id="e75ac-305">Interface Members</span></span>

<span data-ttu-id="e75ac-306">接口的成员包括其成员声明引入的成员和从其基接口继承的成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-306">The members of an interface consist of the members introduced by its member declarations and the members inherited from its base interfaces.</span></span>

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

<span data-ttu-id="e75ac-307">尽管接口不会继承中的成员`Object`，因为每个类或结构实现接口 does 继承`Object`的成员`Object`，其中包括扩展方法、 被视为一个接口的成员并且可以直接而无需强制转换为在接口上调用`Object`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-307">Although interfaces do not inherit members from `Object`, because every class or structure that implements an interface does inherit from `Object`, the members of `Object`, including extension methods, are considered members of an interface and can be called on an interface directly without requiring a cast to `Object`.</span></span> <span data-ttu-id="e75ac-308">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-308">For example:</span></span>

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

<span data-ttu-id="e75ac-309">具有相同名称的接口的成员的成员作为`Object`隐式阴影`Object`成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-309">Members of an interface with the same name as members of `Object` implicitly shadow `Object` members.</span></span> <span data-ttu-id="e75ac-310">只有嵌套类型、方法、属性和事件才能作为接口成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-310">Only nested types, methods, properties, and events may be members of an interface.</span></span> <span data-ttu-id="e75ac-311">方法和属性可能不具有一个主体。</span><span class="sxs-lookup"><span data-stu-id="e75ac-311">Methods and properties may not have a body.</span></span> <span data-ttu-id="e75ac-312">接口成员为隐式`Public`和不能指定访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="e75ac-312">Interface members are implicitly `Public` and may not specify an access modifier.</span></span> <span data-ttu-id="e75ac-313">在接口中声明的作用域是成员的接口正文在其中进行声明，以及该接口的约束列表 （如果它是成员的泛型，并具有约束）。</span><span class="sxs-lookup"><span data-stu-id="e75ac-313">The scope of a member declared in an interface is the interface body in which the declaration occurs, plus the constraint list of that interface (if it is generic and has constraints).</span></span>


## <a name="arrays"></a><span data-ttu-id="e75ac-314">数组</span><span class="sxs-lookup"><span data-stu-id="e75ac-314">Arrays</span></span>

<span data-ttu-id="e75ac-315">*数组*是引用类型，其中包含通过访问变量*索引*数组中的变量的顺序一对一的方式相对应。</span><span class="sxs-lookup"><span data-stu-id="e75ac-315">An *array* is a reference type that contains variables accessed through *indices* corresponding in a one-to-one fashion with the order of the variables in the array.</span></span> <span data-ttu-id="e75ac-316">一个数组中包含的变量也称为*元素*的数组，所有的必须是相同的类型，并且这种类型称为*元素类型*的数组。</span><span class="sxs-lookup"><span data-stu-id="e75ac-316">The variables contained in an array, also called the *elements* of the array, must all be of the same type, and this type is called the *element type* of the array.</span></span>

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

<span data-ttu-id="e75ac-317">数组的元素创建数组实例后，就应该考虑存在并在销毁数组实例时停止存在。</span><span class="sxs-lookup"><span data-stu-id="e75ac-317">The elements of an array come into existence when an array instance is created, and cease to exist when the array instance is destroyed.</span></span> <span data-ttu-id="e75ac-318">数组的每个元素初始化为其类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="e75ac-318">Each element of an array is initialized to the default value of its type.</span></span> <span data-ttu-id="e75ac-319">类型`System.Array`是所有数组类型的基类型，不可能实例化。</span><span class="sxs-lookup"><span data-stu-id="e75ac-319">The type `System.Array` is the base type of all array types and may not be instantiated.</span></span> <span data-ttu-id="e75ac-320">每个数组类型继承的成员来声明`System.Array`类型，并且转换为它 (和`Object`)。</span><span class="sxs-lookup"><span data-stu-id="e75ac-320">Every array type inherits the members declared by the `System.Array` type and is convertible to it (and `Object`).</span></span> <span data-ttu-id="e75ac-321">一维数组类型与元素`T`还实现了接口`System.Collections.Generic.IList(Of T)`并`IReadOnlyList(Of T)`; 如果`T`为引用类型，则数组类型还实现`IList(Of U)`和`IReadOnlyList(Of U)`为任何`U`，其引用从转换扩大`T`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-321">A one-dimensional array type with element `T` also implements the interfaces `System.Collections.Generic.IList(Of T)` and `IReadOnlyList(Of T)`; if `T` is a reference type, then the array type also implements `IList(Of U)` and `IReadOnlyList(Of U)` for any `U` that has a widening  reference conversion from `T`.</span></span>

<span data-ttu-id="e75ac-322">数组具有*排名*，它确定与每个数组元素相关联的索引数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-322">An array has a *rank* that determines the number of indices associated with each array element.</span></span> <span data-ttu-id="e75ac-323">数组的秩确定的数量*维度*的数组。</span><span class="sxs-lookup"><span data-stu-id="e75ac-323">The rank of an array determines the number of *dimensions* of the array.</span></span> <span data-ttu-id="e75ac-324">例如，具有一个级别的数组称为一维数组，，具有秩大于 1 的数组称为多维数组。</span><span class="sxs-lookup"><span data-stu-id="e75ac-324">For example, an array with a rank of one is called a single-dimensional array, and an array with a rank greater than one is called a multidimensional array.</span></span>

<span data-ttu-id="e75ac-325">下面的示例创建整数值的一维数组，初始化数组元素，然后输出每个出：</span><span class="sxs-lookup"><span data-stu-id="e75ac-325">The following example creates a single-dimensional array of integer values, initializes the array elements, and then prints each of them out:</span></span>

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

<span data-ttu-id="e75ac-326">程序输出结果如下：</span><span class="sxs-lookup"><span data-stu-id="e75ac-326">The program outputs the following:</span></span>

```
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

<span data-ttu-id="e75ac-327">数组的每个维度具有一个关联的长度。</span><span class="sxs-lookup"><span data-stu-id="e75ac-327">Each dimension of an array has an associated length.</span></span> <span data-ttu-id="e75ac-328">维的长度不是数组，该类型的一部分，但在运行时创建的数组类型的实例时而建立。</span><span class="sxs-lookup"><span data-stu-id="e75ac-328">Dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run time.</span></span> <span data-ttu-id="e75ac-329">维度的长度决定了该维度的索引的有效范围： 维度的长度`N`，索引可以介于 0 到`N-1`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-329">The length of a dimension determines the valid range of indices for that dimension: for a dimension of length `N`, indices can range from zero to `N-1`.</span></span> <span data-ttu-id="e75ac-330">如果维度的长度为零，没有该维度的有效索引。</span><span class="sxs-lookup"><span data-stu-id="e75ac-330">If a dimension is of length zero, there are no valid indices for that dimension.</span></span> <span data-ttu-id="e75ac-331">数组中元素的总数是数组中每个维度的长度的产品。</span><span class="sxs-lookup"><span data-stu-id="e75ac-331">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="e75ac-332">如果任何数组的维度的长度为零，则称该数组为空。</span><span class="sxs-lookup"><span data-stu-id="e75ac-332">If any of the dimensions of an array has a length of zero, the array is said to be empty.</span></span> <span data-ttu-id="e75ac-333">数组的元素类型可以是任何类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-333">The element type of an array can be any type.</span></span>

<span data-ttu-id="e75ac-334">通过将修饰符添加到现有的类型名称指定数组类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-334">Array types are specified by adding a modifier to an existing type name.</span></span> <span data-ttu-id="e75ac-335">修饰符包含左的括号，零个或多个逗号，一组和右括号。</span><span class="sxs-lookup"><span data-stu-id="e75ac-335">The modifier consists of a left parenthesis, a set of zero or more commas, and a right parenthesis.</span></span> <span data-ttu-id="e75ac-336">修改的类型是数组的元素类型和维度的数目是逗号加 1 的数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-336">The type modified is the element type of the array, and the number of dimensions is the number of commas plus one.</span></span> <span data-ttu-id="e75ac-337">如果指定多个修饰符，则数组的元素类型是一个数组。</span><span class="sxs-lookup"><span data-stu-id="e75ac-337">If more than one modifier is specified, then the element type of the array is an array.</span></span> <span data-ttu-id="e75ac-338">修饰符是左到右读取，使用最左侧的修饰符是最外层的数组。</span><span class="sxs-lookup"><span data-stu-id="e75ac-338">The modifiers are read left to right, with the leftmost modifier being the outermost array.</span></span> <span data-ttu-id="e75ac-339">在示例</span><span class="sxs-lookup"><span data-stu-id="e75ac-339">In the example</span></span>

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

<span data-ttu-id="e75ac-340">元素类型`arr`是一个三维数组的一维数组的元素的二维数组`Integer`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-340">the element type of `arr` is a two-dimensional array of three-dimensional arrays of one-dimensional arrays of `Integer`.</span></span>

<span data-ttu-id="e75ac-341">此外可能会将变量声明为数组类型的通过将数组的类型修饰符或数组大小初始化修饰符放在变量名称。</span><span class="sxs-lookup"><span data-stu-id="e75ac-341">A variable may also be declared to be of an array type by putting an array type modifier or an array-size initialization modifier on the variable name.</span></span> <span data-ttu-id="e75ac-342">在这种情况下，数组元素类型是在声明中，给定的类型和数组维度由变量名称修饰符。</span><span class="sxs-lookup"><span data-stu-id="e75ac-342">In that case, the array element type is the type given in the declaration, and the array dimensions are determined by the variable name modifier.</span></span> <span data-ttu-id="e75ac-343">为清楚起见，不能用来在同一声明中有数组类型修饰符的变量名称和类型名称。</span><span class="sxs-lookup"><span data-stu-id="e75ac-343">For clarity, it is not valid to have an array type modifier on both a variable name and a type name in the same declaration.</span></span>

<span data-ttu-id="e75ac-344">下面的示例演示使用具有数组类型的局部变量声明的各种`Integer`与元素类型：</span><span class="sxs-lookup"><span data-stu-id="e75ac-344">The following example shows a variety of local variable declarations that use array types with `Integer` as the element type:</span></span>

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

<span data-ttu-id="e75ac-345">数组类型名称修饰符将扩展到其后面的括号内的所有集。</span><span class="sxs-lookup"><span data-stu-id="e75ac-345">An array type name modifier extends to all sets of parentheses that follow it.</span></span> <span data-ttu-id="e75ac-346">这意味着，在一组参数括在括号中允许类型名称之后的位置的情况下，它不能指定为数组的类型名称的参数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-346">This means that in the situations where a set of arguments enclosed in parenthesis is allowed after a type name, it is not possible to specify the arguments for an array type name.</span></span> <span data-ttu-id="e75ac-347">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-347">For example:</span></span>

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

<span data-ttu-id="e75ac-348">在最后一种情况下，`(3)`被解释为类型名称的一部分而不是一组构造函数参数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-348">In the last case, `(3)` is interpreted as part of the type name rather than as a set of constructor arguments.</span></span>


## <a name="delegates"></a><span data-ttu-id="e75ac-349">委托</span><span class="sxs-lookup"><span data-stu-id="e75ac-349">Delegates</span></span>

<span data-ttu-id="e75ac-350">一个*委派*是一个引用类型，是指`Shared`方法的类型或实例方法的对象。</span><span class="sxs-lookup"><span data-stu-id="e75ac-350">A *delegate* is a reference type that refers to a `Shared` method of a type or to an instance method of an object.</span></span>

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 <span data-ttu-id="e75ac-351">最接近的其他语言中的委托是函数指针，而只能引用函数指针，但`Shared`函数，委托可以引用同时`Shared`和实例方法。</span><span class="sxs-lookup"><span data-stu-id="e75ac-351">The closest equivalent of a delegate in other languages is a function pointer, but whereas a function pointer can only reference `Shared` functions, a delegate can reference both `Shared` and instance methods.</span></span> <span data-ttu-id="e75ac-352">在后一种情况下，委托不仅存储存储的引用方法的入口点，还对用来调用该方法的对象实例的引用。</span><span class="sxs-lookup"><span data-stu-id="e75ac-352">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance with which to invoke the method.</span></span>

<span data-ttu-id="e75ac-353">委托声明可能没有`Handles`子句中，`Implements`子句中，方法体中，或`End`构造。</span><span class="sxs-lookup"><span data-stu-id="e75ac-353">The delegate declaration may not have  a `Handles` clause, an `Implements` clause, a method body, or an `End` construct.</span></span> <span data-ttu-id="e75ac-354">委托声明的参数列表不能包含`Optional`或`ParamArray`参数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-354">The parameter list of the delegate declaration may not contain `Optional` or `ParamArray` parameters.</span></span> <span data-ttu-id="e75ac-355">返回类型和参数类型的可访问域必须与相同或委托本身的可访问域的超集。</span><span class="sxs-lookup"><span data-stu-id="e75ac-355">The accessibility domain of the return type and parameter types must be the same as or a superset of the accessibility domain of the delegate itself.</span></span>

<span data-ttu-id="e75ac-356">委托的成员都是从类继承的成员`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-356">The members of a delegate are the members inherited from class `System.Delegate`.</span></span> <span data-ttu-id="e75ac-357">委托还定义了以下方法：</span><span class="sxs-lookup"><span data-stu-id="e75ac-357">A delegate also defines the following methods:</span></span>

* <span data-ttu-id="e75ac-358">采用两个参数，一个类型的构造函数`Object`，另一个类型`System.IntPtr`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-358">A constructor that takes two parameters, one of type `Object` and one of type `System.IntPtr`.</span></span>

* <span data-ttu-id="e75ac-359">`Invoke`具有相同的签名与委托的方法。</span><span class="sxs-lookup"><span data-stu-id="e75ac-359">An `Invoke` method that has the same signature as the delegate.</span></span>

* <span data-ttu-id="e75ac-360">一个`BeginInvoke`方法的签名的委托签名，有三个差异。</span><span class="sxs-lookup"><span data-stu-id="e75ac-360">A `BeginInvoke` method whose signature is the delegate signature, with three differences.</span></span> <span data-ttu-id="e75ac-361">首先，将返回类型更改为`System.IAsyncResult`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-361">First, the return type is changed to `System.IAsyncResult`.</span></span> <span data-ttu-id="e75ac-362">其次，两个附加参数添加到参数列表的末尾： 类型的第一个`System.AsyncCallback`类型，第二个`Object`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-362">Second, two additional parameters are added to the end of the parameter list: the first of type `System.AsyncCallback` and the second of type `Object`.</span></span> <span data-ttu-id="e75ac-363">最后，所有`ByRef`参数已发生更改，为`ByVal`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-363">And finally, all `ByRef` parameters are changed to be `ByVal`.</span></span>

* <span data-ttu-id="e75ac-364">`EndInvoke`其返回类型是与委托相同的方法。</span><span class="sxs-lookup"><span data-stu-id="e75ac-364">An `EndInvoke` method whose return type is the same as the delegate.</span></span> <span data-ttu-id="e75ac-365">该方法的参数是仅的委托参数，这一点是`ByRef`参数，它们在委托签名中出现的顺序相同。</span><span class="sxs-lookup"><span data-stu-id="e75ac-365">The parameters of the method are only the delegate parameters exactly that are `ByRef` parameters, in the same order they occur in the delegate signature.</span></span>  <span data-ttu-id="e75ac-366">除了这些参数，还有另一个类型参数`System.IAsyncResult`参数列表的末尾。</span><span class="sxs-lookup"><span data-stu-id="e75ac-366">In addition to those parameters, there is an additional parameter of type `System.IAsyncResult` at the end of the parameter list.</span></span>

<span data-ttu-id="e75ac-367">有三个步骤中定义和使用委托： 声明、 实例化和调用。</span><span class="sxs-lookup"><span data-stu-id="e75ac-367">There are three steps in defining and using delegates: declaration, instantiation, and invocation.</span></span>

<span data-ttu-id="e75ac-368">使用委托声明语法声明委托。</span><span class="sxs-lookup"><span data-stu-id="e75ac-368">Delegates are declared using delegate declaration syntax.</span></span> <span data-ttu-id="e75ac-369">下面的示例声明名为的委托`SimpleDelegate`不采用任何参数：</span><span class="sxs-lookup"><span data-stu-id="e75ac-369">The following example declares a delegate named `SimpleDelegate` that takes no arguments:</span></span>

```vb
Delegate Sub SimpleDelegate()
```

<span data-ttu-id="e75ac-370">下一个示例创建`SimpleDelegate`实例，然后立即调用：</span><span class="sxs-lookup"><span data-stu-id="e75ac-370">The next example creates a `SimpleDelegate` instance and then immediately calls it:</span></span>

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

<span data-ttu-id="e75ac-371">存在不太多点中实例化委托的方法，然后立即调用通过委托，因为它将直接调用该方法更简单。</span><span class="sxs-lookup"><span data-stu-id="e75ac-371">There is not much point in instantiating a delegate for a method and then immediately calling via the delegate, as it would be simpler to call the method directly.</span></span> <span data-ttu-id="e75ac-372">委托的匿名使用时显示它们的有效性。</span><span class="sxs-lookup"><span data-stu-id="e75ac-372">Delegates show their usefulness when their anonymity is used.</span></span> <span data-ttu-id="e75ac-373">下面的示例说明`MultiCall`反复调用的方法`SimpleDelegate`实例：</span><span class="sxs-lookup"><span data-stu-id="e75ac-373">The next example shows a `MultiCall` method that repeatedly calls a `SimpleDelegate` instance:</span></span>

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

<span data-ttu-id="e75ac-374">它对并不重要`MultiCall`方法目标方法的`SimpleDelegate`是，哪些辅助功能具有此方法，或该方法是否是`Shared`或不。</span><span class="sxs-lookup"><span data-stu-id="e75ac-374">It is unimportant to the `MultiCall` method what the target method for the `SimpleDelegate` is, what accessibility this method has, or whether the method is `Shared` or not.</span></span> <span data-ttu-id="e75ac-375">最重要的就是目标方法的签名是与兼容`SimpleDelegate`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-375">All that matters is that the signature of the target method is compatible with `SimpleDelegate`.</span></span>


## <a name="partial-types"></a><span data-ttu-id="e75ac-376">分部类型</span><span class="sxs-lookup"><span data-stu-id="e75ac-376">Partial types</span></span>

<span data-ttu-id="e75ac-377">类和结构声明可*分部*声明。</span><span class="sxs-lookup"><span data-stu-id="e75ac-377">Class and structure declarations can be *partial* declarations.</span></span> <span data-ttu-id="e75ac-378">分部声明可能会或可能不充分描述了在声明中声明的类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-378">A partial declaration may or may not fully describe the declared type within the declaration.</span></span> <span data-ttu-id="e75ac-379">相反，类型的声明可能分布在多个分部声明中程序;不能跨程序边界声明分部类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-379">Instead, the declaration of the type may be spread across multiple partial declarations within the program; partial types cannot be declared across program boundaries.</span></span> <span data-ttu-id="e75ac-380">分部类型声明指定`Partial`声明上的修饰符。</span><span class="sxs-lookup"><span data-stu-id="e75ac-380">A partial type declaration specifies the `Partial` modifier on the declaration.</span></span> <span data-ttu-id="e75ac-381">然后，将在编译时以形成单个的类型声明的分部声明以及合并类型具有相同的完全限定名称的程序中的任何其他声明。</span><span class="sxs-lookup"><span data-stu-id="e75ac-381">Then, any other declarations in the program for a type with the same fully-qualified name will be merged together with the partial declaration at compile-time to form a single type declaration.</span></span> <span data-ttu-id="e75ac-382">例如，下面的代码声明一个类`Test`与成员`Test.C1`和`Test.C2`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-382">For example, the following code declares a single class `Test` with members `Test.C1` and `Test.C2`.</span></span>

<span data-ttu-id="e75ac-383">a.vb:</span><span class="sxs-lookup"><span data-stu-id="e75ac-383">a.vb:</span></span>

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

<span data-ttu-id="e75ac-384">b.vb:</span><span class="sxs-lookup"><span data-stu-id="e75ac-384">b.vb:</span></span>

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

<span data-ttu-id="e75ac-385">当结合使用分部类型声明，在至少其中一个声明必须具有`Partial`修饰符，否则编译时错误结果。</span><span class="sxs-lookup"><span data-stu-id="e75ac-385">When combining partial type declarations, at least one of the declarations must have a `Partial` modifier, otherwise a compile-time error results.</span></span>

<span data-ttu-id="e75ac-386">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="e75ac-386">__Note.__</span></span> <span data-ttu-id="e75ac-387">但也可以指定`Partial`上只有一个声明之间数量的分部声明中，它是更好的窗体以所有分部声明中指定它。</span><span class="sxs-lookup"><span data-stu-id="e75ac-387">Although it is possible to specify `Partial` on only one declaration among many partial declarations, it is better form to specify it on all partial declarations.</span></span> <span data-ttu-id="e75ac-388">在一个分部声明可见但一个或多个分部声明隐藏 （如扩展工具生成的代码的情况下） 情况下，则可以保留`Partial`从可见声明修饰符但中指定它隐藏的声明。</span><span class="sxs-lookup"><span data-stu-id="e75ac-388">In the situation where one partial declaration is visible but one or more partial declarations are hidden (such as the case of extending tool-generated code), it is acceptable to leave the `Partial` modifier off of the visible declaration but specify it on the hidden declarations.</span></span>

<span data-ttu-id="e75ac-389">只有类和结构可以使用分部声明声明。</span><span class="sxs-lookup"><span data-stu-id="e75ac-389">Only classes and structures can be declared using partial declarations.</span></span> <span data-ttu-id="e75ac-390">分部声明一起进行匹配时，被视为一种类型的实参数量： 不被视为具有相同名称但不同数量的类型参数的两个类是在同一时间的分部声明。</span><span class="sxs-lookup"><span data-stu-id="e75ac-390">The arity of a type is considered when matching partial declarations together: two classes with the same name but different numbers of type parameters are not considered to be partial declarations of the same time.</span></span> <span data-ttu-id="e75ac-391">分部声明可以指定属性、 类修饰符`Inherits`语句或`Implements`语句。</span><span class="sxs-lookup"><span data-stu-id="e75ac-391">Partial declarations can specify attributes, class modifiers, `Inherits` statement or `Implements` statement.</span></span> <span data-ttu-id="e75ac-392">在编译时，所有分部声明的各个部分组合在一起，使用的类型声明的一部分。</span><span class="sxs-lookup"><span data-stu-id="e75ac-392">At compile time, all of the pieces of the partial declarations are combined together and used as a part of the type declaration.</span></span> <span data-ttu-id="e75ac-393">如果属性，修饰符，之间存在任何冲突基础，接口或类型成员的编译时错误结果。</span><span class="sxs-lookup"><span data-stu-id="e75ac-393">If there are any conflicts between attributes, modifiers, bases, interfaces, or type members, a compile-time error results.</span></span> <span data-ttu-id="e75ac-394">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-394">For example:</span></span>

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

<span data-ttu-id="e75ac-395">前面的示例声明的类型`Test1`也就是说`Public`，继承自`Object`并实现`System.IDisposable`和`System.IComparable`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-395">The previous example declares a type `Test1` that is `Public`, inherits from `Object` and implements `System.IDisposable` and `System.IComparable`.</span></span> <span data-ttu-id="e75ac-396">分部声明`Test2`将导致编译时错误，因为其中一个声明指出`Test2`是`Public`和另一个指出`Test2`是`Private`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-396">The partial declarations of `Test2` will cause a compile-time error because one of the declarations says that `Test2` is `Public` and another says that `Test2` is `Private`.</span></span>

<span data-ttu-id="e75ac-397">使用类型参数的分部类型可以声明约束和方差的类型参数，但约束和方差从每个分部声明必须匹配。</span><span class="sxs-lookup"><span data-stu-id="e75ac-397">Partial types with type parameters can declare constraints and variance for the type parameters, but the constraints and variance from each partial declaration must match.</span></span> <span data-ttu-id="e75ac-398">因此，约束和方差比较特殊，因为不会自动合并等其他修饰符：</span><span class="sxs-lookup"><span data-stu-id="e75ac-398">Thus, constraints and variance are special in that they are not automatically combined like other modifiers:</span></span>

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

<span data-ttu-id="e75ac-399">使用多个分部声明声明一个类型这一事实并不影响类型中的名称查找规则。</span><span class="sxs-lookup"><span data-stu-id="e75ac-399">The fact that a type is declared using multiple partial declarations does not affect the name lookup rules within the type.</span></span> <span data-ttu-id="e75ac-400">因此，分部类型声明可以使用中的其他分部类型声明，声明的成员，或可能在其他分部类型声明中声明的接口上实现方法。</span><span class="sxs-lookup"><span data-stu-id="e75ac-400">As a result, a partial type declaration can use members declared in other partial type declarations, or may implement methods on interfaces declared in other partial type declarations.</span></span> <span data-ttu-id="e75ac-401">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-401">For example:</span></span>

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

<span data-ttu-id="e75ac-402">嵌套的类型可以有也分部声明。</span><span class="sxs-lookup"><span data-stu-id="e75ac-402">Nested types can have partial declarations as well.</span></span> <span data-ttu-id="e75ac-403">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-403">For example:</span></span>

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

<span data-ttu-id="e75ac-404">初始值设定项中的分部声明仍将按声明顺序; 继续执行但是，没有初始值设定项在单独的分部声明中发生的执行顺序并未保证。</span><span class="sxs-lookup"><span data-stu-id="e75ac-404">Initializers within a partial declaration will still be executed in declaration order; however, there is no guaranteed order of execution for initializers that occur in separate partial declarations.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="e75ac-405">构造的类型</span><span class="sxs-lookup"><span data-stu-id="e75ac-405">Constructed Types</span></span>

<span data-ttu-id="e75ac-406">泛型类型声明，其本身而言，不代表一种类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-406">A generic type declaration, by itself, does not denote a type.</span></span> <span data-ttu-id="e75ac-407">相反，泛型类型声明可以使用作为"蓝图"，以通过应用类型参数构成许多不同类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-407">Instead, a generic type declaration can be used as a "blueprint" to form many different types by applying type arguments.</span></span> <span data-ttu-id="e75ac-408">具有应用于它的类型参数的泛型类型称为*构造类型*。</span><span class="sxs-lookup"><span data-stu-id="e75ac-408">A generic type that has type arguments applied to it is called a *constructed type*.</span></span> <span data-ttu-id="e75ac-409">在构造类型中的类型参数必须始终满足放置到匹配的类型形参的约束。</span><span class="sxs-lookup"><span data-stu-id="e75ac-409">The type arguments in a constructed type must always satisfy the constraints placed on the type parameters they match to.</span></span>

<span data-ttu-id="e75ac-410">类型名称可以确定一个构造的类型，即使它不直接指定类型参数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-410">A type name might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="e75ac-411">这可能发生其中一种类型嵌套在泛型类声明中，并且包含声明的实例类型隐式用于名称查找：</span><span class="sxs-lookup"><span data-stu-id="e75ac-411">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup:</span></span>

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

<span data-ttu-id="e75ac-412">构造的类型`C(Of T1,...,Tn)`可访问的泛型类型和所有类型参数时，才可访问。</span><span class="sxs-lookup"><span data-stu-id="e75ac-412">A constructed type `C(Of T1,...,Tn)` is accessible when the generic type and all the type arguments are accessible.</span></span> <span data-ttu-id="e75ac-413">例如，如果泛型类型`C`是`Public`和类型参数的所有`T1,...,Tn`是`Public`，然后构造的类型是`Public`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-413">For instance, if the generic type `C` is `Public` and all of the type arguments `T1,...,Tn` are `Public`, then the constructed type is `Public`.</span></span> <span data-ttu-id="e75ac-414">如果类型名称或类型参数之一`Private`，但是，则构造类型的可访问性是`Private`。</span><span class="sxs-lookup"><span data-stu-id="e75ac-414">If either the type name or one of the type arguments is `Private`, however, then the accessibility of the constructed type is `Private`.</span></span> <span data-ttu-id="e75ac-415">如果一个类型参数的构造类型是`Protected`和另一个类型参数是`Friend`，然后构造的类型是只能在类，并在此程序集或已被授予任何程序集及其子类中访问`Friend`访问。</span><span class="sxs-lookup"><span data-stu-id="e75ac-415">If one type argument of the constructed type is `Protected` and another type argument is `Friend`, then the constructed type is accessible only in the class and its subclasses in this assembly or any assembly that has been given `Friend` access.</span></span> <span data-ttu-id="e75ac-416">换而言之，构造类型的可访问域是其组成部分的可访问性域的交集。</span><span class="sxs-lookup"><span data-stu-id="e75ac-416">In other words, the accessibility domain for a constructed type is the intersection of the accessibility domains of its constituent parts.</span></span>

<span data-ttu-id="e75ac-417">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="e75ac-417">__Note.__</span></span> <span data-ttu-id="e75ac-418">构造类型的可访问域是其有序的各个部分的交集的事实具有定义新的可访问性级别的有趣的副作用。</span><span class="sxs-lookup"><span data-stu-id="e75ac-418">The fact that the accessibility domain of constructed type is the intersection of its constituted parts has the interesting side effect of defining a new accessibility level.</span></span> <span data-ttu-id="e75ac-419">包含的元素，是一个构造的类型`Protected`的元素，并`Friend`只能在可以访问的上下文中访问*两者* `Friend` *和* `Protected`成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-419">A constructed type that contains an element that is `Protected` and an element that is `Friend` can only be accessed in contexts that can access *both* `Friend` *and* `Protected` members.</span></span> <span data-ttu-id="e75ac-420">但是，没有办法表达中的语言，可访问性为此可访问性级别`Protected Friend`意味着可以在可访问的上下文中访问实体*任一* `Friend` *或*`Protected`成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-420">However, there is no way to express this accessibility level in the language, as the accessibility `Protected Friend` means that an entity can be accessed in a context that can access *either* `Friend` *or* `Protected` members.</span></span>

<span data-ttu-id="e75ac-421">由类型参数的泛型类型的每个匹配项在提供的类型实参替换决定的基实现的接口和构造类型的成员。</span><span class="sxs-lookup"><span data-stu-id="e75ac-421">The base, implemented interfaces and members of constructed types are determined by substituting the supplied type arguments for each occurrence of the type parameter in the generic type.</span></span>

### <a name="open-types-and-closed-types"></a><span data-ttu-id="e75ac-422">开放类型和封闭的类型</span><span class="sxs-lookup"><span data-stu-id="e75ac-422">Open Types and Closed Types</span></span>

<span data-ttu-id="e75ac-423">的一个或多个类型参数是包含类型或方法的类型参数的构造的类型称为*打开类型*。</span><span class="sxs-lookup"><span data-stu-id="e75ac-423">A constructed type for who one or more type arguments are type parameters of a containing type or method is called an *open type*.</span></span> <span data-ttu-id="e75ac-424">这是因为某些类型的类型参数仍然不知道，因此该类型的实际形状还不完全知道。</span><span class="sxs-lookup"><span data-stu-id="e75ac-424">This is because some of the type parameters of the type are still not known, so the actual shape of the type is not yet fully known.</span></span> <span data-ttu-id="e75ac-425">与此相反，该类型形参的实参是所有的非类型参数的泛型类型称为*封闭类型*。</span><span class="sxs-lookup"><span data-stu-id="e75ac-425">In contrast, a generic type whose type arguments are all non-type parameters is called a *closed type*.</span></span> <span data-ttu-id="e75ac-426">始终完全已知的封闭类型的形状。</span><span class="sxs-lookup"><span data-stu-id="e75ac-426">The shape of a closed type is always fully known.</span></span> <span data-ttu-id="e75ac-427">例如：</span><span class="sxs-lookup"><span data-stu-id="e75ac-427">For example:</span></span>

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

<span data-ttu-id="e75ac-428">构造的类型`Base(Of Integer, V)`是开放类型，因为尽管类型参数`T`未提供类型参数`U`已被所提供的另一个类型参数。</span><span class="sxs-lookup"><span data-stu-id="e75ac-428">The constructed type `Base(Of Integer, V)` is an open type because although the type parameter `T` has been supplied, the type parameter `U` has been supplied another type parameter.</span></span> <span data-ttu-id="e75ac-429">因此，完整类型的形状还不知道。</span><span class="sxs-lookup"><span data-stu-id="e75ac-429">Thus, the full shape of the type is not yet known.</span></span> <span data-ttu-id="e75ac-430">构造的类型`Derived(Of Double)`，但是，因为提供的继承层次结构中的所有类型参数是关闭的类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-430">The constructed type `Derived(Of Double)`, however, is a closed type because all type parameters in the inheritance hierarchy have been supplied.</span></span>

<span data-ttu-id="e75ac-431">打开类型定义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e75ac-431">Open types are defined as follows:</span></span>

* <span data-ttu-id="e75ac-432">类型参数是开放类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-432">A type parameter is an open type.</span></span>

* <span data-ttu-id="e75ac-433">数组类型是开放类型，其元素类型是否为开放类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-433">An array type is an open type if its element type is an open type.</span></span>

* <span data-ttu-id="e75ac-434">构造的类型是开放式类型; 如果一个或多个其类型参数为开放类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-434">A constructed type is an open type if one or more of its type arguments are an open type.</span></span>

* <span data-ttu-id="e75ac-435">关闭的类型是不是开放类型的类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-435">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="e75ac-436">由于程序入口点不能为泛型类型中，在运行时使用的所有类型都将封闭的类型。</span><span class="sxs-lookup"><span data-stu-id="e75ac-436">Because the program entry point cannot be in a generic type, all types used at run-time will be closed types.</span></span>

## <a name="special-types"></a><span data-ttu-id="e75ac-437">特殊类型</span><span class="sxs-lookup"><span data-stu-id="e75ac-437">Special Types</span></span>

<span data-ttu-id="e75ac-438">.NET Framework 包含多个将被视为特殊由.NET Framework 和 Visual Basic 语言的类：</span><span class="sxs-lookup"><span data-stu-id="e75ac-438">The .NET Framework contains a number of classes that are treated specially by the .NET Framework and by the Visual Basic language:</span></span>

<span data-ttu-id="e75ac-439">类型`System.Void`，它表示在.NET Framework 中，void 类型，可以直接引用仅在`GetType`表达式。</span><span class="sxs-lookup"><span data-stu-id="e75ac-439">The type `System.Void`, which represents a void type in the .NET Framework, can be directly referenced only in `GetType` expressions.</span></span>

<span data-ttu-id="e75ac-440">类型`System.RuntimeArgumentHandle`，`System.ArgIterator`和`System.TypedReference`所有可以包含的指针到堆栈，因此不能出现在.NET Framework 堆上。</span><span class="sxs-lookup"><span data-stu-id="e75ac-440">The types `System.RuntimeArgumentHandle`, `System.ArgIterator` and `System.TypedReference` all can contain pointers into the stack and so cannot appear on the .NET Framework heap.</span></span> <span data-ttu-id="e75ac-441">因此，它们不能用作数组元素类型、 返回类型、 字段类型、 泛型类型参数，可以为 null 的类型`ByRef`参数类型和值转换为类型`Object`或`System.ValueType`，到实例调用的目标成员`Object`或`System.ValueType`，或提升到闭包。</span><span class="sxs-lookup"><span data-stu-id="e75ac-441">Therefore, they cannot be used as array element types, return types, field types, generic type arguments, nullable types, `ByRef` parameter types, the type of a value being converted to `Object` or `System.ValueType`, the target of a call to instance members of `Object` or `System.ValueType`, or lifted into a closure.</span></span>
