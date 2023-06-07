---
title: 如何在派生类中引发基类事件 - C# 编程指南
description: 了解如何在派生类中引发基类事件。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- events [C#], in derived classes
ms.assetid: 2d20556a-0aad-46fc-845e-f85d86ea617a
ms.openlocfilehash: b9708876e7d46072439ae498fd551a7b3b23a29e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167517"
---
# <a name="how-to-raise-base-class-events-in-derived-classes-c-programming-guide"></a>如何在派生类中引发基类事件（C# 编程指南）

下面的简单示例演示用于在基类中声明事件，以便也可以从派生类引发它们的标准方法。 此模式广泛用于 .NET 类库中的 Windows 窗体类。  
  
 创建可以用作其他类的基类的类时，应考虑到以下事实：事件是特殊类型的委托，只能从声明它们的类中进行调用。 派生类不能直接调用在基类中声明的事件。 虽然有时可能需要只能由基类引发的事件，不过在大多数情况下，应使派生类可以调用基类事件。 为此，可以在包装事件的基类中创建受保护的调用方法。 通过调用或重写此调用方法，派生类可以间接调用事件。  
  
> [!NOTE]
> 不要在基类中声明虚拟事件并在派生类中重写它们。 C# 编译器不会处理这些事件，并且无法预知派生事件的订阅者是否实际上会订阅基类事件。  
  
## <a name="example"></a>示例  

 [!code-csharp[csProgGuideEvents#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#1)]  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [事件](./index.md)
- [委托](../delegates/index.md)
- [访问修饰符](../classes-and-structs/access-modifiers.md)
- [在 Windows 窗体中创建事件处理程序](/dotnet/desktop/winforms/creating-event-handlers-in-windows-forms)
