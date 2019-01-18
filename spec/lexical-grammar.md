---
ms.openlocfilehash: 2f14161222741beb7505f5674b230f1881828b5d
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426636"
---
# <a name="lexical-grammar"></a><span data-ttu-id="194a6-101">词法语法</span><span class="sxs-lookup"><span data-stu-id="194a6-101">Lexical Grammar</span></span>

<span data-ttu-id="194a6-102">首先，编译 Visual Basic 程序需要将 Unicode 字符的原始流转换为一组有序词汇标记。</span><span class="sxs-lookup"><span data-stu-id="194a6-102">Compilation of a Visual Basic program first involves translating the raw stream of Unicode characters into an ordered set of lexical tokens.</span></span> <span data-ttu-id="194a6-103">由于 Visual Basic 语言不是无格式，组标记然后进一步划分为一系列逻辑行。</span><span class="sxs-lookup"><span data-stu-id="194a6-103">Because the Visual Basic language is not free-format, the set of tokens is then further divided into a series of logical lines.</span></span> <span data-ttu-id="194a6-104">一个*逻辑行*范围从流的起始行结束符通过到下一步前面没有由行继续符或通过到流的末尾的行终止符。</span><span class="sxs-lookup"><span data-stu-id="194a6-104">A *logical line* spans from either the start of the stream or a line terminator through to the next line terminator that is not preceded by a line continuation or through to the end of the stream.</span></span>

<span data-ttu-id="194a6-105">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="194a6-105">__Note.__</span></span> <span data-ttu-id="194a6-106">XML 9.0 版中的语言的文本表达式的引入，Visual Basic 不再具有不同的词法语法意义上的 Visual Basic 代码可以标记化而不考虑的语义上下文。</span><span class="sxs-lookup"><span data-stu-id="194a6-106">With the introduction of XML literal expressions in version 9.0 of the language, Visual Basic no longer has a distinct lexical grammar in the sense that Visual Basic code can be tokenized without regard to the syntactic context.</span></span> <span data-ttu-id="194a6-107">这是由于访问的 XML 和 Visual Basic 了不同的词法规则，在任何特定时间使用的词汇规则集取决于当时正在处理哪些语法构造。</span><span class="sxs-lookup"><span data-stu-id="194a6-107">This is due to the fact that XML and Visual Basic have different lexical rules and the set of lexical rules in use at any particular time depends on what syntactic construct is being processed at that moment.</span></span> <span data-ttu-id="194a6-108">此规范保留此词法语法部分作为常规 Visual Basic 代码的词汇规则的指南。</span><span class="sxs-lookup"><span data-stu-id="194a6-108">This specification retains this lexical grammar section as a guide to the lexical rules of regular Visual Basic code.</span></span>

```antlr
LogicalLineStart
    : LogicalLine*
    ;

LogicalLine
    : LogicalLineElement* Comment? LineTerminator
    ;

LogicalLineElement
    : WhiteSpace
    | LineContinuation
    | Token
    ;

Token
    : Identifier
    | Keyword
    | Literal
    | Separator
    | Operator
    ;
```

## <a name="characters-and-lines"></a><span data-ttu-id="194a6-109">字符和行</span><span class="sxs-lookup"><span data-stu-id="194a6-109">Characters and Lines</span></span>

<span data-ttu-id="194a6-110">Visual Basic 程序中的 Unicode 字符的字符组成。</span><span class="sxs-lookup"><span data-stu-id="194a6-110">Visual Basic programs are composed of characters from the Unicode character set.</span></span>

```antlr
Character:
    '<Any Unicode character except a LineTerminator>'
    ;
```

### <a name="line-terminators"></a><span data-ttu-id="194a6-111">行终止符</span><span class="sxs-lookup"><span data-stu-id="194a6-111">Line Terminators</span></span>

<span data-ttu-id="194a6-112">Unicode 换行符分隔逻辑行。</span><span class="sxs-lookup"><span data-stu-id="194a6-112">Unicode line break characters separate logical lines.</span></span>

```antlr
LineTerminator
    : '<Unicode 0x00D>'
    | '<Unicode 0x00A>'
    | '<CR>'
    | '<LF>'
    | '<Unicode 0x2028>'
    | '<Unicode 0x2029>'
    ;
```

### <a name="line-continuation"></a><span data-ttu-id="194a6-113">行继续符</span><span class="sxs-lookup"><span data-stu-id="194a6-113">Line Continuation</span></span>

<span data-ttu-id="194a6-114">一个*行继续符*紧跟一个下划线字符作为文本行中的最后一个字符 （而不是空格） 的至少一个空白字符组成。</span><span class="sxs-lookup"><span data-stu-id="194a6-114">A *line continuation* consists of at least one white-space character that immediately precedes a single underscore character as the last character (other than white space) in a text line.</span></span> <span data-ttu-id="194a6-115">行继续符允许逻辑行以跨多个物理行的开头。</span><span class="sxs-lookup"><span data-stu-id="194a6-115">A line continuation allows a logical line to span more than one physical line.</span></span> <span data-ttu-id="194a6-116">行继续符都被看作是空格，即使它们不是。</span><span class="sxs-lookup"><span data-stu-id="194a6-116">Line continuations are treated as if they were white space, even though they are not.</span></span>

```antlr
LineContinuation
    : WhiteSpace '_' WhiteSpace* LineTerminator
    ;
```

<span data-ttu-id="194a6-117">以下程序显示了一些行继续符：</span><span class="sxs-lookup"><span data-stu-id="194a6-117">The following program shows some line continuations:</span></span>

