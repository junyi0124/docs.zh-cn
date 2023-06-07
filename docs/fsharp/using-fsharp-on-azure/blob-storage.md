---
title: 通过 F# 实现 Azure Blob 入门
description: 通过 Azure Blob 存储将非结构化数据存储在云中。
author: sylvanc
ms.date: 09/20/2016
ms.custom: devx-track-fsharp
ms.openlocfilehash: 91aec8fc2b57c71ce4ba47d62619912af6c71e59
ms.sourcegitcommit: a8a205034eeffc7c3e1bdd6f506a75b0f7099ebf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91756242"
---
# <a name="get-started-with-azure-blob-storage-using-f"></a>使用 F 开始使用 Azure Blob 存储\#

Azure Blob 存储是一种将非结构化数据作为对象/Blob 存储在云中的服务。 Blob 存储可以存储任何类型的文本或二进制数据，例如文档、媒体文件或应用程序安装程序。 Blob 存储也称为对象存储。

本文介绍如何使用 Blob 存储执行常见任务。 使用用于 .NET 的 Azure 存储客户端库的 F # 编写示例。 涉及的任务包括如何上传、列出、下载和删除 blob。

有关 blob 存储的概念性概述，请参阅 [blob 存储的 .net 指南](/azure/storage/blobs/storage-quickstart-blobs-dotnet)。

## <a name="prerequisites"></a>必备知识

若要使用本指南，必须先 [创建 Azure 存储帐户](/azure/storage/common/storage-account-create)。 还需要此帐户的存储访问密钥。

## <a name="create-an-f-script-and-start-f-interactive"></a>创建 F # 脚本并开始 F# 交互窗口

本文中的示例可用于 F # 应用程序或 F # 脚本。 若要创建 F # 脚本，请创建 `.fsx` 扩展名为的文件，例如 `blobs.fsx` f # 开发环境中的文件。

接下来，使用 [包管理器](package-management.md) （如 [Paket](https://fsprojects.github.io/Paket/) 或 [NuGet](https://www.nuget.org/) ）在 `WindowsAzure.Storage` 你的 `Microsoft.WindowsAzure.ConfigurationManager` `WindowsAzure.Storage.dll` `Microsoft.WindowsAzure.Configuration.dll` 脚本中使用指令安装和包和引用 `#r` 。

### <a name="add-namespace-declarations"></a>添加命名空间声明

将下列 `open` 语句添加到 `blobs.fsx` 文件顶部：

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L1-L5)]

### <a name="get-your-connection-string"></a>获取连接字符串

对于本教程，你需要一个 Azure 存储连接字符串。 有关连接字符串的详细信息，请参阅 [配置存储连接字符串](/azure/storage/storage-configure-connection-string)。

对于本教程，请在脚本中输入连接字符串，如下所示：

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L11-L11)]

但对于实际项目， **不建议这样做** 。 存储帐户密钥类似于存储帐户的根密码。 始终要小心保护存储帐户密钥。 避免将其分发给其他用户、对其进行硬编码或将其保存在其他人可以访问的纯文本文件中。 如果你认为密钥可能已泄漏，你可以使用 Azure 门户重新生成密钥。

对于实际应用程序，维护存储连接字符串的最佳方式是在配置文件中。 若要从配置文件中提取连接字符串，可以执行以下操作：

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L13-L15)]

不一定要使用 Azure Configuration Manager。 你还可以使用 API，例如 .NET Framework 的 `ConfigurationManager` 类型。

### <a name="parse-the-connection-string"></a>解析连接字符串

若要分析连接字符串，请使用：

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L21-L22)]

这会返回一个 `CloudStorageAccount` 。

### <a name="create-some-local-dummy-data"></a>创建一些本地虚拟数据

在开始之前，请在脚本的目录中创建一些虚拟本地数据。 稍后上载此数据。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L28-L30)]

### <a name="create-the-blob-service-client"></a>创建 Blob 服务客户端

`CloudBlobClient`类型使你能够检索存储在 Blob 存储中的容器和 blob。 下面是创建服务客户端的一种方法：

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L36-L36)]

现在，已准备好编写从 Blob 存储读取数据并将数据写入 Blob 存储的代码。

## <a name="create-a-container"></a>创建容器

此示例演示如何创建一个容器（如果该容器不存在）：

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L42-L46)]

默认情况下，新容器是专用容器，意思是必须指定存储访问密钥才能从该容器下载 blob。 如果要让容器中的文件可供所有人使用，则可以使用以下代码将容器设置为公共容器：

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L48-L49)]

Internet 中的所有人都可以查看公共容器中的 blob，但是，仅在具有相应的帐户访问密钥或共享的访问签名时，才能修改或删除它们。

## <a name="upload-a-blob-into-a-container"></a>将 Blob 上传到容器中

