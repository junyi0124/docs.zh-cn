---
title: CorTypeAttr 枚举
ms.date: 03/30/2017
api_name:
- CorTypeAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorTypeAttr
helpviewer_keywords:
- CorTypeAttr enumeration [.NET Framework metadata]
ms.assetid: 9bede0ec-5fdf-42a2-b5b7-bee64056acb6
topic_type:
- apiref
ms.openlocfilehash: 50ce4e5e6125eae493bb62032d5c6bd8887c1afb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95699087"
---
# <a name="cortypeattr-enumeration"></a>CorTypeAttr 枚举

包含一些值，用于指示类型元数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorTypeAttr {  
  
    tdVisibilityMask        =   0x00000007,  
    tdNotPublic             =   0x00000000,  
    tdPublic                =   0x00000001,  
    tdNestedPublic          =   0x00000002,  
    tdNestedPrivate         =   0x00000003,  
    tdNestedFamily          =   0x00000004,  
    tdNestedAssembly        =   0x00000005,  
    tdNestedFamANDAssem     =   0x00000006,  
    tdNestedFamORAssem      =   0x00000007,  
  
    tdLayoutMask            =   0x00000018,  
    tdAutoLayout            =   0x00000000,  
    tdSequentialLayout      =   0x00000008,  
    tdExplicitLayout        =   0x00000010,  
  
    tdClassSemanticsMask    =   0x00000020,  
    tdClass                 =   0x00000000,  
    tdInterface             =   0x00000020,  
  
    tdAbstract              =   0x00000080,  
    tdSealed                =   0x00000100,  
    tdSpecialName           =   0x00000400,  
  
    tdImport                =   0x00001000,  
    tdSerializable          =   0x00002000,  
    tdWindowsRuntime        =   0x00004000,  
  
    tdStringFormatMask      =   0x00030000,  
    tdAnsiClass             =   0x00000000,  
    tdUnicodeClass          =   0x00010000,  
    tdAutoClass             =   0x00020000,  
    tdCustomFormatClass     =   0x00030000,  
    tdCustomFormatMask      =   0x00C00000,  
  
    tdBeforeFieldInit       =   0x00100000,  
    tdForwarder             =   0x00200000,  
  
    tdReservedMask          =   0x00040800,  
    tdRTSpecialName         =   0x00000800,  
    tdHasSecurity           =   0x00040000,  
  
} CorTypeAttr;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`tdVisibilityMask`|用于类型可见性信息。|  
|`tdNotPublic`|指定该类型不在公共范围内。|  
|`tdPublic`|指定该类型在公共范围内。|  
|`tdNestedPublic`|指定该类型是用公共可见性嵌套的。|  
|`tdNestedPrivate`|指定该类型是用私有可见性嵌套的。|  
|`tdNestedFamily`|指定该类型嵌套了族可见性。|  
|`tdNestedAssembly`|指定该类型是用程序集可见性嵌套的。|  
|`tdNestedFamANDAssem`|指定该类型嵌套了族和程序集可见性。|  
|`tdNestedFamORAssem`|指定该类型是用族或程序集可见性嵌套的。|  
|`tdLayoutMask`|获取类型的布局信息。|  
|`tdAutoLayout`|指定此类型的字段自动布局。|  
|`tdSequentialLayout`|指定此类型的字段按顺序排列。|  
|`tdExplicitLayout`|指定显式提供字段布局。|  
|`tdClassSemanticsMask`|获取有关类型的语义信息。|  
|`tdClass`|指定该类型为一个类。|  
|`tdInterface`|指定该类型为一个接口。|  
|`tdAbstract`|指定该类型为抽象类型。|  
|`tdSealed`|指定无法扩展类型。|  
|`tdSpecialName`|指定类名称是特殊名称。 其名称描述了如何操作。|  
|`tdImport`|指定导入该类型。|  
|`tdSerializable`|指定该类型是可序列化的。|  
|`tdWindowsRuntime`|将此类型指定为 Windows 运行时类型。|  
|`tdStringFormatMask`|获取有关如何对字符串进行编码和格式化的信息。|  
|`tdAnsiClass`|指定此类型将 LPTSTR 解释为 ANSI。|  
|`tdUnicodeClass`|指定此类型将 LPTSTR 解释为 Unicode。|  
|`tdAutoClass`|指定此类型自动解释 LPTSTR。|  
|`tdCustomFormatClass`|指定该类型具有由指定的非标准编码 `CustomFormatMask` 。|  
|`tdCustomFormatMask`|使用此掩码获取本机互操作的非标准编码信息。 不指定这两个位的值的含义。|  
|`tdBeforeFieldInit`|指定在第一次尝试访问静态字段之前必须先初始化类型。|  
|`tdForwarder`|指定导出类型和类型转发器。|  
|`tdReservedMask`|此标志和下面的标志由公共语言运行时在内部使用。|  
|`tdRTSpecialName`|指定公共语言运行时应检查名称编码。|  
|`tdHasSecurity`|指定该类型具有与之关联的安全性。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