```vb
Module Test
    Sub Print( _
        Param1 As Integer, _
        Param2 As Integer )

        If (Param1 < Param2) Or _
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

<span data-ttu-id="194a6-118">允许的语法的语法中的一些地方*隐式行继续符*。</span><span class="sxs-lookup"><span data-stu-id="194a6-118">Some places in the syntactic grammar allow for *implicit line continuations*.</span></span> <span data-ttu-id="194a6-119">当遇到行终止符</span><span class="sxs-lookup"><span data-stu-id="194a6-119">When a line terminator is encountered</span></span>

* <span data-ttu-id="194a6-120">逗号 (`,`)，请打开括号 (`(`)、 左大括号 (`{`)，或打开嵌入式的表达式 (`<%=`)</span><span class="sxs-lookup"><span data-stu-id="194a6-120">after a comma (`,`), open parenthesis (`(`), open curly brace (`{`), or open embedded expression (`<%=`)</span></span>

* <span data-ttu-id="194a6-121">后成员限定符 (`.`或`.@`或`...`)，前提是正在限定的内容 (即未使用隐式`With`上下文)</span><span class="sxs-lookup"><span data-stu-id="194a6-121">after a member qualifier (`.` or `.@` or `...`), provided that something is being qualified (i.e. is not using an implicit `With` context)</span></span>

* <span data-ttu-id="194a6-122">在右括号之前 (`)`)，右大括号 (`}`)，或嵌入式表达式结束标记 (`%>`)</span><span class="sxs-lookup"><span data-stu-id="194a6-122">before a close parenthesis (`)`), close curly brace (`}`), or close embedded expression (`%>`)</span></span>

* <span data-ttu-id="194a6-123">在小于-比 (`<`) 在属性的上下文中</span><span class="sxs-lookup"><span data-stu-id="194a6-123">after a less-than (`<`) in an attribute context</span></span>

* <span data-ttu-id="194a6-124">前一个大于-比 (`>`) 在属性的上下文中</span><span class="sxs-lookup"><span data-stu-id="194a6-124">before a greater-than (`>`) in an attribute context</span></span>

* <span data-ttu-id="194a6-125">后一个大于-比 (`>`) 在非文件级别的属性的上下文中</span><span class="sxs-lookup"><span data-stu-id="194a6-125">after a greater-than (`>`) in a non-file-level attribute context</span></span>

* <span data-ttu-id="194a6-126">之前和之后查询运算符 (`Where`， `Order`， `Select`，等等。)</span><span class="sxs-lookup"><span data-stu-id="194a6-126">before and after query operators (`Where`, `Order`, `Select`, etc.)</span></span>

* <span data-ttu-id="194a6-127">在二元运算符 (`+`， `-`， `/`，`*`等) 在表达式的上下文中</span><span class="sxs-lookup"><span data-stu-id="194a6-127">after binary operators (`+`, `-`, `/`, `*`, etc.) in an expression context</span></span>

* <span data-ttu-id="194a6-128">在赋值运算符 (`=`， `:=`， `+=`，`-=`等) 在任何上下文中。</span><span class="sxs-lookup"><span data-stu-id="194a6-128">after assignment operators (`=`, `:=`, `+=`, `-=`, etc.) in any context.</span></span>

<span data-ttu-id="194a6-129">就像它是行继续符处理的行终止符。</span><span class="sxs-lookup"><span data-stu-id="194a6-129">the line terminator is treated as if it was a line continuation.</span></span>

```antlr
Comma
    : ',' LineTerminator?
    ;

Period
    : '.' LineTerminator?
    ;

OpenParenthesis
    : '(' LineTerminator?
    ;

CloseParenthesis
    : LineTerminator? ')'
    ;

OpenCurlyBrace
    : '{' LineTerminator?
    ;

CloseCurlyBrace
    : LineTerminator? '}'
    ;

Equals
    : '=' LineTerminator?
    ;

ColonEquals
    : ':' '=' LineTerminator?
    ;
```

<span data-ttu-id="194a6-130">例如，还可以作为编写前面的示例：</span><span class="sxs-lookup"><span data-stu-id="194a6-130">For example, the previous example could also be written as:</span></span>

```vb
Module Test
    Sub Print(
        Param1 As Integer,
        Param2 As Integer)

        If (Param1 < Param2) Or
            (Param1 > Param2) Then
            Console.WriteLine("Not equal")
        End If
    End Function
End Module
```

<span data-ttu-id="194a6-131">直接之前或之后指定的标记，则只会将推断隐式行继续符。</span><span class="sxs-lookup"><span data-stu-id="194a6-131">Implicit line continuations will only ever be inferred directly before or after the specified token.</span></span> <span data-ttu-id="194a6-132">它们将不会推断之前或之后行继续符。</span><span class="sxs-lookup"><span data-stu-id="194a6-132">They will not be inferred before or after a line continuation.</span></span> <span data-ttu-id="194a6-133">例如：</span><span class="sxs-lookup"><span data-stu-id="194a6-133">For example:</span></span>

```vb
Dim y = 10
' Error: Expression expected for assignment to x
Dim x = _

