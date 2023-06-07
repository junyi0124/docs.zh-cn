---
title: 创建、改进微服务 API 及协定并进行版本控制
description: 在考虑演变和版本控制的情况下创建微服务 API 和协定，因为需要改变。
ms.date: 09/20/2018
ms.openlocfilehash: 9164bfd12df18a88ac187c8962f0afc80b702881
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557667"
---
# <a name="creating-evolving-and-versioning-microservice-apis-and-contracts"></a>创建、改进微服务 API 及协定并进行版本控制

微服务 API 是服务及其客户端之间的协定。 用户将能够独立改进微服务，前提是不破坏其 API 协定，这正是协定之所以重要的原因。 如果更改协定，会影响客户端应用程序或 API 网关。

API 定义的本质取决于所使用的协议。 例如，如果使用的是消息传送（如 [AMQP](http://www.amqp.org/)）协议，则 API 由消息类型构成。 如果使用的是 HTTP 和 RESTful 服务，则 API 由 URL、请求和响应 JSON 格式构成。

但是，即使关注的是初始协定，服务 API 仍需随时间推移而改变。 如果发生这种情况 — 尤其是如果 API 是一个供多个客户端应用程序使用的公共 API，通常不能强制所有客户端升级到新的 API 协定。 通常需要逐渐增加对新版服务的部署，并且同时运行旧版和新版的服务协定。 因此，有必要制定服务版本控制策略。

如果 API 改动较小，例如向 API 添加属性或参数，则使用较旧 API 的客户端应切换到服务的新版本。 你或许能够为任何缺少的必需属性提供默认值，并且客户端或许能够忽略任何额外响应属性。

但是，有时需要对服务 API 作出重大但却不兼容的更改。 因为可能不能强制客户端应用程序或服务立即升级到最新版本，服务必须为旧版 API 提供一段时间的支持。 如果使用基于 HTTP 的机制，如 REST，一种方法是在 URL 中或 HTTP 标头中嵌入 API 版本号。 然后可以决定是在同一服务实例内同时实现两个版本的服务，还是部署不同实例，分别处理两个版本的 API。 解决此问题的一个好方法是使用[中介器模式](https://en.wikipedia.org/wiki/Mediator_pattern)（例如，[MediatR 库](https://github.com/jbogard/MediatR)），将不同的实现版本分离到独立的处理程序中去。

最后，如果使用的是 REST 体系结构，[超媒体](https://www.infoq.com/articles/mark-baker-hypermedia)是为服务提供版本控制并允许改进 API 的最佳解决方案。

## <a name="additional-resources"></a>其他资源

- **Scott Hanselman.ASP.NET Core RESTful Web API versioning made easy**（简化 ASP.NET Core RESTful Web API 版本控制） \
  <https://www.hanselman.com/blog/ASPNETCoreRESTfulWebAPIVersioningMadeEasy.aspx>

- **RESTful Web API 版本控制** \
  <https://docs.microsoft.com/azure/architecture/best-practices/api-design#versioning-a-restful-web-api>

- **Roy Fielding。版本控制、超媒体和 REST** \
  <https://www.infoq.com/articles/roy-fielding-on-versioning>

>[!div class="step-by-step"]
>[上一页](asynchronous-message-based-communication.md)
>[下一页](microservices-addressability-service-registry.md)
