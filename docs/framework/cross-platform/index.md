---
title: 使用 .NET Framework 针对多个平台开发
ms.date: 10/21/2020
ms.assetid: b153baaa-130c-4169-860b-e580591de91e
ms.openlocfilehash: ef8fae4238f404d650528d25ab873fa48e2c0703
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687739"
---
# <a name="develop-for-multiple-platforms-with-net-framework"></a>使用 .NET Framework 针对多个平台开发

可以通过使用 .NET Framework 和 Visual Studio，针对 Microsoft 和非 Microsoft 平台开发应用。

[!INCLUDE [net-framework-future](../../../includes/net-framework-future.md)]

## <a name="options-for-cross-platform-development"></a>跨平台开发的选项

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

若要针对多个平台进行开发，你可以共享源代码或二进制文件，并可以在 .NET Framework 代码和 Windows 运行时 API 之间进行调用。

|若希望...|使用...|
|-----------------------|------------|
|在 Windows Phone 8.1 和 Windows 8.1 应用之间共享源代码|共享的项目（Visual Studio 2013 Update 2 中的通用应用模板）。<br /><br /> -   当前不支持 Visual Basic。<br />-   通过使用 #`if` 语句，可分隔平台特定的代码。<br /><br /> 有关详细信息，请参阅：<br /><br /> -   [开始编码](/windows/uwp/get-started/create-uwp-apps)<br />-   [使用 Visual Studio 生成通用 XAML 应用](https://devblogs.microsoft.com/visualstudio/using-visual-studio-to-build-universal-xaml-apps/)（博客文章）<br />-   [使用 Visual Studio 生成 XAML 聚合应用](https://channel9.msdn.com/Events/Build/2014/3-591)（视频）|
|在面向不同平台的应用之间共享二进制文件|平台不可知的代码的可移植类库项目。<br /><br /> -   此方法通常用于实现业务逻辑的代码。<br />-   你可以使用 Visual Basic 或 C#。<br />-   API 支持因平台而异。<br />-   针对 Windows 8.1 和 Windows Phone 8.1 的可移植类库项目支持 Windows 运行时 API 和 XAML。 这些功能在较旧版本的可移植类库中不可用。<br />-   如果需要，可以通过使用接口或抽象类抽象处理平台特定的代码。<br /><br /> 有关详细信息，请参阅：<br /><br /> -   [可移植类库](portable-class-library.md)<br />-   [如何使可移植类库为你工作](/archive/blogs/dsplaisted/how-to-make-portable-class-libraries-work-for-you)（博客文章）<br />-   [将可移植类库与 MVVM 配合使用](using-portable-class-library-with-model-view-view-model.md) <br />-   [面向多个平台的库的应用资源](app-resources-for-libraries-that-target-multiple-platforms.md) <br />-   [.NET 可移植性分析器](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)（Visual Studio 扩展）|
|在平台（Windows8.1 和 Windows Phone 8.1 除外）的应用之间共享源代码|“添加为链接”功能。<br /><br /> -   这种方法适用于应用逻辑，其对这两种应用均常见，但出于某些原因不可移植。 你可以将此功能用于 C＃ 或 Visual Basic 代码。<br />     例如，Windows Phone 8 和 Windows 8 共享 Windows 运行时 API，但可移植类库不支持这些平台的 Windows 运行时。 你可以使用 `Add as link` 在 Windows Phone 8 应用和面向 Windows 8 的 Windows 应用商店应用之间共享公共 Windows 运行时代码。<br /><br /> 有关详细信息，请参阅：<br /><br /> -   [通过“添加为链接”共享代码](/previous-versions/windows/apps/jj714082(v=vs.105))<br />-   [如何：向项目中添加现有项](/previous-versions/visualstudio/visual-studio-2010/9f4t9t92(v=vs.100))|
|使用 .NET Framework 代码或从 NET Framework 代码调用 Windows 运行时 API，编写 Windows 应用商店应用|从 .NET Framework C# 或 Visual Basic 代码调用 Windows 运行时 API，并使用 .NET Framework 来创建 Windows 应用商店应用。 你应该注意这两个平台的 API 差异。 但是，存在帮助你处理这些差异的类。<br /><br /> 有关详细信息，请参阅：<br /><br /> -   [.NET Framework 对 Windows 应用商店应用和 Windows 运行时的支持情况](support-for-windows-store-apps-and-windows-runtime.md) <br />-   [向 Windows 运行时传递 URI](passing-a-uri-to-the-windows-runtime.md) <br />-   <xref:System.IO.WindowsRuntimeStreamExtensions><br />-    <xref:System.WindowsRuntimeSystemExtensions>|
|构建面向非 Microsoft 平台的 .NET Framework 应用|.NET Framework 中的可移植类库参考程序集，以及 Visual Studio 扩展或第三方工具（如 Xamarin）。<br /><br /> 有关详细信息，请参阅：<br /><br /> -   [可移植类库目前在所有平台上都可用。](https://devblogs.microsoft.com/dotnet/portable-class-library-pcl-now-available-on-all-platforms/) （博客文章）<br />-   [Xamarin 文档](/xamarin)|
|将 JavaScript 和 HTML 用于跨平台开发|使用 Visual Studio 2013 Update 2 中的通用应用模板，针对面向 Windows 8.1 和 Windows Phone 8.1 的 Windows 运行时 API 进行开发。 当前，你不能将 .NET Framework API 与 JavaScript 和 HTML 结合使用来开发跨平台应用。<br /><br /> 有关详细信息，请参阅：<br /><br /> -   [JavaScript 项目模板](/previous-versions/windows/apps/hh758331(v=win.10))<br />-   [使用 JavaScript 将 Windows 运行时应用移植到 Windows Phone](/previous-versions/windows/apps/dn636144(v=win.10))|
