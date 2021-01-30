---
description: sp_helprotect (Transact-SQL)
title: sp_helprotect (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helprotect
- sp_helprotect_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helprotect
ms.assetid: faaa3e40-1c95-43c2-9fdc-c61a1d3cc0c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f64ca2d34d11811bec87c1e4e378b424065299c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211932"
---
# <a name="sp_helprotect-transact-sql"></a>sp_helprotect (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve un informe con información acerca de los permisos de usuario para un objeto o los permisos de una instrucción en la base de datos actual.  
  
> [!IMPORTANT]  
>  **sp_helprotect** no devuelve información acerca de los elementos protegibles que se introdujeron en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Utilice [Sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) y [fn_builtin_permissions](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md) en su lugar.  
  
 No muestra los permisos que se asignan siempre a los roles fijos de servidor o a los roles fijos de base de datos. No incluye los inicios de sesión o los usuarios que reciben los permisos en función de su pertenencia a un rol.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helprotect [ [ @name = ] 'object_statement' ]   
     [ , [ @username = ] 'security_account' ]   
     [ , [ @grantorname = ] 'grantor' ]   
     [ , [ @permissionarea = ] 'type' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'object_statement'` Es el nombre del objeto en la base de datos actual, o una instrucción, que tiene los permisos para notificar. *object_statement* es de tipo **nvarchar (776)** y su valor predeterminado es null, que devuelve todos los permisos de objeto y de instrucción. Si el valor es un objeto (tabla, vista, procedimiento almacenado o procedimiento almacenado extendido), tiene que ser un objeto válido de la base de datos actual. El nombre del objeto puede incluir un calificador de propietario en el formulario _Owner_**.** _objeto_.  
  
 Si *object_statement* es una instrucción, puede ser una instrucción CREATE.  
  
`[ @username = ] 'security_account'` Es el nombre de la entidad de seguridad para la que se devuelven los permisos. *security_account* es de **tipo sysname y su** valor predeterminado es null, que devuelve todas las entidades de seguridad en la base de datos actual. *security_account* debe existir en la base de datos actual.  
  
`[ @grantorname = ] 'grantor'` Es el nombre de la entidad de seguridad que concedió permisos. el *otorgante* es de **tipo sysname y su** valor predeterminado es null, que devuelve toda la información de los permisos concedidos por cualquier entidad de seguridad en la base de datos.  
  
`[ @permissionarea = ] 'type'`Es una cadena de caracteres que indica si se van a mostrar los permisos de objeto **(cadena de** caracteres **o**), los permisos de instrucción (cadena de caracteres) o ambos (**so**). *Type* es de tipo **VARCHAR (10)** y su valor predeterminado es **os**. el *tipo* puede ser cualquier combinación **de o** y **s**, con o sin comas o espacios entre **o** y **s**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**Propietario**|**sysname**|Nombre del propietario del objeto.|  
|**Object**|**sysname**|Nombre del objeto.|  
|**Receptores**|**sysname**|Nombre de la entidad de seguridad en la que se conceden los permisos.|  
|**Otorgante de permisos**|**sysname**|Nombre de la entidad de seguridad que ha concedido los permisos al receptor de permisos especificado.|  
|**ProtectType**|**nvarchar(10**|Nombre del tipo de protección:<br /><br /> GRANT REVOKE|  
|**Acción**|**nvarchar(60)**|Nombre del permiso. Las instrucciones válidas de permisos dependen del tipo de objeto.|  
|**Columna**|**sysname**|Tipo de permiso:<br /><br /> All = Permiso sobre todas las columnas actuales del objeto.<br /><br /> New = Permiso sobre las nuevas columnas del objeto que se pueden cambiar (mediante la instrucción ALTER) posteriormente.<br /><br /> All+New = Combinación de Todas y Nuevas.<br /><br /> Devuelve un punto si el tipo de permiso no se aplica a las columnas.|  
  
## <a name="remarks"></a>Observaciones  
 Todos los parámetros del siguiente procedimiento son opcionales. Si se ejecuta sin parámetros, `sp_helprotect` presenta todos los permisos que se han concedido o denegado en la base de datos actual.  
  
 Si se especifican algunos parámetros, pero no todos, se tienen que utilizar parámetros con nombre para identificar el parámetro concreto o `NULL` como marcador de posición. Por ejemplo, para presentar todos los permisos del propietario de la base de datos del otorgante de permisos (`dbo`), ejecute:  
  
```  
EXEC sp_helprotect NULL, NULL, dbo;  
```  
  
 Or  
  
```  
EXEC sp_helprotect @grantorname = 'dbo';  
```  
  
 El resultado está ordenada por categoría de los permisos, propietario, objeto, beneficiario, asignador, categoría del tipo de protección, tipo de protección, acción e Id. secuencial de columna.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
 La información mostrada está sometida a restricciones de acceso a los metadatos. No se mostrarán las entidades en las que la entidad de seguridad no tiene permiso. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-the-permissions-for-a-table"></a>A. Presentar los permisos de una tabla  
 En el siguiente ejemplo se presentan los permisos de la tabla `titles`.  
  
```  
EXEC sp_helprotect 'titles';  
```  
  
### <a name="b-listing-the-permissions-for-a-user"></a>B. Presentar los permisos de un usuario  
 En el siguiente ejemplo se presentan todos los permisos que el usuario `Judy` tiene en la base de datos actual.  
  
```  
EXEC sp_helprotect NULL, 'Judy';  
```  
  
### <a name="c-listing-the-permissions-granted-by-a-specific-user"></a>C. Presentar los permisos concedidos por un usuario específico  
 En el siguiente ejemplo se presentan todos los permisos concedidos por el usuario `Judy` en la base de datos actual, con `NULL` como marcador de posición en los parámetros no especificados.  
  
```  
EXEC sp_helprotect NULL, NULL, 'Judy';  
```  
  
### <a name="d-listing-the-statement-permissions-only"></a>D. Presentar solo los permisos de las instrucciones  
 En el siguiente ejemplo se presentan todos los permisos de instrucciones de la base de datos actual, con `NULL` como marcador de posición para los parámetros no especificados.  
  
```  
EXEC sp_helprotect NULL, NULL, NULL, 's';   
```  
  
### <a name="e-listing-the-permissions-for-a-create-statement"></a>e. Presentar los permisos de una instrucción CREATE  
 En el ejemplo siguiente se muestran todos los usuarios a los que se ha concedido el permiso CREATE TABLE.  
  
```  
EXEC sp_helprotect @name = 'CREATE TABLE';  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
