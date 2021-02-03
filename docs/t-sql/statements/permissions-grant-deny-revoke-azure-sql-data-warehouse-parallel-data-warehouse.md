---
description: 'Permisos: GRANT, DENY, REVOKE (Azure Synapse Analytics, Almacenamiento de datos en paralelo)'
title: Permisos GRANT-DENY-REVOKE
titleSuffix: Azure Synapse Analytics
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5a3b7424-408e-4cb0-8957-667ebf4596fc
author: VanMSFT
ms.author: vanto
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 4756d84874e207b0047127061faff55de296d01e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204632"
---
# <a name="permissions-grant-deny-revoke-azure-synapse-analytics-parallel-data-warehouse"></a>Permisos: GRANT, DENY, REVOKE (Azure Synapse Analytics, Almacenamiento de datos en paralelo)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Use las instrucciones **GRANT** y **DENY** de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] para conceder o denegar un permiso (como **UPDATE**) en un elemento protegible (por ejemplo, una base de datos, una tabla, una vista, etc.) a una entidad de seguridad (un inicio de sesión, un usuario de base de datos o un rol de base de datos). Use **REVOKE** para quitar la concesión o la denegación de un permiso.  
  
 Los permisos de nivel de servidor se aplican a los inicios de sesión. Los permisos de nivel de base de datos se aplican a los usuarios de base de datos y roles de base de datos.  
  
 Para ver qué permisos se han concedido y denegado, consulte las vistas sys.server_permissions y sys.database_permissions. Los permisos que no se conceden o deniegan explícitamente a una entidad de seguridad se pueden heredar si se dispone de una pertenencia en un rol que tiene permisos. Los permisos de los roles fijos de base de datos no se pueden cambiar y no aparecen en las vistas sys.server_permissions y sys.database_permissions.  
  
-   **GRANT** concede explícitamente uno o varios permisos.  
  
-   **DENY** deniega explícitamente uno o varios permisos a la entidad de seguridad.  
  
