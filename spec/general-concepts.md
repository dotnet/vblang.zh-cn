---
ms.openlocfilehash: 8599ffc0ad3313f4e7e42da220c22ffa7a37e56b
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306180"
---
# <a name="general-concepts"></a><span data-ttu-id="ab3c0-101">一般概念</span><span class="sxs-lookup"><span data-stu-id="ab3c0-101">General Concepts</span></span>

<span data-ttu-id="ab3c0-102">本章介绍了一些概念，这些概念需要了解 Microsoft Visual Basic 语言的语义。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-102">This chapter covers a number of concepts that are required to understand the semantics of the Microsoft Visual Basic language.</span></span> <span data-ttu-id="ab3c0-103">Visual Basic 程序员或 C/C++程序员应熟悉许多概念，但它们的精确定义可能不同。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-103">Many of the concepts should be familiar to Visual Basic programmers or C/C++ programmers, but their precise definitions may differ.</span></span>

## <a name="declarations"></a><span data-ttu-id="ab3c0-104">声明</span><span class="sxs-lookup"><span data-stu-id="ab3c0-104">Declarations</span></span>

<span data-ttu-id="ab3c0-105">Visual Basic 程序由命名实体组成。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-105">A Visual Basic program is made up of named entities.</span></span> <span data-ttu-id="ab3c0-106">这些实体是通过*声明*引入的，表示程序的 "含义"。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-106">These entities are introduced through *declarations* and represent the "meaning" of the program.</span></span>

<span data-ttu-id="ab3c0-107">在顶级，*命名空间*是组织其他实体（如嵌套命名空间和类型）的实体。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-107">At a top level, *namespaces* are entities that organize other entities, such as nested namespaces and types.</span></span> <span data-ttu-id="ab3c0-108">*类型*是描述值和定义可执行代码的实体。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-108">*Types* are entities that describe values and define executable code.</span></span> <span data-ttu-id="ab3c0-109">类型可能包含嵌套类型和类型成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-109">Types may contain nested types and type members.</span></span> <span data-ttu-id="ab3c0-110">*类型成员*包括常量、变量、方法、运算符、属性、事件、枚举值和构造函数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-110">*Type members* are constants, variables, methods, operators, properties, events, enumeration values, and constructors.</span></span>

<span data-ttu-id="ab3c0-111">可以包含其他实体的实体定义*声明空间*。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-111">An entity that can contain other entities defines a *declaration space*.</span></span> <span data-ttu-id="ab3c0-112">实体通过声明或继承引入声明空间;包含声明空间称为实体的*声明上下文*。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-112">Entities are introduced into a declaration space either through declarations or inheritance; the containing declaration space is called the entities' *declaration context*.</span></span> <span data-ttu-id="ab3c0-113">在声明空间中声明实体反过来会定义新的声明空间，该空间可以包含进一步嵌套的实体声明;因此，程序中的声明形成了声明空间的层次结构。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-113">Declaring an entity in a declaration space in turn defines a new declaration space that can contain further nested entity declarations; thus, the declarations in a program form a hierarchy of declaration spaces.</span></span>

<span data-ttu-id="ab3c0-114">除重载类型成员的情况外，声明将相同类型的同名实体引入同一声明上下文中是无效的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-114">Except in the case of overloaded type members, it is invalid for declarations to introduce identically named entities of the same kind into the same declaration context.</span></span> <span data-ttu-id="ab3c0-115">此外，声明空间不能包含具有相同名称的不同类型的实体;例如，声明空间不能包含变量和具有相同名称的方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-115">Additionally, a declaration space may never contain different kinds of entities with the same name; for example, a declaration space may never contain a variable and a method by the same name.</span></span>

<span data-ttu-id="ab3c0-116">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ab3c0-116">__Note.__</span></span> <span data-ttu-id="ab3c0-117">其他语言可能会创建一个声明空间，其中包含具有相同名称的不同类型的实体（例如，如果该语言区分大小写，并且允许基于大小写的不同声明）。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-117">It may be possible in other languages to create a declaration space that contains different kinds of entities with the same name (for example, if the language is case sensitive and allows different declarations based on casing).</span></span> <span data-ttu-id="ab3c0-118">在这种情况下，可访问的实体被视为绑定到该名称;如果有多种类型的实体可访问，则名称不明确。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-118">In that situation, the most accessible entity is considered bound to that name; if more than one type of entity is most accessible then the name is ambiguous.</span></span> <span data-ttu-id="ab3c0-119">`Public` 比 `Protected Friend`更易于访问，`Protected Friend` 比 `Protected` 或 `Friend`更易于访问，并且 `Protected` 或 `Friend` 比 `Private`更易于访问。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-119">`Public` is more accessible than `Protected Friend`, `Protected Friend` is more accessible than `Protected` or `Friend`, and `Protected` or `Friend` is more accessible than `Private`.</span></span>

<span data-ttu-id="ab3c0-120">命名空间的声明空间为 "open 结束"，因此两个具有相同完全限定名称的命名空间声明会导致相同的声明空间。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-120">The declaration space of a namespace is "open ended," so two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="ab3c0-121">在下面的示例中，两个命名空间声明构成相同的声明空间，在本例中，将两个具有完全限定名称的类声明 `Data.Customer` 和 `Data.Order:`</span><span class="sxs-lookup"><span data-stu-id="ab3c0-121">In the example below, the two namespace declarations contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Data.Customer` and `Data.Order:`</span></span>

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

<span data-ttu-id="ab3c0-122">由于这两个声明涉及到相同的声明空间，因此，如果每个声明都包含具有相同名称的类的声明，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-122">Because the two declarations contribute to the same declaration space, a compile-time error would occur if each contained a declaration of a class with the same name.</span></span>

### <a name="overloading-and-signatures"></a><span data-ttu-id="ab3c0-123">重载和签名</span><span class="sxs-lookup"><span data-stu-id="ab3c0-123">Overloading and Signatures</span></span>

<span data-ttu-id="ab3c0-124">在声明空间中声明同一种类的名称相同的实体的唯一方法是使用*重载*。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-124">The only way to declare identically named entities of the same kind in a declaration space is through *overloading*.</span></span> <span data-ttu-id="ab3c0-125">只能重载方法、运算符、实例构造函数和属性。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-125">Only methods, operators, instance constructors, and properties may be overloaded.</span></span>

<span data-ttu-id="ab3c0-126">重载类型成员必须具有唯一的签名。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-126">Overloaded type members must possess unique signatures.</span></span> <span data-ttu-id="ab3c0-127">类型成员的签名包含类型参数的数目以及该成员的参数的数量和类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-127">The signature of a type member consists of the number of type parameters, and the number and types of the member's parameters.</span></span> <span data-ttu-id="ab3c0-128">转换运算符还包括签名中运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-128">Conversion operators also include the return type of the operator in the signature.</span></span>

<span data-ttu-id="ab3c0-129">以下内容不是成员签名的一部分，因此无法重载：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-129">The following are not part of a member's signature, and hence cannot be overloaded on:</span></span>

* <span data-ttu-id="ab3c0-130">对类型成员的修饰符（例如 `Shared` 或 `Private`）。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-130">Modifiers to a type member (for example, `Shared` or `Private`).</span></span>

* <span data-ttu-id="ab3c0-131">参数的修饰符（例如 `ByVal` 或 `ByRef`）。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-131">Modifiers to a parameter (for example, `ByVal` or `ByRef`).</span></span>

* <span data-ttu-id="ab3c0-132">参数的名称。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-132">The names of the parameters.</span></span>

* <span data-ttu-id="ab3c0-133">方法或运算符（转换运算符除外）或属性的元素类型的返回类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-133">The return type of a method or operator (except for conversion operators) or the element type of a property.</span></span>

* <span data-ttu-id="ab3c0-134">类型参数上的约束。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-134">Constraints on a type parameter.</span></span>

<span data-ttu-id="ab3c0-135">下面的示例演示一组重载方法声明及其签名。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-135">The following example shows a set of overloaded method declarations along with their signatures.</span></span> <span data-ttu-id="ab3c0-136">此声明无效，因为若干方法声明具有相同的签名。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-136">This declaration would not be valid since several of the method declarations have identical signatures.</span></span>

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

<span data-ttu-id="ab3c0-137">定义泛型类型是有效的，该类型可能包含基于提供的类型参数的签名。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-137">It is valid to define a generic type that may contain members with identical signatures based on the type arguments supplied.</span></span> <span data-ttu-id="ab3c0-138">重载决策规则用于在这种重载之间进行重区分，但在某些情况下可能无法消除歧义。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-138">Overload resolution rules are used to try and disambiguate between such overloads, although there may be situations in which it is impossible to disambiguate.</span></span> <span data-ttu-id="ab3c0-139">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-139">For example:</span></span>

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

## <a name="scope"></a><span data-ttu-id="ab3c0-140">范围</span><span class="sxs-lookup"><span data-stu-id="ab3c0-140">Scope</span></span>

<span data-ttu-id="ab3c0-141">实体名称的*作用域*是所有声明空间的集合，在其中可以引用该名称，而无需进行限定。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-141">The *scope* of an entity's name is the set of all declaration spaces within which it is possible to refer to that name without qualification.</span></span> <span data-ttu-id="ab3c0-142">通常情况下，实体名称的范围是它的整个声明上下文;但是，实体的声明可能包含具有相同名称的实体的嵌套声明。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-142">In general, the scope of an entity's name is its entire declaration context; however, an entity's declaration may contain nested declarations of entities with the same name.</span></span> <span data-ttu-id="ab3c0-143">在这种情况下，嵌套*实体隐藏或隐藏、外部*实体，并且只有通过限定才能访问隐藏的实体。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-143">In that case, the nested entity *shadows*, or hides, the outer entity, and access to the shadowed entity is only possible through qualification.</span></span>

<span data-ttu-id="ab3c0-144">在嵌套于命名空间中的命名空间或类型、嵌套在其他类型中的类型和成员的主体中，将发生隐藏。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-144">Shadowing through nesting occurs in namespaces or types nested within namespaces, in types nested within other types, and in the bodies of type members.</span></span> <span data-ttu-id="ab3c0-145">通过声明嵌套的隐藏操作始终隐式进行。不需要显式语法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-145">Shadowing through the nesting of declarations always occurs implicitly; no explicit syntax is required.</span></span>

<span data-ttu-id="ab3c0-146">在下面的示例中，在 `F` 方法中，实例变量 `i` 由局部变量 `i`隐藏，但在 `G` 方法中，`i` 仍引用实例变量。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-146">In the following example, within the `F` method, the instance variable `i` is shadowed by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

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

<span data-ttu-id="ab3c0-147">当内部范围中的名称隐藏外部范围内的名称时，它将隐藏该名称的所有重载匹配项。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-147">When a name in an inner scope hides a name in an outer scope, it shadows all overloaded occurrences of that name.</span></span> <span data-ttu-id="ab3c0-148">在下面的示例中，调用 `F(1)` 调用在 `Inner` 中声明的 `F`，因为内部声明隐藏了 `F` 的所有外部匹配项。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-148">In the following example, the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="ab3c0-149">由于同样的原因，调用 `F("Hello")` 错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-149">For the same reason, the call `F("Hello")` is in error.</span></span>

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

## <a name="inheritance"></a><span data-ttu-id="ab3c0-150">继承</span><span class="sxs-lookup"><span data-stu-id="ab3c0-150">Inheritance</span></span>

<span data-ttu-id="ab3c0-151">继承关系是指一个类型（*派生*类型）派生自另一个类型（*基*类型），因此派生类型的声明空间隐式包含可访问的非构造函数类型成员和其基类型的嵌套类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-151">An inheritance relationship is one in which one type (the *derived* type) derives from another (the *base* type), such that the derived type's declaration space implicitly contains the accessible non-constructor type members and nested types of its base type.</span></span> <span data-ttu-id="ab3c0-152">在下面的示例中，类 `A` 是 `B`的基类，`B` 派生自 `A`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-152">In the following example, class `A` is the base class of `B`, and `B` is derived from `A`.</span></span>

```vb
Class A
End Class

Class B
    Inherits A
End Class
```

<span data-ttu-id="ab3c0-153">由于 `A` 未显式指定基类，因此它的基类隐式 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-153">Since `A` does not explicitly specify a base class, its base class is implicitly `Object`.</span></span>

<span data-ttu-id="ab3c0-154">下面是继承的重要方面：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-154">The following are important aspects of inheritance:</span></span>

* <span data-ttu-id="ab3c0-155">继承是可传递的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-155">Inheritance is transitive.</span></span> <span data-ttu-id="ab3c0-156">如果类型*c*派生自类型*b*，而类型*b*派生自类型*a*，则类型*c*继承在类型*B*中声明的类型成员以及在类型*a*中声明的类型成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-156">If type *C* is derived from type *B*, and type *B* is derived from type *A*, type *C* inherits the type members declared in type *B* as well as the type members declared in type *A*.</span></span>

* <span data-ttu-id="ab3c0-157">派生类型扩展但不能缩小其基类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-157">A derived type extends, but cannot narrow, its base type.</span></span> <span data-ttu-id="ab3c0-158">派生类型可以添加新的类型成员，并且它可以隐藏继承的类型成员，但不能删除继承的类型成员的定义。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-158">A derived type can add new type members, and it can shadow inherited type members, but it cannot remove the definition of an inherited type member.</span></span>

* <span data-ttu-id="ab3c0-159">因为类型的实例包含其基类型的所有类型成员，所以始终存在从派生类型到其基类型的转换。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-159">Because an instance of a type contains all of the type members of its base type, a conversion always exists from a derived type to its base type.</span></span>

* <span data-ttu-id="ab3c0-160">除 `Object`类型外，所有类型都必须有基类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-160">All types must have a base type, except for the type `Object`.</span></span> <span data-ttu-id="ab3c0-161">因此，`Object` 是所有类型的最终基本类型，所有类型都可以转换为。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-161">Thus, `Object` is the ultimate base type of all types, and all types can be converted to it.</span></span>

