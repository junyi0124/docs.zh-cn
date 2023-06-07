---
title: 安全性： ASP.NET Web 窗体和中的身份验证和授权 Blazor
description: 了解如何在 ASP.NET Web 窗体和中处理身份验证和授权 Blazor 。
author: ardalis
ms.author: daroth
no-loc:
- Blazor
ms.date: 11/20/2020
ms.openlocfilehash: 0344960237a5d9da61eb0d85987c44e136f1be48
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509840"
---
# <a name="security-authentication-and-authorization-in-aspnet-web-forms-and-no-locblazor"></a>安全性：ASP.NET Web Forms 和 Blazor  中的身份验证和授权

从 ASP.NET Web 窗体应用程序迁移到 Blazor 几乎肯定需要更新身份验证和授权的执行方式，假设应用程序已配置身份验证。 本章介绍如何从 ASP.NET Web 窗体通用提供程序模型（ () 成员资格、角色和用户配置文件）迁移，以及如何使用应用 ASP.NET Core 标识 Blazor 。 虽然本章将介绍高级步骤和注意事项，但在引用的文档中可以找到详细步骤和脚本。

## <a name="aspnet-universal-providers"></a>ASP.NET 通用提供程序

自 ASP.NET 2.0 起，ASP.NET Web 窗体平台支持多种功能（包括成员身份）的提供程序模型。 通用成员资格提供程序与可选的角色提供程序一起经常与 ASP.NET Web 窗体应用程序一起部署。 它提供一种强大且安全的方式来管理身份验证和授权。 这些通用 [提供程序的](https://www.nuget.org/packages/Microsoft.AspNet.Providers)最新产品/服务可作为 NuGet 包、

通用提供程序使用包括、、和之类的表的 SQL 数据库 `aspnet_Applications` 架构 `aspnet_Membership` `aspnet_Roles` `aspnet_Users` 。 当通过运行 [aspnet_regsql.exe 命令](/previous-versions/ms229862(v=vs.140))进行配置时，提供程序将安装表和存储过程，这些表和存储过程提供了使用基础数据所需的所有查询和命令。 数据库架构和这些存储过程与较新 ASP.NET Identity 和 ASP.NET Core 标识系统不兼容，因此必须将现有数据迁移到新系统。 图1显示了为通用提供程序配置的示例表架构。

![通用提供程序架构](./media/security/membership-tables.png)

通用提供程序负责处理用户、成员资格、角色和配置文件。 将为用户分配全局唯一标识符，并在表中存储用户 Id、用户名等基本信息。 `aspnet_Users` 身份验证信息（如密码、密码格式、密码 salt、锁定计数器和详细信息等）存储在表中 `aspnet_Membership` 。 角色只包含名称和唯一标识符，这些标识符通过关联表分配给用户 `aspnet_UsersInRoles` ，提供多对多的关系。

如果现有系统除了使用成员身份外，还会使用角色，则需要将用户帐户、关联的密码、角色和角色成员身份迁移到 ASP.NET Core 标识。 你还可能需要使用 if 语句来更新你当前正在执行角色检查的代码，而不是使用声明性筛选器、特性和/或标记帮助程序。 我们将在本章末尾更详细地查看迁移注意事项。

### <a name="authorization-configuration-in-web-forms"></a>Web 窗体中的授权配置

若要在 ASP.NET Web 窗体应用程序中配置对某些页面的授权访问，通常指定匿名用户无法访问某些页面或文件夹。 此配置在 web.config 文件中完成：

```xml
<?xml version="1.0"?>
<configuration>
    <system.web>
      <authentication mode="Forms">
        <forms defaultUrl="~/home.aspx" loginUrl="~/login.aspx"
          slidingExpiration="true" timeout="2880"></forms>
      </authentication>

      <authorization>
        <deny users="?" />
      </authorization>
    </system.web>
</configuration>
```

" `authentication` 配置" 部分设置应用程序的窗体身份验证。 此 `authorization` 部分用于禁止对整个应用程序的匿名用户使用。 不过，你可以基于每个位置提供更精细的授权规则，并应用基于角色的授权检查。

```xml
<location path="login.aspx">
  <system.web>
    <authorization>
      <allow users="*" />
    </authorization>
  </system.web>
</location>
```

上述配置（与第一个配置结合使用时）将允许匿名用户访问登录页，并覆盖未经身份验证的用户的站点范围限制。

```xml
<location path="/admin">
  <system.web>
    <authorization>
      <allow roles="Administrators" />
      <deny users="*" />
    </authorization>
  </system.web>
</location>
```

以上配置在与其他配置结合使用时， `/admin` 会将文件夹及其内的所有资源限制为 "Administrators" 角色的成员。 还可以通过在 `web.config` 文件夹根目录中放置单独的文件来应用此限制 `/admin` 。

### <a name="authorization-code-in-web-forms"></a>Web 窗体中的授权代码

除了使用配置访问之外 `web.config` ，还可以通过编程方式配置 Web 窗体应用程序中的访问和行为。 例如，你可以限制执行特定操作的功能，或根据用户的角色查看特定数据。

此代码既可以在代码隐藏逻辑中使用，也可以在页面本身中使用：

```html
<% if (HttpContext.Current.User.IsInRole("Administrators")) { %>
  <a href="/admin">Go To Admin</a>
<% } %>
```

除了检查用户角色成员身份外，还可以确定是否已对其进行身份验证 (不过，这通常是使用以上) 的基于位置的配置来实现的。 下面是此方法的一个示例。

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!User.Identity.IsAuthenticated)
    {
        FormsAuthentication.RedirectToLoginPage();
    }
    if (!Roles.IsUserInRole(User.Identity.Name, "Administrators"))
    {
        MessageLabel.Text = "Only administrators can view this.";
        SecretPanel.Visible = false;
    }
}
```

在上面的代码中，基于角色的访问控制 (RBAC) 用于根据当前用户的角色确定页面的某些元素（如 `SecretPanel` ）是否可见。

通常情况下，ASP.NET Web 窗体应用程序配置文件中的安全性 `web.config` ，然后在页面中需要的位置 `.aspx` 和相关的代码隐藏文件中添加其他检查 `.aspx.cs` 。 大多数应用程序都使用通用成员资格提供程序，通常使用其他角色提供程序。

## <a name="aspnet-core-identity"></a>ASP.NET Core 标识

尽管仍需要身份验证和授权，但与通用提供程序相比，ASP.NET Core 标识将使用一组不同的抽象和假设。 例如，新的标识模型支持第三方身份验证，允许用户使用社交媒体帐户或其他受信任的身份验证提供程序进行身份验证。 ASP.NET Core 标识支持通常需要的页的 UI，如登录、注销和注册。 它利用 EF Core 来访问其数据，并使用 EF Core 迁移来生成支持其数据模型所需的必需架构。 本 [ASP.NET Core 上的标识简介](/aspnet/core/security/authentication/identity) 提供了 ASP.NET Core 标识中包含的内容以及如何开始使用它。 如果你尚未在应用程序及其数据库中设置 ASP.NET Core 标识，则会帮助你入门。

### <a name="roles-claims-and-policies"></a>角色、声明和策略

通用提供程序和 ASP.NET Core 标识都支持角色的概念。 你可以为用户创建角色并将用户分配到角色。 用户可以属于任意数量的角色，你可以验证角色成员身份作为授权实现的一部分。

除了角色外，ASP.NET Core 标识还支持声明和策略的概念。 虽然角色应专门对应于该角色中的用户应能够访问的一组资源，但声明只是用户标识的一部分。 声明是表示使用者的名称值对，而不是主题可执行的操作。

可以直接检查用户的声明，并根据这些值确定是否应向用户授予对资源的访问权限。 但是，此类检查通常是重复性的，并分散在整个系统中。 更好的方法是定义 *策略*。

授权策略包括一个或多个要求。 在的方法中，将策略注册为授权服务配置的一部分 `ConfigureServices` `Startup.cs` 。 例如，以下代码段将配置名为 "CanadiansOnly" 的策略，该策略要求用户具有值为 "加拿大" 的国家/地区声明。

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("CanadiansOnly", policy => policy.RequireClaim(ClaimTypes.Country, "Canada"));
});
```

