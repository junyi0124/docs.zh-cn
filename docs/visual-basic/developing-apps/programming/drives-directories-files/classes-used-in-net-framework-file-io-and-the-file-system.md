---
title: 在 .NET Framework 文件 I/O 和文件系统中使用的类
ms.date: 07/20/2015
helpviewer_keywords:
- file I/O classes
ms.assetid: 4a5ca924-eea8-4a95-a5f0-6ac10de276a3
ms.openlocfilehash: 2c696d5b8c83ae55caf61e4710630ad1e34c088d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401766"
---
# <a name="classes-used-in-net-framework-file-io-and-the-file-system-visual-basic"></a>在 .NET Framework 文件 I/O 和文件系统中使用的类 (Visual Basic)

下列各表列出了常用于 .NET Framework 文件 I/O 的类，将这些类分为文件 I/O 类、用于创建流的类和用于读取和写入流的类。  
  
要获取更完整的列表，请参阅[类库概述](../../../../standard/class-library-overview.md)。  
  
## <a name="basic-io-classes-for-files-drives-and-directories"></a>用于文件、驱动器和目录的基本 I/O 类  

 下表列出并说明了用于文件 I/O 的主类。  
  
|类|说明|  
|-----------|-----------------|  
|<xref:System.IO.Directory?displayProperty=nameWithType>|提供用于创建、移动和枚举目录和子目录的静态方法。|  
|<xref:System.IO.DirectoryInfo?displayProperty=nameWithType>|提供用于创建、移动和枚举目录和子目录的实例方法。|  
|<xref:System.IO.DriveInfo?displayProperty=nameWithType>|提供通过驱动器用于创建、移动和枚举的实例方法。|  
|<xref:System.IO.File?displayProperty=nameWithType>|提供用于创建、复制、删除、移动和打开文件的静态方法，并可帮助创建 `FileStream`。|  
|<xref:System.IO.FileAccess?displayProperty=nameWithType>|定义文件的读取、写入或读/写访问权限的常量。|  
|<xref:System.IO.FileAttributes?displayProperty=nameWithType>|提供文件和目录的属性，例如 `Archive`、`Hidden` 和 `ReadOnly`。|  
|<xref:System.IO.FileInfo?displayProperty=nameWithType>|提供用于创建、复制、删除、移动和打开文件的静态方法，并可帮助创建 `FileStream`。|  
|<xref:System.IO.FileMode?displayProperty=nameWithType>|控制打开文件的方式。 在多个 `FileStream` 和 `IsolatedStorageFileStream` 的构造函数中指定此参数，此参数用于 <xref:System.IO.File> 和 <xref:System.IO.FileInfo> 的 `Open` 方法。|  
|<xref:System.IO.FileShare?displayProperty=nameWithType>|定义用于控制其他文件流可以对同一文件进行何种类型的访问的常量。|  
|<xref:System.IO.Path?displayProperty=nameWithType>|提供用于处理目录字符串的方法和属性。|  
|<xref:System.Security.Permissions.FileIOPermission?displayProperty=nameWithType>|通过定义 <xref:System.Security.Permissions.FileIOPermissionAttribute.Read%2A>、<xref:System.Security.Permissions.FileIOPermissionAttribute.Write%2A>、<xref:System.Security.Permissions.FileIOPermissionAttribute.Append%2A> 和 <xref:System.Security.Permissions.FileIOPermissionAttribute.PathDiscovery%2A> 权限来控制对文件和文件夹的访问。|  
  
## <a name="classes-used-to-create-streams"></a>用于创建流的类  

 下表列出并说明了用于创建流的主类。  
  
|类|说明|  
|-----------|-----------------|  
|<xref:System.IO.BufferedStream?displayProperty=nameWithType>|将缓冲层添加到另一个流上的读取和写入操作。|  
|<xref:System.IO.FileStream?displayProperty=nameWithType>|支持通过文件的 <xref:System.IO.FileStream.Seek%2A> 方法对其进行随机访问。 默认情况下 <xref:System.IO.FileStream> 同步打开文件，但也支持异步操作。|  
|<xref:System.IO.MemoryStream?displayProperty=nameWithType>|创建一个流，其后备存储为内存而非文件。|  
|<xref:System.Net.Sockets.NetworkStream?displayProperty=nameWithType>|为网络访问提供数据的基础流。|  
|<xref:System.Security.Cryptography.CryptoStream?displayProperty=nameWithType>|定义将数据流链接到加密转换的流。|  
  
## <a name="classes-used-to-read-from-and-write-to-streams"></a>用于读取和写入到流的类  

 下表显示使用流读取和写入到文件的特定类。  
  
|**类**|**说明**|  
|---------------|---------------------|  
|<xref:System.IO.BinaryReader?displayProperty=nameWithType>|从 <xref:System.IO.FileStream> 读取编码字符串和基元数据类型。|  
|<xref:System.IO.BinaryWriter?displayProperty=nameWithType>|将编码字符串和基元数据类型写入 <xref:System.IO.FileStream>。|  
|<xref:System.IO.StreamReader?displayProperty=nameWithType>|从 <xref:System.IO.FileStream> 读取字符，使用 <xref:System.IO.StreamReader.CurrentEncoding%2A> 将字符转换为字节，或将字节转换为字符。 对于给定的流，<xref:System.IO.StreamReader> 的构造函数基于特定 <xref:System.IO.StreamReader.CurrentEncoding%2A> 报头的状态，尝试确定正确的 <xref:System.IO.StreamReader.CurrentEncoding%2A>，如字节顺序标记。|  
|<xref:System.IO.StreamWriter?displayProperty=nameWithType>|将字符写入 `FileStream`，使用 <xref:System.IO.StreamWriter.Encoding%2A> 将字符转换为字节。|  
|<xref:System.IO.StringReader?displayProperty=nameWithType>|从 `String` 读取字符。 输出可以是任意编码中的流或 `String`。|  
|<xref:System.IO.StringWriter?displayProperty=nameWithType>|将字符写入 `String`。 输出可以是任意编码中的流或 `String`。|  
  
## <a name="see-also"></a>另请参阅

- [撰写流](../../../../standard/io/composing-streams.md)
- [文件和流 I/O](../../../../standard/io/index.md)
- [异步文件 I/O](../../../../standard/io/asynchronous-file-i-o.md)
- [.NET Framework 文件 I/O 和文件系统基础知识 (Visual Basic)](basics-of-net-framework-file-io-and-the-file-system.md)
