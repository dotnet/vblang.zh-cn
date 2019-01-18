---
ms.openlocfilehash: 8a36506d9fce1605cf3758536f51782ea7680e84
ms.sourcegitcommit: 6eca149bdc736113e0adb709212bd266c9503c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47426613"
---
# <a name="documentation-comments"></a>文档注释

文档注释是特殊格式进行分析以生成有关其附加到的代码文档的源中的注释。 文档注释的基本格式为 XML。 当包含文档注释的编译代码，编译器可能会选择性地将发出一个 XML 文件，代表源中的文档注释的总和。 此 XML 文件然后可由其他工具来生成印刷品或联机文档。

本章介绍了文档注释，建议用于文档注释的 XML 标记。

## <a name="documentation-comment-format"></a>文档注释格式

文档注释是开头的特殊注释`'''`，三个单引号标记。 它们必须紧跟在记录的类型 （如类、 委托或接口） 或类型成员 （例如字段、 事件、 属性或方法）。 如果有的话，在提供其主体中，该方法的文档注释将被替换分部方法声明上的文档注释。 一起追加所有相邻的文档注释，以生成单个文档注释。 如果没有空格字符以下`'''`字符，则该空白字符不包括在串联。 例如：

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
   ''' <remarks>
   ''' Method <c>Draw</c> renders the point.
   ''' </remarks>
   Sub Draw()
   End Sub
End Class
```

文档注释必须格式正确的 XML 符合以下 http://www.w3.org/TR/REC-xml。 如果 XML 的格式不正确，则会生成警告，并且文档文件将包含条注释，指出遇到错误。

尽管开发人员可以随意创建其自己的标记集下, 一节中定义一组推荐。 部分建议标记具有特殊含义：

* `<param>`标记用于描述参数。 指定的参数`<param>`标记必须存在和类型成员的所有参数必须文档注释中所都述。 如果任何一个条件未得到满足，编译器会发出警告。

* `cref` 属性可以附加到任何标记，以提供对代码元素的引用。 代码元素必须存在;在编译时编译器将名称替换为表示该成员的 ID 字符串。 如果代码元素不存在，编译器会发出警告。 当查找名称中所述`cref`特性，编译器仍会遵循`Imports`出现在包含源文件的语句。

* `<summary>`标记应使用文档查看器以显示有关类型或成员的其他信息。

请注意，文档文件不提供文档注释中包含的内容仅相关类型和成员的完整信息。 若要获取有关类型或成员的详细信息，必须与实际类型或成员上反射结合使用的文档文件。

## <a name="recommended-tags"></a>建议的标记

文档生成器必须接受并处理根据 XML 的规则是有效的任何标记。 下列标记提供用户文档中的常用功能：

`<c>` 设置类似于代码的字体中的文本

`<code>` 类似于代码的字体中设置源的代码或程序输出的一个或多个的行

`<example>` 指示一个示例

`<exception>` 标识一个方法可能会引发的异常

`<include>` 包括外部 XML 文档

`<list>` 创建列表或表

`<para>` 允许结构添加到文本

`<param>` 描述的方法或构造函数的参数

`<paramref>` 标识一个字为参数名称

`<permission>` 记录成员的安全可访问性

`<remarks>` 介绍一种类型

`<returns>` 描述一种方法的返回值

`<see>` 指定的链接

`<seealso>` 生成的另请参阅条目

`<summary>` 描述一种类型的成员

`<typeparam>` 描述类型参数

`<value>` 描述的属性

### <a name="ltcgt"></a>&lt;c&gt;

此标记指定的片段说明中的文本应使用类似的代码块使用的字体。 (对于实际代码行，请使用`<code>`。)

__语法：__

```xml
<c>text to be set like code</c>
```

__示例：__

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltcodegt"></a>&lt;代码&gt;

此标记指定一个或多个源的代码或程序输出行应使用固定宽度字体。 (对于小型代码片段使用`<c>`。)

__语法：__

```xml
<code>source code or program output</code>
```

__示例：__

```vb
''' <summary>
''' This method changes the point's location by the given x- and 
''' y-offsets.
''' <example>
''' For example:
''' <code>
'''    Dim p As Point = New Point(3,5)
'''    p.Translate(-1,3)
''' </code>
''' results in <c>p</c>'s having the value (2,8).
''' </example>
''' </summary>
Public Sub Translate(x As Integer, y As Integer)
    Me.x += x
    Me.y += y
End Sub
```

### <a name="ltexamplegt"></a>&lt;example&gt;

此标记允许的代码示例显示如何使用元素的注释内。 通常，这将涉及到使用标记的`<code>`也。

__语法：__

```xml
<example>description</example>
```

__示例：__

有关示例，请参见 `<code>`。

### <a name="ltexceptiongt"></a>&lt;exception&gt;

此标记提供了一种方法来记录的方法可能引发的异常。

__语法：__

```xml
<exception cref="member">description</exception>
```

__示例：__

```vb
Public Module DataBaseOperations
    ''' <exception cref="MasterFileFormatCorruptException"></exception>
    ''' <exception cref="MasterFileLockedOpenException"></exception>
    Public Sub ReadRecord(flag As Integer)
        If Flag = 1 Then
            Throw New MasterFileFormatCorruptException()
        ElseIf Flag = 2 Then
            Throw New MasterFileLockedOpenException()
        End If
        ' ...
    End Sub
End Module
```

### <a name="ltincludegt"></a>&lt;include&gt;

此标记用于包括来自外部格式正确的 XML 文档的信息。 XPath 表达式应用于要指定什么 XML 应包含从文档的 XML 文档。 `<include>`标记然后替换从外部文档中选定的 XML。

__语法：__

```xml
<include file="filename" path="xpath">
```

__示例：__

如果源代码包含如下所示的声明：

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

外部文件 docs.xml 感到以下内容，并且

```xml
<?xml version="1.0"?>
<extra>
    <class name="IntList">
        <summary>
            Contains a list of integers.
        </summary>
    </class>
    <class name="StringList">
        <summary>
            Contains a list of strings.
        </summary>
    </class>
</extra>
```

然后同一文档是输出，如同的源代码包含：

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a>&lt;list&gt;

此标记用于创建列表或项的表。 它可能包含`<listheader>`块来定义表或定义列表的标题行。 （在定义表时，仅在标题中的术语的项需要提供。）

使用指定列表中的每个项`<item>`块。 创建定义列表时，必须指定术语和说明。 但是，对于表、 项目符号列表或编号的列表，需指定仅说明。

__语法：__

```xml
<list type="bullet" | "number" | "table">
    <listheader>
        <term>term</term>
        <description>description</description>
    </listheader>
    <item>
        <term>term</term>
        <description>description</description>
    </item>
    ...
    <item>
        <term>term</term>
        <description>description</description>
    </item>
</list>
```

__示例：__

```vb
Public Class TestClass
    ''' <remarks>
    ''' Here is an example of a bulleted list:
    ''' <list type="bullet">
    '''     <item>
    '''        <description>Item 1.</description>
    '''     </item>
    '''     <item>
    '''         <description>Item 2.</description>
    '''     </item>
    ''' </list>
    ''' </remarks>
    Public Shared Sub Main()
    End Sub
