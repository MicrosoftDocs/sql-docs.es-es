---
description: GRANT (Transact-SQL)
title: GRANT (Transact-SQL)
ms.custom: ''
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GRANT_TSQL
- GRANT
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], GRANT statement
- schema-level securables [SQL Server]
- GRANT statement
- cross-database permissions
- GRANT statement, about GRANT statement
- server-level securables [SQL Server]
- database-level securables [SQL Server]
- permissions [SQL Server], granting
ms.assetid: a760c16a-4d2d-43f2-be81-ae9315f38185
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cb24af4f9740f5471a05e17898969a9f39624822
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180195"
---
# <a name="grant-transact-sql"></a>GRANT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Concede permisos sobre un elemento protegible a una entidad de seguridad.  El concepto general es CONCEDER \<some permission> EN \<some object> A \<some user, login, or group>. Para ver conceptos generales sobre los permisos, vea [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md).  
  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for GRANT  
GRANT { ALL [ PRIVILEGES ] }  
      | permission [ ( column [ ,...n ] ) ] [ ,...n ]  
      [ ON [ class :: ] securable ] TO principal [ ,...n ]   
      [ WITH GRANT OPTION ] [ AS principal ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
GRANT
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]
    TO principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
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

ALL  
Esta opción ha quedado desusada y solo se mantiene por razones de compatibilidad con versiones anteriores. No concede todos los permisos posibles. Conceder ALL es equivalente a conceder los siguientes permisos:

  - Si el elemento protegible es una base de datos, ALL significa BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE y CREATE VIEW.  
  
  - Si el elemento protegible es una función escalar, ALL significa EXECUTE y REFERENCES.  
  
  - Si el elemento protegible es una función con valores de tabla, ALL significa DELETE, INSERT, REFERENCES, SELECT y UPDATE.  
  
  - Si el elemento protegible es un procedimiento almacenado, ALL significa EXECUTE.  
  
  - Si el elemento protegible es una tabla, ALL significa DELETE, INSERT, REFERENCES, SELECT y UPDATE.  
  
  - Si el elemento protegible es una vista, ALL significa DELETE, INSERT, REFERENCES, SELECT y UPDATE.  

PRIVILEGES  
Incluido por compatibilidad con ISO. No cambia el comportamiento de ALL.  
  
*permission*  
Es el nombre de un permiso. Las asignaciones de permisos válidas a elementos protegibles se describen en los subtemas que se muestran a continuación.  
  
*column*  
Especifica el nombre de una columna de una tabla en la que se van a conceder los permisos. Los paréntesis () son obligatorios.  
  
*class*  
Especifica la clase del elemento protegible en el que se va a conceder el permiso. El calificador de ámbito **::** es obligatorio.  
  
*securable*  
Especifica el elemento protegible para el que se va a conceder el permiso.  
  
TO *principal*  
Es el nombre de una entidad de seguridad. Las entidades de seguridad a las que se pueden conceder permisos para un elemento protegible varían según el elemento protegible. Vea los subtemas enumerados a continuación para comprobar las combinaciones válidas.  
  
GRANT OPTION  
Indica que el receptor también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
AS *principal*  
Use la cláusula AS de la entidad de seguridad para indicar que la entidad de seguridad registrada como el otorgante del permiso debe ser una entidad de seguridad distinta de la persona que ejecuta la instrucción. Por ejemplo, suponga que la usuaria María tiene el principal_id 12 y el usuario Raúl tiene el principal_id 15. Mary ejecuta `GRANT SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;`. Ahora, la tabla sys.database_permissions indicará que el grantor_principal_id era 15 (Raul), aunque la instrucción realmente la ejecutó el usuario 13 (Mary).  

Normalmente, el uso de la cláusula AS no se suele recomendar, a menos que necesite definir explícitamente la cadena de permisos. Para más información, vea la sección **Resumen del algoritmo de comprobación de permiso** de [Permisos (motor de base de datos)](../../relational-databases/security/permissions-database-engine.md).

