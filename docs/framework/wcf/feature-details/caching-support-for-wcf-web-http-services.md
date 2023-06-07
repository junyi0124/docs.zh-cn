---
title: 对 WCF Web HTTP 服务的缓存支持
ms.date: 03/30/2017
ms.assetid: 7f8078e0-00d9-415c-b8ba-c1b6d5c31799
ms.openlocfilehash: 6ce3ceccde01879876960e0288cb600a3a20c204
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279332"
---
# <a name="caching-support-for-wcf-web-http-services"></a>对 WCF Web HTTP 服务的缓存支持

[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 使你能够使用 WCF Web HTTP 服务的 ASP.NET 中已有的声明性缓存机制。 这样，你可以缓存来自 WCF Web HTTP 服务操作的响应。 如果用户向配置为进行缓存的服务发送了 HTTP GET，ASP.NET 将发送回已缓存的响应且不会调用服务方法。 在缓存过期后，下次用户发送 HTTP GET 时，将会调用服务方法且再次缓存响应。 有关 ASP.NET 缓存的详细信息，请参阅 [ASP.NET 缓存概述](/previous-versions/aspnet/ms178597(v=vs.100))。  
  
## <a name="basic-web-http-service-caching"></a>基本 Web HTTP 服务缓存  

  若要启用 WEB HTTP 服务缓存，必须首先通过将应用 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> 于服务设置为或，启用 ASP.NET 兼容性 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute.RequirementsMode%2A> <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Required> 。  
  
 .NET Framework 4 引入了一个名为 <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> 的新属性，该属性允许你指定缓存配置文件名称。 此特性适用于服务操作。 下面的示例将 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> 应用到服务以启用 ASP.NET 兼容性，并配置 `GetCustomer` 操作以进行缓存。 <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> 特性指定包含要使用的缓存设置的缓存配置文件。  
  
```csharp
[ServiceContract]
[AspNetCompatibilityRequirements(RequirementsMode=AspNetCompatibilityRequirementsMode.Allowed)]
public class Service
{
    [WebGet(UriTemplate = "{id}")]
    [AspNetCacheProfile("CacheFor60Seconds")]
    public Customer GetCustomer(string id)
    {
        // ...
    }
}
```  
  
同时在 Web.config 文件中打开 ASP.NET 兼容模式，如下面的示例中所示。  
  
```xml
<system.serviceModel>
  <serviceHostingEnvironment aspNetCompatibilityEnabled="true" />
</system.serviceModel>
```
  
> [!WARNING]
> 如果未启用 ASP.NET 兼容模式且使用了 <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute>，则会引发异常。  
  
 <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> 所指定的缓存配置文件名称标识已添加到 Web.config 配置文件的缓存配置文件。 缓存配置文件在 <> 元素中定义， `outputCacheSetting` 如下面的配置示例所示。  
  
```xml
<!-- ...  -->
<system.web>  
   <caching>  
      <outputCacheSettings>  
         <outputCacheProfiles>  
            <add name="CacheFor60Seconds" duration="60" varyByParam="none" sqlDependency="MyTestDatabase:MyTable"/>  
         </outputCacheProfiles>  
      </outputCacheSettings>  
   </caching>  
   <!-- ... -->  
</system.web>  
```  
  
 这是可供 ASP.NET 应用程序使用的同一配置元素。 有关 ASP.NET 缓存配置文件的详细信息，请参阅 <xref:System.Web.Configuration.OutputCacheProfile> 。 对于 Web HTTP 服务，缓存配置文件中最重要的特性是 `cacheDuration` 和 `varyByParam`。 这两个特性都是必需的。 `cacheDuration` 以秒为单位设置应缓存响应的时间。 `varyByParam` 允许您指定用于缓存响应的查询字符串参数。 对于使用不同查询字符串参数值发出的所有请求，将单独进行缓存。 例如，在对发出初始请求后 `http://MyServer/MyHttpService/MyOperation?param=10` ，只要缓存持续时间) ，就会将缓存的响应 (返回缓存的响应。 对于形式相同但具有不同参数查询字符串参数值的类似请求的响应，将单独进行缓存。 如果不需要此单独缓存行为，请将 `varyByParam` 设置为“无”。  
  
## <a name="sql-cache-dependency"></a>SQL 缓存依赖项  

  还可以跟随 SQL 缓存依赖项来缓存 Web HTTP 服务响应。 如果 WCF Web HTTP 服务依赖于 SQL 数据库中存储的数据，则当 SQL 数据库表中的数据更改时，您可能希望缓存服务的响应并使已缓存的响应无效。 此行为完全在 Web.config 文件中配置。 首先，在 <> 元素中定义一个连接字符串 `connectionStrings` 。  
  
```xml
<connectionStrings>
  <add name="connectString"
       connectionString="Data Source=MyService;Initial Catalog=MyTestDatabase;Integrated Security=True"
       providerName="System.Data.SqlClient" />
</connectionStrings>
```  
  
 然后，必须在 <> 元素内的 <> 元素内启用 SQL 缓存依赖关系 `caching` `system.web` ，如以下配置示例所示。  
  
```xml  
<system.web>
  <caching>
    <sqlCacheDependency enabled="true" pollTime="1000">
      <databases>
        <add name="MyTestDatabase" connectionStringName="connectString" />
      </databases>
    </sqlCacheDependency>
    <!-- ... -->
  </caching>
  <!-- ... -->
</system.web>
```  
  
 此处启用了 SQL 缓存依赖项，并设置了 1000 毫秒的轮询时间。 每当轮询时间结束时，都会检查数据库表是否有更新。 如果检测到更改，则会删除缓存的内容，并且下次调用服务操作时将缓存新响应。 在 <`sqlCacheDependency`> 元素中添加数据库，并在 <> 元素中引用连接字符串， `databases` 如下面的示例中所示。  
  
```xml  
<system.web>
  <caching>
    <sqlCacheDependency enabled="true" pollTime="1000">
      <databases>
        <add name="MyTestDatabase" connectionStringName="connectString" />
      </databases>  
    </sqlCacheDependency>  
    <!-- ... -->  
  </caching>  
  <!-- ... -->  
</system.web>  
```  
  
 接下来，必须在 <> 元素中配置输出缓存设置 `caching` ，如下面的示例中所示。  
  
```xml
<system.web>
  <caching>  
    <!-- ...  -->
    <outputCacheSettings>
      <outputCacheProfiles>
        <add name="CacheFor60Seconds" duration="60" varyByParam="none" sqlDependency="MyTestDatabase:MyTable" />
      </outputCacheProfiles>
    </outputCacheSettings>
  </caching>
  <!-- ... -->
</system.web>
```  
  
 此处，"缓存持续时间" 设置为 "60 `varyByParam` "，设置为 "无"，并 `sqlDependency` 设置为以分号分隔的数据库名称/表对列表，以冒号分隔。 当 `MyTable` 中的数据发生更改时，将删除服务操作的已缓存响应；当调用操作时，将生成新的响应（通过调用服务操作），缓存该响应并将其返回到客户端。  
  
> [!IMPORTANT]
> 要使 ASP.NET 能够访问 SQL 数据库，必须使用 [ASP.NET SQL Server 注册工具](/previous-versions/dotnet/netframework-3.5/ms229862(v=vs.90))。 此外，还必须允许适当的用户帐户对数据库和表具有访问权限。 有关详细信息，请参阅 [从 Web 应用程序访问 SQL Server](/previous-versions/aspnet/ht43wsex(v=vs.100))。  
  
## <a name="conditional-http-get-based-caching"></a>基于条件 HTTP GET 的缓存  

  在 Web HTTP 方案中，服务通常使用条件 HTTP GET 来实现智能 HTTP 缓存，如 [HTTP 规范](https://www.w3.org/Protocols/rfc2616/rfc2616.html)中所述。 为此，服务必须在 HTTP 响应中设置 ETag 标头的值。 服务还必须检查 HTTP 请求中的 If-None-Match 标头，查看是否有任何指定的 ETag 与当前 ETag 相匹配。  
  
 对于 GET 和 HEAD 请求，<xref:System.ServiceModel.Web.IncomingWebRequestContext.CheckConditionalRetrieve%2A> 接受 ETag 值并根据请求的 If-None-Match 标头检查该值。 如果该标头存在并且存在匹配项，则会 <xref:System.ServiceModel.Web.WebFaultException> 引发一个具有 HTTP 状态代码 304 (未) 修改的，并使用匹配的 etag 将 ETag 标头添加到响应中。  
  
 <xref:System.ServiceModel.Web.IncomingWebRequestContext.CheckConditionalRetrieve%2A> 方法的一个重载接受上次修改的日期并根据请求的 If-Modified-Since 标头检查该日期。 如果该标头存在并且自该日期以来尚未修改资源，将引发 <xref:System.ServiceModel.Web.WebFaultException> 并返回 HTTP 状态代码 304（未修改）。  
  
 对于 PUT、POST 和 DELETE 请求，<xref:System.ServiceModel.Web.IncomingWebRequestContext.CheckConditionalUpdate%2A> 采用资源的当前 ETag 值。 如果当前 ETag 值为 null，则该方法将检查 If-None-Match 标头的值是否为 "*"。  如果当前 ETag 值不是默认值，该方法将根据请求的 If-Match 标头检查当前 ETag 值。 在任一情况下，如果请求中不存在预期的标头或该标头的值不满足条件检查，方法将引发 <xref:System.ServiceModel.Web.WebFaultException>，返回 HTTP 状态代码 412（前置条件失败），并将响应的 ETag 标头设置为当前 ETag 值。  
  
 `CheckConditional` 方法和 <xref:System.ServiceModel.Web.OutgoingWebResponseContext.SetETag%2A> 方法都可确保对响应标头设置的 ETag 值是符合 HTTP 规范的有效 ETag。 这包括将 ETag 值用双引号引起来（如果这些值尚不存在）和正确转义任何内部双引号字符。 不支持弱 ETag 比较。  
  
 下面的示例显示如何使用这些方法。  
  
```csharp
[WebGet(UriTemplate = "{id}"), Description("Returns the specified customer from customers collection. Returns NotFound if there is no such customer. Supports conditional GET.")]
public Customer GetCustomer(string id)
{
    lock (writeLock)
    {
        // return NotFound if there is no item with the specified id.
        object itemEtag = customerEtags[id];
        if (itemEtag == null)
        {
            throw new WebFaultException(HttpStatusCode.NotFound);
        }
  
        // return NotModified if the client did a conditional GET and the customer item has not changed
        // since when the client last retrieved it
        WebOperationContext.Current.IncomingRequest.CheckConditionalRetrieve((long)itemEtag);
        Customer result = this.customers[id] as Customer;

        // set the customer etag before returning the result
        WebOperationContext.Current.OutgoingResponse.SetETag((long)itemEtag);
        return result;
    }
}
```  
  
## <a name="security-considerations"></a>安全注意事项  

 对于需要授权的请求，不应缓存其响应，因为从缓存中提供响应时不会执行该授权。  缓存此类响应将会带来严重的安全漏洞。  通常，需要授权的请求会提供用户特定数据，因此服务器端缓存也没有益处。  在这种情况下，客户端缓存或根本不缓存将更合适。