y
```

<span data-ttu-id="194a6-134">将条件编译的上下文中推断出行继续符。</span><span class="sxs-lookup"><span data-stu-id="194a6-134">Line continuations will not be inferred in conditional compilation contexts.</span></span> <span data-ttu-id="194a6-135">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="194a6-135">(__Note.__</span></span> <span data-ttu-id="194a6-136">此最后一个限制是必需的因为未编译的条件编译块中的文本不需要在语法上有效。</span><span class="sxs-lookup"><span data-stu-id="194a6-136">This last restriction is required because text in conditional compilation blocks that are not compiled do not have to be syntactically valid.</span></span> <span data-ttu-id="194a6-137">因此，块中的文本可能会意外地获取"选取"条件编译语句中，尤其是当语言获取将来扩展。）</span><span class="sxs-lookup"><span data-stu-id="194a6-137">Thus, text in the block might accidentally get "picked up" by the conditional compilation statement, especially as the language gets extended in the future.)</span></span>


### <a name="white-space"></a><span data-ttu-id="194a6-138">空白区域</span><span class="sxs-lookup"><span data-stu-id="194a6-138">White Space</span></span>

<span data-ttu-id="194a6-139">*空白*仅用于分隔标记并将忽略此设置。</span><span class="sxs-lookup"><span data-stu-id="194a6-139">*White space* serves only to separate tokens and is otherwise ignored.</span></span> <span data-ttu-id="194a6-140">逻辑行包含仅为空白将被忽略。</span><span class="sxs-lookup"><span data-stu-id="194a6-140">Logical lines containing only white space are ignored.</span></span> <span data-ttu-id="194a6-141">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="194a6-141">(__Note.__</span></span>
<span data-ttu-id="194a6-142">行终止符是不被视为空白区域。）</span><span class="sxs-lookup"><span data-stu-id="194a6-142">Line terminators are not considered white space.)</span></span>

```antlr
WhiteSpace
    : '<Unicode class Zs>'
    | '<Unicode Tab 0x0009>'
    ;
```

### <a name="comments"></a><span data-ttu-id="194a6-143">注释</span><span class="sxs-lookup"><span data-stu-id="194a6-143">Comments</span></span>

<span data-ttu-id="194a6-144">一个*注释*单引号字符或关键字开头`REM`。</span><span class="sxs-lookup"><span data-stu-id="194a6-144">A *comment* begins with a single-quote character or the keyword `REM`.</span></span> <span data-ttu-id="194a6-145">单引号字符是 ASCII 单引号字符、 Unicode 左的单引号字符或 Unicode 右单引号字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-145">A single-quote character is either an ASCII single-quote character, a Unicode left single-quote character, or a Unicode right single-quote character.</span></span> <span data-ttu-id="194a6-146">注释可以在源行、 上任意位置开始，物理行尾结束注释。</span><span class="sxs-lookup"><span data-stu-id="194a6-146">Comments can begin anywhere on a source line, and the end of the physical line ends the comment.</span></span> <span data-ttu-id="194a6-147">编译器将忽略注释的开头之间的行终止符的字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-147">The compiler ignores the characters between the beginning of the comment and the line terminator.</span></span> <span data-ttu-id="194a6-148">因此，注释不能跨多个行扩展通过使用行继续符。</span><span class="sxs-lookup"><span data-stu-id="194a6-148">Consequently, comments cannot extend across multiple lines by using line continuations.</span></span>

```antlr
Comment
    : CommentMarker Character*
    ;

CommentMarker
    : SingleQuoteCharacter
    | 'REM'
    ;

SingleQuoteCharacter
    : '\''
    | '<Unicode 0x2018>'
    | '<Unicode 0x2019>'
    ;
```

## <a name="identifiers"></a><span data-ttu-id="194a6-149">标识符</span><span class="sxs-lookup"><span data-stu-id="194a6-149">Identifiers</span></span>

<span data-ttu-id="194a6-150">*标识符*是一个名称。</span><span class="sxs-lookup"><span data-stu-id="194a6-150">An *identifier* is a name.</span></span> <span data-ttu-id="194a6-151">Visual Basic 标识符符合 Unicode 标准附录 15 有一个例外： 标识符可能以下划线 （连接器） 字符开头。</span><span class="sxs-lookup"><span data-stu-id="194a6-151">Visual Basic identifiers conform to the Unicode Standard Annex 15 with one exception: identifiers may begin with an underscore (connector) character.</span></span> <span data-ttu-id="194a6-152">如果标识符以下划线开头，则必须包含至少一个其他有效的标识符字符，以区别于行继续符。</span><span class="sxs-lookup"><span data-stu-id="194a6-152">If an identifier begins with an underscore, it must contain at least one other valid identifier character to disambiguate it from a line continuation.</span></span>

```antlr
Identifier
    : NonEscapedIdentifier TypeCharacter?
    | Keyword TypeCharacter
    | EscapedIdentifier
    ;

NonEscapedIdentifier
    : '<Any IdentifierName but not Keyword>'
    ;

EscapedIdentifier
    : '[' IdentifierName ']'
    ;

IdentifierName
    : IdentifierStart IdentifierCharacter*
    ;

IdentifierStart
    : AlphaCharacter
    | UnderscoreCharacter IdentifierCharacter
    ;

IdentifierCharacter
    : UnderscoreCharacter
    | AlphaCharacter
    | NumericCharacter
    | CombiningCharacter
    | FormattingCharacter
    ;

AlphaCharacter
    : '<Unicode classes Lu,Ll,Lt,Lm,Lo,Nl>'
    ;

NumericCharacter
    : '<Unicode decimal digit class Nd>'
    ;

CombiningCharacter
    : '<Unicode combining character classes Mn, Mc>'
    ;

FormattingCharacter
    : '<Unicode formatting character class Cf>'
    ;

UnderscoreCharacter
    : '<Unicode connection character class Pc>'
    ;

IdentifierOrKeyword
    : Identifier
    | Keyword
    ;
```

<span data-ttu-id="194a6-153">常规标识符可能不与关键字冲突，但可以转义的标识符或标识符类型字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-153">Regular identifiers may not match keywords, but escaped identifiers or identifiers with a type character can.</span></span> <span data-ttu-id="194a6-154">*转义标识符*是由方括号分隔的标识符。</span><span class="sxs-lookup"><span data-stu-id="194a6-154">An *escaped identifier* is an identifier delimited by square brackets.</span></span> <span data-ttu-id="194a6-155">转义标识符遵循与常规标识符相同的规则，只不过它们可能与关键字冲突，并且可能不具有类型字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-155">Escaped identifiers follow the same rules as regular identifiers except that they may match keywords and may not have type characters.</span></span>

<span data-ttu-id="194a6-156">此示例定义一个名为`class`与名为共享方法`shared`采用名为的参数`boolean`，然后调用该方法。</span><span class="sxs-lookup"><span data-stu-id="194a6-156">This example defines a class named `class` with a shared method named `shared` that takes a parameter named `boolean` and then calls the method.</span></span>

```vb
Class [class]
    Shared Sub [shared]([boolean] As Boolean)
        If [boolean] Then
            Console.WriteLine("true")
        Else
            Console.WriteLine("false")
        End If
    End Sub
