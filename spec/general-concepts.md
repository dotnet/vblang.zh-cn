---
ms.openlocfilehash: be4090cc1ee294342645029e327d33627e8473fe
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "49461351"
---
# <a name="general-concepts"></a><span data-ttu-id="d57fe-101">一般概念</span><span class="sxs-lookup"><span data-stu-id="d57fe-101">General Concepts</span></span>

<span data-ttu-id="d57fe-102">本章我们将介绍数都必需能识别的 Microsoft Visual Basic 语言的语义的概念。</span><span class="sxs-lookup"><span data-stu-id="d57fe-102">This chapter covers a number of concepts that are required to understand the semantics of the Microsoft Visual Basic language.</span></span> <span data-ttu-id="d57fe-103">许多概念应该熟悉的 Visual Basic 编程人员或 C/c + + 程序员提供的但其精确定义可能会不同。</span><span class="sxs-lookup"><span data-stu-id="d57fe-103">Many of the concepts should be familiar to Visual Basic programmers or C/C++ programmers, but their precise definitions may differ.</span></span>

## <a name="declarations"></a><span data-ttu-id="d57fe-104">声明</span><span class="sxs-lookup"><span data-stu-id="d57fe-104">Declarations</span></span>

<span data-ttu-id="d57fe-105">Visual Basic 程序由多个命名实体组成。</span><span class="sxs-lookup"><span data-stu-id="d57fe-105">A Visual Basic program is made up of named entities.</span></span> <span data-ttu-id="d57fe-106">这些实体通过引入*声明*和表示该程序的"含义"。</span><span class="sxs-lookup"><span data-stu-id="d57fe-106">These entities are introduced through *declarations* and represent the "meaning" of the program.</span></span>

<span data-ttu-id="d57fe-107">在高级别中，请*命名空间*是组织其他实体，如嵌套的命名空间和类型的实体。</span><span class="sxs-lookup"><span data-stu-id="d57fe-107">At a top level, *namespaces* are entities that organize other entities, such as nested namespaces and types.</span></span> <span data-ttu-id="d57fe-108">*类型*是描述值并定义可执行代码的实体。</span><span class="sxs-lookup"><span data-stu-id="d57fe-108">*Types* are entities that describe values and define executable code.</span></span> <span data-ttu-id="d57fe-109">类型可能包含嵌套的类型和类型成员。</span><span class="sxs-lookup"><span data-stu-id="d57fe-109">Types may contain nested types and type members.</span></span> <span data-ttu-id="d57fe-110">*类型成员*是常量、 变量、 方法、 运算符、 属性、 事件、 枚举值和构造函数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-110">*Type members* are constants, variables, methods, operators, properties, events, enumeration values, and constructors.</span></span>

<span data-ttu-id="d57fe-111">可以包含其他实体的实体定义*声明空间*。</span><span class="sxs-lookup"><span data-stu-id="d57fe-111">An entity that can contain other entities defines a *declaration space*.</span></span> <span data-ttu-id="d57fe-112">实体被引入声明空间通过声明或继承;包含声明空间称为实体的*声明上下文*。</span><span class="sxs-lookup"><span data-stu-id="d57fe-112">Entities are introduced into a declaration space either through declarations or inheritance; the containing declaration space is called the entities' *declaration context*.</span></span> <span data-ttu-id="d57fe-113">反过来声明声明空间中的实体定义新的声明空间可以包含更多嵌套的 entity 声明;因此，在程序中的声明组成声明空间的层次的结构。</span><span class="sxs-lookup"><span data-stu-id="d57fe-113">Declaring an entity in a declaration space in turn defines a new declaration space that can contain further nested entity declarations; thus, the declarations in a program form a hierarchy of declaration spaces.</span></span>

<span data-ttu-id="d57fe-114">除在重载的类型成员的情况下是无效的声明，可将相同类型的名称相同的实体到同一个声明上下文。</span><span class="sxs-lookup"><span data-stu-id="d57fe-114">Except in the case of overloaded type members, it is invalid for declarations to introduce identically named entities of the same kind into the same declaration context.</span></span> <span data-ttu-id="d57fe-115">此外，声明空间可能永远不会包含不同类型的实体具有相同的名称;例如，声明空间可能永远不会包含一个变量和方法按相同的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-115">Additionally, a declaration space may never contain different kinds of entities with the same name; for example, a declaration space may never contain a variable and a method by the same name.</span></span>

<span data-ttu-id="d57fe-116">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="d57fe-116">__Note.__</span></span> <span data-ttu-id="d57fe-117">它可能在其他语言来创建一个包含不同类型的具有相同名称的实体 （例如，如果语言区分大小写，并且允许基于大小写不同的声明） 的声明空间中。</span><span class="sxs-lookup"><span data-stu-id="d57fe-117">It may be possible in other languages to create a declaration space that contains different kinds of entities with the same name (for example, if the language is case sensitive and allows different declarations based on casing).</span></span> <span data-ttu-id="d57fe-118">在这种情况下，多人可访问的实体将被视为绑定到该名称时;如果多个实体的类型是大多数可访问名称不明确。</span><span class="sxs-lookup"><span data-stu-id="d57fe-118">In that situation, the most accessible entity is considered bound to that name; if more than one type of entity is most accessible then the name is ambiguous.</span></span> <span data-ttu-id="d57fe-119">`Public` 更可访问性比`Protected Friend`，`Protected Friend`更可访问性比`Protected`或`Friend`，并`Protected`或`Friend`更可访问性比`Private`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-119">`Public` is more accessible than `Protected Friend`, `Protected Friend` is more accessible than `Protected` or `Friend`, and `Protected` or `Friend` is more accessible than `Private`.</span></span>

<span data-ttu-id="d57fe-120">命名空间声明空间是"已结束，打开"具有相同的完全限定名称的两个命名空间声明分配到同一声明空间。</span><span class="sxs-lookup"><span data-stu-id="d57fe-120">The declaration space of a namespace is "open ended," so two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="d57fe-121">在下面的示例中，两个命名空间声明参与到同一声明空间，在这种情况下声明两个类的完全限定名称`Data.Customer`和 `Data.Order:`</span><span class="sxs-lookup"><span data-stu-id="d57fe-121">In the example below, the two namespace declarations contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Data.Customer` and `Data.Order:`</span></span>

```vb
Namespace Data
    Class Customer
    End Class
End Namespace

Namespace Data
    Class Order
    End Class
End Namespace
```

<span data-ttu-id="d57fe-122">由于向同一声明空间分配的两个声明，如果每个包含具有相同名称的类的声明会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-122">Because the two declarations contribute to the same declaration space, a compile-time error would occur if each contained a declaration of a class with the same name.</span></span>

### <a name="overloading-and-signatures"></a><span data-ttu-id="d57fe-123">重载和签名</span><span class="sxs-lookup"><span data-stu-id="d57fe-123">Overloading and Signatures</span></span>

<span data-ttu-id="d57fe-124">若要声明空间中声明具有相同名称的实体的相同类型的唯一方法是通过*重载*。</span><span class="sxs-lookup"><span data-stu-id="d57fe-124">The only way to declare identically named entities of the same kind in a declaration space is through *overloading*.</span></span> <span data-ttu-id="d57fe-125">可以重载方法、 运算符、 实例构造函数和属性。</span><span class="sxs-lookup"><span data-stu-id="d57fe-125">Only methods, operators, instance constructors, and properties may be overloaded.</span></span>

<span data-ttu-id="d57fe-126">重载的类型成员必须具有唯一的签名。</span><span class="sxs-lookup"><span data-stu-id="d57fe-126">Overloaded type members must possess unique signatures.</span></span> <span data-ttu-id="d57fe-127">类型成员的签名包含类型参数和数字的数量和类型成员的参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-127">The signature of a type member consists of the number of type parameters, and the number and types of the member's parameters.</span></span> <span data-ttu-id="d57fe-128">转换运算符还包括在签名中的运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-128">Conversion operators also include the return type of the operator in the signature.</span></span>

<span data-ttu-id="d57fe-129">以下不是成员的签名的一部分，因此不能重载上：</span><span class="sxs-lookup"><span data-stu-id="d57fe-129">The following are not part of a member's signature, and hence cannot be overloaded on:</span></span>

* <span data-ttu-id="d57fe-130">对类型成员的修饰符 (例如，`Shared`或`Private`)。</span><span class="sxs-lookup"><span data-stu-id="d57fe-130">Modifiers to a type member (for example, `Shared` or `Private`).</span></span>

* <span data-ttu-id="d57fe-131">参数修饰符 (例如，`ByVal`或`ByRef`)。</span><span class="sxs-lookup"><span data-stu-id="d57fe-131">Modifiers to a parameter (for example, `ByVal` or `ByRef`).</span></span>

* <span data-ttu-id="d57fe-132">参数的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-132">The names of the parameters.</span></span>

* <span data-ttu-id="d57fe-133">一种方法或运算符 （除转换运算符） 或属性的元素类型的返回类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-133">The return type of a method or operator (except for conversion operators) or the element type of a property.</span></span>

* <span data-ttu-id="d57fe-134">在类型参数的约束。</span><span class="sxs-lookup"><span data-stu-id="d57fe-134">Constraints on a type parameter.</span></span>

<span data-ttu-id="d57fe-135">下面的示例显示了一组重载的方法声明，以及它们的签名。</span><span class="sxs-lookup"><span data-stu-id="d57fe-135">The following example shows a set of overloaded method declarations along with their signatures.</span></span> <span data-ttu-id="d57fe-136">此声明不会有效，因为多个方法声明具有相同的签名。</span><span class="sxs-lookup"><span data-stu-id="d57fe-136">This declaration would not be valid since several of the method declarations have identical signatures.</span></span>

```vb
Interface ITest
    Sub F1()                              ' Signature is ().
    Sub F2(x As Integer)                  ' Signature is (Integer).
    Sub F3(ByRef x As Integer)            ' Signature is (Integer).
    Sub F4(x As Integer, y As Integer)    ' Signature is (Integer, Integer).
    Function F5(s As String) As Integer   ' Signature is (String).
    Function F6(x As Integer) As Integer  ' Signature is (Integer).
    Sub F7(a() As String)                 ' Signature is (String()).
    Sub F8(ParamArray a() As String)      ' Signature is (String()).
    Sub F9(Of T)()                        ' Signature is !1().
    Sub F10(Of T, U)(x As T, y As U)      ' Signature is !2(!1, !2)
    Sub F11(Of U, T)(x As T, y As U)      ' Signature is !2(!2, !1)
    Sub F12(Of T)(x As T)                 ' Signature is !1(!1)
    Sub F13(Of T As IDisposable)(x As T)  ' Signature is !1(!1)
End Interface
```

<span data-ttu-id="d57fe-137">若要定义的泛型类型，可能会使用基于提供的类型参数完全相同的签名包含成员无效。</span><span class="sxs-lookup"><span data-stu-id="d57fe-137">It is valid to define a generic type that may contain members with identical signatures based on the type arguments supplied.</span></span> <span data-ttu-id="d57fe-138">重载决策规则用于尝试，此类重载之间产生歧义尽管可能存在的情况下在其中无法消除歧义。</span><span class="sxs-lookup"><span data-stu-id="d57fe-138">Overload resolution rules are used to try and disambiguate between such overloads, although there may be situations in which it is impossible to disambiguate.</span></span> <span data-ttu-id="d57fe-139">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-139">For example:</span></span>

```vb
Class C(Of T)
    Sub F(x As Integer)
    End Sub

    Sub F(x As T)
    End Sub

    Sub G(Of U)(x As T, y As U)
    End Sub

    Sub G(Of U)(x As U, y As T)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New C(Of Integer)
        x.F(10)                   ' Calls C(Of T).F(Integer)
        x.G(Of Integer)(10, 10)    ' Error: Can't choose between overloads
    End Sub
End Module
```

## <a name="scope"></a><span data-ttu-id="d57fe-140">范围</span><span class="sxs-lookup"><span data-stu-id="d57fe-140">Scope</span></span>

<span data-ttu-id="d57fe-141">*作用域*的实体的名称是一的组的所有声明空间，在其中就可以引用而无需限定该名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-141">The *scope* of an entity's name is the set of all declaration spaces within which it is possible to refer to that name without qualification.</span></span> <span data-ttu-id="d57fe-142">一般情况下，实体的作用域是名称的整个声明其上下文中;但是，实体的声明可以包含嵌套的声明具有相同名称的实体。</span><span class="sxs-lookup"><span data-stu-id="d57fe-142">In general, the scope of an entity's name is its entire declaration context; however, an entity's declaration may contain nested declarations of entities with the same name.</span></span> <span data-ttu-id="d57fe-143">在这种情况下，嵌套的 entity *shadows*，或隐藏、 外部实体，以及对隐藏实体访问仅通过限定。</span><span class="sxs-lookup"><span data-stu-id="d57fe-143">In that case, the nested entity *shadows*, or hides, the outer entity, and access to the shadowed entity is only possible through qualification.</span></span>

<span data-ttu-id="d57fe-144">通过嵌套隐藏出现在命名空间或类型嵌套在命名空间、 嵌套在其他类型的类型和类型成员的正文。</span><span class="sxs-lookup"><span data-stu-id="d57fe-144">Shadowing through nesting occurs in namespaces or types nested within namespaces, in types nested within other types, and in the bodies of type members.</span></span> <span data-ttu-id="d57fe-145">通过嵌套声明始终隐藏隐式; 发生不不需要任何显式语法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-145">Shadowing through the nesting of declarations always occurs implicitly; no explicit syntax is required.</span></span>

<span data-ttu-id="d57fe-146">在以下示例中内,`F`方法中，实例变量`i`隐藏由本地变量`i`，但内`G`方法，`i`仍是指实例变量。</span><span class="sxs-lookup"><span data-stu-id="d57fe-146">In the following example, within the `F` method, the instance variable `i` is shadowed by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

```vb
Class Test
    Private i As Integer = 0

    Sub F()
        Dim i As Integer = 1
    End Sub

    Sub G()
        i = 1
    End Sub
End Class
```

<span data-ttu-id="d57fe-147">如果内层作用域中的名称将隐藏外层作用域中的名称，它会隐藏该名称的所有重载的匹配项。</span><span class="sxs-lookup"><span data-stu-id="d57fe-147">When a name in an inner scope hides a name in an outer scope, it shadows all overloaded occurrences of that name.</span></span> <span data-ttu-id="d57fe-148">在下面的示例中，在调用`F(1)`调用`F`中声明`Inner`因为外部出现的所有`F`隐藏内部声明。</span><span class="sxs-lookup"><span data-stu-id="d57fe-148">In the following example, the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="d57fe-149">出于相同原因，在调用`F("Hello")`出现错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-149">For the same reason, the call `F("Hello")` is in error.</span></span>

```vb
Class Outer
    Shared Sub F(i As Integer)
    End Sub

    Shared Sub F(s As String)
    End Sub

    Class Inner
        Shared Sub F(l As Long)
        End Sub

        Sub G()
            F(1) ' Invokes Outer.Inner.F.
            F("Hello") ' Error.
        End Sub
    End Class
End Class
```

## <a name="inheritance"></a><span data-ttu-id="d57fe-150">继承</span><span class="sxs-lookup"><span data-stu-id="d57fe-150">Inheritance</span></span>

<span data-ttu-id="d57fe-151">继承关系是在哪一种类型之一 (*派生*类型) 派生自另一个 (*基*类型)，以便派生的类型的声明空间隐式包含可访问非构造函数类型成员和嵌套的类型及其基类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-151">An inheritance relationship is one in which one type (the *derived* type) derives from another (the *base* type), such that the derived type's declaration space implicitly contains the accessible non-constructor type members and nested types of its base type.</span></span> <span data-ttu-id="d57fe-152">在以下示例中，类`A`是类的基类`B`，并`B`派生自`A`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-152">In the following example, class `A` is the base class of `B`, and `B` is derived from `A`.</span></span>

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

<span data-ttu-id="d57fe-153">由于`A`does 未显式指定的基类，其基本类是隐式`Object`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-153">Since `A` does not explicitly specify a base class, its base class is implicitly `Object`.</span></span>

<span data-ttu-id="d57fe-154">以下是继承的重要方面：</span><span class="sxs-lookup"><span data-stu-id="d57fe-154">The following are important aspects of inheritance:</span></span>

* <span data-ttu-id="d57fe-155">继承是可传递的。</span><span class="sxs-lookup"><span data-stu-id="d57fe-155">Inheritance is transitive.</span></span> <span data-ttu-id="d57fe-156">如果类型*C*派生自类型*B*，然后键入*B*派生自类型*A*，类型*C*继承键入类型中声明成员*B*以及类型中声明的类型成员*A*。</span><span class="sxs-lookup"><span data-stu-id="d57fe-156">If type *C* is derived from type *B*, and type *B* is derived from type *A*, type *C* inherits the type members declared in type *B* as well as the type members declared in type *A*.</span></span>

* <span data-ttu-id="d57fe-157">派生的类型扩展了，但不能缩小，其基类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-157">A derived type extends, but cannot narrow, its base type.</span></span> <span data-ttu-id="d57fe-158">派生的类型可以添加新的类型成员，它都可隐藏继承的类型成员，但它不能删除继承的类型成员的定义。</span><span class="sxs-lookup"><span data-stu-id="d57fe-158">A derived type can add new type members, and it can shadow inherited type members, but it cannot remove the definition of an inherited type member.</span></span>

* <span data-ttu-id="d57fe-159">由于类型的实例包含的所有其基类型的类型成员，因此转换始终存在从派生类型与其基类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-159">Because an instance of a type contains all of the type members of its base type, a conversion always exists from a derived type to its base type.</span></span>

* <span data-ttu-id="d57fe-160">所有类型必须都具有基类型，类型除外`Object`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-160">All types must have a base type, except for the type `Object`.</span></span> <span data-ttu-id="d57fe-161">因此，`Object`是所有类型的 ultimate 基类型和所有类型可以都转换为它。</span><span class="sxs-lookup"><span data-stu-id="d57fe-161">Thus, `Object` is the ultimate base type of all types, and all types can be converted to it.</span></span>

* <span data-ttu-id="d57fe-162">不允许派生中的循环。</span><span class="sxs-lookup"><span data-stu-id="d57fe-162">Circularity in derivation is not permitted.</span></span> <span data-ttu-id="d57fe-163">它是类型`B`派生自类型`A`，它是错误的类型`A`以直接或间接派生自类型`B`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-163">That is, when a type `B` derives from a type `A`, it is an error for type `A` to derive directly or indirectly from type `B`.</span></span>

* <span data-ttu-id="d57fe-164">一种类型可能不直接或间接派生自嵌套在它的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-164">A type may not directly or indirectly derive from a type nested within it.</span></span>

<span data-ttu-id="d57fe-165">以下示例会生成编译时错误，因为类循环彼此依赖。</span><span class="sxs-lookup"><span data-stu-id="d57fe-165">The following example produces a compile-time error because the classes circularly depend on each other.</span></span>

```vb
Class A
    Inherits B
