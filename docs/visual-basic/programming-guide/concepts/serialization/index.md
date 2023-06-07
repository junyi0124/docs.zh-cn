---
title: 序列化
ms.date: 07/20/2015
ms.assetid: 67379a76-5465-4af8-a781-0b0b25a62d9a
ms.openlocfilehash: 98400c9e7ed9e26a9dc6ca548988c3691bfecdc2
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086499"
---
# <a name="serialization-visual-basic"></a>序列化 (Visual Basic)

序列化是指将对象转换成字节流，从而存储对象或将对象传输到内存、数据库或文件的过程。 它的主要用途是保存对象的状态，以便能够在需要时重新创建对象。 反向过程称为“反序列化”。  
  
## <a name="how-serialization-works"></a>序列化的工作原理  

 下图展示了序列化的整个过程。  
  
![图：序列化](./media/index/serialization-process.gif)
  
 对象被序列化成流，其中不仅包含数据，还包含对象类型的相关信息，如版本、区域性和程序集名称。 可以将此流中的内容存储在数据库、文件或内存中。  
  
### <a name="uses-for-serialization"></a>序列化的用途  

 借助序列化，开发者可以保存对象的状态，并能在需要时重新创建对象，同时还能存储对象和交换数据。 通过序列化，开发者可以执行如下操作：通过 Web 服务将对象发送到远程应用程序、在域之间传递对象、以 XML 字符串的形式传递对象通过防火墙、跨应用程序维护安全性或用户专属信息。  
  
### <a name="making-an-object-serializable"></a>让对象可序列化  

 若要序列化对象，需要具有要序列化的对象、包含已序列化对象的一个流，以及一个 <xref:System.Runtime.Serialization.Formatter>。 <xref:System.Runtime.Serialization> 包含序列化和反序列化对象所必需的类。  
  
 将 <xref:System.SerializableAttribute> 特性应用于某个类型，以指示此类型的实例可以被序列化。 如果尝试对没有 <xref:System.SerializableAttribute> 特性的类型进行序列化，则会引发 <xref:System.Runtime.Serialization.SerializationException> 异常。  
  
 如果想让类中的某个字段不可序列化，请应用 <xref:System.NonSerializedAttribute> 特性。 如果可序列化的类型中的一个字段包含指针、句柄或特定环境专用的其他一些数据结构，且不能在其他环境中有意义地重构，不妨让其不可序列化。  
  
 如果已序列化的类引用被标记为 <xref:System.SerializableAttribute> 的其他类的对象，那么这些对象也会被序列化。  
  
## <a name="binary-and-xml-serialization"></a>二进制和 XML 序列化  

 二进制或 XML 序列化均可使用。 在二进制序列化中，所有成员（包括只读成员）都会被序列化，且性能也会有所提升。 XML 序列化可提高代码可读性，以及对象共享和使用的灵活性，从而实现互操作性。  
  
### <a name="binary-serialization"></a>二进制序列化  

 二进制序列化使用二进制编码来生成精简的序列化以供使用，如基于存储或套接字的网络流。  
  
### <a name="xml-serialization"></a>XML 序列化  

 XML 序列化将对象的公共字段和属性或方法的参数和返回值序列化成符合特定 XML 架构定义语言 (XSD) 文档要求的 XML 流。 XML 序列化生成已转换成 XML 的强类型类，其中包含公共属性和字段。 <xref:System.Xml.Serialization> 包含序列化和反序列化 XML 所必需的类。  
  
 可以将特性应用于类和类成员，从而控制 <xref:System.Xml.Serialization.XmlSerializer> 如何序列化或反序列化类的实例。  
  
## <a name="basic-and-custom-serialization"></a>基本和自定义序列化  

 序列化有两种执行方式：基本和自定义序列化。 基本序列化使用 .NET Framework 自动序列化对象。  
  
### <a name="basic-serialization"></a>基本序列化  

 基本序列化的唯一要求是对象必须已应用 <xref:System.SerializableAttribute> 特性。 <xref:System.NonSerializedAttribute> 可用于防止特定字段被序列化。  
  
 使用基本序列化时，对象的版本控制可能会导致问题出现，在这种情况下，最好使用自定义序列化。 基本序列化是最简单的序列化执行方式，但无法提供太多的进程控制。  
  
### <a name="custom-serialization"></a>自定义序列化  

 在自定义序列化中，可以精确指定要序列化的对象以及具体执行方式。 类必须被标记为 <xref:System.SerializableAttribute>，并实现 <xref:System.Runtime.Serialization.ISerializable> 接口。  
  
 如果还想按自定义方式反序列化对象，必须使用自定义构造函数。  
  
## <a name="designer-serialization"></a>设计器序列化  

 设计器序列化是一种特殊形式的序列化，涉及通常与开发工具相关联的对象暂留。 设计器序列化是指将对象图转换成源文件以供日后用于恢复对象图的过程。 源文件可以包含代码、标记或 SQL 表信息。  
  
## <a name="related-topics-and-examples"></a><a name="BKMK_RelatedTopics"></a>相关主题和示例  

 [演练：在 Visual Studio 中暂留对象 (Visual Basic)](walkthrough-persisting-an-object-in-visual-studio.md)  
 展示了如何使用序列化在实例之间暂留对象数据，以便可以存储值并在下次实例化对象时检索值。  
  
 [如何：读取 XML 文件中的对象数据 (Visual Basic)](how-to-read-object-data-from-an-xml-file.md)  
 介绍如何使用 <xref:System.Xml.Serialization.XmlSerializer> 类读取之前写入 XML 文件的对象数据。  
  
 [如何：将对象数据写入 XML 文件 (Visual Basic)](how-to-write-object-data-to-an-xml-file.md)  
 介绍如何使用 <xref:System.Xml.Serialization.XmlSerializer> 类从某个类将对象写入 XML 文件。
