---
title: Docker 容器、映像和注册表
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | Docker 容器、映像和注册表
ms.date: 08/31/2018
ms.openlocfilehash: 3b643a3bf4ca3ce1b8ba3fc40cd2f3ad8bbe5ffb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "73737763"
---
# <a name="docker-containers-images-and-registries"></a>Docker 容器、映像和注册表

使用 Docker 时，开发人员会创建一个应用或服务，并将它及其依赖项打包到一个容器映像中。 映像是应用或服务及其配置和依赖项的静态表示形式。

若要运行应用或服务，应用的映像会实例化，以创建一个在 Docker 主机上运行的容器。 最初，会在开发环境或 PC 中测试容器。

开发人员应将映像存储在注册表中，该注册表可充当映射库并在部署到生产业务流程协调程序时使用。 Docker 通过 [Docker 中心](https://hub.docker.com/)维护公共注册表；其他供应商为不同映像集合提供注册表，包括 [Azure 容器注册表](https://azure.microsoft.com/services/container-registry/)。 或者，企业可以拥有一个本地专用注册表，用于其 Docker 映像。

图 2-4 显示了 Docker 中的映像和注册表与其他组件相关联的方式。 还显示了供应商的多个注册表产品/服务。

![显示 Docker 中的基本分类的关系图。](./media/docker-containers-images-registries/taxonomy-of-docker-terms-and-concepts.png)

**图 2-4**。 Docker 术语和概念的分类

注册表如同用于存储映像的书架，可被拉取以生成容器，从而运行服务或 Web 应用。 本地和公有云上均有专用 Docker 注册表。 Docker 中心是由 Docker 维护的公共注册表，除了 Docker 信任的注册表（企业级解决方案），Azure 还提供了 Azure 容器注册表。 AWS、Google 和其他产品也有容器注册表。

将映射存储到注册表中可存储静态和不可变的应用程序，包括其在框架级别的所有依赖项。 然后可将这些映像部署到多种环境中，并进行版本控制，从而提供一致的部署单元

无论是托管在本地还是托管在云中，在下列情况下都建议使用私有映像注册表：

- 由于保密性，映像不能被公开分享。

- 在映像和所选部署环境之间，希望网络延迟保持最低。 例如，如果生产环境是 Azure 云，为实现最低的网络延迟，需要将映像存储在 [Azure 容器注册表](https://azure.microsoft.com/services/container-registry/)中。 同样，如果生产环境是在本地，便需要使本地 Docker 信任的注册表在相同的本地网络中可用。

>[!div class="step-by-step"]
>[上一页](docker-terminology.md)
>[下一页](../net-core-net-framework-containers/index.md)
