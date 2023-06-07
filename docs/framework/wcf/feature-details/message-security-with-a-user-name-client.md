---
title: 用户名客户端的消息安全
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 36335cb9-76b8-4443-92c7-44f081eabb21
ms.openlocfilehash: 7168b393bde626c8c413cda3c7422e0eee4ce267
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96292865"
---
# <a name="message-security-with-a-user-name-client"></a>用户名客户端的消息安全

下图显示了使用消息级安全性来保护 Windows Communication Foundation (WCF) 服务和客户端。 服务使用 X.509 证书进行身份验证。 客户端使用用户名和密码进行身份验证。  
  
 有关示例应用程序，请参阅 [消息安全用户名](../samples/message-security-user-name.md)。  
  
 ![使用用户名身份验证的消息安全](media/1fb10a61-7e1d-42f5-b1af-195bfee5b3c6.gif "1fb10a61-7e1d-42f5-b1af-195bfee5b3c6")  
  
|特征|说明|  
|--------------------|-----------------|  
|安全模式|消息|  
|互操作性|仅 Windows Communication Foundation (WCF) |  
|身份验证（服务器）|初始协商需要服务器身份验证|  
|身份验证（客户端）|用户名/密码|  
|完整性|是，使用共享安全上下文|  
|机密性|是，使用共享安全上下文|  
|Transport|HTTP|  
|绑定|<xref:System.ServiceModel.WSHttpBinding>|  
  
## <a name="service"></a>服务  

 下面的代码和配置应独立运行。 执行下列操作之一：  
  
- 使用代码（而不使用配置）创建独立服务。  
  
- 使用提供的配置创建服务，但不定义任何终结点。  
  
### <a name="code"></a>代码  

 下面的代码演示如何创建使用消息安全的服务终结点。  
  
 [!code-csharp[C_SecurityScenarios#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#9)]
 [!code-vb[C_SecurityScenarios#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#9)]  
  
### <a name="configuration"></a>Configuration  

 以下配置可代替代码使用：  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="ServiceCredentialsBehavior">  
          <serviceCredentials>  
            <serviceCertificate findValue="Contoso.com"
                                storeLocation="LocalMachine"  
                                storeName="My"
                                x509FindType="FindBySubjectName" />  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <services>  
      <service behaviorConfiguration="ServiceCredentialsBehavior"  
               name="ServiceModel.Calculator">  
        <endpoint address="http://localhost/Calculator"  
                  binding="wsHttpBinding"  
                  bindingConfiguration="MessageAndUserName"  
                  name="SecuredByTransportEndpoint"  
                  contract="ServiceModel.ICalculator" />  
      </service>  
    </services>  
    <bindings>  
      <wsHttpBinding>  
        <binding name="MessageAndUserName">  
          <security mode="Message">
            <message clientCredentialType="UserName" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <client />  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="client"></a>客户端  
  
### <a name="code"></a>代码  

 下面的代码创建客户端。 绑定设置为消息模式安全，客户端凭据类型设置为 `UserName`。 用户名和密码只能使用代码指定（不可配置）。 返回用户名和密码的代码未在此处显示，因为这必须在应用程序级完成。 例如，使用 Windows 窗体对话框查询用户以获得这些数据。  
  
 [!code-csharp[C_SecurityScenarios#16](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_securityscenarios/cs/source.cs#16)]
 [!code-vb[C_SecurityScenarios#16](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securityscenarios/vb/source.vb#16)]  
  
### <a name="configuration"></a>Configuration  

 下面的代码将配置客户端。 绑定设置为消息模式安全，客户端凭据类型设置为 `UserName`。 用户名和密码只能使用代码指定（不可配置）。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <bindings>  
      <wsHttpBinding>  
        <binding name="WSHttpBinding_ICalculator" >  
          <security mode="Message">  
            <message clientCredentialType="UserName" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <client>  
      <endpoint address="http://machineName/Calculator"
                binding="wsHttpBinding"  
                bindingConfiguration="WSHttpBinding_ICalculator"
                contract="ICalculator"  
                name="WSHttpBinding_ICalculator">  
        <identity>  
          <dns value ="Contoso.com" />  
        </identity>  
      </endpoint>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>另请参阅

- [安全性概述](security-overview.md)
- [用户名消息安全](../samples/message-security-user-name.md)
- [服务标识和身份验证](service-identity-and-authentication.md)
- [\<identity>](../../configure-apps/file-schema/wcf/identity.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
