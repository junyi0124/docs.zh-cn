---
title: 开发人员在重写默认行为中的责任
ms.date: 03/30/2017
ms.assetid: c6909ddd-e053-46a8-980c-0e12a9797be1
ms.openlocfilehash: 88a7076e12e39ed23aa6d661aa90f74258f3dded
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200428"
---
# <a name="responsibilities-of-the-developer-in-overriding-default-behavior"></a>开发人员在重写默认行为中的责任

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不会强制执行以下要求，但如果不满足这些要求，则行为是不确定的。  
  
- 重写方法不能调用<xref:System.Data.Linq.DataContext.SubmitChanges%2A> 或 <xref:System.Data.Linq.Table%601.Attach%2A>。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 如果在重写方法中调用这些方法，则将引发异常。  
  
- 重写方法不能用来启动、提交或停止事务。 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 操作以事务的形式执行。 内嵌的事务可能会干扰外层事务。 加载重写方法只能在它们确定 <xref:System.Transactions.Transaction> 中未在执行相应操作后启动事务。  
  
- 重写方法应遵循适用的开放式并发映射。 发生开放式并发冲突时，重写方法应引发 <xref:System.Data.Linq.ChangeConflictException>。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 捕获此异常，以便您可以正确处理 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 在上提供的 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 选项。  
  
- Create (`Insert`) 和 `Update` 重写方法在相应操作成功完成后应使数据库生成的列的值流回对应的对象成员。  
  
     例如，如果 `Order.OrderID` 映射到标识列 (*自动增量* 主键) ，则 `InsertOrder()` 重写方法必须检索数据库生成的 id，并将 `Order.OrderID` 成员设置为该 id。 同样，时间戳成员必须更新为数据库生成的时间戳值，以确保更新后的对象一致。 如果未能传播数据库生成的值，则会造成数据库与 <xref:System.Data.Linq.DataContext> 跟踪的对象之间不一致。  
  
- 调用正确的动态 API 是用户的责任。 例如，在更新重写方法中，只能调用 <xref:System.Data.Linq.DataContext.ExecuteDynamicUpdate%2A>。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不检测或验证调用的动态方法是否与适用的操作相匹配。 如果调用了不适用的方法（例如，为要更新的对象调用了 <xref:System.Data.Linq.DataContext.ExecuteDynamicDelete%2A>），则结果是不明确的。  
  
- 最后，重写方法应执行明确的操作。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 操作（如紧急加载、延迟加载和 <xref:System.Data.Linq.DataContext.SubmitChanges%2A>）的语义要求重写提供明确的服务。 例如，只返回空集合而不检查数据库内容的加载重写有可能会造成数据不一致。  
  
## <a name="see-also"></a>请参阅

- [自定义插入、更新和删除操作](customizing-insert-update-and-delete-operations.md)
