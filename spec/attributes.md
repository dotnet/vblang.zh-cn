# <a name="attributes"></a><span data-ttu-id="4b31f-101">特性</span><span class="sxs-lookup"><span data-stu-id="4b31f-101">Attributes</span></span>

<span data-ttu-id="4b31f-102">Visual Basic 语言使程序员能够指定修饰符上声明，表示正在声明的实体有关的信息。</span><span class="sxs-lookup"><span data-stu-id="4b31f-102">The Visual Basic language enables the programmer to specify modifiers on declarations, which represent information about the entities being declared.</span></span> <span data-ttu-id="4b31f-103">例如，附加和修饰符类方法`Public`， `Protected`， `Friend`， `Protected Friend`，或`Private`指定其可访问性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-103">For example, affixing a class method with the modifiers `Public`, `Protected`, `Friend`, `Protected Friend`, or `Private` specifies its accessibility.</span></span>

<span data-ttu-id="4b31f-104">除了由语言定义的修饰符，Visual Basic 还使程序员可以创建名为的新修饰符*特性*，将其声明新实体时。</span><span class="sxs-lookup"><span data-stu-id="4b31f-104">In addition to the modifiers defined by the language, Visual Basic also enables programmers to create new modifiers, called *attributes*, and to use them when declaring new entities.</span></span> <span data-ttu-id="4b31f-105">这些新修饰符，通过特性类的声明定义，然后分配给通过实体*特性块*。</span><span class="sxs-lookup"><span data-stu-id="4b31f-105">These new modifiers, which are defined through the declaration of attribute classes, are then assigned to entities through *attribute blocks*.</span></span>

<span data-ttu-id="4b31f-106">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="4b31f-106">__Note.__</span></span> <span data-ttu-id="4b31f-107">可以在运行时通过.NET Framework 反射 Api 检索属性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-107">Attributes may be retrieved at run time through the .NET Framework's reflection APIs.</span></span> <span data-ttu-id="4b31f-108">这些 Api 是超出了本规范的范围。</span><span class="sxs-lookup"><span data-stu-id="4b31f-108">These APIs are outside the scope of this specification.</span></span>

<span data-ttu-id="4b31f-109">例如，可以定义一个框架`Help`可以放在如类和方法，以提供到文档中，从程序元素的映射的程序元素中，如以下示例所示的属性：</span><span class="sxs-lookup"><span data-stu-id="4b31f-109">For instance, a framework might define a `Help` attribute that can be placed on program elements such as classes and methods to provide a mapping from program elements to documentation, as the following example demonstrates:</span></span>

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class HelpAttribute
    Inherits Attribute

    Public Sub New(urlValue As String)
        Me.UrlValue = urlValue
    End Sub

    Public Topic As String
    Private UrlValue As String

    Public ReadOnly Property Url() As String
        Get
            Return UrlValue
        End Get
    End Property
End Class
```

<span data-ttu-id="4b31f-110">该示例定义一个名为属性类`HelpAttribute`，或`Help`缩写，它具有一个位置参数 (`UrlValue`) 和一个名为参数 (`Topic`)。</span><span class="sxs-lookup"><span data-stu-id="4b31f-110">The example defines an attribute class named `HelpAttribute`, or `Help` for short, that has one positional parameter (`UrlValue`) and one named argument (`Topic`).</span></span>

<span data-ttu-id="4b31f-111">下面的示例演示几种用法的属性：</span><span class="sxs-lookup"><span data-stu-id="4b31f-111">The next example shows several uses of the attribute:</span></span>

```vb
<Help("http://www.example.com/.../Class1.htm")> _
Public Class Class1
    <Help("http://www.example.com/.../Class1.htm", Topic:="F")> _
    Public Sub F()
    End Sub
