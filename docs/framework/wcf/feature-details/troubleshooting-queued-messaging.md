---
title: 排队消息处理疑难解答
ms.date: 03/30/2017
ms.assetid: a5f2836f-018d-42f5-a571-1e97e64ea5b0
ms.openlocfilehash: f695af3d2ad498e1f5975e1a396f1e7b05bf63bc
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595123"
---
# <a name="troubleshooting-queued-messaging"></a>排队消息处理疑难解答

本部分包含有关在 Windows Communication Foundation （WCF）中使用队列的常见问题和故障排除帮助。

## <a name="common-questions"></a>常见问题

**问：** 我使用了 WCF Beta 1 并安装了 MSMQ 修补程序。 现在，是否需要移除此修补程序？

**答:** 是的。 此修补程序不再受支持。 WCF 现在可以在没有修补程序要求的情况下运行 MSMQ。

**问：** MSMQ：和有两个绑定 <xref:System.ServiceModel.NetMsmqBinding> <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> 。 我应该使用哪一种绑定以及在什么情况下使用？

**答：**<xref:System.ServiceModel.NetMsmqBinding>当你希望使用 MSMQ 作为两个 WCF 应用程序之间排队通信的传输时，请使用。 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>当你想要使用现有的 MSMQ 应用程序与新的 WCF 应用程序通信时，请使用。

**问：** 是否必须升级 MSMQ 才能使用 <xref:System.ServiceModel.NetMsmqBinding> 和 `MsmqIntegration` 绑定？

**答:** 否。 在 Windows XP 和 Windows Server 2003 上，这两种绑定都适用于 MSMQ 3.0。 升级到 Windows Vista 中的 MSMQ 4.0 时，绑定的某些功能将变为可用。

**问：** 和绑定的哪些 <xref:System.ServiceModel.NetMsmqBinding> 功能 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> 在 msmq 4.0 中可用，但在 msmq 3.0 中不可用？

**答：** 以下功能在 MSMQ 4.0 中可用，但在 MSMQ 3.0 中不可用：

- 只有 MSMQ 4.0 才支持自定义死信队列。

- MSMQ 3.0 和 MSMQ 4.0 处理病毒消息的方式不同。

- 只有 MSMQ 4.0 才支持远程事务处理读取。

有关详细信息，请参阅[Windows Vista、Windows Server 2003 和 WINDOWS XP 中的队列功能](diff-in-queue-in-vista-server-2003-windows-xp.md)之间的差异。

**问：** 是否可以在一侧排队通信和 MSMQ 4.0 一端使用 MSMQ 3.0？

**答:** 是的。

**问：** 我想要将现有的 MSMQ 应用程序与新的 WCF 客户端或服务器集成。 是否需要升级两端的 MSMQ 基础结构？

**答:** 否。 您不必将任何一端升级到 MSMQ 4.0。

## <a name="troubleshooting"></a>疑难解答

此部分包含大多数常见疑难问题的答案。 某些已知限制问题还将在发行说明中进行介绍。

**问：** 我尝试使用专用队列，但收到以下异常： `System.InvalidOperationException` ： URL 无效。 队列的 URL 不能包含“$”字符。 使用 net.msmq://machine/private/queueName 中的语法指定专有队列的地址。

**答：** 检查配置和代码中的队列统一资源标识符（URI）。 不要在 URI 中使用“$”字符。 例如，若要对名为 OrdersQueue 的专用队列进行寻址，请将 URI 指定为 `net.msmq://localhost/private/ordersQueue` 。

**问：**`ServiceHost.Open()`对排队的应用程序调用会引发以下异常： `System.ArgumentException` ：基址中不能包含 URI 查询字符串。 为什么？

**答：** 检查配置文件和代码中的队列 URI。 虽然 MSMQ 队列支持使用“?”字符，但 URI 将此字符解释为字符串查询的开头。 若要避免此问题，请使用不包含“?”字符的队列名称。

