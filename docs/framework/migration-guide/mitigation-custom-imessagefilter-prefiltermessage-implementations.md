---
title: 缓解：自定义 IMessageFilter.PreFilterMessage 实现
description: 了解面向 .NET Framework 4.6.1 及更高版本的 Windows 窗体应用中包含的自定义 IMessageFilter.PreFilterMessage 实现。
ms.date: 03/30/2017
ms.assetid: 9cf47c5b-0bb2-45df-9437-61cd7e7c2f4d
ms.openlocfilehash: 5fe7500d3ed6ff293514495df150a747e7946dda
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475250"
---
# <a name="mitigation-custom-imessagefilterprefiltermessage-implementations"></a>缓解：自定义 IMessageFilter.PreFilterMessage 实现

在面向从 .NET Framework 4.6.1 开始的 .NET Framework 版本的 Windows 窗体应用中，如果 <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType> 实现可以满足以下要求，那么在调用 <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> 方法时，自定义实现 <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType> 可以安全地筛选消息：

- 执行下列一项或两项操作：

  - 通过调用 <xref:System.Windows.Forms.Application.AddMessageFilter%2A> 方法添加消息筛选器。

  - 通过调用 <xref:System.Windows.Forms.Application.RemoveMessageFilter%2A> 方法，删除消息筛选器。 方法。

- 以及通过调用 <xref:System.Windows.Forms.Application.DoEvents%2A?displayProperty=nameWithType> 方法抽取消息。

## <a name="impact"></a>影响

此更改仅影响面向从 .NET Framework 4.6.1 开始的 .NET Framework 版本的 Windows 窗体应用。

对于面向以前版本的 .NET framework 的 Windows 窗体应用程序，在调用 <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> 方法时，此类实现在某些情况下会引发 <xref:System.IndexOutOfRangeException> 异常

## <a name="mitigation"></a>缓解

如果此更改不可取，面向 .NET Framework 4.6.1 或更高版本的应用可通过将以下配置设置添加到应用配置文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分，来选择放弃更改：

```xml
<runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Forms.DontSupportReentrantFilterMessage=true" />
</runtime>
```

此外，面向 .NET Framework 以前版本但在 .NET Framework 4.6.1 或更高版本下运行的应用可通过将以下配置设置添加到应用配置文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分，来选择实现此行为：

```xml
<runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Forms.DontSupportReentrantFilterMessage=false" />
</runtime>
```

## <a name="see-also"></a>请参阅

- [应用程序兼容性](application-compatibility.md)
