---
title: 数据协定中的枚举类型
description: 了解数据协定模型如何将枚举表达为 WFC 编程模型的一部分。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], enumeration types
ms.assetid: b5d694da-68cb-4b74-a5fb-75108a68ec3b
ms.openlocfilehash: 88bf2513435a9c00cf11a0681b32871992c8d2b2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276654"
---
# <a name="enumeration-types-in-data-contracts"></a>数据协定中的枚举类型

枚举可以用数据协定模型来表示。 本主题演练几个介绍编程模型的示例。  
  
## <a name="enumeration-basics"></a>枚举基础知识  

 若要使用以数据协定模型表示的枚举类型，一种方法就是将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于该类型。 然后，必须将 <xref:System.Runtime.Serialization.EnumMemberAttribute> 属性应用于每个必须在数据协定中包含的成员。  
  
 下面的示例演示了两个类。 第一个类使用枚举，第二个类定义枚举。  
  
 [!code-csharp[c_DataContractEnumerations#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#1)]
 [!code-vb[c_DataContractEnumerations#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#1)]  
  
 仅当将 `Car` 字段的值设置为 `condition`、`New` 或 `Used` 之一时，才可发送或接收 `Rental` 类的实例。 如果 `condition` 为 `Broken` 或 `Stolen`，则会引发一个 <xref:System.Runtime.Serialization.SerializationException>。  
  
 可以照常将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性（<xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> 和 <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A>）用于枚举数据协定。  
  
### <a name="enumeration-member-values"></a>枚举成员值  

 通常数据协定包括枚举成员名称，而不包括数值。 但是，在使用数据协定模型时，如果接收方是 WCF 客户端，则导出的架构将保留数字值。 请注意，使用 [XmlSerializer 类](using-the-xmlserializer-class.md)时不会出现这种情况。  
  
 在上面的示例中，如果将 `condition` 设置为 `Used` 并且将数据序列化为 XML，则生成的 XML 是 `<condition>Used</condition>` 而不是 `<condition>1</condition>`。 因此，下面的数据协定等效于 `CarConditionEnum` 的数据协定。  
  
 [!code-csharp[c_DataContractEnumerations#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#2)]
 [!code-vb[c_DataContractEnumerations#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#2)]  
  
 例如，您可以在发送端使用 `CarConditionEnum` 并且在接收端使用 `CarConditionWithNumbers`。 尽管发送端的 `Used` 使用值“1”而接收端使用值“20”，但是两端的 XML 表示形式均为 `<condition>Used</condition>`。  
  
 若要包含在数据协定中，必须应用 <xref:System.Runtime.Serialization.EnumMemberAttribute> 属性。 在 .NET Framework 中，始终可以将特殊值 0 (零) 应用于枚举，这也是任何枚举的默认值。 但是，即使对于此特殊值零，也只能使用 <xref:System.Runtime.Serialization.EnumMemberAttribute> 属性标记才能进行序列化。  
  
 以下为此规则的两种例外情况：  
  
- 标志枚举（本主题的后面部分将进行讨论）。  
  
- <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A> 属性设置为 `false` 的枚举数据成员（这种情况下将从序列化数据中省略值为零的枚举）。  
  
### <a name="customizing-enumeration-member-values"></a>自定义枚举成员值  

 可以通过使用 <xref:System.Runtime.Serialization.EnumMemberAttribute.Value%2A> 属性 (attribute) 的 <xref:System.Runtime.Serialization.EnumMemberAttribute> 属性 (property) 对作为数据协定构成部分的枚举成员值进行自定义。  
  
 例如，下面的数据协定也等效于 `CarConditionEnum` 的数据协定。  
  
 [!code-csharp[c_DataContractEnumerations#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#3)]
 [!code-vb[c_DataContractEnumerations#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#3)]  
  
 在序列化时，`PreviouslyOwned` 的值的 XML 表示形式为 `<condition>Used</condition>`。  
  
## <a name="simple-enumerations"></a>简单枚举  

 您还可以对未向其应用 <xref:System.Runtime.Serialization.DataContractAttribute> 属性的枚举类型进行序列化。 可以按照上述方式来处理这种枚举类型，只不过将每个成员（这些成员未应用 <xref:System.NonSerializedAttribute> 属性）视为已应用了 <xref:System.Runtime.Serialization.EnumMemberAttribute> 属性。 例如，下面的枚举隐式具有一个与上面的 `CarConditionEnum` 示例等效的数据协定。  
  
 [!code-csharp[c_DataContractEnumerations#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#6)]
 [!code-vb[c_DataContractEnumerations#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#6)]  
  
 如果您不需要自定义枚举的数据协定名称、命名空间以及枚举成员值，则可以使用简单枚举。  
  
#### <a name="notes-on-simple-enumerations"></a>简单枚举说明  

 将 <xref:System.Runtime.Serialization.EnumMemberAttribute> 属性应用于简单枚举是无效的。  
  
 是否将 <xref:System.SerializableAttribute> 属性应用于简单枚举并没有任何区别。  
  
 <xref:System.Runtime.Serialization.DataContractSerializer> 类采用应用于枚举成员的 <xref:System.NonSerializedAttribute> 属性的事实不同于 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 和 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> 的行为。 这两种序列化程序都忽略 <xref:System.NonSerializedAttribute> 属性。  
  
## <a name="flag-enumerations"></a>标志枚举  

 可以将 <xref:System.FlagsAttribute> 属性应用于枚举。 在这种情况下，可以同时发送或接收包含零个或多个枚举值的列表。  
  
 为此，请将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于标志枚举，然后使用 <xref:System.Runtime.Serialization.EnumMemberAttribute> 属性对所有为 2 的幂的成员进行标记。 请注意，若要使用标志枚举，级数必须为不间断的 2 的幂的序列（例如，1、2、4、8、16、32、64）。  
  
 可以使用下面的步骤来发送标志的枚举值：  
  
1. 尝试查找映射到数值的枚举成员（应用了 <xref:System.Runtime.Serialization.EnumMemberAttribute> 属性）。 如果可以找到，请发送仅包含该成员的列表。  
  
2. 尝试将此数值分解为和的形式，以便枚举成员（每个成员都应用了 <xref:System.Runtime.Serialization.EnumMemberAttribute> 属性）可以映射到和的各个部分。 发送包含所有这些成员的列表。 请注意， *贪婪算法* 用于查找这样的总和，因此不能保证即使存在此类 sum 也是如此。 为避免出现这种问题，请确保枚举成员的数值为 2 的幂。  
  
3. 如果上面的两个步骤均无法实现并且数值为非零，则引发一个 <xref:System.Runtime.Serialization.SerializationException>。 如果数值为零，则发送空列表。  
  
### <a name="example"></a>示例  

 下面的枚举示例可用于标志操作。  
  
 [!code-csharp[c_DataContractEnumerations#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#4)]
 [!code-vb[c_DataContractEnumerations#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#4)]  
  
 下面的示例值将按照指示的方式进行序列化。  
  
 [!code-csharp[c_DataContractEnumerations#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#5)]
 [!code-vb[c_DataContractEnumerations#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#5)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- [使用数据协定](using-data-contracts.md)
- [在服务协定中指定数据传输](specifying-data-transfer-in-service-contracts.md)
