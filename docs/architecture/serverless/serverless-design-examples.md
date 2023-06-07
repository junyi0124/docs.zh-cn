---
title: 无服务器设计示例 - 无服务器应用
description: 了解无服务器体系结构支持的各种方案，从计划和基于事件的处理到文件触发器和流进程。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 3aa9b7951fd8f11a65a64c22443de7041aba7d94
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91171749"
---
# <a name="serverless-design-examples"></a>无服务器设计示例

存在许多无服务器的设计模式。 本部分捕获了一些使用无服务器的常见方案。 所有这些示例的共同点是事件触发器和业务逻辑的基本组合。

## <a name="scheduling"></a>计划

计划任务是一种常见功能。 下图显示了不具有适当完整性检查的旧数据库。 数据库必须定期进行清理。 无服务器函数查找无效数据并将其清除。 触发器是一种按计划运行代码的计时器。

![无服务器计划](./media/serverless-scheduling.png)

## <a name="command-and-query-responsibility-segregation-cqrs"></a>命令和查询责任分离 (CQRS)

命令和查询责任分离 (CQRS) 是一种为读取（或查询）数据和修改数据的操作提供不同接口的模式。 它解决了几个常见问题。 在传统的基于创建读取更新删除 (CRUD) 的系统中，对同一数据存储的大量读写操作可能会产生冲突。 锁定可能频繁发生，并大大降低读取速度。 通常，数据由多个域对象组合而成，且读取操作必须将来自不同实体的数据组合在一起。

使用 CQRS 时，读取可能涉及特殊的“平展”实体，该实体按使用方式对数据进行建模。 读取的处理方式不同于其存储方式。 例如，尽管数据库可以将联系人存储为带有子地址记录的标头记录，但读取可能涉及带有标头和地址属性的实体。 创建这种读取模型的方法有很多。 它可能是从视图中具体化来的。 更新操作可被封装为隔离的事件，然后触发对两个不同模型的更新。 存在单独的模型用于读取和写入。

![CQRS 示例](./media/cqrs-example.png)

无服务器可通过提供隔离的终结点来容纳 CQRS 模式。 一个无服务器函数容纳查询或读取，另一个无服务器函数或函数集处理更新操作。 无服务器函数还可以负责确保读取模型保持最新状态，并且可由数据库的[更改源](/azure/cosmos-db/change-feed)触发。 可简化前端开发以连接到所需的终结点。 事件处理在后端进行。 此模型还适用于大型项目，因为不同的团队可以处理不同的操作。

## <a name="event-based-processing"></a>基于事件的处理

在基于消息的系统中，通常会在队列或发布者/订阅者主题中收集事件以进行处理。 这些事件可触发无服务器函数来执行一种业务逻辑。 事件来源的系统是基于事件的处理的一个示例。 引发“事件”以将任务标记为完成。 事件触发的无服务器函数会更新相应的数据库文档。 第二个无服务器函数可能会使用事件来更新系统的读取模型。 [Azure 事件网格](/azure/event-grid/overview)提供了一种将事件与作为订阅者的函数集成的方法。

> 事件是信息性消息。 有关详细信息，请参阅[事件溯源模式](/azure/architecture/patterns/event-sourcing)。

## <a name="file-triggers-and-transformations"></a>文件触发器和转换

提取、转换和加载 (ETL) 是一种常见的业务功能。 无服务器是优秀的 ETL 解决方案，因为它允许将代码作为管道的一部分触发。 单个代码组件可以解决各个方面的问题。 一个无服务器函数可以下载该文件，另一个应用转换，再一个加载数据。 可以单独测试和部署代码，使其更易于按需维护和缩放。

![无服务器文件触发器和转换](./media/serverless-file-triggers.png)

在关系图中，“冷存储”提供在 [Azure 流分析](/azure/stream-analytics)中分析的数据。 在数据流中遇到的任何问题都会触发 Azure 函数来解决异常。

## <a name="asynchronous-background-processing-and-messaging"></a>异步后台处理和消息传递

有了异步消息传递和后台处理，应用程序无需等待即可启动进程。 OCR 应用是异步处理的一个示例。 提交图像，然后排队等候处理。 扫描图像以提取文本可能需要一些时间，完成后，将发送通知。 在此方案中，无服务器可以处理调用和结果。

## <a name="web-apps-and-apis"></a>Web 应用和 API

无服务器的常见方案是 N 层应用程序，在最常见方案中，UI 层为 Web 应用。 最热门的单页面应用程序 (SPA) 最近有激增的现象。 SPA 应用呈现单个页面，然后依赖 API 调用和返回的数据动态呈现新 UI，而无需重新加载整个页面。 客户端呈现为最终用户提供了一个速度更快、响应度更佳的应用程序。

HTTP 调用触发的无服务器终结点可用于处理 API 请求。 例如，广告服务公司可使用用户配置文件信息调用无服务器函数来请求自定义广告。 无服务器函数返回自定义广告，并由网页呈现。

![无服务器 Web API](./media/serverless-web-api.png)

## <a name="data-pipeline"></a>数据管道

无服务器函数可用于简化数据管道。 在此示例中，文件触发了一个函数，用于将 CSV 文件中的数据转换为表中的数据行。 组织表允许 Power BI 仪表板向最终用户呈现分析。

![无服务器数据管道](./media/serverless-data-pipeline.png)

## <a name="stream-processing"></a>流处理

设备和传感器通常会生成必须实时处理的数据流。 有许多技术可以捕获从[事件中心](/azure/event-hubs/event-hubs-what-is-event-hubs)和 [IoT 中心](/azure/iot-hub)到[服务总线](/azure/service-bus)的消息和流。 无论如何传输，无服务器都是一种处理传入消息和数据流的理想机制。 无服务器可快速缩放，以满足大量数据的需求。 无服务器代码可以应用业务逻辑来分析数据，并输出为机构化格式以执行操作和分析。

![无服务器流处理](./media/serverless-stream-processing.png)

## <a name="api-gateway"></a>API 网关

API 网关为客户端提供单一入口点，然后将请求智能地路由到后端服务。 管理大型服务集很有用。 它还可以通过轻松地将客户端连接到不同的环境来处理版本控制和简化开发。 无服务器可处理单个微服务的后端缩放，同时通过 API 网关提供单个前端。

![无服务器 API 网关](./media/serverless-api-gateway.png)

## <a name="recommended-resources"></a>推荐的资源

- [Azure 事件网格](/azure/event-grid/overview)
- [Azure IoT 中心](/azure/iot-hub)
- [分布式数据管理挑战和解决方案](../microservices/architect-microservice-container-applications/distributed-data-management.md)
- [设计微服务：识别微服务边界](/azure/architecture/microservices/microservice-boundaries)
- [事件中心](/azure/event-hubs/event-hubs-what-is-event-hubs)
- [事件溯源模式](/azure/architecture/patterns/event-sourcing)
- [实现断路器模式](../microservices/implement-resilient-applications/implement-circuit-breaker-pattern.md)
- [IoT 中心](/azure/iot-hub)
- [服务总线](/azure/service-bus)
- [使用 Azure Cosmos DB 中的更改源支持](/azure/cosmos-db/change-feed)

>[!div class="step-by-step"]
>[上一页](serverless-architecture-considerations.md)
>[下一页](azure-serverless-platform.md)
