---
title: 如何：获取独立存储的存储区
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- stores, obtaining
- storing data using isolated storage, obtaining stores
- isolated storage, obtaining stores
- data stores, obtaining
- data storage using isolated storage, obtaining stores
ms.assetid: fcb6b178-d526-47c4-b029-e946f880f9db
ms.openlocfilehash: ac53432374b546a46417e9d3b9de342a7ea2ef81
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734616"
---
# <a name="how-to-obtain-stores-for-isolated-storage"></a>如何：获取独立存储的存储区

独立存储区公开数据隔离舱中的虚拟文件系统。 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> 类提供许多与独立存储区交互的方法。 为了创建和检索存储区，<xref:System.IO.IsolatedStorage.IsolatedStorageFile> 提供了三种静态方法：  
  
- <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A> 返回按用户和程序集隔离的存储。  
  
- <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> 返回按域和程序集隔离的存储。  
  
     这两种方法检索属于所调用代码的存储区。  
  
- 静态方法 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> 返回通过传入一组范围参数指定的独立存储。  
  
 下面的示例返回按用户、程序集和域隔离的存储区。  
  
 [!code-cpp[Conceptual.IsolatedStorage#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source6.cpp#6)]
 [!code-csharp[Conceptual.IsolatedStorage#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source6.cs#6)]
 [!code-vb[Conceptual.IsolatedStorage#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source6.vb#6)]  
  
 您可以使用 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> 方法指定存储区应该和漫游用户配置文件一起漫游。 若要详细了解如何对此进行设置，请参阅[隔离类型](types-of-isolation.md)。  
  
 从不同程序集获取的独立存储默认就是不同的存储。 通过在 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> 方法的参数中传递程序集或域证据，可以访问不同程序集或域的存储区。 这需要拥有权限，才能按应用域标识访问独立存储。 有关详细信息，请参阅 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> 方法重载。  
  
 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A>、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> 和 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> 方法返回 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> 对象。 若要了解如何确定哪种隔离类型最适合自己的情况，请参阅[隔离类型](types-of-isolation.md)。 当您具有了独立存储文件对象时，您便可以使用独立存储方法来读取、写入、创建和删除文件及目录了。  
  
 没有一种机制可用来防止代码向没有足够访问权限来自己获取存储区的代码传递 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> 对象。 仅当获得对 <xref:System.IO.IsolatedStorage.IsolatedStorage> 对象的引用时（通常在 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A>、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> 或 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> 方法中），才会检查域和程序集标识以及独立存储权限。 因此，保护对 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> 对象的引用是使用这些引用的代码的责任。  
  
## <a name="example"></a>示例  

 下面的代码提供了一个简单的类示例，它包含按用户和程序集隔离的存储区。 通过向 <xref:System.IO.IsolatedStorage.IsolatedStorageScope.Domain?displayProperty=nameWithType> 方法传递的自变量添加 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A>，此代码可更改为检索按用户、域和程序集隔离的存储区。  
  
 运行代码后，可以在命令行处键入“ StoreAdm /LIST”  ，以确认存储是否已创建。 这会运行[独立存储工具 (Storeadm.exe)](../../framework/tools/storeadm-exe-isolated-storage-tool.md)，并列出用户当前的所有独立存储。  
  
 [!code-cpp[Conceptual.IsolatedStorage#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source6.cpp#7)]
 [!code-csharp[Conceptual.IsolatedStorage#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source6.cs#7)]
 [!code-vb[Conceptual.IsolatedStorage#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source6.vb#7)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- <xref:System.IO.IsolatedStorage.IsolatedStorageScope>
- [独立存储](isolated-storage.md)
- [隔离的类型](types-of-isolation.md)
- [.NET 中的程序集](../assembly/index.md)
