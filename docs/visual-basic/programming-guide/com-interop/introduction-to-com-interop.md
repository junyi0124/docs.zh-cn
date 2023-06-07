---
title: COM 互操作介绍
ms.date: 07/20/2015
helpviewer_keywords:
- interop assemblies
- COM interop [Visual Basic], about COM interop
ms.assetid: 8bd62e68-383d-407f-998b-29aa0ce0fd67
ms.openlocfilehash: 7bfbf0c6de8519e91a458ab4cbb5693024cdbeb2
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91090386"
---
# <a name="introduction-to-com-interop-visual-basic"></a>COM 互操作介绍 (Visual Basic)

组件对象模型 (COM) 使对象向其他组件和宿主应用程序公开其功能。 虽然 COM 对象多年来一直是 Windows 编程的基础，但为公共语言运行时设计的应用程序 (CLR) 提供多种优势。  
  
 .NET Framework 应用程序最终将取代通过 COM 开发的应用程序。 在此之前，你可能必须使用 Visual Studio 来使用或创建 COM 对象。 与 COM 或 *com 互操作*的互操作性使你能够在按自己的节奏转换到 .NET Framework 时使用现有的 com 对象。  
  
 通过使用 .NET Framework 创建 COM 组件，你可以使用无注册 COM 互操作。 这样，你就可以在计算机上安装了多个版本时控制启用了哪个 DLL 版本，并允许最终用户使用 XCOPY 或 FTP 将你的应用程序复制到其计算机上可以运行它的相应目录。 有关详细信息，请参阅 [免注册 COM 互操作](../../../framework/interop/registration-free-com-interop.md)。  
  
## <a name="managed-code-and-data"></a>托管代码和数据  

 为 .NET Framework 开发的代码称为 *托管代码*，并包含 CLR 使用的元数据。 .NET Framework 应用程序使用的数据称为 *托管数据* ，因为运行时管理与数据相关的任务，例如分配和回收内存以及执行类型检查。 默认情况下，Visual Basic .NET 使用托管代码和数据，但你可以使用互操作程序集访问 COM 对象的非托管代码和数据， (稍后) 此页中所述。  
  
## <a name="assemblies"></a>程序集  

 程序集是 .NET Framework 应用程序的主要构造块。 它是一个功能集合，它作为包含一个或多个文件的单个实现单元进行生成、版本控制和部署。 每个程序集都包含一个程序集清单。  
  
## <a name="type-libraries-and-assembly-manifests"></a>类型库和程序集清单  

 类型库描述 COM 对象的特征，如成员名称和数据类型。 程序集清单对 .NET Framework 应用程序执行相同的功能。 它们包括有关以下内容的信息：  
  
- 程序集标识、版本、区域性和数字签名。  
  
- 组成程序集实现的文件。  
  
- 构成程序集的类型和资源。 这包括从它中导出的。  
  
- 对其他程序集的编译时依赖关系。  
  
- 程序集正常运行所需的权限。  
  
 有关程序集和程序集清单的详细信息，请参阅 [.net 中的程序集](../../../standard/assembly/index.md)。  
  
### <a name="importing-and-exporting-type-libraries"></a>导入和导出类型库  

 Visual Studio 包含一个实用工具 Tlbimp，使你可以将类型库中的信息导入到 .NET Framework 的应用程序中。 可以通过使用 Tlbexp.exe 实用工具从程序集生成类型库。  
  
 有关 Tlbimp 和 Tlbexp.exe 的信息，请参阅 [Tlbimp.exe (类型库导入程序) ](../../../framework/tools/tlbimp-exe-type-library-importer.md) 和 [Tlbexp.exe (类型库导出程序) ](../../../framework/tools/tlbexp-exe-type-library-exporter.md)。  
  
## <a name="interop-assemblies"></a>互操作程序集  

 互操作程序集是在托管代码和非托管代码之间桥接的 .NET Framework 程序集，将 COM 对象成员映射到等效的 .NET Framework 托管成员。 Visual Basic .NET 创建的互操作程序集用于处理使用 COM 对象（如互操作性封送处理）的许多详细信息。  
  
## <a name="interoperability-marshaling"></a>互操作性封送  

 所有 .NET Framework 应用程序共享一组公共类型，这些类型可以实现对象的互操作性，而不考虑所使用的编程语言。 COM 对象的参数和返回值有时使用的数据类型与托管代码中使用的数据类型不同。 *互操作封送处理* 是一种将参数和返回值与 COM 对象来回移动的等效数据类型的过程。 有关详细信息，请参阅 [互操作封送处理](../../../framework/interop/interop-marshaling.md)。  
  
## <a name="see-also"></a>请参阅

- [COM 互操作](index.md)
- [演练：使用 COM 对象实现继承](walkthrough-implementing-inheritance-with-com-objects.md)
- [与非托管代码交互操作](../../../framework/interop/index.md)
- [互操作性疑难解答](troubleshooting-interoperability.md)
- [.NET 中的程序集](../../../standard/assembly/index.md)
- [Tlbimp.exe（类型库导入程序）](../../../framework/tools/tlbimp-exe-type-library-importer.md)
- [Tlbexp.exe（类型库导出程序）](../../../framework/tools/tlbexp-exe-type-library-exporter.md)
- [互操作封送处理](../../../framework/interop/interop-marshaling.md)
- [免注册 COM 互操作](../../../framework/interop/registration-free-com-interop.md)
