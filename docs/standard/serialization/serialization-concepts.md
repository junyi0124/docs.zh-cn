---
title: 序列化概念
description: 序列化可用于捕获对象的状态，以便可以创建副本或按值将对象从一个应用程序域发送到另一个应用程序域。
ms.date: 08/07/2017
ms.assetid: e1ff4740-20a1-4c76-a8ad-d857db307054
ms.openlocfilehash: 90fd8be54e34f1eddd773a69df7ff72f111a58c6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722162"
---
# <a name="serialization-concepts"></a>序列化概念

为什么要使用序列化？ 两个最重要的原因是将对象状态保存到存储媒体，以便可以在以后阶段重新创建精确副本；以及将对象按值从一个应用程序域发送至另一个应用程序域。 例如，序列化用于在 ASP.NET 中保存会话状态，并将对象复制到 Windows 窗体的剪贴板中。 它还可用于在远程处理中将对象按值从一个应用程序域传递至另一个应用程序域。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

## <a name="persistent-storage"></a>永久性存储

经常有必要将对象的字段值存储至磁盘，以后再检索此数据。 尽管不依赖序列化也能很容易地实现这一点，但方法通常麻烦而容易出错，并且需要跟踪对象的层次结构时会逐渐变得更加复杂。 假设要编写一个包含数千个对象的大型商务应用程序，并且必须为每个对象编写代码，以便将字段和属性保存至磁盘以及从磁盘进行还原。 序列化为实现这一目标提供了方便的机制。

公共语言运行库可管理对象在内存中存储的方式，并通过使用[反射](../../framework/reflection-and-codedom/reflection.md)提供一种自动序列化机制。 当序列化对象时，类的名称、程序集和类实例的所有数据成员被写入存储区。 对象经常以成员变量方式将引用存储至其他实例。 当序列化类时，序列化引擎跟踪被引用的对象（已序列化），以确保同一对象不会被多次序列化。 .NET 提供的序列化体系结构可自动正确地处理对象图和循环引用。 对于对象图的唯一要求是，必须将已序列化对象引用的所有对象也标记为 `Serializable`（有关详细信息，请参阅[基本序列化](basic-serialization.md)）。 如果未进行标记，序列化程序尝试序列化未标记的对象时将引发异常。

反序列化已序列化的类时，将重新创建该类，并且将自动还原所有数据成员的值。

## <a name="marshal-by-value"></a>按值封送

对象仅在创建它们的应用程序域中有效。 除非对象从 `MarshalByRefObject` 派生或被标记为 `Serializable`，否则尝试将对象作为参数传递或者作为结果返回时都将失败。 如果将对象标记为 `Serializable`，则会自动序列化该对象，将该对象从一个应用程序域传输至另一个应用程序域，然后反序列化，以便在第二个应用程序域中生成该对象的一个精确副本。 此过程通常称为按值封送。

如果对象从 `MarshalByRefObject` 派生，则会将对象引用（而不是对象本身）从一个应用程序域传递至另一个应用程序域。 还可将派生自 `MarshalByRefObject` 的对象标记为 `Serializable`。 此对象用于远程处理时，负责反序列化的格式化程序已通过代理项选择器 (`SurrogateSelector`) 预配置，该格式化程序控制序列化过程，并用代理替换从 `MarshalByRefObject` 派生的所有对象。 如果本地没有 `SurrogateSelector`，则序列化体系结构遵循[序列化过程中的步骤](steps-in-the-serialization-process.md)中所述的标准序列化规则。  

## <a name="related-sections"></a>相关章节  

 [二进制序列化](binary-serialization.md)  
 描述随公共语言运行库一起提供的二进制序列化机制。
  
 [XML 和 SOAP 序列化](xml-and-soap-serialization.md)  
 描述随公共语言运行库一起提供的 XML 和 SOAP 序列化机制。
