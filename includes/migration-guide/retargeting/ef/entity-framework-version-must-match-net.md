---
ms.openlocfilehash: 863e7035827537e0f943af05c2f0232029b99db8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617122"
---
### <a name="entity-framework-version-must-match-the-net-framework-version"></a>Entity Framework 版本必须与 .NET Framework 版本匹配

#### <a name="details"></a>详细信息

Entity Framework (EF) 版本应与 .NET Framework 版本匹配。 Entity Framework 5 推荐用于 .NET Framework 4.5。 在与 <xref:System.ComponentModel.DataAnnotations> 相关的 .NET Framework 4.5 项目中，存在一些已知的 EF 4.x 问题。 在 .NET Framework 4.5 中，它们被移动到了另一程序集，因此确定要使用的注释时会出现问题。

#### <a name="suggestion"></a>建议

对于 .NET Framework 4.5 升级到 Entity Framework 5

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.5         |
| 类型    | 重定目标 |
