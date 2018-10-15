# <a name="types"></a>类型

在 Visual Basic 中的类型的两个基本类别如下*值类型*并*引用类型*。 基元类型 （除了字符串）、 枚举和结构是值类型。 类、 字符串、 标准模块、 接口、 数组和委托是引用类型。

每个类型具有*默认值*，这是分配给在初始化时该类型的变量的值。

```antlr
TypeName
    : ArrayTypeName
    | NonArrayTypeName
    ;

NonArrayTypeName
    : SimpleTypeName
    | NullableTypeName
    ;

SimpleTypeName
    : QualifiedTypeName
    | BuiltInTypeName
    ;

QualifiedTypeName
    : Identifier TypeArguments? (Period IdentifierOrKeyword TypeArguments?)*
    | 'Global' Period IdentifierOrKeyword TypeArguments?
      (Period IdentifierOrKeyword TypeArguments?)*
    ;

TypeArguments
    : OpenParenthesis 'Of' TypeArgumentList CloseParenthesis
    ;

TypeArgumentList
    : TypeName ( Comma TypeName )*
    ;

BuiltInTypeName
    : 'Object'
    | PrimitiveTypeName
    ;

TypeModifier
    : AccessModifier
    | 'Shadows'
    ;

IdentifierModifiers
    : NullableNameModifier? ArrayNameModifier?
    ;
```

## <a name="value-types-and-reference-types"></a>Value Types and Reference Types

值类型和引用类型可以是在声明语法和用法方面相似，尽管它们的语义是截然不同的。

引用类型存储在运行时堆中;它们可以通过引用该存储访问。 由于引用类型始终通过引用访问，由.NET Framework 管理其生存期。 未完成的跟踪对特定实例的引用，并且仅当没有更多的引用保持时销毁该实例。 引用类型的变量包含对该类型的值，派生程度更大的类型的值或 null 值的引用。 一个*null 值*指的是执行任何操作; 不能使用 null 值，但将其分配执行任何操作。 引用类型的变量进行赋值创建引用的副本而不是非引用值的副本。 对于引用类型的变量，默认值为 null 值。

值类型存储在直接在堆栈上，数组内或在另一个类型;仅可以直接访问其存储。 由于直接在变量中存储的值类型，其生存期由包含它们的变量的生存期确定。 当销毁包含值类型实例的位置时，值类型实例也会被销毁。 值类型始终进行直接调用访问不能创建对值类型的引用。 禁止这种引用会导致无法引用已销毁的值类实例。 由于值类型始终是`NotInheritable`，值类型的变量始终包含该类型的值。 因此，值类型的值不能为 null 值，也不可以引用派生程度更大的类型的对象。 值类型的变量进行赋值会创建一份所赋的值。 对于值类型的变量，默认值为初始化为其默认值的类型的每个变量成员的结果。

下面的示例显示引用类型和值类型之间的差异：

```vb
Class Class1
    Public Value As Integer = 0
End Class

Module Test
    Sub Main()
        Dim val1 As Integer = 0
        Dim val2 As Integer = val1
        val2 = 123
        Dim ref1 As Class1 = New Class1()
        Dim ref2 As Class1 = ref1
        ref2.Value = 123
        Console.WriteLine("Values: " & val1 & ", " & val2)
        Console.WriteLine("Refs: " & ref1.Value & ", " & ref2.Value)
    End Sub
End Module
```

该程序的输出为：

```
Values: 0, 123
Refs: 123, 123
```

对本地变量的赋值`val2`不会影响本地变量`val1`由于这两个本地变量的值类型 (类型`Integer`) 和值类型的每个本地变量都有其自己的存储。 与之相反，赋值`ref2.Value = 123;`影响的对象，同时`ref1`和`ref2`引用。

关于.NET Framework 类型系统需要注意的一点是，即使结构、 枚举和基元类型 (除`String`) 是值类型，它们都继承自引用类型。 结构和基元类型继承自引用类型`System.ValueType`，后者又继承`Object`。 枚举的类型都继承自引用类型`System.Enum`，后者又继承`System.ValueType`。

### <a name="nullable-value-types"></a>可以为 Null 的值类型

对于值类型，`?`修饰符可以添加到类型名称来表示*可以为 null*该类型的版本。

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

可以为 null 值类型可以包含相同的值不可以为 null 的类型的版本以及 null 值。 因此，对于可以为 null 的值类型，将分配`Nothing`到类型的变量设置为 null 值，而不是零个值的值类型的变量的值。 例如：

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

此外可能会将变量声明为可以为 null 的值类型的通过将为 null 的类型修饰符放在变量名称。 为清楚起见，不能用来在同一声明中有变量名称和类型名称为 null 的类型修饰符。 由于可以为 null 的类型使用该类型实现`System.Nullable(Of T)`，类型`T?`等效于类型`System.Nullable(Of T)`，并且这两个名称可以互换使用。 `?`修饰符不能将其置于可以为 null 的类型; 因此，不能声明类型`Integer??`或`System.Nullable(Of Integer)?`。

可以为 null 值类型`T?`具有的成员`System.Nullable(Of T)`以及任何运算符或转换*提升*从基础类型`T`类型到`T?`。 提升副本运算符和转换从基础类型，在大多数情况下替换的不可以为 null 的值类型可以为 null 的值类型。 这样，相同的转换和适用于操作的许多`T`要应用于`T?`也。


