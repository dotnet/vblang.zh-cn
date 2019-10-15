---
ms.openlocfilehash: 4560eac9f4ab52d07c77724aeca696d0195da91a
ms.sourcegitcommit: 0e8c2550c052934e02defb6d6eb9f322e061b674
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2019
ms.locfileid: "72306024"
---
# <a name="types"></a>类型

Visual Basic 中的类型的两个基本类别为*值类型*和*引用类型*。 基元类型（字符串除外）、枚举和结构是值类型。 类、字符串、标准模块、接口、数组和委托是引用类型。

每个类型都有一个*默认值*，该值是在初始化时分配给该类型的变量的值。

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

尽管值类型和引用类型在声明语法和用法方面可能类似，但它们的语义不同。

引用类型存储在运行时堆上;它们只能通过对该存储的引用进行访问。 由于始终通过引用访问引用类型，因此，它们的生存期由 .NET Framework 管理。 将跟踪对特定实例的未完成引用，并且仅当不再有引用保留时，才销毁实例。 引用类型的变量包含对该类型的值、更派生的类型的值或 null 值的引用。 *空值*表示不是;不能使用 null 值执行任何操作，除非赋值。 对引用类型的变量赋值会创建引用的副本，而不是引用的值的副本。 对于引用类型的变量，默认值为 null 值。

值类型直接存储在堆栈中或另一类型内;只能直接访问其存储。 由于值类型直接存储在变量中，因此它们的生存期由包含它们的变量的生存期确定。 当包含值类型实例的位置被销毁时，还会销毁值类型实例。 始终直接访问值类型;不能创建对值类型的引用。 禁止此类引用无法引用已销毁的值类实例。 由于值类型始终 `NotInheritable`，因此值类型的变量始终包含该类型的值。 因此，值类型的值不能为 null 值，也不能引用派生程度更高的类型的对象。 对值类型的变量赋值会创建要分配的值的副本。 对于值类型的变量，默认值为将类型的每个变量成员初始化为其默认值的结果。

下面的示例演示引用类型和值类型之间的差异：

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

```console
Values: 0, 123
Refs: 123, 123
```

对局部变量 `val2` 的赋值不会影响 `val1` 的局部变量，因为两个局部变量都是值类型（类型 `Integer`），并且值类型的每个局部变量都有其自己的存储。 与此相反，赋值 `ref2.Value = 123;` 会影响 `ref1` 和 @no__t 2 引用的对象。

关于 .NET Framework 类型系统，需要注意的一点是，尽管结构、枚举和基元类型（除 @no__t 以外）是值类型，但它们都是从引用类型继承的。 结构和基元类型继承自引用类型 `System.ValueType`，后者继承自 @no__t。 枚举的类型继承自引用类型 `System.Enum`，后者继承自 @no__t。

### <a name="nullable-value-types"></a>可以为 Null 的值类型

对于值类型，可以将 `?` 修饰符添加到类型名称，以表示该类型的*可以为 null*的版本。

```antlr
NullableTypeName
    : NonArrayTypeName '?'
    ;

NullableNameModifier
    : '?'
    ;
```

可以为 null 的值类型可以包含与类型的不可为 null 的版本相同的值，也可以包含 null 值。 因此，对于可以为 null 的值类型，将 @no__t 0 分配给类型的变量会将变量的值设置为 null 值，而不是值类型的零值。 例如：

```vb
Dim x As Integer = Nothing
Dim y As Integer? = Nothing

' Prints zero
Console.WriteLine(x)
' Prints nothing (because the value of y is the null value)
Console.WriteLine(y)
```

还可以通过将可为 null 的类型修饰符放在变量名上，将变量声明为可以为 null 的值类型。 为清楚起见，对同一声明中的变量名称和类型名称都有可以为 null 的类型修饰符是无效的。 由于可以使用类型 `System.Nullable(Of T)` 实现可为 null 的类型，因此，类型 `T?` 与类型 `System.Nullable(Of T)` 的同义词，这两个名称可互换使用。 不能将 `?` 修饰符放置在可以为 null 的类型上;因此，不能声明类型 `Integer??` 或 `System.Nullable(Of Integer)?`。

