---
title: WriteStartHeaders 方法（System.servicemodel. 通道）
ms.date: 11/01/2019
topic_type:
- apiref
api_name:
- System.ServiceModel.Channels.Message.WriteStartHeaders
api_location:
- system.servicemodel.dll
api_type:
- Assembly
ms.openlocfilehash: c826e6a3b976e5705e9815586441e8a25b64f76e
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2020
ms.locfileid: "77214866"
---
# <a name="messagewritestartheaders-method"></a>WriteStartHeaders 方法

通过调用 <xref:System.ServiceModel.Channels.Message.OnWriteStartHeaders%2A?displayProperty=nameWithType> 方法，将开始标头写入 XML 文件。

```csharp
internal void WriteStartHeaders(XmlDictionaryWriter writer)
```

## <a name="parameters"></a>parameters

- `writer` <xref:System.Xml.XmlDictionaryWriter>\
  用于将开始标头写入 XML 文件的编写器。

## <a name="remarks"></a>备注

> [!WARNING]
> `Message.WriteStartHeaders` 方法是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此方法。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.ServiceModel.Channels>

**程序集：** System.servicemodel .dll

**.NET Framework 版本：** 自3.0 起可用。
