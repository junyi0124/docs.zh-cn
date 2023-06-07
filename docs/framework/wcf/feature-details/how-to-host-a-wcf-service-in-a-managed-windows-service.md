---
title: 如何：在托管 Windows 服务中承载 WCF 服务
description: 了解如何创建由 Windows 服务承载的 WCF 服务。 此宿主选项在 Windows 的所有版本中都是可用的。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8e37363b-4dad-4fb6-907f-73c30fac1d9a
ms.openlocfilehash: 21d3dcb05e48154eb3f9f10d8308dc14bd046ae1
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90546336"
---
# <a name="how-to-host-a-wcf-service-in-a-managed-windows-service"></a>如何：在托管 Windows 服务中承载 WCF 服务

本主题概述了创建由 Windows 服务托管的 Windows Communication Foundation (WCF) 服务所需的基本步骤。 此方案由托管 Windows 服务宿主选项启用，该选项是一个长时间运行的 WCF 服务，该服务在未激活消息的安全环境中 Internet Information Services (IIS) 上托管。 服务的生存期改由操作系统控制。 此宿主选项在 Windows 的所有版本中都是可用的。

可以使用 Microsoft 管理控制台 (MMC) 中的 Microsoft.ManagementConsole.SnapIn 管理 Windows 服务，并且可以将其配置为在系统启动时自动启动。 此宿主选项包括注册作为托管 Windows 服务承载 WCF 服务的应用程序域 (AppDomain) ，以便服务的进程生存期由 Windows 服务的服务控制管理器 (SCM) 控制。

服务代码包括服务协定的服务实现、Windows 服务类和安装程序类。 服务实现类 `CalculatorService` 是 WCF 服务。 `CalculatorWindowsService` 是 Windows 服务。 要符合 Windows 服务的要求，该类继承自 `ServiceBase` 并实现 `OnStart` 和 `OnStop` 方法。 在 `OnStart` 中，将为 <xref:System.ServiceModel.ServiceHost> 类型创建 `CalculatorService` 并打开它。 在 `OnStop` 中，停止并释放服务。 主机还负责提供服务主机基址，该基址已在应用程序设置中进行设置。 安装程序类继承自 <xref:System.Configuration.Install.Installer>，允许程序通过 Installutil.exe 工具安装为 Windows 服务。

## <a name="construct-the-service-and-provide-the-hosting-code"></a>构造服务并提供主机代码

1. 创建名为 "**服务**" 的新 Visual Studio**控制台应用程序**项目。

2. 将 Program.cs 重命名为 Service.cs。

3. 将命名空间更改为 `Microsoft.ServiceModel.Samples` 。

4. 添加对下列程序集的引用：

    - System.ServiceModel.dll

    - System.ServiceProcess.dll

    - System.Configuration.Install.dll

