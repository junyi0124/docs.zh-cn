---
title: IMetaDataEmit::DefineMethod 方法
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineMethod
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineMethod
helpviewer_keywords:
- DefineMethod method [.NET Framework metadata]
- IMetaDataEmit::DefineMethod method [.NET Framework metadata]
ms.assetid: 3e2102c5-48b7-4c0e-b805-7e2b5e156e3d
topic_type:
- apiref
ms.openlocfilehash: a32a1eb850943b84a0d368f883e44fd4ccf32ee9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719575"
---
# <a name="imetadataemitdefinemethod-method"></a>IMetaDataEmit::DefineMethod 方法

使用指定的签名创建方法或全局函数的定义，并将标记返回到该方法定义。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DefineMethod (
    [in]  mdTypeDef         td,
    [in]  LPCWSTR           szName,
    [in]  DWORD             dwMethodFlags,
    [in]  PCCOR_SIGNATURE   pvSigBlob,
    [in]  ULONG             cbSigBlob,
    [in]  ULONG             ulCodeRVA,
    [in]  DWORD             dwImplFlags,
    [out] mdMethodDef      *pmd  
);  
```  
  
## <a name="parameters"></a>参数  

 `td`  
 中 `mdTypedef` 方法的父类或父接口的标记。 `td` `mdTokenNil` 如果正在定义全局函数，则设置为。  
  
 `szName`  
 中Unicode 中的成员名称。  
  
 `dwMethodFlags`  
 中 [CorMethodAttr](cormethodattr-enumeration.md) 枚举的一个值，该值指定方法或全局函数的特性。  
  
 `pvSigBlob`  
 中方法签名。 签名按提供的方式持久保存。 如果需要指定任何参数的其他信息，请使用 [IMetaDataEmit：： SetParamProps](imetadataemit-setparamprops-method.md) 方法。  
  
 `cbSigBlob`  
 中中的字节数 `pvSigBlob` 。  
  
 `ulCodeRVA`  
 中代码的地址。  
  
 `dwImplFlags`  
 中 [CorMethodImpl](cormethodimpl-enumeration.md) 枚举的一个值，该值指定方法的实现功能。  
  
 `pmd`  
 弄成员标记。  
  
## <a name="remarks"></a>注解  

 元数据 API 保证按调用方为给定的封闭类或接口发出方法（在参数中指定）来保持方法的顺序相同 `td` 。  
  
 下面给出了有关使用 `DefineMethod` 和特定参数设置的其他信息。  
  
## <a name="slots-in-the-v-table"></a>V-表中的槽  

 运行时使用方法定义设置 v 表槽。 如果需要跳过一个或多个槽，如使用 COM 接口布局保留奇偶校验，则会定义一个虚方法，用于在下表中占据槽;将设置 `dwMethodFlags` 为 `mdRTSpecialName` [CorMethodAttr](cormethodattr-enumeration.md) 枚举的值，并将该名称指定为：  
  
 _VtblGap\<*SequenceNumber*>\<\_*CountOfSlots*>
  
 其中， *SequenceNumber* 是方法的序列号， *CountOfSlots* 是要在 v 表中跳过的槽数。 如果省略 *CountOfSlots* ，则假定为1。 不能从托管或非托管代码中调用这些虚方法，任何从托管或非托管代码调用它们的尝试都将生成异常。 其唯一目的是在运行时为 COM 集成生成的 v 表中占用空间。  
  
## <a name="duplicate-methods"></a>重复方法  

 不应定义重复的方法。 也就是说，不应 `DefineMethod` 使用 `td` 、和参数中的重复值集调用 `wzName` `pvSig` 。  (这三个参数一起唯一定义方法. ) 。 不过，你可以使用一种重复的三重，为其中一个方法定义设置 `mdPrivateScope` 参数中的位 `dwMethodFlags` 。  (`mdPrivateScope` 位意味着编译器不会发出对此方法定义的引用。 )   
  
## <a name="method-implementation-information"></a>方法实现信息  

 在声明该方法时，有关方法实现的信息通常是未知的。 因此，在调用时，不需要在和参数中传递值 `ulCodeRVA` `dwImplFlags` `DefineMethod` 。 可在以后根据需要通过 [IMetaDataEmit：： SetMethodImplFlags](imetadataemit-setmethodimplflags-method.md) 或 [IMetaDataEmit：： SetRVA](imetadataemit-setrva-method.md)来提供这些值。  
  
 在某些情况下（例如平台调用 (PInvoke) 或 COM 互操作方案），将不提供方法体，因此 `ulCodeRVA` 应将其设置为零。 在这些情况下，不应将方法标记为抽象方法，因为运行时将找到实现。  
  
## <a name="defining-a-method-for-pinvoke"></a>为 PInvoke 定义方法  

 对于要通过 PInvoke 调用的每个非托管函数，必须定义一个表示目标非托管函数的托管方法。 若要定义托管方法，请使用，将 `DefineMethod` 某些参数设置为特定值，具体取决于使用 PInvoke 的方式：  
  
- True PInvoke-涉及调用驻留在非托管 DLL 中的外部非托管方法。  
  
- 本地 PInvoke-涉及对嵌入当前托管模块的本机非托管方法的调用。  
  
 下表给出了参数设置。  
  
|参数|True PInvoke 的值|本地 PInvoke 的值|  
|---------------|-----------------------------|------------------------------|  
|`dwMethodFlags`||Set `mdStatic` ; 清除 `mdSynchronized` 和 `mdAbstract` 。|  
|`pvSigBlob`|有效的公共语言运行时 (CLR) 方法签名，其参数是有效的托管类型。|包含有效托管类型参数的有效 CLR 方法签名。|  
|`ulCodeRVA`||0|  
|`dwImplFlags`|设置 `miCil` 和 `miManaged`。|设置 `miNative` 和 `miUnmanaged`。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 用作 MSCorEE.dll 中的资源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataEmit 接口](imetadataemit-interface.md)
- [IMetaDataEmit2 接口](imetadataemit2-interface.md)
