---
title: CLR 探查器和 Windows 应用商店应用
ms.date: 03/30/2017
dev_langs:
- csharp
applies_to:
- Windows 10
- Windows 8
helpviewer_keywords:
- profiling API
- profiling API [.NET Framework]
- profiling managed code
- profiling managed code [Windows Store Apps]
ms.assetid: 1c8eb2e7-f20a-42f9-a795-71503486a0f5
ms.openlocfilehash: 04b4b529a5a1adaa40e804988dee506942c863c4
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94440075"
---
# <a name="clr-profilers-and-windows-store-apps"></a>CLR 探查器和 Windows 应用商店应用

本主题讨论编写分析 Windows 应用商店应用内运行的托管代码的诊断工具时需要考虑的事项。 它还提供了修改现有开发工具的指导原则，使其在针对 Windows 应用商店应用程序运行时继续运行。 若要了解此信息，最好是在熟悉公共语言运行时分析 API 的情况下，已在可对 Windows 桌面应用程序正常运行的诊断工具中使用此 API，并且现在有兴趣修改该工具，以便正确地针对 Windows 应用商店应用程序运行。

## <a name="introduction"></a>介绍

如果您在介绍的段落之后进行了介绍，那么您就会熟悉 CLR 分析 API。 你已经编写了一个适合托管桌面应用程序的诊断工具。 现在，您想知道如何使用托管的 Windows 应用商店应用程序。 也许您已经尝试过此工作，并发现它并不是一个简单的任务。 事实上，对于所有工具开发人员而言，可能不会有很多注意事项。 例如：

- Windows 应用商店应用在权限严重降低的上下文中运行。

- 与传统的托管模块相比，Windows 元数据文件具有独特的特征。

- 当交互式功能下降时，Windows 应用商店应用程序习惯挂起自身。

- 由于各种原因，进程间通信机制可能不再工作。

本主题列出了需要注意的问题以及如何正确处理它们。

如果你不熟悉 CLR 分析 API，请跳转到本主题末尾的资源，找到更好的介绍性信息。

提供有关特定 Windows Api 的详细信息以及如何使用这些 Api 的详细信息，请参阅本主题的讨论范围。 请考虑本主题中的一个起点，并参考 MSDN，详细了解此处引用的任何 Windows Api。

## <a name="architecture-and-terminology"></a>体系结构和术语

通常，诊断工具的体系结构如下图所示。 它使用术语 "探查器"，但许多此类工具比典型的性能或内存分析更好于代码覆盖率、模拟对象框架、时间行程调试、应用程序监视等方面。 为简单起见，本主题将继续引用所有这些工具作为探查器。

本主题中使用了以下术语：

应用程序

这是探查器正在分析的应用程序。 通常，此应用程序的开发人员现在使用探查器来帮助诊断应用程序的问题。 通常，此应用程序将是 Windows 桌面应用程序，但在本主题中，我们将查看 Windows 应用商店应用。

**探查器 DLL**

这是加载到正在分析的应用程序的进程空间中的组件。 此组件（也称为探查器 "agent"）实现 [ICorProfilerCallback](icorprofilercallback-interface.md)[ICorProfilerCallback 接口](icorprofilercallback-interface.md) (2、3等。 ) 接口并使用 [ICorProfilerInfo](icorprofilerinfo-interface.md) (2、3等 ) 接口来收集有关分析的应用程序的数据，并可能修改应用程序行为的各个方面。

**探查器 UI**

这是探查器用户与之交互的桌面应用程序。 它负责向用户显示应用程序状态，并为用户提供控制分析的应用程序行为的方法。 此组件始终在其自己的进程空间中运行，独立于正在分析的应用程序的进程空间。 探查器 UI 还可以充当 "附加触发器"，这是调用 [ICLRProfiling：： AttachProfiler](iclrprofiling-attachprofiler-method.md) 方法的过程，在探查器 dll 未在启动时加载的情况下，该过程将导致分析的应用程序加载探查器 dll。

> [!IMPORTANT]
> 探查器 UI 应保留为 Windows 桌面应用程序，即使它用于控制和报告 Windows 应用商店应用程序也是如此。 不希望能够在 Windows 应用商店中打包和交付您的诊断工具。 你的工具需要执行 Windows 应用商店应用无法执行的操作，而其中的许多问题都驻留在探查器 UI 中。

在本文档中，示例代码假定：

