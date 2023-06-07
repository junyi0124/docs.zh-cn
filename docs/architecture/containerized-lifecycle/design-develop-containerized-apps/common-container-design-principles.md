---
title: 常见容器设计原则
description: 了解良好容器设计的基本原则，即容器应只承载一个进程。
ms.date: 08/06/2020
ms.openlocfilehash: 7dcf4b4af3385a2a500c5bc16a889b56fa2c25d5
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916000"
---
# <a name="common-container-design-principles"></a>常见容器设计原则

在进入开发过程之前，有几个关于如何使用容器的基本概念值得一提。

## <a name="container-equals-a-process"></a>容器等于进程

在容器模型中，容器表示单个进程。 通过将容器定义为进程边界，可以开始创建用于缩放或批处理进程的基元。 运行 Docker 容器时，将看到 [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint) 定义。 这定义了容器的进程和生命周期。 该进程完成，则容器的生命周期结束。 有长时间运行的进程（如 Web 服务器）和存留较短的进程（如批处理作业），其可能已实现为 Microsoft Azure [WebJobs](https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/)。 如果进程失败，则容器结束，Orchestrator 接管。 如果业务流程协调程序已指示为使五个实例保持运行，而其中一个实例失败，则业务流程协调程序会创建另一个容器，来替换失败的进程。 在批处理作业中，使用参数启动该进程。 进程完成，则工作完成。

某些情况下，可能需要在单个容器中运行多个进程。 在任何体系结构文档中，没有“绝不可能”或“总是如此”的情况。 对于需要多个进程的方案，常见的模式是使用 [Supervisor](http://supervisord.org/)。

>[!div class="step-by-step"]
>[上一页](design-docker-applications.md)
>[下一页](monolithic-applications.md)
