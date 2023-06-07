---
title: 如何调试 CLR 激活问题
description: 请参阅如何调试 .NET 中的公共语言运行时 (CLR) 激活问题。 查看和调试 CLR 激活日志，这在确定根本原因时可能很有用。
ms.date: 03/30/2017
helpviewer_keywords:
- CLR activation, debugging issues
ms.assetid: 4fe17546-d56e-4344-a930-6d8e4a545914
ms.openlocfilehash: 5215e82aebf93fa8d6d1937563ab348126a01d97
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622609"
---
# <a name="how-to-debug-clr-activation-issues"></a>如何调试 CLR 激活问题

如果在使用正确版本的公共语言运行时 (CLR) 运行应用程序时遇到问题，可以查看和调试 CLR 激活日志。 如果应用程序加载不同于预期的 CLR 版本或者根本未加载 CLR，这些日志对于确定激活问题的根本原因非常有用。 [.NET Framework 初始化错误：管理用户体验](initialization-errors-managing-the-user-experience.md)中探讨了未能找到应用程序 CLR 的体验。

可以通过使用 HKEY_LOCAL_MACHINE 注册表项或系统环境变量在系统范围内启用 CLR 激活日志记录。 在删除注册表项或环境变量前，将持续生成日志。 或者，可以使用用户或进程局部环境变量来启用具有不同范围和持续时间的日志记录。

CLR 激活日志不应与[程序集绑定日志](../tools/fuslogvw-exe-assembly-binding-log-viewer.md)混淆，两者完全不同。

## <a name="to-enable-clr-activation-logging"></a>启用 CLR 激活日志记录

### <a name="using-the-registry"></a>使用注册表

1. 在注册表编辑器中，导航到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework（在 32 位计算机上）或 HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\\.NETFramework 文件夹（在 64 位计算机上）。

2. 添加名为 `CLRLoadLogDir` 的字符串值，并将其设置为要存储 CLR 激活日志的现有目录的完整路径。

保持启用激活记录，直至删除字符串值。

### <a name="using-an-environment-variable"></a>使用环境变量

- 将 `COMPLUS_CLRLoadLogDir` 环境变量设置为字符串，该字符串表示要存储 CLR 激活日志的现有目录的完整路径。

    环境变量的设置方式将决定其作用域：

  - 如果在系统级别进行设置，将对该计算机上的所有 .NET Framework 应用程序启用激活日志记录，直至删除环境变量。

  - 如果在用户级别进行设置，则仅对当前用户帐户启用激活日志记录。 在删除环境变量前，将持续记录。

  - 如果在加载 CLR 前，从进程内进行设置，将在进程终止前持续启用激活日志记录。

  - 如果在运行应用程序之前，在命令提示符下进行设置，将对通过该命令提示符运行的任意应用程序启用激活日志记录。

    例如，若要将激活日志存储在具有进程级作用域的 c:\clrloadlogs 目录中，请在运行应用程序之前打开“命令提示符”窗口并键入以下内容：

    ```console
    set COMPLUS_CLRLoadLogDir=c:\clrloadlogs
    ```

## <a name="example"></a>示例

CLR 激活日志提供大量关于 CLR 激活和 CLR 宿主 API 的用法的数据。 如本文所述，大部分数据由 Microsoft 内部使用，但有些数据对开发人员也非常有用。

日志反映了 CLR 宿主 API 的调用顺序。 它还包括有关计算机上检测到的已安装运行时集的有用数据。 CLR 激活日志格式本身不进行存档，但可用于帮助需要解决 CLR 激活问题的开发人员。

> [!NOTE]
> 在使用该 CLR 的进程终止前，无法打开激活日志。

> [!NOTE]
> CLR 激活日志未进行本地化；它们始终用英语生成。

在下面的激活日志示例中，日志后面突出显示和说明了最有用的信息。

```output
532,205950.367,CLR Loading log for C:\Tests\myapp.exe
532,205950.367,Log started at 4:26:12 PM on 10/6/2011
532,205950.367,-----------------------------------
532,205950.382,FunctionCall: _CorExeMain
532,205950.382,FunctionCall: ClrCreateInstance, Clsid: {2EBCD49A-1B47-4A61-B13A-4A03701E594B}, Iid: {E2190695-77B2-492E-8E14-C4B3A7FDD593}
532,205950.382,MethodCall: ICLRMetaHostPolicy::GetRequestedRuntime. Version: (null), Metahost Policy Flags: 0x168, Binary: (null), Iid: {BD39D1D2-BA2F-486A-89B0-B4B0CB466891}
532,205950.382,Installed Runtime: v4.0.30319. VERSION_ARCHITECTURE: 0
532,205950.382,Input values for ComputeVersionString follow this line
532,205950.382,-----------------------------------
532,205950.382,Default Application Name: C:\Tests\myapp.exe
532,205950.382,IsLegacyBind is: 0
532,205950.382,IsCapped is 0
532,205950.382,SkuCheckFlags are 0
532,205950.382,ShouldEmulateExeLaunch is 0
532,205950.382,LegacyBindRequired is 0
532,205950.382,-----------------------------------
532,205950.382,Parsing config file: C:\Tests\myapp.exe
532,205950.382,UseLegacyV2RuntimeActivationPolicy is set to 0
532,205950.382,LegacyFunctionCall: GetFileVersion. Filename: C:\Tests\myapp.exe
532,205950.382,LegacyFunctionCall: GetFileVersion. Filename: C:\Tests\myapp.exe
532,205950.382,C:\Tests\myapp.exe was built with version: v2.0.50727
532,205950.382,ERROR: Version v2.0.50727 is not present on the machine.
532,205950.398,SEM_FAILCRITICALERRORS is set to 0
532,205950.398,Launching feature-on-demand installation. CmdLine: C:\Windows\system32\fondue.exe /enable-feature:NetFx3
532,205950.398,FunctionCall: RealDllMain. Reason: 0
532,205950.398,FunctionCall: OnShimDllMainCalled. Reason: 0
```

- CLR 加载日志提供启动加载托管代码的进程的可执行文件的路径。 请注意，这可能是本地主机。

    ```output
    532,205950.367,CLR Loading log for C:\Tests\myapp.exe
    ```

- 安装的运行时是安装在计算机上作为激活请求的候选项的 CLR 版本集。

    ```output
    532,205950.382,Installed Runtime: v4.0.30319. VERSION_ARCHITECTURE: 0
    ```

- 生成所用版本是在生成向 [ICLRMetaHostPolicy::GetRequestedRuntime](../unmanaged-api/hosting/iclrmetahostpolicy-getrequestedruntime-method.md).等方法提供的二进制文件时所用的 CLR 版本。

    ```output
    532,205950.382,C:\Tests\myapp.exe was built with version: v2.0.50727
    ```

- 功能按需安装是指在 Windows 8 上启用 .NET Framework 3.5。 请参阅 [.NET Framework 初始化错误：管理用户体验](initialization-errors-managing-the-user-experience.md)，获取有关此方案的详细信息。

    ```output
    532,205950.398,Launching feature-on-demand installation. CmdLine: C:\Windows\system32\fondue.exe /enable-feature:NetFx3
    ```

## <a name="see-also"></a>请参阅

- [部署](index.md)
- [如何：将应用配置为支持 .NET Framework 4 或更高版本](../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
