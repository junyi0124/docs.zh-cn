---
title: 免注册 COM 互操作
description: 使用免注册 COM 互操作，在不使用 Windows 注册表来存储程序集信息的情况下激活组件。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], interop
- COM interop, registration-free COM interop
- registration-free COM interop
- manifests [.NET Framework]
- registration-free activation
- object activation
- registration-free COM interop, about registration-free COM interop
ms.assetid: 90f308b9-82dc-414a-bce1-77e0155e56bd
ms.openlocfilehash: f61ad954215aecfc9380a47de788d36f327ab0b3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239531"
---
# <a name="registration-free-com-interop"></a>免注册 COM 互操作

免注册 COM 互操作在不使用 Windows 注册表来存储程序集信息的情况下激活组件。 不是在部署过程中在计算机上注册组件，而是在设计时创建包含有关绑定和激活信息的 Win32 样式清单文件。 正是这些清单文件（而不是注册表项）指导对象的激活。  
  
 不在部署期间注册程序集而使用免注册激活具有两大优势：  
  
- 计算机上安装了多个 DLL 版本时，你可以控制要激活的版本。  
  
- 最终用户可以使用 XCOPY 或 FTP 将应用程序复制到计算机上适当的目录。 然后即可从该目录运行该应用程序。  
  
 本节介绍免注册 COM 互操作所需的两种清单类型：应用程序清单和组件清单。 这些清单是 XML 文件。 应用程序清单由应用程序开发人员创建，包含描述程序集和程序集依赖项的元数据。 组件清单由组件开发人员创建，包含 Windows 注册表中的其他信息。  
  
### <a name="requirements-for-registration-free-com-interop"></a>免注册 COM 互操作的需求  
  
1. 对免注册 COM 互操作的支持根据库程序集的类型而略有差异；具体而言，因该程序集是非托管的（COM 并行）还是托管的（基于 NET）而异。 下表显示每个程序集类型对操作系统和 .NET Framework 版本的要求。  
  
    |程序集类型|操作系统|.NET Framework 版本|  
    |-------------------|----------------------|----------------------------|  
    |COM 并行|Microsoft Windows XP|不要求。|  
    |基于 .NET|带有 SP2 的 Windows XP|NET Framework 1.1 或更高版本。|  
  
     Windows Server 2003 系列还支持基于 .NET 的程序集采用免注册 COM 互操作。  
  
     要使基于 .NET 的类与 COM 的免注册激活兼容，类必须具有无参数构造函数，并且必须是公共类。  
  
### <a name="configuring-com-components-for-registration-free-activation"></a>将 COM 组件配置为免注册激活  
  
1. 要使 COM 组件参与免注册激活，必需将其作为并行程序集进行部署。 并行程序集是非托管程序集。  有关详细信息，请参阅[使用并行程序集](/windows/desktop/SbsCs/using-side-by-side-assemblies)。  
  
     要使用 COM 并行程序集，基于 .NET 的应用程序的开发人员必须提供一个包含绑定和激活信息的应用程序清单。 Windows XP 操作系统内置对非托管并行程序集的支持。 当要激活的组件不在注册表中时，操作系统支持的 COM 运行时将扫描应用程序清单以查找激活信息。  
  
     Windows XP 上安装的 COM 组件可以选择进行免注册激活。 有关向应用程序添加并行程序集的详细说明，请参阅[使用并行程序集](/windows/desktop/SbsCs/using-side-by-side-assemblies)。  
  
    > [!NOTE]
    > 并行执行是一项 .NET Framework 功能，它使得多个版本的运行时，以及使用同一个运行时版本的多个版本的应用程序和组件，能够在同一台计算机上同时运行。 并行执行和并行程序集是提供并行功能的两种不同机制。  
  
## <a name="see-also"></a>请参阅

- [如何：配置基于 .NET Framework 的 COM 组件以进行免注册激活](configure-net-framework-based-com-components-for-reg.md)
