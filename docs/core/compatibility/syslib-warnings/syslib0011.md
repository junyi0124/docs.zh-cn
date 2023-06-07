---
title: SYSLIB0011 警告
description: 了解有关生成编译时警告 SYSLIB0011 的过时信息。
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 36292cc5314e2b7677d705780880b7e25ae0dfb6
ms.sourcegitcommit: e301979e3049ce412d19b094c60ed95b316a8f8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97596283"
---
# <a name="syslib0011-binaryformatter-serialization-is-obsolete"></a>SYSLIB0011：BinaryFormatter 序列化已过时

由于 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 存在[安全漏洞](../../../standard/serialization/binaryformatter-security-guide.md#binaryformatter-security-vulnerabilities)，从 .NET 5.0 开始，以下 API 标记为已过时。 在代码中使用这些 API 会在编译时生成警告 `SYSLIB0011`。

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>

## <a name="workarounds"></a>工作区

请考虑使用 <xref:System.Text.Json.JsonSerializer> 或 <xref:System.Xml.Serialization.XmlSerializer>，而不是 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>。

若要详细了解建议的操作，请参阅[修复 BinaryFormatter 过时和禁用错误](https://aka.ms/binaryformatter)。

[!INCLUDE [suppress-syslib-warning](../../../../includes/suppress-syslib-warning.md)]

## <a name="see-also"></a>另请参阅

- [修复 BinaryFormatter 过时和禁用错误](https://aka.ms/binaryformatter)
- [BinaryFormatter 序列化方法已过时，并且已在 ASP.NET 应用中禁用](../core-libraries/5.0/binaryformatter-serialization-obsolete.md)
