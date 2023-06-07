---
title: 日志记录类（System.Net）
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.Logging
- System.Net.Logging.Associate
- System.Net.Logging.Enter
- System.Net.Logging.Exception
- System.Net.Logging.Exit
- System.Net.Logging.get_Http
- System.Net.Logging.get_On
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: ad85fdd41252ed147eb5fe1a9db029db046d720c
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990442"
---
# <a name="logging-class"></a>日志记录类

提供跟踪日志记录功能。

```csharp
internal class Logging
```

> [!WARNING]
> 此类是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="associate-method"></a>关联方法

记录两个对象彼此关联的信息。

```csharp
internal static void Associate(TraceSource traceSource, object objA, object objB)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `objA` <xref:System.Object>

  要与关联的对象 `objB` 。

- `objB` <xref:System.Object>

  要与关联的对象 `objA` 。

## <a name="entertracesource-object-string-string-method"></a>Enter （TraceSource，object，string，string）方法

日志进入到方法。

```csharp
internal static void Enter(TraceSource traceSource, object obj, string method, string param)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `obj` <xref:System.Object>

  在其上调用方法的对象。

- `method` <xref:System.String>

  正在输入的方法。

- `param` <xref:System.String>

  传递给方法的参数。

## <a name="entertracesource-object-string-object-method"></a>Enter （TraceSource，object，string，object）方法

日志进入到方法。

```csharp
internal static void Enter(TraceSource traceSource, object obj, string method, object paramObject)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `obj` <xref:System.Object>

  在其上调用方法的对象。

- `method` <xref:System.String>

  正在输入的方法。

- `paramObject` <xref:System.Object>

  传递给方法的参数。

## <a name="entertracesource-string-string-string-method"></a>Enter （TraceSource，string，string，string）方法

日志进入到方法。

```csharp
internal static void Enter(TraceSource traceSource, string obj, string method, string param)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `obj` <xref:System.String>

  在其上调用方法的对象。

- `method` <xref:System.String>

  正在输入的方法。

- `param` <xref:System.String>

  传递给方法的参数。

## <a name="entertracesource-string-string-object-method"></a>Enter （TraceSource，string，string，object）方法

日志进入到方法。

```csharp
internal static void Enter(TraceSource traceSource, string obj, string method, object paramObject)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `obj` <xref:System.String>

  在其上调用方法的对象。

- `method` <xref:System.String>

  正在输入的方法。

- `paramObject` <xref:System.Object>

  传递给方法的参数。

## <a name="entertracesource-string-string-method"></a>Enter （TraceSource，string，string）方法

日志进入到方法。

```csharp
internal static void Enter(TraceSource traceSource, string method, string parameters)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `method` <xref:System.String>

  正在输入的方法。

- `parameters` <xref:System.String>

  传递给方法的参数。

## <a name="entertracesource-string-method"></a>Enter （TraceSource，string）方法

日志进入到方法。

```csharp
internal static void Enter(TraceSource traceSource, string msg)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `msg` <xref:System.String>

  要记录到跟踪源的入口消息。

## <a name="exception-method"></a>Exception 方法

记录异常并还原缩进。

```csharp
internal static void Exception(TraceSource traceSource, object obj, string method, Exception e)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `obj` <xref:System.Object>

  调用了引发异常的方法的对象。

- `method` <xref:System.String>

  引发异常的方法。

- `e` <xref:System.Exception>

  抛出的异常。

## <a name="exittracesource-object-string-object-method"></a>Exit （TraceSource、object、string、object）方法

日志从函数退出。

```csharp
internal static void Exit(TraceSource traceSource, object obj, string method, object retObject)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `obj` <xref:System.Object>

  在其上调用方法的对象。

- `method` <xref:System.String>

  正在退出的方法。

- `retObject` <xref:System.Object>

  由方法返回的值。

## <a name="exittracesource-string-string-object-method"></a>Exit （TraceSource、string、string、object）方法

日志从函数退出。

```csharp
internal static void Exit(TraceSource traceSource, string obj, string method, object retObject)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `obj` <xref:System.String>

  在其上调用方法的对象。

- `method` <xref:System.String>

  正在退出的方法。

- `retObject` <xref:System.Object>

  由方法返回的值。

## <a name="exittracesource-object-string-string-method"></a>Exit （TraceSource，object，string，string）方法

日志从函数退出。

```csharp
internal static void Exit(TraceSource traceSource, object obj, string method, string retValue)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `obj` <xref:System.Object>

  在其上调用方法的对象。

- `method` <xref:System.String>

  正在退出的方法。

- `retValue` <xref:System.String>

  由方法返回的值。

## <a name="exittracesource-string-string-string-method"></a>Exit （TraceSource，string，string，string）方法

日志从函数退出。

```csharp
internal static void Exit(TraceSource traceSource, string obj, string method, string retValue)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `obj` <xref:System.String>

  在其上调用方法的对象。

- `method` <xref:System.String>

  正在退出的方法。

- `retValue` <xref:System.String>

  由方法返回的值。

## <a name="exittracesource-string-string-method"></a>Exit （TraceSource，string，string）方法

日志从函数退出。

```csharp
internal static void Exit(TraceSource traceSource, string method, string parameters)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `method` <xref:System.String>

  正在退出的方法。

- `parameters` <xref:System.String>

  传递给正在退出的方法的参数。

## <a name="exittracesource-string-method"></a>Exit （TraceSource，string）方法

日志从函数退出。

```csharp
internal static void Exit(TraceSource traceSource, string msg)
```

### <a name="parameters"></a>参数

- `traceSource` <xref:System.Diagnostics.TraceSource>

  要将事件记录到的跟踪源。

- `msg` <xref:System.String>

  要记录到跟踪源的退出消息。

## <a name="http-property"></a>Http 属性

获取 "系统 .Net" 的跟踪源。

```csharp
internal static TraceSource Http { get; }
```

### <a name="property-value"></a>属性值

<xref:System.Diagnostics.TraceSource>\
"系统 .Net" 的跟踪源， `null` 如果未启用日志记录，则为。

## <a name="on-property"></a>On 属性

获取一个值，该值指示是否启用日志记录。

```csharp
internal static bool On { get; }
```

### <a name="property-value"></a>属性值

<xref:System.Boolean>\
如果启用日志记录，则为 `true`；否则为 `false`。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）
