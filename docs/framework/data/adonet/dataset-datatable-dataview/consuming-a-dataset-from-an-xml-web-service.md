---
title: 使用 XML Web Services 中的数据集
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
ms.assetid: 9edd6b71-0fa5-4649-ae1d-ac1c12541019
ms.openlocfilehash: d4f4d5d34698fbb9a7986f4628b282d4425da3f7
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90554692"
---
# <a name="consume-a-dataset-from-an-xml-web-service"></a>使用 XML web services 中的数据集

<xref:System.Data.DataSet> 是用断开式设计来构建的，其部分目的是为了便于通过 Internet 来传输数据。 **数据集**是可序列化的，因为可以将其指定为 XML Web services 的输入或从 XML Web services 输出，而无需进行任何其他编码即可将**数据集**的内容从 XML Web service 流式传输到客户端和返回到客户端。 **数据集**使用 DiffGram 格式隐式转换为 XML 流，并通过网络发送，然后作为接收端上的**数据集**从 xml 流重新构造。 这为你提供了一种简单而灵活的方法，用于通过 XML Web services 传输和返回关系数据。 有关 DiffGram 格式的详细信息，请参阅 [diffgram](diffgrams.md)。  
  
 下面的示例演示如何创建使用 **数据集** 传输关系数据的 XML Web service 和客户端 (包括已修改的数据) 并将所有更新解析回原始数据源。  
  
> [!NOTE]
> `DataSet` `DataTable` 如果输入不受信任，则作为 XML Web service 调用的一部分传输或实例是不安全的。 有关详细信息，请参阅 [数据集和 DataTable 安全指南](security-guidance.md)。
> 我们还建议你在创建 XML Web service 时始终考虑安全问题。 有关保护 XML Web service 的信息，请参阅 [保护使用 ASP.NET 创建的 XML Web Services](/previous-versions/dotnet/netframework-4.0/w67h0dw7(v=vs.100))。  
  
## <a name="create-an-xml-web-service"></a>创建 XML web services
  
