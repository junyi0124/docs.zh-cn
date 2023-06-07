---
title: 数据集和 DataTable 安全指南
ms.date: 07/14/2020
dev_langs:
- csharp
ms.openlocfilehash: 8798c4542acc578c8f7f00c9b26cd01a0db20c42
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726062"
---
# <a name="dataset-and-datatable-security-guidance"></a>数据集和 DataTable 安全指南

本文适用于：

* .NET Framework（所有版本）
* .NET Core 和更高版本
* .NET 5.0 及更高版本

[数据集](/dotnet/api/system.data.dataset)和[DataTable](/dotnet/api/system.data.datatable)类型是旧的 .net 组件，可将数据集表示为托管对象。 这些组件在 .NET Framework 1.0 中引入，作为原始 [ADO.NET 基础结构](./index.md)的一部分。 其目标是通过关系数据集提供托管视图，并将数据的基础数据源抽象到 XML、SQL 或其他技术。

有关 ADO.NET 的详细信息，包括更多新式数据视图模式，请参阅 [ADO.NET 文档](../index.md)。

## <a name="default-restrictions-when-deserializing-a-dataset-or-datatable-from-xml"></a>从 XML 反序列化数据集或 DataTable 时的默认限制

在所有受支持版本的 .NET Framework、.NET Core 和 .NET 上， `DataSet` 并对 `DataTable` 可能存在于反序列化数据中的对象类型施加以下限制。 默认情况下，此列表限制为：

* 基元和基元等效项：、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、、 `bool` `char` `sbyte` `byte` `short` `ushort` `int` `uint` `long` `ulong` `float` `double` `decimal` `DateTime` `DateTimeOffset` `TimeSpan` `string` `Guid` `SqlBinary` `SqlBoolean` `SqlByte` `SqlBytes` `SqlChars` `SqlDateTime` `SqlDecimal` `SqlDouble` `SqlGuid` `SqlInt16` `SqlInt32` `SqlInt64` `SqlMoney` `SqlSingle` 和 `SqlString` 。
* 常用的非基元： `Type` 、 `Uri` 和 `BigInteger` 。
* 常用的 _系统_ 类型： `Color` 、 `Point` 、 `PointF` 、、、 `Rectangle` `RectangleF` `Size` 和 `SizeF` 。
* `Enum` 各种.
* 上述类型的数组和列表。

如果传入的 XML 数据包含其类型不在此列表中的对象：

* 使用以下消息和堆栈跟踪引发异常。
此处不允许出现错误消息： "InvalidOperationException： Type" \<Type Name\> 、Version = \<n.n.n.n\> 、Culture = \<culture\> 、PublicKeyToken = \<token value\> '。 [https://go.microsoft.com/fwlink/?linkid=2132227](https://go.microsoft.com/fwlink/?linkid=2132227)有关更多详细信息，请参阅。
Stack 跟踪：在 TypeLimiter. EnsureTypeIsAllowed (Type type，TypeLimiter capturedLimiter) 在 (Type，UpdateColumnType StorageType) ，System.Data.DataColumn.set_DataType (类型值) 

* 反序列化操作失败。

将 XML 加载到现有的 `DataSet` 或 `DataTable` 实例时，也会考虑现有的列定义。 如果表已包含自定义类型的列定义，则在 XML 反序列化操作期间，该类型暂时添加到允许列表中。

> [!NOTE]
> 向添加列后 `DataTable` ， `ReadXml` 将不会从 XML 读取架构，如果架构不匹配，也不会读取记录，因此，你需要自行添加所有列以使用此方法。

```csharp
XmlReader xmlReader = GetXmlReader();

// Assume the XML blob contains data for type MyCustomClass.
// The following call to ReadXml fails because MyCustomClass isn't in the allowed types list.

DataTable table = new DataTable("MyDataTable");
table.ReadXml(xmlReader);

// However, the following call to ReadXml succeeds, since the DataTable instance
// already defines a column of type MyCustomClass.

DataTable table = new DataTable("MyDataTable");
table.Columns.Add("MyColumn", typeof(MyCustomClass));
table.ReadXml(xmlReader); // this call will succeed
```

