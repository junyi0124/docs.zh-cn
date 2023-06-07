---
title: 使用性能计数器
ms.date: 03/30/2017
ms.assetid: 00a787af-1876-473c-a48d-f52b51e28a3f
ms.openlocfilehash: d3e6b9805bd0b9c5eea991fce4dde2035f8f5c1b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294906"
---
# <a name="using-performance-counters"></a>使用性能计数器

此示例演示如何访问 Windows Communication Foundation (WCF) 性能计数器以及如何创建用户定义的性能计数器。 此示例基于 [入门](getting-started-sample.md)。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 在此示例中，客户端调用 `ICalculator` 服务的四个方法。 客户端一直执行该操作，直到被用户中断。 该服务保持不变。  
  
 性能计数器在该服务的 Web.config 文件的诊断节中启用，如下面的示例配置所示。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <diagnostics performanceCounters="All" />
  </system.serviceModel>  
</configuration>  
```  
  
 还可以使用 [配置编辑器工具 ( # A0) ](../configuration-editor-tool-svcconfigeditor-exe.md)来完成此任务。  
  
 启用性能计数器后，将为服务启用整个 WCF 性能计数器套件。 .NET Framework 自动在三个级别维护性能数据：`ServiceModelService`、`ServiceModelEndpoint` 和 `ServiceModelOperation`。 其中每个级别都有“Calls”（调用）、“Calls per Second”（每秒调用次数）和“Security Calls Not Authorized”（未授权的安全调用次数）等性能计数器。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
### <a name="to-view-performance-data"></a>查看性能数据  
  
1. 通过依次单击 " **开始**"、" **运行 ...**"，输入 `perfmon` 并单击 **"确定** "，或从 "控制面板" 中选择 " **管理工具** "，然后双击 " **性能**"。  
  
    > [!NOTE]
    > 在示例代码运行之前无法添加计数器。  
  
2. 选择列出的性能计数器并按 Delete 键以移除它们。  
  
3. 通过右键单击 "关系图" 窗格并选择 " **添加计数器**" 来添加 WCF 计数器。 在 " **添加计数器** " 对话框中，选择 "性能对象" 下拉列表框中的 **ServiceModelOperation 3.0.0.0、ServiceModelEndpoint 3.0.0.0 或 ServiceModelService 3.0.0.0** 。 从列表中选择要查看的计数器。  
  
    > [!NOTE]
    > 如果未在计算机上运行任何 WCF 服务，则不会为服务提供 WCF 性能计数器。  
  
### <a name="to-use-the-configuration-editor-to-enable-counters"></a>使用配置编辑器来启用计数器  
  
1. 打开 SvcConfigEditor.exe 的一个实例。  
  
2. 在 "文件" 菜单上，单击 " **打开** "，然后单击 " **配置文件 ...**"。  
  
3. 导航到示例应用程序的服务文件夹并打开 Web.config 文件。  
  
4. 在配置树中单击 " **诊断** "。  
  
5. 切换 "**诊断**" 窗口中的 "**性能计数器**" 以显示 "全部"。  
  
6. 保存该配置文件并退出编辑器。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\PerfCounters`  
  
## <a name="see-also"></a>另请参阅

- [AppFabric 监视示例](/previous-versions/appfabric/ff383407(v=azure.10))