**问：** 已成功发送，但没有在接收方调用任何服务操作。 为什么？

**答：** 若要确定答案，请通过以下检查列表进行操作：

- 检查事务性队列需求是否与指定的保证相符。 请注意下面的原则：

  - <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>  =  只能向事务性队列发送持久消息（数据报和会话）并只使用 "一次性" 保证（ `true` ）。

  - 可以发送只具有“一次性”保证的会话。

  - 需要使用事务从事务性队列接收会话中的消息。

  - 只能 <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>  =  向非事务性队列发送或接收易变或持久消息（仅限数据报）且无保证（ `false` ）。

- 检查死信队列。 如果在此处找到消息，则确定没有传送这些消息的原因。

- 检查传出队列的连通性或寻址问题。

**问：** 我指定了一个自定义死信队列，但当我启动发送程序应用程序时，我收到一个例外，指出未找到死信队列，或发送应用程序没有对死信队列的权限。 为何会发生这种情况？

**答：** 自定义死信队列 URI 必须在第一段包含 "localhost" 或计算机名称，例如，net.pipe：//localhost/private/myAppdead-letter queue。

**问：** 是否始终需要定义自定义死信队列，或是否有默认的死信队列？

**答：** 如果保证为 "恰好一次" （ <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>  =  `true` ），并且如果未指定自定义死信队列，则默认为系统级事务性死信队列。

如果保证为 "无 <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>  =  `false` " （），则默认情况下不是 "死信队列" 功能。

**问：** 我的服务在 SvcHost 上抛出。打开并出现消息 "调用 svchost.open 时无法满足 EndpointListener 要求"。 为什么？

A. 请检查您的服务协定。 您可能忘记了 `true` 在所有服务操作上放置 "IsOneWay ="。 队列仅支持单向服务操作。

**问：** 队列中有消息，但未调用任何服务操作。 遇到了什么问题？

**答：** 确定服务主机是否出错。 可以通过查看跟踪情况或实现 `IErrorHandler` 进行检查。 默认情况下，如果检测到病毒消息，则服务主机将出现故障。

**问：** 队列中有消息，但 Web 托管的排队服务未激活。 为什么？

**答：** 最常见的原因是权限。

1. 确保 `NetMsmqActivator` 进程正在运行，并为 `NetMsmqActivator` 进程的标识提供对此队列的读取和查找权限。

2. 如果 `NetMsmqActivator` 正在监视远程计算机上的队列，请确保 `NetMsmqActivator` 不使用受限制的令牌运行。 若要使用不受限制的令牌运行 `NetMsmqActivator`，请执行以下操作：

    ```console
    sc sidtype NetMsmqActivator unrestricted
    ```

有关非安全相关的 Web 主机问题，请参阅：[承载排队应用程序的 web](web-hosting-a-queued-application.md)。

**问：** 访问会话的最简单方法是什么？

**答：**`true`在与会话中的最后一条消息相对应的操作上设置自动完成 =，并设置 `false` 所有剩余服务操作的 "自动完成"。

**问：** 为什么 `ProtocolException` 当从包含排队会话消息和排队数据报消息的队列中进行读取时，服务才会引发。

**答：** 排队会话消息和排队数据报消息的构建方式存在根本的差异。 因此，要读取排队会话消息的服务无法接收排队数据报消息，而要读取排队数据报消息的服务也无法接收会话消息。 试图从同一个队列读取这两种消息时将引发以下异常：

```console
System.ServiceModel.MsmqPoisonMessageException: The transport channel detected a poison message. This occurred because the message exceeded the maximum number of delivery attempts or because the channel detected a fundamental problem with the message. The inner exception may contain additional information.
---> System.ServiceModel.ProtocolException: An incoming MSMQ message contained invalid or unexpected .NET Message Framing information in its body. The message cannot be received. Ensure that the sender is using a compatible service contract with a matching SessionMode.
```

