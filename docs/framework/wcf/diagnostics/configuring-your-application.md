---
title: 配置应用程序
ms.date: 03/30/2017
ms.assetid: a2f995b0-669d-4721-b00f-4561ec7eb6a4
ms.openlocfilehash: e08e474be02ee11a6727df8b908b53ab1403f7f3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294816"
---
# <a name="configuring-your-application"></a>配置应用程序

Windows Communication Foundation (WCF) 使用 .NET 配置系统，并允许你在计算机和应用程序范围中配置服务。  
  
 WCF 定义的配置设置位于 `<system.serviceModel>` 节组中。 有关如何配置 WCF 服务的详细信息，请参阅以下主题：  
  
- [正在配置服务](../configuring-services.md)  
  
- [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md)  
  
 应用程序定义的配置设置是在 `<appSettings>` 节组中定义的。 有关 .NET 配置文件中的应用程序设置的详细信息，请参阅 [\<appSettings>](/previous-versions/dotnet/netframework-4.0/ms228154(v=vs.100)) 。  
  
## <a name="using-the-configuration-editor"></a>使用配置编辑器  

 [ ( # A0) 的 Wcf 配置编辑器工具](../configuration-editor-tool-svcconfigeditor-exe.md)允许管理员和开发人员使用图形用户界面来创建和修改 WCF 服务的配置设置。 利用此工具，无需直接编辑 XML 配置文件，即可管理 WCF 绑定、行为、服务和诊断的设置。  
  
## <a name="editing-configuration-files-in-visual-studio"></a>在 Visual Studio 中编辑配置文件  

 若要在 Visual Studio 中编辑 WCF 服务项目的配置文件，请在 **解决方案资源管理器** 中右键单击该项目，然后选择 " **编辑 WCF 配置** " 上下文菜单项。 这将启动 [ ( # A0) 的配置编辑器工具 ](../configuration-editor-tool-svcconfigeditor-exe.md)。  
  
> [!NOTE]
> 如果你在 Visual Studio 中编辑 WCF Web Services 项目的配置文件，请在 **解决方案资源管理器** 中右键单击该项目，请注意，" **编辑 wcf 配置** " 上下文菜单项缺失。 若要解决此问题，请单击 " **工具** " 菜单，然后选择 " **WCF 服务配置编辑器**"。 之后，您可以右键单击某个配置文件并使用 " **编辑 WCF 配置** " 上下文菜单项。  
  
## <a name="see-also"></a>另请参阅

- [配置编辑器工具 (SvcConfigEditor.exe)](../configuration-editor-tool-svcconfigeditor-exe.md)
- [正在配置服务](../configuring-services.md)
- [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md)
