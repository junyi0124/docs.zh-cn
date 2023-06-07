---
title: 适用于云本机应用的 IdentityServer
description: 构建适用于 Azure 的云本机 .NET 应用 |IdentityServer
ms.date: 05/13/2020
ms.openlocfilehash: bdf193aac348b54f2ebf5b537beef5d61a1d5a1e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91163825"
---
# <a name="identityserver-for-cloud-native-applications"></a>适用于云原生应用程序的 IdentityServer

IdentityServer 是一种开源身份验证服务器，用于实现 ASP.NET Core (OIDC) 和 OAuth 2.0 标准。 它旨在提供一种通用方法来对所有应用程序的请求进行身份验证，无论这些请求是 web、本机、移动还是 API 终结点。 IdentityServer 可用于实现多个应用程序和应用程序类型 (SSO) 的单一登录。 它可用于通过登录窗体和类似用户界面以及基于服务的身份验证（通常涉及令牌颁发、验证和续订，无需任何用户界面）对实际用户进行身份验证。 IdentityServer 旨在作为可自定义的解决方案。 通常，每个实例都可以进行自定义，以适应单个组织和/或一组应用程序的需求。

## <a name="common-web-app-scenarios"></a>常见的 web 应用方案

通常，应用程序需要支持以下部分或所有方案：

- 用户使用浏览器访问 web 应用程序。
- 用于从基于浏览器的应用程序访问后端 Web Api 的用户。
- 移动/本机客户端上访问后端 Web Api 的用户。
- 访问后端 Web Api 的其他应用程序 (没有活动的用户或用户界面) 。
- 任何应用程序都可能需要使用其自己的标识或委托给用户的标识，与其他 Web Api 交互。

![应用程序类型和方案](./media/application-types.png)

**图 8-1**。 应用程序类型和方案。

在上述每种情况下，都需要确保公开的功能不受未经授权的使用。 通常，这通常需要对请求资源的用户进行身份验证。 此身份验证可使用几种常用协议，如 SAML2p、WS 送连接或 OpenID Connect。 与 Api 通信通常使用 OAuth2 协议及其对安全令牌的支持。 从应用程序自身分离这些重要的交叉切削安全问题及其实现细节可确保一致性并提高安全性和可维护性。 将这些问题外包给专用产品（如 IdentityServer）可帮助每个应用程序自行解决这些问题。

IdentityServer 提供在 ASP.NET Core 应用程序中运行的中间件，并添加对 OpenID Connect 和 OAuth2 的支持 (请参阅 [支持的规范](https://docs.identityserver.io/en/latest/intro/specs.html)) 。 组织将使用 IdentityServer 中间件创建自己的 ASP.NET Core 应用程序，将其用作所有基于令牌的安全协议的 STS。 IdentityServer 中间件公开终结点以支持标准功能，其中包括：

- 授权 (对最终用户进行身份验证) 
- 标记 (以编程方式请求标记) 
- 发现 (有关服务器的元数据) 
- 用户信息 (使用有效的访问令牌获取用户信息) 
- 设备授权 (用于启动设备流授权) 
- 自检 (令牌验证) 
- 吊销 (令牌吊销) 
- 结束会话 (跨所有应用触发单一注销) 

## <a name="getting-started"></a>入门

IdentityServer4 是开源的，可免费使用。 你可以使用其 NuGet 包将其添加到你的应用程序。 主包为 [IdentityServer4](https://www.nuget.org/packages/IdentityServer4/) ，已在4000000次下载。 基包不包含任何用户界面代码，并且仅支持内存中的配置。 若要将其与数据库一起使用，你还需要一个数据访问接口（如 [IdentityServer4](https://www.nuget.org/packages/IdentityServer4.EntityFramework) ），它使用 Entity Framework Core 来存储 IdentityServer 的配置和操作数据。 对于用户界面，可以将文件从 [快速入门 UI 存储库](https://github.com/IdentityServer/IdentityServer4.Quickstart.UI) 复制到 ASP.NET Core MVC 应用程序中，以添加对使用 IdentityServer 中间件进行登录和注销的支持。

## <a name="configuration"></a>Configuration

IdentityServer 支持不同种类的协议和社交身份验证提供程序，这些提供程序可配置为每个自定义安装的一部分。 通常在方法的 ASP.NET Core 应用程序类中完成此操作 `Startup` `ConfigureServices` 。 此配置涉及到指定支持的协议以及将使用的服务器和终结点的路径。 图8-2 显示了从 IdentityServer4 快速入门 UI 项目中获取的示例配置：

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();

        // some details omitted
        services.AddIdentityServer();

          services.AddAuthentication()
            .AddGoogle("Google", options =>
            {
                options.SignInScheme = IdentityServerConstants.ExternalCookieAuthenticationScheme;

                options.ClientId = "<insert here>";
                options.ClientSecret = "<insert here>";
            })
            .AddOpenIdConnect("demoidsrv", "IdentityServer", options =>
            {
                options.SignInScheme = IdentityServerConstants.ExternalCookieAuthenticationScheme;
                options.SignOutScheme = IdentityServerConstants.SignoutScheme;

                options.Authority = "https://demo.identityserver.io/";
                options.ClientId = "implicit";
                options.ResponseType = "id_token";
                options.SaveTokens = true;
                options.CallbackPath = new PathString("/signin-idsrv");
                options.SignedOutCallbackPath = new PathString("/signout-callback-idsrv");
                options.RemoteSignOutPath = new PathString("/signout-idsrv");

                options.TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name",
                    RoleClaimType = "role"
                };
            });
    }
}
```

**图 8-2**。 配置 IdentityServer。

IdentityServer 还托管了公共演示网站，可用于测试各种协议和配置。 它位于 [https://demo.identityserver.io/](https://demo.identityserver.io/) ，并包括有关如何根据提供给它的行为配置的信息 `client_id` 。

## <a name="javascript-clients"></a>JavaScript 客户端

许多云本机应用程序利用服务器端 Api 和丰富的客户端单页面应用程序，在前端 (Spa) 。 IdentityServer 附带了 [JavaScript 客户端](https://docs.identityserver.io/en/latest/quickstarts/4_javascript_client.html) (`oidc-client.js`) via NPM，可将其添加到 spa，使其能够将 IdentityServer 用于登录、注销和 web api 的基于令牌的身份验证。

## <a name="references"></a>参考

- [IdentityServer 文档](https://docs.identityserver.io/en/latest/)
- [应用程序类型](/azure/active-directory/develop/app-types)
- [JavaScript OIDC 客户端](https://docs.identityserver.io/en/latest/quickstarts/4_javascript_client.html)

>[!div class="step-by-step"]
>[上一页](azure-active-directory.md)
>[下一页](security.md)