End Class
```

<span data-ttu-id="4b31f-112">下一步的示例将检查以查看是否`Class1`已`Help`属性，并写出关联`Topic`和`Url`值是否存在该属性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-112">The next example checks to see if `Class1` has a `Help` attribute, and writes out the associated `Topic` and `Url` values if the attribute is present.</span></span>

```vb
Module Test
    Sub Main()
        Dim type As Type = GetType(Class1)
        Dim arr() As Object = _
            type.GetCustomAttributes(GetType(HelpAttribute), True)

        If arr.Length = 0 Then
            Console.WriteLine("Class1 has no Help attribute.")
        Else
            Dim ha As HelpAttribute = CType(arr(0), HelpAttribute)
            Console.WriteLine("Url = " & ha.Url & ", Topic = " & ha.Topic)
        End If
    End Sub
End Module
```

## <a name="attribute-classes"></a><span data-ttu-id="4b31f-113">属性类</span><span class="sxs-lookup"><span data-stu-id="4b31f-113">Attribute Classes</span></span>

<span data-ttu-id="4b31f-114">*特性类*是派生自非泛型类`System.Attribute`并不是`MustInherit`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-114">An *attribute class* is a non-generic class that derives from `System.Attribute` and is not `MustInherit`.</span></span> <span data-ttu-id="4b31f-115">特性类可能具有`System.AttributeUsage`声明该属性是对有效、 是否它可能使用多个时间在声明中，以及是否继承的属性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-115">The attribute class may have a `System.AttributeUsage` attribute that declares what the attribute is valid on, whether it may be used multiple times in a declaration, and whether it is inherited.</span></span> <span data-ttu-id="4b31f-116">下面的示例定义一个名为属性类`SimpleAttribute`可放置在类声明和接口声明上：</span><span class="sxs-lookup"><span data-stu-id="4b31f-116">The following example defines an attribute class named `SimpleAttribute` that can be placed on class declarations and interface declarations:</span></span>

```vb
<AttributeUsage(AttributeTargets.Class Or AttributeTargets.Interface)> _
Public Class SimpleAttribute
    Inherits System.Attribute
End Class
```

<span data-ttu-id="4b31f-117">下面的示例演示的一些用途`Simple`属性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-117">The next example shows a few uses of the `Simple` attribute.</span></span> <span data-ttu-id="4b31f-118">尽管为属性类，但`SimpleAttribute`，可以忽略此属性的用法`Attribute`后缀，从而缩短该名称与`Simple`:</span><span class="sxs-lookup"><span data-stu-id="4b31f-118">Although the attribute class is named `SimpleAttribute`, uses of this attribute may omit the `Attribute` suffix, thus shortening the name to `Simple`:</span></span>

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

<span data-ttu-id="4b31f-119">如果该属性缺少`System.AttributeUsage`，则该属性可以置于任何目标 (等效于`AttributeTargets.All`)。</span><span class="sxs-lookup"><span data-stu-id="4b31f-119">If the attribute lacks a `System.AttributeUsage`, then the attribute can be placed on any target (equivalent to `AttributeTargets.All`).</span></span> <span data-ttu-id="4b31f-120">`System.AttributeUsage`属性具有变量的初始值设定项， `AllowMultiple`，它指定是否可以多次指定指示的属性对于给定的声明。</span><span class="sxs-lookup"><span data-stu-id="4b31f-120">The `System.AttributeUsage` attribute has a variable initializer, `AllowMultiple`, which specifies whether the indicated attribute can be specified more than once for a given declaration.</span></span> <span data-ttu-id="4b31f-121">如果`AllowMultiple`属性是`True`，它是*多用途特性类*，并指定一次在声明。</span><span class="sxs-lookup"><span data-stu-id="4b31f-121">If `AllowMultiple` for an attribute is `True`, it is a *multiple-use attribute class*, and can be specified more than once on a declaration.</span></span> <span data-ttu-id="4b31f-122">如果`AllowMultiple`属性是`False`或未指定的属性，它是*一次性特性类*，并指定最多一次在声明。</span><span class="sxs-lookup"><span data-stu-id="4b31f-122">If `AllowMultiple` for an attribute is `False` or unspecified for an attribute, it is a *single-use attribute class*, and can be specified at most once on a declaration.</span></span>

<span data-ttu-id="4b31f-123">下面的示例定义一个名为的多用途特性类`AuthorAttribute`:</span><span class="sxs-lookup"><span data-stu-id="4b31f-123">The following example defines a multiple-use attribute class named `AuthorAttribute`:</span></span>

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True)> _
Public Class AuthorAttribute
    Inherits System.Attribute

    Private _Value As String

    Public Sub New(value As String)
        Me._Value = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return _Value
        End Get
    End Property
End Class
```

