---
title: Azure Functions - 无服务器应用
description: Azure 函数跨多种语言（C#、JavaScript、Java）和平台提供无服务器功能，以提供事件驱动的即时缩放代码。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 04/06/2020
ms.openlocfilehash: 08e1aaecdee753dc25cca0d6356caaafae1ad510
ms.sourcegitcommit: f99115e12a5eb75638abe45072e023a3ce3351ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94557111"
---
# <a name="azure-functions"></a>Azure Functions

Azure Functions 提供无服务器计算体验。 函数由触发器  （如对 HTTP 终结点或计时器的访问权限）调用，并执行代码块或业务逻辑。 函数还支持连接到存储和队列等资源的专门绑定  。

![Azure Functions 徽标](./media/azure-functions-logo.png)

当前运行时版本 3.0 支持跨平台 .NET Core 3.1 应用程序。 支持除 C# 之外的其他语言（如 JavaScript、F#和 Java）。 在门户中创建的函数提供丰富的脚本编写语法。 可以通过完全的平台支持和功能来部署作为独立项目创建的函数。

有关详细信息，请参阅 [Azure Functions 文档](/azure/azure-functions)。

## <a name="programming-language-support"></a>编程语言支持

正式版 (GA) 支持以下语言。

|语言      |支持的运行时|
|--------------|------------------|
|**C#**        |.NET Core 3.1     |
|**JavaScript**|Node 10 和 12      |
|**F#**        |.NET Core 3.1     |
|**Java**      |Java 8            |
|**Python**    |Python 3.6、3.7 和 3.8|
|**TypeScript**|Node 10 和 12（通过 JavaScript）|
|**PowerShell**|PowerShell Core 6|

有关详细信息，请参阅[支持的语言](/azure/azure-functions/supported-languages)。

## <a name="app-service-plans"></a>应用服务计划

函数由应用服务计划  进行支持。 该计划定义函数应用使用的资源。 你可以将计划分配给某个区域，确定将使用的虚拟机的大小和数量，并选择定价层。 为了获取真正的无服务器方法，函数应用可以使用“消耗”  计划。 “消耗”计划将根据负载自动缩放后端。

函数应用的另一个托管选项是[高级计划](/azure/azure-functions/functions-premium-plan)。 此计划提供了一个“alwayson”实例以避免冷启动，支持 VNet 连接等高级功能，并在高级硬件上运行。

有关详细信息，请参阅[应用服务计划](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。

## <a name="create-your-first-function"></a>创建第一个函数

可以通过三种常用方法创建函数应用。

- 门户中的脚本函数。
- 使用 Azure CLI 创建必要的资源。
- 使用最喜欢的 IDE 在本地生成函数并将其发布到 Azure。

有关在门户中创建脚本函数的详细信息，请参阅[在 Azure 门户中创建第一个函数](/azure/azure-functions/functions-create-first-azure-function)。

若要从 Azure CLI 构建，请参阅[使用 Azure CLI 创建你的第一个函数](/azure/azure-functions/functions-create-first-azure-function-azure-cli)。

若要从 Visual Studio 创建函数，请参阅[使用 Visual Studio 创建你的第一个函数](/azure/azure-functions/functions-create-your-first-function-visual-studio)。

## <a name="understand-triggers-and-bindings"></a>了解触发器和绑定

函数由触发器  调用，且只能有一个触发器。 除了调用函数外，某些触发器还用作绑定。 除了触发器外，还可以定义多个绑定。 “绑定”  提供将数据连接到代码的声明性方式。 它们可以传入（输入）或接收数据（输出）。 触发器和绑定使函数易于使用。 绑定消除了手动创建数据库或文件系统连接的系统开销。 绑定所需的所有信息都包含在脚本的特殊 functions.json  文件中，或在代码中用特性声明。

一些常见触发器包括：

- Blob 存储：在存储中上载或更改文件或文件夹时调用函数。
- HTTP：调用函数，如 REST API。
- 队列：在队列中存在项时调用函数。
- 计时器：按定期节奏调用函数。

绑定示例包括：

- CosmosDB：轻松连接到数据库以加载或保存文件。
- 表存储：使用函数应用中的键/值存储。
- 队列存储：轻松从队列中检索项，或在队列中放置新项。

下面的示例 functions.json  文件定义触发器和绑定：

```json
{
  "bindings": [
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "images/{name}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "type": "queue",
      "direction": "out",
      "queueName": "images",
      "connection": "AzureWebJobsStorage"
    }
  ],
  "disabled": false
}
```

在此示例中，函数由对 `images` 容器中的 blob 存储的更改触发。 传递文件的信息，因此触发器也充当绑定。 另一个绑定用于将信息放到名为 `images` 的队列中。

下面是该函数的 C# 脚本：

```csharp
public static string Run(Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
    return name;
}
```

该示例是一个简单的函数，该函数采用已修改或已上载到 blob 存储的文件的名称，并将其放在队列中供以后处理。

有关触发器和绑定的完整列表，请参阅 [Azure Functions 触发器和绑定概念](/azure/azure-functions/functions-triggers-bindings)。

>[!div class="step-by-step"]
>[上一页](azure-serverless-platform.md)
>[下一页](application-insights.md)
