---
title: UDP 激活
ms.date: 03/30/2017
ms.assetid: 4b0ccd10-0dfb-4603-93f9-f0857c581cb7
ms.openlocfilehash: 74ee3e1d53b9ba060820f69c201bfdccb6308d47
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294997"
---
# <a name="udp-activation"></a>UDP 激活

此示例基于 [Transport： UDP](transport-udp.md) 示例。 它扩展了 [传输： UDP](transport-udp.md) 示例，以支持使用) 的 Windows 进程激活服务 (处理激活。  
  
 该示例主要由三个部分组成：  
  
- UDP 协议激活器，代表要激活的应用程序接收 UDP 消息的独立进程。  
  
- 使用 UDP 自定义传输发送消息的客户端。  
  
- 通过 UDP 自定义传输接收消息的服务（承载于 WAS 激活的工作进程中）。  
  
## <a name="udp-protocol-activator"></a>UDP 协议激活器  

 UDP 协议激活器是 WCF 客户端和 WCF 服务之间的桥梁。 该激活器通过 UDP 协议在传输层提供数据通信。 它主要有两个功能：  
  
- WAS 侦听器适配器 (LA)，它与 WAS 协作激活进程以响应传入消息。  
  
- UDP 协议侦听器，它代表要激活的应用程序接受 UDP 消息。  
  
 激活器必须作为独立的程序在服务器计算机上运行。 通常，WAS 侦听器适配器（如 NetTcpActivator 和 NetPipeActivator）在长时间运行的 Windows 服务中实现。 但是，为了简单明确起见，此示例将协议激活器作为独立的应用程序实现。  
  
### <a name="was-listener-adapter"></a>WAS 侦听器适配器  

 UDP 的 WAS 侦听器适配器是在 `UdpListenerAdapter` 类中实现的。 该适配器是与 WAS 交互以针对 UDP 协议执行应用程序激活的模块。 这一过程是通过调用下列 Webhost API 实现的：  
  
- `WebhostRegisterProtocol`  
  
- `WebhostUnregisterProtocol`  
  
- `WebhostOpenListenerChannelInstance`  
  
- `WebhostCloseAllListenerChannelInstances`  
  
 最初调用 `WebhostRegisterProtocol` 之后，侦听器适配器将为 applicationHost.config（位于 %windir%\system32\inetsrv）中注册的所有应用程序接收来自 WAS 的回调 `ApplicationCreated`。 在此示例中，我们仅在启用 UDP 协议（协议 ID 为“net.udp”时）的情况下处理应用程序。 如果其他实现对应用程序的动态配置更改（例如，应用程序从禁用转换为启用）作出响应，这些实现可能会以不同的方式处理上述情况。  
  
 当收到回调 `ConfigManagerInitializationCompleted` 时，则表示 WAS 已经完成协议初始化的所有通知。 此时，侦听器适配器已准备处理激活请求。  
  
 当首次传入针对某个应用程序的新请求时，侦听器适配器会将 `WebhostOpenListenerChannelInstance` 调入 WAS，从而启动工作进程（如果尚未启动）。 然后加载协议处理程序，并可以启动侦听器适配器与虚拟应用程序之间的通信。  
  
 侦听器适配器在 "<>" 部分的 "% SystemRoot% \System32\inetsrv\ApplicationHost.config 中注册，如下所示 `listenerAdapters` ：  
  
```xml  
<add name="net.udp" identity="S-1-5-21-2127521184-1604012920-1887927527-387045" />  
```  
  
### <a name="protocol-listener"></a>协议侦听器  

 UDP 协议侦听器是协议激活器内部的模块，它代表虚拟应用程序在 UDP 终结点进行侦听。 它在 `UdpSocketListener` 类中实现。 终结点表示为 `IPEndpoint`，其端口号从站点协议的绑定中提取。  
  
### <a name="control-service"></a>控制服务  

 在此示例中，我们使用 WCF 在激活器和 WAS 工作进程之间进行通信。 驻留在激活器中的服务称为“控制服务”。  
  
## <a name="protocol-handlers"></a>协议处理程序  

 侦听器适配器调用 `WebhostOpenListenerChannelInstance` 之后，WAS 进程管理器将启动工作进程（如果尚未启动）。 工作进程内部的应用程序管理器随后使用该 `ListenerChannelId` 的请求加载 UDP 进程协议处理程序 (PPH)。 中的 PPH 将调用 `IAdphManager` 。`StartAppDomainProtocolListenerChannel` 若要启动 UDP AppDomain 协议处理程序 (ADPH) 。  
  
## <a name="hostedudptransportconfiguration"></a>HostedUDPTransportConfiguration  

 该信息按如下方式在 Web.config 中注册：  
  
```xml  
<serviceHostingEnvironment>  
<add name="net.udp" transportConfigurationType="Microsoft.ServiceModel.Samples.Hosting.HostedUdpTransportConfiguration, UdpActivation, Version=1.0.0.0, Culture=neutral, PublicKeyToken=6fa904d2da1848d6" />  
</serviceHostingEnvironment>  
```  
  
## <a name="special-setup-for-this-sample"></a>此示例的特殊设置  

 此示例只能在 Windows Vista、Windows Server 2008 或 Windows 7 上生成和运行。 若要运行此示例，必须首先正确设置所有组件。 使用下列步骤安装该示例。  
  
#### <a name="to-set-up-this-sample"></a>设置此示例  
  
1. 使用以下命令安装 ASP.NET 4.0。  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. 在 Windows Vista 上生成项目。 编译后，该项目还将在后期生成阶段执行下列操作：  
  
    - 将 UDP 绑定安装到“默认网站”站点。  
  
    - 创建虚拟应用程序“ServiceModelSamples”，指向物理路径“%SystemDrive%\inetpub\wwwroot\servicemodelsamples”。  
  
    - 这还为该虚拟应用程序启用“net.udp”协议。  
  
3. 启动用户接口应用程序“WasNetActivator.exe”。 单击 " **设置** " 选项卡，选中以下复选框，然后单击 " **安装** " 以安装它们：  
  
    - UDP 侦听器适配器  
  
    - UDP 协议处理程序  
  
4. 单击用户界面应用程序 "WasNetActivator.exe" 的 " **激活** " 选项卡。 单击 " **启动** " 按钮以启动侦听器适配器。 现在即可运行程序。  
  
    > [!NOTE]
    > 完成此示例后，必须运行 Cleanup.bat，从“默认网站”中移除 net.udp 绑定。  
  
## <a name="sample-usage"></a>示例用法  

 编译后，生成四个不同的二进制文件：  
  
- Client.exe：客户端代码。 App.config 编译到客户端配置文件 Client.exe.config 中。  
  
- UDPActivation.dll：包含所有主要 UDP 实现的库。  
  
- Service.dll：服务代码。 此代码被复制到虚拟应用程序 ServiceModelSamples 的 \bin 目录。 服务文件为 node.js，并 Web.config 配置文件。编译完成后，会将其复制到以下位置：%Systemdrive%\inetpub\wwwroot\servicemodelsamples。  
  
- WasNetActivator：UDP 激活器程序。  
  
- 确保所有的必需部分均已正确安装。 下列步骤说明如何运行该示例：  
  
1. 确保已启动下面的 Windows 服务：  
  
    - Windows 进程激活服务 (WAS)。  
  
    - Internet Information Services (IIS)：W3SVC。  
  
2. 然后启动激活器 WasNetActivator.exe。 在 " **激活** " 选项卡下，在下拉列表中选择唯一的协议 " **UDP**"。 单击 " **启动** " 按钮以启动激活器。  
  
3. 启动激活器后，可以通过在命令窗口中运行 Client.exe 来运行客户端代码。 下面是示例输出：  
  
    ```console  
    Testing Udp Activation.  
    Start the status service.  
    Sending UDP datagrams.  
    Type a word that you want to say to the server: Hello, world!  
        Sending datagram: Hello, world![0]  
        Sending datagram: Hello, world![1]  
        Sending datagram: Hello, world![2]  
        Sending datagram: Hello, world![3]  
        Sending datagram: Hello, world![4]  
    Calling UDP duplex contract (ICalculatorContract).  
        0 + 0 = 0  
        1 + 2 = 3  
        2 + 4 = 6  
        3 + 6 = 9  
        4 + 8 = 12  
    Getting status and dump server traces:  
        Operation 'Hello' is called: Hello, world![0]  
        Operation 'Hello' is called: Hello, world![1]  
        Operation 'Hello' is called: Hello, world![2]  
        Operation 'Hello' is called: Hello, world![3]  
        Operation 'Hello' is called: Hello, world![4]  
        Operation 'Add' is called: 0 + 0  
        Operation 'Add' is called: 1 + 2  
        Operation 'Add' is called: 2 + 4  
        Operation 'Add' is called: 3 + 6  
        Operation 'Add' is called: 4 + 8  
    Press <ENTER> to complete test.  
    ```  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Transport\UdpActivation`  
