---
title: 对象
ms.date: 07/20/2015
helpviewer_keywords:
- objects [Visual Basic]
ms.assetid: 651c73e4-dca8-402b-9c6b-e3902b3a3f4b
ms.openlocfilehash: 86852b8335c05f496ea7325ae3defd35c7532b38
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875409"
---
# <a name="objects-visual-basic"></a>对象 (Visual Basic)

本主题收录了记录 Visual Basic 运行时对象的其他主题的链接，并包含这些对象的成员过程、属性和事件表。  
  
## <a name="visual-basic-run-time-objects"></a>Visual Basic 运行时对象  
  
|||  
|---|---|  
|<xref:Microsoft.VisualBasic.Collection>|提供了一种便捷方式，将相关的一组项视为一个对象。|  
|<xref:Microsoft.VisualBasic.Information.Err%2A>|包含运行时错误的相关信息。|  
|`My.Application` 对象由以下类组成：<br /><br /> <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase> 提供了所有项目中的可用成员。<br /><br /> <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> 提供了 Windows 窗体应用中的可用成员。<br /><br /> <xref:Microsoft.VisualBasic.ApplicationServices.ConsoleApplicationBase> 提供了控制台应用中的可用成员。|提供了仅与当前应用或 DLL 相关联的数据。 无法使用 `My.Application` 更改任何系统级信息。<br /><br /> 一些成员仅对 Windows 窗体应用或控制台应用可用。|  
|`My.Application.Info` (<xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Info%2A>)|提供了用于获取应用信息（如版本号、说明、已加载程序集等）的属性。|  
|`My.Application.Log` (<xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.Log%2A>)|提供属性和方法来将事件和异常信息写入应用程序的日志侦听器。|  
|`My.Computer` (<xref:Microsoft.VisualBasic.Devices.Computer>)|提供了用于操控计算机组件（如音频、时钟、键盘、文件系统等）的属性。|  
|`My.Computer.Audio` (<xref:Microsoft.VisualBasic.Devices.Audio>)|提供了用于播放音频的方法。|  
|`My.Computer.Clipboard` (<xref:Microsoft.VisualBasic.Devices.Computer.Clipboard%2A>)|提供了用于操控剪贴板的方法。|  
|`My.Computer.Clock` (<xref:Microsoft.VisualBasic.Devices.Clock>)|提供了用于从系统时钟访问当前本地时间和协调世界时（相当于格林威治标准时间）的属性。|  
|`My.Computer.FileSystem` (<xref:Microsoft.VisualBasic.FileIO.FileSystem>)|提供了用于处理驱动器、文件和目录的属性和方法。|  
|`My.Computer.FileSystem.SpecialDirectories` (<xref:Microsoft.VisualBasic.FileIO.SpecialDirectories>)|提供了用于访问经常引用的目录的属性。|  
|`My.Computer.Info` (<xref:Microsoft.VisualBasic.Devices.ComputerInfo>)|提供了用于获取计算机内存、已加载程序集、名称和操作系统相关信息的属性。|  
|`My.Computer.Keyboard` (<xref:Microsoft.VisualBasic.Devices.Keyboard>)|提供了用于访问键盘当前状态（如当前按下了哪些键）的属性，并提供了用于将击键发送到活动窗口的方法。|  
|`My.Computer.Mouse` (<xref:Microsoft.VisualBasic.Devices.Mouse>)|提供了用于获取本地计算机上安装的鼠标的格式和配置信息的属性。|  
|`My.Computer.Network` (<xref:Microsoft.VisualBasic.Devices.Network>)|提供了用于与计算机连接的网络进行交互的属性、事件和方法。|  
|`My.Computer.Ports` (<xref:Microsoft.VisualBasic.Devices.Ports>)|提供了用于访问计算机的串行端口的属性和方法。|  
|`My.Computer.Registry` (<xref:Microsoft.VisualBasic.MyServices.RegistryProxy>)|提供了用于操控注册表的属性和方法。|  
|[My.Forms 对象](my-forms-object.md)|提供了用于访问当前项目中声明的每个 Windows 窗体实例的属性。|  
|`My.Log` (<xref:Microsoft.VisualBasic.Logging.AspLog>)|提供了用于将事件和异常信息写入 Web 应用的应用日志侦听器的属性和方法。|  
|[My.Request 对象](my-request-object.md)|获取所请求的页面的 <xref:System.Web.HttpRequest> 对象。 `My.Request` 对象包含当前 HTTP 请求的相关信息。<br /><br /> `My.Request` 对象仅适用于 ASP.NET 应用程序。|  
|[My.Resources 对象](my-resources-object.md)|提供了用于访问应用资源的属性和类。|  
|[My.Response 对象](my-response-object.md)|获取与 <xref:System.Web.HttpResponse> 关联的 <xref:System.Web.UI.Page> 对象。 使用此对象，可以将 HTTP 响应数据发送到客户端，并包含此响应的相关信息。<br /><br /> `My.Response` 对象仅适用于 ASP.NET 应用程序。|  
|[My.Settings 对象](my-settings-object.md)|提供了用于访问应用设置的属性和方法。|  
|`My.User` (<xref:Microsoft.VisualBasic.ApplicationServices.User>)|提供了对当前用户信息的访问权限。|  
|[My.WebServices 对象](my-webservices-object.md)|提供了用于创建和访问当前项目引用的每个 Web 服务实例的属性。|  
|<xref:Microsoft.VisualBasic.FileIO.TextFieldParser>|提供分析结构化文本文件的方法和属性。|  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 语言参考](../index.md)
