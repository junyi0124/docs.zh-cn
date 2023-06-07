---
title: WCF 数据服务协议实现详细信息
ms.date: 03/30/2017
ms.assetid: 712d689b-fada-4cbb-bcdb-d65a3ef83b4c
ms.openlocfilehash: 0723d5a473bb87b8bd464a944a5c68f1a8d3f5da
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202157"
---
# <a name="wcf-data-services-protocol-implementation-details"></a>WCF 数据服务协议实现详细信息

## <a name="odata-protocol-implementation-details"></a>OData 协议实现详细信息  

Open Data Protocol (OData) 要求实现协议的数据服务提供特定的最小功能集。 这些功能在协议文档中用 "应该" 和 "必须" 描述。 其他可选功能将根据 "可能" 进行描述。 本文介绍 WCF 数据服务当前未实现的这些可选功能。
  
### <a name="support-for-the-format-query-option"></a>对 $format 查询选项的支持  

 OData 协议支持 JavaScript 符号 (JSON) 和 Atom 馈送，并且 OData 提供 `$format` 系统查询选项，以允许客户端请求响应源的格式。 此系统查询选项（如果数据服务支持）必须重写请求的 Accept 标头的值。 WCF 数据服务支持同时返回 JSON 和 Atom 源。 但是，默认实现不支持 `$format` 查询选项，它只使用 Accept 标头的值来确定响应的格式。 某些情况下，您的数据服务可能需要支持 `$format` 查询选项，例如，当客户端不能设置 Accept 标头时。 为支持这些情况，您必须扩展您的数据服务以在 URI 中处理此选项。 您可以通过从 MSDN 代码库网站下载 [JSONP 和 URL 控制的格式支持 ADO.NET Data Services](https://go.microsoft.com/fwlink/?LinkId=208228) 示例项目并将其添加到您的数据服务项目，将此功能添加到您的数据服务。 此示例删除 `$format` 查询选项并将 Accept 标头更改为 `application/json`。 当您包含此示例项目并将 `JSONPSupportBehaviorAttribute` 添加到您的数据服务类时，将允许服务处理 `$format` 查询选项 `$format=json`。 需要进一步自定义此示例项目以处理 `$format=atom` 或其他自定义格式。  
  
## <a name="wcf-data-services-behaviors"></a>WCF 数据服务行为  

 以下 WCF 数据服务行为未通过 OData 协议显式定义：  
  
### <a name="default-sorting-behavior"></a>默认排序行为  

 当发送到数据服务的查询请求包括 `$top` 或 `$skip` 系统查询选项但不包括 `$orderby` 系统查询选项时，返回的源按键属性升序排序。 这是因为需要顺序以确保结果正确分页。 为此，数据服务向查询中添加排序表达式。 在数据服务中启用服务器驱动的分页时，也会发生此行为。 有关详细信息，请参阅 [配置数据服务](configuring-the-data-service-wcf-data-services.md)。若要控制返回的源的排序，应将包含 `$orderby` 在查询 URI 中。  
  
## <a name="see-also"></a>请参阅

- [定义 WCF 数据服务](defining-wcf-data-services.md)
- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