- 探查器 DLL 是用 c + + 编写的，因为它必须是本机 DLL，这要按照 CLR 分析 API 的要求。

- 探查器 UI 是用 c # 编写的。 这并不是必需的，但由于探查器 UI 过程的语言没有任何要求，为什么不选择简洁而简单的语言？

### <a name="windows-rt-devices"></a>Windows RT 设备

Windows RT 设备已被锁定。 此类设备上不能加载第三方探查器。 本文档重点介绍 Windows 8 Pc。

## <a name="consuming-windows-runtime-apis"></a>使用 Windows 运行时 Api

在以下各节中讨论的几个方案中，探查器 UI 桌面应用程序需要使用一些新的 Windows 运行时 Api。 你需要参考文档来了解可以在桌面应用程序中使用哪些 Windows 运行时 Api，以及从桌面应用程序和 Windows 应用商店应用程序调用它们的行为是否不同。

如果探查器 UI 是用托管代码编写的，则需要执行几个步骤，以便轻松地使用这些 Windows 运行时 Api。 有关详细信息，请参阅 [托管桌面应用和 Windows 运行时](/previous-versions/windows/apps/jj856306(v=win.10)) 文章。

## <a name="loading-the-profiler-dll"></a>加载探查器 DLL

本部分介绍探查器 UI 如何使 Windows 应用商店应用程序加载探查器 DLL。 本节中讨论的代码属于探查器 UI 桌面应用程序，因此涉及使用适用于桌面应用程序的 Windows Api，但对于 Windows 应用商店应用程序不一定是安全的。

探查器 UI 会使探查器 DLL 以两种方式加载到应用程序的进程空间中：

- 在应用程序启动时，由环境变量控制。

- 通过调用 [ICLRProfiling：： AttachProfiler](iclrprofiling-attachprofiler-method.md) 方法，在启动完成后附加到应用程序。

你的第一项障碍是将探查器 DLL 的启动加载和附加负载加载到一起，以便在 Windows 应用商店应用程序中正常工作。 这两种形式的加载均共用一些特别的注意事项，因此让我们从它们开始。

### <a name="common-considerations-for-startup-and-attach-loads"></a>启动和附加加载的常见注意事项

**对探查器 DLL 进行签名**

当 Windows 尝试加载探查器 DLL 时，它将验证探查器 DLL 是否已正确签名。 如果不是，则默认情况下加载失败。 可通过两种方式实现此目的：

- 确保探查器 DLL 已签名。

- 请告诉你的用户，他们必须先在 Windows 8 计算机上安装开发人员许可证，然后才能使用你的工具。 可以通过 Visual Studio 自动完成此操作，也可以从命令提示符手动完成此操作。 有关详细信息，请参阅 [获取开发人员许可证](/previous-versions/windows/apps/hh974578(v=win.10))。

**文件系统权限**

Windows 应用商店应用程序必须有权从文件系统上的 residesBy 默认值加载和执行探查器 DLL，Windows 应用商店应用程序对大多数目录没有此类权限，尝试加载探查器 DLL 将在 Windows 应用程序事件日志中生成类似于以下内容的条目：:

```output
NET Runtime version 4.0.30319.17929 - Loading profiler failed during CoCreateInstance.  Profiler CLSID: '{xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}'.  HRESULT: 0x80070005.  Process ID (decimal): 4688.  Message ID: [0x2504].
```

通常，仅允许 Windows 应用商店应用访问磁盘上有限的位置集。 每个 Windows 应用商店应用程序都可以访问自己的应用程序数据文件夹，还可以访问文件系统中所有 Windows 应用商店应用获得访问权限的其他一些区域。 最好将探查器 DLL 及其依赖项安装在程序文件或程序文件 (x86) 下，因为所有 Windows 应用商店应用程序在默认情况下都具有读取和执行权限。

### <a name="startup-load"></a>启动负载

通常情况下，在桌面应用程序中，探查器 UI 会通过初始化包含所需 CLR 分析 API 环境 (变量的环境块（例如，、、和) ）来提示探查器 DLL 的启动负载， `COR_PROFILER` `COR_ENABLE_PROFILING` `COR_PROFILER_PATH` 然后使用该环境块创建新的进程。 这同样适用于 Windows 应用商店应用，但机制有所不同。

**请勿运行提升**

