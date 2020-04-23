---
ms.openlocfilehash: 54e674bedd587647436b859423ab0f14715eca2d
ms.sourcegitcommit: 19ec79a287fb79180b05a0ad20e8291e75fc63df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82080598"
---
# <a name="documentation-comments"></a>文档注释

文档注释是源中经过特殊格式的注释，可以进行分析，以生成有关它们附加到的代码的文档。 文档注释的基本格式是 XML。 当编译代码包含文档注释时，编译器可以选择发出一个 XML 文件，该文件表示源中文档注释的总和。 然后，其他工具可以使用此 XML 文件生成打印或联机文档。

本章介绍文档注释和建议与文档注释一起使用的 XML 标记。

## <a name="documentation-comment-format"></a>文档注释格式

文档注释是以`'''`、三个单引号开头的特殊注释。 它们必须紧接在它们记录的类型（如类、委托或接口）或类型成员（如字段、事件、属性或方法）之前。 部分方法声明的文档注释将被提供其正文的方法的文档注释替换（如果有）。 所有相邻的文档注释都追加在一起，以生成单个文档注释。 如果`'''`字符后有一个空格字符，则该空白字符不包括在串联中。 例如：

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

文档注释必须根据 格式良好的https://www.w3.org/TR/REC-xmlXML。 如果 XML 格式不正确，将生成警告，文档文件将包含注释，指出遇到错误。

尽管开发人员可以自由创建自己的标记集，但建议的标记集在下一节中定义。 部分建议标记具有特殊含义：

* 标记`<param>`用于描述参数。 `<param>`标记指定的参数必须存在，并且类型成员的所有参数都必须在文档注释中描述。 如果任一条件都不正确，编译器将发出警告。

* `cref` 属性可以附加到任何标记，以提供对代码元素的引用。 代码元素必须存在;在编译时，编译器将名称替换为表示成员的 ID 字符串。 如果代码元素不存在，编译器将发出警告。 查找`cref`属性中描述的名称时，编译器会尊重`Imports`显示在包含源文件中的语句。

* 文档`<summary>`查看器将使用该标记来显示有关类型或成员的其他信息。

请注意，文档文件不提供有关类型和成员的完整信息，仅提供文档注释中包含的内容。 要获取有关类型或成员的详细信息，文档文件必须与对实际类型或成员的反射结合使用。

## <a name="recommended-tags"></a>建议的标记

文档生成器必须接受和处理根据 XML 规则有效的任何标记。 下列标记提供用户文档中的常用功能：

`<c>`以类似代码的字体设置文本

`<code>`以类似代码的字体设置一行或多行源代码或程序输出

`<example>`指示示例

`<exception>`标识方法可以引发的异常

`<include>`包括外部 XML 文档

`<list>`创建列表或表

`<para>`允许将结构添加到文本中

`<param>`描述方法或构造函数的参数

`<paramref>`标识单词是参数名称

`<permission>`记录成员的安全可访问性

`<remarks>`描述类型

`<returns>`描述方法的返回值

`<see>`指定链接

`<seealso>`生成"另请参阅"条目

`<summary>`描述类型的成员

`<typeparam>`描述类型参数

`<value>`描述属性

### <a name="ltcgt"></a>&lt;c&gt;

此标记指定描述中的文本片段应使用类似于代码块的字体。 （对于实际代码行，请使用`<code>`.）

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

### <a name="ltcodegt"></a>&lt;code&gt;

此标记指定一行或多行源代码或程序输出应使用固定宽度字体。 （对于小代码片段，请使用`<c>`.）

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

此标记允许注释中的示例代码显示如何使用元素。 通常，这也将涉及使用标记`<code>`。

__语法：__

```xml
<example>description</example>
```

__示例：__

有关示例，请参见 `<code>`。

### <a name="ltexceptiongt"></a>&lt;exception&gt;

此标记提供了一种记录方法可以引发异常的方法的方法的方法的方法的方法的方法的方法的方法。

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

此标记用于包括来自外部格式良好的 XML 文档中的信息。 XPath 表达式应用于 XML 文档，以指定应从文档中包含哪些 XML。 然后`<include>`，将标记替换为外部文档中的选定 XML。

__语法：__

