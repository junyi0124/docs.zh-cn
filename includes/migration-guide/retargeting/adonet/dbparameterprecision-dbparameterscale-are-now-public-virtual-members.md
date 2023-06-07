---
ms.openlocfilehash: 063e10b0310880af255793215a80a5529a5db0ff
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616028"
---
### <a name="dbparameterprecision-and-dbparameterscale-are-now-public-virtual-members"></a>DbParameter.Precision 和 DbParameter.Scale 现在是公共虚拟成员

#### <a name="details"></a>详细信息

实现 <xref:System.Data.Common.DbParameter.Precision> 和 <xref:System.Data.Common.DbParameter.Scale> 将实现为公共虚拟属性。 它们替换相应的显示接口实现、<xref:System.Data.Common.DbParameter.System%23Data%23IDbDataParameter%23Precision> 和 <xref:System.Data.Common.DbParameter.System%23Data%23IDbDataParameter%23Scale>。

#### <a name="suggestion"></a>建议

在重新生成 ADO.NET 数据库提供程序时，这些差异要求将“override”关键字应用到 Precision 和 Scale 属性。 仅在重新生成组件时才需要这样做；现有二进制文件将继续运行。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.5.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Data.Common.DbParameter.Precision?displayProperty=nameWithType>
- <xref:System.Data.Common.DbParameter.Scale?displayProperty=nameWithType>
