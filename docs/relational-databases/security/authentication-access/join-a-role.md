---
title: Combinación de un rol | Microsoft Docs
description: Obtenga información sobre cómo asignar roles a inicios de sesión y a usuarios de bases de datos en SQL Server mediante SQL Server Management Studio o Transact-SQL. Use roles para administrar permisos.
ms.custom: ''
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.DATABASEUSER.MEMBERSHIP.F1
helpviewer_keywords:
- adding a member to a role
- join a role
ms.assetid: 05c8d10d-5823-46c6-8b1a-81722da6a42b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 88d884d92685b943649006caa894d78a152c7667
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250758"
---
# <a name="join-a-role"></a>combinar un rol
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  En este tema se describe cómo asignar roles a inicios de sesión y a usuarios de base de datos en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Use los roles de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para administrar eficazmente los permisos. Asigne permisos a roles y, a continuación, agregue o quite usuarios e inicios de sesión a los roles. Mediante el uso de roles, los permisos no se tienen que mantener individualmente para cada usuario.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] admite cuatro tipos de roles.  
  
-   Roles fijos de servidor  
  
-   Roles de servidor definidos por el usuario  
  
-   Roles fijos de base de datos  
  
-   Roles de base de datos definidos por el usuario  
  
 Los roles fijos están disponibles automáticamente en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Estos roles disponen de los permisos necesarios para realizar las tareas habituales. Para obtener más información sobre los roles fijos, vea los siguientes vínculos: El usuario crea los roles definidos por el usuario y se pueden personalizar con los permisos que se seleccionen. Para obtener más información sobre los roles definidos por el usuario, vea los siguientes vínculos:  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para asignar roles a inicios de sesión y a usuarios de base de datos, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El cambio de nombre de un rol de base de datos no modifica el número de identificación, el propietario, ni los permisos del rol.  
  
-   Los roles de base de datos se pueden ver en las vistas de catálogos sys.database_role_members y sys.database_principals.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso **ALTER ANY ROLE** en la base de datos, el permiso **ALTER** en el rol o la pertenencia a **db_securityadmin**.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-add-a-member-to-a-fixed-server-role"></a>Para agregar un miembro a un rol fijo de servidor  
  
1.  En el Explorador de objetos, expanda el servidor en el que desea modificar un rol fijo de servidor.  
  
2.  Expanda la carpeta **Seguridad** .  
  
3.  Expanda la carpeta **Roles de servidor** .  
  
4.  Haga clic con el botón derecho en el rol que quiere editar y seleccione **Propiedades**.  
  
5.  En el cuadro de diálogo **Propiedades del rol de servidor -** _nombre\_rol\_servidor_, en la página **Miembros**, haga clic en **Agregar**.  
  
6.  En el cuadro de diálogo **Seleccionar inicio de sesión o rol de servidor** , en **Escribir los nombres de objeto para seleccionar (ejemplos)** , especifique el inicio de sesión o el rol de servidor que quiere agregar a este rol de servidor. O bien, haga clic en **Examinar...** y seleccione cualquier objeto disponible en el cuadro de diálogo **Buscar objetos** o todos ellos. Haga clic en **Aceptar** para volver al cuadro de diálogo **Propiedades del rol de servidor -** _nombre\_rol\_servidor_.  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-add-a-member-to-a-user-defined-database-role"></a>Para agregar un miembro a un rol de base de datos definido por el usuario  
  
1.  En el Explorador de objetos, expanda el servidor en el que desea editar un rol de base de datos definido por el usuario.  
  
2.  Expanda la carpeta **Bases de datos** .  
  
3.  Expanda la base de datos en la que desea editar un rol de base de datos definido por el usuario.  
  
4.  Expanda la carpeta **Seguridad** .  
  
5.  Expanda la carpeta **Roles** .  
  
6.  Expanda la carpeta **Roles de servidor** .  
  
7.  Haga clic con el botón derecho en el rol que quiere editar y seleccione **Propiedades**.  
  
8.  En el cuadro de diálogo **Propiedades del rol de base de datos -** _nombre\_rol\_base de datos_, en la página **General**, haga clic en **Agregar**.  
  
9. En el cuadro de diálogo **Seleccionar usuario o rol de base de datos** , en **Escribir los nombres de objeto para seleccionar (ejemplos)** , especifique el inicio de sesión o el rol de la base de datos que quiere agregar a este rol de base de datos. O bien, haga clic en **Examinar...** y seleccione cualquier objeto disponible en el cuadro de diálogo **Buscar objetos** o todos ellos. Haga clic en **Aceptar** para volver al cuadro de diálogo **Propiedades del rol de base de datos -** _nombre\_rol\_base de datos_.  
  
10. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-add-a-member-to-a-fixed-server-role"></a>Para agregar un miembro a un rol fijo de servidor  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    ALTER SERVER ROLE diskadmin ADD MEMBER [Domain\Juan] ;  
    GO  
    ```  
  
 Para obtener más información, vea [ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md).  
  
#### <a name="to-add-a-member-to-a-user-defined-database-role"></a>Para agregar un miembro a un rol de base de datos definido por el usuario  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    ALTER ROLE Marketing ADD MEMBER [Domain\Juan] ;  
    GO  
    ```  
  
 Para obtener más información, vea [sp_addrolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [Roles de nivel de servidor](../../../relational-databases/security/authentication-access/server-level-roles.md)   
 [Roles de nivel de base de datos](../../../relational-databases/security/authentication-access/database-level-roles.md)   
 [Roles de aplicación](../../../relational-databases/security/authentication-access/application-roles.md)  
  
  
