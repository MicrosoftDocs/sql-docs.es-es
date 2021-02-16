---
title: Permisos (motor de base de datos) | Microsoft Docs
description: Consulte esta lista completa de permisos de SQL Server para saber cuáles se aplican a las plataformas que usa.
ms.custom: ''
ms.date: 10/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseuser.permissions.database.f1--May use common.permissions
- sql13.swb.databaseuser.permissions.object.f1--May use common.permissions
helpviewer_keywords:
- REFERENCES permission
- permissions [SQL Server]
- security [SQL Server], permissions
- naming conventions [SQL Server]
ms.assetid: f28e3dea-24e6-4a81-877b-02ec4c7e36b9
author: AndreasWolter
ms.author: anwolter
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b716f1daa936aa700bd28984ba71d0e9bfc1d2df
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100341689"
---
# <a name="permissions-database-engine"></a>Permisos (motor de base de datos)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Todos los elementos protegibles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tienen permisos asociados que se pueden conceder a una entidad de seguridad. Los permisos de [!INCLUDE[ssDE](../../includes/ssde-md.md)] se administran en el nivel de servidor asignados a los inicios de sesión y roles de servidor, y en el nivel de base de datos asignados a usuarios de base de datos y roles base de datos. El modelo para [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] tiene el mismo sistema para los permisos de base de datos, pero los permisos de nivel de servidor no están disponibles. Este tema contiene una lista completa de los permisos. Para una implementación típica de los permisos, consulte [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
El número total de permisos para [!INCLUDE[ssSQLv15_md](../../includes/sssql19-md.md)] es 248. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] expone 254 permisos. La mayoría de los permisos se aplica a todas las plataformas, pero otros no. Por ejemplo, no se pueden conceder permisos de nivel de servidor en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y algunos permisos solo tienen sentido en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
Con la nueva versión se van a implementar nuevos permisos gradualmente. [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] expuso 238 permisos. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] expuso 230 permisos. [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] expuso 219 permisos. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] expuso 214 permisos. [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] expuso 195 permisos. El tema [sys.fn_builtin_permissions](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md) especifica los permisos de las últimas versiones que son nuevos.

Una vez que comprenda los permisos, aplique los permisos de nivel de servidor a los inicios de sesión y usuarios de permisos de nivel de base de datos con las instrucciones [GRANT](../../t-sql/statements/grant-transact-sql.md), [REVOKE](../../t-sql/statements/revoke-transact-sql.md)y [DENY](../../t-sql/statements/deny-transact-sql.md) . Por ejemplo:   
```sql
GRANT SELECT ON OBJECT::HumanResources.Employee TO Larry;
REVOKE SELECT ON OBJECT::HumanResources.Employee TO Larry;
```   
Para obtener consejos acerca de cómo planificar un sistema de permisos, consulte [Introducción a los permisos de los motores de bases de datos](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).
  
##  <a name="permissions-naming-conventions"></a><a name="_conventions"></a> Convenciones de nomenclatura de permisos  
 A continuación se describen las convenciones generales que se siguen en la nomenclatura de permisos:  
  
-   CONTROL  
  
     Confiere al receptor del permiso capacidades relacionadas con la propiedad. El receptor del permiso dispone de hecho de todos los permisos definidos para el elemento protegible. Una entidad de seguridad a la que se le haya concedido el permiso CONTROL también puede conceder permisos para el elemento protegible. Como el modelo de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es jerárquico, el permiso CONTROL de un determinado ámbito incluye implícitamente el mismo permiso CONTROL para todos los elementos protegibles que abarca dicho ámbito. Por ejemplo, el permiso CONTROL en una base de datos implica todos los permisos de la base de datos, todos los permisos en todos los ensamblados y todos los esquemas de la misma, así como todos los permisos en los objetos de todos los esquemas que incluye la base de datos.  
  
-   ALTER  
  
     Confiere la posibilidad de cambiar las propiedades, excepto la propiedad, de un elemento protegible determinado. Cuando se concede para un ámbito, ALTER también confiere la posibilidad de modificar, crear o quitar cualquier elemento protegible que esté contenido en ese ámbito. Por ejemplo, el permiso ALTER en un esquema incluye la posibilidad de crear, modificar y quitar objetos del esquema.  
  
-   ALTER ANY \<*Server Securable*>, donde *Server Securable* puede ser cualquier elemento protegible del servidor.  
  
     Confiere la posibilidad de crear, modificar o quitar instancias individuales del *Protegible del servidor*. Por ejemplo, ALTER ANY LOGIN confiere la posibilidad de crear, modificar o quitar cualquier inicio de sesión en la instancia.  
  
