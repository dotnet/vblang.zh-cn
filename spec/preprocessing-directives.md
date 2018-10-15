# <a name="preprocessing-directives"></a><span data-ttu-id="ec913-101">预处理指令</span><span class="sxs-lookup"><span data-stu-id="ec913-101">Preprocessing Directives</span></span>

<span data-ttu-id="ec913-102">后一个文件进行了词法分析，会发生几种类型的源预处理。</span><span class="sxs-lookup"><span data-stu-id="ec913-102">Once a file has been lexically analyzed, several kinds of source preprocessing occur.</span></span> <span data-ttu-id="ec913-103">在最重要的是，条件编译中，确定哪些源处理的语法的语法;两个其他类型的指令-外部源指令和区域指令--提供有关源的元信息但是编译中不起作用。</span><span class="sxs-lookup"><span data-stu-id="ec913-103">The most important, conditional compilation, determines which source is processed by the syntactic grammar; two other types of directives -- external source directives and region directives -- provide meta-information about the source but have no effect on compilation.</span></span>

## <a name="conditional-compilation"></a><span data-ttu-id="ec913-104">条件编译</span><span class="sxs-lookup"><span data-stu-id="ec913-104">Conditional Compilation</span></span>

<span data-ttu-id="ec913-105">条件编译控制逻辑行序列将转换为实际代码。</span><span class="sxs-lookup"><span data-stu-id="ec913-105">Conditional compilation controls whether sequences of logical lines are translated into actual code.</span></span> <span data-ttu-id="ec913-106">在条件编译开始时，启用所有逻辑行;但是，在条件编译语句封闭行可能会有选择地禁用在文件中，从而导致在编译过程的其余部分的过程中忽略这些行。</span><span class="sxs-lookup"><span data-stu-id="ec913-106">At the beginning of conditional compilation, all logical lines are enabled; however, enclosing lines in conditional compilation statements may selectively disable those lines within the file, causing them to be ignored during the rest of the compilation process.</span></span>

```antlr
CCStart
    : CCStatement*
    ;

CCStatement
    : CCConstantDeclaration
    | CCIfGroup
    | LogicalLine
    ;

CCExpression
    : LiteralExpression
    | CCParenthesizedExpression
    | CCSimpleNameExpression
    | CCCastExpression
    | CCOperatorExpression
    | CCConditionalExpression
    ;

CCParenthesizedExpression
    : '(' CCExpression ')'
    ;

CCSimpleNameExpression
    : Identifier
    ;

CCCastExpression
    : 'DirectCast' '(' CCExpression ',' TypeName ')'
    | 'TryCast' '(' CCExpression ',' TypeName ')'
    | 'CType' '(' CCExpression ',' TypeName ')'
    | CastTarget '(' CCExpression ')'
    ;

CCOperatorExpression
    : CCUnaryOperator CCExpression
    | CCExpression CCBinaryOperator CCExpression
    ;

CCUnaryOperator
    : '+' | '-' | 'Not'
    ;

CCBinaryOperator
    : '+'     | '-'     | '*'   | '/'       | '\\'     | 'Mod' | '^' | '='
    | '<' '>' | '<'     | '>'   | '<' '='   | '>' '='  | '&'
    | 'And'   | 'Or'    | 'Xor' | 'AndAlso' | 'OrElse'
    | '<' '<' | '>' '>'
    ;

CCConditionalExpression
    : 'If' '(' CCExpression ',' CCExpression ',' CCExpression ')'
    | 'If' '(' CCExpression ',' CCExpression ')'
    ;
```


<span data-ttu-id="ec913-107">例如，该程序</span><span class="sxs-lookup"><span data-stu-id="ec913-107">For example, the program</span></span>

```vb
#Const A = True
#Const B = False

Class C

#If A Then
    Sub F()
    End Sub
#Else
    Sub G()
    End Sub
#End If

#If B Then
    Sub H()
    End Sub
#Else
    Sub I()
    End Sub
#End If

End Class
```

<span data-ttu-id="ec913-108">生成的程序所在的令牌的确切相同序列</span><span class="sxs-lookup"><span data-stu-id="ec913-108">produces the exact same sequence of tokens as the program</span></span>

```vb
Class C
    Sub F()
    End Sub

    Sub I()
    End Sub
End Class
```

