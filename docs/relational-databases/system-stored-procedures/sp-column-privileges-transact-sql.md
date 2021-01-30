---
description: sp_column_privileges (Transact-SQL)
title: sp_column_privileges (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_column_privileges_TSQL
- sp_column_privileges
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges
ms.assetid: a3784301-2517-4b1d-bbd9-47404483fad0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: db99cb2e9acb8ede62a066291cce68d209e495c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174529"
---
# <a name="sp_column_privileges-transact-sql"></a>sp_column_privileges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información acerca de los privilegios de columna de una tabla del entorno actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_column_privileges [ @table_name = ] 'table_name'   
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @table_qualifier = ] 'table_qualifier' ]   
     [ , [ @column_name = ] 'column' ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @table_name =] '*TABLE_NAME*'  
 Es la tabla que se usa para devolver información de catálogo. *TABLE_NAME* es de **tipo sysname** y no tiene ningún valor predeterminado. No se admite la coincidencia de patrón de caracteres comodín.  
  
 [ @table_owner =] '*table_owner*'  
 Es el propietario de la tabla que se utiliza para devolver información de catálogo. *table_owner* es de **tipo sysname y su** valor predeterminado es NULL. No se admite la coincidencia de patrón de caracteres comodín. Si no se especifica *table_owner* , se aplican las reglas predeterminadas de visibilidad de la tabla del sistema de administración de bases de datos (DBMS) subyacente.  
  
 Si el usuario actual es propietario de una tabla con el nombre especificado, se devuelven las columnas de esa tabla. Si no se especifica *table_owner* y el usuario actual no es el propietario de una tabla con el *table_name* especificado, sp_column privilegios busca una tabla con el *TABLE_NAME* especificado propiedad del propietario de la base de datos. Si hay una, se devuelven las columnas de esa tabla.  
  
 [ @table_qualifier =] '*TABLE_QUALIFIER*'  
 Es el nombre del calificador de tabla. *TABLE_QUALIFIER* es de *tipo sysname y su* valor predeterminado es NULL. Varios productos DBMS admiten nombres de tres partes para las tablas (_calificador_**.** _propietario_**.** _nombre_). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla.  
  
 [ @column_name =] '*columna*'  
 Es una sola columna que se usa cuando solo se obtiene una columna de información del catálogo. la *columna* es de tipo **nvarchar (** 384 **)** y su valor predeterminado es NULL. Si no se especifica *Column* , se devuelven todas las columnas. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la *columna* representa el nombre de la columna tal y como aparece en la tabla sys. Columns. la *columna* puede incluir caracteres comodín mediante patrones de coincidencia de caracteres comodín del DBMS subyacente. Para obtener una interoperabilidad máxima, el cliente de puerta de enlace solo debe dar por supuesta la coincidencia de patrón estándar de ISO (caracteres comodín % y _).  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 sp_column_privileges equivale a SQLColumnPrivileges en ODBC. Los resultados devueltos se ordenan por TABLE_QUALIFIER, TABLE_OWNER, TABLE_NAME, COLUMN_NAME y PRIVILEGE.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|TABLE_QUALIFIER|**sysname**|Nombre del calificador de tabla. Este campo puede ser NULL.|  
|TABLE_OWNER|**sysname**|Nombre del propietario de la tabla. Este campo siempre devuelve un valor.|  
|TABLE_NAME|**sysname**|Nombre de la tabla. Este campo siempre devuelve un valor.|  
|COLUMN_NAME|**sysname**|Nombre de columna por cada columna devuelta de TABLE_NAME. Este campo siempre devuelve un valor.|  
|GRANTOR|**sysname**|Nombre del usuario de la base de datos que ha concedido permisos para COLUMN_NAME al GRANTEE presentado. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna es siempre la misma que en TABLE_OWNER. Este campo siempre devuelve un valor.<br /><br /> La columna GRANTOR puede ser el propietario de la base de datos (TABLE_OWNER) o un usuario al que el propietario de la base de datos le haya concedido permisos mediante la cláusula WITH GRANT OPTION en la instrucción GRANT.|  
|GRANTEE|**sysname**|Nombre del usuario de la base de datos al que el GRANTOR presentado le ha concedido permisos para COLUMN_NAME. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta columna incluye siempre un usuario de la base de datos de la tabla sysusers. Este campo siempre devuelve un valor.|  
|PRIVILEGE|**VARCHAR (** 32 **)**|Uno de los permisos de columna disponibles. Los permisos de columna pueden ser uno de los valores siguientes (u otros valores compatibles con el origen de datos cuando se define la implementación):<br /><br /> SELECT = GRANTEE puede recuperar datos de las columnas.<br /><br /> INSERT = GRANTEE puede proporcionar datos para esta columna cuando se inserten nuevas filas (por parte de GRANTEE) en la tabla.<br /><br /> UPDATE = GRANTEE puede modificar datos existentes en la columna.<br /><br /> REFERENCES = GRANTEE puede hacer referencia a una columna de una tabla externa en una relación entre clave principal y clave externa. Las relaciones entre clave principal y clave externa se definen mediante restricciones de tabla.|  
|IS_GRANTABLE|**VARCHAR (** 3 **)**|Indica si se permite que GRANTEE conceda permisos a otros usuarios (a menudo se hace referencia a esta operación como permiso “conceder por concesión”). Puede ser YES, NO o NULL. Un valor desconocido, o NULL, hace referencia a un origen de datos para el que “conceder por concesión” no se aplica.|  
  
## <a name="remarks"></a>Observaciones  
 Con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los permisos se conceden mediante la instrucción GRANT y se retiran mediante la instrucción REVOKE.  
  
## <a name="permissions"></a>Permisos  
 Es necesario contar con un permiso de tipo SELECT sobre el esquema.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve información acerca de privilegios de columna de una tabla específica.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_column_privileges @table_name = 'Employee'   
    ,@table_owner = 'HumanResources'  
    ,@table_qualifier = 'AdventureWorks2012'  
    ,@column_name = 'SalariedFlag';  
```  
  
## <a name="see-also"></a>Consulte también  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
