---
ms.openlocfilehash: f22ee4accf9ff00814fa540adce4b9ecc6686da4
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90537726"
---
无需运行 [`dotnet restore`](~/docs/core/tools/dotnet-restore.md)，因为它由所有需要还原的命令隐式运行，如 `dotnet new`、`dotnet build`、`dotnet run`、`dotnet test`、`dotnet publish` 和 `dotnet pack`。 若要禁用隐式还原，请使用 `--no-restore` 选项。

在执行显式还原有意义的某些情况下，例如 [Azure DevOps Services 中的持续集成生成](/azure/devops/build-release/apps/aspnet/build-aspnet-core)中，或在需要显式控制还原发生时间的生成系统中，`dotnet restore` 命令仍然有用。

有关如何使用 NuGet 源的信息，请参阅 [`dotnet restore` 文档](../docs/core/tools/dotnet-restore.md)。
