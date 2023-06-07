---
title: WCF Discovery
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], discovery
- Windows Communication Foundation [WCF], discovery
- discovery [WCF]
ms.assetid: 462c4913-f388-45a9-9042-28ae96a4e735
ms.openlocfilehash: 176e9760d98f9640bd9d1c7b059287dc29c0d666
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289355"
---
# <a name="wcf-discovery"></a>WCF Discovery

Windows Communication Foundation (WCF) 允许在运行时使用 WS-Discovery 协议以可互操作的方式在运行时发现服务。 WCF 服务可以使用多播消息或发现代理服务器向网络公布其可用性。 客户端应用程序可以搜索网络或发现代理服务器来查找满足一组条件的服务。 本节中的主题分别概述和详细介绍了此功能的编程模型。  
  
## <a name="in-this-section"></a>本节内容  

 [WCF Discovery 概述](wcf-discovery-overview.md)  
 概述 WCF 提供的 WS-Discovery 支持。  
  
 [WCF Discovery 对象模型](wcf-discovery-object-model.md)  
 描述对象模型中的类和 WS-Discovery 支持的扩展性。  
  
 [如何：以编程方式向 WCF 服务和客户端添加可发现性](how-to-programmatically-add-discoverability-to-a-wcf-service-and-client.md)  
 演示如何使 Windows Communication Foundation (WCF) 服务可发现。  
  
 [实现发现代理](implementing-a-discovery-proxy.md)  
 描述一些必需步骤，这些步骤用于实现发现代理、注册到发现代理的可检测服务以及使用发现代理查找可检测服务的客户端。  
  
 [发现版本控制](discovery-versioning.md)  
 简要概述了一些新发现功能的原型实现， 还概述了如何选择要使用的发现版本。  
  
 [在配置文件中配置发现](configuring-discovery-in-a-configuration-file.md)  
 演示如何在配置中配置 Discovery。  
  
 [使用 Discovery 客户端通道](using-the-discovery-client-channel.md)  
 演示如何在编写 WCF 客户端应用程序时使用发现客户端通道。
