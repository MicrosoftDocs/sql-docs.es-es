---
description: sp_special_columns (Transact-SQL)
title: sp_special_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_special_columns_TSQL
- sp_special_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sp_special_columns
ms.assetid: 0b0993f8-73e0-402b-8c6c-1b0963956f5d
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 909bc7ca63546d74e38bdebff733e8e7d753f28e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189252"
---
# <a name="sp_special_columns-transact-sql"></a>sp_special_columns (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el conjunto óptimo de columnas que identifican de forma única a una fila de la tabla. También devuelve las columnas actualizadas automáticamente cuando una transacción actualiza cualquier valor de la fila.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_special_columns [ @table_name = ] 'table_name'     
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @qualifier = ] 'qualifier' ]   
     [ , [ @col_type = ] 'col_type' ]   
     [ , [ @scope = ] 'scope' ]  
     [ , [ @nullable = ] 'nullable' ]   
     [ , [ @ODBCVer = ] 'ODBCVer' ]   
[ ; ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @table_name =] '*TABLE_NAME*'  
 Es el nombre de la tabla que se utiliza para devolver información de catálogo. *Name* es de **tipo sysname** y no tiene ningún valor predeterminado. No se admite la coincidencia de patrón de caracteres comodín.  
  
 [ @table_owner =] '*table_owner*'  
 Es el propietario de la tabla que se utiliza para devolver información del catálogo. *Owner* es de **tipo sysname y su** valor predeterminado es NULL. No se admite la coincidencia de patrón de caracteres comodín. Si no se especifica *Owner* , se aplican las reglas predeterminadas de visibilidad de tabla del DBMS subyacente.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el usuario actual posee una tabla en la que se especifica el nombre, se devuelven las columnas de esa tabla. Si no se especifica *Owner* y el usuario actual no posee una tabla con el *nombre* especificado, este procedimiento busca una tabla con el *nombre* especificado que pertenezca al propietario de la base de datos. Si la tabla existe, se devuelven sus columnas.  
  
 [ @qualifier =] '*calificador*'  
 Es el nombre del calificador de tabla. el *calificador* es de **tipo sysname y su** valor predeterminado es NULL. Varios productos DBMS admiten nombres de tres partes para las tablas (*Qualifier.Owner.Name*). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla.  
  
 [ @col_type =] '*col_type*'  
 Es el tipo de columna. *col_type* es **Char (** 1 **)**, con un valor predeterminado de r. el tipo r devuelve la columna óptima o el conjunto de columnas que, al recuperar valores de la columna o columnas, permite que cualquier fila de la tabla especificada se identifique de forma única. Una columna puede ser una pseudocolumna diseñada específicamente para este propósito o bien la columna o columnas de cualquier índice único de la tabla. El tipo V devuelve la columna o columnas de la tabla especificada (en su caso) que el origen de datos actualiza automáticamente cuando una transacción actualiza cualquier valor de la fila.  
  
 [ @scope =] '*ámbito*'  
 Es el ámbito mínimo necesario del ROWID. el *ámbito* es **Char (** 1 **)** y su valor predeterminado es T. el ámbito C especifica que el ROWID es válido solo cuando se coloca en esa fila. El ámbito T especifica que el ROWID es válido para la transacción.  
  
 [ @nullable =] '*Nullable*'  
 Indica si las columnas especiales pueden o no aceptar un valor NULL. *Nullable* es **Char (** 1 **)** y su valor predeterminado es U. O especifica columnas especiales que no admiten valores NULL. U especifica columnas que admiten parcialmente valores NULL.  
  
 [ @ODBCVer =] '*ODBCVer*'  
 Es la versión de ODBC utilizada. *ODBCVer* es de **tipo int (** 4 **)** y su valor predeterminado es 2. Esto indica ODBC versión 2.0. Para obtener más información acerca de las diferencias entre ODBC versión 2.0 y ODBC versión 3.0, vea la especificación SQLSpecialColumns para ODBC versión 3.0.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|SCOPE|**smallint**|Ámbito real del identificador de fila. Puede ser 0, 1 o 2. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siempre devuelve 0. Este campo siempre devuelve un valor.<br /><br /> 0 = SQL_SCOPE_CURROW. Se garantiza que el Id. de fila es válido solo mientras esté colocado en esa fila. Una nueva selección posterior mediante el Id. de fila podría no devolver una fila si otra transacción ha actualizado o eliminado la fila.<br /><br /> 1 = SQL_SCOPE_TRANSACTION. Se garantiza que el Id. de fila es válido mientras dura la transacción actual.<br /><br /> 2 = SQL_SCOPE_SESSION. Se garantiza que el identificador de fila sea válido mientras dure la sesión (en los límites de la transacción).|  
|COLUMN_NAME|**sysname**|Nombre de columna de cada columna de la *tabla* devuelta. Este campo siempre devuelve un valor.|  
|DATA_TYPE|**smallint**|Tipo de datos de ODBC SQL.|  
|TYPE_NAME|**sysname**|Nombre del tipo de datos dependiente del origen de datos; por ejemplo, **Char**, **VARCHAR**, **Money** o **Text**.|  
|PRECISION|**Int**|Precisión de la columna en el origen de datos. Este campo siempre devuelve un valor.|  
|LENGTH|**Int**|Longitud, en bytes, necesaria para el tipo de datos en su forma binaria en el origen de datos, por ejemplo, 10 para **Char (** 10 **)**, 4 para **Integer** y 2 para **smallint**.|  
|SCALE|**smallint**|Escala de la columna en el origen de datos. NULL se devuelve para los tipos de datos a los que no se aplica la escala.|  
|PSEUDO_COLUMN|**smallint**|Indica si la columna es una pseudocolumna. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siempre devuelve 1:<br /><br /> 0 = SQL_PC_UNKNOWN<br /><br /> 1 = SQL_PC_NOT_PSEUDO<br /><br /> 2 = SQL_PC_PSEUDO|  
  
## <a name="remarks"></a>Observaciones  
 sp_special_columns es equivalente a SQLSpecialColumns en ODBC. Los resultados devueltos se ordenan por medio de SCOPE.  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve información acerca de la columna que identifica de forma exclusiva las filas en la tabla `HumanResources.Department`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_special_columns @table_name = 'Department'   
    ,@table_owner = 'HumanResources';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
