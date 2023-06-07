---
title: dotnet nuget verify 命令
description: dotnet nuget verify 命令可验证已签名的包。
author: kartheekp-ms
ms.date: 10/08/2020
ms.openlocfilehash: 6cb368e2b6c203f3774b4450c0831c5d6b2dc0e8
ms.sourcegitcommit: b59237ca4ec763969a0dd775a3f8f39f8c59fe24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2020
ms.locfileid: "91957085"
---
# <a name="dotnet-nuget-verify"></a>dotnet nuget verify

**本文适用于：** ✔️ .NET 5.0.100-rc.2.x SDK 及更高版本

## <a name="name"></a>名称

`dotnet nuget verify` -验证已签名的 NuGet 包。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet nuget verify [<package-path(s)>]
    [--all]
    [--certificate-fingerprint <FINGERPRINT>]
    [-v|--verbosity <LEVEL>]

dotnet nuget verify -h|--help
```

## <a name="description"></a>描述

`dotnet nuget verify` 命令验证已签名的 NuGet 包。

## <a name="arguments"></a>参数

- **`package-path(s)`**

  指定要验证的包的文件路径。 可以传入多个位置参数以验证多个包。

## <a name="options"></a>选项

- **`--all`**

  指定应对包执行的所有可能的验证。 默认情况下，只验证 `signatures`。

> [!NOTE]
> 此命令当前仅支持 `signature` 验证。

- **`--certificate-fingerprint <FINGERPRINT>`**

  验证签名者证书是否与指定的其中一个 `SHA256` 指纹匹配。 可以多次提供此选项以提供多个指纹。

* **`-v|--verbosity <LEVEL>`**

  设置 MSBuild 详细级别。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `normal`。

* **`-h|--help`**

  打印出有关命令的简短帮助。

## <a name="examples"></a>示例

- 验证 foo.nupkg：

  ```dotnetcli
  dotnet nuget verify foo.nupkg
  ```

- 验证多个 NuGet 包 - foo.nupkg 和指定目录中的所有 .nupkg 文件 ：

  ```dotnetcli
  dotnet nuget verify foo.nupkg c:\mydir\*.nupkg
  ```

- 验证 foo.nupkg 签名是否与指定的证书指纹匹配：

  ```dotnetcli
  dotnet nuget verify foo.nupkg --certificate-fingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039
  ```

- 验证 foo.nupkg 签名是否与指定的其中一个证书指纹匹配：

  ```dotnetcli
  dotnet nuget verify foo.nupkg --certificate-fingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039 --certificate-fingerprint EC10992GG5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E027
  ```
