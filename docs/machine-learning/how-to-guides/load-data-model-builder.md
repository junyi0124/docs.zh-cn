---
title: 为模型生成器加载培训数据
description: 了解如何从 SQL Server 数据库或文件加载培训数据，以在 ML.NET 的众多模型生成器方案中使用。
ms.date: 10/29/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to, mlnet-tooling
ms.openlocfilehash: 64e366b3c66427ccd2810324abeb880f6cb9ebc1
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602201"
---
# <a name="load-training-data-into-model-builder"></a>将培训数据加载到模型生成器

了解如何从文件或 SQL Server 数据库加载培训数据集，以 ML.NET 的众多模型生成器方案中使用。 模型生成器方案可以将 SQL Server 数据库、图像文件和 CSV 或 TSV 文件格式用作培训数据。

## <a name="training-dataset-limitations-in-model-builder"></a>模型生成器的培训数据集限制

模型生成器对于你可用于培训模型的数据的数量和类型有如下限制：

- SQL Server 数据：100,000 行
- CSV 和 TSV 文件：无大小限制
- 图像：仅 PNG 和JPG。

## <a name="model-builder-scenarios"></a>模型生成器方案

模型生成器可帮助你为以下机器学习方案创建模型：

- 情绪分析（二元分类）：将文本数据划分为两个类别。
- 问题分类（多类分类）：将文本数据划分为 3 个或更多个类别。
- 价格预测（回归）：预测一个数值。
- 图像分类（深度学习）：根据特征分类图像。
- 自定义方案：利用回归、分类和其他任务，通过数据生成自定义方案。

本文介绍了使用文本或数值数据的分类和回归方案，以及图像分类方案。

## <a name="load-text-or-numeric-data-from-a-file"></a>从文件加载文本或数值数据

你可以将文件中的文本或数值数据加载到模型生成器中。 它接受逗号分隔 (CSV) 或制表符分隔 (TSV) 的文件格式。

1. 在模型生成器的数据步骤中，从数据源下拉列表中选择“文件”  。
2. 选择“选择文件”文本框旁边的按钮，并使用文件资源管理器浏览并选择数据文件。 
3. 在“要预测的列(标签)”下拉列表中选择一个类别  。
4. 在“输入列(特性)”下拉列表中，确认已选择要包括的数据列。 

你已经为模型生成器设置了数据源文件。 选择“培训”  链接，转到模型生成器中的下一步。

## <a name="load-data-from-a-sql-server-database"></a>从 SQL Server 数据库加载数据

模型生成器支持从本地和远程 SQL Server 数据库加载数据。

如需将 SQL Server 数据库中的数据加载到模型生成器，请执行下列步骤：

1. 在模型生成器的数据步骤中，从数据源下拉列表中选择“SQL Server”  。
1. 选择“连接到 SQL Server 数据库”文本框旁的按钮。 
    1. 在“选择数据”对话框中，选择“Microsoft SQL Server 数据库文件”   。
    1. 取消选中“始终使用此选择”复选框，然后选择“继续”  
    1. 在“连接属性”对话框中，选择“浏览”，然后选择已下载的 .MDF 文件。  
    1. 选择“确定” 
1. 从“表名称”下拉列表选择数据集名称。 
1. 从“要预测的列(标签)”下拉列表中，选择想要预测的数据类别。 
1. 在“输入列(特性)”下拉列表中，确认已选择要包括的列。 

你已经为模型生成器设置了数据源文件。 选择“培训”  链接，转到模型生成器中的下一步。

## <a name="set-up-image-data-files"></a>设置图像数据文件

模型生成器要求图像数据为 JPG 或 PNG 文件，并且整合在与分类类别对应的文件夹中。

若要将图像加载到模型生成器，请提供指向单个顶级目录的路径：

- 此顶级目录包含一个要预测的各个类别的子文件夹。
- 每个子文件夹包含属于它的类别的图像文件。

在下面所示的文件夹结构中，顶级目录为 flower_photos。  有 5 个子目录，它们对应要预测的类别：菊花、蒲公英、玫瑰、向日葵和郁金香。 每个子目录包含属于其各自类别的图像。

```text
\---flower_photos
    +---daisy
    |       100080576_f52e8ee070_n.jpg
    |       102841525_bd6628ae3c.jpg
    |       105806915_a9c13e2106_n.jpg
    |
    +---dandelion
    |       10443973_aeb97513fc_m.jpg
    |       10683189_bd6e371b97.jpg
    |       10919961_0af657c4e8.jpg
    |
    +---roses
    |       102501987_3cdb8e5394_n.jpg
    |       110472418_87b6a3aa98_m.jpg
    |       118974357_0faa23cce9_n.jpg
    |
    +---sunflowers
    |       127192624_afa3d9cb84.jpg
    |       145303599_2627e23815_n.jpg
    |       147804446_ef9244c8ce_m.jpg
    |
    \---tulips
            100930342_92e8746431_n.jpg
            107693873_86021ac4ea_n.jpg
            10791227_7168491604.jpg
```

## <a name="next-steps"></a>后续步骤

按照以下教程使用模型生成器生成机器学习应用：

- [使用回归法预测价格](../tutorials/predict-prices-with-model-builder.md)
- [使用二元分类分析 Web 应用程序中的情绪](../tutorials/sentiment-analysis-model-builder.md)

若使用代码培训模型，请[了解如何使用 ML.NET API 加载数据](load-data-ml-net.md)。
