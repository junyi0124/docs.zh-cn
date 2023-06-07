---
title: METAHOST_POLICY_FLAGS 枚举
ms.date: 03/30/2017
api_name:
- METAHOST_POLICY_FLAGS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- METAHOST_POLICY_FLAGS
helpviewer_keywords:
- METAHOST_POLICY_FLAGS enumeration [.NET Framework hosting]
ms.assetid: 3bb4b526-0118-42e2-ba59-c95648528ce9
topic_type:
- apiref
ms.openlocfilehash: 16fb112cf5b209091001872567c95bb0b3a8ab61
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729988"
---
# <a name="metahost_policy_flags-enumeration"></a>METAHOST_POLICY_FLAGS 枚举

提供大多数运行时主机通用的绑定策略。 此枚举由 [ICLRMetaHostPolicy：： GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md) 方法使用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    METAHOST_POLICY_HIGHCOMPAT              = 0x00,  
    METAHOST_POLICY_APPLY_UPGRADE_POLICY    = 0x08,  
    METAHOST_POLICY_EMULATE_EXE_LAUNCH      = 0x10,  
    METAHOST_POLICY_SHOW_ERROR_DIALOG       = 0x20,  
    METAHOST_POLICY_USE_PROCESS_IMAGE_PATH  = 0x40,  
    METAHOST_POLICY_ENSURE_SKU_SUPPORTED    = 0x80,  
    METAHOST_POLICY_IGNORE_ERROR_MODE       = 0x1000  
  
} METAHOST_POLICY_FLAGS;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`METAHOST_POLICY_HIGHCOMPAT`|定义高兼容性策略，该策略不考虑加载到当前进程 (CLR) 的任何公共语言运行时。 相反，它仅考虑已安装的 Clr 和组件的首选项，派生自程序集文件本身、声明的生成版本或配置文件。|  
|`METAHOST_POLICY_APPLY_UPGRADE_POLICY`|当找不到匹配项的内容时，将升级策略应用于版本绑定结果，具体取决于 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft的内容 \\ 。NETFramework\Policy\Upgrades. 这与 [RUNTIME_INFO_UPGRADE_VERSION](runtime-info-flags-enumeration.md)的效果相同。|  
|`METAHOST_POLICY_EMULATE_EXE_LAUNCH`|返回绑定结果，就好像在新进程中启动了提供给调用的图像一样。 目前，会 `GetRequestedRuntime` 忽略一组可加载的运行时，并将其与已安装的运行时集绑定在一起。 此标志允许主机在启动时确定 EXE 将绑定到的运行时。|  
|`METAHOST_POLICY_SHOW_ERROR_DIALOG`|如果 `GetRequestedRuntime` 找不到与输入参数兼容的运行时，则会显示错误对话框。 从 .NET Framework 4.5 开始，此错误对话框可以采用 Windows 功能对话框的形式，该对话框会询问用户是否要启用相应的功能。|  
|`METAHOST_POLICY_USE_PROCESS_IMAGE_PATH`|`GetRequestedRuntime` 使用进程图像 (和任何相应的配置文件) 作为绑定过程的附加输入。 默认情况下， `GetRequestedRuntime` 不会回退到进程映像路径 (通常是在确定要绑定到的运行时) 用于启动进程的 EXE。|  
|`METAHOST_POLICY_ENSURE_SKU_SUPPORTED`|`GetRequestedRuntime` 如果配置文件中没有可用的信息，则必须检查是否安装了相应的 SKU。 这允许不具有配置文件的应用程序在 .NET Framework 的默认安装的较小 Sku 上正常失败。 默认情况下，不 `GetRequestedRuntime` 会检查是否安装了相应的 sku，除非在配置文件元素中指定了 sku 属性 `<supportedRuntime />` 。|  
|`METAHOST_POLICY_IGNORE_ERROR_MODE`|`GetRequestedRuntime` 应忽略 SEM_FAILCRITICALERRORS (通过调用 [SetErrorMode](/windows/win32/api/errhandlingapi/nf-errhandlingapi-seterrormode) 函数) 设置，并显示错误对话框。 默认情况下，SEM_FAILCRITICALERRORS 禁止显示错误对话框。 它可能已被其他进程继承，并且在你的方案中可能不需要此错误。|  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Metahost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载枚举](hosting-enumerations.md)
- [GetRequestedRuntime 方法](iclrmetahostpolicy-getrequestedruntime-method.md)
