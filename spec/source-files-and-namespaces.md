---
ms.openlocfilehash: 2e917b7aa27afd8a91e8c289e7454785d01bdda8
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426637"
---
# <a name="source-files-and-namespaces"></a>源文件和命名空间

Visual Basic 程序由一个或多个源文件组成。 编译计划后，所有源文件一起处理;因此，源文件可以互相依赖，可能是以循环方式，而无需前向声明。 文本的程序文本中的声明顺序通常是没有意义。

源代码文件由一组可选的选项语句、 导入语句、 和属性后, 跟的命名空间体。 属性，必须每个都`Assembly`或`Module`修饰符，将应用到.NET 程序集或模块生成的编译。 作为全局命名空间，这意味着所有源代码文件的顶层声明都位于全局命名空间中的隐式命名空间声明源文件函数的主体。 例如：

FileA.vb:

```vb
Class A
End Class
```

FileB.vb:

```vb
Class B
End Class
```

全局命名空间，在这种情况下声明两个类的完全限定名称的两个源文件参与`A`和`B`。 由于向同一声明空间分配的两个源文件，重要的是成员的一个错误如果每个包含具有相同名称的声明。

__请注意。__ 编译环境可能会覆盖隐式放入源代码文件的命名空间声明。

除非另有说明，Visual Basic 程序中的语句可以行结束符或分号终止。

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

## <a name="program-startup-and-termination"></a>程序启动和终止

程序启动执行环境执行指定的方法，该程序的方式被称为时会出现*入口点*。 此入口点方法，必须始终命名为`Main`、 必须共享、 不能包含在泛型类型，不能有 async 修饰符和必须具有以下签名之一：

```vb
Sub Main()
Sub Main(args() As String)
Function Main() As Integer
Function Main(args() As String) As Integer
```

入口点方法的可访问性无关。 如果程序包含多个合适的入口点，编译环境必须指定一个作为入口点。 否则，将发生编译时错误。 如果不存在，编译环境还可以创建的入口点方法。

当程序开始，如果入口点有一个参数时，执行环境所提供的自变量将包含表示为字符串程序的命令行参数。 如果入口点返回类型为`Integer`，则从函数返回的值作为该程序的结果返回到执行环境。

在所有其他方面，入口点方法的行为与其他方法相同的方式。 当执行离开所做的执行环境的入口点方法的调用时，该程序将终止。

## <a name="compilation-options"></a>编译选项

源文件可以在源中的代码使用指定的编译选项*选项语句*。

```antlr
OptionStatement
    : OptionExplicitStatement
    | OptionStrictStatement
    | OptionCompareStatement
    | OptionInferStatement
    ;
```

`Option`语句只应用于源文件中的出现，并且只有一个每种类型的`Option`语句可能会出现在源文件中。 例如：

```vb
Option Strict On
Option Compare Text
Option Strict Off    ' Not allowed, Option Strict is already specified.
Option Compare Text  ' Not allowed, Option Compare is already specified.
```

有四个编译选项： 严格类型语义、 显式声明的语义、 比较语义和本地变量的类型推理语义。 如果源文件不包括特定`Option`语句，然后编译环境确定了将使用哪些组特定的语义。 此外，还有第五个编译选项，仅可以通过编译环境指定的整数溢出检查。


### <a name="option-explicit-statement"></a>Option Explicit 语句

`Option Explicit`语句用于确定是否可能会隐式声明的局部变量。 关键字`On`或`Off`可以跟在该语句; 如果未指定，默认值是`On`。 如果在文件中未不指定任何语句，编译环境确定其将被使用。

```antlr
OptionExplicitStatement
    : 'Option' 'Explicit' OnOff? StatementTerminator
    ;

OnOff
    : 'On' | 'Off'
    ;
```


__请注意。__ `Explicit` 和`Off`不是保留的字。

```vb
Option Explicit Off

Module Test
    Sub Main()
        x = 5 ' Valid because Option Explicit is off.
    End Sub
End Module
```

