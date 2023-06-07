---
author: dotpaul
ms.author: paulming
ms.date: 05/01/2019
ms.topic: include
ms.openlocfilehash: cf944ae9ca200d34a1f0f32e68bf3aee73a9500a
ms.sourcegitcommit: c04535ad05e374fb269fcfc6509217755fbc0d54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "96590083"
---
- 如果可能，请使用安全的序列化程序，而 **不允许攻击者指定任意要反序列化的类型**。 一些更安全的序列化程序包括：

  - <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>
  - <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType>
  - <xref:System.Web.Script.Serialization.JavaScriptSerializer?displayProperty=nameWithType> -从不使用 <xref:System.Web.Script.Serialization.SimpleTypeResolver?displayProperty=nameWithType> 。 如果必须使用类型解析程序，请将反序列化类型限制为预期列表。
  - <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>
  - Newtonsoft.json Json.NET-使用 TypeNameHandling。 如果必须为 TypeNameHandling 使用其他值，请将反序列化类型限制为具有自定义 ISerializationBinder 的预期列表。
  - 协议缓冲区

- 使序列化的数据不会被篡改。 序列化后，对序列化的数据进行加密签名。 在反序列化之前，验证加密签名。 保护加密密钥不被泄露，并为密钥轮换设计。
