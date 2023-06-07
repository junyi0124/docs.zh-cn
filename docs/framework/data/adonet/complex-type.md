---
title: Complex Type — 复杂类型
ms.date: 03/30/2017
ms.assetid: 63efbd23-11d4-4871-bc88-ad01b9837553
ms.openlocfilehash: ef20de6a9e72d3123d745ef5501ecdb7fa63967d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203782"
---
# <a name="complex-type"></a>Complex Type — 复杂类型

" *复杂类型* " 是用于定义 [实体类型](entity-type.md) 或其他复杂类型上的丰富的结构化属性的模板。 每个模板都包含以下内容：  
  
- 唯一名称。 （必需）  
  
    > [!NOTE]
    > 复杂类型的名称不能与同一命名空间中的实体类型的名称相同。  
  
- 采用一个或多个 [属性](property.md)格式的数据。 （可选。）  
  
    > [!NOTE]
    > 复杂类型的属性可以是另一个复杂类型。  
  
 复杂类型与实体类型相似，因为复杂类型可以以基元类型属性或其他复杂类型的形式携带数据负载。 但是，在复杂类型与实体类型之间仍存在着一些重要区别：  
  
- 复杂类型没有标识，因此不能独立存在。 复杂类型只能作为实体类型或其他复杂类型的属性而存在。  
  
- 复杂类型不能参与 [关联](association-type.md)。 关联的两端都不能是复杂类型，因此不能为复杂类型定义 [导航属性](navigation-property.md) 。  
  
## <a name="example"></a>示例  

 [ADO.NET 实体框架](./ef/index.md)使用一种特定于域的语言 (DSL) 称为概念架构定义语言 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 来定义概念模型。 下面的 CSDL 定义了一个复杂类型 Address，它具有基元类型属性 `StreetAddress`、`City`、`StateOrProvince`、`Country` 和 `PostalCode`。  
  
 [!code-xml[EDM_Example_Model#ComplexTypeExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books2.edmx#complextypeexample)]  
  
 若要将复杂类型 `Address`（如上所示）定义为某个实体类型的属性，必须在实体类型定义中声明该属性类型。 下面的 CSDL 将 `Address` 属性声明为实体类型 (Publisher) 的复杂类型：  
  
 [!code-xml[EDM_Example_Model#EntityWithComplexType](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books3.edmx#entitywithcomplextype)]  
  
## <a name="see-also"></a>请参阅

- [实体数据模型关键概念](entity-data-model-key-concepts.md)
- [实体数据模型](entity-data-model.md)
