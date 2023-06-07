---
title: 推断表
ms.date: 03/30/2017
ms.assetid: 74a288d4-b8e9-4f1a-b2cd-10df92c1ed1f
ms.openlocfilehash: 4a3d7b239dbc405cf2acae967b5be401dc772e38
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177548"
---
# <a name="inferring-tables"></a>推断表

当从 XML 文档推断 <xref:System.Data.DataSet> 的架构时，ADO.NET 首先会确定哪些 XML 元素表示表。 以下 XML 结构将为 **数据集** 架构生成一个表：  
  
- 具有属性的元素  
  
- 具有子元素的元素  
  
- 重复元素  
  
## <a name="elements-with-attributes"></a>具有属性的元素  

 在其中指定了属性的元素将生成推断表。 例如，考虑以下 XML：  
  
```xml  
<DocumentElement>  
  <Element1 attr1="value1"/>  
  <Element1 attr1="value2">Text1</Element1>  
</DocumentElement>  
```  
  
 推断过程将生成名为“Element1”的表。  
  
 **数据集：** DocumentElement  
  
 **表：** Element1  
  
|attr1|Element1_Text|  
|-----------|--------------------|  
|value1||  
|value2|Text1|  
  
## <a name="elements-with-child-elements"></a>具有子元素的元素  

 具有子元素的元素将生成推断表。 例如，考虑以下 XML：  
  
```xml  
<DocumentElement>  
  <Element1>  
    <ChildElement1>Text1</ChildElement1>  
  </Element1>  
</DocumentElement>  
```  
  
 推断过程将生成名为“Element1”的表。  
  
 **数据集：** DocumentElement  
  
 **表：** Element1  
  
|ChildElement1|  
|-------------------|  
|Text1|  
  
 如果文档元素（即根元素）具有将被推断为列的属性或子元素，将生成推断表。 如果文档元素没有属性，并且没有任何子元素将被推断为列，则该元素将被推断为 **数据集**。 例如，考虑以下 XML：  
  
```xml  
<DocumentElement>  
  <Element1>Text1</Element1>  
  <Element2>Text2</Element2>  
</DocumentElement>  
```  
  
 推断过程将生成名为“DocumentElement”的表。  
  
 **数据集：** NewDataSet  
  
 **表：** DocumentElement  
  
|Element1|Element2|  
|--------------|--------------|  
|Text1|Text2|  
  
 或者，考虑以下 XML：  
  
```xml  
<DocumentElement>  
  <Element1 attr1="value1" attr2="value2"/>  
</DocumentElement>  
```  
  
 推理过程将生成一个名为 "DocumentElement" 的 **数据集** ，其中包含一个名为 "Element1" 的表。  
  
 **数据集：** DocumentElement  
  
 **表：** Element1  
  
|attr1|attr2|  
|-----------|-----------|  
|value1|value2|  
  
## <a name="repeating-elements"></a>重复元素  

 重复的元素将生成单个推断表。 例如，考虑以下 XML：  
  
```xml  
<DocumentElement>  
  <Element1>Text1</Element1>  
  <Element1>Text2</Element1>  
</DocumentElement>  
```  
  
 推断过程将生成名为“Element1”的表。  
  
 **数据集：** DocumentElement  
  
 **表：** Element1  
  
|Element1_Text|  
|--------------------|  
|Text1|  
|Text2|  
  
## <a name="see-also"></a>请参阅

- [从 XML 推断数据集关系结构](inferring-dataset-relational-structure-from-xml.md)
- [从 XML 加载数据集](loading-a-dataset-from-xml.md)
- [从 XML 加载数据集架构信息](loading-dataset-schema-information-from-xml.md)
- [在数据集中使用 XML](using-xml-in-a-dataset.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
