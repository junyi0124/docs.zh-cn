---
description: '#pragma checksum - C# 参考'
title: '#pragma checksum - C# 参考'
ms.date: 07/20/2015
f1_keywords:
- '#pragma checksum'
helpviewer_keywords:
- '#pragma checksum [C#]'
ms.assetid: 3673e4ca-6098-4ec1-890f-8fceb2a794a2
ms.openlocfilehash: df665704ac813adbbf6473e81fad0a1c7ff616d0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91168564"
---
# <a name="pragma-checksum-c-reference"></a>#pragma checksum（C# 参考）

生成源文件的校验和以帮助调试 ASP.NET 页面。  
  
## <a name="syntax"></a>语法  
  
```csharp
#pragma checksum "filename" "{guid}" "checksum bytes"  
```  
  
## <a name="parameters"></a>参数  

 `"filename"`  
 需要监视更改或更新的文件的名称。  
  
 `"{guid}"`  
 哈希算法的全局唯一标识符 (GUID)。  
  
 `"checksum_bytes"`  
 表示校验和字节的十六进制数字的字符串。 必须是偶数个十六进制数字。 奇数个十六进制数字会导致编译时警告出现，且指令遭忽略。  
  
## <a name="remarks"></a>备注  

 Visual Studio 调试器使用校验和确保它可始终找到正确的源。 编译器为源文件计算校验和，然后将输出发出到程序数据库 (PDB) 文件。 调试器随后使用 PDB 针对它为源文件计算的校验和进行比较。  
  
 此解决方案不适用于 ASP.NET 项目，因为计算的校验和用于生成的源文件，而不用于 .aspx 文件。 为解决此问题，`#pragma checksum` 为 ASP.NET 页面提供校验和支持。  
  
 在 Visual C# 中创建 ASP.NET 项目时，生成的源文件包含 .aspx 文件（从该文件生成源）的校验和。 编译器随后将此信息写入 PDB 文件中。  
  
 如果编译器在文件中未遇到 `#pragma checksum` 指令，则它会计算校验和并将值写入 PDB 文件中。  
  
## <a name="example"></a>示例  
  
```csharp
class TestClass  
{  
    static int Main()  
    {  
        #pragma checksum "file.cs" "{406EA660-64CF-4C82-B6F0-42D48172A799}" "ab007f1d23d9" // New checksum  
    }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 预处理器指令](./index.md)
