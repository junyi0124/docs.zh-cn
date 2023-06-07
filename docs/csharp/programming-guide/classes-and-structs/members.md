---
title: 成员 - C# 编程指南
description: C# 中的类和结构具有表示数据和行为的成员，包括在类中声明和在其继承层次结构中声明的成员。
ms.date: 07/20/2015
helpviewer_keywords:
- types [C#], nested types
- C# language, type members
ms.assetid: 4a30a4ab-d690-4936-9124-92ce9448665a
ms.openlocfilehash: aa4981ea20d86994bfae92d5db8c9abfa7c8f906
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864587"
---
# <a name="members-c-programming-guide"></a>成员（C# 编程指南）

类和结构具有表示其数据和行为的成员。 类的成员包括在类中声明的所有成员，以及在该类的继承层次结构中的所有类中声明的所有成员（构造函数和析构函数除外）。 基类中的私有成员被继承，但不能从派生类访问。  
  
 下表列出类或结构中可包含的成员类型：  
  
|成员|描述|  
|------------|-----------------|  
|[字段](./fields.md)|字段是在类范围声明的变量。 字段可以是内置数值类型或其他类的实例。 例如，日历类可能具有一个包含当前日期的字段。|  
|[常量](./constants.md)|常量是在编译时设置其值并且不能更改其值的字段。|  
|[属性](./properties.md)|属性是类中可以像类中的字段一样访问的方法。 属性可以为类字段提供保护，以避免字段在对象不知道的情况下被更改。|  
|[方法](./methods.md)|方法定义类可以执行的操作。 方法可接受提供输入数据的参数，并可通过参数返回输出数据。 方法还可以不使用参数而直接返回值。|  
|[事件](../events/index.md)|事件向其他对象提供有关发生的事情（如单击按钮或成功完成某个方法）的通知。 事件是使用委托定义和触发的。|  
|[运算符](../../language-reference/operators/index.md)|重载运算符被视为类型成员。 重载运算符时，将其定义为类型中的公共静态方法。 有关详细信息，请参阅[运算符重载](../../language-reference/operators/operator-overloading.md)。|  
|[索引器](../indexers/index.md)|使用索引器可以用类似于数组的方式为对象建立索引。|  
|[构造函数](./constructors.md)|构造函数是首次创建对象时调用的方法。 它们通常用于初始化对象的数据。|  
|[终结器](./destructors.md)|C# 中很少使用终结器。 终结器是当对象即将从内存中移除时由运行时执行引擎调用的方法。 它们通常用来确保任何必须释放的资源都得到适当的处理。|  
|[嵌套类型](./nested-types.md)|嵌套类型是在其他类型中声明的类型。 嵌套类型通常用于描述仅由包含它们的类型使用的对象。|  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [类](./classes.md)
