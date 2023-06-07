---
title: 示例：动态编程疑难解答
ms.date: 03/30/2017
ms.assetid: 42ed860a-a022-4682-8b7f-7c9870784671
ms.openlocfilehash: 0cff232668b9eb65b09a22b14e4ae58673ccd6d0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288068"
---
# <a name="example-troubleshooting-dynamic-programming"></a>示例：动态编程疑难解答

> [!NOTE]
> 该主题是指 .NET Native 开发者预览版这款预发布软件。 可从 [Microsoft Connect 网站](https://go.microsoft.com/fwlink/?LinkId=394611)（需要注册）下载该预览版。  
  
 并非所有使用 .NET Native 工具链开发的应用中的元数据查找失败都会导致异常。  有些可能会在一个应用中以不可预知的方式显示。  以下实例展示了由引用一个空对象造成的访问冲突：  
  
```output
Access violation - code c0000005 (first chance)  
App!$3_App::Core::Util::NavigationArgs.Setup  
App!$3_App::Core::Util::NavigationArgs..ctor  
App!$0_App::Gibbon::Util::DesktopNavigationArgs..ctor  
App!$0_App::ViewModels::DesktopAppVM.NavigateToPage  
App!$3_App::Core::ViewModels::AppViewModel.NavigateToFirstPage  
App!$3_App::Core::ViewModels::AppViewModel::<HandleLaunch>d__a.MoveNext  
App!$43_System::Runtime::CompilerServices::AsyncMethodBuilderCore.CallMoveNext  
App!System::Action.InvokeClosedStaticThunk  
App!System::Action.Invoke  
App!$43_System::Threading::Tasks::AwaitTaskContinuation.InvokeAction  
App!$43_System::Threading::SendOrPostCallback.InvokeOpenStaticThunk  
[snip]  
```  
  
 我们可尝试通过使用[入门](getting-started-with-net-native.md)的“手动解析丢失的元数据”一节中列出的三步方法来解决这个异常。  
  
## <a name="what-was-the-app-doing"></a>该应用在执行什么操作？  

 第一件要提起注意的是堆栈基部的 `async` 关键字机械设备。  要确定该设备过去正在一个 `async` 方法中实际上在执行什么操作可能会很困难，因为堆栈已经丢失了始发调用的上下文并且已经在另一个不同的线程上运行了 `async` 代码。 然而，我们可以推断出该应用正在试图加载其首页。  在 `NavigationArgs.Setup` 的实现过程中，以下代码引起了访问冲突：  
  
`AppViewModel.Current.LayoutVM.PageMap`  
  
 在此实例中，`AppViewModel.Current` 上的 `LayoutVM` 属性是 null。  某些元数据的缺失引起微妙的行为差异并导致一个属性（而不是集）未以该应用预设的方式得到初始化。  在 `LayoutVM` 应该得到初始化的代码中设置一个断点可能会缓解这一情况。  然而，请注意 `LayoutVM` 的类型是 `App.Core.ViewModels.Layout.LayoutApplicationVM`。  目前 rd.xml 文件中存在的唯一元数据指令是：  
  
```xml  
<Namespace Name="App.ViewModels" Browse="Required Public" Dynamic="Required Public" />  
```  
  
 引起失败的可能原因是 `App.Core.ViewModels.Layout.LayoutApplicationVM` 是丢失的元数据，原因在于它位于一个不同的命名空间。  
  
 在这种情况下，添加一个运行时指令让 `App.Core.ViewModels` 解决这一问题。 根本原因在于 API 调用了返回 null 的 <xref:System.Type.GetType%28System.String%29?displayProperty=nameWithType> 方法，且该应用默认忽略这一问题直到发生故障。  
  
 在动态编程中，在 .NET Native 下使用反射 Api 是一种很好的做法，就是使用 <xref:System.Type.GetType%2A?displayProperty=nameWithType> 在失败时引发异常的重载。  
  
## <a name="is-this-an-isolated-case"></a>这是一个孤立情形吗？  

 当使用 `App.Core.ViewModels` 时，可能也会出现其他问题。  你必须决定是否值得确定并修复每个丢失的元数据异常，或节省时间并为类型的一个更大类添加指令。  此处，为 `dynamic` 添加 `App.Core.ViewModels` 元数据可能最好的方法，前提是输出的二进制代码变大不会产生问题。  
  
## <a name="could-the-code-be-rewritten"></a>代码能够重写吗？  

 如果该应用过去使用的是 `typeof(LayoutApplicationVM)` 而不是 `Type.GetType("LayoutApplicationVM")`，工具链可能已经保存了 `browse` 元数据。  然而，它也可能没有创建 `invoke` 元数据，这在实例化该类型时可能会导致 [MissingMetadataException](missingmetadataexception-class-net-native.md) 异常。 要阻止这一异常，你仍然必须为命名空间添加一个运行时指令或指定 `dynamic` 策略的类型。 有关运行时指令的信息，请参阅 [运行时指令 (rd.xml) 配置文件参考](runtime-directives-rd-xml-configuration-file-reference.md)。  
  
## <a name="see-also"></a>另请参阅

- [入门](getting-started-with-net-native.md)
- [示例：处理绑定数据时出现的异常](example-handling-exceptions-when-binding-data.md)