在此示例中，本地变量`x`隐式声明通过将分配给它。 类型 `x` 不是 `Object`。


### <a name="option-strict-statement"></a>Option Strict Statement

`Option Strict`语句用于确定是否转换和操作上的`Object`受严格或许可类型语义和是否类型隐式类型化为`Object`如果没有`As`指定子句。 该语句可能跟关键字`On`或`Off`; 如果未指定，默认值是`On`。 如果在文件中未不指定任何语句，编译环境确定其将被使用。

```antlr
OptionStrictStatement
    : 'Option' 'Strict' OnOff? StatementTerminator
    ;
```

__请注意。__ `Strict` 和`Off`不是保留的字。

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

在严格的语义，以下是不允许：

* 而无需显式强制转换运算符的收缩转换。

* 后期绑定。

* 对类型执行操作`Object`而不`TypeOf`...`Is`， `Is`，和`IsNot`。

* 省略`As`子句中不具有推断的类型的声明。


### <a name="option-compare-statement"></a>Option Compare 语句

`Option Compare`语句用于确定字符串比较的语义。 任何一个执行字符串比较时使用二进制比较 （在其中进行比较的每个字符的二进制 Unicode 值） 或 （在其中每个字符的词汇意义进行比较使用当前区域性） 的文本比较。 如果在文件中未不指定任何语句，控制编译环境，将使用哪种类型的比较。

```antlr
OptionCompareStatement
    : 'Option' 'Compare' CompareOption StatementTerminator
    ;

CompareOption
    : 'Binary' | 'Text'
    ;
```

__请注意。__ `Compare``Binary`，和`Text`不是保留的字。

```vb
Option Compare Text

Module Test
    Sub Main()
        Console.WriteLine("a" = "A")    ' Prints True.
    End Sub
End Module
```

在这种情况下，进行字符串比较使用文本比较忽略大小写差异。 如果`Option Compare Binary`具有已指定，则这将打印`False`。


### <a name="integer-overflow-checks"></a>整数溢出检查

整数运算可以检查或在运行时未选中溢出条件。 如果溢出条件已选中并且整数操作溢出，`System.OverflowException`引发异常。 如果未选中溢出条件，整数操作溢出不会引发异常。 编译环境确定此选项为打开或关闭。

### <a name="option-infer-statement"></a>Option Infer 语句

`Option Infer`语句用于确定是否本地变量声明，其中不含`As`子句具有推断出的类型或使用`Object`。 该语句可能跟关键字`On`或`Off`; 如果未指定，默认值是`On`。 如果在文件中未不指定任何语句，编译环境确定其将被使用。

```antlr
OptionInferStatement
    : 'Option' 'Infer' OnOff? StatementTerminator
    ;
```

__请注意。__ `Infer` 和`Off`不是保留的字。

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


## <a name="imports-statement"></a>Imports 语句

`Imports` 语句导入源文件，允许的名称，而无需限定，引用的实体的名称，或导入 XML 表达式中使用的命名空间。

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

在成员声明中包含的源文件内`Imports`语句，给定的命名空间中包含的类型可以直接，引用，如下面的示例中所示：

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

此处，在源文件中的，命名空间的类型成员`N1.N2`可直接使用，并因此类`N3.B`派生自类`N1.N2.A`。

`Imports` 语句必须出现在任何之后`Option`语句但任何之前的类型声明。 编译环境还定义隐式`Imports`语句。

`Imports` 语句在源文件中，使名称可用，但未声明全局命名空间声明空间中的任何内容。 导入的名称的作用域`Imports`语句通过源代码文件中包含的命名空间成员声明进行了扩展。 作用域`Imports`语句具体说来，不包含其他`Imports`语句，也不包括其他源文件。 `Imports` 另一个可能不引用语句。

在此示例中，最后一个`Imports`语句是错误，因为它不受第一个导入别名。

```vb
Imports R1 = N1 ' OK.
Imports R2 = N1.N2 ' OK.
Imports R3 = R1.N2 ' Error: Can't refer to R1.

Namespace N1.N2
End Namespace
```

