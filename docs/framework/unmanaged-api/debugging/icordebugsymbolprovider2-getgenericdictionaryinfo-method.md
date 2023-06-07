---
title: ICorDebugSymbolProvider2::GetGenericDictionaryInfo 方法
ms.date: 03/30/2017
ms.assetid: ba28fe4e-5491-4670-bff7-7fde572d7593
ms.openlocfilehash: a6c32b72c5924399aeb13d56ddf9242fe7990f35
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83379326"
---
# <a name="icordebugsymbolprovider2getgenericdictionaryinfo-method"></a>ICorDebugSymbolProvider2::GetGenericDictionaryInfo 方法

检索泛型字典映射。

## <a name="syntax"></a>语法

```cpp
HRESULT GetGenericDictionaryInfo(
   [out] ICorDebugMemoryBuffer** ppMemoryBuffer
);
```

## <a name="parameters"></a>参数

`ppMemoryBuffer`\
弄指向包含泛型字典映射的[ICorDebugMemoryBuffer](icordebugmemorybuffer-interface.md)对象地址的指针。 有关详细信息，请参阅“备注”部分。

## <a name="remarks"></a>备注

> [!NOTE]
> 此方法仅适用于 .NET Native。

该映射由两个顶级部分组成：

- 一个[目录](#Directory)，其中包含此映射中包含的所有字典的相对虚拟地址（RVA）。

- 一个字节对齐的[堆](#Heap)，其中包含对象实例化信息。 在最后一个目录输入后立即开始。

<a name="Directory"></a>

## <a name="the-directory"></a>目录

目录中的每个条目引用堆内的偏移量；也就是说，它是相对于堆开始的偏移量。 单独条目的值不一定是唯一的；在堆中多个目录条目有可能指向相同的偏移量。

泛型字典映射的目录部分具有以下结构：

- 前 4 个字节包含字典条目的数量（也就是说，字典中的相对虚拟地址数）。 我们将此值称为*N*。如果设置了高位，则按相对虚拟地址（升序）对项进行排序。

- *N*目录项跟随。 每个条目由 8 个字节（两个 4 字节段）构成：

  - 字节 0-3：RVA；字典的相对虚拟地址。

  - 字节 4-7：偏移量；相对于堆开始的偏移量。

<a name="Heap"></a>

## <a name="the-heap"></a>堆

可用流读取器计算堆的大小，计算方法是目录大小 + 4 再减去流的长度。 换言之：

```csharp
Heap Size = Stream.Length – (Directory Size + 4)
```

其中，目录大小为 `N * 8`。

在堆上每个实例化信息项的格式为：

- 此实例化信息项的长度（采用压缩 ECMA 元数据格式，以字节为单位）。 该值不包括此长度信息。

- 采用压缩 ECMA 元数据格式的泛型实例化类型或*T*的数目。

- *T*类型，每个类型都以 ECMA 类型签名格式表示。

包含每个堆元素的长度使目录部分实现简单排序，而不对堆造成影响。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头**：CorDebug.idl、CorDebug.h

**库：** CorGuids.lib

**.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]

## <a name="see-also"></a>请参阅

- [ICorDebugSymbolProvider2 接口](icordebugsymbolprovider2-interface.md)
- [调试接口](debugging-interfaces.md)