<span data-ttu-id="4b31f-124">该示例演示具有两种含义的类声明`Author`属性：</span><span class="sxs-lookup"><span data-stu-id="4b31f-124">The example shows a class declaration with two uses of the `Author` attribute:</span></span>

```vb
<Author("Maria Hammond"), Author("Ramesh Meyyappan")> _
Class Class1
End Class
```

<span data-ttu-id="4b31f-125">`System.AttributeUsage`属性具有一个公共的实例变量`Inherited`，指定是否属性，指定为基类型上时还继承自此基类型派生的类型。</span><span class="sxs-lookup"><span data-stu-id="4b31f-125">The `System.AttributeUsage` attribute has a public instance variable, `Inherited`, that specifies whether the attribute, when specified on a base type, is also inherited by types that derive from this base type.</span></span> <span data-ttu-id="4b31f-126">如果`Inherited`公共实例变量未初始化，默认值为`False`使用。</span><span class="sxs-lookup"><span data-stu-id="4b31f-126">If the `Inherited` public instance variable is not initialized, a default value of `False` is used.</span></span> <span data-ttu-id="4b31f-127">属性和事件不会继承属性，尽管执行这一定义属性和事件的方法。</span><span class="sxs-lookup"><span data-stu-id="4b31f-127">Properties and events do not inherit attributes, although the methods defined by properties and events do.</span></span> <span data-ttu-id="4b31f-128">接口不会继承属性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-128">Interfaces do not inherit attributes.</span></span>

<span data-ttu-id="4b31f-129">如果一次性属性同时继承和派生类型上指定，派生类型上指定的属性重写继承的特性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-129">If a single-use attribute is both inherited and specified on a derived type, the attribute specified on the derived type overrides the inherited attribute.</span></span> <span data-ttu-id="4b31f-130">如果多用途特性同时继承和派生类型上指定，则派生类型上指定这两个属性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-130">If a multiple-use attribute is both inherited and specified on a derived type, both attributes are specified on the derived type.</span></span> <span data-ttu-id="4b31f-131">例如：</span><span class="sxs-lookup"><span data-stu-id="4b31f-131">For example:</span></span>

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True, _
                Inherited:=True) > _
Class MultiUseAttribute
    Inherits System.Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<AttributeUsage(AttributeTargets.Class, Inherited:=True)> _
Class SingleUseAttribute
    Inherits Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<SingleUse(True), MultiUse(True)> Class Base
End Class

' Derived has three attributes defined on it: SingleUse(False),
' MultiUse(True) and MultiUse(False)
<SingleUse(False), MultiUse(False)> _
Class Derived
    Inherits Base
