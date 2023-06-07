---
title: N 层应用程序中的数据检索和 CUD 操作 (LINQ to SQL)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c3133d53-83ed-4a4d-af8b-82edcf3831db
ms.openlocfilehash: 1bc97504b4dd053ce9ef747460a79865cbe836ee
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197412"
---
# <a name="data-retrieval-and-cud-operations-in-n-tier-applications-linq-to-sql"></a>N 层应用程序中的数据检索和 CUD 操作 (LINQ to SQL)

在将实体对象（如 Customers 或 Orders）通过网络序列化到客户端时，这些实体会与其数据上下文分离。 数据上下文不再跟踪这些实体的更改或它们与其他对象的关联。 只要客户端只读取数据，这就不会成为问题。 要使客户端可以向数据库添加新行，也比较容易做到。 但是，如果应用程序要求客户端能够更新或删除数据，则必须在调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A?displayProperty=nameWithType> 之前将实体附加到新的数据上下文。 此外，如果对原始值使用开放式并发检查，则还需要一种为数据库同时提供原始实体和修改后的实体的方式。 使用 `Attach` 方法可以在实体分离后将其放入新的数据上下文中。  
  
 即使要序列化代理对象来代替 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 实体，仍然必须在数据访问层 (DAL) 上构造一个实体，并将其附加到新的 <xref:System.Data.Linq.DataContext?displayProperty=nameWithType>，以便将数据提交给数据库。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 完全不关心实体的序列化方式。 有关如何使用对象关系设计器和 SQLMetal 工具生成可通过 Windows Communication Foundation (WCF) 进行序列化的类的详细信息，请参阅 [如何：使实体可序列化](how-to-make-entities-serializable.md)。  
  
> [!NOTE]
> 仅对新实体或反序列化后的实体调用 `Attach` 方法。 将实体与其原始数据上下文分离的唯一方式是将其序列化。 如果试图将未分离的实体附加到新的数据上下文，并且该实体仍然具有来自其以前的数据上下文的延迟加载程序，则 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 会引发异常。 如果一个实体具有来自两个不同数据上下文的延迟加载程序，则在对该实体执行插入、更新和删除操作时，可能产生意外的结果。 有关延迟加载程序的详细信息，请参阅 [延迟与立即加载](deferred-versus-immediate-loading.md)。  
  
## <a name="retrieving-data"></a>检索数据  
  
### <a name="client-method-call"></a>客户端方法调用  

 下面的示例演示一个从 Windows 窗体客户端对 DAL 进行的示例方法调用。 在此示例中，DAL 被实现为 Windows 服务库：  
  
```vb  
Private Function GetProdsByCat_Click(ByVal sender As Object, ByVal e _  
    As EventArgs)  
  
    ' Create the WCF client proxy.  
    Dim proxy As New NorthwindServiceReference.Service1Client  
  
    ' Call the method on the service.  
    Dim products As NorthwindServiceReference.Product() = _  
        proxy.GetProductsByCategory(1)  
  
    ' If the database uses original values for concurrency checks,  
    ' the client needs to store them and pass them back to the  
    ' middle tier along with the new values when updating data.  
  
    For Each v As NorthwindClient1.NorthwindServiceReference.Product _  
        In products  
        ' Persist to a List(Of Product) declared at class scope.  
        ' Additional change-tracking logic is the responsibility  
        ' of the presentation tier and/or middle tier.  
        originalProducts.Add(v)  
    Next  
  
    ' (Not shown) Bind the products list to a control  
    ' and/or perform whatever processing is necessary.  
End Function  
```  
  
```csharp  
private void GetProdsByCat_Click(object sender, EventArgs e)  
{  
    // Create the WCF client proxy.  
    NorthwindServiceReference.Service1Client proxy =
    new NorthwindClient.NorthwindServiceReference.Service1Client();  
  
    // Call the method on the service.  
    NorthwindServiceReference.Product[] products =
    proxy.GetProductsByCategory(1);  
  
    // If the database uses original values for concurrency checks,
    // the client needs to store them and pass them back to the
    // middle tier along with the new values when updating data.  
    foreach (var v in products)  
    {  
        // Persist to a list<Product> declared at class scope.  
        // Additional change-tracking logic is the responsibility  
        // of the presentation tier and/or middle tier.  
        originalProducts.Add(v);  
    }  
  
    // (Not shown) Bind the products list to a control  
    // and/or perform whatever processing is necessary.  
    }  
```  
  
