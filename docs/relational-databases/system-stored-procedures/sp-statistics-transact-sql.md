---
description: sp_statistics (Transact-SQL)
title: sp_statistics (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_statistics_TSQL
- sp_statistics
dev_langs:
- TSQL
helpviewer_keywords:
- sp_statistics
ms.assetid: 0bb6495f-258a-47ec-9f74-fd16671d23b8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1132a44770b70cae1906bc4ed9a45366616c08f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206480"
---
# <a name="sp_statistics-transact-sql"></a>sp_statistics (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una lista de todos los índices y estadísticas de una vista indizada o tabla especificada.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_statistics [ @table_name = ] 'table_name'    
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @index_name = ] 'index_name' ]   
     [ , [ @is_unique = ] 'is_unique' ]  
     [ , [ @accuracy = ] 'accuracy' ]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumentos  
`[ @table_name = ] 'table_name'` Especifica la tabla que se utiliza para devolver información del catálogo. *TABLE_NAME* es de **tipo sysname** y no tiene ningún valor predeterminado. No se admite la coincidencia de patrón de caracteres comodín.  
  
`[ @table_owner = ] 'owner'` Es el nombre del propietario de la tabla que se utiliza para devolver información del catálogo. *table_owner* es de **tipo sysname y su** valor predeterminado es NULL. No se admite la coincidencia de patrón de caracteres comodín. Si no se especifica *Owner* , se aplican las reglas predeterminadas de visibilidad de tabla del DBMS subyacente.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el usuario actual es propietario de una tabla en la que se especifica el nombre, se devuelven los índices de esa tabla. Si no se especifica *Owner* y el usuario actual no posee una tabla con el *nombre* especificado, este procedimiento busca una tabla con el *nombre* especificado que pertenezca al propietario de la base de datos. Si existe una, se devuelven los índices de esa tabla.  
  
`[ @table_qualifier = ] 'qualifier'` Es el nombre del calificador de tabla. el *calificador* es de **tipo sysname y su** valor predeterminado es NULL. Varios productos DBMS admiten nombres de tres partes para las tablas (_calificador_**.** _propietario_**.** _nombre_). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], este parámetro representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla.  
  
`[ @index_name = ] 'index_name'` Es el nombre del índice. *index_name* es de **tipo sysname y su** valor predeterminado es%. Se admite la coincidencia de patrón de caracteres comodín.  
  
`[ @is_unique = ] 'is_unique'` Indica si solo se devolverán índices únicos (si **Y**). *is_unique* es **Char (1)** y su valor predeterminado es **N**.  
  
`[ @accuracy = ] 'accuracy'` Es el nivel de cardinalidad y la precisión de la página de las estadísticas. la *precisión* es **Char (1)** y su valor predeterminado es **Q**. Especifique **E** para asegurarse de que las estadísticas se actualicen de modo que la cardinalidad y las páginas sean precisas.  
  
 El valor **E** (SQL_ENSURE) pide al controlador que recupere las estadísticas de forma incondicional.  
  
 El valor **Q** (SQL_QUICK) pide al controlador que recupere la cardinalidad y las páginas solo si están disponibles fácilmente desde el servidor. En este caso, el controlador no garantiza que los valores sean actuales. Las aplicaciones que están escritas en el estándar Open Group siempre adoptarán el comportamiento de SQL_QUICK de los controladores compatibles con ODBC 3x.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nombre del calificador de tabla. Esta columna puede ser NULL.|  
|**TABLE_OWNER**|**sysname**|Nombre del propietario de la tabla. Esta columna siempre devuelve un valor.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla. Esta columna siempre devuelve un valor.|  
|**NON_UNIQUE**|**smallint**|NOT NULL.<br /><br /> 0 = Único<br /><br /> 1 = No único|  
|**INDEX_QUALIFIER**|**sysname**|Nombre del propietario del índice. Algunos productos DBMS permiten crear índices a usuarios que no sean los propietarios de la tabla. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta columna es siempre igual que **TABLE_NAME**.|  
|**INDEX_NAME**|**sysname**|Es el nombre del índice. Esta columna siempre devuelve un valor.|  
|**TYPE**|**smallint**|Esta columna siempre devuelve un valor:<br /><br /> 0 = Estadísticas de una tabla<br /><br /> 1 = Clúster<br /><br /> 2 = Hash<br /><br /> 3 = no agrupado|  
|**SEQ_IN_INDEX**|**smallint**|Posición de la columna dentro del índice.|  
|**COLUMN_NAME**|**sysname**|Nombre de columna de cada columna del **TABLE_NAME** devuelto. Esta columna siempre devuelve un valor.|  
|**COLLATION**|**char(1)**|Orden utilizado en la intercalación. Puede ser:<br /><br /> A = Ascendente<br /><br /> D = Descendente<br /><br /> NULL = No aplicable|  
|**CARDINALIDAD**|**int**|Número de filas de la tabla o valores únicos del índice.|  
|**PÁGINAS**|**int**|Número de páginas para el almacenamiento del índice o tabla.|  
|**FILTER_CONDITION**|**varchar(128)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no devuelve ningún valor.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="remarks"></a>Observaciones  
 Los índices del conjunto de resultados aparecen en orden ascendente por las columnas **NON_UNIQUE**, **tipo**, **INDEX_NAME** y **SEQ_IN_INDEX**.  
  
 El tipo de índice clúster hace referencia a un índice en el que los datos de la tabla se almacenan en el orden del índice. Esto corresponde a los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] índices clúster.  
  
 El tipo de índice en hash acepta búsquedas de coincidencia exacta o de intervalos, pero las búsquedas de coincidencia de patrón no utilizan el índice.  
  
 **sp_statistics** es equivalente a **SQLStatistics** en ODBC. Los resultados devueltos se ordenan por **NON_UNIQUE**, **tipo**, **INDEX_QUALIFIER**, **INDEX_NAME** y **SEQ_IN_INDEX**. Para obtener más información, vea la referencia de la [API de ODBC](../../odbc/reference/syntax/odbc-reference.md).  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="example-sssdwfull-and-sspdw"></a>Ejemplo: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el siguiente ejemplo se devuelve información acerca de la `DimEmployee` tabla.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_statistics DimEmployee;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