可以为 null 的值类型 `T?`，以及从基础类型 `T`*提升*为类型 `T?` 的任何运算符或转换的 @no__t 成员。 从基础类型中抬起复制运算符和转换，在大多数情况下，会将可为 null 的值类型替换为不可为 null 的值类型。 这样，就可以将应用到 `T` 的许多相同的转换和操作也应用于 `T?`。


## <a name="interface-implementation"></a>接口实现

结构和类声明可以声明它们通过一个或多个 `Implements` 子句实现一组接口类型。

```antlr
TypeImplementsClause
    : 'Implements' TypeImplements StatementTerminator
    ;

TypeImplements
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

@No__t-0 子句中指定的所有类型都必须是接口，并且该类型必须实现接口的所有成员。 例如：

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

实现接口的类型还隐式实现了接口的所有基接口。 即使类型未在 `Implements` 子句中显式列出所有基接口，也是如此。 在此示例中，`TextBox` 结构同时实现 @no__t 和 @no__t。

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

声明某个类型在和本身实现接口时，不会在该类型的声明空间中声明任何内容。 因此，使用具有相同名称的方法实现两个接口是有效的。

类型本身不能实现类型参数，但它可能涉及范围内的类型参数。

```vb
Class C1(Of V)
    Implements V  ' Error, can't implement type parameter directly
    Implements IEnumerable(Of V)  ' OK, not directly implementing

    ...
End Class
```

泛型接口可使用不同的类型参数实现多次。 但是，如果提供的类型参数（无论类型约束如何）与该接口的另一实现重叠，则泛型类型不能使用类型参数实现泛型接口。 例如：

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

*基元类型*通过关键字标识，关键字是 `System` 命名空间中预定义类型的别名。 基元类型与它的别名类型完全不同：编写保留字 `Byte` 与写入 @no__t 完全相同。 基元类型也称为 "*内部类型*"。

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


由于基元类型会将常规类型作为别名，因此每个基元类型都具有成员。 例如，`Integer` @no__t 中声明的成员。 可以将文本视为其对应类型的实例。

* 基元类型不同于其他结构类型，因为它们允许执行某些其他操作：

* 基元类型允许通过编写文本来创建值。 例如，@no__t 为 `Integer` 类型的文本。

* 可以声明基元类型的常量。

* 如果表达式的操作数均为基元类型常量，则编译器可能会在编译时计算表达式。 此类表达式称为常量表达式。

Visual Basic 定义以下基元类型：

* 整数值类型 `Byte` （1字节无符号整数），`SByte` （1字节有符号整数），`UShort` （2字节无符号整数），`Short` （2字节有符号整数），`UInteger` （4字节无符号整数），`ULong` （8字节无符号整数）和 @no__t 7 （8字节有符号整数）。 这些类型分别映射到 `System.Byte`、`System.SByte`、`System.UInt16`、`System.Int16`、@no__t、@no__t、@no__t 和 @no__t。 整数类型的默认值等效于文本 `0`。

* 浮点值类型 `Single` （4字节浮点数）和 `Double` （8字节浮点）。 这些类型分别映射到 `System.Single` 和 @no__t 1。 浮点类型的默认值等效于文本 `0`。

* @No__t 0 类型（16字节小数值），映射到 `System.Decimal`。 默认值 decimal 等效于文本 `0D`。

* @No__t 值类型，它表示一个真值，通常是关系或逻辑运算的结果。 文本的类型为 `System.Boolean`。 @No__t 类型的默认值与 `False` 的文本等效。

* @No__t-0 值类型，它表示日期和/或时间，并映射到 `System.DateTime`。 @No__t 类型的默认值与 `# 01/01/0001 12:00:00AM #` 的文本等效。

* @No__t-0 值类型，它表示单个 Unicode 字符并映射到 `System.Char`。 @No__t 类型的默认值为 `ChrW(0)` 的常量表达式。

* @No__t-0 引用类型，它表示一个 Unicode 字符序列并映射到 `System.String`。 @No__t 类型的默认值为 null 值。



## <a name="enumerations"></a>枚举

*枚举*是继承自 `System.Enum` 和符号的值类型，它表示一个基元整型类型的一组值。

```antlr
EnumDeclaration
    : Attributes? TypeModifier* 'Enum' Identifier
      ( 'As' NonArrayTypeName )? StatementTerminator
      EnumMemberDeclaration+
      'End' 'Enum' StatementTerminator
    ;
```