当使用 `XmlSerializer` 反序列化或的实例时，对象类型限制也适用 `DataSet` `DataTable` 。 但是，当使用 `BinaryFormatter` 反序列化或的实例时，它们可能不适用 `DataSet` `DataTable` 。

使用时，不应用对象类型限制 `DataAdapter.Fill` ，例如，在 `DataTable` 不使用 XML 反序列化 api 的情况下直接从数据库填充实例时。

### <a name="extend-the-list-of-allowed-types"></a>扩展允许的类型列表

除了上面列出的内置类型外，应用程序还可以扩展允许的类型列表以包括自定义类型。 如果扩展允许的类型列表，则更改会 _all_ 影响 `DataSet` `DataTable` 应用内的所有和实例。 不能从内置允许类型列表中删除类型。

#### <a name="extend-through-configuration-net-framework-40---48"></a>通过配置 ( 扩展 .NET Framework 4.0-4.8) 

_App.config_ 可用于扩展允许的类型列表。 扩展允许的类型列表：

* 使用 `<configSections>` 元素可添加对 " _system.object_ 配置" 部分的引用。
* 使用 `<system.data.dataset.serialization>` / `<allowedTypes>` 指定其他类型。

每个 `<add>` 元素只能使用其程序集限定类型名称指定一种类型。 若要将其他类型添加到 "允许的类型" 列表中，请使用多个 `<add>` 元素。

下面的示例演示如何通过添加自定义类型来扩展允许的类型列表 `Fabrikam.CustomType` 。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <sectionGroup name="system.data.dataset.serialization" type="System.Data.SerializationSettingsSectionGroup, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">
      <section name="allowedTypes" type="System.Data.AllowedTypesSectionHandler, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
    </sectionGroup>
  </configSections>
  <system.data.dataset.serialization>
    <allowedTypes>
      <!-- <add type="assembly qualified type name" /> -->
      <add type="Fabrikam.CustomType, Fabrikam, Version=1.0.0.0, Culture=neutral, PublicKeyToken=2b3831f2f2b744f7" />
      <!-- additional <add /> elements as needed -->
    </allowedTypes>
  </system.data.dataset.serialization>
</configuration>
```

若要检索某个类型的程序集限定名称，请使用 [AssemblyQualifiedName](/dotnet/api/system.type.assemblyqualifiedname) 属性，如以下代码所示。

```csharp
string assemblyQualifiedName = typeof(Fabrikam.CustomType).AssemblyQualifiedName;
```

<a name="etc"></a>

#### <a name="extend-through-configuration-net-framework-20---35"></a>通过配置 ( 扩展 .NET Framework 2.0-3.5) 

如果你的应用面向 .NET Framework 2.0 或3.5，你仍可以使用上述 _App.config_ 机制来扩展允许的类型列表。 但是，你 `<configSections>` 的元素看上去将略有不同，如以下代码所示：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <!-- The below <sectionGroup> and <section> are specific to .NET Framework 2.0 and 3.5. -->
    <sectionGroup name="system.data.dataset.serialization" type="System.Data.SerializationSettingsSectionGroup, System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">
      <section name="allowedTypes" type="System.Data.AllowedTypesSectionHandler, System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
    </sectionGroup>
  </configSections>
  <system.data.dataset.serialization>
    <allowedTypes>
      <!-- <add /> elements, as demonstrated in the .NET Framework 4.0 - 4.8 sample code above. -->
    </allowedTypes>
  </system.data.dataset.serialization>
</configuration>
```

#### <a name="extend-programmatically-net-framework-net-core-net-50"></a>以编程方式扩展 ( .NET Framework，.NET Core，.NET 5.0 +) 

还可以通过将 [DataSetDefaultAllowedTypes 与众所周知的密钥](/dotnet/api/system.appdomain.setdata)_系统_ 一起使用，以编程方式扩展允许的类型列表，如下面的代码所示。

```csharp
Type[] extraAllowedTypes = new Type[]
{
    typeof(Fabrikam.CustomType),
    typeof(Contoso.AdditionalCustomType)
};

AppDomain.CurrentDomain.SetData("System.Data.DataSetDefaultAllowedTypes", extraAllowedTypes);
```

