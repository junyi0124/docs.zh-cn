---
title: 比较 COM+ 和 ServiceModel 中的事务
ms.date: 03/30/2017
ms.assetid: e493bcdd-b91a-4486-853f-83dbcd1931b7
ms.openlocfilehash: 30ecbd374e909141dbc944740f90c1b41ac44ed2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96264903"
---
# <a name="comparing-transactions-in-com-and-servicemodel"></a>比较 COM+ 和 ServiceModel 中的事务

本主题讨论如何使用命名空间提供的 Windows Communication Foundation (WCF) 特性来模拟事务性 COM + 服务的行为 <xref:System.ServiceModel> 。  
  
## <a name="emulating-com-using-servicemodel-attributes"></a>使用 ServiceModel 属性模拟 COM+  

 下表比较了 <xref:System.EnterpriseServices.TransactionOption> 用于创建事务的枚举 `EnterpriseServices` ，以及它们如何与 <xref:System.ServiceModel> 命名空间提供的 WCF 属性关联。  
  
|COM+ 属性|WCF 特性|  
|---------------------|------------------------------------------------------------------------|  
|RequiresNew|将 <xref:System.ServiceModel.TransactionFlowAttribute> 设置为 <xref:System.ServiceModel.TransactionFlowOption.NotAllowed>。<br /><br /> <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 为 `true`。<br /><br /> 绑定元素中的 `TransactionFlow` 属性为 `false`。|  
|必选|将 <xref:System.ServiceModel.TransactionFlowAttribute> 设置为 <xref:System.ServiceModel.TransactionFlowOption.Allowed>。<br /><br /> <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 为 `true`。<br /><br /> 绑定元素中的 `TransactionFlow` 属性为 `true`。|  
|支持|没有直接等效项。 通常，您应采用为 `Required` 指定的行为。|  
|NotSupported|<xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 为 `false`。<br /><br /> 绑定元素中的 `TransactionFlow` 属性为 `false`。|  
|已禁用|没有直接等效项。 通常，您应采用为 `NotSupported` 指定的行为。|
