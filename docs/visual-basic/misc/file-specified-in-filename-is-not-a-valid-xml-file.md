---
title: 在 FileName 中指定的文件不是有效的 XML 文件
ms.date: 07/20/2015
ms.assetid: c4c30bf3-e0ad-4bc8-89e0-2c3e49e9793b
ms.openlocfilehash: 7d9500e58044f52a4e2508c9cb23a0e8186bc08d
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90553891"
---
# <a name="file-specified-in-filename-is-not-a-valid-xml-file"></a>在 FileName 中指定的文件不是有效的 XML 文件

所提供的文件名称不是有效的 XML 文件。 若要指定允许的 XML 文档的结构和内容，可使用文档类型定义 (DTD)、Microsoft XML 数据缩减 (XDR) 架构或 XML 架构定义语言 (XSD) 架构。 XSD 架构是在 .NET Framework 中指定 XML 语法的首选方式。

> [!NOTE]
> 在某些较早版本的 Visual Studio 中，“XML 设计器” **** 是针对类型化数据集和 XML 架构的设计器。 “XML 设计器” **** 仍可用于创建和编辑 XML 架构文件。 但是，在 Visual Studio 2012 中，用于创建和编辑类型化数据集的设计器是 **数据集设计器**。 有关详细信息，请参阅 [创建和编辑类型化数据集](/previous-versions/visualstudio/visual-studio-2013/314t4see(v=vs.120))。

## <a name="to-correct-this-error"></a>更正此错误

- 检查你所提供的文件名是否正确。

- 通过将你想要检查的 XML 文件加载到“XML 设计器” **** 中，检查指定文件是否包含有效的 XML。 从“XML” **** 菜单上，单击“验证 XML” ****。 错误都显示在“输出列表” **** 中。

- 如果 XML 文件具有关联的 XML 架构，请检查显示在已定义结构中的元素，以及单个元素的内容是否符合架构中指定的已声明的数据类型。

## <a name="see-also"></a>请参阅

- <xref:System.Xml>
- [如何：分析文件路径](../developing-apps/programming/drives-directories-files/how-to-parse-file-paths.md)
