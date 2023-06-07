---
title: 有选择的序列化
description: 本文介绍如何用 NonSerialized 属性标记字段，这会阻止字段被序列化。
ms.date: 08/07/2017
dev_langs:
- CSharp
helpviewer_keywords:
- serialization, selective serialization
- binary serialization, selective serialization
ms.assetid: 39c56635-95d2-4afd-aff1-b022e7649bb3
ms.openlocfilehash: 3c99a3be92beb992ff20188b02ff33a92f196baf
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722175"
---
# <a name="selective-serialization"></a>有选择的序列化

类通常包含不应进行序列化的字段。 例如，假定类将线程 ID 存储在成员变量中。 如果反序列化该类，而在对该类进行序列化时，存储了该 ID 的线程可能不再运行。因此，序列化该值毫无意义。 按如下方法使用 [NonSerialized](xref:System.NonSerializedAttribute) 属性标记成员变量，可防止其被序列化。  
  
```csharp  
[Serializable]  
public class MyObject
{  
  public int n1;  
  [NonSerialized] public int n2;  
  public String str;  
}  
```

如有可能，应使可能包含安全敏感数据的对象不可序列化。 如果必须序列化该对象，则可将 `NonSerialized` 属性应用于存储敏感数据的特定字段。 如果没有将这些字段排除在序列化之外，应该注意字段存储的数据会向有权序列化的所有代码公开。 有关编写安全的序列化代码的详细信息，请参阅[安全和序列化](../../framework/misc/security-and-serialization.md)。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]
  
## <a name="see-also"></a>请参阅

- [二进制序列化](binary-serialization.md)
- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
- [安全性和序列化](../../framework/misc/security-and-serialization.md)
