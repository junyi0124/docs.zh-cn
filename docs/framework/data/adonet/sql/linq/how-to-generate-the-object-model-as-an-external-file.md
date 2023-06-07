---
title: 如何：将对象模型作为外部文件生成
ms.date: 03/30/2017
ms.assetid: 2496fa06-3df4-4ecb-86c4-70a49ea08565
ms.openlocfilehash: 2442caec5400759ae2bfeca35f99ebd2ff52d011
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180759"
---
# <a name="how-to-generate-the-object-model-as-an-external-file"></a>如何：将对象模型作为外部文件生成

作为基于属性的映射的替代方法，可以使用 SQLMetal 命令行工具将您的对象模型生成为外部 XML 文件。 有关详细信息，请参阅 [SqlMetal.exe（代码生成工具）](../../../../tools/sqlmetal-exe-code-generation-tool.md)。 使用外部 XML 映射文件可以降低代码中的混乱程度。 您还可以通过修改该外部文件来更改行为，而无需重新编译应用程序的二进制文件。 有关详细信息，请参阅 [外部映射](external-mapping.md)。  
  
> [!NOTE]
> 对象关系设计器不支持生成外部映射文件。  
  
## <a name="example"></a>示例  

 下面的命令从 Northwind 示例数据库生成一个外部映射文件。  
  
```console  
sqlmetal /server:myserver /database:northwind /map:externalfile.xml  
```  
  
## <a name="example"></a>示例  

 下面的内容摘自一个外部映射文件，用于演示 Northwind 示例数据库中的 Customers 表的映射。 此摘录是通过使用 **/map** 选项执行 SQLMetal 而生成的。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<Database xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Name="northwnd">  
  <Table Name="Customers">  
    <Type Name=".Customer">  
      <Column Name="CustomerID" Member="CustomerID" Storage="_CustomerID" DbType="NChar(5) NOT NULL" CanBeNull="False" IsPrimaryKey="True" />  
      <Column Name="CompanyName" Member="CompanyName" Storage="_CompanyName" DbType="NVarChar(40) NOT NULL" CanBeNull="False" />  
      <Column Name="ContactName" Member="ContactName" Storage="_ContactName" DbType="NVarChar(30)" />  
      <Column Name="ContactTitle" Member="ContactTitle" Storage="_ContactTitle" DbType="NVarChar(30)" />  
      <Column Name="Address" Member="Address" Storage="_Address" DbType="NVarChar(60)" />  
      <Column Name="City" Member="City" Storage="_City" DbType="NVarChar(15)" />  
      <Column Name="Region" Member="Region" Storage="_Region" DbType="NVarChar(15)" />  
      <Column Name="PostalCode" Member="PostalCode" Storage="_PostalCode" DbType="NVarChar(10)" />  
      <Column Name="Country" Member="Country" Storage="_Country" DbType="NVarChar(15)" />  
      <Column Name="Phone" Member="Phone" Storage="_Phone" DbType="NVarChar(24)" />  
      <Column Name="Fax" Member="Fax" Storage="_Fax" DbType="NVarChar(24)" />  
      <Association Name="FK_CustomerCustomerDemo_Customers" Member="CustomerCustomerDemos" Storage="_CustomerCustomerDemos" ThisKey="CustomerID" OtherTable="CustomerCustomerDemo" OtherKey="CustomerID" DeleteRule="NO ACTION" />  
      <Association Name="FK_Orders_Customers" Member="Orders" Storage="_Orders" ThisKey="CustomerID" OtherTable="Orders" OtherKey="CustomerID" DeleteRule="NO ACTION" />  
    </Type>  
  </Table>  
</Database>  
```  
  
## <a name="see-also"></a>请参阅

- [创建对象模型](creating-the-object-model.md)
- [外部映射](external-mapping.md)
- [如何：在 Visual Basic 或 C# 中生成对象模型](how-to-generate-the-object-model-in-visual-basic-or-csharp.md)
