---
ms.openlocfilehash: 2e917b7aa27afd8a91e8c289e7454785d01bdda8
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426637"
---
# <a name="source-files-and-namespaces"></a><span data-ttu-id="5e4e9-101">源文件和命名空间</span><span class="sxs-lookup"><span data-stu-id="5e4e9-101">Source Files and Namespaces</span></span>

<span data-ttu-id="5e4e9-102">Visual Basic 程序由一个或多个源文件组成。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-102">A Visual Basic program consists of one or more source files.</span></span> <span data-ttu-id="5e4e9-103">编译计划后，所有源文件一起处理;因此，源文件可以互相依赖，可能是以循环方式，而无需前向声明。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-103">When a program is compiled, all of the source files are processed together; thus, source files can depend on each other, possibly in a circular fashion, without any forward-declaration requirement.</span></span> <span data-ttu-id="5e4e9-104">文本的程序文本中的声明顺序通常是没有意义。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-104">The textual order of declarations in the program text is generally of no significance.</span></span>

<span data-ttu-id="5e4e9-105">源代码文件由一组可选的选项语句、 导入语句、 和属性后, 跟的命名空间体。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-105">A source file consists of an optional set of option statements, import statements, and attributes, which are followed by a namespace body.</span></span> <span data-ttu-id="5e4e9-106">属性，必须每个都`Assembly`或`Module`修饰符，将应用到.NET 程序集或模块生成的编译。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-106">The attributes, which must each have either the `Assembly` or `Module` modifier, apply to the .NET assembly or module produced by the compilation.</span></span> <span data-ttu-id="5e4e9-107">作为全局命名空间，这意味着所有源代码文件的顶层声明都位于全局命名空间中的隐式命名空间声明源文件函数的主体。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-107">The body of the source file functions as an implicit namespace declaration for the global namespace, meaning that all declarations at the top level of a source file are placed in the global namespace.</span></span> <span data-ttu-id="5e4e9-108">例如：</span><span class="sxs-lookup"><span data-stu-id="5e4e9-108">For example:</span></span>

<span data-ttu-id="5e4e9-109">FileA.vb:</span><span class="sxs-lookup"><span data-stu-id="5e4e9-109">FileA.vb:</span></span>

```vb
Class A
End Class
```

<span data-ttu-id="5e4e9-110">FileB.vb:</span><span class="sxs-lookup"><span data-stu-id="5e4e9-110">FileB.vb:</span></span>

```vb
Class B
End Class
```

<span data-ttu-id="5e4e9-111">全局命名空间，在这种情况下声明两个类的完全限定名称的两个源文件参与`A`和`B`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-111">The two source files contribute to the global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="5e4e9-112">由于向同一声明空间分配的两个源文件，重要的是成员的一个错误如果每个包含具有相同名称的声明。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-112">Because the two source files contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

<span data-ttu-id="5e4e9-113">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="5e4e9-113">__Note.__</span></span> <span data-ttu-id="5e4e9-114">编译环境可能会覆盖隐式放入源代码文件的命名空间声明。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-114">The compilation environment may override the namespace declarations into which a source file is implicitly placed.</span></span>

<span data-ttu-id="5e4e9-115">除非另有说明，Visual Basic 程序中的语句可以行结束符或分号终止。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-115">Except where noted, statements within a Visual Basic program can be terminated either by a line terminator or by a colon.</span></span>

```antlr
Start
    : OptionStatement* ImportsStatement* AttributesStatement* NamespaceMemberDeclaration*
    ;

StatementTerminator
    : LineTerminator
    | ':'
    ;

AttributesStatement
    : Attributes StatementTerminator
    ;
```

## <a name="program-startup-and-termination"></a><span data-ttu-id="5e4e9-116">程序启动和终止</span><span class="sxs-lookup"><span data-stu-id="5e4e9-116">Program Startup and Termination</span></span>

<span data-ttu-id="5e4e9-117">程序启动执行环境执行指定的方法，该程序的方式被称为时会出现*入口点*。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-117">Program startup occurs when the execution environment executes a designated method, which is referred to as the program's *entry point*.</span></span> <span data-ttu-id="5e4e9-118">此入口点方法，必须始终命名为`Main`、 必须共享、 不能包含在泛型类型，不能有 async 修饰符和必须具有以下签名之一：</span><span class="sxs-lookup"><span data-stu-id="5e4e9-118">This entry point method, which must always be named `Main`, must be shared, cannot be contained in a generic type, cannot have the async modifier, and must have one of the following signatures:</span></span>

```vb
Sub Main()
Sub Main(args() As String)
Function Main() As Integer
Function Main(args() As String) As Integer
```

<span data-ttu-id="5e4e9-119">入口点方法的可访问性无关。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-119">The accessibility of the entry point method is irrelevant.</span></span> <span data-ttu-id="5e4e9-120">如果程序包含多个合适的入口点，编译环境必须指定一个作为入口点。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-120">If a program contains more than one suitable entry point, the compilation environment must designate one as the entry point.</span></span> <span data-ttu-id="5e4e9-121">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-121">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="5e4e9-122">如果不存在，编译环境还可以创建的入口点方法。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-122">The compilation environment may also create an entry point method if one does not exist.</span></span>