<span data-ttu-id="ec913-109">允许在条件编译指令中使用的常量表达式是常规的常量表达式的子集。</span><span class="sxs-lookup"><span data-stu-id="ec913-109">The constant expressions allowed in conditional compilation directives are a subset of general constant expressions.</span></span>

<span data-ttu-id="ec913-110">预处理器允许空格和显式行继续符之前和之后的每个标记。</span><span class="sxs-lookup"><span data-stu-id="ec913-110">The preprocessor allows whitespace and explicit line continuations before and after every token.</span></span>


### <a name="conditional-constant-directives"></a><span data-ttu-id="ec913-111">条件常量指令</span><span class="sxs-lookup"><span data-stu-id="ec913-111">Conditional Constant Directives</span></span>

<span data-ttu-id="ec913-112">常量的条件语句定义作用域为源文件单独条件编译声明空间中存在的常量。</span><span class="sxs-lookup"><span data-stu-id="ec913-112">Conditional constant statements define constants that exist in a separate conditional compilation declaration space scoped to the source file.</span></span>

```antlr
CCConstantDeclaration
    : '#' 'Const' Identifier '=' CCExpression LineTerminator
    ;
```

<span data-ttu-id="ec913-113">声明空间很特殊，因为没有显式声明的条件编译常量不需要--条件常数可以隐式定义条件编译指令中。</span><span class="sxs-lookup"><span data-stu-id="ec913-113">The declaration space is special in that no explicit declaration of conditional compilation constants is necessary -- conditional constants can be implicitly defined in a conditional compilation directive.</span></span>

<span data-ttu-id="ec913-114">正在分配一个值之前, 条件编译常量具有值`Nothing`。</span><span class="sxs-lookup"><span data-stu-id="ec913-114">Prior to being assigned a value, a conditional compilation constant has the value `Nothing`.</span></span> <span data-ttu-id="ec913-115">条件编译常量分配一个值，该值必须是常量表达式，该常量的类型将成为要传递给它的值的类型。</span><span class="sxs-lookup"><span data-stu-id="ec913-115">When a conditional compilation constant is assigned a value, which must be a constant expression, the type of the constant becomes the type of the value being assigned to it.</span></span> <span data-ttu-id="ec913-116">条件编译常量可能会重新定义多个时间整个源代码文件。</span><span class="sxs-lookup"><span data-stu-id="ec913-116">A conditional compilation constant may be redefined multiple times throughout a source file.</span></span>

<span data-ttu-id="ec913-117">例如，下面的代码将打印只有字符串`about to print value`的值和`Test`。</span><span class="sxs-lookup"><span data-stu-id="ec913-117">For example, the following code prints only the string `about to print value` and the value of `Test`.</span></span>

```vb
Module M1
    Sub PrintValue(Test As Integer)

#Const DebugCode = True

#If DebugCode Then
        Console.WriteLine("about to print value")
#End If

#Const DebugCode = False

        Console.WriteLine(Test)

#If DebugCode Then
        Console.WriteLine("printed value")
#End If

    End Sub
End Module
```

<span data-ttu-id="ec913-118">编译环境也可以在条件编译声明空间中定义条件的常量。</span><span class="sxs-lookup"><span data-stu-id="ec913-118">The compilation environment may also define conditional constants in a conditional compilation declaration space.</span></span>


### <a name="conditional-compilation-directives"></a><span data-ttu-id="ec913-119">条件编译指令</span><span class="sxs-lookup"><span data-stu-id="ec913-119">Conditional Compilation Directives</span></span>

<span data-ttu-id="ec913-120">条件编译指令控制条件编译。</span><span class="sxs-lookup"><span data-stu-id="ec913-120">Conditional compilation directives control conditional compilation.</span></span>

```antlr
CCIfGroup
    : '#' 'If' CCExpression 'Then'? LineTerminator CCStatement*
    CCElseIfGroup* CCElseGroup? '#' 'End' 'If' LineTerminator
    ;

CCElseIfGroup
    : '#' ElseIf CCExpression 'Then'? LineTerminator CCStatement*
    ;

CCElseGroup
    : '#' 'Else' LineTerminator CCStatement*
    ;
```