El uso de AS en esta instrucción no implica la capacidad de suplantar a otro usuario.

## <a name="remarks"></a>Observaciones

La sintaxis completa de la instrucción GRANT es compleja. El diagrama de sintaxis anterior se ha simplificado para concentrar la atención en su estructura. La sintaxis completa para conceder permisos para elementos protegibles específicos se describe en los artículos enumerados a continuación.  

Se puede utilizar la instrucción REVOKE para retirar permisos concedidos y la instrucción DENY para evitar que una entidad de seguridad obtenga un permiso específico mediante una instrucción GRANT.  

La concesión de un permiso quita DENY o REVOKE de ese permiso para el elemento protegible especificado. Si se deniega el mismo permiso en un ámbito superior que contiene el elemento protegible, DENY tiene prioridad. No obstante, revocar el permiso concedido en un ámbito superior no tiene prioridad.  

Los permisos de nivel de base de datos se conceden en el ámbito de la base de datos especificada. Si un usuario necesita permisos para objetos de otra base de datos, cree la cuenta del usuario en la otra base de datos o conceda a la cuenta del usuario acceso a la otra base de datos y a la base de datos actual.  

> [!CAUTION]  
> Un permiso DENY de nivel de tabla no tiene prioridad sobre uno GRANT de nivel de columna. Esta incoherencia en la jerarquía de permisos se ha mantenido por motivos de compatibilidad con versiones anteriores. Se eliminará en una próxima versión.  

El procedimiento almacenado del sistema sp_helprotect informa de los permisos sobre un elemento protegible en el nivel de base de datos.  

## <a name="with-grant-option"></a>WITH GRANT OPTION

El **GRANT** ... **WITH GRANT OPTION** especifica que la entidad de seguridad que recibe el permiso tiene capacidad de conceder el permiso especificado a otras cuentas de seguridad. Cuando la entidad de seguridad que recibe el permiso es un rol o un grupo de Windows, la cláusula **AS** se debe usar cuando el permiso del objeto deba concederse a usuarios que no sean miembros del grupo o rol. Como solo un usuario, y no un grupo o rol, puede ejecutar la instrucción **GRANT**, un miembro específico del grupo o rol debe usar la cláusula **AS** para invocar explícitamente el rol o miembro del grupo al conceder el permiso. En el siguiente ejemplo se muestra cómo se usa **WITH GRANT OPTION** cuando se concede permiso a un rol o grupo de Windows.  
  
```sql
-- Execute the following as a database owner  
GRANT EXECUTE ON TestProc TO TesterRole WITH GRANT OPTION;  
EXEC sp_addrolemember TesterRole, User1;  
-- Execute the following as User1  
-- The following fails because User1 does not have the permission as the User1  
GRANT EXECUTE ON TestMe TO User2;  
-- The following succeeds because User1 invokes the TesterRole membership  
GRANT EXECUTE ON TestMe TO User2 AS TesterRole;  
```

## <a name="chart-of-sql-server-permissions"></a>Gráfico de los permisos de SQL Server

Para ver un gráfico de tamaño cartel de todos los permisos del [!INCLUDE[ssDE](../../includes/ssde-md.md)] en formato PDF, vea [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster).  

## <a name="permissions"></a>Permisos

El otorgante del permiso (o la entidad de seguridad especificada con la opción AS) debe tener el permiso con GRANT OPTION, o un permiso superior que implique el permiso que se va a conceder. Si se utiliza la opción AS, se aplican requisitos adicionales. Para obtener más detalles, vea el artículo específico de los elementos protegibles.

Los propietarios de objetos pueden conceder permisos para los objetos que poseen. Las entidades de seguridad con permiso CONTROL sobre un elemento protegible pueden conceder permisos para ese elemento.

