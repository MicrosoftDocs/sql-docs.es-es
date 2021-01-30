---
description: sp_sproc_columns (Transact-SQL)
title: sp_sproc_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_sproc_columns
- sp_sproc_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_sproc_columns
ms.assetid: 62c18c21-35c5-4772-be0d-ffdcc19c97ab
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c6d7113657e036b0116e212c17df8ed4479cc1cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189320"
---
# <a name="sp_sproc_columns-transact-sql"></a>sp_sproc_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve la información de columna de un único procedimiento almacenado o función definida por el usuario en el entorno actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_sproc_columns [[@procedure_name = ] 'name']   
    [ , [@procedure_owner = ] 'owner']   
    [ , [@procedure_qualifier = ] 'qualifier']   
    [ , [@column_name = ] 'column_name']  
    [ , [@ODBCVer = ] 'ODBCVer']  
    [ , [@fUsePattern = ] 'fUsePattern']  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @procedure_name = ] 'name'` Es el nombre del procedimiento utilizado para devolver información del catálogo. *Name* es de tipo **nvarchar (** 390 **)** y su valor predeterminado es%, que significa todas las tablas de la base de datos actual. Se admite la coincidencia de patrón de caracteres comodín.  
  
`[ @procedure_owner = ] 'owner'` Es el nombre del propietario del procedimiento. *Owner* es **nvarchar (** 384 **)** y su valor predeterminado es NULL. Se admite la coincidencia de patrón de caracteres comodín. Si no se especifica *Owner* , se aplican las reglas predeterminadas de visibilidad de procedimientos del DBMS subyacente.  
  
 Si el usuario actual es propietario de un procedimiento que tiene el nombre especificado, se devuelve información sobre ese procedimiento. Si no se especifica Owner y el usuario actual no es el *propietario* de un procedimiento con el nombre especificado, **sp_sproc_columns** busca un procedimiento con el nombre especificado que pertenezca al propietario de la base de datos. Si el procedimiento existe, se devuelve información sobre sus columnas.  
  
`[ @procedure_qualifier = ] 'qualifier'` Es el nombre del calificador del procedimiento. el *calificador* es de **tipo sysname y su** valor predeterminado es NULL. Varios productos DBMS admiten nombres de tres partes para las tablas (*Qualifier.Owner.Name*). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], este parámetro representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla.  
  
`[ @column_name = ] 'column_name'` Es una sola columna y se utiliza cuando solo se desea una columna de información del catálogo. *column_name* es de tipo **nvarchar (** 384 **)** y su valor predeterminado es NULL. Si se omite *column_name* , se devuelven todas las columnas. Se admite la coincidencia de patrón de caracteres comodín. Para obtener una interoperabilidad máxima, el cliente de puerta de enlace solo debe dar por supuesta la coincidencia de patrón estándar de ISO (caracteres comodín % y _).  
  
`[ @ODBCVer = ] 'ODBCVer'` Es la versión de ODBC que se está utilizando. *ODBCVer* es de **tipo int** y su valor predeterminado es 2, que indica ODBC versión 2,0. Para obtener más información acerca de la diferencia entre ODBC versión 2,0 y ODBC versión 3,0, consulte la especificación de ODBC **SQLProcedureColumns** para odbc versión 3,0  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina si los caracteres de subrayado (_), porcentaje (%) y corchete ([]) se interpretan como caracteres comodín. Los valores válidos son 0 (coincidencia de patrón desactivada) y 1 (coincidencia de patrón activada). *fUsePattern* es de **bit** y su valor predeterminado es 1.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|Nombre del calificador del procedimiento. Esta columna puede ser NULL.|  
|**PROCEDURE_OWNER**|**sysname**|Nombre del propietario del procedimiento. Esta columna siempre devuelve un valor.|  
|**PROCEDURE_NAME**|**nvarchar (** 134 **)**|Nombre del procedimiento. Esta columna siempre devuelve un valor.|  
|**COLUMN_NAME**|**sysname**|Nombre de columna de cada columna del **TABLE_NAME** devuelto. Esta columna siempre devuelve un valor.|  
|**COLUMN_TYPE**|**smallint**|Este campo siempre devuelve un valor:<br /><br /> 0 = SQL_PARAM_TYPE_UNKNOWN<br /><br /> 1 = SQL_PARAM_TYPE_INPUT<br /><br /> 2 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 3 = SQL_RESULT_COL<br /><br /> 4 = SQL_PARAM_OUTPUT<br /><br /> 5 = SQL_RETURN_VALUE|  
|**DATA_TYPE**|**smallint**|Código del tipo de datos entero de un tipo de datos de ODBC. Si no se puede asignar este tipo de datos a un tipo de ISO, el valor es NULL. El nombre del tipo de datos nativo se devuelve en la columna **TYPE_NAME** .|  
|**TYPE_NAME**|**sysname**|Representación de cadena del tipo de datos. Es el nombre del tipo de datos como lo presenta el DBMS subyacente.|  
|**PRECISION**|**int**|Número de dígitos significativos. El valor devuelto para la columna **Precision** está en base 10.|  
|**LENGTH**|**int**|Tamaño de transferencia de los datos.|  
|**ESCALA**|**smallint**|Número de dígitos a la derecha del separador decimal.|  
|**RADIX**|**smallint**|Es la base de tipos numéricos.|  
|**ACEPTA valores NULL**|**smallint**|Especifica la nulabilidad:<br /><br /> 1 = El tipo de datos se puede crear para permitir valores NULL.<br /><br /> 0 = No se permiten valores NULL.|  
|**COMENTARIOS**|**VARCHAR (** 254 **)**|Descripción de la columna de procedimiento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no devuelve ningún valor para esta columna.|  
|**COLUMN_DEF**|**nvarchar (** 4000 **)**|Valor predeterminado de la columna.|  
|**SQL_DATA_TYPE**|**smallint**|Valor del tipo de datos SQL tal como aparece en el campo **Type** del descriptor. Esta columna es igual que la columna **DATA_TYPE**, salvo por los tipos de datos **datetime** e **interval** de ISO. Esta columna siempre devuelve un valor.|  
|**SQL_DATETIME_SUB**|**smallint**|El subcódigo **datetime** ISO **interval** si el valor de **SQL_DATA_TYPE** es **SQL_DATETIME** o **SQL_INTERVAL**. En el caso de tipos de datos distintos de **DateTime** e **Interval** de ISO, este campo es NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Longitud máxima en bytes de una columna de tipo de datos de **caracteres** o **binarios** . Para todos los demás tipos de datos, esta columna devuelve NULL.|  
|**ORDINAL_POSITION**|**int**|Posición ordinal de la columna en la tabla. La primera columna de la tabla es 1. Esta columna siempre devuelve un valor.|  
|**IS_NULLABLE**|**VARCHAR (254)**|Nulabilidad de la columna de la tabla. Se siguen las normas ISO para determinar la nulabilidad. Un DBMS que cumpla la norma ISO no puede devolver una cadena vacía.<br /><br /> Muestra YES si la columna puede incluir valores NULL y muestra NO si la columna no puede contener valores NULL.<br /><br /> Esta columna devuelve una cadena de longitud cero si no se conoce la nulabilidad.<br /><br /> El valor devuelto para esta columna es diferente del valor devuelto para la columna NULLABLE.|  
|**SS_DATA_TYPE**|**tinyint**|Tipo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizado por procedimientos almacenados extendidos. Para obtener más información, vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
  
## <a name="remarks"></a>Observaciones  
 **sp_sproc_columns** es equivalente a **SQLProcedureColumns** en ODBC. Los resultados devueltos se ordenan por **PROCEDURE_QUALIFIER**, **PROCEDURE_OWNER**, **procedure_name** y el orden en que aparecen los parámetros en la definición del procedimiento.  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
