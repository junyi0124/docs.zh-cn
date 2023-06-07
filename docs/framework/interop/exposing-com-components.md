---
title: 向 .NET Framework 公开 COM 组件
description: 了解向 .NET 公开 COM 组件的过程。 作为中间层业务应用程序或独立功能，COM 组件在托管代码中非常宝贵。
ms.date: 03/30/2017
helpviewer_keywords:
- exposing COM components to .NET Framework
- interoperation with unmanaged code, exposing COM components
- COM interop, exposing COM components
ms.assetid: e78b14f1-e487-43cd-9c6d-1a07483f1730
ms.openlocfilehash: 4e18e3f49dacbb3a358aa7e9785a5dda93206164
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267060"
---
# <a name="exposing-com-components-to-the-net-framework"></a>向 .NET Framework 公开 COM 组件

本部分概述向托管代码公开现有 COM 组件所需的步骤。 有关编写与 .NET Framework 紧密集成的 COM 服务器的详细信息，请参阅[互操作的设计注意事项](/previous-versions/dotnet/netframework-4.0/61aax4kh(v=vs.100))。
  
 作为中间层业务应用程序或独立功能，现有 COM 组件是托管代码中的宝贵资源。 理想的组件具有主互操作程序集，且完全符合 COM 实施的编程标准。  
  
#### <a name="to-expose-com-components-to-the-net-framework"></a>向 .NET Framework 公开 COM 组件  
  
1. [将类型库当作程序集导入](importing-a-type-library-as-an-assembly.md)。  
  
     公共语言运行时需要包括 COM 类型在内的所有类型的元数据。 有多种方法来获取包含 COM 类型（作为元数据导入）的程序集。  
  
2. [在托管代码中使用 COM 类型](/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100)).  
  
     可检查 COM 类型、激活实例，并采用在任何托管类型上进行调用的方式在 COM 对象上调用方法。  
  
3. [编译互操作项目](compiling-an-interop-project.md)。  
  
     Windows SDK 为符合公共语言规范 (CLS) 的多种语言提供编译器，其中包括 Visual Basic、C# 和 C++。  
  
4. [部署互操作应用程序](deploying-an-interop-application.md)。  
  
     最好将互操作应用程序部署为全局程序集缓存中具有[强名称](../../standard/assembly/strong-named.md)的签名程序集。  
  
## <a name="see-also"></a>请参阅

- [与非托管代码交互操作](index.md)
- [互操作的设计注意事项](/previous-versions/dotnet/netframework-4.0/61aax4kh(v=vs.100))
- [COM 互操作示例：.NET 客户端和 COM 服务器](com-interop-sample-net-client-and-com-server.md)
- [语言独立性和与语言无关的组件](../../standard/language-independence-and-language-independent-components.md)
- [Gacutil.exe（全局程序集缓存工具）](../tools/gacutil-exe-gac-tool.md)
