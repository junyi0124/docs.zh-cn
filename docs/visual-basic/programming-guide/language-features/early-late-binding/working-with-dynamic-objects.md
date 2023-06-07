---
title: 使用动态对象
ms.date: 07/20/2015
helpviewer_keywords:
- dynamic objects [Visual Basic]
ms.assetid: bdee2a00-07ff-46f9-86dd-fdac9b99cc97
ms.openlocfilehash: 45f8b5c327d40f93b59c2115c75b3b7d385f5a8d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91057918"
---
# <a name="working-with-dynamic-objects-visual-basic"></a>使用动态对象 (Visual Basic)

动态对象除了类型之外，还提供了另一种方法， `Object` 以便在运行时后期绑定到对象。 动态对象通过使用在命名空间中定义的动态接口，在运行时公开属性和方法等成员 <xref:System.Dynamic> 。 你可以使用命名空间中的类 <xref:System.Dynamic> 来创建对象，这些对象使用与静态类型或格式不匹配的数据结构。 还可以使用动态对象（如 IronPython 和 IronRuby）中定义的动态对象。 有关演示如何创建动态对象或使用动态语言定义的动态对象的示例，请参阅 [演练：创建和使用动态对象](../../../../csharp/programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)、 <xref:System.Dynamic.DynamicObject> 或 <xref:System.Dynamic.ExpandoObject> 。  
  
 Visual Basic 使用接口从动态语言运行时和动态语言（例如 IronPython 和 IronRuby）绑定到对象 <xref:System.Dynamic.IDynamicMetaObjectProvider> 。 实现接口的类的示例 `IDynamicMetaObjectProvider` 为 <xref:System.Dynamic.DynamicObject> 和 <xref:System.Dynamic.ExpandoObject> 类。  
  
 如果对实现接口的对象进行了后期绑定调用 `IDynamicMetaObjectProvider` ，Visual Basic 将使用该接口绑定到动态对象。 如果对未实现接口的对象进行后期绑定调用 `IDynamicMetaObjectProvider` ，或者对接口的调用 `IDynamicMetaObjectProvider` 失败，则 Visual Basic 使用 Visual Basic 运行时的后期绑定功能绑定到对象。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Dynamic.DynamicObject>
- <xref:System.Dynamic.ExpandoObject>
- [演练：创建和使用动态对象](../../../../csharp/programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)
- [早期绑定和后期绑定](index.md)
