---
title: UnsafeNclNativeMethods 类（System.Net）
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.UnsafeNclNativeMethods
- System.Net.UnsafeNclNativeMethods.NativePKI
- System.Net.UnsafeNclNativeMethods.NativePKI.FindClientCertificates
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 46756a0d1d69b4768dbb8dcdd7ab098d3f1849bf
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990433"
---
# <a name="unsafenclnativemethods-class"></a>UnsafeNclNativeMethods 类

包含导入不安全的本机网络方法的类。 无法继承此类。

```csharp
internal static class UnsafeNclNativeMethods
```

> [!WARNING]
> 此类是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="nativepki-class"></a>NativePKI 类

包含从 crypt32.dll 导入的方法。 使用超文本传输协议（HTTPS）时，这些方法会处理证书。 无法继承此类。

```csharp
internal static class NativePKI
```

## <a name="nativepkifindclientcertificates-method"></a>NativePKI. FindClientCertificates 方法

发现要发送到服务器的可用客户端证书。

```csharp
internal static X509CertificateCollection FindClientCertificates
```

### <a name="return-value"></a>返回值

<xref:System.Security.Cryptography.X509Certificates.X509CertificateCollection?displayProperty=nameWithType>

可用客户端证书的集合。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）
