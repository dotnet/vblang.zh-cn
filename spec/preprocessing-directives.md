---
ms.openlocfilehash: 7ad305f4b85bce12f174511af5e5f0aa62a7cc0e
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426633"
---
# <a name="preprocessing-directives"></a>预处理指令

后一个文件进行了词法分析，会发生几种类型的源预处理。 在最重要的是，条件编译中，确定哪些源处理的语法的语法;两个其他类型的指令-外部源指令和区域指令--提供有关源的元信息但是编译中不起作用。

## <a name="conditional-compilation"></a>条件编译

条件编译控制逻辑行序列将转换为实际代码。 在条件编译开始时，启用所有逻辑行;但是，在条件编译语句封闭行可能会有选择地禁用在文件中，从而导致在编译过程的其余部分的过程中忽略这些行。

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


例如，该程序

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

生成的程序所在的令牌的确切相同序列

```vb
Class C
    Sub F()
    End Sub

    Sub I()
    End Sub
End Class
```

允许在条件编译指令中使用的常量表达式是常规的常量表达式的子集。

预处理器允许空格和显式行继续符之前和之后的每个标记。


### <a name="conditional-constant-directives"></a>条件常量指令

常量的条件语句定义作用域为源文件单独条件编译声明空间中存在的常量。

```antlr
CCConstantDeclaration
    : '#' 'Const' Identifier '=' CCExpression LineTerminator
    ;
```

声明空间很特殊，因为没有显式声明的条件编译常量不需要--条件常数可以隐式定义条件编译指令中。

正在分配一个值之前, 条件编译常量具有值`Nothing`。 条件编译常量分配一个值，该值必须是常量表达式，该常量的类型将成为要传递给它的值的类型。 条件编译常量可能会重新定义多个时间整个源代码文件。

例如，下面的代码将打印只有字符串`about to print value`的值和`Test`。

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

编译环境也可以在条件编译声明空间中定义条件的常量。


### <a name="conditional-compilation-directives"></a>条件编译指令

条件编译指令控制条件编译。

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

常量表达式和条件编译常量只能引用条件常量。 每个单个条件编译组内的常量表达式是计算并转换为`Boolean`按文本顺序从第一个到最后一个直到其中一个条件表达式的类型的计算结果为`True`。 如果表达式不能转换为`Boolean`，编译时错误结果。 计算条件编译常量表达式，而不考虑任何时始终使用宽松的语义和二进制字符串比较`Option`指令或编译环境设置。

括在组中，包括嵌套的条件编译指令的所有行处于禁用都状态之间包含的语句的行除外`True`表达式和组或介于之间的行的下一步的条件语句`Else`语句和`End If`语句如果`Else`出现在组中的所有表达式的计算结果为和`False`。

在此示例中，在调用`WriteToLog`中`Trace`条件编译指令则不会处理因为周围的`Debug`条件编译指令的计算结果为`False`。

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


## <a name="external-source-directives"></a>外部源指令

源代码文件可以包含外部源指令，以指示源行和源的外部文本之间的映射。

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

外部源指令不会影响编译，而且不能嵌套。 例如：

```vb
Module Test
    Sub Main()

#ExternalSource("c:\wwwroot\inetpub\test.aspx", 30)
        Console.WriteLine("In test.aspx")
#End ExternalSource

    End Sub
End Module
```


## <a name="region-directives"></a>区域指令

区域指令的源代码行进行分组，但对编译没有其他影响。 可以折叠和隐藏的或展开和在集成的开发环境 (IDE) 中查看整个组。

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

可以嵌套区域。 区域指令是程序的特殊，因为它们不能启动或终止的方法主体，并且它们必须遵循块结构。 例如：

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


## <a name="external-checksum-directives"></a>外部校验和指令

源代码文件可以包含一个外部校验和指令，指示哪些校验和不应发出一个外部源指令中引用的文件。 在所有其他方面外部源指令没有任何影响编译。

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

外部校验和指令包含外部文件的文件和文件的校验和与关联的全局唯一标识符 (GUID) 的文件名。 为窗体"{xxxxxxxx xxxx-xxxx-xxxx-在左右加上}"，其中 x 是十六进制数字的字符串常量指定 GUID。 校验和指定为窗体的"...xxxx"，其中 x 是十六进制数字的字符串常数。 校验和中的数字个数必须为偶数。

外部文件可能具有多个外部校验和指令，前提是所有的 GUID 和校验和值完全匹配与之关联。 如果正在编译的文件的名称匹配的外部文件的名称，被忽略校验和，以便编译器的校验和计算支持。

例如：

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

