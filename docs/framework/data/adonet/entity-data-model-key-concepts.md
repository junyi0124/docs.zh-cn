---
title: 实体数据模型关键概念
ms.date: 03/30/2017
ms.assetid: c635a16d-6674-45aa-9344-dcb7df992bab
ms.openlocfilehash: d020de65ff64d93c0ea925b71e5f1546eb4402aa
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91191757"
---
# <a name="entity-data-model-key-concepts"></a>实体数据模型关键概念

实体数据模型 (EDM) 使用三个关键概念来描述数据结构： *实体类型*、 *关联类型*和 *属性*。 在任何 EDM 实现中，这些都是描述数据结构的最重要概念。  
  
## <a name="entity-type"></a>实体类型  

 [实体类型](entity-type.md)是用于描述实体数据模型的数据结构的基本构造块。 在概念模型中，实体类型是从 [属性](property.md) 构造的，描述顶级概念的结构，如业务应用程序中的客户和订单。 正如计算机程序中的类定义是用于类实例的模板一样，实体类型也是实体的模板。 实体表示一个特定对象（例如特定的客户或订单）。 每个实体都必须在[实体集中](entity-set.md)具有唯一的[实体键](entity-key.md)。  实体集是指特定实体类型的实例集合。 实体集 (和 [关联集](association-set.md)) 在 [实体容器](entity-container.md)中进行逻辑分组。  
  
 实体类型支持继承功能：即一个实体类型可以从另一个实体类型中派生。 有关详细信息，请参阅 [实体数据模型：继承](entity-data-model-inheritance.md)。  
  
## <a name="association-type"></a>关联类型  

 [关联类型](association-type.md) (也称为关联) 是用于描述实体数据模型中的关系的基本构造块。 在概念模型中，关联表示两个实体类型（例如 Customer 和 Order）之间的关系。 每个关联都有两个 [关联端](association-end.md) ，用于指定关联中涉及的实体类型。 每个关联端还指定一个 [关联端多重性](association-end-multiplicity.md) ，以指示可以在该关联端的实体数量。 关联 end 多重性的值可以是 1) 、零或 1 (0 () 或多个 (\*) 。 可以通过 [导航属性](navigation-property.md)访问关联一端的实体; 如果在实体类型上公开实体，则可以通过外键访问。 有关详细信息，请参阅 [外键属性](foreign-key-property.md)。  
  
 在应用程序中，关联实例表示特定的关联（例如 Customer 实例与 Order 实例之间的关联）。 关联实例按逻辑分组到一个 [关联集](association-set.md)。  (和 [实体集](entity-set.md)) 的关联集在 [实体容器](entity-container.md)中进行逻辑分组。  
  
## <a name="property"></a>属性  

 [实体类型](entity-type.md) 包含定义其结构和特征的 [属性](property.md) 。 例如，Customer 实体类型可能具有 CustomerId、Name 及 Address 这样的属性。  
  
 概念模型中的属性类似于对计算机程序中的类所定义的属性。 正如类的属性定义类的形状和携带有关对象的信息一样，概念模型中的属性也定义实体类型的形状和携带有关实体类型实例的信息。  
  
 属性可以包含基元数据（例如字符串、整数或布尔值）或结构化数据（例如复杂类型）。 有关详细信息，请参阅 [实体数据模型：基元数据类型](entity-data-model-primitive-data-types.md)。  
  
## <a name="representations-of-a-conceptual-model"></a>概念模型的表示形式  

 *概念模型*是某些数据结构的特定表示形式，作为实体和关系。 概念模型的一种表示形式是图表。 下图用三个实体类型（`Book`、`Publisher` 和 `Author`）以及两个关联（`PublishedBy` 和 `WrittenBy`）来表示概念模型：  
  
 ![显示具有三个实体类型的概念模型的关系图。](./media/entity-data-model-key-concepts/conceptual-model-entity-types-associations.gif)  
  
 然而，如果要传达一些有关模型的详细信息，这种表现形式存在一些不足。 例如，图中没有传达属性类型和实体集信息。 域特定语言 (DSL) 可用于更明确地传达有关概念模型的大量信息。 [ADO.NET 实体框架](./ef/index.md)使用名为*概念架构定义语言* ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 的基于 XML 的 DSL 来定义概念模型。 下面是上图所示的概念模型的 CSDL 定义。  
  
 [!code-xml[EDM_Example_Model#EDMExampleCSDL](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#edmexamplecsdl)]  
  
## <a name="see-also"></a>请参阅

- [实体数据模型](entity-data-model.md)
