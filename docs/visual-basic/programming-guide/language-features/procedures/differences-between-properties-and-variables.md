---
title: 属性和变量之间的差异
ms.date: 07/20/2015
helpviewer_keywords:
- property values [Visual Basic]
- variables [Visual Basic]
- Visual Basic code, procedures
- values [Visual Basic], properties
- variables [Visual Basic], definition
- Visual Basic code, variables
- Visual Basic code, properties
- properties [Visual Basic], values
- properties [Visual Basic], defined
- variables [Visual Basic], and properties
- properties [Visual Basic], and variables
ms.assetid: 7a03a8be-5381-431f-bd7c-16e887e4e07b
ms.openlocfilehash: 95bafcaca98e1a0fbdd62a550291c8ece932c1ba
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075026"
---
# <a name="differences-between-properties-and-variables-in-visual-basic"></a>Visual Basic 中属性和变量的差异

变量和属性都表示可以访问的值。 但在存储和实现方面存在差异。  
  
## <a name="variables"></a>变量  

 *变量*直接对应于内存位置。 使用单个声明语句定义变量。 变量可以是在过程中定义的 *本地变量*，只能在该过程中使用，也可以是在模块、类或结构中定义但不在任何过程内的 *成员变量*。 成员变量也称为 *字段*。  
  
## <a name="properties"></a>属性  

 *属性*是在模块、类或结构上定义的数据元素。 使用和语句之间的代码块定义属性 `Property` `End Property` 。 代码块包含 `Get` 过程和 `Set` /或过程。 这些过程称为 *属性过程* 或 *属性访问器*。 除了检索或存储属性值以外，还可以执行自定义操作，如更新访问计数器。  
  
## <a name="differences"></a>差异  

 下表显示了变量和属性之间的一些重要差异。  
  
|差异点|变量|属性|  
|-------------------------|--------------|--------------|  
|声明|单个声明语句|代码块中的一系列语句|  
|实现|单个存储位置|可执行代码 (属性过程) |  
|存储|直接与变量的值关联|通常，内部存储在属性的包含类或模块外部不可用<br /><br /> 属性的值不能作为存储元素<sup>1</sup>存在，也可能不存在。|  
|可执行代码|None|必须至少有一个过程|  
|读写访问权限|读/写或只读|读/写、只读或只写|  
|除了接受或返回值外， (自定义操作) |不可用|可以在设置或检索属性值的过程中执行|  
  
 <sup>1</sup> 与变量不同，属性的值可能不会直接对应于单个存储项。 为了方便或安全，存储区可能会拆分为若干个部分，或者值可能以加密形式存储。 在这些情况下，该 `Get` 过程会组装部分或解密存储的值，并且该 `Set` 过程将加密新值或将其拆分为构成存储。 属性值可以是暂时的，如一天中的时间，在这种情况下， `Get` 每次访问该属性时，该过程会动态计算该属性。  
  
## <a name="see-also"></a>请参阅

- [Property 过程](./property-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Property Statement](../../../language-reference/statements/property-statement.md)
- [Dim 语句](../../../language-reference/statements/dim-statement.md)
- [如何：创建属性](./how-to-create-a-property.md)
- [如何：声明具有混合访问级别的属性](./how-to-declare-a-property-with-mixed-access-levels.md)
- [如何：调用 Property 过程](./how-to-call-a-property-procedure.md)
- [如何：在 Visual Basic 中声明和调用默认属性](./how-to-declare-and-call-a-default-property.md)
- [如何：在属性中放置值](./how-to-put-a-value-in-a-property.md)
- [如何：从属性获取值](./how-to-get-a-value-from-a-property.md)
