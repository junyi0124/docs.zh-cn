---
title: 无服务器应用的示例业务方案和用例
description: 通过访问从映像处理到移动支持和 ETL 管道的示例，亲自了解无服务器操作。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 04/17/2020
ms.openlocfilehash: df76b132579eb3a6d05ce38c94cb9fceb9281aef
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91171606"
---
# <a name="serverless-business-scenarios-and-use-cases"></a>无服务器业务方案和用例

无服务器应用程序有许多用例和方案。 本章包含演示不同方案的示例。 这些方案包括指向相关文档和公共源代码存储库的链接。 本章中的示例使你能够开始构建和实现无服务器解决方案。

## <a name="big-data-processing"></a>大数据处理

![map/reduce 关系图](/samples/azure-samples/durablefunctions-mapreduce-dotnet/big-data-processing-serverless-mapreduce-on-azure/media/mapreducearchitecture.png)

此示例使用无服务器对大数据集执行 map/reduce 操作。 它确定 2017 年纽约黄色出租车每天行程的平均速度。

[大数据处理：Azure 上的无服务器 MapReduce](/samples/azure-samples/durablefunctions-mapreduce-dotnet/big-data-processing-serverless-mapreduce-on-azure/)

## <a name="create-serverless-applications-hands-on-lab"></a>创建无服务器应用程序：动手实验室

了解如何使用函数执行服务器端逻辑并构建无服务器体系结构。

- 为你的业务选择最佳 Azure 服务
- 创建 Azure Functions
- 使用触发器
- 链接函数
- 长时间运行的工作流
- 监视
- 开发、测试和部署

[创建无服务器应用程序](/learn/paths/create-serverless-applications/)

## <a name="customer-reviews"></a>客户评审

此示例展示了 Visual Studio 中 C# 类库的新 Azure Functions 工具。 创建一个网站，客户可以在其中提交存储在 Azure 存储 blob 和 CosmosDB 中的产品评论。 添加 Azure Function，以使用 Azure 认知服务执行客户评审的自动审核。 使用 Azure 存储队列将网站与函数分离。

[客户通过认知服务评审应用](/samples/azure-samples/functions-customer-reviews/customer-reviews-cognitive-services/)

## <a name="docker-linux-image-support"></a>Docker Linux 映像支持

此示例演示如何创建 `Dockerfile` 以在 Linux Docker 容器上生成和运行 Azure Functions。

[Linux 上的 Azure Functions](/samples/azure-samples/functions-linux-custom-image/azure-functions-on-linux-custom-image-tutorial-sample-project/)

## <a name="file-processing-and-validation"></a>文件处理和验证

此示例分析来自假设客户的一组 CSV 文件。 它确保客户“批处理”所需的所有文件都已准备就绪，然后验证每个文件的结构。 使用 Azure Functions、逻辑应用和 Durable Functions 将提供不同的解决方案。

[使用 Azure Functions、逻辑应用和 Durable Functions 进行文件处理和验证](/samples/azure-samples/serverless-file-validation/file-processing-and-validation-using-azure-functions-logic-apps-and-durable-functions/)

## <a name="game-data-visualization"></a>游戏数据可视化

![游戏遥测](/samples/azure-samples/gaming-in-editor-telemetry/in-editor-telemetry-visualization/media/points.png)

提供了一个示例，演示了开发人员如何为其游戏实现编辑器内数据可视化解决方案。 事实上，Unreal Engine 4 插件和 Unity 插件是使用此示例作为其后端开发的。 服务组件与游戏引擎无关。

[编辑器内游戏遥测可视化](/samples/azure-samples/gaming-in-editor-telemetry/in-editor-telemetry-visualization/)

## <a name="graphql"></a>GraphQL

创建公开 GraphQL API 的无服务器函数。