## <a name="interface-implementation"></a>接口实现

结构和类声明可以声明它们实现的一组通过一个或多个接口类型`Implements`子句。

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

中指定的所有类型`Implements`子句必须是接口，并且该类型必须实现的接口的所有成员。 例如：

```vb
Interface ICloneable
    Function Clone() As Object
End Interface 

Interface IComparable
    Function CompareTo(other As Object) As Integer
End Interface 

Structure ListEntry
    Implements ICloneable, IComparable

    ...

    Public Function Clone() As Object Implements ICloneable.Clone
        ...
    End Function 

    Public Function CompareTo(other As Object) As Integer _
        Implements IComparable.CompareTo
        ...
    End Function 
End Structure
```

此外将隐式实现接口的类型实现的所有接口的基接口。 即使该类型不会显式列出中的所有基接口是 true`Implements`子句。 在此示例中，`TextBox`结构同时实现了`IControl`和`ITextBox`。

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Structure TextBox
    Implements ITextBox

    ...

    Public Sub Paint() Implements ITextBox.Paint
        ...
    End Sub

    Public Sub SetText(text As String) Implements ITextBox.SetText
        ...
    End Sub
End Structure
```

声明一个类型实现的接口，本身不声明类型的声明空间中的任何内容。 因此，它是有效使用的方法实现两个接口，通过相同的名称。

类型不能实现自身、 类型参数，尽管它可能涉及到作用域中的类型参数。

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

泛型接口可以实现的多次使用不同的类型参数。 但是，泛型类型不能实现泛型接口使用类型参数，如果提供的类型参数 （而不考虑类型约束） 无法与另一个实现该接口的重叠。 例如：

```vb
Interface I1(Of T)
End Interface

Class C1
    Implements I1(Of Integer)
    Implements I1(Of Double)    ' OK, no overlap
End Class

Class C2(Of T)
    Implements I1(Of Integer)
    Implements I1(Of T)         ' Error, T could be Integer
End Class
```


## <a name="primitive-types"></a>基元类型

*基元类型*通过是预定义别名的关键字进行标识中的类型`System`命名空间。 基元类型是完全无法区分类型从其别名： 编写保留的字`Byte`正是写入相同`System.Byte`。 基元类型也被称为*内部类型*。

```antlr
PrimitiveTypeName
    : NumericTypeName
    | 'Boolean'
    | 'Date'
    | 'Char'
    | 'String'
    ;

NumericTypeName
    : IntegralTypeName
    | FloatingPointTypeName
    | 'Decimal'
    ;

IntegralTypeName
    : 'Byte' | 'SByte' | 'UShort' | 'Short' | 'UInteger'
    | 'Integer' | 'ULong' | 'Long'
    ;

FloatingPointTypeName
    : 'Single' | 'Double'
    ;
```


由于是基元类型的别名是常规类型，每个基元类型具有成员。 例如，`Integer`具有中声明成员`System.Int32`。 文字可以视为其相应类型的实例。

* 基元类型不同于其他结构类型，不允许某些附加的操作：

* 基元类型允许使用值来创建的编写文本。 例如，`123I`是类型的文字`Integer`。

* 它是可以声明基元类型的常量。

* 所有基元类型常量表达式的操作数时，就可以为编译器在编译时表达式进行求值。 此类表达式被称为常量表达式。

Visual Basic 定义以下基元类型：

* 整数值类型`Byte`（1 字节无符号的整数）， `SByte` （1 字节有符号的整数）， `UShort` （2 字节无符号的整数）， `Short` （2 字节有符号的整数）， `UInteger` （4 字节无符号的整数）， `Integer` (4 字节有符号的整数)， `ULong` （8 字节无符号的整数） 和`Long`（8 字节有符号的整数）。 这些类型将映射到`System.Byte`， `System.SByte`， `System.UInt16`， `System.Int16`， `System.UInt32`， `System.Int32`，`System.UInt64`和`System.Int64`分别。 一种整型类型的默认值相当于文字`0`。

* 浮点值类型`Single`（4 字节浮点数） 和`Double`（8 字节浮点数）。 这些类型将映射到`System.Single`和`System.Double`分别。 浮点类型的默认值相当于文字`0`。

* `Decimal`类型 （16 字节十进制值） 映射到`System.Decimal`。 默认值的小数位数相当于文字`0D`。

* `Boolean`值类型，表示真值，通常关系或逻辑运算的结果。 文本属于类型`System.Boolean`。 默认值`Boolean`类型等效于文字`False`。

* `Date`值类型，用于表示日期和/或时间，并将映射到`System.DateTime`。 默认值`Date`类型等效于文字`# 01/01/0001 12:00:00AM #`。

* `Char`值类型，用于表示单个 Unicode 字符，并将映射到`System.Char`。 默认值`Char`类型等效于常量的表达式`ChrW(0)`。

* `String`引用类型，用于表示 Unicode 字符序列，并将映射到`System.String`。 默认值`String`类型是一个 null 值。



## <a name="enumerations"></a>枚举

