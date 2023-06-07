---
title: 代码访问安全性
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 93e099eb-daa1-4f1e-b031-c1e10a996f88
ms.openlocfilehash: c4c18e8026dc230db896103d29d40426dbd11f16
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203834"
---
# <a name="code-access-security-and-adonet"></a>代码访问安全性和 ADO.NET

.NET Framework 提供基于角色的安全性和代码访问安全性 (CAS)，这两种安全性都可以通过公共语言运行库 (CLR) 提供的公共基础结构实现。 对于非托管代码，大多数应用程序都可以使用用户或主体权限执行。 因此，当拥有提升权限的用户运行恶意软件或包含错误的软件时，计算机系统可能会受到损坏并危及私有数据。  
  
 相对而言，.NET Framework 中执行的托管代码包括单独应用于代码的代码访问安全性。 是否允许运行代码取决于代码的来源或代码标识的其他方面，而不仅仅是主体标识。 这样可以减小滥用托管代码的可能性。  
  
## <a name="code-access-permissions"></a>代码访问权限  

 在执行代码时，代码会提供通过 CLR 安全系统计算的证据。 通常，此证据由代码的来源（包括 URL、站点和区域）以及确保程序集标识的数字签名组成。  
  
 CLR 仅允许代码执行代码具有执行权限的那些操作。 代码可以请求权限，而这些请求需要基于管理员设置的安全策略。  
  
> [!NOTE]
> 在 CLR 中指定的代码不能为自身授予权限。 例如，代码可以请求并获得比安全策略允许的权限少的权限，但决不会获得比安全策略允许的权限多的权限。 在授予权限时，应该从无权限开始，然后为要执行的特定任务添加最少的权限。 一开始就使用所有权限，然后拒绝各个权限会导致应用程序不安全，应用程序可能会授予不必要的权限，从而使应用程序无意中包含安全漏洞。 有关详细信息，请参阅 [配置安全策略](/previous-versions/dotnet/netframework-4.0/7c9c2y1w(v=vs.100)) 和 [安全策略管理](/previous-versions/dotnet/netframework-4.0/c1k0eed6(v=vs.100))。  
  
 代码访问权限有三种类型：  
  
- `Code access permissions`从 <xref:System.Security.CodeAccessPermission> 类派生。 需要具有权限才能访问受保护的资源（如文件和环境变量）和执行受保护的操作（如访问托管代码）。  
  
- `Identity permissions`表示标识程序集的特征。 对程序集授予权限需要基于证据，而证据可以包括如数字签名或代码来源等项。 标识权限也从 <xref:System.Security.CodeAccessPermission> 基类派生。  
  
- `Role-based security permissions`基于主体是否具有指定标识或是否是指定角色的成员。 <xref:System.Security.Permissions.PrincipalPermission> 类允许对活动主体进行声明性和强制性权限检查。  
  
 为了确定代码是否获得了访问某一资源或执行某一操作的授权，运行库的安全系统将遍历调用堆栈，将每个调用方已获得的权限与要求的权限进行比较。 如果调用堆栈中的任何调用方没有要求的权限，则会引发 <xref:System.Security.SecurityException> 并拒绝访问。  
  
### <a name="requesting-permissions"></a>请求权限  

 请求权限的目的是通知运行库您的应用程序要求哪些权限才能运行，并确保应用程序只接收到实际需要的权限。 例如，如果您的应用程序需要将数据写入本地磁盘，则需要 <xref:System.Security.Permissions.FileIOPermission>。 如果尚未授予该权限，则在应用程序尝试写入磁盘时将失败。 不过，如果应用程序请求 `FileIOPermission` 并且尚未授予该权限，则应用程序一开始即会生成异常，因此将不会加载。  
  
 在应用程序只需从磁盘读取数据的情况下，您可以请求永远不为应用程序授予任何写入权限。 在出现 Bug 或受到恶意攻击时，你的代码将不会损坏它所操作的数据。 有关详细信息，请参阅 [请求权限](/previous-versions/dotnet/netframework-4.0/yd267cce(v=vs.100))。  
  