对于 @no__t 为-0 的枚举类型，默认值为表达式 `CType(0, E)` 生成的值。

枚举的基础类型必须是可表示枚举中定义的所有枚举器值的整型类型。 如果指定了基础类型，则它必须在 @no__t 8 命名空间中 `Byte`、`SByte`、`UShort`、`Short`、`UInteger`、`Integer`、@no__t、@no__t 或其对应的类型之一。 如果未显式指定基础类型，则默认值为 `Integer`。

下面的示例声明了基础类型为 `Long` 的枚举：

```vb
Enum Color As Long
    Red
    Green
    Blue
End Enum
```

开发人员可以选择使用 @no__t 的基础类型（如示例所示），以允许使用 @no__t 范围内的值，但不能使用 `Integer` 范围内的值，也可以保留此选项以供将来使用。


### <a name="enumeration-members"></a>枚举成员

枚举的成员是枚举中声明的值和从类 `System.Enum` 继承的成员。

枚举成员的范围是枚举声明体。 这意味着，枚举成员必须始终是限定的（除非该类型通过命名空间导入专门导入到命名空间）。

省略常量表达式值时，枚举成员声明的声明顺序非常重要。 枚举成员隐式具有 @no__t 仅限-0 访问;枚举成员声明中不允许使用访问修饰符。

```antlr
EnumMemberDeclaration
    : Attributes? Identifier ( Equals ConstantExpression )? StatementTerminator
    ;
```

### <a name="enumeration-values"></a>枚举值

枚举成员列表中的枚举值被声明为类型为基础枚举类型的常量，它们可以出现在需要常量的位置。 @No__t 为0的枚举成员定义为关联成员提供常量表达式所指示的值。 常数表达式的计算结果必须是可隐式转换为基础类型并且必须位于可由基础类型表示的值范围内的整数类型。 下面的示例是错误的，因为常量值 `1.5`、`2.3` 和 `3.3` 不能隐式转换为具有严格语义 `Long` 的基础整型类型。

```vb
Option Strict On

Enum Color As Long
    Red = 1.5
    Green = 2.3
    Blue = 3.3
End Enum
```

多个枚举成员可能共享相同的关联值，如下所示：

```vb
Enum Color
    Red
    Green
    Blue
    Max = Blue
End Enum
```

该示例显示一个枚举，该枚举具有两个枚举成员--`Blue` 并 `Max`--具有相同的关联值。

如果枚举中的第一个枚举器值定义没有初始值设定项，则相应的常量的值为 `0`。 如果枚举值定义没有初始值设定项，则会通过 `1` 增加上一个枚举值的值来为枚举器提供值。 这一增加的值必须在可由基础类型表示的值范围内。

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

上面的示例输出枚举值及其关联值。 输出为：

```console
Red = 0
Green = 10
Blue = 11
```

这些值的原因如下：

* 自动为 `Red` 的枚举值 @no__t 赋值，因为它没有初始值设定项，并且是第一个枚举值成员。

* 将 `Green` 的枚举值显式赋予 `10` 的值。

* 枚举值 `Blue` 会自动向其分配一个大于在该

常数表达式不能直接或间接地使用其自身关联的枚举值的值（也就是说，不允许常量表达式中的循环）。 下面的示例无效，因为 `A` 和 `B` 的声明是循环的。

```vb
Enum Circular
    A = B
    B
End Enum
```

`A` 显式依赖于 @no__t 1，`B` 隐式依赖于 @no__t。

## <a name="classes"></a>类

*类*是一种数据结构，它可以包含数据成员（常量、变量和事件）、函数成员（方法、属性、索引器、运算符和构造函数）以及嵌套类型。 类是引用类型。

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

下面的示例演示包含每种成员的类：

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

下面的示例演示了这些成员的用法：

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

有两个特定于类的修饰符： `MustInherit` 和 `NotInheritable`。 同时指定它们是无效的。


### <a name="class-base-specification"></a>类基规范

类声明可以包含定义类的直接基类型的基类型规范。

```antlr
ClassBase
    : 'Inherits' NonArrayTypeName StatementTerminator
    ;
```

如果类声明没有显式基类型，则直接基类型将隐式 `Object`。 例如：

```vb
Class Base
End Class

Class Derived
    Inherits Base
End Class
```

基类型本身不能是类型参数，但它可能涉及范围内的类型参数。

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

