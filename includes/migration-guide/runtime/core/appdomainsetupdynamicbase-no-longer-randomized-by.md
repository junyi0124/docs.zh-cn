---
ms.openlocfilehash: 26c50ac8b0e570e31a38b1913f73acbe21fae08b
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496807"
---
### <a name="appdomainsetupdynamicbase-is-no-longer-randomized-by-userandomizedstringhashalgorithm"></a>UseRandomizedStringHashAlgorithm 不再由 AppDomainSetup.DynamicBase 进行随机化

#### <a name="details"></a>详细信息

在 .NET Framework 4.6 之前，如果在应用的配置文件中启用了 UseRandomizedStringHashAlgorithm，那么 <xref:System.AppDomainSetup.DynamicBase> 的值会在应用程序域或者进程之间进行随机化。 自 .NET Framework 4.6 起，<xref:System.AppDomainSetup.DynamicBase> 将在运行的应用的不同实例之间，以及在不同的应用域之间返回一个稳定的结果。 不同应用的动态基各不相同；此更改仅删除相同应用的不同实例的随机命名元素。

#### <a name="suggestion"></a>建议

请注意，启用 <code>UseRandomizedStringHashAlgorithm</code> 不会导致随机化 <xref:System.AppDomainSetup.DynamicBase>。 如果需要随机基，则必须在应用代码中生成它，而不是通过此 API 生成。

| 名称    | 值       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.6|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.AppDomainSetup.DynamicBase?displayProperty=nameWithType>

<!--

#### Affected APIs

- `P:System.AppDomainSetup.DynamicBase`

-->
