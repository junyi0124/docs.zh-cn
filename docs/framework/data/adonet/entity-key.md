---
title: 实体键
ms.date: 03/30/2017
ms.assetid: 0d447a6d-fa7a-4db0-8e7a-fd45e385fca0
ms.openlocfilehash: b2692faf2f8bea27c8a5d6cdc52689ca186d3194
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200779"
---
# <a name="entity-key"></a>实体键

"*实体键*" 是用于确定标识的[实体类型](entity-type.md)的[一个或一组属性。](property.md) 构成实体键的属性是在设计时选择的。 实体键属性的值必须在运行时唯一标识 [实体集中](entity-set.md) 的实体类型实例。 在选择构成实体键的属性时应确保实例在实体集中的唯一性。  
  
 下面是将一组属性用作实体键的要求：  
  
- 实体集中不能存在两个相同的实体键。 也就是说，对于实体集中的任意两个实体，构成一个键的所有属性的值不能全部相同。 但是，构成实体键的部分（而不是全部）值可以相同。  
  
- 实体键必须包含一组不可为 null 的不可变的 [基元类型属性](entity-data-model-primitive-data-types.md)。  
  
- 构成给定实体类型的实体键的属性不可更改。 对于某个给定实体类型，不能允许存在多个可能的实体键；不支持代理键。  
  
- 当实体位于继承层次结构中时，根实体必须包含构成实体键的所有属性，并且必须在根实体类型上定义实体键。 有关详细信息，请参阅 [实体数据模型：继承](entity-data-model-inheritance.md)。  
  
## <a name="example"></a>示例  

 下图显示了一个具有三个实体类型的概念模型：`Book`、`Publisher` 和 `Author`。 构成其实体键的每个实体类型的属性均用“(Key)”标示出来。 请注意，`Author` 实体类型有一个包含两个属性（`Name` 和 `Address`）的实体键。  
  
 ![具有三个实体类型的示例模型](./media/entity-key/example-model-three-entity-types.gif)  
  
 [ADO.NET 实体框架](./ef/index.md)使用一种特定于域的语言 (DSL) 称为概念架构定义语言 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 来定义概念模型。 下面的 CSDL 定义了上图中显示的 `Book` 实体类型。 请注意，实体键通过引用实体类型的 `ISBN` 属性来定义。  
  
 [!code-xml[EDM_Example_Model#EntityExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#entityexample)]  
  
 对实体键使用 `ISBN` 属性是一个不错的选择，因为国际标准书号 (ISBN) 可以唯一地标识一本书。  
  
 下面的 CSDL 定义了上图中显示的 `Author` 实体类型。 请注意，该实体键包含两个属性，`Name` 和 `Address`。  
  
 [!code-xml[EDM_Example_Model#CompositeKeyExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#compositekeyexample)]  
  
 对实体键使用 `Name` 和 `Address` 是一个合理的选择，因为两个同名作者住址相同的可能性很小。 但是，针对实体键的这种选择并不能绝对确保实体键在实体集中的唯一性。 在这种情况下，建议添加一个可用来唯一标识作者的属性，例如 `AuthorId`。  
  
## <a name="see-also"></a>请参阅

- [实体数据模型关键概念](entity-data-model-key-concepts.md)
- [实体数据模型](entity-data-model.md)
