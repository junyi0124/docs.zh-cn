---
title: XML 和 SOAP 序列化
description: 本概述讨论了 XML 序列化，其可用于将对象序列化为符合 SOAP 规范的 XML 流。
ms.date: 03/30/2017
helpviewer_keywords:
- SOAP, XML serialization
- XML serialization, SOAP
- serialization, SOAP
- serialization, about serialization
- XML serialization
- serialization
ms.assetid: 832ac524-21bc-419a-a27b-ca8bfc45840f
ms.openlocfilehash: f7ad7732f929ac3599942c5440b173ea226cca87
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90544981"
---
# <a name="xml-and-soap-serialization"></a>XML 和 SOAP 序列化

XML 序列化将对象的公共字段和属性以及方法的参数和返回值转换（序列化）为符合特定 XML 架构定义语言 (XSD) 文档的 XML 流。 XML 序列化会生成强类型类，同时将公共属性和字段转换为序列格式（在此情况下为 XML），以便存储或传输。

由于 XML 是开放式的标准，因此可以根据需要由任何应用程序处理 XML 流，而与平台无关。 例如，用 ASP.NET 创建的 XML Web services 使用 <xref:System.Xml.Serialization.XmlSerializer> 类来创建 XML 流，这些流在整个 Internet 中或在 Intranet 上的 XML Web services 应用程序之间传递数据。 相反，反序列化采用这样一个 XML 流并重新构造对象。

XML 序列化还可用于将对象序列化为符合 SOAP 规范的 XML 流。 SOAP 是一种基于 XML 的协议，它是专门为使用 XML 来传输过程调用而设计的。

若要序列化或反序列化对象，请使用 <xref:System.Xml.Serialization.XmlSerializer> 类。 要创建待序列化的类，请使用 XML 架构定义工具。

## <a name="see-also"></a>请参阅

- [二进制序列化](binary-serialization.md)
- [使用 ASP.NET 创建的 XML Web service 以及 XML Web Service 客户端](/previous-versions/dotnet/netframework-4.0/7bkzywba(v=vs.100))
