---
title: ISymUnmanagedENCUpdate 接口
ms.date: 03/30/2017
api_name:
- ISymUnmanagedENCUpdate
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedENCUpdate
helpviewer_keywords:
- ISymUnmanagedENCUpdate interface [.NET Framework debugging]
ms.assetid: 63a9ef45-01a6-46da-b958-5c6dc2dc232c
topic_type:
- apiref
ms.openlocfilehash: 5e3c2ecd1bdd5c1181223c7500eb7473e20fa5d4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731340"
---
# <a name="isymunmanagedencupdate-interface"></a>ISymUnmanagedENCUpdate 接口

为 "编辑并继续" 功能提供函数。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetLocalVariableCount 方法](isymunmanagedencupdate-getlocalvariablecount-method.md)|获取局部变量的数目。|  
|[GetLocalVariables 方法](isymunmanagedencupdate-getlocalvariables-method.md)|获取局部变量。|  
|[InitializeForEnc 方法](isymunmanagedencupdate-initializeforenc-method.md)|允许在首次调用 [ISymUnmanagedENCUpdate：： UpdateSymbolStore2](isymunmanagedencupdate-updatesymbolstore2-method.md) 方法之前计算方法边界。|  
|[UpdateMethodLines 方法](isymunmanagedencupdate-updatemethodlines-method.md)|允许更新尚未重新编译但行已单独移动的方法的行信息。 允许每个语句的增量。|  
|[UpdateSymbolStore2 方法](isymunmanagedencupdate-updatesymbolstore2-method.md)|如果行信息满足要求，则允许编译器省略未从程序数据库 (PDB) 流中修改的函数。 可以通过旧的 PDB 行信息和函数中所有行的一个增量来确定正确的行信息。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym，CorSym  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区接口](diagnostics-symbol-store-interfaces.md)
