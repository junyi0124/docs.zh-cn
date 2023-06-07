---
title: 基础结构即代码
description: 将基础结构用作代码 (IaC) 与云本机应用程序
ms.date: 05/13/2020
ms.openlocfilehash: d130705e19e0d3d7a9e15c73f4758a22ee8ecd43
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91163734"
---
# <a name="infrastructure-as-code"></a>基础结构即代码

云本机系统采用微服务、容器和新式系统设计来实现速度和灵活性。 它们提供自动化的生成和发布阶段，以确保代码的一致性和质量。 但这只是其中的一部分。 如何预配这些系统运行时所用的云环境？

新式云-本机应用程序采用广泛接受的 [基础结构代码](/azure/devops/learn/what-is-infrastructure-as-code)，即，或 `IaC` 。  借助 IaC，你可以自动完成平台设置。 实质上是将软件工程实践（例如测试和版本控制）应用于 DevOps 做法。 你的基础结构和部署是自动、一致且可重复的。 就像连续交付自动执行传统的手动部署模型一样，基础结构即代码 (IaC 的) 在不断演变应用程序环境的管理方式。

Azure 资源管理器 (ARM) 、Terraform 和 Azure 命令行接口等工具 (CLI) 使你能够以声明方式对所需的云基础结构编写脚本。

## <a name="azure-resource-manager-templates"></a>Azure 资源管理器模板

ARM 代表 [Azure 资源管理器](/azure/azure-resource-manager/management/overview)。 它是 Azure 中内置的 API 预配引擎，作为 API 服务公开。 ARM 使你可以通过单个协调的操作来部署、更新、删除和管理 Azure 资源组中包含的资源。 为引擎提供基于 JSON 的模板，该模板指定所需资源及其配置。 ARM 会按正确的顺序与依赖关系自动协调部署。 引擎可确保幂等性。 如果已存在具有相同配置的所需资源，则将忽略设置。

Azure 资源管理器模板是一种基于 JSON 的语言，用于定义 Azure 中的各种资源。 基本架构如图10-14 所示。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "",
  "apiProfile": "",
  "parameters": {  },
  "variables": {  },
  "functions": [  ],
  "resources": [  ],
  "outputs": {  }
}
```

**图 10-14** -资源管理器模板的架构

在此模板中，可能会在资源部分中定义一个存储容器，如下所示：

```json
"resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2018-07-01",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
```

**图 10-15** -资源管理器模板中定义的存储帐户的示例

可以使用动态环境和配置信息对 ARM 模板进行参数化。 这样一来，就可以重复使用它来定义不同的环境，例如开发、QA 或生产环境。 通常，模板会在单个 Azure 资源组中创建所有资源。 如果需要，可以在单个资源管理器模板中定义多个资源组。 可以通过删除资源组本身来删除环境中的所有资源。 成本分析还可以在资源组级别运行，从而可以快速评估每个环境的成本。

GitHub 上的 [Azure 快速入门模板](https://github.com/Azure/azure-quickstart-templates) 项目中提供了许多示例或 ARM 模板。 它们可以帮助加速创建新模板或修改现有模板。

可以通过多种方式运行资源管理器模板。 最简单的方法是将它们粘贴到 Azure 门户中。 对于实验部署，这种方法很简单。 它们还可以作为 Azure DevOps 中的生成或发布过程的一部分运行。 有一些任务将利用连接到 Azure 运行模板。 对资源管理器模板的更改以增量方式应用，这意味着，若要添加新资源，只需将其添加到模板。 工具将协调当前资源与模板中定义的差异之间的差异。 然后，将创建或更改资源，使其与模板中定义的内容相匹配。  

## <a name="terraform"></a>Terraform

通常将云本机应用程序构建为 `cloud agnostic` 。 因此，应用程序并不与特定的云供应商紧密耦合，可以部署到任何公有云。

[Terraform](https://www.terraform.io/) 是可在所有主要云扮演者之间预配云本机应用程序的商业模板工具： Azure、GOOGLE CLOUD PLATFORM、AWS 和 AliCloud。 它使用的简要 YAML 略有不同，而不是使用 JSON 作为模板定义语言。

图10-16 中显示了一个示例 Terraform 文件，该文件与先前资源管理器模板 (图 10-15) 所示：

```terraform
provider "azurerm" {
  version = "=1.28.0"
}

resource "azurerm_resource_group" "test" {
  name     = "production"
  location = "West US"
}

resource "azurerm_storage_account" "testsa" {
  name                     = "${var.storageAccountName}"
  resource_group_name      = "${azurerm_resource_group.testrg.name}"
  location                 = "${var.region}"
  account_tier             = "${var.tier}"
  account_replication_type = "${var.replicationType}"

}
```

**图 10-16** -资源管理器模板的示例

Terraform 还为问题模板提供直观的错误消息。 还有一个便于验证的任务，该任务可在生成阶段用于提前捕获模板错误。

与资源管理器模板一样，可以使用命令行工具来部署 Terraform 模板。 Azure Pipelines 中还提供了社区创建的任务，可以验证和应用 Terraform 模板。

有时，Terraform 和 ARM 模板会输出有意义的值，例如新创建的数据库的连接字符串。 此信息可以在生成管道中捕获并在后续任务中使用。

## <a name="azure-cli-scripts-and-tasks"></a>Azure CLI 脚本和任务

最后，你可以利用 [Azure CLI](/cli/azure/) 以声明方式为你的云基础结构编写脚本。 可以创建、找到并共享 Azure CLI 脚本来预配和配置几乎任何 Azure 资源。 CLI 易于在学习曲线上使用。 脚本在 PowerShell 或 Bash 内执行。 它们也很简单地进行调试，尤其是在与 ARM 模板比较时。

当你需要关闭和重新部署基础结构时，Azure CLI 脚本非常有效。 更新现有环境可能比较棘手。 许多 CLI 命令不是幂等的。 这意味着，它们将在每次运行时重新创建资源，即使资源已存在也是如此。 始终可以添加代码，以便在创建资源前检查是否存在每个资源。 但这样做会使您的脚本变得臃肿且难以管理。

这些脚本也可以嵌入在 Azure DevOps 管道中 `Azure CLI tasks` 。 执行管道将调用脚本。

图10-17 显示了一个 YAML 代码段，其中列出了 Azure CLI 的版本和订阅的详细信息。 请注意内联脚本中包含 Azure CLI 命令的方式。

```yaml
- task: AzureCLI@2
  displayName: Azure CLI
  inputs:
    azureSubscription: <Name of the Azure Resource Manager service connection>
    scriptType: ps
    scriptLocation: inlineScript
    inlineScript: |
      az --version
      az account show
```

**图 10-17** -Azure CLI 脚本

在本文中， [什么是基础结构即代码](/azure/devops/learn/what-is-infrastructure-as-code)，Author Sam Guckenheimer 介绍了 "实现 IaC 的团队如何快速、大规模地交付稳定环境。 团队避免手动配置环境，并通过代码来表示环境所需的状态，从而强制实现一致性。 使用 IaC 的基础结构部署是可重复的，并防止配置偏移或缺少依赖项导致的运行时问题。 DevOps 团队可以结合使用一组统一的做法和工具，迅速、可靠、大规模地提供应用程序及其支持基础结构。

>[!div class="step-by-step"]
>[上一页](feature-flags.md)
>[下一页](application-bundles.md)
