---
title: <example>
ms.date: 07/20/2015
helpviewer_keywords:
- example XML tag
- <example> XML tag
ms.assetid: 90eeda1c-3fc4-427c-879c-5046d265a97c
ms.openlocfilehash: 6e9f63e4d31df7790f51ae4d166b606f2c63f14b
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90872975"
---
# <a name="example-visual-basic"></a>\<example> (Visual Basic)

指定成员的示例。  
  
## <a name="syntax"></a>语法  
  
```xml  
<example>description</example>  
```  
  
## <a name="parameters"></a>参数  

 `description`  
 代码示例的说明。  
  
## <a name="remarks"></a>备注  

 借助 `<example>` 标记，可以指定如何使用方法或其他库成员的示例。 这通常涉及到使用 [\<code>](code.md) 标记。  
  
 使用 [-doc](../../reference/command-line-compiler/doc.md) 进行编译以便将文档注释处理到文件中。  
  
## <a name="example"></a>示例  

 此示例使用 `<example>` 标记来包含使用字段的示例 `ID` 。  
  
 [!code-vb[VbVbcnXmlDocComments#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#2)]  
  
## <a name="see-also"></a>另请参阅

- [XML 注释标记](index.md)