End Class
```

<span data-ttu-id="4b31f-132">由属性类的公共构造函数的参数定义特性的位置的参数。</span><span class="sxs-lookup"><span data-stu-id="4b31f-132">The positional parameters of the attribute are defined by the parameters of the public constructors of the attribute class.</span></span> <span data-ttu-id="4b31f-133">位置参数必须是`ByVal`并不能指定`ByRef`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-133">Positional parameters must be `ByVal` and may not specify `ByRef`.</span></span> <span data-ttu-id="4b31f-134">由公共读写属性或特性类的实例变量定义公共实例变量和属性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-134">Public instance variables and properties are defined by public read-write properties or instance variables of the attribute class.</span></span> <span data-ttu-id="4b31f-135">可以使用位置参数和公共实例变量和属性中的类型仅限于属性类型。</span><span class="sxs-lookup"><span data-stu-id="4b31f-135">The types that can be used in positional parameters and public instance variables and properties are restricted to attribute types.</span></span> <span data-ttu-id="4b31f-136">类型为属性类型，如果它是以下之一：</span><span class="sxs-lookup"><span data-stu-id="4b31f-136">A type is an attribute type if it is one of the following:</span></span>

* <span data-ttu-id="4b31f-137">除任何基元类型`Date`和`Decimal`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-137">Any primitive type except for `Date` and `Decimal`.</span></span>

* <span data-ttu-id="4b31f-138">`Object` 类型。</span><span class="sxs-lookup"><span data-stu-id="4b31f-138">The type `Object`.</span></span>

* <span data-ttu-id="4b31f-139">`System.Type` 类型。</span><span class="sxs-lookup"><span data-stu-id="4b31f-139">The type `System.Type`.</span></span>

* <span data-ttu-id="4b31f-140">枚举的类型，提供它，并在其中它嵌套 （如果有） 的类型具有`Public`可访问性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-140">An enumerated type, provided that it and the types in which it is nested (if any) have `Public` accessibility.</span></span>

* <span data-ttu-id="4b31f-141">此列表中以前的类型之一的一维数组。</span><span class="sxs-lookup"><span data-stu-id="4b31f-141">A one-dimensional array of one of the previous types in this list.</span></span>

## <a name="attribute-blocks"></a><span data-ttu-id="4b31f-142">特性块</span><span class="sxs-lookup"><span data-stu-id="4b31f-142">Attribute Blocks</span></span>

<span data-ttu-id="4b31f-143">以指定属性*特性块*。</span><span class="sxs-lookup"><span data-stu-id="4b31f-143">Attributes are specified in *attribute blocks*.</span></span> <span data-ttu-id="4b31f-144">通过尖括号 ("<>") 分隔每个特性块并在属性块中以逗号分隔的列表中或多个特性块中，可以指定多个属性。</span><span class="sxs-lookup"><span data-stu-id="4b31f-144">Each attribute block is delimited by angle brackets ("<>"), and multiple attributes can be specified in a comma-separated list within an attribute block or in multiple attribute blocks.</span></span> <span data-ttu-id="4b31f-145">指定属性的顺序并不重要。</span><span class="sxs-lookup"><span data-stu-id="4b31f-145">The order in which attributes are specified is not significant.</span></span> <span data-ttu-id="4b31f-146">例如，特性块`<A, B>`， `<B, A>`，`<A> <B>`和`<B> <A>`都是等效的。</span><span class="sxs-lookup"><span data-stu-id="4b31f-146">For example, the attribute blocks `<A, B>`, `<B, A>`, `<A> <B>` and `<B> <A>` are all equivalent.</span></span>

```antlr
Attributes
    : AttributeBlock+
    ;

AttributeBlock
    : LineTerminator? '<' AttributeList LineTerminator? '>' LineTerminator?
    ;

AttributeList
    : Attribute ( Comma Attribute )*
    ;

Attribute
    : ( AttributeModifier ':' )? SimpleTypeName
    ( OpenParenthesis AttributeArguments? CloseParenthesis )?
    ;

AttributeModifier
    : 'Assembly' | 'Module'
    ;
