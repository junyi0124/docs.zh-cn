---
title: Azure 逻辑应用 - 无服务器应用
description: 使用 Azure 逻辑应用可以生成可缩放的自动化工作流，以便跨云服务和本地系统集成应用和数据。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 11fdf5b5f176eb0d66eee6dde7638d3eae1e1f55
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91171840"
---
# <a name="azure-logic-apps"></a>Azure 逻辑应用

[Azure 逻辑应用](/azure/logic-apps)提供无服务器引擎来生成自动化工作流，以便在云服务和本地系统之间集成应用和数据。 使用可视化设计器生成工作流。 可以基于事件或计时器触发工作流，并利用连接器来集成应用程序和促进企业到企业 (B2B) 通信。 逻辑应用与 Azure Functions 无缝集成。

![Azure 逻辑应用徽标](./media/logic-apps-logo.png)

逻辑应用不仅可以将云服务（如 Functions）与云资源（如队列和数据库）连接。 还可以通过本地网关协调本地工作流。 例如，可以使用逻辑应用触发本地 SQL 存储过程，以便响应工作流中基于云的事件或条件逻辑。 详细了解[使用 Azure 本地数据网关连接到本地数据源](/azure/analysis-services/analysis-services-gateway)。

![逻辑应用体系结构](./media/logic-apps-architecture.png)

与 Azure Functions 一样，可以使用触发器启动逻辑应用工作流。 有很多触发器可供选择。 以下截图仅显示数百个可用触发器中较常用的几个触发器。

![逻辑应用触发器](./media/logic-app-triggers.png)

触发应用后，可以使用可视化设计器来构建步骤、循环、条件和操作。 可以在后续步骤中使用上一步中引入的任何数据。 以下工作流从 CosmosDB 数据库加载 URL。 将查找具有主机 `t.co` 的 URL，然后在 Twitter 上搜索它们。 如果找到相应的推文，将通过调用函数来更新具有相关推文的文档。

![逻辑应用工作流](./media/logic-app-workflow.png)

逻辑应用仪表板显示运行工作流的历史记录，以及每次运行是否已成功完成。 可以导航到任何给定运行，并检查每个步骤使用的数据以进行故障排除。 逻辑应用还提供了可编辑且非常适合于复杂企业工作流的现有模板。

若要了解详细信息，请参阅 [Azure 逻辑应用](/azure/logic-apps)。

>[!div class="step-by-step"]
>[上一页](application-insights.md)
>[下一页](event-grid.md)
