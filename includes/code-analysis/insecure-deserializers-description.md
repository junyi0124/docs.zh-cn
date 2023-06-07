---
author: dotpaul
ms.author: paulming
ms.date: 04/05/2019
ms.topic: include
ms.openlocfilehash: 9235f1bcda7529b71aef542d3a49da08a9d4b59e
ms.sourcegitcommit: c04535ad05e374fb269fcfc6509217755fbc0d54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "96590079"
---
反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。 例如，对不安全的反序列化程序的攻击可以在基础操作系统上执行命令，通过网络进行通信，或删除文件。
