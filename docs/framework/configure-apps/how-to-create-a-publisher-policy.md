---
title: 如何：创建发行者策略
description: 了解程序集供应商如何在 .NET 中使用升级的程序集创建发行者策略文件，以规定应用程序应使用较新版本。
ms.date: 03/30/2017
helpviewer_keywords:
- publisher policy assembly
- publisher policy files
- GAC (global assembly cache), publisher policy assembly
- global assembly cache, publisher policy assembly
ms.assetid: 8046bc5d-2fa9-4277-8a5e-6dcc96c281d9
ms.openlocfilehash: 23e9d8144ec5742e0371d566b7af59dc9dd30c9b
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105403"
---
# <a name="how-to-create-a-publisher-policy"></a>如何：创建发行者策略

程序集的供应商可以通过包括发布服务器策略文件和已升级的程序集，指出应用程序应使用较新版本的程序集。 发行者策略文件指定程序集重定向和基本代码设置，并使用与应用程序配置文件相同的格式。 发行者策略文件编译到一个程序集中并置于全局程序集缓存中。

创建发布者策略涉及三个步骤：

1. 创建发布者策略文件。

2. 创建发行者策略程序集。

3. 将发行者策略程序集添加到全局程序集缓存中。

[重定向程序集版本](redirect-assembly-versions.md)中介绍了发行者策略的架构。 下面的示例演示一个发行者策略文件，该文件将的一个版本重定向 `myAssembly` 到另一个版本。

```xml
<configuration>
   <runtime>
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
       <dependentAssembly>
         <assemblyIdentity name="myAssembly"
                           publicKeyToken="32ab4ba45e0a69a1"
                           culture="en-us" />
         <!-- Redirecting to version 2.0.0.0 of the assembly. -->
         <bindingRedirect oldVersion="1.0.0.0"
                          newVersion="2.0.0.0"/>
       </dependentAssembly>
      </assemblyBinding>
   </runtime>
</configuration>
```

若要了解如何指定基本代码，请参阅[指定程序集的位置](specify-assembly-location.md)。

## <a name="creating-the-publisher-policy-assembly"></a>创建发行者策略程序集

使用[程序集链接器（Al.exe）](../tools/al-exe-assembly-linker.md)创建发行者策略程序集。

#### <a name="to-create-a-publisher-policy-assembly"></a>创建发行者策略程序集

在命令提示符处键入以下命令：

```console
al /link:publisherPolicyFile /out:publisherPolicyAssemblyFile /keyfile:keyPairFile /platform:processorArchitecture
```

在此命令中：

- `publisherPolicyFile`参数是发行者策略文件的名称。

- `publisherPolicyAssemblyFile`自变量是此命令生成的发行者策略程序集的名称。 程序集文件名必须采用以下格式：

  "policy.majorNumber.minorNumber.mainAssemblyName.dll"

- `keyPairFile`参数是包含密钥对的文件的名称。 必须用相同的密钥对对程序集和发行者策略程序集进行签名。

- `processorArchitecture`参数标识特定于处理器的程序集的目标平台。

  > [!NOTE]
  > 从 .NET Framework 2.0 开始，可以获得特定处理器体系结构的目标功能。

从 .NET Framework 2.0 开始，可以获得特定处理器体系结构的目标功能。 下面的命令从名为的发行者策略文件中创建一个名为的发行者策略程序集 `policy.1.0.myAssembly` `pub.config` ，使用该文件中的密钥对向程序集分配一个强名称 `sgKey.snk` ，并指定该程序集面向 x86 处理器体系结构。

```console
al /link:pub.config /out:policy.1.0.myAssembly.dll /keyfile:sgKey.snk /platform:x86
```

发行者策略程序集必须与它所应用到的程序集的处理器体系结构匹配。 因此，如果程序集的 <xref:System.Reflection.AssemblyName.ProcessorArchitecture%2A> 值为 <xref:System.Reflection.ProcessorArchitecture.MSIL> ，则该程序集的发行者策略程序集必须使用创建 `/platform:anycpu` 。 必须为每个处理器特定的程序集提供单独的发行者策略程序集。

此规则的结果是，若要更改程序集的处理器体系结构，必须更改版本号的主要或次要组件，以便可以使用正确的处理器体系结构提供新的发行者策略程序集。 如果程序集具有不同的处理器体系结构，则旧的发行者策略程序集无法为程序集提供服务。

另一个结果是版本2.0 链接器无法用于为使用早期版本的 .NET Framework 编译的程序集创建发行者策略程序集，因为它始终指定处理器体系结构。

## <a name="adding-the-publisher-policy-assembly-to-the-global-assembly-cache"></a>将发行者策略程序集添加到全局程序集缓存

使用[全局程序集缓存工具（Gacutil.exe）](../tools/gacutil-exe-gac-tool.md)将发行者策略程序集添加到全局程序集缓存中。

### <a name="to-add-the-publisher-policy-assembly-to-the-global-assembly-cache"></a>将发行者策略程序集添加到全局程序集缓存

在命令提示符处键入以下命令：

```console
gacutil /i publisherPolicyAssemblyFile
```

下面的命令将添加 `policy.1.0.myAssembly.dll` 到全局程序集缓存中。

```console
gacutil /i policy.1.0.myAssembly.dll
```

> [!IMPORTANT]
> 发行者策略程序集不能添加到全局程序集缓存中，除非在该参数中指定的原始发布服务器策略文件与 `/link` 程序集位于同一目录中。

## <a name="see-also"></a>请参阅

- [使用程序集编程](../../standard/assembly/index.md)
- [运行时如何定位程序集](../deployment/how-the-runtime-locates-assemblies.md)
- [使用配置文件配置应用](index.md)
- [运行时设置架构](./file-schema/runtime/index.md)
- [配置文件架构](./file-schema/index.md)
- [重定向程序集版本](redirect-assembly-versions.md)
