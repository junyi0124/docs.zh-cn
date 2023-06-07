---
title: 自动错误
ms.date: 07/20/2015
f1_keywords:
- vbrID440
ms.assetid: 2c4be5c5-2f0d-4a2b-96fe-d1b24f08fc4c
ms.openlocfilehash: d62ba57db8bffefb2cfebed705251d87fe285602
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409889"
---
# <a name="automation-error"></a>自动错误

执行方法或者获取或设置对象变量的属性时发生错误。 错误由创建该对象的应用程序报告。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 检查 `Err` 对象的属性，以确定错误来源和错误性质。  
  
2. 在紧邻访问语句之前使用 `On Error Resume Next` 语句，然后检查紧邻访问语句之后是否存在错误。  
  
## <a name="see-also"></a>另请参阅

- [错误类型](../../programming-guide/language-features/error-types.md)
- [与我们交流](/visualstudio/ide/feedback-options)