End Class

Module [module]
    Sub Main()
        [class].[shared](True)
    End Sub
End Module
```

<span data-ttu-id="194a6-157">标识符是不区分大小写，因此两个标识符被视为是相同的标识符，如果它们的区别仅在于大小写。</span><span class="sxs-lookup"><span data-stu-id="194a6-157">Identifiers are case insensitive, so two identifiers are considered to be the same identifier if they differ only in case.</span></span> <span data-ttu-id="194a6-158">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="194a6-158">(__Note.__</span></span> <span data-ttu-id="194a6-159">在比较标识符时所使用的 Unicode 标准一对一大小写映射以及任何特定于区域设置的大小写映射将被忽略。）</span><span class="sxs-lookup"><span data-stu-id="194a6-159">The Unicode Standard one-to-one case mappings are used when comparing identifiers and any locale-specific case mappings are ignored.)</span></span>


### <a name="type-characters"></a><span data-ttu-id="194a6-160">类型字符</span><span class="sxs-lookup"><span data-stu-id="194a6-160">Type Characters</span></span>

<span data-ttu-id="194a6-161">一个*类型字符*表示上述标识符的类型。</span><span class="sxs-lookup"><span data-stu-id="194a6-161">A *type character* denotes the type of the preceding identifier.</span></span> <span data-ttu-id="194a6-162">类型字符不是被视为标识符的一部分。</span><span class="sxs-lookup"><span data-stu-id="194a6-162">The type character is not considered part of the identifier.</span></span>

```antlr
TypeCharacter
    : IntegerTypeCharacter
    | LongTypeCharacter
    | DecimalTypeCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | StringTypeCharacter
    ;

IntegerTypeCharacter
    : '%'
    ;

LongTypeCharacter
    : '&'
    ;

DecimalTypeCharacter
    : '@'
    ;

SingleTypeCharacter
    : '!'
    ;

DoubleTypeCharacter
    : '#'
    ;

StringTypeCharacter
    : '$'
    ;
```

<span data-ttu-id="194a6-163">如果声明包含类型字符，必须与声明本身; 中指定的类型一致的类型字符否则将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="194a6-163">If a declaration includes a type character, the type character must agree with the type specified in the declaration itself; otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="194a6-164">如果声明中省略类型 (例如，如果未指定`As`子句)，作为声明的类型隐式替换类型字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-164">If the declaration omits the type (for example, if it does not specify an `As` clause), the type character is implicitly substituted as the type of the declaration.</span></span>

<span data-ttu-id="194a6-165">标识符和它的类型字符之间，可能会没有空格。</span><span class="sxs-lookup"><span data-stu-id="194a6-165">No white space may come between an identifier and its type character.</span></span> <span data-ttu-id="194a6-166">对于没有类型字符`Byte`， `SByte`， `UShort`， `Short`，`UInteger`或`ULong`，由于缺乏合适的字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-166">There are no type characters for `Byte`, `SByte`, `UShort`, `Short`, `UInteger` or `ULong`, due to a lack of suitable characters.</span></span>

<span data-ttu-id="194a6-167">追加到从概念上讲不具有类型 （例如，命名空间名称） 的标识符或其类型否定字符类型的类型的标识符类型字符会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="194a6-167">Appending a type character to an identifier that conceptually does not have a type (for example, a namespace name) or to an identifier whose type disagrees with the type of the type character causes a compile-time error.</span></span>

<span data-ttu-id="194a6-168">下面的示例演示使用类型字符：</span><span class="sxs-lookup"><span data-stu-id="194a6-168">The following example shows the use of type characters:</span></span>

```vb
' The follow line will cause an error: standard modules have no type.
Module Test1#
End Module

Module Test2

    ' This function takes a Long parameter and returns a String.
    Function Func$(Param&)

        ' The following line causes an error because the type character
        ' conflicts with the declared type of Func and Param.
        Func# = CStr(Param@)

        ' The following line is valid.
        Func$ = CStr(Param&)
    End Function
