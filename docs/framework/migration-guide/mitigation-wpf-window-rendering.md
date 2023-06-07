---
title: 缓解：WPF 窗口呈现
description: 了解在 Windows 8 或更高版本上运行的 .NET Framework 4.6 中 WPF 窗口呈现的影响和缓解措施。
ms.date: 03/30/2017
ms.assetid: 28ed6bf8-141b-4b73-a4e3-44a99fae5084
ms.openlocfilehash: d624478d17a4076107438f71b0a86eeb6d9f3ea4
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475328"
---
# <a name="mitigation-wpf-window-rendering"></a>缓解：WPF 窗口呈现

在 Windows 8 及更高版本上运行的 .NET Framework 4.6 中，当一个窗口超出多监视器方案中的单个显示屏时，会不经剪辑就呈现整个窗口。

## <a name="impact"></a>影响

通常，跨多个监视器不经剪辑呈现整个窗口是预期行为。 但是，在 Windows 7 和更早版本上，WPF 窗口在超出单个显示屏时将被剪辑，因为在第二个监视器上呈现窗口的一部分具有显著的性能影响。

在 Windows 8 和更高版本上跨监视器呈现 WPF 窗口的具体影响无法精确计量，因为它取决于大量因素。 在某些情况下，它仍可能会对性能产生不良影响，特别是对于运行图形密集型应用程序和具有跨监视器窗口的用户。 在其他情况下，你可能只是希望各 .NET Framework 版本有一致的行为。

## <a name="mitigation"></a>缓解

当窗口超出单个显示屏时，你可以禁用此更改并恢复到以前的剪辑 WPF 窗口的行为。 有两种方法可以实现此目的：

- 通过将 `<EnableMultiMonitorDisplayClipping>` 元素添加到应用程序配置文件的 `<appSettings>` 部分，你可以对在 Windows 8 或更高版本上运行的应用禁用或启用此行为。 例如，下面的配置部分禁用不经剪辑的呈现：

  ```xml
  <appSettings>
      <add key="EnableMultiMonitorDisplayClipping" value="true"/>
    </appSettings>
  ```

  `<EnableMultiMonitorDisplayClipping>` 配置设置可以有两个值之一：

  - `true`，启用在呈现期间监视边界的窗口剪辑。

  - `false`，禁用在呈现期间监视边界的窗口剪辑。

- 通过在应用启动时将 <xref:System.Windows.CoreCompatibilityPreferences.EnableMultiMonitorDisplayClipping%2A> 属性设置为 `true`。

## <a name="see-also"></a>请参阅

- [应用程序兼容性](application-compatibility.md)