如果使用扩展机制，则与密钥 _DataSetDefaultAllowedTypes_ 关联的值的类型必须为 `Type[]` 。

在 .NET Framework 上，可以使用 _App.config_ 和来扩展允许的类型列表 `AppDomain.SetData` 。 在这种情况下， `DataSet` 和 `DataTable` 将允许将对象作为数据的一部分进行反序列化（如果其类型存在于任一列表中）。

### <a name="run-an-app-in-audit-mode-net-framework"></a>在审核模式下运行应用 ( .NET Framework) 

在 .NET Framework 中， `DataSet` 并 `DataTable` 提供审核模式功能。 当启用审核模式时， `DataSet` 并 `DataTable` 根据允许的类型列表比较传入对象的类型。 但是，如果显示不允许其类型的对象，则 **不** 会引发异常。 相反， `DataSet` 并 `DataTable` 通知任何附加的 `TraceListener` 实例存在可疑类型，从而允许 `TraceListener` 记录此信息。 不会引发异常，并且反序列化操作将继续。

> [!WARNING]
> 在 "审核模式" 下运行应用程序时，只应使用临时度量值进行测试。 当启用审核模式时， `DataSet` 并且 `DataTable` 不强制实施类型限制，这可能会在应用内引入安全漏洞。 有关详细信息，请参阅标题为删除[不受信任输入](#swr)相关的[所有类型限制](#ratr)和安全性部分。

可以通过 _App.config_ 启用审核模式：

* 请参阅本文档中的 [扩展配置](#etc) 部分，了解要为元素放入的适当值 `<configSections>` 。
* 使用 `<allowedTypes auditOnly="true">` 启用审核模式，如以下标记所示。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <!-- See the section of this document titled "Extending through configuration" for the appropriate
         <sectionGroup> and <section> elements to put here, depending on whether you're running .NET
         Framework 2.0 - 3.5 or 4.0 - 4.8. -->
  </configSections>
  <system.data.dataset.serialization>
    <allowedTypes auditOnly="true"> <!-- setting auditOnly="true" enables audit mode -->
      <!-- Optional <add /> elements as needed. -->
    </allowedTypes>
  </system.data.dataset.serialization>
</configuration>
```

一旦启用了审核模式，就可以使用 _App.config_ 将您首选的内置 `TraceListener` `DataSet` `TraceSource.` 跟踪 _System.Data.DataSet_ 源名称连接到 "system.string"。 下面的示例演示如何将跟踪事件写入控制台 _和_ 磁盘上的日志文件。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.diagnostics>
    <sources>
      <source name="System.Data.DataSet"
              switchType="System.Diagnostics.SourceSwitch"
              switchValue="Warning">
        <listeners>
          <!-- write to the console -->
          <add name="console"
               type="System.Diagnostics.ConsoleTraceListener" />
          <!-- *and* write to a log file on disk -->
          <add name="filelog"
               type="System.Diagnostics.TextWriterTraceListener"
               initializeData="c:\logs\mylog.txt" />
        </listeners>
      </source>
    </sources>
  </system.diagnostics>
</configuration>
```

有关和的详细 `TraceSource` 信息 `TraceListener` ，请参阅文档 [如何：将 TraceSource 和筛选器与跟踪侦听器一起使用](../../../debug-trace-profile/how-to-use-tracesource-and-filters-with-trace-listeners.md)。

> [!NOTE]
> 在审核模式下运行应用在 .NET Core 或 .NET 5.0 及更高版本中不可用。

<a name="ratr"></a>

### <a name="remove-all-type-restrictions"></a>删除所有类型限制

如果应用必须从和中移除所有类型限制 `DataSet` 限制 `DataTable` ：

* 有几个选项可用于取消类型限制限制。
* 可用的选项取决于应用的目标框架。

> [!WARNING]
> 删除所有类型限制可能会在应用内引入安全漏洞。 使用此机制时，请确保应用 **不** 使用 `DataSet` 或 `DataTable` 读取不受信任的输入。 有关详细信息，请参阅 [CVE-2020-1147](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2020-1147) 和标题为 " [有关不受信任的输入的安全](#swr)" 的部分。

#### <a name="through-appcontext-configuration-net-framework-46---48-net-core-21-and-later-net-50-and-later"></a>通过 AppContext 配置 ( .NET Framework 4.6-4.8、.NET Core 2.1 及更高版本、.NET 5.0 和更高版本) 

`AppContext` `Switch.System.Data.AllowArbitraryDataSetTypeInstantiation` 当设置为时，开关会 `true` 从和中移除所有类型限制限制 `DataSet` `DataTable` 。

在 .NET Framework 中，可以通过 _App.config_ 启用此开关，如以下配置所示：

```xml
<configuration>
   <runtime>
      <!-- Warning: setting the following switch can introduce a security problem. -->
      <AppContextSwitchOverrides value="Switch.System.Data.AllowArbitraryDataSetTypeInstantiation=true" />
   </runtime>
</configuration>
```

在 ASP.NET 中， `<AppContextSwitchOverrides>` 元素不可用。 相反，可以通过 _Web.config_ 启用交换机，如以下配置所示：

```xml
<configuration>
    <appSettings>
        <!-- Warning: setting the following switch can introduce a security problem. -->
        <add key="AppContext.SetSwitch:Switch.System.Data.AllowArbitraryDataSetTypeInstantiation" value="true" />
    </appSettings>
</configuration>
```

有关详细信息，请参阅 [\<AppContextSwitchOverrides>](../../../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 元素。

在 .NET Core、.NET 5 和 ASP.NET Core 中，此设置由 _runtimeconfig.js_ 控制，如以下 JSON 中所示：

```json
{
  "runtimeOptions": {
    "configProperties": {
      "Switch.System.Data.AllowArbitraryDataSetTypeInstantiation": true
    }
  }
}
```

有关详细信息，请参阅 [".Net Core 运行时配置设置"](../../../../core/run-time-config/index.md)。

`AllowArbitraryDataSetTypeInstantiation` 还可以通过 [AppContext](/dotnet/api/system.appcontext.setswitch) （而不是配置文件）以编程方式进行设置，如以下代码所示：

```csharp
// Warning: setting the following switch can introduce a security problem.
AppContext.SetSwitch("Switch.System.Data.AllowArbitraryDataSetTypeInstantiation", true);
```

 如果选择前面的编程方法，则对的调用 `AppContext.SetSwitch` 应在应用程序启动初期进行。

#### <a name="through-the-machine-wide-registry-net-framework-20---48"></a>通过计算机范围内的注册表 ( .NET Framework 2.0-4.8) 

如果不可用 `AppContext` ，则可以使用 Windows 注册表禁用类型限制检查：

* 管理员必须配置注册表。
* 使用注册表是计算机范围的更改，将影响计算机上运行的 _所有_ 应用。

| 类型  |  Value |
|---|---|
| **注册表项** | `HKLM\SOFTWARE\Microsoft\.NETFramework\AppContext` |
| **值名称** | `Switch.System.Data.AllowArbitraryDataSetTypeInstantiation` |
| **值类型** | `REG_SZ` |
| **值数据** | `true` |

在64位操作系统中，需要为上面显示的64位密钥 (添加此值) 和32位密钥。 32位键位于 `HKLM\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\AppContext` 。

有关使用注册表进行配置的详细信息 `AppContext` ，请参阅 ["库使用者的 AppContext"](/dotnet/api/system.appcontext#appcontext-for-library-consumers)。

<a name="swr"></a>

## <a name="safety-with-regard-to-untrusted-input"></a>不受信任输入相关的安全

尽管 `DataSet` 和 `DataTable` 确实对允许在反序列化 XML 有效负载时存在的类型施加默认限制，但 __`DataSet` `DataTable` 在使用不受信任的输入进行填充时，和都是不安全的。__ 下面是 `DataSet` 或 `DataTable` 实例可能读取不受信任的输入的一系列非详尽列表。

* `DataAdapter`引用数据库， `DataAdapter.Fill` 方法用于使用 `DataSet` 数据库查询的内容来填充。
* `DataSet.ReadXml`或 `DataTable.ReadXml` 方法用于读取包含列和行信息的 XML 文件。
* `DataSet`或 `DataTable` 实例序列化为 ASP.NET (SOAP) web 服务或 WCF 终结点。
* 序列化程序（如） `XmlSerializer` 用于 `DataSet` 从 XML 流反序列化或 `DataTable` 实例。
* 序列化程序（如） `JsonConvert` 用于 `DataSet` 从 JSON 流反序列化或 `DataTable` 实例。 `JsonConvert` 是常用的第三方 [Newtonsoft.Js](https://www.newtonsoft.com/json) 库中的一种方法。
* 序列化程序（如） `BinaryFormatter` 用于 `DataSet` `DataTable` 从原始字节流中反序列化或实例。

本文档讨论上述方案的安全注意事项。

## <a name="use-dataadapterfill-to-populate-a-dataset-from-an-untrusted-data-source"></a>用于 `DataAdapter.Fill` `DataSet` 从不受信任的数据源填充

`DataSet` `DataAdapter` 通过使用[ `DataAdapter.Fill` 方法](/dotnet/api/system.data.common.dataadapter.fill)，可以从中填充实例，如下面的示例中所示。

```csharp
// Assumes that connection is a valid SqlConnection object.
string queryString =
  "SELECT CustomerID, CompanyName FROM dbo.Customers";
SqlDataAdapter adapter = new SqlDataAdapter(queryString, connection);

DataSet customers = new DataSet();
adapter.Fill(customers, "Customers");
```

 (上面的代码示例是 [从 DataAdapter 填充数据集中](../populating-a-dataset-from-a-dataadapter.md)找到的更大示例的一部分。 ) 

> 大多数应用程序都可以简化，并假定其数据库层受信任。 但是，如果您正在对应用程序进行 [威胁建模](https://www.microsoft.com/securityengineering/sdl/threatmodeling) ，则威胁模型可能会将应用程序 (客户端) 和数据库层之间的信任边界视为 (服务器) 。 在客户端和服务器之间使用 [相互身份验证](/sql/relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections) 或 [AAD 身份验证](/azure/azure-sql/database/authentication-aad-overview) 是一种帮助解决与此相关的风险的方法。 本部分的剩余部分讨论连接到不受信任的服务器的客户端可能的结果。

指向 `DataAdapter` 不受信任的数据源的后果取决于 `DataAdapter` 其自身的实现。

### <a name="sqldataadapter"></a>SqlDataAdapter

对于内置类型 [SqlDataAdapter](/dotnet/api/microsoft.data.sqlclient.sqldataadapter)，引用不受信任的数据源可能会导致拒绝服务 (DoS) 攻击。 DoS 攻击可能导致应用变得无响应或崩溃。 如果攻击者可以在应用程序中使用 DLL，则他们还可以实现本地代码执行。

### <a name="other-dataadapter-types"></a>其他 DataAdapter 类型

第三方 `DataAdapter` 实现必须对其在不受信任的输入中提供的安全保证进行自我评估。 .NET 无法确保与这些实现有关的任何安全保障。

<a name="dsrdtr"></a>

## <a name="datasetreadxml-and-datatablereadxml"></a>ReadXml 和 ReadXml

`DataSet.ReadXml` `DataTable.ReadXml` 与不受信任的输入一起使用时，和方法是安全的。 强烈建议使用者改为考虑使用本文档后面所述的备选方案之一。

和的实现 `DataSet.ReadXml` `DataTable.ReadXml` 最初是在序列化漏洞成为一种很好理解的威胁类别之前创建的。 因此，代码不遵循当前安全最佳做法。 这些 Api 可用作攻击者对 web 应用执行 DoS 攻击的媒介。 这些攻击可能会导致 web 服务无响应或导致意外的进程终止。 此框架不提供对这些攻击类别的缓解措施，.NET 认为 "按设计" 了此行为。

.NET 已发布安全更新，可在和中缓解某些问题，例如信息泄漏或远程代码执行 `DataSet.ReadXml` `DataTable.ReadXml` 。 .NET 安全更新可能无法提供对这些威胁类别的完整保护。 使用者应该评估其各个应用场景，并考虑他们遇到这些风险的可能性。

使用者应该知道，在某些情况下，这些 Api 的安全更新可能影响应用程序的兼容性。 此外，还可能会发现，.NET 不能几乎不能发布安全更新。

建议使用以下 Api 的使用者：

* 请考虑使用本文档后面所述的一种替代方法。
* 对其应用执行单独的风险评估。

使用者唯一负责确定是否要使用这些 Api。 消费者应评估任何安全、技术和法律风险，包括使用这些 Api 时可能附带的法规要求。

## <a name="dataset-and-datatable-via-aspnet-web-services-or-wcf"></a>通过 ASP.NET web 服务或 WCF 进行数据集和 DataTable

可以接受 `DataSet` `DataTable` ASP.NET (SOAP) web 服务中的或实例，如以下代码所示：

```csharp
using System.Data;
using System.Web.Services;

[WebService(Namespace = "http://contoso.com/")]
public class MyService : WebService
{
    [WebMethod]
    public string MyWebMethod(DataTable dataTable)
    {
        /* Web method implementation. */
    }
}
```

这种情况下的变体不接受 `DataSet` 或 `DataTable` 直接作为参数，而是将其作为整体 SOAP 序列化对象图的一部分接受，如以下代码所示：

```csharp
using System.Data;
using System.Web.Services;

[WebService(Namespace = "http://contoso.com/")]
public class MyService : WebService
{
    [WebMethod]
    public string MyWebMethod(MyClass data)
    {
        /* Web method implementation. */
    }
}

public class MyClass
{
    // Property of type DataTable, automatically serialized and
    // deserialized as part of the overall MyClass payload.
    public DataTable MyDataTable { get; set; }
}
```

或者，使用 WCF 而不是 ASP.NET web 服务：

```csharp
using System.Data;
using System.ServiceModel;

[ServiceContract(Namespace = "http://contoso.com/")]
public interface IMyContract
{
    [OperationContract]
    string MyMethod(DataTable dataTable);
    [OperationContract]
    string MyOtherMethod(MyClass data);
}

public class MyClass
{
    // Property of type DataTable, automatically serialized and
    // deserialized as part of the overall MyClass payload.
    public DataTable MyDataTable { get; set; }
}
```

在所有这些情况下，威胁模型和安全保证都与 [ReadXml 和 ReadXml 节](#dsrdtr)相同。

## <a name="deserialize-a-dataset-or-datatable-via-xmlserializer"></a>通过 XmlSerializer 反序列化数据集或 DataTable

开发人员可以使用 `XmlSerializer` 反序列化 `DataSet` 和 `DataTable` 实例，如以下代码所示：

```csharp
using System.Data;
using System.IO;
using System.Xml.Serialization;

public DataSet PerformDeserialization1(Stream stream) {
    XmlSerializer serializer = new XmlSerializer(typeof(DataSet));
    return (DataSet)serializer.Deserialize(stream);
}

public MyClass PerformDeserialization2(Stream stream) {
    XmlSerializer serializer = new XmlSerializer(typeof(MyClass));
    return (MyClass)serializer.Deserialize(stream);
}

public class MyClass
{
    // Property of type DataTable, automatically serialized and
    // deserialized as part of the overall MyClass payload.
    public DataTable MyDataTable { get; set; }
}
```

在这些情况下，威胁模型和安全保证与[ReadXml 和 ReadXml 节](#dsrdtr)相同

## <a name="deserialize-a-dataset-or-datatable-via-jsonconvert"></a>通过 JsonConvert 反序列化 DataSet 或 DataTable

常见的第三方 Newtonsoft.json 库 [JSON.NET](https://www.newtonsoft.com/json) 可用于反序列化 `DataSet` 和 `DataTable` 实例，如以下代码所示：

```csharp
using System.Data;
using Newtonsoft.Json;

public DataSet PerformDeserialization1(string json) {
    return JsonConvert.DeserializeObject<DataSet>(data);
}

public MyClass PerformDeserialization2(string json) {
    return JsonConvert.DeserializeObject<MyClass>(data);
}

public class MyClass
{
    // Property of type DataTable, automatically serialized and
    // deserialized as part of the overall MyClass payload.
    public DataTable MyDataTable { get; set; }
}
```

`DataSet` `DataTable` 以这种方式从不受信任的 JSON blob 反序列化或反序列化是不安全的。 此模式容易遭受拒绝服务攻击。 此类攻击可能导致应用崩溃或使其无响应。

> [!NOTE]
> Microsoft 不保证或支持实现第三方库，如 _Newtonsoft.Js_。 此信息是为完整性提供的，并且在撰写本文的时间准确无误。

## <a name="deserialize-a-dataset-or-datatable-via-binaryformatter"></a>通过 BinaryFormatter 反序列化 DataSet 或 DataTable

开发人员决不能使用 `BinaryFormatter` 、 `NetDataContractSerializer` 、 `SoapFormatter` 或相关的 ***unsafe** _ 格式化 `DataSet` `DataTable` 程序从不受信任的负载反序列化或实例：

_ 这容易遭受完全远程代码执行攻击。
* 使用自定义 `SerializationBinder` 并不足以防止这种攻击。

## <a name="safe-replacements"></a>安全替换

对于以下两种应用：

* 接受 `DataSet` 或 `DataTable` 通过 .asmx SOAP 终结点或 WCF 终结点。
* 将不受信任的数据反序列化为或的实例 `DataSet` `DataTable` 。

请考虑将对象模型替换为使用 [实体框架](/ef)。 实体框架：

* 是一个丰富的、面向对象的新型框架，可表示关系数据。
* 引入 [了一种不同](/ef/core/providers/) 的数据库提供程序生态系统，使您可以轻松地通过实体框架对象模型来投影数据库查询。
* 在反序列化来自不受信任源的数据时提供内置保护。

对于使用 `.aspx` SOAP 终结点的应用，请考虑将这些终结点更改为使用 [WCF](../../../wcf/index.md)。 WCF 是为 web 服务提供的更好的替代功能 `.asmx` 。 [可以通过 SOAP 公开](../../../wcf/feature-details/how-to-expose-a-contract-to-soap-and-web-clients.md)WCF 终结点，以便与现有调用方兼容。

## <a name="code-analyzers"></a>代码分析器

代码分析器安全规则在您的源代码编译后运行，可帮助找到与 c # 中的此安全问题和 Visual Basic 代码相关的漏洞。 CodeAnalysis. FxCopAnalyzers 是在 [nuget.org](https://www.nuget.org/)上分发的代码分析器 NuGet 包。

有关代码分析器的概述，请参阅 [源代码分析器概述](/visualstudio/code-quality/roslyn-analyzers-overview)。

启用以下 CodeAnalysis FxCopAnalyzers 规则：

- [CA2350](/visualstudio/code-quality/ca2350)：不要使用 DataTable. ReadXml ( 包含不受信任数据的 # A1
- [CA2351](/visualstudio/code-quality/ca2351)：不要使用 ReadXml ( 包含不受信任数据的 # A1
- [CA2352](/visualstudio/code-quality/ca2352)：可序列化的类型中的不安全数据集或 DataTable 可能易受到远程代码执行攻击
- [CA2353](/visualstudio/code-quality/ca2353)：可序列化类型中的不安全数据集或 DataTable
- [CA2354](/visualstudio/code-quality/ca2354)：反序列化对象图中不安全的数据集或 DataTable 可能易受到远程代码执行攻击
- [CA2355](/visualstudio/code-quality/ca2355)：反序列化对象图中发现不安全的数据集或 DataTable 类型
- [CA2356](/visualstudio/code-quality/ca2356)： web 反序列化对象图中的不安全数据集或 DataTable 类型
- [CA2361](/visualstudio/code-quality/ca2361)：确保包含 DataSet 的自动生成的类。 ReadXml ( # A1 不与不受信任的数据一起使用
- [CA2362](/visualstudio/code-quality/ca2362)：自动生成序列化类型中的不安全数据集或 DataTable 可能易受到远程代码执行攻击

有关配置规则的详细信息，请参阅 [使用代码分析器](/visualstudio/code-quality/use-roslyn-analyzers)。

以下 NuGet 包中提供了新的安全规则：

- CodeAnalysis. FxCopAnalyzers 3.3.0： for Visual Studio 2019 版本16.3 或更高版本
- CodeAnalysis. FxCopAnalyzers 2.9.11： for Visual Studio 2017 版本15.9 或更高版本
