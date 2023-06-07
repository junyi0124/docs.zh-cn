---
title: Web 设置架构
ms.date: 03/30/2017
helpviewer_keywords:
- Web.config configuration file [ASP.NET]
- ASP.NET configuration system, Web settings schema
- schema Web settings
- Web settings, schema [ASP.NET]
- configuration files [ASP.NET]
- configuration schema [.NET Framework], Web settings
ms.assetid: ae1ac356-267d-4753-8d7a-7a04eb45a9be
ms.openlocfilehash: c3ac9b42aeff3f7b26f0b36480bc75ceda39e7e6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91185595"
---
# <a name="web-settings-schema"></a>Web 设置架构

Web 设置指定 CPU 和执行级别 ASP.NET 设置，后者应用于由 ASP.NET 承载层托管的进程范围行为。 这些设置与 ASP.NET 应用程序的 Web.config 文件中指定的应用程序域类型设置不同。  
  
Web 设置包含在 Aspnet.config 文件中，该文件位于各版本 .NET Framework 的安装文件夹内。 例如，.NET Framework 2.0 的 Aspnet.config 文件位于以下文件夹中：  
  
`C:\Windows\Microsoft.NET\Framework\v2.0.50727\`  
  
其他任何配置文件（如 machine.config 文件、根 Web.config 或应用程序级别的 Web.config 文件）中都不使用 Web 设置。  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.web>**](system-web-element-web-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<applicationPool>**](applicationpool-element-web-settings.md)

|元素|描述|  
|-------------|-----------------|  
|[\<system.web>](system-web-element-web-settings.md)|包含 ASP.NET 承载层使用的信息。|  
|[\<applicationPool>](applicationpool-element-web-settings.md)|指定 CPU 和执行级别 ASP.NET 设置，后者应用于由 ASP.NET 承载层托管的进程范围行为。|  
  
## <a name="see-also"></a>请参阅

- [配置文件架构](../index.md)
