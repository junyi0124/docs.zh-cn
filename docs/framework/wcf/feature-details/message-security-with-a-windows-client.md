---
title: Windows 客户端的消息安全
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 01e7d0b8-10f9-45c3-a4c5-53d44dc61eb8
ms.openlocfilehash: 1fe50f711c65871b811837a7f48cf6f45f4455b4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275601"
---
# <a name="message-security-with-a-windows-client"></a>Windows 客户端的消息安全

此方案显示 Windows Communication Foundation (WCF) 客户端和服务器安全模式保护的服务器。 客户端和服务使用 Windows 凭据进行身份验证。  
  
 ![Windows 客户端的消息安全性](media/1c8618d4-0005-4022-beb6-32fd087a8c3c.gif "1c8618d4-0005-4022-beb6-32fd087a8c3c")  
  
|特征|说明|  
|--------------------|-----------------|  
|安全模式|消息|  
|互操作性|仅 WCF|  
|身份验证（服务器）|服务器和客户端的相互身份验证|  
|身份验证（客户端）|服务器和客户端的相互身份验证|  
|完整性|是，使用共享安全上下文|  
|机密性|是，使用共享安全上下文|  
|Transport|NET.TCP|  
|绑定|<xref:System.ServiceModel.NetTcpBinding>|  
  
## <a name="service"></a>服务  

 下面的代码和配置应独立运行。 执行下列操作之一：  
  
- 使用代码（而不使用配置）创建独立服务。  
  
- 使用提供的配置创建服务，但不定义任何终结点。  
  
### <a name="code"></a>代码  

 下面的代码演示如何创建一个使用消息安全来建立 Windows 计算机安全上下文的服务终结点。  
  
 [!code-csharp[C_SecurityScenarios#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#11)]
 [!code-vb[C_SecurityScenarios#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#11)]  
  
### <a name="configuration"></a>Configuration  

 下面的配置可代替代码用于设置服务：  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service behaviorConfiguration=""  
               name="ServiceModel.Calculator">  
        <endpoint address="net.tcp://localhost:8008/Calculator"  
                  binding="netTcpBinding"  
                  bindingConfiguration="Windows"  
                  name="WindowsOverMessage"  
                  contract="ServiceModel.ICalculator" />  
      </service>  
    </services>  
    <bindings>  
      <netTcpBinding>  
        <binding name="Windows">  
          <security mode="Message">  
            <message clientCredentialType="Windows" />  
          </security>  
        </binding>  
      </netTcpBinding>  
    </bindings>  
    <client />  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="client"></a>客户端  

 下面的代码和配置应独立运行。 执行下列操作之一：  
  
- 使用代码（和客户端代码）创建独立客户端。  
  
- 创建不定义任何终结点地址的客户端。 而使用将配置名称作为自变量的客户端构造函数。 例如：  
  
     [!code-csharp[C_SecurityScenarios#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#0)]
     [!code-vb[C_SecurityScenarios#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#0)]  
  
### <a name="code"></a>代码  

 下面的代码创建客户端。 绑定设置为 Message 安全模式，客户端凭据类型设置为 `Windows`。  
  
 [!code-csharp[C_SecurityScenarios#18](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#18)]
 [!code-vb[C_SecurityScenarios#18](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#18)]  
  
### <a name="configuration"></a>Configuration  

 下面的配置用于设置客户端属性。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <bindings>  
      <netTcpBinding>  
        <binding name="NetTcpBinding_ICalculator" >  
         <security mode="Message">  
            <message clientCredentialType="Windows" />  
          </security>  
        </binding>  
      </netTcpBinding>  
    </bindings>  
    <client>  
      <endpoint address="net.tcp://machineName:8008/Calculator"
                binding="netTcpBinding"  
                bindingConfiguration="NetTcpBinding_ICalculator"  
                contract="ICalculator"  
                name="NetTcpBinding_ICalculator">
      </endpoint>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>另请参阅

- [安全性概述](security-overview.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
