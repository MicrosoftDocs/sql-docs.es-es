---
description: SET STATISTICS TIME (Transact-SQL)
title: SET STATISTICS TIME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_STATISTICS_TIME_TSQL
- SET STATISTICS TIME
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], statement processing
- time [SQL Server], statement processing statistics
- SET STATISTICS TIME statement
- STATISTICS TIME option
- statements [SQL Server], statistical information
- parsing [SQL Server], SET STATISTICS TIME statement
- compile times [SQL Server]
- execution processing time [SQL Server]
ms.assetid: eec2e1cd-a29d-4cf3-a271-be9d61506f15
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1c9c45ce9e387ab9ccfc7355d9b087b12bbacb56
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206921"
---
# <a name="set-statistics-time-transact-sql"></a>SET STATISTICS TIME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Muestra el número de milisegundos necesarios para analizar, compilar y ejecutar cada instrucción.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SET STATISTICS TIME { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Comentarios
 Cuando SET STATISTICS TIME es ON, se muestran las estadísticas de tiempo de una instrucción. Cuando es OFF no se muestran las estadísticas de tiempo.  
  
 La opción SET STATISTICS TIME se establece en tiempo de ejecución, no en tiempo de análisis.  
  
 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede calcular estadísticas precisas en el modo de fibra, que se activa al habilitar la opción de configuración **agrupación ligera**.  
  
 La columna **cpu** de la tabla **sysprocesses** solo se actualiza cuando se ejecuta una consulta con SET STATISTICS TIME ON. Si SET STATISTICS TIME es OFF, se devuelve **0**.  
  
 Las opciones ON y OFF también afectan a la columna CPU en la vista Información del proceso para la actividad actual, en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="permissions"></a>Permisos  
 Para utilizar SET STATISTICS TIME, los usuarios deben tener permisos apropiados para ejecutar la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]. El permiso SHOWPLAN no es necesario.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se muestran los tiempos de ejecución, análisis y compilación del servidor.  
  
```sql
USE AdventureWorks2012;  
GO         
SET STATISTICS TIME ON;  
GO  
SELECT ProductID, StartDate, EndDate, StandardCost   
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET STATISTICS TIME OFF;  
GO  
```  
  
 El conjunto de resultados es:  
  
```  
SQL Server parse and compile time:   
   CPU time = 0 ms, elapsed time = 1 ms.  
SQL Server parse and compile time:   
   CPU time = 0 ms, elapsed time = 1 ms.  
  
(269 row(s) affected)  
  
SQL Server Execution Times:  
   CPU time = 0 ms,  elapsed time = 2 ms.  
SQL Server parse and compile time:   
   CPU time = 0 ms, elapsed time = 1 ms.  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET STATISTICS IO &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-io-transact-sql.md)  
  
  
