---
title: 利用无服务器函数
description: 在云本机应用程序中利用无服务器和 Azure Functions
ms.date: 05/13/2020
ms.openlocfilehash: 8e5c60d29cd8d635f79f42c232b33f060949e2b5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155362"
---
# <a name="leveraging-serverless-functions"></a>利用无服务器函数

在极大范围内，从管理物理计算机到利用云功能，无服务器生活在极端的位置。 你唯一的责任就是你的代码，你只需在代码运行时付费。 Azure Functions 提供了一种将无服务器功能构建到你的云本机应用程序中的方法。

## <a name="what-is-serverless"></a>什么是无服务器？

无服务器是云计算的全新服务模型。 这并不意味着服务器是可选的，你的代码仍会在某个服务器上的某个位置运行。 不同之处在于，应用程序团队不再关心管理服务器基础结构。 相反，云供应商拥有此责任。 开发团队通过向客户提供业务解决方案而不是管道提高其工作效率。

无服务器计算使用事件触发的无状态容器来托管你的服务。 他们可以根据需要向外扩展和以满足需求。 Azure Functions 的无服务器平台与其他 Azure 服务（如队列、事件和存储）紧密集成。

## <a name="what-challenges-are-solved-by-serverless"></a>无服务器解决了哪些问题？

无服务器平台解决了许多耗时且昂贵的问题：

- 购买计算机和软件许可证
- 承载、保护、配置和维护计算机及其网络、电源和/C 要求
- 修补和升级操作系统和软件
- 将 web 服务器或计算机服务配置为托管应用程序软件
- 在其平台中配置应用程序软件

许多公司分配了大量预算，以支持硬件基础结构问题。 迁移到云有助于降低这些成本;将应用程序切换到无服务器可以帮助消除它们。

## <a name="what-is-the-difference-between-a-microservice-and-a-serverless-function"></a>微服务和无服务器函数之间的区别是什么？

通常，微服务封装企业功能，例如在线电子商务网站的购物车。 它公开了多个操作，使用户能够管理其购物体验。 然而，函数是一种小型的轻型代码块，它执行单一用途操作来响应事件。
通常构造微服务来响应请求，通常来自接口。 请求可以是 HTTP Rest 或基于 gRPC 的。 无服务器服务响应事件。 其事件驱动的体系结构非常适合用于处理短时间运行的后台任务。

## <a name="what-scenarios-are-appropriate-for-serverless"></a>哪些方案适用于无服务器？

无服务器显示为响应触发器而调用的各个短时间运行的函数。 这使它们非常适合用于处理后台任务。

应用程序可能需要在工作流中作为一个步骤发送电子邮件。 不要以微服务请求的形式发送通知，而是将消息详细信息放到队列中。 Azure 函数可以取消对消息的排队并以异步方式发送电子邮件。 这样做可以提高微服务的性能和可伸缩性。 可以实施[基于队列的负载调节](/azure/architecture/patterns/queue-based-load-leveling)，以避免与发送电子邮件相关的瓶颈。 此外，可以在多个不同的应用程序中将此独立服务作为一个实用工具重复使用。

来自队列和主题的异步消息传送是触发无服务器函数的一种常见模式。 但是，可以由其他事件（如对 Azure Blob 存储的更改）触发 Azure Functions。 支持图像上传的服务可以有一个负责优化映像大小的 Azure 函数。 此函数可通过插入到 Azure Blob 存储中直接触发，使微服务操作的复杂性保持不变。

许多服务具有长时间运行的进程，作为其工作流的一部分。 通常，这些任务在用户与应用程序交互的过程中完成。 这些任务可强制用户等待，对其体验产生负面影响。 无服务器计算提供在用户交互循环之外移动较慢任务的一种绝佳方式。 这些任务可以按需求进行缩放，而无需整个应用程序进行缩放。

## <a name="when-should-you-avoid-serverless"></a>何时应避免无服务器服务器？

无服务器解决方案预配，按需缩放。 调用新的实例时，冷启动是一个常见问题。 冷启动是设置此实例所用的时间段。 通常，这种延迟可能会有几秒钟，但可能会更长，具体取决于不同的因素。 预配后，只要单个实例接收到定期请求，就会保持活动状态。 但是，如果调用的服务不太频繁，Azure 可能会将其从内存中删除，并在 reinvoked 时需要冷启动。 当函数扩展到新实例时，还需要冷启动。

图3-10 显示了冷启动模式。 请注意应用程序冷时所需的额外步骤。

![冷启动 ](./media/cold-start-warm-start.png)
 **图 3-10**。 冷启动和热启动。

为了避免冷启动，可能需要从 [消耗计划切换到专用计划](https://azure.microsoft.com/blog/understanding-serverless-cold-start/)。 你还可以配置一个或多个具有高级计划升级的 [预准备好实例](/azure/azure-functions/functions-premium-plan#pre-warmed-instances) 。 在这些情况下，当你需要添加另一个实例时，该实例已准备就绪，可以继续。 这些选项可帮助减少与无服务器计算相关的冷启动问题。

云提供程序根据计算执行时间和使用的内存计费无服务器。 长时间运行的操作或高内存消耗工作负荷并非始终是无服务器的最佳候选项。 无服务器函数适用于可快速完成的小型工作块。 大多数无服务器平台需要在几分钟内完成单个功能。 Azure Functions 默认为5分钟的超时持续时间，最多可以配置10分钟。 Azure Functions 高级计划还可以减少此问题，并将超时设置为30分钟，且可配置的限制更多。 计算时间不是日历时间。 使用 [Azure Durable Functions framework](/azure/azure-functions/durable/durable-functions-overview?tabs=csharp) 的更高级功能可能会在几天内暂停执行。 根据实际执行时间计费-当函数唤醒并继续处理时。

最后，利用应用程序任务 Azure Functions 增加了复杂性。 首先，将应用程序设计为模块化、松散耦合的设计是明智的。 然后，确定是否有无服务器可提供的权益来论证额外的复杂性。

>[!div class="step-by-step"]
>[上一页](leverage-containers-orchestrators.md)
>[下一页](combine-containers-serverless-approaches.md)