1. 创建 XML Web services。  
  
     在此示例中，创建了一个用于返回数据的 XML Web service，在本例中为 **Northwind** 数据库中的客户列表，接收到数据的更新的数据 **集** ，XML Web service 解析回原始数据源。  
  
     XML Web service 公开两个方法： **GetCustomers**，用于返回客户列表和 **UpdateCustomers**，以将更新解析回数据源。 XML Web services 存储在 Web 服务器上名为 DataSetSample.asmx 的文件中。 下面的代码概括了 DataSetSample.asmx 的内容。  
  
    ```vb  
    <% @ WebService Language = "vb" Class = "Sample" %>  
    Imports System  
    Imports System.Data  
    Imports System.Data.SqlClient  
    Imports System.Web.Services  
  
    <WebService(Namespace:="http://microsoft.com/webservices/")> _  
    Public Class Sample  
  
    Public connection As SqlConnection = New SqlConnection("Data Source=(local);Integrated Security=SSPI;Initial Catalog=Northwind")  
  
      <WebMethod( Description := "Returns Northwind Customers", EnableSession := False )> _  
      Public Function GetCustomers() As DataSet  
        Dim adapter As SqlDataAdapter = New SqlDataAdapter( _  
          "SELECT CustomerID, CompanyName FROM Customers", connection)  
  
        Dim custDS As DataSet = New DataSet()  
        adapter.MissingSchemaAction = MissingSchemaAction.AddWithKey  
        adapter.Fill(custDS, "Customers")  
  
        Return custDS  
      End Function  
  
      <WebMethod( Description := "Updates Northwind Customers", EnableSession := False )> _  
      Public Function UpdateCustomers(custDS As DataSet) As DataSet  
        Dim adapter As SqlDataAdapter = New SqlDataAdapter()  
  
        adapter.InsertCommand = New SqlCommand( _  
          "INSERT INTO Customers (CustomerID, CompanyName) " & _  
          "Values(@CustomerID, @CompanyName)", connection)  
        adapter.InsertCommand.Parameters.Add( _  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID")  
        adapter.InsertCommand.Parameters.Add( _  
          "@CompanyName", SqlDbType.NChar, 15, "CompanyName")  
  
        adapter.UpdateCommand = New SqlCommand( _  
          "UPDATE Customers Set CustomerID = @CustomerID, " & _  
          "CompanyName = @CompanyName WHERE CustomerID = " & _  
          @OldCustomerID", connection)  
        adapter.UpdateCommand.Parameters.Add( _  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID")  
        adapter.UpdateCommand.Parameters.Add( _  
          "@CompanyName", SqlDbType.NChar, 15, "CompanyName")  
  
        Dim parameter As SqlParameter = _  
          adapter.UpdateCommand.Parameters.Add( _  
          "@OldCustomerID", SqlDbType.NChar, 5, "CustomerID")  
        parameter.SourceVersion = DataRowVersion.Original  
  
        adapter.DeleteCommand = New SqlCommand( _  
          "DELETE FROM Customers WHERE CustomerID = @CustomerID", _  
          connection)  
        parameter = adapter.DeleteCommand.Parameters.Add( _  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID")  
        parameter.SourceVersion = DataRowVersion.Original  
  
        adapter.Update(custDS, "Customers")  
  
        Return custDS  
      End Function  
    End Class  
    ```  
  
    ```csharp  
    <% @ WebService Language = "C#" Class = "Sample" %>  
    using System;  
    using System.Data;  
    using System.Data.SqlClient;  
    using System.Web.Services;  
  
    [WebService(Namespace="http://microsoft.com/webservices/")]  
    public class Sample  
    {  
      public SqlConnection connection = new SqlConnection("Data Source=(local);Integrated Security=SSPI;Initial Catalog=Northwind");  
  
      [WebMethod( Description = "Returns Northwind Customers", EnableSession = false )]  
      public DataSet GetCustomers()  
      {  
        SqlDataAdapter adapter = new SqlDataAdapter(  
          "SELECT CustomerID, CompanyName FROM Customers", connection);  
  
        DataSet custDS = new DataSet();  
        adapter.MissingSchemaAction = MissingSchemaAction.AddWithKey;  
        adapter.Fill(custDS, "Customers");  
  
        return custDS;  
      }  
  
      [WebMethod( Description = "Updates Northwind Customers",  
        EnableSession = false )]  
      public DataSet UpdateCustomers(DataSet custDS)  
      {  
        SqlDataAdapter adapter = new SqlDataAdapter();  
  
        adapter.InsertCommand = new SqlCommand(  
          "INSERT INTO Customers (CustomerID, CompanyName) " +  
          "Values(@CustomerID, @CompanyName)", connection);  
        adapter.InsertCommand.Parameters.Add(  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID");  
        adapter.InsertCommand.Parameters.Add(  
          "@CompanyName", SqlDbType.NChar, 15, "CompanyName");  
  
        adapter.UpdateCommand = new SqlCommand(  
          "UPDATE Customers Set CustomerID = @CustomerID, " +  
          "CompanyName = @CompanyName WHERE CustomerID = " +  
          "@OldCustomerID", connection);  
        adapter.UpdateCommand.Parameters.Add(  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID");  
        adapter.UpdateCommand.Parameters.Add(  
          "@CompanyName", SqlDbType.NChar, 15, "CompanyName");  
        SqlParameter parameter = adapter.UpdateCommand.Parameters.Add(  
          "@OldCustomerID", SqlDbType.NChar, 5, "CustomerID");  
        parameter.SourceVersion = DataRowVersion.Original;  
  
        adapter.DeleteCommand = new SqlCommand(  
        "DELETE FROM Customers WHERE CustomerID = @CustomerID",  
         connection);  
        parameter = adapter.DeleteCommand.Parameters.Add(  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID");  
        parameter.SourceVersion = DataRowVersion.Original;  
  
        adapter.Update(custDS, "Customers");  
  
        return custDS;  
      }  
    }  
    ```  
  
     在典型方案中，将编写 **UpdateCustomers** 方法来捕获开放式并发冲突。 为了简单起见，以上示例没有包含此方法。 有关开放式并发的详细信息，请参阅 [乐观并发](../optimistic-concurrency.md)。  
  
2. 创建 XML Web services 代理。  
  
     XML Web services 的客户端将需要通过 SOAP 代理来使用公开的方法。 可以使用 Visual Studio 生成此代理。 通过从 Visual Studio 中设置对现有 Web 服务的 Web 引用，此步骤中所述的所有行为均将透明地发生。 如果要自己创建代理类，请继续阅读本讨论。 但是，在大多数情况下，使用 Visual Studio 足以为客户端应用程序创建代理类。  
  
     代理可以使用 Web 服务描述语言工具来创建。 例如，如果在 URL 处公开了 XML Web service，则 `http://myserver/data/DataSetSample.asmx` 发出如下所示的命令，以 Visual Basic 创建一个 DSSample 命名空间为 **WebData** 的 .net 代理，并将其存储在文件中。  
  
    ```console
    wsdl /l:VB -out:sample.vb http://myserver/data/DataSetSample.asmx /n:WebData.DSSample  
    ```  
  
     若要在文件 sample.cs 中创建一个 C# 代理，请发出以下命令。  
  
    ```console
    wsdl -l:CS -out:sample.cs http://myserver/data/DataSetSample.asmx -n:WebData.DSSample  
    ```  
  
     然后，可以将该代理编译为库并导入 XML Web services 客户端。 若要将 sample.vb 中存储的 Visual Basic .NET 代理代码编译为 sample.dll，请发出以下命令。  
  
    ```console  
    vbc -t:library -out:sample.dll sample.vb -r:System.dll -r:System.Web.Services.dll -r:System.Data.dll -r:System.Xml.dll  
    ```  
  
     若要将 sample.cs 中存储的 C# 代理代码编译为 sample.dll，请发出以下命令。  
  
    ```console
    csc -t:library -out:sample.dll sample.cs -r:System.dll -r:System.Web.Services.dll -r:System.Data.dll -r:System.Xml.dll  
    ```  
  
