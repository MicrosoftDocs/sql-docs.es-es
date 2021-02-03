---
title: Introducción a los permisos de los motores de bases de datos | Microsoft Docs
description: Revise algunos conceptos básicos de seguridad en SQL Server y obtenga información sobre una implementación típica de los permisos del motor de base de datos.
ms.custom: ''
ms.date: 01/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: quickstart
helpviewer_keywords:
- permissions [SQL Server], getting started
ms.assetid: 051af34e-bb5b-403e-bd33-007dc02eef7b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a65e01d5d4e43e25d122c614508f8f9c1c05bf39
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236193"
---
# <a name="getting-started-with-database-engine-permissions"></a>Introducción a los permisos de los motores de bases de datos
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Los permisos de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] se administran en el nivel de servidor mediante inicios de sesión y roles de servidor, y en el nivel de base de datos mediante usuarios de base de datos y roles base de datos. El modelo para [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] expone el mismo sistema en cada base de datos, pero los permisos de nivel de servidor no están disponibles. En este tema se tratan algunos conceptos básicos de seguridad y, a continuación, se describe una implementación típica de los permisos.  
  
## <a name="security-principals"></a>Entidades de seguridad  
 "Entidad de seguridad" es el nombre oficial de las identidades que utilizan [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y a las que se pueden conceder permisos para realizar acciones. Suelen ser personas o grupos de personas, pero pueden ser otras entidades que finjan ser personas. Las entidades de seguridad se pueden crear y administrar mediante el [!INCLUDE[tsql](../../../includes/tsql-md.md)] indicado o mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
##### <a name="logins"></a>Inicios de sesión  
 Los inicios de sesión son cuentas de usuario individuales para iniciar sesión en [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] admiten inicios de sesión basados en la autenticación de Windows e inicios de sesión basados en la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para obtener más información acerca de los dos tipos de inicio de sesión, consulte [Choose an Authentication Mode](../../../relational-databases/security/choose-an-authentication-mode.md).  
  
##### <a name="fixed-server-roles"></a>Roles fijos de servidor  
 En [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], los roles fijos de servidor son un conjunto de roles preconfigurados que proporcionan un grupo práctico de permisos de nivel de servidor. Se pueden agregar inicios de sesión a los roles mediante la instrucción `ALTER SERVER ROLE ... ADD MEMBER` . Para obtener más información, consulte [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-role-transact-sql.md). [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] no admite los roles fijos de servidor, pero tiene dos roles en la base de datos maestra (`dbmanager` y `loginmanager`) que actúan como roles de servidor.  
  
##### <a name="user-defined-server-roles"></a>Roles de servidor definidos por el usuario  
 En [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], puede crear sus propios roles de servidor y asignarles permisos de nivel de servidor. Se pueden agregar inicios de sesión al servidor mediante la instrucción `ALTER SERVER ROLE ... ADD MEMBER` . Para obtener más información, consulte [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-role-transact-sql.md). [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] no admite los roles de servidor definidos por el usuario.  
  
##### <a name="database-users"></a>Usuarios de bases de datos  
 Para conceder acceso a una base de datos a los inicios de sesión, se crea un usuario de base de datos en una base de datos y se asigna ese usuario de base de datos al inicio de sesión. Normalmente, el nombre de usuario de base de datos es el mismo que el nombre de inicio de sesión, aunque no tiene por qué ser así en todos los casos. Cada usuario de base de datos se asigna a un inicio de sesión único. Un inicio de sesión se puede asignar a un único usuario en una base de datos, pero se puede asignar como usuario de base de datos en distintas bases de datos.  
  
 También se pueden crear usuarios de base de datos que no tengan el correspondiente inicio de sesión. Estos usuarios se denominan *usuarios de base de datos independiente*. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] recomienda el uso de los usuarios de base de datos independiente porque facilita el traslado de la base de datos a otro servidor. Igual que un inicio de sesión, un usuario de base de datos independiente puede utilizar tanto la autenticación de Windows como la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para obtener más información, vea [Usuarios de base de datos independiente: hacer que la base de datos sea portátil](../../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
 Hay 12 tipos de usuarios con ligeras diferencias en cuanto a cómo se autentican y a quién representan. Para ver una lista de usuarios, consulte [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md).  
  
##### <a name="fixed-database-roles"></a>Roles fijos de base de datos  
 Los roles fijos de base de datos son un conjunto de roles preconfigurados que proporcionan un práctico grupo de permisos de nivel de base de datos. Se pueden agregar usuarios de base de datos y roles de base de datos definidos por el usuario a los roles fijos de base de datos mediante la instrucción `ALTER ROLE ... ADD MEMBER`. Para obtener más información, vea [ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md).  
  
##### <a name="user-defined-database-roles"></a>Roles de base de datos definidos por el usuario  
 Los usuarios con el permiso `CREATE ROLE` pueden crear nuevos roles de base de datos definidos por el usuario para representar grupos de usuarios con permisos comunes. Normalmente, los permisos se conceden o deniegan a todo el rol, lo que simplifica la administración y supervisión de permisos. Se pueden agregar usuarios de base de datos a los roles de base de datos mediante la instrucción `ALTER ROLE ... ADD MEMBER` . Para obtener más información, vea [ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md).  
  
##### <a name="other-principals"></a>Otras entidades de seguridad  
 Otras entidades de seguridad que no se tratan aquí son los roles de aplicación y los inicios de sesión y usuarios basados en certificados o claves asimétricas.  
  
 Para ver un gráfico que muestra las relaciones entre usuarios de Windows, grupos de Windows, inicios de sesión y usuarios de base de datos, consulte [Create a Database User](../../../relational-databases/security/authentication-access/create-a-database-user.md).  
  
## <a name="typical-scenario"></a>Escenario típico  
 En el ejemplo siguiente se muestra un método común y recomendado para la configuración de permisos.  
  
#### <a name="in-active-directory-or-azure-active-directory"></a>En Active Directory o Azure Active Directory:  
  
1.  Cree un usuario de Windows para cada persona.  
  
2.  Cree grupos de Windows que representen las unidades y las funciones de trabajo.  
  
3.  Agregue los usuarios de Windows a los grupos de Windows.  

#### <a name="if-the-person-connecting-will-be-connecting-to-many-databases"></a>Si la persona que se conecta lo va a hacer a diversas bases de datos  
  
1.  Cree un inicio de sesión para los grupos de Windows. (Si usa la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , omita los pasos de Active Directory y cree los inicios de sesión de la autenticación [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aquí).  
  
2.  En la base de datos de usuario, cree un usuario de base de datos para el inicio de sesión que representa los grupos de Windows.  
  
3.  En la base de datos de usuario, cree uno o varios roles de base de datos definidos por el usuario, cada uno para representar una función similar. Por ejemplo, analista financiero y analista de ventas.  
  
4.  Agregue los usuarios de base de datos a uno o varios roles de base de datos definidos por el usuario.  
  
5.  Conceda permisos a los roles de base de datos definidos por el usuario.  
  
#### <a name="if-the-person-connecting-will-be-connecting-to-only-one-database"></a>Si la persona que se conecta lo va a hacer a una única base de datos  
  
1.  En la base de datos de usuario, cree un usuario de base de datos independiente para el grupo de Windows. (Si usa la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , omita los pasos de Active Directory y cree la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] del usuario de base de datos independiente aquí).  
  
1.  En la base de datos de usuario, cree uno o varios roles de base de datos definidos por el usuario, cada uno para representar una función similar. Por ejemplo, analista financiero y analista de ventas.  
  
1.  Agregue los usuarios de base de datos a uno o varios roles de base de datos definidos por el usuario.  
  
1.  Conceda permisos a los roles de base de datos definidos por el usuario.  
  
 Normalmente, el resultado en este punto es que un usuario de Windows es miembro de un grupo de Windows. El grupo de Windows tiene un inicio de sesión en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. El inicio de sesión se asigna a una identidad de usuario en la base de datos de usuario. El usuario es miembro de un rol de base de datos. Ahora debe agregar permisos al rol.  
  
## <a name="assigning-permissions"></a>Asignación de permisos  
 El formato de la mayoría de instrucciones de permiso es:  
  
```  
AUTHORIZATION  PERMISSION  ON  SECURABLE::NAME  TO  PRINCIPAL;  
```  
  
-   `AUTHORIZATION` debe ser `GRANT`, `REVOKE` o `DENY`.  
  
-   `PERMISSION` establece las acciones permitidas o prohibidas. [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] puede especificar 230 permisos. [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] tiene menos permisos porque algunas acciones no son pertinentes en Azure. Los permisos se muestran en el tema [Permissions &#40;Database Engine&#41;](../../../relational-databases/security/permissions-database-engine.md) (Permisos [motor de base de datos]) y en el gráfico al que se hace referencia más abajo.  
  
-   `ON SECURABLE::NAME` es el tipo de elemento protegible (servidor, objeto de servidor, base de datos u objeto de base de datos) y su nombre. Algunos permisos no requieren `ON SECURABLE::NAME` porque este es inequívoco o bien no procede en el contexto. Por ejemplo, el permiso `CREATE TABLE` no requiere la cláusula `ON SECURABLE::NAME`. (Por ejemplo, `GRANT CREATE TABLE TO Mary;` permite a Mary crear tablas).  
  
-   `PRINCIPAL` es la entidad de seguridad (inicio de sesión, usuario o rol) que recibe o pierde el permiso. Conceda permisos a los roles siempre que sea posible.  
  
 En la siguiente instrucción GRANT de ejemplo, se concede el permiso `UPDATE` de la tabla o vista `Parts` incluida en el esquema `Production` al rol denominado `PartsTeam`:  
  
```  
GRANT UPDATE ON OBJECT::Production.Parts TO PartsTeam;  
```  
  
 Los permisos se conceden a las entidades de seguridad (inicios de sesión, usuarios y roles) mediante la instrucción `GRANT` . Los permisos se deniegan explícitamente mediante el comando  `DENY` . Para eliminar un permiso concedido o denegado anteriormente, se usa la instrucción `REVOKE` . Los permisos son acumulativos, de modo que el usuario recibe todos los permisos concedidos al usuario, al inicio de sesión y a cualquier pertenencia a algún grupo. Sin embargo, las denegaciones de permisos invalidan todas las concesiones.  
  
> [!TIP]  
>  Un error común es intentar eliminar un elemento `GRANT` mediante `DENY` en lugar de `REVOKE`. Esto puede causar problemas cuando un usuario recibe permisos de varios orígenes, lo que es bastante común. En el ejemplo siguiente se muestra la entidad de seguridad.  
  
 El grupo Ventas recibe permisos `SELECT` en la tabla OrderStatus mediante la instrucción `GRANT SELECT ON OBJECT::OrderStatus TO Sales;`. El usuario Ted es miembro del rol Ventas. También se ha concedido a Ted el permiso `SELECT` para la tabla OrderStatus bajo su propio nombre de usuario mediante la instrucción  `GRANT SELECT ON OBJECT::OrderStatus TO Ted;`. Supongamos que el administrador desea quitar el elemento `GRANT` al rol Ventas.  
  
-   Si el administrador ejecuta correctamente `REVOKE SELECT ON OBJECT::OrderStatus TO Sales;`, Ted conservará el acceso `SELECT` a la tabla OrderStatus mediante su instrucción `GRANT` individual.  
  
-   Si el administrador no ejecuta correctamente `DENY SELECT ON OBJECT::OrderStatus TO Sales;` , a Ted, como miembro del rol de ventas, le será denegado el permiso `SELECT` porque `DENY` a Ventas invalida su  `GRANT`individual.  
  
> [!NOTE]  
>  Los permisos se pueden configurar mediante [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]. Busque el elemento protegible en el Explorador de objetos, haga clic con el botón derecho en el elemento protegible y, luego, haga clic en **Propiedades**. Seleccione la página **Permisos** . Para obtener ayuda sobre el uso de la página de permisos, consulte [Permissions or Securables Page](../../../relational-databases/security/permissions-or-securables-page.md).  
  
## <a name="permission-hierarchy"></a>Jerarquía de permisos  
 Los permisos tienen una jerarquía de elementos principales y secundarios. Es decir, si se concede el permiso `SELECT` en una base de datos, dicho permiso incluirá el permiso `SELECT` en todos los esquemas (secundarios) de la base de datos. Si se concede el permiso `SELECT` en un esquema, incluirá el permiso `SELECT` en todas las tablas y vistas (secundarias) del esquema. Los permisos son transitivos, es decir, si se concede el permiso `SELECT` en una base de datos, incluirá el permiso `SELECT` en todos los esquemas (secundarios) y todas las tablas y vistas (descendientes de elementos secundarios).  
  
 Los permisos también tienen permisos de cobertura. El permiso `CONTROL` en un objeto normalmente concede todos los otros permisos del objeto.  
  
 Puesto que tanto la jerarquía de elementos principales y secundarios como la jerarquía de cobertura pueden actuar en el mismo permiso, el sistema de permisos puede resultar complicado. Por ejemplo, consideremos una tabla (Región) de en un esquema (Clientes) en una base de datos (SalesDB).  
  
-   `CONTROL` en la tabla Región incluye todos los otros permisos de la tabla Región, incluidos los permisos `ALTER`, `SELECT`, `INSERT`,  `UPDATE`, `DELETE`y algunos otros permisos.  
  
-   `SELECT` en el esquema Clientes que posee la tabla Región incluye el permiso `SELECT` en la tabla Región.  
  
 Así que el permiso `SELECT` en la tabla Región se puede obtener mediante cualquiera de estas seis instrucciones:  
  
```  
GRANT SELECT ON OBJECT::Region TO Ted;   
  
GRANT CONTROL ON OBJECT::Region TO Ted;   
  
GRANT SELECT ON SCHEMA::Customers TO Ted;   
  
GRANT CONTROL ON SCHEMA::Customers TO Ted;   
  
GRANT SELECT ON DATABASE::SalesDB TO Ted;   
  
GRANT CONTROL ON DATABASE::SalesDB TO Ted;  
```  
  
## <a name="grant-the-least-permission"></a>Conceder el permiso mínimo  
 El primer permiso mencionado antes (`GRANT SELECT ON OBJECT::Region TO Ted;`) es el más pormenorizado, es decir, esa instrucción es el permiso mínimo posible que concede `SELECT`. No incluye permisos para los objetos subordinados. Se recomienda conceder siempre el permiso mínimo posible, pero, por el contrario, concederlo en los niveles superiores para simplificar el sistema de concesiones. Por tanto, si Ted necesita permisos para todo el esquema, conceda `SELECT` una vez en el nivel de esquema, en lugar de conceder `SELECT` en el nivel de tabla o vista varias veces. El diseño de la base de datos ejerce un impacto importante sobre el éxito que puede tener esta estrategia. Esta estrategia funcionará mejor si la base de datos está diseñada de forma que los objetos que necesiten permisos idénticos se incluyan en un único esquema.  
  
## <a name="list-of-permissions"></a>Lista de permisos  
 [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] tiene 230 permisos. [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] tiene 219 permisos. [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] tiene 214 permisos. [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] tiene 195 permisos. [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], [!INCLUDE[ssDW](../../../includes/ssdw-md.md)]y [!INCLUDE[ssAPS](../../../includes/ssaps-md.md)] tienen menos permisos porque solo exponen una parte del motor de base de datos, aunque cada uno tiene permisos que no son aplicables a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 
 
 [!INCLUDE[database-engine-permissions](../../../includes/paragraph-content/database-engine-permissions.md)]
 
 Para ver un gráfico que muestra las relaciones entre las entidades de seguridad de [!INCLUDE[ssDE](../../../includes/ssde-md.md)] y los objetos de servidor y base de datos, consulte [Permissions Hierarchy &#40;Database Engine&#41;](../../../relational-databases/security/permissions-hierarchy-database-engine.md) (Jerarquía de permisos [motor de base de datos]).  
  
## <a name="permissions-vs-fixed-server-and-fixed-database-roles"></a>Permisos y roles fijos de servidor y de base de datos  
 Los permisos de los roles fijos de servidor y roles fijos de base de datos son similares a los permisos granulares, pero no son exactamente iguales. Por ejemplo, los miembros del rol fijo de servidor `sysadmin` tienen todos los permisos en la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], igual que los inicios de sesión con el permiso `CONTROL SERVER` . Pero la concesión del permiso `CONTROL SERVER` no convierte un inicio de sesión en miembro del rol fijo de servidor sysadmin, y agregar un inicio de sesión al rol fijo de servidor `sysadmin` no concede explícitamente el permiso `CONTROL SERVER` al inicio de sesión. A veces, un procedimiento almacenado comprobará los permisos comprobando el rol fijo sin comprobar el permiso granular. Por ejemplo, para desasociar una base de datos es necesaria la pertenencia al rol fijo de base de datos `db_owner` . El permiso equivalente `CONTROL DATABASE` no es suficiente. Estos dos sistemas operan en paralelo, pero rara vez interactúan entre sí. Microsoft recomienda utilizar el sistema de permisos granulares más reciente en lugar de los roles fijos siempre que sea posible.
  
## <a name="monitoring-permissions"></a>Supervisión de permisos  
 Las vistas siguientes devuelven información de seguridad.  
  
-   Los inicios de sesión y los roles de servidor definidos por el usuario en un servidor se pueden examinar con la vista `sys.server_principals` . Esta vista no está disponible en [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Los usuarios y los roles definidos por el usuario en una base de datos se pueden examinar con la vista `sys.database_principals` .  
  
-   Los permisos concedidos a los inicios de sesión y los roles fijos de servidor definidos por el usuario se pueden examinar con la vista `sys.server_permissions` . Esta vista no está disponible en [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Los permisos concedidos a los usuarios y los roles fijos de base de datos definidos por el usuario se pueden examinar con la vista `sys.database_permissions` .  
  
-   La pertenencia al rol de base de datos se puede examinar con la vista `sys. sys.database_role_members` .  
  
-   La pertenencia al rol de servidor se puede examinar con la vista `sys.server_role_members` . Esta vista no está disponible en [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Para obtener vistas adicionales relacionadas con la seguridad, vea [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md) .  
  
### <a name="useful-transact-sql-statements"></a>Instrucciones Transact-SQL útiles  
 Las instrucciones siguientes devuelven información útil acerca de los permisos.  
  
 Para devolver los permisos explícitos concedidos o denegados en una base de datos ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]), ejecute la instrucción siguiente en la base de datos.  
  
```sql  
SELECT   
    perms.state_desc AS State,   
    permission_name AS [Permission],   
    obj.name AS [on Object],   
    dPrinc.name AS [to User Name]  
FROM sys.database_permissions AS perms  
JOIN sys.database_principals AS dPrinc  
    ON perms.grantee_principal_id = dPrinc.principal_id  
JOIN sys.objects AS obj  
    ON perms.major_id = obj.object_id;  
```  
  
 Para devolver los miembros de los roles de servidor (solo [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]), ejecute la instrucción siguiente.  
  
```sql  
SELECT sRole.name AS [Server Role Name] , sPrinc.name AS [Members]  
FROM sys.server_role_members AS sRo  
JOIN sys.server_principals AS sPrinc  
    ON sRo.member_principal_id = sPrinc.principal_id  
JOIN sys.server_principals AS sRole  
    ON sRo.role_principal_id = sRole.principal_id;  
```  
  
 
 Para devolver los miembros de los roles de base de datos ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]), ejecute la instrucción siguiente en la base de datos.  
  
```sql  
SELECT dRole.name AS [Database Role Name], dPrinc.name AS [Members]  
FROM sys.database_role_members AS dRo  
JOIN sys.database_principals AS dPrinc  
    ON dRo.member_principal_id = dPrinc.principal_id  
JOIN sys.database_principals AS dRole  
    ON dRo.role_principal_id = dRole.principal_id;  
```  
  
## <a name="next-steps"></a>Pasos siguientes  
 Para obtener más temas para comenzar, consulte:  
  
-   [Tutorial: Introducción al motor de base de datos](../../../relational-databases/tutorial-getting-started-with-the-database-engine.md) 

-   [Crear una base de datos &#40;Tutorial&#41;](../../../t-sql/lesson-1-creating-database-objects.md)  
  
-   [Tutorial: SQL Server Management Studio](../../../ssms/quickstarts/ssms-connect-query-sql-server.md)  
  
-   [Tutorial: Escribir instrucciones Transact-SQL](../../../t-sql/tutorial-writing-transact-sql-statements.md)  
  
## <a name="see-also"></a>Consulte también  
 [Centro de seguridad para el motor de base de datos SQL Server y la base de datos SQL Azure](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Funciones de seguridad &#40;Transact-SQL&#41;](../../../t-sql/functions/security-functions-transact-sql.md)   
 [Funciones y vistas de administración dinámica relacionadas con la seguridad &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Determinar los permisos efectivos del motor de base de datos](../../../relational-databases/security/authentication-access/determining-effective-database-engine-permissions.md)
