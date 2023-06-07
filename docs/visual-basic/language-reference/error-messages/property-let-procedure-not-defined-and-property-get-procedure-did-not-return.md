---
title: 未定义 Property Let 过程，并且 Property Get 过程未返回对象
ms.date: 07/20/2015
f1_keywords:
- vbrID451
ms.assetid: 8542382a-689f-4e1b-abc0-c1e2dadb92f4
ms.openlocfilehash: fbeaa224ea9e095f86c37e571492d83bc98b4397
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90871091"
---
# <a name="property-let-procedure-not-defined-and-property-get-procedure-did-not-return-an-object"></a>未定义 Property Let 过程，并且 Property Get 过程未返回对象

某些属性、方法和操作只能应用于 `Collection` 对象。 指定的操作或属性专用于集合，但该对象不是集合。  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
1. 检查对象或属性名称的拼写，或验证对象是否为 `Collection` 对象。  
  
2. 查看 `Add` 用于将对象添加到集合的方法，以确保语法正确，并且所有标识符拼写正确。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Collection>
