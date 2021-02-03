---
description: REVOKE (Transact-SQL)
title: REVOKE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REVOKE_TSQL
- REVOKE
dev_langs:
- TSQL
helpviewer_keywords:
- schema-level securables [SQL Server]
- REVOKE statement, Transact-SQL syntax
- removing permissions
- server-level securables [SQL Server]
- deleting permissions
- revoking permissions [SQL Server]
- REVOKE statement
- denying permissions [SQL Server], removing
- database-level securables [SQL Server]
- granting permissions [SQL Server], removing
- permissions [SQL Server], revoking
- dropping permissions
ms.assetid: 9d31d3e7-0883-45cd-bf0e-f0361bbb0956
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: db5c82465b97d1425fb68ba42fffb799355d32c8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206552"
---
# <a name="revoke-transact-sql"></a>REVOKE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Quita un permiso concedido o denegado previamente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for REVOKE  
REVOKE [ GRANT OPTION FOR ]  
      {   
        [ ALL [ PRIVILEGES ] ]  
        |  
                permission [ ( column [ ,...n ] ) ] [ ,...n ]  
      }  
      [ ON [ class :: ] securable ]   
      { TO | FROM } principal [ ,...n ]   
      [ CASCADE] [ AS principal ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
REVOKE   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    [ FROM | TO ] principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class_type> ::=  
{  
      LOGIN  
    | DATABASE  
    | OBJECT  
    | ROLE  
    | SCHEMA  
    | USER  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 GRANT OPTION FOR  
 Indica que se revocará la capacidad de conceder el permiso especificado. Se requiere cuando se utiliza el argumento CASCADE.  
  
> [!IMPORTANT]  
>  Si la entidad de seguridad dispone del permiso especificado sin la opción GRANT, se revocará el permiso.  
  
 ALL  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
 Esta opción no revoca todos los permisos posibles. Revocar ALL es equivalente a revocar los siguientes permisos.  
  
-   Si el elemento protegible es una base de datos, ALL significa BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE y CREATE VIEW.  
  
-   Si el elemento protegible es una función escalar, ALL significa EXECUTE y REFERENCES.  
  
-   Si el elemento protegible es una función con valores de tabla, ALL significa DELETE, INSERT, REFERENCES, SELECT y UPDATE.  
  
-   Si el elemento protegible es un procedimiento almacenado, ALL significa EXECUTE.  
  
-   Si el elemento protegible es una tabla, ALL significa DELETE, INSERT, REFERENCES, SELECT y UPDATE.  
  
-   Si el elemento protegible es una vista, ALL significa DELETE, INSERT, REFERENCES, SELECT y UPDATE.  
  
> [!NOTE]  
>  La sintaxis de REVOKE ALL ha quedado desusada. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, revoque permisos concretos.  
  
 PRIVILEGES  
 Incluido por compatibilidad con ISO. No cambia el comportamiento de ALL.  
  
 *permission*  
 Es el nombre de un permiso. Las asignaciones válidas de permisos a elementos protegibles se describen en los temas de la sección [Sintaxis específica de los elementos protegibles](#securable), más adelante en este tema.  
  
 *column*  
 Especifica el nombre de una columna de una tabla en la que se van a revocar los permisos. Es obligatorio utilizar paréntesis.  
  
 *class*  
 Especifica la clase del elemento protegible para el que se va a revocar el permiso. El calificador de ámbito **::** es obligatorio.  
  
 *securable*  
 Especifica el elemento protegible para el que se va a revocar el permiso.  
  
 TO | FROM *principal*  
 Es el nombre de una entidad de seguridad. Las entidades de seguridad de las que se pueden revocar permisos para un elemento protegible varían según el elemento. Para más información sobre las combinaciones válidas, vea los temas que se muestran en [Sintaxis específica de los elementos protegibles](#securable) más adelante en este tema.  
  
 CASCADE  
 Indica que el permiso que se va a revocar también se revocará de otras entidades de seguridad a las que se han concedido permisos por esta entidad de seguridad. Cuando se utiliza el argumento CASCADE, también se debe incluir el argumento GRANT OPTION FOR.  
  
> [!CAUTION]  
>  Una revocación en cascada de un permiso concedido WITH GRANT OPTION revocará tanto GRANT como DENY de dicho permiso.  
  
 AS *principal*  
 Use la cláusula AS principal para indicar que se va a revocar un permiso que ha sido concedido por una entidad de seguridad distinta de usted. Por ejemplo, imagine que la usuaria María tiene el valor principal_id 12 y el usuario Raúl tiene el valor principal_id 15. Tanto María como Raúl conceden el mismo permiso a un usuario llamado Carlos. La tabla sys.database_permissions indicará los permisos dos veces, pero cada uno tendrá un valor de grantor_principal_id diferente. María podría revocar el permiso mediante la cláusula `AS RAUL` para quitar la concesión del permiso de Raúl.
 
El uso de AS en esta instrucción no implica la capacidad de suplantar a otro usuario.  
  
## <a name="remarks"></a>Observaciones  
 La sintaxis completa de la instrucción REVOKE es compleja. El diagrama de sintaxis anterior se ha simplificado para concentrar la atención en su estructura. La sintaxis completa para revocar permisos en elementos protegibles específicos se describe en los temas que se muestran en [Sintaxis específica de los elementos protegibles](#securable) más adelante en este tema.  
  
 Se puede utilizar la instrucción REVOKE para retirar permisos concedidos y la instrucción DENY para evitar que una entidad de seguridad obtenga un permiso específico mediante una instrucción GRANT.  
  
 La concesión de un permiso quita DENY o REVOKE de ese permiso para el elemento protegible especificado. Si se deniega el mismo permiso en un ámbito superior que contiene el elemento protegible, DENY tiene prioridad. Pero tenga en cuenta que revocar el permiso concedido en un ámbito superior no tiene prioridad.  
  
> [!CAUTION]  
>  Un permiso DENY de nivel de tabla no tiene prioridad sobre uno GRANT de nivel de columna. Se ha conservado esta incoherencia en la jerarquía de permisos para mantener la compatibilidad con versiones anteriores. Se eliminará en una próxima versión.  
  
 El procedimiento almacenado del sistema sp_helprotect informa de los permisos sobre un elemento protegible en el nivel de base de datos.  
  
 La instrucción REVOKE generará errores si no se especifica CASCADE al revocar un permiso de una entidad de seguridad que tenía concedido ese permiso con GRANT OPTION especificada.  
  
## <a name="permissions"></a>Permisos  
 Las entidades de seguridad con el permiso CONTROL para un elemento protegible pueden revocar permisos para ese elemento. Los propietarios de objetos pueden revocar permisos en los objetos que poseen.  
  
 Los receptores del permiso CONTROL SERVER, como los miembros del rol fijo de servidor sysadmin, pueden revocar los permisos en cualquier elemento protegible en el servidor. Los receptores del permiso CONTROL en una base de datos, como los miembros del rol fijo de base de datos db_owner, pueden revocar los permisos en cualquier elemento protegible en la base de datos. Los receptores del permiso CONTROL en un esquema pueden revocar los permisos en cualquier objeto del esquema.  
  
##  <a name="securable-specific-syntax"></a><a name="securable"></a> Sintaxis específica de los elementos protegibles  
 En esta tabla se enumeran los elementos protegibles y los temas donde se describe la sintaxis específica de los mismos.  
  
|Elemento protegible|Tema|  
|---------------|-----------|  
|Rol de aplicación|[REVOKE &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|Assembly|[REVOKE &#40;permisos de ensamblado de Transact-SQL&#41;](../../t-sql/statements/revoke-assembly-permissions-transact-sql.md)|  
|Clave asimétrica|[REVOKE &#40;permisos de clave asimétrica de Transact-SQL&#41;](../../t-sql/statements/revoke-asymmetric-key-permissions-transact-sql.md)|  
|Grupo de disponibilidad|[REVOKE Availability Group Permissions &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-availability-group-permissions-transact-sql.md) [REVOKE &#40;permisos de grupos de disponibilidad de Transact-SQL&#41;]|  
|Certificado|[REVOKE &#40;permisos de certificado de Transact-SQL&#41;](../../t-sql/statements/revoke-certificate-permissions-transact-sql.md)|  
|Contrato|[REVOKE &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Base de datos|[REVOKE &#40;permisos de base de datos de Transact-SQL&#41;](../../t-sql/statements/revoke-database-permissions-transact-sql.md)|  
|Punto de conexión|[REVOKE &#40;permisos de punto de conexión de Transact-SQL&#41;](../../t-sql/statements/revoke-endpoint-permissions-transact-sql.md)|  
|Credencial de ámbito de base de datos|[REVOKE (credencial de ámbito de base de datos de Transact-SQL)](../../t-sql/statements/revoke-database-scoped-credential-transact-sql.md)|  
|Catálogo de texto completo|[REVOKE &#40;permisos de texto completo de Transact-SQL&#41;](../../t-sql/statements/revoke-full-text-permissions-transact-sql.md)|  
|Lista de palabras irrelevantes de texto completo|[REVOKE &#40;permisos de texto completo de Transact-SQL&#41;](../../t-sql/statements/revoke-full-text-permissions-transact-sql.md)|  
|Función|[REVOKE &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Iniciar sesión|[REVOKE &#40;permisos de entidad de seguridad de servidor de Transact-SQL&#41;](../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)|  
|Tipo de mensaje|[REVOKE &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Object|[REVOKE &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Cola|[REVOKE &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Enlace de servicio remoto|[REVOKE &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Rol|[REVOKE &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|Ruta|[REVOKE &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Schema|[REVOKE &#40;permisos de esquema de Transact-SQL&#41;](../../t-sql/statements/revoke-schema-permissions-transact-sql.md)|  
|Lista de propiedades de búsqueda|[REVOKE &#40;permisos de la lista de propiedades de búsqueda de Transact-SQL&#41;](../../t-sql/statements/revoke-search-property-list-permissions-transact-sql.md)|  
|Servidor|[REVOKE Server Permissions &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md) [REVOKE (permisos de servidor de Transact-SQL)]|  
|Servicio|[REVOKE &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Procedimiento almacenado|[REVOKE &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Clave simétrica|[REVOKE &#40;permisos de clave simétrica de Transact-SQL&#41;](../../t-sql/statements/revoke-symmetric-key-permissions-transact-sql.md)|  
|Synonym (Sinónimo)|[REVOKE &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Objetos de sistema|[REVOKE &#40;permisos de objeto de sistema de Transact-SQL&#41;](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md)|  
|Tabla|[REVOKE &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Tipo|[REVOKE &#40;permisos de tipo de Transact-SQL&#41;](../../t-sql/statements/revoke-type-permissions-transact-sql.md)|  
|Usuario|[REVOKE &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|Ver|[REVOKE &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|Colección de esquemas XML|[REVOKE &#40;permisos de colección de esquemas XML de Transact-SQL&#41;](../../t-sql/statements/revoke-xml-schema-collection-permissions-transact-sql.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)  
  
  