End Class

Class B
    Inherits C
End Class

Class C
    Inherits A
End Class
```

<span data-ttu-id="d57fe-166">下面的示例也生成编译时错误，因为`B`间接派生自其嵌套类`C`通过类`A`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-166">The following example also produces a compile-time error because `B` indirectly derives from its nested class `C` through class `A`.</span></span>

```vb
Class A
    Inherits B.C
End Class

Class B
    Inherits A

    Public Class C
    End Class 
End Class
```

<span data-ttu-id="d57fe-167">下一个示例不会生成错误，因为类`A`不是派生自类`B`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-167">The next example does not produce an error because class `A` does not derive from class `B`.</span></span>

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a><span data-ttu-id="d57fe-168">MustInherit 和 NotInheritable 类</span><span class="sxs-lookup"><span data-stu-id="d57fe-168">MustInherit and NotInheritable Classes</span></span>

<span data-ttu-id="d57fe-169">一个`MustInherit`类是可以只作为基类型的不完整类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-169">A `MustInherit` class is an incomplete type that can act only as a base type.</span></span> <span data-ttu-id="d57fe-170">一个`MustInherit`类不能进行实例化，因此它是使用错误`New`运算符之一。</span><span class="sxs-lookup"><span data-stu-id="d57fe-170">A `MustInherit` class cannot be instantiated, so it is an error to use the `New` operator on one.</span></span> <span data-ttu-id="d57fe-171">可以声明的变量`MustInherit`类; 仅可以将此类变量分配`Nothing`是派生自的类的一个值或`MustInherit`类。</span><span class="sxs-lookup"><span data-stu-id="d57fe-171">It is valid to declare variables of `MustInherit` classes; such variables can only be assigned `Nothing` or a value that is of a class derived from the `MustInherit` class.</span></span>

<span data-ttu-id="d57fe-172">当常规类派生自`MustInherit`类，正则类必须重写所有继承`MustOverride`成员。</span><span class="sxs-lookup"><span data-stu-id="d57fe-172">When a regular class is derived from a `MustInherit` class, the regular class must override all inherited `MustOverride` members.</span></span> <span data-ttu-id="d57fe-173">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-173">For example:</span></span>

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

MustInherit Class B
    Inherits A

    Public Sub G()
    End Sub
End Class 

Class C
    Inherits B

    Public Overrides Sub F()
    End Sub 
End Class
```

<span data-ttu-id="d57fe-174">`MustInherit`类`A`引入`MustOverride`方法`F`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-174">The `MustInherit` class `A` introduces a `MustOverride` method `F`.</span></span> <span data-ttu-id="d57fe-175">类`B`引入了其他方法`G`，但不提供的实现`F`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-175">Class `B` introduces an additional method `G`, but does not provide an implementation of `F`.</span></span> <span data-ttu-id="d57fe-176">类`B`也因此必须声明`MustInherit`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-176">Class `B` must therefore also be declared `MustInherit`.</span></span> <span data-ttu-id="d57fe-177">类`C`重写`F`并提供实际实现。</span><span class="sxs-lookup"><span data-stu-id="d57fe-177">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="d57fe-178">由于有没有未完成`MustOverride`类中的成员`C`，不需要为`MustInherit`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-178">Since there are no outstanding `MustOverride` members in class `C`, it is not required to be `MustInherit`.</span></span>

<span data-ttu-id="d57fe-179">一个`NotInheritable`类是不能与另一个类派生一个类。</span><span class="sxs-lookup"><span data-stu-id="d57fe-179">A `NotInheritable` class is a class from which another class cannot be derived.</span></span> <span data-ttu-id="d57fe-180">`NotInheritable` 类主要用于防止无意的派生。</span><span class="sxs-lookup"><span data-stu-id="d57fe-180">`NotInheritable` classes are primarily used to prevent unintended derivation.</span></span>

<span data-ttu-id="d57fe-181">在此示例中，类`B`是错误，因为它会尝试派生自`NotInheritable`类`A`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-181">In this example, class `B` is in error because it attempts to derive from the `NotInheritable` class `A`.</span></span> <span data-ttu-id="d57fe-182">类不能将标记都`MustInherit`和`NotInheritable`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-182">A class cannot be marked both `MustInherit` and `NotInheritable`.</span></span>

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a><span data-ttu-id="d57fe-183">接口和多重继承</span><span class="sxs-lookup"><span data-stu-id="d57fe-183">Interfaces and Multiple Inheritance</span></span>

<span data-ttu-id="d57fe-184">与其他类型，只能从单个基类型派生，不同接口可能派生自多个基接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-184">Unlike other types, which only derive from a single base type, an interface may derive from multiple base interfaces.</span></span> <span data-ttu-id="d57fe-185">因此，接口可从不同的基接口继承具有相同名称的类型成员。</span><span class="sxs-lookup"><span data-stu-id="d57fe-185">Because of this, an interface can inherit an identically named type member from different base interfaces.</span></span> <span data-ttu-id="d57fe-186">在这种情况下，乘继承名称可用在派生接口中，并不指派生接口通过这些类型任何的成员会导致编译时错误，而不考虑签名或重载。</span><span class="sxs-lookup"><span data-stu-id="d57fe-186">In such a case, the multiply-inherited name is not available in the derived interface, and referring to any of those type members through the derived interface causes a compile-time error, regardless of signatures or overloading.</span></span> <span data-ttu-id="d57fe-187">相反，必须通过基接口名称引用冲突的类型成员。</span><span class="sxs-lookup"><span data-stu-id="d57fe-187">Instead, conflicting type members must be referenced through a base interface name.</span></span>

<span data-ttu-id="d57fe-188">在以下示例中前, 两个语句导致编译时错误，因为乘法继承成员`Count`界面中不可用`IListCounter`:</span><span class="sxs-lookup"><span data-stu-id="d57fe-188">In the following example, the first two statements cause compile-time errors because the multiply-inherited member `Count` is not available in interface `IListCounter`:</span></span>

```vb
Interface IList
    Property Count() As Integer
End Interface

Interface ICounter
    Sub Count(i As Integer)
End Interface

Interface IListCounter
    Inherits IList
    Inherits ICounter 
End Interface 

Module Test
    Sub F(x As IListCounter)
        x.Count(1)                  ' Error, Count is not available.
        x.Count = 1                 ' Error, Count is not available.
        CType(x, IList).Count = 1   ' Ok, invokes IList.Count.
        CType(x, ICounter).Count(1) ' Ok, invokes ICounter.Count.
    End Sub 
End Module
```

<span data-ttu-id="d57fe-189">如示例所示，通过强制转换来解决二义性`x`为适当的基接口类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-189">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="d57fe-190">这种转换具有不运行时成本;它们只是包含在编译时查看作为无派生类型的实例。</span><span class="sxs-lookup"><span data-stu-id="d57fe-190">Such casts have no run-time costs; they merely consist of viewing the instance as a less-derived type at compile time.</span></span>

<span data-ttu-id="d57fe-191">当一个类型成员继承自相同的基接口通过多个路径时，类型成员被视为如同它仅继承一次。</span><span class="sxs-lookup"><span data-stu-id="d57fe-191">When a single type member is inherited from the same base interface through multiple paths, the type member is treated as if it were only inherited once.</span></span> <span data-ttu-id="d57fe-192">换而言之，派生的接口仅包含从特定的基接口继承的每个类型成员的一个实例。</span><span class="sxs-lookup"><span data-stu-id="d57fe-192">In other words, the derived interface only contains one instance of each type member inherited from a particular base interface.</span></span> <span data-ttu-id="d57fe-193">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-193">For example:</span></span>

```vb
Interface IBase
    Sub F(i As Integer)
End Interface

Interface ILeft
    Inherits IBase
End Interface

Interface IRight
    Inherits IBase
End Interface

Interface IDerived
    Inherits ILeft, IRight
End Interface

Class Derived
    Implements IDerived

    ' Only have to implement F once.
    Sub F(i As Integer) Implements IDerived.F
    End Sub
End Class
```

<span data-ttu-id="d57fe-194">如果一条路径的继承层次结构中隐藏的类型成员名称，名称被阴影中的所有路径。</span><span class="sxs-lookup"><span data-stu-id="d57fe-194">If a type member name is shadowed in one path through the inheritance hierarchy, then the name is shadowed in all paths.</span></span> <span data-ttu-id="d57fe-195">在以下示例中，`IBase.F`隐藏成员`ILeft.F`成员，但不是隐藏在`IRight`:</span><span class="sxs-lookup"><span data-stu-id="d57fe-195">In the following example, the `IBase.F` member is shadowed by the `ILeft.F` member, but is not shadowed in `IRight`:</span></span>

```vb
Interface IBase
    Sub F(i As Integer)
End Interface 

Interface ILeft
    Inherits IBase

    Shadows Sub F(i As Integer)
End Interface 

Interface IRight
    Inherits IBase

    Sub G()
End Interface 

Interface IDerived
    Inherits ILeft, IRight 
End Interface 

Class Test
    Sub H(d As IDerived)
        d.F(1)                  ' Invokes ILeft.F.
        CType(d, IBase).F(1)    ' Invokes IBase.F.
        CType(d, ILeft).F(1)    ' Invokes ILeft.F.
        CType(d, IRight).F(1)   ' Invokes IBase.F.
    End Sub 
End Class
```

<span data-ttu-id="d57fe-196">在调用`d.F(1)`中选择`ILeft.F`，即使`IBase.F`似乎不通过将导致在访问路径中阴影`IRight`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-196">The invocation `d.F(1)` selects `ILeft.F`, even though `IBase.F` appears to not be shadowed in the access path that leads through `IRight`.</span></span> <span data-ttu-id="d57fe-197">因为从的访问路径`IDerived`到`ILeft`到`IBase`阴影`IBase.F`，在访问路径中还隐藏该成员`IDerived`到`IRight`到`IBase`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-197">Because the access path from `IDerived` to `ILeft` to `IBase` shadows `IBase.F`, the member is also shadowed in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

### <a name="shadowing"></a><span data-ttu-id="d57fe-198">隐藏</span><span class="sxs-lookup"><span data-stu-id="d57fe-198">Shadowing</span></span>

<span data-ttu-id="d57fe-199">派生的类型的重新声明隐藏继承的类型成员的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-199">A derived type shadows the name of an inherited type member by re-declaring it.</span></span> <span data-ttu-id="d57fe-200">隐藏名称不会删除具有该名称; 继承的类型成员它只是使所有具有该名称的继承的类型成员在派生类中不可用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-200">Shadowing a name does not remove the inherited type members with that name; it merely makes all of the inherited type members with that name unavailable in the derived class.</span></span> <span data-ttu-id="d57fe-201">隐藏的声明可以是实体的任何类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-201">The shadowing declaration may be any type of entity.</span></span>

<span data-ttu-id="d57fe-202">实体不是可以进行重载可以选择隐藏的两种形式之一。</span><span class="sxs-lookup"><span data-stu-id="d57fe-202">Entities than can be overloaded can choose one of two forms of shadowing.</span></span> <span data-ttu-id="d57fe-203">*按名称隐藏*使用指定的`Shadows`关键字。</span><span class="sxs-lookup"><span data-stu-id="d57fe-203">*Shadowing by name* is specified using the `Shadows` keyword.</span></span> <span data-ttu-id="d57fe-204">按名称隐藏的实体将隐藏所有内容中的基类，包括所有重载该名称的。</span><span class="sxs-lookup"><span data-stu-id="d57fe-204">An entity that shadows by name hides everything by that name in the base class, including all overloads.</span></span> <span data-ttu-id="d57fe-205">*按名称和签名隐藏*使用指定的`Overloads`关键字。</span><span class="sxs-lookup"><span data-stu-id="d57fe-205">*Shadowing by name and signature* is specified using the `Overloads` keyword.</span></span> <span data-ttu-id="d57fe-206">遮盖了具有的名称和签名的实体通过与实体相同的签名与该名称隐藏的所有内容。</span><span class="sxs-lookup"><span data-stu-id="d57fe-206">An entity that shadows by name and signature hides everything by that name with the same signature as the entity.</span></span> <span data-ttu-id="d57fe-207">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-207">For example:</span></span>

```vb
Class Base
    Sub F()
    End Sub

    Sub F(i As Integer)
    End Sub

    Sub G()
    End Sub

    Sub G(i As Integer)
    End Sub
End Class

Class Derived
    Inherits Base

    ' Only hides F(Integer).
    Overloads Sub F(i As Integer)
    End Sub

    ' Hides G() and G(Integer).
    Shadows Sub G(i As Integer)
    End Sub
End Class

Module Test
    Sub Main()
        Dim x As New Derived()

        x.F() ' Calls Base.F().
        x.G() ' Error: Missing parameter.
    End Sub
End Module
```

<span data-ttu-id="d57fe-208">隐藏方法替换`ParamArray`参数按名称和签名隐藏单个签名、 不是所有可能的扩展的签名。</span><span class="sxs-lookup"><span data-stu-id="d57fe-208">Shadowing a method with a `ParamArray` argument by name and signature hides only the individual signature, not all possible expanded signatures.</span></span> <span data-ttu-id="d57fe-209">即使隐藏方法的签名与隐藏方法的未扩展的签名匹配，这是如此。</span><span class="sxs-lookup"><span data-stu-id="d57fe-209">This is true even if the signature of the shadowing method matches the unexpanded signature of the shadowed method.</span></span> <span data-ttu-id="d57fe-210">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="d57fe-210">The following example:</span></span>

```vb
Class Base
    Sub F(ParamArray x() As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived 
    Inherits Base

    Overloads Sub F(x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

<span data-ttu-id="d57fe-211">将打印`Base`，即使`Derived.F`具有相同的签名的未扩展的窗体`Base.F`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-211">prints `Base`, even though `Derived.F` has the same signature as the unexpanded form of `Base.F`.</span></span>

<span data-ttu-id="d57fe-212">相反，具有的方法`ParamArray`参数仅隐藏具有相同的签名不是所有可能的扩展签名的方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-212">Conversely, a method with a `ParamArray` argument only shadows methods with the same signature, not all possible expanded signatures.</span></span> <span data-ttu-id="d57fe-213">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="d57fe-213">The following example:</span></span>

```vb
Class Base
    Sub F(x As Integer)
        Console.WriteLine("Base")
    End Sub
End Class

Class Derived
    Inherits Base

    Overloads Sub F(ParamArray x() As Integer)
        Console.WriteLine("Derived")
    End Sub
End Class

Module Test
    Sub Main()
        Dim d As New Derived()
        d.F(10)
    End Sub
End Module
```

<span data-ttu-id="d57fe-214">将打印`Base`，即使`Derived.F`已具有相同的签名以展开的形式`Base.F`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-214">prints `Base`, even though `Derived.F` has an expanded form that has the same signature as `Base.F`.</span></span>

<span data-ttu-id="d57fe-215">隐藏方法或未指定的属性`Shadows`或`Overloads`假定`Overloads`如果方法或属性声明`Overrides`，`Shadows`否则为。</span><span class="sxs-lookup"><span data-stu-id="d57fe-215">A shadowing method or property that does not specify `Shadows` or `Overloads` assumes `Overloads` if the method or property is declared `Overrides`, `Shadows` otherwise.</span></span> <span data-ttu-id="d57fe-216">如果指定的一组重载实体的一个成员`Shadows`或`Overloads`关键字，它们都必须指定它。</span><span class="sxs-lookup"><span data-stu-id="d57fe-216">If one member of a set of overloaded entities specifies the `Shadows` or `Overloads` keyword, they all must specify it.</span></span> <span data-ttu-id="d57fe-217">`Shadows`和`Overloads`关键字不能同时指定。</span><span class="sxs-lookup"><span data-stu-id="d57fe-217">The `Shadows` and `Overloads` keywords cannot be specified at the same time.</span></span> <span data-ttu-id="d57fe-218">既不`Shadows`也不`Overloads`可以指定标准模块中; 标准模块中的成员隐式隐藏成员继承自`Object`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-218">Neither `Shadows` nor `Overloads` can be specified in a standard module; members in a standard module implicitly shadow members inherited from `Object`.</span></span>

<span data-ttu-id="d57fe-219">它是有效的类型成员的已通过接口继承相乘的继承 （和即从而不可用），名称的卷影从而使该名称在派生接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-219">It is valid to shadow the name of a type member that has been multiply-inherited through interface inheritance (and which is thereby unavailable), thus making the name available in the derived interface.</span></span>

<span data-ttu-id="d57fe-220">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-220">For example:</span></span>

```vb
Interface ILeft
    Sub F()
End Interface

Interface IRight
    Sub F()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight

    Shadows Sub F()
End Interface

Module Test
    Sub G(i As ILeftRight)
        i.F() ' Calls ILeftRight.F.
        CType(i, ILeft).F() ' Calls ILeft.F.
        CType(i, IRight).F() ' Calls IRight.F.
    End Sub
