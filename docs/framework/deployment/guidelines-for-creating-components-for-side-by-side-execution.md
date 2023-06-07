---
title: 并行执行的组件的创建指南
description: 查看有关创建并行执行的组件的指南。 例如，将类型标识绑定到某个文件版本，或使用可识别版本的存储。
ms.date: 03/30/2017
helpviewer_keywords:
- side-by-side execution, multiple application versions
- side-by-side execution, multiple component versions
ms.assetid: 5c540161-6e40-42e9-be92-6175aee2c46a
ms.openlocfilehash: 3ac7c514a69ae05b00e7a486aadcbf41e5d1cbd6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262510"
---
# <a name="guidelines-for-creating-components-for-side-by-side-execution"></a>并行执行的组件的创建指南

创建适用于并行执行的托管应用程序或组件时，请遵循下列一般性准则：  
  
- 将类型标识绑定到文件的特定版本。  
  
     通过使用具有强名称的程序集，公共语言运行时将类型标识绑定到文件的特定版本。 若要创建适用于并行执行的应用程序或组件，你必须为所有的程序集提供强名称。 这样可以创建精确的类型标识，并确保任何类型解析都定向到正确的文件。 具有强名称的程序集包含版本、区域性和发行者信息，运行时使用这些信息来正确地定位文件，从而满足绑定请求。  
  
- 使用可以区别版本的存储。  
  
     运行时使用全局程序集缓存提供可以区别版本的存储。 全局程序集缓存是一个可以区别版本的目录结构，安装在使用 .NET Framework 的每台计算机上。 安装在全局程序集缓存中的程序集不会在安装该程序集的新版本时被覆盖。  
  
- 创建隔离运行的应用程序或组件。  
  
     隔离运行的应用程序或组件必须管理资源，以避免该应用程序或组件的两个实例在同时运行时发生冲突。 应用程序或组件还必须使用版本特定的文件结构。  
  
## <a name="application-and-component-isolation"></a>应用程序和组件的隔离  

 对于并行执行的应用程序或组件，设计成功的关键是隔离。 应用程序或组件必须以隔离方式管理所有的资源，尤其是文件 I/O。 请遵循以下准则，确保你的应用程序或组件以隔离方式运行：  
  
- 按照版本特定的方式，写入注册表。 将指示版本的值存储在配置单元或项中，同时，不要在组件的不同版本间共享信息或状态。 这就防止了同时运行的两个应用程序或组件覆盖信息。  
  
- 使命名的内核对象成为版本特定的内核对象，以避免发生争用条件。 例如，当来自同一应用程序的两个版本的两个信号灯互相等待时，就发生了争用条件。  
  
- 使文件名和目录名可以区别版本。 这意味着文件结构应该依赖于版本信息。  
  
- 按照版本特定的方式，创建用户帐户和组。 应用程序创建的用户帐户和组应该由版本识别。 不要在应用程序的不同版本间共享用户帐户和组。  
  
## <a name="installing-and-uninstalling-versions"></a>安装或卸载版本  

 在设计并行执行的应用程序时，请遵循以下关于安装和卸载版本的准则：  
  
- 请不要从注册表中删除在 .NET Framework 的其他版本下运行的其他应用程序可能需要的信息。  
  
- 请不要在注册表中替换在 .NET Framework 的其他版本下运行的其他应用程序可能需要的信息。  
  
- 请不要注销在 .NET Framework 的其他版本下运行的其他应用程序可能需要的 COM 组件。  
  
- 对于已注册的 COM 服务器，请不要更改 InprocServer32 或其他注册表项。  
  
- 请不要删除在 .NET Framework 的其他版本下运行的其他应用程序可能需要的用户帐户或组。  
  
- 请不要向注册表中添加任何包含未版本化路径的内容。  
  
## <a name="file-version-number-and-assembly-version-number"></a>文件版本号和程序集版本号  

 文件版本是运行时不使用的 Win32 版本资源。 通常情况下，即使是对于就地更新，也应更新文件版本。 两个相同的文件可以有不同的文件版本信息，而两个不同的文件也可以有相同的文件版本信息。  
  
 运行时使用程序集版本进行程序集绑定。 若两个相同的程序集版本号不同，则运行时将它们视为两个不同的程序集。  
  
 仅当文件版本号更新时，[全局程序集缓存工具 (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md) 才允许用户替换程序集。 安装程序在安装时通常不会覆盖程序集，除非该程序集版本号较大。  
  
## <a name="see-also"></a>请参阅

- [并行执行](side-by-side-execution.md)
- [如何：启用和禁用自动绑定重定向](../configure-apps/how-to-enable-and-disable-automatic-binding-redirection.md)
