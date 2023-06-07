---
title: 限制
ms.date: 07/20/2015
helpviewer_keywords:
- limits
- limitations [Visual Basic]
- limitations
- limits, Visual Basic code
- Visual Basic code, limitations
ms.assetid: cf1646b7-5d24-48c6-9616-bda8a4849d91
ms.openlocfilehash: abe4acd5850aa6065bf4f6fd41bc610ede7ad13f
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097951"
---
# <a name="visual-basic-limitations"></a>Visual Basic 限制

的早期版本 Visual Basic 在代码中强制实施边界，如变量名称长度、模块中允许的变量数和模块大小。 在 Visual Basic .NET 中，这些限制已被宽松，使你可以更自由地编写和排列代码。  
  
 物理限制在运行时内存上依赖于编译时注意事项。 如果使用明智的编程做法，并将大型应用程序划分为多个类和模块，则可能会遇到内部 Visual Basic 限制。  
  
 下面是在极端情况下可能遇到的一些限制：  
  
- **名称长度。** 每个声明的编程元素的名称中的字符数最多为个。 如果元素名称是限定的，则此最大值适用于整个限定字符串。 请参阅 [Declared Element Names](../language-features/declared-elements/declared-element-names.md)。  
  
- **行长度。** 源代码的物理行中最多包含65535个字符。 如果使用行继续符，则逻辑源代码行可能会更长。 请参阅 [如何：在代码中中断和组合语句](how-to-break-and-combine-statements-in-code.md)。  
  
- **数组维度。** 可以为数组声明的维数有最大值。 这限制了可用于指定数组元素的索引数。 请参阅 [Visual Basic 中的数组维度](../language-features/arrays/array-dimensions.md)。  
  
- **字符串长度。** 在单个字符串中，可以存储的 Unicode 字符数最多为个。 请参阅 [String 数据类型](../../language-reference/data-types/string-data-type.md)。  
  
- **环境字符串长度。** 对于用作命令行参数的任何环境字符串，最多可包含32768个字符。 这是对所有平台的限制。  
  
## <a name="see-also"></a>请参阅

- [程序结构和代码约定](program-structure-and-code-conventions.md)
- [Visual Basic 命名约定](naming-conventions.md)
