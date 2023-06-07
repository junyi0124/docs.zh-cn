---
ms.openlocfilehash: 557d811e2eeaf926cb958471d214fc23c50a25f2
ms.sourcegitcommit: c04535ad05e374fb269fcfc6509217755fbc0d54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "96590080"
---
- 改为使用安全序列化程序，并且 **不允许攻击者指定任意要反序列化的类型**。 有关详细信息，请参阅 [首选备选方案](/dotnet/standard/serialization/binaryformatter-security-guide#preferred-alternatives)。
- 使序列化的数据不会被篡改。 序列化后，对序列化的数据进行加密签名。 在反序列化之前，验证加密签名。 保护加密密钥不被泄露，并为密钥轮换设计。
- 此选项可使代码容易遭受拒绝服务攻击，以及将来可能会发生远程代码执行攻击。 有关详细信息，请参阅 [BinaryFormatter security guide （安全指南](/dotnet/standard/serialization/binaryformatter-security-guide)）。 限制反序列化的类型。 实现自定义 <xref:System.Runtime.Serialization.SerializationBinder?displayProperty=nameWithType> 。 在反序列化之前，请 `Binder` <xref:System.Runtime.Serialization.SerializationBinder> 在所有代码路径中将属性设置为自定义的实例。 在重写的 <xref:System.Runtime.Serialization.SerializationBinder.BindToType%2A> 方法中，如果类型意外，将引发异常以停止反序列化。
