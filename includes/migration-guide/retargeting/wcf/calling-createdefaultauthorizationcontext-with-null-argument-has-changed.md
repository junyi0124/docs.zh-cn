---
ms.openlocfilehash: c5a061dffa1deb66e1769d6ec70dfa2155ac6a31
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615612"
---
### <a name="calling-createdefaultauthorizationcontext-with-a-null-argument-has-changed"></a>调用具有 null 自变量的 CreateDefaultAuthorizationContext 的方式已更改

#### <a name="details"></a>详细信息

调用具有 null authorizationPolicies 自变量的 <xref:System.IdentityModel.Policy.AuthorizationContext.CreateDefaultAuthorizationContext(System.Collections.Generic.IList{System.IdentityModel.Policy.IAuthorizationPolicy})?displayProperty=fullName> 所返回的 <xref:System.IdentityModel.Policy.AuthorizationContext?displayProperty=fullName> 实现更改了其在 .NET Framework 4.6 中的实现。

#### <a name="suggestion"></a>建议

在极少数情况下，使用自定义身份验证的 WCF 应用可能会看到行为差异。 在这类情况下，可使用以下两种方式之一还原以前的行为：

- 重新编译你的应用以面向早于 4.6 的 .NET Framework 版本。 对于 IIS 承载的服务，请使用 `<httpRuntime targetFramework="x.x">` 元素以面向早期版本的 .NET Framework。
- 将下面的行添加到 app.config 文件的 `<appSettings>` 部分：

    ```xml
    <add key="appContext.SetSwitch:Switch.System.IdentityModel.EnableCachedEmptyDefaultAuthorizationContext" value="true" />
    ```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IdentityModel.Policy.AuthorizationContext.CreateDefaultAuthorizationContext(System.Collections.Generic.IList{System.IdentityModel.Policy.IAuthorizationPolicy})?displayProperty=nameWithType>
