---
title: 通过路由服务进行通信
ms.date: 03/30/2017
ms.assetid: 0f4b0d5b-6522-4ad5-9f3a-baa78316d7d1
ms.openlocfilehash: 3d91634d72481427f04e958f6dc2734829b6158b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253851"
---
# <a name="hello-world-with-the-routing-service"></a>通过路由服务进行通信

此示例演示了 Windows Communication Foundation (WCF) 路由服务。 路由服务是一个 WCF 组件，使您可以轻松地在应用程序中包含基于内容的路由器。 此示例改编了标准 WCF 计算器示例，以使用路由服务进行通信。 在此示例中，计算器客户端配置为将消息发送到由路由器公开的一个终结点。 路由服务配置为接受发送给它的所有消息，然后将这些消息转发至与计算器服务对应的终结点。 因此，从客户端发送的消息将由路由器接收，并重新路由到实际的计算器服务。 来自计算器服务的消息将发回到路由器，后者又将这些消息传回到计算器客户端。

### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2012，打开 HelloRoutingService。

2. 按 F5 或 Ctrl+Shift+B。

    > [!NOTE]
    > 如果按 F5，则计算器客户端将自动启动。 如果按 Ctrl+Shift+B（生成），则您必须自己启动以下应用程序。
    >
    > 1. 计算器客户端 (./CalculatorClient/bin/client.exe)
    > 2. 计算器服务 (./CalculatorService/bin/service.exe)
    > 3. 路由服务 (./RoutingService/bin/RoutingService.exe)

3. 按 Enter 启动客户端。

     你应该会看到以下输出：

    ```console
     Add(100,15.99) = 115.99

     Subtract(145,76.54) = 68.46

     Multiply(9,81.25) = 731.25

     Divide(22,7) = 3.14285714285714
    ```

## <a name="configurable-via-code-or-appconfig"></a>可通过代码或 App.Config 进行配置

 所提供的示例配置为使用 App.config 文件来定义路由器行为。 也可将 App.config 文件的名称更改为无法识别的其他名称，并将对 ConfigureRouterViaCode() 的方法调用取消注释。 以上任一方法都可产生相同的路由器行为。

### <a name="scenario"></a>场景

 此示例演示作为基本消息泵的路由器。 路由服务用作透明的代理节点，该节点配置为将消息直接传递到目标终结点的预配置集。

### <a name="real-world-scenario"></a>实际方案

 Contoso 希望提高其服务的命名、寻址、配置和安全方面的灵活性。 为此，他们将基本消息泵放在其服务的前面，作为面向公共的终结点。 这样，他们就可以将附加安全性放在其实际服务的前面，以便以后更轻松地实现扩展的解决方案或服务版本控制。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\RoutingServices\HelloRoutingService`  
  
## <a name="see-also"></a>另请参阅

- [AppFabric 承载和持久性示例](/previous-versions/appfabric/ff383418(v=azure.10))