Azure Blob 存储支持块 Blob 和页 Blob。 在大多数情况下，建议使用块 blob。

要将文件上传到块 Blob，请获取容器引用，并使用它获取块 Blob 引用。 拥有 blob 引用后，可以通过调用方法，将任何数据流上载到该 blob `UploadFromFile` 。 此操作将创建 blob （如果该 blob 不存在），或者覆盖它（如果该 blob 存在）。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L55-L59)]

## <a name="list-the-blobs-in-a-container"></a>列出容器中的 blob

若要列出容器中的 Blob，首先需要获取容器引用。 然后，可以使用容器的 `ListBlobs` 方法检索其中的 blob 和/或目录。 若要访问返回的的丰富属性和方法集 `IListBlobItem` ，必须将其强制转换为 `CloudBlockBlob` 、 `CloudPageBlob` 或 `CloudBlobDirectory` 对象。 如果类型未知，可以使用类型检查来确定要将其转换为哪种类型。 以下代码演示了如何检索和输出 `mydata` 容器中每一项的 URI：

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L67-L80)]

还可以在名称中命名包含路径信息的 blob。 这会创建一个虚拟目录结构，可以像传统文件系统一样组织和遍历。 目录结构只是虚拟的-Blob 存储中唯一可用的资源是容器和 blob。 但是，存储客户端库提供了一个 `CloudBlobDirectory` 对象来引用虚拟目录，并简化了以这种方式组织的 blob 的使用过程。

例如，考虑名为 `photos`的容器中包含的下面一组块 Blob：

*photo1.jpg*\
*2015/体系结构/description.txt*\
*2015/体系结构/photo3.jpg*\
*2015/体系结构/photo4.jpg*\
*2016/体系结构/photo5.jpg*\
*2016/体系结构/photo6.jpg*\
*2016/体系结构/description.txt*\
*2016/photo7.jpg*\

当你在 `ListBlobs` 容器上调用时 (如以上示例) 中所示，将返回一个层次结构列表。 如果它包含 `CloudBlobDirectory` 和 `CloudBlockBlob` 对象，分别表示容器中的目录和 blob，则生成的输出如下所示：

```console
Directory: https://<accountname>.blob.core.windows.net/photos/2015/
Directory: https://<accountname>.blob.core.windows.net/photos/2016/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

还可以选择将方法的 `UseFlatBlobListing` 参数设置 `ListBlobs` 为 `true` 。 在这种情况下，容器中的每个 blob 都作为 `CloudBlockBlob` 对象返回。 对的调用 `ListBlobs` 将返回一个平面列表，如下所示：

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L82-L89)]

而且，根据容器的当前内容，结果如下所示：

```console
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2015/architecture/description.txt
Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2015/architecture/photo3.jpg
Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2015/architecture/photo4.jpg
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2016/architecture/description.txt
Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2016/architecture/photo5.jpg
Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2016/architecture/photo6.jpg
Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2016/photo7.jpg
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

## <a name="download-blobs"></a>下载 Blob

若要下载 blob，请首先检索 blob 引用，然后调用 `DownloadToStream` 方法。 下面的示例使用 `DownloadToStream` 方法将 blob 内容传输到一个流对象，然后你可以将该对象保存到本地文件。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L95-L101)]

你还可以使用 `DownloadToStream` 方法以文本字符串形式下载 blob 的内容。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L103-L106)]

## <a name="delete-blobs"></a>删除 Blob

若要删除 blob，首先要获取 blob 引用，然后对 `Delete` 其调用方法。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L112-L116)]

## <a name="list-blobs-in-pages-asynchronously"></a>以异步方式列出页中的 Blob

如果要列出大量 Blob，或需要控制一个列表操作中返回的结果数，则可以结果页的方式列出 Blob。 此示例显示如何以页面形式异步返回结果，这样就不会在等待返回大型结果集时阻止操作的执行。

此示例演示平面 blob 列表，但你也可以通过将 `useFlatBlobListing` 方法的参数设置为来执行层次结构列表 `ListBlobsSegmentedAsync` `false` 。

该示例使用块定义了一个异步方法 `async` 。 ``let!``关键字将挂起示例方法的执行，直至列表任务完成。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L122-L160)]

现在，我们可以按如下所示使用此异步例程。 首先，使用在本教程) 之前创建的本地文件上传一些虚拟数据 (。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L162-L166)]

现在，调用例程。 使用 `Async.RunSynchronously` 强制执行异步操作。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L168-L168)]

## <a name="writing-to-an-append-blob"></a>写入追加 Blob

追加 Blob 针对追加操作（例如日志记录）进行了优化。 与块 blob 一样，追加 blob 由块组成，但是当你将新块添加到追加 blob 时，它始终追加到 blob 的末尾。 不能更新或删除追加 Blob 中现有的块。 追加 Blob 的块 ID 不公开，因为它们是用于一个块 Blob 的。

