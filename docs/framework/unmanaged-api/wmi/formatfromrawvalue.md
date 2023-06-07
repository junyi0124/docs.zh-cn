---
title: 'FormatFromRawValue 函数 (非托管 API 参考) '
description: FormatFromRawValue 函数将原始性能数据转换为指定格式。
ms.date: 11/21/2017
api_name:
- FormatFromRawValue
api_location:
- PerfCounter.dll
api_type:
- DLLExport
f1_keywords:
- FormatFromRawValue
helpviewer_keywords:
- FormatFromRawValue function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: e678aca5baf82c07ec9fc5c85cef22630af5ab0a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672326"
---
# <a name="formatfromrawvalue-function"></a>FormatFromRawValue 函数

如果格式转换是基于时间的，则将一个或两个原始性能数据值转换为指定格式。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
int FormatFromRawValue (
   [in] uint                    dwCounterType,
   [in] uint                    dwFormat,
   [in] long*                   pTimeBase,
   [in] PDH_RAW_COUNTER*        pRawValue1,
   [in] PDH_RAW_COUNTER*        pRawValue2,
   [out] PDH_FMT_COUNTERVALUE*  pFmtValue
);
```

## <a name="parameters"></a>参数

`dwCounterType`\
中计数器类型。 有关计数器类型的列表，请参阅 [WMI 性能计数器类型](/windows/desktop/WmiSdk/wmi-performance-counter-types)。 `dwCounterType` 可以是除和以外的任何计数器类型 `PERF_LARGE_RAW_FRACTION` `PERF_LARGE_RAW_BASE` 。

`dwFormat`\
中原始性能数据的转换格式。 可以为下列值之一：

|返回的常量  |Value  |说明 |
|---------|---------|---------|
| `PDH_FMT_DOUBLE` |0x00000200 | 将计算的值作为双精度浮点值返回。 |
| `PDH_FMT_LARGE` | 0x00000400 | 将计算的值作为64位整数返回。 |
| `PDH_FMT_LONG` | 0x00000100 | 将计算的值作为32位整数返回。 |

先前的值之一可以运算以下缩放标志之一：

|返回的常量  |Value  |说明 |
|---------|---------|---------|
| `PDH_FMT_NOSCALE` | 0x00001000 | 不要应用计数器的缩放系数。 |
| `PDH_FMT_1000` | 0x00002000 | 将最终值乘以1000。 |

`pTimeBase`\
中如果格式转换需要，则为指向时间基准的指针。 如果格式转换不需要时间基准信息，则忽略此参数的值。

`pRawValue1`\
中指向 [`PDH_RAW_COUNTER`](/windows/win32/api/pdh/ns-pdh-pdh_raw_counter) 结构的指针，该结构表示原始性能值。

`pRawValue2`\
中指向 [`PDH_RAW_COUNTER`](/windows/win32/api/pdh/ns-pdh-pdh_raw_counter) 结构的指针，该结构表示第二个原始性能值。 如果不需要第二个原始性能值，则此参数应为 `null` 。

`pFmtValue`\
弄指向 [`PDH_FMT_COUNTERVALUE`](/windows/win32/api/pdh/ns-pdh-pdh_fmt_countervalue) 结构的指针，该结构接收格式化的性能值。

## <a name="return-value"></a>返回值

此函数返回以下值：

|返回的常量  |Value  |说明  |
|---------|---------|---------|
| `ERROR_SUCCESS` | 0 | 函数调用成功。 |
| `PDH_INVALID_ARGUMENT` | 0xC0000BBD | 必需的参数缺失或不正确。 |
| `PDH_INVALID_HANDLE` | 0xC0000BBC | 句柄不是有效的 PDH 对象。 |

## <a name="remarks"></a>注解

此函数包装对 [FormatFromRawValue](/previous-versions/ms231047(v=vs.85)) 函数的调用。

## <a name="requirements"></a>要求

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。

 **库：** PerfCounter.dll

 **.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
