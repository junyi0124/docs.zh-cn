---
title: 实现发现代理
ms.date: 03/30/2017
ms.assetid: dda20e79-8df3-438e-a281-69d779d978ec
ms.openlocfilehash: ae003c89bb0f14623c5d31a1596533d821380336
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96268295"
---
# <a name="implementing-a-discovery-proxy"></a>实现发现代理

本节说明实现发现代理所需执行的步骤。 发现代理是包含服务存储库的独立服务。 客户端可以查询发现代理，以便查找该代理已知的可检测服务。 使用服务填充代理的方式由实施者决定。 例如，发现代理可以连接到现有服务存储库并使该信息可供检测，管理员可以使用管理 API 向代理添加可检测服务，或者发现代理也可以使用公告功能更新其内部缓存。  
  
 WCF 实现提供可使您轻松生成代理的基类。 可以利用这些 API 在现有存储库之上生成发现代理。  
  
 此处实现的发现代理与任何其他 WCF 服务类似，您还可以在其中使发现代理可发现，并使客户端定位其终结点。  
  
## <a name="in-this-section"></a>本节内容  

 [如何：实现发现代理](how-to-implement-a-discovery-proxy.md)  
 说明如何实现发现代理。  
  
 [如何：实现向发现代理注册的可发现的服务](discoverable-service-that-registers-with-the-discovery-proxy.md)  
 介绍如何实现使用发现代理注册的可发现的 WCF 服务。  
  
 [如何：实现使用发现代理查找服务的客户端应用程序](client-app-discovery-proxy-to-find-a-service.md)  
 描述如何实现使用发现代理搜索服务的 WCF 客户端应用程序。  
  
 [如何：测试发现代理](how-to-test-the-discovery-proxy.md)  
 说明如何测试前面三个主题中编写的代码。  
  
## <a name="see-also"></a>另请参阅

- [WCF Discovery](wcf-discovery.md)
- [如何：以编程方式向 WCF 服务和客户端添加可发现性](how-to-programmatically-add-discoverability-to-a-wcf-service-and-client.md)