End Module
```

<span data-ttu-id="d57fe-221">由于卷影继承方法允许使用方法，所以有可能类可以包含多个`Overridable`具有相同签名的方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-221">Because methods are allowed to shadow inherited methods, it is possible for a class to contain several `Overridable` methods with the same signature.</span></span> <span data-ttu-id="d57fe-222">这不会造成多义性问题，由于只有派生程度最高的方法是可见。</span><span class="sxs-lookup"><span data-stu-id="d57fe-222">This does not present an ambiguity problem, since only the most-derived method is visible.</span></span> <span data-ttu-id="d57fe-223">在以下示例中，`C`并`D`类包含两个`Overridable`具有相同签名的方法：</span><span class="sxs-lookup"><span data-stu-id="d57fe-223">In the following example, the `C` and `D` classes contain two `Overridable` methods with the same signature:</span></span>

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub 
End Class 

Class B
    Inherits A

    Public Overrides Sub F()
        Console.WriteLine("B.F")
    End Sub 
End Class 

Class C
    Inherits B

    Public Shadows Overridable Sub F()
        Console.WriteLine("C.F")
    End Sub 
End Class 

Class D
    Inherits C

    Public Overrides Sub F()
        Console.WriteLine("D.F")
    End Sub 
End Class 

Module Test
    Sub Main()
        Dim d As New D()
        Dim a As A = d
        Dim b As B = d
        Dim c As C = d
        a.F()
        b.F()
        c.F()
        d.F()
    End Sub 
End Module
```

<span data-ttu-id="d57fe-224">有两个`Overridable`方法： 一个由类引入`A`和一个方法由类`C`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-224">There are two `Overridable` methods here: one introduced by class `A` and the one introduced by class `C`.</span></span> <span data-ttu-id="d57fe-225">引入的类的方法`C`从类继承的方法将隐藏`A`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-225">The method introduced by class `C` hides the method inherited from class `A`.</span></span> <span data-ttu-id="d57fe-226">因此，`Overrides`类中的声明`D`重写方法由类引入`C`，并不能为类`D`若要重写方法由类引入`A`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-226">Thus, the `Overrides` declaration in class `D` overrides the method introduced by class `C`, and it is not possible for class `D` to override the method introduced by class `A`.</span></span> <span data-ttu-id="d57fe-227">该示例生成输出：</span><span class="sxs-lookup"><span data-stu-id="d57fe-227">The example produces the output:</span></span>

```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="d57fe-228">可以调用隐藏`Overridable`通过访问类的实例方法`D`通过该方法不在其中隐藏无派生的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-228">It is possible to invoke the hidden `Overridable` method by accessing an instance of class `D` through a less-derived type in which the method is not hidden.</span></span>

<span data-ttu-id="d57fe-229">它不是有效的卷影`MustOverride`方法，因为在大多数情况下这会导致类不可用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-229">It is not valid to shadow a `MustOverride` method, because in most cases this would make the class unusable.</span></span> <span data-ttu-id="d57fe-230">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-230">For example:</span></span>

```vb
MustInherit Class Base
    Public MustOverride Sub F()
End Class

MustInherit Class Derived
    Inherits Base

    Public Shadows Sub F()
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    ' Error: MustOverride method Base.F is not overridden.
End Class
```

<span data-ttu-id="d57fe-231">在此情况下，该类`MoreDerived`需重写`MustOverride`方法`Base.F`，而是因为类`Derived`阴影`Base.F`，无法做到这一点。</span><span class="sxs-lookup"><span data-stu-id="d57fe-231">In this case, the class `MoreDerived` is required to override the `MustOverride` method `Base.F`, but because the class `Derived` shadows `Base.F`, this is not possible.</span></span> <span data-ttu-id="d57fe-232">没有方法来声明是有效的子代`Derived`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-232">There is no way to declare a valid descendent of `Derived`.</span></span>

<span data-ttu-id="d57fe-233">隐藏来自外部范围的名称，与隐藏继承的作用域中的可访问名称导致警告报告，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="d57fe-233">In contrast to shadowing a name from an outer scope, shadowing an accessible name from an inherited scope causes a warning to be reported, as in the following example:</span></span>

```vb
Class Base
    Public Sub F()
    End Sub

    Private Sub G()
    End Sub 
End Class

Class Derived
    Inherits Base

    Public Sub F() ' Warning: shadowing an inherited name.
    End Sub

    Public Sub G() ' No warning, Base.G is not accessible here.
    End Sub
End Class
```

<span data-ttu-id="d57fe-234">方法的声明`F`类中`Derived`导致报告警告。</span><span class="sxs-lookup"><span data-stu-id="d57fe-234">The declaration of method `F` in class `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="d57fe-235">隐藏继承的名称不具体而言是错误，因为那将排除的基类，这些类的单独演变。</span><span class="sxs-lookup"><span data-stu-id="d57fe-235">Shadowing an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="d57fe-236">例如，如果上述情况可能会发生因为类的更高版本`Base`引入了一种方法`F`未出现在早期版本的类。</span><span class="sxs-lookup"><span data-stu-id="d57fe-236">For example, the above situation might have come about because a later version of class `Base` introduced a method `F` that was not present in an earlier version of the class.</span></span> <span data-ttu-id="d57fe-237">如果上述情况曾出现错误*任何*对基类中单独进行版本控制的类库所做的更改可能会导致派生的类变得无效。</span><span class="sxs-lookup"><span data-stu-id="d57fe-237">Had the above situation been an error, *any* change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="d57fe-238">可以通过使用消除隐藏继承的名称由导致该警告`Shadows`或`Overloads`修饰符：</span><span class="sxs-lookup"><span data-stu-id="d57fe-238">The warning caused by shadowing an inherited name can be eliminated through use of the `Shadows` or `Overloads` modifier:</span></span>

```vb
Class Base
    Public Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Public Shadows Sub F() 'OK.
    End Sub
End Class
```

<span data-ttu-id="d57fe-239">`Shadows`修饰符指示想要隐藏继承的成员。</span><span class="sxs-lookup"><span data-stu-id="d57fe-239">The `Shadows` modifier indicates the intention to shadow the inherited member.</span></span> <span data-ttu-id="d57fe-240">它不是指定的错误`Shadows`或`Overloads`修饰符如果以便隐藏任何类型成员的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-240">It is not an error to specify the `Shadows` or `Overloads` modifier if there is no type member name to shadow.</span></span>

<span data-ttu-id="d57fe-241">新成员的声明隐藏继承的成员只能在新成员，如以下示例所示的范围中：</span><span class="sxs-lookup"><span data-stu-id="d57fe-241">A declaration of a new member shadows an inherited member only within the scope of the new member, as in the following example:</span></span>

```vb
Class Base
    Public Shared Sub F()
    End Sub 
End Class 

Class Derived
    Inherits Base

    Private Shared Shadows Sub F() ' Shadows Base.F in class Derived only.
    End Sub 
End Class 

Class MoreDerived
    Inherits Derived

    Shared Sub G()
        F() ' Invokes Base.F.
    End Sub 
End Class
```

<span data-ttu-id="d57fe-242">在上面的方法声明的示例`F`类中`Derived`隐藏方法`F`类的继承`Base`，但由于新方法`F`类中`Derived`具有`Private`访问权限，其作用域内不会扩展到类`MoreDerived`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-242">In the example above, the declaration of method `F` in class `Derived` shadows the method `F` that was inherited from class `Base`, but since the new method `F` in class `Derived` has `Private` access, its scope does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="d57fe-243">因此，调用`F()`中`MoreDerived.G`有效，并且将调用`Base.F`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-243">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span> <span data-ttu-id="d57fe-244">在重载的类型成员的情况下重载的类型成员的整个集视为如同是它们都具有用于隐藏的最宽松的访问权限。</span><span class="sxs-lookup"><span data-stu-id="d57fe-244">In the case of overloaded type members, the entire set of overloaded type members is treated as if they all had the most permissive access for the purposes of shadowing.</span></span>

```vb
Class Base
    Public Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F()
    End Sub

    Public Shadows Sub F(i As Integer)
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Sub G()
        F()   ' Error. No accessible member with this signature.
    End Sub
End Class
```

<span data-ttu-id="d57fe-245">在此示例中，即使的声明`F()`中`Derived`使用声明`Private`访问，请重载`F(Integer)`使用声明`Public`访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-245">In this example, even though the declaration of `F()` in `Derived` is declared with `Private` access, the overloaded `F(Integer)` is declared with `Public` access.</span></span> <span data-ttu-id="d57fe-246">因此，为了隐藏名称`F`中`Derived`就像它是被视为`Public`，因此这两种方法卷影`F`中`Base`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-246">Therefore, for the purpose of shadowing, the name `F` in `Derived` is treated as if it was `Public`, so both methods shadow `F` in `Base`.</span></span>

## <a name="implementation"></a><span data-ttu-id="d57fe-247">实现</span><span class="sxs-lookup"><span data-stu-id="d57fe-247">Implementation</span></span>

<span data-ttu-id="d57fe-248">*实现*类型声明它实现了接口，则该类型实现的接口的所有类型成员时，存在关系。</span><span class="sxs-lookup"><span data-stu-id="d57fe-248">An *implementation* relationship exists when a type declares that it implements an interface and the type implements all the type members of the interface.</span></span> <span data-ttu-id="d57fe-249">实现特定接口的类型是转换为该接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-249">A type that implements a particular interface is convertible to that interface.</span></span> <span data-ttu-id="d57fe-250">不能实例化接口，但可以声明变量的接口;此类变量只能分配一个值，是实现接口的类。</span><span class="sxs-lookup"><span data-stu-id="d57fe-250">Interfaces cannot be instantiated, but it is valid to declare variables of interfaces; such variables can only be assigned a value that is of a class that implements the interface.</span></span> <span data-ttu-id="d57fe-251">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-251">For example:</span></span>

```vb
Interface ITestable
    Function Test(value As Byte) As Boolean
End Interface

Class TestableClass
    Implements ITestable

    Function Test(value As Byte) As Boolean Implements ITestable.Test
        Return value > 128
    End Function
End Class

Module Test
    Sub F()
        Dim x As ITestable = New TestableClass
        Dim b As Boolean

        b = x.Test(34)
    End Sub
End Module
```

<span data-ttu-id="d57fe-252">与乘继承类型成员实现接口的类型仍必须实现这些方法，即使它们不能直接从要实现的派生接口访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-252">A type implementing an interface with multiply-inherited type members must still implement those methods, even though they cannot be accessed directly from the derived interface being implemented.</span></span> <span data-ttu-id="d57fe-253">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-253">For example:</span></span>

```vb
Interface ILeft
    Sub Test()
End Interface

Interface IRight
    Sub Test()
End Interface

Interface ILeftRight
    Inherits ILeft, IRight
End Interface

Class LeftRight
    Implements ILeftRight

    ' Has to reference ILeft explicitly.
    Sub TestLeft() Implements ILeft.Test
    End Sub

    ' Has to reference IRight explicitly.
    Sub TestRight() Implements IRight.Test
    End Sub

    ' Error: Test is not available in ILeftRight.
    Sub TestLeftRight() Implements ILeftRight.Test
    End Sub
End Class
```

<span data-ttu-id="d57fe-254">甚至`MustInherit`类必须提供的所有成员的实现的接口实现; 但是，可以通过将它们声明为延迟这些方法的实现`MustOverride`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-254">Even `MustInherit` classes must provide implementations of all the members of implemented interfaces; however, they can defer implementation of these methods by declaring them as `MustOverride`.</span></span> <span data-ttu-id="d57fe-255">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-255">For example:</span></span>

```vb
Interface ITest
    Sub Test1()
    Sub Test2()
End Interface

MustInherit Class TestBase
    Implements ITest

    ' Provides an implementation.
    Sub Test1() Implements ITest.Test1
    End Sub

    ' Defers implementation.
    MustOverride Sub Test2() Implements ITest.Test2
End Class

Class TestDerived
    Inherits TestBase

    ' Have to implement MustOverride method.
    Overrides Sub Test2()
    End Sub
End Class
```

<span data-ttu-id="d57fe-256">可以选择一种类型重新实现其基类型实现的接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-256">A type may choose to re-implement an interface that its base type implements.</span></span> <span data-ttu-id="d57fe-257">若要重新实现该接口，该类型必须显式声明它实现该接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-257">To re-implement the interface, the type must explicitly state that it implements the interface.</span></span> <span data-ttu-id="d57fe-258">可以选择重新实现接口的类型重新实现只有某些但并非所有接口-不重新实现的任何成员的成员继续使用基类型实现。</span><span class="sxs-lookup"><span data-stu-id="d57fe-258">A type re-implementing an interface may choose to re-implement only some, but not all, of the members of the interface -- any members not re-implemented continue to use the base type's implementation.</span></span> <span data-ttu-id="d57fe-259">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-259">For example:</span></span>

```vb
Class TestBase
    Implements ITest

    Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class TestDerived
    Inherits TestBase
    Implements ITest  ' Required to re-implement

    Sub DerivedTest1() Implements ITest.Test1
        Console.WriteLine("TestDerived.DerivedTest1")
    End Sub
End Class

Module Test
    Sub Main()
        Dim Test As ITest = New TestDerived()
        Test.Test1()
        Test.Test2()
    End Sub
End Module
```

<span data-ttu-id="d57fe-260">此示例输出：</span><span class="sxs-lookup"><span data-stu-id="d57fe-260">This example prints:</span></span>

```
TestDerived.DerivedTest1
TestBase.Test2
```

<span data-ttu-id="d57fe-261">当派生的类型实现的基接口由派生的类型的基类型实现的接口时，可以选择仅实现已经不由基类型实现的接口的类型成员的派生的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-261">When a derived type implements an interface whose base interfaces are implemented by the derived type's base types, the derived type can choose to only implement the interface's type members that are not already implemented by the base types.</span></span> <span data-ttu-id="d57fe-262">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-262">For example:</span></span>

```vb
Interface IBase
    Sub Base()
End Interface

Interface IDerived
    Inherits IBase

    Sub Derived()
End Interface

Class Base
    Implements IBase

    Public Sub Base() Implements IBase.Base
    End Sub
End Class

Class Derived
    Inherits Base
    Implements IDerived

    ' Required: IDerived.Derived not implemented by Base.
    Public Sub Derived() Implements IDerived.Derived
    End Sub
End Class
```

<span data-ttu-id="d57fe-263">此外可以使用可重写方法中的基类型实现接口方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-263">An interface method can also be implemented using an overridable method in a base type.</span></span> <span data-ttu-id="d57fe-264">在这种情况下，派生的类型也可能会重写可重写方法并更改接口的实现。</span><span class="sxs-lookup"><span data-stu-id="d57fe-264">In that case, a derived type may also override the overridable method and alter the implementation of the interface.</span></span> <span data-ttu-id="d57fe-265">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-265">For example:</span></span>

```vb
Class Base
    Implements ITest

    Public Sub Test1() Implements ITest.Test1
        Console.WriteLine("TestBase.Test1")
    End Sub

    Public Overridable Sub Test2() Implements ITest.Test2
        Console.WriteLine("TestBase.Test2")
    End Sub
End Class

Class Derived
    Inherits Base

    ' Overrides base implementation.
    Public Overrides Sub Test2()
        Console.WriteLine("TestDerived.Test2")
    End Sub
End Class
```

### <a name="implementing-methods"></a><span data-ttu-id="d57fe-266">实现的方法</span><span class="sxs-lookup"><span data-stu-id="d57fe-266">Implementing Methods</span></span>

<span data-ttu-id="d57fe-267">一种类型*实现*通过提供具有的方法实现的接口的类型成员`Implements`子句。</span><span class="sxs-lookup"><span data-stu-id="d57fe-267">A type *implements* a type member of an implemented interface by supplying a method with an `Implements` clause.</span></span> <span data-ttu-id="d57fe-268">两个类型成员必须具有相同数量的参数，所有类型和参数的修饰符必须匹配，包括可选参数的默认值、 返回类型必须与匹配，，所有方法参数的约束必须匹配。</span><span class="sxs-lookup"><span data-stu-id="d57fe-268">The two type members must have the same number of parameters, all of the types and modifiers of the parameters must match, including the default value of optional parameters, the return type must match, and all of the constraints on method parameters must match.</span></span> <span data-ttu-id="d57fe-269">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-269">For example:</span></span>

```vb
Interface ITest
    Sub F(ByRef x As Integer)
    Sub G(Optional y As Integer = 20)
    Sub H(Paramarray z() As Integer)
End Interface

Class Test
    Implements ITest

    ' Error: ByRef/ByVal mismatch.
    Sub F(x As Integer) Implements ITest.F
    End Sub

    ' Error: Defaults do not match.
    Sub G(Optional y As Integer = 10) Implements ITest.G
    End Sub

    ' Error: Paramarray does not match.
    Sub H(z() As Integer) Implements ITest.H
    End Sub
End Class
```

<span data-ttu-id="d57fe-270">如果它们都符合上述条件，一个方法可以实现任意数量的接口类型成员。</span><span class="sxs-lookup"><span data-stu-id="d57fe-270">A single method may implement any number of interface type members if they all meet the above criteria.</span></span> <span data-ttu-id="d57fe-271">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-271">For example:</span></span>

```vb
Interface ITest
    Sub F(i As Integer)
    Sub G(i As Integer)
End Interface

Class Test

    Implements ITest

    Sub F(i As Integer) Implements ITest.F, ITest.G
    End Sub
End Class
```

<span data-ttu-id="d57fe-272">当在泛型接口中实现一种方法，实现方法必须提供对应于接口的类型参数的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-272">When implementing a method in a generic interface, the implementing method must supply the type arguments that correspond to the interface's type parameters.</span></span> <span data-ttu-id="d57fe-273">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-273">For example:</span></span>