5. 将下面的 using 语句添加到 Service.cs。

     [!code-csharp[c_HowTo_HostInNTService#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#0)]
     [!code-vb[c_HowTo_HostInNTService#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#0)]

6. 定义 `ICalculator` 服务协定，如下面的代码所示。

     [!code-csharp[c_HowTo_HostInNTService#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#1)]
     [!code-vb[c_HowTo_HostInNTService#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#1)]

7. 在称为 `CalculatorService` 的类中实现服务协定，如下面的代码所示。

     [!code-csharp[c_HowTo_HostInNTService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#2)]
     [!code-vb[c_HowTo_HostInNTService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#2)]

8. 创建从 `CalculatorWindowsService` 类继承的称为 <xref:System.ServiceProcess.ServiceBase> 的新类。 添加称为 `serviceHost` 的局部变量以引用 <xref:System.ServiceModel.ServiceHost> 实例。 定义调用 `Main` 的 `ServiceBase.Run(new CalculatorWindowsService)` 方法

     [!code-csharp[c_HowTo_HostInNTService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#3)]
     [!code-vb[c_HowTo_HostInNTService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#3)]

9. 通过创建并打开新 <xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> 实例重写 <xref:System.ServiceModel.ServiceHost> 方法，如下面的代码所示。

     [!code-csharp[c_HowTo_HostInNTService#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#4)]
     [!code-vb[c_HowTo_HostInNTService#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#4)]

10. 通过关闭 <xref:System.ServiceProcess.ServiceBase.OnStop%2A> 重写 <xref:System.ServiceModel.ServiceHost> 方法，如下面的代码所示。

     [!code-csharp[c_HowTo_HostInNTService#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#5)]
     [!code-vb[c_HowTo_HostInNTService#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#5)]

11. 创建称为 `ProjectInstaller` 的新类，该类继承自 <xref:System.Configuration.Install.Installer>，且其 <xref:System.ComponentModel.RunInstallerAttribute> 设置为 `true`。 这允许 Windows 服务由 Installutil.exe 工具安装。

     [!code-csharp[c_HowTo_HostInNTService#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#6)]
     [!code-vb[c_HowTo_HostInNTService#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#6)]

12. 移除当您创建项目时生成的 `Service` 类。

13. 将应用程序配置文件添加到项目中。 将文件内容替换为下面的配置 XML。

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <system.serviceModel>    <services>
          <!-- This section is optional with the new configuration model
               introduced in .NET Framework 4. -->
          <service name="Microsoft.ServiceModel.Samples.CalculatorService"
                   behaviorConfiguration="CalculatorServiceBehavior">
            <host>
              <baseAddresses>
                <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>
              </baseAddresses>
            </host>
            <!-- this endpoint is exposed at the base address provided by host: http://localhost:8000/ServiceModelSamples/service  -->
            <endpoint address=""
                      binding="wsHttpBinding"
                      contract="Microsoft.ServiceModel.Samples.ICalculator" />
            <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->
            <endpoint address="mex"
                      binding="mexHttpBinding"
                      contract="IMetadataExchange" />
          </service>
        </services>
        <behaviors>
          <serviceBehaviors>
            <behavior name="CalculatorServiceBehavior">
              <serviceMetadata httpGetEnabled="true"/>
              <serviceDebug includeExceptionDetailInFaults="False"/>
            </behavior>
          </serviceBehaviors>
        </behaviors>
      </system.serviceModel>
    </configuration>
    ```

     右键单击 **解决方案资源管理器** 中的 App.config 文件，然后选择 " **属性**"。 在 " **复制到输出目录** " 下，选择 " **如果较新则复制**"

     此示例显式指定配置文件中的终结点。 如果您不希望向服务添加任何终结点，则运行时为您添加默认终结点。 在此示例中，由于服务的 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 设置为 `true`，因此服务还启用了发布元数据。 有关默认终结点、绑定和行为的详细信息，请参阅[简化配置](../simplified-configuration.md)和 [WCF 服务的简化配置](../samples/simplified-configuration-for-wcf-services.md)。

## <a name="install-and-run-the-service"></a>安装并运行服务

1. 生成解决方案以创建 `Service.exe` 可执行文件。

2. 打开 Visual Studio 开发人员命令提示，并导航到项目目录。 在命令提示符处键入 `installutil bin\service.exe` 来安装 Windows 服务。

     在命令提示符处键入 `services.msc` 以访问服务控制管理器 (SCM)。 Windows 服务应作为“WCFWindowsServiceSample”出现在服务中。 如果 Windows 服务正在运行，WCF 服务只能响应客户端。 若要启动该服务，请在 SCM 中右键单击该服务并选择 "启动"，或在命令提示符下键入 **net Start WCFWindowsServiceSample** 。

3. 如果对服务进行更改，则必须首先停止并卸载服务。 若要停止该服务，请在 SCM 中右键单击该服务并选择 "停止"，或在命令提示符下 **键入 net Stop WCFWindowsServiceSample** 。 请注意，如果停止 Windows 服务然后运行客户端，则在客户端尝试访问该服务时，会发生 <xref:System.ServiceModel.EndpointNotFoundException> 异常。 若要卸载 Windows 服务，请在命令提示符下键入 **installutil.exe/u bin\service.exe** 。

## <a name="example"></a>示例

下面是本主题使用的完整代码列表：

[!code-csharp[c_HowTo_HostInNTService#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinntservice/cs/service.cs#8)]
[!code-vb[c_HowTo_HostInNTService#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_hostinntservice/vb/service.vb#8)]

与“自承载”选项一样，Windows 服务主机环境要求写入一些宿主代码作为应用程序的一部分。 该服务作为控制台应用程序实现，且包含其自己的主机代码。 在其他主机环境（如 Internet 信息服务 (IIS) 中的 Windows 进程激活服务 (WAS) 主机）中，开发人员没有必要编写主机代码。

## <a name="see-also"></a>请参阅

- [简化配置](../simplified-configuration.md)
- [在托管应用程序中承载](hosting-in-a-managed-application.md)
- [承载服务](../hosting-services.md)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
