---
title: .NET API 分析器
description: 了解 .NET API 分析器如何有助于检测弃用的 API 和平台兼容性问题。
author: oliag
ms.date: 02/20/2020
ms.openlocfilehash: 47ef2368692aee56ebd3db7803cbde7368d38049
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819599"
---
# <a name="net-api-analyzer"></a>.NET API 分析器

.NET API 分析器是 Roslyn 分析器，可用于发现不同平台上的潜在 C# API 兼容性风险，并检测是否调用了弃用的 API。 可供所有 C# 开发人员在任何开发阶段使用。

API 分析器以 NuGet 包 [Microsoft.DotNet.Analyzers.Compatibility](https://www.nuget.org/packages/Microsoft.DotNet.Analyzers.Compatibility/) 的形式提供。 在项目中引用此分析器后，它就会自动监视代码，并指出有问题的 API 使用。 还可以单击灯泡图标，获取有关可能解决方案的建议。 下拉菜单包括禁止显示警告的选项。

> [!NOTE]
> .NET API 分析器仍为预发行版。

## <a name="prerequisites"></a>系统必备

- Visual Studio 2017 及更高版本，或 Visual Studio for Mac（所有版本）。

## <a name="discover-deprecated-apis"></a>发现弃用的 API

### <a name="what-are-deprecated-apis"></a>什么是弃用的 API？

.NET 系列是一组不断升级的大型产品，力求更好地满足客户需求。 弃用并将一些 API 替换为新 API 是很自然的。 若有更好的替换 API，API 便会被视为弃用。 指明 API 已遭弃用且不得使用的一种方法是，用 <xref:System.ObsoleteAttribute> 属性标记它。 这种方法的缺点是，所有已过时 API 都只有一个诊断 ID（对于 C#，ID 为 [CS0612](../../csharp/misc/cs0612.md)）。 这表示：

- 无法为每个事例设置专用文档。
- 无法禁止显示特定类别的警告。 要么禁止显示所有警告，要么允许显示所有警告。
- 为了通知用户有新弃用，必须更新引用的程序集或目标包。

API 分析器使用 API 专用错误代码，这些代码以 DE（全称是“Deprecation Error”（弃用错误））开头，这样就可以单独控制各个警告的显示。 分析器发现的弃用的 API 在 [dotnet/platform-compat](https://github.com/dotnet/platform-compat) 存储库中进行定义。

### <a name="add-the-api-analyzer-to-your-project"></a>将 API 分析器添加到项目

1. 打开 Visual Studio。
2. 打开要在其中运行分析器的项目。
3. 在解决方案资源管理器中，右键单击项目并选择“管理 NuGet 包”   。 （也可以在“项目”  菜单中选择此选项。）
4. 在“NuGet 包管理器”选项卡上：
   1. 选择“nuget.org”作为“包源”。
   2. 转到“浏览”  选项卡。
   3. 选择“包括预发行版”。 
   4. 搜索 Microsoft.DotNet.Analyzers.Compatibility  。
   5. 选择列表中的包。
   6. 选择“安装”按钮  。
   7. 选择“预览更改”对话框上的“确定”按钮，如果你同意所列包的许可条款，则选择“接受许可”对话框上的“我接受”按钮。

### <a name="use-the-api-analyzer"></a>使用 API 分析器

如果代码使用了弃用的 API（如 <xref:System.Net.WebClient>），API 分析器便会使用绿色波浪线突出显示它。 如果将鼠标悬停在 API 调用之上，就会看到包含 API 弃用相关信息的灯泡，如下面的示例所示：

![显示绿色波浪线且左侧有灯泡的 WebClient API 的屏幕截图。](media/api-analyzer/green-squiggle.jpg)

弃用的 API 每出现一次，“错误列表”  窗口中都会显示具有唯一 ID 的警告，如下面的示例所示 (`DE004`)：

![显示警告 ID 和说明的“错误列表”窗口的屏幕截图。](media/api-analyzer/warnings-id-and-descriptions.jpg "包含警告的错误列表窗口。")

单击 ID 后，便会转到详细信息网页，其中说明了 API 遭弃用的原因，以及有关可用替换 API 的建议。

可以禁止显示任何警告，具体方法是右键单击突出显示的成员，然后选择“禁止\<diagnostic ID>”。 禁止显示警告的方法有两种：

- [本地（在源中）](#suppress-warnings-locally)
- [全局（在禁止文件中）](#suppress-warnings-globally)- 推荐方法

### <a name="suppress-warnings-locally"></a>在本地禁止显示警告

若要在本地禁止显示警告，请右键单击要对其禁止显示警告的成员，然后依次选择“快速操作和重构” > “禁止 诊断 ID \<diagnostic ID>” > “在源中”。 此时，[#pragma](../../csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning.md) 警告预处理器指令会添加到定义的范围内的源代码中：![用 #pragma warning disable 指令框定的代码的屏幕截图。](media/api-analyzer/suppress-in-source.jpg)

### <a name="suppress-warnings-globally"></a>在全局禁止显示警告

若要在全局禁止显示警告，请右键单击要对其禁止显示警告的成员，然后依次选择“快速操作和重构” > “禁止诊断 ID\<diagnostic ID>” > “在禁止文件中”。

![右键单击菜单的屏幕截图，其中显示了用于在 Visual Studio 中禁止显示警告的选项。](media/api-analyzer/suppress-in-sup-file.jpg)

首次禁止后，GlobalSuppressions.cs  文件会添加到项目中。 此时，新的全局禁止会追加到此文件中。

![解决方案资源管理器中 GlobalSuppressions.cs 文件的屏幕截图。](media/api-analyzer/suppression-file.jpg)

全局禁止是推荐方法，可确保 API 使用跨项目保持一致。

## <a name="discover-cross-platform-issues"></a>发现跨平台问题

> [!NOTE]
> .NET 5.0 引入[平台兼容性分析器](platform-compat-analyzer.md)作为此功能的替代。 平台兼容性分析器包含在 .NET SDK 中（无需单独安装），并且默认启用。

与弃用的 API 类似，分析器可发现所有的非跨平台 API。 例如，<xref:System.Console.WindowWidth?displayProperty=nameWithType> 适用于 Windows，但不适用于 Linux 和 macOS。 诊断 ID 显示在“错误列表”  窗口中。 可以禁止显示警告，具体方法是右键单击并选择“快速操作和重构”  。 与禁止显示 API 弃用警告时的两种选择（要么继续使用弃用的成员并禁止显示警告，要么根本就不使用弃用的成员）不同，如果仅对特定平台开发代码，可以对不打算在其上运行代码的其他所有平台禁止显示任何警告。 为此，只需编辑项目文件，并添加列出要忽略的所有平台的 `PlatformCompatIgnore` 属性即可。 接受的值包括 `Linux`、`macOS` 和 `Windows`。

```xml
<PropertyGroup>
    <PlatformCompatIgnore>Linux;macOS</PlatformCompatIgnore>
</PropertyGroup>
```

如果代码定目标到多个平台，且要利用其中一些平台不支持的 API，可以使用 `if` 语句作为相应部分代码的临界子句：

```csharp
if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
{
     var w = Console.WindowWidth;
     // More code
}
```

可以每目标框架/操作系统进行有条件编译，但暂需要[手动](../frameworks.md#how-to-specify-a-target-framework)这样做。

## <a name="supported-diagnostics"></a>支持的诊断

目前，分析器处理以下情况：

- 使用抛出 <xref:System.PlatformNotSupportedException> 的 .NET Standard API (PC001)。
- 使用 .NET Framework 4.6.1 不支持的 .NET Standard API (PC002)。
- UWP (PC003) 中不存在的本机 API 的用法。
- Delegate.BeginInvoke 和 EndInvoke APIs (PC004) 的用法。
- 使用标记为弃用的 API (DEXXXX)。

## <a name="ci-machine"></a>CI 计算机

所有这些诊断不仅可以通过 IDE 获取，还能通过命令行在生成项目期间获取（包括 CI 服务器）。

## <a name="configuration"></a>Configuration

由用户确定应如何处理这些诊断，是生成警告、错误、建议，还是禁用诊断。 例如，作为架构师，可以确定应将兼容性问题处理为错误，对调用某些弃用的 API 生成警告，而对其他情况仅生成建议。 可以按诊断 ID 和按项目单独对此进行配置。 为此，请在“解决方案资源管理器”  中转到项目下的“依赖关系”  节点。 依次展开节点“依赖关系”   > “分析器”   > “Microsoft.DotNet.Analyzers.Compatibility”  。 右键单击诊断 ID，选择“设置规则集严重性”，然后选中相应选项。

![“解决方案资源管理器”的屏幕截图，其中显示了诊断和带有规则集严重性的弹出对话框。](media/api-analyzer/disable-notifications.jpg)

## <a name="see-also"></a>另请参阅

- [API 分析器简介](https://devblogs.microsoft.com/dotnet/introducing-api-analyzer/)博客文章。
- YouTube 上的 [API 分析器](https://youtu.be/eeBEahYXGd0)演示视频。
- [平台兼容性分析器](platform-compat-analyzer.md)
