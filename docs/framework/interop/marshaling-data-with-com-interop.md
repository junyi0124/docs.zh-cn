---
title: 用 COM 互操作对数据进行封送处理
description: 请参阅涵盖用 COM 互操作对数据进行封送处理的文章。 Tlbimp.exe 和 Tlbexp.exe 工具在 COM 类型库和互操作程序集之间转换。
ms.date: 09/07/2017
helpviewer_keywords:
- COM interop, data marshaling
- marshaling data, COM interop
ms.openlocfilehash: bcbd2c50fcbd9af3f2eead57ac2e26f8db0c6ad6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275939"
---
# <a name="marshaling-data-with-com-interop"></a>用 COM 互操作对数据进行封送处理

COM 互操作同时对在托管代码中使用 COM 对象和向 COM 公开托管对象提供支持。 对于将数据封送到 COM 和从 COM 中封送数据的支持是广泛的，并几乎总是提供正确的封送行为。  
  
 Windows SDK 包括以下 COM 互操作工具：  
  
- [类型库导入程序 (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md)，它将 COM 类型库转换为互操作程序集。 从该程序集中，互操作封送处理服务生成包装器，该包装器在托管内存和非托管内存之间执行数据封送。  
  
- [类型库导出程序 (Tlbexp.exe)](../tools/tlbexp-exe-type-library-exporter.md)，它从程序集产生 COM 类型库，并生成在方法调用期间执行封送的包装器。  
  
 以下部分链接到特定主题，这些主题描述在能够（或必须）为封送拆收器提供附加类型信息时自定义互操作包装器的过程。  
  
## <a name="in-this-section"></a>本节内容  

[如何：手动创建包装器](how-to-create-wrappers-manually.md)描述如何在托管源代码中手动创建 COM 包装器。

 [如何：将托管代码 DCOM 迁移到 WCF](how-to-migrate-managed-code-dcom-to-wcf.md)  
 描述如何将托管 DCOM 代码迁移到 WCF，以得到最安全的解决方案。  
  
## <a name="related-sections"></a>相关章节  

 [COM 数据类型](/previous-versions/dotnet/netframework-4.0/sak564ww(v=vs.100))  
 提供相应的托管和非托管数据类型。  
  
 [自定义 COM 可调用包装器](/previous-versions/dotnet/netframework-4.0/3bwc828w(v=vs.100))  
 描述如何在设计时使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性显式封送数据类型。  
  
 [自定义运行时可调用包装器](/previous-versions/dotnet/netframework-4.0/e753eftz(v=vs.100))  
 描述如何调整互操作程序集中类型的封送行为以及如何以手动方式定义 COM 类型。  
  
 [高级 COM 互操作性](/previous-versions/dotnet/netframework-4.0/bd9cdfyx(v=vs.100))  
 提供一些链接，指向关于将 COM 组件并入 .NET Framework 应用程序中的详细信息。  
  
 [有关从程序集转换到类型库的摘要](/previous-versions/dotnet/netframework-4.0/xk1120c3(v=vs.100))  
 描述从程序集到类型库的导出转换过程。  
  
 [有关从类型库转换到程序集的摘要](/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))  
 描述从类型库到程序集的导入转换过程。  
  
 [使用泛型类型进行交互操作](/previous-versions/dotnet/netframework-4.0/ms229590(v=vs.100))  
 描述在使用用于 COM 互操作性的泛型类型时，哪些操作受支持。
