---
title: 如何：创建事务性服务
ms.date: 03/30/2017
ms.assetid: 1bd2e4ed-a557-43f9-ba98-4c70cb75c154
ms.openlocfilehash: c3d094dbd5822f6025e1cc6c90aab04b61459314
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286287"
---
# <a name="how-to-create-a-transactional-service"></a>如何：创建事务性服务

本示例演示创建事务性服务和使用客户端启动的事务协调服务操作的各个方面。  
  
### <a name="creating-a-transactional-service"></a>创建事务性服务  
  
1. 创建服务协定并用 <xref:System.ServiceModel.TransactionFlowOption> 枚举中所需的设置对操作进行批注以指定传入事务需求。 请注意，您也可以将 <xref:System.ServiceModel.TransactionFlowAttribute> 放在要实现的服务类上。 这允许接口的单一实现（而不是每个实现）使用这些事务设置。  
  
    ```csharp
    [ServiceContract]  
    public interface ICalculator  
    {  
        [OperationContract]  
        // Use this to require an incoming transaction  
        [TransactionFlow(TransactionFlowOption.Mandatory)]  
        double Add(double n1, double n2);  
        [OperationContract]  
        // Use this to permit an incoming transaction  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        double Subtract(double n1, double n2);  
    }  
    ```  
  
2. 创建一个实现类，并使用 <xref:System.ServiceModel.ServiceBehaviorAttribute> 有选择地指定 <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> 和 <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A>。 应该注意的是，默认值为 60 秒的 <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> 和默认值为 <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> 的 `Unspecified` 适用于许多情况。 对于每个操作，可以根据 <xref:System.ServiceModel.OperationBehaviorAttribute> 属性的值，使用 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 属性指定在方法中执行的工作是否应在事务范围内发生。 在本例中，用于 `Add` 方法的事务与从客户端流入的强制传入事务相同，用于 `Subtract` 方法的事务与传入事务相同（如果已从客户端流入了一个事务）或者是一个以隐式方式在本地创建的新事务。  
  
    ```csharp
    [ServiceBehavior(  
        TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable,  
        TransactionTimeout = "00:00:45")]  
    public class CalculatorService : ICalculator  
    {  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Add(double n1, double n2)  
        {  
            // Perform transactional operation  
            RecordToLog($"Adding {n1} to {n2}");
            return n1 + n2;  
        }  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Subtract(double n1, double n2)  
        {  
            // Perform transactional operation  
            RecordToLog($"Subtracting {n2} from {n1}");
            return n1 - n2;  
        }  
  
        private static void RecordToLog(string recordText)  
        {  
            // Database operations omitted for brevity  
            // This is where the transaction provides specific benefit  
            // - changes to the database will be committed only when  
            // the transaction completes.  
        }  
    }  
    ```  
  
3. 在配置文件中配置绑定，指定事务上下文应进行流处理，并指定要使用的协议执行此操作。 有关详细信息，请参阅配置 [配置](servicemodel-transaction-configuration.md)。 具体地说，绑定类型是在终结点元素的 `binding` 属性中指定的。 [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-element.md)元素包含一个 `bindingConfiguration` 属性，该属性引用名为的绑定配置 `transactionalOleTransactionsTcpBinding` ，如下面的示例配置中所示。  
  
    ```xml  
    <service name="CalculatorService">  
      <endpoint address="net.tcp://localhost:8008/CalcService"  
        binding="netTcpBinding"  
        bindingConfiguration="transactionalOleTransactionsTcpBinding"  
        contract="ICalculator"  
        name="OleTransactions_endpoint" />  
    </service>  
    ```  
  
     事务流是在配置级别通过使用 `transactionFlow` 属性启用的，而事务协议是使用 `transactionProtocol` 属性指定的，如下面的配置中所示。  
  
    ```xml  
    <bindings>  
      <netTcpBinding>  
        <binding name="transactionalOleTransactionsTcpBinding"  
          transactionFlow="true"  
          transactionProtocol="OleTransactions"/>  
      </netTcpBinding>  
    </bindings>  
    ```  
  
### <a name="supporting-multiple-transaction-protocols"></a>支持多事务协议  
  