*枚举*是继承的值类型`System.Enum`和符号表示一组基元整型类型之一的值。

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

对于枚举类型`E`，默认值是由表达式生成的值`CType(0, E)`。

枚举的基础类型必须是一种整型类型，可以表示枚举中定义的所有枚举器值。 如果指定的基础类型，则它必须是`Byte`， `SByte`， `UShort`， `Short`， `UInteger`， `Integer`， `ULong`， `Long`，或在其相应的类型之一`System`命名空间。 如果显式不指定任何基础类型，默认值是`Integer`。

下面的示例声明了一个基础类型的枚举`Long`:

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

开发人员可以选择使用的基础类型`Long`，如下所示的示例中，若要启用的范围内的值的用法`Long`，但不是在的范围内`Integer`，还是保留此选项可在将来。


### <a name="enumeration-members"></a>枚举成员

枚举的成员是在枚举中声明的枚举的值和从类继承的成员`System.Enum`。

枚举成员的作用域是枚举声明主体。 这意味着，外部枚举声明，枚举成员始终必须限定 （除非该类型是专门导入通过命名空间导入的命名空间）。

当省略常数表达式的值时，枚举成员声明的声明顺序非常重要。 枚举的成员隐式具有`Public`仅访问; 没有访问修饰符允许对枚举成员声明。

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a>枚举值

枚举成员列表中的枚举的值被声明为常量类型为基础的枚举类型，并且它们可以出现常数的所需的任何位置。 一个枚举成员定义`=`为关联的成员提供了所指示的常量表达式的值。 常数表达式的计算结果必须为整型类型的隐式转换为基础类型，而且必须在基础类型可以表示的值的范围内。 下面的示例是在错误，因为常量值`1.5`， `2.3`，并`3.3`不是隐式转换为的基础整型类型`Long`具有严格的语义。

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

多个枚举成员可能会共享相同的关联的值，如下所示：

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

该示例演示具有两个枚举成员-枚举`Blue`和`Max`-，具有相同的关联值。

如果第一个枚举器值定义枚举中的没有初始值设定项，相应的常量的值是`0`。 没有初始值设定项的枚举值定义为枚举器提供了通过增加由以前的枚举值的值获取的值`1`。 增加的该值必须在基础类型可以表示的值的范围内。

```vb
Enum Color
    Red
    Green = 10
    Blue
End Enum 

Module Test
    Sub Main()
        Console.WriteLine(StringFromColor(Color.Red))
        Console.WriteLine(StringFromColor(Color.Green))
        Console.WriteLine(StringFromColor(Color.Blue))
    End Sub

    Function StringFromColor(c As Color) As String
        Select Case c
            Case Color.Red
                Return String.Format("Red = " & CInt(c))

            Case Color.Green
                Return String.Format("Green = " & CInt(c))

            Case Color.Blue
                Return String.Format("Blue = " & CInt(c))

            Case Else
                Return "Invalid color"
        End Select
    End Function
End Module
```

上面的示例输出的枚举值和相关联的值。 输出为：

```
Red = 0
Green = 10
Blue = 11
```

值的原因是，如下所示：

* 枚举值`Red`自动分配值`0`（因为它没有初始值设定项，并且是第一个枚举值成员）。

* 枚举值`Green`显式提供了值`10`。

* 枚举值`Blue`自动分配的值大于文本上位于前面的枚举值之一。

常量表达式可能会不直接或间接使用其自身关联的枚举值的值 （即，不允许在常量表达式中的循环）。 下面的示例是无效因为的声明`A`和`B`是循环的。

```vb
Enum Circular
    A = B
    B
End Enum
```

`A` 取决于`B`显式，并且`B`取决于`A`隐式。

## <a name="classes"></a>类

一个*类*是可能包含数据成员 （常量、 变量和事件）、 函数成员 （方法、 属性、 索引器、 运算符和构造函数） 和嵌套的类型的数据结构。 类是引用类型。

```antlr
ClassDeclaration
    : Attributes? ClassModifier* 'Class' Identifier TypeParameterList? StatementTerminator
      ClassBase?
      TypeImplementsClause*
      ClassMemberDeclaration*
      'End' 'Class' StatementTerminator
    ;

ClassModifier
    : TypeModifier
    | 'MustInherit'
    | 'NotInheritable'
    | 'Partial'
    ;
```

下面的示例显示了包含每个类型的成员的类：

```vb
Class AClass
    Public Sub New()
        Console.WriteLine("Constructor")
    End Sub

    Public Sub New(value As Integer)
        MyVariable = value
        Console.WriteLine("Constructor")
    End Sub

    Public Const MyConst As Integer = 12
    Public MyVariable As Integer = 34

    Public Sub MyMethod()
        Console.WriteLine("MyClass.MyMethod")
    End Sub

    Public Property MyProperty() As Integer
        Get
            Return MyVariable
        End Get

        Set (value As Integer)
            MyVariable = value
        End Set
    End Property

    Default Public Property Item(index As Integer) As Integer
        Get
            Return 0
        End Get

        Set (value As Integer)
            Console.WriteLine("Item(" & index & ") = " & value)
        End Set
    End Property

    Public Event MyEvent()

    Friend Class MyNestedClass
    End Class 
End Class
```

