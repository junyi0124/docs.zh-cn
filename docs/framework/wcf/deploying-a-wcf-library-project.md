---
title: 部署 WCF 库项目
ms.date: 03/30/2017
ms.assetid: 9f9222fe-d358-443c-9a49-12c5498e35e7
ms.openlocfilehash: e3a90f03639a888b4528a1962a24b7adc5d43c58
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96260885"
---
# <a name="deploying-a-wcf-library-project"></a>部署 WCF 库项目

本主题介绍如何部署 Windows Communication Foundation (WCF) 服务库项目。  
  
## <a name="deploying-a-wcf-service-library"></a>部署 WCF 服务库  

 WCF 服务库是 (DLL) 的动态链接库。 因此，它不能自己运行。 必须将其部署到宿主环境中。 有关此过程的详细信息，请参阅 [托管和使用 WCF 服务](/previous-versions/dotnet/articles/bb332338(v=msdn.10))。  
  
 WCF 服务库可以像任何其他 WCF 服务一样部署。 但请注意，.NET Framework 不支持 Dll 的配置。 <xref:System.Configuration> 支持每个应用程序域一个配置文件。 WCF 服务库项目在开发期间为库提供了一个 App.config 文件，从而减轻了这一限制。 但在部署之后不能识别 App.config 文件。  
  
 必须将配置代码移动到主机环境所识别的配置文件中。 对于自承载，您应将 App.config 文件的内容复制到宿主可执行文件的 App.config 文件中。 如果使用 IIS 承载服务，则应将 App.config 文件的内容复制到虚拟目录的 Web.config 文件中。
