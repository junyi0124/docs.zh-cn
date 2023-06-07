---
title: 服务操作（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service operations [WCF Data Services]
- WCF Data Services, service operations
ms.assetid: 583a690a-e60f-4990-8991-d6efce069d76
ms.openlocfilehash: c254a7362c7bc28f4b38fc0189ae0ea763bc90cc
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568851"
---
# <a name="service-operations-wcf-data-services"></a>服务操作（WCF 数据服务）

WCF 数据服务使你能够在数据服务上定义服务操作，以便在服务器上公开方法。 与其他数据服务资源一样，服务操作也是通过 URI 进行寻址。 使用服务操作可以在数据服务中公开业务逻辑；例如，实现验证逻辑，应用基于角色的安全性或公开专用查询功能。 服务操作是添加到派生自 <xref:System.Data.Services.DataService%601> 的数据服务类的方法。 与所有其他数据服务资源一样，也可以向服务操作方法提供参数。 例如，以下服务操作 URI （基于[快速入门](quickstart-wcf-data-services.md)数据服务）将值 `London` 传递给 `city` 参数：

```http
http://localhost:12345/Northwind.svc/GetOrdersByCity?city='London'
```

此服务操作的定义如下所示：

[!code-csharp[Astoria Northwind Service#ServiceOperationDef](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind2.svc.cs#serviceoperationdef)]
[!code-vb[Astoria Northwind Service#ServiceOperationDef](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind2.svc.vb#serviceoperationdef)]

使用 <xref:System.Data.Services.DataService%601.CurrentDataSource%2A> 的 <xref:System.Data.Services.DataService%601> 可以直接访问数据服务正在使用的数据源。 有关详细信息，请参阅[如何：定义服务操作](how-to-define-a-service-operation-wcf-data-services.md)。

有关如何从 .NET Framework 客户端应用程序调用服务操作的信息，请参阅[调用服务操作](calling-service-operations-wcf-data-services.md)。

## <a name="service-operation-requirements"></a>服务操作要求

在数据服务中定义服务操作需满足下列要求。 如果某一方法不符合这些要求，则不会将该方法公开为数据服务的服务操作。

- 操作必须是作为数据服务类成员的公共实例方法。

- 操作方法只能接受输入参数。 数据服务无法访问在消息正文中发送的数据。

- 如果定义了参数，则每个参数的类型必须为基元类型。 任何非基元类型的数据必须序列化并传递到一个字符串参数。

- 方法必须返回下列值之一：

  - `void`（在 Visual Basic 中为 `Nothing`）

  - <xref:System.Collections.Generic.IEnumerable%601>

  - <xref:System.Linq.IQueryable%601>

  - 数据服务所公开的数据模型中的实体类型。

  - 整数或字符串等基元类。

- 若要支持查询选项（如排序、分页和筛选），服务操作方法应返回 <xref:System.Linq.IQueryable%601>。 只返回 <xref:System.Collections.Generic.IEnumerable%601> 的操作拒绝对包含查询选项的服务操作的请求。

- 若要支持使用导航属性访问相关实体，服务操作必须返回 <xref:System.Linq.IQueryable%601>。

- 必须用 `[WebGet]` 或 `[WebInvoke]` 属性为此方法添加批注。

  - `[WebGet]` 使您能够通过使用 GET 请求调用方法。

  - `[WebInvoke(Method = "POST")]` 使您能够通过使用 POST 请求调用方法。 其他 <xref:System.ServiceModel.Web.WebInvokeAttribute> 方法不受支持。

- 可以用 <xref:System.Data.Services.SingleResultAttribute> 为服务操作添加批注，指定方法的返回值是一个实体而不是一个实体集。 这一区别确定了生成的响应结果串行化以及用 URI 表示其他导航属性遍历的方式。 例如，当使用 AtomPub 序列化时，单个资源类型实例将表示为一个 entry 元素，而单个实例集将表示为一个 feed 元素。

## <a name="addressing-service-operations"></a>对服务操作进行寻址

通过将方法名称放在 URI 的第一个路径段中，可以对服务操作进行寻址。 例如，下面的 URI 访问一个 `GetOrdersByState` 操作，其返回 <xref:System.Linq.IQueryable%601> 对象的 `Orders` 集合。

```http
http://localhost:12345/Northwind.svc/GetOrdersByState?state='CA'&includeItems=true
```

当调用服务操作时，参数作为查询选项提供。 以前的服务操作接受字符串参数 `state` 和布尔值参数 `includeItems`，后者指示在响应中是否包括相关的 `Order_Detail` 对象。

以下是有效的服务操作的返回类型：

|有效的返回类型|URI 规则|
|------------------------|---------------|
|`void`（在 Visual Basic 中为 `Nothing`）<br /><br /> 或<br /><br /> 实体类型<br /><br /> 或<br /><br /> 基元类型|URI 必须是作为服务操作名称的单个路径段。 不允许使用查询选项。|
|<xref:System.Collections.Generic.IEnumerable%601>|URI 必须是作为服务操作名称的单个路径段。 由于结果类型不是 <xref:System.Linq.IQueryable%601> 类型，因此不允许使用查询选项。|
|<xref:System.Linq.IQueryable%601>|除了作为服务操作名称的路径之外，还允许使用查询路径段。 也允许使用查询选项。|

可以将其他路径段或查询选项添加到该 URI，具体取决于服务操作的返回类型。 例如，以下 URI 访问 `GetOrdersByCity` 操作，该操作返回 <xref:System.Linq.IQueryable%601> 对象的 `Orders` 集合（按 `RequiredDate` 的降序顺序排序）以及相关的 `Order_Details` 对象：

```http
http://localhost:12345/Northwind.svc/GetOrdersByCity?city='London'&$expand=Order_Details&$orderby=RequiredDate desc
```

## <a name="service-operations-access-control"></a>服务操作访问控制

服务操作的服务范围可见性由 <xref:System.Data.Services.IDataServiceConfiguration.SetServiceOperationAccessRule%2A> 类的 <xref:System.Data.Services.IDataServiceConfiguration> 方法控制，其方式与使用 <xref:System.Data.Services.IDataServiceConfiguration.SetEntitySetAccessRule%2A> 方法控制实体集可见性的方式大致相同。 例如，数据服务定义中的以下代码行用于访问 `CustomersByCity` 服务操作。

[!code-csharp[Astoria Northwind Service#ServiceOperationConfig](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind2.svc.cs#serviceoperationconfig)]
[!code-vb[Astoria Northwind Service#ServiceOperationConfig](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind2.svc.vb#serviceoperationconfig)]

> [!NOTE]
> 如果服务操作的返回类型已通过限制访问基础实体集的方式被隐藏，则该服务操作将不能用于客户端应用程序。

有关详细信息，请参阅[如何：定义服务操作](how-to-define-a-service-operation-wcf-data-services.md)。

## <a name="raising-exceptions"></a>引发异常

在数据服务执行中引发异常时，我们建议您始终使用 <xref:System.Data.Services.DataServiceException> 类。 这是因为数据服务运行时知道如何正确地将此异常对象的属性映射到 HTTP 响应消息。 在服务操作中引发 <xref:System.Data.Services.DataServiceException> 时，返回的异常包装在 <xref:System.Reflection.TargetInvocationException> 中。 若要返回基 <xref:System.Data.Services.DataServiceException> 而不包含 <xref:System.Reflection.TargetInvocationException>，您必须重写 <xref:System.Data.Services.DataService%601.HandleException%2A> 中的 <xref:System.Data.Services.DataService%601> 方法，从 <xref:System.Data.Services.DataServiceException> 中提取 <xref:System.Reflection.TargetInvocationException>，并将其作为顶级错误返回，如以下示例中所示：

[!code-csharp[Astoria Northwind Service#HandleExceptions](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind2.svc.cs#handleexceptions)]
[!code-vb[Astoria Northwind Service#HandleExceptions](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind2.svc.vb#handleexceptions)]

## <a name="see-also"></a>另请参阅

- [侦听器](interceptors-wcf-data-services.md)
