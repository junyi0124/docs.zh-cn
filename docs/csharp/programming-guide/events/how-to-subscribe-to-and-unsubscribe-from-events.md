---
title: 如何订阅和取消订阅事件 - C# 编程指南
description: 了解如何订阅和取消订阅事件。 以编程方式使用 Visual Studio IDE 订阅事件，或使用匿名方法订阅。
ms.date: 07/20/2015
helpviewer_keywords:
- event handlers [C#], creating
- Code Editor, event handlers
- events [C#], creating using the IDE
ms.assetid: 6319f39f-282c-4173-8a62-6c4657cf51cd
ms.openlocfilehash: 1e090301982a785fed2a8a6a95ee48bd1c7457ab
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91167478"
---
# <a name="how-to-subscribe-to-and-unsubscribe-from-events-c-programming-guide"></a>如何订阅和取消订阅事件（C# 编程指南）

如果想编写引发事件时调用的自定义代码，则可以订阅由其他类发布的事件。 例如，可以订阅某个按钮的 `click` 事件，以使应用程序在用户单击该按钮时执行一些有用的操作。  
  
### <a name="to-subscribe-to-events-by-using-the-visual-studio-ide"></a>使用 Visual Studio IDE 订阅事件  
  
1. 如果看不到“属性”窗口，请在“设计”视图中，右键单击要为其创建事件处理程序的窗体或控件，然后选择“属性”  。  
  
2. 在“属性”窗口的顶部，单击“事件”图标 。  
  
3. 双击要创建的事件，例如 `Load` 事件。  
  
     Visual C# 会创建一个空事件处理程序方法，并将其添加到你的代码中。 或者，也可以在“代码”视图中手动添加代码。 例如，下面的代码行声明了一个在 `Form` 类引发 `Load` 事件时调用的事件处理程序方法。  
  
     [!code-csharp[csProgGuideEvents#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#11)]  
  
     还会在项目的 Form1.Designer.cs 文件的 `InitializeComponent` 方法中自动生成订阅该事件所需的代码行。 该代码行类似于：  
  
    ```csharp
    this.Load += new System.EventHandler(this.Form1_Load);  
    ```  
  
### <a name="to-subscribe-to-events-programmatically"></a>以编程方式订阅事件  
  
1. 定义一个事件处理程序方法，其签名与该事件的委托签名匹配。 例如，如果事件基于 <xref:System.EventHandler> 委托类型，则下面的代码表示方法存根：  
  
    ```csharp
    void HandleCustomEvent(object sender, CustomEventArgs a)  
    {  
       // Do something useful here.  
    }  
    ```  
  
2. 使用加法赋值运算符 (`+=`) 来为事件附加事件处理程序。 在下面的示例中，假设名为 `publisher` 的对象拥有一个名为 `RaiseCustomEvent` 的事件。 请注意，订户类需要引用发行者类才能订阅其事件。  
  
    ```csharp
    publisher.RaiseCustomEvent += HandleCustomEvent;  
    ```  
  
     请注意，前面的语法是 C# 2.0 中的新语法。 此语法完全等效于必须使用 `new` 关键字显式创建封装委托的 C# 1.0 语法：  
  
    ```csharp
    publisher.RaiseCustomEvent += new CustomEventHandler(HandleCustomEvent);  
    ```  
  
     还可以使用 [Lambda 表达式](../../language-reference/operators/lambda-expressions.md)来指定事件处理程序：
  
    ```csharp
    public Form1()  
    {  
        InitializeComponent();  
        this.Click += (s,e) =>
            {
                MessageBox.Show(((MouseEventArgs)e).Location.ToString());
            };
    }  
    ```  
  
### <a name="to-subscribe-to-events-by-using-an-anonymous-method"></a>使用匿名方法订阅事件  
  
- 如果以后不必取消订阅某个事件，则可以使用加法赋值运算符 (`+=`) 将匿名方法附加到此事件。 在下面的示例中，假设名为 `publisher` 的对象拥有一个名为 `RaiseCustomEvent` 的事件，并且还定义了一个 `CustomEventArgs` 类以承载某些类型的专用事件信息。 请注意，订户类需要引用 `publisher` 才能订阅其事件。  
  
    ```csharp
    publisher.RaiseCustomEvent += delegate(object o, CustomEventArgs e)  
    {  
      string s = o.ToString() + " " + e.ToString();  
      Console.WriteLine(s);  
    };  
    ```  
  
     请务必注意，如果使用匿名函数订阅事件，事件的取消订阅过程将比较麻烦。 这种情况下若要取消订阅，必须返回到该事件的订阅代码，将该匿名方法存储在委托变量中，然后将此委托添加到该事件中。 一般来说，如果必须在后面的代码中取消订阅某个事件，则建议不要使用匿名函数订阅此事件。 有关匿名函数的详细信息，请参阅[匿名函数](../statements-expressions-operators/anonymous-functions.md)。  
  
## <a name="unsubscribing"></a>取消订阅  

 若要防止在引发事件时调用事件处理程序，请取消订阅该事件。 为了防止资源泄露，应在释放订户对象之前取消订阅事件。 在取消订阅事件之前，在发布对象中作为该事件的基础的多播委托会引用封装了订户的事件处理程序的委托。 只要发布对象保持该引用，垃圾回收功能就不会删除订户对象。  
  
#### <a name="to-unsubscribe-from-an-event"></a>取消订阅事件  
  
- 使用减法赋值运算符 (`-=`) 取消订阅事件：  
  
    ```csharp
    publisher.RaiseCustomEvent -= HandleCustomEvent;  
    ```  
  
     所有订户都取消订阅事件后，发行者类中的事件实例将设置为 `null`。  
  
## <a name="see-also"></a>请参阅

- [事件](./index.md)
- [event](../../language-reference/keywords/event.md)
- [如何发布符合 .NET 准则的事件](./how-to-publish-events-that-conform-to-net-framework-guidelines.md)
- [- 和 -= 运算符](../../language-reference/operators/subtraction-operator.md)
- [+ 和 += 运算符](../../language-reference/operators/addition-operator.md)
