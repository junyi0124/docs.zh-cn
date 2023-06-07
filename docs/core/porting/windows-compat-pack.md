---
title: 使用 Windows 兼容性包将代码移植到 .NET Core
description: 了解有关 Windows 兼容性包以及如何使用它将现有 .NET Framework 代码移植到 .NET Core 的信息。
author: terrajobst
ms.date: 12/07/2018
ms.openlocfilehash: 166259ca37a2005d67f6c545e4843a940f05fb71
ms.sourcegitcommit: b75a45f0cfe012b71b45dd9bf723adf32369d40c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80228639"
---
# <a name="use-the-windows-compatibility-pack-to-port-code-to-net-core"></a>使用 Windows 兼容性包将代码移植到 .NET Core

将现有代码移植到 .NET Core 时发现的一些最常见问题依赖于仅在 .NET Framework 中找到的 API 和技术。 Windows 兼容性包  提供许多这些技术，因此可以更轻松地生成 .NET Core 应用程序和 .NET Standard 库。

兼容包是 [.NET Standard 2.0 的逻辑扩展](../whats-new/dotnet-core-2-0.md#api-changes-and-library-support)，它大幅扩展了 API 集。 现有代码几乎不修改即可编译。 为了信守 .NET Standard 的承诺（“所有 .NET 实现都提供的一组 API”），.NET Standard 不包括无法跨所有平台工作的技术，如注册表、Windows Management Instrumentation (WMI) 或反射发出 API。 Windows 兼容性包位于 .NET Standard 顶部，提供对这些仅限 Windows 的技术的访问权限。 它对于想要移动到 .NET Core 但至少第一步仍计划停留在 Windows 上的客户尤其有用。 在这种情况下，能够使用仅限 Windows 的技术可消除迁移障碍。

## <a name="package-contents"></a>包的内容

Windows 兼容性包通过 [Microsoft.Windows.Compatibility NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)提供，可从面向 .NET Core 或 .NET Standard 的项目引用。

它提供了约 20,000 个 API，包括仅 Windows API 以及以下技术领域中的跨平台 API：

- 代码页
- CodeDom
- Configuration
- 目录服务
- 绘图
- ODBC
- 权限
- 端口
- Windows 访问控制列表 (ACL)
- Windows Communication Foundation (WCF)
- Windows 加密
- Windows 事件日志
- Windows 管理规范 (WMI)
- Windows 性能计数器
- Windows 注册表
- Windows 运行时缓存
- Windows 服务

有关详细信息，请参阅[兼容包规范](https://github.com/dotnet/designs/blob/master/accepted/2018/compat-pack/compat-pack.md)。

## <a name="get-started"></a>入门

1. 移植之前，请确保查看[移植过程](index.md)。

2. 将现有代码移植到 .NET Core 或 .NET Standard 时，请安装 [Microsoft.Windows.Compatibility NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)。

   如果要停留在 Windows 上，则已准备完毕。

3. 如果要在 Linux 或 macOS 上运行 .NET Core 应用程序或 .NET Standard 库，请使用 [API 分析器](../../standard/analyzers/api-analyzer.md)查找不会跨平台工作的 API 的使用情况。

4. 删除这些 API 的使用情况、将其替换为跨平台替代项，或使用平台检查对其实施保护，例如：

    ```csharp
    private static string GetLoggingPath()
    {
        // Verify the code is running on Windows.
        if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
        {
            using (var key = Registry.CurrentUser.OpenSubKey(@"Software\Fabrikam\AssetManagement"))
            {
                if (key?.GetValue("LoggingDirectoryPath") is string configuredPath)
                    return configuredPath;
            }
        }

        // This is either not running on Windows or no logging path was configured,
        // so just use the path for non-roaming user-specific data files.
        var appDataPath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
        return Path.Combine(appDataPath, "Fabrikam", "AssetManagement", "Logging");
    }
    ```

有关演示，请查看 [Windows 兼容性包的第 9 频道视频](https://channel9.msdn.com/Events/Connect/2017/T123)。
