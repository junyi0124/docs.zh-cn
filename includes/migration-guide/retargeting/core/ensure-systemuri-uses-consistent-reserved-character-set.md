---
ms.openlocfilehash: 21921156295d89aad04f3197fef9fa322f3c8c87
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614331"
---
### <a name="ensure-systemuri-uses-a-consistent-reserved-character-set"></a>确保 System.Uri 使用一致的保留字符集

#### <a name="details"></a>详细信息

在 <xref:System.Uri?displayProperty=fullName> 中，某些有时会被解码的百分比编码字符现在始终保持编码状态。 这种情况发生在访问 URI 的路径、查询、片段或用户信息组件的属性和方法中。 仅当以下两项均为 true 时，该行为才会更改：

- URI 包含以下任意保留字符的编码形式：`:`、`'`、`(`、`)`、`!` 或 `*`。
- URI 包含 Unicode 或编码的非保留字符。 如果以上两项均为 true，则编码的保留字符保持编码状态。 在先前版本的 .NET Framework 中，它们为解码状态。

#### <a name="suggestion"></a>建议

对于面向 .NET Framework 4.7.2 及更高版本的应用程序，默认启用新的解码行为。 如果不需要此更改，可通过将以下 [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 开关添加到应用程序配置文件的 `<runtime>` 部分，从而禁用更改：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Uri.DontEnableStrictRFC3986ReservedCharacterSets=true" />
</runtime>
```

对于面向 .NET Framework 早期版本，但在 .NET Framework 4.7.2 及更高版本下运行的应用程序，默认禁用新的解码行为。 可通过将以下 [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 开关添加到应用程序配置文件的 `<runtime>` 部分，进行启用：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Uri.DontEnableStrictRFC3986ReservedCharacterSets=false" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| 版本 | 4.7.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Uri?displayProperty=nameWithType>