类只能派生自 @no__t 0 和类。 从 `System.ValueType`、`System.Enum`、`System.Array`、@no__t 或 @no__t 中派生类无效。 泛型类不能从 @no__t 派生，也不能从派生自的类派生。

每个类都有一个直接基类，并且不允许派生循环。 不能从 @no__t 0 类派生，并且基类的可访问域必须与类本身的可访问性域的超集或相同。


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

类成员声明可以有 `Public`、`Protected`、`Friend`、@no__t 或 @no__t 访问。 如果类成员声明不包括访问修饰符，则声明默认为 `Public` 访问，除非它是变量声明;在这种情况下，它默认为 `Private` 访问。

类成员的作用域是在其中进行成员声明的类体，加上该类的约束列表（如果它是泛型的并且具有约束）。 如果成员具有 @no__t 0 访问权限，则其范围将扩展到同一程序中任何派生类的类主体，或在给定 `Friend` 访问的任何程序集中，并且如果成员具有 `Public`、`Protected` 或 @no__t 的访问权限，则其范围将扩展到任何派生的类体任何程序中的 v) 类。


## <a name="structures"></a>结构

*结构*是继承自 @no__t 的值类型。 结构与类相似，因为它们表示可以包含数据成员和函数成员的数据结构。 但与类不同的是，结构不需要进行堆分配。

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

对于类，两个变量可以引用同一对象，因此，对一个变量执行的运算可能会影响另一个变量所引用的对象。 使用结构，每个变量都有自己的非 @no__t 0 数据的副本，因此，对一个变量执行的操作不会影响另一个，如下面的示例所示：

```vb
Structure Point
    Public x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

已知以上声明，以下代码会将值输出 `10`：

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

将 @no__t 0 分配给 `b` 将创建值的副本，而 @no__t，则不会将赋值不受 `a.x` 的赋值影响。 如果将 `Point` 改为作为类进行声明，则输出将为 `100`，因为 @no__t 和 @no__t 将引用相同的对象。


### <a name="structure-members"></a>结构成员

结构的成员是由其结构成员声明引入的成员以及从 `System.ValueType` 继承的成员。

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

每个结构都隐式包含一个 @no__t 的无参数实例构造函数，该构造函数可生成结构的默认值。 因此，结构类型声明不可能声明无参数的实例构造函数。 但结构类型允许声明*参数化*实例构造函数，如以下示例中所示：

```vb
Structure Point
    Private x, y As Integer

    Public Sub New(x As Integer, y As Integer)
        Me.x = x
        Me.y = y
    End Sub
End Structure
```

给定上述声明，以下语句将创建一个 @no__t 0，其中 `x`，`y` 初始化为零。

```vb
Dim p1 As Point = New Point()
Dim p2 As Point = New Point(0, 0)
```

由于结构直接包含其字段值（而不是对这些值的引用），因此结构不能包含直接或间接引用自身的字段。 例如，以下代码无效：

```vb
Structure S1
    Dim f1 As S2
End Structure

Structure S2
    ' This would require S1 to contain itself.
    Dim f1 As S1
End Structure
```

通常，结构成员声明只能有 `Public`、`Friend` 或 `Private` 访问，但当重写从 `Object` 继承的成员时，还可以使用 `Protected` 和 @no__t 5 访问。 当结构成员声明不包括访问修饰符时，该声明默认为 `Public` 访问。 结构声明的成员的作用域是在其中进行声明的结构主体，以及该结构的约束（如果它是泛型的并且具有约束）。


## <a name="standard-modules"></a>标准模块

*标准模块*是一种类型，其成员隐式地 `Shared`，范围限定为标准模块的包含命名空间的声明空间，而不只是标准模块声明本身。 标准模块永远不能实例化。 声明标准模块类型的变量是错误的。

```antlr
ModuleDeclaration
    : Attributes? TypeModifier* 'Module' Identifier StatementTerminator
      ModuleMemberDeclaration*
      'End' 'Module' StatementTerminator
    ;
