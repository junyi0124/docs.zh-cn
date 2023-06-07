---
title: 保护服务和客户端的安全
ms.date: 03/30/2017
helpviewer_keywords:
- message security [WCF]
ms.assetid: e681f3bd-0c09-4a58-b0e4-0ecbdf1aa6c7
ms.openlocfilehash: 24e7cc3fd9b349aaba733a809a03be2454bd371d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288393"
---
# <a name="securing-services-and-clients"></a>保护服务和客户端的安全

本部分中的信息重点介绍 Windows Communication Foundation (WCF) 的编程安全性。 通常，这包括选择系统提供的相应绑定、设置安全元素的属性，然后设置服务行为的属性（控制检索凭据以供服务或客户端使用的方式）。 这些技术涵盖大多数用户在大多数情况下的安全要求，如 [常见安全方案](common-security-scenarios.md)中所示。 如果你的方案需要更多功能，请首先查看 [具有自定义绑定的安全功能](security-capabilities-with-custom-bindings.md);如果解决方案不明显，请参阅 [扩展安全性](../extending/extending-security.md)。 如果要创建 (或与) 使用丰富声明的系统进行互操作，请参阅 [授权](authorization-in-wcf.md)中的主题。  
  
## <a name="in-this-section"></a>本节内容  

 [WCF 安全编程](programming-wcf-security.md)  
 用于保护消息安全的编程模型概述。  
  
 [传输安全概述](transport-security-overview.md)  
 如何通过传输层保护消息安全概述。  
  
 [消息安全](message-security-in-wcf.md)  
 汇总在 Windows Communication Foundation 中使用消息级安全性 (WCF) 的原因。  
  
 [安全会话](secure-sessions.md)  
 讨论在保护 WCF 会话时需要注意的事项。  
  
 [使用证书](working-with-certificates.md)  
 说明使用 X.509 证书时必须完成的一些常见任务。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel>  
  
 <xref:System.ServiceModel.Channels>  
  
 <xref:System.ServiceModel.Security>  
  
## <a name="related-sections"></a>相关章节  

 [安全性概念](security-concepts.md)  
  
 [扩展安全性](../extending/extending-security.md)  
  
 [常用安全方案](common-security-scenarios.md)  
  
 [绑定与安全](bindings-and-security.md)  
  
 [使用自定义绑定的安全功能](security-capabilities-with-custom-bindings.md)  
  
 [扩展安全性](../extending/extending-security.md)  
  
 [授权](authorization-in-wcf.md)  
  
## <a name="see-also"></a>请参阅

- [基本 WCF 编程](../basic-wcf-programming.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