### <a name="middle-tier-implementation"></a>中间层实现  

 下面的示例演示中间层上的接口方法的实现。 下面是要注意的两个要点：  
  
- <xref:System.Data.Linq.DataContext> 是在方法范围上声明的。  
  
- 该方法返回实际结果的 <xref:System.Collections.IEnumerable> 集合。 序列化程序将执行查询，以便将结果发送回客户端/表示层。 若要在中间层上对查询结果进行本地访问，可以通过对查询变量调用 `ToList` 或 `ToArray` 来强制执行。 然后，可以将该列表或数组作为 `IEnumerable` 返回。  
  
```vb  
Public Function GetProductsByCategory(ByVal categoryID As Integer) _  
    As IEnumerable(Of Product)  
  
    Dim db As New NorthwindClasses1DataContext(connectionString)  
    Dim productQuery = _  
    From prod In db.Products _  
    Where prod.CategoryID = categoryID _  
    Select prod  
  
    Return productQuery.AsEnumerable()  
  
End Function  
```  
  
```csharp  
public IEnumerable<Product> GetProductsByCategory(int categoryID)  
{  
    NorthwindClasses1DataContext db =
    new NorthwindClasses1DataContext(connectionString);  
  
    IEnumerable<Product> productQuery =  
    from prod in db.Products  
    where prod.CategoryID == categoryID  
    select prod;  
  
    return productQuery.AsEnumerable();
}  
```  
  
 数据上下文的实例应具有一个“工作单元”的生存期。 在松耦合环境中，工作单元通常较小，它可能是一个开放式事务，其中包含对 `SubmitChanges` 的单个调用。 因此，数据上下文在方法范围上创建和释放。 如果工作单元包含对业务规则逻辑的调用，则通常需要为整个操作保持 `DataContext` 实例。 在任何情况下，都不应该使 `DataContext` 实例在任意数量的事务之间长时间保持活动状态。  
  
 此方法会返回 Product 对象，但不会返回与每个 Product 相关联的 Order_Detail 对象的集合。 使用 <xref:System.Data.Linq.DataLoadOptions> 对象可以更改此默认行为。 有关详细信息，请参阅 [如何：控制检索的相关数据量](how-to-control-how-much-related-data-is-retrieved.md)。  
  
## <a name="inserting-data"></a>插入数据  

 为了插入新对象，表示层只是调用中间层接口上的相关方法，并传入要插入的新对象。 在某些情况下，对于客户端而言，仅传入一些值并让中间层来构造完整对象可能更加高效。  
  
### <a name="middle-tier-implementation"></a>中间层实现  

 在中间层上，创建一个新的 <xref:System.Data.Linq.DataContext>，使用 <xref:System.Data.Linq.DataContext> 方法将该对象附加到 <xref:System.Data.Linq.Table%601.InsertOnSubmit%2A>，并在调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 时插入该对象。 异常、回调和错误条件可以像在任何其他 Web 服务方案中一样进行处理。  
  
```vb  
' No call to Attach is necessary for inserts.  
Public Sub InsertOrder(ByVal o As Order)  
  
    Dim db As New NorthwindClasses1DataContext(connectionString)  
    db.Orders.InsertOnSubmit(o)  
  
    ' Exception handling not shown.  
    db.SubmitChanges()  
  
End Sub  
```  
  
```csharp  
// No call to Attach is necessary for inserts.  
    public void InsertOrder(Order o)  
    {  
        NorthwindClasses1DataContext db = new NorthwindClasses1DataContext(connectionString);  
        db.Orders.InsertOnSubmit(o);  
  
        // Exception handling not shown.  
        db.SubmitChanges();  
    }  
```  
  
