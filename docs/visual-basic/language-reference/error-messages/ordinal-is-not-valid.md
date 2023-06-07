---
title: 序号无效
ms.date: 07/20/2015
f1_keywords:
- vbrID452
ms.assetid: 7459562b-cd4f-4590-95e0-6126ae3589a5
ms.openlocfilehash: 64f56b2ecace0ceafb310a175ea605e6959b7256
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90871384"
---
# <a name="ordinal-is-not-valid"></a>序号无效

对动态链接库的调用 (DLL) 指示使用语法，而不是过程名称 `#num` 。 此错误具有以下可能的原因：  
  
- 尝试将 `#num` 表达式转换为序号失败。  
  
- 指定的不 `#num` 指定 DLL 中的任何函数。  
  
- 类型库具有无效的声明，导致内部使用的序号无效。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 请确保表达式表示有效数字，或按名称调用该过程。  
  
2. 请确保 `#num` 在 DLL 中标识有效函数。  
  
3. 通过注释掉代码，隔离导致问题的过程调用。 `Declare`为过程编写语句，并将问题报告给类型库供应商。  
  
## <a name="see-also"></a>另请参阅

- [Declare Statement](../statements/declare-statement.md)
