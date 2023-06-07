---
title: WMI 提供程序
ms.date: 03/30/2017
ms.assetid: 462f0db3-f4a4-4a4b-ac26-41fc25c670a4
ms.openlocfilehash: 9d654527c6897e071f914d4015ba9a225974b0f1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263764"
---
# <a name="wmi-provider"></a>WMI 提供程序

此示例演示如何使用内置于 WCF 中的 Windows Management Instrumentation (WMI) 提供程序，在运行时从 Windows Communication Foundation (WCF) 服务收集数据。 另外，此示例还演示如何向服务添加用户定义的 WMI 对象。 该示例将激活 [入门](getting-started-sample.md) 的 WMI 提供程序，并演示如何 `ICalculator` 在运行时从服务中收集数据。  
  
 WMI 是 Microsoft 基于 Web 的企业管理 (WBEM) 标准的实现。 有关 WMI SDK 的详细信息，请参阅 [Windows Management Instrumentation](/windows/desktop/WmiSdk/wmi-start-page)。 WBEM 是有关应用程序如何向外部管理工具公开管理规范的行业标准。  
  
 WCF 实现一个 WMI 提供程序，该提供程序在运行时通过与 WBEM 兼容的接口公开检测。 管理工具可以在运行时通过接口连接至服务。 WCF 公开服务的属性，如地址、绑定、行为和侦听器。  
  
 在应用程序的配置文件中激活内置 WMI 提供程序。 此操作通过 `wmiProviderEnabled` 部分中的属性完成 [\<diagnostics>](../../configure-apps/file-schema/wcf/diagnostics.md) [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) ，如下面的示例配置所示：  
  
```xml  
<system.serviceModel>  
    ...  
    <diagnostics wmiProviderEnabled="true" />  
    ...  
</system.serviceModel>  
```  
  
 此配置项公开 WMI 接口。 现在，您可以通过此接口连接管理应用程序并访问应用程序的管理规范。  
  
## <a name="custom-wmi-object"></a>自定义 WMI 对象  

 将 WMI 对象添加到服务，可以显示用户定义的信息以及内置的 WMI 提供程序信息。 这可以通过使用 Installutil.exe 应用程序将服务方案发布到 WMI 来实现。 本主题末尾的安装说明介绍了如何实现上述操作的说明以及更详细的信息。  
  
## <a name="accessing-wmi-information"></a>访问 WMI 信息  

可以采用多种不同方式访问 WMI 数据。 Microsoft 为脚本、Visual Basic 应用程序、c + + 应用程序和 .NET Framework 提供 WMI Api。 有关详细信息，请参阅 [使用 WMI](/windows/desktop/wmisdk/using-wmi)。
  
 此示例使用两个 Java 脚本：一个脚本用于枚举在计算机上运行的服务及其某些属性，另一个脚本用于查看用户定义的 WMI 数据。 该脚本打开与 WMI 提供程序的连接、分析数据并显示所收集的数据。  
  
 启动示例以创建 WCF 服务的运行实例。 在该服务运行的同时，在命令提示符处使用以下命令运行每个 Java 脚本：  
  
```console  
cscript EnumerateServices.js  
```  
  
 该脚本访问服务中包含的规范，并生成下面的输出：  
  
```console  
Microsoft (R) Windows Script Host Version 5.6  
Copyright © Microsoft Corporation 1996-2001. All rights reserved.  
  
1 service(s) found.  
|-PID:           5776  
|-DistinguishedName:  CalculatorService@http://localhost/ServiceModelSamples/service.svc  
|-Endpoints:     1 endpoints  
  |-CalculatorService.ICalculator@http://localhost/ServiceModelSamples/service.svc  
    |-Address:                        http://localhost/ServiceModelSamples/service.svc  
    |-CounterInstanceName:  
    |-AddressHeaders:                 0  
    |-ContractType:                   Contract.Name='ICalculator'  
    |-BindingElements:                4 bindings  
      |-BindingElements[0]  
        |-Type:                       TransactionFlowBindingElement  
      |-BindingElements[1]  
        |-Type:                       SymmetricSecurityBindingElement  
      |-BindingElements[2]  
        |-Type:                       TextMessageEncodingBindingElement  
        |-MaxReadPoolSize:            64  
        |-MaxWritePoolSize:           16  
      |-BindingElements[3]  
        |-Type:                       HttpTransportBindingElement  
        |-ManualAddressing:           false  
        |-MaxBufferSize:              65536  
        |-AllowCookies:               false  
        |-AuthenticationScheme:       Anonymous  
        |-BypassProxyOnLocal:         false  
        |-HostNameComparisonMode:     StrongWildcard  
        |-ProxyAddress:               null  
        |-ProxyAuthenticationScheme:  Anonymous  
        |-Realm:  
        |-TransferMode:               Buffered  
        |-UseDefaultWebProxy:         true  
|-Behaviors:     5 behaviors  
      |-Behavior[0]  
      |-Type:                       ServiceBehaviorAttribute  
        |-AddressFilterMode:               Exact  
        |-AutomaticSessionShutdown:        true  
        |-ConcurrencyMode:                 Single  
        |-IncludeExceptionDetailInFaults:  false  
        |-InstanceContextMode:             PerSession  
        |-TransactionIsolationLevel:       Unspecified  
        |-TransactionTimeout:              null  
        |-ValidateMustUnderstand:          true  
      |-Behavior[1]  
      |-Type:                       AspNetCompatibilityRequirementsAttribute  
      |-Behavior[2]  
      |-Type:                       ServiceDebugBehavior  
      |-Behavior[3]  
      |-Type:                       ServiceAuthorizationBehavior  
      |-Behavior[4]  
      |-Type:                       Behavior  
```  
  
 接下来，运行第二个 Java 脚本可以显示用户定义的 WMI 数据：  
  
```console  
cscript EnumerateCustomObjects.js  
```  
  
 该脚本访问服务中包含的用户定义的规范，并生成下面的输出：  
  
```console
1 WMIObject(s) found.  
|-PID:           30285bfd-9d66-4c4e-9be2-310499c5cef5  
|-InstanceId:    3839  
|-WMIInfo:       User Defined WMI Information.  
```  
  
 该输出显示计算机上正在运行一个单一服务。 该服务公开一个实现 `ICalculator` 协定的终结点。 该终结点实现的行为和绑定的设置列作消息堆栈的各个元素的总和。  
  
 WMI 并不局限于公开 WCF 基础结构的管理规范。 该应用程序可以通过相同机制公开它自己的特定域数据项。 WMI 是用于检查和控制 Web 服务的统一机制。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 通过在宿主目录中的 service.dll 文件上运行 InstallUtil.exe（InstallUtil.exe 的默认位置是“%WINDIR%\Microsoft.NET\Framework\v4.0.30319”），可以将服务方案发布到 WMI。 只有在对 service.dll 文件进行了更改的情况下才需要执行此步骤。
  
4. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
    > [!NOTE]
    > 如果在安装 ASP.NET 之后安装了 WCF，则可能需要运行 "% WINDIR% \Net\Framework\v3.0\Windows 通信 Foundation\servicemodelreg.exe "-r-x，为 ASPNET 帐户授予发布 WMI 对象的权限。  
  
5. 使用下面的命令可以查看通过 WMI 显示的示例：`cscript EnumerateServices.js` 或 `cscript EnumerateCustomObjects.js`。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\WMIProvider`  
  
## <a name="see-also"></a>另请参阅

- [AppFabric 监视示例](/previous-versions/appfabric/ff383407(v=azure.10))
