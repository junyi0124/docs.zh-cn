---
ms.openlocfilehash: f980c8029375074889505a8eb7e8a65aaa8d74e4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614367"
---
### <a name="resgen-refuses-to-load-content-from-the-web"></a>Resgen 拒绝从 Web 加载内容

#### <a name="details"></a>详细信息

.resx 文件可能包含二进制格式的输入。 如果尝试使用 resgen 来加载从不受信任的位置下载的文件，则默认情况下将无法加载该输入。

#### <a name="suggestion"></a>建议

需要从不受信任的位置加载二进制格式输入的 resgen 用户可以从输入文件中删除 Web 标记，也可以应用选择退出。添加以下注册表设置，以应用计算机范围的选择退出：[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft.NETFramework\SDK] &quot;AllowProcessOfUntrustedResourceFiles&quot;=&quot;true&quot;

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |
