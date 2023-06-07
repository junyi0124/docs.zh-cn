---
title: 反射提供程序（WCF 数据服务）
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, providers
ms.assetid: ef5ba300-6d7c-455e-a7bd-d0cc6d211ad4
ms.openlocfilehash: 0eeb223093d709cfe2722c2ad7cf622164eab32f
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568876"
---
# <a name="reflection-provider-wcf-data-services"></a>反射提供程序（WCF 数据服务）

除了通过实体框架公开数据模型中的数据以外，WCF 数据服务可以公开未在基于实体的模型中严格定义的数据。 反射提供程序公开类中的数据，这些类返回实现 <xref:System.Linq.IQueryable%601> 接口的类型。 WCF 数据服务使用反射推断这些类的数据模型，并且可以将针对资源的基于地址的查询转换为针对公开 <xref:System.Linq.IQueryable%601> 类型的基于语言集成查询（LINQ）的查询。

> [!NOTE]
> 可使用 <xref:System.Linq.Queryable.AsQueryable%2A> 方法从实现 <xref:System.Linq.IQueryable%601> 接口的任何类返回 <xref:System.Collections.Generic.IEnumerable%601> 接口。 这允许将大多数泛型集合类型用作数据服务的数据源。

反射提供程序支持类型层次结构。 有关详细信息，请参阅[如何：使用反射提供程序创建数据服务](create-a-data-service-using-rp-wcf-data-services.md)。

## <a name="inferring-the-data-model"></a>推断数据模型

创建数据服务时，提供程序通过使用反射来推断数据模型。 以下列表显示了反射提供程序如何推断数据模型：

- 实体容器 – 将数据作为可返回 <xref:System.Linq.IQueryable%601> 实例的属性公开的类。 对基于反射的数据模型进行寻址时，实体容器表示服务的根。 对给定命名空间仅支持一个实体容器类。

- 实体集 – 返回 <xref:System.Linq.IQueryable%601> 实例的属性视为实体集。 在查询中，将把实体集作为资源直接进行寻址。 实体容器中只有一个属性才能返回给定类型的 <xref:System.Linq.IQueryable%601> 实例。

- 实体类型 – 实体集返回的 `T` 的类型 <xref:System.Linq.IQueryable%601>。 反射提供程序将作为继承层次结构一部分的类转换为等效的实体类型层次结构。

- 实体键 – 作为实体类型的每个数据类必须具有一个键属性。 该属性具有 <xref:System.Data.Services.Common.DataServiceKeyAttribute> 特性 (`[DataServiceKeyAttribute]`)。

    > [!NOTE]
    > 您应将 <xref:System.Data.Services.Common.DataServiceKeyAttribute> 特性仅应用于可以用来唯一标识实体类型的实例的属性。 当应用于导航属性时忽略此特性。

- 实体类型属性 – 与实体键不同的是，反射提供程序按如下方式处理作为实体类型的类的可访问非索引器属性：

  - 如果属性返回基元类型，则假定该属性为实体类型的属性。

  - 如果属性返回的类型同时也是实体类型，则假定该属性为导航属性，表示多对一或一对一关系的“一”端。

  - 如果属性返回实体类型的 <xref:System.Collections.Generic.IEnumerable%601>，则假定该属性为导航属性，表示一对多或多对多关系的“多”端。

  - 如果属性的返回类型为一个值类型，则该属性表示一个复杂类型。

> [!NOTE]
> 与基于实体关系模型的数据模型不同，基于反射提供程序的模型不了解关系数据。 应使用实体框架通过 WCF 数据服务公开关系数据。

## <a name="data-type-mapping"></a>数据类型映射

根据 .NET Framework 类推断数据模型时，数据模型中的基元类型按如下方式映射到 .NET Framework 数据类型：

|.NET Framework 数据类型|数据模型类型|
|------------------------------|---------------------|
|<xref:System.Byte> `[]`|`Edm.Binary`|
|<xref:System.Boolean>|`Edm.Boolean`|
|<xref:System.Byte>|`Edm.Byte`|
|<xref:System.DateTime>|`Edm.DateTime`|
|<xref:System.Decimal>|`Edm.Decimal`|
|<xref:System.Double>|`Edm.Double`|
|<xref:System.Guid>|`Edm.Guid`|
|<xref:System.Int16>|`Edm.Int16`|
|<xref:System.Int32>|`Edm.Int32`|
|<xref:System.Int64>|`Edm.Int64`|
|<xref:System.SByte>|`Edm.SByte`|
|<xref:System.Single>|`Edm.Single`|
|<xref:System.String>|`Edm.String`|

