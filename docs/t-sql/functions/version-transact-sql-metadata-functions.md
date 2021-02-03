---
description: Versión - Funciones de metadatos de Transact SQL
title: VERSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 95a79b33-98f2-4929-a1a5-93b522a9e152
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: b96f059d38320b0c07e42f67125fc686ee7866df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159680"
---
# <a name="version---transact-sql-metadata-functions"></a>Versión - Funciones de metadatos de Transact SQL
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

 Devuelve la versión de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW_md](../../includes/sspdw-md.md)] en ejecución en el dispositivo.  
  
![Icono de vínculo a temas](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Azure Synapse Analytics and Parallel Data Warehouse  
VERSION ( )  
```  
  
## <a name="arguments"></a>Argumentos  
  
## <a name="general-remarks"></a>Notas generales  
Debe especificarse un nombre de tabla en una cláusula [FROM](../../t-sql/queries/from-transact-sql.md) para esta función para que devuelva resultados. Se devolverá una fila de resultados para cada fila del conjunto de resultados de la consulta; use [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md) para limitar el número de filas devueltas.  
  
## <a name="examples"></a>Ejemplos  
El ejemplo siguiente se devuelve el número de la versión.  
  
```sql
SELECT VERSION();  
```  
  
## <a name="see-also"></a>Consulte también 
[SESSION_ID (Transact-SQL)](../../t-sql/functions/session-id-transact-sql.md)  
[DB_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/db-name-transact-sql.md)  
  
  
