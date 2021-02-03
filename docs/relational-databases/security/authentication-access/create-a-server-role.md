---
title: Creación de un rol de servidor | Microsoft Docs
description: Cree un rol del servidor en SQL Server mediante SQL Server Management Studio o Transact-SQL. Revise las limitaciones, las restricciones y los permisos necesarios.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.SERVERROLE.GENERAL.F1
- sql13.swb.serverrole.memberships.f1
- sql13.swb.serverrole.members.f1
helpviewer_keywords:
- SERVER ROLE, creating
ms.assetid: 74f19992-8082-4ed7-92a1-04fe676ee82d
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 687d5259c3181d7f82cb16dd35bf019ab1631b1a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250772"
---
# <a name="create-a-server-role"></a>Crear un rol de servidor
[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md](../../../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]
  En este tema se describe cómo crear un nuevo rol de servidor en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear un nuevo rol de servidor, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 No se puede conceder permisos a los roles de servidor en elementos protegibles de nivel de base de datos. Para crear roles de base de datos, vea [CREATE ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-role-transact-sql.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
  
-   Se debe disponer del permiso CREATE SERVER CONTROL o pertenecer al rol fijo de servidor sysadmin.  
  
-   También se requiere el permiso IMPERSONATE en *server_principal* para los inicios de sesión, el permiso ALTER para los roles de servidor que se han usado como *server_principal* o la pertenencia a un grupo de Windows que se ha usado como server_principal.  
  
-   Cuando se utiliza la opción AUTHORIZATION para asignar la propiedad de roles de servidor, también se requieren los siguientes permisos:  
  
    -   La asignación de la propiedad de un rol de servidor a otro inicio de sesión requiere el permiso IMPERSONATE para ese inicio de sesión.  
  
    -   La asignación de la propiedad de un rol de servidor a otro rol de servidor requiere la pertenencia al rol de servidor receptor o el permiso ALTER para ese rol de servidor.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-new-server-role"></a>Para crear un nuevo rol de servidor  
  
1.  En el Explorador de objetos, expanda el servidor donde desea crear el nuevo rol de servidor.  
  
2.  Expanda la carpeta **Seguridad** .  
  
3.  Haga clic con el botón derecho en la carpeta **Roles del servidor** y seleccione **Nuevo rol de servidor...** .  
  
4.  En el cuadro de diálogo **Nuevo rol de servidor -** _nombre\_rol\_servidor_, en la página **General**, escriba un nombre para el nuevo rol de servidor en el cuadro **Nombre del rol de servidor**.  
  
5.  En el cuadro **Propietario** , escriba el nombre de la entidad de seguridad del servidor que poseerá el nuevo rol. Como alternativa, haga clic en los puntos suspensivos **(...)** para abrir el cuadro de diálogo **Seleccionar inicio de sesión o rol de servidor**.  
  
6.  En **Elementos protegibles**, seleccione uno o más elementos protegibles de nivel de servidor. Cuando se selecciona un elemento protegible, se pueden permitir o denegar permisos a este rol de servidor en dicho elemento protegible.  
  
7.  En el cuadro **Permisos: Explícitos**, active la casilla para conceder, conceder por concesión o denegar el permiso a este rol de servidor para los elementos protegibles seleccionados. Si un permiso no se puede conceder o denegar a todos los elementos protegibles seleccionados, el permiso se representará como una selección parcial.  
  
8.  En la página **Miembros** , utilice el botón **Agregar** para agregar inicio de sesión que representan individuos o grupos al nuevo rol de servidor.  
  
9. Un rol de servidor definido por el usuario puede ser miembro de otro rol de servidor. En la página **Pertenencias** , active una casilla para convertir el rol de servidor actual definido por el usuario en miembro de un rol de servidor seleccionado.  
  
10. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-new-server-role"></a>Para crear un nuevo rol de servidor  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    --Creates the server role auditors that is owned the securityadmin fixed server role.  
    USE master;  
    CREATE SERVER ROLE auditors AUTHORIZATION securityadmin;  
    GO  
    ```  
  
 Para obtener más información, vea [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-role-transact-sql.md).  
  
  