> [!NOTE]
> .NET Framework 可以为 null 的值类型会映射到与不能分配 null 的相应值类型相同的数据模型类型。

## <a name="enabling-updates-in-the-data-model"></a>在数据模型中启用更新

为了对通过此类数据模型公开的数据进行更新，反射提供程序定义了一个 <xref:System.Data.Services.IUpdatable> 接口。 该接口指示数据服务如何保持对公开类型的更新。 若要启用对数据模型定义的资源的更新，实体容器类必须实现 <xref:System.Data.Services.IUpdatable> 接口。 有关 <xref:System.Data.Services.IUpdatable> 接口的实现的示例，请参阅[如何：使用 LINQ to SQL 数据源创建数据服务](create-a-data-service-using-linq-to-sql-wcf.md)。

<xref:System.Data.Services.IUpdatable> 接口要求实现以下成员，以便可使用反射提供程序将更新传播到数据源：

|成员|描述|
|------------|-----------------|
|<xref:System.Data.Services.IUpdatable.AddReferenceToCollection%2A>|提供将对象添加到从导航属性访问的相关对象集合的功能。|
|<xref:System.Data.Services.IUpdatable.ClearChanges%2A>|提供取消挂起的数据更改的功能。|
|<xref:System.Data.Services.IUpdatable.CreateResource%2A>|提供在指定容器中创建新资源的功能。|
|<xref:System.Data.Services.IUpdatable.DeleteResource%2A>|提供删除资源的功能。|
|<xref:System.Data.Services.IUpdatable.GetResource%2A>|提供检索由特定查询和类型名称标识的资源的功能。|
|<xref:System.Data.Services.IUpdatable.GetValue%2A>|提供返回资源的属性值的功能。|
|<xref:System.Data.Services.IUpdatable.RemoveReferenceFromCollection%2A>|提供从通过导航属性访问的相关对象集合中移除某一对象的功能。|
|<xref:System.Data.Services.IUpdatable.ResetResource%2A>|提供更新指定资源的功能。|
|<xref:System.Data.Services.IUpdatable.ResolveResource%2A>|提供返回由特定对象实例表示的资源的功能。|
|<xref:System.Data.Services.IUpdatable.SaveChanges%2A>|提供保存挂起的所有更改的功能。|
|<xref:System.Data.Services.IUpdatable.SetReference%2A>|提供通过使用导航属性来设置相关对象引用的功能。|
|<xref:System.Data.Services.IUpdatable.SetValue%2A>|提供设置资源的属性值的功能。|

## <a name="handling-concurrency"></a>处理并发

通过使你能够为实体定义并发标记，WCF 数据服务支持开放式并发模型。 这样一个包含一个或多个实体属性的并发标记由数据服务用来确定，正在请求、更新或删除的数据中是否发生了更改。 如果从请求的 eTag 中获取的标记值与实体的当前值不相同，则数据服务将引发异常。 将 <xref:System.Data.Services.ETagAttribute> 应用于某个实体类型可在反射提供程序中定义并发标记。 并发标记不能包含键属性或导航属性。 有关详细信息，请参阅[更新数据服务](updating-the-data-service-wcf-data-services.md)。

## <a name="using-linq-to-sql-with-the-reflection-provider"></a>配合使用 LINQ to SQL 和反射提供程序

由于默认情况下支持实体框架，因此它是建议用于将关系数据与 WCF 数据服务结合使用的数据提供程序。 但是，可以使用反射提供程序来配合使用 LINQ to SQL 类和数据服务。 由 LINQ to SQL 对象关系设计器（O/R 设计器）生成的 <xref:System.Data.Linq.DataContext> 上的方法返回的 <xref:System.Data.Linq.Table%601> 结果集实现 <xref:System.Linq.IQueryable%601> 接口。 这样，反射提供程序便可以通过使用生成的 LINQ to SQL 类从 SQL Server 访问这些方法和返回实体数据。 但是，由于 LINQ to SQL 不会实现 <xref:System.Data.Services.IUpdatable> 接口，因此需要添加一个可扩展现有 <xref:System.Data.Linq.DataContext> 分部类的分部类才能添加 <xref:System.Data.Services.IUpdatable> 实现。 有关详细信息，请参阅[如何：使用 LINQ to SQL 数据源创建数据服务](create-a-data-service-using-linq-to-sql-wcf.md)。

## <a name="see-also"></a>另请参阅

- [数据服务提供程序](data-services-providers-wcf-data-services.md)