1. 为了获得最佳性能，应将 OleTransactions 协议用于涉及使用 Windows Communication Foundation (WCF) 编写的客户端和服务的方案。 但是，对于需要与第三方协议堆栈之间具有互操作性的方案，可以使用 WS-AtomicTransaction (WS-AT) 协议。 可以通过提供多个具有相应协议特定绑定的终结点，将 WCF 服务配置为接受这两个协议，如下面的示例配置所示。  
  
    ```xml  
    <service name="CalculatorService">  
      <endpoint address="http://localhost:8000/CalcService"  
        binding="wsHttpBinding"  
        bindingConfiguration="transactionalWsatHttpBinding"  
        contract="ICalculator"  
        name="WSAtomicTransaction_endpoint" />  
      <endpoint address="net.tcp://localhost:8008/CalcService"  
        binding="netTcpBinding"  
        bindingConfiguration="transactionalOleTransactionsTcpBinding"  
        contract="ICalculator"  
        name="OleTransactions_endpoint" />  
    </service>  
    ```  
  
     事务协议是使用 `transactionProtocol` 属性指定的。 但是，系统提供的 `wsHttpBinding` 中没有此属性，因为此绑定只能使用 WS-AT 协议。  
  
    ```xml  
    <bindings>  
      <wsHttpBinding>  
        <binding name="transactionalWsatHttpBinding"  
          transactionFlow="true" />  
      </wsHttpBinding>  
      <netTcpBinding>  
        <binding name="transactionalOleTransactionsTcpBinding"  
          transactionFlow="true"  
          transactionProtocol="OleTransactions"/>  
      </netTcpBinding>  
    </bindings>  
    ```  
  
### <a name="controlling-the-completion-of-a-transaction"></a>控制事务的完成  
  
1. 默认情况下，如果未引发未经处理的异常，WCF 操作会自动完成事务。 使用 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> 属性和 <xref:System.ServiceModel.OperationContext.SetTransactionComplete%2A> 方法可以修改此行为。 当需要某一操作与另一操作（例如借贷操作）在同一个事务中发生时，可以通过将 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> 属性设置为 `false` 来禁用自动完成行为，如下面的 `Debit` 操作示例所示。 在调用 `Debit` 属性设置为 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> 的方法前（如操作 `true` 所示）或调用 `Credit1` 方法以将事务显式标记为完成前（如操作 <xref:System.ServiceModel.OperationContext.SetTransactionComplete%2A> 所示），`Credit2` 操作使用的事务不会完成。 请注意，所示的两个贷记操作仅供演示，更常见的情况是只使用一个贷记操作。  
  
    ```csharp
    [ServiceBehavior]  
    public class CalculatorService : IAccount  
    {  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = false)]  
        public void Debit(double n)  
        {  
            // Perform debit operation  
  
            return;  
        }  
  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = true)]  
        public void Credit1(double n)  
        {  
            // Perform credit operation  
  
            return;  
        }  
  
        [OperationBehavior(  
            TransactionScopeRequired = true, TransactionAutoComplete = false)]  
        public void Credit2(double n)  
        {  
            // Perform alternate credit operation  
  
            OperationContext.Current.SetTransactionComplete();  
            return;  
        }  
    }  
    ```  
  
2. 出于事务关联的目的而将 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> 属性设置为 `false` 需要使用会话绑定。 这一需求使用 `SessionMode` 属性在 <xref:System.ServiceModel.ServiceContractAttribute> 上指定。  
  
    ```csharp
    [ServiceContract(SessionMode = SessionMode.Required)]  
    public interface IAccount  
    {  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Debit(double n);  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Credit1(double n);  
        [OperationContract]  
        [TransactionFlow(TransactionFlowOption.Allowed)]  
        void Credit2(double n);  
    }  
    ```  
  
### <a name="controlling-the-lifetime-of-a-transactional-service-instance"></a>控制事务性服务实例的生存期  
  
1. WCF 使用 <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A> 属性来指定在事务完成时是否释放基础服务实例。 由于默认情况下默认为，因此， `true` WCF 展示了有效且可预测的 "实时" 激活行为。 在后续事务上调用服务可确保新服务实例不会保留前一个事务的状态。 虽然此行为通常很有用，但有时你可能希望服务实例在事务完成后仍然保持状态。 例如，所需的状态或资源句柄成本昂贵，难以检索或重建时就属于这种情况。 通过将 <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A> 属性设置为 `false`，可以实现此目的。 使用该设置时，实例和任何关联状态将可用于后续调用。 使用此设置时，应仔细考虑状态和事务何时清除和完成以及如何清除和完成。 下面的示例演示如何通过对 `runningTotal` 变量保持实例来实现这一目的。  
  
    ```csharp
    [ServiceBehavior(TransactionIsolationLevel = [ServiceBehavior(  
        ReleaseServiceInstanceOnTransactionComplete = false)]  
    public class CalculatorService : ICalculator  
    {  
        double runningTotal = 0;  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Add(double n)  
        {  
            // Perform transactional operation  
            RecordToLog($"Adding {n} to {runningTotal}");
            runningTotal = runningTotal + n;  
            return runningTotal;  
        }  
  
        [OperationBehavior(TransactionScopeRequired = true)]  
        public double Subtract(double n)  
        {  
            // Perform transactional operation  
            RecordToLog($"Subtracting {n} from {runningTotal}");
            runningTotal = runningTotal - n;  
            return runningTotal;  
        }  
  
        private static void RecordToLog(string recordText)  
        {  
            // Database operations omitted for brevity  
        }  
    }  
    ```  
  
    > [!NOTE]
    > 由于实例的生存期是服务的内在行为，并通过 <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性进行控制，因此不需要修改服务配置或服务协定来设置实例行为。 另外，网络上也不包含这种表示形式。
