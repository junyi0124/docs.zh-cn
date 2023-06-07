---
title: 安全性和进行中的代码生成
description: 对于以较高信任级别运行的代码，以较高信任级别运行代码，这是一个安全问题，尤其是当调用方可以影响代码生成时。
ms.date: 07/15/2020
helpviewer_keywords:
- code security, on-the-fly code generation
- on-the-fly code generation
- security [.NET], on-the-fly code generation
- secure coding, on-the-fly code generation
ms.assetid: 6d221724-bb21-4d76-90c3-0ee2a2e69be2
ms.openlocfilehash: c94da31f464a5272dd3f3c9f767a40ba7ad88a47
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94824143"
---
# <a name="security-and-on-the-fly-code-generation"></a>安全性和进行中的代码生成

某些库的操作方式是通过生成代码并运行它执行调用方的某项操作。 基本问题是代表信任级别较低的代码生成代码，并在更高的信任级别运行它。 当调用方可影响代码生成时，问题更为恶化，因此必须确保仅生成认为安全的代码。  
  
需要随时了解正在生成的具体代码。 这意味着必须严格控制从用户获取的任何值，无论它们是带引号的字符串（应转义以便不包含意外代码元素）、标识符（应检查以验证是否为有效标识符）或任何其他内容。 标识符可能很危险，因为可能修改已编译的程序集，使其标识符包含可能损程序集的奇怪字符（尽管此类安全漏洞很少见）。  
  
建议用反射发出生成代码，这通常有助于避免很多此类问题。  
  
编译代码时，请注意恶意程序是否可以某种方式修改它。 在编译器读取磁盘上的源代码或代码加载 .dll 文件之前，是否存在一小段时间窗口使恶意代码可在此期间修改此源代码？ 若如此，必须适当使用文件系统中的访问控制列表保护包含这些文件的目录。  
  
## <a name="see-also"></a>另请参阅

- [安全编码准则](secure-coding-guidelines.md)
- [ASP.NET Core 安全性](/aspnet/core/security/)
