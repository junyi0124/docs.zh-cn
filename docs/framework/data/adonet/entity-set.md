---
title: 实体集
ms.date: 03/30/2017
ms.assetid: 59ec6ab0-88e5-4d25-b112-7a4eccbe61f0
ms.openlocfilehash: 6286d3707a8506e7a389359a5aa361c152e75212
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194786"
---
# <a name="entity-set"></a>实体集

*实体集*是某个[实体类型](entity-type.md)的实例和从该实体类型派生的任何类型的实例的逻辑容器。  (有关派生类型的信息，请参阅 [实体数据模型：继承](entity-data-model-inheritance.md)。 ) 实体类型与实体集之间的关系类似于关系数据库中行与表之间的关系：与行相同，实体类型描述数据结构，而与表一样，实体集包含给定结构的实例。 实体集不是一种数据建模构造，它没有描述数据结构。 相反，实体集提供了一种承载或存储环境构造（例如公共语言运行库或 SQL Server 数据库）来分组实体类型实例，以便可以将它们映射到某个数据存储区。  
  
 实体集是在 [实体容器](entity-container.md)中定义的，实体容器是实体集和 [关联集](association-set.md)的逻辑分组。  
  
 对于实体集中存在的实体类型实例，必须满足以下条件：  
  
- 实例的类型或者与实体集所基于的实体类型相同，或者是该实体类型的一个子类型。  
  
- 实例的 [实体键](entity-key.md) 在实体集内是唯一的。  
  
- 实例不存在于任何其他实体集中。  
  
    > [!NOTE]
    > 可以使用相同的实体类型定义多个实体集，但某个给定实体类型的一个实例只能存在于一个实体集中。  
  
 不必为概念模型中的每个实体类型都定义实体集。  
  
## <a name="example"></a>示例  

 下图显示了一个具有三个实体类型的概念模型：`Book`、`Publisher` 和 `Author`。  
  
 ![具有三个实体类型的示例模型](./media/entity-set/example-model-three-entity-types.gif)  
  
 下图显示了基于上面所示的概念模型的两个实体集（`Books` 和 `Publishers`）和一个关联集 (`PublishedBy`)。 `Books`实体集中的 Bi 表示 `Book` 运行时实体类型的实例。 同样，Pj 表示 `Publisher` 实体集中的实例 `Publishers` 。 BiPj 表示 `PublishedBy` 关联集中关联的实例 `PublishedBy` 。  
  
 ![显示集合示例的屏幕截图。](./media/entity-set/sets-example-association.gif)  
  
 [ADO.NET 实体框架](./ef/index.md)使用一种特定于域的语言 (DSL) 称为概念架构定义语言 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 来定义概念模型。 下面的 CSDL 定义了一个实体容器，其中对于上图所示的概念模型中的每个实体类型都有一个实体集。 请注意，每个实体集的名称和实体类型都是使用 XML 特性定义的。  
  
 [!code-xml[EDM_Example_Model#EntityContainerExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#entitycontainerexample)]  
  
 每个类型可以定义多个实体集 (MEST)。 下面的 CSDL 定义了一个实体容器，其中包含 `Book` 实体类型的两个实体集：  
  
 [!code-xml[EDM_Example_Model#MESTExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books2.edmx#mestexample)]  
  
## <a name="see-also"></a>请参阅

- [实体数据模型关键概念](entity-data-model-key-concepts.md)
- [实体数据模型](entity-data-model.md)
