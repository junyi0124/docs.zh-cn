---
title: <value>
ms.date: 07/20/2015
helpviewer_keywords:
- <value> XML tag
- value XML tag
ms.assetid: 0b84b02e-9e6d-41b5-a926-0d5dc76dacb5
ms.openlocfilehash: 550f8b63928f80878ba316bfaf09077e14091305
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875177"
---
# <a name="value-visual-basic"></a>\<value> (Visual Basic)

指定属性的说明。  
  
## <a name="syntax"></a>语法  
  
```xml  
<value>property-description</value>  
```  
  
## <a name="parameters"></a>参数  

 `property-description`  
 属性的说明。  
  
## <a name="remarks"></a>备注  

 使用 `<value>` 标记来描述属性。 请注意，当您使用 Visual Studio 开发环境中的代码向导添加属性时，它将 [\<summary>](summary.md) 为新属性添加标记。 然后，应手动添加 `<value>` 标记，以描述属性表示的值。  
  
 使用 [-doc](../../reference/command-line-compiler/doc.md) 进行编译以便将文档注释处理到文件中。  
  
## <a name="example"></a>示例  

 此示例使用 `<value>` 标记来描述属性包含的值 `Counter` 。  
  
 [!code-vb[VbVbcnXmlDocComments#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#1)]  
  
## <a name="see-also"></a>另请参阅

- [XML 注释标记](index.md)
