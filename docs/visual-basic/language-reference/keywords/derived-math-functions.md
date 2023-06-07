---
title: 派生的数学函数
ms.date: 07/20/2015
helpviewer_keywords:
- arithmetic operations, derived math functions
- cosecant function
- arcsecant function
- arccotangent function
- functions [Visual Basic], derived math functions
- inverse functions
- math functions, derived
- derived math functions
- cotangent function
- angles
- secant function
- trigonometric functions
- logarithms
- arccosecant function
- hyperbolic functions
- arcsine function
- degrees
- arccosine function
ms.assetid: 63e449d8-9444-44fb-8db1-6d9cf346e2aa
ms.openlocfilehash: f84b1ef18ef2a924bb2e47da85ecbb51f982873a
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90869027"
---
# <a name="derived-math-functions-visual-basic"></a>派生的数学函数 (Visual Basic)

下表显示了可以从对象的内部数学函数派生的非内部数学函数 <xref:System.Math?displayProperty=nameWithType> 。 您可以通过将添加 `Imports System.Math` 到您的文件或项目来访问内部数学函数。  
  
|函数|派生等效项|  
|--------------|-------------------------|  
|正割 (秒 (x) # A3|1/Cos (x) |  
|余割 (Csc (x) # A3|1/Sin (x) |  
|余切 (Ctan (x) # A3|1/Tan (x) |  
|反正弦 (Asin (x) # A3|Atan (x/Sqrt (-x * x + 1) # A3|  
|反余弦 (Acos (x) # A3|Atan (-x/Sqrt (-x * x + 1) # A3 + 2 \* Atan (1) |  
|反割 (Asec (x) # A3|2 * Atan (1) – Atan (号 (x) /Sqrt (x \* x – 1) # A7|  
|反向余割 (Acsc (x) # A3|Atan (Sign (x) /Sqrt (x * x – 1) # A5|  
|反余切 (Acot (x) # A3|2 * Atan (1) -Atan (x) |  
|双曲正弦 (Sinh (x) # A3| (Exp (x) – Exp (-x) # A5/2|  
|双曲余弦 (Cosh (x) # A3| (Exp (x) + Exp (-x) # A5/2|  
| (Tanh (x 的双曲正切值) # A3| (Exp (x) – Exp (-x) # A5/ (Exp (x) + Exp (-x) # A11|  
|双曲正则 (Sech (x) # A3|2/ (Exp (x) + Exp (-x) # A5|  
|双曲余割 (Csch (x) # A3|2/ (Exp (x) – Exp (-x) # A5|  
|双曲余切 (Coth (x) # A3| (Exp (x) + Exp (-x) # A5/ (Exp (x) – Exp (-x) # A11|  
|反双曲正弦 (Asinh (x) # A3|记录 (x + Sqrt (x * x + 1) # A3|  
|反双曲余弦 (Acosh (x) # A3|记录 (x + Sqrt (x * x – 1) # A3|  
|反双曲正切值 (Atanh (x) # A3|日志 ( # B1 1 + x) / (1 – x) # A5/2|  
|反双曲正割 (AsecH (x) # A3|日志 ( # B1 Sqrt (-x * x + 1) + 1) /x) |  
|反双曲余割 (Acsch (x) # A3|日志 ( # B1 Sign (x) * Sqrt (x \* x + 1) + 1) /x) |  
|反双曲余切 (Acoth (x) # A3|日志 ( # B1 x + 1) / (x – 1) # A5/2|  
  
## <a name="see-also"></a>另请参阅

- [数学函数](../functions/math-functions.md)
