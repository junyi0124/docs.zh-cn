---
title: 实例编码选项
ms.date: 03/30/2017
ms.assetid: 89e4b029-4f68-438c-8117-9b21fe094ef4
ms.openlocfilehash: 416394eb346a7c82385da32a89bd54179b255cd4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279891"
---
# <a name="instance-encoding-option"></a>实例编码选项

使用 SQL 工作流实例存储的 " **实例编码选项** " 属性，可以指定 sql 永久性提供程序是否应在将信息保存到持久性数据库中之前使用 GZip 算法压缩工作流实例状态信息。 此属性的允许值为 GZip 和 None。 默认值为 None。 以下列表对这两个选项进行了说明。  
  
1. **GZip**。 在将状态信息持久保存到持久性数据库中之前，持久性提供程序使用 GZip 算法对该状态信息进行编码。  
  
2. **无**。 在将状态信息保存到持久性数据库中之前，持久性提供程序不会对该状态信息进行编码。  
  
 使用 GZip 对工作流实例状态信息进行编码可减少 SQL 数据库中的内存消耗，并且还可减少网络消耗（如果数据库位于网络上的另一台计算机上，而不是位于运行工作流服务宿主的计算机上）。 一般的指导是，如果工作流实例的状态为小，则将 " **实例编码选项** " 属性设置为 " **无** "。
