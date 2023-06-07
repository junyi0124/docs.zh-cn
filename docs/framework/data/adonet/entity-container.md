---
title: Entity Container — 实体容器
ms.date: 03/30/2017
ms.assetid: 16e80405-2c75-42fc-b0e4-b1df53b1c584
ms.openlocfilehash: 95fb59c86f951e75f0988f45219fd07cbb003c01
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200818"
---
# <a name="entity-container"></a>Entity Container — 实体容器

*实体容器*是[实体集](entity-set.md)、[关联集](association-set.md)和[函数导入](model-declared-function.md)的逻辑分组。  
  
 对于在概念模型中定义的实体容器，必须满足以下条件：  
  
- 在每个概念模型中必须至少定义一个实体容器。  
  
- 每个概念模型中的实体容器必须有唯一的名称。  
  
 实体容器可以定义使用在一个或多个命名空间中定义的实体类型或关联的实体集或关联集。 有关详细信息，请参阅 [实体数据模型：命名空间](entity-data-model-namespaces.md)。  
  
## <a name="example"></a>示例  

 下图显示了一个具有三个实体类型的概念模型：`Book`、`Publisher` 和 `Author`。  有关更多信息，请参见下一个示例。  
  
 ![具有三个实体类型的示例模型](./media/entity-container/example-model-three-entity-types.gif)  
  
 虽然该图没有传达实体容器信息，但概念模型必须定义一个实体容器。 [ADO.NET 实体框架](./ef/index.md)使用称为概念架构定义语言 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 的 DSL 来定义概念模型。 下面的 CSDL 为上图所示的概念模型定义了一个实体容器。 请注意，实体容器名称是在一个 XML 特性中定义的。  
  
 [!code-xml[EDM_Example_Model#EntityContainerExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#entitycontainerexample)]  
  
## <a name="see-also"></a>请参阅

- [实体数据模型关键概念](entity-data-model-key-concepts.md)
- [实体数据模型](entity-data-model.md)
