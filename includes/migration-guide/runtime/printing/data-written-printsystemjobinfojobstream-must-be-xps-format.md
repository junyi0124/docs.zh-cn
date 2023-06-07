---
ms.openlocfilehash: 19be8a7755d9b238ab6507eaa73319bddf39faa3
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497369"
---
### <a name="data-written-to-printsystemjobinfojobstream-must-be-in-xps-format"></a>写入到 PrintSystemJobInfo.JobStream 的数据必须采用 XPS 格式

#### <a name="details"></a>详细信息

<xref:System.Printing.PrintSystemJobInfo.JobStream> 属性公开打印作业的流。 通过写入到此流，用户可以将原始数据发送到基础操作系统打印组件。从 Windows 操作系统的 Windows 8 和更高版本上的 .NET Framework 4.5 起，写入到此流的数据必须采用作为包流的 XPS 格式。

#### <a name="suggestion"></a>建议

若要输出打印内容，可以执行下列任一操作：<ul><li>使用 <xref:System.Windows.Xps.XpsDocumentWriter> 类输出打印内容。 这是建议的替代项。</li><li>确保发送给 <xref:System.Printing.PrintSystemJobInfo.JobStream> 属性返回的流的数据作为包流采用 XPS 格式。</li></ul>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Printing.PrintSystemJobInfo.JobStream?displayProperty=nameWithType>

<!--

#### Affected APIs

- `P:System.Printing.PrintSystemJobInfo.JobStream`

-->