```xml
<include file="filename" path="xpath">
```

__示例：__

如果源代码包含如下声明：

```vb
''' <include file="docs.xml" path="extra/class[@name="IntList"]/*" />
```

和外部文件 docs.xml 具有以下内容

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

然后输出相同的文档，就像源代码包含一样：

```xml
''' <summary>
''' Contains a list of integers.
''' </summary>
```

### <a name="ltlistgt"></a>&lt;list&gt;

此标记用于创建项目的列表或表。 它可能包含一个`<listheader>`块来定义表或定义列表的标题行。 （定义表时，只需要提供标题中术语的条目。

列表中的每个项都使用`<item>`块指定。 创建定义列表时，必须指定术语和说明。 但是，对于表、项目符号列表或编号列表，只需指定描述。

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

此标记用于其他标记（如`<remarks>`或`<returns>`）内，并允许将结构添加到文本中。

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

此标记描述方法、构造函数或索引属性的参数。

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

此标记指示单词是参数。 可以处理文档文件，以某种不同的方式格式化此参数。

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

### <a name="ltpermissiongt"></a>permission&lt;&gt;

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

此标记指定有关类型的概述信息。 （用于`<summary>`描述类型的成员。

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

此标记允许在文本中指定链接。 （用于`<seealso>`指示要显示在"另请参阅"部分的文本。

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

此标记为"另请参阅"部分生成条目。 （用于`<see>`从文本中指定链接。

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

此标记描述类型成员。 （用于`<remarks>`描述类型本身。

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

### <a name="ltvaluegt"></a>&lt;value&gt;

此标记描述属性。

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

生成文档文件时，编译器会为源代码中的每个元素生成一个 ID 字符串，该元素使用唯一标识它的文档注释进行标记。 外部工具可以使用此 ID 字符串来标识编译程序集中的哪个元素对应于文档注释。

生成 ID 字符串如下所示：

字符串不得包含空格。

字符串的第一部分通过单个字符后跟冒号标识要记录的成员类型。 定义了以下类型的成员，括号中的相应字符：事件 （E）、字段 （F）、方法（包括构造函数和运算符 （M）、命名空间 （N）、属性 （P） 和类型 （T）。 感叹号 （！） 表示生成 ID 字符串时发生错误，字符串的其余部分提供有关错误的信息。

字符串的第二部分是元素的完全限定名称，从全局命名空间开始。 元素的名称、其封闭类型和命名空间由句点分隔。 如果项目本身的名称具有句点，则它们将被磅符号 （#） 替换。 （假定没有元素的名称中具有此字符。具有类型参数的类型的名称以回引 （'） 结尾，后跟一个数字表示类型上的类型参数数。 请务必记住，由于嵌套类型有权访问包含它们的类型的类型参数，因此嵌套类型隐式包含其包含类型的类型参数，在这种情况下，这些类型将计入其类型参数总计中。

对于具有参数的方法和属性，参数列表如下，包含在括号中。 对于没有参数的参数，省略括号。 确保自变量之间用逗号分隔。 每个参数的编码与 CLI 签名相同，如下所示：参数由其完全限定的名称表示。 例如，`Integer`变为`System.Int32` `String` `System.String`，变为`Object`，`System.Object`等等。 具有修饰符的`ByRef`参数的类型名称后有一个"*"。 具有 的`ByVal``Optional``ParamArray`参数 没有 特殊的表示法。 数组的参数表示为`[lowerbound:size, ..., lowerbound:size]`逗号数为排名 - 1，每个维度的下界和大小（如果已知）以小数表示。 如果未指定下限或大小，则省略它。 如果省略某个特定维度的下限和大小，也会省略“:”。 数组数组由每级别一个""`[]`表示。

### <a name="id-string-examples"></a>ID 字符串示例

以下每个示例都显示了 VB 代码的片段，以及从每个源元素生成的 ID 字符串，这些元素能够具有文档注释：

类型使用其完全限定的名称表示。

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

字段由其完全限定的名称表示。

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

运营商。

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

转换运算符具有尾随`~`，后跟返回类型。

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

下面的示例显示了`Point`类的源代码：

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

下面是给定类`Point`的源代码时生成的输出，如下所示：

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
