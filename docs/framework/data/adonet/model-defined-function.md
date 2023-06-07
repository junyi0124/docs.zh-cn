---
title: 模型定义函数
ms.date: 03/30/2017
ms.assetid: 8bb2edc8-e8e7-44c2-adc7-f44e11bda4f0
ms.openlocfilehash: 04d27387c30d5fe09d31c1b2cc94741f21ffe8e0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150773"
---
# <a name="model-defined-function"></a>模型定义函数

*模型定义的函数*是在概念模型中定义的函数。 模型定义函数的主体以 [实体 SQL](./ef/language-reference/entity-sql-language.md)表示，这允许独立于数据源中支持的规则或语言来表示函数。  
  
 模型定义函数的定义包含以下信息：  
  
- 函数名称。 （必需）  
  
- 返回值的类型。 (可选)  
  
    > [!NOTE]
    > 如果未指定返回类型，则返回值为 void。  
  
- 参数信息。 (可选)  
  
- 定义函数主体的 [实体 SQL](./ef/language-reference/entity-sql-language.md) 表达式。  
  
 请注意，模型定义函数不支持输出参数。 这一限制可以保证无法对模型定义函数进行编写。  
  
## <a name="example"></a>示例  

 下图显示了一个具有三个实体类型的概念模型：`Book`、`Publisher` 和 `Author`。  
  
 ![显示具有发布日期的模型的屏幕截图。](./media/model-defined-function/model-published-date-three-entity-types.gif)  
  
 [ADO.NET 实体框架](./ef/index.md)使用一种特定于域的语言 (DSL) 称为概念架构定义语言 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) 来定义概念模型。 下面的 CSDL 在概念模型中定义了一个函数，它返回自某个 `Book` 实例（如上图所示）出版以来的年数。  
  
 [!code-xml[EDM_Example_Model#ModelDefinedFunction](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books4.edmx#modeldefinedfunction)]  
  
## <a name="see-also"></a>请参阅

- [实体数据模型关键概念](entity-data-model-key-concepts.md)
- [实体数据模型](entity-data-model.md)
- [实体数据模型：基元数据类型](entity-data-model-primitive-data-types.md)
