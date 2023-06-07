---
title: 推断限制
ms.date: 03/30/2017
ms.assetid: 78517994-5d57-44f8-9d20-38812977de09
ms.openlocfilehash: 9d8191be137661200e1a6b84d68328c1202880ca
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172770"
---
# <a name="inference-limitations"></a>推断限制

根据每个文档中的 XML 元素，从 XML 推断 <xref:System.Data.DataSet> 架构的过程会产生不同的架构。 例如，考虑以下 XML 文档。  
  
 Document1：  
  
```xml  
<DocumentElement>  
  <Element1>Text1</Element1>  
  <Element1>Text2</Element1>  
</DocumentElement>  
```  
  
 Document2：  
  
```xml  
<DocumentElement>  
  <Element1>Text1</Element1>  
</DocumentElement>  
```  
  
 对于 "Document1"，推理过程将生成一个名为 "DocumentElement" 的 **数据集** 和一个名为 "Element1" 的表，因为 "Element1" 是重复元素。  
  
 **数据集：** DocumentElement  
  
 **表：** Element1  
  
|Element1_Text|  
|--------------------|  
|Text1|  
|Text2|  
  
 但是，对于 "Document2"，推理过程将生成一个名为 "NewDataSet" 的 **数据集** 和一个名为 "DocumentElement" 的表。 由于“Element1”不具有属性和子元素，它将被推断为列。  
  
 **数据集：** NewDataSet  
  
 **表：** DocumentElement  
  
|Element1|  
|--------------|  
|Text1|  
  
 这两个 XML 文档可能本应生成相同的架构，但根据每个文档中包含的不同元素，推断过程生成了极不相同的结果。  
  
 若要避免在从 XML 文档生成架构时可能出现的差异，建议使用 XML 架构定义语言（ (XSD) 或 XML 数据在从 XML 加载 **数据集** 时减少 (XDR) ）显式指定架构。 有关使用 XML 架构显式指定 **数据集** 架构的详细信息，请参阅 [从 Xml 架构派生数据集关系结构 (XSD) ](deriving-dataset-relational-structure-from-xml-schema-xsd.md)。  
  
## <a name="see-also"></a>请参阅

- [从 XML 推断数据集关系结构](inferring-dataset-relational-structure-from-xml.md)
- [从 XML 加载数据集](loading-a-dataset-from-xml.md)
- [从 XML 加载数据集架构信息](loading-dataset-schema-information-from-xml.md)
- [在数据集中使用 XML](using-xml-in-a-dataset.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
