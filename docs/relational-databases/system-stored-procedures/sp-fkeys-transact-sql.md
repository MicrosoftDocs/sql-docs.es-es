---
description: sp_fkeys (Transact-SQL)
title: sp_fkeys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fkeys
- sp_fkeys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fkeys
ms.assetid: 18110444-d38d-4cff-90d2-d1fc6236668b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 11cd43abf45386f8bfd1a7b897bc9ad0e09893ae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189578"
---
# <a name="sp_fkeys-transact-sql"></a>sp_fkeys (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve información acerca de las claves externas lógicas del entorno actual. Este procedimiento muestra las relaciones de las claves externas, incluidas las claves externas deshabilitadas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
sp_fkeys [ @pktable_name = ] 'pktable_name'   
     [ , [ @pktable_owner = ] 'pktable_owner' ]   
     [ , [ @pktable_qualifier = ] 'pktable_qualifier' ]   
     { , [ @fktable_name = ] 'fktable_name' }   
     [ , [ @fktable_owner = ] 'fktable_owner' ]   
     [ , [ @fktable_qualifier = ] 'fktable_qualifier' ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @pktable_name =] '*pktable_name*'  
 Es el nombre de la tabla, con la clave externa, utilizada para devolver información del catálogo. *pktable_name* es de **tipo sysname y su** valor predeterminado es NULL. No se admite la coincidencia de patrón de caracteres comodín. Se deben proporcionar este parámetro o el parámetro *fktable_name* , o ambos.  
  
 [ @pktable_owner =] '*pktable_owner*'  
 Es el nombre del propietario de la tabla (con la clave principal) que se usa para devolver información del catálogo. *pktable_owner* es de **tipo sysname y su** valor predeterminado es NULL. No se admite la coincidencia de patrón de caracteres comodín. Si no se especifica *pktable_owner* , se aplican las reglas predeterminadas de visibilidad de tabla del DBMS subyacente.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el usuario actual es propietario de una tabla con el nombre especificado, se devuelven las columnas de esa tabla. Si no se especifica *pktable_owner* y el usuario actual no es el propietario de una tabla con el *pktable_name* especificado, el procedimiento busca una tabla con el *pktable_name* especificado propiedad del propietario de la base de datos. Si hay una, se devuelven las columnas de esa tabla.  
  
 [ @pktable_qualifier =] '*pktable_qualifier*'  
 Es el nombre del calificador de la tabla (con la clave principal). *pktable_qualifier* es de tipo sysname y su valor predeterminado es NULL. Varios productos DBMS admiten nombres de tres partes para las tablas (*Qualifier.Owner.Name*). En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el calificador representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla.  
  
 [ @fktable_name =] '*fktable_name*'  
 Es el nombre de la tabla, con una clave externa, utilizada para devolver información del catálogo. *fktable_name* es de tipo sysname y su valor predeterminado es NULL. No se admite la coincidencia de patrón de caracteres comodín. Se deben proporcionar este parámetro o el parámetro *pktable_name* , o ambos.  
  
 [ @fktable_owner =] '*fktable_owner*'  
 Es el nombre del propietario de la tabla, con una clave externa, utilizada para devolver información del catálogo. *fktable_owner* es de **tipo sysname y su** valor predeterminado es NULL. No se admite la coincidencia de patrón de caracteres comodín. Si no se especifica *fktable_owner* , se aplican las reglas predeterminadas de visibilidad de tabla del DBMS subyacente.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el usuario actual es propietario de una tabla con el nombre especificado, se devuelven las columnas de esa tabla. Si no se especifica *fktable_owner* y el usuario actual no es el propietario de una tabla con el *fktable_name* especificado, el procedimiento busca una tabla con el *fktable_name* especificado propiedad del propietario de la base de datos. Si hay una, se devuelven las columnas de esa tabla.  
  
 [ @fktable_qualifier =] '*fktable_qualifier*'  
 Es el nombre del calificador de la tabla (con una clave externa). *fktable_qualifier* es de **tipo sysname y su** valor predeterminado es NULL. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el calificador representa el nombre de la base de datos. En algunos productos, representa el nombre del servidor del entorno de base de datos de la tabla.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|PKTABLE_QUALIFIER|**sysname**|Nombre del calificador de la tabla (con la clave principal). Este campo puede ser NULL.|  
