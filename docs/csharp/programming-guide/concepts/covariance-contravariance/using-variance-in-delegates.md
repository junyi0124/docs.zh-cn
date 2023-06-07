---
title: 使用委托中的变体 (C#)
description: 了解如何使用包含的协变和逆变代码示例在委托中使用变体。
ms.date: 07/20/2015
ms.assetid: 1638c95d-dc8b-40c1-972c-c2dcf84be55e
ms.openlocfilehash: 6704c3bf09dd854335f1e2719ccc8462cb7cde26
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176313"
---
# <a name="using-variance-in-delegates-c"></a>使用委托中的变体 (C#)

向委托分配方法时，协变和逆变为匹配委托类型和方法签名提供了灵活性。 协变允许方法具有的派生返回类型多于委托中定义的类型。 逆变允许方法具有的派生参数类型少于委托类型中的类型。  
  
## <a name="example-1-covariance"></a>示例 1：协变  
  
### <a name="description"></a>描述  

 本示例演示如何将委托与具有返回类型的方法一起使用，这些返回类型派生自委托签名中的返回类型。 `DogsHandler` 返回的数据类型属于 `Dogs` 类型，它派生自委托中定义的 `Mammals` 类型。  
  
### <a name="code"></a>代码  
  
```csharp  
class Mammals {}  
class Dogs : Mammals {}  
  
class Program  
{  
    // Define the delegate.  
    public delegate Mammals HandlerMethod();  
  
    public static Mammals MammalsHandler()  
    {  
        return null;  
    }  
  
    public static Dogs DogsHandler()  
    {  
        return null;  
    }  
  
    static void Test()  
    {  
        HandlerMethod handlerMammals = MammalsHandler;  
  
        // Covariance enables this assignment.  
        HandlerMethod handlerDogs = DogsHandler;  
    }  
}  
```  
  
## <a name="example-2-contravariance"></a>示例 2：逆变  
  
### <a name="description"></a>描述

本示例演示如何将委托与具有参数的方法一起使用，这些参数的类型是委托签名参数类型的基类型。 通过逆变可以使用一个事件处理程序而不是多个单独的处理程序。 下面的示例使用两个委托：

- 定义 [Button.KeyDown](xref:System.Windows.Forms.Control.KeyDown) 事件签名的 <xref:System.Windows.Forms.KeyEventHandler> 委托。 其签名为：

   ```csharp
   public delegate void KeyEventHandler(object sender, KeyEventArgs e)
   ```

- 定义 [Button.MouseClick](xref:System.Windows.Forms.Control.MouseDown) 事件签名的 <xref:System.Windows.Forms.MouseEventHandler> 委托。 其签名为：

   ```csharp
   public delegate void MouseEventHandler(object sender, MouseEventArgs e)
   ```

该示例定义了一个具有 <xref:System.EventArgs> 参数的事件处理程序，并使用它来处理 `Button.KeyDown` 和 `Button.MouseClick` 事件。 它可以这样做是因为 <xref:System.EventArgs> 是 <xref:System.Windows.Forms.KeyEventArgs> 和 <xref:System.Windows.Forms.MouseEventArgs> 的基类型。
  
### <a name="code"></a>代码  
  
```csharp  
// Event handler that accepts a parameter of the EventArgs type.  
private void MultiHandler(object sender, System.EventArgs e)  
{  
    label1.Text = System.DateTime.Now.ToString();  
}  
  
public Form1()  
{  
    InitializeComponent();  
  
    // You can use a method that has an EventArgs parameter,  
    // although the event expects the KeyEventArgs parameter.  
    this.button1.KeyDown += this.MultiHandler;  
  
    // You can use the same method
    // for an event that expects the MouseEventArgs parameter.  
    this.button1.MouseClick += this.MultiHandler;  
  
}  
```  
  
## <a name="see-also"></a>请参阅

- [委托中的变体 (C#)](./variance-in-delegates.md)
- [对 Func 和 Action 泛型委托使用变体 (C#)](./using-variance-for-func-and-action-generic-delegates.md)