当应用程序从同一台计算机既发送排队会话消息又发送排队数据报消息时，系统死信队列以及任何自定义死信队列对这种问题尤其敏感。 如果消息无法成功发送，则会被移入死信队列。 在这些情况下，很有可能将会话消息和数据报消息都移入死信队列。 从队列进行读取时，无法在运行时将两种消息分开。因此，应用程序不应从同一台计算机既发送排队会话消息又发送排队数据报消息。

### <a name="msmq-integration-specific-troubleshooting"></a>MSMQ 集成：特定的疑难解答

**问：** 发送消息时，或打开服务主机时，出现错误，指出方案错误。 为什么？

**答：** 如果使用 MSMQ 集成绑定，则必须使用 msmq.formatname 方案。 例如，msmq.formatname:DIRECT=OS:.\private$\OrdersQueue。 但是，如果您指定自定义死信队列，则必须使用 net.msmq 方案。

**问：** 使用公用或专用格式名称并在 Windows Vista 上打开服务主机时，出现错误。 为什么？

**答：** Windows Vista 上的 WCF 集成通道检查是否可以打开用于处理有害消息的主应用程序队列的子队列。 子队列名称派生自传递到侦听器的 msmq.formatname URI。 MSMQ 中的子队列名称只能是直接格式名称。 因此，您会发现以上错误。 将队列 URI 改为直接格式名。

**问：** 从 MSMQ 应用程序接收消息时，消息位于队列中，并且不由接收 WCF 应用程序读取。 为什么？

**答：** 检查消息是否具有正文。 如果消息没有正文，则 MSMQ 集成通道会忽略此消息。 实现向其通知异常的 `IErrorHandler` 并检查跟踪情况。

### <a name="security-related-troubleshooting"></a>与安全相关的疑难解答

**问：** 当我在工作组模式下运行使用默认绑定的示例时，消息似乎已发送，但接收方永远无法接收。

**答：** 默认情况下，使用需要 Active Directory Directory 服务的 MSMQ 内部证书对消息进行签名。 在工作组模式下，因为 Active Directory 不可用，所以对消息的签名会失败。 因此，会显示消息 "死信队列" 和 "失败原因"，如 "签名无效"。

解决方法是关闭安全性。 这是通过将设置 <xref:System.ServiceModel.NetMsmqSecurity.Mode%2A>  =  <xref:System.ServiceModel.NetMsmqSecurityMode.None> 为在工作组模式下工作来完成的。

另一个解决方法是从 <xref:System.ServiceModel.MsmqTransportSecurity> 属性获取 <xref:System.ServiceModel.NetMsmqSecurity.Transport%2A> 并将其设置为 <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate>，然后设置客户端证书。

还有一种解决方法就是安装集成了 Active Directory 的 MSMQ。

**问：** 当我将 Active Directory 中的消息发送到队列时，会收到 "未找到内部证书" 消息。 如何修复此问题？

**答：** 这意味着必须续订发送方 Active Directory 的证书。 为此，请打开 **"控制面板**"、"**管理工具**"、"**计算机管理**"，右键单击 " **MSMQ**"，然后选择 "**属性**"。 选择 "**用户证书**" 选项卡，然后单击 "**续订**" 按钮。

**问：** 当我使用发送消息 <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> 并指定要使用的证书时，会收到 "证书无效" 消息。 如何修复此问题？

**答：** 不能将本地计算机证书存储与证书模式一起使用。 必须使用证书管理单元将证书从计算机证书存储区复制到当前用户存储区。 若要打开证书管理单元，请执行以下操作：

1. 单击 "**开始**"，选择 "**运行**"，键入 `mmc` ，然后单击 **"确定"**。

2. 在**Microsoft 管理控制台**中，打开 "**文件**" 菜单，然后选择 "**添加/删除管理单元**"。

3. 在 "**添加/删除管理单元**" 对话框中，单击 "**添加**" 按钮。

