---
ms.openlocfilehash: a9011514c7c4393ec44de2c7fae88768cdccf435
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496723"
---
### <a name="the-net-framework-46-does-not-use-a-45xx-version-when-registering-itself-in-the-registry"></a>.NET Framework 4.6 在注册表中注册自身时，不会使用 4.5.x.x 版本

#### <a name="details"></a>详细信息

正如所料，在注册表（位于 <code>HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\NET Framework Setup\NDP\v4\Full</code>）中为 .NET Framework 4.6 设置的版本密钥以“4.6”开头，而不是“4.5”。 对于根据这些注册表项了解计算机上应安装哪些版本的 .NET Framework 的应用，应将其更新，以了解 4.6 是新的可行版本，与之前的 4.5.x 版本兼容。

#### <a name="suggestion"></a>建议

通过查找 4.5 注册表项来更新探测 .NET Framework 4.5 安装的应用，从而也可以接受 4.6。

| 名称    | 值       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.6|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