## <a name="deleting-data"></a>删除数据  

 为了从数据库删除现有对象，表示层调用中间层接口上的相关方法，并传入要删除的对象的副本（其中包含该对象的原始值）。  
  
 删除操作涉及到开放式并发检查，并且必须首先将要删除的对象附加到新的数据上下文。 在此示例中，`Boolean` 参数设置为 `false`，以指示该对象没有时间戳 (RowVersion)。 如果数据库表确实为每个记录生成了时间戳，则并发检查会简单得多（特别是对客户端而言）。 只需传入原始对象或已修改的对象，并将 `Boolean` 参数设置为 `true`。 在任何情况下，通常都需要在中间层上捕捉 <xref:System.Data.Linq.ChangeConflictException>。 有关如何处理开放式并发冲突的详细信息，请参阅 [乐观 concurrency：概述](optimistic-concurrency-overview.md)。  
  
 如果要删除的实体具有对关联表的外键约束，则必须首先删除该实体的 <xref:System.Data.Linq.EntitySet%601> 集合中的所有对象。  
  
```vb  
' Attach is necessary for deletes.  
Public Sub DeleteOrder(ByVal order As Order)  
    Dim db As New NorthwindClasses1DataContext(connectionString)  
  
    db.Orders.Attach(order, False)  
    ' This will throw an exception if the order has order details.  
    db.Orders.DeleteOnSubmit(order)  
  
    Try  
        ' ConflictMode is an optional parameter.  
        db.SubmitChanges(ConflictMode.ContinueOnConflict)  
  
    Catch ex As ChangeConflictException  
        ' Get conflict information, and take actions  
        ' that are appropriate for your application.  
        ' See MSDN Article "How to: Manage Change  
        ' Conflicts (LINQ to SQL).  
  
    End Try  
End Sub  
```  
  
```csharp  
// Attach is necessary for deletes.  
public void DeleteOrder(Order order)  
{  
    NorthwindClasses1DataContext db = new NorthwindClasses1DataContext(connectionString);  
  
    db.Orders.Attach(order, false);  
    // This will throw an exception if the order has order details.  
    db.Orders.DeleteOnSubmit(order);  
    try  
    {  
        // ConflictMode is an optional parameter.  
        db.SubmitChanges(ConflictMode.ContinueOnConflict);  
    }  
    catch (ChangeConflictException e)  
    {  
       // Get conflict information, and take actions  
       // that are appropriate for your application.  
       // See MSDN Article How to: Manage Change Conflicts (LINQ to SQL).  
    }  
}  
```  
  
## <a name="updating-data"></a>更新数据  

 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 支持以下这些涉及开放式并发的方案中的更新：  
  
- 基于时间戳或 RowVersion 号的开放式并发。  
  
- 基于实体属性子集的原始值的开放式并发。  
  
- 基于完整原始实体和已修改实体的开放式并发。  
  
 还可以对实体及其关系（如一个 Customer 及其关联 Order 对象的集合）一起执行更新或删除。 如果在客户端上对实体对象及其子代 (`EntitySet`) 集合的关系图进行修改，并且开放式并发检查需要原始值，则客户端必须为每个实体和 <xref:System.Data.Linq.EntitySet%601> 对象提供这些原始值。 如果需要使客户端可以在单个方法调用中进行一组相关的更新、删除和插入操作，则必须为客户端提供一种相应的方式，以便指示要对每个实体执行的操作的类型。 然后，在调用 <xref:System.Data.Linq.ITable.Attach%2A> 之前，必须在中间层上为每个实体调用适当的 <xref:System.Data.Linq.ITable.InsertOnSubmit%2A> 方法，然后依次调用 <xref:System.Data.Linq.ITable.DeleteAllOnSubmit%2A>、<xref:System.Data.Linq.Table%601.InsertOnSubmit%2A> 或 `Attach`（对于插入操作，不调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A>）。 在尝试进行更新之前，不要将从数据库中检索数据作为一种获取原始值的方式。  
  
 有关开放式并发的详细信息，请参阅 [乐观 concurrency：概述](optimistic-concurrency-overview.md)。 有关解决开放式并发更改冲突的详细信息，请参阅 [如何：管理更改冲突](how-to-manage-change-conflicts.md)。  
  
 下面的示例演示每种方案：  
  
