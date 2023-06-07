---
title: dotnet help 命令
description: dotnet help 命令可显示指定命令更详细的在线文档。
ms.date: 02/14/2020
ms.openlocfilehash: d583142edabb24df972bdf9a06dbfe04688f9d97
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634463"
---
# <a name="dotnet-help-reference"></a>dotnet help 参考

 本文适用于： ✔️ .NET Core 2.0 SDK 及更高版本

## <a name="name"></a>名称

`dotnet help` - 显示指定命令更详细的在线文档。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet help <COMMAND_NAME> [-h|--help]
```

## <a name="description"></a>说明

`dotnet help` 命令打开 docs.microsoft.com 参考页，以提供指定命令的更多详细信息。

## <a name="arguments"></a>参数

- **`COMMAND_NAME`**

  .NET CLI 命令名称。 有关有效 CLI 命令的列表，请参阅 [CLI 命令](index.md#cli-commands)。

## <a name="options"></a>选项

- **`-h|--help`**

  打印出有关命令的简短帮助。

## <a name="examples"></a>示例

- 打开 [dotnet new](dotnet-new.md) 命令的文档页：

  ```dotnetcli
  dotnet help new
  ```
