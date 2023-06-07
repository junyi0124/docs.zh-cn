---
title: Serialization2
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a15ae411-8dc2-4ca3-84d2-01c9d5f1972a
ms.openlocfilehash: 778cc73575ffc7421854fd89592f1c4eaa284678
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203548"
---
# <a name="serialization"></a>序列化

本主题介绍 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 序列化功能。 下面几段提供了有关在设计时如何在代码生成期间添加序列化以及 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 类的运行时序列化行为的信息。  
  
 您可以通过以下任一方法在设计时添加序列化代码：  
  
- 在对象关系设计器中，将 " **序列化模式** " 属性更改为 " **单向**"。  
  
- 在 SQLMetal 命令行中，添加 **/serialization** 选项。 有关详细信息，请参阅 [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)。  
  
## <a name="overview"></a>概述  

 由生成的代码 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 默认提供延迟加载功能。 延迟加载对在中间层以透明方式根据需要加载数据而言非常方便。 但是，它会给序列化过程带来问题，原因是不论是否需要进行延迟加载，序列化程序都会触发延迟加载。 实际上，对对象进行序列化时，会对其在所有出站延迟加载引用下的可传递闭包进行序列化。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 的序列化功能主要通过以下两种机制来解决此问题：  
  
- 用于关闭延迟加载的 <xref:System.Data.Linq.DataContext> 模式 (<xref:System.Data.Linq.DataContext.ObjectTrackingEnabled%2A>)。 有关详细信息，请参阅 <xref:System.Data.Linq.DataContext>。  
  
- 用于在生成的实体中生成 <xref:System.Runtime.Serialization.DataContractAttribute?displayProperty=nameWithType> 和 <xref:System.Runtime.Serialization.DataMemberAttribute?displayProperty=nameWithType> 属性的代码生成开关。 这方面（包括处于序列化过程中的延迟加载类的行为）是本主题的主要介绍对象。  
  
### <a name="definitions"></a>定义  
  
- *DataContract 序列化程序*： Windows Communication Framework 使用的默认序列化程序 (.NET Framework 3.0 或更高版本的 WCF) 组件。  
  
- *单向序列化*：仅包含单向关联属性的类的序列化版本 (以避免循环) 。 按照约定，主键-外键关系的父级端的属性标记为序列化。 双向关联中的另一端不进行序列化。  
  
     单向序列化是 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 支持的唯一一种序列化。  
  
## <a name="code-example"></a>代码示例  

 下面的代码使用 Northwind 示例数据库中的传统 `Customer` 和 `Order` 类，并演示了如何用序列化属性修饰这些类。  
  
 [!code-csharp[DLinqSerialization#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSerialization/cs/northwind-ser.cs#1)]
 [!code-vb[DLinqSerialization#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSerialization/vb/northwind-ser.vb#1)]  
  
 [!code-csharp[DLinqSerialization#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSerialization/cs/northwind-ser.cs#2)]
 [!code-vb[DLinqSerialization#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSerialization/vb/northwind-ser.vb#2)]  
  
 [!code-csharp[DLinqSerialization#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSerialization/cs/northwind-ser.cs#3)]
 [!code-vb[DLinqSerialization#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSerialization/vb/northwind-ser.vb#3)]  
  
 对于下例中的 `Order` 类，为简洁起见，只显示与 `Customer` 类对应的反向关联属性。 它没有用来避免出现循环的 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性。  
  
 [!code-csharp[DLinqSerialization#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSerialization/cs/northwind-ser.cs#4)]
 [!code-vb[DLinqSerialization#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSerialization/vb/northwind-ser.vb#4)]  
  
 [!code-csharp[DLinqSerialization#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSerialization/cs/northwind-ser.cs#5)]
 [!code-vb[DLinqSerialization#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSerialization/vb/northwind-ser.vb#5)]  
  
### <a name="how-to-serialize-the-entities"></a>如何序列化实体  

 你可以按如下方式序列化上一节中显示的代码中的实体：  
  
 [!code-csharp[DLinqSerialization#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSerialization/cs/Program.cs#6)]
 [!code-vb[DLinqSerialization#6](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSerialization/vb/Module1.vb#6)]  
  
### <a name="self-recursive-relationships"></a>自递归关系  

 自递归关系遵循相同的模式。 与外键对应的关联属性 (Property) 不具有 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性 (Attribute)，而父属性 (Property) 则具有。  
  
 请考虑以下具有两个自递归关系的类：Employee.Manager/Reports 和 Employee.Mentor/Mentees。  
  
 [!code-csharp[DLinqSerialization#7](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSerialization/cs/northwind-ser.cs#7)]
 [!code-vb[DLinqSerialization#7](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSerialization/vb/northwind-ser.vb#7)]  
  
## <a name="see-also"></a>请参阅

- [背景信息](background-information.md)
- [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)
- [如何：使实体可序列化](how-to-make-entities-serializable.md)
