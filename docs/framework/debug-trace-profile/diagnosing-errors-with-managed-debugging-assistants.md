---
title: 使用托管调试助手诊断错误
description: 通过托管调试助手诊断 .NET 中的错误。 Mda 是与 CLR 结合使用的调试辅助工具，用于提供运行时状态信息。
ms.date: 08/14/2018
f1_keywords:
- EHMDA
helpviewer_keywords:
- run-time error debugging
- managed code, run-time debugging
- resource debugging
- registry, MDAs
- common language runtime, debugging
- MDAs (managed debugging assistants)
- configuration files [.NET Framework], debugging runtime events
- messages, managed debugging assistants
- application configuration files, managed debugging assistants
- debugging [.NET Framework], managed debugging assistants
- environment variables, MDAs
- access violation debugging [.NET Framework]
- diagnostics, managed debugging assistants
- unmanaged code, run-time debugging
- default debug output stream
- memory, debugging
- app.config files, managed debugging assistants
- managed debugging assistants (MDAs)
- debugging [.NET Framework], run-time errors
- trapping events
- runtime, error debugging
- disabling MDAs
- output, managed debugging assistants
- errors [.NET Framework], managed debugging assistants
ms.assetid: 76994ee6-9fa9-4059-b813-26578d24427c
ms.openlocfilehash: ac6fdc09fb057cc55659ce076d37ab96fe2354d1
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85416091"
---
# <a name="diagnose-errors-with-managed-debugging-assistants"></a>用托管调试助手诊断错误

托管调试助手 (MDA) 是调试辅助程序，它与公共语言运行时 (CLR) 一起工作以提供关于运行时状态的信息。 这些助手可生成关于你无法通过其他方式捕获的运行时事件的信息性消息。 可以使用 MDA 隔离在托管代码和非托管代码之间转换时发生的、难以发现的应用程序 Bug。

