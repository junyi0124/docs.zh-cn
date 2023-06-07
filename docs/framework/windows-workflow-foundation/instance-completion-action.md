---
title: 实例完成操作
ms.date: 03/30/2017
ms.assetid: 90cc99d2-9fef-42fd-bcbf-a56917993721
ms.openlocfilehash: 698ac0ed5a7cbd4f6a5623cf8d9b6fbea1128d0a
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2019
ms.locfileid: "70044332"
---
# <a name="instance-completion-action"></a>实例完成操作

利用 SQL 工作流实例存储的 "**实例完成操作**" 属性, 您可以指定在实例完成之后是否从持久性数据库中删除工作流实例的数据和元数据。 此属性的允许值为**DeleteAll**和**DeleteNothing**。 以下列表对这两个选项进行了说明：

- **DeleteAll (默认值)。** 如果该属性的值设置为 DeleteAll，则在工作流实例完成之后会将从持久性数据库中删除该实例的数据和元数据。

- **DeleteNothing.** 如果该属性的值设置为 DeleteNothing，则即使在工作流实例完成之后也会将该实例的数据和元数据保留在持久性数据库中。

  > [!CAUTION]
  > 实例完成之后继续保留实例状态信息会导致持久性数据库增大。 随着数据库的增大，持久性子系统执行数据库操作所花费的时间会越来越长，因此需要定期从持久性数据库中清除实例状态信息，以使服务在满足您的性能需求的级别执行。
