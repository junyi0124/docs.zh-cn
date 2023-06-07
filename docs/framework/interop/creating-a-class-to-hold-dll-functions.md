---
title: 创建用于容纳 DLL 函数的类
description: 在 .NET 中创建用于容纳 DLL 函数的托管类包装器，这有助于封装平台功能。
ms.date: 03/30/2017
helpviewer_keywords:
- COM interop, DLL functions
- unmanaged functions
- COM interop, platform invoke
- interoperation with unmanaged code, DLL functions
- interoperation with unmanaged code, platform invoke
- platform invoke, creating class for functions
- DLL functions
ms.assetid: e08e4c34-0223-45f7-aa55-a3d8dd979b0f
ms.openlocfilehash: c255efe4579635389ac62956cac9d1405bfb184c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96271429"
---
# <a name="creating-a-class-to-hold-dll-functions"></a>创建用于容纳 DLL 函数的类

将常用的 DLL 函数包装在托管类中，这是封装平台功能的一种有效方式。 虽然不必在每种情形下都这样做，但由于定义 DLL 函数相当麻烦且容易出错，所以提供类包装器非常简便。 如果使用 Visual Basic 或 C# 进行编程，必须在一个类或 Visual Basic 模块中声明 DLL 函数。  
  
 在一个类中，为每个要调用的 DLL 函数定义静态方法。 定义中可以包括附加信息，例如传递方法参数使用的字符集或调用约定；如果省略这些信息，则选择默认设置。 有关声明选项及其默认设置的完整列表，请参阅[在托管代码中创建原型](creating-prototypes-in-managed-code.md)。  
  
 包装之后，就可按照调用任何其他类上的静态方法的方式来调用该类上的方法。 平台调用自动处理基础导出函数。  
  
 为平台调用设计托管类时，应考虑类和 DLL 函数之间的关系。 例如，你可以：  
  
- 在现有类内声明 DLL 函数。  
  
- 分别为每个 DLL 函数创建一个类，使函数相互独立，易于查找。  
  
- 为一组相关 DLL 函数创建一个类，形成逻辑分组并减少开销。  
  
 可对该类及其方法随意命名。 有关演示如何构造要用于平台调用、基于 .NET 的声明的示例，请参阅[用平台调用封送数据](marshaling-data-with-platform-invoke.md)。  
  
## <a name="see-also"></a>请参阅

- [使用非托管 DLL 函数](consuming-unmanaged-dll-functions.md)
- [标识 DLL 中的函数](identifying-functions-in-dlls.md)
- [在托管代码中创建原型](creating-prototypes-in-managed-code.md)
- [调用 DLL 函数](calling-a-dll-function.md)
