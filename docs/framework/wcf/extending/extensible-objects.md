---
title: 可扩展对象
ms.date: 03/30/2017
helpviewer_keywords:
- extensible objects [WCF]
ms.assetid: bc88cefc-31fb-428e-9447-6d20a7d452af
ms.openlocfilehash: 682c391e7b3c68de5bf799f77a93df7539681a37
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64654480"
---
# <a name="extensible-objects"></a>可扩展对象

可扩展对象模式用于使用新功能扩展现有运行时类，或者向对象中添加新状态。 附加到可扩展对象之一的扩展名，在访问附加到公共可扩展对象的共享状态和功能过程的各个不同阶段启用行为，各可扩展对象可以访问该公共扩展对象。

## <a name="the-iextensibleobjectt-pattern"></a>IExtensibleObject\<T > 模式

可扩展对象模式中有三个接口：<xref:System.ServiceModel.IExtensibleObject%601>、<xref:System.ServiceModel.IExtension%601> 和 <xref:System.ServiceModel.IExtensionCollection%601>。

<xref:System.ServiceModel.IExtensibleObject%601> 接口通过允许 <xref:System.ServiceModel.IExtension%601> 对象自定义类型的功能的这些类型实现。

可扩展的对象允许动态聚合 <xref:System.ServiceModel.IExtension%601> 对象。 以下接口确定 <xref:System.ServiceModel.IExtension%601> 对象的特点：

```csharp
public interface IExtension<T>
where T : IExtensibleObject<T>
{
    void Attach(T owner);
    void Detach(T owner);
}
```

类型限制保证仅为是 <xref:System.ServiceModel.IExtensibleObject%601> 的类定义扩展名。 <xref:System.ServiceModel.IExtension%601.Attach%2A> 和 <xref:System.ServiceModel.IExtension%601.Detach%2A> 提供聚合或解聚的通知。

对于实现限制它们何时可能会添加到所有者中或从所有者中移除有效。 例如，可以完全禁止移除（这将禁止在所有者或扩展名处于某个特定状态时添加或移除扩展名）、禁止同时添加多个所有者，或者仅允许单个移除后进行单个添加。

<xref:System.ServiceModel.IExtension%601> 不表示与其他标准托管接口进行的任何交互。 具体地说，所有者对象上的 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 方法通常不与它的扩展名分离。

将扩展添加到集合中，当<xref:System.ServiceModel.IExtension%601.Attach%2A>，它将进入集合前调用。 从集合中，删除扩展时<xref:System.ServiceModel.IExtension%601.Detach%2A>它移除后调用。 这意味着 （假设适当同步） 扩展可以依靠于仅当在集合中找到是之间<xref:System.ServiceModel.IExtension%601.Attach%2A>和<xref:System.ServiceModel.IExtension%601.Detach%2A>。

传递给 <xref:System.ServiceModel.IExtensionCollection%601.FindAll%2A> 或 <xref:System.ServiceModel.IExtensionCollection%601.Find%2A> 的对象不需要是 <xref:System.ServiceModel.IExtension%601>（例如，可以传递任何对象），但返回的扩展名是 <xref:System.ServiceModel.IExtension%601>。

如果在集合中的任何扩展，不则<xref:System.ServiceModel.IExtension%601>，<xref:System.ServiceModel.IExtensionCollection%601.Find%2A>返回 null，和<xref:System.ServiceModel.IExtensionCollection%601.FindAll%2A>返回一个空集合。 如果多个扩展名实现<xref:System.ServiceModel.IExtension%601>，<xref:System.ServiceModel.IExtensionCollection%601.Find%2A>返回其中一个。 从 <xref:System.ServiceModel.IExtensionCollection%601.FindAll%2A> 中返回的值是快照。

有两种主要方案。 第一个方案使用 <xref:System.ServiceModel.IExtensibleObject%601.Extensions%2A> 属性作为基于类型的字典插入对象状态，以使另一组件使用该类型进行查找。

第二个方案使用 <xref:System.ServiceModel.IExtension%601.Attach%2A> 和 <xref:System.ServiceModel.IExtension%601.Detach%2A> 属性使对象可以参与自定义行为，例如注册事件、监视状态转换等。

<xref:System.ServiceModel.IExtensionCollection%601> 接口是允许按照其类型检索 <xref:System.ServiceModel.IExtension%601> 的 <xref:System.ServiceModel.IExtension%601> 对象集合。 <xref:System.ServiceModel.IExtensionCollection%601.Find%2A?displayProperty=nameWithType> 返回最近添加的对象，此对象是属于该类型的 <xref:System.ServiceModel.IExtension%601>。

### <a name="extensible-objects-in-windows-communication-foundation"></a>Windows Communication Foundation 中的可扩展对象

有四个可扩展对象在 Windows Communication Foundation (WCF):

- <xref:System.ServiceModel.ServiceHostBase> — 这是服务主机的基类。  此类的扩展名可用于扩展 <xref:System.ServiceModel.ServiceHostBase> 自身的行为，或者用于存储每个服务的状态。

- <xref:System.ServiceModel.InstanceContext> — 此类将服务类型的实例与服务运行时相连。  它包含有关该实例的信息以及对 <xref:System.ServiceModel.InstanceContext> 的包含 <xref:System.ServiceModel.ServiceHostBase> 的引用。 此类的扩展名可用于扩展 <xref:System.ServiceModel.InstanceContext> 的行为，或者用于存储每个服务的状态。

- <xref:System.ServiceModel.OperationContext> — 此类表示为每个操作收集的运行时的操作信息。  这包括的信息有：传入消息头、传入消息属性和传入安全标识，以及其他信息。  此类的扩展名可以扩展 <xref:System.ServiceModel.OperationContext> 的行为，也可以存储每个操作的状态。

- <xref:System.ServiceModel.IContextChannel> — 此接口允检查的每个状态的通道和由 WCF 运行时生成的代理。  此类的扩展名可以扩展 <xref:System.ServiceModel.IClientChannel> 的行为，也可以使用它存储每个通道的状态。

下面的代码示例演示使用简单的扩展名来跟踪 <xref:System.ServiceModel.InstanceContext> 对象。

[!code-csharp[IInstanceContextInitializer#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/iinstancecontextinitializer/cs/initializer.cs#1)]

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.IExtensibleObject%601>
- <xref:System.ServiceModel.IExtension%601>
- <xref:System.ServiceModel.IExtensionCollection%601>
