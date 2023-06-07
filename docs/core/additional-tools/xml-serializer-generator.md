---
title: Microsoft XML 序列化程序生成器
description: Microsoft XML 序列化程序生成器概述。 使用 XML 序列化程序生成器为项目中包含的类型生成 XML 序列化程序集。
author: mlacouture
ms.date: 01/19/2017
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 8005a8a3e5202b0255ec482dfb7e3c284bc2e19b
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90538899"
---
# <a name="using-microsoft-xml-serializer-generator-on-net-core"></a>在 .NET Core 上使用 Microsoft XML 序列化程序生成器

本教程介绍如何在 C# .NET Core 应用程序中使用 Microsoft XML 序列化程序生成器。 在本教程中可学习：

> [!div class="checklist"]
>
> - 如何创建 .NET Core 应用
> - 如何向 Microsoft.XmlSerializer.Generator 包中添加引用
> - 如何编辑 MyApp.csproj 以添加依赖项
> - 如何添加类和 XmlSerializer
> - 如何生成并运行应用程序

正如适用于 .NET Framework 的 [Xml Serializer Generator (sgen.exe)](../../standard/serialization/xml-serializer-generator-tool-sgen-exe.md)，[Microsoft.XmlSerializer.Generator NuGet 包](https://www.nuget.org/packages/Microsoft.XmlSerializer.Generator) 是适用于 .NET Core 和 .NET 标准项目的等效项。 它为程序集中包含的类型创建 XML 序列化程序集，从而提高使用 <xref:System.Xml.Serialization.XmlSerializer> 序列化或反序列化这些类型对象时，XML 序列化的启动性能。

## <a name="prerequisites"></a>先决条件

完成本教程：

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 或更高版本。
- 最喜爱的代码编辑器。

> [!TIP]
> 需要安装代码编辑器？ 试用 [Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)！

## <a name="use-microsoft-xml-serializer-generator-in-a-net-core-console-application"></a>在 .NET Core 控制台应用程序中使用 Microsoft XML 序列化程序生成器

以下说明将展示如何在 .NET Core 控制台应用程序中使用 XML 序列化程序生成器。

### <a name="create-a-net-core-console-application"></a>创建 .NET Core 控制台应用程序

打开命令提示符，创建一个名为“MyApp”的文件夹。 导航到创建的文件夹，并键入以下命令：

```dotnetcli
dotnet new console
```

### <a name="add-a-reference-to-the-microsoftxmlserializergenerator-package-in-the-myapp-project"></a>在 MyApp 项目中向 Microsoft.XmlSerializer.Generator 包添加引用

使用 [`dotnet add package`](../tools/dotnet-add-package.md) 命令在项目中添加引用。

类型：

```dotnetcli
dotnet add package Microsoft.XmlSerializer.Generator -v 1.0.0
```

### <a name="verify-changes-to-myappcsproj-after-adding-the-package"></a>添加包后，验证对 MyApp.csproj 的更改

打开代码编辑器并开始操作！ 仍从生成了应用的 MyApp 目录中进行操作。

在文本编辑器中打开 MyApp.csproj。

运行 [`dotnet add package`](../tools/dotnet-add-package.md) 命令后，会将以下行添加到 MyApp.csproj 项目文件中：

 ```xml
 <ItemGroup>
    <PackageReference Include="Microsoft.XmlSerializer.Generator" Version="1.0.0" />
 </ItemGroup>
 ```

### <a name="add-another-itemgroup-section-for-net-core-cli-tool-support"></a>为 .NET Core CLI 工具支持添加其他 ItemGroup 部分

在已检查的 `ItemGroup` 部分后添加以下行：

 ```xml
 <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.XmlSerializer.Generator" Version="1.0.0" />
 </ItemGroup>
 ```

### <a name="add-a-class-in-the-application"></a>在应用程序中添加类

在文本编辑器中打开 Program.cs。 在 Program.cs 中添加名为“MyClass”的类 。

```csharp
public class MyClass
{
   public int Value;
}
```

### <a name="create-an-xmlserializer-for-myclass"></a>为 MyClass 创建 `XmlSerializer`

在 Main 中添加以下行，为 MyClass 创建 `XmlSerializer`：

```csharp
var serializer = new System.Xml.Serialization.XmlSerializer(typeof(MyClass));
```

### <a name="build-and-run-the-application"></a>编译和运行应用程序

还是在 MyApp 文件夹中，通过 [`dotnet run`](../tools/dotnet-run.md) 运行应用程序，它会在运行时自动加载和使用预生成的序列化程序。

在控制台窗口中键入以下命令：

```dotnetcli
dotnet run
```

> [!NOTE]
> [`dotnet run`](../tools/dotnet-run.md) 调用 [`dotnet build`](../tools/dotnet-build.md) 来确保已生成要生成的目标，然后调用 `dotnet <assembly.dll>` 运行目标应用程序。

> [!IMPORTANT]
> 本教程中用来运行应用程序的命令和步骤仅用于开发过程。 准备好部署应用后，查看适用于 .NET Core 应用的不同[部署策略](../deploying/index.md)和 [`dotnet publish`](../tools/dotnet-publish.md) 命令。

如果一切顺利，则会在输出文件夹中生成名为“MyApp.XmlSerializers.dll”的程序集。

祝贺你！ 你刚才已完成：
> [!div class="checklist"]
>
> - 创建 .NET Core 应用。
> - 向 Microsoft.XmlSerializer.Generator 包中添加引用。
> - 编辑 MyApp.csproj 以添加依赖项。
> - 添加类和 XmlSerializer。
> - 生成和运行应用程序。

## <a name="related-resources"></a>相关资源

- [XML 序列化简介](../../standard/serialization/introducing-xml-serialization.md)
- [如何使用 XmlSerializer 进行序列化 (C#)](../../standard/linq/serialize-xmlserializer.md)
- [如何：使用 XmlSerializer (Visual Basic) 进行序列化](../../standard/linq/serialize-xmlserializer.md)