-   **REVOCAR** quita los permisos **GRANT** o **DENY** existentes.  
  
 ![Icono de vínculo a temas](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Azure Synapse Analytics and Parallel Data Warehouse  
GRANT   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    TO principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
[;]  
  
DENY   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    TO principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
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
  
## <a name="arguments"></a>Argumentos  
 \<permission>[ **,** ...*n* ]  
 Uno o varios permisos que se van a conceder, denegar o revocar.  
  
 ON [ \<class_type> :: ] *protegible* La cláusula **ON** describe el parámetro protegible en el que se van a conceder, denegar o revocar permisos.  
  
 \<class_type> Tipo de clase del elemento protegible. Puede ser **LOGIN**, **DATABASE**, **OBJECT**, **SCHEMA**, **ROLE** o **USER**. También se pueden conceder permisos a **SERVER**_class\_type_, pero **SERVER** no se especifica para esos permisos. **DATABASE** no se especifica si el permiso incluye la palabra **DATABASE** (por ejemplo, **ALTER ANY DATABASE**). Si no se especifica *class_type* y el tipo de permiso no está restringido a la clase de base de datos o servidor, se supone que la clase es **OBJECT**.  
  
 *securable*  
 Nombre del inicio de sesión, base de datos, tabla, vista, esquema, procedimiento, rol o usuario en el que se van a conceder, denegar o revocar permisos. El nombre del objeto se puede especificar con las reglas de nomenclatura de tres partes que se describen en [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
 TO *principal* [ **,** ...*n* ]  
 Una o varias entidades de seguridad a las que se van a conceder, denegar o revocar permisos. "Principal" es el nombre de un inicio de sesión, un usuario de base de datos o un rol de base de datos.  
  
 FROM *principal* [ **,** ...*n* ]  
 Una o varias entidades de seguridad cuyos permisos se van a revocar.  "Principal" es el nombre de un inicio de sesión, un usuario de base de datos o un rol de base de datos. **FROM** solo se puede usar con una instrucción **REVOKE**. **TO** se puede usar con **GRANT**, **DENY** o **REVOKE**.  
  
 WITH GRANT OPTION  
 Indica que el receptor también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
 CASCADE  
 Indica que el permiso se deniega o revoca para la entidad de seguridad especificada y para el resto de entidades de seguridad a las que esta ha concedido el permiso. Es obligatorio cuando la entidad de seguridad tiene el permiso con **GRANT OPTION**.  
  
 GRANT OPTION FOR  
 Indica que se revocará la capacidad de conceder el permiso especificado. Se requiere cuando se usa el argumento **CASCADE**.  
  
> [!IMPORTANT]  
>  Si la entidad de seguridad dispone del permiso especificado sin la opción **GRANT**, se revocará el permiso.  
  
## <a name="permissions"></a>Permisos  
 Para conceder un permiso, el otorgante debe tener el permiso en cuestión con **WITH GRANT OPTION**, o un permiso superior que implique el permiso que se va a conceder.  Los propietarios de objetos pueden conceder permisos para los objetos que poseen. Las entidades de seguridad con permiso **CONTROL** en un elemento protegible pueden conceder permisos para ese elemento.  Los miembros de los roles fijos de base de datos **db_owner** y **db_securityadmin** pueden conceder cualquier permiso en la base de datos.  
  
## <a name="general-remarks"></a>Notas generales  
 El hecho de denegar o revocar permisos para una entidad de seguridad no afectará a las solicitudes que hayan superado la autorización y se estén ejecutando. Para restringir el acceso de inmediato, debe cancelar las solicitudes activas o terminar las sesiones actuales.  
  
> [!NOTE]  
>  La mayoría de los roles fijos de servidor no están disponibles en esta versión. Use en su lugar roles de base de datos definidos por el usuario. Los inicios de sesión no se pueden agregar al rol fijo de servidor **sysadmin**. La concesión del permiso **CONTROL SERVER** es similar a la pertenencia al rol fijo de servidor **sysadmin**.  
  
 Algunas instrucciones requieren varios permisos. Por ejemplo, la creación de una tabla requiere permisos **CREATE TABLE** en la base de datos y el permiso **ALTER SCHEMA** para la tabla que contendrá la tabla.  
  
 En ocasiones, el Almacenamiento de datos paralelos ejecuta procedimientos almacenados para distribuir las acciones del usuario a los nodos de ejecución. Por lo tanto, no se puede denegar el permiso EXECUTE para toda una base de datos. (Por ejemplo, se producirá un error en `DENY EXECUTE ON DATABASE::<name> TO <user>;`). Como solución alternativa, deniegue el permiso EXECUTE para esquemas de usuario u objetos específicos (procedimientos).  
  
### <a name="implicit-and-explicit-permissions"></a>Permisos implícitos y explícitos  
 Un *permiso explícito* es un permiso **GRANT** o **DENY** concedido a una entidad de seguridad mediante una instrucción **GRANT** o **DENY**.  
  
 Un *permiso implícito* es un permiso **GRANT** o **DENY** que una entidad de seguridad (inicio de sesión, usuario o rol de base de datos) ha heredado de otro rol de base de datos.  
  
 También se puede heredar un permiso implícito de un permiso principal o inclusivo. Por ejemplo, el permiso **UPDATE** en una tabla puede heredarse si se tiene el permiso **UPDATE** en el esquema que contiene la tabla o si se tiene el permiso **CONTROL** en la tabla.  
  
### <a name="ownership-chaining"></a>Encadenamiento de propiedad  
 Cuando varios objetos de base de datos obtienen acceso unos a otros de forma secuencial, la secuencia se denomina *cadena*. Aunque estas cadenas no existen de manera independiente, cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recorre los eslabones de una cadena, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] evalúa los permisos de los objetos que la componen de manera distinta a si se estuviese obteniendo acceso a los objetos por separado. El encadenamiento de propiedad tiene implicaciones importantes en lo que respecta a la administración de la seguridad. Para más información sobre las cadenas de propiedad, vea [Cadenas de propiedad](https://msdn.microsoft.com/library/ms188676\(v=sql11\).aspx) y [Tutorial: Cadenas de propiedad y cambio de contexto](../../relational-databases/tutorial-ownership-chains-and-context-switching.md).  
  
## <a name="permission-list"></a>Lista de permisos  
  
### <a name="server-level-permissions"></a>Permisos de nivel de servidor  
 Se pueden conceder, denegar o revocar permisos de nivel de servidor de inicios de sesión.  
  
 **Permisos que se aplican a los servidores**  
  
-   CONTROL SERVER  
  
-   ADMINISTER BULK OPERATIONS  
  
-   ALTER ANY CONNECTION  
  
-   ALTER ANY DATABASE  
  
-   CREATE ANY DATABASE  
  
-   ALTER ANY EXTERNAL DATA SOURCE  
  
-   ALTER ANY EXTERNAL FILE FORMAT  
  
-   ALTER ANY LOGIN  
  
-   ALTER SERVER STATE  
  
-   CONNECT SQL  
  
-   VIEW ANY DEFINITION  
  
-   VIEW ANY DATABASE  
  
-   VIEW SERVER STATE  
  
 **Permisos que se aplican a los inicios de sesión**  
  
-   CONTROL ON LOGIN  
  
-   ALTER ON LOGIN  
  
-   IMPERSONATE ON LOGIN  
  
-   VIEW DEFINITION  
  
### <a name="database-level-permissions"></a>Permisos de nivel de base de datos  
 Los permisos de nivel de base de datos se pueden conceder, denegar y revocar de usuarios de base de datos y roles de base de datos definidos por el usuario.  
  
 **Permisos que se aplican a todas las clases de base de datos**  
  
-   CONTROL  
  
-   ALTER  
  
-   VIEW DEFINITION  
  
 **Permisos que se aplican a todas las clases de base de datos, excepto usuarios**  
  
-   TAKE OWNERSHIP  
  
 **Permisos que se aplican únicamente a las bases de datos**  
  
-   ALTER ANY DATABASE  
  
-   ALTER ON DATABASE  
  
-   ALTER ANY DATASPACE  
  
-   ALTER ANY ROLE  
  
-   ALTER ANY SCHEMA  
  
-   ALTER ANY USER  
  
-   BACKUP DATABASE  
  
-   CONNECT ON DATABASE  
  
-   CREATE PROCEDURE  
  
-   CREATE ROLE  
  
-   CREATE SCHEMA  
  
-   CREATE TABLE  
  
-   CREATE VIEW  
  
-   SHOWPLAN  
  
 **Permisos que se aplican únicamente a los usuarios**  
  
-   IMPERSONATE  
  
 **Permisos que se aplican a las bases de datos, esquemas y objetos**  
  
-   ALTER  
  
-   Delete  
  
-   Ejecute  
  
-   INSERT  
  
-   SELECT  
  
-   UPDATE  
  
-   REFERENCES  
  
 Para una definición de cada tipo de permiso, vea [Permisos (motor de base de datos)](../../relational-databases/security/permissions-database-engine.md).  
  
### <a name="chart-of-permissions"></a>Gráfico de permisos  
 En este póster se representan gráficamente todos los permisos. Es la manera más fácil de ver la jerarquía anidada de permisos. Por ejemplo, el permiso **ALTER LOGIN ON** se puede conceder por sí mismo, pero también se incluye si se concede a un inicio de sesión el permiso **CONTROL** en ese inicio de sesión, o si se concede el permiso **ALTER ANY LOGIN** a un inicio de sesión.  
  
 ![Póster de permisos de seguridad de APS](../../t-sql/statements/media/aps-security-perms-poster.png "Póster de permisos de seguridad de APS")  
  
 Para descargar una versión a tamaño completo de este póster, vea [Permisos de PDW de SQL Server](https://go.microsoft.com/fwlink/?LinkId=244249) en la sección de archivos del sitio de Yammer de APS (o solicítelo por correo electrónico a **apsdoc\@microsoft.com**).  
  
## <a name="default-permissions"></a>Permisos predeterminados  
 En la lista siguiente se describen los permisos predeterminados:  
  
-   Cuando se crea un inicio de sesión mediante la instrucción **CREATE LOGIN**, el nuevo inicio de sesión recibe el permiso **CONNECT SQL**.  
  
-   Todos los inicios de sesión son miembros del rol de servidor **público** y no se pueden quitar de **público**.  
  
-   Cuando se crea un usuario de base de datos mediante el permiso **CREATE USER**, el usuario de base de datos recibe el permiso **CONNECT** en la base de datos.  
  
-   Ninguna entidad de seguridad, incluido el rol **público**, tiene permisos explícitos o implícitos de forma predeterminada.  
  
-   Cuando un inicio de sesión o usuario se convierte en el propietario de una base de datos o un objeto, el inicio de sesión o el usuario siempre tiene todos los permisos en la base de datos o el objeto. Los permisos de propiedad no se pueden cambiar y no están visibles como permisos explícitos. Las instrucciones **GRANT**, **DENY** y **REVOKE** no tienen ningún efecto en los propietarios.  
  
-   El inicio de sesión **sa** tiene todos los permisos en el dispositivo. Al igual que sucede con los permisos de propiedad, los permisos **sa** no se pueden cambiar y no están visibles como permisos explícitos. Las instrucciones **GRANT**, **DENY** y **REVOKE** no tienen ningún efecto en el inicio de sesión **sa**. No se puede cambiar el nombre del inicio de sesión **sa**.  
  
-   La instrucción **USE** no requiere permisos. Todas las entidades de seguridad pueden ejecutar la instrucción **USE** en cualquier base de datos.  
  
##  <a name="examples-sssdw-and-sspdw"></a><a name="Examples"></a> Ejemplos: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-granting-a-server-level-permission-to-a-login"></a>A. Conceder un permiso de nivel de servidor a un inicio de sesión  
 Las dos instrucciones siguientes conceden un permiso de nivel de servidor a un inicio de sesión.  
  
```sql  
GRANT CONTROL SERVER TO [Ted];  
```  
  
```sql  
GRANT ALTER ANY DATABASE TO Mary;  
```  
  
### <a name="b-granting-a-server-level-permission-to-a-login"></a>B. Conceder un permiso de nivel de servidor a un inicio de sesión  
 En el ejemplo siguiente se concede un permiso de nivel de servidor en un inicio de sesión a una entidad de seguridad de servidor (otro inicio de sesión).  
  
```sql  
GRANT  VIEW DEFINITION ON LOGIN::Ted TO Mary;  
```  
  
### <a name="c-granting-a-database-level-permission-to-a-user"></a>C. Conceder un permiso de nivel de base de datos a un usuario  
 En el ejemplo siguiente se concede un permiso de nivel de base de datos en un usuario a una entidad de seguridad de base de datos (otro usuario).  
  
```sql  
GRANT VIEW DEFINITION ON USER::[Ted] TO Mary;  
```  
  
### <a name="d-granting-denying-and-revoking-a-schema-permission"></a>D. Conceder, denegar y revocar un permiso de esquema  
 La siguiente instrucción **GRANT** concede a Yuen la capacidad de seleccionar datos de cualquier tabla o vista del esquema dbo.  
  
```sql  
GRANT SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
 La siguiente instrucción **DENY** impide que Yuen pueda seleccionar datos de cualquier tabla o vista del esquema dbo. Yuen no puede leer los datos, ni siquiera aunque tenga el permiso de otra manera, por ejemplo, mediante una pertenencia a un rol.  
  
```sql  
DENY SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
 La siguiente instrucción **REVOKE** quita el permiso **DENY**. Ahora, los permisos explícitos de Yuen son neutros. Yuen puede seleccionar los datos de cualquier tabla mediante otro permiso implícito, como una pertenencia a un rol.  
  
```sql  
REVOKE SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
### <a name="e-demonstrating-the-optional-object-clause"></a>E. Demostración de la cláusula opcional OBJECT::  
 Dado que OBJECT es la clase predeterminada para una instrucción de permiso, las dos instrucciones siguientes son iguales. La cláusula **OBJECT::** es opcional.  
  
```sql  
GRANT UPDATE ON OBJECT::dbo.StatusTable TO [Ted];  
```  
  
```sql  
GRANT UPDATE ON dbo.StatusTable TO [Ted];  
```  
  
