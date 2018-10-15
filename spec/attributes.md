# <a name="attributes"></a>特性

Visual Basic 语言使程序员能够指定修饰符上声明，表示正在声明的实体有关的信息。 例如，附加和修饰符类方法`Public`， `Protected`， `Friend`， `Protected Friend`，或`Private`指定其可访问性。

除了由语言定义的修饰符，Visual Basic 还使程序员可以创建名为的新修饰符*特性*，将其声明新实体时。 这些新修饰符，通过特性类的声明定义，然后分配给通过实体*特性块*。

__请注意。__ 可以在运行时通过.NET Framework 反射 Api 检索属性。 这些 Api 是超出了本规范的范围。

例如，可以定义一个框架`Help`可以放在如类和方法，以提供到文档中，从程序元素的映射的程序元素中，如以下示例所示的属性：

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class HelpAttribute
    Inherits Attribute

    Public Sub New(urlValue As String)
        Me.UrlValue = urlValue
    End Sub

    Public Topic As String
    Private UrlValue As String

    Public ReadOnly Property Url() As String
        Get
            Return UrlValue
        End Get
    End Property
End Class
```

该示例定义一个名为属性类`HelpAttribute`，或`Help`缩写，它具有一个位置参数 (`UrlValue`) 和一个名为参数 (`Topic`)。

下面的示例演示几种用法的属性：

```vb
<Help("http://www.example.com/.../Class1.htm")> _
Public Class Class1
    <Help("http://www.example.com/.../Class1.htm", Topic:="F")> _
    Public Sub F()
    End Sub
End Class
```

下一步的示例将检查以查看是否`Class1`已`Help`属性，并写出关联`Topic`和`Url`值是否存在该属性。

```vb
Module Test
    Sub Main()
        Dim type As Type = GetType(Class1)
        Dim arr() As Object = _
            type.GetCustomAttributes(GetType(HelpAttribute), True)

        If arr.Length = 0 Then
            Console.WriteLine("Class1 has no Help attribute.")
        Else
            Dim ha As HelpAttribute = CType(arr(0), HelpAttribute)
            Console.WriteLine("Url = " & ha.Url & ", Topic = " & ha.Topic)
        End If
    End Sub
End Module
```

## <a name="attribute-classes"></a>属性类

*特性类*是派生自非泛型类`System.Attribute`并不是`MustInherit`。 特性类可能具有`System.AttributeUsage`声明该属性是对有效、 是否它可能使用多个时间在声明中，以及是否继承的属性。 下面的示例定义一个名为属性类`SimpleAttribute`可放置在类声明和接口声明上：

```vb
<AttributeUsage(AttributeTargets.Class Or AttributeTargets.Interface)> _
Public Class SimpleAttribute
    Inherits System.Attribute
End Class
```

下面的示例演示的一些用途`Simple`属性。 尽管为属性类，但`SimpleAttribute`，可以忽略此属性的用法`Attribute`后缀，从而缩短该名称与`Simple`:

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

如果该属性缺少`System.AttributeUsage`，则该属性可以置于任何目标 (等效于`AttributeTargets.All`)。 `System.AttributeUsage`属性具有变量的初始值设定项， `AllowMultiple`，它指定是否可以多次指定指示的属性对于给定的声明。 如果`AllowMultiple`属性是`True`，它是*多用途特性类*，并指定一次在声明。 如果`AllowMultiple`属性是`False`或未指定的属性，它是*一次性特性类*，并指定最多一次在声明。

下面的示例定义一个名为的多用途特性类`AuthorAttribute`:

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True)> _
Public Class AuthorAttribute
    Inherits System.Attribute

    Private _Value As String

    Public Sub New(value As String)
        Me._Value = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return _Value
        End Get
    End Property
End Class
```

该示例演示具有两种含义的类声明`Author`属性：

```vb
<Author("Maria Hammond"), Author("Ramesh Meyyappan")> _
Class Class1
End Class
```

