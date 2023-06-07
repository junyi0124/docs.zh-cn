---
title: 单并发模式中的有序消息处理
ms.date: 03/30/2017
ms.assetid: a90f5662-a796-46cd-ae33-30a4072838af
ms.openlocfilehash: d70087a6dc1501f9a7f7ed057eae3dad181761ae
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248014"
---
# <a name="ordered-processing-of-messages-in-single-concurrency-mode"></a>单并发模式中的有序消息处理

WCF 不会保证消息的处理顺序，除非基础通道是会话的。  例如，使用 MsmqInputChannel 的 WCF 服务（不是会话通道）将无法按顺序处理消息。 在某些情况下，开发人员可能希望按顺序处理行为，但不希望使用会话。 本主题介绍在单一并发模式中运行服务时，如何配置这种行为。  
  
## <a name="in-order-message-processing"></a>有序消息处理  

 名为 <xref:System.ServiceModel.ServiceBehaviorAttribute.EnsureOrderedDispatch%2A> 的新特性已添加到 <xref:System.ServiceModel.ServiceBehaviorAttribute>。 当 <xref:System.ServiceModel.ServiceBehaviorAttribute.EnsureOrderedDispatch%2A> 设置为 true 并且<xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> 设置为 <xref:System.ServiceModel.ConcurrencyMode.Single> 时，将按顺序处理发送到服务的消息。 下面的代码段演示如何设置这些特性。  
  
```csharp
[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Single, EnsureOrderedDispatch = true )]  
    class Service : IService  
    {  
         // ...  
    }  
```  
  
 如果 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> 设置为任何其他值，则引发 <xref:System.InvalidOperationException>。  
  
## <a name="see-also"></a>另请参阅

- [会话、实例化和并发](sessions-instancing-and-concurrency.md)
- [并发](../samples/concurrency.md)
