---
title: 嵌入到文档中的样式表指令
ms.date: 03/30/2017
ms.assetid: d79fb295-ebc7-438d-ba1b-05be7d534834
ms.openlocfilehash: 25946fd14a82428ae4367cd33511df68e9929203
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94818565"
---
# <a name="style-sheet-directives-embedded-in-a-document"></a>嵌入到文档中的样式表指令

有时，现有 XML 会包含 `<?xml:stylesheet?>` 形式的样式表指令。 Microsoft Internet Explorer 接受此指令作为 `<?xml-stylesheet?>` 语法的替换形式。 当 XML 数据包含 `<?xml:stylesheet?>` 指令时（如下面的数据所示），试图将此数据加载到 XML 文档对象模型 (DOM) 中将引发异常。

```xml
<?xml version="1.0" ?>
<?xml:stylesheet type="text/xsl" href="test2.xsl"?>
<root>
    <test>Node 1</test>
    <test>Node 2</test>
</root>
```

发生这种情况是因为，`<?xml:stylesheet?>` 被视为对 DOM 无效的 ProcessingInstruction  。 根据 XML 命名空间规范，任何 ProcessingInstruction  都只能是无冒号名称 (NCNames)，而不是限定名称 (QNames)。

根据 XML 命名空间规范的第 6 节，让 Load  和 LoadXml  方法符合此规范所产生的效应是，在文档中：

- 所有元素类型和属性名都包含零个或一个冒号。

- 任何实体名称、ProcessingInstruction 目标或表示法名称都不包含冒号。

因为 `<?xml:stylesheet?>` 包含一个冒号，所以您违反了第二条规则。

根据万维网联合会 (W3C) [将样式表与 XML 文档关联的 1.0 版建议](https://www.w3.org/TR/xml-stylesheet/)，将 XSLT 样式表与 XML 文档关联的处理指令是 `<?xml-stylesheet?>`（用短划线代替冒号）。

## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
