---
title: 将数据公开为服务（WCF 数据服务）
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, configuring
- getting started, WCF Data Services
- WCF Data Services, getting started
ms.assetid: df0bbcee-f66f-4a88-abb4-4e73c8b9c908
ms.openlocfilehash: 71f26d7d41d112e13e6a77f0927c61d51b176b27
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975307"
---
# <a name="expose-your-data-as-a-service-wcf-data-services"></a>将数据公开为服务（WCF 数据服务）

WCF 数据服务与 Visual Studio 集成，使您能够更轻松地定义服务，将数据公开为 Open Data Protocol （OData）源。 创建公开 OData 源的数据服务涉及以下基本步骤：

1. **定义数据模型。** WCF 数据服务本身支持基于[ADO.NET 实体框架](../adonet/ef/index.md)的数据模型。 有关详细信息，请参阅[如何：使用 ADO.NET 实体框架数据源创建数据服务](create-a-data-service-using-an-adonet-ef-data-wcf.md)。

     WCF 数据服务还支持基于公共语言运行时（CLR）对象的数据模型，这些对象返回 <xref:System.Linq.IQueryable%601> 接口的实例。 通过此功能，您可以在 .NET Framework 中部署基于列表、数组和集合的数据服务。 若要启用针对这些数据结构的创建、更新和删除操作，还必须实现 <xref:System.Data.Services.IUpdatable> 接口。 有关详细信息，请参阅[如何：使用反射提供程序创建数据服务](create-a-data-service-using-rp-wcf-data-services.md)。

     对于更高级的方案，WCF 数据服务包括一组提供程序，使你能够基于后期绑定数据类型定义数据模型。 有关详细信息，请参阅[自定义数据服务提供程序](custom-data-service-providers-wcf-data-services.md)。

2. **创建数据服务。** 大多数基本数据服务公开一个从 <xref:System.Data.Services.DataService%601> 类继承的类和一个作为实体容器的命名空间限定名称的 `T` 类型。 有关详细信息，请参阅 [Defining WCF Data Services](defining-wcf-data-services.md)的信息。

3. **配置数据服务。** 默认情况下，WCF 数据服务禁用对由实体容器公开的资源的访问。 利用 <xref:System.Data.Services.DataServiceConfiguration> 接口，你可以配置对资源和服务操作的访问、指定支持的 OData 版本，并定义其他服务范围的行为，例如批处理行为或可在单个响应中返回的最大实体数。 有关详细信息，请参阅[配置数据服务](configuring-the-data-service-wcf-data-services.md)。

有关如何创建基于 Northwind 示例数据库的简单数据服务的示例，请参阅[快速入门](quickstart-wcf-data-services.md)。

## <a name="see-also"></a>请参阅

- [入门](getting-started-with-wcf-data-services.md)
- [概述](wcf-data-services-overview.md)