3. 创建 XML Web services 客户端。  
  
     如果想让 Visual Studio 为你生成 Web 服务代理类，只需创建客户端项目，然后在 "解决方案资源管理器" 窗口中右键单击该项目，然后选择 "**添加**  >  **服务引用**"。 在 " **添加服务引用** " 对话框中，选择 " **高级**"，然后选择 " **添加 Web 引用**"。 从可用 Web 服务的列表中选择 Web 服务 (如果 Web 服务在当前解决方案或当前计算机上不可用，则这可能需要提供 Web 服务终结点的地址) 。 如果自己创建 XML Web services 代理（按照前面的步骤所述），可以将其导入客户端代码并使用 XML Web services 方法。

     下面的示例代码导入代理库，调用**GetCustomers**以获取客户列表，添加新客户，然后返回**UpdateCustomers**更新的**数据集**。  
  
     该示例将**GetChanges**返回的**数据集**传递给**UpdateCustomers** ，因为只有修改的行需要传递到**UpdateCustomers**。 **UpdateCustomers** 返回已解析的 **数据集**，然后你可以将其 **合并** 到现有的 **数据集中** ，以合并来自更新的已解决更改和任何行错误信息。 以下代码假定你已使用 Visual Studio 创建 Web 引用，并且已在 " **添加 Web 引用** " 对话框中将 web 引用重命名为 "DsSample"。  
  
    ```vb  
    Imports System  
    Imports System.Data  
  
    Public Class Client  
  
      Public Shared Sub Main()  
        Dim proxySample As New DsSample.Sample ()  ' Proxy object.  
        Dim customersDataSet As DataSet = proxySample.GetCustomers()  
        Dim customersTable As DataTable = _  
          customersDataSet.Tables("Customers")  
  
        Dim rowAs DataRow = customersTable.NewRow()  
        row("CustomerID") = "ABCDE"  
        row("CompanyName") = "New Company Name"  
        customersTable.Rows.Add(row)  
  
        Dim updateDataSet As DataSet = _  
          proxySample.UpdateCustomers(customersDataSet.GetChanges())  
  
        customersDataSet.Merge(updateDataSet)  
        customersDataSet.AcceptChanges()  
      End Sub  
    End Class  
    ```  
  
    ```csharp  
    using System;  
    using System.Data;  
  
    public class Client  
    {  
      public static void Main()  
      {  
        Sample proxySample = new DsSample.Sample();  // Proxy object.  
        DataSet customersDataSet = proxySample.GetCustomers();  
        DataTable customersTable = customersDataSet.Tables["Customers"];  
  
        DataRow row = customersTable.NewRow();  
        row["CustomerID"] = "ABCDE";  
        row["CompanyName"] = "New Company Name";  
        customersTable.Rows.Add(row);  
  
        DataSet updateDataSet = new DataSet();  
  
        updateDataSet =
          proxySample.UpdateCustomers(customersDataSet.GetChanges());  
  
        customersDataSet.Merge(updateDataSet);  
        customersDataSet.AcceptChanges();  
      }  
    }  
    ```  
  
     如果决定自己创建代理类，必须执行下列额外的步骤。 若要编译该示例，将需要提供已创建的代理库 (sample.dll) 和相关的 .NET 库。 若要编译该示例的 Visual Basic .NET 版本（存储在文件 client.vb 中），请发出以下命令。  
  
    ```console
    vbc client.vb -r:sample.dll -r:System.dll -r:System.Data.dll -r:System.Xml.dll -r:System.Web.Services.dll  
    ```  
  
     若要编译该示例的 C# 版本（存储在文件 client.cs 中），请发出以下命令。  
  
    ```console
    csc client.cs -r:sample.dll -r:System.dll -r:System.Data.dll -r:System.Xml.dll -r:System.Web.Services.dll  
    ```  
  
## <a name="see-also"></a>请参阅

- [ADO.NET](../index.md)
- [数据集、数据表和数据视图](index.md)
- [DataTables](datatables.md)
- [从 DataAdapter 填充数据集](../populating-a-dataset-from-a-dataadapter.md)
- [使用 DataAdapter 更新数据源](../updating-data-sources-with-dataadapters.md)
- [DataAdapter 参数](../dataadapter-parameters.md)
- [Web 服务描述语言工具 ( # A0) ](/previous-versions/dotnet/netframework-4.0/7h3ystb6(v=vs.100))
- [ADO.NET 概述](../ado-net-overview.md)
