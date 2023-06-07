---
title: 平台兼容性分析器
description: 可帮助检测跨平台应用和库中的平台兼容性问题的 Roslyn 分析器。
author: buyaa-n
ms.date: 09/17/2020
ms.openlocfilehash: 808e89df49a82e091862a052e62a367e6860fe47
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819482"
---
# <a name="platform-compatibility-analyzer"></a>平台兼容性分析器

你可能听说过“一个 .NET”的格言：一个统一的平台，用于生成任何类型的应用程序。 .NET 5.0 SDK 包括 ASP.NET Core、Entity Framework Core、WinForms、WPF、Xamarin 和 ML.NET，并且随着时间的推移，将添加对更多平台的支持。 .NET 5.0 旨在提供一种无需推出不同 .NET 风格，但不会尝试完全抽象出基础操作系统 (OS) 的体验。 你将继续能够调用特定于平台的 API，例如 P/Invoke、WinRT 或适用于 iOS 和 Android 的 Xamarin 绑定。

但在组件上使用特定于平台的 API 意味着代码在所有平台上都不再有效。 我们需要一种在设计时进行检测的方法，使开发人员在无意中使用特定于平台的 API 时获得诊断。 为了实现此目标，.NET 5.0 引入了[平台兼容性分析器](../../fundamentals/code-analysis/quality-rules/ca1416.md)和补充 API，帮助开发人员根据需要识别和使用特定于平台的 API。

新的 API 包括：

