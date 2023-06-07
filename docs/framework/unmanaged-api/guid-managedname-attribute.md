---
title: GUID_ManagedName 特性
ms.date: 03/30/2017
api_name:
- GUID_ManagedName
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- GUID_ManagedName
helpviewer_keywords:
- GUID_ManagedName attribute
ms.assetid: 11e18095-e444-47bc-aff6-b887ac5dc01e
topic_type:
- apiref
ms.openlocfilehash: 0127b6894f1095521f1b24fc8c0424dc7db824b3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721044"
---
# <a name="guid_managedname-attribute"></a>GUID_ManagedName 特性

定义一个自定义接口特性，该特性指定组件对象模型 (COM) 库的托管命名空间名称。  
  
## <a name="syntax"></a>语法  
  
```idl
[  
   custom(GUID_ManagedName, value)  
]  
```  
  
## <a name="parameters"></a>参数  

 `value`  
 库的托管命名空间名称。  
  
## <a name="definition"></a>定义  

 `GUID_ManagedName` 在 Cor 中定义，如下所示：  
  
```cpp
// {0F21F359-AB84-41e8-9A78-36D110E6D2F9}  
EXTERN_GUID(GUID_ManagedName, 0xf21f359, 0xab84, 0x41e8, 0x9a, 0x78, 0x36, 0xd1, 0x10, 0xe6, 0xd2, 0xf9);  
```  
  
## <a name="remarks"></a>注解  

 自定义接口特性为类型库中的对象定义元数据。  
  
 使用 <xref:System.Runtime.InteropServices.ComTypes.ITypeInfo2.GetCustData%2A?displayProperty=nameWithType> 或 <xref:System.Runtime.InteropServices.ComTypes.ITypeLib2.GetCustData%2A?displayProperty=nameWithType> 从属性中检索托管名称。  
  
 有关详细信息，请参阅 Visual C++ 参考文档中的 [接口特性](/cpp/windows/attributes/interface-attributes) 。  
  
## <a name="example"></a>示例  

 下面的示例演示了一个使用属性的库定义 `GUID_ManagedName` 。  
  
```idl
[  
   ...  
   custom(GUID_ManagedName, Microsoft.VisualStudio.CommandBars.dll")  
]  
library Microsoft_VisualStudio_CommandBars  
{  
   ...  
}  
```  
  
## <a name="requirements"></a>要求  

 **标头：** Cor
