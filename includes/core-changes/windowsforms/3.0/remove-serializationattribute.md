---
ms.openlocfilehash: 4fb1ffed97a5b7f906bed13a69f1e71563d11dae
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556129"
---
### <a name="serializableattribute-removed-from-some-windows-forms-types"></a>已从一些 Windows 窗体类型中删除 SerializableAttribute

已从一些没有已知二进制序列化方案的 Windows 窗体类中删除 <xref:System.SerializableAttribute>。

#### <a name="change-description"></a>更改描述

下列类型在 .NET Framework 中以 <xref:System.SerializableAttribute> 进行修饰，但该属性已在 .NET Core 中删除：

- `System.InvariantComparer`
- <xref:System.ComponentModel.Design.ExceptionCollection?displayProperty=nameWithType>
- <xref:System.ComponentModel.Design.Serialization.CodeDomSerializerException?displayProperty=nameWithType>
- `System.ComponentModel.Design.Serialization.CodeDomComponentSerializationService.CodeDomSerializationStore`
- <xref:System.Drawing.Design.ToolboxItem?displayProperty=nameWithType>
- `System.Resources.ResXNullRef`
- `System.Resources.ResXDataNode`
- `System.Resources.ResXFileRef`
- <xref:System.Windows.Forms.Cursor?displayProperty=nameWithType>
- `System.Windows.Forms.NativeMethods.MSOCRINFOSTRUCT`
- `System.Windows.Forms.NativeMethods.MSG`

从历史记录来看，此序列化机制存在严重的维护和安全性问题。 保证类型上持续具有 `SerializableAttribute` 意味着必须针对版本间的序列化更改以及可能出现的框架间序列化更改测试这些类型。 这使得很难发展这些类型且维护成本昂贵。 这些类型没有已知的二进制序列化方案，这在最大限度上消除了删除该属性带来的影响。

有关详细信息，请参阅[二进制序列化](~/docs/standard/serialization/binary-serialization.md)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

对于要标记为“可序列化”的类型，更新可能依赖它们的所有代码。

#### <a name="category"></a>类别

Windows 窗体

#### <a name="affected-apis"></a>受影响的 API

- 无

<!--

#### Affected APIs

- Not detectable via API analysis

-->
