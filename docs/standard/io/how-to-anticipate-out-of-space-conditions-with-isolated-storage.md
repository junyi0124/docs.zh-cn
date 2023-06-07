---
title: 如何：预见独立存储中的空间不足条件
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data stores, quotas
- isolated storage, quotas
- quantity of isolated storage used
- limit on isolated storage used
- stores, quotas
- stores, out of space conditions
- data storage using isolated storage, quotas
- storing data using isolated storage, quotas
- space remaining in isolated storage
- data stores, out of space conditions
- storing data using isolated storage, out of space conditions
- quotas for isolated storage
- isolated storage, out of space conditions
- data storage using isolated storage, out of space conditions
ms.assetid: e35d4535-3732-421e-b1a3-37412e036145
ms.openlocfilehash: c8052030cdc385fc54852bb8ec48c783f2ee5bc4
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830839"
---
# <a name="how-to-anticipate-out-of-space-conditions-with-isolated-storage"></a>如何：预见独立存储中的空间不足条件

使用独立存储的代码受[配额](isolated-storage.md#quotas)限制，配额指定了独立存储文件和目录所在数据隔离舱的大小上限。 该配额由安全策略定义，管理员可以对其进行配置。 如果尝试写入数据时超过了所允许的最大大小，将引发 <xref:System.IO.IsolatedStorage.IsolatedStorageException> 异常并使操作失败。 这有助于防止恶意的拒绝服务攻击，此类攻击可能会导致应用因为数据存储已满而拒绝请求。

为有助于确定给定写入尝试是否有可能由于此原因而失败，<xref:System.IO.IsolatedStorage.IsolatedStorage> 类提供了下列三个只读属性：<xref:System.IO.IsolatedStorage.IsolatedStorage.AvailableFreeSpace%2A>、<xref:System.IO.IsolatedStorage.IsolatedStorage.UsedSize%2A> 和 <xref:System.IO.IsolatedStorage.IsolatedStorage.Quota%2A>。 您可以使用这些属性来确定写入存储区是否将导致超过存储区所允许的最大大小。 记住独立存储可能被同时访问；因此，当计算剩余存储量时，该存储空间可能在您尝试写入存储区时已被使用。 但是，您可以使用存储区的最大大小来确定是否将达到可用存储的上限。

<xref:System.IO.IsolatedStorage.IsolatedStorage.Quota%2A> 属性依赖程序集中的证据来正常工作。 因此，只应在使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile>、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A> 或 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> 方法创建的 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> 对象上检索此属性。 以其他任何方式创建的 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> 对象（例如从 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetEnumerator%2A> 方法返回的对象）将无法返回准确的最大大小。

## <a name="example"></a>示例

下面的代码示例获取独立存储，创建一些文件，并检索 <xref:System.IO.IsolatedStorage.IsolatedStorage.AvailableFreeSpace%2A> 属性。 剩余空间以字节为单位进行报告。

[!code-cpp[Conceptual.IsolatedStorage#8](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source7.cpp#8)]
[!code-csharp[Conceptual.IsolatedStorage#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source7.cs#8)]
[!code-vb[Conceptual.IsolatedStorage#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source7.vb#8)]

## <a name="see-also"></a>另请参阅

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- [独立存储](isolated-storage.md)
- [如何：获取独立存储的存储区](how-to-obtain-stores-for-isolated-storage.md)