* <span data-ttu-id="ab3c0-162">不允许派生循环。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-162">Circularity in derivation is not permitted.</span></span> <span data-ttu-id="ab3c0-163">也就是说，当类型 `B` 从类型 `A`派生时，类型 `A` 是直接或间接从类型 `B`派生的错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-163">That is, when a type `B` derives from a type `A`, it is an error for type `A` to derive directly or indirectly from type `B`.</span></span>

* <span data-ttu-id="ab3c0-164">类型不能直接或间接从嵌套在它内部的类型派生。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-164">A type may not directly or indirectly derive from a type nested within it.</span></span>

<span data-ttu-id="ab3c0-165">下面的示例将产生编译时错误，因为这些类会循环依赖。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-165">The following example produces a compile-time error because the classes circularly depend on each other.</span></span>

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

<span data-ttu-id="ab3c0-166">下面的示例还会产生编译时错误，因为 `B` 从其嵌套类 `C` 通过类 `A`间接派生。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-166">The following example also produces a compile-time error because `B` indirectly derives from its nested class `C` through class `A`.</span></span>

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

<span data-ttu-id="ab3c0-167">下一个示例不会产生错误，因为类 `A` 不从类 `B`派生。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-167">The next example does not produce an error because class `A` does not derive from class `B`.</span></span>

```vb
Class A
    Class B
        Inherits A
    End Class 
End Class
```

### <a name="mustinherit-and-notinheritable-classes"></a><span data-ttu-id="ab3c0-168">MustInherit 和 NotInheritable 类</span><span class="sxs-lookup"><span data-stu-id="ab3c0-168">MustInherit and NotInheritable Classes</span></span>

<span data-ttu-id="ab3c0-169">`MustInherit` 类是不完整的类型，它只能作为基类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-169">A `MustInherit` class is an incomplete type that can act only as a base type.</span></span> <span data-ttu-id="ab3c0-170">不能对 `MustInherit` 类进行实例化，因此在一个上使用 `New` 运算符是错误的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-170">A `MustInherit` class cannot be instantiated, so it is an error to use the `New` operator on one.</span></span> <span data-ttu-id="ab3c0-171">声明 `MustInherit` 类的变量是有效的;此类变量只能赋值 `Nothing` 或者是派生自 `MustInherit` 类的类的值。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-171">It is valid to declare variables of `MustInherit` classes; such variables can only be assigned `Nothing` or a value that is of a class derived from the `MustInherit` class.</span></span>

<span data-ttu-id="ab3c0-172">当正则类派生自 `MustInherit` 类时，正则类必须重写所有继承的 `MustOverride` 成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-172">When a regular class is derived from a `MustInherit` class, the regular class must override all inherited `MustOverride` members.</span></span> <span data-ttu-id="ab3c0-173">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-173">For example:</span></span>

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

<span data-ttu-id="ab3c0-174">`MustInherit` 类 `A` 引入 `MustOverride` 方法 `F`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-174">The `MustInherit` class `A` introduces a `MustOverride` method `F`.</span></span> <span data-ttu-id="ab3c0-175">类 `B` 引入了附加方法 `G`，但未提供 `F`的实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-175">Class `B` introduces an additional method `G`, but does not provide an implementation of `F`.</span></span> <span data-ttu-id="ab3c0-176">因此，必须 `MustInherit`声明类 `B`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-176">Class `B` must therefore also be declared `MustInherit`.</span></span> <span data-ttu-id="ab3c0-177">类 `C` 重写 `F` 并提供实际实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-177">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="ab3c0-178">由于类 `C`中没有未完成的 `MustOverride` 成员，因此不需要 `MustInherit`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-178">Since there are no outstanding `MustOverride` members in class `C`, it is not required to be `MustInherit`.</span></span>

<span data-ttu-id="ab3c0-179">`NotInheritable` 类是一个类，该类不能从中派生其他类。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-179">A `NotInheritable` class is a class from which another class cannot be derived.</span></span> <span data-ttu-id="ab3c0-180">`NotInheritable` 类主要用于防止无意的派生。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-180">`NotInheritable` classes are primarily used to prevent unintended derivation.</span></span>

<span data-ttu-id="ab3c0-181">在此示例中，类 `B` 出错，因为它尝试从 `NotInheritable` 类 `A`派生。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-181">In this example, class `B` is in error because it attempts to derive from the `NotInheritable` class `A`.</span></span> <span data-ttu-id="ab3c0-182">不能将类标记 `MustInherit` 和 `NotInheritable`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-182">A class cannot be marked both `MustInherit` and `NotInheritable`.</span></span>

```vb
NotInheritable Class A
End Class

Class B
    ' Error, a class cannot derive from a NotInheritable class.
    Inherits A
End Class
```

### <a name="interfaces-and-multiple-inheritance"></a><span data-ttu-id="ab3c0-183">接口和多重继承</span><span class="sxs-lookup"><span data-stu-id="ab3c0-183">Interfaces and Multiple Inheritance</span></span>

<span data-ttu-id="ab3c0-184">与仅从单个基类型派生的其他类型不同，接口可能派生自多个基接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-184">Unlike other types, which only derive from a single base type, an interface may derive from multiple base interfaces.</span></span> <span data-ttu-id="ab3c0-185">因此，接口可以从不同的基接口继承名称相同的类型成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-185">Because of this, an interface can inherit an identically named type member from different base interfaces.</span></span> <span data-ttu-id="ab3c0-186">在这种情况下，交叉继承名称在派生接口中不可用，并且通过派生接口引用这些类型成员中的任何成员都将导致编译时错误，而不考虑签名或重载。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-186">In such a case, the multiply-inherited name is not available in the derived interface, and referring to any of those type members through the derived interface causes a compile-time error, regardless of signatures or overloading.</span></span> <span data-ttu-id="ab3c0-187">相反，必须通过基接口名称来引用冲突的类型成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-187">Instead, conflicting type members must be referenced through a base interface name.</span></span>

<span data-ttu-id="ab3c0-188">在下面的示例中，前两个语句会导致编译时错误，这是因为在接口 `IListCounter`中不提供多重继承成员 `Count`：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-188">In the following example, the first two statements cause compile-time errors because the multiply-inherited member `Count` is not available in interface `IListCounter`:</span></span>

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

<span data-ttu-id="ab3c0-189">如示例所示，通过将 `x` 强制转换为相应的基接口类型来解决歧义。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-189">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="ab3c0-190">此类强制转换没有运行时成本;它们只是在编译时将该实例视为不太派生的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-190">Such casts have no run-time costs; they merely consist of viewing the instance as a less-derived type at compile time.</span></span>

<span data-ttu-id="ab3c0-191">如果通过多个路径从相同的基接口继承单个类型成员，则会将类型成员视为仅继承一次。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-191">When a single type member is inherited from the same base interface through multiple paths, the type member is treated as if it were only inherited once.</span></span> <span data-ttu-id="ab3c0-192">换言之，派生接口仅包含从特定基接口继承的每个类型成员的一个实例。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-192">In other words, the derived interface only contains one instance of each type member inherited from a particular base interface.</span></span> <span data-ttu-id="ab3c0-193">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-193">For example:</span></span>

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

<span data-ttu-id="ab3c0-194">如果某个类型成员名称在通过继承层次结构的一个路径中被隐藏，则该名称将隐藏在所有路径中。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-194">If a type member name is shadowed in one path through the inheritance hierarchy, then the name is shadowed in all paths.</span></span> <span data-ttu-id="ab3c0-195">在下面的示例中，`IBase.F` 成员由 `ILeft.F` 成员隐藏，但没有在 `IRight`中隐藏：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-195">In the following example, the `IBase.F` member is shadowed by the `ILeft.F` member, but is not shadowed in `IRight`:</span></span>

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

<span data-ttu-id="ab3c0-196">调用 `d.F(1)` 将选择 `ILeft.F`，即使 `IBase.F` 在通过 `IRight`导致的访问路径中不会被隐藏。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-196">The invocation `d.F(1)` selects `ILeft.F`, even though `IBase.F` appears to not be shadowed in the access path that leads through `IRight`.</span></span> <span data-ttu-id="ab3c0-197">由于从 `IDerived` 到 `ILeft` 的访问路径 `IBase` 阴影 `IBase.F`，因此，从 `IDerived` 到 `IRight` 到 `IBase`的访问路径中还会隐藏该成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-197">Because the access path from `IDerived` to `ILeft` to `IBase` shadows `IBase.F`, the member is also shadowed in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

### <a name="shadowing"></a><span data-ttu-id="ab3c0-198">阴影操作</span><span class="sxs-lookup"><span data-stu-id="ab3c0-198">Shadowing</span></span>

<span data-ttu-id="ab3c0-199">派生类型通过重新声明继承的类型成员的名称来隐藏该成员的名称。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-199">A derived type shadows the name of an inherited type member by re-declaring it.</span></span> <span data-ttu-id="ab3c0-200">隐藏名称不会删除使用该名称继承的类型成员;它只是使该名称的所有继承的类型成员在派生类中不可用。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-200">Shadowing a name does not remove the inherited type members with that name; it merely makes all of the inherited type members with that name unavailable in the derived class.</span></span> <span data-ttu-id="ab3c0-201">隐藏声明可以是任何类型的实体。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-201">The shadowing declaration may be any type of entity.</span></span>

<span data-ttu-id="ab3c0-202">可以重载的实体可以选择两种形式的隐藏之一。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-202">Entities than can be overloaded can choose one of two forms of shadowing.</span></span> <span data-ttu-id="ab3c0-203">使用 `Shadows` 关键字指定*按名称隐藏*。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-203">*Shadowing by name* is specified using the `Shadows` keyword.</span></span> <span data-ttu-id="ab3c0-204">按名称隐藏的实体隐藏了基类中该名称的所有内容，包括所有重载。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-204">An entity that shadows by name hides everything by that name in the base class, including all overloads.</span></span> <span data-ttu-id="ab3c0-205">*按名称和签名进行隐藏*是使用 `Overloads` 关键字指定的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-205">*Shadowing by name and signature* is specified using the `Overloads` keyword.</span></span> <span data-ttu-id="ab3c0-206">按名称和签名隐藏的实体将使用与实体相同的签名隐藏该名称的所有内容。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-206">An entity that shadows by name and signature hides everything by that name with the same signature as the entity.</span></span> <span data-ttu-id="ab3c0-207">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-207">For example:</span></span>

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

<span data-ttu-id="ab3c0-208">使用按名称和签名 `ParamArray` 参数隐藏方法仅隐藏单个签名，而不会隐藏所有可能的已展开签名。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-208">Shadowing a method with a `ParamArray` argument by name and signature hides only the individual signature, not all possible expanded signatures.</span></span> <span data-ttu-id="ab3c0-209">即使隐藏方法的签名与隐藏方法的未展开签名匹配，也是如此。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-209">This is true even if the signature of the shadowing method matches the unexpanded signature of the shadowed method.</span></span> <span data-ttu-id="ab3c0-210">如下示例中：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-210">The following example:</span></span>

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

<span data-ttu-id="ab3c0-211">即使 `Derived.F` 与 `Base.F`的未展开形式具有相同的签名，也会打印 `Base`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-211">prints `Base`, even though `Derived.F` has the same signature as the unexpanded form of `Base.F`.</span></span>

<span data-ttu-id="ab3c0-212">相反，带有 `ParamArray` 参数的方法只隐藏具有相同签名的方法，而不是所有可能的展开签名。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-212">Conversely, a method with a `ParamArray` argument only shadows methods with the same signature, not all possible expanded signatures.</span></span> <span data-ttu-id="ab3c0-213">如下示例中：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-213">The following example:</span></span>

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

<span data-ttu-id="ab3c0-214">即使 `Derived.F` 具有与 `Base.F`具有相同签名的扩展窗体，也会打印 `Base`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-214">prints `Base`, even though `Derived.F` has an expanded form that has the same signature as `Base.F`.</span></span>

<span data-ttu-id="ab3c0-215">如果 `Overloads` 方法或属性 `Overrides`，则不指定 `Shadows` 或的隐藏方法或属性将假定 `Overloads` `Shadows`，否则为。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-215">A shadowing method or property that does not specify `Shadows` or `Overloads` assumes `Overloads` if the method or property is declared `Overrides`, `Shadows` otherwise.</span></span> <span data-ttu-id="ab3c0-216">如果一组重载实体中的一个成员指定 `Shadows` 或 `Overloads` 关键字，则它们都必须指定。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-216">If one member of a set of overloaded entities specifies the `Shadows` or `Overloads` keyword, they all must specify it.</span></span> <span data-ttu-id="ab3c0-217">不能同时指定 `Shadows` 和 `Overloads` 关键字。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-217">The `Shadows` and `Overloads` keywords cannot be specified at the same time.</span></span> <span data-ttu-id="ab3c0-218">不能在标准模块中指定 `Shadows` 和 `Overloads`;标准模块中的成员隐式隐藏从 `Object`继承的成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-218">Neither `Shadows` nor `Overloads` can be specified in a standard module; members in a standard module implicitly shadow members inherited from `Object`.</span></span>

<span data-ttu-id="ab3c0-219">这是一个有效的方法是，通过接口继承（因而不可用）隐藏已被乘法除继承的类型成员的名称，从而使该名称在派生接口中可用。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-219">It is valid to shadow the name of a type member that has been multiply-inherited through interface inheritance (and which is thereby unavailable), thus making the name available in the derived interface.</span></span>

<span data-ttu-id="ab3c0-220">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-220">For example:</span></span>

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

<span data-ttu-id="ab3c0-221">由于允许方法隐藏继承方法，因此类可能包含多个具有相同签名的 `Overridable` 方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-221">Because methods are allowed to shadow inherited methods, it is possible for a class to contain several `Overridable` methods with the same signature.</span></span> <span data-ttu-id="ab3c0-222">这并不会带来多义性问题，因为只有最常派生的方法是可见的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-222">This does not present an ambiguity problem, since only the most-derived method is visible.</span></span> <span data-ttu-id="ab3c0-223">在下面的示例中，`C` 和 `D` 类包含两个具有相同签名的 `Overridable` 方法：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-223">In the following example, the `C` and `D` classes contain two `Overridable` methods with the same signature:</span></span>

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