你可以 [在文档中了解有关如何创建自定义策略的详细信息](/aspnet/core/security/authorization/policies)。

无论你使用的是策略还是角色，你都可以指定应用程序中的特定页面 Blazor 要求具有属性的角色或策略 `[Authorize]` ，并将其应用于 `@attribute` 指令。

需要角色：

```csharp
@attribute [Authorize(Roles ="administrators")]
```

要求满足策略：

```csharp
@attribute [Authorize(Policy ="CanadiansOnly")]
```

如果需要在代码中访问用户的身份验证状态、角色或声明，可以通过两种主要方式实现此功能。 第一种方式是接收身份验证状态作为级联参数。 第二种是使用注入的来访问状态 `AuthenticationStateProvider` 。 [ Blazor 安全文档](/aspnet/core/blazor/security/)中介绍了上述每种方法的详细信息。

下面的代码演示如何将 `AuthenticationState` 作为级联参数接收：

```csharp
[CascadingParameter]
private Task<AuthenticationState> authenticationStateTask { get; set; }
```

使用此参数时，可以使用以下代码获取用户：

```csharp
var authState = await authenticationStateTask;
var user = authState.User;
```

下面的代码演示如何注入 `AuthenticationStateProvider` ：

```csharp
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider
```

提供程序准备就绪后，可以使用以下代码获取用户的访问权限：

