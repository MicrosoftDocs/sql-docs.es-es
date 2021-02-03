---
title: Creación de un rol de aplicación | Microsoft Docs
description: Cree un rol de aplicación en SQL Server con SQL Server Management Studio o Transact-SQL para restringir el acceso a una base de datos excepto mediante una aplicación.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.approle.general.f1
helpviewer_keywords:
- application roles [SQL Server], creating
ms.assetid: 6b8da1f5-3d8e-4f88-b111-b915788b06f1
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9d77743864b3b5425379ec7b5c570629b470e04
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250760"
---
# <a name="create-an-application-role"></a>Crear un rol de aplicación
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo crear un rol de aplicación en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Los roles de aplicación limitan el acceso de los usuarios a una base de datos excepto a través de aplicaciones específicas. Los roles de aplicación no tienen usuarios, de modo que no aparece la lista **Miembros del rol** cuando se selecciona **Rol de aplicación** .  
  
> [!IMPORTANT]  
>  Al establecer las contraseñas de los roles de aplicación se comprueba la complejidad de la contraseña. Las aplicaciones que invocan roles de aplicación deben almacenar sus propias contraseñas. Las contraseñas de roles de aplicación deben almacenarse cifradas.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para crear un rol de aplicación, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER ANY APPLICATION ROLE en la base de datos.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
##### <a name="to-create-an-application-role"></a>Para crear un rol de aplicación  
  
1.  En el Explorador de objetos, expanda la base de datos donde desea crear un rol de aplicación.  
  
2.  Expanda la carpeta **Seguridad** .  
  
3.  Expanda la carpeta **Roles** .  
  
4.  Haga clic con el botón derecho en la carpeta **Roles de aplicación** y seleccione **Nuevo rol de aplicación...** .  
  
5.  En el cuadro de diálogo **Rol de aplicación - Nuevo**, en la página **General**, escriba el nuevo nombre del nuevo rol de aplicación en el cuadro **Nombre de rol**.  
  
6.  En el cuadro **Esquema predeterminado** , determine el esquema al que pertenecerán los objetos creados por este rol especificando los nombres de objeto. Como alternativa, haga clic en los puntos suspensivos **(...)** para abrir el cuadro de diálogo **Buscar esquema**.  
  
7.  En el cuadro **Contraseña** , escriba una contraseña para el nuevo rol. Vuelva a escribir esa contraseña en el cuadro **Confirmar contraseña** .  
  
8.  En **Esquemas propiedad de este rol**, seleccione o vea los esquemas que pertenecerán a este rol. Un esquema solo puede ser propiedad de otro esquema o de un rol.  
  
9. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

### <a name="additional-options"></a>Opciones adicionales  
 En el cuadro de diálogo **rol de aplicación - Nuevo** también se ofrecen opciones en dos páginas adicionales: **Elementos protegibles** y **Propiedades extendidas**.  
  
-   La página **Elementos protegibles** muestra todos los elementos protegibles posibles y los permisos en esos elementos protegibles que se pueden conceder al inicio de sesión.  
  
-   La página **Propiedades extendidas** permite agregar propiedades personalizadas a los usuarios de base de datos.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-an-application-role"></a>Para crear un rol de aplicación  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- Creates an application role called "weekly_receipts" that has the password "987Gbv876sPYY5m23" and "Sales" as its default schema.  
  
    CREATE APPLICATION ROLE weekly_receipts   
        WITH PASSWORD = '987G^bv876sPY)Y5m23'   
        , DEFAULT_SCHEMA = Sales;  
    GO  
    ```  
  
 Para obtener más información, vea [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-application-role-transact-sql.md).  
  
  
