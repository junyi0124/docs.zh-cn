---
title: <remarks>
ms.date: 07/20/2015
helpviewer_keywords:
- <remarks> XML tag
- remarks XML tag
ms.assetid: c6241773-a7ed-41c9-9a8b-9722a0c606a9
ms.openlocfilehash: 70078752495240ab8c72fe1bbecdca554166fb22
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866430"
---
# <a name="remarks-visual-basic"></a>\<remarks> (Visual Basic)

为成员指定备注部分。  
  
## <a name="syntax"></a>语法  
  
```xml  
<remarks>description</remarks>  
```  
  
## <a name="parameters"></a>参数  

 `description`  
 对成员的说明。  
  
## <a name="remarks"></a>备注  

 使用 `<remarks>` 标记添加有关类型的信息，从而补充使用指定的信息 [\<summary>](summary.md) 。  
  
 此信息显示在对象浏览器中。 有关对象浏览器的信息，请参阅 [查看代码的结构](/visualstudio/ide/viewing-the-structure-of-code)。  
  
 使用 [-doc](../../reference/command-line-compiler/doc.md) 进行编译以便将文档注释处理到文件中。  
  
## <a name="example"></a>示例  

 此示例使用 `<remarks>` 标记解释该方法的作用 `UpdateRecord` 。  
  
 [!code-vb[VbVbcnXmlDocComments#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>另请参阅

- [XML 注释标记](index.md)
