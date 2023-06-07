---
title: .NET 中的类型转换
description: 了解 .NET 中的类型转换，该转换在新类型中创建的值与旧类型的值相等，但不能保留原始类型的标识。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- widening conversions
- explicit conversions
- narrowing conversions
- type conversion, about type conversion
- type conversion
- converting types
- narrowing coercion
- Explicit operator
- IConvertible interface
- base types, converting
- op_Implicit method
- widening coercion
- op_Explicit method
- Convert class
- implicit conversions
- Implicit operator
- data types [.NET], converting
ms.assetid: ba36154f-064c-47d3-9f05-72f93a7ca96d
ms.openlocfilehash: 8e1ea153ccd65de967a34e79d4a26c96e678c4ad
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94831021"
---
# <a name="type-conversion-in-net"></a>.NET 中的类型转换

每个值都有与之关联的类型，此类型定义分配给该值的空间大小、它可以具有的可能值的范围以及它可以提供的成员等特性。 许多值可以表示为多种类型。 例如，值 4 可以表示为整数或浮点值。 类型转换可以创建一个等同于旧类型值的新类型值，但却不必保留原始对象的恒等值（或精确值）。  
  
.NET 自动支持以下转换：  
  
- 从派生类转换为基类。 例如，这意味着可将任何类或结构的实例转换为 <xref:System.Object> 实例。  此转换不需要强制转换或转换运算符。  
  
- 从基类转换回原始的派生类。 在 C# 中，此转换需要强制转换运算符。 在 Visual Basic 中，如果 `Option Strict` 处于开启状态，则它需要 `CType` 运算符。  
  
- 从实现接口的类型转换为表示该接口的接口对象。 此转换不需要强制转换或转换运算符。  
  
- 从接口对象转换回实现该接口的原始类型。  在 C# 中，此转换需要强制转换运算符。 在 Visual Basic 中，如果 `Option Strict` 处于开启状态，则它需要 `CType` 运算符。  
  
除这些自动转换外，.NET 还提供支持自定义类型转换的多种功能。 其中包括：  
  