-   ALTER ANY \<*Database Securable*>, donde *Database Securable* puede ser cualquier elemento protegible en el nivel de base de datos.  
  
     Confiere la posibilidad de crear (CREATE), modificar (ALTER) o quitar (DROP) instancias individuales del *Protegible de la base de datos*. Por ejemplo, ALTER ANY SCHEMA confiere la posibilidad de crear, modificar o quitar cualquier esquema en la base de datos.  
  
-   TAKE OWNERSHIP  
  
     Permite al receptor del permiso tomar propiedad del elemento protegible para el que se concede este permiso.  
  
-   IMPERSONATE \<*Login*>  
  
     Permite al receptor suplantar el inicio de sesión.  
  
-   IMPERSONATE \<*User*>  
  
     Permite al receptor suplantar al usuario.  
  
-   CREATE \<*Server Securable*>  
  
     Confiere al receptor la posibilidad de crear el *Protegible del servidor*.  
  
-   CREATE \<*Database Securable*>  
  
     Confiere al receptor la posibilidad de crear el *Protegible de la base de datos*.  
  
-   CREATE \<*Schema-contained Securable*>  
  
     Confiere la posibilidad de crear el elemento protegible contenido en el esquema. No obstante, para crear el elemento protegible en un esquema concreto se requiere el permiso ALTER en el esquema.  
  
-   VIEW DEFINITION  
  
     Permite al receptor obtener acceso a los metadatos.  
  
-   REFERENCES  
  
     El permiso REFERENCES es necesario en una tabla para crear una restricción FOREIGN KEY que hace referencia a esa tabla.  
  
     El permiso de REFERENCES es necesario en un objeto para crear FUNCTION o VIEW con la cláusula `WITH SCHEMABINDING` que hace referencia a ese objeto.  
  
## <a name="chart-of-sql-server-permissions"></a>Gráfico de los permisos de SQL Server  
[!INCLUDE[database-engine-permissions](../../includes/paragraph-content/database-engine-permissions.md)]
  
##  <a name="permissions-applicable-to-specific-securables"></a><a name="_securables"></a> Permisos aplicables a elementos protegibles específicos  
 En la siguiente tabla se enumeran los principales tipos de permisos y los tipos de elementos protegibles a los que se pueden aplicar.  
  
|Permiso|Se aplica a|  
|----------------|----------------|  
|ALTER|Todas las clases de objetos excepto TYPE.|  
|CONTROL|Todas las clases de objetos: <br />AGGREGATE,<br />APPLICATION ROLE,<br />ASSEMBLY,<br />ASYMMETRIC KEY,<br />AVAILABILITY GROUP,<br />CERTIFICATE,<br />CONTRACT,<br />CREDENTIALS, DATABASE,<br />DATABASE SCOPED CREDENTIAL,<br /> DEFAULT,<br />ENDPOINT,<br />FULLTEXT CATALOG,<br />FULLTEXT STOPLIST,<br />FUNCTION,<br />LOGIN,<br />MESSAGE TYPE,<br />PROCEDURE,<br />QUEUE, <br />REMOTE SERVICE BINDING,<br />ROLE,<br />ROUTE,<br />RULE,<br />SCHEMA,<br />SEARCH PROPERTY LIST,<br />SERVER,<br />SERVER ROLE,<br />SERVICE,<br />SYMMETRIC KEY,<br />SYNONYM,<br />TABLE,<br />TYPE,<br /> USER,<br />VIEW y<br />XML SCHEMA COLLECTION|  
|Delete|Todas las clases de objetos excepto DATABASE SCOPED CONFIGURATION, SERVER y TYPE.|  
|Ejecute|Tipos de CLR, scripts externos, procedimientos ([!INCLUDE[tsql](../../includes/tsql-md.md)] y CLR), funciones escalares y de agregado ([!INCLUDE[tsql](../../includes/tsql-md.md)] y CLR) y sinónimos|  
|IMPERSONATE|Inicios de sesión y usuarios|  
|INSERT|Sinónimos, tablas y columnas, vistas y columnas. El permiso se puede conceder en el nivel de base de datos, en el de esquema o en el de objeto.|  
|RECEIVE|Colas de[!INCLUDE[ssSB](../../includes/sssb-md.md)]|  
|REFERENCES|AGGREGATE,<br />ASSEMBLY,<br />ASYMMETRIC KEY,<br />CERTIFICATE,<br />CONTRACT,<br />DATABASE,<br />DATABASE SCOPED CREDENTIAL,<br />FULLTEXT CATALOG,<br />FULLTEXT STOPLIST,<br />FUNCTION,<br />MESSAGE TYPE,<br />PROCEDURE,<br />QUEUE, <br />RULE,<br />SCHEMA,<br />SEARCH PROPERTY LIST,<br />SEQUENCE OBJECT, <br />SYMMETRIC KEY,<br />TABLE,<br />TYPE,<br />VIEW y<br />XML SCHEMA COLLECTION|  
|SELECT|Sinónimos, tablas y columnas, vistas y columnas. El permiso se puede conceder en el nivel de base de datos, en el de esquema o en el de objeto.|  
|TAKE OWNERSHIP|Todas las clases de objetos excepto DATABASE SCOPED CONFIGURATION, LOGIN, SERVER y USER.|  
|UPDATE|Sinónimos, tablas y columnas, vistas y columnas. El permiso se puede conceder en el nivel de base de datos, en el de esquema o en el de objeto.|  
|VIEW CHANGE TRACKING|Esquemas y tablas|  
|VIEW DEFINITION|Todas las clases de objetos excepto DATABASE SCOPED CONFIGURATION y SERVER.|  
  
