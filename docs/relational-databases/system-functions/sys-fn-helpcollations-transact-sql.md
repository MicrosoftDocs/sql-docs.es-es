---
description: sys.fn_helpcollations (Transact-SQL)
title: sys.fn_helpcollations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_helpcollations
- fn_helpcollations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_helpcollations function
- collations [SQL Server], supported
- fn_helpcollations function
ms.assetid: b5082e81-1fee-4e2c-b567-5412eaee41c1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016|| = azure-sqldw-latest ||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d5f1e211c9ec7f562839c92c9666bceb0e65bef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201993"
---
# <a name="sysfn_helpcollations-transact-sql"></a>sys.fn_helpcollations (Transact-SQL)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una lista de todas las intercalaciones admitidas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```
fn_helpcollations ()  
```  
  
## <a name="tables-returned"></a>Tablas devueltas

 **fn_HelpCollations** devuelve la siguiente información.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|Nombre|**sysname**|Nombre de intercalación estándar|  
|Descripción|**nvarchar(1000)**|Descripción de la intercalación|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite intercalaciones de Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también admite un número limitado (<80) de intercalaciones denominadas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intercalaciones, desarrolladas antes de las [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intercalaciones de Windows admitidas. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las intercalaciones todavía se admiten por compatibilidad con versiones anteriores, pero no deben usarse para nuevos trabajos de desarrollo. Para obtener más información sobre la intercalación de Windows, consulte [Nombre de intercalación de Windows &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md). Para obtener más información sobre las intercalaciones, vea [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Ejemplos

 En el ejemplo siguiente se devuelven todos los nombres de intercalación que comienzan por la letra `L` y que son intercalaciones de orden binario.

> [!Note]
> Las consultas de Azure Synapse Analytics en fn_helpcollations () se deben ejecutar en la base de datos maestra.  
  
```sql  
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name like 'L%' AND Description LIKE '% binary sort';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Name                   Description  
 -------------------    ------------------------------------  
 Lao_100_BIN            Lao-100, binary sort  
 Latin1_General_BIN     Latin1-General, binary sort  
 Latin1_General_100_BIN Latin1-General-100, binary sort  
 Latvian_BIN            Latvian, binary sort  
 Latvian_100_BIN        Latvian-100, binary sort  
 Lithuanian_BIN         Lithuanian, binary sort  
 Lithuanian_100_BIN     Lithuanian-100, binary sort  
  
 (7 row(s) affected)  
 ```
  
## <a name="see-also"></a>Consulte también

[COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md)   
[COLLATIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/collation-functions-collationproperty-transact-sql.md)  
[Compatibilidad con la intercalación de base de datos para Azure Synapse Analytics](https://azure.microsoft.com/blog/database-collation-support-for-azure-sql-data-warehouse-2)  
