---
title: 如何：创建类或结构的基本数据协定
description: 按照此示例，了解如何使用 DataContractAttribute 特性在 WCF 中创建使用类或结构的数据协定。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataMemberAttribute
- DataContractAttribute class
- data contracts [WCF], creating for a class or structure
ms.assetid: bc464889-3070-4a2f-91d2-e788a0f686a7
ms.openlocfilehash: 5634eb3d3ec18d95fd7d6b3c89b572ab4f5b8eca
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254033"
---
# <a name="how-to-create-a-basic-data-contract-for-a-class-or-structure"></a>如何：创建类或结构的基本数据协定

本主题演示使用类或结构创建数据协定的基本步骤。 有关数据协定及其使用方式的详细信息，请参阅 [使用数据协定](using-data-contracts.md)。  
  
 有关演练创建基本 Windows Communication Foundation (WCF) 服务和客户端的步骤的教程，请参阅 [入门教程](../getting-started-tutorial.md)。 对于包含基本服务和客户端的工作示例应用程序，请参阅 [基本数据协定](../samples/basic-data-contract.md)。  
  
### <a name="to-create-a-basic-data-contract-for-a-class-or-structure"></a>创建类或结构的基本数据协定  
  
1. 通过将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于类来声明该类型具有数据协定。 请注意，包括不带属性的公共类型在内的所有公共类型都是可序列化的。 如果不存在 <xref:System.Runtime.Serialization.DataContractSerializer> 属性，<xref:System.Runtime.Serialization.DataContractAttribute> 将推断出一个数据协定。 有关详细信息，请参阅 [可序列化类型](serializable-types.md)。  
  
2. 通过将 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性 (Attribute) 应用于每个成员来定义要序列化的成员（属性 (Property)、字段或事件）。 这些成员称为数据成员。 默认情况下，所有公共类型都是可序列化的。 有关详细信息，请参阅 [可序列化类型](serializable-types.md)。  
  
    > [!NOTE]
    > 您可以将 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性应用于私有字段，这会导致向其他人公开此数据。 请确保成员不包含敏感数据。  
  
## <a name="example"></a>示例  

 下面的示例演示如何通过将 `Person` 和 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于类及其成员来创建 <xref:System.Runtime.Serialization.DataMemberAttribute> 类型的数据协定。  
  
 [!code-csharp[DataContractAttribute#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#2)]
 [!code-vb[DataContractAttribute#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#2)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- [使用数据协定](using-data-contracts.md)
- [入门教程](../getting-started-tutorial.md)
- [入门](../samples/getting-started-sample.md)
