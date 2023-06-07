---
title: 实现业务逻辑 (LINQ to SQL)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c4577590-7b12-42e1-84a6-95aa2562727e
ms.openlocfilehash: 3d703f7a13f21947fb5d2b5adcac8ae4df6a1547
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158365"
---
# <a name="implementing-business-logic-linq-to-sql"></a>实现业务逻辑 (LINQ to SQL)

本主题中的术语“业务逻辑”指的是在对数据库数据进行插入、更新或删除操作之前，应用于数据的任何自定义规则或验证测试。 业务逻辑有时也称为“业务规则”或“域逻辑”。 在 n 层应用程序中，业务逻辑通常设计为逻辑层，因此可以独立于表示层或数据访问层进行修改。 在对数据库数据进行任何更新、插入或删除操作前后，数据访问层可以调用业务逻辑。  
  
 业务逻辑可以和架构验证一样简单，以确保字段类型与表列类型兼容。 它也可以包含一组以任意复杂方式进行交互的对象。 这些规则可以作为数据库上的存储过程或内存中的对象来实现。 无论通过何种方式实现业务逻辑，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 都允许您使用分部类和分部方法，将业务逻辑与数据访问代码分开。  
  
## <a name="how-linq-to-sql-invokes-your-business-logic"></a>LINQ to SQL 如何调用业务逻辑  

 当你在设计时（手动或通过使用对象关系设计器或 SQLMetal）生成实体类时，它将定义为一个分部类。 这意味着，在单独的代码文件中，可以定义包含自定义业务逻辑的另一部分实体类。 在编译时，这两个部分将合并成一个类。 但是，如果必须通过使用对象关系设计器或 SQLMetal 来重新生成实体类，则可以这样做，并且不会修改类的一部分。  
  
 定义实体和 <xref:System.Data.Linq.DataContext> 的分部类包含分部方法。 这些是扩展性点，可以在进行任何更新、插入或删除前后用于对实体或实体属性应用业务逻辑。 分部方法可以视为编译时事件。 代码生成器定义方法签名，并在 get 和 set 属性访问器、`DataContext` 构造函数中调用这些方法，有些情况下还在调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 时在后台调用方法。 但是，如果未实现特殊的分部方法，那么在编译时将移除对该分部方法的所有引用和定义。  
  
 在你在单独的代码文件中编写的实现定义中，可以执行所需的任何自定义逻辑。 可以将分部类本身用作域层，也可以从分部方法的实现定义，将分部类调入单独对象或多个对象。 无论采用何种方式，业务逻辑都将与数据访问代码和表示层代码完全分开。  
  
## <a name="a-closer-look-at-the-extensibility-points"></a>详细了解扩展性点  

 下面的示例演示了由对象关系设计器为类生成的代码的一部分 `DataContext` ，其中包含两个表： `Customers` 和 `Orders` 。 注意为该类的每个表都定义了插入、更新和删除方法。  
  
```vb  
Partial Public Class Northwnd  
    Inherits System.Data.Linq.DataContext  
  
    Private Shared mappingSource As _  
        System.Data.Linq.Mapping.MappingSource = New _  
        AttributeMappingSource  
  
    #Region "Extensibility Method Definitions"  
    Partial Private Sub OnCreated()  
    End Sub  
    Partial Private Sub InsertCustomer(instance As Customer)  
    End Sub  
    Partial Private Sub UpdateCustomer(instance As Customer)  
    End Sub  
    Partial Private Sub DeleteCustomer(instance As Customer)  
    End Sub  
    Partial Private Sub InsertOrder(instance As [Order])  
    End Sub  
    Partial Private Sub UpdateOrder(instance As [Order])  
    End Sub  
    Partial Private Sub DeleteOrder(instance As [Order])  
    End Sub  
    #End Region  
```  
  
```csharp  
public partial class MyNorthWindDataContext : System.Data.Linq.DataContext  
    {  
        private static System.Data.Linq.Mapping.MappingSource mappingSource = new AttributeMappingSource();  
  
        #region Extensibility Method Definitions  
        partial void OnCreated();  
        partial void InsertCustomer(Customer instance);  
        partial void UpdateCustomer(Customer instance);  
        partial void DeleteCustomer(Customer instance);  
        partial void InsertOrder(Order instance);  
        partial void UpdateOrder(Order instance);  
        partial void DeleteOrder(Order instance);  
        #endregion  
```  
  
 如果在分部类中实现插入、更新和删除方法，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 运行时将在调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 时调用这些方法，而不是自己的默认方法。 这使您能够重写创建/读取/更新/删除操作的默认行为。 有关详细信息，请参阅 [演练：自定义实体类的插入、更新和删除行为](/visualstudio/data-tools/walkthrough-customizing-the-insert-update-and-delete-behavior-of-entity-classes)。  
  
 `OnCreated` 方法在类构造函数中调用。  
  