```

<span data-ttu-id="4b31f-147">属性不能指定类型的声明不是支持和一次性属性不能指定多个一次在特性块中。</span><span class="sxs-lookup"><span data-stu-id="4b31f-147">An attribute may not be specified on a kind of declaration it does not support, and single-use attributes may not be specified more than once in an attribute block.</span></span> <span data-ttu-id="4b31f-148">下面的示例会导致这两个错误，因为它尝试使用`HelpString`接口上`Interface1`和多次的声明上`Class1`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-148">The example below causes errors both because it attempts to use `HelpString` on the interface `Interface1` and more than once on the declaration of `Class1`.</span></span>

```vb
<AttributeUsage(AttributeTargets.Class)> _
Public Class HelpStringAttribute
    Inherits System.Attribute

    Private InternalValue As String

    Public Sub New(value As String)
        Me.InternalValue = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return InternalValue
        End Get
    End Property
End Class

' Error: HelpString only applies to classes.
<HelpString("Description of Interface1")> _
Interface Interface1
    Sub Sub1()
End Interface

' Error: HelpString is single-use.
<HelpString("Description of Class1"), _
    HelpString("Another description of Class1")> _
Public Class Class1
End Class
```

<span data-ttu-id="4b31f-149">属性包含了可选特性修饰符、 属性名称、 可选的位置参数和变量/属性初始值设定项列表。</span><span class="sxs-lookup"><span data-stu-id="4b31f-149">An attribute consists of an optional attribute modifier, an attribute name, an optional list of positional arguments, and variable/property initializers.</span></span> <span data-ttu-id="4b31f-150">如果没有任何参数或初始值设定项，则可省略括号。</span><span class="sxs-lookup"><span data-stu-id="4b31f-150">If there are no parameters or initializers, the parentheses may be omitted.</span></span> <span data-ttu-id="4b31f-151">如果某一特性有一个修饰符，则它必须在一个源文件顶部特性块中。</span><span class="sxs-lookup"><span data-stu-id="4b31f-151">If an attribute has a modifier, it must be in an attribute block at the top of a source file.</span></span>

<span data-ttu-id="4b31f-152">如果源代码文件包含特性块指定特性的程序集或模块将包含的源文件的文件的顶部，必须以特性块中的每个属性由前缀`Assembly`或`Module`修饰符和冒号。</span><span class="sxs-lookup"><span data-stu-id="4b31f-152">If a source file contains an attribute block at the top of the file that specifies attributes for the assembly or module that will contain the source file, each attribute in the attribute block must be prefixed by both the `Assembly` or `Module` modifier and a colon.</span></span>


### <a name="attribute-names"></a><span data-ttu-id="4b31f-153">属性名称</span><span class="sxs-lookup"><span data-stu-id="4b31f-153">Attribute Names</span></span>

<span data-ttu-id="4b31f-154">属性的名称指定的特性类。</span><span class="sxs-lookup"><span data-stu-id="4b31f-154">The name of an attribute specifies an attribute class.</span></span> <span data-ttu-id="4b31f-155">按照约定，特性类名为带有后缀`Attribute`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-155">By convention, attribute classes are named with the suffix `Attribute`.</span></span> <span data-ttu-id="4b31f-156">使用属性可以包括或省略此后缀。</span><span class="sxs-lookup"><span data-stu-id="4b31f-156">Uses of an attribute may either include or omit this suffix.</span></span> <span data-ttu-id="4b31f-157">因此，对应于属性标识符的特性类的名称是本身的标识符或限定标识符的串联和`Attribute`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-157">Consequently the name of an attribute class that corresponds to an attribute identifier is either the identifier itself or the concatenation of the qualified identifier and `Attribute`.</span></span> <span data-ttu-id="4b31f-158">当编译器解析属性名称时，它将追加`Attribute`的名称，然后尝试查找匹配。</span><span class="sxs-lookup"><span data-stu-id="4b31f-158">When the compiler resolves an attribute name, it appends `Attribute` to the name and tries the lookup.</span></span> <span data-ttu-id="4b31f-159">如果查找失败，编译器将尝试查找不带后缀。</span><span class="sxs-lookup"><span data-stu-id="4b31f-159">If that lookup fails, the compiler tries the lookup without the suffix.</span></span> <span data-ttu-id="4b31f-160">例如，特性类使用`SimpleAttribute`可能会省略`Attribute`后缀，从而缩短该名称与`Simple`:</span><span class="sxs-lookup"><span data-stu-id="4b31f-160">For example, uses of an attribute class `SimpleAttribute` may omit the `Attribute` suffix, thus shortening the name to `Simple`:</span></span>

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

<span data-ttu-id="4b31f-161">上面的示例在语义上等效于下面：</span><span class="sxs-lookup"><span data-stu-id="4b31f-161">The example above is semantically equivalent to the following:</span></span>

```vb
<SimpleAttribute> Class Class1
End Class