Los receptores del permiso CONTROL SERVER como los miembros del rol fijo de servidor sysadmin pueden conceder los permisos sobre cualquier elemento protegible en el servidor. Los receptores del permiso CONTROL para una base de datos, como los miembros del rol fijo de base de datos db_owner, pueden conceder los permisos para cualquier elemento protegible en la base de datos. Los receptores del permiso CONTROL en un esquema pueden conceder los permisos en cualquier objeto del esquema.

## <a name="examples"></a>Ejemplos

En esta tabla se enumeran los elementos protegibles y los artículos en los que se describe la sintaxis específica de estos.  

| Elemento protegible | Sintaxis GRANT|
| ---------| ------ |
|Rol de aplicación|[GRANT &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Assembly|[GRANT &#40;permisos de ensamblado de Transact-SQL&#41;](../../t-sql/statements/grant-assembly-permissions-transact-sql.md)|  
|Clave asimétrica|[GRANT &#40;permisos de clave asimétrica de Transact-SQL&#41;](../../t-sql/statements/grant-asymmetric-key-permissions-transact-sql.md)|  
|Grupo de disponibilidad|[GRANT &#40;permisos de grupos de disponibilidad de Transact-SQL&#41;](../../t-sql/statements/grant-availability-group-permissions-transact-sql.md)|  
|Certificado|[GRANT &#40;permisos de certificado de Transact-SQL&#41;](../../t-sql/statements/grant-certificate-permissions-transact-sql.md)|  
|Contrato|[GRANT &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Base de datos|[GRANT &#40;permisos de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md)|
|Credencial de ámbito de base de datos|[GRANT (credencial de ámbito de base de datos de Transact-SQL)](../../t-sql/statements/grant-database-scoped-credential-transact-sql.md)|  
|Punto de conexión|[GRANT &#40;permisos de punto de conexión de Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)|  
|Catálogo de texto completo|[GRANT &#40;permisos de texto completo de Transact-SQL&#41;](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|Lista de palabras irrelevantes de texto completo|[GRANT &#40;permisos de texto completo de Transact-SQL&#41;](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|Función|[Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Inicio de sesión|[GRANT &#40;permisos de entidad de seguridad de servidor de Transact-SQL&#41;](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)|  
|Tipo de mensaje|[GRANT &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Object|[Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Cola|[Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Enlace de servicio remoto|[GRANT &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Role|[GRANT &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Enrutar|[GRANT &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Schema|[GRANT &#40;permisos de esquema de Transact-SQL&#41;](../../t-sql/statements/grant-schema-permissions-transact-sql.md)|  
|Lista de propiedades de búsqueda|[GRANT &#40;permisos de lista de propiedades de búsqueda de Transact-SQL&#41;](../../t-sql/statements/grant-search-property-list-permissions-transact-sql.md)|  
|Server|[GRANT &#40;permisos de servidor de Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md)|  
|Servicio|[GRANT &#40;permisos de Service Broker de Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Procedimiento almacenado|[Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Clave simétrica|[GRANT &#40;permisos de clave simétrica de Transact-SQL&#41;](../../t-sql/statements/grant-symmetric-key-permissions-transact-sql.md)|  
|Synonym (Sinónimo)|[Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Objetos de sistema|[Permisos de objeto de sistema GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)|  
|Tabla|[Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Tipo|[GRANT &#40;permisos de tipo de Transact-SQL&#41;](../../t-sql/statements/grant-type-permissions-transact-sql.md)|  
|Usuario|[GRANT &#40;permisos de entidad de seguridad de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Ver|[Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Colección de esquemas XML|[GRANT &#40;permisos de colección de esquemas XML de Transact-SQL&#41;](../../t-sql/statements/grant-xml-schema-collection-permissions-transact-sql.md)|  

## <a name="see-also"></a>Consulte también

- [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)
- [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)
- [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)
- [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)
- [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)
- [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)
- [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)
- [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)
