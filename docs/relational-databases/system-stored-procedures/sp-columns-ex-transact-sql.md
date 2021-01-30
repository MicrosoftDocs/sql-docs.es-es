---
description: sp_columns_ex (Transact-SQL)
title: sp_columns_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_columns_ex
- sp_columns_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_columns_ex
ms.assetid: c12ef6df-58c6-4391-bbbf-683ea874bd81
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6696187b8f2b85b8a7ed179bfe406490e2c650aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174483"
---
# <a name="sp_columns_ex-transact-sql"></a>sp_columns_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la información de la columna, una fila por columna, de las tablas del servidor vinculado especificado. **sp_columns_ex** devuelve información de columna únicamente para la columna específica si se especifica *Column* .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_columns_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @column_name = ] 'column' ]   
     [ , [ @ODBCVer = ] 'ODBCVer' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @table_server = ] 'table_server'` Es el nombre del servidor vinculado cuya información de columna se va a devolver. *table_server* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @table_name = ] 'table_name'` Es el nombre de la tabla para la que se va a devolver información de columna. *TABLE_NAME* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_schema = ] 'table_schema'` Es el nombre de esquema de la tabla para la que se va a devolver información de columna. *TABLE_SCHEMA* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @table_catalog = ] 'table_catalog'` Es el nombre de catálogo de la tabla para la que se va a devolver información de columna. *TABLE_CATALOG* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @column_name = ] 'column'` Es el nombre de la columna de base de datos para la que se va a proporcionar información. la *columna* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @ODBCVer = ] 'ODBCVer'` Es la versión de ODBC que se está utilizando. *ODBCVer* es de **tipo int** y su valor predeterminado es 2. Esto indica ODBC Versión 2. Los valores válidos son 2 ó 3. Para obtener información acerca de las diferencias de comportamiento entre las versiones 2 y 3, vea la especificación ODBC SQLColumns.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nombre del calificador de la tabla o vista. Varios productos DBMS admiten nombres de tres partes para las tablas (_calificador_**.** _propietario_**.** _nombre_). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esta columna representa el nombre de base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla. Este campo puede ser NULL.|  
|**TABLE_SCHEM**|**sysname**|Nombre del propietario de la tabla o vista. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre del usuario de la base de datos que creó la tabla. Este campo siempre devuelve un valor.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla o vista. Este campo siempre devuelve un valor.|  
|**COLUMN_NAME**|**sysname**|Nombre de columna para cada columna del **TABLE_NAME** devuelto. Este campo siempre devuelve un valor.|  
|**DATA_TYPE**|**smallint**|Valor de tipo entero correspondiente a indicadores de tipo ODBC. Si se trata de un tipo de datos que no se puede asignar a un tipo de ODBC, el valor es NULL. El nombre del tipo de datos nativo se devuelve en la columna **TYPE_NAME** .|  
|**TYPE_NAME**|**VARCHAR (** 13 **)**|Cadena que representa un tipo de datos. El DBMS subyacente presenta este nombre del tipo de datos.|  
|**COLUMN_SIZE**|**int**|Número de dígitos significativos. El valor devuelto para la columna **Precision** está en base 10.|  
|**BUFFER_LENGTH**|**int**|Tamaño de transferencia de los datos.1|  
|**DECIMAL_DIGITS**|**smallint**|Número de dígitos a la derecha del separador decimal.|  
|**NUM_PREC_RADIX**|**smallint**|Base para los tipos de datos numéricos.|  
|**ACEPTA valores NULL**|**smallint**|Especifica la nulabilidad.<br /><br /> 1 = Se admiten valores NULL.<br /><br /> 0 = No se admiten valores NULL.|  
|**COMENTARIOS**|**VARCHAR (** 254 **)**|Este campo siempre devuelve NULL.|  
|**COLUMN_DEF**|**VARCHAR (** 254 **)**|Valor predeterminado de la columna.|  
|**SQL_DATA_TYPE**|**smallint**|Valor del tipo de datos SQL tal como aparece en el campo TYPE del descriptor. Esta columna es igual que la columna **data_type** , excepto los tipos de datos de **intervalo** **DateTime** y SQL-92. Esta columna siempre devuelve un valor.|  
|**SQL_DATETIME_SUB**|**smallint**|Código de subtipo para los tipos de datos de **intervalo** **DateTime** y SQL-92. Para otros tipos de datos, esta columna devuelve NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Longitud máxima en bytes de una columna de tipos de datos de caracteres o enteros. Para los demás tipos de datos, esta columna devuelve NULL.|  
|**ORDINAL_POSITION**|**int**|Posición ordinal de la columna en la tabla. La primera columna de la tabla es 1. Esta columna siempre devuelve un valor.|  
|**IS_NULLABLE**|**VARCHAR (** 254 **)**|Nulabilidad de la columna de la tabla. Se siguen las normas ISO para determinar la nulabilidad. Un DBMS que cumpla la norma ISO SQL no puede devolver una cadena vacía.<br /><br /> YES = La columna puede incluir valores NULL.<br /><br /> NO = La columna no puede incluir valores NULL.<br /><br /> Esta columna devuelve una cadena de longitud cero si no se conoce la nulabilidad.<br /><br /> El valor devuelto para esta columna es diferente del valor devuelto para la columna que **acepta valores NULL** .|  
|**SS_DATA_TYPE**|**tinyint**|Tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizado por los procedimientos almacenados extendidos.|  
  
 Para obtener más información, vea la documentación de Microsoft ODBC.  
  
## <a name="remarks"></a>Observaciones  
- **sp_columns_ex** se ejecuta consultando el conjunto de filas Columns de la interfaz **IDBSchemaRowset** del proveedor de OLE DB correspondiente a *table_server*. Los parámetros *TABLE_NAME*, *TABLE_SCHEMA*, *TABLE_CATALOG* y *Column* se pasan a esta interfaz para restringir las filas devueltas.  
  
- **sp_columns_ex** devuelve un conjunto de resultados vacío si el proveedor de OLE DB del servidor vinculado especificado no admite el conjunto de filas Columns de la interfaz **IDBSchemaRowset** .  
  
- **sp_columns_ex** sigue los requisitos de los identificadores delimitados. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
 El siguiente ejemplo devuelve el tipo de datos de la columna `JobTitle` de la tabla `HumanResources.Employee` en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] del servidor vinculado `Seattle1`.  
  
```  
EXEC sp_columns_ex 'Seattle1',   
   'Employee',   
   'HumanResources',   
   'AdventureWorks2012',   
   'JobTitle';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_catalogs &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_foreignkeys &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_indexes &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_primarykeys &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-primarykeys-transact-sql.md)   
 [sp_tables_ex &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
