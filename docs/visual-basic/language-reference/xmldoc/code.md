---
title: <code>
ms.date: 07/20/2015
helpviewer_keywords:
- code XML tag
- <code> XML tag
ms.assetid: 925e5342-be05-45f2-bf66-7398bbd6710e
ms.openlocfilehash: d058663213cf02f2142bff740aeec1b60791362c
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873034"
---
# <a name="code-visual-basic"></a>\<code> (Visual Basic)

指示文本为多行代码。  
  
## <a name="syntax"></a>语法  
  
```xml  
<code>content</code>  
```  
  
## <a name="parameters"></a>参数  

 `content`  
 要标记为代码的文本。  
  
## <a name="remarks"></a>备注  

 使用 `<code>` 标记将多行指示为代码。 用于 [\<c>](c.md) 指示应将说明内的文本标记为代码。  
  
 使用 [-doc](../../reference/command-line-compiler/doc.md) 进行编译以便将文档注释处理到文件中。  
  
## <a name="example"></a>示例  

 此示例使用 \<code> 标记来包含用于使用字段的示例代码 `ID` 。  
  
 [!code-vb[VbVbcnXmlDocComments#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#2)]  
  
## <a name="see-also"></a>请参阅

- [XML 注释标记](index.md)