<span data-ttu-id="ab3c0-224">这里有两种 `Overridable` 方法：一个由类 `A` 引入，另一个由类 `C`引入。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-224">There are two `Overridable` methods here: one introduced by class `A` and the one introduced by class `C`.</span></span> <span data-ttu-id="ab3c0-225">类引入的方法 `C` 隐藏从类 `A`继承的方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-225">The method introduced by class `C` hides the method inherited from class `A`.</span></span> <span data-ttu-id="ab3c0-226">因此，类 `D` 中的 `Overrides` 声明会重写由类 `C`引入的方法，而类 `D` 不可能重写类 `A`引入的方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-226">Thus, the `Overrides` declaration in class `D` overrides the method introduced by class `C`, and it is not possible for class `D` to override the method introduced by class `A`.</span></span> <span data-ttu-id="ab3c0-227">该示例生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-227">The example produces the output:</span></span>

```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="ab3c0-228">可以通过以下方式调用隐藏的 `Overridable` 方法：通过不隐藏该方法的不太派生类型访问 `D` 类的实例。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-228">It is possible to invoke the hidden `Overridable` method by accessing an instance of class `D` through a less-derived type in which the method is not hidden.</span></span>

<span data-ttu-id="ab3c0-229">隐藏 `MustOverride` 方法是无效的，因为在大多数情况下，这会使类不可用。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-229">It is not valid to shadow a `MustOverride` method, because in most cases this would make the class unusable.</span></span> <span data-ttu-id="ab3c0-230">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-230">For example:</span></span>

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

<span data-ttu-id="ab3c0-231">在这种情况下，需要类 `MoreDerived` `Base.F`重写 `MustOverride` 方法，但由于类 `Derived` 阴影 `Base.F`，因此这是不可能的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-231">In this case, the class `MoreDerived` is required to override the `MustOverride` method `Base.F`, but because the class `Derived` shadows `Base.F`, this is not possible.</span></span> <span data-ttu-id="ab3c0-232">无法声明 `Derived`的有效子代。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-232">There is no way to declare a valid descendent of `Derived`.</span></span>

<span data-ttu-id="ab3c0-233">与隐藏外部作用域中的名称不同，从继承的作用域中隐藏可访问的名称会导致报告警告，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-233">In contrast to shadowing a name from an outer scope, shadowing an accessible name from an inherited scope causes a warning to be reported, as in the following example:</span></span>

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

<span data-ttu-id="ab3c0-234">类 `Derived` 中 `F` 的声明会导致报告警告。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-234">The declaration of method `F` in class `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="ab3c0-235">隐藏继承名称并不是一个错误，因为这会阻止基类的单独演化。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-235">Shadowing an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="ab3c0-236">例如，由于类的更高版本 `Base` 引入了在类的早期版本中不存在的方法 `F`，因此可能会出现上述情况。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-236">For example, the above situation might have come about because a later version of class `Base` introduced a method `F` that was not present in an earlier version of the class.</span></span> <span data-ttu-id="ab3c0-237">如果上述情况是错误的，则对单独的版本控制类库中的基类进行的*任何*更改都可能导致派生类无效。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-237">Had the above situation been an error, *any* change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="ab3c0-238">隐藏继承名称导致的警告可以通过使用 `Shadows` 或 `Overloads` 修饰符来消除：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-238">The warning caused by shadowing an inherited name can be eliminated through use of the `Shadows` or `Overloads` modifier:</span></span>

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

<span data-ttu-id="ab3c0-239">`Shadows` 修饰符指示隐藏继承成员的意图。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-239">The `Shadows` modifier indicates the intention to shadow the inherited member.</span></span> <span data-ttu-id="ab3c0-240">如果没有要隐藏的类型成员名称，则指定 `Shadows` 或 `Overloads` 修饰符是错误的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-240">It is not an error to specify the `Shadows` or `Overloads` modifier if there is no type member name to shadow.</span></span>

<span data-ttu-id="ab3c0-241">新成员的声明仅在新成员的范围内隐藏继承的成员，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-241">A declaration of a new member shadows an inherited member only within the scope of the new member, as in the following example:</span></span>

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

<span data-ttu-id="ab3c0-242">在上面的示例中，类 `Derived` 中 `F` 的方法的声明隐藏了继承自类 `Base`的方法 `F`，但由于类 `F` 中的新方法 `Derived` 具有 `Private` 访问权限，因此它的作用域不会扩展到类 `MoreDerived`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-242">In the example above, the declaration of method `F` in class `Derived` shadows the method `F` that was inherited from class `Base`, but since the new method `F` in class `Derived` has `Private` access, its scope does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="ab3c0-243">因此，`MoreDerived.G` 中 `F()` 调用有效，并将调用 `Base.F`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-243">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span> <span data-ttu-id="ab3c0-244">在重载类型成员的情况下，会将整个重载类型成员集视为它们都具有用于隐藏的最宽松的访问权限。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-244">In the case of overloaded type members, the entire set of overloaded type members is treated as if they all had the most permissive access for the purposes of shadowing.</span></span>

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

<span data-ttu-id="ab3c0-245">在此示例中，即使用 `Private` 访问声明了 `Derived` 中 `F()` 的声明，使用 `Public` 访问声明重载的 `F(Integer)`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-245">In this example, even though the declaration of `F()` in `Derived` is declared with `Private` access, the overloaded `F(Integer)` is declared with `Public` access.</span></span> <span data-ttu-id="ab3c0-246">因此，为了进行隐藏，`Derived` 中的名称 `F` 被视为 `Public`，因此两个方法都在 `Base`中隐藏 `F`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-246">Therefore, for the purpose of shadowing, the name `F` in `Derived` is treated as if it was `Public`, so both methods shadow `F` in `Base`.</span></span>

## <a name="implementation"></a><span data-ttu-id="ab3c0-247">实现</span><span class="sxs-lookup"><span data-stu-id="ab3c0-247">Implementation</span></span>

<span data-ttu-id="ab3c0-248">当某个类型声明它实现接口并且该类型实现了该接口的所有类型成员时，就存在*实现*关系。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-248">An *implementation* relationship exists when a type declares that it implements an interface and the type implements all the type members of the interface.</span></span> <span data-ttu-id="ab3c0-249">实现特定接口的类型可转换为该接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-249">A type that implements a particular interface is convertible to that interface.</span></span> <span data-ttu-id="ab3c0-250">接口不能进行实例化，但声明接口的变量是有效的;此类变量只能分配给实现接口的类的值。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-250">Interfaces cannot be instantiated, but it is valid to declare variables of interfaces; such variables can only be assigned a value that is of a class that implements the interface.</span></span> <span data-ttu-id="ab3c0-251">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-251">For example:</span></span>

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

<span data-ttu-id="ab3c0-252">实现带有交叉继承类型成员的接口的类型仍必须实现这些方法，即使无法直接从正在实现的派生接口中访问它们。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-252">A type implementing an interface with multiply-inherited type members must still implement those methods, even though they cannot be accessed directly from the derived interface being implemented.</span></span> <span data-ttu-id="ab3c0-253">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-253">For example:</span></span>

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

<span data-ttu-id="ab3c0-254">即使 `MustInherit` 类也必须提供实现的接口的所有成员的实现;但是，它们可以通过将它们声明为 `MustOverride`来推迟这些方法的实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-254">Even `MustInherit` classes must provide implementations of all the members of implemented interfaces; however, they can defer implementation of these methods by declaring them as `MustOverride`.</span></span> <span data-ttu-id="ab3c0-255">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-255">For example:</span></span>

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

<span data-ttu-id="ab3c0-256">类型可以选择重新实现其基类型实现的接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-256">A type may choose to re-implement an interface that its base type implements.</span></span> <span data-ttu-id="ab3c0-257">若要重新实现接口，类型必须显式声明它实现接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-257">To re-implement the interface, the type must explicitly state that it implements the interface.</span></span> <span data-ttu-id="ab3c0-258">类型重新实现接口可能会选择仅重新实现接口的部分（而不是全部）成员--任何未重新实现的成员都继续使用基类型的实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-258">A type re-implementing an interface may choose to re-implement only some, but not all, of the members of the interface -- any members not re-implemented continue to use the base type's implementation.</span></span> <span data-ttu-id="ab3c0-259">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-259">For example:</span></span>

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

<span data-ttu-id="ab3c0-260">此示例将打印：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-260">This example prints:</span></span>

```console
TestDerived.DerivedTest1
TestBase.Test2
```

<span data-ttu-id="ab3c0-261">当派生类型实现了一个接口，该接口的基接口由派生的类型的基类型实现时，派生的类型可选择仅实现基类型尚未实现的接口的类型成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-261">When a derived type implements an interface whose base interfaces are implemented by the derived type's base types, the derived type can choose to only implement the interface's type members that are not already implemented by the base types.</span></span> <span data-ttu-id="ab3c0-262">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-262">For example:</span></span>

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

<span data-ttu-id="ab3c0-263">接口方法还可以使用基类型中的可重写方法实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-263">An interface method can also be implemented using an overridable method in a base type.</span></span> <span data-ttu-id="ab3c0-264">在这种情况下，派生类型还可以重写可重写的方法，并更改接口的实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-264">In that case, a derived type may also override the overridable method and alter the implementation of the interface.</span></span> <span data-ttu-id="ab3c0-265">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-265">For example:</span></span>

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

### <a name="implementing-methods"></a><span data-ttu-id="ab3c0-266">实现方法</span><span class="sxs-lookup"><span data-stu-id="ab3c0-266">Implementing Methods</span></span>

<span data-ttu-id="ab3c0-267">类型通过向 `Implements` 子句提供方法来*实现*已实现接口的类型成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-267">A type *implements* a type member of an implemented interface by supplying a method with an `Implements` clause.</span></span> <span data-ttu-id="ab3c0-268">这两个类型成员必须具有相同数量的参数，参数的所有类型和修饰符都必须匹配，包括可选参数的默认值、返回类型必须匹配以及方法参数的所有约束必须匹配。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-268">The two type members must have the same number of parameters, all of the types and modifiers of the parameters must match, including the default value of optional parameters, the return type must match, and all of the constraints on method parameters must match.</span></span> <span data-ttu-id="ab3c0-269">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-269">For example:</span></span>

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

<span data-ttu-id="ab3c0-270">如果一个方法满足上述条件，则可以实现任意数量的接口类型成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-270">A single method may implement any number of interface type members if they all meet the above criteria.</span></span> <span data-ttu-id="ab3c0-271">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-271">For example:</span></span>

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

<span data-ttu-id="ab3c0-272">在泛型接口中实现方法时，实现方法必须提供与接口的类型参数相对应的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-272">When implementing a method in a generic interface, the implementing method must supply the type arguments that correspond to the interface's type parameters.</span></span> <span data-ttu-id="ab3c0-273">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-273">For example:</span></span>

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

<span data-ttu-id="ab3c0-274">请注意，可能不会为某些类型参数集实施泛型接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-274">Note that it is possible that a generic interface may not be implementable for some set of type arguments.</span></span>

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

## <a name="polymorphism"></a><span data-ttu-id="ab3c0-275">多态性</span><span class="sxs-lookup"><span data-stu-id="ab3c0-275">Polymorphism</span></span>

<span data-ttu-id="ab3c0-276">*多态性*提供了不同的方法或属性实现的功能。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-276">*Polymorphism* provides the ability to vary the implementation of a method or property.</span></span> <span data-ttu-id="ab3c0-277">对于多态性，相同的方法或属性可以执行不同的操作，具体取决于调用该方法的实例的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-277">With polymorphism, the same method or property can perform different actions depending on the run-time type of the instance that invokes it.</span></span> <span data-ttu-id="ab3c0-278">作为多态的方法或属性称为可*重写*。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-278">Methods or properties that are polymorphic are called *overridable*.</span></span> <span data-ttu-id="ab3c0-279">与此相反，无法重写的方法或属性的实现是固定的;无论方法或属性是在声明它的类的实例上调用的还是派生类的实例，实现都是相同的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-279">By contrast, the implementation of a non-overridable method or property is invariant; the implementation is the same whether the method or property is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="ab3c0-280">调用不可重写的方法或属性时，实例的编译时类型是确定因素。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-280">When a non-overridable method or property is invoked, the compile-time type of the instance is the determining factor.</span></span> <span data-ttu-id="ab3c0-281">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-281">For example:</span></span>

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

<span data-ttu-id="ab3c0-282">可重写的方法也可能 `MustOverride`，这意味着它不提供方法体，因此必须重写。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-282">An overridable method may also be `MustOverride`, which means that it provides no method body and must be overridden.</span></span> <span data-ttu-id="ab3c0-283">仅 `MustInherit` 类中允许 `MustOverride` 方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-283">`MustOverride` methods are only allowed in `MustInherit` classes.</span></span>

<span data-ttu-id="ab3c0-284">在下面的示例中，类 `Shape` 定义可自行绘制的几何形状对象的抽象概念：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-284">In the following example, the class `Shape` defines the abstract notion of a geometrical shape object that can paint itself:</span></span>

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

<span data-ttu-id="ab3c0-285">`MustOverride` `Paint` 方法，因为没有有意义的默认实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-285">The `Paint` method is `MustOverride` because there is no meaningful default implementation.</span></span> <span data-ttu-id="ab3c0-286">`Ellipse` 和 `Box` 类是具体的 `Shape` 实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-286">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="ab3c0-287">由于不 `MustInherit`这些类，因此它们需要重写 `Paint` 方法并提供实际实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-287">Because these classes are not `MustInherit`, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="ab3c0-288">引用 `MustOverride` 方法的基本访问是错误的，如下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-288">It is an error for a base access to reference a `MustOverride` method, as the following example demonstrates:</span></span>

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

<span data-ttu-id="ab3c0-289">为 `MyBase.F()` 调用报告了错误，因为它引用 `MustOverride` 方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-289">An error is reported for the `MyBase.F()` invocation because it references a `MustOverride` method.</span></span>

### <a name="overriding-methods"></a><span data-ttu-id="ab3c0-290">重写方法</span><span class="sxs-lookup"><span data-stu-id="ab3c0-290">Overriding Methods</span></span>

