---
title: 配置 WCF 服务
ms.date: 03/30/2017
helpviewer_keywords:
- configuration [WCF]
ms.assetid: beac771e-f28e-4f84-9ff1-ad9251c726d3
ms.openlocfilehash: a5fe0cabb1a6be7f93bf5f4d753e9bb08a39cea3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253331"
---
# <a name="configuring-wcf-services"></a>配置 WCF 服务

在设计和实现服务协定后，即可配置服务。 在其中可以定义和自定义如何向客户端公开服务，包括指定可以找到服务的地址、服务用于发送和接收消息的传输和消息编码，以及服务需要的安全类型。  
  
 此处使用的配置包括在代码中以强制方式或通过使用配置文件定义和自定义服务的各个方面的所有方法，例如指定其终结点地址、使用的传输及其安全方案。 实际上，编写配置是 WCF 应用程序编程的主要部分。  
  
## <a name="in-this-section"></a>本节内容  

 [简化配置](simplified-configuration.md)  
 从 .NET Framework 4 开始，WCF 附带了新的默认配置模型，该模型可简化 WCF 配置要求。 如果没有为特定服务提供任何 WCF 配置，运行时将使用默认终结点、绑定和行为自动配置服务。  
  
 [使用配置文件配置服务](configuring-services-using-configuration-files.md)  
 Windows Communication Foundation (WCF) 服务可使用 .NET Framework 配置技术进行配置。 最常见的情况是，XML 元素添加到承载 WCF 服务的 Internet Information Services (IIS) 站点的 Web.config 文件中。 这些元素允许您更改详细信息，例如每台计算机上的终结点地址（用于与服务进行通信的实际地址）。  
  
 [绑定](bindings.md)  
 此外，WCF 还包括多个系统提供的常见配置，这些绑定可以让你快速选择有关客户端和服务的通信方式的最基本功能，如使用的传输、安全性和消息编码。  
  
 [Endpoints](endpoints.md)  
 与 WCF 服务的所有通信都通过服务 *终结点* 进行。 终结点包含协定、在绑定中指定的配置信息，以及指示查找服务或获取该服务相关信息的位置的地址。  
  
 [保证服务的安全](securing-services.md)  
 使用 WCF 和现有的安全机制，可以在任何服务中实现保密性、完整性、身份验证和授权。 还可以审核安全成功和安全失败。  
  
 [创建 WS-I 基本配置文件 1.1 可互操作服务](./creating-ws-i-basic-profile-1-1-interoperable-services.md)  
 WS-I 基本配置文件 1.1 规范中概述了部署服务的需求，该服务可与其他任何平台或操作系统上的服务和客户端进行互操作。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel>  
  
 <xref:System.ServiceModel.Channels>  
  
 <xref:System.ServiceModel.Description>  
  
## <a name="related-sections"></a>相关章节  

 [基本编程生命周期](basic-programming-lifecycle.md)  
  
 [设计和实现服务](designing-and-implementing-services.md)  
  
 [承载服务](hosting-services.md)  
  
 [生成客户端](building-clients.md)  
  
 [扩展性介绍](introduction-to-extensibility.md)  
  
 [管理和诊断](./diagnostics/index.md)  
  
## <a name="see-also"></a>请参阅

- [基本 WCF 编程](basic-wcf-programming.md)
- [概念性概述](conceptual-overview.md)
- [WCF 功能详细信息](./feature-details/index.md)
