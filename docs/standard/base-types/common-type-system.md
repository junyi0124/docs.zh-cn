---
title: 常规类型系统
description: 了解 .NET 中的类型系统。 了解 .NET 中的类型（值类型或引用类型）、类型定义、类型成员和类型成员特征。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- type system
- common type system
- assemblies [.NET], types
- reference types
- value types
- cross-language interoperability
- namespaces [.NET], types
- types, about types
ms.assetid: 53c57c96-83e1-4ee3-9543-9ac832671a89
ms.openlocfilehash: 0f80be2d1da43341f8e2af6f32580be2e01289dc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723215"
---
# <a name="common-type-system"></a>通用类型系统

通用类型系统定义了如何在公共语言运行时中声明、使用和管理类型，同时也是运行时跨语言集成支持的一个重要组成部分。 常规类型系统执行以下功能：  
  
- 建立一个支持跨语言集成、类型安全和高性能代码执行的框架。  
  
- 提供一个支持完整实现多种编程语言的面向对象的模型。  
  
- 定义各语言必须遵守的规则，有助于确保用不同语言编写的对象能够交互作用。  
  
- 提供包含应用程序开发中使用的基元数据类型（如<xref:System.Boolean>、<xref:System.Byte>、<xref:System.Char>、<xref:System.Int32> 和 <xref:System.UInt64>）的库。
  
## <a name="types-in-net"></a>.NET 中的类型

 .NET 中的所有类型不是值类型就是引用类型。  
  
 值类型是使用对象实际值来表示对象的数据类型。 如果向一个变量分配值类型的实例，则该变量将被赋以该值的全新副本。  
  
 引用类型是使用对对象实际值的引用（类似于指针）来表示对象的数据类型。 如果为某个变量分配一个引用类型，则该变量将引用（或指向）原始值。 不创建任何副本。  
  
 .NET 中的通用类型系统支持以下五种类别的类型：  
  
