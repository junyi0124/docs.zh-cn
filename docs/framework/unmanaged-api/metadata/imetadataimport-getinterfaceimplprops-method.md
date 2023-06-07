---
title: IMetaDataImport::GetInterfaceImplProps 方法
ms.date: 02/25/2019
api_name:
- IMetaDataImport.GetInterfaceImplProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetInterfaceImplProps
helpviewer_keywords:
- IMetaDataImport::GetInterfaceImplProps method [.NET Framework metadata]
- GetInterfaceImpProps method [.NET Framework metadata]
ms.assetid: be3f5985-b1e4-4036-8602-c16e8508d4af
topic_type:
- apiref
ms.openlocfilehash: e81816ce2194c2c1862cb997ad2c6e5baf301231
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95703991"
---
# <a name="imetadataimportgetinterfaceimplprops-method"></a>IMetaDataImport::GetInterfaceImplProps 方法

获取一个指针，该指针指向实现指定方法的的元数据标记 <xref:System.Type> 和声明该方法的接口。
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetInterfaceImplProps (  
   [in]  mdInterfaceImpl        iiImpl,  
   [out] mdTypeDef              *pClass,  
   [out] mdToken                *ptkIface  
);  
```  
  
## <a name="parameters"></a>参数  

 `iiImpl`  
 中表示要为其返回类和接口标记的方法的元数据标记。  
  
 `pClass`  
 弄表示实现方法的类的元数据标记。  
  
 `ptkIface`  
 弄表示用于定义实现的方法的接口的元数据标记。  

## <a name="remarks"></a>注解

 可以 `iImpl` 通过调用 [EnumInterfaceImpls](imetadataimport-enuminterfaceimpls-method.md) 方法来获取的值。

 例如，假设某个类的 `mdTypeDef` 标记值为0x02000007，并且它实现了三个类型具有标记的接口：

- 0x02000003 (TypeDef) 
- 0x0100000A (TypeRef) 
- 0x0200001C (TypeDef) 

从概念上讲，此信息存储到接口实现表中，如下所示：

| 行号 | 类标记 | 接口令牌 |
|------------|-------------|-----------------|
| 4          |             |                 |
| 5          | 02000007    | 02000003        |
| 6          | 02000007    | 0100000A        |
| 7          |             |                 |
| 8          | 02000007    | 0200001C        |

请记住，该令牌是一个4字节的值：

- 下3个字节保存行号或 RID。
- 上部字节保留标记类型– 0x09 `mdtInterfaceImpl` 。

`GetInterfaceImplProps` 返回在参数中提供其标记的行中保存的信息 `iImpl` 。
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](imetadataimport-interface.md)
- [IMetaDataImport2 接口](imetadataimport2-interface.md)
