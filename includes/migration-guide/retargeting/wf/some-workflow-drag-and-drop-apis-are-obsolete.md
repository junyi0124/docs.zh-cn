---
ms.openlocfilehash: b6cb7edcd6bed50efdf59f3321320ac8cd1b1ab8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617127"
---
### <a name="some-workflow-drag-and-drop-apis-are-obsolete"></a>某些工作流拖放 API 已过时

#### <a name="details"></a>详细信息

此工作流拖放 API 已过时，如果针对 4.5 重新生成应用，将引发编译器警告。

#### <a name="suggestion"></a>建议

应改用支持包含多个对象的操作的新 <xref:System.Activities.Presentation.DragDropHelper?displayProperty=fullName> API。 或者，可以禁止显示生成警告，也可以使用较早的编译器避免出现此类警告。 API 仍受支持。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.5         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Activities.Presentation.DragDropHelper.DoDragMove(System.Activities.Presentation.WorkflowViewElement,System.Windows.Point)?displayProperty=nameWithType>
- <xref:System.Activities.Presentation.DragDropHelper.GetCompositeView(System.Windows.DragEventArgs)?displayProperty=nameWithType>
- <xref:System.Activities.Presentation.DragDropHelper.GetDraggedModelItem(System.Windows.DragEventArgs)?displayProperty=nameWithType>
- <xref:System.Activities.Presentation.DragDropHelper.GetDroppedObject(System.Windows.DependencyObject,System.Windows.DragEventArgs,System.Activities.Presentation.EditingContext)?displayProperty=nameWithType>
