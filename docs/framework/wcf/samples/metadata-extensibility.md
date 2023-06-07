---
title: 元数据可扩展性
ms.date: 03/30/2017
ms.assetid: f92fcc76-0806-4c84-9d63-7aae0d3899de
ms.openlocfilehash: fb37e33026a5f99bfa033f04eea0bb85fbbe65c7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294321"
---
# <a name="metadata-extensibility"></a>元数据可扩展性

本节包含演示自定义元数据的示例。  
  
## <a name="in-this-section"></a>本节内容  

 [自定义安全元数据终结点](custom-secure-metadata-endpoint.md)  
 演示如何实现一个具有安全元数据终结点（该终结点使用非元数据交换绑定）的服务，以及如何配置 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 或客户端从这类元数据终结点获取元数据的安全元数据终结点。  
  
 [自定义 WSDL 发布](custom-wsdl-publication.md)  
 演示如何在自定义 <xref:System.ServiceModel.Description.IWsdlExportExtension?displayProperty=nameWithType> 特性上实现 <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType>，以便在其他功能中将该特性的属性导出为 WSDL 批注。
