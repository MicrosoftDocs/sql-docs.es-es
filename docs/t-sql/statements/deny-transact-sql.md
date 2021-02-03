---
description: DENY (Transact-SQL)
title: DENY (Transact-SQL)
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DENY
- DENY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- schema-level securables [SQL Server]
- security [SQL Server], denying access
- DENY statement
- permissions [SQL Server], denying
- server-level securables [SQL Server]
- inherited security settings [SQL Server]
- denying permissions [SQL Server], DENY statement
- principal security [SQL Server]
- database-level securables [SQL Server]
- denying permissions [SQL Server]
ms.assetid: c32d1e01-9ee9-4665-a516-fcfece58078e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0c07bd840b819a6bb25624c14584089fdcfc2ac8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159358"
---
# <a name="deny-transact-sql"></a>DENY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Deniega un permiso a una entidad de seguridad. Evita que la entidad de seguridad herede permisos por su pertenencia a grupos o roles. DENY tiene prioridad sobre todos los permisos, salvo que DENY no se aplique a los propietarios de objetos o miembros del rol fijo de servidor sysadmin.
  **Nota de seguridad** No se pueden denegar permisos a los miembros del rol fijo de servidor sysadmin y a los propietarios de objetos.
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for DENY  
DENY   { ALL [ PRIVILEGES ] } 
     | <permission>  [ ( column [ ,...n ] ) ] [ ,...n ]  
    [ ON [ <class> :: ] securable ] 
    TO principal [ ,...n ]   
    [ CASCADE] [ AS principal ]  
[;]

<permission> ::=  
{ see the tables below }  
  
<class> ::=  
{ see the tables below }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DENY   
    <permission> [ ,...n ]  
    [ ON [ <class_> :: ] securable ]   
    TO principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class> ::=  
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
 ALL  
 Esta opción no deniega todos los permisos posibles. Al denegar ALL se deniegan los permisos siguientes.  
  
-   Si el elemento protegible es una base de datos, ALL significa BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE y CREATE VIEW.  
  
-   Si el elemento protegible es una función escalar, ALL significa EXECUTE y REFERENCES.  
  
-   Si el elemento protegible es una función con valores de tabla, ALL significa DELETE, INSERT, REFERENCES, SELECT y UPDATE.  
  
-   Si el elemento protegible es un procedimiento almacenado, ALL significa EXECUTE.  
  
-   Si el elemento protegible es una tabla, ALL significa DELETE, INSERT, REFERENCES, SELECT y UPDATE.  
  
-   Si el elemento protegible es una vista, ALL significa DELETE, INSERT, REFERENCES, SELECT y UPDATE.  
  
> [!NOTE]  
>  La sintaxis de DENY ALL está desusada. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, deniegue permisos concretos.  
  
 PRIVILEGES  
 Incluido por compatibilidad con ISO. No cambia el comportamiento de ALL.  
  
 *permission*  
 Es el nombre de un permiso. Las asignaciones de permisos válidas a elementos protegibles se describen en los subtemas que se muestran a continuación.  
  
 *column*  
 Especifica el nombre de una columna de una tabla para la que se deniegan los permisos. Los paréntesis () son obligatorios.  
  
 *class*  
 Especifica la clase de elemento protegible para el que se deniega el permiso. El calificador de ámbito **::** es obligatorio.  
  
 *securable*  
 Especifica el elemento protegible para el que se deniega el permiso.  
  
 TO *principal*  
 Es el nombre de una entidad de seguridad. Las entidades de seguridad para las que se pueden denegar permisos sobre un elemento protegible varían en función de este elemento protegible. Vea los temas sobre elementos protegibles enumerados más abajo para obtener combinaciones válidas.  
  
 CASCADE  
 Indica que el permiso se deniega para la entidad de seguridad especificada y para el resto de entidades de seguridad a las que ésta concedió el permiso. Es obligatorio cuando la entidad de seguridad tiene el permiso con GRANT OPTION.  
  
 AS *principal*  
 Especifica la entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de denegar el permiso.
Use la cláusula AS de la entidad de seguridad para indicar que la entidad de seguridad registrada como el denegador del permiso debe ser una entidad de seguridad distinta de la persona que ejecuta la instrucción. Por ejemplo, suponga que la usuaria María tiene el principal_id 12 y el usuario Raúl tiene el principal_id 15. María ejecuta `DENY SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;`. Ahora bien, la tabla sys.database_permissions indicará que grantor_principal_id de la instrucción DENY fue 15 (Raul), aunque la instrucción realmente la ejecutó el usuario 13 (María).
  
El uso de AS en esta instrucción no implica la capacidad de suplantar a otro usuario.  
  
## <a name="remarks"></a>Observaciones  
 La sintaxis completa de la instrucción DENY es compleja. El diagrama de sintaxis anterior se ha simplificado para concentrar la atención en su estructura. La sintaxis completa para denegar permisos sobre elementos protegibles específicos se describe en los temas enumerados más adelante.  
  
 DENY producirá un error si CASCADE no se especifica al denegar un permiso a una entidad de seguridad a la que se concedió ese permiso con GRANT OPTION.  
  
 El procedimiento almacenado del sistema sp_helprotect informa de los permisos sobre un elemento protegible en el nivel de base de datos.  
  
> [!CAUTION]  
>  Un permiso DENY de nivel de tabla no tiene prioridad sobre uno GRANT de nivel de columna. Esta incoherencia en la jerarquía de permisos se ha mantenido por motivos de compatibilidad con versiones anteriores. Se eliminará en una próxima versión.  
  
