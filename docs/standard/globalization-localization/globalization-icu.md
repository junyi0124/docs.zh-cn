---
title: 全球化和 ICU
ms.date: 05/21/2020
helpviewer_keywords:
- globalization [.NET], about globalization
- global applications, globalization
- international applications [.NET], globalization
- world-ready applications, globalization
- application development [.NET], globalization
- culture, globalization
- icu, icu on windows, ms-icu
ms.openlocfilehash: d0361ba41b3a10d6a316341fcdacb869e7a35d26
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94827108"
---
# <a name="net-globalization-and-icu"></a>.NET 全球化和 ICU

过去，.NET 全球化 API 在不同的平台上使用不同的基础库。 在 Unix 上，API 使用 [Unicode 国际组件 (ICU)](http://site.icu-project.org/home)，在 Windows 上，API 使用 [区域语言支持 (NLS)](/windows/win32/intl/national-language-support)。 这导致在不同平台上运行应用程序时，在少数全球化 API 中存在一些行为差异。 以下方面就存在明显的行为差异：

- 区域性和区域性数据
- 字符串大小写
- 字符串排序和搜索
- 排序关键字
- 字符串规范化
- 国际化域名 (IDN) 支持
- Linux 上的时区显示名称

从 .NET 5.0 开始，开发人员可以更好地控制使用哪个基础库，从而使应用程序可以避免不同平台之间的差异。

## <a name="icu-on-windows"></a>Windows 上的 ICU

Windows 2019 年 5 月 10 日更新及更高版本将 [icu.dll](/windows/win32/intl/international-components-for-unicode--icu-) 作为 OS 的一部分，并且 .NET 5.0 和更高版本默认使用 ICU。 在 Windows 上运行时，.NET 5.0 和更高版本尝试加载 `icu.dll`，如果此库可用，将使用它进行全球化实现。 如果无法找到或无法加载 ICU 库，如在较早版本的 Windows 上运行时，.NET 5.0 和更高版本将回退到基于 NLS 的实现。

> [!NOTE]
> 即使使用 ICU，`CurrentCulture`、`CurrentUICulture` 和 `CurrentRegion` 成员仍使用 Windows 操作系统 API 来遵从用户设置。

### <a name="behavioral-differences"></a>行为差异

如果你将应用升级到目标 .NET 5，即使你不知道正在使用全球化设施，你也可能会在应用中看到更改。 本部分列出了你可能会看到的行为更改之一，但还有其他一些行为更改。

##### <a name="stringindexof"></a>String.IndexOf

请考虑使用以下代码，它调用 <xref:System.String.IndexOf(System.String)?displayProperty=nameWithType> 来查找字符串中的换行符索引。

```csharp
string s = "Hello\r\nworld!";
int idx = s.IndexOf("\n");
Console.WriteLine(idx);
```

- 在 Windows 上的早期版本的 .NET 中，代码片段打印 `6`。
- 在 Windows 10 2019 年 5 月更新和更高版本上的 .NET 5.0 及更高版本中，代码片段打印 `-1`。

若要通过执行序号搜索而不是区分区域性的搜索来修复此代码，请调用 <xref:System.String.IndexOf(System.String,System.StringComparison)> 重载，并传入 <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> 作为参数。

可以运行代码分析规则 [CA1307：为了清晰起见，请指定 StringComparison](../../../docs/fundamentals/code-analysis/quality-rules/ca1307.md) 和 [CA1309：使用序号 StringComparison](../../../docs/fundamentals/code-analysis/quality-rules/ca1309.md) 在代码中查找这些调用站点。

有关详细信息，请参阅[在 .NET 5 及更高版本中比较字符串时的行为更改](../base-types/string-comparison-net-5-plus.md)。

### <a name="use-nls-instead-of-icu"></a>使用 NLS 而不是 ICU

使用 ICU 代替 NLS 可能会导致与一些与全球化相关的操作存在行为差异。 若要恢复为使用 NLS，开发人员可以选择退出 ICU 实现。 应用程序可以通过以下任意方式启用 NLS 模式：

- 在项目文件中：

  ```xml
  <ItemGroup>
    <RuntimeHostConfigurationOption Include="System.Globalization.UseNls" Value="true" />
  </ItemGroup>
  ```

- 在 `runtimeconfig.json` 文件中：

  ```json
  {
    "runtimeOptions": {
       "configProperties": {
         "System.Globalization.UseNls": true
        }
    }
  }
  ```

- 通过将环境变量 `DOTNET_SYSTEM_GLOBALIZATION_USENLS` 设置为值 `true` 或 `1`。

> [!NOTE]
> 在项目或 `runtimeconfig.json` 文件中设置的值优先于环境变量。

有关详细信息，请参阅[运行时配置设置](../../core/run-time-config/globalization.md#nls)。

## <a name="app-local-icu"></a>应用本地 ICU

每个版本的 ICU 都可能附带了 bug 修复以及描述世界语言的更新公共区域设置数据存储库 (CLDR) 数据。 当涉及与全球化相关的操作时，在 ICU 版本间移动可能会对应用行为产生细微影响。 为了帮助应用程序开发人员确保所有部署之间的一致性，.NET 5.0 和更高版本使 Windows 和 Unix 上的应用能够携带和使用其自己的 ICU 副本。

应用程序可以通过以下任一方式选择使用应用本地 ICU 实现模式：

- 在项目文件中：

  ```xml
  <ItemGroup>
    <RuntimeHostConfigurationOption Include="System.Globalization.AppLocalIcu" Value="<suffix>:<version> or <version>" />
  </ItemGroup>
  ```

- 在 `runtimeconfig.json` 文件中：

  ```json
  {
    "runtimeOptions": {
       "configProperties": {
         "System.Globalization.AppLocalIcu": "<suffix>:<version> or <version>"
       }
    }
  }
  ```

- 通过将环境变量 `DOTNET_SYSTEM_GLOBALIZATION_APPLOCALICU` 设置为值 `<suffix>:<version>` 或 `<version>`。

  `<suffix>`：长度小于 36 个字符的可选后缀，遵循公共 ICU 打包约定。 在生成自定义 ICU 时，可以对其自定义以生成 lib 名称并导出符号名称以包含后缀，例如 `libicuucmyapp`，其中 `myapp` 是后缀。

  `<version>`：有效的 ICU 版本，例如 67.1。 此版本用于加载二进制文件和获取导出的符号。

为了在设置应用本地开关时加载 ICU，.NET 使用 <xref:System.Runtime.InteropServices.NativeLibrary.TryLoad%2A?displayProperty=nameWithType> 方法探测多个路径。 该方法首先尝试在 `NATIVE_DLL_SEARCH_DIRECTORIES` 属性中查找库，该属性由 dotnet 主机基于应用的 `deps.json` 文件创建。 有关更多信息，请参阅[默认探测](../../core/dependency-loading/default-probing.md)。

对于独立应用，用户不需要执行任何特殊操作，只需确保 ICU 位于应用目录中（对于独立应用，工作目录默认为 `NATIVE_DLL_SEARCH_DIRECTORIES`）。

如果通过 NuGet 包使用 ICU，则可在依赖框架的应用程序中使用。 NuGet 解析本机资产，并将它们包含在 `deps.json` 文件和 `runtimes` 目录下应用程序的输出目录中。 .NET 从这里加载它。

对于在本地版本使用 ICU 的依赖框架的应用（非独立应用），必须执行额外的步骤。 .NET SDK 尚不具有用于将“松散”的本机二进制文件合并到 `deps.json` 中的功能（请参阅[此 SDK 问题](https://github.com/dotnet/sdk/issues/11373)）。 相反，你可以通过将其他信息添加到应用程序的项目文件来启用此功能。 例如：

```xml
<ItemGroup>
  <IcuAssemblies Include="icu\*.so*" />
  <RuntimeTargetsCopyLocalItems Include="@(IcuAssemblies)" AssetType="native" CopyLocal="true" DestinationSubDirectory="runtimes/linux-x64/native/" DestinationSubPath="%(FileName)%(Extension)" RuntimeIdentifier="linux-x64" NuGetPackageId="System.Private.Runtime.UnicodeData" />
</ItemGroup>
```

必须为支持的运行时的所有 ICU 二进制文件执行此操作。 此外，`RuntimeTargetsCopyLocalItems` 项组中的 `NuGetPackageId` 元数据需要与项目实际引用的 NuGet 包匹配。

### <a name="macos-behavior"></a>macOS 行为

`macOS` 关于使用 `match-o` 文件中指定的加载命令解析依赖的动态库的行为与 Linux 加载程序不同。 在 Linux 加载程序中，.NET 可以尝试使用 `libicudata`、`libicuuc` 和 `libicui18n`（按此顺序）来满足 ICU 依赖项关系图。 但在 macOS 上，这不起作用。 在 macOS 上生成 ICU 时，默认情况下，可以使用 `libicuuc` 中的这些加载命令获取动态库。 以下代码片段演示了一个示例。

```sh
~/ % otool -L /Users/santifdezm/repos/icu-build/icu/install/lib/libicuuc.67.1.dylib
/Users/santifdezm/repos/icu-build/icu/install/lib/libicuuc.67.1.dylib:
 libicuuc.67.dylib (compatibility version 67.0.0, current version 67.1.0)
 libicudata.67.dylib (compatibility version 67.0.0, current version 67.1.0)
 /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1281.100.1)
 /usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 902.1.0)
```

这些命令只引用 ICU 其他组件的依赖库的名称。 加载程序将按照 `dlopen` 约定执行搜索，这涉及到在系统目录中包含这些库，或设置 `LD_LIBRARY_PATH` env var，或在应用级目录中包含 ICU。 如果无法设置 `LD_LIBRARY_PATH` 或无法确保 ICU 二进制文件位于应用级目录中，则需要执行一些额外的操作。

对于加载程序，有一些指令（如 `@loader_path`）会告知加载程序使用加载命令在与二进制文件相同的目录中搜索该依赖项。 可通过两种方式实现此目的：

- `install_name_tool -change`

  运行以下命令：

  ```bash
  install_name_tool -change "libicudata.67.dylib" "@loader_path/libicudata.67.dylib" /path/to/libicuuc.67.1.dylib
  install_name_tool -change "libicudata.67.dylib" "@loader_path/libicudata.67.dylib" /path/to/libicui18n.67.1.dylib
  install_name_tool -change "libicuuc.67.dylib" "@loader_path/libicuuc.67.dylib" /path/to/libicui18n.67.1.dylib
  ```

- 修补 ICU 以生成含有 `@loader_path` 的安装名称

  在运行 autoconf (`./runConfigureICU`) 之前，将[这些行](https://github.com/unicode-org/icu/blob/ef91cc3673d69a5e00407cda94f39fcda3131451/icu4c/source/config/mh-darwin#L32-L37)更改为以下内容：

  ```
  LD_SONAME = -Wl,-compatibility_version -Wl,$(SO_TARGET_VERSION_MAJOR) -Wl,-current_version -Wl,$(SO_TARGET_VERSION) -install_name @loader_path/$(notdir $(MIDDLE_SO_TARGET))
  ```

## <a name="icu-on-webassembly"></a>WebAssembly 上的 ICU

存在专门针对 WebAssembly 工作负荷的 ICU 版本。 此版本提供与桌面配置文件的全球化兼容性。 若要将 ICU 数据文件大小从 24 MB 减小到 1.4 MB（如果使用 Brotli 压缩，可压缩到约 0.3 MB），则此工作负荷有少量限制。

不支持以下 API：

- <xref:System.Globalization.CultureInfo.EnglishName?displayProperty=nameWithType>
- <xref:System.Globalization.CultureInfo.NativeName?displayProperty=nameWithType>
- <xref:System.Globalization.DateTimeFormatInfo.NativeCalendarName?displayProperty=nameWithType>
- <xref:System.Globalization.RegionInfo.NativeName?displayProperty=nameWithType>

支持以下 API，但有一些限制：

- <xref:System.String.Normalize(System.Text.NormalizationForm)?displayProperty=nameWithType> 和 <xref:System.String.IsNormalized(System.Text.NormalizationForm)?displayProperty=nameWithType> 不支持很少使用的 <xref:System.Text.NormalizationForm.FormKC> 和 <xref:System.Text.NormalizationForm.FormKD> 形式。
- <xref:System.Globalization.RegionInfo.CurrencyNativeName?displayProperty=nameWithType> 返回与 <xref:System.Globalization.RegionInfo.CurrencyEnglishName?displayProperty=nameWithType> 相同的值。

此外，还可以在 [dotnet/icu 存储库](https://github.com/dotnet/icu/blob/0f49268ddfd3331ca090f1c51d2baa2f75f6c6c0/icu-filters/optimal.json#L6-L54)中找到受支持区域设置的列表。