> [!CAUTION]  
>  Los permisos predeterminados que se conceden a objetos del sistema en el momento de la instalación se evalúan detenidamente frente a posibles amenazas y no necesitan modificarse como parte de la protección de la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Los cambios a los permisos de los objetos del sistema podrían limitar o interrumpir la funcionalidad y dejar potencialmente a su instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un estado no admitido.  
  
##  <a name="sql-server-permissions"></a><a name="_permissions"></a> Permisos de SQL Server  
 La tabla siguiente contiene una lista completa de los permisos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Los permisos de[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] solo están disponibles para elementos protegibles de base que se admiten. No se pueden conceder permisos de nivel de servidor en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]; sin embargo, en algunos casos los permisos de base de datos están disponibles en su lugar.  
  
|Elemento protegible base|Permisos granulares del elemento protegible base|Código del tipo de permiso|Elemento protegible que contiene un elemento protegible base|Permiso para el elemento protegible contenedor que implica permiso granular para el elemento protegible base|  
|--------------------|--------------------------------------------|--------------------------|--------------------------------------------|------------------------------------------------------------------------------------------|  
|APPLICATION ROLE|ALTER|AL|DATABASE|ALTER ANY APPLICATION ROLE|  
|APPLICATION ROLE|CONTROL|CL|DATABASE|CONTROL|  
|APPLICATION ROLE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ASSEMBLY|ALTER|AL|DATABASE|ALTER ANY ASSEMBLY|  
|ASSEMBLY|CONTROL|CL|DATABASE|CONTROL|  
|ASSEMBLY|REFERENCES|RF|DATABASE|REFERENCES|  
|ASSEMBLY|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ASSEMBLY|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ASYMMETRIC KEY|ALTER|AL|DATABASE|ALTER ANY ASYMMETRIC KEY|  
|ASYMMETRIC KEY|CONTROL|CL|DATABASE|CONTROL|  
|ASYMMETRIC KEY|REFERENCES|RF|DATABASE|REFERENCES|  
|ASYMMETRIC KEY|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ASYMMETRIC KEY|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|AVAILABILITY GROUP|ALTER|AL|SERVER|ALTER ANY AVAILABILITY GROUP|  
|AVAILABILITY GROUP|CONTROL|CL|SERVER|CONTROL SERVER|  
|AVAILABILITY GROUP|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|AVAILABILITY GROUP|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|CERTIFICADO|ALTER|AL|DATABASE|ALTER ANY CERTIFICATE|  
|CERTIFICADO|CONTROL|CL|DATABASE|CONTROL|  
|CERTIFICADO|REFERENCES|RF|DATABASE|REFERENCES|  
|CERTIFICADO|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|CERTIFICADO|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|CONTRACT|ALTER|AL|DATABASE|ALTER ANY CONTRACT|  
|CONTRACT|CONTROL|CL|DATABASE|CONTROL|  
|CONTRACT|REFERENCES|RF|DATABASE|REFERENCES|  
|CONTRACT|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|CONTRACT|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|DATABASE|ADMINISTER DATABASE BULK OPERATIONS|DABO|SERVER|CONTROL SERVER|
|DATABASE|ALTER|AL|SERVER|ALTER ANY DATABASE|  
|DATABASE|ALTER ANY APPLICATION ROLE|ALAR|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ASSEMBLY|ALAS|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ASYMMETRIC KEY|ALAK|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY CERTIFICATE|ALCF|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY COLUMN ENCRYPTION KEY|ALCK<br /><br /> Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la versión actual), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY COLUMN MASTER KEY|ALCM<br /><br /> Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la versión actual), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY CONTRACT|ALSC|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY DATABASE AUDIT|ALDA|SERVER|ALTER ANY SERVER AUDIT|  
|DATABASE|ALTER ANY DATABASE DDL TRIGGER|ALTG|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY DATABASE EVENT NOTIFICATION|ALED|SERVER|ALTER ANY EVENT NOTIFICATION|  
|DATABASE|ALTER ANY DATABASE EVENT SESSION|AADS<br /><br /> Se aplica a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|ALTER ANY EVENT SESSION|  
|DATABASE|ALTER ANY DATABASE SCOPED CONFIGURATION|ALDC<br /><br /> Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la versión actual), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY DATASPACE|ALDS|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY EXTERNAL DATA SOURCE|AEDS|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY EXTERNAL FILE FORMAT|AEFF|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY FULLTEXT CATALOG|ALFT|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY MASK|AAMK<br /><br /> Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la versión actual), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY MESSAGE TYPE|ALMT|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY REMOTE SERVICE BINDING|ALSB|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ROLE|ALRL|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ROUTE|ALRT|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY SCHEMA|ALSM|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY SECURITY POLICY|ALSP<br /><br /> Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la versión actual), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY SENSITIVITY CLASSIFICATION|ALSP<br />Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQL Server 2019 (15.x) hasta la versión actual), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|DATABASE|CONTROL SERVER|
|DATABASE|ALTER ANY SERVICE|ALSV|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY SYMMETRIC KEY|ALSK|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY USER|ALUS|SERVER|CONTROL SERVER|  
|DATABASE|AUTHENTICATE|AUTH|SERVER|AUTHENTICATE SERVER|  
|DATABASE|BACKUP DATABASE|BADB|SERVER|CONTROL SERVER|  
|DATABASE|BACKUP LOG|BALO|SERVER|CONTROL SERVER|  
|DATABASE|CHECKPOINT|CP|SERVER|CONTROL SERVER|  
|DATABASE|CONNECT|CO|SERVER|CONTROL SERVER|  
|DATABASE|CONNECT REPLICATION|CORP|SERVER|CONTROL SERVER|  
|DATABASE|CONTROL|CL|SERVER|CONTROL SERVER|  
|DATABASE|CREATE AGGREGATE|CRAG|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ASSEMBLY|CRAS|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ASYMMETRIC KEY|CRAK|SERVER|CONTROL SERVER|  
|DATABASE|CREATE CERTIFICATE|CRCF|SERVER|CONTROL SERVER|  
|DATABASE|CREATE CONTRACT|CRSC|SERVER|CONTROL SERVER|  
|DATABASE|CREATE DATABASE|CRDB|SERVER|CREATE ANY DATABASE|  
|DATABASE|CREATE DATABASE DDL EVENT NOTIFICATION|CRED|SERVER|CREATE DDL EVENT NOTIFICATION|  
|DATABASE|CREATE DEFAULT|CRDF|SERVER|CONTROL SERVER|  
|DATABASE|CREATE FULLTEXT CATALOG|CRFT|SERVER|CONTROL SERVER|  
|DATABASE|CREATE FUNCTION|CRFN|SERVER|CONTROL SERVER|  
|DATABASE|CREATE MESSAGE TYPE|CRMT|SERVER|CONTROL SERVER|  
|DATABASE|CREATE PROCEDURE|CRPR|SERVER|CONTROL SERVER|  
|DATABASE|CREATE QUEUE|CRQU|SERVER|CONTROL SERVER|  
|DATABASE|CREATE REMOTE SERVICE BINDING|CRSB|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ROLE|CRRL|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ROUTE|CRRT|SERVER|CONTROL SERVER|  
|DATABASE|CREATE RULE|CRRU|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SCHEMA|CRSM|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SERVICE|CRSV|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SYMMETRIC KEY|CRSK|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SYNONYM|CRSN|SERVER|CONTROL SERVER|  
|DATABASE|CREATE TABLE|CRTB|SERVER|CONTROL SERVER|  
|DATABASE|CREATE TYPE|CRTY|SERVER|CONTROL SERVER|  
|DATABASE|CREATE VIEW|CRVW|SERVER|CONTROL SERVER|  
|DATABASE|CREATE XML SCHEMA COLLECTION|CRXS|SERVER|CONTROL SERVER|  
|DATABASE|Delete|DL|SERVER|CONTROL SERVER|  
|DATABASE|Ejecute|EX|SERVER|CONTROL SERVER|  
|DATABASE|EXECUTE ANY EXTERNAL SCRIPT|EAES<br /><br /> Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la versión actual).|SERVER|CONTROL SERVER|  
|DATABASE|INSERT|IN|SERVER|CONTROL SERVER|  
|DATABASE|KILL DATABASE CONNECTION|KIDC<br /><br /> Solo se aplica a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Use ALTER ANY CONNECTION en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|SERVER|ALTER ANY CONNECTION|  
|DATABASE|REFERENCES|RF|SERVER|CONTROL SERVER|  
|DATABASE|SELECT|SL|SERVER|CONTROL SERVER|  
|DATABASE|SHOWPLAN|SPLN|SERVER|ALTER TRACE|  
|DATABASE|SUBSCRIBE QUERY NOTIFICATIONS|SUQN|SERVER|CONTROL SERVER|  
|DATABASE|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|DATABASE|UNMASK|UMSK<br /><br /> Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la versión actual), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|CONTROL SERVER|  
|DATABASE|UPDATE|UP|SERVER|CONTROL SERVER|  
|DATABASE|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|VWCK<br /><br /> Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la versión actual), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|VIEW SERVER STATE|  
|DATABASE|VIEW ANY COLUMN MASTER KEY DEFINITION|vWCM<br /><br /> Se aplica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta la versión actual), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|SERVER|VIEW SERVER STATE|  
|DATABASE|VIEW DATABASE STATE|VWDS|SERVER|VIEW SERVER STATE|  
|DATABASE|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|DATABASE SCOPED CREDENTIAL|ALTER|AL|DATABASE|CONTROL|
|DATABASE SCOPED CREDENTIAL|CONTROL|CL|DATABASE|CONTROL|
|DATABASE SCOPED CREDENTIAL|REFERENCES|RF|DATABASE|REFERENCES|
|DATABASE SCOPED CREDENTIAL|TAKE OWNERSHIP|TO|DATABASE|CONTROL|
|DATABASE SCOPED CREDENTIAL|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|
|ENDPOINT|ALTER|AL|SERVER|ALTER ANY ENDPOINT|  
|ENDPOINT|CONNECT|CO|SERVER|CONTROL SERVER|  
|ENDPOINT|CONTROL|CL|SERVER|CONTROL SERVER|  
|ENDPOINT|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|ENDPOINT|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|FULLTEXT CATALOG|ALTER|AL|DATABASE|ALTER ANY FULLTEXT CATALOG|  
|FULLTEXT CATALOG|CONTROL|CL|DATABASE|CONTROL|  
|FULLTEXT CATALOG|REFERENCES|RF|DATABASE|REFERENCES|  
|FULLTEXT CATALOG|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|FULLTEXT CATALOG|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|FULLTEXT STOPLIST|ALTER|AL|DATABASE|ALTER ANY FULLTEXT CATALOG|  
|FULLTEXT STOPLIST|CONTROL|CL|DATABASE|CONTROL|  
|FULLTEXT STOPLIST|REFERENCES|RF|DATABASE|REFERENCES|  
|FULLTEXT STOPLIST|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|FULLTEXT STOPLIST|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|LOGIN|ALTER|AL|SERVER|ALTER ANY LOGIN|  
|LOGIN|CONTROL|CL|SERVER|CONTROL SERVER|  
|LOGIN|IMPERSONATE|IM|SERVER|CONTROL SERVER|  
|LOGIN|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|MESSAGE TYPE|ALTER|AL|DATABASE|ALTER ANY MESSAGE TYPE|  
|MESSAGE TYPE|CONTROL|CL|DATABASE|CONTROL|  
|MESSAGE TYPE|REFERENCES|RF|DATABASE|REFERENCES|  
|MESSAGE TYPE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|MESSAGE TYPE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|OBJECT|ALTER|AL|SCHEMA|ALTER|  
|OBJECT|CONTROL|CL|SCHEMA|CONTROL|  
|OBJECT|Delete|DL|SCHEMA|Delete|  
|OBJECT|Ejecute|EX|SCHEMA|Ejecute|  
|OBJECT|INSERT|IN|SCHEMA|INSERT|  
|OBJECT|RECEIVE|RC|SCHEMA|CONTROL|  
|OBJECT|REFERENCES|RF|SCHEMA|REFERENCES|  
|OBJECT|SELECT|SL|SCHEMA|SELECT|  
|OBJECT|TAKE OWNERSHIP|TO|SCHEMA|CONTROL|  
|OBJECT|UPDATE|UP|SCHEMA|UPDATE|  
|OBJECT|VIEW CHANGE TRACKING|VWCT|SCHEMA|VIEW CHANGE TRACKING|  
|OBJECT|VIEW DEFINITION|VW|SCHEMA|VIEW DEFINITION|  
|REMOTE SERVICE BINDING|ALTER|AL|DATABASE|ALTER ANY REMOTE SERVICE BINDING|  
|REMOTE SERVICE BINDING|CONTROL|CL|DATABASE|CONTROL|  
|REMOTE SERVICE BINDING|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|REMOTE SERVICE BINDING|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ROLE|ALTER|AL|DATABASE|ALTER ANY ROLE|  
|ROLE|CONTROL|CL|DATABASE|CONTROL|  
|ROLE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ROLE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ROUTE|ALTER|AL|DATABASE|ALTER ANY ROUTE|  
|ROUTE|CONTROL|CL|DATABASE|CONTROL|  
|ROUTE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ROUTE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|SEARCH PROPERTY LIST|ALTER|AL|SERVER|ALTER ANY FULLTEXT CATALOG|  
|SEARCH PROPERTY LIST|CONTROL|CL|SERVER|CONTROL|  
|SEARCH PROPERTY LIST|REFERENCES|RF|SERVER|REFERENCES|  
|SEARCH PROPERTY LIST|TAKE OWNERSHIP|TO|SERVER|CONTROL|  
|SEARCH PROPERTY LIST|VIEW DEFINITION|VW|SERVER|VIEW DEFINITION|  
|SCHEMA|ALTER|AL|DATABASE|ALTER ANY SCHEMA|  
|SCHEMA|CONTROL|CL|DATABASE|CONTROL|  
|SCHEMA|CREATE SEQUENCE|CRSO|DATABASE|CONTROL|  
|SCHEMA|Delete|DL|DATABASE|Delete|  
|SCHEMA|Ejecute|EX|DATABASE|Ejecute|  
|SCHEMA|INSERT|IN|DATABASE|INSERT|  
|SCHEMA|REFERENCES|RF|DATABASE|REFERENCES|  
|SCHEMA|SELECT|SL|DATABASE|SELECT|  
|SCHEMA|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|SCHEMA|UPDATE|UP|DATABASE|UPDATE|  
|SCHEMA|VIEW CHANGE TRACKING|VWCT|DATABASE|VIEW CHANGE TRACKING|  
|SCHEMA|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|SERVER|ADMINISTER BULK OPERATIONS|ADBO|No aplicable|No aplicable|  
|SERVER|ALTER ANY AVAILABILITY GROUP|ALAG|No aplicable|No aplicable|  
|SERVER|ALTER ANY CONNECTION|ALCO|No aplicable|No aplicable|  
|SERVER|ALTER ANY CREDENTIAL|ALCD|No aplicable|No aplicable|  
|SERVER|ALTER ANY DATABASE|ALDB|No aplicable|No aplicable|  
|SERVER|ALTER ANY ENDPOINT|ALHE|No aplicable|No aplicable|  
|SERVER|ALTER ANY EVENT NOTIFICATION|ALES|No aplicable|No aplicable|  
|SERVER|ALTER ANY EVENT SESSION|AAES|No aplicable|No aplicable|  
|SERVER|ALTER ANY LINKED SERVER|ALLS|No aplicable|No aplicable|  
|SERVER|ALTER ANY LOGIN|ALLG|No aplicable|No aplicable|  
|SERVER|ALTER ANY SERVER AUDIT|ALAA|No aplicable|No aplicable|  
|SERVER|ALTER ANY SERVER ROLE|ALSR|No aplicable|No aplicable|  
|SERVER|ALTER RESOURCES|ALRS|No aplicable|No aplicable|  
|SERVER|ALTER SERVER STATE|ALSS|No aplicable|No aplicable|  
|SERVER|ALTER SETTINGS|ALST|No aplicable|No aplicable|  
|SERVER|ALTER TRACE|ALTR|No aplicable|No aplicable|  
|SERVER|AUTHENTICATE SERVER|AUTH|No aplicable|No aplicable|  
|SERVER|CONNECT ANY DATABASE|CADB|No aplicable|No aplicable|  
|SERVER|CONNECT SQL|COSQ|No aplicable|No aplicable|  
|SERVER|CONTROL SERVER|CL|No aplicable|No aplicable|  
|SERVER|CREATE ANY DATABASE|CRDB|No aplicable|No aplicable|  
|SERVER|Crear grupo de disponibilidad|CRAC|No aplicable|No aplicable|  
|SERVER|CREATE DDL EVENT NOTIFICATION|CRDE|No aplicable|No aplicable|  
|SERVER|CREATE ENDPOINT|CRHE|No aplicable|No aplicable|  
|SERVER|CREATE SERVER ROLE|CRSR|No aplicable|No aplicable|  
|SERVER|CREATE TRACE EVENT NOTIFICATION|CRTE|No aplicable|No aplicable|  
|SERVER|EXTERNAL ACCESS ASSEMBLY|XA|No aplicable|No aplicable|  
|SERVER|IMPERSONATE ANY LOGIN|IAL|No aplicable|No aplicable|  
|SERVER|SELECT ALL USER SECURABLES|SUS|No aplicable|No aplicable|  
|SERVER|SHUTDOWN|SHDN|No aplicable|No aplicable|  
|SERVER|UNSAFE ASSEMBLY|XU|No aplicable|No aplicable|  
|SERVER|VIEW ANY DATABASE|VWDB|No aplicable|No aplicable|  
|SERVER|VIEW ANY DEFINITION|VWAD|No aplicable|No aplicable|  
|SERVER|VIEW SERVER STATE|VWSS|No aplicable|No aplicable|  
|SERVER ROLE|ALTER|AL|SERVER|ALTER ANY SERVER ROLE|  
|SERVER ROLE|CONTROL|CL|SERVER|CONTROL SERVER|  
|SERVER ROLE|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|SERVER ROLE|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|SERVICE|ALTER|AL|DATABASE|ALTER ANY SERVICE|  
|SERVICE|CONTROL|CL|DATABASE|CONTROL|  
|SERVICE|ENVIAR|SN|DATABASE|CONTROL|  
|SERVICE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|SERVICE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|SYMMETRIC KEY|ALTER|AL|DATABASE|ALTER ANY SYMMETRIC KEY|  
|SYMMETRIC KEY|CONTROL|CL|DATABASE|CONTROL|  
|SYMMETRIC KEY|REFERENCES|RF|DATABASE|REFERENCES|  
|SYMMETRIC KEY|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|SYMMETRIC KEY|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|TYPE|CONTROL|CL|SCHEMA|CONTROL|  
|TYPE|Ejecute|EX|SCHEMA|Ejecute|  
|TYPE|REFERENCES|RF|SCHEMA|REFERENCES|  
|TYPE|TAKE OWNERSHIP|TO|SCHEMA|CONTROL|  
|TYPE|VIEW DEFINITION|VW|SCHEMA|VIEW DEFINITION|  
|USER|ALTER|AL|DATABASE|ALTER ANY USER|  
|USER|CONTROL|CL|DATABASE|CONTROL|  
|USER|IMPERSONATE|IM|DATABASE|CONTROL|  
|USER|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|XML SCHEMA COLLECTION|ALTER|AL|SCHEMA|ALTER|  
|XML SCHEMA COLLECTION|CONTROL|CL|SCHEMA|CONTROL|  
|XML SCHEMA COLLECTION|Ejecute|EX|SCHEMA|Ejecute|  
|XML SCHEMA COLLECTION|REFERENCES|RF|SCHEMA|REFERENCES|  
|XML SCHEMA COLLECTION|TAKE OWNERSHIP|TO|SCHEMA|CONTROL|  
|XML SCHEMA COLLECTION|VIEW DEFINITION|VW|SCHEMA|VIEW DEFINITION|  
  
