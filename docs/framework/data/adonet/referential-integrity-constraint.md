---
title: 引用完整性约束
description: 了解实体数据模型中的引用完整性约束，该约束确保实体类型之间始终存在有效的关联。
ms.date: 03/30/2017
ms.assetid: 3d3ba44b-4302-40d8-a7a9-62932e0395e5
ms.openlocfilehash: 6ade9d0155a6915757c7f47c5cb3ab0a51dbd437
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172731"
---
# <a name="referential-integrity-constraint"></a>引用完整性约束

实体数据模型中 (EDM) 的 *引用完整性约束* 类似于关系数据库中的引用完整性约束。 与数据库表中的列 (或列) 可以引用另一个表的主键相同的方式一样，[实体类型](entity-type.md)) [属性](property.md) (或属性可以引用另一个实体类型的[实体键](entity-key.md)。 引用的实体类型称为约束的 *主体端* 。 引用主体端的实体类型称为约束的 *依赖端* 。  
  
 引用完整性约束定义为两个实体类型之间的 [关联](association-type.md) 的一部分。 引用完整性约束的定义指定了以下信息：  
  
- 约束的主体端。 （一个实体类型，其实体键由依赖端引用。）  
  
- 主体端的实体键。  
  
- 约束的依赖端。 （一个实体类型，它的一个或多个属性引用主体端的实体键。）  
  
- 依赖端的一个或多个引用属性。  
  
 EDM 中的引用完整性约束的用途是确保始终存在有效关联。 有关详细信息，请参阅 [外键属性](foreign-key-property.md)。  
  
## <a name="example"></a>示例  

 下图显示了一个具有两个关联的概念模型：`WrittenBy` 和 `PublishedBy`。 `Book` 实体类型有一个属性 `PublisherId`，当您为 `Publisher` 关联定义一个引用完整性约束时，它将引用 `PublishedBy` 实体类型的实体键。  
  
 ![RefConstraintModel](./media/referential-integrity-constraint/reference-constraint-model.gif "引用约束模型的示例")  
  
 [ADO.NET 实体框架](./ef/index.md)使用一种特定于域的语言 (DSL) 称为概念架构定义语言 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 来定义概念模型。 下面的 CSDL 为上图所示的概念模型中的 `PublishedBy` 关联定义了一个引用完整性约束。  
  
 [!code-xml[EDM_Example_Model#RefConstraint](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books4.edmx#refconstraint)]  
  
## <a name="see-also"></a>请参阅

- [实体数据模型关键概念](entity-data-model-key-concepts.md)
- [实体数据模型](entity-data-model.md)