`System.AttributeUsage`属性具有一个公共的实例变量`Inherited`，指定是否属性，指定为基类型上时还继承自此基类型派生的类型。 如果`Inherited`公共实例变量未初始化，默认值为`False`使用。 属性和事件不会继承属性，尽管执行这一定义属性和事件的方法。 接口不会继承属性。

如果一次性属性同时继承和派生类型上指定，派生类型上指定的属性重写继承的特性。 如果多用途特性同时继承和派生类型上指定，则派生类型上指定这两个属性。 例如：

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True, _
                Inherited:=True) > _
Class MultiUseAttribute
    Inherits System.Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<AttributeUsage(AttributeTargets.Class, Inherited:=True)> _
Class SingleUseAttribute
    Inherits Attribute

    Public Sub New(value As Boolean)
    End Sub
End Class

<SingleUse(True), MultiUse(True)> Class Base
End Class

' Derived has three attributes defined on it: SingleUse(False),
' MultiUse(True) and MultiUse(False)
<SingleUse(False), MultiUse(False)> _
Class Derived
    Inherits Base
End Class
```

由属性类的公共构造函数的参数定义特性的位置的参数。 位置参数必须是`ByVal`并不能指定`ByRef`。 由公共读写属性或特性类的实例变量定义公共实例变量和属性。 可以使用位置参数和公共实例变量和属性中的类型仅限于属性类型。 类型为属性类型，如果它是以下之一：

* 除任何基元类型`Date`和`Decimal`。

* `Object` 类型。

* `System.Type` 类型。

* 枚举的类型，提供它，并在其中它嵌套 （如果有） 的类型具有`Public`可访问性。

* 此列表中以前的类型之一的一维数组。

## <a name="attribute-blocks"></a>特性块

以指定属性*特性块*。 通过尖括号 ("<>") 分隔每个特性块并在属性块中以逗号分隔的列表中或多个特性块中，可以指定多个属性。 指定属性的顺序并不重要。 例如，特性块`<A, B>`， `<B, A>`，`<A> <B>`和`<B> <A>`都是等效的。

```antlr
Attributes
    : AttributeBlock+
    ;

AttributeBlock
    : LineTerminator? '<' AttributeList LineTerminator? '>' LineTerminator?
    ;

AttributeList
    : Attribute ( Comma Attribute )*
    ;

Attribute
    : ( AttributeModifier ':' )? SimpleTypeName
    ( OpenParenthesis AttributeArguments? CloseParenthesis )?
    ;

AttributeModifier
    : 'Assembly' | 'Module'
    ;
```

属性不能指定类型的声明不是支持和一次性属性不能指定多个一次在特性块中。 下面的示例会导致这两个错误，因为它尝试使用`HelpString`接口上`Interface1`和多次的声明上`Class1`。

```vb
<AttributeUsage(AttributeTargets.Class)> _
Public Class HelpStringAttribute
    Inherits System.Attribute

    Private InternalValue As String

    Public Sub New(value As String)
        Me.InternalValue = value
    End Sub

    Public ReadOnly Property Value() As String
        Get
            Return InternalValue
        End Get
    End Property
End Class

' Error: HelpString only applies to classes.
<HelpString("Description of Interface1")> _
Interface Interface1
    Sub Sub1()
End Interface

' Error: HelpString is single-use.
<HelpString("Description of Class1"), _
    HelpString("Another description of Class1")> _
Public Class Class1
End Class
```

属性包含了可选特性修饰符、 属性名称、 可选的位置参数和变量/属性初始值设定项列表。 如果没有任何参数或初始值设定项，则可省略括号。 如果某一特性有一个修饰符，则它必须在一个源文件顶部特性块中。

如果源代码文件包含特性块指定特性的程序集或模块将包含的源文件的文件的顶部，必须以特性块中的每个属性由前缀`Assembly`或`Module`修饰符和冒号。


### <a name="attribute-names"></a>属性名称

属性的名称指定的特性类。 按照约定，特性类名为带有后缀`Attribute`。 使用属性可以包括或省略此后缀。 因此，对应于属性标识符的特性类的名称是本身的标识符或限定标识符的串联和`Attribute`。 当编译器解析属性名称时，它将追加`Attribute`的名称，然后尝试查找匹配。 如果查找失败，编译器将尝试查找不带后缀。 例如，特性类使用`SimpleAttribute`可能会省略`Attribute`后缀，从而缩短该名称与`Simple`:

```vb
<Simple> Class Class1
End Class

