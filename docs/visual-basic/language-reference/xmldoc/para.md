---
title: <para>
ms.date: 07/20/2015
helpviewer_keywords:
- <para> XML tag
- para XML tag
ms.assetid: a3a18b6c-6416-4358-94ec-37b22675fd37
ms.openlocfilehash: 0846efbaf2afacd3d0783a2d2d8e4dae3fc8b715
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90872710"
---
# <a name="para-visual-basic"></a>\<para> (Visual Basic)

指定将内容的格式设置为段落。  
  
## <a name="syntax"></a>语法  
  
```xml  
<para>content</para>  
```  
  
## <a name="parameters"></a>参数  

 `content`  
 段落文本。  
  
## <a name="remarks"></a>备注  

 `<para>` 标记用于标记内，例如 [\<summary>](summary.md)、[\<remarks>](remarks.md) 或 [\<returns>](returns.md)，允许向文本添加结构。  
  
 使用 [-doc](../../reference/command-line-compiler/doc.md) 进行编译以便将文档注释处理到文件中。  
  
## <a name="example"></a>示例  

 此示例使用 `<para>` 标记将方法的 "备注" 部分拆分为 `UpdateRecord` 两个段落。  
  
 [!code-vb[VbVbcnXmlDocComments#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>另请参阅

- [XML 注释标记](index.md)