### <a name="optimistic-concurrency-with-timestamps"></a>使用时间戳的开放式并发  
  
```vb  
' Assume that "customer" has been sent by client.  
' Attach with "true" to say this is a modified entity  
' and it can be checked for optimistic concurrency  
' because it has a column that is marked with the  
' "RowVersion" attribute.  
  
db.Customers.Attach(customer, True)  
  
Try  
    ' Optional: Specify a ConflictMode value  
    ' in call to SubmitChanges.  
    db.SubmitChanges()  
Catch ex As ChangeConflictException  
    ' Handle conflict based on options provided.  
    ' See MSDN article "How to: Manage Change  
    ' Conflicts (LINQ to SQL)".  
End Try  
```  
  
```csharp  
// Assume that "customer" has been sent by client.  
// Attach with "true" to say this is a modified entity  
// and it can be checked for optimistic concurrency because  
//  it has a column that is marked with "RowVersion" attribute  
db.Customers.Attach(customer, true)  
try  
{  
    // Optional: Specify a ConflictMode value  
    // in call to SubmitChanges.  
    db.SubmitChanges();  
}  
catch(ChangeConflictException e)  
{  
    // Handle conflict based on options provided  
    // See MSDN article How to: Manage Change Conflicts (LINQ to SQL).  
}  
```  
  
### <a name="with-subset-of-original-values"></a>使用原始值子集的开放式并发  

 在此方法中，客户端返回完整的序列化对象以及要修改的值。  
  
```vb  
Public Sub UpdateProductInventory(ByVal p As Product, ByVal _  
    unitsInStock As Short?, ByVal unitsOnOrder As Short?)  
  
    Using db As New NorthwindClasses1DataContext(connectionString)  
        ' p is the original unmodified product  
        ' that was obtained from the database.  
        ' The client kept a copy and returns it now.  
        db.Products.Attach(p, False)  
  
        ' Now that the original values are in the data context,  
        ' apply the changes.  
        p.UnitsInStock = unitsInStock  
        p.UnitsOnOrder = unitsOnOrder  
  
        Try  
            ' Optional: Specify a ConflictMode value  
            ' in call to SubmitChanges.  
            db.SubmitChanges()  
  
        Catch ex As Exception  
            ' Handle conflict based on options provided.  
            ' See MSDN article "How to: Manage Change Conflicts  
            ' (LINQ to SQL)".  
        End Try  
    End Using  
End Sub  
```  
  
```csharp  
public void UpdateProductInventory(Product p, short? unitsInStock, short? unitsOnOrder)  
{  
    using (NorthwindClasses1DataContext db = new NorthwindClasses1DataContext(connectionString))  
    {  
        // p is the original unmodified product  
        // that was obtained from the database.  
        // The client kept a copy and returns it now.  
        db.Products.Attach(p, false);  
  
        // Now that the original values are in the data context, apply the changes.  
        p.UnitsInStock = unitsInStock;  
        p.UnitsOnOrder = unitsOnOrder;  
        try  
        {  
             // Optional: Specify a ConflictMode value  
             // in call to SubmitChanges.  
             db.SubmitChanges();  
        }  
        catch (ChangeConflictException e)  
        {  
            // Handle conflict based on provided options.  
            // See MSDN article How to: Manage Change Conflicts  
            // (LINQ to SQL).  
        }  
    }  
}  
```  
  
### <a name="with-complete-entities"></a>使用完整实体的开放式并发  
  