```

标准模块的成员有两个完全限定的名称，一个没有标准模块名称，另一个具有标准模块名称。 命名空间中的多个标准模块可以定义具有特定名称的成员;对任一模块外的名称的非限定引用不明确。 例如：

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

模块只能在命名空间中声明，而不能嵌套在其他类型中。 标准模块不能实现接口，它们隐式派生自 `Object`，并且只有 @no__t 构造函数。


### <a name="standard-module-members"></a>标准模块成员

标准模块的成员是由其成员声明引入的成员以及从 `Object` 继承的成员。 标准模块可能具有除实例构造函数之外的任何类型的成员。 所有标准模块类型成员均隐式 `Shared`。

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

通常，标准模块成员声明只能有 `Public`、`Friend` 或 `Private` 访问，但当重写从 `Object` 继承的成员时，可以指定 @no__t、@no__t 访问修饰符。 如果标准模块成员声明不包括访问修饰符，则声明默认为 `Public` 访问，除非它是一个变量，默认为 @no__t 1 访问。

如前文所述，标准模块成员的作用域是包含标准模块声明的声明。 继承自 `Object` 的成员不包含在此特殊范围内;这些成员没有作用域，必须始终使用模块的名称进行限定。 如果成员具有 @no__t 0 访问权限，则它的作用域仅扩展到在同一程序中声明的命名空间成员，或在给定 `Friend` 访问权限的程序集中。


## <a name="interfaces"></a>接口

*接口*是其他类型实现以保证它们支持某些方法的引用类型。 接口绝不会直接创建且没有实际表示形式，其他类型必须转换为接口类型。 接口定义协定。 实现接口的类或结构必须遵循它的协定。

```antlr
InterfaceDeclaration
    : Attributes? TypeModifier* 'Interface' Identifier
      TypeParameterList? StatementTerminator
      InterfaceBase*
      InterfaceMemberDeclaration*
      'End' 'Interface' StatementTerminator
    ;
```


下面的示例演示了一个接口，该接口包含一个默认属性 `Item`、一个事件 `E`、一个方法 `F` 和一个属性 `P`：

```vb
Interface IExample
    Default Property Item(index As Integer) As String

    Event E()

    Sub F(value As Integer)

    Property P() As String
End Interface
```

接口可能使用多重继承。 在下面的示例中，接口 `IComboBox` 继承自 `ITextBox` 和 `IListBox`：

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

类和结构可以实现多个接口。 在下面的示例中，类 `EditBox` 派生自类 `Control`，并同时实现 `IControl` 和 @no__t 3：

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

接口的基接口是显式基接口及其基接口。 换言之，基接口集是显式基接口的完全可传递的闭包、其显式基接口等。 如果接口声明没有显式的接口基，则类型的基接口不会从 @no__t 继承（尽管它们确实具有到 @no__t 的扩大转换）。

```antlr
InterfaceBase
    : 'Inherits' InterfaceBases StatementTerminator
    ;

InterfaceBases
    : NonArrayTypeName ( Comma NonArrayTypeName )*
    ;
```

在下面的示例中，`IComboBox` 的基本接口 @no__t 为-1、`ITextBox` 和 @no__t。

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

接口继承其基接口的所有成员。 换句话说，上面 `IComboBox` 接口继承成员 `SetText` 和 `SetItems` 以及 `Paint`。

实现接口的类或结构还隐式实现了接口的所有基接口。

如果接口在基接口的传递闭包中多次出现，它只会将其成员分配给派生接口一次。 实现派生接口的类型只需要实现多次定义的基接口的方法。 在下面的示例中，`Paint` 只需实现一次，即使类实现了 `IComboBox` 和 `IControl`。

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

@No__t-0 子句对其他 @no__t 子句不起作用。 在下面的示例中，`IDerived` 必须用 `IBase` 限定 @no__t 的名称。

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

基接口的可访问域必须与接口本身的可访问域的可访问域相同或超集。


### <a name="interface-members"></a>接口成员

接口的成员由其成员声明引入的成员和从其基接口继承的成员组成。

```antlr
InterfaceMemberDeclaration
    : NonModuleDeclaration
    | InterfaceEventMemberDeclaration
    | InterfaceMethodMemberDeclaration
    | InterfacePropertyMemberDeclaration
    ;
