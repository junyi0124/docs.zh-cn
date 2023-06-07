---
ms.openlocfilehash: 82103d82a6f68c62f3532608718bc71b0ba126bf
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032267"
---
### <a name="hosting-aspnetcoremodule-v1-removed-from-windows-hosting-bundle"></a>托管：从 Windows 托管捆绑包中删除了 AspNetCoreModule V1

从 ASP.NET Core 3.0 开始，Windows 托管捆绑不包含 AspNetCoreModule (ANCM) V1。

ANCM V2 向后兼容 ANCM OutOfProcess，建议与 ASP.NET Core 3.0 应用一起使用。

有关讨论，请参阅 [dotnet/aspnetcore#7095](https://github.com/dotnet/aspnetcore/issues/7095)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

ANCM V1 包含在 Windows 托管捆绑包中。

#### <a name="new-behavior"></a>新行为

ANCM V1 不包含在 Windows 托管捆绑包中。

#### <a name="reason-for-change"></a>更改原因

ANCM V2 向后兼容 ANCM OutOfProcess，建议与 ASP.NET Core 3.0 应用一起使用。

#### <a name="recommended-action"></a>建议操作

将 ANCM V2 与 ASP.NET Core 3.0 应用一起使用。

如果需要 ANCM V1，则可以使用 ASP.NET Core 2.1 或 2.2 Windows 托管捆绑包进行安装。

此更改将中断以下 ASP.NET Core 3.0 应用：

- 已明确选择将 ANCM V1 与 `<AspNetCoreModuleName>AspNetCoreModule</AspNetCoreModuleName>` 结合使用。
- 具有 `<add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />` 的自定义 web.config  文件。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
