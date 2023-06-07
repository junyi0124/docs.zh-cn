---
title: 语言独立性和与语言无关的组件
description: 了解如何使用 .NET 中支持的众多语言之一（如 C#、C++/CLI、F#、IronPython、VB、Visual COBOL 和 PowerShell）进行开发。
dev_langs:
- csharp
- vb
ms.date: 07/22/2016
ms.assetid: 2dbed1bc-86f5-43cd-9a57-adbb1c5efba4
ms.openlocfilehash: 259db1b9d33dd3b068f4d4fa18d2118db34bf0b0
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819079"
---
# <a name="language-independence-and-language-independent-components"></a>语言独立性和与语言无关的组件

.NET 是独立的语言。 这意味着，开发人员可使用面向 .NET 实现的多种语言之一（例如 C#、F# 和 Visual Basic）进行开发。 可访问针对 .NET 实现开发的类库的类型和成员，而不必了解它们的初始编写语言，也不必遵循任何原始语言的约定。 如果你是组件开发人员，无论组件采用哪种语言，均可由任何 .NET 应用访问。

> [!NOTE]
> 本文的第一部分讨论如何创建独立于语言的组件（即以任何语言编写的应用均可以使用的组件）。 还可以从用多种语言编写的源代码创建单个组件或应用；请参阅本文第二部分的[跨语言互操作性](#cross-language-interoperability)。

若要与使用任何语言编写的其他对象完全交互，对象必须只向调用方公开那些所有语言共有的功能。 此组通用功能由公共语言规范 (CLS) 定义，后者是一组适用于生成的程序集的规则。 公共语言规范在 [ECMA-335 标准：公共语言基础结构](https://www.ecma-international.org/publications/standards/Ecma-335.htm)的第 I 部分的第 7 条至第 11 条中进行了定义。

如果你的组件符合公共语言规范，则保证其符合 CLS 并可通过支持 CLS 的任何编程语言编写的程序集中的代码对其进行访问。 可以通过将 [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 特性应用于源代码来确定自己的组件在编译时是否符合公共语言规范。 有关详细信息，请参阅 [CLSCompliantAttribute 特性](#the-clscompliantattribute-attribute)。

## <a name="cls-compliance-rules"></a>CLS 遵从性规则

本节讨论用于创建符合 CLS 的组件的规则。 有关规则的完整列表，请参阅 [ECMA-335 标准：公共语言基础结构](https://www.ecma-international.org/publications/standards/Ecma-335.htm)的第 I 部分的第 7 条至第 11 条中进行了定义。

> [!NOTE]
> 公共语言规范讨论 CLS 遵从性的每个规则，因为它应用于使用者（以编程方式访问符合 CLS 的组件的开发人员）、框架（使用语言编译器创建符合 CLS 的库的开发人员）和扩展人员（创建可创建符合 CLS 的组件的语言编译器或代码分析器等工具的开发人员）。 本文重点介绍适用于框架的规则。 但请注意，一些适用于扩展程序的规则也适用于使用 [Reflection.Emit](xref:System.Reflection.Emit) 创建的程序集。

若要设计独立于语言的组件，您只需将 CLS 遵从性的规则应用于组件的公共接口即可。 您的私有实现不必符合规范。

> [!IMPORTANT]
> CLS 遵从性的规则仅适用于组件的公共接口，而非其私有实现。

例如，[Byte](xref:System.Byte) 之外的无符号整数不符合 CLS。 由于以下示例中的 `Person` 类公开了 [UInt16](xref:System.UInt16) 类型的 `Age` 属性，因此下面的代码显示编译器警告。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Person
{
   private UInt16 personAge = 0;

   public UInt16 Age
   { get { return personAge; } }
}
// The attempt to compile the example displays the following compiler warning:
//    Public1.cs(10,18): warning CS3003: Type of 'Person.Age' is not CLS-compliant
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Person
   Private personAge As UInt16

   Public ReadOnly Property Age As UInt16
      Get
         Return personAge
      End Get
   End Property
End Class
' The attempt to compile the example displays the following compiler warning:
'    Public1.vb(9) : warning BC40027: Return type of function 'Age' is not CLS-compliant.
'
'       Public ReadOnly Property Age As UInt16
'                                ~~~
```

可以通过将 `Age` 属性的类型从 `UInt16` 更改为 [Int16](xref:System.Int16) 来将 Person 类设置为符合 CLS，即一个符合 CLS 的 16 位带符号整数。 您无需更改私有 `personAge` 字段的类型。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Person
{
   private Int16 personAge = 0;

   public Int16 Age
   { get { return personAge; } }
}
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Person
   Private personAge As UInt16

   Public ReadOnly Property Age As Int16
      Get
         Return CType(personAge, Int16)
      End Get
   End Property
End Class
```

库的公共接口包括：

* 公共类的定义。

* 公共类中公共成员的定义，以及派生类可以访问的成员（即受保护的成员）的定义。

* 公共类中公共方法的参数和返回类型，以及派生类可以访问的方法的参数和返回类型。

下表中将列出 CLS 遵从性规则。 规则的文本摘自 [ECMA-335 标准：公共语言基础结构](https://www.ecma-international.org/publications/standards/Ecma-335.htm)（版权所有 2012，Ecma International）。 有关这些规则的详细信息，请参阅以下各节。

类别 | 查看 | 规则 | 规则编号
-------- | --- | ---- | -----------
可访问性 | [成员可访问性](#member-accessibility) | 重写继承的方法时，可访问性应不会更改（重写一个继承自其他具有可访问性 `family-or-assembly` 的程序集的方法除外）。 在此情况下，重写应具有可访问性 `family`。 | 10
可访问性 | [成员可访问性](#member-accessibility) | 类型和成员的可见性和可访问性应是这样的：只要任何成员是可见的且可访问的，则该成员签名中的类型应是可见且可访问的。 例如，在其程序集外部可见的公共方法不应具有其类型仅在程序集内可见的参数。 只要任何成员是可见且可访问的，则构成该成员签名中使用的实例化泛型类型的类型应是可见且可访问的。 例如，一个在其程序集外部可见的成员签名中出现的实例化泛型类型不应具有其类型仅在程序集内可见的泛型实参。 | 12
数组 | [数组](#arrays) | 数组应具有符合 CLS 的类型的元素，且数组的所有维度的下限应为零。 各重载间只需区别：项是数组还是数组的元素类型。 当重载基于两个或更多数组类型时，元素类型应为命名类型。 | 16
特性 | [特性](#attributes) | 特性应为 [System.Attribute](xref:System.Attribute) 类型或继承自该类型。 | 41
特性 | [特性](#attributes) | CLS 只允许自定义特性编码的子集。 应在这些编码中出现的唯一类型是（请参阅第 IV 部分）：[System.Type](xref:System.Type)、[System.String](xref:System.String)、[System.Char](xref:System.Char)、[System.Boolean](xref:System.Boolean)、[System.Byte](xref:System.Byte)、[System.Int16](xref:System.Int16)、[System.Int32](xref:System.Int32)、[System.Int64](xref:System.Int64)、[System.Single](xref:System.Single)、[System.Double](xref:System.Double)，以及基于符合 CLS 的基整数类型的任何枚举类型。 | 34
特性 | [特性](#attributes) | CLS 不允许公开可见的所需修饰符（`modreq`，请参阅第 II 部分)，但允许其不了解的可选修饰符（`modopt`，请参阅第 II 部分）。 | 35
构造函数 | [构造函数](#constructors) | 在访问继承的实例数据之前，对象构造函数应调用其基类的某个实例构造函数。 （这不适用于不需要构造函数的值类型。)  | 21
构造函数 | [构造函数](#constructors) | 对象构造函数只能在创建对象的过程中调用，并且不应将对象初始化两次。 | 22
枚举 | [枚举](#enumerations) | 枚举的基础类型应是内置的 CLS 整数类型，字段的名称应为“value__”，并且应将该字段标记为 `RTSpecialName`。 |  7
枚举 | [枚举](#enumerations) | 有两种不同的枚举，二者由是否存在 [System.FlagsAttribute](xref:System.FlagsAttribute)（请参阅第 IV 部分库）自定义特性指示。 一个表示命名的整数值；另一个表示命名的位标记（可合并以生成一个未命名的值）。 `enum` 的值不仅限于指定的值。 |  8
枚举 | [枚举](#enumerations) | 枚举的文本静态字段需具有枚举本身的类型。 |  9
事件 | [事件](#events) | 实现事件的方法将在元数据中标记为 `SpecialName`。 |29
事件 | [事件](#events) | 事件及其访问器的可访问性应相同。 |30
事件 | [事件](#events) | 事件的 `add` 和 `remove` 方法应同时存在或不存在。 |31
事件 | [事件](#events) | 事件的 `add` 和 `remove` 方法均应采用一个参数，该参数的类型定义了事件的类型，且应派生自 [System.Delegate](xref:System.Delegate)。 |32
事件 | [事件](#events) | 事件应遵循特定的命名模式。 CLS 规则 29 中引用的 SpecialName 特性将在适当名称比较中被忽略，且应遵循标识符规则。  |33
异常 | [异常](#exceptions) | 引发的对象应为 [System.Exception](xref:System.Exception) 类型或从该类型继承的类型。 但是，阻止其他类型的异常的传播无需符合 CLS 的方法。 | 40
常规 | [CLS 遵从性规则](#cls-compliance-rules) | CLS 规则仅适用于类型中在定义程序集之外可访问或可显示的部分。 | 1
常规 | [CLS 遵从性规则](#cls-compliance-rules) | 不应将不符合 CLS 的类型的成员标记为符合 CLS。 | 2
泛型 | [泛型类型和成员](#generic-types-and-members) | 嵌套类型拥有的泛型形参的数目至少应与封闭类型的一样多。 嵌套类型中的泛型参数在位置上与其封闭类型中的泛型参数对应。  | 42
泛型 | [泛型类型和成员](#generic-types-and-members) | 根据上面定义的规则，泛型类型的名称将对非嵌套类型上声明的或新引入到类型（如果嵌套）中的类型参数的数量进行编码。 | 43
泛型 | [泛型类型和成员](#generic-types-and-members) | 泛型类型应该重新声明足够多的约束，以确保对基类型或接口的任何约束能够满足泛型类型约束的需要。 | 44
泛型 | [泛型类型和成员](#generic-types-and-members) | 用作对泛型参数的约束的类型本身应符合 CLS。 | 45
泛型 | [泛型类型和成员](#generic-types-and-members) | 实例化泛型类型中的成员（包括嵌套类型）的可见性和可访问性应被认为限制在特定的实例化中，而不是限制在整个泛型类型声明中。 作出以下假定：CLS 规则 12 的可见性和可访问性仍适用。 | 46
泛型 | [泛型类型和成员](#generic-types-and-members) | 对于每个抽象或虚拟泛型方法，应该有默认的具体（非抽象）实现 | 47
接口 | [接口](#interfaces) | 符合 CLS 的接口不需要不符合 CLS 的方法的定义即可实现这些方法。 | 18
接口 | [接口](#interfaces) | 符合 CLS 的接口不应定义静态方法，也不应定义字段。 | 19
成员 | [类型成员概述](#type-members-in-general) | 全局静态字段和方法不符合 CLS。 | 36
成员 | -- | 通过使用字段初始化元数据指定文本静态值。 符合 CLS 的文本必须在类型与文本（或基本类型，如果文本为 `enum`）完全相同的字段初始化元数据中指定值。 | 13
成员 | [类型成员概述](#type-members-in-general) | vararg 约束不属于 CLS，并且 CLS 支持的唯一调用约定是标准托管调用约定。 | 15
命名约定 | [命名约定](#naming-conventions) | 程序集应遵守用于管理允许启用且包含在标识符中的字符集的 Unicode 标准 3.0 的技术报告 15 的附件 7（可通过 [Unicode 范式](https://unicode.org/reports/tr15/)在线获得）。 标识符应是由 Unicode 范式 C 定义的规范格式。对于 CLS，如果两个标识符的小写映射（由不区分区域设置的 Unicode、一对一小写映射指定）相同，则它们也相同。 也就是说，对于要在 CLS 下视为不同的两个标识符，它们应以大小写之外的差别进行区分。 但是，若要重写继承的定义，CLI 需要对使用的原始声明进行准确编码。 | 4
重载 | [命名约定](#naming-conventions) | 在符合 CLS 的范围中引入的所有名称都应是明显独立的类型，除非名称完全相同且通过重载解析。 也就是说，CTS 允许单个类型对方法和字段使用相同的名称，但 CLS 不允许。 | 5
重载 | [命名约定](#naming-conventions) | 即使 CTS 允许区分不同的签名，但字段和嵌套类型只能由标识符比较区分。 （标识符比较后）具有相同名称的方法、属性和事件应在除返回类型不同之外还具有其他差异，CLS 规则 39 中指定的差异除外 | 6
重载 | [重载](#overloads) | 只可重载属性和方法。 | 37
重载 | [重载](#overloads) |属性和方法仅可基于其参数的数目和类型进行重载，名为 `op_Implicit` 和 `op_Explicit` 的转换运算符除外，这两种转换运算符也可基于其返回类型进行重载。 | 38
重载 | -- | 如果类型中声明的两种或更多符合 CLS 的方法都具有相同的名称，并且对于特定的类型实例化集而言，它们具有相同的参数和返回类型，则所有这些方法在这些类型实例化时都应在语义上保持等效。 | 48
属性 | [属性](#properties) | 实现属性的 getter 和 setter 方法的方法应在元数据中标记为 `SpecialName`。 | 24
属性 | [属性](#properties) | 属性的访问器必须同为静态、同为虚拟或同为实例。 | 26
属性 | [属性](#properties) | 属性的类型应该是 getter 的返回类型和 setter 的最后一个自变量的类型。 属性参数的类型应是对应于 getter 的参数的类型和 setter 的除最后一个参数之外所有参数的类型。 所有这些类型都应该符合 CLS，并且不应是托管指针（也就是说，不应该被引用传递）。 | 27
属性 | [属性](#properties) | 属性应遵循特定的命名模式。 CLS 规则 24 中引用的 `SpecialName` 特性将在适当名称比较中被忽略，且应遵循标识符规则。 属性应具有 getter 和/或 setter 方法。 | 28
类型转换 | [类型转换](#type-conversion) | 如果提供 op_Implicit 或 op_Explicit，则必须提供实现强制转换的替代方法。 | 39
类型 | [类型和类型成员签名](#types-and-type-member-signatures) | 装箱的值类型不符合 CLS。 | 3
类型 | [类型和类型成员签名](#types-and-type-member-signatures) | 显示在签名中的所有类型应符合 CLS。 构成实例化泛型类型的所有类型应符合 CLS。 | 11
类型 | [类型和类型成员签名](#types-and-type-member-signatures) | 类型化的引用是不符合 CLS 的。 | 14
类型 | [类型和类型成员签名](#types-and-type-member-signatures) | 非托管的指针类型不符合 CLS。 | 17
类型 | [类型和类型成员签名](#types-and-type-member-signatures) | 符合 CLS 的类、值类型和接口不应要求实现不符合 CLS 的成员 | 20
类型 | [类型和类型成员签名](#types-and-type-member-signatures) | [System.Object](xref:System.Object) 符合 CLS。 任何其他符合 CLS 的类应从符合 CLS 的类继承。 | 23

索引到各个子部分：

* [类型和类型成员签名](#types-and-type-member-signatures)
* [命名约定](#naming-conventions)
* [类型转换](#type-conversion)
* [数组](#arrays)
* [接口](#interfaces)
* [枚举](#enumerations)
* [类型成员概述](#type-members-in-general)
* [成员可访问性](#member-accessibility)
* [泛型类型和成员](#generic-types-and-members)
* [构造函数](#constructors)
* [属性](#properties)
* [事件](#events)
* [重载](#overloads)
* [异常](#exceptions)
* [特性](#attributes)

### <a name="types-and-type-member-signatures"></a>类型和类型成员签名

[System.Object](xref:System.Object) 类型符合 CLS，是 .NET 类型系统中所有对象类型的基础类型。 .NET 中的继承要么是隐式的（例如，[String](xref:System.String) 类从 `Object` 类隐式继承），要么是显式的（例如，[CultureNotFoundException](xref:System.Globalization.CultureNotFoundException) 类从 [ArgumentException](xref:System.ArgumentException) 类显式继承，后者又从 [Exception](xref:System.Exception) 类显式继承。 对于要符合 CLS 的派生类型，其基本类型也必须符合 CLS。

下面的示例显示基本类型不符合 CLS 的派生类型。 它定义使用无符号 32 位整数作为计数器的基本 `Counter` 类。 由于类通过对无符号整数进行换行来提供计数器功能，因此类标记为不符合 CLS。 因此，派生类 `NonZeroCounter` 也不符合 CLS。

```csharp
using System;

[assembly: CLSCompliant(true)]

[CLSCompliant(false)]
public class Counter
{
   UInt32 ctr;

   public Counter()
   {
      ctr = 0;
   }

   protected Counter(UInt32 ctr)
   {
      this.ctr = ctr;
   }

   public override string ToString()
   {
      return $"{ctr}). ";
   }

   public UInt32 Value
   {
      get { return ctr; }
   }

   public void Increment()
   {
      ctr += (uint) 1;
   }
}

public class NonZeroCounter : Counter
{
   public NonZeroCounter(int startIndex) : this((uint) startIndex)
   {
   }

   private NonZeroCounter(UInt32 startIndex) : base(startIndex)
   {
   }
}
// Compilation produces a compiler warning like the following:
//    Type3.cs(37,14): warning CS3009: 'NonZeroCounter': base type 'Counter' is not
//            CLS-compliant
//    Type3.cs(7,14): (Location of symbol related to previous warning)
```

```vb
<Assembly: CLSCompliant(True)>

<CLSCompliant(False)> _
Public Class Counter
   Dim ctr As UInt32

   Public Sub New
      ctr = 0
   End Sub

   Protected Sub New(ctr As UInt32)
      ctr = ctr
   End Sub

   Public Overrides Function ToString() As String
      Return $"{ctr}). "
   End Function

   Public ReadOnly Property Value As UInt32
      Get
         Return ctr
      End Get
   End Property

   Public Sub Increment()
      ctr += CType(1, UInt32)
   End Sub
End Class

Public Class NonZeroCounter : Inherits Counter
   Public Sub New(startIndex As Integer)
      MyClass.New(CType(startIndex, UInt32))
   End Sub

   Private Sub New(startIndex As UInt32)
      MyBase.New(CType(startIndex, UInt32))
   End Sub
End Class
' Compilation produces a compiler warning like the following:
'    Type3.vb(34) : warning BC40026: 'NonZeroCounter' is not CLS-compliant
'    because it derives from 'Counter', which is not CLS-compliant.
'
'    Public Class NonZeroCounter : Inherits Counter
'                 ~~~~~~~~~~~~~~
```

成员签名中出现的所有类型（包括方法的返回类型或属性类型）必须符合 CLS。 此外，对于泛型类型：

* 构成实例化泛型类型的所有类型必须符合 CLS。

* 所有用作针对泛型参数的约束的类型必须符合 CLS。

.NET [通用类型系统](common-type-system.md)包括大量直接受公共语言运行时支持且专以程序集的元数据编码的内置类型。 在这些内部类型中，下表中所列的类型都符合 CLS。

符合 CLS 的类型 | 描述
------------------ | -----------
[Byte](xref:System.Byte) | 8 位无符号整数
[Int16](xref:System.Int16) | 16 位带符号整数
[Int32](xref:System.Int32) | 32 位带符号整数
[Int64](xref:System.Int64) | 64 位带符号整数
[单精度](xref:System.Single) | 单精度浮点值
[双精度](xref:System.Double) | 双精度浮点值
[布尔值](xref:System.Boolean) | true 或 false 值类型
[Char](xref:System.Char) | UTF 16 编码单元
[小数](xref:System.Decimal) | 非浮点十进制数字
[IntPtr](xref:System.IntPtr) | 平台定义的大小的指针或句柄
[字符串](xref:System.String) | 零个、一个或多个 Char 对象的集合

下表中所列的内部类型不符合 CLS。

不符合类型 | 描述 | 符合 CLS 的替代方法
------------------ | ----------- | -------------------------
[SByte](xref:System.SByte) | 8 位带符号整数数据类型 | [Int16](xref:System.Int16)
[UInt16](xref:System.UInt16) | 16 位无符号整数 | [Int32](xref:System.Int32)
[UInt32](xref:System.UInt32) | 32 位无符号整数 | [Int64](xref:System.Int64)
[UInt64](xref:System.UInt64) | 64 位无符号整数 | [Int64](xref:System.Int64)（可能溢出）、[BigInteger](xref:System.Numerics.BigInteger) 或 [Double](xref:System.Double)
[UIntPtr](xref:System.UIntPtr) | 未签名的指针或句柄 | [IntPtr](xref:System.IntPtr)

.NET 类库或任何其他类库可能包含不符合 CLS 的其他类型；例如：

* 装箱的值类型。 下面的 C# 示例创建一个具有名为 `int*` 的 `Value` 类型的公共属性的类。 由于 `int*` 是一个装箱的值类型，因此编译器将其标记为不符合 CLS。

```csharp
using System;

[assembly:CLSCompliant(true)]

public unsafe class TestClass
{
   private int* val;

   public TestClass(int number)
   {
      val = (int*) number;
   }

   public int* Value {
      get { return val; }
   }
}
// The compiler generates the following output when compiling this example:
//        warning CS3003: Type of 'TestClass.Value' is not CLS-compliant
```

* 类型化的引用是一个特殊构造，它包含一个对对象的引用和一个对类型的引用。

如果类型不符合 CLS，则需对其应用 *isCompliant* 值为 `false` 的 [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 特性。 有关详细信息，请参阅 [CLSCompliantAttribute 特性](#the-clscompliantattribute-attribute)部分。

下面的示例说明了方法签名和泛型类型实例化中 CLS 遵从性的问题。 它定义一个具有类型为 [UInt32](xref:System.UInt32) 的属性和类型为 [Nullable(Of UInt32)](xref:System.Nullable%601) 的属性的 `InvoiceItem` 类，以及一个具有类型为 `UInt32` 和 `Nullable(Of UInt32)` 的参数的构造函数。 您尝试编译此示例时会收到四个编译器警告。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class InvoiceItem
{
   private uint invId = 0;
   private uint itemId = 0;
   private Nullable<uint> qty;

   public InvoiceItem(uint sku, Nullable<uint> quantity)
   {
      itemId = sku;
      qty = quantity;
   }

   public Nullable<uint> Quantity
   {
      get { return qty; }
      set { qty = value; }
   }

   public uint InvoiceId
   {
      get { return invId; }
      set { invId = value; }
   }
}
// The attempt to compile the example displays the following output:
//    Type1.cs(13,23): warning CS3001: Argument type 'uint' is not CLS-compliant
//    Type1.cs(13,33): warning CS3001: Argument type 'uint?' is not CLS-compliant
//    Type1.cs(19,26): warning CS3003: Type of 'InvoiceItem.Quantity' is not CLS-compliant
//    Type1.cs(25,16): warning CS3003: Type of 'InvoiceItem.InvoiceId' is not CLS-compliant
```

```vb
<Assembly: CLSCompliant(True)>

Public Class InvoiceItem

   Private invId As UInteger = 0
   Private itemId As UInteger = 0
   Private qty AS Nullable(Of UInteger)

   Public Sub New(sku As UInteger, quantity As Nullable(Of UInteger))
      itemId = sku
      qty = quantity
   End Sub

   Public Property Quantity As Nullable(Of UInteger)
      Get
         Return qty
      End Get
      Set
         qty = value
      End Set
   End Property

   Public Property InvoiceId As UInteger
      Get
         Return invId
      End Get
      Set
         invId = value
      End Set
   End Property
End Class
' The attempt to compile the example displays output similar to the following:
'    Type1.vb(13) : warning BC40028: Type of parameter 'sku' is not CLS-compliant.
'
'       Public Sub New(sku As UInteger, quantity As Nullable(Of UInteger))
'                      ~~~
'    Type1.vb(13) : warning BC40041: Type 'UInteger' is not CLS-compliant.
'
'       Public Sub New(sku As UInteger, quantity As Nullable(Of UInteger))
'                                                               ~~~~~~~~
'    Type1.vb(18) : warning BC40041: Type 'UInteger' is not CLS-compliant.
'
'       Public Property Quantity As Nullable(Of UInteger)
'                                               ~~~~~~~~
'    Type1.vb(27) : warning BC40027: Return type of function 'InvoiceId' is not CLS-compliant.
'
'       Public Property InvoiceId As UInteger
```

若要消除编译器警告，请将 `InvoiceItem` 公共接口中不符合 CLS 的类型替换为符合 CLS 的类型：

```csharp
using System;

[assembly: CLSCompliant(true)]

public class InvoiceItem
{
   private uint invId = 0;
   private uint itemId = 0;
   private Nullable<int> qty;

   public InvoiceItem(int sku, Nullable<int> quantity)
   {
      if (sku <= 0)
         throw new ArgumentOutOfRangeException("The item number is zero or negative.");
      itemId = (uint) sku;

      qty = quantity;
   }

   public Nullable<int> Quantity
   {
      get { return qty; }
      set { qty = value; }
   }

   public int InvoiceId
   {
      get { return (int) invId; }
      set {
         if (value <= 0)
            throw new ArgumentOutOfRangeException("The invoice number is zero or negative.");
         invId = (uint) value; }
   }
}
```

```vb
Assembly: CLSCompliant(True)>

Public Class InvoiceItem

   Private invId As UInteger = 0
   Private itemId As UInteger = 0
   Private qty AS Nullable(Of Integer)

   Public Sub New(sku As Integer, quantity As Nullable(Of Integer))
      If sku <= 0 Then
         Throw New ArgumentOutOfRangeException("The item number is zero or negative.")
      End If
      itemId = CUInt(sku)
      qty = quantity
   End Sub

   Public Property Quantity As Nullable(Of Integer)
      Get
         Return qty
      End Get
      Set
         qty = value
      End Set
   End Property

   Public Property InvoiceId As Integer
      Get
         Return CInt(invId)
      End Get
      Set
         invId = CUInt(value)
      End Set
   End Property
End Class
```

除了列出的特定类型之外，某些类型的类别也不符合 CLS。 其中包括非托管指针类型和函数指针类型。 下面的示例将生成一个编译器警告，因为它使用指向整数的指针来创建整数数组。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ArrayHelper
{
   unsafe public static Array CreateInstance(Type type, int* ptr, int items)
   {
      Array arr = Array.CreateInstance(type, items);
      int* addr = ptr;
      for (int ctr = 0; ctr < items; ctr++) {
          int value = *addr;
          arr.SetValue(value, ctr);
          addr++;
      }
      return arr;
   }
}
// The attempt to compile this example displays the following output:
//    UnmanagedPtr1.cs(8,57): warning CS3001: Argument type 'int*' is not CLS-compliant
```

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ArrayHelper
{
   unsafe public static Array CreateInstance(Type type, int* ptr, int items)
   {
      Array arr = Array.CreateInstance(type, items);
      int* addr = ptr;
      for (int ctr = 0; ctr < items; ctr++) {
          int value = *addr;
          arr.SetValue(value, ctr);
          addr++;
      }
      return arr;
   }
}
// The attempt to compile this example displays the following output:
//    UnmanagedPtr1.cs(8,57): warning CS3001: Argument type 'int*' is not CLS-compliant
```

对于符合 CLS 的抽象类（即在 C# 中标记为 `abstract` 的类），该类的所有成员也必须符合 CLS。

### <a name="naming-conventions"></a>命名约定

由于一些编程语言不区分大小写，因此标识符（例如，命名空间、类型和成员的名称）必须不仅限于大小写不同。 如果两个标识符的小写映射是相同的，则这两个标识符被视为等效。 下面的 C# 示例定义两个公共类：`Person` 和 `person`。 由于它们只是大小写不同，因此 C# 编译器会将其标记为不符合 CLS。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Person : person
{

}

public class person
{

}
// Compilation produces a compiler warning like the following:
//    Naming1.cs(11,14): warning CS3005: Identifier 'person' differing
//                       only in case is not CLS-compliant
//    Naming1.cs(6,14): (Location of symbol related to previous warning)
```

编程语言标识符（例如，命名空间、类型和成员的名称）必须遵照 [Unicode 标准](https://unicode.org/reports/tr15/)。 这表示：

* 标识符的第一个字符可以是任何 Unicode 大写字母、小写字母、标题大小写字母、修饰符字母、其他字母或字母数字。 有关 Unicode 字符类别的信息，请参阅 [System.Globalization.UnicodeCategory](xref:System.Globalization.UnicodeCategory) 枚举。

* 后继字符可以是任何类别的第一个字符，还可以包含无间隔标记、间距组合标记、十进制数字、连接器标点符号以及格式设置代码。

在比较标识符之前，应筛选格式设置代码，并将标识符转换为 Unicode 范式 C，因为单个字符可由多个 UTF 16 编码的代码单位表示。 在 Unicode 范式 C 中生成相同代码单位的字符序列不符合 CLS。 下列示例定义了一个名为 `Å` 的属性（包含字符 ANGSTROM SIGN (U+212B)）和另一个名为 `Å` 的属性（包含字符 LATIN CAPITAL LETTER A WITH RING ABOVE (U+00C5)）。 C# 编译器将源代码标记为不符合 CLS。

```csharp
public class Size
{
   private double a1;
   private double a2;

   public double Å
   {
       get { return a1; }
       set { a1 = value; }
   }

   public double Å
   {
       get { return a2; }
       set { a2 = value; }
   }
}
// Compilation produces a compiler warning like the following:
//    Naming2a.cs(16,18): warning CS3005: Identifier 'Size.Å' differing only in case is not
//            CLS-compliant
//    Naming2a.cs(10,18): (Location of symbol related to previous warning)
//    Naming2a.cs(18,8): warning CS3005: Identifier 'Size.Å.get' differing only in case is not
//            CLS-compliant
//    Naming2a.cs(12,8): (Location of symbol related to previous warning)
//    Naming2a.cs(19,8): warning CS3005: Identifier 'Size.Å.set' differing only in case is not
//            CLS-compliant
//    Naming2a.cs(13,8): (Location of symbol related to previous warning)
```

```vb
<Assembly: CLSCompliant(True)>
Public Class Size
   Private a1 As Double
   Private a2 As Double

   Public Property Å As Double
       Get
          Return a1
       End Get
       Set
          a1 = value
       End Set
   End Property

   Public Property Å As Double
       Get
          Return a2
       End Get
       Set
          a2 = value
       End Set
   End Property
End Class
' Compilation produces a compiler warning like the following:
'    Naming1.vb(9) : error BC30269: 'Public Property Å As Double' has multiple definitions
'     with identical signatures.
'
'       Public Property Å As Double
'                       ~
```

特定范围内的成员名称（如程序集中的命名空间、命名空间中的类型或某类型中的成员）必须是唯一的，通过重载解析的名称除外。 此要求比常规类型系统的要求更加严格，后者允许一个范围内的多个成员在作为不同类型的成员（例如，一个是方法，一个是字段时）时，可以具有相同的名称。 具体而言，对于类型成员：

* 字段和嵌套类型只能通过名称区分。

* 具有相同名称的方法、属性和事件必须具有除返回类型不同之外的其他不同之处。

下面的示例说明了成员名称在其范围内必须是唯一的要求。 它定义了一个名为 `Converter` 的类，该类包括四个名为 `Conversion` 的成员。 其中三个是方法，一个是属性。 包含 `Int64` 参数的方法具有唯一名称，但是，具有 `Int32` 参数的两个方法不是，因为返回值不被视为成员签名的一部分。 由于属性不能具有与重载方法相同的名称，因此 `Conversion` 属性也违反了此要求。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Converter
{
   public double Conversion(int number)
   {
      return (double) number;
   }

   public float Conversion(int number)
   {
      return (float) number;
   }

   public double Conversion(long number)
   {
      return (double) number;
   }

   public bool Conversion
   {
      get { return true; }
   }
}
// Compilation produces a compiler error like the following:
//    Naming3.cs(13,17): error CS0111: Type 'Converter' already defines a member called
//            'Conversion' with the same parameter types
//    Naming3.cs(8,18): (Location of symbol related to previous error)
//    Naming3.cs(23,16): error CS0102: The type 'Converter' already contains a definition for
//            'Conversion'
//    Naming3.cs(8,18): (Location of symbol related to previous error)
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Converter
   Public Function Conversion(number As Integer) As Double
      Return CDbl(number)
   End Function

   Public Function Conversion(number As Integer) As Single
      Return CSng(number)
   End Function

   Public Function Conversion(number As Long) As Double
      Return CDbl(number)
   End Function

   Public ReadOnly Property Conversion As Boolean
      Get
         Return True
      End Get
   End Property
End Class
' Compilation produces a compiler error like the following:
'    Naming3.vb(8) : error BC30301: 'Public Function Conversion(number As Integer) As Double'
'                    and 'Public Function Conversion(number As Integer) As Single' cannot
'                    overload each other because they differ only by return types.
'
'       Public Function Conversion(number As Integer) As Double
'                       ~~~~~~~~~~
'    Naming3.vb(20) : error BC30260: 'Conversion' is already declared as 'Public Function
'                     Conversion(number As Integer) As Single' in this class.
'
'       Public ReadOnly Property Conversion As Boolean
'                                ~~~~~~~~~~
```

各种语言都包含唯一关键字，因此面向公共语言运行时的语言还必须提供一些机制，以便引用与关键字相符的标识符（例如，类型名称）。 例如，`case` 是 C# 和 Visual Basic 两者中的关键字。 但是，下面的 Visual Basic 示例可以通过使用左大括号和右大括号将名为 `case` 的类从 `case` 关键字中消除。 否则，该示例将生成错误消息“关键字无法用作标识符”，并且编译将失败。

```vb
Public Class [case]
   Private _id As Guid
   Private name As String

   Public Sub New(name As String)
      _id = Guid.NewGuid()
      Me.name = name
   End Sub

   Public ReadOnly Property ClientName As String
      Get
         Return name
      End Get
   End Property
End Class
```

以下 C# 示例可以通过使用 @ 符号从语言关键字中消除标识符来实例化 `case` 类。 如果没有该符号，C# 编译器会显示两条错误消息：“应为类型”和“无效表达式术语‘case’”。

```csharp
using System;

public class Example
{
   public static void Main()
   {
      @case c = new @case("John");
      Console.WriteLine(c.ClientName);
   }
}
```

### <a name="type-conversion"></a>类型转换

公共语言规范定义了两个转换运算符：

* `op_Implicit` 用于扩大转换，不会丢失数据或精度。 例如，[Decimal](xref:System.Decimal) 结构包含一个已重载的 `op_Implicit` 运算符，将整数类型值和 [Char](xref:System.Char) 值转换为 `Decimal` 值。

* `op_Explicit` 用于收缩可能导致丢失量级（将一个值转换为一个范围更小的值）或精度的转换。 例如，`Decimal` 结构包含一个已重载的 `op_Explicit` 运算符，将 [Double](xref:System.Double) 和 [Single](xref:System.Single) 值转换为 `Decimal`，将 `Decimal` 值转换为整数值：`Double`、`Single` 和 `Char`。

但是，并非所有语言都支持运算符重载或定义自定义运算符。 如果选择实现这些转换运算符，您还应提供另一种执行转换的方法。 我们建议提供 `From`Xxx 和 `To`Xxx 方法。

下面的示例定义符合 CLS 的隐式和显式转换。 它创建表示带符号的双精度浮点数字的 `UDouble` 类。 它提供从 `UDouble` 到 `Double` 的隐式转换和从 `UDouble` 到 `Single`、从 `Double` 到 `UDouble` 以及从 `Single` 到 `UDouble` 的显式转换。 它还将 `ToDouble` 方法定义为隐式转换运算符的替代方法，并将 `ToSingle`、`FromDouble` 和 `FromSingle` 方法定义为显式转换运算符的替代方法。

```csharp
using System;

public struct UDouble
{
   private double number;

   public UDouble(double value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      number = value;
   }

   public UDouble(float value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      number = value;
   }

   public static readonly UDouble MinValue = (UDouble) 0.0;
   public static readonly UDouble MaxValue = (UDouble) Double.MaxValue;

   public static explicit operator Double(UDouble value)
   {
      return value.number;
   }

   public static implicit operator Single(UDouble value)
   {
      if (value.number > (double) Single.MaxValue)
         throw new InvalidCastException("A UDouble value is out of range of the Single type.");

      return (float) value.number;
   }

   public static explicit operator UDouble(double value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      return new UDouble(value);
   }

   public static implicit operator UDouble(float value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      return new UDouble(value);
   }

   public static Double ToDouble(UDouble value)
   {
      return (Double) value;
   }

   public static float ToSingle(UDouble value)
   {
      return (float) value;
   }

   public static UDouble FromDouble(double value)
   {
      return new UDouble(value);
   }

   public static UDouble FromSingle(float value)
   {
      return new UDouble(value);
   }
}
```

```vb
Public Structure UDouble
   Private number As Double

   Public Sub New(value As Double)
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      number = value
   End Sub

   Public Sub New(value As Single)
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      number = value
   End Sub

   Public Shared ReadOnly MinValue As UDouble = CType(0.0, UDouble)
   Public Shared ReadOnly MaxValue As UDouble = Double.MaxValue

   Public Shared Widening Operator CType(value As UDouble) As Double
      Return value.number
   End Operator

   Public Shared Narrowing Operator CType(value As UDouble) As Single
      If value.number > CDbl(Single.MaxValue) Then
         Throw New InvalidCastException("A UDouble value is out of range of the Single type.")
      End If
      Return CSng(value.number)
   End Operator

   Public Shared Narrowing Operator CType(value As Double) As UDouble
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      Return New UDouble(value)
   End Operator

   Public Shared Narrowing Operator CType(value As Single) As UDouble
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      Return New UDouble(value)
   End Operator

   Public Shared Function ToDouble(value As UDouble) As Double
      Return CType(value, Double)
   End Function

   Public Shared Function ToSingle(value As UDouble) As Single
      Return CType(value, Single)
   End Function

   Public Shared Function FromDouble(value As Double) As UDouble
      Return New UDouble(value)
   End Function

   Public Shared Function FromSingle(value As Single) As UDouble
      Return New UDouble(value)
   End Function
End Structure
```

### <a name="arrays"></a>数组

符合 CLS 的数组符合以下规则：

* 数组的所有维度必须具有零下限。 下面的示例创建一个不符合 CLS 的数组，其下限为 1。 请注意，无论是否存在 [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 特性，编译器都不检测由 `Numbers.GetTenPrimes` 方法返回的数组是否符合 CLS。

  ```csharp
  [assembly: CLSCompliant(true)]

  public class Numbers
  {
    public static Array GetTenPrimes()
    {
        Array arr = Array.CreateInstance(typeof(Int32), new int[] {10}, new int[] {1});
        arr.SetValue(1, 1);
        arr.SetValue(2, 2);
        arr.SetValue(3, 3);
        arr.SetValue(5, 4);
        arr.SetValue(7, 5);
        arr.SetValue(11, 6);
        arr.SetValue(13, 7);
        arr.SetValue(17, 8);
        arr.SetValue(19, 9);
        arr.SetValue(23, 10);

        return arr;
    }
  }
  ```

  ```vb
  <Assembly: CLSCompliant(True)>

  Public Class Numbers
     Public Shared Function GetTenPrimes() As Array
        Dim arr As Array = Array.CreateInstance(GetType(Int32), {10}, {1})
        arr.SetValue(1, 1)
        arr.SetValue(2, 2)
        arr.SetValue(3, 3)
        arr.SetValue(5, 4)
        arr.SetValue(7, 5)
        arr.SetValue(11, 6)
        arr.SetValue(13, 7)
        arr.SetValue(17, 8)
        arr.SetValue(19, 9)
        arr.SetValue(23, 10)
        Return arr
     End Function
  End Class
  ```

* 所有数组元素必须包括符合 CLS 的类型。 下面的示例定义返回不符合 CLS 的数组的两种方法。 第一个返回 [UInt32](xref:System.UInt32) 值的数组。 第二个返回包括 [Int32](xref:System.Int32) 和 `UInt32` 值的 [Object](xref:System.Object) 数组。 虽然编译器因第一个数组是 `UInt32` 类型而将其标识为不合规，但无法识别第二个包含不符合 CLS 元素的数组。

  ```csharp
  using System;

  [assembly: CLSCompliant(true)]

  public class Numbers
  {
    public static UInt32[] GetTenPrimes()
    {
        uint[] arr = { 1u, 2u, 3u, 5u, 7u, 11u, 13u, 17u, 19u };
        return arr;
    }

    public static Object[] GetFivePrimes()
    {
        Object[] arr = { 1, 2, 3, 5u, 7u };
        return arr;
    }
  }
  // Compilation produces a compiler warning like the following:
  //    Array2.cs(8,27): warning CS3002: Return type of 'Numbers.GetTenPrimes()' is not
  //            CLS-compliant
  ```

  ```vb
  <Assembly: CLSCompliant(True)>

  Public Class Numbers
     Public Shared Function GetTenPrimes() As UInt32()
        Return { 1ui, 2ui, 3ui, 5ui, 7ui, 11ui, 13ui, 17ui, 19ui }
     End Function
     Public Shared Function GetFivePrimes() As Object()
        Dim arr() As Object = { 1, 2, 3, 5ui, 7ui }
        Return arr
     End Function
  End Class
  ' Compilation produces a compiler warning like the following:
  '    warning BC40027: Return type of function 'GetTenPrimes' is not CLS-compliant.
  ```

* 具有数组参数的方法的重载决策基于它们是否为数组及它们的元素类型。 因此，以下对重载的 `GetSquares` 方法的定义符合 CLS。

  ```csharp
  using System;
  using System.Numerics;

  [assembly: CLSCompliant(true)]

  public class Numbers
  {
    public static byte[] GetSquares(byte[] numbers)
    {
        byte[] numbersOut = new byte[numbers.Length];
        for (int ctr = 0; ctr < numbers.Length; ctr++) {
            int square = ((int) numbers[ctr]) * ((int) numbers[ctr]);
            if (square <= Byte.MaxValue)
                numbersOut[ctr] = (byte) square;
            // If there's an overflow, assign MaxValue to the corresponding
            // element.
            else
                numbersOut[ctr] = Byte.MaxValue;

        }
        return numbersOut;
    }

    public static BigInteger[] GetSquares(BigInteger[] numbers)
  {
        BigInteger[] numbersOut = new BigInteger[numbers.Length];
        for (int ctr = 0; ctr < numbers.Length; ctr++)
            numbersOut[ctr] = numbers[ctr] * numbers[ctr];

       return numbersOut;
    }
  }
  ```

  ```vb
  Imports System.Numerics

  <Assembly: CLSCompliant(True)>

  Public Module Numbers
     Public Function GetSquares(numbers As Byte()) As Byte()
        Dim numbersOut(numbers.Length - 1) As Byte
        For ctr As Integer = 0 To numbers.Length - 1
           Dim square As Integer = (CInt(numbers(ctr)) * CInt(numbers(ctr)))
           If square <= Byte.MaxValue Then
              numbersOut(ctr) = CByte(square)
           ' If there's an overflow, assign MaxValue to the corresponding
           ' element.
           Else
              numbersOut(ctr) = Byte.MaxValue
           End If
        Next
        Return numbersOut
     End Function

     Public Function GetSquares(numbers As BigInteger()) As BigInteger()
         Dim numbersOut(numbers.Length - 1) As BigInteger
         For ctr As Integer = 0 To numbers.Length - 1
            numbersOut(ctr) = numbers(ctr) * numbers(ctr)
         Next
         Return numbersOut
     End Function
  End Module
  ```

### <a name="interfaces"></a>接口

符合 CLS 的接口可以定义属性、事件和虚拟方法（没有实现的方法）。 符合 CLS 的接口不能有：

* 静态方法或静态字段。 如果在接口中定义静态成员，C# 编译器将生成编译器错误。

* 字段。 如果在接口中定义字段，C# 编译器将生成编译器错误。

* 不符合 CLS 的方法。 例如，下面的接口定义包括方法 `INumber.GetUnsigned`，该方法标记为不符合 CLS。 此示例生成编译器警告。

  ```csharp
  using System;

  [assembly:CLSCompliant(true)]

  public interface INumber
  {
      int Length();
      [CLSCompliant(false)] ulong GetUnsigned();
  }
  // Attempting to compile the example displays output like the following:
  //    Interface2.cs(8,32): warning CS3010: 'INumber.GetUnsigned()': CLS-compliant interfaces
  //            must have only CLS-compliant members
  ```

  ```vb
  <Assembly: CLSCompliant(True)>

  Public Interface INumber
    Function Length As Integer
      <CLSCompliant(False)> Function GetUnsigned As ULong
    End Interface
    ' Attempting to compile the example displays output like the following:
    '    Interface2.vb(9) : warning BC40033: Non CLS-compliant 'function' is not allowed in a
    '    CLS-compliant interface.
    '
    '       <CLSCompliant(False)> Function GetUnsigned As ULong
    '                                      ~~~~~~~~~~~
  ```

  由于存在此规则，因此符合 CLS 的类型不需要实现不符合 CLS 的成员。 如果一个符合 CLS 的框架公开实现不符合 CLS 接口的类，则其还应提供所有不符合 CLS 的成员的具体实现。

符合 CLS 的语言编译器还必须允许类提供在多个接口中具有相同名称和签名的成员的单独实现。 C# 支持使用显式接口实现来提供同名方法的不同实现。 下面的示例通过定义一个将 `Temperature` 和 `ICelsius` 接口作为显式接口实现的 `IFahrenheit` 类来说明此方案。

```csharp
using System;

[assembly: CLSCompliant(true)]

public interface IFahrenheit
{
   decimal GetTemperature();
}

public interface ICelsius
{
   decimal GetTemperature();
}

public class Temperature : ICelsius, IFahrenheit
{
   private decimal _value;

   public Temperature(decimal value)
   {
      // We assume that this is the Celsius value.
      _value = value;
   }

   decimal IFahrenheit.GetTemperature()
   {
      return _value * 9 / 5 + 32;
   }

   decimal ICelsius.GetTemperature()
   {
      return _value;
   }
}
public class Example
{
   public static void Main()
   {
      Temperature temp = new Temperature(100.0m);
      ICelsius cTemp = temp;
      IFahrenheit fTemp = temp;
      Console.WriteLine("Temperature in Celsius: {0} degrees",
                        cTemp.GetTemperature());
      Console.WriteLine("Temperature in Fahrenheit: {0} degrees",
                        fTemp.GetTemperature());
   }
}
// The example displays the following output:
//       Temperature in Celsius: 100.0 degrees
//       Temperature in Fahrenheit: 212.0 degrees
```

```vb
Assembly: CLSCompliant(True)>

Public Interface IFahrenheit
   Function GetTemperature() As Decimal
End Interface

Public Interface ICelsius
   Function GetTemperature() As Decimal
End Interface

Public Class Temperature : Implements ICelsius, IFahrenheit
   Private _value As Decimal

   Public Sub New(value As Decimal)
      ' We assume that this is the Celsius value.
      _value = value
   End Sub

   Public Function GetFahrenheit() As Decimal _
          Implements IFahrenheit.GetTemperature
      Return _value * 9 / 5 + 32
   End Function

   Public Function GetCelsius() As Decimal _
          Implements ICelsius.GetTemperature
      Return _value
   End Function
End Class

Module Example
   Public Sub Main()
      Dim temp As New Temperature(100.0d)
      Console.WriteLine("Temperature in Celsius: {0} degrees",
                        temp.GetCelsius())
      Console.WriteLine("Temperature in Fahrenheit: {0} degrees",
                        temp.GetFahrenheit())
   End Sub
End Module
' The example displays the following output:
'       Temperature in Celsius: 100.0 degrees
'       Temperature in Fahrenheit: 212.0 degrees
```

### <a name="enumerations"></a>枚举

符合 CLS 的枚举必须遵循下列规则：

* 枚举的基础类型必须是符合 CLS 的内部整数（[Byte](xref:System.Byte)、[Int16](xref:System.Int16)、[Int32](xref:System.Int32) 或 [Int64](xref:System.Int64)）。 例如，下面的代码尝试定义基础类型为 [UInt32](xref:System.UInt32) 的枚举并生成编译器警告。

    ```csharp
    using System;

    [assembly: CLSCompliant(true)]

    public enum Size : uint {
        Unspecified = 0,
        XSmall = 1,
        Small = 2,
        Medium = 3,
        Large = 4,
        XLarge = 5
    };

    public class Clothing
    {
        public string Name;
        public string Type;
        public string Size;
    }
    // The attempt to compile the example displays a compiler warning like the following:
    //    Enum3.cs(6,13): warning CS3009: 'Size': base type 'uint' is not CLS-compliant
    ```

    ```vb
    <Assembly: CLSCompliant(True)>

    Public Enum Size As UInt32
       Unspecified = 0
       XSmall = 1
       Small = 2
       Medium = 3
       Large = 4
       XLarge = 5
    End Enum

    Public Class Clothing
       Public Name As String
       Public Type As String
       Public Size As Size
    End Class
    ' The attempt to compile the example displays a compiler warning like the following:
    '    Enum3.vb(6) : warning BC40032: Underlying type 'UInt32' of Enum is not CLS-compliant.
    '
    '    Public Enum Size As UInt32
    '                ~~~~
    ```

* 枚举类型必须具有名为 `Value__` 且标有 `FieldAttributes.RTSpecialName` 特性的单个实例字段。 这使得您可以隐式引用字段值。

* 枚举包括文本静态字段，该字段的类型与枚举本身的类型匹配。 例如，如果您用 `State` 和 `State.On` 的值定义 `State.Off` 枚举，则 `State.On` 和 `State.Off` 都是文本静态字段，其类型为 `State`。

* 有两种枚举：

  * 一种表示一组互斥的命名整数值的枚举。 这种类型的枚举由缺少 [System.FlagsAttribute](xref:System.FlagsAttribute) 自定义特性表示。

  * 一种表示可结合用来生成未命名值的一组位标志的枚举。 这种类型的枚举由存在 [System.FlagsAttribute](xref:System.FlagsAttribute) 自定义特性表示。

有关详细信息，请参阅 [Enum](xref:System.Enum) 结构的文档。

* 枚举的值不限于其指定值的范围。 换言之，枚举中的值的范围是其基础值的范围。 您可以使用 `Enum.IsDefined` 方法来确定指定的值是否为枚举成员。

### <a name="type-members-in-general"></a>类型成员概述

公共语言规范要求将所有字段和方法作为特定类的成员进行访问。 因此，全局静态字段和方法（即，除类型外定义的静态字段或方法）不符合 CLS。 如果尝试在源代码中包括全局字段或方法，C# 编译器将生成编译器错误。

公共语言规范仅支持标准托管调用约定。 它不支持非托管调用约定和带使用 `varargs` 关键字标记的变量参数列表的方法。 对于符合标准托管调用约定的变量自变量列表，请使用 [ParamArrayAttribute](xref:System.ParamArrayAttribute) 特性或单个语言的实现，如 C# 中的 `params` 关键字和 Visual Basic 中的 `ParamArray` 关键字。

### <a name="member-accessibility"></a>成员可访问性

重写继承成员不能更改该成员的可访问性。 例如，无法在派生类中通过私有方法重写基类中的公共方法。 有一个例外：由其他程序集中的类型重写的程序集中的 `protected internal`（在 C# 中）或 `Protected Friend`（在 Visual Basic 中）成员。  在该示例中，重写的可访问性是 `Protected`。

下面的示例说明了当 [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 特性设置为 `true`，并且 `Person`（它是派生自 `Animal` 的类）尝试将 `Species` 属性的可访问性从公开更改为私有时生成的错误。 如果该示例的可访问性更改为公共，则其编译成功。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Animal
{
   private string _species;

   public Animal(string species)
   {
      _species = species;
   }

   public virtual string Species
   {
      get { return _species; }
   }

   public override string ToString()
   {
      return _species;
   }
}

public class Human : Animal
{
   private string _name;

   public Human(string name) : base("Homo Sapiens")
   {
      _name = name;
   }

   public string Name
   {
      get { return _name; }
   }

   private override string Species
   {
      get { return base.Species; }
   }

   public override string ToString()
   {
      return _name;
   }
}

public class Example
{
   public static void Main()
   {
      Human p = new Human("John");
      Console.WriteLine(p.Species);
      Console.WriteLine(p.ToString());
   }
}
// The example displays the following output:
//    error CS0621: 'Human.Species': virtual or abstract members cannot be private
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Animal
   Private _species As String

   Public Sub New(species As String)
      _species = species
   End Sub

   Public Overridable ReadOnly Property Species As String
      Get
         Return _species
      End Get
   End Property

   Public Overrides Function ToString() As String
      Return _species
   End Function
End Class

Public Class Human : Inherits Animal
   Private _name As String

   Public Sub New(name As String)
      MyBase.New("Homo Sapiens")
      _name = name
   End Sub

   Public ReadOnly Property Name As String
      Get
         Return _name
      End Get
   End Property

   Private Overrides ReadOnly Property Species As String
      Get
         Return MyBase.Species
      End Get
   End Property

   Public Overrides Function ToString() As String
      Return _name
   End Function
End Class

Public Module Example
   Public Sub Main()
      Dim p As New Human("John")
      Console.WriteLine(p.Species)
      Console.WriteLine(p.ToString())
   End Sub
End Module
' The example displays the following output:
'     'Private Overrides ReadOnly Property Species As String' cannot override
'     'Public Overridable ReadOnly Property Species As String' because
'      they have different access levels.
'
'         Private Overrides ReadOnly Property Species As String
```

如果某个成员是可访问的，则该成员签名中的类型必须是可访问的。 例如，这意味着公共成员不能包含类型是私有的、受保护的或内部的参数。 下面的示例说明了当 `StringWrapper` 类构造函数公开一个用于确定如何包装字符串值的内部 `StringOperationType` 枚举值时出现的编译器错误。

```csharp
using System;
using System.Text;

public class StringWrapper
{
   string internalString;
   StringBuilder internalSB = null;
   bool useSB = false;

   public StringWrapper(StringOperationType type)
   {
      if (type == StringOperationType.Normal) {
         useSB = false;
      }
      else {
         useSB = true;
         internalSB = new StringBuilder();
      }
   }

   // The remaining source code...
}

internal enum StringOperationType { Normal, Dynamic }
// The attempt to compile the example displays the following output:
//    error CS0051: Inconsistent accessibility: parameter type
//            'StringOperationType' is less accessible than method
//            'StringWrapper.StringWrapper(StringOperationType)'
```

```vb
Imports System.Text

<Assembly:CLSCompliant(True)>

Public Class StringWrapper

   Dim internalString As String
   Dim internalSB As StringBuilder = Nothing
   Dim useSB As Boolean = False

   Public Sub New(type As StringOperationType)
      If type = StringOperationType.Normal Then
         useSB = False
      Else
         internalSB = New StringBuilder()
         useSB = True
      End If
   End Sub

   ' The remaining source code...
End Class

Friend Enum StringOperationType As Integer
   Normal = 0
   Dynamic = 1
End Enum
' The attempt to compile the example displays the following output:
'    error BC30909: 'type' cannot expose type 'StringOperationType'
'     outside the project through class 'StringWrapper'.
'
'       Public Sub New(type As StringOperationType)
'                              ~~~~~~~~~~~~~~~~~~~
```

### <a name="generic-types-and-members"></a>泛型类型和成员

嵌套类型拥有的泛型参数数目总是至少与封闭类型的一样多。 它们按位置对应于封闭类型中的泛型参数。 泛型类型还可以包括新的泛型参数。

一个包含类型及其嵌套类型的泛型类型参数之间的关系可能由各种语言的语法隐藏。 在下面的示例中，泛型类型 `Outer<T>` 包含两个嵌套的类：`Inner1A` 和 `Inner1B<U>`。 对于每个类从 `ToString` 继承的 `Object.ToString` 方法的调用，表示每个嵌套的类包括其包含的类的类型参数。

```csharp
using System;

[assembly:CLSCompliant(true)]

public class Outer<T>
{
   T value;

   public Outer(T value)
   {
      this.value = value;
   }

   public class Inner1A : Outer<T>
   {
      public Inner1A(T value) : base(value)
      {  }
   }

   public class Inner1B<U> : Outer<T>
   {
      U value2;

      public Inner1B(T value1, U value2) : base(value1)
      {
         this.value2 = value2;
      }
   }
}

public class Example
{
   public static void Main()
   {
      var inst1 = new Outer<String>("This");
      Console.WriteLine(inst1);

      var inst2 = new Outer<String>.Inner1A("Another");
      Console.WriteLine(inst2);

      var inst3 = new Outer<String>.Inner1B<int>("That", 2);
      Console.WriteLine(inst3);
   }
}
// The example displays the following output:
//       Outer`1[System.String]
//       Outer`1+Inner1A[System.String]
//       Outer`1+Inner1B`1[System.String,System.Int32]
```

```vb
<Assembly:CLSCompliant(True)>

Public Class Outer(Of T)
   Dim value As T

   Public Sub New(value As T)
      Me.value = value
   End Sub

   Public Class Inner1A : Inherits Outer(Of T)
      Public Sub New(value As T)
         MyBase.New(value)
      End Sub
   End Class

   Public Class Inner1B(Of U) : Inherits Outer(Of T)
      Dim value2 As U

      Public Sub New(value1 As T, value2 As U)
         MyBase.New(value1)
         Me.value2 = value2
      End Sub
   End Class
End Class

Public Module Example
   Public Sub Main()
      Dim inst1 As New Outer(Of String)("This")
      Console.WriteLine(inst1)

      Dim inst2 As New Outer(Of String).Inner1A("Another")
      Console.WriteLine(inst2)

      Dim inst3 As New Outer(Of String).Inner1B(Of Integer)("That", 2)
      Console.WriteLine(inst3)
   End Sub
End Module
' The example displays the following output:
'       Outer`1[System.String]
'       Outer`1+Inner1A[System.String]
'       Outer`1+Inner1B`1[System.String,System.Int32]
```

泛型类型名称采用 *name*'*n* 格式进行编码，其中 *name* 是类型名称， *`* 是字符文本，*n* 是在类型上声明的参数数目，或对于嵌套泛型类型为最近引入的类型参数的数目。 此泛型类型名称的编码主要对使用反射来访问库中符合 CLS 的泛型类型的开发人员很有用。

如果将约束应用于泛型类型，则任何用作约束的类型也必须符合 CLS。 下面的示例定义一个名为 `BaseClass` 的不符合 CLS 的类和一个其类型参数必须派生自 `BaseCollection` 的名为 `BaseClass` 的泛型类。 但由于 `BaseClass` 不符合 CLS，因此编译器会发出警告。

```csharp
using System;

[assembly:CLSCompliant(true)]

[CLSCompliant(false)] public class BaseClass
{}

public class BaseCollection<T> where T : BaseClass
{}
// Attempting to compile the example displays the following output:
//    warning CS3024: Constraint type 'BaseClass' is not CLS-compliant
```

```vb
Assembly: CLSCompliant(True)>

<CLSCompliant(False)> Public Class BaseClass
End Class

Public Class BaseCollection(Of T As BaseClass)
End Class
' Attempting to compile the example displays the following output:
'    warning BC40040: Generic parameter constraint type 'BaseClass' is not
'    CLS-compliant.
'
'    Public Class BaseCollection(Of T As BaseClass)
'                                        ~~~~~~~~~
```

如果泛型类型派生自泛型基本类型，则其必须重新声明所有约束，以确保也满足对基本类型的约束。 下面的示例定义可表示任何数值类型的 `Number<T>`。 它还定义表示浮点值的 `FloatingPoint<T>` 类。 但是，源代码无法编译，因为它未将 `Number<T>` 上（T 必须是值类型）的约束应用于 `FloatingPoint<T>`。

```csharp
using System;

[assembly:CLSCompliant(true)]

public class Number<T> where T : struct
{
   // use Double as the underlying type, since its range is a superset of
   // the ranges of all numeric types except BigInteger.
   protected double number;

   public Number(T value)
   {
      try {
         this.number = Convert.ToDouble(value);
      }
      catch (OverflowException e) {
         throw new ArgumentException("value is too large.", e);
      }
      catch (InvalidCastException e) {
         throw new ArgumentException("The value parameter is not numeric.", e);
      }
   }

   public T Add(T value)
   {
      return (T) Convert.ChangeType(number + Convert.ToDouble(value), typeof(T));
   }

   public T Subtract(T value)
   {
      return (T) Convert.ChangeType(number - Convert.ToDouble(value), typeof(T));
   }
}

public class FloatingPoint<T> : Number<T>
{
   public FloatingPoint(T number) : base(number)
   {
      if (typeof(float) == number.GetType() ||
          typeof(double) == number.GetType() ||
          typeof(decimal) == number.GetType())
         this.number = Convert.ToDouble(number);
      else
         throw new ArgumentException("The number parameter is not a floating-point number.");
   }
}
// The attempt to compile the example displays the following output:
//       error CS0453: The type 'T' must be a non-nullable value type in
//               order to use it as parameter 'T' in the generic type or method 'Number<T>'
```

```vb
<Assembly:CLSCompliant(True)>

Public Class Number(Of T As Structure)
   ' Use Double as the underlying type, since its range is a superset of
   ' the ranges of all numeric types except BigInteger.
   Protected number As Double

   Public Sub New(value As T)
      Try
         Me.number = Convert.ToDouble(value)
      Catch e As OverflowException
         Throw New ArgumentException("value is too large.", e)
      Catch e As InvalidCastException
         Throw New ArgumentException("The value parameter is not numeric.", e)
      End Try
   End Sub

   Public Function Add(value As T) As T
      Return CType(Convert.ChangeType(number + Convert.ToDouble(value), GetType(T)), T)
   End Function

   Public Function Subtract(value As T) As T
      Return CType(Convert.ChangeType(number - Convert.ToDouble(value), GetType(T)), T)
   End Function
End Class

Public Class FloatingPoint(Of T) : Inherits Number(Of T)
   Public Sub New(number As T)
      MyBase.New(number)
      If TypeOf number Is Single Or
               TypeOf number Is Double Or
               TypeOf number Is Decimal Then
         Me.number = Convert.ToDouble(number)
      Else
         throw new ArgumentException("The number parameter is not a floating-point number.")
      End If
   End Sub
End Class
' The attempt to compile the example displays the following output:
'    error BC32105: Type argument 'T' does not satisfy the 'Structure'
'    constraint for type parameter 'T'.
'
'    Public Class FloatingPoint(Of T) : Inherits Number(Of T)
'                                                          ~
```

如果将此约束添加到 `FloatingPoint<T>` 类中，则该示例成功编译。

```csharp
using System;

[assembly:CLSCompliant(true)]

public class Number<T> where T : struct
{
   // use Double as the underlying type, since its range is a superset of
   // the ranges of all numeric types except BigInteger.
   protected double number;

   public Number(T value)
   {
      try {
         this.number = Convert.ToDouble(value);
      }
      catch (OverflowException e) {
         throw new ArgumentException("value is too large.", e);
      }
      catch (InvalidCastException e) {
         throw new ArgumentException("The value parameter is not numeric.", e);
      }
   }

   public T Add(T value)
   {
      return (T) Convert.ChangeType(number + Convert.ToDouble(value), typeof(T));
   }

   public T Subtract(T value)
   {
      return (T) Convert.ChangeType(number - Convert.ToDouble(value), typeof(T));
   }
}

public class FloatingPoint<T> : Number<T> where T : struct
{
   public FloatingPoint(T number) : base(number)
   {
      if (typeof(float) == number.GetType() ||
          typeof(double) == number.GetType() ||
          typeof(decimal) == number.GetType())
         this.number = Convert.ToDouble(number);
      else
         throw new ArgumentException("The number parameter is not a floating-point number.");
   }
}
```

```vb
<Assembly:CLSCompliant(True)>

Public Class Number(Of T As Structure)
   ' Use Double as the underlying type, since its range is a superset of
   ' the ranges of all numeric types except BigInteger.
   Protected number As Double

   Public Sub New(value As T)
      Try
         Me.number = Convert.ToDouble(value)
      Catch e As OverflowException
         Throw New ArgumentException("value is too large.", e)
      Catch e As InvalidCastException
         Throw New ArgumentException("The value parameter is not numeric.", e)
      End Try
   End Sub

   Public Function Add(value As T) As T
      Return CType(Convert.ChangeType(number + Convert.ToDouble(value), GetType(T)), T)
   End Function

   Public Function Subtract(value As T) As T
      Return CType(Convert.ChangeType(number - Convert.ToDouble(value), GetType(T)), T)
   End Function
End Class

Public Class FloatingPoint(Of T As Structure) : Inherits Number(Of T)
   Public Sub New(number As T)
      MyBase.New(number)
      If TypeOf number Is Single Or
               TypeOf number Is Double Or
               TypeOf number Is Decimal Then
         Me.number = Convert.ToDouble(number)
      Else
         throw new ArgumentException("The number parameter is not a floating-point number.")
      End If
   End Sub
End Class
```

公共语言规范对嵌套类型和受保护成员规定了一个保守的按实例化模型。 开放式泛型类型不能公开具有包含嵌套的、受保护的泛型类型的特定实例化的签名的字段或成员。 扩大泛型基类或接口的特定实例化的非泛型类型不能公开带签名的字段或成员，此类字段或成员包含嵌套的、受保护的泛型类型的不同实例化。

下面的示例定义一个泛型类型 `C1<T>` 和一个受保护的类 `C1<T>.N`。 `C1<T>` 有两种方法：`M1` 和 `M2`。 但是，`M1` 并不符合 CLS，因为它尝试从 `C1<T>` 返回 `C1<int>.N` 对象。 另一个名为 `C2` 的类派生自 `C1<long>`。 它有两种方法：`M3` 和 `M4`。 `M3` 不符合 CLS，因为它尝试从 `C1<long>` 的子类中返回 `C1<int>.N` 对象。 语言编译器可能具有更高限制。 在此示例中，Visual Basic 在尝试编译 `M4` 时显示错误。

```csharp
using System;

[assembly:CLSCompliant(true)]

public class C1<T>
{
   protected class N { }

   protected void M1(C1<int>.N n) { } // Not CLS-compliant - C1<int>.N not
                                      // accessible from within C1<T> in all
                                      // languages
   protected void M2(C1<T>.N n) { }   // CLS-compliant – C1<T>.N accessible
                                      // inside C1<T>
}

public class C2 : C1<long>
{
   protected void M3(C1<int>.N n) { }  // Not CLS-compliant – C1<int>.N is not
                                       // accessible in C2 (extends C1<long>)

   protected void M4(C1<long>.N n) { } // CLS-compliant, C1<long>.N is
                                       // accessible in C2 (extends C1<long>)
}
// Attempting to compile the example displays output like the following:
//       Generics4.cs(9,22): warning CS3001: Argument type 'C1<int>.N' is not CLS-compliant
//       Generics4.cs(18,22): warning CS3001: Argument type 'C1<int>.N' is not CLS-compliant
```

```vb
<Assembly:CLSCompliant(True)>

Public Class C1(Of T)
   Protected Class N
   End Class

   Protected Sub M1(n As C1(Of Integer).N)   ' Not CLS-compliant - C1<int>.N not
                                             ' accessible from within C1(Of T) in all
   End Sub                                   ' languages

   Protected Sub M2(n As C1(Of T).N)     ' CLS-compliant – C1(Of T).N accessible
   End Sub                               ' inside C1(Of T)
End Class

Public Class C2 : Inherits C1(Of Long)
   Protected Sub M3(n As C1(Of Integer).N)   ' Not CLS-compliant – C1(Of Integer).N is not
   End Sub                                   ' accessible in C2 (extends C1(Of Long))

   Protected Sub M4(n As C1(Of Long).N)
   End Sub
End Class
' Attempting to compile the example displays output like the following:
'    error BC30508: 'n' cannot expose type 'C1(Of Integer).N' in namespace
'    '<Default>' through class 'C1'.
'
'       Protected Sub M1(n As C1(Of Integer).N)   ' Not CLS-compliant - C1<int>.N not
'                             ~~~~~~~~~~~~~~~~
'    error BC30389: 'C1(Of T).N' is not accessible in this context because
'    it is 'Protected'.
'
'       Protected Sub M3(n As C1(Of Integer).N)   ' Not CLS-compliant - C1(Of Integer).N is not
'
'                             ~~~~~~~~~~~~~~~~
'
'    error BC30389: 'C1(Of T).N' is not accessible in this context because it is 'Protected'.
'
'       Protected Sub M4(n As C1(Of Long).N)
'                             ~~~~~~~~~~~~~
```

### <a name="constructors"></a>构造函数

符合 CLS 的类和结构中的构造函数必须遵循下列规则：

* 派生类的构造函数必须先调用其基类的实例构造函数，然后才能访问继承的实例数据。 存在此要求是因为基类构造函数并不由它们的派生类继承。 此规则不适用于不支持直接继承的结构。

  通常，编译器独立地强制实施 CLS 遵从性的此规则，如下面的示例所示。 它创建派生自 `Doctor` 类的 `Person` 类，但 `Doctor` 类无法调用 `Person` 类构造函数来初始化继承的实例字段。

    ```csharp
    using System;

    [assembly: CLSCompliant(true)]

    public class Person
    {
    private string fName, lName, _id;

    public Person(string firstName, string lastName, string id)
    {
        if (String.IsNullOrEmpty(firstName + lastName))
            throw new ArgumentNullException("Either a first name or a last name must be provided.");

        fName = firstName;
        lName = lastName;
        _id = id;
    }

    public string FirstName
    {
        get { return fName; }
    }

    public string LastName
    {
        get { return lName; }
    }

    public string Id
    {
        get { return _id; }
    }

    public override string ToString()
    {
        return String.Format("{0}{1}{2}", fName,
                            String.IsNullOrEmpty(fName) ?  "" : " ",
                            lName);
    }
    }

    public class Doctor : Person
    {
    public Doctor(string firstName, string lastName, string id)
    {
    }

    public override string ToString()
    {
        return "Dr. " + base.ToString();
    }
    }
    // Attempting to compile the example displays output like the following:
    //    ctor1.cs(45,11): error CS1729: 'Person' does not contain a constructor that takes 0
    //            arguments
    //    ctor1.cs(10,11): (Location of symbol related to previous error)
    ```

    ```vb
    <Assembly: CLSCompliant(True)>

    Public Class Person
       Private fName, lName, _id As String

       Public Sub New(firstName As String, lastName As String, id As String)
          If String.IsNullOrEmpty(firstName + lastName) Then
             Throw New ArgumentNullException("Either a first name or a last name must be provided.")
          End If

          fName = firstName
          lName = lastName
          _id = id
       End Sub

       Public ReadOnly Property FirstName As String
          Get
             Return fName
          End Get
       End Property

       Public ReadOnly Property LastName As String
          Get
             Return lName
          End Get
       End Property

       Public ReadOnly Property Id As String
          Get
             Return _id
          End Get
       End Property

       Public Overrides Function ToString() As String
          Return String.Format("{0}{1}{2}", fName,
                               If(String.IsNullOrEmpty(fName), "", " "),
                               lName)
       End Function
    End Class

    Public Class Doctor : Inherits Person
       Public Sub New(firstName As String, lastName As String, id As String)
       End Sub

       Public Overrides Function ToString() As String
          Return "Dr. " + MyBase.ToString()
       End Function
    End Class
    ' Attempting to compile the example displays output like the following:
    '    Ctor1.vb(46) : error BC30148: First statement of this 'Sub New' must be a call
    '    to 'MyBase.New' or 'MyClass.New' because base class 'Person' of 'Doctor' does
    '    not have an accessible 'Sub New' that can be called with no arguments.
    '
    '       Public Sub New()
    '                  ~~~
    ````

* 只有在创建对象时才能调用对象构造函数。 此外，不能将一个对象初始化两次。 例如，这意味着 `Object.MemberwiseClone` 不得调用构造函数。

### <a name="properties"></a>属性

符合 CLS 的类型的属性必须遵循下列规则：

* 属性必须具有 setter 和/或 getter。 在程序集中，这些作为特殊方法实现，这意味着它们将显示为单独的方法（getter 命名为 `get`\_*propertyname*，setter 命名为 `set`\_*propertyname*），且在程序集元数据中标记为 `SpecialName`。 C# 编译器会自动实施此规则，无需应用 <xref:System.CLSCompliantAttribute> 特性。

* 属性的类型是属性 getter 的返回类型和 setter 的最后一个自变量。 这些类型必须符合 CLS，并且不能通过引用将参数分配到属性中（即它们不能为托管指针）。

* 如果属性包含 getter 和 setter 两者，则它们必须都是虚拟的、静态的或实例。 C# 编译器通过它们的属性定义语法自动实施此规则。

### <a name="events"></a>事件

事件由其名称和类型定义。 事件类型是用于指示事件的委托。 例如，`DbConnection.StateChange` 事件的类型为 `StateChangeEventHandler`。 除事件本身外，带有基于事件名称的名称的三种方法提供事件的实现并在程序集的元数据中标记为 `SpecialName`：

* 用于添加事件处理程序的名为 `add`_ *EventName* 的方法。 例如，`DbConnection.StateChange` 事件的事件订阅方法名为 `add_StateChange`。

* 用于移除事件处理程序的名为 `remove`_ *EventName* 的方法。 例如，`DbConnection.StateChange` 事件的移除方法名为 `remove_StateChange`。

* 用于指示事件已发生的名为 `raise`\_EventName 的方法。

> [!NOTE]
> 大多数关于事件的公共语言规范的规则都通过语言编译器实施，且对组件开发人员是透明的。

用于添加、移除和引发事件的方法必须拥有相同的可访问性。 它们还必须都为静态、实例或虚拟的。 用于添加和移除事件的方法具有一个类型为事件委托类型的参数。 添加和移除方法必须同时存在或同时不存在。

如果两个读取之间的温度更改等于或超过阈值，则下面的示例将定义一个名为 `Temperature` 的类，该类会引发 `TemperatureChanged` 事件且符合 CLS。 `Temperature` 类显式定义 `raise_TemperatureChanged` 方法，以便其可以有选择地执行事件处理程序。

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

[assembly: CLSCompliant(true)]

public class TemperatureChangedEventArgs : EventArgs
{
   private Decimal originalTemp;
   private Decimal newTemp;
   private DateTimeOffset when;

   public TemperatureChangedEventArgs(Decimal original, Decimal @new, DateTimeOffset time)
   {
      originalTemp = original;
      newTemp = @new;
      when = time;
   }

   public Decimal OldTemperature
   {
      get { return originalTemp; }
   }

   public Decimal CurrentTemperature
   {
      get { return newTemp; }
   }

   public DateTimeOffset Time
   {
      get { return when; }
   }
}

public delegate void TemperatureChanged(Object sender, TemperatureChangedEventArgs e);

public class Temperature
{
   private struct TemperatureInfo
   {
      public Decimal Temperature;
      public DateTimeOffset Recorded;
   }

   public event TemperatureChanged TemperatureChanged;

   private Decimal previous;
   private Decimal current;
   private Decimal tolerance;
   private List<TemperatureInfo> tis = new List<TemperatureInfo>();

   public Temperature(Decimal temperature, Decimal tolerance)
   {
      current = temperature;
      TemperatureInfo ti = new TemperatureInfo();
      ti.Temperature = temperature;
      tis.Add(ti);
      ti.Recorded = DateTimeOffset.UtcNow;
      this.tolerance = tolerance;
   }

   public Decimal CurrentTemperature
   {
      get { return current; }
      set {
         TemperatureInfo ti = new TemperatureInfo();
         ti.Temperature = value;
         ti.Recorded = DateTimeOffset.UtcNow;
         previous = current;
         current = value;
         if (Math.Abs(current - previous) >= tolerance)
            raise_TemperatureChanged(new TemperatureChangedEventArgs(previous, current, ti.Recorded));
      }
   }

   public void raise_TemperatureChanged(TemperatureChangedEventArgs eventArgs)
   {
      if (TemperatureChanged == null)
         return;

      foreach (TemperatureChanged d in TemperatureChanged.GetInvocationList()) {
         if (d.Method.Name.Contains("Duplicate"))
            Console.WriteLine("Duplicate event handler; event handler not executed.");
         else
            d.Invoke(this, eventArgs);
      }
   }
}

public class Example
{
   public Temperature temp;

   public static void Main()
   {
      Example ex = new Example();
   }

   public Example()
   {
      temp = new Temperature(65, 3);
      temp.TemperatureChanged += this.TemperatureNotification;
      RecordTemperatures();
      Example ex = new Example(temp);
      ex.RecordTemperatures();
   }

   public Example(Temperature t)
   {
      temp = t;
      RecordTemperatures();
   }

   public void RecordTemperatures()
   {
      temp.TemperatureChanged += this.DuplicateTemperatureNotification;
      temp.CurrentTemperature = 66;
      temp.CurrentTemperature = 63;
   }

   internal void TemperatureNotification(Object sender, TemperatureChangedEventArgs e)
   {
      Console.WriteLine("Notification 1: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature);
   }

   public void DuplicateTemperatureNotification(Object sender, TemperatureChangedEventArgs e)
   {
      Console.WriteLine("Notification 2: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature);
   }
}
```

```vb
Imports System.Collections
Imports System.Collections.Generic

<Assembly: CLSCompliant(True)>

Public Class TemperatureChangedEventArgs   : Inherits EventArgs
   Private originalTemp As Decimal
   Private newTemp As Decimal
   Private [when] As DateTimeOffset

   Public Sub New(original As Decimal, [new] As Decimal, [time] As DateTimeOffset)
      originalTemp = original
      newTemp = [new]
      [when] = [time]
   End Sub

   Public ReadOnly Property OldTemperature As Decimal
      Get
         Return originalTemp
      End Get
   End Property

   Public ReadOnly Property CurrentTemperature As Decimal
      Get
         Return newTemp
      End Get
   End Property

   Public ReadOnly Property [Time] As DateTimeOffset
      Get
         Return [when]
      End Get
   End Property
End Class

Public Delegate Sub TemperatureChanged(sender As Object, e As TemperatureChangedEventArgs)

Public Class Temperature
   Private Structure TemperatureInfo
      Dim Temperature As Decimal
      Dim Recorded As DateTimeOffset
   End Structure

   Public Event TemperatureChanged As TemperatureChanged

   Private previous As Decimal
   Private current As Decimal
   Private tolerance As Decimal
   Private tis As New List(Of TemperatureInfo)

   Public Sub New(temperature As Decimal, tolerance As Decimal)
      current = temperature
      Dim ti As New TemperatureInfo()
      ti.Temperature = temperature
      ti.Recorded = DateTimeOffset.UtcNow
      tis.Add(ti)
      Me.tolerance = tolerance
   End Sub

   Public Property CurrentTemperature As Decimal
      Get
         Return current
      End Get
      Set
         Dim ti As New TemperatureInfo
         ti.Temperature = value
         ti.Recorded = DateTimeOffset.UtcNow
         previous = current
         current = value
         If Math.Abs(current - previous) >= tolerance Then
            raise_TemperatureChanged(New TemperatureChangedEventArgs(previous, current, ti.Recorded))
         End If
      End Set
   End Property

   Public Sub raise_TemperatureChanged(eventArgs As TemperatureChangedEventArgs)
      If TemperatureChangedEvent Is Nothing Then Exit Sub

      Dim ListenerList() As System.Delegate = TemperatureChangedEvent.GetInvocationList()
      For Each d As TemperatureChanged In TemperatureChangedEvent.GetInvocationList()
         If d.Method.Name.Contains("Duplicate") Then
            Console.WriteLine("Duplicate event handler; event handler not executed.")
         Else
            d.Invoke(Me, eventArgs)
         End If
      Next
   End Sub
End Class

Public Class Example
   Public WithEvents temp As Temperature

   Public Shared Sub Main()
      Dim ex As New Example()
   End Sub

   Public Sub New()
      temp = New Temperature(65, 3)
      RecordTemperatures()
      Dim ex As New Example(temp)
      ex.RecordTemperatures()
   End Sub

   Public Sub New(t As Temperature)
      temp = t
      RecordTemperatures()
   End Sub

   Public Sub RecordTemperatures()
      temp.CurrentTemperature = 66
      temp.CurrentTemperature = 63

   End Sub

   Friend Shared Sub TemperatureNotification(sender As Object, e As TemperatureChangedEventArgs) _
          Handles temp.TemperatureChanged
      Console.WriteLine("Notification 1: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature)
   End Sub

   Friend Shared Sub DuplicateTemperatureNotification(sender As Object, e As TemperatureChangedEventArgs) _
          Handles temp.TemperatureChanged
      Console.WriteLine("Notification 2: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature)
   End Sub
End Class
```

### <a name="overloads"></a>Overloads

公共语言规范对重载成员有下列要求：

* 成员可以根据参数数量和任何参数的类型进行重载。 在重载间进行区分时，不考虑应用于方法或其参数的调用约定、返回类型、自定义修饰符，也不考虑是按照值还是引用传递参数。 有关示例，请参阅[命名约定](#naming-conventions)部分中名称在范围内必须是唯一的代码需求。

* 只可重载属性和方法。 无法重载字段和事件。

* 泛型方法可以基于其泛型参数的数目进行重载。

> [!NOTE]
> `op_Explicit` 和 `op_Implicit` 运算符是返回值不被视为重载决策的方法签名的一部分的规则的例外情况。 可以基于这两个运算符的参数和返回值对其进行重载。

### <a name="exceptions"></a>异常

异常对象必须从 [System.Exception](xref:System.Exception) 派生，或从派生自 `System.Exception` 的另一个类型派生。 下面的示例说明了当名为 `ErrorClass` 的自定义类用于异常处理时产生的编译器错误。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ErrorClass
{
   string msg;

   public ErrorClass(string errorMessage)
   {
      msg = errorMessage;
   }

   public string Message
   {
      get { return msg; }
   }
}

public static class StringUtilities
{
   public static string[] SplitString(this string value, int index)
   {
      if (index < 0 | index > value.Length) {
         ErrorClass badIndex = new ErrorClass("The index is not within the string.");
         throw badIndex;
      }
      string[] retVal = { value.Substring(0, index - 1),
                          value.Substring(index) };
      return retVal;
   }
}
// Compilation produces a compiler error like the following:
//    Exceptions1.cs(26,16): error CS0155: The type caught or thrown must be derived from
//            System.Exception
```

```vb
Imports System.Runtime.CompilerServices

<Assembly: CLSCompliant(True)>

Public Class ErrorClass
   Dim msg As String

   Public Sub New(errorMessage As String)
      msg = errorMessage
   End Sub

   Public ReadOnly Property Message As String
      Get
         Return msg
      End Get
   End Property
End Class

Public Module StringUtilities
   <Extension()> Public Function SplitString(value As String, index As Integer) As String()
      If index < 0 Or index > value.Length Then
         Dim BadIndex As New ErrorClass("The index is not within the string.")
         Throw BadIndex
      End If
      Dim retVal() As String = { value.Substring(0, index - 1),
                                 value.Substring(index) }
      Return retVal
   End Function
End Module
' Compilation produces a compiler error like the following:
'    Exceptions1.vb(27) : error BC30665: 'Throw' operand must derive from 'System.Exception'.
'
'             Throw BadIndex
'             ~~~~~~~~~~~~~~
```

若要更正此错误，`ErrorClass` 类必须继承自 `System.Exception`。 此外，必须重写 Message 属性。 下面的示例更正这些错误以定义符合 CLS 的 `ErrorClass` 类。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ErrorClass : Exception
{
   string msg;

   public ErrorClass(string errorMessage)
   {
      msg = errorMessage;
   }

   public override string Message
   {
      get { return msg; }
   }
}

public static class StringUtilities
{
   public static string[] SplitString(this string value, int index)
   {
      if (index < 0 | index > value.Length) {
         ErrorClass badIndex = new ErrorClass("The index is not within the string.");
         throw badIndex;
      }
      string[] retVal = { value.Substring(0, index - 1),
                          value.Substring(index) };
      return retVal;
   }
}
```

```vb
Imports System.Runtime.CompilerServices

<Assembly: CLSCompliant(True)>

Public Class ErrorClass : Inherits Exception
   Dim msg As String

   Public Sub New(errorMessage As String)
      msg = errorMessage
   End Sub

   Public Overrides ReadOnly Property Message As String
      Get
         Return msg
      End Get
   End Property
End Class

Public Module StringUtilities
   <Extension()> Public Function SplitString(value As String, index As Integer) As String()
      If index < 0 Or index > value.Length Then
         Dim BadIndex As New ErrorClass("The index is not within the string.")
         Throw BadIndex
      End If
      Dim retVal() As String = { value.Substring(0, index - 1),
                                 value.Substring(index) }
      Return retVal
   End Function
End Module
```

### <a name="attributes"></a>特性

在 .NET 程序集中，自定义特性提供了一个可扩展机制，用于存储自定义特性和检索有关编程对象（如程序集、类型、成员和方法参数）的元数据。 自定义特性必须从 [System.Attribute](xref:System.Attribute) 派生或从派生自 `System.Attribute` 的类型派生。

下面的示例与此规则冲突。 它定义了不是从 `NumericAttribute` 派生的 `System.Attribute` 类。 编译器错误仅当应用不符合 CLS 的特性时会出现，而在定义类时不会出现。

```csharp
using System;

[assembly: CLSCompliant(true)]

[AttributeUsageAttribute(AttributeTargets.Class | AttributeTargets.Struct)]
public class NumericAttribute
{
   private bool _isNumeric;

   public NumericAttribute(bool isNumeric)
   {
      _isNumeric = isNumeric;
   }

   public bool IsNumeric
   {
      get { return _isNumeric; }
   }
}

[Numeric(true)] public struct UDouble
{
   double Value;
}
// Compilation produces a compiler error like the following:
//    Attribute1.cs(22,2): error CS0616: 'NumericAttribute' is not an attribute class
//    Attribute1.cs(7,14): (Location of symbol related to previous error)
```

```vb
<Assembly: CLSCompliant(True)>

<AttributeUsageAttribute(AttributeTargets.Class Or AttributeTargets.Struct)> _
Public Class NumericAttribute
   Private _isNumeric As Boolean

   Public Sub New(isNumeric As Boolean)
      _isNumeric = isNumeric
   End Sub

   Public ReadOnly Property IsNumeric As Boolean
      Get
         Return _isNumeric
      End Get
   End Property
End Class

<Numeric(True)> Public Structure UDouble
   Dim Value As Double
End Structure
' Compilation produces a compiler error like the following:
'    error BC31504: 'NumericAttribute' cannot be used as an attribute because it
'    does not inherit from 'System.Attribute'.
'
'    <Numeric(True)> Public Structure UDouble
'     ~~~~~~~~~~~~~
```

构造函数或符合 CLS 的特性的属性只能公开以下类型：

* [布尔值](xref:System.Boolean)

* [Byte](xref:System.Byte)

* [Char](xref:System.Char)

* [双精度](xref:System.Double)

* [Int16](xref:System.Int16)

* [Int32](xref:System.Int32)

* [Int64](xref:System.Int64)

* [单精度](xref:System.Single)

* [字符串](xref:System.String)

* [Type](xref:System.Type)

* 基础类型为 `Byte`、`Int16`、`Int32` 或 `Int64` 的任意枚举类型。

下面的示例定义了从 [Attribute](xref:System.Attribute) 派生的 `DescriptionAttribute` 类。 类构造函数具有 `Descriptor` 类型的参数，因此，该类不符合 CLS。 C# 编译器会发出警告，但编译成功。

```csharp
using System;

[assembly:CLSCompliantAttribute(true)]

public enum DescriptorType { type, member };

public class Descriptor
{
   public DescriptorType Type;
   public String Description;
}

[AttributeUsage(AttributeTargets.All)]
public class DescriptionAttribute : Attribute
{
   private Descriptor desc;

   public DescriptionAttribute(Descriptor d)
   {
      desc = d;
   }

   public Descriptor Descriptor
   { get { return desc; } }
}
// Attempting to compile the example displays output like the following:
//       warning CS3015: 'DescriptionAttribute' has no accessible
//               constructors which use only CLS-compliant types
```

```vb
<Assembly:CLSCompliantAttribute(True)>

Public Enum DescriptorType As Integer
   Type = 0
   Member = 1
End Enum

Public Class Descriptor
   Public Type As DescriptorType
   Public Description As String
End Class

<AttributeUsage(AttributeTargets.All)> _
Public Class DescriptionAttribute : Inherits Attribute
   Private desc As Descriptor

   Public Sub New(d As Descriptor)
      desc = d
   End Sub

   Public ReadOnly Property Descriptor As Descriptor
      Get
         Return desc
      End Get
   End Property
End Class
```

## <a name="the-clscompliantattribute-attribute"></a>CLSCompliantAttribute 特性

[CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 特性用于指示程序元素是否使用公共语言规范进行编译。 `CLSCompliantAttribute.CLSCompliantAttribute(Boolean)` 构造函数包含一个必需的参数 *isCompliant*，此参数指示该程序元素是否符合 CLS。

编译时，编译器检测到假定为符合 CLS 的不合规元素，并发出警告。 编译器不会对显式声明为不符合标准的类型或成员发出警告。

组件开发人员可以通过两种方法使用 `CLSCompliantAttribute` 特性：

* 定义由组件公开的公共接口的符合 CLS 的部件以及不符合 CLS 的部件。 如果特性用于将特定程序元素标记为符合 CLS，则使用该特性可保证能通过面向 .NET 的所有语言和工具访问这些元素。

* 确保组件库的公共接口仅公开符合 CLS 的程序元素。 如果元素不符合 CLS，则编译器通常会发出一个警告。

> [!WARNING]
> 在某些情况下，语言编译器执行符合 CLS 的规则，而不管是否使用 `CLSCompliantAttribute` 特性。 例如，在接口中定义 `*static` 成员会违反 CLS 规则。 但是，如果在接口中定义 `*static` 成员，C# 编译器会显示错误消息且无法编译应用。

`CLSCompliantAttribute` 特性标记为具有 `AttributeTargets.All` 值的 [AttributeUsageAttribute](xref:System.AttributeUsageAttribute) 特性。 利用此值，您可以将 `CLSCompliantAttribute` 特性应用于任何程序元素，包括程序集、模块、类型（类、结构、枚举、接口和委托）、类型成员（构造函数、方法、属性、字段和事件）、参数、泛型参数和返回值。 但实际上，您只应将该特性应用于程序集、类型和类型成员。 否则，编译器在库的公共接口中遇到不符合标准的参数、泛型参数或返回值时，将忽略此特性并继续生成编译器警告。

`CLSCompliantAttribute` 特性的值由包含的程序元素继承。 例如，如果程序集标记为符合 CLS，则其类型也符合 CLS。 如果类型标记为符合 CLS，则其嵌套的类型和成员也符合 CLS。

您可以通过将 `CLSCompliantAttribute` 特性应用到包含的编程元素来显式重写继承的遵从性。 例如，可以使用 isCompliant 值为 `false` 的 `CLSCompliantAttribute` 特性来定义符合标准的程序集中不符合标准的类型，还可以使用 isComplian 值为 `true` 的特性来定义不符合标准的程序集中符合标准的类型。 您还可以在符合标准的类型中定义不符合标准的成员。 但是，不符合标准的类型无法拥有符合标准的成员，因此无法使用 *isCompliant* 值为 `true` 的特性从一个不符合标准的类型重写继承。

在开发组件时，应始终使用 `CLSCompliantAttribute` 特性来指示您的程序集、其类型及其成员是否符合 CLS。

创建符合 CLS 的组件：

1. 使用 `CLSCompliantAttribute` 将程序集标记为符合 CLS。

2. 将程序集中不符合 CLS 的所有公开的类型标记为不符合标准。

3. 将符合 CLS 的类型中的所有公开的成员标记为不符合标准。

4. 为不符合 CLS 的成员提供符合 CLS 的替代项。

如果已成功标记所有不符合标准的类型和成员，您的编译器不会发出任何不符合警告。 但是，您应指出哪些成员不符合 CLS 并在产品文档中列出其不符合 CLS 的替代项。

下面的示例使用 `CLSCompliantAttribute` 特性定义符合 CLS 的程序集和类型 `CharacterUtilities`，该类型具有两个不符合 CLS 的成员。 由于这两个成员标记有 `CLSCompliant(false)` 特性，因此编译器不生成任何警告。 该类还为两种方法提供符合 CLS 的替代项。 通常，我们只向 `ToUTF16` 方法添加两个重载，以便提供符合 CLS 的替代项。 但是，由于无法基于返回值重载这些方法，因此符合 CLS 的方法的名称不同于不符合标准的方法的名称。

```csharp
using System;
using System.Text;

[assembly:CLSCompliant(true)]

public class CharacterUtilities
{
   [CLSCompliant(false)] public static ushort ToUTF16(String s)
   {
      s = s.Normalize(NormalizationForm.FormC);
      return Convert.ToUInt16(s[0]);
   }

   [CLSCompliant(false)] public static ushort ToUTF16(Char ch)
   {
      return Convert.ToUInt16(ch);
   }

   // CLS-compliant alternative for ToUTF16(String).
   public static int ToUTF16CodeUnit(String s)
   {
      s = s.Normalize(NormalizationForm.FormC);
      return (int) Convert.ToUInt16(s[0]);
   }

   // CLS-compliant alternative for ToUTF16(Char).
   public static int ToUTF16CodeUnit(Char ch)
   {
      return Convert.ToInt32(ch);
   }

   public bool HasMultipleRepresentations(String s)
   {
      String s1 = s.Normalize(NormalizationForm.FormC);
      return s.Equals(s1);
   }

   public int GetUnicodeCodePoint(Char ch)
   {
      if (Char.IsSurrogate(ch))
         throw new ArgumentException("ch cannot be a high or low surrogate.");

      return Char.ConvertToUtf32(ch.ToString(), 0);
   }

   public int GetUnicodeCodePoint(Char[] chars)
   {
      if (chars.Length > 2)
         throw new ArgumentException("The array has too many characters.");

      if (chars.Length == 2) {
         if (! Char.IsSurrogatePair(chars[0], chars[1]))
            throw new ArgumentException("The array must contain a low and a high surrogate.");
         else
            return Char.ConvertToUtf32(chars[0], chars[1]);
      }
      else {
         return Char.ConvertToUtf32(chars.ToString(), 0);
      }
   }
}
```

```vb
Imports System.Text

<Assembly:CLSCompliant(True)>

Public Class CharacterUtilities
   <CLSCompliant(False)> Public Shared Function ToUTF16(s As String) As UShort
      s = s.Normalize(NormalizationForm.FormC)
      Return Convert.ToUInt16(s(0))
   End Function

   <CLSCompliant(False)> Public Shared Function ToUTF16(ch As Char) As UShort
      Return Convert.ToUInt16(ch)
   End Function

   ' CLS-compliant alternative for ToUTF16(String).
   Public Shared Function ToUTF16CodeUnit(s As String) As Integer
      s = s.Normalize(NormalizationForm.FormC)
      Return CInt(Convert.ToInt16(s(0)))
   End Function

   ' CLS-compliant alternative for ToUTF16(Char).
   Public Shared Function ToUTF16CodeUnit(ch As Char) As Integer
      Return Convert.ToInt32(ch)
   End Function

   Public Function HasMultipleRepresentations(s As String) As Boolean
      Dim s1 As String = s.Normalize(NormalizationForm.FormC)
      Return s.Equals(s1)
   End Function

   Public Function GetUnicodeCodePoint(ch As Char) As Integer
      If Char.IsSurrogate(ch) Then
         Throw New ArgumentException("ch cannot be a high or low surrogate.")
      End If
      Return Char.ConvertToUtf32(ch.ToString(), 0)
   End Function

   Public Function GetUnicodeCodePoint(chars() As Char) As Integer
      If chars.Length > 2 Then
         Throw New ArgumentException("The array has too many characters.")
      End If
      If chars.Length = 2 Then
         If Not Char.IsSurrogatePair(chars(0), chars(1)) Then
            Throw New ArgumentException("The array must contain a low and a high surrogate.")
         Else
            Return Char.ConvertToUtf32(chars(0), chars(1))
         End If
      Else
         Return Char.ConvertToUtf32(chars.ToString(), 0)
      End If
   End Function
End Class
```

如果您开发的是应用程序而不是库（即，如果不公开可由其他应用程序开发人员使用的类型或成员），则只有在您的语言不支持程序元素时，您的应用程序使用的程序元素的 CLS 遵从性才会引起关注。 在这种情况下，当您尝试使用不符合 CLS 的元素时，您的语言编译器将生成错误。

## <a name="cross-language-interoperability"></a>跨语言互操作性

语言独立性可能有许多含义。 其中一种含义涉及到从使用一种语言编写的应用取用以另一种语言编写的类型。 本文介绍第二个含义，涉及将用多种语言编写的代码组合到一个 .NET 程序集。

以下示例通过创建一个名为 Utilities.dll 的包含两个类（`NumericLib` 和 `StringLib`）的类库演示了跨语言互操作性。 `NumericLib` 类用 C# 编写类，`StringLib` 类用 Visual Basic 编写。 以下是 `StringUtil.vb` 的源代码，该源代码在其 `StringLib` 类中包含单个成员 `ToTitleCase`。

```vb
Imports System.Collections.Generic
Imports System.Runtime.CompilerServices

Public Module StringLib
   Private exclusions As List(Of String)

   Sub New()
      Dim words() As String = { "a", "an", "and", "of", "the" }
      exclusions = New List(Of String)
      exclusions.AddRange(words)
   End Sub

   <Extension()> _
   Public Function ToTitleCase(title As String) As String
      Dim words() As String = title.Split()
      Dim result As String = String.Empty

      For ctr As Integer = 0 To words.Length - 1
         Dim word As String = words(ctr)
         If ctr = 0 OrElse Not exclusions.Contains(word.ToLower()) Then
            result += word.Substring(0, 1).ToUpper() + _
                      word.Substring(1).ToLower()
         Else
            result += word.ToLower()
         End If
         If ctr <= words.Length - 1 Then
            result += " "
         End If
      Next
      Return result
   End Function
End Module
```

以下是 NumberUtil.cs 的源代码，定义具有 `NumericLib` 和 `IsEven` 两个成员的 `NearZero` 类。

```csharp
using System;

public static class NumericLib
{
   public static bool IsEven(this IConvertible number)
   {
      if (number is Byte ||
          number is SByte ||
          number is Int16 ||
          number is UInt16 ||
          number is Int32 ||
          number is UInt32 ||
          number is Int64)
         return ((long) number) % 2 == 0;
      else if (number is UInt64)
         return ((ulong) number) %2 == 0;
      else
         throw new NotSupportedException("IsEven called for a non-integer value.");
   }

   public static bool NearZero(double number)
   {
      return number < .00001;
   }
}
```

若要将两个类打包到单个程序集中，必须将它们编译到模块中。 要将 Visual Basic 源代码文件编译到模块，请使用此命令：

```console
vbc /t:module StringUtil.vb
```

要将 C# 源代码文件编译到模块，请使用此命令：

```console
csc /t:module NumberUtil.cs
```

然后，使用链接工具 (Link.exe) 将两个模块编译到一个程序集：

```console
link numberutil.netmodule stringutil.netmodule /out:UtilityLib.dll /dll
```

然后以下实例调用 `NumericLib.NearZero` 和 `StringLib.ToTitleCase` 方法。 Visual Basic 代码和 C# 代码都能够访问这两个类中的方法。

```csharp
using System;

public class Example
{
   public static void Main()
   {
      Double dbl = 0.0 - Double.Epsilon;
      Console.WriteLine(NumericLib.NearZero(dbl));

      string s = "war and peace";
      Console.WriteLine(s.ToTitleCase());
   }
}
// The example displays the following output:
//       True
//       War and Peace
```

```vb
Module Example
   Public Sub Main()
      Dim dbl As Double = 0.0 - Double.Epsilon
      Console.WriteLine(NumericLib.NearZero(dbl))

      Dim s As String = "war and peace"
      Console.WriteLine(s.ToTitleCase())
   End Sub
End Module
' The example displays the following output:
'       True
'       War and Peace
```

要编译 Visual Basic 代码，请使用此命令：

```console
vbc example.vb /r:UtilityLib.dll
```

要使用 C# 进行编译，请将编译器的名称从 vbc 更改为 csc，将文件扩展名从 .vb 更改为 .cs：

```console
csc example.cs /r:UtilityLib.dll
```