<span data-ttu-id="ab3c0-291">类型可以*重写*继承的可重写方法，方法是声明具有相同名称和签名的方法，并使用 `Overrides` 修饰符标记声明。下面列出了重写方法的其他要求。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-291">A type may *override* an inherited overridable method by declaring a method with the same name and , signature, and marking the declaration with the `Overrides` modifier.There are additional requirements on overriding methods, listed below.</span></span> <span data-ttu-id="ab3c0-292">`Overridable` 方法声明引入了一个新方法，而 `Overrides` 方法声明会替换该方法的继承实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-292">Whereas an `Overridable` method declaration introduces a new method, an `Overrides` method declaration replaces the inherited implementation of the method.</span></span>

<span data-ttu-id="ab3c0-293">重写方法可以 `NotOverridable`声明，这会阻止派生类型中的方法的任何进一步重写。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-293">An overriding method may be declared `NotOverridable`, which prevents any further overriding of the method in derived types.</span></span> <span data-ttu-id="ab3c0-294">实际上，在任何更进一步的派生类中，`NotOverridable` 方法都将变为不可重写。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-294">In effect, `NotOverridable` methods become non-overridable in any further derived classes.</span></span>

<span data-ttu-id="ab3c0-295">请看下面的示例：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-295">Consider the following example:</span></span>

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

<span data-ttu-id="ab3c0-296">在此示例中，类 `B` 提供了两种 `Overrides` 方法：具有 `NotOverridable` 修饰符的方法 `F` 和不包含方法 `G` 的方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-296">In the example, class `B` provides two `Overrides` methods: a method `F` that has the `NotOverridable` modifier and a method `G` that does not.</span></span> <span data-ttu-id="ab3c0-297">使用 `NotOverridable` 修饰符可防止类 `C` 其他重写方法 `F`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-297">Use of the `NotOverridable` modifier prevents class `C` from further overriding method `F`.</span></span>

<span data-ttu-id="ab3c0-298">重写方法还可以 `MustOverride`声明，即使未 `MustOverride`声明其重写的方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-298">An overriding method may also be declared `MustOverride`, even if the method that it is overriding is not declared `MustOverride`.</span></span> <span data-ttu-id="ab3c0-299">这要求 `MustInherit` 中声明包含类，并且任何未声明 `MustInherit` 的其他派生类必须重写方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-299">This requires that the containing class be declared `MustInherit` and that any further derived classes that are not declared `MustInherit` must override the method.</span></span> <span data-ttu-id="ab3c0-300">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-300">For example:</span></span>

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

<span data-ttu-id="ab3c0-301">在此示例中，类 `B` 用 `MustOverride` 方法重写 `A.F`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-301">In the example, class `B` overrides `A.F` with a `MustOverride` method.</span></span> <span data-ttu-id="ab3c0-302">这意味着，从 `B` 派生的任何类都必须重写 `F`，除非它们也 `MustInherit` 声明。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-302">This means that any classes derived from `B` will have to override `F`, unless they are declared `MustInherit` as well.</span></span>

<span data-ttu-id="ab3c0-303">如果以下所有条件都适用，则会发生编译时错误：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-303">A compile-time error occurs unless all of the following are true of an overriding method:</span></span>

* <span data-ttu-id="ab3c0-304">声明上下文包含单个可访问的继承方法，该方法具有相同的签名和返回类型（如果有）作为重写方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-304">The declaration context contains a single accessible inherited method with the same signature and return type (if any) as the overriding method.</span></span>
* <span data-ttu-id="ab3c0-305">要重写的继承方法是可重写的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-305">The inherited method being overridden is overridable.</span></span> <span data-ttu-id="ab3c0-306">换言之，要重写的继承方法不是 `Shared` 或 `NotOverridable`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-306">In other words, the inherited method being overridden is not `Shared` or `NotOverridable`.</span></span>
* <span data-ttu-id="ab3c0-307">所声明方法的可访问域与要重写的继承方法的可访问域相同。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-307">The accessibility domain of the method being declared is the same as the accessibility domain of the inherited method being overridden.</span></span> <span data-ttu-id="ab3c0-308">有一种例外情况：如果另一个方法在重写方法不具有 `Friend` 访问权限的另一个程序集中，则 `Protected Friend` 方法必须由 `Protected` 方法重写。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-308">There is one exception: a `Protected Friend` method must be overridden by a `Protected` method if the other method is in another assembly that the overriding method does not have `Friend` access to.</span></span>
* <span data-ttu-id="ab3c0-309">重写方法的参数与 `ByVal`、`ByRef`、`ParamArray,` 和 `Optional` 修饰符（包括为可选参数提供的值）的使用情况相匹配的已重写方法的参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-309">The parameters of the overriding method match the overridden method's parameters in regards to usage of the `ByVal`, `ByRef`, `ParamArray,` and `Optional` modifiers, including the values provided for optional parameters.</span></span>
* <span data-ttu-id="ab3c0-310">重写方法的类型参数与重写的方法的类型参数匹配，而不是类型约束。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-310">The type parameters of the overriding method match the overridden method's type parameters in regards to type constraints.</span></span>

<span data-ttu-id="ab3c0-311">当重写基泛型类型中的方法时，重写方法必须提供对应于基类型参数的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-311">When overriding a method in a base generic type, the overriding method must supply the type arguments that correspond to the base type parameters.</span></span> <span data-ttu-id="ab3c0-312">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-312">For example:</span></span>

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

<span data-ttu-id="ab3c0-313">请注意，对于某些类型参数集，可能无法重写泛型类中的可重写方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-313">Note that it is possible that an overridable method in a generic class may not be able to be overridden for some sets of type arguments.</span></span> <span data-ttu-id="ab3c0-314">如果该方法 `MustOverride`声明，这意味着可能无法实现某些继承链。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-314">If the method is declared `MustOverride`, this means that some inheritance chains may not be possible.</span></span> <span data-ttu-id="ab3c0-315">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-315">For example:</span></span>

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

<span data-ttu-id="ab3c0-316">重写声明可使用基本访问权限访问重写的基方法，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-316">An override declaration can access the overridden base method using a base access, as in the following example:</span></span>

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

在此示例中，类 `Derived` 中 `MyBase.PrintVariables()` 的调用调用在类 `Base`中声明的 `PrintVariables` 方法。 基本访问将禁用可重写的调用机制，并只将基方法视为不可重写的方法。 <span data-ttu-id="ab3c0-319">将 `Derived` 中的调用写入 `CType(Me, Base).PrintVariables()`，它会以递归方式调用在 `Derived`中声明的 `PrintVariables` 方法，而不是在 `Base`中声明的方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-319">Had the invocation in `Derived` been written `CType(Me, Base).PrintVariables()`, it would recursively invoke the `PrintVariables` method declared in `Derived`, not the one declared in `Base`.</span></span>

<span data-ttu-id="ab3c0-320">仅当包含 `Overrides` 修饰符时，方法才能重写另一个方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-320">Only when it includes an `Overrides` modifier can a method override another method.</span></span> <span data-ttu-id="ab3c0-321">在所有其他情况下，具有与继承方法相同的签名的方法只是隐藏继承方法，如下例所示：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-321">In all other cases, a method with the same signature as an inherited method simply shadows the inherited method, as in the example below:</span></span>

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

<span data-ttu-id="ab3c0-322">在此示例中，类 `Derived` 中 `F` 方法不包括 `Overrides` 修饰符，因此不会重写类 `Base`中的方法 `F`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-322">In the example, the method `F` in class `Derived` does not include an `Overrides` modifier and therefore does not override method `F` in class `Base`.</span></span> <span data-ttu-id="ab3c0-323">相反，类 `Derived` 中的方法 `F` 隐藏类 `Base`中的方法，并且会报告警告，因为声明不包括 `Shadows` 或 `Overloads` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-323">Rather, method `F` in class `Derived` shadows the method in class `Base`, and a warning is reported because the declaration does not include a `Shadows` or `Overloads` modifier.</span></span>

<span data-ttu-id="ab3c0-324">在下面的示例中，类 `Derived` 的方法 `F` 隐藏了 `F` 继承自类 `Base`的可重写方法：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-324">In the following example, method `F` in class `Derived` shadows the overridable method `F` inherited from class `Base`:</span></span>

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

<span data-ttu-id="ab3c0-325">由于类 `Derived` 中的新方法 `F` 具有 `Private` 访问权限，因此它的作用域只包含 `Derived` 的类体，不会扩展到类 `MoreDerived`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-325">Since the new method `F` in class `Derived` has `Private` access, its scope only includes the class body of `Derived` and does not extend to class `MoreDerived`.</span></span> <span data-ttu-id="ab3c0-326">因此，`F` 类 `MoreDerived` 中的方法的声明可以重写 `F` 继承自类 `Base`的方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-326">The declaration of method `F` in class `MoreDerived` is therefore permitted to override the method `F` inherited from class `Base`.</span></span>

<span data-ttu-id="ab3c0-327">调用 `Overridable` 方法时，实例方法的派生程度最高的实现是根据实例的类型调用的，无论调用的是基类中的方法还是派生类中的方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-327">When an `Overridable` method is invoked, the most derived implementation of the instance method is called, based on the type of the instance, regardless of whether the call is to the method in the base class or the derived class.</span></span> <span data-ttu-id="ab3c0-328">与类 `R` `M` 的 `Overridable` 方法的派生程度最高的实现如下所示：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-328">The most derived implementation of an `Overridable` method `M` with respect to a class `R` is determined as follows:</span></span>

* <span data-ttu-id="ab3c0-329">如果 `R` 包含 `M`的引入 `Overridable` 声明，则这是 `M`的派生程度最高的实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-329">If `R` contains the introducing `Overridable` declaration of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="ab3c0-330">否则，如果 `R` 包含 `M`的重写，则这是 `M`的派生程度最高的实现。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-330">Otherwise, if `R` contains an override of `M`, this is the most derived implementation of `M`.</span></span>

* <span data-ttu-id="ab3c0-331">否则，`M` 的派生程度最高的实现与 `R`的直接基类的实现相同。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-331">Otherwise, the most derived implementation of `M` is the same as that of the direct base class of `R`.</span></span>

## <a name="accessibility"></a><span data-ttu-id="ab3c0-332">辅助功能</span><span class="sxs-lookup"><span data-stu-id="ab3c0-332">Accessibility</span></span>

<span data-ttu-id="ab3c0-333">声明指定它声明的实体的*可访问性*。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-333">A declaration specifies the *accessibility* of the entity it declares.</span></span> <span data-ttu-id="ab3c0-334">实体的可访问性不会更改实体名称的作用域。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-334">An entity's accessibility does not change the scope of an entity's name.</span></span> <span data-ttu-id="ab3c0-335">声明的*可访问域*是声明的实体可访问的所有声明空间的集合。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-335">The *accessibility domain* of a declaration is the set of all declaration spaces in which the declared entity is accessible.</span></span>

<span data-ttu-id="ab3c0-336">五种访问类型为 `Public`、`Protected`、`Friend`、`Protected Friend`和 `Private`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-336">The five access types are `Public`, `Protected`, `Friend`, `Protected Friend`, and `Private`.</span></span> <span data-ttu-id="ab3c0-337">`Public` 是最宽松的访问类型，其他四种类型都是 `Public`的子集。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-337">`Public` is the most permissive access type, and the four other types are all subsets of `Public`.</span></span> <span data-ttu-id="ab3c0-338">最小的权限访问类型是 `Private`的，另外四种访问类型都是 `Private`的超集。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-338">The least permissive access type is `Private`, and the four other access types are all supersets of `Private`.</span></span>

```antlr
AccessModifier
    : 'Public'
    | 'Protected'
    | 'Friend'
    | 'Private'
    | 'Protected' 'Friend'
    ;
```

<span data-ttu-id="ab3c0-339">声明的访问类型通过可选的访问修饰符指定，该修饰符可以是 `Public`、`Protected`、`Friend`、`Private`或 `Protected` 和 `Friend`的组合。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-339">The access type for a declaration is specified via an optional access modifier, which can be `Public`, `Protected`, `Friend`, `Private`, or the combination of `Protected` and `Friend`.</span></span> <span data-ttu-id="ab3c0-340">如果未指定访问修饰符，则默认访问类型取决于声明上下文;允许的访问类型也依赖于声明上下文。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-340">If no access modifier is specified, the default access type depends on the declaration context; the permitted access types also depend on the declaration context.</span></span>

* <span data-ttu-id="ab3c0-341">使用 `Public` 修饰符声明的实体具有 `Public` 访问权限。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-341">Entities declared with the `Public` modifier have `Public` access.</span></span> <span data-ttu-id="ab3c0-342">对 `Public` 实体的使用没有限制。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-342">There are no restrictions on the use of `Public` entities.</span></span>

* <span data-ttu-id="ab3c0-343">使用 `Protected` 修饰符声明的实体具有 `Protected` 访问权限。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-343">Entities declared with the `Protected` modifier have `Protected` access.</span></span> <span data-ttu-id="ab3c0-344">只能在类的成员（常规类型成员和嵌套类）或 `Overridable` 标准模块和结构成员（必须通过定义从 `System.Object` 或 `System.ValueType`继承）上指定 `Protected` 访问。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-344">`Protected` access can only be specified on members of classes (both regular type members and nested classes) or on `Overridable` members of standard modules and structures (which must, by definition, be inherited from `System.Object` or `System.ValueType`).</span></span> <span data-ttu-id="ab3c0-345">如果成员不是实例成员，或者通过派生类的实例进行访问，则派生类可以访问 `Protected` 成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-345">A `Protected` member is accessible to a derived class, provided that either the member is not an instance member, or the access takes place through an instance of the derived class.</span></span> <span data-ttu-id="ab3c0-346">`Protected` 访问不是 `Friend` 访问的超集。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-346">`Protected` access is not a superset of `Friend` access.</span></span>

