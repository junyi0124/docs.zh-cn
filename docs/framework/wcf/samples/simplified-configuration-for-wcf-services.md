---
title: WCF 服务的简化配置
description: 了解如何使用 WCF 实现和配置典型的服务和客户端。 服务通过使用配置文件中指定的终结点进行通信。
ms.date: 03/30/2017
ms.assetid: 1e39ec25-18a3-4fdc-b6a3-9dfafbd60112
ms.openlocfilehash: 087618d603ea1c7df75ab5383f6c95b781dca847
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96290018"
---
# <a name="simplified-configuration-for-wcf-services"></a>WCF 服务的简化配置

此示例演示如何使用 Windows Communication Foundation (WCF) 实现和配置典型的服务和客户端。 此示例是所有其他基本技术示例的基础。  
  
 此服务公开用于与服务进行通信的终结点，并使用 .NET Framework 4 中的简化配置。 在 .NET Framework 4 之前，通常在配置文件中定义终结点 ( # A0) ，如下面的示例配置代码所示。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<!-- Copyright ©) Microsoft Corporation.  All Rights Reserved. -->  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceMetadata httpGetEnabled="True"/>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <services>  
      <service name="Microsoft.Samples.GettingStarted.CalculatorService"  
               behaviorConfiguration="CalculatorServiceBehavior">  
        <endpoint address="" binding="basicHttpBinding" contract="ICalculator"/>  
        <endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange"/>  
      </service>  
    </services>  
  </system.serviceModel>  
</configuration>  
```  
  
 在 .NET Framework 4 中， `<service>` 元素是可选的。 当一个服务未定义任何终结点时，会将已实现的每个基址和协定的终结点添加到该服务。 基址将追加到协定名称后面以确定终结点，该地址方案将确定绑定。 以下代码示例演示一个简化的配置文件。 在配置后， `http://localhost/servicemodelsamples/service.svc` 同一台计算机上的客户端可以访问该服务。 若要使远程计算机上的客户端能够访问该服务，必须指定完全限定域名，而不是本地主机。 默认情况下，该服务不公开元数据。 因此，该服务将打开 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 行为。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<!-- Copyright © Microsoft Corporation.  All Rights Reserved. -->  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="">  
          <serviceMetadata httpGetEnabled="True"/>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="to-use-this-sample"></a>使用此示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
3. 按照以下步骤运行示例：  
  
    1. 右键单击 **服务** 项目，选择 " **设为启动项目**"，然后按 **Ctrl + F5**。  
  
    2. 等待确认服务已准备好并且正在运行的控制台输出。  
  
    3. 右键单击 **客户端** 项目，选择 " **设为启动项目**"，然后按 **Ctrl + F5**。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\ConfigSimplificationIn40`  
  
## <a name="see-also"></a>另请参阅

- [AppFabric 管理示例](/previous-versions/appfabric/ff383405(v=azure.10))
- [简化配置](../simplified-configuration.md)
