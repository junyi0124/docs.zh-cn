---
title: XML 注释异常必须具有“cref”特性
ms.date: 07/20/2015
f1_keywords:
- bc42319
- vbc42319
helpviewer_keywords:
- BC42319
ms.assetid: 62eeeba3-6811-48be-b1ef-c2e4feda3177
ms.openlocfilehash: 18e7aa5f6905eaa9c509aa21fe6f5bfcd54d46f0
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163293"
---
# <a name="bc42319-xml-comment-exception-must-have-a-cref-attribute"></a>BC42319： XML 注释异常必须具有 "cref" 特性

\<exception>标记提供了一种方法来记录可由方法引发的异常。 必需的 `cref` 属性指定由文档生成器检查的成员的名称。 如果该成员存在，则将其转换为文档文件中的规范元素名称。

**错误 ID：** BC42319

## <a name="to-correct-this-error"></a>更正此错误

将 `cref` 属性添加到异常，如下所示：

```xml
<exception cref="member">description</exception>
```

## <a name="see-also"></a>另请参阅

- [\<exception>](../xmldoc/exception.md)
- [如何：创建 XML 文档](../../programming-guide/program-structure/how-to-create-xml-documentation.md)
- [XML 注释标记](../xmldoc/index.md)
