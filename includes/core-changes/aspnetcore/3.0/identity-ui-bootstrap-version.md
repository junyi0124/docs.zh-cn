---
ms.openlocfilehash: c8f44ae1a500ed240dbff7d9a2c1479af368b7f1
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032280"
---
### <a name="identity-default-bootstrap-version-of-ui-changed"></a>标识：已更改 UI 的默认 Bootstrap 版本

从 ASP.NET Core 3.0 开始，标识 UI 默认为使用 Bootstrap 版本 4。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

`services.AddDefaultIdentity<IdentityUser>().AddDefaultUI();` 方法调用以前与 `services.AddDefaultIdentity<IdentityUser>().AddDefaultUI(UIFramework.Bootstrap3);` 相同

#### <a name="new-behavior"></a>新行为

`services.AddDefaultIdentity<IdentityUser>().AddDefaultUI();` 方法调用现在与 `services.AddDefaultIdentity<IdentityUser>().AddDefaultUI(UIFramework.Bootstrap4);` 相同

#### <a name="reason-for-change"></a>更改原因

在 ASP.NET Core 3.0 时间范围内发布了 Bootstrap 4。

#### <a name="recommended-action"></a>建议操作

如果使用默认标识用户界面并将其添加到 `Startup.ConfigureServices` 中，则会受到此更改的影响，如以下示例中所示：

```csharp
services.AddDefaultIdentity<IdentityUser>().AddDefaultUI();
```

请执行以下一项操作：

- 迁移应用，以使用其[迁移指南](https://getbootstrap.com/docs/4.0/migration)来使用 Bootstrap 4。
- 更新 `Startup.ConfigureServices` 以强制使用 Bootstrap 3。 例如：

    ```csharp
    services.AddDefaultIdentity<IdentityUser>().AddDefaultUI(UIFramework.Bootstrap3);
    ```

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
