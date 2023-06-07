---
title: 消息传递活动
ms.date: 03/30/2017
ms.assetid: 8498f215-1823-4aba-a6e1-391407f8c273
ms.openlocfilehash: 69a0e9a415b10d9c58d04eac27e48b1ed6a78064
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576390"
---
# <a name="messaging-activities"></a>消息传递活动

消息传递活动使工作流能够发送和接收 WCF 消息。 通过将消息传递活动添加到工作流，您可以对任意复杂的消息交换模式 (MEP) 进行建模。

## <a name="message-exchange-patterns"></a>消息交换模式

有以下三种基本消息交换模式：

- **数据报**-当使用数据报 MEP 时，客户端将向服务发送一条消息，但服务不响应。 有时称为“发后不理”(fire and forget)。 “发后不理”交换形式是一种要求带外确认成功传递的交换形式。 消息在传输过程中可能会丢失，而永远不能到达服务。 即使客户端成功发送消息，也并不保证服务已经收到消息。 数据报是消息传递的基本构造块，因为您可以基于数据报构建自己的 MEP。

- **请求-响应**-当使用请求-响应 MEP 时，客户端将向服务发送一条消息，服务将执行所需的处理，然后将响应发送回客户端。 此模式由请求-响应对组成。 请求-响应调用的示例包括远程过程调用 (RPC) 和浏览器的 GET 请求。 此模式也称为半双工。

- **双工**-使用双工 MEP 时，客户端和服务可以按任意顺序发送消息。 双工 MEP 就像电话通话，所说的每一个字都是一条消息。

使用消息传递活动能够实现这些基本 MEP 中的任何一个以及任意复杂的 MEP。

## <a name="messaging-activities"></a>消息传递活动

[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]定义了下面的消息传递活动：

- <xref:System.ServiceModel.Activities.Send> - 使用 <xref:System.ServiceModel.Activities.Send> 活动可发送消息。

- <xref:System.ServiceModel.Activities.SendReply> - 使用 <xref:System.ServiceModel.Activities.SendReply> 活动可发送对接收到的消息的响应。 此活动由工作流服务用来实现请求/答复 MEP。

- <xref:System.ServiceModel.Activities.Receive> - 使用 <xref:System.ServiceModel.Activities.Receive> 活动可接收消息。

- <xref:System.ServiceModel.Activities.ReceiveReply> - 使用 <xref:System.ServiceModel.Activities.ReceiveReply> 活动可接收答复消息。 此活动由工作流服务客户端用来实现请求/答复 MEP。

## <a name="messaging-activities-and-message-exchange-patterns"></a>消息传递活动和消息交换模式

数据报 MEP 涉及发送消息的客户端和接收消息的服务。 如果客户端为工作流，则使用 <xref:System.ServiceModel.Activities.Send> 活动发送消息。 若要接收工作流中的该消息，请使用 <xref:System.ServiceModel.Activities.Receive> 活动。 <xref:System.ServiceModel.Activities.Send> 和 <xref:System.ServiceModel.Activities.Receive> 活动各自具有一个名为 `Content` 的属性。 此属性包含要发送或接收的数据。 实现请求-响应 MEP 时，客户端和服务都使用活动对。 客户端使用 <xref:System.ServiceModel.Activities.Send> 活动发送消息并使用 <xref:System.ServiceModel.Activities.ReceiveReply> 活动接收来自服务的响应。 这两个活动通过 <xref:System.ServiceModel.Activities.ReceiveReply.Request%2A> 属性相互关联。 该属性设置为发送原始消息的 <xref:System.ServiceModel.Activities.Send> 活动。 服务还使用一对关联的活动：<xref:System.ServiceModel.Activities.Receive> 和 <xref:System.ServiceModel.Activities.SendReply>。 这两个活动通过 <xref:System.ServiceModel.Activities.SendReply.Request%2A> 属性关联。 该属性设置为接收原始消息的 <xref:System.ServiceModel.Activities.Receive> 活动。 <xref:System.ServiceModel.Activities.ReceiveReply> 和 <xref:System.ServiceModel.Activities.SendReply> 活动，与 <xref:System.ServiceModel.Activities.Send> 和 <xref:System.ServiceModel.Activities.Receive> 一样，可用于发送 <xref:System.ServiceModel.Channels.Message> 实例或消息协定类型。

由于工作流具有长时间运行的性质，因此同时支持长时间运行的对话对于双工通信模式非常重要。 若要支持长时间运行的对话，启动对话的客户端必须提供适当的机会，使服务在以后数据变为可用时能够回拨客户端。 例如，已提交采购订单请求等待经理批准，但该请求可能一天、一周甚至一年也未被处理；管理采购订单批准的工作流必须知道在请求得到批准后恢复。 使用相关性的工作流中支持此双工通信模式。 若要实现双工模式，请使用 <xref:System.ServiceModel.Activities.Send> 和 <xref:System.ServiceModel.Activities.Receive> 活动。 在 <xref:System.ServiceModel.Activities.Receive> 活动上，使用初始化相关 <xref:System.ServiceModel.Activities.CorrelationHandle> 。 在 <xref:System.ServiceModel.Activities.Send> 活动上，将该相关性句柄设置为 <xref:System.ServiceModel.Activities.Send.CorrelatesWith%2A> 属性值。 有关详细信息，请参阅[持久双工](durable-duplex-correlation.md)。

