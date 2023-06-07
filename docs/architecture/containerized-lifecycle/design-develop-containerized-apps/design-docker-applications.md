---
title: 设计 Docker 应用程序
description: 此处可以找到微服务架构的深入指南的参考，因为这是本指南中未详述的主题。
ms.date: 08/06/2020
ms.openlocfilehash: b63d4344abb358a393587bdacf91f6091b531af0
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915981"
---
# <a name="design-docker-applications"></a>设计 Docker 应用程序

第 1 章介绍了有关容器和 Docker 的基本概念。 该信息是入门所需的基础级别信息。 但是，企业应用程序可能会很复杂且由多个服务而不是单个服务或容器组成。 对于那些可选用例，需要了解其他设计方法，例如面向服务的体系结构 (SOA) 以及更高级的微服务概念和容器业务流程概念。 本文档的范围并不局限于微服务，也适用于任何 Docker 应用程序生命周期。因此，本文档不会深入介绍微服务体系结构。因为还可以将容器和 Docker 与常规 SOA、后台任务或作业一起使用，甚至可以使用整体式应用程序部署方法。

**详细信息** 要深入了解有关企业应用程序和微服务体系结构的详细信息，请阅读指南 [NET 微服务：适用于容器化 .NET 应用程序的体系结构](../../microservices/index.md)，也可以从 <https://aka.ms/MicroservicesEbook> 下载。

但是，在开始了解应用程序生命周期和 DevOps 之前，了解如何设计和构造应用程序以及设计选择非常重要。

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](common-container-design-principles.md)
