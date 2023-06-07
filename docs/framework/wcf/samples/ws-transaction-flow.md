---
title: WS 事务流
ms.date: 03/30/2017
helpviewer_keywords:
- Transactions
ms.assetid: f8eecbcf-990a-4dbb-b29b-c3f9e3b396bd
ms.openlocfilehash: 7fd4968bbe4e1a3dafbfc35cc0617cef7083d291
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96252382"
---
# <a name="ws-transaction-flow"></a>WS 事务流

本示例演示客户端协调事务和使用 WS-Atomic 事务或 OleTransactions 协议的事务流的客户端和服务器选项的用法。 此示例基于实现计算器服务的 [入门](getting-started-sample.md) ，但操作的作用是为了说明如何使用 `TransactionFlowAttribute` **TransactionFlowOption** 枚举来确定启用了事务流的程度。 在流事务范围内，请求操作的日志将写入数据库并保存，直到客户端协调事务完成。如果客户端事务没有完成，则 Web 服务事务确保不提交对数据库的相应更新。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 初始化到服务的连接和事务后，客户端访问多个服务操作。 用于服务的协定定义如下，每个操作演示 `TransactionFlowOption` 的一种不同的设置。  

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract]  
    [TransactionFlow(TransactionFlowOption.Mandatory)]  
    double Add(double n1, double n2);  
    [OperationContract]  
    [TransactionFlow(TransactionFlowOption.Allowed)]  
    double Subtract(double n1, double n2);  
    [OperationContract]  
    [TransactionFlow(TransactionFlowOption.NotAllowed)]  
    double Multiply(double n1, double n2);  
    [OperationContract]  
    double Divide(double n1, double n2);
}  
```

 这按处理操作的顺序定义这些操作：  
  
- `Add` 操作请求必须包括流事务。  
  
- `Subtract` 操作请求可能包括流事务。  
  
- 由于显式 NotAllowed 设置，`Multiply` 操作请求不得包括流事务。  
  
- 由于省略 `Divide` 属性，`TransactionFlow` 操作请求不得包括流事务。  
  
 若要启用事务流， [\<transactionFlow>](../../configure-apps/file-schema/wcf/transactionflow.md) 必须使用启用了属性的绑定，以及相应的操作属性。 在本示例中，除了元数据交换终结点以外，服务的配置还公开 TCP 终结点和 HTTP 终结点。 TCP 终结点和 HTTP 终结点使用以下绑定，两者都 [\<transactionFlow>](../../configure-apps/file-schema/wcf/transactionflow.md) 启用了属性。  
  
```xml  
<bindings>  
  <netTcpBinding>  
    <binding name="transactionalOleTransactionsTcpBinding"  
             transactionFlow="true"  
             transactionProtocol="OleTransactions"/>  
  </netTcpBinding>  
  <wsHttpBinding>  
    <binding name="transactionalWsatHttpBinding"  
             transactionFlow="true" />  
  </wsHttpBinding>  