下面的示例显示了这些成员的使用：

```vb
Module Test

    ' Event usage.
    Dim WithEvents aInstance As AClass

    Sub Main()
        ' Constructor usage.
        Dim a As AClass = New AClass()
        Dim b As AClass = New AClass(123)

        ' Constant usage.
        Console.WriteLine("MyConst = " & AClass.MyConst)

        ' Variable usage.
        a.MyVariable += 1
        Console.WriteLine("a.MyVariable = " & a.MyVariable)

        ' Method usage.
        a.MyMethod()

        ' Property usage.
        a.MyProperty += 1
        Console.WriteLine("a.MyProperty = " & a.MyProperty)
        a(1) = 1

        ' Event usage.
        aInstance = a
    End Sub 

    Sub MyHandler() Handles aInstance.MyEvent
        Console.WriteLine("Test.MyHandler")
    End Sub 
End Module
```

有两个类专用的修饰符，`MustInherit`和`NotInheritable`。 若要指定这两个无效。


### <a name="class-base-specification"></a>类的基规范

类声明可以包含定义类的直接基类型的基类型规范。

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

如果类声明没有显式的基类型，则直接基类型是隐式`Object`。 例如：

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

基类型不能为其自身的、 上的类型参数，尽管它可能涉及到作用域中的类型参数。

```vb
Class C1(Of V) 
End Class

Class C2(Of V)
    Inherits V    ' Error, type parameter used as base class 
End Class

Class C3(Of V)
    Inherits C1(Of V)    ' OK: not directly inheriting from V.
End Class
```

只能从派生类可能`Object`和类。 是无效的类进行派生`System.ValueType`， `System.Enum`， `System.Array`，`System.MulticastDelegate`或`System.Delegate`。 泛型类不能从派生`System.Attribute`或从其派生的类。

每个类具有一个直接基类，并禁止循环中派生。 不能从派生`NotInheritable`类和基类的可访问域必须与相同或类本身的可访问域的超集。


### <a name="class-members"></a>类成员

类的成员由其类成员声明引入的成员和从其直接基类继承的成员组成。

```antlr
ClassMemberDeclaration
    : NonModuleDeclaration
    | EventMemberDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

类成员声明可能具有`Public`， `Protected`， `Friend`， `Protected Friend`，或`Private`访问。 如果类成员声明不包括访问修饰符，声明将默认为`Public`访问权限，除非它是变量的声明; 在这种情况下它将默认为`Private`访问。

类成员的作用域是类的主体中的成员声明发生，以及此类的约束列表 （如果它是泛型，并具有约束）。 如果成员具有`Friend`访问权限，其范围扩展到在同一程序中的任何派生的类或它已被授予的任何程序集的类主体`Friend`访问权限，并且如果成员具有`Public`， `Protected`，或`Protected Friend`访问，其作用域将扩展到任何程序中任何派生类的类的主体。


## <a name="structures"></a>结构

*结构*是继承的值类型`System.ValueType`。 结构与类很相似，因为它们表示可以包含数据成员和函数成员的数据结构。 与类不同，但是，结构不需要堆分配。

```antlr
StructureDeclaration
    : Attributes? StructureModifier* 'Structure' Identifier
      TypeParameterList? StatementTerminator
      TypeImplementsClause*
      StructMemberDeclaration*
      'End' 'Structure' StatementTerminator
    ;

StructureModifier
    : TypeModifier
    | 'Partial'
    ;
```

对于类，它是两个变量来引用同一对象，并因此可能会对一个变量以影响其他变量所引用的对象的操作。 对于结构，每个变量都有其自己副本非`Shared`数据，因此它不可能实现的操作的另一个用于会影响其他，如以下示例所示：

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

给定以下代码输出的值的上述声明`10`:

```vb
Module Test
    Sub Main()
        Dim a As New Point(10, 10)
        Dim b As Point = a

        a.x = 100
        Console.WriteLine(b.x)
    End Sub
End Module
```

分配`a`到`b`会创建一份值，并`b`因此不受分配到`a.x`。 有`Point`改为已声明为一个类，则输出将为`100`因为`a`和`b`将引用同一对象。


### <a name="structure-members"></a>结构成员

一种结构的成员是其结构成员声明引入的成员和成员继承自`System.ValueType`。

```antlr
StructMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    | OperatorDeclaration
    ;
