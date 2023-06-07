---
title: MSMQ 传输
ms.date: 03/30/2017
ms.assetid: 3f29a2fe-24df-4614-b64c-b0c084fb7003
ms.openlocfilehash: 407512cbb129dd2e5497de92c32b0641dd21080b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238023"
---
# <a name="msmq-transport"></a>MSMQ 传输

本主题列出由 MSMQ 传输生成的所有异常。  
  
## <a name="exception-list"></a>异常列表  
  
|资源代码|资源字符串|  
|-------------------|---------------------|  
|MsmqActiveDirectoryRequiresNativeTransfer|消息的绑定验证失败。 客户端无法发送消息。 绑定属性中的冲突导致此错误。 UseActiveDirectory 被设置为 true，而 QueueTransferProtocol 被设置为 Native。 若要解决该冲突，请更正这两个属性中的一个。|  
|MsmqAuthNoneRequiresProtectionNone|服务的绑定验证失败。 无法启动服务终结点或客户端。 绑定属性中的冲突导致此错误。 MsmqAuthenticationMode 被设置为 None，而 MsmqProtectionLevel 未被设置为 None。 若要解决该冲突，请更正这两个属性中的一个。|  
|MsmqCustomRequiresPerAddDLQ|消息的绑定验证失败。 客户端无法发送消息。 DeadLetterQueue 设置为 Custom，但没有指定 CustomDeadLetterQueue。 在 CustomDeadLetterQueue 属性中指定每个应用程序的死信队列的 URI。|  
|MsmqDeserializationError|反序列化 XML 消息时遇到错误。 无法接收该消息，该消息被丢弃。|  
|MsmqDLQNotWriteable|客户端的绑定验证失败。 客户端无法发送消息。 指定的死信队列不存在或无法写入。 确保该队列存在并具有正确的写入权限。|  
|MsmqGetPrivateComputerInformationError|版本检查失败，产生指定错误。 无法检测到 MSMQ 的版本。该队列通道上的所有操作都将失败。 确保已安装 MSMQ 且 MSMQ 可用。|  
|MsmqNoAssurancesForVolatile|服务的绑定验证失败。 无法启动服务终结点或客户端。 ExactlyOnce 属性被设置为 true，而 Durable 属性被设置为 false。 这不受支持。 若要解决该冲突，请更正这两个属性中的一个。|  
|MsmqNonTransactionalQueueNeeded|检测到绑定与 MSMQ 队列配置不匹配。 无法启动服务终结点。 ExactlyOnce 属性被设置为 false，而从中读取消息的队列是事务性队列。 通过将 ExactlyOnce 属性设置为 true 或者创建非事务性绑定可以更正该错误。|  
|MsmqOpenError|打开指定队列时遇到错误。 无法从队列发送或接收该消息。 确保已安装和运行 MSMQ。 同时确保可以使用所需访问模式和权限打开队列。|  
|MsmqPathLookupError|将指定队列路径名称转换为格式名称时遇到错误。 该队列通道上的所有操作都已失败。 确保队列地址有效。 安装 MSMQ 时，必须启用 Active Directory 集成并且具有对其的访问权限。|  
|MsmqPerAppDLQRequiresCustom|客户端的绑定验证失败。 客户端无法发送消息。 设置了 CustomDeadLetterQueue 属性，但 DeadLetterQueue 属性未设置为 Custom。 请将 DeadLetterQueue 属性设置为 Custom。|  
|MsmqPerAppDLQRequiresExactlyOnce|客户端的绑定验证失败。 客户端无法发送消息。 绑定属性中的冲突导致该错误。 若要使用自定义死信队列，必须将 ExactlyOnce 设置为 true 以解决该冲突。|  
|MsmqPerAppDLQRequiresMsmq4|已检测到绑定与 MSMQ 配置之间不匹配。 客户端无法发送消息。 若要使用自定义死信队列，必须具有 MSMQ 版本 4.0 或更高版本。 如果不具有 MSMQ 版本 4.0 或更高版本，则将 DeadLetterQueue 属性设置为 System 或 None。|  
|MsmqReceiveError|从队列接收消息时发生错误。 确保已安装和运行 MSMQ。 确保队列可用于从中接收消息。|  
|MsmqSameTransactionExpected|此会话发生事务错误。 会话通道出错。 无法发送或接收此会话中的消息。 不能将排队的会话与多个事务相关联。 确保使用单个事务发送或接收会话中的所有消息。|  
|MsmqSendError|发送到指定队列时发生错误。 确保已安装和运行 MSMQ。 如果要发送到本地队列，则确保该队列存在并具有所需的访问模式和权限。|  
|MsmqTimeSpanTooLarge|消息的生存时间太大。 无法发送该消息。 消息的生存时间 (TTL) 不能超过 Int32 最大值。|  
|MsmqTokenProviderNeededForCertificates|无法找到 X509SecurityTokenProvider。 无法发送该消息。 证书身份验证模式需要 X.509 令牌提供程序。 确保安全令牌提供程序可用于已安装的证书。|  
|MsmqTransactedDLQExpected|绑定和 MSMQ 配置不匹配。 无法发送消息。 在绑定中指定的自定义死信队列必须是事务队列。 确保自定义死信队列地址正确并且此队列是事务性队列。|  
|MsmqTransactionalQueueNeeded|绑定与 MSMQ 队列配置不匹配。 无法启动服务终结点。 ExactlyOnce 属性被设置为 true，而从中读取消息的队列不是事务性队列。 若要更正该错误，请将 ExactlyOnce 属性设置为 false 或者为此绑定创建事务性队列。|  
|MsmqTransactionCurrentRequired|没有任何事务可用于发送会话中的消息。 发送排队的会话中的消息需要事务。 确保已指定用于发送会话中的消息的事务范围。|  
|MsmqTransactionRequired|需要事务，但事务不可用。 无法发送或接收消息。 确保已指定用于发送或接收消息的事务范围。|  
|MsmqUnsupportedSerializationFormat|发生反序列化错误。 无法接收该消息，该消息被丢弃。 不支持指定的序列化格式。|  
|MsmqWrongPrivateQueueSyntax|URL 无效。 队列的 URL 不能包含“$”字符。 使用 net.msmq://machine/private/queueName 中的语法指定专有队列的地址。|
