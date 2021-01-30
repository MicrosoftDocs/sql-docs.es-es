---
description: sp_columns (Transact-SQL)
title: sp_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_columns_TSQL
- sp_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sp_columns
ms.assetid: 2dec79cf-2baf-4c0f-8cbb-afb1a8654e1e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 32f865969933b07d2021a96b0af39f86d7f47893
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200813"
---
# <a name="sp_columns-transact-sql"></a>sp_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve la información de columna para los objetos especificados, que se pueden consultar en el entorno actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_columns [ @table_name = ] object  
     [ , [ @table_owner = ] owner ]   
     [ , [ @table_qualifier = ] qualifier ]   
     [ , [ @column_name = ] column ]   
     [ , [ @ODBCVer = ] ODBCVer ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ \@table_name = ] object` Es el nombre del objeto que se utiliza para devolver información del catálogo. el *objeto* puede ser una tabla, una vista u otro objeto que tenga columnas como funciones con valores de tabla. el *objeto* es de tipo **nvarchar (384)** y no tiene ningún valor predeterminado. Se admite la coincidencia de patrón de caracteres comodín.  
  
`[ \@table_owner = ] owner` Es el propietario del objeto que se utiliza para devolver información del catálogo. *Owner* es **nvarchar (384)** y su valor predeterminado es NULL. Se admite la coincidencia de patrón de caracteres comodín. Si no se especifica *Owner* , se aplican las reglas de visibilidad de objeto predeterminadas del DBMS subyacente.  
  
 Si el usuario actual posee un objeto con el nombre especificado, se devuelven las columnas de ese objeto. Si no se especifica *Owner* y el usuario actual no posee un objeto con el *objeto* especificado, **sp_columns** busca un objeto con el *objeto* especificado que pertenezca al propietario de la base de datos. Si existe uno, se devuelven las columnas de ese objeto.  
  
`[ \@table_qualifier = ] qualifier` Es el nombre del calificador de objeto. el *calificador* es de **tipo sysname y su** valor predeterminado es NULL. Varios productos DBMS admiten nombres de tres partes para objetos (_calificador_**.** _propietario_**.** _nombre_). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. En algunos productos, representa el nombre de servidor del entorno de base de datos del objeto.  
  
`[ \@column_name = ] column` Es una sola columna y se utiliza cuando solo se desea una columna de información del catálogo. la *columna* es de tipo **nvarchar (384)** y su valor predeterminado es NULL. Si no se especifica *Column* , se devuelven todas las columnas. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la *columna* representa el nombre de la columna tal y como aparece en la tabla **syscolumns** . Se admite la coincidencia de patrón de caracteres comodín. Para obtener una interoperabilidad máxima, el cliente de puerta de enlace solo debe dar por supuesta la coincidencia de patrón estándar de SQL-92 (caracteres comodín % y _).  
  
`[ \@ODBCVer = ] ODBCVer` Es la versión de ODBC que se está utilizando. *ODBCVer* es de **tipo int** y su valor predeterminado es 2. Esto indica ODBC Versión 2. Los valores válidos son 2 ó 3. Para conocer las diferencias de comportamiento entre las versiones 2 y 3, vea la especificación de **SQLColumns** de ODBC.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 El procedimiento almacenado del catálogo **sp_columns** es equivalente a **SQLColumns** en ODBC. Los resultados devueltos se ordenan por **TABLE_QUALIFIER**, **table_owner** y **TABLE_NAME**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nombre del calificador del objeto. Este campo puede ser NULL.|  
|**TABLE_OWNER**|**sysname**|Nombre del propietario del objeto. Este campo siempre devuelve un valor.|  
|**TABLE_NAME**|**sysname**|Nombre de objeto. Este campo siempre devuelve un valor.|  
|**COLUMN_NAME**|**sysname**|Nombre de columna para cada columna del **TABLE_NAME** devuelto. Este campo siempre devuelve un valor.|  
|**DATA_TYPE**|**smallint**|Código entero del tipo de datos ODBC. Si se trata de un tipo de datos que no se puede asignar a un tipo ODBC, se considera NULL. El nombre del tipo de datos nativo se devuelve en la columna **TYPE_NAME** .|  
|**TYPE_NAME**|**sysname**|Cadena que representa un tipo de datos. El DBMS subyacente presenta este nombre del tipo de datos.|  
|**PRECISION**|**int**|Número de dígitos significativos. El valor devuelto para la columna **Precision** está en base 10.|  
|**LENGTH**|**int**|Tamaño de la transferencia de los datos. <sup>1</sup>|  
|**ESCALA**|**smallint**|Número de dígitos a la derecha del separador decimal.|  
|**RADIX**|**smallint**|Base para tipos de datos numéricos.|  
|**ACEPTA valores NULL**|**smallint**|Especifica la nulabilidad.<br /><br /> 1 = Se admiten valores NULL.<br /><br /> 0 = No se admiten valores NULL.|  
|**COMENTARIOS**|**VARCHAR (254)**|Este campo siempre devuelve NULL.|  
|**COLUMN_DEF**|**nvarchar(4000)**|Valor predeterminado de la columna.|  
|**SQL_DATA_TYPE**|**smallint**|Valor del tipo de datos SQL tal como aparece en el campo TYPE del descriptor. Esta columna es igual que la columna **data_type** , excepto los tipos de datos de **intervalo** **DateTime** y SQL-92. Esta columna siempre devuelve un valor.|  
|**SQL_DATETIME_SUB**|**smallint**|Código de subtipo para los tipos de datos de **intervalo** **DateTime** y SQL-92. Para otros tipos de datos, esta columna devuelve NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Longitud máxima en bytes de una columna de tipos de datos de caracteres o enteros. Para los demás tipos de datos, esta columna devuelve NULL.|  
|**ORDINAL_POSITION**|**int**|Posición ordinal de la columna en el objeto. La primera columna del objeto es 1. Esta columna siempre devuelve un valor.|  
|**IS_NULLABLE**|**VARCHAR (254)**|Indica si la columna admite valores NULL en el objeto. Se siguen las normas ISO para determinar la nulabilidad. Un DBMS que cumpla la norma ISO SQL no puede devolver una cadena vacía.<br /><br /> YES = La columna puede incluir valores NULL.<br /><br /> NO = La columna no puede incluir valores NULL.<br /><br /> Esta columna devuelve una cadena de longitud cero si no se conoce la nulabilidad.<br /><br /> El valor devuelto para esta columna es diferente del valor devuelto para la columna que **acepta valores NULL** .|  
|**SS_DATA_TYPE**|**tinyint**|Tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizado por procedimientos almacenados extendidos. Para obtener más información, vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
  
 <sup>1</sup> para obtener más información, vea la documentación de Microsoft ODBC.  
  
## <a name="permissions"></a>Permisos  
 Requiere permisos SELECT y VIEW DEFINITION en el esquema.  
  
## <a name="remarks"></a>Observaciones  
 **sp_columns** sigue los requisitos de los identificadores delimitados. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve información de columna para una tabla especificada.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_columns @table_name = N'Department',  
   @table_owner = N'HumanResources';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el ejemplo siguiente se devuelve información de columna para una tabla especificada.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_columns @table_name = N'DimEmployee',  
   @table_owner = N'dbo';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_tables &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tables-transact-sql.md)   
 [Procedimientos almacenados de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  


