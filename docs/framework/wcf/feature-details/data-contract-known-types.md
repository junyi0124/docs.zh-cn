---
title: 数据协定已知类型
description: 了解在 WCF 中的反序列化过程中，数据协定模型如何使用 KnownTypeAttribute 类来指定要包含的类型。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], known types
- KnownTypeAttribute [WCF]
- KnownTypes [WCF]
ms.assetid: 1a0baea1-27b7-470d-9136-5bbad86c4337
ms.openlocfilehash: 124083d86c220451c55a9290c2edf996b50d8d28
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286677"
---
# <a name="data-contract-known-types"></a>数据协定已知类型

<xref:System.Runtime.Serialization.KnownTypeAttribute> 类允许您预先指定应该在反序列化期间包括在考虑范围内的类型。 有关工作示例，请参阅 [Known Types](../samples/known-types.md) 示例。  
  
 通常，在客户端和服务之间传递参数和返回值时，这两个终结点共享要传输的数据的所有数据协定。 但是，在以下情况下并非如此：  
  
- 已发送的数据协定派生自预期的数据协定。 有关详细信息，请参阅 [数据协定等效](data-contract-equivalence.md) 性) 中有关继承的部分。 在该情况下，传输的数据没有与接收终结点所预期相同的数据协定。  
  
- 要传输的信息的声明类型是接口，而非类、结构或枚举。 因此，无法预先知道实际发送了实现接口的哪个类型，接收终结点就无法预先确定已传输数据的数据协定。  
  
- 要传输的信息的声明类型是 <xref:System.Object>。 由于每个类型都继承自 <xref:System.Object>，而且无法预先知道实际发送了哪个类型，因此接收终结点无法预先确定已传输数据的数据协定。 这是第一个项的特殊情况：每个数据协定都源自为 <xref:System.Object>生成的默认空数据协定。  
  
- 某些类型（包括 .NET Framework 类型）具有上述三种类别之一中的成员。 例如， <xref:System.Collections.Hashtable> 使用 <xref:System.Object> 在哈希表中存储实际对象。 在序列化这些类型时，接收方无法预先确定这些成员的数据协定。  
  
## <a name="the-knowntypeattribute-class"></a>KnownTypeAttribute 类  

 当数据到达接收终结点时，WCF 运行时尝试将数据反序列化为公共语言运行时 (CLR) 类型的实例。 通过首先检查传入消息选择为反序列化而实例化的类型，以确定消息内容遵循的数据协定。 然后反序列化引擎尝试查找实现与消息内容兼容的数据协定的 CLR 类型。 反序列化引擎在此过程中允许的侯选类型集称为反序列化程序的“已知类型”集。  
  
 让反序列化引擎了解某个类型的一种方法是使用 <xref:System.Runtime.Serialization.KnownTypeAttribute>。 不能将属性应用于单个数据成员，只能将它应用于整个数据协定类型。 将属性应用于可能为类或结构的“外部类型”  。 在其最基本的用法中，应用属性会将类型指定为“已知类型”。 只要反序列化外部类型的对象或通过其成员引用的任何对象，这就会导致已知类型成为已知类型集的一部分。 可以将多个 <xref:System.Runtime.Serialization.KnownTypeAttribute> 属性应用于同一类型。  
  
## <a name="known-types-and-primitives"></a>已知类型和基元  

 基元类型以及被视为基元的某些类型（例如， <xref:System.DateTime> 和 <xref:System.Xml.XmlElement>）始终是“已知”的，且从来不必通过此机制进行添加。 但是，必须显式添加基元类型的数组。 大多数集合被视为等效于数组。 （非泛型集合被视为等效于 <xref:System.Object>的数组）。 有关使用基元、基元数组和基元集合的示例，请参见示例 4。  
  
> [!NOTE]
> 与其他基元类型不同， <xref:System.DateTimeOffset> 结构默认情况下不是已知类型，因此必须将它手动添加到已知类型列表。  
  
