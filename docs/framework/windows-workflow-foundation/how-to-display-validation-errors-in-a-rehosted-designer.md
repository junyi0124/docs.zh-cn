---
title: 如何：在重新承载的设计器中显示验证错误
ms.date: 03/30/2017
ms.assetid: 5aa8fb53-8f75-433b-bc06-7c7d33583d5d
ms.openlocfilehash: ccdb8941f46683dd1fd12387c0c5db2abe1d8dec
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96280034"
---
# <a name="how-to-display-validation-errors-in-a-rehosted-designer"></a>如何：在重新承载的设计器中显示验证错误

本主题介绍如何在重新承载 Windows 工作流设计器中检索和发布验证错误。 这为我们提供了一个用于确认重新承载的设计器中的工作流是否有效的过程。  
  
 此任务包含两个部分。 第一部分提供一个实现 <xref:System.Activities.Presentation.Validation.IValidationErrorService>。  在此接口上要实现一个关键方法 <xref:System.Activities.Presentation.Validation.IValidationErrorService.ShowValidationErrors%2A>，该方法将一个包含有关错误的信息的 <xref:System.Activities.Presentation.Validation.ValidationErrorInfo> 对象的列表传递给调试日志。  实现此接口之后，可通过将该实现的实例发布到编辑上下文来检索错误信息。  
  
### <a name="implement-the-ivalidationerrorservice-interface"></a>实现 IValidationErrorService 接口  
  
1. 下面是一个将验证错误写入调试日志的简单实现的代码示例。  
  
    ```csharp  
    using System.Activities.Presentation.Validation;  
    using System.Collections.Generic;  
    using System.Diagnostics;  
    using System.Linq;  
  
    namespace VariableFinderShell  
    {  
        class DebugValidationErrorService : IValidationErrorService  
        {  
            public void ShowValidationErrors(IList<ValidationErrorInfo> errors)  
            {  
                errors.ToList().ForEach(vei => Debug.WriteLine($"Error: {vei.Message}"));  
            }  
        }  
    }  
    ```  
  
### <a name="publishing-to-the-editing-context"></a>发布到编辑上下文  
  
1. 下面是将此内容发布到编辑上下文的代码。  
  
    ```csharp  
    wd.Context.Services.Publish<IValidationErrorService>(new DebugValidationErrorService());  
    ```
