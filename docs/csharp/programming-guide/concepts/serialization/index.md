---
title: 序列化 (C#)
description: 序列化将对象转换成字节流，以存储对象或将对象传输到内存、数据库或文件。
ms.date: 01/02/2020
ms.openlocfilehash: 29625648b19c97556c107997ef9ecd3f0f971cbf
ms.sourcegitcommit: 7499bdb428d63ed0e19e97f54d3d576c41598659
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2020
ms.locfileid: "87455745"
---
# <a name="serialization-c"></a>序列化 (C#)

序列化是指将对象转换成字节流，从而存储对象或将对象传输到内存、数据库或文件的过程。 它的主要用途是保存对象的状态，以便能够在需要时重新创建对象。 反向过程称为“反序列化”。

## <a name="how-serialization-works"></a>序列化的工作原理

下图展示了序列化的整个过程：

![图：序列化](./media/index/serialization-process.gif)

将对象序列化为带有数据的流。 该流还可能包含有关对象类型的信息，例如其版本、区域性和程序集名称。 可以将此流中的对象存储在数据库、文件或内存中。

### <a name="uses-for-serialization"></a>序列化的用途

通过序列化，开发人员可以保存对象的状态，并能在需要时重新创建对象，同时还能存储对象和交换数据。 通过序列化，开发人员可以执行如下操作：

* 使用 Web 服务将对象发送到远程应用程序
* 将对象从一个域传递到另一个域
* 将对象通过防火墙传递为 JSON 或 XML 字符串
* 跨应用程序维护安全或用户特定的信息

## <a name="json-serialization"></a>JSON 序列化

<xref:System.Text.Json> 命名空间包含用于 JavaScript 对象表示法 (JSON) 序列化和反序列化的类。 JSON 是一种常用于在 Web 上共享数据的开放标准。

JSON 序列化将对象的公共属性序列化为符合 [RFC 8259 JSON 规范](https://tools.ietf.org/html/rfc8259)的字符串、字节数组或流。 若要控制 <xref:System.Text.Json.JsonSerializer> 对类的实例进行序列化或反序列化的方法，请执行以下操作：

* 使用 <xref:System.Text.Json.JsonSerializerOptions> 对象
* 将 <xref:System.Text.Json.Serialization> 命名空间中的特性应用于类或属性
* [实现自定义转换器](../../../../standard/serialization/system-text-json-converters-how-to.md)

## <a name="binary-and-xml-serialization"></a>二进制和 XML 序列化

<xref:System.Runtime.Serialization> 命名空间包含用于对二进制和 XML 进行序列化和反序列化的类。

二进制序列化使用二进制编码来生成精简的序列化以供使用，如基于存储或套接字的网络流。 在二进制序列化中，所有成员（包括只读成员）都会被序列化，且性能也会有所提升。

[!INCLUDE [binary-serialization-warning](~/includes/binary-serialization-warning.md)]

XML 序列化将对象的公共字段和属性或方法的参数和返回值序列化成符合特定 XML 架构定义语言 (XSD) 文档要求的 XML 流。 XML 序列化生成已转换成 XML 的强类型类，其中包含公共属性和字段。 <xref:System.Xml.Serialization> 包含用于对 XML 进行序列化和反序列化的类。 将特性应用于类和类成员，从而控制 <xref:System.Xml.Serialization.XmlSerializer> 如何序列化或反序列化类的实例。

### <a name="making-an-object-serializable"></a>让对象可序列化

若要对二进制或 XML 进行序列化，你需要：

* 要序列化的对象
* 包含序列化对象的流
* 一个 <xref:System.Runtime.Serialization.Formatter?displayProperty=fullName> 实例

将 <xref:System.SerializableAttribute> 特性应用于某个类型，以指示可对此类型进行序列化的实例。 如果尝试对没有 <xref:System.SerializableAttribute> 特性的类型进行序列化，则会引发异常。

若要防止对字段进行序列化，请应用 <xref:System.NonSerializedAttribute> 特性。 如果可序列化的类型中的一个字段包含指针、句柄或特定环境专用的其他一些数据结构，且不能在其他环境中有意义地重构，不妨让其不可序列化。

如果已序列化的类引用被标记为 <xref:System.SerializableAttribute> 的其他类的对象，那么这些对象也会被序列化。

### <a name="basic-and-custom-serialization"></a>基本和自定义序列化

可以使用两种方法对二进制和 XML 进行序列化：基本和自定义。

基本序列化使用 .NET 自动序列化对象。 唯一的要求是类应用 <xref:System.SerializableAttribute> 特性。 <xref:System.NonSerializedAttribute> 可用于防止特定字段被序列化。

使用基本序列化时，对象的版本控制可能会产生问题。 对于重要的版本控制问题，可以使用自定义序列化。 基本序列化是最简单的序列化执行方式，但无法提供太多的进程控制。

在自定义序列化中，可以精确指定要序列化的对象以及具体执行方式。 类必须被标记为 <xref:System.SerializableAttribute>，并实现 <xref:System.Runtime.Serialization.ISerializable> 接口。 如果还希望按自定义方式反序列化对象，请使用自定义构造函数。

## <a name="designer-serialization"></a>设计器序列化

设计器序列化是一种特殊形式的序列化，涉及与开发工具相关联的对象暂留。 设计器序列化是指将对象图转换成源文件以供日后用于恢复对象图的过程。 源文件可以包含代码、标记或 SQL 表信息。

## <a name="related-topics-and-examples"></a><a name="BKMK_RelatedTopics"></a>相关主题和示例  

[System.Text.Json 概述](../../../../standard/serialization/system-text-json-overview.md) 演示如何获取 `System.Text.Json` 库。

[如何在 .NET 中对 JSON 数据进行序列化和反序列化](../../../../standard/serialization/system-text-json-how-to.md)。
演示如何使用 <xref:System.Text.Json.JsonSerializer> 类在 JSON 之间读取和写入对象数据。

[演练：在 Visual Basic 中保持对象 (C#)](walkthrough-persisting-an-object-in-visual-studio.md)  
展示了如何使用序列化在实例之间暂留对象数据，以便可以存储值并在下次实例化对象时检索值。

[如何从 XML 文件读取对象数据 (C#)](how-to-read-object-data-from-an-xml-file.md)  
介绍如何使用 <xref:System.Xml.Serialization.XmlSerializer> 类读取之前写入 XML 文件的对象数据。

[如何将对象数据写入 XML 文件 (C#)](how-to-write-object-data-to-an-xml-file.md)  
介绍如何使用 <xref:System.Xml.Serialization.XmlSerializer> 类从某个类将对象写入 XML 文件。
