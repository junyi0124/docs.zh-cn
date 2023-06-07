---
ms.openlocfilehash: bb264e406c6604c3606e564d99018eda0f9e8d89
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032035"
---
### <a name="apis-that-report-version-now-report-product-and-not-file-version"></a>报告版本的 API 现报告的是产品版本而不是文件版本

返回 .NET Core 中的版本的很多 API 现在返回的是产品版本，而不是文件版本。

#### <a name="change-description"></a>更改描述

在 .NET Core 2.2 及更低版本中，<xref:System.Environment.Version?displayProperty=nameWithType> 和 <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 等方法以及 .NET Core 程序集的“文件属性”对话框反映的都是文件版本。 自 .NET Core 3.0 起，它们反映的是产品版本。

下图展示了由“Windows 资源管理器”文件属性对话框显示的 .NET Core 2.2（左侧）与 .NET Core 3.0（右侧）的 System.Runtime.dll 程序集版本信息区别   。

![产品版本信息中的差异](~/docs/images/core-changes/corefx/version-information-changes/file-details.png)

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

无。 此更改会使版本检测变得直观而非退化。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Environment.Version?displayProperty=nameWithType>
- <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType>

<!--

#### Affected APIs

- `P:System.Environment.Version`
- `P:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription`

-->
