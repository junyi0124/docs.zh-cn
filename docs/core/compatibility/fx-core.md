---
title: 从 .NET Framework 到 .NET Core 的中断性变更
titleSuffix: ''
description: 列出了从 .NET Framework 到 .NET Core 1.0 - 3.1 的中断性变更。
ms.date: 05/05/2020
ms.openlocfilehash: 5904a359813b6d07bd2a27d882ade4395efe3256
ms.sourcegitcommit: 1e8382d0ce8b5515864f8fbb178b9fd692a7503f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2020
ms.locfileid: "89656361"
---
# <a name="breaking-changes-for-migration-from-net-framework-to-net-core"></a>从 .NET Framework 迁移到 .NET Core 的中断性变更

若要将应用从 .NET Framework 迁移到 .NET Core 1.0 至 3.1，本文中列出的中断性变更可能会影响你。 中断性变更按类别进行分组，并且在这些类别中按引入的 .NET Core 版本进行分组。

> [!NOTE]
> 本文不是 .NET Framework 与 .NET Core 之间的中断性变更的完整列表。 在我们发现最重要的中断性变更时，会将其添加到此处。

## <a name="core-net-libraries"></a>Core .NET 库

- [UseShellExecute 默认值更改](#change-in-default-value-of-useshellexecute)
- [FileSystemInfo.Attributes 引发的 UnauthorizedAccessException](#unauthorizedaccessexception-thrown-by-filesysteminfoattributes)
- [不支持处理损坏的进程状态异常](#handling-corrupted-state-exceptions-is-not-supported)
- [UriBuilder 属性不再预置前导字符](#uribuilder-properties-no-longer-prepend-leading-characters)
- [Process.StartInfo 对未启动的进程引发 InvalidOperationException](#processstartinfo-throws-invalidoperationexception-for-processes-you-didnt-start)

### <a name="net-core-21"></a>.NET Core 2.1

[!INCLUDE[Process.Start changes](~/includes/core-changes/corefx/2.1/process-start-changes.md)]

***

### <a name="net-core-10"></a>.NET Core 1.0

[!INCLUDE [UnauthorizedAccessException thrown by FileSystemInfo.Attributes](~/includes/core-changes/corefx/1.0/filesysteminfo-attributes-exceptions.md)]

***

[!INCLUDE [corrupted-state-exceptions](~/includes/core-changes/corefx/1.0/corrupted-state-exceptions.md)]

***

[!INCLUDE [uribuilder-behavior-changes](../../../includes/core-changes/corefx/1.0/uribuilder-behavior-changes.md)]

***

[!INCLUDE [startinfo-throws-exception](../../../includes/core-changes/corefx/1.0/startinfo-throws-exception.md)]

***

## <a name="cryptography"></a>密码

- [已考虑 SignedCms.ComputeSignature 的布尔参数](#boolean-parameter-of-signedcmscomputesignature-is-respected)

### <a name="net-core-21"></a>.NET Core 2.1

[!INCLUDE [Boolean parameter of SignedCms.ComputeSignature is respected](~/includes/core-changes/cryptography/2.1/compute-signature-silent-parameter.md)]

***

## <a name="msbuild"></a>MSBuild

- [资源清单文件名更改](#resource-manifest-file-name-change)

### <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE[Resource file names](~/includes/core-changes/msbuild/3.0/resource-manifest-name.md)]

***

## <a name="networking"></a>网络

- [WebClient.CancelAsync 并不总是立即取消](#webclientcancelasync-doesnt-always-cancel-immediately)

### <a name="net-core-20"></a>.NET Core 2.0

[!INCLUDE [behavior-change-webclient-cancelasync](../../../includes/core-changes/networking/2.0/behavior-change-webclient-cancelasync.md)]

***

## <a name="windows-forms"></a>Windows 窗体

已在 .NET Core 的版本 3.0 中添加 Windows 窗体支持。 如果将 Windows 窗体应用从 .NET Framework 迁移到 .NET Core，此处列出的中断性变更可能会影响你的应用。

- [已删除的控件](#removed-controls)
- [如果显示工具提示，则不引发 CellFormatting 事件](#cellformatting-event-not-raised-if-tooltip-is-shown)
- [Control.DefaultFont 已更改为 Segoe UI 9 pt](#default-control-font-changed-to-segoe-ui-9-pt)
- [FolderBrowserDialog 的现代化](#modernization-of-the-folderbrowserdialog)
- [已从一些 Windows 窗体类型中删除 SerializableAttribute](#serializableattribute-removed-from-some-windows-forms-types)
- [不支持 AllowUpdateChildControlIndexForTabControls 兼容性开关](#allowupdatechildcontrolindexfortabcontrols-compatibility-switch-not-supported)
- [不支持 DomainUpDown.UseLegacyScrolling 兼容性开关](#domainupdownuselegacyscrolling-compatibility-switch-not-supported)
- [不支持 DoNotLoadLatestRichEditControl 兼容性开关](#donotloadlatestricheditcontrol-compatibility-switch-not-supported)
- [不支持 DoNotSupportSelectAllShortcutInMultilineTextBox 兼容性开关](#donotsupportselectallshortcutinmultilinetextbox-compatibility-switch-not-supported)
- [不支持 DontSupportReentrantFilterMessage 兼容性开关](#dontsupportreentrantfiltermessage-compatibility-switch-not-supported)
- [不支持 EnableVisualStyleValidation 兼容性开关](#enablevisualstylevalidation-compatibility-switch-not-supported)
- [不支持 UseLegacyContextMenuStripSourceControlValue 兼容性开关](#uselegacycontextmenustripsourcecontrolvalue-compatibility-switch-not-supported)
- [不支持 UseLegacyImages 兼容性开关](#uselegacyimages-compatibility-switch-not-supported)

### <a name="net-core-31"></a>.NET Core 3.1

[!INCLUDE[Removed controls](~/includes/core-changes/windowsforms/3.1/remove-controls-3.1.md)]

***

[!INCLUDE[CellFormatting event](~/includes/core-changes/windowsforms/3.1/cellformatting-event-not-raised.md)]

***

### <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE[Control.DefaultFont changed to Segoe UI 9 pt](~/includes/core-changes/windowsforms/3.0/control-defaultfont-changed.md)]

***

[!INCLUDE[Modernization of the FolderBrowserDialog](~/includes/core-changes/windowsforms/3.0/modernized-folderbrowserdialog.md)]

***

[!INCLUDE[SerializableAttribute removed from some Windows Forms types](~/includes/core-changes/windowsforms/3.0/remove-serializationattribute.md)]

***

[!INCLUDE[AllowUpdateChildControlIndexForTabControls compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-allowupdatechildcontrolindexfortabcontrols.md)]

***

[!INCLUDE[DomainUpDown.UseLegacyScrolling compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyscrolling.md)]

***

[!INCLUDE[DoNotLoadLatestRichEditControl compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotloadlatestricheditcontrol.md)]

***

[!INCLUDE[DoNotSupportSelectAllShortcutInMultilineTextBox compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotsupportselectallshortcutinmultilinetextbox.md)]

***

[!INCLUDE[DontSupportReentrantFilterMessage compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-dontsupportreentrantfiltermessage.md)]

***

[!INCLUDE[EnableVisualStyleValidation compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-enablevisualstylevalidation.md)]

***

[!INCLUDE[UseLegacyContextMenuStripSourceControlValue compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacycontextmenustripsourcecontrolvalue.md)]

***

[!INCLUDE[UseLegacyImages compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyimages.md)]

***

## <a name="see-also"></a>请参阅

- [始终在 .NET Core 上引发异常的 API](unsupported-apis.md)
- [.NET Framework 技术在 .NET Core 上不可用](../porting/net-framework-tech-unavailable.md)
