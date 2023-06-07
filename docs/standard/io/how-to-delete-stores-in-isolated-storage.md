---
title: 如何：删除独立存储中的存储区
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- stores, deleting
- data stores, deleting
- deleting stores
- removing stores
- isolated storage, deleting stores
- storing data using isolated storage, deleting stores
- data storage using isolated storage, deleting stores
ms.assetid: 3947e333-5af6-4601-b2f1-24d4d6129cf3
ms.openlocfilehash: 88cb0b9c8a09e7eaf11a3e7b830dfbc8d595937b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731118"
---
# <a name="how-to-delete-stores-in-isolated-storage"></a>如何：删除独立存储中的存储区

<xref:System.IO.IsolatedStorage.IsolatedStorageFile> 类提供了两个用于删除独立存储文件的方法：  
  
- 实例方法 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.Remove> 不采用任何参数，并删除调用它的存储区。 此操作无需任何权限。 可以访问此存储区的任何代码都可以删除该存储区内的任何或所有数据。  
  
- 静态方法 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.Remove%28System.IO.IsolatedStorage.IsolatedStorageScope%29> 采用 <xref:System.IO.IsolatedStorage.IsolatedStorageScope.User> 枚举值，并删除当前运行该代码的用户的所有存储区。 此操作需要 <xref:System.Security.Permissions.IsolatedStorageFilePermission> 权限来写入 <xref:System.Security.Permissions.IsolatedStorageContainment.AdministerIsolatedStorageByUser> 值。  
  
## <a name="example"></a>示例  

 下面的代码示例演示了如何使用静态的实例方法 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.Remove%2A> 。 该类获取两个存储区：一个按用户和程序集隔离；另一个按用户、域和程序集隔离。 随后，将通过调用独立存储文件 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.Remove> 的  `isoStore1`方法来删除用户、域和程序集存储区。 接下来，将通过调用静态方法 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.Remove%28System.IO.IsolatedStorage.IsolatedStorageScope%29>来删除该用户的所有剩余存储区。  
  
 [!code-cpp[Conceptual.IsolatedStorage#3](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source3.cpp#3)]
 [!code-csharp[Conceptual.IsolatedStorage#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source3.cs#3)]
 [!code-vb[Conceptual.IsolatedStorage#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source3.vb#3)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- [独立存储](isolated-storage.md)
