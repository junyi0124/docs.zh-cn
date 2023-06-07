---
title: .NET Framework 4 迁移问题
description: 了解 .NET Framework 3.5 Service Pack 1 和 .NET Framework 4 之间的迁移问题，包括标准合规性和安全性的修复和更改。
ms.date: 05/02/2017
helpviewer_keywords:
- .NET Framework 4, migration
- application compatibility
ms.assetid: df478548-8c05-4de2-8ba7-adcdbe1c2a60
ms.openlocfilehash: 50355fd5a502771ce88e1f329ec7effdaf380d48
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90679424"
---
# <a name="net-framework-4-migration-issues"></a>.NET Framework 4 迁移问题

本文描述 .NET Framework 3.5 Service Pack 1 和 .NET Framework 4 之间的迁移问题，包括针对标准符合性和安全性的修复和更改以及基于客户反馈的更改。 这些更改中的大多数更改不需要在应用程序中进行任何编程修改。 有关可能涉及这些修改的更改，请参阅表的“建议的更改”一列。 按区域划分的显著更改，例如，ASP.NET 和 Windows Presentation Foundation (WPF)。

有关本文中问题的更高级概述，请参阅 [.NET Framework 4 的迁移指南](/previous-versions/dotnet/netframework-4.0/ff657133(v=vs.100))。

有关新增功能的信息，请参阅 [.NET Framework 4 中的新增功能](/previous-versions/dotnet/netframework-4.0/ms171868(v=vs.100))。

## <a name="aspnet-and-web"></a>ASP.NET 和 Web

命名空间：<xref:System.Web>、<xref:System.Web.Mobile>、<xref:System.Web.Security>、<xref:System.Web.UI.WebControls>

程序集：System.Web（在 System.Web.dll 中）

