---
title: 调用持续时间
ms.date: 03/30/2017
ms.assetid: e4973ec3-3c66-4c0b-b5d0-294b62c83f7d
ms.openlocfilehash: 99b4f62182c7864fd32b878373814e85926025b5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96285494"
---
# <a name="calls-duration"></a>调用持续时间

计数器名称：调用持续时间  
  
## <a name="description"></a>描述  

 调用该操作的平均持续时间。 平均持续时间根据此公式计算：(N1-N0)/(D1-D0)。  
  
> [!WARNING]
> 在异步 WCF 服务上使用时，调用 Duration 计数器将始终返回-1。  
  
## <a name="see-also"></a>另请参阅

- [PERF_AVERAGE_TIMER](/previous-versions/windows/embedded/ms938538(v=msdn.10))
