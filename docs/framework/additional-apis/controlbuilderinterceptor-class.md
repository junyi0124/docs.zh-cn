---
title: ControlBuilderInterceptor 类
ms.date: 08/11/2020
api_name:
- System.Web.Compilation.ControlBuilderInterceptor
api_location:
- System.Web.dll
api_type:
- Assembly
topic_type:
- apiref
ms.openlocfilehash: 0cd7409deb9cb84783cfa70600999fa4b2a2d2e2
ms.sourcegitcommit: d337df55f83325918cbbd095eb573400bea49064
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2020
ms.locfileid: "88187991"
---
# <a name="controlbuilderinterceptor-class"></a>ControlBuilderInterceptor 类

`ControlBuilderInterceptor`类允许自定义或控制编译过程。

## <a name="syntax"></a>语法

```csharp
internal class ControlBuilderInterceptor
```

> [!WARNING]
> `ControlBuilderInterceptor`类是内部的，不应在代码中直接使用。
>
> 如 "备注" 部分所述，可以检查是否存在此类型，以确定是否存在侦听器类型支持。 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="remarks"></a>备注

在 .NET Framework 2.0 和 .NET Framework 3.5 中， [8 月 2020](https://portal.msrc.microsoft.com/security-guidance/releasenotedetail/2020-Aug) 更新添加了对使用侦听器类型自定义或控制编译过程的支持。 您可以使用检查是否存在该类型来确定是否存在此支持 <xref:System.Type.GetType?displayProperty=nameWithType> `ControlBuilderInterceptor` ，如以下代码所示。

```csharp
Type type = Type.GetType("System.Web.Compilation.ControlBuilderInterceptor, System.Web, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a");
```

如果返回值不为 null，则表示侦听器支持。 如果返回值为 `null` ，或者引发了异常，则不会安装8月2020的更新，并且不存在侦听器支持。

如果提供程序支持，则可以编写并注册与编译过程交互的侦听器类型，就像在 <xref:System.Web.Compilation.ControlBuilderInterceptor> 更高版本的 .NET Framework 上执行的操作一样。 在 .NET Framework 2.0 和 .NET Framework 3.5 中，侦听器类型可以是满足以下要求的任何类：

* 具有一个公共的无参数构造函数。
* 具有与 `PreControlBuilderInit` `OnProcessGeneratedCode` 和方法相同的、具有与和方法相同的签名和语义的公共非静态方法 <xref:System.Web.Compilation.ControlBuilderInterceptor.PreControlBuilderInit(System.Web.UI.ControlBuilder,System.Web.UI.TemplateParser,System.Web.UI.ControlBuilder,System.Type,System.String,System.String,System.Collections.IDictionary,System.Collections.IDictionary)> <xref:System.Web.Compilation.ControlBuilderInterceptor.OnProcessGeneratedCode(System.Web.UI.ControlBuilder,System.CodeDom.CodeCompileUnit,System.CodeDom.CodeTypeDeclaration,System.CodeDom.CodeTypeDeclaration,System.CodeDom.CodeMemberMethod,System.CodeDom.CodeMemberMethod,System.Collections.IDictionary)> ，这些方法存在于 .NET Framework 的更高版本中。

使用 `aspnet:20ControlBuilderInterceptor` ASP.NET 应用程序设置 () 中的密钥注册侦听器类型 `<appSettings>` 。 此应用程序设置必须在您的计算机或应用程序 *web.config* 文件中列出。 使用程序集限定的类型名称指定侦听器类型。 下面的示例演示如何注册名为的侦听器类型 `Fabrikam.Interceptor` 。

```xml
<configuration>
  ...
  <appSettings>
    ...
    <add key="aspnet:20ControlBuilderInterceptor"
         value="Fabrikam.Interceptor, Fabrikam, Version=1.0.0.0, Culture=neutral, PublicKeyToken=2b3831f2f2b744f7" />
  </appSettings>
</configuration>
```

若要检索某个类型的程序集限定名称，请使用 <xref:System.Type.AssemblyQualifiedName?displayProperty=nameWithType> 属性，如下面的代码所示。

```csharp
string assemblyQualifiedName = typeof(Fabrikam.Interceptor).AssemblyQualifiedName;
```

当存在侦听器支持时，编译过程将按照上述方式与列出的类型进行交互。 当不存在侦听器支持时，应用程序设置将被忽略且不起作用。

## <a name="requirements"></a>要求

**命名空间：** System.web. 编译

**程序集：** System.Web.dll 中的 () 

**.NET Framework 版本：** 3.5、2。0
