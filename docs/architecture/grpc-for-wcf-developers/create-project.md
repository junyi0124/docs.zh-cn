---
title: 为 WCF 开发人员创建新的 ASP.NET Core gRPC 项目-gRPC
description: 了解如何使用 Visual Studio 或命令行创建 gRPC 项目。
ms.date: 01/06/2021
ms.openlocfilehash: c9d66a773f0633c2ae93c42ce3ce53084032cd17
ms.sourcegitcommit: 7ef96827b161ef3fcde75f79d839885632e26ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/07/2021
ms.locfileid: "97970253"
---
# <a name="create-a-new-aspnet-core-grpc-project"></a>创建新的 ASP.NET Core gRPC 项目

.NET SDK 附带了强大的 CLI 工具， `dotnet` 它使你能够从命令行创建和管理项目和解决方案。 SDK 与 Visual Studio 紧密集成，因此，所有内容也可通过熟悉的图形用户界面获得。 本章介绍了两种创建新 ASP.NET Core gRPC 项目的方法。

## <a name="create-the-project-by-using-visual-studio"></a>使用 Visual Studio 创建项目

> [!IMPORTANT]
> 若要开发任何 ASP.NET Core 5.0 应用，需要安装 **ASP.NET 和 web 开发** 工作负荷的 Visual Studio 2019 版本16.8 或更高版本。

从 *空白解决方案* 模板创建名为 **TraderSys** 的空解决方案。 添加一个名为的解决方案文件夹 `src` 。 然后，右键单击该文件夹，然后选择 "**添加**  >  **新项目**"。 `grpc`在模板搜索框中输入，应会看到名为的项目模板 `gRPC Service` 。

!["添加新项目" 对话框的屏幕截图](media/create-project/new-grpc-project.png)

选择 " **下一步** " 以继续进入 " **配置新项目** " 对话框。 将项目命名 `TraderSys.Portfolios` 为，并将 `src` 子目录添加到该 **位置**。

!["配置新项目" 对话框的屏幕截图](media/create-project/configure-project.png)

选择 " **下一步** " 以继续转到 " **创建新的 gRPC 服务** " 对话框。

!["创建新的 gRPC 服务" 对话框的屏幕截图](media/create-project/create-new-grpc-service-v2.png)

目前，为服务创建提供了有限的选项。 Docker 将在以后推出，因此现在，请将该选项保持未选中状态。 只需选择 " **创建**"。 将生成第一个 ASP.NET Core 5.0 gRPC 项目，并将其添加到解决方案中。 如果你不想了解如何使用 `dotnet CLI` ，请跳到 [清除示例代码](#clean-up-the-example-code) 部分。

## <a name="create-the-project-by-using-the-net-cli"></a>使用 .NET CLI 创建项目

本部分介绍如何从命令行创建解决方案和项目。

创建解决方案，如以下命令所示。 `-o` (或 `--output`) 标志指定将在当前目录中创建的输出目录（如果尚不存在）。 解决方案与目录同名： `TraderSys.sln` 。 可以通过使用 `-n` (或) 标志来提供不同的名称 `--name` 。

```dotnetcli
dotnet new sln -o TraderSys
cd TraderSys
```

ASP.NET Core 5.0 附带了 gRPC services 的 CLI 模板。 使用此模板创建新项目，并将其放入一个 `src` 子目录，而不是 ASP.NET Core 项目的传统。 该项目命名为 () 目录之后 `TraderSys.Portfolios.csproj` ，除非使用该标志指定其他名称 `-n` 。

```dotnetcli
dotnet new grpc -o src/TraderSys.Portfolios
```

最后，使用命令将该项目添加到解决方案中 `dotnet sln` ：

```dotnetcli
dotnet sln add src/TraderSys.Portfolios
```

> [!TIP]
> 由于特定目录只包含一个 `.csproj` 文件，因此，您可以仅指定目录来保存键入内容。

你现在可以在 Visual Studio 2019、Visual Studio Code 或你喜欢的任何编辑器中打开此解决方案。

## <a name="clean-up-the-example-code"></a>清理示例代码

现在，你已使用 gRPC 模板创建了一个示例服务，该模板已在本书前面回顾过。 此代码在我们的股票交易上下文中并不有用，因此我们将为第一个项目编辑一些内容。

### <a name="rename-and-edit-the-proto-file"></a>重命名和编辑协议文件

继续，将该文件重命名 `Protos/greet.proto` 为 `Protos/portfolios.proto` ，并在编辑器中打开它。 删除行后的所有内容 `package` 。 然后更改 `option csharp_namespace` 、 `package` 和 `service` 名称，并删除默认 `SayHello` 服务。 该代码现在如下所示：

```protobuf
syntax = "proto3";

option csharp_namespace = "TraderSys.Portfolios.Protos";

package PortfolioServer;

service Portfolios {
  // RPCs will go here
}
```

> [!TIP]
> 默认情况下，此模板不会添加 `Protos` 命名空间部分，但是添加它可以更轻松地保留 gRPC 生成的类，并且你自己的类在代码中进行了明确分隔。

如果重命名 `greet.proto` 集成开发环境中的文件 (IDE) 如 Visual Studio），则会在文件中自动更新对此文件的引用 `.csproj` 。 但在某些其他编辑器中，如 Visual Studio Code，此引用不会自动更新，因此需要手动编辑项目文件。

在 gRPC 生成目标中，有一个 `Protobuf` item 元素，可让你指定 `.proto` 应编译哪些文件，以及需要哪种类型的代码生成 (即 "服务器" 或 "客户端" ) 。

```xml
<ItemGroup>
  <Protobuf Include="Protos\portfolios.proto" GrpcServices="Server" />
</ItemGroup>
```

### <a name="rename-the-greeterservice-class"></a>重命名 `GreeterService` 类

`GreeterService`类位于 `Services` 文件夹中，并从继承 `Greeter.GreeterBase` 。 将其重命名为 `PortfolioService` ，并将基类更改为 `Portfolios.PortfoliosBase` 。 删除 `override` 方法。

```csharp
public class PortfolioService : Portfolios.PortfoliosBase
{
}
```

类中的方法存在对 `GreeterService` 类的引用 `Configure` `Startup` 。 如果使用了重构来重命名类，则应自动更新此引用。 但是，如果没有，则需要手动编辑。

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapGrpcService<PortfolioService>();
    });
}
```

在下一部分中，我们将向此新服务添加功能。

>[!div class="step-by-step"]
>[上一页](migrate-wcf-to-grpc.md)
>[下一页](migrate-request-reply.md)
