---
title: 使用 JSONP
ms.date: 03/30/2017
ms.assetid: f386718c-b4ba-4931-a610-40c27a46672a
ms.openlocfilehash: 82290319b5d8b58708f0b2ebf40522ee76127b84
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594954"
---
# <a name="using-jsonp"></a>使用 JSONP

JSON Padding (JSONP) 是一种在 Web 浏览器中启用跨站点脚本支持的机制。 JSONP 是围绕 Web 浏览器的功能而设计的，用于从检索当前所加载文档的站点之外的站点加载脚本。 该机制的工作原理是使用用户定义的回调函数名来填充 JSON 负载，如下面的示例所示。

```javascript
callback({"a" = \\"b\\"});
```

在上例中，JSON 负载 `{"a" = \\"b\\"}` 包装在函数调用 `callback` 中。 必须已在当前网页中定义回调函数。 JSONP 响应的内容类型为 `application/javascript` 。

JSONP 不会自动启用。 若要启用 JSONP，请在某个 HTTP 标准终结点（`javascriptCallbackEnabled` 或 `true`）上将 <xref:System.ServiceModel.Description.WebHttpEndpoint> 特性设置为 <xref:System.ServiceModel.Description.WebScriptEndpoint>，如下面的示例所示。

```xml
<system.serviceModel>
  <standardEndpoints>
    <webHttpEndpoint>
      <standardEndpoint name="" javascriptCallbackEnabled="true"/>
    </webHttpEndpoint>
  </standardEndpoints>
</system.serviceModel>
```

可以在称为回调的查询变量中指定回调函数的名称，如下面的 URL 中所示。

`http://baseaddress/Service/RestService?callback=functionName`

调用时，该服务将发送如下响应。

```javascript
functionName({"root":"Something"});
```  

也可以通过对服务类应用 <xref:System.ServiceModel.Web.JavascriptCallbackBehaviorAttribute> 来指定回调函数名称，如下面的示例所示。

```csharp
[ServiceContract]
[JavascriptCallbackBehavior(ParameterName = "$callback")]
public class Service1
{
    [OperationContract]
    [WebGet(ResponseFormat=WebMessageFormat.Json)]
    public string GetData()
    {
    }
}
```

对于上面显示的服务，将发出如下请求。

`http://baseaddress/Service/RestService?$callback=anotherFunction`

调用时，该服务通过以下内容进行响应。

```javascript
anotherFunction ({"root":"Something"});
```

## <a name="http-status-codes"></a>HTTP 状态代码

具有 200 以外的 HTTP 状态代码的 JSONP 响应包括另一个参数，该参数使用 HTTP 状态代码的数字表示形式，如下面的示例所示。

```javascript
anotherFunction ({"root":"Something"}, 201);
```

## <a name="validations"></a>验证

启用 JSONP 后将执行以下验证：

- 如果启用了 `javascriptCallback`，请求中存在回调查询字符串参数，且将响应格式设置为 JSON，WCF 基础结构将引发异常。

- 如果请求包含回调查询字符串参数，但操作不是 HTTP GET，则将忽略回调参数。

- 如果回调名称为 `null` 或空字符串，则响应的格式不会设置为 JSONP。

## <a name="see-also"></a>另请参阅

- [WCF Web HTTP 编程模型概述](wcf-web-http-programming-model-overview.md)