##  <a name="summary-of-the-permission-check-algorithm"></a><a name="_algorithm"></a> Resumen del algoritmo de comprobación de permiso  
 Comprobar los permisos puede ser complejo. El algoritmo de comprobación de permiso incluye la superposición de la pertenencia a grupos y el encadenamiento de propiedad, tanto el permiso explícito como el implícito, y puede ser afectado por los permisos en las clases protegibles y que contienen la entidad protegible. El proceso general del algoritmo es reunir todos los permisos pertinentes. Si no se encuentra ningún bloqueo DENY, el algoritmo busca un permiso GRANT que proporcione el acceso suficiente. El algoritmo contiene tres elementos esenciales, el **contexto de seguridad**, el **espacio del permiso** y el **permiso necesario**.  
  
> [!NOTE]  
>  No puede conceder, denegar ni revocar permisos a sa, dbo, al propietario de la entidad, information_schema, sys ni a usted mismo.  
  
-   **Contexto de seguridad**  
  
     Es el grupo de entidades de seguridad que aportan los permisos para la comprobación de acceso. Son los permisos que están relacionados con el inicio de sesión actual o el usuario, a menos que el contexto de seguridad se cambiara a otro inicio de sesión o usuario utilizando la instrucción EXECUTE AS. El contexto de seguridad incluye las entidades de seguridad siguientes:  
  
    -   El inicio de sesión  
  
    -   Usuario  
  
    -   Pertenecientes al rol  
  
    -   Pertenecientes al grupo Windows  
  
    -   Si se utiliza la firma de módulo, cualquier cuenta de inicio de sesión o de usuario da cuenta del certificado utilizado para firmar el módulo que el usuario está ejecutando actualmente, así como de los pertenecientes al rol asociados de ese entidad de seguridad.  
  