End Module
```

<span data-ttu-id="194a6-169">类型字符`!`面临一个特殊问题，因为它可以使用类型字符作为和语言中的分隔符。</span><span class="sxs-lookup"><span data-stu-id="194a6-169">The type character `!` presents a special problem in that it can be used both as a type character and as a separator in the language.</span></span> <span data-ttu-id="194a6-170">若要消除歧义，`!`字符，只要它后面的字符不能启动标识符是类型字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-170">To remove ambiguity, a `!` character is a type character as long as the character that follows it cannot start an identifier.</span></span> <span data-ttu-id="194a6-171">如果可以则`!`字符是一个分隔符，而不是类型字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-171">If it can, then the `!` character is a separator, not a type character.</span></span>


## <a name="keywords"></a><span data-ttu-id="194a6-172">关键字</span><span class="sxs-lookup"><span data-stu-id="194a6-172">Keywords</span></span>

<span data-ttu-id="194a6-173">一个*关键字*是一种语言构造中具有特殊含义的词。</span><span class="sxs-lookup"><span data-stu-id="194a6-173">A *keyword* is a word that has special meaning in a language construct.</span></span> <span data-ttu-id="194a6-174">所有关键字保留的语言，除非标识符进行转义，否则可能不作为标识符使用。</span><span class="sxs-lookup"><span data-stu-id="194a6-174">All keywords are reserved by the language and may not be used as identifiers unless the identifiers are escaped.</span></span> <span data-ttu-id="194a6-175">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="194a6-175">(__Note.__</span></span> <span data-ttu-id="194a6-176">`EndIf``GoSub`， `Let`， `Variant`，和`Wend`保留 as 关键字，尽管 Visual Basic 中不再使用。)</span><span class="sxs-lookup"><span data-stu-id="194a6-176">`EndIf`, `GoSub`, `Let`, `Variant`, and `Wend` are retained as keywords, although they are no longer used in Visual Basic.)</span></span>

```antlr
Keyword
    : 'AddHandler'      | 'AddressOf'      | 'Alias'       | 'And'
    | 'AndAlso'         | 'As'             | 'Boolean'     | 'ByRef'
    | 'Byte'            | 'ByVal'          | 'Call'        | 'Case'        
    | 'Catch'           | 'CBool'          | 'CByte'       | 'CChar'       
    | 'CDate'           | 'CDbl'           | 'CDec'        | 'Char'        
    | 'CInt'            | 'Class'          | 'CLng'        | 'CObj'        
    | 'Const'           | 'Continue'       | 'CSByte'      | 'CShort'      
    | 'CSng'            | 'CStr'           | 'CType'       | 'CUInt'       
    | 'CULng'           | 'CUShort'        | 'Date'        | 'Decimal'     
    | 'Declare'         | 'Default'        | 'Delegate'    | 'Dim'         
    | 'DirectCast'      | 'Do'             | 'Double'      | 'Each'        
    | 'Else'            | 'ElseIf'         | 'End'         | 'EndIf'       
    | 'Enum'            | 'Erase'          | 'Error'       | 'Event'       
    | 'Exit'            | 'False'          | 'Finally'     | 'For'         
    | 'Friend'          | 'Function'       | 'Get'         | 'GetType'     
    | 'GetXmlNamespace' | 'Global'         | 'GoSub'       | 'GoTo'        
    | 'Handles'         | 'If'             | 'Implements'  | 'Imports'     
    | 'In'              | 'Inherits'       | 'Integer'     | 'Interface'   
    | 'Is'              | 'IsNot'          | 'Let'         | 'Lib'         
    | 'Like'            | 'Long'           | 'Loop'        | 'Me'          
    | 'Mod'             | 'Module'         | 'MustInherit' | 'MustOverride'
    | 'MyBase'          | 'MyClass'        | 'Namespace'   | 'Narrowing'   
    | 'New'             | 'Next'           | 'Not'         | 'Nothing'     
    | 'NotInheritable'  | 'NotOverridable' | 'Object'      | 'Of'          
    | 'On'              | 'Operator'       | 'Option'      | 'Optional'    
    | 'Or'              | 'OrElse'         | 'Overloads'   | 'Overridable' 
    | 'Overrides'       | 'ParamArray'     | 'Partial'     | 'Private'     
    | 'Property'        | 'Protected'      | 'Public'      | 'RaiseEvent'  
    | 'ReadOnly'        | 'ReDim'          | 'REM'         | 'RemoveHandler'
    | 'Resume'          | 'Return'         | 'SByte'       | 'Select'      
    | 'Set'             | 'Shadows'        | 'Shared'      | 'Short'       
    | 'Single'          | 'Static'         | 'Step'        | 'Stop'        
    | 'String'          | 'Structure'      | 'Sub'         | 'SyncLock'    
    | 'Then'            | 'Throw'          | 'To'          | 'True'        
    | 'Try'             | 'TryCast'        | 'TypeOf'      | 'UInteger'    
    | 'ULong'           | 'UShort'         | 'Using'       | 'Variant'     
    | 'Wend'            | 'When'           | 'While'       | 'Widening'    
    | 'With'            | 'WithEvents'     | 'WriteOnly'   | 'Xor'         
    ;
```

## <a name="literals"></a><span data-ttu-id="194a6-177">文本</span><span class="sxs-lookup"><span data-stu-id="194a6-177">Literals</span></span>

<span data-ttu-id="194a6-178">一个*文字*是一种类型的特定值的文本表示形式。</span><span class="sxs-lookup"><span data-stu-id="194a6-178">A *literal* is a textual representation of a particular value of a type.</span></span> <span data-ttu-id="194a6-179">文本类型包括 boolean 类型的值、 整数、 浮点数、 字符串、 字符和日期。</span><span class="sxs-lookup"><span data-stu-id="194a6-179">Literal types include Boolean, integer, floating point, string, character, and date.</span></span>

```antlr
Literal
    : BooleanLiteral
    | IntegerLiteral
    | FloatingPointLiteral
    | StringLiteral
    | CharacterLiteral
    | DateLiteral
    | Nothing
    ;
```

### <a name="boolean-literals"></a><span data-ttu-id="194a6-180">布尔值文字</span><span class="sxs-lookup"><span data-stu-id="194a6-180">Boolean Literals</span></span>

<span data-ttu-id="194a6-181">`True` 并`False`是文本的`Boolean`分别映射到 true 和 false 状态的类型。</span><span class="sxs-lookup"><span data-stu-id="194a6-181">`True` and `False` are literals of the `Boolean` type that map to the true and false state, respectively.</span></span>

```antlr
BooleanLiteral
    : 'True' | 'False'
    ;
