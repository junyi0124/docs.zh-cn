---
title: 创建基于微服务的复合 UI
description: 微服务体系结构不仅针对后端。 了解微服务在前端中的使用。
ms.date: 09/20/2018
ms.openlocfilehash: 1861d3bb6e5d4a0226aa8f3f72a2e0d3e83be56f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "72275737"
---
# <a name="creating-composite-ui-based-on-microservices"></a>创建基于微服务的复合 UI

微服务体系结构通常从服务器端处理数据和逻辑开始，但在许多情况下，UI 仍作为整体来处理。 但是，一种称为[微前端](https://martinfowler.com/articles/micro-frontends.html)的更高级的方法也是根据微服务设计应用程序 UI。 这意味着拥有的是由微服务生成的复合 UI，而不是服务器上的微服务和使用微服务的整体式客户端应用。 按照这种方法构建的微服务具有逻辑和可视化表示形式，功能十分完整。

图 4-20 显示了在整体式客户端应用程序中使用微服务的更为简单的方式。 当然，在生成 HTML 之后，可以创建一个 ASP.NET MVC 服务，再生成 JavaScript。 该图是一张简图，强调拥有使用微服务的单一（整体式）客户端 UI，该 UI 仅关注逻辑和数据，而不关注 UI 形状（HTML 和 JavaScript）。

![单一 UI 应用程序连接到微服务的关系图。](./media/microservice-based-composite-ui-shape-layout/monolith-ui-consume-microservices.png)

图 4-20  。 使用后端微服务的整体式 UI 应用程序

与之相比，复合 UI 是由微服务本身精确生成和组合成的。 一些微服务可控制 UI 特定区域的视觉形状。 主要区别在于，这具有基于模板的客户端 UI 组件（例如 TypeScript 类），而这些模板的数据构形 UI ViewModel 来自于各个微服务。

客户端应用程序启动时，每个客户端 UI 组件（例如 TypeScript 类）自身都会注册一个基础结构微服务，该微服务能够为特定方案提供 ViewModel。 如果微服务形状发生更改，则 UI 也会更改。

图 4-21 显示了一种复合 UI 方法。 这只是一个简化示例，因为可能还有其他微服务，它们将聚合基于不同技术的精细部件。 具体取决于构建的是传统 Web 方法 (ASP.NET MVC) 还是 SPA （单页应用程序）。

![由多个视图模型组成的复合 UI 的关系图。](./media/microservice-based-composite-ui-shape-layout/microservice-generate-composite-ui.png)

图 4-21  。 由后端微服务形成的复合 UI 应用程序示例

这些 UI 组合微服务中的每一个都类似于一个小型的 API 网关。 但在此情况下，每个服务都负责一个小的 UI 领域。

由微服务驱动的复合 UI 方法的挑战性可能更大也可能更小，具体取决于使用的 UI 技术。 例如，生成传统 Web 应用程序时，不会采用生成 SPA 或本机移动应用的技术（开发 Xamarin 应用时，此方法可能更具挑战性）。

出于多种原因，[eShopOnContainers](https://aka.ms/MicroservicesArchitecture) 示例应用程序使用整体式 UI 方法。 首先，其作用是引入微服务和容器。 虽然复合 UI 更高级，但该 UI 的设计和开发工作也更复杂。 其次，eShopOnContainers 还提供了基于 Xamarin 的本机移动应用，这增加了客户端 C\# 的复杂性。

不过，建议通过以下参考资料，深入了解基于微服务的复合 UI。

## <a name="additional-resources"></a>其他资源

- **微前端（Martin Fowler 的博客）**  
  <https://martinfowler.com/articles/micro-frontends.html>
  
- **微前端（Michael Geers 站点）**  
  <https://micro-frontends.org/>
  
- **使用 ASP.NET（Particular 的 Workshop）的复合 UI**  
  <https://github.com/Particular/Workshop/tree/master/demos/asp-net-core>

- **Ruben Oostinga。微服务体系结构中的整体式前端**  
  <https://xebia.com/blog/the-monolithic-frontend-in-the-microservices-architecture/>

- **Mauro Servienti。优化 UI 组合的秘诀**  
  <https://particular.net/blog/secret-of-better-ui-composition>

- **Viktor Farcic。在微服务中纳入前端 Web 组件**  
  <https://technologyconversations.com/2015/08/09/including-front-end-web-components-into-microservices/>

- **在微服务体系结构中管理前端**  
  <https://allegro.tech/2016/03/Managing-Frontend-in-the-microservices-architecture.html>

>[!div class="step-by-step"]
>[上一页](microservices-addressability-service-registry.md)
>[下一页](resilient-high-availability-microservices.md)