* <span data-ttu-id="ab3c0-347">使用 `Friend` 修饰符声明的实体具有 `Friend` 访问权限。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-347">Entities declared with the `Friend` modifier have `Friend` access.</span></span> <span data-ttu-id="ab3c0-348">只能在包含实体声明的程序中或通过 `System.Runtime.CompilerServices.InternalsVisibleToAttribute` 特性提供 `Friend` 访问的任何程序集的程序中访问具有 `Friend` 访问权限的实体。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-348">An entity with `Friend` access is accessible only within the program that contains the entity declaration or any assemblies that have been given `Friend` access through the `System.Runtime.CompilerServices.InternalsVisibleToAttribute` attribute.</span></span>

* <span data-ttu-id="ab3c0-349">使用 `Protected Friend` 修饰符声明的实体具有 `Protected` 和 `Friend` 访问的联合。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-349">Entities declared with the `Protected Friend` modifiers have the union of `Protected` and `Friend` access.</span></span>

* <span data-ttu-id="ab3c0-350">使用 `Private` 修饰符声明的实体具有 `Private` 访问权限。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-350">Entities declared with the `Private` modifier have `Private` access.</span></span> <span data-ttu-id="ab3c0-351">只能在其声明上下文内（包括任何嵌套实体）访问 `Private` 实体。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-351">A `Private` entity is accessible only within its declaration context, including any nested entities.</span></span>

<span data-ttu-id="ab3c0-352">声明中的可访问性不依赖于声明上下文的可访问性。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-352">The accessibility in a declaration does not depend on the accessibility of the declaration context.</span></span> <span data-ttu-id="ab3c0-353">例如，使用 `Private` 访问声明的类型可能包含具有 `Public` 访问权限的类型成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-353">For example, a type declared with `Private` access may contain a type member with `Public` access.</span></span>

<span data-ttu-id="ab3c0-354">以下代码演示了各种可访问域：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-354">The following code demonstrates various accessibility domains:</span></span>

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

<span data-ttu-id="ab3c0-355">此示例中的类和成员具有以下可访问域：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-355">The classes and members in this example have the following accessibility domains:</span></span>

* <span data-ttu-id="ab3c0-356">`A` 和 `A.X` 的可访问域是不受限制的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-356">The accessibility domain of `A` and `A.X` is unlimited.</span></span>

* <span data-ttu-id="ab3c0-357">`A.Y`、`B`、`B.X`、`B.Y`、`B.C`、`B.C.X`和 `B.C.Y` 的可访问域是包含程序。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-357">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the containing program.</span></span>

* <span data-ttu-id="ab3c0-358">`A.Z` 的可访问域 `A.`</span><span class="sxs-lookup"><span data-stu-id="ab3c0-358">The accessibility domain of `A.Z` is `A.`</span></span>

* <span data-ttu-id="ab3c0-359">`B.Z`、`B.D`、`B.D.X`和 `B.D.Y` 的可访问域是 `B`，其中包括 `B.C` 和 `B.D`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-359">The accessibility domain of `B.Z`, `B.D`, `B.D.X`, and `B.D.Y` is `B`, including `B.C` and `B.D`.</span></span>

* <span data-ttu-id="ab3c0-360">`B.C``B.C.Z` 的可访问域。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-360">The accessibility domain of `B.C.Z` is `B.C`.</span></span>

* <span data-ttu-id="ab3c0-361">`B.D``B.D.Z` 的可访问域。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-361">The accessibility domain of `B.D.Z` is `B.D`.</span></span>

<span data-ttu-id="ab3c0-362">如示例所示，成员的可访问域绝不会超出包含类型的可访问域。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-362">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="ab3c0-363">例如，即使所有 `X` 成员都 `Public` 声明的可访问性，但所有 `A.X` 都具有受包含类型约束的可访问域。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-363">For example, even though all `X` members have `Public` declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="ab3c0-364">对 `Protected` 实例成员的访问必须通过派生类型的实例，以便不相关的类型无法访问对方的每个受保护成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-364">Access to `Protected` instance members must be through an instance of the derived type so that unrelated types cannot gain access to each other's protected members.</span></span> <span data-ttu-id="ab3c0-365">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-365">For example:</span></span>

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

<span data-ttu-id="ab3c0-366">在上面的示例中，类 `Guest` 仅有权访问受保护的 `Password` 字段（如果它使用 `Guest`的实例限定）。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-366">In the above example, the class `Guest` only has access to the protected `Password` field if it is qualified with an instance of `Guest`.</span></span> <span data-ttu-id="ab3c0-367">这会阻止 `Guest` 仅通过将其强制转换为 `User`来访问 `Employee` 对象的 `Password` 字段。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-367">This prevents `Guest` from gaining access to the `Password` field of an `Employee` object simply by casting it to `User`.</span></span>

<span data-ttu-id="ab3c0-368">为了 `Protected` 泛型类型中的成员访问，声明上下文包括类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-368">For the purposes of `Protected` member access in generic types, the declaration context includes type parameters.</span></span> <span data-ttu-id="ab3c0-369">这意味着，具有一组类型参数的派生类型无法访问具有一组不同类型参数的派生类型的 `Protected` 成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-369">This means that a derived type with one set of type arguments does not have access to the `Protected` members of a derived type with a different set of type arguments.</span></span> <span data-ttu-id="ab3c0-370">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-370">For example:</span></span>

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

<span data-ttu-id="ab3c0-371">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ab3c0-371">__Note.__</span></span> <span data-ttu-id="ab3c0-372">无论C#提供哪些类型参数，语言（也可能还有其他语言）都允许泛型类型访问 `Protected` 成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-372">The C# language (and possibly other languages) allows a generic type to access `Protected` members regardless of what type arguments are supplied.</span></span> <span data-ttu-id="ab3c0-373">设计包含 `Protected` 成员的泛型类时，应牢记这一点。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-373">This should be kept in mind when designing generic classes that contain `Protected` members.</span></span>


### <a name="constituent-types"></a><span data-ttu-id="ab3c0-374">构成类型</span><span class="sxs-lookup"><span data-stu-id="ab3c0-374">Constituent Types</span></span>

<span data-ttu-id="ab3c0-375">声明的*构成类型*是声明引用的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-375">The *constituent types* of a declaration are the types that are referenced by the declaration.</span></span> <span data-ttu-id="ab3c0-376">例如，常量的类型、方法的返回类型和构造函数的参数类型均为构成类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-376">For example, the type of a constant, the return type of a method and the parameter types of a constructor are all constituent types.</span></span> <span data-ttu-id="ab3c0-377">声明的构成类型的可访问域必须与声明自身的可访问域的可访问域相同或超集。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-377">The accessibility domain of a constituent type of a declaration must be the same as or a superset of the accessibility domain of the declaration itself.</span></span> <span data-ttu-id="ab3c0-378">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-378">For example:</span></span>

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

## <a name="type-and-namespace-names"></a><span data-ttu-id="ab3c0-379">类型和命名空间名称</span><span class="sxs-lookup"><span data-stu-id="ab3c0-379">Type and Namespace Names</span></span>

<span data-ttu-id="ab3c0-380">许多语言构造需要指定命名空间或类型;可以通过使用命名空间或类型的名称的限定形式来指定这些。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-380">Many language constructs require a namespace or type to be specified; these can be specified by using a qualified form of the namespace or type's name.</span></span> <span data-ttu-id="ab3c0-381">*限定名*由一系列用句点分隔的标识符组成;句点右侧的标识符在由时间段左侧的标识符指定的声明空间中解析。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-381">A *qualified name* consists of a series of identifiers separated by periods; the identifier on the right side of a period is resolved in the declaration space specified by the identifier on the left side of the period.</span></span>

<span data-ttu-id="ab3c0-382">命名空间或类型的*完全限定名称*是一个限定名称，其中包含所有包含命名空间和类型的名称。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-382">The *fully qualified name* of a namespace or type is a qualified name that contains the name of all containing namespaces and types.</span></span> <span data-ttu-id="ab3c0-383">换言之，命名空间或类型的完全限定名是 `N.T`的，其中 `T` 是实体的名称，`N` 是其包含实体的完全限定名称。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-383">In other words, the fully qualified name of a namespace or type is `N.T`, where `T` is the name of the entity and `N` is the fully qualified name of its containing entity.</span></span>

<span data-ttu-id="ab3c0-384">下面的示例演示了多个命名空间和类型声明及其在内联注释中的完全限定名称。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-384">The example below shows several namespace and type declarations together with their associated fully qualified names in in-line comments.</span></span>

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

<span data-ttu-id="ab3c0-385">请注意，命名空间 X. Y 已在源代码中的两个不同位置声明，但这两个分部声明只包含一个名为 X.x 的命名空间，该命名空间同时包含类 D 和类 E。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-385">Observe that the namespace X.Y has been declared in two different locations in the source code, but these two partial declarations constitute just a single namespace called X.Y which contains both class D and class E.</span></span>

<span data-ttu-id="ab3c0-386">在某些情况下，限定名称可能以关键字 `Global`开头。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-386">In some situations, a qualified name may begin with the keyword `Global`.</span></span> <span data-ttu-id="ab3c0-387">关键字表示未命名的最外侧命名空间，这在声明隐藏封闭命名空间的情况下非常有用。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-387">The keyword represents the unnamed outermost namespace, which is useful in situations where a declaration shadows an enclosing namespace.</span></span> <span data-ttu-id="ab3c0-388">`Global` 关键字允许在这种情况下将 "转义" 到最外面的命名空间。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-388">The `Global` keyword allows "escaping" out to the outermost namespace in that situation.</span></span> <span data-ttu-id="ab3c0-389">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-389">For example:</span></span>

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

<span data-ttu-id="ab3c0-390">在上面的示例中，第一个方法调用无效，因为标识符 `System` 绑定到类 `System`，而不是命名空间 `System`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-390">In the above example, the first method call is invalid because the identifier `System` binds to the class `System`, not the namespace `System`.</span></span> <span data-ttu-id="ab3c0-391">访问 `System` 命名空间的唯一方法是使用 `Global` 来转义外部命名空间。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-391">The only way to access the `System` namespace is to use `Global` to escape out to the outermost namespace.</span></span> <span data-ttu-id="ab3c0-392">不能在 `Imports` 语句或 `Namespace` 声明中使用 `Global`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-392">`Global` cannot be used in an `Imports` statement or `Namespace` declaration.</span></span>

<span data-ttu-id="ab3c0-393">由于其他语言可能会引入与语言中的关键字匹配的类型和命名空间，因此 Visual Basic 将关键字识别为限定名称的一部分（只要它们遵循句点即可）。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-393">Because other languages may introduce types and namespaces that match keywords in the language, Visual Basic recognizes keywords to be part of a qualified name as long as they follow a period.</span></span> <span data-ttu-id="ab3c0-394">以这种方式使用的关键字被视为标识符。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-394">Keywords used in this way are treated as identifiers.</span></span> <span data-ttu-id="ab3c0-395">例如，限定的标识符 `X.Default.Class` 是有效的限定标识符，而 `Default.Class` 则不是。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-395">For example, the qualified identifier `X.Default.Class` is a valid qualified identifier, while `Default.Class` is not.</span></span>

### <a name="qualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="ab3c0-396">命名空间和类型的限定名称解析</span><span class="sxs-lookup"><span data-stu-id="ab3c0-396">Qualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="ab3c0-397">给定形式 `N.R(Of A)`的限定命名空间或类型名称，其中 `R` 是限定名称中最右边的标识符，而 `A` 是可选的类型参数列表，以下步骤描述如何确定限定名称是指哪个命名空间或类型：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-397">Given a qualified namespace or type name of the form `N.R(Of A)`, where `R` is the rightmost identifier in the qualified name and `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the qualified name refers:</span></span>

1. <span data-ttu-id="ab3c0-398">使用限定或非限定名称解析的规则解析 `N`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-398">Resolve `N`, using the rules for either qualified or unqualified name resolution.</span></span>

2. <span data-ttu-id="ab3c0-399">如果 `N` 的解析失败或解析为类型参数，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-399">If resolution of `N` fails, or resolves to a type parameter, a compile-time error occurs.</span></span>

3. <span data-ttu-id="ab3c0-400">否则，如果 `R` 与 N 中的命名空间的名称相匹配，但未提供任何类型实参，或 `R` 与 `N` 的类型形参相同的可访问类型（如果有），则限定名称指该命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-400">Otherwise, if `R` matches the name of a namespace in N and no type arguments were supplied, or `R` matches an accessible type in `N` with the same number of type parameters as type arguments, if any, then the qualified name refers to that namespace or type.</span></span>

4. <span data-ttu-id="ab3c0-401">否则，如果 `N` 包含一个或多个标准模块，并且 `R` 与具有相同数量的类型参数（如果有）的类型参数与类型参数（如果有）的名称相匹配，则限定名称指该类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-401">Otherwise, if `N` contains one or more standard modules, and `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the qualified name refers to that type.</span></span> <span data-ttu-id="ab3c0-402">如果 `R` 将具有相同数量的类型参数的可访问类型的名称与类型参数（如果有）的名称相匹配，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-402">If `R` matches the name of accessible types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

5. <span data-ttu-id="ab3c0-403">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-403">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ab3c0-404">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ab3c0-404">__Note.__</span></span> <span data-ttu-id="ab3c0-405">此解析过程的含义是，类型成员在解析命名空间或类型名称时不会隐藏命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-405">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

### <a name="unqualified-name-resolution-for-namespaces-and-types"></a><span data-ttu-id="ab3c0-406">命名空间和类型的非限定名称解析</span><span class="sxs-lookup"><span data-stu-id="ab3c0-406">Unqualified Name Resolution for namespaces and types</span></span>

<span data-ttu-id="ab3c0-407">如果给定了一个不限定的名称 `R(Of A)`（其中 `A` 是可选的类型参数列表），以下步骤将介绍如何确定非限定名称所引用的命名空间或类型：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-407">Given an unqualified name `R(Of A)`, where `A` is an optional type argument list, the following steps describe how to determine to which namespace or type the unqualified name refers:</span></span>

1. <span data-ttu-id="ab3c0-408">如果 R 与当前方法的类型参数的名称匹配，并且未提供任何类型参数，则非限定名称引用该类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-408">If R matches the name of a type parameter of the current method, and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>

