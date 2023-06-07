---
title: Windows 窗体重大更改
description: 列出 .NET Core 3.0 和 3.1 的 Windows 窗体中断性变更。
ms.date: 09/08/2020
ms.openlocfilehash: b944f7eea89b61f41feb8eef99e949c2d6017960
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726426"
---
# <a name="breaking-changes-in-windows-forms-for-net-core-30-and-31"></a>.NET Core 3.0 和 3.1 的 Windows 窗体中断性变更

已在 .NET Core 的版本 3.0 中添加 Windows 窗体支持。 本文按引入了中断性变更的 .NET 版本列出了 Windows 窗体的中断性变更。 如果要从 .NET Framework 或 .NET Core 的早期版本（3.0 或更高版本）升级 Windows 窗体应用，本文能为你提供帮助。

本页记录了以下中断性变更：

| 重大更改 | 引入的版本 |
| - | :-: |
| [已删除的控件](#removed-controls) | 3.1 |
| [如果显示工具提示，则不引发 CellFormatting 事件](#cellformatting-event-not-raised-if-tooltip-is-shown) | 3.1 |
| [Control.DefaultFont 已更改为 Segoe UI 9 pt](#default-control-font-changed-to-segoe-ui-9-pt) | 3.0 |
| [FolderBrowserDialog 的现代化](#modernization-of-the-folderbrowserdialog) | 3.0 |
| [已从一些 Windows 窗体类型中删除 SerializableAttribute](#serializableattribute-removed-from-some-windows-forms-types) | 3.0 |
| [不支持 AllowUpdateChildControlIndexForTabControls 兼容性开关](#allowupdatechildcontrolindexfortabcontrols-compatibility-switch-not-supported) | 3.0 |
| [不支持 DomainUpDown.UseLegacyScrolling 兼容性开关](#domainupdownuselegacyscrolling-compatibility-switch-not-supported) | 3.0 |
| [不支持 DoNotLoadLatestRichEditControl 兼容性开关](#donotloadlatestricheditcontrol-compatibility-switch-not-supported) | 3.0 |
| [不支持 DoNotSupportSelectAllShortcutInMultilineTextBox 兼容性开关](#donotsupportselectallshortcutinmultilinetextbox-compatibility-switch-not-supported) | 3.0 |
| [不支持 DontSupportReentrantFilterMessage 兼容性开关](#dontsupportreentrantfiltermessage-compatibility-switch-not-supported) | 3.0 |
| [不支持 EnableVisualStyleValidation 兼容性开关](#enablevisualstylevalidation-compatibility-switch-not-supported) | 3.0 |
| [不支持 UseLegacyContextMenuStripSourceControlValue 兼容性开关](#uselegacycontextmenustripsourcecontrolvalue-compatibility-switch-not-supported) | 3.0 |
| [不支持 UseLegacyImages 兼容性开关](#uselegacyimages-compatibility-switch-not-supported) | 3.0 |

## <a name="net-core-31"></a>.NET Core 3.1

[!INCLUDE[Removed controls](~/includes/core-changes/windowsforms/3.1/remove-controls-3.1.md)]

***

[!INCLUDE[CellFormatting event](~/includes/core-changes/windowsforms/3.1/cellformatting-event-not-raised.md)]

**_

## <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE[Control.DefaultFont changed to Segoe UI 9pt](~/includes/core-changes/windowsforms/3.0/control-defaultfont-changed.md)]

_*_

[!INCLUDE[Modernization of the FolderBrowserDialog](~/includes/core-changes/windowsforms/3.0/modernized-folderbrowserdialog.md)]

_*_

[!INCLUDE[SerializableAttribute removed from some Windows Forms types](~/includes/core-changes/windowsforms/3.0/remove-serializationattribute.md)]

_*_

[!INCLUDE[Switch.System.Windows.Forms.AllowUpdateChildControlIndexForTabControls compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-allowupdatechildcontrolindexfortabcontrols.md)]

_*_

[!INCLUDE[Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyscrolling.md)]

_*_

[!INCLUDE[Switch.System.Windows.Forms.DoNotLoadLatestRichEditControl compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotloadlatestricheditcontrol.md)]

_*_

[!INCLUDE[Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotsupportselectallshortcutinmultilinetextbox.md)]

_*_

[!INCLUDE[Switch.System.Windows.Forms.DontSupportReentrantFilterMessage compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-dontsupportreentrantfiltermessage.md)]

_*_

[!INCLUDE[Switch.System.Windows.Forms.EnableVisualStyleValidation compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-enablevisualstylevalidation.md)]

_*_

[!INCLUDE[Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacycontextmenustripsourcecontrolvalue.md)]

_*_

[!INCLUDE[Switch.System.Windows.Forms.UseLegacyImages compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyimages.md)]

_**

## <a name="see-also"></a>请参阅

- [将 Windows 窗体应用移植到 .NET Core](/dotnet/desktop/winforms/migration/?view=netdesktop-5.0&preserve-view=true)