> [!CAUTION]  
>  Si se deniega el permiso CONTROL para una base de datos, se deniega implícitamente el permiso CONNECT para la misma. Una entidad de seguridad a la que se deniega el permiso CONTROL para una base de datos no podrá conectarse a esa base de datos.  
  
> [!CAUTION]  
>  Si se deniega el permiso CONTROL SERVER, se deniega implícitamente el permiso CONNECT SQL para el servidor. Una entidad de seguridad a la que se deniega el permiso CONTROL SERVER para un servidor no podrá conectarse a ese servidor.  
  
## <a name="permissions"></a>Permisos  
 El autor de la llamada (o la entidad de seguridad especificada en la opción AS) debe tener el permiso CONTROL sobre el elemento protegible o un permiso superior que implique el permiso CONTROL sobre el elemento protegible. Si utiliza la opción AS, la entidad de seguridad especificada debe poseer el elemento protegible sobre el que se deniega un permiso.  
  
 Los beneficiarios del permiso CONTROL SERVER, por ejemplo, los miembros del rol fijo de servidor sysadmin, pueden denegar cualquier permiso sobre cualquier elemento protegible en el servidor. Los beneficiarios del permiso CONTROL en la base de datos, por ejemplo, los miembros del rol fijo de base de datos db_owner, pueden denegar cualquier permiso sobre cualquier elemento protegible en la base de datos. Los beneficiarios del permiso CONTROL sobre un esquema pueden denegar cualquier permiso sobre cualquier objeto en el esquema. Si se usa la cláusula AS, la entidad de seguridad especificada debe ser propietaria del elemento protegible cuyos permisos que se van a denegar.  
  
## <a name="examples"></a>Ejemplos

 En esta tabla se enumeran los elementos protegibles y los temas donde se describe la sintaxis específica de los mismos.  
  
|Elementos protegibles|Sintaxis|
|----------|------|
|Rol de aplicación|[DENY &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Assembly|[DENY &#40;permisos de ensamblado de Transact-SQL&#41;](../../t-sql/statements/deny-assembly-permissions-transact-sql.md)|  
|Clave asimétrica|[DENY &#40;permisos de clave asimétrica de Transact-SQL&#41;](../../t-sql/statements/deny-asymmetric-key-permissions-transact-sql.md)|  
|Grupo de disponibilidad|[DENY &#40;permisos de grupo de disponibilidad de Transact-SQL&#41;](../../t-sql/statements/deny-availability-group-permissions-transact-sql.md)|  
|Certificado|[DENY &#40;permisos de certificado de Transact-SQL&#41;](../../t-sql/statements/deny-certificate-permissions-transact-sql.md)|  
|Contrato|[DENY &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Base de datos|[DENY &#40;permisos de base de datos de Transact-SQL&#41;](../../t-sql/statements/deny-database-permissions-transact-sql.md)|  
|Credencial de ámbito de base de datos|[DENY (credencial de ámbito de base de datos de Transact-SQL)](../../t-sql/statements/deny-database-scoped-credential-transact-sql.md)|  
|Punto de conexión|[DENY &#40;permisos de extremo de Transact-SQL&#41;](../../t-sql/statements/deny-endpoint-permissions-transact-sql.md)|  
|Catálogo de texto completo|[DENY &#40;permisos de texto completo de Transact-SQL&#41;](../../t-sql/statements/deny-full-text-permissions-transact-sql.md)|  
|Lista de palabras irrelevantes de texto completo|[DENY &#40;permisos de texto completo de Transact-SQL&#41;](../../t-sql/statements/deny-full-text-permissions-transact-sql.md)|  
|Función|[DENY &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Inicio de sesión|[DENY &#40;permisos de entidad de seguridad de servidor de Transact-SQL&#41;](../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)|  
|Tipo de mensaje|[DENY &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Object|[DENY &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Cola|[DENY &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Enlace de servicio remoto|[DENY &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Role|[DENY &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Enrutar|[DENY &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Schema|[DENY &#40;permisos de esquema de Transact-SQL&#41;](../../t-sql/statements/deny-schema-permissions-transact-sql.md)|  
|Lista de propiedades de búsqueda|[GRANT &#40;permisos de lista de propiedades de búsqueda de Transact-SQL&#41;](../../t-sql/statements/deny-search-property-list-permissions-transact-sql.md)|  
|Server|[DENY &#40;permisos de servidor de Transact-SQL&#41;](../../t-sql/statements/deny-server-permissions-transact-sql.md)|  
|Servicio|[DENY &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Procedimiento almacenado|[DENY &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Clave simétrica|[DENY &#40;permisos de clave simétrica de Transact-SQL&#41;](../../t-sql/statements/deny-symmetric-key-permissions-transact-sql.md)|  
|Synonym (Sinónimo)|[DENY &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Objetos de sistema|[DENY &#40;permisos de objeto de sistema de Transact-SQL&#41;](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)|  
|Tabla|[DENY &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Tipo|[DENY &#40;permisos de tipo de Transact-SQL&#41;](../../t-sql/statements/deny-type-permissions-transact-sql.md)|  
|Usuario|[DENY &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Ver|[DENY &#40;permisos de objeto de Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Colección de esquemas XML|[DENY &#40;permisos de colección de esquemas XML de Transact-SQL&#41;](../../t-sql/statements/deny-xml-schema-collection-permissions-transact-sql.md)|  
  
## <a name="see-also"></a>Consulte también  
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)  
  
  
