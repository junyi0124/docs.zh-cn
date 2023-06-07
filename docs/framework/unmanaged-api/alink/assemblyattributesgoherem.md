---
title: Assemblyattributesgohere 类（System.runtime.compilerservices）
ms.date: 03/30/2017
api_name:
- System.Runtime.CompilerServices.AssemblyAttributesGoHereM
api_location:
- mscorlib.dll
api_type:
- Assembly
f1_keywords:
- AssemblyAttributesGoHereM
helpviewer_keywords:
- AssemblyAttributesGoHereM type
- Alink API, AssemblyAttributesGoHereM type
ms.assetid: caaa8ba9-b4bb-4dd6-934d-57e436b2f3e5
topic_type:
- apiref
ms.openlocfilehash: 15b9445aa3eabbd14541cfe5481bfb553c8c0347
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446628"
---
# <a name="assemblyattributesgoherem-class"></a>Assemblyattributesgohere 类

由 ALink 用作占位符以存储有关自定义特性的信息。

## <a name="syntax"></a>语法

```csharp
internal sealed class AssemblyAttributesGoHereM
```

## <a name="remarks"></a>备注

对此类型的引用可能被嵌入网络模块内，模块的源包含程序集自定义属性。 当从一个或多个包含对这些类型的引用的网络模块生成程序集清单时，ALink 将使用附加到这些引用的信息来发出实际的自定义属性。 因此，此类型永远不会被实例化，而且对它的引用仅用作生成过程的一部分，在最终的程序集中不具有任何用途。

对此类型的引用指示了不与安全性相关但有多用途的自定义属性。

这些类型在 .NET Framework 中标记为 "internal"，位于 <xref:System.Runtime.CompilerServices> 命名空间中。

## <a name="requirements"></a>要求

mscorlib.dll

## <a name="see-also"></a>另请参阅

- [AssemblyAttributesGoHere](assemblyattributesgohere.md)
- [AssemblyAttributesGoHereS](assemblyattributesgoheres.md)
- [AssemblyAttributesGoHereSM](assemblyattributesgoheresm.md)