<SimpleAttribute> Interface Interface1
End Interface
```

<span data-ttu-id="4b31f-162">一般情况下，属性名后缀为`Attribute`首选分发点。</span><span class="sxs-lookup"><span data-stu-id="4b31f-162">In general, attributes named with the suffix `Attribute` are preferred.</span></span> <span data-ttu-id="4b31f-163">下面的示例演示两个属性类，名为`T`和`T``Attribute`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-163">The following example shows two attribute classes named `T` and `T``Attribute`.</span></span>

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class T
    Inherits System.Attribute
End Class

<AttributeUsage(AttributeTargets.All)> _
Public Class TAttribute
    Inherits System.Attribute
End Class

' Refers to TAttribute.
<T> Class Class1 
End Class

' Refers to TAttribute.
<TAttribute> Class Class2 
End Class
```

<span data-ttu-id="4b31f-164">特性块`<T>`和特性块`<TAttribute>`到名为的属性类，请参阅`TAttribute`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-164">Both the attribute block `<T>` and the attribute block `<TAttribute>` refer to the attribute class named `TAttribute`.</span></span> <span data-ttu-id="4b31f-165">不能使用`T`作为属性之前删除类的声明`TAttribute`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-165">It is not possible to use `T` as an attribute until you remove the declaration for class `TAttribute`.</span></span>

### <a name="attribute-arguments"></a><span data-ttu-id="4b31f-166">特性参数</span><span class="sxs-lookup"><span data-stu-id="4b31f-166">Attribute Arguments</span></span>

<span data-ttu-id="4b31f-167">属性的自变量可能采用两种形式：*位置自变量*并*实例变量/属性初始值设定项*。</span><span class="sxs-lookup"><span data-stu-id="4b31f-167">Arguments to an attribute may take two forms: *positional arguments* and *instance variable/property initializers*.</span></span> <span data-ttu-id="4b31f-168">任何位置自变量的属性必须在前面的实例变量/属性初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="4b31f-168">Any positional arguments to the attribute must precede the instance variable/property initializers.</span></span> <span data-ttu-id="4b31f-169">位置自变量包含的常量表达式，一维数组创建表达式或`GetType`表达式。</span><span class="sxs-lookup"><span data-stu-id="4b31f-169">A positional argument consists of a constant expression, a one-dimensional array-creation expression or a `GetType` expression.</span></span> <span data-ttu-id="4b31f-170">实例变量/属性初始值设定项包含的标识符，可以匹配关键字后, 跟冒号和等号，由常数表达式终止或`GetType`表达式。</span><span class="sxs-lookup"><span data-stu-id="4b31f-170">An instance variable/property initializer consists of an identifier, which can match keywords, followed by a colon and equal sign, and terminated by a constant expression or a `GetType` expression.</span></span>

<span data-ttu-id="4b31f-171">已知属性的特性类`T`，位置自变量列表`P`，并实例变量/属性初始值设定项列表`N`，以下步骤确定参数是否有效：</span><span class="sxs-lookup"><span data-stu-id="4b31f-171">Given an attribute with attribute class `T`, positional argument list `P`, and instance variable/property initializer list `N`, these steps determine whether the arguments are valid:</span></span>