如果处理尝试生成 Windows 应用商店应用进程 B，则进程 A 应以中等完整性级别运行，而不是在高完整性级别运行 (即，不) 提升。 这意味着探查器 UI 应以中等完整性级别运行，或者必须在中等完整性级别生成另一个桌面进程，才能启动 Windows 应用商店应用。

**选择要分析的 Windows 应用商店应用程序**

首先，需要询问探查器用户要启动哪个 Windows 应用商店应用。 对于桌面应用程序，可能会显示文件浏览对话框，用户将找到并选择 .exe 文件。 但 Windows 应用商店应用程序是不同的，使用 "浏览" 对话框没有什么意义。 相反，更好的做法是向用户显示为该用户安装的 Windows 应用商店应用列表，以供选择。

您可以使用 <xref:Windows.Management.Deployment.PackageManager> 类来生成此列表。 `PackageManager` 是可供桌面应用使用的 Windows 运行时类，实际上它 *仅* 适用于桌面应用程序。

下面的代码示例来自用 c # 编写为桌面应用程序的假设探查器 UI 使用 `PackageManager` 来生成 Windows 应用列表：

```csharp
string currentUserSID = WindowsIdentity.GetCurrent().User.ToString();
IAppxFactory appxFactory = (IAppxFactory) new AppxFactory();
PackageManager packageManager = new PackageManager();
IEnumerable<Package> packages = packageManager.FindPackagesForUser(currentUserSID);
```

**指定自定义环境块**

使用新的 COM 接口 [IPackageDebugSettings](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-ipackagedebugsettings)，你可以自定义 Windows 应用商店应用程序的执行行为，使某些形式的诊断变得更容易。 它的一个方法（ [EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging)）可让你在 Windows 应用商店应用程序启动时将其传递到 Windows 应用商店应用程序，以及其他有用的效果，如禁用自动进程挂起。 环境块很重要，因为在这种情况下，需要指定 CLR 使用的环境变量 `COR_PROFILER` ， (、 `COR_ENABLE_PROFILING` 和) 来 `COR_PROFILER_PATH)` 加载探查器 DLL。

请思考以下代码片段：

```csharp
IPackageDebugSettings pkgDebugSettings = new PackageDebugSettings();
pkgDebugSettings.EnableDebugging(packageFullName, debuggerCommandLine,
                                                                 (IntPtr)fixedEnvironmentPzz);
```

你需要获取几项权限：

- `packageFullName` 可以在循环访问包和抓取时确定 `package.Id.FullName` 。

- `debuggerCommandLine` 稍微有趣一些。 若要将自定义环境块传递到 Windows 应用商店应用程序，需要编写自己的、简化的虚拟调试器。 Windows 将生成 Windows 应用商店应用程序，然后通过使用以下示例中的命令行启动调试器来附加调试器：

    ```console
    MyDummyDebugger.exe -p 1336 -tid 1424
    ```

     其中， `-p 1336` 表示 Windows 应用商店应用程序的进程 id 为1336， `-tid 1424` 表示线程 id 1424 是已挂起的线程。 虚拟调试器将从命令行分析 ThreadID，恢复该线程，然后退出。

     下面是用于执行此操作的一些示例 c + + 代码 (确保添加错误检查！ ) ：

    ```cpp
    int wmain(int argc, wchar_t* argv[])
    {
        // …
        // Parse command line here
        // …

        HANDLE hThread = OpenThread(THREAD_SUSPEND_RESUME,
                                                                  FALSE /* bInheritHandle */, nThreadID);
        ResumeThread(hThread);
        CloseHandle(hThread);
        return 0;
    }
    ```

     需要在诊断工具安装过程中部署此虚拟调试器，然后在参数中指定此调试器的路径 `debuggerCommandLine` 。

**启动 Windows 应用商店应用程序**

开始 Windows 应用商店应用程序的启动时间最后到达。 如果已尝试自行执行此操作，则可能已注意到 [CreateProcess](/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createprocessa) 并不是创建 Windows 应用商店应用程序进程的方式。 相反，你将需要使用 [IApplicationActivationManager：： ActivateApplication](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication) 方法。 为此，需要获取正在启动的 Windows 应用商店应用程序的应用程序用户模型 ID。 这意味着你需要通过清单执行一些深入。