<span data-ttu-id="ec913-121">常量表达式和条件编译常量只能引用条件常量。</span><span class="sxs-lookup"><span data-stu-id="ec913-121">Conditional constants can only reference constant expressions and conditional compilation constants.</span></span> <span data-ttu-id="ec913-122">每个单个条件编译组内的常量表达式是计算并转换为`Boolean`按文本顺序从第一个到最后一个直到其中一个条件表达式的类型的计算结果为`True`。</span><span class="sxs-lookup"><span data-stu-id="ec913-122">Each of the constant expressions within a single conditional compilation group is evaluated and converted to the `Boolean` type in textual order from first to last until one of the conditional expressions evaluates to `True`.</span></span> <span data-ttu-id="ec913-123">如果表达式不能转换为`Boolean`，编译时错误结果。</span><span class="sxs-lookup"><span data-stu-id="ec913-123">If an expression is not convertible to `Boolean`, a compile-time error results.</span></span> <span data-ttu-id="ec913-124">计算条件编译常量表达式，而不考虑任何时始终使用宽松的语义和二进制字符串比较`Option`指令或编译环境设置。</span><span class="sxs-lookup"><span data-stu-id="ec913-124">Permissive semantics and binary string comparisons are always used when evaluating conditional compilation constant expressions, regardless of any `Option` directives or compilation environment settings.</span></span>

<span data-ttu-id="ec913-125">括在组中，包括嵌套的条件编译指令的所有行处于禁用都状态之间包含的语句的行除外`True`表达式和组或介于之间的行的下一步的条件语句`Else`语句和`End If`语句如果`Else`出现在组中的所有表达式的计算结果为和`False`。</span><span class="sxs-lookup"><span data-stu-id="ec913-125">All lines enclosed by the group, including nested conditional compilation directives, are disabled except for lines between the statement containing the `True` expression and the next conditional statement of the group, or lines between the `Else` statement and the `End If` statement if an `Else` appears in the group and all of the expressions evaluate to `False`.</span></span>

<span data-ttu-id="ec913-126">在此示例中，在调用`WriteToLog`中`Trace`条件编译指令则不会处理因为周围的`Debug`条件编译指令的计算结果为`False`。</span><span class="sxs-lookup"><span data-stu-id="ec913-126">In this example, the call to `WriteToLog` in the `Trace` conditional compilation directive is not processed because the surrounding `Debug` conditional compilation directive evaluates to `False`.</span></span>

```vb
#Const Debug = False   ' Debugging off
#Const Trace = True    ' Tracing on

Class PurchaseTransaction
    Sub Commit()

#If Debug Then
        CheckConsistency()
#If Trace Then
        WriteToLog(Me.ToString())
#End If
#End If
        ...
    End Sub
End Class
```


## <a name="external-source-directives"></a><span data-ttu-id="ec913-127">外部源指令</span><span class="sxs-lookup"><span data-stu-id="ec913-127">External Source Directives</span></span>

<span data-ttu-id="ec913-128">源代码文件可以包含外部源指令，以指示源行和源的外部文本之间的映射。</span><span class="sxs-lookup"><span data-stu-id="ec913-128">A source file may include external source directives that indicate a mapping between source lines and text external to the source.</span></span>

```antlr
ESDStart
    : ExternalSourceStatement*
    ;

ExternalSourceStatement
    : ExternalSourceGroup
    | LogicalLine
    ;

ExternalSourceGroup
    : '#' 'ExternalSource' '(' StringLiteral ',' IntLiteral ')' LineTerminator
      LogicalLine* '#' 'End' 'ExternalSource' LineTerminator
    ;
```

<span data-ttu-id="ec913-129">外部源指令不会影响编译，而且不能嵌套。</span><span class="sxs-lookup"><span data-stu-id="ec913-129">External source directives have no effect on compilation and may not be nested.</span></span> <span data-ttu-id="ec913-130">例如：</span><span class="sxs-lookup"><span data-stu-id="ec913-130">For example:</span></span>

```vb
Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```


## <a name="region-directives"></a><span data-ttu-id="ec913-131">区域指令</span><span class="sxs-lookup"><span data-stu-id="ec913-131">Region Directives</span></span>

<span data-ttu-id="ec913-132">区域指令的源代码行进行分组，但对编译没有其他影响。</span><span class="sxs-lookup"><span data-stu-id="ec913-132">Region directives group lines of source code but have no other effect on compilation.</span></span> <span data-ttu-id="ec913-133">可以折叠和隐藏的或展开和在集成的开发环境 (IDE) 中查看整个组。</span><span class="sxs-lookup"><span data-stu-id="ec913-133">The entire group can be collapsed and hidden, or expanded and viewed, in the integrated development environment (IDE).</span></span>