__请注意。__ 在出现的命名空间或类型名称`Imports`语句始终处理，就好像它们是完全限定。 也就是说，全局命名空间中始终会解析命名空间或类型名称中最左侧的标识符和的解决方法的其余部分将按照正常名称解析规则进行。 这是在应用此类规则; 的语言中的唯一位置规则可确保名称不能完全隐藏从限定。 如果没有规则，如果全局命名空间中的名称被隐藏在特定源文件中，它无法限定方式指定该命名空间中的任何名称。

在此示例中，`Imports`语句总是引用全局`System`命名空间，而不是源文件中的类。

```vb
Imports System   ' Imports the namespace, not the class.

Class System
End Class
```


### <a name="import-aliases"></a>导入别名

*导入别名*定义命名空间或类型的别名。

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

此处，在源代码文件中，`A`是其别名`N1.N2.A`，因此类`N3.B`派生自类`N1.N2.A`。 可以通过创建别名来获取相同的效果`R`有关`N1.N2`然后引用`R.A`:

```vb
Imports R = N1.N2

Namespace N3
    Class B
        Inherits R.A
    End Class
End Namespace
```

导入别名的标识符中必须是唯一的声明空间的全局命名空间 （而不仅仅是在其中定义导入别名的源文件中的全局命名空间声明），即使它未声明全局命名空间中的名称声明空间。

__请注意。__ 在模块中的声明不引入包含的声明空间的名称。 因此，它是一个模块，将具有相同的名称为导入别名中的声明有效即使在包含声明空间的声明名称将无法访问。

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

在这里，全局命名空间已有一个成员`A`，因此它是错误的导入别名，以使用该标识符。 它同样是错误的两个或多个要通过相同的名称来声明别名的同一源文件中的导入别名。

导入别名可以创建任何命名空间或类型的别名。 通过别名访问命名空间或类型会产生与通过其声明的名称访问该命名空间或类型相同的结果。

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

此处，名称`N1.N2.A`， `R1.N2.A`，并`R2.A`等效项，以及所有引用的类的完全限定的名称是`N1.N2.A`。

导入指定命名空间或向其创建别名的类型的确切的名称。 这必须是该命名空间或类型的确切的完全限定的名称： 不使用限定的名称解析 （该实例允许通过派生类的基类的成员访问） 的一般规则。

如果导入别名指向的类型或命名空间不能解析的这些规则，然后导入语句将被忽略 （和编译器则报告警告）。

此外，该引用不能为开放式泛型类型--所有泛型类型必须具有有效的类型参数提供，和所有类型实参必须都是可由上述规则解析。 泛型类型的任何不正确的绑定时出错。

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

源文件中的声明可能会隐藏导入别名名称。

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

在前面的示例引用到`R.A`中的声明`B`导致错误，因为`R`指`N3.R`，而不`N1.N2`。

导入别名使别名可在特定的源代码文件中，但它不会影响到基础的声明空间的任何新成员。 换而言之，导入别名不是可传递的但而是会影响仅项所在的源文件。

File1.vb:

```vb
Imports R = N1.N2

Namespace N1.N2
    Class A
    End Class
End Namespace
```

File2.vb:

```vb
Class B
    Inherits R.A ' Error, R unknown.
End Class
```

在上述示例中，因为导入别名的作用域引入`R`仅扩展到在其中包含它，源文件中声明`R`第二个源文件中为未知。


### <a name="namespace-imports"></a>命名空间导入

一个*命名空间导入*导入的所有成员的命名空间或类型，允许的命名空间或类型，而无需限定使用的每个成员的标识符。 对于类型，命名空间导入只允许对类型的共享成员的访问而无需限定类名。 具体而言，它允许枚举的类型，而无需限定使用的成员。

```antlr
MembersImportsClause
    : TypeName
    ;
```

例如：

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

