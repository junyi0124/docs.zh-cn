---
title: <paramref>
ms.date: 07/20/2015
helpviewer_keywords:
- paramref XML tag
- <paramref> XML tag
ms.assetid: 8979d53b-beb1-41b7-b41e-6bbea1c17a03
ms.openlocfilehash: d7aacc5faea22f9c5a4b865a32c832154fe624c5
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90872618"
---
# <a name="paramref-visual-basic"></a>\<paramref> (Visual Basic)

将单词设置为参数格式。  
  
## <a name="syntax"></a>语法  
  
```xml  
<paramref name="name"/>  
```  
  
## <a name="parameters"></a>参数  

 `name`  
 要引用的参数的名称。 用双引号 (" ") 将名称引起来。  
  
## <a name="remarks"></a>备注  

 `<paramref>`标记提供了一种方法，用于指示某个字是参数。 可以处理 XML 文件，以便以某种不同的方式格式化此参数。  
  
 使用 [-doc](../../reference/command-line-compiler/doc.md) 进行编译以便将文档注释处理到文件中。  
  
## <a name="example"></a>示例  

 此示例使用 `<paramref>` 标记来引用 `id` 参数。  
  
 [!code-vb[VbVbcnXmlDocComments#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>另请参阅

- [XML 注释标记](index.md)
