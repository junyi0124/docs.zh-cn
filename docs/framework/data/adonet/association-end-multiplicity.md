---
title: 关联端重数
ms.date: 03/30/2017
ms.assetid: 340926ee-aefb-4bef-92cc-453e5251fd03
ms.openlocfilehash: cf9e6b5af0dc6a33af364901c631b4ce66fe0480
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153503"
---
# <a name="association-end-multiplicity"></a>关联端重数

*关联 end 多重性*定义可以位于[关联](association-type.md)一端的[实体类型](entity-type.md)实例的数量。  
  
 关联端重数可以具有下列值之一：  
  
- 一 (1)：表明在关联端存在且只存在一个实体类型实例。  
  
- 零或一 (0..1)：表明在关联端不存在实体类型实例或存在一个实体类型实例。  
  
- 许多 (\*) ：指示在关联端存在零个、一个或多个实体类型实例。  
  
 关联的特征通常由关联端重数表示。 例如，如果关联的两端有一个 (1) 和多个 (\*) 的重数，则该关联称为一对多关联。 在下面的示例中，`PublishedBy` 关联是一个一对多关联（一个出版商可以出版很多书，而一本书只能由一个出版商出版）。 `WrittenBy` 关联是一个多对多关联（一本书可以有多位作者，而一位作者可以著有多本书）。  
  
## <a name="example"></a>示例  

 下图显示了一个具有两个关联的概念模型：`PublishedBy` 和 `WrittenBy`。 `PublishedBy` 关联的关联端是 `Book` 和 `Publisher` 实体类型。 末尾的重数 `Publisher` 为 1 (1) ，结束的重数 `Book` 为多 (\*) 。  
  
 ![具有三个实体类型的示例模型](./media/association-end-multiplicity/example-model-three-entity-types.gif)  
  
 ADO.NET 实体框架使用一种特定于域的语言 (DSL) 称为概念架构定义语言 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 来定义概念模型。 下面的 CSDL 定义了上图中显示的 `PublishedBy` 关联：  
  
 [!code-xml[EDM_Example_Model#AssociationExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#associationexample)]  
  
## <a name="see-also"></a>请参阅

- [实体数据模型关键概念](entity-data-model-key-concepts.md)
- [实体数据模型](entity-data-model.md)