```

每个结构都隐式具有`Public`生成结构的默认值的无参数实例构造函数。 因此，不能为结构类型声明可以声明一个无参数实例构造函数。 结构类型，但是，允许以声明*参数化*实例构造函数，如以下示例所示：

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

给定上述声明，以下语句都创建`Point`与`x`和`y`初始化为零。

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

由于结构直接包含其字段值 （而非对这些值的引用），结构不能包含直接或间接地引用其自身的字段。 例如，下面的代码不是有效的：

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

通常情况下，只能有一个结构成员声明`Public`， `Friend`，或`Private`访问权限，但当重写成员继承自`Object`，`Protected`和`Protected Friend`访问，也可以使用。 结构成员声明中不包括访问修饰符，声明将默认为`Public`访问。 声明为结构成员的作用域是结构正文在其中进行声明，加上该结构的约束 （如果它是泛型类型和有约束）。


## <a name="standard-modules"></a>标准模块

一个*标准模块*是一种类型的成员是隐式`Shared`且范围限定到标准模块包含的命名空间声明空间而不是只需标准模块声明本身。 标准模块可能永远不会实例化。 它是错误声明的变量的标准模块类型。

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

标准模块的成员具有两个完全限定的名称，一个没有标准的模块名称，另一个具有标准模块名称。 多个命名空间中的标准模块可以定义特定的名称; 的成员对任一模块的外部名称的非限定的引用不明确。 例如：

```vb
Namespace N1
    Module M1
        Sub S1()
        End Sub

        Sub S2()
        End Sub
    End Module

    Module M2
        Sub S2()
        End Sub
    End Module

    Module M3
        Sub Main()
            S1()       ' Valid: Calls N1.M1.S1.
            N1.S1()    ' Valid: Calls N1.M1.S1.
            S2()       ' Not valid: ambiguous.
            N1.S2()    ' Not valid: ambiguous.
            N1.M2.S2() ' Valid: Calls N1.M2.S2.
        End Sub
    End Module
End Namespace
```

模块可能只声明了命名空间中，可能不会嵌套在另一种类型。 标准模块不能实现接口，它们隐式派生`Object`，并且仅具有`Shared`构造函数。


### <a name="standard-module-members"></a>标准模块成员

标准模块的成员是其成员声明引入的成员和成员继承自`Object`。 标准模块可能会有任何类型的实例构造函数除外的成员。 所有标准模块类型成员都是隐式`Shared`。

```antlr
ModuleMemberDeclaration
    : NonModuleDeclaration
    | VariableMemberDeclaration
    | ConstantMemberDeclaration
    | EventMemberDeclaration
    | MethodMemberDeclaration
    | PropertyMemberDeclaration
    | ConstructorMemberDeclaration
    ;
```

通常情况下，只能有一个标准的模块成员声明`Public`， `Friend`，或`Private`访问权限，但当重写成员继承自`Object`，则`Protected`和`Protected Friend`可能指定访问修饰符。 如果标准模块成员声明中不包括访问修饰符，声明将默认为`Public`访问，除非它是变量，默认为`Private`访问。

如上文所述，标准模块成员的作用域是包含标准模块声明的声明。 成员继承自`Object`未包含在此特殊作用域; 这些成员具有无作用域，始终必须使用的模块的名称限定。 如果成员具有`Friend`仅对在同一个程序或程序集的具有给定中声明的命名空间成员的访问权限，其作用域扩展`Friend`访问。


## <a name="interfaces"></a>接口

*接口*是引用类型的其他类型实现此方法可以保证它们支持某些方法。 永远不会直接创建一个接口，并没有实际的表示形式--其他类型必须转换为接口类型。 接口定义一个协定。 类或结构实现的接口必须遵守其协定。

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


下面的示例演示包含默认属性的接口`Item`，事件`E`，一种方法`F`，和一个属性`P`:

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

接口可以采用多个继承。 在下面的示例中，该接口`IComboBox`同时继承`ITextBox`和`IListBox`:

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

类和结构可以实现多个接口。 在下面的示例中，类`EditBox`派生自类`Control`并同时实现`IControl`和`IDataBound`:

```vb
Interface IDataBound
    Sub Bind(b As Binder)
End Interface 

Public Class EditBox
    Inherits Control
    Implements IControl, IDataBound

    Public Sub Paint() Implements IControl.Paint
        ...
    End Sub

    Public Sub Bind(b As Binder) Implements IDataBound.Bind
        ...
    End Sub
End Class
```


### <a name="interface-inheritance"></a>接口继承

接口的基接口是显式的基接口和它们的基接口。 换而言之，组的基接口是完全的显式基接口，其显式基接口，诸如此类的可传递闭包。 如果接口声明具有没有显式基接口，则该类型没有基接口-接口不继承自`Object`(尽管它们具有扩大转换为`Object`)。

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

在下面的示例中，接口的基`IComboBox`都`IControl`， `ITextBox`，和`IListBox`。

```vb
Interface IControl
    Sub Paint()
End Interface 

Interface ITextBox
    Inherits IControl

    Sub SetText(text As String)
End Interface 

Interface IListBox
    Inherits IControl

    Sub SetItems(items() As String)
End Interface 

Interface IComboBox
    Inherits ITextBox, IListBox 
End Interface
```

接口继承其基接口的所有成员。 换而言之，`IComboBox`上述接口继承的成员`SetText`并`SetItems`以及`Paint`。

由类或结构，它还将隐式实现接口实现的所有接口的基接口。

如果接口的基接口的传递闭包中多次出现，它仅分配给派生接口及其成员一次。 类型实现派生的接口仅有以实现 multiply 方法定义一次基接口。 在以下示例中，`Paint`只需实现一次，即使类实现`IComboBox`和`IControl`。

```vb
Class ComboBox
    Implements IControl, IComboBox

    Sub SetText(text As String) Implements IComboBox.SetText
    End Sub

    Sub SetItems(items() As String) Implements IComboBox.SetItems
    End Sub

    Sub Print() Implements IComboBox.Paint
    End Sub
