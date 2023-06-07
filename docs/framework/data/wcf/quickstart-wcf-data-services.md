---
title: 快速入门（WCF 数据服务）
ms.date: 08/24/2018
helpviewer_keywords:
- WCF Data Services, quick-start example
- WCF Data Services, Entity Data Model (EDM) service
ms.assetid: 7b18ca1e-d4d6-4c7a-afb9-ce3cebb98a8d
ms.openlocfilehash: f945d33a4fc789b3c73c1c80040fc8527301758b
ms.sourcegitcommit: 43cbde34970f5f38f30c43cd63b9c7e2e83717ae
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/11/2020
ms.locfileid: "81121212"
---
# <a name="quickstart-wcf-data-services"></a>快速入门（WCF 数据服务）

此快速入门可帮助您通过一系列支持[入门](getting-started-with-wcf-data-services.md)主题的任务熟悉 WCF 数据服务和开放数据协议 （OData）。

## <a name="what-youll-learn"></a>学习内容

此快速入门中的第一个任务演示如何创建数据服务以公开来自北风示例数据库的 OData 源。 在后面的主题中，您将使用 Web 浏览器访问 OData 源，还可以创建一个 Windows 演示文稿基础 （WPF） 客户端应用程序，该应用程序使用客户端库使用 OData 源。

## <a name="prerequisites"></a>先决条件

要完成此快速入门，请安装以下组件：

- Visual Studio

- SQL 服务器的实例。 这包括 SQL Server Express，它包含在 Visual Studio 2015 的默认安装中，或作为 Visual Studio 2017 或更高版本中**的数据存储和处理**工作负载的一部分。

- Northwind 示例数据库。 要安装数据库，请从北风运行 Transact-SQL 脚本[，并为 Microsoft SQL Server 运行 pubs 示例数据库](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs)。

## <a name="wcf-data-services-quickstart-tasks"></a>WCF 数据服务快速启动任务

 [创建数据服务](creating-the-data-service.md)

 定义 ASP.NET 应用程序，定义数据模型，创建数据服务，并启用对资源的访问。

 [从 Web 浏览器访问服务](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)

 通过 Visual Studio 启动服务，并通过 Web 浏览器向公开的源提交 HTTP GET 请求以访问该服务。

 [创建 .NET 框架客户端应用程序](creating-the-dotnet-client-application-wcf-data-services-quickstart.md)

 创建 WPF 应用以使用 OData 源、将数据绑定到 Windows 控件、更改绑定控件中的数据，然后将更改发送回数据服务。

> [!NOTE]
> 项目文件从已完成版本的快速启动可以从[GitHub](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/WCF%20Data%20Services%20Quickstart%20(OData%20Service%20and%20WPF%20Client))下载。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [开始快速入门](creating-the-data-service.md)

## <a name="see-also"></a>请参阅

- [ADO.NET 实体框架](../adonet/ef/index.md)
