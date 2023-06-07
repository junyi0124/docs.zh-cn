---
title: MailAddressParser 类（System.Net）
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.Mail.MailAddressParser
- System.Net.Mail.MailAddressParser.ParseAddress
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: ff83f6946539fa262ccde980052627f98c75601d
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051345"
---
# <a name="mailaddressparser-class"></a>MailAddressParser 类

如 RFC 2822 中所述分析电子邮件地址。 无法继承此类。

```csharp
internal static class MailAddressParser
```

> [!WARNING]
> 此类是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="parseaddress-method"></a>ParseAddress 方法

分析指定字符串中的单个电子邮件地址。

```csharp
internal static MailAddress ParseAddress(string data)
```

### <a name="parameters"></a>参数

`data` <xref:System.String>

包含要分析的电子邮件地址的字符串。

### <a name="return-value"></a>返回值

<xref:System.Net.Mail.MailAddress>

有效的电子邮件地址。

### <a name="exceptions"></a>例外

<xref:System.FormatException?displayProperty=nameWithType>

电子邮件地址无效。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）
