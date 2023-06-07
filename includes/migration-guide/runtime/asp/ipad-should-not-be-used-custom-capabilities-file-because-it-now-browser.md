---
ms.openlocfilehash: af10716fe5f4c07091e8605cdf620e4a499fb1e8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619825"
---
### <a name="ipad-should-not-be-used-in-custom-capabilities-file-because-it-is-now-a-browser-capability"></a>不应在自定义功能文件中使用 IPad，因为它现在是浏览器功能

#### <a name="details"></a>详细信息

自 .NET Framework 4.5 起，iPad 是默认 ASP.NET 浏览器功能文件中的标识符，所以它不应在自定义功能文件中使用

#### <a name="suggestion"></a>建议

如果需要特定于 iPad 的功能，则需要修改 iPad 行为，具体方法为 设置预定义网关 refID &quot;IPad&quot; 上的功能，而不是通过用户代理匹配生成新的 &quot;IPad&quot; ID。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|