1. <span data-ttu-id="4b31f-172">请按照用于编译形式的表达式的编译时处理步骤`New T(P)`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-172">Follow the compile-time processing steps for compiling an expression of the form `New T(P)`.</span></span> <span data-ttu-id="4b31f-173">这会导致编译时错误或确定一个构造函数上`T`这就是最适用于自变量列表。</span><span class="sxs-lookup"><span data-stu-id="4b31f-173">This either results in a compile-time error or determines a constructor on `T` that is most applicable to the argument list.</span></span>

2. <span data-ttu-id="4b31f-174">如果在步骤 1 中确定的构造函数不是属性类型的参数，或无法访问位于声明站点，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="4b31f-174">If the constructor determined in step 1 has parameters that are not attribute types or is inaccessible at the declaration site, a compile-time error occurs.</span></span>

3. <span data-ttu-id="4b31f-175">对于每个实例变量/属性初始值设定项`Arg`中`N`，可让`Name`是实例变量/属性初始值设定项的标识符`Arg`。</span><span class="sxs-lookup"><span data-stu-id="4b31f-175">For each instance variable/property initializer `Arg` in `N`, let `Name` be the identifier of the instance variable/property initializer `Arg`.</span></span> <span data-ttu-id="4b31f-176">`Name` 必须标识一个非`Shared`、 可写`Public`上的实例变量或无参数属性`T`其类型为属性类型。</span><span class="sxs-lookup"><span data-stu-id="4b31f-176">`Name` must identify a non-`Shared`, writeable, `Public` instance variable or parameterless property on `T` whose type is an attribute type.</span></span> <span data-ttu-id="4b31f-177">如果`T`具有无此类的实例变量或属性，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="4b31f-177">If `T` has no such instance variable or property, a compile-time error occurs.</span></span>

<span data-ttu-id="4b31f-178">例如：</span><span class="sxs-lookup"><span data-stu-id="4b31f-178">For example:</span></span>

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class GeneralAttribute
    Inherits Attribute

    Public Sub New(x As Integer)
    End Sub

    Public Sub New(x As Double)
    End Sub

    Public y As Type

    Public Property z As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

' Calls the first constructor.
<General(10, z:=30, y:=GetType(Integer))> _
Class C1
End Class

' Calls the second constructor.
<General(10.5, z:=10)> _
Class C2
End Class
```

<span data-ttu-id="4b31f-179">特性实参中不能任意位置使用类型参数。</span><span class="sxs-lookup"><span data-stu-id="4b31f-179">Type parameters cannot be used anywhere in attribute arguments.</span></span> <span data-ttu-id="4b31f-180">但是，可以使用构造的类型：</span><span class="sxs-lookup"><span data-stu-id="4b31f-180">However, constructed types may be used:</span></span>

```vb
<AttributeUsage(AttributeTargets.All)> _
Class A 
   Inherits System.Attribute 

   Public Sub New(t As Type)
   End Sub 
End Class

Class List(Of T) 
    ' Error: attribute argument cannot use type parameter
    <A(GetType(T))> Dim t1 As T 

    ' OK: closed type
    <A(GetType(List(Of Integer)))> Dim y As Integer
End Class
```


```antlr
AttributeArguments
    : AttributePositionalArgumentList
    | AttributePositionalArgumentList Comma VariablePropertyInitializerList
    | VariablePropertyInitializerList
    ;

AttributePositionalArgumentList
    : AttributeArgumentExpression? ( Comma AttributeArgumentExpression? )*
    ;

VariablePropertyInitializerList
    : VariablePropertyInitializer ( Comma VariablePropertyInitializer )*
    ;

VariablePropertyInitializer
    : IdentifierOrKeyword ColonEquals AttributeArgumentExpression
    ;

AttributeArgumentExpression
    : ConstantExpression
    | GetTypeExpression
    | ArrayExpression
    ;
```