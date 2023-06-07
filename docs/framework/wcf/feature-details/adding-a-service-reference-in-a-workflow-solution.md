---
title: 在工作流解决方案中添加服务引用
ms.date: 03/30/2017
ms.assetid: 83574cf3-9803-49bc-837f-432936dc9c76
ms.openlocfilehash: 1b4cc16d3bd85c28ac7267e88ae0a714620c9861
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90557053"
---
# <a name="add-a-service-reference-in-a-workflow-solution"></a>在工作流解决方案中添加服务引用

在工作流应用程序中添加服务引用后，其工作方式与常规 WCF 应用程序略有不同。 当你选择 "**添加**  >  **服务引用**" 并指定服务的 URL 时，将下载元数据，并生成自定义活动，以允许你调用该 wcf 服务或 wcf 工作流服务。 添加服务引用后，重新生成解决方案，以便构建生成的活动。 之后它们将出现在工作流设计器工具箱中。 仅当在工作流解决方案中添加服务引用时，此操作才有效。 以下 web 转换显示了如何在其他类型的项目中添加服务引用： [从 Web 项目的工作流中调用 WCF 服务](/archive/blogs/endpoint/how-to-consume-a-wcf-service-from-a-wf4-workflow)。

添加对包含相同操作名称的服务的两个或多个服务引用会导致出现问题。 生成的活动将仅调用第一个服务操作。 若要解决此问题，请重命名服务操作，使其不同，或在每个生成的活动中更改终结点配置名称。

## <a name="see-also"></a>请参阅

- [工作流服务](workflow-services.md)
- [从 Web 项目的工作流中调用 WCF 服务](/archive/blogs/endpoint/how-to-consume-a-wcf-service-from-a-wf4-workflow)
