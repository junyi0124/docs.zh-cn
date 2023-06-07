---
title: Infer.NET 游戏匹配应用 - 概率性编程
description: 了解如何使用概率性编程和 Infer.NET 创建基于简化版 TrueSkill 的游戏匹配列表应用。
ms.date: 01/30/2020
ms.custom: mvc,how-to
ms.openlocfilehash: 8e489d61c5e6cca53ba12b13fddd0b73c7f85ef9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "77092598"
---
# <a name="create-a-game-match-up-list-app-with-infernet-and-probabilistic-programming"></a>使用 Infer.NET 和概率性编程创建游戏匹配列表

此操作指南介绍了如何使用 Infer.NET 进行概率性编程。 概率性编程是一种将自定义模型表示为计算机程序的机器学习方法。 借助它可以在模型中包含专业知识，使机器学习系统更易理解。 它还支持在线推断，即在新数据到达时进行学习的过程。 Azure、Xbox 及必应中的多种 Microsoft 产品均使用了 Infer.NET。

## <a name="what-is-probabilistic-programming"></a>什么是概率性编程？

借助概率性编程，可创建真实过程的统计模型。

## <a name="prerequisites"></a>系统必备

- 本地开发环境设置

  此操作指南要求有一台可用于进行开发的计算机。 .NET 教程 [Hello World 10 分钟入门](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro)介绍了如何在 macOS、Windows 或 Linux 上设置本地开发环境。

## <a name="create-your-app"></a>创建应用

1. 打开一个新的命令提示符，并运行下面的命令：

```dotnetcli
dotnet new console -o myApp
cd myApp
```

`dotnet` 命令将创建 `console` 类型的 `new` 应用程序。 `-o` 参数将创建名称为 `myApp` 的目录，会在其中存储应用并填充所需的文件。 `cd myApp` 命令会将你转到新创建的应用目录。

## <a name="install-infernet-package"></a>安装 Infer.NET 包

需安装 `Microsoft.ML.Probabilistic.Compiler` 包才能使用 Infer.NET。 在命令提示符中运行下面的命令：

```dotnetcli
dotnet add package Microsoft.ML.Probabilistic.Compiler
```

## <a name="design-your-model"></a>设计模型

该示例采用在办公室中进行的乒乓球或桌上足球比赛。 你具有参赛者的信息和每场比赛的结果。  你想要通过此数据推断玩家的实力。 假设每位玩家的潜在实力呈正态分布，且他们在给定比赛中的表现是此实力受干扰后的状态。 此数据会将胜者的表现约束在优于败者表现的水平上。 这是常见 [TrueSkill](https://www.microsoft.com/research/project/trueskill-ranking-system/) 模型的简化版，此模型也支持团队、平局及其他扩展项。 热销的 Halo 和 Gears of War 游戏中的比赛安排使用了此模型的[高级版](https://www.microsoft.com/research/publication/trueskill-2-improved-bayesian-skill-rating-system/)。

需要列出所推断玩家的实力及其方差（实力不确定性的度量值）。

*游戏结果示例数据*

游戏 |胜者 | 败者
---------|----------|---------
 1 | 玩家 0 | 玩家 1
 2 | 玩家 0 | 玩家 3
 3 | 玩家 0 | 玩家 4
 4 | 玩家 1 | 玩家 2
 5 | 玩家 3 | 玩家 1
 6 | 玩家 4 | 玩家 2

仔细观察示例数据会发现玩家 3 和玩家 4 分别输赢过一次。 我们来看下使用概率性编程时的排名。 还会注意到有一位玩家 0，因为对于开发人员而言，甚至 Office 匹配列表都是从零开始的。

## <a name="write-some-code"></a>编写一些代码

设计模型后，就可以使用 Infer.NET 建模 API 将其表示为概率性程序。 在常用的文本编辑器中，打开 `Program.cs`，并将其所有内容替换为以下代码：

```csharp
namespace myApp

{
    using System;
    using System.Linq;
    using Microsoft.ML.Probabilistic;
    using Microsoft.ML.Probabilistic.Distributions;
    using Microsoft.ML.Probabilistic.Models;

    class Program
    {

        static void Main(string[] args)
        {
            // The winner and loser in each of 6 samples games
            var winnerData = new[] { 0, 0, 0, 1, 3, 4 };
            var loserData = new[] { 1, 3, 4, 2, 1, 2 };

            // Define the statistical model as a probabilistic program
            var game = new Range(winnerData.Length);
            var player = new Range(winnerData.Concat(loserData).Max() + 1);
            var playerSkills = Variable.Array<double>(player);
            playerSkills[player] = Variable.GaussianFromMeanAndVariance(6, 9).ForEach(player);

            var winners = Variable.Array<int>(game);
            var losers = Variable.Array<int>(game);

            using (Variable.ForEach(game))
            {
                // The player performance is a noisy version of their skill
                var winnerPerformance = Variable.GaussianFromMeanAndVariance(playerSkills[winners[game]], 1.0);
                var loserPerformance = Variable.GaussianFromMeanAndVariance(playerSkills[losers[game]], 1.0);

                // The winner performed better in this game
                Variable.ConstrainTrue(winnerPerformance > loserPerformance);
            }

            // Attach the data to the model
            winners.ObservedValue = winnerData;
            losers.ObservedValue = loserData;

            // Run inference
            var inferenceEngine = new InferenceEngine();
            var inferredSkills = inferenceEngine.Infer<Gaussian[]>(playerSkills);

            // The inferred skills are uncertain, which is captured in their variance
            var orderedPlayerSkills = inferredSkills
               .Select((s, i) => new { Player = i, Skill = s })
               .OrderByDescending(ps => ps.Skill.GetMean());

            foreach (var playerSkill in orderedPlayerSkills)
            {
                Console.WriteLine($"Player {playerSkill.Player} skill: {playerSkill.Skill}");
            }
        }
    }
}
```

## <a name="run-your-app"></a>运行你的应用

在命令提示符中运行下面的命令：

```dotnetcli
dotnet run
```

## <a name="results"></a>结果

结果应如下所示：

```console
Compiling model...done.
Iterating:
.........|.........|.........|.........|.........| 50
Player 0 skill: Gaussian(9.517, 3.926)
Player 3 skill: Gaussian(6.834, 3.892)
Player 4 skill: Gaussian(6.054, 4.731)
Player 1 skill: Gaussian(4.955, 3.503)
Player 2 skill: Gaussian(2.639, 4.288)
```

请注意，在结果中根据我们的模型玩家 3 排名略高于玩家 4。 因为玩家 3 战胜了玩家 1 的意义大于玩家 4 战胜玩家 2 的意义 – 请注意玩家 1 打败了玩家 2. 玩家 0 是总冠军！

## <a name="keep-learning"></a>继续学习

设计统计模型本身就是一种技能。 Microsoft Research Cambridge 团队曾编写过一本[免费的在线书籍](http://mbmlbook.com/)，其中简要介绍了此文。 此书的第 3 章详细介绍了 TrueSkill 模型。 只要脑海中已有模型，即可使用 Infer.NET 网站上的[大量文档](https://dotnet.github.io/infer/)将其转换为代码。

## <a name="next-steps"></a>后续步骤

查看 Infer.NET GitHub 存储库以继续学习，并找到更多示例。
> [!div class="nextstepaction"]
> [dotnet/infer GitHub 存储库](https://github.com/dotnet/infer)
