---
title: 内存映射文件
description: 浏览 .NET 中的内存映射文件，这些文件包含虚拟内存中的文件内容，并允许应用程序直接写入内存来修改文件。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- memory-mapped files
- inter-process communication
ms.assetid: a483d1b5-64aa-45b6-86ef-11b859f7f02e
ms.openlocfilehash: 4a179bff7ec7988c5b7410fa99eab346d4add1df
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734824"
---
# <a name="memory-mapped-files"></a>内存映射文件

内存映射文件包含虚拟内存中文件的内容。 借助文件和内存空间之间的这种映射，应用（包括多个进程）可以直接对内存执行读取和写入操作，从而修改文件。 可以使用托管代码访问内存映射文件，就像本机 Windows 函数访问内存映射文件（如[管理内存映射文件](/previous-versions/ms810613(v=msdn.10))中所述）一样。  
  
内存映射文件分为两种类型：  
  
- 持久化内存映射文件  
  
     持久化文件是与磁盘上的源文件相关联的内存映射文件。 当最后一个进程处理完文件时，数据保存到磁盘上的源文件中。 此类内存映射文件适用于处理非常大的源文件。  
  
- 非持久化内存映射文件  
  
     非持久化文件是不与磁盘上的文件相关联的内存映射文件。 当最后一个进程处理完文件时，数据会丢失，且文件被垃圾回收器回收。 此类文件适合创建共享内存，以进行进程内通信 (IPC)。  
  
## <a name="processes-views-and-managing-memory"></a>进程、视图和管理内存  

 可以跨多个进程共享内存映射文件。 进程可以映射到相同的内存映射文件，只需使用文件创建进程分配的通用名称即可。  
  
 必须创建整个或部分内存映射文件的视图，才能使用内存映射文件。 还可以为内存映射文件的同一部分创建多个视图，从而创建并发内存。 若要让两个视图一直处于并发状态，必须通过同一个内存映射文件创建它们。  
  
 如果文件大于可用于内存映射的应用程序逻辑内存空间（在 32 位计算机中为 2 GB），可能也有必要使用多个视图。  
  
 视图分为以下两种类型：流访问视图和随机访问视图。 使用流访问视图，可以顺序访问文件；建议对非持久化文件和 IPC 使用这种类型。 随机访问视图是处理持久化文件的首选类型。  
  
 由于内存映射文件是通过操作系统的内存管理程序进行访问，因此文件会被自动分区到很多页面，并根据需要进行访问。 无需自行处理内存管理。  
  
 下图展示了多个进程如何同时对同一个内存映射文件有多个重叠视图。

 下图显示了内存映射文件的多个重叠视图：  
  
 ![显示内存映射文件的视图的屏幕截图。](./media/memory-mapped-files/memory-map-persist-file.png)  
  
## <a name="programming-with-memory-mapped-files"></a>使用内存映射文件编程  

 下表列出了与使用内存映射文件对象及其成员相关的指南。  
  
|任务|要使用的方法或属性|  
|----------|----------------------------------|  
|从磁盘上的文件获取表示持久化内存映射文件的 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> 对象。|<xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateFromFile%2A?displayProperty=nameWithType> 方法。|  
|获取表示非持久化内存映射文件的 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> 对象（未与磁盘上的文件关联）。|<xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateNew%2A?displayProperty=nameWithType> 方法。<br /><br /> - 或 -<br /><br /> <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateOrOpen%2A?displayProperty=nameWithType> 方法。|  
|获取现有内存映射文件（持久化或非持久化）的 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> 对象。|<xref:System.IO.MemoryMappedFiles.MemoryMappedFile.OpenExisting%2A?displayProperty=nameWithType> 方法。|  
|获取内存映射文件的顺序访问视图的 <xref:System.IO.UnmanagedMemoryStream> 对象。|<xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateViewStream%2A?displayProperty=nameWithType> 方法。|  
|获取内存映射文件的随机访问视图的 <xref:System.IO.UnmanagedMemoryAccessor> 对象。|<xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateViewAccessor%2A?displayProperty=nameWithType> 方法。|  
|获取要与非托管代码结合使用的 <xref:Microsoft.Win32.SafeHandles.SafeMemoryMappedViewHandle> 对象。|<xref:System.IO.MemoryMappedFiles.MemoryMappedFile.SafeMemoryMappedFileHandle%2A?displayProperty=nameWithType> 属性。<br /><br /> - 或 -<br /><br /> <xref:System.IO.MemoryMappedFiles.MemoryMappedViewAccessor.SafeMemoryMappedViewHandle%2A?displayProperty=nameWithType> 属性。<br /><br /> - 或 -<br /><br /> <xref:System.IO.MemoryMappedFiles.MemoryMappedViewStream.SafeMemoryMappedViewHandle%2A?displayProperty=nameWithType> 属性。|  
|将内存分配一直延迟到视图创建完成（仅限非持久化文件）。<br /><br /> （若要确定当前系统页面大小，请使用 <xref:System.Environment.SystemPageSize%2A?displayProperty=nameWithType> 属性。）|值为 <xref:System.IO.MemoryMappedFiles.MemoryMappedFileOptions.DelayAllocatePages?displayProperty=nameWithType> 的 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateNew%2A> 方法。<br /><br /> - 或 -<br /><br /> 将 <xref:System.IO.MemoryMappedFiles.MemoryMappedFileOptions> 枚举用作参数的 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateOrOpen%2A> 方法。|  
  
