---
title: 示例：处理绑定数据时出现的异常
ms.date: 03/30/2017
ms.assetid: bd63ed96-9853-46dc-ade5-7bd1b0f39110
ms.openlocfilehash: 399bd1af9ef25eca9cdfe1e13fdc4c01021babcd
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251069"
---
# <a name="example-handling-exceptions-when-binding-data"></a>示例：处理绑定数据时出现的异常

> [!NOTE]
> 该主题是指 .NET Native 开发者预览版这款预发布软件。 可从 [Microsoft Connect 网站](https://go.microsoft.com/fwlink/?LinkId=394611)（需要注册）下载该预览版。  
  
 下面的示例演示如何解决在使用 .NET Native 工具链编译的应用尝试绑定数据时引发的 [MissingMetadataException](missingmetadataexception-class-net-native.md) 异常。 以下是有关异常的信息：  
  
```output
This operation cannot be carried out as metadata for the following type was removed for performance reasons:
App.ViewModels.MainPageVM  
```  
  
 以下是相关的调用堆栈：  
  
```output
Reflection::Execution::ReflectionDomainSetupImplementation.CreateNonInvokabilityException+0x238  
Reflection::Core::ReflectionDomain.CreateNonInvokabilityException+0x2e  
Reflection::Core::Execution::ExecutionEnvironment.+0x316  
System::Reflection::Runtime::PropertyInfos::RuntimePropertyInfo.GetValue+0x1cb  
System::Reflection::PropertyInfo.GetValue+0x22  
System::Runtime::InteropServices::WindowsRuntime::CustomPropertyImpl.GetValue+0x42  
App!$66_Interop::McgNative.Func_IInspectable_IInspectable+0x158  
App!$66_Interop::McgNative::__vtable_Windows_UI_Xaml_Data__ICustomProperty.GetValue__STUB+0x46  
Windows_UI_Xaml!DirectUI::PropertyProviderPropertyAccess::GetValue+0x3f
Windows_UI_Xaml!DirectUI::PropertyAccessPathStep::GetValue+0x31
Windows_UI_Xaml!DirectUI::PropertyPathListener::ConnectPathStep+0x113  
```  
  
## <a name="what-was-the-app-doing"></a>该应用在执行什么操作？  

 在堆栈的基础上，命名空间中的帧 <xref:Windows.UI.Xaml?displayProperty=nameWithType> 指示 XAML 呈现引擎正在运行。   对 <xref:System.Reflection.PropertyInfo.GetValue%2A?displayProperty=nameWithType> 方法的显示了属性值的基于反射的查找，该值位于元数据遭到删除的类型上。  
  
 第一步是提供一个元数据指令，将其添加到该类型的 `serialize` 元数据，使其所有属性都可访问：  
  
```xml  
<Type Name="App.ViewModels.MainPageVM" Serialize="Required Public" />  
```  
  
## <a name="is-this-an-isolated-case"></a>这是一个孤立情形吗？  

 在此情况下，如果数据绑定拥有一个 `ViewModel` 的不完整元数据，它对于其他模型可能也是如此。  如果代码的结构方式使得该应用的查看模型都位于 `App.ViewModels` 命名空间，你可以使用一个更一般的运行时指令：  
  
```xml  
<Namespace Name="App.ViewModels " Serialize="Required Public" />  
```  
  
## <a name="could-the-code-be-rewritten-to-not-use-reflection"></a>代码能够重新，改为不使用反射吗？  

 因为数据绑定是反射密集型的，更改代码以避免反射是不可行的。  
  
 然而，有几种方法可以指定 `ViewModel` 到 XAML 页面，从而让工具链在汇编时间可以将属性绑定与正确的类型关联起来并保存元数据，而不必使用运行时指令。  例如，可以 <xref:Windows.UI.Xaml.Data.BindableAttribute?displayProperty=nameWithType> 在属性上应用特性。 这会使得 XAML 编译器生成所需的查找信息并避免在 Default.rd.xml 文件中要求一个运行时指令。  
  
## <a name="see-also"></a>另请参阅

- [入门](getting-started-with-net-native.md)
- [示例：动态编程疑难解答](example-troubleshooting-dynamic-programming.md)
