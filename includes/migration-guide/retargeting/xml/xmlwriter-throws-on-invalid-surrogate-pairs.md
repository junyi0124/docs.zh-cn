---
ms.openlocfilehash: f87b70708398226516ab5eac32c24a9fc2c1106b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615616"
---
### <a name="xmlwriter-throws-on-invalid-surrogate-pairs"></a>XmlWriter 引发无效的代理项对

#### <a name="details"></a>详细信息

对于面向 .NET Framework 4.5.2 或以前的版本的应用程序，使用异常回退处理编写无效的代理项对并不会总是引发异常。 对于面向 .NET Framework 4.6 的应用，尝试编写无效的代理项对会引发 <xref:System.ArgumentException?displayProperty=fullName>。

#### <a name="suggestion"></a>建议

如有必要，可通过面向 .NET Framework 4.5.2 或更早版本来避免此中断。 或者，可在编写无效的代理项对前将其预处理为有效的 XML。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Xml.XmlWriter.WriteAttributeString(System.String,System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteAttributeString(System.String,System.String,System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteAttributeString(System.String,System.String,System.String,System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteAttributeStringAsync(System.String,System.String,System.String,System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteCData(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteCDataAsync(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteChars(System.Char[],System.Int32,System.Int32)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteCharsAsync(System.Char[],System.Int32,System.Int32)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteComment(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteCommentAsync(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteEntityRef(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteEntityRefAsync(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteRaw(System.Char[],System.Int32,System.Int32)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteProcessingInstruction(System.String,System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteProcessingInstructionAsync(System.String,System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteRaw(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteRawAsync(System.Char[],System.Int32,System.Int32)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteRawAsync(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteString(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteStringAsync(System.String)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteSurrogateCharEntity(System.Char,System.Char)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteSurrogateCharEntityAsync(System.Char,System.Char)?displayProperty=nameWithType>
- <xref:System.Xml.XmlWriter.WriteValue(System.String)?displayProperty=nameWithType>
