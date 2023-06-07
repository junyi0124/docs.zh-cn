---
title: 排查 Windows Communication Foundation 教程入门
ms.date: 01/25/2019
ms.assetid: 69a21511-0871-4c41-9a53-93110e84d7fd
ms.openlocfilehash: 4d471372419996c5bc490c2d0fdd83927428a41e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281334"
---
# <a name="troubleshoot-the-get-started-with-windows-communication-foundation-tutorials"></a>排查 Windows Communication Foundation 教程入门

本文提供了有关在执行教程中的步骤时可能会遇到的最常见问题和错误的解决方案 [： Windows Communication Foundation 应用程序入门](getting-started-tutorial.md)。
  
## <a name="common-problems"></a>常见问题

**我在硬盘驱动器上找不到项目文件。**

 Visual Studio 将项目文件保存在 *C:\Users \\ &lt; 用户名 &gt; \source\repos* 中。  

**找不到 *Svcutil.exe* 生成的 *App.config* 文件。**

 在 Visual Studio 中，默认情况下，" **添加现有项** " 窗口仅显示具有以下扩展名的文件：

- .cs
- *.resx*
- *。设置*
- *.xsd*
- *.wsdl*

若要显示所有文件类型，请在 "**添加现有项**" 窗口的右下角的下拉列表中选择 "**所有文件" (\* \*) 。**  
  
## <a name="common-errors"></a>常见错误

### <a name="compile-the-service-application"></a>编译服务应用程序

**在 "GettingStartedHost" 中找不到 "Sub Main" 错误 BC30420。**

Visual Basic 应用程序的入口点不正确。 进行以下更改：

   1. 在 " **解决方案资源管理器** " 窗口中，选择 " **GettingStartedHost** " 文件夹，然后从快捷菜单中选择 " **属性** "。
    a. 在 " **GettingStartedHost** " 窗口中，对于 " **启动对象**"，从列表 **中选择特定应用程序)** 的 (或入口点。
    b. 在主菜单中，选择 "**文件**" "  >  **全部保存**"。

### <a name="run-the-service-application"></a>运行服务应用程序

**HTTP 无法注册 URL "http： \/ /+： 8000/GettingStarted/CalculatorService"。你的进程不具有访问此命名空间的权限。**

 若要进行适当的访问，请使用管理权限启动承载 Windows Communication Foundation (WCF) 服务的进程：

- 对于 visual studio：在 "**开始**" 菜单中选择 "visual studio" 程序，然后从快捷菜单中选择 "**更多**  >  **运行方式管理员**"。
- 对于控制台窗口：在 "**开始**" 菜单中选择 "**命令提示符**"，然后从快捷菜单中选择 "**更多**  >  **运行方式管理员**"。
- 对于 Windows 资源管理器：选择可执行文件，然后从快捷菜单中选择 " **以管理员身份运行** "。

### <a name="compile-the-client-application"></a>编译客户端应用程序

**"CalculatorClient" 不包含 "" 的定义 \<method name> ，并且找不到可 \<method name> 接受类型为 "CalculatorClient" 的第一个参数的扩展方法 "" (是否缺少 using 指令或程序集引用？ )**  

仅公开了用特性标记的那些方法 `ServiceOperationAttribute` 。 如果在 `ServiceOperationAttribute` 接口中省略方法中的属性 `ICalculator` ，则会在编译过程中收到此错误消息。  

**找不到类型或命名空间名称 "CalculatorClient" (是否缺少 using 指令或程序集引用？ )**

 如果你在生成具有 *Svcutil.exe* 工具的 *GeneratedProxy.cs* (或 *generatedProxy*) 文件时未将该文件添加到客户端项目，则会收到此错误。  

### <a name="run-the-client-application"></a>运行客户端应用程序

**未经处理的异常： System.servicemodel.endpointnotfoundexception：无法连接到 "http： \/ /localhost： 8000/GettingStarted/CalculatorService"。TCP 错误代码10061：无法建立连接，因为目标计算机主动拒绝该连接。**

如果在不首先启动服务的情况下运行客户端应用程序，则会发生此错误。 首先，运行主机应用程序以启动该服务，然后运行客户端应用程序。

### <a name="use-the-svcutilexe-tool"></a>使用 Svcutil.exe 工具

**"Svcutil.exe" 未被识别为内部或外部命令、可使用的程序或批处理文件。**

 *Svcutil.exe* 必须在系统路径中。 最简单的解决方案是使用 Visual Studio 命令提示符。 从“开始”菜单中选择“Visual Studio \<version>”目录，然后选择“VS \<version> 开发人员命令提示”  。 对于作为 Visual Studio 的一部分提供的所有工具，此命令提示符将系统路径设置为正确的位置。  
  
### <a name="run-the-service-and-client-applications"></a>运行服务和客户端应用程序

**System.servicemodel.security.securitynegotiationexception： \/ 对于目标 "http： \/ /localhost： 8000/GettingStarted/CalculatorService"，具有 "http：/localhost： 8000/GettingStarted/CalculatorService" 的 SOAP 安全协商失败**  

此错误发生在未建立网络连接的已加入域的计算机上。 将计算机连接到网络，或关闭服务和客户端的安全性。

关闭安全：

- 对于服务，请将创建的代码替换为 `WSHttpBinding` 以下代码：  
  
    ```csharp
    // Step 3: Add a service endpoint.
    selfhost.AddServiceEndpoint(typeof(ICalculator), new WSHttpBinding(SecurityMode.None), "CalculatorService");  
    ```

- 对于客户端，在配置文件中，更新 **\<security>** 元素下的元素，如下所示 **\<binding>** ：  
  
    ```xml
    <binding name="WSHttpBinding_ICalculator">
      <security mode="None" />
    </binding
    ```  

## <a name="see-also"></a>另请参阅  

 [WCF 应用程序入门](getting-started-tutorial.md)  
 [WCF 疑难解答快速入门](wcf-troubleshooting-quickstart.md)  
 [安装问题疑难解答](troubleshooting-setup-issues.md)