[GraphQL 的无服务器函数](https://github.com/softchris/graphql-workshop-dotnet/blob/master/docs/workshop/4.md)

## <a name="internet-of-things-iot-reliable-edge-relay"></a>物联网 (IoT) 可靠边缘中继

![IoT 体系结构](/samples/azure-samples/iot-reliable-edge-relay/iot-reliable-edge-relay/media/architecture.png)

此示例实现新的通信协议，以便从 IoT 设备启用可靠的上游通信。 它自动执行数据间隙检测和回填。

[IoT 可靠边缘中继](/samples/azure-samples/iot-reliable-edge-relay/iot-reliable-edge-relay/)

## <a name="microservices-reference-architecture"></a>微服务参考体系结构

![参考体系结构](/samples/azure-samples/serverless-microservices-reference-architecture/serverless-microservices-reference-architecture/media/macro-architecture.png)

此参考体系结构指导你完成设计、开发和交付 Rideshare by Relecloud 应用程序（虚构公司）所涉及的决策过程。 它包含有关配置和部署体系结构的所有组件的实际操作说明。

[无服务器微服务参考体系结构](/samples/azure-samples/serverless-microservices-reference-architecture/serverless-microservices-reference-architecture/)

## <a name="migrate-console-apps-to-serverless"></a>将控制台应用迁移到无服务器

此示例是一个泛型函数（`.csx` 文件），可用于将任何控制台应用程序转换为 Azure Functions 中的 HTTP Web 服务。 只需编辑配置文件，并指定哪些输入参数将作为自变量传递到 `.exe`。

[在 Azure Functions 上运行控制台应用](/samples/azure-samples/functions-dotnet-migrating-console-apps/run-console-apps-on-azure-functions/)

## <a name="serverless-for-mobile"></a>无服务器移动设备

Azure Functions 易于实现和维护，并可通过 HTTP 访问。 它们是实现移动应用程序 API 的好方法。 Microsoft 通过 Xamarin 提供适用于 iOS、Android 和 Windows 的优质跨平台工具。 因此，Xamarin 和 Azure Functions 可以很好地协同工作。 本文介绍了如何在 Azure 门户或 Visual Studio 中首先实现 Azure Function，并使用在 Android、iOS 和 Windows 上运行的 Xamarin.Forms 构建跨平台客户端。

[通过 Xamarin.Forms 客户端执行单个 Azure 函数](/samples/azure-samples/functions-xamarin-getting-started/implementing-a-simple-azure-function-with-a-xamarinforms-client/)

## <a name="serverless-messaging"></a>无服务器消息

此示例演示如何利用 Durable Functions 的扇出模式跨任意数量的会话/分区加载任意数量的消息。 它针对服务总线、事件中心或存储队列。 该示例还添加了使用其他 Azure Function 来使用这些消息的功能，并将生成的计时数据加载到另一个事件中心。 然后，数据引入到 Azure 数据资源管理器等分析服务中。

[使用 Azure Functions 通过服务总线、事件中心和存储队列生成和使用消息](/samples/azure-samples/durable-functions-producer-consumer/product-consume-messages-az-functions/)

## <a name="recommended-resources"></a>推荐的资源

- [Linux 上的 Azure Functions](/samples/azure-samples/functions-linux-custom-image/azure-functions-on-linux-custom-image-tutorial-sample-project/)
- [大数据处理：Azure 上的无服务器 MapReduce](/samples/azure-samples/durablefunctions-mapreduce-dotnet/big-data-processing-serverless-mapreduce-on-azure/)
- [创建无服务器应用程序](/learn/paths/create-serverless-applications/)
- [客户通过认知服务评审应用](/samples/azure-samples/functions-customer-reviews/customer-reviews-cognitive-services/)
- [使用 Azure Functions、逻辑应用和 Durable Functions 进行文件处理和验证](/samples/azure-samples/serverless-file-validation/file-processing-and-validation-using-azure-functions-logic-apps-and-durable-functions/)
- [通过 Xamarin.Forms 客户端执行单个 Azure 函数](/samples/azure-samples/functions-xamarin-getting-started/implementing-a-simple-azure-function-with-a-xamarinforms-client/)
- [编辑器内游戏遥测可视化](/samples/azure-samples/gaming-in-editor-telemetry/in-editor-telemetry-visualization/)
- [IoT 可靠边缘中继](/samples/azure-samples/iot-reliable-edge-relay/iot-reliable-edge-relay/)
- [使用 Azure Functions 通过服务总线、事件中心和存储队列生成和使用消息](/samples/azure-samples/durable-functions-producer-consumer/product-consume-messages-az-functions/)
- [在 Azure Functions 上运行控制台应用](/samples/azure-samples/functions-dotnet-migrating-console-apps/run-console-apps-on-azure-functions/)
- [GraphQL 的无服务器函数](https://github.com/softchris/graphql-workshop-dotnet/blob/master/docs/workshop/4.md)
- [无服务器微服务参考体系结构](/samples/azure-samples/serverless-microservices-reference-architecture/serverless-microservices-reference-architecture/)

>[!div class="step-by-step"]
>[上一页](orchestration-patterns.md)
>[下一页](serverless-conclusion.md)