```csharp
AuthenticationState authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
ClaimsPrincipal user = authState.User;

if (user.Identity.IsAuthenticated)
{
  // work with user.Claims and/or user.Roles
}
```

**注意：**`AuthorizeView`本章稍后介绍的组件提供了一种声明性方法来控制用户在页面或组件上看到的内容。

若要在服务器应用程序中使用用户和声明 (Blazor) 你可能还需要注入 `UserManager<T>` (用于 `IdentityUser` 默认) ，可用于枚举和修改用户的声明。 首先，注入类型并将其分配给属性：

```csharp
@inject UserManager<IdentityUser> MyUserManager
```

然后使用它来处理用户的声明。 下面的示例演示如何在用户上添加和保存声明：

```csharp
private async Task AddCountryClaim()
{
    var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
    var user = authState.User;
    var identityUser = await MyUserManager.FindByNameAsync(user.Identity.Name);

    if (!user.HasClaim(c => c.Type == ClaimTypes.Country))
    {
        // stores the claim in the cookie
        ClaimsIdentity id = new ClaimsIdentity();
        id.AddClaim(new Claim(ClaimTypes.Country, "Canada"));
        user.AddIdentity(id);

        // save the claim in the database
        await MyUserManager.AddClaimAsync(identityUser, new Claim(ClaimTypes.Country, "Canada"));
    }
}
```

如果需要使用角色，请遵循相同的方法。 可能需要注入 `RoleManager<T>` `IdentityRole` 默认类型的 () ，以便列出和管理角色本身。

**注意：** 在 Blazor WebAssembly 项目中，您将需要提供服务器 api 来 (执行这些操作，而不是使用 `UserManager<T>` 或 `RoleManager<T>` 直接) 。 BlazorWebAssembly 客户端应用程序可以通过安全地调用为此目的而公开的 API 终结点来管理声明和/或角色。

### <a name="migration-guide"></a>迁移指南

从 ASP.NET Web 窗体和通用提供程序迁移到 ASP.NET Core 标识需要几个步骤：

