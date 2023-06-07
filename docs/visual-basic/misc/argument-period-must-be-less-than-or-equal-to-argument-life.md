---
title: 自变量“Period”必须小于或等于自变量“Life”
ms.date: 07/20/2015
f1_keywords:
- vbrFinancial_PeriodLELife
ms.assetid: dc575d41-b376-4b05-bbbe-6de1e98385f1
ms.openlocfilehash: 844192f1168e6fef7906ffc133dcc3b5ba40b42c
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91087032"
---
# <a name="argument-period-must-be-less-than-or-equal-to-argument-life"></a>自变量“Period”必须小于或等于自变量“Life”

`Period` 参数的值（指定计算资产折旧的时期）大于 `Life` 参数的值。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
- 确保 `Life` 和 `Period` 参数以相同的单位表示。 例如，如果 `Life` 以月为单位，则 `Period` 也应以月为单位。  
  
## <a name="see-also"></a>请参阅

- [按值和按引用传递参数](../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)
