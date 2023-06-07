---
title: 教程：创建 .NET 工具
description: 了解如何创建 .NET 工具。 工具是一个通过使用 .NET CLI 安装的控制台应用程序。
ms.topic: tutorial
ms.date: 02/12/2020
ms.openlocfilehash: 93d0567f3d73707f828f84fad6128804debf6579
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94633773"
---
# <a name="tutorial-create-a-net-tool-using-the-net-cli"></a>教程：使用 .NET CLI 创建 .NET 工具

 本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

本教程介绍如何创建和打包 .NET 工具。 使用 .NET CLI，你可以创建一个控制台应用程序作为工具，便于其他人安装并运行。 .NET 工具是从 .NET CLI 安装的 NuGet 包。 有关工具的详细信息，请参阅 [.NET 工具概述](global-tools.md)。

将创建的工具是一个控制台应用程序，它将消息作为输入，并显示消息以及用于创建机器人图像的文本行。

这是一系列教程（包含三个教程）中的第一个。 在本教程中，将创建并打包工具。 在接下来的两个教程中，[使用该工具作为全局工具](global-tools-how-to-use.md)并[使用该工具作为本地工具](local-tools-how-to-use.md)。

## <a name="prerequisites"></a>先决条件

- [.NET Core SDK 3.1](https://dotnet.microsoft.com/download) 或更高版本。

  本教程和下面的[全局工具教程](global-tools-how-to-use.md)适用于 .NET Core SDK 2.1 及更高版本，因为全局工具从该版本开始可用。 但本教程假定你已安装 3.1 或更高版本，因此你可以选择继续学习[本地工具教程](local-tools-how-to-use.md)。 本地工具从 .NET Core SDK 3.0 开始可用。 无论你是将工具用作全局工具还是用作本地工具，创建工具的过程都是相同的。
  
- 按需选择的文本编辑器或代码编辑器。

## <a name="create-a-project"></a>创建项目

1. 打开命令提示符，创建一个名为“repository”  的文件夹。

1. 导航到“repository”文件夹并输入以下命令  ：

   ```dotnetcli
   dotnet new console -n microsoft.botsay
   ```

   此命令将在“repository”文件夹下创建一个名为“microsoft.botsay”的新文件夹   。

1. 导航到“microsoft.botsay”文件夹  。

   ```console
   cd microsoft.botsay
   ```

## <a name="add-the-code"></a>添加代码

1. 使用代码编辑器打开 `Program.cs` 文件。

1. 将下面的 `using` 指令添加到文件的顶部：

   ```csharp
   using System.Reflection;
   ```

1. 将 `Main` 方法替换为以下代码，以便处理应用程序的命令行参数。

   ```csharp
   static void Main(string[] args)
   {
       if (args.Length == 0)
       {
           var versionString = Assembly.GetEntryAssembly()
                                   .GetCustomAttribute<AssemblyInformationalVersionAttribute>()
                                   .InformationalVersion
                                   .ToString();

           Console.WriteLine($"botsay v{versionString}");
           Console.WriteLine("-------------");
           Console.WriteLine("\nUsage:");
           Console.WriteLine("  botsay <message>");
           return;
       }

       ShowBot(string.Join(' ', args));
   }
   ```

   如果未传递任何参数，将显示简短的帮助消息。 否则，所有参数都将连接到单个字符串中，并通过调用在下一步中创建的 `ShowBot` 方法进行打印。

1. 添加一个名为 `ShowBot` 的新方法，该方法采用一个字符串参数。 该方法使用文本行打印出消息和机器人图像。

   ```csharp
   static void ShowBot(string message)
   {
       string bot = $"\n        {message}";
       bot += @"
       __________________
                         \
                          \
                             ....
                             ....'
                              ....
                           ..........
                       .............'..'..
                    ................'..'.....
                  .......'..........'..'..'....
                 ........'..........'..'..'.....
                .'....'..'..........'..'.......'.
                .'..................'...   ......
                .  ......'.........         .....
                .    _            __        ......
               ..    #            ##        ......
              ....       .                 .......
              ......  .......          ............
               ................  ......................
               ........................'................
              ......................'..'......    .......
           .........................'..'.....       .......
        ........    ..'.............'..'....      ..........
      ..'..'...      ...............'.......      ..........
     ...'......     ...... ..........  ......         .......
    ...........   .......              ........        ......
   .......        '...'.'.              '.'.'.'         ....
   .......       .....'..               ..'.....
      ..       ..........               ..'........
             ............               ..............
            .............               '..............
           ...........'..              .'.'............
          ...............              .'.'.............
         .............'..               ..'..'...........
         ...............                 .'..............
          .........                        ..............
           .....
   ";
       Console.WriteLine(bot);
   }
   ```

1. 保存更改。

## <a name="test-the-application"></a>测试应用程序

运行项目并观察输出。 尝试使用命令行处的这些变体来查看不同的结果：

```dotnetcli
dotnet run
dotnet run -- "Hello from the bot"
dotnet run -- Hello from the bot
```

位于 `--` 分隔符后的所有参数均会传递给应用程序。

## <a name="package-the-tool"></a>打包工具

在将应用程序作为工具打包并分发之前，你需要修改项目文件。

1. 打开“microsoft.botsay.csproj”文件，然后将三个新的 XML 节点添加到 `<PropertyGroup>` 节点的末尾  ：

   ```xml
   <PackAsTool>true</PackAsTool>
   <ToolCommandName>botsay</ToolCommandName>
   <PackageOutputPath>./nupkg</PackageOutputPath>
   ```

   `<ToolCommandName>` 是一个可选元素，用于指定在安装工具后将调用该工具的命令。 如果未提供此元素，则该工具的命令名称为没有“.csproj”  扩展名的项目文件名。

   `<PackageOutputPath>` 是一个可选元素，用于确定将在何处生成 NuGet 包。 NuGet 包是.NET Core CLI 用于安装你的工具的包。

   项目文件现在类似于以下示例：

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">
  
     <PropertyGroup>

       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp3.1</TargetFramework>
  
       <PackAsTool>true</PackAsTool>
       <ToolCommandName>botsay</ToolCommandName>
       <PackageOutputPath>./nupkg</PackageOutputPath>
  
     </PropertyGroup>

   </Project>
   ```

1. 通过运行 [dotnet pack](dotnet-pack.md) 命令创建 NuGet 包：

   ```dotnetcli
   dotnet pack
   ```

   “microsoft.botsay.1.0.0.nupkg”文件在由 microsoft.botsay.csproj 文件的 `<PackageOutputPath>` 值标识的文件夹中创建，在本示例中为“./nupkg”文件夹    。
  
   如果想要公开发布一个工具，你可以将其上传到 `https://www.nuget.org`。 该工具在 NuGet 上可用后，开发人员就可以使用 [dotnet tool install](dotnet-tool-install.md) 命令安装该工具。 在本教程中，你将直接从本地“nupkg”  文件夹安装包，因此无需将包上传到 NuGet。

## <a name="troubleshoot"></a>疑难解答

如果在学习本教程时收到错误消息，请参阅[排查 .NET 工具使用问题](troubleshoot-usage-issues.md)。

## <a name="next-steps"></a>后续步骤

在本教程中，你创建了一个控制台应用程序并将其打包为工具。 若要了解如何使用该工具作为全局工具，请转到下一教程。

> [!div class="nextstepaction"]
> [安装和使用全局工具](global-tools-how-to-use.md)

如果需要，可以跳过全局工具教程，直接转到本地工具教程。

> [!div class="nextstepaction"]
> [安装和使用本地工具](local-tools-how-to-use.md)
