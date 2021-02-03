---
description: HAS_PERMS_BY_NAME (Transact-SQL)
title: HAS_PERMS_BY_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HAS_PERMS_BY_NAME
- HAS_PERMS_BY_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], verifying
- current permission status
- checking permission status
- verifying permission status
- testing permissions
- HAS_PERMS_BY_NAME function
ms.assetid: eaf8cc82-1047-4144-9e77-0e1095df6143
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c80d6a873f0004344e7407dbf25adee5a02c2cd4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184000"
---
# <a name="has_perms_by_name-transact-sql"></a>HAS_PERMS_BY_NAME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Evalúa el permiso efectivo del usuario actual sobre un elemento protegible. Una función relacionada es [fn_my_permissions](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
HAS_PERMS_BY_NAME ( securable , securable_class , permission    
    [ , sub-securable ] [ , sub-securable_class ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *securable*  
 Es el nombre del elemento protegible. Si el elemento protegible es el servidor mismo, este valor debe establecerse en NULL. *securable* es una expresión escalar de tipo **sysname**. No tiene ningún valor predeterminado.  
  
 *securable_class*  
 Es el nombre de la clase de elemento protegible en la cual se prueba el permiso. *securable_class* es una expresión escalar de tipo **nvarchar(60)**.  
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], el argumento securable_class debe establecerse en uno de los valores siguientes: **DATABASE**, **OBJECT**, **ROLE**, **SCHEMA** o **USER**.  
  
 *permission*  
 Expresión escalar no NULL de tipo **sysname** que representa el nombre del permiso que se va a comprobar. No tiene ningún valor predeterminado. El nombre de permiso ANY es un comodín.  
  
 *sub-securable*  
 Expresión escalar opcional de tipo **sysname** que representa el nombre de la subentidad protegible en la que se va a probar el permiso. El valor predeterminado es NULL.  
  
> [!NOTE]  
>  Las subentidades protegibles no pueden usar corchetes con el formato **"[** _nombre_de_la_subentidad_ **]"** . Es mejor usar **'** _sub name_ **'** .  
  
 *sub-securable_class*  
 Expresión escalar opcional de tipo **nvarchar(60)** que representa la clase de subentidad protegible en la que se va a probar el permiso. El valor predeterminado es NULL.  
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], el argumento sub-securable_class solo es válido si el argumento securable_class está establecido en **OBJECT**. Si el argumento securable_class se establece en **OBJECT**, el argumento sub-securable_class debe establecerse en **COLUMN**.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
 Devuelve NULL cuando la consulta da error.  
  
## <a name="remarks"></a>Comentarios  
 Esta función integrada determina si la entidad de seguridad actual tiene un permiso efectivo específico sobre un elemento protegible determinado. HAS_PERMS_BY_NAME devuelve 1 cuando el usuario tiene un permiso efectivo sobre el elemento protegible, 0 cuando el usuario no tiene ningún permiso efectivo sobre el elemento protegible y NULL cuando la clase protegible o el permiso no son válidos. Un permiso efectivo puede ser cualquiera de los siguientes:  
  
-   Un permiso concedido directamente a la entidad de seguridad, no denegado.  
  
-   Un permiso implícito en un permiso de nivel superior de la entidad de seguridad, no denegado.  
  
-   Un permiso concedido a un rol o grupo al que pertenece la entidad de seguridad, no denegado.  
  
-   Un permiso de un rol o grupo al que pertenece la entidad de seguridad, no denegado.  
  
 La evaluación de permisos siempre se realiza en el contexto de seguridad del autor de la llamada. Para determinar si algún otro usuario tiene un permiso efectivo, el autor de la llamada debe tener el permiso IMPERSONATE sobre ese usuario.  
  
 En el caso de entidades de esquema, se aceptan nombres no NULL de una, dos o tres partes. En el caso de entidades de base de datos, se aceptan nombres de una parte, con un valor NULL que significa "base de datos actual". En el caso del servidor, es necesario un valor NULL (que significa "servidor actual"). Esta función no puede comprobar permisos en un servidor vinculado ni en un usuario de Windows para los que no se ha creado ninguna entidad de seguridad a nivel de servidor.  
  
 La consulta siguiente devuelve una lista de clases de elementos protegibles integrados:  
  
