---
title: ParamArray
ms.date: 07/20/2015
f1_keywords:
- vb.ParamArray
- ParamArray
helpviewer_keywords:
- ParamArray keyword [Visual Basic]
- ParamArray keyword [Visual Basic], syntax
ms.assetid: a5f18789-92bd-488f-9c7e-cf3719963635
ms.openlocfilehash: baf9303101eea9eed27e8a4eee9e04d255c798e9
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867830"
---
# <a name="paramarray-visual-basic"></a>ParamArray (Visual Basic)

指定过程参数采用指定类型的可选元素数组。 `ParamArray` 只能在参数列表的最后一个参数上使用。  
  
## <a name="remarks"></a>备注  

 `ParamArray` 允许将任意数量的参数传递给过程。 `ParamArray`始终使用[ByVal](byval.md)声明参数。  
  
 可以通过以下方式向参数提供一个或多个参数 `ParamArray` ：传递适当数据类型的数组、以逗号分隔的值列表，或根本不提供任何参数。 有关详细信息，请参阅 [参数数组](../../programming-guide/language-features/procedures/parameter-arrays.md)中的 "调用 ParamArray"。  
  
> [!IMPORTANT]
> 无论何时处理可能会无限大的阵列，都有 overrunning 应用程序的一些内部容量的风险。 如果接受来自调用代码的参数数组，则应测试其长度，如果应用程序太大，则应采取适当的措施。  
  
 `ParamArray` 修饰符可用于下面的上下文中：  
  
 [Declare Statement](../statements/declare-statement.md)  
  
 [Function 语句](../statements/function-statement.md)  
  
 [Property Statement](../statements/property-statement.md)  
  
 [Sub 语句](../statements/sub-statement.md)  
  
## <a name="see-also"></a>另请参阅

- [关键字](../keywords/index.md)
- [参数数组](../../programming-guide/language-features/procedures/parameter-arrays.md)
