---
title: 如何：配置 COM+ 服务设置
ms.date: 03/30/2017
helpviewer_keywords:
- COM+ [WCF], configuring service settings
ms.assetid: f42a55a8-3af8-4394-9fdd-bf12a93780eb
ms.openlocfilehash: b75f5c2a64b7184959e929439893b33193aa7bae
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257465"
---
# <a name="how-to-configure-com-service-settings"></a>如何：配置 COM+ 服务设置

使用 COM+ 服务配置工具添加或移除应用程序接口时，应用程序配置文件中的 Web 服务配置将更新。 在 COM + 托管模式下，Application.config 文件放置在应用程序根目录中 (%Programfiles%\complus applications\ 应用程序 \\ {appid} 是默认的) 。 无论在哪种 Web 宿主模式中，Web.config 文件均放置在指定的 vroot 目录中。  
  
> [!NOTE]
> 应使用消息签名来防止客户端和服务器之间的消息被篡改。 另外，还应该使用消息或传输层加密来防止客户端和服务器之间的消息发生信息泄漏。 与 Windows Communication Foundation (WCF) 服务一样，您应该使用限制来限制并发调用、连接、实例和挂起的操作的数量。 这有助于防止过度消耗资源。 您可以通过设置服务配置文件来指定遏制行为。  
  
## <a name="example"></a>示例  

 假设有一个实现以下接口的组件：  
  
```csharp
[Guid("C551FBA9-E3AA-4272-8C2A-84BD8D290AC7")]  
public interface IFinances  
{  
    string Debit(string accountNo, double amount);  
    string Credit(string accountNo, double amount);  
}  
```  
  
 如果该组件作为 Web 服务公开，则公开的且客户端应遵循的相应服务协定如下：  
  
```csharp
[ServiceContract(Session = true,  
Namespace = "http://tempuri.org/C551FBA9-E3AA-4272-8C2A-84BD8D290AC7",  
Name = "IFinances")]  
public interface IFinancesContract : IDisposable  
{  
    [OperationContract]  
    string Debit(string accountNo, double amount);  
    [OperationContract]  
    string Credit(string accountNo, double amount);  
}  
```  
  
> [!NOTE]
> IID 是该协定的初始命名空间的一部分。  
  
 使用此服务的客户端应用程序需要遵循此协定，并使用一个与应用程序配置中指定的绑定相兼容的绑定。  
  
 下面的代码示例演示了默认的配置文件。 作为 Windows Communication Foundation (WCF) Web 服务，这符合标准的服务模型配置架构，可采用与其他 WCF 服务配置文件相同的方式进行编辑。  
  
 典型的修改包括：  
  
- 将终结点地址从默认的 ApplicationName/ComponentName/InterfaceName 形式更改为更可用的形式。  
  
- 将服务的命名空间从默认 `http://tempuri.org/InterfaceID` 窗体修改为更相关的窗体。  
  
- 将终结点更改为使用其他传输绑定。  
  
     在 COM+ 宿主模式中，默认使用命名管道传输，但也可以改用无线传输（如 TCP）。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
    <system.serviceModel>  
        <bindings>  
            <netNamedPipeBinding>  
                <binding name="comNonTransactionalBinding" />  
                <binding name="comTransactionalBinding" transactionFlow="true" />  
            </netNamedPipeBinding>  
        </bindings>  
        <comContracts>  
            <comContract contract="{C551FBA9-E3AA-4272-8C2A-84BD8D290AC7}"  
                name="IFinances" namespace="http://tempuri.org/C551FBA9-E3AA-4272-8C2A-84BD8D290AC7"  
                requiresSession="true">  
                <exposedMethods>  
                    <add exposedMethod="Debit" />  
                    <add exposedMethod="Credit" />  
                </exposedMethods>  
            </comContract>  
        </comContracts>  
        <services>  
            <service name="{DCDB24CC-0B19-4534-95CD-FBBFF4D67DD9},{C942B840-AD54-4A44-B5F7-928130980AB9}">  
                <endpoint address="IFinances" binding="netNamedPipeBinding" bindingConfiguration="comNonTransactionalBinding"  
                    contract="{C551FBA9-E3AA-4272-8C2A-84BD8D290AC7}" />  
                <host>  
                    <baseAddresses>  
                        <add baseAddress="net.pipe://localhost/ServiceModelDocSampleApp/ServiceModelDocSample.esFinance" />  
                    </baseAddresses>  
                </host>  
            </service>  
        </services>  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>另请参阅

- [与 COM + 应用程序集成](integrating-with-com-plus-applications.md)
