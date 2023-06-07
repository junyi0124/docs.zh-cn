---
title: ServiceModel 事务属性
ms.date: 03/30/2017
helpviewer_keywords:
- transactions [WCF], ServiceModel attributes
ms.assetid: 1e0d2436-6ae5-439b-9765-a448d6f60000
ms.openlocfilehash: d4b7482431404241577111d8dd3841319b65696e
ms.sourcegitcommit: d6e27023aeaffc4b5a3cb4b88685018d6284ada4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2019
ms.locfileid: "67663699"
---
# <a name="servicemodel-transaction-attributes"></a>ServiceModel 事务属性

Windows Communication Foundation (WCF) 提供有关以下三个标准属性<xref:System.ServiceModel>属性，可使您可以配置的 WCF 服务事务行为：

- <xref:System.ServiceModel.TransactionFlowAttribute>

- <xref:System.ServiceModel.ServiceBehaviorAttribute>

- <xref:System.ServiceModel.OperationBehaviorAttribute>

## <a name="transactionflowattribute"></a>TransactionFlowAttribute

<xref:System.ServiceModel.TransactionFlowAttribute> 属性指定服务协定中的操作是否愿意接受来自客户端的传入事务。 该属性提供了此控件使用以下属性：事务使用<xref:System.ServiceModel.TransactionFlowOption>枚举来指定是否传入事务<xref:System.ServiceModel.TransactionFlowOption.Mandatory>， <xref:System.ServiceModel.TransactionFlowOption.Allowed>，或<xref:System.ServiceModel.TransactionFlowOption.NotAllowed>。

此属性是将服务操作和与客户端的外部交互操作相关联的唯一属性。 下面几节中说明的属性与在操作的执行中使用的事务有关。

## <a name="servicebehaviorattribute"></a>ServiceBehaviorAttribute

<xref:System.ServiceModel.ServiceBehaviorAttribute> 属性指定服务协定实现的内部执行行为。 此属性 (Attribute) 的特定于事务的属性 (Property) 包括：

- <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionAutoCompleteOnSessionClose%2A>，此属性指定会话关闭时是否完成未完成的事务。 此属性的默认值为 `false`。 如果此属性为 `true` 且传入会话正常关闭而不是由于网络或客户端故障而关闭，则会成功完成任何未完成的事务。 否则，如果此属性为 `false` 或者如果会话未正常关闭，则会话关闭时任何未完成的事务将会回滚。 如果此属性为 `true`，则传入通道必须基于会话。

- <xref:System.ServiceModel.ServiceBehaviorAttribute.ReleaseServiceInstanceOnTransactionComplete%2A>，此属性指定事务完成时是否释放基础服务实例。 此属性的默认值为 `true`。 下一个入站消息会导致创建新的基础实例，放弃上一个实例可能保持的每个事务的任何状态。 释放服务实例是服务执行的内部操作，对客户端可能已经建立的任何现有连接或会话没有影响。 此功能等效于 COM+ 提供的实时激活功能。 如果此属性为 `true`，则 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> 必须等于 <xref:System.ServiceModel.ConcurrencyMode.Single>。 否则，服务在启动过程中会引发无效配置验证异常。

- <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A>，此属性指定用于服务内事务的隔离级别；此属性采用 <xref:System.Transactions.IsolationLevel> 值之一。 如果本地隔离级别属性是 <xref:System.Transactions.IsolationLevel.Unspecified> 以外的任何值，则传入事务的隔离级别必须与此本地属性的设置相匹配。 否则会拒绝传入事务并将故障发回客户端。 如果 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 为 `true`，且没有对事务进行流处理，则此属性确定要用于本地创建的事务的 <xref:System.Transactions.IsolationLevel> 值。 如果 <xref:System.Transactions.IsolationLevel> 设置为 <xref:System.Transactions.IsolationLevel.Unspecified>，则使用 <xref:System.Transactions.IsolationLevel><xref:System.Transactions.IsolationLevel.Serializable>。

- <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A>，此属性指定一个时间段，在服务中创建的新事务必须在此时间段内完成。 如果达到此时间时事务没有完成，则会中止事务。 对于已将 <xref:System.TimeSpan> 设置为 <xref:System.Transactions.TransactionScope> 的任何操作以及为其创建了新事务的任何操作，<xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 用作 `true` 超时。 该超时是从创建事务到完成两阶段提交协议的第 1 阶段所允许的最长时间。 使用的超时值始终是 <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> 属性和 `transactionTimeout` 配置设置之间的较小值。

## <a name="operationbehaviorattribute"></a>OperationBehaviorAttribute

<xref:System.ServiceModel.OperationBehaviorAttribute> 属性指定服务实现中方法的行为。 可以用此属性指示操作的特定执行行为。 此属性的属性不会影响服务协定的 Web 服务描述语言 (WSDL) 说明，是纯粹启用常用的功能，开发人员将不得不自己实现 WCF 编程模型中的元素。

此属性 (Attribute) 具有以下特定于事务的属性 (Property)：

- <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 指定是否必须在活动事务范围内执行方法。 默认值为 `false`。 如果没有为方法设置 <xref:System.ServiceModel.OperationBehaviorAttribute> 属性，这也暗示着不会在事务中执行该方法。 如果操作不需要事务范围，则不会激活消息头中存在的任何事务，并且这些事务将保持作为 <xref:System.ServiceModel.OperationContext.IncomingMessageProperties%2A> 的 <xref:System.ServiceModel.OperationContext> 的元素。 如果操作需要事务范围，则从下列各项之一中派生该事务的源：

  - 如果从客户端流动事务，则在使用该分布式事务创建的事务范围内执行此方法。

  - 使用排队传输时，使用用于对消息取消排队的事务。 请注意，使用的事务不是流事务，因为它不是由消息的原始发送方提供。

  - 自定义传输可以通过使用 `TransportTransactionProperty` 提供事务。

  - 如果上面的任一属性均没有为事务提供外部源，则会在调用方法之前创建一个新的 <xref:System.Transactions.Transaction> 实例。

- <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> 指定在没有引发未处理的异常的情况下，在其中执行方法的事务是否自动完成。 如果此属性为 `true`，则当用户方法在未引发异常的情况下返回时，调用基础结构会自动将事务标记为“已完成”。 如果此属性为 `false`，则事务会附加到实例，并且仅当客户端调用标记为此属性等于 `true` 的后续方法时，或仅当后续方法显式调 <xref:System.ServiceModel.OperationContext.SetTransactionComplete%2A> 时，事务才会标记为“已完成”。 不执行这两种方案之一会导致事务永远也不会处于“已完成”状态，其中所包含的工作也不会提交，除非将 <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionAutoCompleteOnSessionClose%2A> 属性设置为 `true`。 如果此属性设置为 `true`，则必须与会话一起使用通道，且必须将 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> 设置为 <xref:System.ServiceModel.InstanceContextMode.PerSession>。