- `Implicit` 运算符，该运算符定义类型之间可用的扩大转换。 有关详细信息，请参阅[使用隐式运算符的隐式转换](#implicit-conversion-with-the-implicit-operator)部分。  
  
- `Explicit` 运算符，该运算符定义类型之间可用的收缩转换。 有关详细信息，请参阅[使用显式运算符的显式转换](#explicit-conversion-with-the-explicit-operator)部分。  
  
- <xref:System.IConvertible> 接口，该接口定义到 .NET 每个基数据类型的转换。 有关更多信息，请参阅 [IConvertible 接口](#the-iconvertible-interface)部分。  
  
- <xref:System.Convert> 类，该类提供了一组方法来实现 <xref:System.IConvertible> 接口中的方法。 有关更多信息，请参阅 [Convert 类](#the-convert-class)部分。  
  
- <xref:System.ComponentModel.TypeConverter> 类，该类是一个基类，可以扩展该类以支持指定的类型到任何其他类型的转换。 有关更多信息，请参阅 [TypeConverter 类](#the-typeconverter-class)部分。  

## <a name="implicit-conversion-with-the-implicit-operator"></a>使用隐式运算符的隐式转换

扩大转换涉及从现有类型的值创建一个新值，该现有类型比目标类型具有限制性更强的范围或限制性更强的成员列表。 扩大转换不会导致数据丢失（但可能导致精度损失）。 由于不会丢失数据，因此编译器可以隐式或透明地处理转换，无需使用显式转换方法或强制转换运算符。  
  
> [!NOTE]
> 虽然执行隐式转换的代码可以调用转换方法或使用强制转换运算符，但支持隐式转换的编译器不需要调用转换方法或使用强制转换运算符。  
  
 例如，<xref:System.Decimal> 类型支持从 <xref:System.Byte>、<xref:System.Char>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.SByte>、<xref:System.UInt16>、<xref:System.UInt32> 和 <xref:System.UInt64> 值进行的隐式转换。 下面的示例通过为 <xref:System.Decimal> 变量赋值演示了其中的一些隐式转换。  
  
 [!code-csharp[Conceptual.Conversion#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/implicit1.cs#1)]
 [!code-vb[Conceptual.Conversion#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/implicit1.vb#1)]  
  
 如果特定语言编译器支持自定义运算符，则您还可以在自己的自定义类型中定义隐式转换。 下面的示例提供了一个名为 `ByteWithSign` 的有符号字节数据类型的分部实现，该分部实现使用符号数值表示法。 它支持 <xref:System.Byte> 和 <xref:System.SByte> 值到 `ByteWithSign` 值的隐式转换。  
  
 [!code-csharp[Conceptual.Conversion#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/implicit1.cs#2)]
 [!code-vb[Conceptual.Conversion#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/implicit1.vb#2)]  
  
 然后，客户端代码可以声明一个 `ByteWithSign` 变量，并为该变量赋予 <xref:System.Byte> 和 <xref:System.SByte> 值，而无需执行任何显式转换或使用任何强制转换运算符，如下面的示例所示。  
  
 [!code-csharp[Conceptual.Conversion#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/implicit1.cs#3)]
 [!code-vb[Conceptual.Conversion#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/implicit1.vb#3)]  

## <a name="explicit-conversion-with-the-explicit-operator"></a>使用显式运算符的显式转换

收缩转换涉及从现有类型的值创建一个新值，该现有类型比目标类型具有更大的范围和更大的成员列表。 由于收缩转换可以导致数据丢失，因此编译器通常需要通过调用转换方法或使用强制转换运算符来进行显式转换。 也就是说，必须在开发人员代码中显式处理收缩转换。  
  
> [!NOTE]
> 收缩转换之所以需要使用转换方法或强制转换运算符，主要是为提醒开发人员可能会丢失数据或引发 <xref:System.OverflowException>，以便可以在代码中对其进行处理。 但是，有些编译器可以放宽此要求。 例如，在 Visual Basic 中，如果 `Option Strict` 关闭（其默认设置），则 Visual Basic 编译器会尝试隐式执行收缩转换。  
  
 例如，<xref:System.UInt32>、<xref:System.Int64> 和 <xref:System.UInt64> 数据类型均具有超过 <xref:System.Int32> 数据类型的范围，如下表所示。  
  
|类型|与 Int32 范围的比较|  
|----------|------------------------------------|  
|<xref:System.Int64>|<xref:System.Int64.MaxValue?displayProperty=nameWithType> 大于 <xref:System.Int32.MaxValue?displayProperty=nameWithType>；<xref:System.Int64.MinValue?displayProperty=nameWithType> 小于 <xref:System.Int32.MinValue?displayProperty=nameWithType>（即比后者具有更大的负范围）。|  
|<xref:System.UInt32>|<xref:System.UInt32.MaxValue?displayProperty=nameWithType> 大于 <xref:System.Int32.MaxValue?displayProperty=nameWithType>。|  
|<xref:System.UInt64>|<xref:System.UInt64.MaxValue?displayProperty=nameWithType> 大于 <xref:System.Int32.MaxValue?displayProperty=nameWithType>。|  
  
 为了处理这种收缩转换，.NET 允许类型定义 `Explicit` 运算符。 然后，各种语言编译器可以使用自己的语法实现此运算符，也可以调用 <xref:System.Convert> 类的成员来执行此转换。 （有关 <xref:System.Convert> 类的详细信息，请参阅本主题后面的 [Convert 类](#the-convert-class)。）下面的示例演示如何使用语言功能来处理这些可能超出范围的整数值到 <xref:System.Int32> 值的显式转换。  
  
 [!code-csharp[Conceptual.Conversion#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#4)]
 [!code-vb[Conceptual.Conversion#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/explicit1.vb#4)]  
  
 显式转换在不同的语言中可能会产生不同的结果，并且这些结果可能因对应的 <xref:System.Convert> 方法所返回的值而异。 例如，如果将 <xref:System.Double> 值 12.63251 转换为 <xref:System.Int32>，则 Visual Basic `CInt` 方法和 .NET <xref:System.Convert.ToInt32%28System.Double%29?displayProperty=nameWithType> 方法会对 <xref:System.Double> 进行舍入以返回值 13，而 C# `(int)` 运算符会截断 <xref:System.Double> 以返回值 12。 类似地，C# `(int)` 运算符不支持从布尔值到整数的转换，而 Visual Basic `CInt` 方法会将值 `true` 转换为 -1。 另一方面，<xref:System.Convert.ToInt32%28System.Boolean%29?displayProperty=nameWithType> 方法将值 `true` 转换为 1。  
  
 大多数编译器允许以有检查或无检查的方式执行显式转换。 当执行有检查转换时，如果被转换类型的值超出了目标类型的范围，则会引发 <xref:System.OverflowException>。 在相同条件下执行无检查转换时，转换可能不会引发异常，但无法确定确切的行为，并且可能产生不正确的值。  
  
> [!NOTE]
> 在 C# 中，可将 `checked` 关键字与强制转换运算符一起使用来执行有检查转换，也可通过指定 `/checked+` 编译器选项来执行有检查转换。 反过来，可将 `unchecked` 关键字与强制转换运算符一起使用来执行无检查转换，或者通过指定 `/checked-` 编译器选项来执行无检查转换。 默认情况下，显式转换将为无检查转换。 在 Visual Basic 中，通过清除项目的“高级编译器设置”对话框中的“不做整数溢出检查”复选框或指定 `/removeintchecks-` 编译器选项，可以执行有检查转换。 反之，通过选中项目的“高级编译器设置”对话框中的“不做整数溢出检查”复选框，或者指定 `/removeintchecks+` 编译器选项，可以执行无检查转换。 默认情况下，显式转换将为有检查转换。  
  
 下面的 C# 示例使用 `checked` 和 `unchecked` 关键字阐释了将 <xref:System.Byte> 范围外的值转换为 <xref:System.Byte> 时的行为差异。 有检查转换会引发异常，但无检查转换会向 <xref:System.Byte.MaxValue?displayProperty=nameWithType> 变量赋予 <xref:System.Byte>。  
  
 [!code-csharp[Conceptual.Conversion#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#12)]  
  
 如果特定语言编译器支持自定义重载运算符，您还可以在自己的自定义类型中定义显式转换。 下面的示例提供了一个名为 `ByteWithSign` 的有符号字节数据类型的分部实现，该分部实现使用符号数值表示法。 它支持 <xref:System.Int32> 和 <xref:System.UInt32> 值到 `ByteWithSign` 值的显式转换。  
  
 [!code-csharp[Conceptual.Conversion#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#5)]
 [!code-vb[Conceptual.Conversion#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/explicit1.vb#5)]  
  
 然后，客户端代码可以声明一个 `ByteWithSign` 变量，并为该变量赋予 <xref:System.Int32> 和 <xref:System.UInt32> 值（如果赋值中包括一个强制转换运算符或转换方法），如下面的示例所示。  
  
 [!code-csharp[Conceptual.Conversion#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#6)]
 [!code-vb[Conceptual.Conversion#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/explicit1.vb#6)]  

## <a name="the-iconvertible-interface"></a>IConvertible 接口

为了支持任意类型到公共语言运行时基类型的转换，.NET 提供了 <xref:System.IConvertible> 接口。 需要使用实现类型以提供以下方法：  
  
- 一个返回实现类型的 <xref:System.TypeCode> 的方法。  
  
- 用于将实现类型转换为公共语言运行时的每一种基类型（<xref:System.Boolean>、<xref:System.Byte>、<xref:System.DateTime>、<xref:System.Decimal> 和 <xref:System.Double> 等）的各种方法。  
  
- 一个用于将实现类型的实例转换为另一个指定类型的通用转换方法。 不支持的转换应引发 <xref:System.InvalidCastException>。  
  
 公共语言运行时的每一种基类型（即 <xref:System.Boolean>、<xref:System.Byte>、<xref:System.Char>、<xref:System.DateTime>、<xref:System.Decimal>、<xref:System.Double>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.SByte>、<xref:System.Single>、<xref:System.String>、<xref:System.UInt16>、<xref:System.UInt32> 和 <xref:System.UInt64>）以及 <xref:System.DBNull> 和 <xref:System.Enum> 类型都可以实现 <xref:System.IConvertible> 接口。 但是，这些是显式接口实现；因此只能通过 <xref:System.IConvertible> 接口变量来调用转换方法，如下面的示例所示。 此示例将一个 <xref:System.Int32> 值转换为其等效的 <xref:System.Char> 值。  
  
 [!code-csharp[Conceptual.Conversion#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/iconvertible1.cs#7)]
 [!code-vb[Conceptual.Conversion#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/iconvertible1.vb#7)]  
  
 对转换方法的接口（而不是实现类型）调用转换方法的要求使显式接口实现成为一种代价相对较大的操作。 因此，在公共语言运行时基类型之间进行转换时，建议您调用 <xref:System.Convert> 类的适当成员。 有关详细信息，请参阅下一部分 [Convert 类](#the-convert-class)。  
  
> [!NOTE]
> 除了 .NET 提供的 <xref:System.IConvertible> 接口和 <xref:System.Convert> 类，各种语言还可能会提供其他方法来执行转换。 例如，C# 使用强制转换运算符；Visual Basic 使用编译器实现的转换函数，例如 `CType`、`CInt` 和 `DirectCast`。  
  
 大多数情况下，<xref:System.IConvertible> 接口设计为支持 .NET 中基类型之间的转换。 但是，通过自定义类型也可以实现该接口，以便支持该类型到其他自定义类型的转换。 有关详细信息，请参阅本主题后面的[使用 ChangeType 方法的自定义转换](#custom-conversions-with-the-changetype-method)部分。

## <a name="the-convert-class"></a>Convert 类

虽然可以调用每个基类型的 <xref:System.IConvertible> 接口实现来执行类型转换，但从一种基类型转换为另一种基类型时，建议您调用 <xref:System.Convert?displayProperty=nameWithType> 类的方法，这种方式与语言无关。 此外，<xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 方法还可用于从一个指定的自定义类型转换为另一种类型。  
  
### <a name="conversions-between-base-types"></a>基类型之间的转换

<xref:System.Convert> 类提供了一种与语言无关的方式来执行基类型之间的转换，并且该类可用于面向公共语言运行时的所有语言。 它为扩大转换和收缩转换提供了一组完整的方法，并且会对不支持的转换（例如 <xref:System.InvalidCastException> 值到整数值的转换）引发 <xref:System.DateTime>。 收缩转换是在已检查的上下文中执行的，如果转换失败，将引发 <xref:System.OverflowException>。  
  
> [!IMPORTANT]
> 由于 <xref:System.Convert> 类包含用于转换为每个基类型和从每个基类型进行转换的方法，因此不再需要调用每个基类型的 <xref:System.IConvertible> 显式接口实现。  
  
 下面的示例演示如何使用 <xref:System.Convert?displayProperty=nameWithType> 类来执行 .NET 基类型之间的多种扩大转换和收缩转换。  
  
 [!code-csharp[Conceptual.Conversion#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/convert1.cs#8)]
 [!code-vb[Conceptual.Conversion#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/convert1.vb#8)]  
  
 在某些情况下，尤其是当转换为浮点值和从浮点值进行转换时，转换可能会丢失精度，即使不引发 <xref:System.OverflowException> 时也是如此。 下面的示例演示了这种精度丢失。 在第一种情况下，<xref:System.Decimal> 值在转换为 <xref:System.Double> 后精度降低（有效位减少）。 在第二种情况下，<xref:System.Double> 值从 42.72 四舍五入为 43 以完成转换。  
  
 [!code-csharp[Conceptual.Conversion#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/convert1.cs#9)]
 [!code-vb[Conceptual.Conversion#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/convert1.vb#9)]  
  
 有关列出 <xref:System.Convert> 类支持的扩大转换和收缩转换的表，请参阅[类型转换表](conversion-tables.md)。  

### <a name="custom-conversions-with-the-changetype-method"></a>使用 ChangeType 方法的自定义转换

除了支持到每个基类型的转换外，<xref:System.Convert> 类还可用于将一个自定义类型转换为一个或多个预定义类型。 此转换是通过 <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 方法执行的，而此方法包装了对 <xref:System.IConvertible.ToType%2A?displayProperty=nameWithType> 参数的 `value` 方法的调用。 这意味着 `value` 参数所表示的对象必须提供 <xref:System.IConvertible> 接口的实现。  
  
> [!NOTE]
> 由于 <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%29?displayProperty=nameWithType> 和 <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 方法使用 <xref:System.Type> 对象来指定 `value` 将转换为的目标类型，因此它们可用于执行对象（其类型在编译时是未知的）的动态转换。 但请注意，<xref:System.IConvertible> 的 `value` 实现必须仍支持此转换。  
  
 下面的示例演示 <xref:System.IConvertible> 接口的一个可能实现，该实现允许将 `TemperatureCelsius` 对象转换为 `TemperatureFahrenheit` 对象，反之亦然。 此示例定义一个基类 `Temperature`，该基类实现 <xref:System.IConvertible> 接口并重写 <xref:System.Object.ToString%2A?displayProperty=nameWithType> 方法。 派生的 `TemperatureCelsius` 和 `TemperatureFahrenheit` 类分别重写该基类的 `ToType` 和 `ToString` 方法。  
  
 [!code-csharp[Conceptual.Conversion#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/iconvertible2.cs#10)]
 [!code-vb[Conceptual.Conversion#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/iconvertible2.vb#10)]  
  
 下面的示例演示对这些 <xref:System.IConvertible> 实现的多个调用，以实现 `TemperatureCelsius` 对象和 `TemperatureFahrenheit` 对象之间的相互转换。  
  
 [!code-csharp[Conceptual.Conversion#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/iconvertible2.cs#11)]
 [!code-vb[Conceptual.Conversion#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/iconvertible2.vb#11)]  

## <a name="the-typeconverter-class"></a>TypeConverter 类

.NET 还允许你通过下面的方法为自定义类型定义类型转换器：扩展 <xref:System.ComponentModel.TypeConverter?displayProperty=nameWithType> 类，然后通过 <xref:System.ComponentModel.TypeConverterAttribute?displayProperty=nameWithType> 特性将类型转换器与该类型关联。 下表列出了此方法与为自定义类型实现 <xref:System.IConvertible> 接口之间的差异。  
  
> [!NOTE]
> 只能为已定义了类型转换器的自定义类型提供设计时支持。  
  
|使用 TypeConverter 转换|使用 IConvertible 转换|  
|------------------------------------|-----------------------------------|  
|通过从 <xref:System.ComponentModel.TypeConverter> 派生单独的类来为自定义类型实现。 此派生类通过应用 <xref:System.ComponentModel.TypeConverterAttribute> 特性与自定义类型关联。|由自定义类型实现，以执行转换。 该类型的用户必须对该类型调用 <xref:System.IConvertible> 转换方法。|  
|在设计时和运行时都可以使用。|只能在运行时使用。|  
|使用反射；因此，比 <xref:System.IConvertible> 所启用的转换慢。|不使用反射。|  
|允许自定义类型和其他数据类型间的双向类型转换。 例如，为 <xref:System.ComponentModel.TypeConverter> 定义的 `MyType` 允许从 `MyType` 转换为 <xref:System.String> 以及从 <xref:System.String> 转换为 `MyType`。|允许从自定义类型转换为其他数据类型，但不允许从其他数据类型转换为自定义类型。|  
  
 有关使用类型转换器执行转换的更多信息，请参见 <xref:System.ComponentModel.TypeConverter?displayProperty=nameWithType>。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Convert?displayProperty=nameWithType>
- <xref:System.IConvertible>
- [类型转换表](conversion-tables.md)