您可以通过向 Windows 注册表添加项或通过设置环境变量来[启用或禁用](#enable-and-disable-mdas)所有 mda。 可使用应用程序配置设置来启用特定的 MDA。 可以在应用程序的配置文件中为某些单独的 MDA 设置其他配置设置。 由于将在加载运行时后分析这些配置文件，因此必须在托管应用程序启动之前启用 MDA。 不能为已经启动的应用程序启用 MDA。

下表列出了 .NET Framework 附带的 Mda：

|||
|-|-|
|[asynchronousThreadAbort](asynchronousthreadabort-mda.md)|[bindingFailure](bindingfailure-mda.md)|
|[callbackOnCollectedDelegate](callbackoncollecteddelegate-mda.md)|[contextSwitchDeadlock](contextswitchdeadlock-mda.md)|
|[dangerousThreadingAPI](dangerousthreadingapi-mda.md)|[dateTimeInvalidLocalFormat](datetimeinvalidlocalformat-mda.md)|
|[dirtyCastAndCallOnInterface](dirtycastandcalloninterface-mda.md)|[disconnectedContext](disconnectedcontext-mda.md)|
|[dllMainReturnsFalse](dllmainreturnsfalse-mda.md)|[exceptionSwallowedOnCallFromCom](exceptionswallowedoncallfromcom-mda.md)|
|[failedQI](failedqi-mda.md)|[fatalExecutionEngineError](fatalexecutionengineerror-mda.md)|
|[gcManagedToUnmanaged](gcmanagedtounmanaged-mda.md)|[gcUnmanagedToManaged](gcunmanagedtomanaged-mda.md)|
|[illegalPrepareConstrainedRegion](illegalprepareconstrainedregion-mda.md)|[invalidApartmentStateChange](invalidapartmentstatechange-mda.md)|
|[invalidCERCall](invalidcercall-mda.md)|[invalidFunctionPointerInDelegate](invalidfunctionpointerindelegate-mda.md)|
|[invalidGCHandleCookie](invalidgchandlecookie-mda.md)|[invalidIUnknown](invalidiunknown-mda.md)|
|[invalidMemberDeclaration](invalidmemberdeclaration-mda.md)|[invalidOverlappedToPinvoke](invalidoverlappedtopinvoke-mda.md)|
|[invalidVariant](invalidvariant-mda.md)|[jitCompilationStart](jitcompilationstart-mda.md)|
|[loaderLock](loaderlock-mda.md)|[loadFromContext](loadfromcontext-mda.md)|
|[marshalCleanupError](marshalcleanuperror-mda.md)|[marshaling](marshaling-mda.md)|
|[memberInfoCacheCreation](memberinfocachecreation-mda.md)|[moduloObjectHashcode](moduloobjecthashcode-mda.md)|
|[nonComVisibleBaseClass](noncomvisiblebaseclass-mda.md)|[notMarshalable](notmarshalable-mda.md)|
|[openGenericCERCall](opengenericcercall-mda.md)|[overlappedFreeError](overlappedfreeerror-mda.md)|
|[pInvokeLog](pinvokelog-mda.md)|[pInvokeStackImbalance](pinvokestackimbalance-mda.md)|
|[raceOnRCWCleanup](raceonrcwcleanup-mda.md)|[入](reentrancy-mda.md)|
|[releaseHandleFailed](releasehandlefailed-mda.md)|[reportAvOnComRelease](reportavoncomrelease-mda.md)|
|[streamWriterBufferedDataLost](streamwriterbuffereddatalost-mda.md)|[virtualCERCall](virtualcercall-mda.md)|

默认情况下，.NET Framework 会为所有托管调试器激活 MDA 的子集。 可以通过选择**Windows**  >  "**调试**" 菜单上的 "Windows**异常设置**"，然后展开 "**托管调试助手**" 列表，在 Visual Studio 中查看默认设置。

![Visual Studio 中的 "异常设置" 窗口](./media/diagnosing-errors-with-managed-debugging-assistants/exception-settings-mdas.png)

## <a name="enable-and-disable-mdas"></a>启用和禁用 Mda

可使用注册表项、环境变量和应用程序配置设置，启用和禁用 MDA。 若要使用应用程序配置设置，必须启用注册表项或环境变量。

> [!TIP]
> 无论何时收到 MDA 通知，都可以阻止 Visual Studio 显示 MDA 对话框，而不是禁用 Mda。 为此，请**Windows**  >  在 "**调试**" 菜单上选择 "Windows**异常设置**"，展开 "**托管调试助手**" 列表，然后选中或清除单个 MDA 的 "**引发时中断**" 复选框。

### <a name="registry-key"></a>注册表项

若要启用 Mda，请添加**HKEY_LOCAL_MACHINE \software\microsoft \\ 。NETFramework\MDA**子项（类型 REG_SZ，值为1）。 将以下示例复制到一个名为*mdaenable.reg*的文本文件中。打开 Windows 注册表编辑器（RegEdit.exe），并从 "**文件**" 菜单中选择 "**导入**"。 选择*mdaenable.reg*文件以在该计算机上启用 mda。 将子项设置为字符串值**1** （不是 DWORD 值**1**）可启用从*APPLICATIONNAME*.mda.config 文件中读取 MDA 设置。 例如，记事本的 MDA 配置文件将命名为 notepad.exe.mda.config。

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework]
"MDA"="1"
```

如果计算机在 64 位操作系统上运行 32 位应用程序，应按下方所示设置 MDA 密钥：

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework]
"MDA"="1"
```