遍历包时 (参阅前面的 [启动加载](#startup-load) 部分中的 "选择要配置的 Windows 应用商店应用程序") ，你将需要获取当前包清单中包含的应用程序集：

```csharp
string manifestPath = package.InstalledLocation.Path + "\\AppxManifest.xml";

AppxPackaging.IStream manifestStream;
SHCreateStreamOnFileEx(
                    manifestPath,
                    0x00000040,     // STGM_READ | STGM_SHARE_DENY_NONE
                    0,              // file creation attributes
                    false,          // fCreate
                    null,           // reserved
                    out manifestStream);

IAppxManifestReader manifestReader = appxFactory.CreateManifestReader(manifestStream);

IAppxManifestApplicationsEnumerator appsEnum = manifestReader.GetApplications();
```

是，一个包可以包含多个应用程序，每个应用程序都有自己的应用程序用户模型 ID。 因此，您需要询问用户要分析的应用程序，并从该特定应用程序获取应用程序用户模型 ID：

```csharp
while (appsEnum.GetHasCurrent() != 0)
{
    IAppxManifestApplication app = appsEnum.GetCurrent();
    string appUserModelId = app.GetAppUserModelId();
    //...
}
```

最后，你将需要启动 Windows 应用商店应用程序：

```csharp
IApplicationActivationManager appActivationMgr = new ApplicationActivationManager();
appActivationMgr.ActivateApplication(appUserModelId, appArgs, ACTIVATEOPTIONS.AO_NONE, out pid);
```

**记得调用 DisableDebugging**

调用 [IPackageDebugSettings：： EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging)时，通过调用 [IPackageDebugSettings：:D isabledebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-disabledebugging) 方法，你就会作出一种承诺，因此在分析会话结束时请务必执行此操作。

### <a name="attach-load"></a>附加负载

当探查器 UI 要将其探查器 DLL 附加到已开始运行的应用程序时，它将使用 [ICLRProfiling：： AttachProfiler](iclrprofiling-attachprofiler-method.md)。 这同样适用于 Windows 应用商店应用。 但除了前面列出的常见注意事项外，还请确保目标 Windows 应用商店应用未挂起。

**EnableDebugging**

与启动负载一样，请调用 [IPackageDebugSettings：： EnableDebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-enabledebugging) 方法。 不需要它来传递环境块，但需要其其他功能之一：禁用自动进程挂起。 否则，当探查器 UI 调用 [AttachProfiler](iclrprofiling-attachprofiler-method.md)时，可能会挂起目标 Windows 应用商店应用。 事实上，如果用户现在与探查器 UI 交互，Windows 应用商店应用程序在用户的任何屏幕上都处于非活动状态，则可能会出现这种情况。 如果 Windows 应用商店应用程序被挂起，它将无法响应 CLR 向其发送的任何信号来附加探查器 DLL。

因此，您需要执行如下所示的操作：

```csharp
IPackageDebugSettings pkgDebugSettings = new PackageDebugSettings();
pkgDebugSettings.EnableDebugging(packageFullName, null /* debuggerCommandLine */,
                                                                 IntPtr.Zero /* environment */);
```

除了指定调试器命令行或环境块以外，还可以对启动负载情况进行此调用。

**DisableDebugging**

与往常一样，请不要忘记在分析会话完成时调用 [IPackageDebugSettings：:D isabledebugging](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ipackagedebugsettings-disabledebugging) 。

## <a name="running-inside-the-windows-store-app"></a>在 Windows 应用商店应用内运行

因此，Windows 应用商店应用最终会加载探查器 DLL。 现在，必须通过 Windows 应用商店应用所需的不同规则来讲授探查器 DLL 的播放方式，其中包括允许哪些 Api 以及如何使用降低的权限运行。

### <a name="stick-to-the-windows-store-app-apis"></a>坚持 Windows 应用商店应用 Api

浏览 Windows API 时，你会注意到，每个 API 都记录为适用于桌面应用、Windows 应用商店应用或两者。 例如， [InitializeCriticalSectionAndSpinCount](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionandspincount)函数文档的 " **要求** " 部分指示该函数仅适用于桌面应用程序。 与此相反， [InitializeCriticalSectionEx](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionex) 函数可用于桌面应用和 Windows 应用商店应用。

在开发探查器 DLL 时，将它视为 Windows 应用商店应用程序，并且仅使用 Windows 应用商店应用程序所记录的 Api。 分析依赖关系 (例如，你可以 `link /dump /imports` 针对探查器 DLL 运行审核) ，然后在文档中搜索，以查看哪些依赖项正常，哪些依赖项不正常。 在大多数情况下，只需将冲突替换为一种较新的 API 形式，将其替换为安全 (（例如，将 [InitializeCriticalSectionAndSpinCount](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionandspincount) 替换为 [InitializeCriticalSectionEx](/windows/desktop/api/synchapi/nf-synchapi-initializecriticalsectionex)) ，即可解决冲突。

你可能会注意到，探查器 DLL 会调用一些仅适用于桌面应用的 Api，并且即使在 Windows 应用商店应用程序中加载探查器 DLL，它们仍可正常工作。 请注意，在加载到 Windows 应用商店应用程序进程中时，使用未记录在 Windows 应用商店应用中的任何 API 会出现风险：

- 在运行 Windows 应用商店应用的唯一上下文中调用此类 Api 时，不一定能够正常工作。

- 从不同的 Windows 应用商店应用程序进程中调用此类 Api 可能不会运行。

- 在当前版本的 windows 中，此类 Api 看起来可以正常工作，但可能会在将来的 Windows 版本中中断或禁用。

最好的做法是解决所有冲突，并避免风险。

你可能会发现，如果没有特定的 API，则无法找到适合 Windows 应用商店应用的替换。 在这种情况下，至少要：

- 测试、测试、测试生活中的 daylights 使用该 API。

- 了解如果在 Windows 未来版本中从 Windows 应用商店应用程序内部调用，API 可能突然中断或消失。 这不会被认为是 Microsoft 的兼容性问题，并且支持你的使用不会成为优先考虑。

### <a name="reduced-permissions"></a>降低的权限

本主题的范围超出了 Windows 应用商店应用程序权限与桌面应用程序的不同之处。 但在加载到 Windows 应用商店应用程序中时，与桌面应用程序) 尝试访问任何资源时，每次加载到 Windows 应用商店应用程序时，行为都是不同的 (。 文件系统是最常见的示例。 某些情况下，给定的 Windows 应用商店应用程序可以访问磁盘上的一些位置 (请参阅 [文件访问和权限 (Windows 运行时应用程序](/previous-versions/windows/apps/hh967755(v=win.10))) ，探查器 DLL 将受到相同的限制。 彻底测试你的代码。

### <a name="inter-process-communication"></a>进程内通信

正如本文开头的关系图中所示，探查器 DLL (加载到 Windows 应用商店应用程序的进程空间中) 可能需要与探查器 UI 通信， (在单独的桌面应用程序进程空间中运行) 通过自定义的进程间通信 (IPC) 通道。 探查器 UI 将信号发送到探查器 DLL 以修改其行为，探查器 DLL 将数据从经过分析的 Windows 应用商店应用程序发送回探查器 UI，以便后期处理和向探查器用户显示。

大多数探查器都需要这样的工作，但当探查器 DLL 加载到 Windows 应用商店应用程序中时，对 IPC 机制的选择更受限制。 例如，命名管道不是 Windows 应用商店应用 SDK 的一部分，因此不能使用它们。

当然，文件仍处于中，不过，这种方式的方式更有限。 事件也可用。

**通过文件通信**

你的大多数数据可能会通过文件在探查器 DLL 和探查器 UI 之间传递。 关键是在 Windows 应用商店应用的上下文中选取探查器 DLL (的文件位置) 并且探查器 UI 具有对的读写访问权限。 例如，临时文件夹路径是探查器 DLL 和探查器 UI 可以访问的位置，但其他 Windows 应用商店应用包都不能访问 (因此防护您从其他 Windows 应用商店应用包) 的任何信息。

探查器 UI 和探查器 DLL 都可以独立确定此路径。 探查器 UI （当它循环访问为当前用户安装的所有包时 (查看之前) 的示例代码），获取对类的访问权限 `PackageId` ，通过此代码片段，可以使用类似于此代码段的代码来派生临时文件夹路径。  (为 "始终"，将省略错误检查以便于简洁。 ) 

```csharp
// C# code for the Profiler UI.
ApplicationData appData =
    ApplicationDataManager.CreateForPackageFamily(
        packageId.FamilyName);

tempDir = appData.TemporaryFolder.Path;
```

同时，探查器 DLL 可以执行基本相同的操作，尽管它可以 <xref:Windows.Storage.ApplicationData> 通过使用 [ApplicationData](xref:Windows.Storage.ApplicationData.Current%2A) 属性更轻松地访问类。

**通过事件通信**

如果需要探查器 UI 与探查器 DLL 之间的简单信号语义，可以在 Windows 应用商店应用程序和桌面应用程序中使用事件。

在探查器 DLL 中，只需调用 [CreateEventEx](/windows/desktop/api/synchapi/nf-synchapi-createeventexa) 函数，就可以使用您喜欢的任何名称创建命名事件。 例如：

```cpp
// Profiler DLL in Windows Store app (C++).
CreateEventEx(
    NULL,  // Not inherited
    "MyNamedEvent"
    CREATE_EVENT_MANUAL_RESET, /* explicit ResetEvent() required; leave initial state unsignaled */
    EVENT_ALL_ACCESS);
```

然后，探查器 UI 需要查找 Windows 应用商店应用的命名空间下的命名事件。 例如，探查器 UI 可以调用 [CreateEventEx](/windows/desktop/api/synchapi/nf-synchapi-createeventexa)，并将事件名称指定为

`AppContainerNamedObjects\<acSid>\MyNamedEvent`

`<acSid>` 是 Windows 应用商店应用的 AppContainer SID。 本主题前面部分介绍了如何循环访问为当前用户安装的包。 通过该示例代码，你可以获取 packageId。 然后，可以从 packageId 中获取如下 `<acSid>` 代码：

```csharp
IntPtr acPSID;
DeriveAppContainerSidFromAppContainerName(packageId.FamilyName, out acPSID);

string acSid;
ConvertSidToStringSid(acPSID, out acSid);

string acDir;
GetAppContainerFolderPath(acSid, out acDir);
```

### <a name="no-shutdown-notifications"></a>无关闭通知

在 Windows 应用商店应用程序中运行时，探查器 DLL 不应依赖于 [ICorProfilerCallback：： Shutdown](icorprofilercallback-shutdown-method.md) ，甚至是) 使用 [DllMain](/windows/desktop/Dlls/dllmain) (`DLL_PROCESS_DETACH` 来通知探查器 dll Windows 应用商店应用程序正在退出。 事实上，您应该永远不会调用它们。 从历史上看，许多探查器 Dll 已使用这些通知作为将缓存刷新到磁盘、关闭文件、将通知发送回探查器 UI 等的便利位置。但现在，需要以略有不同的方式组织探查器 DLL。

探查器 DLL 应在记录信息时进行记录。 出于性能方面的考虑，可能需要在内存中批处理信息，并将其刷新到磁盘，因为批大小超过了某个阈值。 但假定尚未刷新到磁盘的任何信息可能会丢失。 这意味着你需要明智地选择阈值，并且需要强化探查器 UI 才能处理探查器 DLL 编写的不完整信息。

## <a name="windows-runtime-metadata-files"></a>Windows 运行时元数据文件

它不在本文档的讨论范围内，可以详细了解 WinMD) 文件 (Windows 运行时元数据。 此部分仅限于在探查器 DLL 正在分析的 Windows 应用商店应用程序加载 WinMD 文件时，CLR 分析 API 如何做出反应。

### <a name="managed-and-non-managed-winmds"></a>托管和非托管 Winmd

如果开发人员使用 Visual Studio 来创建新的 Windows 运行时组件项目，则该项目的生成将生成一个 WinMD 文件，该文件描述由开发人员创作 ) 类、接口等的类型说明中的元数据 (。 如果此项目是使用 c # 或 Visual Basic 编写的托管语言项目，同一 WinMD 文件也包含这些类型的实现 (意味着它包含从开发人员的源代码) 编译的所有 IL。 此类文件称为托管 WinMD 文件。 它们非常有趣，因为它们既包含 Windows 运行时元数据，也包含基础实现。

与此相反，如果开发人员创建一个用于 c + + 的 Windows 运行时组件项目，则该项目的生成将生成一个仅包含元数据的 WinMD 文件，并且该实现将编译为单独的本机 DLL。 同样，在 Windows SDK 中附带的 WinMD 文件仅包含元数据，并将实现编译为 Windows 附带的单独本机 Dll。

下面的信息适用于托管 Winmd，其中包含元数据和实现，以及非托管 Winmd （仅包含元数据）。

### <a name="winmd-files-look-like-clr-modules"></a>WinMD 文件类似于 CLR 模块

与 CLR 相关，所有 WinMD 文件都是模块。 因此，CLR 分析 API 会告诉探查器 DLL 何时加载 WinMD 文件以及它们的 Moduleid，其方式与其他托管模块相同。

探查器 DLL 可以通过调用 [ICorProfilerInfo3：： GetModuleInfo2](icorprofilerinfo3-getmoduleinfo2-method.md) 方法并检查 `pdwModuleFlags` [COR_PRF_MODULE_WINDOWS_RUNTIME](cor-prf-module-flags-enumeration.md) 标志的输出参数来区分来自其他模块的 WinMD 文件。  (当且仅当 ModuleID 表示 WinMD 时设置。 ) 

### <a name="reading-metadata-from-winmds"></a>从 Winmd 读取元数据

WinMD 文件（如常规模块）包含可通过 [元数据 api](../metadata/index.md)读取的元数据。 但是，在读取 WinMD 文件时，CLR 将 Windows 运行时类型映射到 .NET Framework 类型，以便在托管代码中编写和使用 WinMD 文件的开发人员具有更自然的编程体验。 有关这些映射的一些示例，请参阅 [Windows 应用商店应用和 Windows 运行时 .NET Framework 支持](../../cross-platform/support-for-windows-store-apps-and-windows-runtime.md)。

在使用元数据 Api 时，探查器会获得哪个视图：原始 Windows 运行时视图或映射的 .NET Framework 视图？  答案是：由您负责。

调用 WinMD 上的 [ICorProfilerInfo：： GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) 方法以获取元数据接口（如 [IMetaDataImport](../metadata/imetadataimport-interface.md)）时，可以选择在参数中设置 [ofNoTransform](../metadata/coropenflags-enumeration.md) `dwOpenFlags` 以关闭此映射。 否则，默认情况下将启用映射。 通常，探查器将保持启用映射，以便探查器 DLL 从 WinMD 元数据中获取的字符串 (例如，) 类型的名称将对探查器用户非常熟悉和自然。

### <a name="modifying-metadata-from-winmds"></a>修改 Winmd 中的元数据

不支持修改 Winmd 中的元数据。 如果为 WinMD 文件调用 [ICorProfilerInfo：： GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) 方法并在参数中指定 [ofWrite](../metadata/coropenflags-enumeration.md) `dwOpenFlags` ，或者要求提供可写的元数据接口（如 [IMetaDataEmit](../metadata/imetadataemit-interface.md)），则 [GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) 将会失败。 这对于 IL 重写探查器尤其重要，后者需要修改元数据以支持其检测 (例如，将引用或新方法添加) 。 因此，你应该按照上一部分中所述来检查 [COR_PRF_MODULE_WINDOWS_RUNTIME](cor-prf-module-flags-enumeration.md) 第一个 () 并避免在此类模块上请求可写的元数据接口。

### <a name="resolving-assembly-references-with-winmds"></a>用 Winmd 解析程序集引用

许多探查器需要手动解析元数据引用，以帮助进行检测或类型检查。 此类探查器需要了解 CLR 如何解析指向 Winmd 的程序集引用，因为这些引用的解析方式与标准程序集引用完全不同。

## <a name="memory-profilers"></a>内存探查器

在 Windows 应用商店应用和桌面应用中，垃圾回收器和托管堆根本不是不同的。 但是，探查器作者需要注意一些细微的差异。

### <a name="forcegc-creates-a-managed-thread"></a>ForceGC 创建托管线程

进行内存分析时，探查器 DLL 通常会创建一个单独的线程来调用 [ForceGC 方法](icorprofilerinfo-forcegc-method.md) 方法。 这不是什么新内容。 但可能会令人吃惊的是，在 Windows 应用商店应用程序中执行垃圾回收的操作可能会将线程转换为托管线程 (例如，将为该线程) 创建分析 API ThreadID。

若要理解这一点，请务必了解 CLR 分析 API 定义的同步和异步调用之间的差异。 请注意，这与 Windows 应用商店应用程序中异步调用的概念非常不同。 有关详细信息，请参阅博客文章 [CORPROF_E_UNSUPPORTED_CALL_SEQUENCE 原因](/archive/blogs/davbr/why-we-have-corprof_e_unsupported_call_sequence) 。

相关的一点是，在探查器创建的线程上进行的调用始终被视为同步调用，即使这些调用是从某个探查器 DLL 的 [ICorProfilerCallback](icorprofilercallback-interface.md) 方法的实现之外进行的。 至少，这种情况下也是如此。 由于调用了 [ForceGC 方法](icorprofilerinfo-forcegc-method.md)，CLR 已将探查器的线程转换为托管线程，该线程不再被视为探查器的线程。 因此，CLR 强制实施更严格的定义，该定义对该线程来说是同步的（也就是说，调用必须从探查器 DLL 的 [ICorProfilerCallback](icorprofilercallback-interface.md) 方法之一开始，才能作为同步。

这在实践中是什么意思？ 大多数 [ICorProfilerInfo](icorprofilerinfo-interface.md) 方法只是以同步方式调用，并且会立即失败。 因此，如果探查器 DLL 对通常在探查器创建的线程上执行的其他调用重复使用 [ForceGC 方法](icorprofilerinfo-forcegc-method.md) 线程 (例如， [RequestProfilerDetach](icorprofilerinfo3-requestprofilerdetach-method.md)、 [RequestReJIT](icorprofilerinfo4-requestrejit-method.md)或 [RequestRevert](icorprofilerinfo4-requestrevert-method.md)) ，则会遇到问题。 在从托管线程调用时，甚至异步安全函数（如 [DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md) ）都有特殊的规则。  (参见博客文章 [探查器堆栈遍历：基础知识和](/archive/blogs/davbr/profiler-stack-walking-basics-and-beyond) 更高版本，了解详细信息。 ) 

因此，建议使用探查器 DLL 创建的任何线程来调用 [ForceGC 方法](icorprofilerinfo-forcegc-method.md) ， *只* 应使用它来触发 GC，然后响应 gc 回调。 它不应调入分析 API 来执行其他任务，如堆栈采样或分离。

### <a name="conditionalweaktablereferences"></a>ConditionalWeakTableReferences

从 .NET Framework 4.5 开始，有一个新的 GC 回调 [ConditionalWeakTableElementReferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md)，它为探查器提供有关 *依赖句柄* 的更完整信息。 出于 GC 生存期管理的目的，这些处理会有效地将从源对象的引用添加到目标对象。 从属句柄并不新，并且在托管代码中编程的开发人员可以使用类创建自己的依赖句柄， <xref:System.Runtime.CompilerServices.ConditionalWeakTable%602?displayProperty=nameWithType> 即使在 Windows 8 和 .NET Framework 4.5 之前也是如此。

但是，托管的 XAML Windows 应用商店应用现在会大量使用依赖句柄。 具体而言，CLR 使用它们来帮助管理托管对象和非托管 Windows 运行时对象之间的引用周期。 这意味着，与以往相比，要通知这些从属句柄的内存探查器比以往更重要，这样，就可以与堆图中的其余边缘一起可视化。 探查器 DLL 应将 [RootReferences2](icorprofilercallback2-rootreferences2-method.md)、 [ObjectReferences](icorprofilercallback-objectreferences-method.md)和 [ConditionalWeakTableElementReferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md) 一起使用，以形成堆关系图的完整视图。

## <a name="conclusion"></a>结论

可以使用 CLR 分析 API 来分析 Windows 应用商店应用内运行的托管代码。 事实上，你可以获取正在开发的现有探查器，并进行一些具体的更改，使其面向 Windows 应用商店应用。 探查器 UI 应使用新的 Api 在调试模式下激活 Windows 应用商店应用。 请确保探查器 DLL 仅使用适用于 Windows 应用商店应用的 Api。 探查器 DLL 和探查器 UI 之间的通信机制应该在编写时考虑到 Windows 应用商店应用程序 API 限制，并了解 Windows 应用商店应用程序的受限权限。 探查器 DLL 应知道 CLR 如何处理 Winmd，以及垃圾回收器的行为与托管线程的行为是不同的。

## <a name="resources"></a>资源

**公共语言运行时**

- [分析 (非托管 API 参考) ](index.md)

- [ (非托管 API 参考的元数据) ](../metadata/index.md)

**CLR 与 Windows 运行时的交互**

- [.NET Framework 对 Windows 应用商店应用和 Windows 运行时的支持情况](../../cross-platform/support-for-windows-store-apps-and-windows-runtime.md)

**Windows 应用商店应用程序**

- [Windows 运行时应用 (的文件访问和权限](/previous-versions/windows/apps/hh967755(v=win.10))

- [获取开发人员许可证](/previous-versions/windows/apps/hh974578(v=win.10))

- [IPackageDebugSettings 接口](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-ipackagedebugsettings)