```

### <a name="integer-literals"></a><span data-ttu-id="194a6-182">整数文本</span><span class="sxs-lookup"><span data-stu-id="194a6-182">Integer Literals</span></span>

<span data-ttu-id="194a6-183">整数文本可以是十进制 (基数为 10)、 十六进制 (基数为 16) 或八进制 (基数为 8)。</span><span class="sxs-lookup"><span data-stu-id="194a6-183">Integer literals can be decimal (base 10), hexadecimal (base 16), or octal (base 8).</span></span> <span data-ttu-id="194a6-184">十进制整数文本是十进制数字 (0-9) 的字符串。</span><span class="sxs-lookup"><span data-stu-id="194a6-184">A decimal integer literal is a string of decimal digits (0-9).</span></span> <span data-ttu-id="194a6-185">十六进制文本是`&H`跟十六进制数字 (0-9、 A-F) 的字符串。</span><span class="sxs-lookup"><span data-stu-id="194a6-185">A hexadecimal literal is `&H` followed by a string of hexadecimal digits (0-9, A-F).</span></span> <span data-ttu-id="194a6-186">八进制文本是`&O`跟八进制数字 (0-7) 的字符串。</span><span class="sxs-lookup"><span data-stu-id="194a6-186">An octal literal is `&O` followed by a string of octal digits (0-7).</span></span> <span data-ttu-id="194a6-187">十进制文本直接表示整型文本，十进制值，而则八进制和十六进制文本表示整数的二进制值 (因此，`&H8000S`是-32768，不是溢出错误)。</span><span class="sxs-lookup"><span data-stu-id="194a6-187">Decimal literals directly represent the decimal value of the integral literal, whereas octal and hexadecimal literals represent the binary value of the integer literal (thus, `&H8000S` is -32768, not an overflow error).</span></span>

```antlr
IntegerLiteral
    : IntegralLiteralValue IntegralTypeCharacter?
    ;

IntegralLiteralValue
    : IntLiteral
    | HexLiteral
    | OctalLiteral
    ;

IntegralTypeCharacter
    : ShortCharacter
    | UnsignedShortCharacter
    | IntegerCharacter
    | UnsignedIntegerCharacter
    | LongCharacter
    | UnsignedLongCharacter
    | IntegerTypeCharacter
    | LongTypeCharacter
    ;

ShortCharacter
    : 'S'
    ;

UnsignedShortCharacter
    : 'US'
    ;

IntegerCharacter
    : 'I'
    ;

UnsignedIntegerCharacter
    : 'UI'
    ;

LongCharacter
    : 'L'
    ;

UnsignedLongCharacter
    : 'UL'
    ;

IntLiteral
    : Digit+
    ;

HexLiteral
    : '&' 'H' HexDigit+
    ;

OctalLiteral
    : '&' 'O' OctalDigit+
    ;

Digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

HexDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F'
    ;

OctalDigit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7'
    ;
```

<span data-ttu-id="194a6-188">文本类型确定通过其值或以下的类型字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-188">The type of a literal is determined by its value or by the following type character.</span></span> <span data-ttu-id="194a6-189">如果指定没有类型字符，则值的范围内`Integer`类型将被类型化为`Integer`; 对于范围之外的值`Integer`被类型化为`Long`。</span><span class="sxs-lookup"><span data-stu-id="194a6-189">If no type character is specified, values in the range of the `Integer` type are typed as `Integer`; values outside the range for `Integer` are typed as `Long`.</span></span> <span data-ttu-id="194a6-190">如果整数文本的类型的大小不足以保存整数文字，会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="194a6-190">If an integer literal's type is of insufficient size to hold the integer literal, a compile-time error results.</span></span> <span data-ttu-id="194a6-191">(__注意。__</span><span class="sxs-lookup"><span data-stu-id="194a6-191">(__Note.__</span></span> <span data-ttu-id="194a6-192">不存在的类型字符`Byte`因为最自然的字符将`B`，这是十六进制文本中的合法字符。)</span><span class="sxs-lookup"><span data-stu-id="194a6-192">There isn't a type character for `Byte` because the most natural character would be `B`, which is a legal character in a hexadecimal literal.)</span></span>


### <a name="floating-point-literals"></a><span data-ttu-id="194a6-193">浮点文本</span><span class="sxs-lookup"><span data-stu-id="194a6-193">Floating-Point Literals</span></span>

<span data-ttu-id="194a6-194">浮点文本是跟可选小数点 （ASCII 句点字符） 和尾数和一个可选的基 10 指数的整数文字。</span><span class="sxs-lookup"><span data-stu-id="194a6-194">A floating-point literal is an integer literal followed by an optional decimal point (the ASCII period character) and mantissa, and an optional base 10 exponent.</span></span> <span data-ttu-id="194a6-195">默认情况下是类型的浮点数`Double`。</span><span class="sxs-lookup"><span data-stu-id="194a6-195">By default, a floating-point literal is of type `Double`.</span></span> <span data-ttu-id="194a6-196">如果`Single`， `Double`，或`Decimal`指定类型字符时，该整数是该类型。</span><span class="sxs-lookup"><span data-stu-id="194a6-196">If the `Single`, `Double`, or `Decimal` type character is specified, the literal is of that type.</span></span> <span data-ttu-id="194a6-197">如果浮点数的类型的大小不足以保存浮点数，会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="194a6-197">If a floating-point literal's type is of insufficient size to hold the floating-point literal, a compile-time error results.</span></span>

<span data-ttu-id="194a6-198">__请注意。__</span><span class="sxs-lookup"><span data-stu-id="194a6-198">__Note.__</span></span> <span data-ttu-id="194a6-199">值得注意的是，`Decimal`数据类型可以编码中值的尾随零。</span><span class="sxs-lookup"><span data-stu-id="194a6-199">It is worth noting that the `Decimal` data type can encode trailing zeros in a value.</span></span> <span data-ttu-id="194a6-200">该规范目前使有关是否尾随零的处理方法没有注释`Decimal`编译器时应遵循文本。</span><span class="sxs-lookup"><span data-stu-id="194a6-200">The specification currently makes no comment about whether trailing zeros in a `Decimal` literal should be honored by a compiler.</span></span>

```antlr
FloatingPointLiteral
    : FloatingPointLiteralValue FloatingPointTypeCharacter?
    | IntLiteral FloatingPointTypeCharacter
    ;

