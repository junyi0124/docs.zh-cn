---
title: 缓解：跨应用程序域的对象的反序列化
description: 了解如何诊断和缓解尝试跨应用域反序列化逻辑调用上下文中的对象时引发异常的问题。
ms.date: 03/30/2017
ms.assetid: 30c2d66c-04a8-41a5-ad31-646b937f61b5
ms.openlocfilehash: 1b8060870962ddd26d90c4152a270a65936c2af3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96256633"
---
# <a name="mitigation-deserialization-of-objects-across-app-domains"></a>缓解：跨应用程序域的对象反序列化

有时，当一个应用程序使用具有不同应用程序基的两个或多个应用程序域时，如果尝试跨应用程序域在逻辑调用上下文中反序列化对象，则会引发异常。  
  
## <a name="diagnosing-the-issue"></a>诊断问题  

 以下情况将会引发这一问题：  
  
1. 一个应用程序使用具有不同应用程序基的两个或多个应用程序域。  
  
2. 通过调用 <xref:System.Runtime.Remoting.Messaging.LogicalCallContext> 或 <xref:System.Runtime.Remoting.Messaging.LogicalCallContext.SetData%2A?displayProperty=nameWithType> 等方法将某些类型明确添加到 <xref:System.Runtime.Remoting.Messaging.CallContext.LogicalSetData%2A?displayProperty=nameWithType>。 这些类型未标记为可序列化并且未存储在全局程序集缓存中。  
  
3. 之后，在非默认应用程序域中运行的代码尝试从配置文件中读取值，或者使用 XML 来反序列化对象。  
  
4. 为了从配置文件中读取值或反序列化对象，<xref:System.Xml.XmlReader> 对象尝试访问配置系统。  
  
5. 如果配置系统尚未初始化，则必须完成其初始化。 这意味着，运行时环境还必须为配置系统创建稳定的路径，它的作用如下：  
  
    1. 查找非默认应用程序域的证据。  
  
    2. 尝试根据默认应用程序域计算非默认应用程序域的证据。  
  
    3. 为获取默认应用程序域证据而进行的调用，会触发从非默认应用程序域到默认应用程序域的跨应用程序域调用。  
  
    4. 作为 .NET Framework 中跨应用程序域协定的一部分，逻辑调用上下文的内容必须跨应用程序域边界进行封送。  
  
6. 由于逻辑调用上下文中的类型不能在默认应用程序域中解析，因此将会引发异常。  
  
## <a name="mitigation"></a>缓解  

 若要解决此问题，请执行以下操作：  
  
1. 当引发异常时，查找调用堆栈中对 `get_Evidence` 的调用。 这个异常可以是异常集中的任何一个大子集，包括 <xref:System.IO.FileNotFoundException> 和 <xref:System.Runtime.Serialization.SerializationException>。  
  
2. 识别应用程序中没有向逻辑调用上下文添加应用程序的位置，然后添加以下代码：  
  
    ```csharp
    System.Configuration.ConfigurationManager.GetSection("system.xml/xmlReader");  
    ```
  
## <a name="see-also"></a>请参阅

- [应用程序兼容性](application-compatibility.md)