2.  <span data-ttu-id="ab3c0-409">对于包含名称引用的每个嵌套类型，从最内层的类型开始，转到最外层：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-409">For each nested type containing the name reference, starting from the innermost type and going to the outermost:</span></span>
    1. <span data-ttu-id="ab3c0-410">如果 `R` 与当前类型中的类型参数的名称匹配，并且未提供任何类型参数，则非限定名称将引用该类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-410">If `R` matches the name of a type parameter in the current type and no type arguments were supplied, then the unqualified name refers to that type parameter.</span></span>
    2. <span data-ttu-id="ab3c0-411">否则，如果 `R` 将具有相同数量的类型参数的可访问嵌套类型的名称匹配为类型参数（如果有），则非限定名称将引用该类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-411">Otherwise, if `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, then the unqualified name refers to that type.</span></span>

3. <span data-ttu-id="ab3c0-412">对于包含名称引用的每个嵌套命名空间，从最内层命名空间开始，转到最外面的命名空间：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-412">For each nested namespace containing the name reference, starting from the innermost namespace and going to the outermost namespace:</span></span>
    1. <span data-ttu-id="ab3c0-413">如果 `R` 与当前命名空间中的嵌套命名空间的名称匹配，并且未提供类型参数列表，则非限定名称将引用该嵌套命名空间。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-413">If `R` matches the name of a nested namespace in the current namespace and no type argument list is supplied, then the unqualified name refers to that nested namespace.</span></span>
    2. <span data-ttu-id="ab3c0-414">否则，如果 `R` 将具有相同数量的类型参数的可访问类型的名称与当前命名空间中的类型参数（如果有）相匹配，则非限定名称将引用该类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-414">Otherwise, if `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in the current namespace, then the unqualified name refers to that type.</span></span>
    3. <span data-ttu-id="ab3c0-415">否则，如果命名空间包含一个或多个可访问的标准模块，并且 `R` 与具有相同数量的类型参数（如果有）的类型参数与类型参数（如果有）的名称相匹配，则非限定名称将引用该嵌套类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-415">Otherwise, if the namespace contains one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that nested type.</span></span> <span data-ttu-id="ab3c0-416">如果 `R` 将具有相同数量的类型参数的可访问嵌套类型的名称与类型参数（如果有）的名称相匹配，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-416">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

4. <span data-ttu-id="ab3c0-417">如果源文件有一个或多个导入别名，并且 `R` 与其中一个别名的名称匹配，则非限定名称将引用该导入别名。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-417">If the source file has one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="ab3c0-418">如果提供了类型参数列表，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-418">If a type argument list is supplied, a compile-time error occurs.</span></span>

5. <span data-ttu-id="ab3c0-419">如果包含名称引用的源文件有一个或多个导入：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-419">If the source file containing the name reference has one or more imports:</span></span>
    1. <span data-ttu-id="ab3c0-420">如果 `R` 将具有相同数量的类型参数的可访问类型的名称与类型参数（如果有）的名称相匹配（如果有），则只会将该类型命名为。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-420">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="ab3c0-421">如果 `R` 匹配具有与类型参数相同数量的类型参数（如果有）的可访问类型的名称（如果有），则在多个导入和全部都不属于同一类型时，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-421">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import and all are not the same type, a compile-time error occurs.</span></span>
    2. <span data-ttu-id="ab3c0-422">否则，如果未提供任何类型参数列表，并且 `R` 与只包含一个导入的可访问类型的命名空间的名称匹配，则非限定名称将引用该命名空间。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-422">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="ab3c0-423">如果未提供任何类型参数列表，并且 `R` 与具有可访问类型的命名空间的名称相匹配，但在多个导入中，所有不是同一个命名空间，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-423">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import and all are not the same namespace, a compile-time error occurs.</span></span>
    3. <span data-ttu-id="ab3c0-424">否则，如果导入包含一个或多个可访问的标准模块，并且 `R` 与具有相同数量的类型参数（如果有）的类型参数与类型参数（如果有）的名称相匹配，则非限定名称引用该类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-424">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="ab3c0-425">如果 `R` 将具有相同数量的类型参数的可访问嵌套类型的名称与类型参数（如果有）的名称相匹配，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-425">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

6. <span data-ttu-id="ab3c0-426">如果编译环境定义了一个或多个导入别名，并且 `R` 匹配其中一个的名称，则非限定名称将引用该导入别名。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-426">If the compilation environment defines one or more import aliases, and `R` matches the name of one of them, then the unqualified name refers to that import alias.</span></span> <span data-ttu-id="ab3c0-427">如果提供了类型参数列表，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-427">If a type argument list is supplied, a compile-time error occurs.</span></span>

7. <span data-ttu-id="ab3c0-428">如果编译环境定义了一个或多个导入：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-428">If the compilation environment defines one or more imports:</span></span>
    1. <span data-ttu-id="ab3c0-429">如果 `R` 将具有相同数量的类型参数的可访问类型的名称与类型参数（如果有）的名称相匹配（如果有），则只会将该类型命名为。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-429">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in exactly one import, then the unqualified name refers to that type.</span></span> <span data-ttu-id="ab3c0-430">如果 `R` 将具有相同数量的类型参数的可访问类型的名称与类型参数（如果有）相匹配，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-430">If `R` matches the name of an accessible type with the same number of type parameters as type arguments, if any, in more than one import, a compile-time error occurs.</span></span>
    2. <span data-ttu-id="ab3c0-431">否则，如果未提供任何类型参数列表，并且 `R` 与只包含一个导入的可访问类型的命名空间的名称匹配，则非限定名称将引用该命名空间。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-431">Otherwise, if no type argument list was supplied and `R` matches the name of a namespace with accessible types in exactly one import, then the unqualified name refers to that namespace.</span></span> <span data-ttu-id="ab3c0-432">如果未提供任何类型参数列表，并且 `R` 与具有可访问类型的命名空间的名称相匹配，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-432">If no type argument list was supplied and `R` matches the name of a namespace with accessible types in more than one import, a compile-time error occurs.</span></span>
    3. <span data-ttu-id="ab3c0-433">否则，如果导入包含一个或多个可访问的标准模块，并且 `R` 与具有相同数量的类型参数（如果有）的类型参数与类型参数（如果有）的名称相匹配，则非限定名称引用该类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-433">Otherwise, if the imports contain one or more accessible standard modules, and `R` matches the name of an accessible nested type with the same number of type parameters as type arguments, if any, in exactly one standard module, then the unqualified name refers to that type.</span></span> <span data-ttu-id="ab3c0-434">如果 `R` 将具有相同数量的类型参数的可访问嵌套类型的名称与类型参数（如果有）的名称相匹配，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-434">If `R` matches the name of accessible nested types with the same number of type parameters as type arguments, if any, in more than one standard module, a compile-time error occurs.</span></span>

8. <span data-ttu-id="ab3c0-435">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-435">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ab3c0-436">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ab3c0-436">__Note.__</span></span> <span data-ttu-id="ab3c0-437">此解析过程的含义是，类型成员在解析命名空间或类型名称时不会隐藏命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-437">An implication of this resolution process is that type members do not shadow namespaces or types when resolving namespace or type names.</span></span>

<span data-ttu-id="ab3c0-438">通常，名称只能在特定命名空间中出现一次。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-438">Normally, a name can only occur once in a particular namespace.</span></span> <span data-ttu-id="ab3c0-439">但是，由于命名空间可以在多个 .NET 程序集中进行声明，因此在两个程序集定义具有相同完全限定名称的类型时，可能会出现这种情况。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-439">However, because namespaces can be declared across multiple .NET assemblies, it is possible to have a situation where two assemblies define a type with the same fully qualified name.</span></span> <span data-ttu-id="ab3c0-440">在这种情况下，当前源文件集中声明的类型优先于在外部 .NET 程序集中声明的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-440">In that case, a type declared in the current set of source files is preferred over a type declared in an external .NET assembly.</span></span> <span data-ttu-id="ab3c0-441">否则，名称是不明确的，没有办法消除名称的歧义。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-441">Otherwise, the name is ambiguous and there is no way to disambiguate the name.</span></span>

## <a name="variables"></a><span data-ttu-id="ab3c0-442">变量</span><span class="sxs-lookup"><span data-stu-id="ab3c0-442">Variables</span></span>

<span data-ttu-id="ab3c0-443">*变量*表示存储位置。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-443">A *variable* represents a storage location.</span></span> <span data-ttu-id="ab3c0-444">每个变量都有一个类型，用于确定可以在变量中存储的值。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-444">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="ab3c0-445">由于 Visual Basic 是一种类型安全的语言，因此，程序中的每个变量都具有类型，并且该语言保证变量中存储的值始终为适当的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-445">Because Visual Basic is a type-safe language, every variable in a program has a type and the language guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="ab3c0-446">变量始终初始化为其类型的默认值，然后才可以对变量进行引用。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-446">Variables are always initialized to the default value of their type before any reference to the variable can be made.</span></span> <span data-ttu-id="ab3c0-447">不能访问未初始化的内存。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-447">It is not possible to access uninitialized memory.</span></span>

## <a name="generic-types-and-methods"></a><span data-ttu-id="ab3c0-448">泛型类型和方法</span><span class="sxs-lookup"><span data-stu-id="ab3c0-448">Generic Types and Methods</span></span>

<span data-ttu-id="ab3c0-449">类型（标准模块和枚举类型除外）和方法可以声明*类型参数，类型参数*是在声明类型的实例或调用方法之前将不提供的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-449">Types (except for standard modules and enumerated types) and methods can declare *type parameters*, which are types that will not be provided until an instance of the type is declared or the method is invoked.</span></span> <span data-ttu-id="ab3c0-450">具有类型参数的类型和方法也称为*泛型类型*和*泛型方法*，因为必须一般编写类型或方法，而不会对使用类型或方法的代码所提供的类型进行特定的了解。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-450">Types and methods with type parameters are also known as *generic types* and *generic methods*, respectively, because the type or method must be written generically, without specific knowledge of the types that will be supplied by code that uses the type or method.</span></span>

<span data-ttu-id="ab3c0-451">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ab3c0-451">__Note.__</span></span> <span data-ttu-id="ab3c0-452">目前，即使方法和委托可以是泛型，属性、事件和运算符本身也不能是泛型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-452">At this time, even though methods and delegates can be generic, properties, events and operators cannot be generic themselves.</span></span> <span data-ttu-id="ab3c0-453">但是，它们可以使用包含类中的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-453">They may, however, use type parameters from the containing class.</span></span>

<span data-ttu-id="ab3c0-454">从泛型类型或方法的角度来看，类型参数是一个占位符类型，它将在使用类型或方法时用实际类型填充。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-454">From the perspective of the generic type or method, a type parameter is a placeholder type that will be filled in with an actual type when the type or method is used.</span></span> <span data-ttu-id="ab3c0-455">在使用类型或方法的点处，类型参数替换类型或方法中的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-455">Type arguments are substituted for the type parameters in the type or method at the point at which the type or method is used.</span></span> <span data-ttu-id="ab3c0-456">例如，可以将泛型 stack 类实现为：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-456">For example, a generic stack class could be implemented as:</span></span>

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

<span data-ttu-id="ab3c0-457">使用 `Stack(Of ItemType)` 类的声明必须为类型参数 `ItemType`提供类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-457">Declarations that use the `Stack(Of ItemType)` class must supply a type argument for the type parameter `ItemType`.</span></span> <span data-ttu-id="ab3c0-458">然后，将在类中使用 `ItemType` 的任何位置填充此类型：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-458">This type is then filled in wherever `ItemType` is used within the class:</span></span>

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

### <a name="type-parameters"></a><span data-ttu-id="ab3c0-459">类型参数</span><span class="sxs-lookup"><span data-stu-id="ab3c0-459">Type Parameters</span></span>

<span data-ttu-id="ab3c0-460">类型参数可以在类型或方法声明上提供。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-460">Type parameters may be supplied on type or method declarations.</span></span> <span data-ttu-id="ab3c0-461">每个类型参数都是一个标识符，它是为创建构造类型或方法提供的类型自变量的占位符。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-461">Each type parameter is an identifier which is a place-holder for a type argument that is supplied to create a constructed type or method.</span></span> <span data-ttu-id="ab3c0-462">与此相反，类型参数是在使用泛型类型或方法时替换类型参数的实际类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-462">By contrast, a type argument is the actual type that is substituted for the type parameter when a generic type or method is used.</span></span>

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

<span data-ttu-id="ab3c0-463">类型或方法声明中的每个类型参数在该类型或方法的声明空间中定义一个名称。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-463">Each type parameter in a type or method declaration defines a name in the declaration space of that type or method.</span></span> <span data-ttu-id="ab3c0-464">因此，它不能与另一个类型参数、类型成员、方法参数或局部变量具有相同的名称。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-464">Thus, it cannot have the same name as another type parameter, a type member, a method parameter, or a local variable.</span></span> <span data-ttu-id="ab3c0-465">类型或方法的类型参数的作用域是整个类型或方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-465">The scope of a type parameter on a type or method is the entire type or method.</span></span> <span data-ttu-id="ab3c0-466">由于类型参数的作用域为整个类型声明，因此嵌套类型可以使用外部类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-466">Because type parameters are scoped to the entire type declaration, nested types can use outer type parameters.</span></span> <span data-ttu-id="ab3c0-467">这也意味着在访问嵌套在泛型类型中的类型时，必须始终指定类型参数：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-467">This also means that type parameters must always be specified when accessing types nested inside generic types:</span></span>

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

<span data-ttu-id="ab3c0-468">与类的其他成员不同，类型参数不能继承。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-468">Unlike other members of a class, type parameters are not inherited.</span></span> <span data-ttu-id="ab3c0-469">类型中的类型参数只能由其简单名称引用;换句话说，不能用包含类型名称对它们进行限定。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-469">Type parameters in a type can only be referred to by their simple name; in other words, they cannot be qualified with the containing type name.</span></span> <span data-ttu-id="ab3c0-470">尽管编程样式不正确，但嵌套类型中的类型参数可以隐藏在外部类型中声明的成员或类型参数：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-470">Although it is bad programming style, the type parameters in a nested type can hide a member or type parameter declared in the outer type:</span></span>

```vb
Class Outer(Of T)
    Class Inner(Of T)
        Public t1 As T    ' Refers to Inner's T
    End Class
End Class
```

