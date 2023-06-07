---
title: 模型生成器 Azure 培训资源
description: Azure 机器学习资源指南
ms.topic: reference
ms.date: 06/01/2020
ms.author: luquinta
author: luisquintanilla
ms.openlocfilehash: 1321967cacdd373acc19923f992d30c5453ea869
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90556214"
---
# <a name="model-builder-azure-training-resources"></a>模型生成器 Azure 培训资源

以下指南可帮助你详细了解用于通过模型生成器在 Azure 中训练模型的资源。

## <a name="what-is-an-azure-machine-learning-experiment"></a>什么是 Azure 机器学习试验？

Azure 机器学习试验是一项资源，需要先生成它才能在 Azure 上运行模型生成器训练。

试验会封装一次或多次机器学习训练运行的配置和结果。 试验属于特定工作区。 第一次创建试验时，将在该工作区中注册其名称。 如果任何后续运行使用相同的试验名称，则该运行将记录为同一试验的一部分。 否则会创建新试验。

## <a name="what-is-an-azure-machine-learning-workspace"></a>什么是 Azure 机器学习工作区？

工作区是一项 Azure 机器学习资源，它为在运行过程中创建的所有 Azure 机器学习资源和项目提供了一个中心位置。

若要创建 Azure 机器学习工作区，需要满足以下条件：

- 姓名:工作区的名称介于 3-33 个字符之间。 名称只能包含字母数字字符和连字符。
- 地区：工作区和资源部署到的数据中心的地理位置。 建议选择靠近你或你的客户的位置。
- 资源组：用于容纳 Azure 解决方案所有相关资源的容器。

## <a name="what-is-an-azure-machine-learning-compute"></a>什么是 Azure 机器学习计算？

Azure 机器学习计算是基于云的 Linux VM，用于训练。

若要创建 Azure 机器学习工作区，需要满足以下条件：

- 姓名:工作区的名称介于 2-16 个字符之间。 名称只能包含字母数字字符和连字符。
- 计算大小

    模型生成器可以使用以下 GPU 优化的计算类型之一：

    | 大小 | vCPU | 内存: GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大 NIC 数 |
    |---|---|---|---|---|---|---|---|
    | Standard_NC12   | 12 | 112 | 680  | 2 | 24 | 48 | 2 |
    | Standard_NC24   | 24 | 224 | 1440 | 4 | 48 | 64 | 4 |

    有关 GPU 优化计算类型的更多详细信息，请访问 [NC 系列 Linux VM 文档](/azure/virtual-machines/nc-series?bc=%252fazure%252fvirtual-machines%252flinux%252fbreadcrumb%252ftoc.json&toc=%252fazure%252fvirtual-machines%252flinux%252ftoc.json)。
- 计算优先级

  - 低优先级：适用于执行时间较短的任务。 可能会受到中断和缺乏可用性的影响。 通常成本较低，因为它充分利用了 Azure 中的过剩容量。
  - 专用：适用于任何持续时间的任务，特别是长时间运行的作业。 不受中断或缺乏可用性影响。 通常成本更高，因为它在 Azure 中为任务保留一组专用计算资源。

## <a name="training"></a>训练

在 Azure 上训练仅适用于模型生成器图像分类方案。 用于训练这些模型的算法是基于 ResNet50 体系结构的深度神经网络。 训练过程需要一定的时间，并且根据所选计算的大小和数据量，所需时间可能会有所不同。 可以在 Visual Studio 中选择“监视 Azure 门户中的当前运行”链接以跟踪运行进度。

## <a name="results"></a>结果

训练完成后，会将两个项目添加到你的解决方案，它们包含以下后缀：

- *ConsoleApp*：C# .Net Core 控制台应用程序，提供用于生成预测管道和进行预测的起始代码。
- *模型*：C# .NET Standard 应用程序，包含多个数据模型，它们定义了输入和输出模型数据的架构以及以下资产：

  - bestModel.onnx：Open Neural Network Exchange (ONNX) 格式的模型序列化版本。 ONNX 是 AI 模型的开源格式，它支持 ML.NET、PyTorch 和 TensorFlow 等框架之间的互操作性。
  - bestModelMap.json：进行预测以将模型输出映射到文本类别时使用的类别列表。
  - MLModel.zip：ML.NET 预测管道的序列化版本，它使用 bestModel.onnx 模型的序列化版本进行预测，使用 `bestModelMap.json` 文件映射输出。

## <a name="use-the-machine-learning-model"></a>使用机器学习模型

模型项目中的 `ModelInput` 和 `ModelOutput` 类分别定义模型预期输入和输出的架构。

在图像分类方案中，`ModelInput` 包含两列：

- `ImageSource`：图像位置的字符串路径。
- `Label`：图像所属的实际类别。 `Label` 仅在训练时用作输入，进行预测时不需要提供它。

`ModelOutput` 包含两列：

- `Prediction`：图像的预测类别。
- `Score`：所有类别的概率列表（最高为 `Prediction`）。

## <a name="troubleshooting"></a>疑难解答

### <a name="cannot-create-compute"></a>无法创建计算

如果在创建 Azure 机器学习计算期间出错，计算资源可能仍然存在，只是处于出错状态。 如果尝试重新创建同名计算资源，操作将会失败。 请通过以下一种方法修复此错误：

- 创建具有其他名称的新计算
- 转到 Azure 门户，并删除原始计算资源