```vb
Interface I1(Of U, V) 
    Sub M(x As U, y As List(Of V)) 
End Interface

Class C1(Of W, X)
    Implements I1(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Sub M(x As W, y As List(Of X)) Implements I1(Of W, X).M
    End Sub 
End Class

Class C2
    Implements I1(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Sub M(x As String, y As List(Of Integer)) _
        Implements I1(Of String, Integer).M
    End Sub
End Class
```

<span data-ttu-id="d57fe-274">请注意，可能不可能实施的一些组类型参数的泛型接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-274">Note that it is possible that a generic interface may not be implementable for some set of type arguments.</span></span>

```vb
Interface I1(Of T, U)
    Sub S1(x As T)
    Sub S1(y As U)
End Interface

Class C1
    ' Unable to implement because I1.S1 has two identical signatures
    Implements I1(Of Integer, Integer)
End Class
```

## <a name="polymorphism"></a><span data-ttu-id="d57fe-275">多态性</span><span class="sxs-lookup"><span data-stu-id="d57fe-275">Polymorphism</span></span>

<span data-ttu-id="d57fe-276">*多态性*提供了不同的方法或属性的实现的功能。</span><span class="sxs-lookup"><span data-stu-id="d57fe-276">*Polymorphism* provides the ability to vary the implementation of a method or property.</span></span> <span data-ttu-id="d57fe-277">使用多态性，相同的方法或属性可以执行不同的操作，具体取决于对其进行调用的实例的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-277">With polymorphism, the same method or property can perform different actions depending on the run-time type of the instance that invokes it.</span></span> <span data-ttu-id="d57fe-278">调用方法或属性是多态*可重写*。</span><span class="sxs-lookup"><span data-stu-id="d57fe-278">Methods or properties that are polymorphic are called *overridable*.</span></span> <span data-ttu-id="d57fe-279">与此相反，非可重写方法或属性的实现是不变;实现都是相同的方法或属性调用中的类的实例上是否已声明的或派生类的实例。</span><span class="sxs-lookup"><span data-stu-id="d57fe-279">By contrast, the implementation of a non-overridable method or property is invariant; the implementation is the same whether the method or property is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="d57fe-280">调用的非可重写方法或属性时，该实例的编译时类型是决定性因素。</span><span class="sxs-lookup"><span data-stu-id="d57fe-280">When a non-overridable method or property is invoked, the compile-time type of the instance is the determining factor.</span></span> <span data-ttu-id="d57fe-281">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-281">For example:</span></span>

```vb
Class Base
    Public Overridable Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Class Derived
    Inherits Base

    Public Overrides Property X() As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

Module Test
    Sub F()
        Dim Z As Base

        Z = New Base()
        Z.X = 10            ' Calls Base.X
        Z = New Derived()
        Z.X = 10            ' Calls Derived.X
    End Sub
End Module
```

<span data-ttu-id="d57fe-282">可重写方法也可能是`MustOverride`，这意味着它提供没有方法正文，并必须重写。</span><span class="sxs-lookup"><span data-stu-id="d57fe-282">An overridable method may also be `MustOverride`, which means that it provides no method body and must be overridden.</span></span> <span data-ttu-id="d57fe-283">`MustOverride` 方法只允许在`MustInherit`类。</span><span class="sxs-lookup"><span data-stu-id="d57fe-283">`MustOverride` methods are only allowed in `MustInherit` classes.</span></span>

<span data-ttu-id="d57fe-284">在下面的示例中，类`Shape`定义本身就可以绘制一个几何形状对象的抽象概念：</span><span class="sxs-lookup"><span data-stu-id="d57fe-284">In the following example, the class `Shape` defines the abstract notion of a geometrical shape object that can paint itself:</span></span>

```vb
MustInherit Public Class Shape
    Public MustOverride Sub Paint(g As Graphics, r As Rectangle)
End Class 

Public Class Ellipse
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawEllipse(r)
    End Sub 
End Class 

Public Class Box
    Inherits Shape

    Public Overrides Sub Paint(g As Graphics, r As Rectangle)
        g.drawRect(r)
    End Sub 
End Class
```

<span data-ttu-id="d57fe-285">`Paint`方法是`MustOverride`因为没有有意义的默认实现。</span><span class="sxs-lookup"><span data-stu-id="d57fe-285">The `Paint` method is `MustOverride` because there is no meaningful default implementation.</span></span> <span data-ttu-id="d57fe-286">`Ellipse`并`Box`类是具体`Shape`实现。</span><span class="sxs-lookup"><span data-stu-id="d57fe-286">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="d57fe-287">因为这些类不是`MustInherit`，他们需要对重写`Paint`方法，并提供实际实现。</span><span class="sxs-lookup"><span data-stu-id="d57fe-287">Because these classes are not `MustInherit`, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="d57fe-288">它是错误的基本的访问权限，以引用`MustOverride`方法，如下面的示例演示：</span><span class="sxs-lookup"><span data-stu-id="d57fe-288">It is an error for a base access to reference a `MustOverride` method, as the following example demonstrates:</span></span>

```vb
MustInherit Class A
    Public MustOverride Sub F()
End Class

Class B
    Inherits A

    Public Overrides Sub F()
        MyBase.F() ' Error, MyBase.F is MustOverride.
    End Sub 
End Class
```

<span data-ttu-id="d57fe-289">有关报告的错误`MyBase.F()`调用因为它引用了`MustOverride`方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-289">An error is reported for the `MyBase.F()` invocation because it references a `MustOverride` method.</span></span>

### <a name="overriding-methods"></a><span data-ttu-id="d57fe-290">重写方法</span><span class="sxs-lookup"><span data-stu-id="d57fe-290">Overriding Methods</span></span>

<span data-ttu-id="d57fe-291">一种类型可能*重写*继承的可重写方法通过声明的方法具有相同的名称和，签名，并将标记与声明`Overrides`修饰符。没有重写方法，下面列出的其他要求。</span><span class="sxs-lookup"><span data-stu-id="d57fe-291">A type may *override* an inherited overridable method by declaring a method with the same name and , signature, and marking the declaration with the `Overrides` modifier.There are additional requirements on overriding methods, listed below.</span></span> <span data-ttu-id="d57fe-292">而`Overridable`方法声明中引入的新方法，`Overrides`方法声明替换继承的方法实现。</span><span class="sxs-lookup"><span data-stu-id="d57fe-292">Whereas an `Overridable` method declaration introduces a new method, an `Overrides` method declaration replaces the inherited implementation of the method.</span></span>

<span data-ttu-id="d57fe-293">重写方法可能会声明`NotOverridable`，以防止进一步重写的任何派生类型中的方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-293">An overriding method may be declared `NotOverridable`, which prevents any further overriding of the method in derived types.</span></span> <span data-ttu-id="d57fe-294">实际上，`NotOverridable`方法成为中不可重写任何进一步派生类。</span><span class="sxs-lookup"><span data-stu-id="d57fe-294">In effect, `NotOverridable` methods become non-overridable in any further derived classes.</span></span>

<span data-ttu-id="d57fe-295">请看下面的示例：</span><span class="sxs-lookup"><span data-stu-id="d57fe-295">Consider the following example:</span></span>

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub

    Public Overridable Sub G()
        Console.WriteLine("A.G")
    End Sub
End Class

Class B
    Inherits A

    Public Overrides NotOverridable Sub F()
        Console.WriteLine("B.F")
    End Sub

    Public Overrides Sub G()
        Console.WriteLine("B.G")
    End Sub
End Class

Class C
    Inherits B

    Public Overrides Sub G()
        Console.WriteLine("C.G")
    End Sub
End Class
```

<span data-ttu-id="d57fe-296">在示例中，类`B`提供了两个`Overrides`方法： 一种方法`F`具有`NotOverridable`修饰符和方法`G`不执行的。</span><span class="sxs-lookup"><span data-stu-id="d57fe-296">In the example, class `B` provides two `Overrides` methods: a method `F` that has the `NotOverridable` modifier and a method `G` that does not.</span></span> <span data-ttu-id="d57fe-297">利用`NotOverridable`修饰符阻止类`C`进一步重写方法`F`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-297">Use of the `NotOverridable` modifier prevents class `C` from further overriding method `F`.</span></span>

<span data-ttu-id="d57fe-298">此外可以声明重写方法`MustOverride`，即使它在重写的方法未声明`MustOverride`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-298">An overriding method may also be declared `MustOverride`, even if the method that it is overriding is not declared `MustOverride`.</span></span> <span data-ttu-id="d57fe-299">这需要声明包含的类`MustInherit`和任何进一步派生类未声明为`MustInherit`必须重写方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-299">This requires that the containing class be declared `MustInherit` and that any further derived classes that are not declared `MustInherit` must override the method.</span></span> <span data-ttu-id="d57fe-300">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-300">For example:</span></span>

```vb
Class A
    Public Overridable Sub F()
        Console.WriteLine("A.F")
    End Sub
End Class

MustInherit Class B
    Inherits A

    Public Overrides MustOverride Sub F()
End Class
```

<span data-ttu-id="d57fe-301">在示例中，类`B`重写`A.F`与`MustOverride`方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-301">In the example, class `B` overrides `A.F` with a `MustOverride` method.</span></span> <span data-ttu-id="d57fe-302">这意味着任何类派生自`B`将必须重写`F`，除非它们声明为`MustInherit`也。</span><span class="sxs-lookup"><span data-stu-id="d57fe-302">This means that any classes derived from `B` will have to override `F`, unless they are declared `MustInherit` as well.</span></span>

<span data-ttu-id="d57fe-303">除非以下条件都重写方法，则返回 true，否则，将发生编译时错误：</span><span class="sxs-lookup"><span data-stu-id="d57fe-303">A compile-time error occurs unless all of the following are true of an overriding method:</span></span>

* <span data-ttu-id="d57fe-304">声明上下文包含具有相同的签名和返回类型 （如果有） 的单个可访问继承的方法作为重写方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-304">The declaration context contains a single accessible inherited method with the same signature and return type (if any) as the overriding method.</span></span>
* <span data-ttu-id="d57fe-305">正在重写继承的方法是可重写。</span><span class="sxs-lookup"><span data-stu-id="d57fe-305">The inherited method being overridden is overridable.</span></span> <span data-ttu-id="d57fe-306">换而言之，正在重写继承的方法不是`Shared`或`NotOverridable`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-306">In other words, the inherited method being overridden is not `Shared` or `NotOverridable`.</span></span>
* <span data-ttu-id="d57fe-307">所声明的可访问域是方法的被重写继承方法的可访问性域相同。</span><span class="sxs-lookup"><span data-stu-id="d57fe-307">The accessibility domain of the method being declared is the same as the accessibility domain of the inherited method being overridden.</span></span> <span data-ttu-id="d57fe-308">没有一种情况例外：`Protected Friend`必须通过重写方法`Protected`方法，如果另一种方法是在另一个程序集重写方法不具有`Friend`访问权限。</span><span class="sxs-lookup"><span data-stu-id="d57fe-308">There is one exception: a `Protected Friend` method must be overridden by a `Protected` method if the other method is in another assembly that the overriding method does not have `Friend` access to.</span></span>
* <span data-ttu-id="d57fe-309">重写方法的参数匹配的使用情况有关的重写的方法的参数`ByVal`， `ByRef`，`ParamArray,`和`Optional`修饰符，其中包括为可选参数提供的值。</span><span class="sxs-lookup"><span data-stu-id="d57fe-309">The parameters of the overriding method match the overridden method's parameters in regards to usage of the `ByVal`, `ByRef`, `ParamArray,` and `Optional` modifiers, including the values provided for optional parameters.</span></span>
* <span data-ttu-id="d57fe-310">重写方法的类型参数与类型约束有关的重写的方法类型参数匹配。</span><span class="sxs-lookup"><span data-stu-id="d57fe-310">The type parameters of the overriding method match the overridden method's type parameters in regards to type constraints.</span></span>

<span data-ttu-id="d57fe-311">当重写基的泛型类型中的方法，重写方法必须提供对应于基类型参数的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-311">When overriding a method in a base generic type, the overriding method must supply the type arguments that correspond to the base type parameters.</span></span> <span data-ttu-id="d57fe-312">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-312">For example:</span></span>

```vb
Class Base(Of U, V) 
    Public Overridable Sub M(x As U, y As List(Of V)) 
    End Sub
End Class

Class Derived(Of W, X)
    Inherits Base(Of W, X)

    ' W corresponds to U and X corresponds to V
    Public Overrides Sub M(x As W, y As List(Of X)) 
    End Sub 
End Class

Class MoreDerived
    Inherits Derived(Of String, Integer)

    ' String corresponds to U and Integer corresponds to V
    Public Overrides Sub M(x As String, y As List(Of Integer))
    End Sub
End Class
```

<span data-ttu-id="d57fe-313">请注意，可能在泛型类中重写的方法可能无法被覆盖的一些组类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-313">Note that it is possible that an overridable method in a generic class may not be able to be overridden for some sets of type arguments.</span></span> <span data-ttu-id="d57fe-314">如果该方法声明为`MustOverride`，这意味着，某些继承链不可能。</span><span class="sxs-lookup"><span data-stu-id="d57fe-314">If the method is declared `MustOverride`, this means that some inheritance chains may not be possible.</span></span> <span data-ttu-id="d57fe-315">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-315">For example:</span></span>

```vb
MustInherit Class Base(Of T, U)
    Public MustOverride Sub S1(x As T)
    Public MustOverride Sub S1(y As U)
End Class

Class Derived
    Inherits Base(Of Integer, Integer)

    ' Error: Can't override both S1's at once
    Public Overrides Sub S1(x As Integer)
    End Sub
End Class
```

<span data-ttu-id="d57fe-316">重写声明可以访问使用基本的访问权限，如以下示例所示重写基方法：</span><span class="sxs-lookup"><span data-stu-id="d57fe-316">An override declaration can access the overridden base method using a base access, as in the following example:</span></span>

```vb
Class Base
    Private x As Integer

    Public Overridable Sub PrintVariables()
        Console.WriteLine("x = " & x)
    End Sub
End Class

Class Derived
    Inherits Base

    Private y As Integer

    Public Overrides Sub PrintVariables()
        MyBase.PrintVariables()
        Console.WriteLine("y = " & y)
    End Sub
End Class
```

在示例中，调用`MyBase.PrintVariables()`类中`Derived`调用`PrintVariables`类中声明方法`Base`。 基访问禁用的可重写调用机制，并只需将视为非可重写方法的基方法。 <span data-ttu-id="d57fe-319">必须调用`Derived`已写入`CType(Me, Base).PrintVariables()`，它会以递归方式调用`PrintVariables`方法中声明`Derived`中, 声明一个不`Base`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-319">Had the invocation in `Derived` been written `CType(Me, Base).PrintVariables()`, it would recursively invoke the `PrintVariables` method declared in `Derived`, not the one declared in `Base`.</span></span>

<span data-ttu-id="d57fe-320">仅当它包括`Overrides`修饰符可以一种方法重写另一种方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-320">Only when it includes an `Overrides` modifier can a method override another method.</span></span> <span data-ttu-id="d57fe-321">在所有其他情况下，具有与继承的方法相同的签名的方法只是重写继承的方法，如下面的示例中所示：</span><span class="sxs-lookup"><span data-stu-id="d57fe-321">In all other cases, a method with the same signature as an inherited method simply shadows the inherited method, as in the example below:</span></span>

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Public Overridable Sub F() ' Warning, shadowing inherited F().
    End Sub
End Class
```

<span data-ttu-id="d57fe-322">在示例中，该方法`F`类中`Derived`不包括`Overrides`修饰符，因此不重写方法`F`类中`Base`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-322">In the example, the method `F` in class `Derived` does not include an `Overrides` modifier and therefore does not override method `F` in class `Base`.</span></span> <span data-ttu-id="d57fe-323">相反，方法`F`类中`Derived`隐藏类中的方法`Base`，并将报告警告，因为在声明中不包含`Shadows`或`Overloads`修饰符。</span><span class="sxs-lookup"><span data-stu-id="d57fe-323">Rather, method `F` in class `Derived` shadows the method in class `Base`, and a warning is reported because the declaration does not include a `Shadows` or `Overloads` modifier.</span></span>

<span data-ttu-id="d57fe-324">在下面的示例中，方法`F`类中`Derived`隐藏可重写方法`F`继承自类`Base`:</span><span class="sxs-lookup"><span data-stu-id="d57fe-324">In the following example, method `F` in class `Derived` shadows the overridable method `F` inherited from class `Base`:</span></span>

```vb
Class Base
    Public Overridable Sub F()
    End Sub
End Class

Class Derived
    Inherits Base

    Private Shadows Sub F() ' Shadows Base.F within Derived.
    End Sub
End Class

Class MoreDerived
    Inherits Derived

    Public Overrides Sub F() ' Ok, overrides Base.F.
    End Sub
End Class
```

<span data-ttu-id="d57fe-325">以来的新方法`F`类中`Derived`已`Private`访问权限，其作用域仅包括类正文`Derived`并不会扩展到类`MoreDerived`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-325">Since the new method `F` in class `Derived` has `Private` access, its scope only includes the class body of `Derived` and does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="d57fe-326">方法的声明`F`类中`MoreDerived`因此可以重写该方法`F`继承自类`Base`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-326">The declaration of method `F` in class `MoreDerived` is therefore permitted to override the method `F` inherited from class `Base`.</span></span>

<span data-ttu-id="d57fe-327">当`Overridable`调用方法、 调用实例方法的派生程度最高的实现，基于实例，而不考虑调用是对类的基类或派生的类中方法的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-327">When an `Overridable` method is invoked, the most derived implementation of the instance method is called, based on the type of the instance, regardless of whether the call is to the method in the base class or the derived class.</span></span> <span data-ttu-id="d57fe-328">派生程度最高的实现`Overridable`方法`M`相对于一个类`R`，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="d57fe-328">The most derived implementation of an `Overridable` method `M` with respect to a class `R` is determined as follows:</span></span>

