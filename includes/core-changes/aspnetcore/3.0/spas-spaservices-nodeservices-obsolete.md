---
ms.openlocfilehash: ac5a3c4f3aefbb59418ad92b2d795f36916f877f
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032433"
---
### <a name="spas-spaservices-and-nodeservices-marked-obsolete"></a>SPA：SpaServices 和 NodeServices 被标记为过时

自 ASP.NET Core 2.1 起，以下 NuGet 包的内容都是不必要的。 因此，下列包将被标记为过时：

- [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/)
- [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/)

出于相同的原因，以下 npm 模块将被标记为已弃用：

- [aspnet-angular](https://www.npmjs.com/package/aspnet-angular)
- [aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering)
- [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack)
- [aspnet-webpack-react](https://www.npmjs.com/package/aspnet-webpack-react)
- [domain-task](https://www.npmjs.com/package/domain-task)

上述包和 npm 模块稍后将从 .NET 5 中删除。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

已弃用的包和 npm 模块旨在将 ASP.NET Core 与各种单页应用 (SPA) 框架集成。 此类框架包括 Angular、React 和 React Redux。

#### <a name="new-behavior"></a>新行为

[Microsoft.AspNetCore.SpaServices.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions/) NuGet 包中存在新的集成机制。 自 ASP.NET Core 2.1 起，包仍然是 Angular 和 React 项目模板的基础。

#### <a name="reason-for-change"></a>更改原因

ASP.NET Core 支持与各种单页应用 (SPA) 框架（包括 Angular、React 和 React Redux）集成。 最初，与这些框架的集成是通过 ASP.NET Core 特定组件实现的，这些组件用于处理服务器端预呈现和与 Webpack 集成等情况。 随着时间的推移，行业标准也发生了变化。 每个 SPA 框架都发布了其自己的标准命令行接口。 例如，Angular CLI 和 create-react-app。

当 ASP.NET Core 2.1 于 2018 年 5 月发布时，团队对标准的变化做出了响应。 提供了一种更新且更简单的方法来与 SPA 框架自身的工具链集成。 自 ASP.NET Core 2.1 起，包 `Microsoft.AspNetCore.SpaServices.Extensions` 中存在新的集成机制，且仍是 Angular 和 React 项目模板的基础。

为了阐明较低版本的 ASP.NET Core 特定组件不相关且不建议使用，请执行以下操作：

- 2\.1 之前的集成机制被标记为已过时。
- 支持 npm 包被标记为已弃用。

#### <a name="recommended-action"></a>建议操作

如果正在使用这些包，请更新应用以使用以下功能：

- 在 `Microsoft.AspNetCore.SpaServices.Extensions` 包中。
- 由使用的 SPA 框架提供

若要启用服务器端预呈现和热模块重载等功能，请参阅相应的 SPA 框架的文档。 `Microsoft.AspNetCore.SpaServices.Extensions` 中的功能未  过时，将继续受到支持。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

- <xref:Microsoft.AspNetCore.Builder.SpaRouteExtensions?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Builder.WebpackDevMiddleware?displayProperty=nameWithType>

- <xref:Microsoft.AspNetCore.NodeServices.EmbeddedResourceReader?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.INodeServices?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.NodeServicesFactory?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.NodeServicesOptions?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.StringAsTempFile?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.INodeInstance?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.NodeInvocationException?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.NodeInvocationInfo?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.NodeServicesOptionsExtensions?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.NodeServices.HostingModels.OutOfProcessNodeInstance?displayProperty=nameWithType>

- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.ISpaPrerenderer?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.ISpaPrerendererBuilder?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.JavaScriptModuleExport?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.Prerenderer?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.PrerenderTagHelper?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Prerendering.RenderToStringResult?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SpaServices.Webpack.WebpackDevMiddlewareOptions?displayProperty=nameWithType>

- <xref:Microsoft.Extensions.DependencyInjection.NodeServicesServiceCollectionExtensions?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.DependencyInjection.PrerenderingServiceCollectionExtensions?displayProperty=nameWithType>

<!--

#### Affected APIs

- `T:Microsoft.AspNetCore.Builder.SpaRouteExtensions`
- `T:Microsoft.AspNetCore.Builder.WebpackDevMiddleware`

- `T:Microsoft.AspNetCore.NodeServices.EmbeddedResourceReader`
- `T:Microsoft.AspNetCore.NodeServices.INodeServices`
- `T:Microsoft.AspNetCore.NodeServices.NodeServicesFactory`
- `T:Microsoft.AspNetCore.NodeServices.NodeServicesOptions`
- `T:Microsoft.AspNetCore.NodeServices.StringAsTempFile`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.INodeInstance`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.NodeInvocationException`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.NodeInvocationInfo`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.NodeServicesOptionsExtensions`
- `T:Microsoft.AspNetCore.NodeServices.HostingModels.OutOfProcessNodeInstance`

- `T:Microsoft.AspNetCore.SpaServices.Prerendering.ISpaPrerenderer`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.ISpaPrerendererBuilder`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.JavaScriptModuleExport`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.Prerenderer`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.PrerenderTagHelper`
- `T:Microsoft.AspNetCore.SpaServices.Prerendering.RenderToStringResult`
- `T:Microsoft.AspNetCore.SpaServices.Webpack.WebpackDevMiddlewareOptions`

- `T:Microsoft.Extensions.DependencyInjection.NodeServicesServiceCollectionExtensions`
- `T:Microsoft.Extensions.DependencyInjection.PrerenderingServiceCollectionExtensions`

-->
