---
title: 自定义 My 中可用的对象
ms.date: 07/20/2015
helpviewer_keywords:
- My namespace [Visual Basic], customizing
- My namespace
ms.assetid: 4e8279c2-ed5b-4681-8903-8a6671874000
ms.openlocfilehash: 5245c129281bc8c7c1c6fe9215a221889380a901
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410213"
---
# <a name="customizing-which-objects-are-available-in-my-visual-basic"></a>自定义 My 中可用的对象 (Visual Basic)

本主题介绍如何通过设置项目的 `_MYTYPE` 条件编译常量，来控制要启用的 `My` 对象。 Visual Studio 集成开发环境 (IDE) 为与项目类型同步的项目保留 `_MYTYPE` 条件编译常量。  
  
## <a name="predefined-_mytype-values"></a>预定义的 \_MYTYPE 值  

必须使用 `/define` 编译器选项来设置 `_MYTYPE` 条件编译常量。 为 `_MYTYPE` 常量指定自己的值时，必须将字符串值用反斜杠/引号 (\\") 序列括起来。 例如，可以使用：  
  
```console  
/define:_MYTYPE=\"WindowsForms\"  
```  
  
 下表显示了为几个项目类型设置的 `_MYTYPE` 条件编译常量。  
  
|项目类型|\_MYTYPE 值|  
|------------------|--------------------|  
|类库|“Windows”|  
|控制台应用程序|“Console”|  
|Web|“Web”|  
|Web 控件库|“WebControl”|  
|Windows 应用程序|“WindowsForms”|  
|Windows 应用程序（从自定义 `Sub Main` 开始时）|“WindowsFormsWithCustomSubMain”|  
|Windows 控件库|“Windows”|  
|Windows 服务|“Console”|  
|空|“Empty”|  
  
> [!NOTE]
> 不管 `Option Compare` 语句如何设置，所有条件编译字符串比较均区分大小写。  
  
## <a name="dependent-_my-compilation-constants"></a>从属 \_MY 编译常量  

相反，`_MYTYPE` 条件编译常量控制多个其他 `_MY` 编译常量的值：  
  
|\_MYTYPE|\_MYAPPLICATIONTYPE|\_MYCOMPUTERTYPE|\_MYFORMS|\_MYUSERTYPE|\_MYWEBSERVICES|  
|--------------|-------------------------|----------------------|---------------|------------------|---------------------|  
|“Console”|“Console”|“Windows”|未定义|“Windows”|true|  
|“Custom”|未定义|未定义|未定义|未定义|未定义|  
|“Empty”|未定义|未定义|未定义|未定义|未定义|  
|“Web”|未定义|“Web”|false|“Web”|false|  
|“WebControl”|未定义|“Web”|false|“Web”|true|  
|“Windows”或“”|“Windows”|“Windows”|未定义|“Windows”|true|  
|“WindowsForms”|“WindowsForms”|“Windows”|true|“Windows”|true|  
|“WindowsFormsWithCustomSubMain”|“Console”|“Windows”|true|“Windows”|true|  
  
 默认情况下，未定义的条件编译常量解析为 `FALSE`。 在编译项目以覆盖默认行为时，可以为未定义的常量指定值。  
  
> [!NOTE]
> 如果 `_MYTYPE` 设置为“Custom”，则项目包含 `My` 命名空间，但不包含任何对象。 但是，将 `_MYTYPE` 设置为“Empty”将阻止编译器添加 `My` 命名空间及其对象。  
  
 下表描述了 `_MY` 编译常量的预定义值的效果。  
  
|返回的常量|含义|  
|--------------|-------------|  
|`_MYAPPLICATIONTYPE`|如果常量为“Console”、“Windows”或“WindowsForms”，则启用 `My.Application`：<br /><br /> -   “Console”版本派生自 <xref:Microsoft.VisualBasic.ApplicationServices.ConsoleApplicationBase>。 其成员比“Windows”版本少。<br />-   “Windows”版本派生自 <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>。其成员比“WindowsForms”版本少。<br />-   `My.Application` 的“WindowsForms”版本派生自 <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>。 如果 `TARGET` 常量定义为“winexe”，则类包含 `Sub Main` 方法。|  
|`_MYCOMPUTERTYPE`|如果常量为“Web”或“Windows”，则启用 `My.Computer`：<br /><br /> -   “Web”版本派生自 <xref:Microsoft.VisualBasic.Devices.ServerComputer>，其成员比“Windows”版本少。<br />-   `My.Computer` 的“Windows”版本派生自 <xref:Microsoft.VisualBasic.Devices.Computer>。|  
|`_MYFORMS`|如果常量为 `TRUE`，则启用 `My.Forms`。|  
|`_MYUSERTYPE`|如果常量为“Web”或“Windows”，则启用 `My.User`：<br /><br /> -   `My.User` 的“Web”版本与当前 HTTP 请求的用户标识相关联。<br />-   `My.User` 的“Windows”版本与线程的当前主体相关联。|  
|`_MYWEBSERVICES`|如果常量为 `TRUE`，则启用 `My.WebServices`。|  
|`_MYTYPE`|如果常量为“Web”，则启用 `My.Log`、`My.Request` 和 `My.Response`。|  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.Logging.Log>
- <xref:Microsoft.VisualBasic.ApplicationServices.User>
- [My 对项目类型的依赖方式](../development-with-my/how-my-depends-on-project-type.md)
- [条件编译](../../programming-guide/program-structure/conditional-compilation.md)
- [-define (Visual Basic)](../../reference/command-line-compiler/define.md)
- [My.Forms 对象](../../language-reference/objects/my-forms-object.md)
- [My.Request 对象](../../language-reference/objects/my-request-object.md)
- [My.Response 对象](../../language-reference/objects/my-response-object.md)
- [My.WebServices 对象](../../language-reference/objects/my-webservices-object.md)
