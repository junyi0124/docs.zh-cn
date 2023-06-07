---
title: 生成数据服务客户端库（WCF 数据服务）
ms.date: 03/30/2017
helpviewer_keywords:
- client applications, WCF Data Services
- WCF Data Services, client library
- Add Service Reference dialog box
ms.assetid: 314077c1-ac10-47e1-bed4-940b5462359d
ms.openlocfilehash: 050a791736e90b5daf46fd272197ca21a220afb0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172614"
---
# <a name="generating-the-data-service-client-library-wcf-data-services"></a>生成数据服务客户端库（WCF 数据服务）

实现 Open Data Protocol (OData) 的数据服务可以返回描述 OData 源公开的数据模型的服务元数据文档。 有关详细信息，请参阅 [OData：概述](https://www.odata.org/documentation/odata-version-2-0/overview/) 一文中的服务元数据文档部分。 您可以使用 Visual Studio 中的 " **添加服务引用** " 对话框添加对基于 OData 的服务的引用。 使用此工具添加对客户端项目中 OData 源返回的元数据的引用时，它将执行以下操作：  
  
- 从数据服务请求服务元数据文档并截获返回的元数据。  
  
    > [!NOTE]
    > 返回的元数据作为 .edmx 文件存储在客户端项目中。 此 .edmx 文件无法通过实体数据模型设计器打开，因为此文件的格式与实体框架所使用的 .edmx 文件的格式不相同。 可以通过使用 XML 编辑器或任何文本编辑器查看此元数据文件。 有关详细信息，请参阅[ \[ MC-EDMX \] ：用于数据服务打包格式的实体数据模型](/openspecs/windows_protocols/mc-edmx/5dff5e25-56a1-408b-9d44-bff6634c7d16)。
  
- 作为从 <xref:System.Data.Services.Client.DataServiceContext> 继承的实体容器类生成数据服务的表示形式。 此生成的实体容器类与实体数据模型工具生成的实体容器类似。 有关详细信息，请参阅[对象服务概述 (Entity Framework)](/previous-versions/bb386871(v=vs.100))。  
  
- 为在服务元数据中发现的数据模型类型生成数据类。  
  
- 在项目中添加对 `System.Data.Services.Client` 程序集的引用。  
  
 有关详细信息，请参阅 [如何：添加数据服务引用](how-to-add-a-data-service-reference-wcf-data-services.md)。  
  
 还可以通过在命令提示符下使用 [DataSvcUtil.exe](wcf-data-service-client-utility-datasvcutil-exe.md) 工具来生成客户端数据服务类。 有关详细信息，请参阅 [如何：手动生成客户端数据服务类](how-to-manually-generate-client-data-service-classes-wcf-data-services.md)。  
  
## <a name="client-data-type-mapping"></a>客户端数据类型映射  

 使用 Visual Studio 中的 " **添加服务引用** " 对话框或 `DataSvcUtil.exe` 工具生成基于 OData 源的客户端数据类时，.NET Framework 数据类型将按以下方式映射到数据模型中的基元类型：  
  
|数据模型类型|.NET framework 数据类型|  
|---------------------|------------------------------|  
|`Edm.Binary`|<xref:System.Byte> `[]`|  
|`Edm.Boolean`|<xref:System.Boolean>|  
|`Edm.Byte`|<xref:System.Byte>|  
|`Edm.DateTime`|<xref:System.DateTime>|  
|`Edm.Decimal`|<xref:System.Decimal>|  
|`Edm.Double`|<xref:System.Double>|  
|`Edm.Guid`|<xref:System.Guid>|  
|`Edm.Int16`|<xref:System.Int16>|  
|`Edm.Int32`|<xref:System.Int32>|  
|`Edm.Int64`|<xref:System.Int64>|  
|`Edm.SByte`|<xref:System.SByte>|  
|`Edm.Single`|<xref:System.Single>|  
|`Edm.String`|<xref:System.String>|  
  
 有关详细信息，请参阅 [OData：概述](https://www.odata.org/documentation/odata-version-2-0/overview/) 一文中的基元数据类型部分。
  
## <a name="see-also"></a>请参阅

- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
- [快速入门](quickstart-wcf-data-services.md)