<span data-ttu-id="5e4e9-123">当程序开始，如果入口点有一个参数时，执行环境所提供的自变量将包含表示为字符串程序的命令行参数。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-123">When a program begins, if the entry point has a parameter, the argument supplied by the execution environment contains the command-line arguments to the program represented as strings.</span></span> <span data-ttu-id="5e4e9-124">如果入口点返回类型为`Integer`，则从函数返回的值作为该程序的结果返回到执行环境。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-124">If the entry point has a return type of `Integer`, then the value returned from the function is returned to the execution environment as the result of the program.</span></span>

<span data-ttu-id="5e4e9-125">在所有其他方面，入口点方法的行为与其他方法相同的方式。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-125">In all other respects, entry point methods behave in the same manner as other methods.</span></span> <span data-ttu-id="5e4e9-126">当执行离开所做的执行环境的入口点方法的调用时，该程序将终止。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-126">When execution leaves the invocation of the entry point method made by the execution environment, the program terminates.</span></span>

## <a name="compilation-options"></a><span data-ttu-id="5e4e9-127">编译选项</span><span class="sxs-lookup"><span data-stu-id="5e4e9-127">Compilation Options</span></span>

<span data-ttu-id="5e4e9-128">源文件可以在源中的代码使用指定的编译选项*选项语句*。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-128">A source file can specify compilation options in the source code using *option statements*.</span></span>

```antlr
OptionStatement
    : OptionExplicitStatement
    | OptionStrictStatement
    | OptionCompareStatement
    | OptionInferStatement
    ;
```

<span data-ttu-id="5e4e9-129">`Option`语句只应用于源文件中的出现，并且只有一个每种类型的`Option`语句可能会出现在源文件中。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-129">An `Option` statement applies only to the source file in which it appears, and only one of each type of `Option` statement may appear in a source file.</span></span> <span data-ttu-id="5e4e9-130">例如：</span><span class="sxs-lookup"><span data-stu-id="5e4e9-130">For example:</span></span>

```vb
Option Strict On
Option Compare Text
Option Strict Off    ' Not allowed, Option Strict is already specified.
Option Compare Text  ' Not allowed, Option Compare is already specified.
```

<span data-ttu-id="5e4e9-131">有四个编译选项： 严格类型语义、 显式声明的语义、 比较语义和本地变量的类型推理语义。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-131">There are four compilation options: strict type semantics, explicit declaration semantics, comparison semantics, and local variable type inference semantics.</span></span> <span data-ttu-id="5e4e9-132">如果源文件不包括特定`Option`语句，然后编译环境确定了将使用哪些组特定的语义。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-132">If a source file does not include a particular `Option` statement, then the compilation environment determines which particular set of semantics will be used.</span></span> <span data-ttu-id="5e4e9-133">此外，还有第五个编译选项，仅可以通过编译环境指定的整数溢出检查。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-133">There is also a fifth compilation option, integer overflow checks, which can only be specified through the compilation environment.</span></span>


### <a name="option-explicit-statement"></a><span data-ttu-id="5e4e9-134">Option Explicit 语句</span><span class="sxs-lookup"><span data-stu-id="5e4e9-134">Option Explicit Statement</span></span>

<span data-ttu-id="5e4e9-135">`Option Explicit`语句用于确定是否可能会隐式声明的局部变量。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-135">The `Option Explicit` statement determines whether local variables may be implicitly declared.</span></span> <span data-ttu-id="5e4e9-136">关键字`On`或`Off`可以跟在该语句; 如果未指定，默认值是`On`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-136">The keywords `On` or `Off` may follow the statement; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="5e4e9-137">如果在文件中未不指定任何语句，编译环境确定其将被使用。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-137">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionExplicitStatement
    : 'Option' 'Explicit' OnOff? StatementTerminator
    ;

OnOff
    : 'On' | 'Off'
    ;
```


<span data-ttu-id="5e4e9-138">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="5e4e9-138">__Note.__</span></span> <span data-ttu-id="5e4e9-139">`Explicit` 和`Off`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-139">`Explicit` and `Off` are not reserved words.</span></span>

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 5 ' Valid because Option Explicit is off.
    End Sub
End Module
```

<span data-ttu-id="5e4e9-140">在此示例中，本地变量`x`隐式声明通过将分配给它。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-140">In this example, the local variable `x` is implicitly declared by assigning to it.</span></span> <span data-ttu-id="5e4e9-141">类型 `x` 不是 `Object`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-141">The type of `x` is `Object`.</span></span>


### <a name="option-strict-statement"></a><span data-ttu-id="5e4e9-142">Option Strict Statement</span><span class="sxs-lookup"><span data-stu-id="5e4e9-142">Option Strict Statement</span></span>

<span data-ttu-id="5e4e9-143">`Option Strict`语句用于确定是否转换和操作上的`Object`受严格或许可类型语义和是否类型隐式类型化为`Object`如果没有`As`指定子句。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-143">The `Option Strict` statement determines whether conversions and operations on `Object` are governed by strict or permissive type semantics and whether types are implicitly typed as `Object` if no `As` clause is specified.</span></span> <span data-ttu-id="5e4e9-144">该语句可能跟关键字`On`或`Off`; 如果未指定，默认值是`On`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-144">The statement may be followed by the keywords `On` or `Off`; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="5e4e9-145">如果在文件中未不指定任何语句，编译环境确定其将被使用。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-145">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionStrictStatement
    : 'Option' 'Strict' OnOff? StatementTerminator
    ;