- [类](#classes)  
  
- [结构](#structures)  
  
- [枚举](#enumerations)  
  
- [接口](#interfaces)  
  
- [委托](#delegates)  
  
### <a name="classes"></a>类

 类是可以直接从另一个类派生以及从 <xref:System.Object?displayProperty=nameWithType> 隐式派生的引用类型。 类定义对象（它是该类的实例）可以执行的操作（方法、事件或属性）和该对象包含的数据（字段）。 尽管类通常同时包含定义和实现（与接口不同，例如，接口只包含定义而不包含实现），但它也可以有一个或多个没有实现的成员。  
  
 下表介绍了类可以具有的一些特征。 支持运行时的每种语言都提供了一种方法，来指示类或类成员具有其中的一种或多种特征。 但是，针对 .NET 的各个编程语言不能使所有这些特征都可用。  
  
|特征|描述|  
|--------------------|-----------------|  
|sealed|指定不能从此类型派生出另一个类。|  
|实现|指出该类通过提供接口成员的实现，使用一个或多个接口。|  
|abstract|指出无法实例化类。 若要使用它，必须由其派生出另一个类。|  
|继承|指出可以在指定了基类的任何地方使用类的实例。 从基类继承的派生类可以使用基类提供的任何公共成员的实现，派生类也可以用自己的实现重写这些公共成员的实现。|  
|exported 或 not exported|指出某个类在定义它的程序集之外是否可见。 此特征仅适用于顶级类，不适用于嵌套类。|  
  
> [!NOTE]
> 类也可以嵌套在父类或结构中。 嵌套类也有成员特征。 有关详细信息，请参阅[嵌套类型](#nested-types)。  
  
 没有实现的类成员是抽象成员。 有一个或更多抽象成员的类其本身也是抽象的；不可以创建它的新实例。 以运行时为目标的某些语言允许将类标记为抽象类，即使其成员都不是抽象成员也是如此。 当要封装一组派生类可在适当时候继承或重写的基本功能时，可以使用抽象类。 非抽象的类称为具体类。  
  
 类可以实现任意数目的接口，但是它除了 <xref:System.Object?displayProperty=nameWithType>（所有类都可以隐式从它继承）之外，只能从一个基类继承。 所有的类都必须至少有一个构造函数，该函数初始化此类的新实例。 如果没有显式定义构造函数，大多数编译器将自动提供一个无参数构造函数。  
  
### <a name="structures"></a>结构

 结构是隐式从 <xref:System.ValueType?displayProperty=nameWithType> 派生的值类型，后者则是从 <xref:System.Object?displayProperty=nameWithType> 派生的。 对于表示内存要求较小的值以及将值作为按值参数传递给具有强类型参数的方法，结构很有用。 在 .NET 中，所有基元数据类型（<xref:System.Boolean>、<xref:System.Byte>、<xref:System.Char>、<xref:System.DateTime>、<xref:System.Decimal>、<xref:System.Double>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.SByte>、<xref:System.Single>、<xref:System.UInt16>、<xref:System.UInt32> 和 <xref:System.UInt64>）都定义为结构。  
  
 像类一样，结构同时定义数据（结构的字段）和可以对该数据执行的操作（结构的方法）。 这意味着可以对结构调用方法，包括在 <xref:System.Object?displayProperty=nameWithType> 和 <xref:System.ValueType?displayProperty=nameWithType> 类上定义的虚方法以及在值类型自身上定义的任意方法。 换句话说，结构可以具有字段、属性和事件以及静态和非静态方法。 您可以创建结构的实例，将它们作为参数传递，将它们存储为局部变量，或将它们存储在另一值类型或引用类型的字段中。 结构也可以实现接口。  
  
 值类型与类在一些方面也是不同的。 首先，尽管它们都从 <xref:System.ValueType?displayProperty=nameWithType> 隐式继承，但是它们不能从任何类型直接继承。 同样，所有值类型都是密封的，这意味着不能从它们派生出其他类型。 它们也不需要构造函数。  
  
 对于每一种值类型，公共语言运行时都提供一种相应的已装箱类型，它是与值类型有着相同状态和行为的类。 将值类型的实例传递到接受 <xref:System.Object?displayProperty=nameWithType> 类型的参数的方法时，会将它装箱。 当控件从接受值类型作为传引用参数的方法调用返回时，会将它拆箱（即它从类实例重新转换回值类型的实例）。 当需要已装箱的类型时，某些语言要求使用特殊的语法；而另外一些语言会在需要时自动使用已装箱的类型。 在定义值类型时，需要同时定义已装箱和未装箱的类型。  
  
### <a name="enumerations"></a>枚举

 枚举是一种值类型，该值类型直接从 <xref:System.Enum?displayProperty=nameWithType> 继承并为基础基元类型的值提供替代名称。 枚举类型具有一个名称、一个必须为某个内置带符号或不带符号的整数类型的基础类型（如 <xref:System.Byte>、<xref:System.Int32> 或 <xref:System.UInt64>）以及一组字段。 字段是静态文本字段，其中的每一个字段都表示常数。 同一个值可以分配给多个字段。 出现这种情况时，必须将其中某个值标记为主要枚举值，以便进行反射和字符串转换。  
  
 可以将基础类型的值分配给枚举，反之亦然（运行时不要求强制转换）。 可以创建枚举的实例，并调用 <xref:System.Enum?displayProperty=nameWithType> 的方法以及在枚举的基础类型上定义的任何方法。 但是，某些语言可能不允许在要求基础类型的实例时将枚举作为参数传递（反之亦然）。  
  
 对于枚举还有以下附加限制：  
  
- 它们不能定义自己的方法。  
  
- 它们不能实现接口。  
  
- 它们不能定义属性或事件。  
  
- 枚举不能是泛型，除非它嵌套在泛型类型中，才能是泛型。 也就是说，枚举不能有自己的类型参数。  
  
    > [!NOTE]
    > 用 Visual Basic、C# 和 C++ 创建的嵌套类型（包括枚举）包含所有封闭泛型类型的类型参数，因此即使这些嵌套类型没有自己的类型参数，它们也是泛型的。 有关更多信息，请参见 <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> 参考主题中的“嵌套类型”。  
  
 <xref:System.FlagsAttribute> 特性表示一种特殊的枚举，称为位域。 运行时本身不区分传统枚举与位域，但您的语言可能会区分二者。 当区分二者的时候，可以对位域（而不是枚举）使用位操作符以产生未命名的值。 枚举一般用于列出唯一的元素，如一周的各天、国家/地区名称，等等。 位域一般用于列出可能联合发生的质量或数量，比如 `Red And Big And Fast`。  
  
 下面的示例说明如何使用位域和传统枚举。  
  
 [!code-csharp[Conceptual.Types.Enum#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.enum/cs/example.cs#1)]
 [!code-vb[Conceptual.Types.Enum#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.enum/vb/example.vb#1)]  

### <a name="interfaces"></a>接口

 接口定义用于指定“可以执行”关系或“具有”关系的协定。 接口通常用于实现某种功能，如比较和排序（<xref:System.IComparable> 和 <xref:System.IComparable%601> 接口）、测试相等性（<xref:System.IEquatable%601> 接口）或枚举集合中的项（<xref:System.Collections.IEnumerable> 和 <xref:System.Collections.Generic.IEnumerable%601> 接口）。 接口可具有属性、方法和事件，所有这些都是抽象成员；也就是说，虽然接口定义这些成员及其签名，但每个接口成员的功能由实现该接口的类型定义。 这意味着实现接口的任何类或结构都必须为该接口中声明的抽象成员提供定义。 接口也可以要求任何实现类或结构实现一个或多个其他接口。  
  
 对接口有以下限制：  
  
- 接口可以用任何可访问性来声明，但接口成员必须全都具有公共可访问性。  
  
- 接口不能定义构造函数。  
  
- 接口不能定义字段。  
  
- 接口只能定义实例成员。 它们不能定义静态成员。  
  
 每种语言都必须提供映射规则，将实现映射到需要该成员的接口，因为多个接口可以使用同一个签名声明成员，而这些成员可以分别具有单独的实现。  

### <a name="delegates"></a>委托

 委托是用途类似于 C++ 中的函数指针的引用类型。 它们用于 .NET 中的事件处理程序和回调函数。 与函数指针不同，委托是安全、可验证和类型安全的。 委托类型可以表示任何具有兼容签名的实例方法或静态方法。  
  
 如果委托参数的类型的限制性强于方法参数的类型，则该委托的参数与该方法的相应参数兼容，因为这可保证传递给委托的参数可以安全地传递给方法。  
  
 同样，如果方法的返回类型的限制性强于委托的返回类型，则该委托的返回类型与该方法的返回类型兼容，因为这可保证方法的返回值可以安全地强制转换为委托的返回类型。  
  
 例如，具有类型为 <xref:System.Collections.IEnumerable> 的参数和 <xref:System.Object> 返回类型的委托可以表示具有类型为 <xref:System.Object> 的参数和类型为 <xref:System.Collections.IEnumerable> 的返回值的方法。 有关更多信息及代码示例，请参见 <xref:System.Delegate.CreateDelegate%28System.Type%2CSystem.Object%2CSystem.Reflection.MethodInfo%29?displayProperty=nameWithType>。  
  
 委托一般绑定到它表示的方法。 除了绑定到方法之外，委托还可以绑定到对象。 该对象表示方法的第一个参数，每次调用委托时将该对象传递给方法。 如果方法是实例方法，则将绑定的对象作为隐式 `this` 参数（在 Visual Basic 中为 `Me`）传递；如果方法为静态方法，则将该对象作为方法的第一个形参传递，并且委托签名必须匹配其余参数。 有关更多信息及代码示例，请参见 <xref:System.Delegate?displayProperty=nameWithType>。  
  
 所有委托从 <xref:System.MulticastDelegate?displayProperty=nameWithType>（继承自 <xref:System.Delegate?displayProperty=nameWithType>）继承。 C#、Visual Basic 和 C++ 语言不允许从这些类型继承， 而是提供了用于声明委托的关键字。  
  
 由于委托从 <xref:System.MulticastDelegate> 继承，因此委托具有一个调用列表，其中列出了委托表示的方法，在调用委托时将执行该列表中的方法。 列表中的所有方法接收调用委托时提供的自变量。  
  
> [!NOTE]
> 没有为在调用列表中包含多个方法的委托（即使委托具有返回类型）定义返回值。  
  
 在许多情况下（例如使用回调方法），一个委托只表示一个方法，而您需要做的就是创建委托并调用它。  
  
 对于表示多个方法的委托，.NET 提供了 <xref:System.Delegate> 和 <xref:System.MulticastDelegate> 委托类的方法，以支持如下操作：将方法添加到委托的调用列表（<xref:System.Delegate.Combine%2A?displayProperty=nameWithType> 方法）、移除方法（<xref:System.Delegate.Remove%2A?displayProperty=nameWithType> 方法）、获取调用列表（<xref:System.Delegate.GetInvocationList%2A?displayProperty=nameWithType> 方法）。  
  
> [!NOTE]
> 不需要将这些方法用于 C#、C++ 和 Visual Basic 中的事件处理程序委托，因为这些语言为添加和移除事件处理程序提供了语法。  

## <a name="type-definitions"></a>类型定义

 类型定义包括以下内容：  
  
- 对该类型定义的任何特性。  
  
- 类型的可访问性（可见性）。  
  
- 类型的名称。  
  
- 类型的基本类型。  
  
- 该类型实现的任何接口。  
  
- 每个类型的成员的定义。  
  
### <a name="attributes"></a>特性  

 特性提供附加的用户定义元数据。 它们通常用于在程序集中存储有关类型的附加信息，或在设计时或运行时环境中用于修改类型成员的行为。  
  
 特性本身是从 <xref:System.Attribute?displayProperty=nameWithType> 继承的类。 每种支持使用特性的语言都有自己的语法，用于将特性应用到某个语言元素。 特性可应用到几乎任意语言元素；特性可以应用到的特定元素由应用到该特性类的 <xref:System.AttributeUsageAttribute> 定义。  
  
### <a name="type-accessibility"></a>类型可访问性  

 所有类型都有一个修饰符，控制从其他类型对它们的可访问性。 下表说明了运行时所支持的类型可访问性。  
  
|可访问性|描述|  
|-------------------|-----------------|  
|public|所有程序集都可以访问此类型。|  
|程序集 (assembly)|只能在其程序集内访问此类型。|  
  
 嵌套类型的可访问性依赖于它的可访问域，该域是由已声明的成员可访问性和直接包含类型的可访问域这二者共同确定的。 但是，嵌套类型的可访问域不能超出包含类型的可访问域。  
  
 在程序 `M` 的类型 `T` 中，声明的嵌套成员 `P` 的可访问域是按以下方法定义的（注意，`M` 本身也可能是一个类型）：  
  
- 如果 `M` 的已声明可访问性为 `public`，则 `M` 的可访问域就是 `T` 的可访问域。  
  
- 如果 `M` 的已声明可访问性为 `protected internal`，则 `M` 的可访问域就是 `T` 的可访问域与 `P` 的程序文本和从 `T` 派生的（在 `P` 之外声明的）任何类型的程序文本之间的交集。  
  
- 如果 `M` 的已声明可访问性为 `protected`，则 `M` 的可访问域就是 `T` 的可访问域与 `T` 的程序文本和从 `T` 派生的任何类型的程序文本之间的交集。  
  
- 如果 `M` 的已声明可访问性为 `internal`，则 `M` 的可访问域就是 `T` 的可访问域与 `P` 的程序文本之间的交集。  
  
- 如果 `M` 的已声明可访问性为 `private`，则 `M` 的可访问域就是 `T` 的程序文本。  
  
### <a name="type-names"></a>类型名称  

 常规类型系统对名称只有两种限制：  
  
- 所有的名称都按 Unicode（16 位）字符串进行编码。  
  
- 名称不允许有嵌入的（16 位）0x0000 值。  
  
 但是，大多数语言对类型名称都有附加限制。 所有比较都是逐字节进行的，因此会区分大小写并与区域设置无关。  
  
 尽管类型可能引用来自其他模块和程序集的类型，但类型在一个 .NET 模块内必须是完全定义的。 （但是，根据编译器支持的情况，它可以分成多个源代码文件。）类型名称只需要在命名空间内唯一。 要完全标识一个类型，其类型名称必须由包含此类型实现的命名空间加以限定。  
  
### <a name="base-types-and-interfaces"></a>基本类型和接口  

 一个类型可以从另一个类型继承值和行为。 常规类型系统不允许类型从多个基本类型进行继承。  
  
 一个类型可以实现任何数量的接口。 要实现接口，类型必须实现该接口的所有虚拟成员。 虚方法可以由派生的类型来实现，既可静态调用，也可动态调用。  

## <a name="type-members"></a>类型成员

 运行时允许您定义指定类型行为和状态的类型成员。 类型成员包括以下内容：  
  
- [字段](#fields)  
  
- [属性](#properties)  
  
- [方法](#methods)  
  
- [构造函数](#constructors)  
  
- [事件](#events)  
  
- [嵌套类型](#nested-types)  

### <a name="fields"></a>字段

 字段描述并包含类型状态的一部分。 字段可以是运行时支持的任何类型。 字段通常是 `private` 或 `protected`，因此只能在类内部或从派生类访问。 如果可以从类型外部修改字段值，通常使用属性集访问器。 公开的字段通常是只读的，可以为以下两种类型：  
  
- 常量，在设计时赋值。 尽管没有使用 `static`（在 Visual Basic 中为 `Shared`）关键字定义，但它们都是类的静态成员。  
  
- 只读变量，可以在类构造函数中赋值。  
  
 下面的示例演示只读字段的这两种用法。  
  
 [!code-csharp[Conceptual.Types.Members.Fields#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.members.fields/cs/example.cs#1)]
 [!code-vb[Conceptual.Types.Members.Fields#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.members.fields/vb/example.vb#1)]  

### <a name="properties"></a>属性

 属性命名类型的值或状态，并定义获得或设置属性值的方法。 属性可以是基元类型、基元类型的集合、用户定义的类型或用户定义类型的集合。 属性通常用于使类型的公共接口独立于类型的实际表示形式。 这使属性能够反映不直接在类中存储的值（例如，当属性返回计算值时）或能够在将值赋给私有字段前进行验证。 下面的示例演示后一种模式。  
  
 [!code-csharp[Conceptual.Types.Members.Properties#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.members.properties/cs/example.cs#1)]
 [!code-vb[Conceptual.Types.Members.Properties#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.members.properties/vb/example.vb#1)]  
  
 除了包含属性本身之外，包含可读属性的类型的 Microsoft 中间语言 (MSIL) 还包含 `get_`propertyname 方法，包含可写属性的类型的 MSIL 还包含 `set_`propertyname 方法。  

### <a name="methods"></a>方法

 方法描述可用于类型的操作。 方法的签名指定其所有参数和返回值可使用的类型。  
  
 尽管大多数方法都定义方法调用所需的参数的确切数目，但某些方法支持可变数目的参数。 这些方法的最后声明的参数用 <xref:System.ParamArrayAttribute> 特性标记。 语言编译器通常会提供一个关键字（例如，在 C# 中为 `params`，在 Visual Basic 中为 `ParamArray`），使得不必显式使用 <xref:System.ParamArrayAttribute>。  

### <a name="constructors"></a>构造函数

 构造函数是一种特殊类型的方法，可创建类或结构的新实例。 像任何其他方法一样，构造函数可以包含参数，但是它不返回值（即它返回 `void`）。  
  
 如果类的源代码没有显式定义构造函数，则编译器包含一个无参数构造函数。 但是，如果某个类的源代码只定义参数化的构造函数，则 Visual Basic 和 C# 编译器将不会生成无参数构造函数。  
  
 如果一个结构的源代码定义多个构造函数，则这些构造函数必须是参数化的；结构不能定义无参数构造函数，并且编译器不会为结构或其他值类型生成无参数构造函数。 所有值类型都具有隐式无参数构造函数。 此构造函数由公共语言运行时实现，并且将该结构的所有字段都初始化为其默认值。  

### <a name="events"></a>事件

 事件定义可以响应的事情，并定义订阅、取消订阅及引发事件的方法。 事件通常用于通知其他类型的状态改变。 有关详细信息，请参阅[事件](../events/index.md)。  

### <a name="nested-types"></a>嵌套类型

 嵌套类型是作为某其他类型的成员的类型。 嵌套类型应与其包含类型紧密关联，并且不得用作通用类型。 在声明类型使用和创建嵌套类型实例时，嵌套类型很有用，但不在公共成员中公开嵌套类型的使用。  
  
 嵌套类型可能会使有些开发人员感到困惑，因此除非有必要的理由，否则嵌套类型不应是公开可见的。 在设计完善的库中，开发人员几乎不需要使用嵌套类型实例化对象或声明变量。  

## <a name="characteristics-of-type-members"></a>类型成员的特征

 通用类型系统允许类型成员具有多种特征，但并不要求语言能支持所有这些特征。 下表介绍了这些成员特征。  
  
|特征|可应用到|描述|  
|--------------------|------------------|-----------------|  
|abstract|方法、属性和事件|类型不提供方法的实现。 继承或实现抽象方法的类型必须提供方法的实现。 只有当派生的类型本身是抽象类型的时候，情况例外。 所有的抽象方法都是虚的。|  
|private、family、assembly、family 和 assembly、family 或 assembly，或者 public|全部|定义成员的可访问性：<br /><br /> private<br /> 只能在与成员相同的类型或在嵌套类型中访问。<br /><br /> family<br /> 在与成员相同的类型中和从它继承的派生类型访问。<br /><br /> 程序集<br /> 只能在定义该类型的程序集中访问。<br /><br /> family 和 assembly<br /> 只能从同时具备族和程序集访问权的类型进行访问。<br /><br /> family 或 assembly<br /> 只能从具备族和程序集访问权的类型进行访问。<br /><br /> public<br /> 可从任何类型访问。|  
|final|方法、属性和事件|虚方法不能在派生类型中被重写。|  
|initialize-only|字段|该值只能被初始化，不能在初始化之后写入。|  
|实例|字段、方法、属性和事件|如果成员未标记为 `static`（C# 和 C++）、`Shared` (Visual Basic)、`virtual`（C# 和 C++）或 `Overridable` (Visual Basic)，则它是一个实例成员（没有实例关键字）。 内存中这些成员的副本数将会像使用它们的对象数一样多。|  
|文本|字段|分配给该字段的值是一个内置值类型的固定值（在编译时已知）。 文本字段有时指的是常数。|  
|newslot 或 override|全部|定义成员如何与具有相同签名的继承成员进行交互：<br /><br /> newslot<br /> 隐藏具有相同签名的继承成员。<br /><br /> override<br /> 替换继承的虚方法的定义。<br /><br /> 默认为 newslot。|  
|静态|字段、方法、属性和事件|成员属于定义它的类型，而不属于该类型的特定实例；即使不创建类型的实例，成员也会存在，并且它由该类型的所有实例共享。|  
|virtual|方法、属性和事件|此方法可以由派生类型实现，并且既可静态调用，也可动态调用。 如果使用动态调用，在运行时执行调用的实例类型（而不是编译时已知的类型）将确定调用方法的哪一种实现。 若要静态调用虚方法，可能必须将变量强制转换为使用所需方法版本的类型。|  
  
### <a name="overloading"></a>重载  

 每个类型成员都有一个唯一的签名。 方法签名由方法名称和一个参数列表（方法的参数的顺序和类型）组成。 可以在一种类型内定义具有相同名称的多种方法，只要这些方法的签名不同。 当定义两种或多种具有相同名称的方法时，就称作重载。 例如，在 <xref:System.Char?displayProperty=nameWithType> 中，重载了 <xref:System.Char.IsDigit%2A> 方法。 一个方法采用 <xref:System.Char>。 另一个方法采用 <xref:System.String> 和 <xref:System.Int32>。  
  
> [!NOTE]
> 返回类型不被视为方法签名的一部分。 这意味着如果方法只是返回类型不同，就不能重载。  
  
### <a name="inherit-override-and-hide-members"></a>继承、重写和隐藏成员  

 派生类型继承其基类型的所有成员；也就是说，会在派生类型上定义这些成员，并供派生类型使用。 继承成员的行为和质量可以通过以下两种方式来修改：  
  
- 派生类型可通过使用相同的签名定义一个新成员，从而隐藏继承的成员。 将先前的公共成员变成私有成员，或者为标记为 `final` 的继承方法定义新行为时，可以采取这种方法。  
  
- 派生类型可以重写继承的虚方法。 重写方法提供了对方法的一种新定义，将根据运行时的值的类型，而不是编译时已知的变量类型来调用方法。 只有在虚拟方法未标记为 `final` 且新方法至少可以像虚拟方法一样进行访问的情况下，方法才能重写虚拟方法。  
  
## <a name="see-also"></a>请参阅

- [.NET API 浏览器](../../../api/index.md)
- [公共语言运行时](../clr.md)
- [.NET 中的类型转换](type-conversion.md)
