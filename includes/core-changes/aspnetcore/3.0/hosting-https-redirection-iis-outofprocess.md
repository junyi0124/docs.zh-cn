---
ms.openlocfilehash: eb3fa768a491f6c0ff4b15479beabd71b0670338
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032269"
---
### <a name="hosting-https-redirection-enabled-for-iis-out-of-process-apps"></a>托管：已为 IIS 进程外应用启用 HTTPS 重定向

用于通过 IIS 进程外应用进行托管的 [ASP.NET Core 模块 (ANCM)](/aspnet/core/host-and-deploy/aspnet-core-module) 的 13.0.19218.0 版本可为 ASP.NET Core 3.0 和 2.2 应用启用现有 HTTPS 重定向功能。

有关讨论，请参阅 [dotnet/AspNetCore#15243](https://github.com/dotnet/AspNetCore/issues/15243)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

ASP.NET Core 2.1 项目模板首先引入了对 HTTPS 中间件方法的支持，例如 <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A> 和 <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts%2A>。 启用 HTTPS 重定向需要添加配置，因为开发中的应用不使用默认端口 443。 仅当请求已使用 HTTPS 时，[HTTP 严格传输安全 (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) 才处于活动状态。 默认跳过 localhost。

#### <a name="new-behavior"></a>新行为

在 ASP.NET Core 3.0 中，IIS HTTPS 方案[已增强](https://github.com/dotnet/AspNetCore/pull/4685)。 利用此增强功能，应用可以发现服务器的 HTTPS 端口并默认使 `UseHttpsRedirection` 工作。 进程内组件通过 `IServerAddresses` 功能完成端口发现，该功能仅影响 ASP.NET Core 3.0 应用，因为进程内库通过框架进行版本控制。 进程外组件已更改为自动添加 `ASPNETCORE_HTTPS_PORT` 环境变量。 由于进程外组件是全局共享组件，因此此更改会同时影响 ASP.NET Core 2.2 和 3.0 应用。 ASP.NET Core 2.1 应用不会受到影响，因为它们默认使用 ANCM 的先前版本。

前面的行为已在 ASP.NET Core 3.0.1 和 3.1.0 预览版 3 中进行修改，以反转 ASP.NET Core 2.x 中的行为更改。 这些更改仅影响 IIS 进程外应用。

如上所述，安装 ASP.NET Core 3.0.0 还具有在 ASP.NET Core 2.x 应用中激活 `UseHttpsRedirection` 中间件的副作用。 已对 ASP.NET Core 3.0.1 和 3.1.0 预览版 3 中的 ANCM 进行更改，以确保安装它们时不会再对 ASP.NET Core 2.x 应用产生此影响。 ANCM 在 ASP.NET Core 3.0.0 中填充的 `ASPNETCORE_HTTPS_PORT` 环境变量在 ASP.NET Core 3.0.1 和 3.1.0 预览版 3 中已更改为 `ASPNETCORE_ANCM_HTTPS_PORT`。 `UseHttpsRedirection` 在这些版本中也已更新，可同时理解新旧变量。 不会更新 ASP.NET Core 2.x。 因此，它会还原为默认禁用的先前行为。

#### <a name="reason-for-change"></a>更改原因

已改进 ASP.NET Core 3.0 功能。

#### <a name="recommended-action"></a>建议操作

如果希望所有客户端都使用 HTTPS，则无需执行任何操作。 要允许部分客户端使用 HTTP，请执行以下步骤之一：

* 从项目的 `Startup.Configure` 方法中删除对 `UseHttpsRedirection` 和 `UseHsts` 的调用，然后重新部署应用。
* 在 web.config 文件中，将 `ASPNETCORE_HTTPS_PORT` 环境变量设置为空字符串  。 无需重新部署应用即可直接在服务器上进行此更改。 例如：

    ```xml
    <aspNetCore processPath="dotnet" arguments=".\WebApplication3.dll" stdoutLogEnabled="false" stdoutLogFile="\\?\%home%\LogFiles\stdout" >
        <environmentVariables>
        <environmentVariable name="ASPNETCORE_HTTPS_PORT" value="" />
        </environmentVariables>
    </aspNetCore>
    ```

`UseHttpsRedirection` 仍可：

* 在 ASP.NET Core 2.x 中手动激活，方法是将 `ASPNETCORE_HTTPS_PORT` 环境变量设置为相应的端口号（在大多数生产方案中为 443）。
* 在 ASP.NET Core 3.x 中停用，方法是使用空字符串值定义 `ASPNETCORE_ANCM_HTTPS_PORT`。 此值的设置方式与前面的 `ASPNETCORE_HTTPS_PORT` 示例相同。

运行 ASP.NET Core 3.0.0 应用的计算机应先安装 ASP.NET Core 3.0.1 运行时，然后再安装 ASP.NET Core 3.1.0 预览版 3 ANCM。 这样做可以确保继续按预期对 ASP.NET Core 3.0 应用运行 `UseHttpsRedirection`。

在 Azure 应用服务中，由于 ANCM 具有全局性，其部署时间与运行时不同。 部署 ASP.NET Core 3.0.1 和 3.1.0 后，才会将 ANCM 以及相应的更改部署到 Azure。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection(Microsoft.AspNetCore.Builder.IApplicationBuilder)?displayProperty=nameWithType>

<!-- 

#### Affected APIs

`M:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection(Microsoft.AspNetCore.Builder.IApplicationBuilder)`

-->
