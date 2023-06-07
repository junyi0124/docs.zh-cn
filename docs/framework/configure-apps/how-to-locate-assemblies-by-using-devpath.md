---
title: 如何：使用 DEVPATH 查找程序集
description: 使用 XML 计算机配置文件和 DEVPATH 环境变量，测试共享程序集是否可在 .NET 中的许多应用程序中正常工作。
ms.date: 03/30/2017
helpviewer_keywords:
- DEVPATH
- .NET Framework application configuration, assemblies
- application configuration files, specifying assembly's location
- app.config files, assembly locations
- locating assemblies
- assemblies [.NET Framework], location
ms.assetid: 44d2eadf-7eec-443c-a2ac-d601fd919e17
ms.openlocfilehash: 8ae807e46b11d2adb06d6af0c86e1c7297caa0c0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161979"
---
# <a name="how-to-locate-assemblies-by-using-devpath"></a>如何：使用 DEVPATH 查找程序集

开发人员可能希望确保它们所构建的共享程序集在多个应用程序中正常工作。 开发人员可以创建一个 DEVPATH 环境变量，该变量指向程序集的生成输出目录，而不是在开发周期期间将程序集不断置于全局程序集缓存中。  
  
 例如，假设要生成一个名为 MySharedAssembly 的共享程序集，并且输出目录为 C:\MySharedAssembly\Debug。 可以将 C:\MySharedAssembly\Debug 放在 DEVPATH 变量中。 然后必须 [\<developmentMode>](./file-schema/runtime/developmentmode-element.md) 在计算机配置文件中指定元素。 此元素告知公共语言运行时使用 DEVPATH 来定位程序集。  
  
 共享程序集必须可由运行时发现。  若要为解析程序集引用指定一个专用目录，请使用配置文件中的[ \<codeBase> 元素](./file-schema/runtime/codebase-element.md)或[ \<probing> 元素](./file-schema/runtime/probing-element.md)，如[指定程序集的位置](specify-assembly-location.md)中所述。  你还可以将程序集放在应用程序目录的子目录中。 有关详细信息，请参阅 [运行时如何定位程序集](../deployment/how-the-runtime-locates-assemblies.md)。  
  
> [!NOTE]
> 这是一项高级功能，仅用于开发。  
  
 下面的示例演示如何使运行时在由 DEVPATH 环境变量指定的目录中搜索程序集。  
  
## <a name="example"></a>示例  
  
```xml  
<configuration>  
  <runtime>  
    <developmentMode developerInstallation="true"/>  
  </runtime>  
</configuration>  
```  
  
 此设置默认为 false。  
  
> [!NOTE]
> 仅在开发时使用此设置。 运行时不检查在 DEVPATH 中找到的具有强名称的程序集的版本。 它只使用它找到的第一个程序集。  
  
## <a name="see-also"></a>请参阅

- [使用配置文件配置应用](index.md)