```vb  
Public Sub UpdateProductInfo(ByVal newProd As Product, ByVal _  
    originalProd As Product)  
  
    Using db As New NorthwindClasses1DataContext(connectionString)  
        db.Products.Attach(newProd, originalProd)  
  
        Try  
            ' Optional: Specify a ConflictMode value  
            ' in call to SubmitChanges.  
            db.SubmitChanges()  
  
        Catch ex As Exception  
            ' Handle potential change conflicgt in whatever way  
            ' is appropriate for your application.  
            ' For more information, see the MSDN article  
            ' "How to: Manage Change Conflicts (LINQ to  
            ' SQL)".  
        End Try  
  
    End Using  
End Sub  
```  
  
```csharp  
public void UpdateProductInfo(Product newProd, Product originalProd)  
{  
     using (NorthwindClasses1DataContext db = new  
        NorthwindClasses1DataContext(connectionString))  
     {  
         db.Products.Attach(newProd, originalProd);  
         try  
         {  
               // Optional: Specify a ConflictMode value  
               // in call to SubmitChanges.  
               db.SubmitChanges();  
         }  
        catch (ChangeConflictException e)  
        {  
            // Handle potential change conflict in whatever way  
            // is appropriate for your application.  
            // For more information, see the MSDN article  
            // How to: Manage Change Conflicts (LINQ to SQL)/  
        }
    }  
}  
```  
  
 若要更新集合，请调用 <xref:System.Data.Linq.ITable.AttachAll%2A> 而不是 `Attach`。  
  
### <a name="expected-entity-members"></a>期望的实体成员  

 如前所述，在调用 `Attach` 方法之前，只需设置实体对象的特定成员。 需要设置的实体成员必须满足以下条件：  
  
- 属于实体的标识。  
  
- 需要进行修改。  
  
- 为时间戳，或将其 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 属性设置为除 `Never` 之外的某个值。  
  
 如果某个表使用时间戳或版本号进行开放式并发检查，则在调用 <xref:System.Data.Linq.ITable.Attach%2A> 之前必须设置这些成员。 当该 Column 属性 (Attribute) 上的 <xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A> 属性 (Property) 设置为 true 时，相应的成员将专门用于进行开放式并发检查。 只有当数据库具有相同的版本号或时间戳值时，才会提交所请求的任何更新。  
  
 只要成员的 <xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A> 未设置为 `Never`，那么也会在开放式并发检查中使用该成员。 如果未指定其他值，则默认值为 `Always`。  
  
 如果缺少这些必需成员中的任何一个成员，都会在 <xref:System.Data.Linq.ChangeConflictException> 期间引发 <xref:System.Data.Linq.DataContext.SubmitChanges%2A>（“找不到行或行已更改”）。  
  
### <a name="state"></a>状态  

 在将一个实体对象附加到 <xref:System.Data.Linq.DataContext> 实例之后，会将该对象视为处于 `PossiblyModified` 状态。 可通过三种方式强制所附加的对象被视为 `Modified`。  
  
1. 将对象作为未修改的对象进行附加，然后直接修改其字段。  
  
2. 使用接受当前对象实例和原始对象实例作为参数的 <xref:System.Data.Linq.Table%601.Attach%2A> 重载附加对象。 这会向更改跟踪程序提供旧值和新值，以便跟踪程序自动了解哪些字段已更改。  
  
3. 使用接受另一个布尔型参数（设置为 true）的 <xref:System.Data.Linq.Table%601.Attach%2A> 重载附加对象。 这将告诉更改跟踪程序将该对象视为已修改的对象，而无需提供任何原始值。 在此方式中，对象必须具有一个版本/时间戳字段。  
  
 有关详细信息，请参阅 [对象状态和更改跟踪](object-states-and-change-tracking.md)。  
  
 如果 ID 缓存中已存在一个实体对象且该对象具有与要附加的对象相同的标识，则会引发 <xref:System.Data.Linq.DuplicateKeyException>。  
  
 在附加一组 `IEnumerable` 对象时，将在出现已存在的键时引发 <xref:System.Data.Linq.DuplicateKeyException>。 剩余的对象将不会附加。  
  
## <a name="see-also"></a>请参阅

- [N 层和远程应用程序与 LINQ to SQL](n-tier-and-remote-applications-with-linq-to-sql.md)
- [背景信息](background-information.md)
