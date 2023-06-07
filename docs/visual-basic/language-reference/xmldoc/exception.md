---
title: <exception>
ms.date: 07/20/2015
helpviewer_keywords:
- <exception> XML tag
- exception XML tag
ms.assetid: c0517549-171e-4dae-ab88-a9c1700b6eee
ms.openlocfilehash: e3f386d2a1e15ea3e1519d6e1e5e31c34c73fb99
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90872897"
---
# <a name="exception-visual-basic"></a>\<exception> (Visual Basic)

指定可以引发的异常。  
  
## <a name="syntax"></a>语法  
  
```xml  
<exception cref="member">description</exception>  
```  
  
## <a name="parameters"></a>参数  

 `member`  
 对当前编译环境中出现的一个异常的引用。 编译器检查是否存在给定的异常，并将 `member` 转换为输出 XML 中的规范的元素名称。 `member` 必须出现在双引号 (" ") 内。  
  
 `description`  
 说明。  
  
## <a name="remarks"></a>备注  

 使用 `<exception>` 标记指定可以引发的异常。 这是适用于方法定义的标记。  
  
 使用 [-doc](../../reference/command-line-compiler/doc.md) 进行编译以便将文档注释处理到文件中。  
  
## <a name="example"></a>示例  

 此示例使用 `<exception>` 标记来描述 `IntDivide` 函数可以引发的异常。  
  
 [!code-vb[VbVbcnXmlDocComments#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>另请参阅

- [XML 注释标记](index.md)
