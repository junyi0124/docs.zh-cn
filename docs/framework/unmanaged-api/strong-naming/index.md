---
title: 强命名（非托管 API 参考）
ms.date: 03/30/2017
helpviewer_keywords:
- strong naming [.NET Framework], using the unmanaged API
- native API reference [.NET Framework], strong naming
- unmanaged API reference [.NET Framework], strong naming
ms.assetid: 428c68b6-a7b4-44be-b280-75905f46612c
ms.openlocfilehash: 7e431f3a41fadb7247f20d7ab9bb9120e827b0cd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732289"
---
# <a name="strong-naming-unmanaged-api-reference"></a>强命名（非托管 API 参考）

强命名 API 允许客户端管理程序集的强名称签名。  
  
 使用强名称对程序集进行签名将向包含程序集清单的文件添加公钥加密。 强名称签名帮助验证名称的唯一性，避免名称欺骗，并在解析引用时向调用方提供唯一标识。 但是，任何信任级别都不会与强名称关联。  
  
## <a name="in-this-section"></a>本节内容  
  
> [!NOTE]
> 从 .NET Framework 4 开始，所有这些函数都已弃用。 有关建议的替代方案，请参阅 [ICLRStrongName](../hosting/iclrstrongname-interface.md) 接口。  
  
 [GetHashFromAssemblyFile 函数](gethashfromassemblyfile-function.md)  
 使用指定的哈希算法获取指定程序集文件的哈希。 从 .NET Framework 4 开始已弃用。  
  
 [GetHashFromAssemblyFileW 函数](gethashfromassemblyfilew-function.md)  
 使用指定的哈希算法获取指定为 Unicode 字符串的程序集文件的哈希。 从 .NET Framework 4 开始已弃用。  
  
 [GetHashFromBlob 函数](gethashfromblob-function.md)  
 使用指定的哈希算法获取指定内存地址处的程序集的哈希。 从 .NET Framework 4 开始已弃用。  
  
 [GetHashFromFile 函数](gethashfromfile-function.md)  
 生成指定文件内容的哈希。  从 .NET Framework 4 开始已弃用。  
  
 [GetHashFromFileW 函数](gethashfromfilew-function.md)  
 生成由 Unicode 字符串指定的文件内容的哈希。 从 .NET Framework 4 开始已弃用。  
  
 [GetHashFromHandle 函数](gethashfromhandle-function.md)  
 使用指定的哈希算法，生成具有指定文件句柄的文件内容的哈希。  从 .NET Framework 4 开始已弃用。  
  
 [StrongNameCompareAssemblies 函数](strongnamecompareassemblies-function.md)  
 确定两个程序集是否仅是强名称签名不同。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameErrorInfo 函数](strongnameerrorinfo-function.md)  
 获取由其中一个强名称函数引发的最后一个错误代码。  
  
 [StrongNameFreeBuffer 函数](strongnamefreebuffer-function.md)  
 释放先前调用 [StrongNameGetPublicKey](strongnamegetpublickey-function.md)、[StrongNameTokenFromPublicKey](strongnametokenfrompublickey-function.md) 或 [StrongNameSignatureGeneration](strongnamesignaturegeneration-function.md) 强名称函数分配的内存。   从 .NET Framework 4 开始已弃用。  
  
 [StrongNameGetBlob 函数](strongnamegetblob-function.md)  
 使用指定地址处可执行文件的二进制表示形式填充指定的缓冲区。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameGetBlobFromImage 函数](strongnamegetblobfromimage-function.md)  
 获取指定内存地址处程序集映像的二进制表示形式。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameGetPublicKey 函数](strongnamegetpublickey-function.md)  
 从私钥/公钥对中获取公钥。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameHashSize 函数](strongnamehashsize-function.md)  
 使用指定的哈希算法获取哈希所需的缓冲区大小。  从 .NET Framework 4 开始已弃用。  
  
 [StrongNameKeyDelete 函数](strongnamekeydelete-function.md)  
 删除指定的密钥容器。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameKeyGen 函数](strongnamekeygen-function.md)  
 创建新的公钥/私钥对，以便强名称使用。  从 .NET Framework 4 开始已弃用。  
  
 [StrongNameKeyGenEx 函数](strongnamekeygenex-function.md)  
 生成具有指定密钥大小的新公钥/私钥对，以便强名称使用。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameKeyInstall 函数](strongnamekeyinstall-function.md)  
 将公钥/私钥对导入容器。  从 .NET Framework 4 开始已弃用。  
  
 [StrongNameSignatureGeneration 函数](strongnamesignaturegeneration-function.md)  
 为指定的程序集生成强名称签名。   从 .NET Framework 4 开始已弃用。  
  
 [StrongNameSignatureGenerationEx 函数](strongnamesignaturegenerationex-function.md)  
 根据指定标志为指定的程序集生成强名称签名。    从 .NET Framework 4 开始已弃用。  
  
 [StrongNameSignatureSize 函数](strongnamesignaturesize-function.md)  
 返回强名称签名的大小。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameSignatureVerification 函数](strongnamesignatureverification-function.md)  
 获取一个值，该值指示提供的路径中的程序集清单是否包含根据指定标志验证的强名称签名。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameSignatureVerificationEx 函数](strongnamesignatureverificationex-function.md)  
 获取一个值，该值指示提供的路径中的程序集清单是否包含强名称签名。  从 .NET Framework 4 开始已弃用。  
  
 [StrongNameSignatureVerificationFromImage 函数](strongnamesignatureverificationfromimage-function.md)  
 验证已映射到内存的程序集对关联的公钥是否有效。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameTokenFromAssembly 函数](strongnametokenfromassembly-function.md)  
 从指定的程序集文件创建强名称令牌。  从 .NET Framework 4 开始已弃用。  
  
 [StrongNameTokenFromAssemblyEx 函数](strongnametokenfromassemblyex-function.md)  
 从指定的程序集文件创建强名称令牌并返回公钥。 从 .NET Framework 4 开始已弃用。  
  
 [StrongNameTokenFromPublicKey 函数](strongnametokenfrompublickey-function.md)  
 获取表示公钥的令牌。 从 .NET Framework 4 开始已弃用。  
  
 [PublicKeyBlob 结构](publickeyblob-structure.md)  
 表示二进制格式的公钥/私钥对中的公钥。  
  
## <a name="see-also"></a>另请参阅

- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
- [非托管 API 参考](../index.md)
