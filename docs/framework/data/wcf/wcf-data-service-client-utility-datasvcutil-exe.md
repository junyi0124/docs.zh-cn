---
title: WCF 数据服务客户端实用工具 (DataSvcUtil.exe)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, generating client data classes
- WCF Data Services, client library
- WCF Data Services, consuming
ms.assetid: 9d0af606-929b-4c03-b307-3ef5f705afce
ms.openlocfilehash: 600cb9a4f91ff2051f60ee86d4cb80cc5b404c61
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90544292"
---
# <a name="wcf-data-service-client-utility-datasvcutilexe"></a>WCF 数据服务客户端实用工具 (DataSvcUtil.exe)

DataSvcUtil.exe 是 WCF 数据服务提供的命令行工具，它使用 Open Data Protocol (OData) 源，并生成从 .NET Framework 客户端应用程序访问数据服务所需的客户端数据服务类。 通过使用以下元数据源，该实用工具可以生成数据类：

- 数据服务的根 URI。 该实用工具会请求描述数据服务所公开的数据模型的服务元数据文档。 有关详细信息，请参阅 [AtomPub (RFC5023) ](https://tools.ietf.org/html/rfc5023#section-8)。

- 数据模型文件 () 使用概念架构定义语言 (CSDL) 定义，如[ \[ MC \] ：概念架构定义文件格式](/openspecs/windows_protocols/mc-csdl/c03ad8c3-e8b7-4306-af96-a9e52bb3df12)规范中所定义。

- 使用随实体框架提供的实体数据模型工具创建的 .edmx 文件。 有关详细信息，请参阅[ \[ MC-EDMX \] ：用于数据服务打包格式](/openspecs/windows_protocols/mc-edmx/5dff5e25-56a1-408b-9d44-bff6634c7d16)规范的实体数据模型。

有关详细信息，请参阅 [如何：手动生成客户端数据服务类](how-to-manually-generate-client-data-service-classes-wcf-data-services.md)。

DataSvcUtil.exe 工具安装在 .NET Framework 目录中。 在许多情况下，它位于 *C:\Windows\Microsoft.NET\Framework\v4.0*中。 对于64位系统，它位于 *C:\Windows\Microsoft.NET\Framework64\v4.0*中。 你还可以从 Visual Studio 的开发人员命令提示访问 DataSvcUtil.exe 工具。

## <a name="syntax"></a>语法

```console
datasvcutil /out:file [/in:file | /uri:serviceuri] [/dataservicecollection] [/language:devlang] [/nologo] [/version:ver] [/help]
```

## <a name="parameters"></a>参数

|选项|说明|
|------------|-----------------|
|`/dataservicecollection`|指定还生成了将对象绑定到控件所需的代码。|
|`/help`<br /><br /> \- 或 -<br /><br /> `/?`|显示该工具的命令语法和选项。|
|`/in:` *\<file>*|指定 .csdl 或 .edmx 文件或该文件所在的目录。|
|`/language:`[VB&#124;CSharp]|指定生成的源代码文件的语言。 默认语言为 C#。|
|`/nologo`|禁止显示版权信息。|
|`/out:` *\<file>*|指定包含生成的客户端数据服务类的源代码文件的名称。|
|`/uri:` *\<string>*|OData 源的 URI。|
|`/version:`[1.0&#124;2.0]|指定 OData 最高的接受版本。 根据 `DataServiceVersion` 返回的数据服务元数据中的 DataService 元素的属性确定版本。 有关详细信息，请参阅 [数据服务版本控制](data-service-versioning-wcf-data-services.md)。 指定 `/dataservicecollection` 参数时，还必须指定 `/version:2.0` 启用数据绑定。|

## <a name="see-also"></a>请参阅

- [生成数据服务客户端库](generating-the-data-service-client-library-wcf-data-services.md)
- [如何：添加数据服务引用](how-to-add-a-data-service-reference-wcf-data-services.md)
