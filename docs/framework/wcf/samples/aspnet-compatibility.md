---
title: ASP.NET 兼容性
ms.date: 03/30/2017
ms.assetid: c8b51f1e-c096-4c42-ad99-0519887bbbc5
ms.openlocfilehash: b1d1a72b9ac3041a1547ac42a33eb7d3e1f87a63
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90553182"
---
# <a name="aspnet-compatibility"></a>ASP.NET 兼容性

此示例演示如何在 Windows Communication Foundation (WCF) 中启用 ASP.NET 兼容性模式。 在 ASP.NET 兼容模式下运行的服务完全参与 ASP.NET 应用程序管道，并可利用 ASP.NET 功能，如文件/URL 授权、会话状态和 <xref:System.Web.HttpContext> 类。 <xref:System.Web.HttpContext>类允许访问 cookie、会话和其他 ASP.NET 功能。 此模式要求绑定使用 HTTP 传输，且服务本身必须承载于 IIS 中。

在此示例中，客户端是一个控制台应用程序（一个可执行文件），服务由 Internet 信息服务 (IIS) 承载。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

此示例需要 .NET Framework 4 应用程序池才能运行。 若要创建新的应用程序池或修改默认应用程序池，请按照以下步骤操作。

1. 打开“控制面板”  打开 "**系统和安全**" 标题下的 "**管理工具**" 小程序。 ** (IIS) 管理器**小程序打开 Internet Information Services。

2. 展开 " **连接** " 窗格中的 treeview。 选择 " **应用程序池** " 节点。

3. 若要将默认应用程序池设置为使用 .NET Framework 4 (这可能会导致) 的现有站点出现不兼容问题，请右键单击 " **DefaultAppPool** " 列表项并选择 " **基本设置 ...**"。 将 **.Net Framework 版本** 下拉菜单 **设置 (或** 更高版本) 。

4. 若要创建一个新的应用程序池，该应用程序池使用 .NET Framework 4 (来保留其他应用程序的兼容性) ，右键单击 " **应用程序** 池" 节点，然后选择 " **添加应用程序池 ...**"。 命名新的应用程序池，并将 **.Net Framework 版本** 下拉菜单设置为 **.net framework v v4.0.30128** (或更高版本) 。 运行以下安装步骤后，右键单击 **ServiceModelSamples** 应用程序，然后选择 " **管理应用程序**、 **高级设置 ...**"。 将 **应用程序池** 设置为新的应用程序池。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WebHost\ASPNetCompatibility`

此示例基于实现计算器服务的 [入门](getting-started-sample.md)。 `ICalculator` 协定已根据 `ICalculatorSession` 协定进行修改，允许在保持运行结果的同时执行一组运算。

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface ICalculatorSession
{
    [OperationContract]
    void Clear();
    [OperationContract]
    void AddTo(double n);
    [OperationContract]
    void SubtractFrom(double n);
    [OperationContract]
    void MultiplyBy(double n);
    [OperationContract]
    void DivideBy(double n);
    [OperationContract]
    double Result();
}
```

在调用多个服务操作来执行计算时，服务将使用该功能保持每个客户端的状态。 客户端可以通过调用 `Result` 来检索当前结果，通过调用 `Clear` 将结果清零。

服务使用 ASP.NET 会话存储每个客户端会话的结果。 这使服务可以为跨多个服务调用的每个客户端保持运行结果。

> [!NOTE]
> ASP.NET 会话状态和 WCF 会话的用途非常不同。 有关 WCF 会话的详细信息，请参阅 [会话](session.md) 。

该服务对 ASP.NET 会话状态的依赖非常熟悉，需要 ASP.NET 兼容模式才能正常运行。 这些需求是通过应用 `AspNetCompatibilityRequirements` 属性以声明性方式表示的。

```csharp
[AspNetCompatibilityRequirements(RequirementsMode =
                       AspNetCompatibilityRequirementsMode.Required)]
public class CalculatorService : ICalculatorSession
{
    double Result
    {  // store result in AspNet Session
       get {
          if (HttpContext.Current.Session["Result"] != null)
             return (double)HttpContext.Current.Session["Result"];
          return 0.0D;
       }
       set
       {
          HttpContext.Current.Session["Result"] = value;
       }
    }
    public void Clear()
    {
        Result = 0.0D;
    }
    public void AddTo(double n)
    {
        Result += n;
    }
    public void SubtractFrom(double n)
    {
        Result -= n;
    }
    public void MultiplyBy(double n)
    {
        Result *= n;
    }
    public void DivideBy(double n)
    {
        Result /= n;
    }
    public double Result()
    {
        return Result;
    }
}
```

运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```console
0, + 100, - 50, * 17.65, / 2 = 441.25
Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 请确保已 [为 Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

3. 构建解决方案后，运行 Setup.bat 在 IIS 7.0 中设置 ServiceModelSamples 应用程序。 现在，ServiceModelSamples 目录应显示为 IIS 7.0 应用程序。

4. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

## <a name="see-also"></a>请参阅

- [AppFabric 承载和持久性示例](/previous-versions/appfabric/ff383418(v=azure.10))
