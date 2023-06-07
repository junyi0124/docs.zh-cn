---
title: Seedwork（适用于域模型的可重用基类和接口）
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 使用 seedwork 概念作为开始实现面向 DDD 的域模型的起点。
ms.date: 10/08/2018
ms.openlocfilehash: 545be2723ba468a5fd65f81978799328234ca113
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80988305"
---
# <a name="seedwork-reusable-base-classes-and-interfaces-for-your-domain-model"></a>Seedwork（适用于域模型的可重用基类和接口）

解决方案文件夹包含 SeedWork 文件夹  。 此文件夹包含可以用作域实体和值对象的基础的自定义基类。 使用这些基类，从而在每个域的对象类中避免冗余代码。 这些类型的类的文件夹称为 SeedWork，而不是类似于 Framework   。 它称为“SeedWork”  ，因为该文件夹仅包含可重用类的一个小型子集，不能将其视为一个框架。  Seedwork 是由 [Michael Feathers](https://www.artima.com/forums/flat.jsp?forum=106&thread=8826) 引入的一个术语，并且由 [Martin Fowler](https://martinfowler.com/bliki/Seedwork.html) 推广普及，但也可将该文件夹命名为 Common、SharedKernel 或类似名称。

图 7-12 显示在排序的微服务中形成域模型的 seedwork 的类。 它还包含 Entity、ValueObject 和 Enumeration 等自定义基类，以及一些接口。 这些接口（IRepository 和 IUnitOfWork）告知基础结构层需要实现的内容。 还可通过应用程序层中的依赖关系注入使用这些接口。

:::image type="complex" source="./media/seedwork-domain-model-base-classes-interfaces/vs-solution-seedwork-classes.png" alt-text="SeedWork 文件夹中包含的类的屏幕截图。":::
SeedWork 文件夹的详细内容，包含基类和接口：Entity.cs、Enumeration.cs、IAggregateRoot.cs、IRepository.cs、IUnitOfWork.cs 和 ValueObject.cs。
:::image-end:::

**图 7-12**。 域模型“seedwork”基类和接口的示例集

这是许多开发者在项目之间共享的复制和粘贴重用类型，不是正式框架。 seedwork 可存在于任何层或库中。 但是，如果类和接口的集足够大，可能需要创建单个类库。

## <a name="the-custom-entity-base-class"></a>自定义实体基类

以下代码是实体基类的示例，可在其中放置可由任何域实体以相同方式使用的代码，例如实体 ID、[相等运算符](../../../csharp/language-reference/operators/equality-operators.md)、每个实体的域事件列表等。

```csharp
// COMPATIBLE WITH ENTITY FRAMEWORK CORE (1.1 and later)
public abstract class Entity
{
    int? _requestedHashCode;
    int _Id;
    private List<INotification> _domainEvents;
    public virtual int Id
    {
        get
        {
            return _Id;
        }
        protected set
        {
            _Id = value;
        }
    }

    public List<INotification> DomainEvents => _domainEvents;
    public void AddDomainEvent(INotification eventItem)
    {
        _domainEvents = _domainEvents ?? new List<INotification>();
        _domainEvents.Add(eventItem);
    }
    public void RemoveDomainEvent(INotification eventItem)
    {
        if (_domainEvents is null) return;
        _domainEvents.Remove(eventItem);
    }

    public bool IsTransient()
    {
        return this.Id == default(Int32);
    }

    public override bool Equals(object obj)
    {
        if (obj == null || !(obj is Entity))
            return false;
        if (Object.ReferenceEquals(this, obj))
            return true;
        if (this.GetType() != obj.GetType())
            return false;
        Entity item = (Entity)obj;
        if (item.IsTransient() || this.IsTransient())
            return false;
        else
            return item.Id == this.Id;
    }

    public override int GetHashCode()
    {
        if (!IsTransient())
        {
            if (!_requestedHashCode.HasValue)
                _requestedHashCode = this.Id.GetHashCode() ^ 31;
            // XOR for random distribution. See:
            // https://docs.microsoft.com/archive/blogs/ericlippert/guidelines-and-rules-for-gethashcode
            return _requestedHashCode.Value;
        }
        else
            return base.GetHashCode();
    }
    public static bool operator ==(Entity left, Entity right)
    {
        if (Object.Equals(left, null))
            return (Object.Equals(right, null));
        else
            return left.Equals(right);
    }
    public static bool operator !=(Entity left, Entity right)
    {
        return !(left == right);
    }
}
```

使用每个实体的域事件列表的上一个代码将在下一部分（重点是域事件）介绍。

## <a name="repository-contracts-interfaces-in-the-domain-model-layer"></a>域模型层中的存储库协定（接口）

存储库协定是表达存储库的协定要求的 .NET 接口，用于每个聚合。

存储库本身（包含 EF Core 代码或其他任何基础结构依赖项和代码（Linq、SQL 等）不能在域模型内实现，存储库应仅实现你在域模型中定义的接口。

这种做法（在域模型层中放置存储库接口）的相关模式是分隔接口模式。 正如 Martin Fowler [所述](https://www.martinfowler.com/eaaCatalog/separatedInterface.html)，“使用分隔接口在一个包中定义接口，但在另一个包中实现它。 这样一来，需要接口依赖项的客户端可能完全不会识别出实现。”

通过遵循分隔接口模式，应用程序层（在此情况下是微服务的 Web API 项目）可具有在域模型中定义的要求的依赖项，但没有基础结构/持久性层的直接依赖项。 此外，可以使用依赖项注入隔离实现，可在使用存储库的基础结构/持久性层中实现这一点。

例如，包含 IOrderRepository 接口的下面的示例定义 OrderRepository 类在基础机构层中实现所需的操作。 在应用程序的当前实现中，代码只需将订单添加或更新到数据库，因为查询根据简化的 CQRS 方法拆分。

```csharp
// Defined at IOrderRepository.cs
public interface IOrderRepository : IRepository<Order>
{
    Order Add(Order order);

    void Update(Order order);

    Task<Order> GetAsync(int orderId);
}

// Defined at IRepository.cs (Part of the Domain Seedwork)
public interface IRepository<T> where T : IAggregateRoot
{
    IUnitOfWork UnitOfWork { get; }
}
```

## <a name="additional-resources"></a>其他资源

- **Martin Fowler。Separated Interface.** （分隔接口） \
  <https://www.martinfowler.com/eaaCatalog/separatedInterface.html>

>[!div class="step-by-step"]
>[上一页](net-core-microservice-domain-model.md)
>[下一页](implement-value-objects.md)
