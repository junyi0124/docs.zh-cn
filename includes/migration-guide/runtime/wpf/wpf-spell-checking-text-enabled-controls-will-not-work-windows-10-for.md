---
ms.openlocfilehash: 6d7f998cda6326e1f584713576a0aa27b3a68655
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496152"
---
### <a name="wpf-spell-checking-in-text-enabled-controls-will-not-work-in-windows-10-for-languages-not-in-the-oss-input-language-list"></a>支持文本的控件中的 WPF 拼写检查在 Windows 10 中将不适用于 OS 输入语言列表以外的语言

#### <a name="details"></a>详细信息

在 Windows 10 上运行时，拼写检查器可能不适用于支持 WPF 文本的控件，因为平台拼写检查功能仅适用于输入语言列表中的语言。在 Windows 10 中，将语言添加到可用键盘的列表时，Windows 自动下载并安装相应的按需功能 (FOD) 包，以提供拼写检查功能。 通过将语言添加到输入语言列表，将支持拼写检查器。

#### <a name="suggestion"></a>建议

请注意，必须将要进行拼写检查的语言或文本添加到输入语言以在 Windows 10 中使用拼写检查功能。

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
