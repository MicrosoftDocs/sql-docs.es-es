---
description: sp_pkeys (Transact-SQL)
title: sp_pkeys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_pkeys
- sp_pkeys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_pkeys
ms.assetid: e614c75d-847b-4726-8f6f-cd18de688eda
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bfb9d942ae25c8bf690e1ed9740f56c021cf413d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190026"
---
# <a name="sp_pkeys-transact-sql"></a>sp_pkeys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve la información de clave principal de una única tabla en el entorno actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_pkeys [ @table_name = ] 'name'       
    [ , [ @table_owner = ] 'owner' ]   
    [ , [ @table_qualifier = ] 'qualifier' ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @table_name =] '*nombre*'  
 Es la tabla para la que se va a devolver información. *Name* es de **tipo sysname** y no tiene ningún valor predeterminado. No se admite la coincidencia de patrón de caracteres comodín.  
  
 [ @table_owner =] '*propietario*'  
 Especifica el propietario de la tabla especificada. *Owner* es de **tipo sysname y su** valor predeterminado es NULL. No se admite la coincidencia de patrón de caracteres comodín. Si no se especifica *Owner* , se aplican las reglas predeterminadas de visibilidad de tabla del DBMS subyacente.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el usuario actual posee una tabla en la que se especifica el nombre, se devuelven las columnas de esa tabla. Si no se especifica el *propietario* y el usuario actual no posee una tabla con el *nombre* especificado, este procedimiento busca una tabla con el *nombre* especificado que pertenezca al propietario de la base de datos. Si existe una, se devuelven las columnas de esa tabla.  
  
 [ @table_qualifier =] '*calificador*'  
 Es el calificador de la tabla. el *calificador* es de **tipo sysname y su** valor predeterminado es NULL. Varios productos DBMS admiten nombres de tres partes para las tablas (_calificador_**.** _propietario_**.** _nombre_). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|TABLE_QUALIFIER|**sysname**|Nombre del calificador de tabla. Este campo puede ser NULL.|  
|TABLE_OWNER|**sysname**|Nombre del propietario de la tabla. Este campo siempre devuelve un valor.|  
|TABLE_NAME|**sysname**|Nombre de la tabla. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la tabla como se muestra en la tabla sysobjects. Este campo siempre devuelve un valor.|  
|COLUMN_NAME|**sysname**|Nombre de la columna, para cada columna de TABLE_NAME devuelta. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la columna como se muestra en la tabla sys.columns. Este campo siempre devuelve un valor.|  
|KEY_SEQ|**smallint**|Número de secuencia de la columna en una clave principal con varias columnas.|  
|PK_NAME|**sysname**|Identificador de la clave principal. Devuelve NULL si no es aplicable al origen de datos.|  
  
## <a name="remarks"></a>Observaciones  
 sp_pkeys devuelve información acerca de las columnas definidas explícitamente con una restricción PRIMARY KEY. Debido a que no todos los sistemas aceptan claves principales con nombre explícito, el implementador de puerta de enlace determina qué constituye una clave principal. Observe que el término clave principal hace referencia a la clave principal lógica de una tabla. Se espera que cada clave enumerada como clave principal lógica tenga un índice único definido en la misma. Este índice único también se devuelve en sp_statistics.  
  
 El procedimiento almacenado sp_pkeys es equivalente a SQLPrimaryKeys en ODBC. Los resultados devueltos se ordenan por TABLE_QUALIFIER, TABLE_OWNER, TABLE_NAME y KEY_SEQ.  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se recupera la clave principal de la tabla `HumanResources.Department` de la base de datos `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_pkeys @table_name = N'Department'  
    ,@table_owner = N'HumanResources';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el siguiente ejemplo se recupera la clave principal de la tabla `DimAccount` de la base de datos `AdventureWorksPDW2012`. Devuelve cero filas que indican que la tabla no tiene una clave principal.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_pkeys @table_name = N'DimAccount';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

