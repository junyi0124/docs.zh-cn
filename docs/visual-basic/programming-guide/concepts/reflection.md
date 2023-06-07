---
title: 反射
ms.date: 07/20/2015
ms.assetid: d991bc0f-d16a-4ac5-9351-70e5c5b9891b
ms.openlocfilehash: 79603e0951732c7d0d0031d4fc44ddd7dbd392c9
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077249"
---
# <a name="reflection-visual-basic"></a>反射 (Visual Basic)

反射提供描述程序集、模块和类型的对象（<xref:System.Type> 类型）。 可以使用反射动态地创建类型的实例，将类型绑定到现有对象，或从现有对象中获取类型，然后调用其方法或访问器字段和属性。 如果代码中使用了特性，可以利用反射来访问它们。 有关更多信息，请参阅[特性](../../../standard/attributes/index.md)。  
  
 下面一个简单的反射示例，使用静态方法 `GetType`被 `Object` 基类的所有类型继承）以获取变量类型：  
  
```vb  
' Using GetType to obtain type information:  
Dim i As Integer = 42  
Dim type As System.Type = i.GetType()  
System.Console.WriteLine(type)  
```  
  
 输出为：  
  
 `System.Int32`  
  
 下面的示例使用反射获取已加载的程序集的完整名称。  
  
```vb  
' Using Reflection to get information from an Assembly:  
Dim info As System.Reflection.Assembly = GetType(System.Int32).Assembly  
System.Console.WriteLine(info)  
```  
  
 输出为：  
  
 `mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089`  
  
## <a name="reflection-overview"></a>反射概述  

 反射在以下情况下很有用：  
  
- 需要访问程序元数据中的特性时。 有关详细信息，请参阅[检索存储在特性中的信息](../../../standard/attributes/retrieving-information-stored-in-attributes.md)。  
  
- 检查和实例化程序集中的类型。  
  
- 在运行时构建新类型。 使用 <xref:System.Reflection.Emit> 中的类。  
  
- 执行后期绑定，访问在运行时创建的类型上的方法。 请参阅主题 “[动态加载和使用类型](../../../framework/reflection-and-codedom/dynamically-loading-and-using-types.md)”。  
  
## <a name="related-sections"></a>相关章节  

 更多相关信息：  
  
- [反射](../../../framework/reflection-and-codedom/reflection.md)  
  
- [查看类型信息](../../../framework/reflection-and-codedom/viewing-type-information.md)  
  
- [反射类型和泛型类型](../../../framework/reflection-and-codedom/reflection-and-generic-types.md)  
  
- <xref:System.Reflection.Emit>  
  
- [检索存储在特性中的信息](../../../standard/attributes/retrieving-information-stored-in-attributes.md)  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 编程指南](../index.md)
- [.NET 中的程序集](../../../standard/assembly/index.md)
