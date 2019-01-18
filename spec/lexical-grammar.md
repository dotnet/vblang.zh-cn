---
ms.openlocfilehash: 2f14161222741beb7505f5674b230f1881828b5d
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426636"
---
# <a name="lexical-grammar"></a>词法语法

首先，编译 Visual Basic 程序需要将 Unicode 字符的原始流转换为一组有序词汇标记。 由于 Visual Basic 语言不是无格式，组标记然后进一步划分为一系列逻辑行。 一个*逻辑行*范围从流的起始行结束符通过到下一步前面没有由行继续符或通过到流的末尾的行终止符。

__请注意。__ XML 9.0 版中的语言的文本表达式的引入，Visual Basic 不再具有不同的词法语法意义上的 Visual Basic 代码可以标记化而不考虑的语义上下文。 这是由于访问的 XML 和 Visual Basic 了不同的词法规则，在任何特定时间使用的词汇规则集取决于当时正在处理哪些语法构造。 此规范保留此词法语法部分作为常规 Visual Basic 代码的词汇规则的指南。

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

## <a name="characters-and-lines"></a>字符和行

Visual Basic 程序中的 Unicode 字符的字符组成。

```antlr
Character:
    '<Any Unicode character except a LineTerminator>'
    ;
```

### <a name="line-terminators"></a>行终止符

Unicode 换行符分隔逻辑行。

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

### <a name="line-continuation"></a>行继续符

一个*行继续符*紧跟一个下划线字符作为文本行中的最后一个字符 （而不是空格） 的至少一个空白字符组成。 行继续符允许逻辑行以跨多个物理行的开头。 行继续符都被看作是空格，即使它们不是。

```antlr
LineContinuation
    : WhiteSpace '_' WhiteSpace* LineTerminator
    ;
```

以下程序显示了一些行继续符：

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

允许的语法的语法中的一些地方*隐式行继续符*。 当遇到行终止符

* 逗号 (`,`)，请打开括号 (`(`)、 左大括号 (`{`)，或打开嵌入式的表达式 (`<%=`)

* 后成员限定符 (`.`或`.@`或`...`)，前提是正在限定的内容 (即未使用隐式`With`上下文)

* 在右括号之前 (`)`)，右大括号 (`}`)，或嵌入式表达式结束标记 (`%>`)

* 在小于-比 (`<`) 在属性的上下文中

* 前一个大于-比 (`>`) 在属性的上下文中

* 后一个大于-比 (`>`) 在非文件级别的属性的上下文中

* 之前和之后查询运算符 (`Where`， `Order`， `Select`，等等。)

* 在二元运算符 (`+`， `-`， `/`，`*`等) 在表达式的上下文中

* 在赋值运算符 (`=`， `:=`， `+=`，`-=`等) 在任何上下文中。

就像它是行继续符处理的行终止符。

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

例如，还可以作为编写前面的示例：

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

直接之前或之后指定的标记，则只会将推断隐式行继续符。 它们将不会推断之前或之后行继续符。 例如：

```vb
Dim y = 10
' Error: Expression expected for assignment to x
Dim x = _

y
```

