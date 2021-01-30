---
description: sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)
title: sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 6794e073-0895-4507-aba3-c3545acc843f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fbc14748b9799b78e8e92a983f8b0f5eb409f1ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201885"
---
# <a name="sysfn_stmt_sql_handle_from_sql_stmt-transact-sql"></a>sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Obtiene el **stmt_sql_handle** para una [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción en el tipo de parametrización dado (simple o forzada). Esto le permite hacer referencia a las consultas almacenadas en el Almacén de consultas mediante su **stmt_sql_handle** cuando Conozca su texto.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sys.fn_stmt_sql_handle_from_sql_stmt   
(  
    'query_sql_text',   
    [ query_param_type   
) [;]  
```  
  
## <a name="arguments"></a>Argumentos  
 *query_sql_text*  
 Es el texto de la consulta en el almacén de consultas cuyo identificador desea. *query_sql_text* es de tipo **nvarchar (Max)** y no tiene ningún valor predeterminado.  
  
 *query_param_type*  
 Es el tipo de parámetro de la consulta. *query_param_type* es un **tinyint**. Los valores posibles son:  
  
-   NULL: el valor predeterminado es 0  
  
-   0 - Ninguno  
  
-   1: usuario  
  
-   2-simple  
  
-   3-forzado  
  
## <a name="columns-returned"></a>Columnas devueltas  
 En la tabla siguiente se enumeran las columnas que sys.fn_stmt_sql_handle_from_sql_stmt devuelve.  
  
|Nombre de la columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|**statement_sql_handle**|**varbinary (64)**|Identificador de SQL.|  
|**query_sql_text**|**nvarchar(max)**|Texto de la [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción.|  
|**query_parameterization_type**|**tinyint**|Tipo de parametrización de consulta.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso **Execute** en la base de datos y el permiso **Delete** en las vistas de catálogo del almacén de consultas.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se ejecuta una instrucción y, a continuación, `sys.fn_stmt_sql_handle_from_sql_stmt` se usa para devolver el identificador SQL de la instrucción.  
  
```  
SELECT * FROM sys.databases;   
SELECT * FROM sys.fn_stmt_sql_handle_from_sql_stmt('SELECT * FROM sys.databases', NULL);  
```  
  
 Utilice la función para poner en correlación Almacén de consultas datos con otras vistas de administración dinámica. En el ejemplo siguiente:  
  
```  
SELECT qt.query_text_id, q.query_id, qt.query_sql_text, qt.statement_sql_handle,  
q.context_settings_id, qs.statement_context_id   
FROM sys.query_store_query_text AS qt  
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_id  
CROSS APPLY sys.fn_stmt_sql_handle_from_sql_stmt (qt.query_sql_text, null) AS fn_handle_from_stmt  
JOIN sys.dm_exec_query_stats AS qs   
    ON fn_handle_from_stmt.statement_sql_handle = qs.statement_sql_handle;  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_query_store_force_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)   
 [sp_query_store_remove_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)   
 [sp_query_store_unforce_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)   
 [sp_query_store_reset_exec_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)   
 [sp_query_store_flush_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)   
 [sp_query_store_remove_query &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)   
 [Almacén de consultas vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [Supervisar el rendimiento mediante el Almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
  
  
