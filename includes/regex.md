---
ms.openlocfilehash: b893966ceb65246ed6a7d214011b17ca14c50d5a
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85802841"
---

> [!WARNING]
> 如果使用 <xref:System.Text.RegularExpressions> 处理不受信任的输入，则传递一个超时。 恶意用户可能会向 `RegularExpressions` 提供输入，从而导致[拒绝服务攻击](https://www.us-cert.gov/ncas/tips/ST04-015)。 使用 `RegularExpressions` 的 ASP.NET Core 框架 API 会传递一个超时。