```

尽管接口不会继承 `Object` 中的成员，但实现接口的每个类或结构都继承自 `Object`，`Object` 的成员（包括扩展方法）被视为接口的成员，并且可在直接接口，无需强制转换为 @no__t。 例如：

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

与 `Object` 的成员具有相同名称的接口成员隐式隐藏 @no__t 1 个成员。 只有嵌套类型、方法、属性和事件才能作为接口成员。 方法和属性可能没有主体。 接口成员将隐式 @no__t 0，并且不能指定访问修饰符。 在接口中声明的成员的作用域是在其中进行声明的接口主体，以及该接口的约束列表（如果它是泛型并且具有约束）。


## <a name="arrays"></a>数组

*数组*是一种引用类型，它包含通过与数组中的变量顺序相对应的*索引*来访问的变量。 数组中包含的变量（也称为数组的*元素*）必须都属于同一类型，并且此类型称为数组的*元素类型*。

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

数组元素在创建数组实例后就会存在，并且在销毁数组实例时停止存在。 数组的每个元素都初始化为其类型的默认值。 类型 `System.Array` 是所有数组类型的基类型，不能进行实例化。 每个数组类型都继承由 `System.Array` 类型声明的成员并且可转换为它（和 `Object`）。 元素 `T` 的一维数组类型还实现了 `System.Collections.Generic.IList(Of T)` 和 `IReadOnlyList(Of T)` 的接口;如果 @no__t 为引用类型，则数组类型还实现 `IList(Of U)`，对于具有从 `T` 进行扩大引用转换的任何 `U`，则为 `IReadOnlyList(Of U)`。

数组具有确定与每个数组元素关联的索引数的*级别*。 数组的秩决定数组的*维数*。 例如，秩为 one 的数组称为一维数组，而秩大于1的数组称为多维数组。

下面的示例创建一个整数值的一维数组，初始化数组元素，然后将每个元素输出到其中：

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

程序输出以下内容：

```console
arr(0) = 0
arr(1) = 1
arr(2) = 4
arr(3) = 9
arr(4) = 16
arr(5) = 25
```

数组的每个维度都具有关联的长度。 维度长度不是数组类型的一部分，而是在运行时创建数组类型的实例时建立的。 维度的长度决定了该维度的有效索引范围：对于长度为 `N` 的维度，索引范围可以介于0到 `N-1` 之间。 如果维度的长度为零，则该维度没有有效的索引。 数组中元素的总数为数组中每个维度的长度的乘积。 如果数组的任意维度的长度为零，则该数组称为空。 数组的元素类型可以是任何类型。

通过向现有类型名称添加修饰符来指定数组类型。 修饰符由左括号、一组零个或多个逗号和一个右括号组成。 所修改的类型是数组的元素类型，维数是逗号的数目加1。 如果指定了多个修饰符，则数组的元素类型为数组。 修饰符从左到右读取，最左边的修饰符是最外面的数组。 示例中

```vb
Module Test
    Dim arr As Integer(,)(,,)()
End Module
```

@no__t 的元素类型为一维数组的一维数组，该数组是 @no__t 一维数组的一维数组。

还可以通过将数组类型修饰符或数组大小的初始化修饰符放在变量名上，将变量声明为数组类型。 在这种情况下，数组元素类型是声明中提供的类型，数组维度由变量名称修饰符决定。 为清楚起见，对同一声明中的变量名称和类型名称具有数组类型修饰符是无效的。

下面的示例演示了使用数组类型 @no__t 为元素类型的各种局部变量声明：

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

数组类型名称修饰符扩展到其后的所有括号。 这意味着，在类型名称后允许使用括在括号中的一组参数的情况下，不可能为数组类型名称指定参数。 例如：

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

在最后一种情况下，`(3)` 将解释为类型名称的一部分，而不是作为一组构造函数参数。


## <a name="delegates"></a>委托

*委托*是引用类型的 @no__t 1 方法或对象的实例方法的引用类型。

```antlr
DelegateDeclaration
    : Attributes? TypeModifier* 'Delegate' MethodSignature StatementTerminator
    ;

MethodSignature
    : SubSignature
    | FunctionSignature
    ;
