---
title: XmlReader.createSqlReader 方法（系统.Xml）
ms.date: 10/17/2019
topic_type:
- apiref
api_name:
- System.Xml.XmlReader.CreateSqlReader
api_location:
- system.xml.dll
api_type:
- Assembly
ms.openlocfilehash: 7bd2ef5158516acede47f73f9937d06159bc16c9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79155734"
---
# <a name="xmlreadercreatesqlreader-method"></a>XmlReader.CreateSqlReader 方法

使用指定的流、设置和用于分析的上下文信息创建一个新的 <xref:System.Xml.XmlReader> 实例。

```csharp
internal static XmlReader CreateSqlReader(Stream input,
  XmlReaderSettings settings, XmlParserContext inputContext)
```

## <a name="parameters"></a>parameters

- `input` <xref:System.IO.Stream>  
  包含 XML 数据的流。

- `settings` <xref:System.Xml.XmlReaderSettings>  
  新 <xref:System.Xml.XmlReader> 实例的设置。 此值可以为 `null`。

- `inputContext` <xref:System.Xml.XmlParserContext>  
  分析 XML 片段所需的上下文信息. 此值可以为 `null`。

## <a name="returns"></a>返回

<xref:System.Xml.XmlReader>  
一个用于读取数据流中所含数据的对象。

## <a name="remarks"></a>备注

> [!WARNING]
> 该方法`XmlReader.CreateSqlReader`是内部的，不用于直接在代码中使用。
>
> 在任何情况下，Microsoft 都不支持在生产应用程序中使用此方法。

## <a name="requirements"></a>要求

**命名空间：**<xref:System.Xml>

**装配：** 系统.Xml.dll

**.NET 框架版本：** 自 2.0 起可用。