> [!NOTE]
> 工作流使用回调相关性（"持久双工"）的双工实现适用于长时间运行的会话。 这与具有回调协定的 WCF 双工不同，在 WCF 双工中，对话是短时间运行的（通道的生存期）。

## <a name="message-formatting-and-messaging-activities"></a>消息格式和消息传递活动

<xref:System.ServiceModel.Activities.Receive> 和 <xref:System.ServiceModel.Activities.ReceiveReply> 活动具有一个名为 `Content` 的属性。 此属性的类型为 <xref:System.ServiceModel.Activities.ReceiveContent>，它表示 <xref:System.ServiceModel.Activities.Receive> 或 <xref:System.ServiceModel.Activities.ReceiveReply> 活动接收的数据。 .NET Framework 定义两个名为 <xref:System.ServiceModel.Activities.ReceiveMessageContent> 和 <xref:System.ServiceModel.Activities.ReceiveParametersContent> 的相关类，这两个类都派生自 <xref:System.ServiceModel.Activities.ReceiveContent>。 将 <xref:System.ServiceModel.Activities.Receive> 或 <xref:System.ServiceModel.Activities.ReceiveReply> 活动的 `Content` 属性设置为其中一个类型的实例，这样可将数据接收到工作流服务。 要使用的类型取决于活动接收的数据的类型。 如果活动接收到 `Message` 对象或消息协定类型，则使用 <xref:System.ServiceModel.Activities.ReceiveMessageContent>。 如果活动接收到一组数据协定或可序列化的 XML 类型，则使用 <xref:System.ServiceModel.Activities.ReceiveParametersContent>。 使用 <xref:System.ServiceModel.Activities.ReceiveParametersContent> 可以发送多个参数，而使用 <xref:System.ServiceModel.Activities.ReceiveMessageContent> 只能发送一个对象，即消息（或消息协定类型）。

> [!NOTE]
> 还可将 <xref:System.ServiceModel.Activities.ReceiveMessageContent> 与单个数据协定或可序列化的 XML 类型一起使用。 将 <xref:System.ServiceModel.Activities.ReceiveParametersContent> 和单个参数一起使用与直接将对象传递给 <xref:System.ServiceModel.Activities.ReceiveMessageContent> 之间的区别在于连网格式。 参数的内容包装在与操作名称相对应的 XML 元素中，而序列化对象则使用参数名称包装在 XML 元素中（例如 `<Echo><msg>Hello, World</msg></Echo>`）。 消息内容不由操作名称包装。 相反，序列化对象则使用 XML 限定类型名称放置在 XML 元素中（例如 `<string>Hello, World</string>`）。

<xref:System.ServiceModel.Activities.Send> 和 <xref:System.ServiceModel.Activities.SendReply> 活动还具有一个名为 `Content` 的属性。 此属性的类型为 <xref:System.ServiceModel.Activities.SendContent>，它表示 <xref:System.ServiceModel.Activities.Send> 或 <xref:System.ServiceModel.Activities.SendReply> 活动发送的数据。 .NET Framework 定义两个名为 <xref:System.ServiceModel.Activities.SendMessageContent> 和 <xref:System.ServiceModel.Activities.SendParametersContent> 的相关类型，这两个类型都派生自 <xref:System.ServiceModel.Activities.SendContent>。 将 <xref:System.ServiceModel.Activities.Send> 或 <xref:System.ServiceModel.Activities.SendReply> 活动的 `Content` 属性设置为其中一个类型的实例，以便从工作流服务发送数据。 要使用的类型取决于活动发送的数据的类型。 如果活动发送 `Message` 对象或消息协定类型，则使用 <xref:System.ServiceModel.Activities.SendMessageContent>。 如果活动发送数据协定类型，则使用 <xref:System.ServiceModel.Activities.SendParametersContent>。 使用<xref:System.ServiceModel.Activities.SendParametersContent> 可以发送多个参数，而使用 <xref:System.ServiceModel.Activities.SendMessageContent> 只能发送一个对象，即消息（或消息协定类型）。

以命令方式对消息传递活动进行编程时，使用泛型 <xref:System.Activities.InArgument%601> 和 <xref:System.Activities.OutArgument%601> 包装分配给消息的对象或者包装 <xref:System.ServiceModel.Activities.Send>、<xref:System.ServiceModel.Activities.SendReply>、<xref:System.ServiceModel.Activities.Receive> 和 <xref:System.ServiceModel.Activities.ReceiveReply> 活动的参数属性。 用于 <xref:System.Activities.InArgument%601> <xref:System.ServiceModel.Activities.Send> 和 <xref:System.ServiceModel.Activities.SendReply> 活动，以及 <xref:System.Activities.OutArgument%601> <xref:System.ServiceModel.Activities.Receive> 和 <xref:System.ServiceModel.Activities.ReceiveReply> 活动。 `In` 自变量用于发送活动，因为数据将传入到活动。 `Out` 参数用于接收活动，因为数据将传出到活动之外，如下面的示例所示。