追加 Blob 中的每个块可以有不同的大小，最大为 4 MB，并且追加 Blob 最多可包含 50000 个块。 因此，追加 Blob 的最大大小稍微大于 195 GB（4 MB X 50000 块）。

下面的示例创建一个新的追加 blob 并向其追加一些数据，模拟一个简单的日志记录操作。

[!code-fsharp[BlobStorage](~/samples/snippets/fsharp/azure/blob-storage.fsx#L174-L203)]

请参阅 [了解块 Blob、页 Blob 和追加 Blob](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) ，就有关三种 Blob 之间的差异了解详细信息。

## <a name="concurrent-access"></a>并发访问

若要允许从多个客户端或多个进程实例并发访问某个 Blob，可以使用 **ETag** 或**租约**。

- **Etag** - 用于检测 Blob 或容器是否已被其他进程修改

- **租约** -提供一种在一段时间内获取对 blob 的独占、可续订、写入或删除访问的方法

有关详细信息，请参阅 [Microsoft Azure 存储中的 "管理并发](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/)"。

## <a name="naming-containers"></a>命名容器

Azure 存储中的每个 Blob 必须驻留在一个容器中。 该容器构成 Blob 名称的一部分。 例如，在这些示例 Blob URI 中， `mydata` 是容器的名称：

- `https://storagesample.blob.core.windows.net/mydata/blob1.txt`
- `https://storagesample.blob.core.windows.net/mydata/photos/myphoto.jpg`

容器名称必须是有效的 DNS 名称，并符合以下命名规则：

1. 容器名称必须以字母或数字开头，并且只能包含字母、数字和短划线 (-) 字符。
1. 每个短划线 (-) 字符的前面和后面都必须是一个字母或数字；在容器名称中不允许连续的短划线 (-)。
1. 容器名称中的所有字母都必须为小写。
1. 容器名称必须介于 3 到 63 个字符。

容器的名称必须始终为小写。 如果在容器名称中包括大写字母或以其他方式违反了容器命名规则，则可能会收到 400 错误（错误请求）。

## <a name="managing-security-for-blobs"></a>管理 Blob 安全性

默认情况下，Azure 存储会限制拥有帐户访问密钥的帐户所有者的访问权限来保持数据安全。 当需要共享存储帐户中的 Blob 数据时，请注意不可危及帐户访问密钥的安全性。 此外，可以加密 Blob 数据，以确保其在网络中传输时以及在 Azure 存储中时的安全性。

### <a name="controlling-access-to-blob-data"></a>控制对 Blob 数据的访问

默认情况下，存储帐户中的 Blob 数据仅供存储帐户所有者访问。 默认情况下，验证对 Blob 存储的请求需要帐户访问密钥。 但是，你可能希望使某些 blob 数据可供其他用户使用。

### <a name="encrypting-blob-data"></a>加密 Blob 数据

Azure 存储支持在客户端和服务器上加密 blob 数据。

## <a name="next-steps"></a>后续步骤

现在，已了解 Blob 存储的基础知识，可单击下面的链接了解详细信息。

### <a name="tools"></a>工具

- [F # AzureStorageTypeProvider](https://fsprojects.github.io/AzureStorageTypeProvider/)\
F # 类型提供程序，可用于浏览 Blob、表和队列的 Azure 存储资产，并可以轻松地对其应用 CRUD 操作。

- [Fsharp.core](https://github.com/fsprojects/FSharp.Azure.Storage)\
使用 Microsoft Azure 表存储服务的 F # API

- [Microsoft Azure 存储资源管理器 (MASE) ](/azure/vs-azure-tools-storage-manage-with-storage-explorer)\
Microsoft 提供的免费独立应用，可用于在 Windows、OS X 和 Linux 上以可视方式处理 Azure 存储数据。

### <a name="blob-storage-reference"></a>Blob 存储参考

- [用于 .NET 的 Azure 存储 API](/dotnet/api/overview/azure/storage)
- [Azure 存储服务 REST API 参考](/rest/api/storageservices/)

### <a name="related-guides"></a>相关指南

- [适用于 .NET 的 Azure Blob 存储示例](/samples/azure-samples/storage-blob-dotnet-getting-started/storage-blob-dotnet-getting-started/)
- [AzCopy 入门](/azure/storage/common/storage-use-azcopy-v10)
- [Configure Azure Storage connection strings](/azure/storage/common/storage-configure-connection-string)（配置 Azure 存储连接字符串）
- [Azure 存储团队博客](/archive/blogs/windowsazurestorage/)
- [快速入门：使用 .NET 在对象存储中创建 blob](/azure/storage/blobs/storage-quickstart-blobs-dotnet)