End Class
```

### <a name="ltparagt"></a>&lt;para&gt;

此标记是用于其他标记内，例如`<remarks>`或`<returns>`，并允许结构添加到文本。

__语法：__

```xml
<para>content</para>
```

__示例：__

```vb
''' <summary>
''' This is the entry point of the Point class testing program.
''' <para>This program tests each method and operator, and
''' is intended to be run after any non-trvial maintenance has
''' been performed on the Point class.</para>
''' </summary>
Public Shared Sub Main()
End Sub
```

### <a name="ltparamgt"></a>&lt;param&gt;

此标记描述方法、 构造函数或索引的属性的参数。

__语法：__

```xml
<param name="name">description</param>
```

__示例：__

```vb
''' <summary>
''' This method changes the point's location to the given
''' coordinates.
''' </summary>
''' <param name="x"><c>x</c> is the new x-coordinate.</param>
''' <param name="y"><c>y</c> is the new y-coordinate.</param>
Public Sub Move(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub
```

### <a name="ltparamrefgt"></a>&lt;paramref&gt;

此标记指示单词为参数。 可以处理文档文件以设置此参数的格式以不同的方式。

__语法：__

```xml
<paramref name="name"/>
```

__示例：__

```vb
''' <summary>
''' This constructor initializes the new Point to
''' (<paramref name="x"/>,<paramref name="y"/>).
''' </summary>
''' <param name="x"><c>x</c> is the new Point's x-coordinate.</param>
''' <param name="y"><c>y</c> is the new Point's y-coordinate.</param>
Public Sub New(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub
```

### <a name="ltpermissiongt"></a>&lt;permission&gt;

此标记记录成员的安全可访问性

__语法：__

```xml
<permission cref="member">description</permission>
```

__示例：__

```vb
''' <permission cref="System.Security.PermissionSet">Everyone can
''' access this method.</permission>
Public Shared Sub Test()
End Sub
```

### <a name="ltremarksgt"></a>&lt;remarks&gt;

此标记指定一种类型的概述信息。 (使用`<summary>`来描述类型的成员。)

__语法：__

```xml
<remarks>description</remarks>
```

__示例：__

```vb
''' <remarks>
''' Class <c>Point</c> models a point in a two-dimensional plane.
''' </remarks>
Public Class Point 
End Class
```

### <a name="ltreturnsgt"></a>&lt;returns&gt;

此标记描述方法的返回值。

__语法：__

```xml
<returns>description</returns>
```

__示例：__

```vb
''' <summary>
''' Report a point's location as a string.
''' </summary>
''' <returns>
''' A string representing a point's location, in the form (x,y), without
''' any leading, training, or embedded whitespace.
''' </returns>
Public Overrides Function ToString() As String
    Return "(" & x & "," & y & ")"
End Sub
```

### <a name="ltseegt"></a>&lt;see&gt;

此标记允许用于在文本内指定的链接。 (使用`<seealso>`以指示要在另请参见部分中显示的文本。)

__语法：__

```xml
<see cref="member"/>
```

__示例：__

```vb
''' <summary>
''' This method changes the point's location to the given
''' coordinates.
''' </summary>
''' <see cref="Translate"/>
Public Sub Move(x As Integer, y As Integer)
    Me.x = x
    Me.y = y
End Sub

''' <summary>
''' This method changes the point's location by the given x- and
''' y-offsets.
''' </summary>
''' <see cref="Move"/>
Public Sub Translate(x As Integer, y As Integer)
    Me.x += x
    Me.y += y
End Sub
```

### <a name="ltseealsogt"></a>&lt;seealso&gt;

此标记生成的项的另请参见部分。 (使用`<see>`指定从文本中的链接。)

__语法：__

```xml
<seealso cref="member"/>
```

__示例：__

```vb
''' <summary>
''' This method determines whether two Points have the same location.
''' </summary>
''' <seealso cref="operator=="/>
''' <seealso cref="operator!="/>
Public Overrides Function Equals(o As Object) As Boolean
    ' ...
End Function
```

### <a name="ltsummarygt"></a>&lt;summary&gt;

此标记描述一个类型成员。 (使用`<remarks>`描述类型本身。)

__语法：__

```xml
<summary>description</summary>
```

__示例：__

```vb
''' <summary>
''' This constructor initializes the new Point to (0,0).
''' </summary>
Public Sub New()
    Me.New(0,0)
End Sub
```

### <a name="lttypeparamgt"></a>&lt;typeparam&gt;

此标记描述类型参数。

__语法：__

```xml
<typeparam name="name">description</typeparam>
```

__示例：__

```vb
''' <typeparam name="T">
''' The base item type. Must implement IComparable.
''' </typeparam>
Public Class ItemManager(Of T As IComparable)
End Class
```

### <a name="ltvaluegt"></a>&lt;值&gt;

此标记描述的属性。

__语法：__

```xml
<value>property description</value>
```

__示例：__

```vb
''' <value>
''' Property <c>X</c> represents the point's x-coordinate.
''' </value>
Public Property X() As Integer
    Get
        Return _x
    End Get
    Set (Value As Integer)
        _x = Value
    End Set
End Property
```

## <a name="id-strings"></a>ID 字符串

在生成文档文件时，编译器将生成的标记为唯一标识它的文档注释的源代码中每个元素的 ID 字符串。 此 ID 字符串可以由外部工具，用于标识中编译的程序集的哪个元素对应的文档注释。

按如下所示生成 ID 字符串：

字符串不得包含空格。

字符串的第一部分标识成员记录，通过单个跟一个冒号字符的类型。 定义了以下类型的成员，则在它后面的括号中的相应字符： 事件 (E) 字段 (F)，包括构造函数和运算符 (M)、 命名空间 (N)、 属性 (P) 和 (T) 类型的方法。 感叹号 （！） 指示字符串的其余部分提供了有关错误的信息和生成 ID 字符串时出错。

字符串的第二部分是开始全局命名空间的元素的完全限定的名称。 用句点分隔的元素，其包含的类型和命名空间名称。 如果项本身的名称包含句点，它们替换为井号 （#）。 （假定没有任何元素的名称中包含此字符。）具有类型参数的类型的名称结尾反引号 （'） 后跟一个数字，表示类型的类型参数的数目。 务必要记住的因为嵌套的类型有权访问包含这些类型的类型参数、 嵌套的类型隐式包含其包含类型的类型参数和这些类型以在其类型参数总计的计数用例。

对于方法和属性使用的参数，参数列表如下所示，括在括号中。 对于不带参数，则省略括号。 确保自变量之间用逗号分隔。 每个自变量的编码相同的 CLI 签名，如下所示：参数由其完全限定名称表示。 例如，`Integer`变得`System.Int32`，`String`变得`System.String`，`Object`变得`System.Object`，依次类推。 自变量具有`ByRef`修饰符有 @ 按照其类型名称。 参数具有`ByVal`，`Optional`或`ParamArray`修饰符有没有特殊表示法。 参数是数组表示为`[lowerbound:size, ..., lowerbound:size]`其中逗号的数量是秩-1，和的下限和大小的每个维度，以十进制表示如果已知。 如果未指定下限或大小，则省略它。 如果省略某个特定维度的下限和大小，也会省略“:”。 一个表示数组的数组"`[]`"每个级别。

### <a name="id-string-examples"></a>ID 字符串示例

下面的示例分别演示 VB 代码，以及从支持的文档注释每个源元素生成的 ID 字符串的片段：

类型表示使用其完全限定的名称。

```vb
Enum Color
    Red
    Blue
    Green
End Enum

Namespace Acme
    Interface IProcess
    End Interface

    Structure ValueType
        ...
    End Structure

    Class Widget
        Public Class NestedClass
        End Class

        Public Interface IMenuItem
        End Interface

        Public Delegate Sub Del(i As Integer)

        Public Enum Direction
            North
            South
            East
            West
        End Enum
    End Class
End Namespace

"T:Color"
"T:Acme.IProcess"
"T:Acme.ValueType"
"T:Acme.Widget"
"T:Acme.Widget.NestedClass"
"T:Acme.Widget.IMenuItem"
"T:Acme.Widget.Del"
"T:Acme.Widget.Direction"
```

字段由其完全限定名称表示。

```vb
Namespace Acme
    Structure ValueType
        Private total As Integer
    End Structure

    Class Widget
        Public Class NestedClass
            Private value As Integer
        End Class

        Private message As String
        Private Shared defaultColor As Color
        Private Const PI As Double = 3.14159
        Protected ReadOnly monthlyAverage As Double
        Private array1() As Long
        Private array2(,) As Widget
    End Class
End Namespace

"F:Acme.ValueType.total"
"F:Acme.Widget.NestedClass.value"
"F:Acme.Widget.message"
"F:Acme.Widget.defaultColor"
"F:Acme.Widget.PI"
"F:Acme.Widget.monthlyAverage"
"F:Acme.Widget.array1"
"F:Acme.Widget.array2"
```

构造函数。

```vb
Namespace Acme
    Class Widget
        Shared Sub New()
        End Sub

        Public Sub New()
        End Sub

        Public Sub New(s As String)
        End Sub
    End Class
End Namespace

"M:Acme.Widget.#cctor"
"M:Acme.Widget.#ctor"
"M:Acme.Widget.#ctor(System.String)"
```

方法。

```vb
Namespace Acme
    Structure ValueType
        Public Sub M(i As Integer)
        End Sub
    End Structure

    Class Widget
        Public Class NestedClass
            Public Sub M(i As Integer)
            End Sub
        End Class

        Public Shared Sub M0()
        End Sub

        Public Sub M1(c As Char, ByRef f As Float, _
            ByRef v As ValueType)
        End Sub

        Public Sub M2(x1() As Short, x2(,) As Integer, _
            x3()() As Long)
        End Sub

        Public Sub M3(x3()() As Long, x4()(,,) As Widget)
        End Sub

        Public Sub M4(Optional i As Integer = 1)

        Public Sub M5(ParamArray args() As Object)
        End Sub
    End Class
End Namespace

"M:Acme.ValueType.M(System.Int32)"
"M:Acme.Widget.NestedClass.M(System.Int32)"
"M:Acme.Widget.M0"
"M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
"M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
"M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
"M:Acme.Widget.M4(System.Int32)"
"M:Acme.Widget.M5(System.Object[])"
```

属性。

```vb
Namespace Acme
    Class Widget
        Public Property Width() As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property

        Public Default Property Item(i As Integer) As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property

        Public Default Property Item(s As String, _
            i As Integer) As Integer
            Get
            End Get
            Set (Value As Integer)
            End Set
        End Property
    End Class
End Namespace

"P:Acme.Widget.Width"
"P:Acme.Widget.Item(System.Int32)"
"P:Acme.Widget.Item(System.String,System.Int32)"
```

事件   

```vb
Namespace Acme
    Class Widget
        Public Event AnEvent As EventHandler
        Public Event AnotherEvent()
    End Class
End Namespace

"E:Acme.Widget.AnEvent"
"E:Acme.Widget.AnotherEvent"
```

运算符。

```vb
Namespace Acme
    Class Widget
        Public Shared Operator +(x As Widget) As Widget
        End Operator

        Public Shared Operator +(x1 As Widget, x2 As Widget) As Widget
        End Operator
    End Class
End Namespace

"M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
"M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
```

转换运算符具有一个尾随`~`跟的返回类型。

```vb
Namespace Acme
    Class Widget
        Public Shared Narrowing Operator CType(x As Widget) As _
            Integer
        End Operator

        Public Shared Widening Operator CType(x As Widget) As Long
        End Operator
    End Class
End Namespace

"M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
"M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
```

## <a name="documentation-comments-example"></a>文档注释示例

下面的示例显示了源代码`Point`类：

```vb
Namespace Graphics
    ''' <remarks>
    ''' Class <c>Point</c> models a point in a two-dimensional
    ''' plane.
    ''' </remarks>
    Public Class Point
        ''' <summary>
        ''' Instance variable <c>x</c> represents the point's x-coordinate.
        ''' </summary>
        Private _x As Integer

        ''' <summary>
        ''' Instance variable <c>y</c> represents the point's y-coordinate.
        ''' </summary>
        Private _y As Integer

        ''' <value>
        ''' Property <c>X</c> represents the point's x-coordinate.
        ''' </value>
        Public Property X() As Integer
            Get
                Return _x
            End Get
            Set(Value As Integer)
                _x = Value
            End Set
        End Property

        ''' <value>
        ''' Property <c>Y</c> represents the point's y-coordinate.
        ''' </value>
        Public Property Y() As Integer
            Get
                Return _y
            End Get
            Set(Value As Integer)
                _y = Value
            End Set
        End Property

        ''' <summary>
        ''' This constructor initializes the new Point to (0,0).
        ''' </summary>
        Public Sub New()
            Me.New(0, 0)
        End Sub

        ''' <summary>
        ''' This constructor initializes the new Point to
        ''' (<paramref name="x"/>,<paramref name="y"/>).
        ''' </summary>
        ''' <param name="x"><c>x</c> is the new Point's
        ''' x-coordinate.</param>
        ''' <param name="y"><c>y</c> is the new Point's
        ''' y-coordinate.</param>
        Public Sub New(x As Integer, y As Integer)
            Me.X = x
            Me.Y = y
        End Sub

        ''' <summary>
        ''' This method changes the point's location to the given
        ''' coordinates.
        ''' </summary>
        ''' <param name="x"><c>x</c> is the new x-coordinate.</param>
        ''' <param name="y"><c>y</c> is the new y-coordinate.</param>
        ''' <see cref="Translate"/>
        Public Sub Move(x As Integer, y As Integer)
            Me.X = x
            Me.Y = y
        End Sub

        ''' <summary>
        ''' This method changes the point's location by the given x- and
        ''' y-offsets.
        ''' <example>
        ''' For example:
        ''' <code>
        '''    Dim p As Point = New Point(3, 5)
        '''    p.Translate(-1, 3)
        ''' </code>
        ''' results in <c>p</c>'s having the value (2,8).
        ''' </example>
        ''' </summary>
        ''' <param name="x"><c>x</c> is the relative x-offset.</param>
        ''' <param name="y"><c>y</c> is the relative y-offset.</param>
        ''' <see cref="Move"/>
        Public Sub Translate(x As Integer, y As Integer)
            Me.X += x
            Me.Y += y
        End Sub

        ''' <summary>
        ''' This method determines whether two Points have the same
        ''' location.
        ''' </summary>
        ''' <param name="o"><c>o</c> is the object to be compared to the
        ''' current object.</param>
        ''' <returns>
        ''' True if the Points have the same location and they have the
        ''' exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Operator op_Equality"/>
        ''' <seealso cref="Operator op_Inequality"/>
        Public Overrides Function Equals(o As Object) As Boolean
            If o Is Nothing Then
                Return False
            End If
            If o Is Me Then
                Return True
            End If
            If Me.GetType() Is o.GetType() Then
                Dim p As Point = CType(o, Point)
                Return (X = p.X) AndAlso (Y = p.Y)
            End If
            Return False
        End Function

        ''' <summary>
        ''' Report a point's location as a string.
        ''' </summary>
        ''' <returns>
        ''' A string representing a point's location, in the form
        ''' (x,y), without any leading, training, or embedded whitespace.
        ''' </returns>
        Public Overrides Function ToString() As String
            Return "(" & X & "," & Y & ")"
        End Function

        ''' <summary>
        ''' This operator determines whether two Points have the
        ''' same location.
        ''' </summary>
        ''' <param name="p1"><c>p1</c> is the first Point to be compared.
        ''' </param>
        ''' <param name="p2"><c>p2</c> is the second Point to be compared.
        ''' </param>
        ''' <returns>
        ''' True if the Points have the same location and they 
        ''' have the exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Equals"/>
        ''' <seealso cref="op_Inequality"/>
        Public Shared Operator =(p1 As Point, p2 As Point) As Boolean
            If p1 Is Nothing OrElse p2 Is Nothing Then
                Return False
            End If
            If p1.GetType() Is p2.GetType() Then
                Return (p1.X = p2.X) AndAlso (p1.Y = p2.Y)
            End If
            Return False
        End Operator

        ''' <summary>
        ''' This operator determines whether two Points have the
        ''' same location.
        ''' </summary>
        ''' <param name="p1"><c>p1</c> is the first Point to be comapred.
        ''' </param>
        ''' <param name="p2"><c>p2</c> is the second Point to be compared.
        ''' </param>
        ''' <returns>
        ''' True if the Points do not have the same location and
        ''' the exact same type; otherwise, false.
        ''' </returns>
        ''' <seealso cref="Equals"/>
        ''' <seealso cref="op_Equality"/>
        Public Shared Operator <>(p1 As Point, p2 As Point) As Boolean
            Return Not p1 = p2
        End Operator

        ''' <summary>
        ''' This is the entry point of the Point class testing program.
        ''' <para>This program tests each method and operator, and
        ''' is intended to be run after any non-trvial maintenance has
        ''' been performed on the Point class.</para>
        ''' </summary>
        Public Shared Sub Main()
            ' class test code goes here
        End Sub
    End Class
End Namespace
```

下面是当给定类的源代码时生成的输出`Point`，上面所示：

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <remarks>Class <c>Point</c> models a point in a
            two-dimensional plane. </remarks>
        </member>
        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>
        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>
        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
            (0,0).</summary>
        </member>
        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="x"/>,<paramref name="y"/>).</summary>
            <param><c>x</c> is the new Point's x-coordinate.</param>
            <param><c>y</c> is the new Point's y-coordinate.</param>
        </member>
        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>x</c> is the new x-coordinate.</param>
            <param><c>y</c> is the new y-coordinate.</param>
            <see cref=
            "M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>
        <member name=
        "M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by the given
            x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>x</c> is the relative x-offset.</param>
            <param><c>y</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>
        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the
            same location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.</param>
            <returns>True if the Points have the same location and they
            have the exact same type; otherwise, false.</returns>
            <seealso cref=
            "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"
            />
            <seealso cref=
           "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y), without any leading, training, or embedded
            whitespace.</returns>
        </member>
        <member name=
        "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they
            have the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso cref=
           "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name=
        "M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso cref=
            "M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"
            />
        </member>
        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trvial maintenance has
            been performed on the Point class.</para>
            </summary>
        </member>
        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>
        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