```

<span data-ttu-id="5e4e9-146">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="5e4e9-146">__Note.__</span></span> <span data-ttu-id="5e4e9-147">`Strict` 和`Off`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-147">`Strict` and `Off` are not reserved words.</span></span>

```vb
Option Strict On

Module Test
    Sub Main()
        Dim x ' Error, no type specified.
        Dim o As Object
        Dim b As Byte = o ' Error, narrowing conversion.

        o.F() ' Error, late binding disallowed.
        o = o + 1 ' Error, addition is not defined on Object.
    End Sub
End Module
```

<span data-ttu-id="5e4e9-148">在严格的语义，以下是不允许：</span><span class="sxs-lookup"><span data-stu-id="5e4e9-148">Under strict semantics, the following are disallowed:</span></span>

* <span data-ttu-id="5e4e9-149">而无需显式强制转换运算符的收缩转换。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-149">Narrowing conversions without an explicit cast operator.</span></span>

* <span data-ttu-id="5e4e9-150">后期绑定。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-150">Late binding.</span></span>

* <span data-ttu-id="5e4e9-151">对类型执行操作`Object`而不`TypeOf`...`Is`， `Is`，和`IsNot`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-151">Operations on type `Object` other than `TypeOf`...`Is`, `Is`, and `IsNot`.</span></span>

* <span data-ttu-id="5e4e9-152">省略`As`子句中不具有推断的类型的声明。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-152">Omitting the `As` clause in a declaration that does not have an inferred type.</span></span>


### <a name="option-compare-statement"></a><span data-ttu-id="5e4e9-153">Option Compare 语句</span><span class="sxs-lookup"><span data-stu-id="5e4e9-153">Option Compare Statement</span></span>

<span data-ttu-id="5e4e9-154">`Option Compare`语句用于确定字符串比较的语义。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-154">The `Option Compare` statement determines the semantics of string comparisons.</span></span> <span data-ttu-id="5e4e9-155">任何一个执行字符串比较时使用二进制比较 （在其中进行比较的每个字符的二进制 Unicode 值） 或 （在其中每个字符的词汇意义进行比较使用当前区域性） 的文本比较。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-155">String comparisons are carried out either using binary comparisons (in which the binary Unicode value of each character is compared) or text comparisons (in which the lexical meaning of each character is compared using the current culture).</span></span> <span data-ttu-id="5e4e9-156">如果在文件中未不指定任何语句，控制编译环境，将使用哪种类型的比较。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-156">If no statement is specified in a file, the compilation environment controls which type of comparison will be used.</span></span>

```antlr
OptionCompareStatement
    : 'Option' 'Compare' CompareOption StatementTerminator
    ;

CompareOption
    : 'Binary' | 'Text'
    ;
```

<span data-ttu-id="5e4e9-157">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="5e4e9-157">__Note.__</span></span> <span data-ttu-id="5e4e9-158">`Compare``Binary`，和`Text`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-158">`Compare`, `Binary`, and `Text` are not reserved words.</span></span>

```vb
Option Compare Text

Module Test
    Sub Main()
        Console.WriteLine("a" = "A")    ' Prints True.
    End Sub
End Module
```

<span data-ttu-id="5e4e9-159">在这种情况下，进行字符串比较使用文本比较忽略大小写差异。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-159">In this case, the string comparison is done using a text comparison that ignores case differences.</span></span> <span data-ttu-id="5e4e9-160">如果`Option Compare Binary`具有已指定，则这将打印`False`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-160">If `Option Compare Binary` had been specified, then this would have printed `False`.</span></span>


### <a name="integer-overflow-checks"></a><span data-ttu-id="5e4e9-161">整数溢出检查</span><span class="sxs-lookup"><span data-stu-id="5e4e9-161">Integer Overflow Checks</span></span>

<span data-ttu-id="5e4e9-162">整数运算可以检查或在运行时未选中溢出条件。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-162">Integer operations can either be checked or not checked for overflow conditions at run time.</span></span> <span data-ttu-id="5e4e9-163">如果溢出条件已选中并且整数操作溢出，`System.OverflowException`引发异常。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-163">If overflow conditions are checked and an integer operation overflows, a `System.OverflowException` exception is thrown.</span></span> <span data-ttu-id="5e4e9-164">如果未选中溢出条件，整数操作溢出不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-164">If overflow conditions are not checked, integer operation overflows do not throw an exception.</span></span> <span data-ttu-id="5e4e9-165">编译环境确定此选项为打开或关闭。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-165">The compilation environment determines whether this option is on or off.</span></span>

### <a name="option-infer-statement"></a><span data-ttu-id="5e4e9-166">Option Infer 语句</span><span class="sxs-lookup"><span data-stu-id="5e4e9-166">Option Infer Statement</span></span>

<span data-ttu-id="5e4e9-167">`Option Infer`语句用于确定是否本地变量声明，其中不含`As`子句具有推断出的类型或使用`Object`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-167">The `Option Infer` statement determines whether local variable declarations that have no `As` clause have an inferred type or use `Object`.</span></span> <span data-ttu-id="5e4e9-168">该语句可能跟关键字`On`或`Off`; 如果未指定，默认值是`On`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-168">The statement may be followed by the keywords `On` or `Off`; if neither is specified, the default is `On`.</span></span> <span data-ttu-id="5e4e9-169">如果在文件中未不指定任何语句，编译环境确定其将被使用。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-169">If no statement is specified in a file, the compilation environment determines which will be used.</span></span>

```antlr
OptionInferStatement
    : 'Option' 'Infer' OnOff? StatementTerminator
    ;