有关详细信息，请参阅[特定于应用程序的配置设置](#application-specific-configuration-settings)。 可以使用 COMPLUS_MDA 环境变量重写注册表设置。 有关详细信息，请参阅[环境变量](#environment-variable)。

若要禁用 Mda，请使用 Windows 注册表编辑器将 MDA 子项设置为**0** （零）。

默认情况下，即使未添加该注册表项，有些 MDA 也会在运行附加到调试器的应用程序时启用。 如本部分前面所述，可以通过运行*mdadisable.reg*文件来禁用这些助手。

### <a name="environment-variable"></a>环境变量

还可以通过环境变量 COMPLUS_MDA 控制 MDA 激活，此变量可重写注册表项。 COMPLUS_MDA 字符串是一个区分大小写、以分号分隔的 MDA 名称列表或其他特殊控制字符串。 在托管或非托管调试器下启动将默认启用一组 MDA。 这是通过在环境变量或注册表项的值前面，隐式附加默认在调试器下启用的、以分号分隔的 MDA 的列表来实现的。 特殊控制字符串如下所示：

- `0` - 停用所有 MDA。

- `1` - 从 ApplicationName.mda.configg 读取 MDA 设置**。

- `managedDebugger` - 显式激活在调试器下启动托管可执行文件时隐式激活的所有 MDA。

- `unmanagedDebugger` - 显式激活在调试器下启动非托管可执行文件时隐式激活的所有 MDA。

如果存在冲突的设置，则最新的设置将重写先前的设置：

- `COMPLUS_MDA=0` 禁用所有 MDA，包括那些在调试器下隐式启用的 MDA。

- 除了调试器下隐式启用的所有 MDA，`COMPLUS_MDA=gcUnmanagedToManaged` 还启用 `gcUnmanagedToManaged`。

- `COMPLUS_MDA=0;gcUnmanagedToManaged` 启用 `gcUnmanagedToManaged`，但是禁用将在调试器下隐式启用的 MDA。

### <a name="application-specific-configuration-settings"></a>特定于应用程序的配置设置

可以在应用程序的 MDA 配置文件中单独地启用、禁用和配置某些助手。 若要使用用于配置 MDA 的应用程序配置文件，必须设置 MDA 注册表项或 COMPLUS_MDA 环境变量。 应用程序配置文件通常与应用程序的可执行文件 (.exe) 位于同一目录中。 文件名的格式为*ApplicationName*.mda.config;例如，notepad.exe.mda.config。在应用程序配置文件中启用的助手可能具有专门设计用于控制该助理行为的属性或元素。

下面的示例演示如何启用和配置[封送处理](marshaling-mda.md)：

```xml
<mdaConfig>
  <assistants>
    <marshaling>
      <methodFilter>
        <match name="*"/>
      </methodFilter>
      <fieldFilter>
        <match name="*"/>
      </fieldFilter>
    </marshaling>
  </assistants>
</mdaConfig>
```

对于应用程序中每个托管到非托管的转换，`Marshaling` MDA 会发出有关正封送到非托管类型的托管类型信息。 `Marshaling`MDA 还可以分别筛选**MethodFilter**和**fieldFilter**子元素中提供的方法和结构字段的名称。

下面的示例演示如何使用默认设置启用多个 Mda：

```xml
<mdaConfig>
  <assistants>
    <illegalPrepareConstrainedRegion />
    <invalidCERCall />
    <openGenericCERCall />
    <virtualCERCall />
  </assistants>
</mdaConfig>
```

> [!IMPORTANT]
> 若在配置文件中指定了多个助手，则必须按字母顺序列出这些助手。 例如，若要同时启用 `virtualCERCall` 和 `invalidCERCall` MDA，则必须先添加 `<invalidCERCall />` 项，再添加 `<virtualCERCall />` 项。 如果这些项未按字母顺序排列，将显示未处理的无效配置文件异常消息。

## <a name="mda-exceptions"></a>MDA 异常

启用 MDA 后，即使代码未在调试器下执行，它也会处于活动状态。 如果在调试器不存在时引发 MDA 事件，尽管这不是未经处理的异常，事件消息也会出现在未经处理的异常对话框中。 若要避免出现该对话框，请在代码未在调试环境中执行时，移除 MDA 启用设置。

当你的代码在 Visual Studio 集成开发环境（IDE）中执行时，你可以避免针对特定 MDA 事件显示的异常对话框。 为此，请在 "**调试**" 菜单上选择 " **Windows**  >  **异常设置**"。 在 "**异常设置**" 窗口中，展开 "**托管调试助手**" 列表，然后清除单个 MDA 的 "**引发时中断**" 复选框。 你还可以使用此对话框来*启用*MDA 异常对话框的显示。

## <a name="mda-output"></a>MDA 输出

MDA 输出类似于下面的示例，该示例显示来自 MDA 的输出 `PInvokeStackImbalance` ：

**调用 PInvoke 函数 "MDATest！MDATest：： StdCall ' 的堆栈不平衡。这很可能是因为托管 PInvoke 签名与非托管目标签名不匹配。检查 PInvoke 签名的调用约定和参数是否与目标非托管签名匹配。**

## <a name="see-also"></a>请参阅

- [调试、跟踪和分析](index.md)