* <span data-ttu-id="d57fe-329">如果`R`包含引入`Overridable`的声明`M`，这是派生程度最高的实现`M`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-329">If `R` contains the introducing `Overridable` declaration of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="d57fe-330">否则为如果`R`包含的重写`M`，这是派生程度最高的实现`M`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-330">Otherwise, if `R` contains an override of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="d57fe-331">否则，大多数派生的实现`M`是相同的类的直接基类`R`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-331">Otherwise, the most derived implementation of `M` is the same as that of the direct base class of `R`.</span></span>

## <a name="accessibility"></a><span data-ttu-id="d57fe-332">可访问性</span><span class="sxs-lookup"><span data-stu-id="d57fe-332">Accessibility</span></span>

<span data-ttu-id="d57fe-333">声明还指定*可访问性*它声明的实体。</span><span class="sxs-lookup"><span data-stu-id="d57fe-333">A declaration specifies the *accessibility* of the entity it declares.</span></span> <span data-ttu-id="d57fe-334">实体的可访问性不会更改实体的名称的作用域。</span><span class="sxs-lookup"><span data-stu-id="d57fe-334">An entity's accessibility does not change the scope of an entity's name.</span></span> <span data-ttu-id="d57fe-335">*可访问性域*声明的是一的组的所有声明空间中声明的实体是可访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-335">The *accessibility domain* of a declaration is the set of all declaration spaces in which the declared entity is accessible.</span></span>

<span data-ttu-id="d57fe-336">五种访问类型是`Public`， `Protected`， `Friend`， `Protected Friend`，和`Private`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-336">The five access types are `Public`, `Protected`, `Friend`, `Protected Friend`, and `Private`.</span></span> <span data-ttu-id="d57fe-337">`Public` 是最宽松的访问类型和四种类型是所有子集`Public`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-337">`Public` is the most permissive access type, and the four other types are all subsets of `Public`.</span></span> <span data-ttu-id="d57fe-338">最低权限的访问类型是`Private`，和四个其他访问类型是所有的超集`Private`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-338">The least permissive access type is `Private`, and the four other access types are all supersets of `Private`.</span></span>

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

<span data-ttu-id="d57fe-339">声明的访问类型指定一个可选的访问修饰符，可通过`Public`， `Protected`， `Friend`， `Private`，或为的组合`Protected`和`Friend`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-339">The access type for a declaration is specified via an optional access modifier, which can be `Public`, `Protected`, `Friend`, `Private`, or the combination of `Protected` and `Friend`.</span></span> <span data-ttu-id="d57fe-340">如果指定没有访问修饰符，则默认访问类型取决于声明上下文中;允许访问权限类型还取决于声明上下文。</span><span class="sxs-lookup"><span data-stu-id="d57fe-340">If no access modifier is specified, the default access type depends on the declaration context; the permitted access types also depend on the declaration context.</span></span>

* <span data-ttu-id="d57fe-341">使用实体声明`Public`修饰符有`Public`访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-341">Entities declared with the `Public` modifier have `Public` access.</span></span> <span data-ttu-id="d57fe-342">没有任何限制的使用`Public`实体。</span><span class="sxs-lookup"><span data-stu-id="d57fe-342">There are no restrictions on the use of `Public` entities.</span></span>

* <span data-ttu-id="d57fe-343">使用实体声明`Protected`修饰符有`Protected`访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-343">Entities declared with the `Protected` modifier have `Protected` access.</span></span> <span data-ttu-id="d57fe-344">`Protected` 只能对成员的类 （正则类型成员和嵌套的类） 或在指定访问权限`Overridable`标准模块和结构的成员 (这必须根据定义，继承自`System.Object`或`System.ValueType`)。</span><span class="sxs-lookup"><span data-stu-id="d57fe-344">`Protected` access can only be specified on members of classes (both regular type members and nested classes) or on `Overridable` members of standard modules and structures (which must, by definition, be inherited from `System.Object` or `System.ValueType`).</span></span> <span data-ttu-id="d57fe-345">一个`Protected`成员是派生类中，可以访问，但前提是该成员不是实例成员，或者访问都是通过派生类的实例。</span><span class="sxs-lookup"><span data-stu-id="d57fe-345">A `Protected` member is accessible to a derived class, provided that either the member is not an instance member, or the access takes place through an instance of the derived class.</span></span> <span data-ttu-id="d57fe-346">`Protected` 访问不是一个超集`Friend`访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-346">`Protected` access is not a superset of `Friend` access.</span></span>

* <span data-ttu-id="d57fe-347">使用实体声明`Friend`修饰符有`Friend`访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-347">Entities declared with the `Friend` modifier have `Friend` access.</span></span> <span data-ttu-id="d57fe-348">如果一个实体具有`Friend`访问权限是只能在包含实体声明或已被授予的任何程序集的程序中访问`Friend`通过访问`System.Runtime.CompilerServices.InternalsVisibleToAttribute`属性。</span><span class="sxs-lookup"><span data-stu-id="d57fe-348">An entity with `Friend` access is accessible only within the program that contains the entity declaration or any assemblies that have been given `Friend` access through the `System.Runtime.CompilerServices.InternalsVisibleToAttribute` attribute.</span></span>

* <span data-ttu-id="d57fe-349">使用实体声明`Protected Friend`修饰符有的联合`Protected`和`Friend`访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-349">Entities declared with the `Protected Friend` modifiers have the union of `Protected` and `Friend` access.</span></span>

* <span data-ttu-id="d57fe-350">使用实体声明`Private`修饰符有`Private`访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-350">Entities declared with the `Private` modifier have `Private` access.</span></span> <span data-ttu-id="d57fe-351">一个`Private`实体是只能在其声明上下文，包括任何嵌套的实体中访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-351">A `Private` entity is accessible only within its declaration context, including any nested entities.</span></span>

<span data-ttu-id="d57fe-352">在声明中的可访问性不依赖于声明上下文的可访问性。</span><span class="sxs-lookup"><span data-stu-id="d57fe-352">The accessibility in a declaration does not depend on the accessibility of the declaration context.</span></span> <span data-ttu-id="d57fe-353">例如，与声明的类型`Private`访问可能包含的类型成员`Public`访问。</span><span class="sxs-lookup"><span data-stu-id="d57fe-353">For example, a type declared with `Private` access may contain a type member with `Public` access.</span></span>

<span data-ttu-id="d57fe-354">下面的代码演示了各种可访问性域：</span><span class="sxs-lookup"><span data-stu-id="d57fe-354">The following code demonstrates various accessibility domains:</span></span>

```vb
Public Class A
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer
End Class

Friend Class B
    Public Shared X As Integer
    Friend Shared Y As Integer
    Private Shared Z As Integer

    Public Class C
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class

    Private Class D
        Public Shared X As Integer
        Friend Shared Y As Integer
        Private Shared Z As Integer
    End Class
End Class
```

<span data-ttu-id="d57fe-355">类和在此示例中的成员具有以下可访问性域：</span><span class="sxs-lookup"><span data-stu-id="d57fe-355">The classes and members in this example have the following accessibility domains:</span></span>

* <span data-ttu-id="d57fe-356">可访问性域`A`和`A.X`不受限制。</span><span class="sxs-lookup"><span data-stu-id="d57fe-356">The accessibility domain of `A` and `A.X` is unlimited.</span></span>

* <span data-ttu-id="d57fe-357">可访问性域`A.Y`， `B`， `B.X`， `B.Y`， `B.C`， `B.C.X`，和`B.C.Y`是包含程序。</span><span class="sxs-lookup"><span data-stu-id="d57fe-357">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the containing program.</span></span>

* <span data-ttu-id="d57fe-358">可访问域`A.Z`是 `A.`</span><span class="sxs-lookup"><span data-stu-id="d57fe-358">The accessibility domain of `A.Z` is `A.`</span></span>

* <span data-ttu-id="d57fe-359">可访问性域`B.Z`， `B.D`， `B.D.X`，和`B.D.Y`是`B`，其中包括`B.C`和`B.D`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-359">The accessibility domain of `B.Z`, `B.D`, `B.D.X`, and `B.D.Y` is `B`, including `B.C` and `B.D`.</span></span>

* <span data-ttu-id="d57fe-360">可访问性域`B.C.Z`是`B.C`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-360">The accessibility domain of `B.C.Z` is `B.C`.</span></span>

* <span data-ttu-id="d57fe-361">可访问性域`B.D.Z`是`B.D`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-361">The accessibility domain of `B.D.Z` is `B.D`.</span></span>

<span data-ttu-id="d57fe-362">如示例所示，成员的可访问域是类型的永远不会超过包含。</span><span class="sxs-lookup"><span data-stu-id="d57fe-362">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="d57fe-363">例如，即使所有`X`的成员具有`Public`声明可访问性，以外的所有`A.X`有包含类型受约束的可访问性域。</span><span class="sxs-lookup"><span data-stu-id="d57fe-363">For example, even though all `X` members have `Public` declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="d57fe-364">访问`Protected`成员必须通过派生类型的实例，以便不相关的类型不能访问彼此的实例具有受保护的成员。</span><span class="sxs-lookup"><span data-stu-id="d57fe-364">Access to `Protected` instance members must be through an instance of the derived type so that unrelated types cannot gain access to each other's protected members.</span></span> <span data-ttu-id="d57fe-365">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-365">For example:</span></span>

```vb
Class User
    Protected Password As String
End Class

Class Employee
    Inherits User
End Class

Class Guest
    Inherits User

    Public Function GetPassword(u As User) As String
        ' Error: protected access has to go through derived type.
        Return U.Password
    End Function
End Class
```

<span data-ttu-id="d57fe-366">在上面的示例中，类`Guest`仅有权访问受保护`Password`如果它受限定的实例字段`Guest`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-366">In the above example, the class `Guest` only has access to the protected `Password` field if it is qualified with an instance of `Guest`.</span></span> <span data-ttu-id="d57fe-367">这可以防止`Guest`获得对访问权限`Password`字段`Employee`只需通过它强制转换为对象`User`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-367">This prevents `Guest` from gaining access to the `Password` field of an `Employee` object simply by casting it to `User`.</span></span>

<span data-ttu-id="d57fe-368">出于`Protected`成员访问声明上下文中的泛型类型，包括类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-368">For the purposes of `Protected` member access in generic types, the declaration context includes type parameters.</span></span> <span data-ttu-id="d57fe-369">这意味着，使用一组类型参数派生的类型不能访问到`Protected`派生类型的成员与一组不同的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-369">This means that a derived type with one set of type arguments does not have access to the `Protected` members of a derived type with a different set of type arguments.</span></span> <span data-ttu-id="d57fe-370">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-370">For example:</span></span>

```vb
Class Base(Of T)
    Protected x As T
End Class

Class Derived(Of T)
    Inherits Base(Of T)

    Public Sub F(y As Derived(Of String))
        ' Error: Derived(Of T) cannot access Derived(Of String)'s 
        '     protected members
        y.x = "a"
    End Sub
End Class
```

<span data-ttu-id="d57fe-371">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="d57fe-371">__Note.__</span></span> <span data-ttu-id="d57fe-372">C# 语言 （和可能是其他语言） 允许访问的泛型类型`Protected`而不考虑提供类型自变量的成员。</span><span class="sxs-lookup"><span data-stu-id="d57fe-372">The C# language (and possibly other languages) allows a generic type to access `Protected` members regardless of what type arguments are supplied.</span></span> <span data-ttu-id="d57fe-373">在设计包含的泛型类时应记住保留这`Protected`成员。</span><span class="sxs-lookup"><span data-stu-id="d57fe-373">This should be kept in mind when designing generic classes that contain `Protected` members.</span></span>


### <a name="constituent-types"></a><span data-ttu-id="d57fe-374">构成类型</span><span class="sxs-lookup"><span data-stu-id="d57fe-374">Constituent Types</span></span>

<span data-ttu-id="d57fe-375">*构成类型*声明的是由声明引用的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-375">The *constituent types* of a declaration are the types that are referenced by the declaration.</span></span> <span data-ttu-id="d57fe-376">例如，一个常量、 方法的返回类型和构造函数的参数类型的类型是所有构成的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-376">For example, the type of a constant, the return type of a method and the parameter types of a constructor are all constituent types.</span></span> <span data-ttu-id="d57fe-377">声明的构成类型的可访问域必须与相同或声明本身的可访问域的超集。</span><span class="sxs-lookup"><span data-stu-id="d57fe-377">The accessibility domain of a constituent type of a declaration must be the same as or a superset of the accessibility domain of the declaration itself.</span></span> <span data-ttu-id="d57fe-378">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-378">For example:</span></span>

```vb
Public Class X
    Private Class Y
    End Class

    ' Error: Exposing private class Y outside of X.
    Public Function Z() As Y
    End Function

    ' Valid: Not exposing outside of X.
    Private Function A() As Y
    End Function
End Class

Friend Class B
    Private Class C
    End Class

    ' Error: Exposing private class Y outside of B.
    Public Function D() As C
    End Function
End Class
```

## <a name="type-and-namespace-names"></a><span data-ttu-id="d57fe-379">类型和 Namespace 名称</span><span class="sxs-lookup"><span data-stu-id="d57fe-379">Type and Namespace Names</span></span>

<span data-ttu-id="d57fe-380">许多语言构造需要命名空间或类型来指定;可以通过使用命名空间或类型的名称的限定的形式指定这些选项。</span><span class="sxs-lookup"><span data-stu-id="d57fe-380">Many language constructs require a namespace or type to be specified; these can be specified by using a qualified form of the namespace or type's name.</span></span> <span data-ttu-id="d57fe-381">一个*限定的名*组成的标识符的一系列用句点分隔的; 在一段右侧的标识符解析上左侧和右侧的段标识符指定的声明空间中。</span><span class="sxs-lookup"><span data-stu-id="d57fe-381">A *qualified name* consists of a series of identifiers separated by periods; the identifier on the right side of a period is resolved in the declaration space specified by the identifier on the left side of the period.</span></span>

<span data-ttu-id="d57fe-382">*完全限定的名称*的命名空间或类型包含所有的名称的限定的名称包含命名空间和类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-382">The *fully qualified name* of a namespace or type is a qualified name that contains the name of all containing namespaces and types.</span></span> <span data-ttu-id="d57fe-383">换而言之，命名空间或类型的完全限定的名称是`N.T`，其中`T`是实体的名称和`N`是其包含的实体的完全限定的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-383">In other words, the fully qualified name of a namespace or type is `N.T`, where `T` is the name of the entity and `N` is the fully qualified name of its containing entity.</span></span>

<span data-ttu-id="d57fe-384">下面的示例显示在行中的注释以及其关联的完全限定名称的多个命名空间和类型声明。</span><span class="sxs-lookup"><span data-stu-id="d57fe-384">The example below shows several namespace and type declarations together with their associated fully qualified names in in-line comments.</span></span>

```vb
Class A            ' A.
End Class

Namespace X        ' X.
    Class B        ' X.B.
        Class C    ' X.B.C.
        End Class
    End Class

    Namespace Y    ' X.Y.
        Class D    ' X.Y.D.
        End Class
    End Namespace 
End Namespace 

Namespace X.Y      ' X.Y.
    Class E        ' X.Y.E.
    End Class
End Namespace
```

<span data-ttu-id="d57fe-385">观察命名空间 X.Y 已声明在源代码中的两个不同位置，但这两个分部声明构成只需调用 X.Y 包含 D 类和类 E.单个命名空间</span><span class="sxs-lookup"><span data-stu-id="d57fe-385">Observe that the namespace X.Y has been declared in two different locations in the source code, but these two partial declarations constitute just a single namespace called X.Y which contains both class D and class E.</span></span>

<span data-ttu-id="d57fe-386">在某些情况下，限定的名可能会开始使用关键字`Global`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-386">In some situations, a qualified name may begin with the keyword `Global`.</span></span> <span data-ttu-id="d57fe-387">关键字表示未命名的最外层命名空间，在其中声明会覆盖封闭命名空间的情况下很有用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-387">The keyword represents the unnamed outermost namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="d57fe-388">`Global`关键字使"转义"出到这种情况中的最外层命名空间。</span><span class="sxs-lookup"><span data-stu-id="d57fe-388">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="d57fe-389">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-389">For example:</span></span>

```vb
Namespace NS1
    Class System
    End Class

    Module Test
        Sub Main()
            ' Error: Class System does not contain Int32
            Dim x As System.Int32


            ' Legal, binds to System in outermost namespace
            Dim y As Global.System.Int32
        End Sub
    End Module
End Namespace
```

<span data-ttu-id="d57fe-390">在上述示例中，第一个方法调用无效因为标识符`System`绑定到该类`System`，不是命名空间`System`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-390">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="d57fe-391">访问的唯一办法`System`命名空间是使用`Global`来转义扩展到最外层命名空间。</span><span class="sxs-lookup"><span data-stu-id="d57fe-391">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span> <span data-ttu-id="d57fe-392">`Global` 不能用于`Imports`语句或`Namespace`声明。</span><span class="sxs-lookup"><span data-stu-id="d57fe-392">`Global` cannot be used in an `Imports` statement or `Namespace` declaration.</span></span>

<span data-ttu-id="d57fe-393">因为类型和与语言中的关键字匹配的命名空间，可能会造成其他语言，Visual Basic 可以识别的关键字以将其限定名称的一部分，只要它们遵循一段。</span><span class="sxs-lookup"><span data-stu-id="d57fe-393">Because other languages may introduce types and namespaces that match keywords in the language, Visual Basic recognizes keywords to be part of a qualified name as long as they follow a period.</span></span> <span data-ttu-id="d57fe-394">在这种方式中使用的关键字被视为标识符。</span><span class="sxs-lookup"><span data-stu-id="d57fe-394">Keywords used in this way are treated as identifiers.</span></span> <span data-ttu-id="d57fe-395">例如，限定的标识符`X.Default.Class`是一个有效的限定的标识符，而`Default.Class`不是。</span><span class="sxs-lookup"><span data-stu-id="d57fe-395">For example, the qualified identifier `X.Default.Class` is a valid qualified identifier, while `Default.Class` is not.</span></span>