</bindings>  
```  
  
> [!NOTE]
> 系统提供的 netTcpBinding 允许使用 transactionProtocol 的规范，而系统提供的 wsHttpBinding 仅使用互操作性更强的 WSAtomicTransactionOctober2004 协议。 OleTransactions 协议仅可供 Windows Communication Foundation (WCF) 客户端使用。  
  
 对于实现 `ICalculator` 接口的类，所有方法的 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 属性都设置为 `true`。 此设置声明在方法内采取的所有操作都在事务范围内发生。 在本例中，采取的操作包括记录到日志数据库。 如果操作请求包括流事务，则操作发生在传入事务的范围内或自动生成新事务范围。  
  
> [!NOTE]
> <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 属性定义服务方法实现的本地行为，不定义客户端对事务进行流处理的能力或需求。  

```csharp
// Service class that implements the service contract.  
[ServiceBehavior(TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable)]  
public class CalculatorService : ICalculator  
{  
    [OperationBehavior(TransactionScopeRequired = true)]  
    public double Add(double n1, double n2)  
    {  
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Adding {0} to {1}", n1, n2));  
        return n1 + n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true)]  
    public double Subtract(double n1, double n2)  
    {  
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Subtracting {0} from {1}", n2, n1));  
        return n1 - n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true)]  
    public double Multiply(double n1, double n2)  
    {  
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Multiplying {0} by {1}", n1, n2));  
        return n1 * n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true)]  
    public double Divide(double n1, double n2)  
    {  
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Dividing {0} by {1}", n1, n2));  
        return n1 / n2;  
    }  
  
    // Logging method omitted for brevity  
}  
```

 在客户端，针对操作的服务的 `TransactionFlowOption` 设置反映在客户端 `ICalculator` 接口的生成的定义中。 而且服务的 `transactionFlow` 属性设置也反映在客户端的应用程序配置中。 通过选择适当的 `endpointConfigurationName`，客户端可以选择传输和协议。  

```csharp
// Create a client using either wsat or oletx endpoint configurations  
CalculatorClient client = new CalculatorClient("WSAtomicTransaction_endpoint");  
// CalculatorClient client = new CalculatorClient("OleTransactions_endpoint");  
```

> [!NOTE]
> 无论选择哪种协议或传输，观察到的本示例的行为都是相同的。  
  
 启动到服务的连接后，客户端在对服务操作的调用附近创建新的 `TransactionScope`。  

```csharp
// Start a transaction scope  
using (TransactionScope tx =  
            new TransactionScope(TransactionScopeOption.RequiresNew))  
{  
    Console.WriteLine("Starting transaction");  
  
    // Call the Add service operation  
    //  - generatedClient will flow the required active transaction  
    double value1 = 100.00D;  
    double value2 = 15.99D;  
    double result = client.Add(value1, value2);  
    Console.WriteLine("  Add({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Subtract service operation  
    //  - generatedClient will flow the allowed active transaction  
    value1 = 145.00D;  
    value2 = 76.54D;  
    result = client.Subtract(value1, value2);  
    Console.WriteLine("  Subtract({0},{1}) = {2}", value1, value2, result);  
  
    // Start a transaction scope that suppresses the current transaction  
    using (TransactionScope txSuppress =  
                new TransactionScope(TransactionScopeOption.Suppress))  
    {  
        // Call the Subtract service operation  
        //  - the active transaction is suppressed from the generatedClient  
        //    and no transaction will flow  
        value1 = 21.05D;  
        value2 = 42.16D;  
        result = client.Subtract(value1, value2);  
        Console.WriteLine("  Subtract({0},{1}) = {2}", value1, value2, result);  
  
        // Complete the suppressed scope  
        txSuppress.Complete();  
    }  
  
    // Call the Multiply service operation  
    // - generatedClient will not flow the active transaction  
    value1 = 9.00D;  
    value2 = 81.25D;  
    result = client.Multiply(value1, value2);  
    Console.WriteLine("  Multiply({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Divide service operation.  
    // - generatedClient will not flow the active transaction  
    value1 = 22.00D;  
    value2 = 7.00D;  
    result = client.Divide(value1, value2);  
    Console.WriteLine("  Divide({0},{1}) = {2}", value1, value2, result);  
  
    // Complete the transaction scope  
    Console.WriteLine("  Completing transaction");  
    tx.Complete();  
}  
  
Console.WriteLine("Transaction committed");  
```

 对操作的调用如下：  
  
- `Add` 请求将必需的事务流动到服务，服务的操作在客户端的事务范围内发生。  
  
- 第一个 `Subtract` 请求也将允许的事务流动到服务，服务的操作再次在客户端的事务范围内发生。  
  
- 第二个 `Subtract` 请求在用 `TransactionScopeOption.Suppress` 选项声明的新事务范围内执行。 这会禁止客户端的初始外层事务，请求不会将事务流动到服务。 此方法允许客户端显式放弃和防止将事务流动到服务（不需要流动时）。 服务的操作发生在新的未连接的事务范围内。  
  
- `Multiply` 请求不会将事务流动到服务，因为客户端的生成的 `ICalculator` 接口定义包括设置为 <xref:System.ServiceModel.TransactionFlowAttribute><xref:System.ServiceModel.TransactionFlowOption> 的 `NotAllowed`。  
  
- `Divide` 请求不会将事务流动到服务，因为客户端的 `ICalculator` 接口生成的定义再次没有包括 `TransactionFlowAttribute`。 服务的操作再次发生在另一个新的未连接的事务范围内。  
  
 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
Starting transaction  
  Add(100,15.99) = 115.99  
  Subtract(145,76.54) = 68.46  
  Subtract(21.05,42.16) = -21.11  
  Multiply(9,81.25) = 731.25  
  Divide(22,7) = 3.14285714285714  
  Completing transaction  
Transaction committed  
Press <ENTER> to terminate client.  
```  
  
 服务操作请求的日志记录显示在服务的控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
Press <ENTER> to terminate the service.  
  Writing row to database: Adding 100 to 15.99  
  Writing row to database: Subtracting 76.54 from 145  
  Writing row to database: Subtracting 42.16 from 21.05  
  Writing row to database: Multiplying 9 by 81.25  
  Writing row to database: Dividing 22 by 7  
```  
  
 成功执行示例后，客户端的事务范围完成并提交在该范围内采取的所有操作。 特别是，上面提到的 5 个记录保留在服务的数据库中。 其中的前 2 个记录已在客户端的事务范围内发生。  
  
 如果在客户端的 `TransactionScope` 内的任意位置发生异常，则事务无法完成。 这将导致在该范围内写入日志的记录不会被提交到数据库。 注释掉完成外层 `TransactionScope` 的调用后重复运行该示例可以观察到此效果。 当如此运行时，只记录后 3 个操作（来自第二个 `Subtract`、`Multiply` 和 `Divide` 请求），因为客户端事务不流到这些操作。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 若要生成 c # 或 Visual Basic .NET 版本的解决方案，请按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作  
  
2. 确保已安装 SQL Server Express Edition 或 SQL Server，并确保已在服务的应用程序配置文件中正确设置连接字符串。 若要在不使用数据库的情况下运行示例，请将服务的应用程序配置文件中的 `usingSql` 值设置为 `false`  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
    > [!NOTE]
    > 对于跨计算机配置，请按照下面的说明操作来启用分布式事务处理协调器，并使用 Windows SDK 中的 WsatConfig.exe 工具来启用 WCF 事务网络支持。 有关设置 WsatConfig.exe 的信息，请参阅 [配置 WS-Atomic 事务支持](../feature-details/configuring-ws-atomic-transaction-support.md)。  
  
 无论是在同一台计算机上还是在不同的计算机上运行此示例，都必须配置 Microsoft 分布式事务处理协调器 (MSDTC) ，以启用网络事务流，并使用 WsatConfig.exe 工具启用 WCF 事务网络支持。  
  
### <a name="to-configure-the-microsoft-distributed-transaction-coordinator-msdtc-to-support-running-the-sample"></a>配置 Microsoft 分布式事务处理协调器 (MSDTC) 以支持运行示例  
  
1. 在运行 Windows Server 2003 或 Windows XP 的服务计算机上，按以下说明配置 MSDTC 以允许传入网络事务。  
  
    1. 从 " **开始** " 菜单，导航到 **"控制面板**"，然后依次导航到 " **管理工具**" 和 " **组件服务**"。  
  
    2. 展开 " **组件服务**"。 打开 " **计算机** " 文件夹。  
  
    3. 右键单击 **我的电脑** ，然后选择 " **属性**"。  
  
    4. 在 " **MSDTC** " 选项卡上，单击 " **安全配置**"。  
  
    5. 检查 " **网络 DTC 访问** " 和 " **允许入站**"。  
  
    6. 单击 **"确定**"，然后单击 **"是** " 以重新启动 MSDTC 服务。  
  
    7. 单击 **“确定”** 关闭对话框。  
  
2. 在运行 Windows Server 2008 或 Windows Vista 的服务计算机上，按以下说明配置 MSDTC 以允许传入网络事务。  
  
    1. 从 " **开始** " 菜单，导航到 **"控制面板**"，然后依次导航到 " **管理工具**" 和 " **组件服务**"。  
  
    2. 展开 " **组件服务**"。 打开 " **计算机** " 文件夹。 选择 **分布式事务处理协调器**。  
  
    3. 右键单击 " **DTC 协调器** "，然后选择 " **属性**"。  
  
    4. 在 " **安全** " 选项卡上，选中 " **网络 DTC 访问** " 和 " **允许入站**"。  
  
    5. 单击 **"确定**"，然后单击 **"是** " 以重新启动 MSDTC 服务。  
  
    6. 单击 **“确定”** 关闭对话框。  
  
3. 在客户端计算机上，配置 MSDTC 以允许传出网络事务：  
  
    1. 从 " **开始** " 菜单中，依次导航到 `Control Panel` " **管理工具**" 和 " **组件服务**"。  
  
    2. 右键单击 **我的电脑** ，然后选择 " **属性**"。  
  
    3. 在 " **MSDTC** " 选项卡上，单击 " **安全配置**"。  
  
    4. 检查 " **网络 DTC 访问** " 和 " **允许出站**"。  
  
    5. 单击 **"确定**"，然后单击 **"是** " 以重新启动 MSDTC 服务。  
  
    6. 单击 **“确定”** 关闭对话框。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\TransactionFlow`