```csharp
Receive reserveSeat = new Receive
{
    ...
    Content = new ReceiveParametersContent
    {
        Parameters =
        {
            { "ReservationInfo", new OutArgument<ReservationRequest>(reservationInfo) }
        }
    }
};
SendReply reserveSeat = new SendReply
{
    ...
    Request = reserveSeat,
    Content = new SendParametersContent
    {
        Parameters =
        {
            { "ReservationId", new InArgument<string>(reservationId) }
        }
    },
};
```

如果要实现的工作流服务定义了返回 void 的请求/响应操作，则必须实例化 <xref:System.ServiceModel.Activities.SendReply> 活动，并将 <xref:System.ServiceModel.Activities.SendReply.Content%2A> 属性设置为某个内容类型（<xref:System.ServiceModel.Activities.SendMessageContent> 或 <xref:System.ServiceModel.Activities.SendParametersContent>）的空实例，如下面的示例所示。

```csharp
Receive rcv = new Receive()
{
ServiceContractName = "IService",
OperationName = "NullReturningContract",
Content = new ReceiveParametersContent( new Dictionary<string, OutArgument>() { { "message", new OutArgument<string>() } } )
};
SendReply sr = new SendReply()
{
Request = rcv
   Content = new SendParametersContent();
};
```

## <a name="add-service-reference"></a>添加服务引用

从工作流应用程序调用工作流服务时，Visual Studio 2012 会生成自定义消息传递活动，这些活动封装 <xref:System.ServiceModel.Activities.Send> <xref:System.ServiceModel.Activities.ReceiveReply> 请求/答复 MEP 中常用的和活动。 若要使用此功能，请右键单击 Visual Studio 中的客户端项目，然后选择 "**添加**  >  **服务引用**"。 在地址框中键入服务的基址，然后单击“转到”。 可用服务显示在 "**服务：** " 框中。 展开服务节点以显示支持的协定。 选择要调用的协定，"**操作**" 框中会显示可用操作的列表。 然后，您可以指定生成的活动的命名空间，然后单击 **"确定"**。 随即显示一个对话框，指出操作已成功完成，并且在您重新生成项目后，所生成的自定义活动都位于工具箱中。 在服务协定上定义的每个操作都有一个对应的活动。 重新生成项目后，可以将自定义活动拖放到工作流上，并在属性窗口中设置任何所需的属性。

## <a name="messaging-activity-templates"></a>消息传递活动模板

为了更轻松地在客户端和服务上设置请求/响应 MEP，Visual Studio 2012 提供了两个消息传递活动模板。 `System.ServiceModel.Activities.Design.ReceiveAndSendReply` 在服务上使用，`System.ServiceModel.Activities.Design.SendAndReceiveReply` 在客户端上使用。 在两种情况下，模板都会将适当的消息传递活动添加到工作流中。 在服务上，`System.ServiceModel.Activities.Design.ReceiveAndSendReply` 先添加 <xref:System.ServiceModel.Activities.Receive> 活动，再添加 <xref:System.ServiceModel.Activities.SendReply> 活动。 <xref:System.ServiceModel.Activities.SendReply.Request> 属性自动设置为 <xref:System.ServiceModel.Activities.Receive> 活动。 在客户端上，`System.ServiceModel.Activities.Design.SendAndReceiveReply` 先添加 <xref:System.ServiceModel.Activities.Send> 活动，再添加 <xref:System.ServiceModel.Activities.ReceiveReply> 活动。 <xref:System.ServiceModel.Activities.ReceiveReply.Request%2A> 属性自动设置为 <xref:System.ServiceModel.Activities.Send> 活动。 若要使用这些模板，只需将适当的模板拖放到工作流上即可。

## <a name="messaging-activities-and-transactions"></a>消息传递活动和事务

调用工作流服务时，你可能希望将事务流动到服务操作中。 为此，请将 <xref:System.ServiceModel.Activities.Receive> 活动放置到 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 活动中。 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 活动包含 `Receive` 活动和主体。 流向服务的事务在执行 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 的主体的整个过程中保持为环境事务。 事务在执行完主体后完成。 有关工作流和事务的详细信息，请参阅[工作流事务](../../windows-workflow-foundation/workflow-transactions.md)。

## <a name="see-also"></a>另请参阅

- [如何在工作流服务中发送和接收错误](https://go.microsoft.com/fwlink/?LinkId=189151)
- [创建长时间运行的工作流服务](creating-a-long-running-workflow-service.md)
