---
ms.openlocfilehash: 986b647047aaa4a185c1403e96e499ae587bea98
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617520"
---
### <a name="httpruntimeappdomainapppath-throws-a-nullreferenceexception"></a>HttpRuntime.AppDomainAppPath 引发 NullReferenceException

#### <a name="details"></a>详细信息

在 .NET Framework 4.6.2 中，当检索包含空字符的 `P:System.Web.HttpRuntime.AppDomainAppPath` 值时，运行时会引发 `T:System.NullReferenceException`。在 .NET Framework 4.6.1 及早期版本中，运行时将引发 `T:System.ArgumentNullException`。

#### <a name="suggestion"></a>建议

可执行以下任一操作来应对此更改：

- 如果应用程序是在 .NET Framework 4.6.2 上运行，请处理 `T:System.NullReferenceException`。
- 升级到 .NET Framework 4.7，这将还原以前的行为并引发 `T:System.ArgumentNullException`。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Web.HttpRuntime.AppDomainAppPath?displayProperty=nameWithType>