```

<span data-ttu-id="5e4e9-170">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="5e4e9-170">__Note.__</span></span> <span data-ttu-id="5e4e9-171">`Infer` 和`Off`不是保留的字。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-171">`Infer` and `Off` are not reserved words.</span></span>

```vb
Option Infer On

Module Test
    Sub Main()
        ' The type of x is Integer
        Dim x = 10

        ' The type of y is String
        Dim y = "abc"
    End Sub
End Module
```


## <a name="imports-statement"></a><span data-ttu-id="5e4e9-172">Imports 语句</span><span class="sxs-lookup"><span data-stu-id="5e4e9-172">Imports Statement</span></span>

<span data-ttu-id="5e4e9-173">`Imports` 语句导入源文件，允许的名称，而无需限定，引用的实体的名称，或导入 XML 表达式中使用的命名空间。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-173">`Imports` statements import the names of entities into a source file, allowing the names to be referenced without qualification, or import a namespace for use in XML expressions.</span></span>

```antlr
ImportsStatement
    : 'Imports' ImportsClauses StatementTerminator
    ;

ImportsClauses
    : ImportsClause ( Comma ImportsClause )*
    ;

ImportsClause
    : AliasImportsClause
    | MembersImportsClause
    | XMLNamespaceImportsClause
    ;
```

<span data-ttu-id="5e4e9-174">在成员声明中包含的源文件内`Imports`语句，给定的命名空间中包含的类型可以直接，引用，如下面的示例中所示：</span><span class="sxs-lookup"><span data-stu-id="5e4e9-174">Within member declarations in a source file that contains an `Imports` statement, the types contained in the given namespace can be referenced directly, as seen in the following example:</span></span>

```vb
Imports N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace 

Namespace N3
    Class B
        Inherits A
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-175">此处，在源文件中的，命名空间的类型成员`N1.N2`可直接使用，并因此类`N3.B`派生自类`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-175">Here, within the source file, the type members of namespace `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="5e4e9-176">`Imports` 语句必须出现在任何之后`Option`语句但任何之前的类型声明。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-176">`Imports` statements must appear after any `Option` statements but before any type declarations.</span></span> <span data-ttu-id="5e4e9-177">编译环境还定义隐式`Imports`语句。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-177">The compilation environment may also define implicit `Imports` statements.</span></span>

<span data-ttu-id="5e4e9-178">`Imports` 语句在源文件中，使名称可用，但未声明全局命名空间声明空间中的任何内容。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-178">`Imports` statements make names available in a source file, but do not declare anything in the global namespace's declaration space.</span></span> <span data-ttu-id="5e4e9-179">导入的名称的作用域`Imports`语句通过源代码文件中包含的命名空间成员声明进行了扩展。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-179">The scope of the names imported by an `Imports` statement extends over the namespace member declarations contained in the source file.</span></span> <span data-ttu-id="5e4e9-180">作用域`Imports`语句具体说来，不包含其他`Imports`语句，也不包括其他源文件。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-180">The scope of an `Imports` statement specifically does not include other `Imports` statements, nor does it include other source files.</span></span> <span data-ttu-id="5e4e9-181">`Imports` 另一个可能不引用语句。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-181">`Imports` statements may not refer to one another.</span></span>

<span data-ttu-id="5e4e9-182">在此示例中，最后一个`Imports`语句是错误，因为它不受第一个导入别名。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-182">In this example, the last `Imports` statement is in error because it is not affected by the first import alias.</span></span>

```vb
Imports R1 = N1 ' OK.
Imports R2 = N1.N2 ' OK.
Imports R3 = R1.N2 ' Error: Can't refer to R1.

Namespace N1.N2
End Namespace
```

<span data-ttu-id="5e4e9-183">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="5e4e9-183">__Note.__</span></span> <span data-ttu-id="5e4e9-184">在出现的命名空间或类型名称`Imports`语句始终处理，就好像它们是完全限定。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-184">The namespace or type names that appear in `Imports` statements are always treated as if they are fully qualified.</span></span> <span data-ttu-id="5e4e9-185">也就是说，全局命名空间中始终会解析命名空间或类型名称中最左侧的标识符和的解决方法的其余部分将按照正常名称解析规则进行。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-185">That is, the leftmost identifier in a namespace or type name always resolves in the global namespace and the rest of the resolution proceeds according to normal name resolution rules.</span></span> <span data-ttu-id="5e4e9-186">这是在应用此类规则; 的语言中的唯一位置规则可确保名称不能完全隐藏从限定。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-186">This is the only place in the language that applies such a rule; the rule ensures that a name cannot be completely hidden from qualification.</span></span> <span data-ttu-id="5e4e9-187">如果没有规则，如果全局命名空间中的名称被隐藏在特定源文件中，它无法限定方式指定该命名空间中的任何名称。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-187">Without the rule, if a name in the global namespace were hidden in a particular source file, it would be impossible to specify any names from that namespace in a qualified way.</span></span>

<span data-ttu-id="5e4e9-188">在此示例中，`Imports`语句总是引用全局`System`命名空间，而不是源文件中的类。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-188">In this example, the `Imports` statement always refers to the global `System` namespace, and not the class in the source file.</span></span>

```vb
Imports System   ' Imports the namespace, not the class.