## <a name="examples"></a>示例  

 下面的示例说明如何使用 <xref:System.Runtime.Serialization.KnownTypeAttribute> 类。  
  
#### <a name="example-1"></a>示例 1  

 有三个具有继承关系的类。  
  
 [!code-csharp[C_KnownTypeAttribute#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#1)]
 [!code-vb[C_KnownTypeAttribute#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#1)]  
  
 如果 `CompanyLogo` 成员设置为 `ShapeOfLogo` 或 `CircleType` 对象，则可以序列化下面的 `TriangleType` 类，而不能对其进行反序列化，因为反序列化引擎无法识别具有数据协定名称“Circle”或“Triangle”的任何类型。  
  
 [!code-csharp[C_KnownTypeAttribute#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#2)]
 [!code-vb[C_KnownTypeAttribute#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#2)]  
  
 在下面的代码中演示了编写 `CompanyLogo` 类型的正确方法。  
  
 [!code-csharp[C_KnownTypeAttribute#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#3)]
 [!code-vb[C_KnownTypeAttribute#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#3)]  
  
 只要反序列化外部类型 `CompanyLogo2` ，反序列化引擎就会了解有关 `CircleType` 和 `TriangleType` ，因此能够查找“Circle”和“Triangle”数据协定的匹配类型。  
  
#### <a name="example-2"></a>示例 2  

 在下面的示例中，尽管 `CustomerTypeA` 和 `CustomerTypeB` 都具有 `Customer` 数据协定，但是只要反序列化 `CustomerTypeB` 就会创建 `PurchaseOrder` 的实例，因为只有 `CustomerTypeB` 对反序列化引擎是已知的。  
  
 [!code-csharp[C_KnownTypeAttribute#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#4)]
 [!code-vb[C_KnownTypeAttribute#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#4)]  
  
#### <a name="example-3"></a>示例 3  

 在下面的示例中， <xref:System.Collections.Hashtable> 将其内容在内部存储为 <xref:System.Object>。 若要成功反序列化哈希表，反序列化引擎必须知道那里可能出现的一组可能类型。 在这种情况下，我们预先知道只有 `Book` 和 `Magazine` 对象存储在 `Catalog`中，因此使用 <xref:System.Runtime.Serialization.KnownTypeAttribute> 属性添加它们。  
  
 [!code-csharp[C_KnownTypeAttribute#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#5)]
 [!code-vb[C_KnownTypeAttribute#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#5)]  
  
#### <a name="example-4"></a>示例 4  

 在下面的示例中，数据协定存储一个数字和要对该数字执行的操作。 `Numbers` 数据成员可以是整数、整数数组或包含整数的 <xref:System.Collections.Generic.List%601> 。  
  
> [!CAUTION]
> 仅当使用 SVCUTIL.EXE 来生成 WCF 代理时，此方法才能在客户端使用。 SVCUTIL.EXE 从包含任何已知类型的服务中检索元数据。 如果没有此信息，客户端将不能反序列化该类型。  
  
 [!code-csharp[C_KnownTypeAttribute#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#6)]
 [!code-vb[C_KnownTypeAttribute#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#6)]  
  
 这是应用程序代码。  
  
 [!code-csharp[C_KnownTypeAttribute#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#7)]
 [!code-vb[C_KnownTypeAttribute#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#7)]  
  
## <a name="known-types-inheritance-and-interfaces"></a>已知类型、继承和接口  

 使用 `KnownTypeAttribute` 属性将已知类型与特定类型关联时，已知类型也与该类型的所有派生类型关联。 例如，请参见下面的代码。  
  
 [!code-csharp[C_KnownTypeAttribute#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#8)]
 [!code-vb[C_KnownTypeAttribute#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#8)]  
  
 `DoubleDrawing` 类无需 `KnownTypeAttribute` 属性即可在 `Square` 字段中使用 `Circle` 和 `AdditionalShape` ，因为基类 (`Drawing`) 已经应用这些属性。  
  
 已知类型只能与类和结构关联，而不能与接口关联。  
  
## <a name="known-types-using-open-generic-methods"></a>使用开放式泛型方法的已知类型  

 可能需要将泛型类型作为已知类型添加。 但是，不能将开放式泛型类型作为参数传递到 `KnownTypeAttribute` 属性。  
  
 通过使用替代机制可以解决此问题：编写一个返回要添加到已知类型集合的类型列表的方法。 然后将方法名称指定为 `KnownTypeAttribute` 属性的字符串参数（由于某些限制所致）。  
  
 方法必须存在于应用 `KnownTypeAttribute` 属性的类型上，不得接受参数，且必须返回可以分配给 <xref:System.Collections.IEnumerable> 的 <xref:System.Type>的对象。  
  
 不能将具有方法名称的 `KnownTypeAttribute` 属性与实际类型在同一类型上的 `KnownTypeAttribute` 属性组合在一起。 此外，不能将具有方法名称的多个 `KnownTypeAttribute` 应用于同一类型。  
  
 请参见下面的类。  
  
 [!code-csharp[C_KnownTypeAttribute#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#9)]
 [!code-vb[C_KnownTypeAttribute#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#9)]  
  
 `theDrawing` 字段包含泛型类 `ColorDrawing` 和泛型类 `BlackAndWhiteDrawing`的实例，这两个泛型类都是从泛型类 `Drawing`继承的。 通常，必须将它们添加到已知类型，但下面不是有效的属性语法。  
  
```csharp  
// Invalid syntax for attributes:  
// [KnownType(typeof(ColorDrawing<T>))]  
// [KnownType(typeof(BlackAndWhiteDrawing<T>))]  
```  
  
```vb  
' Invalid syntax for attributes:  
' <KnownType(GetType(ColorDrawing(Of T))), _  
' KnownType(GetType(BlackAndWhiteDrawing(Of T)))>  
```  
  
 因此，必须创建一个方法以返回这些类型。 在下面的代码中演示了编写此类型的正确方法。  
  
 [!code-csharp[C_KnownTypeAttribute#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_knowntypeattribute/cs/source.cs#10)]
 [!code-vb[C_KnownTypeAttribute#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_knowntypeattribute/vb/source.vb#10)]  
  
## <a name="additional-ways-to-add-known-types"></a>添加已知类型的其他方法  

 此外，可以通过配置文件添加已知类型。 如果不控制需要已知类型才能正确反序列化的类型（例如，将第三方类型库与 Windows Communication Foundation (WCF) 结合使用时，这将非常有用。  
  
 下面的配置文件演示如何在配置文件中指定已知类型。  
  
 `<configuration>`  
  
 `<system.runtime.serialization>`  
  
 `<dataContractSerializer>`  
  
 `<declaredTypes>`  
  
 `<add type="MyCompany.Library.Shape,`  
  
 `MyAssembly, Version=2.0.0.0, Culture=neutral,`  
  
 `PublicKeyToken=XXXXXX, processorArchitecture=MSIL">`  
  
 `<knownType type="MyCompany.Library.Circle,`  
  
 `MyAssembly, Version=2.0.0.0, Culture=neutral,`  
  
 `PublicKeyToken=XXXXXX, processorArchitecture=MSIL"/>`  
  
 `</add>`  
  
 `</declaredTypes>`  
  
 `</dataContractSerializer>`  
  
 `</system.runtime.serialization>`  
  
 `</configuration>`  
  
 在前面的配置文件中，名为 `MyCompany.Library.Shape` 的数据协定类型被声明具有已知类型 `MyCompany.Library.Circle` 。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.KnownTypeAttribute>
- <xref:System.Collections.Hashtable>
- <xref:System.Object>
- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.DataContractSerializer.KnownTypes%2A>
- [已知类型](../samples/known-types.md)
- [数据协定等效性](data-contract-equivalence.md)
- [设计服务协定](../designing-service-contracts.md)
