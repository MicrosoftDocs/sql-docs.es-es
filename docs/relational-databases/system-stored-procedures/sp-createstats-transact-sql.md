---
description: sp_createstats (Transact-SQL)
title: sp_createstats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_createstats_TSQL
- sp_createstats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_createstats
ms.assetid: 8204f6f2-5704-40a7-8d51-43fc832eeb54
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a0fb35bd43125370dc4123425cc919be3033951f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205212"
---
# <a name="sp_createstats-transact-sql"></a>sp_createstats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Llama a la instrucción [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) para crear estadísticas de columna única en columnas que aún no son la primera columna de un objeto de estadísticas. La creación de estadísticas de columna única aumenta el número de histogramas, lo que puede mejorar las estimaciones de cardinalidad, los planes de consulta y el rendimiento de las consultas. La primera columna de un objeto de estadísticas tiene un histograma; otras columnas no tienen un histograma.  
  
 sp_createstats es útil para aplicaciones como las pruebas comparativas cuando los tiempos de ejecución de la consulta resultan críticos y no se puede esperar a que el optimizador de consultas genere estadísticas de columna única. En la mayoría de los casos, no es necesario utilizar sp_createstats; el optimizador de consultas genera estadísticas de columna única según sea necesario para mejorar los planes de consulta cuando la opción de **AUTO_CREATE_STATISTICS** está activada.  
  
 Para obtener más información sobre las estadísticas, vea [Estadísticas](../../relational-databases/statistics/statistics.md). Para obtener más información sobre cómo generar estadísticas de columna única, vea la opción **AUTO_CREATE_STATISTICS** en [Opciones de ALTER database Set &#40;&#41;de Transact-SQL](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_createstats   
    [   [ @indexonly =   ] { 'indexonly'   | 'NO' } ]   
    [ , [ @fullscan =    ] { 'fullscan'    | 'NO' } ]   
    [ , [ @norecompute = ] { 'norecompute' | 'NO' } ]  
    [ , [ @incremental = ] { 'incremental' | 'NO' } ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @indexonly = ] 'indexonly'` Crea estadísticas solo en las columnas que se encuentran en un índice existente y que no son la primera columna de cualquier definición de índice. **indexonly** es **Char (9)**. El valor predeterminado es NO.  
  
`[ @fullscan = ] 'fullscan'` Usa la instrucción [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) con la opción **FullScan** . **FullScan** es **Char (9)**.  El valor predeterminado es NO.  
  
`[ @norecompute = ] 'norecompute'` Usa la instrucción [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) con la opción **NORECOMPUTE** . **NORECOMPUTE** es **Char (12)**.  El valor predeterminado es NO.  
  
`[ @incremental = ] 'incremental'` Usa la instrucción [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) con la opción **incremental = on** . **Incremental** es **Char (12)**.  El valor predeterminado es NO.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Cada nuevo objeto de estadísticas tiene el mismo nombre que la columna en la que se creó.  
  
## <a name="remarks"></a>Observaciones  
 sp_createstats no crea ni actualiza las estadísticas de las columnas que son la primera columna de un objeto de estadísticas existente.  Esto incluye la primera columna de estadísticas creadas para índices, columnas con estadísticas de columna única generadas con AUTO_CREATE_STATISTICS opción y la primera columna de estadísticas creadas con la instrucción CREATE STATISTICs. sp_createstats no crea estadísticas en las primeras columnas de los índices deshabilitados a menos que se use esa columna en otro índice habilitado. sp_createstats no crea estadísticas en las tablas con un índice clúster deshabilitado.  
  
 Cuando la tabla contiene un conjunto de columnas, sp_createstats no crea automáticamente estadísticas en columnas dispersas. Para obtener más información sobre los conjuntos de columnas y las columnas dispersas, vea [usar conjuntos](../../relational-databases/tables/use-column-sets.md) de columnas y [usar columnas dispersas](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere pertenencia al rol fijo de base de datos db_owner.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-create-single-column-statistics-on-all-eligible-columns"></a>A. Crear estadísticas de columna única en todas las columnas coincidentes  
 En el siguiente ejemplo, se crean estadísticas de columna única en todas las columnas coincidentes de la base de datos actual.  
  
```  
EXEC sp_createstats;  
GO  
```  
  
### <a name="b-create-single-column-statistics-on-all-eligible-index-columns"></a>B. Crear estadísticas de columna única en todas las columnas de índice coincidentes  
 En el ejemplo siguiente, se crean estadísticas de columna única en todas las columnas coincidentes que ya se encuentran en un índice y que no son la primera columna del índice.  
  
```  
EXEC sp_createstats 'indexonly';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Utilizar las estadísticas para mejorar el rendimiento de las consultas](../../relational-databases/statistics/statistics.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
