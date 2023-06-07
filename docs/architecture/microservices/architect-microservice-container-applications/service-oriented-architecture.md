---
title: 面向服务的体系结构
description: 了解微服务与面向服务的体系结构 (SOA) 之间的根本区别。
ms.date: 09/20/2018
ms.openlocfilehash: 84786539fbac0e8b38a81a2580232474774cd355
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "68674924"
---
# <a name="service-oriented-architecture"></a>面向服务的体系结构

面向服务的体系结构 (SOA) 是一个使用过度的术语，不同的人对它的理解不尽相同。 但相同的理解是，SOA 意味着通过将应用程序分解为多个服务（通常为 HTTP 服务），将其分为不同类型（例如子系统或层），从而来划分应用程序的结构。

这些服务现可部署为 Docker 容器，这可解决部署问题，因为容器映像中包含所有依赖关系。 但是，当需要纵向扩展 SOA 应用程序时，如果基于单个 Docker 主机进行部署，可能面临可伸缩性和可用性挑战。 在这种情况下，Docker 群集软件或业务流程协调程序可能会对你有所帮助，我们稍后将在介绍微服务部署方法的部分中进行说明。

对于传统的面向服务的体系结构和更高级的微服务体系结构，Docker 容器都是有用的（但不是必需的）。

微服务源自 SOA，但 SOA 不同于微服务体系结构。 诸如大型中央代理、组织级别的中央业务流程协调程序和[企业服务总线 (ESB)](https://en.wikipedia.org/wiki/Enterprise_service_bus) 等功能在 SOA 中很典型。 但在大多数情况下，这些是微服务社区中的反模式。 事实上，有些人认为“微服务体系结构是处理得当的 SOA”。

本指南关注微服务，因为相较于微服务体系结构中的要求和技术，SOA 方法不那么受普遍认可。 如果知道如何生成基于微服务的应用程序，还会了解如何构建更简单的面向服务的应用程序。

>[!div class="step-by-step"]
>[上一页](docker-application-state-data.md)
>[下一页](microservices-architecture.md)