## <a name="role-based-security-and-cas"></a>基于角色的安全性和 CAS  

 同时实现基于角色的安全性和代码访问安全性 (CAS) 可增强应用程序的整体安全性。 基于角色的安全性可以基于 Windows 帐户或自定义标识，使有关安全主体的信息可用于当前线程。 此外，通常还要求应用程序基于用户提供的凭据提供对数据或资源的访问。 通常情况下，这种应用程序会检查用户的角色，并根据这些角色提供对资源的访问。  
  
 基于角色的安全性使组件能够在运行时标识当前用户及其关联的角色。 然后使用 CAS 策略映射此信息以确定运行时授予的一组权限。 对于指定的应用程序域，主机可以更改基于角色的默认安全策略并设置表示用户的默认安全主体和与该用户关联的角色。  
  
 CLR 使用权限来实现其用于对托管代码实施限制的机制。 基于角色的安全权限提供一种机制，用于发现用户（或代表该用户的代理）是否具有特定标识或者是否是指定角色的成员。 有关详细信息，请参阅 [安全权限](/previous-versions/dotnet/netframework-4.0/5ba4k1c5(v=vs.100))。  
  
 根据要生成的应用程序类型，还应考虑在数据库中实现基于角色的权限。 有关 SQL Server 中基于角色的安全性的详细信息，请参阅 [SQL Server 安全性](./sql/sql-server-security.md)。  
  
## <a name="assemblies"></a>程序集  

 程序集构成 .NET Framework 应用程序部署、版本控制、重复使用、激活范围和安全权限的基本单元。 程序集提供类型和资源的集合，二者结合在一起构成功能的逻辑单元。 对于 CLR，类型不存在于程序集的上下文之外。 有关创建和部署程序集的详细信息，请参阅 [用程序集编程](../../../standard/assembly/index.md)。  
  
### <a name="strong-naming-assemblies"></a>强命名程序集  

 强名称（或数字签名）由程序集的标识组成，该标识包括程序集的简单文本名称、版本号和区域性信息（如果提供）、公钥和数字签名。 数字签名使用相应私钥从程序集文件生成。 程序集文件包含程序集清单，该清单包含组成程序集的所有文件的名称和哈希。  
  
 强命名程序集可为应用程序或组件提供唯一的标识，其他软件可以使用该标识显式引用应用程序或组件。 强命名可以保护程序集，防止包含恶意代码的程序集冒充。 强命名还可以保证组件的不同版本之间的版本一致性。 对于将要部署到全局程序集缓存 (GAC) 的程序集，必须进行强命名。 有关详细信息，请参阅[创建和使用具有强名称的程序集](../../../standard/assembly/create-use-strong-named.md)。  
  
## <a name="partial-trust-in-adonet-20"></a>ADO.NET 2.0 中的部分信任  

 在 ADO.NET 2.0 中，适用于 SQL Server 的 .NET Framework 数据提供程序、适用于 OLE DB 的 .NET Framework 数据提供程序、适用于 ODBC 的 .NET Framework 数据提供程序和适用于 Oracle 的 .NET Framework 数据提供程序现在全部可以在部分信任的环境中运行。 在以前版本的 .NET Framework 中，非完全信任的应用程序仅支持 <xref:System.Data.SqlClient>。  
  
 使用 SQL Server 提供程序的部分信任应用程序必须至少具有执行和 <xref:System.Data.SqlClient.SqlClientPermission> 权限。  
  
### <a name="permission-attribute-properties-for-partial-trust"></a>部分信任的权限属性  

 对于部分信任的方案，可以使用 <xref:System.Data.SqlClient.SqlClientPermissionAttribute> 成员进一步限制 SQL Server .NET Framework 数据提供程序可以使用的功能。  
  
 下表列出了可用的 <xref:System.Data.SqlClient.SqlClientPermissionAttribute> 属性及其说明：  
  
|权限属性|描述|  
|-----------------------------------|-----------------|  
|`Action`|获取或设置安全性操作。 从 <xref:System.Security.Permissions.SecurityAttribute> 继承。|  
|`AllowBlankPassword`|启用或禁用连接字符串中空白密码。 有效值为 `true`（启用空白密码的使用）和 `false`（禁用空白密码的使用）。 从 <xref:System.Data.Common.DBDataPermissionAttribute> 继承。|  
|`ConnectionString`|标识允许的连接字符串。 可标识多个连接字符串。 **注意：**  不要在连接字符串中包括用户 ID 或密码。 此版本中，不能使用 .NET Framework 配置工具更改连接字符串限制。 <br /><br /> 从 <xref:System.Data.Common.DBDataPermissionAttribute> 继承。|  
|`KeyRestrictions`|标识允许或不允许的连接字符串参数。 连接字符串参数以形式标识 *\<parameter name>=* 。 可指定多个参数，并用分号 (;) 进行分隔。 **注意：**  如果未指定 `KeyRestrictions` ，但将 `KeyRestrictionBehavior` 属性设置为 `AllowOnly` 或，则 `PreventUsage` 不允许使用其他连接字符串参数。 从 <xref:System.Data.Common.DBDataPermissionAttribute> 继承。|  
|`KeyRestrictionBehavior`|将连接字符串参数标识为唯一允许的附加参数 (`AllowOnly`)，或标识不允许的附加参数 (`PreventUsage`)。 `AllowOnly` 为默认值。 从 <xref:System.Data.Common.DBDataPermissionAttribute> 继承。|  
|`TypeID`|在派生类中实现此属性时获取唯一标识符。 从 <xref:System.Attribute> 继承。|  
|`Unrestricted`|表明是否声明对该资源的无限制权限。 从 <xref:System.Security.Permissions.SecurityAttribute> 继承。|  
  
