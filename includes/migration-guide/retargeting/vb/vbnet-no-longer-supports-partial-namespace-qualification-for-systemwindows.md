---
ms.openlocfilehash: cef8096c971da8ae245ff974697022f350cb9195
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616023"
---
### <a name="vbnet-no-longer-supports-partial-namespace-qualification-for-systemwindows-apis"></a>VB.NET 不再支持 System.Windows API 的部分命名空间限定

#### <a name="details"></a>详细信息

从 .NET Framework 4.5.2 开始，VB.NET 项目无法通过部分限定命名空间指定 System.Windows API。 例如，对 `Windows.Forms.DialogResult` 的引用将失败。 而代码必须引用完全限定名称 (<xref:System.Windows.Forms.DialogResult>)，或导入特定命名空间并仅引用 <xref:System.Windows.Forms.DialogResult?displayProperty=fullName>。

#### <a name="suggestion"></a>建议

应更新代码，以使用简单名称（并导入相关命名空间）或使用完全限定名称来引用 `System.Windows` API。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.5.2       |
| 类型    | 重定目标 |