将条件编译的上下文中推断出行继续符。 (__注意。__ 此最后一个限制是必需的因为未编译的条件编译块中的文本不需要在语法上有效。 因此，块中的文本可能会意外地获取"选取"条件编译语句中，尤其是当语言获取将来扩展。）


### <a name="white-space"></a>空白区域

*空白*仅用于分隔标记并将忽略此设置。 逻辑行包含仅为空白将被忽略。 (__注意。__
行终止符是不被视为空白区域。）

```antlr
WhiteSpace
    : '<Unicode class Zs>'
    | '<Unicode Tab 0x0009>'
    ;
```

### <a name="comments"></a>注释

一个*注释*单引号字符或关键字开头`REM`。 单引号字符是 ASCII 单引号字符、 Unicode 左的单引号字符或 Unicode 右单引号字符。 注释可以在源行、 上任意位置开始，物理行尾结束注释。 编译器将忽略注释的开头之间的行终止符的字符。 因此，注释不能跨多个行扩展通过使用行继续符。

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

## <a name="identifiers"></a>标识符

*标识符*是一个名称。 Visual Basic 标识符符合 Unicode 标准附录 15 有一个例外： 标识符可能以下划线 （连接器） 字符开头。 如果标识符以下划线开头，则必须包含至少一个其他有效的标识符字符，以区别于行继续符。

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

常规标识符可能不与关键字冲突，但可以转义的标识符或标识符类型字符。 *转义标识符*是由方括号分隔的标识符。 转义标识符遵循与常规标识符相同的规则，只不过它们可能与关键字冲突，并且可能不具有类型字符。

此示例定义一个名为`class`与名为共享方法`shared`采用名为的参数`boolean`，然后调用该方法。

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

标识符是不区分大小写，因此两个标识符被视为是相同的标识符，如果它们的区别仅在于大小写。 (__注意。__ 在比较标识符时所使用的 Unicode 标准一对一大小写映射以及任何特定于区域设置的大小写映射将被忽略。）


### <a name="type-characters"></a>类型字符

一个*类型字符*表示上述标识符的类型。 类型字符不是被视为标识符的一部分。

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

如果声明包含类型字符，必须与声明本身; 中指定的类型一致的类型字符否则将发生编译时错误。 如果声明中省略类型 (例如，如果未指定`As`子句)，作为声明的类型隐式替换类型字符。

标识符和它的类型字符之间，可能会没有空格。 对于没有类型字符`Byte`， `SByte`， `UShort`， `Short`，`UInteger`或`ULong`，由于缺乏合适的字符。

追加到从概念上讲不具有类型 （例如，命名空间名称） 的标识符或其类型否定字符类型的类型的标识符类型字符会导致编译时错误。

下面的示例演示使用类型字符：

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

类型字符`!`面临一个特殊问题，因为它可以使用类型字符作为和语言中的分隔符。 若要消除歧义，`!`字符，只要它后面的字符不能启动标识符是类型字符。 如果可以则`!`字符是一个分隔符，而不是类型字符。


## <a name="keywords"></a>关键字

一个*关键字*是一种语言构造中具有特殊含义的词。 所有关键字保留的语言，除非标识符进行转义，否则可能不作为标识符使用。 (__注意。__ `EndIf``GoSub`， `Let`， `Variant`，和`Wend`保留 as 关键字，尽管 Visual Basic 中不再使用。)

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

## <a name="literals"></a>文本

一个*文字*是一种类型的特定值的文本表示形式。 文本类型包括 boolean 类型的值、 整数、 浮点数、 字符串、 字符和日期。

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

### <a name="boolean-literals"></a>布尔值文字

`True` 并`False`是文本的`Boolean`分别映射到 true 和 false 状态的类型。

```antlr
BooleanLiteral
    : 'True' | 'False'
    ;
```

### <a name="integer-literals"></a>整数文本

整数文本可以是十进制 (基数为 10)、 十六进制 (基数为 16) 或八进制 (基数为 8)。 十进制整数文本是十进制数字 (0-9) 的字符串。 十六进制文本是`&H`跟十六进制数字 (0-9、 A-F) 的字符串。 八进制文本是`&O`跟八进制数字 (0-7) 的字符串。 十进制文本直接表示整型文本，十进制值，而则八进制和十六进制文本表示整数的二进制值 (因此，`&H8000S`是-32768，不是溢出错误)。

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

文本类型确定通过其值或以下的类型字符。 如果指定没有类型字符，则值的范围内`Integer`类型将被类型化为`Integer`; 对于范围之外的值`Integer`被类型化为`Long`。 如果整数文本的类型的大小不足以保存整数文字，会导致编译时错误。 (__注意。__ 不存在的类型字符`Byte`因为最自然的字符将`B`，这是十六进制文本中的合法字符。)


### <a name="floating-point-literals"></a>浮点文本

浮点文本是跟可选小数点 （ASCII 句点字符） 和尾数和一个可选的基 10 指数的整数文字。 默认情况下是类型的浮点数`Double`。 如果`Single`， `Double`，或`Decimal`指定类型字符时，该整数是该类型。 如果浮点数的类型的大小不足以保存浮点数，会导致编译时错误。

__请注意。__ 值得注意的是，`Decimal`数据类型可以编码中值的尾随零。 该规范目前使有关是否尾随零的处理方法没有注释`Decimal`编译器时应遵循文本。

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

### <a name="string-literals"></a>字符串

字符串文字是一系列零个或多个 Unicode 字符开头和结尾的 ASCII 双引号字符，Unicode 左的双引号字符或 Unicode 右双引号字符。 在字符串中，两个双引号字符的序列是表示双引号的字符串中的转义序列。

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

一个字符串常量属于`String`类型。

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

编译器允许将常量字符串表达式替换为字符串文字。 每个字符串文字不一定产生新的字符串实例。 当两个或多个使用二进制比较语义的字符串相等运算符根据等效的字符串文本出现在同一个程序时，这些字符串文本可能指相同的字符串实例。 例如，以下程序的输出可能会返回`True`因为两个字符串可能会引用相同的字符串实例。

```vb
Module Test
    Sub Main()
        Dim a As Object = "he" & "llo"
        Dim b As Object = "hello"
        Console.WriteLine(a Is b)
    End Sub
End Module
```


### <a name="character-literals"></a>字符文本

字符文本表示单个 Unicode 字符的`Char`类型。 两个双引号字符是表示双引号字符的转义序列。

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


### <a name="date-literals"></a>Date 文本

日期文字表示时间表示的值为某个特定时间点`Date`类型。

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

文本可以指定的日期和时间，只是日期或只需一次。 如果省略了日期值，则假定 1 公历日历中每 1 年月 1 日。 如果省略了时间值，则假定 12:00:00 AM。

若要避免出现问题解释日期值的年份值，年份值不能为两位数字。 时表示日期中第一个世纪 AD/CE，则必须指定前导零。

可以使用 24 小时值或一个 12 小时值; 如果指定的时间值时间值省略`AM`或`PM`被假定为 24 小时值。 如果时间值中省略了分钟，文字`0`默认情况下使用。 如果时间值中省略秒，文字`0`默认情况下使用。 如果分钟，第二个省略了，然后`AM`或`PM`必须指定。 如果指定的日期值位于外部的范围`Date`类型，则发生编译时错误。

下面的示例包含多个日期文本。

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


### <a name="nothing"></a>Nothing

`Nothing` 是一个特殊的参数;它不具有一种类型，并在类型系统中，包括类型参数是转换为所有类型。 转换为特定类型时，它是该类型的默认值的等效项。

```antlr
Nothing
    : 'Nothing'
    ;
```

## <a name="separators"></a>分隔符

以下的 ASCII 字符是分隔符：

```antlr
Separator
    : '(' | ')' | '{' | '}' | '!' | '#' | ',' | '.' | ':' | '?'
    ;
```

## <a name="operator-characters"></a>运算符字符

以下的 ASCII 字符或字符序列表示运算符：

```antlr
Operator
    : '&' | '*' | '+' | '-' | '/' | '\\' | '^' | '<' | '=' | '>'
    ;
```

