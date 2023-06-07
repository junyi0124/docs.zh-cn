---
title: 配置通道工厂
ms.date: 03/30/2017
ms.assetid: 3b749493-bd8a-4ccb-893e-5948901a1486
ms.openlocfilehash: bebdd7e5913fd3bbc1ab88d32e6612b9bc951852
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96241143"
---
# <a name="configuration-channel-factory"></a>配置通道工厂

此示例介绍 <xref:System.ServiceModel.Configuration.ConfigurationChannelFactory%601> 的用法。 <xref:System.ServiceModel.Configuration.ConfigurationChannelFactory%601>允许对 WCF 客户端配置进行集中管理。 这可能在应用程序域加载时间之后选择或更改配置的方案中也非常有用。

## <a name="demonstrates"></a>演示

 <xref:System.ServiceModel.Configuration.ConfigurationChannelFactory%601>

## <a name="discussion"></a>讨论 (Discussion)

 此示例演示如何使用 <xref:System.ServiceModel.Configuration.ConfigurationChannelFactory%601> 将特定配置文件添加到客户端应用程序，而不必使用默认的应用程序文件。

 此示例由两个项目组成。 第一个项目是一个简单服务，运行该服务可答复来自客户端的消息。 第二个项目是一个客户端应用程序，它使用 <xref:System.ServiceModel.Configuration.ConfigurationChannelFactory%601> 为 Test.config 配置文件生成两个 <xref:System.Configuration.ExeConfigurationFileMap> 对象，然后使用这两个对象与服务通信。 两个客户端都使用 Test.config 中指定的配置与服务通信。

 以下代码可将自定义配置文件添加到客户端应用程序。

```csharp
ExeConfigurationFileMap fileMap = new ExeConfigurationFileMap();
fileMap.ExeConfigFilename = "Test.config";
Configuration newConfiguration = ConfigurationManager.OpenMappedExeConfiguration(fileMap, ConfigurationUserLevel.None);

ConfigurationChannelFactory<ICalculatorChannel> factory1 = new ConfigurationChannelFactory<ICalculatorChannel>("endpoint1", newConfiguration, new EndpointAddress("http://localhost:8000/servicemodelsamples/service"));
ICalculatorChannel client1 = factory1.CreateChannel();
```

#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 以管理员权限打开 Visual Studio 2012。

2. 右键单击 ConfigurationChannelFactory 解决方案 (2 个项目) 然后选择 " **属性**"。

3. 在 " **通用属性**" 中，选择 " **启动项目**"，然后单击 " **多个启动项目**"。

4. 将 **服务** 项目移动到列表的开头，**操作为 "start"**，然后将该 **客户端** 项目与 "**启动" 操作** 一起移动到 **服务** 项目之后，以便在 **服务** 项目后执行 **客户端** 项目。

5. 单击 **"确定"**，然后按 F5 (或 CTRL + F5) 运行该示例。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\ConfigurationChannelFactory`