### <a name="qualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="d57fe-396">命名空间和类型的限定的名称解析</span><span class="sxs-lookup"><span data-stu-id="d57fe-396">Qualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="d57fe-397">指定限定命名空间或类型名称的窗体`N.R(Of A)`，其中`R`是限定名称中最右边的标识符和`A`是可选的类型参数列表，以下步骤介绍如何确定哪个命名空间或类型的限定名称指的是：</span><span class="sxs-lookup"><span data-stu-id="d57fe-397">Given a qualified namespace or type name of the form `N.R(Of A)`, where `R` is the rightmost identifier in the qualified name and `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the qualified name refers:</span></span>

1. <span data-ttu-id="d57fe-398">解决`N`，使用任一限定或非限定名称解析规则。</span><span class="sxs-lookup"><span data-stu-id="d57fe-398">Resolve `N`, using the rules for either qualified or unqualified name resolution.</span></span>

2. <span data-ttu-id="d57fe-399">如果解析`N`出现故障，或者解析为类型参数，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-399">If resolution of `N` fails, or resolves to a type parameter, a compile-time error occurs.</span></span>

3. <span data-ttu-id="d57fe-400">否则为如果`R`匹配项提供了 N 且不使用类型参数中的命名空间的名称，或`R`匹配中的可访问类型`N`与作为类型参数相同数量的类型参数，如果有，则限定的名称指该命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-400">Otherwise, if `R` matches the name of a namespace in N and no type arguments were supplied, or `R` matches an accessible type in `N` with the same number of type parameters as type arguments, if any, then the qualified name refers to that namespace or type.</span></span>

4. <span data-ttu-id="d57fe-401">否则为如果`N`包含一个或多个标准模块和`R`如果任何一个标准模块，然后的限定的名中引用匹配的类型参数的数作为类型参数相同的可访问类型的名称类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-401">Otherwise, if `N` contains one or more standard modules, and `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the qualified name refers to that type.</span></span> <span data-ttu-id="d57fe-402">如果`R`与具有相同数量的类型参数作为类型参数的可访问类型的名称相匹配，如果发生任何在多个标准模块，编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-402">If `R` matches the name of accessible types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

5. <span data-ttu-id="d57fe-403">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-403">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="d57fe-404">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="d57fe-404">__Note.__</span></span> <span data-ttu-id="d57fe-405">此解析过程的含义是，类型成员不隐藏命名空间或类型解析命名空间或类型的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-405">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="d57fe-406">命名空间和类型的非限定的名称解析</span><span class="sxs-lookup"><span data-stu-id="d57fe-406">Unqualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="d57fe-407">非限定名称`R(Of A)`，其中`A`是可选的类型参数列表，以下步骤介绍如何确定向哪些命名空间或类型的非限定的名称引用：</span><span class="sxs-lookup"><span data-stu-id="d57fe-407">Given an unqualified name `R(Of A)`, where `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the unqualified name refers:</span></span>

1. <span data-ttu-id="d57fe-408">如果 R 与当前方法的类型参数的名称相匹配，提供没有类型参数的非限定的名称指该类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-408">If R matches the name of a type parameter of the current method, and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>

2.  <span data-ttu-id="d57fe-409">为每个嵌套类型，其中名称引用，从最里面的类型开始，并转到最外面：</span><span class="sxs-lookup"><span data-stu-id="d57fe-409">For each nested type containing the name reference, starting from the innermost type and going to the outermost:</span></span>
    1. <span data-ttu-id="d57fe-410">如果`R`匹配中当前的类型并提供参数，任何类型的类型参数的名称，则该类型参数的非限定的名称引用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-410">If `R` matches the name of a type parameter in the current type and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>
    2. <span data-ttu-id="d57fe-411">否则为如果`R`匹配项的可访问名称嵌套具有相同数量的类型作为类型参数，类型参数，如果有，则该类型的非限定的名称引用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-411">Otherwise, if `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, then the unqualified name refers to that type.</span></span>

3. <span data-ttu-id="d57fe-412">对于每个嵌套的命名空间包含的最内部的命名空间从开始，然后转到最外层命名空间的名称引用：</span><span class="sxs-lookup"><span data-stu-id="d57fe-412">For each nested namespace containing the name reference, starting from the innermost namespace and going to the outermost namespace:</span></span>
    1. <span data-ttu-id="d57fe-413">如果`R`提供当前命名空间和任何类型参数列表中的嵌套命名空间的名称，则非限定的名称引用该嵌套命名空间的匹配项。</span><span class="sxs-lookup"><span data-stu-id="d57fe-413">If `R` matches the name of a nested namespace in the current namespace and no type argument list is supplied, then the unqualified name refers to that nested namespace.</span></span>
    2. <span data-ttu-id="d57fe-414">否则为如果`R`如果任何在当前命名空间，则非限定的名称指该类型的可访问类型作为类型参数的类型参数数量与名称匹配。</span><span class="sxs-lookup"><span data-stu-id="d57fe-414">Otherwise, if `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in the current namespace, then the unqualified name refers to that type.</span></span>
    3. <span data-ttu-id="d57fe-415">否则为如果该命名空间包含一个或多个可访问标准模块，和`R`的可访问嵌套类型作为类型参数的类型参数数量与名称匹配，如果有的话，在一个标准模块则不合格该嵌套类型引用名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-415">Otherwise, if the namespace contains one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that nested type.</span></span> <span data-ttu-id="d57fe-416">如果`R`与具有相同数量的类型参数作为类型参数的可访问嵌套类型的名称相匹配，如果发生任何在多个标准模块，编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-416">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

4. <span data-ttu-id="d57fe-417">如果源文件具有一个或多个导入别名和`R`的其中之一，名称匹配，则非限定的名称引用该导入别名。</span><span class="sxs-lookup"><span data-stu-id="d57fe-417">If the source file has one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="d57fe-418">如果提供类型实参列表，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-418">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="d57fe-419">如果包含名称引用的源文件具有一个或多个导入：</span><span class="sxs-lookup"><span data-stu-id="d57fe-419">If the source file containing the name reference has one or more imports:</span></span>
    1. <span data-ttu-id="d57fe-420">如果`R`匹配任何的恰好一个导入，则以非限定的名称指该类型具有相同数量的类型参数作为类型参数的可访问类型的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-420">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="d57fe-421">如果`R`与具有相同数量的类型参数作为类型参数的可访问类型的名称相匹配，如果所有中的多个导入和所有不是相同的类型，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-421">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import and all are not the same type, a compile-time error occurs.</span></span>
    2. <span data-ttu-id="d57fe-422">否则为如果不提供任何类型参数列表和`R`的非限定的名称指该命名空间，然后与一个导入过程中访问的类型相匹配的命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-422">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="d57fe-423">如果不提供任何类型参数列表和`R`匹配项的多个导入中的可访问类型和所有包含的命名空间名称不是相同的命名空间，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-423">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import and all are not the same namespace, a compile-time error occurs.</span></span>
    3. <span data-ttu-id="d57fe-424">否则为如果导入包含一个或多个可访问标准模块，和`R`匹配具有相同数量的类型参数作为类型参数，如果有一个标准模块中的可访问嵌套类型的名称，然后的非限定的名称引用该类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-424">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="d57fe-425">如果`R`与具有相同数量的类型参数作为类型参数的可访问嵌套类型的名称相匹配，如果发生任何在多个标准模块，编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-425">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

6. <span data-ttu-id="d57fe-426">如果编译环境定义了一个或多个导入别名和`R`的其中之一，名称匹配，则非限定的名称引用该导入别名。</span><span class="sxs-lookup"><span data-stu-id="d57fe-426">If the compilation environment defines one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="d57fe-427">如果提供类型实参列表，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-427">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="d57fe-428">如果编译环境定义了一个或多个导入：</span><span class="sxs-lookup"><span data-stu-id="d57fe-428">If the compilation environment defines one or more imports:</span></span>
    1. <span data-ttu-id="d57fe-429">如果`R`匹配任何的恰好一个导入，则以非限定的名称指该类型具有相同数量的类型参数作为类型参数的可访问类型的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-429">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="d57fe-430">如果`R`与具有相同数量的类型参数作为类型参数的可访问类型的名称相匹配，如果发生任何在多个导入、 编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-430">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import, a compile-time error occurs.</span></span>
    2. <span data-ttu-id="d57fe-431">否则为如果不提供任何类型参数列表和`R`的非限定的名称指该命名空间，然后与一个导入过程中访问的类型相匹配的命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-431">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="d57fe-432">如果不提供任何类型参数列表和`R`出现编译时错误，则与在多个导入中访问的类型匹配的命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-432">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import, a compile-time error occurs.</span></span>
    3. <span data-ttu-id="d57fe-433">否则为如果导入包含一个或多个可访问标准模块，和`R`匹配具有相同数量的类型参数作为类型参数，如果有一个标准模块中的可访问嵌套类型的名称，然后的非限定的名称引用该类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-433">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="d57fe-434">如果`R`与具有相同数量的类型参数作为类型参数的可访问嵌套类型的名称相匹配，如果发生任何在多个标准模块，编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-434">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="d57fe-435">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-435">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="d57fe-436">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="d57fe-436">__Note.__</span></span> <span data-ttu-id="d57fe-437">此解析过程的含义是，类型成员不隐藏命名空间或类型解析命名空间或类型的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-437">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

<span data-ttu-id="d57fe-438">通常情况下，名称可仅出现一次在特定的命名空间中。</span><span class="sxs-lookup"><span data-stu-id="d57fe-438">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="d57fe-439">但是，可以跨多个.NET 程序集声明命名空间，因此很可能有两个程序集在其中定义具有相同的完全限定名称的类型的情况。</span><span class="sxs-lookup"><span data-stu-id="d57fe-439">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="d57fe-440">在这种情况下，在当前组源代码文件中声明的类型优于外部.NET 程序集中声明的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-440">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="d57fe-441">否则为名称不明确，则不能消除名称歧义。</span><span class="sxs-lookup"><span data-stu-id="d57fe-441">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>

## <a name="variables"></a><span data-ttu-id="d57fe-442">变量</span><span class="sxs-lookup"><span data-stu-id="d57fe-442">Variables</span></span>

<span data-ttu-id="d57fe-443">一个*变量*表示的存储位置。</span><span class="sxs-lookup"><span data-stu-id="d57fe-443">A *variable* represents a storage location.</span></span> <span data-ttu-id="d57fe-444">每个变量具有类型，用于确定值可以是存储在变量中。</span><span class="sxs-lookup"><span data-stu-id="d57fe-444">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="d57fe-445">因为 Visual Basic 是类型的类型安全语言、 程序中的每个变量具有类型和语言保证值存储在变量是类型的适当始终。</span><span class="sxs-lookup"><span data-stu-id="d57fe-445">Because Visual Basic is a type-safe language, every variable in a program has a type and the language guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="d57fe-446">始终为它们的类型的默认值初始化变量，变量的任何引用之前。</span><span class="sxs-lookup"><span data-stu-id="d57fe-446">Variables are always initialized to the default value of their type before any reference to the variable can be made.</span></span> <span data-ttu-id="d57fe-447">不能访问未初始化的内存。</span><span class="sxs-lookup"><span data-stu-id="d57fe-447">It is not possible to access uninitialized memory.</span></span>

## <a name="generic-types-and-methods"></a><span data-ttu-id="d57fe-448">泛型类型和方法</span><span class="sxs-lookup"><span data-stu-id="d57fe-448">Generic Types and Methods</span></span>

<span data-ttu-id="d57fe-449">（除标准模块和枚举的类型） 的类型和方法可以声明*类型参数*、 哪些不提供类型的实例之前的类型声明或调用的方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-449">Types (except for standard modules and enumerated types) and methods can declare *type parameters*, which are types that will not be provided until an instance of the type is declared or the method is invoked.</span></span> <span data-ttu-id="d57fe-450">类型和使用类型参数的方法是也称为*泛型类型*并*泛型方法*分别，因为类型或方法必须编写以一般方式，而无需知道特定使用的类型或方法的代码将提供的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-450">Types and methods with type parameters are also known as *generic types* and *generic methods*, respectively, because the type or method must be written generically, without specific knowledge of the types that will be supplied by code that uses the type or method.</span></span>

<span data-ttu-id="d57fe-451">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="d57fe-451">__Note.__</span></span> <span data-ttu-id="d57fe-452">在此期间，即使方法和委托可以是泛型，属性、 事件和运算符不能是泛型本身。</span><span class="sxs-lookup"><span data-stu-id="d57fe-452">At this time, even though methods and delegates can be generic, properties, events and operators cannot be generic themselves.</span></span> <span data-ttu-id="d57fe-453">但是，它们可能，使用自包含类的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-453">They may, however, use type parameters from the containing class.</span></span>

<span data-ttu-id="d57fe-454">从泛型类型或方法的角度来看，类型参数是使用的类型或方法时将使用实际类型填充占位符类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-454">From the perspective of the generic type or method, a type parameter is a placeholder type that will be filled in with an actual type when the type or method is used.</span></span> <span data-ttu-id="d57fe-455">类型实参替换类型或方法，而使用的类型或方法的点中的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-455">Type arguments are substituted for the type parameters in the type or method at the point at which the type or method is used.</span></span> <span data-ttu-id="d57fe-456">例如，一个泛型堆栈类就可以实现为：</span><span class="sxs-lookup"><span data-stu-id="d57fe-456">For example, a generic stack class could be implemented as:</span></span>

```vb
Public Class Stack(Of ItemType)
    Protected Items(0 To 99) As ItemType
    Protected CurrentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        If CurrentIndex = 100 Then
            Throw New ArgumentException("Stack is full.")
        End If

        Items(CurrentIndex) = Data
        CurrentIndex += 1
    End Sub

    Public Function Pop() As ItemType
        If CurrentIndex = 0 Then
            Throw New ArgumentException("Stack is empty.")
        End If

        CurrentIndex -= 1
        Return Items(CurrentIndex + 1) 
    End Function
End Class
```

<span data-ttu-id="d57fe-457">使用的声明`Stack(Of ItemType)`类必须提供类型参数的类型参数`ItemType`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-457">Declarations that use the `Stack(Of ItemType)` class must supply a type argument for the type parameter `ItemType`.</span></span> <span data-ttu-id="d57fe-458">此类型然后填写无论在何处`ItemType`类中使用：</span><span class="sxs-lookup"><span data-stu-id="d57fe-458">This type is then filled in wherever `ItemType` is used within the class:</span></span>

```vb
Option Strict On

Module Test
    Sub Main()
        Dim s1 As New Stack(Of Integer)()
        Dim s2 As New Stack(Of Double)()

        s1.Push(10.10)   ' Error: Stack(Of Integer).Push takes an Integer
        s2.Push(10.10)   ' OK: Stack(Of Double).Push takes a Double
        Console.WriteLine(s2.Pop().GetType().ToString()) ' Prints: Double
    End Sub
End Module
```

### <a name="type-parameters"></a><span data-ttu-id="d57fe-459">类型参数</span><span class="sxs-lookup"><span data-stu-id="d57fe-459">Type Parameters</span></span>

<span data-ttu-id="d57fe-460">在类型或方法声明上可能会提供类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-460">Type parameters may be supplied on type or method declarations.</span></span> <span data-ttu-id="d57fe-461">每个类型参数是提供创建构造的类型或方法的类型参数的占位符的标识符。</span><span class="sxs-lookup"><span data-stu-id="d57fe-461">Each type parameter is an identifier which is a place-holder for a type argument that is supplied to create a constructed type or method.</span></span> <span data-ttu-id="d57fe-462">与此相反，类型参数是泛型类型或方法使用时替换为类型参数的实际类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-462">By contrast, a type argument is the actual type that is substituted for the type parameter when a generic type or method is used.</span></span>

```antlr
TypeParameterList
    : OpenParenthesis 'Of' TypeParameter ( Comma TypeParameter )* CloseParenthesis
    ;

TypeParameter
    : VarianceModifier? Identifier TypeParameterConstraints?
    ;

VarianceModifier
    : 'In' | 'Out'
    ;
```

<span data-ttu-id="d57fe-463">在类型或方法声明中的每个类型参数定义该类型或方法的声明空间中的名称。</span><span class="sxs-lookup"><span data-stu-id="d57fe-463">Each type parameter in a type or method declaration defines a name in the declaration space of that type or method.</span></span> <span data-ttu-id="d57fe-464">因此，它不能具有相同名称作为另一个类型参数、 类型成员、 方法参数或局部变量。</span><span class="sxs-lookup"><span data-stu-id="d57fe-464">Thus, it cannot have the same name as another type parameter, a type member, a method parameter, or a local variable.</span></span> <span data-ttu-id="d57fe-465">上一个类型或方法的类型参数的作用域是整个类型或方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-465">The scope of a type parameter on a type or method is the entire type or method.</span></span> <span data-ttu-id="d57fe-466">由于类型参数的作用域为整个类型声明，嵌套的类型可以使用外部类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-466">Because type parameters are scoped to the entire type declaration, nested types can use outer type parameters.</span></span> <span data-ttu-id="d57fe-467">这也意味着访问类型嵌套在泛型类型时，必须始终指定类型参数：</span><span class="sxs-lookup"><span data-stu-id="d57fe-467">This also means that type parameters must always be specified when accessing types nested inside generic types:</span></span>

```vb
Public Class Outer(Of T)
    Public Class Inner
        Public Sub F(x As T)
            ...
        End Sub
    End Class
End Class

Module Test
    Sub Main()
        Dim x As New Outer(Of Integer).Inner()
        ...
    End Sub
