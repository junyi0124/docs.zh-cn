---
title: 服务事务行为
ms.date: 03/30/2017
helpviewer_keywords:
- Service Transaction Behavior Sample [Windows Communication Foundation]
ms.assetid: 1a9842a3-e84d-427c-b6ac-6999cbbc2612
ms.openlocfilehash: 0be5bf0dbe6416febb898fb5150c5a516c8b0969
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591522"
---
# <a name="service-transaction-behavior"></a>服务事务行为

此示例演示客户端协调事务的用法，以及为了控制服务事务行为而对 ServiceBehaviorAttribute 和 OperationBehaviorAttribute 进行的设置。 此示例基于实现计算器服务的[入门](getting-started-sample.md)，但扩展为维护数据库表中所执行操作的服务器日志和计算器操作的有状态运行总计。 持久写入服务器日志表依赖于客户端协调事务的结果 - 如果客户端事务未完成，Web 服务事务可以确保不会提交对数据库的更新。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

服务协定定义所有操作都要求通过请求对事务进行流处理：

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples",
                    SessionMode = SessionMode.Required)]
public interface ICalculator
{
    [OperationContract]
    [TransactionFlow(TransactionFlowOption.Mandatory)]
    double Add(double n);
    [OperationContract]
    [TransactionFlow(TransactionFlowOption.Mandatory)]
    double Subtract(double n);
    [OperationContract]
    [TransactionFlow(TransactionFlowOption.Mandatory)]
    double Multiply(double n);
    [OperationContract]
    [TransactionFlow(TransactionFlowOption.Mandatory)]
    double Divide(double n);
}
```

为了启用传入事务流，使用系统提供的 wsHttpBinding 对服务进行了配置，并将 transactionFlow 属性设置为 `true`。 此绑定使用可互操作的 WSAtomicTransactionOctober2004 协议：

```xml
<bindings>
  <wsHttpBinding>
    <binding name="transactionalBinding" transactionFlow="true" />
  </wsHttpBinding>
</bindings>
```

启动与服务和事务的连接后，客户端访问该事务范围内的几个服务操作，然后完成该事务并断开连接：

```csharp
// Create a client
CalculatorClient client = new CalculatorClient();

// Create a transaction scope with the default isolation level of Serializable
using (TransactionScope tx = new TransactionScope())
{
    Console.WriteLine("Starting transaction");

    // Call the Add service operation.
    double value = 100.00D;
    Console.WriteLine("  Adding {0}, running total={1}",
                                        value, client.Add(value));

    // Call the Subtract service operation.
    value = 45.00D;
    Console.WriteLine("  Subtracting {0}, running total={1}",
                                        value, client.Subtract(value));

    // Call the Multiply service operation.
    value = 9.00D;
    Console.WriteLine("  Multiplying by {0}, running total={1}",
                                        value, client.Multiply(value));

    // Call the Divide service operation.
    value = 15.00D;
    Console.WriteLine("  Dividing by {0}, running total={1}",
                                        value, client.Divide(value));

    Console.WriteLine("  Completing transaction");
    tx.Complete();
}

Console.WriteLine("Transaction committed");

// Closing the client gracefully closes the connection and cleans up resources
client.Close();
```

服务上有三个属性通过以下方式影响服务事务行为：

- 在 `ServiceBehaviorAttribute` 上：

  - `TransactionTimeout` 属性指定必须完成事务的时间段。 在此示例中，此属性设置为 30 秒。

  - `TransactionIsolationLevel` 属性指定服务支持的隔离级别。 这是与客户端隔离级别相匹配所必需的。

  - `ReleaseServiceInstanceOnTransactionComplete` 属性指定当事务完成后是否回收服务实例。 如果将此属性设置为 `false`，服务将在所有操作请求中维护相同的服务实例。 这是维护运行总数所必需的。 如果设置为 `true`，每完成一个操作就会生成一个新实例。

  - `TransactionAutoCompleteOnSessionClose` 属性指定会话关闭时是否完成未处理的事务。 通过将其设置为 `false` ，每个操作都需要将属性设置 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete?displayProperty=nameWithType> 为 `true` 或显式要求调用 <xref:System.ServiceModel.OperationContext.SetTransactionComplete?displayProperty=nameWithType> 方法才能完成事务。 此示例对这两种方法进行了演示。

- 在 `ServiceContractAttribute` 上：

  - `SessionMode` 属性指定服务是否将相应的请求相互关联，从而形成一个逻辑会话。 因为此服务包括将 OperationBehaviorAttribute TransactionAutoComplete 属性设置为 `false` 时的操作（Multiply 和 Divide），所以必须指定 `SessionMode.Required`。 例如，Multiply 操作不会完成其事务，而是依赖稍后使用 `SetTransactionComplete` 方法对 Divide 操作的调用；服务必须能够确定这些服务是在同一个会话中发生的。

- 在 `OperationBehaviorAttribute` 上：

  - `TransactionScopeRequired` 属性指定是否应在事务范围内执行操作。 在此示例中，此属性对于所有操作都设置为 `true`，同时由于客户端将其事务流向所有操作，所以操作是在该客户端事务的范围内发生的。

  - `TransactionAutoComplete` 属性指定如果没有发生未处理的异常，是否自动完成在其中执行方法的事务。 如前面所讲，此属性对于 Add 和 Subtract 操作设置为 `true`，但对于 Multiply 和 Divide 操作则设置为 `false`。 Add 和 Subtract 操作自动完成操作，Divide 操作通过显式调用 `SetTransactionComplete` 方法完成操作，Multiply 操作不会完成，而是依赖并请求稍后的调用（例如，调用 Divide）来完成操作。

属性化服务实现如下：

```csharp
[ServiceBehavior(
    TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable,
    TransactionTimeout = "00:00:30",
    ReleaseServiceInstanceOnTransactionComplete = false,
    TransactionAutoCompleteOnSessionClose = false)]
