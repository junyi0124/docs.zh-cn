---
title: 向 COM 公开 .NET Core 组件
description: 向 COM 公开 .NET 组件。 为互操作限定 .NET 类型。 应用互操作特性。 将 COM 的程序集打包。 从 COM 使用托管类型。
ms.date: 03/30/2017
helpviewer_keywords:
- exposing .NET Framework components to COM
- interoperation with unmanaged code, exposing .NET Framework components
- COM interop, exposing COM components
ms.assetid: e42a65f7-1e61-411f-b09a-aca1bbce24c6
ms.openlocfilehash: dc808f3e8d6bd89ba979d43e5b4ec9d787bd09b1
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90545254"
---
# <a name="exposing-net-components-to-com"></a>向 COM 公开 .NET Core 组件

对开发人员而言，编写 .NET 类型以及从非托管代码使用该类型是不同的活动。 本部分介绍编写与 COM 客户端互操作的托管代码的几个提示：

- [为互操作限定 .NET 类型](../../standard/native-interop/qualify-net-types-for-interoperation.md)。

     要向 COM 公开的所有托管类型、方法、属性、字段和事件都必须是公开的。 各类型必须具有公共无参数默认构造函数，通过 COM 只能调用该构造函数。

- [应用互操作属性](../../standard/native-interop/apply-interop-attributes.md)。

     托管代码中的自定义属性可增强组件的互操作性。

- [将 COM 的程序集打包](packaging-an-assembly-for-com.md)。

     COM 开发人员可能会要求用户总结引用和部署程序集所涉及的步骤。

 此外，本部分还确定了从 COM 客户端使用托管类型的相关任务。

## <a name="to-consume-a-managed-type-from-com"></a>从 COM 使用托管类型

1. [向 COM 注册程序集](registering-assemblies-with-com.md)。

     必须在设计时注册程序集（和类型库）中的类型。 如果安装程序未注册程序集，请指示 COM 开发人员使用 Regasm.exe。

2. [从 COM 引用 .NET 类型](how-to-reference-net-types-from-com.md)。

     COM 开发人员可使用当前使用的相同工具和技术引用程序集中的类型。

3. [调用 .NET 对象](/previous-versions/dotnet/netframework-4.0/8hw8h46b(v=vs.100))。

     COM 开发人员可采用在任何非托管类型上调用方法的方式在 .NET 对象上调用方法。 例如，COM CoCreateInstance API 激活 .NET 对象。

4. [为 COM 访问部署应用程序](/previous-versions/dotnet/netframework-4.0/c2850st8(v=vs.100))。

     具有强名称的程序集可安装在全局程序集缓存中，并向其发布者请求签名。 不具有强名称的程序集必须安装在客户端的应用程序目录中。

## <a name="see-also"></a>请参阅

- [与非托管代码交互操作](index.md)
- [COM 互操作示例：COM 客户端和 .NET 服务器](com-interop-sample-com-client-and-net-server.md)
