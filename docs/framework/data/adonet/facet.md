---
title: facet
ms.date: 03/30/2017
ms.assetid: 91c4e6aa-3e54-4b6c-a38a-abf27808cc85
ms.openlocfilehash: b9ef2276f988923fe83cefce910e8c3685cb9da9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156441"
---
# <a name="facet"></a>facet

*Facet*用于向基元类型属性定义添加详细信息。 [属性](property.md)定义包含有关属性类型的信息，但通常需要更详细的信息。 例如，概念模型中的实体类型可能有一个类型为 `String` 的属性，其值不能设置为 null。 通过方面可以指定这种详细程度。  
  
 下表描述了 EDM 中支持的方面。  
  
> [!NOTE]
> 方面的确切值和行为由使用 EDM 实现的运行时环境确定。  
  
|方面|描述|适用于|  
|-----------|-----------------|----------------|  
|`Collation`|指定在对属性值执行比较和排序操作时要使用的排序序列。|`String`|  
|`ConcurrencyMode`|表示应使用属性的值来进行开放式并发检查。|所有基元类型属性|  
|`Default`|如果在安装时未提供值，则指定属性的默认值。|所有基元类型属性|  
|`FixedLength`|指定属性值的长度是否可变。|`Binary`, `String`|  
|`MaxLength`|指定属性值的最大长度。|`Binary`, `String`|  
|`Nullable`|指定属性是否可以具有 null 值。|所有基元类型属性|  
|`Precision`|对于类型 `Decimal` 的属性，指定属性值可以具有的位数。 对于类型 `Time`、`DateTime` 和 `DateTimeOffset` 的属性，指定属性值的秒的小数部分的位数。|`DateTime`, `DateTimeOffset`, `Decimal`, `Time`,|  
|`Scale`|指定属性值小数点右侧的位数。|小数|  
|`Unicode`|指示是否将属性值存储为 Unicode。|`String`|  
  
## <a name="example"></a>示例  

 [ADO.NET 实体框架](./ef/index.md)使用一种特定于域的语言 (DSL) 称为概念架构定义语言 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 来定义概念模型。 下面的 CSDL 定义了一个 `Book` 实体类型。 请注意，方面是作为 XML 特性实现的。 方面值表明不能将属性设置为 null，并且 `Scale` 属性的 `Precision` 和 `Revision` 都设置为 29。  
  
 [!code-xml[EDM_Example_Model#EntityExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#entityexample)]  
  
## <a name="see-also"></a>请参阅

- [实体数据模型关键概念](entity-data-model-key-concepts.md)
- [实体数据模型](entity-data-model.md)
