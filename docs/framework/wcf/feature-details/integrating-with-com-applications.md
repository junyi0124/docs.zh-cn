---
title: 与 COM 应用程序集成
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, COM integration
- COM [WCF], Windows Communication Foundation integration
- WCF, reusing code
- Windows Communication Foundation, reusing code
- COM [WCF]
- WCF, COM integration
ms.assetid: c98bda3e-6779-419e-8e6d-9aa94053026d
ms.openlocfilehash: bc58e22b64284d66367302d55b5c9554c9ec0d72
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96268230"
---
# <a name="integrating-with-com-applications"></a>与 COM 应用程序集成

Windows Communication Foundation (WCF) 服务可以使用 WCF 服务名字对象直接集成到现有代码中。 服务标记可以在众多基于 COM 的开发环境（如 Office VBA、Visual Basic 6.0 或 Visual C++ 6.0）中使用。  
  
## <a name="in-this-section"></a>本节内容  

 [COM 应用程序集成概述](integrating-with-com-applications-overview.md)  
 对集成过程的主要部分进行概述。  
  
 [如何：注册和配置服务名字对象](how-to-register-and-configure-a-service-moniker.md)  
 若要在 COM 应用程序中使用 WCF 服务标记，请将所需的属性化类型注册到 COM，并使用所需的绑定配置来配置 COM 应用程序和标记。  
  
 [如何：使用未注册的 Windows Communication Foundation 服务名字对象](use-the-wcf-service-moniker-without-registration.md)  
 说明如何以 Web 服务定义语言 (WSDL) 文档的形式或者从 WS-MetadataExchange 终结点获取协定的定义。  
  
 [如何：将服务名字对象用于 WSDL 协定](how-to-use-a-service-moniker-with-wsdl-contracts.md)  
 介绍如何使用 WCF WSDL 名字对象调用 WCF 示例。  
  
 [如何：将服务名字对象与元数据交换协定一起使用](how-to-use-a-service-moniker-with-metadata-exchange-contracts.md)  
 介绍如何使用指定 Mex 终结点的 WCF 名字对象调用 WCF 示例。  
  
 [如何：指定通道安全凭据](how-to-specify-channel-security-credentials.md)  
 WCF 服务标记支持 `IChannelCredentials` 接口，该接口允许使用一系列替代方法来指定通道凭据。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel>  
  
## <a name="see-also"></a>请参阅

- [与 COM + 应用程序集成](integrating-with-com-plus-applications.md)