Class System
End Class
```


### <a name="import-aliases"></a><span data-ttu-id="5e4e9-189">导入别名</span><span class="sxs-lookup"><span data-stu-id="5e4e9-189">Import Aliases</span></span>

<span data-ttu-id="5e4e9-190">*导入别名*定义命名空间或类型的别名。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-190">An *import alias* defines an alias for a namespace or type.</span></span>

```antlr
AliasImportsClause
    : Identifier Equals TypeName
    ;
```

```vb
Imports A = N1.N2.A

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class B
        Inherits A
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-191">此处，在源代码文件中，`A`是其别名`N1.N2.A`，因此类`N3.B`派生自类`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-191">Here, within the source file, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="5e4e9-192">可以通过创建别名来获取相同的效果`R`有关`N1.N2`然后引用`R.A`:</span><span class="sxs-lookup"><span data-stu-id="5e4e9-192">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>

```vb
Imports R = N1.N2

Namespace N3
    Class B
        Inherits R.A
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-193">导入别名的标识符中必须是唯一的声明空间的全局命名空间 （而不仅仅是在其中定义导入别名的源文件中的全局命名空间声明），即使它未声明全局命名空间中的名称声明空间。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-193">The identifier of an import alias must be unique within the declaration space of the global namespace (not just the global namespace declaration in the source file in which the import alias is defined), even though it does not declare a name in the global namespace's declaration space.</span></span>

<span data-ttu-id="5e4e9-194">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="5e4e9-194">__Note.__</span></span> <span data-ttu-id="5e4e9-195">在模块中的声明不引入包含的声明空间的名称。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-195">Declarations in a module do not introduce names into the containing declaration space.</span></span> <span data-ttu-id="5e4e9-196">因此，它是一个模块，将具有相同的名称为导入别名中的声明有效即使在包含声明空间的声明名称将无法访问。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-196">Thus, it is valid for a declaration in a module to have the same name as an import alias, even though the declaration's name will be accessible in the containing declaration space.</span></span>

```vb
' Error: Alias A conflicts with typename A
Imports A = N3.A

Class A
End Class

Namespace N3
    Class A
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-197">在这里，全局命名空间已有一个成员`A`，因此它是错误的导入别名，以使用该标识符。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-197">Here, the global namespace already contains a member `A`, so it is an error for an import alias to use that identifier.</span></span> <span data-ttu-id="5e4e9-198">它同样是错误的两个或多个要通过相同的名称来声明别名的同一源文件中的导入别名。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-198">It is likewise an error for two or more import aliases in the same source file to declare aliases by the same name.</span></span>

<span data-ttu-id="5e4e9-199">导入别名可以创建任何命名空间或类型的别名。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-199">An import alias can create an alias for any namespace or type.</span></span> <span data-ttu-id="5e4e9-200">通过别名访问命名空间或类型会产生与通过其声明的名称访问该命名空间或类型相同的结果。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-200">Accessing a namespace or type through an alias yields exactly the same result as accessing the namespace or type through its declared name.</span></span>

```vb
Imports R1 = N1
Imports R2 = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class B
        Private a As N1.N2.A
        Private b As R1.N2.A
        Private c As R2.A
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-201">此处，名称`N1.N2.A`， `R1.N2.A`，并`R2.A`等效项，以及所有引用的类的完全限定的名称是`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-201">Here, the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent, and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="5e4e9-202">导入指定命名空间或向其创建别名的类型的确切的名称。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-202">The import specifies the exact name of the namespace or type to which it is creating an alias.</span></span> <span data-ttu-id="5e4e9-203">这必须是该命名空间或类型的确切的完全限定的名称： 不使用限定的名称解析 （该实例允许通过派生类的基类的成员访问） 的一般规则。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-203">This must be the exact fully qualified name of that namespace or type: it does not use the normal rules for qualified name resolution (which for instance allow access to the members of a base class through a derived class).</span></span>

<span data-ttu-id="5e4e9-204">如果导入别名指向的类型或命名空间不能解析的这些规则，然后导入语句将被忽略 （和编译器则报告警告）。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-204">If an import alias points to a type or namespace which cannot be resolved by these rules, then the import statement is ignored (and the compiler gives a warning).</span></span>

<span data-ttu-id="5e4e9-205">此外，该引用不能为开放式泛型类型--所有泛型类型必须具有有效的类型参数提供，和所有类型实参必须都是可由上述规则解析。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-205">Also, the reference cannot be to an open generic type -- all generic types must have valid type arguments supplied, and all type arguments must be resolvable by the rules above.</span></span> <span data-ttu-id="5e4e9-206">泛型类型的任何不正确的绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-206">Any incorrect binding of a generic type is an error.</span></span>

```vb
Imports A = G              ' error: since G is an open generic type
Imports B = G(Of Integer)  ' okay
Imports C = Derived.Nested ' warning: Derived.Nested isn't itself a type
Imports D = G(Of Derived.Nested) ' error: Derived.Nested isn't found

Class G(Of T) : End Class

Class Base
    Class Nested : End Class
End Class

Class Derived : Inherits Base
End Class

Module Module1
    Sub Main()
        Dim x As C               ' error: "C" wasn't succesfully defined
        Dim y As Derived.Nested  ' okay
    End Sub
End Module
```