End Module
```

<span data-ttu-id="d57fe-468">与不同的类的其他成员，不会继承类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-468">Unlike other members of a class, type parameters are not inherited.</span></span> <span data-ttu-id="d57fe-469">类型中的类型参数仅对由它们的简单名称; 引用换而言之，它们不能为与包含类型名称限定。</span><span class="sxs-lookup"><span data-stu-id="d57fe-469">Type parameters in a type can only be referred to by their simple name; in other words, they cannot be qualified with the containing type name.</span></span> <span data-ttu-id="d57fe-470">尽管错误编程风格，嵌套类型中的类型参数可以隐藏成员或类型参数中的外部类型声明：</span><span class="sxs-lookup"><span data-stu-id="d57fe-470">Although it is bad programming style, the type parameters in a nested type can hide a member or type parameter declared in the outer type:</span></span>

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

<span data-ttu-id="d57fe-471">类型和方法可以重载基于类型参数的数目 (或*arity*) 类型或方法声明。</span><span class="sxs-lookup"><span data-stu-id="d57fe-471">Types and methods may be overloaded based on the number of type parameters (or *arity*) that the types or methods declare.</span></span> <span data-ttu-id="d57fe-472">例如，以下声明是合法的：</span><span class="sxs-lookup"><span data-stu-id="d57fe-472">For example, the following declarations are legal:</span></span>

```vb
Module C
    Sub M()
    End Sub

    Sub M(Of T)()
    End Sub

    Sub M(Of T, U)()
    End Sub
End Module

Structure C(Of T)
    Dim x As T
End Structure

Class C(Of T, U)
End Class
```

<span data-ttu-id="d57fe-473">对于类型，将始终针对指定的类型参数的数目匹配重载。</span><span class="sxs-lookup"><span data-stu-id="d57fe-473">In the case of types, overloads are always matched against the number of type arguments specified.</span></span> <span data-ttu-id="d57fe-474">在同一个程序一起使用泛型和非泛型类时，这非常有用：</span><span class="sxs-lookup"><span data-stu-id="d57fe-474">This is useful when using both generic and non-generic classes together in the same program:</span></span>

```vb
Class Queue 
End Class      

Class Queue(Of T)
End Class

Class X
    Dim q1 As Queue                 ' Non-generic queue
    Dim q2 As Queue(Of Integer)     ' Generic queue
End Class
```

<span data-ttu-id="d57fe-475">类型参数上重载的方法的规则的方法重载解析部分所述。</span><span class="sxs-lookup"><span data-stu-id="d57fe-475">Rules for methods overloaded on type parameters are covered in the section on method overload resolution.</span></span>

<span data-ttu-id="d57fe-476">在包含的声明中，类型参数被视为完整的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-476">Within the containing declaration, type parameters are considered full types.</span></span> <span data-ttu-id="d57fe-477">由于类型参数可以具有多个不同的实际类型参数实例化，类型参数具有与其他类型，如下所述略有不同的操作和限制：</span><span class="sxs-lookup"><span data-stu-id="d57fe-477">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types as described below:</span></span>

* <span data-ttu-id="d57fe-478">类型参数不能用于直接声明基类或接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-478">A type parameter cannot be used directly to declare a base class or interface.</span></span>

* <span data-ttu-id="d57fe-479">参数取决于该约束，如果有，成员查找类型上的规则应用于类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-479">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span>

* <span data-ttu-id="d57fe-480">可用的转换为类型参数取决于该约束，如果有的话，应用于类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-480">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameters.</span></span>

* <span data-ttu-id="d57fe-481">如果没有`Structure`约束，由类型参数表示的类型值可以与比较`Nothing`使用`Is`和`IsNot`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-481">In the absence of a `Structure` constraint, a value with a type represented by a type parameter can be compared with `Nothing` using `Is` and `IsNot`.</span></span>

* <span data-ttu-id="d57fe-482">类型参数仅可在`New`表达式，如果类型参数受到`New`或`Structure`约束。</span><span class="sxs-lookup"><span data-stu-id="d57fe-482">A type parameter can only be used in a `New` expression if the type parameter is constrained by a `New` or a `Structure` constraint.</span></span>

* <span data-ttu-id="d57fe-483">类型参数无法在任何位置中使用属性异常内`GetType`表达式。</span><span class="sxs-lookup"><span data-stu-id="d57fe-483">A type parameter cannot be used anywhere within an attribute exception within a `GetType` expression.</span></span>

* <span data-ttu-id="d57fe-484">类型参数可以用作其他泛型类型和参数的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-484">Type parameters can be used as type arguments to other generic types and parameters.</span></span>

<span data-ttu-id="d57fe-485">下面的示例是泛型类型扩展`Stack(Of ItemType)`类：</span><span class="sxs-lookup"><span data-stu-id="d57fe-485">The following example is a generic type that extends the `Stack(Of ItemType)` class:</span></span>

```vb
Class MyStack(Of ItemType)
    Inherits Stack(Of ItemType)

    Public ReadOnly Property Size() As Integer
        Get
            Return CurrentIndex
        End Get
    End Property
End Class
```

<span data-ttu-id="d57fe-486">在声明时提供的类型参数`MyStack`，相同的类型参数将应用于`Stack`也。</span><span class="sxs-lookup"><span data-stu-id="d57fe-486">When a declaration supplies a type argument to `MyStack`, the same type argument will be applied to `Stack` as well.</span></span>

<span data-ttu-id="d57fe-487">作为一种类型，类型参数都是纯粹是一个编译时构造。</span><span class="sxs-lookup"><span data-stu-id="d57fe-487">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="d57fe-488">在运行时，每个类型参数绑定到通过提供泛型声明的类型参数指定的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-488">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic declaration.</span></span> <span data-ttu-id="d57fe-489">因此，使用类型参数声明的变量的类型，在运行时，将非泛型类型或特定的构造的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-489">Thus, the type of a variable declared with a type parameter will, at run-time, be a non-generic type or a specific constructed type.</span></span> <span data-ttu-id="d57fe-490">运行时执行的所有语句和包含类型参数的表达式使用提供的实际类型作为该参数的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-490">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>


### <a name="type-constraints"></a><span data-ttu-id="d57fe-491">类型约束</span><span class="sxs-lookup"><span data-stu-id="d57fe-491">Type Constraints</span></span>

<span data-ttu-id="d57fe-492">由于类型系统中，类型参数可以是任何类型，泛型类型或方法不能进行任何假设类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-492">Because a type argument can be any type in the type system, a generic type or method cannot make any assumptions about a type parameter.</span></span> <span data-ttu-id="d57fe-493">因此，类型参数的成员都被认为是该类型的成员`Object`，因为所有类型都派生`Object`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-493">Thus, the members of a type parameter are considered to be the members of the type `Object`, since all types derive from `Object`.</span></span>

<span data-ttu-id="d57fe-494">在类似集合的情况下`Stack(Of ItemType)`，这一事实可能不是一个特别重要的限制，但可能有可能希望进行假设将作为类型自变量提供的类型的泛型类型的情况。</span><span class="sxs-lookup"><span data-stu-id="d57fe-494">In the case of a collection like `Stack(Of ItemType)`, this fact may not be a particularly important restriction, but there may be cases where a generic type may wish to make an assumption about the types that will be supplied as type arguments.</span></span> <span data-ttu-id="d57fe-495">*类型约束*可以施加限制哪些类型可以作为类型参数提供，并允许泛型类型或方法假设类型参数有关的详细信息的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-495">*Type constraints* can be placed on type parameters that restrict which types can be supplied as a type parameter and allow generic types or methods to assume more about type parameters.</span></span>

```antlr
TypeParameterConstraints
    : 'As' Constraint
    | 'As' OpenCurlyBrace ConstraintList CloseCurlyBrace
    ;

ConstraintList
    : Constraint ( Comma Constraint )*
    ;

Constraint
    : TypeName
    | 'New'
    | 'Structure'
    | 'Class'
    ;
```


```vb
Public Class DisposableStack(Of ItemType As IDisposable)
    Implements IDisposable

    Private _items(0 To 99) As ItemType
    Private _currentIndex As Integer = 0

    Public Sub Push(data As ItemType)
        ...
    End Sub

    Public Function Pop() As ItemType
        ...
    End Function

    Private Sub Dispose() Implements IDisposable.Dispose
        For Each item As IDisposable In _items
            If item IsNot Nothing Then
                item.Dispose()
            End If
        Next item
    End Sub
End Class
```

<span data-ttu-id="d57fe-496">在此示例中，`DisposableStack(Of ItemType)`其类型参数限制为实现接口的类型`System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-496">In this example, the `DisposableStack(Of ItemType)` constrains its type parameter to only types that implement the interface `System.IDisposable`.</span></span> <span data-ttu-id="d57fe-497">因此，它可以实现`Dispose`仍保留在队列中释放的任何对象的方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-497">As a result, it can implement a `Dispose` method that disposes any objects still left in the queue.</span></span>

<span data-ttu-id="d57fe-498">类型约束必须是一个特殊约束`Class`， `Structure`，或`New`，或者它必须是类型`T`其中：</span><span class="sxs-lookup"><span data-stu-id="d57fe-498">A type constraint must be one of the special constraints `Class`, `Structure`, or `New`, or it must be a type `T` where:</span></span>

* <span data-ttu-id="d57fe-499">`T` 是一个类、 接口或类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-499">`T` is a class, an interface, or a type parameter.</span></span>

* <span data-ttu-id="d57fe-500">`T` 不是 `NotInheritable`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-500">`T` is not `NotInheritable`.</span></span>

* <span data-ttu-id="d57fe-501">`T` 不是一个，或从一个以下特殊类型的继承类型： `System.Array`， `System.Delegate`， `System.MulticastDelegate`， `System.Enum`，或`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-501">`T` is not one of, or a type inherited from one of, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span>

* <span data-ttu-id="d57fe-502">`T` 不是 `Object`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-502">`T` is not `Object`.</span></span> <span data-ttu-id="d57fe-503">由于所有类型都派生`Object`，如果已允许此类约束会产生任何影响。</span><span class="sxs-lookup"><span data-stu-id="d57fe-503">Since all types derive from `Object`, such a constraint would have no effect if it were permitted.</span></span>

* <span data-ttu-id="d57fe-504">`T` 必须至少与可访问性的泛型类型或所声明的方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-504">`T` must be at least as accessible as the generic type or method being declared.</span></span>

<span data-ttu-id="d57fe-505">可以为单个类型参数括在大括号中的类型约束指定多个类型约束 (`{}`)...</span><span class="sxs-lookup"><span data-stu-id="d57fe-505">Multiple type constraints can be specified for a single type parameter by enclosing the type constraints in curly braces (`{}`)..</span></span> <span data-ttu-id="d57fe-506">只有一种类型约束为给定的类型参数可以是类。</span><span class="sxs-lookup"><span data-stu-id="d57fe-506">Only one type constraint for a given type parameter can be a class.</span></span> <span data-ttu-id="d57fe-507">它是错误合并`Structure`命名的类约束的特殊约束或`Class`特殊约束。</span><span class="sxs-lookup"><span data-stu-id="d57fe-507">It is an error to combine a `Structure` special constraint with a named class constraint or the `Class` special constraint.</span></span>

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

<span data-ttu-id="d57fe-508">包含类型或任何包含类型的类型参数，可以使用类型约束。</span><span class="sxs-lookup"><span data-stu-id="d57fe-508">Type constraints can use the containing types or any of the containing types' type parameters.</span></span> <span data-ttu-id="d57fe-509">在以下示例中，该约束要求提供的类型实参实现泛型接口使用自身作为类型参数：</span><span class="sxs-lookup"><span data-stu-id="d57fe-509">In the following example, the constraint requires that the type argument supplied implements a generic interface using itself as a type argument:</span></span>

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

<span data-ttu-id="d57fe-510">特殊类型约束`Class`约束任何引用类型的提供的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-510">The special type constraint `Class` constrains the supplied type argument to any reference type.</span></span>

<span data-ttu-id="d57fe-511">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="d57fe-511">__Note.__</span></span> <span data-ttu-id="d57fe-512">特殊类型约束`Class`可以满足的接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-512">The special type constraint `Class` can be satisfied by an interface.</span></span> <span data-ttu-id="d57fe-513">和结构可以实现接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-513">And a structure can implement an interface.</span></span> <span data-ttu-id="d57fe-514">因此，该约束`(Of T As U, U As Class)`可能是"T"结构感到满意 (这不满足`Class`特殊约束)，和"U"它实现的接口 (其满足`Class`特殊约束)。</span><span class="sxs-lookup"><span data-stu-id="d57fe-514">Therefore, the constraint `(Of T As U, U As Class)` might be satisfied with "T" a structure (which does not satisfy the `Class` special constraint), and "U" an interface that it implements (which does satisfy the `Class` special constraint).</span></span>

<span data-ttu-id="d57fe-515">特殊类型约束`Structure`以外的任意值类型的提供的类型参数约束，使之`System.Nullable(Of T)`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-515">The special type constraint `Structure` constrains the supplied type argument to any value type except `System.Nullable(Of T)`.</span></span>

<span data-ttu-id="d57fe-516">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="d57fe-516">__Note.__</span></span> <span data-ttu-id="d57fe-517">不允许结构约束`System.Nullable(Of T)`，以便不能提供`System.Nullable(Of T)`用作到其自身的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-517">Structure constraints do not allow `System.Nullable(Of T)` so that it is not possible to supply `System.Nullable(Of T)` as a type argument to itself.</span></span>

<span data-ttu-id="d57fe-518">特殊类型约束`New`要求提供的类型参数必须具有可访问的无参数构造函数并不能声明为`MustInherit`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-518">The special type constraint `New` requires that the supplied type argument must have an accessible parameterless constructor and cannot be declared `MustInherit`.</span></span> <span data-ttu-id="d57fe-519">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-519">For example:</span></span>

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

<span data-ttu-id="d57fe-520">类类型约束要求提供的类型参数必须为类型或从其继承。</span><span class="sxs-lookup"><span data-stu-id="d57fe-520">A class type constraint requires that the supplied type argument must either be that type as or inherit from it.</span></span> <span data-ttu-id="d57fe-521">接口类型约束要求提供的类型实参必须实现该接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-521">An interface type constraint requires that the supplied type argument must implement that interface.</span></span> <span data-ttu-id="d57fe-522">类型参数约束要求提供的类型参数必须派生自或实现所有参数指定的匹配的类型参数的边界。</span><span class="sxs-lookup"><span data-stu-id="d57fe-522">A type parameter constraint requires that the supplied type argument must derive from or implement all of the bounds given for the matching type parameter.</span></span> <span data-ttu-id="d57fe-523">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-523">For example:</span></span>

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

<span data-ttu-id="d57fe-524">在此示例中，类型参数`S`上`AddRange`为类型参数约束`T`的`List`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-524">In this example, the type parameter `S` on `AddRange` is constrained to the type parameter `T` of `List`.</span></span> <span data-ttu-id="d57fe-525">这意味着`List(Of Control)`会约束`AddRange`的类型或继承的任何类型的参数`Control`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-525">This means that a `List(Of Control)` would constrain `AddRange`'s type parameter to any type that is or inherits from `Control`.</span></span>

<span data-ttu-id="d57fe-526">类型参数约束`Of S As T`通过它传递性地添加的所有已解决`T`的约束到`S`，而非特殊约束 (`Class`， `Structure`， `New`)。</span><span class="sxs-lookup"><span data-stu-id="d57fe-526">A type parameter constraint `Of S As T` is resolved by transitively adding all of `T`'s constraints onto `S`, other than the special constraints (`Class`, `Structure`, `New`).</span></span> <span data-ttu-id="d57fe-527">它是错误的循环约束 (例如`Of S As T, T As S`)。</span><span class="sxs-lookup"><span data-stu-id="d57fe-527">It is an error to have circular constraints (e.g. `Of S As T, T As S`).</span></span> <span data-ttu-id="d57fe-528">它是其本身具有错误的类型参数约束`Structure`约束。</span><span class="sxs-lookup"><span data-stu-id="d57fe-528">It is an error to have a type parameter constraint which itself has the `Structure` constraint.</span></span> <span data-ttu-id="d57fe-529">添加约束之后, 就可以很多特殊情况下的，可能会发生：</span><span class="sxs-lookup"><span data-stu-id="d57fe-529">After adding constraints, it is possible that a number of special situations may occur:</span></span>

* <span data-ttu-id="d57fe-530">如果存在多个类约束，派生程度最高的类被视为可约束。</span><span class="sxs-lookup"><span data-stu-id="d57fe-530">If multiple class constraints exist, the most derived class is considered to be the constraint.</span></span> <span data-ttu-id="d57fe-531">如果一个或多个类约束没有任何继承关系，该约束无法满足，它是错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-531">If one or more class constraints have no inheritance relationship, the constraint is unsatisfiable and it is an error.</span></span>

 * <span data-ttu-id="d57fe-532">如果类型参数结合`Structure`命名的类约束的特殊约束或`Class`特殊约束，则返回错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-532">If a type parameter combines a `Structure` special constraint with a named class constraint or the `Class` special constraint, it is an error.</span></span> <span data-ttu-id="d57fe-533">可能是类约束`NotInheritable`、 在这种情况下接受该约束的任何派生的类型和，则是错误。</span><span class="sxs-lookup"><span data-stu-id="d57fe-533">A class constraint may be `NotInheritable`, in which case no derived types of that constraint are accepted and it is an error.</span></span>

<span data-ttu-id="d57fe-534">类型可能是一种，或从以下特殊类型继承的一种类型： `System.Array`， `System.Delegate`， `System.MulticastDelegate`， `System.Enum`，或`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-534">The type may be one of, or a type inherited from, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="d57fe-535">在这种情况下，接受仅的类型或由其继承的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-535">In that case, only the type, or a type inherited from it, is accepted.</span></span> <span data-ttu-id="d57fe-536">类型参数约束为其中一种类型只能使用允许的转换`DirectCast`运算符。</span><span class="sxs-lookup"><span data-stu-id="d57fe-536">A type parameter constrained to one of these types can only use the conversions allowed by the `DirectCast` operator.</span></span> <span data-ttu-id="d57fe-537">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-537">For example:</span></span>