```

 与其他语言中的委托最接近的等效项是函数指针，但函数指针只能引用 `Shared` 函数，而委托可以引用 @no__t 1 和实例方法。 在后一种情况下，委托不仅存储对方法入口点的引用，还存储对用来调用该方法的对象实例的引用。

委托声明不能有 `Handles` 子句、@no__t 子句、方法体或 `End` 构造。 委托声明的参数列表不能包含 `Optional` 或 `ParamArray` 参数。 返回类型和参数类型的可访问域必须与委托本身的可访问域的可访问域相同或超集。

委托的成员是从类 `System.Delegate` 继承的成员。 委托还定义以下方法：

* 一个构造函数，该构造函数采用两个参数，一个类型为 `Object`，一个类型为 `System.IntPtr`。

* 与委托具有相同签名的 @no__t 0 方法。

* 一个 @no__t 0 方法，其签名为委托签名，具有三个不同之处。 首先，返回类型更改为 `System.IAsyncResult`。 其次，将两个附加参数添加到参数列表的末尾：类型的第一个 `System.AsyncCallback`，第二个参数 @no__t 为-1。 最后，所有 @no__t 参数都将更改为 `ByVal`。

* 返回类型与委托相同的 @no__t 0 方法。 方法的参数只是委托参数，它们的参数顺序与 `ByRef` 参数的参数相同，但其顺序与委托签名中出现的顺序相同。  除了这些参数外，还有一个附加参数，类型 `System.IAsyncResult`，位于参数列表的末尾。

定义和使用委托时有三个步骤：声明、实例化和调用。

委托是使用委托声明语法来声明的。 下面的示例声明一个名为 `SimpleDelegate` 的委托，该委托不采用任何参数：

```vb
Delegate Sub SimpleDelegate()
```

下一个示例创建一个 @no__t 的实例，然后立即调用它：

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

为方法实例化委托并随后立即通过委托调用的情况并不多，因为直接调用方法更为简单。 委托在使用匿名时显示其有用性。 下一个示例显示了一个 `MultiCall` 方法，该方法重复调用 @no__t 实例：

```vb
Sub MultiCall(d As SimpleDelegate, count As Integer)
    Dim i As Integer

    For i = 0 To count - 1
        d()
    Next i
End Sub
```

@No__t-0 方法 @no__t 的目标方法是什么，此方法具有哪些可访问性，或者该方法是否 `Shared`，这一点并不重要。 重要的是，目标方法的签名与 `SimpleDelegate` 兼容。


## <a name="partial-types"></a>分部类型

类和结构声明可以是*分部*声明。 分部声明不一定会完全描述声明中的声明类型。 相反，类型的声明可以分布在程序内的多个分部声明中;不能跨程序边界声明分部类型。 分部类型声明指定声明上的 `Partial` 修饰符。 然后，程序中具有相同完全限定名称的类型的任何其他声明将在编译时与分部声明一起合并，以形成单个类型声明。 例如，以下代码声明了一个类 `Test`，成员 `Test.C1` 和 `Test.C2`。

.vb：

```vb
Public Partial Class Test
    Public Sub S1()
    End Sub
End Class
```

b. .vb：

```vb
Public Class Test
    Public Sub S2()
    End Sub
End Class
```

组合分部类型声明时，至少一个声明必须具有 `Partial` 修饰符，否则会导致编译时错误。

__纪录.__ 尽管可以在多个分部声明中的一个声明上指定 `Partial`，但最好是在所有分部声明中指定它。 如果一个分部声明可见，但一个或多个分部声明隐藏（如扩展工具生成的代码的情况），则可接受将 `Partial` 修饰符从可见声明中退出，但在隐藏申报.

只有类和结构可以使用分部声明来声明。 当匹配部分声明时，将考虑类型的 arity：两个名称相同但类型参数不同的类的类不被视为属于同一时间的分部声明。 分部声明可以指定属性、类修饰符、`Inherits` 语句或 @no__t。 在编译时，分部声明的所有部分组合在一起，并用作类型声明的一部分。 如果特性、修饰符、基、接口或类型成员之间存在任何冲突，则会产生编译时错误。 例如：

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

前面的示例声明了一个类型 `Test1`，@no__t 它从 @no__t 继承，并实现 `System.IDisposable` 和 @no__t。 @No__t-0 的分部声明会导致编译时错误，因为其中一个声明指出 `Test2` @no__t 为-2，另一个指示 `Test2` 为 @no__t。

具有类型参数的分部类型可以声明类型参数的约束和方差，但每个分部声明的约束和方差必须匹配。 因此，约束和方差是特殊的，因为它们不会自动组合起来，如其他修饰符：

```vb
Partial Public Class List(Of T As IEnumerable)
End Class