4. 在 "**添加独立管理单元**" 对话框中，选择 "证书"，然后单击 "**添加**"。

5. 在 "**证书**管理单元" 对话框中，选择 "**我的用户帐户"，** 然后单击 "**完成**"。

6. 接下来，使用前面的步骤添加另一个证书管理单元，但这次请选择 "**计算机帐户**"，然后单击 "**下一步**"。

7. 选择“本地计算机”****，然后单击“完成”****。 现在，可以将证书从计算机证书存储区拖放到当前用户存储区。

**问：** 当我的服务在工作组模式下从另一台计算机上的队列中读取时，出现 "拒绝访问" 异常。

**答：** 在工作组模式下，对于远程应用程序，若要获取对队列的访问权限，该应用程序必须有权访问该队列。 向队列的访问控制列表（ACL）添加 "匿名登录"，并向其授予 "读取" 权限。

**问：** 当网络服务客户端（或没有域帐户的任何客户端）发送排队消息时，发送将失败，并出现无效证书。 如何修复此问题？

**答：** 检查绑定配置。 默认绑定会打开 MSMQ 传输安全以对消息进行签名。 关闭该传输安全。

### <a name="remote-transacted-receives"></a>远程事务处理接收

**问：** 当我在计算机 A 上有一个队列，而在计算机 B 上读取消息的 WCF 服务（远程事务处理接收方案）时，不会从队列中读取消息。 跟踪信息指示接收失败，并显示消息 "无法导入事务"。 如何进行修复？

**答：** 有三个可能的原因：

- 如果处于域模式下，则远程事务处理接收要求 Microsoft 分布式事务协调器 (MSDTC) 网络访问。 你可以使用 "**添加/删除组件**" 启用它。

  ![显示启用网络 DTC 访问的屏幕截图。](./media/troubleshooting-queued-messaging/enable-distributed-transaction-coordinator-access.jpg)

- 检查与事务管理器进行通信的身份验证模式。 如果处于工作组模式，则必须选择 "不要求进行身份验证"。 如果处于域模式下，则必须选择 "需要相互身份验证"。

  ![启用 XA 事务](media/4f3695e0-fb0b-4c5b-afac-75f8860d2bb0.jpg "4f3695e0-fb0b-4c5b-afac-75f8860d2bb0")

- 请确保 MSDTC 位于**Internet 连接防火墙**设置中的例外列表中。

- 确保你使用的是 Windows Vista。 Windows Vista 上的 MSMQ 支持远程事务处理读取。 早期的 Windows 版本上的 MSMQ 不支持远程事务处理读取。

**问：** 如果从队列中读取的服务是一个网络服务，例如，在 Web 主机上，从队列中读取时，为什么会出现 "拒绝访问" 异常？

**答：** 必须将网络服务读取访问权限添加到队列 ACL，以确保网络服务可以从队列中读取。

**问：** 是否可以使用 MSMQ 激活服务根据远程计算机上的队列中的消息激活应用程序？

**答:** 是的。 为此，您必须将 MSMQ 激活服务配置为以网络服务方式运行，并将网络服务访问权限添加到远程计算机上的队列中。

## <a name="using-custom-msmq-bindings-with-receivecontext-enabled"></a>使用已启用 ReceiveContext 的自定义 MSMQ 绑定

将自定义 MSMQ 绑定用于 <xref:System.ServiceModel.Channels.ReceiveContext> 启用后，处理传入消息将使用线程池线程，因为本机 MSMQ 不支持异步接收的 i/o 完成 <xref:System.ServiceModel.Channels.ReceiveContext> 。 这是因为处理此类消息对使用内部事务 <xref:System.ServiceModel.Channels.ReceiveContext> ，而 MSMQ 不支持异步处理。 若要解决此问题，可将添加 <xref:System.ServiceModel.Description.SynchronousReceiveBehavior> 到终结点，以强制执行同步处理或将其设置 <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior.MaxPendingReceives%2A> 为1。
