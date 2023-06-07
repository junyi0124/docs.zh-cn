---
ms.openlocfilehash: 9c9c4ec55143ef991e9b69a389e0f3368263bb4e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614390"
---
### <a name="nullreferenceexception-in-exception-handling-code-from-imagesourceconverterconvertfrom"></a>ImageSourceConverter.ConvertFrom 异常处理代码中的 NullReferenceException

#### <a name="details"></a>详细信息

<xref:System.Windows.Media.ImageSourceConverter.ConvertFrom(System.ComponentModel.ITypeDescriptorContext,System.Globalization.CultureInfo,System.Object)> 的异常处理代码中的错误导致引发不正确的 <xref:System.NullReferenceException?displayProperty=fullName> 而不是预期的异常（<xref:System.IO.DirectoryNotFoundException?displayProperty=fullName> 或 <xref:System.IO.FileNotFoundException?displayProperty=fullName>）。 此更改更正了该错误，因此，该方法现在将引发正确的异常。 <p/>默认情况下，所有面向 .NET Framework 4.6.2 和更低版本的应用程序都将继续引发 <xref:System.NullReferenceException?displayProperty=fullName> 以确保兼容性。 面向 .NET Framework 4.7 和更高版本的开发人员应该能看到正确的异常。

#### <a name="suggestion"></a>建议

想要还原为在面向 .NET Framework 4.7 或更高版本时获取 <xref:System.NullReferenceException?displayProperty=fullName> 的开发人员可将以下内容添加/合并到应用程序的 App.config 文件：

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Media.ImageSourceConverter.OverrideExceptionWithNullReferenceException=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Media.ImageSourceConverter.ConvertFrom(System.ComponentModel.ITypeDescriptorContext,System.Globalization.CultureInfo,System.Object)?displayProperty=nameWithType>
