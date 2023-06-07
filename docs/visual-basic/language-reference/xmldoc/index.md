---
title: 建议用于文档注释的 XML 标记
ms.date: 07/20/2015
f1_keywords:
- vb.XmlDocComment
helpviewer_keywords:
- tags, XML
- XML comments, recommended tags [Visual Basic]
- comments, recommended XML tags
ms.assetid: 294e0736-ff1e-498e-af83-6db71ed41a72
ms.openlocfilehash: 9f877ee3fc9d616dc1e946293489a8aab96ac2e1
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90872792"
---
# <a name="recommended-xml-tags-for-documentation-comments-visual-basic"></a>建议的用于文档注释的 XML 标记 (Visual Basic)

Visual Basic 编译器可以在代码中将文档注释处理到 XML 文件中。 您可以使用其他工具将 XML 文件处理到文档中。  
  
 允许对代码构造（如类型和类型成员）使用 XML 注释。 对于分部类型，只有一个类型的部分可以有 XML 注释，尽管注释其成员没有限制。  
  
> [!NOTE]
> 文档注释不能应用于命名空间。 原因是一个命名空间可以跨多个程序集，而不是所有程序集都必须同时加载。  
  
 编译器将处理任何为有效 XML 的标记。 以下标记提供用户文档中的常用功能。  
  
||||  
|---|---|---|  
|[\<c>](c.md)|[\<code>](code.md)|[\<example>](example.md)|  
|[\<exception>](exception.md)<sup>1</sup>|[\<include>](include.md)<sup>1</sup>|[\<list>](list.md)|  
|[\<para>](para.md)|[\<param>](param.md)<sup>1</sup>|[\<paramref>](paramref.md)|  
|[\<permission>](permission.md)<sup>1</sup>|[\<remarks>](remarks.md)|[\<returns>](returns.md)|  
|[\<see>](see.md)<sup>1</sup>|[\<seealso>](seealso.md)<sup>1</sup>|[\<summary>](summary.md)|  
|[\<typeparam>](typeparam.md)<sup>1</sup>|[\<value>](value.md)||  
  
  (<sup>1</sup> 编译器验证语法。 )   
  
> [!NOTE]
> 如果要在文档注释的文本中显示尖括号，请使用 `&lt;` 和 `&gt;` 。 例如，该字符串 `"&lt;text in angle brackets&gt;"` 将显示为 `<text in angle brackets>` 。  
  
## <a name="see-also"></a>请参阅

- [使用 XML 记录代码](../../programming-guide/program-structure/documenting-your-code-with-xml.md)
- [-doc](../../reference/command-line-compiler/doc.md)
- [如何：创建 XML 文档](../../programming-guide/program-structure/how-to-create-xml-documentation.md)
