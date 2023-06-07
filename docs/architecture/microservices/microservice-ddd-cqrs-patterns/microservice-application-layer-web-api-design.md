---
title: 设计微服务应用层和 Web API
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 设计应用层的 SOLID 原则概述。
ms.date: 10/08/2018
ms.openlocfilehash: 491aa7bd90910c7f6c1d0ab56edfe0ae057ca006
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80988448"
---
# <a name="design-the-microservice-application-layer-and-web-api"></a>设计微服务应用层和 Web API

## <a name="use-solid-principles-and-dependency-injection"></a>使用 SOLID 原则和依赖关系注入

SOLID 原则是应用于所有新式任务关键型应用的重要技术，例如使用 DDD 模式开发微服务。 SOLID 是五个基本原则的缩写，它们分别是：

- 单一责任原则

- 开闭原则

- Liskov 替换原则

- 接口分隔原则

- 依赖关系反转原则

SOLID 不仅仅是关于如何设计应用程序或微服务内部层以及如何分离它们之间的依赖关系。 它与域无关，而与应用程序的技术设计相关。 最后一个原则，依赖关系反转原则，让使用者能够将基础结构层从其余层中分离出来，这样便能够更好地分离 DDD 层的实现。

依赖关系注入 (DI) 是实现依赖关系反转原则的一种方法。 它是一种用于在对象与其依赖项之间实现松散耦合的技术。 该技术不是直接实例化协作者或使用静态引用（即使用新的…），而是向类提供（或“注入”）该类执行其操作所需的对象。 大多数情况下，类通过其构造函数声明它们的依赖项，从而允许它们遵循显式依赖关系原则。 依赖关系注入通常基于特定的反向控制 (IoC) 容器。 ASP.NET Core 提供简单的内置 IoC 容器，但也可以使用其他 IoC 容器，如 Autofac 或 Ninject。

使用 SOLID 原则，往往会获得构造良好和易于测试的小型类。 但如何知道注入类的依赖关系是否将过多？ 如果通过构造函数使用 DI，只需通过查看构造函数的参数数目便可轻松检测依赖关系的数量。 如果依赖关系过多，通常表明类需要做的工作太多（[代码异味](https://deviq.com/code-smells/)），而且可能违反了单一责任原则。

如果要深入了解 SOLID，需要参阅另一相关指南。 因此，本指南仅要求读者掌握有关这些主题的最基本的知识。

#### <a name="additional-resources"></a>其他资源

- **SOLID：基本 OOP 原则** \
  <https://deviq.com/solid/>

- **反向控制容器和依赖关系注入模式** \
  <https://martinfowler.com/articles/injection.html>

- **Steve Smith.新增即粘附** \
  <https://ardalis.com/new-is-glue>

> [!div class="step-by-step"]
> [上一页](nosql-database-persistence-infrastructure.md)
> [下一页](microservice-application-layer-implementation-web-api.md)