-   **Espacio del permiso**  
  
     Es la entidad protegible y todas las clases protegibles que contiene la entidad protegible. Por ejemplo, una tabla (una entidad protegible) está contenida en la clase de esquema protegible y en la clase de base de datos protegible. El acceso puede verse afectado por permisos de nivel de tabla, esquema, base de datos y servidor. Para obtener más información, vea [Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md).  
  
-   **Permiso necesario**  
  
     El tipo de permiso que se necesita. Por ejemplo, INSERT, UPDATE, DELETE, SELECT, EXECUTE, ALTER, CONTROL, etc.  
  
     El acceso puede requerir varios permisos, como en los ejemplos siguientes:  
  
    -   Un procedimiento almacenado puede requerir el permiso EXECUTE sobre el procedimiento almacenado y el permiso INSERT sobre varias tablas a las que hace referencia el procedimiento almacenado.  
  
    -   Una vista de administración dinámica puede requerir los permisos VIEW SERVER STATE y SELECT sobre la vista.  
  
### <a name="general-steps-of-the-algorithm"></a>Pasos generales del algoritmo  
 Cuando el algoritmo está determinando si permite el acceso a un elemento protegible, los pasos precisos que utiliza pueden variar, dependiendo de las entidades de seguridad y de los elementos protegibles implicados. Sin embargo, el algoritmo da los siguientes pasos generales:  
  
