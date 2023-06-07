---
title: 调用 DLL 函数
description: 查看有关调用可能令人感到迷惑的 DLL 函数的问题。 函数调用过程有所不同，具体取决于返回类型是否为 blittable。
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged functions, calling
- unmanaged functions
- COM interop, platform invoke
- platform invoke, calling unmanaged functions
- interoperation with unmanaged code, platform invoke
- DLL functions
ms.assetid: 113646de-7ea0-4f0e-8df0-c46dab3e8733
ms.openlocfilehash: 09dd9d30c29660231feee6c624a025ade1fda1d3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96282946"
---
# <a name="calling-a-dll-function"></a>调用 DLL 函数

尽管调用非托管 DLL 函数与调用其他托管代码几乎完全相同，但有一些差异会使 DLL 函数一开始令人感到迷惑。 本部分介绍的主题描述了与一些与调用相关的异常问题。  
  
 从平台调用返回的结构必须是在托管代码和非托管代码中表示形式相同的数据类型。 这些类型称为 blittable 类型，因为它们不需要转换（请参阅 [Blittable 类型和非 Blittable 类型](blittable-and-non-blittable-types.md)）。 若要调用返回类型为 non-blittable 结构的函数，可定义与 non-blittable 类型大小相同的 blittable 帮助程序类型，并在函数返回后转换数据。  
  
## <a name="in-this-section"></a>本节内容  

 [传递结构](passing-structures.md)  
 确定使用预定义布局传递数据结构的问题。  
  
 [回调函数](callback-functions.md)  
 提供有关回调函数的基本信息。  
  
 [如何：实现回调函数](how-to-implement-callback-functions.md)  
 描述如何在托管代码中实现回调函数。  
  
## <a name="related-sections"></a>相关章节  

 [使用非托管 DLL 函数](consuming-unmanaged-dll-functions.md)  
 描述如何使用平台调用调用非托管 DLL 函数。  
  
 [用平台调用封送数据](marshaling-data-with-platform-invoke.md)  
 描述如何声明方法形参以及如何将实参传递给由非托管库导出的函数。