<span data-ttu-id="ab3c0-471">类型和方法可基于类型或方法声明的类型参数（或*arity*）的数目进行重载。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-471">Types and methods may be overloaded based on the number of type parameters (or *arity*) that the types or methods declare.</span></span> <span data-ttu-id="ab3c0-472">例如，以下声明是合法的：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-472">For example, the following declarations are legal:</span></span>

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

<span data-ttu-id="ab3c0-473">对于类型，重载始终与指定的类型参数的数目匹配。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-473">In the case of types, overloads are always matched against the number of type arguments specified.</span></span> <span data-ttu-id="ab3c0-474">在同一程序中同时使用泛型和非泛型类时，这很有用：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-474">This is useful when using both generic and non-generic classes together in the same program:</span></span>

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

<span data-ttu-id="ab3c0-475">方法重载决策部分介绍了类型参数上的重载方法规则。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-475">Rules for methods overloaded on type parameters are covered in the section on method overload resolution.</span></span>

<span data-ttu-id="ab3c0-476">在包含声明中，类型参数被视为完全类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-476">Within the containing declaration, type parameters are considered full types.</span></span> <span data-ttu-id="ab3c0-477">由于类型参数可以使用许多不同的实际类型参数进行实例化，因此类型参数与其他类型的操作和限制略有不同，如下所述：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-477">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types as described below:</span></span>

* <span data-ttu-id="ab3c0-478">类型参数不能直接用于声明基类或接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-478">A type parameter cannot be used directly to declare a base class or interface.</span></span>

* <span data-ttu-id="ab3c0-479">对类型参数进行成员查找的规则取决于应用于该类型参数的约束（如果有）。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-479">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span>

* <span data-ttu-id="ab3c0-480">类型参数的可用转换取决于应用于类型参数的约束（如果有）。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-480">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameters.</span></span>

* <span data-ttu-id="ab3c0-481">如果没有 `Structure` 约束，则可以使用 `Is` 和 `IsNot`将类型参数表示的值与 `Nothing` 进行比较。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-481">In the absence of a `Structure` constraint, a value with a type represented by a type parameter can be compared with `Nothing` using `Is` and `IsNot`.</span></span>

* <span data-ttu-id="ab3c0-482">如果类型参数受 `New` 或 `Structure` 约束的约束，则只能在 `New` 表达式中使用类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-482">A type parameter can only be used in a `New` expression if the type parameter is constrained by a `New` or a `Structure` constraint.</span></span>

* <span data-ttu-id="ab3c0-483">类型参数不能在 `GetType` 表达式内的属性异常中的任何位置使用。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-483">A type parameter cannot be used anywhere within an attribute exception within a `GetType` expression.</span></span>

* <span data-ttu-id="ab3c0-484">类型参数可用作其他泛型类型和参数的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-484">Type parameters can be used as type arguments to other generic types and parameters.</span></span>

<span data-ttu-id="ab3c0-485">下面的示例是一个扩展 `Stack(Of ItemType)` 类的泛型类型：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-485">The following example is a generic type that extends the `Stack(Of ItemType)` class:</span></span>

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

<span data-ttu-id="ab3c0-486">当声明向 `MyStack`提供类型参数时，相同的类型参数也将应用于 `Stack`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-486">When a declaration supplies a type argument to `MyStack`, the same type argument will be applied to `Stack` as well.</span></span>

<span data-ttu-id="ab3c0-487">类型参数只是一种编译时构造。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-487">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="ab3c0-488">在运行时，每个类型参数都绑定到一个运行时类型，该类型是通过向泛型声明提供类型参数来指定的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-488">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic declaration.</span></span> <span data-ttu-id="ab3c0-489">因此，使用类型参数声明的变量的类型将在运行时为非泛型类型或特定构造类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-489">Thus, the type of a variable declared with a type parameter will, at run-time, be a non-generic type or a specific constructed type.</span></span> <span data-ttu-id="ab3c0-490">所有涉及类型参数的语句和表达式的运行时执行都使用作为该参数的类型参数提供的实际类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-490">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>


### <a name="type-constraints"></a><span data-ttu-id="ab3c0-491">类型约束</span><span class="sxs-lookup"><span data-stu-id="ab3c0-491">Type Constraints</span></span>

<span data-ttu-id="ab3c0-492">因为类型参数可以是类型系统中的任何类型，所以泛型类型或方法无法对类型参数作出任何假设。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-492">Because a type argument can be any type in the type system, a generic type or method cannot make any assumptions about a type parameter.</span></span> <span data-ttu-id="ab3c0-493">因此，类型参数的成员被视为 `Object`类型的成员，因为所有类型都派生自 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-493">Thus, the members of a type parameter are considered to be the members of the type `Object`, since all types derive from `Object`.</span></span>

<span data-ttu-id="ab3c0-494">对于诸如 `Stack(Of ItemType)`的集合，这种情况可能不是特别重要的限制，但在某些情况下，泛型类型可能想要对将作为类型参数提供的类型做出假设。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-494">In the case of a collection like `Stack(Of ItemType)`, this fact may not be a particularly important restriction, but there may be cases where a generic type may wish to make an assumption about the types that will be supplied as type arguments.</span></span> <span data-ttu-id="ab3c0-495">*类型约束*可放置在类型参数上，该类型参数限制可以作为类型参数提供的类型，并允许泛型类型或方法更多地使用类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-495">*Type constraints* can be placed on type parameters that restrict which types can be supplied as a type parameter and allow generic types or methods to assume more about type parameters.</span></span>

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

<span data-ttu-id="ab3c0-496">在此示例中，`DisposableStack(Of ItemType)` 仅将其类型参数限制为实现接口 `System.IDisposable`的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-496">In this example, the `DisposableStack(Of ItemType)` constrains its type parameter to only types that implement the interface `System.IDisposable`.</span></span> <span data-ttu-id="ab3c0-497">因此，它可以实现 `Dispose` 方法，该方法可释放仍保留在队列中的任何对象。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-497">As a result, it can implement a `Dispose` method that disposes any objects still left in the queue.</span></span>

<span data-ttu-id="ab3c0-498">类型约束必须是特殊约束之一 `Class`、`Structure`或 `New`，或者必须是一个 `T` 的类型其中：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-498">A type constraint must be one of the special constraints `Class`, `Structure`, or `New`, or it must be a type `T` where:</span></span>

* <span data-ttu-id="ab3c0-499">`T` 是类、接口或类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-499">`T` is a class, an interface, or a type parameter.</span></span>

* <span data-ttu-id="ab3c0-500">`T` 不是 `NotInheritable`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-500">`T` is not `NotInheritable`.</span></span>

* <span data-ttu-id="ab3c0-501">`T` 不是或继承自以下特殊类型之一的类型： `System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`或 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-501">`T` is not one of, or a type inherited from one of, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span>

* <span data-ttu-id="ab3c0-502">`T` 不是 `Object`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-502">`T` is not `Object`.</span></span> <span data-ttu-id="ab3c0-503">由于所有类型都派生自 `Object`，因此如果允许，此类约束将不起作用。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-503">Since all types derive from `Object`, such a constraint would have no effect if it were permitted.</span></span>

* <span data-ttu-id="ab3c0-504">`T` 必须至少与要声明的泛型类型或方法相同。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-504">`T` must be at least as accessible as the generic type or method being declared.</span></span>

<span data-ttu-id="ab3c0-505">可以通过将类型约束括在大括号（`{}`）中，为单个类型形参指定多个类型约束。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-505">Multiple type constraints can be specified for a single type parameter by enclosing the type constraints in curly braces (`{}`)..</span></span> <span data-ttu-id="ab3c0-506">给定类型参数仅有一个类型约束可以为类。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-506">Only one type constraint for a given type parameter can be a class.</span></span> <span data-ttu-id="ab3c0-507">将 `Structure` 特殊约束与命名类约束或 `Class` 特殊约束合并是错误的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-507">It is an error to combine a `Structure` special constraint with a named class constraint or the `Class` special constraint.</span></span>

```vb
Class ControlFactory(Of T As {Control, New})
    ...
End Class
```

<span data-ttu-id="ab3c0-508">类型约束可以使用包含类型或任何包含类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-508">Type constraints can use the containing types or any of the containing types' type parameters.</span></span> <span data-ttu-id="ab3c0-509">在下面的示例中，约束要求提供的类型参数实现泛型接口，并使用其自身作为类型参数：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-509">In the following example, the constraint requires that the type argument supplied implements a generic interface using itself as a type argument:</span></span>

```vb
Class Sorter(Of V As IComparable(Of V))
    ...
End Class
```

<span data-ttu-id="ab3c0-510">特殊类型约束 `Class` 将所提供的类型参数约束到任何引用类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-510">The special type constraint `Class` constrains the supplied type argument to any reference type.</span></span>

<span data-ttu-id="ab3c0-511">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ab3c0-511">__Note.__</span></span> <span data-ttu-id="ab3c0-512">特定类型约束 `Class` 可以通过接口满足。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-512">The special type constraint `Class` can be satisfied by an interface.</span></span> <span data-ttu-id="ab3c0-513">结构可以实现接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-513">And a structure can implement an interface.</span></span> <span data-ttu-id="ab3c0-514">因此，在 "T" 结构（不满足 "`Class` 特殊" 约束）和 "U" 实现的接口（它满足 `Class` 特殊约束）的情况下，可能满足约束 `(Of T As U, U As Class)`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-514">Therefore, the constraint `(Of T As U, U As Class)` might be satisfied with "T" a structure (which does not satisfy the `Class` special constraint), and "U" an interface that it implements (which does satisfy the `Class` special constraint).</span></span>

<span data-ttu-id="ab3c0-515">特殊类型约束 `Structure` 将提供的类型参数约束为除 `System.Nullable(Of T)`以外的任何值类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-515">The special type constraint `Structure` constrains the supplied type argument to any value type except `System.Nullable(Of T)`.</span></span>

<span data-ttu-id="ab3c0-516">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ab3c0-516">__Note.__</span></span> <span data-ttu-id="ab3c0-517">结构约束不允许 `System.Nullable(Of T)`，因此无法将 `System.Nullable(Of T)` 作为其自身的类型参数提供。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-517">Structure constraints do not allow `System.Nullable(Of T)` so that it is not possible to supply `System.Nullable(Of T)` as a type argument to itself.</span></span>

<span data-ttu-id="ab3c0-518">特殊类型约束 `New` 要求提供的类型参数必须具有可访问的无参数构造函数，并且不能 `MustInherit`声明。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-518">The special type constraint `New` requires that the supplied type argument must have an accessible parameterless constructor and cannot be declared `MustInherit`.</span></span> <span data-ttu-id="ab3c0-519">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-519">For example:</span></span>

```vb
Class Factory(Of T As New)
    Function CreateInstance() As T
        Return New T()
    End Function
End Class
```

<span data-ttu-id="ab3c0-520">类类型约束要求提供的类型参数必须是该类型或从其继承。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-520">A class type constraint requires that the supplied type argument must either be that type as or inherit from it.</span></span> <span data-ttu-id="ab3c0-521">接口类型约束要求提供的类型参数必须实现该接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-521">An interface type constraint requires that the supplied type argument must implement that interface.</span></span> <span data-ttu-id="ab3c0-522">类型参数约束要求提供的类型参数必须派生自或实现为匹配的类型参数提供的所有边界。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-522">A type parameter constraint requires that the supplied type argument must derive from or implement all of the bounds given for the matching type parameter.</span></span> <span data-ttu-id="ab3c0-523">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-523">For example:</span></span>

```vb
Class List(Of T)
    Sub AddRange(Of S As T)(collection As IEnumerable(Of S))
        ...
    End Sub
End Class
```

<span data-ttu-id="ab3c0-524">在此示例中，`AddRange` 上 `S` 的类型参数被限制为 `List`的类型参数 `T`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-524">In this example, the type parameter `S` on `AddRange` is constrained to the type parameter `T` of `List`.</span></span> <span data-ttu-id="ab3c0-525">这意味着，`List(Of Control)` 会将 `AddRange`的类型参数约束为任何从 `Control`继承或继承的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-525">This means that a `List(Of Control)` would constrain `AddRange`'s type parameter to any type that is or inherits from `Control`.</span></span>

<span data-ttu-id="ab3c0-526">通过在除特殊约束（`Class`、`Structure`、`New`）以外的 `S`上传递所有 `T`的约束，可解析类型参数约束 `Of S As T`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-526">A type parameter constraint `Of S As T` is resolved by transitively adding all of `T`'s constraints onto `S`, other than the special constraints (`Class`, `Structure`, `New`).</span></span> <span data-ttu-id="ab3c0-527">具有循环约束（例如 `Of S As T, T As S`）是错误的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-527">It is an error to have circular constraints (e.g. `Of S As T, T As S`).</span></span> <span data-ttu-id="ab3c0-528">如果类型参数约束本身具有 `Structure` 约束，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-528">It is an error to have a type parameter constraint which itself has the `Structure` constraint.</span></span> <span data-ttu-id="ab3c0-529">添加约束后，可能会出现很多特殊情况：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-529">After adding constraints, it is possible that a number of special situations may occur:</span></span>

* <span data-ttu-id="ab3c0-530">如果存在多个类约束，派生程度最高的类将被视为约束。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-530">If multiple class constraints exist, the most derived class is considered to be the constraint.</span></span> <span data-ttu-id="ab3c0-531">如果一个或多个类约束没有继承关系，则约束为满足，这是一个错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-531">If one or more class constraints have no inheritance relationship, the constraint is unsatisfiable and it is an error.</span></span>

 * <span data-ttu-id="ab3c0-532">如果类型参数将 `Structure` 特殊约束与命名类约束或 `Class` 特殊约束合并，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-532">If a type parameter combines a `Structure` special constraint with a named class constraint or the `Class` special constraint, it is an error.</span></span> <span data-ttu-id="ab3c0-533">类约束可以 `NotInheritable`，在这种情况下，将不接受该约束的派生类型并且它是错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-533">A class constraint may be `NotInheritable`, in which case no derived types of that constraint are accepted and it is an error.</span></span>