<Simple> Interface Interface1
End Interface
```

上面的示例在语义上等效于下面：

```vb
<SimpleAttribute> Class Class1
End Class

<SimpleAttribute> Interface Interface1
End Interface
```

一般情况下，属性名后缀为`Attribute`首选分发点。 下面的示例演示两个属性类，名为`T`和`T``Attribute`。

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class T
    Inherits System.Attribute
End Class

<AttributeUsage(AttributeTargets.All)> _
Public Class TAttribute
    Inherits System.Attribute
End Class

' Refers to TAttribute.
<T> Class Class1 
End Class

' Refers to TAttribute.
<TAttribute> Class Class2 
End Class
```

特性块`<T>`和特性块`<TAttribute>`到名为的属性类，请参阅`TAttribute`。 不能使用`T`作为属性之前删除类的声明`TAttribute`。

### <a name="attribute-arguments"></a>特性参数

属性的自变量可能采用两种形式：*位置自变量*并*实例变量/属性初始值设定项*。 任何位置自变量的属性必须在前面的实例变量/属性初始值设定项。 位置自变量包含的常量表达式，一维数组创建表达式或`GetType`表达式。 实例变量/属性初始值设定项包含的标识符，可以匹配关键字后, 跟冒号和等号，由常数表达式终止或`GetType`表达式。

已知属性的特性类`T`，位置自变量列表`P`，并实例变量/属性初始值设定项列表`N`，以下步骤确定参数是否有效：

1. 请按照用于编译形式的表达式的编译时处理步骤`New T(P)`。 这会导致编译时错误或确定一个构造函数上`T`这就是最适用于自变量列表。

2. 如果在步骤 1 中确定的构造函数不是属性类型的参数，或无法访问位于声明站点，会发生编译时错误。

3. 对于每个实例变量/属性初始值设定项`Arg`中`N`，可让`Name`是实例变量/属性初始值设定项的标识符`Arg`。 `Name` 必须标识一个非`Shared`、 可写`Public`上的实例变量或无参数属性`T`其类型为属性类型。 如果`T`具有无此类的实例变量或属性，则发生编译时错误。

例如：

```vb
<AttributeUsage(AttributeTargets.All)> _
Public Class GeneralAttribute
    Inherits Attribute

    Public Sub New(x As Integer)
    End Sub

    Public Sub New(x As Double)
    End Sub

    Public y As Type

    Public Property z As Integer
        Get
        End Get

        Set
        End Set
    End Property
End Class

' Calls the first constructor.
<General(10, z:=30, y:=GetType(Integer))> _
Class C1
End Class

' Calls the second constructor.
<General(10.5, z:=10)> _
Class C2
End Class
```

特性实参中不能任意位置使用类型参数。 但是，可以使用构造的类型：

```vb
<AttributeUsage(AttributeTargets.All)> _
Class A 
   Inherits System.Attribute 

   Public Sub New(t As Type)
   End Sub 
End Class

Class List(Of T) 
    ' Error: attribute argument cannot use type parameter
    <A(GetType(T))> Dim t1 As T 

    ' OK: closed type
    <A(GetType(List(Of Integer)))> Dim y As Integer
End Class
```


```antlr
AttributeArguments
    : AttributePositionalArgumentList
    | AttributePositionalArgumentList Comma VariablePropertyInitializerList
    | VariablePropertyInitializerList
    ;

AttributePositionalArgumentList
    : AttributeArgumentExpression? ( Comma AttributeArgumentExpression? )*
    ;

VariablePropertyInitializerList
    : VariablePropertyInitializer ( Comma VariablePropertyInitializer )*
    ;

VariablePropertyInitializer
    : IdentifierOrKeyword ColonEquals AttributeArgumentExpression
    ;

AttributeArgumentExpression
    : ConstantExpression
    | GetTypeExpression
    | ArrayExpression
    ;
```