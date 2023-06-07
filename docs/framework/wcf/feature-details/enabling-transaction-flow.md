---
title: 启用事务流
ms.date: 03/30/2017
helpviewer_keywords:
- transactions [WCF], enabling flow
ms.assetid: a03f5041-5049-43f4-897c-e0292d4718f7
ms.openlocfilehash: 206cbecc35c6b517ddfc3b14f4a1d139cf72fc34
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276745"
---
# <a name="enabling-transaction-flow"></a>启用事务流

Windows Communication Foundation (WCF) 提供了非常灵活的选项来控制事务流。 服务事务流设置可以使用属性与配置的组合来表示。  
  
## <a name="transaction-flow-settings"></a>事务流设置  

 服务终结点的事务流设置根据下列三个值的交集生成：  
  
- 为服务协定中的每个方法指定的 <xref:System.ServiceModel.TransactionFlowAttribute> 属性。  
  
- 特定绑定中的 `TransactionFlow` 绑定属性。  
  
- 特定绑定中的 `TransactionFlowProtocol` 绑定属性。 `TransactionFlowProtocol` 绑定属性允许您在可用于流动事务的两个不同事务协议之间进行选择。 后面几节将对这些协议逐一进行简要描述。  
  
### <a name="ws-atomictransaction-protocol"></a>WS-AtomicTransaction 协议  

 WS-AtomicTransaction (WS-AT) 协议对于要求第三方协议堆栈具有互操作性时的情形非常有用。  
  
### <a name="oletransactions-protocol"></a>OleTransactions 协议  

 OleTransactions 协议对于如下的情形非常有用：即不要求第三方协议堆栈具有互操作性，并且服务部署人员预先知道 WS-AT 协议服务将在本地禁用或者现有网络拓扑不支持使用 WS-AT。  
  
 下表列出了可以使用这些不同组合生成的不同类型的事务流。  
  
|TransactionFlow<br /><br /> binding|TransactionFlow 绑定属性|TransactionFlowProtocol 绑定协议|事务流的类型|  
|---------------------------------|--------------------------------------|----------------------------------------------|------------------------------|  
|必需|true|WS-AT|事务必须以可以互操作的 WS-AT 格式流动。|  
|必需|true|OleTransactions|事务必须以 WCF OleTransactions 格式流动。|  
|必需|false|不适用|不适用，因为这是无效的配置。|  
|然后用户才能访问|true|WS-AT|事务可以以可互操作的 WS-AT 格式流动。|  
|然后用户才能访问|true|OleTransactions|事务可能以 WCF OleTransactions 格式流动。|  
|然后用户才能访问|false|任何值|不流动事务。|  
|NotAllowed|任何值|任何值|不流动事务。|  
  
 下表对消息处理结果做了总结。  
  
|传入消息|TransactionFlow 设置|事务标头|消息处理结果|  
|----------------------|-----------------------------|------------------------|-------------------------------|  
|事务与预期的协议格式匹配|Allowed 或 Mandatory|`MustUnderstand` 等于 `true`。|进程|  
|事务不与预期的协议格式匹配|必需|`MustUnderstand` 等于 `false`。|因要求一个事务而拒绝|  
|事务不与预期的协议格式匹配|然后用户才能访问|`MustUnderstand` 等于 `false`。|因无法理解标头而拒绝|  
|使用任何协议格式的事务|NotAllowed|`MustUnderstand` 等于 `false`。|因无法理解标头而拒绝|  
|无事务|必需|不适用|因要求一个事务而拒绝|  
|无事务|然后用户才能访问|不适用|进程|  
|无事务|NotAllowed|不适用|进程|  
  
 虽然协定上的每个方法都有不同的事务流需求，但事务流协议设置的范围处于绑定级别。 这意味着，共享同一个终结点（并因此共享同一绑定）的所有方法也共享允许或要求事务流的同一个策略以及同一个事务协议（如果适用）。  
  
