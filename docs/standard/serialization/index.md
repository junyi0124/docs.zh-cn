---
title: 序列化 - .NET
description: 本文介绍有关 .NET 序列化技术的信息，包括二进制序列化、XML 和 SOAP 序列化以及 JSON 序列化。
ms.date: 09/02/2019
helpviewer_keywords:
- JSON serialization
- XML serialization, defined
- binary serialization
- serializing objects
- serialization
- objects, serializing
ms.assetid: 4d1111c0-9447-4231-a997-96a2b74b3453
ms.openlocfilehash: b3d76c14dc9180a5f19781122d1a42bcae603e76
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "85840270"
---
# <a name="serialization-in-net"></a>.NET 中的序列化

序列化是将对象状态转换为可保持或传输的形式的过程。 序列化的补集是反序列化，后者将流转换为对象。 这两个过程一起保证能够存储和传输数据。  
  
.NET 具有以下序列化技术：  
  
- [二进制序列化](binary-serialization.md)保持类型保真，这对于多次调用应用程序时保持对象状态非常有用。 例如，通过将对象序列化到剪贴板，可在不同的应用程序之间共享对象。 您可以将对象序列化到流、磁盘、内存和网络等。 远程处理使用序列化，“按值”在计算机或应用程序域之间传递对象。  
  
- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)只序列化公共属性和字段，并且不保持类型保真。 当您希望提供或使用数据而不限制使用该数据的应用程序时，这一点非常有用。 由于 XML 是开放式的标准，因此它对于通过 Web 共享数据来说是一个理想选择。 SOAP 同样是开放式的标准，这使它也成为一个理想选择。  
  
- [JSON 序列化](system-text-json-overview.md)只序列化公共属性，并且不保持类型保真。 JSON 是开放式的标准，对于通过 Web 共享数据来说是一个理想选择。

## <a name="reference"></a>参考

<xref:System.Runtime.Serialization>  
包含可用于序列化和反序列化对象的类。
  
<xref:System.Xml.Serialization>  
包含可用于将对象序列化为 XML 格式的文档或流的类。

<xref:System.Text.Json>  
包含可用于将对象序列化为 JSON 格式的文档或流的类。
