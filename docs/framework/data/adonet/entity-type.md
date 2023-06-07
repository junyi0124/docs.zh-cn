---
title: Entity Type — 实体类型
ms.date: 03/30/2017
ms.assetid: a6dee9ab-9e4a-48f2-a169-3f79cc15821c
ms.openlocfilehash: 3f99667a06d8aa439232802d4909290dfe9db97c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194773"
---
# <a name="entity-type"></a>Entity Type — 实体类型

*实体类型*是用实体数据模型 (EDM) 来描述数据结构的基本构造块。 在概念模型中，实体类型表示顶级概念（例如客户或订单）的结构。 实体类型是实体类型实例的模板。 每个模板都包含以下信息：  
  
- 唯一名称。 （必选。）  
  
- 由一个或多个属性定义的 [实体键](entity-key.md) 。 （必选。）  
  
- [属性](property.md)形式的数据。 （可选。）  
  
- 允许从[关联](association-type.md)的[一端](association-end.md)导航到另一端的[导航属性](navigation-property.md)。 (可选)  
  
 在应用程序中，实体类型的实例表示一个特定对象（例如特定客户或订单）。 实体类型的每个实例都必须在[实体集中](entity-set.md)具有唯一的[实体键](entity-key.md)。  
  
 只有两个实体类型实例的类型相同且其实体键的值也相同时，才认为它们是相等的。  
  
## <a name="example"></a>示例  

 下图显示了一个具有三个实体类型的概念模型：`Book`、`Publisher` 和 `Author`：  
  
 ![具有三个实体类型的示例模型](./media/entity-type/example-model-three-entity-types.gif)  
  
 请注意，构成其实体键的每个实体类型的属性均用“(Key)”标示出来。  
  
 [ADO.NET 实体框架](./ef/index.md)使用一种特定于域的语言 (DSL) 称为概念架构定义语言 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 来定义概念模型。 下面的 CSDL 定义了上图中显示的 `Book` 实体类型。  
  
 [!code-xml[EDM_Example_Model#EntityExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#entityexample)]  
  
## <a name="see-also"></a>请参阅

- [实体数据模型关键概念](entity-data-model-key-concepts.md)
- [实体数据模型](entity-data-model.md)
- [多](facet.md)