#### <a name="connectionstring-syntax"></a>ConnectionString 语法  

 下面的示例演示如何使用配置文件的 `connectionStrings` 元素仅允许使用特定的连接字符串。 有关从配置文件中存储和检索连接字符串的详细信息，请参阅 [连接字符串](connection-strings.md) 。  
  
```xml  
<connectionStrings>  
  <add name="DatabaseConnection"
    connectionString="Data Source=(local);Initial
    Catalog=Northwind;Integrated Security=true;" />  
</connectionStrings>  
```  
  
#### <a name="keyrestrictions-syntax"></a>KeyRestrictions 语法  

 下面的示例启用相同的连接字符串，启用 `Encrypt` 和 `Packet Size` 连接字符串选项，但限制使用任何其他连接字符串选项。  
  
```xml  
<connectionStrings>  
  <add name="DatabaseConnection"
    connectionString="Data Source=(local);Initial
    Catalog=Northwind;Integrated Security=true;"  
    KeyRestrictions="Encrypt=;Packet Size=;"  
    KeyRestrictionBehavior="AllowOnly" />  
</connectionStrings>  
```  
  
#### <a name="keyrestrictionbehavior-with-preventusage-syntax"></a>包含 PreventUsage 的 KeyRestrictionBehavior 的语法  

 下面的示例启用相同的连接字符串，并允许使用 `User Id`、`Password` 和 `Persist Security Info` 以外的所有其他连接参数。  
  
```xml  
<connectionStrings>  
  <add name="DatabaseConnection"
    connectionString="Data Source=(local);Initial
    Catalog=Northwind;Integrated Security=true;"  
    KeyRestrictions="User Id=;Password=;Persist Security Info=;"  
    KeyRestrictionBehavior="PreventUsage" />  
</connectionStrings>  
```  
  
#### <a name="keyrestrictionbehavior-with-allowonly-syntax"></a>包含 AllowOnly 的 KeyRestrictionBehavior 的语法  

 以下示例启用两个连接字符串，其中还包含 `Initial Catalog`、`Connection Timeout`、`Encrypt` 和 `Packet Size` 参数。 而所有其他的连接字符串参数都被限制。  
  
```xml  
<connectionStrings>  
  <add name="DatabaseConnection"
    connectionString="Data Source=(local);Initial
    Catalog=Northwind;Integrated Security=true;"  
    KeyRestrictions="Initial Catalog;Connection Timeout=;  
       Encrypt=;Packet Size=;"
    KeyRestrictionBehavior="AllowOnly" />  
  
  <add name="DatabaseConnection2"
    connectionString="Data Source=SqlServer2;Initial
    Catalog=Northwind2;Integrated Security=true;"  
    KeyRestrictions="Initial Catalog;Connection Timeout=;  
       Encrypt=;Packet Size=;"
    KeyRestrictionBehavior="AllowOnly" />  
</connectionStrings>  
```  
  
### <a name="enabling-partial-trust-with-a-custom-permission-set"></a>启用具有自定义权限集的部分信任  

 要对特定区域启用 <xref:System.Data.SqlClient> 权限，系统管理员必须创建自定义的权限集，并将其设置为特定区域的权限集。 不能修改默认权限集（如 `LocalIntranet`）。 例如，若要包含 <xref:System.Data.SqlClient> 具有的代码的权限， <xref:System.Security.Policy.Zone> `LocalIntranet` 系统管理员可以复制的权限集 `LocalIntranet` ，将其重命名为 "CustomLocalIntranet"，添加 <xref:System.Data.SqlClient> 权限，使用 [Caspol.exe (代码访问安全策略工具) ](../../tools/caspol-exe-code-access-security-policy-tool.md)导入 CustomLocalIntranet 权限集，并将权限集设置 `LocalIntranet_Zone` 为 CustomLocalIntranet。  
  
### <a name="sample-permission-set"></a>示例权限集  

 下面是在部分受信任方案中，SQL Server .NET Framework 数据提供程序的示例权限集。 有关创建自定义权限集的信息，请参阅 [使用 Caspol.exe配置权限集 ](/previous-versions/dotnet/netframework-4.0/4ybs46y6(v=vs.100))。  
  