End Class
```

`Inherits`子句没有任何影响其他`Inherits`子句。 在以下示例中，`IDerived`必须限定的名称`INested`与`IBase`。

```vb
Interface IBase
    Interface INested
        Sub Nested()
    End Interface

    Sub Base()
End Interface

Interface IDerived
    Inherits IBase, INested   ' Error: Must specify IBase.INested.
End Interface
```

基接口的可访问域必须与相同或接口本身的可访问域的超集。


### <a name="interface-members"></a>接口成员

接口的成员包括其成员声明引入的成员和从其基接口继承的成员。

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

尽管接口不会继承中的成员`Object`，因为每个类或结构实现接口 does 继承`Object`的成员`Object`，其中包括扩展方法、 被视为一个接口的成员并且可以直接而无需强制转换为在接口上调用`Object`。 例如：

```vb
Interface I1
End Interface

Class C1
    Implements I1
End Class

Module Test
    Sub Main()
        Dim i As I1 = New C1()
        Dim h As Integer = i.GetHashCode()
    End Sub
End Module
```

具有相同名称的接口的成员的成员作为`Object`隐式阴影`Object`成员。 只有嵌套类型、方法、属性和事件才能作为接口成员。 方法和属性可能不具有一个主体。 接口成员为隐式`Public`和不能指定访问修饰符。 在接口中声明的作用域是成员的接口正文在其中进行声明，以及该接口的约束列表 （如果它是成员的泛型，并具有约束）。


## <a name="arrays"></a>数组

*数组*是引用类型，其中包含通过访问变量*索引*数组中的变量的顺序一对一的方式相对应。 一个数组中包含的变量也称为*元素*的数组，所有的必须是相同的类型，并且这种类型称为*元素类型*的数组。

```antlr
ArrayTypeName
    : NonArrayTypeName ArrayTypeModifiers
    ;

ArrayTypeModifiers
    : ArrayTypeModifier+
    ;

ArrayTypeModifier
    : OpenParenthesis RankList? CloseParenthesis
    ;

RankList
    : Comma*
    ;

ArrayNameModifier
    : ArrayTypeModifiers
    | ArraySizeInitializationModifier
    ;
```

数组的元素创建数组实例后，就应该考虑存在并在销毁数组实例时停止存在。 数组的每个元素初始化为其类型的默认值。 类型`System.Array`是所有数组类型的基类型，不可能实例化。 每个数组类型继承的成员来声明`System.Array`类型，并且转换为它 (和`Object`)。 一维数组类型与元素`T`还实现了接口`System.Collections.Generic.IList(Of T)`并`IReadOnlyList(Of T)`; 如果`T`为引用类型，则数组类型还实现`IList(Of U)`和`IReadOnlyList(Of U)`为任何`U`，其引用从转换扩大`T`。

数组具有*排名*，它确定与每个数组元素相关联的索引数。 数组的秩确定的数量*维度*的数组。 例如，具有一个级别的数组称为一维数组，，具有秩大于 1 的数组称为多维数组。

下面的示例创建整数值的一维数组，初始化数组元素，然后输出每个出：

```vb
Module Test
    Sub Main()
        Dim arr(5) As Integer
        Dim i As Integer

        For i = 0 To arr.Length - 1
            arr(i) = i * i
        Next i

        For i = 0 To arr.Length - 1
            Console.WriteLine("arr(" & i & ") = " & arr(i))
        Next i
    End Sub
End Module
```

程序输出结果如下：

```
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

数组的每个维度具有一个关联的长度。 维的长度不是数组，该类型的一部分，但在运行时创建的数组类型的实例时而建立。 维度的长度决定了该维度的索引的有效范围： 维度的长度`N`，索引可以介于 0 到`N-1`。 如果维度的长度为零，没有该维度的有效索引。 数组中元素的总数是数组中每个维度的长度的产品。 如果任何数组的维度的长度为零，则称该数组为空。 数组的元素类型可以是任何类型。

通过将修饰符添加到现有的类型名称指定数组类型。 修饰符包含左的括号，零个或多个逗号，一组和右括号。 修改的类型是数组的元素类型和维度的数目是逗号加 1 的数。 如果指定多个修饰符，则数组的元素类型是一个数组。 修饰符是左到右读取，使用最左侧的修饰符是最外层的数组。 在示例

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

元素类型`arr`是一个三维数组的一维数组的元素的二维数组`Integer`。

此外可能会将变量声明为数组类型的通过将数组的类型修饰符或数组大小初始化修饰符放在变量名称。 在这种情况下，数组元素类型是在声明中，给定的类型和数组维度由变量名称修饰符。 为清楚起见，不能用来在同一声明中有数组类型修饰符的变量名称和类型名称。

下面的示例演示使用具有数组类型的局部变量声明的各种`Integer`与元素类型：

```vb
Module Test
    Sub Main()
        Dim a1() As Integer    ' Declares 1-dimensional array of integers.
        Dim a2(,) As Integer   ' Declares 2-dimensional array of integers.
        Dim a3(,,) As Integer  ' Declares 3-dimensional array of integers.

        Dim a4 As Integer()    ' Declares 1-dimensional array of integers.
        Dim a5 As Integer(,)   ' Declares 2-dimensional array of integers.
        Dim a6 As Integer(,,)  ' Declares 3-dimensional array of integers.

        ' Declare 1-dimensional array of 2-dimensional arrays of integers 
        Dim a7()(,) As Integer
        ' Declare 2-dimensional array of 1-dimensional arrays of integers.
        Dim a8(,)() As Integer

        Dim a9() As Integer() ' Not allowed.
    End Sub
End Module
```