1.  Omite la comprobación del permiso si el inicio de sesión es un miembro del rol fijo de servidor sysadmin o si el usuario es el usuario de dbo en la base de datos actual.  
  
2.  Permite el acceso si es aplicable el encadenamiento de propiedad y la comprobación de acceso en el objeto anterior de la cadena pasó la comprobación de seguridad.  
  
3.  Agrega las identidades de nivel del servidor, de base de datos y de módulo firmado que se asocian al autor de las llamadas para crear el **contexto de seguridad**.  
  
4.  Para ese **contexto de seguridad**, reúne todos los permisos que se conceden o deniegan para el **espacio del permiso**. El permiso se puede nombrar explícitamente como GRANT, GRANT WITH GRANT o DENY; o los permisos pueden ser un permiso GRANT o DENY implícito o inclusivo. Por ejemplo, el permiso CONTROL sobre un esquema implica CONTROL sobre una tabla. Asimismo, CONTROL sobre una tabla implica SELECT. Por consiguiente, si se otorgó CONTROL sobre el esquema, se otorgó SELECT sobre la tabla. Si se denegó CONTROL sobre la tabla, también se denegó SELECT sobre ella.  
  
    > [!NOTE]  
    >  Un permiso GRANT de nivel de columna invalida un permiso DENY en el nivel de objeto.  
  
