---
title: 公共语言运行时中的类型转发
description: 使用类型转发可以将类型移到另一个 .NET 程序集，而不必重新编译使用原始程序集的应用程序。
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET], type forwarding
- type forwarding
ms.assetid: 51f8ffa3-c253-4201-a3d3-c4fad85ae097
dev_langs:
- csharp
- cpp
ms.openlocfilehash: cd166068993fb5d1a5164615de3926a06dda8098
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687670"
---
# <a name="type-forwarding-in-the-common-language-runtime"></a>公共语言运行时中的类型转发

使用类型转发可以将类型移到另一个程序集，而不必重新编译使用原始程序集的应用程序。  
  
 例如，假设应用程序使用名为 Utility.dll 的程序集中的 `Example` 类。 Utility.dll 的开发人员可能决定重构该程序集，并且在重构过程中可能将 `Example` 类移到另一个程序集。 如果旧版本的 Utility.dll 由新版本的 Utility.dll 及其配套程序集取代，则使用 `Example` 类的应用程序会失败，因其无法在新版本的 Utility.dll 中找到 `Example` 类  。  
  
 Utility.dll 开发人员可使用 <xref:System.Runtime.CompilerServices.TypeForwardedToAttribute> 属性转发 `Example` 类的请求，从而避免这种情况。 如果已向新版本的 Utility.dll 应用了该属性，则对 `Example` 类的请求会转发到该类目前所属的程序集。 现有应用程序将继续正常运行，无需重新编译。

## <a name="forward-a-type"></a>转发类型

 转发类型有四个步骤：  
  
1. 将类型的源代码从原始程序集移到目标程序集。  

2. 在类型曾存于的程序集中，为已移动的类型添加 <xref:System.Runtime.CompilerServices.TypeForwardedToAttribute>。 下面的代码显示名为 `Example` 的被移动类型的属性。  

   ```cpp  
    [assembly:TypeForwardedToAttribute(Example::typeid)]  
   ```

   ```csharp  
    [assembly:TypeForwardedToAttribute(typeof(Example))]  
   ```  

3. 编译该类型目前所属的程序集。  

4. 重新编译该类型原来所属的程序集，其中带有对该类型目前所属的程序集的引用。 例如，如果从命令行编译一个 C# 文件，则使用 [-referenc（C# 编译器选项）](../../csharp/language-reference/compiler-options/reference-compiler-option.md)选项指定包含类型的程序集。 在 C++ 中，在源文件中使用 [#using](/cpp/preprocessor/hash-using-directive-cpp) 指令指定包含类型的程序集。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.CompilerServices.TypeForwardedToAttribute>
- [类型转发 (C++/CLI)](/cpp/windows/type-forwarding-cpp-cli)
- [#using 指令](/cpp/preprocessor/hash-using-directive-cpp)
