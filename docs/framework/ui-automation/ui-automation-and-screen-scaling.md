---
title: UI 自动化和屏幕缩放
description: 了解 Windows 和 UI 自动化如何处理屏幕缩放。 DWM 对所有应用程序进行默认缩放，UI 自动化客户端应用必须考虑到这些应用程序。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- scaling, screens
- screens, scaling
- UI (user interface), automation
- UI Automation
ms.assetid: 4380cad7-e509-448f-b9a5-6de042605fd4
ms.openlocfilehash: cf8069f26b85318994aeeb47d42ad28a3a33834a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262523"
---
# <a name="ui-automation-and-screen-scaling"></a>UI 自动化和屏幕缩放

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
从 Windows Vista 开始，Windows 使用户能够更改每英寸的点数 (dpi) 设置，以便大多数用户界面 () UI 上的元素显示更大。 尽管此功能在 Windows 中很长可用，但在以前的版本中，必须由应用程序实现缩放。 从 Windows Vista 开始，桌面窗口管理器对所有不处理其自身缩放的应用程序执行默认缩放。 UI 自动化客户端应用程序必须考虑到此功能。  
  
<a name="Scaling_in_Windows_Vista"></a>

## <a name="scaling-in-windows-vista"></a>Windows Vista 中的缩放  

 默认 dpi 设置为96，这意味着96像素的宽度或高度均为1。 “一英寸”的确切度量取决于监视器的大小和物理分辨率。 例如，在 12 英寸宽、水平分辨率为 1280 像素的监视器上，96 像素的水平线将扩展大约 9/10 英寸。  
  
 更改 dpi 设置与更改屏幕分辨率不同。 通过 dpi 缩放，屏幕上的物理像素数保持不变。 但是，缩放将应用于 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 元素的大小和位置。 桌面窗口管理器 (DWM) 可以自动对桌面和未显式要求缩放的应用程序执行这种扩展。  
  
 实际上，当用户将缩放比例设置为 120 dpi 时，屏幕上的垂直或水平英寸将增大25%。 所有尺寸会相应缩放。 应用程序窗口距屏幕顶端和左边缘的偏移量将增大 25%。 如果启用了应用程序缩放，且应用程序不能识别 dpi，则窗口的大小将以相同的比例增加，以及其包含的所有元素的偏移量和大小 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 。  
  
> [!NOTE]
> 默认情况下，当用户将 dpi 设置为120时，DWM 不会对非 dpi 感知的应用程序执行缩放，但当 dpi 设置为自定义值144或更高时，将执行此功能。 但是，用户可以重写默认行为。  
  
 屏幕缩放为以任何方式与屏幕坐标有关的应用程序带来了新的挑战。 屏幕现在包含两个坐标系：物理和逻辑。 点的物理坐标是距原点左上部的实际偏移量（以像素为单位）。 如果像素本身进行了缩放，则逻辑坐标为缩放后的偏移量。  
  
 假定你设计了一个对话框，其按钮位于坐标 (100, 48) 处。 此对话框在默认的 96 dpi 下显示时，该按钮位于 (100，48) 的物理坐标。 为 120 dpi 时，该位置位于 (125，60) 的物理坐标处。 但在任何 dpi 设置上，逻辑坐标都是相同的： (100、48) 。  
  
 逻辑坐标非常重要，因为它们使操作系统和应用程序的行为保持一致，而不考虑 dpi 设置。 例如，正常情况下， <xref:System.Windows.Forms.Cursor.Position%2A?displayProperty=nameWithType> 将返回逻辑坐标。 如果将光标移至对话框中的某个元素上，则将返回相同的坐标，而不考虑 dpi 设置。 如果在 (100，100) 绘制控件，则该控件将绘制到这些逻辑坐标，并将在任何 dpi 设置中占用相同的相对位置。  
  
<a name="Scaling_in_UI_Automation_Clients"></a>

## <a name="scaling-in-ui-automation-clients"></a>UI 自动化客户端中的缩放  

 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]API 不使用逻辑坐标。 下面的方法和属性将返回物理坐标或将其用作参数。  
  
- <xref:System.Windows.Automation.AutomationElement.GetClickablePoint%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.TryGetClickablePoint%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.ClickablePointProperty>  
  
- <xref:System.Windows.Automation.AutomationElement.FromPoint%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.BoundingRectangle%2A>  
  
 默认情况下，在非 96 dpi 环境中运行的 UI 自动化客户端应用程序将不能从这些方法和属性中获得正确的结果。 例如，由于光标的位置是逻辑坐标，客户端不能仅将这些坐标传递到 <xref:System.Windows.Automation.AutomationElement.FromPoint%2A> 以获取光标下的元素。 此外，该应用程序不能正确将 Windows 放置在其客户端区域之外。  
  
 解决方法分两个部分。  
  
1. 首先，使客户端应用程序可感知 dpi。 为此，请 `SetProcessDPIAware` 在启动时调用 Win32 函数。 在托管代码中，以下声明使得此函数可用。  
  
     [!code-csharp[Highlighter#101](../../../samples/snippets/csharp/VS_Snippets_Wpf/Highlighter/CSharp/NativeMethods.cs#101)]
     [!code-vb[Highlighter#101](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/Highlighter/VisualBasic/NativeMethods.vb#101)]  
  
     此函数使整个进程 dpi 感知，这意味着属于该进程的所有窗口都是无比例的。 例如，在 [荧光笔示例](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/Highlighter)中，构成突出显示矩形的四个窗口位于从 UI 自动化获取的物理坐标上，而非逻辑坐标。 如果该示例不能识别 dpi，则会在桌面上的逻辑坐标处绘制突出显示，这将导致非 96 dpi 环境中出现不正确的位置。  
  
2. 若要获取光标坐标，请调用 Win32 函数 `GetPhysicalCursorPos` 。 下面的示例演示如何声明和使用此函数。  
  
     [!code-csharp[UIAClient_snip#185](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#185)]
     [!code-vb[UIAClient_snip#185](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#185)]  
  
> [!CAUTION]
> 请勿使用 <xref:System.Windows.Forms.Cursor.Position%2A?displayProperty=nameWithType>。 未定义此属性在扩展环境下客户端窗口以外的行为。  
  
 如果你的应用程序使用非 dpi 感知的应用程序执行直接的跨进程通信，则可以使用 Win32 函数和在逻辑和物理坐标之间 `PhysicalToLogicalPoint` 转换 `LogicalToPhysicalPoint` 。  
  
## <a name="see-also"></a>另请参阅

- [Highlighter Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/Highlighter)
