---
title: 设计面向 DDD 的微服务
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 了解面向 DDD 的订购微服务及其应用层的设计。
ms.date: 10/08/2018
ms.openlocfilehash: 583e103c8bd9d828731a658ea2fd2aa0758e7a12
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80988734"
---
# <a name="design-a-ddd-oriented-microservice"></a>设计面向 DDD 的微服务

域驱动设计 (DDD) 提倡基于与用例相关的真实业务来构建模型。 在构建应用程序的上下文中，DDD 用域来描述问题。 它将独立的问题区域描述为界定的上下文（每个界定的上下文关联一个微服务），并强调使用一种通用的语言来讨论这些问题。 它还提出许多技术概念和模式，如具有充血模型的域实体（无[贫血模型](https://martinfowler.com/bliki/AnemicDomainModel.html)）、值对象、聚合和聚合根（或根实体）规则，用于支持内部实现。 本部分介绍这些内部模式的设计和实现。

有时这些 DDD 技术规则和模式被认为是障碍，因为会导致实施 DDD 方法时的学习曲线较陡峭。 但重要的并非模式本身，而是如何根据业务问题组织代码，并使用相同的业务术语（通用语言）。 此外，只有在需要实现采用重要业务规则的复杂微服务时，才应使用 DDD 方法。 较为简单的功能，如 CRUD 服务，可使用较为简单的方法来管理。

设计和定义微服务时，最关键的任务是界定边界。 借助 DDD 模式，可了解域中的复杂性。 对于每个界定的上下文的域模型，需确定和定义为域建模时所需的实体、值对象和聚合。 生成和优化限定在某个边界内的域模型，该边界用于定义上下文。 如果是微服务的形式，这会十分明晰。 这些边界内的组件最终会成为微服务，但在某些情况下，BC 或业务微服务可由多个物理服务组成。 DDD 与边界相关，微服务也是如此。

## <a name="keep-the-microservice-context-boundaries-relatively-small"></a>保持相对较小的微服务上下文边界

确定界定的上下文之间的边界需要在两个相互冲突的目标之间进行权衡。 一方面，最初需要创建尽可能小的微服务，但这不应是主要的驱动因素；应在需要内聚的内容周围创建边界。 另一方面，要避免微服务之间的非正式通信。 这些目标可能相互冲突。 要平衡这些目标，应将系统分解为尽可能多的小型微服务，直至通信边界数量迅速增加，同时不断试图划分新的界定的上下文。 在单个界定的上下文中，内聚很重要。

它类似于实现类时的[不适当亲密关系代码异味](https://sourcemaking.com/refactoring/smells/inappropriate-intimacy)。 如果两个微服务之间需要大量相互配合，它们极可能是相同的微服务。

另一种考量方法是观察自治性。 如果一个微服务必须依赖另一个服务才能处理请求，那它就不是真正自治。

## <a name="layers-in-ddd-microservices"></a>DDD 微服务中的层

在业务和技术层面较为复杂的大多数企业应用程序都由多个层定义。 层是一种逻辑项目，与服务部署无关。 它们的存在是为了帮助开发人员管理代码中的复杂性。 不同的层（如域模型层与表示层等）可能具有不同的类型，并为这些类型之间的转换提供授权。

例如，实体可从数据库进行加载。 相关信息的一部分或全部信息，包括来自其他实体的其他数据，可通过 REST Web API 发送到客户端 UI。 此处的重点是域实体限定在域模型层内，不应将其传播到其不属于的区域（如表示层）。

此外，需要通过聚合根（根实体）控制始终有效的实体（请参阅[设计域模型层中的验证](domain-model-layer-validations.md)部分）。 因此，不应将实体绑定到客户端视图，因为在 UI 级别，某些数据可能仍未进行验证。 这正是 ViewModel（视图模式）的功能所在。 ViewModel 是专为解决表示层需要而创建的数据模型。 域实体并不直接属于 ViewModel。 相反，还需要在 Viewmodel 和域实体之间进行相互转换。

为解决复杂性，务必要通过聚合根来控制一个域模型，以确保与该组实体（聚合）相关的所有固定协定和规则通过单个入口点或入口（聚合根）来执行。

图 7-5 演示如何在 eShopOnContainers 应用程序中实现分层设计。

![显示域驱动设计微服务中的层的关系图。](./media/ddd-oriented-microservice/domain-driven-design-microservice.png)

**图 7-5**。 eShopOnContainers 订单微服务中的 DDD 层

DDD 微服务（例如，订购）中的三个层。 每层都是一个 VS 项目：应用程序层是 Ordering.API，域层是 Ordering.Domain，基础结构层是 Ordering.Infrastructure。 需要将系统设计为每个层只与某个其他层进行通信。 为了更轻松的实现这种设计，应将层实现为不同的类库，因为这样可以清楚地确定库之间的依赖关系。 例如，域模型层不应在任何其他层（域模型类应为普通旧 CLR 对象（简称 [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)）类）上设置依赖关系。 如图 7-6 所示，**Ordering.Domain** 层库只在 .NET Core 库或 NuGet 包上具有依赖项，而在任何其他自定义库（如数据库或持久性库）上不具有依赖项。

![Ordering.Domain 依赖项的屏幕截图。](./media/ddd-oriented-microservice/ordering-domain-dependencies.png)

**图7-6**。 通过作为库实现的层，可以更好地控制各层之间的依赖关系

### <a name="the-domain-model-layer"></a>域模型层

关于域模型层和应用层，Eric Evans 在其大作[域驱动设计](https://domainlanguage.com/ddd/)一书中作出了以下描述。

**域模型层**：负责表示业务概念、有关业务状况的信息和业务规则。 反映业务状况的状态是通过这个层进行控制和利用的，但有关状态存储的具体技术细节则由基础结构负责实施。 这一层是业务软件的核心。

域模型层是表述业务的地方。 在 .NET 中实现微服务域模型层时，该层被编码为类库，具有用于捕获数据和行为（方法和逻辑）的域实体。

为遵循[持久性忽略](https://deviq.com/persistence-ignorance/)（Persistence Ignorance）和[基础结构忽略](https://ayende.com/blog/3137/infrastructure-ignorance)（Infrastructure Ignorance）原则，此层必须完全忽略数据持久性细节。 应由基础结构层来执行这些持久性任务。 因此，此层不应在基础结构上设置直接的依赖关系，这意味着存在一个重要规则，即域模型实体类应为普通旧 CLR 对象（[POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)）。

域实体不应在任何数据访问基础结构框架（如 Entity Framework 或 NHibernate）上具有任何直接的依赖关系（如派生自基类）。 理想情况下，域实体不应派生自或实现任何在基础结构框架中定义的类型。

大多数新式 ORM 框架（如 Entity Framework Core）允许使用这种方法，以确保域模型类不会耦合到基础结构。 但是使用某些 NoSQL 数据库和框架（如 Azure Service Fabric 中的执行组件和可靠集合）时，并不总是能够获得 POCO 实体。

即使在需要为域模型遵循持久性无感知原则的情况下，也不应忽略对持久性的关注。 仍然十分有必要了解物理数据模型和模型映射到实体对象模型的方式。 否则就不可能创建设计。

但这并不意味可以采用为关系数据库设计的模型，并将其直接移到 NoSQL 或面向文档的数据库。 在某些实体模型中，该模型或许适用，但通常是不适用的。 实体模型仍须遵循某些约束，这些约束基于存储技术和 ORM 技术。

### <a name="the-application-layer"></a>应用层

现在我们来了解应用层，这里可以再次引用 Eric Evans 的[域驱动设计](https://domainlanguage.com/ddd/)一书中的内容：

**应用程序层：** 定义软件要完成的作业并指导富有表现力的域对象解决问题。 这一层负责执行对业务具有意义的任务或与其他系统的应用层进行交互时需执行的任务。 这一层很“薄”。 它不包含业务规则或知识，仅针对下一层中域对象之间的协作，协调任务和委派工作。 它不具有反映业务状况的状态，但它可以具有状态，用于反映用户或程序的任务的进度。

.NET 中微服务的应用层通常被编码为 ASP.NET Core Web API 项目。 该项目实现微服务的交互、远程网络访问和从 UI 或客户端应用中使用的外部 Web API。 它包括查询（如果使用 CQRS 方法）、微服务接受的命令，甚至是微服务之间的事件驱动的通信（集成事件）。 表示应用程序层的 ASP.NET Core Web API 不能包含业务规则或域知识（尤其是用于事务或更新的域规则）；这些规则和知识应由域模型类库所有。 应用层须只能协调任务，不能保有或定义任何域状态（域模型）。 它将业务规则的执行委托给域模型类自身（聚合根和域实体），最终在这些域实体内更新数据。

基本上是在应用逻辑中实现所有依赖于某个前端的用例。 例如，与某个 Web API 服务相关的实现。

目标是域模型层中的域逻辑、其固定协定、数据模型和相关业务规则，必须完全独立于表示层和应用层。 最重要的是，域模型层不能直接依赖于任何基础结构框架。

### <a name="the-infrastructure-layer"></a>基础结构层

基础结构层是关于如何将最初存放在域实体中的数据（内存中）持久保存在数据库或另一个持久性存储区中。 一个例子是使用 Entity Framework Core 代码实现存储库模式类，该类使用 DBContext 将数据持久保存在关系数据库中。

为遵循之前提到的[持久性忽略](https://deviq.com/persistence-ignorance/)和[基础结构忽略](https://ayende.com/blog/3137/infrastructure-ignorance)原则，基础结构层不得“污染”域模型层。 必须通过不在框架上设置硬依赖关系，让域模型实体类对用于保存数据的基础结构（EF 或任何其他框架）保持为不可知状态。 域模型层类库应只具有域代码，且仅为 [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) 实体类，用于实现软件核心，并完全脱离基础结构技术。

因此，层或类库和项目应最终依赖于域模型层（库），但反之则不成立，如图 7-7 所示。

![显示 DDD 服务层之间存在的依赖项的关系图。](./media/ddd-oriented-microservice/ddd-service-layer-dependencies.png)

**图 7-7**。 DDD 中层之间的依赖关系

DDD 服务中的依赖项（应用层）依赖于域和基础结构，基础结构依赖于域，但域不依赖于任何层。 这一层设计对每个微服务应是独立的。 如之前所述，可以实现遵循 DDD 模式的最复杂的微服务，也可以使用简单的方法实现简单的数据驱动微服务（单个层中的简单 CRUD）。

#### <a name="additional-resources"></a>其他资源

- **DevIQ.持久性无感知原则** \
  <https://deviq.com/persistence-ignorance/>

- **Oren Eini。基础结构无感知** \
  <https://ayende.com/blog/3137/infrastructure-ignorance>

- **Angel Lopez。域驱动设计中的分层体系结构** \
  <https://ajlopez.wordpress.com/2008/09/12/layered-architecture-in-domain-driven-design/>

>[!div class="step-by-step"]
>[上一页](cqrs-microservice-reads.md)
>[下一页](microservice-domain-model.md)
