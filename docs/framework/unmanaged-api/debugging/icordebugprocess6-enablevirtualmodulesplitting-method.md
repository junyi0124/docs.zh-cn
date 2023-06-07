---
title: ICorDebugProcess6::EnableVirtualModuleSplitting 方法
ms.date: 03/30/2017
ms.assetid: e7733bd3-68da-47f9-82ef-477db5f2e32d
ms.openlocfilehash: 56795c6879d95253383c26c92e060f252a018914
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95690195"
---
# <a name="icordebugprocess6enablevirtualmodulesplitting-method"></a>ICorDebugProcess6::EnableVirtualModuleSplitting 方法

启用或禁用虚拟模块拆分。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnableVirtualModuleSplitting(  
   BOOL enableSplitting  
);  
```  
  
## <a name="parameters"></a>参数  

 `enableSplitting`  
 `true` 来启用虚拟模块拆分；`false` 来将其禁用。  
  
## <a name="remarks"></a>注解  

 虚拟模块拆分会导致 [ICorDebug](icordebug-interface.md) 识别在生成过程中合并在一起的模块，并将它们显示为一组单独的模块，而不是单个大模块。 执行此操作将更改以下所述的各种 [ICorDebug](icordebug-interface.md) 方法的行为。  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
 可随时调用方法并更改 `enableSplitting` 值。 它不会在 [ICorDebug](icordebug-interface.md) 对象中产生任何有状态的功能更改，而不是在调用 [虚拟模块拆分和非托管调试 api](#APIs) 部分中列出的方法的行为。 调用那些方法时，使用虚拟模块确实会导致性能显著下降。 此外，可能还需要对虚拟化的元数据进行大量的内存中缓存，才能正确实现 [IMetaDataImport](../metadata/imetadataimport-interface.md) api，并且即使在关闭虚拟模块拆分后，这些缓存也会保留。  
  
## <a name="terminology"></a>术语  

 描述虚拟模块拆分时会用到下列术语：  
  
 容器模块，即容器  
 聚合模块。  
  
 子模块，即虚拟模块  
 在容器中发现的各模块。  
  
 普通模块  
 未在生成过程中合并的模块。 它们既不是容器模块也不是子模块。  
  
 但 ICor调试模块接口对象会表示容器模块和子模块。 但是，在每种情况下，接口的行为稍有不同，如 \<x-ref to section> 部分所述。  
  
## <a name="modules-and-assemblies"></a>模块和程序集  

 在程序集合并的情况下，多模块程序集不受支持，因此模块和程序集之间存在一对一的关系。 每个 ICor调试模块对象（不论其代表容器模块还是子模块）都拥有相应的 ICor调试程序集对象。 [ICorDebugModule：： GetAssembly](icordebugmodule-getassembly-method.md)方法从模块转换为程序集。 若要以其他方向映射， [ICorDebugAssembly：： EnumerateModules](icordebugassembly-enumeratemodules-method.md) 方法只枚举1个模块。 因为在此情况下的程序集和模块形成了一个紧密耦合对，术语程序集和模块变得在很大程度上可以互换。  
  
## <a name="behavioral-differences"></a>行为差异  

 容器模块包含以下行为和特征：  
  
- 所有组成子模块的元数据合并在一起。  
  
- 类型名称可能改变。  
  
- [ICorDebugModule：： GetName](icordebugmodule-getname-method.md)方法返回磁盘上的模块的路径。  
  
- [ICorDebugModule：： GetSize](icordebugmodule-getsize-method.md)方法返回该图像的大小。  
  
- ICorDebugAssembly3.EnumerateContainedAssemblies 方法列举子模块。  
  
- ICorDebugAssembly3.GetContainerAssembly 方法返回 `S_FALSE`。  
  
 子模块包含以下行为和特征：  
  
- 他们具有一套减少的元数据，这些元数据只对应合并的原始程序集。  
  
- 元数据名未改变。  
  
- 元数据令牌经生成过程合并之前，不大可能与原始程序集中的令牌匹配。  
  
- [ICorDebugModule：： GetName](icordebugmodule-getname-method.md)方法返回程序集名称，而不是文件路径。  
  
- [ICorDebugModule：： GetSize](icordebugmodule-getsize-method.md)方法返回未合并的原始图像大小。  
  
- ICorDebugModule3.EnumerateContainedAssemblies 方法返回 `S_FALSE`。  
  
- ICorDebugAssembly3.GetContainerAssembly 方法返回包含模块。  
  
## <a name="interfaces-retrieved-from-modules"></a>从模块恢复的接口  

 模块可恢复或创建多个接口。 其中包括：  
  
- ICorDebugClass 对象，由 [ICorDebugModule：： GetClassFromToken](icordebugmodule-getclassfromtoken-method.md) 方法返回。  
  
- ICorDebugAssembly 对象，由 [ICorDebugModule：： GetAssembly](icordebugmodule-getassembly-method.md) 方法返回。  
  
 这些对象始终由 [ICorDebug](icordebug-interface.md)缓存，它们具有相同的指针标识，无论是从容器模块还是子模块中创建或查询它们。 子模块提供了这些缓存对象的筛选视图，而非包含各自副本的单独缓存。  
  
<a name="APIs"></a>

## <a name="virtual-module-splitting-and-the-unmanaged-debugging-apis"></a>虚拟模块拆分和未托管调试 API  

 下表显示了虚拟模块拆分如何影响未托管调试 API 中的其他方法的行为。  
  
|方法|`enableSplitting` = `true`|`enableSplitting` = `false`|  
|------------|---------------------------------|----------------------------------|  
|[ICorDebugFunction::GetModule](icordebugfunction-getmodule-method.md)|返回最初定义该功能的子模块|返回该功能合并到的容器模块|  
|[ICorDebugClass::GetModule](icordebugclass-getmodule-method.md)|返回最初定义该类的子模块。|返回该类合并到的容器模块。|  
|ICorDebugModuleDebugEvent::GetModule|返回加载的容器模块。 不管此设置如何，子模块不会收到加载事件。|返回加载的容器模块。|  
|[ICorDebugAppDomain::EnumerateAssemblies](icordebugappdomain-enumerateassemblies-method.md)|返回一组子程序集和普通程序集；不包含容器程序集。 **注意：**  如果任何容器程序集缺少符号，则将不会枚举其任何子程序集。 如果任一普通程序集缺少符号，则其可能被枚举或不枚举。|返回一组子程序集和普通程序集；不包含子程序集。 **注意：**  如果任何常规程序集缺少符号，则可以或不会枚举它。|  
|) 仅当引用 IL 代码时， [ICorDebugCode：： GetCode](icordebugcode-getcode-method.md) (|返回可能在预合并程序集图像中有效的 IL。 具体来说，当包含 IL 的虚拟模块未定义参考类型时，任一内联元数据令牌都可以作为 TypeRef 或 MemberRef 令牌。 可以在对应的 virtual ICorDebugModule 对象的 [IMetaDataImport](../metadata/imetadataimport-interface.md) 对象中查找这些 TypeRef 或 MemberRef 标记。|返回预合并程序集图像中的 IL。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [“ICor调试进程6”接口](icordebugprocess6-interface.md)
- [调试接口](debugging-interfaces.md)