<span data-ttu-id="ab3c0-534">该类型可以是或从继承的类型之一，以下特殊类型： `System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`或 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-534">The type may be one of, or a type inherited from, the following special types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="ab3c0-535">在这种情况下，只接受类型或从其继承的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-535">In that case, only the type, or a type inherited from it, is accepted.</span></span> <span data-ttu-id="ab3c0-536">约束为这些类型之一的类型参数只能使用 `DirectCast` 运算符允许的转换。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-536">A type parameter constrained to one of these types can only use the conversions allowed by the `DirectCast` operator.</span></span> <span data-ttu-id="ab3c0-537">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-537">For example:</span></span>

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

<span data-ttu-id="ab3c0-538">此外，由于上述某个松弛法，类型参数被限制为值类型，因此无法调用对该值类型定义的任何方法。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-538">Additionally, a type parameter constrained to a value type due to one of the above relaxations cannot call any methods defined on that value type.</span></span> <span data-ttu-id="ab3c0-539">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-539">For example:</span></span>

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

<span data-ttu-id="ab3c0-540">如果在替换后，约束结束为数组类型，则还允许任何协变数组类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-540">If the constraint, after substitution, ends up as an array type, any covariant array type is allowed as well.</span></span> <span data-ttu-id="ab3c0-541">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-541">For example:</span></span>

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

<span data-ttu-id="ab3c0-542">具有类或接口约束的类型参数被视为具有与该类或接口约束相同的成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-542">A type parameter with a class or interface constraint is considered to have the same members as that class or interface constraint.</span></span> <span data-ttu-id="ab3c0-543">如果类型参数具有多个约束，则类型参数被视为具有约束的所有成员的并集。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-543">If a type parameter has multiple constraints, then the type parameter is considered to have the union of all the members of the constraints.</span></span> <span data-ttu-id="ab3c0-544">如果有多个约束中具有相同名称的成员，则成员将按以下顺序隐藏：类约束隐藏接口约束中的成员，这些成员隐藏 `System.ValueType` 中的成员（如果指定了 `Structure` 约束），这将隐藏 `Object`中的成员。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-544">If there are members with the same name in more than one constraint, then members are hidden in the following order: the class constraint hides members in interface constraints, which hide members in `System.ValueType` (if `Structure` constraint is specified), which hides members in `Object`.</span></span> <span data-ttu-id="ab3c0-545">如果具有相同名称的成员出现在多个接口约束中，则成员不可用（如在多个接口继承中），并且类型参数必须强制转换为所需的接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-545">If a member with the same name appears in more than one interface constraint the member is unavailable (as in multiple interface inheritance) and the type parameter must be cast to the desired interface.</span></span> <span data-ttu-id="ab3c0-546">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-546">For example:</span></span>

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

<span data-ttu-id="ab3c0-547">当提供类型参数作为类型参数时，类型参数必须满足匹配类型参数的约束。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-547">When supplying type parameters as type arguments, the type parameters must satisfy the constraints of the matching type parameters.</span></span>

```vb
Class Base(Of T As Class)
End Class

Class Derived(Of V)
    ' Error: V does not satisfy the constraints of T
    Inherits Base(Of V)
End Class
```

<span data-ttu-id="ab3c0-548">受约束的类型参数的值可用于访问在约束中指定的实例成员（包括实例方法）。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-548">Values of a constrained type parameter can be used to access the instance members, including instance methods, specified in the constraint.</span></span>

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

### <a name="type-parameter-variance"></a><span data-ttu-id="ab3c0-549">类型参数的方差</span><span class="sxs-lookup"><span data-stu-id="ab3c0-549">Type Parameter Variance</span></span>

<span data-ttu-id="ab3c0-550">接口或委托类型声明中的类型参数可以选择指定*方差修饰符*。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-550">A type parameter in an interface or a delegate type declaration can optionally specify a *variance modifier*.</span></span> <span data-ttu-id="ab3c0-551">具有变体修饰符的类型参数限制了类型参数可在接口或委托类型中的使用方式，但允许泛型接口或委托类型转换为具有变体兼容类型参数的另一个泛型类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-551">Type parameters with variance modifiers restrict how the type parameter can be used in the interface or delegate type but allow a generic interface or delegate type to be converted to another generic type with variant compatible type arguments.</span></span> <span data-ttu-id="ab3c0-552">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-552">For example:</span></span>

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

<span data-ttu-id="ab3c0-553">具有具有变体修饰符的类型参数的泛型接口具有几个限制：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-553">Generic interfaces that have type parameters with variance modifiers have several restrictions:</span></span>

* <span data-ttu-id="ab3c0-554">它们不能包含指定参数列表的事件声明（但允许使用委托类型的自定义事件声明或事件声明）。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-554">They cannot contain an event declaration that specifies a parameter list (but a custom event declaration or an event declaration with a delegate type is allowed).</span></span>

* <span data-ttu-id="ab3c0-555">它们不能包含嵌套的类、结构或枚举类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-555">They cannot contain a nested class, structure, or enumerated type.</span></span>

<span data-ttu-id="ab3c0-556">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ab3c0-556">__Note.__</span></span> <span data-ttu-id="ab3c0-557">这些限制的原因在于，嵌套在泛型类型中的类型隐式复制其父级的泛型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-557">These restrictions are due to the fact that types nested in generic types implicitly copy the generic parameters of their parent.</span></span> <span data-ttu-id="ab3c0-558">对于嵌套类、结构或枚举类型，这些类型的类型不能对其类型参数具有变体修饰符。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-558">In the case of nested classes, structures, or enumerated types, those kinds of types cannot have variance modifiers on their type parameters.</span></span> <span data-ttu-id="ab3c0-559">对于带有参数列表的事件声明，生成的嵌套委托类在 `In` 位置（即参数类型）中实际使用的类型实际用于 `Out` 位置（即事件的类型）时，可能会出现混淆错误。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-559">In the case of an event declaration with a parameter list, the generated nested delegate class could have confusing errors when a type that appears to be used in an `In` position (i.e. a parameter type) is actually used in an `Out` position (i.e. the type of the event).</span></span>

<span data-ttu-id="ab3c0-560">使用 Out 修饰符声明的类型参数是*协变*的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-560">A type parameter that is declared with the Out modifier is *covariant*.</span></span> <span data-ttu-id="ab3c0-561">非正式类型参数只能在输出位置中使用--即从接口或委托类型返回的值，而不能在输入位置使用。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-561">Informally, a covariant type parameter can only be used in an output position -- i.e. a value that is being returned from the interface or delegate type -- and cannot be used in an input position.</span></span> <span data-ttu-id="ab3c0-562">如果是以下情况，则将类型 `T` 视为*有效的 covariantly* ：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-562">A type `T` is considered to be *valid covariantly* if:</span></span>

* <span data-ttu-id="ab3c0-563">`T` 为类、结构或枚举类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-563">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="ab3c0-564">`T` 为非泛型委托或接口类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-564">`T` is non-generic delegate or interface type.</span></span>

* <span data-ttu-id="ab3c0-565">`T` 是其元素类型为 covariantly 有效的数组类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-565">`T` is an array type whose element type is valid covariantly.</span></span>

* <span data-ttu-id="ab3c0-566">`T` 是未声明为 `Out` 类型参数的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-566">`T` is a type parameter which was not declared as an `Out` type parameter.</span></span>

* <span data-ttu-id="ab3c0-567">`T` 是使用类型参数 `X(Of P1,...,Pn)` 构造接口或委托类型，`A1,...,An` 这样：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-567">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="ab3c0-568">如果 `Pi` 声明为 Out 类型参数，则 `Ai` 是有效的 covariantly。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-568">If `Pi` was declared as an Out type parameter then `Ai` is valid covariantly.</span></span>

  * <span data-ttu-id="ab3c0-569">如果 `Pi` 声明为 In 类型参数，则 `Ai` 是有效的 contravariantly。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-569">If `Pi` was declared as an In type parameter then `Ai` is valid contravariantly.</span></span>

<span data-ttu-id="ab3c0-570">以下内容必须是接口或委托类型中的有效 covariantly：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-570">The following must be valid covariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="ab3c0-571">接口的基接口。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-571">The base interface of an interface.</span></span>

* <span data-ttu-id="ab3c0-572">函数或委托类型的返回类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-572">The return type of a function or the delegate type.</span></span>

* <span data-ttu-id="ab3c0-573">如果有 `Get` 访问器，则为属性的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-573">The type of a property if there is a `Get` accessor.</span></span>

* <span data-ttu-id="ab3c0-574">任何 `ByRef` 参数的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-574">The type of any `ByRef` parameter.</span></span>

<span data-ttu-id="ab3c0-575">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-575">For example:</span></span>

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

<span data-ttu-id="ab3c0-576">__纪录.__</span><span class="sxs-lookup"><span data-stu-id="ab3c0-576">__Note.__</span></span> <span data-ttu-id="ab3c0-577">`Out` 不是保留字。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-577">`Out` is not a reserved word.</span></span>

<span data-ttu-id="ab3c0-578">使用 In 修饰符声明的类型参数是*逆变*的。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-578">A type parameter that is declared with the In modifier is *contravariant*.</span></span> <span data-ttu-id="ab3c0-579">非正式类型参数（非正式类型参数）只能在输入位置使用--即传入接口或委托类型的值，不能用于输出位置。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-579">Informally, a contravariant type parameter can only be used in an input position -- i.e. a value that is being passed in to the interface or delegate type -- and cannot be used in an output position.</span></span> <span data-ttu-id="ab3c0-580">如果是以下情况，则将类型 `T` 视为*有效的 contravariantly* ：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-580">A type `T` is considered to be *valid contravariantly* if:</span></span>

* <span data-ttu-id="ab3c0-581">`T` 为类、结构或枚举类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-581">`T` is a class, structure, or enumerated type.</span></span>

* <span data-ttu-id="ab3c0-582">`T` 为非泛型委托或接口类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-582">`T` is a non-generic delegate or interface type.</span></span>

* <span data-ttu-id="ab3c0-583">`T` 是其元素类型为 contravariantly 有效的数组类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-583">`T` is an array type whose element type is valid contravariantly.</span></span>

* <span data-ttu-id="ab3c0-584">`T` 是未在类型参数中声明为的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-584">`T` is a type parameter which was not declared as an In type parameter.</span></span>

* <span data-ttu-id="ab3c0-585">`T` 是使用类型参数 `X(Of P1,...,Pn)` 构造接口或委托类型，`A1,...,An` 这样：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-585">`T` is a constructed interface or delegate type `X(Of P1,...,Pn)` with type arguments `A1,...,An` such that:</span></span>

  * <span data-ttu-id="ab3c0-586">如果 `Pi` 声明为 `Out` 类型参数，则 `Ai` 为有效的 contravariantly。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-586">If `Pi` was declared as an `Out` type parameter then `Ai` is valid contravariantly.</span></span>

  * <span data-ttu-id="ab3c0-587">如果 `Pi` 声明为 `In` 类型参数，则 `Ai` 为有效的 covariantly。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-587">If `Pi` was declared as an `In` type parameter then `Ai` is valid covariantly.</span></span>

<span data-ttu-id="ab3c0-588">以下内容必须是接口或委托类型中的有效 contravariantly：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-588">The following must be valid contravariantly in an interface or delegate type:</span></span>

* <span data-ttu-id="ab3c0-589">参数的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-589">The type of a parameter.</span></span>

* <span data-ttu-id="ab3c0-590">方法类型参数上的类型约束。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-590">A type constraint on a method type parameter.</span></span>

* <span data-ttu-id="ab3c0-591">如果属性具有 `Set` 访问器，则为属性的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-591">The type of a property if it has a `Set` accessor.</span></span>

* <span data-ttu-id="ab3c0-592">事件的类型。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-592">The type of an event.</span></span>

<span data-ttu-id="ab3c0-593">例如：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-593">For example:</span></span>

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

<span data-ttu-id="ab3c0-594">在类型必须有效的情况下为 contravariantly 和 covariantly （例如，具有 `Get` 和 `Set` 访问器或 `ByRef` 参数的属性）时，不能使用变体类型参数。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-594">In the case where a type must be valid be contravariantly and covariantly (such as a property with both a `Get` and `Set` accessor or a `ByRef` parameter), a variant type parameter cannot be used.</span></span>


<span data-ttu-id="ab3c0-595">协方差和方差会给出 "钻石多义性问题"。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-595">Co- and contra-variance give rise to a "diamond ambiguity problem".</span></span> <span data-ttu-id="ab3c0-596">考虑下列代码：</span><span class="sxs-lookup"><span data-stu-id="ab3c0-596">Consider the following code:</span></span>

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

<span data-ttu-id="ab3c0-597">可以通过两种方式将类 `C` 转换为 `IEnumerable(Of Object)`，这两种方法都可以通过从 `IEnumerable(Of String)` 的协变转换和从 `IEnumerable(Of Exception)`到协变转换。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-597">The class `C` can be converted to `IEnumerable(Of Object)` in two ways, both through covariant conversion from `IEnumerable(Of String)` and through covariant conversion from `IEnumerable(Of Exception)`.</span></span> <span data-ttu-id="ab3c0-598">CLR 未指定 `c.GetEnumerator()`将调用这两种方法中的哪一种。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-598">The CLR does not specify which of the two methods will be called by `c.GetEnumerator()`.</span></span> <span data-ttu-id="ab3c0-599">通常，每当将类声明为实现具有公共超类型的两个不同泛型参数的协变接口时（例如，在这种情况下 `String` 和 `Exception` 具有常见的父类型 `Object`），或者声明一个类来实现具有具有相同子类型的两个不同泛型参数的逆变接口时，可能会出现多义性。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-599">In general, whenever a class is declared to implement a covariant interface with two different generic arguments that have a common supertype (e.g. in this case `String` and `Exception` have the common supertype `Object`), or a class is declared to implement a contravariant interface with two different generic arguments that have a common subtype, then ambiguity is likely to arise.</span></span> <span data-ttu-id="ab3c0-600">编译器会对此类声明发出警告。</span><span class="sxs-lookup"><span data-stu-id="ab3c0-600">The compiler gives a warning on such declarations.</span></span>