<span data-ttu-id="5e4e9-207">源文件中的声明可能会隐藏导入别名名称。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-207">Declarations in the source file may shadow the import alias name.</span></span>

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class R
    End Class

    Class B
        Inherits R.A  ' Error, R has no member A
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-208">在前面的示例引用到`R.A`中的声明`B`导致错误，因为`R`指`N3.R`，而不`N1.N2`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-208">In the preceding example the reference to `R.A` in the declaration of `B` causes an error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="5e4e9-209">导入别名使别名可在特定的源代码文件中，但它不会影响到基础的声明空间的任何新成员。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-209">An import alias makes an alias available within a particular source file, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="5e4e9-210">换而言之，导入别名不是可传递的但而是会影响仅项所在的源文件。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-210">In other words, an import alias is not transitive, but rather affects only the source file in which it occurs.</span></span>

<span data-ttu-id="5e4e9-211">File1.vb:</span><span class="sxs-lookup"><span data-stu-id="5e4e9-211">File1.vb:</span></span>

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-212">File2.vb:</span><span class="sxs-lookup"><span data-stu-id="5e4e9-212">File2.vb:</span></span>

```vb
Class B
    Inherits R.A ' Error, R unknown.
End Class
```

<span data-ttu-id="5e4e9-213">在上述示例中，因为导入别名的作用域引入`R`仅扩展到在其中包含它，源文件中声明`R`第二个源文件中为未知。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-213">In the above example, because the scope of the import alias that introduces `R` only extends to declarations in the source file in which it is contained, `R` is unknown in the second source file.</span></span>


### <a name="namespace-imports"></a><span data-ttu-id="5e4e9-214">命名空间导入</span><span class="sxs-lookup"><span data-stu-id="5e4e9-214">Namespace Imports</span></span>

<span data-ttu-id="5e4e9-215">一个*命名空间导入*导入的所有成员的命名空间或类型，允许的命名空间或类型，而无需限定使用的每个成员的标识符。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-215">A *namespace import* imports all of the members of a namespace or type, allowing the identifier of each member of the namespace or type to be used without qualification.</span></span> <span data-ttu-id="5e4e9-216">对于类型，命名空间导入只允许对类型的共享成员的访问而无需限定类名。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-216">In the case of types, a namespace import only allows access to the shared members of the type without requiring qualification of the class name.</span></span> <span data-ttu-id="5e4e9-217">具体而言，它允许枚举的类型，而无需限定使用的成员。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-217">In particular, it allows the members of enumerated types to be used without qualification.</span></span>

```antlr
MembersImportsClause
    : TypeName
    ;
```

<span data-ttu-id="5e4e9-218">例如：</span><span class="sxs-lookup"><span data-stu-id="5e4e9-218">For example:</span></span>

```vb
Imports Colors

Enum Colors
    Red
    Green
    Blue
End Enum

Module M1
    Sub Main()
        Dim c As Colors = Red
    End Sub
End Module
```

<span data-ttu-id="5e4e9-219">与导入别名，不同的命名空间导入它导入并可以导入命名空间和类型标识符已声明全局命名空间中的名称没有任何限制。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-219">Unlike an import alias, a namespace import has no restrictions on the names it imports and may import namespaces and types whose identifiers are already declared within the global namespace.</span></span> <span data-ttu-id="5e4e9-220">通过导入别名和源代码文件中的声明隐藏导入的正则导入的名称。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-220">The names imported by a regular import are shadowed by import aliases and declarations in the source file.</span></span>

<span data-ttu-id="5e4e9-221">在以下示例中，`A`是指`N3.A`而非`N1.N2.A`中的成员声明中`N3`命名空间。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-221">In the following example, `A` refers to `N3.A` rather than `N1.N2.A` within member declarations in the `N3` namespace.</span></span>

```vb
Imports N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N3
    Class A
    End Class

    Class B
        Inherits A
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-222">当多个导入的命名空间包含同名的成员 （和该名称不是隐藏由导入别名或声明） 时，对该名称的引用不明确，会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-222">When more than one imported namespace contains members by the same name (and that name is not otherwise shadowed by an import alias or declaration), a reference to that name is ambiguous and causes a compile-time error.</span></span>

```vb
Imports N1
Imports N2

Namespace N1
    Class A
    End Class
End Namespace 

Namespace N2
    Class A
    End Class
End Namespace 

Namespace N3
    Class B
        Inherits A ' Error, A is ambiguous.
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-223">在上述示例中，同时`N1`并`N2`都包含一个成员`A`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-223">In the above example, both `N1` and `N2` contain a member `A`.</span></span> <span data-ttu-id="5e4e9-224">因为`N3`导入两者，引用`A`中`N3`会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-224">Because `N3` imports both, referencing `A` in `N3` causes a compile-time error.</span></span> <span data-ttu-id="5e4e9-225">在此情况下，冲突可以解决通过对引用的限定`A`，或通过引入选取特定的导入别名`A`，如下面的示例：</span><span class="sxs-lookup"><span data-stu-id="5e4e9-225">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing an import alias that picks a particular `A`, as in the following example:</span></span>

