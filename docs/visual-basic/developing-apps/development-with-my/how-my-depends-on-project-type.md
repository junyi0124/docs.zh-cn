---
title: My 对项目类型的依赖方式
ms.date: 07/20/2015
helpviewer_keywords:
- _MYTYPE
ms.assetid: c188b38e-bd9d-4121-9983-41ea6a94d28e
ms.openlocfilehash: 740185d8030c09e8813bc7680b451f6326588593
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411709"
---
# <a name="how-my-depends-on-project-type-visual-basic"></a>My 对项目类型的依赖方式 (Visual Basic)

`My` 只公开那些特定项目类型所需的对象。 例如，`My.Forms` 对象在 Windows 窗体应用程序中可用，但在控制台应用程序中不可用。 本主题介绍不同项目类型中可用的 `My` 对象。  
  
## <a name="my-in-windows-applications-and-web-sites"></a>Windows 应用程序和网站中的 My  

 `My` 仅公开可用于当前项目类型的对象；它禁止显示不适用的对象。 例如，下图显示了 Windows 窗体项目中的 `My` 对象模型。  
  
 ![在 Windows 窗体应用程序中显示 My 对象模型的关系图。](./media/how-my-depends-on-project-type/my-object-model-windows-forms.png)  
  
 在网站项目中，`My` 公开与 Web 开发人员相关的对象（如 `My.Request` 和 `My.Response` 对象），同时禁止显示不相关的对象（例如 `My.Forms` 对象）。 下图显示网站项目中的 `My` 对象模型：  
  
 ![显示 Web 应用程序中的 My 对象模型的关系图。](./media/how-my-depends-on-project-type/my-object-model-web.png)  
  
## <a name="project-details"></a>项目详细信息  

 下表显示了默认情况下为八个项目类型启用的 `My` 对象：Windows 应用程序、类库、控制台应用程序、Windows 控件库、Web 控件库、Windows 服务、空和网站。  
  
 有三个版本的 `My.Application` 对象、两个版本的 `My.Computer` 对象以及两个版本的 `My.User` 对象；表后面的脚注中提供了有关这些版本的详细信息。  
  
|My 对象|Windows 应用程序|类库|控制台应用程序|Windows 控件库|Web 控件库|Windows 服务|空|网站|  
|---|---|---|---|---|---|---|---|---|  
|`My.Application`|是  <sup>1</sup>|是  <sup>2</sup>|是  <sup>3</sup>|是  <sup>2</sup>|否|是  <sup>3</sup>|否|否|  
|`My.Computer`|是  <sup>4</sup>|是  <sup>4</sup>|是  <sup>4</sup>|是  <sup>4</sup>|是  <sup>5</sup>|是  <sup>4</sup>|否|是  <sup>5</sup>|  
|`My.Forms`|**是**|否|否|**是**|否|否|否|否|  
|`My.Log`|否|否|否|否|否|否|否|**是**|  
|`My.Request`|否|否|否|否|否|否|否|**是**|  
|`My.Resources`|**是**|**是**|**是**|**是**|**是**|**是**|否|否|  
|`My.Response`|否|否|否|否|否|否|否|**是**|  
|`My.Settings`|**是**|**是**|**是**|**是**|**是**|**是**|否|否|  
|`My.User`|是  <sup>6</sup>|是  <sup>6</sup>|是  <sup>6</sup>|是  <sup>6</sup>|是  <sup>7</sup>|是  <sup>6</sup>|否|是  <sup>7</sup>|  
|`My.WebServices`|**是**|**是**|**是**|**是**|**是**|**是**|否|否|  
  
 <sup>1</sup> `My.Application` 的 Windows 窗体版本。 派生自控制台版本（请参见备注 3）；添加了对与应用程序窗口交互的支持，并提供了 Visual Basic 应用程序模型。  
  
 <sup>2</sup> `My.Application` 的库版本。 提供应用程序所需的基本功能：提供写入应用程序日志和访问应用程序信息的成员。  
  
 <sup>3</sup> `My.Application` 的控制台版本。 派生自库版本（请参见备注 2），并添加用于访问应用程序的命令行参数和 ClickOnce 部署信息的其他成员。  
  
 <sup>4</sup> `My.Computer` 的 Windows 版本。 派生自 Server 版本（请参见备注 5），并提供对客户端计算机上有用对象（如键盘、屏幕和鼠标）的访问。  
  
 <sup>5</sup> `My.Computer` 的 Server 版本。 提供有关计算机的基本信息，如名称、对时钟的访问等。  
  
 <sup>6</sup> `My.User` 的 Windows 版本。 此对象与线程的当前标识相关联。  
  
 <sup>7</sup> `My.User` 的 Web 版本。 此对象与应用程序的当前 HTTP 请求的用户标识相关联。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.Logging.Log>
- <xref:Microsoft.VisualBasic.ApplicationServices.User>
- [自定义 My 中可用的对象](../customizing-extending-my/customizing-which-objects-are-available-in-my.md)
- [条件编译](../../programming-guide/program-structure/conditional-compilation.md)
- [-define (Visual Basic)](../../reference/command-line-compiler/define.md)
- [My.Forms 对象](../../language-reference/objects/my-forms-object.md)
- [My.Request 对象](../../language-reference/objects/my-request-object.md)
- [My.Response 对象](../../language-reference/objects/my-response-object.md)
- [My.WebServices 对象](../../language-reference/objects/my-webservices-object.md)