```antlr
RegionStart
    : RegionStatement*
    ;

RegionStatement
    : RegionGroup
    | LogicalLine
    ;

RegionGroup
    : '#' 'Region' StringLiteral LineTerminator
      RegionStatement*
      '#' 'End' 'Region' LineTerminator
    ;
```

<span data-ttu-id="ec913-134">可以嵌套区域。</span><span class="sxs-lookup"><span data-stu-id="ec913-134">Regions may be nested.</span></span> <span data-ttu-id="ec913-135">区域指令是程序的特殊，因为它们不能启动或终止的方法主体，并且它们必须遵循块结构。</span><span class="sxs-lookup"><span data-stu-id="ec913-135">Region directives are special in that they can neither start nor terminate within a method body, and they must respect the block structure of the program.</span></span> <span data-ttu-id="ec913-136">例如：</span><span class="sxs-lookup"><span data-stu-id="ec913-136">For example:</span></span>

```vb
Module Test
#Region "Startup code - do not edit"
    Sub Main()
    End Sub
#End Region

End Module


' Error due to Region directives breaking the block structure
Class C
#Region "Fred"
End Class
#End Region
```


## <a name="external-checksum-directives"></a><span data-ttu-id="ec913-137">外部校验和指令</span><span class="sxs-lookup"><span data-stu-id="ec913-137">External Checksum Directives</span></span>

<span data-ttu-id="ec913-138">源代码文件可以包含一个外部校验和指令，指示哪些校验和不应发出一个外部源指令中引用的文件。</span><span class="sxs-lookup"><span data-stu-id="ec913-138">A source file may include an external checksum directive that indicates what checksum should be emitted for a file referenced in an external source directive.</span></span> <span data-ttu-id="ec913-139">在所有其他方面外部源指令没有任何影响编译。</span><span class="sxs-lookup"><span data-stu-id="ec913-139">In all other respects external source directives have no effect on compilation.</span></span>

```antlr
ExternalChecksumStart
    : ExternalChecksumStatement*
    ;

ExternalChecksumStatement
    : '#' 'ExternalChecksum' '('
      StringLiteral ',' StringLiteral ',' StringLiteral
      ')' LineTerminator
    ;
```

<span data-ttu-id="ec913-140">外部校验和指令包含外部文件的文件和文件的校验和与关联的全局唯一标识符 (GUID) 的文件名。</span><span class="sxs-lookup"><span data-stu-id="ec913-140">An external checksum directive contains the filename of the external file, a globally unique identifier (GUID) associated with the file and the checksum for the file.</span></span> <span data-ttu-id="ec913-141">为窗体"{xxxxxxxx xxxx-xxxx-xxxx-在左右加上}"，其中 x 是十六进制数字的字符串常量指定 GUID。</span><span class="sxs-lookup"><span data-stu-id="ec913-141">The GUID is specified as a string constant of the form "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}", where x is a hexadecimal digit.</span></span> <span data-ttu-id="ec913-142">校验和指定为窗体的"...xxxx"，其中 x 是十六进制数字的字符串常数。</span><span class="sxs-lookup"><span data-stu-id="ec913-142">The checksum is specified as a string constant of the form "xxxx...", where x is a hexadecimal digit.</span></span> <span data-ttu-id="ec913-143">校验和中的数字个数必须为偶数。</span><span class="sxs-lookup"><span data-stu-id="ec913-143">The number of digits in a checksum must be an even number.</span></span>

<span data-ttu-id="ec913-144">外部文件可能具有多个外部校验和指令，前提是所有的 GUID 和校验和值完全匹配与之关联。</span><span class="sxs-lookup"><span data-stu-id="ec913-144">An external file may have multiple external checksum directives associated with it provided that all of the GUID and checksum values match exactly.</span></span> <span data-ttu-id="ec913-145">如果正在编译的文件的名称匹配的外部文件的名称，被忽略校验和，以便编译器的校验和计算支持。</span><span class="sxs-lookup"><span data-stu-id="ec913-145">If the name of the external file matches the name of a file being compiled, the checksum is ignored in favor of the compiler's checksum calculation.</span></span>

<span data-ttu-id="ec913-146">例如：</span><span class="sxs-lookup"><span data-stu-id="ec913-146">For example:</span></span>

```vb
#ExternalChecksum("c:\wwwroot\inetpub\test.aspx", _
    "{12345678-1234-1234-1234-123456789abc}", _
    "1a2b3c4e5f617239a49b9a9c0391849d34950f923fab9484")

Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```

