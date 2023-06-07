---
title: AssemblyComparisonResult 枚举
ms.date: 03/30/2017
api_name:
- AssemblyComparisonResult
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- AssemblyComparisonResult
helpviewer_keywords:
- AssemblyComparisonResult enumeration [.NET Framework fusion]
ms.assetid: bd042f89-10b1-40ca-946e-46da082f5263
topic_type:
- apiref
ms.openlocfilehash: cde25a9507006c89ef6490c13ae82033f04c2931
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731028"
---
# <a name="assemblycomparisonresult-enumeration"></a>AssemblyComparisonResult 枚举

指示由 [CompareAssemblyIdentity](compareassemblyidentity-function.md) 函数确定的两个程序集标识的等效性。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum _tagAssemblyComparisonResult {  
    ACR_Unknown,
    ACR_EquivalentFullMatch,  
    ACR_EquivalentWeakNamed,  
    ACR_EquivalentFXUnified,  
    ACR_EquivalentUnified,
    ACR_NonEquivalentVersion,  
    ACR_NonEquivalent,
    ACR_EquivalentPartialMatch,  
    ACR_EquivalentPartialWeakNamed,
    ACR_EquivalentPartialUnified,  
    ACR_EquivalentPartialFXUnified,  
    ACR_NonEquivalentPartialVersion
} AssemblyComparisonResult;  
```  
  
## <a name="members"></a>成员  
  
|成员名称|说明|  
|-----------------|-----------------|  
|`ACR_EquivalentFullMatch`|指示比较匹配项中的所有程序集字段。|  
|`ACR_EquivalentFXUnified`|指示基于公共语言运行时版本，将程序集视为等效 (CLR) .NET Framework 版本2.0 中的程序集版本号。|  
|`ACR_EquivalentPartialFXUnified`|指示程序集的部分匹配，这些程序集基于 .NET Framework 2.0 中的程序集版本号的 CLR 统一。|  
|`ACR_EquivalentPartialMatch`|指示程序集的部分匹配项。|  
|`ACR_EquivalentPartialUnified`|指示程序集的部分匹配，这些程序集基于旧的版本号。|  
|`ACR_EquivalentPartialWeakNamed`|指示只与命名的程序集进行部分匹配。|  
|`ACR_EquivalentUnified`|指示程序集根据 .NET Framework 旧版本中的 CLR 统一版本号被视为等效的。|  
|`ACR_EquivalentWeakNamed`|指示两个只命名的程序集的匹配项，这些程序集的版本号被忽略。|  
|`ACR_NonEquivalent`|指示两个程序集之间未发生匹配。|  
|`ACR_NonEquivalentPartialVersion`|指示除了仅部分匹配的版本号外，这两个程序集匹配。|  
|`ACR_NonEquivalentVersion`|指示两个程序集匹配，但其版本号不匹配。|  
|`ACR_Unknown`|指示非等效的原因未知。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [CompareAssemblyIdentity 函数](compareassemblyidentity-function.md)
- [合成枚举](fusion-enumerations.md)