### <a name="security"></a>安全性  

 可以在创建内存映射文件时应用访问权限，具体操作是运行以下需要将 <xref:System.IO.MemoryMappedFiles.MemoryMappedFileAccess> 枚举用作参数的方法：  
  
- <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateFromFile%2A?displayProperty=nameWithType>  
  
- <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateNew%2A?displayProperty=nameWithType>  
  
- <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateOrOpen%2A?displayProperty=nameWithType>  
  
 若要指定打开现有内存映射文件所需的访问权限，可以运行需要将 <xref:System.IO.MemoryMappedFiles.MemoryMappedFileRights> 用作参数的 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.OpenExisting%2A> 方法。  
  
 另外，还可以添加包含预定义访问规则的 <xref:System.IO.MemoryMappedFiles.MemoryMappedFileSecurity> 对象。  
  
 若要将新的或更改后的访问规则应用于内存映射文件，请使用 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.SetAccessControl%2A> 方法。 若要从现有文件检索访问或审核规则，请使用 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.GetAccessControl%2A> 方法。  
  
## <a name="examples"></a>示例  
  
### <a name="persisted-memory-mapped-files"></a>持久化内存映射文件  

 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateFromFile%2A> 方法通过磁盘上的现有文件创建内存映射文件。  
  
 下面的示例为极大文件的一部分创建内存映射视图，并控制其中一部分。  
  
 [!code-csharp[MemoryMappedFiles.MemoryMappedFile.CreateFromFile#1](../../../samples/snippets/csharp/VS_Snippets_CLR/memorymappedfiles.memorymappedfile.createfromfile/cs/program.cs#1)]
 [!code-vb[MemoryMappedFiles.MemoryMappedFile.CreateFromFile#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/memorymappedfiles.memorymappedfile.createfromfile/vb/program.vb#1)]  

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

 下面的示例为另一个进程打开相同的内存映射文件。  
  
 [!code-csharp[MemoryMappedFiles.MemoryMappedFile.OpenExisting#1](../../../samples/snippets/csharp/VS_Snippets_CLR/memorymappedfiles.memorymappedfile.openexisting/cs/program.cs#1)]
 [!code-vb[MemoryMappedFiles.MemoryMappedFile.OpenExisting#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/memorymappedfiles.memorymappedfile.openexisting/vb/program.vb#1)]  
  
### <a name="non-persisted-memory-mapped-files"></a>非持久化内存映射文件  

 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateNew%2A> 和 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile.CreateOrOpen%2A> 方法创建未映射到磁盘上现有文件的内存映射文件。  
  
 下面的示例包含三个独立进程（控制台应用），以将布尔值写入内存映射文件。 各操作按下面的顺序发生：  
  
1. `Process A` 创建内存映射文件，并向其中写入值。  
  
2. `Process B` 打开内存映射文件，并向其中写入值。  
  
3. `Process C` 打开内存映射文件，并向其中写入值。  
  
4. `Process A` 读取并显示内存映射文件中的值。  
  
5. 在 `Process A` 处理完内存映射文件后，此文件立即被垃圾回收器回收。  
  
 若要运行此示例，请按照以下步骤操作：  
  
1. 编译应用并打开三个命令提示符窗口。  
  
2. 在第一个命令提示符窗口中，运行 `Process A`。  
  
3. 在第二个命令提示符窗口中，运行 `Process B`。  
  
4. 返回到 `Process A`，再按 Enter。  
  
5. 在第三个命令提示符窗口中，运行 `Process C`。  
  
6. 返回到 `Process A`，再按 Enter。  
  
 `Process A` 的输出如下所示：  
  
```console  
Start Process B and press ENTER to continue.  
Start Process C and press ENTER to continue.  
Process A says: True  
Process B says: False  
Process C says: True  
```  
  
 **Process A**  
  
 [!code-csharp[System.IO.MemoryMappedFiles_IPC_X#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_x/cs/program.cs#1)]
 [!code-vb[System.IO.MemoryMappedFiles_IPC_X#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_x/vb/program.vb#1)]  
  
 **Process B**  
  
 [!code-csharp[System.IO.MemoryMappedFiles_IPC_A#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_a/cs/program.cs#1)]
 [!code-vb[System.IO.MemoryMappedFiles_IPC_A#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_a/vb/program.vb#1)]  
  
 **Process C**  
  
 [!code-csharp[System.IO.MemoryMappedFiles_IPC_B#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_b/cs/program.cs#1)]
 [!code-vb[System.IO.MemoryMappedFiles_IPC_B#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.memorymappedfiles_ipc_b/vb/program.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [文件和流 I/O](index.md)
