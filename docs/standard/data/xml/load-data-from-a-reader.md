---
title: 从读取器中加载数据
ms.date: 03/30/2017
ms.assetid: 7e74918c-bc72-4977-a49b-e1520a6d8f60
ms.openlocfilehash: 345b6e35c84fe531e131d27bd09fbd8a87b4521a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720134"
---
# <a name="load-data-from-a-reader"></a>从读取器中加载数据

如果使用 <xref:System.Xml.XmlDocument.Load%2A> 方法和 <xref:System.Xml.XmlReader> 的参数加载 XML 文档，与从其他格式加载数据的行为相比，发生的行为有所不同。 如果读取器处于初始状态，<xref:System.Xml.XmlDocument.Load%2A> 将使用读取器中的全部内容，并通过读取器中的所有数据生成 XML 文档对象模型 (DOM)。  
  
 如果读取器已位于文档中某个位置的节点上，并且将读取器传递给 <xref:System.Xml.XmlDocument.Load%2A> 方法，<xref:System.Xml.XmlDocument.Load%2A> 会尝试将当前节点及其所有同级节点（直到关闭当前深度的结束标记）读入内存。 尝试的 <xref:System.Xml.XmlDocument.Load%2A> 是否成功取决于在尝试加载时读取器所处的节点，因为 <xref:System.Xml.XmlDocument.Load%2A> 会验证读取器中的 XML 的格式是否正确。 如果 XML 的格式不正确，<xref:System.Xml.XmlDocument.Load%2A> 将引发异常。 例如，以下节点集包含两个根级别的元素，XML 的格式不正确，<xref:System.Xml.XmlDocument.Load%2A> 将引发异常。  
  
- 依次为 Comment 节点、Element 节点、Element 节点、EndElement 节点。  
  
 下面的节点集创建不完整的 DOM，原因是没有根级别元素。  
  
- Comment 节点，后跟 ProcessingInstruction 节点，后跟 Comment 节点，后跟 EndElement 节点。  
  
 这不引发异常，并且加载数据。 可以向这些节点的顶部添加根元素并创建保存时不会发生错误的格式正确的 XML。  
  
 如果读取器定位于对于文档的根级别来说无效的叶节点（如空白或属性节点），则读取器继续读取，直到定位在可用于根的节点上。 文档在此时开始加载。  
  
 默认情况下，<xref:System.Xml.XmlDocument.Load%2A> 不会使用文档类型定义 (DTD) 或架构验证来验证 XML 是否有效。 只验证 XML 的格式是否正确。 要进行验证，需要使用 <xref:System.Xml.XmlReader> 类创建一个 <xref:System.Xml.XmlReaderSettings>。 <xref:System.Xml.XmlReader> 类可以使用 DTD 或架构定义语言 (XSD) 架构强制进行验证。 <xref:System.Xml.ValidationType> 类的 <xref:System.Xml.XmlReaderSettings> 属性确定 <xref:System.Xml.XmlReader> 实例是否强制进行验证。 有关验证 XML 数据的详细信息，请参阅 <xref:System.Xml.XmlReader> 引用页的“备注”部分。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
