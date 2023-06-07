---
title: 如何：指定要对哪些成员进行并发冲突测试
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d2cda293-1e2f-4878-af0e-5aaf0d092120
ms.openlocfilehash: e774935827934ae73873247def049b4045535272
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197139"
---
# <a name="how-to-specify-which-members-are-tested-for-concurrency-conflicts"></a>如何：指定要对哪些成员进行并发冲突测试

将三个枚举中的一个枚举应用于属性 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 上的属性 <xref:System.Data.Linq.Mapping.ColumnAttribute> ，以指定要包含在更新检查中的成员，以检测是否存在开放式并发冲突。  
  
 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 属性（在设计时映射）与 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中的运行时并发功能一起使用。 有关详细信息，请参阅 [乐观 Concurrency：概述](optimistic-concurrency-overview.md)。  
  
> [!NOTE]
> 只要未将任何成员指定为 `IsVersion=true`，就会将原始成员值与当前数据库状态进行比较。 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A>。  
  
 有关代码示例，请参见<xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A>。  
  
### <a name="to-always-use-this-member-for-detecting-conflicts"></a>始终使用此成员检测冲突  
  
1. 将 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 属性 (Property) 添加到 <xref:System.Data.Linq.Mapping.ColumnAttribute> 属性 (Attribute)。  
  
2. 将 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 属性值设置为 `Always`。  
  
### <a name="to-never-use-this-member-for-detecting-conflicts"></a>永不使用此成员检测冲突  
  
1. 将 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 属性 (Property) 添加到 <xref:System.Data.Linq.Mapping.ColumnAttribute> 属性 (Attribute)。  
  
2. 将 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 属性值设置为 `Never`。  
  
### <a name="to-use-this-member-for-detecting-conflicts-only-when-the-application-has-changed-the-value-of-the-member"></a>仅在应用程序已更改此成员的值时才使用此成员检测冲突  
  
1. 将 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 属性 (Property) 添加到 <xref:System.Data.Linq.Mapping.ColumnAttribute> 属性 (Attribute)。  
  
2. 将 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 属性值设置为 `WhenChanged`。  
  
## <a name="example"></a>示例  

 下面的示例指定在更新检查期间永远都不应该测试 `HomePage` 对象。 有关详细信息，请参阅 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A>。  
  
 [!code-csharp[System.Data.Linq.Mapping.UpdateCheck#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.mapping.updatecheck/cs/northwind.cs#1)]
 [!code-vb[System.Data.Linq.Mapping.UpdateCheck#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.mapping.updatecheck/vb/northwind.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [如何：管理更改冲突](how-to-manage-change-conflicts.md)
- [进行和提交数据更改](making-and-submitting-data-changes.md)