- <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute> 用于将 API 批注为特定于平台，<xref:System.Runtime.Versioning.UnsupportedOSPlatformAttribute> 用于将 API 批注为在特定 OS 上不受支持。 这些属性可以选择包括版本号，并且已应用于核心 .NET 库中的某些特定于平台的 API。
- <xref:System.OperatingSystem?displayProperty=nameWithType> 类中的 `Is<Platform>()` 和 `Is<Platform>VersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0)` 静态方法，用于安全地调用特定于平台的 API。 例如，<xref:System.OperatingSystem.IsWindows?displayProperty=nameWithType> 可用于保护对特定于 Windows 的 API 的调用，<xref:System.OperatingSystem.IsWindowsVersionAtLeast%2A?displayProperty=nameWithType>() 可用于保护受版本控制的特定于 Windows 的 API 调用。 请参阅这些[示例](#guard-platform-specific-apis-with-guard-methods)，了解如何使用这些方法保护特定于平台的 API 引用。

> [!TIP]
> 平台兼容性分析器升级并替换 [.NET API 分析器](../../standard/analyzers/api-analyzer.md)的[发现跨平台问题](../../standard/analyzers/api-analyzer.md#discover-cross-platform-issues)。

## <a name="prerequisites"></a>先决条件

平台兼容性分析器是 Roslyn 代码质量分析器之一。 从 .NET 5.0 开始，这些分析器[包含在 .NET SDK 中](../../fundamentals/code-analysis/overview.md)。 默认情况下，仅为面向 `net5.0` 或更高版本的项目启用平台兼容性分析器。 但是，可以为面向其他框架的项目[启用该分析器](../../fundamentals/code-analysis/quality-rules/ca1416.md#configurability)。

## <a name="how-the-analyzer-determines-platform-dependency"></a>分析器如何确定平台依赖关系

- 无归属的 API 被视为适用于所有 OS 平台。
- 标记为 `[SupportedOSPlatform("platform")]` 的 API 被视为仅可移植到指定 OS `platform`。
  - 可以多次应用该属性，以指示多个平台支持 (`[SupportedOSPlatform("windows"), SupportedOSPlatform("Android6.0")]`)。
  - 如果在没有正确的平台上下文的情况下引用特定于平台的 API，则分析器将生成警告：
    - 如果项目不面向受支持的平台（例如，特定于 Windows 的 API 调用，且项目面向 `<TargetFramework>net5.0-ios14.0</TargetFramework>`），则将生成警告。
    - 如果项目是多定向的 (`<TargetFramework>net5.0</TargetFramework>`)，则将生成警告。
    - 如果在面向任何指定平台的项目中引用特定于平台的 API（例如，对于特定于 Windows 的 API 调用，且项目面向 `<TargetFramework>net5.0-windows</TargetFramework>`），则不会生成警告。
    - 如果特定于平台的 API 调用受到相应的平台检查方法（如 `if(OperatingSystem.IsWindows())`）的保护，则不会生成警告。
    - 如果在相同的特定于平台的上下文（使用 `[SupportedOSPlatform("platform")` 属性化的调用站点）中引用特定于平台的 API，则不会生成警告。
- 标记为 `[UnsupportedOSPlatform("platform")]` 的 API 被视为仅在指定的 OS `platform` 上不受支持，但在所有其他平台上受支持。
  - 可以使用不同平台（例如 `[UnsupportedOSPlatform("iOS"), UnsupportedOSPlatform("Android6.0")]`）多次应用属性。
  - 仅当 `platform` 对调用站点有效时，分析器才会生成警告：
    - 如果项目面向被属性化为不受支持的平台（例如，如果 API 使用 `[UnsupportedOSPlatform("windows")]` 进行了属性化，且调用站点面向 `<TargetFramework>net5.0-windows</TargetFramework>`），则将生成警告。
    - 如果项目是多定向的，且 `platform` 包含在默认 [MSBuild `<SupportedPlatform>`](https://github.com/dotnet/sdk/blob/master/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.SupportedPlatforms.props) 项组中，或者 `platform` 手动包含在 `MSBuild` \<SupportedPlatform> 项组中，则将生成警告：

      ```XML
      <ItemGroup>
          <SupportedPlatform Include="platform" />
      </ItemGroup>
      ```

    - 如果要生成的应用不面向不受支持的平台或是多定向的，并且平台未包含在默认 [MSBuild `<SupportedPlatform>`](https://github.com/dotnet/sdk/blob/master/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.SupportedPlatforms.props) 项组中，则不会生成警告。
- 可以使用或不使用作为平台名称一部分的版本号对两个属性进行实例化。
  - 版本号的格式为 `major.minor[.build[.revision]]`；`major.minor` 是必需的，而 `build` 和 `revision` 部分是可选的。 例如，“Windows7.0”指示 Windows 版本 7.0，但“Windows”被解释为 Windows 0.0。

有关详细信息，请参阅[属性的工作方式及其导致的诊断的示例](#examples-of-how-the-attributes-work-and-what-diagnostics-they-produce)。

### <a name="advanced-scenarios-for-combining-attributes"></a>组合属性的高级方案

- 如果存在 `[SupportedOSPlatform]` 和 `[UnsupportedOSPlatform]` 属性的组合，则所有属性都按 OS 平台标识符分组：
  - 仅受支持的列表。 如果每个 OS 平台的最低版本是 `[SupportedOSPlatform]` 属性，则 API 会被视为仅在列出的平台上受支持，但在所有其他平台上不受支持。 每个平台的可选 `[UnsupportedOSPlatform]` 属性只能具有较高版本的最低支持版本，这表示从指定的版本开始删除 API。

    ```csharp
    // The API only supported on Windows 8.0 and later, not supported for all other.
    // The API is removed/unsupported from version 10.0.19041.0.
    [SupportedOSPlatform("windows8.0")]
    [UnsupportedOSPlatform("windows10.0.19041.0")]
    public void ApiSupportedFromWindows80SupportFromCertainVersion();
    ```

  - 仅不受支持的列表。 如果每个 OS 平台的最低版本是 `[UnsupportedOSPlatform]` 属性，则 API 会被视为仅在列出的平台上不受支持，但在所有其他平台上受支持。 此列表可能具有包含相同平台但版本较高的 `[SupportedOSPlatform]` 属性，这表示从该版本开始支持 API。

    ```csharp
    // The API was unsupported on Windows until version 10.0.19041.0.
    // The API is considered supported everywhere else without constraints.
    [UnsupportedOSPlatform("windows")]
    [SupportedOSPlatform("windows10.0.19041.0")]
    public void ApiSupportedFromWindows8UnsupportFromWindows10();
    ```

  - 不一致的列表。 如果某些平台的最低版本为 `[SupportedOSPlatform]`，而其他平台的最低版本为 `[UnsupportedOSPlatform]`，则会被视为不一致，不受分析器支持。
  - 如果 `[SupportedOSPlatform]` 和 `[UnsupportedOSPlatform]` 属性的最低版本相同，则分析器会将平台视为“仅受支持的列表”的一部分。
- 平台属性可应用于类型、成员（方法、字段、属性和事件）以及具有不同平台名称或版本的程序集。
  - 在顶级 `target` 应用的属性会影响其所有成员和类型。
  - 仅当遵守规则“子批注可以缩小平台支持范围，但无法将其扩大”时才会应用子级属性。
    - 当父级具有仅受支持的列表时，子成员属性无法添加新的平台支持，因为这会扩大父级支持。 只能将新平台支持添加到父级本身。 但对于具有更高版本的同一平台，子级可以有 `Supported` 属性，因为这会缩小支持。 另外，子级可以有同一平台的 `Unsupported` 属性，因为这也会缩小父级支持。
    - 当父级有仅限不支持的列表时，子成员属性可以添加对新平台的支持，因为这会缩小父级支持。 但它不能具有与父级所在平台相同的 `Supported` 属性，因为这会扩大父级支持。 只能将对同一平台的支持添加到应用了原始 `Unsupported` 属性的父级。
  - 如果对具有相同 `platform` 名称的 API 应用 `[SupportedOSPlatform("platformVersion")]` 一次以上，则分析器仅考虑最低版本的 API。
  - 如果对具有相同 `platform` 名称的 API 应用 `[UnsupportedOSPlatform("platformVersion")]` 两次以上，则分析器仅考虑最早版本的两个 API。

  > [!NOTE]
  > 最初受支持但在更高版本中不受支持（删除）的 API 并不希望在更高版本中重新受支持。

### <a name="examples-of-how-the-attributes-work-and-what-diagnostics-they-produce"></a>属性的工作方式及其导致的诊断的示例

  ```csharp
  // An API supported only on Windows.
  [SupportedOSPlatform("Windows")]
  public void WindowsOnlyApi() { }

  // an API supported on Windows and Linux.
  [SupportedOSPlatform("Windows")]
  [SupportedOSPlatform("Linux")]
  public void SupportedOnWindowsAndLinuxOnly() { }

  // an API only supported on Windows 8.0 and later, not supported for all other.
  // an API is removed/unsupported from version 10.0.19041.0.
  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")]
  public void ApiSupportedFromWindows8UnsupportFromWindows10() { }

  // an Assembly supported on Windows, the API added from version 10.0.19041.0.
  [assembly: SupportedOSPlatform("Windows")]
  [SupportedOSPlatform("windows10.0.19041.0")]
  public void AssemblySupportedOnWindowsApiSupportedFromWindows10() { }

  public void Caller()
  {
      WindowsOnlyApi(); // warns: 'WindowsOnlyApi' is supported on 'windows'

      // warns: 'SupportedOnWindowsAndLinuxOnly' is supported on 'Windows'
      // warns: 'SupportedOnWindowsAndLinuxOnly' is supported on 'Linux'
      SupportedOnWindowsAndLinuxOnly();

      // warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is supported on 'windows' 8.0 and later
      // warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is unsupported on 'windows' 10.0.19041.0 and later
      ApiSupportedFromWindows8UnsupportFromWindows10();

      // warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is supported on 'windows' 10.0.19041.0 and later
      // for same platform analyzer only warn for the latest version.
      AssemblySupportedOnWindowsApiSupportedFromWindows10();
  }

  // an API not supported on android but supported on all other.
  [UnsupportedOSPlatform("android")]
  public void DoesNotWorkOnAndroid() { }

  // an API was unsupported on Windows until version 8.0.
  // The API is considered supported everywhere else without constraints.
  [UnsupportedOSPlatform("windows")]
  [SupportedOSPlatform("windows8.0")]
  public void StartedWindowsSupportFromVersion8() { }

  // an API was unsupported on Windows until version8.0.
  // Then the API is removed (unsupported) from version 10.0.19041.0.
  // The API is considered supported everywhere else without constraints.
  [UnsupportedOSPlatform("windows")]
  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")]
  public void StartedWindowsSupportFrom8UnsupportedFrom10() { }

  public void Caller2()
  {
      DoesNotWorkOnAndroid(); // warns 'DoesNotWorkOnAndroid' is unsupported on 'android'

      // warns:'StartedWindowsSupportFromVersion8' is unsupported on 'windows'
      // warns:'StartedWindowsSupportFromVersion8' is supported on 'windows' 8.0 and later
      StartedWindowsSupportFromVersion8();

      // warns:'StartedWindowsSupportFrom8UnsupportedFrom10' is unsupported on 'windows'
      // warns:'StartedWindowsSupportFrom8UnsupportedFrom10' is supported on 'windows' 8.0 and later
      // even there were 3 diagnostics found analyzer warn only for the first 2.
      StartedWindowsSupportFrom8UnsupportedFrom10();
  }
  ```

## <a name="handle-reported-warnings"></a>处理报告的警告

处理这些诊断的建议方法是确保在相应的平台上运行时仅调用特定于平台的 API。 下面是可用于解决警告的选项；选择最适合你的情况的选项：

- 保护调用。 可以通过在运行时有条件地调用代码来实现此目的。 使用平台检查方法之一检查是否正在所需的 `Platform` 上运行，例如 `OperatingSystem.Is<Platform>()` 或 `OperatingSystem.Is<Platform>VersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0)`。

- 将调用站点标记为特定于平台。 还可以选择将自己的 API 标记为特定于平台，从而有效地将要求转发给调用方。 将包含的方法或类型或具有相同属性的整个程序集标记为引用的依赖平台的调用。 [示例](#mark-call-site-as-platform-specific)。

- 通过平台检查来断言调用站点。 如果不希望在运行时增加额外的 `if` 语句，请使用 <xref:System.Diagnostics.Debug.Assert(System.Boolean)?displayProperty=nameWithType>。 [示例](#assert-the-call-site-with-platform-check)。

- 删除代码。 通常不是你想要的，因为这意味着当 Windows 用户使用代码时将失真。 对于存在跨平台替代方法的情况，更好的做法可能是在特定于平台的 API 上使用此方法。

- 禁止显示警告。 通过 EditorConfig 条目或 `#pragma warning disable ca1416` 即可禁止显示警告。 但是，当使用特定于平台的 API 时，如非绝对必要，请勿使用此选项。

### <a name="guard-platform-specific-apis-with-guard-methods"></a>使用保护方法保护特定于平台的 API

保护方法的平台名称应与依赖平台的调用 API 平台名称匹配。 如果调用 API 的平台字符串包括版本：

- 对于 `[SupportedOSPlatform("platformVersion")]` 属性，保护方法平台 `version` 应大于或等于调用平台的 `Version`。
- 对于 `[UnsupportedOSPlatform("platformVersion")]` 属性，保护方法平台 `version` 应小于或等于调用平台的 `Version`。

  ```csharp
  public void CallingSupportedOnlyApis() // Allow list calls
  {
      if (OperatingSystem.IsWindows())
      {
          WindowsOnlyApi(); // will not warn
      }

      if (OperatingSystem.IsLinux())
      {
          SupportedOnWindowsAndLinuxOnly(); // will not warn, within one of the supported context
      }

      // Can use &&, || logical operators to guard combined attributes
      if (OperatingSystem.IsWindowsVersionAtLeast(8) && !OperatingSystem.IsWindowsVersionAtLeast(10.0.19041)))
      {
          ApiSupportedFromWindows8UnsupportFromWindows10();
      }

      if (OperatingSystem.IsWindowsVersionAtLeast(10, 0, 1903))
      {
          AssemblySupportedOnWindowsApiSupportedFromWindows10(); // Only need to check latest supported version
      }
  }

  public void CallingUnsupportedApis()
  {
      if (!OperatingSystem.IsAndroid())
      {
          DoesNotWorkOnAndroid(); // will not warn
      }

      if (!OperatingSystem.IsWindows() || OperatingSystem.IsWindowsVersionAtLeast(8))
      {
          StartedWindowsSupportFromVersion8(); // will not warn
      }

      if (!OperatingSystem.IsWindows() || // supported all other platforms
         (OperatingSystem.IsWindowsVersionAtLeast(8) && !OperatingSystem.IsWindowsVersionAtLeast(10, 0, 1903)))
      {
          StartedWindowsSupportFrom8UnsupportedFrom10(); // will not warn
      }
  }
  ```

- 如果需要保护面向新 <xref:System.OperatingSystem> API 不可用的 `netstandard` 或 `netcoreapp` 的代码，则可以使用 <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform%2A?displayProperty=nameWithType> API 并由分析器遵守。 但不如 <xref:System.OperatingSystem> 中添加的新 API 那样优化。 如果平台在 <xref:System.Runtime.InteropServices.OSPlatform> 结构中不受支持，则可以调用 <xref:System.Runtime.InteropServices.OSPlatform.Create(System.String)?displayProperty=nameWithType> 并传入平台名称（分析器也会遵循此名称）。

  ```csharp
  public void CallingSupportedOnlyApis()
  {
      if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
      {
          SupportedOnWindowsAndLinuxOnly(); // will not warn
      }

      if (RuntimeInformation.IsOSPlatform(OSPlatform.Create("browser")))
      {
          ApiOnlySupportedOnBrowser(); // call of browser specific API
      }
  }
  ```

### <a name="mark-call-site-as-platform-specific"></a>将调用站点标记为特定于平台

平台名称应与依赖平台的调用 API 匹配。 如果平台字符串包括版本：

- 对于 `[SupportedOSPlatform("platformVersion")]` 属性，调用站点平台 `version` 应大于或等于调用平台的 `Version`
- 对于 `[UnsupportedOSPlatform("platformVersion")]` 属性，调用站点平台 `version` 应小于或等于调用平台的 `Version`

  ```csharp
  // an API supported only on Windows.
  [SupportedOSPlatform("windows")]
  public void WindowsOnlyApi() { }

  // an API supported on Windows and Linux.
  [SupportedOSPlatform("Windows")]
  [SupportedOSPlatform("Linux")]
  public void SupportedOnWindowsAndLinuxOnly() { }

  // an API only supported on Windows 8.0 and later, not supported for all other.
  // an API is removed/unsupported from version 10.0.19041.0.
  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")]
  public void ApiSupportedFromWindows8UnsupportFromWindows10() { }

  // an Assembly supported on Windows, the API added from version 10.0.19041.0.
  [assembly: SupportedOSPlatform("Windows")]
  [SupportedOSPlatform("windows10.0.19041.0")]
  public void AssemblySupportedOnWindowsApiSupportedFromWindows10() { }

  [SupportedOSPlatform("windows8.0")] // call site attributed Windows 8.0 or above.
  public void Caller()
  {
      WindowsOnlyApi(); // will not warn as call site is for Windows.

      // will not warn as call site is for Windows.
      SupportedOnWindowsAndLinuxOnly();

      // will not warn for the API's [SupportedOSPlatform("windows8.0")] attribute.
      // Warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is unsupported on 'windows' 10.0.19041.0 and later
      ApiSupportedFromWindows8UnsupportFromWindows10();

      // warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is supported on 'windows' 10.0.19041.0 and later
      // as the call site version is lower than calling version.
      AssemblySupportedOnWindowsApiSupportedFromWindows10();
  }

  [SupportedOSPlatform("windows11.0")] // call site attributed with windows 11.0 or above.
  public void Caller2()
  {
      // Warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is unsupported on 'windows' 10.0.19041.0 and later
      ApiSupportedFromWindows8UnsupportFromWindows10();

      // will not warn as call site version higher than calling API.
      AssemblySupportedOnWindowsApiSupportedFromWindows10();
  }

  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")] // call site supports Windows from version 8.0 to 10.0.19041.0.
  public void Caller3()
  {
      // will not warn as caller has exact same attributes.
      ApiSupportedFromWindows8UnsupportFromWindows10();

      // Warns: 'ApiSupportedFromWindows8UnsupportFromWindows10' is supported on 'windows' 10.0.19041.0 and later
      // As call site stopped support from that version.
      AssemblySupportedOnWindowsApiSupportedFromWindows10();
  }

  // an API not supported on Android but supported on all other.
  [UnsupportedOSPlatform("android")]
  public void DoesNotWorkOnAndroid() { }

  // an API was unsupported on Windows until version 8.0.
  // The API is considered supported everywhere else without constraints.
  [UnsupportedOSPlatform("windows")]
  [SupportedOSPlatform("windows8.0")]
  public void StartedWindowsSupportFromVersion8() { }

  // an API was unsupported on Windows until version8.0.
  // Then the API is removed (unsupported) from version 10.0.19041.0.
  // The API is considered supported everywhere else without constraints.
  [UnsupportedOSPlatform("windows")]
  [SupportedOSPlatform("windows8.0")]
  [UnsupportedOSPlatform("windows10.0.19041.0")]
  public void StartedWindowsSupportFrom8UnsupportedFrom10() { }

  [UnsupportedOSPlatform("windows")] // Caller no support Windows for any version.
  public void Caller4()
  {
      DoesNotWorkOnAndroid(); // warns 'DoesNotWorkOnAndroid' is unsupported on 'Android'.

      // will not warns as the call site not support Windows at all, but supports all other.
      StartedWindowsSupportFromVersion8();

      // same, will not warns as the call site not support Windows at all, but supports all other.
      StartedWindowsSupportFrom8UnsupportedFrom10();
  }

  [UnsupportedOSPlatform("windows")]
  [UnsupportedOSPlatform("android")] // Caller not support Windows and Android for any version.
  public void Caller4()
  {
      DoesNotWorkOnAndroid(); // will not warn as call site not supports Android.

      // will not warns as the call site not support Windows at all, but supports all other.
      StartedWindowsSupportFromVersion8();

      // same, will not warns as the call site not support Windows at all, but supports all other.
      StartedWindowsSupportFrom8UnsupportedFrom10();
  }
  ```

### <a name="assert-the-call-site-with-platform-check"></a>通过平台检查来断言调用站点

[平台保护示例](#guard-platform-specific-apis-with-guard-methods)中使用的所有条件检查也可以用作 <xref:System.Diagnostics.Debug.Assert(System.Boolean)?displayProperty=nameWithType> 的条件。

  ```csharp
  // An API supported only on Linux.
  [SupportedOSPlatform("linux")]
  public void LinuxOnlyApi() { }

  public void Caller()
  {
      Debug.Assert(OperatingSystem.IsLinux());

      LinuxOnlyApi(); // will not warn
  }
  ```

## <a name="see-also"></a>另请参阅

- [.NET 5 中的目标框架名称](https://github.com/dotnet/designs/blob/master/accepted/2020/net5/net5.md)
- [批注特定于平台的 API 并检测其用法](https://github.com/dotnet/designs/blob/master/accepted/2020/platform-checks/platform-checks.md)
- [在特定平台上将 API 批注为不受支持](https://github.com/dotnet/designs/blob/master/accepted/2020/platform-exclusion/platform-exclusion.md)
- [CA1416 平台兼容性分析器](../../fundamentals/code-analysis/quality-rules/ca1416.md)
- [.NET API 分析器](../../standard/analyzers/api-analyzer.md)
