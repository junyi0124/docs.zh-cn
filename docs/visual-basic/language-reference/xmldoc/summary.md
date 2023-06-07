---
title: <summary>
ms.date: 07/20/2015
helpviewer_keywords:
- <summary> XML tag
- summary XML tag
ms.assetid: 861c847d-dd94-478a-aa23-bf4899cdc848
ms.openlocfilehash: 471d3ddb5cf5a234cb77de6d1d93309faf754359
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90865844"
---
# <a name="summary-visual-basic"></a>\<summary> (Visual Basic)

指定成员的摘要。  
  
## <a name="syntax"></a>语法  
  
```xml  
<summary>description</summary>  
```  
  
## <a name="parameters"></a>参数  

 `description`  
 对象的摘要。  
  
## <a name="remarks"></a>备注  

 使用 `<summary>` 标记来描述类型或类型成员。 使用 [\<remarks>](remarks.md) 可针对某个类型说明添加补充信息。  
  
 标记的文本 `<summary>` 是有关 IntelliSense 中类型的唯一信息源，也会显示在对象浏览器中。 有关对象浏览器的信息，请参阅 [查看代码的结构](/visualstudio/ide/viewing-the-structure-of-code)。  
  
 使用 [-doc](../../reference/command-line-compiler/doc.md) 进行编译以便将文档注释处理到文件中。  
  
## <a name="example"></a>示例  

 此示例使用 `<summary>` 标记来描述 `ResetCounter` 方法和 `Counter` 属性。  
  
 [!code-vb[VbVbcnXmlDocComments#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#1)]  
  
## <a name="see-also"></a>另请参阅

- [XML 注释标记](index.md)