```  
SELECT class_desc FROM sys.fn_builtin_permissions(default);  
```  
  
 Se utilizan las intercalaciones siguientes:  
  
-   Intercalación de la base de datos activa: elementos protegibles de base de datos, entre los que se incluyen elementos no incluidos en un esquema; elementos con ámbito de esquema de una o dos partes; base de datos de destino cuando se utiliza un nombre de tres partes.  
  
-   Intercalación de la base de datos maestra: elementos protegibles de servidor.  
  
-   En las comprobaciones de columna no se admite 'ANY'. Debe especificar el permiso apropiado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-do-i-have-the-server-level-view-server-state-permission"></a>A. ¿Tengo el permiso VIEW SERVER STATE en el servidor?  
  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
```sql  
SELECT HAS_PERMS_BY_NAME(null, null, 'VIEW SERVER STATE');  
```  
  
### <a name="b-am-i-able-to-impersonate-server-principal-ps"></a>B. ¿Puedo suplantar (IMPERSONATE) la entidad de seguridad del servidor Ps?  
  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
```sql  
SELECT HAS_PERMS_BY_NAME('Ps', 'LOGIN', 'IMPERSONATE');  
```  
  
### <a name="c-do-i-have-any-permissions-in-the-current-database"></a>C. ¿Tengo algún permiso en la base de datos activa?  
  
```sql  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'ANY');  
```  
  
### <a name="d-does-database-principal-pd-have-any-permission-in-the-current-database"></a>D. ¿Tiene la entidad de seguridad de base de datos Pd algún permiso en la base de datos activa?  
 Suponga que el autor de la llamada tiene el permiso IMPERSONATE sobre la entidad de seguridad `Pd`.  
  
```sql  
EXECUTE AS user = 'Pd'  
GO  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'ANY');  
GO  
REVERT;  
GO  
```  
  
### <a name="e-can-i-create-procedures-and-tables-in-schema-s"></a>E. ¿Puedo crear procedimientos y tablas en el esquema S?  
 Para el ejemplo siguiente es necesario tener el permiso `ALTER` para `S` y el permiso `CREATE PROCEDURE` de la base de datos, y lo mismo para las tablas.  
  
```sql  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'CREATE PROCEDURE')  
    & HAS_PERMS_BY_NAME('S', 'SCHEMA', 'ALTER') AS _can_create_procs,  
    HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'CREATE TABLE') &  
    HAS_PERMS_BY_NAME('S', 'SCHEMA', 'ALTER') AS _can_create_tables;  
```  
  
### <a name="f-which-tables-do-i-have-select-permission-on"></a>F. ¿Sobre qué tablas tengo el permiso SELECT?  
  
```sql  
SELECT HAS_PERMS_BY_NAME  
(QUOTENAME(SCHEMA_NAME(schema_id)) + '.' + QUOTENAME(name),   
    'OBJECT', 'SELECT') AS have_select, * FROM sys.tables  
```  
  
### <a name="g-do-i-have-insert-permission-on-the-salesperson-table-in-adventureworks2012"></a>G. ¿Tengo el permiso INSERT sobre la tabla SalesPerson en AdventureWorks2012?  
 En el ejemplo siguiente se supone que `AdventureWorks2012` es el contexto de la base de datos actual y se utiliza un nombre de dos partes.  
  
```sql  
SELECT HAS_PERMS_BY_NAME('Sales.SalesPerson', 'OBJECT', 'INSERT');  
```  
  
 En el ejemplo siguiente no se asume ningún contexto de base de datos activa y se utiliza un nombre de tres partes.  
  
```sql  
SELECT HAS_PERMS_BY_NAME('AdventureWorks2012.Sales.SalesPerson',   
    'OBJECT', 'INSERT');  
```  
  
### <a name="h-which-columns-of-table-t-do-i-have-select-permission-on"></a>H. ¿Sobre qué columnas de la tabla T tengo el permiso SELECT?  
  
```sql  
SELECT name AS column_name,   
    HAS_PERMS_BY_NAME('T', 'OBJECT', 'SELECT', name, 'COLUMN')   
    AS can_select   
    FROM sys.columns AS c   
    WHERE c.object_id=object_id('T');  
```  
  
## <a name="see-also"></a>Vea también  
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)  
  
  
