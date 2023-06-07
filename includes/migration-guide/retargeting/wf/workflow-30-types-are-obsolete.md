---
ms.openlocfilehash: 1f85b1ce95ca07329aff77af4ec894622e0818d1
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606357"
---
### <a name="workflow-30-types-are-obsolete"></a>WorkFlow 3.0 类型已过时

#### <a name="details"></a>详细信息

Windows Workflow Foundation (WWF) 3.0 API（来自于 System.Workflow 命名空间）现已过时。

#### <a name="suggestion"></a>建议

应改用新的 WWF 4.0 API（在 System.Activities 中）。 可在[此处](~/docs/framework/windows-workflow-foundation/how-to-update-the-definition-of-a-running-workflow-instance.md)找到使用新 API 的示例，[此处](/archive/blogs/workflowteam/wf3-types-marked-obsolete-in-net-4-5)还提供更深入的指导。 或者，由于仍然支持 WWF 3.0 API，因此可能会使用它们，通过禁止显示生成时警告或使用较旧的编译器可以避免出现该警告。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.5         |
| 类型    | 重定目标 |
