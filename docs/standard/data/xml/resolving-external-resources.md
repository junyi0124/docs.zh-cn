---
title: 解析外部资源
ms.date: 03/30/2017
ms.assetid: ad3fa320-4b8f-4e5c-b549-01157591007a
ms.openlocfilehash: dffaef1b27d5814591c935c0ca795ba4ea3eba84
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686600"
---
# <a name="resolving-external-resources"></a>解析外部资源

XmlDocument  类使用 XmlDocument  的 XmlResolver  属性，定位没有在 XML 数据中内联的资源，如外部文档类型定义 (DTD)、实体和架构。 这些项可以位于网络或本地驱动器上，并通过统一资源标识符 (URI) 进行标识。 这样一来，XmlDocument  可以解析文档中的 EntityReference  节点，并根据外部 DTD 或架构验证文档。  
  
## <a name="fully-trusted-xmldocument"></a>完全受信任的 XmlDocument  

 XmlResolver  属性影响 XmlDocument.Load  方法的功能。 下表展示了 XmlDocument.XmlResolver  属性在 XmlDocument  对象完全受信任时的工作原理。 下表展示了 Load 输入是 TextReader  、String  、Stream  或 URI  时的 XmlDocument.Load  方法。 如果 XmlDocument  是从 XmlReader  中加载，此表不适用于 Load  方法。  
  
|XmlResolver 属性|函数|说明|  
|--------------------------|--------------|-----------|  
|属性设置为之前创建且用户已设置属性的 XmlResolver  类。|XmlDocument  使用给定的 XmlResolver  ，以解析文件名、对外部资源（如 DTD、实体和架构）的引用。<br /><br /> XmlResolver  还用来解析在 XmlDocument  中添加或编辑节点时需要的外部资源。|指定给 XmlDocument  的 XmlResolver  是，每当需要定位和解析外部资源时使用的解析程序。|  
|此属性设置为 null  （Microsoft Visual Basic .NET 中的 Nothing  ）。|不支持需要外部资源的功能，如定位外部架构或 DTD。 也不能解析外部实体，而且也不支持执行编辑功能（如插入需要解析的实体节点）。|XmlDocument  将文件加载为匿名文件，而且不尝试解析其他任何资源。|  
|不设置此属性，而是将其保留为默认状态。|解析文件名和定位外部 DTD、实体和架构时，XmlDocument  实例化并使用含 NULL 凭据的 XmlUrlResolver  ，null  凭据在编辑节点时使用。||  
  
 下表展示了 Load  输入是 XmlReader  且 XmlDocument  完全受信任时的 XmlDocument.Load  方法。  
  
|XmlResolver 属性|函数|说明|  
|--------------------------|--------------|-----------|  
|XmlDocument  使用的 XmlResolver  类与 XmlReader  使用的类相同。|XmlDocument  使用分配给 XmlReader  的 XmlResolver  。<br /><br /> 无论 XmlDocument  信任级别如何，都无法设置 XmlDocument.Resolver  属性，因为它是从 XmlReader  获取 XmlResolver  。 不能试图通过设置 XmlDocument  的 XmlResolver  属性来重写 XmlReaders  的 XmlResolver  设置。|XmlReader  可以是 XmlTextReader  、XmlValidatingReader  或自定义编写的读取器。 如果使用的读取器支持实体解析，则解析外部实体。 如果传递的读取器不支持实体引用，那么就不解析实体引用。|  
  
## <a name="semi-trusted-xmldocument"></a>不完全受信任的 XmlDocument  

 下表展示了 XmlDocument.XmlResolver  属性在对象不完全受信任时的工作原理。 如果 Load 输入是 TextReader  、String  、Stream  或 URI  时，此表适用于 XmlDocument.Load  方法。 如果 XmlDocument  是从 XmlReader  中加载，此表不适用于 Load  方法。  
  
|XmlResolver 属性|函数|说明|  
|--------------------------|--------------|-----------|  
|在不完全受信任的方案中，XmlResolver  属性只能设置为 null。|解析文件名和定位外部 DTD、实体和架构时，XmlDocument  实例化并使用含 null  凭据的 XmlUrlResolver  ，null  凭据在编辑节点时使用。|此行为与不设置  XmlResolver 而是保持为默认状态时的行为完全相同。<br /><br /> XmlDocument  对所有操作使用匿名权限。|  
|此属性设置为 null  （Microsoft Visual Basic .NET 中的 Nothing  ）。|不支持需要外部资源的功能，如定位外部架构或 DTD。 也不能解析外部实体，而且也不支持执行编辑功能（如插入需要解析的实体节点）。|如果属性为 null  ，行为是相同的，不管 XmlDocument  是完全受信任，还是不完全受信任。|  
|不设置此属性，而是将其保留为默认状态。|解析文件名和定位外部 DTD、实体和架构时，XmlDocument  实例化并使用含 null  凭据的 XmlUrlResolver  ，null  凭据在编辑节点时使用。|XmlDocument  对所有操作使用匿名权限。|  
  
 如果 Load  输入是 XmlReader  且 XmlDocument  不完全受信任，此表适用于 XmlDocument.Load  方法。  
  
|XmlResolver 属性|函数|说明|  
|--------------------------|--------------|-----------|  
|XmlDocument  使用的 XmlResolver  类与 XmlReader  使用的类相同。|XmlDocument  使用分配给 XmlReader  的 XmlResolver  。<br /><br /> 无论 XmlDocument  信任级别如何，都无法设置 XmlDocument.Resolver  属性，因为它是从 XmlReader  获取 XmlResolver  。 不能试图通过设置 XmlDocument  的 XmlResolver  属性来重写 XmlReaders  的 XmlResolver  设置。|XmlReader  可以是 XmlTextReader  、验证 <xref:System.Xml.XmlReader> 或自定义编写的读取器。 如果使用的读取器支持实体解析，则解析外部实体。 如果传递的读取器不支持实体引用，那么就不解析实体引用。|  
  
 将 XmlResolver 设置为包含正确的凭据后，将可以访问外部资源。  
  
> [!NOTE]
> 无法检索 XmlResolver  属性。 这有助于防止用户重用已设置凭据的 XmlResolver  。 此外，如果使用 XmlTextReader  或验证 <xref:System.Xml.XmlReader> 加载 XmlDocument  ，且 XmlDocument  有已设置的解析程序，XmlDocument  在 Load  阶段后不缓存这些读取器中的解析程序，因为这也会带来安全风险。  
  
 有关详细信息，请参阅 <xref:System.Xml.XmlResolver> 引用页的“备注”部分。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
