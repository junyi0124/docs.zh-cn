---
ms.openlocfilehash: 61d5885c19e39b138090c1a98fa26348724893c5
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496968"
---
### <a name="workflow-now-throws-original-exception-instead-of-nullreferenceexception-in-some-cases"></a>在某些情况下工作流现在引发原始异常，而不是 NullReferenceException

#### <a name="details"></a>详细信息

在 .NET Framework 4.6.2 和更低版本中，当工作流活动的 Execute 方法引发 <xref:System.Exception.Message> 属性 <code>null</code> 值的异常，那么 System.Activities 工作流运行时引发 <xref:System.NullReferenceException?displayProperty=fullName> 掩码原始异常。在 .NET Framework 4.7 中，引发之前掩码的异常。

#### <a name="suggestion"></a>建议

如果你的代码依赖于处理 <xref:System.NullReferenceException?displayProperty=fullName>，请将其更改为捕获自定义活动可能引发的异常。

| 名称    | 值       |
|:--------|:------------|
| 范围   |次要|
|Version|4.7|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Activities.CodeActivity.Execute(System.Activities.CodeActivityContext)?displayProperty=nameWithType>
- <xref:System.Activities.AsyncCodeActivity.BeginExecute(System.Activities.AsyncCodeActivityContext,System.AsyncCallback,System.Object)?displayProperty=nameWithType>
- <xref:System.Activities.AsyncCodeActivity%601.BeginExecute(System.Activities.AsyncCodeActivityContext,System.AsyncCallback,System.Object)?displayProperty=nameWithType>
- <xref:System.Activities.WorkflowInvoker.Invoke?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Activities.CodeActivity.Execute(System.Activities.CodeActivityContext)`
- `M:System.Activities.AsyncCodeActivity.BeginExecute(System.Activities.AsyncCodeActivityContext,System.AsyncCallback,System.Object)`
- ``M:System.Activities.AsyncCodeActivity`1.BeginExecute(System.Activities.AsyncCodeActivityContext,System.AsyncCallback,System.Object)``
- `M:System.Activities.WorkflowInvoker.Invoke`

-->