| 功能  | 与 3.5 SP1 的差异 | 建议的更改 |
| -------- | ------------------------ | ------------------- |
| **浏览器定义文件** | 浏览器定义文件已更新，以包括有关新的和已更新的浏览器和设备的信息。 旧式浏览器和设备（如 Netscape Navigator）已被删除，并且已添加更新的浏览器和设备（如 Google Chrome 和 Apple iPhone）。<br><br>如果应用程序包含的自定义浏览器定义继承自一个已删除的浏览器定义，将会出现错误。<br><br><xref:System.Web.HttpBrowserCapabilities> 对象（由页的 `Request.Browse` 属性公开）由浏览器定义文件驱动。 因此，通过在 ASP.NET 4 中访问此对象的属性返回的信息可能与早期版本的 ASP.NET 中已返回的信息不同。 | 如果应用程序依赖于旧式浏览器定义文件，可从以下文件夹复制这些文件：<br><br>Windows\\Microsoft.NET\\Framework\\v2.0.50727\\CONFIG\\Browsers<br><br>将这些文件复制到 ASP.NET 4 对应的 \\CONFIG\\Browsers 文件夹中。 复制这些文件之后，请运行 [Aspnet_regbrowsers.exe](/previous-versions/dotnet/netframework-3.5/ms229858(v=vs.90)) 命令行工具。 有关详细信息，请参阅 [https://www.asp.net/mobile](/aspnet/mobile/overview) 网站。 |
| **在混合版本的 ASP.NET 下运行的子应用程序** | 由于配置或编译错误，配置为运行 ASP.NET 早期版本的应用程序子级的 ASP.NET 4 应用程序可能无法启动。 发生的具体错误取决于应用程序是在 IIS 6.0、IIS 7 还是 IIS 7.5 下运行。 | 可对受影响应用程序的配置文件进行更改，以便配置系统能正确识别 ASP.NET 4 应用程序。 有关必须进行的更改的信息，请参阅 ASP.NET 网站上的 [ASP.NET 4 Breaking Changes](/aspnet/whitepapers/aspnet4/breaking-changes)（ASP.NET 4 重大更改）文档中“ASP.NET 4 子应用程序在 ASP.NET 2.0 或 ASP.NET 3.5 应用程序下启动失败”一节。 |
| **ClientID 更改** | 利用 ASP.NET 4 中的新 `clientIDMode` 设置，可指定 ASP.NET 生成 HTML 元素的 `id` 特性的方式。 在 ASP.NET 的早期版本中，默认行为与 `clientIDMode` 的 `AutoID` 设置等效。 现在的默认设置为 `Predictable`。 有关详细信息，请参阅 [ASP.NET Web 服务器控件标识](/previous-versions/aspnet/1d04y8ss(v=vs.100))。 | 如果使用 Visual Studio 从 ASP.NET 2.0 或 ASP.NET 3.5 升级应用程序，则此工具会自动向 Web.config 文件添加设置，保留早期版本的 .NET Framework 的行为。 但是，如果通过在 IIS 中将应用程序池更改为面向 .NET Framework 4 来升级应用程序，ASP.NET 将默认使用新的模式。 若要禁用新的客户端 ID 模式，请将以下设置添加到 Web.config 文件：<br><br>`<pages clientIDMode="AutoID" />` |
| **代码访问安全性 (CAS)** | ASP.NET 3.5 中已添加的 ASP.NET 2.0 NET 功能使用 .NET Framework 1.1 和 .NET Framework 2.0 代码访问安全性 (CAS) 模型。 但是，实质上已对 ASP.NET 4 中的 CAS 实现进行了检查。 因此，依赖于在全局程序集缓存中运行的受信任代码的部分信任 ASP.NET 应用程序可能失败，并引发各种安全异常。 此外，依赖于对计算机 CAS 策略进行的大量修改的部分信任应用程序也可能失败，并引发安全异常。 | 可通过使用 `trust` 配置元素中的新 `legacyCasModel` 特性，将部分信任 ASP.NET 4 应用程序还原为 ASP.NET 1.1 和 2.0 的行为，如下面的示例所示：<br><br>`<trust level= "Medium" legacyCasModel="true" />`<br><br>重要提示：还原为早期 CAS 模型可能会降低安全性。<br><br>有关新 ASP.NET 4 代码访问安全性模型的详细信息，请参阅 [ASP.NET 4 应用程序中的代码访问安全性](/previous-versions/dd984947(v=vs.100))。 |
| **配置文件** | .NET Framework 和 ASP.NET 4 的根配置文件（machine.config 文件和根 Web.config 文件）已经更新，包含在 ASP.NET 3.5 的应用程序 Web.config 文件中找到的大多数样本配置信息。 由于托管 IIS 7 和 IIS 7.5 配置系统的复杂性，在 ASP.NET 4 下以及在 IIS 7 和 IIS 7.5 下运行 ASP.NET 3.5 应用程序会导致 ASP.NET 出错或 IIS 出错。 | 使用 Visual Studio 中的项目升级工具将 ASP.NET 3.5 应用程序升级到 ASP.NET 4。 Visual Studio 2010 自动修改 ASP.NET 3.5 应用程序的 Web.config 文件，以包含 ASP.NET 4 的相应设置。<br><br>不过，可使用 .NET Framework 4 运行 ASP.NET 3.5 应用程序，而无需重新编译。 在此情况下，可能必须先手动修改应用程序的 Web.config 文件，然后才能在 .NET Framework 4 和 IIS 7 或 IIS 7.5 下运行应用程序。 必须进行的特定更改取决于所使用的软件组合，包括 Service Pack (SP) 版本。 有关受此更改影响的可能软件组合以及如何解决特定组合相关问题的信息，请参阅 ASP.NET 网站上的 [ASP.NET 4 Breaking Changes](/aspnet/whitepapers/aspnet4/breaking-changes)（ASP.NET 4 重大更改）文档中“有关新的 ASP.NET 4 根配置的配置错误”一节。 |
| **控件呈现** | 在 ASP.NET 的早期版本中，某些控件发出了无法禁用的标记。 默认情况下，ASP.NET 4 中不再生成此类标记。 呈现更改将影响下列控件：<br><br>\* `Image` 和 `ImageButton` 控件不再呈现 `border="0"` 特性。<br>\* 默认情况下，`BaseValidator` 类和派生自该类的验证控件不再呈现红色文本。<br>\* `HtmlForm` 控件不呈现 `name` 特性。<br>\* `border="0"` 控件不再呈现 `Table` 特性。<br><br>对于并不设计用于用户输入的控件（例如，`Label` 控件），如果其 `Enabled` 属性设置为 `false`（或者它们从容器控件继承此设置），则这些控件不再呈现 `disabled="disabled"` 特性。 | 如果使用 Visual Studio 从 ASP.NET 2.0 或 ASP.NET 3.5 升级应用程序，此工具会自动向 Web.config 文件添加设置，保留旧的呈现方式。 但是，如果通过在 IIS 中将应用程序池更改为面向 .NET Framework 4 来升级应用程序，ASP.NET 默认使用新的呈现模式。 若要禁用新的呈现模式，请将以下设置添加到 Web.config 文件：<br><br>`<pages controlRenderingCompatibilityVersion="3.5" />` |
| **默认文档中的事件处理程序** | 在对无扩展的 URL 发出请求（此 URL 具有已映射到它的默认文档）时，ASP.NET 4 会将 HTML `form` 元素的 `action` 特性呈现为空字符串。 在 ASP.NET 的早期版本中，对 `http://contoso.com` 的请求会导致对 Default.aspx 的请求。 在该文档中，开始 `form` 标记的呈现方式如以下示例所示：<br><br>`<form action="Default.aspx" />`<br><br>在 ASP.NET 4 中，对 `http://contoso.com` 的请求还会导致对 Default.aspx 的请求，但此时 ASP.NET 会按以下示例所示方式呈现 HTML 开始 `form` 标记：<br><br>`<form action="" />`<br><br>当 `action` 特性为空字符串时，IIS `DefaultDocumentModule` 对象将创建对 Default.aspx 的子请求。 在大多数情况下，此子请求对于应用程序代码是透明的，并且 Default.aspx 页会正常运行。 但托管代码和 IIS 7 或 IIS 7.5 集成模式之间可能的交互会导致托管 .aspx 页在子请求期间停止正常工作。 如果出现以下情况，则对默认 .aspx 文档的子请求将导致发生错误或意外行为：<br><br>\* 将 .aspx 页发送到其 `form` 元素的 `action` 特性设置为 "" 的浏览器。<br>\* 将窗体回发到 ASP.NET。<br>\* 托管 HTTP 模块读取实体正文的某一部分，如 `Request.Form` 或 `Request.Params`。 这会使 POST 请求的实体正文读入托管内存中。 因此，实体正文不再对在 IIS 7 或 IIS 7.5 集成模式中运行的任何本机代码模块可用。<br>\* IIS `DefaultDocumentModule` 对象最终运行并创建对 Default.aspx 文档的子请求。 但由于一段托管代码已读取实体正文，因此没有可发送给子请求的实体正文。<br>\* 在对子请求运行 HTTP 管道时，将在处理程序执行阶段运行 .aspx 文件的处理程序。<br><br>由于没有实体正文，因此不存在窗体变量和视图状态。 这样一来，便没有可供 .aspx 页处理程序用来确定应引发哪个事件（如果有）的信息。 因此，不会运行针对受影响 .aspx 页的任何回发事件处理程序。 | 有关解决可能因为此更改而引发的问题的方法，请参阅 ASP.NET 网站上的 [ASP.NET 4 Breaking Changes](/aspnet/whitepapers/aspnet4/breaking-changes)（ASP.NET 4 重大更改）文档中“可能会在 IIS 7 或 IIS 7.5 集成模式下的默认文档中引发的事件处理程序”。 |
| **哈希算法** | ASP.NET 使用加密算法和哈希算法来帮助保护数据（如窗体身份验证 Cookie 和视图状态）。 默认情况下，ASP.NET 4 使用 <xref:System.Security.Cryptography.HMACSHA256> 算法对 Cookie 和视图状态进行哈希操作。 早期版本的 ASP.NET 使用较早的 <xref:System.Security.Cryptography.HMACSHA1> 算法。 | 如果运行混合了 ASP.NET 2.0 和 ASP.NET 4 的应用程序，其中的数据（如窗体身份验证 Cookie）必须跨多个 .NET Framework 版本工作，请通过在 Web.config 文件中添加以下设置，将 ASP.NET 4 Web 应用程序配置为使用较早的 <xref:System.Security.Cryptography.HMACSHA1> 算法：<br><br>`<machineKey validation="SHA1" />` |
| **在 Internet Explorer 中托管控件** | 无法再使用 Internet Explorer 来托管 Windows 窗体控件，因为可通过更优的解决方案在 Web 上托管控件。 因此，已从 .NET Framework 中删除 IEHost.dll 和 IEExec.exe 程序集。 | 可使用以下技术在 Web 应用程序中进行自定义控件开发：<br><br>\* 可创建 Silverlight 应用程序并将其配置为在浏览器外部运行。 有关详细信息，请参阅[浏览器外支持](/previous-versions/windows/silverlight/dotnet-windows-silverlight/dd550721(v=vs.95))。<br>\* 可生成 XAML 浏览器应用程序 (XBAP) 来利用 WPF 功能（需要客户端计算机上装有 .NET Framework）。 有关详细信息，请参阅 [WPF XAML 浏览器应用程序概述](/dotnet/desktop/wpf/app-development/wpf-xaml-browser-applications-overview)。 |
| **HtmlEncode 和 UrlEncode 方法** | <xref:System.Web.HttpUtility> 和 <xref:System.Web.HttpServerUtility> 类的 `HtmlEncode` 和 `UrlEncode` 方法已更新，现可按如下方式对单引号字符 (') 进行编码：<br><br>\* `HtmlEncode` 方法会将单引号的实例编码为 `&#39;`<br>\* `UrlEncode` 方法会将单引号的实例编码为 `%27` | 检查使用 `HtmlEncode` 和 `UrlEncode` 方法的位置的代码，并确保对编码进行的更改不会导致将影响应用程序的更改。 |
| **ASP.NET 2.0 应用程序中的 HttpException 错误** | 在 IIS 6 上启用 ASP.NET 4 后，IIS 6 上运行的 ASP.NET 2.0 应用程序（在 Windows Server 2003 或 Windows Server 2003 R2 中）可能会生成错误，如下所示：`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.` | * 如果无需 ASP.NET 4 即可运行网站，请重新映射该网站以改用 ASP.NET 2.0。<br><br>\- 或 -<br><br>\* 如果需要 ASP.NET 4 才能运行网站，请将所有子 ASP.NET 2.0 虚拟目录移至映射到 ASP.NET 2.0 的其他网站。<br><br>\- 或 -<br><br>\* 禁用无扩展 URL。 有关详细信息，请参阅 ASP.NET 网站上的 [ASP.NET 4 Breaking Changes](/aspnet/whitepapers/aspnet4/breaking-changes)（ASP.NET 4 重大更改）文档中“ASP.NET 2.0 应用程序可能生成引用 eurl.axd 的 HttpException 错误”。 |
| **成员资格类型** | 已将 ASP.NET 成员资格中使用的某些类型（例如 <xref:System.Web.Security.MembershipProvider>）从 System.Web.dll 移至 System.Web.ApplicationServices.dll 程序集。 移动这些类型是为了解析客户端中的类型与扩展的 .NET Framework SKU 中的类型之间的体系结构层依赖关系。 | 对于已从早期版本的 ASP.NET 升级的类库以及使用已移动的成员资格类型的类库，在 ASP.NET 4 项目中使用这些类库时，可能无法编译它们。 如果是这样，请在类库项目中添加对 System.Web.ApplicationServices.dll 的引用。 |
| **Menu 控件更改** | 对 <xref:System.Web.UI.WebControls.Menu> 控件进行更改会导致发生以下行为：<br><br>\* 如果 <xref:System.Web.UI.WebControls.MenuRenderingMode> 设置为 `List`，或者 <xref:System.Web.UI.WebControls.MenuRenderingMode> 设置为 `Default`，且 <xref:System.Web.Configuration.PagesSection.ControlRenderingCompatibilityVersion> 设置为 `4.0` 或更高版本，则 <xref:System.Web.UI.WebControls.MenuItem.PopOutImageUrl> 属性不起作用。<br>\* 如果在 <xref:System.Web.UI.WebControls.Menu.StaticPopOutImageUrl%2A> 和 <xref:System.Web.UI.WebControls.Menu.DynamicPopOutImageUrl> 属性中设置的路径包含反斜杠 (\\)，则图像不会呈现。 （在 ASP.NET 的早期版本中，路径可包含反斜杠。） | * 设置父 <xref:System.Web.UI.WebControls.Menu> 控件的 <xref:System.Web.UI.WebControls.Menu.StaticPopOutImageUrl%2A> 或 <xref:System.Web.UI.WebControls.Menu.DynamicPopOutImageUrl>，而不是设置单个菜单项的 <xref:System.Web.UI.WebControls.MenuItem.PopOutImageUrl> 属性。<br><br>\- 或 -<br><br>将 <xref:System.Web.UI.WebControls.MenuRenderingMode> 设置为 `Table`，或者将 <xref:System.Web.UI.WebControls.MenuRenderingMode> 设置为 `Default`，并将 <xref:System.Web.Configuration.PagesSection.ControlRenderingCompatibilityVersion> 设置为 `3.5`。 这些设置会导致 <xref:System.Web.UI.WebControls.Menu> 控件使用在早期版本的 ASP.NET 中使用的基于 HTML 表的布局。<br>\* 如果 <xref:System.Web.UI.WebControls.Menu.StaticPopOutImageUrl%2A> 或 <xref:System.Web.UI.WebControls.Menu.DynamicPopOutImageUrl> 属性中的路径包含反斜杠 (\\)，请替换为斜杠字符 (/)。 |
| **Web.config 文件中的移动程序集** | 在早期版本的 ASP.NET 中，对 System.Web.Mobile.dll 程序集的引用位于根 Web.config 文件 `assemblies` 节中的 `system.web`/`compilation` 下。 为了提高性能，已删除对此程序集的引用。<br><br>注意：System.Web.Mobile.dll 程序集和 ASP.NET 移动控件包含在 ASP.NET 4 中，但它们已被弃用。 | 若要使用此程序集中的类型，请在根 Web.config 文件或应用程序 Web.config 文件中添加对此程序集的引用。 |
| **输出缓存** | 在 ASP.NET 1.0 中，Bug 会导致缓存页（这些页指定 `Location="ServerAndClient"` 作为输出&euro;缓存设置）在响应中发出 `Vary:*` HTTP 标头。 这还能起到告知客户端浏览器绝不要本地对页进行缓存的作用。 ASP.NET 1.1 中已添加 <xref:System.Web.HttpCachePolicy.SetOmitVaryStar%2A> 方法，可调用此方法来禁止显示 `Vary:*` 标头。 但 Bug 报告表明开发人员不了解现有 `SetOmitVaryStar` 行为。<br><br>在 ASP.NET 4 中，不再从指定以下指令的响应中发出 `Vary:*` HTTP 标头：<br><br>`<%@ OutputCache Location="ServerAndClient" %>`<br><br>因此，不再需要使用 <xref:System.Web.HttpCachePolicy.SetOmitVaryStar%2A> 方法即可禁止显示 `Vary:*` 标头。 在为 `Location` 特性指定“ServerAndClient”的应用程序中，可在浏览器中对页进行缓存而无需调用 <xref:System.Web.HttpCachePolicy.SetOmitVaryStar%2A>。 | 如果应用程序中的页必须发出 `Vary:*`，请调用 <xref:System.Web.HttpResponse.AppendHeader%2A> 方法，如以下示例所示：<br><br>`System.Web.HttpResponse.AppendHeader("Vary","*");`<br><br>或者，可将输出缓存 `Location` 特性的值更改为“Server”。 |
| **页分析** | ASP.NET 4 中针对 ASP.NET 网页（.aspx 文件）和用户控件（.aspx 文件）的页分析器不仅比早期版本的 ASP.NET 中的页分析器更为严格，而且会将更多的标记设为无效。 | 检查在页运行时产生的错误消息并纠正因无效标记导致的错误。 |
| **Passport 类型** | 由于 Passport（现在为 Live ID SDK）中的更改，ASP.NET 2.0 中内置的 Passport 支持已过时且不受支持。 因此，现在用 `ObsoleteAttribute` 特性标记 <xref:System.Web.Security> 中与 Passport 相关的类型。 | 更改使用 <xref:System.Web.Security> 命名空间中的 Passport 类型（例如 <xref:System.Web.Security.PassportIdentity>）的所有代码，以使用 Windows Live ID SDK。 |
| **FilePath 属性中的 PathInfo 信息** | ASP.NET 4 不再将 `PathInfo` 值加入从属性（如 <xref:System.Web.HttpRequest.FilePath>、<xref:System.Web.HttpRequest.AppRelativeCurrentExecutionFilePath> 和 <xref:System.Web.HttpRequest.CurrentExecutionFilePath>）返回的值中。 而是在 <xref:System.Web.HttpRequest.PathInfo> 中提供 `PathInfo` 信息。 例如，假定以下 URL 片段：<br><br>`/testapp/Action.mvc/SomeAction`<br><br>在 ASP.NET 的早期版本中，<xref:System.Web.HttpRequest> 属性具有以下值：<br><br>\* <xref:System.Web.HttpRequest.FilePath>: `/testapp/Action.mvc/SomeAction`<br>\* <xref:System.Web.HttpRequest.PathInfo>: (empty)<br><br>而在 ASP.NET 4 中，<xref:System.Web.HttpRequest> 属性具有以下值：<br><br>\* <xref:System.Web.HttpRequest.FilePath>: `/testapp/Action.mvc`<br>\* <xref:System.Web.HttpRequest.PathInfo>: `SomeAction` | 检查所依赖 <xref:System.Web.HttpRequest> 类的属性所在位置的代码以返回路径信息；更改代码以反映对返回路径信息的方式的更改。 |
| **请求验证** | 为了改进请求验证，将在请求生命周期中提早调用 ASP.NET 请求验证。 因此，将为不是针对 .aspx 文件的请求（如针对 Web 服务调用和自定义处理程序的请求）运行请求验证。 此外，在请求处理管道中运行自定义 HTTP 模块时，请求验证也处于活动状态。<br><br>进行此更改后，针对 .aspx 文件之外的资源的请求可能会引发请求验证错误。 在请求管道（例如，自定义 HTTP 模块）中运行的自定义代码也可能会引发请求验证错误。 | 如有必要，可通过使用 Web 配置文件中的以下设置，还原为仅让 .aspx 页触发请求验证这一旧行为：<br><br>`<httpRuntime requestValidationMode="2.0" />`<br><br>警告：如果还原为旧行为，请确保现有处理程序中的所有代码、模块和其他自定义代码对潜在不安全的 HTTP 输入（可能是 XSS 攻击途径）进行检查。 |
| **路由** | 如果在 Visual Studio 2010 中创建一个文件系统网站，并且该网站位于其名称包含点 (.) 的文件夹中，则 URL 路由不会可靠地工作。 从某些虚拟路径返回 HTTP 404 错误。 发生此情况的原因是，Visual Studio 2010 使用错误的根虚拟目录路径启动了 Visual Studio 开发服务器。 | * 在基于文件的网站的“属性”页中，将“虚拟路径”特性更改为“/”。<br><br>\- 或 -<br><br>\* 创建 Web 应用程序项目而非网站项目。 Web 应用程序项目不会出现此问题，并且 URL 路由会正常工作，即使项目文件夹的名称中包含点也是如此。<br><br>\- 或 -<br><br>\* 创建在 IIS 中托管的基于 HTTP 的网站。 IIS 托管的网站可在虚拟路径和项目文件文件夹中包含点。 |
| **SharePoint 站点** | 如果尝试运行部署为 SharePoint 网站（包含名为 `WSS_Minimal` 的自定义部分信任级别）的子级的 ASP.NET 4 网站，将出现以下错误：<br><br>`Could not find permission set named 'ASP.Net'.` | 目前，所有版本的 SharePoint 都与 ASP.NET 不兼容。 因此，不应尝试将 ASP.NET 4 网站作为 SharePoint 网站的子级运行。 |
| **XHTML 1.1 标准** | 为了实现新网站的 XHTML 1.1 符合性，.NET Framework 4 中的 ASP.NET 控件将生成符合 XHTML 1.1 的 HTML。 使用 `<system.Web>` 元素内 Web.config 文件中的以下选项可启用此呈现：<br><br>`<pages controlRenderingCompatibilityVersion="4.0"/>`<br><br>默认情况下，此选项设置为 4.0。 从 Visual Studio 2008 进行升级的 Web 项目启用 3.5 设置以实现兼容性。 | 无。 |

## <a name="core"></a>核心

### <a name="general-features"></a>常规功能

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **CardSpace** | Windows CardSpace 不再包含在 .NET Framework 中，而是单独提供。 | 从 [Microsoft 下载中心](https://go.microsoft.com/fwlink/?LinkId=199868)下载 Windows CardSpace。 |
| **配置文件** | 已对 .NET Framework 访问应用程序配置文件的方式进行了纠正。 | 如果应用程序配置文件名为 application-name.config，则将它重命名为 application-name.exe.config。例如，将 MyApp.config 重命名为 MyApp.exe.config。 |
| **C# 代码编译器** | <xref:Microsoft.CSharp> 命名空间中的 `Compiler`、`CompilerError` 和 `ErrorLevel` 类不再可用，并且其程序集 (cscompmgd.dll) 不再包含在 .NET Framework 中。 | 使用 <xref:System.CodeDom.Compiler.CodeDomProvider> 类和 <xref:System.CodeDom.Compiler> 命名空间中的其他类。 有关详细信息，请参阅[使用 CodeDOM](/previous-versions/dotnet/netframework-4.0/y2k85ax6(v=vs.100))。 |
| **承载**（非托管 API） | 为了改进承载功能，一些承载激活 API 已被弃用。 利用进程内并行执行功能，应用程序能够在同一个进程中加载和启动多个版本的 .NET Framework。 例如，可运行在同一进程中加载基于 .NET Framework 2.0 SP1 的外接程序（或组件）和基于 .NET Framework 4 的外接程序的应用程序。 较旧组件可继续使用 .NET Framework 的较旧版本，新组件则使用 .NET Framework 的新版本。 | 使用[进程内并行执行](../deployment/in-process-side-by-side-execution.md)中描述的配置。 |
| **新安全模型** | 代码访问安全性 (CAS) 策略已关闭并替换为简化模型，如 [.NET Framework 4 中的安全性更改](/previous-versions/dotnet/netframework-4.0/dd233103(v=vs.100))中所述。 | 如果依赖于应用程序中的 CAS，则可能需要做出一些修改。 有关详细信息，请参阅[代码访问安全策略兼容性和迁移](../misc/code-access-security-policy-compatibility-and-migration.md)。 |

### <a name="date-and-time"></a>日期和时间

命名空间：<xref:System>

程序集：mscorlib（在 mscorlib.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **夏时制** | 为了与系统时钟保持一致，时间属性（如 <xref:System.TimeZoneInfo.Local> 和 <xref:System.DateTime.Now>）现在使用操作系统规则而非其他 .NET Framework 数据进行夏时制操作。 | 无。 |
| **字符串格式设置** | 为了支持区分区域性的格式设置，<xref:System.TimeSpan> 结构包含了 `ToString`、`Parse` 和 `TryParse` 方法的新重载，以及新的 `ParseExact` 和 `TryParseExact` 方法。 | 无。 |

### <a name="globalization"></a>全球化

有关新的非特定区域性和特定区域性的列表，请参阅[全球化和本地化中的新增功能](/previous-versions/dotnet/netframework-4.0/dd997383(v=vs.100))。

命名空间：<xref:System.Globalization>

程序集：mscorlib（在 mscorlib.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **区域性名称** | 以下名称更改影响德语、迪维希语和非洲语区域性：<br><br>\* <xref:System.Globalization.CultureAndRegionInfoBuilder.CurrencyEnglishName>：德语（瑞士）(de-CH) 区域性的货币名称已从“sFr.”更改 为“Fr.”。<br>\* <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern>：迪维希语（马尔代夫）(dv-MV) 区域性的长日期模式已从“dd/MMMM/yyyy”更改为“dd/MM/yyyy”。<br>\* <xref:System.Globalization.DateTimeFormatInfo.PMDesignator>：此 南非荷兰语（南非）(af-ZA) 区域性的 P.M. 指示符已从“nm”更改为“PM”。 | 注意区域性名称更改。 |
| **LCID 参数** | 为了与自动化服务器设置中的预期行为保持一致，CLR 不再将 `LCID` 参数的当前区域性传递给基于 COM 的非托管应用程序。 而是为区域性传递 1033 (en-us)。 | 无需进行任何修改，只不过本机应用程序需要指定的区域性。 |
| **已过时的区域性类型** | <xref:System.Globalization.CultureTypes> 和 <xref:System.Globalization.CultureTypes> 区域性类型现已过时。<br><br>为了实现向后兼容，<xref:System.Globalization.CultureTypes> 现返回 .NET Framework 早期版本中附带的非特定区域性和特定区域性，而 <xref:System.Globalization.CultureTypes> 现返回一个空列表。 | 使用 <xref:System.Globalization.CultureTypes> 枚举的其他值。 |
| **检索区域性** | 自 Windows 7 开始，.NET Framework 4 从操作系统检索区域性信息，而不是存储数据本身。 此外，.NET Framework 与 Windows 同步，以便对数据进行排序并设置其大小写。 | 无。 |
| **Unicode 5.1 标准** | .NET Framework 目前支持所有 Unicode 5.1 字符，外加大约 1400 个字符。 这些额外的字符包括新符号、箭头、音调符号、标点、数学符号、CJK 笔划和象形文字、其他马拉雅拉姆语和泰卢固语数字字符以及各种缅甸语、拉丁语、阿拉伯语、希腊语、蒙古语和西里尔语字符。 Unicode 5.1 支持以下新文种：巽他语、列普查语、欧甘语、瓦依语、索拉什拉特语、克耶黎语、勒姜语、果鲁穆奇语、奥里雅语、泰米尔语、泰卢固语、马拉雅拉姆语字符和占语。 | 无。 |

### <a name="exceptions"></a>异常

命名空间：<xref:System><xref:System.Runtime.ExceptionServices>

程序集：mscorlib（在 mscorlib.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **损坏进程状态的异常** | CLR 不再将损坏进程状态的异常传递给托管代码中的异常处理程序。 | 这些异常指示进程状态已损坏。 建议不要在此状态下运行应用程序。<br><br>有关详细信息，请参阅 <xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute> 和 MSDN 杂志中的条目[处理损坏状态异常](/archive/msdn-magazine/2009/february/clr-inside-out-handling-corrupted-state-exceptions)。 |
| **执行引擎异常** | <xref:System.ExecutionEngineException> 现已过时，因为可捕获的异常将允许不稳定的进程继续运行。 此更改提高了运行时中的可预见性和可靠性。 | 使用 <xref:System.InvalidOperationException> 来表示此情况。 |

### <a name="reflection"></a>反射

命名空间：<xref:System.Reflection>

程序集：mscorlib（在 mscorlib.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **程序集哈希算法** | <xref:System.Reflection.AssemblyName.HashAlgorithm> 属性现返回 <xref:System.Configuration.Assemblies.AssemblyHashAlgorithm>，因为在加载程序集时运行时不知道所引用的程序集的哈希算法。 （这表示使用所引用的程序集（如由 <xref:System.Reflection.Assembly.GetReferencedAssemblies%2A> 方法返回的程序集）的属性。） | 无。 |
| **程序集加载** | 为了阻止过多加载程序集并节省虚拟地址空间，CLR 现仅通过使用 Win32 `MapViewOfFile` 函数来加载程序集。 此外，它不再调用 `LoadLibrary` 函数。<br><br>此更改以以下方式影响诊断应用程序：<br><br>\* <xref:System.Diagnostics.ProcessModuleCollection> 将不再包含通过调用 `Process.GetCurrentProcess().Modules` 获取的类库（.dll 文件）中的任何模块。<br>\* 使用 `EnumProcessModules` 函数的 Win32 应用程序将看不到列出的所有托管模块。 | 无。 |
| **声明类型** | 当类型没有声明类型时，<xref:System.Type.DeclaringType> 属性现将正确返回 null。 | 无。 |
| **委托** | 在将 null 值传递给委托的构造函数时，委托现将引发 <xref:System.ArgumentNullException> 而不是 <xref:System.NullReferenceException>。 | 确保任何异常处理捕获 <xref:System.ArgumentNullException>。 |
| **全局程序集缓存位置更改** | 对于 .NET Framework 4 程序集，已将全局程序集缓存从 Windows 目录 (%WINDIR%) 移动到 Microsoft.Net 子目录 (%WINDIR%\\Microsoft.Net)。 早期版本中的程序集保留在较早的目录中。<br><br>非托管 [ASM_CACHE_FLAGS](../unmanaged-api/fusion/asm-cache-flags-enumeration.md) 枚举包含新的 `ASM_CACHE_ROOT_EX` 标志。 此标志可获得 .NET Framework 4 程序集的缓存位置，可通过 [GetCachePath](../unmanaged-api/fusion/getcachepath-function.md) 函数获取此位置。 | 无，假定应用程序不使用程序集的显式路径（不建议这样做）。 |
| **全局程序集缓存工具** | [Gacutil.exe（全局程序集缓存工具）](/previous-versions/dotnet/netframework-4.0/ex0ss12c(v=vs.100))不再支持 shell 插件查看器。 | 无。 |

### <a name="interoperability"></a>互操作性

命名空间：<xref:System.Runtime.InteropServices>

程序集：mscorlib（在 mscorlib.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **缓冲区长度**（非托管 API） | 为了节省内存，已更改 [ICorProfilerInfo2::GetStringLayout](../unmanaged-api/profiling/icorprofilerinfo2-getstringlayout-method.md) 方法的 `pBufferLengthOffset` 参数的功能，以匹配 `pStringLengthOffset` 参数。 这两个参数目前都指向字符串长度的偏移量位置。 已从字符串类的表示形式中删除缓冲区长度。 | 删除缓冲区长度的任何依赖项。 |
| **JIT 调试** | 为了简化实时 (JIT) 调试的注册过程，.NET Framework 调试器现仅使用 AeDebug 注册表项，此注册表项可控制本机代码的 JIT 调试行为。 此更改产生以下结果：<br><br>\* 无法再为托管代码和本机代码注册两个不同的调试器。<br>\* 无法再为非交互式进程自动启动调试器，但可就交互式进程提示用户。<br>\* 当调试器无法启动或当没有应启动的任何注册的调试器时，不再收到通知。<br>\* 不再支持依赖于应用程序交互性的自动启动策略。 | 根据需要调整调试操作。 |
| **平台调用** | 为了改进与非托管代码的互操作性的性能，平台调用中不正确的调用约定会导致应用程序失败。 在早期版本中，封送层在堆栈中向上解析这些错误。 | 在 Microsoft Visual Studio 中调试应用程序时，将向用户通知这些错误，以便能够纠正它们。<br><br>如果二进制文件无法更新，可将 [\<NetFx40_PInvokeStackResilience>](../configure-apps/file-schema/runtime/netfx40-pinvokestackresilience-element.md) 元素包含在应用程序的配置文件中，以便能够在早期版本中的堆栈上解析这些调用错误。 但这可能会影响应用程序的性能。 |
| **移除的接口**（非托管 API） | 为了避免开发人员产生困惑，已移除以下接口，因为这些接口不提供任何有用的运行时方案，并且 CLR 不提供或接受任何实现：<br><br>\* **INativeImageINativeImageDependency**<br>\* **INativeImageInstallInfo**<br>\* **INativeImageEvaluate**<br>\* **INativeImageConverter**<br>\* **ICorModule**<br>\* **IMetaDataConverter** | 无。 |

## <a name="data"></a>数据

本节介绍有关使用数据集和 SQL 客户端、实体框架、LINQ to SQL 和 WCF 数据服务器（以前称作 ADO.NET 数据服务）的迁移问题。

### <a name="dataset-and-sql-client"></a>数据集和 SQL 客户端

下表介绍了对之前具有限制或其他问题的功能的改进。

命名空间：<xref:System.Data>、<xref:System.Data.Objects.DataClasses>、<xref:System.Data.SqlClient>

程序集：System.Data（在 System.Data.dll 中）、System.Data.Entity（在 System.Data.Entity.dll 中）

| 功能 | 与 3.5 SP1 的差异 |
| ------- | ------------------------ |
| **POCO 方案** | <xref:System.Data.Objects.DataClasses.IRelatedEnd> 接口具有的新方法提高了它在普通旧 CLR 对象 (POCO) 方案中的可用性。 这些新方法采用 <xref:System.Object> 而非 <xref:System.Data.Objects.DataClasses.IEntityWithRelationships> 实体作为参数。 |
| **编辑行** | 由 <xref:System.Data.DataView> 类实现的 <xref:System.Collections.IList.IndexOf%2A> 方法现正确返回要编辑的行的值，而不是返回 -1。 |
| **事件** | 在某个行处于已修改状态并调用了 <xref:System.Data.DataRow.RejectChanges%2A> 方法时，现引发 <xref:System.Data.DataRowView.PropertyChanged> 事件。 此更改使创建用于公开 <xref:System.Data.DataSet> 对象的内容的 UI 控件变得更容易。 |
| **异常** | 在未设置或打开连接时，<xref:System.Data.SqlClient.SqlCommand.Prepare%2A> 方法现引发 <xref:System.InvalidOperationException> 而非 <xref:System.NullReferenceException>。 |
| **映射视图** | 现在，在设计时捕获查询视图映射错误，而不是在运行时引发 <xref:System.NullReferenceException>。<br><br>映射验证现在会捕获总体架构 (CSDL) 中的两个关联集映射到同一列这一错误。 |
| **事务** | 如果应用程序尝试在完成事务（包括中止或回退）后对连接执行某个语句，现将引发 <xref:System.InvalidOperationException>。 早期版本不会引发异常，并允许执行其他命令，即使事务已中止也是如此。 |

### <a name="entity-framework"></a>Entity Framework

下表介绍了对之前具有限制或其他问题的功能的改进。

命名空间：<xref:System.Data>、<xref:System.Data.Objects>、<xref:System.Data.Objects.DataClasses>

程序集：System.Data.Entity（在 System.Data.Entity.dll 中）

| 功能 | 与 3.5 SP1 的差异 |
| ------- | ------------------------ |
| **实体对象** | 在调用 <xref:System.Data.Objects.ObjectContext.SaveChanges%2A> 方法时，<xref:System.Data.Objects.ObjectContext.Detach%2A> 方法和实体对象的状态之间现存在奇偶校验。 这可提高一致性，防止引发意外的异常。 |
| **实体 SQL** | 已针对实体 SQL 中的标识符解析改进规则。<br><br>实体 SQL 分析器已改进用于解析多部分标识符的逻辑。 |
| **结构化批注** | 实体框架现可识别结构化批注。 |
| **查询** | 已在查询中进行以下改进：<br><br>\* 对空集合使用空键的 `GroupBy` 查询不会返回任何行，不管查询中是否有任何其他选择。<br>\* 默认情况下，LINQ 和 Entity-SQL 查询中生成的 SQL 会将字符串参数视为非 Unicode 值。 |

### <a name="linq-to-sql"></a>LINQ to SQL

下表介绍了对之前具有限制或其他问题的功能的改进。

命名空间：<xref:System.Data.Linq>

程序集：System.Data.Linq（在 System.Data.Linq.dll 中）

| 功能 | 与 3.5 SP1 的差异 |
| ------- | ------------------------ |
| **事件** | 除了在加载 <xref:System.Data.Linq.EntitySet%601> 时会引发 <xref:System.Data.Linq.EntitySet%601.ListChanged> 事件外，卸载时，<xref:System.Data.Linq.EntitySet%601> 集合现在也会引发该事件以进行添加和删除操作。 |
| **查询** | LINQ to SQL 查询中不再忽略 `Skip(0)`。 因此，具有此方法的查询的行为可能不同。 例如，在某些情况下，`OrderBy` 子句对于 `Skip(0)` 是必需的，如果未包含 `OrderBy` 子句，则查询现将引发 <xref:System.NotSupportedException> 异常。 |

### <a name="wcf-data-services"></a>WCF 数据服务

下表介绍了对之前具有限制或其他问题的功能的改进。

命名空间：<xref:System.Data.Services>、<xref:System.Data.Services.Client>、<xref:System.Data.Services.Common>、<xref:System.Data.Services.Providers>

程序集：System.Data.Services（在 System.Data.Services.dll 中）、System.Data.Services.Client（在 System.Data.Services.Client.dll 中）

| 功能 | 与 3.5 SP1 的差异 |
| ------- | ------------------------ |
| **批处理二进制内容** | WCF 数据服务现支持请求和响应中的批处理二进制内容。 |
| **更改侦听器** | 现在会为删除请求执行更改侦听器。<br><br>更改侦听器是一个方法，此方法在服务器收到修改实体集中某个实体的请求时运行。 此方法在执行传入请求之前运行。 更改侦听器提供对要更改的实体和要对该实体执行的操作的访问权限。 |
| **异常** | 以下情况现引发更有用的异常而不是 <xref:System.NullReferenceException>：<br><br>\* 如果对数据服务的调用超时，引发 <xref:System.ServiceProcess.TimeoutException>。<br>\* 如果对数据服务发出错误请求，引发 <xref:System.Data.Services.Client.DataServiceRequestException>。<br><br>应更改应用程序中的异常处理方式以捕获新的异常。 |
| **标头** | 已对标头进行以下改进：<br><br>\* WCF Data Services 现会正确地拒绝具有未指定的值的 `eTag` 标头。<br>\* WCF Data Services 现将返回一个错误，并且不会执行针对链接的删除请求的请求（如果该请求中包含 `if-*` 标头）。<br>\* WCF Data Services 现以客户端在 Accept 标头中指定的格式（Atom、JSON）将错误返回给客户端。 |
| **JSON 读取器** | 如果 JavaScript 对象表示法 (JSON) 读取器在处理发送到 WCF 数据服务的 JSON 负载时读取到单个反斜杠（“\\”）转义字符，它现将正确返回一个错误。 |
| **合并** | 已对 <xref:System.Data.Services.Client.MergeOption> 枚举进行以下改进：<br><br>\* <xref:System.Data.Services.Client.MergeOption> 合并选项不再修改客户端上的实体作为来自数据服务的任何后续响应的结果。<br>\* <xref:System.Data.Services.Client.MergeOption> 选项目前在动态 SQL 和已存储的基于过程的更新之间保持一致。 |
| **请求** | 在处理对数据服务的请求前，现在会调用 <xref:System.Data.Services.DataService%601.OnStartProcessingRequest%2A> 方法。 这可使该请求对 <xref:System.Data.Services.Providers.ServiceOperation> 服务正常工作。 |
| **流** | WCF 数据服务不再为读取和写入操作关闭基础流。 |
| **URI** | 已纠正 WCF 数据服务客户端对 URI 进行的转义。 |

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

下表介绍了对之前具有限制或其他问题的功能的改进。

| 功能 | 与 3.5 SP1 的差异 |
| ------- | ------------------------ |
| **配置文件** | 为了能够通过配置文件层次结构继承行为，WCF 现支持跨配置文件进行合并。<br><br>现已扩展配置继承模型，允许用户定义将应用于计算机上运行的所有服务的行为。<br><br>如果在层次结构的不同级别上存在同名的行为，则可能会遇到行为更改。 |
| **服务承载** | 通过向元素定义添加特性 `allowDefinition="MachineToApplication"`，可不再指定服务级别的 `<serviceHostingEnvironment>` 配置元素。<br><br>从技术上说，指定服务级别的 `<serviceHostingEnvironment>` 元素是错误的，会导致不一致的行为。 |

## <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

### <a name="applications"></a>应用程序

命名空间：<xref:System.Windows><xref:System.Windows.Controls>

程序集：PresentationFramework（在 PresentationFramework.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **异常处理** | 为了能够提前检测错误，WPF 引发 <xref:System.Reflection.TargetInvocationException> 并将 <xref:System.Exception.InnerException> 属性设置为严重异常（例如 <xref:System.NullReferenceException>、<xref:System.OutOfMemoryException>、<xref:System.StackOverflowException> 和 <xref:System.Security.SecurityException>），而不是捕获原始异常。 | 无。 |
| **链接的资源** | 为了简化链接，位于项目文件夹结构以外的资源文件（例如图像）在生成应用程序时使用其完整路径（而不仅是其文件名）作为资源 ID。 应用程序就能够在运行时找到这些文件。 | 无。 |
| **部分信任应用程序** | 为了安全起见，如果基于 Windows 的应用程序以部分信任的方式运行，并包含带有 HTML 的 <xref:System.Windows.Controls.WebBrowser> 控件或 <xref:System.Windows.Controls.Frame> 控件，则该应用程序会在创建该控件时引发 <xref:System.Security.SecurityException>。<br><br>如果满足以下所有条件，则浏览器应用程序引发异常并显示一条消息：<br><br>\* 该应用程序在 Firefox 中运行。<br>\* 该应用程序在来自不受信任站点的 Internet 区域中以部分信任的方式运行。<br>\* 该应用程序包含带有 HTML 的 <xref:System.Windows.Controls.WebBrowser> 控件或 <xref:System.Windows.Controls.Frame> 控件。<br><br>从受信任站点或 Intranet 区域运行的应用程序不会受到影响。 | 在浏览器应用程序中，可通过执行下列操作之一来减轻此更改的影响：<br><br>\* 以完全信任的方式运行浏览器应用程序。<br>\* 让客户将该应用程序的站点添加到受信任的站点区域。<br>\* 让客户在 Internet Explorer 中运行该应用程序。 |
| **资源字典** | 为了改进主题级资源字典并防止它们发生更改，在资源字典中定义且合并到主题级别字典中的可冻结资源现始终标记为已冻结，因而是不可变的。 这是可冻结资源的预期行为。 | 修改主题级别合并字典中所定义资源的应用程序应克隆该资源，并修改克隆的副本。 或者，也可将该资源标记为 `x:Shared="false"`，以便每次查询该资源时 <xref:System.Windows.ResourceDictionary> 都会创建新副本。 |
| **Windows 7** | 为了使 WPF 应用程序更适用于 Windows 7，已做出以下改进以纠正窗口的行为：<br><br>\* 停靠和笔势状态现基于用户交互按预期方式工作。<br>\* 任务栏命令 “层叠窗口”、“堆叠显示窗口”和“并排显示窗口”现具有正确的行为并更新了相应的属性。<br>\* 最大化或最小化窗口的 `Top`、`Left`、`Width` 和 `Height` 属性现包含窗口的正确还原位置，而不是其他值（具体取决于监视器）。 | 无。 |
| **窗口样式和透明度** | 当 <xref:System.Windows.Window.AllowsTransparency> 为 `true` 且 <xref:System.Windows.WindowState> 为 <xref:System.Windows.WindowState> 时，如果尝试将 <xref:System.Windows.Window.WindowStyle> 设置为 <xref:System.Windows.WindowStyle> 以外的值，则会引发 <xref:System.InvalidOperationException>。 | 当 <xref:System.Windows.Window.AllowsTransparency> 为 `true` 时，如果必须更改 <xref:System.Windows.Window.WindowStyle>，则可以调用 Win32 `SetWindowLongPtr` 函数。 |
| **XPS 查看器** | WPF 不包含 Microsoft XML 纸张规范 Essentials Pack (XPSEP)。 Windows 7 和 Windows Vista 附带 XPSEP。<br><br>在运行 Windows XP 但未安装 .NET Framework 3.5 SP1 的计算机上，使用 <xref:System.Windows.Controls.PrintDialog> 中以外的 WPF API 进行打印将依赖于 WINSPOOL。 有些打印机功能不会被报告出来，有些打印机设置也不会在打印期间应用。 | 如果需要，请安装 Microsoft XML 纸张规范 Essentials Pack。 |

### <a name="controls"></a>控件

命名空间：<xref:System.Windows>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>

程序集：PresentationFramework（在 PresentationFramework.dll 中）、PresentationCore（在 PresentationCore.dll 中）和 WindowsBase（在 WindowsBase.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **对话框** | 为了提高可靠性，对创建了 <xref:Microsoft.Win32.FileDialog> 控件的同一线程调用 <xref:Microsoft.Win32.CommonDialog.ShowDialog%2A> 方法。 | 确保创建 <xref:Microsoft.Win32.FileDialog> 控件并对同一线程调用 <xref:Microsoft.Win32.CommonDialog.ShowDialog%2A> 方法。 |
| **浮动窗口** | 为了修复错误地保持对浮点窗口的重新激活（使窗口看上去像一个模式对话框）的焦点还原逻辑，如果候选项不是窗口的子级，现将会阻止焦点还原。 | 无。 |
| **集合中的项** | 当在基础集合中移动或添加某一项时，如果 <xref:System.Windows.Data.CollectionView> 未进行排序，则该项会显示在 <xref:System.Windows.Data.CollectionView> 中的同一相对位置。 这会确保集合中项的位置与关联的 <xref:System.Windows.Data.CollectionView> 之间的一致性。 | 使用 <xref:System.Windows.Controls.ItemContainerGenerator.ContainerFromItem%2A> 或 <xref:System.Windows.Data.CollectionView.IndexOf%2A> 方法查找项在 <xref:System.Windows.Data.CollectionView> 中的位置，而不是依赖于项的固定位置。 |
| **布局** | 为了消除不必要的重新布局，更改 <xref:System.Windows.Controls.Page.ShowsNavigationUI> 不再会使布局失效或导致执行另一个布局处理过程。 | 如果预期更改 <xref:System.Windows.Controls.Page.ShowsNavigationUI> 会导致执行另一个布局处理过程，请在设置该属性后调用 <xref:System.Windows.UIElement.InvalidateVisual%2A>。 |
| **菜单** | 为了在弹出菜单中启用 ClearType 文本，已对 <xref:System.Windows.Controls.ControlTemplate> 类和 <xref:System.Windows.Controls.MenuItem> 控件以及其他控件做出修改。 | 应用程序不应依赖于控件模板的可视化结构。 只有 <xref:System.Windows.Controls.ControlTemplate> 的已命名部分才包含在公共协定中。 如果应用程序必须在 <xref:System.Windows.Controls.ControlTemplate> 中查找某个对象，则应在可视化树中搜索特定类型的对象，而不是依赖于对象在该树中的固定位置。 |
| **导航** | 如果 <xref:System.Windows.Controls.Frame> 直接导航到某一位置，则在初次导航后 <xref:System.Windows.Navigation.NavigatingCancelEventArgs.IsNavigationInitiator> 属性为 `true`。 此更改阻止在启动方案中引发其他事件。 | 无。 |
| **弹出窗口** | 在布局处理过程中现可多次调用 <xref:System.Windows.Controls.Primitives.CustomPopupPlacementCallback> 委托，而不是仅调用一次。 | 如果 <xref:System.Windows.Controls.Primitives.CustomPopupPlacementCallback> 委托根据其以前的位置计算 <xref:System.Windows.Controls.Primitives.Popup> 的位置，则仅当 `popupSize`、`targetSize` 或 `offset` 参数的值发生更改时才需重新计算该位置。 |
| **属性值** | <xref:System.Windows.DependencyObject.SetCurrentValue%2A> 方法现允许将属性设置为有效值，但它将继续遵从影响该属性的任何绑定、样式或触发器。 | 只要属性值因另一种操作（包括用户操作）的副作用而发生变化，控件作者就应使用 <xref:System.Windows.DependencyObject.SetCurrentValue%2A>。 |
| **文本框** | 为了安全起见，在以部分信任的方式调用 <xref:System.Windows.Forms.TextBoxBase.Copy%2A> 和 <xref:System.Windows.Forms.TextBoxBase.Cut%2A> 方法时，这两种方法会失败，且不给出提示。<br><br>此外，在部分信任的方式下，还会禁止以编程方式对继承自 <xref:System.Windows.Controls.Primitives.TextBoxBase> 的控件执行 <xref:System.Windows.Input.ApplicationCommands.Copy> 或 <xref:System.Windows.Input.ApplicationCommands.Cut> 属性。 不过，用户启动的复制和剪切命令（例如，单击其 <xref:System.Windows.Controls.Primitives.ButtonBase.Command> 属性绑定到其中一个命令的按钮）仍有效。 在部分信任方式下，通过键盘快捷方式和上下文菜单执行的标准复制和剪切仍像之前一样有效。 | 将 <xref:System.Windows.Input.ApplicationCommands.Copy> 或 <xref:System.Windows.Input.ApplicationCommands.Cut> 命令绑定到用户启动的操作，例如单击按钮。 |

### <a name="graphics"></a>图形

命名空间：<xref:System.Windows>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>、<xref:System.Windows.Media.Effects>

程序集：PresentationFramework（在 PresentationFramework.dll 中）、PresentationCore（在 PresentationCore.dll 中）和 WindowsBase（在 WindowsBase.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **位图效果** | 为了改进性能，<xref:System.Windows.Media.Effects.BitmapEffect> 类和继承自 <xref:System.Windows.Media.Effects.BitmapEffect> 类的类虽然仍存在，但已被禁用。 如果以下条件为 true，则将使用硬件加速呈现管道呈现该效果：<br><br>\* 应用程序会使用 <xref:System.Windows.Media.Effects.DropShadowBitmapEffect> 或半径属性设置为小于 100 DIU 的 <xref:System.Windows.Media.Effects.BlurBitmapEffect>。<br>\* 运行应用程序的计算机上的视频卡支持像素着色器 2.0。<br><br>如果不满足以上条件，则 <xref:System.Windows.Media.Effects.BitmapEffect> 对象将不起作用。<br><br>此外，Visual Studio 在遇到 <xref:System.Windows.Media.Effects.BitmapEffect> 对象或子类时还会生成一条编译器警告。<br><br><xref:System.Windows.Media.DrawingContext.PushEffect%2A> 方法被标记为已过时。 | 停止使用旧的 <xref:System.Windows.Media.Effects.BitmapEffect> 及派生类，改用派生自 <xref:System.Windows.Media.Effects.Effect> 的新类：<xref:System.Windows.Media.Effects.BlurEffect>、<xref:System.Windows.Media.Effects.DropShadowEffect> 和 <xref:System.Windows.Media.Effects.ShaderEffect>。<br><br>此外，还可通过从 <xref:System.Windows.Media.Effects.ShaderEffect> 类继承，创建自己的效果。 |
| **位图帧** | 克隆的 <xref:System.Windows.Media.Imaging.BitmapFrame> 对象现会接收 <xref:System.Windows.Media.Imaging.BitmapSource.DownloadProgress>、<xref:System.Windows.Media.Imaging.BitmapSource.DownloadCompleted> 和 <xref:System.Windows.Media.Imaging.BitmapSource.DownloadFailed> 事件。 这使从网站下载并通过 <xref:System.Windows.Style> 应用于 <xref:System.Windows.Controls.Image> 控件的图像可正常工作。<br><br>仅当以下所有表述均成立时，才会看到行为更改：<br><br>\* 订阅了 <xref:System.Windows.Media.Imaging.BitmapSource.DownloadProgress>、<xref:System.Windows.Media.Imaging.BitmapSource.DownloadCompleted> 或 <xref:System.Windows.Media.Imaging.BitmapSource.DownloadFailed> 事件。<br>\* <xref:System.Windows.Media.Imaging.BitmapFrame> 的源来自 Web。<br>\* 在下载仍在进行时对 <xref:System.Windows.Media.Imaging.BitmapFrame> 进行了克隆。 | 检查事件处理程序中的发送方，并仅在该发送方为原始 <xref:System.Windows.Media.Imaging.BitmapFrame> 时才采取操作。 |
| **解码图像** | 为了防止在图像无法解码时不会处理 <xref:System.IO.IOException>，<xref:System.Windows.Media.Imaging.BitmapSource> 类将在不解码图像时引发 <xref:System.Windows.Media.Imaging.BitmapSource.DecodeFailed> 事件。 | 删除针对 <xref:System.IO.IOException> 的任何异常处理并使用 <xref:System.Windows.Media.Imaging.BitmapSource.DecodeFailed> 事件检查解码失败。 |

### <a name="input"></a>输入

命名空间：<xref:System.Windows>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>

程序集：PresentationFramework（在 PresentationFramework.dll 中）、PresentationCore（在 PresentationCore.dll 中）和 WindowsBase（在 WindowsBase.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **绑定命令实例** | 为了提供一种机制，用于将基于视图模型的命令实例绑定到基于视图的输入笔势，<xref:System.Windows.Input.InputBinding> 类现从 <xref:System.Windows.Freezable> 而非 <xref:System.Windows.DependencyObject> 继承。 以下属性现为依赖项属性：<br><br>\* <xref:System.Windows.Input.InputBinding.Command><br>\* <xref:System.Windows.Input.InputBinding.CommandParameter><br>\* <xref:System.Windows.Input.InputBinding.CommandTarget><br><br>此更改产生以下结果：<br><br>\* 注册之后，<xref:System.Windows.Input.InputBinding> 对象现处于冻结状态，而不是仍可变。<br>\* 无法从多个线程访问实例级别的 <xref:System.Windows.Input.InputBinding> 对象，因为存在 <xref:System.Windows.DependencyObject> 类的限制。<br>\* 无法在注册类级别的输入绑定后对其进行转换，因为存在 <xref:System.Windows.Freezable> 类的限制。<br>\* 无法对视图模型中创建的命令实例指定输入绑定。 | 如果绑定可变，则在单独的线程上创建 <xref:System.Windows.Input.InputBinding> 类的单独实例，否则请冻结这些绑定。 不要在注册类级别静态 <xref:System.Windows.Input.InputBinding> 后更改它。 |
| **浏览器应用程序** | WPF 浏览器应用程序 (.XBAP) 现使用与独立 WPF 应用程序相同的方式处理键事件，以便对象以正确顺序接收路由键事件。 | 无。 |
| **语音符号键组合** | WPF 会对语音符号键进行模糊处理，这不会生成任何可见字符，而是指示该键将与下一个字母键组合使用以生成一个字符。 通过将 <xref:System.Windows.Input.KeyEventArgs.Key> 属性设置为 <xref:System.Windows.Input.Key> 值，键输入事件（如 <xref:System.Windows.Input.Keyboard.KeyDownEvent> 事件）会在键成为语音符号键时进行报告。 这通常属于预期行为，因为应用程序一般不会对创建组合字符的键盘输入做出响应。 | 期望读取组合字符组成键的应用程序可通过使用 <xref:System.Windows.Input.KeyEventArgs.DeadCharProcessedKey> 属性，获取现经过模糊处理的键。 |
| **焦点管理器** | 如果向 <xref:System.Windows.Input.FocusManager.GetFocusedElement(System.Windows.DependencyObject)?displayProperty=nameWithType> 方法传递将 [IsFocusScope](xref:System.Windows.Input.FocusManager.IsFocusScope%2A) 附加属性设置为 `true` 的元素，那么当且仅当返回的元素与传递给此方法的元素属于同一 <xref:System.Windows.PresentationSource> 对象时，此方法返回的元素才是相应焦点范围内的上一个键盘焦点元素。 | 无。 |

### <a name="ui-automation"></a>UI 自动化

命名空间：<xref:System.Windows>、<xref:System.Windows.Automation.Peers>、<xref:System.Windows.Automation.Provider>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>

程序集：PresentationFramework（在 PresentationFramework.dll 中）、PresentationCore（在 PresentationCore.dll 中）、UIAutomationProvider（在 UIAutomationProvider.dll 中）、WindowsBase（在 WindowsBase.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **视图的类层次结构** | <xref:System.Windows.Automation.Peers.TreeViewAutomationPeer> 和 <xref:System.Windows.Automation.Peers.TreeViewItemAutomationPeer> 类继承自 <xref:System.Windows.Automation.Peers.ItemsControlAutomationPeer> 而不是 <xref:System.Windows.Automation.Peers.FrameworkElementAutomationPeer>。 | 如果继承自 <xref:System.Windows.Automation.Peers.TreeViewItemAutomationPeer> 类并重写 <xref:System.Windows.Automation.Peers.TreeViewItemAutomationPeer.GetChildrenCore%2A> 方法，请考虑返回继承自新 <xref:System.Windows.Automation.Peers.TreeViewDataItemAutomationPeer> 类的对象。 |
| **离开屏幕的容器** | 为了纠正错误的返回值，如果项容器滚动到视图之外，<xref:System.Windows.Automation.Peers.UIElementAutomationPeer.IsOffscreenCore%2A> 方法现正确返回 `false`。 此外，该方法的值不受被其他窗口封闭或元素是否在特定监视器上可见的影响。 | 无。 |
| **菜单和子对象** | 对于包含 <xref:System.Windows.Controls.MenuItem> 对象以外的子级的菜单，为了实现其 UI 自动化，<xref:System.Windows.Automation.Peers.MenuItemAutomationPeer.GetChildrenCore%2A> 方法现返回子 <xref:System.Windows.UIElement> 对象的 <xref:System.Windows.Automation.Peers.AutomationPeer> 对象，而不是 <xref:System.Windows.Automation.Peers.MenuItemAutomationPeer> 对象。 | 无。 |
| **新接口和程序集** | 为了实现新功能的 UI 自动化，添加了以下接口：<br><br>\* <xref:System.Windows.Automation.Provider.IItemContainerProvider><br>\* <xref:System.Windows.Automation.Provider.ISynchronizedInputProvider><br>\* <xref:System.Windows.Automation.Provider.IVirtualizedItemProvider> | 生成 WPF 自动化对等的任何项目都必须添加对 UIAutomationProvider.dll 的显式引用。 |
| **Thumb** | <xref:System.Windows.Automation.Peers.ThumbAutomationPeer.GetClassNameCore%2A> 方法返回一个值，而不是 Null。 因此，继承自 <xref:System.Windows.Controls.Primitives.Thumb> 类的控件（例如 <xref:System.Windows.Controls.GridSplitter>）会将名称报告给 UI 自动化。 | 无。 |
| **虚拟化元素** | 为了改进性能，<xref:System.Windows.Automation.Peers.UIElementAutomationPeer.GetChildrenCore%2A> 方法只返回确实位于可视化树中的子对象而不是所有子对象，无论它们是否已虚拟化。 | 使用 <xref:System.Windows.Automation.ItemContainerPattern> 循环访问 <xref:System.Windows.Automation.Peers.ItemsControlAutomationPeer> 的所有项。 |

### <a name="xaml"></a>XAML

命名空间：<xref:System.Windows>、<xref:System.Windows.Controls>、<xref:System.Windows.Data>、<xref:System.Windows.Input>、<xref:System.Windows.Markup>

程序集：PresentationFramework（在 PresentationFramework.dll 中）、PresentationCore（在 PresentationCore.dll 中）和 WindowsBase（在 WindowsBase.dll 中）

| 功能 | 与 3.5 SP1 的差异 | 建议的更改 |
| ------- | ------------------------ | ------------------- |
| **标记扩展** | 使用标记扩展在集合中设置属性或创建项时，WPF 现始终正确使用 <xref:System.Windows.Markup.MarkupExtension.ProvideValue%2A> 方法中的值，而不是在某些情况下返回 <xref:System.Windows.Markup.MarkupExtension> 对象。 标记扩展在某些情况下可能会返回自身。 | 如果在早期版本中应用程序访问返回了 <xref:System.Windows.Markup.MarkupExtension> 对象的资源，则引用从 <xref:System.Windows.Markup.MarkupExtension.ProvideValue%2A> 返回的对象，而不是 <xref:System.Windows.Markup.MarkupExtension> 对象。 |
| **分析特性** | XAML 中的特性现可只具有一个句点。 例如，以下特性有效：<br><br>`<Button Background="Red"/>`（无句点）<br><br>`<Button Button.Background = "Red"/>`（一个句点）<br><br>以下特性不再有效：<br><br>`<Button Control.Button.Background = "Red"/>`（多个句点） | 更正具有多个句点的 XAML 特性。 |

## <a name="xml"></a>XML

此表中的各行介绍了对之前具有限制或其他问题的功能的改进。

### <a name="schema-and-transforms"></a>架构和转换

命名空间：<xref:System.Xml.Linq>、<xref:System.Xml.Schema>、<xref:System.Xml.XPath>

程序集：System.Xml（在 System.Xml.dll 中）、System.Xml.Linq（在 System.Xml.Linq.dll 中）

| 功能 | 与 3.5 SP1 的差异 |
| ------- | ------------------------ |
| **Chameleon 架构** | 为了防止数据损坏，当 Chameleon 架构随附于多个架构，现可正确克隆。<br><br>Chameleon 架构是一些不具有目标命名空间的架构，当这些架构包含在另一个 XSD 中时，它们采用导入架构的目标命名空间。 它们通常用于将一般类型包含在架构中。 |
| **ID 函数** | 向 XLST 传递 <xref:System.Xml.XmlReader> 对象时，XSLT [id 函数](/sql/xquery/functions-on-sequences-id)现在返回正确的值而不是 Null。<br><br>如果用户使用 <xref:System.Xml.Linq.XNode.CreateReader%2A> 方法从 LINQ to XML 类创建了 <xref:System.Xml.XmlReader> 对象，且已将此 <xref:System.Xml.XmlReader> 对象传递给 XSLT，则 XSLT 中 `id` 函数的任何实例先前返回了 Null。 此值不是 `id` 函数允许的返回值。 |
| **命名空间特性** | 为了防止数据损坏，<xref:System.Xml.XPath.XPathNavigator> 对象现正确返回 `x:xmlns` 特性的本地名称。 |
| **命名空间声明** | 子树中的 <xref:System.Xml.XmlReader> 对象不再在单个 XML 元素中创建重复的命名空间声明。 |
| **架构验证** | 为了阻止错误的架构验证，<xref:System.Xml.Schema.XmlSchemaSet> 类确保 XSD 架构编译的正确性和一致性。 这些架构可包含其他架构；例如，`A.xsd` 可包含 `B.xsd`，而后者又可包含 `C.xsd`。 编译上述任意架构会导致遍历依赖项图。 |
| **脚本函数** | 当 [function-available 函数](/previous-versions/dotnet/netframework-4.0/ms256124(v=vs.100)) 实际可用时，它不再错误返回 `false`。 |
| **URI** | <xref:System.Xml.Linq.XElement.Load%2A> 方法现会在 LINQ 查询中返回正确的 BaseURI。 |

### <a name="validation"></a>验证

命名空间：<xref:System.Xml.Linq>、<xref:System.Xml.Schema>、<xref:System.Xml.XPath>

程序集：System.Xml（在 System.Xml.dll 中）、System.Xml.Linq（在 System.Xml.Linq.dll 中）

| 功能 | 与 3.5 SP1 的差异 |
| ------- | ------------------------ |
| **命名空间解析器** | <xref:System.Xml.XmlReader.ReadContentAs%2A> 方法不再忽略传递给它的 <xref:System.Xml.IXmlNamespaceResolver> 解析器。<br><br>在早期版本中，忽略了指定的命名空间解析器，而改用 <xref:System.Xml.XmlReader>。 |
| **空格** | 为了在创建读取器时防止数据丢失，<xref:System.Xml.XmlReader.Create%2A> 方法不再丢弃有意义的空格。<br><br>XML 验证识别混合内容模式，文本可在其中与 XML 标记混合。 在混合模式下，所有空格都是有意义的，应该报告出来。 |

### <a name="writing"></a>写入

命名空间：<xref:System.Xml.Linq>、<xref:System.Xml.Schema>、<xref:System.Xml.XPath>

程序集：System.Xml（在 System.Xml.dll 中）、System.Xml.Linq（在 System.Xml.Linq.dll 中）

| 功能 | 与 3.5 SP1 的差异 |
| ------- | ------------------------ |
| **实体引用** | 为了防止数据损坏，不再在 XML 特性中对实体引用实体化两次。<br><br>如果用户尝试使用 <xref:System.Xml.XmlWriter.WriteEntityRef%2A> 方法将实体写入 `xmlns` 特性、`xml:lang` 特性或 `xml:space` 特性中，则会在输出中实体化实体两次，从而将损坏数据。 |
| **新行处理** | 为了防止数据损坏，<xref:System.Xml.XmlWriter> 对象遵循 <xref:System.Xml.NewLineHandling> 选项。 |

## <a name="see-also"></a>请参阅

- [.NET Framework 4 中的新类型和成员](/previous-versions/dotnet/netframework-4.0/ff641764(v=vs.100))
- [迁移到 .NET Framework 4 的指南](/previous-versions/dotnet/netframework-4.0/ff657133(v=vs.100))
- [.NET Framework 4 中的新增功能](/previous-versions/dotnet/netframework-4.0/ms171868(v=vs.100))
- [.NET Framework 的版本兼容性](version-compatibility.md)
- [将 Office 解决方案迁移到 .NET Framework 4](/visualstudio/vsto/migrating-office-solutions-to-the-dotnet-framework-4-or-later)
- [.NET Framework 类库中过时的内容](../whats-new/whats-obsolete.md)
