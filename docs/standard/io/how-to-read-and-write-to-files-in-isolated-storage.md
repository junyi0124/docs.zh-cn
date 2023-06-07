---
title: 如何：在独立存储中读取和写入文件
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- files, isolated storage
- reading data
- storing data using isolated storage, reading and writing to files
- writing to files within store
- data storage using isolated storage, reading and writing to files
- reading files within store
- isolated storage, reading and writing to files
- data stores, reading and writing to files
- stores, reading and writing to files
ms.assetid: f977ebdc-1b55-475a-bc3d-3376470b08ae
ms.openlocfilehash: 3a8b783cf2cce93cb26b11823d9f565961376ca3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734590"
---
# <a name="how-to-read-and-write-to-files-in-isolated-storage"></a>如何：在独立存储中读取和写入文件

若要读取或写入独立存储区的文件，请使用包含流读取器（<xref:System.IO.IsolatedStorage.IsolatedStorageFileStream> 对象）或流编写器（<xref:System.IO.StreamReader> 对象）的 <xref:System.IO.StreamWriter> 对象。  
  
## <a name="example"></a>示例  

 下面的代码示例获取独立存储区，并检查存储区中是否存在 TestStore.txt 文件。 如果不存在，则创建该文件并将“Hello Isolated Storage”写入该文件。 如果 TestStore.txt 已存在，则示例代码会读取该文件。  
  
 [!code-csharp[Conceptual.IsolatedStorage#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source5.cs#5)]
 [!code-vb[Conceptual.IsolatedStorage#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source5.vb#5)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream>
- <xref:System.IO.FileMode?displayProperty=nameWithType>
- <xref:System.IO.FileAccess?displayProperty=nameWithType>
- <xref:System.IO.StreamReader?displayProperty=nameWithType>
- <xref:System.IO.StreamWriter?displayProperty=nameWithType>
- [文件和流 I/O](index.md)
- [独立存储](isolated-storage.md)