```vb
MustInherit Class Base(Of T)
    MustOverride Sub S1(Of U As T)(x As U)
End Class

Class Derived
    Inherits Base(Of Integer)

    ' The constraint of U must be Integer, which is normally not allowed.
    Overrides Sub S1(Of U As Integer)(x As U)
        Dim y As Integer = x    ' OK
        Dim z As Long = x       ' Error: Can't convert
    End Sub
End Class
```

<span data-ttu-id="d57fe-538">此外，类型参数约束为值类型，因为上述松弛法之一不能调用该值类型上定义任何方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-538">Additionally, a type parameter constrained to a value type due to one of the above relaxations cannot call any methods defined on that value type.</span></span> <span data-ttu-id="d57fe-539">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-539">For example:</span></span>

```vb
Class C1(Of T)
    Overridable Sub F(Of G As T)(x As G)
    End Sub
End Class

Class C2
    Inherits C1(Of IntPtr)

    Overrides Sub F(Of G As IntPtr)(ByVal x As G)
        ' Error: Cannot access structure members
         x.ToInt32()
    End Sub
End Class
```

<span data-ttu-id="d57fe-540">如果该约束后替换，最终为数组类型，也可以是任何协变的数组类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-540">If the constraint, after substitution, ends up as an array type, any covariant array type is allowed as well.</span></span> <span data-ttu-id="d57fe-541">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-541">For example:</span></span>

```vb
Module Test
    Class B
    End Class

    Class D
        Inherits B
    End Class

    Function F(Of T, U As T)(x As U) As T
        Return x
    End Function

    Sub Main()
        Dim a(9) As B
        Dim b(9) As D

        a = F(Of B(), D())(b)
    End Sub
End Module
```

<span data-ttu-id="d57fe-542">类或接口约束的类型参数被视为具有相同的成员的类或接口约束。</span><span class="sxs-lookup"><span data-stu-id="d57fe-542">A type parameter with a class or interface constraint is considered to have the same members as that class or interface constraint.</span></span> <span data-ttu-id="d57fe-543">如果类型参数有多个约束，然后将类型参数视为具有约束的所有成员的并集。</span><span class="sxs-lookup"><span data-stu-id="d57fe-543">If a type parameter has multiple constraints, then the type parameter is considered to have the union of all the members of the constraints.</span></span> <span data-ttu-id="d57fe-544">如果有多个约束，一个同名的成员，则成员隐藏按以下顺序： 类约束隐藏接口约束，隐藏中的成员中的成员`System.ValueType`(如果`Structure`指定约束)，这将隐藏中的成员`Object`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-544">If there are members with the same name in more than one constraint, then members are hidden in the following order: the class constraint hides members in interface constraints, which hide members in `System.ValueType` (if `Structure` constraint is specified), which hides members in `Object`.</span></span> <span data-ttu-id="d57fe-545">如果具有相同名称的成员显示在多个接口约束该成员是不可用 （如在多个接口继承） 和类型参数必须强制转换为所需的接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-545">If a member with the same name appears in more than one interface constraint the member is unavailable (as in multiple interface inheritance) and the type parameter must be cast to the desired interface.</span></span> <span data-ttu-id="d57fe-546">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-546">For example:</span></span>

```vb
Class C1
    Sub S1(x As Integer)
    End Sub
End Class

Interface I1
    Sub S1(x As Integer)
End Interface

Interface I2
    Sub S1(y As Double)
End Interface

Module Test
    Sub T1(Of T As {C1, I1, I2})()
        Dim a As T
        a.S1(10)       ' Calls C1.S1, which is preferred
        a.S1(10.10)    ' Also calls C1.S1, class is still preferred
    End Sub

    Sub T2(Of T As {I1, I2})()
        Dim a As T
        a.S1(10)    ' Error: Call is ambiguous between I1.S1, I2.S1
    End Sub
End Module
```

<span data-ttu-id="d57fe-547">提供类型参数作为类型参数时, 的类型参数必须满足的约束匹配的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-547">When supplying type parameters as type arguments, the type parameters must satisfy the constraints of the matching type parameters.</span></span>

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

<span data-ttu-id="d57fe-548">受约束的类型参数的值可以用于访问实例成员，包括约束中指定的实例方法。</span><span class="sxs-lookup"><span data-stu-id="d57fe-548">Values of a constrained type parameter can be used to access the instance members, including instance methods, specified in the constraint.</span></span>

```vb
Interface IPrintable
    Sub Print()
End Interface

Class Printer(Of V As IPrintable)
    Sub PrintOne(v1 As V)
        V1.Print()
    End Sub
End Class
```

### <a name="type-parameter-variance"></a><span data-ttu-id="d57fe-549">类型参数的变体</span><span class="sxs-lookup"><span data-stu-id="d57fe-549">Type Parameter Variance</span></span>

<span data-ttu-id="d57fe-550">可以选择指定的接口或委托类型声明中的类型参数*方差修饰符*。</span><span class="sxs-lookup"><span data-stu-id="d57fe-550">A type parameter in an interface or a delegate type declaration can optionally specify a *variance modifier*.</span></span> <span data-ttu-id="d57fe-551">方差修饰符的类型参数限制如何类型参数可在接口或委托类型，但允许泛型接口或委托类型转换为另一个具有 variant 兼容类型参数的泛型类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-551">Type parameters with variance modifiers restrict how the type parameter can be used in the interface or delegate type but allow a generic interface or delegate type to be converted to another generic type with variant compatible type arguments.</span></span> <span data-ttu-id="d57fe-552">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-552">For example:</span></span>

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class

Module Test
    Sub Main()
        Dim x As IEnumerable(Of Derived) = ...

        ' OK, as IEnumerable(Of Base) is variant compatible
        ' with IEnumerable(Of Derived)
        Dim y As IEnumerable(Of Base) = x
    End Sub
End Module
```

<span data-ttu-id="d57fe-553">具有与方差修饰符的类型参数的泛型接口具有多个限制：</span><span class="sxs-lookup"><span data-stu-id="d57fe-553">Generic interfaces that have type parameters with variance modifiers have several restrictions:</span></span>

* <span data-ttu-id="d57fe-554">它们不能包含指定的参数列表的事件声明 （但允许使用自定义事件或事件声明委托类型）。</span><span class="sxs-lookup"><span data-stu-id="d57fe-554">They cannot contain an event declaration that specifies a parameter list (but a custom event declaration or an event declaration with a delegate type is allowed).</span></span>

* <span data-ttu-id="d57fe-555">它们不能包含嵌套的类、 结构或枚举的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-555">They cannot contain a nested class, structure, or enumerated type.</span></span>

<span data-ttu-id="d57fe-556">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="d57fe-556">__Note.__</span></span> <span data-ttu-id="d57fe-557">这些限制是因为，类型隐式嵌套在泛型类型中复制其父级的泛型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-557">These restrictions are due to the fact that types nested in generic types implicitly copy the generic parameters of their parent.</span></span> <span data-ttu-id="d57fe-558">对于嵌套的类、 结构或枚举的类型，这些类型的类型不能具有类型参数上的方差修饰符。</span><span class="sxs-lookup"><span data-stu-id="d57fe-558">In the case of nested classes, structures, or enumerated types, those kinds of types cannot have variance modifiers on their type parameters.</span></span> <span data-ttu-id="d57fe-559">对于使用参数列表的事件声明，生成嵌套的委托类可以具有混淆时将显示要在中使用的类型的错误`In`位置 （例如参数类型） 实际上在使用`Out`位置 (即事件类型）。</span><span class="sxs-lookup"><span data-stu-id="d57fe-559">In the case of an event declaration with a parameter list, the generated nested delegate class could have confusing errors when a type that appears to be used in an `In` position (i.e. a parameter type) is actually used in an `Out` position (i.e. the type of the event).</span></span>

<span data-ttu-id="d57fe-560">使用 Out 修饰符声明的类型参数是*协变*。</span><span class="sxs-lookup"><span data-stu-id="d57fe-560">A type parameter that is declared with the Out modifier is *covariant*.</span></span> <span data-ttu-id="d57fe-561">通俗地说，协变类型参数仅可用于在输出位置-即从接口或委托类型返回值--，并且不能在输入位置使用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-561">Informally, a covariant type parameter can only be used in an output position -- i.e. a value that is being returned from the interface or delegate type -- and cannot be used in an input position.</span></span> <span data-ttu-id="d57fe-562">一种类型`T`被视为*有效 covariantly*如果：</span><span class="sxs-lookup"><span data-stu-id="d57fe-562">A type `T` is considered to be *valid covariantly* if:</span></span>

* <span data-ttu-id="d57fe-563">`T` 是类、 结构或枚举的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-563">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="d57fe-564">`T` 为非泛型委托或接口类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-564">`T` is non-generic delegate or interface type.</span></span>

* <span data-ttu-id="d57fe-565">`T` 为数组类型的元素类型是 covariantly 有效。</span><span class="sxs-lookup"><span data-stu-id="d57fe-565">`T` is an array type whose element type is valid covariantly.</span></span>

* <span data-ttu-id="d57fe-566">`T` 是一个类型参数的未声明为`Out`类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-566">`T` is a type parameter which was not declared as an `Out` type parameter.</span></span>

* <span data-ttu-id="d57fe-567">`T` 为构造的接口或委托类型`X(Of P1,...,Pn)`具有类型参数`A1,...,An`以便：</span><span class="sxs-lookup"><span data-stu-id="d57fe-567">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="d57fe-568">如果`Pi`然后声明为 Out 类型参数`Ai`covariantly 是否有效。</span><span class="sxs-lookup"><span data-stu-id="d57fe-568">If `Pi` was declared as an Out type parameter then `Ai` is valid covariantly.</span></span>

  * <span data-ttu-id="d57fe-569">如果`Pi`然后声明为 In 类型参数`Ai`是有效的逆变方式起作用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-569">If `Pi` was declared as an In type parameter then `Ai` is valid contravariantly.</span></span>

<span data-ttu-id="d57fe-570">以下中必须是有效 covariantly 接口或委托类型：</span><span class="sxs-lookup"><span data-stu-id="d57fe-570">The following must be valid covariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="d57fe-571">接口的基接口。</span><span class="sxs-lookup"><span data-stu-id="d57fe-571">The base interface of an interface.</span></span>

* <span data-ttu-id="d57fe-572">一个函数的返回类型或委托类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-572">The return type of a function or the delegate type.</span></span>

* <span data-ttu-id="d57fe-573">如果没有属性的类型`Get`访问器。</span><span class="sxs-lookup"><span data-stu-id="d57fe-573">The type of a property if there is a `Get` accessor.</span></span>

* <span data-ttu-id="d57fe-574">类型为 any`ByRef`参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-574">The type of any `ByRef` parameter.</span></span>

<span data-ttu-id="d57fe-575">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-575">For example:</span></span>

```vb
Delegate Function D(Of Out T, U)(x As U) As T

Interface I1(Of Out T)
End Interface

Interface I2(Of Out T)
    Inherits I1(Of T)

    ' OK, T is only used in an Out position
    Function M1(x As I1(Of T)) As T

    ' Error: T is used in an In position
    Function M2(x As T) As T
End Interface
```

<span data-ttu-id="d57fe-576">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="d57fe-576">__Note.__</span></span> <span data-ttu-id="d57fe-577">`Out` 不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="d57fe-577">`Out` is not a reserved word.</span></span>

<span data-ttu-id="d57fe-578">使用 In 修饰符声明的类型参数是*逆变*。</span><span class="sxs-lookup"><span data-stu-id="d57fe-578">A type parameter that is declared with the In modifier is *contravariant*.</span></span> <span data-ttu-id="d57fe-579">通俗地说，逆变类型参数仅可用于在输入的位置-即传递给接口或委托类型值--，并且不能在输出位置使用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-579">Informally, a contravariant type parameter can only be used in an input position -- i.e. a value that is being passed in to the interface or delegate type -- and cannot be used in an output position.</span></span> <span data-ttu-id="d57fe-580">一种类型`T`被视为*有效逆变方式起作用*如果：</span><span class="sxs-lookup"><span data-stu-id="d57fe-580">A type `T` is considered to be *valid contravariantly* if:</span></span>

* <span data-ttu-id="d57fe-581">`T` 是类、 结构或枚举的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-581">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="d57fe-582">`T` 为非泛型委托或接口类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-582">`T` is a non-generic delegate or interface type.</span></span>

* <span data-ttu-id="d57fe-583">`T` 为数组类型的元素类型是有效的逆变方式起作用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-583">`T` is an array type whose element type is valid contravariantly.</span></span>

* <span data-ttu-id="d57fe-584">`T` 是未声明为 In 类型参数的类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-584">`T` is a type parameter which was not declared as an In type parameter.</span></span>

* <span data-ttu-id="d57fe-585">`T` 为构造的接口或委托类型`X(Of P1,...,Pn)`具有类型参数`A1,...,An`以便：</span><span class="sxs-lookup"><span data-stu-id="d57fe-585">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="d57fe-586">如果`Pi`被声明为`Out`然后键入参数`Ai`是有效的逆变方式起作用。</span><span class="sxs-lookup"><span data-stu-id="d57fe-586">If `Pi` was declared as an `Out` type parameter then `Ai` is valid contravariantly.</span></span>

  * <span data-ttu-id="d57fe-587">如果`Pi`被声明为`In`然后键入参数`Ai`covariantly 是否有效。</span><span class="sxs-lookup"><span data-stu-id="d57fe-587">If `Pi` was declared as an `In` type parameter then `Ai` is valid covariantly.</span></span>

<span data-ttu-id="d57fe-588">下列情况必须为有效逆变方式起作用的接口或委托类型中：</span><span class="sxs-lookup"><span data-stu-id="d57fe-588">The following must be valid contravariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="d57fe-589">参数的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-589">The type of a parameter.</span></span>

* <span data-ttu-id="d57fe-590">方法类型形参的类型约束。</span><span class="sxs-lookup"><span data-stu-id="d57fe-590">A type constraint on a method type parameter.</span></span>

* <span data-ttu-id="d57fe-591">如果它具有一个属性的类型`Set`访问器。</span><span class="sxs-lookup"><span data-stu-id="d57fe-591">The type of a property if it has a `Set` accessor.</span></span>

* <span data-ttu-id="d57fe-592">事件的类型。</span><span class="sxs-lookup"><span data-stu-id="d57fe-592">The type of an event.</span></span>

<span data-ttu-id="d57fe-593">例如：</span><span class="sxs-lookup"><span data-stu-id="d57fe-593">For example:</span></span>

```vb
Delegate Function D(Of T, In U)(x As U) As T

Interface I1(Of In T)
End Interface

Interface I2(Of In T)
    ' OK, T is only used in an In position
    Sub M1(x As I1(Of T))

    ' Error: T is used in an Out position
    Function M2() As T
End Interface
```

<span data-ttu-id="d57fe-594">在其中一种类型必须是有效的情况下是逆变方式起作用和 covariantly (如两个属性`Get`并`Set`访问器或`ByRef`参数)，不能使用变体类型参数。</span><span class="sxs-lookup"><span data-stu-id="d57fe-594">In the case where a type must be valid be contravariantly and covariantly (such as a property with both a `Get` and `Set` accessor or a `ByRef` parameter), a variant type parameter cannot be used.</span></span>


<span data-ttu-id="d57fe-595">协变和逆变体为会导致"菱形二义性问题"。</span><span class="sxs-lookup"><span data-stu-id="d57fe-595">Co- and contra-variance give rise to a "diamond ambiguity problem".</span></span> <span data-ttu-id="d57fe-596">考虑下列代码：</span><span class="sxs-lookup"><span data-stu-id="d57fe-596">Consider the following code:</span></span>

```vb
Class C
    Implements IEnumerable(Of String)
    Implements IEnumerable(Of Exception)
     
    Public Function GetEnumerator1() As IEnumerator(Of String) _
       Implements IEnumerable(Of String).GetEnumerator
       Console.WriteLine("string")
    End Function
     
    Public Function GetEnumerator2() As IEnumerator(Of Exception) _
       Implements IEnumerable(Of Execption).GetEnumerator
       Console.WriteLine("exception")
    End Function
End Class
     
Dim c As IEnumerable(Of Object) = New C
c.GetEnumerator()
```

<span data-ttu-id="d57fe-597">该类`C`可以转换为`IEnumerable(Of Object)`两种方式，通过从协变转换`IEnumerable(Of String)`和通过从协变转换`IEnumerable(Of Exception)`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-597">The class `C` can be converted to `IEnumerable(Of Object)` in two ways, both through covariant conversion from `IEnumerable(Of String)` and through covariant conversion from `IEnumerable(Of Exception)`.</span></span> <span data-ttu-id="d57fe-598">CLR 不指定这两种方法将调用`c.GetEnumerator()`。</span><span class="sxs-lookup"><span data-stu-id="d57fe-598">The CLR does not specify which of the two methods will be called by `c.GetEnumerator()`.</span></span> <span data-ttu-id="d57fe-599">一般情况下，每当声明的类来实现协变接口使用两个不同的泛型参数具有通用超类型 (例如在这种情况下`String`和`Exception`具有通用超类型`Object`)，或对声明的类实现逆变接口使用两个不同的泛型参数具有通用的子类型，则可能会出现多义性。</span><span class="sxs-lookup"><span data-stu-id="d57fe-599">In general, whenever a class is declared to implement a covariant interface with two different generic arguments that have a common supertype (e.g. in this case `String` and `Exception` have the common supertype `Object`), or a class is declared to implement a contravariant interface with two different generic arguments that have a common subtype, then ambiguity is likely to arise.</span></span> <span data-ttu-id="d57fe-600">编译器提供了有关此类声明一条警告。</span><span class="sxs-lookup"><span data-stu-id="d57fe-600">The compiler gives a warning on such declarations.</span></span>