与导入别名，不同的命名空间导入它导入并可以导入命名空间和类型标识符已声明全局命名空间中的名称没有任何限制。 通过导入别名和源代码文件中的声明隐藏导入的正则导入的名称。

在以下示例中，`A`是指`N3.A`而非`N1.N2.A`中的成员声明中`N3`命名空间。

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

当多个导入的命名空间包含同名的成员 （和该名称不是隐藏由导入别名或声明） 时，对该名称的引用不明确，会导致编译时错误。

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

在上述示例中，同时`N1`并`N2`都包含一个成员`A`。 因为`N3`导入两者，引用`A`中`N3`会导致编译时错误。 在此情况下，冲突可以解决通过对引用的限定`A`，或通过引入选取特定的导入别名`A`，如下面的示例：

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

唯一命名空间，类、 结构、 枚举类型，并可能会导入标准模块。


### <a name="xml-namespace-imports"></a>XML Namespace 导入

*XML 命名空间导入*定义命名空间或编译单元中包含的非限定 XML 表达式的默认命名空间。

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

例如：

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

XML 命名空间，包括默认命名空间，仅对于一组特定的导入定义一次。 例如：

```vb
Imports <xmlns:db="http://example.org/database-one">
' Error: namespace db is already defined
Imports <xmlns:db="http://example.org/database-two">
```


## <a name="namespaces"></a>命名空间

Visual Basic 程序的组织结构使用的命名空间。 命名空间都在内部组织程序，以及组织程序元素公开给其他程序的方式。

与其他实体不同命名空间是无限的并可能会声明为多个时间在同一程序中和跨许多程序，每个声明共同的同一命名空间的成员。 在以下示例中，两个命名空间声明参与到同一声明空间，声明两个类的完全限定名称`N1.N2.A`和`N1.N2.B`。

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

由于向同一声明空间分配的两个声明，则会出错，如果每个包含一个具有相同名称的成员的声明。

没有具有任何名称和其嵌套的命名空间和类型始终可以访问而无需限定的全局命名空间。 声明全局命名空间中的命名空间成员的作用域是整个程序文本。 否则，类型或命名空间的作用域声明其完全限定名的命名空间中`N`是其对应的命名空间的完全限定的名称以开始，每个命名空间的程序文本`N`或为`N`本身。 （请注意，编译器可以选择将声明放在特定的命名空间，默认情况下。 这不会更改这一事实，还有一个全局的未命名命名空间。）

在此示例中，该类`B`可以请参阅类`A`因为`B`的命名空间`N1.N2.N3`从概念上讲嵌套在命名空间内`N1.N2`。

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

### <a name="namespace-declarations"></a>命名空间声明

有三种形式的命名空间声明。

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

第一个窗体将开始使用关键字`Namespace`跟相对的命名空间名称。 如果相对的命名空间名称限定，命名空间声明被视为如同它从词法上嵌套在限定的名称中的每个名称对应的命名空间声明。 例如，以下两个命名空间在语义上等效:

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

第二个窗体将开始通过关键字`Namespace Global`。 它被视为其所有成员声明了词法上都放置在全局命名空间-而不考虑编译环境提供任何默认值。 这种形式的命名空间声明不能从词法上任何其他命名空间声明内嵌套。

第三个窗体将开始通过关键字`Namespace Global`限定标识符后跟`N`。 它被视为如同它是第一种形式的命名空间声明"`Namespace N`"，从词法上被放在全局命名空间-而不考虑编译环境提供任何默认值。 这种形式的命名空间声明不能从词法上任何其他命名空间声明内嵌套。

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

在处理时命名空间的成员，特定成员的声明位置并不重要。 如果两个程序在同一个命名空间中定义具有相同名称的实体，尝试解析命名空间中的名称会导致多义性错误。

命名空间是根据定义`Public`，因此，命名空间声明不能包含任何访问修饰符。


### <a name="namespace-members"></a>Namespace 成员

Namespace 成员只能是命名空间声明和类型声明。 类型声明可能具有`Public`或`Friend`访问。 类型的默认访问是`Friend`访问。

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