' Error: Constraints on T don't match
Class List(Of T As IComparable)
End Class
```

使用多个分部声明声明类型的事实不影响类型中的名称查找规则。 因此，分部类型声明可以使用在其他分部类型声明中声明的成员，或者可以对其他分部类型声明中声明的接口实现方法。 例如：

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

嵌套类型也可以具有分部声明。 例如：

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

分部声明中的初始值设定项仍将按声明顺序执行;但对于出现在单独分部声明中的初始值设定项，没有保证的执行顺序。

## <a name="constructed-types"></a>构造类型

泛型类型声明本身不表示类型。 相反，可以通过应用类型参数，将泛型类型声明用作 "蓝图" 以形成多种不同类型。 具有应用到它的类型参数的泛型类型称为*构造类型*。 构造类型中的类型参数必须始终满足对它们匹配的类型参数上的约束。

类型名称可以标识构造类型，即使它不直接指定类型参数也是如此。 如果类型嵌套在泛型类声明中，并且包含声明的实例类型隐式用于名称查找，则可能会发生这种情况：

```vb
Class Outer(Of T) 
    Public Class Inner 
    End Class

    ' Type of i is the constructed type Outer(Of T).Inner
    Public i As Inner 
End Class
```

当可访问泛型类型和所有类型参数时，可访问构造类型 `C(Of T1,...,Tn)`。 例如，如果泛型类型 `C` @no__t 为-1，`T1,...,Tn` 的所有类型参数都是 `Public`，则构造的类型为 `Public`。 但是，如果类型名称或类型参数之一为 `Private`，则构造的类型的可访问性为 `Private`。 如果构造类型的一个类型参数为 `Protected`，另一个类型自变量 @no__t 为-1，则构造类型只能在此程序集中的类及其子类中进行访问，或在已提供 `Friend` 访问的任何程序集中访问。 换句话说，构造类型的可访问域是其构成部分的可访问性域的交集。

__纪录.__ 构造类型的可访问域是其构成部分的交集，这是定义新的可访问性级别所产生的副作用。 一个构造类型，其中包含一个 @no__t 为-0 的元素，并且只能在可访问 @no__t*和*@no__t *5 个成员*的上下文中访问 @no__t 为-1 的元素。 但是，无法以语言表达此辅助功能级别，因为辅助功能 `Protected Friend` 表示可以在可以访问 @no__t 2*或*`Protected` 成员的上下文*中访问实体*。

通过为泛型类型中的类型参数的每个匹配项替换提供的类型自变量，来确定基实现接口和构造类型的成员。

### <a name="open-types-and-closed-types"></a>开放式类型和闭合类型

一个或多个类型自变量是包含类型或方法的类型参数的构造类型称为*开放类型*。 这是因为类型的某些类型参数仍是未知的，因此该类型的实际形状还不是已知的。 相反，其类型参数为所有非类型参数的泛型类型称为*闭合类型*。 闭合类型的形状始终是完全已知的。 例如：

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

构造类型 `Base(Of Integer, V)` 是开放类型，因为虽然提供了类型参数 `T`，但已提供另一个类型参数 `U`。 因此，该类型的完整形状是未知的。 但是，构造类型 `Derived(Of Double)`，因为已提供了继承层次结构中的所有类型参数，所以该类型为关闭类型。

开放式类型定义如下：

* 类型参数是开放类型。

* 如果数组的元素类型为开放类型，则数组类型为开放类型。

* 如果构造类型的一个或多个类型参数是开放类型，则构造类型是开放类型。

* 关闭的类型是不是开放类型的类型。

由于程序入口点不能是泛型类型，因此在运行时使用的所有类型都将为关闭类型。

## <a name="special-types"></a>特殊类型

.NET Framework 包含许多由 .NET Framework 和 Visual Basic 语言专门处理的类：

.NET Framework 中表示 void 类型的类型 @no__t，只能在 `GetType` 表达式中直接引用。

类型 `System.RuntimeArgumentHandle`、`System.ArgIterator` 和 `System.TypedReference` 都可以将指针包含到堆栈中，因此不能出现在 .NET Framework 堆上。 因此，它们不能用作数组元素类型、返回类型、字段类型、泛型类型参数、可以为 null 的类型、@no__t 参数类型、要转换为 @no__t 的值的类型、`System.ValueType`、对 `Object` 或 @no 的实例成员的调用目标__t 或提升为闭包。
