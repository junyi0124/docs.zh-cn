---
title: 服务和事务
ms.date: 03/30/2017
helpviewer_keywords:
- service contracts [WCF], designing services and transactions
ms.assetid: 864813ff-2709-4376-912d-f5c8d318c460
ms.openlocfilehash: e60f84aafe6c62a657cd3f27c9ccdb6b65186c35
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96235904"
---
# <a name="services-and-transactions"></a>服务和事务

Windows Communication Foundation (WCF) 应用程序可以从客户端中启动事务，并在服务操作中协调事务。 客户端可以启动事务和调用多个服务操作，并可确保服务操作作为一个单元提交或回滚。  
  
 通过为需要客户端事务的服务操作指定 <xref:System.ServiceModel.ServiceBehaviorAttribute> 并设置其 <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> 和 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 属性，可以启用服务协定中的事务行为。 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> 参数指定在没有引发未处理异常的情况下，在其中执行方法的事务是否自动完成。 有关这些属性的详细信息，请[参阅。](./feature-details/servicemodel-transaction-attributes.md)  
  
 在服务操作中执行并由资源管理器管理的工作（如记录数据库更新）是客户端事务的一部分。  
  
 下面的示例演示如何使用 <xref:System.ServiceModel.ServiceBehaviorAttribute> 和 <xref:System.ServiceModel.OperationBehaviorAttribute> 属性来控制服务端的事务行为。  
  
```csharp
[ServiceBehavior(TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable)]  
public class CalculatorService: ICalculatorLog  
{  
    [OperationBehavior(TransactionScopeRequired = true,  
                           TransactionAutoComplete = true)]  
    public double Add(double n1, double n2)  
    {  
        recordToLog($"Added {n1} to {n2}");
        return n1 + n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true,
                               TransactionAutoComplete = true)]  
    public double Subtract(double n1, double n2)  
    {  
        recordToLog($"Subtracted {n1} from {n2}");
        return n1 - n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true,
                                       TransactionAutoComplete = true)]  
    public double Multiply(double n1, double n2)  
    {  
        recordToLog($"Multiplied {n1} by {n2}");
        return n1 * n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true,
                                       TransactionAutoComplete = true)]  
    public double Divide(double n1, double n2)  
    {  
        recordToLog($"Divided {n1} by {n2}", n1, n2);
        return n1 / n2;  
    }  
  
}  
```  
  
 可以通过将客户端和服务绑定配置为使用 WS-AtomicTransaction 协议，并将元素设置为，来启用事务和事务流 [\<transactionFlow>](../configure-apps/file-schema/wcf/transactionflow.md) `true` ，如下面的示例配置中所示。  
  
```xml  
<client>  
    <endpoint address="net.tcp://localhost/ServiceModelSamples/service"
          binding="netTcpBinding"
          bindingConfiguration="netTcpBindingWSAT"
          contract="Microsoft.ServiceModel.Samples.ICalculatorLog" />  
</client>  
  
<bindings>  
    <netTcpBinding>  
        <binding name="netTcpBindingWSAT"  
                transactionFlow="true"  
                transactionProtocol="WSAtomicTransactionOctober2004" />  
     </netTcpBinding>  
</bindings>  
```  
  
 客户端可以通过创建 <xref:System.Transactions.TransactionScope>，然后在该事务范围内调用服务操作来开始该事务。  
  
```csharp
using (TransactionScope ts = new TransactionScope(TransactionScopeOption.RequiresNew))  
{  
    //Do work here  
    ts.Complete();  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [System.ServiceModel 中的事务性支持](./feature-details/transactional-support-in-system-servicemodel.md)
- [事务模型](./feature-details/transaction-models.md)
- [WS 事务流](./samples/ws-transaction-flow.md)