```vb
Imports N1
Imports N2
Imports A = N1.A

Namespace N3
    Class B
        Inherits A ' A means N1.A.
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-226">唯一命名空间，类、 结构、 枚举类型，并可能会导入标准模块。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-226">Only namespaces, classes, structures, enumerated types, and standard modules may be imported.</span></span>


### <a name="xml-namespace-imports"></a><span data-ttu-id="5e4e9-227">XML Namespace 导入</span><span class="sxs-lookup"><span data-stu-id="5e4e9-227">XML Namespace Imports</span></span>

<span data-ttu-id="5e4e9-228">*XML 命名空间导入*定义命名空间或编译单元中包含的非限定 XML 表达式的默认命名空间。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-228">An *XML namespace import* defines a namespace or the default namespace for unqualified XML expressions contained within the compilation unit.</span></span>

```antlr
XMLNamespaceImportsClause
    : '<' XMLNamespaceAttributeName XMLWhitespace? Equals XMLWhitespace?
      XMLNamespaceValue '>'
    ;

XMLNamespaceValue
    : DoubleQuoteCharacter XMLAttributeDoubleQuoteValueCharacter* DoubleQuoteCharacter
    | SingleQuoteCharacter XMLAttributeSingleQuoteValueCharacter* SingleQuoteCharacter
    ;
```

<span data-ttu-id="5e4e9-229">例如：</span><span class="sxs-lookup"><span data-stu-id="5e4e9-229">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database">

Module Test
    Sub Main()
        ' db namespace is "http://example.org/database"
        Dim x = <db:customer><db:Name>Bob</></>

        Console.WriteLine(x.<db:Name>)
    End Sub
End Module
```

<span data-ttu-id="5e4e9-230">XML 命名空间，包括默认命名空间，仅对于一组特定的导入定义一次。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-230">An XML namespace, including the default namespace, can only be defined once for a particular set of imports.</span></span> <span data-ttu-id="5e4e9-231">例如：</span><span class="sxs-lookup"><span data-stu-id="5e4e9-231">For example:</span></span>

```vb
Imports <xmlns:db="http://example.org/database-one">
' Error: namespace db is already defined
Imports <xmlns:db="http://example.org/database-two">
```


## <a name="namespaces"></a><span data-ttu-id="5e4e9-232">命名空间</span><span class="sxs-lookup"><span data-stu-id="5e4e9-232">Namespaces</span></span>

<span data-ttu-id="5e4e9-233">Visual Basic 程序的组织结构使用的命名空间。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-233">Visual Basic programs are organized using namespaces.</span></span> <span data-ttu-id="5e4e9-234">命名空间都在内部组织程序，以及组织程序元素公开给其他程序的方式。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-234">Namespaces both internally organize a program as well as organize the way program elements are exposed to other programs.</span></span>

<span data-ttu-id="5e4e9-235">与其他实体不同命名空间是无限的并可能会声明为多个时间在同一程序中和跨许多程序，每个声明共同的同一命名空间的成员。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-235">Unlike other entities, namespaces are open-ended, and may be declared multiple times within the same program and across many programs, with each declaration contributing members to the same namespace.</span></span> <span data-ttu-id="5e4e9-236">在以下示例中，两个命名空间声明参与到同一声明空间，声明两个类的完全限定名称`N1.N2.A`和`N1.N2.B`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-236">In the following example, the two namespace declarations contribute to the same declaration space, declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span>

```vb
Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N1.N2
    Class B
    End Class
End Namespace
```

<span data-ttu-id="5e4e9-237">由于向同一声明空间分配的两个声明，则会出错，如果每个包含一个具有相同名称的成员的声明。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-237">Because the two declarations contribute to the same declaration space, it would be an error if each contained a declaration of a member with the same name.</span></span>

<span data-ttu-id="5e4e9-238">没有具有任何名称和其嵌套的命名空间和类型始终可以访问而无需限定的全局命名空间。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-238">There is a global namespace that has no name and whose nested namespaces and types can always be accessed without qualification.</span></span> <span data-ttu-id="5e4e9-239">声明全局命名空间中的命名空间成员的作用域是整个程序文本。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-239">The scope of a namespace member declared in the global namespace is the entire program text.</span></span> <span data-ttu-id="5e4e9-240">否则，类型或命名空间的作用域声明其完全限定名的命名空间中`N`是其对应的命名空间的完全限定的名称以开始，每个命名空间的程序文本`N`或为`N`本身。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-240">Otherwise, the scope of a type or namespace declared in a namespace whose fully qualified name is `N` is the program text of each namespace whose corresponding namespace's fully qualified name begins with `N` or is `N` itself.</span></span> <span data-ttu-id="5e4e9-241">（请注意，编译器可以选择将声明放在特定的命名空间，默认情况下。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-241">(Note that a compiler can choose to put declarations in a particular namespace by default.</span></span> <span data-ttu-id="5e4e9-242">这不会更改这一事实，还有一个全局的未命名命名空间。）</span><span class="sxs-lookup"><span data-stu-id="5e4e9-242">This does not alter the fact that there is still a global, unnamed namespace.)</span></span>

<span data-ttu-id="5e4e9-243">在此示例中，该类`B`可以请参阅类`A`因为`B`的命名空间`N1.N2.N3`从概念上讲嵌套在命名空间内`N1.N2`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-243">In this example, the class `B` can see the class `A` because `B`'s namespace `N1.N2.N3` is conceptually nested within the namespace `N1.N2`.</span></span>

```vb
Namespace N1.N2
    Class A
    End Class
End Namespace

Namespace N1.N2.N3
    Class B
        Inherits A
    End Class
End Namespace
```

### <a name="namespace-declarations"></a><span data-ttu-id="5e4e9-244">命名空间声明</span><span class="sxs-lookup"><span data-stu-id="5e4e9-244">Namespace Declarations</span></span>

