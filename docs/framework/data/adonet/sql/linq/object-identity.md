---
title: 对象标识
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c788f2f9-65cc-4455-9907-e8388a268e00
ms.openlocfilehash: 1a1617b4fb15a6adf94c0241c3ba577308c51a8b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169422"
---
# <a name="object-identity"></a>对象标识

运行库中的对象具有唯一标识。 引用同一对象的两个变量实际上是引用此对象的同一实例。 因为这一事实，您通过一个变量做出更改后，立即就可以通过另一个变量看到这些更改。  
  
 关系数据库表中的行不具有唯一标识。 由于每一行都具有唯一的主键，因此任何两行都不会共用同一键值。 但是，这一事实仅限于数据库表的内容。  
  
 实际上，通常都是将数据从数据库中提取出来放入另一层中，应用程序在该层对数据进行处理。 这就是 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 支持的模型。 将数据作为行从数据库中提取出来时，您不期望表示相同数据的两行实际上对应于相同的行实例。 如果您查询特定客户两次，您将获得两行数据。 每一行包含相同的信息。  
  
 对于对象，您的期望则大不一样。 您期望在您反复向 <xref:System.Data.Linq.DataContext> 索取相同的信息时，它实际上会为您提供同一对象实例。 您之所以期望这种行为，是因为对象对您的应用程序而言有着特殊的含义，您期望它们的行为像实物一样。 您将它们设计为层次结构或关系图。 您希望像检索实物一样检索它们，而不希望仅仅因为您多次索要同一内容而收到大量的复制实例。  
  
 在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中，<xref:System.Data.Linq.DataContext> 管理对象标识。 只要您从数据库中检索新行，该行就会由其主键记录到标识表中，并且会创建一个新的对象。 只要您检索该行，就会将原始对象实例传递回应用程序。 通过这种方式，<xref:System.Data.Linq.DataContext> 将数据库看到的标识（即主键）的概念转换成相应语言看到的标识（即实例）的概念。 应用程序只看到处于第一次检索时的状态的对象。 新数据如果不同，则会被丢弃。 有关详细信息，请参阅 [从标识缓存中检索对象](retrieving-objects-from-the-identity-cache.md)。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 使用此方法来管理本地对象的完整性，以支持开放式更新。 由于在最初创建对象后唯一发生的更改是由应用程序做出的，因此应用程序的意向是很明确的。 如果在中间阶段外部某一方做了更改，则在调用 `SubmitChanges()` 时会识别出这些更改。  
  
> [!NOTE]
> 如果查询请求的对象易被识别为已检索到的对象，则不会执行查询。 标识表用作以前检索到的所有对象的缓存。  
  
## <a name="examples"></a>示例  
  
### <a name="object-caching-example-1"></a>对象缓存示例 1  

 在此示例中，如果您执行同一查询两次，则您每次都会收到对内存中同一对象的引用。  
  
 [!code-csharp[DLinqObjectIdentity#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqObjectIdentity/cs/Program.cs#1)]
 [!code-vb[DLinqObjectIdentity#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqObjectIdentity/vb/Module1.vb#1)]  
  
### <a name="object-caching-example-2"></a>对象缓存示例 2  

 在此示例中，如果您执行返回数据库中同一行的不同查询，则您每次都会收到对内存中同一对象的引用。  
  
 [!code-csharp[DLinqObjectIdentity#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqObjectIdentity/cs/Program.cs#2)]
 [!code-vb[DLinqObjectIdentity#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqObjectIdentity/vb/Module1.vb#2)]  
  
## <a name="see-also"></a>请参阅

- [背景信息](background-information.md)