1. 在目标数据库中创建 ASP.NET Core 标识数据库架构
2. 将数据从通用提供程序架构迁移到 ASP.NET Core 标识架构
3. 将配置从迁移 `web.config` 到中间件和服务，通常为 `Startup.cs`
4. 使用控件和条件更新单个页面，使用标记帮助程序和新的标识 Api。

以下部分详细介绍了这些步骤。

### <a name="creating-the-aspnet-core-identity-schema"></a>创建 ASP.NET Core 标识架构

有多种方法可以创建用于 ASP.NET Core 标识的必要的表结构。 最简单的办法就是创建一个新的 ASP.NET Core Web 应用程序。 选择 "Web 应用程序"，然后将 "身份验证" 更改为使用单个用户帐户。

![具有单独用户帐户的新项目](./media/security/individual-user-accounts.png)

在命令行中，可以通过运行来执行相同的操作 `dotnet new webapp -au Individual` 。 应用创建后，运行它并在站点上注册。 应会触发如下所示的页面：

![应用迁移页面](./media/security/apply-migrations-page.png)

单击 "应用迁移" 按钮，应创建必要的数据库表。 此外，迁移文件应显示在你的项目中，如下所示：

![迁移文件](./media/security/migration-files.png)

你可以使用此命令行工具自行运行迁移，而无需运行 web 应用程序：

```powershell
dotnet ef database update
```

如果要运行脚本将新架构应用到现有数据库，可以从命令行编写这些迁移的脚本。 运行以下命令以生成脚本：

```powershell
dotnet ef migrations script -o auth.sql
```

上述命令将在输出文件中生成一个 SQL 脚本 `auth.sql` ，然后，可以针对所需的任何数据库运行该脚本。 如果运行命令时遇到任何问题 `dotnet ef` ，请 [确保系统上已安装 EF Core 工具](/ef/core/miscellaneous/cli/dotnet)。

如果源表上有其他列，则需要在新架构中确定这些列的最佳位置。 通常， `aspnet_Membership` 应将表中的列映射到 `AspNetUsers` 表。 上的列 `aspnet_Roles` 应映射到 `AspNetRoles` 。 表中的任何其他列 `aspnet_UsersInRoles` 都将添加到 `AspNetUserRoles` 表中。

还需要考虑在单独的表中放置任何其他列。 这样，以后的迁移就不需要考虑默认标识架构的自定义。

### <a name="migrating-data-from-universal-providers-to-aspnet-core-identity"></a>将数据从通用提供程序迁移到 ASP.NET Core 标识

准备好目标表架构后，下一步是将用户和角色记录迁移到新的架构。 架构差异的完整列表，包括映射到哪些新列的列，可在 [此处](/aspnet/core/migration/proper-to-2x/membership-to-core-identity)找到。

若要将用户从成员身份迁移到新的标识表，你应 [按照文档中所述的步骤进行操作](/aspnet/core/migration/proper-to-2x/membership-to-core-identity)。 完成这些步骤和提供的脚本后，用户将需要在下次登录时更改其密码。

可以迁移用户密码，但过程更多。 要求用户在迁移过程中更新其密码，并鼓励他们使用全新的唯一密码，这可能会增强应用程序的整体安全性。

### <a name="migrating-security-settings-from-webconfig-to-startupcs"></a>将 web.config 的安全设置迁移到 Startup.cs

如上所述，ASP.NET 成员身份和角色提供程序在应用程序的文件中进行配置 `web.config` 。 由于 ASP.NET Core 应用未绑定到 IIS 并使用单独的系统进行配置，因此，必须在其他位置配置这些设置。 大多数情况下，在文件中配置 ASP.NET Core 标识 `Startup.cs` 。 打开之前创建的 web 项目 (生成标识表架构) 并查看其 `Startup.cs` 文件。

默认的 ConfigureServices 方法添加对 EF Core 和标识的支持：

```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();

    services.AddRazorPages();
}
```