<span data-ttu-id="5e4e9-245">有三种形式的命名空间声明。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-245">There are three forms of namespace declaration.</span></span>

```antlr
NamespaceDeclaration
    : 'Namespace' NamespaceName StatementTerminator
      NamespaceMemberDeclaration*
      'End' 'Namespace' StatementTerminator
    ;

NamespaceName
    : RelativeNamespaceName
    | 'Global'
    | 'Global' '.' RelativeNamespaceName
    ;

RelativeNamespaceName
    : Identifier ( Period IdentifierOrKeyword )*
    ;
```

<span data-ttu-id="5e4e9-246">第一个窗体将开始使用关键字`Namespace`跟相对的命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-246">The first form starts with the keyword `Namespace` followed by a relative namespace name.</span></span> <span data-ttu-id="5e4e9-247">如果相对的命名空间名称限定，命名空间声明被视为如同它从词法上嵌套在限定的名称中的每个名称对应的命名空间声明。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-247">If the relative namespace name is qualified, the namespace declaration is treated as if it is lexically nested within namespace declarations corresponding to each name in the qualified name.</span></span> <span data-ttu-id="5e4e9-248">例如，以下两个命名空间在语义上等效:</span><span class="sxs-lookup"><span data-stu-id="5e4e9-248">For example, the following two namespaces are semantically equivalent:</span></span>

```vb
Namespace N1.N2
    Class A
    End Class

    Class B
    End Class
End Namespace 

Namespace N1
    Namespace N2
        Class A
        End Class

        Class B
        End Class
    End Namespace
End Namespace
```

<span data-ttu-id="5e4e9-249">第二个窗体将开始通过关键字`Namespace Global`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-249">The second form starts with the keywords `Namespace Global`.</span></span> <span data-ttu-id="5e4e9-250">它被视为其所有成员声明了词法上都放置在全局命名空间-而不考虑编译环境提供任何默认值。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-250">It is treated as if all its member declarations were lexically placed in the global unnamed namespace -- regardless of any defaults provided by the compilation environment.</span></span> <span data-ttu-id="5e4e9-251">这种形式的命名空间声明不能从词法上任何其他命名空间声明内嵌套。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-251">This form of namespace declaration may not be lexically nested within any other namespace declaration.</span></span>

<span data-ttu-id="5e4e9-252">第三个窗体将开始通过关键字`Namespace Global`限定标识符后跟`N`。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-252">The third form starts with the keywords `Namespace Global` followed by a qualified identifier `N`.</span></span> <span data-ttu-id="5e4e9-253">它被视为如同它是第一种形式的命名空间声明"`Namespace N`"，从词法上被放在全局命名空间-而不考虑编译环境提供任何默认值。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-253">It is treated as if it were a namespace declaration of the first form "`Namespace N`" that was lexically placed in the global unnamed namespace -- regardless of any defaults provided by the compilation environment.</span></span> <span data-ttu-id="5e4e9-254">这种形式的命名空间声明不能从词法上任何其他命名空间声明内嵌套。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-254">This form of namespace declaration may not be lexically nested within any other namespace declaration.</span></span>

```vb
Namespace Global       ' Puts N1.A in the global namespace
    Namespace N1
        Class A
        End Class
    End Namespace
End Namespace

Namespace Global.N1    ' Equivalent to the above
    Class A
    End Class
End Namespace

Namespace N1           ' May or may not be equivalent to the above,
    Class A            ' depending on defaults provided by the
    End Class          ' compilation environment
End Namespace
```

<span data-ttu-id="5e4e9-255">在处理时命名空间的成员，特定成员的声明位置并不重要。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-255">When dealing with the members of a namespace, it is not important where a particular member is declared.</span></span> <span data-ttu-id="5e4e9-256">如果两个程序在同一个命名空间中定义具有相同名称的实体，尝试解析命名空间中的名称会导致多义性错误。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-256">If two programs define an entity with the same name in the same namespace, attempting to resolve the name in the namespace causes an ambiguity error.</span></span>

<span data-ttu-id="5e4e9-257">命名空间是根据定义`Public`，因此，命名空间声明不能包含任何访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-257">Namespaces are by definition `Public`, so a namespace declaration cannot include any access modifiers.</span></span>


### <a name="namespace-members"></a><span data-ttu-id="5e4e9-258">Namespace 成员</span><span class="sxs-lookup"><span data-stu-id="5e4e9-258">Namespace Members</span></span>

<span data-ttu-id="5e4e9-259">Namespace 成员只能是命名空间声明和类型声明。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-259">Namespace members can only be namespace declarations and type declarations.</span></span> <span data-ttu-id="5e4e9-260">类型声明可能具有`Public`或`Friend`访问。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-260">Type declarations may have `Public` or `Friend` access.</span></span> <span data-ttu-id="5e4e9-261">类型的默认访问是`Friend`访问。</span><span class="sxs-lookup"><span data-stu-id="5e4e9-261">The default access for types is `Friend` access.</span></span>

```antlr
NamespaceMemberDeclaration
    : NamespaceDeclaration
    | TypeDeclaration
    ;

TypeDeclaration
    : ModuleDeclaration
    | NonModuleDeclaration
    ;

NonModuleDeclaration
    : EnumDeclaration
    | StructureDeclaration
    | InterfaceDeclaration
    | ClassDeclaration
    | DelegateDeclaration
    ;
```
