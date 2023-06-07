---
title: 创建和使用组件
ms.date: 07/20/2015
helpviewer_keywords:
- components [Visual Basic]
ms.assetid: ee6a4156-73f7-4e9b-8e01-c74c4798b65c
ms.openlocfilehash: 106b8791ee5cb3db95759ccca2fddd799661ef3c
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93282059"
---
# <a name="creating-and-using-components-in-visual-basic"></a>创建和使用组件 (Visual Basic)

组件  是一个类，该类实现 <xref:System.ComponentModel.IComponent?displayProperty=nameWithType> 接口或直接/间接派生自实现 <xref:System.ComponentModel.IComponent> 的类。 NET 组件是可重复使用的对象，可以和其他对象进行交互，并提供对外部资源和设计时支持的控制。  
  
 组件的一个重要特性在于它们是可设计的，这意味着可在 Visual Studio 集成开发环境中使用作为组件的类。 可以将组件添加到“工具箱”、拖放到窗体以及在设计图面上操作。 对组件的基本设计时支持已内置于 .NET 中。 组件开发人员不必执行任何附加工作便可利用基本设计时功能。  
  
 控件与组件类似，二者都是可设计的。  不过，控件提供用户界面，而组件不提供。 控件必须派生自基控件类之一：<xref:System.Windows.Forms.Control> 或 <xref:System.Web.UI.Control>。  
  
## <a name="when-to-create-a-component"></a>创建组件的时间  

 如果类将在设计图面（如 Windows 窗体设计器或 Web 窗体设计器）上使用，但没有用户界面，此类应该是一个组件并实现 <xref:System.ComponentModel.IComponent>，或者是从直接或间接实现 <xref:System.ComponentModel.IComponent> 的类派生的。  
  
 <xref:System.ComponentModel.Component> 和 <xref:System.ComponentModel.MarshalByValueComponent> 类是 <xref:System.ComponentModel.IComponent> 接口的基实现。 这些类之间的主要区别在于 <xref:System.ComponentModel.Component> 类由引用封送，而 <xref:System.ComponentModel.IComponent> 由值封送。 以下列表为实施者提供了全面的指南。  
  
- 如果组件需要由引用封送，请从 <xref:System.ComponentModel.Component> 派生。  
  
- 如果组件需要由值封送，请从 <xref:System.ComponentModel.MarshalByValueComponent> 派生。  
  
- 如果因单一继承导致无法从其中一个基实现派生组件，则请实现 <xref:System.ComponentModel.IComponent>。  
  
## <a name="component-classes"></a>组件类  

 <xref:System.ComponentModel> 命名空间提供用于实现组件和控件的运行时和设计时行为的类。 此命名空间包括用于特性和类型转换器的实现、数据源绑定和组件授权的基类和接口。  
  
 核心组件类包括：  
  
- <xref:System.ComponentModel.Component>. <xref:System.ComponentModel.IComponent> 接口的基实现。 此类可以实现在应用程序之间共享对象。  
  
- <xref:System.ComponentModel.MarshalByValueComponent>. <xref:System.ComponentModel.IComponent> 接口的基实现。  
  
- <xref:System.ComponentModel.Container>. <xref:System.ComponentModel.IContainer> 接口的基实现。 此类封装零个或多个组件。  
  
 部分用于组件授权的类包括：  
  
- <xref:System.ComponentModel.License>. 所有许可证的抽象基类。 对组件的特定实例授予许可证。  
  
- <xref:System.ComponentModel.LicenseManager>. 提供属性和方法，用以将许可证添加到组件和管理 <xref:System.ComponentModel.LicenseProvider>。  
  
- <xref:System.ComponentModel.LicenseProvider>. 实现许可证提供程序的抽象基类。  
  
- <xref:System.ComponentModel.LicenseProviderAttribute>. 指定要与某个类一起使用的 <xref:System.ComponentModel.LicenseProvider> 类。  
  
 常用于描述和保存组件的类。  
  
- <xref:System.ComponentModel.TypeDescriptor>. 提供有关组件特征的信息，如组件的特性、属性和事件。  
  
- <xref:System.ComponentModel.EventDescriptor>. 提供有关事件的信息。  
  
- <xref:System.ComponentModel.PropertyDescriptor>. 提供有关属性的信息。  
  
## <a name="related-sections"></a>相关章节  

 [控件和组件创作疑难解答](/dotnet/desktop/winforms/controls/troubleshooting-control-and-component-authoring)  
 解释如何解决常见问题。  
  
## <a name="see-also"></a>请参阅

- [如何：在 Windows 窗体中访问设计时支持](/dotnet/desktop/winforms/controls/developing-windows-forms-controls-at-design-time)
