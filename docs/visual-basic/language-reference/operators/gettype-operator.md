---
title: GetType Operator
ms.date: 07/20/2015
f1_keywords:
- vb.GetType
helpviewer_keywords:
- GetType operator [Visual Basic]
- GetType keyword [Visual Basic]
ms.assetid: 4f733297-2503-4607-850c-15eba65fff90
ms.openlocfilehash: 9ff207ea4f2b89ea30eb8f46a3e640ccf3789974
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867009"
---
# <a name="gettype-operator-visual-basic"></a>GetType 运算符 (Visual Basic)

返回 <xref:System.Type> 指定类型的对象。 <xref:System.Type>对象提供有关类型的信息，如属性、方法和事件。  
  
## <a name="syntax"></a>语法  
  
```vb  
GetType(typename)  
```  
  
## <a name="parameters"></a>参数  
  
|参数|描述|  
|---|---|  
|`typename`|要获取其信息的类型的名称。|  
  
## <a name="remarks"></a>备注  

 `GetType`运算符返回 <xref:System.Type> 指定的对象 `typename` 。 可以在中传递任何已定义类型的名称 `typename` 。 这包括：  
  
- 任何 Visual Basic 数据类型，如 `Boolean` 或 `Date` 。  
  
- 任何 .NET Framework 类、结构、模块或接口，如 <xref:System.ArgumentException?displayProperty=nameWithType> 或 <xref:System.Double?displayProperty=nameWithType> 。  
  
- 应用程序定义的任何类、结构、模块或接口。  
  
- 应用程序定义的任何数组。  
  
- 应用程序定义的任何委托。  
  
- Visual Basic、.NET Framework 或应用程序定义的任何枚举。  
  
 如果要获取对象变量的类型对象，请使用 <xref:System.Type.GetType%2A?displayProperty=nameWithType> 方法。  
  
 `GetType`在以下情况下，该运算符可能很有用：  
  
- 您必须在运行时访问类型的元数据。 <xref:System.Type>对象提供类型成员和部署信息等元数据。 例如，您需要这样做来反映程序集。 有关详细信息，请参阅 <xref:System.Reflection?displayProperty=nameWithType>。  
  
- 要比较两个对象引用，以确定它们是否引用相同类型的实例。 如果是，则 `GetType` 返回对同一对象的引用 <xref:System.Type> 。  
  
## <a name="example"></a>示例  

 以下示例显示 `GetType` 使用中的运算符。  
  
 [!code-vb[VbVbalrOperators#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#26)]  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [运算符和表达式](../../programming-guide/language-features/operators-and-expressions/index.md)