FloatingPointTypeCharacter
    : SingleCharacter
    | DoubleCharacter
    | DecimalCharacter
    | SingleTypeCharacter
    | DoubleTypeCharacter
    | DecimalTypeCharacter
    ;

SingleCharacter
    : 'F'
    ;

DoubleCharacter
    : 'R'
    ;

DecimalCharacter
    : 'D'
    ;

FloatingPointLiteralValue
    : IntLiteral '.' IntLiteral Exponent?
    | '.' IntLiteral Exponent?
    | IntLiteral Exponent
    ;

Exponent
    : 'E' Sign? IntLiteral
    ;

Sign
    : '+'
    | '-'
    ;
```

### <a name="string-literals"></a><span data-ttu-id="194a6-201">字符串</span><span class="sxs-lookup"><span data-stu-id="194a6-201">String Literals</span></span>

<span data-ttu-id="194a6-202">字符串文字是一系列零个或多个 Unicode 字符开头和结尾的 ASCII 双引号字符，Unicode 左的双引号字符或 Unicode 右双引号字符。</span><span class="sxs-lookup"><span data-stu-id="194a6-202">A string literal is a sequence of zero or more Unicode characters beginning and ending with an ASCII double-quote character, a Unicode left double-quote character, or a Unicode right double-quote character.</span></span> <span data-ttu-id="194a6-203">在字符串中，两个双引号字符的序列是表示双引号的字符串中的转义序列。</span><span class="sxs-lookup"><span data-stu-id="194a6-203">Within a string, a sequence of two double-quote characters is an escape sequence representing a double quote in the string.</span></span>

```antlr
StringLiteral
    : DoubleQuoteCharacter StringCharacter* DoubleQuoteCharacter
    ;

DoubleQuoteCharacter
    : '"'
    | '<unicode left double-quote 0x201c>'
    | '<unicode right double-quote 0x201D>'
    ;

StringCharacter
    : '<Any character except DoubleQuoteCharacter>'
    | DoubleQuoteCharacter DoubleQuoteCharacter
    ;
