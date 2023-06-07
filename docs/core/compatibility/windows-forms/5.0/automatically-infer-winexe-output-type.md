---
title: 中断性变更：对于 WPF 和 WinForms 应用，OutputType 设置为 WinExe
description: 了解 .NET 5.0 中的以下中断性变更：对于 Windows 窗体应用，OutputType 自动设置为 WinExe。
ms.date: 09/18/2020
ms.openlocfilehash: 072c5b11c8304eb540e176ce9747930789f28505
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759292"
---
# <a name="outputtype-set-to-winexe-for-wpf-and-winforms-apps"></a>对于 WPF 和 WinForms 应用，OutputType 设置为 WinExe

对于 Windows Presentation Foundation (WPF) 和 Windows 窗体应用，`OutputType` 自动设置为 `WinExe`。 如果 `OutputType` 设置为 `WinExe`，则在执行应用时不会打开控制台窗口。

## <a name="change-description"></a>更改说明

在早期版本的 .NET 中，使用为项目文件中的 `OutputType` 指定的值。 例如：

```xml
<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>
```

从 .NET 5.0 开始，对于 WPF 和 Windows 窗体应用，`OutputType` 自动设置为 `WinExe`。 例如：

```xml
<PropertyGroup>
  <OutputType>WinExe</OutputType>
</PropertyGroup>
```

## <a name="reason-for-change"></a>更改原因

假定在执行 WPF 或 Windows 窗体应用时，大多数用户不希望打开控制台窗口。 此外，[这些应用程序类型现在使用 .NET SDK](sdk-and-target-framework-change.md) 而不是 Windows 桌面 SDK，因此会设置正确的默认值。 而且，添加了对面向 iOS 和 Android 的支持时，如果多个平台使用相同的输出类型，则在这些平台之间进行多目标操作将更轻松。

## <a name="version-introduced"></a>引入的版本

.NET 5.0

## <a name="recommended-action"></a>建议操作

你无需执行任何操作。 但是，如果你想还原为旧行为，请在项目文件中将 `DisableWinExeOutputInference` 属性设置为 `true`。

```xml
<DisableWinExeOutputInference>true</DisableWinExeOutputInference>
```

## <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

### Affected APIs

Not detectable via API analysis.

### Category

- Windows Forms
- Windows Presentation Framework (WPF)

-->
