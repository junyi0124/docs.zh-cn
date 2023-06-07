---
ms.openlocfilehash: 3164233f1ac056de385faa119143d75d3c2dc50c
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92224334"
---
> [!CAUTION]
> 代码访问安全性 (CA) 和部分受信任代码
>
> .NET Framework 提供一种机制，对在相同应用程序中运行的不同代码强制实施不同的信任级别，该机制称为代码访问安全性 (CAS)。
>
> **.NET Core、.NET 5 或更高版本中不支持 CAS。低于7.0 的 c # 版本不支持 CAS。**
>
> .NET Framework 中的 CAS 不应作为一种机制，用于根据代码来源或其他标识方面强制实施安全边界。 CA 和 Security-Transparent 代码不支持用作部分受信任的代码（尤其是未知来源的代码）的安全边界。 建议在未实施其他安全措施的情况下，不要加载和执行未知来源的代码。 .NET Framework 不会为可能会针对 CAS 沙盒发现的任何特权提升攻击发出安全修补程序。
>
> 此策略适用于 .NET Framework 的所有版本，但不适用于 Silverlight 中所含的 .NET Framework。
