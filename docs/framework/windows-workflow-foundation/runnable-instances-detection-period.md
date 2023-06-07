---
title: 可运行实例的检测周期
ms.date: 03/30/2017
ms.assetid: 4ea5c787-b638-47fd-bfc8-ede8c2898ce6
ms.openlocfilehash: aefde2726bb2ccc15f9e68009e5e141602b13b10
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245765"
---
# <a name="runnable-instances-detection-period"></a>可运行实例的检测周期

SQL 工作流实例存储运行一个内部任务，该任务将定期唤醒并检测持久性数据库中是否有可运行或可激活的实例。 SQL 工作流实例存储的可 **运行实例检测周期** 属性指定了一个时间段，在该时间段后，Sql 工作流实例存储将运行检测任务来检测持久性数据库中的任何可运行或可激活的工作流实例。  
  
 为此属性设置一个较短的间隔可减少与工作流实例相关联的计时器到期与发出事件信号并在随后加载实例之间的时间。 但是，这同时也会增大主机上的处理负载，在很少采用持久性计时器和/或很少发生主机故障的情况下，您可能不希望采用此类设置。 该属性的类型为 TimeSpan，其值遵循以下格式：hh:mm:ss。 该属性的最小值为 00:00:01， 默认值为 00:00:05。  
  
 有关检测和激活可运行和可激活的工作流实例的详细信息，请参阅 [实例激活](instance-activation.md)。
