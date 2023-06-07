---
title: 如何：手动生成客户端数据服务类（WCF 数据服务）
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, configuring
- WCF Data Services, client library
ms.assetid: b98cb1d6-956a-4e50-add6-67e4f2587346
ms.openlocfilehash: 368f2546652d21be44c0ffb4cc5f279c56beda51
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91165987"
---
# <a name="how-to-manually-generate-client-data-service-classes-wcf-data-services"></a>如何：手动生成客户端数据服务类（WCF 数据服务）

WCF 数据服务与 Visual Studio 集成，使您能够在使用 " **添加服务引用** " 对话框在 Visual Studio 项目中添加对数据服务的引用时自动生成客户端数据服务类。 有关详细信息，请参阅 [如何：添加数据服务引用](how-to-add-a-data-service-reference-wcf-data-services.md)。 此外，你也可以使用代码生成工具 `DataSvcUtil.exe` 手动生成相同的客户端数据服务类。 此工具随 WCF 数据服务提供，它将从数据服务定义生成 .NET Framework 类。 还可以使用此工具根据概念模型 (.csdl) 文件和表示 Visual Studio 项目中的实体框架模型的 .edmx 文件生成数据服务类。

 本主题中的示例基于 Northwind 示例数据服务创建客户端数据服务类。 此服务是在完成 [WCF 数据服务快速入门](quickstart-wcf-data-services.md)时创建的。 本主题中的某些示例需要 Northwind 模型的概念模型文件。 有关详细信息，请参阅 [如何：使用 EdmGen.exe 生成模型和映射文件](../adonet/ef/how-to-use-edmgen-exe-to-generate-the-model-and-mapping-files.md)。 本主题中的某些示例需要 Northwind 模型的 .edmx 文件。 有关详细信息，请参阅 [.Edmx 文件概述](/previous-versions/dotnet/netframework-4.0/cc982042(v=vs.100))。

### <a name="to-generate-c-classes-that-support-data-binding"></a>生成支持数据绑定的 C# 类

- 在命令提示符下执行以下命令（无换行符）：

    ```console
    "%windir%\Microsoft.NET\Framework\v3.5\DataSvcUtil.exe" /dataservicecollection /version:2.0 /language:CSharp /out:Northwind.cs /uri:http://localhost:12345/Northwind.svc
    ```

    > [!NOTE]
    > 必须用 Northwind 示例数据服务实例的 URI 替换向 `/uri:` 参数提供的值。

### <a name="to-generate-visual-basic-classes-that-support-data-binding"></a>生成支持数据绑定的 Visual Basic 类

- 在命令提示符下执行以下命令（无换行符）：

    ```console
    "%windir%\Microsoft.NET\Framework\v3.5\DataSvcUtil.exe" /dataservicecollection /version:2.0 /language:VB /out:Northwind.vb /uri:http://localhost:12345/Northwind.svc
    ```

    > [!NOTE]
    > 必须用 Northwind 示例数据服务实例的 URI 替换向 `/uri:` 参数提供的值。

### <a name="to-generate-c-classes-based-on-the-service-uri"></a>基于服务 URI 生成 C# 类

- 在命令提示符下执行以下命令（无换行符）：

    ```console
    "%windir%\Microsoft.NET\Framework\v3.5\DataSvcUtil.exe" /language:CSharp /out:northwind.cs /uri:http://localhost:12345/Northwind.svc
    ```

    > [!NOTE]
    > 必须用 Northwind 示例数据服务实例的 URI 替换向 `/uri:` 参数提供的值。

### <a name="to-generate-visual-basic-classes-based-on-the-service-uri"></a>基于服务 URI 生成 Visual Basic 类

- 在命令提示符下执行以下命令（无换行符）：

    ```console
    "%windir%\Microsoft.NET\Framework\v3.5\datasvcutil.exe" /language:VB /out:Northwind.vb /uri:http://localhost:12345/Northwind.svc
    ```

    > [!NOTE]
    > 必须用 Northwind 示例数据服务实例的 URI 替换向 `/uri:` 参数提供的值。

### <a name="to-generate-c-classes-based-on-the-conceptual-model-file-csdl"></a>基于概念模型文件 (CSDL) 生成 C# 类

- 在命令提示符下执行以下命令（无换行符）：

    ```console
    "%windir%\Microsoft.NET\Framework\v3.5\datasvcutil.exe" /language:CSharp /in:Northwind.csdl /out:Northwind.cs
    ```

### <a name="to-generate-visual-basic-classes-based-on-the-conceptual-model-file-csdl"></a>基于概念模型文件 (CSDL) 生成 Visual Basic 类

- 在命令提示符下执行以下命令（无换行符）：

    ```console
    "%windir%\Microsoft.NET\Framework\v3.5\datasvcutil.exe" /language:VB /in:Northwind.csdl /out:Northwind.vb
    ```

### <a name="to-generate-c-classes-based-on-the-edmx-file"></a>基于 .edmx 文件生成 C# 类

- 在命令提示符下执行以下命令（无换行符）：

    ```console
    "%windir%\Microsoft.NET\Framework\v3.5\datasvcutil.exe" /language:CSharp /in:Northwind.edmx /out:c:\northwind.cs
    ```

### <a name="to-generate-visual-basic-classes-based-on-the-edmx-file"></a>基于 .edmx 文件生成 Visual Basic 类

- 在命令提示符下执行以下命令（无换行符）：

    ```console
    "%windir%\Microsoft.NET\Framework\v3.5\datasvcutil.exe" /language:VB /in:Northwind.edmx /out:c:\northwind.vb
    ```

## <a name="see-also"></a>请参阅

- [生成数据服务客户端库](generating-the-data-service-client-library-wcf-data-services.md)
- [如何：添加数据服务引用](how-to-add-a-data-service-reference-wcf-data-services.md)
- [WCF 数据服务客户端实用工具 (DataSvcUtil.exe)](wcf-data-service-client-utility-datasvcutil-exe.md)