```vb  
Public Sub New(ByVal connection As String)  
    MyBase.New(connection, mappingSource)  
    OnCreated()  
End Sub  
```  
  
```csharp  
public MyNorthWindDataContext(string connection) :  
            base(connection, mappingSource)  
        {  
            OnCreated();  
        }  
```  
  
 此实体类有三个方法，当创建、加载和验证实体时（调用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 时），`SubmitChanges` 运行时会调用这些方法。 此实体类还为每个属性提供两个分部方法，一个在设置属性前调用，另一个在设置属性后调用。 下面的代码示例显示为 `Customer` 类生成的一些方法：  
  
```vb  
#Region "Extensibility Method Definitions"  
    Partial Private Sub OnLoaded()  
    End Sub  
    Partial Private Sub OnValidate(action As _  
        System.Data.Linq.ChangeAction)  
    End Sub  
    Partial Private Sub OnCreated()  
    End Sub  
    Partial Private Sub OnCustomerIDChanging(value As String)  
    End Sub  
    Partial Private Sub OnCustomerIDChanged()  
    End Sub  
    Partial Private Sub OnCompanyNameChanging(value As String)  
    End Sub  
    Partial Private Sub OnCompanyNameChanged()  
    End Sub  
' ...Additional Changing/Changed methods for each property.  
```  
  
```csharp  
#region Extensibility Method Definitions  
    partial void OnLoaded();  
    partial void OnValidate();  
    partial void OnCreated();  
    partial void OnCustomerIDChanging(string value);  
    partial void OnCustomerIDChanged();  
    partial void OnCompanyNameChanging(string value);  
    partial void OnCompanyNameChanged();  
// ...additional Changing/Changed methods for each property  
```  
  
 在属性 set 访问器中调用这些方法，如下面的 `CustomerID` 属性示例中所示。  
  
```vb  
Public Property CustomerID() As String  
    Set  
        If (String.Equals(Me._CustomerID, value) = False) Then  
            Me.OnCustomerIDChanging(value)  
            Me.SendPropertyChanging()  
            Me._CustomerID = value  
            Me.SendPropertyChanged("CustomerID")  
            Me.OnCustomerIDChanged()  
        End If  
    End Set  
End Property  
```  
  
```csharp  
public string CustomerID  
{  
    set  
    {  
        if ((this._CustomerID != value))  
        {  
            this.OnCustomerIDChanging(value);  
            this.SendPropertyChanging();  
            this._CustomerID = value;  
            this.SendPropertyChanged("CustomerID");  
            this.OnCustomerIDChanged();  
        }  
     }  
}  
```  
  
 在类的自定义部分，编写方法的实现定义。 在 Visual Studio 中，键入后 `partial` 将在类的其他部分中看到方法定义的 IntelliSense。  
  
```vb  
Partial Public Class Customer  
    Private Sub OnCustomerIDChanging(value As String)  
        ' Perform custom validation logic here.  
    End Sub  
End Class  
```  
  
```csharp  
partial class Customer
    {  
        partial void OnCustomerIDChanging(string value)  
        {  
            //Perform custom validation logic here.  
        }  
    }  
```  
  
 有关如何使用分部方法向应用程序添加业务逻辑的更多信息，请参见下列主题：  
  
 [如何：向实体类添加验证](/visualstudio/data-tools/how-to-add-validation-to-entity-classes)  
  
 [演练：自定义实体类的插入、更新和删除行为](/visualstudio/data-tools/walkthrough-customizing-the-insert-update-and-delete-behavior-of-entity-classes)  
  
 [演练：向实体类添加验证](/previous-versions/visualstudio/visual-studio-2013/bb629301(v=vs.120))  
  
## <a name="see-also"></a>请参阅

- [分部类和方法](../../../../../csharp/programming-guide/classes-and-structs/partial-classes-and-methods.md)
- [分部方法](../../../../../visual-basic/programming-guide/language-features/procedures/partial-methods.md)
- [Visual Studio 中的 LINQ to SQL 工具](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)
- [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)
