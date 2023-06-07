---
title: 访问 XML
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ to XML [Visual Basic], accessing XML
- Visual Basic code, XML
- accessing XML [Visual Basic], axis properties
- XML [Visual Basic], axis properties
- XML [Visual Basic], accessing
ms.assetid: c47f88b2-3cbc-4bb1-b4b9-be60f71ffc6a
ms.openlocfilehash: 8ffe6d5ed368aee6d6984ec6ab28c8832921a3f8
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91080174"
---
# <a name="accessing-xml-in-visual-basic"></a>在 Visual Basic 中访问 XML

Visual Basic 提供了用于访问和导航结构的 XML 轴属性 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 。 这些属性使用特殊语法，使你能够通过指定 XML 名称访问元素和属性。  
  
 下表列出了可用于访问 Visual Basic 中的 XML 元素和属性的语言功能。  
  
### <a name="xml-axis-properties"></a>XML 轴属性  
  
|属性说明|示例|描述|  
|--------------------------|-------------|-----------------|  
|*child 轴*|`contact.<phone>`|获取 `phone` 作为元素的子元素的所有元素 `contact` 。|  
|*属性轴*|`phone.@type`|获取 `type` 元素的所有属性 `phone` 。|  
|*descendant 轴*|`contacts...<name>`|获取元素的所有 `name` 元素 `contacts` ，而不考虑它们在层次结构中出现的深度。|  
|*扩展索引器*|`contacts...<name>(0)`|获取序列中的第一个 `name` 元素。|  
|*value*|`contacts...<name>.Value`|获取序列中第一个对象的字符串表示形式; `Nothing` 如果序列为空，则为。|  
  
## <a name="in-this-section"></a>本节内容  

 [如何：访问 XML 子代元素](how-to-access-xml-descendant-elements.md)  
 演示如何使用子代轴属性访问具有指定名称并包含在指定的 XML 元素下的所有 XML 元素。  
  
 [如何：访问 XML 子元素](how-to-access-xml-child-elements.md)  
 演示如何使用子轴属性访问 XML 元素中具有指定名称的所有 XML 子元素。  
  
 [如何：访问 XML 属性](how-to-access-xml-attributes.md)  
 演示如何使用属性轴属性访问 XML 元素中具有指定名称的所有 XML 属性。  
  
 [如何：声明和使用 XML 命名空间前缀](how-to-declare-and-use-xml-namespace-prefixes.md)  
 演示如何声明 XML 命名空间前缀并使用它来创建和访问 XML 元素。  
  
## <a name="related-sections"></a>相关章节  

 [XML 轴属性](../../../language-reference/xml-axis/index.md)  
 提供一些链接，这些链接指向描述各种 XML 访问属性的部分。  
  
 [Visual Basic 中的 LINQ to XML 概述](overview-of-linq-to-xml.md)  
 介绍如何 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 在 Visual Basic 中使用。  
  
 [在 Visual Basic 中创建 XML](creating-xml.md)  
 介绍如何使用 Visual Basic 中的 XML 文本。  
  
 [在 Visual Basic 中操作 XML](manipulating-xml.md)  
 提供一些链接，这些链接指向有关在 Visual Basic 中加载和修改 XML 的部分。  
  
 [XML](index.md)  
 提供一些链接，这些链接指向介绍如何 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 在 Visual Basic 中使用的部分。