public class CalculatorService : ICalculator
{
    double runningTotal;

    public CalculatorService()
    {
        Console.WriteLine("Creating new service instance...");
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public double Add(double n)
    {
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Adding {0} to {1}", n, runningTotal));
        runningTotal = runningTotal + n;
        return runningTotal;
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public double Subtract(double n)
    {
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Subtracting {0} from {1}", n, runningTotal));
        runningTotal = runningTotal - n;
        return runningTotal;
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = false)]
    public double Multiply(double n)
    {
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Multiplying {0} by {1}", runningTotal, n));
        runningTotal = runningTotal * n;
        return runningTotal;
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = false)]
    public double Divide(double n)
    {
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Dividing {0} by {1}", runningTotal, n));
        runningTotal = runningTotal / n;
        OperationContext.Current.SetTransactionComplete();
        return runningTotal;
    }

    // Logging method omitted for brevity
}
```

运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```console
Starting transaction
Performing calculations...
  Adding 100, running total=100
  Subtracting 45, running total=55
  Multiplying by 9, running total=495
  Dividing by 15, running total=33
  Completing transaction
Transaction committed
Press <ENTER> to terminate client.
```

服务操作请求的日志记录显示在服务的控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```console
Press <ENTER> to terminate service.
Creating new service instance...
  Writing row 1 to database: Adding 100 to 0
  Writing row 2 to database: Subtracting 45 from 100
  Writing row 3 to database: Multiplying 55 by 9
  Writing row 4 to database: Dividing 495 by 15
```

请注意，除了维护计算的运行总数外，服务还报告实例的创建（此示例中有一个实例）并将操作请求记录到数据库中。 因为所有请求都对客户端事务进行流处理，所以只要未完成事务就会导致所有数据库操作回滚。 这可以通过几种方式进行演示：

- 注释掉客户端对 `tx.Complete`() 的调用并重新运行 - 这将导致客户端在未完成其事务的情况下退出事务范围。

- 注释掉对 Divide 服务操作的调用 - 这将导致由 Multiply 操作启动的操作无法完成，从而使客户端事务最终也无法完成。

- 在客户端事务范围内的任意位置引发未处理的异常 - 这同样会阻止客户端完成其事务。

上述任一情况下的结果都是在该范围内执行的任何操作不会提交，持久化到数据库中的行计数也不会增加。

> [!NOTE]
> 作为生成过程的一部分，数据库文件被复制到 bin 文件夹中。 您必须查看数据库文件的副本以观察持久化到日志中的行数，而不是 Visual Studio 项目中包括的文件。

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已安装 SQL Server 2005 Express Edition 或 SQL Server 2005。 在服务的 App.config 文件中，可能设置了数据库 `connectionString`，或者通过将 appSettings `usingSql` 值设置为 `false` 而禁用了数据库交互。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

3. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

如果跨计算机运行此示例，则必须配置 Microsoft 分布式事务处理协调器（MSDTC），以启用网络事务流并使用 Wsatconfig.exe 工具启用 Windows Communication Foundation （WCF）事务网络支持。

### <a name="to-configure-the-microsoft-distributed-transaction-coordinator-msdtc-to-support-running-the-sample-across-machines"></a>配置 Microsoft 分布式事务处理协调器 (MSDTC) 以支持跨计算机运行示例

1. 在服务计算机上，配置 MSDTC 以允许传入网络事务。

    1. 从 "**开始**" 菜单，导航到 **"控制面板**"，然后依次导航到 "**管理工具**" 和 "**组件服务**"。

    2. 右键单击**我的电脑**，然后选择 "**属性**"。

    3. 在 " **MSDTC** " 选项卡上，单击 "**安全配置**"。

    4. 检查 "**网络 DTC 访问**" 和 "**允许入站**"。

    5. 单击 **"是"** 以重新启动 MS DTC 服务，然后单击 **"确定"**。

    6. 单击“确定”  关闭对话框。

2. 在服务计算机和客户端计算机上，配置 Windows 防火墙以便在例外应用程序列表中包括 Microsoft 分布式事务处理协调器 (MSDTC)：

    1. 从“控制面板”上运行 Windows 防火墙应用程序。

    2. 在 "**例外**" 选项卡中，单击 "**添加程序**"。

    3. 浏览到文件夹 C:\WINDOWS\System32。

    4. 选择 "Msdtc"，并单击 "**打开**"。

    5. 单击 **"确定"** 关闭 "**添加程序**" 对话框，然后再次单击 **"确定"** 关闭 Windows 防火墙小程序。

3. 在客户端计算机上，配置 MSDTC 以允许传出网络事务：

    1. 从 "**开始**" 菜单，导航到 **"控制面板**"，然后依次导航到 "**管理工具**" 和 "**组件服务**"。

    2. 右键单击**我的电脑**，然后选择 "**属性**"。

    3. 在 " **MSDTC** " 选项卡上，单击 "**安全配置**"。

    4. 检查 "**网络 DTC 访问**" 和 "**允许出站**"。

    5. 单击 **"是"** 以重新启动 MS DTC 服务，然后单击 **"确定"**。

    6. 单击“确定”  关闭对话框。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Behaviors\Transactions`