|PKTABLE_OWNER|**sysname**|Nombre del propietario de la tabla (con la clave principal). Este campo siempre devuelve un valor.|  
|PKTABLE_NAME|**sysname**|Nombre de la tabla (con la clave principal). Este campo siempre devuelve un valor.|  
|PKCOLUMN_NAME|**sysname**|Nombre de las columnas de clave principal para cada columna de TABLE_NAME devuelta. Este campo siempre devuelve un valor.|  
|FKTABLE_QUALIFIER|**sysname**|Nombre del calificador de la tabla (con una clave externa). Este campo puede ser NULL.|  
|FKTABLE_OWNER|**sysname**|Nombre del propietario de la tabla (con una clave externa). Este campo siempre devuelve un valor.|  
|FKTABLE_NAME|**sysname**|Nombre de la tabla (con una clave externa). Este campo siempre devuelve un valor.|  
|FKCOLUMN_NAME|**sysname**|Nombre de la columna de clave externa para cada columna de TABLE_NAME devuelta. Este campo siempre devuelve un valor.|  
|KEY_SEQ|**smallint**|Número de secuencia de la columna en una clave principal con varias columnas. Este campo siempre devuelve un valor.|  
|UPDATE_RULE|**smallint**|Acción aplicada a la clave externa si la operación de SQL es una actualización.  Valores posibles:<br /> 0=CASCADE cambia a clave externa.<br /> 1=NO ACTION cambia si la clave externa está presente.<br />   2 = establecer null <br /> 3 = establecer predeterminado |  
|DELETE_RULE|**smallint**|Acción aplicada a la clave externa si la operación de SQL es una eliminación. Valores posibles:<br /> 0=CASCADE cambia a clave externa.<br /> 1=NO ACTION cambia si la clave externa está presente.<br />   2 = establecer null <br /> 3 = establecer predeterminado |  
|FK_NAME|**sysname**|Identificador de la clave externa. Es NULL si no es aplicable al origen de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve el nombre de la restricción FOREIGN KEY.|  
|PK_NAME|**sysname**|Identificador de la clave principal. Es NULL si no es aplicable al origen de datos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve el nombre de la restricción PRIMARY KEY.|  
  
 Los resultados devueltos se ordenan por FKTABLE_QUALIFIER, FKTABLE_OWNER, FKTABLE_NAME y KEY_SEQ.  
  
## <a name="remarks"></a>Observaciones  
 La codificación de aplicaciones que incluye tablas con claves externas deshabilitadas se puede implementar si:  
  
-   Se deshabilita temporalmente la comprobación de restricciones (ALTER TABLE NOCHECK o CREATE TABLE NOT FOR REPLICATION) mientras se trabaja con las tablas y, después, se vuelve a habilitar.  
  
-   Se utilizan desencadenadores o código de aplicación para exigir relaciones.  
  
Si se proporciona el nombre de tabla de la clave principal y el nombre de tabla de la clave externa es NULL, sp_fkeys devuelve todas las tablas que contengan una clave externa para la tabla dada. Si se proporciona el nombre de tabla de la clave externa y el nombre de tabla de la clave principal es NULL, sp_fkeys devuelve todas las tablas relacionadas mediante una relación de clave principal y clave externa con las claves externas de la tabla de claves externas.  
  
El procedimiento almacenado sp_fkeys es equivalente a SQLForeignKeys en ODBC.  
  
## <a name="permissions"></a>Permisos  
 Requiere `SELECT` el permiso en el esquema.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se recupera una lista de claves externas de la tabla `HumanResources.Department` en la base de datos `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_fkeys @pktable_name = N'Department'  
    ,@pktable_owner = N'HumanResources';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el siguiente ejemplo se recupera una lista de claves externas de la tabla `DimDate` en la base de datos `AdventureWorksPDW2012`. No se devuelven filas porque no [!INCLUDE[ssDW](../../includes/ssdw-md.md)] admite claves externas.  
  
```sql  
EXEC sp_fkeys @pktable_name = N'DimDate';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_pkeys &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-pkeys-transact-sql.md)  
  
  

