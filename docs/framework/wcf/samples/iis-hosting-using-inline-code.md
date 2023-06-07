---
title: 使用内联代码的 IIS 承载
ms.date: 03/30/2017
helpviewer_keywords:
- Web hosted service
- IIS Hosting Using Inline Code Sample [Windows Communication Foundation]
ms.assetid: 56fe3687-a34b-4661-8e30-b33770f413fa
ms.openlocfilehash: 41e19c318953d7e97eb5183c5a16a3780813f94c
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90558987"
---
# <a name="iis-hosting-using-inline-code"></a>使用内联代码的 IIS 承载

此示例演示如何实现由 Internet 信息服务 (IIS) 承载的服务，该服务的服务代码以内联方式包含在一个 .svc 文件中，并且可以按需编译。 服务代码还可以直接在源代码文件（位于应用程序的 \App_Code 目录中）中实现，也可以编译为 \bin 中所部署的程序集。 此示例不演示这些技术。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WebHost\InlineCode`

本示例演示一个典型的服务，该服务实现定义“请求-答复”通信模式的协定。 该服务由 IIS 承载，服务代码完全包含在 Service.svc 文件中。 该服务由主机激活，并由发送给它的第一条消息按需编译。 没有必要进行预先编译。 该服务实现一个 `ICalculator` 协定，下面的代码对该协定进行了定义：

```csharp
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
    public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    [OperationContract]
    double Subtract(double n1, double n2);
    [OperationContract]
    double Multiply(double n1, double n2);
    [OperationContract]
    double Divide(double n1, double n2);
}
```

服务实现计算并返回相应的结果。

`<%@ServiceHost language=c# Debug="true" Service="Microsoft.ServiceModel.Samples.CalculatorService" %>`

```csharp
// Service class that implements the service contract.
public class CalculatorService : ICalculator
{
    public double Add(double n1, double n2)
    {
        return n1 + n2;
    }
    public double Subtract(double n1, double n2)
    {
        return n1 - n2;
    }
    public double Multiply(double n1, double n2)
    {
        return n1 * n2;
    }
    public double Divide(double n1, double n2)
    {
        return n1 / n2;
    }
}
```

运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

3. 构建解决方案后，运行 setup.bat 在 IIS 7.0 中设置 ServiceModelSamples 应用程序。 现在，ServiceModelSamples 目录应显示为 IIS 7.0 应用程序。

4. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。 有关如何创建可调用此服务的客户端应用程序的示例，请参阅 [如何：创建客户端](../how-to-create-a-wcf-client.md)。

## <a name="see-also"></a>请参阅

- [AppFabric 承载和持久性示例](/previous-versions/appfabric/ff383418(v=azure.10))
