---
title: 云本机应用程序捆绑包
description: 构建适用于 Azure 的云本机 .NET 应用 |云本机应用程序捆绑包
ms.date: 05/13/2020
ms.openlocfilehash: 7f1fcd448f3299a31043bf269717f7b777329c62
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158118"
---
# <a name="cloud-native-application-bundles"></a>云本机应用程序捆绑包

云本机应用程序的一个关键属性是，它们利用云的功能加快开发速度。 此设计通常意味着完整应用程序使用不同种类的技术。 应用程序可以在 Docker 容器中提供，某些服务可能使用 Azure Functions，而其他部分则可能会直接在具有硬件 GPU 加速功能的大型金属服务器上运行的虚拟机上运行。 没有两个云本机应用程序是相同的，因此很难提供单一机制来传送它们。

Docker 容器可以在 Kubernetes 上使用 Helm 图表进行部署。 可以使用 Terraform 模板分配 Azure Functions。 最后，可以使用 Terraform 分配虚拟机，但使用 Ansible 构建虚拟机。 这是一种全面的技术，没有办法将它们一起打包到合理的包中。 到现在为止。

云本机应用程序捆绑 (CNABs) 通过许多相投公司（如 Microsoft、Docker 和 HashiCorp）进行联合工作，以开发用于打包分布式应用程序的规范。

这项工作已于2018年12月公布，因此，为了向更大的社区公开工作，仍有一些工作要做。 不过，已经有一个 [开放规范](https://github.com/deislabs/cnab-spec) 和一个称为 [Duffle](https://duffle.sh/)的引用实现。 此工具是在中转和 Microsoft 之间的共同努力。

CNABs 可以包含不同种类的安装技术。 这就允许 Helm 图、Terraform 模板和 Ansible 行动手册共存于同一包中。 构建后，这些包将是独立的，并且是可移植的;可以通过 USB 驾驶杆进行安装。  对包进行加密签名，以确保它们来自声明的参与方。

CNAB 的核心是一个名为的文件 `bundle.json` 。 此文件定义绑定的内容，这些内容为 Terraform 或 images 或其他任何内容。 图11-9 定义调用某些 Terraform 的 CNAB。 但请注意，它实际上定义了用于调用 Terraform 的调用映像。 打包时，位于 *cnab* 目录中的 docker 文件内置于 docker 映像中，该映像将包含在包中。 将 Terraform 安装在捆绑中的 Docker 容器内意味着用户无需在其计算机上安装 Terraform 即可运行绑定。

```json
{
    "name": "terraform",
    "version": "0.1.0",
    "schemaVersion": "v1.0.0-WD",
    "parameters": {
        "backend": {
            "type": "boolean",
            "defaultValue": false,
            "destination": {
                "env": "TF_VAR_backend"
            }
        }
    },
    "invocationImages": [
        {
        "imageType": "docker",
        "image": "cnab/terraform:latest"
        }
    ],
    "credentials": {
        "tenant_id": {
            "env": "TF_VAR_tenant_id"
        },
        "client_id": {
            "env": "TF_VAR_client_id"
        },
        "client_secret": {
            "env": "TF_VAR_client_secret"
        },
        "subscription_id": {
            "env": "TF_VAR_subscription_id"
        },
        "ssh_authorized_key": {
            "env": "TF_VAR_ssh_authorized_key"
        }
    },
    "actions": {
        "status": {
            "modifies": true
        }
    }
}
```

**图 10-18** -示例 Terraform 文件

`bundle.json`还定义了一组可向下传递到 Terraform 的参数。 捆绑的参数化允许在各种不同的环境中进行安装。

CNAB 格式也是灵活的，使其可用于任何云。 甚至可以针对本地解决方案（例如 [OpenStack](https://www.openstack.org/)）使用它。

## <a name="devops-decisions"></a>DevOps 决策

在 DevOps 空间中，有很多有用的工具，这些工具还提供了有关如何成功的更好的书籍和文章。 DevOps 旅程入门的最喜爱的书籍是 [Phoenix 项目](https://www.oreilly.com/library/view/the-phoenix-project/9781457191350/)，该项目遵循从 Noop 到 DevOps 的虚构公司的转换。 特别是在部署复杂的云本机应用程序时，DevOps 不再是 "更好的"。 这是一种要求，在任何项目开始时，应计划并为其分配资源。

## <a name="references"></a>参考

- [Azure DevOps](https://azure.microsoft.com/services/devops/)
- [Azure Resource Manager](/azure/azure-resource-manager/management/overview)
- [Terraform](https://www.terraform.io/)
- [Azure CLI](/cli/azure/)

>[!div class="step-by-step"]
>[上一页](infrastructure-as-code.md)
>[下一页](summary.md)
