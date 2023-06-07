---
title: 扩展索引器属性
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyExtensionIndexer
helpviewer_keywords:
- Visual Basic code, accessing XML
- XML extension indexer [Visual Basic]
- extension indexer [Visual Basic]
- XML [Visual Basic], accessing
ms.assetid: a16a4b13-54be-432c-82b3-a87091464ada
ms.openlocfilehash: 23417cd982c2ddf06afce69d9b120ae0737fb87d
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866109"
---
# <a name="extension-indexer-property-visual-basic"></a>扩展索引器属性 (Visual Basic)

提供对集合中各个元素的访问。  
  
## <a name="syntax"></a>语法  
  
```vb  
object(index)  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`object`|必需。 可查询的集合。 即，实现或的集合 <xref:System.Collections.Generic.IEnumerable%601> <xref:System.Linq.IQueryable%601> 。|  
|(|必需。 表示索引器属性的开头。|  
|`index`|必需。 一个整数表达式，指定集合的元素的从零开始的位置。|  
|)|必需。 表示索引器属性的结尾。|  
  
## <a name="return-value"></a>返回值  

 集合中指定位置的对象; `Nothing` 如果索引超出范围，则为。  
  
## <a name="remarks"></a>备注  

 可以使用扩展索引器属性访问集合中的单个元素。 此索引器属性通常用于 XML 轴属性的输出。 XML 子和 XML 子代轴属性返回对象的集合 <xref:System.Xml.Linq.XElement> 或属性值。  
  
 Visual Basic 编译器将扩展索引器属性转换为对方法的调用 `ElementAtOrDefault` 。 与数组索引器不同， `ElementAtOrDefault` 如果索引超出范围，则该方法返回 `Nothing` 。 如果无法轻松确定集合中的元素数，则此行为很有用。  
  
 此索引器属性类似于实现或的集合的扩展 <xref:System.Collections.Generic.IEnumerable%601> 属性 <xref:System.Linq.IQueryable%601> ：只有当集合没有索引器或默认属性时，才使用此属性。  
  
 若要访问或对象的集合中第一个元素的 <xref:System.Xml.Linq.XElement> 值 <xref:System.Xml.Linq.XAttribute> ，可以使用 XML `Value` 属性。 有关详细信息，请参阅 [XML Value 属性](xml-value-property.md)。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用扩展索引器访问对象集合中的第二个子节点 <xref:System.Xml.Linq.XElement> 。 使用子轴属性访问该集合，该属性可获取对象中命名的所有子元素 `phone` `contact` 。  
  
 [!code-vb[VbXMLSamples#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples11.vb#24)]  
  
 此代码显示以下文本：  
  
 `Second phone number: 425-555-0145`  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Xml.Linq.XElement>
- [XML 轴属性](index.md)
- [XML 文本](../xml-literals/index.md)
- [在 Visual Basic 中创建 XML](../../programming-guide/language-features/xml/creating-xml.md)
- [XML 值属性](xml-value-property.md)