```xml  
<PermissionSet class="System.Security.NamedPermissionSet"  
  version="1"  
  Name="CustomLocalIntranet"  
  Description="Custom permission set given to applications on  
    the local intranet">  
  
<IPermission class="System.Data.SqlClient.SqlClientPermission, System.Data, Version=2.0.0000.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"  
version="1"  
AllowBlankPassword="False">  
<add ConnectionString="Data Source=(local);Integrated Security=true;"  
 KeyRestrictions="Initial Catalog=;Connection Timeout=;  
   Encrypt=;Packet Size=;"
 KeyRestrictionBehavior="AllowOnly" />  
 </IPermission>  
</PermissionSet>  
```  
  
## <a name="verifying-adonet-code-access-using-security-permissions"></a>使用安全权限验证 ADO.NET 代码访问  

 对于部分信任方案，可以通过指定 <xref:System.Data.SqlClient.SqlClientPermissionAttribute> 来要求代码中的特定方法具有 CAS 特权。 如果当前受限制的安全策略不允许该权限，在运行代码之前将引发异常。 有关安全策略的详细信息，请参阅 [安全策略管理](/previous-versions/dotnet/netframework-4.0/c1k0eed6(v=vs.100)) 和 [安全策略最佳实践](/previous-versions/dotnet/netframework-4.0/sa4se9bc(v=vs.100))。  
  
### <a name="example"></a>示例  

 以下示例演示如何编写要求特定连接字符串的代码。 该示例模拟拒绝为 <xref:System.Data.SqlClient> 授予无限制权限的过程，系统管理员在实际工作中将会使用 CAS 策略实现该过程。  
  
> [!IMPORTANT]
> 在设计 ADO.NET 的 CAS 权限时，正确的模式是以限制性最强的情况开始（无任何权限），然后添加代码执行特定任务所需的特定权限。 相反的模式是一开始就授予所有权限，然后拒绝特定权限，这样做是不安全的，因为表达同一连接字符串可以有许多方式。 例如，如果一开始就授予所有权限，然后尝试拒绝使用连接字符串“server=someserver”，则仍将允许使用“server=someserver.mycompany.com”。 通过在开始时始终不授予任何权限，可以降低权限集中存在漏洞的几率。  
  
 下面的代码演示 `SqlClient` 如何执行安全请求，如果没有相应的 CAS 权限，将引发 <xref:System.Security.SecurityException>。 控制台窗口中显示 <xref:System.Security.SecurityException> 输出。  
  
 [!code-csharp[DataWorks SqlClient.CAS#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.CAS/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.CAS#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.CAS/VB/source.vb#1)]  
  
 在控制台窗口中应看到以下输出：  
  
```output  
Failed, as expected: <IPermission class="System.Data.SqlClient.  
SqlClientPermission, System.Data, Version=2.0.0.0,
  Culture=neutral, PublicKeyToken=b77a5c561934e089" version="1"  
  AllowBlankPassword="False">  
<add ConnectionString="Data Source=(local);Initial Catalog=  
  Northwind;Integrated Security=SSPI" KeyRestrictions=""  
KeyRestrictionBehavior="AllowOnly"/>  
</IPermission>  
  
Connection opened, as expected.  
Failed, as expected: Request failed.  
```  
  
## <a name="interoperability-with-unmanaged-code"></a>与非托管代码的互操作性  

 在 CLR 外部运行的代码称为非托管代码。 因此，安全机制（如 CAS）不能应用于非托管代码。 COM 组件、ActiveX 接口和 Windows API 函数都是非托管代码的示例。 在执行非托管代码时应考虑特殊安全注意事项，以便不会危害应用程序的整体安全性。 有关详细信息，请参阅[与非托管代码交互操作](../../interop/index.md)。  
  
 .NET Framework 可以通过 COM 互操作提供访问，因此还支持与现有 COM 组件的向后兼容。 通过使用 COM 互操作工具导入相关的 COM 类型，可以将 COM 组件合并到 .NET Framework 应用程序中。 一旦导入后，就可以使用 COM 类型了。 通过将程序集元数据导出到类型库并将托管组件注册为 COM 组件，COM 互操作还可以使 COM 客户端访问托管代码。 有关详细信息，请参阅 [高级 COM 互操作性](/previous-versions/dotnet/netframework-4.0/bd9cdfyx(v=vs.100))。  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](securing-ado-net-applications.md)
- [本机代码和 .NET Framework 代码的安全性](/previous-versions/visualstudio/visual-studio-2010/1787tk12(v=vs.100))
- [基于角色的安全性](../../../standard/security/role-based-security.md)
- [ADO.NET 概述](ado-net-overview.md)
