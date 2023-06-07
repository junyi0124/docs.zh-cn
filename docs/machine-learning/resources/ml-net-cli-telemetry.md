---
title: ML.NET CLI 遥测收集
description: 了解收集使用情况信息以供分析的 ML.NET CLI 遥测功能、收集的数据，以及如何禁用遥测。 此外，还可以找到 .NET 许可协议的链接以及有关 Microsoft GDPR 合规性的信息。
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: mlnet-tooling
ms.openlocfilehash: 753361abdac5a2e979873003f419232a069b2015
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90546427"
---
# <a name="telemetry-collection-by-the-mlnet-cli"></a>ML.NET CLI 遥测收集

[ML.NET CLI](../automate-training-with-cli.md) 包含遥测功能，可收集聚合后供 Microsoft 使用的匿名使用数据。

## <a name="how-microsoft-uses-the-data"></a>Microsoft 如何使用这些数据

产品团队使用 ML.NET CLI 遥测数据来帮助了解如何改进工具。 例如，如果客户不经常使用特定机器学习任务，则产品团队可调查原因并使用调查结果来确定功能开发的优先级。 ML.NET CLI 遥测还可以帮助调试崩溃和代码异常等问题。

尽管产品团队很感激大家提供此类见解，我们也知道并非每位用户都愿意发送此类数据。 [了解如何禁用遥测。](#opt-out-of-data-collection)

## <a name="scope"></a>范围

`mlnet` 命令可启动 ML.NET CLI，但命令本身不收集遥测。

在未附加其他命令的情况下，遥测在运行 `mlnet` 命令时处于*未启用*状态。 例如：

- `mlnet`
- `mlnet --help`

运行 [ML.NET CLI 命令](../reference/ml-net-cli-reference.md)（例如 `mlnet classification`）时，遥测处于*启用*状态。

## <a name="opt-out-of-data-collection"></a>选择退出数据收集

ML.NET CLI 遥测功能默认处于启用状态。

通过将 `MLDOTNET_CLI_TELEMETRY_OPTOUT` 环境变量设置为 `1` 或 `true`，可以选择退出遥测功能。 此环境变量全局适用于 ML.NET CLI 工具。

## <a name="data-points-collected"></a>收集的数据点

此功能收集以下数据：

- 调用了哪个命令，如 `classification`
- 使用的命令行参数名称（即“dataset、label-col、output-path、train-time、verbosity”）
- 经过哈希处理的 MAC 地址：计算机的加密 (SHA256) 匿名唯一 ID
- 调用时间戳
- 仅用于确定地理位置的三个八进制数 IP 地址（不是完整 IP 地址）
- 使用的所有自变量/参数的名称。 不属于客户提供的值，例如字符串
- 经过哈希处理的数据集的文件名
- 数据集文件大小存储桶
- 操作系统和版本
- ML 任务命令的值：分类值，例如 `regression`、`classification` 和 `recommendation`
- ML.NET CLI 版本（即 0.3.27703.4）

数据通过 [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 技术安全地发送到 Microsoft 服务器，提供对保留数据的受限访问权限，并在严格的安全控制下从安全的 [Azure 存储](https://azure.microsoft.com/services/storage/)系统进行使用。

### <a name="data-points-not-collected"></a>未收集的数据点

遥测功能*不*收集：

- 个人数据，例如用户名
- 数据集文件名
- 数据集文件中的数据

如果怀疑 ML.NET CLI 遥测在收集敏感数据，或认为我们处理数据的方式不安全或不恰当，请在 [ML.NET](https://github.com/dotnet/machinelearning) 存储库中记录问题以供调查。

## <a name="license"></a>许可证

ML.NET CLI 的 Microsoft 分发由 [Microsoft 软件许可条款：Microsoft .NET 库](https://aka.ms/dotnet-core-eula)许可。 有关数据收集和处理的详细信息，请参阅标题为“数据”的部分。

## <a name="disclosure"></a>公开

首次运行 [ML.NET CLI 命令](../reference/ml-net-cli-reference.md)（例如 `mlnet classification`）时，ML.NET CLI 工具会显示披露信息文本，告诉如何选择退出遥测。 文本可能会因运行的 CLI 版本而略有不同。

## <a name="see-also"></a>请参阅

- [ML.NET CLI 参考](../reference/ml-net-cli-reference.md)
- [Microsoft 软件许可条款：Microsoft .NET 库](https://aka.ms/dotnet-core-eula)
- [Microsoft 隐私政策](https://www.microsoft.com/trustcenter/privacy/)
- [Microsoft 隐私声明](https://privacy.microsoft.com/privacystatement)
