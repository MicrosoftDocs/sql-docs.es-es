---
description: CREATE SCHEMA (Transact-SQL)
title: CREATE SCHEMA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SCHEMA_TSQL
- SCHEMA
- CREATE SCHEMA
- SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], schemas
- table creation [SQL Server], CREATE SCHEMA statement
- permissions [SQL Server], schemas
- schemas [SQL Server], creating
- CREATE SCHEMA statement
ms.assetid: df74fc36-20da-4efa-b412-c4e191786695
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 838a9c242b729d0fe0433aa9f155d5074f248c7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188523"
---
# <a name="create-schema-transact-sql"></a>CREATE SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Crea un esquema en la base de datos actual. La transacción CREATE SCHEMA también puede crear tablas y vistas dentro del nuevo esquema, así como establecer la concesión, denegación o revocación (GRANT, DENY o REVOKE) de permisos en esos objetos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE SCHEMA schema_name_clause [ <schema_element> [ ...n ] ]  
  
<schema_name_clause> ::=  
    {  
    schema_name  
    | AUTHORIZATION owner_name  
    | schema_name AUTHORIZATION owner_name  
    }  
  
<schema_element> ::=   
    {   
        table_definition | view_definition | grant_statement |   
        revoke_statement | deny_statement   
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE SCHEMA schema_name [ AUTHORIZATION owner_name ] [;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *schema_name*  
 Es el nombre por el que se identifica al esquema en esta base de datos.  
  
 AUTHORIZATION *owner_name*  
 Especifica el nombre de la entidad de seguridad de la base de datos que poseerá el esquema. Es posible que esta entidad de seguridad posea otros esquemas y no utilice el esquema actual como predeterminado.  
  
 *table_definition*  
 Especifica una instrucción CREATE TABLE que crea una tabla en el esquema. La entidad de seguridad que ejecuta esta instrucción debe tener el permiso CREATE TABLE en la base de datos actual.  
  
 *view_definition*  
 Especifica una instrucción CREATE VIEW que crea una vista en el esquema. La entidad de seguridad que ejecuta esta instrucción debe tener el permiso CREATE VIEW en la base de datos actual.  
  
 *grant_statement*  
 Especifica una instrucción GRANT que otorga permisos sobre cualquier elemento protegible, excepto el esquema nuevo.  
  
 *revoke_statement*  
 Especifica una instrucción REVOKE que revoca permisos sobre cualquier elemento protegible, excepto el esquema nuevo.  
  
 *deny_statement*  
 Especifica una instrucción DENY que deniega permisos sobre cualquier elemento protegible, excepto el esquema nuevo.  
  
## <a name="remarks"></a>Observaciones  
  
> [!NOTE]  
>  Las instrucciones que contienen CREATE SCHEMA AUTHORIZATION pero no especifican ningún nombre solo se admiten por razones de compatibilidad con versiones anteriores. La instrucción no provoca errores, pero no crea esquemas.  
  
 CREATE SCHEMA puede crear un esquema, las tablas y las vistas que lo contienen; asimismo, puede tener permisos GRANT, REVOKE o DENY para cualquier elemento protegible; todo ello en una sola instrucción. Esta instrucción debe ejecutarse como un lote independiente. Los objetos creados por la instrucción CREATE SCHEMA se crean dentro del esquema que se está creando.  
  
 Las transacciones CREATE SCHEMA son atómicas. Si ocurre un error durante la ejecución de una instrucción CREATE SCHEMA, no se crea ninguno de los elementos protegibles especificados ni se conceden permisos.  
  
 Los elementos protegibles, que va a crear CREATE SCHEMA, se pueden enumerar en cualquier orden, excepto en el caso de las vistas que hacen referencia a otras vistas. En estos casos, la vista a la que se hace referencia debe crearse antes que la vista que hace la referencia.  
  
 Por lo tanto, una instrucción GRANT puede conceder permiso para un objeto antes de que ese objeto se haya creado o una instrucción CREATE VIEW puede aparecer antes que las instrucciones CREATE TABLE que crean las tablas a las que hace referencia la vista. Además, las instrucciones CREATE TABLE pueden declarar claves externas a las tablas definidas posteriormente en la instrucción CREATE SCHEMA.  
  
> [!NOTE]  
>  DENY y REVOKE se admiten dentro de instrucciones CREATE SCHEMA. Las cláusulas DENY y REVOKE se ejecutarán en el orden en que aparecen en la instrucción CREATE SCHEMA.  
  
 La entidad de seguridad que ejecuta CREATE SCHEMA puede especificar otra entidad de seguridad de base de datos como el propietario del esquema que se crea. Esto requiere permisos adicionales, como se describe en la sección "Permisos", más adelante en este tema.  
  
 El esquema nuevo es propiedad de una de las siguientes entidades de seguridad de nivel de base de datos: usuario de base de datos, rol de base de datos o rol de aplicación. Los objetos creados dentro de un esquema son propiedad del esquema y tienen un **principal_id** NULL en **sys.objects**. La propiedad de los objetos incluidos en el esquema puede transferirse a cualquier entidad de seguridad de nivel de base de datos, pero el propietario del esquema siempre mantiene el permiso CONTROL en los objetos del esquema.  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
 **Esquema implícito y creación de usuario**  
  
 En algunos casos, un usuario puede usar una base de datos sin necesidad de tener una cuenta de usuario de base de datos (una entidad de seguridad de base de datos en la base de datos). El error puede ocurrir en las siguientes situaciones:  
  
-   Un inicio de sesión tiene privilegios **CONTROL SERVER**.  
  
-   Un usuario de Windows no tiene una cuenta de usuario de base de datos individual (una entidad de seguridad de base de datos en la base de datos), sino que obtiene acceso a una base de datos como miembro de un grupo de Windows que tiene una cuenta de usuario de base de datos (una entidad de seguridad de base de datos para el grupo de Windows).  
  
 Cuando un usuario sin cuenta de usuario de base de datos crea un objeto sin especificar un esquema existente, se crea automáticamente una entidad de seguridad de base de datos y un esquema predeterminado en la base de datos para ese usuario. La entidad de seguridad de base de datos y el esquema que se creen tendrán el mismo nombre que el usuario empleado para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (el nombre de inicio de sesión de autenticación en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el nombre de usuario de Windows).  
  
 Este comportamiento es necesario para permitir a los usuarios basados en grupos de Windows crear y poseer objetos. Sin embargo, puede dar lugar a la creación accidental de esquemas y usuarios. Para evitar crear implícitamente usuarios y esquemas, siempre que sea posible cree explícitamente entidades de seguridad de base de datos y asigne un esquema predeterminado. O establezca explícitamente un esquema existente cuando cree objetos en una base de datos, utilizando nombres de objetos de dos o tres elementos.  

> [!NOTE]
>  La creación implícita de un usuario de Azure Active Directory no es posible en [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]. Como la creación de un usuario de Azure AD a partir de un proveedor externo conlleva comprobar el estado de los usuarios en AAD, se producirá el error 2760: **El nombre de esquema "\<user_name@domain>" especificado no existe o no tiene permisos para utilizarlo.** Y después el error 2759: **Error en CREATE SCHEMA provocado por los errores anteriores**. Para solucionarlos, cree el usuario de Azure AD de proveedor externo en primer lugar y, después, vuelva a ejecutar la instrucción al crear el objeto.
 
  
## <a name="deprecation-notice"></a>Aviso sobre elementos desusados  
 Las instrucciones CREATE SCHEMA que no especifiquen ningún nombre de esquema se admiten actualmente por razones de compatibilidad con versiones anteriores. Estas instrucciones no crean realmente un esquema dentro de la base de datos, sino que crean tablas y vistas, y conceden permisos. Las entidades de seguridad no necesitan el permiso CREATE SCHEMA para ejecutar esta forma anterior de CREATE SCHEMA, ya que en realidad no se crea ningún esquema. Esta funcionalidad se quitará de una próxima versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CREATE SCHEMA en la base de datos.  
  
 Para crear un objeto especificado dentro de la instrucción CREATE SCHEMA, el usuario debe tener el permiso CREATE correspondiente.  
  
 Para especificar otro usuario como el propietario del esquema que se está creando, el autor de la llamada debe tener el permiso IMPERSONATE sobre ese usuario. Si se especifica un rol de base de datos como propietario, el autor de la llamada debe pertenecer al rol o debe tener el permiso ALTER para el rol.  
  
> [!NOTE]  
>  En la sintaxis compatible con las versiones anteriores, no se comprueban los permisos CREATE SCHEMA porque no se está creando ningún esquema.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-schema-and-granting-permissions"></a>A. Crear un esquema y conceder permisos  
 En el ejemplo siguiente se crea el esquema `Sprockets`, que es propiedad de `Annik` y contiene la tabla `NineProngs`. La instrucción concede el permiso `SELECT` a `Mandar` y deniega el permiso `SELECT` a `Prasanna`. Tenga en cuenta que `Sprockets` y `NineProngs` se crean en una sola instrucción.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE SCHEMA Sprockets AUTHORIZATION Annik  
    CREATE TABLE NineProngs (source int, cost int, partnumber int)  
    GRANT SELECT ON SCHEMA::Sprockets TO Mandar  
    DENY SELECT ON SCHEMA::Sprockets TO Prasanna;  
GO   
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-creating-a-schema-and-a-table-in-the-schema"></a>B. Crear un esquema y una tabla en el esquema  
 En el siguiente ejemplo se crea un esquema `Sales` y, luego, una tabla `Sales.Region` en ese esquema.  
  
```sql  
CREATE SCHEMA Sales;  
GO
  
CREATE TABLE Sales.Region   
(Region_id INT NOT NULL,  
Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
```  
  
### <a name="c-setting-the-owner-of-a-schema"></a>C. Establecer el propietario de un esquema  
 En el siguiente ejemplo se crea un esquema `Production`, propiedad de `Mary`.  
  
```sql  
CREATE SCHEMA Production AUTHORIZATION [Contoso\Mary];  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md)   
 [DROP SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/drop-schema-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.schemas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)   
 [Crear un esquema de la base de datos](../../relational-databases/security/authentication-access/create-a-database-schema.md)  
  
  


