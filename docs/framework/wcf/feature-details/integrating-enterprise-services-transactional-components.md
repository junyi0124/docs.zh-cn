---
title: 集成企业服务事务性组件
ms.date: 03/30/2017
ms.assetid: 05dab277-b8b2-48cf-b40c-826be128b175
ms.openlocfilehash: d235806ba94d68cadca91a17361bfd5bab1e1332
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265864"
---
# <a name="integrating-enterprise-services-transactional-components"></a>集成企业服务事务性组件

Windows Communication Foundation (WCF) 提供了用于与企业服务集成的自动机制 (请参阅 [与 COM + 应用程序集成](integrating-with-com-plus-applications.md)) 。 但是，你可能希望灵活地开发可在内部使用承载于企业服务中的事务性组件的服务。 由于 WCF 事务功能是基于 <xref:System.Transactions> 基础结构构建的，因此将企业服务与 WCF 集成的过程与指定和企业服务之间的互操作性的过程相同 <xref:System.Transactions> ，如 [与企业服务和 com + 事务的互操作性](/previous-versions/dotnet/netframework-3.0/ms229974(v=vs.85))中所述。  
  
 若要在传入的流式事务和 COM+ 上下文事务之间提供所需级别的互操作性，服务实现必须创建一个 <xref:System.Transactions.TransactionScope> 实例并使用 <xref:System.Transactions.EnterpriseServicesInteropOption> 枚举中的适当值。  
  
## <a name="integrating-enterprise-services-with-a-service-operation"></a>使企业服务与服务操作相集成  

 下面的代码演示对 Allowed 事务流执行的操作，该操作创建一个具有 <xref:System.Transactions.TransactionScope> 选项的 <xref:System.Transactions.EnterpriseServicesInteropOption.Full>。 以下条件适用于这种情况：  
  
- 如果客户端对事务进行流式处理，则操作（包括调用企业服务组件）将在该事务范围内执行。 使用 <xref:System.Transactions.EnterpriseServicesInteropOption.Full> 可确保事务与 <xref:System.EnterpriseServices> 上下文同步，这意味着 <xref:System.Transactions> 的环境事务和 <xref:System.EnterpriseServices> 相同。  
  
- 如果客户端不对事务进行流式处理，则将 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 设置为 `true` 可为操作创建新的事务范围。 同样，使用 <xref:System.Transactions.EnterpriseServicesInteropOption.Full> 可确保操作的事务与 <xref:System.EnterpriseServices> 组件的上下文中使用的事务相同。  
  
 任何其他方法调用也发生在同一个操作的事务范围内。  
  
```csharp
[ServiceContract()]  
public interface ICustomerServiceContract  
{  
   [OperationContract]  
   [TransactionFlow(TransactionFlowOption.Allowed)]  
   void UpdateCustomerNameOperation(int customerID, string newCustomerName);  
}  
  
[ServiceBehavior(TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable)]  
public class CustomerService : ICustomerServiceContract  
{  
   [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]  
   public void UpdateCustomerNameOperation(int customerID, string newCustomerName)  
   {  
   // Create a transaction scope with full ES interop  
      using (TransactionScope ts = new TransactionScope(  
                     TransactionScopeOption.Required,  
                     new TransactionOptions(),  
                     EnterpriseServicesInteropOption.Full))  
      {  
         // Create an Enterprise Services component  
         // Call UpdateCustomer method on an Enterprise Services
         // component
  
         // Call UpdateOtherCustomerData method on an Enterprise
         // Services component
         ts.Complete();  
      }  
  
      // Do UpdateAdditionalData on an non-Enterprise Services  
      // component  
   }  
}  
```  
  
 如果某一操作的当前事务和对事务性企业服务组件的调用之间不需要同步，则在实例化 <xref:System.Transactions.EnterpriseServicesInteropOption.None> 实例时请使用 <xref:System.Transactions.TransactionScope> 选项。  
  
## <a name="integrating-enterprise-services-with-a-client"></a>使企业服务与客户端相集成  

 下面的代码演示使用 <xref:System.Transactions.TransactionScope> 实例的客户端代码，该实例具有 <xref:System.Transactions.EnterpriseServicesInteropOption.Full> 设置。 在这种情况下，对支持事务流的服务操作的调用与对企业服务组件的调用发生在同一个事务范围内。  
  
```csharp
static void Main()  
{  
    // Create a client  
    CalculatorClient client = new CalculatorClient();  
  
    // Create a transaction scope with full ES interop  
    using (TransactionScope ts = new TransactionScope(  
          TransactionScopeOption.Required,  
          new TransactionOptions(),  
          EnterpriseServicesInteropOption.Full))  
    {  
        // Call Add calculator service operation  
  
        // Create an Enterprise Services component  
  
        // Call UpdateCustomer method on an Enterprise Services
        // component
  
        ts.Complete();  
    }  
  
    // Closing the client gracefully closes the connection and
    // cleans up resources  
    client.Close();  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [与 COM + 应用程序集成](integrating-with-com-plus-applications.md)
- [与 COM 应用程序集成](integrating-with-com-applications.md)