## <a name="enabling-transaction-flow-at-the-method-level"></a>在方法级别启用事务流  

 对于服务协定中的所有方法，事务流要求并不总是相同。 因此，WCF 还提供基于特性的机制，以允许表示每个方法的事务流首选项。 这通过指定服务操作接受事务标头所处的级别的 <xref:System.ServiceModel.TransactionFlowAttribute> 来实现。 如果需要启用事务流，则应使用此属性标记服务协定方法。 此属性采用 <xref:System.ServiceModel.TransactionFlowOption> 枚举值之一，其中默认值为 <xref:System.ServiceModel.TransactionFlowOption.NotAllowed>。 如果指定除 <xref:System.ServiceModel.TransactionFlowOption.NotAllowed> 以外的任何值，则要求该方法不要成为单向方法。 开发人员可以使用此属性在设计时指定方法级别的事务流需求或约束。  
  
## <a name="enabling-transaction-flow-at-the-endpoint-level"></a>在终结点级别启用事务流  

 除了属性提供的方法级别事务流设置外 <xref:System.ServiceModel.TransactionFlowAttribute> ，WCF 还为事务流提供了一个终结点范围的设置，以允许管理员在更高级别控制事务流。  
  
 这可以通过 <xref:System.ServiceModel.Channels.TransactionFlowBindingElement> 来实现，该类允许您在终结点绑定设置中启用或禁用传入事务流，并允许指定传入事务所需的事务协议格式。  
  
 如果该绑定已禁用事务流，但对服务协定的操作之一要求一个传入事务，则将在服务启动时引发验证异常。  
  
 WCF 提供的大多数持续绑定 `transactionFlow` 都包含和 `transactionProtocol` 特性，使你能够将特定的绑定配置为接受传入事务。 有关设置配置元素的详细信息，请参阅 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 。  
  
 管理员或部署人员可以使用终结点级别的事务流在部署时使用配置文件来配置事务流需求或约束。  
  
## <a name="security"></a>安全性  

 若要确保系统的安全性和完整性，你必须在应用程序之间流动事务时保护消息交换。 你不应向无资格参与某一事务的任何应用程序流动或透露该事务的详细信息。  
  
 使用元数据交换将 WCF 客户端生成到未知或不受信任的 Web 服务时，在可能的情况下，对这些 Web 服务上的操作的调用应禁止显示当前事务。 下面的示例演示如何执行此操作。  
  
```csharp
//client code which has an ambient transaction  
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Suppress))  
{  
    // No transaction will flow to this operation  
    untrustedProxy.Operation1(...);  
    scope.Complete();  
}  
//remainder of client code  
```  
  
 此外，还应将这些服务配置为仅接受来自它们已对其进行身份验证和授权的客户端的传入事务。 如果传入事务来自高度受信任的客户端，则应仅接受传入事务。  
  
## <a name="policy-assertions"></a>策略断言  

 WCF 使用策略断言来控制事务流。 可以在服务的策略文档中找到策略断言，该断言是通过聚合协定、配置和属性而生成的。 客户端可以使用 HTTP GET 或 WS-MetadataExchange 请求-回复来获取服务的策略文档。 然后，客户端可以通过处理策略文档来确定服务协定上的哪些操作可以支持或要求事务流。  
  
 事务流策略断言通过指定客户端应发送给服务以表示事务的 SOAP 标头来影响事务流。 所有事务标头都必须标记为 `MustUnderstand` 等于 `true`。 以其他方式标记标头的任何消息都将被拒绝，并出现 SOAP 错误。  
  
 一个操作上只能出现一个与事务相关的策略断言。 操作上具有多个事务断言的策略文档被视为无效，并被 WCF 拒绝。 此外，每个端口类型内仅可出现一个事务协议。 在单个端口类型内引用多个事务协议的操作的策略文档被视为无效，并被工作的 [元数据实用工具工具 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md)拒绝。 事务断言出现在输出消息或单向输入消息上的策略文档也将被视为无效。
