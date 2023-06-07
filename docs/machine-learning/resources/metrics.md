---
title: ML.NET 指标
description: 了解用于评估 ML.NET 模型性能的指标
ms.date: 12/17/2019
ms.openlocfilehash: 046e0a3feea2da702dfef5ca9ce4f498fce5fb26
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91804817"
---
# <a name="evaluate-your-mlnet-model-with-metrics"></a>使用指标评估 ML.NET 模型

了解用于评估 ML.NET 模型的指标。

评估指标特定于模型所执行的机器学习任务的类型。

例如，对于分类任务，通过衡量预测类别与实际类别的匹配程度来评估模型。 对于聚类分析，评估基于群集项的接近程度，以及群集之间的距离。

## <a name="evaluation-metrics-for-binary-classification"></a>二元分类评估指标

| 指标   |      说明      |  期望 |
|-----------|-----------------------|-----------|
| **准确性** |  [准确性](https://en.wikipedia.org/wiki/Accuracy_and_precision#In_binary_classification)指正确预测与测试数据集的比例。 它是正确预测次数与输入示例总数的比率。 如果属于每个类的示例数量相似，此方法会很有效。| **越接近 1.00 越好**。 但刚好 1.00 表示存在问题（通常包括：标签/目标泄漏、过度拟合或使用训练数据进行测试）。 当测试数据处于非平衡状态（大部分实例属于其中一个类）、数据集较小，或分数趋近 0.00 或 1.00 时，准确性并不能真实反应分类器的效果，此时需要检查其他指标。 |
| **AUC** |    [aucROC](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) 或曲线下面积通过扫描真正率和假正率来测量曲线下面积。  |   **越接近 1.00 越好**。 它应大于 0.50 才能接受模型。 AUC 为 0.50 或更低的模型毫无意义。 |
| **AUCPR** | aucPR 或 *查准率-查全率曲线的曲线下面积*：在类不均衡的情况下（高度偏斜的数据集），是成功预测的有用度量值。 |  **越接近 1.00 越好**。 接近 1.00 的高分表明分类器返回准确的结果（高查准率），以及返回大部分正结果（高查全率）。 |
| **F1 分数** | [F1 分数](https://en.wikipedia.org/wiki/F1_score)也称为 *均衡 F 分数或 F 度量值*。 这是查准率和查全率的调和平均数。 如果想要在查准率和查全率之间寻求平衡，F1 分数很有用。| **越接近 1.00 越好**。  F1 分数的最佳值为 1.00，最差值为 0.00。 它可指示分类器的查准率。 |

有关二元分类指标的更多详细信息，请阅读以下文章：

- [准确性、查准率、查全率还是 F1？](https://towardsdatascience.com/accuracy-precision-recall-or-f1-331fb37c5cb9)
- [二元分类指标类](xref:Microsoft.ML.Data.BinaryClassificationMetrics)
- [The Relationship Between Precision-Recall and ROC Curves](http://pages.cs.wisc.edu/~jdavis/davisgoadrichcamera2.pdf)（查准率-查全率曲线与 ROC 曲线之间的关系）

## <a name="evaluation-metrics-for-multi-class-classification"></a>多类分类评估指标

| 指标   |      说明      |  期望 |
|-----------|-----------------------|-----------|
| **微观准确性** |  [微平均准确性](xref:Microsoft.ML.Data.MulticlassClassificationMetrics.MicroAccuracy)聚合所有类的贡献度来计算平均指标。 它是正确预测的实例的部分。 微平均不考虑类成员资格。 基本而言，每个“示例-类”对对准确性指标的贡献度相同。 | **越接近 1.00 越好**。 在多类分类任务中，如果怀疑可能存在类不均衡的现象（即 某个类的实例可能比其他类的实例多很多），则应选择微观准确性，而不是宏观准确性。|
| **宏观准确性** | [宏平均准确性](xref:Microsoft.ML.Data.MulticlassClassificationMetrics.MacroAccuracy)指类级别的平均准确性。 每个类的准确性都会进行计算，宏观准确性就是这些准确性的平均值。 基本而言，每个类对准确性指标的贡献度相同。 占比较小的类与占比较大的类拥有同等的权重。 无论数据集包含多少个来自该类的实例，宏平均指标都会对每个类赋予相同的权重。 |  **越接近 1.00 越好**。  它为每个类单独计算该指标，然后取平均值（因此实现平等对待所有类） |
| **对数损失**| 对数损失测量分类模型的性能，其中预测输入是介于 0.00 和 1.00 之间的概率值。 随着预测概率偏离实际标签，对数损失会增加。 | **越接近 0.00 越好**。 完美模型的对数损失为 0.00。 我们的机器学习模型旨在最小化此值。|
| **对数损失减小** | [对数损失减小](xref:Microsoft.ML.Data.MulticlassClassificationMetrics.LogLossReduction)可以解释为分类器相较随机预测的优势。| **取值范围为 [-inf, 1.00]，其中 1.00 表示完美的预测，0.00 表示准确性一般的预测**。 例如，如果该值等于 0.20，则可以将其解释为“正确预测的概率比随机猜测的概率高 20%”|

微观准确性通常能够更好地与 ML 预测的业务需求保持一致。 如果想要选择单个指标用于选择多类分类任务的质量，则通常应选择微观准确性。

例如，对于支持票证分类任务：（将传入的票证映射到支持团队）

- 微观准确性 - 传入票证分类给正确团队的频率如何？
- 宏观准确性 - 对于普通团队而言，传入票证符合其业务范围的频率如何？

在此示例中，宏观准确性对于小型团队而言任务过重；一个每年仅收到 10 个票证的小团队被视为每年可收到 10,000 个票证的大团队。 在本例中，微观准确性与“公司可通过自动化票证路由流程节省多少时间/金钱”这一业务需求相关性更高。

有关多类分类指标的更多详细信息，请阅读以下文章：

- [查准率、查全率和 F 分数的微平均及宏平均](https://rushdishams.blogspot.com/2011/08/micro-and-macro-average-of-precision.html)
- [Multiclass Classification with Imbalanced Dataset](https://towardsdatascience.com/machine-learning-multiclass-classification-with-imbalanced-data-set-29f6a177c1a)（不均衡数据集的多类分类）

## <a name="evaluation-metrics-for-regression-and-recommendation"></a>回归和建议评估指标

回归和建议任务均预测数字。 对于回归，数字可以是受输入属性影响的任何输出属性。 对于建议，数字通常是一个分级值（例如 1 和 5 之间），或者为是/否建议（分别用 1 和 0 表示）。

| 指标   |      说明      |  期望 |
|----------|-----------------------|-----------|
| **R 平方** |  [R 平方 (R2)](https://en.wikipedia.org/wiki/Coefficient_of_determination) 又称为 *决定系数*，其将模型的预测能力表示为取值范围介于 -inf 和 1.00 之间的值。 1.00 意味着完美拟合，且拟合度可以无穷差，因此分数可能为负数。 分数为 0.00 表示模型正在猜测标签的预期值。 R2 测量实际测试数据值与预测值的接近程度。 | **越接近 1.00 表示质量越好**。 但是，有时较低的 R 平方值（例如 0.50）对于方案而言可能完全正常或足够好，并且较高的 R 平方值并不总是好结果且有时可疑。 |
| **绝对值损失** |  [绝对值损失](https://en.wikipedia.org/wiki/Mean_absolute_error)又称为 *平均绝对误差 (MAE)*，其测量预测结果与实际结果的接近程度。 它是所有模型误差的平均值，其中模型误差是预测标签值和正确标签值之间的绝对差距。 为测试数据集的每个记录计算此预测误差。 最后，计算所有已记录的绝对误差的平均值。| **越接近 0.00 表示质量越好。** 平均绝对误差使用与待测量数据相同的比例（未规范化为特定范围）。 绝对值损失、平方损失和 RMS 损失只能用于同一数据集的模型或具有类似标签值分布的数据集之间的比较。 |
| **平方损失** |  [平方损失](https://en.wikipedia.org/wiki/Mean_squared_error)或均方误差 (MSE)（也称为“均方差 (MSD)”）通过计算从点到回归线的距离（这些距离为误差 E）并对它们进行求平方，可以告诉你回归线与一组测试数据值的距离。 求平方可赋予较大的差异更大的权重。 | 它始终是非负值，并且 **越接近 0.00 的值越好**。 可能会无法获得非常小的均方误差值，具体取决于数据。|
| **RMS 损失** |  [RMS 损失](https://en.wikipedia.org/wiki/Root-mean-square_deviation)或“均方根误差 (RMSE)”又称为“均方根偏差 (RMSD)”），测量模型预测的值与在建模环境中观测到的值之间的差异。 RMS 损失是平方损失的平方根，其具有与标签相同的单位，类似于绝对值损失，但赋予了较大的差异更大的权重。 均方根误差通常用于气候学、预测和回归分析，以验证试验结果。 | 它始终是非负值，并且 **越接近 0.00 的值越好**。 RMSD 是准确性度量值，用于比较特定数据集的不同模型的预测误差，而不用于比较数据集之间的预测误差，因为它与比例相关。|

有关回归指标的更多详细信息，请阅读以下文章：

- [Regression Analysis:How Do I Interpret R-squared and Assess the Goodness-of-Fit?](https://blog.minitab.com/blog/adventures-in-statistics-2/regression-analysis-how-do-i-interpret-r-squared-and-assess-the-goodness-of-fit)（回归分析：我如何解释 R 平方并评估拟合优度？）
- [How To Interpret R-squared in Regression Analysis](https://statisticsbyjim.com/regression/interpret-r-squared-regression)（如何在回归分析中解释 R 平方）
- [R-Squared Definition](https://www.investopedia.com/terms/r/r-squared.asp)（R 平方定义）
- [Mean Squared Error Definition](https://www.statisticshowto.datasciencecentral.com/mean-squared-error/)（均方误差定义）
- [What are Mean Squared Error and Root Mean Squared Error?](https://www.vernier.com/til/1014/)（什么是均方误差和均方根误差？）

## <a name="evaluation-metrics-for-clustering"></a>聚类分析的评估指标

| 指标   |      说明      |  期望 |
|----------|-----------------------|-----------|
|**平均距离**|数据点与其分配的群集中心之间的平均距离。 平均距离测量数据点到群集中心的距离。 这是对群集“紧密型”的度量。|值越接近 0 越好。 平均距离越接近零，聚集的数据越多。 但请注意，如果增加了群集的数量，则此指标将减少，在极端情况下（其中每个不同的数据点是其自身的群集），它将等于零。
|**Davies Bouldin 索引**|群集内距离与群集间距离之间的平均比率。 群集越紧密，群集之间的距离越远，此值就越低。|值越接近 0 越好。 距离较远、分散程度较低的群集将获得更好的分数。|
|**规范化相互信息**|如果用于训练聚类分析模型的训练数据还附带地面真值标签（即监督式聚类分析），则可以使用此指标。 规范化相互信息指标用于测量是否向相同的群集分配类似的数据点，并将不同的数据点分配给不同群集。 规范化相互信息值介于 0 和 1 之间|值越接近 1 越好|

## <a name="evaluation-metrics-for-ranking"></a>排名评估指标

| 指标   |      说明      |  期望 |
|----------|-----------------------|-----------|
|**折扣累积增益**|折扣累积增益 (DCG) 用于衡量排名质量。 它派生自两个假设。 1：高相关性项在排名靠前时更有用。 2：有用性跟踪相关性，即相关性越高，项目越有用。 为排名中的特定位置计算折扣累积增益。 它使相关性评分除以排名索引的对数，直到达到目标位置。 它使用 $\sum_{i=0}^{p} \frac {rel_i} {\log_{e}{i+1}}$ 进行计算，关联等级作为地面真值标签提供给排名训练算法。 为排名表中的每个位置提供一个 DCG 值，因此获得名称“折扣累积增益。 |**值越大越好**|
|**规范化折扣累积增益**|对 DCG 进行规范化可以比较不同长度排名列表的指标|**值越接近 1 越好**|

## <a name="evaluation-metrics-for-anomaly-detection"></a>异常情况检测评估指标

| 指标   |      说明      |  期望 |
|----------|-----------------------|-----------|
|**ROC 曲线下面积**|接收方运算符曲线下面积测量模型将异常数据点和常用数据点相分离的程度。|**值越接近 1 越好**。 只有大于 0.5 的值才能证明模型的有效性。 0.5 或更低的值表明，该模型并不优于将输入随机分配到异常和常见类别|
|**假正计数的检测率**|假正计数的检测率是指测试集中正确识别的异常数与异常总数的比率，按每个假正进行索引。 也就是说，每个假正项的假正计数都有一个检测率值。|**值越接近 1 越好**。 如果没有假正，则此值为 1|