5.  Identifique el **permiso requerido**.  
  
6.  La comprobación del permiso no se realiza correctamente si el **permiso requerido** es denegado directa o implícitamente a cualquiera de las identidades del **contexto de seguridad** para los objetos del **espacio del permiso**.  
  
7.  La comprobación del permiso es correcta si no se denegó el **permiso requerido** y el **permiso requerido** contiene un permiso GRANT o un permiso GRANT WITH GRANT directo o implícito para cualquiera de las identidades del **contexto de seguridad** de cualquier objeto del **espacio del permiso**.  

## <a name="special-considerations-for-column-level-permissions"></a>Consideraciones especiales sobre los permisos de nivel de columna

Los permisos de nivel de columna se conceden con la sintaxis *<nombre_tabla>(\<column _name>)* . Por ejemplo:
```sql
GRANT SELECT ON OBJECT::Customer(CustomerName) TO UserJoe;
```
Un permiso DENY en la tabla se reemplaza por un permiso GRANT en una columna. Pero un permiso DENY subsiguiente en la tabla quitará la columna GRANT. 
  
##  <a name="examples"></a><a name="_examples"></a> Ejemplos  
 En los ejemplos de esta sección se muestra cómo se recupera la información sobre permisos.  
  
### <a name="a-returning-the-complete-list-of-grantable-permissions"></a>A. Devolver la lista completa de los permisos que pueden concederse.  
 La siguiente instrucción devuelve todos los permisos de [!INCLUDE[ssDE](../../includes/ssde-md.md)] mediante la función `fn_builtin_permissions` . Para obtener más información, vea [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md).  
  
