---
title: 自变量“Life”不能为零
ms.date: 07/20/2015
f1_keywords:
- vbrFinancial_LifeNEZero
ms.assetid: c402da97-a2b2-4219-a83a-0cebbfdffef2
ms.openlocfilehash: 280571b9f9c799305efd53359e079d25d16ffd03
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91087201"
---
# <a name="argument-life-cannot-be-zero"></a>自变量“Life”不能为零

`Life`的参数（必须为指定资产的使用年限的长度的 `Double` ）无效。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 检查表达式中参数的拼写是否正确。 拼写错误的变量名可能会隐式创建一个初始化为零的数值变量。  
  
- 检查之前对表达式中的变量进行的操作，尤其是那些从其他过程作为参数传递给该过程的操作。  
  
## <a name="see-also"></a>请参阅

- [按值和按引用传递参数](../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)
