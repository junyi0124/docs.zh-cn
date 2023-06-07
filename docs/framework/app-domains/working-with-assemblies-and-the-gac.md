---
title: 使用程序集和全局程序集缓存
description: 使用 .NET 中的程序集和全局程序集缓存 (GAC)。 查看建议在 GAC 中安装程序集的原因。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- global assembly cache, benefits
- ACLs [.NET Framework]
- GAC (global assembly cache), benefits
- access control lists [.NET Framework]
ms.assetid: 8a18e5c2-d41d-49ef-abcb-7c27e2469433
ms.openlocfilehash: e27fdd7def2d234e1e8eb7557e869bf478d68210
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279306"
---
# <a name="working-with-assemblies-and-the-global-assembly-cache"></a>使用程序集和全局程序集缓存

如果需要在几个应用程序间共享程序集，可将其安装到全局程序集缓存中。 安装了公共语言运行时的每台计算机均具有此计算机范围的代码缓存。 全局程序集缓存中存储专门指定给由计算机中若干应用程序共享的程序集。 程序集必须具有强名称才可安装到全局程序集缓存中。  
  
> [!NOTE]
> 全局程序集缓存中放置的程序集必须具有相同的程序集名称和文件名（不包括文件扩展名）。 例如，程序集名称为 myAssembly 的程序集必须具有名为 myAssembly.exe 或 myAssembly.dll 的文件。  
  
只能在需要时才通过将程序集安装到全局程序集缓存中来共享程序集。 一般原则是：程序集依赖项保持专用，并在应用程序目录中定位程序集，除非明确要求共享程序集。 另外，无需为了使程序集可以访问 COM 互操作或非托管代码而将程序集安装到全局程序集缓存。  
  
建议将程序集安装到全局程序集缓存中的原因有以下几点：  
  
- 共享位置。  
  
     应该由应用程序使用的程序集可以置于全局程序集缓存。 例如，如果所有应用程序都应使用位于全局程序集缓存中的程序集，则可将版本策略语句添加到 Machine.config 文件中，此文件将引用重新定向到程序集。  
  
- 文件安全。  
  
     管理员通常使用访问控制列表 (ACL) 来保护 systemroot 目录，从而控制写入和执行访问。 由于全局程序集缓存安装在 systemroot 目录中，因此它将继承该目录的 ACL。 建议只允许具有“管理员”权限的用户从全局程序集缓存中删除文件。  
  
- 并行版本。  
  
     可在全局程序集缓存中维护名称相同但版本信息不同的程序集的多个副本。  
  
- 其他搜索位置。  
  
     在探测或使用配置文件中的基本代码信息之前，公共语言运行时会先检查全局程序集缓存中符合程序集请求的程序集。  
  
 请注意，在有些情况下，很明显不需要将程序集安装到全局程序集缓存中。 如果将组成应用程序的某个程序集置于全局程序集缓存中，就无法再通过使用 XCOPY 复制应用程序目录来复制或安装应用程序。 在这种情况下，还必须将程序集移到全局程序集缓存中。  
  
## <a name="in-this-section"></a>本节内容  

[如何：将程序集安装到全局程序集缓存](install-assembly-into-gac.md)  
描述将程序集安装到全局程序集缓存的方法。  
  
[如何：查看全局程序集缓存的内容](how-to-view-the-contents-of-the-gac.md)  
说明如何使用 [Gacutil.exe（全局程序集缓存工具）](../tools/gacutil-exe-gac-tool.md)来查看全局程序集缓存的内容。  
  
[如何：从全局程序集缓存中删除程序集](how-to-remove-an-assembly-from-the-gac.md)  
说明如何使用 [Gacutil.exe（全局程序集缓存工具）](../tools/gacutil-exe-gac-tool.md)从全局程序集缓存中删除程序集。  
  
[将服务组件和全局程序集缓存结合使用](use-serviced-components-with-the-gac.md)  
说明服务组件（托管 COM+ 组件）应置于全局程序集缓存中的原因。  
  
## <a name="related-sections"></a>相关章节  

[创建程序集](../../standard/assembly/create.md)  
概述创建程序集。  
  
[全局程序集缓存](gac.md)  
描述全局程序集缓存。  
  
[如何：查看程序集内容](../../standard/assembly/view-contents.md)  
说明如何使用 [Ildasm.exe（IL 反汇编程序）](../tools/ildasm-exe-il-disassembler.md)来查看程序集中的 Microsoft 中间语言 (MSIL) 信息。  
  
[运行时如何定位程序集](../deployment/how-the-runtime-locates-assemblies.md)  
描述公共语言运行时如何查找并加载构成应用程序的程序集。  
  
[使用程序集编程](../../standard/assembly/index.md)  
描述托管应用程序的构造块 - 程序集。
