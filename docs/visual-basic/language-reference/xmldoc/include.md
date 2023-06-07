---
title: <include>
ms.date: 07/20/2015
helpviewer_keywords:
- include XML tag
- <include> XML tag
ms.assetid: ba8e9173-82cd-460b-8938-a075a2dfb36d
ms.openlocfilehash: df8749ca9d6c92cf9ef95f03eea2704812ff495a
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90872872"
---
# <a name="include-visual-basic"></a>\<include> (Visual Basic)

引用另一个文件，该文件描述源代码中的类型和成员。  
  
## <a name="syntax"></a>语法  
  
```xml  
<include file="filename" path="tagpath[@name='id']" />  
```  
  
## <a name="parameters"></a>参数  

 `filename`  
 必需。 包含文档的文件的名称。 可使用路径来限定文件名。 用 `filename` 双引号将 ( "" ) 。  
  
 `tagpath`  
 必需。 `filename` 中标记的路径，它指向标记 `name`。 将路径用双引号引起来 ( "" ) 。  
  
 `name`  
 必需。 注释前面的标记中的名称说明符。 `Name` 将有一个 `id` 。  
  
 `id`  
 必需。 标记的 ID（位于注释之前）。 将 ID 括在单引号中 ( "" ) 。  
  
## <a name="remarks"></a>备注  

 使用 `<include>` 标记来引用另一个文件中描述源代码中的类型和成员的注释。 这是对直接在源代码文件中放入文档注释的替代方法。  
  
 `<include>`标记使用 W3C XML Path 语言 (XPath) 版本1.0 建议。 有关自定义使用方式的详细信息 `<include>` ，请参阅 <https://www.w3.org/TR/xpath> 。  
  
## <a name="example"></a>示例  

 此示例使用 `<include>` 标记从名为的文件导入成员文档注释 `commentFile.xml` 。  
  
 [!code-vb[VbVbcnXmlDocComments#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#4)]  
  
 的格式如下所示 `commentFile.xml` 。  
  
```xml  
<Docs>  
<Members name="Open">  
<summary>Opens a file.</summary>  
<param name="filename">File name to open.</param>  
</Members>  
<Members name="Close">  
<summary>Closes a file.</summary>  
<param name="filename">File name to close.</param>  
</Members>  
</Docs>  
```  
  
## <a name="see-also"></a>另请参阅

- [XML 注释标记](index.md)
