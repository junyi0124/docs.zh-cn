---
ms.openlocfilehash: ddc98448101c65003001ad05e67f29d75d99ad44
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616025"
---
### <a name="workflowdesignerload-doesnt-remove-symbol-property"></a>WorkflowDesigner.Load 不会删除符号属性

#### <a name="details"></a>详细信息

当在工作流设计器中面向 .NET Framework 4.5，并使用 <xref:System.Activities.Presentation.WorkflowDesigner.Load> 方法加载重新托管的 3.5 工作流时，保存该工作流的同时将引发 <xref:System.Xaml.XamlDuplicateMemberException?displayProperty=fullName>。

#### <a name="suggestion"></a>建议

此 bug 仅当在工作流设计器中面向 .NET Framework 4.5 时才显示，因此可通过将 `WorkflowDesigner.Context.Services.GetService&lt;DesignerConfigurationService&gt;().TargetFrameworkName` 设为 4.0 .NET Framework 进行解决。或者，可使用 <xref:System.Activities.Presentation.WorkflowDesigner.Load(System.String)> 方法而非 <xref:System.Activities.Presentation.WorkflowDesigner.Load> 方法加载工作流来避免此问题。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.5         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Activities.Presentation.WorkflowDesigner.Load?displayProperty=nameWithType>