`AddDefaultIdentity`扩展方法用于将标识配置为使用默认值 `ApplicationDbContext` 和框架的 `IdentityUser` 类型。 如果使用的是自定义 `IdentityUser` ，请确保在此处指定其类型。 如果这些扩展方法在你的应用程序中不起作用，请检查你是否具有适当的 using 语句，以及你是否具有必要的 NuGet 包引用。 例如，你的项目应具有的某些版本 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 和 `Microsoft.AspNetCore.Identity.UI` 引用的包。

此外，在中 `Startup.cs` ，你应看到为站点配置的必要中间件。 具体来说 `UseAuthentication` ， `UseAuthorization` 应在适当的位置设置和。

```csharp

// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

ASP.NET Identity 不会从匿名或基于角色的访问中配置位置 `Startup.cs` 。 需要将任何特定于位置的授权配置数据迁移到 ASP.NET Core 中的筛选器。 请注意需要此类更新的文件夹和页面。 你将在下一节中进行这些更改。

### <a name="updating-individual-pages-to-use-aspnet-core-identity-abstractions"></a>更新单个页面以使用 ASP.NET Core 标识抽象化

在 ASP.NET Web 窗体应用程序中，如果您有 `web.config` 一些设置拒绝匿名用户访问某些页面或文件夹，则可以通过将 `[Authorize]` 属性添加到此类页面来迁移这些更改：

```razor
@attribute [Authorize]
```

如果你进一步拒绝了对属于特定角色的用户的访问，则你也可以通过添加指定角色的属性来迁移此行为：

```razor
@attribute [Authorize(Roles ="administrators")]
```

此 `[Authorize]` 属性仅适用于 `@page` 通过路由器访问的组件 Blazor 。 特性不适用于子组件，而子组件应改用 `AuthorizeView` 。

如果在页标记中有用于确定是否向特定用户显示某些代码的逻辑，则可以将其替换为 `AuthorizeView` 组件。 [AuthorizeView 组件](/aspnet/core/blazor/security#authorizeview-component)根据用户是否有权查看它来有选择地显示 UI。 它还公开了 `context` 可用于访问用户信息的变量。

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you are authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You are not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

可以通过从配置为属性的中访问用户，来访问过程逻辑中的身份验证状态 `Task<AuthenticationState` `[CascadingParameter]` 。 此配置将使你可以访问用户，这可以让你确定他们是否经过身份验证以及是否属于特定角色。 如果需要评估策略过程，可以注入实例， `IAuthorizationService` 并对 `AuthorizeAsync` 其调用方法。 下面的示例代码演示如何获取用户信息，并允许授权用户执行策略限制的任务 `content-editor` 。

```razor
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the
            // 'content-editor' policy.
        }
    }
}
```

`AuthenticationState`首先需要将其设置为级联值，然后才能将其绑定到此类级联参数。 通常使用组件完成此操作 `CascadingAuthenticationState` 。 此配置通常在中完成 `App.razor` ：

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData"
                DefaultLayout="@typeof(MainLayout)" />
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```

## <a name="summary"></a>总结

Blazor 使用与 ASP.NET Core 相同的安全模型，ASP.NET Core 标识。 从通用提供程序迁移到 ASP.NET Core 标识相对简单，假设不太太多自定义应用到了原始数据架构。 数据迁移完成后，在应用中使用身份验证和授权 Blazor ，可配置并提供对大多数安全要求的编程支持。

## <a name="references"></a>参考

- [ASP.NET Core 上的标识简介](/aspnet/core/security/authentication/identity)
- [从 ASP.NET 成员身份验证迁移到 ASP.NET Core 2.0 标识](/aspnet/core/migration/proper-to-2x/membership-to-core-identity)
- [将身份验证和标识迁移到 ASP.NET Core](/aspnet/core/migration/identity)
- [ASP.NET Core Blazor 身份验证和授权](/aspnet/core/blazor/security/)

>[!div class="step-by-step"]
>[上一页](config.md)
>[下一页](migration.md)
