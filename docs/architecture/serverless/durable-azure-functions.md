---
title: 持久 Azure Functions - 无服务器应用
description: 持久 Azure Functions 将扩展 Azure Functions 运行时，以便在代码中启用有状态的工作流。
author: cecilphillip
ms.author: cephilli
ms.date: 06/26/2018
ms.openlocfilehash: c3ee628b5c2239cd13395fda7714b38b06efa058
ms.sourcegitcommit: f99115e12a5eb75638abe45072e023a3ce3351ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94557150"
---
# <a name="durable-azure-functions"></a>持久 Azure Functions

使用 Azure Functions 创建无服务器应用程序时，操作通常会设计为以无状态方式运行。 选择此设计是因为，随着平台规模的不断增加，很难知道代码运行在哪些服务器上。 也很难知道任意给定点上有多少实例处于活动状态。 但是，有些应用程序类需要知道进程的当前状态。 考虑向在线商店提交订单的过程。 签出操作可能是由多个需要知道进程状态的操作组成的工作流。 如果客户在其帐户上有任何信用额度，以及处理信用卡的结果，则此类信息可能包括产品清单。 这些操作可以很容易地成为它们自己的内部工作流，甚至是来自第三方系统的服务。

目前存在多种模式，这些模式可帮助协调内部和外部系统之间的应用程序状态。 这是一种很常见的解决方案，这些解决方案依赖于集中式队列系统、分布式键值存储或共享数据库来管理此状态。 不过，这些是现在需要设置和管理的所有其他资源。 在无服务器环境中，尝试手动协调这些资源可能会变得非常麻烦。 Azure Functions 提供了一种替代方法，用于创建名为 Durable Functions 的有状态函数。

Durable Functions 是 Azure Functions 运行时的扩展，可在代码中启用有状态工作流的定义。 通过将工作流分解为活动，Durable Functions 扩展可以管理状态、创建进度检查点，并处理跨服务器的函数调用分布。 在后台，它利用 Azure 存储帐户来持久保存执行历史记录、计划活动函数和检索响应。 无服务器代码决不应与此存储帐户中的持久信息进行交互，通常也无需与开发人员进行交互。

## <a name="triggering-a-stateful-workflow"></a>触发有状态工作流

Durable Functions 中的有状态工作流可以分解为两个内部组件；业务流程和活动触发器。 触发器和绑定是 Azure Functions 用来让你的无服务器函数在启动、接收输入和返回结果时获得通知的核心组件。

### <a name="working-with-the-orchestration-client"></a>使用业务流程客户端

与 Azure Functions 中的触发操作的其他样式相比，业务流程是唯一的。 Durable Functions 允许执行可能需要数小时甚至数天才能完成的函数。 这种类型的行为需要能够检查正在运行的业务流程的状态，提前终止或发送外部事件的通知。

对于这种情况，Durable Functions 扩展提供了 `DurableOrchestrationClient` 类，可用于与已协调的函数进行交互。 你可以使用 `OrchestrationClientAttribute` 绑定来访问业务流程客户端。 通常，会将此属性与另一种触发器类型（如 `HttpTrigger` 或 `ServiceBusTrigger`）包括在内。 一旦触发了源函数，就可以使用业务流程客户端启动业务流程函数。

```csharp
[FunctionName("KickOff")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "POST")]HttpRequestMessage req,
    [OrchestrationClient ] DurableOrchestrationClient<orchestrationClient>)
{
    OrderRequestData data = await req.Content.ReadAsAsync<OrderRequestData>();

    string instanceId = await orchestrationClient.StartNewAsync("PlaceOrder", data);

    return orchestrationClient.CreateCheckStatusResponse(req, instanceId);
}
```

### <a name="the-orchestrator-function"></a>业务流程协调程序函数

在 Azure Functions 中使用 OrchestrationTriggerAttribute 注释函数，将其用作业务流程协调程序函数。 负责管理构成有状态工作流的各种活动。

业务流程协调程序函数无法使用 OrchestrationTriggerAttribute 以外的绑定。 此属性只能与 DurableOrchestrationContext 参数类型一起使用。 由于不支持反序列化函数签名中的输入，因此不能使用其他任何输入。 若要获取业务流程客户端提供的输入，必须使用 GetInput\<T\> 方法。

此外，业务流程函数的返回类型必须是 void、Task 或 JSON 可序列化值。

> *为简洁起见，已省略错误处理代码*

```csharp
[FunctionName("PlaceOrder")]
public static async Task<string> PlaceOrder([OrchestrationTrigger] DurableOrchestrationContext context)
{
    OrderRequestData orderData = context.GetInput<OrderRequestData>();

    await context.CallActivityAsync<bool>("CheckAndReserveInventory", orderData);
    await context.CallActivityAsync<string>("ProcessPayment", orderData);

    string trackingNumber = await context.CallActivityAsync<string>("ScheduleShipping", orderData);
    await context.CallActivityAsync<string>("EmailCustomer", trackingNumber);

    return trackingNumber;
}
```

可以同时启动并运行多个业务流程实例。 对 `DurableOrchestrationClient` 调用 `StartNewAsync` 方法将启动业务流程的新实例。 方法返回在业务流程启动时完成的 `Task<string>`。 如果业务流程未在 30 秒内启动，则会引发 `TimeoutException` 类型的异常。

从 `StartNewAsync` 完成的 `Task<string>` 应包含业务流程实例的唯一 ID。 此实例 ID 可用于在该特定业务流程上的调用操作。 可以查询业务流程以获取状态或发送事件通知。

### <a name="the-activity-functions"></a>活动函数

活动函数是可在业务流程函数内组合在一起以创建工作流的离散操作。 大多数情况下，大多数实际工作都是在这里进行。 它们表示业务逻辑，长时间运行的进程，以及更大的解决方案中的难题。

`ActivityTriggerAttribute` 可用于注释 `DurableActivityContext` 类型的函数参数。 使用注释通知运行时，此函数将计划用作活动函数。 使用 `DurableActivityContext` 参数的 `GetInput<T>` 方法检索活动函数的输入值。

与业务流程函数类似，活动的返回类型必须是 void、Task 或 JSON 可序列化值。

活动函数中引发的任何未经处理的异常都将发送到调用业务流程协调程序函数并显示为 `TaskFailedException`。 此时，可以在业务流程协调程序中捕获并记录错误，并且可以重试此活动。

```csharp
[FunctionName("CheckAndReserveInventory")]
public static bool CheckAndReserveInventory([ActivityTrigger] DurableActivityContext context)
{
    OrderRequestData orderData = context.GetInput<OrderRequestData>();

    // Connect to inventory system and try to reserve items
    return true;
}
```

## <a name="recommended-resources"></a>推荐的资源

- [Durable Functions](/azure/azure-functions/durable-functions-overview)
- [Durable Functions 的绑定](/azure/azure-functions/durable-functions-bindings)
- [在 Durable Functions 中管理实例](/azure/azure-functions/durable-functions-instance-management)

>[!div class="step-by-step"]
>[上一页](event-grid.md)
>[下一页](orchestration-patterns.md)
