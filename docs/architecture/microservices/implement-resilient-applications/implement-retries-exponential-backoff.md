---
title: 实现使用指数退避算法的重试
description: 了解如何使用指数退避算法实现重试。
ms.date: 10/16/2018
ms.openlocfilehash: 1b948e399495eeb12016006442ac08d2b04f2e69
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "68674534"
---
# <a name="implement-retries-with-exponential-backoff"></a>实现使用指数退避算法的重试

[*使用指数退避算法的重试*](/azure/architecture/patterns/retry)是一种重试操作方法，随着等待时间以指数方式增长，重试次数达到最大值（[指数退避算法](https://en.wikipedia.org/wiki/Exponential_backoff)）。 该技术接受以下事实：云资源可能因任何原因而出现几秒钟以上的间歇性不可用情况。 例如，业务流程协调程序可能会将容器移动到群集中的另一个节点以进行负载均衡。 在此期间，某些请求可能会失败。 另一个实例是像 SQL Azure 这样的数据库，在该数据库中，数据库可以移动到另一台服务器进行负载均衡，这会导致数据库有几秒钟时间不可用。

有好几种方法可实现使用指数退避算法的重试逻辑。

>[!div class="step-by-step"]
>[上一页](partial-failure-strategies.md)
>[下一页](implement-resilient-entity-framework-core-sql-connections.md)
