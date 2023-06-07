---
title: MailBnfHelper 类（System.Net）
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.Mime.MailBnfHelper
- System.Net.Mime.MailBnfHelper.Ascii7bitMaxValue
- System.Net.Mime.MailBnfHelper.Atext
- System.Net.Mime.MailBnfHelper.CR
- System.Net.Mime.MailBnfHelper.Ctext
- System.Net.Mime.MailBnfHelper.Dot
- System.Net.Mime.MailBnfHelper.EndComment
- System.Net.Mime.MailBnfHelper.LF
- System.Net.Mime.MailBnfHelper.Space
- System.Net.Mime.MailBnfHelper.StartComment
- System.Net.Mime.MailBnfHelper.Tab
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 86c98726a7886285917b6be8c7631ca1e9e425e6
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990436"
---
# <a name="mailbnfhelper-class"></a>MailBnfHelper 类

包含用于分析 internet 邮件格式字符串的实用工具方法。 无法继承此类。

```csharp
internal static class MailBnfHelper
```

> [!WARNING]
> 此类是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="ascii7bitmaxvalue-field"></a>Ascii7bitMaxValue 字段

表示最大7位 Ascii 值。

```csharp
internal static readonly int Ascii7bitMaxValue
```

## <a name="atext-field"></a>由于文本字段

表示原子中允许使用的字符。

```csharp
internal static bool[] Atext
```

## <a name="cr-field"></a>CR 字段

表示回车符。

```csharp
internal static readonly char CR
```

## <a name="ctext-field"></a>Ctext 字段

表示注释内允许使用的字符。

```csharp
internal static bool[] Ctext
```

## <a name="dot-field"></a>点字段

表示全停止字符（ `.` ）。

```csharp
internal static readonly char Dot
```

## <a name="endcomment-field"></a>EndComment 字段

表示指定注释末尾的字符。

```csharp
internal static readonly char EndComment
```

## <a name="lf-field"></a>LF 字段

表示换行符。

```csharp
internal static readonly char LF
```

## <a name="space-field"></a>空间字段

表示空格字符。

```csharp
internal static readonly char Space
```

## <a name="startcomment-field"></a>StartComment 字段

表示指定注释开头的字符。

```csharp
internal static readonly char StartComment
```

## <a name="tab-field"></a>选项卡字段

表示制表符。

```csharp
internal static readonly char Tab
```

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）
