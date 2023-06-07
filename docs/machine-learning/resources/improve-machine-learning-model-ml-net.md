---
title: 操作说明：提高模型准确性
description: 了解如何提高模型准确性
ms.date: 04/29/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc
ms.openlocfilehash: 8f3b283de378a37bfe429688207ea9fb52f9ca7f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "75739582"
---
# <a name="improve-mlnet-model-accuracy"></a>提高 ML.NET 模型准确性

了解如何提高模型的准确性。

## <a name="reframe-the-problem"></a>重新定义问题

有时，改进模型可能与用于训练模型的数据或技术无关。 相反，可能只是提出了错误的问题。 考虑从不同的角度看待问题，并利用数据提取潜在指示和隐藏关系，以便优化问题。

## <a name="provide-more-data-samples"></a>提供更多数据示例

与人类一样，训练算法获得的信息越多，性能更好的可能性就越大。 提高模型性能的方法之一是为算法提供更多训练数据示例。 模型从中进行学习的数据越多，模型能够正确识别的案例就越多。

## <a name="add-context-to-the-data"></a>为数据添加上下文

单个数据点的含义可能难以解读。 围绕数据点生成上下文有助于算法和主题专家更好地做出决策。 例如，房屋拥有三间卧室这一事实本身并不能很好地说明其价格。 但是，如果在添加上下文之后，现在知道它位于大都市圈附近的郊区，屋主平均年龄为 38 岁，家庭平均收入为 80,000 美元，附近学校的排名在前 20%，那么算法可在更多信息的基础之上做出自己的决策。 所有这些上下文都可以作为特征添加到机器学习模型的输入中。

## <a name="use-meaningful-data-and-features"></a>使用有意义的数据和特征

虽然更多的数据示例和特征可以帮助提高模型的准确性，但它们也可能会引入干扰，因为并非所有数据和特征都有意义。 因此，了解哪些特征对算法所做的决策具有最大影响非常重要。 使用排列特征重要性 (PFI) 等技术可以帮助识别这些突出的特征，这不仅有助于解释模型，还可以使用输出作为特征选择方法来减少进入训练过程的干扰特征的数量。

有关使用 PFI 的详细信息，请参阅[使用排列特征重要性解释模型预测](../how-to-guides/explain-machine-learning-model-permutation-feature-importance-ml-net.md)。

## <a name="cross-validation"></a>交叉验证

交叉验证是一种训练和模型评估技术，可将数据拆分为多个分区，并利用这些分区训练多个算法。 此技术通过保留来自训练过程的数据来提高模型的可靠性。 除提高不可见观测的性能之外，在数据受限的环境中，它还可用作使用较小数据集训练模型的有效工具。

访问以下链接，了解[如何在 ML.NET 中使用交叉验证](../how-to-guides/train-machine-learning-model-cross-validation-ml-net.md)

## <a name="hyperparameter-tuning"></a>超参数调整

训练机器学习模型是一个迭代和探索的过程。 例如，使用 K-Means 算法训练模型时，最优群集数是多少？ 答案取决于许多因素，例如数据的结构。 找到该数字需要尝试不同的 k 值，然后通过评估性能来确定最佳值。 调整这些参数以找到最佳模型的做法称为超参数调整。

## <a name="choose-a-different-algorithm"></a>选择其他算法

回归和分类等机器学习任务包含各种算法实现。 可能出现尝试解决的问题和数据的构造方式与当前算法不能很好地匹配的情况。 在此类情况下，请考虑为任务使用其他算法，查看它是否能够从数据中更好地学习。

以下链接提供[有关如何选择算法的更多指导](../how-to-choose-an-ml-net-algorithm.md)。
