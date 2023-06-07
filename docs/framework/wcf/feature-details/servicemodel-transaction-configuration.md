---
title: ServiceModel 事务配置
ms.date: 03/30/2017
helpviewer_keywords:
- transactions [WCF], ServiceModel configuration
ms.assetid: 5636067a-7fbd-4485-aaa2-8141c502acf3
ms.openlocfilehash: 27deaf38a8809b8a7fca560cc6783bd24dc43686
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242885"
---
# <a name="servicemodel-transaction-configuration"></a>ServiceModel 事务配置

Windows Communication Foundation (WCF) 提供三个属性，用于为服务配置事务： `transactionFlow` 、 `transactionProtocol` 和 `transactionTimeout` 。  
  
## <a name="configuring-transactionflow"></a>配置 transactionFlow  

 WCF 提供的大多数预定义绑定都包含 `transactionFlow` 和 `transactionProtocol` 属性，以便您可以将绑定配置为使用特定的事务流协议为特定终结点接受传入事务。 此外，你可以使用 `transactionFlow` 元素及其 `transactionProtocol` 特性生成你自己的自定义绑定。 有关设置配置元素的详细信息，请参阅 [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 和 [WCF 配置架构](../../configure-apps/file-schema/wcf/index.md)。  
  
 `transactionFlow` 属性指定是否为使用绑定的服务终结点启用事务流。  
  
## <a name="configuring-transactionprotocol"></a>配置 transactionProtocol  

 `transactionProtocol` 属性指定要应用于使用绑定的服务终结点的事务协议。  
  
 下面是一个配置节示例，该配置节将指定的绑定配置为支持事务流并且使用 WS-AtomicTransaction 协议。  
  
```xml  
<netNamedPipeBinding>  
   <binding name="test"  
      closeTimeout="00:00:10"  
      openTimeout="00:00:20"
      receiveTimeout="00:00:30"  
      sendTimeout="00:00:40"  
      transactionFlow="true"  
      transactionProtocol="WSAtomicTransactionOctober2004"  
      hostNameComparisonMode="WeakWildcard"  
      maxBufferSize="1001"  
      maxConnections="123"
      maxReceivedMessageSize="1000">  
   </binding>  
</netNamedPipeBinding>  
```  
  
## <a name="configuring-transactiontimeout"></a>配置 transactionTimeout  

 您可以 `transactionTimeout` 在配置文件的元素中配置您的 WCF 服务的属性 `behavior` 。 下面的代码演示如何执行此操作。  
  
```xml  
<configuration>  
   <system.serviceModel>  
      <behaviors>  
         <behavior name="NewBehavior" transactionTimeout="00:01:00" /> <!-- 1 minute timeout -->  
      </behaviors>  
   </system.serviceModel>  
</configuration>  
```  
  
 `transactionTimeout` 属性指定了在该服务中创建的新事务必须在此期间完成的时间段。 它被用作任何建立新事务的操作的 <xref:System.Transactions.TransactionScope> 超时，而且，如果应用了 <xref:System.ServiceModel.OperationBehaviorAttribute>，则 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 属性将设置为 `true`。  
  
 超时指定了从创建事务到完成两阶段提交协议的第 1 阶段之间的持续时间。  
  
 如果在 `service` 配置节内设置了此属性，则应该通过 <xref:System.ServiceModel.OperationBehaviorAttribute> 应用相应服务的至少一个方法，其中 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 属性设置为 `true`。  
  
 请注意，所使用的超时值是此 `transactionTimeout` 配置设置和任何 <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> 属性之间的较小值。  
  
## <a name="see-also"></a>另请参阅

- [\<binding>](../../configure-apps/file-schema/wcf/bindings.md)
- [WCF 配置架构](../../configure-apps/file-schema/wcf/index.md)
