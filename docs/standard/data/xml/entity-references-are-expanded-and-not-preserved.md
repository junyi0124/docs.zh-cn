---
title: 扩展但不保留实体引用
ms.date: 03/30/2017
ms.assetid: ffd97806-ab43-4538-8de2-5828bfbbde57
ms.openlocfilehash: 5a3c93807866c5925696f2d913dfc443d8ff12a4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687445"
---
# <a name="entity-references-are-expanded-and-not-preserved"></a>扩展但不保留实体引用

如果实体引用进行扩展且替换为它表示的文本，将不创建 XmlEntityReference  节点。 相反，将分析实体声明，并复制通过声明内容创建的节点以取代 XmlEntityReference  。 因此，`&publisher;` 示例不保存 `&publisher;`，而是创建 XmlText  节点。  
  
 ![展开的树结构](media/xmlentityref-expanded-nodes.gif "xmlentityref_expanded_nodes")  
展开的实体引用的树结构  
  
 不保留 `B` 或 `<` 这类字符实体。 相反，它们总是扩展并表示为文本节点。  
  
 若要暂留 XmlEntityReference  节点，以及附加到此节点的实体引用的子节点，请将 EntityHandling  标志设置为 ExpandCharEntities  。 否则，保持 EntityHandling  标志的默认设置（即 ExpandEntities  ）不变。 这种情况下，您在 DOM 中将看不到实体引用节点。 这些节点已被作为实体声明子节点副本的节点取代。  
  
 不保留实体引用的一个副作用是，当保存文档并将其传递给另一个应用程序时，接收应用程序不知道这些节点是由实体引用生成的。 然而，当保留实体引用时，接收应用程序将查看实体引用并读取子节点。 显而易见，这些子节点表示实体声明中的信息。 例如，在保留实体引用的情况下，DOM 理论上包含以下结构。  
  
 XmlElement：发布者  
  
 XmlEntityReference：`&publisher;`  
  
 XmlText：Microsoft Press  
  
 如果在 DOM 中展开实体引用（这是默认方法），则结构包含此类型树：  
  
 XmlElement：发布者  
  
 XmlText：Microsoft Press  
  
 请注意，实体引用节点已消失，接收应用无法判断包含“Microsoft Press”的 XmlText  节点是否是通过实体声明创建。  
  
 如果使用无法解析实体的读取器，Load  方法在遇到实体引用时抛出异常。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