```

<span data-ttu-id="194a6-204">一个字符串常量属于`String`类型。</span><span class="sxs-lookup"><span data-stu-id="194a6-204">A string constant is of the `String` type.</span></span>

```vb
Module Test
    Sub Main()

        ' This prints out: ".
        Console.WriteLine("""")

        ' This prints out: a"b.
        Console.WriteLine("a""b")

        ' This causes a compile error due to mismatched double-quotes.
        Console.WriteLine("a"b")
    End Sub
End Module
```

<span data-ttu-id="194a6-205">编译器允许将常量字符串表达式替换为字符串文字。</span><span class="sxs-lookup"><span data-stu-id="194a6-205">The compiler is allowed to replace a constant string expression with a string literal.</span></span> <span data-ttu-id="194a6-206">每个字符串文字不一定产生新的字符串实例。</span><span class="sxs-lookup"><span data-stu-id="194a6-206">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="194a6-207">当两个或多个使用二进制比较语义的字符串相等运算符根据等效的字符串文本出现在同一个程序时，这些字符串文本可能指相同的字符串实例。</span><span class="sxs-lookup"><span data-stu-id="194a6-207">When two or more string literals that are equivalent according to the string equality operator using binary comparison semantics appear in the same program, these string literals may refer to the same string instance.</span></span> <span data-ttu-id="194a6-208">例如，以下程序的输出可能会返回`True`因为两个字符串可能会引用相同的字符串实例。</span><span class="sxs-lookup"><span data-stu-id="194a6-208">For instance, the output of the following program may return `True` because the two literals may refer to the same string instance.</span></span>

```vb
Module Test
    Sub Main()
        Dim a As Object = "he" & "llo"
        Dim b As Object = "hello"
        Console.WriteLine(a Is b)
    End Sub
End Module
```


### <a name="character-literals"></a><span data-ttu-id="194a6-209">字符文本</span><span class="sxs-lookup"><span data-stu-id="194a6-209">Character Literals</span></span>

<span data-ttu-id="194a6-210">字符文本表示单个 Unicode 字符的`Char`类型。</span><span class="sxs-lookup"><span data-stu-id="194a6-210">A character literal represents a single Unicode character of the `Char` type.</span></span> <span data-ttu-id="194a6-211">两个双引号字符是表示双引号字符的转义序列。</span><span class="sxs-lookup"><span data-stu-id="194a6-211">Two double-quote characters is an escape sequence representing the double-quote character.</span></span>

```antlr
CharacterLiteral
    : DoubleQuoteCharacter StringCharacter DoubleQuoteCharacter 'C'
    ;
```


```vb
Module Test
    Sub Main()

        ' This prints out: a.
        Console.WriteLine("a"c)

        ' This prints out: ".
        Console.WriteLine(""""c)
    End Sub
End Module
```


### <a name="date-literals"></a><span data-ttu-id="194a6-212">Date 文本</span><span class="sxs-lookup"><span data-stu-id="194a6-212">Date Literals</span></span>

<span data-ttu-id="194a6-213">日期文字表示时间表示的值为某个特定时间点`Date`类型。</span><span class="sxs-lookup"><span data-stu-id="194a6-213">A date literal represents a particular moment in time expressed as a value of the `Date` type.</span></span>

```antlr
DateLiteral
    : '#' WhiteSpace* DateOrTime WhiteSpace* '#'
    ;

DateOrTime
    : DateValue WhiteSpace+ TimeValue
    | DateValue
    | TimeValue
    ;

DateValue
    : MonthValue '/' DayValue '/' YearValue
    | MonthValue '-' DayValue '-' YearValue
    ;

TimeValue
    : HourValue ':' MinuteValue ( ':' SecondValue )? WhiteSpace* AMPM?
    | HourValue WhiteSpace* AMPM
    ;

MonthValue
    : IntLiteral
    ;

DayValue
    : IntLiteral
    ;

YearValue
    : IntLiteral
    ;

HourValue
    : IntLiteral
    ;

MinuteValue
    : IntLiteral
    ;

SecondValue
    : IntLiteral
    ;

AMPM
    : 'AM' | 'PM'
    ;    
```

<span data-ttu-id="194a6-214">文本可以指定的日期和时间，只是日期或只需一次。</span><span class="sxs-lookup"><span data-stu-id="194a6-214">The literal may specify both a date and a time, just a date, or just a time.</span></span> <span data-ttu-id="194a6-215">如果省略了日期值，则假定 1 公历日历中每 1 年月 1 日。</span><span class="sxs-lookup"><span data-stu-id="194a6-215">If the date value is omitted, then January 1 of the year 1 in the Gregorian calendar is assumed.</span></span> <span data-ttu-id="194a6-216">如果省略了时间值，则假定 12:00:00 AM。</span><span class="sxs-lookup"><span data-stu-id="194a6-216">If the time value is omitted, then 12:00:00 AM is assumed.</span></span>

<span data-ttu-id="194a6-217">若要避免出现问题解释日期值的年份值，年份值不能为两位数字。</span><span class="sxs-lookup"><span data-stu-id="194a6-217">To avoid problems with interpreting the year value in a date value, the year value cannot be two digits.</span></span> <span data-ttu-id="194a6-218">时表示日期中第一个世纪 AD/CE，则必须指定前导零。</span><span class="sxs-lookup"><span data-stu-id="194a6-218">When expressing a date in the first century AD/CE, leading zeros must be specified.</span></span>

<span data-ttu-id="194a6-219">可以使用 24 小时值或一个 12 小时值; 如果指定的时间值时间值省略`AM`或`PM`被假定为 24 小时值。</span><span class="sxs-lookup"><span data-stu-id="194a6-219">A time value may be specified either using a 24-hour value or a 12-hour value; time values that omit an `AM` or `PM` are assumed to be 24-hour values.</span></span> <span data-ttu-id="194a6-220">如果时间值中省略了分钟，文字`0`默认情况下使用。</span><span class="sxs-lookup"><span data-stu-id="194a6-220">If a time value omits the minutes, the literal `0` is used by default.</span></span> <span data-ttu-id="194a6-221">如果时间值中省略秒，文字`0`默认情况下使用。</span><span class="sxs-lookup"><span data-stu-id="194a6-221">If a time value omits the seconds, the literal `0` is used by default.</span></span> <span data-ttu-id="194a6-222">如果分钟，第二个省略了，然后`AM`或`PM`必须指定。</span><span class="sxs-lookup"><span data-stu-id="194a6-222">If both minutes and second are omitted, then `AM` or `PM` must be specified.</span></span> <span data-ttu-id="194a6-223">如果指定的日期值位于外部的范围`Date`类型，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="194a6-223">If the date value specified is outside the range of the `Date` type, a compile-time error occurs.</span></span>

<span data-ttu-id="194a6-224">下面的示例包含多个日期文本。</span><span class="sxs-lookup"><span data-stu-id="194a6-224">The following example contains several date literals.</span></span>

```vb
Dim d As Date

d = # 8/23/1970 3:45:39AM #
d = # 8/23/1970 #              ' Date value: 8/23/1970 12:00:00AM.
d = # 3:45:39AM #              ' Date value: 1/1/1 3:45:39AM.
d = # 3:45:39 #                ' Date value: 1/1/1 3:45:39AM.
d = # 13:45:39 #               ' Date value: 1/1/1 1:45:39PM.
d = # 1AM #                    ' Date value: 1/1/1 1:00:00AM.
d = # 13:45:39PM #             ' This date value is not valid.
```


### <a name="nothing"></a><span data-ttu-id="194a6-225">Nothing</span><span class="sxs-lookup"><span data-stu-id="194a6-225">Nothing</span></span>

<span data-ttu-id="194a6-226">`Nothing` 是一个特殊的参数;它不具有一种类型，并在类型系统中，包括类型参数是转换为所有类型。</span><span class="sxs-lookup"><span data-stu-id="194a6-226">`Nothing` is a special literal; it does not have a type and is convertible to all types in the type system, including type parameters.</span></span> <span data-ttu-id="194a6-227">转换为特定类型时，它是该类型的默认值的等效项。</span><span class="sxs-lookup"><span data-stu-id="194a6-227">When converted to a particular type, it is the equivalent of the default value of that type.</span></span>

```antlr
Nothing
    : 'Nothing'
    ;
```

## <a name="separators"></a><span data-ttu-id="194a6-228">分隔符</span><span class="sxs-lookup"><span data-stu-id="194a6-228">Separators</span></span>

<span data-ttu-id="194a6-229">以下的 ASCII 字符是分隔符：</span><span class="sxs-lookup"><span data-stu-id="194a6-229">The following ASCII characters are separators:</span></span>

```antlr
Separator
    : '(' | ')' | '{' | '}' | '!' | '#' | ',' | '.' | ':' | '?'
    ;
```

## <a name="operator-characters"></a><span data-ttu-id="194a6-230">运算符字符</span><span class="sxs-lookup"><span data-stu-id="194a6-230">Operator Characters</span></span>

<span data-ttu-id="194a6-231">以下的 ASCII 字符或字符序列表示运算符：</span><span class="sxs-lookup"><span data-stu-id="194a6-231">The following ASCII characters or character sequences denote operators:</span></span>

```antlr
Operator
    : '&' | '*' | '+' | '-' | '/' | '\\' | '^' | '<' | '=' | '>'
    ;
```

