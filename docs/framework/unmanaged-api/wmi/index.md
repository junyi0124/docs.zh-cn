---
title: WMI 和性能计数器（非托管 API 参考）
description: 总结用于 WMI 的 .NET Framework 非托管 API 和性能计数器信息。
ms.date: 11/06/2017
ms.openlocfilehash: f28cd25ee6c3511dc5ac8a6dd4076c81f43fe74a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "73127410"
---
# <a name="windows-management-instrumentation-wmi-and-performance-counters-unmanaged-api-reference"></a>Windows Management Instrumentation (WMI) 和性能计数器（非托管 API 参考）

.NET Framework WMI 和性能计数器非托管 API 由一组函数组成，这些函数包含对[本机 Windows Management Instrumentation API](/windows/desktop/WmiSdk/com-api-for-wmi) 的调用。 通过它，用户可开发管理和监视远程计算机系统的工具和库。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

API 包括以下函数：

| 函数 | 说明 |
|---------|---------|
| [BeginEnumeration 函数](beginenumeration.md) | 将枚举器重置到 WMI 对象属性枚举的起始处。 |
| [BeginMethodEnumeration 函数](beginmethodenumeration.md) |  开始枚举对象可用的方法。 |
| [BlessIWbemServices 函数](blessiwbemservices.md) | 指示用户凭据是否允许访问指定的 IWbemServices 类。 |
| [BlessIWbemServicesObject 函数](blessiwbemservicesobject.md) | 指示用户凭据是否允许访问指定的 IWbem 服务对象。 |
| [Clone 函数](clone.md) | 返回一个新对象，该对象是当前对象的完整克隆。 |
| [CloneEnumWbemClassObject 函数](cloneenumwbemclassobject.md) | 制作枚举器的逻辑副本，并保留其在枚举中的当前位置。 |
| [CompareTo 函数](compareto.md) | 将对象与另一个 Windows 管理对象进行比较。 |
| [ConnectServerWmi 函数](connectserverwmi.md) | 通过 DCOM 创建到指定计算机上的 WMI 命名空间的连接。 |
| [CreateClassEnumWmi 函数](createclassenumwmi.md) | 返回满足指定选择条件的所有类的枚举器。 |
| [CreateInstanceEnumWmi 函数](createinstanceenumwmi.md) | 返回枚举器，该枚举器返回符合指定选择条件的指定类的实例。 |
| [Delete 函数](delete.md) | 从类定义及其所有限定符中删除指定属性。 |
| [DeleteMethod 函数](deletemethod.md) | 从 CIM 类定义中删除指定方法。 |
| [EndEnumeration 函数](endenumeration.md) | 终止枚举序列。 |
| [EndMethodEnumeration 函数](endmethodenumeration.md) | 终止通过调用 [BeginMethodEnumeration 函数](beginmethodenumeration.md)开始的枚举序列。 |
| [ExecNotificationQueryWmi 函数](execnotificationquerywmi.md) | 执行查询以接收事件。 |
| [ExecQueryWmi 函数](execquerywmi.md) | 执行查询以检索对象。 |
| [FormatFromRawValue 函数](formatfromrawvalue.md) | 如果格式转换是基于时间的，则将一个或两个原始性能数据值转换为指定格式。 |
| [Get 函数](get.md) | 检索指定的属性值（如果存在）。 |
| [GetCurrentApartmentType 函数](getcurrentapartmenttype.md) | 检索调用方执行操作所在的单元类型。 |
| [GetDemultiplexedStub 函数](getdemultiplexedstub.md) | 创建对象转发器接收器，帮助客户端从 Windows Management 接收异步调用。 |
| [GetErrorInfo 函数](geterrorinfo.md) | 从上一个函数调用中检索错误信息。 |
| [GetMethod 函数](getmethod.md) | 检索有关指定方法的信息。 |
| [GetMethodOrigin 函数](getmethodorigin.md) | 确定声明方法的类。 |
| [GetMethodQualifierSet 函数](getmethodqualifierset.md) | 检索特定方法的限定符集。 |
| [GetNames 函数](getnames.md) | 检索对象属性的子集或所有名称。 |
| [GetObjectText 函数](getobjecttext.md) | 返回 MOF 语法中对象的文本呈现。 |
| [GetPropertyHandle 函数](getpropertyhandle.md) | 返回标识属性的唯一句柄。 |
| [GetPropertyOrigin 函数](getpropertyorigin.md) | 确定声明方法的类。 |
| [GetPropertyQualifierSet 函数](getpropertyqualifierset.md) | 检索特定属性的限定符集。  |
| [GetQualifierSet 函数](getqualifierset.md) | 检索类实例或类定义的限定符集。 |
| [InheritsFrom 函数](inheritsfrom.md) | 确定当前类或实例是否派生自指定的父类。 |
| [Initialize 函数](initialize.md) | 执行 WMI 初始化。 |
| [Next 函数](next.md) | 检索枚举中的下一个属性。 |
| [NextMethod 函数](nextmethod.md) | 检索枚举中的下一个方法。 |
| [Put 函数](put.md) | 将命名属性设置为新值。 |
| [PutClassWmi 函数](putclasswmi.md) | 创建新类或更新现有类。 |
| [PutInstanceWmi 函数](putinstancewmi.md) | 创建或更新现有类的实例。 将该实例写入 WMI 存储库。 |
| [PutMethod 函数](putmethod.md) | 创建方法。 |
| [QualifierSet_BeginEnumeration 函数](qualifierset-beginenumeration.md) | 将对象限定符的枚举器重置到枚举的起始处。 |
| [QualifierSet_Delete 函数](qualifierset-delete.md) | 按名称删除指定限定符。  |
| [QualifierSet_EndEnumeration 函数](qualifierset-endenumeration.md) | 终止以调用 `QualifierSet_BeginEnumeration` 函数开始的枚举。 |
| [QualifierSet_Get 函数](qualifierset-get.md) | 获取指定的命名限定符。  |
| [QualifierSet_GetNames 函数](qualifierset-getnames.md) | 检索当前对象或属性中可用的所有限定符或指定限定符的名称。 |
| [QualifierSet_Next 函数](qualifierset-next.md) | 检索枚举中的下一个限定符，该枚举以调用 [QualifierSet_BeginEnumeration ](qualifierset-beginenumeration.md) 函数开始。 |
| [QualifierSet_Put 函数](qualifierset-put.md) | 写入命名限定符和值。 |
| [ResetSecurity 函数](resetsecurity.md) | 将提供的模拟令牌分配给当前线程。 |
| [SetSecurity 函数](setsecurity.md) | 检索与当前线程关联的模拟令牌。 |
| [SpawnDerivedClass 函数](spawnderivedclass.md) | 从指定对象创建新派生的类对象。 |
| [SpawnInstance 函数](spawninstance.md) | 创建类的新实例。 |
| [VerifyClient 函数](verifyclientkey.md) | 确保客户端密钥具有正确的安全性。 |
| [WritePropertyValue 函数](writepropertyvalue.md) | 将指定数量的字节写入由属性句柄标识的属性。 |

## <a name="see-also"></a>另请参阅

- [非托管 API 参考](../index.md)