```sql  
SELECT * FROM fn_builtin_permissions(default);  
GO  
```  
  
### <a name="b-returning-the-permissions-on-a-particular-class-of-objects"></a>B. Devolver los permisos de una clase de objetos concreta  
 En el ejemplo siguiente se usa `fn_builtin_permissions` para ver todos los permisos que están disponibles para una categoría de elemento protegible. El ejemplo devuelve permisos de ensamblados.  
  
```sql  
SELECT * FROM fn_builtin_permissions('assembly');  
GO    
```  
  
### <a name="c-returning-the-permissions-granted-to-the-executing-principal-on-an-object"></a>C. Devolver los permisos de un objeto concedidos a la entidad de seguridad que se ejecuta  
 En el ejemplo siguiente se usa `fn_my_permissions` para devolver una lista de los permisos efectivos que son retenidos por la entidad de seguridad de la llamada sobre un elemento protegible específico. El ejemplo devuelve los permisos de un objeto denominado `Orders55`. Para obtener más información, vea [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md).  
  
```sql  
SELECT * FROM fn_my_permissions('Orders55', 'object');  
GO  
```  
  
### <a name="d-returning-the-permissions-applicable-to-a-specified-object"></a>D. Devolver los permisos aplicables a un objeto especificado  
 El ejemplo siguiente devuelve los permisos aplicables a un objeto denominado `Yttrium`. Observe que la función integrada `OBJECT_ID` se utiliza para recuperar el identificador del objeto `Yttrium`.  
  
```sql  
SELECT * FROM sys.database_permissions   
    WHERE major_id = OBJECT_ID('Yttrium');  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
