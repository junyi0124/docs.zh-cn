---
ms.openlocfilehash: b88f7d4a17f885b687d99ab9410a56039e176080
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614323"
---
### <a name="calls-to-claimsidentity-constructors"></a>调用 ClaimsIdentity 构造函数

#### <a name="details"></a>详细信息

从 .NET Framework 4.6.2 开始，具有 <xref:System.Security.Principal.IIdentity?displayProperty=fullName> 参数的 <xref:System.Security.Claims.ClaimsIdentity> 构造函数设置 <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> 属性的方式发生了变化。 如果 <xref:System.Security.Principal.IIdentity?displayProperty=fullName> 参数是 <xref:System.Security.Claims.ClaimsIdentity> 对象，且该 <xref:System.Security.Claims.ClaimsIdentity> 对象的 <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> 属性不为 `null`，则 <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> 属性是使用 <xref:System.Security.Claims.ClaimsIdentity.Clone> 方法附加的。 在 Framework 4.6.1 及早期版本中，<xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> 属性作为现有引用进行附加。由于此更改，从 .NET Framework 4.6.2 开始，新 <xref:System.Security.Claims.ClaimsIdentity> 对象的 <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> 属性不等于构造函数的 <xref:System.Security.Principal.IIdentity?displayProperty=fullName> 参数的 <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> 属性。 在 .NET Framework 4.6.1 及更早版本中，它们是相等的。

#### <a name="suggestion"></a>建议

如果不需要此行为，可以在应用程序配置文件中将 `Switch.System.Security.ClaimsIdentity.SetActorAsReferenceWhenCopyingClaimsIdentity` 开关设置为 `true`，从而还原旧行为。 为此，必须在 web.config 文件的 `<runtime>` 部分中添加以下内容：

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.Security.ClaimsIdentity.SetActorAsReferenceWhenCopyingClaimsIdentity=true" />
  </runtime>
</configuration>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Claims.ClaimsIdentity.%23ctor(System.Security.Principal.IIdentity)>
- <xref:System.Security.Claims.ClaimsIdentity.%23ctor(System.Security.Principal.IIdentity,System.Collections.Generic.IEnumerable{System.Security.Claims.Claim})>
- <xref:System.Security.Claims.ClaimsIdentity.%23ctor(System.Security.Principal.IIdentity,System.Collections.Generic.IEnumerable{System.Security.Claims.Claim},System.String,System.String,System.String)>