数组类型名称修饰符将扩展到其后面的括号内的所有集。 这意味着，在一组参数括在括号中允许类型名称之后的位置的情况下，它不能指定为数组的类型名称的参数。 例如：

```vb
Module Test
    Sub Main()
        ' This calls the Integer constructor.
        Dim x As New Integer(3)

        ' This declares a variable of Integer().
        Dim y As Integer()

        ' This gives an error.
        ' Array sizes can not be specified in a type name.
        Dim z As Integer()(3)
    End Sub
End Module
```

在最后一种情况下，`(3)`被解释为类型名称的一部分而不是一组构造函数参数。


## <a name="delegates"></a>委托

一个*委派*是一个引用类型，是指`Shared`方法的类型或实例方法的对象。

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 最接近的其他语言中的委托是函数指针，而只能引用函数指针，但`Shared`函数，委托可以引用同时`Shared`和实例方法。 在后一种情况下，委托不仅存储存储的引用方法的入口点，还对用来调用该方法的对象实例的引用。

委托声明可能没有`Handles`子句中，`Implements`子句中，方法体中，或`End`构造。 委托声明的参数列表不能包含`Optional`或`ParamArray`参数。 返回类型和参数类型的可访问域必须与相同或委托本身的可访问域的超集。

委托的成员都是从类继承的成员`System.Delegate`。 委托还定义了以下方法：

* 采用两个参数，一个类型的构造函数`Object`，另一个类型`System.IntPtr`。

* `Invoke`具有相同的签名与委托的方法。

* 一个`BeginInvoke`方法的签名的委托签名，有三个差异。 首先，将返回类型更改为`System.IAsyncResult`。 其次，两个附加参数添加到参数列表的末尾： 类型的第一个`System.AsyncCallback`类型，第二个`Object`。 最后，所有`ByRef`参数已发生更改，为`ByVal`。

* `EndInvoke`其返回类型是与委托相同的方法。 该方法的参数是仅的委托参数，这一点是`ByRef`参数，它们在委托签名中出现的顺序相同。  除了这些参数，还有另一个类型参数`System.IAsyncResult`参数列表的末尾。

有三个步骤中定义和使用委托： 声明、 实例化和调用。

使用委托声明语法声明委托。 下面的示例声明名为的委托`SimpleDelegate`不采用任何参数：

```vb
Delegate Sub SimpleDelegate()
```

下一个示例创建`SimpleDelegate`实例，然后立即调用：

```vb
Module Test
    Sub F()
        System.Console.WriteLine("Test.F")
    End Sub 

    Sub Main()
        Dim d As SimpleDelegate = AddressOf F
        d()
    End Sub 
End Module
```

存在不太多点中实例化委托的方法，然后立即调用通过委托，因为它将直接调用该方法更简单。 委托的匿名使用时显示它们的有效性。 下面的示例说明`MultiCall`反复调用的方法`SimpleDelegate`实例：

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

它对并不重要`MultiCall`方法目标方法的`SimpleDelegate`是，哪些辅助功能具有此方法，或该方法是否是`Shared`或不。 最重要的就是目标方法的签名是与兼容`SimpleDelegate`。


## <a name="partial-types"></a>分部类型

类和结构声明可*分部*声明。 分部声明可能会或可能不充分描述了在声明中声明的类型。 相反，类型的声明可能分布在多个分部声明中程序;不能跨程序边界声明分部类型。 分部类型声明指定`Partial`声明上的修饰符。 然后，将在编译时以形成单个的类型声明的分部声明以及合并类型具有相同的完全限定名称的程序中的任何其他声明。 例如，下面的代码声明一个类`Test`与成员`Test.C1`和`Test.C2`。

a.vb:

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

b.vb:

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

当结合使用分部类型声明，在至少其中一个声明必须具有`Partial`修饰符，否则编译时错误结果。

__请注意。__ 但也可以指定`Partial`上只有一个声明之间数量的分部声明中，它是更好的窗体以所有分部声明中指定它。 在一个分部声明可见但一个或多个分部声明隐藏 （如扩展工具生成的代码的情况下） 情况下，则可以保留`Partial`从可见声明修饰符但中指定它隐藏的声明。

只有类和结构可以使用分部声明声明。 分部声明一起进行匹配时，被视为一种类型的实参数量： 不被视为具有相同名称但不同数量的类型参数的两个类是在同一时间的分部声明。 分部声明可以指定属性、 类修饰符`Inherits`语句或`Implements`语句。 在编译时，所有分部声明的各个部分组合在一起，使用的类型声明的一部分。 如果属性，修饰符，之间存在任何冲突基础，接口或类型成员的编译时错误结果。 例如：

```vb
Public Partial Class Test1
    Implements IDisposable
End Class

Class Test1
    Inherits Object
    Implements IComparable
End Class

Public Partial Class Test2
End Class

Private Partial Class Test2
End Class
```

