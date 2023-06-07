---
title: <useLegacyJit> 元素
ms.date: 04/26/2017
ms.assetid: c2cf97f0-9262-4f1f-a754-5568b51110ad
ms.openlocfilehash: a126b8c0050a8d1fd96a3d090f9b018a9faa07a7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "73968856"
---
# <a name="uselegacyjit-element"></a>\<useLegacyJit> 元素

确定公共语言运行时是否使用实时编译的旧版 64 位 JIT 编译器。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<useLegacyJit>**  
  
## <a name="syntax"></a>语法  
  
```xml
<useLegacyJit enabled=0|1 />
```

元素名称 `useLegacyJit` 区分大小写。
  
## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
| 属性 | 说明                                                                                   |  
| --------- | --------------------------------------------------------------------------------------------- |  
| `enabled` | 必需的特性。<br><br>指定运行时是否使用旧版64位 JIT 编译器。 |  
  
### <a name="enabled-attribute"></a>enabled 属性  
  
| 值 | 说明                                                                                                         |  
| ----- | ------------------------------------------------------------------------------------------------------------------- |  
| 0     | 公共语言运行时使用 .NET Framework 4.6 及更高版本中包含的新64位 JIT 编译器。 |  
| 1     | 公共语言运行时使用较旧的64位 JIT 编译器。                                                     |  
  
### <a name="child-elements"></a>子元素

无
  
### <a name="parent-elements"></a>父元素  
  
| 元素         | 描述                                                                                                       |  
| --------------- | ----------------------------------------------------------------------------------------------------------------- |  
| `configuration` | 公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。 |  
| `runtime`       | 包含有关运行时初始化选项的信息。                                                        |  
  
## <a name="remarks"></a>注解  

从 .NET Framework 4.6 开始，默认情况下，公共语言运行时为实时（JIT）编译使用新的64位编译器。 在某些情况下，这可能会导致应用程序代码的行为与以前版本的64位 JIT 编译器进行 JIT 编译的行为不同。 通过将 `enabled` 元素的属性设置 `<useLegacyJit>` 为 `1` ，可以禁用新的64位 jit 编译器，而使用旧的64位 jit 编译器编译应用程序。  
  
> [!NOTE]
> `<useLegacyJit>`元素仅影响64位 JIT 编译。 与32位 JIT 编译器的编译不受影响。  
  
您可以通过两种其他方法启用旧版64位 JIT 编译器，而不是使用配置文件设置：  
  
- 设置环境变量

  将 `COMPLUS_useLegacyJit` 环境变量设置为 `0` （使用新的64位 jit 编译器）或 `1` （使用较旧的64位 jit 编译器）：
  
  ```env  
  COMPLUS_useLegacyJit=0|1  
  ```  
  
  环境变量具有*全局作用域*，这意味着它会影响计算机上运行的所有应用程序。 如果已设置，则它可以被应用程序配置文件设置重写。 环境变量名称不区分大小写。
  
- 添加注册表项

  可以通过将 `REG_DWORD` 值添加到 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` 注册表中的或键来启用旧的64位 JIT 编译器 `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` 。 此值的名称为 `useLegacyJit` 。 如果该值为0，则使用新编译器。 如果值为1，则启用旧版64位 JIT 编译器。 注册表值名称不区分大小写。
  
  将该值添加到 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` 密钥会影响计算机上运行的所有应用。 将该值添加到 `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` 密钥会影响当前用户运行的所有应用。 如果使用多个用户帐户配置了计算机，则只有当前用户运行的应用才会受到影响，除非还将该值添加到其他用户的注册表项。 如果将 `<useLegacyJit>` 元素添加到配置文件中，将重写注册表设置（如果存在）。  
  
## <a name="example"></a>示例  

下面的配置文件使用新的64位 JIT 编译器禁用编译，而改用旧的64位 JIT 编译器。  
  
```xml  
<?xml version ="1.0"?>  
<configuration>  
  <runtime>  
    <useLegacyJit enabled="1" />  
  </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>另请参阅

- [\<runtime>Element](runtime-element.md)
- [\<configuration>Element](../configuration-element.md)
- [缓解：新的 64 位 JIT 编译器](../../../migration-guide/mitigation-new-64-bit-jit-compiler.md)