前面的示例声明的类型`Test1`也就是说`Public`，继承自`Object`并实现`System.IDisposable`和`System.IComparable`。 分部声明`Test2`将导致编译时错误，因为其中一个声明指出`Test2`是`Public`和另一个指出`Test2`是`Private`。

使用类型参数的分部类型可以声明约束和方差的类型参数，但约束和方差从每个分部声明必须匹配。 因此，约束和方差比较特殊，因为不会自动合并等其他修饰符：

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

使用多个分部声明声明一个类型这一事实并不影响类型中的名称查找规则。 因此，分部类型声明可以使用中的其他分部类型声明，声明的成员，或可能在其他分部类型声明中声明的接口上实现方法。 例如：

```vb
Public Partial Class Test1
    Implements IDisposable

    Private IsDisposed As Boolean = False
End Class

Class Test1
    Private Sub Dispose() Implements IDisposable.Dispose
        If Not IsDisposed Then
            ...
        End If
    End Sub
End Class
```

嵌套的类型可以有也分部声明。 例如：

```vb
Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S1()
        End Sub
    End Class
End Class

Public Partial Class Test
    Public Partial Class NestedTest
        Public Sub S2()
        End Sub
    End Class
End Class
```

初始值设定项中的分部声明仍将按声明顺序; 继续执行但是，没有初始值设定项在单独的分部声明中发生的执行顺序并未保证。

## <a name="constructed-types"></a>构造的类型

泛型类型声明，其本身而言，不代表一种类型。 相反，泛型类型声明可以使用作为"蓝图"，以通过应用类型参数构成许多不同类型。 具有应用于它的类型参数的泛型类型称为*构造类型*。 在构造类型中的类型参数必须始终满足放置到匹配的类型形参的约束。

类型名称可以确定一个构造的类型，即使它不直接指定类型参数。 这可能发生其中一种类型嵌套在泛型类声明中，并且包含声明的实例类型隐式用于名称查找：

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

构造的类型`C(Of T1,...,Tn)`可访问的泛型类型和所有类型参数时，才可访问。 例如，如果泛型类型`C`是`Public`和类型参数的所有`T1,...,Tn`是`Public`，然后构造的类型是`Public`。 如果类型名称或类型参数之一`Private`，但是，则构造类型的可访问性是`Private`。 如果一个类型参数的构造类型是`Protected`和另一个类型参数是`Friend`，然后构造的类型是只能在类，并在此程序集或已被授予任何程序集及其子类中访问`Friend`访问。 换而言之，构造类型的可访问域是其组成部分的可访问性域的交集。

__请注意。__ 构造类型的可访问域是其有序的各个部分的交集的事实具有定义新的可访问性级别的有趣的副作用。 包含的元素，是一个构造的类型`Protected`的元素，并`Friend`只能在可以访问的上下文中访问*两者* `Friend` *和* `Protected`成员。 但是，没有办法表达中的语言，可访问性为此可访问性级别`Protected Friend`意味着可以在可访问的上下文中访问实体*任一* `Friend` *或*`Protected`成员。

由类型参数的泛型类型的每个匹配项在提供的类型实参替换决定的基实现的接口和构造类型的成员。

### <a name="open-types-and-closed-types"></a>开放类型和封闭的类型

的一个或多个类型参数是包含类型或方法的类型参数的构造的类型称为*打开类型*。 这是因为某些类型的类型参数仍然不知道，因此该类型的实际形状还不完全知道。 与此相反，该类型形参的实参是所有的非类型参数的泛型类型称为*封闭类型*。 始终完全已知的封闭类型的形状。 例如：

```vb
Class Base(Of T, V)
End Class

Class Derived(Of V)
    Inherits Base(Of Integer, V)
End Class

Class MoreDerived
    Inherits Derived(Of Double)
End Class
```

构造的类型`Base(Of Integer, V)`是开放类型，因为尽管类型参数`T`未提供类型参数`U`已被所提供的另一个类型参数。 因此，完整类型的形状还不知道。 构造的类型`Derived(Of Double)`，但是，因为提供的继承层次结构中的所有类型参数是关闭的类型。

打开类型定义，如下所示：

* 类型参数是开放类型。

* 数组类型是开放类型，其元素类型是否为开放类型。

* 构造的类型是开放式类型; 如果一个或多个其类型参数为开放类型。

* 关闭的类型是不是开放类型的类型。

由于程序入口点不能为泛型类型中，在运行时使用的所有类型都将封闭的类型。

## <a name="special-types"></a>特殊类型

.NET Framework 包含多个将被视为特殊由.NET Framework 和 Visual Basic 语言的类：

类型`System.Void`，它表示在.NET Framework 中，void 类型，可以直接引用仅在`GetType`表达式。

类型`System.RuntimeArgumentHandle`，`System.ArgIterator`和`System.TypedReference`所有可以包含的指针到堆栈，因此不能出现在.NET Framework 堆上。 因此，它们不能用作数组元素类型、 返回类型、 字段类型、 泛型类型参数，可以为 null 的类型`ByRef`参数类型和值转换为类型`Object`或`System.ValueType`，到实例调用的目标成员`Object`或`System.ValueType`，或提升到闭包。
